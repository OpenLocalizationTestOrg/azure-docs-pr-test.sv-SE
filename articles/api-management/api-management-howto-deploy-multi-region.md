---
title: aaaDeploy Azure API Management services toomultiple Azure regioner | Microsoft Docs
description: "Lär dig hur toodeploy ett Azure API Management-tjänsten instans toomultiple Azure regioner."
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
ms.openlocfilehash: 04a3e762261237d73a769320a21363f99f1d20cb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toodeploy-an-azure-api-management-service-instance-toomultiple-azure-regions"></a><span data-ttu-id="18f18-103">Hur toodeploy ett Azure API Management-tjänsten instans toomultiple Azure regioner</span><span class="sxs-lookup"><span data-stu-id="18f18-103">How toodeploy an Azure API Management service instance toomultiple Azure regions</span></span>
<span data-ttu-id="18f18-104">API-hantering stöder distribution av flera regioner där API utgivare toodistribute ett enda API-tjänsten till alla önskade Azure-regioner.</span><span class="sxs-lookup"><span data-stu-id="18f18-104">API Management supports multi-region deployment which enables API publishers toodistribute a single API management service across any number of desired Azure regions.</span></span> <span data-ttu-id="18f18-105">Detta minskar begäran latens uppfattas av geografiskt distribuerat API-konsumenter och förbättrar även tjänsttillgängligheten om en region tas offline.</span><span class="sxs-lookup"><span data-stu-id="18f18-105">This helps reduce request latency perceived by geographically distributed API consumers and also improves service availability if one region goes offline.</span></span> 

<span data-ttu-id="18f18-106">När en API Management-tjänst skapas från början, innehåller endast en [enhet] [ unit] och finns i en enda Azure-region, som är tilldelad som hello primär Region.</span><span class="sxs-lookup"><span data-stu-id="18f18-106">When an API Management service is created initially, it contains only one [unit][unit] and resides in a single Azure region, which is designated as hello Primary Region.</span></span> <span data-ttu-id="18f18-107">Ytterligare regioner kan enkelt lagts till via hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="18f18-107">Additional regions can be easily added through hello Azure Portal.</span></span> <span data-ttu-id="18f18-108">En API Management gateway-servern är distribuerad tooeach region och trafik call blir routade toohello närmaste gateway.</span><span class="sxs-lookup"><span data-stu-id="18f18-108">An API Management gateway server is deployed tooeach region and call traffic will be routed toohello closest gateway.</span></span> <span data-ttu-id="18f18-109">Om en region frånkopplas är hello trafik automatiskt igen dirigerad toohello nästa närmaste gateway.</span><span class="sxs-lookup"><span data-stu-id="18f18-109">If a region goes offline, hello traffic is automatically re-directed toohello next closest gateway.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="18f18-110">Distribution av flera regioner är endast tillgängligt i hello  **[Premium] [ Premium]**  nivå.</span><span class="sxs-lookup"><span data-stu-id="18f18-110">Multi-region deployment is only available in hello **[Premium][Premium]** tier.</span></span>
> 
> 

## <span data-ttu-id="18f18-111"><a name="add-region"></a>Distribuera en API Management service-instans tooa nya region</span><span class="sxs-lookup"><span data-stu-id="18f18-111"><a name="add-region"> </a>Deploy an API Management service instance tooa new region</span></span>
> [!NOTE]
> <span data-ttu-id="18f18-112">Om du inte har skapat en instans för API Management-tjänsten finns [skapa en instans för API Management-tjänsten] [ Create an API Management service instance] i hello [Kom igång med Azure API Management] [ Get started with Azure API Management] kursen.</span><span class="sxs-lookup"><span data-stu-id="18f18-112">If you have not yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in hello [Get started with Azure API Management][Get started with Azure API Management] tutorial.</span></span>
> 
> 

