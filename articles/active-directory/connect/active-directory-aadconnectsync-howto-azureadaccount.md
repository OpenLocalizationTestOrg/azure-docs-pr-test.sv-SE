---
title: "Azure AD Connect-synkronisering: hur du hanterar Azure AD-tjänstkontot | Microsoft Docs"
description: "Det här avsnittet beskrivs hur du återställer Azure AD-tjänstkontot."
services: active-directory
keywords: "AADSTS70002, AADSTS50054, hur du återställer lösenordet för Azure AD Connect-synkronisering Connector-tjänstkontot"
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
ms.openlocfilehash: 8e9e8192ee4fcb636b5be91d2616acbc9120c8c0
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="azure-ad-connect-sync-how-to-manage-the-azure-ad-service-account"></a><span data-ttu-id="3b0a1-104">Azure AD Connect-synkronisering: hur du hanterar Azure AD-tjänstkontot</span><span class="sxs-lookup"><span data-stu-id="3b0a1-104">Azure AD Connect sync: How to manage the Azure AD service account</span></span>
<span data-ttu-id="3b0a1-105">Tjänstkontot som används av Azure AD-anslutningen ska vara tjänsten gratis.</span><span class="sxs-lookup"><span data-stu-id="3b0a1-105">The service account used by the Azure AD Connector is supposed to be service free.</span></span> <span data-ttu-id="3b0a1-106">Om du behöver återställa referenserna är det här avsnittet för dig.</span><span class="sxs-lookup"><span data-stu-id="3b0a1-106">If you need to reset its credentials, then this topic is for you.</span></span> <span data-ttu-id="3b0a1-107">Till exempel om en Global administratör har av misstag återställa lösenordet för kontot med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="3b0a1-107">For example, if a Global Administrator has by mistake reset the password on the service account using PowerShell.</span></span>

## <a name="reset-the-credentials"></a><span data-ttu-id="3b0a1-108">Återställa autentiseringsuppgifterna</span><span class="sxs-lookup"><span data-stu-id="3b0a1-108">Reset the credentials</span></span>
<span data-ttu-id="3b0a1-109">Om tjänstkontot som definierats i Azure AD Connector inte kan kontakta Azure AD på grund av autentiseringsproblem med, kan du återställa lösenordet.</span><span class="sxs-lookup"><span data-stu-id="3b0a1-109">If the service account defined on the Azure AD Connector cannot contact Azure AD due to authentication problems, the password can be reset.</span></span>

1. <span data-ttu-id="3b0a1-110">Logga in på Azure AD Connect sync-servern och starta PowerShell.</span><span class="sxs-lookup"><span data-stu-id="3b0a1-110">Sign in to the Azure AD Connect sync server and start PowerShell.</span></span>
2. <span data-ttu-id="3b0a1-111">Kör `Add-ADSyncAADServiceAccount`.</span><span class="sxs-lookup"><span data-stu-id="3b0a1-111">Run `Add-ADSyncAADServiceAccount`.</span></span>  
   <span data-ttu-id="3b0a1-112">![PowerShell-cmdlet addadsyncaadserviceaccount](./media/active-directory-aadconnectsync-howto-azureadaccount/addadsyncaadserviceaccount.png)</span><span class="sxs-lookup"><span data-stu-id="3b0a1-112">![PowerShell cmdlet addadsyncaadserviceaccount](./media/active-directory-aadconnectsync-howto-azureadaccount/addadsyncaadserviceaccount.png)</span></span>
3. <span data-ttu-id="3b0a1-113">Ange autentiseringsuppgifter för Global administratör för Azure AD.</span><span class="sxs-lookup"><span data-stu-id="3b0a1-113">Provide Azure AD Global admin credentials.</span></span>

<span data-ttu-id="3b0a1-114">Denna cmdlet återställer lösenordet för kontot och uppdatera den i Azure AD och Synkroniseringsmotorn.</span><span class="sxs-lookup"><span data-stu-id="3b0a1-114">This cmdlet resets the password for the service account and update it both in Azure AD and in the sync engine.</span></span>

## <a name="known-issues-these-steps-can-solve"></a><span data-ttu-id="3b0a1-115">Kända problem som kan lösa de här stegen</span><span class="sxs-lookup"><span data-stu-id="3b0a1-115">Known issues these steps can solve</span></span>
<span data-ttu-id="3b0a1-116">Det här avsnittet är en lista över felen som rapporteras av kunder som korrigerades med autentiseringsuppgifter för att återställa på Azure AD-tjänstkontot.</span><span class="sxs-lookup"><span data-stu-id="3b0a1-116">This section is a list of errors reported by customers that were fixed by a credentials reset on the Azure AD service account.</span></span>

- - -
<span data-ttu-id="3b0a1-117">Händelsen 6900</span><span class="sxs-lookup"><span data-stu-id="3b0a1-117">Event 6900</span></span>  
<span data-ttu-id="3b0a1-118">Ett oväntat fel uppstod under bearbetning av en lösenordsändring:</span><span class="sxs-lookup"><span data-stu-id="3b0a1-118">The server encountered an unexpected error while processing a password change notification:</span></span>  
<span data-ttu-id="3b0a1-119">AADSTS70002: Fel verifierar referenser.</span><span class="sxs-lookup"><span data-stu-id="3b0a1-119">AADSTS70002: Error validating credentials.</span></span> <span data-ttu-id="3b0a1-120">AADSTS50054: Gammalt lösenord används för autentisering.</span><span class="sxs-lookup"><span data-stu-id="3b0a1-120">AADSTS50054: Old password is used for authentication.</span></span>

- - -
<span data-ttu-id="3b0a1-121">Händelsen 659</span><span class="sxs-lookup"><span data-stu-id="3b0a1-121">Event 659</span></span>  
<span data-ttu-id="3b0a1-122">Fel vid hämtning av lösenord principkonfigurationen synkronisering.</span><span class="sxs-lookup"><span data-stu-id="3b0a1-122">Error while retrieving password policy sync configuration.</span></span> <span data-ttu-id="3b0a1-123">Microsoft.IdentityModel.Clients.ActiveDirectory.AdalServiceException:</span><span class="sxs-lookup"><span data-stu-id="3b0a1-123">Microsoft.IdentityModel.Clients.ActiveDirectory.AdalServiceException:</span></span>  
<span data-ttu-id="3b0a1-124">AADSTS70002: Fel verifierar referenser.</span><span class="sxs-lookup"><span data-stu-id="3b0a1-124">AADSTS70002: Error validating credentials.</span></span> <span data-ttu-id="3b0a1-125">AADSTS50054: Gammalt lösenord används för autentisering.</span><span class="sxs-lookup"><span data-stu-id="3b0a1-125">AADSTS50054: Old password is used for authentication.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3b0a1-126">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3b0a1-126">Next steps</span></span>
<span data-ttu-id="3b0a1-127">**Översiktsavsnitt**</span><span class="sxs-lookup"><span data-stu-id="3b0a1-127">**Overview topics**</span></span>

* [<span data-ttu-id="3b0a1-128">Azure AD Connect-synkronisering: Förstå och anpassa synkronisering</span><span class="sxs-lookup"><span data-stu-id="3b0a1-128">Azure AD Connect sync: Understand and customize synchronization</span></span>](active-directory-aadconnectsync-whatis.md)
* [<span data-ttu-id="3b0a1-129">Integrera dina lokala identiteter med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="3b0a1-129">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)

