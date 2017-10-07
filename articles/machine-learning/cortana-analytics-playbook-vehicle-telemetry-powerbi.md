---
title: "aaaPower BI-instrumentpanel för vehicle hälso- och köra vanor - Azure | Microsoft Docs"
description: "Använda hello funktionerna i Cortana Intelligence toogain i realtid och förutsägbara insikter på vehicle hälsotillstånd och andra vanor."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: aaeb29a5-4a13-4eab-bbf1-885690d86c56
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/16/2016
ms.author: bradsev
ms.openlocfilehash: 0bd054d943387ecad7301236eebae22458173aba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="vehicle-telemetry-analytics-solution-template-power-bi-dashboard-setup-instructions"></a>Vehicle telemetri analytics lösning mallen Power BI-instrumentpanel instruktioner
Detta **menyn** länkar toohello kapitlen i den här playbook. 

[!INCLUDE [cap-vehicle-telemetry-playbook-selector](../../includes/cap-vehicle-telemetry-playbook-selector.md)]

hello Vehicle telemetri Analytics lösningen visar hur bil hos återförsäljarna, bil tillverkare och försäkringsbolag kan utnyttja hello funktionerna i Cortana Intelligence toogain i realtid och förutsägbara insikter om vehicle hälsa och körning vanor toodrive förbättringar i hello area kunden upplevelse, R & D och marknadsföringskampanjer. Det här dokumentet innehåller steg-för-steg-anvisningar om hur du kan konfigurera hello Power BI-rapporter och instrumentpanel när hello lösningen har distribuerats i din prenumeration. 

