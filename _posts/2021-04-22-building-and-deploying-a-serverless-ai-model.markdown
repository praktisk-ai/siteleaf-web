---
title: Building and deploying a serverless AI model
date: 2021-04-22 14:28:00 +02:00
---

Let me take you on a little journey. An adventure. We will create a small AI model from scratch and then deploy it serverless in the cloud. This article will be slightly technical but the amount of code is quite small.I will show you all the steps required to go from an EDA (Exploratory Data Analysis)  on Google Colab to serving a deployed AI model with AWS Lambda and Google Cloud Functions. Of course there are many other options for deploying your models, but this barebones solution shows you what is actually happening and that there is nothing magic going on. As you will see, it is quite simple! Let us begin our journey!


# The end result

![ai-sls-simple (1).jpg](/uploads/ai-sls-simple%20(1).jpg)


The image above shows the final result we will achieve and also the actions to get there. When the model has been developed, we will download it to our local computer and then upload it to a storage solution provided by the cloud provider. AFter deploying some simple infrastructure we will have created and exposed a simple API. We will then use curl to invoke our model and run the inference. Since we only use HTTP GET requests you could also use your browser to look at the results.


# Creating the model

Google Colab is an excellent free service useful for all sorts of AI things, but particularly loading a dataset and creating a quick model. So this is what we will do here. We load a housing dataset and predict the sales price based on a single parameter (the ground floor space) because it will be easier to visualize and because the main point here is the bigger picture.

`
import pandas as pd
df = pd.read_csv('http://jse.amstat.org/v19n3/decock/AmesHousing.txt', sep='\t')
df.plot.scatter(x='Gr Liv Area', y='SalePrice', figsize=(20,5))
`



Now we create a simple linear regression based on this dataset and draw the regression line through our data.

`
import matplotlib.pyplot as plt
from sklearn.linear_model import LinearRegression
 
X = df[['Gr Liv Area']]
y = df['SalePrice']
model = LinearRegression().fit(X, y)
 
plt.figure(figsize=(20,5))
plt.scatter(X, y,color='g', alpha=0.3)
plt.plot(X, model.predict(X),color='k')
plt.show()
`

Image house-2


We use our new model to predict a value

`
model.predict([[2000]])

236677.63608036
`

This looks reasonable if we also look at this in the images above. Time to save the model to a file. For this we use pickle.

`
import pickle
pickle.dump(model, open('model.pkl', 'wb'))
`

We can now download the created file from Colab to our local computer. Later on we will upload this model to the cloud providers, but for now we will leave it.


# Deploying the model on AWS
Our first deployment will be AWS so you will obviously need to sign up for an account. You also need the CLI tool “awscli” and some credentials. But this you can read about in the AWS documentation. 

We will use S3 to store our model so we need to create a bucket and then upload our model.

`
aws s3 mb s3://lab-ai-sls-model
aws s3 cp ~/Downloads/model.pkl s3://lab-ai-sls-model
`

# Serving the model from AWS
Now the time has come to deploy the serving infrastructure. For this step we will use the extremely competent Serverless Framework (sls).

First we need some simple Python code to load the model, read the incoming parameter (the floor space) and then run the inference.

`
import json
import boto3
import pickle
from sklearn.linear_model import LinearRegression

def run_inference(event, context):
   area = int(event['queryStringParameters']['a'])

   s3 = boto3.resource('s3')
   data = s3.Bucket("lab-ai-sls-model").Object("model.pkl").get()['Body'].read()
   model = pickle.loads(data)

   predicted_price = model.predict([[area]])[0]
   print(predicted_price)

   body = {
       "area": area,
       "predicted_price": predicted_price,
   }

   response = {
       "statusCode": 200,
       "body": json.dumps(body)
   }

   return response

Serverless Framework also needs a configuration file.

service: ai-sls-py-simple

provider:
 name: aws
 runtime: python3.7
 region: eu-west-1

 iam:
   role:
     statements:
     - Effect: "Allow"
       Action:
         - "s3:Get*"
       Resource: "arn:aws:s3:::lab-ai-sls-model/*"

functions:
 run_inference:
   handler: handler.run_inference
   events:
     - httpApi:
         path: /run
         method: get

plugins:
 - serverless-python-requirements

custom:
 pythonRequirements:
   dockerizePip: non-linux
`


