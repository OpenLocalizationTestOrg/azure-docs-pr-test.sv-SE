---
title: "Azure AD Connect: Sömlös Single Sign-On - Snabbstart | Microsoft Docs"
description: "Den här artikeln beskriver hur du kommer igång med Azure Active Directory sömlös enkel inloggning."
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
ms.openlocfilehash: 977108687734a5eb7f7a30419de2a6bdef184d0e
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="azure-active-directory-seamless-single-sign-on-quick-start"></a><span data-ttu-id="965fe-104">Azure Active Directory sömlös enkel inloggning: Snabbstart</span><span class="sxs-lookup"><span data-stu-id="965fe-104">Azure Active Directory Seamless Single Sign-On: Quick start</span></span>

## <a name="how-to-deploy-seamless-sso"></a><span data-ttu-id="965fe-105">Så här distribuerar du sömlös SSO</span><span class="sxs-lookup"><span data-stu-id="965fe-105">How to deploy Seamless SSO</span></span>

<span data-ttu-id="965fe-106">Azure Active Directory sömlös enkel inloggning (Azure AD sömlös SSO) automatiskt loggar användarna när de är på sina företagets datorer som är anslutna till företagsnätverket.</span><span class="sxs-lookup"><span data-stu-id="965fe-106">Azure Active Directory Seamless Single Sign-On (Azure AD Seamless SSO) automatically signs users in when they are on their corporate desktops connected to your corporate network.</span></span> <span data-ttu-id="965fe-107">Det ger dina användare enkel åtkomst till dina molnbaserade program utan att behöva ytterligare lokala komponenter.</span><span class="sxs-lookup"><span data-stu-id="965fe-107">It provides your users easy access to your cloud-based applications without needing any additional on-premises components.</span></span>

>[!IMPORTANT]
><span data-ttu-id="965fe-108">Sömlös SSO-funktionen är för närvarande under förhandsgranskning.</span><span class="sxs-lookup"><span data-stu-id="965fe-108">The Seamless SSO feature is currently in preview.</span></span>

<span data-ttu-id="965fe-109">Om du vill distribuera sömlös SSO, måste du följa dessa steg:</span><span class="sxs-lookup"><span data-stu-id="965fe-109">To deploy Seamless SSO, you need to follow these steps:</span></span>

## <a name="step-1-check-prerequisites"></a><span data-ttu-id="965fe-110">Steg 1: Kontrollera krav</span><span class="sxs-lookup"><span data-stu-id="965fe-110">Step 1: Check prerequisites</span></span>

<span data-ttu-id="965fe-111">Se till att följande krav är uppfyllda:</span><span class="sxs-lookup"><span data-stu-id="965fe-111">Ensure that the following prerequisites are in place:</span></span>

