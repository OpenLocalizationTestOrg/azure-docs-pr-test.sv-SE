---
title: "aaaDeploy Vårversionen Start Web App på Linux i Azure Container Service | Microsoft Docs"
description: "Den här självstudiekursen vägleder dig även om hello steg toodeploy en källan startprogrammet som en Linux-webbapp i Microsoft Azure."
services: container-service
documentationcenter: java
author: rmcmurray
manager: cfowler
editor: 
ms.assetid: 
ms.service: container-service
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 08/04/2017
ms.author: asirveda;robmcm
ms.custom: mvc
ms.openlocfilehash: 2c44be1c7f66a38f48239001f0be9e90c7e6edef
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-spring-boot-application-on-linux-in-hello-azure-container-service"></a><span data-ttu-id="43071-103">Distribuera ett program med vår Start på Linux i hello Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="43071-103">Deploy a Spring Boot application on Linux in hello Azure Container Service</span></span>

<span data-ttu-id="43071-104">Hej  **[Vårversionen Framework]**  är en öppen källkod som hjälper till att Java-utvecklare som skapar program på företagsnivå.</span><span class="sxs-lookup"><span data-stu-id="43071-104">hello **[Spring Framework]** is an open-source solution that helps Java developers create enterprise-level applications.</span></span> <span data-ttu-id="43071-105">En mer populära hello-projekt som är inbyggd som plattform är [Vårversionen Start], vilket ger en förenklad metod för att skapa fristående Java-program.</span><span class="sxs-lookup"><span data-stu-id="43071-105">One of hello more-popular projects that is built on top of that platform is [Spring Boot], which provides a simplified approach for creating stand-alone Java applications.</span></span>

<span data-ttu-id="43071-106">**[Docker]**  är öppen källkod som hjälper utvecklare att automatisera hello distribution, skalning och hanteringen av sina program som körs i behållare.</span><span class="sxs-lookup"><span data-stu-id="43071-106">**[Docker]** is open-source solutions that helps developers automate hello deployment, scaling, and management of their applications that are running in containers.</span></span>

<span data-ttu-id="43071-107">Den här självstudiekursen vägleder dig genom med Docker toodevelop och distribuera en källan Start programmet tooa Linux-värd i hello [Azure Container Service (ACS)].</span><span class="sxs-lookup"><span data-stu-id="43071-107">This tutorial walks you through using Docker toodevelop and deploy a Spring Boot application tooa Linux host in hello [Azure Container Service (ACS)].</span></span>

## <a name="prerequisites"></a><span data-ttu-id="43071-108">Krav</span><span class="sxs-lookup"><span data-stu-id="43071-108">Prerequisites</span></span>

<span data-ttu-id="43071-109">I ordning toocomplete hello steg i den här kursen behöver du toohave hello följande krav:</span><span class="sxs-lookup"><span data-stu-id="43071-109">In order toocomplete hello steps in this tutorial, you need toohave hello following prerequisites:</span></span>

* <span data-ttu-id="43071-110">En Azure-prenumeration; Om du inte redan har en Azure-prenumeration, kan du aktivera din [MSDN-prenumerantförmåner] eller registrera dig för en [kostnadsfritt Azure-konto].</span><span class="sxs-lookup"><span data-stu-id="43071-110">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="43071-111">Hej [Azure-kommandoradsgränssnittet (CLI)].</span><span class="sxs-lookup"><span data-stu-id="43071-111">hello [Azure Command-Line Interface (CLI)].</span></span>
* <span data-ttu-id="43071-112">En uppdaterad [Java Developer Kit (JDK)].</span><span class="sxs-lookup"><span data-stu-id="43071-112">An up-to-date [Java Developer Kit (JDK)].</span></span>
* <span data-ttu-id="43071-113">Apache's [Maven] skapa verktyget (Version 3).</span><span class="sxs-lookup"><span data-stu-id="43071-113">Apache's [Maven] build tool (Version 3).</span></span>
* <span data-ttu-id="43071-114">En [Git] klienten.</span><span class="sxs-lookup"><span data-stu-id="43071-114">A [Git] client.</span></span>
* <span data-ttu-id="43071-115">En [Docker] klienten.</span><span class="sxs-lookup"><span data-stu-id="43071-115">A [Docker] client.</span></span>

