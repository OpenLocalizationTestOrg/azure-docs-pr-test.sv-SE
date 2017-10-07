---
title: aaaHow toouse io.js med Azure App Service Web Apps
description: "Lär dig hur toouse en webbapp i Azure App Service med io.js."
services: app-service\web
documentationcenter: nodejs
author: TomArcher
manager: routlaw
editor: 
ms.assetid: d6320725-ffcb-4ad7-ba63-fc72fa2f2808
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/17/2016
ms.author: tarcher
ms.openlocfilehash: 5dfdac37546b36bc91ab43d9e0a39c2126b4fa9d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-iojs-with-azure-app-service-web-apps"></a>Hur toouse io.js med Azure App Service Web Apps
hello populära nod förgrening [io.js] funktioner olika skillnader tooJoyent Node.js-projekt, inklusive en mer öppna styrning modell, en snabbare utgivningscykeln och en snabbare antagandet av nya och experiment JavaScript-funktioner.

Medan [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) Web Apps har många Node.js-versioner som är förinstallerade, kan också för en som användaren tillhandahållit Node.js binär. Den här artikeln beskrivs två metoder som gör att hello använda io.js på App Service Web Apps: hello användning av en utökad distributionsskriptet som automatiskt konfigurerar Azure toouse hello senaste tillgängliga io.js versionen, samt hello manuell överföring av en io.js binär. 

<a id="deploymentscript"></a>

## <a name="using-a-deployment-script"></a>Med hjälp av ett skript för distribution
Vid distribution av en Node.js-app kör App Service Web Apps ett antal små kommandon tooensure som hello miljö har konfigurerats korrekt. Med en distribution med skriptet för kan den här processen vara anpassade tooinclude hello hämtningen och konfigurationen av io.js.

Hej [io.js distributionsskriptet](https://github.com/felixrieseberg/iojs-azure) finns på GitHub. tooenable io.js på ditt webbprogram helt enkelt kopiera **.deployment**, **deploy.cmd** och **IISNode.yml** toohello rot i programmappen och distribuera tooWeb appar.  

hello första filen, **.deployment**, instruerar Web Apps toorun **deploy.cmd** på distribution. Det här skriptet körs alla hello vanliga åtgärder för ett Node.js-program, men också hämtar hello senaste versionen av io.js. Slutligen **IISNode.yml** konfigurerar Web Apps toouse bara hello hämtas io.js binära i stället för en förinstallerad Node.js binär.

> [!NOTE]
> tooupdate hello används io.js binary, distribuera bara om programmet - hello skript hämtar en ny version av io.js varje gång hello programmet distribueras.
> 
> 

<a id="manualinstallation"></a>

## <a name="using-manual-installation"></a>Med manuell Installation
hello manuell installation av en anpassad io.js version omfattar bara två steg. Hämta först hello **win-x64** binära direkt från hello [io.js distribution]. Krävs är två filer – **iojs.exe** och **iojs.lib**. Spara båda filer tooa mapp inuti ditt webbprogram, till exempel i **bin/iojs**.

tooconfigure Web Apps toouse **iojs.exe** i stället för en förinstallerad nod-version, skapa en **IISNode.yml** hello roten i ditt program och Lägg till följande rad hello.

    nodeProcessCommandLine: "D:\home\site\wwwroot\bin\iojs\iojs.exe"

<a id="nextsteps"></a>

## <a name="next-steps"></a>Nästa steg
I den här artikeln du lärt dig hur toouse io.js med App Service Web Apps med hjälp av både tillhandahålls distribution skript samt manuell installation. 

> [!NOTE]
> IO.js är under utveckling tunga och uppdateras oftare än Node.js. Ett antal Node.js-moduler kanske inte fungerar med io.js - Kontrollera finns [io.js på GitHub] för felsökning.
> 
> 

## <a name="whats-changed"></a>Nyheter
* En guide toohello övergången från webbplatser tooApp tjänsten finns: [Azure App Service och dess påverkan på befintliga Azure-tjänster](http://go.microsoft.com/fwlink/?LinkId=529714)

> [!NOTE]
> Om du vill tooget igång med Azure App Service innan du registrerar dig för ett Azure-konto går för[prova App Service](https://azure.microsoft.com/try/app-service/), där kan du direkt skapa en tillfällig startwebbapp i App Service. Inget kreditkort krävs, och du gör inga åtaganden.
> 
> 

[io.js]: https://iojs.org
[io.js distribution]: https://iojs.org/dist/
[io.js på GitHub]: https://github.com/iojs/io.js
[io.js Deployment Script]: https://github.com/felixrieseberg/iojs-azure
