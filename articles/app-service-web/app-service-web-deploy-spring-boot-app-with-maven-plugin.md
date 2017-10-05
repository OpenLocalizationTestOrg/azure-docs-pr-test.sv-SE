---
title: "Hur du använder Maven-plugin-program för Azure Web Apps för att distribuera en start av vår app till Azure"
description: "Lär dig hur du använder Maven-plugin-program för Azure Web Apps för att distribuera en start av vår app till Azure."
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
ms.openlocfilehash: dceb7edf788bd87b1de04aa435a12cd5853755b9
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-use-the-maven-plugin-for-azure-web-apps-to-deploy-a-spring-boot-app-to-azure"></a><span data-ttu-id="4f24b-103">Hur du använder Maven-plugin-program för Azure Web Apps för att distribuera en start av vår app till Azure</span><span class="sxs-lookup"><span data-stu-id="4f24b-103">How to use the Maven Plugin for Azure Web Apps to deploy a Spring Boot app to Azure</span></span>

<span data-ttu-id="4f24b-104">Den [Maven plugin-program för Azure-Webbappar](https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin) för [Apache Maven](http://maven.apache.org/) ger sömlös integration av Azure App Service till Maven-projekt och förenklar processen för utvecklare att distribuera webbappar till Azure App Tjänsten.</span><span class="sxs-lookup"><span data-stu-id="4f24b-104">The [Maven Plugin for Azure Web Apps](https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin) for [Apache Maven](http://maven.apache.org/) provides seamless integration of Azure App Service into Maven projects, and streamlines the process for developers to deploy web apps to Azure App Service.</span></span>

<span data-ttu-id="4f24b-105">Den här artikeln visas hur du använder Maven-plugin-program för Azure Web Apps för att distribuera ett exempelprogram källan Start till Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="4f24b-105">This article demonstrates using the Maven Plugin for Azure Web Apps to deploy a sample Spring Boot application to Azure App Services.</span></span>

> [!NOTE]
>
> <span data-ttu-id="4f24b-106">Maven-plugin-program för Azure Web Apps är tillgänglig som en förhandsgranskning.</span><span class="sxs-lookup"><span data-stu-id="4f24b-106">The Maven Plugin for Azure Web Apps is currently available as a preview.</span></span> <span data-ttu-id="4f24b-107">För närvarande stöds endast FTP-publicering, även om ytterligare funktioner som är tänkta för framtiden.</span><span class="sxs-lookup"><span data-stu-id="4f24b-107">For now, only FTP publishing is supported, although additional features are planned for the future.</span></span>
>

## <a name="prerequisites"></a><span data-ttu-id="4f24b-108">Krav</span><span class="sxs-lookup"><span data-stu-id="4f24b-108">Prerequisites</span></span>

<span data-ttu-id="4f24b-109">För att kunna slutföra stegen i den här självstudiekursen, måste du ha följande krav:</span><span class="sxs-lookup"><span data-stu-id="4f24b-109">In order to complete the steps in this tutorial, you need to have the following prerequisites:</span></span>

* <span data-ttu-id="4f24b-110">En Azure-prenumeration; Om du inte redan har en Azure-prenumeration, kan du aktivera din [MSDN-prenumerantförmåner] eller registrera dig för en [kostnadsfritt Azure-konto].</span><span class="sxs-lookup"><span data-stu-id="4f24b-110">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="4f24b-111">Den [Azure-kommandoradsgränssnittet (CLI)].</span><span class="sxs-lookup"><span data-stu-id="4f24b-111">The [Azure Command-Line Interface (CLI)].</span></span>
* <span data-ttu-id="4f24b-112">En uppdaterad [Java Development Kit (JDK)], version 1.7 eller senare.</span><span class="sxs-lookup"><span data-stu-id="4f24b-112">An up-to-date [Java Development Kit (JDK)], version 1.7 or later.</span></span>
* <span data-ttu-id="4f24b-113">Apache's [Maven] skapa verktyget (Version 3).</span><span class="sxs-lookup"><span data-stu-id="4f24b-113">Apache's [Maven] build tool (Version 3).</span></span>
* <span data-ttu-id="4f24b-114">En [Git] klienten.</span><span class="sxs-lookup"><span data-stu-id="4f24b-114">A [Git] client.</span></span>

## <a name="clone-the-sample-spring-boot-web-app"></a><span data-ttu-id="4f24b-115">Klona exempelwebbapp för vår Start</span><span class="sxs-lookup"><span data-stu-id="4f24b-115">Clone the sample Spring Boot web app</span></span>

<span data-ttu-id="4f24b-116">I det här avsnittet klona en slutförd startprogrammet för källan och testa den lokalt.</span><span class="sxs-lookup"><span data-stu-id="4f24b-116">In this section, you clone a completed Spring Boot application and test it locally.</span></span>

1. <span data-ttu-id="4f24b-117">Öppna en kommandotolk eller ett terminalfönster och skapa en lokal katalog för att hålla tillämpningsprogrammet källan start och ändra till katalogen; Exempel:</span><span class="sxs-lookup"><span data-stu-id="4f24b-117">Open a command prompt or terminal window and create a local directory to hold your Spring Boot application, and change to that directory; for example:</span></span>
   ```shell
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   <span data-ttu-id="4f24b-118">-- eller--</span><span class="sxs-lookup"><span data-stu-id="4f24b-118">-- or --</span></span>
   ```shell
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. <span data-ttu-id="4f24b-119">Klona den [Vårversionen Start komma igång] exempelprojektet till den katalog som du har skapat, till exempel:</span><span class="sxs-lookup"><span data-stu-id="4f24b-119">Clone the [Spring Boot Getting Started] sample project into the directory you created; for example:</span></span>
   ```shell
   git clone https://github.com/microsoft/gs-spring-boot
   ```

1. <span data-ttu-id="4f24b-120">Ändra katalogen till slutförda projektet. Exempel:</span><span class="sxs-lookup"><span data-stu-id="4f24b-120">Change directory to the completed project; for example:</span></span>
   ```shell
   cd gs-spring-boot/complete
   ```

1. <span data-ttu-id="4f24b-121">Skapa JAR-filen med Maven; Exempel:</span><span class="sxs-lookup"><span data-stu-id="4f24b-121">Build the JAR file using Maven; for example:</span></span>
   ```shell
   mvn clean package
   ```

1. <span data-ttu-id="4f24b-122">När webbappen har skapats, starta webbprogram med Maven; Exempel:</span><span class="sxs-lookup"><span data-stu-id="4f24b-122">When the web app has been created, start the web app using Maven; for example:</span></span>
   ```shell
   mvn spring-boot:run
   ```

1. <span data-ttu-id="4f24b-123">Testa webbappen genom att bläddra till den lokalt med hjälp av en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="4f24b-123">Test the web app by browsing to it locally using a web browser.</span></span> <span data-ttu-id="4f24b-124">Du kan exempelvis använda följande kommando om du har curl som är tillgängliga:</span><span class="sxs-lookup"><span data-stu-id="4f24b-124">For example, you could use the following command if you have curl available:</span></span>
   ```shell
   curl http://localhost:8080
   ```

1. <span data-ttu-id="4f24b-125">Du bör se följande meddelande visas: **helg från källan Start!**</span><span class="sxs-lookup"><span data-stu-id="4f24b-125">You should see the following message displayed: **Greetings from Spring Boot!**</span></span>

## <a name="create-an-azure-service-principal"></a><span data-ttu-id="4f24b-126">Skapa en Azure huvudnamn för tjänsten</span><span class="sxs-lookup"><span data-stu-id="4f24b-126">Create an Azure service principal</span></span>

<span data-ttu-id="4f24b-127">I det här avsnittet skapar du en Azure tjänstens huvudnamn som Maven plugin-programmet använder när du distribuerar ditt webbprogram till Azure.</span><span class="sxs-lookup"><span data-stu-id="4f24b-127">In this section, you create an Azure service principal that the Maven plugin uses when deploying your web app to Azure.</span></span>

1. <span data-ttu-id="4f24b-128">Öppna en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="4f24b-128">Open a command prompt.</span></span>

1. <span data-ttu-id="4f24b-129">Logga in på ditt Azure-konto med hjälp av Azure CLI:</span><span class="sxs-lookup"><span data-stu-id="4f24b-129">Sign into your Azure account by using the Azure CLI:</span></span>
   ```shell
   az login
   ```
   <span data-ttu-id="4f24b-130">Följ instruktionerna för att slutföra inloggningen.</span><span class="sxs-lookup"><span data-stu-id="4f24b-130">Follow the instructions to complete the sign-in process.</span></span>

1. <span data-ttu-id="4f24b-131">Skapa en Azure-tjänstens huvudnamn:</span><span class="sxs-lookup"><span data-stu-id="4f24b-131">Create an Azure service principal:</span></span>
   ```shell
   az ad sp create-for-rbac --name "uuuuuuuu" --password "pppppppp"
   ```
   <span data-ttu-id="4f24b-132">Där `uuuuuuuu` är användarnamnet och `pppppppp` är lösenordet för tjänstens huvudnamn.</span><span class="sxs-lookup"><span data-stu-id="4f24b-132">Where `uuuuuuuu` is the user name and `pppppppp` is the password for the service principal.</span></span>

1. <span data-ttu-id="4f24b-133">Azure svarar med JSON som liknar följande exempel:</span><span class="sxs-lookup"><span data-stu-id="4f24b-133">Azure responds with JSON that resembles the following example:</span></span>
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
   > <span data-ttu-id="4f24b-134">Du kommer använda värden från den här JSON-svar när du konfigurerar Maven-plugin-programmet för att distribuera webbappen till Azure.</span><span class="sxs-lookup"><span data-stu-id="4f24b-134">You will use the values from this JSON response when you configure the Maven plugin to deploy your web app to Azure.</span></span> <span data-ttu-id="4f24b-135">Den `aaaaaaaa`, `uuuuuuuu`, `pppppppp`, och `tttttttt` är för platshållarvärden som används i det här exemplet för att göra det enklare att mappa värdena till deras respektive element när du konfigurerar din Maven `settings.xml` filen i nästa avsnitt .</span><span class="sxs-lookup"><span data-stu-id="4f24b-135">The `aaaaaaaa`, `uuuuuuuu`, `pppppppp`, and `tttttttt` are placeholder values, which are used in this example to make it easier to map these values to their respective elements when you configure your Maven `settings.xml` file in the next section.</span></span>
   >
   >

## <a name="configure-maven-to-use-your-azure-service-principal"></a><span data-ttu-id="4f24b-136">Konfigurera Maven för att använda din Azure-tjänstens huvudnamn</span><span class="sxs-lookup"><span data-stu-id="4f24b-136">Configure Maven to use your Azure service principal</span></span>

<span data-ttu-id="4f24b-137">I det här avsnittet använder du värdena från din Azure tjänstens huvudnamn så här konfigurerar du autentisering med Maven när distribuera webbappen till Azure.</span><span class="sxs-lookup"><span data-stu-id="4f24b-137">In this section, you use the values from your Azure service principal to configure the authentication that Maven uses when deploying your web app to Azure.</span></span>

1. <span data-ttu-id="4f24b-138">Öppna din Maven `settings.xml` filen i en textredigerare; den här filen kan vara en sökväg som i följande exempel:</span><span class="sxs-lookup"><span data-stu-id="4f24b-138">Open your Maven `settings.xml` file in a text editor; this file might be in a path like the following examples:</span></span>
   * `/etc/maven/settings.xml`
   * `%ProgramFiles%\apache-maven\3.5.0\conf\settings.xml`
   * `$HOME/.m2/settings.xml`

1. <span data-ttu-id="4f24b-139">Lägg till Azure-tjänstens huvudnamn inställningarna från föregående avsnitt i den här kursen och den `<servers>` samling i den *settings.xml* filen, till exempel:</span><span class="sxs-lookup"><span data-stu-id="4f24b-139">Add your Azure service principal settings from the previous section of this tutorial to the `<servers>` collection in the *settings.xml* file; for example:</span></span>

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
   <span data-ttu-id="4f24b-140">Där:</span><span class="sxs-lookup"><span data-stu-id="4f24b-140">Where:</span></span>
   <span data-ttu-id="4f24b-141">Element</span><span class="sxs-lookup"><span data-stu-id="4f24b-141">Element</span></span> | <span data-ttu-id="4f24b-142">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="4f24b-142">Description</span></span>
   ---|---|---
   `<id>` | <span data-ttu-id="4f24b-143">Anger ett unikt namn som Maven använder för att leta upp dina säkerhetsinställningar när du distribuerar ditt webbprogram till Azure.</span><span class="sxs-lookup"><span data-stu-id="4f24b-143">Specifies a unique name which Maven uses to look up your security settings when you deploy your web app to Azure.</span></span>
   `<client>` | <span data-ttu-id="4f24b-144">Innehåller den `appId` värde från tjänstens huvudnamn.</span><span class="sxs-lookup"><span data-stu-id="4f24b-144">Contains the `appId` value from your service principal.</span></span>
   `<tenant>` | <span data-ttu-id="4f24b-145">Innehåller den `tenant` värde från tjänstens huvudnamn.</span><span class="sxs-lookup"><span data-stu-id="4f24b-145">Contains the `tenant` value from your service principal.</span></span>
   `<key>` | <span data-ttu-id="4f24b-146">Innehåller den `password` värde från tjänstens huvudnamn.</span><span class="sxs-lookup"><span data-stu-id="4f24b-146">Contains the `password` value from your service principal.</span></span>
   `<environment>` | <span data-ttu-id="4f24b-147">Definierar målmiljön Azure-molnet, vilket är `AZURE` i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="4f24b-147">Defines the target Azure cloud environment, which is `AZURE` in this example.</span></span> <span data-ttu-id="4f24b-148">(En fullständig lista över miljöer finns i den [Maven plugin-program för Azure Web Apps] dokumentation)</span><span class="sxs-lookup"><span data-stu-id="4f24b-148">(A full list of environments is available in the [Maven Plugin for Azure Web Apps] documentation)</span></span>

1. <span data-ttu-id="4f24b-149">Spara och Stäng den *settings.xml* fil.</span><span class="sxs-lookup"><span data-stu-id="4f24b-149">Save and close the *settings.xml* file.</span></span>

## <a name="optional-customize-your-pomxml-before-deploying-your-web-app-to-azure"></a><span data-ttu-id="4f24b-150">Valfritt: Anpassa din pom.xml innan du distribuerar ditt webbprogram till Azure</span><span class="sxs-lookup"><span data-stu-id="4f24b-150">OPTIONAL: Customize your pom.xml before deploying your web app to Azure</span></span>

<span data-ttu-id="4f24b-151">Öppna den `pom.xml` filen för tillämpningsprogrammet källan Start i en textredigerare och leta upp den `<plugin>` element för `azure-webapp-maven-plugin`.</span><span class="sxs-lookup"><span data-stu-id="4f24b-151">Open the `pom.xml` file for your Spring Boot application in a text editor, and then locate the `<plugin>` element for `azure-webapp-maven-plugin`.</span></span> <span data-ttu-id="4f24b-152">Det här elementet bör likna följande exempel:</span><span class="sxs-lookup"><span data-stu-id="4f24b-152">This element should resemble the following example:</span></span>

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

<span data-ttu-id="4f24b-153">Det finns flera värden som du kan ändra för Maven plugin-programmet och en utförlig beskrivning för var och en av dessa element är tillgänglig i den [Maven plugin-program för Azure Web Apps] dokumentation.</span><span class="sxs-lookup"><span data-stu-id="4f24b-153">There are several values that you can modify for the Maven plugin, and a detailed description for each of these elements is available in the [Maven Plugin for Azure Web Apps] documentation.</span></span> <span data-ttu-id="4f24b-154">Som som säger att det finns flera värden som är värda markering i den här artikeln:</span><span class="sxs-lookup"><span data-stu-id="4f24b-154">That being said, there are several values that are worth highlighting in this article:</span></span>

<span data-ttu-id="4f24b-155">Element</span><span class="sxs-lookup"><span data-stu-id="4f24b-155">Element</span></span> | <span data-ttu-id="4f24b-156">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="4f24b-156">Description</span></span>
---|---|---
`<version>` | <span data-ttu-id="4f24b-157">Anger versionen av den [Maven plugin-program för Azure Web Apps].</span><span class="sxs-lookup"><span data-stu-id="4f24b-157">Specifies the version of the [Maven Plugin for Azure Web Apps].</span></span> <span data-ttu-id="4f24b-158">Du bör kontrollera versionen som anges i den [Maven centrala lagringsplatsen](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-webapp-maven-plugin%22) så att du använder den senaste versionen.</span><span class="sxs-lookup"><span data-stu-id="4f24b-158">You should check the version listed in the [Maven Central Respository](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-webapp-maven-plugin%22) to ensure that you are using the latest version.</span></span>
`<authentication>` | <span data-ttu-id="4f24b-159">Anger autentiseringsinformationen för Azure, som i det här exemplet innehåller en `<serverId>` element som innehåller `azure-auth`; Maven används värdet för att slå upp Azure tjänstens huvudnamn värden i din Maven *settings.xml* fil som du har definierat i en tidigare i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="4f24b-159">Specifies the authentication information for Azure, which in this example contains a `<serverId>` element that contains `azure-auth`; Maven uses that value to look up the Azure service principal values in your Maven *settings.xml* file, which you defined in an earlier section of this article.</span></span>
`<resourceGroup>` | <span data-ttu-id="4f24b-160">Anger målresursgruppen som är `maven-plugin` i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="4f24b-160">Specifies the target resource group, which is `maven-plugin` in this example.</span></span> <span data-ttu-id="4f24b-161">Resursgruppens namn skapas under distributionen om det inte redan finns.</span><span class="sxs-lookup"><span data-stu-id="4f24b-161">The resource group is created during deployment if it does not already exist.</span></span>
`<appName>` | <span data-ttu-id="4f24b-162">Anger målnamn för ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="4f24b-162">Specifies the target name for your web app.</span></span> <span data-ttu-id="4f24b-163">I det här exemplet är målnamnet `maven-web-app-${maven.build.timestamp}`, där den `${maven.build.timestamp}` suffix läggs till i det här exemplet för att undvika konflikter.</span><span class="sxs-lookup"><span data-stu-id="4f24b-163">In this example, the target name is `maven-web-app-${maven.build.timestamp}`, where the `${maven.build.timestamp}` suffix is appended in this example to avoid conflict.</span></span> <span data-ttu-id="4f24b-164">(Tidsstämpeln är valfria och du kan ange en unik sträng för namnet på appen.)</span><span class="sxs-lookup"><span data-stu-id="4f24b-164">(The timestamp is optional; you can specify any unique string for the app name.)</span></span>
`<region>` | <span data-ttu-id="4f24b-165">Anger målregionen, som i det här exemplet är `westus`.</span><span class="sxs-lookup"><span data-stu-id="4f24b-165">Specifies the target region, which in this example is `westus`.</span></span> <span data-ttu-id="4f24b-166">(En fullständig lista finns i den [Maven plugin-program för Azure Web Apps] dokumentation.)</span><span class="sxs-lookup"><span data-stu-id="4f24b-166">(A full list is in the [Maven Plugin for Azure Web Apps] documentation.)</span></span>
`<javaVersion>` | <span data-ttu-id="4f24b-167">Anger vilken Java runtime version för ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="4f24b-167">Specifies the Java runtime version for your web app.</span></span> <span data-ttu-id="4f24b-168">(En fullständig lista finns i den [Maven plugin-program för Azure Web Apps] dokumentation.)</span><span class="sxs-lookup"><span data-stu-id="4f24b-168">(A full list is in the [Maven Plugin for Azure Web Apps] documentation.)</span></span>
`<deploymentType>` | <span data-ttu-id="4f24b-169">Anger typen av distribution för ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="4f24b-169">Specifies deployment type for your web app.</span></span> <span data-ttu-id="4f24b-170">För närvarande endast `ftp` stöds även om stöd för andra distributionstyper är under utveckling.</span><span class="sxs-lookup"><span data-stu-id="4f24b-170">For now, only `ftp` is supported, although support for other deployment types is in development.</span></span>
`<resources>` | <span data-ttu-id="4f24b-171">Anger resurser och mål-mål som Maven använder när du distribuerar ditt webbprogram till Azure.</span><span class="sxs-lookup"><span data-stu-id="4f24b-171">Specifies resources and target destinations which Maven uses when deploying your web app to Azure.</span></span> <span data-ttu-id="4f24b-172">I det här exemplet två `<resource>` element anger att Maven distribuerar JAR-filen för ditt webbprogram och *web.config* filen från källan Start-projektet.</span><span class="sxs-lookup"><span data-stu-id="4f24b-172">In this example, two `<resource>` elements specify that Maven will deploy the JAR file for your web app and the *web.config* file from the Spring Boot project.</span></span>

## <a name="build-and-deploy-your-web-app-to-azure"></a><span data-ttu-id="4f24b-173">Skapa och distribuera webbappen till Azure</span><span class="sxs-lookup"><span data-stu-id="4f24b-173">Build and deploy your web app to Azure</span></span>

<span data-ttu-id="4f24b-174">När du har konfigurerat alla inställningar i föregående avsnitt i den här artikeln, är du redo att distribuera webbappen till Azure.</span><span class="sxs-lookup"><span data-stu-id="4f24b-174">Once you have configured all of the settings in the preceding sections of this article, you are ready to deploy your web app to Azure.</span></span> <span data-ttu-id="4f24b-175">Så här loggar du in:</span><span class="sxs-lookup"><span data-stu-id="4f24b-175">To do so, use the following steps:</span></span>

1. <span data-ttu-id="4f24b-176">Från Kommandotolken eller terminalfönster som du använde tidigare, återskapa JAR-filen med Maven om du har gjort ändringar i *pom.xml* filen, till exempel:</span><span class="sxs-lookup"><span data-stu-id="4f24b-176">From the command prompt or terminal window that you were using earlier, rebuild the JAR file using Maven if you made any changes to the *pom.xml* file; for example:</span></span>
   ```shell
   mvn clean package
   ```

1. <span data-ttu-id="4f24b-177">Distribuera webbappen till Azure med hjälp av Maven; Exempel:</span><span class="sxs-lookup"><span data-stu-id="4f24b-177">Deploy your web app to Azure by using Maven; for example:</span></span>
   ```shell
   mvn azure-webapp:deploy
   ```

<span data-ttu-id="4f24b-178">Maven ska distribuera webbappen till Azure; Om webbappen inte redan finns, skapas.</span><span class="sxs-lookup"><span data-stu-id="4f24b-178">Maven will deploy your web app to Azure; if the web app does not already exist, it will be created.</span></span>

<span data-ttu-id="4f24b-179">När webbplatsen har distribuerats, kommer du att kunna hantera den med hjälp av den [Azure-portalen].</span><span class="sxs-lookup"><span data-stu-id="4f24b-179">When your web has been deployed, you will be able to manage it by using the [Azure portal].</span></span>

* <span data-ttu-id="4f24b-180">Ditt webbprogram visas i **Apptjänster**:</span><span class="sxs-lookup"><span data-stu-id="4f24b-180">Your web app will be listed in **App Services**:</span></span>

   ![Webbprogram som anges i Azure portal Apptjänster][AP01]

* <span data-ttu-id="4f24b-182">Och URL: en för ditt webbprogram kommer att visas i den **översikt** för ditt webbprogram:</span><span class="sxs-lookup"><span data-stu-id="4f24b-182">And the URL for your web app will be listed in the **Overview** for your web app:</span></span>

   ![Bestämma URL: en för webbappen][AP02]

<!--
##  OPTIONAL: Configure the embedded Tomcat server to run on a different port

The embedded Tomcat server in the sample Spring Boot application is configured to run on port 8080 by default. However, if you want to run the embedded Tomcat server to run on a different port, such as port 80 for local testing, you can configure the port by using the following steps.

1. Go to the *resources* directory (or create the directory if it does not exist); for example:
   ```shell
   cd src/main/resources
   ```

1. Open the *application.yml* file in a text editor if it exists, or create a new YAML file if it does not exist.

1. Modify the **server** setting so that the server runs on port 80; for example:
   ```yaml
   server:
      port: 80
   ```

1. Save and close the *application.yml* file.
-->

## <a name="next-steps"></a><span data-ttu-id="4f24b-184">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4f24b-184">Next steps</span></span>

<span data-ttu-id="4f24b-185">Mer information om de olika tekniker som beskrivs i den här artikeln finns i följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="4f24b-185">For more information about the various technologies discussed in this article, see the following articles:</span></span>

* <span data-ttu-id="4f24b-186">[Maven plugin-program för Azure Web Apps]</span><span class="sxs-lookup"><span data-stu-id="4f24b-186">[Maven Plugin for Azure Web Apps]</span></span>

* [<span data-ttu-id="4f24b-187">Logga in till Azure från Azure CLI</span><span class="sxs-lookup"><span data-stu-id="4f24b-187">Log in to Azure from the Azure CLI</span></span>](/azure/xplat-cli-connect)

* [<span data-ttu-id="4f24b-188">Hur du använder Maven-plugin-program för Azure Web Apps för att distribuera en behållare källan Start-app till Azure</span><span class="sxs-lookup"><span data-stu-id="4f24b-188">How to use the Maven Plugin for Azure Web Apps to deploy a containerized Spring Boot app to Azure</span></span>](app-service-web-deploy-containerized-spring-boot-app-with-maven-plugin.md)

* [<span data-ttu-id="4f24b-189">Skapa en Azure tjänstens huvudnamn med Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="4f24b-189">Create an Azure service principal with Azure CLI 2.0</span></span>](/cli/azure/create-an-azure-service-principal-azure-cli)

* [<span data-ttu-id="4f24b-190">Referens för maven</span><span class="sxs-lookup"><span data-stu-id="4f24b-190">Maven Settings Reference</span></span>](https://maven.apache.org/settings.html)

<!-- URL List -->

<span data-ttu-id="4f24b-191">[Azure-kommandoradsgränssnittet (CLI)]: /cli/azure/overview</span><span class="sxs-lookup"><span data-stu-id="4f24b-191">[Azure Command-Line Interface (CLI)]: /cli/azure/overview</span></span>
[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
<span data-ttu-id="4f24b-192">[Azure-portalen]: https://portal.azure.com/</span><span class="sxs-lookup"><span data-stu-id="4f24b-192">[Azure portal]: https://portal.azure.com/</span></span>
<span data-ttu-id="4f24b-193">[kostnadsfritt Azure-konto]: https://azure.microsoft.com/pricing/free-trial/</span><span class="sxs-lookup"><span data-stu-id="4f24b-193">[free Azure account]: https://azure.microsoft.com/pricing/free-trial/</span></span>
<span data-ttu-id="4f24b-194">[Git]: https://github.com/</span><span class="sxs-lookup"><span data-stu-id="4f24b-194">[Git]: https://github.com/</span></span>
[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
<span data-ttu-id="4f24b-195">[Maven]: http://maven.apache.org/</span><span class="sxs-lookup"><span data-stu-id="4f24b-195">[Maven]: http://maven.apache.org/</span></span>
<span data-ttu-id="4f24b-196">[MSDN-prenumerantförmåner]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/</span><span class="sxs-lookup"><span data-stu-id="4f24b-196">[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/</span></span>
[Spring Boot]: http://projects.spring.io/spring-boot/
<span data-ttu-id="4f24b-197">[Vårversionen Start komma igång]: https://github.com/microsoft/gs-spring-boot</span><span class="sxs-lookup"><span data-stu-id="4f24b-197">[Spring Boot Getting Started]: https://github.com/microsoft/gs-spring-boot</span></span>
[Spring Framework]: https://spring.io/
<span data-ttu-id="4f24b-198">[Maven plugin-program för Azure Web Apps]: https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin</span><span class="sxs-lookup"><span data-stu-id="4f24b-198">[Maven Plugin for Azure Web Apps]: https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin</span></span>

<!-- IMG List -->

[AP01]: ./media/app-service-web-deploy-spring-boot-app-with-maven-plugin/AP01.png
[AP02]: ./media/app-service-web-deploy-spring-boot-app-with-maven-plugin/AP02.png
