---
title: "aaaDownload hello Azure SDK för PHP"
description: "Lär dig hur toodownload och installera hello Azure SDK för PHP."
documentationcenter: php
services: app-service\web
author: allclark
manager: douge
editor: 
ms.assetid: bac355ac-4c25-42f4-8273-c5112eafa8d4
ms.service: app-service-web
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 06/01/2016
ms.author: allclark;yaqiyang
ms.openlocfilehash: 94f56fc4f91bb175c08b9f7a43cb221c827694a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="download-hello-azure-sdk-for-php"></a><span data-ttu-id="4efc5-103">Hämta hello Azure SDK för PHP</span><span class="sxs-lookup"><span data-stu-id="4efc5-103">Download hello Azure SDK for PHP</span></span>
## <a name="overview"></a><span data-ttu-id="4efc5-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="4efc5-104">Overview</span></span>
<span data-ttu-id="4efc5-105">hello Azure SDK för PHP innehåller komponenter som gör att du toodevelop, distribuera och hantera PHP-program för Azure.</span><span class="sxs-lookup"><span data-stu-id="4efc5-105">hello Azure SDK for PHP includes components that allow you toodevelop, deploy, and manage PHP applications for Azure.</span></span> <span data-ttu-id="4efc5-106">Hello Azure SDK för PHP innehåller främst hello följande:</span><span class="sxs-lookup"><span data-stu-id="4efc5-106">Specifically, hello Azure SDK for PHP includes hello following:</span></span>

* <span data-ttu-id="4efc5-107">**Hej PHP-klientbibliotek för Azure**.</span><span class="sxs-lookup"><span data-stu-id="4efc5-107">**hello PHP client libraries for Azure**.</span></span> <span data-ttu-id="4efc5-108">Dessa klassbibliotek tillhandahåller ett gränssnitt för att komma åt Azure funktioner, till exempel datatjänster och molntjänster.</span><span class="sxs-lookup"><span data-stu-id="4efc5-108">These class libraries provide an interface for accessing Azure features, such as data management services and cloud services.</span></span>  
* <span data-ttu-id="4efc5-109">**hello Azure-kommandoradsgränssnittet för Mac, Linux och Windows (Azure CLI)**.</span><span class="sxs-lookup"><span data-stu-id="4efc5-109">**hello Azure Command-Line Interface for Mac, Linux, and Windows (Azure CLI)**.</span></span> <span data-ttu-id="4efc5-110">Det här är en uppsättning kommandon för att distribuera och hantera Azure-tjänster, till exempel Azure Websites och Azure Virtual Machines.</span><span class="sxs-lookup"><span data-stu-id="4efc5-110">This is a set of commands for deploying and managing Azure services, such as Azure Websites and Azure Virtual Machines.</span></span> <span data-ttu-id="4efc5-111">hello Azure CLI fungerar på alla plattformar, inklusive Mac, Linux och Windows.</span><span class="sxs-lookup"><span data-stu-id="4efc5-111">hello Azure CLI work on any platform, including Mac, Linux, and Windows.</span></span>
* <span data-ttu-id="4efc5-112">**Azure PowerShell (Windows)**.</span><span class="sxs-lookup"><span data-stu-id="4efc5-112">**Azure PowerShell (Windows Only)**.</span></span> <span data-ttu-id="4efc5-113">Det här är en uppsättning PowerShell-cmdlets för att distribuera och hantera Azure-tjänster, till exempel molntjänster och virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="4efc5-113">This is a set of PowerShell cmdlets for deploying and managing Azure Services, such as Cloud Services and Virtual Machines.</span></span>
* <span data-ttu-id="4efc5-114">**hello (endast Windows) Azure Emulatorerna**.</span><span class="sxs-lookup"><span data-stu-id="4efc5-114">**hello Azure Emulators (Windows Only)**.</span></span> <span data-ttu-id="4efc5-115">hello beräkning och lagring emulatorerna är lokala emulatorerna för molntjänster och tjänster för data som gör att du tootest ett program lokalt.</span><span class="sxs-lookup"><span data-stu-id="4efc5-115">hello compute and storage emulators are local emulators of cloud services and data management services that allow you tootest an application locally.</span></span> <span data-ttu-id="4efc5-116">hello Azure Emulatorerna körs bara i Windows.</span><span class="sxs-lookup"><span data-stu-id="4efc5-116">hello Azure Emulators run on Windows only.</span></span>

