---
title: "aaaHow toomonitor en molnbaserad tjänst | Microsoft Docs"
description: "Lär dig hur toomonitor cloud services med hjälp av hello klassiska Azure-portalen."
services: cloud-services
documentationcenter: 
author: thraka
manager: timlt
editor: 
ms.assetid: 5c48d2fb-b8ea-420f-80df-7aebe2b66b1b
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/07/2015
ms.author: adegeo
ms.openlocfilehash: ee98c56e0b98b85d75a5c1d796800069c4f06d20
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomonitor-cloud-services"></a>Hur tooMonitor Cloud Services
[!INCLUDE [disclaimer](../../includes/disclaimer.md)]

Du kan övervaka `key` prestandastatistik för dina molntjänster i hello klassiska Azure-portalen. Du kan ange hello nivå av övervakning toominimal och utförlig för varje roll för tjänsten och kan anpassa hello övervakning visar. Utförlig övervakningsdata lagras i ett lagringskonto som du kan komma åt utanför hello-portalen. 

Övervakning visas i hello klassiska Azure-portalen kan konfigureras för hög. Du hello mått du vill toomonitor under hello mått på hello **övervakaren** , och du kan välja vilka mått tooplot i mått diagram på hello **övervakaren** sida och hello instrumentpanelen. 

## <a name="concepts"></a>Koncept
Som standard ges minimal övervakning för en ny molntjänst med hjälp av prestandaräknare som samlats in från hello värddatorns operativsystem för hello roller instanser (virtuella datorer). hello är minimal begränsad tooCPU procent, Data i, utdata, Diskgenomflödet läsa och skriva Diskgenomflödet. Genom att konfigurera utförlig övervakning kan få du ytterligare mått baserat på prestandadata hello virtuella datorer (rollinstanser). hello utförlig mått aktivera närmare analys av problem som uppstår under program-åtgärder.

Som standard är prestandaräknardata från rollinstanser provtagning och överförs från hälsningspaket rollinstansen vid 3-minutersintervall. När du aktiverar utförlig övervakning sammanställs hello rådata prestandaräknardata för varje rollinstans och över rollinstanser för varje roll med intervall på 5 minuter, 1 timme och 12 timmar. hello aggregerade data tas bort efter 10 dagar.

När du aktiverar utförlig övervakning hello samman lagras övervakningsdata i tabeller i ditt lagringskonto. tooenable utförlig övervakning för en roll, måste du konfigurera en anslutningssträng för diagnostik som länkar toohello storage-konto. Du kan använda olika lagringskonton för olika roller.

Aktivera utförlig övervakning ökar relaterade din lagringskostnader toodata lagringsutrymme, dataöverföring och lagringstransaktioner. Minimal övervakning kräver inte ett lagringskonto. hello data för hello mått som exponeras i hello övervakning miniminivå lagras inte i ditt lagringskonto, även om du anger hello övervakning nivå tooverbose.

## <a name="how-to-configure-monitoring-for-cloud-services"></a>Så här: Konfigurera övervakning för molntjänster
Använd följande procedurer tooconfigure utförlig eller minimalt övervakning i hello klassiska Azure-portalen hello. 

