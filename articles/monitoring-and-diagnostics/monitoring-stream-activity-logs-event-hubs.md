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
# <a name="stream-hello-azure-activity-log-tooevent-hubs"></a><span data-ttu-id="2efb6-103">Strömma hello Azure-aktivitetsloggen tooEvent hubbar</span><span class="sxs-lookup"><span data-stu-id="2efb6-103">Stream hello Azure Activity Log tooEvent Hubs</span></span>
<span data-ttu-id="2efb6-104">Hej [ **Azure-aktivitetsloggen** ](monitoring-overview-activity-logs.md) kan strömmas i nära realtid tooany program med hjälp av hello inbyggda ”Export” alternativet hello-portalen eller genom att aktivera hello Service Bus regel-Id i en logg profil via hello Azure PowerShell-Cmdlets eller Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="2efb6-104">hello [**Azure Activity Log**](monitoring-overview-activity-logs.md) can be streamed in near real time tooany application using hello built-in “Export” option in hello portal, or by enabling hello Service Bus Rule Id in a Log Profile via hello Azure PowerShell Cmdlets or Azure CLI.</span></span>

## <a name="what-you-can-do-with-hello-activity-log-and-event-hubs"></a><span data-ttu-id="2efb6-105">Vad du kan göra med hello aktivitetsloggen och Händelsehubbar</span><span class="sxs-lookup"><span data-stu-id="2efb6-105">What you can do with hello Activity Log and Event Hubs</span></span>
<span data-ttu-id="2efb6-106">Här följer några olika sätt som du kan använda hello strömning möjligheten för hello aktivitetsloggen:</span><span class="sxs-lookup"><span data-stu-id="2efb6-106">Here are just a few ways you might use hello streaming capability for hello Activity Log:</span></span>

* <span data-ttu-id="2efb6-107">**Strömma toothird loggning och telemetri part** – över tiden, Händelsehubbar strömning ska bli hello mekanism toopipe din aktivitetsloggen till Siem från tredje part och logga Analyslösningar.</span><span class="sxs-lookup"><span data-stu-id="2efb6-107">**Stream toothird-party logging and telemetry systems** – Over time, Event Hubs streaming will become hello mechanism toopipe your Activity Log into third-party SIEMs and log analytics solutions.</span></span>
* <span data-ttu-id="2efb6-108">**Skapa en anpassad telemetri och loggning plattform** – om du redan har en specialbyggt telemetri plattform eller är bara du tänker skapa en mycket skalbar hello publicera och prenumerera uppbyggnad Händelsehubbar kan du tooflexibly mata in hello aktivitetsloggen.</span><span class="sxs-lookup"><span data-stu-id="2efb6-108">**Build a custom telemetry and logging platform** – If you already have a custom-built telemetry platform or are just thinking about building one, hello highly scalable publish-subscribe nature of Event Hubs allows you tooflexibly ingest hello activity log.</span></span> [<span data-ttu-id="2efb6-109">Se Dan Rosanova guide toousing Händelsehubbar i en global skala telemetri platform här.</span><span class="sxs-lookup"><span data-stu-id="2efb6-109">See Dan Rosanova’s guide toousing Event Hubs in a global scale telemetry platform here.</span></span>](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/)

## <a name="enable-streaming-of-hello-activity-log"></a><span data-ttu-id="2efb6-110">Strömning av hello aktivitetsloggen</span><span class="sxs-lookup"><span data-stu-id="2efb6-110">Enable streaming of hello Activity Log</span></span>
<span data-ttu-id="2efb6-111">Du kan aktivera strömning av hello aktivitetsloggen programmässigt eller via hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="2efb6-111">You can enable streaming of hello Activity Log either programmatically or via hello portal.</span></span> <span data-ttu-id="2efb6-112">Oavsett hur du väljer en Service Bus-Namespace och en princip för delad åtkomst för det namnområdet och en Händelsehubb skapas i namnutrymmet när hello första nya aktivitetsloggen händelse inträffar.</span><span class="sxs-lookup"><span data-stu-id="2efb6-112">Either way, you pick a Service Bus Namespace and a shared access policy for that namespace, and an Event Hub is created in that namespace when hello first new Activity Log event occurs.</span></span> <span data-ttu-id="2efb6-113">Om du inte har en Service Bus-Namespace måste du först toocreate en.</span><span class="sxs-lookup"><span data-stu-id="2efb6-113">If you do not have a Service Bus Namespace, you first need toocreate one.</span></span> <span data-ttu-id="2efb6-114">Om du har tidigare strömmas aktivitetsloggen händelser toothis Service Bus Namespace, kan hello Event Hub som skapades tidigare återanvändas.</span><span class="sxs-lookup"><span data-stu-id="2efb6-114">If you have previously streamed Activity Log events toothis Service Bus Namespace, hello Event Hub that was previously created will be reused.</span></span> <span data-ttu-id="2efb6-115">hello delad åtkomstprincip definierar hello behörigheter som har hello strömmande mekanism.</span><span class="sxs-lookup"><span data-stu-id="2efb6-115">hello shared access policy defines hello permissions that hello streaming mechanism has.</span></span> <span data-ttu-id="2efb6-116">Idag strömning tooan Händelsehubbar kräver **hantera**, **skicka**, och **lyssna** behörigheter.</span><span class="sxs-lookup"><span data-stu-id="2efb6-116">Today, streaming tooan Event Hubs requires **Manage**, **Send**, and **Listen** permissions.</span></span> <span data-ttu-id="2efb6-117">Du kan skapa eller ändra principer för Service Bus Namespace delad åtkomst i hello klassiska portalen hello ”konfigurera” fliken för ditt Service Bus-Namespace.</span><span class="sxs-lookup"><span data-stu-id="2efb6-117">You can create or modify Service Bus Namespace shared access policies in hello classic portal under hello “Configure” tab for your Service Bus Namespace.</span></span> <span data-ttu-id="2efb6-118">tooupdate Hej aktivitetsloggen loggen profil tooinclude strömning, hello användaren hello ändringen måste ha hello ListKey behörighet på den Service Bus-auktoriseringsregeln.</span><span class="sxs-lookup"><span data-stu-id="2efb6-118">tooupdate hello Activity Log log profile tooinclude streaming, hello user making hello change must have hello ListKey permission on that Service Bus Authorization Rule.</span></span>

