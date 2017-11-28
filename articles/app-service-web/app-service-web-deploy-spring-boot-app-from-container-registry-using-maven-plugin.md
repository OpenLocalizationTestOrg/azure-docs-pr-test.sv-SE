---
title: "aaaHow toouse hello Maven-plugin-programmet för Azure Web Apps toodeploy en källan Start-app i Azure Container registret tooAzure Apptjänst"
description: "Den här kursen får du om hello steg toodeploy en källan startprogrammet i Azure Container registret tooAzure tooAzure Apptjänst med hjälp av ett Maven plugin-program."
services: 
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
ms.date: 08/07/2017
ms.author: robmcm;kevinzha
ms.openlocfilehash: 55b95e310c9ee186a6d77d941c5a620c2e259d8a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-maven-plugin-for-azure-web-apps-toodeploy-a-spring-boot-app-in-azure-container-registry-tooazure-app-service"></a><span data-ttu-id="10190-103">Hur toouse hello Maven-plugin-programmet för Azure Web Apps toodeploy en källan Start-app i Azure Container registret tooAzure Apptjänst</span><span class="sxs-lookup"><span data-stu-id="10190-103">How toouse hello Maven Plugin for Azure Web Apps toodeploy a Spring Boot app in Azure Container Registry tooAzure App Service</span></span>

<span data-ttu-id="10190-104">Hej ** [Vårversionen Framework] ** ett populära ramverk för öppen källkod som gör att Java-utvecklare skapa webb-, mobil- och API-program.</span><span class="sxs-lookup"><span data-stu-id="10190-104">hello **[Spring Framework]** is a popular open-source framework that helps Java developers create web, mobile, and API applications.</span></span> <span data-ttu-id="10190-105">Den här kursen använder en exempelapp som skapats med hjälp av [Vårversionen Start], en konvention inriktat tillvägagångssätt för att använda vår tooget igång snabbt.</span><span class="sxs-lookup"><span data-stu-id="10190-105">This tutorial uses a sample app created using [Spring Boot], a convention-driven approach for using Spring tooget started quickly.</span></span>

<span data-ttu-id="10190-106">Den här artikeln visar hur toodeploy en exempel källan Start programmet tooAzure behållare registret, och sedan använda hello Maven-plugin-programmet för Azure Web Apps toodeploy ditt program tooAzure Apptjänst.</span><span class="sxs-lookup"><span data-stu-id="10190-106">This article demonstrates how toodeploy a sample Spring Boot application tooAzure Container Registry, and then use hello Maven Plugin for Azure Web Apps toodeploy your application tooAzure App Service.</span></span>

> [!NOTE]
>
> <span data-ttu-id="10190-107">Hej Maven plugin-program för Azure Web Apps är tillgänglig som en förhandsgranskning.</span><span class="sxs-lookup"><span data-stu-id="10190-107">hello Maven Plugin for Azure Web Apps is currently available as a preview.</span></span> <span data-ttu-id="10190-108">För närvarande stöds endast FTP-publicering, även om ytterligare funktioner som planeras att hello framtida.</span><span class="sxs-lookup"><span data-stu-id="10190-108">For now, only FTP publishing is supported, although additional features are planned for hello future.</span></span>
>

## <a name="prerequisites"></a><span data-ttu-id="10190-109">Krav</span><span class="sxs-lookup"><span data-stu-id="10190-109">Prerequisites</span></span>

<span data-ttu-id="10190-110">I ordning toocomplete hello steg i den här kursen behöver du toohave hello följande krav:</span><span class="sxs-lookup"><span data-stu-id="10190-110">In order toocomplete hello steps in this tutorial, you need toohave hello following prerequisites:</span></span>

* <span data-ttu-id="10190-111">En Azure-prenumeration; Om du inte redan har en Azure-prenumeration, kan du aktivera din [MSDN-prenumerantförmåner] eller registrera dig för en [kostnadsfritt Azure-konto].</span><span class="sxs-lookup"><span data-stu-id="10190-111">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="10190-112">Hej [Azure-kommandoradsgränssnittet (CLI)].</span><span class="sxs-lookup"><span data-stu-id="10190-112">hello [Azure Command-Line Interface (CLI)].</span></span>
* <span data-ttu-id="10190-113">En uppdaterad [Java Development Kit (JDK)], version 1.7 eller senare.</span><span class="sxs-lookup"><span data-stu-id="10190-113">An up-to-date [Java Development Kit (JDK)], version 1.7 or later.</span></span>
* <span data-ttu-id="10190-114">Apache's [Maven] skapa verktyget (Version 3).</span><span class="sxs-lookup"><span data-stu-id="10190-114">Apache's [Maven] build tool (Version 3).</span></span>
* <span data-ttu-id="10190-115">En [Git] klienten.</span><span class="sxs-lookup"><span data-stu-id="10190-115">A [Git] client.</span></span>
* <span data-ttu-id="10190-116">En [Docker] klienten.</span><span class="sxs-lookup"><span data-stu-id="10190-116">A [Docker] client.</span></span>

