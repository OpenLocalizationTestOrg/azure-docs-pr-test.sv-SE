---
title: "medgivandetext för aaaUnexpected när du loggar in tooan program | Microsoft Docs"
description: "Hur tootroubleshoot när en användare ser en fråga om medgivande för ett program du har integrerat med Azure AD som du inte förväntar dig"
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
ms.openlocfilehash: 32b7a5e6256faee0dcfe2e1644da3d3428812d35
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="unexpected-consent-prompt-when-signing-in-tooan-application"></a><span data-ttu-id="2767d-103">Oväntat medgivandetext när du loggar in tooan program</span><span class="sxs-lookup"><span data-stu-id="2767d-103">Unexpected consent prompt when signing in tooan application</span></span>

<span data-ttu-id="2767d-104">Många program som integreras med Azure Active Directory kräver behörighet toovarious resurser i ordning toorun.</span><span class="sxs-lookup"><span data-stu-id="2767d-104">Many applications that integrate with Azure Active Directory require permissions toovarious resources in order toorun.</span></span> <span data-ttu-id="2767d-105">När resurserna är även integrerad med Azure Active Directory, medgivande framework behörigheter tooaccess begärs dem med hjälp av hello Azure AD.</span><span class="sxs-lookup"><span data-stu-id="2767d-105">When these resources are also integrated with Azure Active Directory, permissions tooaccess them is requested using hello Azure AD consent framework.</span></span> 

<span data-ttu-id="2767d-106">Detta resulterar i en fråga om medgivande visas hello första gången ett program används, vilket ofta är en gång.</span><span class="sxs-lookup"><span data-stu-id="2767d-106">This results in a consent prompt being shown hello first time an application is used, which is often a one-time operation.</span></span> 

## <a name="scenarios-in-which-users-see-consent-prompts"></a><span data-ttu-id="2767d-107">Scenarier som användarna ser medgivande prompter</span><span class="sxs-lookup"><span data-stu-id="2767d-107">Scenarios in which users see consent prompts</span></span>

<span data-ttu-id="2767d-108">Fler meddelanden kan förväntas i olika scenarier:</span><span class="sxs-lookup"><span data-stu-id="2767d-108">Additional prompts can be expected in various scenarios:</span></span>

* <span data-ttu-id="2767d-109">hello uppsättning behörigheter som krävs av programmet hello har ändrats.</span><span class="sxs-lookup"><span data-stu-id="2767d-109">hello set of permissions required by hello application have changed.</span></span>

* <span data-ttu-id="2767d-110">hello-användare som ursprungligen godkänt toohello programmet var inte en administratör och nu en annan (icke-administratörer) användare använder hello program för hello första gången.</span><span class="sxs-lookup"><span data-stu-id="2767d-110">hello user who originally consented toohello application was not an administrator, and now a different (non-admin) User is using hello application for hello first time.</span></span>

* <span data-ttu-id="2767d-111">hello-användare som godkänt toohello programmet ursprungligen var en administratör, men de samtycker inte på uppdrag av hello hela organisationen.</span><span class="sxs-lookup"><span data-stu-id="2767d-111">hello user who originally consented toohello application was an administrator, but they did not consent on-behalf of hello entire organization.</span></span>

* <span data-ttu-id="2767d-112">med hjälp av programmet hello [inkrementell och dynamiska medgivande](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-compare#incremental-and-dynamic-consent) toorequest ytterligare behörighet efter medgivande beviljats från början.</span><span class="sxs-lookup"><span data-stu-id="2767d-112">hello application is using [incremental and dynamic consent](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-compare#incremental-and-dynamic-consent) toorequest additional permissions after consent was initially granted.</span></span> <span data-ttu-id="2767d-113">Det här används ofta när valfria funktioner i ett program ytterligare kräver behörigheter utöver de som krävs för grundläggande funktioner.</span><span class="sxs-lookup"><span data-stu-id="2767d-113">This is often used when optional features of an application additional require permissions beyond those required for baseline functionality.</span></span>

* <span data-ttu-id="2767d-114">Medgivande återkallades efter beviljas från början.</span><span class="sxs-lookup"><span data-stu-id="2767d-114">Consent was revoked after being granted initially.</span></span>

* <span data-ttu-id="2767d-115">hello utvecklare har konfigurerat hello programmet toorequire en fråga om medgivande varje gång den används (Obs: Detta är inte bästa praxis).</span><span class="sxs-lookup"><span data-stu-id="2767d-115">hello developer has configured hello application toorequire a consent prompt every time it is used (note: this is not best practice).</span></span>

## <a name="next-steps"></a><span data-ttu-id="2767d-116">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2767d-116">Next steps</span></span>

-   [<span data-ttu-id="2767d-117">Appar, behörigheter och medgivande i Azure Active Directory (v1.0 slutpunkten)</span><span class="sxs-lookup"><span data-stu-id="2767d-117">Apps, permissions, and consent in Azure Active Directory (v1.0 endpoint)</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-apps-permissions-consent)

-   [<span data-ttu-id="2767d-118">Scope, behörigheter och medgivande i hello Azure Active Directory (v2.0-slutpunkten)</span><span class="sxs-lookup"><span data-stu-id="2767d-118">Scopes, permissions, and consent in hello Azure Active Directory (v2.0 endpoint)</span></span>](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)


