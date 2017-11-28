---
title: 'Connect Intel EDISON (C) till Azure IoT - lektionen 2: Azure-verktyg (Ubuntu) | Microsoft Docs'
description: "Installera Python och Azure-kommandoradsgränssnittet (Azure CLI) på Ubuntu."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "Azure cli, iot-Molntjänsten, arduino moln"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-c-get-started
ms.assetid: 2463cb8e-5758-4d72-af98-62520d41f2f7
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 897ab63af85a1f830ed49084ce7c3e74c84a4cc9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-azure-tools-ubuntu-1604"></a><span data-ttu-id="8b3a8-104">Hämta Azure-verktyg (Ubuntu 16.04)</span><span class="sxs-lookup"><span data-stu-id="8b3a8-104">Get Azure tools (Ubuntu 16.04)</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="8b3a8-105">[Windows 7 och senare][windows]</span><span class="sxs-lookup"><span data-stu-id="8b3a8-105">[Windows 7 and later][windows]</span></span>
> * <span data-ttu-id="8b3a8-106">[Ubuntu 16.04][ubuntu]</span><span class="sxs-lookup"><span data-stu-id="8b3a8-106">[Ubuntu 16.04][ubuntu]</span></span>
> * <span data-ttu-id="8b3a8-107">[macOS 10.10][macos]</span><span class="sxs-lookup"><span data-stu-id="8b3a8-107">[macOS 10.10][macos]</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="8b3a8-108">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="8b3a8-108">What you will do</span></span>
<span data-ttu-id="8b3a8-109">Installera Azure-kommandoradsgränssnittet (Azure CLI).</span><span class="sxs-lookup"><span data-stu-id="8b3a8-109">Install the Azure command-line interface (Azure CLI).</span></span> <span data-ttu-id="8b3a8-110">Om du har några problem kan hitta lösningar på den [felsökning sidan][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="8b3a8-110">If you have any problems, look for solutions on the [troubleshooting page][troubleshooting].</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="8b3a8-111">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="8b3a8-111">What you will learn</span></span>
<span data-ttu-id="8b3a8-112">I den här artikeln får du lära dig:</span><span class="sxs-lookup"><span data-stu-id="8b3a8-112">In this article, you will learn:</span></span>
* <span data-ttu-id="8b3a8-113">Så här installerar du Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="8b3a8-113">How to install the Azure CLI.</span></span>
* <span data-ttu-id="8b3a8-114">Hur du lägger till en IoT-undergrupp av Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="8b3a8-114">How to add an IoT subgroup of the Azure CLI.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="8b3a8-115">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="8b3a8-115">What you need</span></span>
* <span data-ttu-id="8b3a8-116">En Ubuntu-dator med en Internet-anslutning.</span><span class="sxs-lookup"><span data-stu-id="8b3a8-116">An Ubuntu computer with an Internet connection.</span></span>
* <span data-ttu-id="8b3a8-117">En aktiv Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="8b3a8-117">An active Azure subscription.</span></span> <span data-ttu-id="8b3a8-118">Om du inte har ett konto kan du skapa en [ledigt utvärderingskonto](http://azure.microsoft.com/pricing/free-trial/) i bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="8b3a8-118">If you don't have an account, you can create a [free trial account](http://azure.microsoft.com/pricing/free-trial/) in just a few minutes.</span></span>

## <a name="install-the-azure-cli"></a><span data-ttu-id="8b3a8-119">Installera Azure CLI</span><span class="sxs-lookup"><span data-stu-id="8b3a8-119">Install the Azure CLI</span></span>
<span data-ttu-id="8b3a8-120">Azure CLI tillhandahåller en flera plattformar kommandoraden för Azure, så att du kan arbeta direkt från kommandoraden för att etablera och hantera resurser.</span><span class="sxs-lookup"><span data-stu-id="8b3a8-120">The Azure CLI provides a multiplatform command-line experience for Azure, enabling you to work directly from your command line to provision and manage resources.</span></span>

<span data-ttu-id="8b3a8-121">Följ dessa steg om du vill installera den senaste Azure CLI:</span><span class="sxs-lookup"><span data-stu-id="8b3a8-121">To install the latest Azure CLI, follow these steps:</span></span>

1. <span data-ttu-id="8b3a8-122">Kör följande kommandon i ett terminalfönster.</span><span class="sxs-lookup"><span data-stu-id="8b3a8-122">Run the following commands in a terminal window.</span></span> <span data-ttu-id="8b3a8-123">Det kan ta fem minuter att installera Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="8b3a8-123">It might take five minutes to install the Azure CLI.</span></span>

   ```bash
   sudo apt-get update
   sudo apt-get install -y libssl-dev libffi-dev
   sudo apt-get install -y python-dev
   sudo apt-get install -y build-essential
   sudo apt-get install -y python-pip
   sudo pip install --upgrade azure-cli
   sudo pip install --upgrade azure-cli-iot
   ```
2. <span data-ttu-id="8b3a8-124">Verifiera installationen genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="8b3a8-124">Verify the installation by running the following command:</span></span>

   ```bash
   az iot -h
   ```

<span data-ttu-id="8b3a8-125">Du bör se följande utdata om installationen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="8b3a8-125">You should see the following output if the installation is successful.</span></span>

![Utdata som indikerar att det lyckades](media/iot-hub-intel-edison-lessons/lesson2/az_iot_help_ubuntu.png)

## <a name="summary"></a><span data-ttu-id="8b3a8-127">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="8b3a8-127">Summary</span></span>
<span data-ttu-id="8b3a8-128">Du har installerat Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="8b3a8-128">You've installed the Azure CLI.</span></span> <span data-ttu-id="8b3a8-129">Nästa uppgift är att skapa din Azure IoT hub- och enhetsidentitet som med hjälp av Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="8b3a8-129">Your next task is to create your Azure IoT hub and device identity using the Azure CLI.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8b3a8-130">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8b3a8-130">Next steps</span></span>
<span data-ttu-id="8b3a8-131">[Skapa din IoT-hubb och registrera Intel modern][create-your-iot-hub-and-register-intel-edison]</span><span class="sxs-lookup"><span data-stu-id="8b3a8-131">[Create your IoT hub and register Intel Edison][create-your-iot-hub-and-register-intel-edison]</span></span>


<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-c-troubleshooting.md
[create-your-iot-hub-and-register-intel-edison]: iot-hub-intel-edison-kit-c-lesson2-prepare-azure-iot-hub.md
[windows]: iot-hub-intel-edison-kit-c-lesson2-get-azure-tools-win32.md
[ubuntu]: iot-hub-intel-edison-kit-c-lesson2-get-azure-tools-ubuntu.md
[macos]: iot-hub-intel-edison-kit-c-lesson2-get-azure-tools-mac.md
