---
title: "livslängd för aaaHow toochange hello token som standard för en anpassad utvecklade program | Microsoft Docs"
description: "Hur tooupdate livslängd för Token principer för programmet som du utvecklar på Azure AD"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 6e1aa1f2a7c33c1f55c5fb619c618ad43cd96273
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toochange-hello-token-lifetime-defaults-for-a-custom-developed-application"></a><span data-ttu-id="70a62-103">Hur livslängd för token toochange hello standardvärden för ett anpassat utvecklade program</span><span class="sxs-lookup"><span data-stu-id="70a62-103">How toochange hello token lifetime defaults for a custom-developed application</span></span>

<span data-ttu-id="70a62-104">Azure AD Premium kan appen utvecklare och administratörer klient tooconfigure hello livslängd för token som utfärdats för icke-konfidentiella klienter.</span><span class="sxs-lookup"><span data-stu-id="70a62-104">Azure AD Premium allows app developers and tenant admins tooconfigure hello lifetime of tokens issued for non-confidential clients.</span></span> <span data-ttu-id="70a62-105">Livslängd för token policys ställs in på klienten hela bas eller hello resurser som används.</span><span class="sxs-lookup"><span data-stu-id="70a62-105">Token lifetime policies are set on a tenant-wide basis or hello resources being accessed.</span></span>

 * <span data-ttu-id="70a62-106">tooset en livslängd för token-princip måste toodownload hello [Azure AD PowerShell-modulen](https://www.powershellgallery.com/packages/AzureADPreview).</span><span class="sxs-lookup"><span data-stu-id="70a62-106">tooset a token lifetime policy, you need toodownload hello [Azure AD PowerShell Module](https://www.powershellgallery.com/packages/AzureADPreview).</span></span>

 * <span data-ttu-id="70a62-107">Kör hello **Connect-AzureAD-Bekräfta** kommando.</span><span class="sxs-lookup"><span data-stu-id="70a62-107">Run hello **Connect-AzureAD -Confirm** command.</span></span>

 * <span data-ttu-id="70a62-108">Här är en exempelprincip som anger Hej max ålder en faktor uppdateringstoken.</span><span class="sxs-lookup"><span data-stu-id="70a62-108">Here’s an example policy that sets hello max age single factor refresh token.</span></span> <span data-ttu-id="70a62-109">Skapa hello princip:```New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1, "MaxAgeSingleFactor":"until-revoked"}}') -DisplayName "OrganizationDefaultPolicyScenario" -IsOrganizationDefault $true -Type "TokenLifetimePolicy"```</span><span class="sxs-lookup"><span data-stu-id="70a62-109">Create hello policy: ```New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1, "MaxAgeSingleFactor":"until-revoked"}}') -DisplayName "OrganizationDefaultPolicyScenario" -IsOrganizationDefault $true -Type "TokenLifetimePolicy"```</span></span>

 * <span data-ttu-id="70a62-110">Utcheckning hello [konfigurera livslängd för token](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-configurable-token-lifetimes) dokumentera toolearn hur toocreate andra anpassade.</span><span class="sxs-lookup"><span data-stu-id="70a62-110">Checkout hello [Configuring token lifetime](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-configurable-token-lifetimes)   document toolearn how toocreate other custom.</span></span>

## <a name="next-steps"></a><span data-ttu-id="70a62-111">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="70a62-111">Next steps</span></span>
[<span data-ttu-id="70a62-112">Konfigurera livslängd för Token</span><span class="sxs-lookup"><span data-stu-id="70a62-112">Configuring Token Lifetime</span></span>](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-configurable-token-lifetimes)<br>

[<span data-ttu-id="70a62-113">Referens för Azure AD-Token</span><span class="sxs-lookup"><span data-stu-id="70a62-113">Azure AD Token Reference</span></span>](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-token-and-claims)

