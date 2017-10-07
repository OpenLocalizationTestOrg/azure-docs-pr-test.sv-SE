---
title: "aaaUse jobbet webbläsare och visa jobb för Azure Data Lake Analytics-jobb | Microsoft Docs"
description: "Lär dig hur toouse jobbet webbläsare och visa jobb för Azure Data Lake Analytics-jobb. "
services: data-lake-analytics
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: bdf27b4d-6f58-4093-ab83-4fa3a99b5650
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/02/2017
ms.author: jgao
ms.openlocfilehash: c45e618426808349ca380b1bcfaefd4c947ce7ab
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-job-browser-and-job-view-for-azure-data-lake-analytics-jobs"></a>Använd jobbet webbläsare och jobbet för Azure Data lake Analytics-jobb
hello Azure Data Lake Analytics-tjänsten Arkiv skickats jobb i en [frågearkivet](#query-store). I den här artikeln får du lära dig hur toouse jobbet webbläsare och visa jobb i Azure Data Lake-verktyg för Visual Studio toofind hello historiska jobbinformation. 

Som standard arkiveras hello Data Lake Analytics-tjänsten hello jobb i 30 dagar. hello förfalloperiod kan konfigureras från hello Azure-portalen genom att konfigurera hello anpassad princip. Du kommer inte att kunna tooaccess hello jobbinformation efter förfallodatumet. 

## <a name="prerequisites"></a>Krav
Se [Data Lake-verktyg för Visual Studio förutsättningar](data-lake-analytics-data-lake-tools-get-started.md#prerequisites).

## <a name="open-hello-job-browser"></a>Öppna hello jobbet webbläsare
Åtkomst hello jobbet webbläsare via **Server Explorer > Azure > Data Lake Analytics > jobb** i Visual Studio.  Med hello jobbet webbläsare kan du komma åt hello frågearkivet för Data Lake Analytics-konto. Jobbet webbläsaren visar Query Store på hello vänster, grundläggande jobbinformation och jobbet visa på hello rätt visar detaljerad information om jobbet.

## <a name="job-view"></a>Visa jobb
Jobbet vy som visar hello detaljerad information om ett jobb. tooopen ett jobb, du kan dubbelklicka på ett jobb i hello jobbet webbläsare eller öppna den hello Data Lake-menyn genom att klicka på jobbet. Du bör se en dialogruta med hello jobbet URL.

![Data Lake-verktyg webbläsare för Visual Studio-projekt](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-view.png)

Jobbet innehåller:

* Jobbsammanfattning
  
    Uppdatera hello jobbet toosee hello senare information av jobb som körs.
  
  * Jobbets Status (diagram):
    
      Jobbstatus beskrivs hello jobbet faser:
    
      ![Azure Data Lake Analytics-jobbstatusen faser](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-phases.png)
    
    * Förbereder: Överför din skriptet toohello moln, kompilering och optimera hello skript med hello kompilera tjänsten.
    * Jobb i kö: Är köade vassle de väntar på att tillräckligt med resurser eller hello jobb överskrider hello högsta antal samtidiga jobb per konto begränsning. hello prioritetsinställningen anger hello sekvens med köade jobb - hello lägre hello nummer hello högre hello prioritet.
    * Körs: hello jobbet faktiskt körs i ditt Data Lake Analytics-konto.
    * Slutför: hello jobbet slutförs (till exempel Slutför hello-fil).
      
      hello jobb kan misslyckas i varje fas. Till exempel kompileringsfel i hello förbereda fas, timeout-fel i hello i kö fasen och körningsfel i hello körs fasen osv.
  * Grundläggande Information
    
      information om grundläggande hello visar hello nedre delen av hello jobbsammanfattning panelen.
    
      ![Azure Data Lake Analytics-jobbstatusen faser](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-info.png)
    
    * Jobbet resultat: Lyckades eller misslyckades. hello-jobb misslyckas i varje fas.
    * Total varaktighet: Väggskåp systemtid (varaktighet) mellan skickar tid och sluttid.
    * Total tid för beräkning: hello summan av alla vertex körningstid, kan du den som hello tid hello jobbet körs i en enda nod. Läs tooTotal formhörnen toofind mer information om hörn.
    * Skicka/Starttid/Sluttid: hello tid när hello Data Lake Analytics-tjänsten tar emot jobbet skicka/startar toorun hello jobbet/avslutar hello jobb eller inte.
    * Körs-kompilering/i kö: Klocktid använt under hello förbereda/i kö/körs fasen.
    * Konto: hello Data Lake Analytics-konto används för att köra hello jobb.
    * Skapad av: hello användare som skickade hello jobb, det kan vara en verklig person konto eller ett system.
    * Prioritet: hello prioriteten för hello projektet. hello lägre hello nummer, hello högre hello prioritet. Den påverkar bara hello sifferföljd hello jobb i kö för hello. Jobb som körs åsidosätter inte när du anger en högre prioritet.
    * Parallellitet: hello begärt maxantalet samtidiga Azure Data Lake Analytics-enheter (ADLAUs), även kallat formhörnen. För närvarande ett hörn är lika tooone VM med två virtuella kärnor och sex GB RAM-minne, även om detta kunde uppgraderas i framtiden Data Lake Analytics-uppdateringar.
    * Byte kvar: Antal byte som behöver toobe behandlas förrän hello jobbet har slutförts.
    * Läs/skrivna byte: byte som har lästs/skrivits sedan hello jobbet har startats.
    * Totalt antal formhörnen: hello jobbet har delats upp i många arbeten, varje arbetsuppgifter kallas en nod. Det här värdet beskriver hur många arbeten hello jobb består av. Du kan överväga att en nod som en grundläggande process-enhet, även kallat Azure Data Lake Analytics enhet (ADLAU), och formhörnen kan köras i parallellitet. 
    * Slutfört/körs/misslyckades: hello antal formhörnen slutförts/körs/misslyckades. Formhörnen kan misslyckas på grund av tooboth användaren kod och system fel, men hello system försök misslyckades formhörnen automatiskt ett fåtal gånger. Om hello vertex fortfarande misslyckas efter att ha försökt hello hela jobbet kommer att misslyckas.
* Jobbdiagram
  
    Ett U-SQL-skript representerar hello logiken för att omvandla inkommande data toooutput data. hello skriptet kompileras och optimerade tooa fysiska åtgärdsplan vid hello förbereda fasen. Jobbdiagram är tooshow hello fysiska åtgärdsplan.  hello följande diagram illustrerar processen för hello:
  
    ![Azure Data Lake Analytics-jobbstatusen faser](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-logical-to-physical-plan.png)
  
    Ett jobb har delats upp i många arbeten. Varje arbetsuppgifter kallas för en nod. hello formhörnen grupperas som Super Vertex (aka delen) och visualiseras som Jobbdiagram. hello grön steget skyltar i hello jobbdiagram visar hello steg.
  
    Varje nod i ett steg gör hello samma typ av fungerar med olika delar av hello samma data. Om du har en fil med en TB data och det finns hundratals formhörnen läsning från den, var och en av dem läser ett segment. Dessa formhörnen grupperas i hello samma steg och gör samma fungerar på olika delar av samma indatafilen.
  
  * <a name="state-information"></a>Steg-information
    
      Tal visas i en viss fas i hello placard.
    
      ![Azure Data Lake Analytics-jobbet diagrammet fas](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-graph-stage.png)
    
    * SV1 Extrahera: hello namnet på ett steg, med namnet med en siffra och hello åtgärden-metod.
    * 84 formhörnen: hello Totalt antal formhörnen i det här steget. hello bild visar hur många arbeten är uppdelad i det här steget.
    * 12.90 s/vertex: hello brytpunkt Genomsnittlig körningstid för det här steget. Denna bild beräknas genom att SUMMAN (varje vertex körningstid) / (totalt antal Vertex). Vilket innebär att om du kan tilldela alla hello formhörnen köras i parallellitet hello hela steget har slutförts i 12.90 s. Det innebär också om alla hello arbete i det här steget utförs i följd, hello kostnaden skulle #vertices * Genomsnittlig tid.
    * 850,895 rader skrivna: Totalt antal rader som har skrivits i det här skedet.
    * R/b: mängden data Läs/skriftliga i det här steget i byte.
    * Färger: Färger som används i hello tooindicate status för olika hörn.
      
      * Grönt anger hello vertex lyckades.
      * Orange anger hello vertex försöks. hello igen vertex gick men försöks automatiskt och har genom hello system och hello övergripande steg har slutförts. Om hello vertex igen men fortfarande misslyckades, aktiverar hello färg röd och hello hela jobb som misslyckades.
      * Rött anger misslyckades, vilket innebär att en viss nod hade varit igen några gånger av hello system men fortfarande misslyckades. Detta medför hello hela jobbet toofail.
      * Blå innebär en viss nod körs.
      * Vit anger hello vertex väntar. hello vertex kanske väntar toobe schemalagda när en ADLAU blir tillgänglig, eller det kanske väntar på indata eftersom dess indata är inte kanske redo.
      
      Du hittar mer information för hello steget hovrar pekaren av ett tillstånd:
      
      ![Azure Data Lake Analytics-Jobbdetaljer diagrammet fas](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-graph-stage-details.png)
  * Formhörnen: Beskriver hello formhörnen information, till exempel hur många formhörnen totalt, hur många formhörnen har slutförts, är de misslyckades eller fortfarande körs/väntar, osv.
  * Läsa data mellan/inom baljor: filer och data lagras i flera skida i distributed file system. hello värde här beskrivs hur mycket data har lästs i hello samma baljor eller mellan baljor.
  * Total tid för beräkning: hello summan för varje vertex körningstid i hello steget kan du den som hello tid det tar om alla fungerar i hello steget utförs i en enda nod.
  * Data och rader skrivs/läses: Anger hur mycket data rader har lästs/skrivits eller toobe behöver läsa.
  * Vertex läsa fel: Beskriver hur många formhörnen misslyckades vid läsning av data.
  * Ignorerar dubblett Vertex: om en nod är för långsam, hello system kan schemalägga flera formhörnen toorun hello samma mängd arbete. Reductant formhörnen ignoreras när något av hello formhörnen slutföras. Dubbla Vertex ignorerar poster hello antalet formhörnen ignoreras som upprepningar i hello steget.
  * Återkallelse av Vertex: hello vertex lyckades, men hämta senare kör igen på grund av toosome skäl. Till exempel om underordnade vertex förlorar mellanliggande indata, uppmanas hello överordnade vertex toorerun.
  * Vertex schema körningar: hello total tid hello formhörnen har schemalagts.
  * Max-min/genomsnittlig Vertex data läses: hello medelvärde/gränsvärden för varje hörn läsa data.
  * Varaktighet: hello klocktid ett steg tar måste tooload profil toosee det här värdet.
  * Jobbuppspelning
    
      Data Lake Analytics kör jobb och Arkiv hello formhörnen kör information för hello jobb, t.ex när hello formhörnen startas, stoppas, misslyckades och hur de är på nytt, osv. All information som hello är automatiskt inloggad hello frågearkivet och lagras i dess jobbet profil. Du kan hämta hello jobbet profil via ”Beläggningsprofil” i jobbet och du kan visa hello Jobbuppspelning när du har hämtat hello jobbet profil.
    
      Jobbuppspelning är en epitome visualisering av vad som hände i hello klustret. Det hjälper dig se förloppet för jobbkörningen och identifiera visuella prestandaavvikelser och flaskhalsar i mycket kort tid (mindre än vanligtvis 30s).
  * Jobbet termiska karta visning 
    
      Du kan välja jobbet termisk karta via hello visas listrutan Jobbdiagram. 
    
      ![Azure Data Lake Analytics-jobbet diagrammet heap kartbilden](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-graph-heat-map-display.png)
    
      Den visar hello i/o, tid och genomströmning termisk karta för ett jobb som du hittar där hello jobbet tillbringar den mesta hello tid, eller om ditt jobb ett jobb för i/o-gräns och så vidare.
    
      ![Azure Data Lake Analytics-jobbet diagrammet heap kartan exempel](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-graph-heat-map-example.png)
    
    * Förlopp: Hej förloppet för jobbkörningen, se Information i [mellanlagra information](#stage-information).
    * Läsa/skriva data: hello termisk karta av totala data lästa/skrivna i varje steg.
    * Beräkna tid: hello termisk karta för SUM (varje vertex körningstid), kan du detta som hur lång tid det tar om allt arbete i hello steget körs med endast 1 hörn.
    * Genomsnittlig körningstid per nod: hello termisk karta för SUM (varje vertex körningstid) / (Vertex nummer). Vilket innebär att om du kan tilldela alla hello formhörnen köras i parallellitet hello hela steg ska utföras i denna tidsperiod.
    * I/o-genomströmning: hello termisk karta av i/o-genomflöde i varje fas, du kan bekräfta om jobbet är ett i/o-bundna jobb via.
* Åtgärder för metadata
  
    Du kan utföra vissa åtgärder för metadata i U-SQL-skript, exempelvis skapa en databas, ta bort en tabell, osv. Dessa åtgärder visas i Metadata-åtgärd efter kompilering. Du kan hitta intyg, skapa entiteter, släpp entiteter här.
  
    ![Azure Data Lake Analytics-jobbet visa metadataåtgärder](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-view-metadata-operations.png)
* Tillståndshistorik
  
    Hej Tillståndshistorik också visualiseras i jobbsammanfattning, men du kan få mer information här. Du kan hitta hello detaljerad information, till exempel när hello jobbet förbereds i kö, startats, avslutades. Du kan söka efter hur många gånger hello jobbet har kompilerats (hello CcsAttempts: 1), när är hello jobbet skickade toohello klustret (hello detalj: sändning av jobbet toocluster) osv.
  
    ![Azure Data Lake Analytics-jobbet visa tillståndshistorik](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-view-state-history.png)
* Diagnostik
  
    hello verktyget diagnostiserar jobbkörningen automatiskt. Du får aviseringar när det inte finns några fel eller prestandaproblem i dina jobb. Observera att du behöver toodownload profil tooget fullständig information här. 
  
    ![Azure Data Lake Analytics-jobbet visa diagnostik](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-view-diagnostics.png)
  
  * Varningar: En avisering visas här med kompileringsvarning. Du kan klicka på ”x eller flera” länken toohave mer information när hello-meddelande.
  * Vertex kör för lång: om alla vertex tar slut tid (Säg 5 timmar), problem hittas här.
  * Resursanvändning: Om du har tilldelat fler eller finns inte tillräckligt med parallellitet än behöver problem hittas här. Du kan också på resursen användning toosee mer information och utföra olika toofind en bättre resursallokering (Mer information finns i den här guiden).
  * Minneskontroll: om alla vertex använder mer än 5 GB minne, problem hittas här. Jobbkörningen kan hämta avslutats av systemet om det använder mer minne än begränsning i filsystemet.

## <a name="job-detail"></a>Jobbdetaljer
Information om jobbet visar hello detaljerad information för hello jobb, inklusive skript, resurser och Vertex vy.

![Azure Data Lake Analytics-Jobbdetaljer](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-details.png)

* Skript
  
    hello U-SQL-skript för hello jobb lagras i hello frågearkivet. Du kan visa hello ursprungliga U-SQL-skript och skicka det igen om det behövs.
* Resurser
  
    Du kan hitta hello jobbet kompilering utdata lagras i hello frågearkivet genom resurser. Exempelvis kan du hitta ”algebra.xml” som används tooshow hello Jobbdiagram hello sammansättningar som du har registrerat osv här.
* Vertex vy
  
    Den visar formhörnen körning av information. hello jobbet profil arkiveras varje vertex körningsloggen, till exempel totala läsa/skriva, runtime, tillstånd, osv. Den här vyn kan får du mer information om hur en jobbet kördes. Mer information finns i [Använd hello Vertex vy i Data Lake-verktyg för Visual Studio](data-lake-analytics-data-lake-tools-use-vertex-execution-view.md).

## <a name="next-steps"></a>Nästa steg
* toolog diagnostikinformation, se [åtkomst till diagnostik loggar för Azure Data Lake Analytics](data-lake-analytics-diagnostic-logs.md)
* toosee en mer komplex fråga, se [analysera webbplatsloggar med hjälp av Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).
* toouse vertex vy, se [Använd hello Vertex vy i Data Lake-verktyg för Visual Studio](data-lake-analytics-data-lake-tools-use-vertex-execution-view.md)

