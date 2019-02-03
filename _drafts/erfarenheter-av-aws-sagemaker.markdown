---
title: Erfarenheter av AWS SageMaker
date: 2019-02-03 20:05:00 +01:00
image: "/uploads/awssagemaker.png"
---

Just nu jobbar jag med Örebro Universitet där jag hjälper några forskare att flytta in ett projekt i AWS SageMaker.

[https://aws.amazon.com/sagemaker](https://aws.amazon.com/sagemaker)

Projektet i sig handlar om detektering och klassificering av objekt i bilder vilket är ett spännande område i sig, men här och nu tänkte jag skriva lite om erfarenheterna av att flytta upp till molnet i allmänhet, och till AWS SageMaker i synnerhet.

Projektet började långt innan jag blev inblandad och då kördes alla beräkningar på lokala datorer under någons skrivbord. Perfekt för ett nytt projekt. Man testar lite, ser vad som funkar och i vilken riktning man ska fortsätta. I nästa steg köptes en monsterdator in till projektet som även den hamnade under ett skrivbord. Den körde beräkningar under nätter och över helger. I takt med att man började testa större och komplexare modeller tog iterationerna längre och längre tid, ibland flera veckor. 

Nu, om inte tidigare, börjar det bli dags att leta nya jaktmarker, dvs CPU- och GPU-kraft. Jag pratar om molnet. Det är där vi vill vara, speciellt så fort det handlar om mycket data eller mycket processorkraft. Forskarna insåg detta och det var här jag hade nöjet att komma in i projektet för att bidra lite. 

**Alternativ**
Nu hade de redan börjat kolla lite smått på både AWS och SageMaker och var inställda på det, så det blev den vägen vi valde. Men SageMaker är inte så mystiskt som det låter - grunden är en Jupyter notebook. Visserligen har SageMaker en hel del smakliga kringtjänster kopplade till denna notebook (som jag återkommer till), men det finns andra alternativ också. Exempelvis Google har ett helt gratis alternativ som heter Colab (https://colab.research.google.com) och sen har vi ju alltid Kaggle (https://www.kaggle.com) om man vill träna på dataset som andra redan försökt knäcka. Eller så laddar man bara ner Jupyter och kör på valfri VM.

**Fördelar med SageMaker**
Men tillbaka till SageMaker. När du kör på SageMaker så kan du välja att köra dina vanliga ramverk och algoritmer direkt i notebooken som vanligt (dvs på din notebook ec2-instans), men du kan också välja att starta upp en helt annan maskin att köra träningen på. Detta sker i form av en Docker-container. AWS tillhandahåller ett antal färdiga Docker-images med optimerade versioner av vanliga AI/ML-algoritmer som man enkelt kan använda. Men man har även möjligheten att göra sina egna Docker-images om man till exempel behöver en proprietär algoritm.

![awssm.png](/uploads/awssm.png)

En driftsättning funkar på ungefär samma sätt - man väljer en Docker-image (från AWS eller sin egen) och startar upp den och får direkt en API-endpoint. Sånt underlättar, och man har nära till S3 om man nu skulle ha sparat sin träningsdata, eller tränade modell, där.

**Algoritmen**
Eftersom projektet handlar om objektdetektering så är det alltså algoritmer som identifierar ett eller flera objekt i en bild. Här är ett exempel på ansiktsdetektering.

![faces.png](/uploads/faces.png)


Algoritmer som man då använder är till exempel YOLO, VGG, Resnet, … Det kommer nya och bättre algoritmer varje år som ett resultat av tävlingen: ILSVRC - ImageNet Large Scale Visual Recognition Challenge (https://en.wikipedia.org/wiki/ImageNet#ImageNet_Challenge). SageMaker ligger lite efter men ibland finns de senaste algoritmerna på AWS Marketplace. I vårt fall med objektdetektering så finns till exempel Inception v3 tillgängligt (https://aws.amazon.com/marketplace/pp/B07KCPVRJ8?qid=1549219794881&sr=0-1&ref_=srh_res_product_title).

I bilden nedan visas mAP vs inference speed, dvs precision vs detekteringshastighet. Helst ska en algoritm ligga i övre vänstra hörnet, dvs snabb och med hög precision. AWS tillhandahåller som standard VGG-16 och Resnet-50 i Docker-images för träning. 

![imalgos.png](/uploads/imalgos.png)

**Små steg**
För att komma igång så letade jag upp en objektdetekterings-notebook från AWS som gjorde ungefär det vi ville göra och fick den att exekvera från början till slut. Det finns otroligt många färdiga notebooks för SageMaker vilket är en bra utgångspunkt för nästan alla projekt.

Nästa steg blev att ta vår aktuella data, ett enda träningsexempel, och få det att funka genom att massera träningsdatat och anpassa notebooken. Kan man inte få igenom det enklaste fallet så får man troligen läsa på lite mer! Slutligen driftsatte jag även en API-endpoint som jag kunde anropa för att göra en inferens. Så nu hade jag alltså bevisat att hela kedjan funkade med mitt eget träningsdata på enklast möjliga sätt. Så nu var det bara att iterera på!

Det var det dags att skala upp träningen till nästa nivå - 1000 träningsexempel. Det tog så klart längre tid att träna, men om jag skulle vilja köra träningen på en större maskin så hade det bara varit att skriva in namnet på en större ec2-instanstyp i konfigurationen av mitt ML-objekt (“Estimator” på SageMaker-språk). Nu började jag även kolla på metrics i Cloudwatch. Dessa samlas in automatiskt och man grafar dem enkelt. Som standard får man mAP och hur många procent av träningen som klarats av. Det finns även konceptet “early stopping” som man kan konfigurera att avbryta träningen om precisionen inte förbättras under ett antal träningsrundor.

**Några praktiska tips**
När man börjar utforska sitt dataset så gör man troligen allt i samma notebook. Ju fler iterationer man gjort desto större värde finns det dock i att dela upp sitt arbete i en notebook varje delfas av projektets, exempelvis dessa:

* Datapreparering
* Träning
* Inferens

Genom att renodla koden för varje fas blir varje notebook enklare, kortare och mer fokuserad. Det underlättar ju också för flera att jobba i projektet samtidigt. Dessutom finns det stöd att jobba direkt mot Git i din notebook om du kör JupyterLab istället för den vanliga “vanlij-Jupyter” i SageMaker.


**Problem och utmaningar**
Det första som brukar vara lite svårt att greppa är SageMakers upplägg vad gäller instanser. Det finns inom SageMaker följande:

* Notebook - här körs din notebook. Detta kan vara allt du behöver för ett litet projekt.
* Training - om du vill spinna upp en monstermaskin att köra träningen på helt separat från din notebook. Denna instans stängs ner när träningen är klar.
* Inference - när du via SageMaker vill driftsätta en tränad modell och få en API-endpoint så startas en separat instans upp, med sin egen separata livscykel.

Dessa instanser har alltså inget med dina egenstartade ec2-or att göra, även om de utgår från samma (ungefär...)  AMI:er så manageras dessa helt av SageMaker.

Vad gäller dataformat så använder sig SageMaker av sitt egen JSON-format (inte superväldokumenterat…) eller det lite mer etablerade RecordIO. JSON-formatet är enkelt att komma igång med, speciellt om man skulle vilja handkoda det första exemplet för att verifiera en AWS-notebook. RecordIO passar dock bättre när du kommit igång och har mycket data. Men det finns så klart även en del semantiska skillnader mellan system, tex var man placerar origo på en bild. Det verkar vara en grej att alla ramverk ska hitta ett helt nytt ställe för detta… 

Ska man bygga sin AI lokalt och sen flytta upp till molnet? Personligen vill jag ha så lite som möjligt på min dator. Jag börjar hellre på tex Kaggle eller Colab för att testa saker, och sen när man vill köra något “på riktigt” så börjar man fundera på “var finns mitt dataset?”, “hur mycket CPU kommer jag att behöva?”, “vill jag nyttja någon av till exempel SageMakers finesser som enkelt deploy?”, etc. 

**Slutligen**
Det finns alltså en hel del fördelar med SageMaker. De tillhandahåller optimerade versioner av standardalgoritmerna, man kan enkelt träna på stora maskiner, och driftsätta en modell kan vara i princip en kodrad. Att man kan skapa egna Docker-images är ett plus. På AWS Marketplace kan man hitta mycket kul och slipper uppfinna hjulet igen. Git integrationen är också trevlig. Men det är inte gratis att köra på SageMaker - kanske inga jättesummor så länge man kör på en liten notebook-instans men när man drar igång ett GPU-monster och tränar en vecka så sticker det lätt iväg. Jojo, smakar det så kostar det. Ut och träna dina modeller nu!

/Tobbe