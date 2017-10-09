---
title: "aaaCreate och använda en signatur för delad åtkomst (SAS) med Azure Blob storage | Microsoft Docs"
description: "Den här kursen visar hur toocreate signaturer för delad åtkomst för användning med Blob storage och hur tooconsume dem i ditt klientprogram."
services: storage
documentationcenter: 
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: 491e0b3c-76d4-4149-9a80-bbbd683b1f3e
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 05/15/2017
ms.author: marsma
ms.openlocfilehash: 629f5c0aee3f41115a0d514a2010d8cc0187126d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="shared-access-signatures-part-2-create-and-use-a-sas-with-blob-storage"></a><span data-ttu-id="ec683-103">Signaturer för delad åtkomst, del 2: Skapa och använda en SAS med Blob storage</span><span class="sxs-lookup"><span data-stu-id="ec683-103">Shared Access Signatures, Part 2: Create and use a SAS with Blob storage</span></span>

<span data-ttu-id="ec683-104">[Del 1](storage-dotnet-shared-access-signature-part-1.md) i den här kursen utforskade signaturer för delad åtkomst (SAS) och beskrivs bästa praxis för att använda dem.</span><span class="sxs-lookup"><span data-stu-id="ec683-104">[Part 1](storage-dotnet-shared-access-signature-part-1.md) of this tutorial explored shared access signatures (SAS) and explained best practices for using them.</span></span> <span data-ttu-id="ec683-105">Del 2 visar hur toogenerate och sedan använda delad åtkomst signaturer med Blob storage.</span><span class="sxs-lookup"><span data-stu-id="ec683-105">Part 2 shows you how toogenerate and then use shared access signatures with Blob storage.</span></span> <span data-ttu-id="ec683-106">hello exemplen är skrivna i C# och använder hello Azure Storage-klientbibliotek för .NET.</span><span class="sxs-lookup"><span data-stu-id="ec683-106">hello examples are written in C# and use hello Azure Storage Client Library for .NET.</span></span> <span data-ttu-id="ec683-107">hello exemplen i den här kursen:</span><span class="sxs-lookup"><span data-stu-id="ec683-107">hello examples in this tutorial:</span></span>

* <span data-ttu-id="ec683-108">Generera en signatur för delad åtkomst i en behållare</span><span class="sxs-lookup"><span data-stu-id="ec683-108">Generate a shared access signature on a container</span></span>
* <span data-ttu-id="ec683-109">Generera en signatur för delad åtkomst på en blob</span><span class="sxs-lookup"><span data-stu-id="ec683-109">Generate a shared access signature on a blob</span></span>
* <span data-ttu-id="ec683-110">Skapa en lagrade åtkomst princip toomanage signaturer på en behållare resurser</span><span class="sxs-lookup"><span data-stu-id="ec683-110">Create a stored access policy toomanage signatures on a container's resources</span></span>
* <span data-ttu-id="ec683-111">Testa hello delade åtkomstsignaturer i klientprogram</span><span class="sxs-lookup"><span data-stu-id="ec683-111">Test hello shared access signatures in a client application</span></span>

## <a name="about-this-tutorial"></a><span data-ttu-id="ec683-112">Om den här självstudiekursen</span><span class="sxs-lookup"><span data-stu-id="ec683-112">About this tutorial</span></span>
<span data-ttu-id="ec683-113">I den här självstudiekursen skapar vi två konsolprogram som visar hur du skapar och använder signaturer för delad åtkomst för behållare och blobbar:</span><span class="sxs-lookup"><span data-stu-id="ec683-113">In this tutorial, we create two console applications that demonstrate creating and using shared access signatures for containers and blobs:</span></span>

<span data-ttu-id="ec683-114">**Programmet 1**: hello hanteringsprogram.</span><span class="sxs-lookup"><span data-stu-id="ec683-114">**Application 1**: hello management application.</span></span> <span data-ttu-id="ec683-115">Genererar en signatur för delad åtkomst för en behållare och en blob.</span><span class="sxs-lookup"><span data-stu-id="ec683-115">Generates a shared access signature for a container and a blob.</span></span> <span data-ttu-id="ec683-116">Innehåller hello åtkomstnyckeln för lagringskontot i källkoden.</span><span class="sxs-lookup"><span data-stu-id="ec683-116">Includes hello storage account access key in source code.</span></span>

<span data-ttu-id="ec683-117">**Programmet 2**: hello klientprogrammet.</span><span class="sxs-lookup"><span data-stu-id="ec683-117">**Application 2**: hello client application.</span></span> <span data-ttu-id="ec683-118">Ansluter till behållare och blob resurser med hello delade åtkomstsignaturer skapats med hello första programmet.</span><span class="sxs-lookup"><span data-stu-id="ec683-118">Accesses container and blob resources using hello shared access signatures created with hello first application.</span></span> <span data-ttu-id="ec683-119">Använder endast hello delad åtkomst signaturer tooaccess behållare och resurser för blob - har *inte* inkluderar hello åtkomstnyckeln för lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="ec683-119">Uses only hello shared access signatures tooaccess container and blob resources--it does *not* include hello storage account access key.</span></span>

