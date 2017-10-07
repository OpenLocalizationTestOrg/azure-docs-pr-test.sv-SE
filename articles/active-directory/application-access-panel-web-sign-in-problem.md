---
title: "aaaProblem inloggning toohello åtkomst panelen webbplats | Microsoft Docs"
description: "Vägledning tootroubleshoot problem som kan uppstå när toosign i toouse hello åtkomstpanelen"
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
ms.reviwer: japere
ms.openlocfilehash: 1037f7c5fbaa9425760ad5739b383c716d5fc3a3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="problem-signing-in-toohello-access-panel-website"></a><span data-ttu-id="c4b7b-103">Problem med att logga i toohello åtkomst panelen webbplats</span><span class="sxs-lookup"><span data-stu-id="c4b7b-103">Problem signing in toohello access panel website</span></span>

<span data-ttu-id="c4b7b-104">Hej åtkomstpanelen är en webbaserad portal som aktiverar en användare som har ett arbets- eller skolkonto konto i Azure Active Directory (AD Azure) tooview och starta molnbaserade program att hello Azure AD-administratör har gett dem åtkomst till.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-104">hello Access Panel is a web-based portal which enables a user who has a work or school account in Azure Active Directory (Azure AD) tooview and launch cloud-based applications that hello Azure AD administrator has granted them access to.</span></span> <span data-ttu-id="c4b7b-105">En användare med Azure AD-versioner kan också använda självbetjäning och funktioner för hantering av appen via hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-105">A user who has Azure AD editions can also use self-service group and app management capabilities through hello Access Panel.</span></span> <span data-ttu-id="c4b7b-106">Hej åtkomstpanelen skiljer sig från hello Azure-portalen och kräver inte användare toohave en Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-106">hello Access Panel is separate from hello Azure portal and does not require users toohave an Azure subscription.</span></span>

<span data-ttu-id="c4b7b-107">Användarna kan logga in toohello åtkomstpanelen om de har ett arbets- eller skolkonto konto i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-107">Users can sign in toohello Access Panel if they have a work or school account in Azure AD.</span></span>

-   <span data-ttu-id="c4b7b-108">Användare kan autentiseras av Azure AD direkt.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-108">Users can be authenticated by Azure AD directly.</span></span>

-   <span data-ttu-id="c4b7b-109">Användare kan autentiseras med hjälp av Active Directory Federation Services (AD FS).</span><span class="sxs-lookup"><span data-stu-id="c4b7b-109">Users can be authenticated by using Active Directory Federation Services (AD FS).</span></span>

-   <span data-ttu-id="c4b7b-110">Användare kan autentiseras av Windows Server Active Directory.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-110">Users can be authenticated by Windows Server Active Directory.</span></span>

<span data-ttu-id="c4b7b-111">Om en användare har en prenumeration på Azure eller Office 365 och har använt hello Azure-portalen eller ett Office 365-program, kommer de att kunna toouse hello åtkomstpanelen sömlöst utan att behöva toosign i igen.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-111">If a user has a subscription for Azure or Office 365 and has been using hello Azure portal or an Office 365 application, they'll be able toouse hello Access Panel seamlessly without needing toosign in again.</span></span> <span data-ttu-id="c4b7b-112">Användare som inte autentiseras att ange toosign i med hjälp av hello användarnamn och lösenord för sitt konto i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-112">Users who are not authenticated be prompted toosign in by using hello username and password for their account in Azure AD.</span></span> <span data-ttu-id="c4b7b-113">Om hello organisationen har konfigurerat federation, räcker att skriva hello användarnamn.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-113">If hello organization has configured federation, typing hello username is sufficient.</span></span>

## <a name="general-issues-toocheck-first"></a><span data-ttu-id="c4b7b-114">Allmänna problem toocheck först</span><span class="sxs-lookup"><span data-stu-id="c4b7b-114">General issues toocheck first</span></span> 

-   <span data-ttu-id="c4b7b-115">Kontrollera att hello användaren loggar in toohello **korrigera URL**: <https://myapps.microsoft.com></span><span class="sxs-lookup"><span data-stu-id="c4b7b-115">Make sure hello user is signing in toohello **correct URL**: <https://myapps.microsoft.com></span></span>

