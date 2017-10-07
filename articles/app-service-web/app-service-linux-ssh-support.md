---
title: "aaaSSH stöd för Azure App Service Web App på Linux | Microsoft Docs"
description: "Lär dig mer om hur du använder SSH med Azure Web App på Linux."
keywords: "Azure apptjänst, webbprogram, linux, oss"
services: app-service
documentationcenter: 
author: wesmc7777
manager: erikre
editor: 
ms.assetid: 66f9988f-8ffa-414a-9137-3a9b15a5573c
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/25/2017
ms.author: wesmc
ms.openlocfilehash: e00be6d4631e8936a2a8bc106da1fc06237a4b39
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="ssh-support-for-azure-web-app-on-linux"></a>SSH-stöd för Azure Web App på Linux

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]

## <a name="overview"></a>Översikt

[Secure Shell (SSH)](https://en.wikipedia.org/wiki/Secure_Shell) är kryptografiska nätverksprotokoll för att använda nätverkstjänster på ett säkert sätt. Det är de vanligaste toolog till ett system via fjärranslutning på ett säkert sätt från en kommandorad och utföra administrativa kommandon från en fjärrdator.

Web App på Linux stöder SSH till hello appbehållare med respektive hello inbyggda Docker bilder som används för hello Runtime-Stack för nya webbappar. 

![Runtime stackar](./media/app-service-linux-ssh-support/app-service-linux-runtime-stack.png)

Du kan också använda SSH med egna, anpassade avbildningar Docker inklusive hello SSH-server som en del av hello avbildning och konfigurerar det enligt beskrivningen i det här avsnittet.



## <a name="making-a-client-connection"></a>Gör en klientanslutning

toomake en SSH-klientanslutning, hello huvudwebbplatsen måste vara igång. 

Klistra in hello källa Control (SCM) slutpunkt för ditt webbprogram i webbläsaren med hello följande format:

        https://<your sitename>.scm.azurewebsites.net/webssh/host

Om du inte redan har autentiserats, är obligatoriska tooauthenticate med din Azure-prenumeration tooconnect.

![SSH-anslutning](./media/app-service-linux-ssh-support/app-service-linux-ssh-connection.png)


## <a name="ssh-support-with-custom-docker-images"></a>SSH-support med anpassade Docker-avbildningar

Utföra hello följande steg för Docker-avbildning för en anpassad Docker bild toosupport SSH kommunikation mellan hello behållare och hello klienten i hello Azure-portalen. 

De här stegen är visas i hello Azure App Service-databasen som exempel [här](https://github.com/Azure-App-Service/node/blob/master/6.9.3/).

1. Inkludera hello `openssh-server` installationen i [ `RUN` instruktion](https://docs.docker.com/engine/reference/builder/#run) i hello Dockerfile för avbildningen och ställa in hello lösenordet för rotkontot hello också`"Docker!"`. 

    > [!NOTE] 
    > Den här konfigurationen tillåter inte externa anslutningar toohello behållare. SSH kan endast nås via hello Kudu / SCM plats som autentiseras med hjälp av hello publishing autentiseringsuppgifter.

    ```docker
    # ------------------------
    # SSH Server support
    # ------------------------
    RUN apt-get update \ 
      && apt-get install -y --no-install-recommends openssh-server \
      && echo "root:Docker!" | chpasswd
    ``` 

2. Lägg till en [ `COPY` instruktion](https://docs.docker.com/engine/reference/builder/#copy) toohello Dockerfile toocopy en [sshd_config](http://man.openbsd.org/sshd_config) filen toohello */etc/ssh/* directory. Konfigurationsfilen ska baseras på vår sshd_config-filen i hello Azure App Service GitHub-lagringsplatsen [här](https://github.com/Azure-App-Service/node/blob/master/6.11/sshd_config).

    > [!NOTE] 
    > Hej *sshd_config* filen måste innehålla följande hello eller hello anslutningen misslyckas: 
    > * `Ciphers`måste innehålla minst en av följande hello: `aes128-cbc,3des-cbc,aes256-cbc`.
    > * `MACs`måste innehålla minst en av följande hello: `hmac-sha1,hmac-sha1-96`.

    ```docker
    COPY sshd_config /etc/ssh/
    ```


3. Inkludera porten 2222 i hello [ `EXPOSE` instruktion](https://docs.docker.com/engine/reference/builder/#expose) för hello Dockerfile. Även om hello rotlösenordet är känd port 2222 kan inte nås från hello internet. Det är ett internt endast port tillgänglig endast av behållare i hello brygga för nätverk i ett privat virtuellt nätverk.

    ```docker
    EXPOSE 2222 80
    ```

4. Kontrollera att toostart hello ssh-tjänsten. hello exempel [här](https://github.com/Azure-App-Service/node/blob/master/6.9.3/startup/init_container.sh) använder ett kommandoskript i */bin* directory.

    ```bash
    #!/bin/bash
    service ssh start
    ```

    Hej Dockerfile använder hello [ `CMD` instruktion](https://docs.docker.com/engine/reference/builder/#cmd) toorun hello skript.

    ```docker
    COPY init_container.sh /bin/
      ...
    RUN chmod 755 /bin/init_container.sh 
      ...       
    CMD ["/bin/init_container.sh"]
    ```



## <a name="next-steps"></a>Nästa steg
Se följande länkar för mer information om webbprogrammet på Linux hello. Du kan publicera frågor och funderingar på [vårt forum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview).

* [Hur toouse anpassade Docker bild för Azure Web App på Linux](app-service-linux-using-custom-docker-image.md)
* [Med PM2 konfiguration för Node.js i Azure Webbapp på Linux](app-service-linux-using-nodejs-pm2.md)
* [Med .NET Core i Azure-Webbapp på Linux](app-service-linux-using-dotnetcore.md)
* [Med hjälp av Ruby i Azure Webbapp på Linux](app-service-linux-ruby-get-started.md)
* [Azure App Service Webbapp på Linux vanliga frågor och svar](app-service-linux-faq.md)

