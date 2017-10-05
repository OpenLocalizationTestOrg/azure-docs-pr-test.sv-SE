---
title: Kontot som etablerar meddelanden | Microsoft Docs
description: "Lär dig mer om att se till att du meddelas om problem som rör användaretablering som kräver din uppmärksamhet genom att aktivera konto etablering meddelanden."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: a637aac7-f06b-48ef-a66d-639835a8edec
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: markvi
ms.openlocfilehash: b99037fc28eca1a3ebffefb9e99991e74f52c9a5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="account-provisioning-notifications"></a><span data-ttu-id="695a7-103">Kontoetableringsmeddelanden</span><span class="sxs-lookup"><span data-stu-id="695a7-103">Account Provisioning Notifications</span></span>
<span data-ttu-id="695a7-104">Med användaretablering, kan du automatisera processen för att hantera användare i SaaS-program från tredje part.</span><span class="sxs-lookup"><span data-stu-id="695a7-104">With user provisioning, you can automate the process of managing users in third party SaaS applications.</span></span> <br>
<span data-ttu-id="695a7-105">Även om detta är en automatiserad process krävs då interaktionen med den här processen.</span><span class="sxs-lookup"><span data-stu-id="695a7-105">While this is an automated process, your interaction with this process is from time to time required.</span></span> <br>
<span data-ttu-id="695a7-106">Detta gäller, till exempel, när lösenordet för det konto som du har konfigurerat för att utbyta data med tredje part SaaS-program har upphört att gälla.</span><span class="sxs-lookup"><span data-stu-id="695a7-106">This is, for example the case, when the password of the account you have configured to exchange data with a third party SaaS application has expired.</span></span> 

<span data-ttu-id="695a7-107">Genom att aktivera konto etablering meddelanden, kan du se till att du meddelas om problem som rör användaretablering som kräver din uppmärksamhet.</span><span class="sxs-lookup"><span data-stu-id="695a7-107">By enabling account provisioning notifications, you can ensure that you are notified of issues related to user provisioning that require your attention.</span></span>

<span data-ttu-id="695a7-108">Du aktiverar eller inaktiverar kontot etablering meddelanden som en del av din konfiguration för tredje part SaaS-program för användaretablering.</span><span class="sxs-lookup"><span data-stu-id="695a7-108">You activate or deactivate account provisioning notifications as part of your user provisioning configuration for a third party SaaS application.</span></span>

![Användaretablering][1] 

<span data-ttu-id="695a7-110">Om du vill aktivera konto för etablering av meddelanden, markerar du kryssrutan relaterade på den **bekräftelse** dialogrutan sidan och skriv sedan e postalias av mottagaren.</span><span class="sxs-lookup"><span data-stu-id="695a7-110">To activate account provisioning notifications, select the related checkbox on the **Confirmation** dialog page, and then type the email alias of the recipient.</span></span>

![Kontoetableringsmeddelanden][2]

<span data-ttu-id="695a7-112">Du kan ange en distributionslista som mottagaren. Det är dock viktigt att notera att e-postmeddelandet innehåller länkar till rapporter som endast är tillgängliga för Azure AD-administratörer.</span><span class="sxs-lookup"><span data-stu-id="695a7-112">You can enter a distribution list as recipient; however, it is important to note that the notification email contains links to reports that are only accessible by the Azure AD administrators.</span></span>

<span data-ttu-id="695a7-113">Om du har konto etablering meddelanden aktiverad, får e-postmeddelanden om kritiska problem som är relaterade till användaretablering.</span><span class="sxs-lookup"><span data-stu-id="695a7-113">If you have account provisioning notifications enabled, you will receive emails about critical issues that are related to user provisioning.</span></span> <span data-ttu-id="695a7-114">Men för att undvika en e-överlagring kan får endast du ett e-postmeddelande per dag för varje SaaS-program som e-postmeddelandet har aktiverats för.</span><span class="sxs-lookup"><span data-stu-id="695a7-114">However, to avoid an email overload, you will only receive one notification email per day for each SaaS application the notification email is enabled for.</span></span>

## <a name="related-articles"></a><span data-ttu-id="695a7-115">Relaterade artiklar</span><span class="sxs-lookup"><span data-stu-id="695a7-115">Related Articles</span></span>
* [<span data-ttu-id="695a7-116">Artikelindex för programhantering i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="695a7-116">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
* [<span data-ttu-id="695a7-117">Automatisera användaren etablering/avetablering för SaaS-appar</span><span class="sxs-lookup"><span data-stu-id="695a7-117">Automate User Provisioning/Deprovisioning to SaaS Apps</span></span>](active-directory-saas-app-provisioning.md)
* [<span data-ttu-id="695a7-118">Anpassa attributmappning för Användaretablering</span><span class="sxs-lookup"><span data-stu-id="695a7-118">Customizing Attribute Mappings for User Provisioning</span></span>](active-directory-saas-customizing-attribute-mappings.md)
* [<span data-ttu-id="695a7-119">Skriva uttryck för attributmappning</span><span class="sxs-lookup"><span data-stu-id="695a7-119">Writing Expressions for Attribute Mappings</span></span>](active-directory-saas-writing-expressions-for-attribute-mappings.md)
* [<span data-ttu-id="695a7-120">Omfångsfilter för Användaretablering</span><span class="sxs-lookup"><span data-stu-id="695a7-120">Scoping Filters for User Provisioning</span></span>](active-directory-saas-scoping-filters.md)
* [<span data-ttu-id="695a7-121">Använda SCIM för att aktivera automatisk etablering av användare och grupper från Azure Active Directory till program</span><span class="sxs-lookup"><span data-stu-id="695a7-121">Using SCIM to enable automatic provisioning of users and groups from Azure Active Directory to applications</span></span>](active-directory-scim-provisioning.md)
* [<span data-ttu-id="695a7-122">Lista över självstudier om hur du integrerar SaaS-appar</span><span class="sxs-lookup"><span data-stu-id="695a7-122">List of Tutorials on How to Integrate SaaS Apps</span></span>](active-directory-saas-tutorial-list.md)

<!--Image references-->
[1]: ./media/active-directory-saas-account-provisioning-notifications/ic766307.png
[2]: ./media/active-directory-saas-account-provisioning-notifications/ic766308.png
