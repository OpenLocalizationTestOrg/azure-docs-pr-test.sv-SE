---
title: "Felsöka dynamiskt medlemskap för grupper | Microsoft Docs"
description: "Felsökningstips för dynamiskt medlemskap för grupper i Azure AD."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 89bb04b6-a379-49c2-8465-fe386641816a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2017
ms.author: curtand
ms.openlocfilehash: 32947e8cc69c9a48d9a285bf0a37ab3398571f86
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshooting-dynamic-memberships-for-groups"></a><span data-ttu-id="ed0d5-103">Felsöka dynamiska medlemskap för grupper</span><span class="sxs-lookup"><span data-stu-id="ed0d5-103">Troubleshooting dynamic memberships for groups</span></span>
<span data-ttu-id="ed0d5-104">**Jag har konfigurerat en regel för en grupp men inget medlemskap uppdateras i gruppen**</span><span class="sxs-lookup"><span data-stu-id="ed0d5-104">**I configured a rule on a group but no memberships get updated in the group**</span></span><br/><span data-ttu-id="ed0d5-105">Kontrollera att den **aktivera delegerad grupphantering** inställningen **Ja** i den **konfigurera** fliken.</span><span class="sxs-lookup"><span data-stu-id="ed0d5-105">Verify that the **Enable delegated group management** setting is set to **Yes** in the **Configure** tab.</span></span> <span data-ttu-id="ed0d5-106">Den här inställningen visas bara om du är inloggad som en användare som tilldelats en Azure Active Directory Premium-licens.</span><span class="sxs-lookup"><span data-stu-id="ed0d5-106">You will see this setting only if you are signed in as a user to whom an Azure Active Directory Premium license is assigned.</span></span> <span data-ttu-id="ed0d5-107">Kontrollera värdena för användarattribut på regeln: det finns användare som uppfyller regeln?</span><span class="sxs-lookup"><span data-stu-id="ed0d5-107">Verify the values for user attributes on the rule: are there users that satisfy the rule?</span></span>

<span data-ttu-id="ed0d5-108">**Jag har konfigurerat en regel, men nu befintliga medlemmar i regeln tas bort**</span><span class="sxs-lookup"><span data-stu-id="ed0d5-108">**I configured a rule, but now the existing members of the rule are removed**</span></span><br/><span data-ttu-id="ed0d5-109">Detta är förväntat.</span><span class="sxs-lookup"><span data-stu-id="ed0d5-109">This is expected behavior.</span></span> <span data-ttu-id="ed0d5-110">Befintliga medlemmar i gruppen tas bort när en regel är aktiverad eller har ändrats.</span><span class="sxs-lookup"><span data-stu-id="ed0d5-110">Existing members of the group are removed when a rule is enabled or changed.</span></span> <span data-ttu-id="ed0d5-111">Användare som har returnerats från tillämpningen av regeln läggs till som medlemmar i gruppen.</span><span class="sxs-lookup"><span data-stu-id="ed0d5-111">The users returned from evaluation of the rule are added as members to the group.</span></span>     

<span data-ttu-id="ed0d5-112">**Jag ser medlemskap ändras direkt när jag lägger till eller ändra en regel, varför inte?**</span><span class="sxs-lookup"><span data-stu-id="ed0d5-112">**I don’t see membership changes instantly when I add or change a rule, why not?**</span></span><br/><span data-ttu-id="ed0d5-113">Dedikerad medlemskapsutvärdering görs med jämna mellanrum i en asynkron bakgrunden.</span><span class="sxs-lookup"><span data-stu-id="ed0d5-113">Dedicated membership evaluation is done periodically in an asynchronous background process.</span></span> <span data-ttu-id="ed0d5-114">Hur lång tid tar processen bestäms av antalet användare i din katalog och storleken på gruppen som skapades på grund av regeln.</span><span class="sxs-lookup"><span data-stu-id="ed0d5-114">How long the process takes is determined by the number of users in your directory and the size of the group created as a result of the rule.</span></span> <span data-ttu-id="ed0d5-115">Normalt ser med mindre antal användare ändringarna av gruppmedlemskap mindre än några minuter.</span><span class="sxs-lookup"><span data-stu-id="ed0d5-115">Typically, directories with small numbers of users will see the group membership changes in less than a few minutes.</span></span> <span data-ttu-id="ed0d5-116">Med ett stort antal användare kan ta 30 minuter eller längre att fylla i.</span><span class="sxs-lookup"><span data-stu-id="ed0d5-116">Directories with a large number of users can take 30 minutes or longer to populate.</span></span>

### <a name="next-steps"></a><span data-ttu-id="ed0d5-117">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ed0d5-117">Next steps</span></span>
<span data-ttu-id="ed0d5-118">Dessa artiklar innehåller ytterligare information om Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="ed0d5-118">These articles provide additional information on Azure Active Directory.</span></span>

* [<span data-ttu-id="ed0d5-119">Hantera åtkomst till resurser med Azure Active Directory-grupper</span><span class="sxs-lookup"><span data-stu-id="ed0d5-119">Managing access to resources with Azure Active Directory groups</span></span>](active-directory-manage-groups.md)
* [<span data-ttu-id="ed0d5-120">Artikelindex för programhantering i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ed0d5-120">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
* [<span data-ttu-id="ed0d5-121">Vad är Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="ed0d5-121">What is Azure Active Directory?</span></span>](active-directory-whatis.md)
* [<span data-ttu-id="ed0d5-122">Integrera dina lokala identiteter med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ed0d5-122">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)
