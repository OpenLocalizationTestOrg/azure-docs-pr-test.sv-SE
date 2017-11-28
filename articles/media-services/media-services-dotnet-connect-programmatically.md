---
title: "aaaConnecting tooMedia Services konto med hjälp av .NET"
description: "Det här avsnittet visar hur tooconnect tooMedia Services med .NET."
services: media-services
documentationcenter: 
author: juliako
manager: erikre
editor: 
ms.assetid: a8412a29-59dc-44a0-ace0-be79a97dab63
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 09/26/2016
ms.author: juliako
ms.openlocfilehash: a23bd285f7cae17ae5831e1e50e73947afbb9a3d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connecting-toomedia-services-account-using-media-services-sdk-for-net"></a><span data-ttu-id="a1bce-103">Ansluta tooMedia Services-konto med Media Services SDK för .NET</span><span class="sxs-lookup"><span data-stu-id="a1bce-103">Connecting tooMedia Services Account using Media Services SDK for .NET</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="a1bce-104">REST</span><span class="sxs-lookup"><span data-stu-id="a1bce-104">REST</span></span>](media-services-rest-connect-programmatically.md)
> * [<span data-ttu-id="a1bce-105">.NET</span><span class="sxs-lookup"><span data-stu-id="a1bce-105">.NET</span></span>](media-services-dotnet-connect-programmatically.md)
> 
> 

<span data-ttu-id="a1bce-106">Det här avsnittet beskrivs hur tooobtain programmässiga anslutning-tooMicrosoft Azure Media Services vid programmering med hello Media Services SDK för .NET.</span><span class="sxs-lookup"><span data-stu-id="a1bce-106">This topic describes how tooobtain a programmatic connection tooMicrosoft Azure Media Services when you are programming with hello Media Services SDK for .NET.</span></span>

## <a name="connecting-toomedia-services"></a><span data-ttu-id="a1bce-107">Anslutningstjänsterna tooMedia</span><span class="sxs-lookup"><span data-stu-id="a1bce-107">Connecting tooMedia Services</span></span>
<span data-ttu-id="a1bce-108">tooconnect tooMedia Services programmässigt, du måste ha tidigare har skapat ett Azure-konto konfigurerat Media Services för kontot och sedan skapa ett Visual Studio-projekt för utveckling med hello Media Services SDK för .NET.</span><span class="sxs-lookup"><span data-stu-id="a1bce-108">tooconnect tooMedia Services programmatically, you must have previously set up an Azure account, configured Media Services on that account, and then set up a Visual Studio project for development with hello Media Services SDK for .NET.</span></span> <span data-ttu-id="a1bce-109">Mer information finns i installationsprogrammet för utveckling med hello Media Services SDK för .NET.</span><span class="sxs-lookup"><span data-stu-id="a1bce-109">For more information, see Setup for Development with hello Media Services SDK for .NET.</span></span>

<span data-ttu-id="a1bce-110">Hello slutet av installationen för hello Media Services-konto, som du fick hello följande krävs anslutningsvärden.</span><span class="sxs-lookup"><span data-stu-id="a1bce-110">At hello end of hello Media Services account setup process, you obtained hello following required connection values.</span></span> <span data-ttu-id="a1bce-111">Använda toomake programmässiga anslutningarna tooMedia tjänster.</span><span class="sxs-lookup"><span data-stu-id="a1bce-111">Use these toomake programmatic connections tooMedia Services.</span></span>

* <span data-ttu-id="a1bce-112">Namnet på ditt Media Services.</span><span class="sxs-lookup"><span data-stu-id="a1bce-112">Your Media Services account name.</span></span>
* <span data-ttu-id="a1bce-113">Din nyckel för Media Services-konto.</span><span class="sxs-lookup"><span data-stu-id="a1bce-113">Your Media Services account key.</span></span>

<span data-ttu-id="a1bce-114">toofind dessa värden, gå toohello Azure Management Portal, Välj Media Service-konto och klicka på hello ”**hantera nycklar**” ikon på hello ned i portalen hello-fönstret.</span><span class="sxs-lookup"><span data-stu-id="a1bce-114">toofind these values, go toohello Azure Managment Portal, select your Media Service account, and click on hello “**MANAGE KEYS**” icon on hello bottom of hello portal window.</span></span> <span data-ttu-id="a1bce-115">Klicka på hello ikonen nästa tooeach text rutan kopior hello värdet toohello system Urklipp.</span><span class="sxs-lookup"><span data-stu-id="a1bce-115">Clicking on hello icon next tooeach text box copies hello value toohello system clipboard.</span></span>

