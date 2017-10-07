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
# <a name="requestdisallowedbypolicy-error-with-azure-resource-policy"></a><span data-ttu-id="e1c17-103">RequestDisallowedByPolicy fel med Azure resursprincip</span><span class="sxs-lookup"><span data-stu-id="e1c17-103">RequestDisallowedByPolicy error with Azure resource policy</span></span>

<span data-ttu-id="e1c17-104">Den här artikeln beskriver hello felorsak hello RequestDisallowedByPolicy, ger den också lösning för det här felet.</span><span class="sxs-lookup"><span data-stu-id="e1c17-104">This article describes hello cause of hello RequestDisallowedByPolicy error, it also provides solution for this error.</span></span>

## <a name="symptom"></a><span data-ttu-id="e1c17-105">Symtom</span><span class="sxs-lookup"><span data-stu-id="e1c17-105">Symptom</span></span>

<span data-ttu-id="e1c17-106">När du försöker toodo åtgärden under distributionen kan du få en **RequestDisallowedByPolicy** fel som förhindrar hello åtgärden utföras.</span><span class="sxs-lookup"><span data-stu-id="e1c17-106">When you try toodo an action during deployment, you might receive a **RequestDisallowedByPolicy** error that prevents hello action be performed.</span></span> <span data-ttu-id="e1c17-107">hello följande är ett exempel på hello-fel:</span><span class="sxs-lookup"><span data-stu-id="e1c17-107">hello following is a sample of hello error:</span></span>

```
{
  "statusCode": "Forbidden",
  "serviceRequestId": null,
  "statusMessage": "{\"error\":{\"code\":\"RequestDisallowedByPolicy\",\"message\":\"hello resource action 'Microsoft.Network/publicIpAddresses/write' is disallowed by one or more policies. Policy identifier(s): '/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition'.\"}}",
  "responseBody": "{\"error\":{\"code\":\"RequestDisallowedByPolicy\",\"message\":\"hello resource action 'Microsoft.Network/publicIpAddresses/write' is disallowed by one or more policies. Policy identifier(s): '/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition'.\"}}"
}
```

## <a name="troubleshooting"></a><span data-ttu-id="e1c17-108">Felsökning</span><span class="sxs-lookup"><span data-stu-id="e1c17-108">Troubleshooting</span></span>

<span data-ttu-id="e1c17-109">tooretrieve information om hello-princip som blockeras distributionen, Använd hello efter någon av hello metoder:</span><span class="sxs-lookup"><span data-stu-id="e1c17-109">tooretrieve details about hello policy that blocked your deployment, use hello following one of hello methods:</span></span>

### <a name="method-1"></a><span data-ttu-id="e1c17-110">Metod 1</span><span class="sxs-lookup"><span data-stu-id="e1c17-110">Method 1</span></span>

<span data-ttu-id="e1c17-111">Ange den Principidentifierare som hello i PowerShell **Id** parametern tooretrieve information om hello-princip som blockerats av din distribution.</span><span class="sxs-lookup"><span data-stu-id="e1c17-111">In PowerShell, provide that policy identifier as hello **Id** parameter tooretrieve details about hello policy that blocked your deployment.</span></span>

```PowerShell
(Get-AzureRmPolicyDefinition -Id "/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition").Properties.policyRule | ConvertTo-Json
```

### <a name="method-2"></a><span data-ttu-id="e1c17-112">Metod 2</span><span class="sxs-lookup"><span data-stu-id="e1c17-112">Method 2</span></span> 

<span data-ttu-id="e1c17-113">Ange hello namnet på definitionen av hello principen i Azure CLI 2.0:</span><span class="sxs-lookup"><span data-stu-id="e1c17-113">In Azure CLI 2.0, provide hello name of hello policy definition:</span></span> 

```azurecli
az policy definition show --name regionPolicyAssignment
```

## <a name="solution"></a><span data-ttu-id="e1c17-114">Lösning</span><span class="sxs-lookup"><span data-stu-id="e1c17-114">Solution</span></span>

<span data-ttu-id="e1c17-115">IT-avdelningen kan tillämpa en resursprincip som förhindrar att skapa offentlig IP-adresser, Nätverkssäkerhetsgrupper, användardefinierade vägar eller routningstabeller för säkerhet och efterlevnad.</span><span class="sxs-lookup"><span data-stu-id="e1c17-115">For security or compliance, your IT department might enforce a resource policy that prohibits creating Public IP addresses, Network Security Groups, User-Defined Routes, or route tables.</span></span> <span data-ttu-id="e1c17-116">I exemplet hello hello felmeddelande som beskrivs i hello ”symptom” hello princip heter **regionPolicyDefinition**, men det kan vara olika.</span><span class="sxs-lookup"><span data-stu-id="e1c17-116">In hello sample of hello error message that is described in hello "Symptoms" section, hello policy is named **regionPolicyDefinition**, but it could be different.</span></span>
<span data-ttu-id="e1c17-117">tooresolve det här problemet kan arbeta med din IT-avdelningen tooreview hello resursprinciper och avgöra hur tooperform hello den begärda åtgärden enlighet med dessa principer.</span><span class="sxs-lookup"><span data-stu-id="e1c17-117">tooresolve this problem, work with your IT department tooreview hello resource policies, and determine how tooperform hello requested action in compliance with those policies.</span></span>


<span data-ttu-id="e1c17-118">Mer information finns i följande artiklar hello:</span><span class="sxs-lookup"><span data-stu-id="e1c17-118">For more information, see hello following articles:</span></span>

- [<span data-ttu-id="e1c17-119">Översikt över princip för resurs</span><span class="sxs-lookup"><span data-stu-id="e1c17-119">Resource policy overview</span></span>](resource-manager-policy.md)
- [<span data-ttu-id="e1c17-120">Vanliga fel-RequestDisallowedByPolicy för distribution</span><span class="sxs-lookup"><span data-stu-id="e1c17-120">Common deployment errors-RequestDisallowedByPolicy</span></span>](resource-manager-common-deployment-errors.md#requestdisallowedbypolicy)

 


