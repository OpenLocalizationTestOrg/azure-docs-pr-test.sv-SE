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
# <a name="connecting-toomedia-services-account-using-media-services-sdk-for-net"></a>Ansluta tooMedia Services-konto med Media Services SDK för .NET
> [!div class="op_single_selector"]
> * [REST](media-services-rest-connect-programmatically.md)
> * [.NET](media-services-dotnet-connect-programmatically.md)
> 
> 

Det här avsnittet beskrivs hur tooobtain programmässiga anslutning-tooMicrosoft Azure Media Services vid programmering med hello Media Services SDK för .NET.

## <a name="connecting-toomedia-services"></a>Anslutningstjänsterna tooMedia
tooconnect tooMedia Services programmässigt, du måste ha tidigare har skapat ett Azure-konto konfigurerat Media Services för kontot och sedan skapa ett Visual Studio-projekt för utveckling med hello Media Services SDK för .NET. Mer information finns i installationsprogrammet för utveckling med hello Media Services SDK för .NET.

Hello slutet av installationen för hello Media Services-konto, som du fick hello följande krävs anslutningsvärden. Använda toomake programmässiga anslutningarna tooMedia tjänster.

* Namnet på ditt Media Services.
* Din nyckel för Media Services-konto.

toofind dessa värden, gå toohello Azure Management Portal, Välj Media Service-konto och klicka på hello ”**hantera nycklar**” ikon på hello ned i portalen hello-fönstret. Klicka på hello ikonen nästa tooeach text rutan kopior hello värdet toohello system Urklipp.

## <a name="creating-a-cloudmediacontext-instance"></a>Skapa en instans av CloudMediaContext
toostart programmera mot Media Services behöver du toocreate en **CloudMediaContext** -instans som representerar hello serverkontext. Hej **CloudMediaContext** innehåller referenser tooimportant samlingar inklusive jobb, tillgångar, filer, principer för åtkomst och lokaliserare.

> [!NOTE]
> Hej **CloudMediaContext** klass är inte trådsäker. Du bör skapa en ny CloudMediaContext per tråd eller uppsättning åtgärder.
> 
> 

CloudMediaContext har fem konstruktorn överlagringar. Är det rekommenderade toouse konstruktorer som tar **MediaServicesCredentials** som en parameter. Mer information finns i hello **återanvända Access Control Service Tokens** som följer. 

hello används följande exempel hello offentlig konstruktor för CloudMediaContext(MediaServicesCredentials credentials):

    // _cachedCredentials and _context are class member variables. 
    _cachedCredentials = new MediaServicesCredentials(
                    _mediaServicesAccountName,
                    _mediaServicesAccountKey);

    _context = new CloudMediaContext(_cachedCredentials);


## <a name="reusing-access-control-service-tokens"></a>Återanvända Access Control Service Tokens
Det här avsnittet visar hur tooreuse åtkomstkontrolltjänsten token med hjälp av CloudMediaContext konstruktorer som tar MediaServicesCredentials som en parameter.

