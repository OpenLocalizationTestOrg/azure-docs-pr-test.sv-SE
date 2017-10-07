---
title: "aaaAzure diagnostik tillägget konfigurationsversionerna schema och historik | Microsoft Docs"
description: "Relevanta toocollecting prestandaräknarna i Azure Virtual Machines, Skalningsuppsättningar, Service Fabric och Cloud Services."
services: monitoring-and-diagnostics
documentationcenter: .net
author: rboucher
manager: carmonm
editor: 
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 05/16/2017
ms.author: robb
ms.openlocfilehash: 854ad118f660810aa38703670284794d658142c7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-diagnostics-extention-configuration-schema-versions-and-history"></a><span data-ttu-id="d5cb6-103">Azure Diagnostics-tillägget configuration schemat versioner och historik</span><span class="sxs-lookup"><span data-stu-id="d5cb6-103">Azure Diagnostics extention configuration schema versions and history</span></span>
<span data-ttu-id="d5cb6-104">Den här sidan index Azure Diagnostics tillägget schemat versioner levereras som en del av hello Microsoft Azure SDK.</span><span class="sxs-lookup"><span data-stu-id="d5cb6-104">This page indexes Azure Diagnostics extension schema versions shipped as part of hello Microsoft Azure SDK.</span></span>  

> [!NOTE]
> <span data-ttu-id="d5cb6-105">hello Azure Diagnostics tillägget är hello komponent används toocollect prestandaräknare och annan statistik från:</span><span class="sxs-lookup"><span data-stu-id="d5cb6-105">hello Azure Diagnostics extension is hello component used toocollect performance counters and other statistics from:</span></span>
> - <span data-ttu-id="d5cb6-106">Azure Virtual Machines</span><span class="sxs-lookup"><span data-stu-id="d5cb6-106">Azure Virtual Machines</span></span> 
> - <span data-ttu-id="d5cb6-107">Skalningsuppsättningar för Virtual Machines</span><span class="sxs-lookup"><span data-stu-id="d5cb6-107">Virtual Machine Scale Sets</span></span>
> - <span data-ttu-id="d5cb6-108">Service Fabric</span><span class="sxs-lookup"><span data-stu-id="d5cb6-108">Service Fabric</span></span> 
> - <span data-ttu-id="d5cb6-109">Molntjänster</span><span class="sxs-lookup"><span data-stu-id="d5cb6-109">Cloud Services</span></span> 
> - <span data-ttu-id="d5cb6-110">Nätverkssäkerhetsgrupper</span><span class="sxs-lookup"><span data-stu-id="d5cb6-110">Network Security Groups</span></span>
> 
> <span data-ttu-id="d5cb6-111">Den här sidan gäller endast om du använder någon av dessa tjänster.</span><span class="sxs-lookup"><span data-stu-id="d5cb6-111">This page is only relevant if you are using one of these services.</span></span>

