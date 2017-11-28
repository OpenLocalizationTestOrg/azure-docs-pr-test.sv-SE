---
title: "aaaTroubleshooting dynamiskt medlemskap för grupper | Microsoft Docs"
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
ms.openlocfilehash: d792fc406288844e2c5dbe3702c2c9870d09473e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-dynamic-memberships-for-groups"></a><span data-ttu-id="59337-103">Felsöka dynamiska medlemskap för grupper</span><span class="sxs-lookup"><span data-stu-id="59337-103">Troubleshooting dynamic memberships for groups</span></span>
<span data-ttu-id="59337-104">**Jag har konfigurerat en regel för en grupp, men inget medlemskap uppdateras i hello grupp**</span><span class="sxs-lookup"><span data-stu-id="59337-104">**I configured a rule on a group but no memberships get updated in hello group**</span></span><br/><span data-ttu-id="59337-105">Kontrollera att hello **aktivera delegerad grupphantering** inställningen för**Ja** i hello **konfigurera** fliken. Den här inställningen visas bara om du är inloggad som en användare toowhom en Azure Active Directory Premium-licens har tilldelats.</span><span class="sxs-lookup"><span data-stu-id="59337-105">Verify that hello **Enable delegated group management** setting is set too**Yes** in hello **Configure** tab. You will see this setting only if you are signed in as a user toowhom an Azure Active Directory Premium license is assigned.</span></span> <span data-ttu-id="59337-106">Kontrollera hello värden för användarattribut på hello regeln: det finns användare som uppfyller hello regeln?</span><span class="sxs-lookup"><span data-stu-id="59337-106">Verify hello values for user attributes on hello rule: are there users that satisfy hello rule?</span></span>

<span data-ttu-id="59337-107">**Jag har konfigurerat en regel, men nu hello befintliga medlemmar i hello regeln tas bort**</span><span class="sxs-lookup"><span data-stu-id="59337-107">**I configured a rule, but now hello existing members of hello rule are removed**</span></span><br/><span data-ttu-id="59337-108">Detta är förväntat.</span><span class="sxs-lookup"><span data-stu-id="59337-108">This is expected behavior.</span></span> <span data-ttu-id="59337-109">Befintliga medlemmarna i hello tas bort när en regel är aktiverad eller har ändrats.</span><span class="sxs-lookup"><span data-stu-id="59337-109">Existing members of hello group are removed when a rule is enabled or changed.</span></span> <span data-ttu-id="59337-110">hello-användare som har returnerats från utvärdering av hello regeln läggs till som medlemmar toohello grupp.</span><span class="sxs-lookup"><span data-stu-id="59337-110">hello users returned from evaluation of hello rule are added as members toohello group.</span></span>     

<span data-ttu-id="59337-111">**Jag ser medlemskap ändras direkt när jag lägger till eller ändra en regel, varför inte?**</span><span class="sxs-lookup"><span data-stu-id="59337-111">**I don’t see membership changes instantly when I add or change a rule, why not?**</span></span><br/><span data-ttu-id="59337-112">Dedikerad medlemskapsutvärdering görs med jämna mellanrum i en asynkron bakgrunden.</span><span class="sxs-lookup"><span data-stu-id="59337-112">Dedicated membership evaluation is done periodically in an asynchronous background process.</span></span> <span data-ttu-id="59337-113">Hur länge hello processen tar bestäms av hello antalet användare i din katalog och hello storleken på hello-gruppen som skapades på grund av hello regeln.</span><span class="sxs-lookup"><span data-stu-id="59337-113">How long hello process takes is determined by hello number of users in your directory and hello size of hello group created as a result of hello rule.</span></span> <span data-ttu-id="59337-114">Normalt ser med mindre antal användare hello ändringar i gruppmedlemskap mindre än några minuter.</span><span class="sxs-lookup"><span data-stu-id="59337-114">Typically, directories with small numbers of users will see hello group membership changes in less than a few minutes.</span></span> <span data-ttu-id="59337-115">Med ett stort antal användare kan ta 30 minuter eller längre toopopulate.</span><span class="sxs-lookup"><span data-stu-id="59337-115">Directories with a large number of users can take 30 minutes or longer toopopulate.</span></span>

### <a name="next-steps"></a><span data-ttu-id="59337-116">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="59337-116">Next steps</span></span>
<span data-ttu-id="59337-117">Dessa artiklar innehåller ytterligare information om Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="59337-117">These articles provide additional information on Azure Active Directory.</span></span>

* [<span data-ttu-id="59337-118">Hantera åtkomst tooresources med Azure Active Directory-grupper</span><span class="sxs-lookup"><span data-stu-id="59337-118">Managing access tooresources with Azure Active Directory groups</span></span>](active-directory-manage-groups.md)
* [<span data-ttu-id="59337-119">Artikelindex för programhantering i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="59337-119">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
* [<span data-ttu-id="59337-120">Vad är Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="59337-120">What is Azure Active Directory?</span></span>](active-directory-whatis.md)
* [<span data-ttu-id="59337-121">Integrera dina lokala identiteter med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="59337-121">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)
