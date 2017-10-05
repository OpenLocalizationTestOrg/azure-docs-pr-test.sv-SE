---
title: "Hur du använder Maven-plugin-program för Azure Web Apps för att distribuera en källan Start-app i Azure Container registret till Azure App Service"
description: "Den här självstudiekursen vägleder dig genom stegen för att distribuera en källan startprogrammet i registret för Azure-behållare till Azure till Azure App Service med hjälp av ett Maven plugin-program."
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
ms.openlocfilehash: f47ee59d72ea49d62be2cb435ebaf8bc841e4198
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-use-the-maven-plugin-for-azure-web-apps-to-deploy-a-spring-boot-app-in-azure-container-registry-to-azure-app-service"></a><span data-ttu-id="a8af1-103">Hur du använder Maven-plugin-program för Azure Web Apps för att distribuera en källan Start-app i Azure Container registret till Azure App Service</span><span class="sxs-lookup"><span data-stu-id="a8af1-103">How to use the Maven Plugin for Azure Web Apps to deploy a Spring Boot app in Azure Container Registry to Azure App Service</span></span>

<span data-ttu-id="a8af1-104">Den  **[Vårversionen Framework]**  ett populära ramverk för öppen källkod som gör att Java-utvecklare skapa webb-, mobil- och API-program.</span><span class="sxs-lookup"><span data-stu-id="a8af1-104">The **[Spring Framework]** is a popular open-source framework that helps Java developers create web, mobile, and API applications.</span></span> <span data-ttu-id="a8af1-105">Den här kursen använder en exempelapp som skapats med hjälp av [Vårversionen Start], en konvention inriktat tillvägagångssätt för att komma igång snabbt med källan.</span><span class="sxs-lookup"><span data-stu-id="a8af1-105">This tutorial uses a sample app created using [Spring Boot], a convention-driven approach for using Spring to get started quickly.</span></span>

<span data-ttu-id="a8af1-106">Den här artikeln visar hur du distribuerar ett exempelprogram källan Start till registret för Azure-behållaren och sedan distribuera programmet till Azure App Service med hjälp av plugin-program för Maven för Azure Web Apps.</span><span class="sxs-lookup"><span data-stu-id="a8af1-106">This article demonstrates how to deploy a sample Spring Boot application to Azure Container Registry, and then use the Maven Plugin for Azure Web Apps to deploy your application to Azure App Service.</span></span>

> [!NOTE]
>
> <span data-ttu-id="a8af1-107">Maven-plugin-program för Azure Web Apps är tillgänglig som en förhandsgranskning.</span><span class="sxs-lookup"><span data-stu-id="a8af1-107">The Maven Plugin for Azure Web Apps is currently available as a preview.</span></span> <span data-ttu-id="a8af1-108">För närvarande stöds endast FTP-publicering, även om ytterligare funktioner som är tänkta för framtiden.</span><span class="sxs-lookup"><span data-stu-id="a8af1-108">For now, only FTP publishing is supported, although additional features are planned for the future.</span></span>
>

## <a name="prerequisites"></a><span data-ttu-id="a8af1-109">Krav</span><span class="sxs-lookup"><span data-stu-id="a8af1-109">Prerequisites</span></span>

<span data-ttu-id="a8af1-110">För att kunna slutföra stegen i den här självstudiekursen, måste du ha följande krav:</span><span class="sxs-lookup"><span data-stu-id="a8af1-110">In order to complete the steps in this tutorial, you need to have the following prerequisites:</span></span>

* <span data-ttu-id="a8af1-111">En Azure-prenumeration; Om du inte redan har en Azure-prenumeration, kan du aktivera din [MSDN-prenumerantförmåner] eller registrera dig för en [kostnadsfritt Azure-konto].</span><span class="sxs-lookup"><span data-stu-id="a8af1-111">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="a8af1-112">Den [Azure-kommandoradsgränssnittet (CLI)].</span><span class="sxs-lookup"><span data-stu-id="a8af1-112">The [Azure Command-Line Interface (CLI)].</span></span>
* <span data-ttu-id="a8af1-113">En uppdaterad [Java Development Kit (JDK)], version 1.7 eller senare.</span><span class="sxs-lookup"><span data-stu-id="a8af1-113">An up-to-date [Java Development Kit (JDK)], version 1.7 or later.</span></span>
* <span data-ttu-id="a8af1-114">Apache's [Maven] skapa verktyget (Version 3).</span><span class="sxs-lookup"><span data-stu-id="a8af1-114">Apache's [Maven] build tool (Version 3).</span></span>
* <span data-ttu-id="a8af1-115">En [Git] klienten.</span><span class="sxs-lookup"><span data-stu-id="a8af1-115">A [Git] client.</span></span>
* <span data-ttu-id="a8af1-116">En [Docker] klienten.</span><span class="sxs-lookup"><span data-stu-id="a8af1-116">A [Docker] client.</span></span>

