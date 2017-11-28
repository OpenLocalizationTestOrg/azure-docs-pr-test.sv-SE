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
# <a name="ssh-support-for-azure-web-app-on-linux"></a><span data-ttu-id="7615e-104">SSH-stöd för Azure Web App på Linux</span><span class="sxs-lookup"><span data-stu-id="7615e-104">SSH support for Azure Web App on Linux</span></span>

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]

## <a name="overview"></a><span data-ttu-id="7615e-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="7615e-105">Overview</span></span>

<span data-ttu-id="7615e-106">[Secure Shell (SSH)](https://en.wikipedia.org/wiki/Secure_Shell) är kryptografiska nätverksprotokoll för att använda nätverkstjänster på ett säkert sätt.</span><span class="sxs-lookup"><span data-stu-id="7615e-106">[Secure Shell (SSH)](https://en.wikipedia.org/wiki/Secure_Shell) is a cryptographic network protocol for using network services securely.</span></span> <span data-ttu-id="7615e-107">Det är de vanligaste toolog till ett system via fjärranslutning på ett säkert sätt från en kommandorad och utföra administrativa kommandon från en fjärrdator.</span><span class="sxs-lookup"><span data-stu-id="7615e-107">It is most commonly used toolog into a system remotely securely from a command-line and execute administrative commands remotely.</span></span>

<span data-ttu-id="7615e-108">Web App på Linux stöder SSH till hello appbehållare med respektive hello inbyggda Docker bilder som används för hello Runtime-Stack för nya webbappar.</span><span class="sxs-lookup"><span data-stu-id="7615e-108">Web App on Linux provides SSH support into hello app container with each of hello built-in Docker images used for hello Runtime Stack of new web apps.</span></span> 

![Runtime stackar](./media/app-service-linux-ssh-support/app-service-linux-runtime-stack.png)

<span data-ttu-id="7615e-110">Du kan också använda SSH med egna, anpassade avbildningar Docker inklusive hello SSH-server som en del av hello avbildning och konfigurerar det enligt beskrivningen i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="7615e-110">You can also use SSH with your custom Docker images by including hello SSH server as part of hello image and configuring it as described in this topic.</span></span>



## <a name="making-a-client-connection"></a><span data-ttu-id="7615e-111">Gör en klientanslutning</span><span class="sxs-lookup"><span data-stu-id="7615e-111">Making a client connection</span></span>

<span data-ttu-id="7615e-112">toomake en SSH-klientanslutning, hello huvudwebbplatsen måste vara igång.</span><span class="sxs-lookup"><span data-stu-id="7615e-112">toomake an SSH client connection, hello main site must be started.</span></span> 

<span data-ttu-id="7615e-113">Klistra in hello källa Control (SCM) slutpunkt för ditt webbprogram i webbläsaren med hello följande format:</span><span class="sxs-lookup"><span data-stu-id="7615e-113">Paste hello Source Control Management (SCM) endpoint for your web app into your browser using hello following form:</span></span>

        https://<your sitename>.scm.azurewebsites.net/webssh/host

<span data-ttu-id="7615e-114">Om du inte redan har autentiserats, är obligatoriska tooauthenticate med din Azure-prenumeration tooconnect.</span><span class="sxs-lookup"><span data-stu-id="7615e-114">If you are not already authenticated, you are required tooauthenticate with your Azure subscription tooconnect.</span></span>

![SSH-anslutning](./media/app-service-linux-ssh-support/app-service-linux-ssh-connection.png)


## <a name="ssh-support-with-custom-docker-images"></a><span data-ttu-id="7615e-116">SSH-support med anpassade Docker-avbildningar</span><span class="sxs-lookup"><span data-stu-id="7615e-116">SSH support with custom Docker images</span></span>

<span data-ttu-id="7615e-117">Utföra hello följande steg för Docker-avbildning för en anpassad Docker bild toosupport SSH kommunikation mellan hello behållare och hello klienten i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="7615e-117">In order for a custom Docker image toosupport SSH communication between hello container and hello client in hello Azure portal, perform hello following steps for your Docker image.</span></span> 

<span data-ttu-id="7615e-118">De här stegen är visas i hello Azure App Service-databasen som exempel [här](https://github.com/Azure-App-Service/node/blob/master/6.9.3/).</span><span class="sxs-lookup"><span data-stu-id="7615e-118">These steps are are shown in hello Azure App Service repository as an example [here](https://github.com/Azure-App-Service/node/blob/master/6.9.3/).</span></span>

1. <span data-ttu-id="7615e-119">Inkludera hello `openssh-server` installationen i [ `RUN` instruktion](https://docs.docker.com/engine/reference/builder/#run) i hello Dockerfile för avbildningen och ställa in hello lösenordet för rotkontot hello också`"Docker!"`.</span><span class="sxs-lookup"><span data-stu-id="7615e-119">Include hello `openssh-server` installation in [`RUN` instruction](https://docs.docker.com/engine/reference/builder/#run) in hello Dockerfile for your image and set hello password for hello root account too`"Docker!"`.</span></span> 

    > [!NOTE] 
    > <span data-ttu-id="7615e-120">Den här konfigurationen tillåter inte externa anslutningar toohello behållare.</span><span class="sxs-lookup"><span data-stu-id="7615e-120">This configuration does not allow external connections toohello container.</span></span> <span data-ttu-id="7615e-121">SSH kan endast nås via hello Kudu / SCM plats som autentiseras med hjälp av hello publishing autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="7615e-121">SSH can only be accessed via hello Kudu / SCM Site, which is authenticated using hello publishing credentials.</span></span>

    ```docker
    # ------------------------
    # SSH Server support
    # ------------------------
    RUN apt-get update \ 
      && apt-get install -y --no-install-recommends openssh-server \
      && echo "root:Docker!" | chpasswd
    ``` 

2. <span data-ttu-id="7615e-122">Lägg till en [ `COPY` instruktion](https://docs.docker.com/engine/reference/builder/#copy) toohello Dockerfile toocopy en [sshd_config](http://man.openbsd.org/sshd_config) filen toohello */etc/ssh/* directory.</span><span class="sxs-lookup"><span data-stu-id="7615e-122">Add a [`COPY` instruction](https://docs.docker.com/engine/reference/builder/#copy) toohello Dockerfile toocopy a [sshd_config](http://man.openbsd.org/sshd_config) file toohello */etc/ssh/* directory.</span></span> <span data-ttu-id="7615e-123">Konfigurationsfilen ska baseras på vår sshd_config-filen i hello Azure App Service GitHub-lagringsplatsen [här](https://github.com/Azure-App-Service/node/blob/master/6.11/sshd_config).</span><span class="sxs-lookup"><span data-stu-id="7615e-123">Your configuration file should be based on our sshd_config file in hello Azure-App-Service GitHub repository [here](https://github.com/Azure-App-Service/node/blob/master/6.11/sshd_config).</span></span>

    > [!NOTE] 
    > <span data-ttu-id="7615e-124">Hej *sshd_config* filen måste innehålla följande hello eller hello anslutningen misslyckas:</span><span class="sxs-lookup"><span data-stu-id="7615e-124">hello *sshd_config* file must include hello following or hello connection fails:</span></span> 
    > * <span data-ttu-id="7615e-125">`Ciphers`måste innehålla minst en av följande hello: `aes128-cbc,3des-cbc,aes256-cbc`.</span><span class="sxs-lookup"><span data-stu-id="7615e-125">`Ciphers` must include at least one of hello following: `aes128-cbc,3des-cbc,aes256-cbc`.</span></span>
    > * <span data-ttu-id="7615e-126">`MACs`måste innehålla minst en av följande hello: `hmac-sha1,hmac-sha1-96`.</span><span class="sxs-lookup"><span data-stu-id="7615e-126">`MACs` must include at least one of hello following: `hmac-sha1,hmac-sha1-96`.</span></span>

    ```docker
    COPY sshd_config /etc/ssh/
    ```


3. <span data-ttu-id="7615e-127">Inkludera porten 2222 i hello [ `EXPOSE` instruktion](https://docs.docker.com/engine/reference/builder/#expose) för hello Dockerfile.</span><span class="sxs-lookup"><span data-stu-id="7615e-127">Include port 2222 in hello [`EXPOSE` instruction](https://docs.docker.com/engine/reference/builder/#expose) for hello Dockerfile.</span></span> <span data-ttu-id="7615e-128">Även om hello rotlösenordet är känd port 2222 kan inte nås från hello internet.</span><span class="sxs-lookup"><span data-stu-id="7615e-128">Although hello root password is known, port 2222 cannot be accessed from hello internet.</span></span> <span data-ttu-id="7615e-129">Det är ett internt endast port tillgänglig endast av behållare i hello brygga för nätverk i ett privat virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="7615e-129">It is an internal only port accessible only by containers within hello bridge network of a private virtual network.</span></span>

    ```docker
    EXPOSE 2222 80
    ```

4. <span data-ttu-id="7615e-130">Kontrollera att toostart hello ssh-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="7615e-130">Make sure toostart hello ssh service.</span></span> <span data-ttu-id="7615e-131">hello exempel [här](https://github.com/Azure-App-Service/node/blob/master/6.9.3/startup/init_container.sh) använder ett kommandoskript i */bin* directory.</span><span class="sxs-lookup"><span data-stu-id="7615e-131">hello example [here](https://github.com/Azure-App-Service/node/blob/master/6.9.3/startup/init_container.sh) uses a shell script in */bin* directory.</span></span>

    ```bash
    #!/bin/bash
    service ssh start
    ```

    <span data-ttu-id="7615e-132">Hej Dockerfile använder hello [ `CMD` instruktion](https://docs.docker.com/engine/reference/builder/#cmd) toorun hello skript.</span><span class="sxs-lookup"><span data-stu-id="7615e-132">hello Dockerfile uses hello [`CMD` instruction](https://docs.docker.com/engine/reference/builder/#cmd) toorun hello script.</span></span>

    ```docker
    COPY init_container.sh /bin/
      ...
    RUN chmod 755 /bin/init_container.sh 
      ...       
    CMD ["/bin/init_container.sh"]
    ```



## <a name="next-steps"></a><span data-ttu-id="7615e-133">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7615e-133">Next steps</span></span>
<span data-ttu-id="7615e-134">Se följande länkar för mer information om webbprogrammet på Linux hello.</span><span class="sxs-lookup"><span data-stu-id="7615e-134">See hello following links for more information regarding Web App on Linux.</span></span> <span data-ttu-id="7615e-135">Du kan publicera frågor och funderingar på [vårt forum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview).</span><span class="sxs-lookup"><span data-stu-id="7615e-135">You can post questions and concerns on [our forum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview).</span></span>

* [<span data-ttu-id="7615e-136">Hur toouse anpassade Docker bild för Azure Web App på Linux</span><span class="sxs-lookup"><span data-stu-id="7615e-136">How toouse a custom Docker image for Azure Web App on Linux</span></span>](app-service-linux-using-custom-docker-image.md)
* [<span data-ttu-id="7615e-137">Med PM2 konfiguration för Node.js i Azure Webbapp på Linux</span><span class="sxs-lookup"><span data-stu-id="7615e-137">Using PM2 Configuration for Node.js in Azure Web App on Linux</span></span>](app-service-linux-using-nodejs-pm2.md)
* [<span data-ttu-id="7615e-138">Med .NET Core i Azure-Webbapp på Linux</span><span class="sxs-lookup"><span data-stu-id="7615e-138">Using .NET Core in Azure Web App on Linux</span></span>](app-service-linux-using-dotnetcore.md)
* [<span data-ttu-id="7615e-139">Med hjälp av Ruby i Azure Webbapp på Linux</span><span class="sxs-lookup"><span data-stu-id="7615e-139">Using Ruby in Azure Web App on Linux</span></span>](app-service-linux-ruby-get-started.md)
* [<span data-ttu-id="7615e-140">Azure App Service Webbapp på Linux vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="7615e-140">Azure App Service Web App on Linux FAQ</span></span>](app-service-linux-faq.md)

