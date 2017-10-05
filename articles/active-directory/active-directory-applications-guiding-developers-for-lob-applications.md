---
title: "Utveckla appar för Azure AD | Microsoft Docs"
description: "Den här artikeln innehåller skrivs för IT-proffs riktlinjer för integrera Azure-program med Active Directory."
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
ms.openlocfilehash: 6b119be9c06d8c1ccc8e747168429e6c2d2e7a8f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="develop-line-of-business-apps-for-azure-active-directory"></a><span data-ttu-id="8d581-103">Utveckla av branschspecifika appar för Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="8d581-103">Develop line-of-business apps for Azure Active Directory</span></span>
<span data-ttu-id="8d581-104">Den här guiden innehåller en översikt över utvecklar line-of-business (LoB)-program för Azure Active Directory (AD). Målgruppen är globala administratörer för Active Directory/Office 365.</span><span class="sxs-lookup"><span data-stu-id="8d581-104">This guide provides an overview of developing line-of-business (LoB) applications for Azure Active Directory (AD).The intended audience is Active Directory/Office 365 global administrators.</span></span>

## <a name="overview"></a><span data-ttu-id="8d581-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="8d581-105">Overview</span></span>
<span data-ttu-id="8d581-106">Skapa program som är integrerade med Azure AD ger användare i din organisation enkel inloggning med Office 365.</span><span class="sxs-lookup"><span data-stu-id="8d581-106">Building applications integrated with Azure AD gives users in your organization single sign-on with Office 365.</span></span> <span data-ttu-id="8d581-107">Med programmet i Azure AD-ger dig kontroll över autentiseringsprincipen för programmet.</span><span class="sxs-lookup"><span data-stu-id="8d581-107">Having the application in Azure AD gives you control over the authentication policy for the application.</span></span> <span data-ttu-id="8d581-108">Mer information om hur villkorlig åtkomst och skydda appar med multifaktorautentisering (MFA) Se [konfigurera åtkomstregler](active-directory-conditional-access-azuread-connected-apps.md).</span><span class="sxs-lookup"><span data-stu-id="8d581-108">To learn more about conditional access and how to protect apps with multi-factor authentication (MFA) see [Configuring access rules](active-directory-conditional-access-azuread-connected-apps.md).</span></span>

<span data-ttu-id="8d581-109">Registrera ditt program att använda Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="8d581-109">Register your application to use Azure Active Directory.</span></span> <span data-ttu-id="8d581-110">Registrera programmet innebär att utvecklarna kan använda Azure AD för att autentisera användare och begära åtkomst till resurser som e-post, kalender och dokument för användare.</span><span class="sxs-lookup"><span data-stu-id="8d581-110">Registering the application means that your developers can use Azure AD to authenticate users and request access to user resources such as email, calendar, and documents.</span></span>

<span data-ttu-id="8d581-111">Alla medlemmar i din katalog (inte gäster) kan registrera ett program, även kallat *skapar ett programobjekt*.</span><span class="sxs-lookup"><span data-stu-id="8d581-111">Any member of your directory (not guests) can register an application, otherwise known as *creating an application object*.</span></span>

<span data-ttu-id="8d581-112">Registrera ett program kan alla användare att göra följande:</span><span class="sxs-lookup"><span data-stu-id="8d581-112">Registering an application allows any user to do the following:</span></span>

* <span data-ttu-id="8d581-113">Hämta en identitet för de program som kan identifieras av Azure AD</span><span class="sxs-lookup"><span data-stu-id="8d581-113">Get an identity for their application that Azure AD recognizes</span></span>
* <span data-ttu-id="8d581-114">Hämta en eller flera hemligheter/nycklar som programmet kan använda för att autentisera sig till AD</span><span class="sxs-lookup"><span data-stu-id="8d581-114">Get one or more secrets/keys that the application can use to authenticate itself to AD</span></span>
* <span data-ttu-id="8d581-115">Lägga till programmet i Azure-portalen med ett anpassat namn, logotyp och så vidare.</span><span class="sxs-lookup"><span data-stu-id="8d581-115">Brand the application in the Azure portal with a custom name, logo, etc.</span></span>
* <span data-ttu-id="8d581-116">Gäller Azure AD-funktioner för auktorisering för appen, inklusive:</span><span class="sxs-lookup"><span data-stu-id="8d581-116">Apply Azure AD authorization features to their app, including:</span></span>

  * <span data-ttu-id="8d581-117">Rollbaserad åtkomstkontroll (RBAC)</span><span class="sxs-lookup"><span data-stu-id="8d581-117">Role-Based Access Control (RBAC)</span></span>
  * <span data-ttu-id="8d581-118">Azure Active Directory som server för oAuth-auktorisering (skydda en API som exponeras av programmet)</span><span class="sxs-lookup"><span data-stu-id="8d581-118">Azure Active Directory as oAuth authorization server (secure an API exposed by the application)</span></span>
