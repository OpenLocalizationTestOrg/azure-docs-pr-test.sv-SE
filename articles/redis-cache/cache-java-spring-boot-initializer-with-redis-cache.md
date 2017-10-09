---
title: "aaaHow tooconfigure en källan Start initieraren app toouse Redis-Cache"
description: "Lär dig hur tooconfigure en källan startprogrammet skapas med hello källan Initializr toouse Azure Redis-Cache."
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
ms.openlocfilehash: ad532c88d2d67b97079eeb0e0e392add29ac365b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-a-spring-boot-initializer-app-toouse-redis-cache"></a><span data-ttu-id="405ba-104">Hur tooconfigure en källan Start initieraren app toouse Redis-Cache</span><span class="sxs-lookup"><span data-stu-id="405ba-104">How tooconfigure a Spring Boot Initializer app toouse Redis Cache</span></span>

## <a name="overview"></a><span data-ttu-id="405ba-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="405ba-105">Overview</span></span>

<span data-ttu-id="405ba-106">Hej  **[Vårversionen Framework]**  är en lösning för öppen källkod som hjälper Java-utvecklare som skapar program på företagsnivå.</span><span class="sxs-lookup"><span data-stu-id="405ba-106">hello **[Spring Framework]** is an open-source solution which helps Java developers create enterprise-level applications.</span></span> <span data-ttu-id="405ba-107">En mer populära hello-projekt som är inbyggd som plattform är [Vårversionen Start], vilket ger en förenklad metod för att skapa fristående Java-program.</span><span class="sxs-lookup"><span data-stu-id="405ba-107">One of hello more-popular projects which is built on top of that platform is [Spring Boot], which provides a simplified approach for creating stand-alone Java applications.</span></span> <span data-ttu-id="405ba-108">toohelp utvecklare Kom igång med vår Start, flera exempel källan Start paket finns på <https://github.com/spring-guides/>.</span><span class="sxs-lookup"><span data-stu-id="405ba-108">toohelp developers get started with Spring Boot, several sample Spring Boot packages are available at <https://github.com/spring-guides/>.</span></span> <span data-ttu-id="405ba-109">Dessutom toochoosing hello listan över grundläggande källan Start projekten, hello  **[Vårversionen Initializr]**  gör att utvecklare kan komma igång med att skapa anpassade källan startprogram.</span><span class="sxs-lookup"><span data-stu-id="405ba-109">In addition toochoosing from hello list of basic Spring Boot projects, hello **[Spring Initializr]** helps developers get started with creating custom Spring Boot applications.</span></span>

<span data-ttu-id="405ba-110">Den här artikeln vägleder dig genom att skapa ett Redis-cache med hjälp av hello Azure-portalen med hello **Vårversionen Initializr** toocreate ett anpassat program och sedan skapa en Java-webbprogram som lagrar och hämtar data med hjälp av din Redis-cache.</span><span class="sxs-lookup"><span data-stu-id="405ba-110">This article walks you through creating a Redis cache using hello Azure portal, then using hello **Spring Initializr** toocreate a custom application, and then creating a Java web application which stores and retrieves data using your Redis cache.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="405ba-111">Krav</span><span class="sxs-lookup"><span data-stu-id="405ba-111">Prerequisites</span></span>

<span data-ttu-id="405ba-112">hello följande förutsättningar som krävs i ordning toofollow hello stegen i den här artikeln:</span><span class="sxs-lookup"><span data-stu-id="405ba-112">hello following prerequisites are required in order toofollow hello steps in this article:</span></span>

* <span data-ttu-id="405ba-113">En Azure-prenumeration; Om du inte redan har en Azure-prenumeration, kan du aktivera din [MSDN-prenumerantförmåner] eller registrera dig för en [kostnadsfritt Azure-konto].</span><span class="sxs-lookup"><span data-stu-id="405ba-113">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>

* <span data-ttu-id="405ba-114">En [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/), 1,7 eller senare.</span><span class="sxs-lookup"><span data-stu-id="405ba-114">A [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/), version 1.7 or later.</span></span>

