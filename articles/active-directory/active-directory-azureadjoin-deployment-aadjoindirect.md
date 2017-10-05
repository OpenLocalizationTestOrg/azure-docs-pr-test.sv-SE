---
title: "Användningsscenarier och överväganden vid distribution för Azure AD Join | Microsoft Docs"
description: "Förklarar hur administratörer kan konfigurera Azure AD Join för slutanvändarna (anställda, studenter, andra användare). Här beskrivs också olika verkliga scenarier för att använda Azure AD Join."
services: active-directory
documentationcenter: 
author: femila
manager: femila
editor: 
tags: azure-classic-portal
ms.assetid: 81d4461e-21c8-4fdd-9076-0e4991979f62
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: markvi
ms.openlocfilehash: fd0aab1a14bbd324e734e5efe8fe101e8a8dfefa
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="usage-scenarios-and-deployment-considerations-for-azure-ad-join"></a><span data-ttu-id="cc407-104">Användningsscenarier och överväganden vid distribution för Azure AD Join</span><span class="sxs-lookup"><span data-stu-id="cc407-104">Usage scenarios and deployment considerations for Azure AD Join</span></span>
## <a name="usage-scenarios-for-azure-ad-join"></a><span data-ttu-id="cc407-105">Användningsscenarier för Azure AD Join</span><span class="sxs-lookup"><span data-stu-id="cc407-105">Usage scenarios for Azure AD Join</span></span>
### <a name="scenario-1-businesses-largely-in-the-cloud"></a><span data-ttu-id="cc407-106">Scenario 1: Företag i stort sett i molnet</span><span class="sxs-lookup"><span data-stu-id="cc407-106">Scenario 1: Businesses largely in the cloud</span></span>
<span data-ttu-id="cc407-107">Azure Active Directory Join (Azure AD Join) du kan ha nytta om du för närvarande fungerar och hantera identiteter för ditt företag i molnet eller flytt till molnet snart.</span><span class="sxs-lookup"><span data-stu-id="cc407-107">Azure Active Directory Join (Azure AD Join) can benefit you if you currently operate and manage identities for your business in the cloud or are moving to the cloud soon.</span></span> <span data-ttu-id="cc407-108">Du kan använda ett konto som du har skapat i Azure AD för att logga in på Windows 10.</span><span class="sxs-lookup"><span data-stu-id="cc407-108">You can use an account that you have created in Azure AD to sign in to Windows 10.</span></span> <span data-ttu-id="cc407-109">Via [första gången du kör uppstå (FRX) processen](active-directory-azureadjoin-user-frx.md), eller genom att ansluta till Azure AD från [inställningsmenyn](active-directory-azureadjoin-user-upgrade.md), kan användarna ansluta sina datorer till Azure AD.</span><span class="sxs-lookup"><span data-stu-id="cc407-109">Through [the first run experience (FRX) process](active-directory-azureadjoin-user-frx.md), or by joining Azure AD from [the settings menu](active-directory-azureadjoin-user-upgrade.md), your users can join their machines to Azure AD.</span></span>  <span data-ttu-id="cc407-110">Användarna kan också få enkel inloggning (SSO) åtkomst till molnresurser som Office 365 i webbläsaren eller i Office-program.</span><span class="sxs-lookup"><span data-stu-id="cc407-110">Your users can also enjoy single sign-on (SSO) access to  cloud resources like Office 365, either in their browsers or in Office applications.</span></span>

