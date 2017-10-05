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
# <a name="using-a-custom-docker-image-for-azure-web-app-on-linux"></a><span data-ttu-id="b7e45-104">Använda en anpassad Docker-avbildning för Azure Web App på Linux</span><span class="sxs-lookup"><span data-stu-id="b7e45-104">Using a custom Docker image for Azure Web App on Linux</span></span> #

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]


<span data-ttu-id="b7e45-105">Apptjänst innehåller fördefinierade program stackar på Linux med stöd för specifika versioner, till exempel PHP 7.0 och Node.js 4.5.</span><span class="sxs-lookup"><span data-stu-id="b7e45-105">App Service provides pre-defined application stacks on Linux with support for specific versions, such as PHP 7.0 and Node.js 4.5.</span></span> <span data-ttu-id="b7e45-106">Apptjänst i Linux använder Docker-behållare som värd för dessa grupper med fördefinierade program.</span><span class="sxs-lookup"><span data-stu-id="b7e45-106">App Service on Linux uses Docker containers to host these pre-built application stacks.</span></span> <span data-ttu-id="b7e45-107">Du kan också använda en anpassad Docker-avbildning för att distribuera ditt webbprogram till en programstack som inte redan har definierats i Azure.</span><span class="sxs-lookup"><span data-stu-id="b7e45-107">You can also use a custom Docker image to deploy your web app to an application stack that is not already defined in Azure.</span></span> <span data-ttu-id="b7e45-108">Anpassade Docker-avbildningar kan finnas på antingen ett offentligt eller privat Docker-databasen.</span><span class="sxs-lookup"><span data-stu-id="b7e45-108">Custom Docker images can be hosted on either a public or private Docker repository.</span></span>


