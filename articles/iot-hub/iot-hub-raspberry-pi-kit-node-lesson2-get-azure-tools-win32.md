---
title: "Ansluta hallon Pi (nod) till Azure IoT - lektionen 2: Hämta verktyg (Windows) | Microsoft Docs"
description: "Installera Python och Azure-kommandoradsgränssnittet (Azure CLI) för Windows 7 och senare versioner."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: IOT cloud service, azure cli
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started
ms.assetid: acfa13e3-6d2c-4e68-9a77-1cbc2cf3f9c1
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: c7b927997b738f7a80b71c06d4dff2278dc7c64e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-azure-tools-windows-7-and-later"></a><span data-ttu-id="137bb-104">Hämta Azure-verktyg (Windows 7 och senare)</span><span class="sxs-lookup"><span data-stu-id="137bb-104">Get Azure tools (Windows 7 and later)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="137bb-105">Windows 7 och senare</span><span class="sxs-lookup"><span data-stu-id="137bb-105">Windows 7 and later</span></span>](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-win32.md)
> * [<span data-ttu-id="137bb-106">Ubuntu 16.04</span><span class="sxs-lookup"><span data-stu-id="137bb-106">Ubuntu 16.04</span></span>](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-ubuntu.md)
> * [<span data-ttu-id="137bb-107">macOS 10.10</span><span class="sxs-lookup"><span data-stu-id="137bb-107">macOS 10.10</span></span>](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-mac.md)

