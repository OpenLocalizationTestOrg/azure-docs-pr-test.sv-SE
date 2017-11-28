---
title: "Installera MongoDB på en Windows-dator i Azure | Microsoft Docs"
description: "Lär dig hur du installerar MongoDB på en Azure VM som skapats med den klassiska distributionsmodellen som kör Windows Server."
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 4095df41-bb69-4bbe-9c1c-70923b0d84ba
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/07/2017
ms.author: iainfou
ms.openlocfilehash: 6b5af18d02fd508a21cdc21b38b1c16e79f07ecb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="install-mongodb-on-a-windows-vm-in-azure"></a><span data-ttu-id="0cd46-103">Installera MongoDB på en Windows-dator i Azure</span><span class="sxs-lookup"><span data-stu-id="0cd46-103">Install MongoDB on a Windows VM in Azure</span></span>
> [!IMPORTANT]
> <span data-ttu-id="0cd46-104">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="0cd46-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span>  <span data-ttu-id="0cd46-105">Den här artikeln täcker den klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="0cd46-105">This article covers using the Classic deployment model.</span></span> <span data-ttu-id="0cd46-106">Microsoft rekommenderar att de flesta nya distributioner använder Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="0cd46-106">Microsoft recommends that most new deployments use the Resource Manager model.</span></span> <span data-ttu-id="0cd46-107">Om du vill installera och konfigurera MongoDB med hjälp av Resource Manager-distributionsmodellen, se [i den här artikeln](../install-mongodb.md).</span><span class="sxs-lookup"><span data-stu-id="0cd46-107">To install and configure MongoDB using the Resource Manager deployment model, see [this article](../install-mongodb.md).</span></span>

<span data-ttu-id="0cd46-108">[MongoDB] [ MongoDB] är en populär öppen källkod, högpresterande NoSQL-databas.</span><span class="sxs-lookup"><span data-stu-id="0cd46-108">[MongoDB][MongoDB] is a popular open-source, high-performance NoSQL database.</span></span> <span data-ttu-id="0cd46-109">Den här artikeln hjälper dig att skapa en Windows Server virtuell dator (VM) med hjälp av den [Azure-portalen][AzurePortal].</span><span class="sxs-lookup"><span data-stu-id="0cd46-109">This article guides you through creating a Windows Server virtual machine (VM) using the [Azure portal][AzurePortal].</span></span> <span data-ttu-id="0cd46-110">Du skapar och bifogar en datadisk till den virtuella datorn innan du installerar och konfigurerar MongoDB.</span><span class="sxs-lookup"><span data-stu-id="0cd46-110">You then create and attach a data disk to the VM before installing and configuring MongoDB.</span></span> <span data-ttu-id="0cd46-111">Om du har en befintlig virtuell dator i Azure som du vill använda kan du hoppa direkt till [installera och konfigurera MongoDB](#install-and-run-mongodb-on-the-virtual-machine).</span><span class="sxs-lookup"><span data-stu-id="0cd46-111">If you have an existing VM in Azure that you would like to use, you can jump straight to [installing and configuring MongoDB](#install-and-run-mongodb-on-the-virtual-machine).</span></span>

## <a name="create-a-virtual-machine-running-windows-server"></a><span data-ttu-id="0cd46-112">Skapa en virtuell dator som kör Windows Server</span><span class="sxs-lookup"><span data-stu-id="0cd46-112">Create a virtual machine running Windows Server</span></span>
<span data-ttu-id="0cd46-113">Följ instruktionerna för att skapa en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="0cd46-113">Follow these instructions to create a virtual machine.</span></span>

[!INCLUDE [virtual-machines-create-WindowsVM](../../../../includes/virtual-machines-create-windowsvm.md)]

> [!NOTE]
> <span data-ttu-id="0cd46-114">Du kan lägga till en slutpunkt för MongoDB när du skapar den virtuella datorn och konfigurera den på följande sätt: namnet som **Mongo**, använda **TCP** som protokoll, och Ställ in både offentliga och privata portar på  **27017**.</span><span class="sxs-lookup"><span data-stu-id="0cd46-114">You can add an endpoint for MongoDB while creating the virtual machine, and configure it as follows: name it as **Mongo**, use **TCP** as the protocol, and set both the public and private ports to **27017**.</span></span>
>
>

## <a name="attach-a-data-disk"></a><span data-ttu-id="0cd46-115">Anslut en datadisk</span><span class="sxs-lookup"><span data-stu-id="0cd46-115">Attach a data disk</span></span>
<span data-ttu-id="0cd46-116">Ansluta en datadisk för att ge lagring för den virtuella datorn, och sedan initiera så att Windows kan använda den.</span><span class="sxs-lookup"><span data-stu-id="0cd46-116">To provide storage for the virtual machine, attach a data disk and then initialize it so that Windows can use it.</span></span> <span data-ttu-id="0cd46-117">Om du redan har en datadisk, kan du koppla den befintliga disken eller du kan koppla en tom disk.</span><span class="sxs-lookup"><span data-stu-id="0cd46-117">If you already have a data disk, you can attach that existing disk, or you can attach an empty disk.</span></span>

[!INCLUDE [howto-attach-disk-windows-linux](../../../../includes/howto-attach-disk-windows-linux.md)]

<span data-ttu-id="0cd46-118">Anvisningar för att initiera disken, finns i ”så här: initierar en ny datadisk i Windows Server” i [hur du kopplar en datadisk till en virtuell dator för Windows](attach-disk.md).</span><span class="sxs-lookup"><span data-stu-id="0cd46-118">For instructions on initializing the disk, see "How to: Initialize a new data disk in Windows Server" in [How to attach a data disk to a Windows virtual machine](attach-disk.md).</span></span>

## <a name="install-and-run-mongodb-on-the-virtual-machine"></a><span data-ttu-id="0cd46-119">Installera och köra MongoDB på den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="0cd46-119">Install and run MongoDB on the virtual machine</span></span>
[!INCLUDE [install-and-run-mongo-on-win2k8-vm](../../../../includes/install-and-run-mongo-on-win2k8-vm.md)]

## <a name="summary"></a><span data-ttu-id="0cd46-120">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="0cd46-120">Summary</span></span>
<span data-ttu-id="0cd46-121">I den här självstudiekursen beskrivs hur du skapar en virtuell dator som kör Windows Server från en annan dator att ansluta till den och ansluta en datadisk.</span><span class="sxs-lookup"><span data-stu-id="0cd46-121">In this tutorial, you learned how to create a virtual machine running Windows Server, remotely connect to it, and attach a data disk.</span></span>  <span data-ttu-id="0cd46-122">Du också fått lära dig hur du installerar och konfigurerar MongoDB på Windows-baserad virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="0cd46-122">You also learned how to install and configure MongoDB on the Windows-based virtual machine.</span></span> <span data-ttu-id="0cd46-123">Du kan nu komma åt MongoDB på Windows-baserad virtuell dator, genom att följa de avancerade ämnena i det [MongoDB-dokumentation][MongoDocs].</span><span class="sxs-lookup"><span data-stu-id="0cd46-123">You can now access MongoDB on the Windows-based virtual machine, by following the advanced topics in the [MongoDB documentation][MongoDocs].</span></span>

[MongoDocs]: http://docs.mongodb.org/manual/
[MongoDB]: http://www.mongodb.org/
[AzurePortal]: https://portal.azure.com/