> [!NOTE]
>
> <span data-ttu-id="a8af1-117">På grund av virtualisering kraven i den här självstudiekursen, kan du följa stegen i den här artikeln på en virtuell dator; Du måste använda en fysisk dator med aktiverat funktionerna för virtualisering.</span><span class="sxs-lookup"><span data-stu-id="a8af1-117">Due to the virtualization requirements of this tutorial, you cannot follow the steps in this article on a virtual machine; you must use a physical computer with virtualization features enabled.</span></span>
>

## <a name="clone-the-sample-spring-boot-on-docker-web-app"></a><span data-ttu-id="a8af1-118">Klona exempel källan Start på Docker-webbprogram</span><span class="sxs-lookup"><span data-stu-id="a8af1-118">Clone the sample Spring Boot on Docker web app</span></span>

<span data-ttu-id="a8af1-119">I det här avsnittet klona en container startprogrammet för källan och testa den lokalt.</span><span class="sxs-lookup"><span data-stu-id="a8af1-119">In this section, you clone a containerized Spring Boot application and test it locally.</span></span>

1. <span data-ttu-id="a8af1-120">Öppna en kommandotolk eller ett terminalfönster och skapa en lokal katalog för att hålla tillämpningsprogrammet källan start och ändra till katalogen; Exempel:</span><span class="sxs-lookup"><span data-stu-id="a8af1-120">Open a command prompt or terminal window and create a local directory to hold your Spring Boot application, and change to that directory; for example:</span></span>
   ```shell
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   <span data-ttu-id="a8af1-121">-- eller--</span><span class="sxs-lookup"><span data-stu-id="a8af1-121">-- or --</span></span>
   ```shell
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. <span data-ttu-id="a8af1-122">Klona den [Vårversionen Start på Docker komma igång] exempelprojektet till den katalog som du har skapat, till exempel:</span><span class="sxs-lookup"><span data-stu-id="a8af1-122">Clone the [Spring Boot on Docker Getting Started] sample project into the directory you created; for example:</span></span>
   ```shell
   git clone -b private-registry https://github.com/Microsoft/gs-spring-boot-docker
   ```

1. <span data-ttu-id="a8af1-123">Ändra katalogen till slutförda projektet. Exempel:</span><span class="sxs-lookup"><span data-stu-id="a8af1-123">Change directory to the completed project; for example:</span></span>
   ```shell
   cd gs-spring-boot-docker/complete
   ```

1. <span data-ttu-id="a8af1-124">Skapa JAR-filen med Maven; Exempel:</span><span class="sxs-lookup"><span data-stu-id="a8af1-124">Build the JAR file using Maven; for example:</span></span>
   ```shell
   mvn clean package
   ```

1. <span data-ttu-id="a8af1-125">När webbappen har skapats, starta webbprogram med Maven; Exempel:</span><span class="sxs-lookup"><span data-stu-id="a8af1-125">When the web app has been created, start the web app using Maven; for example:</span></span>
   ```shell
   mvn spring-boot:run
   ```

1. <span data-ttu-id="a8af1-126">Testa webbappen genom att bläddra till den lokalt med hjälp av en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="a8af1-126">Test the web app by browsing to it locally using a web browser.</span></span> <span data-ttu-id="a8af1-127">Du kan exempelvis använda följande kommando om du har curl som är tillgängliga:</span><span class="sxs-lookup"><span data-stu-id="a8af1-127">For example, you could use the following command if you have curl available:</span></span>
   ```shell
   curl http://localhost:8080
   ```

1. <span data-ttu-id="a8af1-128">Du bör se följande meddelande visas: **Hello Docker World**</span><span class="sxs-lookup"><span data-stu-id="a8af1-128">You should see the following message displayed: **Hello Docker World**</span></span>

   ![Bläddra Sample-appen lokalt][SB01]

## <a name="create-an-azure-service-principal"></a><span data-ttu-id="a8af1-130">Skapa en Azure huvudnamn för tjänsten</span><span class="sxs-lookup"><span data-stu-id="a8af1-130">Create an Azure service principal</span></span>

<span data-ttu-id="a8af1-131">I det här avsnittet skapar du en Azure tjänstens huvudnamn som Maven plugin-programmet använder när du distribuerar din behållare till Azure.</span><span class="sxs-lookup"><span data-stu-id="a8af1-131">In this section, you create an Azure service principal that the Maven plugin uses when deploying your container to Azure.</span></span>

1. <span data-ttu-id="a8af1-132">Öppna en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="a8af1-132">Open a command prompt.</span></span>

1. <span data-ttu-id="a8af1-133">Logga in på ditt Azure-konto med hjälp av Azure CLI:</span><span class="sxs-lookup"><span data-stu-id="a8af1-133">Sign into your Azure account by using the Azure CLI:</span></span>
   ```azurecli
   az login
   ```
   <span data-ttu-id="a8af1-134">Följ instruktionerna för att slutföra inloggningen.</span><span class="sxs-lookup"><span data-stu-id="a8af1-134">Follow the instructions to complete the sign-in process.</span></span>

