---
title: "Skapa en klassisk virtuell Linux-dator med hjälp av Azure CLI 1.0 | Microsoft Docs"
description: "Lär dig hur du skapar en virtuell Linux-dator med Azure CLI-1.0 med hjälp av den klassiska distributionsmodellen"
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
ms.openlocfilehash: 8ddbacbbb70c0cf1a2537fab4d981a316610a4d7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-create-a-classic-linux-vm-with-the-azure-cli-10"></a><span data-ttu-id="60046-103">Så här skapar du en klassisk virtuell Linux-dator med Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="60046-103">How to Create a Classic Linux VM with the Azure CLI 1.0</span></span>
> [!IMPORTANT] 
> <span data-ttu-id="60046-104">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="60046-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="60046-105">Den här artikeln täcker den klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="60046-105">This article covers using the Classic deployment model.</span></span> <span data-ttu-id="60046-106">Microsoft rekommenderar att de flesta nya distributioner använder Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="60046-106">Microsoft recommends that most new deployments use the Resource Manager model.</span></span> <span data-ttu-id="60046-107">Resource Manager-version finns [här](../create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="60046-107">For the Resource Manager version, see [here](../create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="60046-108">Det här avsnittet beskriver hur du skapar en Linux-dator (VM) med Azure CLI-1.0 med hjälp av den klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="60046-108">This topic describes how to create a Linux virtual machine (VM) with the Azure CLI 1.0 using the Classic deployment model.</span></span> <span data-ttu-id="60046-109">Vi använder en Linux-avbildning från de tillgängliga **bilder** på Azure.</span><span class="sxs-lookup"><span data-stu-id="60046-109">We use a Linux image from the available **IMAGES** on Azure.</span></span> <span data-ttu-id="60046-110">Azure CLI 1.0-kommandona ger följande konfigurationsalternativ, bland annat:</span><span class="sxs-lookup"><span data-stu-id="60046-110">The Azure CLI 1.0 commands give the following configuration choices, among others:</span></span>

* <span data-ttu-id="60046-111">Ansluta den virtuella datorn till ett virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="60046-111">Connecting the VM to a virtual network</span></span>
* <span data-ttu-id="60046-112">Att lägga till den virtuella datorn till en befintlig molntjänst</span><span class="sxs-lookup"><span data-stu-id="60046-112">Adding the VM to an existing cloud service</span></span>
* <span data-ttu-id="60046-113">Att lägga till den virtuella datorn till ett befintligt lagringskonto</span><span class="sxs-lookup"><span data-stu-id="60046-113">Adding the VM to an existing storage account</span></span>
* <span data-ttu-id="60046-114">Lägger till den virtuella datorn till en tillgänglighetsuppsättning eller plats</span><span class="sxs-lookup"><span data-stu-id="60046-114">Adding the VM to an availability set or location</span></span>

> [!IMPORTANT]
> <span data-ttu-id="60046-115">Om du vill att den virtuella datorn använder ett virtuellt nätverk så att du kan ansluta till den direkt av värdnamn eller konfigurera anslutningar mellan platser, kontrollera att du anger det virtuella nätverket när du skapar den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="60046-115">If you want your VM to use a virtual network so you can connect to it directly by hostname or set up cross-premises connections, make sure you specify the virtual network when you create the VM.</span></span> <span data-ttu-id="60046-116">En virtuell dator kan konfigureras för att ansluta ett virtuellt nätverk endast när du skapar den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="60046-116">A VM can be configured to join a virtual network only when you create the VM.</span></span> <span data-ttu-id="60046-117">Mer information om virtuella nätverk finns [Azure översikt över virtuella nätverk](http://go.microsoft.com/fwlink/p/?LinkID=294063).</span><span class="sxs-lookup"><span data-stu-id="60046-117">For details on virtual networks, see [Azure Virtual Network Overview](http://go.microsoft.com/fwlink/p/?LinkID=294063).</span></span>
> 
> 

## <a name="how-to-create-a-linux-vm-using-the-classic-deployment-model"></a><span data-ttu-id="60046-118">Så här skapar du en Linux VM som använder den klassiska distributionsmodellen</span><span class="sxs-lookup"><span data-stu-id="60046-118">How to create a Linux VM using the Classic deployment model</span></span>
[!INCLUDE [virtual-machines-create-LinuxVM](../../../../includes/virtual-machines-create-linuxvm.md)]

