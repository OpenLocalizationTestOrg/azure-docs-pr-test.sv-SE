toocreate en cache först logga in toohello [Azure-portalen](https://portal.azure.com), och klicka på **ny** > **databaser** > **Redis-Cache**.

> [!NOTE]
> Om du inte har något Azure-konto så kan du [registrera ett kostnadsfritt Azure-konto](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=redis_cache_hero) på bara några minuter.
> 
> 

![Nytt cacheminne](media/redis-cache-create/redis-cache-new-cache-menu.png)

> [!NOTE]
> Dessutom toocreating cachelagrar i hello Azure-portalen kan du också skapa dem med hjälp av Resource Manager-mallar, PowerShell eller Azure CLI.
> 
> * toocreate en cache med Resource Manager-mallar finns [skapa ett Redis-cache med hjälp av en mall](../articles/redis-cache/cache-redis-cache-arm-provision.md).
> * toocreate en cache med Azure PowerShell finns [hantera Azure Redis-Cache med Azure PowerShell](../articles/redis-cache/cache-howto-manage-redis-cache-powershell.md).
> * toocreate en cache med Azure CLI, se [hur toocreate och hantera Azure Redis-Cache med hjälp av hello Azure-kommandoradsgränssnittet (Azure CLI)](../articles/redis-cache/cache-manage-cli.md).
> 
> 

I hello **nytt Redis-Cache** bladet anger hello önskad konfiguration för hello-cachen.

![Skapa ett cacheminne](media/redis-cache-create/redis-cache-cache-create.png) 

* I **Dns-namnet**, ange ett unikt cache namnet toouse för hello cache-slutpunkten. Hej cachenamnet måste vara en sträng mellan 1 och 63 tecken och innehålla endast siffror, bokstäver och hello `-` tecken. Hej cachenamnet får inte inledas eller avslutas med hello `-` tecken och efterföljande `-` tecken är inte giltiga.
* För **prenumeration**, Välj hello Azure-prenumeration som du vill toouse för hello-cachen. Om ditt konto bara har en prenumeration, den väljs automatiskt och hello **prenumeration** listrutan visas inte.
* I **Resursgrupp** väljer eller skapar du en resursgrupp för ditt cacheminne. Mer information finns i [med resursgrupper toomanage resurserna i Azure](../articles/azure-resource-manager/resource-group-overview.md). 
* Använd **plats** toospecify hello geografisk plats som är värd för ditt cacheminne. För hello bästa prestanda rekommenderar Microsoft att du skapar hello-cache i hello samma region som cacheklientprogrammet hello.
* Använd **prisnivå** tooselect hello önskad cachestorlek och funktioner.
* **Redis-kluster** kan du toocreate cacheminnen större än 53 GB och tooshard data över flera Redis-noder. Mer information finns i [hur tooconfigure klustring för Premium Azure Redis-Cache](../articles/redis-cache/cache-how-to-premium-clustering.md).
* **Redis-persistence** erbjuder hello möjlighet toopersist din cache tooan Azure Storage-konto. Anvisningar om hur du konfigurerar persistence finns [hur tooconfigure persistence för Premium Azure Redis-Cache](../articles/redis-cache/cache-how-to-premium-persistence.md).
* **Virtuellt nätverk** ger förbättrad säkerhet och isolering genom att begränsa åtkomst tooyour cache tooonly dessa klienter inom hello angetts Azure Virtual Network. Du kan använda alla hello funktioner för virtuella nätverk som undernät, principer för åtkomstkontroll och andra funktioner toofurther begränsa åtkomst tooRedis. Mer information finns i [hur tooconfigure virtuella nätverket har stöd för Premium Azure Redis-Cache](../articles/redis-cache/cache-how-to-premium-vnet.md).
* Som standard är icke-SSL-åtkomst inaktiverad för nya cacheminnen. tooenable hello icke-SSL-porten, kontrollera **avblockera port 6379 (inte SSL-krypterade)**.

När hello nya cachealternativen har konfigurerats, klickar du på **skapa**. Det kan ta några minuter för hello cache toobe skapas. Du kan övervaka hello förloppet på startsidan hello toocheck hello status. När hello cachen har skapats, ditt nya cacheminne har en **kör** status och är redo för användning med [standardinställningar](../articles/redis-cache/cache-configure.md#default-redis-server-configuration).

![Cachen har skapats](media/redis-cache-create/redis-cache-cache-created.png)

