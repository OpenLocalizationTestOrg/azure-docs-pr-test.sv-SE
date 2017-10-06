---
title: "aaaHow toouse hello Maven-plugin-programmet för Azure Web Apps toodeploy en start av vår app tooAzure"
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
ms.openlocfilehash: 376fe90fe20621e15d7c9856214937c78b66026a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-maven-plugin-for-azure-web-apps-toodeploy-a-spring-boot-app-tooazure"></a><span data-ttu-id="386a2-103">Hur toouse hello Maven-plugin-programmet för Azure Web Apps toodeploy en start av vår app tooAzure</span><span class="sxs-lookup"><span data-stu-id="386a2-103">How toouse hello Maven Plugin for Azure Web Apps toodeploy a Spring Boot app tooAzure</span></span>

<span data-ttu-id="386a2-104">Hej [Maven plugin-program för Azure-Webbappar](https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin) för [Apache Maven](http://maven.apache.org/) ger sömlös integration av Azure App Service till Maven-projekt och förenklar hello process för utvecklare toodeploy web apps tooAzure Apptjänst.</span><span class="sxs-lookup"><span data-stu-id="386a2-104">hello [Maven Plugin for Azure Web Apps](https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin) for [Apache Maven](http://maven.apache.org/) provides seamless integration of Azure App Service into Maven projects, and streamlines hello process for developers toodeploy web apps tooAzure App Service.</span></span>

<span data-ttu-id="386a2-105">Den här artikeln visas hur du använder hello Maven-plugin-programmet för Azure Web Apps toodeploy en exempel källan Start programmet tooAzure Apptjänster.</span><span class="sxs-lookup"><span data-stu-id="386a2-105">This article demonstrates using hello Maven Plugin for Azure Web Apps toodeploy a sample Spring Boot application tooAzure App Services.</span></span>

> [!NOTE]
>
> <span data-ttu-id="386a2-106">Hej Maven plugin-program för Azure Web Apps är tillgänglig som en förhandsgranskning.</span><span class="sxs-lookup"><span data-stu-id="386a2-106">hello Maven Plugin for Azure Web Apps is currently available as a preview.</span></span> <span data-ttu-id="386a2-107">För närvarande stöds endast FTP-publicering, även om ytterligare funktioner som planeras att hello framtida.</span><span class="sxs-lookup"><span data-stu-id="386a2-107">For now, only FTP publishing is supported, although additional features are planned for hello future.</span></span>
>

## <a name="prerequisites"></a><span data-ttu-id="386a2-108">Krav</span><span class="sxs-lookup"><span data-stu-id="386a2-108">Prerequisites</span></span>

<span data-ttu-id="386a2-109">I ordning toocomplete hello steg i den här kursen behöver du toohave hello följande krav:</span><span class="sxs-lookup"><span data-stu-id="386a2-109">In order toocomplete hello steps in this tutorial, you need toohave hello following prerequisites:</span></span>

* <span data-ttu-id="386a2-110">En Azure-prenumeration; Om du inte redan har en Azure-prenumeration, kan du aktivera din [MSDN-prenumerantförmåner] eller registrera dig för en [kostnadsfritt Azure-konto].</span><span class="sxs-lookup"><span data-stu-id="386a2-110">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="386a2-111">Hej [Azure-kommandoradsgränssnittet (CLI)].</span><span class="sxs-lookup"><span data-stu-id="386a2-111">hello [Azure Command-Line Interface (CLI)].</span></span>
* <span data-ttu-id="386a2-112">En uppdaterad [Java Development Kit (JDK)], version 1.7 eller senare.</span><span class="sxs-lookup"><span data-stu-id="386a2-112">An up-to-date [Java Development Kit (JDK)], version 1.7 or later.</span></span>
* <span data-ttu-id="386a2-113">Apache's [Maven] skapa verktyget (Version 3).</span><span class="sxs-lookup"><span data-stu-id="386a2-113">Apache's [Maven] build tool (Version 3).</span></span>
* <span data-ttu-id="386a2-114">En [Git] klienten.</span><span class="sxs-lookup"><span data-stu-id="386a2-114">A [Git] client.</span></span>

## <a name="clone-hello-sample-spring-boot-web-app"></a><span data-ttu-id="386a2-115">Klona hello exempelwebbapp för vår Start</span><span class="sxs-lookup"><span data-stu-id="386a2-115">Clone hello sample Spring Boot web app</span></span>

<span data-ttu-id="386a2-116">I det här avsnittet klona en slutförd startprogrammet för källan och testa den lokalt.</span><span class="sxs-lookup"><span data-stu-id="386a2-116">In this section, you clone a completed Spring Boot application and test it locally.</span></span>

1. <span data-ttu-id="386a2-117">Öppna en kommandotolk eller ett terminalfönster och skapa en lokal katalog toohold källan Start programmet och ändra toothat katalog. Exempel:</span><span class="sxs-lookup"><span data-stu-id="386a2-117">Open a command prompt or terminal window and create a local directory toohold your Spring Boot application, and change toothat directory; for example:</span></span>
   ```shell
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   <span data-ttu-id="386a2-118">-- eller--</span><span class="sxs-lookup"><span data-stu-id="386a2-118">-- or --</span></span>
   ```shell
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. <span data-ttu-id="386a2-119">Klona hello [Vårversionen Start komma igång] exempelprojektet i hello-katalog som du har skapat, till exempel:</span><span class="sxs-lookup"><span data-stu-id="386a2-119">Clone hello [Spring Boot Getting Started] sample project into hello directory you created; for example:</span></span>
   ```shell
   git clone https://github.com/microsoft/gs-spring-boot
   ```

1. <span data-ttu-id="386a2-120">Ändra katalogen toohello slutförts projektet. Exempel:</span><span class="sxs-lookup"><span data-stu-id="386a2-120">Change directory toohello completed project; for example:</span></span>
   ```shell
   cd gs-spring-boot/complete
   ```

1. <span data-ttu-id="386a2-121">Skapa hello JAR-filen med Maven; Exempel:</span><span class="sxs-lookup"><span data-stu-id="386a2-121">Build hello JAR file using Maven; for example:</span></span>
   ```shell
   mvn clean package
   ```

1. <span data-ttu-id="386a2-122">När hello webbprogrammet har skapats, starta hello webbprogrammet med Maven; Exempel:</span><span class="sxs-lookup"><span data-stu-id="386a2-122">When hello web app has been created, start hello web app using Maven; for example:</span></span>
   ```shell
   mvn spring-boot:run
   ```

1. <span data-ttu-id="386a2-123">Testa hello webbprogram genom att bläddra tooit lokalt med hjälp av en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="386a2-123">Test hello web app by browsing tooit locally using a web browser.</span></span> <span data-ttu-id="386a2-124">Du kan till exempel använda följande kommando om du har curl tillgängliga hello:</span><span class="sxs-lookup"><span data-stu-id="386a2-124">For example, you could use hello following command if you have curl available:</span></span>
   ```shell
   curl http://localhost:8080
   ```

1. <span data-ttu-id="386a2-125">Du bör se hello följande meddelande visas: **helg från källan Start!**</span><span class="sxs-lookup"><span data-stu-id="386a2-125">You should see hello following message displayed: **Greetings from Spring Boot!**</span></span>

## <a name="create-an-azure-service-principal"></a><span data-ttu-id="386a2-126">Skapa en Azure huvudnamn för tjänsten</span><span class="sxs-lookup"><span data-stu-id="386a2-126">Create an Azure service principal</span></span>

<span data-ttu-id="386a2-127">I det här avsnittet skapar du en Azure tjänstens huvudnamn som hello Maven plugin-programmet använder när du distribuerar din web app tooAzure.</span><span class="sxs-lookup"><span data-stu-id="386a2-127">In this section, you create an Azure service principal that hello Maven plugin uses when deploying your web app tooAzure.</span></span>

1. <span data-ttu-id="386a2-128">Öppna en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="386a2-128">Open a command prompt.</span></span>

1. <span data-ttu-id="386a2-129">Logga in på ditt Azure-konto med hjälp av hello Azure CLI:</span><span class="sxs-lookup"><span data-stu-id="386a2-129">Sign into your Azure account by using hello Azure CLI:</span></span>
   ```shell
   az login
   ```
   <span data-ttu-id="386a2-130">Följ hello instruktioner toocomplete hello inloggningsprocessen.</span><span class="sxs-lookup"><span data-stu-id="386a2-130">Follow hello instructions toocomplete hello sign-in process.</span></span>

1. <span data-ttu-id="386a2-131">Skapa en Azure-tjänstens huvudnamn:</span><span class="sxs-lookup"><span data-stu-id="386a2-131">Create an Azure service principal:</span></span>
   ```shell
   az ad sp create-for-rbac --name "uuuuuuuu" --password "pppppppp"
   ```
   <span data-ttu-id="386a2-132">Där `uuuuuuuu` är hello användarnamn och `pppppppp` är hello lösenord för hello tjänstens huvudnamn.</span><span class="sxs-lookup"><span data-stu-id="386a2-132">Where `uuuuuuuu` is hello user name and `pppppppp` is hello password for hello service principal.</span></span>

1. <span data-ttu-id="386a2-133">Azure svarar med JSON som liknar följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="386a2-133">Azure responds with JSON that resembles hello following example:</span></span>
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
   > <span data-ttu-id="386a2-134">Du använder hello värden från den här JSON-svar när du konfigurerar hello Maven plugin-programmet toodeploy tooAzure dina webb-app.</span><span class="sxs-lookup"><span data-stu-id="386a2-134">You will use hello values from this JSON response when you configure hello Maven plugin toodeploy your web app tooAzure.</span></span> <span data-ttu-id="386a2-135">hello `aaaaaaaa`, `uuuuuuuu`, `pppppppp`, och `tttttttt` platshållare för värden som används i det här exemplet toomake den enklare toomap värden tootheir respektive elementen när du konfigurerar din Maven `settings.xml` filen i hello nästa avsnittet.</span><span class="sxs-lookup"><span data-stu-id="386a2-135">hello `aaaaaaaa`, `uuuuuuuu`, `pppppppp`, and `tttttttt` are placeholder values, which are used in this example toomake it easier toomap these values tootheir respective elements when you configure your Maven `settings.xml` file in hello next section.</span></span>
   >
   >

## <a name="configure-maven-toouse-your-azure-service-principal"></a><span data-ttu-id="386a2-136">Konfigurera Maven toouse din Azure-tjänstens huvudnamn</span><span class="sxs-lookup"><span data-stu-id="386a2-136">Configure Maven toouse your Azure service principal</span></span>

<span data-ttu-id="386a2-137">I det här avsnittet använder du hello värden från din Azure-tjänstens huvudnamn tooconfigure hello autentisering med Maven när du distribuerar din web app tooAzure.</span><span class="sxs-lookup"><span data-stu-id="386a2-137">In this section, you use hello values from your Azure service principal tooconfigure hello authentication that Maven uses when deploying your web app tooAzure.</span></span>

1. <span data-ttu-id="386a2-138">Öppna din Maven `settings.xml` filen i en textredigerare; den här filen kan vara en sökväg som hello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="386a2-138">Open your Maven `settings.xml` file in a text editor; this file might be in a path like hello following examples:</span></span>
   * `/etc/maven/settings.xml`
   * `%ProgramFiles%\apache-maven\3.5.0\conf\settings.xml`
   * `$HOME/.m2/settings.xml`

1. <span data-ttu-id="386a2-139">Lägg till Azure-tjänstens huvudnamn inställningarna från hello föregående avsnitt i den här självstudiekursen toohello `<servers>` samling i hello *settings.xml* filen, till exempel:</span><span class="sxs-lookup"><span data-stu-id="386a2-139">Add your Azure service principal settings from hello previous section of this tutorial toohello `<servers>` collection in hello *settings.xml* file; for example:</span></span>

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
   <span data-ttu-id="386a2-140">Där:</span><span class="sxs-lookup"><span data-stu-id="386a2-140">Where:</span></span>
   <span data-ttu-id="386a2-141">Element</span><span class="sxs-lookup"><span data-stu-id="386a2-141">Element</span></span> | <span data-ttu-id="386a2-142">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="386a2-142">Description</span></span>
   ---|---|---
   `<id>` | <span data-ttu-id="386a2-143">Anger ett unikt namn som Maven använder toolook in dina säkerhetsinställningar när du distribuerar din web app tooAzure.</span><span class="sxs-lookup"><span data-stu-id="386a2-143">Specifies a unique name which Maven uses toolook up your security settings when you deploy your web app tooAzure.</span></span>
   `<client>` | <span data-ttu-id="386a2-144">Innehåller hello `appId` värde från tjänstens huvudnamn.</span><span class="sxs-lookup"><span data-stu-id="386a2-144">Contains hello `appId` value from your service principal.</span></span>
   `<tenant>` | <span data-ttu-id="386a2-145">Innehåller hello `tenant` värde från tjänstens huvudnamn.</span><span class="sxs-lookup"><span data-stu-id="386a2-145">Contains hello `tenant` value from your service principal.</span></span>
   `<key>` | <span data-ttu-id="386a2-146">Innehåller hello `password` värde från tjänstens huvudnamn.</span><span class="sxs-lookup"><span data-stu-id="386a2-146">Contains hello `password` value from your service principal.</span></span>
   `<environment>` | <span data-ttu-id="386a2-147">Definierar hello Azure-molnet målmiljön, vilket är `AZURE` i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="386a2-147">Defines hello target Azure cloud environment, which is `AZURE` in this example.</span></span> <span data-ttu-id="386a2-148">(En fullständig lista över miljöer finns i hello [Maven plugin-program för Azure Web Apps] dokumentation)</span><span class="sxs-lookup"><span data-stu-id="386a2-148">(A full list of environments is available in hello [Maven Plugin for Azure Web Apps] documentation)</span></span>

1. <span data-ttu-id="386a2-149">Spara och Stäng hello *settings.xml* fil.</span><span class="sxs-lookup"><span data-stu-id="386a2-149">Save and close hello *settings.xml* file.</span></span>

## <a name="optional-customize-your-pomxml-before-deploying-your-web-app-tooazure"></a><span data-ttu-id="386a2-150">Valfritt: Anpassa din pom.xml innan du distribuerar din web app tooAzure</span><span class="sxs-lookup"><span data-stu-id="386a2-150">OPTIONAL: Customize your pom.xml before deploying your web app tooAzure</span></span>

<span data-ttu-id="386a2-151">Öppna hello `pom.xml` filen för tillämpningsprogrammet källan Start i en textredigerare och leta sedan upp hello `<plugin>` element för `azure-webapp-maven-plugin`.</span><span class="sxs-lookup"><span data-stu-id="386a2-151">Open hello `pom.xml` file for your Spring Boot application in a text editor, and then locate hello `<plugin>` element for `azure-webapp-maven-plugin`.</span></span> <span data-ttu-id="386a2-152">Det här elementet bör likna följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="386a2-152">This element should resemble hello following example:</span></span>

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
         <appName>maven-web-app-${maven.build.timestamp}</appName>
         <region>westus</region>
         <javaVersion>1.8</javaVersion>
         <deploymentType>ftp</deploymentType>
         <resources>
            <resource>
               <directory>${project.basedir}/target</directory>
               <targetPath>/</targetPath>
               <includes>
                  <include>*.jar</include>
               </includes>
            </resource>
            <resource>
               <directory>${project.basedir}</directory>
               <targetPath>/</targetPath>
               <includes>
                  <include>web.config</include>
               </includes>
            </resource>
         </resources>
      </configuration>
   </plugin>
   ```

<span data-ttu-id="386a2-153">Det finns flera värden som du kan ändra för hello Maven plugin-program och en utförlig beskrivning för var och en av dessa element är tillgänglig i hello [Maven plugin-program för Azure Web Apps] dokumentation.</span><span class="sxs-lookup"><span data-stu-id="386a2-153">There are several values that you can modify for hello Maven plugin, and a detailed description for each of these elements is available in hello [Maven Plugin for Azure Web Apps] documentation.</span></span> <span data-ttu-id="386a2-154">Som som säger att det finns flera värden som är värda markering i den här artikeln:</span><span class="sxs-lookup"><span data-stu-id="386a2-154">That being said, there are several values that are worth highlighting in this article:</span></span>

<span data-ttu-id="386a2-155">Element</span><span class="sxs-lookup"><span data-stu-id="386a2-155">Element</span></span> | <span data-ttu-id="386a2-156">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="386a2-156">Description</span></span>
---|---|---
`<version>` | <span data-ttu-id="386a2-157">Anger hello version av hello [Maven plugin-program för Azure Web Apps].</span><span class="sxs-lookup"><span data-stu-id="386a2-157">Specifies hello version of hello [Maven Plugin for Azure Web Apps].</span></span> <span data-ttu-id="386a2-158">Du bör kontrollera hello-version som anges i hello [Maven centrala lagringsplatsen](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-webapp-maven-plugin%22) tooensure som du använder hello senaste versionen.</span><span class="sxs-lookup"><span data-stu-id="386a2-158">You should check hello version listed in hello [Maven Central Respository](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-webapp-maven-plugin%22) tooensure that you are using hello latest version.</span></span>
`<authentication>` | <span data-ttu-id="386a2-159">Anger hello autentiseringsinformationen för Azure, som i det här exemplet innehåller en `<serverId>` element som innehåller `azure-auth`; Maven använder den värdet toolook hello Azure-tjänstens huvudnamn värden i din Maven *settings.xml* fil som du har definierat i en tidigare i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="386a2-159">Specifies hello authentication information for Azure, which in this example contains a `<serverId>` element that contains `azure-auth`; Maven uses that value toolook up hello Azure service principal values in your Maven *settings.xml* file, which you defined in an earlier section of this article.</span></span>
`<resourceGroup>` | <span data-ttu-id="386a2-160">Anger hello målresursgruppen, vilket är `maven-plugin` i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="386a2-160">Specifies hello target resource group, which is `maven-plugin` in this example.</span></span> <span data-ttu-id="386a2-161">hello resursgruppen skapas under distributionen om det inte redan finns.</span><span class="sxs-lookup"><span data-stu-id="386a2-161">hello resource group is created during deployment if it does not already exist.</span></span>
`<appName>` | <span data-ttu-id="386a2-162">Anger hello målets namn för ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="386a2-162">Specifies hello target name for your web app.</span></span> <span data-ttu-id="386a2-163">I det här exemplet är hello målnamnet `maven-web-app-${maven.build.timestamp}`, där hello `${maven.build.timestamp}` suffix läggs till i det här exemplet tooavoid konflikt.</span><span class="sxs-lookup"><span data-stu-id="386a2-163">In this example, hello target name is `maven-web-app-${maven.build.timestamp}`, where hello `${maven.build.timestamp}` suffix is appended in this example tooavoid conflict.</span></span> <span data-ttu-id="386a2-164">(hello tidsstämpeln är valfria och du kan ange en unik sträng för hello programnamn.)</span><span class="sxs-lookup"><span data-stu-id="386a2-164">(hello timestamp is optional; you can specify any unique string for hello app name.)</span></span>
`<region>` | <span data-ttu-id="386a2-165">Anger hello målregionen, som i det här exemplet är `westus`.</span><span class="sxs-lookup"><span data-stu-id="386a2-165">Specifies hello target region, which in this example is `westus`.</span></span> <span data-ttu-id="386a2-166">(En fullständig lista finns i hello [Maven plugin-program för Azure Web Apps] dokumentation.)</span><span class="sxs-lookup"><span data-stu-id="386a2-166">(A full list is in hello [Maven Plugin for Azure Web Apps] documentation.)</span></span>
`<javaVersion>` | <span data-ttu-id="386a2-167">Anger hello Java runtime version för ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="386a2-167">Specifies hello Java runtime version for your web app.</span></span> <span data-ttu-id="386a2-168">(En fullständig lista finns i hello [Maven plugin-program för Azure Web Apps] dokumentation.)</span><span class="sxs-lookup"><span data-stu-id="386a2-168">(A full list is in hello [Maven Plugin for Azure Web Apps] documentation.)</span></span>
`<deploymentType>` | <span data-ttu-id="386a2-169">Anger typen av distribution för ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="386a2-169">Specifies deployment type for your web app.</span></span> <span data-ttu-id="386a2-170">För närvarande endast `ftp` stöds även om stöd för andra distributionstyper är under utveckling.</span><span class="sxs-lookup"><span data-stu-id="386a2-170">For now, only `ftp` is supported, although support for other deployment types is in development.</span></span>
`<resources>` | <span data-ttu-id="386a2-171">Anger resurser och mål-mål som Maven använder när du distribuerar din web app tooAzure.</span><span class="sxs-lookup"><span data-stu-id="386a2-171">Specifies resources and target destinations which Maven uses when deploying your web app tooAzure.</span></span> <span data-ttu-id="386a2-172">I det här exemplet två `<resource>` element anger att Maven distribuerar hello JAR-filen för ditt webbprogram och hello *web.config* filen från hello källan Start projektet.</span><span class="sxs-lookup"><span data-stu-id="386a2-172">In this example, two `<resource>` elements specify that Maven will deploy hello JAR file for your web app and hello *web.config* file from hello Spring Boot project.</span></span>

## <a name="build-and-deploy-your-web-app-tooazure"></a><span data-ttu-id="386a2-173">Skapa och distribuera din web app tooAzure</span><span class="sxs-lookup"><span data-stu-id="386a2-173">Build and deploy your web app tooAzure</span></span>

<span data-ttu-id="386a2-174">När du har konfigurerat alla hello inställningar i föregående avsnitt i den här artikeln hello, är du redo toodeploy tooAzure dina webb-app.</span><span class="sxs-lookup"><span data-stu-id="386a2-174">Once you have configured all of hello settings in hello preceding sections of this article, you are ready toodeploy your web app tooAzure.</span></span> <span data-ttu-id="386a2-175">toodo så Använd hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="386a2-175">toodo so, use hello following steps:</span></span>

1. <span data-ttu-id="386a2-176">Från Kommandotolken hello eller terminalfönster som du använde tidigare återskapa hello JAR-filen med Maven om du har gjort några ändringar toohello *pom.xml* filen, till exempel:</span><span class="sxs-lookup"><span data-stu-id="386a2-176">From hello command prompt or terminal window that you were using earlier, rebuild hello JAR file using Maven if you made any changes toohello *pom.xml* file; for example:</span></span>
   ```shell
   mvn clean package
   ```

1. <span data-ttu-id="386a2-177">Distribuera din web app tooAzure med Maven; Exempel:</span><span class="sxs-lookup"><span data-stu-id="386a2-177">Deploy your web app tooAzure by using Maven; for example:</span></span>
   ```shell
   mvn azure-webapp:deploy
   ```

<span data-ttu-id="386a2-178">Maven distribuerar din web app tooAzure; Om hello webbapp inte redan finns, skapas.</span><span class="sxs-lookup"><span data-stu-id="386a2-178">Maven will deploy your web app tooAzure; if hello web app does not already exist, it will be created.</span></span>

<span data-ttu-id="386a2-179">När webbplatsen har distribuerats, kommer du att kunna toomanage den med hjälp av hello [Azure-portalen].</span><span class="sxs-lookup"><span data-stu-id="386a2-179">When your web has been deployed, you will be able toomanage it by using hello [Azure portal].</span></span>

* <span data-ttu-id="386a2-180">Ditt webbprogram visas i **Apptjänster**:</span><span class="sxs-lookup"><span data-stu-id="386a2-180">Your web app will be listed in **App Services**:</span></span>

   ![Webbprogram som anges i Azure portal Apptjänster][AP01]

* <span data-ttu-id="386a2-182">Och hello URL för ditt webbprogram visas i hello **översikt** för ditt webbprogram:</span><span class="sxs-lookup"><span data-stu-id="386a2-182">And hello URL for your web app will be listed in hello **Overview** for your web app:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="386a2-184">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="386a2-184">Next steps</span></span>

<span data-ttu-id="386a2-185">Mer information om hello finns olika tekniker som beskrivs i den här artikeln hello följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="386a2-185">For more information about hello various technologies discussed in this article, see hello following articles:</span></span>

* <span data-ttu-id="386a2-186">[Maven plugin-program för Azure Web Apps]</span><span class="sxs-lookup"><span data-stu-id="386a2-186">[Maven Plugin for Azure Web Apps]</span></span>

* [<span data-ttu-id="386a2-187">Logga in tooAzure från hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="386a2-187">Log in tooAzure from hello Azure CLI</span></span>](/azure/xplat-cli-connect)

* [<span data-ttu-id="386a2-188">Hur toouse hello Maven-plugin-programmet för Azure Web Apps toodeploy en av vår Start app tooAzure</span><span class="sxs-lookup"><span data-stu-id="386a2-188">How toouse hello Maven Plugin for Azure Web Apps toodeploy a containerized Spring Boot app tooAzure</span></span>](app-service-web-deploy-containerized-spring-boot-app-with-maven-plugin.md)

* [<span data-ttu-id="386a2-189">Skapa en Azure tjänstens huvudnamn med Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="386a2-189">Create an Azure service principal with Azure CLI 2.0</span></span>](/cli/azure/create-an-azure-service-principal-azure-cli)

* [<span data-ttu-id="386a2-190">Referens för maven</span><span class="sxs-lookup"><span data-stu-id="386a2-190">Maven Settings Reference</span></span>](https://maven.apache.org/settings.html)

<!-- URL List -->

[Azure-kommandoradsgränssnittet (CLI)]: /cli/azure/overview
[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[Azure-portalen]: https://portal.azure.com/
[kostnadsfritt Azure-konto]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[Maven]: http://maven.apache.org/
[MSDN-prenumerantförmåner]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Vårversionen Start komma igång]: https://github.com/microsoft/gs-spring-boot
[Spring Framework]: https://spring.io/
[Maven plugin-program för Azure Web Apps]: https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin

<!-- IMG List -->

[AP01]: ./media/app-service-web-deploy-spring-boot-app-with-maven-plugin/AP01.png
[AP02]: ./media/app-service-web-deploy-spring-boot-app-with-maven-plugin/AP02.png