<span data-ttu-id="2efb6-119">hello service bus eller händelse hubb namnrymd har inte toobe i hello samma prenumeration som hello prenumeration avger loggar så länge hello användare som konfigurerar hello inställningen har lämplig RBAC åtkomst tooboth prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="2efb6-119">hello service bus or event hub namespace does not have toobe in hello same subscription as hello subscription emitting logs as long as hello user who configures hello setting has appropriate RBAC access tooboth subscriptions.</span></span>

### <a name="via-azure-portal"></a><span data-ttu-id="2efb6-120">Via Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="2efb6-120">Via Azure portal</span></span>
1. <span data-ttu-id="2efb6-121">Navigera toohello **aktivitetsloggen** blad med hjälp av hello-menyn på vänster sida av hello portal hello.</span><span class="sxs-lookup"><span data-stu-id="2efb6-121">Navigate toohello **Activity Log** blade using hello menu on hello left side of hello portal.</span></span>
   
    ![Navigera tooActivity logg i portalen](./media/monitoring-overview-activity-logs/activity-logs-portal-navigate.png)
2. <span data-ttu-id="2efb6-123">Klicka på hello **exportera** knappen hello överst i hello-bladet.</span><span class="sxs-lookup"><span data-stu-id="2efb6-123">Click hello **Export** button at hello top of hello blade.</span></span>
   
    ![Exportera-knappen i portalen](./media/monitoring-overview-activity-logs/activity-logs-portal-export.png)
3. <span data-ttu-id="2efb6-125">Du kan välja hello regioner som du vill att toostream händelser och hello Service Bus Namespace som du vill att en Händelsehubb toobe som skapats för direktuppspelning av dessa händelser i hello-bladet som visas.</span><span class="sxs-lookup"><span data-stu-id="2efb6-125">In hello blade that appears, you can select hello regions for which you would like toostream events and hello Service Bus Namespace in which you would like an Event Hub toobe created for streaming these events.</span></span>
   
    ![Exportera aktivitetsloggen bladet](./media/monitoring-overview-activity-logs/activity-logs-portal-export-blade.png)
4. <span data-ttu-id="2efb6-127">Klicka på **spara** toosave inställningarna.</span><span class="sxs-lookup"><span data-stu-id="2efb6-127">Click **Save** toosave these settings.</span></span> <span data-ttu-id="2efb6-128">hello-inställningarna är omedelbart vara tillämpade tooyour prenumeration.</span><span class="sxs-lookup"><span data-stu-id="2efb6-128">hello settings are immediately be applied tooyour subscription.</span></span>

### <a name="via-powershell-cmdlets"></a><span data-ttu-id="2efb6-129">Via PowerShell-Cmdlets</span><span class="sxs-lookup"><span data-stu-id="2efb6-129">Via PowerShell Cmdlets</span></span>
<span data-ttu-id="2efb6-130">Om det finns redan en logg-profil, måste du först tooremove profilen.</span><span class="sxs-lookup"><span data-stu-id="2efb6-130">If a log profile already exists, you first need tooremove that profile.</span></span>

