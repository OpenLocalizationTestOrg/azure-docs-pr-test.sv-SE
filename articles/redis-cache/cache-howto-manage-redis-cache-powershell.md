---
title: aaaManage Azure Redis-Cache med Azure PowerShell | Microsoft Docs
description: "Lär dig hur tooperform administrativa uppgifter för Azure Redis-Cache med hjälp av Azure PowerShell."
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: 1136efe5-1e33-4d91-bb49-c8e2a6dca475
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: sdanie
ms.openlocfilehash: 1d526ce65c4bc05345cd6c3ff370211ed562cab4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-redis-cache-with-azure-powershell"></a>Hantera Azure Redis-Cache med Azure PowerShell
> [!div class="op_single_selector"]
> * [PowerShell](cache-howto-manage-redis-cache-powershell.md)
> * [Azure CLI](cache-manage-cli.md)
> 
> 

Det här avsnittet visar hur tooperform vanliga uppgifter som att skapa, uppdatera och skala dina Azure Redis-Cache-instanser hur tooregenerate snabbtangenter och hur tooview information om dina cacheminnen. En fullständig lista över Azure Redis-Cache PowerShell-cmdlets finns i [cmdlets för Azure Redis-Cache](https://msdn.microsoft.com/library/azure/mt634513.aspx).

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]

