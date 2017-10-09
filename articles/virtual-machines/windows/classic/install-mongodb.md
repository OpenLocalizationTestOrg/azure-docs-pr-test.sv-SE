---
title: "aaaInstall MongoDB på en Windows-dator i Azure | Microsoft Docs"
description: "Lär dig hur du skapar tooinstall MongoDB på en virtuell dator i Azure med hello klassiska distributionsmodellen med Windows Server."
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
ms.openlocfilehash: aafd86b4c49c6a97ae8a1a2191ae375b6c47483a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="install-mongodb-on-a-windows-vm-in-azure"></a><span data-ttu-id="361e1-103">Installera MongoDB på en Windows-dator i Azure</span><span class="sxs-lookup"><span data-stu-id="361e1-103">Install MongoDB on a Windows VM in Azure</span></span>
> [!IMPORTANT]
> <span data-ttu-id="361e1-104">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="361e1-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span>  <span data-ttu-id="361e1-105">Den här artikeln täcker hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="361e1-105">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="361e1-106">Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="361e1-106">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="361e1-107">tooinstall och konfigurera MongoDB med hello Resource Manager-modellen, se [i den här artikeln](../install-mongodb.md).</span><span class="sxs-lookup"><span data-stu-id="361e1-107">tooinstall and configure MongoDB using hello Resource Manager deployment model, see [this article](../install-mongodb.md).</span></span>

<span data-ttu-id="361e1-108">[MongoDB] [ MongoDB] är en populär öppen källkod, högpresterande NoSQL-databas.</span><span class="sxs-lookup"><span data-stu-id="361e1-108">[MongoDB][MongoDB] is a popular open-source, high-performance NoSQL database.</span></span> <span data-ttu-id="361e1-109">Den här artikeln hjälper dig att skapa en Windows Server virtuell dator (VM) med hello [Azure-portalen][AzurePortal].</span><span class="sxs-lookup"><span data-stu-id="361e1-109">This article guides you through creating a Windows Server virtual machine (VM) using hello [Azure portal][AzurePortal].</span></span> <span data-ttu-id="361e1-110">Du kan sedan skapa och koppla en data disk toohello VM innan du installerar och konfigurerar MongoDB.</span><span class="sxs-lookup"><span data-stu-id="361e1-110">You then create and attach a data disk toohello VM before installing and configuring MongoDB.</span></span> <span data-ttu-id="361e1-111">Om du har en befintlig virtuell dator i Azure som du vill att toouse kan du hoppa direkt för[installera och konfigurera MongoDB](#install-and-run-mongodb-on-the-virtual-machine).</span><span class="sxs-lookup"><span data-stu-id="361e1-111">If you have an existing VM in Azure that you would like toouse, you can jump straight too[installing and configuring MongoDB](#install-and-run-mongodb-on-the-virtual-machine).</span></span>

## <a name="create-a-virtual-machine-running-windows-server"></a><span data-ttu-id="361e1-112">Skapa en virtuell dator som kör Windows Server</span><span class="sxs-lookup"><span data-stu-id="361e1-112">Create a virtual machine running Windows Server</span></span>
<span data-ttu-id="361e1-113">Följ dessa instruktioner toocreate en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="361e1-113">Follow these instructions toocreate a virtual machine.</span></span>

[!INCLUDE [virtual-machines-create-WindowsVM](../../../../includes/virtual-machines-create-windowsvm.md)]

> [!NOTE]
> <span data-ttu-id="361e1-114">Du kan lägga till en slutpunkt för MongoDB när du skapar hello virtuell dator och konfigurera den på följande sätt: namnet som **Mongo**, använda **TCP** som hello-protokollet och ange båda hello offentliga och privata portar för**27017**.</span><span class="sxs-lookup"><span data-stu-id="361e1-114">You can add an endpoint for MongoDB while creating hello virtual machine, and configure it as follows: name it as **Mongo**, use **TCP** as hello protocol, and set both hello public and private ports too**27017**.</span></span>
>
>

## <a name="attach-a-data-disk"></a><span data-ttu-id="361e1-115">Anslut en datadisk</span><span class="sxs-lookup"><span data-stu-id="361e1-115">Attach a data disk</span></span>
<span data-ttu-id="361e1-116">tooprovide lagring för hello virtuell dator kan ansluta en datadisk och initiera den så att Windows kan använda den.</span><span class="sxs-lookup"><span data-stu-id="361e1-116">tooprovide storage for hello virtual machine, attach a data disk and then initialize it so that Windows can use it.</span></span> <span data-ttu-id="361e1-117">Om du redan har en datadisk, kan du koppla den befintliga disken eller du kan koppla en tom disk.</span><span class="sxs-lookup"><span data-stu-id="361e1-117">If you already have a data disk, you can attach that existing disk, or you can attach an empty disk.</span></span>

[!INCLUDE [howto-attach-disk-windows-linux](../../../../includes/howto-attach-disk-windows-linux.md)]

<span data-ttu-id="361e1-118">Anvisningar för att initiera hello disk, finns i ”så här: initierar en ny datadisk i Windows Server” i [hur tooattach data disk tooa Windows-dator](attach-disk.md).</span><span class="sxs-lookup"><span data-stu-id="361e1-118">For instructions on initializing hello disk, see "How to: Initialize a new data disk in Windows Server" in [How tooattach a data disk tooa Windows virtual machine](attach-disk.md).</span></span>

## <a name="install-and-run-mongodb-on-hello-virtual-machine"></a><span data-ttu-id="361e1-119">Installera och köra MongoDB på hello virtuell dator</span><span class="sxs-lookup"><span data-stu-id="361e1-119">Install and run MongoDB on hello virtual machine</span></span>
[!INCLUDE [install-and-run-mongo-on-win2k8-vm](../../../../includes/install-and-run-mongo-on-win2k8-vm.md)]

## <a name="summary"></a><span data-ttu-id="361e1-120">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="361e1-120">Summary</span></span>
<span data-ttu-id="361e1-121">I kursen får du har lärt dig hur toocreate en virtuell dator som kör Windows Server fjärransluta tooit, och ansluta en datadisk.</span><span class="sxs-lookup"><span data-stu-id="361e1-121">In this tutorial, you learned how toocreate a virtual machine running Windows Server, remotely connect tooit, and attach a data disk.</span></span>  <span data-ttu-id="361e1-122">Du också lära sig hur tooinstall och konfigurera MongoDB på hello Windows-baserad virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="361e1-122">You also learned how tooinstall and configure MongoDB on hello Windows-based virtual machine.</span></span> <span data-ttu-id="361e1-123">Du kan nu komma åt MongoDB på hello Windows-baserade virtuella datorn genom följande hello avancerade avsnitt i hello [MongoDB-dokumentation][MongoDocs].</span><span class="sxs-lookup"><span data-stu-id="361e1-123">You can now access MongoDB on hello Windows-based virtual machine, by following hello advanced topics in hello [MongoDB documentation][MongoDocs].</span></span>

[MongoDocs]: http://docs.mongodb.org/manual/
[MongoDB]: http://www.mongodb.org/
[AzurePortal]: https://portal.azure.com/

