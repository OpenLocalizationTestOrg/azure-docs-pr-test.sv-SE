---
title: 'Connect Intel EDISON (C) till Azure IoT - lektionen 2: Azure-verktyg (Windows) | Microsoft Docs'
description: "Installera Python och Azure-kommandoradsgränssnittet (Azure CLI) för Windows 7 och senare versioner."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "Azure cli, iot-Molntjänsten, arduino moln"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-c-get-started
ms.assetid: 1035760e-cdd1-4d99-8003-067e98b29762
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 90ef113ae84ccc8cb3cbdbe8c245e138320a93c6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-azure-tools-windows-7-and-later"></a><span data-ttu-id="d28e4-104">Hämta Azure-verktyg (Windows 7 och senare)</span><span class="sxs-lookup"><span data-stu-id="d28e4-104">Get Azure tools (Windows 7 and later)</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="d28e4-105">[Windows 7 och senare][windows]</span><span class="sxs-lookup"><span data-stu-id="d28e4-105">[Windows 7 and later][windows]</span></span>
> * <span data-ttu-id="d28e4-106">[Ubuntu 16.04][ubuntu]</span><span class="sxs-lookup"><span data-stu-id="d28e4-106">[Ubuntu 16.04][ubuntu]</span></span>
> * <span data-ttu-id="d28e4-107">[macOS 10.10][macos]</span><span class="sxs-lookup"><span data-stu-id="d28e4-107">[macOS 10.10][macos]</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="d28e4-108">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="d28e4-108">What you will do</span></span>
<span data-ttu-id="d28e4-109">Installera Python och Azure-kommandoradsgränssnittet (Azure CLI).</span><span class="sxs-lookup"><span data-stu-id="d28e4-109">Install Python and the Azure command-line interface (Azure CLI).</span></span> <span data-ttu-id="d28e4-110">Om du har några problem kan hitta lösningar på den [felsökning sidan][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="d28e4-110">If you have any problems, look for solutions on the [troubleshooting page][troubleshooting].</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="d28e4-111">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="d28e4-111">What you will learn</span></span>
<span data-ttu-id="d28e4-112">I den här artikeln får du lära dig:</span><span class="sxs-lookup"><span data-stu-id="d28e4-112">In this article, you will learn:</span></span>
* <span data-ttu-id="d28e4-113">Hur du installerar Python.</span><span class="sxs-lookup"><span data-stu-id="d28e4-113">How to install Python.</span></span>
* <span data-ttu-id="d28e4-114">Så här installerar du Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="d28e4-114">How to install the Azure CLI.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="d28e4-115">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="d28e4-115">What you need</span></span>
* <span data-ttu-id="d28e4-116">En Windows-dator med en Internet-anslutning.</span><span class="sxs-lookup"><span data-stu-id="d28e4-116">A Windows computer with an Internet connection.</span></span>
* <span data-ttu-id="d28e4-117">En aktiv Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="d28e4-117">An active Azure subscription.</span></span> <span data-ttu-id="d28e4-118">Om du inte har ett Azure-konto kan du skapa en [kostnadsfria Azure utvärderingskonto](http://azure.microsoft.com/pricing/free-trial/) i bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="d28e4-118">If you don't have an Azure account, create a [free Azure trial account](http://azure.microsoft.com/pricing/free-trial/) in just a few minutes.</span></span>

## <a name="install-python"></a><span data-ttu-id="d28e4-119">Installera Python</span><span class="sxs-lookup"><span data-stu-id="d28e4-119">Install Python</span></span>
<span data-ttu-id="d28e4-120">[Installera Python](https://www.python.org/downloads/) på din Windows-dator.</span><span class="sxs-lookup"><span data-stu-id="d28e4-120">[Install Python](https://www.python.org/downloads/) on your Windows computer.</span></span> <span data-ttu-id="d28e4-121">Du kan installera Python 2.7, 3.4 eller 3.5.</span><span class="sxs-lookup"><span data-stu-id="d28e4-121">You can install Python 2.7, 3.4 or 3.5.</span></span> <span data-ttu-id="d28e4-122">Den här kursen är baserad på Python 2.7.</span><span class="sxs-lookup"><span data-stu-id="d28e4-122">This tutorial is based on Python 2.7.</span></span> <span data-ttu-id="d28e4-123">Om du redan har installerat Python, gå till nästa avsnitt och installera Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="d28e4-123">If you've already installed Python, go to the next section and install the Azure CLI.</span></span>

<span data-ttu-id="d28e4-124">Du måste också lägga till sökvägen till de mappar där python.exe och pip.exe är installerade i systemet `PATH` miljövariabeln.</span><span class="sxs-lookup"><span data-stu-id="d28e4-124">You also need to add the path of the folders where python.exe and pip.exe are installed to the system `PATH` environment variable.</span></span> <span data-ttu-id="d28e4-125">Som standard installeras python.exe i `C:\Python27` och pip.exe installeras i `C:\Python27\Scripts`.</span><span class="sxs-lookup"><span data-stu-id="d28e4-125">By default, python.exe is installed in `C:\Python27` and pip.exe is installed in `C:\Python27\Scripts`.</span></span>

## <a name="install-the-azure-cli"></a><span data-ttu-id="d28e4-126">Installera Azure CLI</span><span class="sxs-lookup"><span data-stu-id="d28e4-126">Install the Azure CLI</span></span>
<span data-ttu-id="d28e4-127">Azure CLI tillhandahåller en flera plattformar kommandoraden för Azure.</span><span class="sxs-lookup"><span data-stu-id="d28e4-127">The Azure CLI provides a multiplatform command-line experience for Azure.</span></span> <span data-ttu-id="d28e4-128">Du arbetar direkt från kommandoraden för att etablera och hantera resurser.</span><span class="sxs-lookup"><span data-stu-id="d28e4-128">You work directly from your command line to provision and manage resources.</span></span>

<span data-ttu-id="d28e4-129">Så här installerar du Azure CLI:</span><span class="sxs-lookup"><span data-stu-id="d28e4-129">To install the Azure CLI, follow these steps:</span></span>

1. <span data-ttu-id="d28e4-130">Öppna ett kommandotolksfönster som administratör.</span><span class="sxs-lookup"><span data-stu-id="d28e4-130">Open a Command Prompt window as an administrator.</span></span>
2. <span data-ttu-id="d28e4-131">Kör följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="d28e4-131">Run the following commands:</span></span>

   ```cmd
   pip install --upgrade azure-cli
   pip install --upgrade azure-cli-iot
   ```
3. <span data-ttu-id="d28e4-132">Verifiera installationen genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="d28e4-132">Verify the installation by running the following command:</span></span>

   ```cmd
   az iot -h
   ```

<span data-ttu-id="d28e4-133">Du ser i följande utdata om installationen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="d28e4-133">You see the following output if the installation is successful.</span></span>

![Utdata som indikerar att det lyckades](media/iot-hub-intel-edison-lessons/lesson2/az_iot_help_win.png)

## <a name="summary"></a><span data-ttu-id="d28e4-135">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="d28e4-135">Summary</span></span>
<span data-ttu-id="d28e4-136">Du har installerat Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="d28e4-136">You've installed the Azure CLI.</span></span> <span data-ttu-id="d28e4-137">Nästa uppgift att skapa din Azure IoT hub- och enhetsidentitet med Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="d28e4-137">Your next task to create your Azure IoT hub and device identity by using the Azure CLI.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d28e4-138">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d28e4-138">Next steps</span></span>
<span data-ttu-id="d28e4-139">[Skapa din IoT-hubb och registrera Intel modern][create-your-iot-hub-and-register-intel-edison]</span><span class="sxs-lookup"><span data-stu-id="d28e4-139">[Create your IoT hub and register Intel Edison][create-your-iot-hub-and-register-intel-edison]</span></span>
<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-c-troubleshooting.md
[create-your-iot-hub-and-register-intel-edison]: iot-hub-intel-edison-kit-c-lesson2-prepare-azure-iot-hub.md
[windows]: iot-hub-intel-edison-kit-c-lesson2-get-azure-tools-win32.md
[ubuntu]: iot-hub-intel-edison-kit-c-lesson2-get-azure-tools-ubuntu.md
[macos]: iot-hub-intel-edison-kit-c-lesson2-get-azure-tools-mac.md
