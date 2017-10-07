---
title: "aaaUsage scenarier och överväganden vid distribution för Azure AD Join | Microsoft Docs"
description: "Förklarar hur administratörer kan konfigurera Azure AD Join för slutanvändarna (anställda, studenter, andra användare). Här beskrivs också hello olika verkliga scenarier för att använda Azure AD Join."
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
ms.openlocfilehash: 7e57971481aa312ebf8a69999d194f9dcc3d4708
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="usage-scenarios-and-deployment-considerations-for-azure-ad-join"></a><span data-ttu-id="bcf5d-104">Användningsscenarier och överväganden vid distribution för Azure AD Join</span><span class="sxs-lookup"><span data-stu-id="bcf5d-104">Usage scenarios and deployment considerations for Azure AD Join</span></span>
## <a name="usage-scenarios-for-azure-ad-join"></a><span data-ttu-id="bcf5d-105">Användningsscenarier för Azure AD Join</span><span class="sxs-lookup"><span data-stu-id="bcf5d-105">Usage scenarios for Azure AD Join</span></span>
### <a name="scenario-1-businesses-largely-in-hello-cloud"></a><span data-ttu-id="bcf5d-106">Scenario 1: Företag i stort sett i hello moln</span><span class="sxs-lookup"><span data-stu-id="bcf5d-106">Scenario 1: Businesses largely in hello cloud</span></span>
<span data-ttu-id="bcf5d-107">Azure Active Directory Join (Azure AD Join) du kan ha nytta om du för närvarande fungerar och hantera identiteter för ditt företag i molnet hello eller flyttar toohello moln snart.</span><span class="sxs-lookup"><span data-stu-id="bcf5d-107">Azure Active Directory Join (Azure AD Join) can benefit you if you currently operate and manage identities for your business in hello cloud or are moving toohello cloud soon.</span></span> <span data-ttu-id="bcf5d-108">Du kan använda ett konto som du har skapat i Azure AD toosign i tooWindows 10.</span><span class="sxs-lookup"><span data-stu-id="bcf5d-108">You can use an account that you have created in Azure AD toosign in tooWindows 10.</span></span> <span data-ttu-id="bcf5d-109">Via [hello först köra experience (FRX) processen](active-directory-azureadjoin-user-frx.md), eller genom att ansluta till Azure AD från [hello inställningsmenyn](active-directory-azureadjoin-user-upgrade.md), dina användare kan ansluta till sina datorer tooAzure AD.</span><span class="sxs-lookup"><span data-stu-id="bcf5d-109">Through [hello first run experience (FRX) process](active-directory-azureadjoin-user-frx.md), or by joining Azure AD from [hello settings menu](active-directory-azureadjoin-user-upgrade.md), your users can join their machines tooAzure AD.</span></span>  <span data-ttu-id="bcf5d-110">Användarna kan också få enkel inloggning (SSO) åtkomst för molnresurser som Office 365 i webbläsaren eller i Office-program.</span><span class="sxs-lookup"><span data-stu-id="bcf5d-110">Your users can also enjoy single sign-on (SSO) access too cloud resources like Office 365, either in their browsers or in Office applications.</span></span>

### <a name="scenario-2-educational-institutions"></a><span data-ttu-id="bcf5d-111">Scenario 2: Utbildningssyfte</span><span class="sxs-lookup"><span data-stu-id="bcf5d-111">Scenario 2: Educational institutions</span></span>
<span data-ttu-id="bcf5d-112">Utbildningssyfte har vanligtvis två användartyper: lärare och övrig personal och studenter.</span><span class="sxs-lookup"><span data-stu-id="bcf5d-112">Educational institutions usually have two user types: faculty and students.</span></span> <span data-ttu-id="bcf5d-113">Fakultetsmedlemmar anses långsiktig medlemmar i hello organisation.</span><span class="sxs-lookup"><span data-stu-id="bcf5d-113">Faculty members are considered longer-term members of hello organization.</span></span> <span data-ttu-id="bcf5d-114">Skapa lokala konton för dem. är önskvärt.</span><span class="sxs-lookup"><span data-stu-id="bcf5d-114">Creating on-premises accounts for them is desirable.</span></span> <span data-ttu-id="bcf5d-115">Men studenter tillhör shorter-term hello organisation och sina konton kan hanteras i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="bcf5d-115">But students are shorter-term members of hello organization and  their accounts can be managed in Azure AD.</span></span> <span data-ttu-id="bcf5d-116">Det innebär att directory skala flyttas toohello moln i stället för att lagras lokalt.</span><span class="sxs-lookup"><span data-stu-id="bcf5d-116">This means that directory scale can be pushed toohello cloud instead of being stored on-premises.</span></span> <span data-ttu-id="bcf5d-117">Det innebär också att studenter som kommer att kunna toosign i tooWindows med sina Azure AD-konton och få åtkomst till tooOffice 365 resurser i Office-program.</span><span class="sxs-lookup"><span data-stu-id="bcf5d-117">It also means that students  will be able toosign in tooWindows with their Azure AD accounts and get access tooOffice 365 resources in Office applications.</span></span>

