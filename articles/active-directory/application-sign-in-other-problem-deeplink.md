---
title: "logga in tooan program med hjälp av en djuplänken aaaProblems | Microsoft Docs"
description: "Hur tootroubleshoot problem med åtkomst till ett program från en Webbadress med hjälp av Azure AD-djuplänken"
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
ms.openlocfilehash: dc82410001ac05895cc0244c3a89ace71bcf01a9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="problems-signing-in-tooan-application-using-a-deeplink"></a><span data-ttu-id="db8db-103">Problem med att logga i tooan program som använder en djuplänken</span><span class="sxs-lookup"><span data-stu-id="db8db-103">Problems signing in tooan application using a deeplink</span></span>

<span data-ttu-id="db8db-104">Hej åtkomstpanelen är en webbaserad portal som gör att en användare med ett arbets eller skolkonto i Azure Active Directory (AD Azure) tooview och starta molnbaserade program som hello Azure AD-administratör har ge dem tillgång till.</span><span class="sxs-lookup"><span data-stu-id="db8db-104">hello Access Panel is a web-based portal which enables a user with a work or school account in Azure Active Directory (Azure AD) tooview and start cloud-based applications that hello Azure AD administrator has granted them access to.</span></span> 

<span data-ttu-id="db8db-105">Dessa program är konfigurerade för hello användare i hello Azure AD-portalen.</span><span class="sxs-lookup"><span data-stu-id="db8db-105">These applications are configured on behalf of hello user in hello Azure AD portal.</span></span> <span data-ttu-id="db8db-106">hello-programmet måste konfigureras korrekt och tilldelade toohello användare eller en grupp hello användare är medlem i toosee hello programmet hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="db8db-106">hello application must be configured properly and assigned toohello user or a group hello user is a member of toosee hello application in hello Access Panel.</span></span>

<span data-ttu-id="db8db-107">Djuplänkar eller användarbehörighet är URL: er länkar som användarna kan använda tooaccess sina lösenord SSO-program direkt från sina webbläsare URL fält.</span><span class="sxs-lookup"><span data-stu-id="db8db-107">Deep links or User access URLs are links your users may use tooaccess their password-SSO applications directly from their browsers URL bars.</span></span> <span data-ttu-id="db8db-108">Genom att gå toothis länken användare att automatiskt inloggad hello programmet utan att först ha toogo toohello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="db8db-108">By navigating toothis link, users be automatically signed into hello application without having toogo toohello Access Panel first.</span></span> <span data-ttu-id="db8db-109">Detta är hello samma länk som användaren använder tooaccess programmen från startprogrammet hello Office 365.</span><span class="sxs-lookup"><span data-stu-id="db8db-109">This is hello same link that users use tooaccess these applications from hello Office 365 application launcher.</span></span>

## <a name="general-issues-toocheck-first"></a><span data-ttu-id="db8db-110">Allmänna problem toocheck först</span><span class="sxs-lookup"><span data-stu-id="db8db-110">General issues toocheck first</span></span>

-   <span data-ttu-id="db8db-111">Kontrollera att du med hjälp av en **webbläsare** som uppfyller hello minimikraven för hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="db8db-111">Make sure your using a **browser** that meets hello minimum requirements for hello Access Panel.</span></span>

-   <span data-ttu-id="db8db-112">Kontrollera att hello användarens webbläsare har lagt till hello URL för hello programmet tooits **tillförlitliga platser**.</span><span class="sxs-lookup"><span data-stu-id="db8db-112">Make sure hello user’s browser has added hello URL of hello application tooits **trusted sites**.</span></span>

-   <span data-ttu-id="db8db-113">Se till att toocheck hello programmet **konfigurerats** korrekt.</span><span class="sxs-lookup"><span data-stu-id="db8db-113">Make sure toocheck hello application is **configured** correctly.</span></span>

