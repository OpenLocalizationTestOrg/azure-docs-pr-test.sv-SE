---
title: "Så här konfigurerar du en källan Start initieraren app om du vill använda Redis-Cache"
description: "Lär dig hur du konfigurerar en källan startprogrammet skapats med vår Initializr att använda Azure Redis-Cache."
services: redis-cache
documentationcenter: java
author: rmcmurray
manager: cfowler
editor: 
keywords: "Källan källan Start Starter Redis-Cache"
ms.assetid: 
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: java
ms.topic: article
ms.date: 7/21/2017
ms.author: robmcm;zhijzhao;yidon
ms.openlocfilehash: fb3fc96a2136b7c326bb0eb291b7204e7acf0190
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-configure-a-spring-boot-initializer-app-to-use-redis-cache"></a><span data-ttu-id="a8b0e-104">Så här konfigurerar du en källan Start initieraren app om du vill använda Redis-Cache</span><span class="sxs-lookup"><span data-stu-id="a8b0e-104">How to configure a Spring Boot Initializer app to use Redis Cache</span></span>

## <a name="overview"></a><span data-ttu-id="a8b0e-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="a8b0e-105">Overview</span></span>

<span data-ttu-id="a8b0e-106">Den  **[Vårversionen Framework]**  är en lösning för öppen källkod som hjälper Java-utvecklare som skapar program på företagsnivå.</span><span class="sxs-lookup"><span data-stu-id="a8b0e-106">The **[Spring Framework]** is an open-source solution which helps Java developers create enterprise-level applications.</span></span> <span data-ttu-id="a8b0e-107">En av de mer populära projekt som är inbyggd som plattform är [Vårversionen Start], vilket ger en förenklad metod för att skapa fristående Java-program.</span><span class="sxs-lookup"><span data-stu-id="a8b0e-107">One of the more-popular projects which is built on top of that platform is [Spring Boot], which provides a simplified approach for creating stand-alone Java applications.</span></span> <span data-ttu-id="a8b0e-108">Om du vill hjälpa utvecklare att komma igång med vår Start flera exempel källan Start paket finns på <https://github.com/spring-guides/>.</span><span class="sxs-lookup"><span data-stu-id="a8b0e-108">To help developers get started with Spring Boot, several sample Spring Boot packages are available at <https://github.com/spring-guides/>.</span></span> <span data-ttu-id="a8b0e-109">Förutom att välja från listan över grundläggande källan Start-projekt i  **[Vårversionen Initializr]**  gör att utvecklare kan komma igång med att skapa anpassade källan startprogram.</span><span class="sxs-lookup"><span data-stu-id="a8b0e-109">In addition to choosing from the list of basic Spring Boot projects, the **[Spring Initializr]** helps developers get started with creating custom Spring Boot applications.</span></span>

<span data-ttu-id="a8b0e-110">Den här artikeln vägleder dig genom att skapa ett Redis-cache med hjälp av Azure-portalen med hjälp av den **Vårversionen Initializr** att skapa ett anpassat program och sedan skapa en Java-webbapp som lagrar och hämtar data med hjälp av din Redis cache.</span><span class="sxs-lookup"><span data-stu-id="a8b0e-110">This article walks you through creating a Redis cache using the Azure portal, then using the **Spring Initializr** to create a custom application, and then creating a Java web application which stores and retrieves data using your Redis cache.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a8b0e-111">Krav</span><span class="sxs-lookup"><span data-stu-id="a8b0e-111">Prerequisites</span></span>

<span data-ttu-id="a8b0e-112">Följande krav är nödvändiga för att följa stegen i den här artikeln:</span><span class="sxs-lookup"><span data-stu-id="a8b0e-112">The following prerequisites are required in order to follow the steps in this article:</span></span>

* <span data-ttu-id="a8b0e-113">En Azure-prenumeration; Om du inte redan har en Azure-prenumeration, kan du aktivera din [MSDN-prenumerantförmåner] eller registrera dig för en [kostnadsfritt Azure-konto].</span><span class="sxs-lookup"><span data-stu-id="a8b0e-113">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>