### <a name="scenario-3-retail-businesses"></a><span data-ttu-id="bcf5d-118">Scenario 3: Retail företag</span><span class="sxs-lookup"><span data-stu-id="bcf5d-118">Scenario 3: Retail businesses</span></span>
<span data-ttu-id="bcf5d-119">Retail företag har säsongsbaserade arbetare och långsiktig anställda.</span><span class="sxs-lookup"><span data-stu-id="bcf5d-119">Retail businesses have seasonal workers and long-term employees.</span></span> <span data-ttu-id="bcf5d-120">Du vanligtvis skapa lokala konton och använda domänanslutna datorer för långsiktig heltidsanställda.</span><span class="sxs-lookup"><span data-stu-id="bcf5d-120">You typically create on-premises accounts and use domain-joined machines for longer-term full-time employees.</span></span> <span data-ttu-id="bcf5d-121">Men när anställda tillhör shorter-term hello organisation, och det är önskvärt toomanage sina konton där användarlicenser kan enkelt flytta runt.</span><span class="sxs-lookup"><span data-stu-id="bcf5d-121">But seasonal workers are shorter-term members of hello organization, and it's desirable toomanage their accounts where user licenses can be more easily moved around.</span></span> <span data-ttu-id="bcf5d-122">När du skapar sina konton i hello moln med Office 365 licenser kan få dessa användare hello fördelarna med att logga in tooWindows och Office-program med Azure AD-kontot, samtidigt som du behåller mer flexibelt med licenser när de lämnar.</span><span class="sxs-lookup"><span data-stu-id="bcf5d-122">When you create their user accounts in hello cloud with Office 365 licenses, these users get hello benefits of signing in tooWindows and Office applications with an Azure AD account, while you maintain more flexibility with their licenses after they leave.</span></span>

### <a name="scenario-4-additional-scenarios"></a><span data-ttu-id="bcf5d-123">Scenario 4: Fler scenarier</span><span class="sxs-lookup"><span data-stu-id="bcf5d-123">Scenario 4: Additional scenarios</span></span>
<span data-ttu-id="bcf5d-124">Tillsammans med hello fördelar som tidigare diskuterats, dra nytta av att användarna ansluta sina enheter tooAzure AD på grund av en förenklad anslutande upplevelse, effektiv enhetshantering, automatisk hantering av mobilenhetsregistrering och tooAzure för enkel inloggning AD och lokala resurser.</span><span class="sxs-lookup"><span data-stu-id="bcf5d-124">Along with hello benefits discussed earlier, you  benefit from having your users join their devices tooAzure AD because of a simplified joining experience, efficient device management, automatic mobile device management enrollment, and single sign-on tooAzure AD and on-premises resources.</span></span>  

