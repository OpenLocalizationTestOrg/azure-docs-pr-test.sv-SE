---
title: "aaaProblems inloggning tooan programmet från hello åtkomstpanelen | Microsoft Docs"
description: "Hur hello tootroubleshoot problem med åtkomst till ett program från Microsoft Azure AD-åtkomstpanelen på myapps.microsoft.com"
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
ms.reviewer: japere
ms.openlocfilehash: 346c4da06416bb9b330bdd5b1201253af19ba58b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="problems-signing-in-tooan-application-from-hello-access-panel"></a><span data-ttu-id="0998e-103">Problem med att logga i tooan program från hello åtkomstpanelen</span><span class="sxs-lookup"><span data-stu-id="0998e-103">Problems signing in tooan application from hello access panel</span></span>

<span data-ttu-id="0998e-104">Hej åtkomstpanelen är en webbaserad portal som gör att en användare med ett arbets eller skolkonto i Azure Active Directory (AD Azure) tooview och starta molnbaserade program som hello Azure AD-administratör har ge dem tillgång till.</span><span class="sxs-lookup"><span data-stu-id="0998e-104">hello Access Panel is a web-based portal which enables a user with a work or school account in Azure Active Directory (Azure AD) tooview and start cloud-based applications that hello Azure AD administrator has granted them access to.</span></span> 

<span data-ttu-id="0998e-105">Dessa program är konfigurerade för hello användare i hello Azure AD-portalen.</span><span class="sxs-lookup"><span data-stu-id="0998e-105">These applications are configured on behalf of hello user in hello Azure AD portal.</span></span> <span data-ttu-id="0998e-106">hello-programmet måste konfigureras korrekt och tilldelade toohello användare eller en grupp hello användare är medlem i toosee hello programmet hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="0998e-106">hello application must be configured properly and assigned toohello user or a group hello user is a member of toosee hello application in hello Access Panel.</span></span>

<span data-ttu-id="0998e-107">hello typ av appar som visas kanske en användare finns i hello följande kategorier:</span><span class="sxs-lookup"><span data-stu-id="0998e-107">hello type of apps a user may be seeing fall in hello following categories:</span></span>

-   <span data-ttu-id="0998e-108">Office 365-program</span><span class="sxs-lookup"><span data-stu-id="0998e-108">Office 365 Applications</span></span>

-   <span data-ttu-id="0998e-109">Microsoft och tredje parts program som har konfigurerats med federationsbaserat enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="0998e-109">Microsoft and third-party applications configured with federation-based SSO</span></span>

-   <span data-ttu-id="0998e-110">Lösenordsbaserade SSO-program</span><span class="sxs-lookup"><span data-stu-id="0998e-110">Password-based SSO applications</span></span>

-   <span data-ttu-id="0998e-111">Program med befintliga lösningar för enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="0998e-111">Applications with existing SSO solutions</span></span>

## <a name="general-issues-toocheck-first"></a><span data-ttu-id="0998e-112">Allmänna problem toocheck först</span><span class="sxs-lookup"><span data-stu-id="0998e-112">General issues toocheck first</span></span>

-   <span data-ttu-id="0998e-113">Kontrollera att du med hjälp av en **webbläsare** som uppfyller hello minimikraven för hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="0998e-113">Make sure your using a **browser** that meets hello minimum requirements for hello Access Panel.</span></span>

-   <span data-ttu-id="0998e-114">Kontrollera att hello användarens webbläsare har lagt till hello URL för hello programmet tooits **tillförlitliga platser**.</span><span class="sxs-lookup"><span data-stu-id="0998e-114">Make sure hello user’s browser has added hello URL of hello application tooits **trusted sites**.</span></span>

-   <span data-ttu-id="0998e-115">Se till att toocheck hello programmet **konfigurerats** korrekt.</span><span class="sxs-lookup"><span data-stu-id="0998e-115">Make sure toocheck hello application is **configured** correctly.</span></span>

-   <span data-ttu-id="0998e-116">Se till att hello användarkonto **aktiverat** för inloggningar.</span><span class="sxs-lookup"><span data-stu-id="0998e-116">Make sure hello user’s account is **enabled** for sign ins.</span></span>

-   <span data-ttu-id="0998e-117">Kontrollera att hello användarens konto är **inte låst.**</span><span class="sxs-lookup"><span data-stu-id="0998e-117">Make sure hello user’s account is **not locked out.**</span></span>

-   <span data-ttu-id="0998e-118">Se till att hello användarens **lösenord inte har upphört att gälla eller har glömt.**</span><span class="sxs-lookup"><span data-stu-id="0998e-118">Make sure hello user’s **password is not expired or forgotten.**</span></span>

-   <span data-ttu-id="0998e-119">Kontrollera att **Multifaktorautentisering** inte blockerar åtkomst.</span><span class="sxs-lookup"><span data-stu-id="0998e-119">Make sure **Multi-Factor Authentication** is not blocking user access.</span></span>

-   <span data-ttu-id="0998e-120">Kontrollera att en **principen för villkorlig åtkomst** eller **identitetsskydd** principen inte blockerar åtkomst.</span><span class="sxs-lookup"><span data-stu-id="0998e-120">Make sure a **Conditional Access policy** or **Identity Protection** policy is not blocking user access.</span></span>

-   <span data-ttu-id="0998e-121">Se till att en användares **autentisering kontaktinformation** fungerar toodate tooallow Multifaktorautentisering eller villkorlig åtkomst principer toobe tillämpas.</span><span class="sxs-lookup"><span data-stu-id="0998e-121">Make sure that a user’s **authentication contact info** is up toodate tooallow Multi-Factor Authentication or Conditional Access policies toobe enforced.</span></span>

-   <span data-ttu-id="0998e-122">Se till att tooalso försök rensar cookies i webbläsaren och försök toosign i igen.</span><span class="sxs-lookup"><span data-stu-id="0998e-122">Make sure tooalso try clearing your browser’s cookies and trying toosign in again.</span></span>

## <a name="meeting-browser-requirements-for-hello-access-panel"></a><span data-ttu-id="0998e-123">Möte Webbläsarkrav för hello åtkomstpanelen</span><span class="sxs-lookup"><span data-stu-id="0998e-123">Meeting browser requirements for hello Access Panel</span></span>

<span data-ttu-id="0998e-124">Hej åtkomstpanelen kräver en webbläsare som stöder JavaScript och har aktiverat CSS.</span><span class="sxs-lookup"><span data-stu-id="0998e-124">hello Access Panel requires a browser that supports JavaScript and has CSS enabled.</span></span> <span data-ttu-id="0998e-125">toouse lösenordsbaserade enkel inloggning (SSO) i hello åtkomstpanelen hello åtkomstpanelen tillägget måste vara installerad i hello användarens webbläsare.</span><span class="sxs-lookup"><span data-stu-id="0998e-125">toouse password-based single sign-on (SSO) in hello Access Panel, hello Access Panel extension must be installed in hello user’s browser.</span></span> <span data-ttu-id="0998e-126">Det här tillägget laddas ned automatiskt när en användare väljer ett program som har konfigurerats för lösenordsbaserad enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="0998e-126">This extension is downloaded automatically when a user selects an application that is configured for password-based SSO.</span></span>

<span data-ttu-id="0998e-127">För lösenordsbaserad SSO kan hello användarens webbläsare vara:</span><span class="sxs-lookup"><span data-stu-id="0998e-127">For password-based SSO, hello end user’s browsers can be:</span></span>

-   <span data-ttu-id="0998e-128">Internet Explorer 8, 9, 10, 11--på Windows 7 eller senare</span><span class="sxs-lookup"><span data-stu-id="0998e-128">Internet Explorer 8, 9, 10, 11 -- on Windows 7 or later</span></span>

-   <span data-ttu-id="0998e-129">Kanten på Windows 10 årsdagar Edition eller senare</span><span class="sxs-lookup"><span data-stu-id="0998e-129">Edge on Windows 10 Anniversary Edition or later</span></span>