-   <span data-ttu-id="c4b7b-116">Kontrollera att hello användarens webbläsare har lagt till hello URL tooits **tillförlitliga platser**</span><span class="sxs-lookup"><span data-stu-id="c4b7b-116">Make sure hello user’s browser has added hello URL tooits **trusted sites**</span></span>

-   <span data-ttu-id="c4b7b-117">Se till att hello användarkonto **aktiverat** för inloggningar.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-117">Make sure hello user’s account is **enabled** for sign ins.</span></span>

-   <span data-ttu-id="c4b7b-118">Kontrollera att hello användarens konto är **inte låst.**</span><span class="sxs-lookup"><span data-stu-id="c4b7b-118">Make sure hello user’s account is **not locked out.**</span></span>

-   <span data-ttu-id="c4b7b-119">Se till att hello användarens **lösenord inte har upphört att gälla eller har glömt.**</span><span class="sxs-lookup"><span data-stu-id="c4b7b-119">Make sure hello user’s **password is not expired or forgotten.**</span></span>

-   <span data-ttu-id="c4b7b-120">Kontrollera att **Multifaktorautentisering** inte blockerar åtkomst.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-120">Make sure **Multi-Factor Authentication** is not blocking user access.</span></span>

-   <span data-ttu-id="c4b7b-121">Kontrollera att en **principen för villkorlig åtkomst** eller **identitetsskydd** principen inte blockerar åtkomst.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-121">Make sure a **Conditional Access policy** or **Identity Protection** policy is not blocking user access.</span></span>

-   <span data-ttu-id="c4b7b-122">Se till att en användares **autentisering kontaktinformation** fungerar toodate tooallow Multifaktorautentisering eller villkorlig åtkomst principer toobe tillämpas.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-122">Make sure that a user’s **authentication contact info** is up toodate tooallow Multi-Factor Authentication or Conditional Access policies toobe enforced.</span></span>

-   <span data-ttu-id="c4b7b-123">Se till att tooalso försök rensar cookies i webbläsaren och försök toosign i igen.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-123">Make sure tooalso try clearing your browser’s cookies and trying toosign in again.</span></span>

## <a name="meeting-browser-requirements-for-hello-access-panel"></a><span data-ttu-id="c4b7b-124">Möte Webbläsarkrav för hello åtkomstpanelen</span><span class="sxs-lookup"><span data-stu-id="c4b7b-124">Meeting browser requirements for hello Access Panel</span></span>

<span data-ttu-id="c4b7b-125">Hej åtkomstpanelen kräver en webbläsare som stöder JavaScript och har aktiverat CSS.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-125">hello Access Panel requires a browser that supports JavaScript and has CSS enabled.</span></span> <span data-ttu-id="c4b7b-126">toouse lösenordsbaserade enkel inloggning (SSO) i hello åtkomstpanelen hello åtkomstpanelen tillägget måste vara installerad i hello användarens webbläsare.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-126">toouse password-based single sign-on (SSO) in hello Access Panel, hello Access Panel extension must be installed in hello user’s browser.</span></span> <span data-ttu-id="c4b7b-127">Det här tillägget laddas ned automatiskt när en användare väljer ett program som har konfigurerats för lösenordsbaserad enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-127">This extension is downloaded automatically when a user selects an application that is configured for password-based SSO.</span></span>

<span data-ttu-id="c4b7b-128">För lösenordsbaserad SSO kan hello användarens webbläsare vara:</span><span class="sxs-lookup"><span data-stu-id="c4b7b-128">For password-based SSO, hello end user’s browsers can be:</span></span>

-   <span data-ttu-id="c4b7b-129">Internet Explorer 8, 9, 10, 11--på Windows 7 eller senare</span><span class="sxs-lookup"><span data-stu-id="c4b7b-129">Internet Explorer 8, 9, 10, 11 -- on Windows 7 or later</span></span>

-   <span data-ttu-id="c4b7b-130">Kanten på Windows 10 årsdagar Edition eller senare</span><span class="sxs-lookup"><span data-stu-id="c4b7b-130">Edge on Windows 10 Anniversary Edition or later</span></span> 

