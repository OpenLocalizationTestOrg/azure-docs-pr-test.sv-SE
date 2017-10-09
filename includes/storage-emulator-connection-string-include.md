<span data-ttu-id="21b2f-101">hello storage-emulatorn stöder ett fast konto och en välkänd autentiseringsnyckel för autentisering med delad nyckel.</span><span class="sxs-lookup"><span data-stu-id="21b2f-101">hello storage emulator supports a single fixed account and a well-known authentication key for Shared Key authentication.</span></span> <span data-ttu-id="21b2f-102">Det här kontot och nyckeln är hello endast delad nyckel autentiseringsuppgifter som tillåts för användning med hello storage-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="21b2f-102">This account and key are hello only Shared Key credentials permitted for use with hello storage emulator.</span></span> <span data-ttu-id="21b2f-103">De är:</span><span class="sxs-lookup"><span data-stu-id="21b2f-103">They are:</span></span>

```
Account name: devstoreaccount1
Account key: Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==
```

> [!NOTE]
> <span data-ttu-id="21b2f-104">hello autentiseringsnyckel som stöds av hello storage-emulatorn är endast avsett för testning hello-funktionerna i din klientkod för autentisering.</span><span class="sxs-lookup"><span data-stu-id="21b2f-104">hello authentication key supported by hello storage emulator is intended only for testing hello functionality of your client authentication code.</span></span> <span data-ttu-id="21b2f-105">Den har inte något syfte säkerhet.</span><span class="sxs-lookup"><span data-stu-id="21b2f-105">It does not serve any security purpose.</span></span> <span data-ttu-id="21b2f-106">Du kan inte använda ditt lagringskonto för produktion och en nyckel med hello storage-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="21b2f-106">You cannot use your production storage account and key with hello storage emulator.</span></span> <span data-ttu-id="21b2f-107">Du bör inte använda hello development konto med produktionsdata.</span><span class="sxs-lookup"><span data-stu-id="21b2f-107">You should not use hello development account with production data.</span></span>
> 
> <span data-ttu-id="21b2f-108">hello storage-emulatorn stöder endast anslutningar via HTTP.</span><span class="sxs-lookup"><span data-stu-id="21b2f-108">hello storage emulator supports connection via HTTP only.</span></span> <span data-ttu-id="21b2f-109">HTTPS är dock hello rekommenderas protokoll för att komma åt resurser i en Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="21b2f-109">However, HTTPS is hello recommended protocol for accessing resources in a production Azure storage account.</span></span>
> 

#### <a name="connect-toohello-emulator-account-using-a-shortcut"></a><span data-ttu-id="21b2f-110">Ansluta toohello emulatorn konto med hjälp av en genväg</span><span class="sxs-lookup"><span data-stu-id="21b2f-110">Connect toohello emulator account using a shortcut</span></span>
<span data-ttu-id="21b2f-111">hello enklaste sättet tooconnect toohello storage-emulatorn från ditt program är tooconfigure en anslutningssträng i programmets konfigurationsfil som refererar till hello genvägen `UseDevelopmentStorage=true`.</span><span class="sxs-lookup"><span data-stu-id="21b2f-111">hello easiest way tooconnect toohello storage emulator from your application is tooconfigure a connection string in your application's configuration file that references hello shortcut `UseDevelopmentStorage=true`.</span></span> <span data-ttu-id="21b2f-112">Här är ett exempel på en anslutning sträng toohello storage-emulatorn i en *app.config* fil:</span><span class="sxs-lookup"><span data-stu-id="21b2f-112">Here's an example of a connection string toohello storage emulator in an *app.config* file:</span></span> 

```xml
<appSettings>
  <add key="StorageConnectionString" value="UseDevelopmentStorage=true" />
</appSettings>
```

#### <a name="connect-toohello-emulator-account-using-hello-well-known-account-name-and-key"></a><span data-ttu-id="21b2f-113">Ansluta toohello emulatorn konto med hjälp av hello välkända kontonamnet och nyckeln</span><span class="sxs-lookup"><span data-stu-id="21b2f-113">Connect toohello emulator account using hello well-known account name and key</span></span>
<span data-ttu-id="21b2f-114">toocreate en anslutningssträng att referenser hello emulatorn kontonamnet och nyckeln, måste du ange hello slutpunkter för var och en av hello tjänster du vill toouse från hello emulatorn i hello anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="21b2f-114">toocreate a connection string that references hello emulator account name and key, you must specify hello endpoints for each of hello services you wish toouse from hello emulator in hello connection string.</span></span> <span data-ttu-id="21b2f-115">Detta är nödvändigt så att hello anslutningssträngen refererar hello emulatorn slutpunkter som är annorlunda än för ett lagringskonto för produktion.</span><span class="sxs-lookup"><span data-stu-id="21b2f-115">This is necessary so that hello connection string will reference hello emulator endpoints, which are different than those for a production storage account.</span></span> <span data-ttu-id="21b2f-116">Hello-värdet för anslutningssträngen ska se ut så här:</span><span class="sxs-lookup"><span data-stu-id="21b2f-116">For example, hello value of your connection string will look like this:</span></span>

```
DefaultEndpointsProtocol=http;AccountName=devstoreaccount1;
AccountKey=Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==;
BlobEndpoint=http://127.0.0.1:10000/devstoreaccount1;
TableEndpoint=http://127.0.0.1:10002/devstoreaccount1;
QueueEndpoint=http://127.0.0.1:10001/devstoreaccount1;
```

<span data-ttu-id="21b2f-117">Det här värdet är identiska toohello genväg som visas ovan, `UseDevelopmentStorage=true`.</span><span class="sxs-lookup"><span data-stu-id="21b2f-117">This value is identical toohello shortcut shown above, `UseDevelopmentStorage=true`.</span></span>

#### <a name="specify-an-http-proxy"></a><span data-ttu-id="21b2f-118">Ange en HTTP-proxy</span><span class="sxs-lookup"><span data-stu-id="21b2f-118">Specify an HTTP proxy</span></span>
<span data-ttu-id="21b2f-119">Du kan även ange en HTTP-proxy toouse när du testar din tjänst mot hello storage-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="21b2f-119">You can also specify an HTTP proxy toouse when you're testing your service against hello storage emulator.</span></span> <span data-ttu-id="21b2f-120">Detta kan vara användbart för sett HTTP-begäranden och -svar när du felsöker åtgärder mot hello lagringstjänster.</span><span class="sxs-lookup"><span data-stu-id="21b2f-120">This can be useful for observing HTTP requests and responses while you're debugging operations against hello storage services.</span></span> <span data-ttu-id="21b2f-121">toospecify en proxy, lägga till hello `DevelopmentStorageProxyUri` alternativet toohello anslutningssträngen och ange dess värde toohello proxy-URI.</span><span class="sxs-lookup"><span data-stu-id="21b2f-121">toospecify a proxy, add hello `DevelopmentStorageProxyUri` option toohello connection string, and set its value toohello proxy URI.</span></span> <span data-ttu-id="21b2f-122">Här är till exempel en anslutningssträng som pekar toohello storage-emulatorn och konfigurerar en HTTP-proxy:</span><span class="sxs-lookup"><span data-stu-id="21b2f-122">For example, here is a connection string that points toohello storage emulator and configures an HTTP proxy:</span></span>

```
UseDevelopmentStorage=true;DevelopmentStorageProxyUri=http://myProxyUri
```