> [!NOTE]
>
> <span data-ttu-id="10190-117">På grund av toohello virtualisering kraven i den här kursen får inte kan du använda hello stegen i den här artikeln på en virtuell dator; Du måste använda en fysisk dator med aktiverat funktionerna för virtualisering.</span><span class="sxs-lookup"><span data-stu-id="10190-117">Due toohello virtualization requirements of this tutorial, you cannot follow hello steps in this article on a virtual machine; you must use a physical computer with virtualization features enabled.</span></span>
>

## <a name="clone-hello-sample-spring-boot-on-docker-web-app"></a><span data-ttu-id="10190-118">Klona hello exempel källan Start på Docker-webbprogram</span><span class="sxs-lookup"><span data-stu-id="10190-118">Clone hello sample Spring Boot on Docker web app</span></span>

<span data-ttu-id="10190-119">I det här avsnittet klona en container startprogrammet för källan och testa den lokalt.</span><span class="sxs-lookup"><span data-stu-id="10190-119">In this section, you clone a containerized Spring Boot application and test it locally.</span></span>

1. <span data-ttu-id="10190-120">Öppna en kommandotolk eller ett terminalfönster och skapa en lokal katalog toohold källan Start programmet och ändra toothat katalog. Exempel:</span><span class="sxs-lookup"><span data-stu-id="10190-120">Open a command prompt or terminal window and create a local directory toohold your Spring Boot application, and change toothat directory; for example:</span></span>
   ```shell
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   <span data-ttu-id="10190-121">-- eller--</span><span class="sxs-lookup"><span data-stu-id="10190-121">-- or --</span></span>
   ```shell
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. <span data-ttu-id="10190-122">Klona hello [Vårversionen Start på Docker komma igång] exempelprojektet i hello-katalog som du har skapat, till exempel:</span><span class="sxs-lookup"><span data-stu-id="10190-122">Clone hello [Spring Boot on Docker Getting Started] sample project into hello directory you created; for example:</span></span>
   ```shell
   git clone -b private-registry https://github.com/Microsoft/gs-spring-boot-docker
   ```

1. <span data-ttu-id="10190-123">Ändra katalogen toohello slutförts projektet. Exempel:</span><span class="sxs-lookup"><span data-stu-id="10190-123">Change directory toohello completed project; for example:</span></span>
   ```shell
   cd gs-spring-boot-docker/complete
   ```

1. <span data-ttu-id="10190-124">Skapa hello JAR-filen med Maven; Exempel:</span><span class="sxs-lookup"><span data-stu-id="10190-124">Build hello JAR file using Maven; for example:</span></span>
   ```shell
   mvn clean package
   ```

1. <span data-ttu-id="10190-125">När hello webbprogrammet har skapats, starta hello webbprogrammet med Maven; Exempel:</span><span class="sxs-lookup"><span data-stu-id="10190-125">When hello web app has been created, start hello web app using Maven; for example:</span></span>
   ```shell
   mvn spring-boot:run
   ```

1. <span data-ttu-id="10190-126">Testa hello webbprogram genom att bläddra tooit lokalt med hjälp av en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="10190-126">Test hello web app by browsing tooit locally using a web browser.</span></span> <span data-ttu-id="10190-127">Du kan till exempel använda följande kommando om du har curl tillgängliga hello:</span><span class="sxs-lookup"><span data-stu-id="10190-127">For example, you could use hello following command if you have curl available:</span></span>
   ```shell
   curl http://localhost:8080
   ```

1. <span data-ttu-id="10190-128">Du bör se hello följande meddelande visas: **Docker hälsningsmeddelande**</span><span class="sxs-lookup"><span data-stu-id="10190-128">You should see hello following message displayed: **Hello Docker World**</span></span>

   ![Bläddra Sample-appen lokalt][SB01]

## <a name="create-an-azure-service-principal"></a><span data-ttu-id="10190-130">Skapa en Azure huvudnamn för tjänsten</span><span class="sxs-lookup"><span data-stu-id="10190-130">Create an Azure service principal</span></span>

<span data-ttu-id="10190-131">I det här avsnittet skapar du en Azure tjänstens huvudnamn som hello Maven plugin-programmet använder när du distribuerar behållare-tooAzure.</span><span class="sxs-lookup"><span data-stu-id="10190-131">In this section, you create an Azure service principal that hello Maven plugin uses when deploying your container tooAzure.</span></span>

1. <span data-ttu-id="10190-132">Öppna en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="10190-132">Open a command prompt.</span></span>