## <a name="deployment-considerations-for-azure-ad-join"></a><span data-ttu-id="bcf5d-125">Överväganden vid distribution för Azure AD Join</span><span class="sxs-lookup"><span data-stu-id="bcf5d-125">Deployment considerations for Azure AD Join</span></span>
### <a name="enable-your-users-toojoin-a-company-owned-device-directly-tooazure-ad"></a><span data-ttu-id="bcf5d-126">Aktivera din användare toojoin en företagsägd enhet direkt tooAzure AD</span><span class="sxs-lookup"><span data-stu-id="bcf5d-126">Enable your users toojoin a company-owned device directly tooAzure AD</span></span>
<span data-ttu-id="bcf5d-127">Företag kan tillhandahålla molnbaserad konton toopartner företag och organisationer.</span><span class="sxs-lookup"><span data-stu-id="bcf5d-127">Enterprises can provide cloud-only accounts toopartner companies and organizations.</span></span> <span data-ttu-id="bcf5d-128">Dessa partners kan sedan enkelt komma åt företagets appar och resurser med enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="bcf5d-128">These partners can then easily access company apps and resources with single sign-on.</span></span> <span data-ttu-id="bcf5d-129">Det här scenariot är tillämpliga toousers som har åtkomst till resurser i första hand i hello molnet, till exempel Office 365 eller SaaS-appar som förlitar sig på Azure AD för autentisering.</span><span class="sxs-lookup"><span data-stu-id="bcf5d-129">This scenario is applicable toousers who access resources primarily in hello cloud, such as Office 365 or SaaS apps that rely on Azure AD for authentication.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="bcf5d-130">Krav</span><span class="sxs-lookup"><span data-stu-id="bcf5d-130">Prerequisites</span></span>
<span data-ttu-id="bcf5d-131">**På företagsnivå hello (administratör)**</span><span class="sxs-lookup"><span data-stu-id="bcf5d-131">**At hello enterprise level (administrator)**</span></span>

* <span data-ttu-id="bcf5d-132">Azure-prenumeration med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="bcf5d-132">Azure subscription with Azure Active Directory</span></span>  

<span data-ttu-id="bcf5d-133">**På användarnivå hello**</span><span class="sxs-lookup"><span data-stu-id="bcf5d-133">**At hello user level**</span></span>

* <span data-ttu-id="bcf5d-134">Windows 10 (Professional och Enterprise Edition)</span><span class="sxs-lookup"><span data-stu-id="bcf5d-134">Windows 10 (Professional and Enterprise editions)</span></span>

### <a name="administrator-tasks"></a><span data-ttu-id="bcf5d-135">Administratörsåtgärder</span><span class="sxs-lookup"><span data-stu-id="bcf5d-135">Administrator tasks</span></span>
* [<span data-ttu-id="bcf5d-136">Konfigurera enhetsregistrering</span><span class="sxs-lookup"><span data-stu-id="bcf5d-136">Set up device registration</span></span>](active-directory-azureadjoin-setup.md)

### <a name="user-tasks"></a><span data-ttu-id="bcf5d-137">Användaruppgifter</span><span class="sxs-lookup"><span data-stu-id="bcf5d-137">User tasks</span></span>
* [<span data-ttu-id="bcf5d-138">Skapa en ny Windows 10-enhet med Azure AD under installationen</span><span class="sxs-lookup"><span data-stu-id="bcf5d-138">Set up a new Windows 10 device with Azure AD during setup</span></span>](active-directory-azureadjoin-user-frx.md)
* [<span data-ttu-id="bcf5d-139">Konfigurera en Windows 10-enhet med Azure AD hello inställningsmenyn</span><span class="sxs-lookup"><span data-stu-id="bcf5d-139">Set up a Windows 10 device with Azure AD from hello settings menu</span></span>](active-directory-azureadjoin-user-upgrade.md)
* [<span data-ttu-id="bcf5d-140">Ansluta till en personlig tooyour organisation för Windows 10-enhet</span><span class="sxs-lookup"><span data-stu-id="bcf5d-140">Join a personal Windows 10 device tooyour organization</span></span>](active-directory-azureadjoin-personal-device.md)

## <a name="enable-byod-in-your-organization-for-windows-10"></a><span data-ttu-id="bcf5d-141">Aktivera BYOD i din organisation för Windows 10</span><span class="sxs-lookup"><span data-stu-id="bcf5d-141">Enable BYOD in your organization for Windows 10</span></span>
<span data-ttu-id="bcf5d-142">Du kan ställa in din användare och anställda toouse sina personliga Windows-enheter (BYOD) tooaccess företagets appar och resurser.</span><span class="sxs-lookup"><span data-stu-id="bcf5d-142">You can set up your users and employees toouse their personal Windows devices (BYOD) tooaccess company apps and resources.</span></span> <span data-ttu-id="bcf5d-143">Användarna kan lägga till sina Azure AD-konton (arbets- eller skolkonto konton) tooa personliga Windows tooaccess resurser på ett sätt som skyddade och kompatibla.</span><span class="sxs-lookup"><span data-stu-id="bcf5d-143">Your users can add their Azure AD accounts (work or school accounts) tooa personal Windows device tooaccess resources in a secure and compliant fashion.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="bcf5d-144">Krav</span><span class="sxs-lookup"><span data-stu-id="bcf5d-144">Prerequisites</span></span>
<span data-ttu-id="bcf5d-145">**På företagsnivå hello (administratör)**</span><span class="sxs-lookup"><span data-stu-id="bcf5d-145">**At hello enterprise level (administrator)**</span></span>