* <span data-ttu-id="a8b0e-114">En [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/), 1,7 eller senare.</span><span class="sxs-lookup"><span data-stu-id="a8b0e-114">A [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/), version 1.7 or later.</span></span>

* <span data-ttu-id="a8b0e-115">[Apache Maven](http://maven.apache.org/), version 3.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="a8b0e-115">[Apache Maven](http://maven.apache.org/), version 3.0 or later.</span></span>

## <a name="create-a-redis-cache-on-azure"></a><span data-ttu-id="a8b0e-116">Skapa en Redis-cache på Azure</span><span class="sxs-lookup"><span data-stu-id="a8b0e-116">Create a Redis cache on Azure</span></span>

1. <span data-ttu-id="a8b0e-117">Bläddra till den Azure-portalen på <https://portal.azure.com/> och klicka på det **+ ny**.</span><span class="sxs-lookup"><span data-stu-id="a8b0e-117">Browse to the Azure portal at <https://portal.azure.com/> and click the item for **+New**.</span></span>

   ![Azure Portal][AZ01]

1. <span data-ttu-id="a8b0e-119">Klicka på **databasen**, och klicka sedan på **Redis-Cache**.</span><span class="sxs-lookup"><span data-stu-id="a8b0e-119">Click **Database**, and then click **Redis Cache**.</span></span>

   ![Azure Portal][AZ02]

1. <span data-ttu-id="a8b0e-121">På den **nytt Redis-Cache** anger du den **DNS-namnet** för ditt cacheminne, ange din **prenumeration**, **resursgruppen**, **Plats**, och **prisnivå**.</span><span class="sxs-lookup"><span data-stu-id="a8b0e-121">On the **New Redis Cache** page, enter the **DNS name** for your cache, then specify your **Subscription**, **Resource group**, **Location**, and **Pricing tier**.</span></span> <span data-ttu-id="a8b0e-122">När du har angett dessa alternativ, klickar du på **skapa** att skapa ditt cacheminne.</span><span class="sxs-lookup"><span data-stu-id="a8b0e-122">When you have specified these options, click **Create** to create your cache.</span></span>

   ![Azure Portal][AZ03]

1. <span data-ttu-id="a8b0e-124">När ditt cacheminne har slutförts, visas den på din Azure **instrumentpanelen**, samt enligt den **alla resurser**, och **Redis-cache** sidor.</span><span class="sxs-lookup"><span data-stu-id="a8b0e-124">Once your cache has been completed, you will see it listed on your Azure **Dashboard**, as well as under the **All Resources**, and **Redis Caches** pages.</span></span> <span data-ttu-id="a8b0e-125">Du kan klicka på ditt cacheminne på någon av dessa platser för att öppna egenskapssidan för ditt cacheminne.</span><span class="sxs-lookup"><span data-stu-id="a8b0e-125">You can click on your cache on any of those locations to open the properties page for your cache.</span></span>

   ![Azure Portal][AZ04]

1. <span data-ttu-id="a8b0e-127">När sidan som innehåller listan över egenskaper för ditt cacheminne visas, klickar på **åtkomstnycklar** och kopiera dina åtkomstnycklar för ditt cacheminne.</span><span class="sxs-lookup"><span data-stu-id="a8b0e-127">When the page which contains the list of properties for your cache is displayed, click **Access keys** and copy your access keys for your cache.</span></span>

   ![Azure Portal][AZ05]

## <a name="create-a-custom-application-using-the-spring-initializr"></a><span data-ttu-id="a8b0e-129">Skapa ett anpassat program med hjälp av vår Initializr</span><span class="sxs-lookup"><span data-stu-id="a8b0e-129">Create a custom application using the Spring Initializr</span></span>

1. <span data-ttu-id="a8b0e-130">Bläddra till <https://start.spring.io/>.</span><span class="sxs-lookup"><span data-stu-id="a8b0e-130">Browse to <https://start.spring.io/>.</span></span>

1. <span data-ttu-id="a8b0e-131">Ange att du vill skapa en **Maven** projektet med **Java**, ange den **grupp** och **Aritifact** namn för ditt program och Klicka på länken till **växla till den fullständiga versionen** av vår Initializr.</span><span class="sxs-lookup"><span data-stu-id="a8b0e-131">Specify that you want to generate a **Maven** project with **Java**, enter the **Group** and **Aritifact** names for your application, and then click the link to **Switch to the full version** of the Spring Initializr.</span></span>

   ![Basic-källan Initializr alternativ][SI01]

   > [!NOTE]
   >
   > <span data-ttu-id="a8b0e-133">Källan Initializr kommer att använda den **grupp** och **Aritifact** namn för att skapa paketnamn, till exempel: *com.contoso.myazuredemo*.</span><span class="sxs-lookup"><span data-stu-id="a8b0e-133">The Spring Initializr will use the **Group** and **Aritifact** names to create the package name; for example: *com.contoso.myazuredemo*.</span></span>
   >

1. <span data-ttu-id="a8b0e-134">Rulla ned till den **Web** avsnittet och markera kryssrutan för **Web**, bläddra till den **NoSQL** avsnittet och markera kryssrutan för **Redis**, sedan Bläddra längst ned på sidan och klicka på knappen för att **generera projekt**.</span><span class="sxs-lookup"><span data-stu-id="a8b0e-134">Scroll down to the **Web** section and check the box for **Web**, then scroll down to the **NoSQL** section and check the box for **Redis**, then scroll to the bottom of the page and click the button to **Generate Project**.</span></span>

   ![Alternativ för fullständig Vårversionen Initializr][SI02]

1. <span data-ttu-id="a8b0e-136">När du uppmanas, hämta projektet till en sökväg på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="a8b0e-136">When prompted, download the project to a path on your local computer.</span></span>

   ![Hämta anpassade källan Start-projekt][SI03]

1. <span data-ttu-id="a8b0e-138">När du har extraherat filerna på den lokala datorn, vara anpassade källan Start program klar för redigering.</span><span class="sxs-lookup"><span data-stu-id="a8b0e-138">After you have extracted the files on your local system, your custom Spring Boot application will be ready for editing.</span></span>

   ![Projektfiler för anpassade källan Start][SI04]

## <a name="configure-your-custom-spring-boot-to-use-your-redis-cache"></a><span data-ttu-id="a8b0e-140">Konfigurera din anpassade källan Start om du vill använda Redis-Cache</span><span class="sxs-lookup"><span data-stu-id="a8b0e-140">Configure your custom Spring Boot to use your Redis Cache</span></span>

1. <span data-ttu-id="a8b0e-141">Leta upp den *application.properties* filen i den *resurser* katalogen för din app, eller skapa filen om den inte redan finns.</span><span class="sxs-lookup"><span data-stu-id="a8b0e-141">Locate the *application.properties* file in the *resources* directory of your app, or create the file if it does not already exist.</span></span>

   ![Leta reda på filen application.properties][RE01]

1. <span data-ttu-id="a8b0e-143">Öppna den *application.properties* filen i en textredigerare och Lägg till följande rader i filen och Ersätt exempelvärdena med lämpliga egenskaper från ditt cacheminne:</span><span class="sxs-lookup"><span data-stu-id="a8b0e-143">Open the *application.properties* file in a text editor, and add the following lines to the file, and replace the sample values with the appropriate properties from your cache:</span></span>

   ```yaml
   # Specify the DNS URI of your Redis cache.
   spring.redis.host=myspringbootcache.redis.cache.windows.net

   # Specify the port for your Redis cache.
   spring.redis.port=6380

   # Specify the access key for your Redis cache.
   spring.redis.password=57686f6120447564652c2049495320526f636b73=
   ```

   ![Redigera filen application.properties][RE02]

1. <span data-ttu-id="a8b0e-145">Spara och Stäng den *application.properties* fil.</span><span class="sxs-lookup"><span data-stu-id="a8b0e-145">Save and close the *application.properties* file.</span></span>

1. <span data-ttu-id="a8b0e-146">Skapa en mapp med namnet *domänkontrollant* under källmappen för paketet, till exempel:</span><span class="sxs-lookup"><span data-stu-id="a8b0e-146">Create a folder named *controller* under the source folder for your package; for example:</span></span>

   `C:\SpringBoot\myazuredemo\src\main\java\com\contoso\myazuredemo\controller`

   <span data-ttu-id="a8b0e-147">ELLER</span><span class="sxs-lookup"><span data-stu-id="a8b0e-147">-or-</span></span>

   `/users/example/home/myazuredemo/src/main/java/com/contoso/myazuredemo/controller`

1. <span data-ttu-id="a8b0e-148">Skapa en ny fil med namnet *HelloController.java* i den *domänkontrollant* mapp.</span><span class="sxs-lookup"><span data-stu-id="a8b0e-148">Create a new file named *HelloController.java* in the *controller* folder.</span></span> <span data-ttu-id="a8b0e-149">Öppna filen i en textredigerare och Lägg till följande kod:</span><span class="sxs-lookup"><span data-stu-id="a8b0e-149">Open the file in a text editor and add the following code to it:</span></span>

   ```java
   package com.contoso.myazuredemo;

   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.bind.annotation.RestController;
   import org.springframework.beans.factory.annotation.Value;
   import redis.clients.jedis.Jedis;
   import redis.clients.jedis.JedisShardInfo;

   @RestController
   public class HelloController {
   
      // Retrieve the DNS name for your cache.
      @Value("${spring.redis.host}")
      private String redisHost;

      // Retrieve the port for your cache.
      @Value("${spring.redis.port}")
      private int redisPort;

      // Retrieve the access key for your cache.
      @Value("${spring.redis.password}")
      private String redisPassword;

      @RequestMapping("/")
      // Define the Hello World controller.
      public String hello() {
      
         // Create a JedisShardInfo object to connect to your Redis cache.
         JedisShardInfo jedisShardInfo = new JedisShardInfo(redisHost, redisPort, true);
         // Specify your access key.
         jedisShardInfo.setPassword(redisPassword);
         // Create a Jedis object to store/retrieve information from your cache.
         Jedis jedis = new Jedis(jedisShardInfo);

         // Add a Hello World string to your cache.
         jedis.set("greeting", "Hello World!");

         // Return the string from your cache.
         return jedis.get("greeting");
      }
   }
   ```
   
   <span data-ttu-id="a8b0e-150">Om du behöver ersätta `com.contoso.myazuredemo` med paketnamnet för projektet.</span><span class="sxs-lookup"><span data-stu-id="a8b0e-150">Where you will need to replace `com.contoso.myazuredemo` with the package name for your project.</span></span>

1. <span data-ttu-id="a8b0e-151">Spara och Stäng den *HelloController.java* fil.</span><span class="sxs-lookup"><span data-stu-id="a8b0e-151">Save and close the *HelloController.java* file.</span></span>

1. <span data-ttu-id="a8b0e-152">Skapa din källan startprogrammet med Maven och kör den. Exempel:</span><span class="sxs-lookup"><span data-stu-id="a8b0e-152">Build your Spring Boot application with Maven and run it; for example:</span></span>

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

1. <span data-ttu-id="a8b0e-153">Testa webbappen genom att bläddra till http://localhost: 8080 i en webbläsare, eller Använd syntax som i följande exempel om du har curl som är tillgängliga:</span><span class="sxs-lookup"><span data-stu-id="a8b0e-153">Test the web app by browsing to http://localhost:8080 using a web browser, or use the syntax like the following example if you have curl available:</span></span>

   ```shell
   curl http://localhost:8080
   ```

   <span data-ttu-id="a8b0e-154">Du bör se ”Hello World”!</span><span class="sxs-lookup"><span data-stu-id="a8b0e-154">You should see the "Hello World!"</span></span> <span data-ttu-id="a8b0e-155">meddelande från ditt exempel controller visas som hämtas dynamiskt från Redis-cache.</span><span class="sxs-lookup"><span data-stu-id="a8b0e-155">message from your sample controller displayed, which is being retrieved dynamically from your Redis cache.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a8b0e-156">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a8b0e-156">Next steps</span></span>

<span data-ttu-id="a8b0e-157">Mer information om hur du använder värdet startprogram i Azure finns i följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="a8b0e-157">For more information about using Spring Boot applications on Azure, see the following articles:</span></span>

* [<span data-ttu-id="a8b0e-158">Distribuera ett program för start av källan till Azure App Service</span><span class="sxs-lookup"><span data-stu-id="a8b0e-158">Deploy a Spring Boot Application to the Azure App Service</span></span>](../app-service/app-service-deploy-spring-boot-web-app-on-azure.md)

* [<span data-ttu-id="a8b0e-159">Kör ett program för start av värdet på ett Kubernetes kluster i Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="a8b0e-159">Running a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service</span></span>](../container-service/container-service-deploy-spring-boot-app-on-kubernetes.md)

<span data-ttu-id="a8b0e-160">Mer information om hur du använder Azure med Java finns på [Azure Java Developer Center] och i [Java Tools för Visual Studio Team Services].</span><span class="sxs-lookup"><span data-stu-id="a8b0e-160">For more information about using Azure with Java, see the [Azure Java Developer Center] and the [Java Tools for Visual Studio Team Services].</span></span>

<span data-ttu-id="a8b0e-161">Mer information om att komma igång med Redis-Cache med Java i Azure, finns [hur du använder Azure Redis-Cache med Java][Redis Cache with Java].</span><span class="sxs-lookup"><span data-stu-id="a8b0e-161">For more information about getting started using Redis Cache with Java on Azure, see [How to use Azure Redis Cache with Java][Redis Cache with Java].</span></span>

<!-- URL List -->

<span data-ttu-id="a8b0e-162">[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/</span><span class="sxs-lookup"><span data-stu-id="a8b0e-162">[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/</span></span>
<span data-ttu-id="a8b0e-163">[kostnadsfritt Azure-konto]: https://azure.microsoft.com/pricing/free-trial/</span><span class="sxs-lookup"><span data-stu-id="a8b0e-163">[free Azure account]: https://azure.microsoft.com/pricing/free-trial/</span></span>
<span data-ttu-id="a8b0e-164">[Java Tools för Visual Studio Team Services]: https://java.visualstudio.com/</span><span class="sxs-lookup"><span data-stu-id="a8b0e-164">[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/</span></span>
<span data-ttu-id="a8b0e-165">[MSDN-prenumerantförmåner]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/</span><span class="sxs-lookup"><span data-stu-id="a8b0e-165">[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/</span></span>
<span data-ttu-id="a8b0e-166">[Vårversionen Start]: http://projects.spring.io/spring-boot/</span><span class="sxs-lookup"><span data-stu-id="a8b0e-166">[Spring Boot]: http://projects.spring.io/spring-boot/</span></span>
<span data-ttu-id="a8b0e-167">[Vårversionen Initializr]: https://start.spring.io/</span><span class="sxs-lookup"><span data-stu-id="a8b0e-167">[Spring Initializr]: https://start.spring.io/</span></span>
<span data-ttu-id="a8b0e-168">[Vårversionen Framework]: https://spring.io/</span><span class="sxs-lookup"><span data-stu-id="a8b0e-168">[Spring Framework]: https://spring.io/</span></span>
[Redis Cache with Java]: cache-java-get-started.md

<!-- IMG List -->

[AZ01]: ./media/cache-java-spring-boot-initializer-with-redis-cache/AZ01.png
[AZ02]: ./media/cache-java-spring-boot-initializer-with-redis-cache/AZ02.png
[AZ03]: ./media/cache-java-spring-boot-initializer-with-redis-cache/AZ03.png
[AZ04]: ./media/cache-java-spring-boot-initializer-with-redis-cache/AZ04.png
[AZ05]: ./media/cache-java-spring-boot-initializer-with-redis-cache/AZ05.png

[SI01]: ./media/cache-java-spring-boot-initializer-with-redis-cache/SI01.png
[SI02]: ./media/cache-java-spring-boot-initializer-with-redis-cache/SI02.png
[SI03]: ./media/cache-java-spring-boot-initializer-with-redis-cache/SI03.png
[SI04]: ./media/cache-java-spring-boot-initializer-with-redis-cache/SI04.png

[RE01]: ./media/cache-java-spring-boot-initializer-with-redis-cache/RE01.png
[RE02]: ./media/cache-java-spring-boot-initializer-with-redis-cache/RE02.png
