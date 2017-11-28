---
title: aaaHow tootag en virtuell Azure Linux-dator | Microsoft Docs
description: "Mer information om taggar en Azure Linux-dator som skapats i Azure med hjälp av hello Resource Manager-modellen."
services: virtual-machines-linux
documentationcenter: 
author: mmccrory
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: ca0e17e5-d78e-42e6-9dad-c1e8f1c58027
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/28/2017
ms.author: memccror
ms.openlocfilehash: 0e99ea66a87b7e00eb21a2f72dd2bce8673778dd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tootag-a-linux-virtual-machine-in-azure"></a><span data-ttu-id="c10ec-103">Hur tootag en Linux-dator i Azure</span><span class="sxs-lookup"><span data-stu-id="c10ec-103">How tootag a Linux virtual machine in Azure</span></span>
<span data-ttu-id="c10ec-104">Den här artikeln beskrivs olika sätt tootag en Linux-dator i Azure via hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="c10ec-104">This article describes different ways tootag a Linux virtual machine in Azure through hello Resource Manager deployment model.</span></span> <span data-ttu-id="c10ec-105">Taggar är användardefinierade nyckel/värde-par som kan placeras direkt på en resurs eller en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="c10ec-105">Tags are user-defined key/value pairs which can be placed directly on a resource or a resource group.</span></span> <span data-ttu-id="c10ec-106">Azure stöder för närvarande in too15 taggar per resurs och resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="c10ec-106">Azure currently supports up too15 tags per resource and resource group.</span></span> <span data-ttu-id="c10ec-107">Taggar kan placeras på en resurs för närvarande hello skapas eller lägga tooan befintlig resurs.</span><span class="sxs-lookup"><span data-stu-id="c10ec-107">Tags may be placed on a resource at hello time of creation or added tooan existing resource.</span></span> <span data-ttu-id="c10ec-108">Observera taggar stöds för resurser som skapats via hello Resource Manager distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="c10ec-108">Please note, tags are supported for resources created via hello Resource Manager deployment model only.</span></span>

[!INCLUDE [virtual-machines-common-tag](../../../includes/virtual-machines-common-tag.md)]

## <a name="tagging-with-azure-cli"></a><span data-ttu-id="c10ec-109">Tagga med Azure CLI</span><span class="sxs-lookup"><span data-stu-id="c10ec-109">Tagging with Azure CLI</span></span>
<span data-ttu-id="c10ec-110">toobegin, [installera och konfigurera hello Azure CLI](../../xplat-cli-azure-resource-manager.md) och kontrollera att du är i läget Resource Manager (`azure config mode arm`).</span><span class="sxs-lookup"><span data-stu-id="c10ec-110">toobegin, [install and configure hello Azure CLI](../../xplat-cli-azure-resource-manager.md) and make sure you are in Resource Manager mode (`azure config mode arm`).</span></span>

<span data-ttu-id="c10ec-111">Du kan visa alla egenskaper för en viss virtuell dator, inklusive hello taggar, med det här kommandot:</span><span class="sxs-lookup"><span data-stu-id="c10ec-111">You can view all properties for a given Virtual Machine, including hello tags, using this command:</span></span>

        azure vm show -g MyResourceGroup -n MyTestVM

<span data-ttu-id="c10ec-112">tooadd en ny VM-tagg via hello Azure CLI, kan du använda hello `azure vm set` kommandot tillsammans med hello taggen parametern **-t**:</span><span class="sxs-lookup"><span data-stu-id="c10ec-112">tooadd a new VM tag through hello Azure CLI, you can use hello `azure vm set` command along with hello tag parameter **-t**:</span></span>

        azure vm set -g MyResourceGroup -n MyTestVM –t myNewTagName1=myNewTagValue1;myNewTagName2=myNewTagValue2

<span data-ttu-id="c10ec-113">tooremove alla taggar, du kan använda hello **– T** parameter i hello `azure vm set` kommando.</span><span class="sxs-lookup"><span data-stu-id="c10ec-113">tooremove all tags, you can use hello **–T** parameter in hello `azure vm set` command.</span></span>

        azure vm set – g MyResourceGroup –n MyTestVM -T


<span data-ttu-id="c10ec-114">Nu när vi har använt taggar tooour resurser Azure CLI och hello Portal ska vi ta en titt på hello användning information toosee hello taggar i hello fakturering portal.</span><span class="sxs-lookup"><span data-stu-id="c10ec-114">Now that we have applied tags tooour resources Azure CLI and hello Portal, let’s take a look at hello usage details toosee hello tags in hello billing portal.</span></span>

[!INCLUDE [virtual-machines-common-tag-usage](../../../includes/virtual-machines-common-tag-usage.md)]

## <a name="next-steps"></a><span data-ttu-id="c10ec-115">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c10ec-115">Next steps</span></span>
* <span data-ttu-id="c10ec-116">toolearn mer information om resurserna i Azure-märkning finns [översikt över Azure Resource Manager] [ Azure Resource Manager Overview] och [med hjälp av taggar tooorganize dina Azure-resurser] [ Using Tags tooorganize your Azure Resources].</span><span class="sxs-lookup"><span data-stu-id="c10ec-116">toolearn more about tagging your Azure resources, see [Azure Resource Manager Overview][Azure Resource Manager Overview] and [Using Tags tooorganize your Azure Resources][Using Tags tooorganize your Azure Resources].</span></span>
* <span data-ttu-id="c10ec-117">toosee hur taggar kan hjälpa dig att hantera din användning av Azure-resurser, se [förstå fakturan Azure] [ Understanding your Azure Bill] och [få insikter om dina Microsoft Azure-resursförbrukning] [Gain insights into your Microsoft Azure resource consumption].</span><span class="sxs-lookup"><span data-stu-id="c10ec-117">toosee how tags can help you manage your use of Azure resources, see [Understanding your Azure Bill][Understanding your Azure Bill] and [Gain insights into your Microsoft Azure resource consumption][Gain insights into your Microsoft Azure resource consumption].</span></span>

[Azure CLI environment]: ../../azure-resource-manager/xplat-cli-azure-resource-manager.md
[Azure Resource Manager Overview]: ../../azure-resource-manager/resource-group-overview.md
[Using Tags tooorganize your Azure Resources]: ../../azure-resource-manager/resource-group-using-tags.md
[Understanding your Azure Bill]: ../../billing/billing-understand-your-bill.md
[Gain insights into your Microsoft Azure resource consumption]: ../../billing/billing-usage-rate-card-overview.md