1. <span data-ttu-id="10190-133">Logga in på ditt Azure-konto med hjälp av hello Azure CLI:</span><span class="sxs-lookup"><span data-stu-id="10190-133">Sign into your Azure account by using hello Azure CLI:</span></span>
   ```azurecli
   az login
   ```
   <span data-ttu-id="10190-134">Följ hello instruktioner toocomplete hello inloggningsprocessen.</span><span class="sxs-lookup"><span data-stu-id="10190-134">Follow hello instructions toocomplete hello sign-in process.</span></span>

1. <span data-ttu-id="10190-135">Skapa en Azure-tjänstens huvudnamn:</span><span class="sxs-lookup"><span data-stu-id="10190-135">Create an Azure service principal:</span></span>
   ```azurecli
   az ad sp create-for-rbac --name "uuuuuuuu" --password "pppppppp"
   ```
   <span data-ttu-id="10190-136">Där `uuuuuuuu` är hello användarnamn och `pppppppp` är hello lösenord för hello tjänstens huvudnamn.</span><span class="sxs-lookup"><span data-stu-id="10190-136">Where `uuuuuuuu` is hello user name and `pppppppp` is hello password for hello service principal.</span></span>

1. <span data-ttu-id="10190-137">Azure svarar med JSON som liknar följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="10190-137">Azure responds with JSON that resembles hello following example:</span></span>
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
   > <span data-ttu-id="10190-138">Du använder hello värden från den här JSON-svar när du konfigurerar hello Maven plugin-programmet toodeploy behållaren-tooAzure.</span><span class="sxs-lookup"><span data-stu-id="10190-138">You will use hello values from this JSON response when you configure hello Maven plugin toodeploy your container tooAzure.</span></span> <span data-ttu-id="10190-139">hello `aaaaaaaa`, `uuuuuuuu`, `pppppppp`, och `tttttttt` platshållare för värden som används i det här exemplet toomake den enklare toomap värden tootheir respektive elementen när du konfigurerar din Maven `settings.xml` filen i hello nästa avsnittet.</span><span class="sxs-lookup"><span data-stu-id="10190-139">hello `aaaaaaaa`, `uuuuuuuu`, `pppppppp`, and `tttttttt` are placeholder values, which are used in this example toomake it easier toomap these values tootheir respective elements when you configure your Maven `settings.xml` file in hello next section.</span></span>
   >
   >

## <a name="create-an-azure-container-registry-using-hello-azure-cli"></a><span data-ttu-id="10190-140">Skapa ett Azure Container registret med hjälp av hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="10190-140">Create an Azure Container Registry using hello Azure CLI</span></span>

1. <span data-ttu-id="10190-141">Öppna en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="10190-141">Open a command prompt.</span></span>

1. <span data-ttu-id="10190-142">Logga in tooyour Azure-konto:</span><span class="sxs-lookup"><span data-stu-id="10190-142">Log in tooyour Azure account:</span></span>
   ```azurecli
   az login
   ```

1. <span data-ttu-id="10190-143">Skapa en resursgrupp för hello Azure-resurser du vill använda i den här artikeln:</span><span class="sxs-lookup"><span data-stu-id="10190-143">Create a resource group for hello Azure resources you will use in this article:</span></span>
   ```azurecli
   az group create --name=wingtiptoysresources --location=westus
   ```
   <span data-ttu-id="10190-144">Ersätt `wingtiptoysresources` i det här exemplet med ett unikt namn för resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="10190-144">Replace `wingtiptoysresources` in this example with a unique name for your resource group.</span></span>

1. <span data-ttu-id="10190-145">Skapa ett register för privat Azure-behållaren i hello resursgruppen för vår Start appen:</span><span class="sxs-lookup"><span data-stu-id="10190-145">Create a private Azure container registry in hello resource group for your Spring Boot app:</span></span> 
   ```azurecli
   az acr create --admin-enabled --resource-group wingtiptoysresources --location westus --name wingtiptoysregistry --sku Basic
   ```
   <span data-ttu-id="10190-146">Ersätt `wingtiptoysregistry` i det här exemplet med ett unikt namn för behållaren registret.</span><span class="sxs-lookup"><span data-stu-id="10190-146">Replace `wingtiptoysregistry` in this example with a unique name for your container registry.</span></span>

1. <span data-ttu-id="10190-147">Hämta hello lösenord för behållaren registret:</span><span class="sxs-lookup"><span data-stu-id="10190-147">Retrieve hello password for your container registry:</span></span>
   ```azurecli
   az acr credential show --name wingtiptoysregistry --query passwords[0]
   ```
   <span data-ttu-id="10190-148">Azure kommer att svara med ditt lösenord. Exempel:</span><span class="sxs-lookup"><span data-stu-id="10190-148">Azure will respond with your password; for example:</span></span>
   ```json
   {
      "name": "password",
      "value": "xxxxxxxxxx"
   }
   ```

