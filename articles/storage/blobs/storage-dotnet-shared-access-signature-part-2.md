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
ms.openlocfilehash: 32004d7d29a190a7ed7234513428c3c156b833b7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="shared-access-signatures-part-2-create-and-use-a-sas-with-blob-storage"></a>Signaturer för delad åtkomst, del 2: Skapa och använda en SAS med Blob storage

[Del 1](../common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) i den här kursen utforskade signaturer för delad åtkomst (SAS) och beskrivs bästa praxis för att använda dem. Del 2 visar hur toogenerate och sedan använda delad åtkomst signaturer med Blob storage. hello exemplen är skrivna i C# och använder hello Azure Storage-klientbibliotek för .NET. hello exemplen i den här kursen:

* Generera en signatur för delad åtkomst i en behållare
* Generera en signatur för delad åtkomst på en blob
* Skapa en lagrade åtkomst princip toomanage signaturer på en behållare resurser
* Testa hello delade åtkomstsignaturer i klientprogram

## <a name="about-this-tutorial"></a>Om den här självstudiekursen
I den här självstudiekursen skapar vi två konsolprogram som visar hur du skapar och använder signaturer för delad åtkomst för behållare och blobbar:

**Programmet 1**: hello hanteringsprogram. Genererar en signatur för delad åtkomst för en behållare och en blob. Innehåller hello åtkomstnyckeln för lagringskontot i källkoden.

**Programmet 2**: hello klientprogrammet. Ansluter till behållare och blob resurser med hello delade åtkomstsignaturer skapats med hello första programmet. Använder endast hello delad åtkomst signaturer tooaccess behållare och resurser för blob - har *inte* inkluderar hello åtkomstnyckeln för lagringskontot.

