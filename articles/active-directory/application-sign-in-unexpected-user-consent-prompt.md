---
title: "Oväntat medgivandetext när du loggar in till ett program | Microsoft Docs"
description: "Felsökning av när en användare ser en fråga om medgivande för ett program som du har integrerat med Azure AD som du inte förväntar dig"
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
ms.openlocfilehash: e5b823e1251a7221f73efe6838d439f827f9665d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="unexpected-consent-prompt-when-signing-in-to-an-application"></a><span data-ttu-id="14a70-103">Oväntat medgivandetext när du loggar in till ett program</span><span class="sxs-lookup"><span data-stu-id="14a70-103">Unexpected consent prompt when signing in to an application</span></span>

<span data-ttu-id="14a70-104">Många program som integreras med Azure Active Directory kräver behörigheter för olika resurser för att kunna köra.</span><span class="sxs-lookup"><span data-stu-id="14a70-104">Many applications that integrate with Azure Active Directory require permissions to various resources in order to run.</span></span> <span data-ttu-id="14a70-105">När dessa resurser också är integrerade med Azure Active Directory, behörighet att komma åt dem begärs med hjälp av Azure AD medgivande framework.</span><span class="sxs-lookup"><span data-stu-id="14a70-105">When these resources are also integrated with Azure Active Directory, permissions to access them is requested using the Azure AD consent framework.</span></span> 

<span data-ttu-id="14a70-106">Detta resulterar i en medgivande fråga som visas första gången ett program används, vilket ofta är en gång.</span><span class="sxs-lookup"><span data-stu-id="14a70-106">This results in a consent prompt being shown the first time an application is used, which is often a one-time operation.</span></span> 

## <a name="scenarios-in-which-users-see-consent-prompts"></a><span data-ttu-id="14a70-107">Scenarier som användarna ser medgivande prompter</span><span class="sxs-lookup"><span data-stu-id="14a70-107">Scenarios in which users see consent prompts</span></span>

<span data-ttu-id="14a70-108">Fler meddelanden kan förväntas i olika scenarier:</span><span class="sxs-lookup"><span data-stu-id="14a70-108">Additional prompts can be expected in various scenarios:</span></span>

* <span data-ttu-id="14a70-109">En uppsättning behörigheter som krävs för programmet har ändrats.</span><span class="sxs-lookup"><span data-stu-id="14a70-109">The set of permissions required by the application have changed.</span></span>

* <span data-ttu-id="14a70-110">Den användare som ursprungligen har samtyckt till programmet var inte en administratör och nu en annan (icke-administratörer) användare använder programmet för första gången.</span><span class="sxs-lookup"><span data-stu-id="14a70-110">The user who originally consented to the application was not an administrator, and now a different (non-admin) User is using the application for the first time.</span></span>

* <span data-ttu-id="14a70-111">Den användare som ursprungligen har samtyckt till att programmet har en administratör, men de samtycker inte på uppdrag av hela organisationen.</span><span class="sxs-lookup"><span data-stu-id="14a70-111">The user who originally consented to the application was an administrator, but they did not consent on-behalf of the entire organization.</span></span>

* <span data-ttu-id="14a70-112">Programmet använder [inkrementell och dynamiska medgivande](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-compare#incremental-and-dynamic-consent) och begär ytterligare behörighet efter medgivande beviljats från början.</span><span class="sxs-lookup"><span data-stu-id="14a70-112">The application is using [incremental and dynamic consent](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-compare#incremental-and-dynamic-consent) to request additional permissions after consent was initially granted.</span></span> <span data-ttu-id="14a70-113">Det här används ofta när valfria funktioner i ett program ytterligare kräver behörigheter utöver de som krävs för grundläggande funktioner.</span><span class="sxs-lookup"><span data-stu-id="14a70-113">This is often used when optional features of an application additional require permissions beyond those required for baseline functionality.</span></span>

* <span data-ttu-id="14a70-114">Medgivande återkallades efter beviljas från början.</span><span class="sxs-lookup"><span data-stu-id="14a70-114">Consent was revoked after being granted initially.</span></span>

* <span data-ttu-id="14a70-115">Utvecklaren har konfigurerat program kräver en fråga om medgivande varje gång den används (Obs: Detta är inte bästa praxis).</span><span class="sxs-lookup"><span data-stu-id="14a70-115">The developer has configured the application to require a consent prompt every time it is used (note: this is not best practice).</span></span>

## <a name="next-steps"></a><span data-ttu-id="14a70-116">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="14a70-116">Next steps</span></span>

-   [<span data-ttu-id="14a70-117">Appar, behörigheter och medgivande i Azure Active Directory (v1.0 slutpunkten)</span><span class="sxs-lookup"><span data-stu-id="14a70-117">Apps, permissions, and consent in Azure Active Directory (v1.0 endpoint)</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-apps-permissions-consent)

-   [<span data-ttu-id="14a70-118">Scope, behörigheter och medgivande i Azure Active Directory (v2.0-slutpunkten)</span><span class="sxs-lookup"><span data-stu-id="14a70-118">Scopes, permissions, and consent in the Azure Active Directory (v2.0 endpoint)</span></span>](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)