<span data-ttu-id="4efc5-117">hello avsnitten nedan beskrivs hur toodownload och installera hello komponenter som beskrivs ovan.</span><span class="sxs-lookup"><span data-stu-id="4efc5-117">hello sections below describe how toodownload and install hello components described above.</span></span>

<span data-ttu-id="4efc5-118">hello instruktionerna i det här avsnittet förutsätter att du har [PHP] [ install-php] installerad.</span><span class="sxs-lookup"><span data-stu-id="4efc5-118">hello instructions in this topic assume that you have [PHP][install-php] installed.</span></span>

> [!NOTE]
> <span data-ttu-id="4efc5-119">Du måste ha PHP 5.5 eller högre toouse hello PHP-klientbibliotek för Azure.</span><span class="sxs-lookup"><span data-stu-id="4efc5-119">You must have PHP 5.5 or higher toouse hello PHP client libraries for Azure.</span></span>
> 
> 

## <a name="php-client-libraries-for-azure"></a><span data-ttu-id="4efc5-120">PHP-klientbibliotek för Azure</span><span class="sxs-lookup"><span data-stu-id="4efc5-120">PHP client libraries for Azure</span></span>
<span data-ttu-id="4efc5-121">hello PHP-klientbibliotek för Azure tillhandahåller ett gränssnitt för att komma åt Azure funktioner, till exempel datatjänster och molntjänster, från alla operativsystem.</span><span class="sxs-lookup"><span data-stu-id="4efc5-121">hello PHP Client Libraries for Azure provide an interface for accessing Azure features, such as data management services and cloud services, from any operating system.</span></span> <span data-ttu-id="4efc5-122">Dessa bibliotek kan installeras via hello Composer.</span><span class="sxs-lookup"><span data-stu-id="4efc5-122">These libraries can be installed via hello Composer.</span></span>

<span data-ttu-id="4efc5-123">Information om hur toouse hello PHP-klientbibliotek för Azure finns [hur tooUse hello Blob-tjänsten][blob-service], [hur tooUse hello Tabelltjänsten] [ table-service] och [hur tooUse hello kötjänsten][queue-service].</span><span class="sxs-lookup"><span data-stu-id="4efc5-123">For information about how toouse hello PHP Client Libraries for Azure, see [How tooUse hello Blob Service][blob-service], [How tooUse hello Table Service][table-service] and [How tooUse hello Queue Service][queue-service].</span></span>

### <a name="install-via-composer"></a><span data-ttu-id="4efc5-124">Installera via Composer</span><span class="sxs-lookup"><span data-stu-id="4efc5-124">Install via Composer</span></span>
1. <span data-ttu-id="4efc5-125">[Installera Git][install-git].</span><span class="sxs-lookup"><span data-stu-id="4efc5-125">[Install Git][install-git].</span></span>

    > [AZURE.NOTE] <span data-ttu-id="4efc5-126">I Windows måste du också tooadd hello Git körbara tooyour PATH-miljövariabeln.</span><span class="sxs-lookup"><span data-stu-id="4efc5-126">On Windows, you will also need tooadd hello Git executable tooyour PATH environment variable.</span></span>

1. <span data-ttu-id="4efc5-127">Skapa en fil med namnet **composer.json** i hello roten av projektet och Lägg till följande kod tooit hello:</span><span class="sxs-lookup"><span data-stu-id="4efc5-127">Create a file named **composer.json** in hello root of your project and add hello following code tooit:</span></span>
   
        {
            "require": {
                "microsoft/windowsazure": "^0.4"
            }
        }
2. <span data-ttu-id="4efc5-128">Hämta  **[composer.phar] [ composer-phar]**  i projektroten.</span><span class="sxs-lookup"><span data-stu-id="4efc5-128">Download **[composer.phar][composer-phar]** in your project root.</span></span>
3. <span data-ttu-id="4efc5-129">Öppna en kommandotolk och kör det i projektroten</span><span class="sxs-lookup"><span data-stu-id="4efc5-129">Open a command prompt and execute this in your project root</span></span>
   
        php composer.phar install

