---
title: 'Ansluta Intel modern (nod) tooAzure IoT - lektionen 2: Azure-verktyg (macOS) | Microsoft Docs'
description: "Installera Python och Azure-kommandoradsgränssnittet (Azure CLI) på macOS."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "Azure cli, iot-Molntjänsten, arduino moln"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-node-get-started
ms.assetid: 8a2a8031-b1a6-4219-b17d-2825550c35e1
ms.service: iot-hub
ms.devlang: nodejs
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: e33b3c9ee8f06f1c6175457f98a0600dd945cf1b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-azure-tools-macos-1010"></a><span data-ttu-id="62085-104">Hämta Azure-verktyg (macOS 10.10)</span><span class="sxs-lookup"><span data-stu-id="62085-104">Get Azure tools (macOS 10.10)</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="62085-105">[Windows 7 och senare][windows]</span><span class="sxs-lookup"><span data-stu-id="62085-105">[Windows 7 and later][windows]</span></span>
> * <span data-ttu-id="62085-106">[Ubuntu 16.04][ubuntu]</span><span class="sxs-lookup"><span data-stu-id="62085-106">[Ubuntu 16.04][ubuntu]</span></span>
> * <span data-ttu-id="62085-107">[macOS 10.10][macos]</span><span class="sxs-lookup"><span data-stu-id="62085-107">[macOS 10.10][macos]</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="62085-108">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="62085-108">What you will do</span></span>
<span data-ttu-id="62085-109">Installera hello Azure-kommandoradsgränssnittet (Azure CLI).</span><span class="sxs-lookup"><span data-stu-id="62085-109">Install hello Azure command-line interface (Azure CLI).</span></span> <span data-ttu-id="62085-110">Om du har några problem med söka efter lösningar på hello [felsökning sidan][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="62085-110">If you have any problems, look for solutions on hello [troubleshooting page][troubleshooting].</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="62085-111">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="62085-111">What you will learn</span></span>
<span data-ttu-id="62085-112">I den här artikeln får du lära dig:</span><span class="sxs-lookup"><span data-stu-id="62085-112">In this article, you will learn:</span></span>
* <span data-ttu-id="62085-113">Hur tooinstall Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="62085-113">How tooinstall Azure CLI.</span></span>
* <span data-ttu-id="62085-114">Hur tooadd en IoT-undergrupp till hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="62085-114">How tooadd an IoT subgroup of hello Azure CLI.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="62085-115">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="62085-115">What you need</span></span>
* <span data-ttu-id="62085-116">En Mac med en Internet-anslutning.</span><span class="sxs-lookup"><span data-stu-id="62085-116">A Mac with an Internet connection.</span></span>
* <span data-ttu-id="62085-117">En aktiv Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="62085-117">An active Azure subscription.</span></span> <span data-ttu-id="62085-118">Om du inte har ett Azure-konto kan du skapa en [kostnadsfria Azure utvärderingskonto](http://azure.microsoft.com/pricing/free-trial/) i bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="62085-118">If you don't have an Azure account, you can create a [free Azure trial account](http://azure.microsoft.com/pricing/free-trial/) in just a few minutes.</span></span>

## <a name="install-python"></a><span data-ttu-id="62085-119">Installera Python</span><span class="sxs-lookup"><span data-stu-id="62085-119">Install Python</span></span>
<span data-ttu-id="62085-120">Även om macOS medföljer Python 2.7 out of box hello, rekommenderar vi att du installerar Python via Homebrew.</span><span class="sxs-lookup"><span data-stu-id="62085-120">Although macOS comes with Python 2.7 out of hello box, we recommend that you install Python through Homebrew.</span></span> <span data-ttu-id="62085-121">Se [installerar Python på macOS](http://docs.python-guide.org/en/latest/starting/install/osx/).</span><span class="sxs-lookup"><span data-stu-id="62085-121">See [Installing Python on macOS](http://docs.python-guide.org/en/latest/starting/install/osx/).</span></span>

<span data-ttu-id="62085-122">Installera Python och pip genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="62085-122">Install Python and pip by running hello following command:</span></span>

```bash
brew install python
```

## <a name="install-hello-azure-cli"></a><span data-ttu-id="62085-123">Installera hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="62085-123">Install hello Azure CLI</span></span>
<span data-ttu-id="62085-124">hello Azure CLI tillhandahåller en flera plattformar kommandoraden för Azure.</span><span class="sxs-lookup"><span data-stu-id="62085-124">hello Azure CLI provides a multiplatform command-line experience for Azure.</span></span> <span data-ttu-id="62085-125">Du arbetar direkt från kommandoraden-tooprovision och hantera resurser.</span><span class="sxs-lookup"><span data-stu-id="62085-125">You work directly from your command line tooprovision and manage resources.</span></span> 

<span data-ttu-id="62085-126">tooinstall Hej senaste Azure CLI, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="62085-126">tooinstall hello latest Azure CLI, follow these steps:</span></span>

1. <span data-ttu-id="62085-127">Kör följande kommandon i ett terminalfönster hello.</span><span class="sxs-lookup"><span data-stu-id="62085-127">Run hello following commands in a terminal window.</span></span> <span data-ttu-id="62085-128">Det kan ta fem minuter tooinstall hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="62085-128">It might take five minutes tooinstall hello Azure CLI.</span></span>

   ```bash
   pip install --upgrade azure-cli
   pip install --upgrade azure-cli-iot
   ```
2. <span data-ttu-id="62085-129">Kontrollera hello installationen genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="62085-129">Verify hello installation by running hello following command:</span></span>

   ```bash
   az iot -h
   ```

<span data-ttu-id="62085-130">Du bör se hello följande utdata om hello-installationen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="62085-130">You should see hello following output if hello installation is successful.</span></span>

![Utdata som indikerar att det lyckades](media/iot-hub-intel-edison-lessons/lesson2/az_iot_help_osx.png)

## <a name="summary"></a><span data-ttu-id="62085-132">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="62085-132">Summary</span></span>
<span data-ttu-id="62085-133">Du har installerat hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="62085-133">You've installed hello Azure CLI.</span></span> <span data-ttu-id="62085-134">Nästa uppgift är toocreate dina Azure IoT hub- och enhetsidentitet med hjälp av hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="62085-134">Your next task is toocreate your Azure IoT hub and device identity by using hello Azure CLI.</span></span>

## <a name="next-steps"></a><span data-ttu-id="62085-135">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="62085-135">Next steps</span></span>
<span data-ttu-id="62085-136">[Skapa din IoT-hubb och registrera Intel modern][create-your-iot-hub-and-register-intel-edison]</span><span class="sxs-lookup"><span data-stu-id="62085-136">[Create your IoT hub and register Intel Edison][create-your-iot-hub-and-register-intel-edison]</span></span>
<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-node-troubleshooting.md
[create-your-iot-hub-and-register-intel-edison]: iot-hub-intel-edison-kit-node-lesson2-prepare-azure-iot-hub.md
[windows]: iot-hub-intel-edison-kit-node-lesson2-get-azure-tools-win32.md
[ubuntu]: iot-hub-intel-edison-kit-node-lesson2-get-azure-tools-ubuntu.md
[macos]: iot-hub-intel-edison-kit-node-lesson2-get-azure-tools-mac.md
