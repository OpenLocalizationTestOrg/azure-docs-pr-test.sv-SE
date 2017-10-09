---
title: "aaaAzure App Service Web App på Linux vanliga frågor och svar | Microsoft Docs"
description: "Azure App Service Webbapp på Linux vanliga frågor och svar."
keywords: "Azure apptjänst, webbprogram, vanliga frågor och svar, linux, oss"
services: app-service
documentationCenter: 
author: ahmedelnably
manager: erikre
editor: 
ms.assetid: 
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: aelnably;wesmc
ms.openlocfilehash: c7798d9144d936eecdc0e191fc870b0ee0b220c8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-app-service-web-app-on-linux-faq"></a><span data-ttu-id="076a5-104">Azure App Service Webbapp på Linux vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="076a5-104">Azure App Service Web App on Linux FAQ</span></span>

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]


<span data-ttu-id="076a5-105">Hello versionen av webbprogram på Linux arbetar vi med att lägga till funktioner och göra förbättringar tooour plattform.</span><span class="sxs-lookup"><span data-stu-id="076a5-105">With hello release of Web App on Linux, we're working on adding features and making improvements tooour platform.</span></span> <span data-ttu-id="076a5-106">Här är några vanliga frågor (FAQ) som våra kunder har efterfrågat oss via hello senast månader.</span><span class="sxs-lookup"><span data-stu-id="076a5-106">Here are some frequently asked questions (FAQ) that our customers have been asking us over hello last months.</span></span>
<span data-ttu-id="076a5-107">Om du har en fråga, kommentera hello artikeln och vi kommer att svara så snart som möjligt.</span><span class="sxs-lookup"><span data-stu-id="076a5-107">If you have a question, comment on hello article and we'll answer it as soon as possible.</span></span>

## <a name="built-in-images"></a><span data-ttu-id="076a5-108">Inbyggda bilder</span><span class="sxs-lookup"><span data-stu-id="076a5-108">Built-in images</span></span>

<span data-ttu-id="076a5-109">**F:** jag vill toofork hello inbyggda Docker-behållare som hello plattform tillhandahåller.</span><span class="sxs-lookup"><span data-stu-id="076a5-109">**Q:** I want toofork hello built-in Docker containers that hello platform provides.</span></span> <span data-ttu-id="076a5-110">Var hittar jag dessa filer?</span><span class="sxs-lookup"><span data-stu-id="076a5-110">Where can I find those files?</span></span>

