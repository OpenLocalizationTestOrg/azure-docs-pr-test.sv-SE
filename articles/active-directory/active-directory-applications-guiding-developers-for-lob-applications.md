---
title: "aaaDevelop appar för Azure AD | Microsoft Docs"
description: "Den här artikeln innehåller riktlinjer för integrera Azure-program med Active Directory skrivna för hello IT-proffs."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
editor: 
ms.assetid: dd69f2bc-37c5-457c-857d-27acb84267fb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2017
ms.author: kgremban
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d2924be752af0be2843b1d9b74d9ec446d3fe1ea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="develop-line-of-business-apps-for-azure-active-directory"></a><span data-ttu-id="f3d3a-103">Utveckla av branschspecifika appar för Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="f3d3a-103">Develop line-of-business apps for Azure Active Directory</span></span>
<span data-ttu-id="f3d3a-104">Den här guiden ger en översikt över utvecklar line-of-business (LoB)-program för Azure Active Directory (AD) .hello avsedd målgruppen är globala administratörer för Active Directory/Office 365.</span><span class="sxs-lookup"><span data-stu-id="f3d3a-104">This guide provides an overview of developing line-of-business (LoB) applications for Azure Active Directory (AD).hello intended audience is Active Directory/Office 365 global administrators.</span></span>

## <a name="overview"></a><span data-ttu-id="f3d3a-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="f3d3a-105">Overview</span></span>
<span data-ttu-id="f3d3a-106">Skapa program som är integrerade med Azure AD ger användare i din organisation enkel inloggning med Office 365.</span><span class="sxs-lookup"><span data-stu-id="f3d3a-106">Building applications integrated with Azure AD gives users in your organization single sign-on with Office 365.</span></span> <span data-ttu-id="f3d3a-107">Med hello program i Azure AD-ger dig kontroll över hello autentiseringsprincip för hello program.</span><span class="sxs-lookup"><span data-stu-id="f3d3a-107">Having hello application in Azure AD gives you control over hello authentication policy for hello application.</span></span> <span data-ttu-id="f3d3a-108">Mer om villkorsstyrd åtkomst och hur tooprotect appar med multifaktorautentisering (MFA) Se toolearn [konfigurera åtkomstregler](active-directory-conditional-access-azuread-connected-apps.md).</span><span class="sxs-lookup"><span data-stu-id="f3d3a-108">toolearn more about conditional access and how tooprotect apps with multi-factor authentication (MFA) see [Configuring access rules](active-directory-conditional-access-azuread-connected-apps.md).</span></span>

<span data-ttu-id="f3d3a-109">Registrera ditt program toouse Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="f3d3a-109">Register your application toouse Azure Active Directory.</span></span> <span data-ttu-id="f3d3a-110">Registrera hello program innebär att utvecklarna kan använda Azure AD-användare tooauthenticate och begär åtkomst toouser resurser, till exempel e-post, kalender och dokument.</span><span class="sxs-lookup"><span data-stu-id="f3d3a-110">Registering hello application means that your developers can use Azure AD tooauthenticate users and request access toouser resources such as email, calendar, and documents.</span></span>

<span data-ttu-id="f3d3a-111">Alla medlemmar i din katalog (inte gäster) kan registrera ett program, även kallat *skapar ett programobjekt*.</span><span class="sxs-lookup"><span data-stu-id="f3d3a-111">Any member of your directory (not guests) can register an application, otherwise known as *creating an application object*.</span></span>

<span data-ttu-id="f3d3a-112">Registrera ett program kan alla användare toodo hello följande:</span><span class="sxs-lookup"><span data-stu-id="f3d3a-112">Registering an application allows any user toodo hello following:</span></span>

* <span data-ttu-id="f3d3a-113">Hämta en identitet för de program som kan identifieras av Azure AD</span><span class="sxs-lookup"><span data-stu-id="f3d3a-113">Get an identity for their application that Azure AD recognizes</span></span>
* <span data-ttu-id="f3d3a-114">Skaffa en eller flera hemligheter/nycklar som hello program kan du använda tooauthenticate själva tooAD</span><span class="sxs-lookup"><span data-stu-id="f3d3a-114">Get one or more secrets/keys that hello application can use tooauthenticate itself tooAD</span></span>
* <span data-ttu-id="f3d3a-115">Varumärken hello programmet i hello Azure-portalen med ett anpassat namn, logotyp och så vidare.</span><span class="sxs-lookup"><span data-stu-id="f3d3a-115">Brand hello application in hello Azure portal with a custom name, logo, etc.</span></span>
* <span data-ttu-id="f3d3a-116">Använda Azure AD auktorisering funktioner tootheir appen, inklusive:</span><span class="sxs-lookup"><span data-stu-id="f3d3a-116">Apply Azure AD authorization features tootheir app, including:</span></span>

  * <span data-ttu-id="f3d3a-117">Rollbaserad åtkomstkontroll (RBAC)</span><span class="sxs-lookup"><span data-stu-id="f3d3a-117">Role-Based Access Control (RBAC)</span></span>
  * <span data-ttu-id="f3d3a-118">Azure Active Directory som server för oAuth-auktorisering (skydda en API som exponeras av programmet hello)</span><span class="sxs-lookup"><span data-stu-id="f3d3a-118">Azure Active Directory as oAuth authorization server (secure an API exposed by hello application)</span></span>