1. <span data-ttu-id="a8af1-135">Skapa en Azure-tjänstens huvudnamn:</span><span class="sxs-lookup"><span data-stu-id="a8af1-135">Create an Azure service principal:</span></span>
   ```azurecli
   az ad sp create-for-rbac --name "uuuuuuuu" --password "pppppppp"
   ```
   <span data-ttu-id="a8af1-136">Där `uuuuuuuu` är användarnamnet och `pppppppp` är lösenordet för tjänstens huvudnamn.</span><span class="sxs-lookup"><span data-stu-id="a8af1-136">Where `uuuuuuuu` is the user name and `pppppppp` is the password for the service principal.</span></span>

1. <span data-ttu-id="a8af1-137">Azure svarar med JSON som liknar följande exempel:</span><span class="sxs-lookup"><span data-stu-id="a8af1-137">Azure responds with JSON that resembles the following example:</span></span>
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
   > <span data-ttu-id="a8af1-138">Du kommer använda värden från den här JSON-svar när du konfigurerar Maven-plugin-programmet för att distribuera din behållare till Azure.</span><span class="sxs-lookup"><span data-stu-id="a8af1-138">You will use the values from this JSON response when you configure the Maven plugin to deploy your container to Azure.</span></span> <span data-ttu-id="a8af1-139">Den `aaaaaaaa`, `uuuuuuuu`, `pppppppp`, och `tttttttt` är för platshållarvärden som används i det här exemplet för att göra det enklare att mappa värdena till deras respektive element när du konfigurerar din Maven `settings.xml` filen i nästa avsnitt .</span><span class="sxs-lookup"><span data-stu-id="a8af1-139">The `aaaaaaaa`, `uuuuuuuu`, `pppppppp`, and `tttttttt` are placeholder values, which are used in this example to make it easier to map these values to their respective elements when you configure your Maven `settings.xml` file in the next section.</span></span>
   >
   >

## <a name="create-an-azure-container-registry-using-the-azure-cli"></a><span data-ttu-id="a8af1-140">Skapa ett Azure Container registret med hjälp av Azure CLI</span><span class="sxs-lookup"><span data-stu-id="a8af1-140">Create an Azure Container Registry using the Azure CLI</span></span>

1. <span data-ttu-id="a8af1-141">Öppna en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="a8af1-141">Open a command prompt.</span></span>

1. <span data-ttu-id="a8af1-142">Logga in på ditt Azure-konto:</span><span class="sxs-lookup"><span data-stu-id="a8af1-142">Log in to your Azure account:</span></span>
   ```azurecli
   az login
   ```

1. <span data-ttu-id="a8af1-143">Skapa en resursgrupp för Azure-resurser du vill använda i den här artikeln:</span><span class="sxs-lookup"><span data-stu-id="a8af1-143">Create a resource group for the Azure resources you will use in this article:</span></span>
   ```azurecli
   az group create --name=wingtiptoysresources --location=westus
   ```
   <span data-ttu-id="a8af1-144">Ersätt `wingtiptoysresources` i det här exemplet med ett unikt namn för resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="a8af1-144">Replace `wingtiptoysresources` in this example with a unique name for your resource group.</span></span>

1. <span data-ttu-id="a8af1-145">Skapa ett register för privat Azure-behållaren i resursgruppen för vår Start appen:</span><span class="sxs-lookup"><span data-stu-id="a8af1-145">Create a private Azure container registry in the resource group for your Spring Boot app:</span></span> 
   ```azurecli
   az acr create --admin-enabled --resource-group wingtiptoysresources --location westus --name wingtiptoysregistry --sku Basic
   ```
   <span data-ttu-id="a8af1-146">Ersätt `wingtiptoysregistry` i det här exemplet med ett unikt namn för behållaren registret.</span><span class="sxs-lookup"><span data-stu-id="a8af1-146">Replace `wingtiptoysregistry` in this example with a unique name for your container registry.</span></span>

1. <span data-ttu-id="a8af1-147">Hämta lösenordet för behållaren registret:</span><span class="sxs-lookup"><span data-stu-id="a8af1-147">Retrieve the password for your container registry:</span></span>
   ```azurecli
   az acr credential show --name wingtiptoysregistry --query passwords[0]
   ```
   <span data-ttu-id="a8af1-148">Azure kommer att svara med ditt lösenord. Exempel:</span><span class="sxs-lookup"><span data-stu-id="a8af1-148">Azure will respond with your password; for example:</span></span>
   ```json
   {
      "name": "password",
      "value": "xxxxxxxxxx"
   }
   ```

## <a name="add-your-azure-container-registry-and-azure-service-principal-to-your-maven-settings"></a><span data-ttu-id="a8af1-149">Lägg till Azure-behållaren registret och Azure tjänstens huvudnamn Maven-inställningar</span><span class="sxs-lookup"><span data-stu-id="a8af1-149">Add your Azure container registry and Azure service principal to your Maven settings</span></span>

