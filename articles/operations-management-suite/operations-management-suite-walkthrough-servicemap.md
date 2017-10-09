---
title: "aaaService kartan lösning egen takt demo | Microsoft Docs"
description: "Tjänstkarta är en lösning i Operations Management Suite (OMS) och som automatiskt identifierar programkomponenter i Windows och Linux-system och kartor hello kommunikation mellan tjänster.  Det här är en självsignerat takt demo som går igenom med Tjänstkarta tooidentify och diagnostisera simulerade problem i ett webbprogram."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: 9dc437b9-e83c-45da-917c-cb4f4d8d6333
ms.service: operations-management-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/12/2017
ms.author: bwren
ms.openlocfilehash: 13f26241cd55a9b35c07d6ca52760a968abffc64
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="operations-management-suite-oms-self-paced-demo---service-map"></a>En demo för Operations Management Suite (OMS) – Tjänstkarta
Det här är en självsignerat takt demo går igenom med hello [Tjänstkarta lösning](operations-management-suite-service-map.md) i Operations Management Suite (OMS) tooidentify och diagnostisera simulerade problem i ett webbprogram.  Tjänstkarta identifierar automatiskt programkomponenter för Windows och Linux-datorer och maps hello kommunikation mellan tjänster.  Konsolideras också data som samlas in av andra tjänster OMS-tooassist Analysera prestanda och identifiera problem.  Du använder också [logga sökningar i logganalys](../log-analytics/log-analytics-log-searches.md) toodrill ned på insamlade data i ordning tooidentify hello rotproblemet.


## <a name="scenario-description"></a>Scenariobeskrivning
Du har precis fått ett meddelande om att hello ABC Kundportal program har problem med prestanda.  hello enda information som du har är att de här problemen startats cirka 4:00 am PST idag.  Du inte är helt säker på att alla komponenter i hello den hello-portalen är beroende av än en uppsättning webbservrar.  

## <a name="components-and-features-used"></a>Komponenter och funktioner som används
- [Lösningen Tjänstkarta](operations-management-suite-service-map.md)
- [Loggsökningar i Log Analytics](../log-analytics/log-analytics-log-searches.md)


## <a name="walk-through"></a>Genomgång

