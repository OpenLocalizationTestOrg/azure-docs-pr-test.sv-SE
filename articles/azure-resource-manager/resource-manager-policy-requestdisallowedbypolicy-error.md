---
title: aaaRequestDisallowedByPolicy fel med Azure resursprincip | Microsoft Docs
description: Beskriver hello orsaken till hello RequestDisallowedByPolicy fel.
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
ms.openlocfilehash: 7870e40205cf433ccb4ba02376b5fe809f20d0df
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="requestdisallowedbypolicy-error-with-azure-resource-policy"></a>RequestDisallowedByPolicy fel med Azure resursprincip

Den här artikeln beskriver hello felorsak hello RequestDisallowedByPolicy, ger den också lösning för det här felet.

## <a name="symptom"></a>Symtom

När du försöker toodo åtgärden under distributionen kan du få en **RequestDisallowedByPolicy** fel som förhindrar hello åtgärden utföras. hello följande är ett exempel på hello-fel:

```
{
  "statusCode": "Forbidden",
  "serviceRequestId": null,
  "statusMessage": "{\"error\":{\"code\":\"RequestDisallowedByPolicy\",\"message\":\"hello resource action 'Microsoft.Network/publicIpAddresses/write' is disallowed by one or more policies. Policy identifier(s): '/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition'.\"}}",
  "responseBody": "{\"error\":{\"code\":\"RequestDisallowedByPolicy\",\"message\":\"hello resource action 'Microsoft.Network/publicIpAddresses/write' is disallowed by one or more policies. Policy identifier(s): '/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition'.\"}}"
}
```

## <a name="troubleshooting"></a>Felsökning

tooretrieve information om hello-princip som blockeras distributionen, Använd hello efter någon av hello metoder:

### <a name="method-1"></a>Metod 1

Ange den Principidentifierare som hello i PowerShell **Id** parametern tooretrieve information om hello-princip som blockerats av din distribution.

```PowerShell
(Get-AzureRmPolicyDefinition -Id "/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition").Properties.policyRule | ConvertTo-Json
```

### <a name="method-2"></a>Metod 2 

Ange hello namnet på definitionen av hello principen i Azure CLI 2.0: 

```azurecli
az policy definition show --name regionPolicyAssignment
```

## <a name="solution"></a>Lösning

IT-avdelningen kan tillämpa en resursprincip som förhindrar att skapa offentlig IP-adresser, Nätverkssäkerhetsgrupper, användardefinierade vägar eller routningstabeller för säkerhet och efterlevnad. I exemplet hello hello felmeddelande som beskrivs i hello ”symptom” hello princip heter **regionPolicyDefinition**, men det kan vara olika.
tooresolve det här problemet kan arbeta med din IT-avdelningen tooreview hello resursprinciper och avgöra hur tooperform hello den begärda åtgärden enlighet med dessa principer.


Mer information finns i följande artiklar hello:

- [Översikt över princip för resurs](resource-manager-policy.md)
- [Vanliga fel-RequestDisallowedByPolicy för distribution](resource-manager-common-deployment-errors.md#requestdisallowedbypolicy)

 