1. <span data-ttu-id="a8af1-150">Öppna din Maven `settings.xml` filen i en textredigerare; den här filen kan vara en sökväg som i följande exempel:</span><span class="sxs-lookup"><span data-stu-id="a8af1-150">Open your Maven `settings.xml` file in a text editor; this file might be in a path like the following examples:</span></span>
   * `/etc/maven/settings.xml`
   * `%ProgramFiles%\apache-maven\3.5.0\conf\settings.xml`
   * `$HOME/.m2/settings.xml`

1. <span data-ttu-id="a8af1-151">Lägga till din Azure-behållare åtkomst registerinställningar från föregående avsnitt i den här artikeln i `<servers>` samling i den *settings.xml* filen, till exempel:</span><span class="sxs-lookup"><span data-stu-id="a8af1-151">Add your Azure Container Registry access settings from the previous section of this article to the `<servers>` collection in the *settings.xml* file; for example:</span></span>

   ```xml
   <servers>
      <server>
         <id>wingtiptoysregistry</id>
         <username>wingtiptoysregistry</username>
         <password>xxxxxxxxxx</password>
      </server>
   </servers>
   ```
   <span data-ttu-id="a8af1-152">Där:</span><span class="sxs-lookup"><span data-stu-id="a8af1-152">Where:</span></span>
   <span data-ttu-id="a8af1-153">Element</span><span class="sxs-lookup"><span data-stu-id="a8af1-153">Element</span></span> | <span data-ttu-id="a8af1-154">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a8af1-154">Description</span></span>
   ---|---|---
   `<id>` | <span data-ttu-id="a8af1-155">Innehåller namnet på behållaren för privat Azure-registret.</span><span class="sxs-lookup"><span data-stu-id="a8af1-155">Contains the name of your private Azure container registry.</span></span>
   `<username>` | <span data-ttu-id="a8af1-156">Innehåller namnet på behållaren för privat Azure-registret.</span><span class="sxs-lookup"><span data-stu-id="a8af1-156">Contains the name of your private Azure container registry.</span></span>
   `<password>` | <span data-ttu-id="a8af1-157">Innehåller det lösenord som du hämtade i föregående avsnitt i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="a8af1-157">Contains the password you retrieved in the previous section of this article.</span></span>

1. <span data-ttu-id="a8af1-158">Lägg till Azure-tjänstens huvudnamn inställningarna från ett tidigare avsnitt i den här artikeln i `<servers>` samling i den *settings.xml* filen, till exempel:</span><span class="sxs-lookup"><span data-stu-id="a8af1-158">Add your Azure service principal settings from an earlier section of this article to the `<servers>` collection in the *settings.xml* file; for example:</span></span>

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
   <span data-ttu-id="a8af1-159">Där:</span><span class="sxs-lookup"><span data-stu-id="a8af1-159">Where:</span></span>
   <span data-ttu-id="a8af1-160">Element</span><span class="sxs-lookup"><span data-stu-id="a8af1-160">Element</span></span> | <span data-ttu-id="a8af1-161">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a8af1-161">Description</span></span>
   ---|---|---
   `<id>` | <span data-ttu-id="a8af1-162">Anger ett unikt namn som Maven använder för att leta upp dina säkerhetsinställningar när du distribuerar ditt webbprogram till Azure.</span><span class="sxs-lookup"><span data-stu-id="a8af1-162">Specifies a unique name which Maven uses to look up your security settings when you deploy your web app to Azure.</span></span>
   `<client>` | <span data-ttu-id="a8af1-163">Innehåller den `appId` värde från tjänstens huvudnamn.</span><span class="sxs-lookup"><span data-stu-id="a8af1-163">Contains the `appId` value from your service principal.</span></span>
   `<tenant>` | <span data-ttu-id="a8af1-164">Innehåller den `tenant` värde från tjänstens huvudnamn.</span><span class="sxs-lookup"><span data-stu-id="a8af1-164">Contains the `tenant` value from your service principal.</span></span>
   `<key>` | <span data-ttu-id="a8af1-165">Innehåller den `password` värde från tjänstens huvudnamn.</span><span class="sxs-lookup"><span data-stu-id="a8af1-165">Contains the `password` value from your service principal.</span></span>
   `<environment>` | <span data-ttu-id="a8af1-166">Definierar målmiljön Azure-molnet, vilket är `AZURE` i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="a8af1-166">Defines the target Azure cloud environment, which is `AZURE` in this example.</span></span> <span data-ttu-id="a8af1-167">(En fullständig lista över miljöer finns i den [Maven plugin-program för Azure Web Apps] dokumentation)</span><span class="sxs-lookup"><span data-stu-id="a8af1-167">(A full list of environments is available in the [Maven Plugin for Azure Web Apps] documentation)</span></span>

1. <span data-ttu-id="a8af1-168">Spara och Stäng den *settings.xml* fil.</span><span class="sxs-lookup"><span data-stu-id="a8af1-168">Save and close the *settings.xml* file.</span></span>

