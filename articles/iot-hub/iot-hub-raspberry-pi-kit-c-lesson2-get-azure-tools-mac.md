---
title: 'Connect Raspberry PI (C) tooAzure IoT - lektionen 2: Azure-verktyg (macOS) | Microsoft Docs'
description: "Installera Python och Azure-kommandoradsgränssnittet (Azure CLI) på macOS."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: IOT cloud service, azure cli
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-c-get-started
ms.assetid: f2d7d584-7734-401c-976c-81788a7282a3
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: a079250fd94fa9bc1c11b6c21de02a8d46f6f3bd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-azure-tools-macos-1010"></a><span data-ttu-id="a7365-104">Hämta Azure-verktyg (macOS 10.10)</span><span class="sxs-lookup"><span data-stu-id="a7365-104">Get Azure tools (macOS 10.10)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="a7365-105">Windows 7 och senare</span><span class="sxs-lookup"><span data-stu-id="a7365-105">Windows 7 and later</span></span>](iot-hub-raspberry-pi-kit-c-lesson2-get-azure-tools-win32.md)
> * [<span data-ttu-id="a7365-106">Ubuntu 16.04</span><span class="sxs-lookup"><span data-stu-id="a7365-106">Ubuntu 16.04</span></span>](iot-hub-raspberry-pi-kit-c-lesson2-get-azure-tools-ubuntu.md)
> * [<span data-ttu-id="a7365-107">macOS 10.10</span><span class="sxs-lookup"><span data-stu-id="a7365-107">macOS 10.10</span></span>](iot-hub-raspberry-pi-kit-c-lesson2-get-azure-tools-mac.md)

## <a name="what-you-will-do"></a><span data-ttu-id="a7365-108">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="a7365-108">What you will do</span></span>
<span data-ttu-id="a7365-109">Installera hello Azure-kommandoradsgränssnittet (Azure CLI).</span><span class="sxs-lookup"><span data-stu-id="a7365-109">Install hello Azure command-line interface (Azure CLI).</span></span> <span data-ttu-id="a7365-110">Om du har några problem med söka efter lösningar på hello [felsökning sidan](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="a7365-110">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="a7365-111">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="a7365-111">What you will learn</span></span>
<span data-ttu-id="a7365-112">I den här artikeln får du lära dig:</span><span class="sxs-lookup"><span data-stu-id="a7365-112">In this article, you will learn:</span></span>
* <span data-ttu-id="a7365-113">Hur tooinstall Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="a7365-113">How tooinstall Azure CLI.</span></span>
* <span data-ttu-id="a7365-114">Hur tooadd en IoT-undergrupp till hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="a7365-114">How tooadd an IoT subgroup of hello Azure CLI.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="a7365-115">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="a7365-115">What you need</span></span>
* <span data-ttu-id="a7365-116">En Mac med en Internet-anslutning.</span><span class="sxs-lookup"><span data-stu-id="a7365-116">A Mac with an Internet connection.</span></span>
* <span data-ttu-id="a7365-117">En aktiv Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="a7365-117">An active Azure subscription.</span></span> <span data-ttu-id="a7365-118">Om du inte har ett Azure-konto kan du skapa en [kostnadsfria Azure utvärderingskonto](http://azure.microsoft.com/pricing/free-trial/) i bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="a7365-118">If you don't have an Azure account, you can create a [free Azure trial account](http://azure.microsoft.com/pricing/free-trial/) in just a few minutes.</span></span>

## <a name="install-python"></a><span data-ttu-id="a7365-119">Installera Python</span><span class="sxs-lookup"><span data-stu-id="a7365-119">Install Python</span></span>
<span data-ttu-id="a7365-120">Även om macOS medföljer Python 2.7 out of box hello, rekommenderar vi att du installerar Python via Homebrew.</span><span class="sxs-lookup"><span data-stu-id="a7365-120">Although macOS comes with Python 2.7 out of hello box, we recommend that you install Python through Homebrew.</span></span> <span data-ttu-id="a7365-121">Se [installerar Python på macOS](http://docs.python-guide.org/en/latest/starting/install/osx/).</span><span class="sxs-lookup"><span data-stu-id="a7365-121">See [Installing Python on macOS](http://docs.python-guide.org/en/latest/starting/install/osx/).</span></span>

<span data-ttu-id="a7365-122">Installera Python och pip genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="a7365-122">Install Python and pip by running hello following command:</span></span>

```bash
brew install python
```

## <a name="install-hello-azure-cli"></a><span data-ttu-id="a7365-123">Installera hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="a7365-123">Install hello Azure CLI</span></span>
<span data-ttu-id="a7365-124">hello Azure CLI tillhandahåller en flera plattformar kommandoraden för Azure.</span><span class="sxs-lookup"><span data-stu-id="a7365-124">hello Azure CLI provides a multiplatform command-line experience for Azure.</span></span> <span data-ttu-id="a7365-125">Du arbetar direkt från kommandoraden-tooprovision och hantera resurser.</span><span class="sxs-lookup"><span data-stu-id="a7365-125">You work directly from your command line tooprovision and manage resources.</span></span> 

<span data-ttu-id="a7365-126">tooinstall Hej senaste Azure CLI, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="a7365-126">tooinstall hello latest Azure CLI, follow these steps:</span></span>

1. <span data-ttu-id="a7365-127">Kör följande kommandon i ett terminalfönster hello.</span><span class="sxs-lookup"><span data-stu-id="a7365-127">Run hello following commands in a terminal window.</span></span> <span data-ttu-id="a7365-128">Det kan ta fem minuter tooinstall hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="a7365-128">It might take five minutes tooinstall hello Azure CLI.</span></span>

   ```bash
   pip install --upgrade azure-cli
   pip install --upgrade azure-cli-iot
   ```
2. <span data-ttu-id="a7365-129">Kontrollera hello installationen genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="a7365-129">Verify hello installation by running hello following command:</span></span>

   ```bash
   az iot -h
   ```

<span data-ttu-id="a7365-130">Du bör se hello följande utdata om hello-installationen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="a7365-130">You should see hello following output if hello installation is successful.</span></span>

![Utdata som indikerar att det lyckades](media/iot-hub-raspberry-pi-lessons/lesson2/az_iot_help_osx.png)

## <a name="summary"></a><span data-ttu-id="a7365-132">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="a7365-132">Summary</span></span>
<span data-ttu-id="a7365-133">Du har installerat hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="a7365-133">You've installed hello Azure CLI.</span></span> <span data-ttu-id="a7365-134">Nästa uppgift är toocreate dina Azure IoT hub- och enhetsidentitet med hjälp av hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="a7365-134">Your next task is toocreate your Azure IoT hub and device identity by using hello Azure CLI.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a7365-135">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a7365-135">Next steps</span></span>
[<span data-ttu-id="a7365-136">Skapa din IoT-hubb och registrera hallon Pi 3</span><span class="sxs-lookup"><span data-stu-id="a7365-136">Create your IoT hub and register Raspberry Pi 3</span></span>](iot-hub-raspberry-pi-kit-c-lesson2-prepare-azure-iot-hub.md)

