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
# <a name="requestdisallowedbypolicy-error-with-azure-resource-policy"></a><span data-ttu-id="935d6-103">RequestDisallowedByPolicy fel med Azure resursprincip</span><span class="sxs-lookup"><span data-stu-id="935d6-103">RequestDisallowedByPolicy error with Azure resource policy</span></span>

<span data-ttu-id="935d6-104">Den här artikeln beskriver orsaken till felet RequestDisallowedByPolicy, ger den också lösning för det här felet.</span><span class="sxs-lookup"><span data-stu-id="935d6-104">This article describes the cause of the RequestDisallowedByPolicy error, it also provides solution for this error.</span></span>

## <a name="symptom"></a><span data-ttu-id="935d6-105">Symtom</span><span class="sxs-lookup"><span data-stu-id="935d6-105">Symptom</span></span>

<span data-ttu-id="935d6-106">När du försöker utföra åtgärden under distributionen kan du få en **RequestDisallowedByPolicy** fel som förhindrar åtgärden utföras.</span><span class="sxs-lookup"><span data-stu-id="935d6-106">When you try to do an action during deployment, you might receive a **RequestDisallowedByPolicy** error that prevents the action be performed.</span></span> <span data-ttu-id="935d6-107">Följande är ett exempel av felet:</span><span class="sxs-lookup"><span data-stu-id="935d6-107">The following is a sample of the error:</span></span>

```
{
  "statusCode": "Forbidden",
  "serviceRequestId": null,
  "statusMessage": "{\"error\":{\"code\":\"RequestDisallowedByPolicy\",\"message\":\"The resource action 'Microsoft.Network/publicIpAddresses/write' is disallowed by one or more policies. Policy identifier(s): '/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition'.\"}}",
  "responseBody": "{\"error\":{\"code\":\"RequestDisallowedByPolicy\",\"message\":\"The resource action 'Microsoft.Network/publicIpAddresses/write' is disallowed by one or more policies. Policy identifier(s): '/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition'.\"}}"
}
```

## <a name="troubleshooting"></a><span data-ttu-id="935d6-108">Felsökning</span><span class="sxs-lookup"><span data-stu-id="935d6-108">Troubleshooting</span></span>

<span data-ttu-id="935d6-109">Använd följande någon av metoderna för att hämta information om den princip som blockeras distributionen:</span><span class="sxs-lookup"><span data-stu-id="935d6-109">To retrieve details about the policy that blocked your deployment, use the following one of the methods:</span></span>

### <a name="method-1"></a><span data-ttu-id="935d6-110">Metod 1</span><span class="sxs-lookup"><span data-stu-id="935d6-110">Method 1</span></span>

<span data-ttu-id="935d6-111">I PowerShell anger du den princip-ID som den **Id** parametern för att hämta information om den princip som blockerats av din distribution.</span><span class="sxs-lookup"><span data-stu-id="935d6-111">In PowerShell, provide that policy identifier as the **Id** parameter to retrieve details about the policy that blocked your deployment.</span></span>

```PowerShell
(Get-AzureRmPolicyDefinition -Id "/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition").Properties.policyRule | ConvertTo-Json
```

### <a name="method-2"></a><span data-ttu-id="935d6-112">Metod 2</span><span class="sxs-lookup"><span data-stu-id="935d6-112">Method 2</span></span> 

<span data-ttu-id="935d6-113">Ange namnet på definitionen av principen i Azure CLI 2.0:</span><span class="sxs-lookup"><span data-stu-id="935d6-113">In Azure CLI 2.0, provide the name of the policy definition:</span></span> 

```azurecli
az policy definition show --name regionPolicyAssignment
```

## <a name="solution"></a><span data-ttu-id="935d6-114">Lösning</span><span class="sxs-lookup"><span data-stu-id="935d6-114">Solution</span></span>

<span data-ttu-id="935d6-115">IT-avdelningen kan tillämpa en resursprincip som förhindrar att skapa offentlig IP-adresser, Nätverkssäkerhetsgrupper, användardefinierade vägar eller routningstabeller för säkerhet och efterlevnad.</span><span class="sxs-lookup"><span data-stu-id="935d6-115">For security or compliance, your IT department might enforce a resource policy that prohibits creating Public IP addresses, Network Security Groups, User-Defined Routes, or route tables.</span></span> <span data-ttu-id="935d6-116">I det här exemplet i det felmeddelande som beskrivs i avsnittet ”Symptom” principen heter **regionPolicyDefinition**, men det kan vara olika.</span><span class="sxs-lookup"><span data-stu-id="935d6-116">In the sample of the error message that is described in the "Symptoms" section, the policy is named **regionPolicyDefinition**, but it could be different.</span></span>
<span data-ttu-id="935d6-117">Arbeta med din IT-avdelningen att granska principer för företagsresurser för att lösa problemet, och så att utföra den begärda åtgärden i enlighet med dessa principer.</span><span class="sxs-lookup"><span data-stu-id="935d6-117">To resolve this problem, work with your IT department to review the resource policies, and determine how to perform the requested action in compliance with those policies.</span></span>


<span data-ttu-id="935d6-118">Mer information finns i följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="935d6-118">For more information, see the following articles:</span></span>

- [<span data-ttu-id="935d6-119">Översikt över princip för resurs</span><span class="sxs-lookup"><span data-stu-id="935d6-119">Resource policy overview</span></span>](resource-manager-policy.md)
- [<span data-ttu-id="935d6-120">Vanliga fel-RequestDisallowedByPolicy för distribution</span><span class="sxs-lookup"><span data-stu-id="935d6-120">Common deployment errors-RequestDisallowedByPolicy</span></span>](resource-manager-common-deployment-errors.md#requestdisallowedbypolicy)

 


