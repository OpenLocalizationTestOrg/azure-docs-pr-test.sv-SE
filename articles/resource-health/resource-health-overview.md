---
title: "Översikt av Azure Resource health | Microsoft Docs"
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
ms.openlocfilehash: d54979995ca97a70ba92c64915b919da09f548ec
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-resource-health-overview"></a><span data-ttu-id="7e80f-103">Översikt över Azure-resurs-hälsa</span><span class="sxs-lookup"><span data-stu-id="7e80f-103">Azure resource health overview</span></span>
 
<span data-ttu-id="7e80f-104">Resource Health hjälper dig att diagnostisera och få support när ett problem med Azure påverkar dina resurser.</span><span class="sxs-lookup"><span data-stu-id="7e80f-104">Resource health helps you diagnose and get support when an Azure issue impacts your resources.</span></span> <span data-ttu-id="7e80f-105">Det informerar dig om det aktuella och tidigare hälsotillståndet för dina resurser och hjälper dig att åtgärda problem.</span><span class="sxs-lookup"><span data-stu-id="7e80f-105">It informs you about the current and past health of your resources and helps you mitigate issues.</span></span> <span data-ttu-id="7e80f-106">Resource Health ger teknisk support när du behöver hjälp med problem med Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="7e80f-106">Resource health provides technical support when you need help with Azure service issues.</span></span>

