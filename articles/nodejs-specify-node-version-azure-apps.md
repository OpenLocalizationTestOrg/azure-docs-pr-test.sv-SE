---
title: aaaSpecifying en Node.js-Version
description: "Lär dig hur toospecify hello version av Node.js som används av Azure-webbplatser och molntjänster"
services: 
documentationcenter: nodejs
author: TomArcher
manager: routlaw
editor: 
ms.assetid: d0e15278-2ab4-4ec8-8256-913839c6d5ef
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/17/2016
ms.author: tarcher
ms.openlocfilehash: 09c27bfc43c132b6d66f9a2943179e06ee75bedc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="specifying-a-nodejs-version-in-an-azure-application"></a>Ange en Node.js-version i ett Azure-program
När värd för ett Node.js-program, kanske du vill att ditt program använder en viss version av Node.js tooensure. Det finns flera sätt tooaccomplish detta för program som finns på Azure.

## <a name="default-versions"></a>Standardversioner
Hej Node.js-versioner som tillhandahålls av Azure uppdateras kontinuerligt. Om inget annat anges hello standardversion som anges i hello `WEBSITE_NODE_DEFAULT_VERSION` miljövariabeln kommer att användas. toooverride standardvärdet, följ hello stegen i följande avsnitt i den här artikeln

> [!NOTE]
> Om du är värd för ditt program i Azure Cloud Service (webb eller arbetare roll) och det är hello första gången som du har distribuerat programmet hello, Azure försöker toouse hello samma version av Node.js som du har installerat på din utvecklingsmiljö om den matchar hello standardversioner på Azure.
>
>

## <a name="versioning-with-packagejson"></a>Versionshantering med package.json
Du kan ange hello version av Node.js toobe används genom att lägga till följande tooyour hello **package.json** fil:

    "engines":{"node":version}

Där *version* är hello specifik version nummer toouse. Du kan ange mer komplexa villkor för versionen, exempelvis:

    "engines":{"node": "0.6.22 || 0.8.x"}

Eftersom 0.6.22 inte är någon av hello-versioner som är tillgängliga i hello värdmiljön användas hello högsta version av hello 0,8 serie som är tillgängliga kommer att istället - 0.8.4.

## <a name="versioning-websites-with-app-settings"></a>Versionshantering webbplatser med App-inställningar
Om du är värd för hello program på en webbplats, kan du ange miljövariabeln hello **WEBSITE_NODE_DEFAULT_VERSION** toohello önskade versionen.

## <a name="versioning-cloud-services-with-powershell"></a>Versionshantering molntjänster med PowerShell
Om du är värd för programmet hello i en molntjänst och distribuerar hello program med hjälp av Azure PowerShell, kan du åsidosätta hello Node.js-standardversion med hjälp av hello **Set AzureServiceProjectRole** PowerShell-cmdlet. Exempel:

    Set-AzureServiceProjectRole WebRole1 Node 0.8.4

Obs hello parametrar i hello ovan instruktionen är skiftlägeskänsliga.  Du kan verifiera hello rätt version av Node.js har valts genom att kontrollera hello **motorer** egenskap i din roll **package.json**.

Du kan också använda hello **Get-AzureServiceProjectRoleRuntime** tooretrieve en lista över Node.js-versioner som är tillgängliga för program som finns som en tjänst i molnet.  Kontrollera alltid hello-versionen av Node.js projektet beroende är i den här listan.

## <a name="using-a-custom-version-with-azure-websites"></a>Använda en anpassad version med Azure Websites
Medan Azure innehåller flera standardversioner av Node.js, kan du kanske vill toouse en version som inte har angetts som standard. Om du är värd för programmet som en Azure-webbplats, du kan göra detta med hjälp av hello **iisnode.yml** fil. hello igenom följande steg hello processen med att använda en anpassad version av Node.Js med en Azure-webbplats:

1. Skapa en ny katalog och skapa sedan en **server.js** filen i hello katalog. Hej **server.js** -filen ska innehålla hello följande:

        var http = require('http');
        http.createServer(function(req,res) {
          res.writeHead(200, {'Content-Type': 'text/html'});
          res.end('Hello from Azure running node version: ' + process.version + '</br>');
        }).listen(process.env.PORT || 3000);

    Hej Node.js-version som används när du bläddrar hello webbplatsen visas.
2. Skapa en ny webbplats och Observera hello namnet på hello-platsen. Till exempel hello följande använder hello [Azure kommandoradsverktyg] toocreate en ny Azure-webbplats med namnet **minwebbplats**, och sedan aktivera en Git-lagringsplats för hello webbplats.

        azure site create mywebsite --git
3. Skapa en ny katalog med namnet **bin** som underordnad till hello katalog som innehåller hello **server.js** fil.
4. Hämta hello specifik version av **node.exe** (version för Windows hello) som du vill toouse med ditt program. Till exempel hello följande använder **curl** toodownload version 0.8.1:

        curl -O http://nodejs.org/dist/v0.8.1/node.exe

    Spara hello **node.exe** filen till hello **bin** mapp som skapats tidigare.
5. Skapa en **iisnode.yml** filen i hello samma katalog som hello **server.js** filen och Lägg sedan till följande innehåll toohello hello **iisnode.yml** fil:

        nodeProcessCommandLine: "D:\home\site\wwwroot\bin\node.exe"

    Den här sökvägen är där hello **node.exe** fil i projektet kommer att finnas när du har publicerat din programmet toohello Azure-webbplats.
6. Publicera programmet. Till exempel sedan jag tidigare skapade en ny webbplats med hello--git-parametern hello följande kommandon ska lägga till hello programmet filer toomy lokal Git-lagringsplats sedan push-installera dem toohello webbplats databasen:

        git add .
        git commit -m "testing node v0.8.1"
        git push azure master

    Öppna hello webbplatsen i en webbläsare när hello programmet har publicerats. Du bör se ett meddelande om ”Hello från Azure körs nod-version: v0.8.1”.

## <a name="next-steps"></a>Nästa steg
Nu när du förstår hur toospecify hello version av Node.js används av ditt program, lär du dig hur för[fungerar med moduler], [skapa och distribuera en Node.js-webbplats](app-service-web/app-service-web-get-started-nodejs.md), och [hur toouse hello Azure Kommandoradsverktyg för Mac- och Linux].

Mer information finns i hello [Node.js Developer Center](https://azure.microsoft.com/develop/nodejs/).

[hur toouse hello Azure Kommandoradsverktyg för Mac- och Linux]:cli-install-nodejs.md
[Azure kommandoradsverktyg]:cli-install-nodejs.md
[fungerar med moduler]: nodejs-use-node-modules-azure-apps.md
[build and deploy a Node.js Web Site]: app-service-web/app-service-web-get-started-nodejs.md
