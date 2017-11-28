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
# <a name="how-toouse-hello-spring-boot-starter-with-azure-cosmos-db-documentdb-api"></a><span data-ttu-id="5ca58-104">Hur toouse hello källan Start Starter med Azure Cosmos DB DocumentDB API</span><span class="sxs-lookup"><span data-stu-id="5ca58-104">How toouse hello Spring Boot Starter with Azure Cosmos DB DocumentDB API</span></span>

## <a name="overview"></a><span data-ttu-id="5ca58-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="5ca58-105">Overview</span></span>

<span data-ttu-id="5ca58-106">Hej  **[Vårversionen Framework]**  är en öppen källkod som hjälper till att Java-utvecklare som skapar program på företagsnivå.</span><span class="sxs-lookup"><span data-stu-id="5ca58-106">hello **[Spring Framework]** is an open-source solution that helps Java developers create enterprise-level applications.</span></span> <span data-ttu-id="5ca58-107">En mer populära hello-projekt som är inbyggd som plattform är [Vårversionen Start], vilket ger en förenklad metod för att skapa fristående Java-program.</span><span class="sxs-lookup"><span data-stu-id="5ca58-107">One of hello more-popular projects that is built on top of that platform is [Spring Boot], which provides a simplified approach for creating stand-alone Java applications.</span></span> <span data-ttu-id="5ca58-108">toohelp utvecklare Kom igång med vår Start, flera exempel källan Start paket finns på <https://github.com/spring-guides/>.</span><span class="sxs-lookup"><span data-stu-id="5ca58-108">toohelp developers get started with Spring Boot, several sample Spring Boot packages are available at <https://github.com/spring-guides/>.</span></span> <span data-ttu-id="5ca58-109">Dessutom toochoosing hello listan över grundläggande källan Start projekten, hello  **[Vårversionen Initializr]**  gör att utvecklare kan komma igång med att skapa anpassade källan startprogram.</span><span class="sxs-lookup"><span data-stu-id="5ca58-109">In addition toochoosing from hello list of basic Spring Boot projects, hello **[Spring Initializr]** helps developers get started with creating custom Spring Boot applications.</span></span>

<span data-ttu-id="5ca58-110">Azure Cosmos-DB är ett globalt distribuerad databastjänst som gör att utvecklare toowork med data med en mängd olika standard API: er, till exempel DocumentDB, MongoDB, diagram och tabell-API: er.</span><span class="sxs-lookup"><span data-stu-id="5ca58-110">Azure Cosmos DB is a globally-distributed database service that allows developers toowork with data using a variety of standard APIs, such as DocumentDB, MongoDB, Graph, and Table APIs.</span></span> <span data-ttu-id="5ca58-111">Microsofts källan Start Starter kan utvecklare toouse källan startprogram som lätt kan integrerar med Azure Cosmos DB med DocumentDB APIs.</span><span class="sxs-lookup"><span data-stu-id="5ca58-111">Microsoft's Spring Boot Starter enables developers toouse Spring Boot applications that easily integrate with Azure Cosmos DB by using DocumentDB APIs.</span></span>

<span data-ttu-id="5ca58-112">Den här artikeln visar hur du skapar en Azure-Cosmos DB använder hello Azure-portalen, och sedan använda hello **Vårversionen Initializr** toocreate ett anpassat java-program och Lägg sedan till hello källan Start Starter funktioner tooyour anpassad toostore programdata i och hämta data från Azure Cosmos-databasen med hjälp av hello DocumentDB-API.</span><span class="sxs-lookup"><span data-stu-id="5ca58-112">This article demonstrates creating an Azure Cosmos DB using hello Azure portal, then using hello **Spring Initializr** toocreate a custom java application, and then add hello Spring Boot Starter functionality tooyour custom application toostore data in and retrieve data from your Azure Cosmos DB by using hello DocumentDB API.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5ca58-113">Krav</span><span class="sxs-lookup"><span data-stu-id="5ca58-113">Prerequisites</span></span>

<span data-ttu-id="5ca58-114">hello följande förutsättningar som krävs i ordning toofollow hello stegen i den här artikeln:</span><span class="sxs-lookup"><span data-stu-id="5ca58-114">hello following prerequisites are required in order toofollow hello steps in this article:</span></span>

