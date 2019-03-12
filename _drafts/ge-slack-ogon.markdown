---
title: Ge Slack ögon
date: 2019-03-12 20:06:00 +01:00
---

I denna artikel tänkte jag diskutera olika nivåer av AI-programmering samt visa hur man enkelt kan göra Slack lite mer intelligent genom att lägga till motsvarigheten till ett par ögon - AI. Jag kommer att gå igenom hur man kan få till en enkel bildanalys med hjälp av GCP Cloud Functions och Googles Vision API på en bild man lagt upp på Slack.

![slack.png](/uploads/slack.png)

**Nivåer**
En sak som jag vill sprida till fler är de olika tillgängliga nivåerna av AI-programmering: API, bibliotek och grundforskning. AI behöver inte innebära att man sysslar med “bleeding edge”-forskning eller vet exakt hur alla algoritmer funkar, utan det kan räcka med ett API-anrop för att få tillgång till ett system som tränats av de vassaste företagen i världen. Jag tycker att man i sin första “spik” av ett system alltid ska börja med den översta nivån - dvs låt AI-implementationen bestå av ett API-anrop. Kanske räcker det för den funktionalitet man vill ha? Eller så kanske det räcker för att få ett system som är tillräckligt bra för att utvärdera och veta om man ska gå vidare eller inte. Dessutom finns det liknande API:er hos olika företag men som funkar lite olika bra på olika saker. Bildanalys via API erbjuds ju av exempelvis både Google, AWS och Azure. Alla större plattformar supportar också HTTP REST mot sina AI-lösningar och den mesta funktionaliteten erbjuds dessutom ofta via en SDK för flera olika språk. Då blir det busenkelt att koppla ihop saker!

En annan stor fördel med att gå via AI-API:er är att alla framtida förbättringar “på andra sidan” kommer din applikation till godo utan något merarbete. Dessutom slipper du ansvaret att drifta denna del, vilket också är trevligt. 

Om man bestämmer sig för att gå vidare på grund av att funktionaliteten via API inte räcker så går man till nästa nivå - bibliotek med färdiga algoritmer. I vanliga företag är detta i princip så djupt man går. Bara i undantagsfall är kraven så specifika att man måste knåpa ihop sitt eget neurala nät. Alltså, man börjar med det enklast möjliga och filar på det tills man inte kommer längre. Först då tar man nästa steg. Det är AI i praktiken! 

Applikationen
I denna lilla applikation så skickar jag in en uppladdad bild till en förtränad bildanalysalgoritm och får tillbaka resultatet utan någon större ansträngning. Allt körs i GCP (Google Cloud Platform) med serverless (Cloud Functions) och AI via API (Google Vision API). 

I min första “spik” tänkte jag skicka upp en bild, låta Vision API hitta all text i bilden och sedan posta tillbaka texten som ett svar på den uppladdade bilden.

Här kommer en översikt på hur saker hänger ihop:

![ai-slack.jpg](/uploads/ai-slack.jpg)

1. En användare laddar upp en bild till Slack
2. Slack skickar ett event via en webhook till min Cloud Function
3. Min funktion hämtar bilden från Slack
4. Bilden skickas till Cloud Vision för analys och returnerar all text som hittats i bilden
5. Min funktion postar tillbaka den hittade texten som ett svar på den uppladdade  bilden
6. Användare får en notis om ett nytt inlägg på Slack

Om man tittar på bilden ovan så är det ganska mycket pilar och streck, mycket integrationer. Men om man försöker hitta “min” AI-del, så består den bara av starten på pil 4 - dvs “AI by integration”! Så i hela denna implementation där jag “gett Slack ögon” så består AI-delen av ett enda API-anrop. Det låter väl inte så svettigt?! 

Nu är det visserligen inte någon stor applikation - men det spelar ingen roll - max 5% av arbetet bestog av själva AI-delen och konfigurationen av den. Resten är vanlig hederlig programmering, konfiguration och googling...

**Slack**
Slack är otroligt enkelt att integrera med. Det finns bland annat utgående anrop till en webhook, till Slack inkommande webhooks, email-till-Slack-kanal, API:er, etc.
Därför  blev Slack mitt GUI för denna applikation. Enkelt att ladda upp en bild från mobilen (nummer 1 i bilden) och enkelt att få till en utgående webhook (nummer 2 i bilden) till min Cloud Function. Att ladda ner bilden däremot var lite omständligt på grund av behörigheter etc. Men till slut så gick det med.

**Google Cloud Functions**
Detta är ju Googles svar på AWS Lambdas. Dock ska man veta att Cloud Functions är lite mer “i början av sin karriär” jämfört med Lambdas, i alla fall om man kör ramverket Serverless som jag gjorde. Exempelvis så stöds bara nodejs6 som GA och nodejs8 i Beta (funkade inte alls för mig). Men det som var mest irriterande var att en deploy tog godtyckligt lång tid från 3-10 minuter. Hela supporten inom GCP runt Cloud Functions är inte heller särskilt välutbyggd jämfört med AWS. Alltså, kul att man kan köra serverless på GCP, men det behöver verkligen byggas ut. Det verkar inte heller som att särskilt många kör via ramverket Serverless av de få sökresultaten att döma.

**Google Vision API**
Anropet till Vision API:et var i princip bara att aktivera API:et, skapa en API-nyckel och göra anropet via en SDK. Busenkelt, ja när man väl hittat en fungerande exempel alltså. Dokumentation verkar ligga lite efter här och ofta så visas bara en liten del av det som behövs för att allt ska fungera. 

**Resultatet**
Så vad har vi nu då - jo, kolla in videon nedan.

[Video](https://youtu.be/n5DlVr-s894)


**Framåt**
När man väl kommit så här långt så är det bara att ösa på med mer funktionalitet. Bara Vision API erbjuder ett flertal olika analyser man kan be den göra. Jag körde här bara “text detection” men det finns en massa andra roliga saker man kan få tillbaka också, som tex ansikten i bilden och deras sinnesstämning, kända landmärken, bildkategorisering, logotypes, etc.
Sen kanske man vill addera ett anrop till AWS Rekognition för att göra samma analys mot deras tjänst och se om man får samma resultat. De är som sagt olika bra på olika uppgifter. Bara att prova!

**Summering**
Nu vet du alltså hur man med enkla medel kan lägga till intelligens till sin applikation. Sätt igång!


Torbjörn Stavenek

