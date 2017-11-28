---
title: "Ansluta hallon Pi (nod) tooAzure IoT - lektionen 2: Hämta verktyg (Ubuntu) | Microsoft Docs"
description: "Installera Python och hello Azure-kommandoradsgränssnittet (Azure CLI) på Ubuntu."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: IOT cloud service, azure cli
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started
ms.assetid: 2f98923a-5274-4220-87d4-77ac8beb4d0f
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 0adf91bef41f502e1333fdcc75cfb2fe912364c9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-azure-tools-ubuntu-1604"></a><span data-ttu-id="8b3c9-104">Hämta Azure-verktyg (Ubuntu 16.04)</span><span class="sxs-lookup"><span data-stu-id="8b3c9-104">Get Azure tools (Ubuntu 16.04)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="8b3c9-105">Windows 7 och senare</span><span class="sxs-lookup"><span data-stu-id="8b3c9-105">Windows 7 and later</span></span>](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-win32.md)
> * [<span data-ttu-id="8b3c9-106">Ubuntu 16.04</span><span class="sxs-lookup"><span data-stu-id="8b3c9-106">Ubuntu 16.04</span></span>](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-ubuntu.md)
> * [<span data-ttu-id="8b3c9-107">macOS 10.10</span><span class="sxs-lookup"><span data-stu-id="8b3c9-107">macOS 10.10</span></span>](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-mac.md)

## <a name="what-you-will-do"></a><span data-ttu-id="8b3c9-108">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="8b3c9-108">What you will do</span></span>
<span data-ttu-id="8b3c9-109">Installera hello Azure-kommandoradsgränssnittet (Azure CLI).</span><span class="sxs-lookup"><span data-stu-id="8b3c9-109">Install hello Azure command-line interface (Azure CLI).</span></span> <span data-ttu-id="8b3c9-110">Om du har några problem med söka efter lösningar på hello [felsökning sidan](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="8b3c9-110">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="8b3c9-111">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="8b3c9-111">What you will learn</span></span>
<span data-ttu-id="8b3c9-112">I den här artikeln får du lära dig:</span><span class="sxs-lookup"><span data-stu-id="8b3c9-112">In this article, you will learn:</span></span>
* <span data-ttu-id="8b3c9-113">Hur tooinstall hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="8b3c9-113">How tooinstall hello Azure CLI.</span></span>
* <span data-ttu-id="8b3c9-114">Hur tooadd en IoT-undergrupp till hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="8b3c9-114">How tooadd an IoT subgroup of hello Azure CLI.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="8b3c9-115">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="8b3c9-115">What you need</span></span>
* <span data-ttu-id="8b3c9-116">En Ubuntu-dator med en Internet-anslutning.</span><span class="sxs-lookup"><span data-stu-id="8b3c9-116">An Ubuntu computer with an Internet connection.</span></span>
* <span data-ttu-id="8b3c9-117">En aktiv Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="8b3c9-117">An active Azure subscription.</span></span> <span data-ttu-id="8b3c9-118">Om du inte har ett konto kan du skapa en [ledigt utvärderingskonto](http://azure.microsoft.com/pricing/free-trial/) i bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="8b3c9-118">If you don't have an account, you can create a [free trial account](http://azure.microsoft.com/pricing/free-trial/) in just a few minutes.</span></span>

## <a name="install-hello-azure-cli"></a><span data-ttu-id="8b3c9-119">Installera hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="8b3c9-119">Install hello Azure CLI</span></span>
<span data-ttu-id="8b3c9-120">hello Azure CLI tillhandahåller en flera plattformar kommandoraden för Azure, vilket gör att du toowork direkt från din kommandoradsverktyget tooprovision och hantera resurser.</span><span class="sxs-lookup"><span data-stu-id="8b3c9-120">hello Azure CLI provides a multiplatform command-line experience for Azure, enabling you toowork directly from your command-line tooprovision and manage resources.</span></span>

<span data-ttu-id="8b3c9-121">tooinstall Hej senaste Azure CLI, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="8b3c9-121">tooinstall hello latest Azure CLI, follow these steps:</span></span>

1. <span data-ttu-id="8b3c9-122">Kör följande kommandon i ett terminalfönster hello.</span><span class="sxs-lookup"><span data-stu-id="8b3c9-122">Run hello following commands in a terminal window.</span></span> <span data-ttu-id="8b3c9-123">Det kan ta fem minuter tooinstall hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="8b3c9-123">It might take five minutes tooinstall hello Azure CLI.</span></span>

   ```bash
   sudo apt-get update
   sudo apt-get install -y libssl-dev libffi-dev
   sudo apt-get install -y python-dev
   sudo apt-get install -y build-essential
   sudo apt-get install -y python-pip
   sudo pip install --upgrade azure-cli
   sudo pip install --upgrade azure-cli-iot
   ```
2. <span data-ttu-id="8b3c9-124">Kontrollera hello installationen genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="8b3c9-124">Verify hello installation by running hello following command:</span></span>

   ```bash
   az iot -h
   ```

<span data-ttu-id="8b3c9-125">Du bör se hello följande utdata om hello-installationen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="8b3c9-125">You should see hello following output if hello installation is successful.</span></span>

![Utdata som indikerar att det lyckades](media/iot-hub-raspberry-pi-lessons/lesson2/az_iot_help_ubuntu.png)

## <a name="summary"></a><span data-ttu-id="8b3c9-127">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="8b3c9-127">Summary</span></span>
<span data-ttu-id="8b3c9-128">Du har installerat hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="8b3c9-128">You've installed hello Azure CLI.</span></span> <span data-ttu-id="8b3c9-129">Nästa uppgift är toocreate dina Azure IoT-hubb och enheter identitet med hjälp av hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="8b3c9-129">Your next task is toocreate your Azure IoT hub and device identity using hello Azure CLI.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8b3c9-130">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8b3c9-130">Next steps</span></span>
[<span data-ttu-id="8b3c9-131">Skapa din IoT-hubb och registrera hallon Pi 3</span><span class="sxs-lookup"><span data-stu-id="8b3c9-131">Create your IoT hub and register Raspberry Pi 3</span></span>](iot-hub-raspberry-pi-kit-node-lesson2-prepare-azure-iot-hub.md)