-   <span data-ttu-id="db8db-114">Se till att hello användarkonto **aktiverat** för inloggningar.</span><span class="sxs-lookup"><span data-stu-id="db8db-114">Make sure hello user’s account is **enabled** for sign ins.</span></span>

-   <span data-ttu-id="db8db-115">Kontrollera att hello användarens konto är **inte låst.**</span><span class="sxs-lookup"><span data-stu-id="db8db-115">Make sure hello user’s account is **not locked out.**</span></span>

-   <span data-ttu-id="db8db-116">Se till att hello användarens **lösenord inte har upphört att gälla eller har glömt.**</span><span class="sxs-lookup"><span data-stu-id="db8db-116">Make sure hello user’s **password is not expired or forgotten.**</span></span>

-   <span data-ttu-id="db8db-117">Kontrollera att **Multifaktorautentisering** inte blockerar åtkomst.</span><span class="sxs-lookup"><span data-stu-id="db8db-117">Make sure **Multi-Factor Authentication** is not blocking user access.</span></span>

-   <span data-ttu-id="db8db-118">Kontrollera att en **principen för villkorlig åtkomst** eller **identitetsskydd** principen inte blockerar åtkomst.</span><span class="sxs-lookup"><span data-stu-id="db8db-118">Make sure a **Conditional Access policy** or **Identity Protection** policy is not blocking user access.</span></span>

-   <span data-ttu-id="db8db-119">Se till att en användares **autentisering kontaktinformation** fungerar toodate tooallow Multifaktorautentisering eller villkorlig åtkomst principer toobe tillämpas.</span><span class="sxs-lookup"><span data-stu-id="db8db-119">Make sure that a user’s **authentication contact info** is up toodate tooallow Multi-Factor Authentication or Conditional Access policies toobe enforced.</span></span>

-   <span data-ttu-id="db8db-120">Se till att tooalso försök rensar cookies i webbläsaren och försök toosign i igen.</span><span class="sxs-lookup"><span data-stu-id="db8db-120">Make sure tooalso try clearing your browser’s cookies and trying toosign in again.</span></span>

## <a name="checking-hello-deeplink"></a><span data-ttu-id="db8db-121">Kontrollera hello djuplänken</span><span class="sxs-lookup"><span data-stu-id="db8db-121">Checking hello deeplink</span></span>

<span data-ttu-id="db8db-122">toocheck om du har rätt hello-djuplänken gör hello nedan:</span><span class="sxs-lookup"><span data-stu-id="db8db-122">toocheck if you have hello correct deeplink, follow hello steps below:</span></span>

1.  <span data-ttu-id="db8db-123">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**</span><span class="sxs-lookup"><span data-stu-id="db8db-123">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="db8db-124">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="db8db-124">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="db8db-125">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="db8db-125">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="db8db-126">Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="db8db-126">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="db8db-127">Klicka på **alla program** tooview en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="db8db-127">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="db8db-128">Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**</span><span class="sxs-lookup"><span data-stu-id="db8db-128">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="db8db-129">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**</span><span class="sxs-lookup"><span data-stu-id="db8db-129">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

7.  <span data-ttu-id="db8db-130">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="db8db-130">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

8.  <span data-ttu-id="db8db-131">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="db8db-131">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

9.  <span data-ttu-id="db8db-132">Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="db8db-132">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

10. <span data-ttu-id="db8db-133">Klicka på **alla program** tooview en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="db8db-133">click **All Applications** tooview a list of all your applications.</span></span>

   * <span data-ttu-id="db8db-134">Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**</span><span class="sxs-lookup"><span data-stu-id="db8db-134">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

11. <span data-ttu-id="db8db-135">Välj hello-program som du vill använda hello Kontrollera hello djuplänken för.</span><span class="sxs-lookup"><span data-stu-id="db8db-135">Select hello application you want hello check hello deeplink for.</span></span>