<span data-ttu-id="076a5-111">**S:** hittar du alla Docker-filer på [GitHub](https://github.com/azure-app-service).</span><span class="sxs-lookup"><span data-stu-id="076a5-111">**A:** You can find all Docker files on [GitHub](https://github.com/azure-app-service).</span></span> <span data-ttu-id="076a5-112">Du hittar alla behållare i Docker på [Docker-hubb](https://hub.docker.com/u/appsvc/).</span><span class="sxs-lookup"><span data-stu-id="076a5-112">You can find all Docker containers on [Docker Hub](https://hub.docker.com/u/appsvc/).</span></span>

<span data-ttu-id="076a5-113">**F:** vilka är hello förväntade värden för hello startfil avsnitt när jag konfigurera hello runtime stack?</span><span class="sxs-lookup"><span data-stu-id="076a5-113">**Q:** What are hello expected values for hello Startup File section when I configure hello runtime stack?</span></span>

<span data-ttu-id="076a5-114">**S:** för Node.Js som du anger hello PM2 konfigurationsfilen eller skriptfilen.</span><span class="sxs-lookup"><span data-stu-id="076a5-114">**A:** For Node.Js, you specify hello PM2 configuration file or your script file.</span></span> <span data-ttu-id="076a5-115">Ange namnet på din kompilerade DLL-filen för .NET Core.</span><span class="sxs-lookup"><span data-stu-id="076a5-115">For .NET Core, specify your compiled DLL name.</span></span> <span data-ttu-id="076a5-116">För Ruby, kan du ange hello Ruby skript som du vill tooinitialize din app med.</span><span class="sxs-lookup"><span data-stu-id="076a5-116">For Ruby, you can specify hello Ruby script that you want tooinitialize your app with.</span></span>

## <a name="management"></a><span data-ttu-id="076a5-117">Hantering</span><span class="sxs-lookup"><span data-stu-id="076a5-117">Management</span></span>

<span data-ttu-id="076a5-118">**F:** vad som händer när jag trycka hello omstart av-knappen i hello Azure-portalen?</span><span class="sxs-lookup"><span data-stu-id="076a5-118">**Q:** What happens when I press hello restart button in hello Azure portal?</span></span>

<span data-ttu-id="076a5-119">**S:** detta är hello motsvarighet till Docker omstart.</span><span class="sxs-lookup"><span data-stu-id="076a5-119">**A:** This is hello equivalent of Docker restart.</span></span>

<span data-ttu-id="076a5-120">**F:** kan jag använda SSH (Secure Shell) tooconnect toohello app behållaren virtuell dator (VM)?</span><span class="sxs-lookup"><span data-stu-id="076a5-120">**Q:** Can I use Secure Shell (SSH) tooconnect toohello app container virtual machine (VM)?</span></span>

<span data-ttu-id="076a5-121">**S:** Ja, du kan göra att kontrollera hello följande artikel via hello SCM-webbplatsen för mer information [SSH-stöd för webbprogrammet på Linux](./app-service-linux-ssh-support.md)</span><span class="sxs-lookup"><span data-stu-id="076a5-121">**A:** Yes, you can do that through hello SCM site, check hello following article for more information [SSH support for Web App on Linux](./app-service-linux-ssh-support.md)</span></span>

<span data-ttu-id="076a5-122">**F:** jag vill toocreate ett Linux App Service-plan från SDK eller en ARM-mall, hur kan jag göra detta?</span><span class="sxs-lookup"><span data-stu-id="076a5-122">**Q:** I want toocreate a Linux App Service plane through SDK or an ARM template, how can I achieve this?</span></span>

<span data-ttu-id="076a5-123">**S:** måste tooset hello `reserved` för hello app service för`true`.</span><span class="sxs-lookup"><span data-stu-id="076a5-123">**A:** You need tooset hello `reserved` field of hello app service too`true`.</span></span>

## <a name="continuous-integrationdeployment"></a><span data-ttu-id="076a5-124">Kontinuerlig integration/distribution</span><span class="sxs-lookup"><span data-stu-id="076a5-124">Continuous integration/deployment</span></span>

<span data-ttu-id="076a5-125">**F:** webbappen fortfarande använder en gammal Docker behållare avbildning när jag har uppdaterat hello avbildningen på Docker-hubben.</span><span class="sxs-lookup"><span data-stu-id="076a5-125">**Q:** My web app still uses an old Docker container image after I've updated hello image on Docker Hub.</span></span> <span data-ttu-id="076a5-126">Stöder kontinuerlig integration/distribution av anpassade behållare?</span><span class="sxs-lookup"><span data-stu-id="076a5-126">Do you support continuous integration/deployment of custom containers?</span></span>

<span data-ttu-id="076a5-127">**S:** tooset in kontinuerlig integration/distribution för Azure-behållare registret eller DockerHub bilder genom att kontrollera hello följande artikel [kontinuerlig distribution med Azure Web App på Linux](./app-service-linux-ci-cd.md).</span><span class="sxs-lookup"><span data-stu-id="076a5-127">**A:** tooset up continuous integration/deployment for Azure Container Registry or DockerHub images by check hello following article [Continuous Deployment with Azure Web App on Linux](./app-service-linux-ci-cd.md).</span></span> <span data-ttu-id="076a5-128">Du kan uppdatera hello behållare genom att stoppa och starta ditt webbprogram för privata register.</span><span class="sxs-lookup"><span data-stu-id="076a5-128">For private registries, you can refresh hello container by stopping and then starting your web app.</span></span> <span data-ttu-id="076a5-129">Eller kan du ändra eller lägga till en dummy tillämpningsinställningen tooforce en uppdatering av din behållare.</span><span class="sxs-lookup"><span data-stu-id="076a5-129">Or you can change or add a dummy application setting tooforce a refresh of your container.</span></span>

<span data-ttu-id="076a5-130">**F:** stöder du fristående datorn?</span><span class="sxs-lookup"><span data-stu-id="076a5-130">**Q:** Do you support staging environments?</span></span>

<span data-ttu-id="076a5-131">**S:** Ja.</span><span class="sxs-lookup"><span data-stu-id="076a5-131">**A:** Yes.</span></span>

<span data-ttu-id="076a5-132">**F:** kan du använda **webbdistribution** toodeploy webbappen?</span><span class="sxs-lookup"><span data-stu-id="076a5-132">**Q:** Can I use **web deploy** toodeploy my web app?</span></span>

<span data-ttu-id="076a5-133">**S:** Ja, du behöver tooset en app inställningen kallas `WEBSITE_WEBDEPLOY_USE_SCM` för`false`.</span><span class="sxs-lookup"><span data-stu-id="076a5-133">**A:** Yes, you need tooset an app setting called `WEBSITE_WEBDEPLOY_USE_SCM` too`false`.</span></span>

## <a name="language-support"></a><span data-ttu-id="076a5-134">Språkstöd</span><span class="sxs-lookup"><span data-stu-id="076a5-134">Language support</span></span>

<span data-ttu-id="076a5-135">**F:** stöder du Okompilerade .NET Core appar?</span><span class="sxs-lookup"><span data-stu-id="076a5-135">**Q:** Do you support uncompiled .NET Core apps?</span></span>

<span data-ttu-id="076a5-136">**S:** Ja.</span><span class="sxs-lookup"><span data-stu-id="076a5-136">**A:** Yes.</span></span>

<span data-ttu-id="076a5-137">**F:** stöder du Composer som en beroende-hanterare för PHP-appar?</span><span class="sxs-lookup"><span data-stu-id="076a5-137">**Q:** Do you support Composer as a dependency manager for PHP apps?</span></span>

<span data-ttu-id="076a5-138">**S:** Ja.</span><span class="sxs-lookup"><span data-stu-id="076a5-138">**A:** Yes.</span></span> <span data-ttu-id="076a5-139">Under en Git-distribution bör Kudu identifiera som du distribuerar ett PHP-program (tack toohello förekomsten av en composer.json-fil) och utlöser en composer installation för dig.</span><span class="sxs-lookup"><span data-stu-id="076a5-139">During a Git deployment, Kudu should detect that you are deploying a PHP application (thanks toohello presence of a composer.json file) and will trigger a composer install for you.</span></span>

## <a name="custom-containers"></a><span data-ttu-id="076a5-140">Anpassade behållare</span><span class="sxs-lookup"><span data-stu-id="076a5-140">Custom containers</span></span>

<span data-ttu-id="076a5-141">**F:** jag använder egna anpassade container.</span><span class="sxs-lookup"><span data-stu-id="076a5-141">**Q:** I'm using my own custom container.</span></span> <span data-ttu-id="076a5-142">Min app finns i hello `\home\` directory, men jag kan inte hitta Mina filer när jag söker hello innehåll med hjälp av hello [SCM plats](https://github.com/projectkudu/kudu) eller en FTP-klient.</span><span class="sxs-lookup"><span data-stu-id="076a5-142">My app resides in hello `\home\` directory, but I can't find my files when I browse hello content by using hello [SCM site](https://github.com/projectkudu/kudu) or an FTP client.</span></span> <span data-ttu-id="076a5-143">Var finns Mina filer?</span><span class="sxs-lookup"><span data-stu-id="076a5-143">Where are my files?</span></span>

<span data-ttu-id="076a5-144">**S:** vi montera en SMB-resurs toohello `\home\` directory.</span><span class="sxs-lookup"><span data-stu-id="076a5-144">**A:** We mount an SMB share toohello `\home\` directory.</span></span> <span data-ttu-id="076a5-145">Det åsidosätter allt innehåll som redan finns där.</span><span class="sxs-lookup"><span data-stu-id="076a5-145">This will override any content that's there.</span></span>

<span data-ttu-id="076a5-146">**F:** jag använder egna anpassade container.</span><span class="sxs-lookup"><span data-stu-id="076a5-146">**Q:** I'm using my own custom container.</span></span> <span data-ttu-id="076a5-147">Jag vill inte hello plattform toomount en SMB-resurs toohello `\home\`.</span><span class="sxs-lookup"><span data-stu-id="076a5-147">I don't want hello platform toomount an SMB share toohello `\home\`.</span></span>

<span data-ttu-id="076a5-148">**S:** kan du göra det genom att ange hello `WEBSITES_ENABLE_APP_SERVICE_STORAGE` app inställning för`false`.</span><span class="sxs-lookup"><span data-stu-id="076a5-148">**A:** You can do that by setting hello `WEBSITES_ENABLE_APP_SERVICE_STORAGE` app setting too`false`.</span></span>

<span data-ttu-id="076a5-149">**F:** mitt anpassade container tar en lång tid toostart och hello plattform omstart hello behållare innan den har slutförts startas.</span><span class="sxs-lookup"><span data-stu-id="076a5-149">**Q:** My custom container takes a long time toostart, and hello platform restart hello container before it finishes starting up.</span></span>

<span data-ttu-id="076a5-150">**S:** kan du konfigurera hello tid hello plattform ska vänta innan du startar om din behållare.</span><span class="sxs-lookup"><span data-stu-id="076a5-150">**A:** You can configure hello time hello platform will wait before restarting your container.</span></span> <span data-ttu-id="076a5-151">Detta kan göras genom att ange hello `WEBSITES_CONTAINER_START_TIME_LIMIT` app inställningen toohello det önskade värdet i sekunder.</span><span class="sxs-lookup"><span data-stu-id="076a5-151">This can be done by setting hello `WEBSITES_CONTAINER_START_TIME_LIMIT` app setting toohello desired value in seconds.</span></span> <span data-ttu-id="076a5-152">hello standardvärdet är 230 sekunder och Hej max är 600 sekunder.</span><span class="sxs-lookup"><span data-stu-id="076a5-152">hello default is 230 seconds, and hello max is 600 seconds.</span></span>

<span data-ttu-id="076a5-153">**F:** vad är hello format för privata registret serverns url?</span><span class="sxs-lookup"><span data-stu-id="076a5-153">**Q:** What is hello format for private registry server url?</span></span>

<span data-ttu-id="076a5-154">**S:** måste tooprovide hello fullständig registret url inklusive `http://` eller `https://`.</span><span class="sxs-lookup"><span data-stu-id="076a5-154">**A:** You need tooprovide hello full registry url including `http://` or `https://`.</span></span>

<span data-ttu-id="076a5-155">**F:** vad är hello format för hello avbildningsnamn i privata registret?</span><span class="sxs-lookup"><span data-stu-id="076a5-155">**Q:** What is hello format for hello image name in private registry option?</span></span>

<span data-ttu-id="076a5-156">**S:** måste tooadd hello fullständiga namn inklusive hello privata registret url (t.ex.</span><span class="sxs-lookup"><span data-stu-id="076a5-156">**A:** You need tooadd hello full image name including hello private registry url (eg.</span></span> <span data-ttu-id="076a5-157">myacr.azurecr.IO/DotNet:Latest)</span><span class="sxs-lookup"><span data-stu-id="076a5-157">myacr.azurecr.io/dotnet:latest)</span></span>

<span data-ttu-id="076a5-158">**F:** jag vill tooexpose mer än en port på min anpassade container avbildningen.</span><span class="sxs-lookup"><span data-stu-id="076a5-158">**Q:** I want tooexpose more than one port on my custom container image.</span></span> <span data-ttu-id="076a5-159">Är som möjligt?</span><span class="sxs-lookup"><span data-stu-id="076a5-159">Is that possible?</span></span>

<span data-ttu-id="076a5-160">**S:** för närvarande, som inte stöds.</span><span class="sxs-lookup"><span data-stu-id="076a5-160">**A:** Currently, that isn't supported.</span></span>

<span data-ttu-id="076a5-161">**F:** kan jag använda min egen storage?</span><span class="sxs-lookup"><span data-stu-id="076a5-161">**Q:** Can I bring my own storage?</span></span>

<span data-ttu-id="076a5-162">**S:** för närvarande som inte stöds.</span><span class="sxs-lookup"><span data-stu-id="076a5-162">**A:** Currently that isn't supported.</span></span>

<span data-ttu-id="076a5-163">**F:** det går inte att bläddra mitt anpassade container filen system eller som kör processer från hello SCM plats.</span><span class="sxs-lookup"><span data-stu-id="076a5-163">**Q:** I can't browse my custom container's file system or running processes from hello SCM site.</span></span> <span data-ttu-id="076a5-164">Varför är?</span><span class="sxs-lookup"><span data-stu-id="076a5-164">Why is that?</span></span>

<span data-ttu-id="076a5-165">**S:** hello SCM webbplatsen körs i en separat behållare.</span><span class="sxs-lookup"><span data-stu-id="076a5-165">**A:** hello SCM site runs in a separate container.</span></span> <span data-ttu-id="076a5-166">Du kan inte kontrollera hello filsystem eller kör processer på hello appbehållare.</span><span class="sxs-lookup"><span data-stu-id="076a5-166">You can't check hello file system or running processes of hello app container.</span></span>

<span data-ttu-id="076a5-167">**F:** mitt anpassade container lyssnar tooa port än port 80.</span><span class="sxs-lookup"><span data-stu-id="076a5-167">**Q:** My custom container listens tooa port other than port 80.</span></span> <span data-ttu-id="076a5-168">Hur kan jag konfigurera min app tooroute hello begäranden toothat port?</span><span class="sxs-lookup"><span data-stu-id="076a5-168">How can I configure my app tooroute hello requests toothat port?</span></span>

<span data-ttu-id="076a5-169">**S:** har vi automatisk identifiering av port, du kan också ange ett program ange kallas **WEBSITES_PORT**, och ge den hello värdet för hello förväntades portnummer.</span><span class="sxs-lookup"><span data-stu-id="076a5-169">**A:** We have auto port detection, also you can specify an application setting called **WEBSITES_PORT**, and give it hello value of hello expected port number.</span></span> <span data-ttu-id="076a5-170">Tidigare hello plattform med `PORT` app inställningen genom att vi planerar toodeprecate hello används den här appen inställningen och flytta toousing `WEBSITES_PORT` exklusivt.</span><span class="sxs-lookup"><span data-stu-id="076a5-170">Previously hello platform was using `PORT` app setting, we are planning toodeprecate hello use this app setting and move toousing `WEBSITES_PORT` exclusively.</span></span>

<span data-ttu-id="076a5-171">**F:** behöver jag tooimplement HTTPS i min anpassade container.</span><span class="sxs-lookup"><span data-stu-id="076a5-171">**Q:** Do I need tooimplement HTTPS in my custom container.</span></span>

<span data-ttu-id="076a5-172">**S:** Nej, hello plattformen hanterar HTTPS-avslutning vid hello delade frontends.</span><span class="sxs-lookup"><span data-stu-id="076a5-172">**A:** No, hello platform handles HTTPS termination at hello shared frontends.</span></span>

## <a name="pricing-and-sla"></a><span data-ttu-id="076a5-173">Priser och SLA</span><span class="sxs-lookup"><span data-stu-id="076a5-173">Pricing and SLA</span></span>

<span data-ttu-id="076a5-174">**F:** vad är hello priser när du använder hello förhandsversion?</span><span class="sxs-lookup"><span data-stu-id="076a5-174">**Q:** What's hello pricing while you're using hello public preview?</span></span>

<span data-ttu-id="076a5-175">**S:** debiteras du hälften hello antal timmar som appen körs med hello normal priser Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="076a5-175">**A:** You are charged half hello number of hours that your app runs, with hello normal Azure App Service pricing.</span></span> <span data-ttu-id="076a5-176">Det innebär att du får 50 procent rabatt på normala priser för Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="076a5-176">This means that you get a 50 percent discount on normal Azure App Service pricing.</span></span>

## <a name="other"></a><span data-ttu-id="076a5-177">Annat</span><span class="sxs-lookup"><span data-stu-id="076a5-177">Other</span></span>

<span data-ttu-id="076a5-178">**F:** vad är hello stöds tecken i programnamn inställningar?</span><span class="sxs-lookup"><span data-stu-id="076a5-178">**Q:** What are hello supported characters in application settings names?</span></span>

<span data-ttu-id="076a5-179">**S:** du kan bara använda A-Z, a-z, 0-9 och hello understreck för programinställningar.</span><span class="sxs-lookup"><span data-stu-id="076a5-179">**A:** You can only use A-Z, a-z, 0-9, and hello underscore for application settings.</span></span>

<span data-ttu-id="076a5-180">**F:** där du kan begära det nya funktioner?</span><span class="sxs-lookup"><span data-stu-id="076a5-180">**Q:** Where can I request new features?</span></span>

<span data-ttu-id="076a5-181">**S:** kan du skicka din idé på hello [Web Apps Feedbackforum](https://aka.ms/webapps-uservoice).</span><span class="sxs-lookup"><span data-stu-id="076a5-181">**A:** You can submit your idea at hello [Web Apps feedback forum](https://aka.ms/webapps-uservoice).</span></span> <span data-ttu-id="076a5-182">Lägg till ”[Linux]” toohello titeln på din idé.</span><span class="sxs-lookup"><span data-stu-id="076a5-182">Add "[Linux]" toohello title of your idea.</span></span>

## <a name="next-steps"></a><span data-ttu-id="076a5-183">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="076a5-183">Next steps</span></span>
* [<span data-ttu-id="076a5-184">Vad är Azure Web App på Linux?</span><span class="sxs-lookup"><span data-stu-id="076a5-184">What is Azure Web App on Linux?</span></span>](app-service-linux-intro.md)
* [<span data-ttu-id="076a5-185">SSH-stöd för Azure Web App på Linux</span><span class="sxs-lookup"><span data-stu-id="076a5-185">SSH support for Azure Web App on Linux</span></span>](./app-service-linux-ssh-support.md)
* [<span data-ttu-id="076a5-186">Skapa mellanlagringsmiljöer i Azure App Service</span><span class="sxs-lookup"><span data-stu-id="076a5-186">Set up staging environments in Azure App Service</span></span>](./web-sites-staged-publishing.md)
* [<span data-ttu-id="076a5-187">Kontinuerlig distribution med Azure-Webbapp på Linux</span><span class="sxs-lookup"><span data-stu-id="076a5-187">Continuous Deployment with Azure Web App on Linux</span></span>](./app-service-linux-ci-cd.md)
