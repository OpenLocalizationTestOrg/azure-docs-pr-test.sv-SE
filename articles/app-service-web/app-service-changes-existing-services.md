---
title: "Azure App Service och dess påverkan på befintliga Azure-tjänster"
description: "Beskriver hur den nya Azure App Service och dess funktioner påverkar befintliga tjänster i Azure."
services: app-service
documentationcenter: 
author: yochay
manager: nirma
editor: 
ms.assetid: 86c6a292-3c33-49f4-890c-89cc0321b397
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/12/2016
ms.author: yochaykk
ms.openlocfilehash: ed967fda7a216ed49532be54228ebe888cf16b6f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-app-service-and-existing-azure-services"></a><span data-ttu-id="59e87-103">Azure App Service och befintliga Azure-tjänster</span><span class="sxs-lookup"><span data-stu-id="59e87-103">Azure App Service and existing Azure services</span></span>
<span data-ttu-id="59e87-104">Den här artikeln beskrivs ändringar av befintliga Azure-tjänster som en del av att ändra samordnar flera Azure-tjänster i [Azure App Service](https://azure.microsoft.com/services/app-service/), ett nytt integrerade erbjudande.</span><span class="sxs-lookup"><span data-stu-id="59e87-104">This article outlines the changes to existing Azure services as part of the change to bring together several Azure services into [Azure App Service](https://azure.microsoft.com/services/app-service/), a new integrated offering.</span></span>

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="overview"></a><span data-ttu-id="59e87-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="59e87-105">Overview</span></span>
<span data-ttu-id="59e87-106">[Azure Apptjänst](https://azure.microsoft.com/services/app-service/) är en nya och unika molnbaserad tjänst som gör att utvecklare kan skapa webb- och mobilappar för alla plattformar och alla enheter.</span><span class="sxs-lookup"><span data-stu-id="59e87-106">[Azure App Service](https://azure.microsoft.com/services/app-service/) is a new and unique cloud service that enables developers to create web and mobile apps for any platform and any device.</span></span> <span data-ttu-id="59e87-107">Apptjänst är en integrerad lösning som utformats för att förenkla upprepade kodning funktioner, integrera med enterprise och SaaS-system och automatisera processer och uppfyller dina behov för säkerhet, tillförlitlighet och skalbarhet.</span><span class="sxs-lookup"><span data-stu-id="59e87-107">App Service is an integrated solution designed to streamline repeated coding functions, integrate with enterprise and SaaS systems, and automate business processes while meeting your needs for security, reliability, and scalability.</span></span>

<span data-ttu-id="59e87-108">Apptjänst sammanför följande befintliga Azure-tjänster - [webbplatser](https://azure.microsoft.com/services/websites/), [Mobile Services](https://azure.microsoft.com/services/mobile-services/), och [Biztalk-tjänst](https://azure.microsoft.com/services/biztalk-services/) till en enda kombineras tjänsten, när du lägger till kraftfulla nya funktioner.</span><span class="sxs-lookup"><span data-stu-id="59e87-108">App Service brings together the following existing Azure services - [Websites](https://azure.microsoft.com/services/websites/), [Mobile Services](https://azure.microsoft.com/services/mobile-services/), and [Biztalk Services](https://azure.microsoft.com/services/biztalk-services/) into a single combined service, while adding powerful new capabilities.</span></span>  <span data-ttu-id="59e87-109">Apptjänst kan du vara värd för följande typer:</span><span class="sxs-lookup"><span data-stu-id="59e87-109">App Service allows you to host the following app types:</span></span>

* <span data-ttu-id="59e87-110">Web Apps</span><span class="sxs-lookup"><span data-stu-id="59e87-110">Web Apps</span></span>
* <span data-ttu-id="59e87-111">Mobile Apps</span><span class="sxs-lookup"><span data-stu-id="59e87-111">Mobile Apps</span></span>
* <span data-ttu-id="59e87-112">API Apps</span><span class="sxs-lookup"><span data-stu-id="59e87-112">API Apps</span></span>
* <span data-ttu-id="59e87-113">Logic Apps</span><span class="sxs-lookup"><span data-stu-id="59e87-113">Logic Apps</span></span>

<span data-ttu-id="59e87-114">I följande tabell beskrivs hur Azure-tjänster karta till App Service och apptyper som är tillgängliga i den.</span><span class="sxs-lookup"><span data-stu-id="59e87-114">The following table explains how existing Azure services map to App Service and the app types available within it.</span></span>

<table>
<thead>
<tr class="header">
<th align="left", style="width:10%"><span data-ttu-id="59e87-115">Befintlig Azure-tjänst</span><span class="sxs-lookup"><span data-stu-id="59e87-115">Existing Azure Service</span></span></th>
<th align="left", style="width:10%"><span data-ttu-id="59e87-116">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="59e87-116">Azure App Service</span></span></th>
<th align="left", style="width:80%"><span data-ttu-id="59e87-117">Detta har ändrats</span><span class="sxs-lookup"><span data-stu-id="59e87-117">What changed</span></span></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><span data-ttu-id="59e87-118">Azure Websites</span><span class="sxs-lookup"><span data-stu-id="59e87-118">Azure Websites</span></span></td>
<td align="left"><span data-ttu-id="59e87-119">Web Apps</span><span class="sxs-lookup"><span data-stu-id="59e87-119">Web Apps</span></span></td>
<td align="left"><li><span data-ttu-id="59e87-120">Apptjänst är begränsas till att ändra namnet på webbplatser för Web Apps för Azure Websites.</span><span class="sxs-lookup"><span data-stu-id="59e87-120">For Azure Websites, App Service is strictly limited to changing the name  Websites to Web Apps.</span></span>
<p><li><span data-ttu-id="59e87-121">Alla befintliga instanser av webbplatser är nu Web Apps i Apptjänst.</span><span class="sxs-lookup"><span data-stu-id="59e87-121">All your existing instances of Websites are now Web Apps in App Service.</span></span></p>
<p><li><span data-ttu-id="59e87-122">Du kan komma åt dina befintliga webbplatser via den <a href="http://go.microsoft.com/fwlink/?LinkId=529715">Azure Portal</a>, där du hittar alla befintliga platser under <em>Web Apps</em>.</span><span class="sxs-lookup"><span data-stu-id="59e87-122">You can access your existing websites via the <a href="http://go.microsoft.com/fwlink/?LinkId=529715">Azure Portal</a>, where you will find all your existing sites under <em>Web Apps</em>.</span></span></p>
<p><li><span data-ttu-id="59e87-123"><em>Web värd planera</em> är nu <em>Apptjänstplan</em>.</span><span class="sxs-lookup"><span data-stu-id="59e87-123"><em>Web Hosting Plan</em> is now <em>App Service Plan</em>.</span></span> <span data-ttu-id="59e87-124">En <em>Apptjänstplan</em> kan vara värd för någon typ av app av App Service, till exempel webb, mobil, logik eller API apps.</span><span class="sxs-lookup"><span data-stu-id="59e87-124">An <em>App Service Plan</em> can host any app type of App Service, such as Web, Mobile, Logic, or API apps.</span></span></p>
<p><li><span data-ttu-id="59e87-125">Azure App Service Web Apps är i allmänhet tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="59e87-125">Azure App Service Web Apps is in General Availability.</span></span></p>
<p><li><span data-ttu-id="59e87-126"><a href="http://azure.microsoft.com/services/app-service/web/">Lär dig mer om Web Apps</a>.</span><span class="sxs-lookup"><span data-stu-id="59e87-126"><a href="http://azure.microsoft.com/services/app-service/web/">Learn more about Web Apps</a>.</span></span></p></td>
</tr>
<tr class="even">
<td align="left"><span data-ttu-id="59e87-127">Azure Mobile Services</span><span class="sxs-lookup"><span data-stu-id="59e87-127">Azure Mobile Services</span></span></td>
<td align="left"><span data-ttu-id="59e87-128">Mobile Apps</span><span class="sxs-lookup"><span data-stu-id="59e87-128">Mobile Apps</span></span></td>
<td align="left"><p><li><span data-ttu-id="59e87-129">Mobiltjänster fortsätter att vara tillgänglig som en fristående tjänst och förblir fullt stöd.</span><span class="sxs-lookup"><span data-stu-id="59e87-129">Mobile Services continue to be available as a standalone service and remain fully supported.</span></span></p>
<p><li><span data-ttu-id="59e87-130">Mobile Apps är en typ av app i App Service som integrerar alla funktionerna i Mobile Services och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="59e87-130">Mobile Apps is an app type in App Service, which integrates all of the functionality of Mobile Services and more.</span></span></p>
<p><li><span data-ttu-id="59e87-131">Det är lätt att <a href="http://go.microsoft.com/fwlink/?LinkID=724279&clcid=0x409">migrera från Mobile Services till Mobile Apps</a>.</span><span class="sxs-lookup"><span data-stu-id="59e87-131">It is easy to <a href="http://go.microsoft.com/fwlink/?LinkID=724279&clcid=0x409">migrate from Mobile Services to Mobile Apps</a>.</span></span></p>
<p><li><span data-ttu-id="59e87-132">Som en del av Apptjänst hämta Mobile Apps nya funktioner utöver Mobile Services, till exempel integrering med lokala och SaaS-system, mellanlagring kortplatser, WebJobs, bättre skalningsalternativ och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="59e87-132">As part of App Service, Mobile Apps get new capabilities beyond Mobile Services, such as  integration with on-premises and SaaS systems, staging slots, WebJobs, better scaling options, and more.</span></span></p>
<p><li><span data-ttu-id="59e87-133"><a href="http://azure.microsoft.com/services/app-service/mobile/">Mer information om Mobile Apps</a>.</span><span class="sxs-lookup"><span data-stu-id="59e87-133"><a href="http://azure.microsoft.com/services/app-service/mobile/">Learn more about Mobile Apps</a>.</span></span></p>
</tr>
<tr class="odd">
<td align="left"></td>
<td align="left"><span data-ttu-id="59e87-134">API Apps</span><span class="sxs-lookup"><span data-stu-id="59e87-134">API Apps</span></span></td>
<td align="left">
<p><li><span data-ttu-id="59e87-135">API Apps är en ny typ av app i App Service som gör att du enkelt skapa och använda API: er i molnet.</span><span class="sxs-lookup"><span data-stu-id="59e87-135">API Apps is a new app type in App Service that lets you easily build and consume APIs in the cloud.</span></span></p>
<p><li><span data-ttu-id="59e87-136"><a href="http://azure.microsoft.com/services/app-service/api/">Mer information om API Apps</a>.</span><span class="sxs-lookup"><span data-stu-id="59e87-136"><a href="http://azure.microsoft.com/services/app-service/api/">Learn more about API Apps</a>.</span></span></p></td>
</tr>
<tr class="even">
<td align="left"></td>
<td align="left"><span data-ttu-id="59e87-137">Logic Apps</span><span class="sxs-lookup"><span data-stu-id="59e87-137">Logic Apps</span></span></td>
<td align="left">
<p><li><span data-ttu-id="59e87-138">Logic Apps är en ny typ av app i App Service som gör att du enkelt automatisera affärsprocesser.</span><span class="sxs-lookup"><span data-stu-id="59e87-138">Logic Apps is a new app type in App Service that lets you easily automate business processes.</span></span></p>
<p><li><span data-ttu-id="59e87-139"><a href="http://azure.microsoft.com/services/app-service/logic/">Mer information om Logic Apps</a>.</span><span class="sxs-lookup"><span data-stu-id="59e87-139"><a href="http://azure.microsoft.com/services/app-service/logic/">Learn more about Logic Apps</a>.</span></span></p></td>
</tr>
<tr class="odd">
<td align="left"><span data-ttu-id="59e87-140">Azure BizTalk Services</span><span class="sxs-lookup"><span data-stu-id="59e87-140">Azure BizTalk Services</span></span></td>
<td align="left"><span data-ttu-id="59e87-141">BizTalk API Apps</span><span class="sxs-lookup"><span data-stu-id="59e87-141">BizTalk API Apps</span></span></td>
<td align="left">
<li><p><span data-ttu-id="59e87-142">BizTalk-tjänster fortsätter att vara tillgänglig som en fristående tjänst och förblir fullt stöd.</span><span class="sxs-lookup"><span data-stu-id="59e87-142">BizTalk Services continue to be available as a standalone service and remain fully supported.</span></span></p>
<li><p><span data-ttu-id="59e87-143">Alla funktioner i BizTalk-tjänst är integrerade i App Service som API Apps att användare kan utföra integration av företagsprogram och B2B integrationsscenarier med typer av app i App Service.</span><span class="sxs-lookup"><span data-stu-id="59e87-143">All the capabilities of BizTalk Services are integrated into App Service as API Apps enabling users to perform enterprise application integration and B2B integration scenarios with any of the app types in App Service.</span></span></p>
<li><p><span data-ttu-id="59e87-144">Du kan nu automatisera affärsprocesser med en visuell design-upplevelse för att skapa arbetsflöden med Logic Apps.</span><span class="sxs-lookup"><span data-stu-id="59e87-144">With Logic Apps, you can now automate business processes using a visual design experience to create workflows.</span></span></p></td>
</tr>
</tbody>
</table>

<span data-ttu-id="59e87-145">Mer information finns [Apptjänst dokumentationen](https://azure.microsoft.com/documentation/services/app-service/).</span><span class="sxs-lookup"><span data-stu-id="59e87-145">To learn more, please visit [App Service documentation](https://azure.microsoft.com/documentation/services/app-service/).</span></span>

