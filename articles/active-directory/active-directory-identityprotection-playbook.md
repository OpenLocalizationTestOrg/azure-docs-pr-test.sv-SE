---
title: aaaAzure Active Directory-identitetsskydd playbook | Microsoft Docs
description: "Lär dig hur Azure AD Identity Protection kan du toolimit hello möjligheten för en angripare tooexploit en komprometterad identitet eller enheten och toosecure en identitet eller en enhet som tidigare var misstänkt eller kända toobe komprometteras."
services: active-directory
keywords: "Azure active directory identitetsskydd, cloud app discovery, hantera program, säkerhet, risk, risknivå, säkerhetsproblem och säkerhetsprincip"
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 60836abf-f0e9-459d-b344-8e06b8341d25
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 6252bc133fc0c0f84800ee245a04bbf62d4cd25b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-identity-protection-playbook"></a><span data-ttu-id="b274f-104">Azure Active Directory-identitetsskydd playbook</span><span class="sxs-lookup"><span data-stu-id="b274f-104">Azure Active Directory Identity Protection playbook</span></span>
<span data-ttu-id="b274f-105">Den här playbook hjälper dig att:</span><span class="sxs-lookup"><span data-stu-id="b274f-105">This playbook helps you to:</span></span>

* <span data-ttu-id="b274f-106">Fyll i data i hello identitetsskydd miljö genom simulering riskhändelser och säkerhetsproblem</span><span class="sxs-lookup"><span data-stu-id="b274f-106">Populate data in hello Identity Protection environment by simulating risk events and vulnerabilities</span></span>
* <span data-ttu-id="b274f-107">Konfigurera principer för risk-baserad villkorlig åtkomst och testa hello effekten av dessa principer</span><span class="sxs-lookup"><span data-stu-id="b274f-107">Set up risk-based conditional access policies and test hello impact of these policies</span></span>

## <a name="simulating-risk-events"></a><span data-ttu-id="b274f-108">Simulera riskhändelser</span><span class="sxs-lookup"><span data-stu-id="b274f-108">Simulating Risk Events</span></span>
<span data-ttu-id="b274f-109">Det här avsnittet innehåller steg för att simulera hello följande risker händelsetyper:</span><span class="sxs-lookup"><span data-stu-id="b274f-109">This section provides you with steps for simulating hello following risk event types:</span></span>

* <span data-ttu-id="b274f-110">Inloggningar från anonyma IP-adresser (enkel)</span><span class="sxs-lookup"><span data-stu-id="b274f-110">Sign-ins from anonymous IP addresses (easy)</span></span>
* <span data-ttu-id="b274f-111">Inloggningar från okända platser (måttlig)</span><span class="sxs-lookup"><span data-stu-id="b274f-111">Sign-ins from unfamiliar locations (moderate)</span></span>
* <span data-ttu-id="b274f-112">Omöjlig resa tooatypical platser (svårt)</span><span class="sxs-lookup"><span data-stu-id="b274f-112">Impossible travel tooatypical locations (difficult)</span></span>

<span data-ttu-id="b274f-113">Andra riskhändelser kan simuleras på ett säkert sätt.</span><span class="sxs-lookup"><span data-stu-id="b274f-113">Other risk events cannot be simulated in a secure manner.</span></span>

### <a name="sign-ins-from-anonymous-ip-addresses"></a><span data-ttu-id="b274f-114">Inloggningar från anonyma IP-adresser</span><span class="sxs-lookup"><span data-stu-id="b274f-114">Sign-ins from anonymous IP addresses</span></span>
<span data-ttu-id="b274f-115">Den här typen av risk händelse identifierar användare som har loggat in från en IP-adress har identifierats som en anonym proxyserver IP-adress.</span><span class="sxs-lookup"><span data-stu-id="b274f-115">This risk event type identifies users who have successfully signed in from an IP address that has been identified as an anonymous proxy IP address.</span></span> <span data-ttu-id="b274f-116">Dessa proxyservrar är som används av användare som vill toohide sina enhetens IP-adress och kan användas för skadliga åtgärder.</span><span class="sxs-lookup"><span data-stu-id="b274f-116">These proxies are used by people who want toohide their device’s IP address and may be used for malicious intent.</span></span>

<span data-ttu-id="b274f-117">**toosimulate loggar in från en anonym IP utföra hello följande**:</span><span class="sxs-lookup"><span data-stu-id="b274f-117">**toosimulate a sign-in from an anonymous IP, perform hello following steps**:</span></span>