### <a name="1-connect-toohello-oms-experience-center"></a>1. Ansluta toohello OMS upplevelse Center
Denna genomgång använder hello [Operations Management Suite upplevelse Center](https://experience.mms.microsoft.com/) som innehåller en fullständig OMS-miljö med exempeldata. Starta genom att följa den här länken, ange din information och väljer sedan hello **insikt och Analytics** scenario.


### <a name="2-start-service-map"></a>2. Starta Tjänstkarta
Starta hello Tjänstkarta lösning genom att klicka på hello **Tjänstkarta** panelen.

![Panelen Tjänstkarta](media/operations-management-suite-walkthrough-servicemap/tile.png)

Hej Tjänstkarta konsolen visas.  I hello är till vänster en lista över datorer i din miljö med hello Tjänstkarta-agenten är installerad.  Väljer du hello-dator som du vill tooview från den här listan.

![Lista med datorer](media/operations-management-suite-walkthrough-servicemap/computer-list.png)


### <a name="3-view-computer"></a>3. Visa datorn
Vi vet att hello webbservrar kallas AcmeWFE001 och AcmeWFE002, så att det verkar som en rimlig plats toostart.  Klicka på **AcmeWFE001**.  Visar hello mappning för AcmeWFE001 och alla dess beroenden.  Du kan se vilka processer som körs på hello vald dator och som externa tjänster som de kommunicerar med.

![Webbserver](media/operations-management-suite-walkthrough-servicemap/web-server.png)

Vi är orolig hello prestanda för våra webbtjänster programmet så klickar du på hello **AcmeAppPool (IIS-programpool)** process.  Detta visar hello information för den här processen och visar dess beroenden.  

![Programpool](media/operations-management-suite-walkthrough-servicemap/app-pool.png)


### <a name="4-change-time-window"></a>4. Ändra tidsfönstret

Vi har hört hello åtgärdas startade kl. 4:00 AM vi en titt på vad som händer vid den tiden. Klicka på **tidsintervall** och ändra hello tid too4: 00 AM PST (och behålla hello aktuellt datum och justera för din lokala tidszon) med en varaktighet på 20 minuter.

![Tidväljaren](./media/operations-management-suite-walkthrough-servicemap/time-picker.png)


### <a name="5-view-alert"></a>5. Visa aviseringar

Vi kan nu se att hello **acmetomcat** beroende har en avisering som visas, så det är vår potentiella problem.  Klicka på hello varningsikon i **acmetomcat** tooshow hello information för hello aviseringen.  Vi kan se att processoranvändningen var kritiskt hög och kan expandera den här informationen.  Det här har förmodligen orsakat våra prestandaproblem. 

![Varning](./media/operations-management-suite-walkthrough-servicemap/alert.png)


### <a name="6-view-performance"></a>6. Visa prestanda

Låt oss titta närmare på **acmetomcat**.  Klicka i hello övre högra **acmetomcat** och välj **belastningen Server kartan** tooshow hello detaljer och beroenden för den här datorn. Vi kan sedan ser lite mer till dessa prestandaräknare tooverify våra misstanke.  Välj hello **prestanda** fliken toodisplay hello [prestandaräknare som samlas in av logganalys](../log-analytics/log-analytics-data-sources-performance-counters.md) över hello tidsintervall.  Vi kan se att vi håller på att periodiska toppar i hello processor och minne.

![Prestanda](./media/operations-management-suite-walkthrough-servicemap/performance.png)


### <a name="7-view-change-tracking"></a>7. Visa spårning av ändringar
Nu ska vi se om vi kan ta reda på vad som har orsakat den här höga användningsnivån.  Klicka på hello **sammanfattning** fliken.  Detta ger information som OMS har samlats in från hello datorn som misslyckades anslutningar, kritiska aviseringar och ändringar av programvaran.  Avsnitt med intressant senaste information redan ska utökas och du kan expandera andra avsnitt tooinspect information som de innehåller.


Om **Spårning av ändringar** inte redan är öppet expanderar du det här avsnittet.  Detta visar information som samlas in av hello [ändringsspårning lösning](../log-analytics/log-analytics-change-tracking.md).  Det verkar som om att en ändring i programvaran har utförts under den här tidsperioden.  Klicka på **programvara** tooget information.  En säkerhetskopieringsprocess har lagts till toohello datorn precis efter 4:00 AM, så visas detta toobe hello sällan för hello alltför stora resurser används.

![Spårning av ändringar](./media/operations-management-suite-walkthrough-servicemap/change-tracking.png)



### <a name="8-view-details-in-log-search"></a>8. Visa information i loggsökningar
Vi verifiera detta ytterligare genom att titta på hello detaljerad prestandainformation som samlas in i hello logganalys-databasen.  Klicka på hello **aviseringar** fliken igen och klicka sedan på något av hello **hög CPU** aviseringar.  Klicka på **Visa i loggsökning**.  Öppnas hello loggen Sök där du kan utföra [logga sökningar](../log-analytics/log-analytics-log-searches.md) mot data som lagrats i hello-databas.  Tjänstkarta ifylld en queriy för oss tooretrieve hello avisering vi är intresserad av.  

![Loggsökning](./media/operations-management-suite-walkthrough-servicemap/log-search.png)


### <a name="9-open-saved-search"></a>9. Öppna sparad sökning
Låt oss se om vi kan hämta vissa mer information om insamling av prestanda hello som genererade aviseringen och verifiera vår misstanke hello problem som orsakas av att säkerhetskopieringen.  Ändra hello tidsintervall för**6 timmar**.  Klicka på **Favoriter** och rulla ned toohello sparade sökningar för **Tjänstkarta**.  Det här är frågor som vi har skapat särskilt för den här analysen.  Klicka på **Top 5 Processes by CPU for acmetomcat** (De 5 processer som har högst processoranvändning för acmetomcat).

![Sparad sökning](./media/operations-management-suite-walkthrough-servicemap/saved-search.png)


Den här frågan returnerar en lista över hello 5 viktigaste processer som förbrukar processor på **acmetomcat**.  Du kan inspektera hello frågan tooget en introduktion toohello-frågespråket som används för sökningar i loggen.  Om du är intresserad av hello processer på andra datorer, kan du ändra hello frågan tooretrieve informationen.

I det här fallet kan vi se att hello säkerhetskopieringsprocessen konsekvent förbrukar ungefär 60% av CPU hello app-servern.  Det är ganska uppenbart att den här nya processen orsakar våra prestandaproblem.  Vår lösning givetvis är detta något nytt tooremove säkerhetskopieringsprogram av hello-programserver.  Vi kan faktiskt utnyttja önskad tillstånd Configuration (DSC) hanteras av Azure Automation toodefine principer som den här processen ska aldrig köras på dessa kritiska system.


## <a name="summary-points"></a>Sammanfattning
- Med [Tjänstkarta](operations-management-suite-service-map.md) får du en överblick över hela programmet även om du inte känner till alla servrar och beroenden.
- Tjänstkarta hämtar data som samlats in av andra lösningar OMS-toohelp du identifierar problem med programmet och dess underliggande infrastruktur.
- [Logga sökningar](../log-analytics/log-analytics-log-searches.md) kan du toodrill ned till specifika data som samlas in i hello logganalys-databasen.    

## <a name="next-steps"></a>Nästa steg
- Läs mer om [Tjänstkarta](operations-management-suite-service-map.md).
- [Distribuera och konfigurera](operations-management-suite-service-map-configure.md) Tjänstkarta.
- Läs mer om [Log Analytics](../log-analytics/log-analytics-overview.md) som krävs för Tjänstkarta och som lagrar driftdata från agenter.