* <span data-ttu-id="f3d3a-119">Deklarera behörighet krävs för hello programmet toofunction som förväntat, inklusive:</span><span class="sxs-lookup"><span data-stu-id="f3d3a-119">Declare required permissions necessary for hello application toofunction as expected, including:</span></span>

      - <span data-ttu-id="f3d3a-120">Appbehörigheter (endast globala administratörer).</span><span class="sxs-lookup"><span data-stu-id="f3d3a-120">App permissions (global administrators only).</span></span> <span data-ttu-id="f3d3a-121">Till exempel: medlemskap i en annan Azure AD-program eller roll medlemskap relativa tooan Azure resurs, resursgrupp, eller en prenumeration</span><span class="sxs-lookup"><span data-stu-id="f3d3a-121">For example: Role membership in another Azure AD application or role membership relative tooan Azure Resource, Resource Group, or Subscription</span></span>
      - <span data-ttu-id="f3d3a-122">Delegerad behörighet (alla användare).</span><span class="sxs-lookup"><span data-stu-id="f3d3a-122">Delegated permissions (any user).</span></span> <span data-ttu-id="f3d3a-123">Till exempel: Azure AD-inloggning och Läs profil</span><span class="sxs-lookup"><span data-stu-id="f3d3a-123">For example: Azure AD, Sign-in, and Read Profile</span></span>

