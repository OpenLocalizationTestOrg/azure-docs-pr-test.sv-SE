---
title: "aaaSession tillstånd med Azure Redis-cache i Azure App Service"
description: "Lär dig hur toouse hello Azure Cache-tjänst toosupport ASP.NET session cachelagring."
services: app-service\web
documentationcenter: .net
author: Rick-Anderson
manager: erikre
editor: none
ms.assetid: 4f98d289-2698-464d-85cd-99ec40fad1f2
ms.service: app-service-web
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: get-started-article
ms.date: 06/27/2016
ms.author: riande
ms.openlocfilehash: f689b6754ea072aa195f822ab6482f4bf2748375
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="session-state-with-azure-redis-cache-in-azure-app-service"></a><span data-ttu-id="1268a-103">Sessionstillstånd med Azure Redis-cache i Azure App Service</span><span class="sxs-lookup"><span data-stu-id="1268a-103">Session state with Azure Redis cache in Azure App Service</span></span>
<span data-ttu-id="1268a-104">Det här avsnittet beskrivs hur toouse hello Azure Redis-Cache-tjänst för sessionstillstånd.</span><span class="sxs-lookup"><span data-stu-id="1268a-104">This topic explains how toouse hello Azure Redis Cache Service for session state.</span></span>

<span data-ttu-id="1268a-105">Om din ASP.NET-webbapp använder sessionstillstånd, behöver du tooconfigure en extern sessionstillståndsprovider (hello Redis Cache-tjänst eller en sessionstillståndsprovider för SQL Server).</span><span class="sxs-lookup"><span data-stu-id="1268a-105">If your ASP.NET web app uses session state, you will need tooconfigure an external session state provider (either hello Redis Cache Service or a SQL Server session state provider).</span></span> <span data-ttu-id="1268a-106">Om du använder sessionstillstånd och inte använder en extern leverantör, kommer du att begränsad tooone instans av webbappen.</span><span class="sxs-lookup"><span data-stu-id="1268a-106">If you use session state, and don't use an external provider, you will be limited tooone instance of your web app.</span></span> <span data-ttu-id="1268a-107">Hej Redis Cache-tjänst är hello snabbast och enklast tooenable.</span><span class="sxs-lookup"><span data-stu-id="1268a-107">hello Redis Cache Service is hello fastest and simplest tooenable.</span></span>

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <span data-ttu-id="1268a-108"><a id="createcache"></a>Skapa hello Cache</span><span class="sxs-lookup"><span data-stu-id="1268a-108"><a id="createcache"></a>Create hello Cache</span></span>
<span data-ttu-id="1268a-109">Följ [de här anvisningarna](../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md#create-cache) toocreate hello cache.</span><span class="sxs-lookup"><span data-stu-id="1268a-109">Follow [these directions](../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md#create-cache) toocreate hello cache.</span></span>

## <span data-ttu-id="1268a-110"><a id="configureproject"></a>Lägg till hello RedisSessionStateProvider NuGet-paketet tooyour webbprogram</span><span class="sxs-lookup"><span data-stu-id="1268a-110"><a id="configureproject"></a>Add hello RedisSessionStateProvider NuGet package tooyour web app</span></span>
<span data-ttu-id="1268a-111">Installera hello NuGet `RedisSessionStateProvider` paketet.</span><span class="sxs-lookup"><span data-stu-id="1268a-111">Install hello NuGet `RedisSessionStateProvider` package.</span></span>  <span data-ttu-id="1268a-112">Använd hello följande kommando tooinstall från hello package manager-konsolen (**verktyg** > **NuGet Package Manager** > **Pakethanterarkonsolen**):</span><span class="sxs-lookup"><span data-stu-id="1268a-112">Use hello following command tooinstall from hello package manager console (**Tools** > **NuGet Package Manager** > **Package Manager Console**):</span></span>

  `PM> Install-Package Microsoft.Web.RedisSessionStateProvider`

<span data-ttu-id="1268a-113">tooinstall från **verktyg** > **NuGet Package Manager** > **NugGet hantera paket för lösningen**, söka efter `RedisSessionStateProvider`.</span><span class="sxs-lookup"><span data-stu-id="1268a-113">tooinstall from **Tools** > **NuGet Package Manager** > **Manage NugGet Packages for Solution**, search for `RedisSessionStateProvider`.</span></span>

<span data-ttu-id="1268a-114">Mer information finns i hello [NuGet RedisSessionStateProvider-sidan](http://www.nuget.org/packages/Microsoft.Web.RedisSessionStateProvider/) och [konfigurera hello cacheklienten](../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md#NuGet).</span><span class="sxs-lookup"><span data-stu-id="1268a-114">For more information see hello [NuGet RedisSessionStateProvider page](http://www.nuget.org/packages/Microsoft.Web.RedisSessionStateProvider/) and [Configure hello cache client](../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md#NuGet).</span></span>

## <span data-ttu-id="1268a-115"><a id="configurewebconfig"></a>Ändra hello Web.Config-filen</span><span class="sxs-lookup"><span data-stu-id="1268a-115"><a id="configurewebconfig"></a>Modify hello Web.Config File</span></span>
<span data-ttu-id="1268a-116">Dessutom toomaking sammansättningen refererar till för Cache, hello NuGet-paketet lägger till stub-poster i hello *web.config* fil.</span><span class="sxs-lookup"><span data-stu-id="1268a-116">In addition toomaking assembly references for Cache, hello NuGet package adds stub entries in hello *web.config* file.</span></span> 

1. <span data-ttu-id="1268a-117">Öppna hello *web.config* och hitta hello hello **sessionState** element.</span><span class="sxs-lookup"><span data-stu-id="1268a-117">Open hello *web.config* and find hello hello **sessionState** element.</span></span>
2. <span data-ttu-id="1268a-118">Ange hello värden för `host`, `accessKey`, `port` (hello SSL-porten ska vara 6380), och ange `SSL` för`true`.</span><span class="sxs-lookup"><span data-stu-id="1268a-118">Enter hello values for `host`, `accessKey`, `port` (hello SSL port should be 6380), and set `SSL` too`true`.</span></span> <span data-ttu-id="1268a-119">Dessa värden kan hämtas från hello [Azure Portal](http://go.microsoft.com/fwlink/?LinkId=529715) bladet för din cache-instans.</span><span class="sxs-lookup"><span data-stu-id="1268a-119">These values can be obtained from hello [Azure Portal](http://go.microsoft.com/fwlink/?LinkId=529715) blade for your cache instance.</span></span> <span data-ttu-id="1268a-120">Mer information finns i [ansluta toohello cache](../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-cache).</span><span class="sxs-lookup"><span data-stu-id="1268a-120">For more information, see [Connect toohello cache](../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-cache).</span></span> <span data-ttu-id="1268a-121">Observera att hello icke-SSL-porten är inaktiverad som standard för nya cacheminnen.</span><span class="sxs-lookup"><span data-stu-id="1268a-121">Note that hello non-SSL port is disabled by default for new caches.</span></span> <span data-ttu-id="1268a-122">Mer information om hur du aktiverar hello icke-SSL-porten finns hello [Åtkomstportar](https://msdn.microsoft.com/library/azure/dn793612.aspx#AccessPorts) avsnitt i hello [konfigurera en cache i Azure Redis-Cache](https://msdn.microsoft.com/library/azure/dn793612.aspx) avsnittet.</span><span class="sxs-lookup"><span data-stu-id="1268a-122">For more information about enabling hello non-SSL port, see hello [Access Ports](https://msdn.microsoft.com/library/azure/dn793612.aspx#AccessPorts) section in hello [Configure a cache in Azure Redis Cache](https://msdn.microsoft.com/library/azure/dn793612.aspx) topic.</span></span> <span data-ttu-id="1268a-123">hello följande markering visar hello ändringar toohello *web.config* -fil, särskilt hello ändringar för*port*, *värden*, accessKey * och *ssl*.</span><span class="sxs-lookup"><span data-stu-id="1268a-123">hello following markup shows hello changes toohello *web.config* file, specifically hello changes too*port*, *host*, accessKey*, and *ssl*.</span></span>
   
          <system.web>;
            <customErrors mode="Off" />;
            <authentication mode="None" />;
            <compilation debug="true" targetFramework="4.5" />;
            <httpRuntime targetFramework="4.5" />;
            <sessionState mode="Custom" customProvider="RedisSessionProvider">;
              <providers>;  
                  <!--<add name="RedisSessionProvider" 
                    host = "127.0.0.1" [String]
                    port = "" [number]
                    accessKey = "" [String]
                    ssl = "false" [true|false]
                    throwOnError = "true" [true|false]
                    retryTimeoutInMilliseconds = "0" [number]
                    databaseId = "0" [number]
                    applicationName = "" [String]
                  />;-->;
                 <add name="RedisSessionProvider" 
                      type="Microsoft.Web.Redis.RedisSessionStateProvider" 
                      port="6380"
                      host="movie2.redis.cache.windows.net" 
                      accessKey="m7PNV60CrvKpLqMUxosC3dSe6kx9nQ6jP5del8TmADk=" 
                      ssl="true" />;
              <!--<add name="MySessionStateStore" type="Microsoft.Web.Redis.RedisSessionStateProvider" host="127.0.0.1" accessKey="" ssl="false" />;-->;
              </providers>;
            </sessionState>;
          </system.web>;

## <span data-ttu-id="1268a-124"><a id="usesessionobject"></a>Använda hello sessionsobjektet i kod</span><span class="sxs-lookup"><span data-stu-id="1268a-124"><a id="usesessionobject"></a> Use hello Session Object in Code</span></span>
<span data-ttu-id="1268a-125">hello sista steget är toobegin med hello sessionsobjektet i ASP.NET-koden.</span><span class="sxs-lookup"><span data-stu-id="1268a-125">hello final step is toobegin using hello Session object in your ASP.NET code.</span></span> <span data-ttu-id="1268a-126">Du lägger till objekt toosession tillstånd med hello **Session.Add** metod.</span><span class="sxs-lookup"><span data-stu-id="1268a-126">You add objects toosession state by using hello **Session.Add** method.</span></span> <span data-ttu-id="1268a-127">Den här metoden används nyckel / värde-par toostore objekt i sessionstillståndscachen hello.</span><span class="sxs-lookup"><span data-stu-id="1268a-127">This method uses key-value pairs toostore items in hello session state cache.</span></span>

    string strValue = "yourvalue";
    Session.Add("yourkey", strValue);

<span data-ttu-id="1268a-128">hello följande kod hämtar värdet från sessionstillstånd.</span><span class="sxs-lookup"><span data-stu-id="1268a-128">hello following code retrieves this value from session state.</span></span>

    object objValue = Session["yourkey"];
    if (objValue != null)
       strValue = (string)objValue;    

<span data-ttu-id="1268a-129">Du kan också använda hello Redis-Cache toocache objekt i ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="1268a-129">You can also use hello Redis Cache toocache objects in your web app.</span></span> <span data-ttu-id="1268a-130">Mer information finns i [MVC-filmapp med Azure Redis-cache på 15 minuter](https://azure.microsoft.com/blog/2014/06/05/mvc-movie-app-with-azure-redis-cache-in-15-minutes/).</span><span class="sxs-lookup"><span data-stu-id="1268a-130">For more info, see [MVC movie app with Azure Redis Cache in 15 minutes](https://azure.microsoft.com/blog/2014/06/05/mvc-movie-app-with-azure-redis-cache-in-15-minutes/).</span></span>
<span data-ttu-id="1268a-131">Mer information om hur toouse ASP.NET session state finns [översikt över sessionstillståndet ASP.NET][ASP.NET Session State Overview].</span><span class="sxs-lookup"><span data-stu-id="1268a-131">For more details about how toouse ASP.NET session state, see [ASP.NET Session State Overview][ASP.NET Session State Overview].</span></span>

> [!NOTE]
> <span data-ttu-id="1268a-132">Om du vill tooget igång med Azure App Service innan du registrerar dig för ett Azure-konto går för[prova App Service](https://azure.microsoft.com/try/app-service/), där kan du direkt skapa en tillfällig startwebbapp i App Service.</span><span class="sxs-lookup"><span data-stu-id="1268a-132">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="1268a-133">Inget kreditkort krävs, och du gör inga åtaganden.</span><span class="sxs-lookup"><span data-stu-id="1268a-133">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="whats-changed"></a><span data-ttu-id="1268a-134">Nyheter</span><span class="sxs-lookup"><span data-stu-id="1268a-134">What's changed</span></span>
* <span data-ttu-id="1268a-135">En guide toohello övergången från webbplatser tooApp tjänsten finns: [Azure App Service och dess påverkan på befintliga Azure-tjänster](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="1268a-135">For a guide toohello change from Websites tooApp Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>
  
  <span data-ttu-id="1268a-136">*Av [Rick Anderson](https://twitter.com/RickAndMSFT)*</span><span class="sxs-lookup"><span data-stu-id="1268a-136">*By [Rick Anderson](https://twitter.com/RickAndMSFT)*</span></span>

[installed hello latest]: http://www.windowsazure.com/downloads/?sdk=net  
[ASP.NET Session State Overview]: http://msdn.microsoft.com/library/ms178581.aspx

[NewIcon]: ./media/web-sites-dotnet-session-state-caching/CacheScreenshot_NewButton.png
[NewCacheDialog]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_CreateOptions.png
[CacheIcon]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_CacheIcon.png
[NuGetDialog]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_NuGet.png
[OutputConfig]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_OC_WebConfig.png
[CacheConfig]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_CacheConfig.png
[EndpointURL]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_EndpointURL.png
[ManageKeys]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_ManageAccessKeys.png