## <a name="azure-powershell-and-azure-emulators"></a><span data-ttu-id="4efc5-130">Azure PowerShell och Azure Emulatorerna</span><span class="sxs-lookup"><span data-stu-id="4efc5-130">Azure PowerShell and Azure Emulators</span></span>
<span data-ttu-id="4efc5-131">Azure PowerShell är en uppsättning PowerShell-cmdlets för att distribuera och hantera Azure-tjänster (till exempel molntjänster och virtuella datorer).</span><span class="sxs-lookup"><span data-stu-id="4efc5-131">Azure PowerShell is a set of PowerShell cmdlets for deploying and managing Azure Services (such as Cloud Services and Virtual Machines).</span></span> <span data-ttu-id="4efc5-132">hello Azure Emulatorerna är emulatorerna för molntjänster och tjänster för data som gör att du tootest ett program lokalt.</span><span class="sxs-lookup"><span data-stu-id="4efc5-132">hello Azure Emulators are emulators of cloud services and data management services that allow you tootest an application locally.</span></span> <span data-ttu-id="4efc5-133">Dessa komponenter stöds endast Windows.</span><span class="sxs-lookup"><span data-stu-id="4efc5-133">These components are supported Windows only.</span></span>

<span data-ttu-id="4efc5-134">Hej rekommenderade sättet tooinstall Azure PowerShell och hello Azure Emulatorerna är toouse hello [Microsoft Web Platform Installer][download-wpi].</span><span class="sxs-lookup"><span data-stu-id="4efc5-134">hello recommended way tooinstall Azure PowerShell and hello Azure Emulators is toouse hello [Microsoft Web Platform Installer][download-wpi].</span></span> <span data-ttu-id="4efc5-135">Observera att du kan också välja tooinstall andra utvecklingskomponenter, till exempel PHP, SQL Server, hello Drivers Microsoft för SQL Server för PHP och WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="4efc5-135">Note that you can also choose tooinstall other development components, such as PHP, SQL Server, hello Microsoft Drivers for SQL Server for PHP, and WebMatrix.</span></span>

<span data-ttu-id="4efc5-136">Information om hur toouse Azure PowerShell finns [hur tooUse Azure PowerShell][powershell-tools].</span><span class="sxs-lookup"><span data-stu-id="4efc5-136">For information about how toouse Azure PowerShell, see [How tooUse Azure PowerShell][powershell-tools].</span></span>

## <a name="azure-cli"></a><span data-ttu-id="4efc5-137">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="4efc5-137">Azure CLI</span></span>
<span data-ttu-id="4efc5-138">hello Azure CLI är en uppsättning kommandon för att distribuera och hantera Azure-tjänster, till exempel Azure Websites och Azure Virtual Machines.</span><span class="sxs-lookup"><span data-stu-id="4efc5-138">hello Azure CLI is a set of commands for deploying and managing Azure services, such as Azure Websites and Azure Virtual Machines.</span></span> <span data-ttu-id="4efc5-139">Information om hur du installerar Azure CLI finns [installera hello Azure CLI](cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="4efc5-139">For information about installing Azure CLI, see [Install hello Azure CLI](cli-install-nodejs.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="4efc5-140">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4efc5-140">Next steps</span></span>
<span data-ttu-id="4efc5-141">Mer information finns i hello [PHP Developer Center](/develop/php/).</span><span class="sxs-lookup"><span data-stu-id="4efc5-141">For more information, see hello [PHP Developer Center](/develop/php/).</span></span>

[install-php]: http://www.php.net/manual/en/install.php
[composer-github]: https://github.com/composer/composer
[composer-phar]: http://getcomposer.org/composer.phar
[nodejs-org]: http://nodejs.org/
[install-node-linux]: https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager
[download-wpi]: http://go.microsoft.com/fwlink/?LinkId=253447
[mac-installer]: http://go.microsoft.com/fwlink/?LinkId=252249
[blob-service]: http://go.microsoft.com/fwlink/?LinkId=252714
[table-service]: http://go.microsoft.com/fwlink/?LinkId=252715
[queue-service]: http://go.microsoft.com/fwlink/?LinkId=252716
[azure cli]: http://go.microsoft.com/fwlink/?LinkId=252717
[powershell-tools]: http://go.microsoft.com/fwlink/?LinkId=252718
[php-sdk-github]: http://go.microsoft.com/fwlink/?LinkId=252719
[install-git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