We also need to specify our dependencies in a file called requirements.txt:

`
pickle5
scikit-learn==0.22.2.post1
`

Now all we need to do is deploy it.

`
sls deploy

…
endpoints:
  GET - https://aaa.execute-api.eu-west-1.amazonaws.com/run
...
`


That’s it! Really! Now let us use this model by visiting the URL that sls displayed .

`
curl  "https://4h5hatxm9h.execute-api.eu-west-1.amazonaws.com/run?a=2000"

{
  "area": 2000,
  "predicted_price": 236677.63608036027
}
`

Looks like it is the same value as we got in Colab, so we know the model loaded correctly and the inference seems to be working!



# Deploying the model on GCP
This time we will do the same thing but on the Google Cloud Platform (GCP). Here too you will need to create an account, install the CLI tool (gcloud and gsutil) and get some CLI credentials.

In GCP we will use Cloud Storage to store our model. We create a bucket and upload the model.


gsutil mb gs://tobbe-ml-lab 
gsutil cp ~/Downloads/model.pkl gs://tobbe-ml-lab 
 

# Serving the model from GCP
Creating the infrastructure on GCP is even simpler than on AWS for these kinds of labs. We need the Python code for the handler and you can see it is almost identical.

`
import json
import pickle
from sklearn.linear_model import LinearRegression
from google.cloud import storage
from flask import jsonify

def download_blob(bucket_name, source_blob_name, destination_file_name):
   storage_client = storage.Client()
   bucket = storage_client.get_bucket(bucket_name)
   blob = bucket.blob(source_blob_name)
   blob.download_to_filename(destination_file_name)

def run_inference(request):
   area = int(request.args.get('a'))

   download_blob('tobbe-ml-lab', 'model.pkl', '/tmp/model.pkl')
   model = pickle.load(open('/tmp/model.pkl', 'rb'))

   predicted_price = model.predict([[area]])[0]
   print(predicted_price)

   body = {
       "area": area,
       "predicted_price": predicted_price,
   }

   return jsonify(body)
`

We also need to specify our dependencies in a file called requirements.txt:

`
pickle5
flask
scikit-learn==0.22.2.post1
google-cloud-storage==1.16.1
`


Now we only need to deploy the cloud function.

`
gcloud functions deploy run_inference --runtime python37 --trigger-http --source=py_source \
     --allow-unauthenticated --verbosity=info --region=europe-west1

...
entryPoint: run_inference
httpsTrigger:
  securityLevel: SECURE_OPTIONAL
  url: https://europe-west1-aaa.cloudfunctions.net/run_inference
...
`

… and we are done! Let us call the inference endpoint to see what happens.

`
curl "https://europe-west1-aaa.cloudfunctions.net/run_inference?a=2000"
{"area":2000,"predicted_price":236677.63608036027}
`

Looking good right?!


# Summary

In this article I have shown you the entire AI-prototyping workflow from idea to deployed solution using both AWS and GCP.

Note that deploying on serverless comes with some constraints based on the serverless infrastructure, like the available memory and the running time. So for extremely large models the memory will not be big enough. In this case you will have to look at e.g. containerized solutions or cloud provider specific solutions. Also note that you may face some other problems as well like cold starts and latency while downloading the model. However, for fast and simple prototypes or applications with no latency requirements this will work quite nicely.

So now you have some simple code to expand further and continue to build from if you want to start out small and just test some AI-serverless thingies.

No more excuses for not deploying your cool models! Just do it!


Torbjörn Stavenek



