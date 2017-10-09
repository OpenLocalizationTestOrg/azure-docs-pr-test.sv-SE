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
# <a name="get-started-with-azure-data-lake-store-using-java"></a><span data-ttu-id="eff64-103">Kom igång med Azure Data Lake Store med hjälp av Java</span><span class="sxs-lookup"><span data-stu-id="eff64-103">Get started with Azure Data Lake Store using Java</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="eff64-104">Portalen</span><span class="sxs-lookup"><span data-stu-id="eff64-104">Portal</span></span>](data-lake-store-get-started-portal.md)
> * [<span data-ttu-id="eff64-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="eff64-105">PowerShell</span></span>](data-lake-store-get-started-powershell.md)
> * [<span data-ttu-id="eff64-106">.NET SDK</span><span class="sxs-lookup"><span data-stu-id="eff64-106">.NET SDK</span></span>](data-lake-store-get-started-net-sdk.md)
> * [<span data-ttu-id="eff64-107">Java SDK</span><span class="sxs-lookup"><span data-stu-id="eff64-107">Java SDK</span></span>](data-lake-store-get-started-java-sdk.md)
> * [<span data-ttu-id="eff64-108">REST-API</span><span class="sxs-lookup"><span data-stu-id="eff64-108">REST API</span></span>](data-lake-store-get-started-rest-api.md)
> * [<span data-ttu-id="eff64-109">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="eff64-109">Azure CLI 2.0</span></span>](data-lake-store-get-started-cli-2.0.md)
> * [<span data-ttu-id="eff64-110">Node.js</span><span class="sxs-lookup"><span data-stu-id="eff64-110">Node.js</span></span>](data-lake-store-manage-use-nodejs.md)
> * [<span data-ttu-id="eff64-111">Python</span><span class="sxs-lookup"><span data-stu-id="eff64-111">Python</span></span>](data-lake-store-get-started-python.md)
>
> 

<span data-ttu-id="eff64-112">Lär dig hur toouse hello Azure Data Lake Store Java SDK tooperform grundläggande åtgärder som att skapa mappar, ladda upp och hämta datafiler, osv. Mer information om Data Lake finns i [Azure Data Lake Store](data-lake-store-overview.md).</span><span class="sxs-lookup"><span data-stu-id="eff64-112">Learn how toouse hello Azure Data Lake Store Java SDK tooperform basic operations such as create folders, upload and download data files, etc. For more information about Data Lake, see [Azure Data Lake Store](data-lake-store-overview.md).</span></span>

<span data-ttu-id="eff64-113">Du kan komma åt hello Java SDK API-dokumentation för Azure Data Lake Store på [Azure Data Lake Store Java API docs](https://azure.github.io/azure-data-lake-store-java/javadoc/).</span><span class="sxs-lookup"><span data-stu-id="eff64-113">You can access hello Java SDK API docs for Azure Data Lake Store at [Azure Data Lake Store Java API docs](https://azure.github.io/azure-data-lake-store-java/javadoc/).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="eff64-114">Krav</span><span class="sxs-lookup"><span data-stu-id="eff64-114">Prerequisites</span></span>
* <span data-ttu-id="eff64-115">Java Development Kit (JDK 7 eller senare, med Java version 1.7 eller senare)</span><span class="sxs-lookup"><span data-stu-id="eff64-115">Java Development Kit (JDK 7 or higher, using Java version 1.7 or higher)</span></span>
* <span data-ttu-id="eff64-116">Azure Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="eff64-116">Azure Data Lake Store account.</span></span> <span data-ttu-id="eff64-117">Följ instruktionerna för hello på [Kom igång med Azure Data Lake Store med hjälp av hello Azure Portal](data-lake-store-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="eff64-117">Follow hello instructions at [Get started with Azure Data Lake Store using hello Azure Portal](data-lake-store-get-started-portal.md).</span></span>
* <span data-ttu-id="eff64-118">[Maven](https://maven.apache.org/install.html).</span><span class="sxs-lookup"><span data-stu-id="eff64-118">[Maven](https://maven.apache.org/install.html).</span></span> <span data-ttu-id="eff64-119">Den här självstudien använder Maven för bygg- och projektberoenden.</span><span class="sxs-lookup"><span data-stu-id="eff64-119">This tutorial uses Maven for build and project dependencies.</span></span> <span data-ttu-id="eff64-120">Men det är möjligt toobuild utan att använda ett build-system som Maven eller Gradle, är dessa system Kontrollera mycket enklare toomanage beroenden.</span><span class="sxs-lookup"><span data-stu-id="eff64-120">Although it is possible toobuild without using a build system like Maven or Gradle, these systems make is much easier toomanage dependencies.</span></span>
* <span data-ttu-id="eff64-121">(Valfritt) Och IDE-liknande [IntelliJ IDEA](https://www.jetbrains.com/idea/download/) eller [Eclipse](https://www.eclipse.org/downloads/) eller liknande.</span><span class="sxs-lookup"><span data-stu-id="eff64-121">(Optional) And IDE like [IntelliJ IDEA](https://www.jetbrains.com/idea/download/) or [Eclipse](https://www.eclipse.org/downloads/) or similar.</span></span>

## <a name="how-do-i-authenticate-using-azure-active-directory"></a><span data-ttu-id="eff64-122">Hur autentiserar jag med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="eff64-122">How do I authenticate using Azure Active Directory?</span></span>
<span data-ttu-id="eff64-123">I den här kursen använder vi en hemlig tooretrieve för Azure AD application klienten på en Azure Active Directory-token (service to service autentisering).</span><span class="sxs-lookup"><span data-stu-id="eff64-123">In this tutorial we use a Azure AD application client secret tooretrieve an Azure Active Directory token (service-to-service authentication).</span></span> <span data-ttu-id="eff64-124">Vi använder den här token toocreate ett Data Lake Store-objektet tooperform operations klientfil och directory åtgärder.</span><span class="sxs-lookup"><span data-stu-id="eff64-124">We use this token toocreate an Data Lake Store client object tooperform operations file and directory operations.</span></span> <span data-ttu-id="eff64-125">Anvisningar för hur tooauthenticate med Azure Data Lake Store med hjälp av hello klienthemlighet, utför vi hello följande anvisningar:</span><span class="sxs-lookup"><span data-stu-id="eff64-125">For instructions on how tooauthenticate with Azure Data Lake Store using hello client secret, we perform hello following high-level steps:</span></span>

1. <span data-ttu-id="eff64-126">Skapa en Azure AD-webbapp</span><span class="sxs-lookup"><span data-stu-id="eff64-126">Create an Azure AD web application</span></span>
2. <span data-ttu-id="eff64-127">Hämta hello klient-ID och klienthemlighet token slutpunkt för hello Azure AD-webbprogram.</span><span class="sxs-lookup"><span data-stu-id="eff64-127">Retrieve hello client ID, client secret, and token endpoint for hello Azure AD web application.</span></span>
3. <span data-ttu-id="eff64-128">Konfigurera åtkomst för hello Azure AD-webbprogrammet på hello Data Lake Store filen/mappen som du vill tooaccess från hello Java-program som du skapar.</span><span class="sxs-lookup"><span data-stu-id="eff64-128">Configure access for hello Azure AD web application on hello Data Lake Store file/folder that you want tooaccess from hello Java application you are creating.</span></span>

<span data-ttu-id="eff64-129">Anvisningar för hur tooperform stegen, se [skapa ett Active Directory-program](data-lake-store-authenticate-using-active-directory.md).</span><span class="sxs-lookup"><span data-stu-id="eff64-129">For instructions on how tooperform these steps, see [Create an Active Directory application](data-lake-store-authenticate-using-active-directory.md).</span></span>

<span data-ttu-id="eff64-130">Azure Active Directory erbjuder andra alternativ samt tooretrieve en token.</span><span class="sxs-lookup"><span data-stu-id="eff64-130">Azure Active Directory provides other options as well tooretrieve a token.</span></span> <span data-ttu-id="eff64-131">Du kan välja från ett antal olika mekanismer toosuit ditt scenario, till exempel ett program som körs i en webbläsare, ett program som distribueras som ett skrivbordsprogram eller ett serverprogram som körs lokalt eller i en virtuell Azure datorn.</span><span class="sxs-lookup"><span data-stu-id="eff64-131">You can pick from a number of different authentication mechanisms toosuit your scenario, for example, an application running in a browser, an application distributed as a desktop application, or a server application running on-premises or in an Azure virtual machine.</span></span> <span data-ttu-id="eff64-132">Du kan också välja mellan olika typer av autentiseringsuppgifter som lösenord, certifikat, tvåfaktorautentisering och så vidare. Dessutom Azure Active Directory kan du toosynchronize din lokala Active Directory-användare med hello molnet.</span><span class="sxs-lookup"><span data-stu-id="eff64-132">You can also pick from different types of credentials like passwords, certificates, 2-factor authentication, etc. In addition, Azure Active Directory allows you toosynchronize your on-premises Active Directory users with hello cloud.</span></span> <span data-ttu-id="eff64-133">För information, se [Autentiseringsscenarier för Azure Active Directory](../active-directory/active-directory-authentication-scenarios.md).</span><span class="sxs-lookup"><span data-stu-id="eff64-133">For details, see [Authentication Scenarios for Azure Active Directory](../active-directory/active-directory-authentication-scenarios.md).</span></span> 

## <a name="create-a-java-application"></a><span data-ttu-id="eff64-134">Skapa ett Java-program</span><span class="sxs-lookup"><span data-stu-id="eff64-134">Create a Java application</span></span>
<span data-ttu-id="eff64-135">hello-kodexempel tillgängliga [på GitHub](https://azure.microsoft.com/documentation/samples/data-lake-store-java-upload-download-get-started/) vägleder dig genom hello processen att skapa filer i hello arkivet, sammanfoga filer, hämta en fil och ta bort några filer i hello store.</span><span class="sxs-lookup"><span data-stu-id="eff64-135">hello code sample available [on GitHub](https://azure.microsoft.com/documentation/samples/data-lake-store-java-upload-download-get-started/) walks you through hello process of creating files in hello store, concatenating files, downloading a file, and deleting some files in hello store.</span></span> <span data-ttu-id="eff64-136">Det här avsnittet av hello artikeln vägleder dig genom hello huvuddelar hello kod.</span><span class="sxs-lookup"><span data-stu-id="eff64-136">This section of hello article walk you through hello main parts of hello code.</span></span>

1. <span data-ttu-id="eff64-137">Skapa ett Maven-projekt med [mvn archetype](https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html) hello från kommandoraden eller med hjälp av IDE-miljö.</span><span class="sxs-lookup"><span data-stu-id="eff64-137">Create a Maven project using [mvn archetype](https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html) from hello command-line or using an IDE.</span></span> <span data-ttu-id="eff64-138">Anvisningar för hur toocreate en Java projekt med IntelliJ finns [här](https://www.jetbrains.com/help/idea/2016.1/creating-and-running-your-first-java-application.html).</span><span class="sxs-lookup"><span data-stu-id="eff64-138">For instructions on how toocreate a Java project using IntelliJ, see [here](https://www.jetbrains.com/help/idea/2016.1/creating-and-running-your-first-java-application.html).</span></span> <span data-ttu-id="eff64-139">Anvisningar för hur toocreate ett projekt med Eclipse finns [här](http://help.eclipse.org/mars/index.jsp?topic=%2Forg.eclipse.jdt.doc.user%2FgettingStarted%2Fqs-3.htm).</span><span class="sxs-lookup"><span data-stu-id="eff64-139">For instructions on how toocreate a project using Eclipse, see [here](http://help.eclipse.org/mars/index.jsp?topic=%2Forg.eclipse.jdt.doc.user%2FgettingStarted%2Fqs-3.htm).</span></span> 
2. <span data-ttu-id="eff64-140">Lägg till följande beroenden tooyour Maven hello **pom.xml** fil.</span><span class="sxs-lookup"><span data-stu-id="eff64-140">Add hello following dependencies tooyour Maven **pom.xml** file.</span></span> <span data-ttu-id="eff64-141">Lägg till följande fragment av text mellan hello hello  **\</version >** taggen och hello  **\</project >** tagg:</span><span class="sxs-lookup"><span data-stu-id="eff64-141">Add hello following snippet of text between hello **\</version>** tag and hello **\</project>** tag:</span></span>
   
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
   
    <span data-ttu-id="eff64-142">hello första beroende är toouse hello Data Lake Store SDK (`azure-data-lake-store-sdk`) från hello maven-databasen.</span><span class="sxs-lookup"><span data-stu-id="eff64-142">hello first dependency is toouse hello Data Lake Store SDK (`azure-data-lake-store-sdk`) from hello maven repository.</span></span> <span data-ttu-id="eff64-143">Hej andra beroende (`slf4j-nop`) är toospecify vilka loggning framework toouse för det här programmet.</span><span class="sxs-lookup"><span data-stu-id="eff64-143">hello second dependency (`slf4j-nop`) is toospecify which logging framework toouse for this application.</span></span> <span data-ttu-id="eff64-144">hello Data Lake Store SDK använder [slf4j](http://www.slf4j.org/) loggning facade, där du kan välja mellan ett antal olika populära loggning ramverk som log4j, Java logging logback osv., eller ingen loggning.</span><span class="sxs-lookup"><span data-stu-id="eff64-144">hello Data Lake Store SDK uses [slf4j](http://www.slf4j.org/) logging façade, which lets you choose from a number of popular logging frameworks, like log4j, Java logging, logback, etc., or no logging.</span></span> <span data-ttu-id="eff64-145">Vi kommer att inaktivera loggning för det här exemplet, använder vi därför hello **slf4j nop** bindning.</span><span class="sxs-lookup"><span data-stu-id="eff64-145">For this example, we will disable logging, hence we use hello **slf4j-nop** binding.</span></span> <span data-ttu-id="eff64-146">toouse andra alternativ för loggning i din app Se [här](http://www.slf4j.org/manual.html#projectDep).</span><span class="sxs-lookup"><span data-stu-id="eff64-146">toouse other logging options in your app, see [here](http://www.slf4j.org/manual.html#projectDep).</span></span>

### <a name="add-hello-application-code"></a><span data-ttu-id="eff64-147">Lägg till hello programkod</span><span class="sxs-lookup"><span data-stu-id="eff64-147">Add hello application code</span></span>
<span data-ttu-id="eff64-148">Det finns tre delar toohello kod.</span><span class="sxs-lookup"><span data-stu-id="eff64-148">There are three main parts toohello code.</span></span>

1. <span data-ttu-id="eff64-149">Hämta hello Azure Active Directory-token</span><span class="sxs-lookup"><span data-stu-id="eff64-149">Obtain hello Azure Active Directory token</span></span>
2. <span data-ttu-id="eff64-150">Använd hello token toocreate ett Data Lake Store-klienten.</span><span class="sxs-lookup"><span data-stu-id="eff64-150">Use hello token toocreate a Data Lake Store client.</span></span>
3. <span data-ttu-id="eff64-151">Använd hello Data Lake Store tooperform Klientåtgärder.</span><span class="sxs-lookup"><span data-stu-id="eff64-151">Use hello Data Lake Store client tooperform operations.</span></span>

#### <a name="step-1-obtain-an-azure-active-directory-token"></a><span data-ttu-id="eff64-152">Steg 1: Hämta en Azure Active Directory-token.</span><span class="sxs-lookup"><span data-stu-id="eff64-152">Step 1: Obtain an Azure Active Directory token.</span></span>
<span data-ttu-id="eff64-153">hello Data Lake Store SDK ger praktiska metoder som gör att du kan hantera hello säkerhetstoken behövs tootalk toohello Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="eff64-153">hello Data Lake Store SDK provides convenient methods that let you manage hello security tokens needed tootalk toohello Data Lake Store account.</span></span> <span data-ttu-id="eff64-154">Dock tvingade hello SDK inte att endast dessa metoder kan användas.</span><span class="sxs-lookup"><span data-stu-id="eff64-154">However, hello SDK does not mandate that only these methods be used.</span></span> <span data-ttu-id="eff64-155">Du kan använda något annat sätt för att hämta token, t.ex. använder hello [Azure Active Directory SDK](https://github.com/AzureAD/azure-activedirectory-library-for-java), eller din egen kod.</span><span class="sxs-lookup"><span data-stu-id="eff64-155">You can use any other means of obtaining token as well, like using hello [Azure Active Directory SDK](https://github.com/AzureAD/azure-activedirectory-library-for-java), or your own custom code.</span></span>

<span data-ttu-id="eff64-156">toouse hello Data Lake Store SDK tooobtain token för hello Active Directory webbprogrammet du skapade tidigare, Använd en av hello underklasser för `AccessTokenProvider` (hello exemplet nedan används `ClientCredsTokenProvider`).</span><span class="sxs-lookup"><span data-stu-id="eff64-156">toouse hello Data Lake Store SDK tooobtain token for hello Active Directory Web application you created earlier, use one of hello subclasses of `AccessTokenProvider` (hello example below uses `ClientCredsTokenProvider`).</span></span> <span data-ttu-id="eff64-157">hello tokenleverantör cacheminnen hello inloggningsuppgifter tooobtain hello token som används i minnet och förnyas automatiskt hello token om det handlar om tooexpire.</span><span class="sxs-lookup"><span data-stu-id="eff64-157">hello token provider caches hello creds used tooobtain hello token in memory, and automatically renews hello token if it is about tooexpire.</span></span> <span data-ttu-id="eff64-158">Det är möjligt toocreate egna underklasser för `AccessTokenProvider` så token hämtas av kunden koden, men nu ska vi bara Använd hello en enligt hello SDK.</span><span class="sxs-lookup"><span data-stu-id="eff64-158">It is possible toocreate your own subclasses of `AccessTokenProvider` so tokens are obtained by your customer code, but for now let's just use hello one provided in hello SDK.</span></span>

<span data-ttu-id="eff64-159">Ersätt **Fyll-här** med hello hello Azure Active Directory webbprogram med verkliga värden.</span><span class="sxs-lookup"><span data-stu-id="eff64-159">Replace **FILL-IN-HERE** with hello actual values for hello Azure Active Directory Web application.</span></span>

    private static String clientId = "FILL-IN-HERE";
    private static String authTokenEndpoint = "FILL-IN-HERE";
    private static String clientKey = "FILL-IN-HERE";

    AccessTokenProvider provider = new ClientCredsTokenProvider(authTokenEndpoint, clientId, clientKey);

#### <a name="step-2-create-an-azure-data-lake-store-client-adlstoreclient-object"></a><span data-ttu-id="eff64-160">Steg 2: Skapa ett Azure Data Lake Store-klientobjekt (ADLStoreClient)</span><span class="sxs-lookup"><span data-stu-id="eff64-160">Step 2: Create an Azure Data Lake Store client (ADLStoreClient) object</span></span>
<span data-ttu-id="eff64-161">Skapa en [ADLStoreClient](https://azure.github.io/azure-data-lake-store-java/javadoc/) objekt kräver toospecify hello Data Lake Store-konto namn och hello tokenleverantör du genererade i hello sista steget.</span><span class="sxs-lookup"><span data-stu-id="eff64-161">Creating an [ADLStoreClient](https://azure.github.io/azure-data-lake-store-java/javadoc/) object requires you toospecify hello Data Lake Store account name and hello token provider you generated in hello last step.</span></span> <span data-ttu-id="eff64-162">Observera att hello Data Lake Store-konto namn måste toobe ett fullständigt kvalificerat domännamn.</span><span class="sxs-lookup"><span data-stu-id="eff64-162">Note that hello Data Lake Store account name needs toobe a fully qualified domain name.</span></span> <span data-ttu-id="eff64-163">Ersätt till exempel **FILL-IN-HERE** med något som **mydatalakestore.azuredatalakestore.net**.</span><span class="sxs-lookup"><span data-stu-id="eff64-163">For example, replace **FILL-IN-HERE** with something like **mydatalakestore.azuredatalakestore.net**.</span></span>

    private static String accountFQDN = "FILL-IN-HERE";  // full account FQDN, not just hello account name
    ADLStoreClient client = ADLStoreClient.createClient(accountFQDN, provider);

### <a name="step-3-use-hello-adlstoreclient-tooperform-file-and-directory-operations"></a><span data-ttu-id="eff64-164">Steg 3: Använd hello ADLStoreClient tooperform fil- och åtgärder</span><span class="sxs-lookup"><span data-stu-id="eff64-164">Step 3: Use hello ADLStoreClient tooperform file and directory operations</span></span>
<span data-ttu-id="eff64-165">hello koden nedan innehåller exempel kodavsnitt för några vanliga åtgärder.</span><span class="sxs-lookup"><span data-stu-id="eff64-165">hello code below contains example snippets of some common operations.</span></span> <span data-ttu-id="eff64-166">Du kan titta på hello fullständig [Data Lake Store Java SDK API docs](https://azure.github.io/azure-data-lake-store-java/javadoc/) av hello **ADLStoreClient** objekt toosee andra åtgärder.</span><span class="sxs-lookup"><span data-stu-id="eff64-166">You can look at hello full [Data Lake Store Java SDK API docs](https://azure.github.io/azure-data-lake-store-java/javadoc/) of hello **ADLStoreClient** object toosee other operations.</span></span>

<span data-ttu-id="eff64-167">Observera att filer läses från och skrivs till med hjälp av standard Java-strömmar.</span><span class="sxs-lookup"><span data-stu-id="eff64-167">Note that files are read from and written into using standard Java streams.</span></span> <span data-ttu-id="eff64-168">Det innebär att du layer någon hello Java dataströmmar ovanpå hello Data Lake Store-strömmar toobenefit från vanliga Java-funktioner (t.ex. Skriv ut dataströmmar för formaterade utdata eller någon av hello komprimera eller kryptera dataströmmar för ytterligare funktioner överkant, osv).</span><span class="sxs-lookup"><span data-stu-id="eff64-168">This means that you can layer any of hello Java streams on top of hello Data Lake Store streams toobenefit from standard Java functionality (e.g., Print streams for formatted output, or any of hello compression or encryption streams for additional functionality on top, etc.).</span></span>

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

#### <a name="step-4-build-and-run-hello-application"></a><span data-ttu-id="eff64-169">Steg 4: Skapa och köra programmet hello</span><span class="sxs-lookup"><span data-stu-id="eff64-169">Step 4: Build and run hello application</span></span>
1. <span data-ttu-id="eff64-170">toorun från inom ett IDE, leta upp och tryck på hello **kör** knappen.</span><span class="sxs-lookup"><span data-stu-id="eff64-170">toorun from within an IDE, locate and press hello **Run** button.</span></span> <span data-ttu-id="eff64-171">toorun från Maven, Använd [exec: exec](http://www.mojohaus.org/exec-maven-plugin/exec-mojo.html).</span><span class="sxs-lookup"><span data-stu-id="eff64-171">toorun from Maven, use [exec:exec](http://www.mojohaus.org/exec-maven-plugin/exec-mojo.html).</span></span>
2. <span data-ttu-id="eff64-172">tooproduce en fristående jar som körs från kommandoraden build hello jar med alla beroenden som ingår, med hello [Maven sammansättningen plugin](http://maven.apache.org/plugins/maven-assembly-plugin/usage.html).</span><span class="sxs-lookup"><span data-stu-id="eff64-172">tooproduce a standalone jar that you can run from command-line build hello jar with all dependencies included, using hello [Maven assembly plugin](http://maven.apache.org/plugins/maven-assembly-plugin/usage.html).</span></span> <span data-ttu-id="eff64-173">Hej pom.xml i hello [exempel källkoden på github](https://github.com/Azure-Samples/data-lake-store-java-upload-download-get-started/blob/master/pom.xml) innehåller ett exempel på hur toodo detta.</span><span class="sxs-lookup"><span data-stu-id="eff64-173">hello pom.xml in hello [example source code on github](https://github.com/Azure-Samples/data-lake-store-java-upload-download-get-started/blob/master/pom.xml) has an example of how toodo this.</span></span>

## <a name="next-steps"></a><span data-ttu-id="eff64-174">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="eff64-174">Next steps</span></span>
* [<span data-ttu-id="eff64-175">Utforska JavaDoc för hello Java SDK</span><span class="sxs-lookup"><span data-stu-id="eff64-175">Explore JavaDoc for hello Java SDK</span></span>](https://azure.github.io/azure-data-lake-store-java/javadoc/)
* [<span data-ttu-id="eff64-176">Säkra data i Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="eff64-176">Secure data in Data Lake Store</span></span>](data-lake-store-secure-data.md)
* [<span data-ttu-id="eff64-177">Använd Azure Data Lake Analytics med Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="eff64-177">Use Azure Data Lake Analytics with Data Lake Store</span></span>](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [<span data-ttu-id="eff64-178">Använd Azure HDInsight med Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="eff64-178">Use Azure HDInsight with Data Lake Store</span></span>](data-lake-store-hdinsight-hadoop-use-portal.md)