## <a name="build-your-docker-container-image-and-push-it-to-your-azure-container-registry"></a><span data-ttu-id="a8af1-169">Skapa din Docker behållaren image och push-installera den Azure-behållaren registret</span><span class="sxs-lookup"><span data-stu-id="a8af1-169">Build your Docker container image and push it to your Azure container registry</span></span>

1. <span data-ttu-id="a8af1-170">Navigera till slutförda projektkatalogen för tillämpningsprogrammet källan start (t.ex.) ”*C:\SpringBoot\gs-spring-boot-docker\complete*” eller ”*/users/robert/SpringBoot/gs-spring-boot-docker/complete*”), och öppna den *pom.xml* fil med en text redigeraren.</span><span class="sxs-lookup"><span data-stu-id="a8af1-170">Navigate to the completed project directory for your Spring Boot application, (e.g. "*C:\SpringBoot\gs-spring-boot-docker\complete*" or "*/users/robert/SpringBoot/gs-spring-boot-docker/complete*"), and open the *pom.xml* file with a text editor.</span></span>

1. <span data-ttu-id="a8af1-171">Uppdatering av `<properties>` samling i den *pom.xml* fil med inloggningen servervärdet för registernyckeln din Azure-behållare från föregående avsnitt i den här kursen; till exempel:</span><span class="sxs-lookup"><span data-stu-id="a8af1-171">Update the `<properties>` collection in the *pom.xml* file with the login server value for your Azure Container Registry from the previous section of this tutorial; for example:</span></span>

   ```xml
   <properties>
      <azure.containerRegistry>wingtiptoysregistry</azure.containerRegistry>
      <docker.image.prefix>${azure.containerRegistry}.azurecr.io</docker.image.prefix>
      <java.version>1.8</java.version>
      <maven.build.timestamp.format>yyyyMMddHHmmssSSS</maven.build.timestamp.format>
   </properties>
   ```
   <span data-ttu-id="a8af1-172">Där:</span><span class="sxs-lookup"><span data-stu-id="a8af1-172">Where:</span></span>
   <span data-ttu-id="a8af1-173">Element</span><span class="sxs-lookup"><span data-stu-id="a8af1-173">Element</span></span> | <span data-ttu-id="a8af1-174">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a8af1-174">Description</span></span>
   ---|---|---
   `<azure.containerRegistry>` | <span data-ttu-id="a8af1-175">Anger namnet på behållaren för privat Azure-registret.</span><span class="sxs-lookup"><span data-stu-id="a8af1-175">Specifies the name of your private Azure container registry.</span></span>
   `<docker.image.prefix>` | <span data-ttu-id="a8af1-176">Anger Webbadressen för privat Azure-behållaren registret, som hämtas genom att lägga till ”. azurecr.io” till namnet på din privata behållare registret.</span><span class="sxs-lookup"><span data-stu-id="a8af1-176">Specifies the URL of your private Azure container registry, which is derived by appending ".azurecr.io" to the name of your private container registry.</span></span>

1. <span data-ttu-id="a8af1-177">Kontrollera att `<plugin>` för Docker-plugin-programmet i din *pom.xml* filen innehåller rätt egenskaper för server-adress och registret inloggningsnamnet från föregående steg i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="a8af1-177">Verify that `<plugin>` for the Docker plugin in your *pom.xml* file contains the correct properties for the login server address and registry name from the previous step in this tutorial.</span></span> <span data-ttu-id="a8af1-178">Exempel:</span><span class="sxs-lookup"><span data-stu-id="a8af1-178">For example:</span></span>

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
   <span data-ttu-id="a8af1-179">Där:</span><span class="sxs-lookup"><span data-stu-id="a8af1-179">Where:</span></span>
   <span data-ttu-id="a8af1-180">Element</span><span class="sxs-lookup"><span data-stu-id="a8af1-180">Element</span></span> | <span data-ttu-id="a8af1-181">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a8af1-181">Description</span></span>
   ---|---|---
   `<serverId>` | <span data-ttu-id="a8af1-182">Anger den egenskap som innehåller namnet på behållaren för privat Azure-registret.</span><span class="sxs-lookup"><span data-stu-id="a8af1-182">Specifies the property which contains name of your private Azure container registry.</span></span>
   `<registryUrl>` | <span data-ttu-id="a8af1-183">Anger den egenskap som innehåller URL för privat Azure-behållaren registret.</span><span class="sxs-lookup"><span data-stu-id="a8af1-183">Specifies the property which contains the URL of your private Azure container registry.</span></span>

1. <span data-ttu-id="a8af1-184">Navigera till slutförda projektkatalogen för tillämpningsprogrammet källan start och kör följande kommando för att återskapa programmet och push-behållaren i Azure-behållaren registret:</span><span class="sxs-lookup"><span data-stu-id="a8af1-184">Navigate to the completed project directory for your Spring Boot application and run the following command to rebuild the application and push the container to your Azure container registry:</span></span>

   ```
   mvn package docker:build -DpushImage 
   ```

