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
# <a name="using-a-custom-docker-image-for-azure-web-app-on-linux"></a><span data-ttu-id="ac20a-104">Använda en anpassad Docker-avbildning för Azure Web App på Linux</span><span class="sxs-lookup"><span data-stu-id="ac20a-104">Using a custom Docker image for Azure Web App on Linux</span></span> #

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]


<span data-ttu-id="ac20a-105">Apptjänst innehåller fördefinierade program stackar på Linux med stöd för specifika versioner, till exempel PHP 7.0 och Node.js 4.5.</span><span class="sxs-lookup"><span data-stu-id="ac20a-105">App Service provides pre-defined application stacks on Linux with support for specific versions, such as PHP 7.0 and Node.js 4.5.</span></span> <span data-ttu-id="ac20a-106">Apptjänst i Linux använder Docker behållare toohost dessa fördefinierade program stackar.</span><span class="sxs-lookup"><span data-stu-id="ac20a-106">App Service on Linux uses Docker containers toohost these pre-built application stacks.</span></span> <span data-ttu-id="ac20a-107">Du kan också använda en anpassad Docker bild toodeploy din web app tooan programstack som inte redan har definierats i Azure.</span><span class="sxs-lookup"><span data-stu-id="ac20a-107">You can also use a custom Docker image toodeploy your web app tooan application stack that is not already defined in Azure.</span></span> <span data-ttu-id="ac20a-108">Anpassade Docker-avbildningar kan finnas på antingen ett offentligt eller privat Docker-databasen.</span><span class="sxs-lookup"><span data-stu-id="ac20a-108">Custom Docker images can be hosted on either a public or private Docker repository.</span></span>


