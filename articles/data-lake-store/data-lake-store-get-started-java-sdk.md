---
title: aaaUse hello Java SDK toodevelop program i Azure Data Lake Store | Microsoft Docs
description: "Använda Azure Data Lake Store Java SDK toocreate ett Data Lake Store-konto och utföra grundläggande åtgärder i hello Data Lake Store"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: d10e09db-5232-4e84-bb50-52efc2c21887
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/28/2017
ms.author: nitinme
ms.openlocfilehash: d3bcee449c2a2a4bd2f7b241af46ecc010b6b62e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-data-lake-store-using-java"></a>Kom igång med Azure Data Lake Store med hjälp av Java
> [!div class="op_single_selector"]
> * [Portalen](data-lake-store-get-started-portal.md)
> * [PowerShell](data-lake-store-get-started-powershell.md)
> * [.NET SDK](data-lake-store-get-started-net-sdk.md)
> * [Java SDK](data-lake-store-get-started-java-sdk.md)
> * [REST-API](data-lake-store-get-started-rest-api.md)
> * [Azure CLI 2.0](data-lake-store-get-started-cli-2.0.md)
> * [Node.js](data-lake-store-manage-use-nodejs.md)
> * [Python](data-lake-store-get-started-python.md)
>
> 

Lär dig hur toouse hello Azure Data Lake Store Java SDK tooperform grundläggande åtgärder som att skapa mappar, ladda upp och hämta datafiler, osv. Mer information om Data Lake finns i [Azure Data Lake Store](data-lake-store-overview.md).

