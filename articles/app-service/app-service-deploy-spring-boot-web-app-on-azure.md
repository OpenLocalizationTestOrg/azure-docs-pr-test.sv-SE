---
title: "aaaDeploy källan startprogrammet-toohello Azure App Service | Microsoft Docs"
description: "Den här kursen hjälper utvecklare via hello steg toodeploy hello källan Start komma igång web app tooAzure Apptjänst."
services: app-service\web
documentationcenter: java
author: rmcmurray
manager: cfowler
editor: 
ms.assetid: 
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 08/04/2017
ms.author: asirveda;robmcm
ms.openlocfilehash: 69f9c4903fd740125194402cdb4b4db46a1f2773
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-spring-boot-application-toohello-azure-app-service"></a><span data-ttu-id="5f47b-103">Distribuera en källan startprogrammet toohello Azure App Service</span><span class="sxs-lookup"><span data-stu-id="5f47b-103">Deploy a Spring Boot Application toohello Azure App Service</span></span>

<span data-ttu-id="5f47b-104">Hej  **[Vårversionen Framework]**  är en öppen källkod som hjälper Java-utvecklare som skapar program på företagsnivå och en hello mer populära projekt som bygger på den plattformen är [Vårversionen Start], vilket ger en förenklad metod för att skapa fristående Java-program.</span><span class="sxs-lookup"><span data-stu-id="5f47b-104">hello **[Spring Framework]** is an open-source solution which helps Java developers create enterprise-level applications, and one of hello more-popular projects which is built on top of that platform is [Spring Boot], which provides a simplified approach for creating stand-alone Java applications.</span></span>

<span data-ttu-id="5f47b-105">Den här kursen får du om att skapa hello källan Start komma igång exempelwebbapp och distribuera den för[Azure App Service].</span><span class="sxs-lookup"><span data-stu-id="5f47b-105">This tutorial will walk you though creating hello sample Spring Boot Getting Started web app and deploying it too[Azure App Service].</span></span>

### <a name="prerequisites"></a><span data-ttu-id="5f47b-106">Krav</span><span class="sxs-lookup"><span data-stu-id="5f47b-106">Prerequisites</span></span>

<span data-ttu-id="5f47b-107">I ordning toocomplete hello steg i den här kursen behöver du toohave hello följande:</span><span class="sxs-lookup"><span data-stu-id="5f47b-107">In order toocomplete hello steps in this tutorial, you need toohave hello following:</span></span>

* <span data-ttu-id="5f47b-108">En Azure-prenumeration; Om du inte redan har en Azure-prenumeration, kan du aktivera din [MSDN-prenumerantförmåner] eller registrera dig för en [kostnadsfritt Azure-konto].</span><span class="sxs-lookup"><span data-stu-id="5f47b-108">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="5f47b-109">En uppdaterad [Java Developer Kit (JDK)].</span><span class="sxs-lookup"><span data-stu-id="5f47b-109">An up-to-date [Java Developer Kit (JDK)].</span></span>
* <span data-ttu-id="5f47b-110">Apache's [Maven] skapa verktyget (Version 3).</span><span class="sxs-lookup"><span data-stu-id="5f47b-110">Apache's [Maven] build tool (Version 3).</span></span>
* <span data-ttu-id="5f47b-111">En [Git] klienten.</span><span class="sxs-lookup"><span data-stu-id="5f47b-111">A [Git] client.</span></span>

## <a name="create-hello-spring-boot-getting-started-web-app"></a><span data-ttu-id="5f47b-112">Skapa hello källan Start komma igång-webbprogram</span><span class="sxs-lookup"><span data-stu-id="5f47b-112">Create hello Spring Boot Getting Started web app</span></span>

<span data-ttu-id="5f47b-113">hello vägleder följande steg dig igenom hello steg som är nödvändiga toocreate ett enkelt källan Start-webbprogram och testa den lokalt.</span><span class="sxs-lookup"><span data-stu-id="5f47b-113">hello following steps will walk you through hello steps that are required toocreate a simple Spring Boot web application and test it locally.</span></span>

1. <span data-ttu-id="5f47b-114">Öppna en kommandotolk och skapa en lokal katalog toohold programmet och ändra toothat katalog. Exempel:</span><span class="sxs-lookup"><span data-stu-id="5f47b-114">Open a command-prompt and create a local directory toohold your application, and change toothat directory; for example:</span></span>
   ```
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   <span data-ttu-id="5f47b-115">-- eller--</span><span class="sxs-lookup"><span data-stu-id="5f47b-115">-- or --</span></span>
   ```
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. <span data-ttu-id="5f47b-116">Klona hello [Vårversionen Start komma igång] exempelprojektet i hello-katalog som du just skapat, till exempel:</span><span class="sxs-lookup"><span data-stu-id="5f47b-116">Clone hello [Spring Boot Getting Started] sample project into hello directory you just created; for example:</span></span>
   ```
   git clone https://github.com/spring-guides/gs-spring-boot.git
   ```