<span data-ttu-id="d5cb6-112">hello Azure Diagnostics tillägget används med andra Microsoft-produkter för diagnostik som Azure-Monitor, Application Insights och logganalys.</span><span class="sxs-lookup"><span data-stu-id="d5cb6-112">hello Azure Diagnostics extension is used with other Microsoft diagnostics products like Azure Monitor, Application Insights, and Log Analytics.</span></span> <span data-ttu-id="d5cb6-113">Mer information finns i [övervakning verktyg översikt över Microsoft](monitoring-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d5cb6-113">For more information see [Microsoft Monitoring Tools Overview](monitoring-overview.md).</span></span>

## <a name="azure-sdk-and-diagnostics-versions-shipping-chart"></a><span data-ttu-id="d5cb6-114">Azure SDK och diagnostik versioner leverans diagram</span><span class="sxs-lookup"><span data-stu-id="d5cb6-114">Azure SDK and diagnostics versions shipping chart</span></span>  

|<span data-ttu-id="d5cb6-115">Azure SDK-version</span><span class="sxs-lookup"><span data-stu-id="d5cb6-115">Azure SDK version</span></span> | <span data-ttu-id="d5cb6-116">Version för diagnostik-tillägg</span><span class="sxs-lookup"><span data-stu-id="d5cb6-116">Diagnostics extension version</span></span> | <span data-ttu-id="d5cb6-117">Modellen</span><span class="sxs-lookup"><span data-stu-id="d5cb6-117">Model</span></span>|  
|------------------|-------------------------------|------|  
|<span data-ttu-id="d5cb6-118">1.x</span><span class="sxs-lookup"><span data-stu-id="d5cb6-118">1.x</span></span>               |<span data-ttu-id="d5cb6-119">1.0</span><span class="sxs-lookup"><span data-stu-id="d5cb6-119">1.0</span></span>                            |<span data-ttu-id="d5cb6-120">plugin-program</span><span class="sxs-lookup"><span data-stu-id="d5cb6-120">plug-in</span></span>|  
|<span data-ttu-id="d5cb6-121">2.0 - 2.4</span><span class="sxs-lookup"><span data-stu-id="d5cb6-121">2.0 - 2.4</span></span>         |<span data-ttu-id="d5cb6-122">1.0</span><span class="sxs-lookup"><span data-stu-id="d5cb6-122">1.0</span></span>                            |<span data-ttu-id="d5cb6-123">plugin-program</span><span class="sxs-lookup"><span data-stu-id="d5cb6-123">plug-in</span></span>|  
|<span data-ttu-id="d5cb6-124">2.5</span><span class="sxs-lookup"><span data-stu-id="d5cb6-124">2.5</span></span>               |<span data-ttu-id="d5cb6-125">1.2</span><span class="sxs-lookup"><span data-stu-id="d5cb6-125">1.2</span></span>                            |<span data-ttu-id="d5cb6-126">Tillägg</span><span class="sxs-lookup"><span data-stu-id="d5cb6-126">extension</span></span>|  
|<span data-ttu-id="d5cb6-127">2.6</span><span class="sxs-lookup"><span data-stu-id="d5cb6-127">2.6</span></span>               |<span data-ttu-id="d5cb6-128">1.3</span><span class="sxs-lookup"><span data-stu-id="d5cb6-128">1.3</span></span>                            |<span data-ttu-id="d5cb6-129">"</span><span class="sxs-lookup"><span data-stu-id="d5cb6-129">"</span></span>|  
|<span data-ttu-id="d5cb6-130">2.7</span><span class="sxs-lookup"><span data-stu-id="d5cb6-130">2.7</span></span>               |<span data-ttu-id="d5cb6-131">1.4</span><span class="sxs-lookup"><span data-stu-id="d5cb6-131">1.4</span></span>                            |<span data-ttu-id="d5cb6-132">"</span><span class="sxs-lookup"><span data-stu-id="d5cb6-132">"</span></span>|  
|<span data-ttu-id="d5cb6-133">2.8</span><span class="sxs-lookup"><span data-stu-id="d5cb6-133">2.8</span></span>               |<span data-ttu-id="d5cb6-134">1.5</span><span class="sxs-lookup"><span data-stu-id="d5cb6-134">1.5</span></span>                            |<span data-ttu-id="d5cb6-135">"</span><span class="sxs-lookup"><span data-stu-id="d5cb6-135">"</span></span>|  
|<span data-ttu-id="d5cb6-136">2.9</span><span class="sxs-lookup"><span data-stu-id="d5cb6-136">2.9</span></span>               |<span data-ttu-id="d5cb6-137">1.6</span><span class="sxs-lookup"><span data-stu-id="d5cb6-137">1.6</span></span>                            |<span data-ttu-id="d5cb6-138">"</span><span class="sxs-lookup"><span data-stu-id="d5cb6-138">"</span></span>|
|<span data-ttu-id="d5cb6-139">2.96</span><span class="sxs-lookup"><span data-stu-id="d5cb6-139">2.96</span></span>              |<span data-ttu-id="d5cb6-140">1.7</span><span class="sxs-lookup"><span data-stu-id="d5cb6-140">1.7</span></span>                            |<span data-ttu-id="d5cb6-141">"</span><span class="sxs-lookup"><span data-stu-id="d5cb6-141">"</span></span>|
|<span data-ttu-id="d5cb6-142">2.96</span><span class="sxs-lookup"><span data-stu-id="d5cb6-142">2.96</span></span>              |<span data-ttu-id="d5cb6-143">1.8</span><span class="sxs-lookup"><span data-stu-id="d5cb6-143">1.8</span></span>                            |<span data-ttu-id="d5cb6-144">"</span><span class="sxs-lookup"><span data-stu-id="d5cb6-144">"</span></span>|
|<span data-ttu-id="d5cb6-145">2.96</span><span class="sxs-lookup"><span data-stu-id="d5cb6-145">2.96</span></span>              |<span data-ttu-id="d5cb6-146">1.8.1</span><span class="sxs-lookup"><span data-stu-id="d5cb6-146">1.8.1</span></span>                          |<span data-ttu-id="d5cb6-147">"</span><span class="sxs-lookup"><span data-stu-id="d5cb6-147">"</span></span>|
|<span data-ttu-id="d5cb6-148">2.96</span><span class="sxs-lookup"><span data-stu-id="d5cb6-148">2.96</span></span>              |<span data-ttu-id="d5cb6-149">1.9</span><span class="sxs-lookup"><span data-stu-id="d5cb6-149">1.9</span></span>                            |<span data-ttu-id="d5cb6-150">"</span><span class="sxs-lookup"><span data-stu-id="d5cb6-150">"</span></span>|



 <span data-ttu-id="d5cb6-151">Azure Diagnostics version 1.0 levererades först i en plugin-modell--vilket innebär att du fick hello versionen av Azure diagnostics som medföljer den när du installerade hello Azure SDK.</span><span class="sxs-lookup"><span data-stu-id="d5cb6-151">Azure Diagnostics version 1.0 first shipped in a plug-in model -- meaning that when you installed hello Azure SDK, you got hello version of Azure diagnostics shipped with it.</span></span>  

 <span data-ttu-id="d5cb6-152">Från och med SDK 2.5 (diagnostics version 1.2), Azure-diagnostik har gått tooan tillägget modellen.</span><span class="sxs-lookup"><span data-stu-id="d5cb6-152">Starting with SDK 2.5 (diagnostics version 1.2), Azure diagnostics went tooan extension model.</span></span> <span data-ttu-id="d5cb6-153">nya funktioner för hello verktyg tooutilize endast var tillgängliga i den nyare Azure SDK, men alla tjänster med hjälp av Azure-diagnostik skulle hämta hello senaste leverans versionen direkt från Azure.</span><span class="sxs-lookup"><span data-stu-id="d5cb6-153">hello tools tooutilize new features were only available in newer Azure SDKs, but any service using Azure diagnostics would pick up hello latest shipping version directly from Azure.</span></span> <span data-ttu-id="d5cb6-154">Till exempel skulle alla som fortfarande använder SDK 2.5 laddas hello senaste versionen visas i föregående tabell hello oavsett om de använder hello nyare funktioner.</span><span class="sxs-lookup"><span data-stu-id="d5cb6-154">For example, anyone still using SDK 2.5 would be loading hello latest version shown in hello previous table, regardless if they are using hello newer features.</span></span>  

## <a name="schemas-index"></a><span data-ttu-id="d5cb6-155">Scheman index</span><span class="sxs-lookup"><span data-stu-id="d5cb6-155">Schemas index</span></span>  
<span data-ttu-id="d5cb6-156">Konfiguration av olika scheman används för olika versioner av Azure-diagnostik.</span><span class="sxs-lookup"><span data-stu-id="d5cb6-156">Different versions of Azure diagnostics use different configuration schemas.</span></span> 

[<span data-ttu-id="d5cb6-157">Diagnostik 1.0 Konfigurationsschemat</span><span class="sxs-lookup"><span data-stu-id="d5cb6-157">Diagnostics 1.0 Configuration Schema</span></span>](azure-diagnostics-schema-1dot0.md)  

[<span data-ttu-id="d5cb6-158">Diagnostik 1.2 Konfigurationsschemat</span><span class="sxs-lookup"><span data-stu-id="d5cb6-158">Diagnostics 1.2 Configuration Schema</span></span>](azure-diagnostics-schema-1dot2.md)  

[<span data-ttu-id="d5cb6-159">Diagnostik 1.3 och senare Konfigurationsschemat</span><span class="sxs-lookup"><span data-stu-id="d5cb6-159">Diagnostics 1.3 and later Configuration Schema</span></span>](azure-diagnostics-schema-1dot3-and-later.md)  

## <a name="version-history"></a><span data-ttu-id="d5cb6-160">Versionshistorik</span><span class="sxs-lookup"><span data-stu-id="d5cb6-160">Version history</span></span>


### <a name="diagnostics-extension-19"></a><span data-ttu-id="d5cb6-161">Diagnostik tillägget 1.9.</span><span class="sxs-lookup"><span data-stu-id="d5cb6-161">Diagnostics extension 1.9</span></span> 
<span data-ttu-id="d5cb6-162">Stöd har lagts till Docker.</span><span class="sxs-lookup"><span data-stu-id="d5cb6-162">Added Docker support.</span></span>


### <a name="diagnostics-extension-181"></a><span data-ttu-id="d5cb6-163">Diagnostik tillägget 1.8.1</span><span class="sxs-lookup"><span data-stu-id="d5cb6-163">Diagnostics extension 1.8.1</span></span> 
<span data-ttu-id="d5cb6-164">Ange en SAS-token i stället för en lagringskontonyckel i hello privata config. Om en SAS-token har angetts, ignoreras hello lagringskontonyckel.</span><span class="sxs-lookup"><span data-stu-id="d5cb6-164">Can specify a SAS token instead of a storage account key in hello private config. If a SAS token is provided, hello storage account key is ignored.</span></span>


```json
{
    "storageAccountName": "diagstorageaccount",
    "storageAccountEndPoint": "https://core.windows.net",
    "storageAccountSasToken": "{sas token}",
    "SecondaryStorageAccounts": {
        "StorageAccount": [
            {
                "name": "secondarydiagstorageaccount",
                "endpoint": "https://core.windows.net",
                "sasToken": "{sas token}"
            }
        ]
    }
}
```

```xml
<PrivateConfig>
    <StorageAccount name="diagstorageaccount" endpoint="https://core.windows.net" sasToken="{sas token}" />
    <SecondaryStorageAccounts>
        <StorageAccount name="secondarydiagstorageaccount" endpoint="https://core.windows.net" sasToken="{sas token}" />
    </SecondaryStorageAccounts>
</PrivateConfig>
```


### <a name="diagnostics-extension-18"></a><span data-ttu-id="d5cb6-165">Diagnostik tillägget 1.8</span><span class="sxs-lookup"><span data-stu-id="d5cb6-165">Diagnostics extension 1.8</span></span> 
<span data-ttu-id="d5cb6-166">Tillagda lagringstyp tooPublicConfig.</span><span class="sxs-lookup"><span data-stu-id="d5cb6-166">Added Storage Type tooPublicConfig.</span></span> <span data-ttu-id="d5cb6-167">StorageType kan vara *tabell*, *Blob*, *TableAndBlob*.</span><span class="sxs-lookup"><span data-stu-id="d5cb6-167">StorageType can be *Table*, *Blob*, *TableAndBlob*.</span></span> <span data-ttu-id="d5cb6-168">*Tabellen* hello standard.</span><span class="sxs-lookup"><span data-stu-id="d5cb6-168">*Table* is hello default.</span></span>


```json
{
    "WadCfg": {
    },
    "StorageAccount": "diagstorageaccount",
    "StorageType": "TableAndBlob"
}
```

```xml
<PublicConfig>
    <WadCfg />
    <StorageAccount>diagstorageaccount</StorageAccount>
    <StorageType>TableAndBlob</StorageType>
</PublicConfig>
```


### <a name="diagnostics-extension-17"></a><span data-ttu-id="d5cb6-169">Diagnostik tillägget 1.7</span><span class="sxs-lookup"><span data-stu-id="d5cb6-169">Diagnostics extension 1.7</span></span> 
<span data-ttu-id="d5cb6-170">Tillagda hello möjlighet tooroute tooEventHub.</span><span class="sxs-lookup"><span data-stu-id="d5cb6-170">Added hello ability tooroute tooEventHub.</span></span>

### <a name="diagnostics-extension-15"></a><span data-ttu-id="d5cb6-171">Diagnostik tillägget 1.5</span><span class="sxs-lookup"><span data-stu-id="d5cb6-171">Diagnostics extension 1.5</span></span>
<span data-ttu-id="d5cb6-172">Lägga till hello egenskaperna element och hello möjlighet toosend diagnostikdata för[Programinsikter](../application-insights/app-insights-cloudservices.md) vilket gör det enklare toodiagnose problem i din App samt hello system och infrastruktur-nivå.</span><span class="sxs-lookup"><span data-stu-id="d5cb6-172">Added hello sinks element and hello ability toosend diagnostics data too[Application Insights](../application-insights/app-insights-cloudservices.md) making it easier toodiagnose issues across your application as well as hello system and infrastructure level.</span></span>

### <a name="azure-sdk-26-and-diagnostics-extension-13"></a><span data-ttu-id="d5cb6-173">Azure SDK 2.6 och diagnostik tillägget 1.3</span><span class="sxs-lookup"><span data-stu-id="d5cb6-173">Azure SDK 2.6 and diagnostics extension 1.3</span></span> 
<span data-ttu-id="d5cb6-174">Hello följande ändringar har gjorts för Cloud Service-projekt i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d5cb6-174">For Cloud Service projects in Visual Studio, hello following changes were made.</span></span> <span data-ttu-id="d5cb6-175">(Ändringarna gäller även toolater versioner av Azure SDK.)</span><span class="sxs-lookup"><span data-stu-id="d5cb6-175">(These changes also apply toolater versions of Azure SDK.)</span></span>

* <span data-ttu-id="d5cb6-176">hello lokala emulator stöder nu diagnostik.</span><span class="sxs-lookup"><span data-stu-id="d5cb6-176">hello local emulator now supports diagnostics.</span></span> <span data-ttu-id="d5cb6-177">Det innebär att du kan samla in diagnostikdata och se till att programmet skapar hello rätt spår när du utvecklar och testar i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d5cb6-177">This means you can collect diagnostics data and ensure your application is creating hello right traces while you're developing and testing in Visual Studio.</span></span> <span data-ttu-id="d5cb6-178">Hej anslutningssträngen `UseDevelopmentStorage=true` aktiverar diagnostik datainsamling när du kör cloud service-projekt i Visual Studio med hjälp av hello Azure storage-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="d5cb6-178">hello connection string `UseDevelopmentStorage=true` enables diagnostics data collection while you're running your cloud service project in Visual Studio by using hello Azure storage emulator.</span></span> <span data-ttu-id="d5cb6-179">Alla diagnostikdata samlas in i hello (utveckling lagring) storage-konto.</span><span class="sxs-lookup"><span data-stu-id="d5cb6-179">All diagnostics data is collected in hello (Development Storage) storage account.</span></span>
* <span data-ttu-id="d5cb6-180">hello diagnostik konto lagringsanslutningssträng (Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString) lagras igen i hello tjänstekonfigurationsfilen (.cscfg).</span><span class="sxs-lookup"><span data-stu-id="d5cb6-180">hello diagnostics storage account connection string (Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString) is stored once again in hello service configuration (.cscfg) file.</span></span> <span data-ttu-id="d5cb6-181">I Azure SDK 2.5 angavs hello diagnostiklagringskonto i hello diagnostics.wadcfgx-filen.</span><span class="sxs-lookup"><span data-stu-id="d5cb6-181">In Azure SDK 2.5 hello diagnostics storage account was specified in hello diagnostics.wadcfgx file.</span></span>

<span data-ttu-id="d5cb6-182">Det finns några viktiga skillnader mellan hur hello anslutningssträngen fungerade i Azure SDK 2.4 och tidigare och hur det fungerar i Azure SDK 2.6 eller senare.</span><span class="sxs-lookup"><span data-stu-id="d5cb6-182">There are some notable differences between how hello connection string worked in Azure SDK 2.4 and earlier and how it works in Azure SDK 2.6 and later.</span></span>

* <span data-ttu-id="d5cb6-183">I Azure SDK 2.4 och tidigare användes hello anslutningssträngen som en körning av hello diagnostik plugin-programmet tooget hello information om lagringskonto för överföring av diagnostik loggar.</span><span class="sxs-lookup"><span data-stu-id="d5cb6-183">In Azure SDK 2.4 and earlier, hello connection string was used as a runtime by hello diagnostics plugin tooget hello storage account information for transferring diagnostics logs.</span></span>
* <span data-ttu-id="d5cb6-184">Azure SDK 2.6 och senare, används hello diagnostik anslutningssträngen av Visual Studio tooconfigure hello diagnostik tillägg med information om hello lämpliga lagringskonto vid publicering.</span><span class="sxs-lookup"><span data-stu-id="d5cb6-184">In Azure SDK 2.6 and later, hello diagnostics connection string is used by Visual Studio tooconfigure hello diagnostics extension with hello appropriate storage account information during publishing.</span></span> <span data-ttu-id="d5cb6-185">hello anslutningssträngen kan du definiera olika lagringskonton för olika konfigurationer som Visual Studio ska använda när du publicerar.</span><span class="sxs-lookup"><span data-stu-id="d5cb6-185">hello connection string lets you define different storage accounts for different service configurations that Visual Studio will use when publishing.</span></span> <span data-ttu-id="d5cb6-186">Eftersom hello diagnostik plugin-programmet inte längre är tillgängliga (efter Azure SDK 2.5) kan inte hello .cscfg-filen ensamt aktivera hello diagnostik tillägg.</span><span class="sxs-lookup"><span data-stu-id="d5cb6-186">However, because hello diagnostics plugin is no longer available (after Azure SDK 2.5), hello .cscfg file by itself can't enable hello Diagnostics Extension.</span></span> <span data-ttu-id="d5cb6-187">Du har tooenable hello tillägget separat via verktyg som Visual Studio eller PowerShell.</span><span class="sxs-lookup"><span data-stu-id="d5cb6-187">You have tooenable hello extension separately through tools such as Visual Studio or PowerShell.</span></span>
* <span data-ttu-id="d5cb6-188">toosimplify hello konfigureringen av hello diagnostik tillägget med PowerShell, hello paketet utdata från Visual Studio innehåller också hello offentliga konfigurations-XML för hello diagnostik filnamnstillägget för varje roll.</span><span class="sxs-lookup"><span data-stu-id="d5cb6-188">toosimplify hello process of configuring hello diagnostics extension with PowerShell, hello package output from Visual Studio also contains hello public configuration XML for hello diagnostics extension for each role.</span></span> <span data-ttu-id="d5cb6-189">Visual Studio använder hello diagnostik toopopulate hello storage-kontoinformation om anslutningssträngar finns i hello offentliga konfiguration.</span><span class="sxs-lookup"><span data-stu-id="d5cb6-189">Visual Studio uses hello diagnostics connection string toopopulate hello storage account information present in hello public configuration.</span></span> <span data-ttu-id="d5cb6-190">hello offentliga config-filer skapas i hello tillägg mapp och följer hello mönster PaaSDiagnostics. <RoleName>. PubConfig.xml.</span><span class="sxs-lookup"><span data-stu-id="d5cb6-190">hello public config files are created in hello Extensions folder and follow hello pattern PaaSDiagnostics.<RoleName>.PubConfig.xml.</span></span> <span data-ttu-id="d5cb6-191">PowerShell-baserade distributioner kan använda det här mönstret toomap varje configuration tooa roll.</span><span class="sxs-lookup"><span data-stu-id="d5cb6-191">Any PowerShell based deployments can use this pattern toomap each configuration tooa Role.</span></span>
* <span data-ttu-id="d5cb6-192">hello anslutningssträngen i hello .cscfg-filen används också av hello Azure portal tooaccess hello diagnostikdata så visas det i hello **övervakning** fliken hello anslutningssträngen är nödvändiga tooconfigure hello service tooshow utförlig övervakningsdata i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="d5cb6-192">hello connection string in hello .cscfg file is also used by hello Azure portal tooaccess hello diagnostics data so it can appear in hello **Monitoring** tab. hello connection string is needed tooconfigure hello service tooshow verbose monitoring data in hello portal.</span></span>

#### <a name="migrating-projects-tooazure-sdk-26-and-later"></a><span data-ttu-id="d5cb6-193">Migrera projekt tooAzure SDK 2.6 och senare</span><span class="sxs-lookup"><span data-stu-id="d5cb6-193">Migrating projects tooAzure SDK 2.6 and later</span></span>
<span data-ttu-id="d5cb6-194">När du migrerar från Azure SDK 2,5 tooAzure SDK 2.6 eller senare, om du har en diagnostiklagringskonto som anges i hello .wadcfgx filen sedan förblir den det.</span><span class="sxs-lookup"><span data-stu-id="d5cb6-194">When migrating from Azure SDK 2.5 tooAzure SDK 2.6 or later, if you had a diagnostics storage account specified in hello .wadcfgx file, then it will stay there.</span></span> <span data-ttu-id="d5cb6-195">tootake nytta av hello flexibilitet för att använda olika lagringsplatser konton för olika lagringskonfigurationer, har du toomanually lägga till hello anslutning sträng tooyour projekt.</span><span class="sxs-lookup"><span data-stu-id="d5cb6-195">tootake advantage of hello flexibility of using different storage accounts for different storage configurations, you'll have toomanually add hello connection string tooyour project.</span></span> <span data-ttu-id="d5cb6-196">Om du migrerar ett projekt från Azure SDK 2.4 eller tidigare tooAzure SDK 2.6 hello du diagnostik anslutningssträngar bevaras.</span><span class="sxs-lookup"><span data-stu-id="d5cb6-196">If you're migrating a project from Azure SDK 2.4 or earlier tooAzure SDK 2.6, then hello diagnostics connection strings are preserved.</span></span> <span data-ttu-id="d5cb6-197">Observera dock hello ändringar i hur anslutningssträngar behandlas i Azure SDK 2.6 som anges i hello föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="d5cb6-197">However, please note hello changes in how connection strings are treated in Azure SDK 2.6 as specified in hello previous section.</span></span>

#### <a name="how-visual-studio-determines-hello-diagnostics-storage-account"></a><span data-ttu-id="d5cb6-198">Hur hello diagnostiklagringskonto avgör i Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d5cb6-198">How Visual Studio determines hello diagnostics storage account</span></span>
* <span data-ttu-id="d5cb6-199">Om en anslutningssträng för diagnostik anges i .cscfg-filen för hello använder Visual Studio den tooconfigure hello diagnostik tillägget när du publicerar och vid generering av xml-filer för hello offentliga konfiguration under paketering.</span><span class="sxs-lookup"><span data-stu-id="d5cb6-199">If a diagnostics connection string is specified in hello .cscfg file, Visual Studio uses it tooconfigure hello diagnostics extension when publishing, and when generating hello public configuration xml files during packaging.</span></span>
* <span data-ttu-id="d5cb6-200">Om inga diagnostik anslutningssträngen har angetts i hello .cscfg-filen, sedan faller Visual Studio tillbaka toousing hello storage-konto som anges i hello .wadcfgx tooconfigure hello diagnostik filnamnstillägg när filen publicering och generera hello offentliga XML-konfigurationsfiler när paketera.</span><span class="sxs-lookup"><span data-stu-id="d5cb6-200">If no diagnostics connection string is specified in hello .cscfg file, then Visual Studio falls back toousing hello storage account specified in hello .wadcfgx file tooconfigure hello diagnostics extension when publishing, and generating hello public configuration xml files when packaging.</span></span>
* <span data-ttu-id="d5cb6-201">hello diagnostik anslutningssträngen i hello .cscfg-filen har företräde framför hello storage-konto i hello .wadcfgx-filen.</span><span class="sxs-lookup"><span data-stu-id="d5cb6-201">hello diagnostics connection string in hello .cscfg file takes precedence over hello storage account in hello .wadcfgx file.</span></span> <span data-ttu-id="d5cb6-202">Om en anslutningssträng för diagnostik är anges i hello .cscfg-filen har Visual Studio använder som och ignorerar hello storage-konto i .wadcfgx.</span><span class="sxs-lookup"><span data-stu-id="d5cb6-202">If a diagnostics connection string is specified in hello .cscfg file, then Visual Studio uses that and ignores hello storage account in .wadcfgx.</span></span>

#### <a name="what-does-hello-update-development-storage-connection-strings-checkbox-do"></a><span data-ttu-id="d5cb6-203">Vad hello ”uppdatera development storage-anslutningssträngar...”</span><span class="sxs-lookup"><span data-stu-id="d5cb6-203">What does hello "Update development storage connection strings…"</span></span> <span data-ttu-id="d5cb6-204">kryssrutan för?</span><span class="sxs-lookup"><span data-stu-id="d5cb6-204">checkbox do?</span></span>
<span data-ttu-id="d5cb6-205">Hej kryssrutan för **uppdatera development storage-anslutningssträngar för diagnostik- och cachelagring med Microsoft Azure storage-kontoautentiseringsuppgifter när du publicerar tooMicrosoft Azure** ger dig ett enkelt sätt tooupdate alla utveckling lagringskontots anslutningssträngar med hello Azure storage-konto anges vid publicering.</span><span class="sxs-lookup"><span data-stu-id="d5cb6-205">hello checkbox for **Update development storage connection strings for Diagnostics and Caching with Microsoft Azure storage account credentials when publishing tooMicrosoft Azure** gives you a convenient way tooupdate any development storage account connection strings with hello Azure storage account specified during publishing.</span></span>

<span data-ttu-id="d5cb6-206">Anta att du väljer den här kryssrutan och hello diagnostik anslutningssträngen anger `UseDevelopmentStorage=true`.</span><span class="sxs-lookup"><span data-stu-id="d5cb6-206">For example, suppose you select this checkbox and hello diagnostics connection string specifies `UseDevelopmentStorage=true`.</span></span> <span data-ttu-id="d5cb6-207">När du publicerar hello projektet tooAzure uppdateras automatiskt hello diagnostik anslutningssträngen med hello storage-konto som du angav i guiden för hello publicera i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d5cb6-207">When you publish hello project tooAzure, Visual Studio will automatically update hello diagnostics connection string with hello storage account you specified in hello Publish wizard.</span></span> <span data-ttu-id="d5cb6-208">Men om en verklig lagringskonto har angetts som hello diagnostik anslutningssträng, används detta konto i stället.</span><span class="sxs-lookup"><span data-stu-id="d5cb6-208">However, if a real storage account was specified as hello diagnostics connection string, then that account is used instead.</span></span>

### <a name="diagnostics-functionality-differences-between-azure-sdk-24-and-earlier-and-azure-sdk-25-and-later"></a><span data-ttu-id="d5cb6-209">Diagnostik funktioner skillnaderna mellan Azure SDK 2.4 och tidigare och Azure SDK 2,5 och senare</span><span class="sxs-lookup"><span data-stu-id="d5cb6-209">Diagnostics functionality differences between Azure SDK 2.4 and earlier and Azure SDK 2.5 and later</span></span>
<span data-ttu-id="d5cb6-210">Om du uppgraderar ditt projekt från Azure SDK 2.4 tooAzure SDK 2.5 eller senare, du bör ha i åtanke hello följande diagnostik funktioner skillnader.</span><span class="sxs-lookup"><span data-stu-id="d5cb6-210">If you're upgrading your project from Azure SDK 2.4 tooAzure SDK 2.5 or later, you should bear in mind hello following diagnostics functionality differences.</span></span>

* <span data-ttu-id="d5cb6-211">**Konfiguration av API: er är inaktuella** – programkonfiguration av diagnostik är tillgänglig i Azure SDK 2.4 eller tidigare versioner, men är föråldrad i Azure SDK 2.5 och senare.</span><span class="sxs-lookup"><span data-stu-id="d5cb6-211">**Configuration APIs are deprecated** – Programmatic configuration of diagnostics is available in Azure SDK 2.4 or earlier versions, but is deprecated in Azure SDK 2.5 and later.</span></span> <span data-ttu-id="d5cb6-212">Om konfigurationen av diagnostik har definierats i koden, behöver du tooreconfigure dessa inställningar från grunden i hello migrerade projekt för diagnostik tookeep fungerar.</span><span class="sxs-lookup"><span data-stu-id="d5cb6-212">If your diagnostics configuration is currently defined in code, you'll need tooreconfigure those settings from scratch in hello migrated project in order for diagnostics tookeep working.</span></span> <span data-ttu-id="d5cb6-213">konfigurationsfilen för hello diagnostik för Azure SDK 2.4 är diagnostics.wadcfg och diagnostics.wadcfgx för Azure SDK 2.5 och senare.</span><span class="sxs-lookup"><span data-stu-id="d5cb6-213">hello diagnostics configuration file for Azure SDK 2.4 is diagnostics.wadcfg, and diagnostics.wadcfgx for Azure SDK 2.5 and later.</span></span>
* <span data-ttu-id="d5cb6-214">**Diagnostik för molnprogram för tjänsten kan bara konfigureras på hello rollnivå, inte på instansnivå hello.**</span><span class="sxs-lookup"><span data-stu-id="d5cb6-214">**Diagnostics for cloud service applications can only be configured at hello role level, not at hello instance level.**</span></span>
* <span data-ttu-id="d5cb6-215">**Varje gång du distribuerar appen hello diagnostik konfiguration uppdateras** – om du ändrar konfigurationen diagnostik från Server Explorer och distribuera din app kan det medföra problem för paritet.</span><span class="sxs-lookup"><span data-stu-id="d5cb6-215">**Every time you deploy your app, hello diagnostics configuration is updated** – This can cause parity issues if you change your diagnostics configuration from Server Explorer and then redeploy your app.</span></span>
* <span data-ttu-id="d5cb6-216">**Azure SDK 2.5 och senare, krascher Dumpar konfigureras i hello konfigurationsfil diagnostik, inte i kod** – om du har krascher Dumpar som konfigurerats i koden har du toomanually överföring hello konfiguration från kod toohello konfigurationsfilen eftersom hello kraschdumpar överförs inte under hello migrering tooAzure SDK 2.6.</span><span class="sxs-lookup"><span data-stu-id="d5cb6-216">**In Azure SDK 2.5 and later, crash dumps are configured in hello diagnostics configuration file, not in code** – If you have crash dumps configured in code, you'll have toomanually transfer hello configuration from code toohello configuration file, because hello crash dumps aren't transferred during hello migration tooAzure SDK 2.6.</span></span>

