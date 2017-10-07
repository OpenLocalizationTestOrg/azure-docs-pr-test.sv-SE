---
title: "aaaRequire säker överföring i Azure Storage | Microsoft Docs"
description: "Läs mer om hello ”kräver säker överföring” funktionen för Azure Storage, och hur tooenable den."
services: storage
documentationcenter: na
author: fhryo-msft
manager: Jason.Hogg
editor: fhryo-msft
ms.assetid: 
ms.service: storage
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage
ms.date: 06/20/2017
ms.author: fryu
ms.openlocfilehash: 27f745c5e771b50213c1dbb39dee081947be1f39
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="require-secure-transfer"></a>Kräv säker överföring

Hej ”överföringen säker krävs” alternativet ökar hello säkerheten för ditt lagringskonto genom att bara tillåta begäranden toohello storage-konto från säkra anslutningar. Till exempel vid anrop av REST API: er tooaccess storage-konto, måste du ansluta med hjälp av HTTPS. Alla begäranden som använder HTTP avvisas när ”säker överföring krävs” är aktiverat.

När du använder hello Azure Files tjänsten misslyckas en anslutning utan kryptering när ”säker överföring krävs” är aktiverat. Detta inkluderar scenarier med hjälp av SMB 2.1 och SMB 3.0 utan kryptering vissa varianter av hello Linux SMB-klienten. 

Hej ”överföringen säker krävs” alternativet är inaktiverat som standard.

> [!NOTE]
> Eftersom Azure Storage stöder HTTPS för anpassade domännamn, tillämpas inte det här alternativet när du använder ett anpassat domännamn.

## <a name="enable-secure-transfer-required-in-hello-azure-portal"></a>Aktivera ”säker överföring krävs” i hello Azure-portalen

Du kan aktivera hello ”säker överföring krävs” inställningen både när du skapar ett lagringskonto i hello [Azure-portalen](https://portal.azure.com), och för befintliga lagringskonton.

### <a name="require-secure-transfer-when-you-create-a-storage-account"></a>Kräv säker överföring när du skapar ett lagringskonto

1. Öppna hello **skapa lagringskonto** bladet i hello Azure-portalen.
1. Under **säker överföring krävs**väljer **aktiverad**.

  ![Skärmbild](./media/storage-require-secure-transfer/secure_transfer_field_in_portal_en_1.png)

### <a name="require-secure-transfer-for-an-existing-storage-account"></a>Kräv säker överföring för ett befintligt lagringskonto

1. Välj ett befintligt lagringskonto i hello Azure-portalen.
1. Välj **Configuration** under **inställningar** i hello lagring kontoblad-menyn.
1. Under **säker överföring krävs**väljer **aktiverad**.

  ![Skärmbild](./media/storage-require-secure-transfer/secure_transfer_field_in_portal_en_2.png)

## <a name="enable-secure-transfer-required-programmatically"></a>Aktivera ”säker överföring krävs” programmässigt

hello Inställningens namn är _supportsHttpsTrafficOnly_ i lagringskontoegenskaperna. Du kan aktivera ”säker överföring krävs” med REST API eller verktyg, bibliotek:

* **REST API** (Version: 2016-12-01): [versionspaket](https://docs.microsoft.com/en-us/rest/api/storagerp/storageaccounts)
* **PowerShell** (Version: 4.1.0): [versionspaket](https://docs.microsoft.com/en-us/powershell/module/azurerm.storage/set-azurermstorageaccount?view=azurermps-4.1.0)
* **CLI** (Version: 2.0.11): [versionspaket](https://pypi.python.org/pypi/azure-cli-storage/2.0.11)
* **NodeJS** (Version: 1.1.0): [versionspaket](https://www.npmjs.com/package/azure-arm-storage/)
* **.NET SDK** (Version: 6.3.0): [versionspaket](https://www.nuget.org/packages/Microsoft.Azure.Management.Storage/6.3.0-preview)
* **Python SDK** (Version: 1.1.0): [versionspaket](https://pypi.python.org/pypi/azure-mgmt-storage/1.1.0)
* **Ruby SDK** (Version: 0.11.0): [versionspaket](https://rubygems.org/gems/azure_mgmt_storage)

### <a name="enable-secure-transfer-required-setting-with-rest-api"></a>Aktivera ”säker överföring krävs” med REST API

toosimplify testning med REST API, som du kan använda [ArmClient](https://github.com/projectkudu/ARMClient) toocall från kommandoraden.

 Du kan använda under kommandoraden toocheck hello med hello REST-API:

```
# Login Azure and proceed with your credentials
> armclient login

> armclient GET  /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{accountName}?api-version=2016-12-01
```

Hello svar hittar du _supportsHttpsTrafficOnly_ inställningen. Exempel:

```Json
{
  "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{accountName}",
  "kind": "Storage",
  ...
  "properties": {
    ...
    "supportsHttpsTrafficOnly": false
  },
  "type": "Microsoft.Storage/storageAccounts"
}
```

Du kan använda under kommandoraden tooenable hello med hello REST-API:

```
# Login Azure and proceed with your credentials
> armclient login

> armclient PUT /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{accountName}?api-version=2016-12-01 < Input.json
```
Exempel på Input.json:
```Json
{
  "location": "westus",
  "properties": {
    "supportsHttpsTrafficOnly": true
  }
}
```

## <a name="next-steps"></a>Nästa steg
Azure Storage tillhandahåller en omfattande uppsättning säkerhetsfunktioner för, vilket tillsammans ger utvecklare möjligheten toobuild säkra program. Mer information finns i hello [säkerhetsguiden för lagring](storage-security-guide.md).