## <a name="part-1-create-a-console-application-toogenerate-shared-access-signatures"></a><span data-ttu-id="ec683-120">Del 1: Skapa en delad toogenerate för konsolen programåtkomst signaturer</span><span class="sxs-lookup"><span data-stu-id="ec683-120">Part 1: Create a console application toogenerate shared access signatures</span></span>
<span data-ttu-id="ec683-121">Kontrollera först att du har hello Azure Storage-klientbibliotek för .NET installerats.</span><span class="sxs-lookup"><span data-stu-id="ec683-121">First, ensure that you have hello Azure Storage Client Library for .NET installed.</span></span> <span data-ttu-id="ec683-122">Du kan installera hello [NuGet-paketet](http://nuget.org/packages/WindowsAzure.Storage/ "NuGet-paketet") som innehåller hello senaste sammansättningar för hello klientbiblioteket.</span><span class="sxs-lookup"><span data-stu-id="ec683-122">You can install hello [NuGet package](http://nuget.org/packages/WindowsAzure.Storage/ "NuGet package") containing hello most up-to-date assemblies for hello client library.</span></span> <span data-ttu-id="ec683-123">Detta är hello rekommenderad metod för att säkerställa att du har hello senaste korrigeringar.</span><span class="sxs-lookup"><span data-stu-id="ec683-123">This is hello recommended method for ensuring that you have hello most recent fixes.</span></span> <span data-ttu-id="ec683-124">Du kan också hämta hello klientbiblioteket som en del av hello senaste versionen av hello [Azure SDK för .NET](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="ec683-124">You can also download hello client library as part of hello most recent version of hello [Azure SDK for .NET](https://azure.microsoft.com/downloads/).</span></span>

<span data-ttu-id="ec683-125">Skapa ett nytt Windows-konsolprogram i Visual Studio och ger den namnet **GenerateSharedAccessSignatures**.</span><span class="sxs-lookup"><span data-stu-id="ec683-125">In Visual Studio, create a new Windows console application and name it **GenerateSharedAccessSignatures**.</span></span> <span data-ttu-id="ec683-126">Lägg till referenser för[Microsoft.WindowsAzure.ConfigurationManager](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager) och [WindowsAzure.Storage](https://www.nuget.org/packages/WindowsAzure.Storage/) med någon av följande metoder hello:</span><span class="sxs-lookup"><span data-stu-id="ec683-126">Add references too[Microsoft.WindowsAzure.ConfigurationManager](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager) and [WindowsAzure.Storage](https://www.nuget.org/packages/WindowsAzure.Storage/) by using one of hello following approaches:</span></span>

* <span data-ttu-id="ec683-127">Använd hello [NuGet-Pakethanteraren](https://docs.nuget.org/consume/installing-nuget) i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ec683-127">Use hello [NuGet package manager](https://docs.nuget.org/consume/installing-nuget) in Visual Studio.</span></span> <span data-ttu-id="ec683-128">Välj **projekt** > **hantera NuGet-paket**, Sök online efter varje paket (Microsoft.WindowsAzure.ConfigurationManager och WindowsAzure.Storage) och installera dem.</span><span class="sxs-lookup"><span data-stu-id="ec683-128">Select **Project** > **Manage NuGet Packages**, search online for each package (Microsoft.WindowsAzure.ConfigurationManager and WindowsAzure.Storage) and install them.</span></span>
* <span data-ttu-id="ec683-129">Du kan också söka efter dessa sammansättningar i installationen av hello Azure SDK och Lägg till referenser toothem:</span><span class="sxs-lookup"><span data-stu-id="ec683-129">Alternatively, locate these assemblies in your installation of hello Azure SDK and add references toothem:</span></span>
  * <span data-ttu-id="ec683-130">Microsoft.WindowsAzure.Configuration.dll</span><span class="sxs-lookup"><span data-stu-id="ec683-130">Microsoft.WindowsAzure.Configuration.dll</span></span>
  * <span data-ttu-id="ec683-131">Microsoft.WindowsAzure.Storage.dll</span><span class="sxs-lookup"><span data-stu-id="ec683-131">Microsoft.WindowsAzure.Storage.dll</span></span>

<span data-ttu-id="ec683-132">Lägg till följande hello överst hello i hello Program.cs-filen, **med** direktiven:</span><span class="sxs-lookup"><span data-stu-id="ec683-132">At hello top of hello Program.cs file, add hello following **using** directives:</span></span>

```csharp
using System.IO;
using Microsoft.Azure;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Blob;
```

<span data-ttu-id="ec683-133">Redigera hello app.config-filen så att den innehåller en konfigurationsinställning med en anslutningssträng som pekar tooyour storage-konto.</span><span class="sxs-lookup"><span data-stu-id="ec683-133">Edit hello app.config file so that it contains a configuration setting with a connection string that points tooyour storage account.</span></span> <span data-ttu-id="ec683-134">Filen app.config bör se ut ungefär toothis en:</span><span class="sxs-lookup"><span data-stu-id="ec683-134">Your app.config file should look similar toothis one:</span></span>

```xml
<configuration>
  <startup>
    <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5.2" />
  </startup>
  <appSettings>
    <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=mykey"/>
  </appSettings>
</configuration>
```

### <a name="generate-a-shared-access-signature-uri-for-a-container"></a><span data-ttu-id="ec683-135">Generera en signatur för delad åtkomst URI för en behållare</span><span class="sxs-lookup"><span data-stu-id="ec683-135">Generate a shared access signature URI for a container</span></span>
<span data-ttu-id="ec683-136">toobegin med vi lägga till en metod toogenerate en signatur för delad åtkomst på en ny behållare.</span><span class="sxs-lookup"><span data-stu-id="ec683-136">toobegin with, we add a method toogenerate a shared access signature on a new container.</span></span> <span data-ttu-id="ec683-137">I det här fallet hello signaturen är inte associerad med en lagrad åtkomstprincip, så den bedriver hello URI hello information som anger dess förfallodatum tid och hello behörigheter beviljas.</span><span class="sxs-lookup"><span data-stu-id="ec683-137">In this case, hello signature is not associated with a stored access policy, so it carries on hello URI hello information indicating its expiry time and hello permissions it grants.</span></span>

<span data-ttu-id="ec683-138">Lägg först till kod toohello **Main()** metoden tooauthenticate åtkomst tooyour lagringskontot och skapa en ny behållare:</span><span class="sxs-lookup"><span data-stu-id="ec683-138">First, add code toohello **Main()** method tooauthenticate access tooyour storage account and create a new container:</span></span>

```csharp
static void Main(string[] args)
{
    //Parse hello connection string and return a reference toohello storage account.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(CloudConfigurationManager.GetSetting("StorageConnectionString"));

    //Create hello blob client object.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    //Get a reference tooa container toouse for hello sample code, and create it if it does not exist.
    CloudBlobContainer container = blobClient.GetContainerReference("sascontainer");
    container.CreateIfNotExists();

    //Insert calls toohello methods created below here...

    //Require user input before closing hello console window.
    Console.ReadLine();
}
```

<span data-ttu-id="ec683-139">Lägg till en metod som genererar hello signatur för delad åtkomst för hello behållare och returnerar hello signatur URI:</span><span class="sxs-lookup"><span data-stu-id="ec683-139">Next, add a method that generates hello shared access signature for hello container and returns hello signature URI:</span></span>

```csharp
static string GetContainerSasUri(CloudBlobContainer container)
{
    //Set hello expiry time and permissions for hello container.
    //In this case no start time is specified, so hello shared access signature becomes valid immediately.
    SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy();
    sasConstraints.SharedAccessExpiryTime = DateTimeOffset.UtcNow.AddHours(24);
    sasConstraints.Permissions = SharedAccessBlobPermissions.List | SharedAccessBlobPermissions.Write;

    //Generate hello shared access signature on hello container, setting hello constraints directly on hello signature.
    string sasContainerToken = container.GetSharedAccessSignature(sasConstraints);

    //Return hello URI string for hello container, including hello SAS token.
    return container.Uri + sasContainerToken;
}
```

<span data-ttu-id="ec683-140">Lägg till följande rader längst ned hello hello hello **Main()** metoden innan hello anropa för**Console.ReadLine()**, toocall **GetContainerSasUri()** och skriva hello signaturen URI toohello konsolfönstret:</span><span class="sxs-lookup"><span data-stu-id="ec683-140">Add hello following lines at hello bottom of hello **Main()** method, before hello call too**Console.ReadLine()**, toocall **GetContainerSasUri()** and write hello signature URI toohello console window:</span></span>

```csharp
//Generate a SAS URI for hello container, without a stored access policy.
Console.WriteLine("Container SAS URI: " + GetContainerSasUri(container));
Console.WriteLine();
```

<span data-ttu-id="ec683-141">Kompilera och köra toooutput hello signatur för delad åtkomst URI för hello ny behållare.</span><span class="sxs-lookup"><span data-stu-id="ec683-141">Compile and run toooutput hello shared access signature URI for hello new container.</span></span> <span data-ttu-id="ec683-142">hello URI är liknande toohello följande:</span><span class="sxs-lookup"><span data-stu-id="ec683-142">hello URI will be similar toohello following:</span></span>

```
https://storageaccount.blob.core.windows.net/sascontainer?sv=2012-02-12&se=2013-04-13T00%3A12%3A08Z&sr=c&sp=wl&sig=t%2BbzU9%2B7ry4okULN9S0wst%2F8MCUhTjrHyV9rDNLSe8g%3D
```

<span data-ttu-id="ec683-143">När du har kört hello kod är signatur för delad åtkomst hello du skapade för hello-behållaren giltiga för hello nästkommande 24 timmar.</span><span class="sxs-lookup"><span data-stu-id="ec683-143">Once you have run hello code, hello shared access signature you created for hello container will be valid for hello next 24 hours.</span></span> <span data-ttu-id="ec683-144">hello signatur ger en klient behörighet toolist blobbar i behållaren hello och toowrite ny BLOB toohello behållare.</span><span class="sxs-lookup"><span data-stu-id="ec683-144">hello signature grants a client permission toolist blobs in hello container and toowrite new blobs toohello container.</span></span>

### <a name="generate-a-shared-access-signature-uri-for-a-blob"></a><span data-ttu-id="ec683-145">Generera en signatur för delad åtkomst URI för en blob</span><span class="sxs-lookup"><span data-stu-id="ec683-145">Generate a shared access signature URI for a blob</span></span>
<span data-ttu-id="ec683-146">Sedan vi skriva liknande kod toocreate en ny blob i hello-behållaren och generera en signatur för delad åtkomst för den.</span><span class="sxs-lookup"><span data-stu-id="ec683-146">Next, we write similar code toocreate a new blob within hello container and generate a shared access signature for it.</span></span> <span data-ttu-id="ec683-147">Signatur för delad åtkomst är inte associerad med en princip för lagrade åtkomst så att den omfattar hello starttid och förfallotiden behörighetsinformation i hello URI.</span><span class="sxs-lookup"><span data-stu-id="ec683-147">This shared access signature is not associated with a stored access policy, so it includes hello start time, expiry time, and permission information in hello URI.</span></span>

<span data-ttu-id="ec683-148">Lägg till en ny metod som skapar en ny blob och skriver vissa text tooit och sedan genererar en signatur för delad åtkomst och returnerar hello signatur URI:</span><span class="sxs-lookup"><span data-stu-id="ec683-148">Add a new method that creates a new blob and writes some text tooit, then generates a shared access signature and returns hello signature URI:</span></span>

```csharp
static string GetBlobSasUri(CloudBlobContainer container)
{
    //Get a reference tooa blob within hello container.
    CloudBlockBlob blob = container.GetBlockBlobReference("sasblob.txt");

    //Upload text toohello blob. If hello blob does not yet exist, it will be created.
    //If hello blob does exist, its existing content will be overwritten.
    string blobContent = "This blob will be accessible tooclients via a shared access signature (SAS).";
    blob.UploadText(blobContent);

    //Set hello expiry time and permissions for hello blob.
    //In this case, hello start time is specified as a few minutes in hello past, toomitigate clock skew.
    //hello shared access signature will be valid immediately.
    SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy();
    sasConstraints.SharedAccessStartTime = DateTimeOffset.UtcNow.AddMinutes(-5);
    sasConstraints.SharedAccessExpiryTime = DateTimeOffset.UtcNow.AddHours(24);
    sasConstraints.Permissions = SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.Write;

    //Generate hello shared access signature on hello blob, setting hello constraints directly on hello signature.
    string sasBlobToken = blob.GetSharedAccessSignature(sasConstraints);

    //Return hello URI string for hello container, including hello SAS token.
    return blob.Uri + sasBlobToken;
}
```

<span data-ttu-id="ec683-149">Längst ned hello hello **Main()** metod, Lägg till följande rader toocall hello **GetBlobSasUri()**innan hello anropa för**Console.ReadLine()**, och skriva hello delade åtkomst till konsolfönstret signatur URI toohello:</span><span class="sxs-lookup"><span data-stu-id="ec683-149">At hello bottom of hello **Main()** method, add hello following lines toocall **GetBlobSasUri()**, before hello call too**Console.ReadLine()**, and write hello shared access signature URI toohello console window:</span></span>

```csharp
//Generate a SAS URI for a blob within hello container, without a stored access policy.
Console.WriteLine("Blob SAS URI: " + GetBlobSasUri(container));
Console.WriteLine();
```

<span data-ttu-id="ec683-150">Kompilera och köra toooutput hello signatur för delad åtkomst URI för nya hello-blob.</span><span class="sxs-lookup"><span data-stu-id="ec683-150">Compile and run toooutput hello shared access signature URI for hello new blob.</span></span> <span data-ttu-id="ec683-151">hello URI är liknande toohello följande:</span><span class="sxs-lookup"><span data-stu-id="ec683-151">hello URI will be similar toohello following:</span></span>

```
https://storageaccount.blob.core.windows.net/sascontainer/sasblob.txt?sv=2012-02-12&st=2013-04-12T23%3A37%3A08Z&se=2013-04-13T00%3A12%3A08Z&sr=b&sp=rw&sig=dF2064yHtc8RusQLvkQFPItYdeOz3zR8zHsDMBi4S30%3D
```

### <a name="create-a-stored-access-policy-on-hello-container"></a><span data-ttu-id="ec683-152">Skapa en princip för lagrade åtkomst på hello behållare</span><span class="sxs-lookup"><span data-stu-id="ec683-152">Create a stored access policy on hello container</span></span>
<span data-ttu-id="ec683-153">Nu skapar vi en åtkomstprincip som är lagrade på hello behållare, som definierar hello begränsningar för alla signaturer för delad åtkomst som är associerade med den.</span><span class="sxs-lookup"><span data-stu-id="ec683-153">Now let's create a stored access policy on hello container, which will define hello constraints for any shared access signatures that are associated with it.</span></span>

<span data-ttu-id="ec683-154">I hello föregående exempel kan vi starttid som anges hello (implicit eller explicit), hello förfallotiden och hello behörigheter på hello delad åtkomstsignatur URI sig själv.</span><span class="sxs-lookup"><span data-stu-id="ec683-154">In hello previous examples, we specified hello start time (implicitly or explicitly), hello expiry time, and hello permissions on hello shared access signature URI itself.</span></span> <span data-ttu-id="ec683-155">I följande exempel hello, ange vi dessa hello lagras åtkomstprincip, inte hello signatur för delad åtkomst.</span><span class="sxs-lookup"><span data-stu-id="ec683-155">In hello following examples, we specify these on hello stored access policy, not on hello shared access signature.</span></span> <span data-ttu-id="ec683-156">Gör så gör oss toochange dessa villkor utan att återutfärda hello delad åtkomst till signatur.</span><span class="sxs-lookup"><span data-stu-id="ec683-156">Doing so enables us toochange these constraints without reissuing hello shared access signature.</span></span>

<span data-ttu-id="ec683-157">Det är möjligt toohave en eller flera hello begränsningar på hello signatur för delad åtkomst och hello resten på hello lagras åtkomstprincip.</span><span class="sxs-lookup"><span data-stu-id="ec683-157">It's possible toohave one or more of hello constraints on hello shared access signature, and hello remainder on hello stored access policy.</span></span> <span data-ttu-id="ec683-158">Du kan dock endast ange hello starttid, förfallotiden och behörigheter i en plats eller hello andra.</span><span class="sxs-lookup"><span data-stu-id="ec683-158">However, you can only specify hello start time, expiry time, and permissions in one place or hello other.</span></span> <span data-ttu-id="ec683-159">Du kan inte ange behörigheter på hello signatur för delad åtkomst och även ange dem på hello lagras åtkomstprincip.</span><span class="sxs-lookup"><span data-stu-id="ec683-159">For example, you can't specify permissions on hello shared access signature and also specify them on hello stored access policy.</span></span>

<span data-ttu-id="ec683-160">När du lägger till en behållare för lagrade åtkomst princip tooa måste hämta hello behållaren befintliga behörigheter, lägga till hello nya åtkomstprincip och sedan ange hello behållaren behörigheter.</span><span class="sxs-lookup"><span data-stu-id="ec683-160">When you add a stored access policy tooa container, you must get hello container's existing permissions, add hello new access policy, and then set hello container's permissions.</span></span>

<span data-ttu-id="ec683-161">Lägg till en ny metod som skapar en ny princip för lagrade åtkomst på en behållare och returnerar hello namnet på hello principen:</span><span class="sxs-lookup"><span data-stu-id="ec683-161">Add a new method that creates a new stored access policy on a container and returns hello name of hello policy:</span></span>

```csharp
static void CreateSharedAccessPolicy(CloudBlobClient blobClient, CloudBlobContainer container,
    string policyName)
{
    //Get hello container's existing permissions.
    BlobContainerPermissions permissions = container.GetPermissions();

    //Create a new shared access policy and define its constraints.
    SharedAccessBlobPolicy sharedPolicy = new SharedAccessBlobPolicy()
    {
        SharedAccessExpiryTime = DateTimeOffset.UtcNow.AddHours(24),
        Permissions = SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.List | SharedAccessBlobPermissions.Read
    };

    //Add hello new policy toohello container's permissions, and set hello container's permissions.
    permissions.SharedAccessPolicies.Add(policyName, sharedPolicy);
    container.SetPermissions(permissions);
}
```

<span data-ttu-id="ec683-162">Längst ned hello hello **Main()** metoden innan hello anropa för**Console.ReadLine()**, lägga till hello följande rader toofirst Radera alla befintliga principer för åtkomst och sedan anropa hello  **CreateSharedAccessPolicy()** metoden:</span><span class="sxs-lookup"><span data-stu-id="ec683-162">At hello bottom of hello **Main()** method, before hello call too**Console.ReadLine()**, add hello following lines toofirst clear any existing access policies, and then call hello **CreateSharedAccessPolicy()** method:</span></span>

```csharp
//Clear any existing access policies on container.
BlobContainerPermissions perms = container.GetPermissions();
perms.SharedAccessPolicies.Clear();
container.SetPermissions(perms);

//Create a new access policy on hello container, which may be optionally used tooprovide constraints for
//shared access signatures on hello container and hello blob.
string sharedAccessPolicyName = "tutorialpolicy";
CreateSharedAccessPolicy(blobClient, container, sharedAccessPolicyName);
```

<span data-ttu-id="ec683-163">När du avmarkerar hello åtkomstprinciper i en behållare, måste du först hämta hello behållaren befintliga behörigheter och sedan avmarkera hello behörigheter och sedan ange hello behörigheter igen.</span><span class="sxs-lookup"><span data-stu-id="ec683-163">When you clear hello access policies on a container, you must first get hello container's existing permissions, then clear hello permissions, then set hello permissions again.</span></span>

### <a name="generate-a-shared-access-signature-uri-on-hello-container-that-uses-an-access-policy"></a><span data-ttu-id="ec683-164">Generera en signatur för delad åtkomst URI hello-behållaren som använder en åtkomstprincip</span><span class="sxs-lookup"><span data-stu-id="ec683-164">Generate a shared access signature URI on hello container that uses an access policy</span></span>
<span data-ttu-id="ec683-165">Därefter skapar vi en annan signatur för delad åtkomst för hello-behållare som vi skapade tidigare men nu vi hello signaturen till hello lagras åtkomstprincip som vi skapade i föregående exempel för hello.</span><span class="sxs-lookup"><span data-stu-id="ec683-165">Next, we create another shared access signature for hello container that we created earlier, but this time we associate hello signature with hello stored access policy we created in hello previous example.</span></span>

<span data-ttu-id="ec683-166">Lägg till en ny metod toogenerate en annan signatur för delad åtkomst för hello behållare:</span><span class="sxs-lookup"><span data-stu-id="ec683-166">Add a new method toogenerate another shared access signature for hello container:</span></span>

```csharp
static string GetContainerSasUriWithPolicy(CloudBlobContainer container, string policyName)
{
    //Generate hello shared access signature on hello container. In this case, all of hello constraints for the
    //shared access signature are specified on hello stored access policy.
    string sasContainerToken = container.GetSharedAccessSignature(null, policyName);

    //Return hello URI string for hello container, including hello SAS token.
    return container.Uri + sasContainerToken;
}
```

<span data-ttu-id="ec683-167">Längst ned hello hello **Main()** metoden innan hello anropa för**Console.ReadLine()**, Lägg till följande rader toocall hello hello **GetContainerSasUriWithPolicy** metod :</span><span class="sxs-lookup"><span data-stu-id="ec683-167">At hello bottom of hello **Main()** method, before hello call too**Console.ReadLine()**, add hello following lines toocall hello **GetContainerSasUriWithPolicy** method:</span></span>

```csharp
//Generate a SAS URI for hello container, using a stored access policy tooset constraints on hello SAS.
Console.WriteLine("Container SAS URI using stored access policy: " + GetContainerSasUriWithPolicy(container, sharedAccessPolicyName));
Console.WriteLine();
```

### <a name="generate-a-shared-access-signature-uri-on-hello-blob-that-uses-an-access-policy"></a><span data-ttu-id="ec683-168">Generera en delad åtkomst signatur URI på hello Blob som använder en åtkomstprincip</span><span class="sxs-lookup"><span data-stu-id="ec683-168">Generate a Shared Access Signature URI on hello Blob That Uses an Access Policy</span></span>
<span data-ttu-id="ec683-169">Slutligen vi lägga till en liknande metoden toocreate en annan blob och generera en signatur för delad åtkomst som är kopplad till en princip för lagrade åtkomst.</span><span class="sxs-lookup"><span data-stu-id="ec683-169">Finally, we add a similar method toocreate another blob and generate a shared access signature that's associated with a stored access policy.</span></span>

<span data-ttu-id="ec683-170">Lägg till en ny metod toocreate en blob och generera en signatur för delad åtkomst:</span><span class="sxs-lookup"><span data-stu-id="ec683-170">Add a new method toocreate a blob and generate a shared access signature:</span></span>

```csharp
static string GetBlobSasUriWithPolicy(CloudBlobContainer container, string policyName)
{
    //Get a reference tooa blob within hello container.
    CloudBlockBlob blob = container.GetBlockBlobReference("sasblobpolicy.txt");

    //Upload text toohello blob. If hello blob does not yet exist, it will be created.
    //If hello blob does exist, its existing content will be overwritten.
    string blobContent = "This blob will be accessible tooclients via a shared access signature. " +
    "A stored access policy defines hello constraints for hello signature.";
    MemoryStream ms = new MemoryStream(Encoding.UTF8.GetBytes(blobContent));
    ms.Position = 0;
    using (ms)
    {
        blob.UploadFromStream(ms);
    }

    //Generate hello shared access signature on hello blob.
    string sasBlobToken = blob.GetSharedAccessSignature(null, policyName);

    //Return hello URI string for hello container, including hello SAS token.
    return blob.Uri + sasBlobToken;
}
```

<span data-ttu-id="ec683-171">Längst ned hello hello **Main()** metoden innan hello anropa för**Console.ReadLine()**, Lägg till följande rader toocall hello hello **GetBlobSasUriWithPolicy** metod:</span><span class="sxs-lookup"><span data-stu-id="ec683-171">At hello bottom of hello **Main()** method, before hello call too**Console.ReadLine()**, add hello following lines toocall hello **GetBlobSasUriWithPolicy** method:</span></span>

```csharp
//Generate a SAS URI for a blob within hello container, using a stored access policy tooset constraints on hello SAS.
Console.WriteLine("Blob SAS URI using stored access policy: " + GetBlobSasUriWithPolicy(container, sharedAccessPolicyName));
Console.WriteLine();
```

<span data-ttu-id="ec683-172">Hej **Main()** metod bör nu se ut så här i sin helhet.</span><span class="sxs-lookup"><span data-stu-id="ec683-172">hello **Main()** method should now look like this in its entirety.</span></span> <span data-ttu-id="ec683-173">Kör den signatur för delad åtkomst toowrite hello URI: er toohello konsolfönstret, och sedan kopiera och klistra in dem i en textfil för användning i hello andra delen av den här kursen.</span><span class="sxs-lookup"><span data-stu-id="ec683-173">Run it toowrite hello shared access signature URIs toohello console window, then copy and paste them into a text file for use in hello second part of this tutorial.</span></span>

```csharp
static void Main(string[] args)
{
    //Parse hello connection string and return a reference toohello storage account.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(CloudConfigurationManager.GetSetting("StorageConnectionString"));

    //Create hello blob client object.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    //Get a reference tooa container toouse for hello sample code, and create it if it does not exist.
    CloudBlobContainer container = blobClient.GetContainerReference("sascontainer");
    container.CreateIfNotExists();

    //Generate a SAS URI for hello container, without a stored access policy.
    Console.WriteLine("Container SAS URI: " + GetContainerSasUri(container));
    Console.WriteLine();

    //Generate a SAS URI for a blob within hello container, without a stored access policy.
    Console.WriteLine("Blob SAS URI: " + GetBlobSasUri(container));
    Console.WriteLine();

    //Clear any existing access policies on container.
    BlobContainerPermissions perms = container.GetPermissions();
    perms.SharedAccessPolicies.Clear();
    container.SetPermissions(perms);

    //Create a new access policy on hello container, which may be optionally used tooprovide constraints for
    //shared access signatures on hello container and hello blob.
    string sharedAccessPolicyName = "tutorialpolicy";
    CreateSharedAccessPolicy(blobClient, container, sharedAccessPolicyName);

    //Generate a SAS URI for hello container, using a stored access policy tooset constraints on hello SAS.
    Console.WriteLine("Container SAS URI using stored access policy: " + GetContainerSasUriWithPolicy(container, sharedAccessPolicyName));
    Console.WriteLine();

    //Generate a SAS URI for a blob within hello container, using a stored access policy tooset constraints on hello SAS.
    Console.WriteLine("Blob SAS URI using stored access policy: " + GetBlobSasUriWithPolicy(container, sharedAccessPolicyName));
    Console.WriteLine();

    Console.ReadLine();
}
```

<span data-ttu-id="ec683-174">När du kör hello GenerateSharedAccessSignatures konsolprogram visas utdata liknande toohello följande.</span><span class="sxs-lookup"><span data-stu-id="ec683-174">When you run hello GenerateSharedAccessSignatures console application, you'll see output similar toohello following.</span></span> <span data-ttu-id="ec683-175">Dessa är hello delade åtkomstsignaturer du använder i en del 2 av hello självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="ec683-175">These are hello shared access signatures you use in Part 2 of hello tutorial.</span></span>

```
Container SAS URI: https://storagesample.blob.core.windows.net/sascontainer?sv=2016-05-31&sr=c&sig=pFlEZD%2F6sJTNLxD%2FQ26Hh85j%2FzYPxZav6mP1KJwnvJE%3D&se=2017-05-16T16%3A16%3A47Z&sp=wl

Blob SAS URI: https://storagesample.blob.core.windows.net/sascontainer/sasblob.txt?sv=2016-05-31&sr=b&sig=%2FiBWAZbXESzCMvRcm7JwJBK0gT0BtPSWEq4pRwmlBRI%3D&st=2017-05-15T16%3A11%3A48Z&se=2017-05-16T16%3A16%3A48Z&sp=rw

Container SAS URI using stored access policy: https://storagesample.blob.core.windows.net/sascontainer?sv=2016-05-31&sr=c&si=tutorialpolicy&sig=aMb6rKDvvpfiGVsZI2rCmyUra6ZPpq%2BZ%2FLyTgAeec%2Bk%3D

Blob SAS URI using stored access policy: https://storagesample.blob.core.windows.net/sascontainer/sasblobpolicy.txt?sv=2016-05-31&sr=b&si=tutorialpolicy&sig=%2FkTWkT23SS45%2FoF4bK2mqXkN%2BPKs%2FyHuzkfQ4GFoZVU%3D
```

## <a name="part-2-create-a-console-application-tootest-hello-shared-access-signatures"></a><span data-ttu-id="ec683-176">Del 2: Skapa ett program tootest hello delade konsolåtkomst signaturer</span><span class="sxs-lookup"><span data-stu-id="ec683-176">Part 2: Create a console application tootest hello shared access signatures</span></span>
<span data-ttu-id="ec683-177">tootest hello signaturer för delad åtkomst skapade i föregående exempel för hello, skapar vi ett andra konsolprogram som använder hello signaturer tooperform åtgärder på hello behållare och en blob.</span><span class="sxs-lookup"><span data-stu-id="ec683-177">tootest hello shared access signatures created in hello previous examples, we create a second console application that uses hello signatures tooperform operations on hello container and on a blob.</span></span>

> [!NOTE]
> <span data-ttu-id="ec683-178">Om mer än 24 timmar har gått sedan du slutfört hello första delen av kursen hello, kommer hello signaturer som du genererade inte längre giltig.</span><span class="sxs-lookup"><span data-stu-id="ec683-178">If more than 24 hours have passed since you completed hello first part of hello tutorial, hello signatures you generated will no longer be valid.</span></span> <span data-ttu-id="ec683-179">I det här fallet bör du köra hello koden i hello första konsolen programmet toogenerate signaturer för ny delad åtkomst för användning i hello andra delen av kursen hello.</span><span class="sxs-lookup"><span data-stu-id="ec683-179">In this case, you should run hello code in hello first console application toogenerate fresh shared access signatures for use in hello second part of hello tutorial.</span></span>
>

<span data-ttu-id="ec683-180">Skapa ett nytt Windows-konsolprogram i Visual Studio och ger den namnet **ConsumeSharedAccessSignatures**.</span><span class="sxs-lookup"><span data-stu-id="ec683-180">In Visual Studio, create a new Windows console application and name it **ConsumeSharedAccessSignatures**.</span></span> <span data-ttu-id="ec683-181">Lägg till referenser för[Microsoft.WindowsAzure.ConfigurationManager](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager) och [WindowsAzure.Storage](https://www.nuget.org/packages/WindowsAzure.Storage/), som du gjorde tidigare.</span><span class="sxs-lookup"><span data-stu-id="ec683-181">Add references too[Microsoft.WindowsAzure.ConfigurationManager](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager) and [WindowsAzure.Storage](https://www.nuget.org/packages/WindowsAzure.Storage/), as you did previously.</span></span>

<span data-ttu-id="ec683-182">Lägg till följande hello överst hello i hello Program.cs-filen, **med** direktiven:</span><span class="sxs-lookup"><span data-stu-id="ec683-182">At hello top of hello Program.cs file, add hello following **using** directives:</span></span>

```csharp
using System.IO;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Blob;
```

<span data-ttu-id="ec683-183">I hello brödtext hello **Main()** metod, lägga till hello följande strängkonstanter, ändra deras värden toohello delade åtkomstsignaturer du genererade i del 1 av hello kursen.</span><span class="sxs-lookup"><span data-stu-id="ec683-183">In hello body of hello **Main()** method, add hello following string constants, changing their values toohello shared access signatures you generated in part 1 of hello tutorial.</span></span>

```csharp
static void Main(string[] args)
{
    const string containerSAS = "<your container SAS>";
    const string blobSAS = "<your blob SAS>";
    const string containerSASWithAccessPolicy = "<your container SAS with access policy>";
    const string blobSASWithAccessPolicy = "<your blob SAS with access policy>";
}
```

### <a name="add-a-method-tootry-container-operations-using-a-shared-access-signature"></a><span data-ttu-id="ec683-184">Lägg till en metod tootry behållare åtgärder med hjälp av en signatur för delad åtkomst</span><span class="sxs-lookup"><span data-stu-id="ec683-184">Add a method tootry container operations using a shared access signature</span></span>
<span data-ttu-id="ec683-185">Nu ska vi lägga till en metod som testar vissa åtgärder för behållaren med hjälp av en signatur för delad åtkomst för hello behållare.</span><span class="sxs-lookup"><span data-stu-id="ec683-185">Next, we add a method that tests some container operations using a shared access signature for hello container.</span></span> <span data-ttu-id="ec683-186">hello signatur för delad åtkomst är används tooreturn en referens toohello behållare, autentisera åtkomst toohello behållare baserat på hello signatur enbart.</span><span class="sxs-lookup"><span data-stu-id="ec683-186">hello shared access signature is used tooreturn a reference toohello container, authenticating access toohello container based on hello signature alone.</span></span>

<span data-ttu-id="ec683-187">Lägg till följande metod tooProgram.cs hello:</span><span class="sxs-lookup"><span data-stu-id="ec683-187">Add hello following method tooProgram.cs:</span></span>

```csharp
static void UseContainerSAS(string sas)
{
    //Try performing container operations with hello SAS provided.

    //Return a reference toohello container using hello SAS URI.
    CloudBlobContainer container = new CloudBlobContainer(new Uri(sas));

    //Create a list toostore blob URIs returned by a listing operation on hello container.
    List<ICloudBlob> blobList = new List<ICloudBlob>();

    //Write operation: write a new blob toohello container.
    try
    {
        CloudBlockBlob blob = container.GetBlockBlobReference("blobCreatedViaSAS.txt");
        string blobContent = "This blob was created with a shared access signature granting write permissions toohello container. ";
        blob.UploadText(blobContent);

        Console.WriteLine("Write operation succeeded for SAS " + sas);
        Console.WriteLine();
    }
    catch (StorageException e)
    {
        Console.WriteLine("Write operation failed for SAS " + sas);
        Console.WriteLine("Additional error information: " + e.Message);
        Console.WriteLine();
    }

    //List operation: List hello blobs in hello container.
    try
    {
        foreach (ICloudBlob blob in container.ListBlobs())
        {
            blobList.Add(blob);
        }
        Console.WriteLine("List operation succeeded for SAS " + sas);
        Console.WriteLine();
    }
    catch (StorageException e)
    {
        Console.WriteLine("List operation failed for SAS " + sas);
        Console.WriteLine("Additional error information: " + e.Message);
        Console.WriteLine();
    }

    //Read operation: Get a reference tooone of hello blobs in hello container and read it.
    try
    {
        CloudBlockBlob blob = container.GetBlockBlobReference(blobList[0].Name);
        MemoryStream msRead = new MemoryStream();
        msRead.Position = 0;
        using (msRead)
        {
            blob.DownloadToStream(msRead);
            Console.WriteLine(msRead.Length);
        }
        Console.WriteLine("Read operation succeeded for SAS " + sas);
        Console.WriteLine();
    }
    catch (StorageException e)
    {
        Console.WriteLine("Read operation failed for SAS " + sas);
        Console.WriteLine("Additional error information: " + e.Message);
        Console.WriteLine();
    }
    Console.WriteLine();

    //Delete operation: Delete a blob in hello container.
    try
    {
        CloudBlockBlob blob = container.GetBlockBlobReference(blobList[0].Name);
        blob.Delete();
        Console.WriteLine("Delete operation succeeded for SAS " + sas);
        Console.WriteLine();
    }
    catch (StorageException e)
    {
        Console.WriteLine("Delete operation failed for SAS " + sas);
        Console.WriteLine("Additional error information: " + e.Message);
        Console.WriteLine();
    }
}
```

<span data-ttu-id="ec683-188">Uppdatera hello **Main()** metoden toocall **UseContainerSAS()** med både hello signaturer för delad åtkomst du skapade på hello behållare:</span><span class="sxs-lookup"><span data-stu-id="ec683-188">Update hello **Main()** method toocall **UseContainerSAS()** with both of hello shared access signatures you created on hello container:</span></span>

```csharp
static void Main(string[] args)
{
    string containerSAS = "<your container SAS>";
    string blobSAS = "<your blob SAS>";
    string containerSASWithAccessPolicy = "<your container SAS with access policy>";
    string blobSASWithAccessPolicy = "<your blob SAS with access policy>";

    //Call hello test methods with hello shared access signatures created on hello container, with and without hello access policy.
    UseContainerSAS(containerSAS);
    UseContainerSAS(containerSASWithAccessPolicy);

    Console.ReadLine();
}
```

### <a name="add-a-method-tootry-blob-operations-using-a-shared-access-signature"></a><span data-ttu-id="ec683-189">Lägg till en metod tootry blob-åtgärder med hjälp av en signatur för delad åtkomst</span><span class="sxs-lookup"><span data-stu-id="ec683-189">Add a method tootry blob operations using a shared access signature</span></span>
<span data-ttu-id="ec683-190">Slutligen vi lägga till en metod som testar vissa blob-åtgärder med hjälp av en signatur för delad åtkomst på hello-blob.</span><span class="sxs-lookup"><span data-stu-id="ec683-190">Finally, we add a method that tests some blob operations using a shared access signature on hello blob.</span></span> <span data-ttu-id="ec683-191">I detta fall kan vi använda hello konstruktorn **CloudBlockBlob(String)**, och skickar hello-signatur för delad åtkomst, tooreturn en referens toohello blob.</span><span class="sxs-lookup"><span data-stu-id="ec683-191">In this case, we use hello constructor **CloudBlockBlob(String)**, passing in hello shared access signature, tooreturn a reference toohello blob.</span></span> <span data-ttu-id="ec683-192">Inga andra autentisering krävs. den är baserad på hello signatur enbart.</span><span class="sxs-lookup"><span data-stu-id="ec683-192">No other authentication is required; it's based on hello signature alone.</span></span>

<span data-ttu-id="ec683-193">Lägg till följande metod tooProgram.cs hello:</span><span class="sxs-lookup"><span data-stu-id="ec683-193">Add hello following method tooProgram.cs:</span></span>

```csharp
static void UseBlobSAS(string sas)
{
    //Try performing blob operations using hello SAS provided.

    //Return a reference toohello blob using hello SAS URI.
    CloudBlockBlob blob = new CloudBlockBlob(new Uri(sas));

    //Write operation: Write a new blob toohello container.
    try
    {
        string blobContent = "This blob was created with a shared access signature granting write permissions toohello blob. ";
        MemoryStream msWrite = new MemoryStream(Encoding.UTF8.GetBytes(blobContent));
        msWrite.Position = 0;
        using (msWrite)
        {
            blob.UploadFromStream(msWrite);
        }
        Console.WriteLine("Write operation succeeded for SAS " + sas);
        Console.WriteLine();
    }
    catch (StorageException e)
    {
        Console.WriteLine("Write operation failed for SAS " + sas);
        Console.WriteLine("Additional error information: " + e.Message);
        Console.WriteLine();
    }

    //Read operation: Read hello contents of hello blob.
    try
    {
        MemoryStream msRead = new MemoryStream();
        using (msRead)
        {
            blob.DownloadToStream(msRead);
            msRead.Position = 0;
            using (StreamReader reader = new StreamReader(msRead, true))
            {
                string line;
                while ((line = reader.ReadLine()) != null)
                {
                    Console.WriteLine(line);
                }
            }
        }
        Console.WriteLine("Read operation succeeded for SAS " + sas);
        Console.WriteLine();
    }
    catch (StorageException e)
    {
        Console.WriteLine("Read operation failed for SAS " + sas);
        Console.WriteLine("Additional error information: " + e.Message);
        Console.WriteLine();
    }

    //Delete operation: Delete hello blob.
    try
    {
        blob.Delete();
        Console.WriteLine("Delete operation succeeded for SAS " + sas);
        Console.WriteLine();
    }
    catch (StorageException e)
    {
        Console.WriteLine("Delete operation failed for SAS " + sas);
        Console.WriteLine("Additional error information: " + e.Message);
        Console.WriteLine();
    }
}
```

<span data-ttu-id="ec683-194">Uppdatera hello **Main()** metoden toocall **UseBlobSAS()** med både hello signaturer för delad åtkomst som du skapade på hello blob:</span><span class="sxs-lookup"><span data-stu-id="ec683-194">Update hello **Main()** method toocall **UseBlobSAS()** with both of hello shared access signatures that you created on hello blob:</span></span>

```csharp
static void Main(string[] args)
{
    string containerSAS = "<your container SAS>";
    string blobSAS = "<your blob SAS>";
    string containerSASWithAccessPolicy = "<your container SAS with access policy>";
    string blobSASWithAccessPolicy = "<your blob SAS with access policy>";

    //Call hello test methods with hello shared access signatures created on hello container, with and without hello access policy.
    UseContainerSAS(containerSAS);
    UseContainerSAS(containerSASWithAccessPolicy);

    //Call hello test methods with hello shared access signatures created on hello blob, with and without hello access policy.
    UseBlobSAS(blobSAS);
    UseBlobSAS(blobSASWithAccessPolicy);

    Console.ReadLine();
}
```

<span data-ttu-id="ec683-195">Kör hello konsolprogram och se hello utdata toosee vilka åtgärder som är tillåtna för vilka signaturer.</span><span class="sxs-lookup"><span data-stu-id="ec683-195">Run hello console application and observe hello output toosee which operations are permitted for which signatures.</span></span> <span data-ttu-id="ec683-196">hello utdata i konsolfönstret hello ser liknande toohello följande:</span><span class="sxs-lookup"><span data-stu-id="ec683-196">hello output in hello console window will look similar toohello following:</span></span>

```
Write operation succeeded for SAS https://storagesample.blob.core.windows.net/sascontainer?sv=2016-05-31&sr=c&sig=32EaQGuFyDMb3yOAey3wq%2B%2FLwgPQxAgSo7UhzLdyIDU%3D&se=2017-05-16T15%3A41%3A20Z&sp=wl

List operation succeeded for SAS https://storagesample.blob.core.windows.net/sascontainer?sv=2016-05-31&sr=c&sig=32EaQGuFyDMb3yOAey3wq%2B%2FLwgPQxAgSo7UhzLdyIDU%3D&se=2017-05-16T15%3A41%3A20Z&sp=wl

Read operation failed for SAS https://storagesample.blob.core.windows.net/sascontainer?sv=2016-05-31&sr=c&sig=32EaQGuFyDMb3yOAey3wq%2B%2FLwgPQxAgSo7UhzLdyIDU%3D&se=2017-05-16T15%3A41%3A20Z&sp=wl
Additional error information: hello remote server returned an error: (403) Forbidden.

Delete operation failed for SAS https://storagesample.blob.core.windows.net/sascontainer?sv=2016-05-31&sr=c&sig=32EaQGuFyDMb3yOAey3wq%2B%2FLwgPQxAgSo7UhzLdyIDU%3D&se=2017-05-16T15%3A41%3A20Z&sp=wl
Additional error information: hello remote server returned an error: (403) Forbidden.

...
```

## <a name="next-steps"></a><span data-ttu-id="ec683-197">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ec683-197">Next Steps</span></span>

* [<span data-ttu-id="ec683-198">Signaturer för delad åtkomst, del 1: Förstå hello SAS-modellen</span><span class="sxs-lookup"><span data-stu-id="ec683-198">Shared Access Signatures, Part 1: Understanding hello SAS Model</span></span>](storage-dotnet-shared-access-signature-part-1.md)
* [<span data-ttu-id="ec683-199">Hantera anonym läsbehörighet toocontainers och blobbar</span><span class="sxs-lookup"><span data-stu-id="ec683-199">Manage anonymous read access toocontainers and blobs</span></span>](storage-manage-access-to-resources.md)
* [<span data-ttu-id="ec683-200">Delegera åtkomst med en signatur för delad åtkomst (REST-API)</span><span class="sxs-lookup"><span data-stu-id="ec683-200">Delegating access with a shared access signature (REST API)</span></span>](http://msdn.microsoft.com/library/azure/ee395415.aspx)
* [<span data-ttu-id="ec683-201">Introduktion till tabellen och kön SAS</span><span class="sxs-lookup"><span data-stu-id="ec683-201">Introducing Table and Queue SAS</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx)
