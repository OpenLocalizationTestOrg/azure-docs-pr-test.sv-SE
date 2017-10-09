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
# <a name="session-state-with-azure-redis-cache-in-azure-app-service"></a>Sessionstillstånd med Azure Redis-cache i Azure App Service
Det här avsnittet beskrivs hur toouse hello Azure Redis-Cache-tjänst för sessionstillstånd.

Om din ASP.NET-webbapp använder sessionstillstånd, behöver du tooconfigure en extern sessionstillståndsprovider (hello Redis Cache-tjänst eller en sessionstillståndsprovider för SQL Server). Om du använder sessionstillstånd och inte använder en extern leverantör, kommer du att begränsad tooone instans av webbappen. Hej Redis Cache-tjänst är hello snabbast och enklast tooenable.

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a id="createcache"></a>Skapa hello Cache
Följ [de här anvisningarna](../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md#create-cache) toocreate hello cache.

## <a id="configureproject"></a>Lägg till hello RedisSessionStateProvider NuGet-paketet tooyour webbprogram
Installera hello NuGet `RedisSessionStateProvider` paketet.  Använd hello följande kommando tooinstall från hello package manager-konsolen (**verktyg** > **NuGet Package Manager** > **Pakethanterarkonsolen**):

  `PM> Install-Package Microsoft.Web.RedisSessionStateProvider`

tooinstall från **verktyg** > **NuGet Package Manager** > **NugGet hantera paket för lösningen**, söka efter `RedisSessionStateProvider`.

Mer information finns i hello [NuGet RedisSessionStateProvider-sidan](http://www.nuget.org/packages/Microsoft.Web.RedisSessionStateProvider/) och [konfigurera hello cacheklienten](../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md#NuGet).

## <a id="configurewebconfig"></a>Ändra hello Web.Config-filen
Dessutom toomaking sammansättningen refererar till för Cache, hello NuGet-paketet lägger till stub-poster i hello *web.config* fil. 

1. Öppna hello *web.config* och hitta hello hello **sessionState** element.
2. Ange hello värden för `host`, `accessKey`, `port` (hello SSL-porten ska vara 6380), och ange `SSL` för`true`. Dessa värden kan hämtas från hello [Azure Portal](http://go.microsoft.com/fwlink/?LinkId=529715) bladet för din cache-instans. Mer information finns i [ansluta toohello cache](../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-cache). Observera att hello icke-SSL-porten är inaktiverad som standard för nya cacheminnen. Mer information om hur du aktiverar hello icke-SSL-porten finns hello [Åtkomstportar](https://msdn.microsoft.com/library/azure/dn793612.aspx#AccessPorts) avsnitt i hello [konfigurera en cache i Azure Redis-Cache](https://msdn.microsoft.com/library/azure/dn793612.aspx) avsnittet. hello följande markering visar hello ändringar toohello *web.config* -fil, särskilt hello ändringar för*port*, *värden*, accessKey * och *ssl*.
   
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

## <a id="usesessionobject"></a>Använda hello sessionsobjektet i kod
hello sista steget är toobegin med hello sessionsobjektet i ASP.NET-koden. Du lägger till objekt toosession tillstånd med hello **Session.Add** metod. Den här metoden används nyckel / värde-par toostore objekt i sessionstillståndscachen hello.

    string strValue = "yourvalue";
    Session.Add("yourkey", strValue);

hello följande kod hämtar värdet från sessionstillstånd.

    object objValue = Session["yourkey"];
    if (objValue != null)
       strValue = (string)objValue;    

Du kan också använda hello Redis-Cache toocache objekt i ditt webbprogram. Mer information finns i [MVC-filmapp med Azure Redis-cache på 15 minuter](https://azure.microsoft.com/blog/2014/06/05/mvc-movie-app-with-azure-redis-cache-in-15-minutes/).
Mer information om hur toouse ASP.NET session state finns [översikt över sessionstillståndet ASP.NET][ASP.NET Session State Overview].

> [!NOTE]
> Om du vill tooget igång med Azure App Service innan du registrerar dig för ett Azure-konto går för[prova App Service](https://azure.microsoft.com/try/app-service/), där kan du direkt skapa en tillfällig startwebbapp i App Service. Inget kreditkort krävs, och du gör inga åtaganden.
> 
> 

## <a name="whats-changed"></a>Nyheter
* En guide toohello övergången från webbplatser tooApp tjänsten finns: [Azure App Service och dess påverkan på befintliga Azure-tjänster](http://go.microsoft.com/fwlink/?LinkId=529714)
  
  *Av [Rick Anderson](https://twitter.com/RickAndMSFT)*

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

