---
title: "aaaAzure Resource health översikt | Microsoft Docs"
description: "Översikt över Azure Resource health"
services: Resource health
documentationcenter: 
author: BernardoAMunoz
manager: 
editor: 
ms.assetid: 85cc88a4-80fd-4b9b-a30a-34ff3782855f
ms.service: resource-health
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Supportability
ms.date: 06/01/2016
ms.author: BernardoAMunoz
ms.openlocfilehash: b920241b2f64a0695ba2c32e9ccb0c2c3739986d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-resource-health-overview"></a><span data-ttu-id="e0354-103">Översikt över Azure-resurs-hälsa</span><span class="sxs-lookup"><span data-stu-id="e0354-103">Azure resource health overview</span></span>
 
<span data-ttu-id="e0354-104">Resource Health hjälper dig att diagnostisera och få support när ett problem med Azure påverkar dina resurser.</span><span class="sxs-lookup"><span data-stu-id="e0354-104">Resource health helps you diagnose and get support when an Azure issue impacts your resources.</span></span> <span data-ttu-id="e0354-105">Det informerar om hello aktuella och tidigare dina resursers hälsa och hjälper dig att minska problem.</span><span class="sxs-lookup"><span data-stu-id="e0354-105">It informs you about hello current and past health of your resources and helps you mitigate issues.</span></span> <span data-ttu-id="e0354-106">Resource Health ger teknisk support när du behöver hjälp med problem med Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="e0354-106">Resource health provides technical support when you need help with Azure service issues.</span></span>

