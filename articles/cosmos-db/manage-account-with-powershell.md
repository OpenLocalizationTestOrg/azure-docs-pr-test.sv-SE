---
title: Azure Cosmos DB Automation - hantering med Powershell | Microsoft Docs
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
ms.openlocfilehash: 25c543528119410dff0684845a713dcb0d6151d6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-azure-cosmos-db-account-using-powershell"></a><span data-ttu-id="88acd-103">Skapa ett Azure DB som Cosmos-konto med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="88acd-103">Create an Azure Cosmos DB account using PowerShell</span></span>

<span data-ttu-id="88acd-104">Enligt följande anvisningar beskriver kommandon för att automatisera hanteringen av dina Azure Cosmos DB databasen konton med hjälp av Azure Powershell.</span><span class="sxs-lookup"><span data-stu-id="88acd-104">The following guide describes commands to automate management of your Azure Cosmos DB database accounts using Azure Powershell.</span></span> <span data-ttu-id="88acd-105">Den inkluderar också kommandon för att hantera nycklar och prioriteringar för växling vid fel i [flera regioner databasen konton][scaling-globally].</span><span class="sxs-lookup"><span data-stu-id="88acd-105">It also includes commands to manage account keys and failover priorities in [multi-region database accounts][scaling-globally].</span></span> <span data-ttu-id="88acd-106">Uppdatera ditt konto kan du ändra konsekvenskontroll principer och Lägg till/ta bort regioner.</span><span class="sxs-lookup"><span data-stu-id="88acd-106">Updating your database account allows you to modify consistency policies and add/remove regions.</span></span> <span data-ttu-id="88acd-107">Plattformsoberoende hantering av Azure DB som Cosmos-konto, kan använda antingen [Azure CLI](cli-samples.md), [Resource Provider REST API][rp-rest-api], eller [Azure-portalen ](create-documentdb-dotnet.md#create-account).</span><span class="sxs-lookup"><span data-stu-id="88acd-107">For cross-platform management of your Azure Cosmos DB account, you can use either [Azure CLI](cli-samples.md), the [Resource Provider REST API][rp-rest-api], or the [Azure portal](create-documentdb-dotnet.md#create-account).</span></span>

## <a name="getting-started"></a><span data-ttu-id="88acd-108">Komma igång</span><span class="sxs-lookup"><span data-stu-id="88acd-108">Getting Started</span></span>

<span data-ttu-id="88acd-109">Följ instruktionerna i [hur du installerar och konfigurerar du Azure PowerShell] [ powershell-install-configure] att installera och logga in på ditt Azure Resource Manager-konto i Powershell.</span><span class="sxs-lookup"><span data-stu-id="88acd-109">Follow the instructions in [How to install and configure Azure PowerShell][powershell-install-configure] to install and log in to your Azure Resource Manager account in Powershell.</span></span>

### <a name="notes"></a><span data-ttu-id="88acd-110">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="88acd-110">Notes</span></span>

* <span data-ttu-id="88acd-111">Om du vill köra följande kommandon utan bekräftelse från användaren, Lägg till den `-Force` flaggan i kommandot.</span><span class="sxs-lookup"><span data-stu-id="88acd-111">If you would like to execute the following commands without requiring user confirmation, append the `-Force` flag to the command.</span></span>
* <span data-ttu-id="88acd-112">Följande kommandon är synkrona.</span><span class="sxs-lookup"><span data-stu-id="88acd-112">All the following commands are synchronous.</span></span>

## <span data-ttu-id="88acd-113"><a id="create-documentdb-account-powershell"></a>Skapa ett Azure Cosmos DB-konto</span><span class="sxs-lookup"><span data-stu-id="88acd-113"><a id="create-documentdb-account-powershell"></a> Create an Azure Cosmos DB Account</span></span>

<span data-ttu-id="88acd-114">Det här kommandot kan du skapa ett databaskonto Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="88acd-114">This command allows you to create an Azure Cosmos DB database account.</span></span> <span data-ttu-id="88acd-115">Konfigurera ditt nya konto som antingen en region eller [flera regioner] [ scaling-globally] med en viss [konsekvent princip](consistency-levels.md).</span><span class="sxs-lookup"><span data-stu-id="88acd-115">Configure your new database account as either single-region or [multi-region][scaling-globally] with a certain [consistency policy](consistency-levels.md).</span></span>

    $locations = @(@{"locationName"="<write-region-location>"; "failoverPriority"=0}, @{"locationName"="<read-region-location>"; "failoverPriority"=1})
    $iprangefilter = "<ip-range-filter>"
    $consistencyPolicy = @{"defaultConsistencyLevel"="<default-consistency-level>"; "maxIntervalInSeconds"="<max-interval>"; "maxStalenessPrefix"="<max-staleness-prefix>"}
    $CosmosDBProperties = @{"databaseAccountOfferType"="Standard"; "locations"=$locations; "consistencyPolicy"=$consistencyPolicy; "ipRangeFilter"=$iprangefilter}
    New-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName <resource-group-name>  -Location "<resource-group-location>" -Name <database-account-name> -Properties $CosmosDBProperties
    
* <span data-ttu-id="88acd-116">`<write-region-location>`Platsnamnet för databaskontot skrivåtgärder regionen.</span><span class="sxs-lookup"><span data-stu-id="88acd-116">`<write-region-location>` The location name of the write region of the database account.</span></span> <span data-ttu-id="88acd-117">Den här platsen måste ha ett värde för växling vid fel prioritet 0.</span><span class="sxs-lookup"><span data-stu-id="88acd-117">This location is required to have a failover priority value of 0.</span></span> <span data-ttu-id="88acd-118">Det måste finnas exakt en skrivning region per konto.</span><span class="sxs-lookup"><span data-stu-id="88acd-118">There must be exactly one write region per database account.</span></span>
* <span data-ttu-id="88acd-119">`<read-region-location>`Platsnamnet för den skrivskyddade regionen för databaskontot.</span><span class="sxs-lookup"><span data-stu-id="88acd-119">`<read-region-location>` The location name of the read region of the database account.</span></span> <span data-ttu-id="88acd-120">Den här platsen måste ha ett värde för prioritet för växling vid fel som är större än 0.</span><span class="sxs-lookup"><span data-stu-id="88acd-120">This location is required to have a failover priority value of greater than 0.</span></span> <span data-ttu-id="88acd-121">Det kan finnas mer än en skrivskyddad regioner per konto.</span><span class="sxs-lookup"><span data-stu-id="88acd-121">There can be more than one read regions per database account.</span></span>
* <span data-ttu-id="88acd-122">`<ip-range-filter>`Anger uppsättningen IP-adresser och IP-adressintervall i CIDR-format ska ingå som listan över tillåtna klientens IP-adresser för en viss databas-konto.</span><span class="sxs-lookup"><span data-stu-id="88acd-122">`<ip-range-filter>` Specifies the set of IP addresses or IP address ranges in CIDR form to be included as the allowed list of client IPs for a given database account.</span></span> <span data-ttu-id="88acd-123">IP-adressintervall måste vara kommatecken avgränsade och får inte innehålla blanksteg.</span><span class="sxs-lookup"><span data-stu-id="88acd-123">IP addresses/ranges must be comma separated and must not contain any spaces.</span></span> <span data-ttu-id="88acd-124">Mer information finns i [Azure Cosmos DB-brandväggsstöd](firewall-support.md)</span><span class="sxs-lookup"><span data-stu-id="88acd-124">For more information, see [Azure Cosmos DB Firewall Support](firewall-support.md)</span></span>
* <span data-ttu-id="88acd-125">`<default-consistency-level>`Konsekvenskontroll standardnivå för Azure DB som Cosmos-konto.</span><span class="sxs-lookup"><span data-stu-id="88acd-125">`<default-consistency-level>` The default consistency level of the Azure Cosmos DB account.</span></span> <span data-ttu-id="88acd-126">Mer information finns i [Konsekvensnivåer i Azure Cosmos DB](consistency-levels.md).</span><span class="sxs-lookup"><span data-stu-id="88acd-126">For more information, see [Consistency Levels in Azure Cosmos DB](consistency-levels.md).</span></span>
* <span data-ttu-id="88acd-127">`<max-interval>`När det används med begränsas föråldringskonsekvens representerar det här värdet tid mängden föråldrad (i sekunder) tolereras.</span><span class="sxs-lookup"><span data-stu-id="88acd-127">`<max-interval>` When used with Bounded Staleness consistency, this value represents the time amount of staleness (in seconds) tolerated.</span></span> <span data-ttu-id="88acd-128">Tillåtet område för det här värdet är 1-100.</span><span class="sxs-lookup"><span data-stu-id="88acd-128">Accepted range for this value is 1 - 100.</span></span>
* <span data-ttu-id="88acd-129">`<max-staleness-prefix>`När det används med begränsas föråldringskonsekvens representerar det här värdet antalet inaktuella begäranden tolereras.</span><span class="sxs-lookup"><span data-stu-id="88acd-129">`<max-staleness-prefix>` When used with Bounded Staleness consistency, this value represents the number of stale requests tolerated.</span></span> <span data-ttu-id="88acd-130">Tillåtet område för det här värdet är 1 – 2 147 483 647.</span><span class="sxs-lookup"><span data-stu-id="88acd-130">Accepted range for this value is 1 – 2,147,483,647.</span></span>
* <span data-ttu-id="88acd-131">`<resource-group-name>`Namnet på den [Azure-resursgrupp] [ azure-resource-groups] som det nya kontot i Azure Cosmos DB databasen tillhör.</span><span class="sxs-lookup"><span data-stu-id="88acd-131">`<resource-group-name>` The name of the [Azure Resource Group][azure-resource-groups] to which the new Azure Cosmos DB database account belongs to.</span></span>
* <span data-ttu-id="88acd-132">`<resource-group-location>`Platsen för Azure-resursgrupp som det nya kontot i Azure Cosmos DB databasen tillhör.</span><span class="sxs-lookup"><span data-stu-id="88acd-132">`<resource-group-location>` The location of the Azure Resource Group to which the new Azure Cosmos DB database account belongs to.</span></span>
* <span data-ttu-id="88acd-133">`<database-account-name>`Namnet på kontot som ska skapas Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="88acd-133">`<database-account-name>` The name of the Azure Cosmos DB database account to be created.</span></span> <span data-ttu-id="88acd-134">Det kan endast använda gemena bokstäver, siffror, den '-' tecken och måste vara mellan 3 och 50 tecken.</span><span class="sxs-lookup"><span data-stu-id="88acd-134">It can only use lowercase letters, numbers, the '-' character, and must be between 3 and 50 characters.</span></span>

<span data-ttu-id="88acd-135">Exempel:</span><span class="sxs-lookup"><span data-stu-id="88acd-135">Example:</span></span> 

    $locations = @(@{"locationName"="West US"; "failoverPriority"=0}, @{"locationName"="East US"; "failoverPriority"=1})
    $iprangefilter = ""
    $consistencyPolicy = @{"defaultConsistencyLevel"="BoundedStaleness"; "maxIntervalInSeconds"=5; "maxStalenessPrefix"=100}
    $CosmosDBProperties = @{"databaseAccountOfferType"="Standard"; "locations"=$locations; "consistencyPolicy"=$consistencyPolicy; "ipRangeFilter"=$iprangefilter}
    New-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Location "West US" -Name "docdb-test" -Properties $CosmosDBProperties

### <a name="notes"></a><span data-ttu-id="88acd-136">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="88acd-136">Notes</span></span>
* <span data-ttu-id="88acd-137">I föregående exempel skapas ett databaskonto med två områden.</span><span class="sxs-lookup"><span data-stu-id="88acd-137">The preceding example creates a database account with two regions.</span></span> <span data-ttu-id="88acd-138">Det är också möjligt att skapa ett databaskonto med en region (som är utsedd till skrivåtgärder region och har ett prioritetsvärde för växling vid fel 0) eller fler än två regioner.</span><span class="sxs-lookup"><span data-stu-id="88acd-138">It is also possible to create a database account with either one region (which is designated as the write region and have a failover priority value of 0) or more than two regions.</span></span> <span data-ttu-id="88acd-139">Mer information finns i [flera regioner databasen konton][scaling-globally].</span><span class="sxs-lookup"><span data-stu-id="88acd-139">For more information, see [multi-region database accounts][scaling-globally].</span></span>
* <span data-ttu-id="88acd-140">Platser måste vara regioner som Azure Cosmos DB är allmänt tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="88acd-140">The locations must be regions in which Azure Cosmos DB is generally available.</span></span> <span data-ttu-id="88acd-141">Den aktuella listan över regioner finns på den [Azure-regioner sidan](https://azure.microsoft.com/regions/#services).</span><span class="sxs-lookup"><span data-stu-id="88acd-141">The current list of regions is provided on the [Azure Regions page](https://azure.microsoft.com/regions/#services).</span></span>

## <span data-ttu-id="88acd-142"><a id="update-documentdb-account-powershell"></a>Uppdatera ett DocumentDB-databaskonto</span><span class="sxs-lookup"><span data-stu-id="88acd-142"><a id="update-documentdb-account-powershell"></a> Update a DocumentDB Database Account</span></span>

<span data-ttu-id="88acd-143">Det här kommandot kan du uppdatera kontoegenskaperna Azure DB som Cosmos-databasen.</span><span class="sxs-lookup"><span data-stu-id="88acd-143">This command allows you to update your Azure Cosmos DB database account properties.</span></span> <span data-ttu-id="88acd-144">Detta inkluderar konsekvenskontroll principen och de platser som kontot som finns i.</span><span class="sxs-lookup"><span data-stu-id="88acd-144">This includes the consistency policy and the locations which the database account exists in.</span></span>

> [!NOTE]
> <span data-ttu-id="88acd-145">Det här kommandot kan du lägga till och ta bort regioner, men tillåter inte att ändra prioriteringarna för växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="88acd-145">This command allows you to add and remove regions but does not allow you to modify failover priorities.</span></span> <span data-ttu-id="88acd-146">Om du vill ändra prioriteringarna för växling vid fel finns [under](#modify-failover-priority-powershell).</span><span class="sxs-lookup"><span data-stu-id="88acd-146">To modify failover priorities, see [below](#modify-failover-priority-powershell).</span></span>

    $locations = @(@{"locationName"="<write-region-location>"; "failoverPriority"=0}, @{"locationName"="<read-region-location>"; "failoverPriority"=1})
    $iprangefilter = "<ip-range-filter>"
    $consistencyPolicy = @{"defaultConsistencyLevel"="<default-consistency-level>"; "maxIntervalInSeconds"="<max-interval>"; "maxStalenessPrefix"="<max-staleness-prefix>"}
    $CosmosDBProperties = @{"databaseAccountOfferType"="Standard"; "locations"=$locations; "consistencyPolicy"=$consistencyPolicy; "ipRangeFilter"=$iprangefilter}
    Set-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName <resource-group-name> -Name <database-account-name> -Properties $CosmosDBProperties
    
* <span data-ttu-id="88acd-147">`<write-region-location>`Platsnamnet för databaskontot skrivåtgärder regionen.</span><span class="sxs-lookup"><span data-stu-id="88acd-147">`<write-region-location>` The location name of the write region of the database account.</span></span> <span data-ttu-id="88acd-148">Den här platsen måste ha ett värde för växling vid fel prioritet 0.</span><span class="sxs-lookup"><span data-stu-id="88acd-148">This location is required to have a failover priority value of 0.</span></span> <span data-ttu-id="88acd-149">Det måste finnas exakt en skrivning region per konto.</span><span class="sxs-lookup"><span data-stu-id="88acd-149">There must be exactly one write region per database account.</span></span>
* <span data-ttu-id="88acd-150">`<read-region-location>`Platsnamnet för den skrivskyddade regionen för databaskontot.</span><span class="sxs-lookup"><span data-stu-id="88acd-150">`<read-region-location>` The location name of the read region of the database account.</span></span> <span data-ttu-id="88acd-151">Den här platsen måste ha ett värde för prioritet för växling vid fel som är större än 0.</span><span class="sxs-lookup"><span data-stu-id="88acd-151">This location is required to have a failover priority value of greater than 0.</span></span> <span data-ttu-id="88acd-152">Det kan finnas mer än en skrivskyddad regioner per konto.</span><span class="sxs-lookup"><span data-stu-id="88acd-152">There can be more than one read regions per database account.</span></span>
* <span data-ttu-id="88acd-153">`<default-consistency-level>`Konsekvenskontroll standardnivå för Azure DB som Cosmos-konto.</span><span class="sxs-lookup"><span data-stu-id="88acd-153">`<default-consistency-level>` The default consistency level of the Azure Cosmos DB account.</span></span> <span data-ttu-id="88acd-154">Mer information finns i [Konsekvensnivåer i Azure Cosmos DB](consistency-levels.md).</span><span class="sxs-lookup"><span data-stu-id="88acd-154">For more information, see [Consistency Levels in Azure Cosmos DB](consistency-levels.md).</span></span>
* <span data-ttu-id="88acd-155">`<ip-range-filter>`Anger uppsättningen IP-adresser och IP-adressintervall i CIDR-format ska ingå som listan över tillåtna klientens IP-adresser för en viss databas-konto.</span><span class="sxs-lookup"><span data-stu-id="88acd-155">`<ip-range-filter>` Specifies the set of IP addresses or IP address ranges in CIDR form to be included as the allowed list of client IPs for a given database account.</span></span> <span data-ttu-id="88acd-156">IP-adressintervall måste vara kommatecken avgränsade och får inte innehålla blanksteg.</span><span class="sxs-lookup"><span data-stu-id="88acd-156">IP addresses/ranges must be comma separated and must not contain any spaces.</span></span> <span data-ttu-id="88acd-157">Mer information finns i [Azure Cosmos DB-brandväggsstöd](firewall-support.md)</span><span class="sxs-lookup"><span data-stu-id="88acd-157">For more information, see [Azure Cosmos DB Firewall Support](firewall-support.md)</span></span>
* <span data-ttu-id="88acd-158">`<max-interval>`När det används med begränsas föråldringskonsekvens representerar det här värdet tid mängden föråldrad (i sekunder) tolereras.</span><span class="sxs-lookup"><span data-stu-id="88acd-158">`<max-interval>` When used with Bounded Staleness consistency, this value represents the time amount of staleness (in seconds) tolerated.</span></span> <span data-ttu-id="88acd-159">Tillåtet område för det här värdet är 1-100.</span><span class="sxs-lookup"><span data-stu-id="88acd-159">Accepted range for this value is 1 - 100.</span></span>
* <span data-ttu-id="88acd-160">`<max-staleness-prefix>`När det används med begränsas föråldringskonsekvens representerar det här värdet antalet inaktuella begäranden tolereras.</span><span class="sxs-lookup"><span data-stu-id="88acd-160">`<max-staleness-prefix>` When used with Bounded Staleness consistency, this value represents the number of stale requests tolerated.</span></span> <span data-ttu-id="88acd-161">Tillåtet område för det här värdet är 1 – 2 147 483 647.</span><span class="sxs-lookup"><span data-stu-id="88acd-161">Accepted range for this value is 1 – 2,147,483,647.</span></span>
* <span data-ttu-id="88acd-162">`<resource-group-name>`Namnet på den [Azure-resursgrupp] [ azure-resource-groups] som det nya kontot i Azure Cosmos DB databasen tillhör.</span><span class="sxs-lookup"><span data-stu-id="88acd-162">`<resource-group-name>` The name of the [Azure Resource Group][azure-resource-groups] to which the new Azure Cosmos DB database account belongs to.</span></span>
* <span data-ttu-id="88acd-163">`<resource-group-location>`Platsen för Azure-resursgrupp som det nya kontot i Azure Cosmos DB databasen tillhör.</span><span class="sxs-lookup"><span data-stu-id="88acd-163">`<resource-group-location>` The location of the Azure Resource Group to which the new Azure Cosmos DB database account belongs to.</span></span>
* <span data-ttu-id="88acd-164">`<database-account-name>`Namnet på kontot som ska uppdateras Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="88acd-164">`<database-account-name>` The name of the Azure Cosmos DB database account to be updated.</span></span>

<span data-ttu-id="88acd-165">Exempel:</span><span class="sxs-lookup"><span data-stu-id="88acd-165">Example:</span></span> 

    $locations = @(@{"locationName"="West US"; "failoverPriority"=0}, @{"locationName"="East US"; "failoverPriority"=1})
    $iprangefilter = ""
    $consistencyPolicy = @{"defaultConsistencyLevel"="BoundedStaleness"; "maxIntervalInSeconds"=5; "maxStalenessPrefix"=100}
    $CosmosDBProperties = @{"databaseAccountOfferType"="Standard"; "locations"=$locations; "consistencyPolicy"=$consistencyPolicy; "ipRangeFilter"=$iprangefilter}
    Set-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test" -Properties $CosmosDBProperties

## <span data-ttu-id="88acd-166"><a id="delete-documentdb-account-powershell"></a>Ta bort en DocumentDB-databaskonto</span><span class="sxs-lookup"><span data-stu-id="88acd-166"><a id="delete-documentdb-account-powershell"></a> Delete a DocumentDB Database Account</span></span>

<span data-ttu-id="88acd-167">Det här kommandot kan du ta bort ett befintligt konto i Azure DB som Cosmos-databasen.</span><span class="sxs-lookup"><span data-stu-id="88acd-167">This command allows you to delete an existing Azure Cosmos DB database account.</span></span>

    Remove-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>"
    
* <span data-ttu-id="88acd-168">`<resource-group-name>`Namnet på den [Azure-resursgrupp] [ azure-resource-groups] som det nya kontot i Azure Cosmos DB databasen tillhör.</span><span class="sxs-lookup"><span data-stu-id="88acd-168">`<resource-group-name>` The name of the [Azure Resource Group][azure-resource-groups] to which the new Azure Cosmos DB database account belongs to.</span></span>
* <span data-ttu-id="88acd-169">`<database-account-name>`Namnet på det Azure Cosmos DB kontot ska tas bort.</span><span class="sxs-lookup"><span data-stu-id="88acd-169">`<database-account-name>` The name of the Azure Cosmos DB database account to be deleted.</span></span>

<span data-ttu-id="88acd-170">Exempel:</span><span class="sxs-lookup"><span data-stu-id="88acd-170">Example:</span></span>

    Remove-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test"

## <span data-ttu-id="88acd-171"><a id="get-documentdb-properties-powershell"></a>Hämta egenskaper för ett DocumentDB-databaskonto</span><span class="sxs-lookup"><span data-stu-id="88acd-171"><a id="get-documentdb-properties-powershell"></a> Get Properties of a DocumentDB Database Account</span></span>

<span data-ttu-id="88acd-172">Det här kommandot kan du hämta egenskaperna för ett befintligt konto i Azure DB som Cosmos-databasen.</span><span class="sxs-lookup"><span data-stu-id="88acd-172">This command allows you to get the properties of an existing Azure Cosmos DB database account.</span></span>

    Get-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>"

* <span data-ttu-id="88acd-173">`<resource-group-name>`Namnet på den [Azure-resursgrupp] [ azure-resource-groups] som det nya kontot i Azure Cosmos DB databasen tillhör.</span><span class="sxs-lookup"><span data-stu-id="88acd-173">`<resource-group-name>` The name of the [Azure Resource Group][azure-resource-groups] to which the new Azure Cosmos DB database account belongs to.</span></span>
* <span data-ttu-id="88acd-174">`<database-account-name>`Namnet på Azure Cosmos DB databaskontot.</span><span class="sxs-lookup"><span data-stu-id="88acd-174">`<database-account-name>` The name of the Azure Cosmos DB database account.</span></span>

<span data-ttu-id="88acd-175">Exempel:</span><span class="sxs-lookup"><span data-stu-id="88acd-175">Example:</span></span>

    Get-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test"

## <span data-ttu-id="88acd-176"><a id="update-tags-powershell"></a>Uppdatera taggar för ett konto för Azure Cosmos DB-databas</span><span class="sxs-lookup"><span data-stu-id="88acd-176"><a id="update-tags-powershell"></a> Update Tags of an Azure Cosmos DB Database Account</span></span>

<span data-ttu-id="88acd-177">I följande exempel beskrivs hur du ställer in [Azure resurstaggar] [ azure-resource-tags] Azure Cosmos-DB databasen i kontot.</span><span class="sxs-lookup"><span data-stu-id="88acd-177">The following example describes how to set [Azure resource tags][azure-resource-tags] for your Azure Cosmos DB database account.</span></span>

> [!NOTE]
> <span data-ttu-id="88acd-178">Det här kommandot kan kombineras med kommandon för att skapa eller uppdatera genom att lägga till den `-Tags` flaggan med motsvarande parameter.</span><span class="sxs-lookup"><span data-stu-id="88acd-178">This command can be combined with the create or update commands by appending the `-Tags` flag with the corresponding parameter.</span></span>

<span data-ttu-id="88acd-179">Exempel:</span><span class="sxs-lookup"><span data-stu-id="88acd-179">Example:</span></span>

    $tags = @{"dept" = "Finance”; environment = “Production”}
    Set-AzureRmResource -ResourceType “Microsoft.DocumentDB/databaseAccounts”  -ResourceGroupName "rg-test" -Name "docdb-test" -Tags $tags

## <span data-ttu-id="88acd-180"><a id="list-account-keys-powershell"></a>Lista nycklar</span><span class="sxs-lookup"><span data-stu-id="88acd-180"><a id="list-account-keys-powershell"></a> List Account Keys</span></span>

<span data-ttu-id="88acd-181">När du skapar ett konto i Azure Cosmos DB genererar tjänsten två master åtkomstnycklar som kan användas för autentisering vid åtkomst av Azure DB som Cosmos-konto.</span><span class="sxs-lookup"><span data-stu-id="88acd-181">When you create an Azure Cosmos DB account, the service generates two master access keys that can be used for authentication when the Azure Cosmos DB account is accessed.</span></span> <span data-ttu-id="88acd-182">Genom att tillhandahålla två åtkomstnycklar kan Azure Cosmos DB du återskapa nycklarna utan avbrott i Azure DB som Cosmos-konto.</span><span class="sxs-lookup"><span data-stu-id="88acd-182">By providing two access keys, Azure Cosmos DB enables you to regenerate the keys with no interruption to your Azure Cosmos DB account.</span></span> <span data-ttu-id="88acd-183">Det finns också skrivskyddade nycklar för att autentisera skrivskyddade åtgärder.</span><span class="sxs-lookup"><span data-stu-id="88acd-183">Read-only keys for authenticating read-only operations are also available.</span></span> <span data-ttu-id="88acd-184">Det finns två skrivskyddad (primär eller sekundär) och två skrivskyddade nycklar (primär eller sekundär).</span><span class="sxs-lookup"><span data-stu-id="88acd-184">There are two read-write keys (primary and secondary) and two read-only keys (primary and secondary).</span></span>

    $keys = Invoke-AzureRmResourceAction -Action listKeys -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>"

* <span data-ttu-id="88acd-185">`<resource-group-name>`Namnet på den [Azure-resursgrupp] [ azure-resource-groups] som det nya kontot i Azure Cosmos DB databasen tillhör.</span><span class="sxs-lookup"><span data-stu-id="88acd-185">`<resource-group-name>` The name of the [Azure Resource Group][azure-resource-groups] to which the new Azure Cosmos DB database account belongs to.</span></span>
* <span data-ttu-id="88acd-186">`<database-account-name>`Namnet på Azure Cosmos DB databaskontot.</span><span class="sxs-lookup"><span data-stu-id="88acd-186">`<database-account-name>` The name of the Azure Cosmos DB database account.</span></span>

<span data-ttu-id="88acd-187">Exempel:</span><span class="sxs-lookup"><span data-stu-id="88acd-187">Example:</span></span>

    $keys = Invoke-AzureRmResourceAction -Action listKeys -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test"

## <span data-ttu-id="88acd-188"><a id="list-connection-strings-powershell"></a>Lista över anslutningssträngar</span><span class="sxs-lookup"><span data-stu-id="88acd-188"><a id="list-connection-strings-powershell"></a> List Connection Strings</span></span>

<span data-ttu-id="88acd-189">För MongoDB-konton, kan du hämta anslutningssträngen till Anslut MongoDB-appen till databaskontot med följande kommando.</span><span class="sxs-lookup"><span data-stu-id="88acd-189">For MongoDB accounts, the connection string to connect your MongoDB app to the database account can be retrieved using the following command.</span></span>

    $keys = Invoke-AzureRmResourceAction -Action listConnectionStrings -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>"

* <span data-ttu-id="88acd-190">`<resource-group-name>`Namnet på den [Azure-resursgrupp] [ azure-resource-groups] som det nya kontot i Azure Cosmos DB databasen tillhör.</span><span class="sxs-lookup"><span data-stu-id="88acd-190">`<resource-group-name>` The name of the [Azure Resource Group][azure-resource-groups] to which the new Azure Cosmos DB database account belongs to.</span></span>
* <span data-ttu-id="88acd-191">`<database-account-name>`Namnet på Azure Cosmos DB databaskontot.</span><span class="sxs-lookup"><span data-stu-id="88acd-191">`<database-account-name>` The name of the Azure Cosmos DB database account.</span></span>

<span data-ttu-id="88acd-192">Exempel:</span><span class="sxs-lookup"><span data-stu-id="88acd-192">Example:</span></span>

    $keys = Invoke-AzureRmResourceAction -Action listConnectionStrings -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test"

## <span data-ttu-id="88acd-193"><a id="regenerate-account-key-powershell"></a>Återskapa Kontonyckel</span><span class="sxs-lookup"><span data-stu-id="88acd-193"><a id="regenerate-account-key-powershell"></a> Regenerate Account Key</span></span>

<span data-ttu-id="88acd-194">Du bör ändra åtkomstnycklarna till ditt Azure DB som Cosmos-konto med jämna mellanrum för att skydda dina anslutningar.</span><span class="sxs-lookup"><span data-stu-id="88acd-194">You should change the access keys to your Azure Cosmos DB account periodically to help keep your connections more secure.</span></span> <span data-ttu-id="88acd-195">Två åtkomstnycklar tilldelas så att du kan upprätthålla anslutningar till Azure Cosmos DB kontot den ena åtkomstnyckeln medan du återskapar den andra åtkomstnyckeln.</span><span class="sxs-lookup"><span data-stu-id="88acd-195">Two access keys are assigned to enable you to maintain connections to the Azure Cosmos DB account using one access key while you regenerate the other access key.</span></span>

    Invoke-AzureRmResourceAction -Action regenerateKey -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>" -Parameters @{"keyKind"="<key-kind>"}

* <span data-ttu-id="88acd-196">`<resource-group-name>`Namnet på den [Azure-resursgrupp] [ azure-resource-groups] som det nya kontot i Azure Cosmos DB databasen tillhör.</span><span class="sxs-lookup"><span data-stu-id="88acd-196">`<resource-group-name>` The name of the [Azure Resource Group][azure-resource-groups] to which the new Azure Cosmos DB database account belongs to.</span></span>
* <span data-ttu-id="88acd-197">`<database-account-name>`Namnet på Azure Cosmos DB databaskontot.</span><span class="sxs-lookup"><span data-stu-id="88acd-197">`<database-account-name>` The name of the Azure Cosmos DB database account.</span></span>
* <span data-ttu-id="88acd-198">`<key-kind>`En av de fyra typerna av nycklar: [”primär” | ” Sekundär ”|” PrimaryReadonly ”|” SecondaryReadonly ”] som du vill återskapa.</span><span class="sxs-lookup"><span data-stu-id="88acd-198">`<key-kind>` One of the four types of keys: ["Primary"|"Secondary"|"PrimaryReadonly"|"SecondaryReadonly"] that you would like to regenerate.</span></span>

<span data-ttu-id="88acd-199">Exempel:</span><span class="sxs-lookup"><span data-stu-id="88acd-199">Example:</span></span>

    Invoke-AzureRmResourceAction -Action regenerateKey -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test" -Parameters @{"keyKind"="Primary"}

## <span data-ttu-id="88acd-200"><a id="modify-failover-priority-powershell"></a>Ändra redundans prioriteten för en Azure Cosmos DB-databaskonto</span><span class="sxs-lookup"><span data-stu-id="88acd-200"><a id="modify-failover-priority-powershell"></a> Modify Failover Priority of an Azure Cosmos DB Database Account</span></span>

<span data-ttu-id="88acd-201">Du kan ändra prioriteten för växling vid fel för olika regioner som Azure Cosmos DB databaskontot finns i för flera regioner databasen konton.</span><span class="sxs-lookup"><span data-stu-id="88acd-201">For multi-region database accounts, you can change the failover priority of the various regions which the Azure Cosmos DB database account exists in.</span></span> <span data-ttu-id="88acd-202">Mer information om växling vid fel i din Azure Cosmos DB databaskonto finns [distribuera data globalt med Azure Cosmos DB][distribute-data-globally].</span><span class="sxs-lookup"><span data-stu-id="88acd-202">For more information on failover in your Azure Cosmos DB database account, see [Distribute data globally with Azure Cosmos DB][distribute-data-globally].</span></span>

    $failoverPolicies = @(@{"locationName"="<write-region-location>"; "failoverPriority"=0},@{"locationName"="<read-region-location>"; "failoverPriority"=1})
    Invoke-AzureRmResourceAction -Action failoverPriorityChange -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>" -Parameters @{"failoverPolicies"=$failoverPolicies}

* <span data-ttu-id="88acd-203">`<write-region-location>`Platsnamnet för databaskontot skrivåtgärder regionen.</span><span class="sxs-lookup"><span data-stu-id="88acd-203">`<write-region-location>` The location name of the write region of the database account.</span></span> <span data-ttu-id="88acd-204">Den här platsen måste ha ett värde för växling vid fel prioritet 0.</span><span class="sxs-lookup"><span data-stu-id="88acd-204">This location is required to have a failover priority value of 0.</span></span> <span data-ttu-id="88acd-205">Det måste finnas exakt en skrivning region per konto.</span><span class="sxs-lookup"><span data-stu-id="88acd-205">There must be exactly one write region per database account.</span></span>
* <span data-ttu-id="88acd-206">`<read-region-location>`Platsnamnet för den skrivskyddade regionen för databaskontot.</span><span class="sxs-lookup"><span data-stu-id="88acd-206">`<read-region-location>` The location name of the read region of the database account.</span></span> <span data-ttu-id="88acd-207">Den här platsen måste ha ett värde för prioritet för växling vid fel som är större än 0.</span><span class="sxs-lookup"><span data-stu-id="88acd-207">This location is required to have a failover priority value of greater than 0.</span></span> <span data-ttu-id="88acd-208">Det kan finnas mer än en skrivskyddad regioner per konto.</span><span class="sxs-lookup"><span data-stu-id="88acd-208">There can be more than one read regions per database account.</span></span>
* <span data-ttu-id="88acd-209">`<resource-group-name>`Namnet på den [Azure-resursgrupp] [ azure-resource-groups] som det nya kontot i Azure Cosmos DB databasen tillhör.</span><span class="sxs-lookup"><span data-stu-id="88acd-209">`<resource-group-name>` The name of the [Azure Resource Group][azure-resource-groups] to which the new Azure Cosmos DB database account belongs to.</span></span>
* <span data-ttu-id="88acd-210">`<database-account-name>`Namnet på Azure Cosmos DB databaskontot.</span><span class="sxs-lookup"><span data-stu-id="88acd-210">`<database-account-name>` The name of the Azure Cosmos DB database account.</span></span>

<span data-ttu-id="88acd-211">Exempel:</span><span class="sxs-lookup"><span data-stu-id="88acd-211">Example:</span></span>

    $failoverPolicies = @(@{"locationName"="East US"; "failoverPriority"=0},@{"locationName"="West US"; "failoverPriority"=1})
    Invoke-AzureRmResourceAction -Action failoverPriorityChange -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test" -Parameters @{"failoverPolicies"=$failoverPolicies}

## <a name="next-steps"></a><span data-ttu-id="88acd-212">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="88acd-212">Next steps</span></span>

* <span data-ttu-id="88acd-213">Om du vill ansluta med hjälp av .NET finns [ansluter och frågar med .NET](create-documentdb-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="88acd-213">To connect using .NET, see [Connect and query with .NET](create-documentdb-dotnet.md).</span></span>
* <span data-ttu-id="88acd-214">Om du vill ansluta med .NET Core, se [Anslut och fråga med .NET Core](create-documentdb-dotnet-core.md).</span><span class="sxs-lookup"><span data-stu-id="88acd-214">To connect using .NET Core, see [Connect and query with .NET Core](create-documentdb-dotnet-core.md).</span></span>
* <span data-ttu-id="88acd-215">Om du vill ansluta med Node.js, se [ansluter och frågar med Node.js och en MongoDB-app](create-mongodb-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="88acd-215">To connect using Node.js, see [Connect and query with Node.js and a MongoDB app](create-mongodb-nodejs.md).</span></span>

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[powershell-install-configure]: https://docs.microsoft.com/azure/powershell-install-configure
[scaling-globally]: distribute-data-globally.md#EnableGlobalDistribution
[distribute-data-globally]: distribute-data-globally.md
[azure-resource-groups]: https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups
[azure-resource-tags]: https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags
[rp-rest-api]: https://docs.microsoft.com/rest/api/documentdbresourceprovider/
