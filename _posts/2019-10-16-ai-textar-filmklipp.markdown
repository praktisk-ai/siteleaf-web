---
title: AI textar filmklipp
date: 2019-10-16 10:21:00 +02:00
---

Min senaste upptäckt bland bolag som använder AI heter screen9. De har en platform för video: redigering, managering, etc. De verkar även ha haft möjlighet att manuellt texta filmer som hanteras av plattformen tidigare, men nu har de automatiserat detta mha AI. De räknar med att man på detta sätt inte längre behöver lägga 8 timmar redigering per timme film, utan bara 1 timme! 

Detta är ett klockrent exempel på hur man kan förenkla mänskligt arbete mha AI. När algoritmerna blivit tillräckligt bra för att vara användbara så kommer deras användning att öka exponentiellt. Nu är visserligen inte algoritmerna perfekta. Hur bra textningen blir beror mycket på hur tydligt man hör rösten, bakgrundsbrus, intonation, etc. Så det blir alltid en del efterbearbetning, men det funkar alltså i alla fall tillräckligt bra för att vara användbart.

Det här är inte bara en ett klockrent användningsfall, även tekniskt är det implementerat på ett sätt som vi på praktisk.ai förespråkar: ett enkelt API-anrop. I detta fallet har de valt att använda sig av Googles APIer for transkribering då de har bäst stöd för svenska. Men om det skulle visa sig att AWS släpper en mycket bättre funktion imorgon, så kan de så klart med mycket lite arbete enkelt byta leverantör. De anropar helt enkelt bara det nya bättre API:et istället. 
I framtiden ser de även flera andra funktioner de skulle vilja nyttja, tex för översättning och realtidstextning - även dessa är bara ett API-anrop bort. 

Det är så här man bygger in AI i sina system på ett snabbt och flexibelt sätt. Bra jobbat screen9!

