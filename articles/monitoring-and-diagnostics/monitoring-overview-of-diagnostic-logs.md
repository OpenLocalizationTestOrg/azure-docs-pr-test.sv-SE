---
title: aaaOverview av Azure diagnostikloggar | Microsoft Docs
description: "Lär dig vad Azure diagnostikloggar är och hur du kan använda dem toounderstand händelser som inträffar inom en Azure-resurs."
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: fe8887df-b0e6-46f8-b2c0-11994d28e44f
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/21/2017
ms.author: johnkem; magoedte
ms.openlocfilehash: e38991c540626b4bb5b5b9a995276881ee58f368
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="collect-and-consume-log-data-from-your-azure-resources"></a>Samla in och använda loggdata från resurserna i Azure

## <a name="what-are-azure-resource-diagnostic-logs"></a>Vad är Azure-resurs diagnostikloggar
**Azure resursnivå diagnostikloggar** är loggar orsakat av en resurs som innehåller omfattande, ofta data om hello driften av den här resursen. hello innehållet i de här loggarna varierar beroende på resurstypen. Till exempel finns Nätverkssäkerhetsgruppen regeln räknare och Key Vault granskningar två typer av loggar för resursen.

Resursnivå diagnostikloggar skiljer sig från hello [aktivitetsloggen](monitoring-overview-activity-logs.md). hello aktivitetsloggen ger inblick i hello-åtgärder som utfördes på resurser i prenumerationen med Resource Manager, till exempel skapa en virtuell dator eller ta bort en logikapp. hello aktivitetsloggen är en prenumeration på objektnivå logg. Resursnivå diagnostikloggar ger kunskaper om åtgärder som utförts i den här resursen, till exempel en hemlighet komma från ett Nyckelvalv.

Resursnivå diagnostikloggar också skilja sig från diagnostikloggar för gäst-OS-nivå. Gästoperativsystem diagnostikloggar är de som samlats in av en agent som körs i en virtuell dator eller andra stöd för resurstypen. Resursnivå diagnostikloggar kräver ingen agent och avbilda resurs-specifik information från hello Azure-plattformen, medan gäst-OS-nivå diagnostikloggar samla in data från hello operativsystem och program som körs på en virtuell dator.

Inte alla resurser som stöder hello ny typ av resurs diagnostikloggar som beskrivs här. Den här artikeln innehåller en lista över avsnitt som vilka resurstyper stöder hello nya resursnivå diagnostikloggar.

![Resursen diagnostik loggar jämfört med andra typer av loggar ](./media/monitoring-overview-of-diagnostic-logs/Diagnostics_Logs_vs_other_logs_v5.png)

## <a name="what-you-can-do-with-resource-level-diagnostic-logs"></a>Vad du kan göra med resursnivå diagnostikloggar
Här följer några av hello saker du kan göra med resurs diagnostikloggar:

![Logiska placeringen av resursen diagnostikloggar](./media/monitoring-overview-of-diagnostic-logs/Diagnostics_Logs_Actions.png)


* Spara dem tooa [ **Lagringskonto** ](monitoring-archive-diagnostic-logs.md) för granskning eller Manuell kontroll. Du kan ange hello kvarhållning tid (i dagar) med **diagnostiska resursinställningar**.
* [Strömma dem för**Händelsehubbar** ](monitoring-stream-diagnostic-logs-to-event-hubs.md) för införandet av en tjänst från tredje part eller anpassade analytics lösning, till exempel PowerBI.
* Analysera dem med [OMS logganalys](../log-analytics/log-analytics-azure-storage.md)

Du kan använda ett lagringskonto eller Händelsehubbar namnområde som inte är i hello samma prenumeration som hello en avger loggar. hello-användare som konfigurerar hello inställning måste ha hello lämpliga RBAC åtkomst tooboth prenumerationer.

## <a name="resource-diagnostic-settings"></a>Diagnostikinställningar för resurs
Resursen diagnostikloggar för icke-beräkning resurser konfigureras med hjälp av diagnostikinställningar för resursen. **Diagnostiska resursinställningar** för en resurskontroll:

