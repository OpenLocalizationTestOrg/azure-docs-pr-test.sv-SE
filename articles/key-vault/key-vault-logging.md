---
title: aaaAzure Key Vault-loggning | Microsoft Docs
description: "Använd den här självstudiekursen toohelp dig att komma igång med Azure Key Vault-loggning."
services: key-vault
documentationcenter: 
author: cabailey
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 43f96a2b-3af8-4adc-9344-bc6041fface8
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 07/19/2017
ms.author: cabailey
ms.openlocfilehash: 38a173297948748bef45e3d857c06b50b3e21e74
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-key-vault-logging"></a>Azure Key Vault-loggning
Azure Key Vault är tillgängligt i de flesta regioner. Mer information finns i hello [Key Vault-priser](https://azure.microsoft.com/pricing/details/key-vault/).

## <a name="introduction"></a>Introduktion
När du har skapat en eller flera nyckelvalv, vill du förmodligen toomonitor hur och när din nyckel valv är ofta, och av vem. Du kan göra det genom att aktivera loggning för nyckelvalvet, vilket sparar information i ett Azure-lagringskonto som du anger. En ny behållare med namnet **insights-logs-auditevent** skapas automatiskt för det angivna lagringskontot och du kan använda samma lagringskonto för att samla in loggar för flera nyckelvalv.

Du kan komma åt din loggningsinformation högst, 10 minuter efter hello nyckeln valvet igen. Oftast är informationen dock tillgänglig snabbare än så.  Är det upp tooyou toomanage loggarna på ditt lagringskonto:

* Använd standard Azure access control metoder toosecure loggarna genom att begränsa vem som kan komma åt dem.
* Ta bort loggar som du inte längre vill tookeep i ditt lagringskonto.

Använd den här självstudiekursen toohelp dig att komma igång med Azure Key Vault-loggning, toocreate ditt lagringskonto, aktivera loggning och tolka hello logga information som samlas in.  

> [!NOTE]
> Den här självstudiekursen innehåller inte instruktioner för hur toocreate nyckeln valv, nycklar och hemligheter. Den här informationen finns i [Komma igång med Azure Key Vault](key-vault-get-started.md). Anvisningar för plattformsoberoende kommandoradsgränssnitt finns i [den här självstudiekursen](key-vault-manage-with-cli2.md).
>
> För närvarande kan du konfigurera Azure Key Vault i hello Azure-portalen. I stället använder du dessa Azure PowerShell-instruktioner.
>
>

Översiktlig information om Azure Key Vault finns i [Vad är Azure Key Vault?](key-vault-whatis.md)

## <a name="prerequisites"></a>Krav
toocomplete den här självstudien måste du ha hello följande:

* Ett befintligt nyckelvalv som du har använt.  
* Azure PowerShell, **minst version 1.0.1**. tooinstall Azure PowerShell och koppla den till din Azure-prenumeration, se [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview). Om du redan har installerat Azure PowerShell och inte vet hello version från hello Azure PowerShell-konsolen, Skriv `(Get-Module azure -ListAvailable).Version`.  
* Tillräckligt med utrymme i Azure för Key Vault-loggarna.

## <a id="connect"></a>Ansluta tooyour prenumerationer
Starta en Azure PowerShell-session och logga in tooyour Azure-konto med hello följande kommando:  

    Login-AzureRmAccount

Ange ditt användarnamn för Azure-konto och lösenord i hello popup-webbläsarfönstret. Azure PowerShell får alla hello-prenumerationer som är associerade med det här kontot och som standard, använder hello första.

Om du har flera prenumerationer du kanske toospecify en som har använt toocreate Azure Key Vault. Skriv hello följande toosee hello prenumerationer för ditt konto:

    Get-AzureRmSubscription

Sedan toospecify hello prenumeration som är kopplad till nyckelvalvet du loggar, typ:

    Set-AzureRmContext -SubscriptionId <subscription ID>

> [!NOTE]
> Detta är ett viktigt steg och särskilt användbart om du har flera prenumerationer som är kopplade till ditt konto. Du kan få ett fel tooregister Microsoft.Insights om det här steget hoppas över.
>   
>

Mer information om hur du konfigurerar Azure PowerShell finns [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).

## <a id="storage"></a>Skapa ett nytt lagringskonto för dina loggar
Men du kan använda ett befintligt lagringskonto för dina loggar, ska vi skapa ett nytt lagringskonto som är dedikerad tooKey valvet loggar. Av praktiska skäl för när vi har toospecify detta senare hello information ska lagras i en variabel med namnet **sa**.

För ytterligare enkel hantering, vi också använder hello samma resursgrupp som hello en som innehåller våra nyckelvalvet. Från hello [komma igång-självstudiekurs](key-vault-get-started.md), den här resursgruppen heter **ContosoResourceGroup** och vi kommer att fortsätta toouse hello Östasien plats. Ersätt värdena med dina egna efter behov:

    $sa = New-AzureRmStorageAccount -ResourceGroupName ContosoResourceGroup -Name contosokeyvaultlogs -Type Standard_LRS -Location 'East Asia'


> [!NOTE]
> Om du väljer toouse ett befintligt lagringskonto, måste den använda hello samma prenumeration som ditt nyckelvalv och den måste använda hello Resource Manager-modellen i stället för hello klassiska distributionsmodellen.
>
>

## <a id="identify"></a>Identifiera hello nyckelvalv för loggarna
I vår komma igång-kursen var vår nyckelvalv namnet **ContosoKeyVault**, så vi kommer att fortsätta toouse som namn och lagra hello information till en variabel med namnet **kv**:

    $kv = Get-AzureRmKeyVault -VaultName 'ContosoKeyVault'


## <a id="enable"></a>Aktivera loggning
tooenable loggning för Key Vault vi använder hello Set-AzureRmDiagnosticSetting cmdlet, tillsammans med hello variabler som vi skapade för vår nya storage-konto och våra nyckelvalvet. Vi ska också ange hello **-aktiverad** flaggan för**$true** och ange hello kategori tooAuditEvent (hello endast kategori för Key Vault-loggning):

    Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent

hello utdata för detta omfattar:

    StorageAccountId   : /subscriptions/<subscription-GUID>/resourceGroups/ContosoResourceGroup/providers/Microsoft.Storage/storageAccounts/ContosoKeyVaultLogs
    ServiceBusRuleId   :
    StorageAccountName :
        Logs
        Enabled           : True
        Category          : AuditEvent
        RetentionPolicy
        Enabled : False
        Days    : 0


Det här bekräftar att loggning har nu aktiverats för nyckelvalvet, spara information tooyour storage-konto.

Du kan också ange bevarandeprincip för loggar så att äldre loggar tas bort automatiskt. Till exempel kvarhållning princip genom att använda **- RetentionEnabled** flaggan för**$true** och ange **- RetentionInDays** parameter för**90** så att loggar som är äldre än 90 dagar tas automatiskt bort.

    Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent -RetentionEnabled $true -RetentionInDays 90

Vad loggas:

* Alla autentiserade REST-API-förfrågningar loggas, vilket omfattar förfrågningar som misslyckats på grund av åtkomstbehörigheter, systemfel eller ogiltiga förfrågningar.
* Åtgärder på hello nyckeln valvet sig själv, vilket inkluderar skapande, borttagning, inställningen nyckelvalv åtkomstprinciper, och uppdaterar nyckelvalv attribut som etiketter.
* Åtgärder för nycklar och hemligheter i hello nyckelvalvet, vilket innefattar att skapa, ändra eller ta bort dessa nycklar och hemligheter; åtgärder som till exempel signera, verifiera, kryptera, dekryptera, omsluter och unwrap nycklar, hämta hemligheter, lista över nycklar och hemligheter och deras versioner.
* Oautentiserade förfrågningar som resulterar i ett 401-svar. Till exempel förfrågningar som inte har någon ägartoken, som är felaktiga, som har upphört att gälla eller som har en ogiltig token.  

## <a id="access"></a>Komma åt loggarna
Nyckelvalv loggfilerna lagras i hello **insights-loggar-auditevent** behållare i hello storage-konto som du angav. toolist skriver du alla hello blobbar i den här behållaren:

Skapa först en variabel för hello behållarnamn. Detta kommer att användas i hela hello resten av hello genomgång.

    $container = 'insights-logs-auditevent'

toolist skriver du alla hello blobbar i den här behållaren:

    Get-AzureStorageBlob -Container $container -Context $sa.Context
hello-utdata kommer att se något liknande toothis:

**Behållarens URI: https://contosokeyvaultlogs.blob.core.windows.net/insights-logs-auditevent**

**Namn**

- - -
**resourceId=/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSORESOURCEGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=05/h=01/m=00/PT1H.json**

**resourceId=/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSORESOURCEGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=04/h=02/m=00/PT1H.json**

**resourceId=/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSORESOURCEGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=04/h=18/m=00/PT1H.json****

Som du ser i den här utdatan hello blobbar följa en namngivningskonvention: **resourceId =<ARM resource ID>/y =<year>/m =<month>/d =<day of month>/tim =<hour>/m =<minute>/filename.json**

hello datum- och tidsvärden Använd UTC.

Eftersom hello samma lagringskonto kan vara används toocollect loggar för olika resurser, är hello fullständiga resurs-ID i hello blob-namnet mycket användbar tooaccess eller hämta bara hello blob som du behöver. Men innan vi gör det kan vi först tar upp hur toodownload alla hello blobbar.

Först skapa en mapp toodownload hello blobbar. Exempel:

    New-Item -Path 'C:\Users\username\ContosoKeyVaultLogs' -ItemType Directory -Force

Hämta sedan en lista över alla blobbar:  

    $blobs = Get-AzureStorageBlob -Container $container -Context $sa.Context

Skicka den här listan via 'Get-AzureStorageBlobContent' toodownload hello blobbar i vår målmappen:

    $blobs | Get-AzureStorageBlobContent -Destination 'C:\Users\username\ContosoKeyVaultLogs'

När du kör andra kommandot hello  **/**  avgränsare i hello blobbnamnen skapa en fullständig mappstruktur under hello målmappen och den här strukturen kommer att använda toodownload och lagra hello blob som filer.

tooselectively ladda ned blobbar kan använda jokertecken. Exempel:

* Om du har flera viktiga valv och vill toodownload loggar för ett enda nyckelvalv med namnet CONTOSOKEYVAULT3:

        Get-AzureStorageBlob -Container $container -Context $sa.Context -Blob '*/VAULTS/CONTOSOKEYVAULT3
* Om du har flera resursgrupper och vill toodownload loggar för en resursgrupp, använda `-Blob '*/RESOURCEGROUPS/<resource group name>/*'`:

        Get-AzureStorageBlob -Container $container -Context $sa.Context -Blob '*/RESOURCEGROUPS/CONTOSORESOURCEGROUP3/*'
* Om du vill toodownload alla hello loggar för hello månad januari 2016 använder `-Blob '*/year=2016/m=01/*'`:

        Get-AzureStorageBlob -Container $container -Context $sa.Context -Blob '*/year=2016/m=01/*'

Du är nu redo toostart tittar på vad som finns i hello loggar. Men innan du flyttar till två fler parametrar för att du kan behöva tooknow Get-AzureRmDiagnosticSetting som:

* tooquery hello status för diagnostikinställningar för nyckelvalvet-resurs:`Get-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId`
* toodisable loggning för nyckelvalvet-resurs:`Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $false -Categories AuditEvent`

## <a id="interpret"></a>Tolka Key Vault-loggarna
Enskilda blobbar lagras som text, formaterad som en JSON-blobb. Det här är ett exempel på en loggpost från `Get-AzureRmKeyVault -VaultName 'contosokeyvault'`:

    {
        "records":
        [
            {
                "time": "2016-01-05T01:32:01.2691226Z",
                "resourceId": "/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSOGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT",
                "operationName": "VaultGet",
                "operationVersion": "2015-06-01",
                "category": "AuditEvent",
                "resultType": "Success",
                "resultSignature": "OK",
                "resultDescription": "",
                "durationMs": "78",
                "callerIpAddress": "104.40.82.76",
                "correlationId": "",
                "identity": {"claim":{"http://schemas.microsoft.com/identity/claims/objectidentifier":"d9da5048-2737-4770-bd64-XXXXXXXXXXXX","http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn":"live.com#username@outlook.com","appid":"1950a258-227b-4e31-a9cf-XXXXXXXXXXXX"}},
                "properties": {"clientInfo":"azure-resource-manager/2.0","requestUri":"https://control-prod-wus.vaultcore.azure.net/subscriptions/361da5d4-a47a-4c79-afdd-XXXXXXXXXXXX/resourcegroups/contosoresourcegroup/providers/Microsoft.KeyVault/vaults/contosokeyvault?api-version=2015-06-01","id":"https://contosokeyvault.vault.azure.net/","httpStatusCode":200}
            }
        ]
    }


hello följande tabell visas hello fältnamn och beskrivningar.

| Fältnamn | Beskrivning |
| --- | --- |
| time |Datum och tid (UTC). |
| resourceId |Azure Resource Manager Resource-ID. För Key Vault loggar är alltid hello Key Vault resurs-ID. |
| operationName |Namnet på hello åtgärd utförs, enligt beskrivningen i hello nästa tabell. |
| operationVersion |Detta är hello REST API-version som begärs av hello-klient. |
| category |För Key Vault loggar är AuditEvent hello enda, tillgänglig värde. |
| resultType |Resultatet av REST-API-begäran. |
| resultSignature |HTTP-status. |
| resultDescription |Ytterligare beskrivning om hello resultat när det är tillgängligt. |
| durationMs |Tiden det tog tooservice hello REST-API-begäran, i millisekunder. Detta inkluderar inte hello Nätverksfördröjningen, så hello tid du mäta på klientsidan för hello inte kanske stämmer med den här gången. |
| callerIpAddress |Hello-klienten som gjorde begäran hello IP-adress. |
| correlationId |Ett valfritt GUID som hello klienten kan skicka toocorrelate klientsidan loggar med loggar av tjänsten på klientsidan (Key Vault). |
| identity |Identitet från hello-token som angavs när du gör hello REST API-begäran. Detta är vanligtvis en ”användare”, ”tjänstens huvudnamn” eller en kombination av ”användare+app-ID”, till exempel då en begäran är resultatet av en Azure PowerShell-cmdlet. |
| properties |Det här fältet innehåller olika typer av information baserat på hello åtgärden (operationName). I de flesta fall innehåller information om klienter (hello useragent sträng skickades av klientens hello), hello exakt URI för REST API-begäran och HTTP-statuskod. Dessutom, när ett objekt returneras ett resultat av en begäran (till exempel KeyCreate eller VaultGet) innehåller den också hello nyckeln URI (som ”id”), valvet URI eller hemlighet URI. |

Hej **operationName** fältvärden har ObjectVerb format. Exempel:

* Alla åtgärder i nyckelvalvet har hello ' valvet`<action>`-formatet som `VaultGet` och `VaultCreate`.
* Alla åtgärder som nyckel har hello ' nyckel`<action>`-formatet som `KeySign` och `KeyList`.
* Alla hemliga åtgärder har hello ' hemlighet`<action>`-formatet som `SecretGet` och `SecretListVersions`.

hello i den följande tabellen listas hello operationName och motsvarande REST API-kommandot.

| operationName | REST-API-kommando |
| --- | --- |
| Autentisering |Via Azure Active Directory-slutpunkt |
| VaultGet |[Hämta information om ett nyckelvalv](https://msdn.microsoft.com/en-us/library/azure/mt620026.aspx) |
| VaultPut |[Skapa eller uppdatera ett nyckelvalv](https://msdn.microsoft.com/en-us/library/azure/mt620025.aspx) |
| VaultDelete |[Ta bort ett nyckelvalv](https://msdn.microsoft.com/en-us/library/azure/mt620022.aspx) |
| VaultPatch |[Uppdatera ett nyckelvalv](https://msdn.microsoft.com/library/azure/mt620025.aspx) |
| VaultList |[Visa en lista med alla nyckelvalv i en resursgrupp](https://msdn.microsoft.com/en-us/library/azure/mt620027.aspx) |
| KeyCreate |[Skapa en nyckel](https://msdn.microsoft.com/en-us/library/azure/dn903634.aspx) |
| KeyGet |[Hämta information om en nyckel](https://msdn.microsoft.com/en-us/library/azure/dn878080.aspx) |
| KeyImport |[Importera en nyckel till ett valv](https://msdn.microsoft.com/en-us/library/azure/dn903626.aspx) |
| KeyBackup |[Säkerhetskopiera en nyckel](https://msdn.microsoft.com/en-us/library/azure/dn878058.aspx). |
| KeyDelete |[Ta bort en nyckel](https://msdn.microsoft.com/en-us/library/azure/dn903611.aspx) |
| KeyRestore |[Återställa en nyckel](https://msdn.microsoft.com/en-us/library/azure/dn878106.aspx) |
| KeySign |[Signera med en nyckel](https://msdn.microsoft.com/en-us/library/azure/dn878096.aspx) |
| KeyVerify |[Verifiera med en nyckel](https://msdn.microsoft.com/en-us/library/azure/dn878082.aspx) |
| KeyWrap |[Omsluta en nyckel](https://msdn.microsoft.com/en-us/library/azure/dn878066.aspx) |
| KeyUnwrap |[Ta bort en nyckelomslutning](https://msdn.microsoft.com/en-us/library/azure/dn878079.aspx) |
| KeyEncrypt |[Kryptera med en nyckel](https://msdn.microsoft.com/en-us/library/azure/dn878060.aspx) |
| KeyDecrypt |[Dekryptera med en nyckel](https://msdn.microsoft.com/en-us/library/azure/dn878097.aspx) |
| KeyUpdate |[Uppdatera en nyckel](https://msdn.microsoft.com/en-us/library/azure/dn903616.aspx) |
| KeyList |[Lista hello nycklar i ett valv](https://msdn.microsoft.com/en-us/library/azure/dn903629.aspx) |
| KeyListVersions |[Lista hello versioner av en nyckel](https://msdn.microsoft.com/en-us/library/azure/dn986822.aspx) |
| SecretSet |[Skapa en hemlighet](https://msdn.microsoft.com/en-us/library/azure/dn903618.aspx) |
| SecretGet |[Hämta en hemlighet](https://msdn.microsoft.com/en-us/library/azure/dn903633.aspx) |
| SecretUpdate |[Uppdatera en hemlighet](https://msdn.microsoft.com/en-us/library/azure/dn986818.aspx) |
| SecretDelete |[Ta bort en hemlighet](https://msdn.microsoft.com/en-us/library/azure/dn903613.aspx) |
| SecretList |[Visa en lista över hemligheterna i ett valv](https://msdn.microsoft.com/en-us/library/azure/dn903614.aspx) |
| SecretListVersions |[Visa en lista över versionerna av en hemlighet](https://msdn.microsoft.com/en-us/library/azure/dn986824.aspx) |

## <a id="loganalytics"></a>Använda Log Analytics

Du kan använda hello Azure Key Vault-lösning i logganalys tooreview Azure Key Vault AuditEvent loggar. Mer information, inklusive hur tooset detta, se [Azure Key Vault-lösning i logganalys](../log-analytics/log-analytics-azure-key-vault.md). Den här artikeln innehåller också instruktioner om du behöver toomigrate från hello gamla Key Vault-lösning som erbjöds hello logganalys förhandsversionen, där du först dirigeras din loggar tooan Azure Storage-konto och konfigurerat Log Analytics tooread därifrån.

## <a id="next"></a>Nästa steg
En självstudiekurs där Azure Key Vault används i en webbapp finns i [Använda Azure Key Vault från en webbapp](key-vault-use-from-web-application.md).

Programmering referenser finns [hello Azure Key Vault Utvecklarhandbok](key-vault-developers-guide.md).

En lista med Azure PowerShell 1.0-cmdlets för Azure Key Vault finns i [Cmdlets för Azure Key Vault](/powershell/module/azurerm.keyvault/#key_vault).

En självstudiekurs om viktiga rotation och logg granskning med Azure Key Vault finns [hur toosetup Key Vault med end tooend nyckeln rotation och granskning](key-vault-key-rotation-log-monitoring.md).