## <a name="how-to-set-a-custom-docker-image-for-a-web-app"></a><span data-ttu-id="b7e45-109">Så här: Ange en anpassad Docker-avbildning för en webbapp</span><span class="sxs-lookup"><span data-stu-id="b7e45-109">How to: set a custom Docker image for a web app</span></span>
<span data-ttu-id="b7e45-110">Du kan ange den anpassade Docker-avbildningen för både nya och befintliga webbplatser appar.</span><span class="sxs-lookup"><span data-stu-id="b7e45-110">You can set the custom Docker image for both new and existing webs apps.</span></span> <span data-ttu-id="b7e45-111">När du skapar ett webbprogram på Linux i den [Azure-portalen](https://portal.azure.com/#create/Microsoft.AppSvcLinux), klickar du på **konfigurera behållaren** att ange en anpassad avbildning Docker:</span><span class="sxs-lookup"><span data-stu-id="b7e45-111">When you create a web app on Linux in the [Azure portal](https://portal.azure.com/#create/Microsoft.AppSvcLinux), click **Configure container** to set a custom Docker image:</span></span>

![Anpassad Docker-avbildning för en ny webbapp i Linux][1]


## <a name="how-to-use-a-custom-docker-image-from-docker-hub"></a><span data-ttu-id="b7e45-113">Så här: använda en anpassad Docker-avbildning från Docker-hubb</span><span class="sxs-lookup"><span data-stu-id="b7e45-113">How to: use a custom Docker image from Docker Hub</span></span> ##
<span data-ttu-id="b7e45-114">Använda en anpassad Docker-avbildning från Docker-hubben:</span><span class="sxs-lookup"><span data-stu-id="b7e45-114">To use a custom Docker image from Docker Hub:</span></span>

1. <span data-ttu-id="b7e45-115">I den [Azure-portalen](https://portal.azure.com), leta upp ditt webbprogram på Linux, sedan i **inställningar** klickar du på **Dockerbehållare**.</span><span class="sxs-lookup"><span data-stu-id="b7e45-115">In the [Azure portal](https://portal.azure.com), locate your web app on Linux, then in **Settings** click **Docker Container**.</span></span>

2.  <span data-ttu-id="b7e45-116">Välj **Docker-hubb** som den **Bildkälla**, klicka på antingen **offentliga** eller **privata** och skriver den **avbildningen och valfria taggnamnet**, som `node:4.5`.</span><span class="sxs-lookup"><span data-stu-id="b7e45-116">Select **Docker Hub** as the **Image source**, then click either **Public** or **Private** and type the **Image and optional tag name**, such as `node:4.5`.</span></span> <span data-ttu-id="b7e45-117">Den **startkommandot** set automatiskt utifrån vad som har definierats i Docker image-filen, men du kan ange egna kommandon.</span><span class="sxs-lookup"><span data-stu-id="b7e45-117">The **Startup command** is set automatically based on what is defined in the Docker image file, but you can set your own commands.</span></span>  

    ![Konfigurera Docker-hubb offentliga lagringsplats avbildningen][2]

    <span data-ttu-id="b7e45-119">När avbildningen är från en privat databas kan du också behöva ange autentiseringsuppgifterna för Docker-hubb som (**inloggning användarnamn** och **lösenord**) för privata Docker-hubb-databasen.</span><span class="sxs-lookup"><span data-stu-id="b7e45-119">When your image is from a private repository, you also need to enter the Docker Hub credentials as (**Login username** and **Password**) for the private Docker Hub repository.</span></span>

    ![Konfigurera Docker-hubb privata databasen avbildningen][3]

3. <span data-ttu-id="b7e45-121">När du har konfigurerat behållaren, klickar du på **spara**.</span><span class="sxs-lookup"><span data-stu-id="b7e45-121">After you have configured the container, click **Save**.</span></span>

## <a name="how-to-use-a-docker-image-from-a-private-image-registry"></a><span data-ttu-id="b7e45-122">Hur du använder en Docker-avbildning från ett privat avbildningen register</span><span class="sxs-lookup"><span data-stu-id="b7e45-122">How to use a Docker image from a private image registry</span></span> ##
<span data-ttu-id="b7e45-123">Använda en anpassad Docker-avbildning från ett privat avbildningen registret:</span><span class="sxs-lookup"><span data-stu-id="b7e45-123">To use a custom Docker image from a private image registry:</span></span>

1. <span data-ttu-id="b7e45-124">I den [Azure-portalen](https://portal.azure.com), leta upp ditt webbprogram på Linux, sedan i **inställningar** klickar du på **Dockerbehållare**.</span><span class="sxs-lookup"><span data-stu-id="b7e45-124">In the [Azure portal](https://portal.azure.com), locate your web app on Linux, then in **Settings** click **Docker Container**.</span></span>

2.  <span data-ttu-id="b7e45-125">Klicka på **privata registret** som den **avbildningskällan**.</span><span class="sxs-lookup"><span data-stu-id="b7e45-125">Click **Private registry** as the **Image source**.</span></span> <span data-ttu-id="b7e45-126">Ange den **avbildningen och valfria taggnamnet**, **Serveradress** för privata registret, tillsammans med autentiseringsuppgifterna (**inloggning användarnamn** och **lösenord**).</span><span class="sxs-lookup"><span data-stu-id="b7e45-126">Enter the **Image and optional tag name**, **Server URL** for the private registry, along with the credentials (**Login username** and **Password**).</span></span> <span data-ttu-id="b7e45-127">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="b7e45-127">Click **Save**.</span></span>

    ![Konfigurera Docker-avbildningen från privata registret][4]


## <a name="how-to-set-the-port-used-by-your-docker-image"></a><span data-ttu-id="b7e45-129">Så här: Ange den port som används av Docker-avbildningen</span><span class="sxs-lookup"><span data-stu-id="b7e45-129">How to: set the port used by your Docker image</span></span> ##

<span data-ttu-id="b7e45-130">När du använder en anpassad Docker-avbildning för ditt webbprogram, kan du använda den `WEBSITES_PORT` miljövariabeln i din Dockerfile som läggs till skapas behållaren.</span><span class="sxs-lookup"><span data-stu-id="b7e45-130">When you use a custom Docker image for your web app, you can use the `WEBSITES_PORT` environment variable in your Dockerfile, which gets added to the generated container.</span></span> <span data-ttu-id="b7e45-131">Överväg följande exempel visar en docker-fil för ett Ruby program:</span><span class="sxs-lookup"><span data-stu-id="b7e45-131">Consider the following example of a docker file for a Ruby application:</span></span>

    FROM ruby:2.2.0
    RUN mkdir /app
    WORKDIR /app
    ADD . /app
    RUN bundle install
    CMD bundle exec puma config.ru -p WEBSITES_PORT -e production

<span data-ttu-id="b7e45-132">Du kan se att miljövariabeln WEBSITES_PORT överförs vid körning på sista raden i kommandot.</span><span class="sxs-lookup"><span data-stu-id="b7e45-132">On last line of the command, you can see that the WEBSITES_PORT environment variable is passed at runtime.</span></span> <span data-ttu-id="b7e45-133">Kom ihåg att skiftläge som är viktiga i kommandona.</span><span class="sxs-lookup"><span data-stu-id="b7e45-133">Remember that casing matters in commands.</span></span>

<span data-ttu-id="b7e45-134">Tidigare plattformen använde `PORT` app inställningen genom att vi planerar att inaktualisera använda den här appen inställningen och gå till med hjälp av `WEBSITES_PORT` exklusivt.</span><span class="sxs-lookup"><span data-stu-id="b7e45-134">Previously the platform was using `PORT` app setting, we are planning to deprecate the use this app setting and move to using `WEBSITES_PORT` exclusively.</span></span>

<span data-ttu-id="b7e45-135">När du använder en befintlig Docker-avbildning som skapats av någon annan, kan du behöva ange en annan port än port 80 för programmet.</span><span class="sxs-lookup"><span data-stu-id="b7e45-135">When you use an existing Docker image built by someone else, you may need to specify a port other than port 80 for the application.</span></span> <span data-ttu-id="b7e45-136">Konfigurera porten genom att lägga till ett program som inställning med namnet `WEBSITES_PORT` med värdet som visas nedan:</span><span class="sxs-lookup"><span data-stu-id="b7e45-136">To configure the port, add an application setting named `WEBSITES_PORT` with the value as shown below:</span></span>

![Konfigurera appen portinställning för anpassad Docker-avbildning][6]

## <a name="how-to-set-the-startup-time-for-your-docker-image"></a><span data-ttu-id="b7e45-138">Så här: Ange starttiden för Docker-bild</span><span class="sxs-lookup"><span data-stu-id="b7e45-138">How to: set the startup time for your Docker image</span></span> ##

<span data-ttu-id="b7e45-139">Som standard om din behållare inte startar innan 230 sekunder plattformen startar om din behållare.</span><span class="sxs-lookup"><span data-stu-id="b7e45-139">By default, if your container does not start before 230 seconds, the platform will restart your container.</span></span> <span data-ttu-id="b7e45-140">Om den anpassade Docker-avbildningen startas i mer än 230 sekunder, du kan använda den `WEBSITES_CONTAINER_START_TIME_LIMIT` appen inställningen genom att värdet för den här inställningen är i sekunder, det gör att plattform behålla din behållare som körs innan du startar om den.</span><span class="sxs-lookup"><span data-stu-id="b7e45-140">If your custom Docker image starts in more than 230 seconds, you can use the `WEBSITES_CONTAINER_START_TIME_LIMIT` app setting, the value for this setting is in seconds, this will allow the platform keep your container running before restarting it.</span></span> <span data-ttu-id="b7e45-141">Standardvärdet är 230 sekunder och det högsta tillåtna värdet är 600 sekunder.</span><span class="sxs-lookup"><span data-stu-id="b7e45-141">The default value is 230 seconds, and the max allowed value is 600 seconds.</span></span>

## <a name="how-to-unmount-the-platform-provided-storage"></a><span data-ttu-id="b7e45-142">Så här: demontera plattform lagring</span><span class="sxs-lookup"><span data-stu-id="b7e45-142">How to: unmount the platform provided storage</span></span> ##

<span data-ttu-id="b7e45-143">Som standard kommer plattformen montera en beständig storage-resurs för den `\home\` directory.</span><span class="sxs-lookup"><span data-stu-id="b7e45-143">By default, the platform will mount a persistent storage share to the `\home\` directory.</span></span> <span data-ttu-id="b7e45-144">Om behållaren avbildningen inte behöver ha en beständig resurs, kan du inaktivera montera den lagringen genom att ange den `WEBSITES_ENABLE_APP_SERVICE_STORAGE` appinställningen `false`.</span><span class="sxs-lookup"><span data-stu-id="b7e45-144">If your container image does not need a persistent share, you can disable mounting that storage by setting the `WEBSITES_ENABLE_APP_SERVICE_STORAGE` app setting to `false`.</span></span> <span data-ttu-id="b7e45-145">Du har fortfarande åtkomst till den lagringsplats från scm-platsen och alla Docker-loggar (om aktiverat) kommer att skrivas till loggfiler som skapas av plattformen.</span><span class="sxs-lookup"><span data-stu-id="b7e45-145">You will still have access to that storage from the scm site, and all Docker logs (if enabled) will be written to the log files generated by the platform.</span></span>

## <a name="how-to-switch-back-to-using-a-built-in-image"></a><span data-ttu-id="b7e45-146">Så här: växla tillbaka till med hjälp av en inbyggd avbildning</span><span class="sxs-lookup"><span data-stu-id="b7e45-146">How to: Switch back to using a built-in image</span></span> ##

<span data-ttu-id="b7e45-147">Växla från att använda en anpassad avbildning till med hjälp av en inbyggd avbildning:</span><span class="sxs-lookup"><span data-stu-id="b7e45-147">To switch from using a custom image to using a built-in image:</span></span>

1. <span data-ttu-id="b7e45-148">I den [Azure-portalen](https://portal.azure.com), leta upp ditt webbprogram på Linux, sedan i **inställningar** klickar du på **Apptjänst**.</span><span class="sxs-lookup"><span data-stu-id="b7e45-148">In the [Azure portal](https://portal.azure.com), locate your web app on Linux, then in **Settings** click **App Service**.</span></span>

2. <span data-ttu-id="b7e45-149">Välj din **Runtime Stack** om du vill använda för den inbyggda avbildningen Klicka **spara**.</span><span class="sxs-lookup"><span data-stu-id="b7e45-149">Select your **Runtime Stack** to use for the built-in image, then click **Save**.</span></span> 

![Konfigurera inbyggda Docker-avbildningen][5]


## <a name="troubleshooting"></a><span data-ttu-id="b7e45-151">Felsökning</span><span class="sxs-lookup"><span data-stu-id="b7e45-151">Troubleshooting</span></span> ##

<span data-ttu-id="b7e45-152">Kontrollera Docker loggar i katalogen loggfiler när programmet inte startar med den anpassade Docker-avbildningen.</span><span class="sxs-lookup"><span data-stu-id="b7e45-152">When your application fails to start with your custom Docker image, check the Docker logs in the LogFiles directory.</span></span> <span data-ttu-id="b7e45-153">Du kan komma åt den här katalogen via webbplatsen SCM eller via FTP.</span><span class="sxs-lookup"><span data-stu-id="b7e45-153">You can access this directory either through your SCM site or via FTP.</span></span>
<span data-ttu-id="b7e45-154">Loggen i `stdout` och `stderr` från din behållare, måste du aktivera **Dockerbehållare loggning** under **diagnostik loggar**.</span><span class="sxs-lookup"><span data-stu-id="b7e45-154">To log the `stdout` and `stderr` from your container, you need to enable **Docker Container logging** under **Diagnostics Logs**.</span></span>

![Aktivera loggning][8]

![Använda Kudu för att visa Docker-loggar][7]

<span data-ttu-id="b7e45-157">Du kan komma åt webbplatsen SCM från **avancerade verktyg** i den **utvecklingsverktyg** menyn.</span><span class="sxs-lookup"><span data-stu-id="b7e45-157">You can access the SCM site from **Advanced Tools** in the **Development Tools** menu.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b7e45-158">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b7e45-158">Next Steps</span></span> ##

<span data-ttu-id="b7e45-159">Följ länken nedan för att komma igång med Webbappen på Linux.</span><span class="sxs-lookup"><span data-stu-id="b7e45-159">Follow the following links to get started with Web App on Linux.</span></span>   

* [<span data-ttu-id="b7e45-160">Introduktion till Azure-Webbapp på Linux</span><span class="sxs-lookup"><span data-stu-id="b7e45-160">Introduction to Azure Web App on Linux</span></span>](./app-service-linux-intro.md)
* [<span data-ttu-id="b7e45-161">Med PM2 konfiguration för Node.js i Azure Webbapp på Linux</span><span class="sxs-lookup"><span data-stu-id="b7e45-161">Using PM2 Configuration for Node.js in Azure Web App on Linux</span></span>](./app-service-linux-using-nodejs-pm2.md)
* [<span data-ttu-id="b7e45-162">Azure App Service Webbapp på Linux vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="b7e45-162">Azure App Service Web App on Linux FAQ</span></span>](app-service-linux-faq.md)

<span data-ttu-id="b7e45-163">Bokför frågor och funderingar på [vårt forum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview).</span><span class="sxs-lookup"><span data-stu-id="b7e45-163">Post questions and concerns on [our forum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview).</span></span>


<!--Image references-->
[1]: ./media/app-service-linux-using-custom-docker-image/new-configure-container.png
[2]: ./media/app-service-linux-using-custom-docker-image/existingapp-configure-dockerhub-public.png
[3]: ./media/app-service-linux-using-custom-docker-image/existingapp-configure-dockerhub-private.png
[4]: ./media/app-service-linux-using-custom-docker-image/existingapp-configure-privateregistry.png
[5]: ./media/app-service-linux-using-custom-docker-image/existingapp-configure-builtin.png
[6]: ./media/app-service-linux-using-custom-docker-image/setting-port.png
[7]: ./media/app-service-linux-using-custom-docker-image/kudu-docker-logs.png
[8]: ./media/app-service-linux-using-custom-docker-image/logging.png
