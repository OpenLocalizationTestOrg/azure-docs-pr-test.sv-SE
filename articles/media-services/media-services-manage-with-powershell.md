---
title: aaaManage Azure Media Services-konton med PowerShell
description: "Lär dig hur toomanage Azure Media Services-konton med PowerShell-cmdlets."
author: Juliako
manager: erikre
editor: 
services: media-services
documentationcenter: 
ms.assetid: 17a10c25-d94f-421c-b6bc-ae0958e2ac96
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/03/2016
ms.author: juliako
ms.openlocfilehash: e8f97bb2393343e45fabf9c437b4fc09f2525dc2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-media-services-accounts-with-powershell"></a>Hantera Azure Media Services-konton med PowerShell
> [!div class="op_single_selector"]
> * [Portal](media-services-portal-create-account.md)
> * [PowerShell](media-services-manage-with-powershell.md)
> * [REST](https://docs.microsoft.com/rest/api/media/mediaservice)
> 
> [!NOTE]
> toobe kan toocreate ett Azure Media Services-konto, måste du ha ett Azure-konto. Om du inte har något konto kan skapa du ett kostnadsfritt utvärderingskonto på bara några minuter. Mer information om den <a href="http://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A8A8397B5" target="_blank">kostnadsfria utvärderingsversionen av Azure</a>.
> 
> 

## <a name="overview"></a>Översikt
Den här artikeln innehåller hello Azure PowerShell-cmdlets för Azure Media Services (AMS) hello Azure Resource Manager framework. hello-cmdlets finns i hello **Microsoft.Azure.Commands.Media** namnområde.

## <a name="versions"></a>Versioner
**ApiVersion**: ”2015-10-01”

## <a name="new-azurermmediaservice"></a>New-AzureRmMediaService
Skapar en medietjänst.

### <a name="syntax"></a>Syntax
Parameteruppsättningen: StorageAccountIdParamSet

    New-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string> [-Location] <string> [-StorageAccountId] <string> [-Tags <hashtable>]  [<CommonParameters>]

Parameteruppsättningen: StorageAccountsParamSet

    New-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string> [-Location] <string> [-StorageAccounts] <PSStorageAccount[]> [-Tags <hashtable>]  [<CommonParameters>]

### <a name="parameters"></a>Parametrar
**-ResourceGroupName &lt;sträng&gt;**

Anger hello hello resurs grupp toowhich media tjänsten tillhör namn.

| Alias | Ingen |
| --- | --- |
| Krävs? |SANT |
| Placera? |0 |
| Standardvärde |Ingen |
| Acceptera indata från pipeline? |true(ByPropertyName) |
| Acceptera jokertecken? |FALSKT |

**-AccountName &lt;sträng&gt;**

Anger hello för hello media service.

| Alias | Namn |
| --- | --- |
| Krävs? |SANT |
| Placera? |1 |
| Standardvärde |Ingen |
| Acceptera indata från pipeline? |FALSKT |
| Acceptera jokertecken? |FALSKT |

**-Plats &lt;sträng&gt;**

Anger hello resursplats av hello media service.

| Alias | Ingen |
| --- | --- |
| Krävs? |SANT |
| Placera? |2 |
| Standardvärde |Ingen |
| Acceptera indata från pipeline? |true(ByPropertyName) |
| Acceptera jokertecken? |FALSKT |

**-StorageAccountId &lt;sträng&gt;**

Anger en primär storage-konto som tillhör hello media service.

* Nytt lagringskonto (som skapats med hello Resource Manager API) stöds endast.
* hello storage-konto måste finnas och har hello hello media service samma plats.

| Alias | Ingen |
| --- | --- |
| Krävs? |SANT |
| Placera? |3 |
| Standardvärde |Ingen |
| Acceptera indata från pipeline? |true(ByPropertyName) |
| Ange namn för parametern |StorageAccountIdParamSet |
| Acceptera jokertecken? |FALSKT |

**-StorageAccounts &lt;PSStorageAccount\[\]&gt;**

Anger storage-konton som är associerade med hello media service.

* Nytt lagringskonto (som skapats med hello Resource Manager API) stöds endast.
* hello storage-konto måste finnas och har hello hello media service samma plats.
* Endast ett lagringskonto kan anges som primär.

| Alias | Ingen |
| --- | --- |
| Krävs? |SANT |
| Placera? |3 |
| Standardvärde |Ingen |
| Acceptera indata från pipeline? |true(ByPropertyName) |
| Ange namn för parametern |StorageAccountsParamSet |
| Acceptera jokertecken? |FALSKT |

**-Taggar &lt;hash-tabell&gt;**

Anger en hash-tabell hello-taggar som är associerade med hello media service.

* Exempel: @{”tag1” = ”value1” ”; tag2 ”=: value2”}

| Alias | Ingen |
| --- | --- |
| Krävs? |FALSKT |
| Placera? |Med namnet |
| Standardvärde |Ingen |
| Acceptera indata från pipeline? |FALSKT |
| Acceptera jokertecken? |FALSKT |

**&lt;CommandParameters&gt;**

Denna cmdlet stöder hello vanliga parametrar:-Debug, - ErrorAction, - ErrorVariable, - InformationAction, -InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - Verbose, - WarningAction och -WarningVariable.

### <a name="inputs"></a>Indata
hello indatatypen är hello typ av hello objekt kan du skicka toohello cmdlet.

### <a name="outputs"></a>utdata
hello utdatatypen är hello typ av hello-objekt som hello cmdleten genererar.

## <a name="set-azurermmediaservice"></a>Set-AzureRmMediaService
Uppdaterar en medietjänst.

### <a name="syntax"></a>Syntax
    Set-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string> [-Tags <hashtable>] [-StorageAccounts <PSStorageAccount[]>]  [<CommonParameters>]

### <a name="parameters"></a>Parametrar
**-ResourceGroupName &lt;sträng&gt;**

Anger hello hello resurs grupp toowhich media tjänsten tillhör namn.

| Alias | Ingen |
| --- | --- |
| Krävs? |SANT |
| Placera? |0 |
| Standardvärde |Ingen |
| Acceptera indata från Pipeline? |true(ByPropertyName) |
| Acceptera jokertecken? |FALSKT |

**-AccountName &lt;sträng&gt;**

Anger hello för hello media service.

| Alias | Namn |
| --- | --- |
| Krävs? |True |
| Placera? |1 |
| Standardvärde |Ingen |
| Acceptera indata från pipeline? |true(ByPropertyName) |
| Acceptera jokertecken? |False |

**-StorageAccounts &lt;PSStorageAccount\[\]&gt;**

Anger storage-konton som är associerade med hello media service.

* Nytt lagringskonto (som skapats med hello Resource Manager API) stöds endast.
* hello storage-konto måste finnas och har hello hello media service samma plats.
* Endast ett lagringskonto kan anges som primär.

| Alias | Ingen |
| --- | --- |
| Krävs? |FALSKT |
| Placera? |Med namnet |
| Standardvärde |Ingen |
| Acceptera indata från pipeline? |true(ByPropertyName) |
| Ange namn för parametern |StorageAccountsParamSet |
| Acceptera jokertecken? |FALSKT |

**-Taggar &lt;hash-tabell&gt;**

Anger en hash-tabell hello-taggar som är associerade med den här media-tjänsten.

* hello-taggar som är associerade med hello media service ersätts med värdet som anges av hello kunden.

| Alias | Ingen |
| --- | --- |
| Krävs? |False |
| Placera? |Med namnet |
| Standardvärde |Ingen |
| Acceptera indata från pipeline? |true(ByPropertyName) |
| Acceptera jokertecken? |FALSKT |

**&lt;CommandParameters&gt;**

Denna cmdlet stöder hello vanliga parametrar:-Debug, - ErrorAction, - ErrorVariable, - InformationAction, -InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - Verbose, - WarningAction och -WarningVariable.

### <a name="inputs"></a>Indata
hello indatatypen är hello typ av hello objekt kan du skicka toohello cmdlet.

### <a name="outputs"></a>utdata
hello utdatatypen är hello typ av hello-objekt som hello cmdleten genererar.

## <a name="remove-azurermmediaservice"></a>Remove-AzureRmMediaService
Tar bort en medietjänst.

### <a name="syntax"></a>Syntax
    Remove-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string>  [<CommonParameters>]

### <a name="parameters"></a>Parametrar
**-ResourceGroupName &lt;sträng&gt;**

Anger hello hello resurs grupp toowhich media tjänsten tillhör namn.

| Alias | Ingen |
| --- | --- |
| Krävs? |SANT |
| Placera? |0 |
| Standardvärde |Ingen |
| Acceptera indata från pipeline? |true(ByPropertyName) |
| Acceptera jokertecken? |FALSKT |

**-AccountName &lt;sträng&gt;**

Anger hello för hello media service.

| Alias | Ingen |
| --- | --- |
| Krävs? |SANT |
| Placera? |2 |
| Standardvärde |Ingen |
| Acceptera indata från pipeline? |true(ByPropertyName) |
| Acceptera jokertecken? |False |

**&lt;CommandParameters&gt;**

Denna cmdlet stöder hello vanliga parametrar:-Debug, - ErrorAction, - ErrorVariable, - InformationAction, -InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - Verbose, - WarningAction och -WarningVariable.

### <a name="inputs"></a>Indata
hello indatatypen är hello typ av hello objekt kan du skicka toohello cmdlet.

### <a name="outputs"></a>utdata
hello utdatatypen är hello typ av hello-objekt som hello cmdleten genererar.

## <a name="get-azurermmediaservice"></a>Get-AzureRmMediaService
Hämtar alla media services i en resursgrupp eller en medietjänst med ett angivet namn.

### <a name="syntax"></a>Syntax
ParameterSet: ResourceGroupParameterSet

    Get-AzureRmMediaService [-ResourceGroupName] <string>  [<CommonParameters>]    

ParameterSet: AccountNameParameterSet

    Get-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string>  [<CommonParameters>]

### <a name="parameters"></a>Parametrar
**-ResourceGroupName &lt;sträng&gt;**

Anger hello hello resurs grupp toowhich media tjänsten tillhör namn.

| Alias | Ingen |
| --- | --- |
| Krävs? |SANT |
| Placera? |0 |
| Standardvärde |Ingen |
| Acceptera indata från pipeline? |true(ByPropertyName) |
| Ange namn för parametern |ResourceGroupParameterSet AccountNameParameterSet |

Acceptera jokertecken?   FALSKT

**-AccountName &lt;sträng&gt;**

Anger hello för hello media service.

| Alias | Ingen |
| --- | --- |
| Krävs? |SANT |
| Placera? |1 |
| Standardvärde |Ingen |
| Acceptera indata från pipeline? |true(ByPropertyName) |
| Ange namn för parametern |AccountNameParameterSet |
| Acceptera jokertecken? |FALSKT |

**&lt;CommandParameters&gt;**

Denna cmdlet stöder hello vanliga parametrar:-Debug, - ErrorAction, - ErrorVariable, - InformationAction, -InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - Verbose, - WarningAction och -WarningVariable.

### <a name="inputs"></a>Indata
hello indatatypen är hello typ av hello objekt kan du skicka toohello cmdlet.

### <a name="outputs"></a>utdata
hello utdatatypen är hello typ av hello-objekt som hello cmdleten genererar.

## <a name="get-azurermmediaservicekeys"></a>Get-AzureRmMediaServiceKeys
Hämtar en medietjänst nycklar.

### <a name="syntax"></a>Syntax
    Get-AzureRmMediaServiceKeys [-ResourceGroupName] <string> [-AccountName] <string>  [<CommonParameters>]

### <a name="parameters"></a>Parametrar
**-ResourceGroupName &lt;sträng&gt;**

Anger hello hello resurs grupp toowhich media tjänsten tillhör namn.

| Alias | Ingen |
| --- | --- |
| Krävs? |SANT |
| Placera? |0 |
| Standardvärde |Ingen |
| Acceptera indata från pipeline? |true(ByPropertyName) |
| Acceptera jokertecken? |FALSKT |

**-AccountName &lt;sträng&gt;**

Anger hello för hello media service.

| Alias | Ingen |
| --- | --- |
| Krävs? |SANT |
| Placera? |1 |
| Standardvärde |Ingen |
| Acceptera indata från pipeline? |true(ByPropertyName) |
| Acceptera jokertecken? |FALSKT |

**&lt;CommandParameters&gt;**

Denna cmdlet stöder hello vanliga parametrar:-Debug, - ErrorAction, - ErrorVariable, - InformationAction, -InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - Verbose, - WarningAction och -WarningVariable.

### <a name="inputs"></a>Indata
hello indatatypen är hello typ av hello objekt kan du skicka toohello cmdlet.

### <a name="outputs"></a>utdata
hello utdatatypen är hello typ av hello-objekt som hello cmdleten genererar.

## <a name="set-azurermmediaservicekey"></a>Set-AzureRmMediaServiceKey
Återskapar en primär eller sekundär nyckel för en medietjänst.

### <a name="syntax"></a>Syntax
    Set-AzureRmMediaServiceKey [-ResourceGroupName] <string> [-AccountName] <string> [-KeyType] <KeyType> {Primary | Secondary}  [<CommonParameters>]

### <a name="parameters"></a>Parametrar
**-ResourceGroupName &lt;sträng&gt;**

Anger hello hello resurs grupp toowhich media tjänsten tillhör namn.

| Alias | Ingen |
| --- | --- |
| Krävs? |SANT |
| Placera? |0 |
| Standardvärde |Ingen |
| Acceptera indata från pipeline? |true(ByPropertyName) |
| Acceptera jokertecken? |FALSKT |

**-AccountName &lt;sträng&gt;**

Anger hello för hello media service.

| Alias | Ingen |
| --- | --- |
| Krävs? |SANT |
| Placera? |1 |
| Standardvärde |Ingen |
| Acceptera indata från pipeline? |true(ByPropertyName) |
| Acceptera jokertecken? |FALSKT |

**KeyType - &lt;KeyType&gt;**

Anger hello viktiga hello media service.

* Primär eller sekundär

| Alias | Ingen |
| --- | --- |
| Krävs? |SANT |
| Placera? |2 |
| Standardvärde |Ingen |
| Acceptera indata från pipeline? |FALSKT |
| Acceptera jokertecken? |FALSKT |

**&lt;CommandParameters&gt;**

Denna cmdlet stöder hello vanliga parametrar:-Debug, - ErrorAction, - ErrorVariable, - InformationAction, -InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - Verbose, - WarningAction och -WarningVariable.

### <a name="inputs"></a>Indata
hello indatatypen är hello typ av hello objekt kan du skicka toothe cmdlet.

### <a name="outputs"></a>utdata
hello utdatatypen är hello typ av hello-objekt som hello cmdleten genererar.

## <a name="sync-azurermmediaservicestoragekeys"></a>Sync-AzureRmMediaServiceStorageKeys
Synkroniserar lagringskontonycklar för ett lagringskonto som är associerade med hello media service.

### <a name="syntax"></a>Syntax
    Sync-AzureRmMediaServiceStorageKeys [-ResourceGroupName] <string> [-MediaServiceAccountName] <string>    [-StorageAccountId] <string>  [<CommonParameters>]

### <a name="parameters"></a>Parametrar
**-ResourceGroupName &lt;sträng&gt;**

Anger hello hello resurs grupp toowhich media tjänsten tillhör namn.

| Alias | Ingen |
| --- | --- |
| Krävs? |SANT |
| Placera? |0 |
| Standardvärde |Ingen |
| Acceptera indata från pipeline? |true(ByPropertyName) |
| Acceptera jokertecken? |FALSKT |

**-AccountName &lt;sträng&gt;**

Anger hello för hello media service.

| Alias | Ingen |
| --- | --- |
| Krävs? |SANT |
| Placera? |1 |
| Standardvärde |Ingen |
| Acceptera indata från pipeline? |true(ByPropertyName) |
| Acceptera jokertecken? |FALSKT |

**-StorageAccountId &lt;sträng&gt;**

Anger hello storage-konto som är associerade med hello media service.

| Alias | Id |
| --- | --- |
| Krävs? |SANT |
| Placera? |2 |
| Standardvärde |Ingen |
| Acceptera indata från pipeline? |true(ByPropertyName) |
| Acceptera jokertecken? |FALSKT |

**&lt;CommandParameters&gt;**

Denna cmdlet stöder hello vanliga parametrar:-Debug, - ErrorAction, - ErrorVariable, - InformationAction, -InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - Verbose, - WarningAction och -WarningVariable.

### <a name="inputs"></a>Indata
hello indatatypen är hello typ av hello objekt kan du skicka toohello cmdlet.

### <a name="outputs"></a>utdata
hello utdatatypen är hello typ av hello-objekt som hello cmdleten genererar.

## <a name="next-step"></a>Nästa steg
Checka ut utbildningsvägar för Media Services.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