## <a name="prerequisites"></a>Krav
1. Distribuera hello [telemetri Analytics](https://gallery.cortanaintelligence.com/Solution/5bdb23f3abb448268b7402ab8907cc90) lösning  
2. [Installera Microsoft Power BI Desktop](http://www.microsoft.com/download/details.aspx?id=45331)
3. En [Azure-prenumeration](https://azure.microsoft.com/pricing/free-trial/). Om du inte har en Azure-prenumeration, komma igång med Azure kostnadsfri prenumeration
4. Microsoft Power BI-konto

## <a name="cortana-intelligence-suite-components"></a>Cortana Intelligence Suite-komponenter
Som en del av hello Vehicle telemetri Analytics lösningsmall distribueras hello följande Cortana Intelligence-tjänster i din prenumeration.

* **Event Hub** för att föra in miljontals vehicle telemetriska händelser i Azure.
* **Strömma Analytics** för att få realtidsinsikter på vehicle hälsa och kvarstår dessa data till långsiktig lagring för bättre batch analytics.
* **Machine Learning** för identifiering av avvikelse i realtid och batchbearbetning toogain förutsägande insikter.
* **HDInsight** är balanserad tootransform data i skala
* **Data Factory** hanterar orchestration, schemaläggning, resurshantering och övervakning av hello batch process-pipelinen.

**Power BI** ger den här lösningen en omfattande instrumentpanel för data i realtid och förutsägelseanalys visualiseringar. 

hello lösningen använder två olika datakällor: **simulerade vehicle signaler och diagnostik dataset** och **vehicle katalogen**.

Vehicle telematik simulator ingår som en del av den här lösningen. Den genererar diagnostisk information och signalerar till motsvarande toohello tillstånd hello vehicle och intresseväckande mönster vid en viss tidpunkt. 

hello Vehicle katalog är en referens dataset som innehåller VIN toomodel mappning

## <a name="power-bi-dashboard-preparation"></a>Förberedelse av Power BI-instrumentpanel
### <a name="setup-power-bi-real-time-dashboard"></a>Konfigurera realtid Power BI-instrumentpanel

**Starta hello realtid instrumentpanelen programmet** när hello distributionen är klar bör du följa hello manuell åtgärd instruktioner

* Hämta realtid instrumentpanelen programmet RealtimeDashboardApp.zip och packa upp den.
*  Öppna appen konfigurationsfilen RealtimeDashboardApp.exe.config, Ersätt appSettings för Eventhub, Blob Storage och ML service-anslutningar med hello värden i hello manuell åtgärd instruktioner och spara ändringarna i uppackade hello-mappen.
* Kör program RealtimeDashboardApp.exe. Ett inloggningsfönster kommer popup-, ange din giltiga PowerBI-autentiseringsuppgifter och klickar på hello **acceptera** knappen. Hello app startar toorun.

   ![Logga in tooPower BI](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/5-sign-into-powerbi.png)
   
   ![Power BI-instrumentpanel behörigheter](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/6-powerbi-dashboard-permissions.png)

* Inloggningen tooPowerBI webbplats och skapa instrumentpanel i realtid.

Nu är du redo tooconfigure hello Powerbi-instrumentpanel med omfattande visualiseringar toogain realtid och förutsägbara insikter om vehicle hälsa och köra vanor. Det tar cirka 45 minuter tooan timme toocreate alla hello rapporter och konfigurera hello instrumentpanelen. 

### <a name="configure-power-bi-reports"></a>Konfigurera Power BI-rapporter
ta om 30 45 minuter toocomplete hello realtid rapporter och hello instrumentpanelen. Bläddra för[http://powerbi.com](http://powerbi.com) och logga in.

![Logga in tooPower BI](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/6-1-powerbi-signin.png)

En ny datamängd skapas i Power BI. Klicka på hello **ConnectedCarsRealtime** dataset.

![Markerad anslutna bilar realtid dataset](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/7-select-connected-cars-realtime-dataset.png)

Spara hello tom rapport med **Ctrl + s**.

![Spara tom rapport](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/8-save-blank-report.png)

Ange rapportens namn *Vehicle telemetri Analytics realtid - rapporter*.

![Ange rapportens namn](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/9-provide-report-name.png)

## <a name="real-time-reports"></a>Realtid rapporter
Det finns tre realtid rapporter i den här lösningen:

1. Fordon i åtgärden
2. Fordon kräver Underhåll
3. Fordon hälsostatistik

Du kan välja tooconfigure alla hello tre realtid rapporter eller stoppa efter någon gång och fortsätta toohello nästa avsnitt för att konfigurera hello batch-rapporter. Vi rekommenderar att du toocreate alla hello tre rapporterar toovisualize hello fullständig insikter hello realtid sökväg hello lösning.  

### <a name="1-vehicles-in-operation"></a>1. Fordon i åtgärden
Dubbelklicka på **sida 1** och Byt namn på den för ”fordon i åtgärden”  
    ![Anslutna bilar - fordon i åtgärd](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4a.png)  

Välj **vin** från **fält** och välj typ av visualiseringen som **”kort”**.  

Kort visualiseringen skapas som visas i bild.  
    ![Anslutna bilar - väljer vin](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4b.png)

Klicka på hello tomt område tooadd nya visualiseringen.  

Välj **Stad** och **vin** från fält. Ändra visualiseringen för**”karta”**. Dra **vin** i värdeområdet. Dra **Stad** från fält för**förklaring** område.   
    ![Ansluten bilar - kort visualiseringen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4c.png)

Välj **format** avsnittet från **visualiseringar**, klickar du på **rubrik** och ändra hello **Text** för**”fordon i åtgärden efter ort ”**.  
    ![Anslutna bilar - fordon i åtgärd efter ort](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4d.png)   

Sista visualiseringen ser ut som visas i bild.    
    ![Anslutna bilar - slutliga visualiseringen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4e.png)

Klicka på hello tomt område tooadd nya visualiseringen.  

Välj **Stad** och **vin**, ändra typen av visualisering för**grupperat stående stapeldiagram**. Se till att **Stad** i **axel området** och **vin** i **värdet område**  

Sortera diagram av **”antal vin”**  
    ![Anslutna bilar - antal vin](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4f.png)  

Ändra diagramtyp **rubrik** för**”fordon i åtgärd efter ort”**  

Klicka på hello **Format** avsnittet och väljer sedan **Data färger**, klicka på hello **”på”** för**visa alla**  
    ![Anslutna bilar – visa alla Data färger](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4g.png)  

Ändra hello färg på enskilda ort genom att klicka på ikonen färg.  
    ![Ansluten bilar - ändra färger](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4h.png)  

Klicka på hello tomt område tooadd nya visualiseringen.  

Välj **grupperat stående stapeldiagram** visualiseringen från visualiseringar, dra **Stad** i **axel** området **modellen** i **förklaring** området och **vin** i **värdet** område.  
    ![Anslutna bilar - grupperat stående stapeldiagram](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4i.png)  
    ![Anslutna bilar - återgivning](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4j.png)

Ordna om alla visualiseringen på den här sidan som visas i bild.  
    ![Anslutna bilar - visualiseringar](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4k.png)

Du har konfigurerat hello ”fordon i åtgärden” realtid rapporten. Du kan fortsätta toocreate hello nästa realtid rapport eller stoppa här och konfigurera hello instrumentpanelen. 

### <a name="2-vehicles-requiring-maintenance"></a>2. Fordon kräver Underhåll
Klicka på ![Lägg till](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4add.png) tooadd en ny rapport, byta namn på den för**”fordon kräver Underhåll”**

![Anslutna bilar - fordon kräver Underhåll](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4l.png)  

Välj **vin** fältet och ändra visualiseringen för**kort**.  
    ![Anslutna bilar - Vin kort visualiseringen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4m.png)  

Vi har ett fält med namnet ”MaintenanceLabel” i hello dataset. Det här fältet kan ha värdet ”0” eller ”1” ”. Den anges av hello Azure Machine Learning modell etablerats som en del av lösningen och integrerats med hello realtid sökväg. hello värdet ”1” anger fordon kräver underhåll. 

tooadd en **sidnivå** filter för att visa fordon data, vilket kräver Underhåll: 

1. Dra hello **”MaintenanceLabel”** omvandlas **nivå sidfilter**.  
   ![Anslutna bilar - sidnivå filter](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n1.png)  
2. Klicka på **grundläggande filtrering** menyn som finns längst ned i MaintenanceLabel sidfilter nivå.  
   ![Ansluten bilar - grundläggande filtrering](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n2.png)  
3. Ange filtervärdet för**”1”**    
   ![Anslutna bilar - filtervärdet](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n3.png)  

Klicka på hello tomt område tooadd nya visualiseringen.  

Välj **grupperat stående stapeldiagram** från visualiseringar  
![Anslutna bilar - Vind kort visualiseringen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4o.png)  
![Anslutna bilar - grupperat stående stapeldiagram](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4p.png)

Dra fältet **modellen** till **axel** området **Vin** för**värdet** område. Sortera visualisering av **antal vin**.  Ändra diagramtyp **rubrik** för**”fordon kräver underhåll av modell”**  

Dra **vin** fält i **färgmättnad** finns på **fält** ![fält](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4field.png) avsnitt i **visualiseringen** fliken  
![Anslutna bilar - färgmättnad](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4q.png)  

Ändra **Data färger** i visualiseringar från **Format** avsnitt  
Ändra minimifärg till: **F2C812**  
Ändra maxfärg till: **FF6300**  
![Ansluten bilar - ändringar](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4r.png)  
![Ansluten bilar - nya Visualiseringsfärger](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4s.png)  

Klicka på hello tomt område tooadd nya visualiseringen.  

Välj **klustrade stapeldiagram** dra från visualiseringar, **vin** fältet i **värdet** område, dra **Stad** omvandlas **axel** område. Sortera diagram av **”antal vin”**. Ändra diagramtyp **rubrik** för**”fordon kräver underhåll av ort”**   
![Anslutna bilar - fordon kräver underhåll av ort](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4t.png)  

Klicka på hello tomt område tooadd nya visualiseringen.  

Välj **flerradiga kort** visualiseringen från visualiseringar, dra **modellen** och **vin** till hello **fält** område.  
![Anslutna bilar - flerradiga-kort](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4u.png)    

Flytta alla hello visualiseringen hello slutgiltiga rapporten ser ut som följer:  
![Anslutna bilar - flerradiga-kort](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4v.png)  

Du har konfigurerat hello ”fordon kräver Underhåll” realtid rapporten. Du kan fortsätta toocreate hello nästa realtid rapport eller stoppa här och konfigurera hello instrumentpanelen. 

### <a name="3-vehicles-health-statistics"></a>3. Fordon hälsostatistik
Klicka på ![Lägg till](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4add.png) tooadd ny rapport, byta namn på den för**”fordon hälsostatistik”**  

Välj **mätaren** visualiseringen från visualiseringar, dra hello **hastighet** omvandlas **värde, minimalt värde maxvärdet** områden.  
![Anslutna bilar - flerradiga-kort](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4w.png)  

Ändra hello standard aggregering av **hastighet** i **värdet området** för**Genomsnittlig** 

Ändra hello standard aggregering av **hastighet** i **minsta område** för**minsta**

Ändra hello standard aggregering av **hastighet** i **maximalt området** för**maximala**

![Anslutna bilar - flerradiga-kort](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4x.png)  

Byt namn på hello **mätaren rubrik** för**”genomsnittlig hastighet”** 

![Anslutna bilar - mätare](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4y.png)  

Klicka på hello tomt område tooadd nya visualiseringen.  

Lägga till en **mätaren** för **genomsnittlig motorolja**, **genomsnittlig bränsle**, och **genomsnittlig motorn tempererade**.  

Ändra hello standard aggregering av fälten i varje mätaren enligt ovan stegen i **”genomsnittlig hastighet”** mätare.

![Anslutna bilar - mätare](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4z.png)

Klicka på hello tomt område tooadd nya visualiseringen.

Välj **linjediagram och grupperat stående stapeldiagram** från visualiseringar, dra **Stad** omvandlas **delade axel**, dra **hastighet**, **fälten tirepressure och engineoil** till **kolumnvärdena** område, ändra deras aggregeringstypen för**genomsnittlig**. 

Dra hello **engineTemperature** omvandlas **radvärden** område, ändra hello aggregeringstypen för**genomsnittlig**. 

![Anslutna bilar - visualiseringar fält](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4aa.png)

Ändra hello diagram **rubrik** för**”genomsnittlig hastighet, däck tryck, motorolja och motorn temperatur”**.  

![Anslutna bilar - visualiseringar fält](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4bb.png)

Klicka på hello tomt område tooadd nya visualiseringen.

Välj **Treemap** visualiseringen från visualiseringar, dra hello **modellen** omvandlas hello **grupp** området och dra hello fältet  **MaintenanceProbability** till hello **värden** område.

Ändra hello diagram **rubrik** för**”Vehicle modeller som kräver Underhåll”**.

![Ansluten bilar - ändra diagrammets rubrik](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4cc.png)

Klicka på hello tomt område tooadd nya visualiseringen.

Välj **100% staplad liggande diagram** dra hello från visualisering, **Stad** omvandlas hello **axel** området och dra hello **MaintenanceProbability**, **RecallProbability** fält i hello **värdet** område.

![Anslutna bilar - Lägg till nya visualiseringen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4dd.png)

Klicka på **Format**väljer **Data färger**, och ange hello **MaintenanceProbability** färgvärden toohello **”F2C80F”**.

Ändra hello **rubrik** av hello diagram för**”sannolikheten för Vehicle underhåll och återkalla av ort”**.

![Anslutna bilar - Lägg till nya visualiseringen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ee.png)

Klicka på hello tomt område tooadd nya visualiseringen.

Välj **ytdiagram** dra hello från visualiseringen från visualiseringar **modellen** omvandlas hello **axel** området och dra hello **engineOil, tirepressure, hastighet och MaintenanceProbability** fält i hello **värden** område. Ändra sina sammansättningstyp för**”genomsnittliga”**. 

![Ansluten bilar - ändra sammansättningstyp](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ff.png)

Ändra hello rubrik hello diagram för**”genomsnittlig motorolja, tröttnar sannolikheten för hög belastning, hastighet och underhåll av modell”**.

![Ansluten bilar - ändra diagrammets rubrik](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4gg.png)

Klicka på hello tomt område tooadd nya visualiseringen:

1. Välj **punktdiagram** visualiseringen från visualiseringar.
2. Dra hello **modellen** omvandlas hello **information** och **förklaring** område.
3. Dra hello **bränsle** omvandlas hello **x-axeln** område, ändra hello aggregering för**genomsnittlig**.
4. Dra **engineTemparature** till **y-axeln området**, ändra hello aggregering för**Genomsnittlig**
5. Dra hello **vin** omvandlas hello **storlek** område.

![Anslutna bilar - Lägg till nya visualiseringen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4hh.png)

Ändra hello diagram **rubrik** för**”medelvärden bränsle, motorn temperatur av modell”**.

![Ansluten bilar - ändra diagrammets rubrik](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ii.png)

hello slutgiltiga rapporten ser ut som nedan.

![Ansluten bilar-slutgiltig rapport](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4jj.png)

### <a name="pin-visualizations-from-hello-reports-toohello-real-time-dashboard"></a>PIN-kod visualiseringar hello rapporter toohello realtid instrumentpanel
Skapa en tom instrumentpanel genom att klicka på nästa tooDashboards för hello plus ikon. Du kan kalla den ”Vehicle telemetri instrumentpanelen”

![Anslutna bilar-instrumentpanelen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.5.png)

Fäst hello visualiseringen från hello ovan rapporter toohello instrumentpanelen. 

![Anslutna bilar-instrumentpanelen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.6.png)

hello instrumentpanelen ska se ut så här när alla hello tre rapporter skapas och hello motsvarande visualiseringar är fäst toohello instrumentpanelen. Om du inte har skapat alla hello rapporter instrumentpanelen kan se annorlunda ut. 

![Anslutna bilar-instrumentpanelen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-4.0.png)

Grattis! Du har skapat hello realtid instrumentpanelen. När du fortsätter tooexecute CarEventGenerator.exe och RealtimeDashboardApp.exe, bör du se live uppdateringar på hello instrumentpanel. Det bör ta cirka 10 too15 minuter toocomplete hello följande steg.

## <a name="setup-power-bi-batch-processing-dashboard"></a>Installera Power BI batch-bearbetning instrumentpanel
> [!NOTE]
> Det tar ca två timmar (från hello slutförande av hello distribution) för hello slutet tooend batchbearbetning toofinish pipelinekörningen och bearbeta ett år som skapas. Vänta så hello bearbetning toofinish innan du fortsätter med hello nästa steg. 
> 
> 

**Hämta hello Power BI designer-fil**

* En förkonfigurerad Power BI designer fil ingår som en del av hello distribution manuell åtgärd instruktioner
* Leta efter 2. Installationsprogrammet PowerBI batch bearbetning instrumentpanelen kan du hämta hello PowerBI-mall för batchbearbetning instrumentpanelen namnet **ConnectedCarsPbiReport.pbix**.
* Spara lokalt

**Konfigurera Power BI-rapporter**

* Öppna hello designer filen '**ConnectedCarsPbiReport.pbix**' med hjälp av Power BI Desktop. Om du inte redan har, installerar hello Power BI Desktop från [Power BI Desktop installera](http://www.microsoft.com/download/details.aspx?id=45331). 
* Klicka på hello **redigera frågor**.

![Redigera Power BI-fråga](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/10-edit-powerbi-query.png)

* Dubbelklicka på hello **källa**.

![Ange Power BI-källa](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/11-set-powerbi-source.png)

* Uppdatera Server-anslutningssträngen med hello Azure SQL-server som har etablerats som en del av hello-distribution.  Leta i hello manuell åtgärd anvisningarna under 

    4. Azure SQL Database
    
    * Server: somethingsrv.database.windows.net
    * Databas: connectedcar
    * Användarnamn: användarnamn
    * Lösenord: Du kan hantera ditt lösenord för SQL server från Azure-portalen

* Lämna **databasen** som *connectedcar*.

![Ange Power BI-databasen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/12-set-powerbi-database.png)

* Klicka på **OK**.
* Du ser **Windows-autentiseringsuppgifter** fliken markerad som standard kan ändra det för**databasen autentiseringsuppgifter** genom att klicka på **databasen** fliken längst till höger.
* Ange hello **användarnamn** och **lösenord** i din Azure SQL Database som angavs under installationen för distribution.

![Ange Databasautentiseringsuppgifter för](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/13-provide-database-credentials.png)

* Klicka på **ansluta**
* Upprepa hello ovanstående steg för varje hello tre återstående frågor finns i högra fönstret och uppdatera sedan hello anslutningsinformation om datakälla.
* Klicka på **Stäng och läsa in**. Power BI Desktop filen datauppsättningar är anslutna tooSQL Azure databastabeller.
* **Stäng** Power BI Desktop-fil.

![Stäng Power BI desktop](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/14-close-powerbi-desktop.png)

* Klicka på **spara** knappen toosave hello ändras. 

Du har nu konfigurerat alla hello rapporter motsvarande toohello batch bearbetning sökväg i hello-lösning. 

## <a name="upload-toopowerbicom"></a>Överför för*powerbi.com*
1. Navigera toohello Power BI-webbportalen på http://powerbi.com och logga in.
2. Klicka på **hämta Data**  
3. Överför hello Power BI Desktop-fil.  
4. tooupload, klickar du på **hämta Data -> hämta filer -> lokal fil**  
5. Navigera toohello **”**ConnectedCarsPbiReport.pbix**”**  
6. När hello-filen har överförts, kommer du att navigera tillbaka tooyour Power BI-arbetsyta.  

En datamängd, rapporten och en tom instrumentpanel skapas för dig.  

PIN-kod diagram tooa ny instrumentpanel som kallas **Vehicle telemetri instrumentpanelen** i **Power BI**. Klicka på hello tomma instrumentpanelen skapade ovan och gå sedan toohello **rapporter** avsnittet klickar du på hello nyligen har överförts till rapporten.  

![Vehicle telemetri Power BI.com](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard1.png) 

**Hello rapport har sex sidor:**  
Sidan 1: Vehicle densitet  
Sidan 2: Realtid vehicle hälsa  
Sidan 3: Aggressivt drivs fordon   
Sidan 4: Återkallas fordon  
Sidan 5: Bränsle effektivt drivs fordon  
Sidan 6: Contoso-logotyp  

![Anslutna bilar Power BI.com](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard2.png)

**Från sidan 3**, fästa hello följande:  

1. Antal VIN  
   ![Anslutna bilar Power BI.com](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard3.png) 
2. Aggressivt drivs fordon av modellen – vattenfallet diagram  
   ![Vehicle telemetri - PIN-kod diagram 4](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard4.png)

**Från sidan 5**, fästa hello följande: 

1. Antal vin    
   ![Vehicle telemetri - PIN-kod diagram 5](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard5.png)  
2. Bränsle effektivt fordon av modell: grupperat stående stapeldiagram  
   ![Vehicle telemetri - PIN-kod diagram 6](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard6.png)

**Från sidan 4**, fästa hello följande:  

1. Antal vin  
   ![Vehicle telemetri - PIN-kod diagram 7](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard7.png) 
2. Återkallade fordon efter ort, modell: Treemap  
   ![Vehicle telemetri - PIN-kod diagram 8](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard8.png)  

**Från sidan 6**, fästa hello följande:  

1. Contoso motorer-logotyp  
   ![Vehicle telemetri - PIN-kod diagram 9](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard9.png)

**Ordna hello instrumentpanelen**  

1. Navigera toohello instrumentpanelen
2. Hovra över varje diagram och Byt baserat på hello naming i hello komplett instrumentpanel bilden nedan. Även flytta hello diagram runt toolook som hello instrumentpanelen nedan.  
   ![Vehicle telemetri - ordna instrumentpanelen 2](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-organize-dashboard2.png)  
   ![Vehicle telemetri Power BI.com](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard.png)
3. Om du har skapat alla hello rapporter som anges i det här dokumentet hello sista slutförda instrumentpanelen bör se ut som hello följande bild. 

![Vehicle telemetri - ordna instrumentpanelen 2](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-organize-dashboard3.png)

Grattis! Du har skapat hello rapporter och hello instrumentpanelen toogain realtid, förutsägbara och batch insikter om vehicle hälsa och köra vanor.  
