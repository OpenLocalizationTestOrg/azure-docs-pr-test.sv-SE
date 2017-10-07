---
title: aaaAzure SQL Database connectivity arkitektur | Microsoft Docs
description: "Det här dokumentet förklarar hello Azure SQLDB anslutning arkitektur från Azure eller från utanför Azure."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: monicar
ms.assetid: 
ms.service: sql-database
ms.custom: DBs & servers
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 06/05/2017
ms.author: carlrab
ms.openlocfilehash: 917df6d88a16f1b841b617fb2a53025b4d14d034
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-sql-database-connectivity-architecture"></a>Azure SQL Database Connectivity-arkitektur 

Den här artikeln beskriver hello Azure SQL Database connectivity arkitektur och förklarar hur hello olika komponenter fungerar toodirect trafik tooyour instans av Azure SQL Database. Komponenterna Azure SQL Database connectivity fungera toodirect nätverk trafik toohello Azure-databas med klienter som ansluter från i Azure och anslutande klienter från utanför Azure. Den här artikeln innehåller också skript prover toochange hur anslutningen sker och hello överväganden relaterade toochanging hello anslutningen standardinställningarna. Om det finns några frågor när du har läst den här artikeln kan du kontakta Dhruv på dmalik@microsoft.com. 

## <a name="connectivity-architecture"></a>Anslutningsarkitektur

hello följande diagram ger en översikt över hello Azure SQL Database connectivity arkitektur. 

![arkitektur, översikt](./media/sql-database-connectivity-architecture/architecture-overview.png)


hello följande steg beskriver hur en anslutning är upprättad tooan Azure SQL database via hello Azure SQL Database-programbelastningsutjämnaren (SLB) och hello Azure SQL Database-gateway.

- Klienter i Azure eller utanför Azure ansluta toohello SLB som har en offentlig IP-adress och lyssnar på port 1433.
- Hej SLB dirigerar trafik toohello Azure SQL Database-gateway.
- hello-gateway dirigerar hello trafik toohello rätt proxy mellanprogram.
- hello proxy mellanprogram omdirigerar hello-trafik toohello lämplig Azure SQL-databas.

> [!IMPORTANT]
> Var och en av dessa komponenter har distribuerats för tjänsten (DDoS) skydd inbyggda på hello nätverk och hello app lager.
>

## <a name="connectivity-from-within-azure"></a>Om du använder i Azure

Om du ansluter från i Azure anslutningarna har en princip för **omdirigera** som standard. En princip av **omdirigera** innebär att anslutningar när hello TCP-session är etablerad toohello Azure SQL database, hello klientsessionen sedan omdirigeras toohello proxy mellanprogram med en ändring toohello mål virtuella IP-adress från som hello Azure SQL Database gateway toothat av hello proxy mellanprogram. Därefter alla efterföljande paket som flödar direkt via hello proxy mellanprogram, vilket kringgår hello Azure SQL Database-gateway. hello följande diagram illustrerar flödet i nätverkstrafiken.

![arkitektur, översikt](./media/sql-database-connectivity-architecture/connectivity-from-within-azure.png)

## <a name="connectivity-from-outside-of-azure"></a>Anslutningen från utanför Azure

Om du ansluter från utanför Azure anslutningarna har en princip för **Proxy** som standard. En princip av **Proxy** innebär att hello TCP-sessionen har upprättats via hello Azure SQL Database-gateway och alla efterföljande paket flöda hello gateway. hello följande diagram illustrerar flödet i nätverkstrafiken.

![arkitektur, översikt](./media/sql-database-connectivity-architecture/connectivity-from-outside-azure.png)

## <a name="azure-sql-database-gateway-ip-addresses"></a>Azure SQL Database gateway IP-adresser

tooconnect tooan Azure SQL-databas från lokala resurser, behöver du tooallow utgående trafik toohello Azure SQL Database nätverksgateway för din Azure-region. Anslutningar kan bara gå via hello gateway vid anslutning i proxyläge är hello standard när du ansluter från lokala resurser.

följande tabell visar hello hello primära och sekundära IP-adresser för hello Azure SQL Database-gateway för alla dataområden. Det finns två IP-adresser för vissa regioner. I dessa regioner hello primära IP-adress är hello aktuella IP-adressen för hello gateway och hello andra IP-adressen är en IP-adress för redundans. hello-adress för redundans är hello adress toowhich vi kan flytta din server tookeep hello tjänsttillgänglighet hög. För dessa regioner rekommenderar vi att du utgående tooboth hello IP-adresser. hello andra IP-adressen ägs av Microsoft och lyssnar inte på några tjänster förrän den aktiveras av Azure SQL Database tooaccept anslutningar.

