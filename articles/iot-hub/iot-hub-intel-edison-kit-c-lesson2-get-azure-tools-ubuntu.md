---
title: 'Connect Intel EDISON (C) tooAzure IoT - lektionen 2: Azure-verktyg (Ubuntu) | Microsoft Docs'
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
ms.openlocfilehash: a7691c13d43aa6dfff24adf2b470728d5266713e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-azure-tools-ubuntu-1604"></a><span data-ttu-id="9405f-104">Hämta Azure-verktyg (Ubuntu 16.04)</span><span class="sxs-lookup"><span data-stu-id="9405f-104">Get Azure tools (Ubuntu 16.04)</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="9405f-105">[Windows 7 och senare][windows]</span><span class="sxs-lookup"><span data-stu-id="9405f-105">[Windows 7 and later][windows]</span></span>
> * <span data-ttu-id="9405f-106">[Ubuntu 16.04][ubuntu]</span><span class="sxs-lookup"><span data-stu-id="9405f-106">[Ubuntu 16.04][ubuntu]</span></span>
> * <span data-ttu-id="9405f-107">[macOS 10.10][macos]</span><span class="sxs-lookup"><span data-stu-id="9405f-107">[macOS 10.10][macos]</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="9405f-108">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="9405f-108">What you will do</span></span>
<span data-ttu-id="9405f-109">Installera hello Azure-kommandoradsgränssnittet (Azure CLI).</span><span class="sxs-lookup"><span data-stu-id="9405f-109">Install hello Azure command-line interface (Azure CLI).</span></span> <span data-ttu-id="9405f-110">Om du har några problem med söka efter lösningar på hello [felsökning sidan][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="9405f-110">If you have any problems, look for solutions on hello [troubleshooting page][troubleshooting].</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="9405f-111">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="9405f-111">What you will learn</span></span>
<span data-ttu-id="9405f-112">I den här artikeln får du lära dig:</span><span class="sxs-lookup"><span data-stu-id="9405f-112">In this article, you will learn:</span></span>
* <span data-ttu-id="9405f-113">Hur tooinstall hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="9405f-113">How tooinstall hello Azure CLI.</span></span>
* <span data-ttu-id="9405f-114">Hur tooadd en IoT-undergrupp till hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="9405f-114">How tooadd an IoT subgroup of hello Azure CLI.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="9405f-115">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="9405f-115">What you need</span></span>
* <span data-ttu-id="9405f-116">En Ubuntu-dator med en Internet-anslutning.</span><span class="sxs-lookup"><span data-stu-id="9405f-116">An Ubuntu computer with an Internet connection.</span></span>
* <span data-ttu-id="9405f-117">En aktiv Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="9405f-117">An active Azure subscription.</span></span> <span data-ttu-id="9405f-118">Om du inte har ett konto kan du skapa en [ledigt utvärderingskonto](http://azure.microsoft.com/pricing/free-trial/) i bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="9405f-118">If you don't have an account, you can create a [free trial account](http://azure.microsoft.com/pricing/free-trial/) in just a few minutes.</span></span>

## <a name="install-hello-azure-cli"></a><span data-ttu-id="9405f-119">Installera hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="9405f-119">Install hello Azure CLI</span></span>
<span data-ttu-id="9405f-120">hello Azure CLI tillhandahåller en flera plattformar kommandoraden för Azure, vilket gör att du toowork direkt från kommandoraden-tooprovision och hantera resurser.</span><span class="sxs-lookup"><span data-stu-id="9405f-120">hello Azure CLI provides a multiplatform command-line experience for Azure, enabling you toowork directly from your command line tooprovision and manage resources.</span></span>

<span data-ttu-id="9405f-121">tooinstall Hej senaste Azure CLI, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="9405f-121">tooinstall hello latest Azure CLI, follow these steps:</span></span>

1. <span data-ttu-id="9405f-122">Kör följande kommandon i ett terminalfönster hello.</span><span class="sxs-lookup"><span data-stu-id="9405f-122">Run hello following commands in a terminal window.</span></span> <span data-ttu-id="9405f-123">Det kan ta fem minuter tooinstall hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="9405f-123">It might take five minutes tooinstall hello Azure CLI.</span></span>

   ```bash
   sudo apt-get update
   sudo apt-get install -y libssl-dev libffi-dev
   sudo apt-get install -y python-dev
   sudo apt-get install -y build-essential
   sudo apt-get install -y python-pip
   sudo pip install --upgrade azure-cli
   sudo pip install --upgrade azure-cli-iot
   ```
2. <span data-ttu-id="9405f-124">Kontrollera hello installationen genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="9405f-124">Verify hello installation by running hello following command:</span></span>

   ```bash
   az iot -h
   ```

<span data-ttu-id="9405f-125">Du bör se hello följande utdata om hello-installationen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="9405f-125">You should see hello following output if hello installation is successful.</span></span>

![Utdata som indikerar att det lyckades](media/iot-hub-intel-edison-lessons/lesson2/az_iot_help_ubuntu.png)

## <a name="summary"></a><span data-ttu-id="9405f-127">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="9405f-127">Summary</span></span>
<span data-ttu-id="9405f-128">Du har installerat hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="9405f-128">You've installed hello Azure CLI.</span></span> <span data-ttu-id="9405f-129">Nästa uppgift är toocreate dina Azure IoT-hubb och enheter identitet med hjälp av hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="9405f-129">Your next task is toocreate your Azure IoT hub and device identity using hello Azure CLI.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9405f-130">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9405f-130">Next steps</span></span>
<span data-ttu-id="9405f-131">[Skapa din IoT-hubb och registrera Intel modern][create-your-iot-hub-and-register-intel-edison]</span><span class="sxs-lookup"><span data-stu-id="9405f-131">[Create your IoT hub and register Intel Edison][create-your-iot-hub-and-register-intel-edison]</span></span>


<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-c-troubleshooting.md
[create-your-iot-hub-and-register-intel-edison]: iot-hub-intel-edison-kit-c-lesson2-prepare-azure-iot-hub.md
[windows]: iot-hub-intel-edison-kit-c-lesson2-get-azure-tools-win32.md
[ubuntu]: iot-hub-intel-edison-kit-c-lesson2-get-azure-tools-ubuntu.md
[macos]: iot-hub-intel-edison-kit-c-lesson2-get-azure-tools-mac.md