## <a name="add-your-azure-container-registry-and-azure-service-principal-tooyour-maven-settings"></a><span data-ttu-id="10190-149">Lägg till din Azure-behållaren registret och Azure service principal tooyour Maven-inställningar</span><span class="sxs-lookup"><span data-stu-id="10190-149">Add your Azure container registry and Azure service principal tooyour Maven settings</span></span>

1. <span data-ttu-id="10190-150">Öppna din Maven `settings.xml` filen i en textredigerare; den här filen kan vara en sökväg som hello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="10190-150">Open your Maven `settings.xml` file in a text editor; this file might be in a path like hello following examples:</span></span>
   * `/etc/maven/settings.xml`
   * `%ProgramFiles%\apache-maven\3.5.0\conf\settings.xml`
   * `$HOME/.m2/settings.xml`

1. <span data-ttu-id="10190-151">Lägga till din Azure-behållare åtkomst registerinställningar från hello föregående avsnitt i den här artikeln toohello `<servers>` samling i hello *settings.xml* filen, till exempel:</span><span class="sxs-lookup"><span data-stu-id="10190-151">Add your Azure Container Registry access settings from hello previous section of this article toohello `<servers>` collection in hello *settings.xml* file; for example:</span></span>

   ```xml
   <servers>
      <server>
         <id>wingtiptoysregistry</id>
         <username>wingtiptoysregistry</username>
         <password>xxxxxxxxxx</password>
      </server>
   </servers>
   ```
   <span data-ttu-id="10190-152">Där:</span><span class="sxs-lookup"><span data-stu-id="10190-152">Where:</span></span>
   <span data-ttu-id="10190-153">Element</span><span class="sxs-lookup"><span data-stu-id="10190-153">Element</span></span> | <span data-ttu-id="10190-154">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="10190-154">Description</span></span>
   ---|---|---
   `<id>` | <span data-ttu-id="10190-155">Innehåller hello namnet på behållaren för privat Azure-registret.</span><span class="sxs-lookup"><span data-stu-id="10190-155">Contains hello name of your private Azure container registry.</span></span>
   `<username>` | <span data-ttu-id="10190-156">Innehåller hello namnet på behållaren för privat Azure-registret.</span><span class="sxs-lookup"><span data-stu-id="10190-156">Contains hello name of your private Azure container registry.</span></span>
   `<password>` | <span data-ttu-id="10190-157">Innehåller hello-lösenord som du hämtade i hello föregående avsnitt i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="10190-157">Contains hello password you retrieved in hello previous section of this article.</span></span>

1. <span data-ttu-id="10190-158">Lägg till Azure-tjänstens huvudnamn inställningarna från ett tidigare avsnitt i den här artikeln toohello `<servers>` samling i hello *settings.xml* filen, till exempel:</span><span class="sxs-lookup"><span data-stu-id="10190-158">Add your Azure service principal settings from an earlier section of this article toohello `<servers>` collection in hello *settings.xml* file; for example:</span></span>

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
   <span data-ttu-id="10190-159">Där:</span><span class="sxs-lookup"><span data-stu-id="10190-159">Where:</span></span>
   <span data-ttu-id="10190-160">Element</span><span class="sxs-lookup"><span data-stu-id="10190-160">Element</span></span> | <span data-ttu-id="10190-161">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="10190-161">Description</span></span>
   ---|---|---
   `<id>` | <span data-ttu-id="10190-162">Anger ett unikt namn som Maven använder toolook in dina säkerhetsinställningar när du distribuerar din web app tooAzure.</span><span class="sxs-lookup"><span data-stu-id="10190-162">Specifies a unique name which Maven uses toolook up your security settings when you deploy your web app tooAzure.</span></span>
   `<client>` | <span data-ttu-id="10190-163">Innehåller hello `appId` värde från tjänstens huvudnamn.</span><span class="sxs-lookup"><span data-stu-id="10190-163">Contains hello `appId` value from your service principal.</span></span>
   `<tenant>` | <span data-ttu-id="10190-164">Innehåller hello `tenant` värde från tjänstens huvudnamn.</span><span class="sxs-lookup"><span data-stu-id="10190-164">Contains hello `tenant` value from your service principal.</span></span>
   `<key>` | <span data-ttu-id="10190-165">Innehåller hello `password` värde från tjänstens huvudnamn.</span><span class="sxs-lookup"><span data-stu-id="10190-165">Contains hello `password` value from your service principal.</span></span>
   `<environment>` | <span data-ttu-id="10190-166">Definierar hello Azure-molnet målmiljön, vilket är `AZURE` i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="10190-166">Defines hello target Azure cloud environment, which is `AZURE` in this example.</span></span> <span data-ttu-id="10190-167">(En fullständig lista över miljöer finns i hello [Maven plugin-program för Azure Web Apps] dokumentation)</span><span class="sxs-lookup"><span data-stu-id="10190-167">(A full list of environments is available in hello [Maven Plugin for Azure Web Apps] documentation)</span></span>