### <a name="scenario-2-educational-institutions"></a><span data-ttu-id="cc407-111">Scenario 2: Utbildningssyfte</span><span class="sxs-lookup"><span data-stu-id="cc407-111">Scenario 2: Educational institutions</span></span>
<span data-ttu-id="cc407-112">Utbildningssyfte har vanligtvis två användartyper: lärare och övrig personal och studenter.</span><span class="sxs-lookup"><span data-stu-id="cc407-112">Educational institutions usually have two user types: faculty and students.</span></span> <span data-ttu-id="cc407-113">Fakultetsmedlemmar anses långsiktig medlemmar i organisationen.</span><span class="sxs-lookup"><span data-stu-id="cc407-113">Faculty members are considered longer-term members of the organization.</span></span> <span data-ttu-id="cc407-114">Skapa lokala konton för dem. är önskvärt.</span><span class="sxs-lookup"><span data-stu-id="cc407-114">Creating on-premises accounts for them is desirable.</span></span> <span data-ttu-id="cc407-115">Men studenter shorter-term som ingår i organisationen och sina konton kan hanteras i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="cc407-115">But students are shorter-term members of the organization and  their accounts can be managed in Azure AD.</span></span> <span data-ttu-id="cc407-116">Det innebär att directory skala kan skickas till molnet i stället för att lagras lokalt.</span><span class="sxs-lookup"><span data-stu-id="cc407-116">This means that directory scale can be pushed to the cloud instead of being stored on-premises.</span></span> <span data-ttu-id="cc407-117">Det innebär också att studenter som kommer att kunna logga in till Windows med sina Azure AD-konton och få åtkomst till Office 365-resurser i Office-program.</span><span class="sxs-lookup"><span data-stu-id="cc407-117">It also means that students  will be able to sign in to Windows with their Azure AD accounts and get access to Office 365 resources in Office applications.</span></span>

### <a name="scenario-3-retail-businesses"></a><span data-ttu-id="cc407-118">Scenario 3: Retail företag</span><span class="sxs-lookup"><span data-stu-id="cc407-118">Scenario 3: Retail businesses</span></span>
<span data-ttu-id="cc407-119">Retail företag har säsongsbaserade arbetare och långsiktig anställda.</span><span class="sxs-lookup"><span data-stu-id="cc407-119">Retail businesses have seasonal workers and long-term employees.</span></span> <span data-ttu-id="cc407-120">Du vanligtvis skapa lokala konton och använda domänanslutna datorer för långsiktig heltidsanställda.</span><span class="sxs-lookup"><span data-stu-id="cc407-120">You typically create on-premises accounts and use domain-joined machines for longer-term full-time employees.</span></span> <span data-ttu-id="cc407-121">Men när anställda är shorter-term medlemmar i organisationen och det är klokt att hantera sina konton där användarlicenser kan enkelt flytta runt.</span><span class="sxs-lookup"><span data-stu-id="cc407-121">But seasonal workers are shorter-term members of the organization, and it's desirable to manage their accounts where user licenses can be more easily moved around.</span></span> <span data-ttu-id="cc407-122">När du skapar sina konton i molnet med Office 365 licenser kan få dessa användare fördelarna med att logga in på Windows och Office-program med Azure AD-kontot, samtidigt som du behåller mer flexibelt med licenser när de lämnar.</span><span class="sxs-lookup"><span data-stu-id="cc407-122">When you create their user accounts in the cloud with Office 365 licenses, these users get the benefits of signing in to Windows and Office applications with an Azure AD account, while you maintain more flexibility with their licenses after they leave.</span></span>

### <a name="scenario-4-additional-scenarios"></a><span data-ttu-id="cc407-123">Scenario 4: Fler scenarier</span><span class="sxs-lookup"><span data-stu-id="cc407-123">Scenario 4: Additional scenarios</span></span>
<span data-ttu-id="cc407-124">Tillsammans med de fördelar som tidigare diskuterats, dra nytta av att användarna ansluta sina enheter till Azure AD på grund av en förenklad anslutande upplevelse, effektiv enhetshantering, automatisk hantering av mobilenhetsregistrering och enkel inloggning till Azure AD och lokala resurser.</span><span class="sxs-lookup"><span data-stu-id="cc407-124">Along with the benefits discussed earlier, you  benefit from having your users join their devices to Azure AD because of a simplified joining experience, efficient device management, automatic mobile device management enrollment, and single sign-on to Azure AD and on-premises resources.</span></span>  

