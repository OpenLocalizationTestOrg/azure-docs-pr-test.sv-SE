---
title: "aaaRelated och länkade resurser i hello panelen galleri"
description: "Läs mer om relaterade och länkade resurser som visas i hello panelen galleriet med hello Azure preview portal."
services: azure-portal
documentationcenter: 
author: adamabdelhamed
manager: wpickett
editor: 
ms.assetid: dba96d29-f518-49c8-bfd2-f14cecb44cbf
ms.service: azure-portal
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/16/2015
ms.author: adamab
ms.openlocfilehash: c8f99be8e23dc9641ec3cd3169604b33a4b049f4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="related-and-linked-resources-in-hello-tile-gallery"></a><span data-ttu-id="82c37-103">Relaterade och länkade resurser i hello sida vid sida-galleriet</span><span class="sxs-lookup"><span data-stu-id="82c37-103">Related and linked resources in hello tile gallery</span></span>
<span data-ttu-id="82c37-104">hello panelen galleriet kan du toofind paneler för en viss resurs och drar dem till din aktuella bladet.</span><span class="sxs-lookup"><span data-stu-id="82c37-104">hello tile gallery enables you toofind tiles for a particular resource and drag them onto your current blade.</span></span> <span data-ttu-id="82c37-105">Du kan skapa vyer som sträcker sig över resurser med hello sida vid sida-galleriet.</span><span class="sxs-lookup"><span data-stu-id="82c37-105">Using hello tile gallery, you can create management views that span resources.</span></span> <span data-ttu-id="82c37-106">För alla angivna resurser relaterade hello resurser omfattar alla hello resurser i dess resursgruppen och alla resurser som länkar tooor från hello resurs.</span><span class="sxs-lookup"><span data-stu-id="82c37-106">For any specified resource, hello related resources include all hello resources in its resource group, and any resources that link tooor from hello resource.</span></span>

## <a name="linked-resources-in-resource-manager"></a><span data-ttu-id="82c37-107">Länkade resurser i Resource Manager</span><span class="sxs-lookup"><span data-stu-id="82c37-107">Linked resources in Resource Manager</span></span>
<span data-ttu-id="82c37-108">Länka är en funktion i hello Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="82c37-108">Linking is a feature of hello Resource Manager.</span></span>  <span data-ttu-id="82c37-109">Du kan använda toodeclare relationerna mellan resurser även om de inte finns i hello samma resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="82c37-109">It enables you toodeclare relationships between resources even if they do not reside in hello same resource group.</span></span> <span data-ttu-id="82c37-110">Länka har ingen inverkan på hello körtiden för dina resurser, utan inverkan på fakturering och utan inverkan på rollbaserad åtkomst.</span><span class="sxs-lookup"><span data-stu-id="82c37-110">Linking has no impact on hello runtime of your resources, no impact on billing, and no impact on role-based access.</span></span>  <span data-ttu-id="82c37-111">Det är bara en mekanism som du kan använda toorepresent relationer så att verktyg som hello panelen galleriet kan tillhandahålla en omfattande hantering.</span><span class="sxs-lookup"><span data-stu-id="82c37-111">It's simply a mechanism you can use toorepresent relationships so that tools like hello tile gallery can provide a rich management experience.</span></span>  <span data-ttu-id="82c37-112">Din verktyg kan inspektera hello länkar med hello länkar API och tillhandahålla anpassade hantering inträffar också.</span><span class="sxs-lookup"><span data-stu-id="82c37-112">Your tools can inspect hello links using hello links API and provide custom relationship management experiences as well.</span></span> 

## <a name="how-do-i-link-my-resources"></a><span data-ttu-id="82c37-113">Hur länkar mina resurser?</span><span class="sxs-lookup"><span data-stu-id="82c37-113">How do I link my resources?</span></span>
<span data-ttu-id="82c37-114">När du skapar resurser via hello portalen eller genom att distribuera en mall med Azure PowerShell eller Azure CLI, skapas automatiskt länkar för vissa beroende resurser.</span><span class="sxs-lookup"><span data-stu-id="82c37-114">When you create resources through hello portal or by deploying a template through Azure PowerShell or Azure CLI, links are automatically created for some dependent resources.</span></span> <span data-ttu-id="82c37-115">Du kan också programmässigt länka resurser med hjälp av hello [länkade resurser REST API](/rest/api/resources/resourcelinks).</span><span class="sxs-lookup"><span data-stu-id="82c37-115">You can also programmatically link resources by using hello [Linked Resources REST API](/rest/api/resources/resourcelinks).</span></span>

## <a name="next-steps"></a><span data-ttu-id="82c37-116">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="82c37-116">Next steps</span></span>
* <span data-ttu-id="82c37-117">Om du behöver en introduktion toowriting Resource Manager-mallar finns [Webbsidemallar](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="82c37-117">If you need an introduction toowriting Resource Manager templates, see [Authoring templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="82c37-118">toounderstand mer information om hur du arbetar med resursgrupper via hello portal finns [Using hello Azure portal toomanage resurserna i Azure](../azure-resource-manager/resource-group-portal.md).</span><span class="sxs-lookup"><span data-stu-id="82c37-118">toounderstand more about working with resource groups through hello portal, see [Using hello Azure portal toomanage your Azure resources](../azure-resource-manager/resource-group-portal.md).</span></span>

