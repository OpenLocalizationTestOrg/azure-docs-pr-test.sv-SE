---
title: "Ansluta hallon Pi (nod) tooAzure IoT - lektionen 2: Hämta verktyg (Windows) | Microsoft Docs"
description: "Installera Python och hello Azure-kommandoradsgränssnittet (Azure CLI) för Windows 7 och senare versioner."
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
ms.openlocfilehash: 25b50214322137ea32861fd1131c474e2fc7ebb7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-azure-tools-windows-7-and-later"></a><span data-ttu-id="43494-104">Hämta Azure-verktyg (Windows 7 och senare)</span><span class="sxs-lookup"><span data-stu-id="43494-104">Get Azure tools (Windows 7 and later)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="43494-105">Windows 7 och senare</span><span class="sxs-lookup"><span data-stu-id="43494-105">Windows 7 and later</span></span>](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-win32.md)
> * [<span data-ttu-id="43494-106">Ubuntu 16.04</span><span class="sxs-lookup"><span data-stu-id="43494-106">Ubuntu 16.04</span></span>](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-ubuntu.md)
> * [<span data-ttu-id="43494-107">macOS 10.10</span><span class="sxs-lookup"><span data-stu-id="43494-107">macOS 10.10</span></span>](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-mac.md)

## <a name="what-you-will-do"></a><span data-ttu-id="43494-108">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="43494-108">What you will do</span></span>
<span data-ttu-id="43494-109">Installera Python och hello Azure-kommandoradsgränssnittet (Azure CLI).</span><span class="sxs-lookup"><span data-stu-id="43494-109">Install Python and hello Azure command-line interface (Azure CLI).</span></span> <span data-ttu-id="43494-110">Om du har några problem med söka efter lösningar på hello [felsökning sidan](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="43494-110">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="43494-111">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="43494-111">What you will learn</span></span>
<span data-ttu-id="43494-112">I den här artikeln får du lära dig:</span><span class="sxs-lookup"><span data-stu-id="43494-112">In this article, you will learn:</span></span>
* <span data-ttu-id="43494-113">Hur tooinstall Python.</span><span class="sxs-lookup"><span data-stu-id="43494-113">How tooinstall Python.</span></span>
* <span data-ttu-id="43494-114">Hur tooinstall hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="43494-114">How tooinstall hello Azure CLI.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="43494-115">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="43494-115">What you need</span></span>
* <span data-ttu-id="43494-116">En Windows-dator med en Internet-anslutning.</span><span class="sxs-lookup"><span data-stu-id="43494-116">A Windows computer with an Internet connection.</span></span>
* <span data-ttu-id="43494-117">En aktiv Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="43494-117">An active Azure subscription.</span></span> <span data-ttu-id="43494-118">Om du inte har ett Azure-konto kan du skapa en [kostnadsfria Azure utvärderingskonto](http://azure.microsoft.com/pricing/free-trial/) i bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="43494-118">If you don't have an Azure account, create a [free Azure trial account](http://azure.microsoft.com/pricing/free-trial/) in just a few minutes.</span></span>

## <a name="install-python"></a><span data-ttu-id="43494-119">Installera Python</span><span class="sxs-lookup"><span data-stu-id="43494-119">Install Python</span></span>
<span data-ttu-id="43494-120">[Installera Python](https://www.python.org/downloads/) på din Windows-dator.</span><span class="sxs-lookup"><span data-stu-id="43494-120">[Install Python](https://www.python.org/downloads/) on your Windows computer.</span></span> <span data-ttu-id="43494-121">Du kan installera Python 2.7, 3.4 eller 3.5.</span><span class="sxs-lookup"><span data-stu-id="43494-121">You can install Python 2.7, 3.4 or 3.5.</span></span> <span data-ttu-id="43494-122">Den här kursen är baserad på Python 2.7.</span><span class="sxs-lookup"><span data-stu-id="43494-122">This tutorial is based on Python 2.7.</span></span> <span data-ttu-id="43494-123">Om du redan har installerat Python gå toohello nästa avsnitt och installera hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="43494-123">If you've already installed Python, go toohello next section and install hello Azure CLI.</span></span>

<span data-ttu-id="43494-124">Du måste också tooadd hello sökvägen hello mappar där python.exe och pip.exe är installerade toohello system `PATH` miljövariabeln.</span><span class="sxs-lookup"><span data-stu-id="43494-124">You also need tooadd hello path of hello folders where python.exe and pip.exe are installed toohello system `PATH` environment variable.</span></span> <span data-ttu-id="43494-125">Som standard installeras python.exe i `C:\Python27` och pip.exe installeras i `C:\Python27\Scripts`.</span><span class="sxs-lookup"><span data-stu-id="43494-125">By default, python.exe is installed in `C:\Python27` and pip.exe is installed in `C:\Python27\Scripts`.</span></span>

## <a name="install-hello-azure-cli"></a><span data-ttu-id="43494-126">Installera hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="43494-126">Install hello Azure CLI</span></span>
<span data-ttu-id="43494-127">hello Azure CLI tillhandahåller en flera plattformar kommandoraden för Azure.</span><span class="sxs-lookup"><span data-stu-id="43494-127">hello Azure CLI provides a multiplatform command-line experience for Azure.</span></span> <span data-ttu-id="43494-128">Du arbetar direkt från din kommandoradsverktyget tooprovision och hantera resurser.</span><span class="sxs-lookup"><span data-stu-id="43494-128">You work directly from your command-line tooprovision and manage resources.</span></span>

<span data-ttu-id="43494-129">tooinstall hello Azure CLI, Följ dessa steg:</span><span class="sxs-lookup"><span data-stu-id="43494-129">tooinstall hello Azure CLI, follow these steps:</span></span>

1. <span data-ttu-id="43494-130">Öppna ett kommandotolksfönster som administratör.</span><span class="sxs-lookup"><span data-stu-id="43494-130">Open a Command Prompt window as an administrator.</span></span>
2. <span data-ttu-id="43494-131">Kör följande kommandon hello:</span><span class="sxs-lookup"><span data-stu-id="43494-131">Run hello following commands:</span></span>

   ```bash
   pip install --upgrade azure-cli
   pip install --upgrade azure-cli-iot
   ```
3. <span data-ttu-id="43494-132">Kontrollera hello installationen genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="43494-132">Verify hello installation by running hello following command:</span></span>

   ```bash
   az iot -h
   ```

<span data-ttu-id="43494-133">Du kan se hello följande utdata om hello-installationen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="43494-133">You see hello following output if hello installation is successful.</span></span>

![Utdata som indikerar att det lyckades](media/iot-hub-raspberry-pi-lessons/lesson2/az_iot_help_win.png)

## <a name="summary"></a><span data-ttu-id="43494-135">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="43494-135">Summary</span></span>
<span data-ttu-id="43494-136">Du har installerat hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="43494-136">You've installed hello Azure CLI.</span></span> <span data-ttu-id="43494-137">Nästa uppgift toocreate dina Azure IoT hub- och enhetsidentitet med hjälp av hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="43494-137">Your next task toocreate your Azure IoT hub and device identity by using hello Azure CLI.</span></span>

## <a name="next-steps"></a><span data-ttu-id="43494-138">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="43494-138">Next steps</span></span>
[<span data-ttu-id="43494-139">Skapa din IoT-hubb och registrera hallon Pi 3</span><span class="sxs-lookup"><span data-stu-id="43494-139">Create your IoT hub and register Raspberry Pi 3</span></span>](iot-hub-raspberry-pi-kit-node-lesson2-prepare-azure-iot-hub.md)