> [!NOTE]
>
> <span data-ttu-id="43071-116">På grund av toohello virtualisering kraven i den här kursen får inte kan du använda hello stegen i den här artikeln på en virtuell dator; Du måste använda en fysisk dator med aktiverat funktionerna för virtualisering.</span><span class="sxs-lookup"><span data-stu-id="43071-116">Due toohello virtualization requirements of this tutorial, you cannot follow hello steps in this article on a virtual machine; you must use a physical computer with virtualization features enabled.</span></span>
>

## <a name="create-hello-spring-boot-on-docker-getting-started-web-app"></a><span data-ttu-id="43071-117">Skapa hello källan Start på Docker komma igång webbprogram</span><span class="sxs-lookup"><span data-stu-id="43071-117">Create hello Spring Boot on Docker Getting Started web app</span></span>

<span data-ttu-id="43071-118">hello följande steg vägleder dig igenom hello steg som är nödvändiga toocreate ett enkelt källan Start-webbprogram och testa den lokalt.</span><span class="sxs-lookup"><span data-stu-id="43071-118">hello following steps walk you through hello steps that are required toocreate a simple Spring Boot web application and test it locally.</span></span>

1. <span data-ttu-id="43071-119">Öppna en kommandotolk och skapa en lokal katalog toohold programmet och ändra toothat katalog. Exempel:</span><span class="sxs-lookup"><span data-stu-id="43071-119">Open a command-prompt and create a local directory toohold your application, and change toothat directory; for example:</span></span>
   ```
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   <span data-ttu-id="43071-120">-- eller--</span><span class="sxs-lookup"><span data-stu-id="43071-120">-- or --</span></span>
   ```
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. <span data-ttu-id="43071-121">Klona hello [Vårversionen Start på Docker komma igång] exempelprojektet i hello-katalog som du har skapat, till exempel:</span><span class="sxs-lookup"><span data-stu-id="43071-121">Clone hello [Spring Boot on Docker Getting Started] sample project into hello directory you created; for example:</span></span>
   ```
   git clone https://github.com/spring-guides/gs-spring-boot-docker.git
   ```

1. <span data-ttu-id="43071-122">Ändra katalogen toohello slutförts projektet. Exempel:</span><span class="sxs-lookup"><span data-stu-id="43071-122">Change directory toohello completed project; for example:</span></span>
   ```
   cd gs-spring-boot-docker/complete
   ```

1. <span data-ttu-id="43071-123">Skapa hello JAR-filen med Maven; Exempel:</span><span class="sxs-lookup"><span data-stu-id="43071-123">Build hello JAR file using Maven; for example:</span></span>
   ```
   mvn package
   ```

1. <span data-ttu-id="43071-124">När hello webbprogrammet har skapats kan du ändra directory toohello `target` katalogen där hello JAR-filen finns och starta hello webbprogram, till exempel:</span><span class="sxs-lookup"><span data-stu-id="43071-124">Once hello web app has been created, change directory toohello `target` directory where hello JAR file is located and start hello web app; for example:</span></span>
   ```
   cd target
   java -jar gs-spring-boot-docker-0.1.0.jar
   ```

1. <span data-ttu-id="43071-125">Testa hello webbprogram genom att bläddra tooit lokalt med hjälp av en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="43071-125">Test hello web app by browsing tooit locally using a web browser.</span></span> <span data-ttu-id="43071-126">Till exempel om du har konfigurerade curl som är tillgängliga och du hello Tomcat server toorun på port 80:</span><span class="sxs-lookup"><span data-stu-id="43071-126">For example, if you have curl available and you configured hello Tomcat server toorun on port 80:</span></span>
   ```
   curl http://localhost
   ```

