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
# <a name="how-tooconfigure-a-spring-boot-initializer-app-toouse-redis-cache"></a>Hur tooconfigure en källan Start initieraren app toouse Redis-Cache

## <a name="overview"></a>Översikt

Hej  **[Vårversionen Framework]**  är en lösning för öppen källkod som hjälper Java-utvecklare som skapar program på företagsnivå. En mer populära hello-projekt som är inbyggd som plattform är [Vårversionen Start], vilket ger en förenklad metod för att skapa fristående Java-program. toohelp utvecklare Kom igång med vår Start, flera exempel källan Start paket finns på <https://github.com/spring-guides/>. Dessutom toochoosing hello listan över grundläggande källan Start projekten, hello  **[Vårversionen Initializr]**  gör att utvecklare kan komma igång med att skapa anpassade källan startprogram.

Den här artikeln vägleder dig genom att skapa ett Redis-cache med hjälp av hello Azure-portalen med hello **Vårversionen Initializr** toocreate ett anpassat program och sedan skapa en Java-webbprogram som lagrar och hämtar data med hjälp av din Redis-cache.

## <a name="prerequisites"></a>Krav

hello följande förutsättningar som krävs i ordning toofollow hello stegen i den här artikeln:

* En Azure-prenumeration; Om du inte redan har en Azure-prenumeration, kan du aktivera din [MSDN-prenumerantförmåner] eller registrera dig för en [kostnadsfritt Azure-konto].

* En [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/), 1,7 eller senare.

* [Apache Maven](http://maven.apache.org/), version 3.0 eller senare.

## <a name="create-a-redis-cache-on-azure"></a>Skapa en Redis-cache på Azure

1. Bläddra toohello Azure portal på <https://portal.azure.com/> och på hello-objekt för **+ ny**.

   ![Azure Portal][AZ01]

1. Klicka på **databasen**, och klicka sedan på **Redis-Cache**.

   ![Azure Portal][AZ02]

1. På hello **nytt Redis-Cache** anger hello **DNS-namnet** för ditt cacheminne, ange din **prenumeration**, **resursgruppen**,  **Plats**, och **prisnivå**. När du har angett dessa alternativ, klickar du på **skapa** toocreate ditt cacheminne.

   ![Azure Portal][AZ03]

1. När ditt cacheminne har slutförts, visas den på din Azure **instrumentpanelen**samt enligt hello **alla resurser**, och **Redis-cache** sidor. Du kan klicka på ditt cacheminne på någon av dessa platser tooopen hello egenskapssidan för ditt cacheminne.

   ![Azure Portal][AZ04]

1. När hello-sida som innehåller hello lista över egenskaper för ditt cacheminne visas, klickar du på **åtkomstnycklar** och kopiera dina åtkomstnycklar för ditt cacheminne.

   ![Azure Portal][AZ05]

## <a name="create-a-custom-application-using-hello-spring-initializr"></a>Skapa ett anpassat program som använder hello källan Initializr

1. Bläddra för<https://start.spring.io/>.

1. Ange att du vill toogenerate en **Maven** projektet med **Java**, ange hello **grupp** och **Aritifact** namn för ditt program och klicka sedan på hello länk för**växel toohello fullständig version** av hello källan Initializr.

   ![Basic-källan Initializr alternativ][SI01]

   > [!NOTE]
   >
   > Hej Vårversionen Initializr använder hello **grupp** och **Aritifact** toocreate hello paketnamn, till exempel: *com.contoso.myazuredemo*.
   >

1. Bläddra nedåt toohello **Web** avsnittet och kryssrutan för hello **Web**, bläddrar sedan nedåt toohello **NoSQL** avsnittet och kryssrutan för hello **Redis**, bläddrar sedan toohello längst ned på sidan för hello och klicka på knappen hello för**generera projekt**.

   ![Alternativ för fullständig Vårversionen Initializr][SI02]

1. När du uppmanas, hämta hello projektet tooa sökväg på den lokala datorn.

   ![Hämta anpassade källan Start-projekt][SI03]

1. När du har extraherat hello filer i det lokala systemet vara anpassade källan Start program klar för redigering.

   ![Projektfiler för anpassade källan Start][SI04]

## <a name="configure-your-custom-spring-boot-toouse-your-redis-cache"></a>Konfigurera din anpassade källan Start toouse Redis-Cache

1. Leta upp hello *application.properties* filen i hello *resurser* katalogen för din app, eller skapa hello-filen om den inte redan finns.

   ![Leta upp hello application.properties fil][RE01]

1. Öppna hello *application.properties* filen i en textredigerare och Lägg till följande rader toohello hello och Ersätt hello exempelvärden med hello lämpliga egenskaper från ditt cacheminne:

   ```yaml
   # Specify hello DNS URI of your Redis cache.
   spring.redis.host=myspringbootcache.redis.cache.windows.net

   # Specify hello port for your Redis cache.
   spring.redis.port=6380

   # Specify hello access key for your Redis cache.
   spring.redis.password=57686f6120447564652c2049495320526f636b73=
   ```

   ![Hej Filredigering application.properties][RE02]

1. Spara och Stäng hello *application.properties* fil.

1. Skapa en mapp med namnet *domänkontrollant* under hello källmappen för paketet, till exempel:

   `C:\SpringBoot\myazuredemo\src\main\java\com\contoso\myazuredemo\controller`

   ELLER

   `/users/example/home/myazuredemo/src/main/java/com/contoso/myazuredemo/controller`

1. Skapa en ny fil med namnet *HelloController.java* i hello *domänkontrollant* mapp. Öppna hello-filen i en textredigerare och Lägg till följande kod tooit hello:

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
   
   Om du behöver tooreplace `com.contoso.myazuredemo` med hello paketnamn för projektet.

1. Spara och Stäng hello *HelloController.java* fil.

1. Skapa din källan startprogrammet med Maven och kör den. Exempel:

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

1. Testa hello webbprogram genom att bläddra toohttp://localhost:8080 i en webbläsare eller använder hello syntax som hello följande exempel om du har curl som är tillgängliga:

   ```shell
   curl http://localhost:8080
   ```

   Du bör se hello ”Hello World”! meddelande från ditt exempel controller visas som hämtas dynamiskt från Redis-cache.

## <a name="next-steps"></a>Nästa steg

Mer information om hur du använder värdet startprogram i Azure finns i hello följande artiklar:

* [Distribuera en källan startprogrammet toohello Azure App Service](../app-service/app-service-deploy-spring-boot-web-app-on-azure.md)

* [Kör ett program för start av värdet på ett kluster med Kubernetes på hello Azure Container Service](../container-service/container-service-deploy-spring-boot-app-on-kubernetes.md)

Mer information om hur du använder Azure med Java finns hello [Azure Java Developer Center] och hello [Java-verktyg för Visual Studio Team Services].

Mer information om att komma igång med Redis-Cache med Java i Azure, finns [hur toouse Azure Redis-Cache med Java][Redis Cache with Java].

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
