---
title: "Relaterade och länkade resurser i galleriet sida vid sida"
description: "Läs mer om relaterade och länkade resurser som visas i panelen galleriet med Azure preview portal."
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
ms.openlocfilehash: efa7bce26c2c4c153b083e0e34d689a11d27dd16
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="related-and-linked-resources-in-the-tile-gallery"></a><span data-ttu-id="16b86-103">Relaterade och länkade resurser i galleriet sida vid sida</span><span class="sxs-lookup"><span data-stu-id="16b86-103">Related and linked resources in the tile gallery</span></span>
<span data-ttu-id="16b86-104">Galleriet sida vid sida kan du hitta paneler för en viss resurs och drar dem till din aktuella bladet.</span><span class="sxs-lookup"><span data-stu-id="16b86-104">The tile gallery enables you to find tiles for a particular resource and drag them onto your current blade.</span></span> <span data-ttu-id="16b86-105">Med galleriet sida vid sida kan skapa du vyer som sträcker sig över resurser.</span><span class="sxs-lookup"><span data-stu-id="16b86-105">Using the tile gallery, you can create management views that span resources.</span></span> <span data-ttu-id="16b86-106">För alla angivna resurser innehåller relaterade resurser alla resurser i dess resursgruppen och alla resurser som länkar till och från resursen.</span><span class="sxs-lookup"><span data-stu-id="16b86-106">For any specified resource, the related resources include all the resources in its resource group, and any resources that link to or from the resource.</span></span>

## <a name="linked-resources-in-resource-manager"></a><span data-ttu-id="16b86-107">Länkade resurser i Resource Manager</span><span class="sxs-lookup"><span data-stu-id="16b86-107">Linked resources in Resource Manager</span></span>
<span data-ttu-id="16b86-108">Länka är en funktion av Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="16b86-108">Linking is a feature of the Resource Manager.</span></span>  <span data-ttu-id="16b86-109">På så sätt kan du deklarera relationerna mellan resurser, även om de inte finnas i samma resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="16b86-109">It enables you to declare relationships between resources even if they do not reside in the same resource group.</span></span> <span data-ttu-id="16b86-110">Länka har ingen inverkan på körning av dina resurser, utan inverkan på fakturering och ingen inverkan på rollbaserad åtkomst.</span><span class="sxs-lookup"><span data-stu-id="16b86-110">Linking has no impact on the runtime of your resources, no impact on billing, and no impact on role-based access.</span></span>  <span data-ttu-id="16b86-111">Det är bara en mekanism som du kan använda för att representera relationer så att verktyg som sida vid sida-galleriet kan tillhandahålla en omfattande hantering.</span><span class="sxs-lookup"><span data-stu-id="16b86-111">It's simply a mechanism you can use to represent relationships so that tools like the tile gallery can provide a rich management experience.</span></span>  <span data-ttu-id="16b86-112">Din verktyg kan inspektera länkar med hjälp av länkarna API och tillhandahålla anpassade hantering inträffar också.</span><span class="sxs-lookup"><span data-stu-id="16b86-112">Your tools can inspect the links using the links API and provide custom relationship management experiences as well.</span></span> 

## <a name="how-do-i-link-my-resources"></a><span data-ttu-id="16b86-113">Hur länkar mina resurser?</span><span class="sxs-lookup"><span data-stu-id="16b86-113">How do I link my resources?</span></span>
<span data-ttu-id="16b86-114">När du skapar resurser via portalen eller genom att distribuera en mall med Azure PowerShell eller Azure CLI, skapas automatiskt länkar för vissa beroende resurser.</span><span class="sxs-lookup"><span data-stu-id="16b86-114">When you create resources through the portal or by deploying a template through Azure PowerShell or Azure CLI, links are automatically created for some dependent resources.</span></span> <span data-ttu-id="16b86-115">Du kan också programmässigt länka resurser med hjälp av den [länkade resurser REST API](/rest/api/resources/resourcelinks).</span><span class="sxs-lookup"><span data-stu-id="16b86-115">You can also programmatically link resources by using the [Linked Resources REST API](/rest/api/resources/resourcelinks).</span></span>

## <a name="next-steps"></a><span data-ttu-id="16b86-116">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="16b86-116">Next steps</span></span>
* <span data-ttu-id="16b86-117">Om du behöver en introduktion till skrivning Resource Manager-mallar finns [Webbsidemallar](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="16b86-117">If you need an introduction to writing Resource Manager templates, see [Authoring templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="16b86-118">Om du vill veta mer om hur du arbetar med resursgrupper via portalen, se [hantera Azure-resurser med hjälp av Azure portal](../azure-resource-manager/resource-group-portal.md).</span><span class="sxs-lookup"><span data-stu-id="16b86-118">To understand more about working with resource groups through the portal, see [Using the Azure portal to manage your Azure resources](../azure-resource-manager/resource-group-portal.md).</span></span>

