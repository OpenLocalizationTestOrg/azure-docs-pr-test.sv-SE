---
title: "Sessionstillstånd med Azure Redis-cache i Azure App Service"
description: "Lär dig hur du använder Azure Cache-tjänsten till att hantera cachelagring av ASP.NET-sessionstillstånd."
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
ms.openlocfilehash: 64fa909daf92b2b1f0cf4c7b334edba807fe7228
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="session-state-with-azure-redis-cache-in-azure-app-service"></a><span data-ttu-id="ac9f7-103">Sessionstillstånd med Azure Redis-cache i Azure App Service</span><span class="sxs-lookup"><span data-stu-id="ac9f7-103">Session state with Azure Redis cache in Azure App Service</span></span>
<span data-ttu-id="ac9f7-104">I det här avsnittet får du veta hur du använder tjänsten Azure Redis-cache för sessionstillstånd.</span><span class="sxs-lookup"><span data-stu-id="ac9f7-104">This topic explains how to use the Azure Redis Cache Service for session state.</span></span>

<span data-ttu-id="ac9f7-105">Om din ASP.NET-webbapp använder sessionstillstånd måste du konfigurera en extern sessionstillståndsprovider (tjänsten Redis-cache eller en sessionstillståndsprovider för SQL Server).</span><span class="sxs-lookup"><span data-stu-id="ac9f7-105">If your ASP.NET web app uses session state, you will need to configure an external session state provider (either the Redis Cache Service or a SQL Server session state provider).</span></span> <span data-ttu-id="ac9f7-106">Om du använder sessionstillstånd och inte använder en extern provider begränsas du till en instans av webbappen.</span><span class="sxs-lookup"><span data-stu-id="ac9f7-106">If you use session state, and don't use an external provider, you will be limited to one instance of your web app.</span></span> <span data-ttu-id="ac9f7-107">Tjänsten Redis-cache är den som det går snabbast och enklast att aktivera.</span><span class="sxs-lookup"><span data-stu-id="ac9f7-107">The Redis Cache Service is the fastest and simplest to enable.</span></span>

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <span data-ttu-id="ac9f7-108"><a id="createcache"></a>Skapa cachen</span><span class="sxs-lookup"><span data-stu-id="ac9f7-108"><a id="createcache"></a>Create the Cache</span></span>
<span data-ttu-id="ac9f7-109">Skapa cachen genom att följa [de här anvisningarna](../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md#create-cache).</span><span class="sxs-lookup"><span data-stu-id="ac9f7-109">Follow [these directions](../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md#create-cache) to create the cache.</span></span>

## <span data-ttu-id="ac9f7-110"><a id="configureproject"></a>Lägga till NuGet-paketet RedisSessionStateProvider till webbappen</span><span class="sxs-lookup"><span data-stu-id="ac9f7-110"><a id="configureproject"></a>Add the RedisSessionStateProvider NuGet package to your web app</span></span>
<span data-ttu-id="ac9f7-111">Installera NuGet-paketet `RedisSessionStateProvider`.</span><span class="sxs-lookup"><span data-stu-id="ac9f7-111">Install the NuGet `RedisSessionStateProvider` package.</span></span>  <span data-ttu-id="ac9f7-112">Installera från pakethanterarkonsolen med hjälp av följande kommando (**Tools** > **NuGet Package Manager** > **Package Manager Console**) (Verktyg, NuGet-pakethanteraren, Pakethanterarkonsol):</span><span class="sxs-lookup"><span data-stu-id="ac9f7-112">Use the following command to install from the package manager console (**Tools** > **NuGet Package Manager** > **Package Manager Console**):</span></span>

  `PM> Install-Package Microsoft.Web.RedisSessionStateProvider`

<span data-ttu-id="ac9f7-113">Om du vill installera från **Tools** > **NuGet Package Manager** > **Manage NugGet Packages for Solution** (Verktyg, NuGet-pakethanteraren, Hantera NuGet-paket för lösning) söker du efter `RedisSessionStateProvider`.</span><span class="sxs-lookup"><span data-stu-id="ac9f7-113">To install from **Tools** > **NuGet Package Manager** > **Manage NugGet Packages for Solution**, search for `RedisSessionStateProvider`.</span></span>

<span data-ttu-id="ac9f7-114">Mer information finns i [NuGet RedisSessionStateProvider-sidan](http://www.nuget.org/packages/Microsoft.Web.RedisSessionStateProvider/) och [Konfigurera cacheklienten](../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md#NuGet).</span><span class="sxs-lookup"><span data-stu-id="ac9f7-114">For more information see the [NuGet RedisSessionStateProvider page](http://www.nuget.org/packages/Microsoft.Web.RedisSessionStateProvider/) and [Configure the cache client](../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md#NuGet).</span></span>

## <span data-ttu-id="ac9f7-115"><a id="configurewebconfig"></a>Ändra filen web.config</span><span class="sxs-lookup"><span data-stu-id="ac9f7-115"><a id="configurewebconfig"></a>Modify the Web.Config File</span></span>
<span data-ttu-id="ac9f7-116">Med NuGet-paketet skapas inte bara sammansättningsreferenser för cachen, med det läggs också stub-poster till i filen *web.config*.</span><span class="sxs-lookup"><span data-stu-id="ac9f7-116">In addition to making assembly references for Cache, the NuGet package adds stub entries in the *web.config* file.</span></span> 

1. <span data-ttu-id="ac9f7-117">Öppna *web.config* och leta reda på elementet **sessionState**.</span><span class="sxs-lookup"><span data-stu-id="ac9f7-117">Open the *web.config* and find the the **sessionState** element.</span></span>
2. <span data-ttu-id="ac9f7-118">Ange värden för `host`, `accessKey` och `port` (SSL-porten ska vara 6380), och ange `SSL` till `true`.</span><span class="sxs-lookup"><span data-stu-id="ac9f7-118">Enter the values for `host`, `accessKey`, `port` (the SSL port should be 6380), and set `SSL` to `true`.</span></span> <span data-ttu-id="ac9f7-119">Du kan hämta de här värdena från bladet för [Azure Portal](http://go.microsoft.com/fwlink/?LinkId=529715) för cacheinstansen.</span><span class="sxs-lookup"><span data-stu-id="ac9f7-119">These values can be obtained from the [Azure Portal](http://go.microsoft.com/fwlink/?LinkId=529715) blade for your cache instance.</span></span> <span data-ttu-id="ac9f7-120">Mer information finns i [Ansluta till cacheminnet](../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-cache).</span><span class="sxs-lookup"><span data-stu-id="ac9f7-120">For more information, see [Connect to the cache](../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-cache).</span></span> <span data-ttu-id="ac9f7-121">Observera att icke-SSL-porten är inaktiverad som standard för nya cacheminnen.</span><span class="sxs-lookup"><span data-stu-id="ac9f7-121">Note that the non-SSL port is disabled by default for new caches.</span></span> <span data-ttu-id="ac9f7-122">Mer information om hur du aktiverar icke-SSL-porten finns i avsnittet [Åtkomstportar](https://msdn.microsoft.com/library/azure/dn793612.aspx#AccessPorts) i artikeln [Konfigurera en cache i Azure Redis-cache](https://msdn.microsoft.com/library/azure/dn793612.aspx).</span><span class="sxs-lookup"><span data-stu-id="ac9f7-122">For more information about enabling the non-SSL port, see the [Access Ports](https://msdn.microsoft.com/library/azure/dn793612.aspx#AccessPorts) section in the [Configure a cache in Azure Redis Cache](https://msdn.microsoft.com/library/azure/dn793612.aspx) topic.</span></span> <span data-ttu-id="ac9f7-123">Följande markering visar ändringarna i den *web.config* -fil, särskilt ändringarna av *port*, *värden*, accessKey * och *ssl* .</span><span class="sxs-lookup"><span data-stu-id="ac9f7-123">The following markup shows the changes to the *web.config* file, specifically the changes to *port*, *host*, accessKey*, and *ssl*.</span></span>
   
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

## <span data-ttu-id="ac9f7-124"><a id="usesessionobject"></a>Använda sessionsobjektet i kod</span><span class="sxs-lookup"><span data-stu-id="ac9f7-124"><a id="usesessionobject"></a> Use the Session Object in Code</span></span>
<span data-ttu-id="ac9f7-125">Det sista steget är att börja använda sessionsobjektet i ASP.NET-koden.</span><span class="sxs-lookup"><span data-stu-id="ac9f7-125">The final step is to begin using the Session object in your ASP.NET code.</span></span> <span data-ttu-id="ac9f7-126">Du lägger till objekt till sessionstillstånd med hjälp av metoden **Session.Add**.</span><span class="sxs-lookup"><span data-stu-id="ac9f7-126">You add objects to session state by using the **Session.Add** method.</span></span> <span data-ttu-id="ac9f7-127">I den här metoden används nyckel/värde-par till att lagra objekt i sessionstillståndscachen.</span><span class="sxs-lookup"><span data-stu-id="ac9f7-127">This method uses key-value pairs to store items in the session state cache.</span></span>

    string strValue = "yourvalue";
    Session.Add("yourkey", strValue);

<span data-ttu-id="ac9f7-128">Följande kod hämtar värdet från sessionstillstånd.</span><span class="sxs-lookup"><span data-stu-id="ac9f7-128">The following code retrieves this value from session state.</span></span>

    object objValue = Session["yourkey"];
    if (objValue != null)
       strValue = (string)objValue;    

<span data-ttu-id="ac9f7-129">Du kan också använda Redis-cache för cacheobjekt i webbappen.</span><span class="sxs-lookup"><span data-stu-id="ac9f7-129">You can also use the Redis Cache to cache objects in your web app.</span></span> <span data-ttu-id="ac9f7-130">Mer information finns i [MVC-filmapp med Azure Redis-cache på 15 minuter](https://azure.microsoft.com/blog/2014/06/05/mvc-movie-app-with-azure-redis-cache-in-15-minutes/).</span><span class="sxs-lookup"><span data-stu-id="ac9f7-130">For more info, see [MVC movie app with Azure Redis Cache in 15 minutes](https://azure.microsoft.com/blog/2014/06/05/mvc-movie-app-with-azure-redis-cache-in-15-minutes/).</span></span>
<span data-ttu-id="ac9f7-131">Mer information om hur du använder ASP.NET-sessionstillståndet finns i [ASP.NET Session State Overview][ASP.NET Session State Overview] (Översikt över ASP.NET-sessionstillståndet).</span><span class="sxs-lookup"><span data-stu-id="ac9f7-131">For more details about how to use ASP.NET session state, see [ASP.NET Session State Overview][ASP.NET Session State Overview].</span></span>

> [!NOTE]
> <span data-ttu-id="ac9f7-132">Om du vill komma igång med Azure Apptjänst innan du registrerar dig för ett Azure-konto kan du gå till [Prova Apptjänst](https://azure.microsoft.com/try/app-service/). Där kan du direkt skapa en tillfällig startwebbapp i Apptjänst.</span><span class="sxs-lookup"><span data-stu-id="ac9f7-132">If you want to get started with Azure App Service before signing up for an Azure account, go to [Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="ac9f7-133">Inget kreditkort krävs, och du gör inga åtaganden.</span><span class="sxs-lookup"><span data-stu-id="ac9f7-133">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="whats-changed"></a><span data-ttu-id="ac9f7-134">Nyheter</span><span class="sxs-lookup"><span data-stu-id="ac9f7-134">What's changed</span></span>
* <span data-ttu-id="ac9f7-135">En guide till övergången från Webbplatser till App Service finns i: [Azure App Service och dess påverkan på befintliga Azure-tjänster](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="ac9f7-135">For a guide to the change from Websites to App Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>
  
  <span data-ttu-id="ac9f7-136">*Av [Rick Anderson](https://twitter.com/RickAndMSFT)*</span><span class="sxs-lookup"><span data-stu-id="ac9f7-136">*By [Rick Anderson](https://twitter.com/RickAndMSFT)*</span></span>

[installed the latest]: http://www.windowsazure.com/downloads/?sdk=net  
[ASP.NET Session State Overview]: http://msdn.microsoft.com/library/ms178581.aspx

[NewIcon]: ./media/web-sites-dotnet-session-state-caching/CacheScreenshot_NewButton.png
[NewCacheDialog]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_CreateOptions.png
[CacheIcon]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_CacheIcon.png
[NuGetDialog]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_NuGet.png
[OutputConfig]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_OC_WebConfig.png
[CacheConfig]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_CacheConfig.png
[EndpointURL]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_EndpointURL.png
[ManageKeys]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_ManageAccessKeys.png

