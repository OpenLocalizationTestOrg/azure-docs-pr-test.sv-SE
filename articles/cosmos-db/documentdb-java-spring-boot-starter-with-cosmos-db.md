---
title: "aaaHow toouse hello källan Start Starter med en Azure Cosmos DB DocumentDB-API"
description: "Lär dig hur tooconfigure ett program skapas med hello källan Start initieraren med hello Azure Cosmos DB DocumentDB API."
services: cosmos-db
documentationcenter: java
author: rmcmurray
manager: cfowler
editor: 
keywords: "Källan källan Start Starter Cosmos DB"
ms.assetid: 
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 08/08/2017
ms.author: robmcm;yungez;kevinzha
ms.openlocfilehash: a2c6de678f850676cb2887e224e5c12950db0e53
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-spring-boot-starter-with-azure-cosmos-db-documentdb-api"></a>Hur toouse hello källan Start Starter med Azure Cosmos DB DocumentDB API

## <a name="overview"></a>Översikt

Hej  **[Vårversionen Framework]**  är en öppen källkod som hjälper till att Java-utvecklare som skapar program på företagsnivå. En mer populära hello-projekt som är inbyggd som plattform är [Vårversionen Start], vilket ger en förenklad metod för att skapa fristående Java-program. toohelp utvecklare Kom igång med vår Start, flera exempel källan Start paket finns på <https://github.com/spring-guides/>. Dessutom toochoosing hello listan över grundläggande källan Start projekten, hello  **[Vårversionen Initializr]**  gör att utvecklare kan komma igång med att skapa anpassade källan startprogram.

Azure Cosmos-DB är ett globalt distribuerad databastjänst som gör att utvecklare toowork med data med en mängd olika standard API: er, till exempel DocumentDB, MongoDB, diagram och tabell-API: er. Microsofts källan Start Starter kan utvecklare toouse källan startprogram som lätt kan integrerar med Azure Cosmos DB med DocumentDB APIs.

Den här artikeln visar hur du skapar en Azure-Cosmos DB använder hello Azure-portalen, och sedan använda hello **Vårversionen Initializr** toocreate ett anpassat java-program och Lägg sedan till hello källan Start Starter funktioner tooyour anpassad toostore programdata i och hämta data från Azure Cosmos-databasen med hjälp av hello DocumentDB-API.

## <a name="prerequisites"></a>Krav

hello följande förutsättningar som krävs i ordning toofollow hello stegen i den här artikeln:

* En Azure-prenumeration; Om du inte redan har en Azure-prenumeration, kan du aktivera din [MSDN-prenumerantförmåner] eller registrera dig för en [kostnadsfritt Azure-konto].

* En [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/), 1,7 eller senare.

