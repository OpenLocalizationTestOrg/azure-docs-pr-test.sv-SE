---
title: "Hur du använder Maven-plugin-program för Azure Web Apps för att distribuera en behållare källan Start-app till Azure"
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
ms.openlocfilehash: 883040590291cee94daa227fbc6715ad4be0b393
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-use-the-maven-plugin-for-azure-web-apps-to-deploy-a-containerized-spring-boot-app-to-azure"></a><span data-ttu-id="91053-103">Hur du använder Maven-plugin-program för Azure Web Apps för att distribuera en behållare källan Start-app till Azure</span><span class="sxs-lookup"><span data-stu-id="91053-103">How to use the Maven Plugin for Azure Web Apps to deploy a containerized Spring Boot app to Azure</span></span>

<span data-ttu-id="91053-104">Den [Maven plugin-program för Azure-Webbappar](https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin) för [Apache Maven](http://maven.apache.org/) ger sömlös integration av Azure App Service till Maven-projekt och förenklar processen för utvecklare att distribuera webbappar till Azure App Tjänsten.</span><span class="sxs-lookup"><span data-stu-id="91053-104">The [Maven Plugin for Azure Web Apps](https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin) for [Apache Maven](http://maven.apache.org/) provides seamless integration of Azure App Service  into Maven projects, and streamlines the process for developers to deploy web apps to Azure App Service .</span></span>

<span data-ttu-id="91053-105">Den här artikeln visas hur du använder Maven-plugin-program för Azure Web Apps för att distribuera ett exempelprogram källan Start i en dockerbehållare till Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="91053-105">This article demonstrates using the Maven Plugin for Azure Web Apps to deploy a sample Spring Boot application in a Docker container to Azure App Services.</span></span>

> [!NOTE]
>
> <span data-ttu-id="91053-106">Maven-plugin-program för Azure Web Apps är tillgänglig som en förhandsgranskning.</span><span class="sxs-lookup"><span data-stu-id="91053-106">The Maven Plugin for Azure Web Apps is currently available as a preview.</span></span> <span data-ttu-id="91053-107">För närvarande stöds endast FTP-publicering, även om ytterligare funktioner som är tänkta för framtiden.</span><span class="sxs-lookup"><span data-stu-id="91053-107">For now, only FTP publishing is supported, although additional features are planned for the future.</span></span>
>

## <a name="prerequisites"></a><span data-ttu-id="91053-108">Krav</span><span class="sxs-lookup"><span data-stu-id="91053-108">Prerequisites</span></span>

<span data-ttu-id="91053-109">För att kunna slutföra stegen i den här självstudiekursen, måste du ha följande krav:</span><span class="sxs-lookup"><span data-stu-id="91053-109">In order to complete the steps in this tutorial, you need to have the following prerequisites:</span></span>

* <span data-ttu-id="91053-110">En Azure-prenumeration; Om du inte redan har en Azure-prenumeration, kan du aktivera din [MSDN-prenumerantförmåner] eller registrera dig för en [kostnadsfritt Azure-konto].</span><span class="sxs-lookup"><span data-stu-id="91053-110">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="91053-111">Den [Azure-kommandoradsgränssnittet (CLI)].</span><span class="sxs-lookup"><span data-stu-id="91053-111">The [Azure Command-Line Interface (CLI)].</span></span>
* <span data-ttu-id="91053-112">En uppdaterad [Java Development Kit (JDK)], version 1.7 eller senare.</span><span class="sxs-lookup"><span data-stu-id="91053-112">An up-to-date [Java Development Kit (JDK)], version 1.7 or later.</span></span>
* <span data-ttu-id="91053-113">Apache's [Maven] skapa verktyget (Version 3).</span><span class="sxs-lookup"><span data-stu-id="91053-113">Apache's [Maven] build tool (Version 3).</span></span>
* <span data-ttu-id="91053-114">En [Git] klienten.</span><span class="sxs-lookup"><span data-stu-id="91053-114">A [Git] client.</span></span>
* <span data-ttu-id="91053-115">En [Docker] klienten.</span><span class="sxs-lookup"><span data-stu-id="91053-115">A [Docker] client.</span></span>

> [!NOTE]
>
> <span data-ttu-id="91053-116">På grund av virtualisering kraven i den här självstudiekursen, kan du följa stegen i den här artikeln på en virtuell dator; Du måste använda en fysisk dator med aktiverat funktionerna för virtualisering.</span><span class="sxs-lookup"><span data-stu-id="91053-116">Due to the virtualization requirements of this tutorial, you cannot follow the steps in this article on a virtual machine; you must use a physical computer with virtualization features enabled.</span></span>
>

## <a name="clone-the-sample-spring-boot-on-docker-web-app"></a><span data-ttu-id="91053-117">Klona exempel källan Start på Docker-webbprogram</span><span class="sxs-lookup"><span data-stu-id="91053-117">Clone the sample Spring Boot on Docker web app</span></span>

<span data-ttu-id="91053-118">I det här avsnittet klona en container startprogrammet för källan och testa den lokalt.</span><span class="sxs-lookup"><span data-stu-id="91053-118">In this section, you clone a containerized Spring Boot application and test it locally.</span></span>

1. <span data-ttu-id="91053-119">Öppna en kommandotolk eller ett terminalfönster och skapa en lokal katalog för att hålla tillämpningsprogrammet källan start och ändra till katalogen; Exempel:</span><span class="sxs-lookup"><span data-stu-id="91053-119">Open a command prompt or terminal window and create a local directory to hold your Spring Boot application, and change to that directory; for example:</span></span>
   ```shell
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   <span data-ttu-id="91053-120">-- eller--</span><span class="sxs-lookup"><span data-stu-id="91053-120">-- or --</span></span>
   ```shell
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. <span data-ttu-id="91053-121">Klona den [Vårversionen Start på Docker komma igång] exempelprojektet till den katalog som du har skapat, till exempel:</span><span class="sxs-lookup"><span data-stu-id="91053-121">Clone the [Spring Boot on Docker Getting Started] sample project into the directory you created; for example:</span></span>
   ```shell
   git clone https://github.com/microsoft/gs-spring-boot-docker
   ```

1. <span data-ttu-id="91053-122">Ändra katalogen till slutförda projektet. Exempel:</span><span class="sxs-lookup"><span data-stu-id="91053-122">Change directory to the completed project; for example:</span></span>
   ```shell
   cd gs-spring-boot-docker/complete
   ```

1. <span data-ttu-id="91053-123">Skapa JAR-filen med Maven; Exempel:</span><span class="sxs-lookup"><span data-stu-id="91053-123">Build the JAR file using Maven; for example:</span></span>
   ```shell
   mvn clean package
   ```

1. <span data-ttu-id="91053-124">När webbappen har skapats, starta webbprogram med Maven; Exempel:</span><span class="sxs-lookup"><span data-stu-id="91053-124">When the web app has been created, start the web app using Maven; for example:</span></span>
   ```shell
   mvn spring-boot:run
   ```

1. <span data-ttu-id="91053-125">Testa webbappen genom att bläddra till den lokalt med hjälp av en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="91053-125">Test the web app by browsing to it locally using a web browser.</span></span> <span data-ttu-id="91053-126">Du kan exempelvis använda följande kommando om du har curl som är tillgängliga:</span><span class="sxs-lookup"><span data-stu-id="91053-126">For example, you could use the following command if you have curl available:</span></span>
   ```shell
   curl http://localhost:8080
   ```

1. <span data-ttu-id="91053-127">Du bör se följande meddelande visas: **Hello Docker World**</span><span class="sxs-lookup"><span data-stu-id="91053-127">You should see the following message displayed: **Hello Docker World**</span></span>

## <a name="create-an-azure-service-principal"></a><span data-ttu-id="91053-128">Skapa en Azure huvudnamn för tjänsten</span><span class="sxs-lookup"><span data-stu-id="91053-128">Create an Azure service principal</span></span>

<span data-ttu-id="91053-129">I det här avsnittet skapar du en Azure tjänstens huvudnamn som Maven plugin-programmet använder när du distribuerar din behållare till Azure.</span><span class="sxs-lookup"><span data-stu-id="91053-129">In this section, you create an Azure service principal that the Maven plugin uses when deploying your container to Azure.</span></span>

1. <span data-ttu-id="91053-130">Öppna en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="91053-130">Open a command prompt.</span></span>

1. <span data-ttu-id="91053-131">Logga in på ditt Azure-konto med hjälp av Azure CLI:</span><span class="sxs-lookup"><span data-stu-id="91053-131">Sign into your Azure account by using the Azure CLI:</span></span>
   ```shell
   az login
   ```
   <span data-ttu-id="91053-132">Följ instruktionerna för att slutföra inloggningen.</span><span class="sxs-lookup"><span data-stu-id="91053-132">Follow the instructions to complete the sign-in process.</span></span>

1. <span data-ttu-id="91053-133">Skapa en Azure-tjänstens huvudnamn:</span><span class="sxs-lookup"><span data-stu-id="91053-133">Create an Azure service principal:</span></span>
   ```shell
   az ad sp create-for-rbac --name "uuuuuuuu" --password "pppppppp"
   ```
   <span data-ttu-id="91053-134">Där `uuuuuuuu` är användarnamnet och `pppppppp` är lösenordet för tjänstens huvudnamn.</span><span class="sxs-lookup"><span data-stu-id="91053-134">Where `uuuuuuuu` is the user name and `pppppppp` is the password for the service principal.</span></span>

1. <span data-ttu-id="91053-135">Azure svarar med JSON som liknar följande exempel:</span><span class="sxs-lookup"><span data-stu-id="91053-135">Azure responds with JSON that resembles the following example:</span></span>
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
   > <span data-ttu-id="91053-136">Du kommer använda värden från den här JSON-svar när du konfigurerar Maven-plugin-programmet för att distribuera din behållare till Azure.</span><span class="sxs-lookup"><span data-stu-id="91053-136">You will use the values from this JSON response when you configure the Maven plugin to deploy your container to Azure.</span></span> <span data-ttu-id="91053-137">Den `aaaaaaaa`, `uuuuuuuu`, `pppppppp`, och `tttttttt` är för platshållarvärden som används i det här exemplet för att göra det enklare att mappa värdena till deras respektive element när du konfigurerar din Maven `settings.xml` filen i nästa avsnitt .</span><span class="sxs-lookup"><span data-stu-id="91053-137">The `aaaaaaaa`, `uuuuuuuu`, `pppppppp`, and `tttttttt` are placeholder values, which are used in this example to make it easier to map these values to their respective elements when you configure your Maven `settings.xml` file in the next section.</span></span>
   >
   >

## <a name="configure-maven-to-use-your-azure-service-principal"></a><span data-ttu-id="91053-138">Konfigurera Maven för att använda din Azure-tjänstens huvudnamn</span><span class="sxs-lookup"><span data-stu-id="91053-138">Configure Maven to use your Azure service principal</span></span>

<span data-ttu-id="91053-139">I det här avsnittet använder du värdena från din Azure tjänstens huvudnamn konfigurera autentisering som Maven ska använda när du distribuerar din behållare till Azure.</span><span class="sxs-lookup"><span data-stu-id="91053-139">In this section, you use the values from your Azure service principal to configure the authentication that Maven will use when deploying your container to Azure.</span></span>

1. <span data-ttu-id="91053-140">Öppna din Maven `settings.xml` filen i en textredigerare; den här filen kan vara en sökväg som i följande exempel:</span><span class="sxs-lookup"><span data-stu-id="91053-140">Open your Maven `settings.xml` file in a text editor; this file might be in a path like the following examples:</span></span>
   * `/etc/maven/settings.xml`
   * `%ProgramFiles%\apache-maven\3.5.0\conf\settings.xml`
   * `$HOME/.m2/settings.xml`

1. <span data-ttu-id="91053-141">Lägg till Azure-tjänstens huvudnamn inställningarna från föregående avsnitt i den här kursen och den `<servers>` samling i den *settings.xml* filen, till exempel:</span><span class="sxs-lookup"><span data-stu-id="91053-141">Add your Azure service principal settings from the previous section of this tutorial to the `<servers>` collection in the *settings.xml* file; for example:</span></span>

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
   <span data-ttu-id="91053-142">Där:</span><span class="sxs-lookup"><span data-stu-id="91053-142">Where:</span></span>
   <span data-ttu-id="91053-143">Element</span><span class="sxs-lookup"><span data-stu-id="91053-143">Element</span></span> | <span data-ttu-id="91053-144">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="91053-144">Description</span></span>
   ---|---|---
   `<id>` | <span data-ttu-id="91053-145">Anger ett unikt namn som Maven använder för att leta upp dina säkerhetsinställningar när du distribuerar ditt webbprogram till Azure.</span><span class="sxs-lookup"><span data-stu-id="91053-145">Specifies a unique name which Maven uses to look up your security settings when you deploy your web app to Azure.</span></span>
   `<client>` | <span data-ttu-id="91053-146">Innehåller den `appId` värde från tjänstens huvudnamn.</span><span class="sxs-lookup"><span data-stu-id="91053-146">Contains the `appId` value from your service principal.</span></span>
   `<tenant>` | <span data-ttu-id="91053-147">Innehåller den `tenant` värde från tjänstens huvudnamn.</span><span class="sxs-lookup"><span data-stu-id="91053-147">Contains the `tenant` value from your service principal.</span></span>
   `<key>` | <span data-ttu-id="91053-148">Innehåller den `password` värde från tjänstens huvudnamn.</span><span class="sxs-lookup"><span data-stu-id="91053-148">Contains the `password` value from your service principal.</span></span>
   `<environment>` | <span data-ttu-id="91053-149">Definierar målmiljön Azure-molnet, vilket är `AZURE` i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="91053-149">Defines the target Azure cloud environment, which is `AZURE` in this example.</span></span> <span data-ttu-id="91053-150">(En fullständig lista över miljöer finns i den [Maven plugin-program för Azure Web Apps] dokumentation)</span><span class="sxs-lookup"><span data-stu-id="91053-150">(A full list of environments is available in the [Maven Plugin for Azure Web Apps] documentation)</span></span>

1. <span data-ttu-id="91053-151">Spara och Stäng den *settings.xml* fil.</span><span class="sxs-lookup"><span data-stu-id="91053-151">Save and close the *settings.xml* file.</span></span>

## <a name="optional-deploy-your-local-docker-file-to-docker-hub"></a><span data-ttu-id="91053-152">Valfritt: Distribuera din lokala Docker-fil till Docker-hubb</span><span class="sxs-lookup"><span data-stu-id="91053-152">OPTIONAL: Deploy your local Docker file to Docker Hub</span></span>

<span data-ttu-id="91053-153">Om du har en Docker-konto du push till Docker-NAV och skapa din Docker behållaren image lokalt.</span><span class="sxs-lookup"><span data-stu-id="91053-153">If you have a Docker account, you can build your Docker container image locally and push it to Docker Hub.</span></span> <span data-ttu-id="91053-154">Om du vill göra det använder du följande steg.</span><span class="sxs-lookup"><span data-stu-id="91053-154">To do so, use the following steps.</span></span>

1. <span data-ttu-id="91053-155">Öppna den `pom.xml` filen för tillämpningsprogrammet källan Start i en textredigerare.</span><span class="sxs-lookup"><span data-stu-id="91053-155">Open the `pom.xml` file for your Spring Boot application in a text editor.</span></span>

1. <span data-ttu-id="91053-156">Leta upp den `<imageName>` underordnat element för den `<containerSettings>` element.</span><span class="sxs-lookup"><span data-stu-id="91053-156">Locate the `<imageName>` child element of the `<containerSettings>` element.</span></span>

1. <span data-ttu-id="91053-157">Uppdatering av `${docker.image.prefix}` värdet med namnet på ditt Docker:</span><span class="sxs-lookup"><span data-stu-id="91053-157">Update the `${docker.image.prefix}` value with your Docker account name:</span></span>
   ```xml
   <containerSettings>
      <imageName>mydockeraccountname/${project.artifactId}</imageName>
   </containerSettings>
   ```

1. <span data-ttu-id="91053-158">Välj något av följande distributionsmetoder:</span><span class="sxs-lookup"><span data-stu-id="91053-158">Choose one of the following deployment methods:</span></span>

   * <span data-ttu-id="91053-159">Skapa behållaren avbildningen lokalt med Maven, och sedan använda Docker för att skicka din behållare till Docker-hubb:</span><span class="sxs-lookup"><span data-stu-id="91053-159">Build your container image locally with Maven, and then use Docker to push your container to Docker Hub:</span></span>
      ```shell
      mvn clean package docker:build
      docker push
      ```
   
   * <span data-ttu-id="91053-160">Om du har den [Docker-plugin för Maven] installerat, kan du automatiskt bygga och din behållaren bild till Docker-hubben med hjälp av den `-DpushImage` parameter:</span><span class="sxs-lookup"><span data-stu-id="91053-160">If you have the [Docker plugin for Maven] installed, you can automatically build and your container image to Docker Hub by using the `-DpushImage` parameter:</span></span>
      ```shell
      mvn clean package docker:build -DpushImage
      ```

## <a name="optional-customize-your-pomxml-before-deploying-your-container-to-azure"></a><span data-ttu-id="91053-161">Valfritt: Anpassa din pom.xml innan du distribuerar din behållare till Azure</span><span class="sxs-lookup"><span data-stu-id="91053-161">OPTIONAL: Customize your pom.xml before deploying your container to Azure</span></span>

<span data-ttu-id="91053-162">Öppna den `pom.xml` filen för tillämpningsprogrammet källan Start i en textredigerare och leta upp den `<plugin>` element för `azure-webapp-maven-plugin`.</span><span class="sxs-lookup"><span data-stu-id="91053-162">Open the `pom.xml` file for your Spring Boot application in a text editor, and then locate the `<plugin>` element for `azure-webapp-maven-plugin`.</span></span> <span data-ttu-id="91053-163">Det här elementet bör likna följande exempel:</span><span class="sxs-lookup"><span data-stu-id="91053-163">This element should resemble the following example:</span></span>

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

<span data-ttu-id="91053-164">Det finns flera värden som du kan ändra för Maven plugin-programmet och en utförlig beskrivning för var och en av dessa element är tillgänglig i den [Maven plugin-program för Azure Web Apps] dokumentation.</span><span class="sxs-lookup"><span data-stu-id="91053-164">There are several values that you can modify for the Maven plugin, and a detailed description for each of these elements is available in the [Maven Plugin for Azure Web Apps] documentation.</span></span> <span data-ttu-id="91053-165">Som som säger att det finns flera värden som är värda markering i den här artikeln:</span><span class="sxs-lookup"><span data-stu-id="91053-165">That being said, there are several values that are worth highlighting in this article:</span></span>

<span data-ttu-id="91053-166">Element</span><span class="sxs-lookup"><span data-stu-id="91053-166">Element</span></span> | <span data-ttu-id="91053-167">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="91053-167">Description</span></span>
---|---|---
`<version>` | <span data-ttu-id="91053-168">Anger versionen av den [Maven plugin-program för Azure Web Apps].</span><span class="sxs-lookup"><span data-stu-id="91053-168">Specifies the version of the [Maven Plugin for Azure Web Apps].</span></span> <span data-ttu-id="91053-169">Du bör kontrollera versionen som anges i den [Maven centrala lagringsplatsen](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-webapp-maven-plugin%22) så att du använder den senaste versionen.</span><span class="sxs-lookup"><span data-stu-id="91053-169">You should check the version listed in the [Maven Central Respository](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-webapp-maven-plugin%22) to ensure that you are using the latest version.</span></span>
`<authentication>` | <span data-ttu-id="91053-170">Anger autentiseringsinformationen för Azure, som i det här exemplet innehåller en `<serverId>` element som innehåller `azure-auth`; Maven används värdet för att slå upp Azure tjänstens huvudnamn värden i din Maven *settings.xml* fil som du har definierat i en tidigare i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="91053-170">Specifies the authentication information for Azure, which in this example contains a `<serverId>` element that contains `azure-auth`; Maven uses that value to look up the Azure service principal values in your Maven *settings.xml* file, which you defined in an earlier section of this article.</span></span>
`<resourceGroup>` | <span data-ttu-id="91053-171">Anger målresursgruppen som är `maven-plugin` i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="91053-171">Specifies the target resource group, which is `maven-plugin` in this example.</span></span> <span data-ttu-id="91053-172">Resursgruppens namn kommer att skapas under distributionen om det inte redan finns.</span><span class="sxs-lookup"><span data-stu-id="91053-172">The resource group will be created during deployment if it does not already exist.</span></span>
`<appName>` | <span data-ttu-id="91053-173">Anger målnamn för ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="91053-173">Specifies the target name for your web app.</span></span> <span data-ttu-id="91053-174">I det här exemplet är målnamnet `maven-linux-app-${maven.build.timestamp}`, där den `${maven.build.timestamp}` suffix läggs till i det här exemplet för att undvika konflikter.</span><span class="sxs-lookup"><span data-stu-id="91053-174">In this example, the target name is `maven-linux-app-${maven.build.timestamp}`, where the `${maven.build.timestamp}` suffix is appended in this example to avoid conflict.</span></span> <span data-ttu-id="91053-175">(Tidsstämpeln är valfria och du kan ange en unik sträng för namnet på appen.)</span><span class="sxs-lookup"><span data-stu-id="91053-175">(The timestamp is optional; you can specify any unique string for the app name.)</span></span>
`<region>` | <span data-ttu-id="91053-176">Anger målregionen, som i det här exemplet är `westus`.</span><span class="sxs-lookup"><span data-stu-id="91053-176">Specifies the target region, which in this example is `westus`.</span></span> <span data-ttu-id="91053-177">(En fullständig lista finns i den [Maven plugin-program för Azure Web Apps] dokumentation.)</span><span class="sxs-lookup"><span data-stu-id="91053-177">(A full list is in the [Maven Plugin for Azure Web Apps] documentation.)</span></span>
`<appSettings>` | <span data-ttu-id="91053-178">Anger alla unika inställningar för Maven ska användas när du distribuerar ditt webbprogram till Azure.</span><span class="sxs-lookup"><span data-stu-id="91053-178">Specifies any unique settings for Maven to use when deploying your web app to Azure.</span></span> <span data-ttu-id="91053-179">I det här exemplet en `<property>` elementet innehåller ett namn/värde-par för underordnade element som anger porten för din app.</span><span class="sxs-lookup"><span data-stu-id="91053-179">In this example, a `<property>` element contains a name/value pair of child elements that specify the port for your app.</span></span>

> [!NOTE]
>
> <span data-ttu-id="91053-180">Inställningar för att ändra portnumret i det här exemplet är bara nödvändigt när du ändrar porten från standard.</span><span class="sxs-lookup"><span data-stu-id="91053-180">The settings to change the port number in this example are only necessary when you are changing the port from the default.</span></span>
>

## <a name="build-and-deploy-your-container-to-azure"></a><span data-ttu-id="91053-181">Skapa och distribuera din behållare till Azure</span><span class="sxs-lookup"><span data-stu-id="91053-181">Build and deploy your container to Azure</span></span>

<span data-ttu-id="91053-182">När du har konfigurerat alla inställningar i föregående avsnitt i den här artikeln, är du redo att distribuera din behållare till Azure.</span><span class="sxs-lookup"><span data-stu-id="91053-182">Once you have configured all of the settings in the preceding sections of this article, you are ready to deploy your container to Azure.</span></span> <span data-ttu-id="91053-183">Så här loggar du in:</span><span class="sxs-lookup"><span data-stu-id="91053-183">To do so, use the following steps:</span></span>

1. <span data-ttu-id="91053-184">Från Kommandotolken eller terminalfönster som du använde tidigare, återskapa JAR-filen med Maven om du har gjort ändringar i *pom.xml* filen, till exempel:</span><span class="sxs-lookup"><span data-stu-id="91053-184">From the command prompt or terminal window that you were using earlier, rebuild the JAR file using Maven if you made any changes to the *pom.xml* file; for example:</span></span>
   ```shell
   mvn clean package
   ```

1. <span data-ttu-id="91053-185">Distribuera webbappen till Azure med hjälp av Maven; Exempel:</span><span class="sxs-lookup"><span data-stu-id="91053-185">Deploy your web app to Azure by using Maven; for example:</span></span>
   ```shell
   mvn azure-webapp:deploy
   ```

<span data-ttu-id="91053-186">Maven ska distribuera webbappen till Azure; Om webbappen inte redan finns, skapas.</span><span class="sxs-lookup"><span data-stu-id="91053-186">Maven will deploy your web app to Azure; if the web app does not already exist, it will be created.</span></span>

> [!NOTE]
>
> <span data-ttu-id="91053-187">Om den region som du anger i den `<region>` element i din *pom.xml* filen har inte tillräckligt många servrar som är tillgängliga när du börjar din distribution, kan uppstå ett fel som liknar följande exempel:</span><span class="sxs-lookup"><span data-stu-id="91053-187">If the region which you specify in the `<region>` element of your *pom.xml* file does not have enough servers available when you start your deployment, you might see an error similar to the following example:</span></span>
>
> ```
> [INFO] Start deploying to Web App maven-linux-app-20170804...
> [INFO] ------------------------------------------------------------------------
> [INFO] BUILD FAILURE
> [INFO] ------------------------------------------------------------------------
> [INFO] Total time: 31.059 s
> [INFO] Finished at: 2017-08-04T12:15:47-07:00
> [INFO] Final Memory: 51M/279M
> [INFO] ------------------------------------------------------------------------
> [ERROR] Failed to execute goal com.microsoft.azure:azure-webapp-maven-plugin:0.1.3:deploy (default-cli) on project gs-spring-boot-docker: null: MojoExecutionException: CloudException: OnError while emitting onNext value: retrofit2.Response.class
> ```
>
> <span data-ttu-id="91053-188">Om det händer kan du ange en annan region och kör igen Maven-kommando för att distribuera ditt program.</span><span class="sxs-lookup"><span data-stu-id="91053-188">If this happens, you can specify another region and re-run the Maven command to deploy your application.</span></span>
>
>

<span data-ttu-id="91053-189">När webbplatsen har distribuerats, kommer du att kunna hantera den med hjälp av den [Azure-portalen].</span><span class="sxs-lookup"><span data-stu-id="91053-189">When your web has been deployed, you will be able to manage it by using the [Azure portal].</span></span>

* <span data-ttu-id="91053-190">Ditt webbprogram visas i **Apptjänster**:</span><span class="sxs-lookup"><span data-stu-id="91053-190">Your web app will be listed in **App Services**:</span></span>

   ![Webbprogram som anges i Azure portal Apptjänster][AP01]

* <span data-ttu-id="91053-192">Och URL: en för ditt webbprogram kommer att visas i den **översikt** för ditt webbprogram:</span><span class="sxs-lookup"><span data-stu-id="91053-192">And the URL for your web app will be listed in the **Overview** for your web app:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="91053-194">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="91053-194">Next steps</span></span>

<span data-ttu-id="91053-195">Mer information om de olika tekniker som beskrivs i den här artikeln finns i följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="91053-195">For more information about the various technologies discussed in this article, see the following articles:</span></span>

* <span data-ttu-id="91053-196">[Maven plugin-program för Azure Web Apps]</span><span class="sxs-lookup"><span data-stu-id="91053-196">[Maven Plugin for Azure Web Apps]</span></span>

* [<span data-ttu-id="91053-197">Logga in till Azure från Azure CLI</span><span class="sxs-lookup"><span data-stu-id="91053-197">Log in to Azure from the Azure CLI</span></span>](/azure/xplat-cli-connect)

* [<span data-ttu-id="91053-198">Hur du använder Maven-plugin-program för Azure Web Apps för att distribuera en start av vår app till Azure App Service</span><span class="sxs-lookup"><span data-stu-id="91053-198">How to use the Maven Plugin for Azure Web Apps to deploy a Spring Boot app to Azure App Service </span></span>](app-service-web-deploy-spring-boot-app-with-maven-plugin.md)

* [<span data-ttu-id="91053-199">Skapa en Azure tjänstens huvudnamn med Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="91053-199">Create an Azure service principal with Azure CLI 2.0</span></span>](/cli/azure/create-an-azure-service-principal-azure-cli)

* [<span data-ttu-id="91053-200">Referens för maven</span><span class="sxs-lookup"><span data-stu-id="91053-200">Maven Settings Reference</span></span>](https://maven.apache.org/settings.html)

* <span data-ttu-id="91053-201">[Docker-plugin för Maven]</span><span class="sxs-lookup"><span data-stu-id="91053-201">[Docker plugin for Maven]</span></span>

<!-- URL List -->

<span data-ttu-id="91053-202">[Azure-kommandoradsgränssnittet (CLI)]: /cli/azure/overview</span><span class="sxs-lookup"><span data-stu-id="91053-202">[Azure Command-Line Interface (CLI)]: /cli/azure/overview</span></span>
[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
<span data-ttu-id="91053-203">[Azure-portalen]: https://portal.azure.com/</span><span class="sxs-lookup"><span data-stu-id="91053-203">[Azure portal]: https://portal.azure.com/</span></span>
<span data-ttu-id="91053-204">[Docker]: https://www.docker.com/</span><span class="sxs-lookup"><span data-stu-id="91053-204">[Docker]: https://www.docker.com/</span></span>
<span data-ttu-id="91053-205">[Docker-plugin för Maven]: https://github.com/spotify/docker-maven-plugin</span><span class="sxs-lookup"><span data-stu-id="91053-205">[Docker plugin for Maven]: https://github.com/spotify/docker-maven-plugin</span></span>
<span data-ttu-id="91053-206">[kostnadsfritt Azure-konto]: https://azure.microsoft.com/pricing/free-trial/</span><span class="sxs-lookup"><span data-stu-id="91053-206">[free Azure account]: https://azure.microsoft.com/pricing/free-trial/</span></span>
<span data-ttu-id="91053-207">[Git]: https://github.com/</span><span class="sxs-lookup"><span data-stu-id="91053-207">[Git]: https://github.com/</span></span>
[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
<span data-ttu-id="91053-208">[Maven]: http://maven.apache.org/</span><span class="sxs-lookup"><span data-stu-id="91053-208">[Maven]: http://maven.apache.org/</span></span>
<span data-ttu-id="91053-209">[MSDN-prenumerantförmåner]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/</span><span class="sxs-lookup"><span data-stu-id="91053-209">[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/</span></span>
[Spring Boot]: http://projects.spring.io/spring-boot/
<span data-ttu-id="91053-210">[Vårversionen Start på Docker komma igång]: https://github.com/spring-guides/gs-spring-boot-docker</span><span class="sxs-lookup"><span data-stu-id="91053-210">[Spring Boot on Docker Getting Started]: https://github.com/spring-guides/gs-spring-boot-docker</span></span>
[Spring Framework]: https://spring.io/
<span data-ttu-id="91053-211">[Maven plugin-program för Azure Web Apps]: https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin</span><span class="sxs-lookup"><span data-stu-id="91053-211">[Maven Plugin for Azure Web Apps]: https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin</span></span>

<!-- IMG List -->

[AP01]: ./media/app-service-web-deploy-containerized-spring-boot-app-with-maven-plugin/AP01.png
[AP02]: ./media/app-service-web-deploy-containerized-spring-boot-app-with-maven-plugin/AP02.png
