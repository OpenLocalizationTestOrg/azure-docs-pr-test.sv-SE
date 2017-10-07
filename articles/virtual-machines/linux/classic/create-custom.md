---
title: "aaaCreate en klassisk virtuell Linux-dator med hjälp av hello Azure CLI 1.0 | Microsoft Docs"
description: "Lär dig hur toocreate en Linux-dator med hello Azure CLI 1.0 med hello klassiska distributionsmodellen"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: f8071a2e-ed91-4f77-87d9-519a37e5364f
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/09/2017
ms.author: iainfou
ms.openlocfilehash: 5c3c54e5d1444a79e8e609c76d04a3a621c8d03f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-a-classic-linux-vm-with-hello-azure-cli-10"></a><span data-ttu-id="d5047-103">Hur tooCreate en klassisk virtuell Linux-dator med hello Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="d5047-103">How tooCreate a Classic Linux VM with hello Azure CLI 1.0</span></span>
> [!IMPORTANT] 
> <span data-ttu-id="d5047-104">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="d5047-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="d5047-105">Den här artikeln täcker hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="d5047-105">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="d5047-106">Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="d5047-106">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="d5047-107">Hello Resource Manager version finns [här](../create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="d5047-107">For hello Resource Manager version, see [here](../create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="d5047-108">Det här avsnittet beskrivs hur toocreate en Linux-dator (VM) hello Azure CLI 1.0 med hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="d5047-108">This topic describes how toocreate a Linux virtual machine (VM) with hello Azure CLI 1.0 using hello Classic deployment model.</span></span> <span data-ttu-id="d5047-109">Vi använder en Linux-avbildning från hello tillgängliga **bilder** på Azure.</span><span class="sxs-lookup"><span data-stu-id="d5047-109">We use a Linux image from hello available **IMAGES** on Azure.</span></span> <span data-ttu-id="d5047-110">hello Azure CLI 1.0 kommandon ge hello följande konfigurationsalternativ, bland annat:</span><span class="sxs-lookup"><span data-stu-id="d5047-110">hello Azure CLI 1.0 commands give hello following configuration choices, among others:</span></span>

* <span data-ttu-id="d5047-111">Ansluta hello VM tooa virtuella nätverk</span><span class="sxs-lookup"><span data-stu-id="d5047-111">Connecting hello VM tooa virtual network</span></span>
* <span data-ttu-id="d5047-112">Lägga till hello VM tooan befintlig molntjänst</span><span class="sxs-lookup"><span data-stu-id="d5047-112">Adding hello VM tooan existing cloud service</span></span>
* <span data-ttu-id="d5047-113">Lägga till hello VM tooan befintligt lagringskonto</span><span class="sxs-lookup"><span data-stu-id="d5047-113">Adding hello VM tooan existing storage account</span></span>
* <span data-ttu-id="d5047-114">Lägger till hello VM tooan tillgänglighetsuppsättning eller plats</span><span class="sxs-lookup"><span data-stu-id="d5047-114">Adding hello VM tooan availability set or location</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d5047-115">Om du vill att din VM toouse ett virtuellt nätverk så att du kan ansluta tooit direkt av värdnamn eller konfigurera anslutningar mellan platser, se till att ange hello virtuellt nätverk när du skapar hello VM.</span><span class="sxs-lookup"><span data-stu-id="d5047-115">If you want your VM toouse a virtual network so you can connect tooit directly by hostname or set up cross-premises connections, make sure you specify hello virtual network when you create hello VM.</span></span> <span data-ttu-id="d5047-116">En virtuell dator kan vara konfigurerade toojoin ett virtuellt nätverk endast när du skapar hello VM.</span><span class="sxs-lookup"><span data-stu-id="d5047-116">A VM can be configured toojoin a virtual network only when you create hello VM.</span></span> <span data-ttu-id="d5047-117">Mer information om virtuella nätverk finns [Azure översikt över virtuella nätverk](http://go.microsoft.com/fwlink/p/?LinkID=294063).</span><span class="sxs-lookup"><span data-stu-id="d5047-117">For details on virtual networks, see [Azure Virtual Network Overview](http://go.microsoft.com/fwlink/p/?LinkID=294063).</span></span>
> 
> 

## <a name="how-toocreate-a-linux-vm-using-hello-classic-deployment-model"></a><span data-ttu-id="d5047-118">Hur toocreate en Linux VM som använder hello klassiska distributionsmodellen</span><span class="sxs-lookup"><span data-stu-id="d5047-118">How toocreate a Linux VM using hello Classic deployment model</span></span>
[!INCLUDE [virtual-machines-create-LinuxVM](../../../../includes/virtual-machines-create-linuxvm.md)]