## <a name="how-to-set-a-custom-docker-image-for-a-web-app"></a><span data-ttu-id="ac20a-109">Så här: Ange en anpassad Docker-avbildning för en webbapp</span><span class="sxs-lookup"><span data-stu-id="ac20a-109">How to: set a custom Docker image for a web app</span></span>
<span data-ttu-id="ac20a-110">Du kan ange hello anpassad Docker-avbildning för både nya och befintliga webbplatser appar.</span><span class="sxs-lookup"><span data-stu-id="ac20a-110">You can set hello custom Docker image for both new and existing webs apps.</span></span> <span data-ttu-id="ac20a-111">När du skapar ett webbprogram på Linux i hello [Azure-portalen](https://portal.azure.com/#create/Microsoft.AppSvcLinux), klickar du på **konfigurera behållaren** tooset en anpassad avbildning Docker:</span><span class="sxs-lookup"><span data-stu-id="ac20a-111">When you create a web app on Linux in hello [Azure portal](https://portal.azure.com/#create/Microsoft.AppSvcLinux), click **Configure container** tooset a custom Docker image:</span></span>

![Anpassad Docker-avbildning för en ny webbapp i Linux][1]


## <a name="how-to-use-a-custom-docker-image-from-docker-hub"></a><span data-ttu-id="ac20a-113">Så här: använda en anpassad Docker-avbildning från Docker-hubb</span><span class="sxs-lookup"><span data-stu-id="ac20a-113">How to: use a custom Docker image from Docker Hub</span></span> ##
<span data-ttu-id="ac20a-114">toouse en anpassad Docker-avbildning från Docker-hubben:</span><span class="sxs-lookup"><span data-stu-id="ac20a-114">toouse a custom Docker image from Docker Hub:</span></span>

1. <span data-ttu-id="ac20a-115">I hello [Azure-portalen](https://portal.azure.com), leta upp ditt webbprogram på Linux, sedan i **inställningar** klickar du på **Dockerbehållare**.</span><span class="sxs-lookup"><span data-stu-id="ac20a-115">In hello [Azure portal](https://portal.azure.com), locate your web app on Linux, then in **Settings** click **Docker Container**.</span></span>

2.  <span data-ttu-id="ac20a-116">Välj **Docker-hubb** som hello **avbildningskällan**, klickar sedan på antingen **offentliga** eller **privata** och typen hello **avbildningen och valfria taggnamnet**, som `node:4.5`.</span><span class="sxs-lookup"><span data-stu-id="ac20a-116">Select **Docker Hub** as hello **Image source**, then click either **Public** or **Private** and type hello **Image and optional tag name**, such as `node:4.5`.</span></span> <span data-ttu-id="ac20a-117">Hej **startkommandot** set automatiskt utifrån vad som har definierats i hello Docker image-filen, men du kan ange egna kommandon.</span><span class="sxs-lookup"><span data-stu-id="ac20a-117">hello **Startup command** is set automatically based on what is defined in hello Docker image file, but you can set your own commands.</span></span>  

    ![Konfigurera Docker-hubb offentliga lagringsplats avbildningen][2]

    <span data-ttu-id="ac20a-119">När avbildningen är från en privat databas, behöver du också tooenter hello Docker hubb autentiseringsuppgifter som (**inloggning användarnamn** och **lösenord**) för hello privata Docker hubb lagringsplats.</span><span class="sxs-lookup"><span data-stu-id="ac20a-119">When your image is from a private repository, you also need tooenter hello Docker Hub credentials as (**Login username** and **Password**) for hello private Docker Hub repository.</span></span>

    ![Konfigurera Docker-hubb privata databasen avbildningen][3]

3. <span data-ttu-id="ac20a-121">När du har konfigurerat hello behållare, klickar du på **spara**.</span><span class="sxs-lookup"><span data-stu-id="ac20a-121">After you have configured hello container, click **Save**.</span></span>

## <a name="how-toouse-a-docker-image-from-a-private-image-registry"></a><span data-ttu-id="ac20a-122">Hur toouse en Docker bild av en privat image-registret</span><span class="sxs-lookup"><span data-stu-id="ac20a-122">How toouse a Docker image from a private image registry</span></span> ##
<span data-ttu-id="ac20a-123">toouse en anpassad avbildning Docker privata avbildningen registret:</span><span class="sxs-lookup"><span data-stu-id="ac20a-123">toouse a custom Docker image from a private image registry:</span></span>

1. <span data-ttu-id="ac20a-124">I hello [Azure-portalen](https://portal.azure.com), leta upp ditt webbprogram på Linux, sedan i **inställningar** klickar du på **Dockerbehållare**.</span><span class="sxs-lookup"><span data-stu-id="ac20a-124">In hello [Azure portal](https://portal.azure.com), locate your web app on Linux, then in **Settings** click **Docker Container**.</span></span>

2.  <span data-ttu-id="ac20a-125">Klicka på **privata registret** som hello **avbildningskällan**.</span><span class="sxs-lookup"><span data-stu-id="ac20a-125">Click **Private registry** as hello **Image source**.</span></span> <span data-ttu-id="ac20a-126">Ange hello **avbildningen och valfria taggnamnet**, **Serveradress** för hello privata registret, tillsammans med hello autentiseringsuppgifter (**inloggning användarnamn** och **lösenord **).</span><span class="sxs-lookup"><span data-stu-id="ac20a-126">Enter hello **Image and optional tag name**, **Server URL** for hello private registry, along with hello credentials (**Login username** and **Password**).</span></span> <span data-ttu-id="ac20a-127">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="ac20a-127">Click **Save**.</span></span>

    ![Konfigurera Docker-avbildningen från privata registret][4]


## <a name="how-to-set-hello-port-used-by-your-docker-image"></a><span data-ttu-id="ac20a-129">Så här: Ange hello-port som används av Docker-bild</span><span class="sxs-lookup"><span data-stu-id="ac20a-129">How to: set hello port used by your Docker image</span></span> ##

<span data-ttu-id="ac20a-130">När du använder en anpassad Docker-avbildning för ditt webbprogram, kan du använda hello `WEBSITES_PORT` miljövariabeln i din Dockerfile, läggs toohello genereras behållare.</span><span class="sxs-lookup"><span data-stu-id="ac20a-130">When you use a custom Docker image for your web app, you can use hello `WEBSITES_PORT` environment variable in your Dockerfile, which gets added toohello generated container.</span></span> <span data-ttu-id="ac20a-131">Överväg följande exempel på en docker-fil för Ruby programmet hello:</span><span class="sxs-lookup"><span data-stu-id="ac20a-131">Consider hello following example of a docker file for a Ruby application:</span></span>

    FROM ruby:2.2.0
    RUN mkdir /app
    WORKDIR /app
    ADD . /app
    RUN bundle install
    CMD bundle exec puma config.ru -p WEBSITES_PORT -e production

<span data-ttu-id="ac20a-132">Du kan se miljövariabel som hello WEBSITES_PORT skickas vid körning på sista raden i hello-kommando.</span><span class="sxs-lookup"><span data-stu-id="ac20a-132">On last line of hello command, you can see that hello WEBSITES_PORT environment variable is passed at runtime.</span></span> <span data-ttu-id="ac20a-133">Kom ihåg att skiftläge som är viktiga i kommandona.</span><span class="sxs-lookup"><span data-stu-id="ac20a-133">Remember that casing matters in commands.</span></span>

<span data-ttu-id="ac20a-134">Tidigare hello plattform med `PORT` app inställningen genom att vi planerar toodeprecate hello används den här appen inställningen och flytta toousing `WEBSITES_PORT` exklusivt.</span><span class="sxs-lookup"><span data-stu-id="ac20a-134">Previously hello platform was using `PORT` app setting, we are planning toodeprecate hello use this app setting and move toousing `WEBSITES_PORT` exclusively.</span></span>

<span data-ttu-id="ac20a-135">När du använder en befintlig Docker-avbildning som skapats av någon annan kan du behöva toospecify en annan port än port 80 för hello program.</span><span class="sxs-lookup"><span data-stu-id="ac20a-135">When you use an existing Docker image built by someone else, you may need toospecify a port other than port 80 for hello application.</span></span> <span data-ttu-id="ac20a-136">tooconfigure Hej port, Lägg till ett program som inställning med namnet `WEBSITES_PORT` med hello-värde som visas nedan:</span><span class="sxs-lookup"><span data-stu-id="ac20a-136">tooconfigure hello port, add an application setting named `WEBSITES_PORT` with hello value as shown below:</span></span>

![Konfigurera appen portinställning för anpassad Docker-avbildning][6]

## <a name="how-to-set-hello-startup-time-for-your-docker-image"></a><span data-ttu-id="ac20a-138">Så här: Ange hello starttiden för Docker-bild</span><span class="sxs-lookup"><span data-stu-id="ac20a-138">How to: set hello startup time for your Docker image</span></span> ##

<span data-ttu-id="ac20a-139">Som standard om din behållare inte startar innan 230 sekunder hello plattform startar om din behållare.</span><span class="sxs-lookup"><span data-stu-id="ac20a-139">By default, if your container does not start before 230 seconds, hello platform will restart your container.</span></span> <span data-ttu-id="ac20a-140">Om den anpassade Docker-avbildningen startas i mer än 230 sekunder, kan du använda hello `WEBSITES_CONTAINER_START_TIME_LIMIT` appen inställningen hello värdet för den här inställningen är i sekunder, det gör att hello plattform behålla din behållare som körs innan du startar om den.</span><span class="sxs-lookup"><span data-stu-id="ac20a-140">If your custom Docker image starts in more than 230 seconds, you can use hello `WEBSITES_CONTAINER_START_TIME_LIMIT` app setting, hello value for this setting is in seconds, this will allow hello platform keep your container running before restarting it.</span></span> <span data-ttu-id="ac20a-141">hello standardvärdet är 230 sekunder och hello högsta tillåtna värde är 600 sekunder.</span><span class="sxs-lookup"><span data-stu-id="ac20a-141">hello default value is 230 seconds, and hello max allowed value is 600 seconds.</span></span>

## <a name="how-to-unmount-hello-platform-provided-storage"></a><span data-ttu-id="ac20a-142">Så här: demontera hello plattform lagring</span><span class="sxs-lookup"><span data-stu-id="ac20a-142">How to: unmount hello platform provided storage</span></span> ##

<span data-ttu-id="ac20a-143">Som standard hello plattform att montera en beständig lagring resursen toohello `\home\` directory.</span><span class="sxs-lookup"><span data-stu-id="ac20a-143">By default, hello platform will mount a persistent storage share toohello `\home\` directory.</span></span> <span data-ttu-id="ac20a-144">Om behållaren avbildningen inte behöver ha en beständig resurs, kan du inaktivera montera den lagringen genom att ange hello `WEBSITES_ENABLE_APP_SERVICE_STORAGE` app inställning för`false`.</span><span class="sxs-lookup"><span data-stu-id="ac20a-144">If your container image does not need a persistent share, you can disable mounting that storage by setting hello `WEBSITES_ENABLE_APP_SERVICE_STORAGE` app setting too`false`.</span></span> <span data-ttu-id="ac20a-145">Du har fortfarande åtkomst toothat lagring från hello scm platsen och alla Docker-loggar (om aktiverat) kommer att skrivas toohello loggfiler som skapas av hello-plattformen.</span><span class="sxs-lookup"><span data-stu-id="ac20a-145">You will still have access toothat storage from hello scm site, and all Docker logs (if enabled) will be written toohello log files generated by hello platform.</span></span>

## <a name="how-to-switch-back-toousing-a-built-in-image"></a><span data-ttu-id="ac20a-146">Så här: växlar tillbaka toousing en inbyggd avbildning</span><span class="sxs-lookup"><span data-stu-id="ac20a-146">How to: Switch back toousing a built-in image</span></span> ##

<span data-ttu-id="ac20a-147">tooswitch från att använda en anpassad avbildning toousing en inbyggd avbildning:</span><span class="sxs-lookup"><span data-stu-id="ac20a-147">tooswitch from using a custom image toousing a built-in image:</span></span>

1. <span data-ttu-id="ac20a-148">I hello [Azure-portalen](https://portal.azure.com), leta upp ditt webbprogram på Linux, sedan i **inställningar** klickar du på **Apptjänst**.</span><span class="sxs-lookup"><span data-stu-id="ac20a-148">In hello [Azure portal](https://portal.azure.com), locate your web app on Linux, then in **Settings** click **App Service**.</span></span>

2. <span data-ttu-id="ac20a-149">Välj din **Runtime Stack** toouse för hello inbyggda bilden, klicka på **spara**.</span><span class="sxs-lookup"><span data-stu-id="ac20a-149">Select your **Runtime Stack** toouse for hello built-in image, then click **Save**.</span></span> 

![Konfigurera inbyggda Docker-avbildningen][5]


## <a name="troubleshooting"></a><span data-ttu-id="ac20a-151">Felsökning</span><span class="sxs-lookup"><span data-stu-id="ac20a-151">Troubleshooting</span></span> ##

<span data-ttu-id="ac20a-152">Kontrollera hello Docker loggar i hello loggfilerna directory om programmet misslyckas toostart med den anpassade Docker-avbildningen.</span><span class="sxs-lookup"><span data-stu-id="ac20a-152">When your application fails toostart with your custom Docker image, check hello Docker logs in hello LogFiles directory.</span></span> <span data-ttu-id="ac20a-153">Du kan komma åt den här katalogen via webbplatsen SCM eller via FTP.</span><span class="sxs-lookup"><span data-stu-id="ac20a-153">You can access this directory either through your SCM site or via FTP.</span></span>
<span data-ttu-id="ac20a-154">toolog hello `stdout` och `stderr` från din behållaren måste tooenable **Dockerbehållare loggning** under **diagnostik loggar**.</span><span class="sxs-lookup"><span data-stu-id="ac20a-154">toolog hello `stdout` and `stderr` from your container, you need tooenable **Docker Container logging** under **Diagnostics Logs**.</span></span>

![Aktivera loggning][8]

![Med Kudu tooview Docker-loggar][7]

<span data-ttu-id="ac20a-157">Du kan komma åt hello SCM plats från **avancerade verktyg** i hello **utvecklingsverktyg** menyn.</span><span class="sxs-lookup"><span data-stu-id="ac20a-157">You can access hello SCM site from **Advanced Tools** in hello **Development Tools** menu.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ac20a-158">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ac20a-158">Next Steps</span></span> ##

<span data-ttu-id="ac20a-159">Följ hello följande länkar tooget igång med Webbappen på Linux.</span><span class="sxs-lookup"><span data-stu-id="ac20a-159">Follow hello following links tooget started with Web App on Linux.</span></span>   

* [<span data-ttu-id="ac20a-160">Introduktion tooAzure webbprogrammet på Linux</span><span class="sxs-lookup"><span data-stu-id="ac20a-160">Introduction tooAzure Web App on Linux</span></span>](./app-service-linux-intro.md)
* [<span data-ttu-id="ac20a-161">Med PM2 konfiguration för Node.js i Azure Webbapp på Linux</span><span class="sxs-lookup"><span data-stu-id="ac20a-161">Using PM2 Configuration for Node.js in Azure Web App on Linux</span></span>](./app-service-linux-using-nodejs-pm2.md)
* [<span data-ttu-id="ac20a-162">Azure App Service Webbapp på Linux vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="ac20a-162">Azure App Service Web App on Linux FAQ</span></span>](app-service-linux-faq.md)

<span data-ttu-id="ac20a-163">Bokför frågor och funderingar på [vårt forum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview).</span><span class="sxs-lookup"><span data-stu-id="ac20a-163">Post questions and concerns on [our forum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview).</span></span>


<!--Image references-->
[1]: ./media/app-service-linux-using-custom-docker-image/new-configure-container.png
[2]: ./media/app-service-linux-using-custom-docker-image/existingapp-configure-dockerhub-public.png
[3]: ./media/app-service-linux-using-custom-docker-image/existingapp-configure-dockerhub-private.png
[4]: ./media/app-service-linux-using-custom-docker-image/existingapp-configure-privateregistry.png
[5]: ./media/app-service-linux-using-custom-docker-image/existingapp-configure-builtin.png
[6]: ./media/app-service-linux-using-custom-docker-image/setting-port.png
[7]: ./media/app-service-linux-using-custom-docker-image/kudu-docker-logs.png
[8]: ./media/app-service-linux-using-custom-docker-image/logging.png