1. <span data-ttu-id="10190-168">Spara och Stäng hello *settings.xml* fil.</span><span class="sxs-lookup"><span data-stu-id="10190-168">Save and close hello *settings.xml* file.</span></span>

## <a name="build-your-docker-container-image-and-push-it-tooyour-azure-container-registry"></a><span data-ttu-id="10190-169">Skapa din Docker behållaren image och skicka tooyour Azure-behållaren registret</span><span class="sxs-lookup"><span data-stu-id="10190-169">Build your Docker container image and push it tooyour Azure container registry</span></span>

1. <span data-ttu-id="10190-170">Navigera toohello slutförts projektkatalogen för tillämpningsprogrammet källan start (t.ex.) ”*C:\SpringBoot\gs-spring-boot-docker\complete*” eller ”*/users/robert/SpringBoot/gs-spring-boot-docker/complete*”), och öppna hello *pom.xml* filen med en textredigerare.</span><span class="sxs-lookup"><span data-stu-id="10190-170">Navigate toohello completed project directory for your Spring Boot application, (e.g. "*C:\SpringBoot\gs-spring-boot-docker\complete*" or "*/users/robert/SpringBoot/gs-spring-boot-docker/complete*"), and open hello *pom.xml* file with a text editor.</span></span>

1. <span data-ttu-id="10190-171">Uppdatera hello `<properties>` samling i hello *pom.xml* fil med hello inloggningen server värde för Azure-behållare registret från hello föregående avsnitt i den här självstudiekursen, till exempel:</span><span class="sxs-lookup"><span data-stu-id="10190-171">Update hello `<properties>` collection in hello *pom.xml* file with hello login server value for your Azure Container Registry from hello previous section of this tutorial; for example:</span></span>

   ```xml
   <properties>
      <azure.containerRegistry>wingtiptoysregistry</azure.containerRegistry>
      <docker.image.prefix>${azure.containerRegistry}.azurecr.io</docker.image.prefix>
      <java.version>1.8</java.version>
      <maven.build.timestamp.format>yyyyMMddHHmmssSSS</maven.build.timestamp.format>
   </properties>
   ```
   <span data-ttu-id="10190-172">Där:</span><span class="sxs-lookup"><span data-stu-id="10190-172">Where:</span></span>
   <span data-ttu-id="10190-173">Element</span><span class="sxs-lookup"><span data-stu-id="10190-173">Element</span></span> | <span data-ttu-id="10190-174">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="10190-174">Description</span></span>
   ---|---|---
   `<azure.containerRegistry>` | <span data-ttu-id="10190-175">Anger hello för privat Azure-behållaren registret.</span><span class="sxs-lookup"><span data-stu-id="10190-175">Specifies hello name of your private Azure container registry.</span></span>
   `<docker.image.prefix>` | <span data-ttu-id="10190-176">Anger hello URL för privat Azure-behållaren registret, som hämtas genom att lägga till ”. azurecr.io” toohello namnet på din privata behållare registret.</span><span class="sxs-lookup"><span data-stu-id="10190-176">Specifies hello URL of your private Azure container registry, which is derived by appending ".azurecr.io" toohello name of your private container registry.</span></span>

1. <span data-ttu-id="10190-177">Kontrollera att `<plugin>` för hello Docker-plugin-programmet i din *pom.xml* filen innehåller hello rätt egenskaper för hello server adress och registret inloggningsnamn hello föregående steg i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="10190-177">Verify that `<plugin>` for hello Docker plugin in your *pom.xml* file contains hello correct properties for hello login server address and registry name from hello previous step in this tutorial.</span></span> <span data-ttu-id="10190-178">Exempel:</span><span class="sxs-lookup"><span data-stu-id="10190-178">For example:</span></span>

   ```xml
   <plugin>
      <groupId>com.spotify</groupId>
      <artifactId>docker-maven-plugin</artifactId>
      <version>0.4.11</version>
      <configuration>
         <imageName>${docker.image.prefix}/${project.artifactId}</imageName>
         <registryUrl>https://${docker.image.prefix}</registryUrl>
         <serverId>${azure.containerRegistry}</serverId>
         <dockerDirectory>src/main/docker</dockerDirectory>
         <resources>
            <resource>
               <targetPath>/</targetPath>
               <directory>${project.build.directory}</directory>
               <include>${project.build.finalName}.jar</include>
            </resource>
         </resources>
      </configuration>
   </plugin>
   ```
   <span data-ttu-id="10190-179">Där:</span><span class="sxs-lookup"><span data-stu-id="10190-179">Where:</span></span>
   <span data-ttu-id="10190-180">Element</span><span class="sxs-lookup"><span data-stu-id="10190-180">Element</span></span> | <span data-ttu-id="10190-181">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="10190-181">Description</span></span>
   ---|---|---
   `<serverId>` | <span data-ttu-id="10190-182">Anger hello-egenskap som innehåller namnet på behållaren för privat Azure-registret.</span><span class="sxs-lookup"><span data-stu-id="10190-182">Specifies hello property which contains name of your private Azure container registry.</span></span>
   `<registryUrl>` | <span data-ttu-id="10190-183">Anger hello-egenskap som innehåller hello URL för privat Azure-behållaren registret.</span><span class="sxs-lookup"><span data-stu-id="10190-183">Specifies hello property which contains hello URL of your private Azure container registry.</span></span>