-   <span data-ttu-id="c4b7b-131">Chrome--På Windows 7 eller senare, och i MacOS X eller senare</span><span class="sxs-lookup"><span data-stu-id="c4b7b-131">Chrome -- on Windows 7 or later, and on MacOS X or later</span></span>

-   <span data-ttu-id="c4b7b-132">Firefox 26.0 eller senare--på Windows XP SP2 eller senare, och på Mac OS X 10,6 eller senare</span><span class="sxs-lookup"><span data-stu-id="c4b7b-132">Firefox 26.0 or later -- on Windows XP SP2 or later, and on Mac OS X 10.6 or later</span></span>


## <a name="problems-with-hello-users-account"></a><span data-ttu-id="c4b7b-133">Problem med hello användarkonto</span><span class="sxs-lookup"><span data-stu-id="c4b7b-133">Problems with hello user’s account</span></span>

<span data-ttu-id="c4b7b-134">Åtkomst toohello åtkomstpanelen blockeras på grund av tooa problem med hello användarkonto.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-134">Access toohello Access Panel can be blocked due tooa problem with hello user’s account.</span></span> <span data-ttu-id="c4b7b-135">Här följer några metoder som du kan felsöka och lösa problem med användare och deras kontoinställningar:</span><span class="sxs-lookup"><span data-stu-id="c4b7b-135">Below are some ways you can troubleshoot and solve problems with users and their account settings:</span></span>

