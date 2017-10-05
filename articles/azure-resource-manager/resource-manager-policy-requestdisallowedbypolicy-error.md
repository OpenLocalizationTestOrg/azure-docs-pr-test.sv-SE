---
title: RequestDisallowedByPolicy fel med Azure resursprincip | Microsoft Docs
description: Beskriver orsaken till felet RequestDisallowedByPolicy.
services: azure-resource-manager,azure-portal
documentationcenter: 
author: genlin
manager: cshepard
editor: 
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: genli
ms.openlocfilehash: 182a27e444c2f5db66d518a1a0c608d3e319d553
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/29/2017
---
# <a name="requestdisallowedbypolicy-error-with-azure-resource-policy"></a>RequestDisallowedByPolicy fel med Azure resursprincip

Den här artikeln beskriver orsaken till felet RequestDisallowedByPolicy, ger den också lösning för det här felet.

## <a name="symptom"></a>Symtom

När du försöker utföra åtgärden under distributionen kan du få en **RequestDisallowedByPolicy** fel som förhindrar åtgärden utföras. Följande är ett exempel av felet:

```
{
  "statusCode": "Forbidden",
  "serviceRequestId": null,
  "statusMessage": "{\"error\":{\"code\":\"RequestDisallowedByPolicy\",\"message\":\"The resource action 'Microsoft.Network/publicIpAddresses/write' is disallowed by one or more policies. Policy identifier(s): '/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition'.\"}}",
  "responseBody": "{\"error\":{\"code\":\"RequestDisallowedByPolicy\",\"message\":\"The resource action 'Microsoft.Network/publicIpAddresses/write' is disallowed by one or more policies. Policy identifier(s): '/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition'.\"}}"
}
```

## <a name="troubleshooting"></a>Felsökning

Använd följande någon av metoderna för att hämta information om den princip som blockeras distributionen:

### <a name="method-1"></a>Metod 1

I PowerShell anger du den princip-ID som den **Id** parametern för att hämta information om den princip som blockerats av din distribution.

```PowerShell
(Get-AzureRmPolicyDefinition -Id "/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition").Properties.policyRule | ConvertTo-Json
```

### <a name="method-2"></a>Metod 2 

Ange namnet på definitionen av principen i Azure CLI 2.0: 

```azurecli
az policy definition show --name regionPolicyAssignment
```

## <a name="solution"></a>Lösning

IT-avdelningen kan tillämpa en resursprincip som förhindrar att skapa offentlig IP-adresser, Nätverkssäkerhetsgrupper, användardefinierade vägar eller routningstabeller för säkerhet och efterlevnad. I det här exemplet i det felmeddelande som beskrivs i avsnittet ”Symptom” principen heter **regionPolicyDefinition**, men det kan vara olika.
Arbeta med din IT-avdelningen att granska principer för företagsresurser för att lösa problemet, och så att utföra den begärda åtgärden i enlighet med dessa principer.


Mer information finns i följande artiklar:

- [Översikt över princip för resurs](resource-manager-policy.md)
- [Vanliga fel-RequestDisallowedByPolicy för distribution](resource-manager-common-deployment-errors.md#requestdisallowedbypolicy)

 