1. <span data-ttu-id="5f47b-117">Ändra katalogen toohello slutförts projektet. Exempel:</span><span class="sxs-lookup"><span data-stu-id="5f47b-117">Change directory toohello completed project; for example:</span></span>
   ```
   cd gs-spring-boot
   cd complete
   ```

1. <span data-ttu-id="5f47b-118">Skapa hello JAR-filen med Maven; Exempel:</span><span class="sxs-lookup"><span data-stu-id="5f47b-118">Build hello JAR file using Maven; for example:</span></span>
   ```
   mvn package
   ```

1. <span data-ttu-id="5f47b-119">När hello webbprogrammet har skapats, ändra directory toohello JAR-filen och starta hello webbprogrammet; Exempel:</span><span class="sxs-lookup"><span data-stu-id="5f47b-119">Once hello web app has been created, change directory toohello JAR file and start hello web app; for example:</span></span>
   ```
   cd target
   java -jar gs-spring-boot-0.1.0.jar
   ```

1. <span data-ttu-id="5f47b-120">Testa hello webbprogram genom att bläddra toohttp://localhost:8080 i en webbläsare eller använder hello syntax som hello följande exempel om du har curl som är tillgängliga:</span><span class="sxs-lookup"><span data-stu-id="5f47b-120">Test hello web app by browsing toohttp://localhost:8080 using a web browser, or use hello syntax like hello following example if you have curl available:</span></span>
   ```
   curl http://localhost:8080
   ```

1. <span data-ttu-id="5f47b-121">Du bör se hello följande meddelande visas: **helg från källan Start!**</span><span class="sxs-lookup"><span data-stu-id="5f47b-121">You should see hello following message displayed: **Greetings from Spring Boot!**</span></span>

   ![Bläddra Sample-appen][SB01]

## <a name="create-an-azure-web-app-for-use-with-java"></a><span data-ttu-id="5f47b-123">Skapa en Azure-webbapp för användning med Java</span><span class="sxs-lookup"><span data-stu-id="5f47b-123">Create an Azure web app for use with Java</span></span>

<span data-ttu-id="5f47b-124">hello följande steg vägleder dig genom hello steg toocreate en Azure-Webbapp, konfigurera inställningar för hello som krävs för Java och konfigurera FTP-autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="5f47b-124">hello following steps will walk you through hello steps toocreate an Azure Web App, configure hello required settings for Java, and configure your FTP credentials.</span></span>

1. <span data-ttu-id="5f47b-125">Bläddra toohello [Azure-portalen] och logga in.</span><span class="sxs-lookup"><span data-stu-id="5f47b-125">Browse toohello [Azure portal] and log in.</span></span>

1. <span data-ttu-id="5f47b-126">När du har loggat in på ditt konto på hello Azure-portalen klickar du på hello menyn ikonen för **Apptjänster**:</span><span class="sxs-lookup"><span data-stu-id="5f47b-126">Once you have logged into your account on hello Azure portal, click hello menu icon for **App Services**:</span></span>
   
   ![Azure Portal][AZ01]

1. <span data-ttu-id="5f47b-128">När hello **Apptjänster** visas klickar du på **+ Lägg till** toocreate en ny App-tjänst.</span><span class="sxs-lookup"><span data-stu-id="5f47b-128">When hello **App Services** page is displayed, click **+ Add** toocreate a new App Service.</span></span>

   ![Skapa Apptjänst][AZ02]

1. <span data-ttu-id="5f47b-130">När hello lista över mallar för app visas, klickar du på hello länk för hello grundläggande Microsoft Web App.</span><span class="sxs-lookup"><span data-stu-id="5f47b-130">When hello list of web app templates is displayed, click hello link for hello basic Microsoft Web App.</span></span>

   ![Mallar för App][AZ03]

1. <span data-ttu-id="5f47b-132">När hello information för hello webbprogrammet mallen visas, klickar du på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="5f47b-132">When hello information page for hello Web App template is displayed, click **Create**.</span></span>

   ![Skapa webbapp][AZ04]

