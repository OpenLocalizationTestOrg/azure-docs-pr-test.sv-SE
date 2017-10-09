<span data-ttu-id="ede64-101">.NET-program kan använda hello **StackExchange.Redis** cacheklienten, som kan konfigureras i Visual Studio med hjälp av en NuGet-paketet som förenklar hello konfigurationen av cache-klientprogram.</span><span class="sxs-lookup"><span data-stu-id="ede64-101">.NET applications can use hello **StackExchange.Redis** cache client, which can be configured in Visual Studio using a NuGet package that simplifies hello configuration of cache client applications.</span></span> 

> [!NOTE]
> <span data-ttu-id="ede64-102">Mer information finns i hello [StackExchange.Redis](http://github.com/StackExchange/StackExchange.Redis) github sida och hello [StackExchange.Redis cache klienten dokumentationen](http://github.com/StackExchange/StackExchange.Redis#documentation).</span><span class="sxs-lookup"><span data-stu-id="ede64-102">For more information, see hello [StackExchange.Redis](http://github.com/StackExchange/StackExchange.Redis) github page and  hello [StackExchange.Redis cache client documentation](http://github.com/StackExchange/StackExchange.Redis#documentation).</span></span>
> 
> 

<span data-ttu-id="ede64-103">tooconfigure ett klientprogram i Visual Studio med hello StackExchange.Redis NuGet-paketet, högerklicka på hello-projekt i **Solution Explorer** och välj **hantera NuGet-paket**.</span><span class="sxs-lookup"><span data-stu-id="ede64-103">tooconfigure a client application in Visual Studio using hello StackExchange.Redis NuGet package, right-click hello project in **Solution Explorer** and choose **Manage NuGet Packages**.</span></span> 

![Hantera NuGet-paket](media/redis-cache-configure-stackexchange-redis-nuget/redis-cache-manage-nuget-menu.png)

<span data-ttu-id="ede64-105">Typen **StackExchange.Redis** eller **StackExchange.Redis.StrongName** Välj hello önskade versionen hello resultat till hello sökrutan och klicka på **installera**.</span><span class="sxs-lookup"><span data-stu-id="ede64-105">Type **StackExchange.Redis** or **StackExchange.Redis.StrongName** into hello search text box, select hello desired version from hello results, and click **Install**.</span></span>

> [!NOTE]
> <span data-ttu-id="ede64-106">Om du föredrar toouse ett starkt krypterat namn version av hello **StackExchange.Redis** klientbiblioteket, Välj **StackExchange.Redis.StrongName**; annars väljer **StackExchange.Redis**.</span><span class="sxs-lookup"><span data-stu-id="ede64-106">If you prefer toouse a strong-named version of hello **StackExchange.Redis** client library, choose **StackExchange.Redis.StrongName**; otherwise choose **StackExchange.Redis**.</span></span>
> 
> 

![NuGet-paket för StackExchange.Redis](media/redis-cache-configure-stackexchange-redis-nuget/redis-cache-stackexchange-redis.png)

<span data-ttu-id="ede64-108">Hej NuGet paket hämtar och lägger till hello krävs sammansättningsreferenser för din klient programmet tooaccess Azure Redis-Cache med hello StackExchange.Redis cache-klienten.</span><span class="sxs-lookup"><span data-stu-id="ede64-108">hello NuGet package downloads and adds hello required assembly references for your client application tooaccess Azure Redis Cache with hello StackExchange.Redis cache client.</span></span>

> [!NOTE]
> <span data-ttu-id="ede64-109">Om du tidigare har konfigurerat dina projekt toouse StackExchange.Redis du kan söka efter uppdateringar toohello paket från hello **NuGet Package Manager**.</span><span class="sxs-lookup"><span data-stu-id="ede64-109">If you have previously configured your project toouse StackExchange.Redis, you can check for updates toohello package from hello **NuGet Package Manager**.</span></span> <span data-ttu-id="ede64-110">Klicka på toocheck för och installera uppdaterade versioner av hello StackExchange.Redis NuGet-paketet **uppdateringar** i hello hello **NuGet Package Manager** fönster.</span><span class="sxs-lookup"><span data-stu-id="ede64-110">toocheck for and install updated versions of hello StackExchange.Redis NuGet package, click **Updates** in hello hello **NuGet Package Manager** window.</span></span> <span data-ttu-id="ede64-111">Om en uppdatering toohello StackExchange.Redis NuGet-paketet är tillgänglig, kan du uppdatera din project toouse hello uppdaterad version.</span><span class="sxs-lookup"><span data-stu-id="ede64-111">If an update toohello StackExchange.Redis NuGet package is available, you can update your project toouse hello updated version.</span></span>
> 
> 

<span data-ttu-id="ede64-112">Du kan också installera hello StackExchange.Redis NuGet-paketet genom att klicka på **NuGet Package Manager**, **Pakethanterarkonsolen** från hello **verktyg** -menyn och körs hello följande kommando från hello **Pakethanterarkonsolen** fönster.</span><span class="sxs-lookup"><span data-stu-id="ede64-112">You can also install hello StackExchange.Redis NuGet package by clicking **NuGet Package Manager**, **Package Manager Console** from hello **Tools** menu, and running hello following command from hello **Package Manager Console** window.</span></span>
    
```
Install-Package StackExchange.Redis
```