* <span data-ttu-id="8d581-119">Deklarera behörigheter som krävs för nödvändiga för att programmet ska fungera som förväntat, inklusive:</span><span class="sxs-lookup"><span data-stu-id="8d581-119">Declare required permissions necessary for the application to function as expected, including:</span></span>

      - <span data-ttu-id="8d581-120">Appbehörigheter (endast globala administratörer).</span><span class="sxs-lookup"><span data-stu-id="8d581-120">App permissions (global administrators only).</span></span> <span data-ttu-id="8d581-121">Till exempel: medlemskap i en annan Azure AD-program eller roll medlemskap i förhållande till en Azure resurs, resursgrupp eller prenumeration</span><span class="sxs-lookup"><span data-stu-id="8d581-121">For example: Role membership in another Azure AD application or role membership relative to an Azure Resource, Resource Group, or Subscription</span></span>
      - <span data-ttu-id="8d581-122">Delegerad behörighet (alla användare).</span><span class="sxs-lookup"><span data-stu-id="8d581-122">Delegated permissions (any user).</span></span> <span data-ttu-id="8d581-123">Till exempel: Azure AD-inloggning och Läs profil</span><span class="sxs-lookup"><span data-stu-id="8d581-123">For example: Azure AD, Sign-in, and Read Profile</span></span>