* Där resursen diagnostikloggar och mått som skickas (Storage-konto, Event Hubs och/eller OMS logganalys).
* Vilka kategorier loggen skickas och om mått data skickas också.
* Hur länge varje logg kategori ska behållas i ett lagringskonto
    - En kvarhållning av noll dagar innebär loggar behålls alltid. Annars kan hello värdet vara valfritt antal dagar mellan 1 och 2147483647.
    - Om bevarandeprinciper har angetts men lagring loggar i ett Storage-konto har inaktiverats (till exempel om endast Händelsehubbar eller OMS-alternativen är markerade), har hello bevarandeprinciper ingen effekt.
    - Bevarandeprinciper tillämpade per dag, så vid hello slutet på dagen (UTC) loggar från hello dag som inte har nu hello bevarandeprincip tas bort. Till exempel om du har en bevarandeprincip på en dag, skulle hello början av hello dagen idag hello loggar från hello dag före igår tas bort.

Dessa inställningar konfigureras enkelt via hello diagnostikinställningarna för en resurs i hello Azure-portalen, via Azure PowerShell och CLI-kommandon eller via hello [REST-API för Azure-Monitor](https://msdn.microsoft.com/library/azure/dn931943.aspx).

> [!WARNING]
> Diagnostikloggar och mått för från hello gäst OS lager av beräkning resurser (till exempel virtuella datorer eller Service Fabric) Använd [en separat mekanism för konfiguration och val av utdata](../azure-diagnostics.md).
>
>

## <a name="how-tooenable-collection-of-resource-diagnostic-logs"></a>Hur tooenable samling resurs diagnostikloggar
Samling av resursen diagnostikloggar kan aktiveras [som en del av att skapa en resurs i en Resource Manager-mall](./monitoring-enable-diagnostic-logs-using-template.md) eller när en resurs har skapats från den här resursen sida i hello-portalen. Du kan också aktivera samlingen när som helst via Azure PowerShell eller CLI-kommandon, eller använder hello Azure övervakaren REST API.

> [!TIP]
> Dessa instruktioner gäller kanske inte direkt tooevery resurs. Finns hello schemat länkarna längst ned hello i den här sidan toounderstand särskilda åtgärder som gäller toocertain resurstyper.
>
>

### <a name="enable-collection-of-resource-diagnostic-logs-in-hello-portal"></a>Aktivera insamling av resursen diagnostikloggar hello-portalen
Du kan aktivera insamling av resursen diagnostikloggar i hello Azure portal när en resurs har skapats genom att gå tooa specifik resurs eller genom att gå tooAzure övervakaren. tooenable via Azure-Monitor:

1. I hello [Azure-portalen](http://portal.azure.com), navigera tooAzure övervakaren och klicka på **diagnostikinställningar**

    ![Avsnittet av Azure-Monitor övervakning](media/monitoring-overview-of-diagnostic-logs/diagnostic-settings-blade.png)

2. Du kan också hello-filterlista av resursgruppen eller resursen, och klicka sedan på hello resursen som du vill att tooset en diagnostikinställningen.

3. Om det finns inga inställningar på hello resursen som du har valt, kan du ange toocreate en inställning. Klicka på ”Aktivera diagnostik”.

   ![Lägg till diagnostikinställningen - inga befintliga inställningar](media/monitoring-overview-of-diagnostic-logs/diagnostic-settings-none.png)

   Om det finns befintliga inställningarna på hello resurs, visas en lista över inställningar som redan har konfigurerats på den här resursen. Klicka på ”Lägg till diagnostikinställningen”.

   ![Lägg till diagnostikinställningen - befintliga inställningar](media/monitoring-overview-of-diagnostic-logs/diagnostic-settings-multiple.png)

3. Ge din ange ett namn, hello kryssrutorna för varje mål toowhich du gillar toosend data och konfigurera vilken resurs som används för varje mål. Du kan också ange ett antal dagar tooretain loggarna för med hello **bevarande (dagar)** skjutreglage (endast tillämpliga toohello konto lagringsplats). En kvarhållning av noll dagar lagrar hello loggar under obestämd tid.
   
   ![Lägg till diagnostikinställningen - befintliga inställningar](media/monitoring-overview-of-diagnostic-logs/diagnostic-settings-configure.png)
    
4. Klicka på **Spara**.

Efter en liten stund angetts hello nya visas inställningen i din lista över inställningar för den här resursen och diagnostikloggar skickas toohello mål som nya händelsedata genereras.

### <a name="enable-collection-of-resource-diagnostic-logs-via-powershell"></a>Aktivera insamling av resursen diagnostikloggar via PowerShell
tooenable samling resurs diagnostikloggar via Azure PowerShell, Använd hello följande kommandon:

tooenable lagring av diagnostiska loggar i ett lagringskonto, Använd följande kommando:

```powershell
Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -StorageAccountId [your storage account id] -Enabled $true
```

hello lagring konto-ID är hello resurs-ID för hello storage-konto toowhich du vill toosend hello loggar.

tooenable strömning av diagnostikloggar tooan händelsehubb, Använd följande kommando:

```powershell
Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -ServiceBusRuleId [your Service Bus rule id] -Enabled $true
```

regel-ID för hello service bus är en sträng med formatet: `{Service Bus resource ID}/authorizationrules/{key name}`.

tooenable att skicka diagnostikloggar tooa logganalys-arbetsytan, Använd följande kommando:

```powershell
Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -WorkspaceId [resource id of hello log analytics workspace] -Enabled $true
```

Du kan hämta hello resurs-ID för logganalys-arbetsytan med hello följande kommando:

```powershell
(Get-AzureRmOperationalInsightsWorkspace).ResourceId
```

Du kan kombinera flera Utdataalternativ tooenable dessa parametrar.

### <a name="enable-collection-of-resource-diagnostic-logs-via-cli"></a>Aktivera insamling av resursen diagnostikloggar via CLI
tooenable samling resurs diagnostikloggar via hello Azure CLI, använder hello följande kommandon:

tooenable lagring av diagnostiska loggar i ett Lagringskonto, Använd följande kommando:

```azurecli
azure insights diagnostic set --resourceId <resourceId> --storageId <storageAccountId> --enabled true
```

hello lagring konto-ID är hello resurs-ID för hello storage-konto toowhich du vill toosend hello loggar.

tooenable strömning av diagnostikloggar tooan händelsehubb, Använd följande kommando:

```azurecli
azure insights diagnostic set --resourceId <resourceId> --serviceBusRuleId <serviceBusRuleId> --enabled true
```

regel-ID för hello service bus är en sträng med formatet: `{Service Bus resource ID}/authorizationrules/{key name}`.

tooenable att skicka diagnostikloggar tooa logganalys-arbetsytan, Använd följande kommando:

```azurecli
azure insights diagnostic set --resourceId <resourceId> --workspaceId <resource id of hello log analytics workspace> --enabled true
```

Du kan kombinera flera Utdataalternativ tooenable dessa parametrar.

### <a name="enable-collection-of-resource-diagnostic-logs-via-rest-api"></a>Aktivera insamling av resursen diagnostikloggar via REST API
toochange diagnostikinställningar med hello Azure övervakaren REST API finns [dokumentet](https://msdn.microsoft.com/library/azure/dn931931.aspx).

## <a name="manage-resource-diagnostic-settings-in-hello-portal"></a>Hantera diagnostiska resursinställningar hello-portalen
Se till att alla dina resurser har ställts in för diagnostikinställningar. Navigera för**övervakaren** i hello portalen och öppna **diagnostikinställningar**.

![Den diagnostiska loggar bladet i hello portal](./media/monitoring-overview-of-diagnostic-logs/diagnostic-settings-nav.png)

Du kan ha tooclick ”fler tjänster” toofind hello övervakaren avsnitt.

Här kan du visa och filtrera alla resurser som stöder diagnostikinställningar toosee om de har aktiverat diagnostik. Du kan också detaljnivån toosee om flera inställningar konfigureras för en resurs och kontrollera vilka storage-konto, Händelsehubbar namnområde och/eller logganalys-arbetsytan som data flödar till.

![Den diagnostiska loggar resulterar i portalen](./media/monitoring-overview-of-diagnostic-logs/diagnostic-settings-blade.png)

Lägga till en diagnostikinställningen öppnas hello diagnostikinställningar för vyn, där du kan aktivera, inaktivera eller ändra dina diagnostikinställningar för hello markerat resurs.

## <a name="supported-services-categories-and-schemas-for-resource-diagnostic-logs"></a>Tjänster som stöds, kategorier och scheman för resource diagnostikloggar
[Finns den här artikeln](monitoring-diagnostic-logs-schema.md) för en fullständig lista över tjänster som stöds och hello loggen kategorier och scheman som används av tjänsterna.

## <a name="next-steps"></a>Nästa steg

* [Strömma resurs diagnostikloggar för**Händelsehubbar**](monitoring-stream-diagnostic-logs-to-event-hubs.md)
* [Ändra resurs diagnostikinställningar med hello Azure övervakaren REST API](https://msdn.microsoft.com/library/azure/dn931931.aspx)
* [Analysera loggar från Azure storage med logganalys](../log-analytics/log-analytics-azure-storage.md)