Du kan komma åt hello Java SDK API-dokumentation för Azure Data Lake Store på [Azure Data Lake Store Java API docs](https://azure.github.io/azure-data-lake-store-java/javadoc/).

## <a name="prerequisites"></a>Krav
* Java Development Kit (JDK 7 eller senare, med Java version 1.7 eller senare)
* Azure Data Lake Store-konto. Följ instruktionerna för hello på [Kom igång med Azure Data Lake Store med hjälp av hello Azure Portal](data-lake-store-get-started-portal.md).
* [Maven](https://maven.apache.org/install.html). Den här självstudien använder Maven för bygg- och projektberoenden. Men det är möjligt toobuild utan att använda ett build-system som Maven eller Gradle, är dessa system Kontrollera mycket enklare toomanage beroenden.
* (Valfritt) Och IDE-liknande [IntelliJ IDEA](https://www.jetbrains.com/idea/download/) eller [Eclipse](https://www.eclipse.org/downloads/) eller liknande.

## <a name="how-do-i-authenticate-using-azure-active-directory"></a>Hur autentiserar jag med Azure Active Directory?
I den här kursen använder vi en hemlig tooretrieve för Azure AD application klienten på en Azure Active Directory-token (service to service autentisering). Vi använder den här token toocreate ett Data Lake Store-objektet tooperform operations klientfil och directory åtgärder. Anvisningar för hur tooauthenticate med Azure Data Lake Store med hjälp av hello klienthemlighet, utför vi hello följande anvisningar:

1. Skapa en Azure AD-webbapp
2. Hämta hello klient-ID och klienthemlighet token slutpunkt för hello Azure AD-webbprogram.
3. Konfigurera åtkomst för hello Azure AD-webbprogrammet på hello Data Lake Store filen/mappen som du vill tooaccess från hello Java-program som du skapar.

Anvisningar för hur tooperform stegen, se [skapa ett Active Directory-program](data-lake-store-authenticate-using-active-directory.md).

Azure Active Directory erbjuder andra alternativ samt tooretrieve en token. Du kan välja från ett antal olika mekanismer toosuit ditt scenario, till exempel ett program som körs i en webbläsare, ett program som distribueras som ett skrivbordsprogram eller ett serverprogram som körs lokalt eller i en virtuell Azure datorn. Du kan också välja mellan olika typer av autentiseringsuppgifter som lösenord, certifikat, tvåfaktorautentisering och så vidare. Dessutom Azure Active Directory kan du toosynchronize din lokala Active Directory-användare med hello molnet. För information, se [Autentiseringsscenarier för Azure Active Directory](../active-directory/active-directory-authentication-scenarios.md). 

## <a name="create-a-java-application"></a>Skapa ett Java-program
hello-kodexempel tillgängliga [på GitHub](https://azure.microsoft.com/documentation/samples/data-lake-store-java-upload-download-get-started/) vägleder dig genom hello processen att skapa filer i hello arkivet, sammanfoga filer, hämta en fil och ta bort några filer i hello store. Det här avsnittet av hello artikeln vägleder dig genom hello huvuddelar hello kod.

1. Skapa ett Maven-projekt med [mvn archetype](https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html) hello från kommandoraden eller med hjälp av IDE-miljö. Anvisningar för hur toocreate en Java projekt med IntelliJ finns [här](https://www.jetbrains.com/help/idea/2016.1/creating-and-running-your-first-java-application.html). Anvisningar för hur toocreate ett projekt med Eclipse finns [här](http://help.eclipse.org/mars/index.jsp?topic=%2Forg.eclipse.jdt.doc.user%2FgettingStarted%2Fqs-3.htm). 
2. Lägg till följande beroenden tooyour Maven hello **pom.xml** fil. Lägg till följande fragment av text mellan hello hello  **\</version >** taggen och hello  **\</project >** tagg:
   
        <dependencies>
          <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>azure-data-lake-store-sdk</artifactId>
            <version>2.1.5</version>
          </dependency>
          <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-nop</artifactId>
            <version>1.7.21</version>
          </dependency>
        </dependencies>
   
    hello första beroende är toouse hello Data Lake Store SDK (`azure-data-lake-store-sdk`) från hello maven-databasen. Hej andra beroende (`slf4j-nop`) är toospecify vilka loggning framework toouse för det här programmet. hello Data Lake Store SDK använder [slf4j](http://www.slf4j.org/) loggning facade, där du kan välja mellan ett antal olika populära loggning ramverk som log4j, Java logging logback osv., eller ingen loggning. Vi kommer att inaktivera loggning för det här exemplet, använder vi därför hello **slf4j nop** bindning. toouse andra alternativ för loggning i din app Se [här](http://www.slf4j.org/manual.html#projectDep).

### <a name="add-hello-application-code"></a>Lägg till hello programkod
Det finns tre delar toohello kod.

1. Hämta hello Azure Active Directory-token
2. Använd hello token toocreate ett Data Lake Store-klienten.
3. Använd hello Data Lake Store tooperform Klientåtgärder.

#### <a name="step-1-obtain-an-azure-active-directory-token"></a>Steg 1: Hämta en Azure Active Directory-token.
hello Data Lake Store SDK ger praktiska metoder som gör att du kan hantera hello säkerhetstoken behövs tootalk toohello Data Lake Store-konto. Dock tvingade hello SDK inte att endast dessa metoder kan användas. Du kan använda något annat sätt för att hämta token, t.ex. använder hello [Azure Active Directory SDK](https://github.com/AzureAD/azure-activedirectory-library-for-java), eller din egen kod.

toouse hello Data Lake Store SDK tooobtain token för hello Active Directory webbprogrammet du skapade tidigare, Använd en av hello underklasser för `AccessTokenProvider` (hello exemplet nedan används `ClientCredsTokenProvider`). hello tokenleverantör cacheminnen hello inloggningsuppgifter tooobtain hello token som används i minnet och förnyas automatiskt hello token om det handlar om tooexpire. Det är möjligt toocreate egna underklasser för `AccessTokenProvider` så token hämtas av kunden koden, men nu ska vi bara Använd hello en enligt hello SDK.

Ersätt **Fyll-här** med hello hello Azure Active Directory webbprogram med verkliga värden.

    private static String clientId = "FILL-IN-HERE";
    private static String authTokenEndpoint = "FILL-IN-HERE";
    private static String clientKey = "FILL-IN-HERE";

    AccessTokenProvider provider = new ClientCredsTokenProvider(authTokenEndpoint, clientId, clientKey);

#### <a name="step-2-create-an-azure-data-lake-store-client-adlstoreclient-object"></a>Steg 2: Skapa ett Azure Data Lake Store-klientobjekt (ADLStoreClient)
Skapa en [ADLStoreClient](https://azure.github.io/azure-data-lake-store-java/javadoc/) objekt kräver toospecify hello Data Lake Store-konto namn och hello tokenleverantör du genererade i hello sista steget. Observera att hello Data Lake Store-konto namn måste toobe ett fullständigt kvalificerat domännamn. Ersätt till exempel **FILL-IN-HERE** med något som **mydatalakestore.azuredatalakestore.net**.

    private static String accountFQDN = "FILL-IN-HERE";  // full account FQDN, not just hello account name
    ADLStoreClient client = ADLStoreClient.createClient(accountFQDN, provider);

### <a name="step-3-use-hello-adlstoreclient-tooperform-file-and-directory-operations"></a>Steg 3: Använd hello ADLStoreClient tooperform fil- och åtgärder
hello koden nedan innehåller exempel kodavsnitt för några vanliga åtgärder. Du kan titta på hello fullständig [Data Lake Store Java SDK API docs](https://azure.github.io/azure-data-lake-store-java/javadoc/) av hello **ADLStoreClient** objekt toosee andra åtgärder.

Observera att filer läses från och skrivs till med hjälp av standard Java-strömmar. Det innebär att du layer någon hello Java dataströmmar ovanpå hello Data Lake Store-strömmar toobenefit från vanliga Java-funktioner (t.ex. Skriv ut dataströmmar för formaterade utdata eller någon av hello komprimera eller kryptera dataströmmar för ytterligare funktioner överkant, osv).

     // create file and write some content
     String filename = "/a/b/c.txt";
     OutputStream stream = client.createFile(filename, IfExists.OVERWRITE  );
     PrintStream out = new PrintStream(stream);
     for (int i = 1; i <= 10; i++) {
         out.println("This is line #" + i);
         out.format("This is hello same line (%d), but using formatted output. %n", i);
     }
     out.close();
    
    // set file permission
    client.setPermission(filename, "744");

    // append toofile
    stream = client.getAppendStream(filename);
    stream.write(getSampleContent());
    stream.close();

    // Read File
    InputStream in = client.getReadStream(filename);
    byte[] b = new byte[64000];
    while (in.read(b) != -1) {
        System.out.write(b);
    }
    in.close();

    // concatenate hello two files into one
    List<String> fileList = Arrays.asList("/a/b/c.txt", "/a/b/d.txt");
    client.concatenateFiles("/a/b/f.txt", fileList);

    //rename hello file
    client.rename("/a/b/f.txt", "/a/b/g.txt");

    // list directory contents
    List<DirectoryEntry> list = client.enumerateDirectory("/a/b", 2000);
    System.out.println("Directory listing for directory /a/b:");
    for (DirectoryEntry entry : list) {
        printDirectoryInfo(entry);
    }

    // delete directory along with all hello subdirectories and files in it
    client.deleteRecursive("/a");

#### <a name="step-4-build-and-run-hello-application"></a>Steg 4: Skapa och köra programmet hello
1. toorun från inom ett IDE, leta upp och tryck på hello **kör** knappen. toorun från Maven, Använd [exec: exec](http://www.mojohaus.org/exec-maven-plugin/exec-mojo.html).
2. tooproduce en fristående jar som körs från kommandoraden build hello jar med alla beroenden som ingår, med hello [Maven sammansättningen plugin](http://maven.apache.org/plugins/maven-assembly-plugin/usage.html). Hej pom.xml i hello [exempel källkoden på github](https://github.com/Azure-Samples/data-lake-store-java-upload-download-get-started/blob/master/pom.xml) innehåller ett exempel på hur toodo detta.

## <a name="next-steps"></a>Nästa steg
* [Utforska JavaDoc för hello Java SDK](https://azure.github.io/azure-data-lake-store-java/javadoc/)
* [Säkra data i Data Lake Store](data-lake-store-secure-data.md)
* [Använd Azure Data Lake Analytics med Data Lake Store](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Använd Azure HDInsight med Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md)

