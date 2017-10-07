---
title: aaaStream hello Azure-aktivitetsloggen tooEvent Hubs | Microsoft Docs
description: "Lär dig hur toostream hello Azure-aktivitetsloggen tooEvent Hubs."
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: ec4c2d2c-8907-484f-a910-712403a06829
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 6/06/2017
ms.author: johnkem
ms.openlocfilehash: 336f92771b9d4379ad9dbcadc6997dfae7fae7bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="stream-hello-azure-activity-log-tooevent-hubs"></a>Strömma hello Azure-aktivitetsloggen tooEvent hubbar
Hej [ **Azure-aktivitetsloggen** ](monitoring-overview-activity-logs.md) kan strömmas i nära realtid tooany program med hjälp av hello inbyggda ”Export” alternativet hello-portalen eller genom att aktivera hello Service Bus regel-Id i en logg profil via hello Azure PowerShell-Cmdlets eller Azure CLI.

## <a name="what-you-can-do-with-hello-activity-log-and-event-hubs"></a>Vad du kan göra med hello aktivitetsloggen och Händelsehubbar
Här följer några olika sätt som du kan använda hello strömning möjligheten för hello aktivitetsloggen:

* **Strömma toothird loggning och telemetri part** – över tiden, Händelsehubbar strömning ska bli hello mekanism toopipe din aktivitetsloggen till Siem från tredje part och logga Analyslösningar.
* **Skapa en anpassad telemetri och loggning plattform** – om du redan har en specialbyggt telemetri plattform eller är bara du tänker skapa en mycket skalbar hello publicera och prenumerera uppbyggnad Händelsehubbar kan du tooflexibly mata in hello aktivitetsloggen. [Se Dan Rosanova guide toousing Händelsehubbar i en global skala telemetri platform här.](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/)

## <a name="enable-streaming-of-hello-activity-log"></a>Strömning av hello aktivitetsloggen
Du kan aktivera strömning av hello aktivitetsloggen programmässigt eller via hello-portalen. Oavsett hur du väljer en Service Bus-Namespace och en princip för delad åtkomst för det namnområdet och en Händelsehubb skapas i namnutrymmet när hello första nya aktivitetsloggen händelse inträffar. Om du inte har en Service Bus-Namespace måste du först toocreate en. Om du har tidigare strömmas aktivitetsloggen händelser toothis Service Bus Namespace, kan hello Event Hub som skapades tidigare återanvändas. hello delad åtkomstprincip definierar hello behörigheter som har hello strömmande mekanism. Idag strömning tooan Händelsehubbar kräver **hantera**, **skicka**, och **lyssna** behörigheter. Du kan skapa eller ändra principer för Service Bus Namespace delad åtkomst i hello klassiska portalen hello ”konfigurera” fliken för ditt Service Bus-Namespace. tooupdate Hej aktivitetsloggen loggen profil tooinclude strömning, hello användaren hello ändringen måste ha hello ListKey behörighet på den Service Bus-auktoriseringsregeln.

hello service bus eller händelse hubb namnrymd har inte toobe i hello samma prenumeration som hello prenumeration avger loggar så länge hello användare som konfigurerar hello inställningen har lämplig RBAC åtkomst tooboth prenumerationer.

### <a name="via-azure-portal"></a>Via Azure-portalen
1. Navigera toohello **aktivitetsloggen** blad med hjälp av hello-menyn på vänster sida av hello portal hello.
   
    ![Navigera tooActivity logg i portalen](./media/monitoring-overview-activity-logs/activity-logs-portal-navigate.png)
2. Klicka på hello **exportera** knappen hello överst i hello-bladet.
   
    ![Exportera-knappen i portalen](./media/monitoring-overview-activity-logs/activity-logs-portal-export.png)
3. Du kan välja hello regioner som du vill att toostream händelser och hello Service Bus Namespace som du vill att en Händelsehubb toobe som skapats för direktuppspelning av dessa händelser i hello-bladet som visas.
   
    ![Exportera aktivitetsloggen bladet](./media/monitoring-overview-activity-logs/activity-logs-portal-export-blade.png)
4. Klicka på **spara** toosave inställningarna. hello-inställningarna är omedelbart vara tillämpade tooyour prenumeration.

### <a name="via-powershell-cmdlets"></a>Via PowerShell-Cmdlets
Om det finns redan en logg-profil, måste du först tooremove profilen.

1. Använd `Get-AzureRmLogProfile` tooidentify om det finns en logg-profil
2. I så fall använder `Remove-AzureRmLogProfile` tooremove den.
3. Använd `Set-AzureRmLogProfile` toocreate en profil:

```
Add-AzureRmLogProfile -Name my_log_profile -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Locations global,westus,eastus -RetentionInDays 90 -Categories Write,Delete,Action
```

hello regel-ID för Service Bus är en sträng med formatet: {service bus-resurs-ID} /authorizationrules/ {namn}, till exempel 

### <a name="via-azure-cli"></a>Via Azure CLI
Om det finns redan en logg-profil, måste du först tooremove profilen.

1. Använd `azure insights logprofile list` tooidentify om det finns en logg-profil
2. I så fall använder `azure insights logprofile delete` tooremove den.
3. Använd `azure insights logprofile add` toocreate en profil:

```
azure insights logprofile add --name my_log_profile --storageId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/my_storage --serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey --locations global,westus,eastus,northeurope --retentionInDays 90 –categories Write,Delete,Action
```

hello regel-ID för Service Bus är en sträng med formatet: `{service bus resource ID}/authorizationrules/{key name}`.

## <a name="how-do-i-consume-hello-log-data-from-event-hubs"></a>Hur jag för att använda hello loggdata från Event Hubs?
[hello-schemat för hello aktivitetsloggen finns här](monitoring-overview-activity-logs.md). Varje händelse är i en matris av JSON-blobbar som kallas ”poster”.

## <a name="next-steps"></a>Nästa steg
* [Arkivera hello aktivitetsloggen tooa storage-konto](monitoring-archive-activity-log.md)
* [Läs hello översikt över hello Azure-aktivitetsloggen](monitoring-overview-activity-logs.md)
* [Konfigurera en avisering baserat på en aktivitet händelselogg](insights-auditlog-to-webhook-email.md)