1. <span data-ttu-id="a8af1-185">Valfritt: Bläddra till den [Azure-portalen] och kontrollera att det finns Docker behållare bild med namnet **gs-källan-Start-docker** i behållaren-registret.</span><span class="sxs-lookup"><span data-stu-id="a8af1-185">OPTIONAL: Browse to the [Azure portal] and verify that there is Docker container image named **gs-spring-boot-docker** in your container registry.</span></span>

   ![Kontrollera behållare i Azure-portalen][CR01]

## <a name="customize-your-pomxml-then-build-and-deploy-your-container-to-azure"></a><span data-ttu-id="a8af1-187">Anpassa din pom.xml och sedan skapa och distribuera din behållare till Azure</span><span class="sxs-lookup"><span data-stu-id="a8af1-187">Customize your pom.xml, then build and deploy your container to Azure</span></span>

<span data-ttu-id="a8af1-188">Öppna den `pom.xml` filen för tillämpningsprogrammet källan Start i en textredigerare och leta upp den `<plugin>` element för `azure-webapp-maven-plugin`.</span><span class="sxs-lookup"><span data-stu-id="a8af1-188">Open the `pom.xml` file for your Spring Boot application in a text editor, and then locate the `<plugin>` element for `azure-webapp-maven-plugin`.</span></span> <span data-ttu-id="a8af1-189">Det här elementet bör likna följande exempel:</span><span class="sxs-lookup"><span data-stu-id="a8af1-189">This element should resemble the following example:</span></span>

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

<span data-ttu-id="a8af1-190">Det finns flera värden som du kan ändra för Maven plugin-programmet och en utförlig beskrivning för var och en av dessa element är tillgänglig i den [Maven plugin-program för Azure Web Apps] dokumentation.</span><span class="sxs-lookup"><span data-stu-id="a8af1-190">There are several values that you can modify for the Maven plugin, and a detailed description for each of these elements is available in the [Maven Plugin for Azure Web Apps] documentation.</span></span> <span data-ttu-id="a8af1-191">Som som säger att det finns flera värden som är värda markering i den här artikeln:</span><span class="sxs-lookup"><span data-stu-id="a8af1-191">That being said, there are several values that are worth highlighting in this article:</span></span>

<span data-ttu-id="a8af1-192">Element</span><span class="sxs-lookup"><span data-stu-id="a8af1-192">Element</span></span> | <span data-ttu-id="a8af1-193">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a8af1-193">Description</span></span>
---|---|---
`<version>` | <span data-ttu-id="a8af1-194">Anger versionen av den [Maven plugin-program för Azure Web Apps].</span><span class="sxs-lookup"><span data-stu-id="a8af1-194">Specifies the version of the [Maven Plugin for Azure Web Apps].</span></span> <span data-ttu-id="a8af1-195">Du bör kontrollera versionen som anges i den [Maven centrala lagringsplatsen](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-webapp-maven-plugin%22) så att du använder den senaste versionen.</span><span class="sxs-lookup"><span data-stu-id="a8af1-195">You should check the version listed in the [Maven Central Respository](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-webapp-maven-plugin%22) to ensure that you are using the latest version.</span></span>
`<authentication>` | <span data-ttu-id="a8af1-196">Anger autentiseringsinformationen för Azure, som i det här exemplet innehåller en `<serverId>` element som innehåller `azure-auth`; Maven används värdet för att slå upp Azure tjänstens huvudnamn värden i din Maven *settings.xml* fil som du har definierat i en tidigare i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="a8af1-196">Specifies the authentication information for Azure, which in this example contains a `<serverId>` element that contains `azure-auth`; Maven uses that value to look up the Azure service principal values in your Maven *settings.xml* file, which you defined in an earlier section of this article.</span></span>
`<resourceGroup>` | <span data-ttu-id="a8af1-197">Anger målresursgruppen som är `wingtiptoysresources` i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="a8af1-197">Specifies the target resource group, which is `wingtiptoysresources` in this example.</span></span> <span data-ttu-id="a8af1-198">Resursgruppens namn kommer att skapas under distributionen om det inte redan finns.</span><span class="sxs-lookup"><span data-stu-id="a8af1-198">The resource group will be created during deployment if it does not already exist.</span></span>
`<appName>` | <span data-ttu-id="a8af1-199">Anger målnamn för ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="a8af1-199">Specifies the target name for your web app.</span></span> <span data-ttu-id="a8af1-200">I det här exemplet är målnamnet `maven-linux-app-${maven.build.timestamp}`, där den `${maven.build.timestamp}` suffix läggs till i det här exemplet för att undvika konflikter.</span><span class="sxs-lookup"><span data-stu-id="a8af1-200">In this example, the target name is `maven-linux-app-${maven.build.timestamp}`, where the `${maven.build.timestamp}` suffix is appended in this example to avoid conflict.</span></span> <span data-ttu-id="a8af1-201">(Tidsstämpeln är valfria och du kan ange en unik sträng för namnet på appen.)</span><span class="sxs-lookup"><span data-stu-id="a8af1-201">(The timestamp is optional; you can specify any unique string for the app name.)</span></span>
`<region>` | <span data-ttu-id="a8af1-202">Anger målregionen, som i det här exemplet är `westus`.</span><span class="sxs-lookup"><span data-stu-id="a8af1-202">Specifies the target region, which in this example is `westus`.</span></span> <span data-ttu-id="a8af1-203">(En fullständig lista finns i den [Maven plugin-program för Azure Web Apps] dokumentation.)</span><span class="sxs-lookup"><span data-stu-id="a8af1-203">(A full list is in the [Maven Plugin for Azure Web Apps] documentation.)</span></span>
`<containerSettings>` | <span data-ttu-id="a8af1-204">Anger de egenskaper som innehåller namn och URL för din behållare.</span><span class="sxs-lookup"><span data-stu-id="a8af1-204">Specifies the properties which contain the name and URL of your container.</span></span>
`<appSettings>` | <span data-ttu-id="a8af1-205">Anger alla unika inställningar för Maven ska användas när du distribuerar ditt webbprogram till Azure.</span><span class="sxs-lookup"><span data-stu-id="a8af1-205">Specifies any unique settings for Maven to use when deploying your web app to Azure.</span></span> <span data-ttu-id="a8af1-206">I det här exemplet en `<property>` elementet innehåller ett namn/värde-par för underordnade element som anger porten för din app.</span><span class="sxs-lookup"><span data-stu-id="a8af1-206">In this example, a `<property>` element contains a name/value pair of child elements that specify the port for your app.</span></span>