1. <span data-ttu-id="43071-127">Du bör se hello följande meddelande visas: **Docker hälsningsmeddelande!**</span><span class="sxs-lookup"><span data-stu-id="43071-127">You should see hello following message displayed: **Hello Docker World!**</span></span>

   ![Bläddra Sample-appen lokalt][SB01]

## <a name="create-an-azure-container-registry-toouse-as-a-private-docker-registry"></a><span data-ttu-id="43071-129">Skapa ett Azure Container registret toouse som ett privat Docker-register</span><span class="sxs-lookup"><span data-stu-id="43071-129">Create an Azure Container Registry toouse as a Private Docker Registry</span></span>

<span data-ttu-id="43071-130">hello följande steg beskriver hur du använder hello Azure portal toocreate registret en Azure-behållare.</span><span class="sxs-lookup"><span data-stu-id="43071-130">hello following steps walk you through using hello Azure portal toocreate an Azure Container Registry.</span></span>

> [!NOTE]
>
> <span data-ttu-id="43071-131">Om du vill toouse hello Azure CLI i stället för hello Azure-portalen gör hello i [skapa en privat Docker behållare registret med hjälp av hello Azure CLI 2.0](../../container-registry/container-registry-get-started-azure-cli.md).</span><span class="sxs-lookup"><span data-stu-id="43071-131">If you want toouse hello Azure CLI instead of hello Azure portal, follow hello steps in [Create a private Docker container registry using hello Azure CLI 2.0](../../container-registry/container-registry-get-started-azure-cli.md).</span></span>
>

1. <span data-ttu-id="43071-132">Bläddra toohello [Azure-portalen] och logga in.</span><span class="sxs-lookup"><span data-stu-id="43071-132">Browse toohello [Azure portal] and sign in.</span></span>

   <span data-ttu-id="43071-133">När du har loggat in tooyour konto på hello Azure-portalen kan du följa hello stegen i hello [skapa en privat Docker behållare registret med hjälp av hello Azure-portalen] artikel som omskrivning i följande steg för hello hello saké är lämpligt.</span><span class="sxs-lookup"><span data-stu-id="43071-133">Once you have signed in tooyour account on hello Azure portal, you can follow hello steps in hello [Create a private Docker container registry using hello Azure portal] article, which are paraphrased in hello following steps for hello sake of expediency.</span></span>

1. <span data-ttu-id="43071-134">Hello menyn ikonen för **+ ny**, klicka på **behållare**, och klicka sedan på **Azure Container registret**.</span><span class="sxs-lookup"><span data-stu-id="43071-134">Click hello menu icon for **+ New**, then click **Containers**, and then click **Azure Container Registry**.</span></span>
   
   ![Skapa en ny Azure-behållare registernyckel][AR01]

1. <span data-ttu-id="43071-136">När hello information för hello Azure Container registret mallen visas, klickar du på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="43071-136">When hello information page for hello Azure Container Registry template is displayed, click **Create**.</span></span> 

   ![Skapa en ny Azure-behållare registernyckel][AR02]

1. <span data-ttu-id="43071-138">När hello **skapa behållaren registret** visas, ange din **registrets** och **resursgruppen**, Välj **aktivera** för Hej **administratörsanvändare**, och klicka sedan på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="43071-138">When hello **Create container registry** page is displayed, enter your **Registry name** and **Resource group**, choose **Enable** for hello **Admin user**, and then click **Create**.</span></span>

   ![Konfigurera registerinställningar för Azure-behållare][AR03]

1. <span data-ttu-id="43071-140">När registret behållaren har skapats, navigera tooyour behållare registret i hello Azure-portalen och klicka sedan på **åtkomstnycklar**.</span><span class="sxs-lookup"><span data-stu-id="43071-140">Once your container registry has been created, navigate tooyour container registry in hello Azure portal, and then click **Access Keys**.</span></span> <span data-ttu-id="43071-141">Anteckna hello användarnamn och lösenord för hello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="43071-141">Take note of hello username and password for hello next steps.</span></span>

   ![Azure Container åtkomst registernycklar][AR04]