1. <span data-ttu-id="10190-184">Navigera toohello slutförts projektkatalogen för tillämpningsprogrammet källan start och kör följande kommando toorebuild hello programmet hello och push hello behållaren tooyour Azure-behållaren registret:</span><span class="sxs-lookup"><span data-stu-id="10190-184">Navigate toohello completed project directory for your Spring Boot application and run hello following command toorebuild hello application and push hello container tooyour Azure container registry:</span></span>

   ```
   mvn package docker:build -DpushImage 
   ```

1. <span data-ttu-id="10190-185">Valfritt: Bläddra toohello [Azure-portalen] och kontrollera att det finns Docker behållare bild med namnet **gs-källan-Start-docker** i behållaren-registret.</span><span class="sxs-lookup"><span data-stu-id="10190-185">OPTIONAL: Browse toohello [Azure portal] and verify that there is Docker container image named **gs-spring-boot-docker** in your container registry.</span></span>

   ![Kontrollera behållare i Azure-portalen][CR01]

## <a name="customize-your-pomxml-then-build-and-deploy-your-container-tooazure"></a><span data-ttu-id="10190-187">Anpassa din pom.xml och sedan skapa och distribuera behållaren-tooAzure</span><span class="sxs-lookup"><span data-stu-id="10190-187">Customize your pom.xml, then build and deploy your container tooAzure</span></span>

