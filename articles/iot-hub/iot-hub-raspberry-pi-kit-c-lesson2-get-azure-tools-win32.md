---
title: 'Connect Raspberry PI (C) till Azure IoT - lektionen 2: Azure-verktyg (Windows) | Microsoft Docs'
description: "Installera Python och Azure-kommandoradsgränssnittet (Azure CLI) för Windows 7 och senare versioner."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: IOT cloud service, azure cli
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-c-get-started
ms.assetid: a3c083b5-0d76-46af-bc77-2ad7d8aadc1e
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: aa96000cb676c088a90f2b3d45c159913185a2e1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-azure-tools-windows-7-and-later"></a><span data-ttu-id="ef5d3-104">Hämta Azure-verktyg (Windows 7 och senare)</span><span class="sxs-lookup"><span data-stu-id="ef5d3-104">Get Azure tools (Windows 7 and later)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="ef5d3-105">Windows 7 och senare</span><span class="sxs-lookup"><span data-stu-id="ef5d3-105">Windows 7 and later</span></span>](iot-hub-raspberry-pi-kit-c-lesson2-get-azure-tools-win32.md)
> * [<span data-ttu-id="ef5d3-106">Ubuntu 16.04</span><span class="sxs-lookup"><span data-stu-id="ef5d3-106">Ubuntu 16.04</span></span>](iot-hub-raspberry-pi-kit-c-lesson2-get-azure-tools-ubuntu.md)
> * [<span data-ttu-id="ef5d3-107">macOS 10.10</span><span class="sxs-lookup"><span data-stu-id="ef5d3-107">macOS 10.10</span></span>](iot-hub-raspberry-pi-kit-c-lesson2-get-azure-tools-mac.md)