Läs mer om hello klassiska distributionsmodellen [Azure Resource Manager och klassisk distribution: Förstå distributionsmodeller och hello för dina resurser](../azure-resource-manager/resource-manager-deployment-model.md#classic-deployment-characteristics).

## <a name="prerequisites"></a>Krav
Om du redan har installerat Azure PowerShell, måste du ha Azure PowerShell version 1.0.0 eller senare. Du kan kontrollera hello-versionen av Azure PowerShell som du har installerat med detta kommando i Kommandotolken för hello Azure PowerShell.

    Get-Module azure | format-table version


Först måste du logga in tooAzure med det här kommandot.

    Login-AzureRmAccount

Ange hello e-postadressen för ditt Azure-konto och lösenordet i dialogrutan för hello Microsoft Azure-inloggning.

Sedan om du har flera Azure-prenumerationer måste tooset din Azure-prenumeration. toosee en lista med din aktuella prenumeration, kör kommandot.

    Get-AzureRmSubscription | sort SubscriptionName | Select SubscriptionName

toospecify hello prenumeration, kör följande kommando hello. I följande exempel hello, hello prenumerationsnamn är `ContosoSubscription`.

    Select-AzureRmSubscription -SubscriptionName ContosoSubscription

Innan du kan använda Windows PowerShell med Azure Resource Manager, måste du hello följande:

* Windows PowerShell Version 3.0 eller 4.0. toofind hello version av Windows PowerShell, skriv:`$PSVersionTable` och kontrollera hello värdet för `PSVersion` 3.0 eller 4.0. tooinstall en kompatibel version finns [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) eller [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855).

tooget utförlig information för alla cmdletar som du ser i den här kursen används hello Get-Help cmdleten.

    Get-Help <cmdlet-name> -Detailed

Till exempel tooget hjälp för hello `New-AzureRmRedisCache` cmdlet, skriv:

    Get-Help New-AzureRmRedisCache -Detailed

### <a name="how-tooconnect-tooother-clouds"></a>Hur tooconnect tooother moln
Miljön är som standard hello Azure `AzureCloud`, som representerar hello globala Azure-molnet instans. tooconnect tooa annan instans, Använd hello `Add-AzureRmAccount` med hello `-Environment` eller -`EnvironmentName` kommandoradsväxeln med hello önskad miljö eller namn.

toosee hello lista över tillgängliga miljöer, kör hello `Get-AzureRmEnvironment` cmdlet.

### <a name="tooconnect-toohello-azure-government-cloud"></a>tooconnect toohello Azure Government-molnet
tooconnect toohello Azure offentliga moln, med någon av följande kommandon hello.

    Add-AzureRMAccount -EnvironmentName AzureUSGovernment

eller

    Add-AzureRmAccount -Environment (Get-AzureRmEnvironment -Name AzureUSGovernment)

toocreate en cachelagring i hello Azure offentliga moln, med någon av följande platser hello.

* USA: s regering Virginia
* USA: s regering Iowa

Mer information om hello Azure Government-molnet finns [Microsoft Azure Government](https://azure.microsoft.com/features/gov/) och [Utvecklarhandbok för Microsoft Azure Government](../azure-government-developer-guide.md).

### <a name="tooconnect-toohello-azure-china-cloud"></a>tooconnect toohello Azure-molnet för Kina
tooconnect toohello Azure Kina molnet, med någon av följande kommandon hello.

    Add-AzureRMAccount -EnvironmentName AzureChinaCloud

eller

    Add-AzureRmAccount -Environment (Get-AzureRmEnvironment -Name AzureChinaCloud)

toocreate en cachelagring i hello Azure Kina molnet, med någon av följande platser hello.

* Östra Kina
* Norra Kina

Mer information om hello Azure Kina molnet finns [AzureChinaCloud för Azure som drivs av 21Vianet i Kina](http://www.windowsazure.cn/).

### <a name="tooconnect-toomicrosoft-azure-germany"></a>tooconnect tooMicrosoft Azure Tyskland
tooconnect tooMicrosoft Azure Tyskland med någon av följande kommandon hello.

    Add-AzureRMAccount -EnvironmentName AzureGermanCloud


eller

    Add-AzureRmAccount -Environment (Get-AzureRmEnvironment -Name AzureGermanCloud)

toocreate en cachelagring i Microsoft Azure Tyskland med någon av följande platser hello.

* Centrala Tyskland
* Nordöstra Tyskland

Läs mer om Microsoft Azure Tyskland [Microsoft Azure Tyskland](https://azure.microsoft.com/overview/clouds/germany/).

### <a name="properties-used-for-azure-redis-cache-powershell"></a>Egenskaper som används för Azure Redis-Cache PowerShell
hello följande tabell innehåller egenskaper och beskrivningar för vanliga parametrar när du skapar och hanterar din Azure Redis-Cache-instanser som använder Azure PowerShell.

| Parameter | Beskrivning | Standard |
| --- | --- | --- |
| Namn |Namnet på hello cache | |
| Plats |Platsen för hello cache | |
| resourceGroupName |Resursgruppens namn i vilken toocreate hello-cache | |
| Storlek |hello storleken på hello cache. Giltiga värden är: P1, P2, P3, P4, C0, C1, C2, C3, C4, C5, C6, 250MB, 1GB, 2,5 GB, 6 GB, 13 GB, 26 GB, 53 GB |1 GB |
| ShardCount |hello antal shards toocreate när du skapar en premium-cache med aktiverad klustring. Giltiga värden är: 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 | |
| SKU |Anger hello SKU hello-cache. Giltiga värden är: Basic, Standard, Premium |Standard |
| RedisConfiguration |Anger inställningar för Redis-konfiguration. Mer information om varje inställning finns hello följande [RedisConfiguration egenskaper](#redisconfiguration-properties) tabell. | |
| enableNonSslPort |Anger om hello icke-SSL-porten är aktiverad. |False |
| MaxMemoryPolicy |Den här parametern är inaktuell - Använd RedisConfiguration i stället. | |
| StaticIP |När värd för ditt cacheminne i ett virtuellt nätverk, anger du en unik IP-adress i hello undernät för hello-cachen. Om inte är en valt från hello undernät. | |
| Undernät |När värd för ditt cacheminne i ett virtuellt nätverk, anger hello hello undernät i vilken toodeploy hello cache. | |
| VirtualNetwork |När värd för ditt cacheminne i ett VNET anger hello resurs-ID för hello virtuella nätverk i vilka toodeploy hello-cachen. | |
| KeyType |Anger vilka åtkomstnyckel tooregenerate när du förnyar åtkomstnycklar. Giltiga värden är: primära, sekundära | |

### <a name="redisconfiguration-properties"></a>RedisConfiguration egenskaper
| Egenskap | Beskrivning | Prisnivåer |
| --- | --- | --- |
| RDB säkerhetskopiering aktiverad |Om [Redis-datapersistence](cache-how-to-premium-persistence.md) är aktiverad |Endast Premium |
| RDB---anslutningssträngen för lagring |Hej anslutning sträng toohello storage-konto för [Redis-datapersistence](cache-how-to-premium-persistence.md) |Endast Premium |
| RDB säkerhetskopieringsfrekvens |Hej säkerhetskopieringsfrekvensen för [Redis-datapersistence](cache-how-to-premium-persistence.md) |Endast Premium |
| maxmemory-reserverade |Konfigurerar hello [minne som reserverats](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) för icke-cache-processer |Standard och Premium |
| maxmemory-princip |Konfigurerar hello [av principen](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) för hello-cache |Alla prisnivåer |
| meddela-keyspace-händelser |Konfigurerar [keyspace-meddelanden](cache-configure.md#keyspace-notifications-advanced-settings) |Standard och Premium |
| hash-max-ziplist-poster |Konfigurerar [minnesoptimering](http://redis.io/topics/memory-optimization) för små sammanställd datatyper |Standard och Premium |
| hash--ziplist-värdet för maximalt antal |Konfigurerar [minnesoptimering](http://redis.io/topics/memory-optimization) för små sammanställd datatyper |Standard och Premium |
| set-max-intset-poster |Konfigurerar [minnesoptimering](http://redis.io/topics/memory-optimization) för små sammanställd datatyper |Standard och Premium |
| zset-max-ziplist-poster |Konfigurerar [minnesoptimering](http://redis.io/topics/memory-optimization) för små sammanställd datatyper |Standard och Premium |
| zset--ziplist-värdet för maximalt antal |Konfigurerar [minnesoptimering](http://redis.io/topics/memory-optimization) för små sammanställd datatyper |Standard och Premium |
| databaser |Konfigurerar hello antalet databaser. Den här egenskapen kan endast konfigureras hos skapa cache. |Standard och Premium |

## <a name="toocreate-a-redis-cache"></a>toocreate Redis-Cache
Nya instanser av Azure Redis-Cache skapas med hello [ny AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) cmdlet.

> [!IMPORTANT]
> hello första gången du skapar ett Redis-cache i en prenumeration med hjälp av hello Azure-portalen hello portal registrerar hello `Microsoft.Cache` namnområde för den prenumerationen. Om du försöker toocreate hello först Redis-cache i en prenumeration med hjälp av PowerShell, måste du först registrera namnutrymmet med hjälp av följande kommando; hello Annars cmdlets som `New-AzureRmRedisCache` och `Get-AzureRmRedisCache` misslyckas.
> 
> `Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.Cache"`
> 
> 

toosee en lista över tillgängliga parametrar och deras beskrivningar för `New-AzureRmRedisCache`kör hello följande kommando.

    PS C:\> Get-Help New-AzureRmRedisCache -detailed

    NAME
        New-AzureRmRedisCache

    SYNOPSIS
        Creates a new redis cache.


    SYNTAX
        New-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -Location <String> [-RedisVersion <String>]
        [-Size <String>] [-Sku <String>] [-MaxMemoryPolicy <String>] [-RedisConfiguration <Hashtable>] [-EnableNonSslPort
        <Boolean>] [-ShardCount <Integer>] [-VirtualNetwork <String>] [-Subnet <String>] [-StaticIP <String>]
        [<CommonParameters>]


    DESCRIPTION
        hello New-AzureRmRedisCache cmdlet creates a new redis cache.


    PARAMETERS
        -Name <String>
            Name of hello redis cache toocreate.

        -ResourceGroupName <String>
            Name of resource group in which toocreate hello redis cache.

        -Location <String>
            Location in which toocreate hello redis cache.

        -RedisVersion <String>
            RedisVersion is deprecated and will be removed in future release.

        -Size <String>
            Size of hello redis cache. hello default value is 1GB or C1. Possible values are P1, P2, P3, P4, C0, C1, C2, C3,
            C4, C5, C6, 250MB, 1GB, 2.5GB, 6GB, 13GB, 26GB, 53GB.

        -Sku <String>
            Sku of redis cache. hello default value is Standard. Possible values are Basic, Standard and Premium.

        -MaxMemoryPolicy <String>
            hello 'MaxMemoryPolicy' setting has been deprecated. Please use 'RedisConfiguration' setting tooset
            MaxMemoryPolicy. e.g. -RedisConfiguration @{"maxmemory-policy" = "allkeys-lru"}

        -RedisConfiguration <Hashtable>
            All Redis Configuration Settings. Few possible keys: rdb-backup-enabled, rdb-storage-connection-string,
            rdb-backup-frequency, maxmemory-reserved, maxmemory-policy, notify-keyspace-events, hash-max-ziplist-entries,
            hash-max-ziplist-value, set-max-intset-entries, zset-max-ziplist-entries, zset-max-ziplist-value, databases.

        -EnableNonSslPort <Boolean>
            EnableNonSslPort is used by Azure Redis Cache. If no value is provided, hello default value is false and the
            non-SSL port will be disabled. Possible values are true and false.

        -ShardCount <Integer>
            hello number of shards toocreate on a Premium Cluster Cache.

        -VirtualNetwork <String>
            hello exact ARM resource ID of hello virtual network toodeploy hello redis cache in. Example format: /subscriptions/{
            subid}/resourceGroups/{resourceGroupName}/providers/Microsoft.ClassicNetwork/VirtualNetworks/{vnetName}

        -Subnet <String>
            Required when deploying a redis cache inside an existing Azure Virtual Network.

        -StaticIP <String>
            Required when deploying a redis cache inside an existing Azure Virtual Network.

        <CommonParameters>
            This cmdlet supports hello common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

toocreate en cache med standardparametrar, kör följande kommando hello.

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US"

`ResourceGroupName`, `Name`, och `Location` parametrar är obligatoriska, men resten av hello är valfria och har standardvärden. Kör hello föregående kommando skapar en Standard SKU Azure Redis-Cache-instans med hello angivet namn, plats och resursgruppen som är 1 GB i storlek med hello SSL-porten är inaktiverad.

toocreate premium-cache, ange en storlek på P1 (6 GB - 60 GB), P2 (13 GB - 130 GB), P3 (26 GB - 260 GB), eller P4 (53 GB - 530 GB). tooenable klustring, ange ett Fragmentera antal med hello `ShardCount` parameter. hello skapar följande exempel en premium P1-cache med 3 delar. Premium P1-cache är 6 GB i storlek och eftersom vi angett tre delar hello totala storlek är 18 GB (3 x 6 GB).

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -Sku Premium -Size P1 -ShardCount 3

toospecify värden för hello `RedisConfiguration` parameter, ange hello värden i `{}` som nyckel/värde-par som `@{"maxmemory-policy" = "allkeys-random", "notify-keyspace-events" = "KEA"}`. hello följande exempel skapar en standard 1 GB-cache med `allkeys-random` maxmemory-principen och keyspace-meddelanden som konfigurerats med `KEA`. Mer information finns i [Keyspace-meddelanden (avancerade inställningar)](cache-configure.md#keyspace-notifications-advanced-settings) och [minne principer](cache-configure.md#memory-policies).

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -RedisConfiguration @{"maxmemory-policy" = "allkeys-random", "notify-keyspace-events" = "KEA"}

<a name="databases"></a>

## <a name="tooconfigure-hello-databases-setting-during-cache-creation"></a>tooconfigure hello databaser anger under Skapa cache
Hej `databases` inställningen konfigureras bara under Skapa cache. hello följande exempel skapar en premium P3 (26 GB)-cache med 48 databaser med hjälp av hello [ny AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) cmdlet.

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -Sku Premium -Size P3 -RedisConfiguration @{"databases" = "48"}

Mer information om hello `databases` egenskap, se [Default Azure Redis-Cache-serverkonfigurationen](cache-configure.md#default-redis-server-configuration). Mer information om hur du skapar en cache med hello [ny AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) cmdlet, se föregående hello [toocreate Redis-Cache](#to-create-a-redis-cache) avsnitt.

## <a name="tooupdate-a-redis-cache"></a>tooupdate ett Redis-cache
Azure Redis-Cache-instanser har uppdaterats med hello [Set AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634518.aspx) cmdlet.

toosee en lista över tillgängliga parametrar och deras beskrivningar för `Set-AzureRmRedisCache`kör hello följande kommando.

    PS C:\> Get-Help Set-AzureRmRedisCache -detailed

    NAME
        Set-AzureRmRedisCache

    SYNOPSIS
        Set redis cache updatable parameters.

    SYNTAX
        Set-AzureRmRedisCache -Name <String> -ResourceGroupName <String> [-Size <String>] [-Sku <String>]
        [-MaxMemoryPolicy <String>] [-RedisConfiguration <Hashtable>] [-EnableNonSslPort <Boolean>] [-ShardCount
        <Integer>] [<CommonParameters>]

    DESCRIPTION
        hello Set-AzureRmRedisCache cmdlet sets redis cache parameters.

    PARAMETERS
        -Name <String>
            Name of hello redis cache tooupdate.

        -ResourceGroupName <String>
            Name of hello resource group for hello cache.

        -Size <String>
            Size of hello redis cache. hello default value is 1GB or C1. Possible values are P1, P2, P3, P4, C0, C1, C2, C3,
            C4, C5, C6, 250MB, 1GB, 2.5GB, 6GB, 13GB, 26GB, 53GB.

        -Sku <String>
            Sku of redis cache. hello default value is Standard. Possible values are Basic, Standard and Premium.

        -MaxMemoryPolicy <String>
            hello 'MaxMemoryPolicy' setting has been deprecated. Please use 'RedisConfiguration' setting tooset
            MaxMemoryPolicy. e.g. -RedisConfiguration @{"maxmemory-policy" = "allkeys-lru"}

        -RedisConfiguration <Hashtable>
            All Redis Configuration Settings. Few possible keys: rdb-backup-enabled, rdb-storage-connection-string,
            rdb-backup-frequency, maxmemory-reserved, maxmemory-policy, notify-keyspace-events, hash-max-ziplist-entries,
            hash-max-ziplist-value, set-max-intset-entries, zset-max-ziplist-entries, zset-max-ziplist-value.

        -EnableNonSslPort <Boolean>
            EnableNonSslPort is used by Azure Redis Cache. hello default value is null and no change will be made toothe
            currently configured value. Possible values are true and false.

        -ShardCount <Integer>
            hello number of shards toocreate on a Premium Cluster Cache.

        <CommonParameters>
            This cmdlet supports hello common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

Hej `Set-AzureRmRedisCache` används tooupdate egenskaper som kan vara `Size`, `Sku`, `EnableNonSslPort`, och hello `RedisConfiguration` värden. 

hello följande kommando uppdateringar hello maxmemory-princip för hello Redis-Cache med namnet myCache.

    Set-AzureRmRedisCache -ResourceGroupName "myGroup" -Name "myCache" -RedisConfiguration @{"maxmemory-policy" = "allkeys-random"}

<a name="scale"></a>

## <a name="tooscale-a-redis-cache"></a>tooscale ett Redis-cache
`Set-AzureRmRedisCache`kan använda tooscale en Azure Redis-cache-instans när hello `Size`, `Sku`, eller `ShardCount` ändras egenskaperna. 

> [!NOTE]
> Skala en cache med PowerShell är ämne toohello samma begränsningar och riktlinjer som skalning en cache från hello Azure-portalen. Du kan skala tooa olika prisnivån med hello följande begränsningar.
> 
> * Du kan skala från en högre prisnivå nivå tooa lägre prisnivån.
> * Du kan skala från en **Premium** cache ned tooa **Standard** eller en **grundläggande** cache.
> * Du kan skala från en **Standard** cache ned tooa **grundläggande** cache.
> * Du kan skala från en **grundläggande** cachelagra tooa **Standard** cache men du kan inte ändra hello storleken på hello samtidigt. Om du behöver olika storlek, kan du göra en efterföljande skalning åtgärden toohello önskad storlek.
> * Du kan skala från en **grundläggande** cachelagra direkt tooa **Premium** cache. Du måste skala från **grundläggande** för**Standard** i en skalning åtgärd och sedan från **Standard** för**Premium** i ett efterföljande skalning åtgärden.
> * Du kan skala från en större storlek ned toohello **C0 (250 MB)** storlek.
> 
> Mer information finns i [hur tooScale Azure Redis-Cache](cache-how-to-scale.md).
> 
> 

hello följande exempel visas hur tooscale en cache med namnet `myCache` tooa 2,5 GB cache. Observera att det här kommandot fungerar för både en Basic eller Standard cache.

    Set-AzureRmRedisCache -ResourceGroupName myGroup -Name myCache -Size 2.5GB

Efter det här kommandot har utfärdats, returneras hello status hello-cache (liknande toocalling `Get-AzureRmRedisCache`). Observera att hello `ProvisioningState` är `Scaling`.

    PS C:\> Set-AzureRmRedisCache -Name myCache -ResourceGroupName myGroup -Size 2.5GB


    Name               : mycache
    Id                 : /subscriptions/12ad12bd-abdc-2231-a2ed-a2b8b246bbad4/resourceGroups/mygroup/providers/Mi
                         crosoft.Cache/Redis/mycache
    Location           : South Central US
    Type               : Microsoft.Cache/Redis
    HostName           : mycache.redis.cache.windows.net
    Port               : 6379
    ProvisioningState  : Scaling
    SslPort            : 6380
    RedisConfiguration : {[maxmemory-policy, volatile-lru], [maxmemory-reserved, 150], [notify-keyspace-events, KEA],
                         [maxmemory-delta, 150]...}
    EnableNonSslPort   : False
    RedisVersion       : 3.0
    Size               : 1GB
    Sku                : Standard
    ResourceGroupName  : mygroup
    PrimaryKey         : ....
    SecondaryKey       : ....
    VirtualNetwork     :
    Subnet             :
    StaticIP           :
    TenantSettings     : {}
    ShardCount         :

När hello skalning åtgärden är klar, hello `ProvisioningState` ändras för`Succeeded`. Du måste vänta tills hello föregående åtgärd har slutförts eller så visas ett felmeddelande liknande toohello följande om du behöver toomake en efterföljande skalning åtgärd som att ändra från grundläggande tooStandard och sedan ändrar hello storlek.

    Set-AzureRmRedisCache : Conflict: hello resource '...' is not in a stable state, and is currently unable tooaccept hello update request.

## <a name="tooget-information-about-a-redis-cache"></a>tooget information om ett Redis-cache
Du kan hämta information om en cache med hello [Get-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634514.aspx) cmdlet.

toosee en lista över tillgängliga parametrar och deras beskrivningar för `Get-AzureRmRedisCache`kör hello följande kommando.

    PS C:\> Get-Help Get-AzureRmRedisCache -detailed

    NAME
        Get-AzureRmRedisCache

    SYNOPSIS
        Gets details about a single cache or all caches in hello specified resource group or all caches in hello current
        subscription.

    SYNTAX
        Get-AzureRmRedisCache [-Name <String>] [-ResourceGroupName <String>] [<CommonParameters>]

    DESCRIPTION
        hello Get-AzureRmRedisCache cmdlet gets hello details about a cache or caches depending on input parameters. If both
        ResourceGroupName and Name parameters are provided then Get-AzureRmRedisCache will return details about the
        specific cache name provided.

        If only ResourceGroupName is provided than it will return details about all caches in hello specified resource group.

        If no parameters are given than it will return details about all caches hello current subscription.

    PARAMETERS
        -Name <String>
            hello name of hello cache. When this parameter is provided along with ResourceGroupName, Get-AzureRmRedisCache
            returns hello details for hello cache.

        -ResourceGroupName <String>
            hello name of hello resource group that contains hello cache or caches. If ResourceGroupName is provided with Name
            then Get-AzureRmRedisCache returns hello details of hello cache specified by Name. If only hello ResourceGroup
            parameter is provided, then details for all caches in hello resource group are returned.

        <CommonParameters>
            This cmdlet supports hello common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

tooreturn information om alla Cacheminnena i hello aktuella prenumeration kör `Get-AzureRmRedisCache` utan några parametrar.

    Get-AzureRmRedisCache

tooreturn information om alla cacheminnen i en viss resursgrupp kör `Get-AzureRmRedisCache` med hello `ResourceGroupName` parameter.

    Get-AzureRmRedisCache -ResourceGroupName myGroup

Kör tooreturn information om en specifik cache `Get-AzureRmRedisCache` med hello `Name` parameter med hello namn hello cache och hello `ResourceGroupName` parameter med hello resursgruppen som innehåller om cachen.

    PS C:\> Get-AzureRmRedisCache -Name myCache -ResourceGroupName myGroup

    Name               : mycache
    Id                 : /subscriptions/12ad12bd-abdc-2231-a2ed-a2b8b246bbad4/resourceGroups/myGroup/providers/Mi
                         crosoft.Cache/Redis/mycache
    Location           : South Central US
    Type               : Microsoft.Cache/Redis
    HostName           : mycache.redis.cache.windows.net
    Port               : 6379
    ProvisioningState  : Succeeded
    SslPort            : 6380
    RedisConfiguration : {[maxmemory-policy, volatile-lru], [maxmemory-reserved, 62], [notify-keyspace-events, KEA],
                         [maxclients, 1000]...}
    EnableNonSslPort   : False
    RedisVersion       : 3.0
    Size               : 1GB
    Sku                : Standard
    ResourceGroupName  : myGroup
    VirtualNetwork     :
    Subnet             :
    StaticIP           :
    TenantSettings     : {}
    ShardCount         :

## <a name="tooretrieve-hello-access-keys-for-a-redis-cache"></a>tooretrieve hello åtkomstnycklar för Redis-cache
tooretrieve hello åtkomstnycklarna för ditt cacheminne, kan du använda hello [Get-AzureRmRedisCacheKey](https://msdn.microsoft.com/library/azure/mt634516.aspx) cmdlet.

toosee en lista över tillgängliga parametrar och deras beskrivningar för `Get-AzureRmRedisCacheKey`kör hello följande kommando.

    PS C:\> Get-Help Get-AzureRmRedisCacheKey -detailed

    NAME
        Get-AzureRmRedisCacheKey

    SYNOPSIS
        Gets hello accesskeys for hello specified redis cache.


    SYNTAX
        Get-AzureRmRedisCacheKey -Name <String> -ResourceGroupName <String> [<CommonParameters>]

    DESCRIPTION
        hello Get-AzureRmRedisCacheKey cmdlet gets hello access keys for hello specified cache.

    PARAMETERS
        -Name <String>
            Name of hello redis cache.

        -ResourceGroupName <String>
            Name of hello resource group for hello cache.

        <CommonParameters>
            This cmdlet supports hello common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

tooretrieve hello nycklar för ditt cacheminne anropet hello `Get-AzureRmRedisCacheKey` cmdlet- och pass i hello namnet på ditt cacheminne hello namnet på hello resursgruppen som innehåller hello cache.

    PS C:\> Get-AzureRmRedisCacheKey -Name myCache -ResourceGroupName myGroup

    PrimaryKey   : b2wdt43sfetlju4hfbryfnregrd9wgIcc6IA3zAO1lY=
    SecondaryKey : ABhfB757JgjIgt785JgKH9865eifmekfnn649303JKL=

## <a name="tooregenerate-access-keys-for-your-redis-cache"></a>tooregenerate åtkomstnycklarna för Redis-cache
tooregenerate hello åtkomstnycklarna för ditt cacheminne, kan du använda hello [ny AzureRmRedisCacheKey](https://msdn.microsoft.com/library/azure/mt634512.aspx) cmdlet.

toosee en lista över tillgängliga parametrar och deras beskrivningar för `New-AzureRmRedisCacheKey`kör hello följande kommando.

    PS C:\> Get-Help New-AzureRmRedisCacheKey -detailed

    NAME
        New-AzureRmRedisCacheKey

    SYNOPSIS
        Regenerates hello access key of a redis cache.

    SYNTAX
        New-AzureRmRedisCacheKey -Name <String> -ResourceGroupName <String> -KeyType <String> [-Force] [<CommonParameters>]

    DESCRIPTION
        hello New-AzureRmRedisCacheKey cmdlet regenerate hello access key of a redis cache.

    PARAMETERS
        -Name <String>
            Name of hello redis cache.

        -ResourceGroupName <String>
            Name of hello resource group for hello cache.

        -KeyType <String>
            Specifies whether tooregenerate hello primary or secondary access key. Possible values are Primary or Secondary.

        -Force
            When hello Force parameter is provided, hello specified access key is regenerated without any confirmation prompts.

        <CommonParameters>
            This cmdlet supports hello common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

tooregenerate hello primära och sekundära nycklarna för ditt cacheminne anropet hello `New-AzureRmRedisCacheKey` cmdlet pass i hello namn, resursgruppens namn och ange antingen `Primary` eller `Secondary` för hello `KeyType` parameter. I följande exempel hello, återskapas hello sekundära åtkomstnyckeln för cache.

    PS C:\> New-AzureRmRedisCacheKey -Name myCache -ResourceGroupName myGroup -KeyType Secondary

    Confirm
    Are you sure you want tooregenerate Secondary key for redis cache 'myCache'?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y


    PrimaryKey   : b2wdt43sfetlju4hfbryfnregrd9wgIcc6IA3zAO1lY=
    SecondaryKey : c53hj3kh4jhHjPJk8l0jji785JgKH9865eifmekfnn6=

## <a name="toodelete-a-redis-cache"></a>toodelete ett Redis-cache
toodelete Redis-cache använder hello [ta bort AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634515.aspx) cmdlet.

toosee en lista över tillgängliga parametrar och deras beskrivningar för `Remove-AzureRmRedisCache`kör hello följande kommando.

    PS C:\> Get-Help Remove-AzureRmRedisCache -detailed

    NAME
        Remove-AzureRmRedisCache

    SYNOPSIS
        Remove redis cache if exists.

    SYNTAX
        Remove-AzureRmRedisCache -Name <String> -ResourceGroupName <String> [-Force] [-PassThru] [<CommonParameters>

    DESCRIPTION
        hello Remove-AzureRmRedisCache cmdlet removes a redis cache if it exists.

    PARAMETERS
        -Name <String>
            Name of hello redis cache tooremove.

        -ResourceGroupName <String>
            Name of hello resource group of hello cache tooremove.

        -Force
            When hello Force parameter is provided, hello cache is removed without any confirmation prompts.

        -PassThru
            By default Remove-AzureRmRedisCache removes hello cache and does not return any value. If hello PassThru par
            is provided then Remove-AzureRmRedisCache returns a boolean value indicating hello success of hello operatio

        <CommonParameters>
            This cmdlet supports hello common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

I följande exempel hello, hello cache med namnet `myCache` tas bort.

    PS C:\> Remove-AzureRmRedisCache -Name myCache -ResourceGroupName myGroup

    Confirm
    Are you sure you want tooremove redis cache 'myCache'?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y


## <a name="tooimport-a-redis-cache"></a>tooimport ett Redis-cache
Du kan importera data till en Azure Redis-Cache-instans med hello `Import-AzureRmRedisCache` cmdlet.

> [!IMPORTANT]
> Import/Export är endast tillgängligt för [premiumnivån](cache-premium-tier-intro.md) cachelagrar. Mer information om Import/Export finns [importera och exportera data i Azure Redis-Cache](cache-how-to-import-export-data.md).
> 
> 

toosee en lista över tillgängliga parametrar och deras beskrivningar för `Import-AzureRmRedisCache`kör hello följande kommando.

    PS C:\> Get-Help Import-AzureRmRedisCache -detailed

    NAME
        Import-AzureRmRedisCache

    SYNOPSIS
        Import data from blobs tooAzure Redis Cache.


    SYNTAX
        Import-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -Files <String[]> [-Format <String>] [-Force]
        [-PassThru] [<CommonParameters>]


    DESCRIPTION
        hello Import-AzureRmRedisCache cmdlet imports data from hello specified blobs into Azure Redis Cache.


    PARAMETERS
        -Name <String>
            hello name of hello cache.

        -ResourceGroupName <String>
            hello name of hello resource group that contains hello cache.

        -Files <String[]>
            SAS urls of blobs whose content should be imported into hello cache.

        -Format <String>
            Format for hello blob.  Currently "rdb" is hello only supported, with other formats expected in hello future.

        -Force
            When hello Force parameter is provided, import will be performed without any confirmation prompts.

        -PassThru
            By default Import-AzureRmRedisCache imports data in cache and does not return any value. If hello PassThru
            parameter is provided then Import-AzureRmRedisCache returns a boolean value indicating hello success of the
            operation.

        <CommonParameters>
            This cmdlet supports hello common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).


hello importerar följande kommando data från hello blob som anges av hello SAS-uri i Azure Redis-Cache.

    PS C:\>Import-AzureRmRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -Files @("https://mystorageaccount.blob.core.windows.net/mycontainername/blobname?sv=2015-04-05&sr=b&sig=caIwutG2uDa0NZ8mjdNJdgOY8%2F8mhwRuGNdICU%2B0pI4%3D&st=2016-05-27T00%3A00%3A00Z&se=2016-05-28T00%3A00%3A00Z&sp=rwd") -Force

## <a name="tooexport-a-redis-cache"></a>tooexport ett Redis-cache
Du kan exportera data från en Azure Redis-Cache-instans med hello `Export-AzureRmRedisCache` cmdlet.

> [!IMPORTANT]
> Import/Export är endast tillgängligt för [premiumnivån](cache-premium-tier-intro.md) cachelagrar. Mer information om Import/Export finns [importera och exportera data i Azure Redis-Cache](cache-how-to-import-export-data.md).
> 
> 

toosee en lista över tillgängliga parametrar och deras beskrivningar för `Export-AzureRmRedisCache`kör hello följande kommando.

    PS C:\> Get-Help Export-AzureRmRedisCache -detailed

    NAME
        Export-AzureRmRedisCache

    SYNOPSIS
        Exports data from Azure Redis Cache tooa specified container.


    SYNTAX
        Export-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -Prefix <String> -Container <String> [-Format
        <String>] [-PassThru] [<CommonParameters>]


    DESCRIPTION
        hello Export-AzureRmRedisCache cmdlet exports data from Azure Redis Cache tooa specified container.


    PARAMETERS
        -Name <String>
            hello name of hello cache.

        -ResourceGroupName <String>
            hello name of hello resource group that contains hello cache.

        -Prefix <String>
            Prefix toouse for blob names.

        -Container <String>
            SAS url of container where data should be exported.

        -Format <String>
            Format for hello blob.  Currently "rdb" is hello only supported, with other formats expected in hello future.

        -PassThru
            By default Export-AzureRmRedisCache does not return any value. If hello PassThru parameter is provided
            then Export-AzureRmRedisCache returns a boolean value indicating hello success of hello operation.

        <CommonParameters>
            This cmdlet supports hello common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).


hello exporterar följande kommando data från en Azure Redis-Cache-instans till hello-behållare som anges av hello SAS-uri.

        PS C:\>Export-AzureRmRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -Prefix "blobprefix"
        -Container "https://mystorageaccount.blob.core.windows.net/mycontainer?sv=2015-04-05&sr=c&sig=HezZtBZ3DURmEGDduauE7
        pvETY4kqlPI8JCNa8ATmaw%3D&st=2016-05-27T00%3A00%3A00Z&se=2016-05-28T00%3A00%3A00Z&sp=rwdl"

## <a name="tooreboot-a-redis-cache"></a>tooreboot ett Redis-cache
Du kan starta om din Azure Redis-Cache-instans med hello `Reset-AzureRmRedisCache` cmdlet.

> [!IMPORTANT]
> Omstart är endast tillgängligt för [premiumnivån](cache-premium-tier-intro.md) cachelagrar. Mer information om omstart ditt cacheminne finns [cachelagra administration - omstart](cache-administration.md#reboot).
> 
> 

toosee en lista över tillgängliga parametrar och deras beskrivningar för `Reset-AzureRmRedisCache`kör hello följande kommando.

    PS C:\> Get-Help Reset-AzureRmRedisCache -detailed

    NAME
        Reset-AzureRmRedisCache

    SYNOPSIS
        Reboot specified node(s) of an Azure Redis Cache instance.


    SYNTAX
        Reset-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -RebootType <String> [-ShardId <Integer>]
        [-Force] [-PassThru] [<CommonParameters>]


    DESCRIPTION
        hello Reset-AzureRmRedisCache cmdlet reboots hello specified node(s) of an Azure Redis Cache instance.


    PARAMETERS
        -Name <String>
            hello name of hello cache.

        -ResourceGroupName <String>
            hello name of hello resource group that contains hello cache.

        -RebootType <String>
            Which node tooreboot. Possible values are "PrimaryNode", "SecondaryNode", "AllNodes".

        -ShardId <Integer>
            Which shard tooreboot when rebooting a premium cache with clustering enabled.

        -Force
            When hello Force parameter is provided, reset will be performed without any confirmation prompts.

        -PassThru
            By default Reset-AzureRmRedisCache does not return any value. If hello PassThru parameter is provided
            then Reset-AzureRmRedisCache returns a boolean value indicating hello success of hello operation.

        <CommonParameters>
            This cmdlet supports hello common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).


hello följande kommando startar om båda noderna i hello angetts cache.

        PS C:\>Reset-AzureRmRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -RebootType "AllNodes"
        -Force


## <a name="next-steps"></a>Nästa steg
toolearn mer information om hur du använder Windows PowerShell med Azure, se hello följande resurser:

* [Azure Redis-Cache cmdlet-dokumentationen på MSDN](https://msdn.microsoft.com/library/azure/mt634513.aspx)
* [Azure Resource Manager-Cmdlets](http://go.microsoft.com/fwlink/?LinkID=394765): Lär dig toouse hello-cmdletar i hello Azure Resource Manager-modulen.
* [Resursnamnet grupper toomanage resurserna i Azure](../azure-resource-manager/resource-group-template-deploy-portal.md): Lär dig hur toocreate och hantera resursgrupper i hello Azure-portalen.
* [Azure blogg](http://blogs.msdn.com/windowsazure): Lär dig mer om nya funktioner i Azure.
* [Windows PowerShell-blogg](http://blogs.msdn.com/powershell): Lär dig mer om nya funktioner i Windows PowerShell.
* [”Hej, skriptdoktorn”! Bloggen](http://blogs.technet.com/b/heyscriptingguy/): hämta verkliga tips och råd från hello Windows PowerShell-communityn.

