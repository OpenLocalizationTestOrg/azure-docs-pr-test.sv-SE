---
title: "aaaHow toouse hello Maven-plugin-programmet för Azure Web Apps toodeploy en av vår Start app tooAzure"
description: "Lär dig hur toouse hello Maven-plugin-programmet för Azure Web Apps toodeploy en start av vår app tooAzure."
services: app-service\web
documentationcenter: java
author: rmcmurray
manager: cfowler
editor: 
ms.assetid: 
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 08/07/2017
ms.author: robmcm;kevinzha
ms.openlocfilehash: e7e760d4ef5bd4c92a4126a50a2b12e5c8f2b4a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-maven-plugin-for-azure-web-apps-toodeploy-a-containerized-spring-boot-app-tooazure"></a><span data-ttu-id="64c12-103">Hur toouse hello Maven-plugin-programmet för Azure Web Apps toodeploy en av vår Start app tooAzure</span><span class="sxs-lookup"><span data-stu-id="64c12-103">How toouse hello Maven Plugin for Azure Web Apps toodeploy a containerized Spring Boot app tooAzure</span></span>

<span data-ttu-id="64c12-104">Hej [Maven plugin-program för Azure-Webbappar](https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin) för [Apache Maven](http://maven.apache.org/) ger sömlös integration av Azure App Service till Maven-projekt och förenklar hello process för utvecklare toodeploy web apps tooAzure Apptjänst.</span><span class="sxs-lookup"><span data-stu-id="64c12-104">hello [Maven Plugin for Azure Web Apps](https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin) for [Apache Maven](http://maven.apache.org/) provides seamless integration of Azure App Service  into Maven projects, and streamlines hello process for developers toodeploy web apps tooAzure App Service .</span></span>

<span data-ttu-id="64c12-105">Den här artikeln visas hur du använder hello Maven-plugin-programmet för Azure Web Apps toodeploy ett exempelprogram källan Start i en Docker-behållare tooAzure Apptjänster.</span><span class="sxs-lookup"><span data-stu-id="64c12-105">This article demonstrates using hello Maven Plugin for Azure Web Apps toodeploy a sample Spring Boot application in a Docker container tooAzure App Services.</span></span>

> [!NOTE]
>
> <span data-ttu-id="64c12-106">Hej Maven plugin-program för Azure Web Apps är tillgänglig som en förhandsgranskning.</span><span class="sxs-lookup"><span data-stu-id="64c12-106">hello Maven Plugin for Azure Web Apps is currently available as a preview.</span></span> <span data-ttu-id="64c12-107">För närvarande stöds endast FTP-publicering, även om ytterligare funktioner som planeras att hello framtida.</span><span class="sxs-lookup"><span data-stu-id="64c12-107">For now, only FTP publishing is supported, although additional features are planned for hello future.</span></span>
>

## <a name="prerequisites"></a><span data-ttu-id="64c12-108">Krav</span><span class="sxs-lookup"><span data-stu-id="64c12-108">Prerequisites</span></span>

<span data-ttu-id="64c12-109">I ordning toocomplete hello steg i den här kursen behöver du toohave hello följande krav:</span><span class="sxs-lookup"><span data-stu-id="64c12-109">In order toocomplete hello steps in this tutorial, you need toohave hello following prerequisites:</span></span>

* <span data-ttu-id="64c12-110">En Azure-prenumeration; Om du inte redan har en Azure-prenumeration, kan du aktivera din [MSDN-prenumerantförmåner] eller registrera dig för en [kostnadsfritt Azure-konto].</span><span class="sxs-lookup"><span data-stu-id="64c12-110">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="64c12-111">Hej [Azure-kommandoradsgränssnittet (CLI)].</span><span class="sxs-lookup"><span data-stu-id="64c12-111">hello [Azure Command-Line Interface (CLI)].</span></span>
* <span data-ttu-id="64c12-112">En uppdaterad [Java Development Kit (JDK)], version 1.7 eller senare.</span><span class="sxs-lookup"><span data-stu-id="64c12-112">An up-to-date [Java Development Kit (JDK)], version 1.7 or later.</span></span>
* <span data-ttu-id="64c12-113">Apache's [Maven] skapa verktyget (Version 3).</span><span class="sxs-lookup"><span data-stu-id="64c12-113">Apache's [Maven] build tool (Version 3).</span></span>
* <span data-ttu-id="64c12-114">En [Git] klienten.</span><span class="sxs-lookup"><span data-stu-id="64c12-114">A [Git] client.</span></span>
* <span data-ttu-id="64c12-115">En [Docker] klienten.</span><span class="sxs-lookup"><span data-stu-id="64c12-115">A [Docker] client.</span></span>

> [!NOTE]
>
> <span data-ttu-id="64c12-116">På grund av toohello virtualisering kraven i den här kursen får inte kan du använda hello stegen i den här artikeln på en virtuell dator; Du måste använda en fysisk dator med aktiverat funktionerna för virtualisering.</span><span class="sxs-lookup"><span data-stu-id="64c12-116">Due toohello virtualization requirements of this tutorial, you cannot follow hello steps in this article on a virtual machine; you must use a physical computer with virtualization features enabled.</span></span>
>

## <a name="clone-hello-sample-spring-boot-on-docker-web-app"></a><span data-ttu-id="64c12-117">Klona hello exempel källan Start på Docker-webbprogram</span><span class="sxs-lookup"><span data-stu-id="64c12-117">Clone hello sample Spring Boot on Docker web app</span></span>

<span data-ttu-id="64c12-118">I det här avsnittet klona en container startprogrammet för källan och testa den lokalt.</span><span class="sxs-lookup"><span data-stu-id="64c12-118">In this section, you clone a containerized Spring Boot application and test it locally.</span></span>

1. <span data-ttu-id="64c12-119">Öppna en kommandotolk eller ett terminalfönster och skapa en lokal katalog toohold källan Start programmet och ändra toothat katalog. Exempel:</span><span class="sxs-lookup"><span data-stu-id="64c12-119">Open a command prompt or terminal window and create a local directory toohold your Spring Boot application, and change toothat directory; for example:</span></span>
   ```shell
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   <span data-ttu-id="64c12-120">-- eller--</span><span class="sxs-lookup"><span data-stu-id="64c12-120">-- or --</span></span>
   ```shell
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. <span data-ttu-id="64c12-121">Klona hello [Vårversionen Start på Docker komma igång] exempelprojektet i hello-katalog som du har skapat, till exempel:</span><span class="sxs-lookup"><span data-stu-id="64c12-121">Clone hello [Spring Boot on Docker Getting Started] sample project into hello directory you created; for example:</span></span>
   ```shell
   git clone https://github.com/microsoft/gs-spring-boot-docker
   ```

1. <span data-ttu-id="64c12-122">Ändra katalogen toohello slutförts projektet. Exempel:</span><span class="sxs-lookup"><span data-stu-id="64c12-122">Change directory toohello completed project; for example:</span></span>
   ```shell
   cd gs-spring-boot-docker/complete
   ```

1. <span data-ttu-id="64c12-123">Skapa hello JAR-filen med Maven; Exempel:</span><span class="sxs-lookup"><span data-stu-id="64c12-123">Build hello JAR file using Maven; for example:</span></span>
   ```shell
   mvn clean package
   ```

1. <span data-ttu-id="64c12-124">När hello webbprogrammet har skapats, starta hello webbprogrammet med Maven; Exempel:</span><span class="sxs-lookup"><span data-stu-id="64c12-124">When hello web app has been created, start hello web app using Maven; for example:</span></span>
   ```shell
   mvn spring-boot:run
   ```

1. <span data-ttu-id="64c12-125">Testa hello webbprogram genom att bläddra tooit lokalt med hjälp av en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="64c12-125">Test hello web app by browsing tooit locally using a web browser.</span></span> <span data-ttu-id="64c12-126">Du kan till exempel använda följande kommando om du har curl tillgängliga hello:</span><span class="sxs-lookup"><span data-stu-id="64c12-126">For example, you could use hello following command if you have curl available:</span></span>
   ```shell
   curl http://localhost:8080
   ```

1. <span data-ttu-id="64c12-127">Du bör se hello följande meddelande visas: **Docker hälsningsmeddelande**</span><span class="sxs-lookup"><span data-stu-id="64c12-127">You should see hello following message displayed: **Hello Docker World**</span></span>

## <a name="create-an-azure-service-principal"></a><span data-ttu-id="64c12-128">Skapa en Azure huvudnamn för tjänsten</span><span class="sxs-lookup"><span data-stu-id="64c12-128">Create an Azure service principal</span></span>

<span data-ttu-id="64c12-129">I det här avsnittet skapar du en Azure tjänstens huvudnamn som hello Maven plugin-programmet använder när du distribuerar behållare-tooAzure.</span><span class="sxs-lookup"><span data-stu-id="64c12-129">In this section, you create an Azure service principal that hello Maven plugin uses when deploying your container tooAzure.</span></span>

1. <span data-ttu-id="64c12-130">Öppna en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="64c12-130">Open a command prompt.</span></span>

1. <span data-ttu-id="64c12-131">Logga in på ditt Azure-konto med hjälp av hello Azure CLI:</span><span class="sxs-lookup"><span data-stu-id="64c12-131">Sign into your Azure account by using hello Azure CLI:</span></span>
   ```shell
   az login
   ```
   <span data-ttu-id="64c12-132">Följ hello instruktioner toocomplete hello inloggningsprocessen.</span><span class="sxs-lookup"><span data-stu-id="64c12-132">Follow hello instructions toocomplete hello sign-in process.</span></span>

1. <span data-ttu-id="64c12-133">Skapa en Azure-tjänstens huvudnamn:</span><span class="sxs-lookup"><span data-stu-id="64c12-133">Create an Azure service principal:</span></span>
   ```shell
   az ad sp create-for-rbac --name "uuuuuuuu" --password "pppppppp"
   ```
   <span data-ttu-id="64c12-134">Där `uuuuuuuu` är hello användarnamn och `pppppppp` är hello lösenord för hello tjänstens huvudnamn.</span><span class="sxs-lookup"><span data-stu-id="64c12-134">Where `uuuuuuuu` is hello user name and `pppppppp` is hello password for hello service principal.</span></span>

1. <span data-ttu-id="64c12-135">Azure svarar med JSON som liknar följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="64c12-135">Azure responds with JSON that resembles hello following example:</span></span>
   ```json
   {
      "appId": "aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa",
      "displayName": "uuuuuuuu",
      "name": "http://uuuuuuuu",
      "password": "pppppppp",
      "tenant": "tttttttt-tttt-tttt-tttt-tttttttttttt"
   }
   ```

   > [!NOTE]
   >
   > <span data-ttu-id="64c12-136">Du använder hello värden från den här JSON-svar när du konfigurerar hello Maven plugin-programmet toodeploy behållaren-tooAzure.</span><span class="sxs-lookup"><span data-stu-id="64c12-136">You will use hello values from this JSON response when you configure hello Maven plugin toodeploy your container tooAzure.</span></span> <span data-ttu-id="64c12-137">hello `aaaaaaaa`, `uuuuuuuu`, `pppppppp`, och `tttttttt` platshållare för värden som används i det här exemplet toomake den enklare toomap värden tootheir respektive elementen när du konfigurerar din Maven `settings.xml` filen i hello nästa avsnittet.</span><span class="sxs-lookup"><span data-stu-id="64c12-137">hello `aaaaaaaa`, `uuuuuuuu`, `pppppppp`, and `tttttttt` are placeholder values, which are used in this example toomake it easier toomap these values tootheir respective elements when you configure your Maven `settings.xml` file in hello next section.</span></span>
   >
   >

## <a name="configure-maven-toouse-your-azure-service-principal"></a><span data-ttu-id="64c12-138">Konfigurera Maven toouse din Azure-tjänstens huvudnamn</span><span class="sxs-lookup"><span data-stu-id="64c12-138">Configure Maven toouse your Azure service principal</span></span>

<span data-ttu-id="64c12-139">I det här avsnittet använder du hello värden från Azure-tjänstens huvudnamn tooconfigure hello autentiseringen som Maven ska använda när du distribuerar behållare-tooAzure.</span><span class="sxs-lookup"><span data-stu-id="64c12-139">In this section, you use hello values from your Azure service principal tooconfigure hello authentication that Maven will use when deploying your container tooAzure.</span></span>

1. <span data-ttu-id="64c12-140">Öppna din Maven `settings.xml` filen i en textredigerare; den här filen kan vara en sökväg som hello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="64c12-140">Open your Maven `settings.xml` file in a text editor; this file might be in a path like hello following examples:</span></span>
   * `/etc/maven/settings.xml`
   * `%ProgramFiles%\apache-maven\3.5.0\conf\settings.xml`
   * `$HOME/.m2/settings.xml`

1. <span data-ttu-id="64c12-141">Lägg till Azure-tjänstens huvudnamn inställningarna från hello föregående avsnitt i den här självstudiekursen toohello `<servers>` samling i hello *settings.xml* filen, till exempel:</span><span class="sxs-lookup"><span data-stu-id="64c12-141">Add your Azure service principal settings from hello previous section of this tutorial toohello `<servers>` collection in hello *settings.xml* file; for example:</span></span>

   ```xml
   <servers>
      <server>
        <id>azure-auth</id>
         <configuration>
            <client>aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa</client>
            <tenant>tttttttt-tttt-tttt-tttt-tttttttttttt</tenant>
            <key>pppppppp</key>
            <environment>AZURE</environment>
         </configuration>
      </server>
   </servers>
   ```
   <span data-ttu-id="64c12-142">Där:</span><span class="sxs-lookup"><span data-stu-id="64c12-142">Where:</span></span>
   <span data-ttu-id="64c12-143">Element</span><span class="sxs-lookup"><span data-stu-id="64c12-143">Element</span></span> | <span data-ttu-id="64c12-144">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="64c12-144">Description</span></span>
   ---|---|---
   `<id>` | <span data-ttu-id="64c12-145">Anger ett unikt namn som Maven använder toolook in dina säkerhetsinställningar när du distribuerar din web app tooAzure.</span><span class="sxs-lookup"><span data-stu-id="64c12-145">Specifies a unique name which Maven uses toolook up your security settings when you deploy your web app tooAzure.</span></span>
   `<client>` | <span data-ttu-id="64c12-146">Innehåller hello `appId` värde från tjänstens huvudnamn.</span><span class="sxs-lookup"><span data-stu-id="64c12-146">Contains hello `appId` value from your service principal.</span></span>
   `<tenant>` | <span data-ttu-id="64c12-147">Innehåller hello `tenant` värde från tjänstens huvudnamn.</span><span class="sxs-lookup"><span data-stu-id="64c12-147">Contains hello `tenant` value from your service principal.</span></span>
   `<key>` | <span data-ttu-id="64c12-148">Innehåller hello `password` värde från tjänstens huvudnamn.</span><span class="sxs-lookup"><span data-stu-id="64c12-148">Contains hello `password` value from your service principal.</span></span>
   `<environment>` | <span data-ttu-id="64c12-149">Definierar hello Azure-molnet målmiljön, vilket är `AZURE` i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="64c12-149">Defines hello target Azure cloud environment, which is `AZURE` in this example.</span></span> <span data-ttu-id="64c12-150">(En fullständig lista över miljöer finns i hello [Maven plugin-program för Azure Web Apps] dokumentation)</span><span class="sxs-lookup"><span data-stu-id="64c12-150">(A full list of environments is available in hello [Maven Plugin for Azure Web Apps] documentation)</span></span>

1. <span data-ttu-id="64c12-151">Spara och Stäng hello *settings.xml* fil.</span><span class="sxs-lookup"><span data-stu-id="64c12-151">Save and close hello *settings.xml* file.</span></span>

## <a name="optional-deploy-your-local-docker-file-toodocker-hub"></a><span data-ttu-id="64c12-152">Valfritt: Distribuera din lokala Docker filen tooDocker Hub</span><span class="sxs-lookup"><span data-stu-id="64c12-152">OPTIONAL: Deploy your local Docker file tooDocker Hub</span></span>

<span data-ttu-id="64c12-153">Om du har en Docker-konto du push-installera den tooDocker hubb och skapa din Docker behållaren image lokalt.</span><span class="sxs-lookup"><span data-stu-id="64c12-153">If you have a Docker account, you can build your Docker container image locally and push it tooDocker Hub.</span></span> <span data-ttu-id="64c12-154">toodo så Använd hello följande steg.</span><span class="sxs-lookup"><span data-stu-id="64c12-154">toodo so, use hello following steps.</span></span>

1. <span data-ttu-id="64c12-155">Öppna hello `pom.xml` filen för tillämpningsprogrammet källan Start i en textredigerare.</span><span class="sxs-lookup"><span data-stu-id="64c12-155">Open hello `pom.xml` file for your Spring Boot application in a text editor.</span></span>

1. <span data-ttu-id="64c12-156">Leta upp hello `<imageName>` underordnat element för hello `<containerSettings>` element.</span><span class="sxs-lookup"><span data-stu-id="64c12-156">Locate hello `<imageName>` child element of hello `<containerSettings>` element.</span></span>

1. <span data-ttu-id="64c12-157">Uppdatera hello `${docker.image.prefix}` värdet med namnet på ditt Docker:</span><span class="sxs-lookup"><span data-stu-id="64c12-157">Update hello `${docker.image.prefix}` value with your Docker account name:</span></span>
   ```xml
   <containerSettings>
      <imageName>mydockeraccountname/${project.artifactId}</imageName>
   </containerSettings>
   ```

1. <span data-ttu-id="64c12-158">Välj något av följande distributionsmetoder hello:</span><span class="sxs-lookup"><span data-stu-id="64c12-158">Choose one of hello following deployment methods:</span></span>

   * <span data-ttu-id="64c12-159">Skapa behållaren avbildningen lokalt med Maven, och sedan använda Docker toopush din behållaren tooDocker hubb:</span><span class="sxs-lookup"><span data-stu-id="64c12-159">Build your container image locally with Maven, and then use Docker toopush your container tooDocker Hub:</span></span>
      ```shell
      mvn clean package docker:build
      docker push
      ```
   
   * <span data-ttu-id="64c12-160">Om du har hello [Docker-plugin för Maven] installerat, kan du automatiskt bygga och din behållaren image tooDocker hubb med hjälp av hello `-DpushImage` parameter:</span><span class="sxs-lookup"><span data-stu-id="64c12-160">If you have hello [Docker plugin for Maven] installed, you can automatically build and your container image tooDocker Hub by using hello `-DpushImage` parameter:</span></span>
      ```shell
      mvn clean package docker:build -DpushImage
      ```

## <a name="optional-customize-your-pomxml-before-deploying-your-container-tooazure"></a><span data-ttu-id="64c12-161">Valfritt: Anpassa din pom.xml innan du distribuerar behållare-tooAzure</span><span class="sxs-lookup"><span data-stu-id="64c12-161">OPTIONAL: Customize your pom.xml before deploying your container tooAzure</span></span>

<span data-ttu-id="64c12-162">Öppna hello `pom.xml` filen för tillämpningsprogrammet källan Start i en textredigerare och leta sedan upp hello `<plugin>` element för `azure-webapp-maven-plugin`.</span><span class="sxs-lookup"><span data-stu-id="64c12-162">Open hello `pom.xml` file for your Spring Boot application in a text editor, and then locate hello `<plugin>` element for `azure-webapp-maven-plugin`.</span></span> <span data-ttu-id="64c12-163">Det här elementet bör likna följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="64c12-163">This element should resemble hello following example:</span></span>

   ```xml
   <plugin>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-webapp-maven-plugin</artifactId>
      <version>0.1.3</version>
      <configuration>
         <authentication>
            <serverId>azure-auth</serverId>
         </authentication>
         <resourceGroup>maven-plugin</resourceGroup>
         <appName>maven-linux-app-${maven.build.timestamp}</appName>
         <region>westus</region>
         <containerSettings>
            <imageName>${docker.image.prefix}/${project.artifactId}</imageName>
         </containerSettings>
         <appSettings>
            <property>
               <name>PORT</name>
               <value>8080</value>
            </property>
         </appSettings>
      </configuration>
   </plugin>
   ```

<span data-ttu-id="64c12-164">Det finns flera värden som du kan ändra för hello Maven plugin-program och en utförlig beskrivning för var och en av dessa element är tillgänglig i hello [Maven plugin-program för Azure Web Apps] dokumentation.</span><span class="sxs-lookup"><span data-stu-id="64c12-164">There are several values that you can modify for hello Maven plugin, and a detailed description for each of these elements is available in hello [Maven Plugin for Azure Web Apps] documentation.</span></span> <span data-ttu-id="64c12-165">Som som säger att det finns flera värden som är värda markering i den här artikeln:</span><span class="sxs-lookup"><span data-stu-id="64c12-165">That being said, there are several values that are worth highlighting in this article:</span></span>

<span data-ttu-id="64c12-166">Element</span><span class="sxs-lookup"><span data-stu-id="64c12-166">Element</span></span> | <span data-ttu-id="64c12-167">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="64c12-167">Description</span></span>
---|---|---
`<version>` | <span data-ttu-id="64c12-168">Anger hello version av hello [Maven plugin-program för Azure Web Apps].</span><span class="sxs-lookup"><span data-stu-id="64c12-168">Specifies hello version of hello [Maven Plugin for Azure Web Apps].</span></span> <span data-ttu-id="64c12-169">Du bör kontrollera hello-version som anges i hello [Maven centrala lagringsplatsen](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-webapp-maven-plugin%22) tooensure som du använder hello senaste versionen.</span><span class="sxs-lookup"><span data-stu-id="64c12-169">You should check hello version listed in hello [Maven Central Respository](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-webapp-maven-plugin%22) tooensure that you are using hello latest version.</span></span>
`<authentication>` | <span data-ttu-id="64c12-170">Anger hello autentiseringsinformationen för Azure, som i det här exemplet innehåller en `<serverId>` element som innehåller `azure-auth`; Maven använder den värdet toolook hello Azure-tjänstens huvudnamn värden i din Maven *settings.xml* fil som du har definierat i en tidigare i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="64c12-170">Specifies hello authentication information for Azure, which in this example contains a `<serverId>` element that contains `azure-auth`; Maven uses that value toolook up hello Azure service principal values in your Maven *settings.xml* file, which you defined in an earlier section of this article.</span></span>
`<resourceGroup>` | <span data-ttu-id="64c12-171">Anger hello målresursgruppen, vilket är `maven-plugin` i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="64c12-171">Specifies hello target resource group, which is `maven-plugin` in this example.</span></span> <span data-ttu-id="64c12-172">hello resursgruppen kommer att skapas under distributionen om det inte redan finns.</span><span class="sxs-lookup"><span data-stu-id="64c12-172">hello resource group will be created during deployment if it does not already exist.</span></span>
`<appName>` | <span data-ttu-id="64c12-173">Anger hello målets namn för ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="64c12-173">Specifies hello target name for your web app.</span></span> <span data-ttu-id="64c12-174">I det här exemplet är hello målnamnet `maven-linux-app-${maven.build.timestamp}`, där hello `${maven.build.timestamp}` suffix läggs till i det här exemplet tooavoid konflikt.</span><span class="sxs-lookup"><span data-stu-id="64c12-174">In this example, hello target name is `maven-linux-app-${maven.build.timestamp}`, where hello `${maven.build.timestamp}` suffix is appended in this example tooavoid conflict.</span></span> <span data-ttu-id="64c12-175">(hello tidsstämpeln är valfria och du kan ange en unik sträng för hello programnamn.)</span><span class="sxs-lookup"><span data-stu-id="64c12-175">(hello timestamp is optional; you can specify any unique string for hello app name.)</span></span>
`<region>` | <span data-ttu-id="64c12-176">Anger hello målregionen, som i det här exemplet är `westus`.</span><span class="sxs-lookup"><span data-stu-id="64c12-176">Specifies hello target region, which in this example is `westus`.</span></span> <span data-ttu-id="64c12-177">(En fullständig lista finns i hello [Maven plugin-program för Azure Web Apps] dokumentation.)</span><span class="sxs-lookup"><span data-stu-id="64c12-177">(A full list is in hello [Maven Plugin for Azure Web Apps] documentation.)</span></span>
`<appSettings>` | <span data-ttu-id="64c12-178">Anger alla unika inställningar för Maven toouse när du distribuerar din web app tooAzure.</span><span class="sxs-lookup"><span data-stu-id="64c12-178">Specifies any unique settings for Maven toouse when deploying your web app tooAzure.</span></span> <span data-ttu-id="64c12-179">I det här exemplet en `<property>` elementet innehåller ett namn/värde-par för underordnade element som anger hello port för din app.</span><span class="sxs-lookup"><span data-stu-id="64c12-179">In this example, a `<property>` element contains a name/value pair of child elements that specify hello port for your app.</span></span>

> [!NOTE]
>
> <span data-ttu-id="64c12-180">portnummer för hello inställningar toochange hello i det här exemplet krävs bara när du ändrar hello port hello standardprincipen.</span><span class="sxs-lookup"><span data-stu-id="64c12-180">hello settings toochange hello port number in this example are only necessary when you are changing hello port from hello default.</span></span>
>

## <a name="build-and-deploy-your-container-tooazure"></a><span data-ttu-id="64c12-181">Skapa och distribuera behållaren-tooAzure</span><span class="sxs-lookup"><span data-stu-id="64c12-181">Build and deploy your container tooAzure</span></span>

<span data-ttu-id="64c12-182">När du har konfigurerat alla hello inställningar i föregående avsnitt i den här artikeln hello, är du redo toodeploy behållaren-tooAzure.</span><span class="sxs-lookup"><span data-stu-id="64c12-182">Once you have configured all of hello settings in hello preceding sections of this article, you are ready toodeploy your container tooAzure.</span></span> <span data-ttu-id="64c12-183">toodo så Använd hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="64c12-183">toodo so, use hello following steps:</span></span>

1. <span data-ttu-id="64c12-184">Från Kommandotolken hello eller terminalfönster som du använde tidigare återskapa hello JAR-filen med Maven om du har gjort några ändringar toohello *pom.xml* filen, till exempel:</span><span class="sxs-lookup"><span data-stu-id="64c12-184">From hello command prompt or terminal window that you were using earlier, rebuild hello JAR file using Maven if you made any changes toohello *pom.xml* file; for example:</span></span>
   ```shell
   mvn clean package
   ```

1. <span data-ttu-id="64c12-185">Distribuera din web app tooAzure med Maven; Exempel:</span><span class="sxs-lookup"><span data-stu-id="64c12-185">Deploy your web app tooAzure by using Maven; for example:</span></span>
   ```shell
   mvn azure-webapp:deploy
   ```

<span data-ttu-id="64c12-186">Maven distribuerar din web app tooAzure; Om hello webbapp inte redan finns, skapas.</span><span class="sxs-lookup"><span data-stu-id="64c12-186">Maven will deploy your web app tooAzure; if hello web app does not already exist, it will be created.</span></span>

> [!NOTE]
>
> <span data-ttu-id="64c12-187">Om hello region som du anger i hello `<region>` element i din *pom.xml* filen har inte tillräckligt många servrar som är tillgängliga när du börjar din distribution, visas ett felmeddelande liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="64c12-187">If hello region which you specify in hello `<region>` element of your *pom.xml* file does not have enough servers available when you start your deployment, you might see an error similar toohello following example:</span></span>
>
> ```
> [INFO] Start deploying tooWeb App maven-linux-app-20170804...
> [INFO] ------------------------------------------------------------------------
> [INFO] BUILD FAILURE
> [INFO] ------------------------------------------------------------------------
> [INFO] Total time: 31.059 s
> [INFO] Finished at: 2017-08-04T12:15:47-07:00
> [INFO] Final Memory: 51M/279M
> [INFO] ------------------------------------------------------------------------
> [ERROR] Failed tooexecute goal com.microsoft.azure:azure-webapp-maven-plugin:0.1.3:deploy (default-cli) on project gs-spring-boot-docker: null: MojoExecutionException: CloudException: OnError while emitting onNext value: retrofit2.Response.class
> ```
>
> <span data-ttu-id="64c12-188">Om det händer kan ange du en annan region och kör igen hello Maven-kommando toodeploy ditt program.</span><span class="sxs-lookup"><span data-stu-id="64c12-188">If this happens, you can specify another region and re-run hello Maven command toodeploy your application.</span></span>
>
>

<span data-ttu-id="64c12-189">När webbplatsen har distribuerats, kommer du att kunna toomanage den med hjälp av hello [Azure-portalen].</span><span class="sxs-lookup"><span data-stu-id="64c12-189">When your web has been deployed, you will be able toomanage it by using hello [Azure portal].</span></span>

* <span data-ttu-id="64c12-190">Ditt webbprogram visas i **Apptjänster**:</span><span class="sxs-lookup"><span data-stu-id="64c12-190">Your web app will be listed in **App Services**:</span></span>

   ![Webbprogram som anges i Azure portal Apptjänster][AP01]

* <span data-ttu-id="64c12-192">Och hello URL för ditt webbprogram visas i hello **översikt** för ditt webbprogram:</span><span class="sxs-lookup"><span data-stu-id="64c12-192">And hello URL for your web app will be listed in hello **Overview** for your web app:</span></span>

   ![När du fastställer hello URL: en för webbappen][AP02]

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

## <a name="next-steps"></a><span data-ttu-id="64c12-194">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="64c12-194">Next steps</span></span>

<span data-ttu-id="64c12-195">Mer information om hello finns olika tekniker som beskrivs i den här artikeln hello följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="64c12-195">For more information about hello various technologies discussed in this article, see hello following articles:</span></span>

* <span data-ttu-id="64c12-196">[Maven plugin-program för Azure Web Apps]</span><span class="sxs-lookup"><span data-stu-id="64c12-196">[Maven Plugin for Azure Web Apps]</span></span>

* [<span data-ttu-id="64c12-197">Logga in tooAzure från hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="64c12-197">Log in tooAzure from hello Azure CLI</span></span>](/azure/xplat-cli-connect)

* [<span data-ttu-id="64c12-198">Hur toouse hello Maven-plugin-programmet för Azure Web Apps toodeploy en start av vår app tooAzure Apptjänst</span><span class="sxs-lookup"><span data-stu-id="64c12-198">How toouse hello Maven Plugin for Azure Web Apps toodeploy a Spring Boot app tooAzure App Service </span></span>](app-service-web-deploy-spring-boot-app-with-maven-plugin.md)

* [<span data-ttu-id="64c12-199">Skapa en Azure tjänstens huvudnamn med Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="64c12-199">Create an Azure service principal with Azure CLI 2.0</span></span>](/cli/azure/create-an-azure-service-principal-azure-cli)

* [<span data-ttu-id="64c12-200">Referens för maven</span><span class="sxs-lookup"><span data-stu-id="64c12-200">Maven Settings Reference</span></span>](https://maven.apache.org/settings.html)

* <span data-ttu-id="64c12-201">[Docker-plugin för Maven]</span><span class="sxs-lookup"><span data-stu-id="64c12-201">[Docker plugin for Maven]</span></span>

<!-- URL List -->

[Azure-kommandoradsgränssnittet (CLI)]: /cli/azure/overview
[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[Azure-portalen]: https://portal.azure.com/
[Docker]: https://www.docker.com/
[Docker-plugin för Maven]: https://github.com/spotify/docker-maven-plugin
[kostnadsfritt Azure-konto]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[Maven]: http://maven.apache.org/
[MSDN-prenumerantförmåner]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Vårversionen Start på Docker komma igång]: https://github.com/spring-guides/gs-spring-boot-docker
[Spring Framework]: https://spring.io/
[Maven plugin-program för Azure Web Apps]: https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin

<!-- IMG List -->

[AP01]: ./media/app-service-web-deploy-containerized-spring-boot-app-with-maven-plugin/AP01.png
[AP02]: ./media/app-service-web-deploy-containerized-spring-boot-app-with-maven-plugin/AP02.png