### <a name="before-you-begin"></a>Innan du börjar
* Skapa en *klassiska* storage-konto toostore hello övervakningsdata. Du kan använda olika lagringskonton för olika roller. Mer information finns i [hur toocreate ett lagringskonto](../storage/common/storage-create-storage-account.md#create-a-storage-account).
* Aktivera Azure-diagnostik för dina molntjänstroller. Se [konfigurera diagnostik för molntjänster](cloud-services-dotnet-diagnostics.md).

Kontrollera att anslutningssträngen för hello diagnostik finns i hello-rollkonfigurationen. Du kan inte aktivera utförlig övervakning tills du aktiverar Azure-diagnostik och inkludera en anslutningssträng för diagnostik i hello-rollkonfigurationen.   

> [!NOTE]
> Projekt riktad Azure SDK 2.5 inkluderade inte automatiskt hello diagnostik anslutningssträngen i hello projektmall. För dessa projekt toomanually måste lägga till hello diagnostik anslutning sträng toohello rollkonfiguration.
> 
> 

**toomanually lägger till diagnostik anslutningskonfiguration sträng tooRole**

1. Öppna hello Cloud Service-projekt i Visual Studio
2. Dubbelklicka på hello **rollen** tooopen hello rollen designer och välj hello **inställningar** fliken
3. Leta efter en inställning med namnet **Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString**. 
4. Om den här inställningen inte finns, klickar du på hello **Lägg till inställning** knappen tooadd den toohello konfiguration och ändra hello typ för hello ny inställning för**ConnectionString**
5. Ange hello värde för anslutningen sträng hello genom att klicka på hello **...**  knappen. Då öppnas en dialogruta där du tooselect ett lagringskonto.
   
    ![Visual Studio-inställningar](./media/cloud-services-how-to-monitor/CloudServices_Monitor_VisualStudioDiagnosticsConnectionString.png)

### <a name="toochange-hello-monitoring-level-tooverbose-or-minimal"></a>toochange hello övervakning nivå tooverbose eller minimalt
1. I hello [klassiska Azure-portalen](https://manage.windowsazure.com/)öppnar hello **konfigurera** för hello cloud service-distributionen.
2. I **nivå**, klickar du på **utförlig** eller **Minimal**. 
3. Klicka på **Spara**.

När du aktiverar utförlig övervakning, bör du börja ser hello övervakningsdata i hello klassiska Azure-portalen hello timmen.

hello rådata prestandaräknardata och aggregerade övervakningsdata lagras i hello storage-konto i tabeller kvalificerat av hello distributions-ID för hello roller. 

## <a name="how-to-receive-alerts-for-cloud-service-metrics"></a>Så här: få aviseringar om cloud service mått
Du kan ta emot varningar baserat på din molntjänst övervakning mått. På hello **hanteringstjänster** sidan hello klassiska Azure-portalen kan du skapa en regel tootrigger en avisering när hello mått som du når ett värde som du anger. Du kan också välja toohave e-post skickas när hello avisering utlöses. Mer information finns i [så här: ta emot aviseringar och hantera Aviseringsregler i Azure](http://go.microsoft.com/fwlink/?LinkId=309356).

## <a name="how-to-add-metrics-toohello-metrics-table"></a>Så här: Lägg till mått toohello mått tabell
1. I hello [klassiska Azure-portalen](http://manage.windowsazure.com/)öppnar hello **övervakaren** för hello-Molntjänsten.
   
    Som standard visar hello mått tabellen en delmängd av hello tillgängliga mått. hello bild visar hello standard utförlig mätvärden för en molnbaserad tjänst som är begränsad toohello prestandaräknaren för Minne\Tillgängliga megabyte med data som aggregeras på hello rollnivå. Använd **lägga till mätvärden** tooselect ytterligare mängd- eller rollnivå mått toomonitor i hello klassiska Azure-portalen.
   
    ![Visa utförlig](./media/cloud-services-how-to-monitor/CloudServices_DefaultVerboseDisplay.png)
2. tooadd mått toohello mått tabell:
   
   1. Klicka på **lägga till mätvärden** tooopen **välja mått**, visas nedan.
      
       hello första tillgängliga mått är expanderat tooshow alternativ som är tillgängliga. Hello översta alternativet visar aggregerade övervakningsdata för alla roller för varje mått. Dessutom kan välja du enskilda roller toodisplay data för.
      
       ![Lägg till mått](./media/cloud-services-how-to-monitor/CloudServices_AddMetrics.png)
   2. tooselect mått toodisplay
      
      * Klicka på hello NEDPIL av hello mått tooexpand hello alternativ för övervakning.
      * Välj hello kryssrutan för varje övervakning alternativ toodisplay.
        
        Du kan visa upp too50 mått i hello mått tabell.
        
        > [!TIP]
        > Utförlig övervaka kan hello mått listan innehålla dussintals mått. toodisplay en rullningslist hovra över hello höger sida av hello dialogrutan. toofilter hello-listan, klicka på sökikonen hello och ange text i sökrutan hello enligt nedan.
        > 
        > 
        
        ![Lägg till mått-sökning](./media/cloud-services-how-to-monitor/CloudServices_AddMetrics_Search.png)
3. Klicka på OK (markering) när du är klar med att välja mått.
   
    hello läggs valda mått toohello mått tabell, enligt nedan.
   
    ![Övervakaren mått](./media/cloud-services-how-to-monitor/CloudServices_Monitor_UpdatedMetrics.png)
4. toodelete ett mått från hello mått tabell, klickar du på hello mått tooselect och klicka sedan på **ta bort måttet**. (Du kan bara se **ta bort måttet** när du har ett mått har valts.)

### <a name="tooadd-custom-metrics-toohello-metrics-table"></a>tooadd anpassade mått toohello mått tabell
Hej **utförlig** övervakning nivå innehåller en lista över standard mått som du kan övervaka på hello-portalen. Dessutom toothese kan du övervaka alla anpassade mått eller en prestandaräknare som definieras av programmet hello-portalen.

hello följande steg förutsätter att du har aktiverat **utförlig** övervakning nivå och har konfigurerat dina program toocollect och överför anpassade prestandaräknare. 

toodisplay hello anpassade prestandaräknare i hello portal som du behöver tooupdate hello konfiguration i bomullstuss-kontroll-behållaren:

1. Öppna hello bomullstuss-kontroll-container blob i din diagnostiklagringskonto. Du kan använda Visual Studio eller några andra lagring explorer toodo detta.
   
    ![Visual Studio Server Explorer](./media/cloud-services-how-to-monitor/CloudServices_Monitor_VisualStudioBlobExplorer.png)
2. Navigera hello blob sökväg med hello mönster **RoleInstance-DeploymentId/RoleName** toofind hello konfigurationen för din rollinstans. 
   
    ![Lagringsutforskaren för Visual Studio](./media/cloud-services-how-to-monitor/CloudServices_Monitor_VisualStudioStorage.png)
3. Hämta hello konfigurationsfil för din rollinstans och uppdatera den tooinclude eventuella anpassade prestandaräknare. Till exempel toomonitor *Disk-skrivna byte/s* för hello *C-enheten* Lägg till följande hello under **PerformanceCounters\Subscriptions** nod
   
    ```xml
    <PerformanceCounterConfiguration>
    <CounterSpecifier>\LogicalDisk(C:)\Disk Write Bytes/sec</CounterSpecifier>
    <SampleRateInSeconds>180</SampleRateInSeconds>
    </PerformanceCounterConfiguration>
    ```
4. Spara hello ändringar och överför hello configuration file tillbaka toohello hello samma plats att skriva över befintliga filer i hello blob.
5. Växla tooVerbose läget i hello Azure klassiska portal-konfiguration. Om du redan fanns i utförligt läge har du tootoggle toominimal och tillbaka tooverbose.
6. hello anpassade prestandaräknare kommer nu att vara tillgänglig i hello **lägga till mätvärden** dialogrutan. 

## <a name="how-to-customize-hello-metrics-chart"></a>Så här: anpassa hello mått diagram
1. Välj too6 mått tooplot i hello mått diagram i hello mått tabell. tooselect mått, klickar du på hello på vänster sida. tooremove ett mått från hello mått diagram, avmarkerar du kryssrutan i hello mått tabell.
   
    När du väljer mått i hello mått tabell läggs hello mått toohello mått diagram. En smala visas en **n mer** listrutan innehåller mått huvuden som inte får plats hello visas.
2. tooswitch mellan att visa relativa värden (sista värde endast för varje mått) och absoluta värden (Y-axeln visas), Välj relativ eller absolut hello överst i hello diagram.
   
    ![Relativ eller absolut](./media/cloud-services-how-to-monitor/CloudServices_Monitor_RelativeAbsolute.png)
3. toochange hello tid intervallet hello mått diagrammet visar, Välj 1 timme, 24 timmar eller sju dagar hello överst i hello diagram.
   
    ![Övervakaren Visa period](./media/cloud-services-how-to-monitor/CloudServices_Monitor_DisplayPeriod.png)
   
    Hello-metoden för ritområdet mått är olika i hello instrumentpanelen mått diagram. En standarduppsättning mått är tillgänglig och mått läggs till eller tas bort genom att välja mått hello-huvud.

### <a name="toocustomize-hello-metrics-chart-on-hello-dashboard"></a>toocustomize hello mått diagram på instrumentpanelen för hello
1. Öppna hello instrumentpanelen hello-Molntjänsten.
2. Lägg till eller ta bort mått från hello diagram:
   
   * tooplot en ny mått, Välj hello kryssruta för hello mått i hello diagram rubriker. På en smala skärm, klickar du på hello NEDPIL av  ***n* ? mått** tooplot ett huvud för mått hello diagramområde inte kan visas.
   * toodelete ett mått som ska ritas på hello diagram, rensa hello kryssrutan vid huvudet.
   
3. Växla mellan **relativa** och **absolut** visar.
4. Välj 1 timme, 24 timmar eller data toodisplay 7 dagar.

## <a name="how-to-access-verbose-monitoring-data-outside-hello-azure-classic-portal"></a>Så här: komma åt utförlig övervakningsdata utanför hello klassiska Azure-portalen
Utförlig övervakningsdata lagras i tabeller i hello storage-konton som du anger för varje roll. För varje cloud service-distribution skapas sex tabeller för hello roll. Två tabeller skapas för varje (5 minuter, 1 timme och 12 timmar). En av dessa tabeller lagrar roll på objektnivå aggregeringar; Hej andra tabell lagrar aggregeringar för rollinstanser. 

hello tabellnamn har hello följande format:

```
WAD*deploymentID*PT*aggregation_interval*[R|RI]Table
```

Där:

* *deploymentID* är hello GUID som tilldelas toohello cloud service-distributionen
* *aggregation_interval* = 5 M, 1 H eller 12 H
* rollen på objektnivå aggregeringar = R
* aggregeringar för rollinstanser = RI

Till exempel lagras hello följande tabeller utförlig övervakningsdata samman med 1 timmars intervall:

```
WAD8b7c4233802442b494d0cc9eb9d8dd9fPT1HRTable (hourly aggregations for hello role)

WAD8b7c4233802442b494d0cc9eb9d8dd9fPT1HRITable (hourly aggregations for role instances)
```
