---
title: 'Ansluta Arduino tooAzure IoT - lektionen 2: Azure-verktyg (macOS) | Microsoft Docs'
description: "Installera Python och Azure-kommandoradsgränssnittet (Azure CLI) på macOS."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "Azure cli, iot-Molntjänsten, arduino moln"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started
ms.assetid: 9b719293-01d2-4a2d-9c49-476e67f4816d
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 8f0ec4131e54af5475cd0b4240480c3fda497e14
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-azure-tools-macos-1010"></a><span data-ttu-id="0a2c4-104">Hämta Azure-verktyg (macOS 10.10)</span><span class="sxs-lookup"><span data-stu-id="0a2c4-104">Get Azure tools (macOS 10.10)</span></span>

> [!div class="op_single_selector"]
> * <span data-ttu-id="0a2c4-105">[Windows 7 eller senare][windows]</span><span class="sxs-lookup"><span data-stu-id="0a2c4-105">[Windows 7 or later][windows]</span></span>
> * <span data-ttu-id="0a2c4-106">[Ubuntu 16.04][ubuntu]</span><span class="sxs-lookup"><span data-stu-id="0a2c4-106">[Ubuntu 16.04][ubuntu]</span></span>
> * <span data-ttu-id="0a2c4-107">[macOS 10.10][macos]</span><span class="sxs-lookup"><span data-stu-id="0a2c4-107">[macOS 10.10][macos]</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="0a2c4-108">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="0a2c4-108">What you will do</span></span>

<span data-ttu-id="0a2c4-109">Installera hello Azure-kommandoradsgränssnittet (Azure CLI).</span><span class="sxs-lookup"><span data-stu-id="0a2c4-109">Install hello Azure command-line interface (Azure CLI).</span></span> <span data-ttu-id="0a2c4-110">Om du har några problem med söka efter lösningar på hello [felsökning sidan](iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md) för Adafruit ludd M0 WiFi Arduino-skiva.</span><span class="sxs-lookup"><span data-stu-id="0a2c4-110">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md) for your Adafruit Feather M0 WiFi Arduino board.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="0a2c4-111">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="0a2c4-111">What you will learn</span></span>
<span data-ttu-id="0a2c4-112">I den här artikeln får du lära dig:</span><span class="sxs-lookup"><span data-stu-id="0a2c4-112">In this article, you will learn:</span></span>
* <span data-ttu-id="0a2c4-113">Hur tooinstall Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="0a2c4-113">How tooinstall Azure CLI.</span></span>
* <span data-ttu-id="0a2c4-114">Hur tooadd en IoT-undergrupp till hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="0a2c4-114">How tooadd an IoT subgroup of hello Azure CLI.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="0a2c4-115">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="0a2c4-115">What you need</span></span>
* <span data-ttu-id="0a2c4-116">En Mac med en Internet-anslutning.</span><span class="sxs-lookup"><span data-stu-id="0a2c4-116">A Mac with an Internet connection.</span></span>
* <span data-ttu-id="0a2c4-117">En aktiv Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="0a2c4-117">An active Azure subscription.</span></span> <span data-ttu-id="0a2c4-118">Om du inte har ett Azure-konto kan du skapa en [kostnadsfria Azure utvärderingskonto](http://azure.microsoft.com/pricing/free-trial/) i bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="0a2c4-118">If you don't have an Azure account, you can create a [free Azure trial account](http://azure.microsoft.com/pricing/free-trial/) in just a few minutes.</span></span>

## <a name="install-python"></a><span data-ttu-id="0a2c4-119">Installera Python</span><span class="sxs-lookup"><span data-stu-id="0a2c4-119">Install Python</span></span>
<span data-ttu-id="0a2c4-120">Även om macOS medföljer Python 2.7 out of box hello, rekommenderar vi att du installerar Python via Homebrew.</span><span class="sxs-lookup"><span data-stu-id="0a2c4-120">Although macOS comes with Python 2.7 out of hello box, we recommend that you install Python through Homebrew.</span></span> <span data-ttu-id="0a2c4-121">Se [installerar Python på macOS](http://docs.python-guide.org/en/latest/starting/install/osx/).</span><span class="sxs-lookup"><span data-stu-id="0a2c4-121">See [Installing Python on macOS](http://docs.python-guide.org/en/latest/starting/install/osx/).</span></span>

<span data-ttu-id="0a2c4-122">Installera Python och pip genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="0a2c4-122">Install Python and pip by running hello following command:</span></span>

```bash
brew install python
```

## <a name="install-hello-azure-cli"></a><span data-ttu-id="0a2c4-123">Installera hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="0a2c4-123">Install hello Azure CLI</span></span>
<span data-ttu-id="0a2c4-124">hello Azure CLI tillhandahåller en flera plattformar kommandoraden för Azure.</span><span class="sxs-lookup"><span data-stu-id="0a2c4-124">hello Azure CLI provides a multiplatform command-line experience for Azure.</span></span> <span data-ttu-id="0a2c4-125">Du arbetar direkt från kommandoraden-tooprovision och hantera resurser.</span><span class="sxs-lookup"><span data-stu-id="0a2c4-125">You work directly from your command line tooprovision and manage resources.</span></span>

<span data-ttu-id="0a2c4-126">tooinstall Hej senaste Azure CLI, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="0a2c4-126">tooinstall hello latest Azure CLI, follow these steps:</span></span>

1. <span data-ttu-id="0a2c4-127">Kör följande kommandon i ett terminalfönster hello.</span><span class="sxs-lookup"><span data-stu-id="0a2c4-127">Run hello following commands in a terminal window.</span></span> <span data-ttu-id="0a2c4-128">Det kan ta fem minuter tooinstall hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="0a2c4-128">It might take five minutes tooinstall hello Azure CLI.</span></span>

   ```bash
   pip install --upgrade azure-cli
   pip install --upgrade azure-cli-iot
   ```
2. <span data-ttu-id="0a2c4-129">Kontrollera hello installationen genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="0a2c4-129">Verify hello installation by running hello following command:</span></span>

   ```bash
   az iot -h
   ```

<span data-ttu-id="0a2c4-130">Du bör se hello följande utdata om hello-installationen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="0a2c4-130">You should see hello following output if hello installation is successful.</span></span>

![Utdata som indikerar att det lyckades][output]

## <a name="summary"></a><span data-ttu-id="0a2c4-132">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="0a2c4-132">Summary</span></span>
<span data-ttu-id="0a2c4-133">Du har installerat hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="0a2c4-133">You've installed hello Azure CLI.</span></span> <span data-ttu-id="0a2c4-134">Nästa uppgift är toocreate dina Azure IoT hub- och enhetsidentitet med hjälp av hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="0a2c4-134">Your next task is toocreate your Azure IoT hub and device identity by using hello Azure CLI.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0a2c4-135">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0a2c4-135">Next steps</span></span>
<span data-ttu-id="0a2c4-136">[Skapa din IoT-hubb och registrera Arduino-kort][create-your-iot-hub-and-register-your-arduino-board]</span><span class="sxs-lookup"><span data-stu-id="0a2c4-136">[Create your IoT hub and register your Arduino board][create-your-iot-hub-and-register-your-arduino-board]</span></span>


<!-- Images and links -->

[windows]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-get-azure-tools-win32.md
[ubuntu]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-get-azure-tools-ubuntu.md
[macos]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-get-azure-tools-mac.md
[output]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson2/az_iot_help_osx.png
[create-your-iot-hub-and-register-your-arduino-board]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-prepare-azure-iot-hub.md