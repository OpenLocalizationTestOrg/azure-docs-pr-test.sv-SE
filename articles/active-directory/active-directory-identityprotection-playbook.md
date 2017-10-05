---
title: Azure Active Directory-identitetsskydd playbook | Microsoft Docs
description: "Lär dig hur Azure AD Identity Protection gör att du kan begränsa möjligheten för en angripare som utnyttjar en komprometterad identitet eller en enhet och att skydda en identitet eller en enhet som har tidigare eller misstänks vara hotad."
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
ms.openlocfilehash: 2ecd07faed785fa6aa179ac1cca35a70d965e1dc
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="azure-active-directory-identity-protection-playbook"></a><span data-ttu-id="b8263-104">Azure Active Directory-identitetsskydd playbook</span><span class="sxs-lookup"><span data-stu-id="b8263-104">Azure Active Directory Identity Protection playbook</span></span>
<span data-ttu-id="b8263-105">Den här playbook hjälper dig att:</span><span class="sxs-lookup"><span data-stu-id="b8263-105">This playbook helps you to:</span></span>

* <span data-ttu-id="b8263-106">Fyll i data i Identity Protection-miljö genom simulering riskhändelser och säkerhetsproblem</span><span class="sxs-lookup"><span data-stu-id="b8263-106">Populate data in the Identity Protection environment by simulating risk events and vulnerabilities</span></span>
* <span data-ttu-id="b8263-107">Konfigurera principer för risk-baserad villkorlig åtkomst och testa effekten av dessa principer</span><span class="sxs-lookup"><span data-stu-id="b8263-107">Set up risk-based conditional access policies and test the impact of these policies</span></span>

## <a name="simulating-risk-events"></a><span data-ttu-id="b8263-108">Simulera riskhändelser</span><span class="sxs-lookup"><span data-stu-id="b8263-108">Simulating Risk Events</span></span>
<span data-ttu-id="b8263-109">Det här avsnittet innehåller steg för att simulera händelsetyper för följande risker:</span><span class="sxs-lookup"><span data-stu-id="b8263-109">This section provides you with steps for simulating the following risk event types:</span></span>

* <span data-ttu-id="b8263-110">Inloggningar från anonyma IP-adresser (enkel)</span><span class="sxs-lookup"><span data-stu-id="b8263-110">Sign-ins from anonymous IP addresses (easy)</span></span>
* <span data-ttu-id="b8263-111">Inloggningar från okända platser (måttlig)</span><span class="sxs-lookup"><span data-stu-id="b8263-111">Sign-ins from unfamiliar locations (moderate)</span></span>
* <span data-ttu-id="b8263-112">Omöjligt att resa till ovanliga platser (svårt)</span><span class="sxs-lookup"><span data-stu-id="b8263-112">Impossible travel to atypical locations (difficult)</span></span>

<span data-ttu-id="b8263-113">Andra riskhändelser kan simuleras på ett säkert sätt.</span><span class="sxs-lookup"><span data-stu-id="b8263-113">Other risk events cannot be simulated in a secure manner.</span></span>

### <a name="sign-ins-from-anonymous-ip-addresses"></a><span data-ttu-id="b8263-114">Inloggningar från anonyma IP-adresser</span><span class="sxs-lookup"><span data-stu-id="b8263-114">Sign-ins from anonymous IP addresses</span></span>
<span data-ttu-id="b8263-115">Den här typen av risk händelse identifierar användare som har loggat in från en IP-adress har identifierats som en anonym proxyserver IP-adress.</span><span class="sxs-lookup"><span data-stu-id="b8263-115">This risk event type identifies users who have successfully signed in from an IP address that has been identified as an anonymous proxy IP address.</span></span> <span data-ttu-id="b8263-116">Dessa proxyservrar används av personer som du vill dölja enhetens IP-adress och kan användas för skadliga åtgärder.</span><span class="sxs-lookup"><span data-stu-id="b8263-116">These proxies are used by people who want to hide their device’s IP address and may be used for malicious intent.</span></span>

<span data-ttu-id="b8263-117">**Utför följande steg för att simulera en inloggning från en anonym IP**:</span><span class="sxs-lookup"><span data-stu-id="b8263-117">**To simulate a sign-in from an anonymous IP, perform the following steps**:</span></span>