12. <span data-ttu-id="db8db-136">Hitta hello etikett **användaren åtkomst-URL**.</span><span class="sxs-lookup"><span data-stu-id="db8db-136">Find hello label **User Access URL**.</span></span> <span data-ttu-id="db8db-137">Du djuplänken ska matcha denna URL.</span><span class="sxs-lookup"><span data-stu-id="db8db-137">You deeplink should match this URL.</span></span>

## <a name="how-tooinstall-hello-access-panel-browser-extension"></a><span data-ttu-id="db8db-138">Hur tooinstall hello åtkomst panelen webbläsartillägg</span><span class="sxs-lookup"><span data-stu-id="db8db-138">How tooinstall hello Access Panel Browser extension</span></span>

<span data-ttu-id="db8db-139">tooinstall hello webbläsartillägget för åtkomst till Kontrollpanelen, så hello nedan:</span><span class="sxs-lookup"><span data-stu-id="db8db-139">tooinstall hello Access Panel Browser extension, follow hello steps below:</span></span>

1.  <span data-ttu-id="db8db-140">Öppna hello [åtkomstpanelen](https://myapps.microsoft.com) i någon av hello stöds webbläsare och logga in som en **användaren** i din Azure AD.</span><span class="sxs-lookup"><span data-stu-id="db8db-140">Open hello [Access Panel](https://myapps.microsoft.com) in one of hello supported browsers and sign in as a **user** in your Azure AD.</span></span>

2.  <span data-ttu-id="db8db-141">Klicka på en **lösenord SSO-program** i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="db8db-141">click a **password-SSO application** in hello Access Panel.</span></span>

3.  <span data-ttu-id="db8db-142">I hello fråga frågar tooinstall hello programvara, väljer **installera nu**.</span><span class="sxs-lookup"><span data-stu-id="db8db-142">In hello prompt asking tooinstall hello software, select **Install Now**.</span></span>

4.  <span data-ttu-id="db8db-143">Baserat på din webbläsare vara du riktad toohello länken.</span><span class="sxs-lookup"><span data-stu-id="db8db-143">Based on your browser you be directed toohello download link.</span></span> <span data-ttu-id="db8db-144">**Lägg till** hello tillägget tooyour webbläsare.</span><span class="sxs-lookup"><span data-stu-id="db8db-144">**Add** hello extension tooyour browser.</span></span>

5.  <span data-ttu-id="db8db-145">Om din webbläsare frågar väljer tooeither **aktivera** eller **Tillåt** hello tillägg.</span><span class="sxs-lookup"><span data-stu-id="db8db-145">If your browser asks, select tooeither **Enable** or **Allow** hello extension.</span></span>

6.  <span data-ttu-id="db8db-146">När den har installerats, **starta om** webbläsarsessionen.</span><span class="sxs-lookup"><span data-stu-id="db8db-146">Once installed, **restart** your browser session.</span></span>

7.  <span data-ttu-id="db8db-147">Logga in till hello åtkomstpanelen och se om kan du **starta** lösenord SSO-program</span><span class="sxs-lookup"><span data-stu-id="db8db-147">Sign in into hello Access Panel and see if you can **launch** your password-SSO applications</span></span>

<span data-ttu-id="db8db-148">Du kan också hämta hello-tillägget för Chrome och Firefox från hello Direktlänkar nedan:</span><span class="sxs-lookup"><span data-stu-id="db8db-148">You may also download hello extension for Chrome and Firefox from hello direct links below:</span></span>

-   [<span data-ttu-id="db8db-149">Tillägget för Chrome åtkomst panelen</span><span class="sxs-lookup"><span data-stu-id="db8db-149">Chrome Access Panel Extension</span></span>](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [<span data-ttu-id="db8db-150">Tillägget för Firefox åtkomst panelen</span><span class="sxs-lookup"><span data-stu-id="db8db-150">Firefox Access Panel Extension</span></span>](https://addons.mozilla.org/firefox/addon/access-panel-extension/)

## <a name="how-tooconfigure-password-single-sign-on-for-an-azure-ad-gallery-application"></a><span data-ttu-id="db8db-151">Hur tooconfigure lösenord enkel inloggning för ett program för Azure AD-galleriet</span><span class="sxs-lookup"><span data-stu-id="db8db-151">How tooconfigure password single sign-on for an Azure AD gallery application</span></span>

<span data-ttu-id="db8db-152">tooconfigure ett program från hello Azure AD-galleriet måste du:</span><span class="sxs-lookup"><span data-stu-id="db8db-152">tooconfigure an application from hello Azure AD gallery you need to:</span></span>

-   [<span data-ttu-id="db8db-153">Lägga till ett program från hello Azure AD-galleriet</span><span class="sxs-lookup"><span data-stu-id="db8db-153">Add an application from hello Azure AD gallery</span></span>](#add-an-application-from-the-Azure-AD-gallery)

-   [<span data-ttu-id="db8db-154">Konfigurera hello program för lösenord för enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="db8db-154">Configure hello application for password single sign-on</span></span>](#configure-the-application-for-password-single-sign-on)

### <a name="add-an-application-from-hello-azure-ad-gallery"></a><span data-ttu-id="db8db-155">Lägga till ett program från hello Azure AD-galleriet</span><span class="sxs-lookup"><span data-stu-id="db8db-155">Add an application from hello Azure AD gallery</span></span>

<span data-ttu-id="db8db-156">tooadd ett program från hello Azure AD-galleriet så hello nedan:</span><span class="sxs-lookup"><span data-stu-id="db8db-156">tooadd an application from hello Azure AD Gallery, follow hello steps below:</span></span>

1.  <span data-ttu-id="db8db-157">Öppna hello [Azure Portal](https://portal.azure.com) och logga in som en **Global administratör** eller **medadministratör**.</span><span class="sxs-lookup"><span data-stu-id="db8db-157">Open hello [Azure Portal](https://portal.azure.com) and sign in as a **Global Administrator** or **Co-admin**.</span></span>

2.  <span data-ttu-id="db8db-158">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="db8db-158">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="db8db-159">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="db8db-159">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="db8db-160">Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="db8db-160">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="db8db-161">Klicka på hello **Lägg till** längst hello övre högra hörnet på hello **företagsprogram** bladet.</span><span class="sxs-lookup"><span data-stu-id="db8db-161">click hello **Add** button at hello top-right corner on hello **Enterprise Applications** blade.</span></span>

6.  <span data-ttu-id="db8db-162">I hello **anger du ett namn** textruta från hello **Lägg till från galleriet hello** avsnitt, hello-typnamn för hello program.</span><span class="sxs-lookup"><span data-stu-id="db8db-162">In hello **Enter a name** textbox from hello **Add from hello gallery** section, type hello name of hello application.</span></span>

7.  <span data-ttu-id="db8db-163">Välj hello-program som du vill använda tooconfigure för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="db8db-163">Select hello application you want tooconfigure for single sign-on.</span></span>

8.  <span data-ttu-id="db8db-164">Innan du lägger till programmet hello kan du ändra dess namn från hello **namn** textruta.</span><span class="sxs-lookup"><span data-stu-id="db8db-164">Before adding hello application, you can change its name from hello **Name** textbox.</span></span>

9.  <span data-ttu-id="db8db-165">Klicka på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="db8db-165">Click **Add** button, tooadd hello application.</span></span>

<span data-ttu-id="db8db-166">Efter en kort period vara kan toosee hello programmets konfiguration bladet.</span><span class="sxs-lookup"><span data-stu-id="db8db-166">After a short period, you be able toosee hello application’s configuration blade.</span></span>

### <a name="configure-hello-application-for-password-single-sign-on"></a><span data-ttu-id="db8db-167">Konfigurera hello program för lösenord för enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="db8db-167">Configure hello application for password single sign-on</span></span>

<span data-ttu-id="db8db-168">tooconfigure enkel inloggning för ett program gör hello nedan:</span><span class="sxs-lookup"><span data-stu-id="db8db-168">tooconfigure single sign-on for an application, follow hello steps below:</span></span>

1.  <span data-ttu-id="db8db-169">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**</span><span class="sxs-lookup"><span data-stu-id="db8db-169">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="db8db-170">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="db8db-170">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="db8db-171">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="db8db-171">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="db8db-172">Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="db8db-172">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="db8db-173">Klicka på **alla program** tooview en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="db8db-173">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="db8db-174">Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**</span><span class="sxs-lookup"><span data-stu-id="db8db-174">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="db8db-175">Välj hello-program som du vill tooconfigure enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="db8db-175">Select hello application you want tooconfigure single sign-on.</span></span>

7.  <span data-ttu-id="db8db-176">När programmet hello läses in klickar du på hello **enkel inloggning** från hello programmet vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="db8db-176">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="db8db-177">Välj hello läge **lösenordsbaserade inloggning.**</span><span class="sxs-lookup"><span data-stu-id="db8db-177">Select hello mode **Password-based Sign-on.**</span></span>

9.  <span data-ttu-id="db8db-178">[Tilldela användare toohello program](#how-to-assign-a-user-to-an-application-directly).</span><span class="sxs-lookup"><span data-stu-id="db8db-178">[Assign users toohello application](#how-to-assign-a-user-to-an-application-directly).</span></span>

10. <span data-ttu-id="db8db-179">Du kan dessutom också ange autentiseringsuppgifter för hello användares räkning genom att markera rader hello hello användare och klicka på **referenser uppdatering** och ange hello användarnamn och lösenord för åt hello användare.</span><span class="sxs-lookup"><span data-stu-id="db8db-179">Additionally, you can also provide credentials on behalf of hello user by selecting hello rows of hello users and clicking on **Update Credentials** and entering hello username and password on behalf of hello users.</span></span> <span data-ttu-id="db8db-180">Annars kommer att användare tillfrågas tooenter hello autentiseringsuppgifter sig vid start.</span><span class="sxs-lookup"><span data-stu-id="db8db-180">Otherwise, users be prompted tooenter hello credentials themselves upon launch.</span></span>

## <a name="how-tooconfigure-password-single-sign-on-for-a-non-gallery-application"></a><span data-ttu-id="db8db-181">Hur tooconfigure lösenord enkel inloggning för ett program för icke-galleriet</span><span class="sxs-lookup"><span data-stu-id="db8db-181">How tooconfigure password single sign-on for a non-gallery application</span></span>

<span data-ttu-id="db8db-182">tooconfigure ett program från hello Azure AD-galleriet måste du:</span><span class="sxs-lookup"><span data-stu-id="db8db-182">tooconfigure an application from hello Azure AD gallery you need to:</span></span>

-   [<span data-ttu-id="db8db-183">Lägga till ett icke-galleriet program</span><span class="sxs-lookup"><span data-stu-id="db8db-183">Add a non-gallery application</span></span>](#add-a-non-gallery-application)

-   [<span data-ttu-id="db8db-184">Konfigurera hello program för lösenord för enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="db8db-184">Configure hello application for password single sign-on</span></span>](#configure-the-application-for-password-single-sign-on)

### <a name="add-a-non-gallery-application"></a><span data-ttu-id="db8db-185">Lägga till ett icke-galleriet program</span><span class="sxs-lookup"><span data-stu-id="db8db-185">Add a non-gallery application</span></span>

<span data-ttu-id="db8db-186">tooadd ett program från hello Azure AD-galleriet så hello nedan:</span><span class="sxs-lookup"><span data-stu-id="db8db-186">tooadd an application from hello Azure AD Gallery, follow hello steps below:</span></span>

1.  <span data-ttu-id="db8db-187">Öppna hello [Azure Portal](https://portal.azure.com) och logga in som en **Global administratör** eller **medadministratör**.</span><span class="sxs-lookup"><span data-stu-id="db8db-187">Open hello [Azure Portal](https://portal.azure.com) and sign in as a **Global Administrator** or **Co-admin**.</span></span>

2.  <span data-ttu-id="db8db-188">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="db8db-188">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="db8db-189">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="db8db-189">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="db8db-190">Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="db8db-190">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="db8db-191">Klicka på hello **Lägg till** längst hello övre högra hörnet på hello **företagsprogram** bladet.</span><span class="sxs-lookup"><span data-stu-id="db8db-191">click hello **Add** button at hello top-right corner on hello **Enterprise Applications** blade.</span></span>

6.  <span data-ttu-id="db8db-192">Klicka på **icke-galleriet program.**</span><span class="sxs-lookup"><span data-stu-id="db8db-192">click **Non-gallery application.**</span></span>

7.  <span data-ttu-id="db8db-193">Ange hello namnet på ditt program i hello **namn** textruta.</span><span class="sxs-lookup"><span data-stu-id="db8db-193">Enter hello name of your application in hello **Name** textbox.</span></span> <span data-ttu-id="db8db-194">Välj **lägga till.**</span><span class="sxs-lookup"><span data-stu-id="db8db-194">Select **Add.**</span></span>

<span data-ttu-id="db8db-195">Efter en kort period vara kan toosee hello programmets konfiguration bladet.</span><span class="sxs-lookup"><span data-stu-id="db8db-195">After a short period, you be able toosee hello application’s configuration blade.</span></span>

### <a name="configure-hello-application-for-password-single-sign-on"></a><span data-ttu-id="db8db-196">Konfigurera hello program för lösenord för enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="db8db-196">Configure hello application for password single sign-on</span></span>

<span data-ttu-id="db8db-197">tooconfigure enkel inloggning för ett program gör hello nedan:</span><span class="sxs-lookup"><span data-stu-id="db8db-197">tooconfigure single sign-on for an application, follow hello steps below:</span></span>

1.  <span data-ttu-id="db8db-198">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**</span><span class="sxs-lookup"><span data-stu-id="db8db-198">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="db8db-199">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="db8db-199">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="db8db-200">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="db8db-200">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="db8db-201">Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="db8db-201">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="db8db-202">Klicka på **alla program** tooview en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="db8db-202">click **All Applications** tooview a list of all your applications.</span></span>

    1.  <span data-ttu-id="db8db-203">Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**</span><span class="sxs-lookup"><span data-stu-id="db8db-203">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="db8db-204">Välj hello-program som du vill tooconfigure enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="db8db-204">Select hello application you want tooconfigure single sign-on.</span></span>

7.  <span data-ttu-id="db8db-205">När programmet hello läses in klickar du på hello **enkel inloggning** från hello programmet vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="db8db-205">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="db8db-206">Välj hello läge **lösenordsbaserade inloggning.**</span><span class="sxs-lookup"><span data-stu-id="db8db-206">Select hello mode **Password-based Sign-on.**</span></span>

9.  <span data-ttu-id="db8db-207">Ange hello **inloggnings-URL**.</span><span class="sxs-lookup"><span data-stu-id="db8db-207">Enter hello **Sign-on URL**.</span></span> <span data-ttu-id="db8db-208">Det här är hello URL där användarna anger sina användarnamn och lösenord toosign i till.</span><span class="sxs-lookup"><span data-stu-id="db8db-208">This is hello URL where users enter their username and password toosign in to.</span></span> <span data-ttu-id="db8db-209">Kontrollera hello inloggning fält är synliga på hello-URL.</span><span class="sxs-lookup"><span data-stu-id="db8db-209">Ensure hello sign in fields are visible at hello URL.</span></span>

10. <span data-ttu-id="db8db-210">Tilldela användare toohello program.</span><span class="sxs-lookup"><span data-stu-id="db8db-210">Assign users toohello application.</span></span>

11. <span data-ttu-id="db8db-211">Du kan dessutom också ange autentiseringsuppgifter för hello användares räkning genom att markera rader hello hello användare och klicka på **referenser uppdatering** och ange hello användarnamn och lösenord för åt hello användare.</span><span class="sxs-lookup"><span data-stu-id="db8db-211">Additionally, you can also provide credentials on behalf of hello user by selecting hello rows of hello users and clicking on **Update Credentials** and entering hello username and password on behalf of hello users.</span></span> <span data-ttu-id="db8db-212">Annars kommer att användare tillfrågas tooenter hello autentiseringsuppgifter sig vid start.</span><span class="sxs-lookup"><span data-stu-id="db8db-212">Otherwise, users be prompted tooenter hello credentials themselves upon launch.</span></span>

## <a name="how-tooassign-a-user-tooan-application-directly"></a><span data-ttu-id="db8db-213">Hur tooassign en tooan användarprogram direkt</span><span class="sxs-lookup"><span data-stu-id="db8db-213">How tooassign a user tooan application directly</span></span>

<span data-ttu-id="db8db-214">tooassign en eller flera användare tooan programmet direkt, gör hello nedan:</span><span class="sxs-lookup"><span data-stu-id="db8db-214">tooassign one or more users tooan application directly, follow hello steps below:</span></span>

1.  <span data-ttu-id="db8db-215">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="db8db-215">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="db8db-216">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="db8db-216">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="db8db-217">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="db8db-217">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="db8db-218">Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="db8db-218">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="db8db-219">Klicka på **alla program** tooview en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="db8db-219">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="db8db-220">Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**</span><span class="sxs-lookup"><span data-stu-id="db8db-220">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="db8db-221">Välj hello-program som du vill tooassign en toofrom hello-användarlistan.</span><span class="sxs-lookup"><span data-stu-id="db8db-221">Select hello application you want tooassign a user toofrom hello list.</span></span>

7.  <span data-ttu-id="db8db-222">När programmet hello läses in klickar du på **användare och grupper** från hello programmet vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="db8db-222">Once hello application loads, click **Users and Groups** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="db8db-223">Klicka på hello **Lägg till** knappen ovanpå hello **användare och grupper** lista tooopen hello **Lägg uppdrag** bladet.</span><span class="sxs-lookup"><span data-stu-id="db8db-223">Click hello **Add** button on top of hello **Users and Groups** list tooopen hello **Add Assignment** blade.</span></span>

9.  <span data-ttu-id="db8db-224">Klicka på hello **användare och grupper** selector från hello **Lägg uppdrag** bladet.</span><span class="sxs-lookup"><span data-stu-id="db8db-224">click hello **Users and groups** selector from hello **Add Assignment** blade.</span></span>

10. <span data-ttu-id="db8db-225">Typen i hello **fullständigt namn** eller **e-postadress** för hello-användare som du vill tilldela till hello **Sök efter namn eller e-postadress** sökrutan.</span><span class="sxs-lookup"><span data-stu-id="db8db-225">Type in hello **full name** or **email address** of hello user you are interested in assigning into hello **Search by name or email address** search box.</span></span>

11. <span data-ttu-id="db8db-226">Hovra över hello **användare** i hello listan tooreveal en **kryssrutan**.</span><span class="sxs-lookup"><span data-stu-id="db8db-226">Hover over hello **user** in hello list tooreveal a **checkbox**.</span></span> <span data-ttu-id="db8db-227">Klicka på hello kryssrutan nästa toohello användarens profil foto eller logotypen tooadd användaren-toohello **valda** lista.</span><span class="sxs-lookup"><span data-stu-id="db8db-227">Click hello checkbox next toohello user’s profile photo or logo tooadd your user toohello **Selected** list.</span></span>

12. <span data-ttu-id="db8db-228">**Valfritt:** om du vill ha för**lägga till fler än en användare**, typ i en annan **fullständigt namn** eller **e-postadress** till hello **Sök efter namn eller e-postadress** sökrutan och klicka på hello kryssrutan tooadd den här användaren toohello **valda** lista.</span><span class="sxs-lookup"><span data-stu-id="db8db-228">**Optional:** If you would like too**add more than one user**, type in another **full name** or **email address** into hello **Search by name or email address** search box, and click hello checkbox tooadd this user toohello **Selected** list.</span></span>

13. <span data-ttu-id="db8db-229">När du har valt användare klickar du på hello **Välj** knappen tooadd dem toohello lista över användare och grupper toobe tilldelat toohello program.</span><span class="sxs-lookup"><span data-stu-id="db8db-229">When you are finished selecting users, click hello **Select** button tooadd them toohello list of users and groups toobe assigned toohello application.</span></span>

14. <span data-ttu-id="db8db-230">**Valfritt:** klickar du på hello **Välj roll** Väljaren i hello **Lägg uppdrag** bladet tooselect en roll tooassign toohello användare som du har valt.</span><span class="sxs-lookup"><span data-stu-id="db8db-230">**Optional:** click hello **Select Role** selector in hello **Add Assignment** blade tooselect a role tooassign toohello users you have selected.</span></span>

15. <span data-ttu-id="db8db-231">Klicka på hello **tilldela** knappen tooassign hello programmet toohello markerade användare.</span><span class="sxs-lookup"><span data-stu-id="db8db-231">Click hello **Assign** button tooassign hello application toohello selected users.</span></span>

<span data-ttu-id="db8db-232">Efter en kort period hello-användare som du har valt att kan toolaunch programmen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="db8db-232">After a short period, hello users you have selected be able toolaunch these applications in hello Access Panel.</span></span>

## <a name="if-these-troubleshooting-steps-do-not-hello-resolve-hello-issue"></a><span data-ttu-id="db8db-233">Om de här åtgärderna inte hello lösa problemet med hello.</span><span class="sxs-lookup"><span data-stu-id="db8db-233">If these troubleshooting steps do not hello resolve hello issue.</span></span> 

<span data-ttu-id="db8db-234">Öppna ett supportärende med hello efter information om tillgängliga:</span><span class="sxs-lookup"><span data-stu-id="db8db-234">open a support ticket with hello following information if available:</span></span>

-   <span data-ttu-id="db8db-235">Fel-ID för korrelation</span><span class="sxs-lookup"><span data-stu-id="db8db-235">Correlation error ID</span></span>

-   <span data-ttu-id="db8db-236">UPN (användarens e-postadress)</span><span class="sxs-lookup"><span data-stu-id="db8db-236">UPN (user email address)</span></span>

-   <span data-ttu-id="db8db-237">Klient-ID</span><span class="sxs-lookup"><span data-stu-id="db8db-237">TenantID</span></span>

-   <span data-ttu-id="db8db-238">Typ av webbläsare</span><span class="sxs-lookup"><span data-stu-id="db8db-238">Browser type</span></span>

-   <span data-ttu-id="db8db-239">Tidszon och tid/tidsperioden under fel inträffar</span><span class="sxs-lookup"><span data-stu-id="db8db-239">Time zone and time/timeframe during error occurs</span></span>

-   <span data-ttu-id="db8db-240">Fiddler spårningar</span><span class="sxs-lookup"><span data-stu-id="db8db-240">Fiddler traces</span></span>

## <a name="next-steps"></a><span data-ttu-id="db8db-241">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="db8db-241">Next steps</span></span>
[<span data-ttu-id="db8db-242">Tillhandahålla enkel inloggning tooyour appar med Application Proxy</span><span class="sxs-lookup"><span data-stu-id="db8db-242">Provide single sign-on tooyour apps with Application Proxy</span></span>](active-directory-application-proxy-sso-using-kcd.md)