-   <span data-ttu-id="0998e-130">Chrome--På Windows 7 eller senare, och i MacOS X eller senare</span><span class="sxs-lookup"><span data-stu-id="0998e-130">Chrome -- on Windows 7 or later, and on MacOS X or later</span></span>

-   <span data-ttu-id="0998e-131">Firefox 26.0 eller senare--på Windows XP SP2 eller senare, och på Mac OS X 10,6 eller senare</span><span class="sxs-lookup"><span data-stu-id="0998e-131">Firefox 26.0 or later -- on Windows XP SP2 or later, and on Mac OS X 10.6 or later</span></span>

## <a name="how-tooinstall-hello-access-panel-browser-extension"></a><span data-ttu-id="0998e-132">Hur tooinstall hello åtkomst panelen webbläsartillägg</span><span class="sxs-lookup"><span data-stu-id="0998e-132">How tooinstall hello Access Panel Browser extension</span></span>

<span data-ttu-id="0998e-133">tooinstall hello webbläsartillägget för åtkomst till Kontrollpanelen, så hello nedan:</span><span class="sxs-lookup"><span data-stu-id="0998e-133">tooinstall hello Access Panel Browser extension, follow hello steps below:</span></span>

1.  <span data-ttu-id="0998e-134">Öppna hello [åtkomstpanelen](https://myapps.microsoft.com) i någon av hello stöds webbläsare och logga in som en **användaren** i din Azure AD.</span><span class="sxs-lookup"><span data-stu-id="0998e-134">Open hello [Access Panel](https://myapps.microsoft.com) in one of hello supported browsers and sign in as a **user** in your Azure AD.</span></span>

2.  <span data-ttu-id="0998e-135">Klicka på en **lösenord SSO-program** i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="0998e-135">Click a **password-SSO application** in hello Access Panel.</span></span>

3.  <span data-ttu-id="0998e-136">I hello fråga frågar tooinstall hello programvara, väljer **installera nu**.</span><span class="sxs-lookup"><span data-stu-id="0998e-136">In hello prompt asking tooinstall hello software, select **Install Now**.</span></span>

4.  <span data-ttu-id="0998e-137">Baserat på din webbläsare vara du riktad toohello länken.</span><span class="sxs-lookup"><span data-stu-id="0998e-137">Based on your browser you be directed toohello download link.</span></span> <span data-ttu-id="0998e-138">**Lägg till** hello tillägget tooyour webbläsare.</span><span class="sxs-lookup"><span data-stu-id="0998e-138">**Add** hello extension tooyour browser.</span></span>

5.  <span data-ttu-id="0998e-139">Om din webbläsare frågar väljer tooeither **aktivera** eller **Tillåt** hello tillägg.</span><span class="sxs-lookup"><span data-stu-id="0998e-139">If your browser asks, select tooeither **Enable** or **Allow** hello extension.</span></span>

6.  <span data-ttu-id="0998e-140">När den har installerats, **starta om** webbläsarsessionen.</span><span class="sxs-lookup"><span data-stu-id="0998e-140">Once installed, **restart** your browser session.</span></span>

7.  <span data-ttu-id="0998e-141">Logga in till hello åtkomstpanelen och se om kan du **starta** lösenord SSO-program</span><span class="sxs-lookup"><span data-stu-id="0998e-141">Sign in into hello Access Panel and see if you can **launch** your password-SSO applications</span></span>

<span data-ttu-id="0998e-142">Du kan också hämta hello-tillägget för Chrome och kanten på hello Direktlänkar nedan:</span><span class="sxs-lookup"><span data-stu-id="0998e-142">You may also download hello extension for Chrome and Edge from hello direct links below:</span></span>

-   [<span data-ttu-id="0998e-143">Tillägget för Chrome åtkomst panelen</span><span class="sxs-lookup"><span data-stu-id="0998e-143">Chrome Access Panel Extension</span></span>](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [<span data-ttu-id="0998e-144">Tillägget för Microsoft Edge åtkomst panelen</span><span class="sxs-lookup"><span data-stu-id="0998e-144">Edge Access Panel Extension</span></span>](https://www.microsoft.com/store/apps/9pc9sckkzk84)

## <a name="how-tooconfigure-federated-single-sign-on-for-an-azure-ad-gallery-application"></a><span data-ttu-id="0998e-145">Hur tooconfigure federerad enkel inloggning för ett program för Azure AD-galleriet</span><span class="sxs-lookup"><span data-stu-id="0998e-145">How tooconfigure federated single sign-on for an Azure AD gallery application</span></span>

<span data-ttu-id="0998e-146">Alla program i hello Azure AD-galleriet aktiverad med Enterprise Single Sign-On-funktionen har en stegvis självstudiekurs som är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="0998e-146">All application in hello Azure AD gallery enabled with Enterprise Single Sign-On capability has a step-by-step tutorial available.</span></span> <span data-ttu-id="0998e-147">Du kan komma åt hello [lista över självstudier om hur toointegrate SaaS-appar med Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) en stegvisa instruktioner för detaljer.</span><span class="sxs-lookup"><span data-stu-id="0998e-147">You can access hello [List of tutorials on how toointegrate SaaS apps with Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) for a detail step-by-step guidance.</span></span>

<span data-ttu-id="0998e-148">tooconfigure ett program från hello Azure AD-galleriet måste du:</span><span class="sxs-lookup"><span data-stu-id="0998e-148">tooconfigure an application from hello Azure AD gallery you need to:</span></span>

-   [<span data-ttu-id="0998e-149">Lägga till ett program från hello Azure AD-galleriet</span><span class="sxs-lookup"><span data-stu-id="0998e-149">Add an application from hello Azure AD gallery</span></span>](#add-an-application)

-   [<span data-ttu-id="0998e-150">Konfigurera hello programmets metadatavärden i Azure AD (logga in på URL: en identifierare, Reply URL)</span><span class="sxs-lookup"><span data-stu-id="0998e-150">Configure hello application’s metadata values in Azure AD (Sign on URL, Identifier, Reply URL)</span></span>](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

-   [<span data-ttu-id="0998e-151">Välj användar-ID och Lägg till attribut toobe skickas toohello-användarprogram</span><span class="sxs-lookup"><span data-stu-id="0998e-151">Select User Identifier and add user attributes toobe sent toohello application</span></span>](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [<span data-ttu-id="0998e-152">Hämta Azure AD-metadata och certifikat</span><span class="sxs-lookup"><span data-stu-id="0998e-152">Retrieve Azure AD metadata and certificate</span></span>](#download-the-azure-ad-metadata-or-certificate)

-   [<span data-ttu-id="0998e-153">Konfigurera Azure AD metadatavärden i programmet hello (logga in på URL: en, utfärdare, logga ut URL-Adressen och certifikatet)</span><span class="sxs-lookup"><span data-stu-id="0998e-153">Configure Azure AD metadata values in hello application (Sign on URL, Issuer, Logout URL and certificate)</span></span>](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

-   [<span data-ttu-id="0998e-154">Tilldela användare toohello program</span><span class="sxs-lookup"><span data-stu-id="0998e-154">Assign users toohello application</span></span>](#assign-users-to-the-application)

### <a name="add-an-application-from-hello-azure-ad-gallery"></a><span data-ttu-id="0998e-155">Lägga till ett program från hello Azure AD-galleriet</span><span class="sxs-lookup"><span data-stu-id="0998e-155">Add an application from hello Azure AD gallery</span></span>

<span data-ttu-id="0998e-156">tooadd ett program från hello Azure AD-galleriet så hello nedan:</span><span class="sxs-lookup"><span data-stu-id="0998e-156">tooadd an application from hello Azure AD Gallery, follow hello steps below:</span></span>

1.  <span data-ttu-id="0998e-157">Öppna hello [Azure Portal](https://portal.azure.com) och logga in som en **Global administratör** eller **medadministratör**</span><span class="sxs-lookup"><span data-stu-id="0998e-157">Open hello [Azure Portal](https://portal.azure.com) and sign in as a **Global Administrator** or **Co-admin**</span></span>

2.  <span data-ttu-id="0998e-158">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="0998e-158">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="0998e-159">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="0998e-159">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="0998e-160">Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="0998e-160">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="0998e-161">Klicka på hello **Lägg till** längst hello övre högra hörnet på hello **företagsprogram** bladet</span><span class="sxs-lookup"><span data-stu-id="0998e-161">click hello **Add** button at hello top-right corner on hello **Enterprise Applications** blade</span></span>

6.  <span data-ttu-id="0998e-162">I hello **anger du ett namn** textruta från hello **Lägg till från galleriet hello** avsnitt, hello-typnamn för hello program.</span><span class="sxs-lookup"><span data-stu-id="0998e-162">In hello **Enter a name** textbox from hello **Add from hello gallery** section, type hello name of hello application.</span></span>

7.  <span data-ttu-id="0998e-163">Välj hello-program som du vill använda tooconfigure för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="0998e-163">Select hello application you want tooconfigure for single sign-on.</span></span>

8.  <span data-ttu-id="0998e-164">Innan du lägger till programmet hello kan du ändra dess namn från hello **namn** textruta.</span><span class="sxs-lookup"><span data-stu-id="0998e-164">Before adding hello application, you can change its name from hello **Name** textbox.</span></span>

9.  <span data-ttu-id="0998e-165">Klicka på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="0998e-165">Click **Add** button, tooadd hello application.</span></span>

<span data-ttu-id="0998e-166">Efter en kort period vara kan toosee hello programmets konfiguration bladet.</span><span class="sxs-lookup"><span data-stu-id="0998e-166">After a short period, you be able toosee hello application’s configuration blade.</span></span>

### <a name="configure-single-sign-on-for-an-application-from-hello-azure-ad-gallery"></a><span data-ttu-id="0998e-167">Konfigurera enkel inloggning för ett program från hello Azure AD-galleriet</span><span class="sxs-lookup"><span data-stu-id="0998e-167">Configure single sign-on for an application from hello Azure AD gallery</span></span>

<span data-ttu-id="0998e-168">tooconfigure enkel inloggning för ett program gör hello nedan:</span><span class="sxs-lookup"><span data-stu-id="0998e-168">tooconfigure single sign-on for an application, follow hello steps below:</span></span>

1.  <span data-ttu-id="0998e-169"><span id="_Hlk477187909" class="anchor"><span id="_Hlk477001983" class="anchor"></span></span>Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**</span><span class="sxs-lookup"><span data-stu-id="0998e-169"><span id="_Hlk477187909" class="anchor"><span id="_Hlk477001983" class="anchor"></span></span>Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="0998e-170">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="0998e-170">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="0998e-171">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="0998e-171">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="0998e-172">Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="0998e-172">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="0998e-173">Klicka på **alla program** tooview en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="0998e-173">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="0998e-174">Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**</span><span class="sxs-lookup"><span data-stu-id="0998e-174">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="0998e-175">Välj hello-program som du vill tooconfigure enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="0998e-175">Select hello application you want tooconfigure single sign-on.</span></span>

7.  <span data-ttu-id="0998e-176">När programmet hello läses in klickar du på hello **enkel inloggning** från hello programmet vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="0998e-176">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="0998e-177">Välj **SAML-baserade inloggning** från hello **läge** listrutan.</span><span class="sxs-lookup"><span data-stu-id="0998e-177">Select **SAML-based Sign-on** from hello **Mode** dropdown.</span></span>

9.  <span data-ttu-id="0998e-178">Ange hello krävs värden i **domän och URL: er.**</span><span class="sxs-lookup"><span data-stu-id="0998e-178">Enter hello required values in **Domain and URLs.**</span></span> <span data-ttu-id="0998e-179">Du bör få värdena från hello programvaruleverantören.</span><span class="sxs-lookup"><span data-stu-id="0998e-179">You should get these values from hello application vendor.</span></span>

   1. <span data-ttu-id="0998e-180">tooconfigure hello program som SP-initierad SSO hello logga på URL: en är ett obligatoriskt värde.</span><span class="sxs-lookup"><span data-stu-id="0998e-180">tooconfigure hello application as SP-initiated SSO, hello Sign on URL it’s a required value.</span></span> <span data-ttu-id="0998e-181">Hello identifierare är också ett obligatoriskt värde för vissa program.</span><span class="sxs-lookup"><span data-stu-id="0998e-181">For some applications, hello Identifier is also a required value.</span></span>

   2. <span data-ttu-id="0998e-182">tooconfigure hello program som IdP-initierad SSO hello Reply-URL som det är ett obligatoriskt värde.</span><span class="sxs-lookup"><span data-stu-id="0998e-182">tooconfigure hello application as IdP-initiated SSO, hello Reply URL it’s a required value.</span></span> <span data-ttu-id="0998e-183">Hello identifierare är också ett obligatoriskt värde för vissa program.</span><span class="sxs-lookup"><span data-stu-id="0998e-183">For some applications, hello Identifier is also a required value.</span></span>

10. <span data-ttu-id="0998e-184">**Valfritt:** klickar du på **visa avancerade inställningar för URL: en** om du vill toosee hello icke obligatoriska värden.</span><span class="sxs-lookup"><span data-stu-id="0998e-184">**Optional:** click **Show advanced URL settings** if you want toosee hello non-required values.</span></span>

11. <span data-ttu-id="0998e-185">I hello **användarattribut**, Välj hello Unik identifierare för användarna i hello **användar-ID** listrutan.</span><span class="sxs-lookup"><span data-stu-id="0998e-185">In hello **User attributes**, select hello unique identifier for your users in hello **User Identifier** dropdown.</span></span>

12. <span data-ttu-id="0998e-186">**Valfritt:** klickar du på **visa och redigera andra användarattribut** tooedit hello attribut toobe skickas toohello programmet hello SAML-token när användaren loggar in.</span><span class="sxs-lookup"><span data-stu-id="0998e-186">**Optional:** click **View and edit all other user attributes** tooedit hello attributes toobe sent toohello application in hello SAML token when user sign in.</span></span>

   <span data-ttu-id="0998e-187">tooadd ett attribut:</span><span class="sxs-lookup"><span data-stu-id="0998e-187">tooadd an attribute:</span></span>

   1. <span data-ttu-id="0998e-188">Klicka på **Lägg till attributet**.</span><span class="sxs-lookup"><span data-stu-id="0998e-188">click **Add attribute**.</span></span> <span data-ttu-id="0998e-189">Ange hello **namn** och hello väljer hello **värdet** hello listrutan.</span><span class="sxs-lookup"><span data-stu-id="0998e-189">Enter hello **Name** and hello select hello **Value** from hello dropdown.</span></span>

   2. <span data-ttu-id="0998e-190">Klicka på **spara.**</span><span class="sxs-lookup"><span data-stu-id="0998e-190">Click **Save.**</span></span> <span data-ttu-id="0998e-191">Du kan se hello nytt attribut i hello tabell.</span><span class="sxs-lookup"><span data-stu-id="0998e-191">You see hello new attribute in hello table.</span></span>

13. <span data-ttu-id="0998e-192">Klicka på **konfigurera &lt;programnamn&gt;**  tooaccess-dokumentationen om tooconfigure enkel inloggning i hello program.</span><span class="sxs-lookup"><span data-stu-id="0998e-192">click **Configure &lt;application name&gt;** tooaccess documentation on how tooconfigure single sign-on in hello application.</span></span> <span data-ttu-id="0998e-193">Du har dessutom hello metadata URL: er och certifikat som krävs för toosetup SSO med hello program.</span><span class="sxs-lookup"><span data-stu-id="0998e-193">Also, you has hello metadata URLs and certificate required toosetup SSO with hello application.</span></span>

14. <span data-ttu-id="0998e-194">Klicka på **spara** toosave hello konfiguration.</span><span class="sxs-lookup"><span data-stu-id="0998e-194">Click **Save** toosave hello configuration.</span></span>

15. <span data-ttu-id="0998e-195">Tilldela användare toohello program.</span><span class="sxs-lookup"><span data-stu-id="0998e-195">Assign users toohello application.</span></span>

### <a name="select-user-identifier-and-add-user-attributes-toobe-sent-toohello-application"></a><span data-ttu-id="0998e-196">Välj användar-ID och Lägg till attribut toobe skickas toohello-användarprogram</span><span class="sxs-lookup"><span data-stu-id="0998e-196">Select User Identifier and add user attributes toobe sent toohello application</span></span>

<span data-ttu-id="0998e-197">tooselect hello användar-ID och lägga till användarattribut, hello följande sätt:</span><span class="sxs-lookup"><span data-stu-id="0998e-197">tooselect hello User Identifier or add user attributes, follow hello steps below:</span></span>

1.  <span data-ttu-id="0998e-198">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**</span><span class="sxs-lookup"><span data-stu-id="0998e-198">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="0998e-199">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="0998e-199">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="0998e-200">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="0998e-200">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="0998e-201">Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="0998e-201">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="0998e-202">Klicka på **alla program** tooview en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="0998e-202">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="0998e-203">Om du inte ser hello-program som du vill tooshow här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**</span><span class="sxs-lookup"><span data-stu-id="0998e-203">If you do not see hello application you want tooshow up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="0998e-204">Välj hello-program som du har konfigurerat för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="0998e-204">Select hello application you have configured single sign-on.</span></span>

7.  <span data-ttu-id="0998e-205">När programmet hello läses in klickar du på hello **enkel inloggning** från hello programmet vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="0998e-205">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="0998e-206">Under hello **användarattribut** väljer hello Unik identifierare för användarna i hello **användar-ID** listrutan.</span><span class="sxs-lookup"><span data-stu-id="0998e-206">Under hello **User attributes** section, select hello unique identifier for your users in hello **User Identifier** dropdown.</span></span> <span data-ttu-id="0998e-207">hello måste markerade alternativet toomatch hello förväntat värde i hello programanvändare tooauthenticate hello.</span><span class="sxs-lookup"><span data-stu-id="0998e-207">hello selected option needs toomatch hello expected value in hello application tooauthenticate hello user.</span></span>

    >[!NOTE]
    ><span data-ttu-id="0998e-208">Azure AD väljer hello format för hello NameID attribut (användar-ID) baserat på valda hello värdet eller hello format som begärs av programmet hello i hello SAML AuthRequest.</span><span class="sxs-lookup"><span data-stu-id="0998e-208">Azure AD select hello format for hello NameID attribute (User Identifier) based on hello value selected or hello format requested by hello application in hello SAML AuthRequest.</span></span> <span data-ttu-id="0998e-209">Mer information finns i artikeln hello [enkel inloggning SAML protokoll](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) under hello avsnittet NameIDPolicy.</span><span class="sxs-lookup"><span data-stu-id="0998e-209">For more information visit hello article [Single Sign-On SAML protocol](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) under hello section NameIDPolicy.</span></span>
    >
    >

9.  <span data-ttu-id="0998e-210">tooadd användarattribut klickar du på **visa och redigera andra användarattribut** tooedit hello attribut toobe skickas toohello programmet hello SAML-token när användaren loggar in.</span><span class="sxs-lookup"><span data-stu-id="0998e-210">tooadd user attributes, click **View and edit all other user attributes** tooedit hello attributes toobe sent toohello application in hello SAML token when user sign in.</span></span>

   <span data-ttu-id="0998e-211">tooadd ett attribut:</span><span class="sxs-lookup"><span data-stu-id="0998e-211">tooadd an attribute:</span></span>

   1. <span data-ttu-id="0998e-212">Klicka på **Lägg till attributet**.</span><span class="sxs-lookup"><span data-stu-id="0998e-212">click **Add attribute**.</span></span> <span data-ttu-id="0998e-213">Ange hello **namn** och hello väljer hello **värdet** hello listrutan.</span><span class="sxs-lookup"><span data-stu-id="0998e-213">Enter hello **Name** and hello select hello **Value** from hello dropdown.</span></span>

   2. <span data-ttu-id="0998e-214">Klicka på **spara.**</span><span class="sxs-lookup"><span data-stu-id="0998e-214">Click **Save.**</span></span> <span data-ttu-id="0998e-215">Du kan se hello nytt attribut i hello tabell.</span><span class="sxs-lookup"><span data-stu-id="0998e-215">You see hello new attribute in hello table.</span></span>

### <a name="download-hello-azure-ad-metadata-or-certificate"></a><span data-ttu-id="0998e-216">Hämta metadata för hello Azure AD eller certifikat</span><span class="sxs-lookup"><span data-stu-id="0998e-216">Download hello Azure AD metadata or certificate</span></span>

<span data-ttu-id="0998e-217">toodownload hello programmetadata eller certifikat från Azure AD gör hello nedan:</span><span class="sxs-lookup"><span data-stu-id="0998e-217">toodownload hello application metadata or certificate from Azure AD, follow hello steps below:</span></span>

1.  <span data-ttu-id="0998e-218">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**</span><span class="sxs-lookup"><span data-stu-id="0998e-218">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="0998e-219">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="0998e-219">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="0998e-220">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="0998e-220">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="0998e-221">Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="0998e-221">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="0998e-222">Klicka på **alla program** tooview en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="0998e-222">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="0998e-223">Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**</span><span class="sxs-lookup"><span data-stu-id="0998e-223">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="0998e-224">Välj hello-program som du har konfigurerat för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="0998e-224">Select hello application you have configured single sign-on.</span></span>

7.  <span data-ttu-id="0998e-225">När programmet hello läses in klickar du på hello **enkel inloggning** från hello programmet vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="0998e-225">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="0998e-226">Gå för**SAML-signeringscertifikat** avsnittet och klicka sedan på **hämta** värde i kolumnen.</span><span class="sxs-lookup"><span data-stu-id="0998e-226">Go too**SAML Signing Certificate** section, then click **Download** column value.</span></span> <span data-ttu-id="0998e-227">Beroende på vilka hello-programmet kräver Konfigurera enkel inloggning, kan du se antingen hello alternativet toodownload hello XML-Metadata eller hello certifikat.</span><span class="sxs-lookup"><span data-stu-id="0998e-227">Depending on what hello application requires configuring single sign-on, you see either hello option toodownload hello Metadata XML or hello Certificate.</span></span>

    <span data-ttu-id="0998e-228">Azure AD Ange inte en URL tooget hello metadata.</span><span class="sxs-lookup"><span data-stu-id="0998e-228">Azure AD doesn’t provide a URL tooget hello metadata.</span></span> <span data-ttu-id="0998e-229">hello metadata kan endast hämtas som en XML-fil.</span><span class="sxs-lookup"><span data-stu-id="0998e-229">hello metadata can only be retrieved as a XML file.</span></span>

## <a name="how-tooconfigure-federated-single-sign-on-for-a-non-gallery-application"></a><span data-ttu-id="0998e-230">Hur tooconfigure federerad enkel inloggning för ett program för icke-galleriet</span><span class="sxs-lookup"><span data-stu-id="0998e-230">How tooconfigure federated single sign-on for a non-gallery application</span></span>

<span data-ttu-id="0998e-231">tooconfigure ett icke-galleriet program behöver du toohave Azure AD premium och hello programmet stöder SAML 2.0.</span><span class="sxs-lookup"><span data-stu-id="0998e-231">tooconfigure a non-gallery application, you need toohave Azure AD premium and hello application supports SAML 2.0.</span></span> <span data-ttu-id="0998e-232">Mer information om Azure AD-versioner finns [priser för Azure AD](https://azure.microsoft.com/pricing/details/active-directory/).</span><span class="sxs-lookup"><span data-stu-id="0998e-232">For more information about Azure AD versions, visit [Azure AD pricing](https://azure.microsoft.com/pricing/details/active-directory/).</span></span>

-   [<span data-ttu-id="0998e-233">Konfigurera hello programmets metadatavärden i Azure AD (logga in på URL: en identifierare, Reply URL)</span><span class="sxs-lookup"><span data-stu-id="0998e-233">Configure hello application’s metadata values in Azure AD (Sign on URL, Identifier, Reply URL)</span></span>](#configuring-single-sign-on)

-   [<span data-ttu-id="0998e-234">Välj användar-ID och Lägg till attribut toobe skickas toohello-användarprogram</span><span class="sxs-lookup"><span data-stu-id="0998e-234">Select User Identifier and add user attributes toobe sent toohello application</span></span>](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [<span data-ttu-id="0998e-235">Hämta Azure AD-metadata och certifikat</span><span class="sxs-lookup"><span data-stu-id="0998e-235">Retrieve Azure AD metadata and certificate</span></span>](#download-the-azure-ad-metadata-or-certificate)

-   [<span data-ttu-id="0998e-236">Konfigurera Azure AD metadatavärden i programmet hello (logga in på URL: en, utfärdare, logga ut URL-Adressen och certifikatet)</span><span class="sxs-lookup"><span data-stu-id="0998e-236">Configure Azure AD metadata values in hello application (Sign on URL, Issuer, Logout URL and certificate)</span></span>](#configuring-single-sign-on)

### <a name="configure-hello-applications-metadata-values-in-azure-ad-sign-on-url-identifier-reply-url"></a><span data-ttu-id="0998e-237">Konfigurera hello programmets metadatavärden i Azure AD (logga in på URL: en identifierare, Reply URL)</span><span class="sxs-lookup"><span data-stu-id="0998e-237">Configure hello application’s metadata values in Azure AD (Sign on URL, Identifier, Reply URL)</span></span>

<span data-ttu-id="0998e-238">tooconfigure enkel inloggning för ett program som inte är i hello Azure AD-galleriet gör hello nedan:</span><span class="sxs-lookup"><span data-stu-id="0998e-238">tooconfigure single sign-on for an application that is not in hello Azure AD gallery, follow hello steps below:</span></span>

1.  <span data-ttu-id="0998e-239">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**</span><span class="sxs-lookup"><span data-stu-id="0998e-239">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="0998e-240">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="0998e-240">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="0998e-241">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="0998e-241">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="0998e-242">Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="0998e-242">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="0998e-243">Klicka på hello **Lägg till** längst hello övre högra hörnet på hello **företagsprogram** bladet</span><span class="sxs-lookup"><span data-stu-id="0998e-243">click hello **Add** button at hello top-right corner on hello **Enterprise Applications** blade</span></span>

6.  <span data-ttu-id="0998e-244">Klicka på **icke-galleriet programmet** i hello **lägga till egna app** avsnitt</span><span class="sxs-lookup"><span data-stu-id="0998e-244">click **Non-gallery application** in hello **Add your own app** section</span></span>

7.  <span data-ttu-id="0998e-245">Ange hello namnet på programmet hello i hello **namn** textruta.</span><span class="sxs-lookup"><span data-stu-id="0998e-245">Enter hello name of hello application in hello **Name** textbox.</span></span>

8.  <span data-ttu-id="0998e-246">Klicka på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="0998e-246">Click **Add** button, tooadd hello application.</span></span>

9.  <span data-ttu-id="0998e-247">När programmet hello läses in klickar du på hello **enkel inloggning** från hello programmet vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="0998e-247">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

10. <span data-ttu-id="0998e-248">Välj **SAML-baserade inloggning** i hello **läge** listrutan</span><span class="sxs-lookup"><span data-stu-id="0998e-248">Select **SAML-based Sign-on** in hello **Mode** dropdown</span></span>

11. <span data-ttu-id="0998e-249">Ange hello krävs värden i **domän och URL: er.**</span><span class="sxs-lookup"><span data-stu-id="0998e-249">Enter hello required values in **Domain and URLs.**</span></span> <span data-ttu-id="0998e-250">Du bör få värdena från hello programvaruleverantören.</span><span class="sxs-lookup"><span data-stu-id="0998e-250">You should get these values from hello application vendor.</span></span>

  1. <span data-ttu-id="0998e-251">tooconfigure hello program som IdP-initierad SSO ange hello Reply URL och hello identifierare.</span><span class="sxs-lookup"><span data-stu-id="0998e-251">tooconfigure hello application as IdP-initiated SSO, enter hello Reply URL and hello Identifier.</span></span>

  2. <span data-ttu-id="0998e-252">**Valfritt:** tooconfigure hello program som SP-initierad SSO hello logga på URL: en är ett obligatoriskt värde.</span><span class="sxs-lookup"><span data-stu-id="0998e-252">**Optional:** tooconfigure hello application as SP-initiated SSO, hello Sign on URL it’s a required value.</span></span>

12. <span data-ttu-id="0998e-253">I hello **användarattribut**, Välj hello Unik identifierare för användarna i hello **användar-ID** listrutan.</span><span class="sxs-lookup"><span data-stu-id="0998e-253">In hello **User attributes**, select hello unique identifier for your users in hello **User Identifier** dropdown.</span></span>

13. <span data-ttu-id="0998e-254">**Valfritt:** klickar du på **visa och redigera andra användarattribut** tooedit hello attribut toobe skickas toohello programmet hello SAML-token när användaren loggar in.</span><span class="sxs-lookup"><span data-stu-id="0998e-254">**Optional:** click **View and edit all other user attributes** tooedit hello attributes toobe sent toohello application in hello SAML token when user sign in.</span></span>

   <span data-ttu-id="0998e-255">tooadd ett attribut:</span><span class="sxs-lookup"><span data-stu-id="0998e-255">tooadd an attribute:</span></span>

   1. <span data-ttu-id="0998e-256">Klicka på **Lägg till attributet**.</span><span class="sxs-lookup"><span data-stu-id="0998e-256">click **Add attribute**.</span></span> <span data-ttu-id="0998e-257">Ange hello **namn** och hello väljer hello **värdet** hello listrutan.</span><span class="sxs-lookup"><span data-stu-id="0998e-257">Enter hello **Name** and hello select hello **Value** from hello dropdown.</span></span>

   2. <span data-ttu-id="0998e-258">Klicka på **spara.**</span><span class="sxs-lookup"><span data-stu-id="0998e-258">Click **Save.**</span></span> <span data-ttu-id="0998e-259">Du kan se hello nytt attribut i hello tabell.</span><span class="sxs-lookup"><span data-stu-id="0998e-259">You see hello new attribute in hello table.</span></span>

14. <span data-ttu-id="0998e-260">Klicka på **konfigurera &lt;programnamn&gt;**  tooaccess-dokumentationen om tooconfigure enkel inloggning i hello program.</span><span class="sxs-lookup"><span data-stu-id="0998e-260">click **Configure &lt;application name&gt;** tooaccess documentation on how tooconfigure single sign-on in hello application.</span></span> <span data-ttu-id="0998e-261">Du har även Azure AD-URL: er och certifikat som krävs för programmet hello.</span><span class="sxs-lookup"><span data-stu-id="0998e-261">Also, you has Azure AD URLs and certificate required for hello application.</span></span>

### <a name="select-user-identifier-and-add-user-attributes-toobe-sent-toohello-application"></a><span data-ttu-id="0998e-262">Välj användar-ID och Lägg till attribut toobe skickas toohello-användarprogram</span><span class="sxs-lookup"><span data-stu-id="0998e-262">Select User Identifier and add user attributes toobe sent toohello application</span></span>

<span data-ttu-id="0998e-263">tooselect hello användar-ID och lägga till användarattribut, hello följande sätt:</span><span class="sxs-lookup"><span data-stu-id="0998e-263">tooselect hello User Identifier or add user attributes, follow hello steps below:</span></span>

1.  <span data-ttu-id="0998e-264">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**</span><span class="sxs-lookup"><span data-stu-id="0998e-264">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="0998e-265">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="0998e-265">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="0998e-266">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="0998e-266">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="0998e-267">Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="0998e-267">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="0998e-268">Klicka på **alla program** tooview en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="0998e-268">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="0998e-269">Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**</span><span class="sxs-lookup"><span data-stu-id="0998e-269">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="0998e-270">Välj hello-program som du har konfigurerat för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="0998e-270">Select hello application you have configured single sign-on.</span></span>

7.  <span data-ttu-id="0998e-271">När programmet hello läses in klickar du på hello **enkel inloggning** från hello programmet vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="0998e-271">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="0998e-272">Under hello **användarattribut** väljer hello Unik identifierare för användarna i hello **användar-ID** listrutan.</span><span class="sxs-lookup"><span data-stu-id="0998e-272">Under hello **User attributes** section, select hello unique identifier for your users in hello **User Identifier** dropdown.</span></span> <span data-ttu-id="0998e-273">hello måste markerade alternativet toomatch hello förväntat värde i hello programanvändare tooauthenticate hello.</span><span class="sxs-lookup"><span data-stu-id="0998e-273">hello selected option needs toomatch hello expected value in hello application tooauthenticate hello user.</span></span>

   >[!NOTE]
   ><span data-ttu-id="0998e-274">Azure AD väljer hello format för hello NameID attribut (användar-ID) baserat på valda hello värdet eller hello format som begärs av programmet hello i hello SAML AuthRequest.</span><span class="sxs-lookup"><span data-stu-id="0998e-274">Azure AD select hello format for hello NameID attribute (User Identifier) based on hello value selected or hello format requested by hello application in hello SAML AuthRequest.</span></span> <span data-ttu-id="0998e-275">Mer information finns i artikeln hello [enkel inloggning SAML protokoll](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) under hello avsnittet NameIDPolicy.</span><span class="sxs-lookup"><span data-stu-id="0998e-275">For more information visit hello article [Single Sign-On SAML protocol](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) under hello section NameIDPolicy.</span></span>
   >
   >

9.  <span data-ttu-id="0998e-276">tooadd användarattribut klickar du på **visa och redigera andra användarattribut** tooedit hello attribut toobe skickas toohello programmet hello SAML-token när användaren loggar in.</span><span class="sxs-lookup"><span data-stu-id="0998e-276">tooadd user attributes, click **View and edit all other user attributes** tooedit hello attributes toobe sent toohello application in hello SAML token when user sign in.</span></span>

   <span data-ttu-id="0998e-277">tooadd ett attribut:</span><span class="sxs-lookup"><span data-stu-id="0998e-277">tooadd an attribute:</span></span>

   <span data-ttu-id="0998e-278">1. Klicka **Lägg till attributet**.</span><span class="sxs-lookup"><span data-stu-id="0998e-278">1.click **Add attribute**.</span></span> <span data-ttu-id="0998e-279">Ange hello **namn** och hello väljer hello **värdet** hello listrutan.</span><span class="sxs-lookup"><span data-stu-id="0998e-279">Enter hello **Name** and hello select hello **Value** from hello dropdown.</span></span>

   <span data-ttu-id="0998e-280">2 Klicka **spara.**</span><span class="sxs-lookup"><span data-stu-id="0998e-280">2 Click **Save.**</span></span> <span data-ttu-id="0998e-281">Du kan se hello nytt attribut i hello tabell.</span><span class="sxs-lookup"><span data-stu-id="0998e-281">You see hello new attribute in hello table.</span></span>

### <a name="download-hello-azure-ad-metadata-or-certificate"></a><span data-ttu-id="0998e-282">Hämta metadata för hello Azure AD eller certifikat</span><span class="sxs-lookup"><span data-stu-id="0998e-282">Download hello Azure AD metadata or certificate</span></span>

<span data-ttu-id="0998e-283">toodownload hello programmetadata eller certifikat från Azure AD gör hello nedan:</span><span class="sxs-lookup"><span data-stu-id="0998e-283">toodownload hello application metadata or certificate from Azure AD, follow hello steps below:</span></span>

1.  <span data-ttu-id="0998e-284">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**</span><span class="sxs-lookup"><span data-stu-id="0998e-284">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="0998e-285">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="0998e-285">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="0998e-286">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="0998e-286">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="0998e-287">Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="0998e-287">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="0998e-288">Klicka på **alla program** tooview en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="0998e-288">click **All Applications** tooview a list of all your applications.</span></span>

   * <span data-ttu-id="0998e-289">Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**</span><span class="sxs-lookup"><span data-stu-id="0998e-289">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="0998e-290">Välj hello-program som du har konfigurerat för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="0998e-290">Select hello application you have configured single sign-on.</span></span>

7.  <span data-ttu-id="0998e-291">När programmet hello läses in klickar du på hello **enkel inloggning** från hello programmet vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="0998e-291">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="0998e-292">Gå för**SAML-signeringscertifikat** avsnittet och klicka sedan på **hämta** värde i kolumnen.</span><span class="sxs-lookup"><span data-stu-id="0998e-292">Go too**SAML Signing Certificate** section, then click **Download** column value.</span></span> <span data-ttu-id="0998e-293">Beroende på vilka hello-programmet kräver Konfigurera enkel inloggning, kan du se antingen hello alternativet toodownload hello XML-Metadata eller hello certifikat.</span><span class="sxs-lookup"><span data-stu-id="0998e-293">Depending on what hello application requires configuring single sign-on, you see either hello option toodownload hello Metadata XML or hello Certificate.</span></span>

    <span data-ttu-id="0998e-294">Azure AD Ange inte en URL tooget hello metadata.</span><span class="sxs-lookup"><span data-stu-id="0998e-294">Azure AD doesn’t provide a URL tooget hello metadata.</span></span> <span data-ttu-id="0998e-295">hello metadata kan endast hämtas som en XML-fil.</span><span class="sxs-lookup"><span data-stu-id="0998e-295">hello metadata can only be retrieved as a XML file.</span></span>

## <a name="how-tooconfigure-password-single-sign-on-for-an-azure-ad-gallery-application"></a><span data-ttu-id="0998e-296">Hur tooconfigure lösenord enkel inloggning för ett program för Azure AD-galleriet</span><span class="sxs-lookup"><span data-stu-id="0998e-296">How tooconfigure password single sign-on for an Azure AD gallery application</span></span>

<span data-ttu-id="0998e-297">tooconfigure ett program från hello Azure AD-galleriet måste du:</span><span class="sxs-lookup"><span data-stu-id="0998e-297">tooconfigure an application from hello Azure AD gallery you need to:</span></span>

-   [<span data-ttu-id="0998e-298">Lägga till ett program från hello Azure AD-galleriet</span><span class="sxs-lookup"><span data-stu-id="0998e-298">Add an application from hello Azure AD gallery</span></span>](#add-an-application)

-   [<span data-ttu-id="0998e-299">Konfigurera hello program för lösenord för enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="0998e-299">Configure hello application for password single sign-on</span></span>](#configure-the-application)

### <a name="add-an-application-from-hello-azure-ad-gallery"></a><span data-ttu-id="0998e-300">Lägga till ett program från hello Azure AD-galleriet</span><span class="sxs-lookup"><span data-stu-id="0998e-300">Add an application from hello Azure AD gallery</span></span>

<span data-ttu-id="0998e-301">tooadd ett program från hello Azure AD-galleriet så hello nedan:</span><span class="sxs-lookup"><span data-stu-id="0998e-301">tooadd an application from hello Azure AD Gallery, follow hello steps below:</span></span>

1.  <span data-ttu-id="0998e-302">Öppna hello [Azure Portal](https://portal.azure.com) och logga in som en **Global administratör** eller **medadministratör**</span><span class="sxs-lookup"><span data-stu-id="0998e-302">Open hello [Azure Portal](https://portal.azure.com) and sign in as a **Global Administrator** or **Co-admin**</span></span>

2.  <span data-ttu-id="0998e-303">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="0998e-303">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="0998e-304">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="0998e-304">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="0998e-305">Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="0998e-305">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="0998e-306">Klicka på hello **Lägg till** längst hello övre högra hörnet på hello **företagsprogram** bladet</span><span class="sxs-lookup"><span data-stu-id="0998e-306">click hello **Add** button at hello top-right corner on hello **Enterprise Applications** blade</span></span>

6.  <span data-ttu-id="0998e-307">I hello **anger du ett namn** textruta från hello **Lägg till från galleriet hello** avsnitt, hello-typnamn för hello program</span><span class="sxs-lookup"><span data-stu-id="0998e-307">In hello **Enter a name** textbox from hello **Add from hello gallery** section, type hello name of hello application</span></span>

7.  <span data-ttu-id="0998e-308">Välj hello-program som du vill använda tooconfigure för enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="0998e-308">Select hello application you want tooconfigure for single sign-on</span></span>

8.  <span data-ttu-id="0998e-309">Innan du lägger till programmet hello kan du ändra dess namn från hello **namn** textruta.</span><span class="sxs-lookup"><span data-stu-id="0998e-309">Before adding hello application, you can change its name from hello **Name** textbox.</span></span>

9.  <span data-ttu-id="0998e-310">Klicka på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="0998e-310">Click **Add** button, tooadd hello application.</span></span>

<span data-ttu-id="0998e-311">Efter en kort period vara kan toosee hello programmets konfiguration bladet.</span><span class="sxs-lookup"><span data-stu-id="0998e-311">After a short period, you be able toosee hello application’s configuration blade.</span></span>

### <a name="configure-hello-application-for-password-single-sign-on"></a><span data-ttu-id="0998e-312">Konfigurera hello program för lösenord för enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="0998e-312">Configure hello application for password single sign-on</span></span>

<span data-ttu-id="0998e-313">tooconfigure enkel inloggning för ett program gör hello nedan:</span><span class="sxs-lookup"><span data-stu-id="0998e-313">tooconfigure single sign-on for an application, follow hello steps below:</span></span>

1.  <span data-ttu-id="0998e-314">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**</span><span class="sxs-lookup"><span data-stu-id="0998e-314">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="0998e-315">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="0998e-315">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="0998e-316">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="0998e-316">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="0998e-317">Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="0998e-317">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="0998e-318">Klicka på **alla program** tooview en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="0998e-318">click **All Applications** tooview a list of all your applications.</span></span>

 * <span data-ttu-id="0998e-319">Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**</span><span class="sxs-lookup"><span data-stu-id="0998e-319">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="0998e-320">Välj hello-program som du vill tooconfigure enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="0998e-320">Select hello application you want tooconfigure single sign-on</span></span>

7.  <span data-ttu-id="0998e-321">När programmet hello läses in klickar du på hello **enkel inloggning** från hello programmet vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="0998e-321">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="0998e-322">Välj hello läge **lösenordsbaserade inloggning.**</span><span class="sxs-lookup"><span data-stu-id="0998e-322">Select hello mode **Password-based Sign-on.**</span></span>

9.  <span data-ttu-id="0998e-323">Tilldela användare toohello program.</span><span class="sxs-lookup"><span data-stu-id="0998e-323">Assign users toohello application.</span></span>

10. <span data-ttu-id="0998e-324">Du kan dessutom också ange autentiseringsuppgifter för hello användares räkning genom att markera rader hello hello användare och klicka på **referenser uppdatering** och ange hello användarnamn och lösenord för åt hello användare.</span><span class="sxs-lookup"><span data-stu-id="0998e-324">Additionally, you can also provide credentials on behalf of hello user by selecting hello rows of hello users and clicking on **Update Credentials** and entering hello username and password on behalf of hello users.</span></span> <span data-ttu-id="0998e-325">Annars kommer att användare tillfrågas tooenter hello autentiseringsuppgifter sig vid start.</span><span class="sxs-lookup"><span data-stu-id="0998e-325">Otherwise, users be prompted tooenter hello credentials themselves upon launch.</span></span>

## <a name="how-tooconfigure-password-single-sign-on-for-a-non-gallery-application"></a><span data-ttu-id="0998e-326">Hur tooconfigure lösenord enkel inloggning för ett program för icke-galleriet</span><span class="sxs-lookup"><span data-stu-id="0998e-326">How tooconfigure password single sign-on for a non-gallery application</span></span>

<span data-ttu-id="0998e-327">tooconfigure ett program från hello Azure AD-galleriet måste du:</span><span class="sxs-lookup"><span data-stu-id="0998e-327">tooconfigure an application from hello Azure AD gallery you need to:</span></span>

-   [<span data-ttu-id="0998e-328">Lägga till ett icke-galleriet program</span><span class="sxs-lookup"><span data-stu-id="0998e-328">Add a non-gallery application</span></span>](#add-a-non-gallery-application)

-   [<span data-ttu-id="0998e-329">Konfigurera hello program för lösenord för enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="0998e-329">Configure hello application for password single sign-on</span></span>](#configure-the-application-for-password-single-sign-on)

### <a name="add-a-non-gallery-application"></a><span data-ttu-id="0998e-330">Lägga till ett icke-galleriet program</span><span class="sxs-lookup"><span data-stu-id="0998e-330">Add a non-gallery application</span></span>

<span data-ttu-id="0998e-331">tooadd ett program från hello Azure AD-galleriet så hello nedan:</span><span class="sxs-lookup"><span data-stu-id="0998e-331">tooadd an application from hello Azure AD Gallery, follow hello steps below:</span></span>

1.  <span data-ttu-id="0998e-332">Öppna hello [Azure Portal](https://portal.azure.com) och logga in som en **Global administratör** eller **medadministratör**</span><span class="sxs-lookup"><span data-stu-id="0998e-332">Open hello [Azure Portal](https://portal.azure.com) and sign in as a **Global Administrator** or **Co-admin**</span></span>

2.  <span data-ttu-id="0998e-333">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="0998e-333">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="0998e-334">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="0998e-334">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="0998e-335">Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="0998e-335">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="0998e-336">Klicka på hello **Lägg till** längst hello övre högra hörnet på hello **företagsprogram** bladet</span><span class="sxs-lookup"><span data-stu-id="0998e-336">click hello **Add** button at hello top-right corner on hello **Enterprise Applications** blade</span></span>

6.  <span data-ttu-id="0998e-337">Klicka på **icke-galleriet program.**</span><span class="sxs-lookup"><span data-stu-id="0998e-337">click **Non-gallery application.**</span></span>

7.  <span data-ttu-id="0998e-338">Ange hello namnet på ditt program i hello **namn** textruta.</span><span class="sxs-lookup"><span data-stu-id="0998e-338">Enter hello name of your application in hello **Name** textbox.</span></span> <span data-ttu-id="0998e-339">Välj **lägga till.**</span><span class="sxs-lookup"><span data-stu-id="0998e-339">Select **Add.**</span></span>

<span data-ttu-id="0998e-340">Efter en kort period vara kan toosee hello programmets konfiguration bladet.</span><span class="sxs-lookup"><span data-stu-id="0998e-340">After a short period, you be able toosee hello application’s configuration blade.</span></span>

### <a name="configure-hello-application-for-password-single-sign-on"></a><span data-ttu-id="0998e-341">Konfigurera hello program för lösenord för enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="0998e-341">Configure hello application for password single sign-on</span></span>

<span data-ttu-id="0998e-342">tooconfigure enkel inloggning för ett program gör hello nedan:</span><span class="sxs-lookup"><span data-stu-id="0998e-342">tooconfigure single sign-on for an application, follow hello steps below:</span></span>

1.  <span data-ttu-id="0998e-343">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**</span><span class="sxs-lookup"><span data-stu-id="0998e-343">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="0998e-344">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="0998e-344">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="0998e-345">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="0998e-345">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="0998e-346">Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="0998e-346">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="0998e-347">Klicka på **alla program** tooview en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="0998e-347">click **All Applications** tooview a list of all your applications.</span></span>

 * <span data-ttu-id="0998e-348">Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**</span><span class="sxs-lookup"><span data-stu-id="0998e-348">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="0998e-349">Välj hello-program som du vill tooconfigure enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="0998e-349">Select hello application you want tooconfigure single sign-on.</span></span>

7.  <span data-ttu-id="0998e-350">När programmet hello läses in klickar du på hello **enkel inloggning** från hello programmet vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="0998e-350">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="0998e-351">Välj hello läge **lösenordsbaserade inloggning.**</span><span class="sxs-lookup"><span data-stu-id="0998e-351">Select hello mode **Password-based Sign-on.**</span></span>

9.  <span data-ttu-id="0998e-352">Ange hello **inloggnings-URL**.</span><span class="sxs-lookup"><span data-stu-id="0998e-352">Enter hello **Sign-on URL**.</span></span> <span data-ttu-id="0998e-353">Det här är hello URL där användarna anger sina användarnamn och lösenord toosign i till.</span><span class="sxs-lookup"><span data-stu-id="0998e-353">This is hello URL where users enter their username and password toosign in to.</span></span> <span data-ttu-id="0998e-354">Kontrollera hello inloggning fält är synliga på hello-URL.</span><span class="sxs-lookup"><span data-stu-id="0998e-354">Ensure hello sign in fields are visible at hello URL.</span></span>

10. <span data-ttu-id="0998e-355">Tilldela användare toohello program.</span><span class="sxs-lookup"><span data-stu-id="0998e-355">Assign users toohello application.</span></span>

11. <span data-ttu-id="0998e-356">Du kan dessutom också ange autentiseringsuppgifter för hello användares räkning genom att markera rader hello hello användare och klicka på **referenser uppdatering** och ange hello användarnamn och lösenord för åt hello användare.</span><span class="sxs-lookup"><span data-stu-id="0998e-356">Additionally, you can also provide credentials on behalf of hello user by selecting hello rows of hello users and clicking on **Update Credentials** and entering hello username and password on behalf of hello users.</span></span> <span data-ttu-id="0998e-357">Annars kommer att användare tillfrågas tooenter hello autentiseringsuppgifter sig vid start.</span><span class="sxs-lookup"><span data-stu-id="0998e-357">Otherwise, users be prompted tooenter hello credentials themselves upon launch.</span></span>

## <a name="how-tooassign-a-user-tooan-application-directly"></a><span data-ttu-id="0998e-358">Hur tooassign en tooan användarprogram direkt</span><span class="sxs-lookup"><span data-stu-id="0998e-358">How tooassign a user tooan application directly</span></span>

<span data-ttu-id="0998e-359">tooassign en eller flera användare tooan programmet direkt, gör hello nedan:</span><span class="sxs-lookup"><span data-stu-id="0998e-359">tooassign one or more users tooan application directly, follow hello steps below:</span></span>

1.  <span data-ttu-id="0998e-360">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="0998e-360">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="0998e-361">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="0998e-361">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="0998e-362">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="0998e-362">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="0998e-363">Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="0998e-363">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="0998e-364">Klicka på **alla program** tooview en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="0998e-364">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="0998e-365">Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**</span><span class="sxs-lookup"><span data-stu-id="0998e-365">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="0998e-366">Välj hello-program som du vill tooassign en toofrom hello-användarlistan.</span><span class="sxs-lookup"><span data-stu-id="0998e-366">Select hello application you want tooassign a user toofrom hello list.</span></span>

7.  <span data-ttu-id="0998e-367">När programmet hello läses in klickar du på **användare och grupper** från hello programmet vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="0998e-367">Once hello application loads, click **Users and Groups** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="0998e-368">Klicka på hello **Lägg till** knappen ovanpå hello **användare och grupper** lista tooopen hello **Lägg uppdrag** bladet.</span><span class="sxs-lookup"><span data-stu-id="0998e-368">Click hello **Add** button on top of hello **Users and Groups** list tooopen hello **Add Assignment** blade.</span></span>

9.  <span data-ttu-id="0998e-369">Klicka på hello **användare och grupper** selector från hello **Lägg uppdrag** bladet.</span><span class="sxs-lookup"><span data-stu-id="0998e-369">click hello **Users and groups** selector from hello **Add Assignment** blade.</span></span>

10. <span data-ttu-id="0998e-370">Typen i hello **fullständigt namn** eller **e-postadress** för hello-användare som du vill tilldela till hello **Sök efter namn eller e-postadress** sökrutan.</span><span class="sxs-lookup"><span data-stu-id="0998e-370">Type in hello **full name** or **email address** of hello user you are interested in assigning into hello **Search by name or email address** search box.</span></span>

11. <span data-ttu-id="0998e-371">Hovra över hello **användare** i hello listan tooreveal en **kryssrutan**.</span><span class="sxs-lookup"><span data-stu-id="0998e-371">Hover over hello **user** in hello list tooreveal a **checkbox**.</span></span> <span data-ttu-id="0998e-372">Klicka på hello kryssrutan nästa toohello användarens profil foto eller logotypen tooadd användaren-toohello **valda** lista.</span><span class="sxs-lookup"><span data-stu-id="0998e-372">Click hello checkbox next toohello user’s profile photo or logo tooadd your user toohello **Selected** list.</span></span>

12. <span data-ttu-id="0998e-373">**Valfritt:** om du vill ha för**lägga till fler än en användare**, typ i en annan **fullständigt namn** eller **e-postadress** till hello **Sök efter namn eller e-postadress** sökrutan och klicka på hello kryssrutan tooadd den här användaren toohello **valda** lista.</span><span class="sxs-lookup"><span data-stu-id="0998e-373">**Optional:** If you would like too**add more than one user**, type in another **full name** or **email address** into hello **Search by name or email address** search box, and click hello checkbox tooadd this user toohello **Selected** list.</span></span>

13. <span data-ttu-id="0998e-374">När du har valt användare klickar du på hello **Välj** knappen tooadd dem toohello lista över användare och grupper toobe tilldelat toohello program.</span><span class="sxs-lookup"><span data-stu-id="0998e-374">When you are finished selecting users, click hello **Select** button tooadd them toohello list of users and groups toobe assigned toohello application.</span></span>

14. <span data-ttu-id="0998e-375">**Valfritt:** klickar du på hello **Välj roll** Väljaren i hello **Lägg uppdrag** bladet tooselect en roll tooassign toohello användare som du har valt.</span><span class="sxs-lookup"><span data-stu-id="0998e-375">**Optional:** click hello **Select Role** selector in hello **Add Assignment** blade tooselect a role tooassign toohello users you have selected.</span></span>

15. <span data-ttu-id="0998e-376">Klicka på hello **tilldela** knappen tooassign hello programmet toohello markerade användare.</span><span class="sxs-lookup"><span data-stu-id="0998e-376">Click hello **Assign** button tooassign hello application toohello selected users.</span></span>

<span data-ttu-id="0998e-377">Efter en kort period hello-användare som du har valt att kan toolaunch programmen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="0998e-377">After a short period, hello users you have selected be able toolaunch these applications in hello Access Panel.</span></span>

## <a name="if-these-troubleshooting-steps-do-not-hello-resolve-hello-issue"></a><span data-ttu-id="0998e-378">Om de här åtgärderna inte hello problemet hello</span><span class="sxs-lookup"><span data-stu-id="0998e-378">If these troubleshooting steps do not hello resolve hello issue</span></span>

<span data-ttu-id="0998e-379">Öppna ett supportärende med hello efter information om tillgängliga:</span><span class="sxs-lookup"><span data-stu-id="0998e-379">open a support ticket with hello following information if available:</span></span>

-   <span data-ttu-id="0998e-380">Fel-ID för korrelation</span><span class="sxs-lookup"><span data-stu-id="0998e-380">Correlation error ID</span></span>

-   <span data-ttu-id="0998e-381">UPN (användarens e-postadress)</span><span class="sxs-lookup"><span data-stu-id="0998e-381">UPN (user email address)</span></span>

-   <span data-ttu-id="0998e-382">Klient-ID</span><span class="sxs-lookup"><span data-stu-id="0998e-382">TenantID</span></span>

-   <span data-ttu-id="0998e-383">Typ av webbläsare</span><span class="sxs-lookup"><span data-stu-id="0998e-383">Browser type</span></span>

-   <span data-ttu-id="0998e-384">Tidszon och tid/tidsperioden under fel inträffar</span><span class="sxs-lookup"><span data-stu-id="0998e-384">Time zone and time/timeframe during error occurs</span></span>

-   <span data-ttu-id="0998e-385">Fiddler spårningar</span><span class="sxs-lookup"><span data-stu-id="0998e-385">Fiddler traces</span></span>

## <a name="next-steps"></a><span data-ttu-id="0998e-386">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0998e-386">Next steps</span></span>
[<span data-ttu-id="0998e-387">Tillhandahålla enkel inloggning tooyour appar med Application Proxy</span><span class="sxs-lookup"><span data-stu-id="0998e-387">Provide single sign-on tooyour apps with Application Proxy</span></span>](active-directory-application-proxy-sso-using-kcd.md)