<span data-ttu-id="7e80f-107">Medan [Azure Status](https://status.azure.com) informerar om tjänsten problem som påverkar en bred uppsättning Azure-kunder, resurshälsa ger dig en anpassad instrumentpanel för hälsotillståndet för dina resurser.</span><span class="sxs-lookup"><span data-stu-id="7e80f-107">Whereas [Azure Status](https://status.azure.com) informs you about service issues that affect a broad set of Azure customers, resource health provides you with a personalized dashboard of the health of your resources.</span></span> <span data-ttu-id="7e80f-108">Resurshälsa visar alltid resurserna var inte tillgängliga i förflutna på grund av problem med Azure-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="7e80f-108">Resource health shows you all the times your resources were unavailable in the past due to Azure service issues.</span></span> <span data-ttu-id="7e80f-109">Detta gör det enkelt för dig att förstå om ett SLA har överskridits.</span><span class="sxs-lookup"><span data-stu-id="7e80f-109">This makes it simple for you to understand if an SLA was violated.</span></span> 

## <a name="what-is-considered-a-resource-and-how-does-resource-health-decides-if-a-resource-is-healthy-or-not"></a><span data-ttu-id="7e80f-110">Vad som anses en resurs och hur fungerar resurshälsa bestämmer om en resurs är felfri?</span><span class="sxs-lookup"><span data-stu-id="7e80f-110">What is considered a resource and how does resource health decides if a resource is healthy or not?</span></span>
<span data-ttu-id="7e80f-111">En resurs är en instans av en resurstyp som erbjuds av en Azure-tjänst via Azure Resource Manager, till exempel: en virtuell dator, ett webbprogram eller en SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="7e80f-111">A resource is an instance of a resource type offered by an Azure service through Azure Resource Manager, for example: a virtual machine, a web app, or a SQL database.</span></span>

<span data-ttu-id="7e80f-112">Resurshälsa är beroende av signaler sänds av olika Azure-tjänster för att bedöma om en resurs är felfri eller inte.</span><span class="sxs-lookup"><span data-stu-id="7e80f-112">Resource health relies on signals emitted by the different Azure services to assess if a resource is healthy or not.</span></span> <span data-ttu-id="7e80f-113">Om en resurs är i feltillstånd, analyserar resurshälsa ytterligare information för att fastställa orsaken till problemet.</span><span class="sxs-lookup"><span data-stu-id="7e80f-113">If a resource is unhealthy, resource health analyzes additional information to determine the source of the problem.</span></span> <span data-ttu-id="7e80f-114">Den visar även de åtgärder som Microsoft tar för att åtgärda problemet eller vilka åtgärder du kan vidta för att åtgärda orsaken till problemet.</span><span class="sxs-lookup"><span data-stu-id="7e80f-114">It also identifies actions Microsoft is taking to fix the issue or what actions you can take to address the cause of the problem.</span></span> 

<span data-ttu-id="7e80f-115">Läs den fullständiga listan över resurstyper och kontroll [Azure resource health](resource-health-checks-resource-types.md) för mer information om hur hälsa utvärderas.</span><span class="sxs-lookup"><span data-stu-id="7e80f-115">Review the full list of resource types and health checks in [Azure resource health](resource-health-checks-resource-types.md) for additional details on how health is assessed.</span></span>

## <a name="health-status-provided-by-resource-health"></a><span data-ttu-id="7e80f-116">Hälsostatus som tillhandahålls av resurshälsa</span><span class="sxs-lookup"><span data-stu-id="7e80f-116">Health status provided by resource health</span></span>
<span data-ttu-id="7e80f-117">Hälsotillståndet för en resurs är en av följande status:</span><span class="sxs-lookup"><span data-stu-id="7e80f-117">The health of a resource is one of the following statuses:</span></span>

### <a name="available"></a><span data-ttu-id="7e80f-118">Tillgänglig</span><span class="sxs-lookup"><span data-stu-id="7e80f-118">Available</span></span>
<span data-ttu-id="7e80f-119">Tjänsten har inte identifierat eventuella händelser som påverkar hälsotillståndet för resursen.</span><span class="sxs-lookup"><span data-stu-id="7e80f-119">The service has not detected any events impacting the health of the resource.</span></span> <span data-ttu-id="7e80f-120">I fall där resursen har återställts från oplanerade driftsavbrott under de senaste 24 timmarna visas den **har nyligen återställts** meddelande.</span><span class="sxs-lookup"><span data-stu-id="7e80f-120">In cases where the resource has recovered from unplanned downtime during the last 24 hours you will see the **recently recovered** notification.</span></span>

![Resource health tillgänglig virtuell dator](./media/resource-health-overview/Available.png)

### <a name="unavailable"></a><span data-ttu-id="7e80f-122">Inte tillgänglig</span><span class="sxs-lookup"><span data-stu-id="7e80f-122">Unavailable</span></span>
<span data-ttu-id="7e80f-123">Tjänsten har upptäckt en pågående plattform eller icke-plattform händelse som påverkar hälsotillståndet för resursen.</span><span class="sxs-lookup"><span data-stu-id="7e80f-123">The service has detected an ongoing platform or non-platform event impacting the health of the resource.</span></span>

#### <a name="platform-events"></a><span data-ttu-id="7e80f-124">Plattform händelser</span><span class="sxs-lookup"><span data-stu-id="7e80f-124">Platform events</span></span>
<span data-ttu-id="7e80f-125">Dessa händelser har utlösts av flera komponenter i Azure-infrastrukturen och inkluderar både schemalagda åtgärder som planerat underhåll och oväntat incidenter som en oplanerad värden omstart.</span><span class="sxs-lookup"><span data-stu-id="7e80f-125">These events are triggered by multiple components of the Azure infrastructure and include both scheduled actions like planned maintenance and unexpected incidents like an unplanned host reboot.</span></span>

<span data-ttu-id="7e80f-126">Resurshälsa innehåller ytterligare information om händelsen, återställningsprocessen och kan du kontakta supporten, även om du inte har en aktiv Microsoft supportavtal.</span><span class="sxs-lookup"><span data-stu-id="7e80f-126">Resource health provides additional details on the event, the recovery process and enables you to contact support even if you don't have an active Microsoft support agreement.</span></span>

![Resource health tillgänglig virtuell dator på grund av plattformshändelsen](./media/resource-health-overview/Unavailable.png)

#### <a name="non-platform-events"></a><span data-ttu-id="7e80f-128">Icke-plattform händelser</span><span class="sxs-lookup"><span data-stu-id="7e80f-128">Non-Platform events</span></span>
<span data-ttu-id="7e80f-129">Dessa händelser som utlöses av åtgärder som vidtas av användare, till exempel stoppar en virtuell dator eller når det maximala antalet anslutningar till ett Redis-Cache.</span><span class="sxs-lookup"><span data-stu-id="7e80f-129">These events are triggered by actions taken by users, for example stopping a virtual machine or reaching the maximum number of connections to a Redis Cache.</span></span>

![Resource health tillgänglig virtuella på grund av icke-plattformshändelsen](./media/resource-health-overview/Unavailable_NonPlatform.png)

### <a name="unknown"></a><span data-ttu-id="7e80f-131">Okänd</span><span class="sxs-lookup"><span data-stu-id="7e80f-131">Unknown</span></span>
<span data-ttu-id="7e80f-132">Den här hälsostatus anger att resurshälsa inte har tagit emot information om den här resursen för mer än 10 minuter.</span><span class="sxs-lookup"><span data-stu-id="7e80f-132">This health status indicates that resource health has not received information about this resource for more than 10 minutes.</span></span> <span data-ttu-id="7e80f-133">Denna status inte är en slutgiltig angivelse av resursens tillstånd, men det är ett viktigt datapunkten i felsökningen:</span><span class="sxs-lookup"><span data-stu-id="7e80f-133">While this status is not a definitive indication of the state of the resource, it is an important data point in the troubleshooting process:</span></span>
* <span data-ttu-id="7e80f-134">Om resursen körs som förväntat status för resursen uppdateras till tillgänglig efter några minuter.</span><span class="sxs-lookup"><span data-stu-id="7e80f-134">If the resource is running as expected the status of the resource will update to Available after a few minutes.</span></span>
* <span data-ttu-id="7e80f-135">Om du har problem med resursen som kan okänd hälsostatus föreslå resursen påverkas av en händelse i plattformen.</span><span class="sxs-lookup"><span data-stu-id="7e80f-135">If you are experiencing problems with the resource, the Unknown health status may suggest the resource is impacted by an event in the platform.</span></span>

![Resource health okänd virtuell dator](./media/resource-health-overview/Unknown.png)

## <a name="report-an-incorrect-status"></a><span data-ttu-id="7e80f-137">Rapportera felaktig status</span><span class="sxs-lookup"><span data-stu-id="7e80f-137">Report an incorrect status</span></span>
<span data-ttu-id="7e80f-138">Om du tror aktuella hälsostatus är felaktig för när som helst, du kan berätta för oss genom att klicka på **Rapportera felaktig hälsostatus**.</span><span class="sxs-lookup"><span data-stu-id="7e80f-138">If at any point you believe the current health status is incorrect, you can let us know by clicking **Report incorrect health status**.</span></span> <span data-ttu-id="7e80f-139">I fall där du påverkas av ett problem med Azure kan rekommenderar vi att du kontakta support från resursbladet för hälsotillstånd.</span><span class="sxs-lookup"><span data-stu-id="7e80f-139">In cases where you are impacted by an Azure problem, we encourage you to contact support from the resource health blade.</span></span> 

![Resurshälsa rapporten felaktig Status](./media/resource-health-overview/incorrect-status.png)

## <a name="historical-information"></a><span data-ttu-id="7e80f-141">Historisk Information</span><span class="sxs-lookup"><span data-stu-id="7e80f-141">Historical Information</span></span>
<span data-ttu-id="7e80f-142">Du kan komma åt upp till 14 dagar historiska hälsa data genom att klicka på **Visa historik över** i resursbladet för hälsotillstånd.</span><span class="sxs-lookup"><span data-stu-id="7e80f-142">You can access up to 14 days of historical health data by clicking **View History** in the Resource health blade.</span></span> 

![Resource health rapporthistorik](./media/resource-health-overview/history-blade.png)

## <a name="getting-started"></a><span data-ttu-id="7e80f-144">Komma igång</span><span class="sxs-lookup"><span data-stu-id="7e80f-144">Getting started</span></span>
<span data-ttu-id="7e80f-145">Öppna resursen hälsan för en resurs</span><span class="sxs-lookup"><span data-stu-id="7e80f-145">To open Resource health for one resource</span></span>
1.  <span data-ttu-id="7e80f-146">Logga in på Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="7e80f-146">Sign in into the Azure portal.</span></span>
2.  <span data-ttu-id="7e80f-147">Gå till din resurs.</span><span class="sxs-lookup"><span data-stu-id="7e80f-147">Navigate to your resource.</span></span>
3.  <span data-ttu-id="7e80f-148">I menyn resursen finns i den vänstra sidan klickar du på **Resource health**.</span><span class="sxs-lookup"><span data-stu-id="7e80f-148">In the resource menu located in the left-hand side, click **Resource health**.</span></span>

![Öppna Resource health från resursbladet](./media/resource-health-overview/from-resource-blade.png)

<span data-ttu-id="7e80f-150">Du kan också komma åt resurshälsa genom att klicka på **fler tjänster**, och skriva **resurshälsa** i filterrutan att öppna den **hjälp + stöd** bladet.</span><span class="sxs-lookup"><span data-stu-id="7e80f-150">You can also access resource health by clicking **More services**, and typing **resource health** in filter text box to open the **Help + Support** blade.</span></span> <span data-ttu-id="7e80f-151">Klicka slutligen på [ **resurshälsa**](https://ms.portal.azure.com/#blade/Microsoft_Azure_Monitoring/AzureMonitoringBrowseBlade/resourceHealth).</span><span class="sxs-lookup"><span data-stu-id="7e80f-151">Finally click [**Resource health**](https://ms.portal.azure.com/#blade/Microsoft_Azure_Monitoring/AzureMonitoringBrowseBlade/resourceHealth).</span></span>

![Öppna Resource health från flera service](./media/resource-health-overview/FromOtherServices.png)

## <a name="next-steps"></a><span data-ttu-id="7e80f-153">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7e80f-153">Next steps</span></span>

<span data-ttu-id="7e80f-154">Checka ut dessa resurser om du vill veta mer om resurshälsa:</span><span class="sxs-lookup"><span data-stu-id="7e80f-154">Check out these resources to learn more about resource health:</span></span>
-  [<span data-ttu-id="7e80f-155">Hälsa och resurstyper kontrollerar i Azure resurshälsa</span><span class="sxs-lookup"><span data-stu-id="7e80f-155">Resource types and health checks in Azure resource health</span></span>](resource-health-checks-resource-types.md)
-  [<span data-ttu-id="7e80f-156">Vanliga frågor och svar om Azure-resurs hälsotillstånd</span><span class="sxs-lookup"><span data-stu-id="7e80f-156">Frequently asked questions about Azure resource health</span></span>](resource-health-faq.md)