-   [<span data-ttu-id="c4b7b-136">Kontrollera om det finns ett konto i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="c4b7b-136">Check if a user account exists in Azure Active Directory</span></span>](#check-if-a-user-account-exists-in-azure-active-directory)

-   [<span data-ttu-id="c4b7b-137">Kontrollera status för en användare</span><span class="sxs-lookup"><span data-stu-id="c4b7b-137">Check a user’s account status</span></span>](#check-a-users-account-status)

-   [<span data-ttu-id="c4b7b-138">Återställa en användares lösenord</span><span class="sxs-lookup"><span data-stu-id="c4b7b-138">Reset a user’s password</span></span>](#reset-a-users-password)

-   [<span data-ttu-id="c4b7b-139">Aktivera lösenordsåterställning via självbetjäning</span><span class="sxs-lookup"><span data-stu-id="c4b7b-139">Enable self-service password reset</span></span>](#enable-self-service-password-reset)

-   [<span data-ttu-id="c4b7b-140">Kontrollera status för multifaktorautentisering för en användare</span><span class="sxs-lookup"><span data-stu-id="c4b7b-140">Check a user’s multi-factor authentication status</span></span>](#check-a-users-multi-factor-authentication-status)

-   [<span data-ttu-id="c4b7b-141">Kontrollera en användares kontaktinformation för autentisering</span><span class="sxs-lookup"><span data-stu-id="c4b7b-141">Check a user’s authentication contact info</span></span>](#check-a-users-authentication-contact-info)

-   [<span data-ttu-id="c4b7b-142">Kontrollera en användares gruppmedlemskap</span><span class="sxs-lookup"><span data-stu-id="c4b7b-142">Check a user’s group memberships</span></span>](#check-a-users-group-memberships)

-   [<span data-ttu-id="c4b7b-143">Kontrollera en användares tilldelade licenser</span><span class="sxs-lookup"><span data-stu-id="c4b7b-143">Check a user’s assigned licenses</span></span>](#check-a-users-assigned-licenses)

-   [<span data-ttu-id="c4b7b-144">Tilldela en användare en licens</span><span class="sxs-lookup"><span data-stu-id="c4b7b-144">Assign a user a license</span></span>](#assign-a-user-a-license)

### <a name="check-if-a-user-account-exists-in-azure-active-directory"></a><span data-ttu-id="c4b7b-145">Kontrollera om det finns ett konto i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="c4b7b-145">Check if a user account exists in Azure Active Directory</span></span>

<span data-ttu-id="c4b7b-146">toocheck om användarkontot finns, så hello nedan:</span><span class="sxs-lookup"><span data-stu-id="c4b7b-146">toocheck if a user’s account is present, follow hello steps below:</span></span>

1.  <span data-ttu-id="c4b7b-147">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="c4b7b-147">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="c4b7b-148">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-148">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="c4b7b-149">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-149">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="c4b7b-150">Klicka på **användare och grupper** i hello navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-150">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="c4b7b-151">Klicka på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-151">click **All users**.</span></span>

6.  <span data-ttu-id="c4b7b-152">**Sök** för hello-användare som du är intresserad av och **klickar du på raden hello** tooselect.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-152">**Search** for hello user you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="c4b7b-153">Kontrollera hello egenskaper för hello användaren objektet toobe till att de ser ut som förväntat och inga data saknas.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-153">Check hello properties of hello user object toobe sure that they look as you expect and no data is missing.</span></span>

### <a name="check-a-users-account-status"></a><span data-ttu-id="c4b7b-154">Kontrollera status för en användare</span><span class="sxs-lookup"><span data-stu-id="c4b7b-154">Check a user’s account status</span></span>

<span data-ttu-id="c4b7b-155">toocheck en användare konto status, hello följande sätt:</span><span class="sxs-lookup"><span data-stu-id="c4b7b-155">toocheck a user’s account status, follow hello steps below:</span></span>

1.  <span data-ttu-id="c4b7b-156">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="c4b7b-156">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="c4b7b-157">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-157">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="c4b7b-158">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-158">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="c4b7b-159">Klicka på **användare och grupper** i hello navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-159">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="c4b7b-160">Klicka på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-160">click **All users**.</span></span>

6.  <span data-ttu-id="c4b7b-161">**Sök** för hello-användare som du är intresserad av och **klickar du på raden hello** tooselect.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-161">**Search** for hello user you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="c4b7b-162">Klicka på **profil**.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-162">click **Profile**.</span></span>

8.  <span data-ttu-id="c4b7b-163">Under **inställningar** se till att **Block logga in** har angetts för**nr**.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-163">Under **Settings** ensure that **Block sign in** is set too**No**.</span></span>

### <a name="reset-a-users-password"></a><span data-ttu-id="c4b7b-164">Återställa en användares lösenord</span><span class="sxs-lookup"><span data-stu-id="c4b7b-164">Reset a user’s password</span></span>

<span data-ttu-id="c4b7b-165">tooreset en användares lösenord, följ hello stegen nedan:</span><span class="sxs-lookup"><span data-stu-id="c4b7b-165">tooreset a user’s password, follow hello steps below:</span></span>

1.  <span data-ttu-id="c4b7b-166">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="c4b7b-166">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="c4b7b-167">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-167">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="c4b7b-168">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-168">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="c4b7b-169">Klicka på **användare och grupper** i hello navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-169">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="c4b7b-170">Klicka på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-170">click **All users**.</span></span>

6.  <span data-ttu-id="c4b7b-171">**Sök** för hello-användare som du är intresserad av och **klickar du på raden hello** tooselect.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-171">**Search** for hello user you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="c4b7b-172">Klicka på hello **Återställ lösenord** knappen hello överst på bladet för hello användare.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-172">click hello **Reset password** button at hello top of hello user blade.</span></span>

8.  <span data-ttu-id="c4b7b-173">Klicka på hello **Återställ lösenord** hello-knappen **Återställ lösenord** bladet som visas.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-173">click hello **Reset password** button on hello **Reset password** blade that appears.</span></span>

9.  <span data-ttu-id="c4b7b-174">Kopiera hello **tillfälligt lösenord** eller **ange ett nytt lösenord** för hello användare.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-174">Copy hello **temporary password** or **enter a new password** for hello user.</span></span>

10. <span data-ttu-id="c4b7b-175">Meddela den nya lösenord toohello användaren, de nödvändiga toochange lösenordet vid nästa inloggning tooAzure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-175">Communicate this new password toohello user, they be required toochange this password during their next sign in tooAzure Active Directory.</span></span>

### <a name="enable-self-service-password-reset"></a><span data-ttu-id="c4b7b-176">Aktivera lösenordsåterställning via självbetjäning</span><span class="sxs-lookup"><span data-stu-id="c4b7b-176">Enable self-service password reset</span></span>

<span data-ttu-id="c4b7b-177">tooenable självbetjäning lösenord återställa, hello distribution av följande sätt:</span><span class="sxs-lookup"><span data-stu-id="c4b7b-177">tooenable self-service password reset, follow hello deployment steps below:</span></span>

-   [<span data-ttu-id="c4b7b-178">Aktivera användare tooreset sina Azure Active Directory-lösenord</span><span class="sxs-lookup"><span data-stu-id="c4b7b-178">Enable users tooreset their Azure Active Directory passwords</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started#enable-users-to-reset-their-azure-ad-passwords)

-   [<span data-ttu-id="c4b7b-179">Aktivera användare tooreset eller ändra sina lokala Active Directory-lösenord</span><span class="sxs-lookup"><span data-stu-id="c4b7b-179">Enable users tooreset or change their Active Directory on-premises passwords</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started#enable-users-to-reset-or-change-their-ad-passwords)

### <a name="check-a-users-multi-factor-authentication-status"></a><span data-ttu-id="c4b7b-180">Kontrollera status för multifaktorautentisering för en användare</span><span class="sxs-lookup"><span data-stu-id="c4b7b-180">Check a user’s multi-factor authentication status</span></span>

<span data-ttu-id="c4b7b-181">toocheck en användare har Multi-Factor authentication status, följ hello stegen nedan:</span><span class="sxs-lookup"><span data-stu-id="c4b7b-181">toocheck a user’s multi-factor authentication status, follow hello steps below:</span></span>

1.  <span data-ttu-id="c4b7b-182">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="c4b7b-182">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="c4b7b-183">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-183">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="c4b7b-184">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-184">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="c4b7b-185">Klicka på **användare och grupper** i hello navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-185">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="c4b7b-186">Klicka på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-186">click **All users**.</span></span>

6.  <span data-ttu-id="c4b7b-187">Klicka på hello **Multifaktorautentisering** knappen hello överst i hello-bladet.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-187">click hello **Multi-Factor Authentication** button at hello top of hello blade.</span></span>

7.  <span data-ttu-id="c4b7b-188">En gång hello **Multi-Factor Authentication-administrationsportalen** belastning, kontrollera att du är på hello **användare** fliken.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-188">Once hello **Multi-Factor Authentication Administration Portal** loads, ensure you are on hello **Users** tab.</span></span>

8.  <span data-ttu-id="c4b7b-189">Hitta hello användaren i hello lista över användare genom att söka, filtrering och sortering.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-189">Find hello user in hello list of users by searching, filtering, or sorting.</span></span>

9.  <span data-ttu-id="c4b7b-190">Välj hello användaren hello listan över användare och **aktivera**, **inaktivera**, eller **framtvinga** multifaktorautentisering efter behov.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-190">Select hello user from hello list of users and **Enable**, **Disable**, or **Enforce** multi-factor authentication as desired.</span></span>

   >[!NOTE]
   ><span data-ttu-id="c4b7b-191">Om en användare finns i en **tvingande** tillstånd, så kan du ange dem för**inaktiverad** tillfälligt toolet dem tillbaka till deras konto.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-191">If a user is in an **Enforced** state, you may set them too**Disabled** temporarily toolet them back into their account.</span></span> <span data-ttu-id="c4b7b-192">När de är tillbaka i sedan kan du ändra tillståndet för**aktiverad** igen toorequire dem toore-register kontaktuppgifter under nästa inloggning.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-192">Once they are back in, you can then change their state too**Enabled** again toorequire them toore-register their contact information during their next sign in.</span></span> <span data-ttu-id="c4b7b-193">Du kan också följa hello stegen i hello [Kontrollera en användares autentisering kontaktinformation](#check-a-users-authentication-contact-info) tooverify eller ange informationen för dessa.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-193">Alternatively, you can follow hello steps in hello [Check a user’s authentication contact info](#check-a-users-authentication-contact-info) tooverify or set this data for them.</span></span>
   >
   >

### <a name="check-a-users-authentication-contact-info"></a><span data-ttu-id="c4b7b-194">Kontrollera en användares kontaktinformation för autentisering</span><span class="sxs-lookup"><span data-stu-id="c4b7b-194">Check a user’s authentication contact info</span></span>

<span data-ttu-id="c4b7b-195">toocheck en användarautentisering kontaktuppgifter som används för multifaktorautentisering, villkorlig åtkomst, identitetsskydd och återställning av lösenord gör hello nedan:</span><span class="sxs-lookup"><span data-stu-id="c4b7b-195">toocheck a user’s authentication contact info used for Multi-factor authentication, Conditional Access, Identity Protection, and Password Reset, follow hello steps below:</span></span>

1.  <span data-ttu-id="c4b7b-196">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="c4b7b-196">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="c4b7b-197">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-197">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="c4b7b-198">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-198">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="c4b7b-199">Klicka på **användare och grupper** i hello navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-199">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="c4b7b-200">Klicka på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-200">click **All users**.</span></span>

6.  <span data-ttu-id="c4b7b-201">**Sök** för hello-användare som du är intresserad av och **klickar du på raden hello** tooselect.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-201">**Search** for hello user you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="c4b7b-202">Klicka på **profil**.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-202">click **Profile**.</span></span>

8.  <span data-ttu-id="c4b7b-203">Rulla nedåt för**autentisering kontaktinformation**.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-203">Scroll down too**Authentication contact info**.</span></span>

9.  <span data-ttu-id="c4b7b-204">**Granska** hello data som registrerats för hello användaren och uppdatera efter behov.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-204">**Review** hello data registered for hello user and update as needed.</span></span>

### <a name="check-a-users-group-memberships"></a><span data-ttu-id="c4b7b-205">Kontrollera en användares gruppmedlemskap</span><span class="sxs-lookup"><span data-stu-id="c4b7b-205">Check a user’s group memberships</span></span>

<span data-ttu-id="c4b7b-206">toocheck en användare gruppmedlemskap, hello följande sätt:</span><span class="sxs-lookup"><span data-stu-id="c4b7b-206">toocheck a user’s group memberships, follow hello steps below:</span></span>

1.  <span data-ttu-id="c4b7b-207">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="c4b7b-207">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="c4b7b-208">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-208">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="c4b7b-209">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-209">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="c4b7b-210">Klicka på **användare och grupper** i hello navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-210">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="c4b7b-211">Klicka på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-211">click **All users**.</span></span>

6.  <span data-ttu-id="c4b7b-212">**Sök** för hello-användare som du är intresserad av och **klickar du på raden hello** tooselect.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-212">**Search** for hello user you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="c4b7b-213">Klicka på **grupper** toosee vilka grupper hello användaren är medlem i.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-213">click **Groups** toosee which groups hello user is a member of.</span></span>

### <a name="check-a-users-assigned-licenses"></a><span data-ttu-id="c4b7b-214">Kontrollera en användares tilldelade licenser</span><span class="sxs-lookup"><span data-stu-id="c4b7b-214">Check a user’s assigned licenses</span></span>

<span data-ttu-id="c4b7b-215">toocheck en användare tilldelade licenser, följ hello stegen nedan:</span><span class="sxs-lookup"><span data-stu-id="c4b7b-215">toocheck a user’s assigned licenses, follow hello steps below:</span></span>

1.  <span data-ttu-id="c4b7b-216">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="c4b7b-216">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="c4b7b-217">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-217">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="c4b7b-218">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-218">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="c4b7b-219">Klicka på **användare och grupper** i hello navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-219">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="c4b7b-220">Klicka på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-220">click **All users**.</span></span>

6.  <span data-ttu-id="c4b7b-221">**Sök** för hello-användare som du är intresserad av och **klickar du på raden hello** tooselect.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-221">**Search** for hello user you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="c4b7b-222">Klicka på **licenser** toosee som licenser hello användare för närvarande har tilldelats.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-222">click **Licenses** toosee which licenses hello user currently has assigned.</span></span>

### <a name="assign-a-user-a-license"></a><span data-ttu-id="c4b7b-223">Tilldela en användare en licens</span><span class="sxs-lookup"><span data-stu-id="c4b7b-223">Assign a user a license</span></span> 

<span data-ttu-id="c4b7b-224">tooassign en licens tooa användare gör hello nedan:</span><span class="sxs-lookup"><span data-stu-id="c4b7b-224">tooassign a license tooa user, follow hello steps below:</span></span>

1.  <span data-ttu-id="c4b7b-225">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="c4b7b-225">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="c4b7b-226">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-226">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="c4b7b-227">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-227">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="c4b7b-228">Klicka på **användare och grupper** i hello navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-228">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="c4b7b-229">Klicka på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-229">click **All users**.</span></span>

6.  <span data-ttu-id="c4b7b-230">**Sök** för hello-användare som du är intresserad av och **klickar du på raden hello** tooselect.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-230">**Search** for hello user you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="c4b7b-231">Klicka på **licenser** toosee som licenser hello användare för närvarande har tilldelats.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-231">click **Licenses** toosee which licenses hello user currently has assigned.</span></span>

8.  <span data-ttu-id="c4b7b-232">Klicka på hello **tilldela** knappen.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-232">click hello **Assign** button.</span></span>

9.  <span data-ttu-id="c4b7b-233">Välj **en eller flera produkter** hello listan över tillgängliga produkter.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-233">Select **one or more products** from hello list of available products.</span></span>

10. <span data-ttu-id="c4b7b-234">**Valfria** klickar du på hello **tilldelning alternativ** objektet toogranularly tilldela produkter.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-234">**Optional** click hello **assignment options** item toogranularly assign products.</span></span> <span data-ttu-id="c4b7b-235">Klicka på **Ok** när detta har slutförts.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-235">Click **Ok** when this is completed.</span></span>

11. <span data-ttu-id="c4b7b-236">Klicka på hello **tilldela** knappen tooassign dessa licenser toothis användare.</span><span class="sxs-lookup"><span data-stu-id="c4b7b-236">Click hello **Assign** button tooassign these licenses toothis user.</span></span>

## <a name="if-these-troubleshooting-steps-do-not-resolve-hello-issue"></a><span data-ttu-id="c4b7b-237">Om felsökningen inte löser problemet hello</span><span class="sxs-lookup"><span data-stu-id="c4b7b-237">If these troubleshooting steps do not resolve hello issue</span></span>

<span data-ttu-id="c4b7b-238">Öppna ett supportärende med hello efter information om tillgängliga:</span><span class="sxs-lookup"><span data-stu-id="c4b7b-238">open a support ticket with hello following information if available:</span></span>

-   <span data-ttu-id="c4b7b-239">Fel-ID för korrelation</span><span class="sxs-lookup"><span data-stu-id="c4b7b-239">Correlation error ID</span></span>

-   <span data-ttu-id="c4b7b-240">UPN (användarens e-postadress)</span><span class="sxs-lookup"><span data-stu-id="c4b7b-240">UPN (user email address)</span></span>

-   <span data-ttu-id="c4b7b-241">Klient-ID:t</span><span class="sxs-lookup"><span data-stu-id="c4b7b-241">Tenant ID</span></span>

-   <span data-ttu-id="c4b7b-242">Typ av webbläsare</span><span class="sxs-lookup"><span data-stu-id="c4b7b-242">Browser type</span></span>

-   <span data-ttu-id="c4b7b-243">Tidszon och tid/tidsperioden under fel inträffar</span><span class="sxs-lookup"><span data-stu-id="c4b7b-243">Time zone and time/timeframe during error occurs</span></span>

-   <span data-ttu-id="c4b7b-244">Fiddler spårningar</span><span class="sxs-lookup"><span data-stu-id="c4b7b-244">Fiddler traces</span></span>

## <a name="next-steps"></a><span data-ttu-id="c4b7b-245">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c4b7b-245">Next steps</span></span>
[<span data-ttu-id="c4b7b-246">Tillhandahålla enkel inloggning tooyour appar med Application Proxy</span><span class="sxs-lookup"><span data-stu-id="c4b7b-246">Provide single sign-on tooyour apps with Application Proxy</span></span>](active-directory-application-proxy-sso-using-kcd.md)
