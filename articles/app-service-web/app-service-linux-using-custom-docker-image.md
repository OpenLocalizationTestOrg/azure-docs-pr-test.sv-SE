---
title: "aaaHow toouse en anpassad avbildning Docker för Azure Web App på Linux | Microsoft Docs"
description: "Hur toouse anpassade Docker bild för Azure Web App på Linux."
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
ms.openlocfilehash: 8853095d0e1067cfea4297bbd23b622fe4a0d4db
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-a-custom-docker-image-for-azure-web-app-on-linux"></a>Använda en anpassad Docker-avbildning för Azure Web App på Linux #

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]


Apptjänst innehåller fördefinierade program stackar på Linux med stöd för specifika versioner, till exempel PHP 7.0 och Node.js 4.5. Apptjänst i Linux använder Docker behållare toohost dessa fördefinierade program stackar. Du kan också använda en anpassad Docker bild toodeploy din web app tooan programstack som inte redan har definierats i Azure. Anpassade Docker-avbildningar kan finnas på antingen ett offentligt eller privat Docker-databasen.


## <a name="how-to-set-a-custom-docker-image-for-a-web-app"></a>Så här: Ange en anpassad Docker-avbildning för en webbapp
Du kan ange hello anpassad Docker-avbildning för både nya och befintliga webbplatser appar. När du skapar ett webbprogram på Linux i hello [Azure-portalen](https://portal.azure.com/#create/Microsoft.AppSvcLinux), klickar du på **konfigurera behållaren** tooset en anpassad avbildning Docker:

![Anpassad Docker-avbildning för en ny webbapp i Linux][1]


## <a name="how-to-use-a-custom-docker-image-from-docker-hub"></a>Så här: använda en anpassad Docker-avbildning från Docker-hubb ##
toouse en anpassad Docker-avbildning från Docker-hubben:

1. I hello [Azure-portalen](https://portal.azure.com), leta upp ditt webbprogram på Linux, sedan i **inställningar** klickar du på **Dockerbehållare**.

2.  Välj **Docker-hubb** som hello **avbildningskällan**, klickar sedan på antingen **offentliga** eller **privata** och typen hello **avbildningen och valfria taggnamnet**, som `node:4.5`. Hej **startkommandot** set automatiskt utifrån vad som har definierats i hello Docker image-filen, men du kan ange egna kommandon.  

    ![Konfigurera Docker-hubb offentliga lagringsplats avbildningen][2]

    När avbildningen är från en privat databas, behöver du också tooenter hello Docker hubb autentiseringsuppgifter som (**inloggning användarnamn** och **lösenord**) för hello privata Docker hubb lagringsplats.

    ![Konfigurera Docker-hubb privata databasen avbildningen][3]

3. När du har konfigurerat hello behållare, klickar du på **spara**.

## <a name="how-toouse-a-docker-image-from-a-private-image-registry"></a>Hur toouse en Docker bild av en privat image-registret ##
toouse en anpassad avbildning Docker privata avbildningen registret:

1. I hello [Azure-portalen](https://portal.azure.com), leta upp ditt webbprogram på Linux, sedan i **inställningar** klickar du på **Dockerbehållare**.

2.  Klicka på **privata registret** som hello **avbildningskällan**. Ange hello **avbildningen och valfria taggnamnet**, **Serveradress** för hello privata registret, tillsammans med hello autentiseringsuppgifter (**inloggning användarnamn** och **lösenord **). Klicka på **Spara**.

    ![Konfigurera Docker-avbildningen från privata registret][4]


## <a name="how-to-set-hello-port-used-by-your-docker-image"></a>Så här: Ange hello-port som används av Docker-bild ##

När du använder en anpassad Docker-avbildning för ditt webbprogram, kan du använda hello `WEBSITES_PORT` miljövariabeln i din Dockerfile, läggs toohello genereras behållare. Överväg följande exempel på en docker-fil för Ruby programmet hello:

    FROM ruby:2.2.0
    RUN mkdir /app
    WORKDIR /app
    ADD . /app
    RUN bundle install
    CMD bundle exec puma config.ru -p WEBSITES_PORT -e production

Du kan se miljövariabel som hello WEBSITES_PORT skickas vid körning på sista raden i hello-kommando. Kom ihåg att skiftläge som är viktiga i kommandona.

Tidigare hello plattform med `PORT` app inställningen genom att vi planerar toodeprecate hello används den här appen inställningen och flytta toousing `WEBSITES_PORT` exklusivt.

När du använder en befintlig Docker-avbildning som skapats av någon annan kan du behöva toospecify en annan port än port 80 för hello program. tooconfigure Hej port, Lägg till ett program som inställning med namnet `WEBSITES_PORT` med hello-värde som visas nedan:

![Konfigurera appen portinställning för anpassad Docker-avbildning][6]

## <a name="how-to-set-hello-startup-time-for-your-docker-image"></a>Så här: Ange hello starttiden för Docker-bild ##

Som standard om din behållare inte startar innan 230 sekunder hello plattform startar om din behållare. Om den anpassade Docker-avbildningen startas i mer än 230 sekunder, kan du använda hello `WEBSITES_CONTAINER_START_TIME_LIMIT` appen inställningen hello värdet för den här inställningen är i sekunder, det gör att hello plattform behålla din behållare som körs innan du startar om den. hello standardvärdet är 230 sekunder och hello högsta tillåtna värde är 600 sekunder.

## <a name="how-to-unmount-hello-platform-provided-storage"></a>Så här: demontera hello plattform lagring ##

Som standard hello plattform att montera en beständig lagring resursen toohello `\home\` directory. Om behållaren avbildningen inte behöver ha en beständig resurs, kan du inaktivera montera den lagringen genom att ange hello `WEBSITES_ENABLE_APP_SERVICE_STORAGE` app inställning för`false`. Du har fortfarande åtkomst toothat lagring från hello scm platsen och alla Docker-loggar (om aktiverat) kommer att skrivas toohello loggfiler som skapas av hello-plattformen.

## <a name="how-to-switch-back-toousing-a-built-in-image"></a>Så här: växlar tillbaka toousing en inbyggd avbildning ##

tooswitch från att använda en anpassad avbildning toousing en inbyggd avbildning:

1. I hello [Azure-portalen](https://portal.azure.com), leta upp ditt webbprogram på Linux, sedan i **inställningar** klickar du på **Apptjänst**.

2. Välj din **Runtime Stack** toouse för hello inbyggda bilden, klicka på **spara**. 

![Konfigurera inbyggda Docker-avbildningen][5]


## <a name="troubleshooting"></a>Felsökning ##

Kontrollera hello Docker loggar i hello loggfilerna directory om programmet misslyckas toostart med den anpassade Docker-avbildningen. Du kan komma åt den här katalogen via webbplatsen SCM eller via FTP.
toolog hello `stdout` och `stderr` från din behållaren måste tooenable **Dockerbehållare loggning** under **diagnostik loggar**.

![Aktivera loggning][8]

![Med Kudu tooview Docker-loggar][7]

Du kan komma åt hello SCM plats från **avancerade verktyg** i hello **utvecklingsverktyg** menyn.

## <a name="next-steps"></a>Nästa steg ##

Följ hello följande länkar tooget igång med Webbappen på Linux.   

* [Introduktion tooAzure webbprogrammet på Linux](./app-service-linux-intro.md)
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