1. <span data-ttu-id="b274f-118">Hämta hello [Tor Browser](https://www.torproject.org/projects/torbrowser.html.en).</span><span class="sxs-lookup"><span data-stu-id="b274f-118">Download hello [Tor Browser](https://www.torproject.org/projects/torbrowser.html.en).</span></span>
2. <span data-ttu-id="b274f-119">Använd hello Tor Browser, navigera för[https://myapps.microsoft.com](https://myapps.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="b274f-119">Using hello Tor Browser, navigate too[https://myapps.microsoft.com](https://myapps.microsoft.com).</span></span>   
3. <span data-ttu-id="b274f-120">Ange hello autentiseringsuppgifter på hello-konto som du vill tooappear i hello **inloggningar från anonyma IP-adresser** rapporten.</span><span class="sxs-lookup"><span data-stu-id="b274f-120">Enter hello credentials of hello account you want tooappear in hello **Sign-ins from anonymous IP addresses** report.</span></span>

<span data-ttu-id="b274f-121">hello-inloggning kommer att visas på instrumentpanelen för hello identitetsskydd inom 5 minuter.</span><span class="sxs-lookup"><span data-stu-id="b274f-121">hello sign-in will show up on hello Identity Protection dashboard within 5 minutes.</span></span> 

### <a name="sign-ins-from-unfamiliar-locations"></a><span data-ttu-id="b274f-122">Inloggningar från okända platser</span><span class="sxs-lookup"><span data-stu-id="b274f-122">Sign-ins from unfamiliar locations</span></span>
<span data-ttu-id="b274f-123">hello okända platser risken är en mekanism för realtid inloggning utvärdering som överväger tidigare inloggning platser (IP, latitud / longitud och ASN) toodetermine nya / okända platser.</span><span class="sxs-lookup"><span data-stu-id="b274f-123">hello unfamiliar locations risk is a real-time sign-in evaluation mechanism that considers past sign-in locations (IP, Latitude / Longitude and ASN) toodetermine new / unfamiliar locations.</span></span> <span data-ttu-id="b274f-124">hello lagras tidigare IP-adresser, latitud / longitud och ASN: er för en användare så att dessa toobe bekant platser.</span><span class="sxs-lookup"><span data-stu-id="b274f-124">hello system stores previous IPs, Latitude / Longitude, and ASNs of a user and considers these toobe familiar locations.</span></span> <span data-ttu-id="b274f-125">En inloggning plats anses vet om hello inloggning plats inte matchar någon av hello befintliga bekant platser.</span><span class="sxs-lookup"><span data-stu-id="b274f-125">A sign-in location is considered unfamiliar if hello sign-in location does not match any of hello existing familiar locations.</span></span>

<span data-ttu-id="b274f-126">Azure Active Directory identitetsskydd:</span><span class="sxs-lookup"><span data-stu-id="b274f-126">Azure Active Directory Identity Protection:</span></span>  

* <span data-ttu-id="b274f-127">har en inledande learning-period på 14 dagar då inte flaggas några nya platser som okända platser.</span><span class="sxs-lookup"><span data-stu-id="b274f-127">has an initial learning period of 14 days during which it does not flag any new locations as unfamiliar locations.</span></span>
* <span data-ttu-id="b274f-128">ignorerar inloggningar från bekant enheter och platser som är geografiskt nära tooan befintlig bekant plats.</span><span class="sxs-lookup"><span data-stu-id="b274f-128">ignores sign-ins from familiar devices and locations that are geographically close tooan existing familiar location.</span></span>

<span data-ttu-id="b274f-129">toosimulate okända platser, har du toosign i från en plats och en enhet som hello-kontot inte har loggat in från innan.</span><span class="sxs-lookup"><span data-stu-id="b274f-129">toosimulate unfamiliar locations, you have toosign in from a location and device that hello account has not signed in from before.</span></span> 

<span data-ttu-id="b274f-130">**toosimulate loggar in från en okänd plats, utföra hello följande**:</span><span class="sxs-lookup"><span data-stu-id="b274f-130">**toosimulate a sign-in from an unfamiliar location, perform hello following steps**:</span></span>

1. <span data-ttu-id="b274f-131">Välj ett konto som har minst en 14 dagar inloggning historik.</span><span class="sxs-lookup"><span data-stu-id="b274f-131">Choose an account that has at least a 14-day sign-in history.</span></span> 
2. <span data-ttu-id="b274f-132">Gör något:</span><span class="sxs-lookup"><span data-stu-id="b274f-132">Do either:</span></span>
   
   <span data-ttu-id="b274f-133">a.</span><span class="sxs-lookup"><span data-stu-id="b274f-133">a.</span></span> <span data-ttu-id="b274f-134">När du använder en VPN-anslutning kan navigera för[https://myapps.microsoft.com](https://myapps.microsoft.com) och ange hello autentiseringsuppgifter på hello-konto som du vill toosimulate hello risk händelse för.</span><span class="sxs-lookup"><span data-stu-id="b274f-134">While using a VPN, navigate too[https://myapps.microsoft.com](https://myapps.microsoft.com) and enter hello credentials of hello account you want toosimulate hello risk event for.</span></span>
   
   <span data-ttu-id="b274f-135">b.</span><span class="sxs-lookup"><span data-stu-id="b274f-135">b.</span></span> <span data-ttu-id="b274f-136">Be en kollega i en annan plats toosign med hello kontots autentiseringsuppgifter (rekommenderas inte).</span><span class="sxs-lookup"><span data-stu-id="b274f-136">Ask an associate in a different location toosign in using hello account’s credentials (not recommended).</span></span>

<span data-ttu-id="b274f-137">hello-inloggning kommer att visas på instrumentpanelen för hello identitetsskydd inom 5 minuter.</span><span class="sxs-lookup"><span data-stu-id="b274f-137">hello sign-in will show up on hello Identity Protection dashboard within 5 minutes.</span></span>

### <a name="impossible-travel-tooatypical-location"></a><span data-ttu-id="b274f-138">Omöjlig resa tooatypical plats</span><span class="sxs-lookup"><span data-stu-id="b274f-138">Impossible travel tooatypical location</span></span>
<span data-ttu-id="b274f-139">Det är svårt att simulera hello omöjligt att resa villkoret eftersom hello algoritmen använder machine learning tooweed ut FALSKT positiva identifieringar som omöjligt att resa från bekant enheter eller inloggningar från VPN-anslutningar som används av andra användare i hello directory.</span><span class="sxs-lookup"><span data-stu-id="b274f-139">Simulating hello impossible travel condition is difficult because hello algorithm uses machine learning tooweed out false-positives such as impossible travel from familiar devices, or sign-ins from VPNs that are used by other users in hello directory.</span></span> <span data-ttu-id="b274f-140">Dessutom kräver hello algoritmen en historik 3 too14 dagar för hello användaren innan den börjar skapa riskhändelser.</span><span class="sxs-lookup"><span data-stu-id="b274f-140">Additionally, hello algorithm requires a sign-in history of 3 too14 days for hello user before it begins generating risk events.</span></span>

<span data-ttu-id="b274f-141">**toosimulate en omöjlig resa tooatypical plats, utföra hello följande**:</span><span class="sxs-lookup"><span data-stu-id="b274f-141">**toosimulate an impossible travel tooatypical location, perform hello following steps**:</span></span>

1. <span data-ttu-id="b274f-142">Använd din standard-webbläsare, navigera för[https://myapps.microsoft.com](https://myapps.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="b274f-142">Using your standard browser, navigate too[https://myapps.microsoft.com](https://myapps.microsoft.com).</span></span>  
2. <span data-ttu-id="b274f-143">Ange hello autentiseringsuppgifterna för hello-konto som du vill toogenerate händelsen omöjligt att resa risken för.</span><span class="sxs-lookup"><span data-stu-id="b274f-143">Enter hello credentials of hello account you want toogenerate an impossible travel risk event for.</span></span>
3. <span data-ttu-id="b274f-144">Ändra din användaragent.</span><span class="sxs-lookup"><span data-stu-id="b274f-144">Change your user agent.</span></span> <span data-ttu-id="b274f-145">Du kan ändra användaragent i Internet Explorer från utvecklingsverktyg eller ändra din användaragent i Firefox eller Chrome med en användaragent switcher tillägg.</span><span class="sxs-lookup"><span data-stu-id="b274f-145">You can change user agent in Internet Explorer from Developer Tools, or change your user agent in Firefox or Chrome using a user-agent switcher add-on.</span></span>
4. <span data-ttu-id="b274f-146">Ändra IP-adress.</span><span class="sxs-lookup"><span data-stu-id="b274f-146">Change your IP address.</span></span> <span data-ttu-id="b274f-147">Du kan ändra din IP-adress med hjälp av en VPN-anslutning, Tor-tillägg eller startas en ny dator i Azure i olika datacenter.</span><span class="sxs-lookup"><span data-stu-id="b274f-147">You can change your IP address by using a VPN, a Tor add-on, or spinning up a new machine in Azure in a different data center.</span></span>
5. <span data-ttu-id="b274f-148">Logga in för[https://myapps.microsoft.com](https://myapps.microsoft.com) med hello samma autentiseringsuppgifter som tidigare och inom några minuter efter hello tidigare inloggning.</span><span class="sxs-lookup"><span data-stu-id="b274f-148">Sign-in too[https://myapps.microsoft.com](https://myapps.microsoft.com) using hello same credentials as before and within a few minutes after hello previous sign-in.</span></span>

<span data-ttu-id="b274f-149">hello-inloggning kommer att visas i instrumentpanelen för hello identitetsskydd inom 2-4 timmar.</span><span class="sxs-lookup"><span data-stu-id="b274f-149">hello sign-in will show up in hello Identity Protection dashboard within 2-4 hours.</span></span><br>
<span data-ttu-id="b274f-150">På grund av hello komplexa maskininlärning modeller som ingår, ökar risken för den inte får tas upp.</span><span class="sxs-lookup"><span data-stu-id="b274f-150">Because of hello complex machine learning models involved, there is a chance it will not get picked up.</span></span><br> <span data-ttu-id="b274f-151">Du kanske vill tooreplicate stegen för flera Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="b274f-151">You might want tooreplicate these steps for multiple Azure AD accounts.</span></span>

## <a name="simulating-vulnerabilities"></a><span data-ttu-id="b274f-152">Simulera säkerhetsrisker</span><span class="sxs-lookup"><span data-stu-id="b274f-152">Simulating vulnerabilities</span></span>
<span data-ttu-id="b274f-153">Säkerhetsrisker är svagheter i en Azure AD-miljö som kan utnyttjas av en felaktig aktören.</span><span class="sxs-lookup"><span data-stu-id="b274f-153">Vulnerabilities are weaknesses in an Azure AD environment that can be exploited by a bad actor.</span></span> <span data-ttu-id="b274f-154">För närvarande exponeras 3 typer av säkerhetsproblem i Azure AD Identity Protection som utnyttjar andra funktioner i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b274f-154">Currently 3 types of vulnerabilities are surfaced in Azure AD Identity Protection that leverage other features of Azure AD.</span></span> <span data-ttu-id="b274f-155">Dessa problem visas på hello identitetsskydd instrumentpanel automatiskt när dessa funktioner har ställts in.</span><span class="sxs-lookup"><span data-stu-id="b274f-155">These Vulnerabilities will be displayed on hello Identity Protection dashboard automatically once these features are set up.</span></span>

* <span data-ttu-id="b274f-156">Azure AD [Multi-Factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md)</span><span class="sxs-lookup"><span data-stu-id="b274f-156">Azure AD [Multi-Factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md)</span></span>
* <span data-ttu-id="b274f-157">Azure AD [Cloud App Discovery](active-directory-cloudappdiscovery-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="b274f-157">Azure AD [Cloud App Discovery](active-directory-cloudappdiscovery-whatis.md).</span></span>
* <span data-ttu-id="b274f-158">Azure AD [Privileged Identity Management](active-directory-privileged-identity-management-configure.md).</span><span class="sxs-lookup"><span data-stu-id="b274f-158">Azure AD [Privileged Identity Management](active-directory-privileged-identity-management-configure.md).</span></span> 

## <a name="user-compromise-risk"></a><span data-ttu-id="b274f-159">Användaren har komprometterats risk</span><span class="sxs-lookup"><span data-stu-id="b274f-159">User compromise risk</span></span>
<span data-ttu-id="b274f-160">**tootest användaren risken för angrepp, utföra hello följande**:</span><span class="sxs-lookup"><span data-stu-id="b274f-160">**tootest User compromise risk, perform hello following steps**:</span></span>

1. <span data-ttu-id="b274f-161">Logga in för[https://portal.azure.com](https://portal.azure.com) med globala administratörsbehörigheter för din klient.</span><span class="sxs-lookup"><span data-stu-id="b274f-161">Sign-in too[https://portal.azure.com](https://portal.azure.com) with global administrator credentials for your tenant.</span></span>
2. <span data-ttu-id="b274f-162">Navigera för**identitetsskydd**.</span><span class="sxs-lookup"><span data-stu-id="b274f-162">Navigate too**Identity Protection**.</span></span> 
3. <span data-ttu-id="b274f-163">På hello huvudsakliga **Azure AD Identity Protection** bladet, klickar du på **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="b274f-163">On hello main **Azure AD Identity Protection** blade, click **Settings**.</span></span> 
4. <span data-ttu-id="b274f-164">På hello **portalinställningar** bladet under **säkerhetsregler**, klickar du på **användaren risken för angrepp**.</span><span class="sxs-lookup"><span data-stu-id="b274f-164">On hello **Portal Settings** blade, under **Security rules**, click **User compromise risk**.</span></span> 
5. <span data-ttu-id="b274f-165">På hello **logga in Risk** bladet aktivera **aktivera regeln** inaktiverat och klicka sedan på **spara** inställningar.</span><span class="sxs-lookup"><span data-stu-id="b274f-165">On hello **Sign in Risk** blade, turn **Enable rule** off, and then click **Save** settings.</span></span>
6. <span data-ttu-id="b274f-166">För ett givet användarkonto simulera en okänd platser eller anonym IP-händelsen för risk.</span><span class="sxs-lookup"><span data-stu-id="b274f-166">For a given user account, simulate an unfamiliar locations or anonymous IP risk event.</span></span> <span data-ttu-id="b274f-167">Detta kommer höjer hello användaren risknivå för användaren för**medel**.</span><span class="sxs-lookup"><span data-stu-id="b274f-167">This will elevate hello user risk level for that user too**Medium**.</span></span>
7. <span data-ttu-id="b274f-168">Vänta en stund och kontrollera att användarnivå för dina användare är **medel**.</span><span class="sxs-lookup"><span data-stu-id="b274f-168">Wait a few minutes, and then verify that user level for your user is **Medium**.</span></span>
8. <span data-ttu-id="b274f-169">Gå toohello **portalinställningar** bladet.</span><span class="sxs-lookup"><span data-stu-id="b274f-169">Go toohello **Portal Settings** blade.</span></span>
9. <span data-ttu-id="b274f-170">På hello **användaren risken för angrepp** bladet under **aktivera regeln**väljer **på** .</span><span class="sxs-lookup"><span data-stu-id="b274f-170">On hello **User Compromise Risk** blade, under **Enable rule**, select **On** .</span></span> 
10. <span data-ttu-id="b274f-171">Välj något av följande alternativ för hello:</span><span class="sxs-lookup"><span data-stu-id="b274f-171">Select one of hello following options:</span></span>
    
    <span data-ttu-id="b274f-172">a.</span><span class="sxs-lookup"><span data-stu-id="b274f-172">a.</span></span> <span data-ttu-id="b274f-173">tooblock, Välj **medel** under **Block logga in**.</span><span class="sxs-lookup"><span data-stu-id="b274f-173">tooblock, select **Medium** under **Block sign in**.</span></span>
    
    <span data-ttu-id="b274f-174">b.</span><span class="sxs-lookup"><span data-stu-id="b274f-174">b.</span></span> <span data-ttu-id="b274f-175">tooenforce säker lösenordsändring väljer **medel** under **kräver Multi-Factor authentication**.</span><span class="sxs-lookup"><span data-stu-id="b274f-175">tooenforce secure password change, select **Medium** under **Require multi-factor authentication**.</span></span>
11. <span data-ttu-id="b274f-176">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="b274f-176">Click **Save**.</span></span>
12. <span data-ttu-id="b274f-177">Nu kan du testa risk-baserad villkorlig åtkomst genom att logga in med hjälp av en användare med en ökad risk för nivå.</span><span class="sxs-lookup"><span data-stu-id="b274f-177">You can now test risk-based conditional access by signing in using a user with an elevated risk level.</span></span> <span data-ttu-id="b274f-178">Om hello användaren risken är medel, beroende på hello konfigurationen av din princip är din inloggning är blockeras antingen eller tvingas du toochange ditt lösenord.</span><span class="sxs-lookup"><span data-stu-id="b274f-178">If hello user risk is Medium, depending on hello configuration of your policy, your sign-in is be either blocked or you are forced toochange your password.</span></span> 
    <br><br><span data-ttu-id="b274f-179">
    ![Playbook](./media/active-directory-identityprotection-playbook/201.png "Playbook")
    </span><span class="sxs-lookup"><span data-stu-id="b274f-179">
![Playbook](./media/active-directory-identityprotection-playbook/201.png "Playbook")
</span></span><br>

## <a name="sign-in-risk"></a><span data-ttu-id="b274f-180">Logga in risk</span><span class="sxs-lookup"><span data-stu-id="b274f-180">Sign-in risk</span></span>
<span data-ttu-id="b274f-181">**tootest en inloggning risk, utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="b274f-181">**tootest a sign in risk, perform hello following steps:**</span></span>

1. <span data-ttu-id="b274f-182">Logga in för[https://portal.azure.com ](https://portal.azure.com) med globala administratörsbehörigheter för din klient.</span><span class="sxs-lookup"><span data-stu-id="b274f-182">Sign-in too[https://portal.azure.com ](https://portal.azure.com) with global administrator credentials for your tenant.</span></span>
2. <span data-ttu-id="b274f-183">Navigera för**identitetsskydd**.</span><span class="sxs-lookup"><span data-stu-id="b274f-183">Navigate too**Identity Protection**.</span></span>
3. <span data-ttu-id="b274f-184">På hello huvudsakliga **Azure AD Identity Protection** bladet, klickar du på **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="b274f-184">On hello main **Azure AD Identity Protection** blade, click **Settings**.</span></span> 
4. <span data-ttu-id="b274f-185">På hello **portalinställningar** bladet under **säkerhetsregler**, klickar du på **logga in risk**.</span><span class="sxs-lookup"><span data-stu-id="b274f-185">On hello **Portal Settings** blade, under **Security rules**, click **Sign in risk**.</span></span>
5. <span data-ttu-id="b274f-186">På hello ** logga in Risk ** bladet väljer **på** under **aktivera regeln**.</span><span class="sxs-lookup"><span data-stu-id="b274f-186">On hello **Sign in Risk **blade, select **On** under **Enable rule**.</span></span> 
6. <span data-ttu-id="b274f-187">Välj något av följande alternativ för hello:</span><span class="sxs-lookup"><span data-stu-id="b274f-187">Select one of hello following options:</span></span>
   
   <span data-ttu-id="b274f-188">a.</span><span class="sxs-lookup"><span data-stu-id="b274f-188">a.</span></span> <span data-ttu-id="b274f-189">tooblock, Välj **medel** under **Block logga in**</span><span class="sxs-lookup"><span data-stu-id="b274f-189">tooblock, select **Medium** under **Block sign in**</span></span>
   
   <span data-ttu-id="b274f-190">b.</span><span class="sxs-lookup"><span data-stu-id="b274f-190">b.</span></span> <span data-ttu-id="b274f-191">tooenforce säker lösenordsändring väljer **medel** under **kräver Multi-Factor authentication**.</span><span class="sxs-lookup"><span data-stu-id="b274f-191">tooenforce secure password change, select **Medium** under **Require multi-factor authentication**.</span></span>
7. <span data-ttu-id="b274f-192">tooblock, Välj medel under Block logga in.</span><span class="sxs-lookup"><span data-stu-id="b274f-192">tooblock, select Medium under Block sign in.</span></span>
8. <span data-ttu-id="b274f-193">tooenforce multifaktorautentisering, Välj **medel** under **kräver Multi-Factor authentication**.</span><span class="sxs-lookup"><span data-stu-id="b274f-193">tooenforce multi-factor authentication, select **Medium** under **Require multi-factor authentication**.</span></span>
9. <span data-ttu-id="b274f-194">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="b274f-194">Click on **Save**.</span></span>
10. <span data-ttu-id="b274f-195">Nu kan du testa risk-baserad villkorlig åtkomst genom att simulera hello okända platser eller anonym IP riskerar händelser eftersom de båda **medel** riskerar händelser.</span><span class="sxs-lookup"><span data-stu-id="b274f-195">You can now test risk-based conditional access by simulating hello unfamiliar locations or anonymous IP risk events because they are both **Medium** risk events.</span></span>


<span data-ttu-id="b274f-196">![Playbook](./media/active-directory-identityprotection-playbook/200.png "Playbook")</span><span class="sxs-lookup"><span data-stu-id="b274f-196">![Playbook](./media/active-directory-identityprotection-playbook/200.png "Playbook")</span></span>


## <a name="see-also"></a><span data-ttu-id="b274f-197">Se även</span><span class="sxs-lookup"><span data-stu-id="b274f-197">See also</span></span>
* [<span data-ttu-id="b274f-198">Azure Active Directory Identity Protection</span><span class="sxs-lookup"><span data-stu-id="b274f-198">Azure Active Directory Identity Protection</span></span>](active-directory-identityprotection.md)