[Azure Active Directory-åtkomstkontroll](https://msdn.microsoft.com/library/hh147631.aspx) (även kallat åtkomstkontrolltjänsten eller ACS) är en molnbaserad tjänst som tillhandahåller ett enkelt sätt för utfärdande och autentiseras användare toogain åtkomst tootheir webbprogram. Microsoft Azure Media Services kontrollerar åtkomsten tooits tjänster om OAuth-protokollet som kräver en ACS-token. Media Services får hello ACS-token från en server för auktorisering.

När du utvecklar med hello Media Services SDK kan du välja toonot behandlar hello token eftersom hello SDK kod chefer dem åt dig. Dock hantera låta hello SDK hello ACS-token leads toounnecessary token-förfrågningar. Begär token tar tid och förbrukar en hel hello klienten och servern. Dessutom begränsar hello ACS-servern hello begäranden om hello frekvensen är för högt. hello gränsen är 30 begäranden per sekund, se [ACS-tjänsten begränsningar](https://msdn.microsoft.com/library/gg185909.aspx) för mer information.

Från och med hello Media Services SDK version 3.0.0.0 kan återanvända du hello ACS-token. Hej **CloudMediaContext** konstruktorer som tar **MediaServicesCredentials** aktivera delning hello ACS-token mellan flera kontexter som en parameter. Hej MediaServicesCredentials klassen kapslar in hello Media Services-autentiseringsuppgifter. Om en ACS-token är tillgänglig och dess förfallotid är känd du skapar en ny instans av MediaServicesCredentials med hello token och skickar den toohello konstruktorn för CloudMediaContext. Observera att hello Media Services SDK automatiskt uppdaterar token när de upphör att gälla. Det finns två sätt tooreuse ACS-token som visas i hello exemplen nedan.

* Du kan cachelagra hello **MediaServicesCredentials** objekt i minnet (till exempel i en statisk klassvariabel). Sen skicka hello cachelagrade objekt toohello CloudMediaContext konstruktor. Hej MediaServicesCredentials objektet innehåller en ACS-token som kan återanvändas om den fortfarande är giltig. Om hello token inte är giltigt och kommer att uppdateras av hello eftersom Media Services SDK med hjälp av autentiseringsuppgifter för hello toohello MediaServicesCredentials konstruktor.
  
    Observera att hello **MediaServicesCredentials** objekt hämtar en giltig token efter hello RefreshToken anropas. Hej **CloudMediaContext** anrop hello **RefreshToken** metod i hello konstruktor. Om du planerar toosave hello tokenvärden tooan externa lagringsenheter, gör att toocheck om hello TokenExpiration värde är giltigt innan du sparar hello token data. Om det inte är giltigt, anropa RefreshToken innan cachelagring.
  
        // Create and cache hello Media Services credentials in a static class variable.
        _cachedCredentials = new MediaServicesCredentials(_mediaServicesAccountName, _mediaServicesAccountKey);

        // Use hello cached credentials toocreate a new CloudMediaContext object.
        if(_cachedCredentials == null)
        {
            _cachedCredentials = new MediaServicesCredentials(_mediaServicesAccountName, _mediaServicesAccountKey);
        }

        CloudMediaContext context = new CloudMediaContext(_cachedCredentials);

* Du kan också cachelagra hello AccessToken sträng och hello TokenExpiration värden. hello värden kunde senare används toocreate nya MediaServicesCredentials objekt med hello cachelagras token data.  Detta är särskilt användbart för scenarier där hello token kan delas på ett säkert sätt mellan flera processer eller datorer.
  
    hello anropa följande kodavsnitt hello SaveTokenDataToExternalStorage och GetTokenDataFromExternalStorage UpdateTokenDataInExternalStorageIfNeeded metoder som inte har definierats i det här exemplet. Du kan definiera dessa metoder toostore, hämta och uppdatera token data i en extern lagringsenhet. 
  
        CloudMediaContext context1 = new CloudMediaContext(_mediaServicesAccountName, _mediaServicesAccountKey);
  
        // Get token values from hello context.
        var accessToken = context1.Credentials.AccessToken;
        var tokenExpiration = context1.Credentials.TokenExpiration;
  
        // Save token values for later use. 
        // hello SaveTokenDataToExternalStorage method should check 
        // whether hello TokenExpiration value is valid before saving hello token data. 
        // If it is not valid, call MediaServicesCredentials’s RefreshToken before caching.
        SaveTokenDataToExternalStorage(accessToken, tokenExpiration);
  
    Använd hello spara tokenvärden toocreate MediaServicesCredentials.

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

    Uppdatera hello token kopia om hello token uppdaterades av hello Media Services SDK. 

        if(tokenExpiration != context2.Credentials.TokenExpiration)
        {
            UpdateTokenDataInExternalStorageIfNeeded(accessToken, context2.Credentials.TokenExpiration);
        }


* Om du har flera Media Services-konton (t.ex, för belastningsdelande ändamål eller Geo-distribution) kan du Cachelagra MediaServicesCredentials objekt med hjälp av hello System.Collections.Concurrent.ConcurrentDictionary samling (hello ConcurrentDictionary samling representerar en trådsäker mängd nyckel/värde-par som kan användas av flera trådar samtidigt). Du kan sedan använda hello GetOrAdd metoden tooget hello cachelagrade autentiseringsuppgifter. 
  
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

## <a name="connecting-tooa-media-services-account-located-in-hello-north-china-region"></a>Ansluta tooa Media Services-konto finns i hello norra Kina region
Om ditt konto finns i hello norra Kina region, Använd följande konstruktor hello:

    public CloudMediaContext(Uri apiServer, string accountName, string accountKey, string scope, string acsBaseAddress)

Exempel:

    _context = new CloudMediaContext(
        new Uri("https://wamsbjbclus001rest-hs.chinacloudapp.cn/API/"),
        _mediaServicesAccountName,
        _mediaServicesAccountKey,
        "urn:WindowsAzureMediaServices",
        "https://wamsprodglobal001acs.accesscontrol.chinacloudapi.cn");


## <a name="storing-connection-values-in-configuration"></a>Lagra anslutningsvärden i konfigurationen
Det är en hög rekommenderas toostore anslutningsvärden, särskilt känsliga värden, till exempel ditt kontonamn och lösenord i konfigurationen. Det är också en rekommenderas tooencrypt känsliga konfigurationsdata. Du kan kryptera hello hela konfigurationsfilen med hjälp av hello Windows Krypterande filsystem (EFS). Välj tooenable EFS på en fil, högerklicka på hello filen **egenskaper**, och aktivera kryptering i hello **Avancerat** fliken Inställningar. Eller skapa en anpassad lösning för att kryptera delar av en konfigurationsfil med hjälp av skyddade configuration. Se [kryptera konfigurationsinformation med skyddade Configuration](https://msdn.microsoft.com/library/53tyfkaw.aspx).

hello följande App.config-filen innehåller hello krävs anslutningsvärden. Hej värden i hello <appSettings> element är hello krävs värden som du har fått från installationsprocessen för hello Media Services-konto.

    <configuration>
      <appSettings>
        <add key="MediaServicesAccountName" value="Media-Services-Account-Name" />
        <add key="MediaServicesAccountKey" value="Media-Services-Account-Key" />
      </appSettings>
    </configuration>


tooretrieve anslutningsvärden från konfigurationen, kan du använda hello **ConfigurationManager** klassen och tilldela sedan hello värden toofields i koden:

    private static readonly string _accountName = ConfigurationManager.AppSettings["MediaServicesAccountName"];
    private static readonly string _accountKey = ConfigurationManager.AppSettings["MediaServicesAccountKey"];



## <a name="media-services-learning-paths"></a>Sökvägar för Media Services-utbildning
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