> [!NOTE]
>
> <span data-ttu-id="a8af1-207">Inställningar för att ändra portnumret i det här exemplet är bara nödvändigt när du ändrar porten från standard.</span><span class="sxs-lookup"><span data-stu-id="a8af1-207">The settings to change the port number in this example are only necessary when you are changing the port from the default.</span></span>
>

1. <span data-ttu-id="a8af1-208">Från Kommandotolken eller terminalfönster som du använde tidigare, återskapa JAR-filen med Maven om du har gjort ändringar i *pom.xml* filen, till exempel:</span><span class="sxs-lookup"><span data-stu-id="a8af1-208">From the command prompt or terminal window that you were using earlier, rebuild the JAR file using Maven if you made any changes to the *pom.xml* file; for example:</span></span>
   ```shell
   mvn clean package
   ```

1. <span data-ttu-id="a8af1-209">Distribuera webbappen till Azure med hjälp av Maven; Exempel:</span><span class="sxs-lookup"><span data-stu-id="a8af1-209">Deploy your web app to Azure by using Maven; for example:</span></span>
   ```shell
   mvn azure-webapp:deploy
   ```

<span data-ttu-id="a8af1-210">Maven ska distribuera webbappen till Azure; Om webbappen inte redan finns, skapas.</span><span class="sxs-lookup"><span data-stu-id="a8af1-210">Maven will deploy your web app to Azure; if the web app does not already exist, it will be created.</span></span>

> [!NOTE]
>
> <span data-ttu-id="a8af1-211">Om den region som du anger i den `<region>` element i din *pom.xml* filen har inte tillräckligt många servrar som är tillgängliga när du börjar din distribution, kan uppstå ett fel som liknar följande exempel:</span><span class="sxs-lookup"><span data-stu-id="a8af1-211">If the region which you specify in the `<region>` element of your *pom.xml* file does not have enough servers available when you start your deployment, you might see an error similar to the following example:</span></span>
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
> <span data-ttu-id="a8af1-212">Om det händer kan du ange en annan region och kör igen Maven-kommando för att distribuera ditt program.</span><span class="sxs-lookup"><span data-stu-id="a8af1-212">If this happens, you can specify another region and re-run the Maven command to deploy your application.</span></span>
>
>

<span data-ttu-id="a8af1-213">När webbplatsen har distribuerats, kommer du att kunna hantera den med hjälp av den [Azure-portalen].</span><span class="sxs-lookup"><span data-stu-id="a8af1-213">When your web has been deployed, you will be able to manage it by using the [Azure portal].</span></span>

* <span data-ttu-id="a8af1-214">Ditt webbprogram visas i **Apptjänster**:</span><span class="sxs-lookup"><span data-stu-id="a8af1-214">Your web app will be listed in **App Services**:</span></span>

   ![Webbprogram som anges i Azure portal Apptjänster][AP01]

* <span data-ttu-id="a8af1-216">Och URL: en för ditt webbprogram kommer att visas i den **översikt** för ditt webbprogram:</span><span class="sxs-lookup"><span data-stu-id="a8af1-216">And the URL for your web app will be listed in the **Overview** for your web app:</span></span>

   ![Bestämma URL: en för webbappen][AP02]

## <a name="next-steps"></a><span data-ttu-id="a8af1-218">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a8af1-218">Next steps</span></span>

<span data-ttu-id="a8af1-219">Mer information om de olika tekniker som beskrivs i den här artikeln finns i följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="a8af1-219">For more information about the various technologies discussed in this article, see the following articles:</span></span>

