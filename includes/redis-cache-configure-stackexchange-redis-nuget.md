.NET-program kan använda hello **StackExchange.Redis** cacheklienten, som kan konfigureras i Visual Studio med hjälp av en NuGet-paketet som förenklar hello konfigurationen av cache-klientprogram. 

> [!NOTE]
> Mer information finns i hello [StackExchange.Redis](http://github.com/StackExchange/StackExchange.Redis) github sida och hello [StackExchange.Redis cache klienten dokumentationen](http://github.com/StackExchange/StackExchange.Redis#documentation).
> 
> 

tooconfigure ett klientprogram i Visual Studio med hello StackExchange.Redis NuGet-paketet, högerklicka på hello-projekt i **Solution Explorer** och välj **hantera NuGet-paket**. 

![Hantera NuGet-paket](media/redis-cache-configure-stackexchange-redis-nuget/redis-cache-manage-nuget-menu.png)

Typen **StackExchange.Redis** eller **StackExchange.Redis.StrongName** Välj hello önskade versionen hello resultat till hello sökrutan och klicka på **installera**.

> [!NOTE]
> Om du föredrar toouse ett starkt krypterat namn version av hello **StackExchange.Redis** klientbiblioteket, Välj **StackExchange.Redis.StrongName**; annars väljer **StackExchange.Redis**.
> 
> 

![NuGet-paket för StackExchange.Redis](media/redis-cache-configure-stackexchange-redis-nuget/redis-cache-stackexchange-redis.png)

Hej NuGet paket hämtar och lägger till hello krävs sammansättningsreferenser för din klient programmet tooaccess Azure Redis-Cache med hello StackExchange.Redis cache-klienten.

> [!NOTE]
> Om du tidigare har konfigurerat dina projekt toouse StackExchange.Redis du kan söka efter uppdateringar toohello paket från hello **NuGet Package Manager**. Klicka på toocheck för och installera uppdaterade versioner av hello StackExchange.Redis NuGet-paketet **uppdateringar** i hello hello **NuGet Package Manager** fönster. Om en uppdatering toohello StackExchange.Redis NuGet-paketet är tillgänglig, kan du uppdatera din project toouse hello uppdaterad version.
> 
> 

Du kan också installera hello StackExchange.Redis NuGet-paketet genom att klicka på **NuGet Package Manager**, **Pakethanterarkonsolen** från hello **verktyg** -menyn och körs hello följande kommando från hello **Pakethanterarkonsolen** fönster.
    
```
Install-Package StackExchange.Redis
```