## <a name="configure-maven-toouse-your-azure-container-registry-access-keys"></a><span data-ttu-id="43071-143">Konfigurera Maven toouse din Azure-behållare åtkomst registernycklar</span><span class="sxs-lookup"><span data-stu-id="43071-143">Configure Maven toouse your Azure Container Registry access keys</span></span>

1. <span data-ttu-id="43071-144">Navigera toohello konfigurationskatalogen för Maven-installation och öppna hello *settings.xml* fil med en textredigerare.</span><span class="sxs-lookup"><span data-stu-id="43071-144">Navigate toohello configuration directory for your Maven installation and open hello *settings.xml* file with a text editor.</span></span>

1. <span data-ttu-id="43071-145">Lägga till din Azure-behållare åtkomst registerinställningar från hello föregående avsnitt i den här självstudiekursen toohello `<servers>` samling i hello *settings.xml* filen, till exempel:</span><span class="sxs-lookup"><span data-stu-id="43071-145">Add your Azure Container Registry access settings from hello previous section of this tutorial toohello `<servers>` collection in hello *settings.xml* file; for example:</span></span>

   ```xml
   <servers>
      <server>
         <id>wingtiptoysregistry</id>
         <username>wingtiptoysregistry</username>
         <password>AbCdEfGhIjKlMnOpQrStUvWxYz</password>
      </server>
   </servers>
   ```

1. <span data-ttu-id="43071-146">Navigera toohello slutförts projektkatalogen för tillämpningsprogrammet källan start (till exempel ”:*C:\SpringBoot\gs-spring-boot-docker\complete*” eller ”*/users/robert/SpringBoot/gs-spring-boot-docker / fullständig*”), och öppna hello *pom.xml* fil med en textredigerare.</span><span class="sxs-lookup"><span data-stu-id="43071-146">Navigate toohello completed project directory for your Spring Boot application, (for example: "*C:\SpringBoot\gs-spring-boot-docker\complete*" or "*/users/robert/SpringBoot/gs-spring-boot-docker/complete*"), and open hello *pom.xml* file with a text editor.</span></span>

1. <span data-ttu-id="43071-147">Uppdatera hello `<properties>` samling i hello *pom.xml* fil med hello inloggningen server värde för Azure-behållare registret från hello föregående avsnitt i den här självstudiekursen, till exempel:</span><span class="sxs-lookup"><span data-stu-id="43071-147">Update hello `<properties>` collection in hello *pom.xml* file with hello login server value for your Azure Container Registry from hello previous section of this tutorial; for example:</span></span>

   ```xml
   <properties>
      <docker.image.prefix>wingtiptoysregistry.azurecr.io</docker.image.prefix>
      <java.version>1.8</java.version>
   </properties>
   ```

1. <span data-ttu-id="43071-148">Uppdatera hello `<plugins>` samling i hello *pom.xml* filen så som hello `<plugin>` innehåller hello server adress och registret inloggningsnamn för ditt Azure-behållare registernyckeln från hello föregående avsnitt i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="43071-148">Update hello `<plugins>` collection in hello *pom.xml* file so that hello `<plugin>` contains hello login server address and registry name for your Azure Container Registry from hello previous section of this tutorial.</span></span> <span data-ttu-id="43071-149">Exempel:</span><span class="sxs-lookup"><span data-stu-id="43071-149">For example:</span></span>

   ```xml
   <plugin>
      <groupId>com.spotify</groupId>
      <artifactId>docker-maven-plugin</artifactId>
      <version>0.4.11</version>
      <configuration>
         <imageName>${docker.image.prefix}/${project.artifactId}</imageName>
         <dockerDirectory>src/main/docker</dockerDirectory>
         <resources>
            <resource>
               <targetPath>/</targetPath>
               <directory>${project.build.directory}</directory>
               <include>${project.build.finalName}.jar</include>
            </resource>
         </resources>
         <serverId>wingtiptoysregistry</serverId>
         <registryUrl>https://wingtiptoysregistry.azurecr.io</registryUrl>
      </configuration>
   </plugin>
   ```

