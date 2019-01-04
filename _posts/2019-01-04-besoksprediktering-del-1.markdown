---
title: Besöksprediktering del 1
date: 2019-01-04 14:59:00 Z
---

**Bakgrund**
Sommaren 2017 öppnade jag och min fru ett utomhuslekland ([www.lustigalandet.se](http://www.lustigalandet.se)). Under säsongen visade det sig att om vi hade kunnat förutsäga ungefär hur många besökare som skulle komma varje dag skulle många saker underlättats. Skulle det räcka med de sommarjobbare vi tagit in för dagen? Hur många grillkorvar och kanelbullar skulle vi tina? Skulle det att bli kaos på parkeringen om vi inte hade någon som höll koll där?

Att skapa en modell för dessa prediktioner är ett perfekt exempel på vad man kan göra med maskininlärning: man har någon slags mätvärden, tex webbplatsbesök, som man tror kan förutspå något annat, tex fysiska besökare på ett lekland. Sagt och gjort - jag drog igång projektet!

**Målbild**


Det första jag gjorde var att fundera ut hur jag skulle vilja att det fungerade om det blev perfekt. Eftersom jag har en Amazon Alexa i köket kändes det naturligt att använda den som gränssnitt mot prediktionerna.

*Söndag kväll.
-Alexa, kommer det många besökare imorgon?
-Det ser ut att bli bra väder imorgon och det har varit mer trafik på hemsidan än vanligt på sistone.
Dessutom är det ju ofta mycket folk på måndagar.
Enligt mina beräkningar bör det komma runt 120 barn.
Du bör se till att det finns ca 50 korvar och korvbröd tinade.      Enligt schemat finns Maria tillgänglig vid behov och jag kan skicka ett SMS och förvarna henne om du vill. Ska jag göra det?
-Ja, absolut.
-Ok, jag har skickat ett meddelande till Maria om att vi eventuellt behöver henne imorgon.*

*Måndag morgon.
-Alexa, kommer det mycket folk idag?
-Det ser ut att bli bra väder idag och det har varit mer trafik på hemsidan än vanligt igår och nu på morgonen.
Enligt mina beräkningar bör det komma runt 130 barn.
Du bör se till att det finns ca 55 korvar och korvbröd tinade.
Jag förvarnade ju Maria om att vi kanske skulle behöva henne idag.
Ska jag be henne komma till jobbet direkt nu på morgonen?
-Ja tack!
-Ok, jag har skickat ett meddelande till Maria och bett henne komma hit så fort hon kan.*

Lite prediktioner och underlag till dessa, såsom väder, samt direkt lite förslag på åtgärder. Att exempelvis kunna hantera extra personal direkt via Alexa gör att det gjort i samma ögonblick som man accepterar förslaget om att kontakta någon - ingen risk att det glöms bort eller inte hinns med!

Detta är alltså den utopiska målbilen. Nu gäller det bara att omsätta den i praktiken också!

**Planen**


Som i alla projekt bygger man inte allt direkt, utan en bra start är ofta att försöka göra en så kallad “spik” - ett komplett fungerande system men där man bara gjort minsta möjliga arbetsinsats i varje ingående del.

Hur konversationen med Alexa ska funka efter första “spiken”:

* Hur många besökare kommer det idag?

* Baserat på de senaste 24 timmarnas besök på webbplatsen uppskattar jag att det kommer ca 30 barn idag.

Första delmålet i projektet blev då följande:

* Ladda ner rådata för ML-modelleringen

* Skapa en enkel ML-modell

* Skapa röstgränssnittet för Alexa

* Skapa integrationen mot Google Analytics

Här är en skiss på detta:

**![](https://lh6.googleusercontent.com/yGWc_VRMIUudXEjlDH6Xvb8b5v76sIho_Zi2kxX274Dm_KRfk6qFFVgeRbJxOMMxVchyGD1qUVh4DU8wd79FbgX0-gNrFZqEdDAqIMzyj47kaQhG7TKKi_jX9FRZpjAMM3zYwsHu)**

**Datakällor**


Via iZettle kunde jag få tag på all rå transaktionsdata. På så vis kunde jag med hjälp av lite “datamassage” få fram min målvariabel - antal besökande barn för en specifik dag.

En hypotes för prediktionen var antalet besökare på webbsidan och eftersom vi använder Google Analytics så kunde jag tanka ner denna data relativt enkelt. Nu finns det ju massor med ytterligare parametrar man skulle kunna kolla på, som till exempel antalet sessioner, sessionslängd, återkommande besökare, etc. Men för att hålla det enkelt i denna “spik” så kollade jag alltså endast på det råa antalet besökare på webbsidan.

En annan hypotes var att det kunde hänga ihop med antalet besökare på Facebook-sidan, eller andra aktiviteter som gjort där. Eller en självklarhet som att kolla på vädret - oerhört viktigt för ett besöksmål utomhus. Eller huruvida en viss dag var en helgdag eller semester. Men som sagt, i denna första spik så gällde det att hålla igen lite.

**Kända problem**


När man jobbar med maskininlärning så finns den några grundregler, som exempelvis att man bör ha en hel del data att träna sin modell på. Ofta är mer bättre. En nedre gräns är kanske tusentals eller eventuellt hundratals exempel. För denna modell hade jag data för ca 2-3 månader, och då hade det dessutom varit stängt på måndagar och lite andra blandade dagar… Så nu vi pratar om runt 60 datapunkter - inte ens med lite god vilja jättemycket underlag.

En annan komplicerande faktor var att verksamheten var nyöppnad. Detta innebär, troligen, att besöksströmmarna inte har stabiliserat sig tillräckligt för att vara något särskilt kvalitativt underlag för en ML-modell.

Med dessa ursäkter ur vägen, och med vetskapen om att modellen i alla fall blir bättre för varje säsong så fortsätter vi nu mot det första delmålet.

**ML-modellen**


Så här såg besöksgrafen ut för första säsongen - alltså det som jag försökte skapa en modell för.

**![](https://lh6.googleusercontent.com/OfvNHQPDnejawt79t0vNB1ju764nE7JccwpenhLNiEDQuFYaCTmnLKcTRIQEEcrQwipcJgizv8yGPhP-EfP5fx4u-2ODfztH8cvBApd6nb_Fy4ad8c0qZHq2qcRidglQHkYaOiUa)**

Det ser nästan slumpmässigt ut, men det finns en viss stigande trend från och med öppningsdagen fram till sommaren och industrisemestern, för att därefter falla allt eftersom fler och fler börjar jobba igen och sommarlovet är över. De nedåtgående dipparna är mestadels måndagar då det var stängt.

Min första modell blev enkel linjär regression. Det brukar vara en bra idé att bara få igång nåt och se vad som trillar ut. Då har man ett basvärde att jämföra med när man börjar kolla på mer avancerade modeller. Dessutom var ju tanken att få ihop hela systemet först - och därefter iterera fram en bättre och bättre modell.

Några enkla features som jag lade till var exempelvis veckodagar och veckonummer. Därefter lite “idiotkontroller” för att se att datan hänger ihop. Här visas summerat antal besökare per veckodag.

**![](https://lh6.googleusercontent.com/0hFJsrwmTXZjZpvYN_GeKkWtVBHXOEXjj-xIYfiI94ox3oI6lWiSFER1H8bUlFXI_2FDSA85deCwCbizFEX-8ZG-tgF56UsOPxZjjEe0Xry-AENXZq6EtVCnI5kyIY664801SUV2)**Noll är förstås måndagar. Tisdagar var generellt populärast.

Nedan visas besökare summerat per vecka.

**![](https://lh3.googleusercontent.com/DUKYPwrH3pS6iiwE5GdHWAiXxSCeIS3ceJIYNPyEp-V0HQqyQlW-ndUMmixW0z0svXZsPTc7fGBTd4JdCbmQOnWPdgF3PiAcRnMR84ShKzEgU6sN4_iWmtglZy3cXYiZAsQlp7Mg)**Det ser ut som om semester och sommarlov låg vecka 26 till 32.

Ok, nu åker vi! Med hjälp av en 70-30 train-test-split, linjär regression, datuminformation som enda hjälpmedel för prediktionen, för att sen prediktera på hela datasetet så blev första försöket så här:

**![](https://lh6.googleusercontent.com/7h8FUhbrACKQ0qe2ZOqhol5idp6AyRXii01oKHJqcN90D2d4slNktJ12X8td5DEoxuK7dIoX-HvccUAMxbkCg_JWSs1WoyMA34X3Yy87avHN1tYR1HFb_iSlnROoSZI2V80eOO-o)**Model score (r2): 0.48\
Inte lysande kanske, men för att bara ha haft datumet att utgå ifrån så, tja, på rätt väg.

Om man kollar sambanden mellan några variabler och antal barn (på y-axeln) så ser man att det inte finns någon jättestark korrelation, men ändå lite användbar.

**![](https://lh4.googleusercontent.com/CvGGv9dbkz5dEtybo5L2Ol14X9wd0TCWWaQB7H8Y0NVq1-gpJSiuMG3zxbL5HBDsORwAkiB7WgKN4nrrNxqQHxQobyPPJorSMBxzdMhDKIa31b7qhNd4RYvyH13OKhSzZyz4kHpX)**

På bilden nedan ligger rätt svar på x-axeln och prediktionen på y-axeln. Vid en perfekt prediktion skulle alla prickar legat på linjen. Så det ser lite spretigt ut, men ok för ett första försök.

**![](https://lh4.googleusercontent.com/AOfZtNg-0Yx4GgPzl5vqKqHipXyTX3NOr2l4QgulyDbx-8pYiRMnITud0qwsEOqp_FeBYBnQYUmooKznGRcHmxoDlsvSINDkc5HvWU_MEFCl57hRBSf6q9QXWHUxhglfiTIEqDw6)**Ok, vi lägger till Google Analytics data också för att se om det blir bättre. Så här ser GAs antal sessioner ut jämfört med det faktiska antalet besökande barn:

**![](https://lh5.googleusercontent.com/CUzdhvn7o_KA5o0dpyVR_n0JM91wYHNA3fv01ezXs48LepPgpQe75SEhOBYO4QUZcX4yAA1w-6uC5yyZ7NlI1Sb2h1Z2kLg0U0yXmzdBulVLQOx19ve7oJgxPiI8pbKNA3l4VjW1)**

Så det verkar ju finnas någon slags korrelation. Vi kollar med webbplatsbesök på x-axeln och antal barn på y-axeln:

**![](https://lh3.googleusercontent.com/SQPWSV7RHcaSMUG5kYECDx9io3tsbYQypXl3ZaMDYAn49lrl_v2yGoMjhkzWNuw_u4CMzSPkyul3UNrPR0FzL0WwhoB3FnQMAx3EkoJJuxGcspr5QbMqLBm0MwFPutlOtfLcyCLY)**

Ja, en tydlig korrelation! Det borde hjälpa prediktionen en del!

Efter en ny träningsomgång och prediktion blev det så här:

**![](https://lh3.googleusercontent.com/6tWdzdy76NRRQln9k6tbNusmjbEDXOXLqjGeaXemmjaFAU4FjcWpJfIp6iH-19HC_7ZX2h-zrSYXcRSBsKWjGhpCHg1W1YiEdNn9BpAZoxKzqLhZW8__KPWZXc2e1GXytMvM2BWU)**

**![](https://lh3.googleusercontent.com/naU86JJYHG-4PStXH2T2KonwzHoJ8_XPRLjkkqHM36EpW5MpsBejXWDfSEz3TYSFzjD-awE4-M9QnxhctQsYjs79zkoYc5IEb5H-2c6v-yHRA1gOiRcV2ikvxoNBpW5Yi7gazdkH)**

Model score (r2): 0.56

Diagrammen ser ju ungefär lika ut, men r2 har ökat lite grann.
R2 (r-kvadrat) talar om hur väl modellen matchar datat den beräknats på. Om r2 är 1.0 så har vi en perfekt modell och om r2 är 0.0 (eller negativ) så har vi inte fångat något intressant i modellen.

Slutligen testade jag även att använda bara webbplatsbesöken för prediktionen, och då ramlade R2 ut på 0.58, dvs bäst hittils. Så det verkade som om någon del av datuminformationen bara bidrog med brus till modellen. Nåja, det blev ju bra att kunna börja med något enkelt som jag kunde koda ihop utan att ens behöva använda några externa kodbibliotek.

**Hämta data från GA**


När jag hämtade basedatan för modellträningen hade jag använt ett Python-skript, alltså ville jag köra det igen för att hämta data kontinuerligt. Min plan var att köra detta i en schemalagd AWS Lambda-funktion (serverless) men det var inte helt lätt att få det att funka. Istället blev det plan B, starta en AWS EC2-instans (Linux server) och köra ett cron-jobb som varje timme:
hämtade GA-data
gjorde ML-beräkningen enligt min framtagna modell
Sparade prediktionen till en fil på AWS S3.

**Alexa**


Slutligen återstod bara att skapa röstgränssnittet till Alexa. Detta är lite konfiguration plus en AWS Lambda, i TypeScript denna gång, som helt enkelt bara läser min prediktion från s3 och svarar användaren.

**Slutresultatet**


När man kollar på hur det blev känns det verkligen som toppen på isberget, inte mycket synligt!

VIDEO

**Vidareutveckling**


Detta var alltså första “spiken”. Men det finns mycket mer skoj att titta vidare på och som kommer i nästa del av denna artikelserie.

* Ytterligare data från säsong 2

* Randomforestregressor och andra algoritmer

* Mer info från datum

* Väder

* Semesterdagar

* Mer mätvärden från både Google Analytics och Facebook

* Cross-validation träning, pga lite data

* GridSearchCV för hyperparam tuning

* Data lag, webbplatsbesök senaste dagarna

* Ignorera extremvärden i datat