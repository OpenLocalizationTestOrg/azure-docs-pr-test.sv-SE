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
# <a name="download-hello-azure-sdk-for-php"></a>Hämta hello Azure SDK för PHP
## <a name="overview"></a>Översikt
hello Azure SDK för PHP innehåller komponenter som gör att du toodevelop, distribuera och hantera PHP-program för Azure. Hello Azure SDK för PHP innehåller främst hello följande:

* **Hej PHP-klientbibliotek för Azure**. Dessa klassbibliotek tillhandahåller ett gränssnitt för att komma åt Azure funktioner, till exempel datatjänster och molntjänster.  
* **hello Azure-kommandoradsgränssnittet för Mac, Linux och Windows (Azure CLI)**. Det här är en uppsättning kommandon för att distribuera och hantera Azure-tjänster, till exempel Azure Websites och Azure Virtual Machines. hello Azure CLI fungerar på alla plattformar, inklusive Mac, Linux och Windows.
* **Azure PowerShell (Windows)**. Det här är en uppsättning PowerShell-cmdlets för att distribuera och hantera Azure-tjänster, till exempel molntjänster och virtuella datorer.
* **hello (endast Windows) Azure Emulatorerna**. hello beräkning och lagring emulatorerna är lokala emulatorerna för molntjänster och tjänster för data som gör att du tootest ett program lokalt. hello Azure Emulatorerna körs bara i Windows.

hello avsnitten nedan beskrivs hur toodownload och installera hello komponenter som beskrivs ovan.

hello instruktionerna i det här avsnittet förutsätter att du har [PHP] [ install-php] installerad.

> [!NOTE]
> Du måste ha PHP 5.5 eller högre toouse hello PHP-klientbibliotek för Azure.
> 
> 

## <a name="php-client-libraries-for-azure"></a>PHP-klientbibliotek för Azure
hello PHP-klientbibliotek för Azure tillhandahåller ett gränssnitt för att komma åt Azure funktioner, till exempel datatjänster och molntjänster, från alla operativsystem. Dessa bibliotek kan installeras via hello Composer.

Information om hur toouse hello PHP-klientbibliotek för Azure finns [hur tooUse hello Blob-tjänsten][blob-service], [hur tooUse hello Tabelltjänsten] [ table-service] och [hur tooUse hello kötjänsten][queue-service].

### <a name="install-via-composer"></a>Installera via Composer
1. [Installera Git][install-git].

    > [AZURE.NOTE] I Windows måste du också tooadd hello Git körbara tooyour PATH-miljövariabeln.

1. Skapa en fil med namnet **composer.json** i hello roten av projektet och Lägg till följande kod tooit hello:
   
        {
            "require": {
                "microsoft/windowsazure": "^0.4"
            }
        }
2. Hämta  **[composer.phar] [ composer-phar]**  i projektroten.
3. Öppna en kommandotolk och kör det i projektroten
   
        php composer.phar install

## <a name="azure-powershell-and-azure-emulators"></a>Azure PowerShell och Azure Emulatorerna
Azure PowerShell är en uppsättning PowerShell-cmdlets för att distribuera och hantera Azure-tjänster (till exempel molntjänster och virtuella datorer). hello Azure Emulatorerna är emulatorerna för molntjänster och tjänster för data som gör att du tootest ett program lokalt. Dessa komponenter stöds endast Windows.

Hej rekommenderade sättet tooinstall Azure PowerShell och hello Azure Emulatorerna är toouse hello [Microsoft Web Platform Installer][download-wpi]. Observera att du kan också välja tooinstall andra utvecklingskomponenter, till exempel PHP, SQL Server, hello Drivers Microsoft för SQL Server för PHP och WebMatrix.

Information om hur toouse Azure PowerShell finns [hur tooUse Azure PowerShell][powershell-tools].

## <a name="azure-cli"></a>Azure CLI
hello Azure CLI är en uppsättning kommandon för att distribuera och hantera Azure-tjänster, till exempel Azure Websites och Azure Virtual Machines. Information om hur du installerar Azure CLI finns [installera hello Azure CLI](cli-install-nodejs.md).

## <a name="next-steps"></a>Nästa steg
Mer information finns i hello [PHP Developer Center](/develop/php/).

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