1. <span data-ttu-id="b8263-118">Hämta den [Tor webbläsare](https://www.torproject.org/projects/torbrowser.html.en).</span><span class="sxs-lookup"><span data-stu-id="b8263-118">Download the [Tor Browser](https://www.torproject.org/projects/torbrowser.html.en).</span></span>
2. <span data-ttu-id="b8263-119">Använd Tor Browser, navigera till [https://myapps.microsoft.com](https://myapps.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="b8263-119">Using the Tor Browser, navigate to [https://myapps.microsoft.com](https://myapps.microsoft.com).</span></span>   
3. <span data-ttu-id="b8263-120">Ange autentiseringsuppgifter för det konto som du vill ska visas i den **inloggningar från anonyma IP-adresser** rapporten.</span><span class="sxs-lookup"><span data-stu-id="b8263-120">Enter the credentials of the account you want to appear in the **Sign-ins from anonymous IP addresses** report.</span></span>

<span data-ttu-id="b8263-121">Logga in kommer att visas på instrumentpanelen identitetsskydd inom 5 minuter.</span><span class="sxs-lookup"><span data-stu-id="b8263-121">The sign-in will show up on the Identity Protection dashboard within 5 minutes.</span></span> 

### <a name="sign-ins-from-unfamiliar-locations"></a><span data-ttu-id="b8263-122">Inloggningar från okända platser</span><span class="sxs-lookup"><span data-stu-id="b8263-122">Sign-ins from unfamiliar locations</span></span>
<span data-ttu-id="b8263-123">Okända platser risk är en mekanism för realtid inloggning utvärdering som överväger tidigare inloggning platser (IP, latitud / longitud och ASN) att fastställa nya / okända platser.</span><span class="sxs-lookup"><span data-stu-id="b8263-123">The unfamiliar locations risk is a real-time sign-in evaluation mechanism that considers past sign-in locations (IP, Latitude / Longitude and ASN) to determine new / unfamiliar locations.</span></span> <span data-ttu-id="b8263-124">Systemet lagrar tidigare IP-adresser, latitud / longitud och ASN: er för en användare så att de ska vara bekant platser.</span><span class="sxs-lookup"><span data-stu-id="b8263-124">The system stores previous IPs, Latitude / Longitude, and ASNs of a user and considers these to be familiar locations.</span></span> <span data-ttu-id="b8263-125">Okänd anses vara en inloggning plats om platsen för inloggning inte matchar någon av de befintliga bekanta platserna.</span><span class="sxs-lookup"><span data-stu-id="b8263-125">A sign-in location is considered unfamiliar if the sign-in location does not match any of the existing familiar locations.</span></span>

<span data-ttu-id="b8263-126">Azure Active Directory identitetsskydd:</span><span class="sxs-lookup"><span data-stu-id="b8263-126">Azure Active Directory Identity Protection:</span></span>  

* <span data-ttu-id="b8263-127">har en inledande learning-period på 14 dagar då inte flaggas några nya platser som okända platser.</span><span class="sxs-lookup"><span data-stu-id="b8263-127">has an initial learning period of 14 days during which it does not flag any new locations as unfamiliar locations.</span></span>
* <span data-ttu-id="b8263-128">ignorerar inloggningar från bekant enheter och platser som är geografiskt nära en befintlig bekant plats.</span><span class="sxs-lookup"><span data-stu-id="b8263-128">ignores sign-ins from familiar devices and locations that are geographically close to an existing familiar location.</span></span>

<span data-ttu-id="b8263-129">Du måste logga in från en plats och en enhet som kontot inte har loggat in från innan för att simulera okända platser.</span><span class="sxs-lookup"><span data-stu-id="b8263-129">To simulate unfamiliar locations, you have to sign in from a location and device that the account has not signed in from before.</span></span> 

<span data-ttu-id="b8263-130">**Utför följande steg för att simulera en inloggning från en okänd plats**:</span><span class="sxs-lookup"><span data-stu-id="b8263-130">**To simulate a sign-in from an unfamiliar location, perform the following steps**:</span></span>

1. <span data-ttu-id="b8263-131">Välj ett konto som har minst en 14 dagar inloggning historik.</span><span class="sxs-lookup"><span data-stu-id="b8263-131">Choose an account that has at least a 14-day sign-in history.</span></span> 
2. <span data-ttu-id="b8263-132">Gör något:</span><span class="sxs-lookup"><span data-stu-id="b8263-132">Do either:</span></span>
   
   <span data-ttu-id="b8263-133">a.</span><span class="sxs-lookup"><span data-stu-id="b8263-133">a.</span></span> <span data-ttu-id="b8263-134">När du använder en VPN-anslutning, gå till [https://myapps.microsoft.com](https://myapps.microsoft.com) och ange autentiseringsuppgifter för det konto som du vill simulera händelsen risken för.</span><span class="sxs-lookup"><span data-stu-id="b8263-134">While using a VPN, navigate to [https://myapps.microsoft.com](https://myapps.microsoft.com) and enter the credentials of the account you want to simulate the risk event for.</span></span>
   
   <span data-ttu-id="b8263-135">b.</span><span class="sxs-lookup"><span data-stu-id="b8263-135">b.</span></span> <span data-ttu-id="b8263-136">Be en kollega på en annan plats att logga in med autentiseringsuppgifter (rekommenderas inte).</span><span class="sxs-lookup"><span data-stu-id="b8263-136">Ask an associate in a different location to sign in using the account’s credentials (not recommended).</span></span>

<span data-ttu-id="b8263-137">Logga in kommer att visas på instrumentpanelen identitetsskydd inom 5 minuter.</span><span class="sxs-lookup"><span data-stu-id="b8263-137">The sign-in will show up on the Identity Protection dashboard within 5 minutes.</span></span>

### <a name="impossible-travel-to-atypical-location"></a><span data-ttu-id="b8263-138">Omöjligt att resa till ovanliga plats</span><span class="sxs-lookup"><span data-stu-id="b8263-138">Impossible travel to atypical location</span></span>
<span data-ttu-id="b8263-139">Det är svårt att simulera villkoret omöjligt att resa eftersom algoritmen använder maskininlärning att filtrera ut FALSKT positiva identifieringar som omöjligt att resa från bekant enheter eller inloggningar från VPN-anslutningar som används av andra användare i katalogen.</span><span class="sxs-lookup"><span data-stu-id="b8263-139">Simulating the impossible travel condition is difficult because the algorithm uses machine learning to weed out false-positives such as impossible travel from familiar devices, or sign-ins from VPNs that are used by other users in the directory.</span></span> <span data-ttu-id="b8263-140">Dessutom kräver algoritmen en historik av 3 till 14 dagar för användaren innan den börjar skapa riskhändelser.</span><span class="sxs-lookup"><span data-stu-id="b8263-140">Additionally, the algorithm requires a sign-in history of 3 to 14 days for the user before it begins generating risk events.</span></span>

<span data-ttu-id="b8263-141">**Utför följande steg för att simulera en omöjligt att resa till ovanliga plats**:</span><span class="sxs-lookup"><span data-stu-id="b8263-141">**To simulate an impossible travel to atypical location, perform the following steps**:</span></span>

1. <span data-ttu-id="b8263-142">Använd din standard-webbläsare, navigera till [https://myapps.microsoft.com](https://myapps.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="b8263-142">Using your standard browser, navigate to [https://myapps.microsoft.com](https://myapps.microsoft.com).</span></span>  
2. <span data-ttu-id="b8263-143">Ange autentiseringsuppgifter för det konto som du vill generera en händelse med omöjliga resor risken för.</span><span class="sxs-lookup"><span data-stu-id="b8263-143">Enter the credentials of the account you want to generate an impossible travel risk event for.</span></span>
3. <span data-ttu-id="b8263-144">Ändra din användaragent.</span><span class="sxs-lookup"><span data-stu-id="b8263-144">Change your user agent.</span></span> <span data-ttu-id="b8263-145">Du kan ändra användaragent i Internet Explorer från utvecklingsverktyg eller ändra din användaragent i Firefox eller Chrome med en användaragent switcher tillägg.</span><span class="sxs-lookup"><span data-stu-id="b8263-145">You can change user agent in Internet Explorer from Developer Tools, or change your user agent in Firefox or Chrome using a user-agent switcher add-on.</span></span>
4. <span data-ttu-id="b8263-146">Ändra IP-adress.</span><span class="sxs-lookup"><span data-stu-id="b8263-146">Change your IP address.</span></span> <span data-ttu-id="b8263-147">Du kan ändra din IP-adress med hjälp av en VPN-anslutning, Tor-tillägg eller startas en ny dator i Azure i olika datacenter.</span><span class="sxs-lookup"><span data-stu-id="b8263-147">You can change your IP address by using a VPN, a Tor add-on, or spinning up a new machine in Azure in a different data center.</span></span>
5. <span data-ttu-id="b8263-148">Logga in på [https://myapps.microsoft.com](https://myapps.microsoft.com) använder du samma inloggningsuppgifter som förut och inom några minuter efter den föregående inloggningen.</span><span class="sxs-lookup"><span data-stu-id="b8263-148">Sign-in to [https://myapps.microsoft.com](https://myapps.microsoft.com) using the same credentials as before and within a few minutes after the previous sign-in.</span></span>

<span data-ttu-id="b8263-149">Logga in visas i instrumentpanelen för identitetsskydd inom 2-4 timmar.</span><span class="sxs-lookup"><span data-stu-id="b8263-149">The sign-in will show up in the Identity Protection dashboard within 2-4 hours.</span></span><br>
<span data-ttu-id="b8263-150">På grund av den komplexa maskininlärning modeller som ingår, ökar risken för den inte får tas upp.</span><span class="sxs-lookup"><span data-stu-id="b8263-150">Because of the complex machine learning models involved, there is a chance it will not get picked up.</span></span><br> <span data-ttu-id="b8263-151">Du kanske vill replikera de här stegen för flera Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="b8263-151">You might want to replicate these steps for multiple Azure AD accounts.</span></span>

## <a name="simulating-vulnerabilities"></a><span data-ttu-id="b8263-152">Simulera säkerhetsrisker</span><span class="sxs-lookup"><span data-stu-id="b8263-152">Simulating vulnerabilities</span></span>
<span data-ttu-id="b8263-153">Säkerhetsrisker är svagheter i en Azure AD-miljö som kan utnyttjas av en felaktig aktören.</span><span class="sxs-lookup"><span data-stu-id="b8263-153">Vulnerabilities are weaknesses in an Azure AD environment that can be exploited by a bad actor.</span></span> <span data-ttu-id="b8263-154">För närvarande exponeras 3 typer av säkerhetsproblem i Azure AD Identity Protection som utnyttjar andra funktioner i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b8263-154">Currently 3 types of vulnerabilities are surfaced in Azure AD Identity Protection that leverage other features of Azure AD.</span></span> <span data-ttu-id="b8263-155">Dessa problem visas på instrumentpanelen identitetsskydd automatiskt när dessa funktioner har ställts in.</span><span class="sxs-lookup"><span data-stu-id="b8263-155">These Vulnerabilities will be displayed on the Identity Protection dashboard automatically once these features are set up.</span></span>

* <span data-ttu-id="b8263-156">Azure AD [Multi-Factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md)</span><span class="sxs-lookup"><span data-stu-id="b8263-156">Azure AD [Multi-Factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md)</span></span>
* <span data-ttu-id="b8263-157">Azure AD [Cloud App Discovery](active-directory-cloudappdiscovery-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="b8263-157">Azure AD [Cloud App Discovery](active-directory-cloudappdiscovery-whatis.md).</span></span>
* <span data-ttu-id="b8263-158">Azure AD [Privileged Identity Management](active-directory-privileged-identity-management-configure.md).</span><span class="sxs-lookup"><span data-stu-id="b8263-158">Azure AD [Privileged Identity Management](active-directory-privileged-identity-management-configure.md).</span></span> 

## <a name="user-compromise-risk"></a><span data-ttu-id="b8263-159">Användaren har komprometterats risk</span><span class="sxs-lookup"><span data-stu-id="b8263-159">User compromise risk</span></span>
<span data-ttu-id="b8263-160">**Utför följande steg om du vill testa användare risken för angrepp**:</span><span class="sxs-lookup"><span data-stu-id="b8263-160">**To test User compromise risk, perform the following steps**:</span></span>

1. <span data-ttu-id="b8263-161">Logga in på [https://portal.azure.com](https://portal.azure.com) med globala administratörsbehörigheter för din klient.</span><span class="sxs-lookup"><span data-stu-id="b8263-161">Sign-in to [https://portal.azure.com](https://portal.azure.com) with global administrator credentials for your tenant.</span></span>
2. <span data-ttu-id="b8263-162">Gå till **identitetsskydd**.</span><span class="sxs-lookup"><span data-stu-id="b8263-162">Navigate to **Identity Protection**.</span></span> 
3. <span data-ttu-id="b8263-163">På primära **Azure AD Identity Protection** bladet, klickar du på **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="b8263-163">On the main **Azure AD Identity Protection** blade, click **Settings**.</span></span> 
4. <span data-ttu-id="b8263-164">På den **portalinställningar** bladet under **säkerhetsregler**, klickar du på **användaren risken för angrepp**.</span><span class="sxs-lookup"><span data-stu-id="b8263-164">On the **Portal Settings** blade, under **Security rules**, click **User compromise risk**.</span></span> 
5. <span data-ttu-id="b8263-165">På den **logga in Risk** bladet aktivera **aktivera regeln** inaktiverat och klicka sedan på **spara** inställningar.</span><span class="sxs-lookup"><span data-stu-id="b8263-165">On the **Sign in Risk** blade, turn **Enable rule** off, and then click **Save** settings.</span></span>
6. <span data-ttu-id="b8263-166">För ett givet användarkonto simulera en okänd platser eller anonym IP-händelsen för risk.</span><span class="sxs-lookup"><span data-stu-id="b8263-166">For a given user account, simulate an unfamiliar locations or anonymous IP risk event.</span></span> <span data-ttu-id="b8263-167">Detta ska upphöja användaren risknivå för **medel**.</span><span class="sxs-lookup"><span data-stu-id="b8263-167">This will elevate the user risk level for that user to **Medium**.</span></span>
7. <span data-ttu-id="b8263-168">Vänta en stund och kontrollera att användarnivå för dina användare är **medel**.</span><span class="sxs-lookup"><span data-stu-id="b8263-168">Wait a few minutes, and then verify that user level for your user is **Medium**.</span></span>
8. <span data-ttu-id="b8263-169">Gå till den **portalinställningar** bladet.</span><span class="sxs-lookup"><span data-stu-id="b8263-169">Go to the **Portal Settings** blade.</span></span>
9. <span data-ttu-id="b8263-170">På den **användaren risken för angrepp** bladet under **aktivera regeln**väljer **på** .</span><span class="sxs-lookup"><span data-stu-id="b8263-170">On the **User Compromise Risk** blade, under **Enable rule**, select **On** .</span></span> 
10. <span data-ttu-id="b8263-171">Välj något av följande alternativ:</span><span class="sxs-lookup"><span data-stu-id="b8263-171">Select one of the following options:</span></span>
    
    <span data-ttu-id="b8263-172">a.</span><span class="sxs-lookup"><span data-stu-id="b8263-172">a.</span></span> <span data-ttu-id="b8263-173">Att blockera, Välj **medel** under **Block logga in**.</span><span class="sxs-lookup"><span data-stu-id="b8263-173">To block, select **Medium** under **Block sign in**.</span></span>
    
    <span data-ttu-id="b8263-174">b.</span><span class="sxs-lookup"><span data-stu-id="b8263-174">b.</span></span> <span data-ttu-id="b8263-175">Om du vill framtvinga ändring av säkra lösenord, Välj **medel** under **kräver Multi-Factor authentication**.</span><span class="sxs-lookup"><span data-stu-id="b8263-175">To enforce secure password change, select **Medium** under **Require multi-factor authentication**.</span></span>
11. <span data-ttu-id="b8263-176">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="b8263-176">Click **Save**.</span></span>
12. <span data-ttu-id="b8263-177">Nu kan du testa risk-baserad villkorlig åtkomst genom att logga in med hjälp av en användare med en ökad risk för nivå.</span><span class="sxs-lookup"><span data-stu-id="b8263-177">You can now test risk-based conditional access by signing in using a user with an elevated risk level.</span></span> <span data-ttu-id="b8263-178">Om användaren risken medel, beroende på konfigurationen av din princip för inloggningen är blockeras antingen eller du måste ändra ditt lösenord.</span><span class="sxs-lookup"><span data-stu-id="b8263-178">If the user risk is Medium, depending on the configuration of your policy, your sign-in is be either blocked or you are forced to change your password.</span></span> 
    <br><br><span data-ttu-id="b8263-179">
    ![Playbook](./media/active-directory-identityprotection-playbook/201.png "Playbook")
    </span><span class="sxs-lookup"><span data-stu-id="b8263-179">
![Playbook](./media/active-directory-identityprotection-playbook/201.png "Playbook")
</span></span><br>

## <a name="sign-in-risk"></a><span data-ttu-id="b8263-180">Logga in risk</span><span class="sxs-lookup"><span data-stu-id="b8263-180">Sign-in risk</span></span>
<span data-ttu-id="b8263-181">**Om du vill testa ett tecken i risk, utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="b8263-181">**To test a sign in risk, perform the following steps:**</span></span>

1. <span data-ttu-id="b8263-182">Logga in på [https://portal.azure.com ](https://portal.azure.com) med globala administratörsbehörigheter för din klient.</span><span class="sxs-lookup"><span data-stu-id="b8263-182">Sign-in to [https://portal.azure.com ](https://portal.azure.com) with global administrator credentials for your tenant.</span></span>
2. <span data-ttu-id="b8263-183">Gå till **identitetsskydd**.</span><span class="sxs-lookup"><span data-stu-id="b8263-183">Navigate to **Identity Protection**.</span></span>
3. <span data-ttu-id="b8263-184">På primära **Azure AD Identity Protection** bladet, klickar du på **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="b8263-184">On the main **Azure AD Identity Protection** blade, click **Settings**.</span></span> 
4. <span data-ttu-id="b8263-185">På den **portalinställningar** bladet under **säkerhetsregler**, klickar du på **logga in risk**.</span><span class="sxs-lookup"><span data-stu-id="b8263-185">On the **Portal Settings** blade, under **Security rules**, click **Sign in risk**.</span></span>
5. <span data-ttu-id="b8263-186">På den ** logga in Risk ** bladet väljer **på** under **aktivera regeln**.</span><span class="sxs-lookup"><span data-stu-id="b8263-186">On the **Sign in Risk **blade, select **On** under **Enable rule**.</span></span> 
6. <span data-ttu-id="b8263-187">Välj något av följande alternativ:</span><span class="sxs-lookup"><span data-stu-id="b8263-187">Select one of the following options:</span></span>
   
   <span data-ttu-id="b8263-188">a.</span><span class="sxs-lookup"><span data-stu-id="b8263-188">a.</span></span> <span data-ttu-id="b8263-189">Om du vill blockera, Välj **medel** under **Block logga in**</span><span class="sxs-lookup"><span data-stu-id="b8263-189">To block, select **Medium** under **Block sign in**</span></span>
   
   <span data-ttu-id="b8263-190">b.</span><span class="sxs-lookup"><span data-stu-id="b8263-190">b.</span></span> <span data-ttu-id="b8263-191">Om du vill framtvinga ändring av säkra lösenord, Välj **medel** under **kräver Multi-Factor authentication**.</span><span class="sxs-lookup"><span data-stu-id="b8263-191">To enforce secure password change, select **Medium** under **Require multi-factor authentication**.</span></span>
7. <span data-ttu-id="b8263-192">Om du vill blockera, Välj Medium under Block inloggning.</span><span class="sxs-lookup"><span data-stu-id="b8263-192">To block, select Medium under Block sign in.</span></span>
8. <span data-ttu-id="b8263-193">Om du vill framtvinga multifaktorautentisering, Välj **medel** under **kräver Multi-Factor authentication**.</span><span class="sxs-lookup"><span data-stu-id="b8263-193">To enforce multi-factor authentication, select **Medium** under **Require multi-factor authentication**.</span></span>
9. <span data-ttu-id="b8263-194">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="b8263-194">Click on **Save**.</span></span>
10. <span data-ttu-id="b8263-195">Nu kan du testa risk-baserad villkorlig åtkomst genom att simulera okända platser eller anonym IP riskerar händelser eftersom de båda **medel** riskerar händelser.</span><span class="sxs-lookup"><span data-stu-id="b8263-195">You can now test risk-based conditional access by simulating the unfamiliar locations or anonymous IP risk events because they are both **Medium** risk events.</span></span>


<span data-ttu-id="b8263-196">![Playbook](./media/active-directory-identityprotection-playbook/200.png "Playbook")</span><span class="sxs-lookup"><span data-stu-id="b8263-196">![Playbook](./media/active-directory-identityprotection-playbook/200.png "Playbook")</span></span>


## <a name="see-also"></a><span data-ttu-id="b8263-197">Se även</span><span class="sxs-lookup"><span data-stu-id="b8263-197">See also</span></span>
* [<span data-ttu-id="b8263-198">Azure Active Directory Identity Protection</span><span class="sxs-lookup"><span data-stu-id="b8263-198">Azure Active Directory Identity Protection</span></span>](active-directory-identityprotection.md)

