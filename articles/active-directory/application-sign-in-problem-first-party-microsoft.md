---
title: Problem med att logga till en Microsoft-program | Microsoft Docs
description: "Felsöka vanliga problem med inför när de loggar in på från första part Microsoft Applications med Azure AD (till exempel Office 365)"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 5638434270ee82d2b9737ea8eed8b5a8c62f7121
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
## <a name="problems-signing-in-to-a-microsoft-application"></a><span data-ttu-id="633e4-103">Problem med att logga till en Microsoft-program</span><span class="sxs-lookup"><span data-stu-id="633e4-103">Problems signing in to a Microsoft application</span></span>

<span data-ttu-id="633e4-104">Microsoft Applications (till exempel Office 365 Exchange, SharePoint, Yammer, etc.) tilldelas och hanteras annorlunda än 3 part SaaS-program eller andra program som du har integrerat med Azure AD för enkel inloggning på.</span><span class="sxs-lookup"><span data-stu-id="633e4-104">Microsoft Applications (like Office 365 Exchange, SharePoint, Yammer, etc.) are assigned and managed a bit differently than 3rd party SaaS applications or other applications you integrate with Azure AD for single sign on.</span></span>

<span data-ttu-id="633e4-105">Det finns tre sätt att en användare kan få åtkomst till ett Microsoft-publicerade program.</span><span class="sxs-lookup"><span data-stu-id="633e4-105">There are three main ways that a user can get access to a Microsoft-published application.</span></span>

-   <span data-ttu-id="633e4-106">För program i Office 365 eller andra betald paket användare som beviljas åtkomst via **licens tilldelning** antingen direkt till användarkontot eller via en grupp med vår gruppbaserade licens tilldelning-funktionen.</span><span class="sxs-lookup"><span data-stu-id="633e4-106">For applications in the Office 365 or other paid suites, users are granted access through **license assignment** either directly to their user account, or through a group using our group-based license assignment capability.</span></span>

-   <span data-ttu-id="633e4-107">För program som Microsoft eller tredje part publicerar fritt för alla använda användare beviljas åtkomst via **användargodkännande**.</span><span class="sxs-lookup"><span data-stu-id="633e4-107">For applications that Microsoft or a Third Party publishes freely for anyone to use, users may be granted access through **user consent**.</span></span> <span data-ttu-id="633e4-108">This0 innebär att de loggar in på programmet med ett konto med Azure AD-arbetsplatsen eller skolan och tillåta att den har åtkomst till vissa begränsad uppsättning data på sitt konto.</span><span class="sxs-lookup"><span data-stu-id="633e4-108">This0 means that they sign in to the application with their Azure AD Work or School account and allow it to have access to some limited set of data on their account.</span></span>

-   <span data-ttu-id="633e4-109">För program som Microsoft eller 3 part publicerar fritt för alla använda användare även beviljas åtkomst via **administratör medgivande**.</span><span class="sxs-lookup"><span data-stu-id="633e4-109">For applications that Microsoft or a 3rd Party publishes freely for anyone to use, users may also be granted access through **administrator consent**.</span></span> <span data-ttu-id="633e4-110">Det innebär att en administratör har fastställt programmet kan användas av alla i organisationen, så att de loggar in på programmet med ett globalt administratörskonto och bevilja åtkomst till alla i organisationen.</span><span class="sxs-lookup"><span data-stu-id="633e4-110">This means that an administrator has determined the application may be used by everyone in the organization, so they sign in to the application with a Global Administrator account and grant access to everyone in the organization.</span></span>