1. <span data-ttu-id="5f47b-134">Ange ett unikt namn för din webbapp och ange eventuella ytterligare inställningar och sedan **skapa**.</span><span class="sxs-lookup"><span data-stu-id="5f47b-134">Provide a unique name for your web app and specify any additional settings, and then **Create**.</span></span>

   ![Skapa Webbprograminställningar][AZ05]

1. <span data-ttu-id="5f47b-136">När webbappen har skapats, klickar du på hello menyn ikonen för **Apptjänster**, och klicka sedan på ditt nyligen skapade webbprogram:</span><span class="sxs-lookup"><span data-stu-id="5f47b-136">Once your web app has been created, click hello menu icon for **App Services**, and then click your newly-created web app:</span></span>

   ![Lista över Webbappar][AZ06]

1. <span data-ttu-id="5f47b-138">När webbappen visas, ange hello Java version med hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="5f47b-138">When your web app is displayed, specify hello Java version by using hello following steps:</span></span>

   <span data-ttu-id="5f47b-139">a.</span><span class="sxs-lookup"><span data-stu-id="5f47b-139">a.</span></span> <span data-ttu-id="5f47b-140">Klicka på hello **programinställningar** menyalternativet.</span><span class="sxs-lookup"><span data-stu-id="5f47b-140">Click hello **Application Settings** menu item.</span></span>

   <span data-ttu-id="5f47b-141">b.</span><span class="sxs-lookup"><span data-stu-id="5f47b-141">b.</span></span> <span data-ttu-id="5f47b-142">Välj **Java 8** för hello Java-version.</span><span class="sxs-lookup"><span data-stu-id="5f47b-142">Choose **Java 8** for hello Java version.</span></span>

   <span data-ttu-id="5f47b-143">c.</span><span class="sxs-lookup"><span data-stu-id="5f47b-143">c.</span></span> <span data-ttu-id="5f47b-144">Välj **nyaste** för Java hello delversion.</span><span class="sxs-lookup"><span data-stu-id="5f47b-144">Choose **Newest** for hello minor Java version.</span></span>

   <span data-ttu-id="5f47b-145">d.</span><span class="sxs-lookup"><span data-stu-id="5f47b-145">d.</span></span> <span data-ttu-id="5f47b-146">Välj **senaste Tomcat 8.5** för hello webbehållare.</span><span class="sxs-lookup"><span data-stu-id="5f47b-146">Choose **Newest Tomcat 8.5** for hello web container.</span></span> <span data-ttu-id="5f47b-147">(Den här behållaren faktiskt används inte; Azure använder hello behållare från källan Start programmet.)</span><span class="sxs-lookup"><span data-stu-id="5f47b-147">(This container will not actually be used; Azure will use hello container from your Spring Boot application.)</span></span>

   <span data-ttu-id="5f47b-148">e.</span><span class="sxs-lookup"><span data-stu-id="5f47b-148">e.</span></span> <span data-ttu-id="5f47b-149">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="5f47b-149">Click **Save**.</span></span>

   ![Programinställningar][AZ07]

1. <span data-ttu-id="5f47b-151">Ange dina autentiseringsuppgifter för FTP-distribution med hjälp av hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="5f47b-151">Specify your FTP deployment credentials by using hello following steps:</span></span>

   <span data-ttu-id="5f47b-152">a.</span><span class="sxs-lookup"><span data-stu-id="5f47b-152">a.</span></span> <span data-ttu-id="5f47b-153">Klicka på hello **Distributionsbehörigheterna** menyalternativet.</span><span class="sxs-lookup"><span data-stu-id="5f47b-153">Click hello **Deployment Credentials** menu item.</span></span>

   <span data-ttu-id="5f47b-154">b.</span><span class="sxs-lookup"><span data-stu-id="5f47b-154">b.</span></span> <span data-ttu-id="5f47b-155">Ange ditt användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="5f47b-155">Specify your username and password.</span></span>

   <span data-ttu-id="5f47b-156">c.</span><span class="sxs-lookup"><span data-stu-id="5f47b-156">c.</span></span> <span data-ttu-id="5f47b-157">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="5f47b-157">Click **Save**.</span></span>

   ![Ange autentiseringsuppgifter för distribution][AZ08]