* [Apache Maven](http://maven.apache.org/), version 3.0 eller senare.

## <a name="create-an-azure-cosmos-db-by-using-hello-azure-portal"></a>Skapa en Azure-Cosmos-databas med hjälp av hello Azure-portalen

1. Bläddra toohello Azure portal på <https://portal.azure.com/> och på **+ ny**.

   ![Azure Portal][AZ01]

1. Klicka på **databaser**, och klicka sedan på **Azure Cosmos DB**.

   ![Azure Portal][AZ02]

1. På hello **Azure Cosmos DB** anger hello följande information:

   * Ange ett unikt **ID**, som du använder som hello URI för din databas. Till exempel: *wingtiptoysdata.documents.azure.com*.
   * Välj **SQL (dokumentet DB)** för hello API.
   * Välj hello **prenumeration** du vill använda toouse för din databas.
   * Ange om toocreate en ny **resursgruppen** för databasen, eller välj en befintlig resursgrupp.
   * Ange hello **plats** för din databas.
   
   När du har angett dessa alternativ, klickar du på **skapa** toocreate din databas.

   ![Azure Portal][AZ03]

1. När databasen har skapats kan den visas på din Azure **instrumentpanelen**samt enligt hello **alla resurser** och **Azure Cosmos DB** sidor. Du kan klicka på din databas på någon av dessa platser tooopen hello egenskapssidan för ditt cacheminne.

   ![Azure Portal][AZ04]

1. När hello egenskapssidan för din databas visas, klickar du på **åtkomstnycklar** och kopiera URI och åtkomst-nycklar för din databas, dessa värden används i tillämpningsprogrammet källan start.

   ![Azure Portal][AZ05]

## <a name="create-a-simple-spring-boot-application-with-hello-spring-initializr"></a>Skapa ett enkelt källan Start-program med hello källan Initializr

1. Bläddra för<https://start.spring.io/>.

1. Ange att du vill toogenerate en **Maven** projektet med **Java**, ange hello **grupp** och **artefakt** namn för ditt program och Klicka på knappen hello för**generera projekt**.

   ![Basic-källan Initializr alternativ][SI01]

   > [!NOTE]
   >
   > Hej Vårversionen Initializr använder hello **grupp** och **artefakt** toocreate hello paketnamn, till exempel: *com.example.wintiptoys*.
   >

1. När du uppmanas, hämta hello projektet tooa sökväg på den lokala datorn.

   ![Hämta anpassade källan Start-projekt][SI02]

1. När du har extraherat hello filer i det lokala systemet vara enkla källan Start programmet klar för redigering.

   ![Projektfiler för anpassade källan Start][SI03]

## <a name="configure-your-spring-boot-app-toouse-hello-azure-spring-boot-starter"></a>Konfigurera din källan Start app toouse hello Azure källan Start Starter

1. Leta upp hello *pom.xml* filen i hello katalogen för din app, till exempel:

   `C:\SpringBoot\wingtiptoys\pom.xml`

   ELLER

   `/users/example/home/wingtiptoys/pom.xml`

   ![Leta upp hello pom.xml fil][PM01]

1. Öppna hello *pom.xml* filen i en textredigerare och Lägg till följande rader toolist av hello `<dependencies>`:

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-documentdb-spring-boot-starter</artifactId>
      <version>0.1.4</version>
   </dependency>
   ```

   ![Hej Filredigering pom.xml][PM02]

1. Spara och Stäng hello *pom.xml* fil.

## <a name="configure-your-spring-boot-app-toouse-your-azure-cosmos-db"></a>Konfigurera din app källan Start-toouse Azure Cosmos-DB

1. Leta upp hello *application.properties* filen i hello *resurser* katalogen för din app, till exempel:

   `C:\SpringBoot\wingtiptoys\src\main\resources\application.properties`

   ELLER

   `/users/example/home/wingtiptoys/src/main/resources/application.properties`

   ![Leta upp hello application.properties fil][RE01]

1. Öppna hello *application.properties* filen i en textredigerare och Lägg till följande rader toohello hello och Ersätt hello exempelvärden med hello lämpliga egenskaper för databasen:

   ```yaml
   # Specify hello DNS URI of your Azure Cosmos DB.
   azure.documentdb.uri=https://wingtiptoys.documents.azure.com:443/

   # Specify hello access key for your database.
   azure.documentdb.key=57686f6120447564652c20426f6220526f636b73==

   # Specify hello name of your database.
   azure.documentdb.database=wingtiptoysdata
   ```

   ![Hej Filredigering application.properties][RE02]

1. Spara och Stäng hello *application.properties* fil.

## <a name="add-sample-code-tooimplement-basic-database-functionality"></a>Lägga till exempel kod tooimplement grundläggande funktioner

I det här avsnittet skapar du två Java-klasser för lagring av användardata och sedan du ändra din programmets klass toocreate en instans av klassen hello och spara den tooyour databas.

### <a name="define-a-basic-class-for-storing-user-data"></a>Definiera en grundläggande klass för lagring av användardata

1. Skapa en ny fil med namnet *User.java* i hello samma katalog som programmets Java-huvudfil.

1. Öppna hello *User.java* filen i en textredigerare och Lägg till följande hello rader toohello filen toodefine en allmän användarklass som lagrar och hämtar värden i databasen:

   ```java
   package com.example.wingtiptoys;

   public class User {
      private String id;
      private String firstName;
      private String lastName;
 
      public User(String id, String firstName, String lastName) {
         this.id = id;
         this.firstName = firstName;
         this.lastName = lastName;
      }
   
      public String getId() {
         return this.id;
      }

      public void setId(String id) {
         this.id = id;
      }

      public String getFirstName() {
         return firstName;
      }

      public void setFirstName(String firstName) {
         this.firstName = firstName;
      }

      public String getLastName() {
         return lastName;
      }

      public void setLastName(String lastName) {
         this.lastName = lastName;
      }

      @Override
      public String toString() {
         return String.format("User: %s %s", firstName, lastName);
      }
   }
   ```

1. Spara och Stäng hello *User.java* fil.

### <a name="define-a-data-repository-interface"></a>Definiera ett gränssnitt för databasen

1. Skapa en ny fil med namnet *UserRepository.java* i hello samma katalog som programmets Java-huvudfil.

1. Öppna hello *UserRepository.java* filen i en textredigerare och Lägg till hello följande rader toohello filen toodefine ett användargränssnitt för databasen som utökar hello standardgränssnittet DocumentDB databasen:

   ```java
   package com.example.wingtiptoys;

   import com.microsoft.azure.spring.data.documentdb.repository.DocumentDbRepository;
   import org.springframework.stereotype.Repository;

   @Repository
   public interface UserRepository extends DocumentDbRepository<User, String> {}   
   ```

1. Spara och Stäng hello *UserRepository.java* fil.

### <a name="modify-hello-main-application-class"></a>Ändra hello programmets klass

1. Leta upp hello programmets Java huvudfil i hello paketkatalogen i din app; Exempel:

   `C:\SpringBoot\wingtiptoys\src\main\java\com\example\wingtiptoys\WingtiptoysApplication.java`

   ELLER

   `/users/example/home/wingtiptoys/src/main/java/com/example/wingtiptoys/WingtiptoysApplication.java`

   ![Leta upp hello programfilen Java][JV01]

1. Öppna hello programmets Java-filen i en textredigerare och Lägg till följande rader toohello hello:

   ```java
   package com.example.wingtiptoys;

   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.boot.CommandLineRunner;

   @SpringBootApplication
   public class WingtiptoysApplication implements CommandLineRunner {

      @Autowired
      private UserRepository repository;
    
      public static void main(String[] args) {
         SpringApplication.run(WingtiptoysApplication.class, args);
      }

      public void run(String... var1) throws Exception {
         final User testUser = new User("testId", "testFirstName", "testLastName");

         repository.deleteAll();
         repository.save(testUser);

         final User result = repository.findOne(testUser.getId());

         System.out.printf("\n\n%s\n\n",result.toString());
      }
   }
   ```

1. Spara och Stäng hello programmets Java huvudfil.

## <a name="build-and-test-your-app"></a>Skapa och testa din app

1. Öppna en kommandotolk och ändra katalogen toohello mappen där din *pom.xml* finns, till exempel:

   `cd C:\SpringBoot\wingtiptoys`

   ELLER

   `cd /users/example/home/wingtiptoys`

1. Skapa din källan startprogrammet med Maven och kör den. Exempel:

   ```shell
   mvn package
   java -jar target/wingtiptoys-0.0.1-SNAPSHOT.jar
   ```

1. Programmet visas flera runtime-meddelanden och du bör se hello-meddelande `User: testFirstName testLastName` visas tooindicate som värden har lagras och hämtas från databasen.

   ![Lyckad utdata från hello-program][JV02]

1. Valfritt: Du kan använda hello Azure portal tooview hello innehållet i din Azure-Cosmos-DB från hello egenskapssidan för din databas genom att klicka på **Dokumentutforskaren**, och sedan välja och objekt från hello visas listan tooview hello innehållet.

   ![Med hjälp av hello Dokumentutforskaren tooview dina data][JV03]

## <a name="next-steps"></a>Nästa steg

Mer information om hur du använder Azure Cosmos DB och Java Se hello följande artiklar:

* [Dokumentation för Azure Cosmos DB].

* [Azure DB Cosmos: Skapa en DocumentDB-API-app med Java och hello Azure-portalen][Build a DocumentDB API app with Java]

Mer information om hur du använder värdet startprogram i Azure finns i hello följande artiklar:

* [Källan Start DocumenDB Starter för Azure](https://github.com/Microsoft/azure-spring-boot-starters/tree/master/azure-documentdb-spring-boot-starter-sample)

* [Distribuera en källan startprogrammet toohello Azure App Service](../app-service/app-service-deploy-spring-boot-web-app-on-azure.md)

* [Kör ett program för start av värdet på ett kluster med Kubernetes på hello Azure Container Service](../container-service/container-service-deploy-spring-boot-app-on-kubernetes.md)

Mer information om hur du använder Azure med Java finns hello [Azure Java Developer Center] och hello [Java-verktyg för Visual Studio Team Services].

<!-- URL List -->

[Dokumentation för Azure Cosmos DB]: /azure/cosmos-db/
[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[Build a DocumentDB API app with Java]: https://docs.microsoft.com/azure/cosmos-db/create-documentdb-java
[kostnadsfritt Azure-konto]: https://azure.microsoft.com/pricing/free-trial/
[Java-verktyg för Visual Studio Team Services]: https://java.visualstudio.com/
[MSDN-prenumerantförmåner]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Vårversionen Start]: http://projects.spring.io/spring-boot/
[Vårversionen Initializr]: https://start.spring.io/
[Vårversionen Framework]: https://spring.io/

<!-- IMG List -->

[AZ01]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/AZ01.png
[AZ02]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/AZ02.png
[AZ03]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/AZ03.png
[AZ04]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/AZ04.png
[AZ05]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/AZ05.png

[SI01]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/SI01.png
[SI02]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/SI02.png
[SI03]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/SI03.png

[RE01]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/RE01.png
[RE02]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/RE02.png

[PM01]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/PM01.png
[PM02]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/PM02.png

[JV01]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/JV01.png
[JV02]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/JV02.png
[JV03]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/JV03.png