<span data-ttu-id="633e4-111">Om du vill felsöka problemet, börja med den [allmänna problemområden med programåtkomst att tänka på](#general-problem-areas-with-application-access-to-consider) och Läs den [genomgång: steg för att felsöka Microsoft Application åtkomst](#walkthrough-steps-to-troubleshoot-microsoft-application-access) att komma till den information.</span><span class="sxs-lookup"><span data-stu-id="633e4-111">To troubleshoot your issue, start with the [General Problem Areas with Application Access to consider](#general-problem-areas-with-application-access-to-consider) and then read the [Walkthrough: Steps to troubleshoot Microsoft Application access](#walkthrough-steps-to-troubleshoot-microsoft-application-access) to get into the details.</span></span>

## <a name="general-problem-areas-with-application-access-to-consider"></a><span data-ttu-id="633e4-112">Allmänna problemområden med programåtkomst att tänka på</span><span class="sxs-lookup"><span data-stu-id="633e4-112">General Problem Areas with Application Access to consider</span></span>

<span data-ttu-id="633e4-113">Nedan visas en lista över allmänna problemområden som du kan detaljerat om du har en uppfattning om var du startar, men vi rekommenderar att du läser den här genomgången för att komma igång snabbt: [genomgång: steg för att felsöka Microsoft Application åtkomst](#walkthrough-steps-to-troubleshoot-microsoft-application-access).</span><span class="sxs-lookup"><span data-stu-id="633e4-113">Below is a list of the general problem areas that you can drill into if you have an idea of where to start, but we recommend you read the walkthrough to get going quickly: [Walkthrough: Steps to troubleshoot Microsoft Application access](#walkthrough-steps-to-troubleshoot-microsoft-application-access).</span></span>

-   [<span data-ttu-id="633e4-114">Problem med användarkontot</span><span class="sxs-lookup"><span data-stu-id="633e4-114">Problems with the user’s account</span></span>](#problems-with-the-users-account)

-   [<span data-ttu-id="633e4-115">Problem med grupper</span><span class="sxs-lookup"><span data-stu-id="633e4-115">Problems with groups</span></span>](#problems-with-groups)

-   [<span data-ttu-id="633e4-116">Problem med principer för villkorlig åtkomst</span><span class="sxs-lookup"><span data-stu-id="633e4-116">Problems with conditional access policies</span></span>](#problems-with-conditional-access-policies)

-   [<span data-ttu-id="633e4-117">Problem med programmet medgivande</span><span class="sxs-lookup"><span data-stu-id="633e4-117">Problems with application consent</span></span>](#problems-with-application-consent)

## <a name="steps-to-troubleshoot-microsoft-application-access"></a><span data-ttu-id="633e4-118">Steg för att felsöka Microsoft Application åtkomst</span><span class="sxs-lookup"><span data-stu-id="633e4-118">Steps to troubleshoot Microsoft Application access</span></span>

<span data-ttu-id="633e4-119">Nedan är några vanliga problem avdelningen stöter på när användarna inte logga in i ett Microsoft-program.</span><span class="sxs-lookup"><span data-stu-id="633e4-119">Below are some common issues folks run into when their users cannot sign in to a Microsoft application.</span></span>

-   <span data-ttu-id="633e4-120">Allmänna problem med att kontrollera först</span><span class="sxs-lookup"><span data-stu-id="633e4-120">General issues to check first</span></span>

  * <span data-ttu-id="633e4-121">Kontrollera att användaren loggar in på den **korrigera URL** och inte en lokal programmets URL.</span><span class="sxs-lookup"><span data-stu-id="633e4-121">Make sure the user is signing in to the **correct URL** and not a local application URL.</span></span>

  * <span data-ttu-id="633e4-122">Kontrollera att användarkontot är **inte låst.**</span><span class="sxs-lookup"><span data-stu-id="633e4-122">Make sure the user’s account is **not locked out.**</span></span>

  * <span data-ttu-id="633e4-123">Kontrollera att den **användarkontot finns** i Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="633e4-123">Make sure the **user’s account exists** in Azure Active Directory.</span></span> [<span data-ttu-id="633e4-124">Kontrollera om det finns ett konto i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="633e4-124">Check if a user account exists in Azure Active Directory</span></span>](#problems-with-the-users-account)

  * <span data-ttu-id="633e4-125">Kontrollera att användarkontot är **aktiverat** för inloggningar.</span><span class="sxs-lookup"><span data-stu-id="633e4-125">Make sure the user’s account is **enabled** for sign ins.</span></span> [<span data-ttu-id="633e4-126">Kontrollera status för en användare</span><span class="sxs-lookup"><span data-stu-id="633e4-126">Check a user’s account status</span></span>](#problems-with-the-users-account)

  * <span data-ttu-id="633e4-127">Kontrollera att användarens **lösenord inte har upphört att gälla eller har glömt.**</span><span class="sxs-lookup"><span data-stu-id="633e4-127">Make sure the user’s **password is not expired or forgotten.**</span></span> <span data-ttu-id="633e4-128">[Återställa en användares lösenord](#reset-a-users-password) eller [aktivera lösenordsåterställning via självbetjäning](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started)</span><span class="sxs-lookup"><span data-stu-id="633e4-128">[Reset a user’s password](#reset-a-users-password) or [Enable self-service password reset](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started)</span></span>

   * <span data-ttu-id="633e4-129">Kontrollera att **Multifaktorautentisering** inte blockerar åtkomst.</span><span class="sxs-lookup"><span data-stu-id="633e4-129">Make sure **Multi-Factor Authentication** is not blocking user access.</span></span> <span data-ttu-id="633e4-130">[Kontrollera status för en användares multifaktorautentisering](#check-a-users-multi-factor-authentication-status) eller [Kontrollera en användares kontaktinformation för autentisering](#check-a-users-authentication-contact-info)</span><span class="sxs-lookup"><span data-stu-id="633e4-130">[Check a user’s multi-factor authentication status](#check-a-users-multi-factor-authentication-status) or [Check a user’s authentication contact info](#check-a-users-authentication-contact-info)</span></span>

   * <span data-ttu-id="633e4-131">Kontrollera att en **principen för villkorlig åtkomst** eller **identitetsskydd** principen inte blockerar åtkomst.</span><span class="sxs-lookup"><span data-stu-id="633e4-131">Make sure a **Conditional Access policy** or **Identity Protection** policy is not blocking user access.</span></span> <span data-ttu-id="633e4-132">[Kontrollera en viss villkorlig åtkomstprincip](#problems-with-conditional-access-policies) eller [Kontrollera principen för villkorlig åtkomst för ett visst program](#check-a-specific-applications-conditional-access-policy) eller [inaktivera en princip för specifik villkorlig åtkomst](#disable-a-specific-conditional-access-policy)</span><span class="sxs-lookup"><span data-stu-id="633e4-132">[Check a specific conditional access policy](#problems-with-conditional-access-policies) or [Check a specific application’s conditional access policy](#check-a-specific-applications-conditional-access-policy) or [Disable a specific conditional access policy](#disable-a-specific-conditional-access-policy)</span></span>

   * <span data-ttu-id="633e4-133">Se till att en användares **autentisering kontaktinformation** är uppdaterade så att Multi-Factor Authentication eller villkorlig åtkomst principer som ska framtvingas.</span><span class="sxs-lookup"><span data-stu-id="633e4-133">Make sure that a user’s **authentication contact info** is up to date to allow Multi-Factor Authentication or Conditional Access policies to be enforced.</span></span> <span data-ttu-id="633e4-134">[Kontrollera status för en användares multifaktorautentisering](#check-a-users-multi-factor-authentication-status) eller [Kontrollera en användares kontaktinformation för autentisering](#check-a-users-authentication-contact-info)</span><span class="sxs-lookup"><span data-stu-id="633e4-134">[Check a user’s multi-factor authentication status](#check-a-users-multi-factor-authentication-status) or [Check a user’s authentication contact info](#check-a-users-authentication-contact-info)</span></span>

-   <span data-ttu-id="633e4-135">För **Microsoft** **program som kräver en licens** (till exempel Office 365) sparas här är några problem med att kontrollera när du har konstaterat tillvägagångssättet ovan:</span><span class="sxs-lookup"><span data-stu-id="633e4-135">For **Microsoft** **applications that require a license** (like Office365), here are some specific issues to check once you have ruled out the general issues above:</span></span>

   * <span data-ttu-id="633e4-136">Se till att användaren eller har en **-licens.**</span><span class="sxs-lookup"><span data-stu-id="633e4-136">Ensure the user or has a **license assigned.**</span></span> <span data-ttu-id="633e4-137">[Kontrollera en användares tilldelade licenser](#check-a-users-assigned-licenses) eller [Kontrollera tilldelade licenser för en grupp](#check-a-groups-assigned-licenses)</span><span class="sxs-lookup"><span data-stu-id="633e4-137">[Check a user’s assigned licenses](#check-a-users-assigned-licenses) or [Check a group’s assigned licenses](#check-a-groups-assigned-licenses)</span></span>

   * <span data-ttu-id="633e4-138">Om licensen **tilldelas en** **statisk grupp**, se till att den **användaren är medlem** i gruppen.</span><span class="sxs-lookup"><span data-stu-id="633e4-138">If the license is **assigned to a** **static group**, ensure that the **user is a member** of that group.</span></span> [<span data-ttu-id="633e4-139">Kontrollera en användares gruppmedlemskap</span><span class="sxs-lookup"><span data-stu-id="633e4-139">Check a user’s group memberships</span></span>](#check-a-users-group-memberships)

   * <span data-ttu-id="633e4-140">Om licensen **tilldelas en** **dynamisk grupp**, se till att den **dynamisk gruppregeln är korrekt**.</span><span class="sxs-lookup"><span data-stu-id="633e4-140">If the license is **assigned to a** **dynamic group**, ensure that the **dynamic group rule is set correctly**.</span></span> [<span data-ttu-id="633e4-141">Kontrollera kriterier för medlemskap i en dynamisk grupp</span><span class="sxs-lookup"><span data-stu-id="633e4-141">Check a dynamic group’s membership criteria</span></span>](#check-a-dynamic-groups-membership-criteria)

   * <span data-ttu-id="633e4-142">Om licensen **tilldelas en** **dynamisk grupp**, se till att den dynamiska gruppen har **har behandlats klart** sitt medlemskap och att den **användaren är medlem**  (Detta kan ta lite tid).</span><span class="sxs-lookup"><span data-stu-id="633e4-142">If the license is **assigned to a** **dynamic group**, ensure that the dynamic group has **finished processing** its membership and that the **user is a member** (this can take some time).</span></span> [<span data-ttu-id="633e4-143">Kontrollera en användares gruppmedlemskap</span><span class="sxs-lookup"><span data-stu-id="633e4-143">Check a user’s group memberships</span></span>](#check-a-users-group-memberships)

   *  <span data-ttu-id="633e4-144">När du kontrollera licensen tilldelas Kontrollera licensen **inte gått**.</span><span class="sxs-lookup"><span data-stu-id="633e4-144">Once you make sure the license is assigned, make sure the license is **not expired**.</span></span>

   *  <span data-ttu-id="633e4-145">Kontrollera att licensen **för programmet** de använder.</span><span class="sxs-lookup"><span data-stu-id="633e4-145">Make sure the license is **for the application** they are accessing.</span></span>

-   <span data-ttu-id="633e4-146">För **Microsoft** **program som inte kräver en licens**, här är några saker att kontrollera:</span><span class="sxs-lookup"><span data-stu-id="633e4-146">For **Microsoft** **applications that don’t require a license**, here are some other things to check:</span></span>

   * <span data-ttu-id="633e4-147">Om programmet begär **behörigheter på objektnivå** (till exempel ”komma åt den här användarens postlåda”), se till att användaren har loggat in till programmet och har utfört en **användarnivå medgivande åtgärden** så att appen kan komma åt sina data.</span><span class="sxs-lookup"><span data-stu-id="633e4-147">If the application is requesting **user-level permissions** (for example “Access this user’s mailbox”), make sure that the user has signed in to the application and has performed a **user-level consent operation** to let the application access her data.</span></span>

   * <span data-ttu-id="633e4-148">Om programmet begär **på administratörsnivå** (till exempel ”åt postlådor för alla användare”), se till att en Global administratör har genomfört en **administratörsnivå medgivande åtgärden på räkning av alla användare** i organisationen.</span><span class="sxs-lookup"><span data-stu-id="633e4-148">If the application is requesting **administrator-level permissions** (for example “Access all user’s mailboxes”), make sure that a Global Administrator has performed an **administrator-level consent operation on behalf of all users** in the organization.</span></span>

## <a name="problems-with-the-users-account"></a><span data-ttu-id="633e4-149">Problem med användarkontot</span><span class="sxs-lookup"><span data-stu-id="633e4-149">Problems with the user’s account</span></span>

<span data-ttu-id="633e4-150">Programåtkomst blockeras på grund av ett problem med en användare som har tilldelats programmet.</span><span class="sxs-lookup"><span data-stu-id="633e4-150">Application access can be blocked due to a problem with a user that is assigned to the application.</span></span> <span data-ttu-id="633e4-151">Här följer några metoder som du kan felsöka och lösa problem med användare och deras kontoinställningar:</span><span class="sxs-lookup"><span data-stu-id="633e4-151">Below are some ways you can troubleshoot and solve problems with users and their account settings:</span></span>

-   [<span data-ttu-id="633e4-152">Kontrollera om det finns ett konto i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="633e4-152">Check if a user account exists in Azure Active Directory</span></span>](#check-if-a-user-account-exists-in-azure-active-directory)

-   [<span data-ttu-id="633e4-153">Kontrollera status för en användare</span><span class="sxs-lookup"><span data-stu-id="633e4-153">Check a user’s account status</span></span>](#check-a-users-account-status)

-   [<span data-ttu-id="633e4-154">Återställa en användares lösenord</span><span class="sxs-lookup"><span data-stu-id="633e4-154">Reset a user’s password</span></span>](#reset-a-users-password)

-   [<span data-ttu-id="633e4-155">Aktivera lösenordsåterställning via självbetjäning</span><span class="sxs-lookup"><span data-stu-id="633e4-155">Enable self-service password reset</span></span>](#enable-self-service-password-reset)

-   [<span data-ttu-id="633e4-156">Kontrollera status för multifaktorautentisering för en användare</span><span class="sxs-lookup"><span data-stu-id="633e4-156">Check a user’s multi-factor authentication status</span></span>](#check-a-users-multi-factor-authentication-status)

-   [<span data-ttu-id="633e4-157">Kontrollera en användares kontaktinformation för autentisering</span><span class="sxs-lookup"><span data-stu-id="633e4-157">Check a user’s authentication contact info</span></span>](#check-a-users-authentication-contact-info)

-   [<span data-ttu-id="633e4-158">Kontrollera en användares gruppmedlemskap</span><span class="sxs-lookup"><span data-stu-id="633e4-158">Check a user’s group memberships</span></span>](#check-a-users-group-memberships)

-   [<span data-ttu-id="633e4-159">Kontrollera en användares tilldelade licenser</span><span class="sxs-lookup"><span data-stu-id="633e4-159">Check a user’s assigned licenses</span></span>](#check-a-users-assigned-licenses)

-   [<span data-ttu-id="633e4-160">Tilldela en användare en licens</span><span class="sxs-lookup"><span data-stu-id="633e4-160">Assign a user a license</span></span>](#assign-a-user-a-license)

### <a name="check-if-a-user-account-exists-in-azure-active-directory"></a><span data-ttu-id="633e4-161">Kontrollera om det finns ett konto i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="633e4-161">Check if a user account exists in Azure Active Directory</span></span>

<span data-ttu-id="633e4-162">Följ stegen nedan om du vill kontrollera om det finns ett användarkonto:</span><span class="sxs-lookup"><span data-stu-id="633e4-162">To check if a user’s account is present, follow the steps below:</span></span>

1.  <span data-ttu-id="633e4-163">Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="633e4-163">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="633e4-164">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="633e4-164">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="633e4-165">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="633e4-165">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="633e4-166">Klicka på **användare och grupper** på navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="633e4-166">click **Users and groups** in the navigation menu.</span></span>

5.  <span data-ttu-id="633e4-167">Klicka på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="633e4-167">click **All users**.</span></span>

6.  <span data-ttu-id="633e4-168">**Sök** för den användare som du är intresserad av och **klickar du på raden** att välja.</span><span class="sxs-lookup"><span data-stu-id="633e4-168">**Search** for the user you are interested in and **click the row** to select.</span></span>

7.  <span data-ttu-id="633e4-169">Kontrollera egenskaperna för användarobjektet för att säkerställa att de ser ut som förväntat och inga data saknas.</span><span class="sxs-lookup"><span data-stu-id="633e4-169">Check the properties of the user object to be sure that they look as you expect and no data is missing.</span></span>

### <a name="check-a-users-account-status"></a><span data-ttu-id="633e4-170">Kontrollera status för en användare</span><span class="sxs-lookup"><span data-stu-id="633e4-170">Check a user’s account status</span></span>

<span data-ttu-id="633e4-171">Följ stegen nedan om du vill kontrollera status för en användare:</span><span class="sxs-lookup"><span data-stu-id="633e4-171">To check a user’s account status, follow the steps below:</span></span>

1.  <span data-ttu-id="633e4-172">Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="633e4-172">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="633e4-173">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="633e4-173">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="633e4-174">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="633e4-174">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="633e4-175">Klicka på **användare och grupper** på navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="633e4-175">click **Users and groups** in the navigation menu.</span></span>

5.  <span data-ttu-id="633e4-176">Klicka på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="633e4-176">click **All users**.</span></span>

6.  <span data-ttu-id="633e4-177">**Sök** för den användare som du är intresserad av och **klickar du på raden** att välja.</span><span class="sxs-lookup"><span data-stu-id="633e4-177">**Search** for the user you are interested in and **click the row** to select.</span></span>

7.  <span data-ttu-id="633e4-178">Klicka på **profil**.</span><span class="sxs-lookup"><span data-stu-id="633e4-178">click **Profile**.</span></span>

8.  <span data-ttu-id="633e4-179">Under **inställningar** se till att **Block logga in** är inställd på **nr**.</span><span class="sxs-lookup"><span data-stu-id="633e4-179">Under **Settings** ensure that **Block sign in** is set to **No**.</span></span>

### <a name="reset-a-users-password"></a><span data-ttu-id="633e4-180">Återställa en användares lösenord</span><span class="sxs-lookup"><span data-stu-id="633e4-180">Reset a user’s password</span></span>

<span data-ttu-id="633e4-181">Följ stegen nedan om du vill återställa en användares lösenord:</span><span class="sxs-lookup"><span data-stu-id="633e4-181">To reset a user’s password, follow the steps below:</span></span>

1.  <span data-ttu-id="633e4-182">Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="633e4-182">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="633e4-183">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="633e4-183">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="633e4-184">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="633e4-184">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="633e4-185">Klicka på **användare och grupper** på navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="633e4-185">click **Users and groups** in the navigation menu.</span></span>

5.  <span data-ttu-id="633e4-186">Klicka på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="633e4-186">click **All users**.</span></span>

6.  <span data-ttu-id="633e4-187">**Sök** för den användare som du är intresserad av och **klickar du på raden** att välja.</span><span class="sxs-lookup"><span data-stu-id="633e4-187">**Search** for the user you are interested in and **click the row** to select.</span></span>

7.  <span data-ttu-id="633e4-188">Klicka på den **Återställ lösenord** längst upp på bladet för användaren.</span><span class="sxs-lookup"><span data-stu-id="633e4-188">click the **Reset password** button at the top of the user blade.</span></span>

8.  <span data-ttu-id="633e4-189">Klicka på den **Återställ lösenord** knappen på den **Återställ lösenord** bladet som visas.</span><span class="sxs-lookup"><span data-stu-id="633e4-189">click the **Reset password** button on the **Reset password** blade that appears.</span></span>

9.  <span data-ttu-id="633e4-190">Kopiera den **tillfälligt lösenord** eller **ange ett nytt lösenord** för användaren.</span><span class="sxs-lookup"><span data-stu-id="633e4-190">Copy the **temporary password** or **enter a new password** for the user.</span></span>

10. <span data-ttu-id="633e4-191">Meddela detta nya lösenord för användaren, de krävas för att ändra lösenordet vid nästa inloggning i till Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="633e4-191">Communicate this new password to the user, they be required to change this password during their next sign in to Azure Active Directory.</span></span>

### <a name="enable-self-service-password-reset"></a><span data-ttu-id="633e4-192">Aktivera lösenordsåterställning via självbetjäning</span><span class="sxs-lookup"><span data-stu-id="633e4-192">Enable self-service password reset</span></span>

<span data-ttu-id="633e4-193">Om du vill aktivera lösenordsåterställning via självbetjäning åtgärderna distribution nedan:</span><span class="sxs-lookup"><span data-stu-id="633e4-193">To enable self-service password reset, follow the deployment steps below:</span></span>

-   [<span data-ttu-id="633e4-194">Användarna kan återställa sina Azure Active Directory-lösenord</span><span class="sxs-lookup"><span data-stu-id="633e4-194">Enable users to reset their Azure Active Directory passwords</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started#enable-users-to-reset-their-azure-ad-passwords)

-   [<span data-ttu-id="633e4-195">Användarna kan återställa eller ändra sina lokala Active Directory-lösenord</span><span class="sxs-lookup"><span data-stu-id="633e4-195">Enable users to reset or change their Active Directory on-premises passwords</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started#enable-users-to-reset-or-change-their-ad-passwords)

### <a name="check-a-users-multi-factor-authentication-status"></a><span data-ttu-id="633e4-196">Kontrollera status för multifaktorautentisering för en användare</span><span class="sxs-lookup"><span data-stu-id="633e4-196">Check a user’s multi-factor authentication status</span></span>

<span data-ttu-id="633e4-197">Följ stegen nedan om du vill kontrollera status för multifaktorautentisering för en användare:</span><span class="sxs-lookup"><span data-stu-id="633e4-197">To check a user’s multi-factor authentication status, follow the steps below:</span></span>

1.  <span data-ttu-id="633e4-198">Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="633e4-198">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="633e4-199">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="633e4-199">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="633e4-200">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="633e4-200">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="633e4-201">Klicka på **användare och grupper** på navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="633e4-201">click **Users and groups** in the navigation menu.</span></span>

5.  <span data-ttu-id="633e4-202">Klicka på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="633e4-202">click **All users**.</span></span>

6.  <span data-ttu-id="633e4-203">Klicka på den **Multifaktorautentisering** längst upp på bladet.</span><span class="sxs-lookup"><span data-stu-id="633e4-203">click the **Multi-Factor Authentication** button at the top of the blade.</span></span>

7.  <span data-ttu-id="633e4-204">En gång i **Multi-Factor Authentication-administrationsportalen** belastning, kontrollera att du är på den **användare** fliken.</span><span class="sxs-lookup"><span data-stu-id="633e4-204">Once the **Multi-Factor Authentication Administration Portal** loads, ensure you are on the **Users** tab.</span></span>

8.  <span data-ttu-id="633e4-205">Hitta användaren i listan över användare genom att söka, filtrera eller sortera.</span><span class="sxs-lookup"><span data-stu-id="633e4-205">Find the user in the list of users by searching, filtering, or sorting.</span></span>

9.  <span data-ttu-id="633e4-206">Välj användaren i listan över användare och **aktivera**, **inaktivera**, eller **framtvinga** multifaktorautentisering efter behov.</span><span class="sxs-lookup"><span data-stu-id="633e4-206">Select the user from the list of users and **Enable**, **Disable**, or **Enforce** multi-factor authentication as desired.</span></span>

  * <span data-ttu-id="633e4-207">**Obs**: om en användare finns i en **tvingande** tillstånd, så kan du ange dem **inaktiverad** tillfälligt och låter dem tillbaka till deras konto.</span><span class="sxs-lookup"><span data-stu-id="633e4-207">**Note**: If a user is in an **Enforced** state, you may set them to **Disabled** temporarily to let them back into their account.</span></span> <span data-ttu-id="633e4-208">När de är tillbaka i sedan kan du ändra tillståndet till **aktiverad** igen för att kräva att registrera kontaktuppgifter under nästa inloggning i.</span><span class="sxs-lookup"><span data-stu-id="633e4-208">Once they are back in, you can then change their state to **Enabled** again to require them to re-register their contact information during their next sign in.</span></span> <span data-ttu-id="633e4-209">Alternativt, följer du stegen i den [Kontrollera en användares autentisering kontaktinformation](#check-a-users-authentication-contact-info) att kontrollera eller ange informationen för dessa.</span><span class="sxs-lookup"><span data-stu-id="633e4-209">Alternatively, you can follow the steps in the [Check a user’s authentication contact info](#check-a-users-authentication-contact-info) to verify or set this data for them.</span></span>

### <a name="check-a-users-authentication-contact-info"></a><span data-ttu-id="633e4-210">Kontrollera en användares kontaktinformation för autentisering</span><span class="sxs-lookup"><span data-stu-id="633e4-210">Check a user’s authentication contact info</span></span>

<span data-ttu-id="633e4-211">Följ stegen nedan om du vill kontrollera en användares autentisering kontaktinformation som användes för multifaktorautentisering, villkorlig åtkomst, identitetsskydd och återställning av lösenord:</span><span class="sxs-lookup"><span data-stu-id="633e4-211">To check a user’s authentication contact info used for Multi-factor authentication, Conditional Access, Identity Protection, and Password Reset, follow the steps below:</span></span>

1.  <span data-ttu-id="633e4-212">Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="633e4-212">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="633e4-213">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="633e4-213">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="633e4-214">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="633e4-214">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="633e4-215">Klicka på **användare och grupper** på navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="633e4-215">click **Users and groups** in the navigation menu.</span></span>

5.  <span data-ttu-id="633e4-216">Klicka på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="633e4-216">click **All users**.</span></span>

6.  <span data-ttu-id="633e4-217">**Sök** för den användare som du är intresserad av och **klickar du på raden** att välja.</span><span class="sxs-lookup"><span data-stu-id="633e4-217">**Search** for the user you are interested in and **click the row** to select.</span></span>

7.  <span data-ttu-id="633e4-218">Klicka på **profil**.</span><span class="sxs-lookup"><span data-stu-id="633e4-218">click **Profile**.</span></span>

8.  <span data-ttu-id="633e4-219">Rulla ned till **autentisering kontaktinformation**.</span><span class="sxs-lookup"><span data-stu-id="633e4-219">Scroll down to **Authentication contact info**.</span></span>

9.  <span data-ttu-id="633e4-220">**Granska** data som registrerats för användaren och uppdatera efter behov.</span><span class="sxs-lookup"><span data-stu-id="633e4-220">**Review** the data registered for the user and update as needed.</span></span>

### <a name="check-a-users-group-memberships"></a><span data-ttu-id="633e4-221">Kontrollera en användares gruppmedlemskap</span><span class="sxs-lookup"><span data-stu-id="633e4-221">Check a user’s group memberships</span></span>

<span data-ttu-id="633e4-222">Följ stegen nedan om du vill kontrollera en användares gruppmedlemskap:</span><span class="sxs-lookup"><span data-stu-id="633e4-222">To check a user’s group memberships, follow the steps below:</span></span>

1.  <span data-ttu-id="633e4-223">Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="633e4-223">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="633e4-224">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="633e4-224">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="633e4-225">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="633e4-225">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="633e4-226">Klicka på **användare och grupper** på navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="633e4-226">click **Users and groups** in the navigation menu.</span></span>

5.  <span data-ttu-id="633e4-227">Klicka på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="633e4-227">click **All users**.</span></span>

6.  <span data-ttu-id="633e4-228">**Sök** för den användare som du är intresserad av och **klickar du på raden** att välja.</span><span class="sxs-lookup"><span data-stu-id="633e4-228">**Search** for the user you are interested in and **click the row** to select.</span></span>

7.  <span data-ttu-id="633e4-229">Klicka på **grupper** att se vilka grupper användaren är medlem i.</span><span class="sxs-lookup"><span data-stu-id="633e4-229">click **Groups** to see which groups the user is a member of.</span></span>

### <a name="check-a-users-assigned-licenses"></a><span data-ttu-id="633e4-230">Kontrollera en användares tilldelade licenser</span><span class="sxs-lookup"><span data-stu-id="633e4-230">Check a user’s assigned licenses</span></span>

<span data-ttu-id="633e4-231">Följ stegen nedan om du vill kontrollera en användares tilldelade licenser:</span><span class="sxs-lookup"><span data-stu-id="633e4-231">To check a user’s assigned licenses, follow the steps below:</span></span>

1.  <span data-ttu-id="633e4-232">Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="633e4-232">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="633e4-233">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="633e4-233">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="633e4-234">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="633e4-234">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="633e4-235">Klicka på **användare och grupper** på navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="633e4-235">click **Users and groups** in the navigation menu.</span></span>

5.  <span data-ttu-id="633e4-236">Klicka på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="633e4-236">click **All users**.</span></span>

6.  <span data-ttu-id="633e4-237">**Sök** för den användare som du är intresserad av och **klickar du på raden** att välja.</span><span class="sxs-lookup"><span data-stu-id="633e4-237">**Search** for the user you are interested in and **click the row** to select.</span></span>

7.  <span data-ttu-id="633e4-238">Klicka på **licenser** finns som licenser användaren för närvarande har tilldelats.</span><span class="sxs-lookup"><span data-stu-id="633e4-238">click **Licenses** to see which licenses the user currently has assigned.</span></span>

### <a name="assign-a-user-a-license"></a><span data-ttu-id="633e4-239">Tilldela en användare en licens</span><span class="sxs-lookup"><span data-stu-id="633e4-239">Assign a user a license</span></span> 

<span data-ttu-id="633e4-240">Följ stegen nedan om du vill tilldela en licens till en användare:</span><span class="sxs-lookup"><span data-stu-id="633e4-240">To assign a license to a user, follow the steps below:</span></span>

1.  <span data-ttu-id="633e4-241">Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="633e4-241">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="633e4-242">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="633e4-242">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="633e4-243">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="633e4-243">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="633e4-244">Klicka på **användare och grupper** på navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="633e4-244">click **Users and groups** in the navigation menu.</span></span>

5.  <span data-ttu-id="633e4-245">Klicka på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="633e4-245">click **All users**.</span></span>

6.  <span data-ttu-id="633e4-246">**Sök** för den användare som du är intresserad av och **klickar du på raden** att välja.</span><span class="sxs-lookup"><span data-stu-id="633e4-246">**Search** for the user you are interested in and **click the row** to select.</span></span>

7.  <span data-ttu-id="633e4-247">Klicka på **licenser** finns som licenser användaren för närvarande har tilldelats.</span><span class="sxs-lookup"><span data-stu-id="633e4-247">click **Licenses** to see which licenses the user currently has assigned.</span></span>

8.  <span data-ttu-id="633e4-248">Klicka på den **tilldela** knappen.</span><span class="sxs-lookup"><span data-stu-id="633e4-248">click the **Assign** button.</span></span>

9.  <span data-ttu-id="633e4-249">Välj **en eller flera produkter** från listan över tillgängliga produkter.</span><span class="sxs-lookup"><span data-stu-id="633e4-249">Select **one or more products** from the list of available products.</span></span>

10. <span data-ttu-id="633e4-250">**Valfria** klickar du på den **tilldelning alternativ** objektet om du vill tilldela granularly produkter.</span><span class="sxs-lookup"><span data-stu-id="633e4-250">**Optional** click the **assignment options** item to granularly assign products.</span></span> <span data-ttu-id="633e4-251">Klicka på **Ok** när detta har slutförts.</span><span class="sxs-lookup"><span data-stu-id="633e4-251">Click **Ok** when this is completed.</span></span>

11. <span data-ttu-id="633e4-252">Klicka på den **tilldela** för att tilldela licenserna till den här användaren.</span><span class="sxs-lookup"><span data-stu-id="633e4-252">Click the **Assign** button to assign these licenses to this user.</span></span>

## <a name="problems-with-groups"></a><span data-ttu-id="633e4-253">Problem med grupper</span><span class="sxs-lookup"><span data-stu-id="633e4-253">Problems with groups</span></span>

<span data-ttu-id="633e4-254">Programåtkomst blockeras på grund av ett problem med en grupp som har tilldelats programmet.</span><span class="sxs-lookup"><span data-stu-id="633e4-254">Application access can be blocked due to a problem with a group that is assigned to the application.</span></span> <span data-ttu-id="633e4-255">Här följer några metoder som du kan felsöka och lösa problem med grupper och gruppmedlemskap:</span><span class="sxs-lookup"><span data-stu-id="633e4-255">Below are some ways you can troubleshoot and solve problems with groups and group memberships:</span></span>

-   [<span data-ttu-id="633e4-256">Kontrollera medlemskapet för en grupp</span><span class="sxs-lookup"><span data-stu-id="633e4-256">Check a group’s membership</span></span>](#check-a-groups-membership)

-   [<span data-ttu-id="633e4-257">Kontrollera kriterier för medlemskap i en dynamisk grupp</span><span class="sxs-lookup"><span data-stu-id="633e4-257">Check a dynamic group’s membership criteria</span></span>](#check-a-dynamic-groups-membership-criteria)

-   [<span data-ttu-id="633e4-258">Kontrollera tilldelade licenser för en grupp</span><span class="sxs-lookup"><span data-stu-id="633e4-258">Check a group’s assigned licenses</span></span>](#check-a-groups-assigned-licenses)

-   [<span data-ttu-id="633e4-259">Ombearbeta licenser för en grupp</span><span class="sxs-lookup"><span data-stu-id="633e4-259">Reprocess a group’s licenses</span></span>](#reprocess-a-groups-licenses)

-   [<span data-ttu-id="633e4-260">Tilldela en licens för en grupp</span><span class="sxs-lookup"><span data-stu-id="633e4-260">Assign a group a license</span></span>](#assign-a-group-a-license)

### <a name="check-a-groups-membership"></a><span data-ttu-id="633e4-261">Kontrollera medlemskapet för en grupp</span><span class="sxs-lookup"><span data-stu-id="633e4-261">Check a group’s membership</span></span>

<span data-ttu-id="633e4-262">Följ stegen nedan om du vill kontrollera en grupps medlemskap:</span><span class="sxs-lookup"><span data-stu-id="633e4-262">To check a group’s membership, follow the steps below:</span></span>

1.  <span data-ttu-id="633e4-263">Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="633e4-263">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="633e4-264">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="633e4-264">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="633e4-265">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="633e4-265">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="633e4-266">Klicka på **användare och grupper** på navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="633e4-266">click **Users and groups** in the navigation menu.</span></span>

5.  <span data-ttu-id="633e4-267">Klicka på **alla grupper**.</span><span class="sxs-lookup"><span data-stu-id="633e4-267">click **All groups**.</span></span>

6.  <span data-ttu-id="633e4-268">**Sök** för gruppen som du är intresserad av och **klickar du på raden** att välja.</span><span class="sxs-lookup"><span data-stu-id="633e4-268">**Search** for the group you are interested in and **click the row** to select.</span></span>

7.  <span data-ttu-id="633e4-269">Klicka på **medlemmar** att granska listan över användare som är tilldelade till den här gruppen.</span><span class="sxs-lookup"><span data-stu-id="633e4-269">click **Members** to review the list of users assigned to this group.</span></span>

### <a name="check-a-dynamic-groups-membership-criteria"></a><span data-ttu-id="633e4-270">Kontrollera kriterier för medlemskap i en dynamisk grupp</span><span class="sxs-lookup"><span data-stu-id="633e4-270">Check a dynamic group’s membership criteria</span></span> 

<span data-ttu-id="633e4-271">Följ stegen nedan om du vill kontrollera kriterier för medlemskap i en dynamisk grupp:</span><span class="sxs-lookup"><span data-stu-id="633e4-271">To check a dynamic group’s membership criteria, follow the steps below:</span></span>

1.  <span data-ttu-id="633e4-272">Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="633e4-272">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="633e4-273">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="633e4-273">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="633e4-274">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="633e4-274">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="633e4-275">Klicka på **användare och grupper** på navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="633e4-275">click **Users and groups** in the navigation menu.</span></span>

5.  <span data-ttu-id="633e4-276">Klicka på **alla grupper**.</span><span class="sxs-lookup"><span data-stu-id="633e4-276">click **All groups**.</span></span>

6.  <span data-ttu-id="633e4-277">**Sök** för gruppen som du är intresserad av och **klickar du på raden** att välja.</span><span class="sxs-lookup"><span data-stu-id="633e4-277">**Search** for the group you are interested in and **click the row** to select.</span></span>

7.  <span data-ttu-id="633e4-278">Klicka på **dynamiskt medlemskapsregler.**</span><span class="sxs-lookup"><span data-stu-id="633e4-278">click **Dynamic membership rules.**</span></span>

8.  <span data-ttu-id="633e4-279">Granska de **enkel** eller **avancerade** regel definierad för den här gruppen och kontrollera att den användare som du vill ska vara medlem i den här gruppen uppfyller dessa villkor.</span><span class="sxs-lookup"><span data-stu-id="633e4-279">Review the **simple** or **advanced** rule defined for this group and ensure that the user you want to be a member of this group meets these criteria.</span></span>

### <a name="check-a-groups-assigned-licenses"></a><span data-ttu-id="633e4-280">Kontrollera tilldelade licenser för en grupp</span><span class="sxs-lookup"><span data-stu-id="633e4-280">Check a group’s assigned licenses</span></span>

<span data-ttu-id="633e4-281">Följ stegen nedan om du vill kontrollera tilldelade licenser för en grupp:</span><span class="sxs-lookup"><span data-stu-id="633e4-281">To check a group’s assigned licenses, follow the steps below:</span></span>

1.  <span data-ttu-id="633e4-282">Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="633e4-282">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="633e4-283">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="633e4-283">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="633e4-284">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="633e4-284">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="633e4-285">Klicka på **användare och grupper** på navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="633e4-285">click **Users and groups** in the navigation menu.</span></span>

5.  <span data-ttu-id="633e4-286">Klicka på **alla grupper**.</span><span class="sxs-lookup"><span data-stu-id="633e4-286">click **All groups**.</span></span>

6.  <span data-ttu-id="633e4-287">**Sök** för gruppen som du är intresserad av och **klickar du på raden** att välja.</span><span class="sxs-lookup"><span data-stu-id="633e4-287">**Search** for the group you are interested in and **click the row** to select.</span></span>

7.  <span data-ttu-id="633e4-288">Klicka på **licenser** finns som licenser gruppen för närvarande har tilldelats.</span><span class="sxs-lookup"><span data-stu-id="633e4-288">click **Licenses** to see which licenses the group currently has assigned.</span></span>

### <a name="reprocess-a-groups-licenses"></a><span data-ttu-id="633e4-289">Ombearbeta licenser för en grupp</span><span class="sxs-lookup"><span data-stu-id="633e4-289">Reprocess a group’s licenses</span></span>

<span data-ttu-id="633e4-290">Följ stegen nedan för att Ombearbeta tilldelade licenser för en grupp:</span><span class="sxs-lookup"><span data-stu-id="633e4-290">To reprocess a group’s assigned licenses, follow the steps below:</span></span>

1.  <span data-ttu-id="633e4-291">Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="633e4-291">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="633e4-292">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="633e4-292">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="633e4-293">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="633e4-293">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="633e4-294">Klicka på **användare och grupper** på navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="633e4-294">click **Users and groups** in the navigation menu.</span></span>

5.  <span data-ttu-id="633e4-295">Klicka på **alla grupper**.</span><span class="sxs-lookup"><span data-stu-id="633e4-295">click **All groups**.</span></span>

6.  <span data-ttu-id="633e4-296">**Sök** för gruppen som du är intresserad av och **klickar du på raden** att välja.</span><span class="sxs-lookup"><span data-stu-id="633e4-296">**Search** for the group you are interested in and **click the row** to select.</span></span>

7.  <span data-ttu-id="633e4-297">Klicka på **licenser** finns som licenser gruppen för närvarande har tilldelats.</span><span class="sxs-lookup"><span data-stu-id="633e4-297">click **Licenses** to see which licenses the group currently has assigned.</span></span>

8.  <span data-ttu-id="633e4-298">Klicka på den **Ombearbeta** för att säkerställa att de licenser som tilldelats den här gruppens medlemmar är uppdaterade.</span><span class="sxs-lookup"><span data-stu-id="633e4-298">click the **Reprocess** button to ensure that the licenses assigned to this group’s members are up-to-date.</span></span> <span data-ttu-id="633e4-299">Det kan ta lång tid, beroende på storleken och komplexiteten i gruppen.</span><span class="sxs-lookup"><span data-stu-id="633e4-299">This may take a long time, depending on the size and complexity of the group.</span></span>

   >[!NOTE]
   ><span data-ttu-id="633e4-300">Överväg att tillfälligt tilldela en licens till användaren direkt om du vill göra detta snabbare.</span><span class="sxs-lookup"><span data-stu-id="633e4-300">To do this faster, consider temporarily assigning a license to the user directly.</span></span> <span data-ttu-id="633e4-301">[Tilldela en användare en licens](#problems-with-application-consent).</span><span class="sxs-lookup"><span data-stu-id="633e4-301">[Assign a user a license](#problems-with-application-consent).</span></span>
   >
   >

### <a name="assign-a-group-a-license"></a><span data-ttu-id="633e4-302">Tilldela en licens för en grupp</span><span class="sxs-lookup"><span data-stu-id="633e4-302">Assign a group a license</span></span>

<span data-ttu-id="633e4-303">Följ stegen nedan om du vill tilldela en licens till en grupp:</span><span class="sxs-lookup"><span data-stu-id="633e4-303">To assign a license to a group, follow the steps below:</span></span>

1.  <span data-ttu-id="633e4-304">Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="633e4-304">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="633e4-305">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="633e4-305">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="633e4-306">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="633e4-306">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="633e4-307">Klicka på **användare och grupper** på navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="633e4-307">click **Users and groups** in the navigation menu.</span></span>

5.  <span data-ttu-id="633e4-308">Klicka på **alla grupper**.</span><span class="sxs-lookup"><span data-stu-id="633e4-308">click **All groups**.</span></span>

6.  <span data-ttu-id="633e4-309">**Sök** för gruppen som du är intresserad av och **klickar du på raden** att välja.</span><span class="sxs-lookup"><span data-stu-id="633e4-309">**Search** for the group you are interested in and **click the row** to select.</span></span>

7.  <span data-ttu-id="633e4-310">Klicka på **licenser** finns som licenser gruppen för närvarande har tilldelats.</span><span class="sxs-lookup"><span data-stu-id="633e4-310">click **Licenses** to see which licenses the group currently has assigned.</span></span>

8.  <span data-ttu-id="633e4-311">Klicka på den **tilldela** knappen.</span><span class="sxs-lookup"><span data-stu-id="633e4-311">click the **Assign** button.</span></span>

9.  <span data-ttu-id="633e4-312">Välj **en eller flera produkter** från listan över tillgängliga produkter.</span><span class="sxs-lookup"><span data-stu-id="633e4-312">Select **one or more products** from the list of available products.</span></span>

10. <span data-ttu-id="633e4-313">**Valfria** klickar du på den **tilldelning alternativ** objektet om du vill tilldela granularly produkter.</span><span class="sxs-lookup"><span data-stu-id="633e4-313">**Optional** click the **assignment options** item to granularly assign products.</span></span> <span data-ttu-id="633e4-314">Klicka på **Ok** när detta har slutförts.</span><span class="sxs-lookup"><span data-stu-id="633e4-314">Click **Ok** when this is completed.</span></span>

11. <span data-ttu-id="633e4-315">Klicka på den **tilldela** för att tilldela licenserna till den här gruppen.</span><span class="sxs-lookup"><span data-stu-id="633e4-315">Click the **Assign** button to assign these licenses to this group.</span></span> <span data-ttu-id="633e4-316">Det kan ta lång tid, beroende på storleken och komplexiteten i gruppen.</span><span class="sxs-lookup"><span data-stu-id="633e4-316">This may take a long time, depending on the size and complexity of the group.</span></span>

   >[!NOTE]
   ><span data-ttu-id="633e4-317">Överväg att tillfälligt tilldela en licens till användaren direkt om du vill göra detta snabbare.</span><span class="sxs-lookup"><span data-stu-id="633e4-317">To do this faster, consider temporarily assigning a license to the user directly.</span></span> <span data-ttu-id="633e4-318">[Tilldela en användare en licens](#problems-with-application-consent).</span><span class="sxs-lookup"><span data-stu-id="633e4-318">[Assign a user a license](#problems-with-application-consent).</span></span>
   > 
   >

## <a name="problems-with-conditional-access-policies"></a><span data-ttu-id="633e4-319">Problem med principer för villkorlig åtkomst</span><span class="sxs-lookup"><span data-stu-id="633e4-319">Problems with conditional access policies</span></span>

### <a name="check-a-specific-conditional-access-policy"></a><span data-ttu-id="633e4-320">Kontrollera en princip för specifik villkorlig åtkomst</span><span class="sxs-lookup"><span data-stu-id="633e4-320">Check a specific conditional access policy</span></span>

<span data-ttu-id="633e4-321">Kontrollera eller validera en princip för enskild villkorlig åtkomst:</span><span class="sxs-lookup"><span data-stu-id="633e4-321">To check or validate a single conditional access policy:</span></span>

1.  <span data-ttu-id="633e4-322">Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="633e4-322">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="633e4-323">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="633e4-323">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="633e4-324">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="633e4-324">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="633e4-325">Klicka på **företagsprogram** på navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="633e4-325">click **Enterprise applications** in the navigation menu.</span></span>

5.  <span data-ttu-id="633e4-326">Klicka på den **villkorlig åtkomst** navigeringsobjektet.</span><span class="sxs-lookup"><span data-stu-id="633e4-326">click the **Conditional access** navigation item.</span></span>

6.  <span data-ttu-id="633e4-327">Klicka på den princip som du är intresserad undersöks.</span><span class="sxs-lookup"><span data-stu-id="633e4-327">click the policy you are interested in inspecting.</span></span>

7.  <span data-ttu-id="633e4-328">Granska att det finns inga särskilda villkor, tilldelningar och andra inställningar som kan blockera åtkomst.</span><span class="sxs-lookup"><span data-stu-id="633e4-328">Review that there are no specific conditions, assignments, or other settings which may be blocking user access.</span></span>

   >[!NOTE]
   ><span data-ttu-id="633e4-329">Kan du tillfälligt inaktiverar den här principen för att säkerställa att det inte påverkar inloggningar.</span><span class="sxs-lookup"><span data-stu-id="633e4-329">You may wish to temporarily disable this policy to ensure it is not affecting sign ins.</span></span> <span data-ttu-id="633e4-330">Gör detta genom att ange den **aktiverar principen** växla till **nr** och klicka på den **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="633e4-330">To do this, set the **Enable policy** toggle to **No** and click the **Save** button.</span></span>
   >
   >

### <a name="check-a-specific-applications-conditional-access-policy"></a><span data-ttu-id="633e4-331">Kontrollera principen för villkorlig åtkomst för ett visst program</span><span class="sxs-lookup"><span data-stu-id="633e4-331">Check a specific application’s conditional access policy</span></span>

<span data-ttu-id="633e4-332">Om du vill kontrollera eller verifiera ett enda program för tillfället konfigurerade principen för villkorlig åtkomst:</span><span class="sxs-lookup"><span data-stu-id="633e4-332">To check or validate a single application’s currently configured conditional access policy:</span></span>

1.  <span data-ttu-id="633e4-333">Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="633e4-333">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="633e4-334">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="633e4-334">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="633e4-335">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="633e4-335">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="633e4-336">Klicka på **företagsprogram** på navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="633e4-336">click **Enterprise applications** in the navigation menu.</span></span>

5.  <span data-ttu-id="633e4-337">Klicka på **alla program**.</span><span class="sxs-lookup"><span data-stu-id="633e4-337">click **All applications**.</span></span>

6.  <span data-ttu-id="633e4-338">Sök efter programmet som du är intresserad eller användaren försöker logga in med programmets visningsnamn eller id.</span><span class="sxs-lookup"><span data-stu-id="633e4-338">Search for the application you are interested in, or the user is attempting to sign in to by application display name or application id.</span></span>

     >[!NOTE]
     ><span data-ttu-id="633e4-339">Om programmet som du söker efter inte visas klickar du på den **Filter** knappen och expandera omfattningen i listan för att **alla program**.</span><span class="sxs-lookup"><span data-stu-id="633e4-339">If you don’t see the application you are looking for, click the **Filter** button and expand the scope of the list to **All applications**.</span></span> <span data-ttu-id="633e4-340">Om du vill se fler kolumner klickar du på den **kolumner** för att lägga till ytterligare information om dina program.</span><span class="sxs-lookup"><span data-stu-id="633e4-340">If you want to see more columns, click the **Columns** button to add additional details for your applications.</span></span>
     >
     >

7.  <span data-ttu-id="633e4-341">Klicka på den **villkorlig åtkomst** navigeringsobjektet.</span><span class="sxs-lookup"><span data-stu-id="633e4-341">click the **Conditional access** navigation item.</span></span>

8.  <span data-ttu-id="633e4-342">Klicka på den princip som du är intresserad undersöks.</span><span class="sxs-lookup"><span data-stu-id="633e4-342">click the policy you are interested in inspecting.</span></span>

9.  <span data-ttu-id="633e4-343">Granska att det finns inga särskilda villkor, tilldelningar och andra inställningar som kan blockera åtkomst.</span><span class="sxs-lookup"><span data-stu-id="633e4-343">Review that there are no specific conditions, assignments, or other settings which may be blocking user access.</span></span>

     >[!NOTE]
     ><span data-ttu-id="633e4-344">Kan du tillfälligt inaktiverar den här principen för att säkerställa att det inte påverkar inloggningar.</span><span class="sxs-lookup"><span data-stu-id="633e4-344">You may wish to temporarily disable this policy to ensure it is not affecting sign ins.</span></span> <span data-ttu-id="633e4-345">Gör detta genom att ange den **aktiverar principen** växla till **nr** och klicka på den **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="633e4-345">To do this, set the **Enable policy** toggle to **No** and click the **Save** button.</span></span>
     >
     >

### <a name="disable-a-specific-conditional-access-policy"></a><span data-ttu-id="633e4-346">Inaktivera en princip för specifik villkorlig åtkomst</span><span class="sxs-lookup"><span data-stu-id="633e4-346">Disable a specific conditional access policy</span></span>

<span data-ttu-id="633e4-347">Kontrollera eller validera en princip för enskild villkorlig åtkomst:</span><span class="sxs-lookup"><span data-stu-id="633e4-347">To check or validate a single conditional access policy:</span></span>

1.  <span data-ttu-id="633e4-348">Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="633e4-348">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="633e4-349">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="633e4-349">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="633e4-350">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="633e4-350">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="633e4-351">Klicka på **företagsprogram** på navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="633e4-351">click **Enterprise applications** in the navigation menu.</span></span>

5.  <span data-ttu-id="633e4-352">Klicka på den **villkorlig åtkomst** navigeringsobjektet.</span><span class="sxs-lookup"><span data-stu-id="633e4-352">click the **Conditional access** navigation item.</span></span>

6.  <span data-ttu-id="633e4-353">Klicka på den princip som du är intresserad undersöks.</span><span class="sxs-lookup"><span data-stu-id="633e4-353">click the policy you are interested in inspecting.</span></span>

7.  <span data-ttu-id="633e4-354">Inaktivera principen genom att ange den **aktiverar principen** växla till **nr** och klicka på den **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="633e4-354">Disable the policy by setting the **Enable policy** toggle to **No** and click the **Save** button.</span></span>

## <a name="problems-with-application-consent"></a><span data-ttu-id="633e4-355">Problem med programmet medgivande</span><span class="sxs-lookup"><span data-stu-id="633e4-355">Problems with application consent</span></span>

<span data-ttu-id="633e4-356">Programåtkomst blockeras eftersom åtkomstbehörighet medgivande åtgärden inte har inträffat.</span><span class="sxs-lookup"><span data-stu-id="633e4-356">Application access can be blocked because the proper permissions consent operation has not occurred.</span></span> <span data-ttu-id="633e4-357">Här följer några metoder som du kan felsöka och lösa problem med programmet medgivande:</span><span class="sxs-lookup"><span data-stu-id="633e4-357">Below are some ways you can troubleshoot and solve application consent issues:</span></span>

-   [<span data-ttu-id="633e4-358">Utföra en åtgärd på användarnivå medgivande</span><span class="sxs-lookup"><span data-stu-id="633e4-358">Perform a user-level consent operation</span></span>](#perform-a-user-level-consent-operation)

-   [<span data-ttu-id="633e4-359">Utföra administratörsnivå medgivande åtgärden för alla program</span><span class="sxs-lookup"><span data-stu-id="633e4-359">Perform administrator-level consent operation for any application</span></span>](#perform-administrator-level-consent-operation-for-any-application)

-   [<span data-ttu-id="633e4-360">Utföra administratörsnivå medgivande för en enskild klient-program</span><span class="sxs-lookup"><span data-stu-id="633e4-360">Perform administrator-level consent for a single-tenant application</span></span>](#perform-administrator-level-consent-for-a-single-tenant-application)

-   [<span data-ttu-id="633e4-361">Utföra administratörsnivå medgivande för ett program med flera innehavare</span><span class="sxs-lookup"><span data-stu-id="633e4-361">Perform administrator-level consent for a multi-tenant application</span></span>](#perform-administrator-level-consent-for-a-multi-tenant-application)

### <a name="perform-a-user-level-consent-operation"></a><span data-ttu-id="633e4-362">Utföra en åtgärd på användarnivå medgivande</span><span class="sxs-lookup"><span data-stu-id="633e4-362">Perform a user-level consent operation</span></span>

-   <span data-ttu-id="633e4-363">Alla öppna ID Connect-aktiverade program som begär behörighet, utför navigera till programmets inloggning skärmen en nivå tillstånd till programmet för den inloggade användaren.</span><span class="sxs-lookup"><span data-stu-id="633e4-363">For any Open ID Connect-enabled application that requests permissions, navigating to the application’s sign in screen performs a user level consent to the application for the signed-in user.</span></span>

-   <span data-ttu-id="633e4-364">Om du vill göra det programmässigt finns [begär enskilda användargodkännande](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#requesting-individual-user-consent).</span><span class="sxs-lookup"><span data-stu-id="633e4-364">If you wish to do this programmatically, see [Requesting individual user consent](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#requesting-individual-user-consent).</span></span>

### <a name="perform-administrator-level-consent-operation-for-any-application"></a><span data-ttu-id="633e4-365">Utföra administratörsnivå medgivande åtgärden för alla program</span><span class="sxs-lookup"><span data-stu-id="633e4-365">Perform administrator-level consent operation for any application</span></span>

-   <span data-ttu-id="633e4-366">För **bara program som utvecklats med programmodell V1**, du kan framtvinga den här nivån medgivande administratör ska ske genom att lägga till ”**? uppmana = admin\_medgivande**” i slutet av en programmets tecken i URL: en.</span><span class="sxs-lookup"><span data-stu-id="633e4-366">For **only applications developed using the V1 application model**, you can force this administrator level consent to occur by adding “**?prompt=admin\_consent**” to the end of an application’s sign in URL.</span></span>

-   <span data-ttu-id="633e4-367">För **alla program som utvecklats med programmodell V2**, kan du använda den här nivån administratör medgivande till uppstå genom att följa anvisningarna under den **begära behörigheter från katalogadministratör** avsnitt i [med medgivande adminslutpunkten](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint).</span><span class="sxs-lookup"><span data-stu-id="633e4-367">For **any application developed using the V2 application model**, you can enforce this administrator-level consent to occur by following the instructions under the **Request the permissions from a directory admin** section of [Using the admin consent endpoint](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint).</span></span>

### <a name="perform-administrator-level-consent-for-a-single-tenant-application"></a><span data-ttu-id="633e4-368">Utföra administratörsnivå medgivande för en enskild klient-program</span><span class="sxs-lookup"><span data-stu-id="633e4-368">Perform administrator-level consent for a single-tenant application</span></span>

-   <span data-ttu-id="633e4-369">För **program enkel klienter** som begär behörighet (till exempel de som du utvecklar eller äger i din organisation), kan du utföra en **administrativ nivå medgivande** åtgärden för alla användare genom att logga in som Global administratör och klicka på den **bevilja behörigheter** längst upp i den **programmet registret -&gt; alla program -&gt; Välj en App -&gt; Nödvändiga behörigheter** bladet.</span><span class="sxs-lookup"><span data-stu-id="633e4-369">For **single-tenant applications** that request permissions (like those you are developing or own in your organization), you can perform an **administrative-level consent** operation on behalf of all users by signing in as a Global Administrator and clicking on the **Grant permissions** button at the top of the **Application Registry -&gt; All Applications -&gt; Select an App -&gt; Required Permissions** blade.</span></span>

-   <span data-ttu-id="633e4-370">För **alla program som utvecklats med programmodell V1 eller V2**, kan du använda den här nivån administratör medgivande till uppstå genom att följa anvisningarna under den **begära behörigheter från en katalogadministratör**  avsnitt i [med medgivande adminslutpunkten](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint).</span><span class="sxs-lookup"><span data-stu-id="633e4-370">For **any application developed using the V1 or V2 application model**, you can enforce this administrator-level consent to occur by following the instructions under the **Request the permissions from a directory admin** section of [Using the admin consent endpoint](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint).</span></span>

### <a name="perform-administrator-level-consent-for-a-multi-tenant-application"></a><span data-ttu-id="633e4-371">Utföra administratörsnivå medgivande för ett program med flera innehavare</span><span class="sxs-lookup"><span data-stu-id="633e4-371">Perform administrator-level consent for a multi-tenant application</span></span>

-   <span data-ttu-id="633e4-372">För **program med flera klienter** som begär behörighet (t.ex. ett program en tredje part eller Microsoft, utvecklar), kan du utföra en **administrativ nivå medgivande** igen.</span><span class="sxs-lookup"><span data-stu-id="633e4-372">For **multi-tenant applications** that request permissions (like an application a third party, or Microsoft, develops), you can perform an **administrative-level consent** operation.</span></span> <span data-ttu-id="633e4-373">Logga in som Global administratör och klicka på den **bevilja behörigheter** knappen den **företagsprogram -&gt; alla program -&gt; Välj en App -&gt; behörigheter**  bladet (tillgänglig snart).</span><span class="sxs-lookup"><span data-stu-id="633e4-373">Sign in as a Global Administrator and clicking on the **Grant permissions** button under the **Enterprise Applications -&gt; All Applications -&gt; Select an App -&gt; Permissions** blade (available soon).</span></span>

-   <span data-ttu-id="633e4-374">Kan du också använda den här nivån administratör medgivande inträffar genom att följa anvisningarna i avsnittet den **begära behörigheter från en katalogadministratör** avsnitt i [med medgivande adminslutpunkten](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint).</span><span class="sxs-lookup"><span data-stu-id="633e4-374">You can also enforce this administrator-level consent to occur by following the instructions under the **Request the permissions from a directory admin** section of [Using the admin consent endpoint](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint).</span></span>

## <a name="next-steps"></a><span data-ttu-id="633e4-375">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="633e4-375">Next steps</span></span>
[<span data-ttu-id="633e4-376">Med hjälp av admin medgivande-slutpunkten</span><span class="sxs-lookup"><span data-stu-id="633e4-376">Using the admin consent endpoint</span></span>](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint)