1. <span data-ttu-id="43071-150">Navigera toohello slutförts projektkatalogen för tillämpningsprogrammet källan start och kör följande kommando toorebuild hello programmet hello och push hello behållaren tooyour registret för Azure-behållare:</span><span class="sxs-lookup"><span data-stu-id="43071-150">Navigate toohello completed project directory for your Spring Boot application and run hello following command toorebuild hello application and push hello container tooyour Azure Container Registry:</span></span>

   ```
   mvn package docker:build -DpushImage 
   ```

> [!NOTE]
>
> <span data-ttu-id="43071-151">När du trycker tooAzure din Docker-behållaren får ett felmeddelande liknande tooone av hello följande även om din dockerbehållare har skapats:</span><span class="sxs-lookup"><span data-stu-id="43071-151">When you are pushing your Docker container tooAzure, you may receive an error message that is similar tooone of hello following even though your Docker container was created successfully:</span></span>
>
> * `[ERROR] Failed tooexecute goal com.spotify:docker-maven-plugin:0.4.11:build (default-cli) on project gs-spring-boot-docker: Exception caught: no basic auth credentials`
>
> * `[ERROR] Failed tooexecute goal com.spotify:docker-maven-plugin:0.4.11:build (default-cli) on project gs-spring-boot-docker: Exception caught: Incomplete Docker registry authorization credentials. Please provide all of username, password, and email or none.`
>
> <span data-ttu-id="43071-152">Om det händer kan behöva du toosign i tooyour Azure-konto från hello Docker-kommandoraden. Exempel:</span><span class="sxs-lookup"><span data-stu-id="43071-152">If this happens, you may need toosign in tooyour Azure account from hello Docker command line; for example:</span></span>
>
> `docker login -u wingtiptoysregistry -p "AbCdEfGhIjKlMnOpQrStUvWxYz" wingtiptoysregistry.azurecr.io`
>
> <span data-ttu-id="43071-153">Du kan sedan överför din behållare från hello-kommandoraden. Exempel:</span><span class="sxs-lookup"><span data-stu-id="43071-153">You can then push your container from hello command line; for example:</span></span>
>
> `docker push wingtiptoysregistry.azurecr.io/gs-spring-boot-docker`
>

## <a name="create-a-web-app-on-linux-on-azure-app-service-using-your-container-image"></a><span data-ttu-id="43071-154">Skapa en webbapp i Linux på Azure App Service med behållaren avbildningen</span><span class="sxs-lookup"><span data-stu-id="43071-154">Create a web app on Linux on Azure App Service using your container image</span></span>

1. <span data-ttu-id="43071-155">Bläddra toohello [Azure-portalen] och logga in.</span><span class="sxs-lookup"><span data-stu-id="43071-155">Browse toohello [Azure portal] and sign in.</span></span>

1. <span data-ttu-id="43071-156">Hello menyn ikonen för **+ ny**, klicka på **webb + mobilt**, och klicka sedan på **webbprogrammet på Linux**.</span><span class="sxs-lookup"><span data-stu-id="43071-156">Click hello menu icon for **+ New**, then click **Web + Mobile**, and then click **Web App on Linux**.</span></span>
   
   ![Skapa en ny webbapp i hello Azure-portalen][LX01]