<span data-ttu-id="18f18-113">Navigera i hello Azure Portal toohello **skala och prissättning** för din API Management service-instans.</span><span class="sxs-lookup"><span data-stu-id="18f18-113">In hello Azure Portal navigate toohello **Scale and pricing** page for your API Management service instance.</span></span> 

![Skala][api-management-scale-service]

<span data-ttu-id="18f18-115">toodeploy tooa ny region, klicka på **+ Lägg till region** hello-verktygsfältet.</span><span class="sxs-lookup"><span data-stu-id="18f18-115">toodeploy tooa new region, click on **+ Add region** from hello toolbar.</span></span>

![Lägg till region][api-management-add-region]

<span data-ttu-id="18f18-117">Välj hello plats från hello listrutan och ange hello antal enheter för med hello skjutreglaget.</span><span class="sxs-lookup"><span data-stu-id="18f18-117">Select hello location from hello drop-down list and set hello number of units for with hello slider.</span></span>

![Ange enheter][api-management-select-location-units]

<span data-ttu-id="18f18-119">Klicka på **Lägg till** tooplace ditt val i tabellen för hello-platser.</span><span class="sxs-lookup"><span data-stu-id="18f18-119">Click **Add** tooplace your selection in hello Locations table.</span></span> 

<span data-ttu-id="18f18-120">Upprepa den här processen tills du har alla platser som har konfigurerats och klicka på **spara** från hello verktygsfältet toostart hello distributionsprocessen.</span><span class="sxs-lookup"><span data-stu-id="18f18-120">Repeat this process until you have all locations configured and click **Save** from hello toolbar toostart hello deployment process.</span></span>

## <span data-ttu-id="18f18-121"><a name="remove-region"></a>Ta bort en instans för API Management-tjänsten från en plats</span><span class="sxs-lookup"><span data-stu-id="18f18-121"><a name="remove-region"> </a>Delete an API Management service instance from a location</span></span>
<span data-ttu-id="18f18-122">Navigera i hello Azure Portal toohello **skala och prissättning** för din API Management service-instans.</span><span class="sxs-lookup"><span data-stu-id="18f18-122">In hello Azure Portal navigate toohello **Scale and pricing** page for your API Management service instance.</span></span> 

![Skala][api-management-scale-service]

<span data-ttu-id="18f18-124">Hello-plats som öppnar tooremove hello snabbmenyn med hello **...**  längst hello högra ände hello tabell.</span><span class="sxs-lookup"><span data-stu-id="18f18-124">For hello location you would like tooremove open hello context menu using hello **...** button at hello right end of hello table.</span></span> <span data-ttu-id="18f18-125">Välj hello **ta bort** alternativet.</span><span class="sxs-lookup"><span data-stu-id="18f18-125">Select hello **Delete** option.</span></span>

![Ta bort region][api-management-remove-region]

<span data-ttu-id="18f18-127">Bekräfta borttagning av hello och klicka på **spara** tooapply hello ändringar.</span><span class="sxs-lookup"><span data-stu-id="18f18-127">Confirm hello deletion and click **Save** tooapply hello changes.</span></span>

[api-management-management-console]: ./media/api-management-howto-deploy-multi-region/api-management-management-console.png

[api-management-scale-service]: ./media/api-management-howto-deploy-multi-region/api-management-scale-service.png
[api-management-add-region]: ./media/api-management-howto-deploy-multi-region/api-management-add-region.png
[api-management-select-location-units]: ./media/api-management-howto-deploy-multi-region/api-management-select-location-units.png
[api-management-remove-region]: ./media/api-management-howto-deploy-multi-region/api-management-remove-region.png

[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[Get started with Azure API Management]: api-management-get-started.md

[Deploy an API Management service instance tooa new region]: #add-region
[Delete an API Management service instance from a region]: #remove-region

[unit]: http://azure.microsoft.com/pricing/details/api-management/
[Premium]: http://azure.microsoft.com/pricing/details/api-management/

