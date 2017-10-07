---
title: "Azure AD Connect: Sömlös Single Sign-On - Snabbstart | Microsoft Docs"
description: "Den här artikeln beskriver hur tooget igång med Azure Active Directory sömlös enkel inloggning."
services: active-directory
keywords: "Vad är Azure AD Connect, installera Active Directory, nödvändiga komponenter för Azure AD, SSO, Single Sign-on"
documentationcenter: 
author: swkrish
manager: femila
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/04/2017
ms.author: billmath
ms.openlocfilehash: 97d40ed41b3bfad9ff7e11ddbdb8de594ee85de1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-seamless-single-sign-on-quick-start"></a><span data-ttu-id="aa26f-104">Azure Active Directory sömlös enkel inloggning: Snabbstart</span><span class="sxs-lookup"><span data-stu-id="aa26f-104">Azure Active Directory Seamless Single Sign-On: Quick start</span></span>

## <a name="how-toodeploy-seamless-sso"></a><span data-ttu-id="aa26f-105">Hur toodeploy sömlös SSO</span><span class="sxs-lookup"><span data-stu-id="aa26f-105">How toodeploy Seamless SSO</span></span>

<span data-ttu-id="aa26f-106">Azure Active Directory sömlös enkel inloggning (Azure AD sömlös SSO) automatiskt loggar användarna när de är på företagets datorer anslutna tooyour företagets nätverk.</span><span class="sxs-lookup"><span data-stu-id="aa26f-106">Azure Active Directory Seamless Single Sign-On (Azure AD Seamless SSO) automatically signs users in when they are on their corporate desktops connected tooyour corporate network.</span></span> <span data-ttu-id="aa26f-107">Det ger dina användare enkel åtkomst tooyour molnbaserade program utan att behöva ytterligare lokala komponenter.</span><span class="sxs-lookup"><span data-stu-id="aa26f-107">It provides your users easy access tooyour cloud-based applications without needing any additional on-premises components.</span></span>

>[!IMPORTANT]
><span data-ttu-id="aa26f-108">hello sömlös SSO-funktionen är för närvarande under förhandsgranskning.</span><span class="sxs-lookup"><span data-stu-id="aa26f-108">hello Seamless SSO feature is currently in preview.</span></span>

<span data-ttu-id="aa26f-109">toodeploy sömlös SSO, behöver du toofollow de här stegen:</span><span class="sxs-lookup"><span data-stu-id="aa26f-109">toodeploy Seamless SSO, you need toofollow these steps:</span></span>

## <a name="step-1-check-prerequisites"></a><span data-ttu-id="aa26f-110">Steg 1: Kontrollera krav</span><span class="sxs-lookup"><span data-stu-id="aa26f-110">Step 1: Check prerequisites</span></span>

<span data-ttu-id="aa26f-111">Kontrollera att hello följande förutsättningar är uppfyllda:</span><span class="sxs-lookup"><span data-stu-id="aa26f-111">Ensure that hello following prerequisites are in place:</span></span>

