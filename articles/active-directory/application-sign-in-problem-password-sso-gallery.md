---
title: "aaaProblems inloggning tooan Azure AD-galleriet program konfigurerade för federerad enkel inloggning | Microsoft Docs"
description: "Hur tootroubleshoot problem med Azure AD-galleriet program som har konfigurerats för lösenord för enkel inloggning"
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
ms.openlocfilehash: 7ba28765e1d1f13025d740790a2f87654ae0daed
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="problems-signing-in-tooan-azure-ad-gallery-application-configured-for-federated-single-sign-on"></a><span data-ttu-id="6c33d-103">Problem med att logga i tooan Galleri för Azure AD-program som konfigurerats för federerad enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="6c33d-103">Problems signing in tooan Azure AD Gallery application configured for federated single sign-on</span></span>

<span data-ttu-id="6c33d-104">Hej åtkomstpanelen är en webbaserad portal som aktiverar en användare som har ett arbets- eller skolkonto konto i Azure Active Directory (AD Azure) tooview och starta molnbaserade program att hello Azure AD-administratör har gett dem åtkomst till.</span><span class="sxs-lookup"><span data-stu-id="6c33d-104">hello Access Panel is a web-based portal which enables a user who has a work or school account in Azure Active Directory (Azure AD) tooview and launch cloud-based applications that hello Azure AD administrator has granted them access to.</span></span> <span data-ttu-id="6c33d-105">En användare med Azure AD-versioner kan också använda självbetjäning och funktioner för hantering av appen via hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="6c33d-105">A user who has Azure AD editions can also use self-service group and app management capabilities through hello Access Panel.</span></span> <span data-ttu-id="6c33d-106">Hej åtkomstpanelen skiljer sig från hello Azure-portalen och kräver inte användare toohave en Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="6c33d-106">hello Access Panel is separate from hello Azure portal and does not require users toohave an Azure subscription.</span></span>

<span data-ttu-id="6c33d-107">toouse lösenordsbaserade enkel inloggning (SSO) i hello åtkomstpanelen hello åtkomstpanelen tillägget måste vara installerad i hello användarens webbläsare.</span><span class="sxs-lookup"><span data-stu-id="6c33d-107">toouse password-based single sign-on (SSO) in hello Access Panel, hello Access Panel extension must be installed in hello user’s browser.</span></span> <span data-ttu-id="6c33d-108">Det här tillägget laddas ned automatiskt när en användare väljer ett program som har konfigurerats för lösenordsbaserad enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="6c33d-108">This extension is downloaded automatically when a user selects an application that is configured for password-based SSO.</span></span>

## <a name="meeting-browser-requirements-for-hello-access-panel"></a><span data-ttu-id="6c33d-109">Möte Webbläsarkrav för hello åtkomstpanelen</span><span class="sxs-lookup"><span data-stu-id="6c33d-109">Meeting browser requirements for hello Access Panel</span></span>

<span data-ttu-id="6c33d-110">Hej åtkomstpanelen kräver en webbläsare som stöder JavaScript och har aktiverat CSS.</span><span class="sxs-lookup"><span data-stu-id="6c33d-110">hello Access Panel requires a browser that supports JavaScript and has CSS enabled.</span></span> <span data-ttu-id="6c33d-111">toouse lösenordsbaserade enkel inloggning (SSO) i hello åtkomstpanelen hello åtkomstpanelen tillägget måste vara installerad i hello användarens webbläsare.</span><span class="sxs-lookup"><span data-stu-id="6c33d-111">toouse password-based single sign-on (SSO) in hello Access Panel, hello Access Panel extension must be installed in hello user’s browser.</span></span> <span data-ttu-id="6c33d-112">Det här tillägget laddas ned automatiskt när en användare väljer ett program som har konfigurerats för lösenordsbaserad enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="6c33d-112">This extension is downloaded automatically when a user selects an application that is configured for password-based SSO.</span></span>

<span data-ttu-id="6c33d-113">För lösenordsbaserad SSO kan hello användarens webbläsare vara:</span><span class="sxs-lookup"><span data-stu-id="6c33d-113">For password-based SSO, hello end user’s browsers can be:</span></span>

-   <span data-ttu-id="6c33d-114">Internet Explorer 8, 9, 10, 11 - på Windows 7 eller senare</span><span class="sxs-lookup"><span data-stu-id="6c33d-114">Internet Explorer 8, 9, 10, 11 - on Windows 7 or later</span></span>

