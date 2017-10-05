---
title: "Hur du använder en anpassad Docker-avbildning för Azure Web App på Linux | Microsoft Docs"
description: "Hur du använder en anpassad Docker-avbildning för Azure Web App på Linux."
keywords: "Azure apptjänst, webbprogram, linux, docker, behållare"
services: app-service
documentationcenter: 
author: naziml
manager: erikre
editor: 
ms.assetid: b97bd4e6-dff0-4976-ac20-d5c109a559a8
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/16/2017
ms.author: naziml;wesmc
ms.openlocfilehash: 1458217a31c4781b28877c030a665f5b22819e13
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="using-a-custom-docker-image-for-azure-web-app-on-linux"></a>Använda en anpassad Docker-avbildning för Azure Web App på Linux #

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]


Apptjänst innehåller fördefinierade program stackar på Linux med stöd för specifika versioner, till exempel PHP 7.0 och Node.js 4.5. Apptjänst i Linux använder Docker-behållare som värd för dessa grupper med fördefinierade program. Du kan också använda en anpassad Docker-avbildning för att distribuera ditt webbprogram till en programstack som inte redan har definierats i Azure. Anpassade Docker-avbildningar kan finnas på antingen ett offentligt eller privat Docker-databasen.


## <a name="how-to-set-a-custom-docker-image-for-a-web-app"></a>Så här: Ange en anpassad Docker-avbildning för en webbapp
Du kan ange den anpassade Docker-avbildningen för både nya och befintliga webbplatser appar. När du skapar ett webbprogram på Linux i den [Azure-portalen](https://portal.azure.com/#create/Microsoft.AppSvcLinux), klickar du på **konfigurera behållaren** att ange en anpassad avbildning Docker:

![Anpassad Docker-avbildning för en ny webbapp i Linux][1]


## <a name="how-to-use-a-custom-docker-image-from-docker-hub"></a>Så här: använda en anpassad Docker-avbildning från Docker-hubb ##
Använda en anpassad Docker-avbildning från Docker-hubben:

1. I den [Azure-portalen](https://portal.azure.com), leta upp ditt webbprogram på Linux, sedan i **inställningar** klickar du på **Dockerbehållare**.

2.  Välj **Docker-hubb** som den **Bildkälla**, klicka på antingen **offentliga** eller **privata** och skriver den **avbildningen och valfria taggnamnet**, som `node:4.5`. Den **startkommandot** set automatiskt utifrån vad som har definierats i Docker image-filen, men du kan ange egna kommandon.  

    ![Konfigurera Docker-hubb offentliga lagringsplats avbildningen][2]

    När avbildningen är från en privat databas kan du också behöva ange autentiseringsuppgifterna för Docker-hubb som (**inloggning användarnamn** och **lösenord**) för privata Docker-hubb-databasen.

    ![Konfigurera Docker-hubb privata databasen avbildningen][3]

3. När du har konfigurerat behållaren, klickar du på **spara**.

## <a name="how-to-use-a-docker-image-from-a-private-image-registry"></a>Hur du använder en Docker-avbildning från ett privat avbildningen register ##
Använda en anpassad Docker-avbildning från ett privat avbildningen registret:

1. I den [Azure-portalen](https://portal.azure.com), leta upp ditt webbprogram på Linux, sedan i **inställningar** klickar du på **Dockerbehållare**.

2.  Klicka på **privata registret** som den **avbildningskällan**. Ange den **avbildningen och valfria taggnamnet**, **Serveradress** för privata registret, tillsammans med autentiseringsuppgifterna (**inloggning användarnamn** och **lösenord**). Klicka på **Spara**.

    ![Konfigurera Docker-avbildningen från privata registret][4]


## <a name="how-to-set-the-port-used-by-your-docker-image"></a>Så här: Ange den port som används av Docker-avbildningen ##

När du använder en anpassad Docker-avbildning för ditt webbprogram, kan du använda den `WEBSITES_PORT` miljövariabeln i din Dockerfile som läggs till skapas behållaren. Överväg följande exempel visar en docker-fil för ett Ruby program:

    FROM ruby:2.2.0
    RUN mkdir /app
    WORKDIR /app
    ADD . /app
    RUN bundle install
    CMD bundle exec puma config.ru -p WEBSITES_PORT -e production

Du kan se att miljövariabeln WEBSITES_PORT överförs vid körning på sista raden i kommandot. Kom ihåg att skiftläge som är viktiga i kommandona.

Tidigare plattformen använde `PORT` app inställningen genom att vi planerar att inaktualisera använda den här appen inställningen och gå till med hjälp av `WEBSITES_PORT` exklusivt.

När du använder en befintlig Docker-avbildning som skapats av någon annan, kan du behöva ange en annan port än port 80 för programmet. Konfigurera porten genom att lägga till ett program som inställning med namnet `WEBSITES_PORT` med värdet som visas nedan:

![Konfigurera appen portinställning för anpassad Docker-avbildning][6]

## <a name="how-to-set-the-startup-time-for-your-docker-image"></a>Så här: Ange starttiden för Docker-bild ##

Som standard om din behållare inte startar innan 230 sekunder plattformen startar om din behållare. Om den anpassade Docker-avbildningen startas i mer än 230 sekunder, du kan använda den `WEBSITES_CONTAINER_START_TIME_LIMIT` appen inställningen genom att värdet för den här inställningen är i sekunder, det gör att plattform behålla din behållare som körs innan du startar om den. Standardvärdet är 230 sekunder och det högsta tillåtna värdet är 600 sekunder.

## <a name="how-to-unmount-the-platform-provided-storage"></a>Så här: demontera plattform lagring ##

Som standard kommer plattformen montera en beständig storage-resurs för den `\home\` directory. Om behållaren avbildningen inte behöver ha en beständig resurs, kan du inaktivera montera den lagringen genom att ange den `WEBSITES_ENABLE_APP_SERVICE_STORAGE` appinställningen `false`. Du har fortfarande åtkomst till den lagringsplats från scm-platsen och alla Docker-loggar (om aktiverat) kommer att skrivas till loggfiler som skapas av plattformen.

## <a name="how-to-switch-back-to-using-a-built-in-image"></a>Så här: växla tillbaka till med hjälp av en inbyggd avbildning ##

Växla från att använda en anpassad avbildning till med hjälp av en inbyggd avbildning:

1. I den [Azure-portalen](https://portal.azure.com), leta upp ditt webbprogram på Linux, sedan i **inställningar** klickar du på **Apptjänst**.

2. Välj din **Runtime Stack** om du vill använda för den inbyggda avbildningen Klicka **spara**. 

![Konfigurera inbyggda Docker-avbildningen][5]


## <a name="troubleshooting"></a>Felsökning ##

Kontrollera Docker loggar i katalogen loggfiler när programmet inte startar med den anpassade Docker-avbildningen. Du kan komma åt den här katalogen via webbplatsen SCM eller via FTP.
Loggen i `stdout` och `stderr` från din behållare, måste du aktivera **Dockerbehållare loggning** under **diagnostik loggar**.

![Aktivera loggning][8]

![Använda Kudu för att visa Docker-loggar][7]

Du kan komma åt webbplatsen SCM från **avancerade verktyg** i den **utvecklingsverktyg** menyn.

## <a name="next-steps"></a>Nästa steg ##

Följ länken nedan för att komma igång med Webbappen på Linux.   

* [Introduktion till Azure-Webbapp på Linux](./app-service-linux-intro.md)
* [Med PM2 konfiguration för Node.js i Azure Webbapp på Linux](./app-service-linux-using-nodejs-pm2.md)
* [Azure App Service Webbapp på Linux vanliga frågor och svar](app-service-linux-faq.md)

Bokför frågor och funderingar på [vårt forum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview).


<!--Image references-->
[1]: ./media/app-service-linux-using-custom-docker-image/new-configure-container.png
[2]: ./media/app-service-linux-using-custom-docker-image/existingapp-configure-dockerhub-public.png
[3]: ./media/app-service-linux-using-custom-docker-image/existingapp-configure-dockerhub-private.png
[4]: ./media/app-service-linux-using-custom-docker-image/existingapp-configure-privateregistry.png
[5]: ./media/app-service-linux-using-custom-docker-image/existingapp-configure-builtin.png
[6]: ./media/app-service-linux-using-custom-docker-image/setting-port.png
[7]: ./media/app-service-linux-using-custom-docker-image/kudu-docker-logs.png
[8]: ./media/app-service-linux-using-custom-docker-image/logging.png
