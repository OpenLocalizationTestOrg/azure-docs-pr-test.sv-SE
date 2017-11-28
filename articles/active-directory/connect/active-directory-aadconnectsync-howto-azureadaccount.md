---
title: "Azure AD Connect-synkronisering: hur toomanage hello Azure AD-tjänstkontot | Microsoft Docs"
description: "Det här avsnittet beskrivs hur toorestore hello Azure AD-tjänstkontot."
services: active-directory
keywords: "AADSTS70002, AADSTS50054, hello hur tooreset hello lösenordet för Azure AD Connect-synkronisering Connector-tjänstkontot"
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: 6077043a-27f1-4304-a44b-81dc46620f24
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: e563518eae173de42a1d40bb5a76e63f29f9da42
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-how-toomanage-hello-azure-ad-service-account"></a><span data-ttu-id="92265-104">Azure AD Connect-synkronisering: hur toomanage hello Azure AD-tjänstkonto</span><span class="sxs-lookup"><span data-stu-id="92265-104">Azure AD Connect sync: How toomanage hello Azure AD service account</span></span>
<span data-ttu-id="92265-105">hello-tjänstkontot som används av hello Azure AD Connector måste toobe tjänsten gratis.</span><span class="sxs-lookup"><span data-stu-id="92265-105">hello service account used by hello Azure AD Connector is supposed toobe service free.</span></span> <span data-ttu-id="92265-106">Om du behöver tooreset autentiseringsuppgifterna, är det här avsnittet för dig.</span><span class="sxs-lookup"><span data-stu-id="92265-106">If you need tooreset its credentials, then this topic is for you.</span></span> <span data-ttu-id="92265-107">Om exempelvis en Global administratör har av misstag Återställ hello lösenord på hello-tjänstkontot med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="92265-107">For example, if a Global Administrator has by mistake reset hello password on hello service account using PowerShell.</span></span>

## <a name="reset-hello-credentials"></a><span data-ttu-id="92265-108">Återställ hello autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="92265-108">Reset hello credentials</span></span>
<span data-ttu-id="92265-109">Om hello tjänstkontot som definierats på hello Azure AD Connector inte kan kontakta Azure AD på grund av problem med tooauthentication, kan hello lösenord återställas.</span><span class="sxs-lookup"><span data-stu-id="92265-109">If hello service account defined on hello Azure AD Connector cannot contact Azure AD due tooauthentication problems, hello password can be reset.</span></span>

1. <span data-ttu-id="92265-110">Logga in toohello Azure AD Connect sync-servern och starta PowerShell.</span><span class="sxs-lookup"><span data-stu-id="92265-110">Sign in toohello Azure AD Connect sync server and start PowerShell.</span></span>
2. <span data-ttu-id="92265-111">Kör `Add-ADSyncAADServiceAccount`.</span><span class="sxs-lookup"><span data-stu-id="92265-111">Run `Add-ADSyncAADServiceAccount`.</span></span>  
   <span data-ttu-id="92265-112">![PowerShell-cmdlet addadsyncaadserviceaccount](./media/active-directory-aadconnectsync-howto-azureadaccount/addadsyncaadserviceaccount.png)</span><span class="sxs-lookup"><span data-stu-id="92265-112">![PowerShell cmdlet addadsyncaadserviceaccount](./media/active-directory-aadconnectsync-howto-azureadaccount/addadsyncaadserviceaccount.png)</span></span>
3. <span data-ttu-id="92265-113">Ange autentiseringsuppgifter för Global administratör för Azure AD.</span><span class="sxs-lookup"><span data-stu-id="92265-113">Provide Azure AD Global admin credentials.</span></span>

<span data-ttu-id="92265-114">Denna cmdlet återställer hello lösenordet för tjänstkontot för hello och uppdatera den i Azure AD och hello Synkroniseringsmotorn.</span><span class="sxs-lookup"><span data-stu-id="92265-114">This cmdlet resets hello password for hello service account and update it both in Azure AD and in hello sync engine.</span></span>

## <a name="known-issues-these-steps-can-solve"></a><span data-ttu-id="92265-115">Kända problem som kan lösa de här stegen</span><span class="sxs-lookup"><span data-stu-id="92265-115">Known issues these steps can solve</span></span>
<span data-ttu-id="92265-116">Det här avsnittet är en lista över felen som rapporteras av kunder som korrigerades med autentiseringsuppgifter för att återställa på hello Azure AD-tjänstkontot.</span><span class="sxs-lookup"><span data-stu-id="92265-116">This section is a list of errors reported by customers that were fixed by a credentials reset on hello Azure AD service account.</span></span>

- - -
<span data-ttu-id="92265-117">Händelsen 6900</span><span class="sxs-lookup"><span data-stu-id="92265-117">Event 6900</span></span>  
<span data-ttu-id="92265-118">hello-servern påträffade ett oväntat fel vid bearbetning av en lösenordsändring:</span><span class="sxs-lookup"><span data-stu-id="92265-118">hello server encountered an unexpected error while processing a password change notification:</span></span>  
<span data-ttu-id="92265-119">AADSTS70002: Fel verifierar referenser.</span><span class="sxs-lookup"><span data-stu-id="92265-119">AADSTS70002: Error validating credentials.</span></span> <span data-ttu-id="92265-120">AADSTS50054: Gammalt lösenord används för autentisering.</span><span class="sxs-lookup"><span data-stu-id="92265-120">AADSTS50054: Old password is used for authentication.</span></span>

- - -
<span data-ttu-id="92265-121">Händelsen 659</span><span class="sxs-lookup"><span data-stu-id="92265-121">Event 659</span></span>  
<span data-ttu-id="92265-122">Fel vid hämtning av lösenord principkonfigurationen synkronisering.</span><span class="sxs-lookup"><span data-stu-id="92265-122">Error while retrieving password policy sync configuration.</span></span> <span data-ttu-id="92265-123">Microsoft.IdentityModel.Clients.ActiveDirectory.AdalServiceException:</span><span class="sxs-lookup"><span data-stu-id="92265-123">Microsoft.IdentityModel.Clients.ActiveDirectory.AdalServiceException:</span></span>  
<span data-ttu-id="92265-124">AADSTS70002: Fel verifierar referenser.</span><span class="sxs-lookup"><span data-stu-id="92265-124">AADSTS70002: Error validating credentials.</span></span> <span data-ttu-id="92265-125">AADSTS50054: Gammalt lösenord används för autentisering.</span><span class="sxs-lookup"><span data-stu-id="92265-125">AADSTS50054: Old password is used for authentication.</span></span>

## <a name="next-steps"></a><span data-ttu-id="92265-126">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="92265-126">Next steps</span></span>
<span data-ttu-id="92265-127">**Översiktsavsnitt**</span><span class="sxs-lookup"><span data-stu-id="92265-127">**Overview topics**</span></span>

* [<span data-ttu-id="92265-128">Azure AD Connect-synkronisering: Förstå och anpassa synkronisering</span><span class="sxs-lookup"><span data-stu-id="92265-128">Azure AD Connect sync: Understand and customize synchronization</span></span>](active-directory-aadconnectsync-whatis.md)
* [<span data-ttu-id="92265-129">Integrera dina lokala identiteter med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="92265-129">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)