-   <span data-ttu-id="6c33d-115">Chrome - på Windows 7 eller senare, och i MacOS X eller senare</span><span class="sxs-lookup"><span data-stu-id="6c33d-115">Chrome - on Windows 7 or later, and on MacOS X or later</span></span>

-   <span data-ttu-id="6c33d-116">Firefox 26.0 eller senare - på Windows XP SP2 eller senare, och på Mac OS X 10,6 eller senare</span><span class="sxs-lookup"><span data-stu-id="6c33d-116">Firefox 26.0 or later - on Windows XP SP2 or later, and on Mac OS X 10.6 or later</span></span>

>[!NOTE]
><span data-ttu-id="6c33d-117">hello lösenordsbaserade SSO tillägget blir tillgängliga för kvalificerade i Windows 10 när webbläsartillägg blir stöd för kant.</span><span class="sxs-lookup"><span data-stu-id="6c33d-117">hello password-based SSO extension become available for Edge in Windows 10 when browser extensions become supported for Edge.</span></span>
>
>

## <a name="how-tooinstall-hello-access-panel-browser-extension"></a><span data-ttu-id="6c33d-118">Hur tooinstall hello åtkomst panelen webbläsartillägg</span><span class="sxs-lookup"><span data-stu-id="6c33d-118">How tooinstall hello Access Panel Browser extension</span></span>

<span data-ttu-id="6c33d-119">tooinstall hello webbläsartillägget för åtkomst till Kontrollpanelen, så hello nedan:</span><span class="sxs-lookup"><span data-stu-id="6c33d-119">tooinstall hello Access Panel Browser extension, follow hello steps below:</span></span>

