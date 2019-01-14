---
title: Tankar om träningsdata
date: 2019-01-14 23:13:00 +01:00
image: "/uploads/synthetic.png"
---

I de flesta ML-projekt behövs träningsdata och gärna mycket data. Ibland kan det vara svårt att få fram detta och ibland kan det vara svårt att ens veta vilken data som behövs. 

I ett projekt jag medverkade i hade vi identifierat både problemet och datan vi behövde, fixat åtkomst till databasen efter mycket om och men, fått prata kort med de hårt uppbokade utvecklarna som förstod hur tabellerna hängde ihop, och till slut börjat kunna skriva SQL-frågor mot databasen. Men hur vi än vände och vred på frågorna så var det för alldeles lite data, eller ingen data alls, för en specifik kund.

Utvecklarna som skulle kunna hjälpa oss hade ont om tid. Troligen skulle vi även behöva hämta data från en stor kunds interna databas för att få tillräckligt många transaktioner att träna på. Vi som bara ville komma igång för att snabbt kunna verifiera problemet...

**Idéen**
Då kom vi på att just detta problem skulle vi kunna göra den initiala verifieringen för med hjälp av väldigt enkelt data, till och med syntetisk data! Om vi helt enkelt gjorde vissa antaganden skulle vi kunna generera all träningsdata vi behövde för både de enkla och de lite mer komplicerade scenarierna, för att först därefter (om det var lönt) återuppta arbetet med att få tag på riktigt data.

På detta sätt kunde vi initialt hoppa över en massa jobbiga steg som man normalt sett måste ta sig igenom för att kunna börja jobba. Hela ETL-processen (Extract, Transform, Load) kan i många projekt pågå flera veckor eller månader innan man ens kan börja med det riktiga, mer spännande ML-arbetet.

**Rätt antal träningsexempel**

![histo-bars.png](/uploads/histo-bars.png)

Sagt och gjort, vi skapade grunddata som vi sedan kunde generera transaktionsdata utifrån. Eftersom det var syntetiskt data kunde vi generera exakt hur mycket eller lite data vi behövde och på det sättet även få en uppfattning om hur mycket “riktig” data vi skulle behöva för att uppnå ett visst resultat. Om vi till exempel skulle nöja oss med ett ett kvalitetsmått på 80% kanske det skulle räcka med 1000 exempel, men för att uppnå 90% kanske vi behövde 100.000 exempel. På detta vis kunde vi tidigt bedöma om det ens var teoretiskt möjligt att uppnå det önskvärda kvalitetmåttet baserat på mängden riktigt data som fanns tillgänglig. Om det inte fanns en kund med tillräckligt många transaktioner skulle det troligen inte ens vara lönt att försöka (men kanske inte omöjligt).

**Analys**

![analys.png](/uploads/analys.png)

För själva analysen förutspådde vi hur våra resultat skulle bli om vi visualiserade dem och efter att vi kört träningen kunde vi sedan enkelt, och visuellt, bekräfta eller avvisa dessa gissningar. Det var även informativt att visuellt kunna jämföra resultatet på träningen visuellt baserat på mängden tillgänglig data.

Sammanfattning
För att snabbt komma igång med ditt ML-projekt så fundera över om du kan nyttja syntetiskt data. Du kommer igång snabbt, du får en uppfattning om hur mycket riktig data du egentligen behöver och du kan börja teoretisera kring resultaten. 

Detta behöver som sagt var inte vara den slutliga modellen eller ens ett komplett resultat, men om du snabbt kan verifiera att ditt problem troligen går att lösa så är du redan halvvägs igenom projektet! I princip… 

/Tobbe
