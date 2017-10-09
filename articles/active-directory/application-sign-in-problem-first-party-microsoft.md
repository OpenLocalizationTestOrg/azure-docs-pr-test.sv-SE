---
title: aaaProblems inloggning tooa Microsoft-program | Microsoft Docs
description: "Felsöka vanliga problem med står inför när du loggar in toofirst parts Microsoft Applications med Azure AD (till exempel Office 365)"
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
ms.openlocfilehash: 35849ca8dbaa909d17b6d0da572f5c11041a8559
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
## <a name="problems-signing-in-tooa-microsoft-application"></a><span data-ttu-id="36e84-103">Problem med att logga i tooa Microsoft-program</span><span class="sxs-lookup"><span data-stu-id="36e84-103">Problems signing in tooa Microsoft application</span></span>

<span data-ttu-id="36e84-104">Microsoft Applications (till exempel Office 365 Exchange, SharePoint, Yammer, etc.) tilldelas och hanteras annorlunda än 3 part SaaS-program eller andra program som du har integrerat med Azure AD för enkel inloggning på.</span><span class="sxs-lookup"><span data-stu-id="36e84-104">Microsoft Applications (like Office 365 Exchange, SharePoint, Yammer, etc.) are assigned and managed a bit differently than 3rd party SaaS applications or other applications you integrate with Azure AD for single sign on.</span></span>

<span data-ttu-id="36e84-105">Det finns tre sätt som en användare kan få åtkomst tooa Microsoft publicerade program.</span><span class="sxs-lookup"><span data-stu-id="36e84-105">There are three main ways that a user can get access tooa Microsoft-published application.</span></span>

-   <span data-ttu-id="36e84-106">För program i hello Office 365 eller andra betald paket användare som beviljas åtkomst via **licens tilldelning** antingen direkt tootheir användarkonto, eller via en grupp med vår gruppbaserade licens tilldelning-funktionen.</span><span class="sxs-lookup"><span data-stu-id="36e84-106">For applications in hello Office 365 or other paid suites, users are granted access through **license assignment** either directly tootheir user account, or through a group using our group-based license assignment capability.</span></span>

-   <span data-ttu-id="36e84-107">För program som Microsoft eller tredje part publicerar fritt för alla toouse användare beviljas åtkomst via **användargodkännande**.</span><span class="sxs-lookup"><span data-stu-id="36e84-107">For applications that Microsoft or a Third Party publishes freely for anyone toouse, users may be granted access through **user consent**.</span></span> <span data-ttu-id="36e84-108">This0 innebär att de loggar in toohello program med Azure AD arbets- eller Skol-konto och Tillåt toohave åtkomst toosome begränsad uppsättning data på sitt konto.</span><span class="sxs-lookup"><span data-stu-id="36e84-108">This0 means that they sign in toohello application with their Azure AD Work or School account and allow it toohave access toosome limited set of data on their account.</span></span>

-   <span data-ttu-id="36e84-109">För program som Microsoft eller 3 part publicerar fritt för alla toouse användare även beviljas åtkomst via **administratör medgivande**.</span><span class="sxs-lookup"><span data-stu-id="36e84-109">For applications that Microsoft or a 3rd Party publishes freely for anyone toouse, users may also be granted access through **administrator consent**.</span></span> <span data-ttu-id="36e84-110">Det innebär att en administratör har fastställt hello program kan användas av alla i hello organisation, så att de loggar in toohello program med ett globalt administratörskonto och bevilja åtkomst tooeveryone i hello organisation.</span><span class="sxs-lookup"><span data-stu-id="36e84-110">This means that an administrator has determined hello application may be used by everyone in hello organization, so they sign in toohello application with a Global Administrator account and grant access tooeveryone in hello organization.</span></span>