1.  <span data-ttu-id="6c33d-120">Öppna hello [åtkomstpanelen](https://myapps.microsoft.com) i någon av hello stöds webbläsare och logga in som en **användaren** i din Azure AD.</span><span class="sxs-lookup"><span data-stu-id="6c33d-120">Open hello [Access Panel](https://myapps.microsoft.com) in one of hello supported browsers and sign in as a **user** in your Azure AD.</span></span>

2.  <span data-ttu-id="6c33d-121">Klicka på en **lösenord SSO-program** i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="6c33d-121">click a **password-SSO application** in hello Access Panel.</span></span>

3.  <span data-ttu-id="6c33d-122">I hello fråga frågar tooinstall hello programvara, väljer **installera nu**.</span><span class="sxs-lookup"><span data-stu-id="6c33d-122">In hello prompt asking tooinstall hello software, select **Install Now**.</span></span>

4.  <span data-ttu-id="6c33d-123">Baserat på din webbläsare vara du riktad toohello länken.</span><span class="sxs-lookup"><span data-stu-id="6c33d-123">Based on your browser you be directed toohello download link.</span></span> <span data-ttu-id="6c33d-124">**Lägg till** hello tillägget tooyour webbläsare.</span><span class="sxs-lookup"><span data-stu-id="6c33d-124">**Add** hello extension tooyour browser.</span></span>

5.  <span data-ttu-id="6c33d-125">Om din webbläsare frågar väljer tooeither **aktivera** eller **Tillåt** hello tillägg.</span><span class="sxs-lookup"><span data-stu-id="6c33d-125">If your browser asks, select tooeither **Enable** or **Allow** hello extension.</span></span>

6.  <span data-ttu-id="6c33d-126">När den har installerats, **starta om** webbläsarsessionen.</span><span class="sxs-lookup"><span data-stu-id="6c33d-126">Once installed, **restart** your browser session.</span></span>

7.  <span data-ttu-id="6c33d-127">Logga in till hello åtkomstpanelen och se om kan du **starta** lösenord SSO-program</span><span class="sxs-lookup"><span data-stu-id="6c33d-127">Sign in into hello Access Panel and see if you can **launch** your password-SSO applications</span></span>

<span data-ttu-id="6c33d-128">Du kan också hämta hello-tillägget för Chrome och Firefox från hello Direktlänkar nedan:</span><span class="sxs-lookup"><span data-stu-id="6c33d-128">You may also download hello extension for Chrome and Firefox from hello direct links below:</span></span>

-   [<span data-ttu-id="6c33d-129">Tillägget för Chrome åtkomst panelen</span><span class="sxs-lookup"><span data-stu-id="6c33d-129">Chrome Access Panel Extension</span></span>](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [<span data-ttu-id="6c33d-130">Tillägget för Firefox åtkomst panelen</span><span class="sxs-lookup"><span data-stu-id="6c33d-130">Firefox Access Panel Extension</span></span>](https://addons.mozilla.org/firefox/addon/access-panel-extension/)

## <a name="setting-up-a-group-policy-for-internet-explorer"></a><span data-ttu-id="6c33d-131">Konfigurera en grupprincip för Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="6c33d-131">Setting up a group policy for Internet Explorer</span></span>

<span data-ttu-id="6c33d-132">Du kan konfigurera en grupprincip som gör att du tooremotely installera hello åtkomstpanelen tillägg för Internet Explorer på användarnas datorer.</span><span class="sxs-lookup"><span data-stu-id="6c33d-132">You can setup a group policy that allow you tooremotely install hello Access Panel extension for Internet Explorer on your users' machines.</span></span>

<span data-ttu-id="6c33d-133">omfattar hello krav:</span><span class="sxs-lookup"><span data-stu-id="6c33d-133">hello prerequisites include:</span></span>

-   <span data-ttu-id="6c33d-134">Du har ställt in [Active Directory Domain Services](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx), och du har anslutit användarnas datorer tooyour domän.</span><span class="sxs-lookup"><span data-stu-id="6c33d-134">You have set up [Active Directory Domain Services](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx), and you have joined your users' machines tooyour domain.</span></span>

-   <span data-ttu-id="6c33d-135">Du måste ha hello ”redigera inställningar för” behörighet tooedit hello grupprincipobjektet (GPO).</span><span class="sxs-lookup"><span data-stu-id="6c33d-135">You must have hello "Edit settings" permission tooedit hello Group Policy Object (GPO).</span></span> <span data-ttu-id="6c33d-136">Som standard medlemmar i hello följande säkerhetsgrupper har denna behörighet: Domänadministratörer, Företagsadministratörer och skapare och ägare av Grupprincip.</span><span class="sxs-lookup"><span data-stu-id="6c33d-136">By default, members of hello following security groups have this permission: Domain Administrators, Enterprise Administrators, and Group Policy Creator Owners.</span></span> <span data-ttu-id="6c33d-137">[Läs mer](https://technet.microsoft.com/library/cc781991%28v=ws.10%29.aspx).</span><span class="sxs-lookup"><span data-stu-id="6c33d-137">[Learn more](https://technet.microsoft.com/library/cc781991%28v=ws.10%29.aspx).</span></span>

<span data-ttu-id="6c33d-138">Följ hello kursen [hur tooDeploy hello Access panelen-tillägg för Internet Explorer med hjälp av Grupprincip](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ie-group-policy) steg-för-steg-instruktioner för hur tooconfigure hello Grupprincip och distribuera den toousers.</span><span class="sxs-lookup"><span data-stu-id="6c33d-138">Follow hello tutorial [How tooDeploy hello Access Panel Extension for Internet Explorer using Group Policy](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ie-group-policy) for step by step instructions on how tooconfigure hello group policy and deploy it toousers.</span></span>

## <a name="troubleshoot-hello-access-panel-in-internet-explorer"></a><span data-ttu-id="6c33d-139">Felsöka hello åtkomstpanelen i Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="6c33d-139">Troubleshoot hello Access Panel in Internet Explorer</span></span>

<span data-ttu-id="6c33d-140">Följ hello [Felsök hello Access-tillägg för Internet Explorer på panelen](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ie-Troubleshoot) guide för åtkomst till en diagnostik och steg-för-steg-instruktioner om hur du konfigurerar hello-tillägg för Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="6c33d-140">Follow hello [Troubleshoot hello Access Panel Extension for Internet Explorer](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ie-Troubleshoot) guide for access a diagnostics tool and step by step instructions on configuring hello extension for IE.</span></span>

## <a name="how-tooconfigure-password-single-sign-on-for-an-azure-ad-gallery-application"></a><span data-ttu-id="6c33d-141">Hur tooconfigure lösenord enkel inloggning för ett program för Azure AD-galleriet</span><span class="sxs-lookup"><span data-stu-id="6c33d-141">How tooconfigure password single sign-on for an Azure AD gallery application</span></span>

<span data-ttu-id="6c33d-142">tooconfigure ett program från hello Azure AD-galleriet måste du:</span><span class="sxs-lookup"><span data-stu-id="6c33d-142">tooconfigure an application from hello Azure AD gallery you need to:</span></span>

-   [<span data-ttu-id="6c33d-143">Lägga till ett program från hello Azure AD-galleriet</span><span class="sxs-lookup"><span data-stu-id="6c33d-143">Add an application from hello Azure AD gallery</span></span>](#_Add_an_application)

-   [<span data-ttu-id="6c33d-144">Konfigurera hello program för lösenord för enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="6c33d-144">Configure hello application for password single sign-on</span></span>](#configure-the-application-for-password-single-sign-on)

-   [<span data-ttu-id="6c33d-145">Tilldela användare toohello program</span><span class="sxs-lookup"><span data-stu-id="6c33d-145">Assign users toohello application</span></span>](#assign-users-to-the-application)

### <a name="add-an-application-from-hello-azure-ad-gallery"></a><span data-ttu-id="6c33d-146">Lägga till ett program från hello Azure AD-galleriet</span><span class="sxs-lookup"><span data-stu-id="6c33d-146">Add an application from hello Azure AD gallery</span></span>

<span data-ttu-id="6c33d-147">tooadd ett program från hello Azure AD-galleriet så hello nedan:</span><span class="sxs-lookup"><span data-stu-id="6c33d-147">tooadd an application from hello Azure AD Gallery, follow hello steps below:</span></span>

1.  <span data-ttu-id="6c33d-148">Öppna hello [Azure Portal](https://portal.azure.com) och logga in som en **Global administratör** eller **medadministratör**</span><span class="sxs-lookup"><span data-stu-id="6c33d-148">Open hello [Azure Portal](https://portal.azure.com) and sign in as a **Global Administrator** or **Co-admin**</span></span>

2.  <span data-ttu-id="6c33d-149">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="6c33d-149">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="6c33d-150">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="6c33d-150">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="6c33d-151">Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="6c33d-151">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="6c33d-152">Klicka på hello **Lägg till** längst hello övre högra hörnet på hello **företagsprogram** bladet.</span><span class="sxs-lookup"><span data-stu-id="6c33d-152">click hello **Add** button at hello top-right corner on hello **Enterprise Applications** blade.</span></span>

6.  <span data-ttu-id="6c33d-153">I hello **anger du ett namn** textruta från hello **Lägg till från galleriet hello** avsnitt, hello-typnamn för hello program.</span><span class="sxs-lookup"><span data-stu-id="6c33d-153">In hello **Enter a name** textbox from hello **Add from hello gallery** section, type hello name of hello application.</span></span>

7.  <span data-ttu-id="6c33d-154">Välj hello-program som du vill använda tooconfigure för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="6c33d-154">Select hello application you want tooconfigure for single sign-on.</span></span>

8.  <span data-ttu-id="6c33d-155">Innan du lägger till programmet hello kan du ändra dess namn från hello **namn** textruta.</span><span class="sxs-lookup"><span data-stu-id="6c33d-155">Before adding hello application, you can change its name from hello **Name** textbox.</span></span>

9.  <span data-ttu-id="6c33d-156">Klicka på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="6c33d-156">Click **Add** button, tooadd hello application.</span></span>

<span data-ttu-id="6c33d-157">Efter en kort period vara kan toosee hello programmets konfiguration bladet.</span><span class="sxs-lookup"><span data-stu-id="6c33d-157">After a short period, you be able toosee hello application’s configuration blade.</span></span>

### <a name="configure-hello-application-for-password-single-sign-on"></a><span data-ttu-id="6c33d-158">Konfigurera hello program för lösenord för enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="6c33d-158">Configure hello application for password single sign-on</span></span>

<span data-ttu-id="6c33d-159">tooconfigure enkel inloggning för ett program gör hello nedan:</span><span class="sxs-lookup"><span data-stu-id="6c33d-159">tooconfigure single sign-on for an application, follow hello steps below:</span></span>

1.  <span data-ttu-id="6c33d-160">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**</span><span class="sxs-lookup"><span data-stu-id="6c33d-160">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="6c33d-161">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="6c33d-161">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="6c33d-162">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="6c33d-162">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="6c33d-163">Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="6c33d-163">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="6c33d-164">Klicka på **alla program** tooview en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="6c33d-164">click **All Applications** tooview a list of all your applications.</span></span>

   * <span data-ttu-id="6c33d-165">Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**</span><span class="sxs-lookup"><span data-stu-id="6c33d-165">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="6c33d-166">Välj hello-program som du vill tooconfigure enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="6c33d-166">Select hello application you want tooconfigure single sign-on</span></span>

7.  <span data-ttu-id="6c33d-167">När programmet hello läses in klickar du på hello **enkel inloggning** från hello programmet vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="6c33d-167">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="6c33d-168">Välj hello läge **lösenordsbaserade inloggning.**</span><span class="sxs-lookup"><span data-stu-id="6c33d-168">Select hello mode **Password-based Sign-on.**</span></span>

9.  <span data-ttu-id="6c33d-169">[Tilldela användare toohello program](#_How_to_assign).</span><span class="sxs-lookup"><span data-stu-id="6c33d-169">[Assign users toohello application](#_How_to_assign).</span></span>

10. <span data-ttu-id="6c33d-170">Du kan dessutom också ange autentiseringsuppgifter för hello användares räkning genom att markera rader hello hello användare och klicka på **referenser uppdatering** och ange hello användarnamn och lösenord för åt hello användare.</span><span class="sxs-lookup"><span data-stu-id="6c33d-170">Additionally, you can also provide credentials on behalf of hello user by selecting hello rows of hello users and clicking on **Update Credentials** and entering hello username and password on behalf of hello users.</span></span> <span data-ttu-id="6c33d-171">Annars kommer att användare tillfrågas tooenter hello autentiseringsuppgifter sig vid start.</span><span class="sxs-lookup"><span data-stu-id="6c33d-171">Otherwise, users be prompted tooenter hello credentials themselves upon launch.</span></span>

### <a name="assign-users-toohello-application"></a><span data-ttu-id="6c33d-172">Tilldela användare toohello program</span><span class="sxs-lookup"><span data-stu-id="6c33d-172">Assign users toohello application</span></span>

<span data-ttu-id="6c33d-173">tooassign en eller flera användare tooan programmet direkt, gör hello nedan:</span><span class="sxs-lookup"><span data-stu-id="6c33d-173">tooassign one or more users tooan application directly, follow hello steps below:</span></span>

1.  <span data-ttu-id="6c33d-174">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="6c33d-174">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="6c33d-175">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="6c33d-175">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="6c33d-176">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="6c33d-176">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="6c33d-177">Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="6c33d-177">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="6c33d-178">Klicka på **alla program** tooview en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="6c33d-178">click **All Applications** tooview a list of all your applications.</span></span>

   * <span data-ttu-id="6c33d-179">Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**</span><span class="sxs-lookup"><span data-stu-id="6c33d-179">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="6c33d-180">Välj hello-program som du vill tooassign en toofrom hello-användarlistan.</span><span class="sxs-lookup"><span data-stu-id="6c33d-180">Select hello application you want tooassign a user toofrom hello list.</span></span>

7.  <span data-ttu-id="6c33d-181">När programmet hello läses in klickar du på **användare och grupper** från hello programmet vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="6c33d-181">Once hello application loads, click **Users and Groups** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="6c33d-182">Klicka på hello **Lägg till** knappen ovanpå hello **användare och grupper** lista tooopen hello **Lägg uppdrag** bladet.</span><span class="sxs-lookup"><span data-stu-id="6c33d-182">Click hello **Add** button on top of hello **Users and Groups** list tooopen hello **Add Assignment** blade.</span></span>

9.  <span data-ttu-id="6c33d-183">Klicka på hello **användare och grupper** selector från hello **Lägg uppdrag** bladet.</span><span class="sxs-lookup"><span data-stu-id="6c33d-183">click hello **Users and groups** selector from hello **Add Assignment** blade.</span></span>

10. <span data-ttu-id="6c33d-184">Typen i hello **fullständigt namn** eller **e-postadress** för hello-användare som du vill tilldela till hello **Sök efter namn eller e-postadress** sökrutan.</span><span class="sxs-lookup"><span data-stu-id="6c33d-184">Type in hello **full name** or **email address** of hello user you are interested in assigning into hello **Search by name or email address** search box.</span></span>

11. <span data-ttu-id="6c33d-185">Hovra över hello **användare** i hello listan tooreveal en **kryssrutan**.</span><span class="sxs-lookup"><span data-stu-id="6c33d-185">Hover over hello **user** in hello list tooreveal a **checkbox**.</span></span> <span data-ttu-id="6c33d-186">Klicka på hello kryssrutan nästa toohello användarens profil foto eller logotypen tooadd användaren-toohello **valda** lista.</span><span class="sxs-lookup"><span data-stu-id="6c33d-186">Click hello checkbox next toohello user’s profile photo or logo tooadd your user toohello **Selected** list.</span></span>

12. <span data-ttu-id="6c33d-187">**Valfritt:** om du vill ha för**lägga till fler än en användare**, typ i en annan **fullständigt namn** eller **e-postadress** till hello **Sök efter namn eller e-postadress** sökrutan och klicka på hello kryssrutan tooadd den här användaren toohello **valda** lista.</span><span class="sxs-lookup"><span data-stu-id="6c33d-187">**Optional:** If you would like too**add more than one user**, type in another **full name** or **email address** into hello **Search by name or email address** search box, and click hello checkbox tooadd this user toohello **Selected** list.</span></span>

13. <span data-ttu-id="6c33d-188">När du har valt användare klickar du på hello **Välj** knappen tooadd dem toohello lista över användare och grupper toobe tilldelat toohello program.</span><span class="sxs-lookup"><span data-stu-id="6c33d-188">When you are finished selecting users, click hello **Select** button tooadd them toohello list of users and groups toobe assigned toohello application.</span></span>

14. <span data-ttu-id="6c33d-189">**Valfritt:** klickar du på hello **Välj roll** Väljaren i hello **Lägg uppdrag** bladet tooselect en roll tooassign toohello användare som du har valt.</span><span class="sxs-lookup"><span data-stu-id="6c33d-189">**Optional:** click hello **Select Role** selector in hello **Add Assignment** blade tooselect a role tooassign toohello users you have selected.</span></span>

15. <span data-ttu-id="6c33d-190">Klicka på hello **tilldela** knappen tooassign hello programmet toohello markerade användare.</span><span class="sxs-lookup"><span data-stu-id="6c33d-190">Click hello **Assign** button tooassign hello application toohello selected users.</span></span>

<span data-ttu-id="6c33d-191">Efter en kort period hello-användare som du har valt att kan toolaunch programmen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="6c33d-191">After a short period, hello users you have selected be able toolaunch these applications in hello Access Panel.</span></span>

## <a name="if-these-troubleshoot-steps-dont-resolve-hello-issue"></a><span data-ttu-id="6c33d-192">Om felsökning av dessa åtgärder inte kan lösa hello problemet</span><span class="sxs-lookup"><span data-stu-id="6c33d-192">If these troubleshoot steps don't resolve hello issue</span></span> 
<span data-ttu-id="6c33d-193">Öppna ett supportärende med hello efter information om tillgängliga:</span><span class="sxs-lookup"><span data-stu-id="6c33d-193">open a support ticket with hello following information if available:</span></span>

-   <span data-ttu-id="6c33d-194">Fel-ID för korrelation</span><span class="sxs-lookup"><span data-stu-id="6c33d-194">Correlation error ID</span></span>

-   <span data-ttu-id="6c33d-195">UPN (användarens e-postadress)</span><span class="sxs-lookup"><span data-stu-id="6c33d-195">UPN (user email address)</span></span>

-   <span data-ttu-id="6c33d-196">Klient-ID</span><span class="sxs-lookup"><span data-stu-id="6c33d-196">TenantID</span></span>

-   <span data-ttu-id="6c33d-197">Typ av webbläsare</span><span class="sxs-lookup"><span data-stu-id="6c33d-197">Browser type</span></span>

-   <span data-ttu-id="6c33d-198">Tidszon och tid/tidsperioden under fel inträffar</span><span class="sxs-lookup"><span data-stu-id="6c33d-198">Time zone and time/timeframe during error occurs</span></span>

-   <span data-ttu-id="6c33d-199">Fiddler spårningar</span><span class="sxs-lookup"><span data-stu-id="6c33d-199">Fiddler traces</span></span>

## <a name="next-steps"></a><span data-ttu-id="6c33d-200">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6c33d-200">Next steps</span></span>
[<span data-ttu-id="6c33d-201">Tillhandahålla enkel inloggning tooyour appar med Application Proxy</span><span class="sxs-lookup"><span data-stu-id="6c33d-201">Provide single sign-on tooyour apps with Application Proxy</span></span>](active-directory-application-proxy-sso-using-kcd.md)