<span data-ttu-id="e0354-107">Medan [Azure Status](https://status.azure.com) informerar om tjänsten problem som påverkar en bred uppsättning Azure-kunder, resurshälsa ger dig en anpassad instrumentpanel hello hälsotillståndet hos dina resurser.</span><span class="sxs-lookup"><span data-stu-id="e0354-107">Whereas [Azure Status](https://status.azure.com) informs you about service issues that affect a broad set of Azure customers, resource health provides you with a personalized dashboard of hello health of your resources.</span></span> <span data-ttu-id="e0354-108">Resurshälsa visar hello alltid resurserna var inte tillgängliga i hello förfallit till betalning tooAzure problem med tjänsten.</span><span class="sxs-lookup"><span data-stu-id="e0354-108">Resource health shows you all hello times your resources were unavailable in hello past due tooAzure service issues.</span></span> <span data-ttu-id="e0354-109">Detta gör det enkelt för du toounderstand om ett SLA har överskridits.</span><span class="sxs-lookup"><span data-stu-id="e0354-109">This makes it simple for you toounderstand if an SLA was violated.</span></span> 

## <a name="what-is-considered-a-resource-and-how-does-resource-health-decides-if-a-resource-is-healthy-or-not"></a><span data-ttu-id="e0354-110">Vad som anses en resurs och hur fungerar resurshälsa bestämmer om en resurs är felfri?</span><span class="sxs-lookup"><span data-stu-id="e0354-110">What is considered a resource and how does resource health decides if a resource is healthy or not?</span></span>
<span data-ttu-id="e0354-111">En resurs är en instans av en resurstyp som erbjuds av en Azure-tjänst via Azure Resource Manager, till exempel: en virtuell dator, ett webbprogram eller en SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="e0354-111">A resource is an instance of a resource type offered by an Azure service through Azure Resource Manager, for example: a virtual machine, a web app, or a SQL database.</span></span>

<span data-ttu-id="e0354-112">Resurshälsa är beroende av signaler sänds av hello olika Azure-tjänster tooassess om en resurs är felfri eller inte.</span><span class="sxs-lookup"><span data-stu-id="e0354-112">Resource health relies on signals emitted by hello different Azure services tooassess if a resource is healthy or not.</span></span> <span data-ttu-id="e0354-113">Om en resurs är i feltillstånd, analyserar resurshälsa ytterligare information toodetermine hello källan hello problemet.</span><span class="sxs-lookup"><span data-stu-id="e0354-113">If a resource is unhealthy, resource health analyzes additional information toodetermine hello source of hello problem.</span></span> <span data-ttu-id="e0354-114">Den visar även de åtgärder som Microsoft tar toofix hello problem eller vad du kan göra tooaddress hello orsaka hello problemet.</span><span class="sxs-lookup"><span data-stu-id="e0354-114">It also identifies actions Microsoft is taking toofix hello issue or what actions you can take tooaddress hello cause of hello problem.</span></span> 

<span data-ttu-id="e0354-115">Granska hello fullständig lista över resurstyper och hälsa checkar in [Azure resource health](resource-health-checks-resource-types.md) för mer information om hur hälsa utvärderas.</span><span class="sxs-lookup"><span data-stu-id="e0354-115">Review hello full list of resource types and health checks in [Azure resource health](resource-health-checks-resource-types.md) for additional details on how health is assessed.</span></span>

## <a name="health-status-provided-by-resource-health"></a><span data-ttu-id="e0354-116">Hälsostatus som tillhandahålls av resurshälsa</span><span class="sxs-lookup"><span data-stu-id="e0354-116">Health status provided by resource health</span></span>
<span data-ttu-id="e0354-117">hello hälsotillståndet för en resurs är en av följande statusar hello:</span><span class="sxs-lookup"><span data-stu-id="e0354-117">hello health of a resource is one of hello following statuses:</span></span>

### <a name="available"></a><span data-ttu-id="e0354-118">Tillgänglig</span><span class="sxs-lookup"><span data-stu-id="e0354-118">Available</span></span>
<span data-ttu-id="e0354-119">hello-tjänsten har inte identifierat eventuella händelser som påverkar hello hälsotillstånd hello resurs.</span><span class="sxs-lookup"><span data-stu-id="e0354-119">hello service has not detected any events impacting hello health of hello resource.</span></span> <span data-ttu-id="e0354-120">I fall där hello resurs har återställts från oplanerade driftsavbrott under hello senaste 24 timmarna visas hello **har nyligen återställts** meddelande.</span><span class="sxs-lookup"><span data-stu-id="e0354-120">In cases where hello resource has recovered from unplanned downtime during hello last 24 hours you will see hello **recently recovered** notification.</span></span>

![Resource health tillgänglig virtuell dator](./media/resource-health-overview/Available.png)

### <a name="unavailable"></a><span data-ttu-id="e0354-122">Inte tillgänglig</span><span class="sxs-lookup"><span data-stu-id="e0354-122">Unavailable</span></span>
<span data-ttu-id="e0354-123">hello-tjänsten har upptäckt en pågående plattform eller -plattform händelse som påverkar hello hälsotillstånd hello resurs.</span><span class="sxs-lookup"><span data-stu-id="e0354-123">hello service has detected an ongoing platform or non-platform event impacting hello health of hello resource.</span></span>

#### <a name="platform-events"></a><span data-ttu-id="e0354-124">Plattform händelser</span><span class="sxs-lookup"><span data-stu-id="e0354-124">Platform events</span></span>
<span data-ttu-id="e0354-125">Dessa händelser har utlösts av flera komponenter i hello Azure-infrastrukturen och inkluderar både schemalagda åtgärder som planerat underhåll och oväntat incidenter som en oplanerad värden omstart.</span><span class="sxs-lookup"><span data-stu-id="e0354-125">These events are triggered by multiple components of hello Azure infrastructure and include both scheduled actions like planned maintenance and unexpected incidents like an unplanned host reboot.</span></span>

<span data-ttu-id="e0354-126">Resurshälsa innehåller ytterligare information om hello händelse hello återställningsprocessen och aktiverar du stöd för toocontact även om du inte har en aktiv Microsoft supportavtal.</span><span class="sxs-lookup"><span data-stu-id="e0354-126">Resource health provides additional details on hello event, hello recovery process and enables you toocontact support even if you don't have an active Microsoft support agreement.</span></span>

![Resursen hälsa tillgänglig virtuell dator på grund av tooplatform händelse](./media/resource-health-overview/Unavailable.png)

#### <a name="non-platform-events"></a><span data-ttu-id="e0354-128">Icke-plattform händelser</span><span class="sxs-lookup"><span data-stu-id="e0354-128">Non-Platform events</span></span>
<span data-ttu-id="e0354-129">Dessa händelser som utlöses av åtgärder som vidtas av användare, till exempel stoppar en virtuell dator eller når hello maximalt antal anslutningar tooa Redis-Cache.</span><span class="sxs-lookup"><span data-stu-id="e0354-129">These events are triggered by actions taken by users, for example stopping a virtual machine or reaching hello maximum number of connections tooa Redis Cache.</span></span>

![Resource health tillgänglig virtuella på grund av toonon plattformshändelsen](./media/resource-health-overview/Unavailable_NonPlatform.png)

### <a name="unknown"></a><span data-ttu-id="e0354-131">Okänd</span><span class="sxs-lookup"><span data-stu-id="e0354-131">Unknown</span></span>
<span data-ttu-id="e0354-132">Den här hälsostatus anger att resurshälsa inte har tagit emot information om den här resursen för mer än 10 minuter.</span><span class="sxs-lookup"><span data-stu-id="e0354-132">This health status indicates that resource health has not received information about this resource for more than 10 minutes.</span></span> <span data-ttu-id="e0354-133">Denna status inte är en slutgiltig indikation på hello hello resource tillstånd, men det är ett viktigt datapunkten i hello felsökningsprocessen:</span><span class="sxs-lookup"><span data-stu-id="e0354-133">While this status is not a definitive indication of hello state of hello resource, it is an important data point in hello troubleshooting process:</span></span>
* <span data-ttu-id="e0354-134">Om hello resurs körs som förväntat hello status för hello resurs uppdateras tooAvailable efter några minuter.</span><span class="sxs-lookup"><span data-stu-id="e0354-134">If hello resource is running as expected hello status of hello resource will update tooAvailable after a few minutes.</span></span>
* <span data-ttu-id="e0354-135">Om du har problem med hello resurs kan hello okänd hälsostatus föreslå hello resurs påverkas av en händelse i hello plattform.</span><span class="sxs-lookup"><span data-stu-id="e0354-135">If you are experiencing problems with hello resource, hello Unknown health status may suggest hello resource is impacted by an event in hello platform.</span></span>

![Resource health okänd virtuell dator](./media/resource-health-overview/Unknown.png)

## <a name="report-an-incorrect-status"></a><span data-ttu-id="e0354-137">Rapportera felaktig status</span><span class="sxs-lookup"><span data-stu-id="e0354-137">Report an incorrect status</span></span>
<span data-ttu-id="e0354-138">Om du tror hello aktuella status är felaktigt att när som helst, du kan berätta för oss genom att klicka på **Rapportera felaktig hälsostatus**.</span><span class="sxs-lookup"><span data-stu-id="e0354-138">If at any point you believe hello current health status is incorrect, you can let us know by clicking **Report incorrect health status**.</span></span> <span data-ttu-id="e0354-139">I fall där du påverkas av en Azure problem gärna du toocontact stöd från hello resursbladet hälsa.</span><span class="sxs-lookup"><span data-stu-id="e0354-139">In cases where you are impacted by an Azure problem, we encourage you toocontact support from hello resource health blade.</span></span> 

![Resurshälsa rapporten felaktig Status](./media/resource-health-overview/incorrect-status.png)

## <a name="historical-information"></a><span data-ttu-id="e0354-141">Historisk Information</span><span class="sxs-lookup"><span data-stu-id="e0354-141">Historical Information</span></span>
<span data-ttu-id="e0354-142">Du kan komma åt in too14 dagar historiska hälsa data genom att klicka på **Visa historik över** i hello resursbladet hälsa.</span><span class="sxs-lookup"><span data-stu-id="e0354-142">You can access up too14 days of historical health data by clicking **View History** in hello Resource health blade.</span></span> 

![Resource health rapporthistorik](./media/resource-health-overview/history-blade.png)

## <a name="getting-started"></a><span data-ttu-id="e0354-144">Komma igång</span><span class="sxs-lookup"><span data-stu-id="e0354-144">Getting started</span></span>
<span data-ttu-id="e0354-145">tooopen Resource health för en resurs</span><span class="sxs-lookup"><span data-stu-id="e0354-145">tooopen Resource health for one resource</span></span>
1.  <span data-ttu-id="e0354-146">Logga in till hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="e0354-146">Sign in into hello Azure portal.</span></span>
2.  <span data-ttu-id="e0354-147">Navigera tooyour resurs.</span><span class="sxs-lookup"><span data-stu-id="e0354-147">Navigate tooyour resource.</span></span>
3.  <span data-ttu-id="e0354-148">Resurs-hello menyn finns i hello vänster **Resource health**.</span><span class="sxs-lookup"><span data-stu-id="e0354-148">In hello resource menu located in hello left-hand side, click **Resource health**.</span></span>

![Öppna Resource health från resursbladet](./media/resource-health-overview/from-resource-blade.png)

<span data-ttu-id="e0354-150">Du kan också komma åt resurshälsa genom att klicka på **fler tjänster**, och skriva **resurshälsa** i filtret text rutan tooopen hello **hjälp + stöd** bladet.</span><span class="sxs-lookup"><span data-stu-id="e0354-150">You can also access resource health by clicking **More services**, and typing **resource health** in filter text box tooopen hello **Help + Support** blade.</span></span> <span data-ttu-id="e0354-151">Klicka slutligen på [ **resurshälsa**](https://ms.portal.azure.com/#blade/Microsoft_Azure_Monitoring/AzureMonitoringBrowseBlade/resourceHealth).</span><span class="sxs-lookup"><span data-stu-id="e0354-151">Finally click [**Resource health**](https://ms.portal.azure.com/#blade/Microsoft_Azure_Monitoring/AzureMonitoringBrowseBlade/resourceHealth).</span></span>

![Öppna Resource health från flera service](./media/resource-health-overview/FromOtherServices.png)

## <a name="next-steps"></a><span data-ttu-id="e0354-153">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e0354-153">Next steps</span></span>

<span data-ttu-id="e0354-154">Checka ut dessa resurser toolearn mer om resurshälsa:</span><span class="sxs-lookup"><span data-stu-id="e0354-154">Check out these resources toolearn more about resource health:</span></span>
-  [<span data-ttu-id="e0354-155">Hälsa och resurstyper kontrollerar i Azure resurshälsa</span><span class="sxs-lookup"><span data-stu-id="e0354-155">Resource types and health checks in Azure resource health</span></span>](resource-health-checks-resource-types.md)
-  [<span data-ttu-id="e0354-156">Vanliga frågor och svar om Azure-resurs hälsotillstånd</span><span class="sxs-lookup"><span data-stu-id="e0354-156">Frequently asked questions about Azure resource health</span></span>](resource-health-faq.md)