## <a name="what-you-will-do"></a><span data-ttu-id="ef5d3-108">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="ef5d3-108">What you will do</span></span>
<span data-ttu-id="ef5d3-109">Installera Python och Azure-kommandoradsgränssnittet (Azure CLI).</span><span class="sxs-lookup"><span data-stu-id="ef5d3-109">Install Python and the Azure command-line interface (Azure CLI).</span></span> <span data-ttu-id="ef5d3-110">Om du har några problem kan hitta lösningar på den [felsökning sidan](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="ef5d3-110">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="ef5d3-111">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="ef5d3-111">What you will learn</span></span>
<span data-ttu-id="ef5d3-112">I den här artikeln får du lära dig:</span><span class="sxs-lookup"><span data-stu-id="ef5d3-112">In this article, you will learn:</span></span>
* <span data-ttu-id="ef5d3-113">Hur du installerar Python.</span><span class="sxs-lookup"><span data-stu-id="ef5d3-113">How to install Python.</span></span>
* <span data-ttu-id="ef5d3-114">Så här installerar du Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="ef5d3-114">How to install the Azure CLI.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="ef5d3-115">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="ef5d3-115">What you need</span></span>
* <span data-ttu-id="ef5d3-116">En Windows-dator med en Internet-anslutning.</span><span class="sxs-lookup"><span data-stu-id="ef5d3-116">A Windows computer with an Internet connection.</span></span>
* <span data-ttu-id="ef5d3-117">En aktiv Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="ef5d3-117">An active Azure subscription.</span></span> <span data-ttu-id="ef5d3-118">Om du inte har ett Azure-konto kan du skapa en [kostnadsfria Azure utvärderingskonto](http://azure.microsoft.com/pricing/free-trial/) i bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="ef5d3-118">If you don't have an Azure account, create a [free Azure trial account](http://azure.microsoft.com/pricing/free-trial/) in just a few minutes.</span></span>

## <a name="install-python"></a><span data-ttu-id="ef5d3-119">Installera Python</span><span class="sxs-lookup"><span data-stu-id="ef5d3-119">Install Python</span></span>
<span data-ttu-id="ef5d3-120">[Installera Python](https://www.python.org/downloads/) på din Windows-dator.</span><span class="sxs-lookup"><span data-stu-id="ef5d3-120">[Install Python](https://www.python.org/downloads/) on your Windows computer.</span></span> <span data-ttu-id="ef5d3-121">Du kan installera Python 2.7, 3.4 eller 3.5.</span><span class="sxs-lookup"><span data-stu-id="ef5d3-121">You can install Python 2.7, 3.4 or 3.5.</span></span> <span data-ttu-id="ef5d3-122">Den här kursen är baserad på Python 2.7.</span><span class="sxs-lookup"><span data-stu-id="ef5d3-122">This tutorial is based on Python 2.7.</span></span> <span data-ttu-id="ef5d3-123">Om du redan har installerat Python, gå till nästa avsnitt och installera Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="ef5d3-123">If you've already installed Python, go to the next section and install the Azure CLI.</span></span>

<span data-ttu-id="ef5d3-124">Du måste också lägga till sökvägen till de mappar där python.exe och pip.exe är installerade i systemet `PATH` miljövariabeln.</span><span class="sxs-lookup"><span data-stu-id="ef5d3-124">You also need to add the path of the folders where python.exe and pip.exe are installed to the system `PATH` environment variable.</span></span> <span data-ttu-id="ef5d3-125">Som standard installeras python.exe i `C:\Python27` och pip.exe installeras i `C:\Python27\Scripts`.</span><span class="sxs-lookup"><span data-stu-id="ef5d3-125">By default, python.exe is installed in `C:\Python27` and pip.exe is installed in `C:\Python27\Scripts`.</span></span>

## <a name="install-the-azure-cli"></a><span data-ttu-id="ef5d3-126">Installera Azure CLI</span><span class="sxs-lookup"><span data-stu-id="ef5d3-126">Install the Azure CLI</span></span>
<span data-ttu-id="ef5d3-127">Azure CLI tillhandahåller en flera plattformar kommandoraden för Azure.</span><span class="sxs-lookup"><span data-stu-id="ef5d3-127">The Azure CLI provides a multiplatform command-line experience for Azure.</span></span> <span data-ttu-id="ef5d3-128">Du arbetar direkt från kommandoraden för att etablera och hantera resurser.</span><span class="sxs-lookup"><span data-stu-id="ef5d3-128">You work directly from your command line to provision and manage resources.</span></span>

<span data-ttu-id="ef5d3-129">Så här installerar du Azure CLI:</span><span class="sxs-lookup"><span data-stu-id="ef5d3-129">To install the Azure CLI, follow these steps:</span></span>

1. <span data-ttu-id="ef5d3-130">Öppna ett kommandotolksfönster som administratör.</span><span class="sxs-lookup"><span data-stu-id="ef5d3-130">Open a Command Prompt window as an administrator.</span></span>
2. <span data-ttu-id="ef5d3-131">Kör följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="ef5d3-131">Run the following commands:</span></span>

   ```bash
   pip install --upgrade azure-cli
   pip install --upgrade azure-cli-iot
   ```
3. <span data-ttu-id="ef5d3-132">Verifiera installationen genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="ef5d3-132">Verify the installation by running the following command:</span></span>

   ```bash
   az iot -h
   ```

<span data-ttu-id="ef5d3-133">Du ser i följande utdata om installationen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="ef5d3-133">You see the following output if the installation is successful.</span></span>

![Utdata som indikerar att det lyckades](media/iot-hub-raspberry-pi-lessons/lesson2/az_iot_help_win.png)

## <a name="summary"></a><span data-ttu-id="ef5d3-135">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="ef5d3-135">Summary</span></span>
<span data-ttu-id="ef5d3-136">Du har installerat Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="ef5d3-136">You've installed the Azure CLI.</span></span> <span data-ttu-id="ef5d3-137">Nästa uppgift att skapa din Azure IoT hub- och enhetsidentitet med Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="ef5d3-137">Your next task to create your Azure IoT hub and device identity by using the Azure CLI.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ef5d3-138">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ef5d3-138">Next steps</span></span>
[<span data-ttu-id="ef5d3-139">Skapa din IoT-hubb och registrera hallon Pi 3</span><span class="sxs-lookup"><span data-stu-id="ef5d3-139">Create your IoT hub and register Raspberry Pi 3</span></span>](iot-hub-raspberry-pi-kit-c-lesson2-prepare-azure-iot-hub.md)

