---
title: 'Connect Intel EDISON (C) till Azure IoT - lektionen 2: Azure-verktyg (macOS) | Microsoft Docs'
description: "Installera Python och Azure-kommandoradsgränssnittet (Azure CLI) på macOS."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "Azure cli, iot-Molntjänsten, arduino moln"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-c-get-started
ms.assetid: d561680f-69cc-427a-820d-24f710ba05a8
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 97beb04781f3f9809cdafc945d9499e71cbbd9b1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-azure-tools-macos-1010"></a><span data-ttu-id="e31f6-104">Hämta Azure-verktyg (macOS 10.10)</span><span class="sxs-lookup"><span data-stu-id="e31f6-104">Get Azure tools (macOS 10.10)</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="e31f6-105">[Windows 7 och senare][windows]</span><span class="sxs-lookup"><span data-stu-id="e31f6-105">[Windows 7 and later][windows]</span></span>
> * <span data-ttu-id="e31f6-106">[Ubuntu 16.04][ubuntu]</span><span class="sxs-lookup"><span data-stu-id="e31f6-106">[Ubuntu 16.04][ubuntu]</span></span>
> * <span data-ttu-id="e31f6-107">[macOS 10.10][macos]</span><span class="sxs-lookup"><span data-stu-id="e31f6-107">[macOS 10.10][macos]</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="e31f6-108">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="e31f6-108">What you will do</span></span>
<span data-ttu-id="e31f6-109">Installera Azure-kommandoradsgränssnittet (Azure CLI).</span><span class="sxs-lookup"><span data-stu-id="e31f6-109">Install the Azure command-line interface (Azure CLI).</span></span> <span data-ttu-id="e31f6-110">Om du har några problem kan hitta lösningar på den [felsökning sidan][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="e31f6-110">If you have any problems, look for solutions on the [troubleshooting page][troubleshooting].</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="e31f6-111">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="e31f6-111">What you will learn</span></span>
<span data-ttu-id="e31f6-112">I den här artikeln får du lära dig:</span><span class="sxs-lookup"><span data-stu-id="e31f6-112">In this article, you will learn:</span></span>
* <span data-ttu-id="e31f6-113">Så här installerar du Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="e31f6-113">How to install Azure CLI.</span></span>
* <span data-ttu-id="e31f6-114">Hur du lägger till en IoT-undergrupp av Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="e31f6-114">How to add an IoT subgroup of the Azure CLI.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="e31f6-115">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="e31f6-115">What you need</span></span>
* <span data-ttu-id="e31f6-116">En Mac med en Internet-anslutning.</span><span class="sxs-lookup"><span data-stu-id="e31f6-116">A Mac with an Internet connection.</span></span>
* <span data-ttu-id="e31f6-117">En aktiv Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="e31f6-117">An active Azure subscription.</span></span> <span data-ttu-id="e31f6-118">Om du inte har ett Azure-konto kan du skapa en [kostnadsfria Azure utvärderingskonto](http://azure.microsoft.com/pricing/free-trial/) i bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="e31f6-118">If you don't have an Azure account, you can create a [free Azure trial account](http://azure.microsoft.com/pricing/free-trial/) in just a few minutes.</span></span>

## <a name="install-python"></a><span data-ttu-id="e31f6-119">Installera Python</span><span class="sxs-lookup"><span data-stu-id="e31f6-119">Install Python</span></span>
<span data-ttu-id="e31f6-120">Även om macOS medföljer Python 2.7 direkt, rekommenderar vi att du installerar Python via Homebrew.</span><span class="sxs-lookup"><span data-stu-id="e31f6-120">Although macOS comes with Python 2.7 out of the box, we recommend that you install Python through Homebrew.</span></span> <span data-ttu-id="e31f6-121">Se [installerar Python på macOS](http://docs.python-guide.org/en/latest/starting/install/osx/).</span><span class="sxs-lookup"><span data-stu-id="e31f6-121">See [Installing Python on macOS](http://docs.python-guide.org/en/latest/starting/install/osx/).</span></span>

<span data-ttu-id="e31f6-122">Installera Python och pip genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="e31f6-122">Install Python and pip by running the following command:</span></span>

```bash
brew install python
```

## <a name="install-the-azure-cli"></a><span data-ttu-id="e31f6-123">Installera Azure CLI</span><span class="sxs-lookup"><span data-stu-id="e31f6-123">Install the Azure CLI</span></span>
<span data-ttu-id="e31f6-124">Azure CLI tillhandahåller en flera plattformar kommandoraden för Azure.</span><span class="sxs-lookup"><span data-stu-id="e31f6-124">The Azure CLI provides a multiplatform command-line experience for Azure.</span></span> <span data-ttu-id="e31f6-125">Du arbetar direkt från kommandoraden för att etablera och hantera resurser.</span><span class="sxs-lookup"><span data-stu-id="e31f6-125">You work directly from your command line to provision and manage resources.</span></span> 

<span data-ttu-id="e31f6-126">Följ dessa steg om du vill installera den senaste Azure CLI:</span><span class="sxs-lookup"><span data-stu-id="e31f6-126">To install the latest Azure CLI, follow these steps:</span></span>

1. <span data-ttu-id="e31f6-127">Kör följande kommandon i ett terminalfönster.</span><span class="sxs-lookup"><span data-stu-id="e31f6-127">Run the following commands in a terminal window.</span></span> <span data-ttu-id="e31f6-128">Det kan ta fem minuter att installera Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="e31f6-128">It might take five minutes to install the Azure CLI.</span></span>

   ```bash
   pip install --upgrade azure-cli
   pip install --upgrade azure-cli-iot
   ```
2. <span data-ttu-id="e31f6-129">Verifiera installationen genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="e31f6-129">Verify the installation by running the following command:</span></span>

   ```bash
   az iot -h
   ```

<span data-ttu-id="e31f6-130">Du bör se följande utdata om installationen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="e31f6-130">You should see the following output if the installation is successful.</span></span>

![Utdata som indikerar att det lyckades](media/iot-hub-intel-edison-lessons/lesson2/az_iot_help_osx.png)

## <a name="summary"></a><span data-ttu-id="e31f6-132">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="e31f6-132">Summary</span></span>
<span data-ttu-id="e31f6-133">Du har installerat Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="e31f6-133">You've installed the Azure CLI.</span></span> <span data-ttu-id="e31f6-134">Nästa uppgift är att skapa din Azure IoT hub- och enhetsidentitet med Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="e31f6-134">Your next task is to create your Azure IoT hub and device identity by using the Azure CLI.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e31f6-135">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e31f6-135">Next steps</span></span>
<span data-ttu-id="e31f6-136">[Skapa din IoT-hubb och registrera Intel modern][create-your-iot-hub-and-register-intel-edison]</span><span class="sxs-lookup"><span data-stu-id="e31f6-136">[Create your IoT hub and register Intel Edison][create-your-iot-hub-and-register-intel-edison]</span></span>
<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-c-troubleshooting.md
[create-your-iot-hub-and-register-intel-edison]: iot-hub-intel-edison-kit-c-lesson2-prepare-azure-iot-hub.md
[windows]: iot-hub-intel-edison-kit-c-lesson2-get-azure-tools-win32.md
[ubuntu]: iot-hub-intel-edison-kit-c-lesson2-get-azure-tools-ubuntu.md
[macos]: iot-hub-intel-edison-kit-c-lesson2-get-azure-tools-mac.md