> [!NOTE]
> <span data-ttu-id="8d581-124">Som standard kan alla medlemmar registrera ett program.</span><span class="sxs-lookup"><span data-stu-id="8d581-124">By default, any member can register an application.</span></span> <span data-ttu-id="8d581-125">Information om hur du begränsar behörigheter för att registrera program till specifika medlemmar finns [hur program läggs till Azure AD](develop/active-directory-how-applications-are-added.md#who-has-permission-to-add-applications-to-my-azure-ad-instance).</span><span class="sxs-lookup"><span data-stu-id="8d581-125">To learn how to restrict permissions for registering applications to specific members, see [How applications are added to Azure AD](develop/active-directory-how-applications-are-added.md#who-has-permission-to-add-applications-to-my-azure-ad-instance).</span></span>
>
>

<span data-ttu-id="8d581-126">Här är vad du den globala administratören behöver för att hjälpa utvecklare att förbereda sina program för produktion:</span><span class="sxs-lookup"><span data-stu-id="8d581-126">Here’s what you, the global administrator, need to do to help developers make their application ready for production:</span></span>

* <span data-ttu-id="8d581-127">Konfigurera åtkomstregler (åtkomst princip/MFA)</span><span class="sxs-lookup"><span data-stu-id="8d581-127">Configure access rules (access policy/MFA)</span></span>
* <span data-ttu-id="8d581-128">Konfigurera appen för att kräva Användartilldelning och tilldela användare</span><span class="sxs-lookup"><span data-stu-id="8d581-128">Configure the app to require user assignment and assign users</span></span>
* <span data-ttu-id="8d581-129">Ignorera standardgränssnittet för godkännande</span><span class="sxs-lookup"><span data-stu-id="8d581-129">Suppress the default user consent experience</span></span>

## <a name="configure-access-rules"></a><span data-ttu-id="8d581-130">Konfigurera åtkomstregler</span><span class="sxs-lookup"><span data-stu-id="8d581-130">Configure access rules</span></span>
<span data-ttu-id="8d581-131">Konfigurera regler för programspecifika åtkomst till SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="8d581-131">Configure per-application access rules to your SaaS apps.</span></span> <span data-ttu-id="8d581-132">Du kan till exempel kräva MFA eller bara tillåta åtkomst till användare i betrodda nätverk.</span><span class="sxs-lookup"><span data-stu-id="8d581-132">For example, you can require MFA or only allow access to users on trusted networks.</span></span> <span data-ttu-id="8d581-133">Information om detta finns i dokumentet [konfigurera åtkomstregler](active-directory-conditional-access-azuread-connected-apps.md).</span><span class="sxs-lookup"><span data-stu-id="8d581-133">The details for this are available in the document [Configuring access rules](active-directory-conditional-access-azuread-connected-apps.md).</span></span>

## <a name="configure-the-app-to-require-user-assignment-and-assign-users"></a><span data-ttu-id="8d581-134">Konfigurera appen för att kräva Användartilldelning och tilldela användare</span><span class="sxs-lookup"><span data-stu-id="8d581-134">Configure the app to require user assignment and assign users</span></span>
<span data-ttu-id="8d581-135">Som standard kan användare komma åt program utan att ha tilldelats.</span><span class="sxs-lookup"><span data-stu-id="8d581-135">By default, users can access applications without being assigned.</span></span> <span data-ttu-id="8d581-136">Om programmet visar roller eller om du vill att programmet ska visas på åtkomstpanelen för en användare, bör du kräva Användartilldelning.</span><span class="sxs-lookup"><span data-stu-id="8d581-136">However, if the application exposes roles or if you want the application to appear on a user’s access panel, you should require user assignment.</span></span>

[<span data-ttu-id="8d581-137">Kräv användartilldelning</span><span class="sxs-lookup"><span data-stu-id="8d581-137">Requiring user assignment</span></span>](active-directory-applications-guiding-developers-requiring-user-assignment.md)

<span data-ttu-id="8d581-138">Om du är en Azure AD Premium eller Enterprise Mobility Suite (EMS) prenumerant, rekommenderar vi starkt med hjälp av grupper.</span><span class="sxs-lookup"><span data-stu-id="8d581-138">If you’re an Azure AD Premium or Enterprise Mobility Suite (EMS) subscriber, we strongly recommend using groups.</span></span> <span data-ttu-id="8d581-139">Tilldela grupper till programmet kan du delegera hanteringen av pågående åtkomst till ägare av gruppen.</span><span class="sxs-lookup"><span data-stu-id="8d581-139">Assigning groups to the application allows you to delegate ongoing access management to the owner of the group.</span></span> <span data-ttu-id="8d581-140">Du kan skapa gruppen eller be ansvarig part i din organisation att skapa gruppen med din grupp management anläggning.</span><span class="sxs-lookup"><span data-stu-id="8d581-140">You can create the group or ask the responsible party in your organization to create the group using your group management facility.</span></span>

[<span data-ttu-id="8d581-141">Tilldela användare till ett program</span><span class="sxs-lookup"><span data-stu-id="8d581-141">Assigning users to an application</span></span>](active-directory-applications-guiding-developers-assigning-users.md)  
[<span data-ttu-id="8d581-142">Tilldela grupper till ett program</span><span class="sxs-lookup"><span data-stu-id="8d581-142">Assigning groups to an application</span></span>](active-directory-applications-guiding-developers-assigning-groups.md)

## <a name="suppress-user-consent"></a><span data-ttu-id="8d581-143">Ignorera tillstånd</span><span class="sxs-lookup"><span data-stu-id="8d581-143">Suppress user consent</span></span>
<span data-ttu-id="8d581-144">Varje användare genomgår en medgivande upplevelse att logga in som standard.</span><span class="sxs-lookup"><span data-stu-id="8d581-144">By default, each user goes through a consent experience to sign in.</span></span> <span data-ttu-id="8d581-145">Medgivande upplevelse, genom att be användarna att tilldela behörigheter till ett program kan vara förvirrande för användare som inte är bekant med att fatta detta beslut.</span><span class="sxs-lookup"><span data-stu-id="8d581-145">The consent experience, asking users to grant permissions to an application, can be disconcerting for users who are unfamiliar with making such decisions.</span></span>

<span data-ttu-id="8d581-146">För program som du litar på, kan du förenkla användarupplevelsen av principer för program uppdrag av din organisation.</span><span class="sxs-lookup"><span data-stu-id="8d581-146">For applications that you trust, you can simplify the user experience by consenting to the application on behalf of your organization.</span></span>

<span data-ttu-id="8d581-147">Mer information om tillstånd och samtycke uppstår i Azure finns [integrera program med Azure Active Directory](active-directory-integrating-applications.md).</span><span class="sxs-lookup"><span data-stu-id="8d581-147">For more information about user consent and the consent experience in Azure, see [Integrating Applications with Azure Active Directory](active-directory-integrating-applications.md).</span></span>

## <a name="related-articles"></a><span data-ttu-id="8d581-148">Relaterade artiklar</span><span class="sxs-lookup"><span data-stu-id="8d581-148">Related Articles</span></span>
* [<span data-ttu-id="8d581-149">Aktivera säker fjärråtkomst till lokala program med Azure AD Application Proxy</span><span class="sxs-lookup"><span data-stu-id="8d581-149">Enable secure remote access to on-premises applications with Azure AD Application Proxy</span></span>](active-directory-application-proxy-get-started.md)
* [<span data-ttu-id="8d581-150">Förhandsgranska Azure villkorlig åtkomst för SaaS-appar</span><span class="sxs-lookup"><span data-stu-id="8d581-150">Azure Conditional Access Preview for SaaS Apps</span></span>](active-directory-conditional-access-azuread-connected-apps.md)
* [<span data-ttu-id="8d581-151">Hantera åtkomst till appar med Azure AD</span><span class="sxs-lookup"><span data-stu-id="8d581-151">Managing access to apps with Azure AD</span></span>](active-directory-managing-access-to-apps.md)
* [<span data-ttu-id="8d581-152">Artikelindex för programhantering i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="8d581-152">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
