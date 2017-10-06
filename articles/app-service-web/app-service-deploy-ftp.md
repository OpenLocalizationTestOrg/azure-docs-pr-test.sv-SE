---
title: aaaDeploy din app tooAzure App Service med FTP/S | Microsoft Docs
description: "Lär dig hur toodeploy din app tooAzure App Service med FTP- eller FTPS."
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: ae78b410-1bc0-4d72-8fc4-ac69801247ae
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/05/2016
ms.author: cephalin;dariac
ms.openlocfilehash: 318ae79d4fae269f853ea5c3ce28353b0864131e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-your-app-tooazure-app-service-using-ftps"></a>Distribuera din app tooAzure App Service med FTP/S

Den här artikeln beskrivs hur du toouse FTP eller FTPS toodeploy ditt webbprogram, mobilappsserverdel eller API-app för[Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714).

hello FTP/S-slutpunkten för din app är redan aktiv. Ingen konfiguration är nödvändiga tooenable FTP/S-distributionen.

> [!IMPORTANT]
> Vi tar kontinuerligt steg tooimprove säkerhet för Microsoft Azure-plattformen. Som en del av den här pågående arbete planerad uppgradering av webbprogram för Tyskland Central och Tyskland nordöst regioner. Web Apps inaktiverar under denna hello användning av klartext FTP-protokollet för distributioner. Vår rekommendation tooour kunder är tooswitch tooFTPS för distributioner. Vi förväntar sig inte några avbrott tooyour tjänsten under den här uppgraderingen som är planerat till 9/5. Vi uppskattar du stöd för i den här aktiviteten.

<a name="step1"></a>
## <a name="step-1-set-deployment-credentials"></a>Steg 1: Ange autentiseringsuppgifter för distribution

tooaccess hello FTP-server för din app, måste du först autentiseringsuppgifter för distribution. 

tooset eller återställa dina autentiseringsuppgifter för distribution finns [autentiseringsuppgifter för distribution av Azure App Service](app-service-deployment-credentials.md). Den här kursen visar hello använder användarnivå autentiseringsuppgifter.

## <a name="step-2-get-ftp-connection-information"></a>Steg 2: Hämta information om FTP-anslutning

1. I hello [Azure-portalen](https://portal.azure.com), öppna appens [resursbladet](../azure-resource-manager/resource-group-portal.md#manage-resources).
2. Välj **översikt** hello vänstra menyn anteckna hello värden för **FTP/distribution användaren**, **värdnamn för FTP-**, och **FTPS värdnamn**. 

    ![Information om FTP-anslutning](./media/web-sites-deploy/FTP-Connection-Info.PNG)

    > [!NOTE]
    > Hej **FTP/distribution användaren** värde som användaren som visas av hello Azure Portal, inklusive appnamn hälsningspaket i ordning tooprovide rätt kontext för hello FTP-servern.
    > Du kan hitta hello samma information när du väljer **egenskaper** hello vänstra menyn. 
    >
    > Dessutom visas aldrig hello distribution lösenord. Om du glömmer lösenordet distribution går du tillbaka för[steg 1](#step1) och återställa lösenordet distribution.
    >
    >

## <a name="step-3-deploy-files-tooazure"></a>Steg 3: Distribuera filer tooAzure

1. Från FTP-klient ([Visual Studio](https://www.visualstudio.com/vs/community/), [FileZilla](https://filezilla-project.org/download.php?type=client), osv.), Använd hello anslutningsinformation du samlade in tooconnect tooyour app.
3. Kopiera filerna och deras respektive directory strukturen toohello [ **/platsen/wwwroot** directory](https://github.com/projectkudu/kudu/wiki/File-structure-on-azure) i Azure (eller hello **/platsen/wwwroot/App_Data/jobb/** katalogen för WebJobs).
4. Bläddra tooyour appens URL tooverify hello appen körs korrekt. 

> [!NOTE] 
> Till skillnad från [Git-baserade distributioner](app-service-deploy-local-git.md), FTP-distributionen inte stöder hello efter distributionen automatiseringar: 
>
> - beroende återställning (till exempel NuGet och NPM, PIP. Composer automatiseringar)
> - .NET-binärfiler för
> - generering av web.config (det här är en [Node.js-exempel](https://github.com/projectkudu/kudu/wiki/Using-a-custom-web.config-for-Node-apps))
> 
> Du måste återställa, skapa, skapa dessa nödvändiga filer manuellt på den lokala datorn och distribuera dem tillsammans med din app.
>
>

## <a name="next-steps"></a>Nästa steg

Mer avancerade scenarier för distribution, försök [distribuera tooAzure med Git](app-service-deploy-local-git.md). Distribution av Git-baserade tooAzure kan versionskontroll, paketet återställning, MSBuild och mycket mer.

## <a name="more-resources"></a>Fler resurser

* [Skapa en PHP-MySQL-webbapp och distribuera med FTP](web-sites-php-mysql-deploy-use-ftp.md).
* [Autentiseringsuppgifter för distribution av Azure App Service](app-service-deploy-ftp.md)
