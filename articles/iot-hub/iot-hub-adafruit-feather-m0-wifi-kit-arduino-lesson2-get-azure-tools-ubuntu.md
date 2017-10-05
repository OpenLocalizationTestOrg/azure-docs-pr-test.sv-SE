---
title: 'Ansluta Arduino till Azure IoT - lektionen 2: Azure-verktyg (Ubuntu) | Microsoft Docs'
description: "Installera Python och Azure-kommandoradsgränssnittet (Azure CLI) på Ubuntu."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "Azure cli, iot-Molntjänsten, arduino moln"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started
ms.assetid: bb5cb602-7292-4772-ac90-c0b52ebc8340
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: a2f83e59a37abc3f44e770b22ac089b88481a6a4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-azure-tools-ubuntu-1604"></a><span data-ttu-id="659c2-104">Hämta Azure-verktyg (Ubuntu 16.04)</span><span class="sxs-lookup"><span data-stu-id="659c2-104">Get Azure tools (Ubuntu 16.04)</span></span>

> [!div class="op_single_selector"]
> * <span data-ttu-id="659c2-105">[Windows 7 eller senare][windows]</span><span class="sxs-lookup"><span data-stu-id="659c2-105">[Windows 7 or later][windows]</span></span>
> * <span data-ttu-id="659c2-106">[Ubuntu 16.04][ubuntu]</span><span class="sxs-lookup"><span data-stu-id="659c2-106">[Ubuntu 16.04][ubuntu]</span></span>
> * <span data-ttu-id="659c2-107">[macOS 10.10][macos]</span><span class="sxs-lookup"><span data-stu-id="659c2-107">[macOS 10.10][macos]</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="659c2-108">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="659c2-108">What you will do</span></span>

<span data-ttu-id="659c2-109">Installera Azure-kommandoradsgränssnittet (Azure CLI).</span><span class="sxs-lookup"><span data-stu-id="659c2-109">Install the Azure command-line interface (Azure CLI).</span></span> <span data-ttu-id="659c2-110">Om du har några problem kan hitta lösningar på den [felsökning sidan](iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md) för Adafruit ludd M0 WiFi Arduino-skiva.</span><span class="sxs-lookup"><span data-stu-id="659c2-110">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md) for your Adafruit Feather M0 WiFi Arduino board.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="659c2-111">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="659c2-111">What you will learn</span></span>
<span data-ttu-id="659c2-112">I den här artikeln får du lära dig:</span><span class="sxs-lookup"><span data-stu-id="659c2-112">In this article, you will learn:</span></span>
* <span data-ttu-id="659c2-113">Så här installerar du Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="659c2-113">How to install the Azure CLI.</span></span>
* <span data-ttu-id="659c2-114">Hur du lägger till en IoT-undergrupp av Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="659c2-114">How to add an IoT subgroup of the Azure CLI.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="659c2-115">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="659c2-115">What you need</span></span>
* <span data-ttu-id="659c2-116">En Ubuntu-dator med en Internet-anslutning.</span><span class="sxs-lookup"><span data-stu-id="659c2-116">An Ubuntu computer with an Internet connection.</span></span>
* <span data-ttu-id="659c2-117">En aktiv Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="659c2-117">An active Azure subscription.</span></span> <span data-ttu-id="659c2-118">Om du inte har ett konto kan du skapa en [ledigt utvärderingskonto](http://azure.microsoft.com/pricing/free-trial/) i bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="659c2-118">If you don't have an account, you can create a [free trial account](http://azure.microsoft.com/pricing/free-trial/) in just a few minutes.</span></span>

## <a name="install-the-azure-cli"></a><span data-ttu-id="659c2-119">Installera Azure CLI</span><span class="sxs-lookup"><span data-stu-id="659c2-119">Install the Azure CLI</span></span>
<span data-ttu-id="659c2-120">Azure CLI tillhandahåller en flera plattformar kommandoraden för Azure, så att du kan arbeta direkt från kommandoraden för att etablera och hantera resurser.</span><span class="sxs-lookup"><span data-stu-id="659c2-120">The Azure CLI provides a multiplatform command-line experience for Azure, enabling you to work directly from your command line to provision and manage resources.</span></span>

<span data-ttu-id="659c2-121">Följ dessa steg om du vill installera den senaste Azure CLI:</span><span class="sxs-lookup"><span data-stu-id="659c2-121">To install the latest Azure CLI, follow these steps:</span></span>

1. <span data-ttu-id="659c2-122">Kör följande kommandon i ett terminalfönster.</span><span class="sxs-lookup"><span data-stu-id="659c2-122">Run the following commands in a terminal window.</span></span> <span data-ttu-id="659c2-123">Det kan ta fem minuter att installera Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="659c2-123">It might take five minutes to install the Azure CLI.</span></span>

   ```bash
   sudo apt-get update
   sudo apt-get install -y libssl-dev libffi-dev
   sudo apt-get install -y python-dev
   sudo apt-get install -y build-essential
   sudo apt-get install -y python-pip
   sudo pip install --upgrade azure-cli
   sudo pip install --upgrade azure-cli-iot
   ```
2. <span data-ttu-id="659c2-124">Verifiera installationen genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="659c2-124">Verify the installation by running the following command:</span></span>

   ```bash
   az iot -h
   ```

<span data-ttu-id="659c2-125">Du bör se följande utdata om installationen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="659c2-125">You should see the following output if the installation is successful.</span></span>

![Utdata som indikerar att det lyckades][output]

## <a name="summary"></a><span data-ttu-id="659c2-127">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="659c2-127">Summary</span></span>
<span data-ttu-id="659c2-128">Du har installerat Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="659c2-128">You've installed the Azure CLI.</span></span> <span data-ttu-id="659c2-129">Nästa uppgift är att skapa din Azure IoT hub- och enhetsidentitet som med hjälp av Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="659c2-129">Your next task is to create your Azure IoT hub and device identity using the Azure CLI.</span></span>

## <a name="next-steps"></a><span data-ttu-id="659c2-130">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="659c2-130">Next steps</span></span>
<span data-ttu-id="659c2-131">[Skapa din IoT-hubb och registrera Arduino-kort][create-your-iot-hub-and-register-your-arduino-board]</span><span class="sxs-lookup"><span data-stu-id="659c2-131">[Create your IoT hub and register your Arduino board][create-your-iot-hub-and-register-your-arduino-board]</span></span>
<!-- Images and links -->

[windows]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-get-azure-tools-win32.md
[ubuntu]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-get-azure-tools-ubuntu.md
[macos]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-get-azure-tools-mac.md
[output]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson2/az_iot_help_ubuntu.png
[create-your-iot-hub-and-register-your-arduino-board]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-prepare-azure-iot-hub.md