<span data-ttu-id="10190-188">Öppna hello `pom.xml` filen för tillämpningsprogrammet källan Start i en textredigerare och leta sedan upp hello `<plugin>` element för `azure-webapp-maven-plugin`.</span><span class="sxs-lookup"><span data-stu-id="10190-188">Open hello `pom.xml` file for your Spring Boot application in a text editor, and then locate hello `<plugin>` element for `azure-webapp-maven-plugin`.</span></span> <span data-ttu-id="10190-189">Det här elementet bör likna följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="10190-189">This element should resemble hello following example:</span></span>

   ```xml
   <plugin>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-webapp-maven-plugin</artifactId>
      <version>0.1.3</version>
      <configuration>
         <authentication>
            <serverId>azure-auth</serverId>
         </authentication>
         <resourceGroup>wingtiptoysresources</resourceGroup>
         <appName>maven-linux-app-${maven.build.timestamp}</appName>
         <region>westus</region>
         <containerSettings>
            <imageName>${docker.image.prefix}/${project.artifactId}</imageName>
            <registryUrl>https://${docker.image.prefix}</registryUrl>
            <serverId>${azure.containerRegistry}</serverId>
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

<span data-ttu-id="10190-190">Det finns flera värden som du kan ändra för hello Maven plugin-program och en utförlig beskrivning för var och en av dessa element är tillgänglig i hello [Maven plugin-program för Azure Web Apps] dokumentation.</span><span class="sxs-lookup"><span data-stu-id="10190-190">There are several values that you can modify for hello Maven plugin, and a detailed description for each of these elements is available in hello [Maven Plugin for Azure Web Apps] documentation.</span></span> <span data-ttu-id="10190-191">Som som säger att det finns flera värden som är värda markering i den här artikeln:</span><span class="sxs-lookup"><span data-stu-id="10190-191">That being said, there are several values that are worth highlighting in this article:</span></span>

<span data-ttu-id="10190-192">Element</span><span class="sxs-lookup"><span data-stu-id="10190-192">Element</span></span> | <span data-ttu-id="10190-193">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="10190-193">Description</span></span>
---|---|---
`<version>` | <span data-ttu-id="10190-194">Anger hello version av hello [Maven plugin-program för Azure Web Apps].</span><span class="sxs-lookup"><span data-stu-id="10190-194">Specifies hello version of hello [Maven Plugin for Azure Web Apps].</span></span> <span data-ttu-id="10190-195">Du bör kontrollera hello-version som anges i hello [Maven centrala lagringsplatsen](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-webapp-maven-plugin%22) tooensure som du använder hello senaste versionen.</span><span class="sxs-lookup"><span data-stu-id="10190-195">You should check hello version listed in hello [Maven Central Respository](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-webapp-maven-plugin%22) tooensure that you are using hello latest version.</span></span>
`<authentication>` | <span data-ttu-id="10190-196">Anger hello autentiseringsinformationen för Azure, som i det här exemplet innehåller en `<serverId>` element som innehåller `azure-auth`; Maven använder den värdet toolook hello Azure-tjänstens huvudnamn värden i din Maven *settings.xml* fil som du har definierat i en tidigare i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="10190-196">Specifies hello authentication information for Azure, which in this example contains a `<serverId>` element that contains `azure-auth`; Maven uses that value toolook up hello Azure service principal values in your Maven *settings.xml* file, which you defined in an earlier section of this article.</span></span>
`<resourceGroup>` | <span data-ttu-id="10190-197">Anger hello målresursgruppen, vilket är `wingtiptoysresources` i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="10190-197">Specifies hello target resource group, which is `wingtiptoysresources` in this example.</span></span> <span data-ttu-id="10190-198">hello resursgruppen kommer att skapas under distributionen om det inte redan finns.</span><span class="sxs-lookup"><span data-stu-id="10190-198">hello resource group will be created during deployment if it does not already exist.</span></span>
`<appName>` | <span data-ttu-id="10190-199">Anger hello målets namn för ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="10190-199">Specifies hello target name for your web app.</span></span> <span data-ttu-id="10190-200">I det här exemplet är hello målnamnet `maven-linux-app-${maven.build.timestamp}`, där hello `${maven.build.timestamp}` suffix läggs till i det här exemplet tooavoid konflikt.</span><span class="sxs-lookup"><span data-stu-id="10190-200">In this example, hello target name is `maven-linux-app-${maven.build.timestamp}`, where hello `${maven.build.timestamp}` suffix is appended in this example tooavoid conflict.</span></span> <span data-ttu-id="10190-201">(hello tidsstämpeln är valfria och du kan ange en unik sträng för hello programnamn.)</span><span class="sxs-lookup"><span data-stu-id="10190-201">(hello timestamp is optional; you can specify any unique string for hello app name.)</span></span>
`<region>` | <span data-ttu-id="10190-202">Anger hello målregionen, som i det här exemplet är `westus`.</span><span class="sxs-lookup"><span data-stu-id="10190-202">Specifies hello target region, which in this example is `westus`.</span></span> <span data-ttu-id="10190-203">(En fullständig lista finns i hello [Maven plugin-program för Azure Web Apps] dokumentation.)</span><span class="sxs-lookup"><span data-stu-id="10190-203">(A full list is in hello [Maven Plugin for Azure Web Apps] documentation.)</span></span>
`<containerSettings>` | <span data-ttu-id="10190-204">Anger egenskaperna för hello som innehåller hello namn och URL för din behållare.</span><span class="sxs-lookup"><span data-stu-id="10190-204">Specifies hello properties which contain hello name and URL of your container.</span></span>
`<appSettings>` | <span data-ttu-id="10190-205">Anger alla unika inställningar för Maven toouse när du distribuerar din web app tooAzure.</span><span class="sxs-lookup"><span data-stu-id="10190-205">Specifies any unique settings for Maven toouse when deploying your web app tooAzure.</span></span> <span data-ttu-id="10190-206">I det här exemplet en `<property>` elementet innehåller ett namn/värde-par för underordnade element som anger hello port för din app.</span><span class="sxs-lookup"><span data-stu-id="10190-206">In this example, a `<property>` element contains a name/value pair of child elements that specify hello port for your app.</span></span>

> [!NOTE]
>
> <span data-ttu-id="10190-207">portnummer för hello inställningar toochange hello i det här exemplet krävs bara när du ändrar hello port hello standardprincipen.</span><span class="sxs-lookup"><span data-stu-id="10190-207">hello settings toochange hello port number in this example are only necessary when you are changing hello port from hello default.</span></span>
>

1. <span data-ttu-id="10190-208">Från Kommandotolken hello eller terminalfönster som du använde tidigare återskapa hello JAR-filen med Maven om du har gjort några ändringar toohello *pom.xml* filen, till exempel:</span><span class="sxs-lookup"><span data-stu-id="10190-208">From hello command prompt or terminal window that you were using earlier, rebuild hello JAR file using Maven if you made any changes toohello *pom.xml* file; for example:</span></span>
   ```shell
   mvn clean package
   ```

1. <span data-ttu-id="10190-209">Distribuera din web app tooAzure med Maven; Exempel:</span><span class="sxs-lookup"><span data-stu-id="10190-209">Deploy your web app tooAzure by using Maven; for example:</span></span>
   ```shell
   mvn azure-webapp:deploy
   ```

<span data-ttu-id="10190-210">Maven distribuerar din web app tooAzure; Om hello webbapp inte redan finns, skapas.</span><span class="sxs-lookup"><span data-stu-id="10190-210">Maven will deploy your web app tooAzure; if hello web app does not already exist, it will be created.</span></span>

> [!NOTE]
>
> <span data-ttu-id="10190-211">Om hello region som du anger i hello `<region>` element i din *pom.xml* filen har inte tillräckligt många servrar som är tillgängliga när du börjar din distribution, visas ett felmeddelande liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="10190-211">If hello region which you specify in hello `<region>` element of your *pom.xml* file does not have enough servers available when you start your deployment, you might see an error similar toohello following example:</span></span>
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
> <span data-ttu-id="10190-212">Om det händer kan ange du en annan region och kör igen hello Maven-kommando toodeploy ditt program.</span><span class="sxs-lookup"><span data-stu-id="10190-212">If this happens, you can specify another region and re-run hello Maven command toodeploy your application.</span></span>
>
>

<span data-ttu-id="10190-213">När webbplatsen har distribuerats, kommer du att kunna toomanage den med hjälp av hello [Azure-portalen].</span><span class="sxs-lookup"><span data-stu-id="10190-213">When your web has been deployed, you will be able toomanage it by using hello [Azure portal].</span></span>

* <span data-ttu-id="10190-214">Ditt webbprogram visas i **Apptjänster**:</span><span class="sxs-lookup"><span data-stu-id="10190-214">Your web app will be listed in **App Services**:</span></span>

   ![Webbprogram som anges i Azure portal Apptjänster][AP01]

* <span data-ttu-id="10190-216">Och hello URL för ditt webbprogram visas i hello **översikt** för ditt webbprogram:</span><span class="sxs-lookup"><span data-stu-id="10190-216">And hello URL for your web app will be listed in hello **Overview** for your web app:</span></span>

   ![När du fastställer hello URL: en för webbappen][AP02]

## <a name="next-steps"></a><span data-ttu-id="10190-218">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="10190-218">Next steps</span></span>

<span data-ttu-id="10190-219">Mer information om hello finns olika tekniker som beskrivs i den här artikeln hello följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="10190-219">For more information about hello various technologies discussed in this article, see hello following articles:</span></span>

* <span data-ttu-id="10190-220">[Maven-plugin-program för Azure-Webbappar]</span><span class="sxs-lookup"><span data-stu-id="10190-220">[Maven Plugin for Azure Web Apps]</span></span>

* [<span data-ttu-id="10190-221">Logga in tooAzure från hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="10190-221">Log in tooAzure from hello Azure CLI</span></span>](/azure/xplat-cli-connect)

* [<span data-ttu-id="10190-222">Skapa en Azure tjänstens huvudnamn med Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="10190-222">Create an Azure service principal with Azure CLI 2.0</span></span>](/cli/azure/create-an-azure-service-principal-azure-cli)

* [<span data-ttu-id="10190-223">Referens för maven</span><span class="sxs-lookup"><span data-stu-id="10190-223">Maven Settings Reference</span></span>](https://maven.apache.org/settings.html)

* <span data-ttu-id="10190-224">[Docker-plugin för Maven]</span><span class="sxs-lookup"><span data-stu-id="10190-224">[Docker plugin for Maven]</span></span>

<!-- URL List -->

[Azure-kommandoradsgränssnittet (CLI)]: /cli/azure/overview
[Azure Container Service (ACS)]: https://azure.microsoft.com/services/container-service/
[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[Azure Portal]: https://portal.azure.com/
[Maven-plugin-program för Azure-Webbappar]: https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin
[Create a private Docker container registry using hello Azure portal]: /azure/container-registry/container-registry-get-started-portal
[Using a custom Docker image for Azure Web App on Linux]: /azure/app-service-web/app-service-linux-using-custom-docker-image
[Docker]: https://www.docker.com/
[Docker-plugin för Maven]: https://github.com/spotify/docker-maven-plugin
[kostnadsfritt Azure-konto]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[Maven 3.]: http://maven.apache.org/
[MSDN-prenumerantförmåner]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Källan Start]: http://projects.spring.io/spring-boot/
[Källan Start på Docker komma igång]: https://github.com/spring-guides/gs-spring-boot-docker
[Källan Framework]: https://spring.io/

<!-- IMG List -->

[SB01]: ./media/app-service-web-deploy-spring-boot-app-from-container-registry-using-maven-plugin/SB01.png
[CR01]: ./media/app-service-web-deploy-spring-boot-app-from-container-registry-using-maven-plugin/CR01.png
[AP01]: ./media/app-service-web-deploy-spring-boot-app-from-container-registry-using-maven-plugin/AP01.png
[AP02]: ./media/app-service-web-deploy-spring-boot-app-from-container-registry-using-maven-plugin/AP02.png
