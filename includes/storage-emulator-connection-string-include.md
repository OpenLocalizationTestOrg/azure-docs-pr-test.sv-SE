<span data-ttu-id="1ff36-101">Storage-emulatorn stöder ett fast konto och en välkänd autentiseringsnyckel för autentisering med delad nyckel.</span><span class="sxs-lookup"><span data-stu-id="1ff36-101">The storage emulator supports a single fixed account and a well-known authentication key for Shared Key authentication.</span></span> <span data-ttu-id="1ff36-102">Det här kontot och nyckeln är endast delad nyckel autentiseringsuppgifterna som tillåts för användning med storage-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="1ff36-102">This account and key are the only Shared Key credentials permitted for use with the storage emulator.</span></span> <span data-ttu-id="1ff36-103">De är:</span><span class="sxs-lookup"><span data-stu-id="1ff36-103">They are:</span></span>

```
Account name: devstoreaccount1
Account key: Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==
```

> [!NOTE]
> <span data-ttu-id="1ff36-104">Autentiseringsnyckeln stöds av storage-emulatorn är endast avsett för testning funktionerna i din klientkod för autentisering.</span><span class="sxs-lookup"><span data-stu-id="1ff36-104">The authentication key supported by the storage emulator is intended only for testing the functionality of your client authentication code.</span></span> <span data-ttu-id="1ff36-105">Den har inte något syfte säkerhet.</span><span class="sxs-lookup"><span data-stu-id="1ff36-105">It does not serve any security purpose.</span></span> <span data-ttu-id="1ff36-106">Du kan inte använda ditt lagringskonto för produktion och en nyckel med storage-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="1ff36-106">You cannot use your production storage account and key with the storage emulator.</span></span> <span data-ttu-id="1ff36-107">Du bör inte använda kontot för utveckling med produktionsdata.</span><span class="sxs-lookup"><span data-stu-id="1ff36-107">You should not use the development account with production data.</span></span>
> 
> <span data-ttu-id="1ff36-108">Storage-emulatorn stöder endast anslutningar via HTTP.</span><span class="sxs-lookup"><span data-stu-id="1ff36-108">The storage emulator supports connection via HTTP only.</span></span> <span data-ttu-id="1ff36-109">HTTPS är dock rekommenderade protokollet för att komma åt resurser i en Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="1ff36-109">However, HTTPS is the recommended protocol for accessing resources in a production Azure storage account.</span></span>
> 

#### <a name="connect-to-the-emulator-account-using-a-shortcut"></a><span data-ttu-id="1ff36-110">Anslut till emulatorn-konto med hjälp av en genväg</span><span class="sxs-lookup"><span data-stu-id="1ff36-110">Connect to the emulator account using a shortcut</span></span>
<span data-ttu-id="1ff36-111">Det enklaste sättet att ansluta till storage-emulatorn från ditt program är att konfigurera en anslutningssträng i programmets konfigurationsfil som refererar till en genväg till `UseDevelopmentStorage=true`.</span><span class="sxs-lookup"><span data-stu-id="1ff36-111">The easiest way to connect to the storage emulator from your application is to configure a connection string in your application's configuration file that references the shortcut `UseDevelopmentStorage=true`.</span></span> <span data-ttu-id="1ff36-112">Här är ett exempel på en anslutningssträng till storage-emulatorn i en *app.config* fil:</span><span class="sxs-lookup"><span data-stu-id="1ff36-112">Here's an example of a connection string to the storage emulator in an *app.config* file:</span></span> 

```xml
<appSettings>
  <add key="StorageConnectionString" value="UseDevelopmentStorage=true" />
</appSettings>
```

#### <a name="connect-to-the-emulator-account-using-the-well-known-account-name-and-key"></a><span data-ttu-id="1ff36-113">Anslut till emulatorn-konto med hjälp av det välkända kontonamnet och nyckeln</span><span class="sxs-lookup"><span data-stu-id="1ff36-113">Connect to the emulator account using the well-known account name and key</span></span>
<span data-ttu-id="1ff36-114">Om du vill skapa en anslutningssträng som refererar till emulatorn kontonamnet och nyckeln måste du ange slutpunkterna för var och en av de tjänster som du vill använda från emulatorn i anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="1ff36-114">To create a connection string that references the emulator account name and key, you must specify the endpoints for each of the services you wish to use from the emulator in the connection string.</span></span> <span data-ttu-id="1ff36-115">Detta är nödvändigt så att anslutningssträngen refererar emulatorn slutpunkter, vilket skiljer sig från de för ett lagringskonto för produktion.</span><span class="sxs-lookup"><span data-stu-id="1ff36-115">This is necessary so that the connection string will reference the emulator endpoints, which are different than those for a production storage account.</span></span> <span data-ttu-id="1ff36-116">Värdet för anslutningssträngen ska se ut så här:</span><span class="sxs-lookup"><span data-stu-id="1ff36-116">For example, the value of your connection string will look like this:</span></span>

```
DefaultEndpointsProtocol=http;AccountName=devstoreaccount1;
AccountKey=Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==;
BlobEndpoint=http://127.0.0.1:10000/devstoreaccount1;
TableEndpoint=http://127.0.0.1:10002/devstoreaccount1;
QueueEndpoint=http://127.0.0.1:10001/devstoreaccount1;
```

<span data-ttu-id="1ff36-117">Det här värdet är identisk med en genväg som visas ovan, `UseDevelopmentStorage=true`.</span><span class="sxs-lookup"><span data-stu-id="1ff36-117">This value is identical to the shortcut shown above, `UseDevelopmentStorage=true`.</span></span>

#### <a name="specify-an-http-proxy"></a><span data-ttu-id="1ff36-118">Ange en HTTP-proxy</span><span class="sxs-lookup"><span data-stu-id="1ff36-118">Specify an HTTP proxy</span></span>
<span data-ttu-id="1ff36-119">Du kan också ange en HTTP-proxy ska användas när du testar din tjänst mot storage-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="1ff36-119">You can also specify an HTTP proxy to use when you're testing your service against the storage emulator.</span></span> <span data-ttu-id="1ff36-120">Detta kan vara användbart för sett HTTP-begäranden och -svar när du felsöker åtgärder mot storage-tjänster.</span><span class="sxs-lookup"><span data-stu-id="1ff36-120">This can be useful for observing HTTP requests and responses while you're debugging operations against the storage services.</span></span> <span data-ttu-id="1ff36-121">Ange en proxy genom att lägga till den `DevelopmentStorageProxyUri` alternativet i anslutningssträngen och ange värdet till proxy-URI.</span><span class="sxs-lookup"><span data-stu-id="1ff36-121">To specify a proxy, add the `DevelopmentStorageProxyUri` option to the connection string, and set its value to the proxy URI.</span></span> <span data-ttu-id="1ff36-122">Här är till exempel en anslutningssträng som pekar på storage-emulatorn och konfigurerar en HTTP-proxy:</span><span class="sxs-lookup"><span data-stu-id="1ff36-122">For example, here is a connection string that points to the storage emulator and configures an HTTP proxy:</span></span>

```
UseDevelopmentStorage=true;DevelopmentStorageProxyUri=http://myProxyUri
```

