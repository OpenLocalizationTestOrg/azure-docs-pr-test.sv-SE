---
title: 'Connect Raspberry PI (C) tooAzure IoT - lektionen 2: Azure-verktyg (Ubuntu) | Microsoft Docs'
description: "Installera Python och Azure-kommandoradsgränssnittet (Azure CLI) på Ubuntu."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: IOT cloud service, azure cli
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-c-get-started
ms.assetid: a03512f2-fabe-40c5-8505-b93eef8e5bec
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 1af4848f9fe0508e362c15b36eec8a35aea9ac5b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-azure-tools-ubuntu-1604"></a><span data-ttu-id="33bf5-104">Hämta Azure-verktyg (Ubuntu 16.04)</span><span class="sxs-lookup"><span data-stu-id="33bf5-104">Get Azure tools (Ubuntu 16.04)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="33bf5-105">Windows 7 och senare</span><span class="sxs-lookup"><span data-stu-id="33bf5-105">Windows 7 and later</span></span>](iot-hub-raspberry-pi-kit-c-lesson2-get-azure-tools-win32.md)
> * [<span data-ttu-id="33bf5-106">Ubuntu 16.04</span><span class="sxs-lookup"><span data-stu-id="33bf5-106">Ubuntu 16.04</span></span>](iot-hub-raspberry-pi-kit-c-lesson2-get-azure-tools-ubuntu.md)
> * [<span data-ttu-id="33bf5-107">macOS 10.10</span><span class="sxs-lookup"><span data-stu-id="33bf5-107">macOS 10.10</span></span>](iot-hub-raspberry-pi-kit-c-lesson2-get-azure-tools-mac.md)

## <a name="what-you-will-do"></a><span data-ttu-id="33bf5-108">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="33bf5-108">What you will do</span></span>
<span data-ttu-id="33bf5-109">Installera hello Azure-kommandoradsgränssnittet (Azure CLI).</span><span class="sxs-lookup"><span data-stu-id="33bf5-109">Install hello Azure command-line interface (Azure CLI).</span></span> <span data-ttu-id="33bf5-110">Om du har några problem med söka efter lösningar på hello [felsökning sidan](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="33bf5-110">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="33bf5-111">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="33bf5-111">What you will learn</span></span>
<span data-ttu-id="33bf5-112">I den här artikeln får du lära dig:</span><span class="sxs-lookup"><span data-stu-id="33bf5-112">In this article, you will learn:</span></span>
* <span data-ttu-id="33bf5-113">Hur tooinstall hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="33bf5-113">How tooinstall hello Azure CLI.</span></span>
* <span data-ttu-id="33bf5-114">Hur tooadd en IoT-undergrupp till hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="33bf5-114">How tooadd an IoT subgroup of hello Azure CLI.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="33bf5-115">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="33bf5-115">What you need</span></span>
* <span data-ttu-id="33bf5-116">En Ubuntu-dator med en Internet-anslutning.</span><span class="sxs-lookup"><span data-stu-id="33bf5-116">An Ubuntu computer with an Internet connection.</span></span>
* <span data-ttu-id="33bf5-117">En aktiv Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="33bf5-117">An active Azure subscription.</span></span> <span data-ttu-id="33bf5-118">Om du inte har ett konto kan du skapa en [ledigt utvärderingskonto](http://azure.microsoft.com/pricing/free-trial/) i bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="33bf5-118">If you don't have an account, you can create a [free trial account](http://azure.microsoft.com/pricing/free-trial/) in just a few minutes.</span></span>

## <a name="install-hello-azure-cli"></a><span data-ttu-id="33bf5-119">Installera hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="33bf5-119">Install hello Azure CLI</span></span>
<span data-ttu-id="33bf5-120">hello Azure CLI tillhandahåller en flera plattformar kommandoraden för Azure, vilket gör att du toowork direkt från kommandoraden-tooprovision och hantera resurser.</span><span class="sxs-lookup"><span data-stu-id="33bf5-120">hello Azure CLI provides a multiplatform command-line experience for Azure, enabling you toowork directly from your command line tooprovision and manage resources.</span></span>

<span data-ttu-id="33bf5-121">tooinstall Hej senaste Azure CLI, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="33bf5-121">tooinstall hello latest Azure CLI, follow these steps:</span></span>

1. <span data-ttu-id="33bf5-122">Kör följande kommandon i ett terminalfönster hello.</span><span class="sxs-lookup"><span data-stu-id="33bf5-122">Run hello following commands in a terminal window.</span></span> <span data-ttu-id="33bf5-123">Det kan ta fem minuter tooinstall hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="33bf5-123">It might take five minutes tooinstall hello Azure CLI.</span></span>

   ```bash
   sudo apt-get update
   sudo apt-get install -y libssl-dev libffi-dev
   sudo apt-get install -y python-dev
   sudo apt-get install -y build-essential
   sudo apt-get install -y python-pip
   sudo pip install --upgrade azure-cli
   sudo pip install --upgrade azure-cli-iot
   ```
2. <span data-ttu-id="33bf5-124">Kontrollera hello installationen genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="33bf5-124">Verify hello installation by running hello following command:</span></span>

   ```bash
   az iot -h
   ```

<span data-ttu-id="33bf5-125">Du bör se hello följande utdata om hello-installationen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="33bf5-125">You should see hello following output if hello installation is successful.</span></span>

![Utdata som indikerar att det lyckades](media/iot-hub-raspberry-pi-lessons/lesson2/az_iot_help_ubuntu.png)

## <a name="summary"></a><span data-ttu-id="33bf5-127">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="33bf5-127">Summary</span></span>
<span data-ttu-id="33bf5-128">Du har installerat hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="33bf5-128">You've installed hello Azure CLI.</span></span> <span data-ttu-id="33bf5-129">Nästa uppgift är toocreate dina Azure IoT-hubb och enheter identitet med hjälp av hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="33bf5-129">Your next task is toocreate your Azure IoT hub and device identity using hello Azure CLI.</span></span>

## <a name="next-steps"></a><span data-ttu-id="33bf5-130">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="33bf5-130">Next steps</span></span>
[<span data-ttu-id="33bf5-131">Skapa din IoT-hubb och registrera hallon Pi 3</span><span class="sxs-lookup"><span data-stu-id="33bf5-131">Create your IoT hub and register Raspberry Pi 3</span></span>](iot-hub-raspberry-pi-kit-c-lesson2-prepare-azure-iot-hub.md)