<span data-ttu-id="36e84-111">tootroubleshoot problemet, börja med hello [allmänna problemområden med programåtkomst tooconsider](#general-problem-areas-with-application-access-to-consider) och läsa hello [genomgång: steg tootroubleshoot Microsoft Application åtkomst](#walkthrough-steps-to-troubleshoot-microsoft-application-access) tooget hello detaljer.</span><span class="sxs-lookup"><span data-stu-id="36e84-111">tootroubleshoot your issue, start with hello [General Problem Areas with Application Access tooconsider](#general-problem-areas-with-application-access-to-consider) and then read hello [Walkthrough: Steps tootroubleshoot Microsoft Application access](#walkthrough-steps-to-troubleshoot-microsoft-application-access) tooget into hello details.</span></span>

## <a name="general-problem-areas-with-application-access-tooconsider"></a><span data-ttu-id="36e84-112">Allmänna problemområden med programåtkomst tooconsider</span><span class="sxs-lookup"><span data-stu-id="36e84-112">General Problem Areas with Application Access tooconsider</span></span>

<span data-ttu-id="36e84-113">Nedan visas en lista över hello allmänna problemområden detaljer om om du har en uppfattning om där toostart, men vi rekommenderar att du läsa hello genomgången tooget igång snabbare: [genomgång: steg tootroubleshoot Microsoft Application åtkomst](#walkthrough-steps-to-troubleshoot-microsoft-application-access).</span><span class="sxs-lookup"><span data-stu-id="36e84-113">Below is a list of hello general problem areas that you can drill into if you have an idea of where toostart, but we recommend you read hello walkthrough tooget going quickly: [Walkthrough: Steps tootroubleshoot Microsoft Application access](#walkthrough-steps-to-troubleshoot-microsoft-application-access).</span></span>

-   [<span data-ttu-id="36e84-114">Problem med hello användarkonto</span><span class="sxs-lookup"><span data-stu-id="36e84-114">Problems with hello user’s account</span></span>](#problems-with-the-users-account)

-   [<span data-ttu-id="36e84-115">Problem med grupper</span><span class="sxs-lookup"><span data-stu-id="36e84-115">Problems with groups</span></span>](#problems-with-groups)

-   [<span data-ttu-id="36e84-116">Problem med principer för villkorlig åtkomst</span><span class="sxs-lookup"><span data-stu-id="36e84-116">Problems with conditional access policies</span></span>](#problems-with-conditional-access-policies)

-   [<span data-ttu-id="36e84-117">Problem med programmet medgivande</span><span class="sxs-lookup"><span data-stu-id="36e84-117">Problems with application consent</span></span>](#problems-with-application-consent)

## <a name="steps-tootroubleshoot-microsoft-application-access"></a><span data-ttu-id="36e84-118">Steg tootroubleshoot Microsoft Application åtkomst</span><span class="sxs-lookup"><span data-stu-id="36e84-118">Steps tootroubleshoot Microsoft Application access</span></span>

<span data-ttu-id="36e84-119">Nedan är några vanliga problem avdelningen stöter på när användarna inte logga in tooa Microsoft-program.</span><span class="sxs-lookup"><span data-stu-id="36e84-119">Below are some common issues folks run into when their users cannot sign in tooa Microsoft application.</span></span>

-   <span data-ttu-id="36e84-120">Allmänna problem toocheck först</span><span class="sxs-lookup"><span data-stu-id="36e84-120">General issues toocheck first</span></span>

  * <span data-ttu-id="36e84-121">Kontrollera att hello användaren loggar in toohello **korrigera URL** och inte en lokal programmets URL.</span><span class="sxs-lookup"><span data-stu-id="36e84-121">Make sure hello user is signing in toohello **correct URL** and not a local application URL.</span></span>

  * <span data-ttu-id="36e84-122">Kontrollera att hello användarens konto är **inte låst.**</span><span class="sxs-lookup"><span data-stu-id="36e84-122">Make sure hello user’s account is **not locked out.**</span></span>

  * <span data-ttu-id="36e84-123">Se till att hello **användarkontot finns** i Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="36e84-123">Make sure hello **user’s account exists** in Azure Active Directory.</span></span> [<span data-ttu-id="36e84-124">Kontrollera om det finns ett konto i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="36e84-124">Check if a user account exists in Azure Active Directory</span></span>](#problems-with-the-users-account)

  * <span data-ttu-id="36e84-125">Se till att hello användarkonto **aktiverat** för inloggningar. [Kontrollera status för en användare](#problems-with-the-users-account)</span><span class="sxs-lookup"><span data-stu-id="36e84-125">Make sure hello user’s account is **enabled** for sign ins. [Check a user’s account status](#problems-with-the-users-account)</span></span>

  * <span data-ttu-id="36e84-126">Se till att hello användarens **lösenord inte har upphört att gälla eller har glömt.**</span><span class="sxs-lookup"><span data-stu-id="36e84-126">Make sure hello user’s **password is not expired or forgotten.**</span></span> <span data-ttu-id="36e84-127">[Återställa en användares lösenord](#reset-a-users-password) eller [aktivera lösenordsåterställning via självbetjäning](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started)</span><span class="sxs-lookup"><span data-stu-id="36e84-127">[Reset a user’s password](#reset-a-users-password) or [Enable self-service password reset](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started)</span></span>

   * <span data-ttu-id="36e84-128">Kontrollera att **Multifaktorautentisering** inte blockerar åtkomst.</span><span class="sxs-lookup"><span data-stu-id="36e84-128">Make sure **Multi-Factor Authentication** is not blocking user access.</span></span> <span data-ttu-id="36e84-129">[Kontrollera status för en användares multifaktorautentisering](#check-a-users-multi-factor-authentication-status) eller [Kontrollera en användares kontaktinformation för autentisering](#check-a-users-authentication-contact-info)</span><span class="sxs-lookup"><span data-stu-id="36e84-129">[Check a user’s multi-factor authentication status](#check-a-users-multi-factor-authentication-status) or [Check a user’s authentication contact info](#check-a-users-authentication-contact-info)</span></span>

   * <span data-ttu-id="36e84-130">Kontrollera att en **principen för villkorlig åtkomst** eller **identitetsskydd** principen inte blockerar åtkomst.</span><span class="sxs-lookup"><span data-stu-id="36e84-130">Make sure a **Conditional Access policy** or **Identity Protection** policy is not blocking user access.</span></span> <span data-ttu-id="36e84-131">[Kontrollera en viss villkorlig åtkomstprincip](#problems-with-conditional-access-policies) eller [Kontrollera principen för villkorlig åtkomst för ett visst program](#check-a-specific-applications-conditional-access-policy) eller [inaktivera en princip för specifik villkorlig åtkomst](#disable-a-specific-conditional-access-policy)</span><span class="sxs-lookup"><span data-stu-id="36e84-131">[Check a specific conditional access policy](#problems-with-conditional-access-policies) or [Check a specific application’s conditional access policy](#check-a-specific-applications-conditional-access-policy) or [Disable a specific conditional access policy](#disable-a-specific-conditional-access-policy)</span></span>

   * <span data-ttu-id="36e84-132">Se till att en användares **autentisering kontaktinformation** fungerar toodate tooallow Multifaktorautentisering eller villkorlig åtkomst principer toobe tillämpas.</span><span class="sxs-lookup"><span data-stu-id="36e84-132">Make sure that a user’s **authentication contact info** is up toodate tooallow Multi-Factor Authentication or Conditional Access policies toobe enforced.</span></span> <span data-ttu-id="36e84-133">[Kontrollera status för en användares multifaktorautentisering](#check-a-users-multi-factor-authentication-status) eller [Kontrollera en användares kontaktinformation för autentisering](#check-a-users-authentication-contact-info)</span><span class="sxs-lookup"><span data-stu-id="36e84-133">[Check a user’s multi-factor authentication status](#check-a-users-multi-factor-authentication-status) or [Check a user’s authentication contact info](#check-a-users-authentication-contact-info)</span></span>

-   <span data-ttu-id="36e84-134">För **Microsoft** **program som kräver en licens** (till exempel Office 365) sparas följer toocheck vissa specifika problem när du har konstaterat hello allmänna problem ovan:</span><span class="sxs-lookup"><span data-stu-id="36e84-134">For **Microsoft** **applications that require a license** (like Office365), here are some specific issues toocheck once you have ruled out hello general issues above:</span></span>

   * <span data-ttu-id="36e84-135">Se till att användaren hello eller har en **-licens.**</span><span class="sxs-lookup"><span data-stu-id="36e84-135">Ensure hello user or has a **license assigned.**</span></span> <span data-ttu-id="36e84-136">[Kontrollera en användares tilldelade licenser](#check-a-users-assigned-licenses) eller [Kontrollera tilldelade licenser för en grupp](#check-a-groups-assigned-licenses)</span><span class="sxs-lookup"><span data-stu-id="36e84-136">[Check a user’s assigned licenses](#check-a-users-assigned-licenses) or [Check a group’s assigned licenses](#check-a-groups-assigned-licenses)</span></span>

   * <span data-ttu-id="36e84-137">Om hello licens **tilldelade tooa** **statisk grupp**, se till att hello **användaren är medlem** i gruppen.</span><span class="sxs-lookup"><span data-stu-id="36e84-137">If hello license is **assigned tooa** **static group**, ensure that hello **user is a member** of that group.</span></span> [<span data-ttu-id="36e84-138">Kontrollera en användares gruppmedlemskap</span><span class="sxs-lookup"><span data-stu-id="36e84-138">Check a user’s group memberships</span></span>](#check-a-users-group-memberships)

   * <span data-ttu-id="36e84-139">Om hello licens **tilldelade tooa** **dynamisk grupp**, se till att hello **dynamisk gruppregeln är korrekt**.</span><span class="sxs-lookup"><span data-stu-id="36e84-139">If hello license is **assigned tooa** **dynamic group**, ensure that hello **dynamic group rule is set correctly**.</span></span> [<span data-ttu-id="36e84-140">Kontrollera kriterier för medlemskap i en dynamisk grupp</span><span class="sxs-lookup"><span data-stu-id="36e84-140">Check a dynamic group’s membership criteria</span></span>](#check-a-dynamic-groups-membership-criteria)

   * <span data-ttu-id="36e84-141">Om hello licens **tilldelade tooa** **dynamisk grupp**, se till att den dynamiska gruppen hello har **har behandlats klart** sitt medlemskap och den hello **användaren är en medlemmen** (Detta kan ta lite tid).</span><span class="sxs-lookup"><span data-stu-id="36e84-141">If hello license is **assigned tooa** **dynamic group**, ensure that hello dynamic group has **finished processing** its membership and that hello **user is a member** (this can take some time).</span></span> [<span data-ttu-id="36e84-142">Kontrollera en användares gruppmedlemskap</span><span class="sxs-lookup"><span data-stu-id="36e84-142">Check a user’s group memberships</span></span>](#check-a-users-group-memberships)

   *  <span data-ttu-id="36e84-143">När du kontrollera hello tilldelas Kontrollera hello licens **inte gått**.</span><span class="sxs-lookup"><span data-stu-id="36e84-143">Once you make sure hello license is assigned, make sure hello license is **not expired**.</span></span>

   *  <span data-ttu-id="36e84-144">Kontrollera att hello licens **för programmet hello** de använder.</span><span class="sxs-lookup"><span data-stu-id="36e84-144">Make sure hello license is **for hello application** they are accessing.</span></span>

-   <span data-ttu-id="36e84-145">För **Microsoft** **program som inte kräver en licens**, här är några andra saker toocheck:</span><span class="sxs-lookup"><span data-stu-id="36e84-145">For **Microsoft** **applications that don’t require a license**, here are some other things toocheck:</span></span>

   * <span data-ttu-id="36e84-146">Om programmet hello begär **behörigheter på objektnivå** (till exempel ”komma åt den här användarens postlåda”), kontrollera hello användaren har loggat in toohello program och har utfört en **användarnivå medgivande åtgärden**  toolet hello programmet åtkomst till sina data.</span><span class="sxs-lookup"><span data-stu-id="36e84-146">If hello application is requesting **user-level permissions** (for example “Access this user’s mailbox”), make sure that hello user has signed in toohello application and has performed a **user-level consent operation** toolet hello application access her data.</span></span>

   * <span data-ttu-id="36e84-147">Om programmet hello begär **på administratörsnivå** (till exempel ”åt postlådor för alla användare”), se till att en Global administratör har genomfört en **administratörsnivå medgivande åtgärden på räkning av alla användare** i hello organisation.</span><span class="sxs-lookup"><span data-stu-id="36e84-147">If hello application is requesting **administrator-level permissions** (for example “Access all user’s mailboxes”), make sure that a Global Administrator has performed an **administrator-level consent operation on behalf of all users** in hello organization.</span></span>

## <a name="problems-with-hello-users-account"></a><span data-ttu-id="36e84-148">Problem med hello användarkonto</span><span class="sxs-lookup"><span data-stu-id="36e84-148">Problems with hello user’s account</span></span>

<span data-ttu-id="36e84-149">Programåtkomst blockeras på grund av tooa problem med en användare som har tilldelats toohello program.</span><span class="sxs-lookup"><span data-stu-id="36e84-149">Application access can be blocked due tooa problem with a user that is assigned toohello application.</span></span> <span data-ttu-id="36e84-150">Här följer några metoder som du kan felsöka och lösa problem med användare och deras kontoinställningar:</span><span class="sxs-lookup"><span data-stu-id="36e84-150">Below are some ways you can troubleshoot and solve problems with users and their account settings:</span></span>

-   [<span data-ttu-id="36e84-151">Kontrollera om det finns ett konto i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="36e84-151">Check if a user account exists in Azure Active Directory</span></span>](#check-if-a-user-account-exists-in-azure-active-directory)

-   [<span data-ttu-id="36e84-152">Kontrollera status för en användare</span><span class="sxs-lookup"><span data-stu-id="36e84-152">Check a user’s account status</span></span>](#check-a-users-account-status)

-   [<span data-ttu-id="36e84-153">Återställa en användares lösenord</span><span class="sxs-lookup"><span data-stu-id="36e84-153">Reset a user’s password</span></span>](#reset-a-users-password)

-   [<span data-ttu-id="36e84-154">Aktivera lösenordsåterställning via självbetjäning</span><span class="sxs-lookup"><span data-stu-id="36e84-154">Enable self-service password reset</span></span>](#enable-self-service-password-reset)

-   [<span data-ttu-id="36e84-155">Kontrollera status för multifaktorautentisering för en användare</span><span class="sxs-lookup"><span data-stu-id="36e84-155">Check a user’s multi-factor authentication status</span></span>](#check-a-users-multi-factor-authentication-status)

-   [<span data-ttu-id="36e84-156">Kontrollera en användares kontaktinformation för autentisering</span><span class="sxs-lookup"><span data-stu-id="36e84-156">Check a user’s authentication contact info</span></span>](#check-a-users-authentication-contact-info)

-   [<span data-ttu-id="36e84-157">Kontrollera en användares gruppmedlemskap</span><span class="sxs-lookup"><span data-stu-id="36e84-157">Check a user’s group memberships</span></span>](#check-a-users-group-memberships)

-   [<span data-ttu-id="36e84-158">Kontrollera en användares tilldelade licenser</span><span class="sxs-lookup"><span data-stu-id="36e84-158">Check a user’s assigned licenses</span></span>](#check-a-users-assigned-licenses)

-   [<span data-ttu-id="36e84-159">Tilldela en användare en licens</span><span class="sxs-lookup"><span data-stu-id="36e84-159">Assign a user a license</span></span>](#assign-a-user-a-license)

### <a name="check-if-a-user-account-exists-in-azure-active-directory"></a><span data-ttu-id="36e84-160">Kontrollera om det finns ett konto i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="36e84-160">Check if a user account exists in Azure Active Directory</span></span>

<span data-ttu-id="36e84-161">toocheck om användarkontot finns, så hello nedan:</span><span class="sxs-lookup"><span data-stu-id="36e84-161">toocheck if a user’s account is present, follow hello steps below:</span></span>

1.  <span data-ttu-id="36e84-162">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="36e84-162">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="36e84-163">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="36e84-163">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="36e84-164">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="36e84-164">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="36e84-165">Klicka på **användare och grupper** i hello navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="36e84-165">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="36e84-166">Klicka på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="36e84-166">click **All users**.</span></span>

6.  <span data-ttu-id="36e84-167">**Sök** för hello-användare som du är intresserad av och **klickar du på raden hello** tooselect.</span><span class="sxs-lookup"><span data-stu-id="36e84-167">**Search** for hello user you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="36e84-168">Kontrollera hello egenskaper för hello användaren objektet toobe till att de ser ut som förväntat och inga data saknas.</span><span class="sxs-lookup"><span data-stu-id="36e84-168">Check hello properties of hello user object toobe sure that they look as you expect and no data is missing.</span></span>

### <a name="check-a-users-account-status"></a><span data-ttu-id="36e84-169">Kontrollera status för en användare</span><span class="sxs-lookup"><span data-stu-id="36e84-169">Check a user’s account status</span></span>

<span data-ttu-id="36e84-170">toocheck en användare konto status, hello följande sätt:</span><span class="sxs-lookup"><span data-stu-id="36e84-170">toocheck a user’s account status, follow hello steps below:</span></span>

1.  <span data-ttu-id="36e84-171">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="36e84-171">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="36e84-172">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="36e84-172">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="36e84-173">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="36e84-173">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="36e84-174">Klicka på **användare och grupper** i hello navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="36e84-174">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="36e84-175">Klicka på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="36e84-175">click **All users**.</span></span>

6.  <span data-ttu-id="36e84-176">**Sök** för hello-användare som du är intresserad av och **klickar du på raden hello** tooselect.</span><span class="sxs-lookup"><span data-stu-id="36e84-176">**Search** for hello user you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="36e84-177">Klicka på **profil**.</span><span class="sxs-lookup"><span data-stu-id="36e84-177">click **Profile**.</span></span>

8.  <span data-ttu-id="36e84-178">Under **inställningar** se till att **Block logga in** har angetts för**nr**.</span><span class="sxs-lookup"><span data-stu-id="36e84-178">Under **Settings** ensure that **Block sign in** is set too**No**.</span></span>

### <a name="reset-a-users-password"></a><span data-ttu-id="36e84-179">Återställa en användares lösenord</span><span class="sxs-lookup"><span data-stu-id="36e84-179">Reset a user’s password</span></span>

<span data-ttu-id="36e84-180">tooreset en användares lösenord, följ hello stegen nedan:</span><span class="sxs-lookup"><span data-stu-id="36e84-180">tooreset a user’s password, follow hello steps below:</span></span>

1.  <span data-ttu-id="36e84-181">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="36e84-181">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="36e84-182">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="36e84-182">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="36e84-183">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="36e84-183">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="36e84-184">Klicka på **användare och grupper** i hello navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="36e84-184">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="36e84-185">Klicka på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="36e84-185">click **All users**.</span></span>

6.  <span data-ttu-id="36e84-186">**Sök** för hello-användare som du är intresserad av och **klickar du på raden hello** tooselect.</span><span class="sxs-lookup"><span data-stu-id="36e84-186">**Search** for hello user you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="36e84-187">Klicka på hello **Återställ lösenord** knappen hello överst på bladet för hello användare.</span><span class="sxs-lookup"><span data-stu-id="36e84-187">click hello **Reset password** button at hello top of hello user blade.</span></span>

8.  <span data-ttu-id="36e84-188">Klicka på hello **Återställ lösenord** hello-knappen **Återställ lösenord** bladet som visas.</span><span class="sxs-lookup"><span data-stu-id="36e84-188">click hello **Reset password** button on hello **Reset password** blade that appears.</span></span>

9.  <span data-ttu-id="36e84-189">Kopiera hello **tillfälligt lösenord** eller **ange ett nytt lösenord** för hello användare.</span><span class="sxs-lookup"><span data-stu-id="36e84-189">Copy hello **temporary password** or **enter a new password** for hello user.</span></span>

10. <span data-ttu-id="36e84-190">Meddela den nya lösenord toohello användaren, de nödvändiga toochange lösenordet vid nästa inloggning tooAzure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="36e84-190">Communicate this new password toohello user, they be required toochange this password during their next sign in tooAzure Active Directory.</span></span>

### <a name="enable-self-service-password-reset"></a><span data-ttu-id="36e84-191">Aktivera lösenordsåterställning via självbetjäning</span><span class="sxs-lookup"><span data-stu-id="36e84-191">Enable self-service password reset</span></span>

<span data-ttu-id="36e84-192">tooenable självbetjäning lösenord återställa, hello distribution av följande sätt:</span><span class="sxs-lookup"><span data-stu-id="36e84-192">tooenable self-service password reset, follow hello deployment steps below:</span></span>

-   [<span data-ttu-id="36e84-193">Aktivera användare tooreset sina Azure Active Directory-lösenord</span><span class="sxs-lookup"><span data-stu-id="36e84-193">Enable users tooreset their Azure Active Directory passwords</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started#enable-users-to-reset-their-azure-ad-passwords)

-   [<span data-ttu-id="36e84-194">Aktivera användare tooreset eller ändra sina lokala Active Directory-lösenord</span><span class="sxs-lookup"><span data-stu-id="36e84-194">Enable users tooreset or change their Active Directory on-premises passwords</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started#enable-users-to-reset-or-change-their-ad-passwords)

### <a name="check-a-users-multi-factor-authentication-status"></a><span data-ttu-id="36e84-195">Kontrollera status för multifaktorautentisering för en användare</span><span class="sxs-lookup"><span data-stu-id="36e84-195">Check a user’s multi-factor authentication status</span></span>

<span data-ttu-id="36e84-196">toocheck en användare har Multi-Factor authentication status, följ hello stegen nedan:</span><span class="sxs-lookup"><span data-stu-id="36e84-196">toocheck a user’s multi-factor authentication status, follow hello steps below:</span></span>

1.  <span data-ttu-id="36e84-197">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="36e84-197">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="36e84-198">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="36e84-198">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="36e84-199">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="36e84-199">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="36e84-200">Klicka på **användare och grupper** i hello navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="36e84-200">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="36e84-201">Klicka på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="36e84-201">click **All users**.</span></span>

6.  <span data-ttu-id="36e84-202">Klicka på hello **Multifaktorautentisering** knappen hello överst i hello-bladet.</span><span class="sxs-lookup"><span data-stu-id="36e84-202">click hello **Multi-Factor Authentication** button at hello top of hello blade.</span></span>

7.  <span data-ttu-id="36e84-203">En gång hello **Multi-Factor Authentication-administrationsportalen** belastning, kontrollera att du är på hello **användare** fliken.</span><span class="sxs-lookup"><span data-stu-id="36e84-203">Once hello **Multi-Factor Authentication Administration Portal** loads, ensure you are on hello **Users** tab.</span></span>

8.  <span data-ttu-id="36e84-204">Hitta hello användaren i hello lista över användare genom att söka, filtrering och sortering.</span><span class="sxs-lookup"><span data-stu-id="36e84-204">Find hello user in hello list of users by searching, filtering, or sorting.</span></span>

9.  <span data-ttu-id="36e84-205">Välj hello användaren hello listan över användare och **aktivera**, **inaktivera**, eller **framtvinga** multifaktorautentisering efter behov.</span><span class="sxs-lookup"><span data-stu-id="36e84-205">Select hello user from hello list of users and **Enable**, **Disable**, or **Enforce** multi-factor authentication as desired.</span></span>

  * <span data-ttu-id="36e84-206">**Obs**: om en användare finns i en **tvingande** tillstånd, så kan du ange dem för**inaktiverad** tillfälligt toolet dem tillbaka till deras konto.</span><span class="sxs-lookup"><span data-stu-id="36e84-206">**Note**: If a user is in an **Enforced** state, you may set them too**Disabled** temporarily toolet them back into their account.</span></span> <span data-ttu-id="36e84-207">När de är tillbaka i sedan kan du ändra tillståndet för**aktiverad** igen toorequire dem toore-register kontaktuppgifter under nästa inloggning.</span><span class="sxs-lookup"><span data-stu-id="36e84-207">Once they are back in, you can then change their state too**Enabled** again toorequire them toore-register their contact information during their next sign in.</span></span> <span data-ttu-id="36e84-208">Du kan också följa hello stegen i hello [Kontrollera en användares autentisering kontaktinformation](#check-a-users-authentication-contact-info) tooverify eller ange informationen för dessa.</span><span class="sxs-lookup"><span data-stu-id="36e84-208">Alternatively, you can follow hello steps in hello [Check a user’s authentication contact info](#check-a-users-authentication-contact-info) tooverify or set this data for them.</span></span>

### <a name="check-a-users-authentication-contact-info"></a><span data-ttu-id="36e84-209">Kontrollera en användares kontaktinformation för autentisering</span><span class="sxs-lookup"><span data-stu-id="36e84-209">Check a user’s authentication contact info</span></span>

<span data-ttu-id="36e84-210">toocheck en användarautentisering kontaktuppgifter som används för multifaktorautentisering, villkorlig åtkomst, identitetsskydd och återställning av lösenord gör hello nedan:</span><span class="sxs-lookup"><span data-stu-id="36e84-210">toocheck a user’s authentication contact info used for Multi-factor authentication, Conditional Access, Identity Protection, and Password Reset, follow hello steps below:</span></span>

1.  <span data-ttu-id="36e84-211">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="36e84-211">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="36e84-212">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="36e84-212">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="36e84-213">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="36e84-213">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="36e84-214">Klicka på **användare och grupper** i hello navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="36e84-214">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="36e84-215">Klicka på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="36e84-215">click **All users**.</span></span>

6.  <span data-ttu-id="36e84-216">**Sök** för hello-användare som du är intresserad av och **klickar du på raden hello** tooselect.</span><span class="sxs-lookup"><span data-stu-id="36e84-216">**Search** for hello user you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="36e84-217">Klicka på **profil**.</span><span class="sxs-lookup"><span data-stu-id="36e84-217">click **Profile**.</span></span>

8.  <span data-ttu-id="36e84-218">Rulla nedåt för**autentisering kontaktinformation**.</span><span class="sxs-lookup"><span data-stu-id="36e84-218">Scroll down too**Authentication contact info**.</span></span>

9.  <span data-ttu-id="36e84-219">**Granska** hello data som registrerats för hello användaren och uppdatera efter behov.</span><span class="sxs-lookup"><span data-stu-id="36e84-219">**Review** hello data registered for hello user and update as needed.</span></span>

### <a name="check-a-users-group-memberships"></a><span data-ttu-id="36e84-220">Kontrollera en användares gruppmedlemskap</span><span class="sxs-lookup"><span data-stu-id="36e84-220">Check a user’s group memberships</span></span>

<span data-ttu-id="36e84-221">toocheck en användare gruppmedlemskap, hello följande sätt:</span><span class="sxs-lookup"><span data-stu-id="36e84-221">toocheck a user’s group memberships, follow hello steps below:</span></span>

1.  <span data-ttu-id="36e84-222">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="36e84-222">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="36e84-223">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="36e84-223">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="36e84-224">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="36e84-224">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="36e84-225">Klicka på **användare och grupper** i hello navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="36e84-225">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="36e84-226">Klicka på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="36e84-226">click **All users**.</span></span>

6.  <span data-ttu-id="36e84-227">**Sök** för hello-användare som du är intresserad av och **klickar du på raden hello** tooselect.</span><span class="sxs-lookup"><span data-stu-id="36e84-227">**Search** for hello user you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="36e84-228">Klicka på **grupper** toosee vilka grupper hello användaren är medlem i.</span><span class="sxs-lookup"><span data-stu-id="36e84-228">click **Groups** toosee which groups hello user is a member of.</span></span>

### <a name="check-a-users-assigned-licenses"></a><span data-ttu-id="36e84-229">Kontrollera en användares tilldelade licenser</span><span class="sxs-lookup"><span data-stu-id="36e84-229">Check a user’s assigned licenses</span></span>

<span data-ttu-id="36e84-230">toocheck en användare tilldelade licenser, följ hello stegen nedan:</span><span class="sxs-lookup"><span data-stu-id="36e84-230">toocheck a user’s assigned licenses, follow hello steps below:</span></span>

1.  <span data-ttu-id="36e84-231">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="36e84-231">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="36e84-232">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="36e84-232">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="36e84-233">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="36e84-233">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="36e84-234">Klicka på **användare och grupper** i hello navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="36e84-234">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="36e84-235">Klicka på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="36e84-235">click **All users**.</span></span>

6.  <span data-ttu-id="36e84-236">**Sök** för hello-användare som du är intresserad av och **klickar du på raden hello** tooselect.</span><span class="sxs-lookup"><span data-stu-id="36e84-236">**Search** for hello user you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="36e84-237">Klicka på **licenser** toosee som licenser hello användare för närvarande har tilldelats.</span><span class="sxs-lookup"><span data-stu-id="36e84-237">click **Licenses** toosee which licenses hello user currently has assigned.</span></span>

### <a name="assign-a-user-a-license"></a><span data-ttu-id="36e84-238">Tilldela en användare en licens</span><span class="sxs-lookup"><span data-stu-id="36e84-238">Assign a user a license</span></span> 

<span data-ttu-id="36e84-239">tooassign en licens tooa användare gör hello nedan:</span><span class="sxs-lookup"><span data-stu-id="36e84-239">tooassign a license tooa user, follow hello steps below:</span></span>

1.  <span data-ttu-id="36e84-240">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="36e84-240">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="36e84-241">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="36e84-241">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="36e84-242">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="36e84-242">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="36e84-243">Klicka på **användare och grupper** i hello navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="36e84-243">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="36e84-244">Klicka på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="36e84-244">click **All users**.</span></span>

6.  <span data-ttu-id="36e84-245">**Sök** för hello-användare som du är intresserad av och **klickar du på raden hello** tooselect.</span><span class="sxs-lookup"><span data-stu-id="36e84-245">**Search** for hello user you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="36e84-246">Klicka på **licenser** toosee som licenser hello användare för närvarande har tilldelats.</span><span class="sxs-lookup"><span data-stu-id="36e84-246">click **Licenses** toosee which licenses hello user currently has assigned.</span></span>

8.  <span data-ttu-id="36e84-247">Klicka på hello **tilldela** knappen.</span><span class="sxs-lookup"><span data-stu-id="36e84-247">click hello **Assign** button.</span></span>

9.  <span data-ttu-id="36e84-248">Välj **en eller flera produkter** hello listan över tillgängliga produkter.</span><span class="sxs-lookup"><span data-stu-id="36e84-248">Select **one or more products** from hello list of available products.</span></span>

10. <span data-ttu-id="36e84-249">**Valfria** klickar du på hello **tilldelning alternativ** objektet toogranularly tilldela produkter.</span><span class="sxs-lookup"><span data-stu-id="36e84-249">**Optional** click hello **assignment options** item toogranularly assign products.</span></span> <span data-ttu-id="36e84-250">Klicka på **Ok** när detta har slutförts.</span><span class="sxs-lookup"><span data-stu-id="36e84-250">Click **Ok** when this is completed.</span></span>

11. <span data-ttu-id="36e84-251">Klicka på hello **tilldela** knappen tooassign dessa licenser toothis användare.</span><span class="sxs-lookup"><span data-stu-id="36e84-251">Click hello **Assign** button tooassign these licenses toothis user.</span></span>

## <a name="problems-with-groups"></a><span data-ttu-id="36e84-252">Problem med grupper</span><span class="sxs-lookup"><span data-stu-id="36e84-252">Problems with groups</span></span>

<span data-ttu-id="36e84-253">Programåtkomst blockeras på grund av tooa problem med en grupp som har tilldelats toohello program.</span><span class="sxs-lookup"><span data-stu-id="36e84-253">Application access can be blocked due tooa problem with a group that is assigned toohello application.</span></span> <span data-ttu-id="36e84-254">Här följer några metoder som du kan felsöka och lösa problem med grupper och gruppmedlemskap:</span><span class="sxs-lookup"><span data-stu-id="36e84-254">Below are some ways you can troubleshoot and solve problems with groups and group memberships:</span></span>

-   [<span data-ttu-id="36e84-255">Kontrollera medlemskapet för en grupp</span><span class="sxs-lookup"><span data-stu-id="36e84-255">Check a group’s membership</span></span>](#check-a-groups-membership)

-   [<span data-ttu-id="36e84-256">Kontrollera kriterier för medlemskap i en dynamisk grupp</span><span class="sxs-lookup"><span data-stu-id="36e84-256">Check a dynamic group’s membership criteria</span></span>](#check-a-dynamic-groups-membership-criteria)

-   [<span data-ttu-id="36e84-257">Kontrollera tilldelade licenser för en grupp</span><span class="sxs-lookup"><span data-stu-id="36e84-257">Check a group’s assigned licenses</span></span>](#check-a-groups-assigned-licenses)

-   [<span data-ttu-id="36e84-258">Ombearbeta licenser för en grupp</span><span class="sxs-lookup"><span data-stu-id="36e84-258">Reprocess a group’s licenses</span></span>](#reprocess-a-groups-licenses)

-   [<span data-ttu-id="36e84-259">Tilldela en licens för en grupp</span><span class="sxs-lookup"><span data-stu-id="36e84-259">Assign a group a license</span></span>](#assign-a-group-a-license)

### <a name="check-a-groups-membership"></a><span data-ttu-id="36e84-260">Kontrollera medlemskapet för en grupp</span><span class="sxs-lookup"><span data-stu-id="36e84-260">Check a group’s membership</span></span>

<span data-ttu-id="36e84-261">toocheck en grupps medlemskap, följ hello stegen nedan:</span><span class="sxs-lookup"><span data-stu-id="36e84-261">toocheck a group’s membership, follow hello steps below:</span></span>

1.  <span data-ttu-id="36e84-262">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="36e84-262">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="36e84-263">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="36e84-263">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="36e84-264">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="36e84-264">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="36e84-265">Klicka på **användare och grupper** i hello navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="36e84-265">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="36e84-266">Klicka på **alla grupper**.</span><span class="sxs-lookup"><span data-stu-id="36e84-266">click **All groups**.</span></span>

6.  <span data-ttu-id="36e84-267">**Sök** för hello-grupp som du är intresserad av och **klickar du på raden hello** tooselect.</span><span class="sxs-lookup"><span data-stu-id="36e84-267">**Search** for hello group you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="36e84-268">Klicka på **medlemmar** tooreview hello lista över användare som tilldelats toothis grupp.</span><span class="sxs-lookup"><span data-stu-id="36e84-268">click **Members** tooreview hello list of users assigned toothis group.</span></span>

### <a name="check-a-dynamic-groups-membership-criteria"></a><span data-ttu-id="36e84-269">Kontrollera kriterier för medlemskap i en dynamisk grupp</span><span class="sxs-lookup"><span data-stu-id="36e84-269">Check a dynamic group’s membership criteria</span></span> 

<span data-ttu-id="36e84-270">toocheck kriterier för medlemskap av en dynamisk grupp gör hello nedan:</span><span class="sxs-lookup"><span data-stu-id="36e84-270">toocheck a dynamic group’s membership criteria, follow hello steps below:</span></span>

1.  <span data-ttu-id="36e84-271">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="36e84-271">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="36e84-272">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="36e84-272">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="36e84-273">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="36e84-273">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="36e84-274">Klicka på **användare och grupper** i hello navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="36e84-274">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="36e84-275">Klicka på **alla grupper**.</span><span class="sxs-lookup"><span data-stu-id="36e84-275">click **All groups**.</span></span>

6.  <span data-ttu-id="36e84-276">**Sök** för hello-grupp som du är intresserad av och **klickar du på raden hello** tooselect.</span><span class="sxs-lookup"><span data-stu-id="36e84-276">**Search** for hello group you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="36e84-277">Klicka på **dynamiskt medlemskapsregler.**</span><span class="sxs-lookup"><span data-stu-id="36e84-277">click **Dynamic membership rules.**</span></span>

8.  <span data-ttu-id="36e84-278">Granska hello **enkel** eller **avancerade** regel definierad för den här gruppen och se till att hello-användare som du vill toobe medlem i den här gruppen uppfyller dessa villkor.</span><span class="sxs-lookup"><span data-stu-id="36e84-278">Review hello **simple** or **advanced** rule defined for this group and ensure that hello user you want toobe a member of this group meets these criteria.</span></span>

### <a name="check-a-groups-assigned-licenses"></a><span data-ttu-id="36e84-279">Kontrollera tilldelade licenser för en grupp</span><span class="sxs-lookup"><span data-stu-id="36e84-279">Check a group’s assigned licenses</span></span>

<span data-ttu-id="36e84-280">toocheck en grupp tilldelade licenser, följ hello stegen nedan:</span><span class="sxs-lookup"><span data-stu-id="36e84-280">toocheck a group’s assigned licenses, follow hello steps below:</span></span>

1.  <span data-ttu-id="36e84-281">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="36e84-281">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="36e84-282">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="36e84-282">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="36e84-283">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="36e84-283">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="36e84-284">Klicka på **användare och grupper** i hello navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="36e84-284">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="36e84-285">Klicka på **alla grupper**.</span><span class="sxs-lookup"><span data-stu-id="36e84-285">click **All groups**.</span></span>

6.  <span data-ttu-id="36e84-286">**Sök** för hello-grupp som du är intresserad av och **klickar du på raden hello** tooselect.</span><span class="sxs-lookup"><span data-stu-id="36e84-286">**Search** for hello group you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="36e84-287">Klicka på **licenser** toosee vilken licenser hello grupp för närvarande har tilldelats.</span><span class="sxs-lookup"><span data-stu-id="36e84-287">click **Licenses** toosee which licenses hello group currently has assigned.</span></span>

### <a name="reprocess-a-groups-licenses"></a><span data-ttu-id="36e84-288">Ombearbeta licenser för en grupp</span><span class="sxs-lookup"><span data-stu-id="36e84-288">Reprocess a group’s licenses</span></span>

<span data-ttu-id="36e84-289">tooreprocess en grupp tilldelade licenser, följ hello stegen nedan:</span><span class="sxs-lookup"><span data-stu-id="36e84-289">tooreprocess a group’s assigned licenses, follow hello steps below:</span></span>

1.  <span data-ttu-id="36e84-290">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="36e84-290">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="36e84-291">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="36e84-291">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="36e84-292">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="36e84-292">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="36e84-293">Klicka på **användare och grupper** i hello navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="36e84-293">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="36e84-294">Klicka på **alla grupper**.</span><span class="sxs-lookup"><span data-stu-id="36e84-294">click **All groups**.</span></span>

6.  <span data-ttu-id="36e84-295">**Sök** för hello-grupp som du är intresserad av och **klickar du på raden hello** tooselect.</span><span class="sxs-lookup"><span data-stu-id="36e84-295">**Search** for hello group you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="36e84-296">Klicka på **licenser** toosee vilken licenser hello grupp för närvarande har tilldelats.</span><span class="sxs-lookup"><span data-stu-id="36e84-296">click **Licenses** toosee which licenses hello group currently has assigned.</span></span>

8.  <span data-ttu-id="36e84-297">Klicka på hello **Ombearbeta** knappen tooensure som hello användarlicenser toothis gruppens medlemmar är uppdaterade.</span><span class="sxs-lookup"><span data-stu-id="36e84-297">click hello **Reprocess** button tooensure that hello licenses assigned toothis group’s members are up-to-date.</span></span> <span data-ttu-id="36e84-298">Det kan ta lång tid, beroende på hello storleken och komplexiteten för hello grupp.</span><span class="sxs-lookup"><span data-stu-id="36e84-298">This may take a long time, depending on hello size and complexity of hello group.</span></span>

   >[!NOTE]
   ><span data-ttu-id="36e84-299">toodo detta snabbare, Överväg att tillfälligt tilldela en licens toohello användare direkt.</span><span class="sxs-lookup"><span data-stu-id="36e84-299">toodo this faster, consider temporarily assigning a license toohello user directly.</span></span> <span data-ttu-id="36e84-300">[Tilldela en användare en licens](#problems-with-application-consent).</span><span class="sxs-lookup"><span data-stu-id="36e84-300">[Assign a user a license](#problems-with-application-consent).</span></span>
   >
   >

### <a name="assign-a-group-a-license"></a><span data-ttu-id="36e84-301">Tilldela en licens för en grupp</span><span class="sxs-lookup"><span data-stu-id="36e84-301">Assign a group a license</span></span>

<span data-ttu-id="36e84-302">tooassign tooa licensgrupp, så hello nedan:</span><span class="sxs-lookup"><span data-stu-id="36e84-302">tooassign a license tooa group, follow hello steps below:</span></span>

1.  <span data-ttu-id="36e84-303">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="36e84-303">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="36e84-304">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="36e84-304">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="36e84-305">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="36e84-305">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="36e84-306">Klicka på **användare och grupper** i hello navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="36e84-306">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="36e84-307">Klicka på **alla grupper**.</span><span class="sxs-lookup"><span data-stu-id="36e84-307">click **All groups**.</span></span>

6.  <span data-ttu-id="36e84-308">**Sök** för hello-grupp som du är intresserad av och **klickar du på raden hello** tooselect.</span><span class="sxs-lookup"><span data-stu-id="36e84-308">**Search** for hello group you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="36e84-309">Klicka på **licenser** toosee vilken licenser hello grupp för närvarande har tilldelats.</span><span class="sxs-lookup"><span data-stu-id="36e84-309">click **Licenses** toosee which licenses hello group currently has assigned.</span></span>

8.  <span data-ttu-id="36e84-310">Klicka på hello **tilldela** knappen.</span><span class="sxs-lookup"><span data-stu-id="36e84-310">click hello **Assign** button.</span></span>

9.  <span data-ttu-id="36e84-311">Välj **en eller flera produkter** hello listan över tillgängliga produkter.</span><span class="sxs-lookup"><span data-stu-id="36e84-311">Select **one or more products** from hello list of available products.</span></span>

10. <span data-ttu-id="36e84-312">**Valfria** klickar du på hello **tilldelning alternativ** objektet toogranularly tilldela produkter.</span><span class="sxs-lookup"><span data-stu-id="36e84-312">**Optional** click hello **assignment options** item toogranularly assign products.</span></span> <span data-ttu-id="36e84-313">Klicka på **Ok** när detta har slutförts.</span><span class="sxs-lookup"><span data-stu-id="36e84-313">Click **Ok** when this is completed.</span></span>

11. <span data-ttu-id="36e84-314">Klicka på hello **tilldela** knappen tooassign dessa licenser toothis grupp.</span><span class="sxs-lookup"><span data-stu-id="36e84-314">Click hello **Assign** button tooassign these licenses toothis group.</span></span> <span data-ttu-id="36e84-315">Det kan ta lång tid, beroende på hello storleken och komplexiteten för hello grupp.</span><span class="sxs-lookup"><span data-stu-id="36e84-315">This may take a long time, depending on hello size and complexity of hello group.</span></span>

   >[!NOTE]
   ><span data-ttu-id="36e84-316">toodo detta snabbare, Överväg att tillfälligt tilldela en licens toohello användare direkt.</span><span class="sxs-lookup"><span data-stu-id="36e84-316">toodo this faster, consider temporarily assigning a license toohello user directly.</span></span> <span data-ttu-id="36e84-317">[Tilldela en användare en licens](#problems-with-application-consent).</span><span class="sxs-lookup"><span data-stu-id="36e84-317">[Assign a user a license](#problems-with-application-consent).</span></span>
   > 
   >

## <a name="problems-with-conditional-access-policies"></a><span data-ttu-id="36e84-318">Problem med principer för villkorlig åtkomst</span><span class="sxs-lookup"><span data-stu-id="36e84-318">Problems with conditional access policies</span></span>

### <a name="check-a-specific-conditional-access-policy"></a><span data-ttu-id="36e84-319">Kontrollera en princip för specifik villkorlig åtkomst</span><span class="sxs-lookup"><span data-stu-id="36e84-319">Check a specific conditional access policy</span></span>

<span data-ttu-id="36e84-320">toocheck, eller validera en princip för enskild villkorlig åtkomst:</span><span class="sxs-lookup"><span data-stu-id="36e84-320">toocheck or validate a single conditional access policy:</span></span>

1.  <span data-ttu-id="36e84-321">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="36e84-321">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="36e84-322">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="36e84-322">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="36e84-323">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="36e84-323">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="36e84-324">Klicka på **företagsprogram** i hello navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="36e84-324">click **Enterprise applications** in hello navigation menu.</span></span>

5.  <span data-ttu-id="36e84-325">Klicka på hello **villkorlig åtkomst** navigeringsobjektet.</span><span class="sxs-lookup"><span data-stu-id="36e84-325">click hello **Conditional access** navigation item.</span></span>

6.  <span data-ttu-id="36e84-326">Klicka på hello princip som du är intresserad undersöks.</span><span class="sxs-lookup"><span data-stu-id="36e84-326">click hello policy you are interested in inspecting.</span></span>

7.  <span data-ttu-id="36e84-327">Granska att det finns inga särskilda villkor, tilldelningar och andra inställningar som kan blockera åtkomst.</span><span class="sxs-lookup"><span data-stu-id="36e84-327">Review that there are no specific conditions, assignments, or other settings which may be blocking user access.</span></span>

   >[!NOTE]
   ><span data-ttu-id="36e84-328">Du tootemporarily inaktivera den här principen tooensure den inte påverkar logga modulerna toodo detta, ange hello **aktiverar principen** växla för**nr** och klicka på hello **spara** knappen .</span><span class="sxs-lookup"><span data-stu-id="36e84-328">You may wish tootemporarily disable this policy tooensure it is not affecting sign ins. toodo this, set hello **Enable policy** toggle too**No** and click hello **Save** button.</span></span>
   >
   >

### <a name="check-a-specific-applications-conditional-access-policy"></a><span data-ttu-id="36e84-329">Kontrollera principen för villkorlig åtkomst för ett visst program</span><span class="sxs-lookup"><span data-stu-id="36e84-329">Check a specific application’s conditional access policy</span></span>

<span data-ttu-id="36e84-330">toocheck eller verifiera principen för ett enda program för tillfället konfigurerade villkorlig åtkomst:</span><span class="sxs-lookup"><span data-stu-id="36e84-330">toocheck or validate a single application’s currently configured conditional access policy:</span></span>

1.  <span data-ttu-id="36e84-331">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="36e84-331">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="36e84-332">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="36e84-332">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="36e84-333">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="36e84-333">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="36e84-334">Klicka på **företagsprogram** i hello navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="36e84-334">click **Enterprise applications** in hello navigation menu.</span></span>

5.  <span data-ttu-id="36e84-335">Klicka på **alla program**.</span><span class="sxs-lookup"><span data-stu-id="36e84-335">click **All applications**.</span></span>

6.  <span data-ttu-id="36e84-336">Sök Visa namn eller ett program-id för hello programmet du är intresserad av eller hello användaren försöker toosign i tooby program.</span><span class="sxs-lookup"><span data-stu-id="36e84-336">Search for hello application you are interested in, or hello user is attempting toosign in tooby application display name or application id.</span></span>

     >[!NOTE]
     ><span data-ttu-id="36e84-337">Om hello-program som du letar efter inte visas klickar du på hello **Filter** knappen och expandera hello omfattning hello lista för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="36e84-337">If you don’t see hello application you are looking for, click hello **Filter** button and expand hello scope of hello list too**All applications**.</span></span> <span data-ttu-id="36e84-338">Om du vill toosee fler kolumner, klickar du på hello **kolumner** knappen tooadd ytterligare information om dina program.</span><span class="sxs-lookup"><span data-stu-id="36e84-338">If you want toosee more columns, click hello **Columns** button tooadd additional details for your applications.</span></span>
     >
     >

7.  <span data-ttu-id="36e84-339">Klicka på hello **villkorlig åtkomst** navigeringsobjektet.</span><span class="sxs-lookup"><span data-stu-id="36e84-339">click hello **Conditional access** navigation item.</span></span>

8.  <span data-ttu-id="36e84-340">Klicka på hello princip som du är intresserad undersöks.</span><span class="sxs-lookup"><span data-stu-id="36e84-340">click hello policy you are interested in inspecting.</span></span>

9.  <span data-ttu-id="36e84-341">Granska att det finns inga särskilda villkor, tilldelningar och andra inställningar som kan blockera åtkomst.</span><span class="sxs-lookup"><span data-stu-id="36e84-341">Review that there are no specific conditions, assignments, or other settings which may be blocking user access.</span></span>

     >[!NOTE]
     ><span data-ttu-id="36e84-342">Du tootemporarily inaktivera den här principen tooensure den inte påverkar logga modulerna toodo detta, ange hello **aktiverar principen** växla för**nr** och klicka på hello **spara** knappen .</span><span class="sxs-lookup"><span data-stu-id="36e84-342">You may wish tootemporarily disable this policy tooensure it is not affecting sign ins. toodo this, set hello **Enable policy** toggle too**No** and click hello **Save** button.</span></span>
     >
     >

### <a name="disable-a-specific-conditional-access-policy"></a><span data-ttu-id="36e84-343">Inaktivera en princip för specifik villkorlig åtkomst</span><span class="sxs-lookup"><span data-stu-id="36e84-343">Disable a specific conditional access policy</span></span>

<span data-ttu-id="36e84-344">toocheck, eller validera en princip för enskild villkorlig åtkomst:</span><span class="sxs-lookup"><span data-stu-id="36e84-344">toocheck or validate a single conditional access policy:</span></span>

1.  <span data-ttu-id="36e84-345">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="36e84-345">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="36e84-346">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="36e84-346">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="36e84-347">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="36e84-347">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="36e84-348">Klicka på **företagsprogram** i hello navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="36e84-348">click **Enterprise applications** in hello navigation menu.</span></span>

5.  <span data-ttu-id="36e84-349">Klicka på hello **villkorlig åtkomst** navigeringsobjektet.</span><span class="sxs-lookup"><span data-stu-id="36e84-349">click hello **Conditional access** navigation item.</span></span>

6.  <span data-ttu-id="36e84-350">Klicka på hello princip som du är intresserad undersöks.</span><span class="sxs-lookup"><span data-stu-id="36e84-350">click hello policy you are interested in inspecting.</span></span>

7.  <span data-ttu-id="36e84-351">Inaktivera hello princip genom att ange hello **aktiverar principen** växla för**nr** och klicka på hello **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="36e84-351">Disable hello policy by setting hello **Enable policy** toggle too**No** and click hello **Save** button.</span></span>

## <a name="problems-with-application-consent"></a><span data-ttu-id="36e84-352">Problem med programmet medgivande</span><span class="sxs-lookup"><span data-stu-id="36e84-352">Problems with application consent</span></span>

<span data-ttu-id="36e84-353">Programåtkomst blockeras eftersom hello åtkomstbehörighet medgivande åtgärden inte har inträffat.</span><span class="sxs-lookup"><span data-stu-id="36e84-353">Application access can be blocked because hello proper permissions consent operation has not occurred.</span></span> <span data-ttu-id="36e84-354">Här följer några metoder som du kan felsöka och lösa problem med programmet medgivande:</span><span class="sxs-lookup"><span data-stu-id="36e84-354">Below are some ways you can troubleshoot and solve application consent issues:</span></span>

-   [<span data-ttu-id="36e84-355">Utföra en åtgärd på användarnivå medgivande</span><span class="sxs-lookup"><span data-stu-id="36e84-355">Perform a user-level consent operation</span></span>](#perform-a-user-level-consent-operation)

-   [<span data-ttu-id="36e84-356">Utföra administratörsnivå medgivande åtgärden för alla program</span><span class="sxs-lookup"><span data-stu-id="36e84-356">Perform administrator-level consent operation for any application</span></span>](#perform-administrator-level-consent-operation-for-any-application)

-   [<span data-ttu-id="36e84-357">Utföra administratörsnivå medgivande för en enskild klient-program</span><span class="sxs-lookup"><span data-stu-id="36e84-357">Perform administrator-level consent for a single-tenant application</span></span>](#perform-administrator-level-consent-for-a-single-tenant-application)

-   [<span data-ttu-id="36e84-358">Utföra administratörsnivå medgivande för ett program med flera innehavare</span><span class="sxs-lookup"><span data-stu-id="36e84-358">Perform administrator-level consent for a multi-tenant application</span></span>](#perform-administrator-level-consent-for-a-multi-tenant-application)

### <a name="perform-a-user-level-consent-operation"></a><span data-ttu-id="36e84-359">Utföra en åtgärd på användarnivå medgivande</span><span class="sxs-lookup"><span data-stu-id="36e84-359">Perform a user-level consent operation</span></span>

-   <span data-ttu-id="36e84-360">Alla öppna ID Connect-aktiverade program som begär behörighet, utför navigera toohello programmet inloggning skärmen en nivå medgivande toohello användarprogram för hello inloggad användare.</span><span class="sxs-lookup"><span data-stu-id="36e84-360">For any Open ID Connect-enabled application that requests permissions, navigating toohello application’s sign in screen performs a user level consent toohello application for hello signed-in user.</span></span>

-   <span data-ttu-id="36e84-361">Om du inte vill toodo detta programmässigt, se [begär enskilda användargodkännande](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#requesting-individual-user-consent).</span><span class="sxs-lookup"><span data-stu-id="36e84-361">If you wish toodo this programmatically, see [Requesting individual user consent](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#requesting-individual-user-consent).</span></span>

### <a name="perform-administrator-level-consent-operation-for-any-application"></a><span data-ttu-id="36e84-362">Utföra administratörsnivå medgivande åtgärden för alla program</span><span class="sxs-lookup"><span data-stu-id="36e84-362">Perform administrator-level consent operation for any application</span></span>

-   <span data-ttu-id="36e84-363">För **bara program som utvecklats med hello V1 programmodell**, du kan framtvinga den här administratören nivån medgivande toooccur genom att lägga till ”**? uppmana = admin\_medgivande**” toohello slutet av en programmets tecken i URL: en.</span><span class="sxs-lookup"><span data-stu-id="36e84-363">For **only applications developed using hello V1 application model**, you can force this administrator level consent toooccur by adding “**?prompt=admin\_consent**” toohello end of an application’s sign in URL.</span></span>

-   <span data-ttu-id="36e84-364">För **alla program som utvecklats med hello V2 programmodell**, kan du använda den här nivån administratör medgivande toooccur genom att följa instruktionerna hello under hello **begär hello behörighet från en katalog Admin** avsnitt i [med hello medgivande adminslutpunkten](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint).</span><span class="sxs-lookup"><span data-stu-id="36e84-364">For **any application developed using hello V2 application model**, you can enforce this administrator-level consent toooccur by following hello instructions under hello **Request hello permissions from a directory admin** section of [Using hello admin consent endpoint](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint).</span></span>

### <a name="perform-administrator-level-consent-for-a-single-tenant-application"></a><span data-ttu-id="36e84-365">Utföra administratörsnivå medgivande för en enskild klient-program</span><span class="sxs-lookup"><span data-stu-id="36e84-365">Perform administrator-level consent for a single-tenant application</span></span>

-   <span data-ttu-id="36e84-366">För **program enkel klienter** som begär behörighet (till exempel de som du utvecklar eller äger i din organisation), kan du utföra en **administrativ nivå medgivande** åtgärden för alla användare genom att logga in som Global administratör och klicka på hello **bevilja behörigheter** knappen hello överst i hello **programmet registret -&gt; alla program -&gt; Välj en App - &gt; Nödvändiga behörigheter** bladet.</span><span class="sxs-lookup"><span data-stu-id="36e84-366">For **single-tenant applications** that request permissions (like those you are developing or own in your organization), you can perform an **administrative-level consent** operation on behalf of all users by signing in as a Global Administrator and clicking on hello **Grant permissions** button at hello top of hello **Application Registry -&gt; All Applications -&gt; Select an App -&gt; Required Permissions** blade.</span></span>

-   <span data-ttu-id="36e84-367">För **alla program som utvecklats med hello V1 eller V2 programmodell**, kan du använda den här nivån administratör medgivande toooccur genom att följa instruktionerna hello under hello **begära hello behörigheter från en katalogadministratör** avsnitt i [med hello medgivande adminslutpunkten](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint).</span><span class="sxs-lookup"><span data-stu-id="36e84-367">For **any application developed using hello V1 or V2 application model**, you can enforce this administrator-level consent toooccur by following hello instructions under hello **Request hello permissions from a directory admin** section of [Using hello admin consent endpoint](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint).</span></span>

### <a name="perform-administrator-level-consent-for-a-multi-tenant-application"></a><span data-ttu-id="36e84-368">Utföra administratörsnivå medgivande för ett program med flera innehavare</span><span class="sxs-lookup"><span data-stu-id="36e84-368">Perform administrator-level consent for a multi-tenant application</span></span>

-   <span data-ttu-id="36e84-369">För **program med flera klienter** som begär behörighet (t.ex. ett program en tredje part eller Microsoft, utvecklar), kan du utföra en **administrativ nivå medgivande** igen.</span><span class="sxs-lookup"><span data-stu-id="36e84-369">For **multi-tenant applications** that request permissions (like an application a third party, or Microsoft, develops), you can perform an **administrative-level consent** operation.</span></span> <span data-ttu-id="36e84-370">Logga in som Global administratör och klicka på hello **bevilja behörigheter** knappen under hello **företagsprogram -&gt; alla program -&gt; Välj en App -&gt; Behörigheter** bladet (tillgänglig snart).</span><span class="sxs-lookup"><span data-stu-id="36e84-370">Sign in as a Global Administrator and clicking on hello **Grant permissions** button under hello **Enterprise Applications -&gt; All Applications -&gt; Select an App -&gt; Permissions** blade (available soon).</span></span>

-   <span data-ttu-id="36e84-371">Du kan även tillämpa den här nivån administratör medgivande toooccur genom att följa instruktionerna hello under hello **begär hello behörighet från en katalogadministratör** avsnitt i [med hello medgivande adminslutpunkten](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint).</span><span class="sxs-lookup"><span data-stu-id="36e84-371">You can also enforce this administrator-level consent toooccur by following hello instructions under hello **Request hello permissions from a directory admin** section of [Using hello admin consent endpoint](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint).</span></span>

## <a name="next-steps"></a><span data-ttu-id="36e84-372">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="36e84-372">Next steps</span></span>
[<span data-ttu-id="36e84-373">Med hjälp av hello admin medgivande-slutpunkten</span><span class="sxs-lookup"><span data-stu-id="36e84-373">Using hello admin consent endpoint</span></span>](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint)