1. <span data-ttu-id="5f47b-159">Hämta din FTP-anslutningsinformation med hjälp av hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="5f47b-159">Retrieve your FTP connection information by using hello following steps:</span></span>

   <span data-ttu-id="5f47b-160">a.</span><span class="sxs-lookup"><span data-stu-id="5f47b-160">a.</span></span> <span data-ttu-id="5f47b-161">Klicka på hello **Distributionsbehörigheterna** menyalternativet.</span><span class="sxs-lookup"><span data-stu-id="5f47b-161">Click hello **Deployment Credentials** menu item.</span></span>

   <span data-ttu-id="5f47b-162">b.</span><span class="sxs-lookup"><span data-stu-id="5f47b-162">b.</span></span> <span data-ttu-id="5f47b-163">Kopiera hela FTP-användarnamn och URL: en och spara dem för hello nästa avsnitt i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="5f47b-163">Copy your full FTP username and URL and save them for hello next section of this tutorial.</span></span>

   ![FTP-URL och autentiseringsuppgifter][AZ09]

## <a name="deploy-your-spring-boot-web-app-tooazure"></a><span data-ttu-id="5f47b-165">Distribuera din källan Start web app tooAzure</span><span class="sxs-lookup"><span data-stu-id="5f47b-165">Deploy your Spring Boot web app tooAzure</span></span>

<span data-ttu-id="5f47b-166">hello följande tar dig igenom hello steg toodeploy tooAzure din källan Start web app.</span><span class="sxs-lookup"><span data-stu-id="5f47b-166">hello following steps will walk you through hello steps toodeploy your Spring Boot web app tooAzure.</span></span>

