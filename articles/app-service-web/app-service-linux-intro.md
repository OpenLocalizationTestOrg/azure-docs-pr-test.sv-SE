---
title: "aaaIntroduction tooAzure webbprogrammet på Linux | Microsoft Docs"
description: "Läs mer om Azure-Webbapp på Linux."
keywords: "Azure apptjänst, linux, oss"
services: app-service
documentationcenter: 
author: naziml
manager: erikre
editor: 
ms.assetid: bc85eff6-bbdf-410a-93dc-0f1222796676
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/16/2017
ms.author: naziml;wesmc
ms.openlocfilehash: 43b9865ade251909a77429eb3e18fe0bcaac3bde
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooazure-web-app-on-linux"></a>Introduktion tooAzure webbprogrammet på Linux

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]

## <a name="overview"></a>Översikt
Kunder kan använda Web App i Linux toohost webbprogram internt på Linux för stackar för program som stöds. hello innehåller följande avsnitt hello programmet stackar som stöds för närvarande. 

## <a name="features"></a>Funktioner
Web App på Linux stöder för närvarande följande program stackar hello:

* Node.js
    * 4.4
    * 4.5
    * 6.2
    * 6.6
    * 6.9
    * 6.10
    * 6.11
    * 8.0
    * 8.1
* PHP
    * 5.6
    * 7.0
* .NET core
    * 1.0
    * 1.1
* Ruby
    * 2.3

Kunder kan distribuera sina program med hjälp av:

* FTP
* Lokal Git
* GitHub
* Bitbucket

För att skala program:

* Kunder kan skala webbprogram uppåt och nedåt genom att ändra hello nivå av deras programtjänstplan
* Kunder kan skala ut program och kör appen instanser inom gränserna för hello av deras SKU

För Kudu del av hello grundläggande funktioner:

* Miljöer
* Distributioner
* Basic-konsolen
* SSH

För devops:

* Mellanlagringsmiljöer
* ACR och DockerHub CI/CD

## <a name="limitations"></a>Begränsningar
hello Azure-portalen visar endast funktioner som för närvarande stöds för webbprogrammet på Linux och döljer hello rest. Som vi aktivera flera funktioner som visas de på hello-portalen.

Vissa funktioner, till exempel virtuell nätverksintegration, Azure Active Directory/autentiseringsmetoder från tredje part eller Kudu webbplatstillägg är inte tillgängliga ännu. När dessa funktioner är tillgängliga, kommer vi uppdatera våra dokumentationen och bloggen om hello ändringar.

Den här förhandsversion finns för närvarande bara i hello följande områden:

* Västra USA
* Östra USA
* Västra Europa
* Norra Europa
* Södra centrala USA
* Norra centrala USA
* Sydostasien
* Östasien
* Östra Australien
* Östra Japan
* Södra Brasilien
* Södra Indien

Web Apps i Linux stöds endast i hello dedikerade apptjänstplaner och har inte en kostnadsfri eller delad nivå. Dessutom är App Service-planer för vanliga och Linux-webbprogram ömsesidigt uteslutande, så du inte kan skapa ett Linux-webbprogram i en icke-Linux app service-plan.

Web Apps i Linux måste skapas i en resursgrupp som inte innehåller icke-Linux web apps i hello samma region.

## <a name="troubleshooting"></a>Felsökning ##

När programmet inte toostart eller du toocheck hello loggning från din app, kontrollera hello Docker loggar i hello loggfilerna directory. Du kan komma åt den här katalogen via webbplatsen SCM eller via FTP.
toolog hello `stdout` och `stderr` från din behållaren måste tooenable **Dockerbehållare loggning** under **diagnostik loggar**.

![Aktivera loggning][2]

![Med Kudu tooview Docker-loggar][1]

Du kan komma åt hello SCM plats från **avancerade verktyg** i hello **utvecklingsverktyg** menyn.

## <a name="next-steps"></a>Nästa steg
Se följande länkar tooget igång med Apptjänst i Linux hello. Du kan publicera frågor och funderingar på [vårt forum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview).

* [Hur toouse anpassade Docker bild för Azure Web App på Linux](app-service-linux-using-custom-docker-image.md)
* [Med PM2 konfiguration för Node.js i Azure Webbapp på Linux](app-service-linux-using-nodejs-pm2.md)
* [Med .NET Core i Azure App Service Webbapp på Linux](app-service-linux-using-dotnetcore.md)
* [Med hjälp av Ruby i Azure App Service Webbapp på Linux](app-service-linux-ruby-get-started.md)
* [Azure App Service Webbapp på Linux vanliga frågor och svar](app-service-linux-faq.md)
* [SSH-stöd för Azure Web App på Linux](./app-service-linux-ssh-support.md)
* [Skapa mellanlagringsmiljöer i Azure App Service](./web-sites-staged-publishing.md)
* [Docker-hubb kontinuerlig distribution med Azure-Webbapp på Linux](./app-service-linux-ci-cd.md)

<!--Image references-->
[1]: ./media/app-service-linux-intro/kudu-docker-logs.png
[2]: ./media/app-service-linux-intro/logging.png