## <a name="creating-a-cloudmediacontext-instance"></a><span data-ttu-id="a1bce-116">Skapa en instans av CloudMediaContext</span><span class="sxs-lookup"><span data-stu-id="a1bce-116">Creating a CloudMediaContext Instance</span></span>
<span data-ttu-id="a1bce-117">toostart programmera mot Media Services behöver du toocreate en **CloudMediaContext** -instans som representerar hello serverkontext.</span><span class="sxs-lookup"><span data-stu-id="a1bce-117">toostart programming against Media Services you need toocreate a **CloudMediaContext** instance that represents hello server context.</span></span> <span data-ttu-id="a1bce-118">Hej **CloudMediaContext** innehåller referenser tooimportant samlingar inklusive jobb, tillgångar, filer, principer för åtkomst och lokaliserare.</span><span class="sxs-lookup"><span data-stu-id="a1bce-118">hello **CloudMediaContext** includes references tooimportant collections including jobs, assets, files, access policies, and locators.</span></span>

> [!NOTE]
> <span data-ttu-id="a1bce-119">Hej **CloudMediaContext** klass är inte trådsäker.</span><span class="sxs-lookup"><span data-stu-id="a1bce-119">hello **CloudMediaContext** class is not thread safe.</span></span> <span data-ttu-id="a1bce-120">Du bör skapa en ny CloudMediaContext per tråd eller uppsättning åtgärder.</span><span class="sxs-lookup"><span data-stu-id="a1bce-120">You should create a new CloudMediaContext per thread or per set of operations.</span></span>
> 
> 

<span data-ttu-id="a1bce-121">CloudMediaContext har fem konstruktorn överlagringar.</span><span class="sxs-lookup"><span data-stu-id="a1bce-121">CloudMediaContext has five constructor overloads.</span></span> <span data-ttu-id="a1bce-122">Är det rekommenderade toouse konstruktorer som tar **MediaServicesCredentials** som en parameter.</span><span class="sxs-lookup"><span data-stu-id="a1bce-122">It is recommended toouse constructors that take **MediaServicesCredentials** as a parameter.</span></span> <span data-ttu-id="a1bce-123">Mer information finns i hello **återanvända Access Control Service Tokens** som följer.</span><span class="sxs-lookup"><span data-stu-id="a1bce-123">For more information, see hello **Reusing Access Control Service Tokens** that follows.</span></span> 

<span data-ttu-id="a1bce-124">hello används följande exempel hello offentlig konstruktor för CloudMediaContext(MediaServicesCredentials credentials):</span><span class="sxs-lookup"><span data-stu-id="a1bce-124">hello following example uses hello public CloudMediaContext(MediaServicesCredentials credentials) constructor:</span></span>

    // _cachedCredentials and _context are class member variables. 
    _cachedCredentials = new MediaServicesCredentials(
                    _mediaServicesAccountName,
                    _mediaServicesAccountKey);

    _context = new CloudMediaContext(_cachedCredentials);


## <a name="reusing-access-control-service-tokens"></a><span data-ttu-id="a1bce-125">Återanvända Access Control Service Tokens</span><span class="sxs-lookup"><span data-stu-id="a1bce-125">Reusing Access Control Service Tokens</span></span>
<span data-ttu-id="a1bce-126">Det här avsnittet visar hur tooreuse åtkomstkontrolltjänsten token med hjälp av CloudMediaContext konstruktorer som tar MediaServicesCredentials som en parameter.</span><span class="sxs-lookup"><span data-stu-id="a1bce-126">This section shows how tooreuse Access Control Service tokens by using CloudMediaContext constructors that take MediaServicesCredentials as a parameter.</span></span>