1. <span data-ttu-id="5f47b-167">Öppna en textredigerare, till exempel Anteckningar och klistra in följande text i ett nytt dokument hello och spara hello som *web.config*:</span><span class="sxs-lookup"><span data-stu-id="5f47b-167">Open a text editor such as Windows Notepad and paste hello following text into a new document, then save hello file as *web.config*:</span></span>
   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <configuration>
     <system.webServer>
       <handlers>
         <add name="httpPlatformHandler" path="*" verb="*" modules="httpPlatformHandler" resourceType="Unspecified" />
       </handlers>
       <httpPlatform processPath="%JAVA_HOME%\bin\java.exe"
           arguments="-Djava.net.preferIPv4Stack=true -Dserver.port=%HTTP_PLATFORM_PORT% -jar &quot;%HOME%\site\wwwroot\gs-spring-boot-0.1.0.jar&quot;">
       </httpPlatform>
     </system.webServer>
   </configuration>
   ```

1. <span data-ttu-id="5f47b-168">När du har sparat hello *web.config* tooyour filsystem, ansluta tooyour webbprogrammet via FTP-med hello URL, användarnamn och lösenord från hello föregående avsnitt i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="5f47b-168">After you have saved hello *web.config* file tooyour system, connect tooyour web app via FTP using hello URL, username, and password from hello preceding section of this tutorial.</span></span> <span data-ttu-id="5f47b-169">Exempel:</span><span class="sxs-lookup"><span data-stu-id="5f47b-169">For example:</span></span>
   ```
   ftp
   open waws-prod-sn0-000.ftp.azurewebsites.windows.net
   user wingtiptoys-springboot\wingtiptoysuser
   pass ********
   ```

1. <span data-ttu-id="5f47b-170">Ändra hello fjärrkatalog toohello rotmapp ditt webbprogram (som är */platsen/wwwroot*), kopiera hello JAR-filen från källan Start programmet och hello *web.config* från tidigare.</span><span class="sxs-lookup"><span data-stu-id="5f47b-170">Change hello remote directory toohello root folder of your web app, (which is at */site/wwwroot*), then copy hello JAR file from your Spring Boot application and hello *web.config* from earlier.</span></span> <span data-ttu-id="5f47b-171">Exempel:</span><span class="sxs-lookup"><span data-stu-id="5f47b-171">For example:</span></span>
   ```
   cd site/wwwroot
   put gs-spring-boot-0.1.0.jar
   put web.config
   ```

1. <span data-ttu-id="5f47b-172">När du har distribuerat din JAR och *web.config* filer tooyour webbapp behöver du toorestart ditt webbprogram med hjälp av hello Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="5f47b-172">After you have deployed your JAR and *web.config* files tooyour web app, you need toorestart your web app using hello Azure portal:</span></span>

   ![][AZ10]

1. <span data-ttu-id="5f47b-173">Testa hello webbprogram genom att bläddra tooyour webbappens Webbadress i en webbläsare eller använder hello syntax som hello följande exempel om du har curl som är tillgängliga:</span><span class="sxs-lookup"><span data-stu-id="5f47b-173">Test hello web app by browsing tooyour web app's URL using a web browser, or use hello syntax like hello following example if you have curl available:</span></span>
   ```
   curl http://wingtiptoys-springboot.azurewebsites.net/
   ```

1. <span data-ttu-id="5f47b-174">Du bör se hello följande meddelande visas: **helg från källan Start!**</span><span class="sxs-lookup"><span data-stu-id="5f47b-174">You should see hello following message displayed: **Greetings from Spring Boot!**</span></span>

   ![Bläddra Sample-appen][SB02]

## <a name="next-steps"></a><span data-ttu-id="5f47b-176">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5f47b-176">Next steps</span></span>

<span data-ttu-id="5f47b-177">Mer information om hur du använder värdet startprogram i Azure finns i hello följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="5f47b-177">For more information about using Spring Boot applications on Azure, see hello following articles:</span></span>

* [<span data-ttu-id="5f47b-178">Distribuera en källan startprogrammet på Linux i hello Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="5f47b-178">Deploy a Spring Boot Application on Linux in hello Azure Container Service</span></span>](../container-service/kubernetes/container-service-deploy-spring-boot-app-on-linux.md)

* [<span data-ttu-id="5f47b-179">Distribuera ett program för start av värdet på ett kluster med Kubernetes på hello Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="5f47b-179">Deploy a Spring Boot Application on a Kubernetes Cluster in hello Azure Container Service</span></span>](../container-service/kubernetes/container-service-deploy-spring-boot-app-on-kubernetes.md)

<span data-ttu-id="5f47b-180">Mer information om hur du använder Azure med Java finns hello [Azure Java Developer Center] och hello [Java-verktyg för Visual Studio Team Services].</span><span class="sxs-lookup"><span data-stu-id="5f47b-180">For more information about using Azure with Java, see hello [Azure Java Developer Center] and hello [Java Tools for Visual Studio Team Services].</span></span>

<span data-ttu-id="5f47b-181">Mer information om depoying web apps tooAzure med FTP finns [distribuera din app tooAzure App Service med FTP/S].</span><span class="sxs-lookup"><span data-stu-id="5f47b-181">For additional information about depoying web apps tooAzure using FTP, see [Deploy your app tooAzure App Service using FTP/S].</span></span>

<span data-ttu-id="5f47b-182">Mer information om hello källan Start exempelprojektet finns [Vårversionen Start komma igång].</span><span class="sxs-lookup"><span data-stu-id="5f47b-182">For further details about hello Spring Boot sample project, see [Spring Boot Getting Started].</span></span>

<span data-ttu-id="5f47b-183">Hjälp med att komma igång med din egen källan startprogram, finns hello **Vårversionen Initializr** på https://start.spring.io/.</span><span class="sxs-lookup"><span data-stu-id="5f47b-183">For help with getting started with your own Spring Boot applications, see hello **Spring Initializr** at https://start.spring.io/.</span></span>

<span data-ttu-id="5f47b-184">Mer information om hur du konfigurerar ytterligare inställningar för ditt webbprogram finns [konfigurera webbappar i Azure App Service].</span><span class="sxs-lookup"><span data-stu-id="5f47b-184">For more information about configuring additional settings for your web app, see [Configure web apps in Azure App Service].</span></span>

<!-- URL List -->

[Azure App Service]: https://azure.microsoft.com/services/app-service/
[Azure Container Service]: https://azure.microsoft.com/services/container-service/
[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[Azure-portalen]: https://portal.azure.com/
[konfigurera webbappar i Azure App Service]: /azure/app-service-web/web-sites-configure
[distribuera din app tooAzure App Service med FTP/S]: https://docs.microsoft.com/azure/app-service-web/app-service-deploy-ftp
[kostnadsfritt Azure-konto]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Java-verktyg för Visual Studio Team Services]: https://java.visualstudio.com/
[Maven]: http://maven.apache.org/
[MSDN-prenumerantförmåner]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Vårversionen Start]: http://projects.spring.io/spring-boot/
[Vårversionen Start komma igång]: https://github.com/spring-guides/gs-spring-boot
[Vårversionen Framework]: https://spring.io/

<!-- IMG List -->

[SB01]: ./media/app-service-deploy-spring-boot-web-app-on-azure/SB01.png
[SB02]: ./media/app-service-deploy-spring-boot-web-app-on-azure/SB02.png

[AZ01]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ01.png
[AZ02]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ02.png
[AZ03]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ03.png
[AZ04]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ04.png
[AZ05]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ05.png
[AZ06]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ06.png
[AZ07]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ07.png
[AZ08]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ08.png
[AZ09]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ09.png
[AZ10]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ10.png