* <span data-ttu-id="405ba-115">[Apache Maven](http://maven.apache.org/), version 3.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="405ba-115">[Apache Maven](http://maven.apache.org/), version 3.0 or later.</span></span>

## <a name="create-a-redis-cache-on-azure"></a><span data-ttu-id="405ba-116">Skapa en Redis-cache på Azure</span><span class="sxs-lookup"><span data-stu-id="405ba-116">Create a Redis cache on Azure</span></span>

1. <span data-ttu-id="405ba-117">Bläddra toohello Azure portal på <https://portal.azure.com/> och på hello-objekt för **+ ny**.</span><span class="sxs-lookup"><span data-stu-id="405ba-117">Browse toohello Azure portal at <https://portal.azure.com/> and click hello item for **+New**.</span></span>

   ![Azure Portal][AZ01]

1. <span data-ttu-id="405ba-119">Klicka på **databasen**, och klicka sedan på **Redis-Cache**.</span><span class="sxs-lookup"><span data-stu-id="405ba-119">Click **Database**, and then click **Redis Cache**.</span></span>

   ![Azure Portal][AZ02]

1. <span data-ttu-id="405ba-121">På hello **nytt Redis-Cache** anger hello **DNS-namnet** för ditt cacheminne, ange din **prenumeration**, **resursgruppen**,  **Plats**, och **prisnivå**.</span><span class="sxs-lookup"><span data-stu-id="405ba-121">On hello **New Redis Cache** page, enter hello **DNS name** for your cache, then specify your **Subscription**, **Resource group**, **Location**, and **Pricing tier**.</span></span> <span data-ttu-id="405ba-122">När du har angett dessa alternativ, klickar du på **skapa** toocreate ditt cacheminne.</span><span class="sxs-lookup"><span data-stu-id="405ba-122">When you have specified these options, click **Create** toocreate your cache.</span></span>

   ![Azure Portal][AZ03]

1. <span data-ttu-id="405ba-124">När ditt cacheminne har slutförts, visas den på din Azure **instrumentpanelen**samt enligt hello **alla resurser**, och **Redis-cache** sidor.</span><span class="sxs-lookup"><span data-stu-id="405ba-124">Once your cache has been completed, you will see it listed on your Azure **Dashboard**, as well as under hello **All Resources**, and **Redis Caches** pages.</span></span> <span data-ttu-id="405ba-125">Du kan klicka på ditt cacheminne på någon av dessa platser tooopen hello egenskapssidan för ditt cacheminne.</span><span class="sxs-lookup"><span data-stu-id="405ba-125">You can click on your cache on any of those locations tooopen hello properties page for your cache.</span></span>

   ![Azure Portal][AZ04]

1. <span data-ttu-id="405ba-127">När hello-sida som innehåller hello lista över egenskaper för ditt cacheminne visas, klickar du på **åtkomstnycklar** och kopiera dina åtkomstnycklar för ditt cacheminne.</span><span class="sxs-lookup"><span data-stu-id="405ba-127">When hello page which contains hello list of properties for your cache is displayed, click **Access keys** and copy your access keys for your cache.</span></span>

   ![Azure Portal][AZ05]

## <a name="create-a-custom-application-using-hello-spring-initializr"></a><span data-ttu-id="405ba-129">Skapa ett anpassat program som använder hello källan Initializr</span><span class="sxs-lookup"><span data-stu-id="405ba-129">Create a custom application using hello Spring Initializr</span></span>

1. <span data-ttu-id="405ba-130">Bläddra för<https://start.spring.io/>.</span><span class="sxs-lookup"><span data-stu-id="405ba-130">Browse too<https://start.spring.io/>.</span></span>

1. <span data-ttu-id="405ba-131">Ange att du vill toogenerate en **Maven** projektet med **Java**, ange hello **grupp** och **Aritifact** namn för ditt program och klicka sedan på hello länk för**växel toohello fullständig version** av hello källan Initializr.</span><span class="sxs-lookup"><span data-stu-id="405ba-131">Specify that you want toogenerate a **Maven** project with **Java**, enter hello **Group** and **Aritifact** names for your application, and then click hello link too**Switch toohello full version** of hello Spring Initializr.</span></span>

   ![Basic-källan Initializr alternativ][SI01]

   > [!NOTE]
   >
   > <span data-ttu-id="405ba-133">Hej Vårversionen Initializr använder hello **grupp** och **Aritifact** toocreate hello paketnamn, till exempel: *com.contoso.myazuredemo*.</span><span class="sxs-lookup"><span data-stu-id="405ba-133">hello Spring Initializr will use hello **Group** and **Aritifact** names toocreate hello package name; for example: *com.contoso.myazuredemo*.</span></span>
   >

1. <span data-ttu-id="405ba-134">Bläddra nedåt toohello **Web** avsnittet och kryssrutan för hello **Web**, bläddrar sedan nedåt toohello **NoSQL** avsnittet och kryssrutan för hello **Redis**, bläddrar sedan toohello längst ned på sidan för hello och klicka på knappen hello för**generera projekt**.</span><span class="sxs-lookup"><span data-stu-id="405ba-134">Scroll down toohello **Web** section and check hello box for **Web**, then scroll down toohello **NoSQL** section and check hello box for **Redis**, then scroll toohello bottom of hello page and click hello button too**Generate Project**.</span></span>

   ![Alternativ för fullständig Vårversionen Initializr][SI02]

1. <span data-ttu-id="405ba-136">När du uppmanas, hämta hello projektet tooa sökväg på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="405ba-136">When prompted, download hello project tooa path on your local computer.</span></span>

   ![Hämta anpassade källan Start-projekt][SI03]

1. <span data-ttu-id="405ba-138">När du har extraherat hello filer i det lokala systemet vara anpassade källan Start program klar för redigering.</span><span class="sxs-lookup"><span data-stu-id="405ba-138">After you have extracted hello files on your local system, your custom Spring Boot application will be ready for editing.</span></span>

   ![Projektfiler för anpassade källan Start][SI04]

## <a name="configure-your-custom-spring-boot-toouse-your-redis-cache"></a><span data-ttu-id="405ba-140">Konfigurera din anpassade källan Start toouse Redis-Cache</span><span class="sxs-lookup"><span data-stu-id="405ba-140">Configure your custom Spring Boot toouse your Redis Cache</span></span>

1. <span data-ttu-id="405ba-141">Leta upp hello *application.properties* filen i hello *resurser* katalogen för din app, eller skapa hello-filen om den inte redan finns.</span><span class="sxs-lookup"><span data-stu-id="405ba-141">Locate hello *application.properties* file in hello *resources* directory of your app, or create hello file if it does not already exist.</span></span>

   ![Leta upp hello application.properties fil][RE01]

1. <span data-ttu-id="405ba-143">Öppna hello *application.properties* filen i en textredigerare och Lägg till följande rader toohello hello och Ersätt hello exempelvärden med hello lämpliga egenskaper från ditt cacheminne:</span><span class="sxs-lookup"><span data-stu-id="405ba-143">Open hello *application.properties* file in a text editor, and add hello following lines toohello file, and replace hello sample values with hello appropriate properties from your cache:</span></span>

   ```yaml
   # Specify hello DNS URI of your Redis cache.
   spring.redis.host=myspringbootcache.redis.cache.windows.net

   # Specify hello port for your Redis cache.
   spring.redis.port=6380

   # Specify hello access key for your Redis cache.
   spring.redis.password=57686f6120447564652c2049495320526f636b73=
   ```

   ![Hej Filredigering application.properties][RE02]

1. <span data-ttu-id="405ba-145">Spara och Stäng hello *application.properties* fil.</span><span class="sxs-lookup"><span data-stu-id="405ba-145">Save and close hello *application.properties* file.</span></span>

1. <span data-ttu-id="405ba-146">Skapa en mapp med namnet *domänkontrollant* under hello källmappen för paketet, till exempel:</span><span class="sxs-lookup"><span data-stu-id="405ba-146">Create a folder named *controller* under hello source folder for your package; for example:</span></span>

   `C:\SpringBoot\myazuredemo\src\main\java\com\contoso\myazuredemo\controller`

   <span data-ttu-id="405ba-147">ELLER</span><span class="sxs-lookup"><span data-stu-id="405ba-147">-or-</span></span>

   `/users/example/home/myazuredemo/src/main/java/com/contoso/myazuredemo/controller`

1. <span data-ttu-id="405ba-148">Skapa en ny fil med namnet *HelloController.java* i hello *domänkontrollant* mapp.</span><span class="sxs-lookup"><span data-stu-id="405ba-148">Create a new file named *HelloController.java* in hello *controller* folder.</span></span> <span data-ttu-id="405ba-149">Öppna hello-filen i en textredigerare och Lägg till följande kod tooit hello:</span><span class="sxs-lookup"><span data-stu-id="405ba-149">Open hello file in a text editor and add hello following code tooit:</span></span>

   ```java
   package com.contoso.myazuredemo;

   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.bind.annotation.RestController;
   import org.springframework.beans.factory.annotation.Value;
   import redis.clients.jedis.Jedis;
   import redis.clients.jedis.JedisShardInfo;

   @RestController
   public class HelloController {
   
      // Retrieve hello DNS name for your cache.
      @Value("${spring.redis.host}")
      private String redisHost;

      // Retrieve hello port for your cache.
      @Value("${spring.redis.port}")
      private int redisPort;

      // Retrieve hello access key for your cache.
      @Value("${spring.redis.password}")
      private String redisPassword;

      @RequestMapping("/")
      // Define hello Hello World controller.
      public String hello() {
      
         // Create a JedisShardInfo object tooconnect tooyour Redis cache.
         JedisShardInfo jedisShardInfo = new JedisShardInfo(redisHost, redisPort, true);
         // Specify your access key.
         jedisShardInfo.setPassword(redisPassword);
         // Create a Jedis object toostore/retrieve information from your cache.
         Jedis jedis = new Jedis(jedisShardInfo);

         // Add a Hello World string tooyour cache.
         jedis.set("greeting", "Hello World!");

         // Return hello string from your cache.
         return jedis.get("greeting");
      }
   }
   ```
   
   <span data-ttu-id="405ba-150">Om du behöver tooreplace `com.contoso.myazuredemo` med hello paketnamn för projektet.</span><span class="sxs-lookup"><span data-stu-id="405ba-150">Where you will need tooreplace `com.contoso.myazuredemo` with hello package name for your project.</span></span>

1. <span data-ttu-id="405ba-151">Spara och Stäng hello *HelloController.java* fil.</span><span class="sxs-lookup"><span data-stu-id="405ba-151">Save and close hello *HelloController.java* file.</span></span>

1. <span data-ttu-id="405ba-152">Skapa din källan startprogrammet med Maven och kör den. Exempel:</span><span class="sxs-lookup"><span data-stu-id="405ba-152">Build your Spring Boot application with Maven and run it; for example:</span></span>

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

1. <span data-ttu-id="405ba-153">Testa hello webbprogram genom att bläddra toohttp://localhost:8080 i en webbläsare eller använder hello syntax som hello följande exempel om du har curl som är tillgängliga:</span><span class="sxs-lookup"><span data-stu-id="405ba-153">Test hello web app by browsing toohttp://localhost:8080 using a web browser, or use hello syntax like hello following example if you have curl available:</span></span>

   ```shell
   curl http://localhost:8080
   ```

   <span data-ttu-id="405ba-154">Du bör se hello ”Hello World”!</span><span class="sxs-lookup"><span data-stu-id="405ba-154">You should see hello "Hello World!"</span></span> <span data-ttu-id="405ba-155">meddelande från ditt exempel controller visas som hämtas dynamiskt från Redis-cache.</span><span class="sxs-lookup"><span data-stu-id="405ba-155">message from your sample controller displayed, which is being retrieved dynamically from your Redis cache.</span></span>

## <a name="next-steps"></a><span data-ttu-id="405ba-156">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="405ba-156">Next steps</span></span>

<span data-ttu-id="405ba-157">Mer information om hur du använder värdet startprogram i Azure finns i hello följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="405ba-157">For more information about using Spring Boot applications on Azure, see hello following articles:</span></span>

* [<span data-ttu-id="405ba-158">Distribuera en källan startprogrammet toohello Azure App Service</span><span class="sxs-lookup"><span data-stu-id="405ba-158">Deploy a Spring Boot Application toohello Azure App Service</span></span>](../app-service/app-service-deploy-spring-boot-web-app-on-azure.md)

* [<span data-ttu-id="405ba-159">Kör ett program för start av värdet på ett kluster med Kubernetes på hello Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="405ba-159">Running a Spring Boot Application on a Kubernetes Cluster in hello Azure Container Service</span></span>](../container-service/container-service-deploy-spring-boot-app-on-kubernetes.md)

<span data-ttu-id="405ba-160">Mer information om hur du använder Azure med Java finns hello [Azure Java Developer Center] och hello [Java-verktyg för Visual Studio Team Services].</span><span class="sxs-lookup"><span data-stu-id="405ba-160">For more information about using Azure with Java, see hello [Azure Java Developer Center] and hello [Java Tools for Visual Studio Team Services].</span></span>

<span data-ttu-id="405ba-161">Mer information om att komma igång med Redis-Cache med Java i Azure, finns [hur toouse Azure Redis-Cache med Java][Redis Cache with Java].</span><span class="sxs-lookup"><span data-stu-id="405ba-161">For more information about getting started using Redis Cache with Java on Azure, see [How toouse Azure Redis Cache with Java][Redis Cache with Java].</span></span>

<!-- URL List -->

[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[kostnadsfritt Azure-konto]: https://azure.microsoft.com/pricing/free-trial/
[Java-verktyg för Visual Studio Team Services]: https://java.visualstudio.com/
[MSDN-prenumerantförmåner]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Vårversionen Start]: http://projects.spring.io/spring-boot/
[Vårversionen Initializr]: https://start.spring.io/
[Vårversionen Framework]: https://spring.io/
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