* <span data-ttu-id="a8af1-220">[Maven plugin-program för Azure Web Apps]</span><span class="sxs-lookup"><span data-stu-id="a8af1-220">[Maven Plugin for Azure Web Apps]</span></span>

* [<span data-ttu-id="a8af1-221">Logga in till Azure från Azure CLI</span><span class="sxs-lookup"><span data-stu-id="a8af1-221">Log in to Azure from the Azure CLI</span></span>](/azure/xplat-cli-connect)

* [<span data-ttu-id="a8af1-222">Skapa en Azure tjänstens huvudnamn med Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="a8af1-222">Create an Azure service principal with Azure CLI 2.0</span></span>](/cli/azure/create-an-azure-service-principal-azure-cli)

* [<span data-ttu-id="a8af1-223">Referens för maven</span><span class="sxs-lookup"><span data-stu-id="a8af1-223">Maven Settings Reference</span></span>](https://maven.apache.org/settings.html)

* <span data-ttu-id="a8af1-224">[Docker-plugin för Maven]</span><span class="sxs-lookup"><span data-stu-id="a8af1-224">[Docker plugin for Maven]</span></span>

<!-- URL List -->

<span data-ttu-id="a8af1-225">[Azure-kommandoradsgränssnittet (CLI)]: /cli/azure/overview</span><span class="sxs-lookup"><span data-stu-id="a8af1-225">[Azure Command-Line Interface (CLI)]: /cli/azure/overview</span></span>
[Azure Container Service (ACS)]: https://azure.microsoft.com/services/container-service/
[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
<span data-ttu-id="a8af1-226">[Azure-portalen]: https://portal.azure.com/</span><span class="sxs-lookup"><span data-stu-id="a8af1-226">[Azure portal]: https://portal.azure.com/</span></span>
<span data-ttu-id="a8af1-227">[Maven plugin-program för Azure Web Apps]: https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin</span><span class="sxs-lookup"><span data-stu-id="a8af1-227">[Maven Plugin for Azure Web Apps]: https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin</span></span>
[Create a private Docker container registry using the Azure portal]: /azure/container-registry/container-registry-get-started-portal
[Using a custom Docker image for Azure Web App on Linux]: /azure/app-service-web/app-service-linux-using-custom-docker-image
<span data-ttu-id="a8af1-228">[Docker]: https://www.docker.com/</span><span class="sxs-lookup"><span data-stu-id="a8af1-228">[Docker]: https://www.docker.com/</span></span>
<span data-ttu-id="a8af1-229">[Docker-plugin för Maven]: https://github.com/spotify/docker-maven-plugin</span><span class="sxs-lookup"><span data-stu-id="a8af1-229">[Docker plugin for Maven]: https://github.com/spotify/docker-maven-plugin</span></span>
<span data-ttu-id="a8af1-230">[kostnadsfritt Azure-konto]: https://azure.microsoft.com/pricing/free-trial/</span><span class="sxs-lookup"><span data-stu-id="a8af1-230">[free Azure account]: https://azure.microsoft.com/pricing/free-trial/</span></span>
<span data-ttu-id="a8af1-231">[Git]: https://github.com/</span><span class="sxs-lookup"><span data-stu-id="a8af1-231">[Git]: https://github.com/</span></span>
[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
<span data-ttu-id="a8af1-232">[Maven]: http://maven.apache.org/</span><span class="sxs-lookup"><span data-stu-id="a8af1-232">[Maven]: http://maven.apache.org/</span></span>
<span data-ttu-id="a8af1-233">[MSDN-prenumerantförmåner]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/</span><span class="sxs-lookup"><span data-stu-id="a8af1-233">[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/</span></span>
<span data-ttu-id="a8af1-234">[Vårversionen Start]: http://projects.spring.io/spring-boot/</span><span class="sxs-lookup"><span data-stu-id="a8af1-234">[Spring Boot]: http://projects.spring.io/spring-boot/</span></span>
<span data-ttu-id="a8af1-235">[Vårversionen Start på Docker komma igång]: https://github.com/spring-guides/gs-spring-boot-docker</span><span class="sxs-lookup"><span data-stu-id="a8af1-235">[Spring Boot on Docker Getting Started]: https://github.com/spring-guides/gs-spring-boot-docker</span></span>
<span data-ttu-id="a8af1-236">[Vårversionen Framework]: https://spring.io/</span><span class="sxs-lookup"><span data-stu-id="a8af1-236">[Spring Framework]: https://spring.io/</span></span>

<!-- IMG List -->

[SB01]: ./media/app-service-web-deploy-spring-boot-app-from-container-registry-using-maven-plugin/SB01.png
[CR01]: ./media/app-service-web-deploy-spring-boot-app-from-container-registry-using-maven-plugin/CR01.png
[AP01]: ./media/app-service-web-deploy-spring-boot-app-from-container-registry-using-maven-plugin/AP01.png
[AP02]: ./media/app-service-web-deploy-spring-boot-app-from-container-registry-using-maven-plugin/AP02.png