1. <span data-ttu-id="2efb6-131">Använd `Get-AzureRmLogProfile` tooidentify om det finns en logg-profil</span><span class="sxs-lookup"><span data-stu-id="2efb6-131">Use `Get-AzureRmLogProfile` tooidentify if a log profile exists</span></span>
2. <span data-ttu-id="2efb6-132">I så fall använder `Remove-AzureRmLogProfile` tooremove den.</span><span class="sxs-lookup"><span data-stu-id="2efb6-132">If so, use `Remove-AzureRmLogProfile` tooremove it.</span></span>
3. <span data-ttu-id="2efb6-133">Använd `Set-AzureRmLogProfile` toocreate en profil:</span><span class="sxs-lookup"><span data-stu-id="2efb6-133">Use `Set-AzureRmLogProfile` toocreate a profile:</span></span>

```
Add-AzureRmLogProfile -Name my_log_profile -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Locations global,westus,eastus -RetentionInDays 90 -Categories Write,Delete,Action
```

<span data-ttu-id="2efb6-134">hello regel-ID för Service Bus är en sträng med formatet: {service bus-resurs-ID} /authorizationrules/ {namn}, till exempel</span><span class="sxs-lookup"><span data-stu-id="2efb6-134">hello Service Bus Rule ID is a string with this format: {service bus resource ID}/authorizationrules/{key name}, for example</span></span> 

### <a name="via-azure-cli"></a><span data-ttu-id="2efb6-135">Via Azure CLI</span><span class="sxs-lookup"><span data-stu-id="2efb6-135">Via Azure CLI</span></span>
<span data-ttu-id="2efb6-136">Om det finns redan en logg-profil, måste du först tooremove profilen.</span><span class="sxs-lookup"><span data-stu-id="2efb6-136">If a log profile already exists, you first need tooremove that profile.</span></span>

1. <span data-ttu-id="2efb6-137">Använd `azure insights logprofile list` tooidentify om det finns en logg-profil</span><span class="sxs-lookup"><span data-stu-id="2efb6-137">Use `azure insights logprofile list` tooidentify if a log profile exists</span></span>
2. <span data-ttu-id="2efb6-138">I så fall använder `azure insights logprofile delete` tooremove den.</span><span class="sxs-lookup"><span data-stu-id="2efb6-138">If so, use `azure insights logprofile delete` tooremove it.</span></span>
3. <span data-ttu-id="2efb6-139">Använd `azure insights logprofile add` toocreate en profil:</span><span class="sxs-lookup"><span data-stu-id="2efb6-139">Use `azure insights logprofile add` toocreate a profile:</span></span>

```
azure insights logprofile add --name my_log_profile --storageId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/my_storage --serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey --locations global,westus,eastus,northeurope --retentionInDays 90 –categories Write,Delete,Action
```

<span data-ttu-id="2efb6-140">hello regel-ID för Service Bus är en sträng med formatet: `{service bus resource ID}/authorizationrules/{key name}`.</span><span class="sxs-lookup"><span data-stu-id="2efb6-140">hello Service Bus Rule ID is a string with this format: `{service bus resource ID}/authorizationrules/{key name}`.</span></span>

## <a name="how-do-i-consume-hello-log-data-from-event-hubs"></a><span data-ttu-id="2efb6-141">Hur jag för att använda hello loggdata från Event Hubs?</span><span class="sxs-lookup"><span data-stu-id="2efb6-141">How do I consume hello log data from Event Hubs?</span></span>
<span data-ttu-id="2efb6-142">[hello-schemat för hello aktivitetsloggen finns här](monitoring-overview-activity-logs.md).</span><span class="sxs-lookup"><span data-stu-id="2efb6-142">[hello schema for hello Activity Log is available here](monitoring-overview-activity-logs.md).</span></span> <span data-ttu-id="2efb6-143">Varje händelse är i en matris av JSON-blobbar som kallas ”poster”.</span><span class="sxs-lookup"><span data-stu-id="2efb6-143">Each event is in an array of JSON blobs called “records.”</span></span>

## <a name="next-steps"></a><span data-ttu-id="2efb6-144">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2efb6-144">Next Steps</span></span>
* [<span data-ttu-id="2efb6-145">Arkivera hello aktivitetsloggen tooa storage-konto</span><span class="sxs-lookup"><span data-stu-id="2efb6-145">Archive hello Activity Log tooa storage account</span></span>](monitoring-archive-activity-log.md)
* [<span data-ttu-id="2efb6-146">Läs hello översikt över hello Azure-aktivitetsloggen</span><span class="sxs-lookup"><span data-stu-id="2efb6-146">Read hello overview of hello Azure Activity Log</span></span>](monitoring-overview-activity-logs.md)
* [<span data-ttu-id="2efb6-147">Konfigurera en avisering baserat på en aktivitet händelselogg</span><span class="sxs-lookup"><span data-stu-id="2efb6-147">Set up an alert based on an Activity Log event</span></span>](insights-auditlog-to-webhook-email.md)

