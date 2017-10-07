---
title: aaaAzure Cosmos DB Automation - hantering med Powershell | Microsoft Docs
description: "Använd Azure Powershell hantera dina Azure DB som Cosmos-konton."
services: cosmos-db
author: dmakwana
manager: jhubbard
editor: 
tags: azure-resource-manager
documentationcenter: 
ms.assetid: 
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/21/2017
ms.author: dimakwan
ms.openlocfilehash: 3239fb815918a0e47bff69fcd1ab6562519e429b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-cosmos-db-account-using-powershell"></a><span data-ttu-id="7ec16-103">Skapa ett Azure DB som Cosmos-konto med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="7ec16-103">Create an Azure Cosmos DB account using PowerShell</span></span>

<span data-ttu-id="7ec16-104">hello beskriver följande guiden kommandon tooautomate hanteringen av dina Azure Cosmos DB databasen konton med hjälp av Azure Powershell.</span><span class="sxs-lookup"><span data-stu-id="7ec16-104">hello following guide describes commands tooautomate management of your Azure Cosmos DB database accounts using Azure Powershell.</span></span> <span data-ttu-id="7ec16-105">Den innehåller också kommandon toomanage nycklar och prioriteringar för växling vid fel i [flera regioner databasen konton][scaling-globally].</span><span class="sxs-lookup"><span data-stu-id="7ec16-105">It also includes commands toomanage account keys and failover priorities in [multi-region database accounts][scaling-globally].</span></span> <span data-ttu-id="7ec16-106">Uppdatera ditt konto kan du toomodify konsekvenskontroll principer och Lägg till/ta bort regioner.</span><span class="sxs-lookup"><span data-stu-id="7ec16-106">Updating your database account allows you toomodify consistency policies and add/remove regions.</span></span> <span data-ttu-id="7ec16-107">Plattformsoberoende hantering av Azure DB som Cosmos-konto, kan använda antingen [Azure CLI](cli-samples.md), hello [Resource Provider REST API][rp-rest-api], eller hello [Azure portalen](create-documentdb-dotnet.md#create-account).</span><span class="sxs-lookup"><span data-stu-id="7ec16-107">For cross-platform management of your Azure Cosmos DB account, you can use either [Azure CLI](cli-samples.md), hello [Resource Provider REST API][rp-rest-api], or hello [Azure portal](create-documentdb-dotnet.md#create-account).</span></span>

## <a name="getting-started"></a><span data-ttu-id="7ec16-108">Komma igång</span><span class="sxs-lookup"><span data-stu-id="7ec16-108">Getting Started</span></span>

<span data-ttu-id="7ec16-109">Följ anvisningarna för hello i [hur tooinstall och konfigurera Azure PowerShell] [ powershell-install-configure] tooinstall och logga in tooyour Azure Resource Manager-kontot i Powershell.</span><span class="sxs-lookup"><span data-stu-id="7ec16-109">Follow hello instructions in [How tooinstall and configure Azure PowerShell][powershell-install-configure] tooinstall and log in tooyour Azure Resource Manager account in Powershell.</span></span>

### <a name="notes"></a><span data-ttu-id="7ec16-110">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="7ec16-110">Notes</span></span>

* <span data-ttu-id="7ec16-111">Om du vill att tooexecute hello följande kommandon utan bekräftelse från användaren att lägga till hello `-Force` flaggan toohello kommando.</span><span class="sxs-lookup"><span data-stu-id="7ec16-111">If you would like tooexecute hello following commands without requiring user confirmation, append hello `-Force` flag toohello command.</span></span>
* <span data-ttu-id="7ec16-112">Alla hello följande kommandon är synkrona.</span><span class="sxs-lookup"><span data-stu-id="7ec16-112">All hello following commands are synchronous.</span></span>

## <span data-ttu-id="7ec16-113"><a id="create-documentdb-account-powershell"></a>Skapa ett Azure Cosmos DB-konto</span><span class="sxs-lookup"><span data-stu-id="7ec16-113"><a id="create-documentdb-account-powershell"></a> Create an Azure Cosmos DB Account</span></span>

<span data-ttu-id="7ec16-114">Det här kommandot kan du toocreate ett konto för Azure DB som Cosmos-databasen.</span><span class="sxs-lookup"><span data-stu-id="7ec16-114">This command allows you toocreate an Azure Cosmos DB database account.</span></span> <span data-ttu-id="7ec16-115">Konfigurera ditt nya konto som antingen en region eller [flera regioner] [ scaling-globally] med en viss [konsekvent princip](consistency-levels.md).</span><span class="sxs-lookup"><span data-stu-id="7ec16-115">Configure your new database account as either single-region or [multi-region][scaling-globally] with a certain [consistency policy](consistency-levels.md).</span></span>

    $locations = @(@{"locationName"="<write-region-location>"; "failoverPriority"=0}, @{"locationName"="<read-region-location>"; "failoverPriority"=1})
    $iprangefilter = "<ip-range-filter>"
    $consistencyPolicy = @{"defaultConsistencyLevel"="<default-consistency-level>"; "maxIntervalInSeconds"="<max-interval>"; "maxStalenessPrefix"="<max-staleness-prefix>"}
    $CosmosDBProperties = @{"databaseAccountOfferType"="Standard"; "locations"=$locations; "consistencyPolicy"=$consistencyPolicy; "ipRangeFilter"=$iprangefilter}
    New-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName <resource-group-name>  -Location "<resource-group-location>" -Name <database-account-name> -Properties $CosmosDBProperties
    
* <span data-ttu-id="7ec16-116">`<write-region-location>`namn på hello hello skriva regionens hello konto.</span><span class="sxs-lookup"><span data-stu-id="7ec16-116">`<write-region-location>` hello location name of hello write region of hello database account.</span></span> <span data-ttu-id="7ec16-117">Den här platsen är nödvändig toohave redundans prioriteringsvärdet 0.</span><span class="sxs-lookup"><span data-stu-id="7ec16-117">This location is required toohave a failover priority value of 0.</span></span> <span data-ttu-id="7ec16-118">Det måste finnas exakt en skrivning region per konto.</span><span class="sxs-lookup"><span data-stu-id="7ec16-118">There must be exactly one write region per database account.</span></span>
* <span data-ttu-id="7ec16-119">`<read-region-location>`namn på hello hello läsa regionens hello databaskonto.</span><span class="sxs-lookup"><span data-stu-id="7ec16-119">`<read-region-location>` hello location name of hello read region of hello database account.</span></span> <span data-ttu-id="7ec16-120">Den här platsen är nödvändig toohave ett prioritetsvärde för växling vid fel som är större än 0.</span><span class="sxs-lookup"><span data-stu-id="7ec16-120">This location is required toohave a failover priority value of greater than 0.</span></span> <span data-ttu-id="7ec16-121">Det kan finnas mer än en skrivskyddad regioner per konto.</span><span class="sxs-lookup"><span data-stu-id="7ec16-121">There can be more than one read regions per database account.</span></span>
* <span data-ttu-id="7ec16-122">`<ip-range-filter>`Anger hello uppsättning IP-adresser och IP-adressintervall i CIDR formuläret toobe inkluderad som hello tillåts lista över klientens IP-adresser för en viss databas-konto.</span><span class="sxs-lookup"><span data-stu-id="7ec16-122">`<ip-range-filter>` Specifies hello set of IP addresses or IP address ranges in CIDR form toobe included as hello allowed list of client IPs for a given database account.</span></span> <span data-ttu-id="7ec16-123">IP-adressintervall måste vara kommatecken avgränsade och får inte innehålla blanksteg.</span><span class="sxs-lookup"><span data-stu-id="7ec16-123">IP addresses/ranges must be comma separated and must not contain any spaces.</span></span> <span data-ttu-id="7ec16-124">Mer information finns i [Azure Cosmos DB-brandväggsstöd](firewall-support.md)</span><span class="sxs-lookup"><span data-stu-id="7ec16-124">For more information, see [Azure Cosmos DB Firewall Support](firewall-support.md)</span></span>
* <span data-ttu-id="7ec16-125">`<default-consistency-level>`hello konsekvenskontroll standardnivå för hello Azure DB som Cosmos-konto.</span><span class="sxs-lookup"><span data-stu-id="7ec16-125">`<default-consistency-level>` hello default consistency level of hello Azure Cosmos DB account.</span></span> <span data-ttu-id="7ec16-126">Mer information finns i [Konsekvensnivåer i Azure Cosmos DB](consistency-levels.md).</span><span class="sxs-lookup"><span data-stu-id="7ec16-126">For more information, see [Consistency Levels in Azure Cosmos DB](consistency-levels.md).</span></span>
* <span data-ttu-id="7ec16-127">`<max-interval>`När det används med begränsas föråldringskonsekvens representerar det här värdet hello tid föråldrad (i sekunder) tolereras.</span><span class="sxs-lookup"><span data-stu-id="7ec16-127">`<max-interval>` When used with Bounded Staleness consistency, this value represents hello time amount of staleness (in seconds) tolerated.</span></span> <span data-ttu-id="7ec16-128">Tillåtet område för det här värdet är 1-100.</span><span class="sxs-lookup"><span data-stu-id="7ec16-128">Accepted range for this value is 1 - 100.</span></span>
* <span data-ttu-id="7ec16-129">`<max-staleness-prefix>`När det används med begränsas föråldringskonsekvens representerar det här värdet hello antal inaktiva begäranden tolereras.</span><span class="sxs-lookup"><span data-stu-id="7ec16-129">`<max-staleness-prefix>` When used with Bounded Staleness consistency, this value represents hello number of stale requests tolerated.</span></span> <span data-ttu-id="7ec16-130">Tillåtet område för det här värdet är 1 – 2 147 483 647.</span><span class="sxs-lookup"><span data-stu-id="7ec16-130">Accepted range for this value is 1 – 2,147,483,647.</span></span>
* <span data-ttu-id="7ec16-131">`<resource-group-name>`hello namnet på hello [Azure-resursgrupp] [ azure-resource-groups] toowhich hello nya Azure Cosmos DB databaskonto tillhör.</span><span class="sxs-lookup"><span data-stu-id="7ec16-131">`<resource-group-name>` hello name of hello [Azure Resource Group][azure-resource-groups] toowhich hello new Azure Cosmos DB database account belongs to.</span></span>
* <span data-ttu-id="7ec16-132">`<resource-group-location>`hello platsen för hello Azure-resursgrupp toowhich hello nya Azure Cosmos DB konto tillhör.</span><span class="sxs-lookup"><span data-stu-id="7ec16-132">`<resource-group-location>` hello location of hello Azure Resource Group toowhich hello new Azure Cosmos DB database account belongs to.</span></span>
* <span data-ttu-id="7ec16-133">`<database-account-name>`hello namnet på hello Azure Cosmos DB databasen konto toobe skapas.</span><span class="sxs-lookup"><span data-stu-id="7ec16-133">`<database-account-name>` hello name of hello Azure Cosmos DB database account toobe created.</span></span> <span data-ttu-id="7ec16-134">Det kan endast använda gemena bokstäver, siffror, hello '-' tecken och måste vara mellan 3 och 50 tecken.</span><span class="sxs-lookup"><span data-stu-id="7ec16-134">It can only use lowercase letters, numbers, hello '-' character, and must be between 3 and 50 characters.</span></span>

<span data-ttu-id="7ec16-135">Exempel:</span><span class="sxs-lookup"><span data-stu-id="7ec16-135">Example:</span></span> 

    $locations = @(@{"locationName"="West US"; "failoverPriority"=0}, @{"locationName"="East US"; "failoverPriority"=1})
    $iprangefilter = ""
    $consistencyPolicy = @{"defaultConsistencyLevel"="BoundedStaleness"; "maxIntervalInSeconds"=5; "maxStalenessPrefix"=100}
    $CosmosDBProperties = @{"databaseAccountOfferType"="Standard"; "locations"=$locations; "consistencyPolicy"=$consistencyPolicy; "ipRangeFilter"=$iprangefilter}
    New-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Location "West US" -Name "docdb-test" -Properties $CosmosDBProperties

### <a name="notes"></a><span data-ttu-id="7ec16-136">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="7ec16-136">Notes</span></span>
* <span data-ttu-id="7ec16-137">hello skapar exemplet ovan ett konto med två områden.</span><span class="sxs-lookup"><span data-stu-id="7ec16-137">hello preceding example creates a database account with two regions.</span></span> <span data-ttu-id="7ec16-138">Det är också möjligt toocreate ett databaskonto med en region (som är utsedd till hello skrivåtgärder region och har ett prioritetsvärde för växling vid fel 0) eller fler än två regioner.</span><span class="sxs-lookup"><span data-stu-id="7ec16-138">It is also possible toocreate a database account with either one region (which is designated as hello write region and have a failover priority value of 0) or more than two regions.</span></span> <span data-ttu-id="7ec16-139">Mer information finns i [flera regioner databasen konton][scaling-globally].</span><span class="sxs-lookup"><span data-stu-id="7ec16-139">For more information, see [multi-region database accounts][scaling-globally].</span></span>
* <span data-ttu-id="7ec16-140">hello platser måste vara regioner som Azure Cosmos DB är allmänt tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="7ec16-140">hello locations must be regions in which Azure Cosmos DB is generally available.</span></span> <span data-ttu-id="7ec16-141">hello aktuell lista över regioner finns på hello [Azure-regioner sidan](https://azure.microsoft.com/regions/#services).</span><span class="sxs-lookup"><span data-stu-id="7ec16-141">hello current list of regions is provided on hello [Azure Regions page](https://azure.microsoft.com/regions/#services).</span></span>

## <span data-ttu-id="7ec16-142"><a id="update-documentdb-account-powershell"></a>Uppdatera ett DocumentDB-databaskonto</span><span class="sxs-lookup"><span data-stu-id="7ec16-142"><a id="update-documentdb-account-powershell"></a> Update a DocumentDB Database Account</span></span>

<span data-ttu-id="7ec16-143">Det här kommandot ger tooupdate kontoegenskaperna Azure DB som Cosmos-databasen.</span><span class="sxs-lookup"><span data-stu-id="7ec16-143">This command allows you tooupdate your Azure Cosmos DB database account properties.</span></span> <span data-ttu-id="7ec16-144">Detta inkluderar hello konsekvent princip och hello platser vilka hello konto finns i.</span><span class="sxs-lookup"><span data-stu-id="7ec16-144">This includes hello consistency policy and hello locations which hello database account exists in.</span></span>

> [!NOTE]
> <span data-ttu-id="7ec16-145">Det här kommandot kan du tooadd och ta bort regioner, men kan du inte toomodify redundans prioriteter.</span><span class="sxs-lookup"><span data-stu-id="7ec16-145">This command allows you tooadd and remove regions but does not allow you toomodify failover priorities.</span></span> <span data-ttu-id="7ec16-146">toomodify redundans prioriteter finns [under](#modify-failover-priority-powershell).</span><span class="sxs-lookup"><span data-stu-id="7ec16-146">toomodify failover priorities, see [below](#modify-failover-priority-powershell).</span></span>

    $locations = @(@{"locationName"="<write-region-location>"; "failoverPriority"=0}, @{"locationName"="<read-region-location>"; "failoverPriority"=1})
    $iprangefilter = "<ip-range-filter>"
    $consistencyPolicy = @{"defaultConsistencyLevel"="<default-consistency-level>"; "maxIntervalInSeconds"="<max-interval>"; "maxStalenessPrefix"="<max-staleness-prefix>"}
    $CosmosDBProperties = @{"databaseAccountOfferType"="Standard"; "locations"=$locations; "consistencyPolicy"=$consistencyPolicy; "ipRangeFilter"=$iprangefilter}
    Set-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName <resource-group-name> -Name <database-account-name> -Properties $CosmosDBProperties
    
* <span data-ttu-id="7ec16-147">`<write-region-location>`namn på hello hello skriva regionens hello konto.</span><span class="sxs-lookup"><span data-stu-id="7ec16-147">`<write-region-location>` hello location name of hello write region of hello database account.</span></span> <span data-ttu-id="7ec16-148">Den här platsen är nödvändig toohave redundans prioriteringsvärdet 0.</span><span class="sxs-lookup"><span data-stu-id="7ec16-148">This location is required toohave a failover priority value of 0.</span></span> <span data-ttu-id="7ec16-149">Det måste finnas exakt en skrivning region per konto.</span><span class="sxs-lookup"><span data-stu-id="7ec16-149">There must be exactly one write region per database account.</span></span>
* <span data-ttu-id="7ec16-150">`<read-region-location>`namn på hello hello läsa regionens hello databaskonto.</span><span class="sxs-lookup"><span data-stu-id="7ec16-150">`<read-region-location>` hello location name of hello read region of hello database account.</span></span> <span data-ttu-id="7ec16-151">Den här platsen är nödvändig toohave ett prioritetsvärde för växling vid fel som är större än 0.</span><span class="sxs-lookup"><span data-stu-id="7ec16-151">This location is required toohave a failover priority value of greater than 0.</span></span> <span data-ttu-id="7ec16-152">Det kan finnas mer än en skrivskyddad regioner per konto.</span><span class="sxs-lookup"><span data-stu-id="7ec16-152">There can be more than one read regions per database account.</span></span>
* <span data-ttu-id="7ec16-153">`<default-consistency-level>`hello konsekvenskontroll standardnivå för hello Azure DB som Cosmos-konto.</span><span class="sxs-lookup"><span data-stu-id="7ec16-153">`<default-consistency-level>` hello default consistency level of hello Azure Cosmos DB account.</span></span> <span data-ttu-id="7ec16-154">Mer information finns i [Konsekvensnivåer i Azure Cosmos DB](consistency-levels.md).</span><span class="sxs-lookup"><span data-stu-id="7ec16-154">For more information, see [Consistency Levels in Azure Cosmos DB](consistency-levels.md).</span></span>
* <span data-ttu-id="7ec16-155">`<ip-range-filter>`Anger hello uppsättning IP-adresser och IP-adressintervall i CIDR formuläret toobe inkluderad som hello tillåts lista över klientens IP-adresser för en viss databas-konto.</span><span class="sxs-lookup"><span data-stu-id="7ec16-155">`<ip-range-filter>` Specifies hello set of IP addresses or IP address ranges in CIDR form toobe included as hello allowed list of client IPs for a given database account.</span></span> <span data-ttu-id="7ec16-156">IP-adressintervall måste vara kommatecken avgränsade och får inte innehålla blanksteg.</span><span class="sxs-lookup"><span data-stu-id="7ec16-156">IP addresses/ranges must be comma separated and must not contain any spaces.</span></span> <span data-ttu-id="7ec16-157">Mer information finns i [Azure Cosmos DB-brandväggsstöd](firewall-support.md)</span><span class="sxs-lookup"><span data-stu-id="7ec16-157">For more information, see [Azure Cosmos DB Firewall Support](firewall-support.md)</span></span>
* <span data-ttu-id="7ec16-158">`<max-interval>`När det används med begränsas föråldringskonsekvens representerar det här värdet hello tid föråldrad (i sekunder) tolereras.</span><span class="sxs-lookup"><span data-stu-id="7ec16-158">`<max-interval>` When used with Bounded Staleness consistency, this value represents hello time amount of staleness (in seconds) tolerated.</span></span> <span data-ttu-id="7ec16-159">Tillåtet område för det här värdet är 1-100.</span><span class="sxs-lookup"><span data-stu-id="7ec16-159">Accepted range for this value is 1 - 100.</span></span>
* <span data-ttu-id="7ec16-160">`<max-staleness-prefix>`När det används med begränsas föråldringskonsekvens representerar det här värdet hello antal inaktiva begäranden tolereras.</span><span class="sxs-lookup"><span data-stu-id="7ec16-160">`<max-staleness-prefix>` When used with Bounded Staleness consistency, this value represents hello number of stale requests tolerated.</span></span> <span data-ttu-id="7ec16-161">Tillåtet område för det här värdet är 1 – 2 147 483 647.</span><span class="sxs-lookup"><span data-stu-id="7ec16-161">Accepted range for this value is 1 – 2,147,483,647.</span></span>
* <span data-ttu-id="7ec16-162">`<resource-group-name>`hello namnet på hello [Azure-resursgrupp] [ azure-resource-groups] toowhich hello nya Azure Cosmos DB databaskonto tillhör.</span><span class="sxs-lookup"><span data-stu-id="7ec16-162">`<resource-group-name>` hello name of hello [Azure Resource Group][azure-resource-groups] toowhich hello new Azure Cosmos DB database account belongs to.</span></span>
* <span data-ttu-id="7ec16-163">`<resource-group-location>`hello platsen för hello Azure-resursgrupp toowhich hello nya Azure Cosmos DB konto tillhör.</span><span class="sxs-lookup"><span data-stu-id="7ec16-163">`<resource-group-location>` hello location of hello Azure Resource Group toowhich hello new Azure Cosmos DB database account belongs to.</span></span>
* <span data-ttu-id="7ec16-164">`<database-account-name>`hello namnet på hello Azure Cosmos DB databasen konto toobe uppdateras.</span><span class="sxs-lookup"><span data-stu-id="7ec16-164">`<database-account-name>` hello name of hello Azure Cosmos DB database account toobe updated.</span></span>

<span data-ttu-id="7ec16-165">Exempel:</span><span class="sxs-lookup"><span data-stu-id="7ec16-165">Example:</span></span> 

    $locations = @(@{"locationName"="West US"; "failoverPriority"=0}, @{"locationName"="East US"; "failoverPriority"=1})
    $iprangefilter = ""
    $consistencyPolicy = @{"defaultConsistencyLevel"="BoundedStaleness"; "maxIntervalInSeconds"=5; "maxStalenessPrefix"=100}
    $CosmosDBProperties = @{"databaseAccountOfferType"="Standard"; "locations"=$locations; "consistencyPolicy"=$consistencyPolicy; "ipRangeFilter"=$iprangefilter}
    Set-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test" -Properties $CosmosDBProperties

## <span data-ttu-id="7ec16-166"><a id="delete-documentdb-account-powershell"></a>Ta bort en DocumentDB-databaskonto</span><span class="sxs-lookup"><span data-stu-id="7ec16-166"><a id="delete-documentdb-account-powershell"></a> Delete a DocumentDB Database Account</span></span>

<span data-ttu-id="7ec16-167">Det här kommandot kan du toodelete ett befintligt konto i Azure DB som Cosmos-databasen.</span><span class="sxs-lookup"><span data-stu-id="7ec16-167">This command allows you toodelete an existing Azure Cosmos DB database account.</span></span>

    Remove-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>"
    
* <span data-ttu-id="7ec16-168">`<resource-group-name>`hello namnet på hello [Azure-resursgrupp] [ azure-resource-groups] toowhich hello nya Azure Cosmos DB databaskonto tillhör.</span><span class="sxs-lookup"><span data-stu-id="7ec16-168">`<resource-group-name>` hello name of hello [Azure Resource Group][azure-resource-groups] toowhich hello new Azure Cosmos DB database account belongs to.</span></span>
* <span data-ttu-id="7ec16-169">`<database-account-name>`hello namnet på hello Azure Cosmos DB databasen konto toobe tas bort.</span><span class="sxs-lookup"><span data-stu-id="7ec16-169">`<database-account-name>` hello name of hello Azure Cosmos DB database account toobe deleted.</span></span>

<span data-ttu-id="7ec16-170">Exempel:</span><span class="sxs-lookup"><span data-stu-id="7ec16-170">Example:</span></span>

    Remove-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test"

## <span data-ttu-id="7ec16-171"><a id="get-documentdb-properties-powershell"></a>Hämta egenskaper för ett DocumentDB-databaskonto</span><span class="sxs-lookup"><span data-stu-id="7ec16-171"><a id="get-documentdb-properties-powershell"></a> Get Properties of a DocumentDB Database Account</span></span>

<span data-ttu-id="7ec16-172">Det här kommandot kan du tooget hello egenskaper för ett befintligt konto i Azure DB som Cosmos-databasen.</span><span class="sxs-lookup"><span data-stu-id="7ec16-172">This command allows you tooget hello properties of an existing Azure Cosmos DB database account.</span></span>

    Get-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>"

* <span data-ttu-id="7ec16-173">`<resource-group-name>`hello namnet på hello [Azure-resursgrupp] [ azure-resource-groups] toowhich hello nya Azure Cosmos DB databaskonto tillhör.</span><span class="sxs-lookup"><span data-stu-id="7ec16-173">`<resource-group-name>` hello name of hello [Azure Resource Group][azure-resource-groups] toowhich hello new Azure Cosmos DB database account belongs to.</span></span>
* <span data-ttu-id="7ec16-174">`<database-account-name>`hello namnet på hello Azure Cosmos DB konto.</span><span class="sxs-lookup"><span data-stu-id="7ec16-174">`<database-account-name>` hello name of hello Azure Cosmos DB database account.</span></span>

<span data-ttu-id="7ec16-175">Exempel:</span><span class="sxs-lookup"><span data-stu-id="7ec16-175">Example:</span></span>

    Get-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test"

## <span data-ttu-id="7ec16-176"><a id="update-tags-powershell"></a>Uppdatera taggar för ett konto för Azure Cosmos DB-databas</span><span class="sxs-lookup"><span data-stu-id="7ec16-176"><a id="update-tags-powershell"></a> Update Tags of an Azure Cosmos DB Database Account</span></span>

<span data-ttu-id="7ec16-177">hello följande exempel beskrivs hur tooset [Azure resurstaggar] [ azure-resource-tags] Azure Cosmos-DB databasen i kontot.</span><span class="sxs-lookup"><span data-stu-id="7ec16-177">hello following example describes how tooset [Azure resource tags][azure-resource-tags] for your Azure Cosmos DB database account.</span></span>

> [!NOTE]
> <span data-ttu-id="7ec16-178">Det här kommandot kan kombineras med hello skapa eller uppdatera kommandon genom att lägga till hello `-Tags` flagga hello motsvarande parameter.</span><span class="sxs-lookup"><span data-stu-id="7ec16-178">This command can be combined with hello create or update commands by appending hello `-Tags` flag with hello corresponding parameter.</span></span>

<span data-ttu-id="7ec16-179">Exempel:</span><span class="sxs-lookup"><span data-stu-id="7ec16-179">Example:</span></span>

    $tags = @{"dept" = "Finance”; environment = “Production”}
    Set-AzureRmResource -ResourceType “Microsoft.DocumentDB/databaseAccounts”  -ResourceGroupName "rg-test" -Name "docdb-test" -Tags $tags

## <span data-ttu-id="7ec16-180"><a id="list-account-keys-powershell"></a>Lista nycklar</span><span class="sxs-lookup"><span data-stu-id="7ec16-180"><a id="list-account-keys-powershell"></a> List Account Keys</span></span>

<span data-ttu-id="7ec16-181">När du skapar ett konto i Azure Cosmos DB genererar hello tjänsten två master åtkomstnycklar som kan användas för autentisering när hello Azure Cosmos DB konto används.</span><span class="sxs-lookup"><span data-stu-id="7ec16-181">When you create an Azure Cosmos DB account, hello service generates two master access keys that can be used for authentication when hello Azure Cosmos DB account is accessed.</span></span> <span data-ttu-id="7ec16-182">Genom att tillhandahålla två åtkomstnycklar kan Azure Cosmos DB du tooregenerate hello nycklarna utan avbrott tooyour Azure DB som Cosmos-konto.</span><span class="sxs-lookup"><span data-stu-id="7ec16-182">By providing two access keys, Azure Cosmos DB enables you tooregenerate hello keys with no interruption tooyour Azure Cosmos DB account.</span></span> <span data-ttu-id="7ec16-183">Det finns också skrivskyddade nycklar för att autentisera skrivskyddade åtgärder.</span><span class="sxs-lookup"><span data-stu-id="7ec16-183">Read-only keys for authenticating read-only operations are also available.</span></span> <span data-ttu-id="7ec16-184">Det finns två skrivskyddad (primär eller sekundär) och två skrivskyddade nycklar (primär eller sekundär).</span><span class="sxs-lookup"><span data-stu-id="7ec16-184">There are two read-write keys (primary and secondary) and two read-only keys (primary and secondary).</span></span>

    $keys = Invoke-AzureRmResourceAction -Action listKeys -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>"

* <span data-ttu-id="7ec16-185">`<resource-group-name>`hello namnet på hello [Azure-resursgrupp] [ azure-resource-groups] toowhich hello nya Azure Cosmos DB databaskonto tillhör.</span><span class="sxs-lookup"><span data-stu-id="7ec16-185">`<resource-group-name>` hello name of hello [Azure Resource Group][azure-resource-groups] toowhich hello new Azure Cosmos DB database account belongs to.</span></span>
* <span data-ttu-id="7ec16-186">`<database-account-name>`hello namnet på hello Azure Cosmos DB konto.</span><span class="sxs-lookup"><span data-stu-id="7ec16-186">`<database-account-name>` hello name of hello Azure Cosmos DB database account.</span></span>

<span data-ttu-id="7ec16-187">Exempel:</span><span class="sxs-lookup"><span data-stu-id="7ec16-187">Example:</span></span>

    $keys = Invoke-AzureRmResourceAction -Action listKeys -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test"

## <span data-ttu-id="7ec16-188"><a id="list-connection-strings-powershell"></a>Lista över anslutningssträngar</span><span class="sxs-lookup"><span data-stu-id="7ec16-188"><a id="list-connection-strings-powershell"></a> List Connection Strings</span></span>

<span data-ttu-id="7ec16-189">Hej anslutning sträng tooconnect MongoDB för app toohello databaskonto kan hämtas med hello följande kommando för MongoDB-konton.</span><span class="sxs-lookup"><span data-stu-id="7ec16-189">For MongoDB accounts, hello connection string tooconnect your MongoDB app toohello database account can be retrieved using hello following command.</span></span>

    $keys = Invoke-AzureRmResourceAction -Action listConnectionStrings -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>"

* <span data-ttu-id="7ec16-190">`<resource-group-name>`hello namnet på hello [Azure-resursgrupp] [ azure-resource-groups] toowhich hello nya Azure Cosmos DB databaskonto tillhör.</span><span class="sxs-lookup"><span data-stu-id="7ec16-190">`<resource-group-name>` hello name of hello [Azure Resource Group][azure-resource-groups] toowhich hello new Azure Cosmos DB database account belongs to.</span></span>
* <span data-ttu-id="7ec16-191">`<database-account-name>`hello namnet på hello Azure Cosmos DB konto.</span><span class="sxs-lookup"><span data-stu-id="7ec16-191">`<database-account-name>` hello name of hello Azure Cosmos DB database account.</span></span>

<span data-ttu-id="7ec16-192">Exempel:</span><span class="sxs-lookup"><span data-stu-id="7ec16-192">Example:</span></span>

    $keys = Invoke-AzureRmResourceAction -Action listConnectionStrings -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test"

## <span data-ttu-id="7ec16-193"><a id="regenerate-account-key-powershell"></a>Återskapa Kontonyckel</span><span class="sxs-lookup"><span data-stu-id="7ec16-193"><a id="regenerate-account-key-powershell"></a> Regenerate Account Key</span></span>

<span data-ttu-id="7ec16-194">Du bör ändra hello nycklar tooyour Azure Cosmos DB åtkomstkonto regelbundet toohelp skydda dina anslutningar.</span><span class="sxs-lookup"><span data-stu-id="7ec16-194">You should change hello access keys tooyour Azure Cosmos DB account periodically toohelp keep your connections more secure.</span></span> <span data-ttu-id="7ec16-195">Två åtkomstnycklar tilldelas tooenable du toomaintain anslutningar toohello Azure DB som Cosmos-konto med hjälp av ena åtkomstnyckeln medan du återskapar hello andra snabbtangent.</span><span class="sxs-lookup"><span data-stu-id="7ec16-195">Two access keys are assigned tooenable you toomaintain connections toohello Azure Cosmos DB account using one access key while you regenerate hello other access key.</span></span>

    Invoke-AzureRmResourceAction -Action regenerateKey -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>" -Parameters @{"keyKind"="<key-kind>"}

* <span data-ttu-id="7ec16-196">`<resource-group-name>`hello namnet på hello [Azure-resursgrupp] [ azure-resource-groups] toowhich hello nya Azure Cosmos DB databaskonto tillhör.</span><span class="sxs-lookup"><span data-stu-id="7ec16-196">`<resource-group-name>` hello name of hello [Azure Resource Group][azure-resource-groups] toowhich hello new Azure Cosmos DB database account belongs to.</span></span>
* <span data-ttu-id="7ec16-197">`<database-account-name>`hello namnet på hello Azure Cosmos DB konto.</span><span class="sxs-lookup"><span data-stu-id="7ec16-197">`<database-account-name>` hello name of hello Azure Cosmos DB database account.</span></span>
* <span data-ttu-id="7ec16-198">`<key-kind>`Hello fyra typer av nycklar: [”primär” | ” Sekundär ”|” PrimaryReadonly ”|” SecondaryReadonly ”] som du vill att tooregenerate.</span><span class="sxs-lookup"><span data-stu-id="7ec16-198">`<key-kind>` One of hello four types of keys: ["Primary"|"Secondary"|"PrimaryReadonly"|"SecondaryReadonly"] that you would like tooregenerate.</span></span>

<span data-ttu-id="7ec16-199">Exempel:</span><span class="sxs-lookup"><span data-stu-id="7ec16-199">Example:</span></span>

    Invoke-AzureRmResourceAction -Action regenerateKey -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test" -Parameters @{"keyKind"="Primary"}

## <span data-ttu-id="7ec16-200"><a id="modify-failover-priority-powershell"></a>Ändra redundans prioriteten för en Azure Cosmos DB-databaskonto</span><span class="sxs-lookup"><span data-stu-id="7ec16-200"><a id="modify-failover-priority-powershell"></a> Modify Failover Priority of an Azure Cosmos DB Database Account</span></span>

<span data-ttu-id="7ec16-201">Du kan ändra hello redundans prioriteten för hello olika regioner som hello Azure Cosmos DB konto finns i för flera regioner databasen konton.</span><span class="sxs-lookup"><span data-stu-id="7ec16-201">For multi-region database accounts, you can change hello failover priority of hello various regions which hello Azure Cosmos DB database account exists in.</span></span> <span data-ttu-id="7ec16-202">Mer information om växling vid fel i din Azure Cosmos DB databaskonto finns [distribuera data globalt med Azure Cosmos DB][distribute-data-globally].</span><span class="sxs-lookup"><span data-stu-id="7ec16-202">For more information on failover in your Azure Cosmos DB database account, see [Distribute data globally with Azure Cosmos DB][distribute-data-globally].</span></span>

    $failoverPolicies = @(@{"locationName"="<write-region-location>"; "failoverPriority"=0},@{"locationName"="<read-region-location>"; "failoverPriority"=1})
    Invoke-AzureRmResourceAction -Action failoverPriorityChange -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>" -Parameters @{"failoverPolicies"=$failoverPolicies}

* <span data-ttu-id="7ec16-203">`<write-region-location>`namn på hello hello skriva regionens hello konto.</span><span class="sxs-lookup"><span data-stu-id="7ec16-203">`<write-region-location>` hello location name of hello write region of hello database account.</span></span> <span data-ttu-id="7ec16-204">Den här platsen är nödvändig toohave redundans prioriteringsvärdet 0.</span><span class="sxs-lookup"><span data-stu-id="7ec16-204">This location is required toohave a failover priority value of 0.</span></span> <span data-ttu-id="7ec16-205">Det måste finnas exakt en skrivning region per konto.</span><span class="sxs-lookup"><span data-stu-id="7ec16-205">There must be exactly one write region per database account.</span></span>
* <span data-ttu-id="7ec16-206">`<read-region-location>`namn på hello hello läsa regionens hello databaskonto.</span><span class="sxs-lookup"><span data-stu-id="7ec16-206">`<read-region-location>` hello location name of hello read region of hello database account.</span></span> <span data-ttu-id="7ec16-207">Den här platsen är nödvändig toohave ett prioritetsvärde för växling vid fel som är större än 0.</span><span class="sxs-lookup"><span data-stu-id="7ec16-207">This location is required toohave a failover priority value of greater than 0.</span></span> <span data-ttu-id="7ec16-208">Det kan finnas mer än en skrivskyddad regioner per konto.</span><span class="sxs-lookup"><span data-stu-id="7ec16-208">There can be more than one read regions per database account.</span></span>
* <span data-ttu-id="7ec16-209">`<resource-group-name>`hello namnet på hello [Azure-resursgrupp] [ azure-resource-groups] toowhich hello nya Azure Cosmos DB databaskonto tillhör.</span><span class="sxs-lookup"><span data-stu-id="7ec16-209">`<resource-group-name>` hello name of hello [Azure Resource Group][azure-resource-groups] toowhich hello new Azure Cosmos DB database account belongs to.</span></span>
* <span data-ttu-id="7ec16-210">`<database-account-name>`hello namnet på hello Azure Cosmos DB konto.</span><span class="sxs-lookup"><span data-stu-id="7ec16-210">`<database-account-name>` hello name of hello Azure Cosmos DB database account.</span></span>

<span data-ttu-id="7ec16-211">Exempel:</span><span class="sxs-lookup"><span data-stu-id="7ec16-211">Example:</span></span>

    $failoverPolicies = @(@{"locationName"="East US"; "failoverPriority"=0},@{"locationName"="West US"; "failoverPriority"=1})
    Invoke-AzureRmResourceAction -Action failoverPriorityChange -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test" -Parameters @{"failoverPolicies"=$failoverPolicies}

## <a name="next-steps"></a><span data-ttu-id="7ec16-212">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7ec16-212">Next steps</span></span>

* <span data-ttu-id="7ec16-213">tooconnect med hjälp av .NET, se [ansluter och frågar med .NET](create-documentdb-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="7ec16-213">tooconnect using .NET, see [Connect and query with .NET](create-documentdb-dotnet.md).</span></span>
* <span data-ttu-id="7ec16-214">tooconnect med .NET Core finns [Anslut och fråga med .NET Core](create-documentdb-dotnet-core.md).</span><span class="sxs-lookup"><span data-stu-id="7ec16-214">tooconnect using .NET Core, see [Connect and query with .NET Core](create-documentdb-dotnet-core.md).</span></span>
* <span data-ttu-id="7ec16-215">tooconnect med Node.js, se [ansluter och frågar med Node.js och en MongoDB-app](create-mongodb-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="7ec16-215">tooconnect using Node.js, see [Connect and query with Node.js and a MongoDB app](create-mongodb-nodejs.md).</span></span>

<!--Reference style links - using these makes hello source content way more readable than using inline links-->
[powershell-install-configure]: https://docs.microsoft.com/azure/powershell-install-configure
[scaling-globally]: distribute-data-globally.md#EnableGlobalDistribution
[distribute-data-globally]: distribute-data-globally.md
[azure-resource-groups]: https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups
[azure-resource-tags]: https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags
[rp-rest-api]: https://docs.microsoft.com/rest/api/documentdbresourceprovider/