* <span data-ttu-id="bcf5d-146">Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="bcf5d-146">Azure AD subscription</span></span>

<span data-ttu-id="bcf5d-147">**På användarnivå hello**</span><span class="sxs-lookup"><span data-stu-id="bcf5d-147">**At hello user level**</span></span>

* <span data-ttu-id="bcf5d-148">Windows 10 (Professional och Enterprise Edition)</span><span class="sxs-lookup"><span data-stu-id="bcf5d-148">Windows 10 (Professional and Enterprise editions)</span></span>

### <a name="administrator-tasks"></a><span data-ttu-id="bcf5d-149">Administratörsåtgärder</span><span class="sxs-lookup"><span data-stu-id="bcf5d-149">Administrator tasks</span></span>
* [<span data-ttu-id="bcf5d-150">Konfigurera enhetsregistrering</span><span class="sxs-lookup"><span data-stu-id="bcf5d-150">Set up device registration</span></span>](active-directory-azureadjoin-setup.md)

### <a name="user-tasks"></a><span data-ttu-id="bcf5d-151">Användaruppgifter</span><span class="sxs-lookup"><span data-stu-id="bcf5d-151">User tasks</span></span>
* [<span data-ttu-id="bcf5d-152">Ansluta till en personlig tooyour organisation för Windows 10-enhet</span><span class="sxs-lookup"><span data-stu-id="bcf5d-152">Join a personal Windows 10 device tooyour organization</span></span>](active-directory-azureadjoin-personal-device.md)

## <a name="additional-information"></a><span data-ttu-id="bcf5d-153">Ytterligare information</span><span class="sxs-lookup"><span data-stu-id="bcf5d-153">Additional information</span></span>
* [<span data-ttu-id="bcf5d-154">Windows 10 för hello företaget: sätt toouse enheter för arbete</span><span class="sxs-lookup"><span data-stu-id="bcf5d-154">Windows 10 for hello enterprise: Ways toouse devices for work</span></span>](active-directory-azureadjoin-windows10-devices-overview.md)
* [<span data-ttu-id="bcf5d-155">Utöka molnet funktioner tooWindows 10-enheter via Azure Active Directory Join</span><span class="sxs-lookup"><span data-stu-id="bcf5d-155">Extending cloud capabilities tooWindows 10 devices through Azure Active Directory Join</span></span>](active-directory-azureadjoin-user-upgrade.md)
* [<span data-ttu-id="bcf5d-156">Autentisera identiteter utan lösenord via Microsoft Passport</span><span class="sxs-lookup"><span data-stu-id="bcf5d-156">Authenticating identities without passwords through Microsoft Passport</span></span>](active-directory-azureadjoin-passport.md)
* [<span data-ttu-id="bcf5d-157">Läs mer om användningsscenarier för Azure AD Join</span><span class="sxs-lookup"><span data-stu-id="bcf5d-157">Learn about usage scenarios for Azure AD Join</span></span>](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [<span data-ttu-id="bcf5d-158">Ansluta domänanslutna enheter tooAzure AD för Windows 10-upplevelser</span><span class="sxs-lookup"><span data-stu-id="bcf5d-158">Connect domain-joined devices tooAzure AD for Windows 10 experiences</span></span>](active-directory-azureadjoin-devices-group-policy.md)
* [<span data-ttu-id="bcf5d-159">Konfigurera Azure AD Join</span><span class="sxs-lookup"><span data-stu-id="bcf5d-159">Set up Azure AD Join</span></span>](active-directory-azureadjoin-setup.md)