## <a name="deployment-considerations-for-azure-ad-join"></a><span data-ttu-id="cc407-125">Överväganden vid distribution för Azure AD Join</span><span class="sxs-lookup"><span data-stu-id="cc407-125">Deployment considerations for Azure AD Join</span></span>
### <a name="enable-your-users-to-join-a-company-owned-device-directly-to-azure-ad"></a><span data-ttu-id="cc407-126">Ge användarna att ansluta till en företagsägd enhet direkt till Azure AD</span><span class="sxs-lookup"><span data-stu-id="cc407-126">Enable your users to join a company-owned device directly to Azure AD</span></span>
<span data-ttu-id="cc407-127">Företag kan tillhandahålla molnbaserad konton till partnerföretag och organisationer.</span><span class="sxs-lookup"><span data-stu-id="cc407-127">Enterprises can provide cloud-only accounts to partner companies and organizations.</span></span> <span data-ttu-id="cc407-128">Dessa partners kan sedan enkelt komma åt företagets appar och resurser med enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="cc407-128">These partners can then easily access company apps and resources with single sign-on.</span></span> <span data-ttu-id="cc407-129">Det här scenariot gäller för användare som har åtkomst till resurser i första hand i molnet, till exempel Office 365 eller SaaS-appar som förlitar sig på Azure AD för autentisering.</span><span class="sxs-lookup"><span data-stu-id="cc407-129">This scenario is applicable to users who access resources primarily in the cloud, such as Office 365 or SaaS apps that rely on Azure AD for authentication.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="cc407-130">Krav</span><span class="sxs-lookup"><span data-stu-id="cc407-130">Prerequisites</span></span>
<span data-ttu-id="cc407-131">**På företagsnivå (administratör)**</span><span class="sxs-lookup"><span data-stu-id="cc407-131">**At the enterprise level (administrator)**</span></span>

* <span data-ttu-id="cc407-132">Azure-prenumeration med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="cc407-132">Azure subscription with Azure Active Directory</span></span>  

<span data-ttu-id="cc407-133">**På användarnivå**</span><span class="sxs-lookup"><span data-stu-id="cc407-133">**At the user level**</span></span>

* <span data-ttu-id="cc407-134">Windows 10 (Professional och Enterprise Edition)</span><span class="sxs-lookup"><span data-stu-id="cc407-134">Windows 10 (Professional and Enterprise editions)</span></span>

### <a name="administrator-tasks"></a><span data-ttu-id="cc407-135">Administratörsåtgärder</span><span class="sxs-lookup"><span data-stu-id="cc407-135">Administrator tasks</span></span>
* [<span data-ttu-id="cc407-136">Konfigurera enhetsregistrering</span><span class="sxs-lookup"><span data-stu-id="cc407-136">Set up device registration</span></span>](active-directory-azureadjoin-setup.md)

### <a name="user-tasks"></a><span data-ttu-id="cc407-137">Användaruppgifter</span><span class="sxs-lookup"><span data-stu-id="cc407-137">User tasks</span></span>
* [<span data-ttu-id="cc407-138">Skapa en ny Windows 10-enhet med Azure AD under installationen</span><span class="sxs-lookup"><span data-stu-id="cc407-138">Set up a new Windows 10 device with Azure AD during setup</span></span>](active-directory-azureadjoin-user-frx.md)
* [<span data-ttu-id="cc407-139">Konfigurera en Windows 10-enhet med Azure AD från inställningsmenyn</span><span class="sxs-lookup"><span data-stu-id="cc407-139">Set up a Windows 10 device with Azure AD from the settings menu</span></span>](active-directory-azureadjoin-user-upgrade.md)
* [<span data-ttu-id="cc407-140">Ansluta en egen Windows 10-enhet till din organisation</span><span class="sxs-lookup"><span data-stu-id="cc407-140">Join a personal Windows 10 device to your organization</span></span>](active-directory-azureadjoin-personal-device.md)