1. <span data-ttu-id="aa26f-112">Konfigurera Azure AD Connect-servern: Om du använder [direkt autentisering](active-directory-aadconnect-pass-through-authentication.md) som din inloggningsmetod, ingen ytterligare åtgärd krävs.</span><span class="sxs-lookup"><span data-stu-id="aa26f-112">Set up your Azure AD Connect server: If you use [Pass-through Authentication](active-directory-aadconnect-pass-through-authentication.md) as your sign-in method, no further action is required.</span></span> <span data-ttu-id="aa26f-113">Om du använder [synkronisering av lösenords-Hash](active-directory-aadconnectsync-implement-password-synchronization.md) som din inloggningsmetod, och om det finns en brandvägg mellan Azure AD Connect och Azure AD, kontrollera att:</span><span class="sxs-lookup"><span data-stu-id="aa26f-113">If you use [Password Hash Synchronization](active-directory-aadconnectsync-implement-password-synchronization.md) as your sign-in method, and if there is a firewall between Azure AD Connect and Azure AD, ensure that:</span></span>
- <span data-ttu-id="aa26f-114">Du använder versioner 1.1.484.0 eller senare av Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="aa26f-114">You are using versions 1.1.484.0 or later of Azure AD Connect.</span></span>
- <span data-ttu-id="aa26f-115">Azure AD Connect kan kommunicera med `*.msappproxy.net` URL: er och över port 443.</span><span class="sxs-lookup"><span data-stu-id="aa26f-115">Azure AD Connect can communicate with `*.msappproxy.net` URLs and over port 443.</span></span> <span data-ttu-id="aa26f-116">Det här kravet gäller bara när du aktiverar hello-funktionen, inte för faktiska användarinloggningar.</span><span class="sxs-lookup"><span data-stu-id="aa26f-116">This prerequisite is applicable only when you enable hello feature, not for actual user sign-ins.</span></span>
- <span data-ttu-id="aa26f-117">Azure AD Connect kan göra direkt IP-anslutningar toohello [Azure Datacenter IP-adressintervall](https://www.microsoft.com/download/details.aspx?id=41653).</span><span class="sxs-lookup"><span data-stu-id="aa26f-117">Azure AD Connect can make direct IP connections toohello [Azure data center IP ranges](https://www.microsoft.com/download/details.aspx?id=41653).</span></span> <span data-ttu-id="aa26f-118">Det här kravet gäller igen, bara när du aktiverar hello-funktionen.</span><span class="sxs-lookup"><span data-stu-id="aa26f-118">Again, this prerequisite is applicable only when you enable hello feature.</span></span>
2. <span data-ttu-id="aa26f-119">Du behöver inloggningsuppgifterna för domänadministratören för varje AD-skog som du synkroniserar tooAzure AD (med Azure AD Connect) och vars användare du vill ha tooenable sömlös SSO.</span><span class="sxs-lookup"><span data-stu-id="aa26f-119">You need Domain Administrator credentials for each AD forest that you synchronize tooAzure AD (using Azure AD Connect) and for whose users you want tooenable Seamless SSO.</span></span>

## <a name="step-2-enable-hello-feature"></a><span data-ttu-id="aa26f-120">Steg 2: Aktivera hello-funktionen</span><span class="sxs-lookup"><span data-stu-id="aa26f-120">Step 2: Enable hello feature</span></span>

<span data-ttu-id="aa26f-121">Sömlös SSO kan aktiveras med [Azure AD Connect](active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="aa26f-121">Seamless SSO can be enabled using [Azure AD Connect](active-directory-aadconnect.md).</span></span>

<span data-ttu-id="aa26f-122">Om du gör en ren installation av Azure AD Connect väljer hello [anpassade installationssökvägen](active-directory-aadconnect-get-started-custom.md).</span><span class="sxs-lookup"><span data-stu-id="aa26f-122">If you are doing a fresh installation of Azure AD Connect, choose hello [custom installation path](active-directory-aadconnect-get-started-custom.md).</span></span> <span data-ttu-id="aa26f-123">Vid hello ”användare” inloggningssidan, kontrollera hello ”aktivera enkel inloggning” alternativet.</span><span class="sxs-lookup"><span data-stu-id="aa26f-123">At hello "User sign-in" page, check hello "Enable single sign on" option.</span></span>

![Azure AD Connect - användarinloggning](./media/active-directory-aadconnect-sso/sso8.png)

<span data-ttu-id="aa26f-125">Om du redan har en installation av Azure AD Connect, Välj ”Ändra användare inloggningssidan” på Azure AD Connect och klicka på ”Nästa”.</span><span class="sxs-lookup"><span data-stu-id="aa26f-125">If you already have an installation of Azure AD Connect, choose "Change user sign-in page" on Azure AD Connect and click "Next".</span></span> <span data-ttu-id="aa26f-126">Kontrollera sedan hello ”aktivera enkel inloggning” alternativet.</span><span class="sxs-lookup"><span data-stu-id="aa26f-126">Then check hello "Enable single sign on" option.</span></span>

![Azure AD Connect - ändra användarens inloggning](./media/active-directory-aadconnect-user-signin/changeusersignin.png)

<span data-ttu-id="aa26f-128">Fortsätt hello guiden tills du kommer toohello ”aktivera enkel inloggning” sidan.</span><span class="sxs-lookup"><span data-stu-id="aa26f-128">Continue through hello wizard until you get toohello "Enable single sign on" page.</span></span> <span data-ttu-id="aa26f-129">Ange autentiseringsuppgifter för domänadministratör för varje AD-skog att du synkroniserar tooAzure AD (med Azure AD Connect) och vars användare du vill ha tooenable sömlös SSO.</span><span class="sxs-lookup"><span data-stu-id="aa26f-129">Provide Domain Administrator credentials for each AD forest that you synchronize tooAzure AD (using Azure AD Connect) and for whose users you want tooenable Seamless SSO.</span></span> 

<span data-ttu-id="aa26f-130">När du har slutfört guiden hello är sömlös SSO aktiverat på din klient.</span><span class="sxs-lookup"><span data-stu-id="aa26f-130">After completion of hello wizard, Seamless SSO is enabled on your tenant.</span></span>

>[!NOTE]
> <span data-ttu-id="aa26f-131">hello domänadministratörsbehörighet lagras inte i Azure AD Connect eller i Azure AD, men det är bara använda tooenable hello-funktionen.</span><span class="sxs-lookup"><span data-stu-id="aa26f-131">hello Domain Administrator credentials are not stored in Azure AD Connect or in Azure AD, but are only used tooenable hello feature.</span></span>

<span data-ttu-id="aa26f-132">Följ dessa instruktioner tooverify att du har aktiverat sömlös SSO korrekt:</span><span class="sxs-lookup"><span data-stu-id="aa26f-132">Follow these instructions tooverify that you have enabled Seamless SSO correctly:</span></span>

1. <span data-ttu-id="aa26f-133">Logga in toohello [Azure Active Directory Administrationscenter](https://aad.portal.azure.com) med hello globala administratörsbehörigheter för din klient.</span><span class="sxs-lookup"><span data-stu-id="aa26f-133">Sign in toohello [Azure Active Directory admin center](https://aad.portal.azure.com) with hello Global Administrator credentials for your tenant.</span></span>
2. <span data-ttu-id="aa26f-134">Välj **Azure Active Directory** på hello vänstra navigeringsfönstret.</span><span class="sxs-lookup"><span data-stu-id="aa26f-134">Select **Azure Active Directory** on hello left-hand navigation.</span></span>
3. <span data-ttu-id="aa26f-135">Välj **Azure AD Connect**.</span><span class="sxs-lookup"><span data-stu-id="aa26f-135">Select **Azure AD Connect**.</span></span>
4. <span data-ttu-id="aa26f-136">Kontrollera att hello **sömlös enkel inloggning** funktionen visar som **aktiverad**.</span><span class="sxs-lookup"><span data-stu-id="aa26f-136">Verify that hello **Seamless Single Sign-On** feature shows as **Enabled**.</span></span>

![Azure portal – Azure AD Connect-bladet](./media/active-directory-aadconnect-sso/sso10.png)

## <a name="step-3-roll-out-hello-feature"></a><span data-ttu-id="aa26f-138">Steg 3: Lansera hello-funktion</span><span class="sxs-lookup"><span data-stu-id="aa26f-138">Step 3: Roll out hello feature</span></span>

<span data-ttu-id="aa26f-139">tooroll ut hello funktionen tooyour användare måste tooadd två Azure AD-URL: er (https://autologon.microsoftazuread-sso.com och https://aadg.windows.net.nsatc.net) toousers' intranät Zoninställningar via en Grupprincip i Active Directory.</span><span class="sxs-lookup"><span data-stu-id="aa26f-139">tooroll out hello feature tooyour users, you need tooadd two Azure AD URLs (https://autologon.microsoftazuread-sso.com and https://aadg.windows.net.nsatc.net) toousers' Intranet zone settings via Group Policy in Active Directory.</span></span>

>[!NOTE]
> <span data-ttu-id="aa26f-140">hello följa instruktionerna bara arbetar för Internet Explorer och Google Chrome på Windows (om de har samma uppsättning URL: er för betrodda webbplatser i Internet Explorer).</span><span class="sxs-lookup"><span data-stu-id="aa26f-140">hello following instructions only work for Internet Explorer and Google Chrome on Windows  (if it shares set of trusted site URLs with Internet Explorer).</span></span> <span data-ttu-id="aa26f-141">Läsa hello nästa avsnitt för instruktioner tooset Mozilla Firefox och Google Chrome på Mac.</span><span class="sxs-lookup"><span data-stu-id="aa26f-141">Read hello next section for instructions tooset up Mozilla Firefox and Google Chrome on Mac.</span></span>

### <a name="why-do-you-need-toomodify-users-intranet-zone-settings"></a><span data-ttu-id="aa26f-142">Varför behöver du toomodify användarnas zonen intranätsinställningar?</span><span class="sxs-lookup"><span data-stu-id="aa26f-142">Why do you need toomodify users' Intranet zone settings?</span></span>

<span data-ttu-id="aa26f-143">Som standard beräknas hello webbläsaren automatiskt hello rätt zon (Internet eller intranätet) från en URL.</span><span class="sxs-lookup"><span data-stu-id="aa26f-143">By default, hello browser automatically calculates hello right zone (Internet or Intranet) from a URL.</span></span> <span data-ttu-id="aa26f-144">Http://contoso/ är till exempel mappade toohello intranätzonen http://intranet.contoso.com/ är mappade toohello zonen för Internet (eftersom hello URL innehåller en punkt).</span><span class="sxs-lookup"><span data-stu-id="aa26f-144">For example, http://contoso/ is mapped toohello Intranet zone, whereas http://intranet.contoso.com/ is mapped toohello Internet zone (because hello URL contains a period).</span></span> <span data-ttu-id="aa26f-145">Webbläsare Skicka inte molnslutpunkt för Kerberos-biljetter tooa - som hello två Azure AD - om dess URL läggs explicit toohello webbläsarens intranätzonen.</span><span class="sxs-lookup"><span data-stu-id="aa26f-145">Browsers don't send Kerberos tickets tooa cloud endpoint - like hello two Azure AD URLs - unless its URL is explicitly added toohello browser's Intranet zone.</span></span>

### <a name="detailed-steps"></a><span data-ttu-id="aa26f-146">Detaljerade steg</span><span class="sxs-lookup"><span data-stu-id="aa26f-146">Detailed steps</span></span>

1. <span data-ttu-id="aa26f-147">Öppna hello verktyg för hantering av Grupprincip.</span><span class="sxs-lookup"><span data-stu-id="aa26f-147">Open hello Group Policy Management tool.</span></span>
2. <span data-ttu-id="aa26f-148">Redigera hello Grupprincip som är kopplade toosome eller alla användare.</span><span class="sxs-lookup"><span data-stu-id="aa26f-148">Edit hello Group Policy that is applied toosome or all your users.</span></span> <span data-ttu-id="aa26f-149">I det här exemplet använder vi hello **Standarddomänprincip**.</span><span class="sxs-lookup"><span data-stu-id="aa26f-149">In this example, we use hello **Default Domain Policy**.</span></span>
3. <span data-ttu-id="aa26f-150">Navigera för**användaren Datorkonfiguration\Administrativa mallar\Windows-komponenter\Internet Explorer\Internet Panel\Security kontrollsida** och välj **plats tooZone Tilldelningslista**.</span><span class="sxs-lookup"><span data-stu-id="aa26f-150">Navigate too**User Configuration\Administrative Templates\Windows Components\Internet Explorer\Internet Control Panel\Security Page** and select **Site tooZone Assignment List**.</span></span>
<span data-ttu-id="aa26f-151">![Enkel inloggning](./media/active-directory-aadconnect-sso/sso6.png)</span><span class="sxs-lookup"><span data-stu-id="aa26f-151">![Single sign-on](./media/active-directory-aadconnect-sso/sso6.png)</span></span>  
4. <span data-ttu-id="aa26f-152">Aktivera princip för hello och ange hello följande värden (Azure AD-URL: er som Kerberos-biljetter vidarebefordras) och data (*1* anger zonen intranät) hello i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="aa26f-152">Enable hello policy, and enter hello following values (Azure AD URLs where Kerberos tickets are forwarded) and data (*1* indicates Intranet zone) in hello dialog box.</span></span>

        Value: https://autologon.microsoftazuread-sso.com
        Data: 1
        Value: https://aadg.windows.net.nsatc.net
        Data: 1
>[!NOTE]
> <span data-ttu-id="aa26f-153">Om du vill toodisallow vissa användare från att använda sömlös SSO - exempelvis om dessa användare loggar in på delade kiosker - ange hello föregående värden för*4*.</span><span class="sxs-lookup"><span data-stu-id="aa26f-153">If you want toodisallow some users from using Seamless SSO - for instance, if these users are signing in on shared kiosks - set hello preceding values too*4*.</span></span> <span data-ttu-id="aa26f-154">Den här åtgärden lägger till hello Azure AD-URL: er toohello begränsad zon och misslyckas sömlös SSO alla hello tid.</span><span class="sxs-lookup"><span data-stu-id="aa26f-154">This action adds hello Azure AD URLs toohello Restricted Zone, and fails Seamless SSO all hello time.</span></span>

5. <span data-ttu-id="aa26f-155">Klicka på **OK** och **OK** igen.</span><span class="sxs-lookup"><span data-stu-id="aa26f-155">Click **OK** and **OK** again.</span></span>

![Enkel inloggning](./media/active-directory-aadconnect-sso/sso7.png)

### <a name="browser-considerations"></a><span data-ttu-id="aa26f-157">Överväganden för webbläsare</span><span class="sxs-lookup"><span data-stu-id="aa26f-157">Browser considerations</span></span>

#### <a name="mozilla-firefox"></a><span data-ttu-id="aa26f-158">Mozilla Firefox</span><span class="sxs-lookup"><span data-stu-id="aa26f-158">Mozilla Firefox</span></span>

<span data-ttu-id="aa26f-159">Mozilla Firefox inte automatiskt Kerberos-autentisering.</span><span class="sxs-lookup"><span data-stu-id="aa26f-159">Mozilla Firefox doesn't automatically do Kerberos Authentication.</span></span> <span data-ttu-id="aa26f-160">Varje användare har toomanually lägga till hello Azure AD-URL: er tootheir Firefox inställningar med hjälp av hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="aa26f-160">Each user has toomanually add hello Azure AD URLs tootheir Firefox settings using hello following steps:</span></span>
1. <span data-ttu-id="aa26f-161">Kör Firefox och ange `about:config` i hello adressfältet.</span><span class="sxs-lookup"><span data-stu-id="aa26f-161">Run Firefox and enter `about:config` in hello address bar.</span></span> <span data-ttu-id="aa26f-162">Ignorera alla meddelanden som visas.</span><span class="sxs-lookup"><span data-stu-id="aa26f-162">Dismiss any notifications that you see.</span></span>
2. <span data-ttu-id="aa26f-163">Sök efter hello **network.negotiate-auth.trusted-URI: er** inställningar.</span><span class="sxs-lookup"><span data-stu-id="aa26f-163">Search for hello **network.negotiate-auth.trusted-uris** preference.</span></span> <span data-ttu-id="aa26f-164">Den här inställningen visar Firefoxs betrodda platser för Kerberos-autentisering.</span><span class="sxs-lookup"><span data-stu-id="aa26f-164">This preference lists Firefox's trusted sites for Kerberos authentication.</span></span>
3. <span data-ttu-id="aa26f-165">Högerklicka och välj ”Ändra”.</span><span class="sxs-lookup"><span data-stu-id="aa26f-165">Right-click and select "Modify".</span></span>
4. <span data-ttu-id="aa26f-166">Ange ”https://autologon.microsoftazuread-sso.com, https://aadg.windows.net.nsatc.net” hello-fältet.</span><span class="sxs-lookup"><span data-stu-id="aa26f-166">Enter "https://autologon.microsoftazuread-sso.com, https://aadg.windows.net.nsatc.net" in hello field.</span></span>
5. <span data-ttu-id="aa26f-167">Klicka på ”OK” och öppna hello webbläsare.</span><span class="sxs-lookup"><span data-stu-id="aa26f-167">Click "OK" and reopen hello browser.</span></span>

#### <a name="safari-on-mac-os"></a><span data-ttu-id="aa26f-168">Safari på Mac OS</span><span class="sxs-lookup"><span data-stu-id="aa26f-168">Safari on Mac OS</span></span>

<span data-ttu-id="aa26f-169">Se till att hello-dator som kör Mac OS är domänansluten tooAD.</span><span class="sxs-lookup"><span data-stu-id="aa26f-169">Ensure that hello machine running Mac OS is joined tooAD.</span></span> <span data-ttu-id="aa26f-170">Se anvisningar för hur toodo som [här](http://training.apple.com/pdf/Best_Practices_for_Integrating_OS_X_with_Active_Directory.pdf).</span><span class="sxs-lookup"><span data-stu-id="aa26f-170">See instructions on how toodo that [here](http://training.apple.com/pdf/Best_Practices_for_Integrating_OS_X_with_Active_Directory.pdf).</span></span>

#### <a name="google-chrome-on-mac-os"></a><span data-ttu-id="aa26f-171">Google Chrome på Mac OS</span><span class="sxs-lookup"><span data-stu-id="aa26f-171">Google Chrome on Mac OS</span></span>

<span data-ttu-id="aa26f-172">Google Chrome på Mac OS x och andra icke-Windows-plattformar finns för[i den här artikeln](https://dev.chromium.org/administrators/policy-list-3#AuthServerWhitelist) information om hur toowhitelist hello Azure AD-URL: er för integrerad autentisering.</span><span class="sxs-lookup"><span data-stu-id="aa26f-172">For Google Chrome on Mac OS and other non-Windows platforms, refer too[this article](https://dev.chromium.org/administrators/policy-list-3#AuthServerWhitelist) for information on how toowhitelist hello Azure AD URLs for integrated authentication.</span></span>

<span data-ttu-id="aa26f-173">Med hjälp av tredjeparts-Grupprincip i Active Directory tillägg tooroll hello Azure AD-URL: er tooFirefox och Google Chrome på Mac-användare som ligger utanför omfånget för den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="aa26f-173">Using third-party Active Directory Group Policy extensions tooroll out hello Azure AD URLs tooFirefox and Google Chrome on Mac users is outside of this article's scope.</span></span>

#### <a name="known-limitations"></a><span data-ttu-id="aa26f-174">Kända begränsningar</span><span class="sxs-lookup"><span data-stu-id="aa26f-174">Known limitations</span></span>

<span data-ttu-id="aa26f-175">Sömlös SSO fungerar inte i privat webbläsarens läge i Firefox och Edge-webbläsare.</span><span class="sxs-lookup"><span data-stu-id="aa26f-175">Seamless SSO doesn't work in private browsing mode on Firefox and Edge browsers.</span></span> <span data-ttu-id="aa26f-176">Den också fungerar inte på Internet Explorer om hello webbläsaren körs i läget för utökat skydd.</span><span class="sxs-lookup"><span data-stu-id="aa26f-176">It also doesn't work on Internet Explorer if hello browser is running in Enhanced Protection mode.</span></span>

>[!IMPORTANT]
><span data-ttu-id="aa26f-177">Vi nyligen återställde stöd för Edge tooinvestigate kunden rapporterade problem.</span><span class="sxs-lookup"><span data-stu-id="aa26f-177">We recently rolled back support for Edge tooinvestigate customer-reported issues.</span></span>

## <a name="step-4-test-hello-feature"></a><span data-ttu-id="aa26f-178">Steg 4: Testa hello-funktion</span><span class="sxs-lookup"><span data-stu-id="aa26f-178">Step 4: Test hello feature</span></span>

<span data-ttu-id="aa26f-179">tootest hello funktionen för en viss användare, se till att _alla_ hello följande villkor är uppfyllda:</span><span class="sxs-lookup"><span data-stu-id="aa26f-179">tootest hello feature for a specific user, ensure that _all_ hello following conditions are in place:</span></span>
  - <span data-ttu-id="aa26f-180">hello användaren loggar in på företagets enheter.</span><span class="sxs-lookup"><span data-stu-id="aa26f-180">hello user is signing in on a corporate device.</span></span>
  - <span data-ttu-id="aa26f-181">hello enhet har varit tidigare kopplade tooyour Active Directory (AD)-domän.</span><span class="sxs-lookup"><span data-stu-id="aa26f-181">hello device has been previously joined tooyour Active Directory (AD) domain.</span></span>
  - <span data-ttu-id="aa26f-182">hello-enhet har en direktanslutning tooyour domänkontrollanten (DC), antingen på hello företagets kabelanslutna eller trådlösa nätverk eller via en fjärranslutning, till exempel en VPN-anslutning.</span><span class="sxs-lookup"><span data-stu-id="aa26f-182">hello device has a direct connection tooyour Domain Controller (DC), either on hello corporate wired or wireless network or via a remote access connection, such as a VPN connection.</span></span>
  - <span data-ttu-id="aa26f-183">Du har [distribuerat hello funktionen](##step-3-roll-out-the-feature) toothis användare med hjälp av en Grupprincip.</span><span class="sxs-lookup"><span data-stu-id="aa26f-183">You have [rolled out hello feature](##step-3-roll-out-the-feature) toothis user using Group Policy.</span></span>

<span data-ttu-id="aa26f-184">tootest hello scenario där hello användaren anger endast hello användarnamn, men inte hello lösenord:</span><span class="sxs-lookup"><span data-stu-id="aa26f-184">tootest hello scenario where hello user enters only hello username, but not hello password:</span></span>
- <span data-ttu-id="aa26f-185">Logga in på *https://myapps.microsoft.com/* i en ny privat webbläsarsession.</span><span class="sxs-lookup"><span data-stu-id="aa26f-185">Sign into *https://myapps.microsoft.com/* in a new private browser session.</span></span>

<span data-ttu-id="aa26f-186">tootest hello scenario där hello användaren inte har tooenter hello användarnamn eller hello lösenord:</span><span class="sxs-lookup"><span data-stu-id="aa26f-186">tootest hello scenario where hello user doesn't have tooenter hello username or hello password:</span></span> 
- <span data-ttu-id="aa26f-187">Logga in på *https://myapps.microsoft.com/contoso.onmicrosoft.com* i en ny privat webbläsarsession.</span><span class="sxs-lookup"><span data-stu-id="aa26f-187">Sign into *https://myapps.microsoft.com/contoso.onmicrosoft.com* in a new private browser session.</span></span> <span data-ttu-id="aa26f-188">Ersätt ”*contoso*” med namnet på din klient.</span><span class="sxs-lookup"><span data-stu-id="aa26f-188">Replace "*contoso*" with your tenant's name.</span></span>
- <span data-ttu-id="aa26f-189">Eller logga in på *https://myapps.microsoft.com/contoso.com* i en ny privat webbläsarsession.</span><span class="sxs-lookup"><span data-stu-id="aa26f-189">Or sign into *https://myapps.microsoft.com/contoso.com* in a new private browser session.</span></span> <span data-ttu-id="aa26f-190">Ersätt ”*contoso.com*” med en verifierad domän (inte en federerad domän) i din klient.</span><span class="sxs-lookup"><span data-stu-id="aa26f-190">Replace "*contoso.com*" with a verified domain (not a federated domain) in your tenant.</span></span>

## <a name="step-5-roll-over-keys"></a><span data-ttu-id="aa26f-191">Steg 5: Återställ över nycklar</span><span class="sxs-lookup"><span data-stu-id="aa26f-191">Step 5: Roll over keys</span></span>

<span data-ttu-id="aa26f-192">I steg 2 skapar Azure AD Connect datorkonton (som representerar Azure AD) i alla hello AD-skogar som du har aktiverat sömlös SSO.</span><span class="sxs-lookup"><span data-stu-id="aa26f-192">In Step 2, Azure AD Connect creates computer accounts (representing Azure AD) in all hello AD forests on which you have enabled Seamless SSO.</span></span> <span data-ttu-id="aa26f-193">Läs mer i detalj [här](active-directory-aadconnect-sso-how-it-works.md).</span><span class="sxs-lookup"><span data-stu-id="aa26f-193">Learn more in detail [here](active-directory-aadconnect-sso-how-it-works.md).</span></span> <span data-ttu-id="aa26f-194">För förbättrad säkerhet bör du lanserar ofta över hello Kerberos dekrypteringsnycklar för dessa konton.</span><span class="sxs-lookup"><span data-stu-id="aa26f-194">For improved security, it is recommended that  you frequently roll over hello Kerberos decryption keys of these computer accounts.</span></span>

>[!IMPORTANT]
><span data-ttu-id="aa26f-195">Du behöver inte toodo det här steget _omedelbart_ när du har aktiverat hello-funktionen.</span><span class="sxs-lookup"><span data-stu-id="aa26f-195">You don't need toodo this step _immediately_ after you have enabled hello feature.</span></span> <span data-ttu-id="aa26f-196">Rulla över hello Kerberos dekrypteringsnycklar minst var 30 dagar.</span><span class="sxs-lookup"><span data-stu-id="aa26f-196">Roll over hello Kerberos decryption keys at least every 30 days.</span></span>

## <a name="next-steps"></a><span data-ttu-id="aa26f-197">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="aa26f-197">Next steps</span></span>

- <span data-ttu-id="aa26f-198">[**Tekniska ingående** ](active-directory-aadconnect-sso-how-it-works.md) -förstå hur funktionen fungerar.</span><span class="sxs-lookup"><span data-stu-id="aa26f-198">[**Technical Deep Dive**](active-directory-aadconnect-sso-how-it-works.md) - Understand how this feature works.</span></span>
- <span data-ttu-id="aa26f-199">[**Vanliga frågor och svar** ](active-directory-aadconnect-sso-faq.md) -toofrequently frågor och svar.</span><span class="sxs-lookup"><span data-stu-id="aa26f-199">[**Frequently Asked Questions**](active-directory-aadconnect-sso-faq.md) - Answers toofrequently asked questions.</span></span>
- <span data-ttu-id="aa26f-200">[**Felsöka** ](active-directory-aadconnect-troubleshoot-sso.md) – Lär dig hur tooresolve vanliga problem med hello-funktionen.</span><span class="sxs-lookup"><span data-stu-id="aa26f-200">[**Troubleshoot**](active-directory-aadconnect-troubleshoot-sso.md) - Learn how tooresolve common issues with hello feature.</span></span>
- <span data-ttu-id="aa26f-201">[**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) – för arkivering av nya funktioner som efterfrågas.</span><span class="sxs-lookup"><span data-stu-id="aa26f-201">[**UserVoice**](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - For filing new feature requests.</span></span>