## <a name="what-you-will-do"></a><span data-ttu-id="137bb-108">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="137bb-108">What you will do</span></span>
<span data-ttu-id="137bb-109">Installera Python och Azure-kommandoradsgränssnittet (Azure CLI).</span><span class="sxs-lookup"><span data-stu-id="137bb-109">Install Python and the Azure command-line interface (Azure CLI).</span></span> <span data-ttu-id="137bb-110">Om du har några problem kan hitta lösningar på den [felsökning sidan](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="137bb-110">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="137bb-111">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="137bb-111">What you will learn</span></span>
<span data-ttu-id="137bb-112">I den här artikeln får du lära dig:</span><span class="sxs-lookup"><span data-stu-id="137bb-112">In this article, you will learn:</span></span>
* <span data-ttu-id="137bb-113">Hur du installerar Python.</span><span class="sxs-lookup"><span data-stu-id="137bb-113">How to install Python.</span></span>
* <span data-ttu-id="137bb-114">Så här installerar du Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="137bb-114">How to install the Azure CLI.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="137bb-115">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="137bb-115">What you need</span></span>
* <span data-ttu-id="137bb-116">En Windows-dator med en Internet-anslutning.</span><span class="sxs-lookup"><span data-stu-id="137bb-116">A Windows computer with an Internet connection.</span></span>
* <span data-ttu-id="137bb-117">En aktiv Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="137bb-117">An active Azure subscription.</span></span> <span data-ttu-id="137bb-118">Om du inte har ett Azure-konto kan du skapa en [kostnadsfria Azure utvärderingskonto](http://azure.microsoft.com/pricing/free-trial/) i bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="137bb-118">If you don't have an Azure account, create a [free Azure trial account](http://azure.microsoft.com/pricing/free-trial/) in just a few minutes.</span></span>

## <a name="install-python"></a><span data-ttu-id="137bb-119">Installera Python</span><span class="sxs-lookup"><span data-stu-id="137bb-119">Install Python</span></span>
<span data-ttu-id="137bb-120">[Installera Python](https://www.python.org/downloads/) på din Windows-dator.</span><span class="sxs-lookup"><span data-stu-id="137bb-120">[Install Python](https://www.python.org/downloads/) on your Windows computer.</span></span> <span data-ttu-id="137bb-121">Du kan installera Python 2.7, 3.4 eller 3.5.</span><span class="sxs-lookup"><span data-stu-id="137bb-121">You can install Python 2.7, 3.4 or 3.5.</span></span> <span data-ttu-id="137bb-122">Den här kursen är baserad på Python 2.7.</span><span class="sxs-lookup"><span data-stu-id="137bb-122">This tutorial is based on Python 2.7.</span></span> <span data-ttu-id="137bb-123">Om du redan har installerat Python, gå till nästa avsnitt och installera Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="137bb-123">If you've already installed Python, go to the next section and install the Azure CLI.</span></span>

<span data-ttu-id="137bb-124">Du måste också lägga till sökvägen till de mappar där python.exe och pip.exe är installerade i systemet `PATH` miljövariabeln.</span><span class="sxs-lookup"><span data-stu-id="137bb-124">You also need to add the path of the folders where python.exe and pip.exe are installed to the system `PATH` environment variable.</span></span> <span data-ttu-id="137bb-125">Som standard installeras python.exe i `C:\Python27` och pip.exe installeras i `C:\Python27\Scripts`.</span><span class="sxs-lookup"><span data-stu-id="137bb-125">By default, python.exe is installed in `C:\Python27` and pip.exe is installed in `C:\Python27\Scripts`.</span></span>

## <a name="install-the-azure-cli"></a><span data-ttu-id="137bb-126">Installera Azure CLI</span><span class="sxs-lookup"><span data-stu-id="137bb-126">Install the Azure CLI</span></span>
<span data-ttu-id="137bb-127">Azure CLI tillhandahåller en flera plattformar kommandoraden för Azure.</span><span class="sxs-lookup"><span data-stu-id="137bb-127">The Azure CLI provides a multiplatform command-line experience for Azure.</span></span> <span data-ttu-id="137bb-128">Du arbetar direkt från din för att etablera och hantera resurser.</span><span class="sxs-lookup"><span data-stu-id="137bb-128">You work directly from your command-line to provision and manage resources.</span></span>

<span data-ttu-id="137bb-129">Så här installerar du Azure CLI:</span><span class="sxs-lookup"><span data-stu-id="137bb-129">To install the Azure CLI, follow these steps:</span></span>

1. <span data-ttu-id="137bb-130">Öppna ett kommandotolksfönster som administratör.</span><span class="sxs-lookup"><span data-stu-id="137bb-130">Open a Command Prompt window as an administrator.</span></span>
2. <span data-ttu-id="137bb-131">Kör följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="137bb-131">Run the following commands:</span></span>

   ```bash
   pip install --upgrade azure-cli
   pip install --upgrade azure-cli-iot
   ```
3. <span data-ttu-id="137bb-132">Verifiera installationen genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="137bb-132">Verify the installation by running the following command:</span></span>

   ```bash
   az iot -h
   ```

<span data-ttu-id="137bb-133">Du ser i följande utdata om installationen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="137bb-133">You see the following output if the installation is successful.</span></span>

![Utdata som indikerar att det lyckades](media/iot-hub-raspberry-pi-lessons/lesson2/az_iot_help_win.png)

## <a name="summary"></a><span data-ttu-id="137bb-135">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="137bb-135">Summary</span></span>
<span data-ttu-id="137bb-136">Du har installerat Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="137bb-136">You've installed the Azure CLI.</span></span> <span data-ttu-id="137bb-137">Nästa uppgift att skapa din Azure IoT hub- och enhetsidentitet med Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="137bb-137">Your next task to create your Azure IoT hub and device identity by using the Azure CLI.</span></span>

## <a name="next-steps"></a><span data-ttu-id="137bb-138">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="137bb-138">Next steps</span></span>
[<span data-ttu-id="137bb-139">Skapa din IoT-hubb och registrera hallon Pi 3</span><span class="sxs-lookup"><span data-stu-id="137bb-139">Create your IoT hub and register Raspberry Pi 3</span></span>](iot-hub-raspberry-pi-kit-node-lesson2-prepare-azure-iot-hub.md)