| Regionsnamn | Primära IP-adress | Sekundär IP-adress |
| --- | --- |--- |
| Östra Australien | 191.238.66.109 | 13.75.149.87 |
| Sydöstra Australien | 191.239.192.109 | 13.73.109.251 |
| Södra Brasilien | 104.41.11.5 | |    
| Centrala Kanada | 40.85.224.249 | |    
| Östra Kanada | 40.86.226.166 | |
| Centrala USA | 23.99.160.139 | 13.67.215.62 |
| Östasien | 191.234.2.139 | 52.175.33.150 |
| Östra USA 1 | 191.238.6.43 | 40.121.158.30 |
| Östra USA 2 | 191.239.224.107 | 40.79.84.180 |
| Centrala Indien | 104.211.96.159  | |   
| Södra Indien | 104.211.224.146  | |
| Västra Indien | 104.211.160.80 | |
| Östra Japan | 191.237.240.43 | 13.78.61.196 |
| Västra Japan | 191.238.68.11 | 104.214.148.156 |
| Centrala Korea | 52.231.32.42 | |
| Sydkorea | 52.231.200.86 |  |
| Norra centrala USA | 23.98.55.75 | 23.96.178.199 |
| Norra Europa | 191.235.193.75 | 40.113.93.91 |
| Södra centrala USA | 23.98.162.75 | 13.66.62.124 |
| Sydostasien | 23.100.117.95 | 104.43.15.0 |
| Storbritannien, norra | 13.87.97.210 | |
| Storbritannien, Syd 1 | 51.140.184.11 | |    
| Storbritannien, södra 2 | 13.87.34.7 | |
| Storbritannien, västra | 51.141.8.11  | |
| Västra centrala USA | 13.78.145.25 | |
| Västra Europa | 191.237.232.75 | 40.68.37.158 |
| USA, västra 1 | 23.99.34.75 | 104.42.238.205 |
| Västra USA 2 | 13.66.226.202  | |
||||

## <a name="change-azure-sql-database-connection-policy"></a>Ändra principen för Azure SQL Database-anslutning

toochange hello Azure SQL Database-princip för en Azure SQL Database-server, Använd hello [REST API](https://msdn.microsoft.com/library/azure/mt604439.aspx). 

- Om din anslutningsprincip har angetts för**Proxy**, alla nätverksenheter paket flöde via hello Azure SQL Database-gateway. Du måste tooallow utgående tooonly hello Azure SQL Database gateway IP för den här inställningen. Med hjälp av inställningen **Proxy** har flera svarstider än inställningen **omdirigera**. 
- Om din anslutning principinställningen **omdirigera**, alla nätverkspaket flöda direkt toohello mellanprogram proxy. Du måste tooallow utgående toomultiple IP-adresser för den här inställningen. 

## <a name="script-toochange-connection-settings"></a>Skriptet toochange anslutningsinställningar

> [!IMPORTANT]
> Skriptet kräver hello [Azure PowerShell-modulen](/powershell/azure/install-azurerm-ps).
>

hello följande PowerShell-skript visar hur toochange hello anslutningsprincip.

```powershell
import-module azureRm
Login-AzureRmAccount

$tenantId =  #your AAD tenant ID
$subscriptionId = #Azure SubscriptionID
$uri = #AAD uri
$authUrl = "https://login.microsoftonline.com/$tenantId"
$serverName = #sqldb server name 
$resourceGroupName=#sqldb resource group
$AuthContext = [Microsoft.IdentityModel.Clients.ActiveDirectory.AuthenticationContext]$authUrl

$result = $AuthContext.AcquireToken("https://management.core.windows.net/",
$clientId,
[Uri]$uri, 
[Microsoft.IdentityModel.Clients.ActiveDirectory.PromptBehavior]::Auto)

$authHeader = @{
'Content-Type'='application\json; '
'Authorization'=$result.CreateAuthorizationHeader()
}

#getting hello current connection property
Invoke-RestMethod -Uri "https://management.azure.com/subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.Sql/servers/$serverName/connectionPolicies/Default?api-version=2014-04-01-preview" -Method GET -Headers $authHeader

#setting hello property too‘Proxy’
$connectionType=”Proxy” <#Redirect / Default are other options#>
$body = @{properties=@{connectionType=$connectionType}} | ConvertTo-Json

Invoke-RestMethod -Uri "https://management.azure.com/subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.Sql/servers/$serverName/connectionPolicies/Default?api-version=2014-04-01-preview" -Method PUT -Headers $authHeader -Body $body -ContentType "application/json"
```

## <a name="next-steps"></a>Nästa steg

- Mer information om hur toochange hello Azure SQL Database-princip för en Azure SQL Database-server finns [skapa eller uppdateringsprincip Server-anslutning med hjälp av hello REST API](https://msdn.microsoft.com/library/azure/mt604439.aspx).
- Mer information om Azure SQL Database-anslutningen för klienter som använder ADO.NET 4.5 eller senare finns [portar utöver 1433 ADO.NET 4.5](sql-database-develop-direct-route-ports-adonet-v12.md).
- Allmänt program development översikt översiktsinformation finns i [SQL Database Development Programöversikt](sql-database-develop-overview.md).