1. <span data-ttu-id="43071-158">När hello **webbprogrammet på Linux** visas, ange hello följande information:</span><span class="sxs-lookup"><span data-stu-id="43071-158">When hello **Web App on Linux** page is displayed, enter hello following information:</span></span>

   <span data-ttu-id="43071-159">a.</span><span class="sxs-lookup"><span data-stu-id="43071-159">a.</span></span> <span data-ttu-id="43071-160">Ange ett unikt namn för hello **appnamn**, till exempel ”:*wingtiptoyslinux*”.</span><span class="sxs-lookup"><span data-stu-id="43071-160">Enter a unique name for hello **App name**; for example: "*wingtiptoyslinux*."</span></span>

   <span data-ttu-id="43071-161">b.</span><span class="sxs-lookup"><span data-stu-id="43071-161">b.</span></span> <span data-ttu-id="43071-162">Välj din **prenumeration** hello nedrullningsbara listan.</span><span class="sxs-lookup"><span data-stu-id="43071-162">Choose your **Subscription** from hello drop-down list.</span></span>

   <span data-ttu-id="43071-163">c.</span><span class="sxs-lookup"><span data-stu-id="43071-163">c.</span></span> <span data-ttu-id="43071-164">Välj en befintlig **resursgruppen**, eller ange name-toocreate en ny resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="43071-164">Choose an existing **Resource Group**, or specify a name toocreate a new resource group.</span></span>

   <span data-ttu-id="43071-165">d.</span><span class="sxs-lookup"><span data-stu-id="43071-165">d.</span></span> <span data-ttu-id="43071-166">Klicka på **konfigurera behållaren** och ange hello följande information:</span><span class="sxs-lookup"><span data-stu-id="43071-166">Click **Configure container** and enter hello following information:</span></span>

      * <span data-ttu-id="43071-167">Välj **privata registret**.</span><span class="sxs-lookup"><span data-stu-id="43071-167">Choose **Private registry**.</span></span>

      * <span data-ttu-id="43071-168">**Avbildningen och valfria taggen**: Ange din behållarnamn från tidigare, till exempel ”:*wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest*”</span><span class="sxs-lookup"><span data-stu-id="43071-168">**Image and optional tag**: Specify your container name from earlier; for example: "*wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest*"</span></span>

      * <span data-ttu-id="43071-169">**Serveradress**: Ange registret URL: en från tidigare, till exempel ”:*https://wingtiptoysregistry.azurecr.io*”</span><span class="sxs-lookup"><span data-stu-id="43071-169">**Server URL**: Specify your registry URL from earlier; for example: "*https://wingtiptoysregistry.azurecr.io*"</span></span>

      * <span data-ttu-id="43071-170">**Användarnamn för inloggning** och **lösenord**: Ange dina inloggningsuppgifter från din **åtkomstnycklar** som du använde i föregående steg.</span><span class="sxs-lookup"><span data-stu-id="43071-170">**Login username** and **Password**: Specify your login credentials from your **Access Keys** that you used in previous steps.</span></span>
   
   <span data-ttu-id="43071-171">e.</span><span class="sxs-lookup"><span data-stu-id="43071-171">e.</span></span> <span data-ttu-id="43071-172">När du har angett alla hello ovan information klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="43071-172">Once you have entered all of hello above information, click **OK**.</span></span>

   ![Konfigurera inställningarna för webbappen][LX02]

1. <span data-ttu-id="43071-174">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="43071-174">Click **Create**.</span></span>

