---
title: "Distribuera Azure API Management-tjänster i Azure-regioner | Microsoft Docs"
description: "Lär dig hur du distribuerar en Azure API Management service-instans till Azure-regioner."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 47389ad6-f865-4706-833f-846115e22e4d
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: 1c39fee739c2f5fd4b928e1e76e1ea57f072b5f8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-deploy-an-azure-api-management-service-instance-to-multiple-azure-regions"></a><span data-ttu-id="64b2a-103">Distribuera ett Azure API Management service-instans till Azure-regioner</span><span class="sxs-lookup"><span data-stu-id="64b2a-103">How to deploy an Azure API Management service instance to multiple Azure regions</span></span>
<span data-ttu-id="64b2a-104">API-hantering stöder distribution av flera regioner där API utgivare att distribuera en enda API-tjänsten till alla önskade Azure-regioner.</span><span class="sxs-lookup"><span data-stu-id="64b2a-104">API Management supports multi-region deployment which enables API publishers to distribute a single API management service across any number of desired Azure regions.</span></span> <span data-ttu-id="64b2a-105">Detta minskar begäran latens uppfattas av geografiskt distribuerat API-konsumenter och förbättrar även tjänsttillgängligheten om en region tas offline.</span><span class="sxs-lookup"><span data-stu-id="64b2a-105">This helps reduce request latency perceived by geographically distributed API consumers and also improves service availability if one region goes offline.</span></span> 

<span data-ttu-id="64b2a-106">När en API Management-tjänst skapas från början, innehåller endast en [enhet] [ unit] och finns i en enda Azure-region, som anges som den primära regionen.</span><span class="sxs-lookup"><span data-stu-id="64b2a-106">When an API Management service is created initially, it contains only one [unit][unit] and resides in a single Azure region, which is designated as the Primary Region.</span></span> <span data-ttu-id="64b2a-107">Ytterligare regioner kan enkelt lägga via Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="64b2a-107">Additional regions can be easily added through the Azure Portal.</span></span> <span data-ttu-id="64b2a-108">En API Management gateway-servern har distribuerats till varje region och anrop trafik vidarebefordras till närmaste gateway.</span><span class="sxs-lookup"><span data-stu-id="64b2a-108">An API Management gateway server is deployed to each region and call traffic will be routed to the closest gateway.</span></span> <span data-ttu-id="64b2a-109">Om en region försätts offline, är trafiken automatiskt igen dirigerad till nästa närmaste gateway.</span><span class="sxs-lookup"><span data-stu-id="64b2a-109">If a region goes offline, the traffic is automatically re-directed to the next closest gateway.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="64b2a-110">Distribution av flera regioner är endast tillgängligt i den  **[Premium] [ Premium]**  nivå.</span><span class="sxs-lookup"><span data-stu-id="64b2a-110">Multi-region deployment is only available in the **[Premium][Premium]** tier.</span></span>
> 
> 

## <span data-ttu-id="64b2a-111"><a name="add-region"></a>Distribuera en API Management service-instans till en ny region</span><span class="sxs-lookup"><span data-stu-id="64b2a-111"><a name="add-region"> </a>Deploy an API Management service instance to a new region</span></span>
> [!NOTE]
> <span data-ttu-id="64b2a-112">Om du inte har skapat en API Management-tjänstinstans än läser du [Skapa en API Management-tjänstinstans][Create an API Management service instance] i självstudiekursen [Komma igång med Azure API Management][Get started with Azure API Management].</span><span class="sxs-lookup"><span data-stu-id="64b2a-112">If you have not yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in the [Get started with Azure API Management][Get started with Azure API Management] tutorial.</span></span>
> 
> 

<span data-ttu-id="64b2a-113">I Azure Portal går du till den **skala och prissättning** för din API Management service-instans.</span><span class="sxs-lookup"><span data-stu-id="64b2a-113">In the Azure Portal navigate to the **Scale and pricing** page for your API Management service instance.</span></span> 

![Skala][api-management-scale-service]

<span data-ttu-id="64b2a-115">Om du vill distribuera till en ny region, klickar du på **+ Lägg till region** i verktygsfältet.</span><span class="sxs-lookup"><span data-stu-id="64b2a-115">To deploy to a new region, click on **+ Add region** from the toolbar.</span></span>

![Lägg till region][api-management-add-region]

<span data-ttu-id="64b2a-117">Välja platsen i den nedrullningsbara listrutan och ange antalet enheter för med skjutreglaget.</span><span class="sxs-lookup"><span data-stu-id="64b2a-117">Select the location from the drop-down list and set the number of units for with the slider.</span></span>

![Ange enheter][api-management-select-location-units]

<span data-ttu-id="64b2a-119">Klicka på **Lägg till** att placera ditt val i tabellen platser.</span><span class="sxs-lookup"><span data-stu-id="64b2a-119">Click **Add** to place your selection in the Locations table.</span></span> 

<span data-ttu-id="64b2a-120">Upprepa den här processen tills du har alla platser som har konfigurerats och klicka på **spara** från verktygsfältet för att starta distributionsprocessen.</span><span class="sxs-lookup"><span data-stu-id="64b2a-120">Repeat this process until you have all locations configured and click **Save** from the toolbar to start the deployment process.</span></span>

## <span data-ttu-id="64b2a-121"><a name="remove-region"></a>Ta bort en instans för API Management-tjänsten från en plats</span><span class="sxs-lookup"><span data-stu-id="64b2a-121"><a name="remove-region"> </a>Delete an API Management service instance from a location</span></span>
<span data-ttu-id="64b2a-122">I Azure Portal går du till den **skala och prissättning** för din API Management service-instans.</span><span class="sxs-lookup"><span data-stu-id="64b2a-122">In the Azure Portal navigate to the **Scale and pricing** page for your API Management service instance.</span></span> 

![Skala][api-management-scale-service]

<span data-ttu-id="64b2a-124">Öppna menyn kontext med för den plats som du vill ta bort den **...**  längst till höger i tabellen.</span><span class="sxs-lookup"><span data-stu-id="64b2a-124">For the location you would like to remove open the context menu using the **...** button at the right end of the table.</span></span> <span data-ttu-id="64b2a-125">Välj den **ta bort** alternativet.</span><span class="sxs-lookup"><span data-stu-id="64b2a-125">Select the **Delete** option.</span></span>

![Ta bort region][api-management-remove-region]

<span data-ttu-id="64b2a-127">Bekräfta borttagningen och klicka på **spara** att tillämpa ändringarna.</span><span class="sxs-lookup"><span data-stu-id="64b2a-127">Confirm the deletion and click **Save** to apply the changes.</span></span>

[api-management-management-console]: ./media/api-management-howto-deploy-multi-region/api-management-management-console.png

[api-management-scale-service]: ./media/api-management-howto-deploy-multi-region/api-management-scale-service.png
[api-management-add-region]: ./media/api-management-howto-deploy-multi-region/api-management-add-region.png
[api-management-select-location-units]: ./media/api-management-howto-deploy-multi-region/api-management-select-location-units.png
[api-management-remove-region]: ./media/api-management-howto-deploy-multi-region/api-management-remove-region.png

[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[Get started with Azure API Management]: api-management-get-started.md

[Deploy an API Management service instance to a new region]: #add-region
[Delete an API Management service instance from a region]: #remove-region

[unit]: http://azure.microsoft.com/pricing/details/api-management/
[Premium]: http://azure.microsoft.com/pricing/details/api-management/

