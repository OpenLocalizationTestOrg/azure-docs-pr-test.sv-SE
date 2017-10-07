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
# <a name="create-an-azure-cosmos-db-account-using-powershell"></a>Skapa ett Azure DB som Cosmos-konto med hjälp av PowerShell

hello beskriver följande guiden kommandon tooautomate hanteringen av dina Azure Cosmos DB databasen konton med hjälp av Azure Powershell. Den innehåller också kommandon toomanage nycklar och prioriteringar för växling vid fel i [flera regioner databasen konton][scaling-globally]. Uppdatera ditt konto kan du toomodify konsekvenskontroll principer och Lägg till/ta bort regioner. Plattformsoberoende hantering av Azure DB som Cosmos-konto, kan använda antingen [Azure CLI](cli-samples.md), hello [Resource Provider REST API][rp-rest-api], eller hello [Azure portalen](create-documentdb-dotnet.md#create-account).

## <a name="getting-started"></a>Komma igång

Följ anvisningarna för hello i [hur tooinstall och konfigurera Azure PowerShell] [ powershell-install-configure] tooinstall och logga in tooyour Azure Resource Manager-kontot i Powershell.

### <a name="notes"></a>Anteckningar

* Om du vill att tooexecute hello följande kommandon utan bekräftelse från användaren att lägga till hello `-Force` flaggan toohello kommando.
* Alla hello följande kommandon är synkrona.

## <a id="create-documentdb-account-powershell"></a>Skapa ett Azure Cosmos DB-konto

Det här kommandot kan du toocreate ett konto för Azure DB som Cosmos-databasen. Konfigurera ditt nya konto som antingen en region eller [flera regioner] [ scaling-globally] med en viss [konsekvent princip](consistency-levels.md).

    $locations = @(@{"locationName"="<write-region-location>"; "failoverPriority"=0}, @{"locationName"="<read-region-location>"; "failoverPriority"=1})
    $iprangefilter = "<ip-range-filter>"
    $consistencyPolicy = @{"defaultConsistencyLevel"="<default-consistency-level>"; "maxIntervalInSeconds"="<max-interval>"; "maxStalenessPrefix"="<max-staleness-prefix>"}
    $CosmosDBProperties = @{"databaseAccountOfferType"="Standard"; "locations"=$locations; "consistencyPolicy"=$consistencyPolicy; "ipRangeFilter"=$iprangefilter}
    New-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName <resource-group-name>  -Location "<resource-group-location>" -Name <database-account-name> -Properties $CosmosDBProperties
    
* `<write-region-location>`namn på hello hello skriva regionens hello konto. Den här platsen är nödvändig toohave redundans prioriteringsvärdet 0. Det måste finnas exakt en skrivning region per konto.
* `<read-region-location>`namn på hello hello läsa regionens hello databaskonto. Den här platsen är nödvändig toohave ett prioritetsvärde för växling vid fel som är större än 0. Det kan finnas mer än en skrivskyddad regioner per konto.
* `<ip-range-filter>`Anger hello uppsättning IP-adresser och IP-adressintervall i CIDR formuläret toobe inkluderad som hello tillåts lista över klientens IP-adresser för en viss databas-konto. IP-adressintervall måste vara kommatecken avgränsade och får inte innehålla blanksteg. Mer information finns i [Azure Cosmos DB-brandväggsstöd](firewall-support.md)
* `<default-consistency-level>`hello konsekvenskontroll standardnivå för hello Azure DB som Cosmos-konto. Mer information finns i [Konsekvensnivåer i Azure Cosmos DB](consistency-levels.md).
* `<max-interval>`När det används med begränsas föråldringskonsekvens representerar det här värdet hello tid föråldrad (i sekunder) tolereras. Tillåtet område för det här värdet är 1-100.
* `<max-staleness-prefix>`När det används med begränsas föråldringskonsekvens representerar det här värdet hello antal inaktiva begäranden tolereras. Tillåtet område för det här värdet är 1 – 2 147 483 647.
* `<resource-group-name>`hello namnet på hello [Azure-resursgrupp] [ azure-resource-groups] toowhich hello nya Azure Cosmos DB databaskonto tillhör.
* `<resource-group-location>`hello platsen för hello Azure-resursgrupp toowhich hello nya Azure Cosmos DB konto tillhör.
* `<database-account-name>`hello namnet på hello Azure Cosmos DB databasen konto toobe skapas. Det kan endast använda gemena bokstäver, siffror, hello '-' tecken och måste vara mellan 3 och 50 tecken.

Exempel: 

    $locations = @(@{"locationName"="West US"; "failoverPriority"=0}, @{"locationName"="East US"; "failoverPriority"=1})
    $iprangefilter = ""
    $consistencyPolicy = @{"defaultConsistencyLevel"="BoundedStaleness"; "maxIntervalInSeconds"=5; "maxStalenessPrefix"=100}
    $CosmosDBProperties = @{"databaseAccountOfferType"="Standard"; "locations"=$locations; "consistencyPolicy"=$consistencyPolicy; "ipRangeFilter"=$iprangefilter}
    New-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Location "West US" -Name "docdb-test" -Properties $CosmosDBProperties

### <a name="notes"></a>Anteckningar
* hello skapar exemplet ovan ett konto med två områden. Det är också möjligt toocreate ett databaskonto med en region (som är utsedd till hello skrivåtgärder region och har ett prioritetsvärde för växling vid fel 0) eller fler än två regioner. Mer information finns i [flera regioner databasen konton][scaling-globally].
* hello platser måste vara regioner som Azure Cosmos DB är allmänt tillgänglig. hello aktuell lista över regioner finns på hello [Azure-regioner sidan](https://azure.microsoft.com/regions/#services).

## <a id="update-documentdb-account-powershell"></a>Uppdatera ett DocumentDB-databaskonto

Det här kommandot ger tooupdate kontoegenskaperna Azure DB som Cosmos-databasen. Detta inkluderar hello konsekvent princip och hello platser vilka hello konto finns i.

> [!NOTE]
> Det här kommandot kan du tooadd och ta bort regioner, men kan du inte toomodify redundans prioriteter. toomodify redundans prioriteter finns [under](#modify-failover-priority-powershell).

    $locations = @(@{"locationName"="<write-region-location>"; "failoverPriority"=0}, @{"locationName"="<read-region-location>"; "failoverPriority"=1})
    $iprangefilter = "<ip-range-filter>"
    $consistencyPolicy = @{"defaultConsistencyLevel"="<default-consistency-level>"; "maxIntervalInSeconds"="<max-interval>"; "maxStalenessPrefix"="<max-staleness-prefix>"}
    $CosmosDBProperties = @{"databaseAccountOfferType"="Standard"; "locations"=$locations; "consistencyPolicy"=$consistencyPolicy; "ipRangeFilter"=$iprangefilter}
    Set-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName <resource-group-name> -Name <database-account-name> -Properties $CosmosDBProperties
    
* `<write-region-location>`namn på hello hello skriva regionens hello konto. Den här platsen är nödvändig toohave redundans prioriteringsvärdet 0. Det måste finnas exakt en skrivning region per konto.
* `<read-region-location>`namn på hello hello läsa regionens hello databaskonto. Den här platsen är nödvändig toohave ett prioritetsvärde för växling vid fel som är större än 0. Det kan finnas mer än en skrivskyddad regioner per konto.
* `<default-consistency-level>`hello konsekvenskontroll standardnivå för hello Azure DB som Cosmos-konto. Mer information finns i [Konsekvensnivåer i Azure Cosmos DB](consistency-levels.md).
* `<ip-range-filter>`Anger hello uppsättning IP-adresser och IP-adressintervall i CIDR formuläret toobe inkluderad som hello tillåts lista över klientens IP-adresser för en viss databas-konto. IP-adressintervall måste vara kommatecken avgränsade och får inte innehålla blanksteg. Mer information finns i [Azure Cosmos DB-brandväggsstöd](firewall-support.md)
* `<max-interval>`När det används med begränsas föråldringskonsekvens representerar det här värdet hello tid föråldrad (i sekunder) tolereras. Tillåtet område för det här värdet är 1-100.
* `<max-staleness-prefix>`När det används med begränsas föråldringskonsekvens representerar det här värdet hello antal inaktiva begäranden tolereras. Tillåtet område för det här värdet är 1 – 2 147 483 647.
* `<resource-group-name>`hello namnet på hello [Azure-resursgrupp] [ azure-resource-groups] toowhich hello nya Azure Cosmos DB databaskonto tillhör.
* `<resource-group-location>`hello platsen för hello Azure-resursgrupp toowhich hello nya Azure Cosmos DB konto tillhör.
* `<database-account-name>`hello namnet på hello Azure Cosmos DB databasen konto toobe uppdateras.

Exempel: 

    $locations = @(@{"locationName"="West US"; "failoverPriority"=0}, @{"locationName"="East US"; "failoverPriority"=1})
    $iprangefilter = ""
    $consistencyPolicy = @{"defaultConsistencyLevel"="BoundedStaleness"; "maxIntervalInSeconds"=5; "maxStalenessPrefix"=100}
    $CosmosDBProperties = @{"databaseAccountOfferType"="Standard"; "locations"=$locations; "consistencyPolicy"=$consistencyPolicy; "ipRangeFilter"=$iprangefilter}
    Set-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test" -Properties $CosmosDBProperties

## <a id="delete-documentdb-account-powershell"></a>Ta bort en DocumentDB-databaskonto

Det här kommandot kan du toodelete ett befintligt konto i Azure DB som Cosmos-databasen.

    Remove-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>"
    
* `<resource-group-name>`hello namnet på hello [Azure-resursgrupp] [ azure-resource-groups] toowhich hello nya Azure Cosmos DB databaskonto tillhör.
* `<database-account-name>`hello namnet på hello Azure Cosmos DB databasen konto toobe tas bort.

Exempel:

    Remove-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test"

## <a id="get-documentdb-properties-powershell"></a>Hämta egenskaper för ett DocumentDB-databaskonto

Det här kommandot kan du tooget hello egenskaper för ett befintligt konto i Azure DB som Cosmos-databasen.

    Get-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>"

* `<resource-group-name>`hello namnet på hello [Azure-resursgrupp] [ azure-resource-groups] toowhich hello nya Azure Cosmos DB databaskonto tillhör.
* `<database-account-name>`hello namnet på hello Azure Cosmos DB konto.

Exempel:

    Get-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test"

## <a id="update-tags-powershell"></a>Uppdatera taggar för ett konto för Azure Cosmos DB-databas

hello följande exempel beskrivs hur tooset [Azure resurstaggar] [ azure-resource-tags] Azure Cosmos-DB databasen i kontot.

> [!NOTE]
> Det här kommandot kan kombineras med hello skapa eller uppdatera kommandon genom att lägga till hello `-Tags` flagga hello motsvarande parameter.

Exempel:

    $tags = @{"dept" = "Finance”; environment = “Production”}
    Set-AzureRmResource -ResourceType “Microsoft.DocumentDB/databaseAccounts”  -ResourceGroupName "rg-test" -Name "docdb-test" -Tags $tags

## <a id="list-account-keys-powershell"></a>Lista nycklar

När du skapar ett konto i Azure Cosmos DB genererar hello tjänsten två master åtkomstnycklar som kan användas för autentisering när hello Azure Cosmos DB konto används. Genom att tillhandahålla två åtkomstnycklar kan Azure Cosmos DB du tooregenerate hello nycklarna utan avbrott tooyour Azure DB som Cosmos-konto. Det finns också skrivskyddade nycklar för att autentisera skrivskyddade åtgärder. Det finns två skrivskyddad (primär eller sekundär) och två skrivskyddade nycklar (primär eller sekundär).

    $keys = Invoke-AzureRmResourceAction -Action listKeys -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>"

* `<resource-group-name>`hello namnet på hello [Azure-resursgrupp] [ azure-resource-groups] toowhich hello nya Azure Cosmos DB databaskonto tillhör.
* `<database-account-name>`hello namnet på hello Azure Cosmos DB konto.

Exempel:

    $keys = Invoke-AzureRmResourceAction -Action listKeys -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test"

## <a id="list-connection-strings-powershell"></a>Lista över anslutningssträngar

Hej anslutning sträng tooconnect MongoDB för app toohello databaskonto kan hämtas med hello följande kommando för MongoDB-konton.

    $keys = Invoke-AzureRmResourceAction -Action listConnectionStrings -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>"

* `<resource-group-name>`hello namnet på hello [Azure-resursgrupp] [ azure-resource-groups] toowhich hello nya Azure Cosmos DB databaskonto tillhör.
* `<database-account-name>`hello namnet på hello Azure Cosmos DB konto.

Exempel:

    $keys = Invoke-AzureRmResourceAction -Action listConnectionStrings -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test"

## <a id="regenerate-account-key-powershell"></a>Återskapa Kontonyckel

Du bör ändra hello nycklar tooyour Azure Cosmos DB åtkomstkonto regelbundet toohelp skydda dina anslutningar. Två åtkomstnycklar tilldelas tooenable du toomaintain anslutningar toohello Azure DB som Cosmos-konto med hjälp av ena åtkomstnyckeln medan du återskapar hello andra snabbtangent.

    Invoke-AzureRmResourceAction -Action regenerateKey -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>" -Parameters @{"keyKind"="<key-kind>"}

* `<resource-group-name>`hello namnet på hello [Azure-resursgrupp] [ azure-resource-groups] toowhich hello nya Azure Cosmos DB databaskonto tillhör.
* `<database-account-name>`hello namnet på hello Azure Cosmos DB konto.
* `<key-kind>`Hello fyra typer av nycklar: [”primär” | ” Sekundär ”|” PrimaryReadonly ”|” SecondaryReadonly ”] som du vill att tooregenerate.

Exempel:

    Invoke-AzureRmResourceAction -Action regenerateKey -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test" -Parameters @{"keyKind"="Primary"}

## <a id="modify-failover-priority-powershell"></a>Ändra redundans prioriteten för en Azure Cosmos DB-databaskonto

Du kan ändra hello redundans prioriteten för hello olika regioner som hello Azure Cosmos DB konto finns i för flera regioner databasen konton. Mer information om växling vid fel i din Azure Cosmos DB databaskonto finns [distribuera data globalt med Azure Cosmos DB][distribute-data-globally].

    $failoverPolicies = @(@{"locationName"="<write-region-location>"; "failoverPriority"=0},@{"locationName"="<read-region-location>"; "failoverPriority"=1})
    Invoke-AzureRmResourceAction -Action failoverPriorityChange -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>" -Parameters @{"failoverPolicies"=$failoverPolicies}

* `<write-region-location>`namn på hello hello skriva regionens hello konto. Den här platsen är nödvändig toohave redundans prioriteringsvärdet 0. Det måste finnas exakt en skrivning region per konto.
* `<read-region-location>`namn på hello hello läsa regionens hello databaskonto. Den här platsen är nödvändig toohave ett prioritetsvärde för växling vid fel som är större än 0. Det kan finnas mer än en skrivskyddad regioner per konto.
* `<resource-group-name>`hello namnet på hello [Azure-resursgrupp] [ azure-resource-groups] toowhich hello nya Azure Cosmos DB databaskonto tillhör.
* `<database-account-name>`hello namnet på hello Azure Cosmos DB konto.

Exempel:

    $failoverPolicies = @(@{"locationName"="East US"; "failoverPriority"=0},@{"locationName"="West US"; "failoverPriority"=1})
    Invoke-AzureRmResourceAction -Action failoverPriorityChange -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test" -Parameters @{"failoverPolicies"=$failoverPolicies}

## <a name="next-steps"></a>Nästa steg

* tooconnect med hjälp av .NET, se [ansluter och frågar med .NET](create-documentdb-dotnet.md).
* tooconnect med .NET Core finns [Anslut och fråga med .NET Core](create-documentdb-dotnet-core.md).
* tooconnect med Node.js, se [ansluter och frågar med Node.js och en MongoDB-app](create-mongodb-nodejs.md).

<!--Reference style links - using these makes hello source content way more readable than using inline links-->
[powershell-install-configure]: https://docs.microsoft.com/azure/powershell-install-configure
[scaling-globally]: distribute-data-globally.md#EnableGlobalDistribution
[distribute-data-globally]: distribute-data-globally.md
[azure-resource-groups]: https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups
[azure-resource-tags]: https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags
[rp-rest-api]: https://docs.microsoft.com/rest/api/documentdbresourceprovider/
