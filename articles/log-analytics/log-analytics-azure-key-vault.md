---
title: "aaaAzure Key Vault-lösning i Log Analytics | Microsoft Docs"
description: "Du kan använda hello Azure Key Vault-lösning i logganalys tooreview Azure Key Vault loggar."
services: log-analytics
documentationcenter: 
author: richrundmsft
manager: jochan
editor: 
ms.assetid: 5e25e6d6-dd20-4528-9820-6e2958a40dae
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/09/2017
ms.author: richrund
ms.openlocfilehash: 1c6eae26ded7ad55b0159a3be09cdc9901596298
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-key-vault-analytics-solution-in-log-analytics"></a>Azure Key Vault Analytics lösning i logganalys

![Key Vault symbol](./media/log-analytics-azure-keyvault/key-vault-analytics-symbol.png)

Du kan använda hello Azure Key Vault-lösning i logganalys tooreview Azure Key Vault AuditEvent loggar.

toouse hello lösningen måste tooenable loggning av Azure Key Vault diagnostik- och direkt hello diagnostik tooa logganalys-arbetsytan. Det är inte nödvändigt toowrite hello loggar tooAzure Blob storage.

> [!NOTE]
> Hello stöds i januari 2017 sätt att skicka loggar från Nyckelvalvet tooLog Analytics har ändrats. Visar om hello Key Vault-lösningen som du använder *(föråldrad)* i hello titeln, referera för[migrera från hello gamla Key Vault lösning](#migrating-from-the-old-key-vault-solution) för steg behöver du toofollow.
>
>

## <a name="install-and-configure-hello-solution"></a>Installera och konfigurera hello lösning
Använd följande instruktioner tooinstall hello och konfigurera hello Azure Key Vault-lösningen:

1. Aktivera hello Azure Key Vault-lösning från [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.KeyVaultAnalyticsOMS?tab=Overview) eller genom att använda hello process som beskrivs i [lägga till logganalys lösningar från hello lösningar galleriet](log-analytics-add-solutions.md).
2. Aktivera diagnostik loggning för hello Key Vault resurser toomonitor, med antingen hello [portal](#enable-key-vault-diagnostics-in-the-portal) eller [PowerShell](#enable-key-vault-diagnostics-using-powershell)

### <a name="enable-key-vault-diagnostics-in-hello-portal"></a>Aktivera Key Vault-diagnostik i hello-portalen

1. Navigera i hello Azure-portalen, toohello Key Vault resurs toomonitor
2. Välj *diagnostik loggar* tooopen hello följande sida

   ![Bild av Azure Key Vault sida vid sida](./media/log-analytics-azure-keyvault/log-analytics-keyvault-enable-diagnostics01.png)
3. Klicka på *aktivera diagnostiken* tooopen hello följande sida

   ![Bild av Azure Key Vault sida vid sida](./media/log-analytics-azure-keyvault/log-analytics-keyvault-enable-diagnostics02.png)
4. tooturn på diagnostik, klickar du på *på* under *Status*
5. Klicka på hello kryssrutan för *skicka tooLog Analytics*
6. Välj en befintlig logganalys-arbetsyta eller skapa en arbetsyta
7. tooenable *AuditEvent* loggar, klickar du på kryssrutan hello under logg
8. Klicka på *spara* tooenable hello loggning av diagnostik tooLog Analytics

### <a name="enable-key-vault-diagnostics-using-powershell"></a>Aktivera diagnostik för Key Vault med hjälp av PowerShell
hello följande PowerShell-skript innehåller ett exempel på hur toouse `Set-AzureRmDiagnosticSetting` tooenable loggning för Key Vault:
```
$workspaceId = "/subscriptions/d2e37fee-1234-40b2-5678-0b2199de3b50/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/rollingbaskets"

$kv = Get-AzureRmKeyVault -VaultName 'ContosoKeyVault'

Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId  -WorkspaceId $workspaceId -Enabled $true
```



## <a name="review-azure-key-vault-data-collection-details"></a>Granska Azure Key Vault information för samlingen
Azure Key Vault-lösningen samlar in diagnostik loggarna direkt från hello Key Vault.
Det är inte nödvändigt toowrite hello loggar tooAzure Blob storage och ingen agent krävs för datainsamling.

hello följande tabell visar metoder för insamling av data och annan information om hur data samlas in för Azure Key Vault.

| Plattform | Styr agent | System Center Operations Manager-agenten | Azure | Operations Manager som krävs? | Operations Manager agent-data som skickas via management-grupp | Insamlingsfrekvens |
| --- | --- | --- | --- | --- | --- | --- |
| Azure |  |  |&#8226; |  |  | anländer |

## <a name="use-azure-key-vault"></a>Använda Azure Key Vault
När du [installera hello lösning](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.KeyVaultAnalyticsOMS?tab=Overview), visa hello Key Vault data genom att klicka på hello **Azure Key Vault** panelen från hello **översikt** sidan i logganalys.

![Bild av Azure Key Vault sida vid sida](./media/log-analytics-azure-keyvault/log-analytics-keyvault-tile.png)

När du klickar på hello **översikt** sida vid sida, kan du visa sammanfattningar av loggar och sedan gå i toodetails för hello följande kategorier:

* Volymen för alla åtgärder i nyckelvalvet över tid
* Det gick inte åtgärden volymer över tid
* Genomsnittlig svarstid för operativa för åtgärden
* Tjänstkvalitet för åtgärder med hello antal åtgärder som tar mer än 1 000 ms och en lista över åtgärder som tar mer än 1 000 ms

![Bild av Azure Key Vault-instrumentpanelen](./media/log-analytics-azure-keyvault/log-analytics-keyvault01.png)

![Bild av Azure Key Vault-instrumentpanelen](./media/log-analytics-azure-keyvault/log-analytics-keyvault02.png)

### <a name="tooview-details-for-any-operation"></a>tooview information för någon åtgärd
1. På hello **översikt** klickar du på hello **Azure Key Vault** panelen.
2. På hello **Azure Key Vault** instrumentpanel, granska hello sammanfattningsinformation i ett hello blad och klicka sedan på en tooview detaljerad information om den hello söksidan för loggen.

    Du kan visa resultaten av tid, detaljerade resultat och Logghistoriken på någon av hello loggen söksidor. Du kan också filtrera efter facets toonarrow hello resultat.

## <a name="log-analytics-records"></a>Log Analytics-poster
hello Azure Key Vault lösning analyserar poster som har en typ av **KeyVaults** som samlas in från [AuditEvent loggar](../key-vault/key-vault-logging.md) i Azure-diagnostik.  Egenskaper för dessa poster finns i följande tabell hello:  

| Egenskap | Beskrivning |
|:--- |:--- |
| Typ |*AzureDiagnostics* |
| SourceSystem |*Azure* |
| CallerIpAddress |IP-adressen för hello-klienten som gjorde begäran hello |
| Kategori | *AuditEvent* |
| CorrelationId |Ett valfritt GUID som hello klienten kan skicka toocorrelate klientsidan loggar med loggar av tjänsten på klientsidan (Key Vault). |
| DurationMs |Tiden det tog tooservice hello REST-API-begäran, i millisekunder. Nu omfattar inte Nätverksfördröjningen, så hello tid som du mäter på klientsidan för hello inte kanske stämmer med den här gången. |
| httpStatusCode_d |HTTP-statuskoden som returnerades av hello begäran (till exempel *200*) |
| id_s |Unikt ID för hello begäran |
| identity_claim_appid_g | GUID för hello program-id |
| OperationName |Namnet på hello åtgärd utförs, enligt beskrivningen i [Azure Key Vault-loggning](../key-vault/key-vault-logging.md) |
| OperationVersion |REST API-version som begärs av hello klient (till exempel *2015-06-01*) |
| requestUri_s |URI för begäran om hello |
| Resurs |Namnet på hello nyckelvalv |
| ResourceGroup |Resursgruppen för hello nyckelvalv |
| Resurs-ID |Azure Resource Manager Resource-ID. För Key Vault loggar är hello Key Vault resurs-ID. |
| ResourceProvider |*MICROSOFT. KEYVAULT* |
| ResourceType | *VALV* |
| ResultSignature |HTTP-status (till exempel *OK*) |
| ResultType |Resultatet av REST API-begäran (till exempel *lyckade*) |
| SubscriptionId |Azure prenumerations-ID för hello prenumeration som innehåller hello Key Vault |

## <a name="migrating-from-hello-old-key-vault-solution"></a>Migrera från hello gamla Key Vault-lösning
Hello stöds i januari 2017 sätt att skicka loggar från Nyckelvalvet tooLog Analytics har ändrats. Ändringarna ger hello följande fördelar:
+ Loggarna skrivs direkt tooLog Analytics utan hello måste toouse ett lagringskonto
+ Mindre fördröjning från hello tid när loggar är genereras toothem som är tillgängliga i logganalys
+ Färre konfigurationssteg
+ Ett vanligt format för alla typer av Azure-diagnostik

toouse hello uppdateras lösningen:

1. [Konfigurera diagnostik toobe tooLog Analytics skickas direkt från Nyckelvalvet](#enable-key-vault-diagnostics-in-the-portal)  
2. Aktivera hello Azure Key Vault-lösning med hjälp av hello process som beskrivs i [lägga till logganalys lösningar från hello lösningar galleri](log-analytics-add-solutions.md)
3. Uppdatera alla sparade frågor, instrumentpaneler eller aviseringar toouse hello ny datatyp
  + Typen är ändra från: KeyVaults tooAzureDiagnostics. Du kan använda hello ResourceType toofilter tooKey valvet loggar.
  - I stället för: `Type=KeyVaults`, använda`Type=AzureDiagnostics ResourceType=VAULTS`
  + Fält: (fältnamn är skiftlägeskänsligt)
  - För alla fält som har suffixet \_s, \_d, eller \_g i hello namn, ändra hello första toolower skiftlägeskänslighet
  - För alla fält som har suffixet \_o i namn hello data delas upp i enskilda fält baserat på hello kapslade fältnamn. Till exempel lagras hello UPN för hello anroparen i ett fält`identity_claim_http_schemas_xmlsoap_org_ws_2005_05_identity_claims_upn_s`
   - Fältet CallerIpAddress ändras tooCallerIPAddress
   - Fältet RemoteIPCountry finns inte längre
4. Ta bort hello *Key Vault Analytics (inaktuell)* lösning. Om du använder PowerShell använder du`Set-AzureOperationalInsightsIntelligencePack -ResourceGroupName <resource group that hello workspace is in> -WorkspaceName <name of hello log analytics workspace> -IntelligencePackName "KeyVault" -Enabled $false`

Data som samlas in innan hello ändringen inte visas i hello ny lösning. Du kan fortsätta tooquery för den här data med hjälp av hello gamla typ och fältnamn.

## <a name="troubleshooting"></a>Felsökning
[!INCLUDE [log-analytics-troubleshoot-azure-diagnostics](../../includes/log-analytics-troubleshoot-azure-diagnostics.md)]

## <a name="next-steps"></a>Nästa steg
* Använd [logga sökningar i logganalys](log-analytics-log-searches.md) tooview detaljerade Azure Key Vault-data.
