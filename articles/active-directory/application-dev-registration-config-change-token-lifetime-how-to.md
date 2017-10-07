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
# <a name="how-toochange-hello-token-lifetime-defaults-for-a-custom-developed-application"></a>Hur livslängd för token toochange hello standardvärden för ett anpassat utvecklade program

Azure AD Premium kan appen utvecklare och administratörer klient tooconfigure hello livslängd för token som utfärdats för icke-konfidentiella klienter. Livslängd för token policys ställs in på klienten hela bas eller hello resurser som används.

 * tooset en livslängd för token-princip måste toodownload hello [Azure AD PowerShell-modulen](https://www.powershellgallery.com/packages/AzureADPreview).

 * Kör hello **Connect-AzureAD-Bekräfta** kommando.

 * Här är en exempelprincip som anger Hej max ålder en faktor uppdateringstoken. Skapa hello princip:```New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1, "MaxAgeSingleFactor":"until-revoked"}}') -DisplayName "OrganizationDefaultPolicyScenario" -IsOrganizationDefault $true -Type "TokenLifetimePolicy"```

 * Utcheckning hello [konfigurera livslängd för token](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-configurable-token-lifetimes) dokumentera toolearn hur toocreate andra anpassade.

## <a name="next-steps"></a>Nästa steg
[Konfigurera livslängd för Token](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-configurable-token-lifetimes)<br>

[Referens för Azure AD-Token](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-token-and-claims)