> [!NOTE]
> <span data-ttu-id="f3d3a-124">Som standard kan alla medlemmar registrera ett program.</span><span class="sxs-lookup"><span data-stu-id="f3d3a-124">By default, any member can register an application.</span></span> <span data-ttu-id="f3d3a-125">hur toorestrict behörigheter för att registrera program toospecific medlemmar, se toolearn [hur program läggs tooAzure AD](develop/active-directory-how-applications-are-added.md#who-has-permission-to-add-applications-to-my-azure-ad-instance).</span><span class="sxs-lookup"><span data-stu-id="f3d3a-125">toolearn how toorestrict permissions for registering applications toospecific members, see [How applications are added tooAzure AD](develop/active-directory-how-applications-are-added.md#who-has-permission-to-add-applications-to-my-azure-ad-instance).</span></span>
>
>

<span data-ttu-id="f3d3a-126">Här är vad du, hello global administratör, måste toodo toohelp utvecklare förbereda sina program för produktion:</span><span class="sxs-lookup"><span data-stu-id="f3d3a-126">Here’s what you, hello global administrator, need toodo toohelp developers make their application ready for production:</span></span>

* <span data-ttu-id="f3d3a-127">Konfigurera åtkomstregler (åtkomst princip/MFA)</span><span class="sxs-lookup"><span data-stu-id="f3d3a-127">Configure access rules (access policy/MFA)</span></span>
* <span data-ttu-id="f3d3a-128">Konfigurera hello app toorequire Användartilldelning och tilldela användare</span><span class="sxs-lookup"><span data-stu-id="f3d3a-128">Configure hello app toorequire user assignment and assign users</span></span>
* <span data-ttu-id="f3d3a-129">Ignorera hello medgivande standardgränssnittet</span><span class="sxs-lookup"><span data-stu-id="f3d3a-129">Suppress hello default user consent experience</span></span>

## <a name="configure-access-rules"></a><span data-ttu-id="f3d3a-130">Konfigurera åtkomstregler</span><span class="sxs-lookup"><span data-stu-id="f3d3a-130">Configure access rules</span></span>
<span data-ttu-id="f3d3a-131">Konfigurera programspecifika regler tooyour SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="f3d3a-131">Configure per-application access rules tooyour SaaS apps.</span></span> <span data-ttu-id="f3d3a-132">Du kan till exempel kräva MFA eller bara tillåta åtkomst toousers på betrodda nätverk.</span><span class="sxs-lookup"><span data-stu-id="f3d3a-132">For example, you can require MFA or only allow access toousers on trusted networks.</span></span> <span data-ttu-id="f3d3a-133">hello information om detta finns i hello dokument [konfigurera åtkomstregler](active-directory-conditional-access-azuread-connected-apps.md).</span><span class="sxs-lookup"><span data-stu-id="f3d3a-133">hello details for this are available in hello document [Configuring access rules](active-directory-conditional-access-azuread-connected-apps.md).</span></span>

## <a name="configure-hello-app-toorequire-user-assignment-and-assign-users"></a><span data-ttu-id="f3d3a-134">Konfigurera hello app toorequire Användartilldelning och tilldela användare</span><span class="sxs-lookup"><span data-stu-id="f3d3a-134">Configure hello app toorequire user assignment and assign users</span></span>
<span data-ttu-id="f3d3a-135">Som standard kan användare komma åt program utan att ha tilldelats.</span><span class="sxs-lookup"><span data-stu-id="f3d3a-135">By default, users can access applications without being assigned.</span></span> <span data-ttu-id="f3d3a-136">Om programmet hello exponerar roller eller om du vill hello programmet tooappear på åtkomstpanelen för en användare, bör du kräva Användartilldelning.</span><span class="sxs-lookup"><span data-stu-id="f3d3a-136">However, if hello application exposes roles or if you want hello application tooappear on a user’s access panel, you should require user assignment.</span></span>

[<span data-ttu-id="f3d3a-137">Kräv användartilldelning</span><span class="sxs-lookup"><span data-stu-id="f3d3a-137">Requiring user assignment</span></span>](active-directory-applications-guiding-developers-requiring-user-assignment.md)

<span data-ttu-id="f3d3a-138">Om du är en Azure AD Premium eller Enterprise Mobility Suite (EMS) prenumerant, rekommenderar vi starkt med hjälp av grupper.</span><span class="sxs-lookup"><span data-stu-id="f3d3a-138">If you’re an Azure AD Premium or Enterprise Mobility Suite (EMS) subscriber, we strongly recommend using groups.</span></span> <span data-ttu-id="f3d3a-139">Tilldela grupper toohello program kan du toodelegate pågående access management toohello ägare hello gruppen.</span><span class="sxs-lookup"><span data-stu-id="f3d3a-139">Assigning groups toohello application allows you toodelegate ongoing access management toohello owner of hello group.</span></span> <span data-ttu-id="f3d3a-140">Du kan skapa hello grupp eller be hello ansvarig part i din organisation toocreate hello-gruppen med din grupp management anläggning.</span><span class="sxs-lookup"><span data-stu-id="f3d3a-140">You can create hello group or ask hello responsible party in your organization toocreate hello group using your group management facility.</span></span>

[<span data-ttu-id="f3d3a-141">Tilldela användare tooan program</span><span class="sxs-lookup"><span data-stu-id="f3d3a-141">Assigning users tooan application</span></span>](active-directory-applications-guiding-developers-assigning-users.md)  
[<span data-ttu-id="f3d3a-142">Tilldela grupper tooan program</span><span class="sxs-lookup"><span data-stu-id="f3d3a-142">Assigning groups tooan application</span></span>](active-directory-applications-guiding-developers-assigning-groups.md)

## <a name="suppress-user-consent"></a><span data-ttu-id="f3d3a-143">Ignorera tillstånd</span><span class="sxs-lookup"><span data-stu-id="f3d3a-143">Suppress user consent</span></span>
<span data-ttu-id="f3d3a-144">Som standard genomgår varje användare en medgivande upplevelse toosign i.</span><span class="sxs-lookup"><span data-stu-id="f3d3a-144">By default, each user goes through a consent experience toosign in.</span></span> <span data-ttu-id="f3d3a-145">hello kan medgivande, och ber användarna toogrant behörigheter tooan programmet vara förvirrande för användare som inte är bekant med att fatta detta beslut.</span><span class="sxs-lookup"><span data-stu-id="f3d3a-145">hello consent experience, asking users toogrant permissions tooan application, can be disconcerting for users who are unfamiliar with making such decisions.</span></span>

<span data-ttu-id="f3d3a-146">För program som du litar på, kan du förenkla hello användarupplevelse av principer toohello program uppdrag av din organisation.</span><span class="sxs-lookup"><span data-stu-id="f3d3a-146">For applications that you trust, you can simplify hello user experience by consenting toohello application on behalf of your organization.</span></span>

<span data-ttu-id="f3d3a-147">Mer information om tillstånd och hello samtycke upplevelse i Azure, se [integrera program med Azure Active Directory](active-directory-integrating-applications.md).</span><span class="sxs-lookup"><span data-stu-id="f3d3a-147">For more information about user consent and hello consent experience in Azure, see [Integrating Applications with Azure Active Directory](active-directory-integrating-applications.md).</span></span>

## <a name="related-articles"></a><span data-ttu-id="f3d3a-148">Relaterade artiklar</span><span class="sxs-lookup"><span data-stu-id="f3d3a-148">Related Articles</span></span>
* [<span data-ttu-id="f3d3a-149">Aktivera säker fjärråtkomst tooon lokala program med Azure AD Application Proxy</span><span class="sxs-lookup"><span data-stu-id="f3d3a-149">Enable secure remote access tooon-premises applications with Azure AD Application Proxy</span></span>](active-directory-application-proxy-get-started.md)
* [<span data-ttu-id="f3d3a-150">Förhandsgranska Azure villkorlig åtkomst för SaaS-appar</span><span class="sxs-lookup"><span data-stu-id="f3d3a-150">Azure Conditional Access Preview for SaaS Apps</span></span>](active-directory-conditional-access-azuread-connected-apps.md)
* [<span data-ttu-id="f3d3a-151">Hantera åtkomst tooapps med Azure AD</span><span class="sxs-lookup"><span data-stu-id="f3d3a-151">Managing access tooapps with Azure AD</span></span>](active-directory-managing-access-to-apps.md)
* [<span data-ttu-id="f3d3a-152">Artikelindex för programhantering i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="f3d3a-152">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
