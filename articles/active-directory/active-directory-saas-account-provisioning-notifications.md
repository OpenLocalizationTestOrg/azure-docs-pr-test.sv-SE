---
title: aaaAccount etablering meddelanden | Microsoft Docs
description: "Lär dig hur tooensure att meddelas du om problem relaterade toouser etablering som kräver din uppmärksamhet genom att aktivera etablering av meddelanden."
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
ms.openlocfilehash: e33d1dd806fff43fc96f843a9dcddd7375d2e3c4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="account-provisioning-notifications"></a><span data-ttu-id="3fb3f-103">Kontoetableringsmeddelanden</span><span class="sxs-lookup"><span data-stu-id="3fb3f-103">Account Provisioning Notifications</span></span>
<span data-ttu-id="3fb3f-104">Med användaretablering, kan du automatisera hello process för att hantera användare i SaaS-program från tredje part.</span><span class="sxs-lookup"><span data-stu-id="3fb3f-104">With user provisioning, you can automate hello process of managing users in third party SaaS applications.</span></span> <br>
<span data-ttu-id="3fb3f-105">Även om detta är en automatiserad process är interaktionen med den här processen från tid tootime krävs.</span><span class="sxs-lookup"><span data-stu-id="3fb3f-105">While this is an automated process, your interaction with this process is from time tootime required.</span></span> <br>
<span data-ttu-id="3fb3f-106">Detta gäller, till exempel hello, när hello lösenord för hello-konto som du har konfigurerat tooexchange data med tredje part SaaS-program har upphört att gälla.</span><span class="sxs-lookup"><span data-stu-id="3fb3f-106">This is, for example hello case, when hello password of hello account you have configured tooexchange data with a third party SaaS application has expired.</span></span> 

<span data-ttu-id="3fb3f-107">Genom att aktivera konto etablering meddelanden kan du se till att du meddelas om problem relaterade toouser etablering som kräver din uppmärksamhet.</span><span class="sxs-lookup"><span data-stu-id="3fb3f-107">By enabling account provisioning notifications, you can ensure that you are notified of issues related toouser provisioning that require your attention.</span></span>

<span data-ttu-id="3fb3f-108">Du aktiverar eller inaktiverar kontot etablering meddelanden som en del av din konfiguration för tredje part SaaS-program för användaretablering.</span><span class="sxs-lookup"><span data-stu-id="3fb3f-108">You activate or deactivate account provisioning notifications as part of your user provisioning configuration for a third party SaaS application.</span></span>

![Användaretablering][1] 

<span data-ttu-id="3fb3f-110">tooactivate konto etablering meddelanden, Välj hello relaterade kryssruta på hello **bekräftelse** dialogrutan sidan och typen hello e-alias för hello mottagaren.</span><span class="sxs-lookup"><span data-stu-id="3fb3f-110">tooactivate account provisioning notifications, select hello related checkbox on hello **Confirmation** dialog page, and then type hello email alias of hello recipient.</span></span>

![Kontoetableringsmeddelanden][2]

<span data-ttu-id="3fb3f-112">Du kan ange en distributionslista som mottagaren. Det är dock viktigt toonote att hello e-postmeddelandet innehåller länkar tooreports som endast är tillgängliga för administratörer hello Azure AD.</span><span class="sxs-lookup"><span data-stu-id="3fb3f-112">You can enter a distribution list as recipient; however, it is important toonote that hello notification email contains links tooreports that are only accessible by hello Azure AD administrators.</span></span>

<span data-ttu-id="3fb3f-113">Om du har konto etablering meddelanden aktiverad, får e-postmeddelanden om kritiska problem som är relaterade toouser etablering.</span><span class="sxs-lookup"><span data-stu-id="3fb3f-113">If you have account provisioning notifications enabled, you will receive emails about critical issues that are related toouser provisioning.</span></span> <span data-ttu-id="3fb3f-114">Dock tooavoid en e-överlagring endast får du ett e-postmeddelande per dag för varje SaaS programmet hello e-postmeddelandet har aktiverats för.</span><span class="sxs-lookup"><span data-stu-id="3fb3f-114">However, tooavoid an email overload, you will only receive one notification email per day for each SaaS application hello notification email is enabled for.</span></span>

## <a name="related-articles"></a><span data-ttu-id="3fb3f-115">Relaterade artiklar</span><span class="sxs-lookup"><span data-stu-id="3fb3f-115">Related Articles</span></span>
* [<span data-ttu-id="3fb3f-116">Artikelindex för programhantering i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="3fb3f-116">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
* [<span data-ttu-id="3fb3f-117">Automatisera användaren etablering/avetablering tooSaaS appar</span><span class="sxs-lookup"><span data-stu-id="3fb3f-117">Automate User Provisioning/Deprovisioning tooSaaS Apps</span></span>](active-directory-saas-app-provisioning.md)
* [<span data-ttu-id="3fb3f-118">Anpassa attributmappning för Användaretablering</span><span class="sxs-lookup"><span data-stu-id="3fb3f-118">Customizing Attribute Mappings for User Provisioning</span></span>](active-directory-saas-customizing-attribute-mappings.md)
* [<span data-ttu-id="3fb3f-119">Skriva uttryck för attributmappning</span><span class="sxs-lookup"><span data-stu-id="3fb3f-119">Writing Expressions for Attribute Mappings</span></span>](active-directory-saas-writing-expressions-for-attribute-mappings.md)
* [<span data-ttu-id="3fb3f-120">Omfångsfilter för Användaretablering</span><span class="sxs-lookup"><span data-stu-id="3fb3f-120">Scoping Filters for User Provisioning</span></span>](active-directory-saas-scoping-filters.md)
* [<span data-ttu-id="3fb3f-121">Använda SCIM tooenable Automatisk etablering av användare och grupper från Azure Active Directory tooapplications</span><span class="sxs-lookup"><span data-stu-id="3fb3f-121">Using SCIM tooenable automatic provisioning of users and groups from Azure Active Directory tooapplications</span></span>](active-directory-scim-provisioning.md)
* [<span data-ttu-id="3fb3f-122">Lista över självstudier om hur tooIntegrate SaaS-appar</span><span class="sxs-lookup"><span data-stu-id="3fb3f-122">List of Tutorials on How tooIntegrate SaaS Apps</span></span>](active-directory-saas-tutorial-list.md)

<!--Image references-->
[1]: ./media/active-directory-saas-account-provisioning-notifications/ic766307.png
[2]: ./media/active-directory-saas-account-provisioning-notifications/ic766308.png