* <span data-ttu-id="5ca58-115">En Azure-prenumeration; Om du inte redan har en Azure-prenumeration, kan du aktivera din [MSDN-prenumerantförmåner] eller registrera dig för en [kostnadsfritt Azure-konto].</span><span class="sxs-lookup"><span data-stu-id="5ca58-115">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>

* <span data-ttu-id="5ca58-116">En [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/), 1,7 eller senare.</span><span class="sxs-lookup"><span data-stu-id="5ca58-116">A [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/), version 1.7 or later.</span></span>

* <span data-ttu-id="5ca58-117">[Apache Maven](http://maven.apache.org/), version 3.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="5ca58-117">[Apache Maven](http://maven.apache.org/), version 3.0 or later.</span></span>

## <a name="create-an-azure-cosmos-db-by-using-hello-azure-portal"></a><span data-ttu-id="5ca58-118">Skapa en Azure-Cosmos-databas med hjälp av hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="5ca58-118">Create an Azure Cosmos DB by using hello Azure portal</span></span>

1. <span data-ttu-id="5ca58-119">Bläddra toohello Azure portal på <https://portal.azure.com/> och på **+ ny**.</span><span class="sxs-lookup"><span data-stu-id="5ca58-119">Browse toohello Azure portal at <https://portal.azure.com/> and click **+New**.</span></span>

   ![Azure Portal][AZ01]

1. <span data-ttu-id="5ca58-121">Klicka på **databaser**, och klicka sedan på **Azure Cosmos DB**.</span><span class="sxs-lookup"><span data-stu-id="5ca58-121">Click **Databases**, and then click **Azure Cosmos DB**.</span></span>

   ![Azure Portal][AZ02]

1. <span data-ttu-id="5ca58-123">På hello **Azure Cosmos DB** anger hello följande information:</span><span class="sxs-lookup"><span data-stu-id="5ca58-123">On hello **Azure Cosmos DB** page, enter hello following information:</span></span>

   * <span data-ttu-id="5ca58-124">Ange ett unikt **ID**, som du använder som hello URI för din databas.</span><span class="sxs-lookup"><span data-stu-id="5ca58-124">Enter a unique **ID**, which you will use as hello URI for your database.</span></span> <span data-ttu-id="5ca58-125">Till exempel: *wingtiptoysdata.documents.azure.com*.</span><span class="sxs-lookup"><span data-stu-id="5ca58-125">For example: *wingtiptoysdata.documents.azure.com*.</span></span>
   * <span data-ttu-id="5ca58-126">Välj **SQL (dokumentet DB)** för hello API.</span><span class="sxs-lookup"><span data-stu-id="5ca58-126">Choose **SQL (Document DB)** for hello API.</span></span>
   * <span data-ttu-id="5ca58-127">Välj hello **prenumeration** du vill använda toouse för din databas.</span><span class="sxs-lookup"><span data-stu-id="5ca58-127">Choose hello **Subscription** you want toouse for your database.</span></span>
   * <span data-ttu-id="5ca58-128">Ange om toocreate en ny **resursgruppen** för databasen, eller välj en befintlig resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="5ca58-128">Specify whether toocreate a new **Resource group** for your database, or choose an existing resource group.</span></span>
   * <span data-ttu-id="5ca58-129">Ange hello **plats** för din databas.</span><span class="sxs-lookup"><span data-stu-id="5ca58-129">Specify hello **Location** for your database.</span></span>
   
   <span data-ttu-id="5ca58-130">När du har angett dessa alternativ, klickar du på **skapa** toocreate din databas.</span><span class="sxs-lookup"><span data-stu-id="5ca58-130">When you have specified these options, click **Create** toocreate your database.</span></span>

   ![Azure Portal][AZ03]

1. <span data-ttu-id="5ca58-132">När databasen har skapats kan den visas på din Azure **instrumentpanelen**samt enligt hello **alla resurser** och **Azure Cosmos DB** sidor.</span><span class="sxs-lookup"><span data-stu-id="5ca58-132">When your database has been created, it is listed on your Azure **Dashboard**, as well as under hello **All Resources** and **Azure Cosmos DB** pages.</span></span> <span data-ttu-id="5ca58-133">Du kan klicka på din databas på någon av dessa platser tooopen hello egenskapssidan för ditt cacheminne.</span><span class="sxs-lookup"><span data-stu-id="5ca58-133">You can click on your database on any of those locations tooopen hello properties page for your cache.</span></span>

   ![Azure Portal][AZ04]

1. <span data-ttu-id="5ca58-135">När hello egenskapssidan för din databas visas, klickar du på **åtkomstnycklar** och kopiera URI och åtkomst-nycklar för din databas, dessa värden används i tillämpningsprogrammet källan start.</span><span class="sxs-lookup"><span data-stu-id="5ca58-135">When hello properties page for your database is displayed, click **Access keys** and copy your URI and access keys for your database; you will use these values in your Spring Boot application.</span></span>

   ![Azure Portal][AZ05]

## <a name="create-a-simple-spring-boot-application-with-hello-spring-initializr"></a><span data-ttu-id="5ca58-137">Skapa ett enkelt källan Start-program med hello källan Initializr</span><span class="sxs-lookup"><span data-stu-id="5ca58-137">Create a simple Spring Boot application with hello Spring Initializr</span></span>

1. <span data-ttu-id="5ca58-138">Bläddra för<https://start.spring.io/>.</span><span class="sxs-lookup"><span data-stu-id="5ca58-138">Browse too<https://start.spring.io/>.</span></span>

1. <span data-ttu-id="5ca58-139">Ange att du vill toogenerate en **Maven** projektet med **Java**, ange hello **grupp** och **artefakt** namn för ditt program och Klicka på knappen hello för**generera projekt**.</span><span class="sxs-lookup"><span data-stu-id="5ca58-139">Specify that you want toogenerate a **Maven** project with **Java**, enter hello **Group** and **Artifact** names for your application, and then click hello button too**Generate Project**.</span></span>

   ![Basic-källan Initializr alternativ][SI01]

   > [!NOTE]
   >
   > <span data-ttu-id="5ca58-141">Hej Vårversionen Initializr använder hello **grupp** och **artefakt** toocreate hello paketnamn, till exempel: *com.example.wintiptoys*.</span><span class="sxs-lookup"><span data-stu-id="5ca58-141">hello Spring Initializr uses hello **Group** and **Artifact** names toocreate hello package name; for example: *com.example.wintiptoys*.</span></span>
   >

1. <span data-ttu-id="5ca58-142">När du uppmanas, hämta hello projektet tooa sökväg på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="5ca58-142">When prompted, download hello project tooa path on your local computer.</span></span>

   ![Hämta anpassade källan Start-projekt][SI02]

1. <span data-ttu-id="5ca58-144">När du har extraherat hello filer i det lokala systemet vara enkla källan Start programmet klar för redigering.</span><span class="sxs-lookup"><span data-stu-id="5ca58-144">After you have extracted hello files on your local system, your simple Spring Boot application will be ready for editing.</span></span>

   ![Projektfiler för anpassade källan Start][SI03]

## <a name="configure-your-spring-boot-app-toouse-hello-azure-spring-boot-starter"></a><span data-ttu-id="5ca58-146">Konfigurera din källan Start app toouse hello Azure källan Start Starter</span><span class="sxs-lookup"><span data-stu-id="5ca58-146">Configure your Spring Boot app toouse hello Azure Spring Boot Starter</span></span>

1. <span data-ttu-id="5ca58-147">Leta upp hello *pom.xml* filen i hello katalogen för din app, till exempel:</span><span class="sxs-lookup"><span data-stu-id="5ca58-147">Locate hello *pom.xml* file in hello directory of your app; for example:</span></span>

   `C:\SpringBoot\wingtiptoys\pom.xml`

   <span data-ttu-id="5ca58-148">ELLER</span><span class="sxs-lookup"><span data-stu-id="5ca58-148">-or-</span></span>

   `/users/example/home/wingtiptoys/pom.xml`

   ![Leta upp hello pom.xml fil][PM01]

1. <span data-ttu-id="5ca58-150">Öppna hello *pom.xml* filen i en textredigerare och Lägg till följande rader toolist av hello `<dependencies>`:</span><span class="sxs-lookup"><span data-stu-id="5ca58-150">Open hello *pom.xml* file in a text editor, and add hello following lines toolist of `<dependencies>`:</span></span>

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-documentdb-spring-boot-starter</artifactId>
      <version>0.1.4</version>
   </dependency>
   ```

   ![Hej Filredigering pom.xml][PM02]

1. <span data-ttu-id="5ca58-152">Spara och Stäng hello *pom.xml* fil.</span><span class="sxs-lookup"><span data-stu-id="5ca58-152">Save and close hello *pom.xml* file.</span></span>

## <a name="configure-your-spring-boot-app-toouse-your-azure-cosmos-db"></a><span data-ttu-id="5ca58-153">Konfigurera din app källan Start-toouse Azure Cosmos-DB</span><span class="sxs-lookup"><span data-stu-id="5ca58-153">Configure your Spring Boot app toouse your Azure Cosmos DB</span></span>

1. <span data-ttu-id="5ca58-154">Leta upp hello *application.properties* filen i hello *resurser* katalogen för din app, till exempel:</span><span class="sxs-lookup"><span data-stu-id="5ca58-154">Locate hello *application.properties* file in hello *resources* directory of your app; for example:</span></span>

   `C:\SpringBoot\wingtiptoys\src\main\resources\application.properties`

   <span data-ttu-id="5ca58-155">ELLER</span><span class="sxs-lookup"><span data-stu-id="5ca58-155">-or-</span></span>

   `/users/example/home/wingtiptoys/src/main/resources/application.properties`

   ![Leta upp hello application.properties fil][RE01]

1. <span data-ttu-id="5ca58-157">Öppna hello *application.properties* filen i en textredigerare och Lägg till följande rader toohello hello och Ersätt hello exempelvärden med hello lämpliga egenskaper för databasen:</span><span class="sxs-lookup"><span data-stu-id="5ca58-157">Open hello *application.properties* file in a text editor, and add hello following lines toohello file, and replace hello sample values with hello appropriate properties for your database:</span></span>

   ```yaml
   # Specify hello DNS URI of your Azure Cosmos DB.
   azure.documentdb.uri=https://wingtiptoys.documents.azure.com:443/

   # Specify hello access key for your database.
   azure.documentdb.key=57686f6120447564652c20426f6220526f636b73==

   # Specify hello name of your database.
   azure.documentdb.database=wingtiptoysdata
   ```

   ![Hej Filredigering application.properties][RE02]

1. <span data-ttu-id="5ca58-159">Spara och Stäng hello *application.properties* fil.</span><span class="sxs-lookup"><span data-stu-id="5ca58-159">Save and close hello *application.properties* file.</span></span>

## <a name="add-sample-code-tooimplement-basic-database-functionality"></a><span data-ttu-id="5ca58-160">Lägga till exempel kod tooimplement grundläggande funktioner</span><span class="sxs-lookup"><span data-stu-id="5ca58-160">Add sample code tooimplement basic database functionality</span></span>

<span data-ttu-id="5ca58-161">I det här avsnittet skapar du två Java-klasser för lagring av användardata och sedan du ändra din programmets klass toocreate en instans av klassen hello och spara den tooyour databas.</span><span class="sxs-lookup"><span data-stu-id="5ca58-161">In this section you create two Java classes for storing user data, and then you modify your main application class toocreate an instance of hello user class and save it tooyour database.</span></span>

### <a name="define-a-basic-class-for-storing-user-data"></a><span data-ttu-id="5ca58-162">Definiera en grundläggande klass för lagring av användardata</span><span class="sxs-lookup"><span data-stu-id="5ca58-162">Define a basic class for storing user data</span></span>

1. <span data-ttu-id="5ca58-163">Skapa en ny fil med namnet *User.java* i hello samma katalog som programmets Java-huvudfil.</span><span class="sxs-lookup"><span data-stu-id="5ca58-163">Create a new file named *User.java* in hello same directory as your main application Java file.</span></span>

1. <span data-ttu-id="5ca58-164">Öppna hello *User.java* filen i en textredigerare och Lägg till följande hello rader toohello filen toodefine en allmän användarklass som lagrar och hämtar värden i databasen:</span><span class="sxs-lookup"><span data-stu-id="5ca58-164">Open hello *User.java* file in a text editor, and add hello following lines toohello file toodefine a generic user class that stores and retrieve values in your database:</span></span>

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

1. <span data-ttu-id="5ca58-165">Spara och Stäng hello *User.java* fil.</span><span class="sxs-lookup"><span data-stu-id="5ca58-165">Save and close hello *User.java* file.</span></span>

### <a name="define-a-data-repository-interface"></a><span data-ttu-id="5ca58-166">Definiera ett gränssnitt för databasen</span><span class="sxs-lookup"><span data-stu-id="5ca58-166">Define a data repository interface</span></span>

1. <span data-ttu-id="5ca58-167">Skapa en ny fil med namnet *UserRepository.java* i hello samma katalog som programmets Java-huvudfil.</span><span class="sxs-lookup"><span data-stu-id="5ca58-167">Create a new file named *UserRepository.java* in hello same directory as your main application Java file.</span></span>

1. <span data-ttu-id="5ca58-168">Öppna hello *UserRepository.java* filen i en textredigerare och Lägg till hello följande rader toohello filen toodefine ett användargränssnitt för databasen som utökar hello standardgränssnittet DocumentDB databasen:</span><span class="sxs-lookup"><span data-stu-id="5ca58-168">Open hello *UserRepository.java* file in a text editor, and add hello following lines toohello file toodefine a user repository interface that extends hello default DocumentDB repository interface:</span></span>

   ```java
   package com.example.wingtiptoys;

   import com.microsoft.azure.spring.data.documentdb.repository.DocumentDbRepository;
   import org.springframework.stereotype.Repository;

   @Repository
   public interface UserRepository extends DocumentDbRepository<User, String> {}   
   ```

1. <span data-ttu-id="5ca58-169">Spara och Stäng hello *UserRepository.java* fil.</span><span class="sxs-lookup"><span data-stu-id="5ca58-169">Save and close hello *UserRepository.java* file.</span></span>

### <a name="modify-hello-main-application-class"></a><span data-ttu-id="5ca58-170">Ändra hello programmets klass</span><span class="sxs-lookup"><span data-stu-id="5ca58-170">Modify hello main application class</span></span>

1. <span data-ttu-id="5ca58-171">Leta upp hello programmets Java huvudfil i hello paketkatalogen i din app; Exempel:</span><span class="sxs-lookup"><span data-stu-id="5ca58-171">Locate hello main application Java file in hello package directory of your app; for example:</span></span>

   `C:\SpringBoot\wingtiptoys\src\main\java\com\example\wingtiptoys\WingtiptoysApplication.java`

   <span data-ttu-id="5ca58-172">ELLER</span><span class="sxs-lookup"><span data-stu-id="5ca58-172">-or-</span></span>

   `/users/example/home/wingtiptoys/src/main/java/com/example/wingtiptoys/WingtiptoysApplication.java`

   ![Leta upp hello programfilen Java][JV01]

1. <span data-ttu-id="5ca58-174">Öppna hello programmets Java-filen i en textredigerare och Lägg till följande rader toohello hello:</span><span class="sxs-lookup"><span data-stu-id="5ca58-174">Open hello main application Java file in a text editor, and add hello following lines toohello file:</span></span>

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

1. <span data-ttu-id="5ca58-175">Spara och Stäng hello programmets Java huvudfil.</span><span class="sxs-lookup"><span data-stu-id="5ca58-175">Save and close hello main application Java file.</span></span>

## <a name="build-and-test-your-app"></a><span data-ttu-id="5ca58-176">Skapa och testa din app</span><span class="sxs-lookup"><span data-stu-id="5ca58-176">Build and test your app</span></span>

1. <span data-ttu-id="5ca58-177">Öppna en kommandotolk och ändra katalogen toohello mappen där din *pom.xml* finns, till exempel:</span><span class="sxs-lookup"><span data-stu-id="5ca58-177">Open a command prompt and change directory toohello folder where your *pom.xml* file is located; for example:</span></span>

   `cd C:\SpringBoot\wingtiptoys`

   <span data-ttu-id="5ca58-178">ELLER</span><span class="sxs-lookup"><span data-stu-id="5ca58-178">-or-</span></span>

   `cd /users/example/home/wingtiptoys`

1. <span data-ttu-id="5ca58-179">Skapa din källan startprogrammet med Maven och kör den. Exempel:</span><span class="sxs-lookup"><span data-stu-id="5ca58-179">Build your Spring Boot application with Maven and run it; for example:</span></span>

   ```shell
   mvn package
   java -jar target/wingtiptoys-0.0.1-SNAPSHOT.jar
   ```

1. <span data-ttu-id="5ca58-180">Programmet visas flera runtime-meddelanden och du bör se hello-meddelande `User: testFirstName testLastName` visas tooindicate som värden har lagras och hämtas från databasen.</span><span class="sxs-lookup"><span data-stu-id="5ca58-180">Your application will display several runtime messages, and you should see hello message `User: testFirstName testLastName` displayed tooindicate that values have been successfully stored and retrieved from your database.</span></span>

   ![Lyckad utdata från hello-program][JV02]

1. <span data-ttu-id="5ca58-182">Valfritt: Du kan använda hello Azure portal tooview hello innehållet i din Azure-Cosmos-DB från hello egenskapssidan för din databas genom att klicka på **Dokumentutforskaren**, och sedan välja och objekt från hello visas listan tooview hello innehållet.</span><span class="sxs-lookup"><span data-stu-id="5ca58-182">OPTIONAL: You can use hello Azure portal tooview hello contents of your Azure Cosmos DB from hello properties page for your database by clicking  **Document Explorer**, and then selecting and item from hello displayed list tooview hello contents.</span></span>

   ![Med hjälp av hello Dokumentutforskaren tooview dina data][JV03]

## <a name="next-steps"></a><span data-ttu-id="5ca58-184">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5ca58-184">Next steps</span></span>

<span data-ttu-id="5ca58-185">Mer information om hur du använder Azure Cosmos DB och Java Se hello följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="5ca58-185">For more information about using Azure Cosmos DB and Java, see hello following articles:</span></span>

* <span data-ttu-id="5ca58-186">[Dokumentation för Azure Cosmos DB].</span><span class="sxs-lookup"><span data-stu-id="5ca58-186">[Azure Cosmos DB Documentation].</span></span>

* <span data-ttu-id="5ca58-187">[Azure DB Cosmos: Skapa en DocumentDB-API-app med Java och hello Azure-portalen][Build a DocumentDB API app with Java]</span><span class="sxs-lookup"><span data-stu-id="5ca58-187">[Azure Cosmos DB: Build a DocumentDB API app with Java and hello Azure portal][Build a DocumentDB API app with Java]</span></span>

<span data-ttu-id="5ca58-188">Mer information om hur du använder värdet startprogram i Azure finns i hello följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="5ca58-188">For more information about using Spring Boot applications on Azure, see hello following articles:</span></span>

* [<span data-ttu-id="5ca58-189">Källan Start DocumenDB Starter för Azure</span><span class="sxs-lookup"><span data-stu-id="5ca58-189">Spring Boot DocumenDB Starter for Azure</span></span>](https://github.com/Microsoft/azure-spring-boot-starters/tree/master/azure-documentdb-spring-boot-starter-sample)

* [<span data-ttu-id="5ca58-190">Distribuera en källan startprogrammet toohello Azure App Service</span><span class="sxs-lookup"><span data-stu-id="5ca58-190">Deploy a Spring Boot Application toohello Azure App Service</span></span>](../app-service/app-service-deploy-spring-boot-web-app-on-azure.md)

* [<span data-ttu-id="5ca58-191">Kör ett program för start av värdet på ett kluster med Kubernetes på hello Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="5ca58-191">Running a Spring Boot Application on a Kubernetes Cluster in hello Azure Container Service</span></span>](../container-service/container-service-deploy-spring-boot-app-on-kubernetes.md)

<span data-ttu-id="5ca58-192">Mer information om hur du använder Azure med Java finns hello [Azure Java Developer Center] och hello [Java-verktyg för Visual Studio Team Services].</span><span class="sxs-lookup"><span data-stu-id="5ca58-192">For more information about using Azure with Java, see hello [Azure Java Developer Center] and hello [Java Tools for Visual Studio Team Services].</span></span>

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
