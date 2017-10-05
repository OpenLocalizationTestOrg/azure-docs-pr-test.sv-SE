---
title: "Så här ändrar du livslängd för token som standard för en anpassad utvecklade program | Microsoft Docs"
description: "Så här uppdaterar du principer för livslängd för Token för programmet som du utvecklar på Azure AD"
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
ms.openlocfilehash: a28eacd820ed28a6470992ce86b060e886c00bcb
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-change-the-token-lifetime-defaults-for-a-custom-developed-application"></a><span data-ttu-id="5dd2c-103">Så här ändrar du standardvärdena livslängd för token för en anpassad utvecklade program</span><span class="sxs-lookup"><span data-stu-id="5dd2c-103">How to change the token lifetime defaults for a custom-developed application</span></span>

<span data-ttu-id="5dd2c-104">Azure AD Premium kan utvecklare av program och innehavaradministratörer för att konfigurera livslängd för token som utfärdats för icke-konfidentiella klienter.</span><span class="sxs-lookup"><span data-stu-id="5dd2c-104">Azure AD Premium allows app developers and tenant admins to configure the lifetime of tokens issued for non-confidential clients.</span></span> <span data-ttu-id="5dd2c-105">Livslängd för token policys ställs in på klienten hela bas eller resurser som används.</span><span class="sxs-lookup"><span data-stu-id="5dd2c-105">Token lifetime policies are set on a tenant-wide basis or the resources being accessed.</span></span>

 * <span data-ttu-id="5dd2c-106">Om du vill ange en livslängd för token-princip måste du hämta den [Azure AD PowerShell-modulen](https://www.powershellgallery.com/packages/AzureADPreview).</span><span class="sxs-lookup"><span data-stu-id="5dd2c-106">To set a token lifetime policy, you need to download the [Azure AD PowerShell Module](https://www.powershellgallery.com/packages/AzureADPreview).</span></span>

 * <span data-ttu-id="5dd2c-107">Kör den **Connect-AzureAD-Bekräfta** kommando.</span><span class="sxs-lookup"><span data-stu-id="5dd2c-107">Run the **Connect-AzureAD -Confirm** command.</span></span>

 * <span data-ttu-id="5dd2c-108">Här är en exempelprincip som anger den maximala ålder en faktor uppdateringstoken.</span><span class="sxs-lookup"><span data-stu-id="5dd2c-108">Here’s an example policy that sets the max age single factor refresh token.</span></span> <span data-ttu-id="5dd2c-109">Skapa principen:```New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1, "MaxAgeSingleFactor":"until-revoked"}}') -DisplayName "OrganizationDefaultPolicyScenario" -IsOrganizationDefault $true -Type "TokenLifetimePolicy"```</span><span class="sxs-lookup"><span data-stu-id="5dd2c-109">Create the policy: ```New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1, "MaxAgeSingleFactor":"until-revoked"}}') -DisplayName "OrganizationDefaultPolicyScenario" -IsOrganizationDefault $true -Type "TokenLifetimePolicy"```</span></span>

 * <span data-ttu-id="5dd2c-110">Checka ut den [konfigurera livslängd för token](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-configurable-token-lifetimes) dokument för att lära dig hur du skapar andra anpassade.</span><span class="sxs-lookup"><span data-stu-id="5dd2c-110">Checkout the [Configuring token lifetime](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-configurable-token-lifetimes)   document to learn how to create other custom.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5dd2c-111">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5dd2c-111">Next steps</span></span>
[<span data-ttu-id="5dd2c-112">Konfigurera livslängd för Token</span><span class="sxs-lookup"><span data-stu-id="5dd2c-112">Configuring Token Lifetime</span></span>](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-configurable-token-lifetimes)<br>

[<span data-ttu-id="5dd2c-113">Referens för Azure AD-Token</span><span class="sxs-lookup"><span data-stu-id="5dd2c-113">Azure AD Token Reference</span></span>](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-token-and-claims)

