---
title: "Använda io.js med Webbappar i Azure Apptjänst"
description: "Lär dig hur du använder en webbapp i Azure App Service med io.js."
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
ms.openlocfilehash: 4800504e1939a46842d15e8c9d4279a4b9cae787
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-use-iojs-with-azure-app-service-web-apps"></a>Använda io.js med Webbappar i Azure Apptjänst
Populära nod förgrening [io.js] funktioner olika skillnader till Joyent's Node.js-projekt, inklusive en mer öppna styrning modell, en snabbare utgivningscykeln och en snabbare antagandet av nya och experiment JavaScript-funktioner.

Medan [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) Web Apps har många Node.js-versioner som är förinstallerade, kan också för en som användaren tillhandahållit Node.js binär. Den här artikeln beskrivs två metoder som gör att du kan använda io.js på App Service Web Apps: användning av en utökad distributionsskriptet som automatiskt konfigurerar Azure för att använda den senaste tillgängliga io.js versionen, samt en io.js binär manuella överfördes. 

<a id="deploymentscript"></a>

## <a name="using-a-deployment-script"></a>Med hjälp av ett skript för distribution
Vid distribution av en Node.js-app kör App Service Web Apps ett antal små kommandon för att kontrollera att miljön är korrekt konfigurerad. Den här processen innehåller ett skript för distribution, kan anpassas så att ladda ned och konfigurationen av io.js.

Den [io.js distributionsskriptet](https://github.com/felixrieseberg/iojs-azure) finns på GitHub. Om du vill aktivera io.js på ditt webbprogram, helt enkelt kopiera **.deployment**, **deploy.cmd** och **IISNode.yml** till roten i programmappen och distribuera till Web Apps.  

Den första filen **.deployment**, instruerar Web Apps att köra **deploy.cmd** på distribution. Det här skriptet körs alla vanliga åtgärder för ett Node.js-program, men även laddar ned den senaste versionen av io.js. Slutligen **IISNode.yml** konfigurerar webbprogram att använda just den hämtade io.js binära i stället för en förinstallerad Node.js binär.

> [!NOTE]
> Om du vill uppdatera använda io.js binärfilen bara distribuera ditt program - skriptet laddar ned en ny version av io.js varje gång programmet distribueras.
> 
> 

<a id="manualinstallation"></a>

## <a name="using-manual-installation"></a>Med manuell Installation
Manuell installation av en anpassad io.js version omfattar bara två steg. Hämta först den **win-x64** binära direkt från den [io.js distribution]. Krävs är två filer – **iojs.exe** och **iojs.lib**. Spara filer till en mapp i ditt webbprogram, till exempel i **bin/iojs**.

Du konfigurerar webbprogram att använda **iojs.exe** i stället för en förinstallerad nod-version, skapa en **IISNode.yml** filen i roten för ditt program och Lägg till följande rad.

    nodeProcessCommandLine: "D:\home\site\wwwroot\bin\iojs\iojs.exe"

<a id="nextsteps"></a>

## <a name="next-steps"></a>Nästa steg
I den här artikeln som du har lärt dig hur du använder som io.js med App Service Web Apps med hjälp av både distribution skript samt som manuell installation. 

> [!NOTE]
> IO.js är under utveckling tunga och uppdateras oftare än Node.js. Ett antal Node.js-moduler kanske inte fungerar med io.js - Kontrollera finns [io.js på GitHub] för felsökning.
> 
> 

## <a name="whats-changed"></a>Nyheter
* En guide till övergången från Webbplatser till App Service finns i: [Azure App Service och dess påverkan på befintliga Azure-tjänster](http://go.microsoft.com/fwlink/?LinkId=529714)

> [!NOTE]
> Om du vill komma igång med Azure Apptjänst innan du registrerar dig för ett Azure-konto kan du gå till [Prova Apptjänst](https://azure.microsoft.com/try/app-service/). Där kan du direkt skapa en tillfällig startwebbapp i Apptjänst. Inget kreditkort krävs, och du gör inga åtaganden.
> 
> 

[io.js]: https://iojs.org
[io.js distribution]: https://iojs.org/dist/
[io.js på GitHub]: https://github.com/iojs/io.js
[io.js Deployment Script]: https://github.com/felixrieseberg/iojs-azure