## <a name="part-1-create-a-console-application-toogenerate-shared-access-signatures"></a>Del 1: Skapa en delad toogenerate för konsolen programåtkomst signaturer
Kontrollera först att du har hello Azure Storage-klientbibliotek för .NET installerats. Du kan installera hello [NuGet-paketet](http://nuget.org/packages/WindowsAzure.Storage/ "NuGet-paketet") som innehåller hello senaste sammansättningar för hello klientbiblioteket. Detta är hello rekommenderad metod för att säkerställa att du har hello senaste korrigeringar. Du kan också hämta hello klientbiblioteket som en del av hello senaste versionen av hello [Azure SDK för .NET](https://azure.microsoft.com/downloads/).

Skapa ett nytt Windows-konsolprogram i Visual Studio och ger den namnet **GenerateSharedAccessSignatures**. Lägg till referenser för[Microsoft.WindowsAzure.ConfigurationManager](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager) och [WindowsAzure.Storage](https://www.nuget.org/packages/WindowsAzure.Storage/) med någon av följande metoder hello:

* Använd hello [NuGet-Pakethanteraren](https://docs.nuget.org/consume/installing-nuget) i Visual Studio. Välj **projekt** > **hantera NuGet-paket**, Sök online efter varje paket (Microsoft.WindowsAzure.ConfigurationManager och WindowsAzure.Storage) och installera dem.
* Du kan också söka efter dessa sammansättningar i installationen av hello Azure SDK och Lägg till referenser toothem:
  * Microsoft.WindowsAzure.Configuration.dll
  * Microsoft.WindowsAzure.Storage.dll

Lägg till följande hello överst hello i hello Program.cs-filen, **med** direktiven:

```csharp
using System.IO;
using Microsoft.Azure;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Blob;
```

Redigera hello app.config-filen så att den innehåller en konfigurationsinställning med en anslutningssträng som pekar tooyour storage-konto. Filen app.config bör se ut ungefär toothis en:

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

### <a name="generate-a-shared-access-signature-uri-for-a-container"></a>Generera en signatur för delad åtkomst URI för en behållare
toobegin med vi lägga till en metod toogenerate en signatur för delad åtkomst på en ny behållare. I det här fallet hello signaturen är inte associerad med en lagrad åtkomstprincip, så den bedriver hello URI hello information som anger dess förfallodatum tid och hello behörigheter beviljas.

Lägg först till kod toohello **Main()** metoden tooauthenticate åtkomst tooyour lagringskontot och skapa en ny behållare:

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

Lägg till en metod som genererar hello signatur för delad åtkomst för hello behållare och returnerar hello signatur URI:

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

Lägg till följande rader längst ned hello hello hello **Main()** metoden innan hello anropa för**Console.ReadLine()**, toocall **GetContainerSasUri()** och skriva hello signaturen URI toohello konsolfönstret:

```csharp
//Generate a SAS URI for hello container, without a stored access policy.
Console.WriteLine("Container SAS URI: " + GetContainerSasUri(container));
Console.WriteLine();
```

Kompilera och köra toooutput hello signatur för delad åtkomst URI för hello ny behållare. hello URI är liknande toohello följande:

```
https://storageaccount.blob.core.windows.net/sascontainer?sv=2012-02-12&se=2013-04-13T00%3A12%3A08Z&sr=c&sp=wl&sig=t%2BbzU9%2B7ry4okULN9S0wst%2F8MCUhTjrHyV9rDNLSe8g%3D
```

När du har kört hello kod är signatur för delad åtkomst hello du skapade för hello-behållaren giltiga för hello nästkommande 24 timmar. hello signatur ger en klient behörighet toolist blobbar i behållaren hello och toowrite ny BLOB toohello behållare.

### <a name="generate-a-shared-access-signature-uri-for-a-blob"></a>Generera en signatur för delad åtkomst URI för en blob
Sedan vi skriva liknande kod toocreate en ny blob i hello-behållaren och generera en signatur för delad åtkomst för den. Signatur för delad åtkomst är inte associerad med en princip för lagrade åtkomst så att den omfattar hello starttid och förfallotiden behörighetsinformation i hello URI.

Lägg till en ny metod som skapar en ny blob och skriver vissa text tooit och sedan genererar en signatur för delad åtkomst och returnerar hello signatur URI:

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

Längst ned hello hello **Main()** metod, Lägg till följande rader toocall hello **GetBlobSasUri()**innan hello anropa för**Console.ReadLine()**, och skriva hello delade åtkomst till konsolfönstret signatur URI toohello:

```csharp
//Generate a SAS URI for a blob within hello container, without a stored access policy.
Console.WriteLine("Blob SAS URI: " + GetBlobSasUri(container));
Console.WriteLine();
```

Kompilera och köra toooutput hello signatur för delad åtkomst URI för nya hello-blob. hello URI är liknande toohello följande:

```
https://storageaccount.blob.core.windows.net/sascontainer/sasblob.txt?sv=2012-02-12&st=2013-04-12T23%3A37%3A08Z&se=2013-04-13T00%3A12%3A08Z&sr=b&sp=rw&sig=dF2064yHtc8RusQLvkQFPItYdeOz3zR8zHsDMBi4S30%3D
```

### <a name="create-a-stored-access-policy-on-hello-container"></a>Skapa en princip för lagrade åtkomst på hello behållare
Nu skapar vi en åtkomstprincip som är lagrade på hello behållare, som definierar hello begränsningar för alla signaturer för delad åtkomst som är associerade med den.

I hello föregående exempel kan vi starttid som anges hello (implicit eller explicit), hello förfallotiden och hello behörigheter på hello delad åtkomstsignatur URI sig själv. I följande exempel hello, ange vi dessa hello lagras åtkomstprincip, inte hello signatur för delad åtkomst. Gör så gör oss toochange dessa villkor utan att återutfärda hello delad åtkomst till signatur.

Det är möjligt toohave en eller flera hello begränsningar på hello signatur för delad åtkomst och hello resten på hello lagras åtkomstprincip. Du kan dock endast ange hello starttid, förfallotiden och behörigheter i en plats eller hello andra. Du kan inte ange behörigheter på hello signatur för delad åtkomst och även ange dem på hello lagras åtkomstprincip.

När du lägger till en behållare för lagrade åtkomst princip tooa måste hämta hello behållaren befintliga behörigheter, lägga till hello nya åtkomstprincip och sedan ange hello behållaren behörigheter.

Lägg till en ny metod som skapar en ny princip för lagrade åtkomst på en behållare och returnerar hello namnet på hello principen:

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

Längst ned hello hello **Main()** metoden innan hello anropa för**Console.ReadLine()**, lägga till hello följande rader toofirst Radera alla befintliga principer för åtkomst och sedan anropa hello  **CreateSharedAccessPolicy()** metoden:

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

När du avmarkerar hello åtkomstprinciper i en behållare, måste du först hämta hello behållaren befintliga behörigheter och sedan avmarkera hello behörigheter och sedan ange hello behörigheter igen.

### <a name="generate-a-shared-access-signature-uri-on-hello-container-that-uses-an-access-policy"></a>Generera en signatur för delad åtkomst URI hello-behållaren som använder en åtkomstprincip
Därefter skapar vi en annan signatur för delad åtkomst för hello-behållare som vi skapade tidigare men nu vi hello signaturen till hello lagras åtkomstprincip som vi skapade i föregående exempel för hello.

Lägg till en ny metod toogenerate en annan signatur för delad åtkomst för hello behållare:

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

Längst ned hello hello **Main()** metoden innan hello anropa för**Console.ReadLine()**, Lägg till följande rader toocall hello hello **GetContainerSasUriWithPolicy** metod :

```csharp
//Generate a SAS URI for hello container, using a stored access policy tooset constraints on hello SAS.
Console.WriteLine("Container SAS URI using stored access policy: " + GetContainerSasUriWithPolicy(container, sharedAccessPolicyName));
Console.WriteLine();
```

### <a name="generate-a-shared-access-signature-uri-on-hello-blob-that-uses-an-access-policy"></a>Generera en delad åtkomst signatur URI på hello Blob som använder en åtkomstprincip
Slutligen vi lägga till en liknande metoden toocreate en annan blob och generera en signatur för delad åtkomst som är kopplad till en princip för lagrade åtkomst.

Lägg till en ny metod toocreate en blob och generera en signatur för delad åtkomst:

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

Längst ned hello hello **Main()** metoden innan hello anropa för**Console.ReadLine()**, Lägg till följande rader toocall hello hello **GetBlobSasUriWithPolicy** metod:

```csharp
//Generate a SAS URI for a blob within hello container, using a stored access policy tooset constraints on hello SAS.
Console.WriteLine("Blob SAS URI using stored access policy: " + GetBlobSasUriWithPolicy(container, sharedAccessPolicyName));
Console.WriteLine();
```

Hej **Main()** metod bör nu se ut så här i sin helhet. Kör den signatur för delad åtkomst toowrite hello URI: er toohello konsolfönstret, och sedan kopiera och klistra in dem i en textfil för användning i hello andra delen av den här kursen.

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

När du kör hello GenerateSharedAccessSignatures konsolprogram visas utdata liknande toohello följande. Dessa är hello delade åtkomstsignaturer du använder i en del 2 av hello självstudiekursen.

```
Container SAS URI: https://storagesample.blob.core.windows.net/sascontainer?sv=2016-05-31&sr=c&sig=pFlEZD%2F6sJTNLxD%2FQ26Hh85j%2FzYPxZav6mP1KJwnvJE%3D&se=2017-05-16T16%3A16%3A47Z&sp=wl

Blob SAS URI: https://storagesample.blob.core.windows.net/sascontainer/sasblob.txt?sv=2016-05-31&sr=b&sig=%2FiBWAZbXESzCMvRcm7JwJBK0gT0BtPSWEq4pRwmlBRI%3D&st=2017-05-15T16%3A11%3A48Z&se=2017-05-16T16%3A16%3A48Z&sp=rw

Container SAS URI using stored access policy: https://storagesample.blob.core.windows.net/sascontainer?sv=2016-05-31&sr=c&si=tutorialpolicy&sig=aMb6rKDvvpfiGVsZI2rCmyUra6ZPpq%2BZ%2FLyTgAeec%2Bk%3D

Blob SAS URI using stored access policy: https://storagesample.blob.core.windows.net/sascontainer/sasblobpolicy.txt?sv=2016-05-31&sr=b&si=tutorialpolicy&sig=%2FkTWkT23SS45%2FoF4bK2mqXkN%2BPKs%2FyHuzkfQ4GFoZVU%3D
```

## <a name="part-2-create-a-console-application-tootest-hello-shared-access-signatures"></a>Del 2: Skapa ett program tootest hello delade konsolåtkomst signaturer
tootest hello signaturer för delad åtkomst skapade i föregående exempel för hello, skapar vi ett andra konsolprogram som använder hello signaturer tooperform åtgärder på hello behållare och en blob.

> [!NOTE]
> Om mer än 24 timmar har gått sedan du slutfört hello första delen av kursen hello, kommer hello signaturer som du genererade inte längre giltig. I det här fallet bör du köra hello koden i hello första konsolen programmet toogenerate signaturer för ny delad åtkomst för användning i hello andra delen av kursen hello.
>

Skapa ett nytt Windows-konsolprogram i Visual Studio och ger den namnet **ConsumeSharedAccessSignatures**. Lägg till referenser för[Microsoft.WindowsAzure.ConfigurationManager](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager) och [WindowsAzure.Storage](https://www.nuget.org/packages/WindowsAzure.Storage/), som du gjorde tidigare.

Lägg till följande hello överst hello i hello Program.cs-filen, **med** direktiven:

```csharp
using System.IO;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Blob;
```

I hello brödtext hello **Main()** metod, lägga till hello följande strängkonstanter, ändra deras värden toohello delade åtkomstsignaturer du genererade i del 1 av hello kursen.

```csharp
static void Main(string[] args)
{
    const string containerSAS = "<your container SAS>";
    const string blobSAS = "<your blob SAS>";
    const string containerSASWithAccessPolicy = "<your container SAS with access policy>";
    const string blobSASWithAccessPolicy = "<your blob SAS with access policy>";
}
```

### <a name="add-a-method-tootry-container-operations-using-a-shared-access-signature"></a>Lägg till en metod tootry behållare åtgärder med hjälp av en signatur för delad åtkomst
Nu ska vi lägga till en metod som testar vissa åtgärder för behållaren med hjälp av en signatur för delad åtkomst för hello behållare. hello signatur för delad åtkomst är används tooreturn en referens toohello behållare, autentisera åtkomst toohello behållare baserat på hello signatur enbart.

Lägg till följande metod tooProgram.cs hello:

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

Uppdatera hello **Main()** metoden toocall **UseContainerSAS()** med både hello signaturer för delad åtkomst du skapade på hello behållare:

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

### <a name="add-a-method-tootry-blob-operations-using-a-shared-access-signature"></a>Lägg till en metod tootry blob-åtgärder med hjälp av en signatur för delad åtkomst
Slutligen vi lägga till en metod som testar vissa blob-åtgärder med hjälp av en signatur för delad åtkomst på hello-blob. I detta fall kan vi använda hello konstruktorn **CloudBlockBlob(String)**, och skickar hello-signatur för delad åtkomst, tooreturn en referens toohello blob. Inga andra autentisering krävs. den är baserad på hello signatur enbart.

Lägg till följande metod tooProgram.cs hello:

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

Uppdatera hello **Main()** metoden toocall **UseBlobSAS()** med både hello signaturer för delad åtkomst som du skapade på hello blob:

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

Kör hello konsolprogram och se hello utdata toosee vilka åtgärder som är tillåtna för vilka signaturer. hello utdata i konsolfönstret hello ser liknande toohello följande:

```
Write operation succeeded for SAS https://storagesample.blob.core.windows.net/sascontainer?sv=2016-05-31&sr=c&sig=32EaQGuFyDMb3yOAey3wq%2B%2FLwgPQxAgSo7UhzLdyIDU%3D&se=2017-05-16T15%3A41%3A20Z&sp=wl

List operation succeeded for SAS https://storagesample.blob.core.windows.net/sascontainer?sv=2016-05-31&sr=c&sig=32EaQGuFyDMb3yOAey3wq%2B%2FLwgPQxAgSo7UhzLdyIDU%3D&se=2017-05-16T15%3A41%3A20Z&sp=wl

Read operation failed for SAS https://storagesample.blob.core.windows.net/sascontainer?sv=2016-05-31&sr=c&sig=32EaQGuFyDMb3yOAey3wq%2B%2FLwgPQxAgSo7UhzLdyIDU%3D&se=2017-05-16T15%3A41%3A20Z&sp=wl
Additional error information: hello remote server returned an error: (403) Forbidden.

Delete operation failed for SAS https://storagesample.blob.core.windows.net/sascontainer?sv=2016-05-31&sr=c&sig=32EaQGuFyDMb3yOAey3wq%2B%2FLwgPQxAgSo7UhzLdyIDU%3D&se=2017-05-16T15%3A41%3A20Z&sp=wl
Additional error information: hello remote server returned an error: (403) Forbidden.

...
```

## <a name="next-steps"></a>Nästa steg

* [Signaturer för delad åtkomst, del 1: Förstå hello SAS-modellen](../common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
* [Hantera anonym läsbehörighet toocontainers och blobbar](storage-manage-access-to-resources.md)
* [Delegera åtkomst med en signatur för delad åtkomst (REST-API)](http://msdn.microsoft.com/library/azure/ee395415.aspx)
* [Introduktion till tabellen och kön SAS](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx)