> [!NOTE]
>
> <span data-ttu-id="43071-175">Azure kommer automatiskt att mappa begäranden tooembedded Tomcat Internetserver som körs på hello standardporten 80 eller 8080.</span><span class="sxs-lookup"><span data-stu-id="43071-175">Azure will automatically map Internet requests tooembedded Tomcat server that is running on hello standard ports of 80 or 8080.</span></span> <span data-ttu-id="43071-176">Om du har konfigurerat dina inbäddade Tomcat server toorun på en anpassad port, måste du tooadd en miljö variabeln tooyour webbprogram som definierar hello port för inbäddade Tomcat-servern.</span><span class="sxs-lookup"><span data-stu-id="43071-176">However, if you configured your embedded Tomcat server toorun on a custom port, you need tooadd an environment variable tooyour web app that defines hello port for your embedded Tomcat server.</span></span> <span data-ttu-id="43071-177">toodo så Använd hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="43071-177">toodo so, use hello following steps:</span></span>
>
> 1. <span data-ttu-id="43071-178">Bläddra toohello [Azure-portalen] och logga in.</span><span class="sxs-lookup"><span data-stu-id="43071-178">Browse toohello [Azure portal] and sign in.</span></span>
> 
> 2. <span data-ttu-id="43071-179">Klicka på ikonen hello för **Apptjänster**.</span><span class="sxs-lookup"><span data-stu-id="43071-179">Click hello icon for **App Services**.</span></span> <span data-ttu-id="43071-180">(Se artikeln #1 i hello bilden nedan.)</span><span class="sxs-lookup"><span data-stu-id="43071-180">(See item #1 in hello image below.)</span></span>
>
> 3. <span data-ttu-id="43071-181">Välj ditt webbprogram hello-listan.</span><span class="sxs-lookup"><span data-stu-id="43071-181">Select your web app from hello list.</span></span> <span data-ttu-id="43071-182">(Artikeln #2 i hello bilden nedan.)</span><span class="sxs-lookup"><span data-stu-id="43071-182">(Item #2 in hello image below.)</span></span>
>
> 4. <span data-ttu-id="43071-183">Klicka på **programinställningar**.</span><span class="sxs-lookup"><span data-stu-id="43071-183">Click **Application Settings**.</span></span> <span data-ttu-id="43071-184">(Artikeln #3 i hello bilden nedan.)</span><span class="sxs-lookup"><span data-stu-id="43071-184">(Item #3 in hello image below.)</span></span>
>
> 5. <span data-ttu-id="43071-185">I hello **appinställningar** lägger du till en ny miljövariabel med namnet **PORT** och ange din anpassade portnummer för hello värde.</span><span class="sxs-lookup"><span data-stu-id="43071-185">In hello **App settings** section, add a new environment variable named **PORT** and enter your custom port number for hello value.</span></span> <span data-ttu-id="43071-186">(Artikeln #4 i hello bilden nedan.)</span><span class="sxs-lookup"><span data-stu-id="43071-186">(Item #4 in hello image below.)</span></span>
>
> 6. <span data-ttu-id="43071-187">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="43071-187">Click **Save**.</span></span> <span data-ttu-id="43071-188">(Artikeln #5 i hello bilden nedan.)</span><span class="sxs-lookup"><span data-stu-id="43071-188">(Item #5 in hello image below.)</span></span>
>
> ![Om du sparar ett anpassat portnummer i hello Azure-portalen][LX03]
>

<!--
##  OPTIONAL: Configure hello embedded Tomcat server toorun on a different port

hello embedded Tomcat server in hello sample Spring Boot application is configured toorun on port 8080 by default. However, if you want toorun hello embedded Tomcat server toorun on a different port, such as port 80 for local testing, you can configure hello port by using hello following steps.

1. Go toohello *resources* directory (or create hello directory if it does not exist); for example:
   ```shell
   cd src/main/resources
   ```

1. Open hello *application.yml* file in a text editor if it exists, or create a new YAML file if it does not exist.

1. Modify hello **server** setting so that hello server runs on port 80; for example:
   ```yaml
   server:
      port: 80
   ```

1. Save and close hello *application.yml* file.
-->

## <a name="next-steps"></a><span data-ttu-id="43071-190">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="43071-190">Next steps</span></span>

<span data-ttu-id="43071-191">Mer information om hur du använder värdet startprogram i Azure finns i hello följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="43071-191">For more information about using Spring Boot applications on Azure, see hello following articles:</span></span>

* [<span data-ttu-id="43071-192">Distribuera en källan startprogrammet toohello Azure App Service</span><span class="sxs-lookup"><span data-stu-id="43071-192">Deploy a Spring Boot Application toohello Azure App Service</span></span>](../../app-service/app-service-deploy-spring-boot-web-app-on-azure.md)
* [<span data-ttu-id="43071-193">Distribuera ett program för start av värdet på ett kluster med Kubernetes på hello Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="43071-193">Deploy a Spring Boot Application on a Kubernetes Cluster in hello Azure Container Service</span></span>](container-service-deploy-spring-boot-app-on-kubernetes.md)

<span data-ttu-id="43071-194">Mer information om hur du använder Azure med Java finns hello [Azure Java Developer Center] och hello [Java-verktyg för Visual Studio Team Services].</span><span class="sxs-lookup"><span data-stu-id="43071-194">For more information about using Azure with Java, see hello [Azure Java Developer Center] and hello [Java Tools for Visual Studio Team Services].</span></span>

<span data-ttu-id="43071-195">Mer information om hello källan Start på Docker exempelprojektet finns [Vårversionen Start på Docker komma igång].</span><span class="sxs-lookup"><span data-stu-id="43071-195">For further details about hello Spring Boot on Docker sample project, see [Spring Boot on Docker Getting Started].</span></span>

<span data-ttu-id="43071-196">Hjälp med att komma igång med din egen källan startprogram, finns hello **Vårversionen Initializr** på https://start.spring.io/.</span><span class="sxs-lookup"><span data-stu-id="43071-196">For help with getting started with your own Spring Boot applications, see hello **Spring Initializr** at https://start.spring.io/.</span></span>

<span data-ttu-id="43071-197">Mer information om att komma igång med att skapa ett enkelt källan Start-program finns i hello källan Initializr på https://start.spring.io/.</span><span class="sxs-lookup"><span data-stu-id="43071-197">For more information about getting started with creating a simple Spring Boot application, see hello Spring Initializr at https://start.spring.io/.</span></span>

<span data-ttu-id="43071-198">Ytterligare exempel för hur toouse anpassade Docker bilder med Azure finns [med hjälp av en anpassad Docker-avbildning för Azure Web App på Linux].</span><span class="sxs-lookup"><span data-stu-id="43071-198">For additional examples for how toouse custom Docker images with Azure, see [Using a custom Docker image for Azure Web App on Linux].</span></span>

<!-- URL List -->

[Azure-kommandoradsgränssnittet (CLI)]: /cli/azure/overview
[Azure Container Service (ACS)]: https://azure.microsoft.com/services/container-service/
[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[Azure-portalen]: https://portal.azure.com/
[skapa en privat Docker behållare registret med hjälp av hello Azure-portalen]: /azure/container-registry/container-registry-get-started-portal
[med hjälp av en anpassad Docker-avbildning för Azure Web App på Linux]: /azure/app-service-web/app-service-linux-using-custom-docker-image
[Docker]: https://www.docker.com/
[kostnadsfritt Azure-konto]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Java-verktyg för Visual Studio Team Services]: https://java.visualstudio.com/
[Maven]: http://maven.apache.org/
[MSDN-prenumerantförmåner]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Vårversionen Start]: http://projects.spring.io/spring-boot/
[Vårversionen Start på Docker komma igång]: https://github.com/spring-guides/gs-spring-boot-docker
[Vårversionen Framework]: https://spring.io/

<!-- IMG List -->

[SB01]: ./media/container-service-deploy-spring-boot-app-on-linux/SB01.png
[SB02]: ./media/container-service-deploy-spring-boot-app-on-linux/SB02.png

[AR01]: ./media/container-service-deploy-spring-boot-app-on-linux/AR01.png
[AR02]: ./media/container-service-deploy-spring-boot-app-on-linux/AR02.png
[AR03]: ./media/container-service-deploy-spring-boot-app-on-linux/AR03.png
[AR04]: ./media/container-service-deploy-spring-boot-app-on-linux/AR04.png

[LX01]: ./media/container-service-deploy-spring-boot-app-on-linux/LX01.png
[LX02]: ./media/container-service-deploy-spring-boot-app-on-linux/LX02.png
[LX03]: ./media/container-service-deploy-spring-boot-app-on-linux/LX03.png