## <a name="enable-byod-in-your-organization-for-windows-10"></a><span data-ttu-id="cc407-141">Aktivera BYOD i din organisation för Windows 10</span><span class="sxs-lookup"><span data-stu-id="cc407-141">Enable BYOD in your organization for Windows 10</span></span>
<span data-ttu-id="cc407-142">Du kan konfigurera användare och anställda använda sina personliga Windows-enheter (BYOD) för att få åtkomst till företagets appar och resurser.</span><span class="sxs-lookup"><span data-stu-id="cc407-142">You can set up your users and employees to use their personal Windows devices (BYOD) to access company apps and resources.</span></span> <span data-ttu-id="cc407-143">Användarna kan lägga till sina Azure AD-konton (arbets- eller skolkonto konton) till en personlig windowsenhet för att komma åt resurser på ett sätt som skyddade och kompatibla.</span><span class="sxs-lookup"><span data-stu-id="cc407-143">Your users can add their Azure AD accounts (work or school accounts) to a personal Windows device to access resources in a secure and compliant fashion.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="cc407-144">Krav</span><span class="sxs-lookup"><span data-stu-id="cc407-144">Prerequisites</span></span>
<span data-ttu-id="cc407-145">**På företagsnivå (administratör)**</span><span class="sxs-lookup"><span data-stu-id="cc407-145">**At the enterprise level (administrator)**</span></span>

* <span data-ttu-id="cc407-146">Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="cc407-146">Azure AD subscription</span></span>

<span data-ttu-id="cc407-147">**På användarnivå**</span><span class="sxs-lookup"><span data-stu-id="cc407-147">**At the user level**</span></span>

* <span data-ttu-id="cc407-148">Windows 10 (Professional och Enterprise Edition)</span><span class="sxs-lookup"><span data-stu-id="cc407-148">Windows 10 (Professional and Enterprise editions)</span></span>

### <a name="administrator-tasks"></a><span data-ttu-id="cc407-149">Administratörsåtgärder</span><span class="sxs-lookup"><span data-stu-id="cc407-149">Administrator tasks</span></span>
* [<span data-ttu-id="cc407-150">Konfigurera enhetsregistrering</span><span class="sxs-lookup"><span data-stu-id="cc407-150">Set up device registration</span></span>](active-directory-azureadjoin-setup.md)

### <a name="user-tasks"></a><span data-ttu-id="cc407-151">Användaruppgifter</span><span class="sxs-lookup"><span data-stu-id="cc407-151">User tasks</span></span>
* [<span data-ttu-id="cc407-152">Ansluta en egen Windows 10-enhet till din organisation</span><span class="sxs-lookup"><span data-stu-id="cc407-152">Join a personal Windows 10 device to your organization</span></span>](active-directory-azureadjoin-personal-device.md)

## <a name="additional-information"></a><span data-ttu-id="cc407-153">Ytterligare information</span><span class="sxs-lookup"><span data-stu-id="cc407-153">Additional information</span></span>
* [<span data-ttu-id="cc407-154">Windows 10 för företaget: Sätt att använda enheter för arbete</span><span class="sxs-lookup"><span data-stu-id="cc407-154">Windows 10 for the enterprise: Ways to use devices for work</span></span>](active-directory-azureadjoin-windows10-devices-overview.md)
* [<span data-ttu-id="cc407-155">Utöka molnkapaciteten till Windows 10-enheter via Azure Active Directory Join</span><span class="sxs-lookup"><span data-stu-id="cc407-155">Extending cloud capabilities to Windows 10 devices through Azure Active Directory Join</span></span>](active-directory-azureadjoin-user-upgrade.md)
* [<span data-ttu-id="cc407-156">Autentisera identiteter utan lösenord via Microsoft Passport</span><span class="sxs-lookup"><span data-stu-id="cc407-156">Authenticating identities without passwords through Microsoft Passport</span></span>](active-directory-azureadjoin-passport.md)
* [<span data-ttu-id="cc407-157">Läs mer om användningsscenarier för Azure AD Join</span><span class="sxs-lookup"><span data-stu-id="cc407-157">Learn about usage scenarios for Azure AD Join</span></span>](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [<span data-ttu-id="cc407-158">Ansluta domänanslutna enheter till miljöer med Azure AD och Windows 10</span><span class="sxs-lookup"><span data-stu-id="cc407-158">Connect domain-joined devices to Azure AD for Windows 10 experiences</span></span>](active-directory-azureadjoin-devices-group-policy.md)
* [<span data-ttu-id="cc407-159">Konfigurera Azure AD Join</span><span class="sxs-lookup"><span data-stu-id="cc407-159">Set up Azure AD Join</span></span>](active-directory-azureadjoin-setup.md)