1. <span data-ttu-id="965fe-112">Konfigurera Azure AD Connect-servern: Om du använder [direkt autentisering](active-directory-aadconnect-pass-through-authentication.md) som din inloggningsmetod, ingen ytterligare åtgärd krävs.</span><span class="sxs-lookup"><span data-stu-id="965fe-112">Set up your Azure AD Connect server: If you use [Pass-through Authentication](active-directory-aadconnect-pass-through-authentication.md) as your sign-in method, no further action is required.</span></span> <span data-ttu-id="965fe-113">Om du använder [synkronisering av lösenords-Hash](active-directory-aadconnectsync-implement-password-synchronization.md) som din inloggningsmetod, och om det finns en brandvägg mellan Azure AD Connect och Azure AD, kontrollera att:</span><span class="sxs-lookup"><span data-stu-id="965fe-113">If you use [Password Hash Synchronization](active-directory-aadconnectsync-implement-password-synchronization.md) as your sign-in method, and if there is a firewall between Azure AD Connect and Azure AD, ensure that:</span></span>
- <span data-ttu-id="965fe-114">Du använder versioner 1.1.484.0 eller senare av Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="965fe-114">You are using versions 1.1.484.0 or later of Azure AD Connect.</span></span>
- <span data-ttu-id="965fe-115">Azure AD Connect kan kommunicera med `*.msappproxy.net` URL: er och över port 443.</span><span class="sxs-lookup"><span data-stu-id="965fe-115">Azure AD Connect can communicate with `*.msappproxy.net` URLs and over port 443.</span></span> <span data-ttu-id="965fe-116">Det här kravet gäller bara när du aktiverar funktionen, inte för faktiska användarinloggningar.</span><span class="sxs-lookup"><span data-stu-id="965fe-116">This prerequisite is applicable only when you enable the feature, not for actual user sign-ins.</span></span>
- <span data-ttu-id="965fe-117">Azure AD Connect kan göra direkta IP-anslutningar till den [Azure Datacenter IP-adressintervall](https://www.microsoft.com/download/details.aspx?id=41653).</span><span class="sxs-lookup"><span data-stu-id="965fe-117">Azure AD Connect can make direct IP connections to the [Azure data center IP ranges](https://www.microsoft.com/download/details.aspx?id=41653).</span></span> <span data-ttu-id="965fe-118">Det här kravet gäller igen, bara när du aktiverar funktionen.</span><span class="sxs-lookup"><span data-stu-id="965fe-118">Again, this prerequisite is applicable only when you enable the feature.</span></span>
2. <span data-ttu-id="965fe-119">Du behöver inloggningsuppgifterna för domänadministratören för varje AD-skog som du synkroniserar till Azure AD (med Azure AD Connect) och vars användare du vill aktivera sömlös SSO.</span><span class="sxs-lookup"><span data-stu-id="965fe-119">You need Domain Administrator credentials for each AD forest that you synchronize to Azure AD (using Azure AD Connect) and for whose users you want to enable Seamless SSO.</span></span>

## <a name="step-2-enable-the-feature"></a><span data-ttu-id="965fe-120">Steg 2: Aktivera funktionen</span><span class="sxs-lookup"><span data-stu-id="965fe-120">Step 2: Enable the feature</span></span>

<span data-ttu-id="965fe-121">Sömlös SSO kan aktiveras med [Azure AD Connect](active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="965fe-121">Seamless SSO can be enabled using [Azure AD Connect](active-directory-aadconnect.md).</span></span>

<span data-ttu-id="965fe-122">Om du gör en ren installation av Azure AD Connect, väljer du den [anpassade installationssökvägen](active-directory-aadconnect-get-started-custom.md).</span><span class="sxs-lookup"><span data-stu-id="965fe-122">If you are doing a fresh installation of Azure AD Connect, choose the [custom installation path](active-directory-aadconnect-get-started-custom.md).</span></span> <span data-ttu-id="965fe-123">Markera alternativet ”Aktivera enkel inloggning på” på sidan ”användarens inloggning”.</span><span class="sxs-lookup"><span data-stu-id="965fe-123">At the "User sign-in" page, check the "Enable single sign on" option.</span></span>

![Azure AD Connect - användarinloggning](./media/active-directory-aadconnect-sso/sso8.png)

<span data-ttu-id="965fe-125">Om du redan har en installation av Azure AD Connect, Välj ”Ändra användare inloggningssidan” på Azure AD Connect och klicka på ”Nästa”.</span><span class="sxs-lookup"><span data-stu-id="965fe-125">If you already have an installation of Azure AD Connect, choose "Change user sign-in page" on Azure AD Connect and click "Next".</span></span> <span data-ttu-id="965fe-126">Kontrollera sedan alternativet ”Aktivera enkel inloggning på”.</span><span class="sxs-lookup"><span data-stu-id="965fe-126">Then check the "Enable single sign on" option.</span></span>

![Azure AD Connect - ändra användarens inloggning](./media/active-directory-aadconnect-user-signin/changeusersignin.png)

<span data-ttu-id="965fe-128">Fortsätt genom guiden tills du kommer till sidan ”Aktivera enkel inloggning på”.</span><span class="sxs-lookup"><span data-stu-id="965fe-128">Continue through the wizard until you get to the "Enable single sign on" page.</span></span> <span data-ttu-id="965fe-129">Ange autentiseringsuppgifter för domänadministratör för varje AD-skog som du synkroniserar till Azure AD (med Azure AD Connect) och vars användare du vill aktivera sömlös SSO.</span><span class="sxs-lookup"><span data-stu-id="965fe-129">Provide Domain Administrator credentials for each AD forest that you synchronize to Azure AD (using Azure AD Connect) and for whose users you want to enable Seamless SSO.</span></span> 

<span data-ttu-id="965fe-130">När guiden har slutförts, är sömlös SSO aktiverat på din klient.</span><span class="sxs-lookup"><span data-stu-id="965fe-130">After completion of the wizard, Seamless SSO is enabled on your tenant.</span></span>

>[!NOTE]
> <span data-ttu-id="965fe-131">Domänadministratören autentiseringsuppgifterna lagras inte i Azure AD Connect eller i Azure AD, men endast används för att aktivera funktionen.</span><span class="sxs-lookup"><span data-stu-id="965fe-131">The Domain Administrator credentials are not stored in Azure AD Connect or in Azure AD, but are only used to enable the feature.</span></span>

<span data-ttu-id="965fe-132">Följ instruktionerna för att kontrollera att du har aktiverat sömlös SSO korrekt:</span><span class="sxs-lookup"><span data-stu-id="965fe-132">Follow these instructions to verify that you have enabled Seamless SSO correctly:</span></span>

1. <span data-ttu-id="965fe-133">Logga in på den [Azure Active Directory Administrationscenter](https://aad.portal.azure.com) med Global administratörsbehörighet för din klient.</span><span class="sxs-lookup"><span data-stu-id="965fe-133">Sign in to the [Azure Active Directory admin center](https://aad.portal.azure.com) with the Global Administrator credentials for your tenant.</span></span>
2. <span data-ttu-id="965fe-134">Välj **Azure Active Directory** i navigeringen till vänster.</span><span class="sxs-lookup"><span data-stu-id="965fe-134">Select **Azure Active Directory** on the left-hand navigation.</span></span>
3. <span data-ttu-id="965fe-135">Välj **Azure AD Connect**.</span><span class="sxs-lookup"><span data-stu-id="965fe-135">Select **Azure AD Connect**.</span></span>
4. <span data-ttu-id="965fe-136">Kontrollera att den **sömlös enkel inloggning** funktionen visar som **aktiverad**.</span><span class="sxs-lookup"><span data-stu-id="965fe-136">Verify that the **Seamless Single Sign-On** feature shows as **Enabled**.</span></span>

![Azure portal – Azure AD Connect-bladet](./media/active-directory-aadconnect-sso/sso10.png)

## <a name="step-3-roll-out-the-feature"></a><span data-ttu-id="965fe-138">Steg 3: Lansera funktionen</span><span class="sxs-lookup"><span data-stu-id="965fe-138">Step 3: Roll out the feature</span></span>

<span data-ttu-id="965fe-139">För att lansera funktionen till användarna, måste du lägga till två Azure AD-URL: er (https://autologon.microsoftazuread-sso.com och https://aadg.windows.net.nsatc.net) till användarnas intranät Zoninställningar via en Grupprincip i Active Directory.</span><span class="sxs-lookup"><span data-stu-id="965fe-139">To roll out the feature to your users, you need to add two Azure AD URLs (https://autologon.microsoftazuread-sso.com and https://aadg.windows.net.nsatc.net) to users' Intranet zone settings via Group Policy in Active Directory.</span></span>

>[!NOTE]
> <span data-ttu-id="965fe-140">Följande instruktioner fungerar endast för Internet Explorer och Google Chrome på Windows (om de har samma uppsättning URL: er för betrodda webbplatser i Internet Explorer).</span><span class="sxs-lookup"><span data-stu-id="965fe-140">The following instructions only work for Internet Explorer and Google Chrome on Windows  (if it shares set of trusted site URLs with Internet Explorer).</span></span> <span data-ttu-id="965fe-141">Läs avsnittet om hur du ställer in Mozilla Firefox och Google Chrome på Mac.</span><span class="sxs-lookup"><span data-stu-id="965fe-141">Read the next section for instructions to set up Mozilla Firefox and Google Chrome on Mac.</span></span>

### <a name="why-do-you-need-to-modify-users-intranet-zone-settings"></a><span data-ttu-id="965fe-142">Varför behöver du ändra användarnas zonen intranätsinställningar?</span><span class="sxs-lookup"><span data-stu-id="965fe-142">Why do you need to modify users' Intranet zone settings?</span></span>

<span data-ttu-id="965fe-143">Som standard beräknas webbläsaren automatiskt rätt zonen (Internet eller intranätet) från en URL.</span><span class="sxs-lookup"><span data-stu-id="965fe-143">By default, the browser automatically calculates the right zone (Internet or Intranet) from a URL.</span></span> <span data-ttu-id="965fe-144">Till exempel mappas http://contoso/ till zonen Intranät, medan http://intranet.contoso.com/ mappas till zonen Internet (eftersom den innehåller en punkt).</span><span class="sxs-lookup"><span data-stu-id="965fe-144">For example, http://contoso/ is mapped to the Intranet zone, whereas http://intranet.contoso.com/ is mapped to the Internet zone (because the URL contains a period).</span></span> <span data-ttu-id="965fe-145">Webbläsare Skicka inte Kerberos-biljetter till en molnslutpunkt, till exempel två Azure AD URL: er – om URL: en explicit har lagts till i webbläsarens intranätzonen.</span><span class="sxs-lookup"><span data-stu-id="965fe-145">Browsers don't send Kerberos tickets to a cloud endpoint - like the two Azure AD URLs - unless its URL is explicitly added to the browser's Intranet zone.</span></span>

### <a name="detailed-steps"></a><span data-ttu-id="965fe-146">Detaljerade steg</span><span class="sxs-lookup"><span data-stu-id="965fe-146">Detailed steps</span></span>

1. <span data-ttu-id="965fe-147">Öppna verktyget hantering av Grupprincip.</span><span class="sxs-lookup"><span data-stu-id="965fe-147">Open the Group Policy Management tool.</span></span>
2. <span data-ttu-id="965fe-148">Redigera en grupprincip som tillämpas på vissa eller alla användare.</span><span class="sxs-lookup"><span data-stu-id="965fe-148">Edit the Group Policy that is applied to some or all your users.</span></span> <span data-ttu-id="965fe-149">I det här exemplet använder vi den **Standarddomänprincip**.</span><span class="sxs-lookup"><span data-stu-id="965fe-149">In this example, we use the **Default Domain Policy**.</span></span>
3. <span data-ttu-id="965fe-150">Gå till **användaren Datorkonfiguration\Administrativa mallar\Windows-komponenter\Internet Explorer\Internet Panel\Security kontrollsida** och välj **plats till zon Tilldelningslista**.</span><span class="sxs-lookup"><span data-stu-id="965fe-150">Navigate to **User Configuration\Administrative Templates\Windows Components\Internet Explorer\Internet Control Panel\Security Page** and select **Site to Zone Assignment List**.</span></span>
<span data-ttu-id="965fe-151">![Enkel inloggning](./media/active-directory-aadconnect-sso/sso6.png)</span><span class="sxs-lookup"><span data-stu-id="965fe-151">![Single sign-on](./media/active-directory-aadconnect-sso/sso6.png)</span></span>  
4. <span data-ttu-id="965fe-152">Aktivera principen och ange följande värden (Azure AD-URL: er som Kerberos-biljetter vidarebefordras) och data (*1* anger zonen intranät) i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="965fe-152">Enable the policy, and enter the following values (Azure AD URLs where Kerberos tickets are forwarded) and data (*1* indicates Intranet zone) in the dialog box.</span></span>

        Value: https://autologon.microsoftazuread-sso.com
        Data: 1
        Value: https://aadg.windows.net.nsatc.net
        Data: 1
>[!NOTE]
> <span data-ttu-id="965fe-153">Om du vill hindra vissa användare från att använda sömlös SSO - exempelvis om dessa användare loggar in på delade kiosker - värdet föregående värden *4*.</span><span class="sxs-lookup"><span data-stu-id="965fe-153">If you want to disallow some users from using Seamless SSO - for instance, if these users are signing in on shared kiosks - set the preceding values to *4*.</span></span> <span data-ttu-id="965fe-154">Den här åtgärden lägger till URL: er för Azure AD för zonen Ej tillförlitliga och misslyckas sömlös SSO hela tiden.</span><span class="sxs-lookup"><span data-stu-id="965fe-154">This action adds the Azure AD URLs to the Restricted Zone, and fails Seamless SSO all the time.</span></span>

5. <span data-ttu-id="965fe-155">Klicka på **OK** och **OK** igen.</span><span class="sxs-lookup"><span data-stu-id="965fe-155">Click **OK** and **OK** again.</span></span>

![Enkel inloggning](./media/active-directory-aadconnect-sso/sso7.png)

### <a name="browser-considerations"></a><span data-ttu-id="965fe-157">Överväganden för webbläsare</span><span class="sxs-lookup"><span data-stu-id="965fe-157">Browser considerations</span></span>

#### <a name="mozilla-firefox"></a><span data-ttu-id="965fe-158">Mozilla Firefox</span><span class="sxs-lookup"><span data-stu-id="965fe-158">Mozilla Firefox</span></span>

<span data-ttu-id="965fe-159">Mozilla Firefox inte automatiskt Kerberos-autentisering.</span><span class="sxs-lookup"><span data-stu-id="965fe-159">Mozilla Firefox doesn't automatically do Kerberos Authentication.</span></span> <span data-ttu-id="965fe-160">Varje användare har manuellt lägga till URL: er för Azure AD i sina Firefox-inställningar med hjälp av följande steg:</span><span class="sxs-lookup"><span data-stu-id="965fe-160">Each user has to manually add the Azure AD URLs to their Firefox settings using the following steps:</span></span>
1. <span data-ttu-id="965fe-161">Kör Firefox och ange `about:config` i adressfältet.</span><span class="sxs-lookup"><span data-stu-id="965fe-161">Run Firefox and enter `about:config` in the address bar.</span></span> <span data-ttu-id="965fe-162">Ignorera alla meddelanden som visas.</span><span class="sxs-lookup"><span data-stu-id="965fe-162">Dismiss any notifications that you see.</span></span>
2. <span data-ttu-id="965fe-163">Sök efter den **network.negotiate-auth.trusted-URI: er** inställningar.</span><span class="sxs-lookup"><span data-stu-id="965fe-163">Search for the **network.negotiate-auth.trusted-uris** preference.</span></span> <span data-ttu-id="965fe-164">Den här inställningen visar Firefoxs betrodda platser för Kerberos-autentisering.</span><span class="sxs-lookup"><span data-stu-id="965fe-164">This preference lists Firefox's trusted sites for Kerberos authentication.</span></span>
3. <span data-ttu-id="965fe-165">Högerklicka och välj ”Ändra”.</span><span class="sxs-lookup"><span data-stu-id="965fe-165">Right-click and select "Modify".</span></span>
4. <span data-ttu-id="965fe-166">Ange ”https://autologon.microsoftazuread-sso.com, https://aadg.windows.net.nsatc.net” i fältet.</span><span class="sxs-lookup"><span data-stu-id="965fe-166">Enter "https://autologon.microsoftazuread-sso.com, https://aadg.windows.net.nsatc.net" in the field.</span></span>
5. <span data-ttu-id="965fe-167">Klicka på ”OK” och öppna webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="965fe-167">Click "OK" and reopen the browser.</span></span>

#### <a name="safari-on-mac-os"></a><span data-ttu-id="965fe-168">Safari på Mac OS</span><span class="sxs-lookup"><span data-stu-id="965fe-168">Safari on Mac OS</span></span>

<span data-ttu-id="965fe-169">Se till att datorn som kör Mac OS är ansluten till AD.</span><span class="sxs-lookup"><span data-stu-id="965fe-169">Ensure that the machine running Mac OS is joined to AD.</span></span> <span data-ttu-id="965fe-170">Instruktioner om hur du gör finns [här](http://training.apple.com/pdf/Best_Practices_for_Integrating_OS_X_with_Active_Directory.pdf).</span><span class="sxs-lookup"><span data-stu-id="965fe-170">See instructions on how to do that [here](http://training.apple.com/pdf/Best_Practices_for_Integrating_OS_X_with_Active_Directory.pdf).</span></span>

#### <a name="google-chrome-on-mac-os"></a><span data-ttu-id="965fe-171">Google Chrome på Mac OS</span><span class="sxs-lookup"><span data-stu-id="965fe-171">Google Chrome on Mac OS</span></span>

<span data-ttu-id="965fe-172">Google Chrome på Mac OS x och andra icke-Windows-plattformar finns i [i den här artikeln](https://dev.chromium.org/administrators/policy-list-3#AuthServerWhitelist) information om hur du godkända Azure AD-URL: er för integrerad autentisering.</span><span class="sxs-lookup"><span data-stu-id="965fe-172">For Google Chrome on Mac OS and other non-Windows platforms, refer to [this article](https://dev.chromium.org/administrators/policy-list-3#AuthServerWhitelist) for information on how to whitelist the Azure AD URLs for integrated authentication.</span></span>

<span data-ttu-id="965fe-173">Med hjälp av Grupprincip i Active Directory-tredjepartstillägg lansera URL: er för Azure AD Firefox och Google Chrome på Mac-användare ligger utanför omfånget för den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="965fe-173">Using third-party Active Directory Group Policy extensions to roll out the Azure AD URLs to Firefox and Google Chrome on Mac users is outside of this article's scope.</span></span>

#### <a name="known-limitations"></a><span data-ttu-id="965fe-174">Kända begränsningar</span><span class="sxs-lookup"><span data-stu-id="965fe-174">Known limitations</span></span>

<span data-ttu-id="965fe-175">Sömlös SSO fungerar inte i privat webbläsarens läge i Firefox och Edge-webbläsare.</span><span class="sxs-lookup"><span data-stu-id="965fe-175">Seamless SSO doesn't work in private browsing mode on Firefox and Edge browsers.</span></span> <span data-ttu-id="965fe-176">Den också fungerar inte på Internet Explorer om webbläsaren körs i läget för utökat skydd.</span><span class="sxs-lookup"><span data-stu-id="965fe-176">It also doesn't work on Internet Explorer if the browser is running in Enhanced Protection mode.</span></span>

>[!IMPORTANT]
><span data-ttu-id="965fe-177">Vi nyligen återställde stöd för kant för att undersöka problem som rapporteras av kunden.</span><span class="sxs-lookup"><span data-stu-id="965fe-177">We recently rolled back support for Edge to investigate customer-reported issues.</span></span>

## <a name="step-4-test-the-feature"></a><span data-ttu-id="965fe-178">Steg 4: Testa funktionen</span><span class="sxs-lookup"><span data-stu-id="965fe-178">Step 4: Test the feature</span></span>

<span data-ttu-id="965fe-179">Om du vill testa funktionen för en viss användare, se till att _alla_ följande villkor är uppfyllda:</span><span class="sxs-lookup"><span data-stu-id="965fe-179">To test the feature for a specific user, ensure that _all_ the following conditions are in place:</span></span>
  - <span data-ttu-id="965fe-180">Användaren loggar in på företagets enheter.</span><span class="sxs-lookup"><span data-stu-id="965fe-180">The user is signing in on a corporate device.</span></span>
  - <span data-ttu-id="965fe-181">Enheten har tidigare har anslutit till Active Directory (AD)-domän.</span><span class="sxs-lookup"><span data-stu-id="965fe-181">The device has been previously joined to your Active Directory (AD) domain.</span></span>
  - <span data-ttu-id="965fe-182">Enheten har en direkt anslutning till din domänkontrollanten (DC), antingen på företagets kabelanslutna eller trådlösa nätverk eller via en fjärranslutning, till exempel en VPN-anslutning.</span><span class="sxs-lookup"><span data-stu-id="965fe-182">The device has a direct connection to your Domain Controller (DC), either on the corporate wired or wireless network or via a remote access connection, such as a VPN connection.</span></span>
  - <span data-ttu-id="965fe-183">Du har [distribuerat funktionen](##step-3-roll-out-the-feature) till den här användaren med hjälp av en Grupprincip.</span><span class="sxs-lookup"><span data-stu-id="965fe-183">You have [rolled out the feature](##step-3-roll-out-the-feature) to this user using Group Policy.</span></span>

<span data-ttu-id="965fe-184">Att testa scenariot där användaren anger endast användarnamnet men inte lösenordet:</span><span class="sxs-lookup"><span data-stu-id="965fe-184">To test the scenario where the user enters only the username, but not the password:</span></span>
- <span data-ttu-id="965fe-185">Logga in på *https://myapps.microsoft.com/* i en ny privat webbläsarsession.</span><span class="sxs-lookup"><span data-stu-id="965fe-185">Sign into *https://myapps.microsoft.com/* in a new private browser session.</span></span>

<span data-ttu-id="965fe-186">Att testa scenariot där användaren inte behöver ange användarnamnet eller lösenordet:</span><span class="sxs-lookup"><span data-stu-id="965fe-186">To test the scenario where the user doesn't have to enter the username or the password:</span></span> 
- <span data-ttu-id="965fe-187">Logga in på *https://myapps.microsoft.com/contoso.onmicrosoft.com* i en ny privat webbläsarsession.</span><span class="sxs-lookup"><span data-stu-id="965fe-187">Sign into *https://myapps.microsoft.com/contoso.onmicrosoft.com* in a new private browser session.</span></span> <span data-ttu-id="965fe-188">Ersätt ”*contoso*” med namnet på din klient.</span><span class="sxs-lookup"><span data-stu-id="965fe-188">Replace "*contoso*" with your tenant's name.</span></span>
- <span data-ttu-id="965fe-189">Eller logga in på *https://myapps.microsoft.com/contoso.com* i en ny privat webbläsarsession.</span><span class="sxs-lookup"><span data-stu-id="965fe-189">Or sign into *https://myapps.microsoft.com/contoso.com* in a new private browser session.</span></span> <span data-ttu-id="965fe-190">Ersätt ”*contoso.com*” med en verifierad domän (inte en federerad domän) i din klient.</span><span class="sxs-lookup"><span data-stu-id="965fe-190">Replace "*contoso.com*" with a verified domain (not a federated domain) in your tenant.</span></span>

## <a name="step-5-roll-over-keys"></a><span data-ttu-id="965fe-191">Steg 5: Återställ över nycklar</span><span class="sxs-lookup"><span data-stu-id="965fe-191">Step 5: Roll over keys</span></span>

<span data-ttu-id="965fe-192">I steg 2 skapar Azure AD Connect datorkonton (som representerar Azure AD) i alla AD-skogar som du har aktiverat sömlös SSO.</span><span class="sxs-lookup"><span data-stu-id="965fe-192">In Step 2, Azure AD Connect creates computer accounts (representing Azure AD) in all the AD forests on which you have enabled Seamless SSO.</span></span> <span data-ttu-id="965fe-193">Läs mer i detalj [här](active-directory-aadconnect-sso-how-it-works.md).</span><span class="sxs-lookup"><span data-stu-id="965fe-193">Learn more in detail [here](active-directory-aadconnect-sso-how-it-works.md).</span></span> <span data-ttu-id="965fe-194">För förbättrad säkerhet rekommenderas det att du ofta slås via Kerberos dekrypteringsnycklar för dessa konton.</span><span class="sxs-lookup"><span data-stu-id="965fe-194">For improved security, it is recommended that  you frequently roll over the Kerberos decryption keys of these computer accounts.</span></span>

>[!IMPORTANT]
><span data-ttu-id="965fe-195">Du behöver inte göra det här steget _omedelbart_ när du har aktiverat funktionen.</span><span class="sxs-lookup"><span data-stu-id="965fe-195">You don't need to do this step _immediately_ after you have enabled the feature.</span></span> <span data-ttu-id="965fe-196">Rulla över dekrypteringsnycklar Kerberos minst var 30 dagar.</span><span class="sxs-lookup"><span data-stu-id="965fe-196">Roll over the Kerberos decryption keys at least every 30 days.</span></span>

## <a name="next-steps"></a><span data-ttu-id="965fe-197">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="965fe-197">Next steps</span></span>

- <span data-ttu-id="965fe-198">[**Tekniska ingående** ](active-directory-aadconnect-sso-how-it-works.md) -förstå hur funktionen fungerar.</span><span class="sxs-lookup"><span data-stu-id="965fe-198">[**Technical Deep Dive**](active-directory-aadconnect-sso-how-it-works.md) - Understand how this feature works.</span></span>
- <span data-ttu-id="965fe-199">[**Vanliga frågor och svar** ](active-directory-aadconnect-sso-faq.md) -svar på vanliga frågor och svar.</span><span class="sxs-lookup"><span data-stu-id="965fe-199">[**Frequently Asked Questions**](active-directory-aadconnect-sso-faq.md) - Answers to frequently asked questions.</span></span>
- <span data-ttu-id="965fe-200">[**Felsöka** ](active-directory-aadconnect-troubleshoot-sso.md) – Lär dig att lösa vanliga problem med funktionen.</span><span class="sxs-lookup"><span data-stu-id="965fe-200">[**Troubleshoot**](active-directory-aadconnect-troubleshoot-sso.md) - Learn how to resolve common issues with the feature.</span></span>
- <span data-ttu-id="965fe-201">[**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) – för arkivering av nya funktioner som efterfrågas.</span><span class="sxs-lookup"><span data-stu-id="965fe-201">[**UserVoice**](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - For filing new feature requests.</span></span>