<span data-ttu-id="a1bce-127">[Azure Active Directory-åtkomstkontroll](https://msdn.microsoft.com/library/hh147631.aspx) (även kallat åtkomstkontrolltjänsten eller ACS) är en molnbaserad tjänst som tillhandahåller ett enkelt sätt för utfärdande och autentiseras användare toogain åtkomst tootheir webbprogram.</span><span class="sxs-lookup"><span data-stu-id="a1bce-127">[Azure Active Directory Access Control](https://msdn.microsoft.com/library/hh147631.aspx) (also known as Access Control Service or ACS) is a cloud-based service that provides an easy way of authenticating and authorizing users toogain access tootheir web applications.</span></span> <span data-ttu-id="a1bce-128">Microsoft Azure Media Services kontrollerar åtkomsten tooits tjänster om OAuth-protokollet som kräver en ACS-token.</span><span class="sxs-lookup"><span data-stu-id="a1bce-128">Microsoft Azure Media Services controls access tooits services though OAuth protocol that requires an ACS token.</span></span> <span data-ttu-id="a1bce-129">Media Services får hello ACS-token från en server för auktorisering.</span><span class="sxs-lookup"><span data-stu-id="a1bce-129">Media Services receives hello ACS tokens from an authorization server.</span></span>

<span data-ttu-id="a1bce-130">När du utvecklar med hello Media Services SDK kan du välja toonot behandlar hello token eftersom hello SDK kod chefer dem åt dig.</span><span class="sxs-lookup"><span data-stu-id="a1bce-130">When developing with hello Media Services SDK, you can choose toonot deal with hello tokens because hello SDK code managers them for you.</span></span> <span data-ttu-id="a1bce-131">Dock hantera låta hello SDK hello ACS-token leads toounnecessary token-förfrågningar.</span><span class="sxs-lookup"><span data-stu-id="a1bce-131">However, letting hello SDK fully manage hello ACS tokens leads toounnecessary token requests.</span></span> <span data-ttu-id="a1bce-132">Begär token tar tid och förbrukar en hel hello klienten och servern.</span><span class="sxs-lookup"><span data-stu-id="a1bce-132">Requesting tokens takes time and consumes hello client and server resources.</span></span> <span data-ttu-id="a1bce-133">Dessutom begränsar hello ACS-servern hello begäranden om hello frekvensen är för högt.</span><span class="sxs-lookup"><span data-stu-id="a1bce-133">Also, hello ACS server throttles hello requests if hello rate is too high.</span></span> <span data-ttu-id="a1bce-134">hello gränsen är 30 begäranden per sekund, se [ACS-tjänsten begränsningar](https://msdn.microsoft.com/library/gg185909.aspx) för mer information.</span><span class="sxs-lookup"><span data-stu-id="a1bce-134">hello limit is 30 requests per second, see [ACS Service Limitations](https://msdn.microsoft.com/library/gg185909.aspx) for more details.</span></span>

<span data-ttu-id="a1bce-135">Från och med hello Media Services SDK version 3.0.0.0 kan återanvända du hello ACS-token.</span><span class="sxs-lookup"><span data-stu-id="a1bce-135">Starting with hello Media Services SDK version 3.0.0.0, you can reuse hello ACS tokens.</span></span> <span data-ttu-id="a1bce-136">Hej **CloudMediaContext** konstruktorer som tar **MediaServicesCredentials** aktivera delning hello ACS-token mellan flera kontexter som en parameter.</span><span class="sxs-lookup"><span data-stu-id="a1bce-136">hello **CloudMediaContext** constructors that take **MediaServicesCredentials** as a parameter enable sharing hello ACS tokens between multiple contexts.</span></span> <span data-ttu-id="a1bce-137">Hej MediaServicesCredentials klassen kapslar in hello Media Services-autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="a1bce-137">hello MediaServicesCredentials class encapsulates hello Media Services credentials.</span></span> <span data-ttu-id="a1bce-138">Om en ACS-token är tillgänglig och dess förfallotid är känd du skapar en ny instans av MediaServicesCredentials med hello token och skickar den toohello konstruktorn för CloudMediaContext.</span><span class="sxs-lookup"><span data-stu-id="a1bce-138">If an ACS token is available and its expiration time is known, you can create a new MediaServicesCredentials instance with hello token and pass it toohello constructor of CloudMediaContext.</span></span> <span data-ttu-id="a1bce-139">Observera att hello Media Services SDK automatiskt uppdaterar token när de upphör att gälla.</span><span class="sxs-lookup"><span data-stu-id="a1bce-139">Note that hello Media Services SDK automatically refreshes tokens whenever they expire.</span></span> <span data-ttu-id="a1bce-140">Det finns två sätt tooreuse ACS-token som visas i hello exemplen nedan.</span><span class="sxs-lookup"><span data-stu-id="a1bce-140">There are two ways tooreuse ACS tokens, as shown in hello examples below.</span></span>

* <span data-ttu-id="a1bce-141">Du kan cachelagra hello **MediaServicesCredentials** objekt i minnet (till exempel i en statisk klassvariabel).</span><span class="sxs-lookup"><span data-stu-id="a1bce-141">You can cache hello **MediaServicesCredentials** object in memory (for example, in a static class variable).</span></span> <span data-ttu-id="a1bce-142">Sen skicka hello cachelagrade objekt toohello CloudMediaContext konstruktor.</span><span class="sxs-lookup"><span data-stu-id="a1bce-142">Then, pass hello cached object toohello CloudMediaContext constructor.</span></span> <span data-ttu-id="a1bce-143">Hej MediaServicesCredentials objektet innehåller en ACS-token som kan återanvändas om den fortfarande är giltig.</span><span class="sxs-lookup"><span data-stu-id="a1bce-143">hello MediaServicesCredentials object contains an ACS token that can be reused if it is still valid.</span></span> <span data-ttu-id="a1bce-144">Om hello token inte är giltigt och kommer att uppdateras av hello eftersom Media Services SDK med hjälp av autentiseringsuppgifter för hello toohello MediaServicesCredentials konstruktor.</span><span class="sxs-lookup"><span data-stu-id="a1bce-144">If hello token is not valid, it will be refreshed by hello Media Services SDK using hello credentials given toohello MediaServicesCredentials constructor.</span></span>
  
    <span data-ttu-id="a1bce-145">Observera att hello **MediaServicesCredentials** objekt hämtar en giltig token efter hello RefreshToken anropas.</span><span class="sxs-lookup"><span data-stu-id="a1bce-145">Note that hello **MediaServicesCredentials** object gets a valid token after hello RefreshToken is called.</span></span> <span data-ttu-id="a1bce-146">Hej **CloudMediaContext** anrop hello **RefreshToken** metod i hello konstruktor.</span><span class="sxs-lookup"><span data-stu-id="a1bce-146">hello **CloudMediaContext** calls hello **RefreshToken** method in hello constructor.</span></span> <span data-ttu-id="a1bce-147">Om du planerar toosave hello tokenvärden tooan externa lagringsenheter, gör att toocheck om hello TokenExpiration värde är giltigt innan du sparar hello token data.</span><span class="sxs-lookup"><span data-stu-id="a1bce-147">If you are planning toosave hello token values tooan external storage, make sure toocheck whether hello TokenExpiration value is valid before saving hello token data.</span></span> <span data-ttu-id="a1bce-148">Om det inte är giltigt, anropa RefreshToken innan cachelagring.</span><span class="sxs-lookup"><span data-stu-id="a1bce-148">If it is not valid, call RefreshToken before caching.</span></span>
  
        // Create and cache hello Media Services credentials in a static class variable.
        _cachedCredentials = new MediaServicesCredentials(_mediaServicesAccountName, _mediaServicesAccountKey);

        // Use hello cached credentials toocreate a new CloudMediaContext object.
        if(_cachedCredentials == null)
        {
            _cachedCredentials = new MediaServicesCredentials(_mediaServicesAccountName, _mediaServicesAccountKey);
        }

        CloudMediaContext context = new CloudMediaContext(_cachedCredentials);

* <span data-ttu-id="a1bce-149">Du kan också cachelagra hello AccessToken sträng och hello TokenExpiration värden.</span><span class="sxs-lookup"><span data-stu-id="a1bce-149">You can also cache hello AccessToken string and hello TokenExpiration values.</span></span> <span data-ttu-id="a1bce-150">hello värden kunde senare används toocreate nya MediaServicesCredentials objekt med hello cachelagras token data.</span><span class="sxs-lookup"><span data-stu-id="a1bce-150">hello values could later be used toocreate a new MediaServicesCredentials object with hello cached token data.</span></span>  <span data-ttu-id="a1bce-151">Detta är särskilt användbart för scenarier där hello token kan delas på ett säkert sätt mellan flera processer eller datorer.</span><span class="sxs-lookup"><span data-stu-id="a1bce-151">This is especially useful for scenarios where hello token can be securely shared among multiple processes or computers.</span></span>
  
    <span data-ttu-id="a1bce-152">hello anropa följande kodavsnitt hello SaveTokenDataToExternalStorage och GetTokenDataFromExternalStorage UpdateTokenDataInExternalStorageIfNeeded metoder som inte har definierats i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="a1bce-152">hello following code snippets call hello SaveTokenDataToExternalStorage, GetTokenDataFromExternalStorage, and UpdateTokenDataInExternalStorageIfNeeded methods that are not defined in this example.</span></span> <span data-ttu-id="a1bce-153">Du kan definiera dessa metoder toostore, hämta och uppdatera token data i en extern lagringsenhet.</span><span class="sxs-lookup"><span data-stu-id="a1bce-153">You could define these methods toostore, retrieve, and update token data in an external storage.</span></span> 
  
        CloudMediaContext context1 = new CloudMediaContext(_mediaServicesAccountName, _mediaServicesAccountKey);
  
        // Get token values from hello context.
        var accessToken = context1.Credentials.AccessToken;
        var tokenExpiration = context1.Credentials.TokenExpiration;
  
        // Save token values for later use. 
        // hello SaveTokenDataToExternalStorage method should check 
        // whether hello TokenExpiration value is valid before saving hello token data. 
        // If it is not valid, call MediaServicesCredentials’s RefreshToken before caching.
        SaveTokenDataToExternalStorage(accessToken, tokenExpiration);
  
    <span data-ttu-id="a1bce-154">Använd hello spara tokenvärden toocreate MediaServicesCredentials.</span><span class="sxs-lookup"><span data-stu-id="a1bce-154">Use hello saved token values toocreate MediaServicesCredentials.</span></span>

        var accessToken = "";
        var tokenExpiration = DateTime.UtcNow;

        // Retrieve saved token values.
        GetTokenDataFromExternalStorage(out accessToken, out tokenExpiration);

        // Create a new MediaServicesCredentials object using saved token values.
        MediaServicesCredentials credentials = new MediaServicesCredentials(_mediaServicesAccountName, _mediaServicesAccountKey)
        {
            AccessToken = accessToken,
            TokenExpiration = tokenExpiration
        };

        CloudMediaContext context2 = new CloudMediaContext(credentials);

    <span data-ttu-id="a1bce-155">Uppdatera hello token kopia om hello token uppdaterades av hello Media Services SDK.</span><span class="sxs-lookup"><span data-stu-id="a1bce-155">Update hello token copy in case hello token was updated by hello Media Services SDK.</span></span> 

        if(tokenExpiration != context2.Credentials.TokenExpiration)
        {
            UpdateTokenDataInExternalStorageIfNeeded(accessToken, context2.Credentials.TokenExpiration);
        }


* <span data-ttu-id="a1bce-156">Om du har flera Media Services-konton (t.ex, för belastningsdelande ändamål eller Geo-distribution) kan du Cachelagra MediaServicesCredentials objekt med hjälp av hello System.Collections.Concurrent.ConcurrentDictionary samling (hello ConcurrentDictionary samling representerar en trådsäker mängd nyckel/värde-par som kan användas av flera trådar samtidigt).</span><span class="sxs-lookup"><span data-stu-id="a1bce-156">If you have multiple Media Services accounts (for example, for load sharing purposes or Geo-distribution) you can cache MediaServicesCredentials objects using hello System.Collections.Concurrent.ConcurrentDictionary collection (hello ConcurrentDictionary collection represents a thread-safe collection of key/value pairs that can be accessed by multiple threads concurrently).</span></span> <span data-ttu-id="a1bce-157">Du kan sedan använda hello GetOrAdd metoden tooget hello cachelagrade autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="a1bce-157">You can then use hello GetOrAdd method tooget hello cached credentials.</span></span> 
  
        // Declare a static class variable of hello ConcurrentDictionary type in which hello Media Services credentials will be cached.  
        private static readonly ConcurrentDictionary<string, MediaServicesCredentials> mediaServicesCredentialsCache = 
            new ConcurrentDictionary<string, MediaServicesCredentials>();

        // Cache (or get already cached) Media Services credentials. Use these credentials toocreate a new CloudMediaContext object.
        static public CloudMediaContext CreateMediaServicesContext(string accountName, string accountKey)
        {
            CloudMediaContext cloudMediaContext;
            MediaServicesCredentials mediaServicesCredentials;

            mediaServicesCredentials = mediaServicesCredentialsCache.GetOrAdd(
                accountName,
                valueFactory => new MediaServicesCredentials(accountName, accountKey));

            cloudMediaContext = new CloudMediaContext(mediaServicesCredentials);

            return cloudMediaContext;
        }

## <a name="connecting-tooa-media-services-account-located-in-hello-north-china-region"></a><span data-ttu-id="a1bce-158">Ansluta tooa Media Services-konto finns i hello norra Kina region</span><span class="sxs-lookup"><span data-stu-id="a1bce-158">Connecting tooa Media Services account located in hello North China region</span></span>
<span data-ttu-id="a1bce-159">Om ditt konto finns i hello norra Kina region, Använd följande konstruktor hello:</span><span class="sxs-lookup"><span data-stu-id="a1bce-159">If your account is located in hello North China region, use hello following constructor:</span></span>

    public CloudMediaContext(Uri apiServer, string accountName, string accountKey, string scope, string acsBaseAddress)

<span data-ttu-id="a1bce-160">Exempel:</span><span class="sxs-lookup"><span data-stu-id="a1bce-160">For example:</span></span>

    _context = new CloudMediaContext(
        new Uri("https://wamsbjbclus001rest-hs.chinacloudapp.cn/API/"),
        _mediaServicesAccountName,
        _mediaServicesAccountKey,
        "urn:WindowsAzureMediaServices",
        "https://wamsprodglobal001acs.accesscontrol.chinacloudapi.cn");


## <a name="storing-connection-values-in-configuration"></a><span data-ttu-id="a1bce-161">Lagra anslutningsvärden i konfigurationen</span><span class="sxs-lookup"><span data-stu-id="a1bce-161">Storing Connection Values in Configuration</span></span>
<span data-ttu-id="a1bce-162">Det är en hög rekommenderas toostore anslutningsvärden, särskilt känsliga värden, till exempel ditt kontonamn och lösenord i konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="a1bce-162">It is a highly recommended practice toostore connection values, especially sensitive values such as your account name and password, in configuration.</span></span> <span data-ttu-id="a1bce-163">Det är också en rekommenderas tooencrypt känsliga konfigurationsdata.</span><span class="sxs-lookup"><span data-stu-id="a1bce-163">Also, it is a recommended practice tooencrypt sensitive configuration data.</span></span> <span data-ttu-id="a1bce-164">Du kan kryptera hello hela konfigurationsfilen med hjälp av hello Windows Krypterande filsystem (EFS).</span><span class="sxs-lookup"><span data-stu-id="a1bce-164">You can encrypt hello entire configuration file by using hello Windows Encrypting File System (EFS).</span></span> <span data-ttu-id="a1bce-165">Välj tooenable EFS på en fil, högerklicka på hello filen **egenskaper**, och aktivera kryptering i hello **Avancerat** fliken Inställningar. Eller skapa en anpassad lösning för att kryptera delar av en konfigurationsfil med hjälp av skyddade configuration.</span><span class="sxs-lookup"><span data-stu-id="a1bce-165">tooenable EFS on a file, right-click hello file, select **Properties**, and enable encryption in hello **Advanced** settings tab. Or you can create a custom solution for encrypting selected portions of a configuration file by using protected configuration.</span></span> <span data-ttu-id="a1bce-166">Se [kryptera konfigurationsinformation med skyddade Configuration](https://msdn.microsoft.com/library/53tyfkaw.aspx).</span><span class="sxs-lookup"><span data-stu-id="a1bce-166">See [Encrypting Configuration Information Using Protected Configuration](https://msdn.microsoft.com/library/53tyfkaw.aspx).</span></span>

<span data-ttu-id="a1bce-167">hello följande App.config-filen innehåller hello krävs anslutningsvärden.</span><span class="sxs-lookup"><span data-stu-id="a1bce-167">hello following App.config file contains hello required connection values.</span></span> <span data-ttu-id="a1bce-168">Hej värden i hello <appSettings> element är hello krävs värden som du har fått från installationsprocessen för hello Media Services-konto.</span><span class="sxs-lookup"><span data-stu-id="a1bce-168">hello values in hello <appSettings> element are hello required values that you got from hello Media Services account setup process.</span></span>

    <configuration>
      <appSettings>
        <add key="MediaServicesAccountName" value="Media-Services-Account-Name" />
        <add key="MediaServicesAccountKey" value="Media-Services-Account-Key" />
      </appSettings>
    </configuration>


<span data-ttu-id="a1bce-169">tooretrieve anslutningsvärden från konfigurationen, kan du använda hello **ConfigurationManager** klassen och tilldela sedan hello värden toofields i koden:</span><span class="sxs-lookup"><span data-stu-id="a1bce-169">tooretrieve connection values from configuration, you can use hello **ConfigurationManager** class and then assign hello values toofields in your code:</span></span>

    private static readonly string _accountName = ConfigurationManager.AppSettings["MediaServicesAccountName"];
    private static readonly string _accountKey = ConfigurationManager.AppSettings["MediaServicesAccountKey"];



## <a name="media-services-learning-paths"></a><span data-ttu-id="a1bce-170">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="a1bce-170">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="a1bce-171">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="a1bce-171">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

