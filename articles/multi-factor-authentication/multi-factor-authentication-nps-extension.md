---
title: "Använda befintliga NPS-servrar för att ge funktioner för Azure MFA | Microsoft Docs"
description: "NPS-tillägget för Azure Multi-Factor Authentication är en enkel lösning för att lägga till molnbaserade tvåstegsverifiering vericiation funktioner i din befintliga infrastruktur för autentisering."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: H1Hack27Feb2017; it-pro
ms.openlocfilehash: fa125292ee85bd9b5329cffeff7f076d3002cbf3
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="integrate-your-existing-nps-infrastructure-with-azure-multi-factor-authentication"></a><span data-ttu-id="1aa0b-103">Integrera din befintliga infrastruktur för NPS med Azure Multi-Factor Authentication</span><span class="sxs-lookup"><span data-stu-id="1aa0b-103">Integrate your existing NPS infrastructure with Azure Multi-Factor Authentication</span></span>

<span data-ttu-id="1aa0b-104">Server (NPS)-tillägget för Azure MFA lägger till funktioner för molnbaserade MFA till din infrastruktur för autentisering med hjälp av din befintliga servrar.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-104">The Network Policy Server (NPS) extension for Azure MFA adds cloud-based MFA capabilities to your authentication infrastructure using your existing servers.</span></span> <span data-ttu-id="1aa0b-105">Med filnamnstillägget NPS du kan lägga till telefonsamtal, textmeddelande eller app telefonverifiering din befintliga autentiseringsflödet utan att behöva installera, konfigurera och underhålla nya servrar.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-105">With the NPS extension, you can add phone call, text message, or phone app verification to your existing authentication flow without having to install, configure, and maintain new servers.</span></span> 

<span data-ttu-id="1aa0b-106">Det här tillägget har skapats för organisationer som vill skydda VPN-anslutningar utan att distribuera Azure MFA-Server.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-106">This extension was created for organizations that want to protect VPN connections without deploying the Azure MFA Server.</span></span> <span data-ttu-id="1aa0b-107">NPS-tillägget fungerar som ett kort mellan RADIUS och molnbaserad Azure MFA för att tillhandahålla en andra faktor-autentisering för federerad eller synkroniserade användare.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-107">The NPS extension acts as an adapter between RADIUS and cloud-based Azure MFA to provide a second factor of authentication for federated or synced users.</span></span>

<span data-ttu-id="1aa0b-108">När du använder NPS-tillägget för Azure MFA innehåller autentiseringsflödet följande komponenter:</span><span class="sxs-lookup"><span data-stu-id="1aa0b-108">When using the NPS extension for Azure MFA, the authentication flow includes the following components:</span></span> 

1. <span data-ttu-id="1aa0b-109">**NAS-VPN-Server** tar emot förfrågningar från VPN-klienter och konverterar dem till RADIUS-förfrågningar till NPS-servrar.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-109">**NAS/VPN Server** receives requests from VPN clients and converts them into RADIUS requests to NPS servers.</span></span> 
2. <span data-ttu-id="1aa0b-110">**NPS-servern** ansluter till Active Directory för att utföra den primära autentiseringen för RADIUS-begäranden och, vid lyckades, skickar begäran till alla installerade tillägg.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-110">**NPS Server** connects to Active Directory to perform the primary authentication for the RADIUS requests and, upon success, passes the request to any installed extensions.</span></span>  
3. <span data-ttu-id="1aa0b-111">**NPS-tillägget** utlöser en begäran om att Azure MFA för den sekundära autentiseringen.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-111">**NPS Extension** triggers a request to Azure MFA for the secondary authentication.</span></span> <span data-ttu-id="1aa0b-112">När tillägget tar emot svaret och om MFA-kontrollen lyckas, den är klar autentiseringsbegäran genom att tillhandahålla NPS-servern med säkerhetstoken som innehåller ett MFA anspråk kan utfärdas av Azure STS.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-112">Once the extension receives the response, and if the MFA challenge succeeds, it completes the authentication request by providing the NPS server with security tokens that include an MFA claim, issued by Azure STS.</span></span>  
4. <span data-ttu-id="1aa0b-113">**Azure MFA** kommunicerar med Azure Active Directory för att hämta information om användarens och utför sekundära autentiseringen med hjälp av en verifieringsmetod som konfigurerats för användaren.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-113">**Azure MFA** communicates with Azure Active Directory to retrieve the user’s details and performs the secondary authentication using a verification method configured to the user.</span></span>

<span data-ttu-id="1aa0b-114">Följande diagram illustrerar det här övergripande autentiseringsflödet för begäran:</span><span class="sxs-lookup"><span data-stu-id="1aa0b-114">The following diagram illustrates this high-level authentication request flow:</span></span> 

![Flödesdiagram för autentisering](./media/multi-factor-authentication-nps-extension/auth-flow.png)

## <a name="plan-your-deployment"></a><span data-ttu-id="1aa0b-116">Planera distributionen</span><span class="sxs-lookup"><span data-stu-id="1aa0b-116">Plan your deployment</span></span>

<span data-ttu-id="1aa0b-117">NPS-tillägget hanterar automatiskt redundans, så du behöver en särskild konfiguration.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-117">The NPS extension automatically handles redundancy, so you don't need a special configuration.</span></span>

<span data-ttu-id="1aa0b-118">Du kan skapa så många Azure MFA-aktiverade NPS-servrar som du behöver.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-118">You can create as many Azure MFA-enabled NPS servers as you need.</span></span> <span data-ttu-id="1aa0b-119">Om du installerar flera servrar, bör du använda ett certifikat för skillnaden för var och en av dem.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-119">If you do install multiple servers, you should use a difference client certificate for each one of them.</span></span> <span data-ttu-id="1aa0b-120">När du skapar ett certifikat för varje server innebär att du kan uppdatera varje cert individuellt och oroa dig inte om driftstopp på alla servrar.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-120">Creating a cert for each server means that you can update each cert individually, and not worry about downtime across all your servers.</span></span>

<span data-ttu-id="1aa0b-121">VPN-servrar vidarebefordra autentiseringsbegäranden, så att de behöver känna till de nya Azure MFA-aktiverade NPS-servrarna.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-121">VPN servers route authentication requests, so they need to be aware of the new Azure MFA-enabled NPS servers.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1aa0b-122">Krav</span><span class="sxs-lookup"><span data-stu-id="1aa0b-122">Prerequisites</span></span>

<span data-ttu-id="1aa0b-123">NPS-tillägget är avsedda att fungera med den befintliga infrastrukturen.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-123">The NPS extension is meant to work with your existing infrastructure.</span></span> <span data-ttu-id="1aa0b-124">Kontrollera att du har följande krav innan du börjar.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-124">Make sure you have the following prerequisites before you begin.</span></span>

### <a name="licenses"></a><span data-ttu-id="1aa0b-125">Licenser</span><span class="sxs-lookup"><span data-stu-id="1aa0b-125">Licenses</span></span>

<span data-ttu-id="1aa0b-126">NPS-tillägget för Azure MFA är tillgängligt för kunder med [licenser för Azure Multi-Factor Authentication](multi-factor-authentication.md) (ingår i Azure AD Premium, EMS eller en MFA-prenumeration).</span><span class="sxs-lookup"><span data-stu-id="1aa0b-126">The NPS Extension for Azure MFA is available to customers with [licenses for Azure Multi-Factor Authentication](multi-factor-authentication.md) (included with Azure AD Premium, EMS, or an MFA subscription).</span></span>

### <a name="software"></a><span data-ttu-id="1aa0b-127">Programvara</span><span class="sxs-lookup"><span data-stu-id="1aa0b-127">Software</span></span>

<span data-ttu-id="1aa0b-128">Windows Server 2008 R2 SP1 eller senare.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-128">Windows Server 2008 R2 SP1 or above.</span></span>

### <a name="libraries"></a><span data-ttu-id="1aa0b-129">Bibliotek</span><span class="sxs-lookup"><span data-stu-id="1aa0b-129">Libraries</span></span>

<span data-ttu-id="1aa0b-130">Dessa bibliotek installeras automatiskt med tillägget.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-130">These libraries are installed automatically with the extension.</span></span>
-   [<span data-ttu-id="1aa0b-131">Visual C++ Redistributable-paket för Visual Studio 2013 (X64)</span><span class="sxs-lookup"><span data-stu-id="1aa0b-131">Visual C++ Redistributable Packages for Visual Studio 2013 (X64)</span></span>](https://www.microsoft.com/download/details.aspx?id=40784)
-   [<span data-ttu-id="1aa0b-132">Microsoft Azure Active Directory-modulen för Windows PowerShell version 1.1.166.0</span><span class="sxs-lookup"><span data-stu-id="1aa0b-132">Microsoft Azure Active Directory Module for Windows PowerShell version 1.1.166.0</span></span>](https://connect.microsoft.com/site1164/Downloads/DownloadDetails.aspx?DownloadID=59185)

### <a name="azure-active-directory"></a><span data-ttu-id="1aa0b-133">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="1aa0b-133">Azure Active Directory</span></span>

<span data-ttu-id="1aa0b-134">Alla som använder tillägget NPS måste synkroniseras till Azure Active Directory med Azure AD Connect och måste registreras för MFA.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-134">Everyone using the NPS extension must be synced to Azure Active Directory using Azure AD Connect, and must be registered for MFA.</span></span>

<span data-ttu-id="1aa0b-135">När du installerar tillägget behöver du autentiseringsuppgifter för katalog-ID och administratör för Azure AD-klienten.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-135">When you install the extension, you need the directory ID and admin credentials for your Azure AD tenant.</span></span> <span data-ttu-id="1aa0b-136">Du kan hitta din katalog-ID i den [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="1aa0b-136">You can find your directory ID in the [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="1aa0b-137">Logga in som administratör, Välj den **Azure Active Directory** ikon på vänster och välj sedan **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-137">Sign in as an administrator, select the **Azure Active Directory** icon on the left, then select **Properties**.</span></span> <span data-ttu-id="1aa0b-138">Kopiera GUID i den **katalog-ID** och spara den.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-138">Copy the GUID in the **Directory ID** box and save it.</span></span> <span data-ttu-id="1aa0b-139">Du kan använda detta GUID som klient-ID när du installerar NPS-tillägget.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-139">You use this GUID as the tenant ID when you install the NPS extension.</span></span>

![Hitta din katalog-ID under Azure Active Directory-egenskaper](./media/multi-factor-authentication-nps-extension/find-directory-id.png)

## <a name="prepare-your-environment"></a><span data-ttu-id="1aa0b-141">Förbered din miljö</span><span class="sxs-lookup"><span data-stu-id="1aa0b-141">Prepare your environment</span></span>

<span data-ttu-id="1aa0b-142">Innan du installerar NPS-tillägget som du vill förbereda miljön för att hantera autentiseringstrafiken.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-142">Before you install the NPS extension, you want to prepare you environment to handle the authentication traffic.</span></span>

### <a name="enable-the-nps-role-on-a-domain-joined-server"></a><span data-ttu-id="1aa0b-143">Aktivera NPS-rollen på en domänansluten server</span><span class="sxs-lookup"><span data-stu-id="1aa0b-143">Enable the NPS role on a domain-joined server</span></span>

<span data-ttu-id="1aa0b-144">NPS-servern ansluter till Azure Active Directory och autentiseras av MFA-begäranden.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-144">The NPS server connects to Azure Active Directory and authenticates the MFA requests.</span></span> <span data-ttu-id="1aa0b-145">Välj en server för den här rollen.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-145">Choose one server for this role.</span></span> <span data-ttu-id="1aa0b-146">Vi rekommenderar att du väljer en server som inte hanterar förfrågningar från andra tjänster, eftersom tillägget NPS genererar fel för alla begäranden som inte är RADIUS.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-146">We recommend choosing a server that doesn't handle requests from other services, because the NPS extension throws errors for any requests that aren't RADIUS.</span></span>

1. <span data-ttu-id="1aa0b-147">Öppna på servern, den **guiden Lägg till roller och funktioner** Serverhanteraren Quickstart-menyn.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-147">On your server, open the **Add Roles and Features Wizard** from the Server Manager Quickstart menu.</span></span>
2. <span data-ttu-id="1aa0b-148">Välj **rollbaserad eller funktionsbaserad installation** för din installation.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-148">Choose **Role-based or feature-based installation** for your installation type.</span></span>
3. <span data-ttu-id="1aa0b-149">Välj den **nätverksprincip och åtkomsttjänster** -serverrollen.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-149">Select the **Network Policy and Access Services** server role.</span></span> <span data-ttu-id="1aa0b-150">Ett fönster kan visas för att meddela dig om nödvändiga funktioner för att köra den här rollen.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-150">A window may pop up to inform you of required features to run this role.</span></span>
4. <span data-ttu-id="1aa0b-151">Vill du fortsätta med guiden förrän bekräftelsesidan.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-151">Continue through the wizard until the Confirmation page.</span></span> <span data-ttu-id="1aa0b-152">Välj **installera**.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-152">Select **Install**.</span></span>

<span data-ttu-id="1aa0b-153">Nu när du har en server som är avsedd för NPS kan konfigurera du också den här servern för att hantera inkommande RADIUS-förfrågningar från VPN-lösning.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-153">Now that you have a server designated for NPS, you should also configure this server to handle incoming RADIUS requests from the VPN solution.</span></span>

### <a name="configure-your-vpn-solution-to-communicate-with-the-nps-server"></a><span data-ttu-id="1aa0b-154">Konfigurera din VPN-lösning för att kommunicera med NPS-servern</span><span class="sxs-lookup"><span data-stu-id="1aa0b-154">Configure your VPN solution to communicate with the NPS server</span></span>

<span data-ttu-id="1aa0b-155">Stegen för att konfigurera din princip för RADIUS-autentisering varierar beroende på vilken VPN-lösning som du använder.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-155">Depending on which VPN solution you use, the steps to configure your RADIUS authentication policy vary.</span></span> <span data-ttu-id="1aa0b-156">Konfigurera principen så att den pekar till RADIUS-NPS-servern.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-156">Configure this policy to point to your RADIUS NPS server.</span></span>

### <a name="sync-domain-users-to-the-cloud"></a><span data-ttu-id="1aa0b-157">Synkronisera användare till molnet</span><span class="sxs-lookup"><span data-stu-id="1aa0b-157">Sync domain users to the cloud</span></span>

<span data-ttu-id="1aa0b-158">Det här steget kan redan vara klar på din klient, men det är bra att kontrollera att Azure AD Connect har synkroniserat dina databaser nyligen.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-158">This step may already be complete on your tenant, but it's good to double-check that Azure AD Connect has synchronized your databases recently.</span></span>

1. <span data-ttu-id="1aa0b-159">Logga in på [Azure Portal](https://portal.azure.com) som administratör.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-159">Sign in to the [Azure portal](https://portal.azure.com) as an administrator.</span></span>
2. <span data-ttu-id="1aa0b-160">Välj **Azure Active Directory** > **Azure AD Connect**</span><span class="sxs-lookup"><span data-stu-id="1aa0b-160">Select **Azure Active Directory** > **Azure AD Connect**</span></span>
3. <span data-ttu-id="1aa0b-161">Kontrollera att din synkroniseringsstatus **aktiverad** och att dina senaste synkronisering har mindre än en timme sedan.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-161">Verify that your sync status is **Enabled** and that your last sync was less than an hour ago.</span></span>

<span data-ttu-id="1aa0b-162">Om du startar en ny runda av synkronisering oss instruktionerna i [Azure AD Connect-synkronisering: Schemaläggaren](../active-directory/connect/active-directory-aadconnectsync-feature-scheduler.md#start-the-scheduler).</span><span class="sxs-lookup"><span data-stu-id="1aa0b-162">If you need to kick off a new round of synchronization, us the instructions in [Azure AD Connect sync: Scheduler](../active-directory/connect/active-directory-aadconnectsync-feature-scheduler.md#start-the-scheduler).</span></span>

### <a name="determine-which-authentication-methods-your-users-can-use"></a><span data-ttu-id="1aa0b-163">Bestämma vilka autentiseringsmetoder som användarna kan använda</span><span class="sxs-lookup"><span data-stu-id="1aa0b-163">Determine which authentication methods your users can use</span></span>

<span data-ttu-id="1aa0b-164">Det finns två faktorer som påverkar vilka autentiseringsmetoder som är tillgängliga med en NPS-tillägg-distribution:</span><span class="sxs-lookup"><span data-stu-id="1aa0b-164">There are two factors that affect which authentication methods are available with an NPS extension deployment:</span></span>

1. <span data-ttu-id="1aa0b-165">Lösenord krypteringsalgoritmen som används mellan RADIUS-klienten (VPN, Netscaler server eller andra) och NPS-servrar.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-165">The password encryption algorithm used between the RADIUS client (VPN, Netscaler server, or other) and the NPS servers.</span></span>
   - <span data-ttu-id="1aa0b-166">**PAP** stöder alla autentiseringsmetoder i Azure MFA i molnet: telefonsamtal, enkelriktade SMS, mobilappavisering och Verifieringskod för mobila appar.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-166">**PAP** supports all the authentication methods of Azure MFA in the cloud: phone call, one-way text message, mobile app notification, and mobile app verification code.</span></span>
   - <span data-ttu-id="1aa0b-167">**CHAPv2** och **EAP** stöd för telefonsamtal och mobilappavisering.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-167">**CHAPV2** and **EAP** support phone call and mobile app notification.</span></span>
2. <span data-ttu-id="1aa0b-168">Inkommande metoder som klientprogram (VPN, Netscaler server eller andra) kan hantera.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-168">The input methods that the client application (VPN, Netscaler server, or other) can handle.</span></span> <span data-ttu-id="1aa0b-169">Till exempel har vissa innebär att tillåta användaren att ange verifieringskoden från text- eller mobilapp i VPN-klienten?</span><span class="sxs-lookup"><span data-stu-id="1aa0b-169">For example, does the VPN client have some means to allow the user to type in a verification code from a text or mobile app?</span></span>

<span data-ttu-id="1aa0b-170">När du distribuerar NPS-tillägget kan du använda dessa faktorer för att utvärdera vilka metoder som är tillgängliga för användarna.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-170">When you deploy the NPS extension, use these factors to evaluate which methods are available for your users.</span></span> <span data-ttu-id="1aa0b-171">Om RADIUS-klienten stöder PAP, men klienten UX saknar inmatningsfält för en Verifieringskod, sedan telefonsamtal och mobilappavisering är de två alternativ som stöds.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-171">If your RADIUS client supports PAP, but the client UX doesn't have input fields for a verification code, then phone call and mobile app notification are the two supported options.</span></span>

<span data-ttu-id="1aa0b-172">Du kan [inaktivera stöds inte autentiseringsmetoder](multi-factor-authentication-whats-next.md#selectable-verification-methods) i Azure.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-172">You can [disable unsupported authentication methods](multi-factor-authentication-whats-next.md#selectable-verification-methods) in Azure.</span></span>

### <a name="enable-users-for-mfa"></a><span data-ttu-id="1aa0b-173">Aktivera användare för MFA</span><span class="sxs-lookup"><span data-stu-id="1aa0b-173">Enable users for MFA</span></span>

<span data-ttu-id="1aa0b-174">Innan du distribuerar fullständig NPS-tillägget som du behöver aktivera MFA för användare som du vill utföra tvåstegsverifiering.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-174">Before you deploy the full NPS extension, you need to enable MFA for the users that you want to perform two-step verification.</span></span> <span data-ttu-id="1aa0b-175">Mer direkt om du vill testa tillägget när du distribuerar det, behöver du minst ett testkonto som är fullständigt registrerad för Multifaktorautentisering.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-175">More immediately, to test the extension as you deploy it, you need at least one test account that is fully registered for Multi-Factor Authentication.</span></span>

<span data-ttu-id="1aa0b-176">Följ dessa steg för att hämta ett testkonto igång:</span><span class="sxs-lookup"><span data-stu-id="1aa0b-176">Use these steps to get a test account started:</span></span>
1. <span data-ttu-id="1aa0b-177">Logga in på [https://aka.ms/mfasetup](https://aka.ms/mfasetup) med ett testkonto.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-177">Sign in to [https://aka.ms/mfasetup](https://aka.ms/mfasetup) with a test account.</span></span> 
2. <span data-ttu-id="1aa0b-178">Följ anvisningarna för att ställa in en verifieringsmetod.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-178">Follow the prompts to set up a verification method.</span></span>
3. <span data-ttu-id="1aa0b-179">Skapa en princip för villkorlig åtkomst eller [ändra användartillståndet](multi-factor-authentication-get-started-user-states.md) kräver tvåstegsverifiering för test-kontot.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-179">Either create a conditional access policy or [change the user state](multi-factor-authentication-get-started-user-states.md) to require two-step verification for the test account.</span></span> 

<span data-ttu-id="1aa0b-180">Användarna måste också att följa dessa steg för att registrera innan de kan autentisera med NPS-tillägget.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-180">Your users also need to follow these steps to enroll before they can authenticate with the NPS extension.</span></span>

## <a name="install-the-nps-extension"></a><span data-ttu-id="1aa0b-181">Installera NPS-tillägg</span><span class="sxs-lookup"><span data-stu-id="1aa0b-181">Install the NPS extension</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1aa0b-182">Installera NPS-tillägg på en annan server än åtkomstpunkt VPN.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-182">Install the NPS extension on a different server than the VPN access point.</span></span>

### <a name="download-and-install-the-nps-extension-for-azure-mfa"></a><span data-ttu-id="1aa0b-183">Hämta och installera NPS-tillägget för Azure MFA</span><span class="sxs-lookup"><span data-stu-id="1aa0b-183">Download and install the NPS extension for Azure MFA</span></span>

1.  <span data-ttu-id="1aa0b-184">[Hämta tillägget NPS](https://aka.ms/npsmfa) från Microsoft Download Center.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-184">[Download the NPS Extension](https://aka.ms/npsmfa) from the Microsoft Download Center.</span></span>
2.  <span data-ttu-id="1aa0b-185">Kopiera den binära filen till nätverksprincipservern som du vill konfigurera.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-185">Copy the binary to the Network Policy Server you want to configure.</span></span>
3.  <span data-ttu-id="1aa0b-186">Kör *setup.exe* och Följ installationsanvisningarna.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-186">Run *setup.exe* and follow the installation instructions.</span></span> <span data-ttu-id="1aa0b-187">Om det uppstår fel kontrollerar du att två bibliotek från avsnittet förutsättningar har installerats.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-187">If you encounter errors, double-check that the two libraries from the prerequisite section were successfully installed.</span></span>

### <a name="run-the-powershell-script"></a><span data-ttu-id="1aa0b-188">Kör PowerShell-skriptet</span><span class="sxs-lookup"><span data-stu-id="1aa0b-188">Run the PowerShell script</span></span>

<span data-ttu-id="1aa0b-189">Installationsprogrammet skapar en PowerShell-skript på den här platsen: `C:\Program Files\Microsoft\AzureMfa\Config` (där C:\ är installationsenheten).</span><span class="sxs-lookup"><span data-stu-id="1aa0b-189">The installer creates a PowerShell script in this location: `C:\Program Files\Microsoft\AzureMfa\Config` (where C:\ is your installation drive).</span></span> <span data-ttu-id="1aa0b-190">Det här PowerShell-skriptet utförs följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="1aa0b-190">This PowerShell script performs the following actions:</span></span>

-   <span data-ttu-id="1aa0b-191">Skapa ett självsignerat certifikat.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-191">Create a self-signed certificate.</span></span>
-   <span data-ttu-id="1aa0b-192">Koppla den offentliga nyckeln för certifikatet till tjänstens huvudnamn på Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-192">Associate the public key of the certificate to the service principal on Azure AD.</span></span>
-   <span data-ttu-id="1aa0b-193">Lagra certifikatet i lokala datorns certifikatarkiv.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-193">Store the cert in the local machine cert store.</span></span>
-   <span data-ttu-id="1aa0b-194">Bevilja åtkomst till certifikatets privata nyckel till användare i nätverket.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-194">Grant access to the certificate’s private key to Network User.</span></span>
-   <span data-ttu-id="1aa0b-195">Starta om NPS.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-195">Restart the NPS.</span></span>

<span data-ttu-id="1aa0b-196">Om du inte vill använda dina egna certifikat (i stället för de självsignerade certifikat som genereras av PowerShell-skriptet), Kör PowerShell-skript för att slutföra installationen.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-196">Unless you want to use your own certificates (instead of the self-signed certificates that the PowerShell script generates), run the PowerShell Script to complete the installation.</span></span> <span data-ttu-id="1aa0b-197">Om du installerar tillägget på flera servrar, ha var och en sina egna certifikat.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-197">If you install the extension on multiple servers, each one should have its own certificate.</span></span>

1. <span data-ttu-id="1aa0b-198">Kör Windows PowerShell som administratör.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-198">Run Windows PowerShell as an administrator.</span></span>
2. <span data-ttu-id="1aa0b-199">Ändra kataloger.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-199">Change directories.</span></span>

   `cd "C:\Program Files\Microsoft\AzureMfa\Config"`

3. <span data-ttu-id="1aa0b-200">Kör PowerShell-skript som skapas av installationsprogrammet.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-200">Run the PowerShell script created by the installer.</span></span>

   `.\AzureMfaNpsExtnConfigSetup.ps1`

4. <span data-ttu-id="1aa0b-201">PowerShell frågar efter klient-ID.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-201">PowerShell prompts for your tenant ID.</span></span> <span data-ttu-id="1aa0b-202">Använd katalog-ID-GUID som du kopierade från Azure-portalen i avsnittet förutsättningar.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-202">Use the Directory ID GUID that you copied from the Azure portal in the prerequisites section.</span></span>
5. <span data-ttu-id="1aa0b-203">Logga in på Azure AD som administratör.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-203">Sign in to Azure AD as an administrator.</span></span>
6. <span data-ttu-id="1aa0b-204">PowerShell visar ett meddelande när skriptet har slutförts.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-204">PowerShell shows a success message when the script is finished.</span></span>  

<span data-ttu-id="1aa0b-205">Upprepa dessa steg på ytterligare NPS-servrar som du vill konfigurera för belastningsutjämning.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-205">Repeat these steps on any additional NPS servers that you want to set up for load balancing.</span></span>

>[!NOTE]
><span data-ttu-id="1aa0b-206">Om du använder egna certifikat i stället för att certifikat med PowerShell-skript, kontrollera att justera NPS namngivningskonventionen.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-206">If you use your own certificates instead of generating certificates with the PowerShell script, make sure that they align to the NPS naming convention.</span></span> <span data-ttu-id="1aa0b-207">Ämnesnamn måste vara **CN =\<TenantID\>, OU = Microsoft NPS tillägget**.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-207">The subject name must be **CN=\<TenantID\>,OU=Microsoft NPS Extension**.</span></span> 

## <a name="configure-your-nps-extension"></a><span data-ttu-id="1aa0b-208">Konfigurera NPS-tillägget</span><span class="sxs-lookup"><span data-stu-id="1aa0b-208">Configure your NPS extension</span></span>

<span data-ttu-id="1aa0b-209">Det här avsnittet innehåller överväganden vid utformning och förslag för lyckad distribution för NPS-tillägget.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-209">This section includes design considerations and suggestions for successful NPS extension deployments.</span></span>

### <a name="configuration-limitations"></a><span data-ttu-id="1aa0b-210">Begränsningar för konfiguration</span><span class="sxs-lookup"><span data-stu-id="1aa0b-210">Configuration limitations</span></span>

- <span data-ttu-id="1aa0b-211">NPS-tillägget för Azure MFA innehåller inte verktyg för att migrera användare och inställningar från MFA-Server till molnet.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-211">The NPS extension for Azure MFA does not include tools to migrate users and settings from MFA Server to the cloud.</span></span> <span data-ttu-id="1aa0b-212">Därför föreslår vi använder tillägget för nya distributioner, i stället för befintlig distribution.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-212">For this reason, we suggest using the extension for new deployments, rather than existing deployment.</span></span> <span data-ttu-id="1aa0b-213">Om du använder tillägget på en befintlig distribution kan måste användarna utföra bevis upp igen för att fylla i information om MFA i molnet.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-213">If you use the extension on an existing deployment, your users have to perform proof-up again to populate their MFA details in the cloud.</span></span>  
- <span data-ttu-id="1aa0b-214">NPS-tillägget använder UPN från lokala Active directory för att identifiera användaren vid Azure MFA för att utföra den sekundära auktor Tillägget kan konfigureras för att använda en annan identifierare som alternativt inloggnings-ID eller anpassade Active Directory-fältet än UPN.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-214">The NPS extension uses the UPN from the on-premises Active directory to identify the user on Azure MFA for performing the Secondary Auth. The extension can be configured to use a different identifier like alternate login ID or custom Active Directory field other than UPN.</span></span> <span data-ttu-id="1aa0b-215">Se [avancerade konfigurationsalternativ för NPS-tillägg för multi-Factor Authentication](multi-factor-authentication-advanced-vpn-configurations.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-215">See [Advanced configuration options for the NPS extension for Multi-Factor Authentication](multi-factor-authentication-advanced-vpn-configurations.md) for more information.</span></span>
- <span data-ttu-id="1aa0b-216">Inte alla krypteringsprotokoll stöder alla verifieringsmetoderna.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-216">Not all encryption protocols support all verification methods.</span></span>
   - <span data-ttu-id="1aa0b-217">**PAP** stöder telefonsamtal, enkelriktade SMS, mobilappavisering och Verifieringskod för mobila appar</span><span class="sxs-lookup"><span data-stu-id="1aa0b-217">**PAP** supports phone call, one-way text message, mobile app notification, and mobile app verification code</span></span>
   - <span data-ttu-id="1aa0b-218">**CHAPv2** och **EAP** stöd för telefonsamtal och mobilappavisering</span><span class="sxs-lookup"><span data-stu-id="1aa0b-218">**CHAPV2** and **EAP** support phone call and mobile app notification</span></span>

### <a name="control-radius-clients-that-require-mfa"></a><span data-ttu-id="1aa0b-219">Kontrollen RADIUS-klienter som kräver MFA</span><span class="sxs-lookup"><span data-stu-id="1aa0b-219">Control RADIUS clients that require MFA</span></span>

<span data-ttu-id="1aa0b-220">När du aktiverar MFA för en RADIUS-klient med hjälp av NPS-tillägget, måste alla autentiseringar för den här klienten gör en MFA.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-220">Once you enable MFA for a RADIUS client using the NPS Extension, all authentications for this client are required to perform MFA.</span></span> <span data-ttu-id="1aa0b-221">Om du vill aktivera MFA för vissa RADIUS-klienter, men inte andra kan du konfigurera två NPS-servrar och installera tillägget på endast en av dem.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-221">If you want to enable MFA for some RADIUS clients but not others, you can configure two NPS servers and install the extension on only one of them.</span></span> <span data-ttu-id="1aa0b-222">Konfigurera RADIUS-klienter som du vill kräva MFA för att skicka begäranden till NPS-server som konfigurerats med tillägget och andra RADIUS-klienter till NPS-servern inte har konfigurerats med tillägget.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-222">Configure RADIUS clients that you want to require MFA to send requests to the NPS server configured with the extension, and other RADIUS clients to the NPS server not configured with the extension.</span></span>

### <a name="prepare-for-users-that-arent-enrolled-for-mfa"></a><span data-ttu-id="1aa0b-223">Förbereda för användare som inte är registrerade för MFA</span><span class="sxs-lookup"><span data-stu-id="1aa0b-223">Prepare for users that aren't enrolled for MFA</span></span>

<span data-ttu-id="1aa0b-224">Om du har användare som inte är registrerade för MFA, kan du bestämma vad som händer när de försöker autentisera.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-224">If you have users that aren't enrolled for MFA, you can determine what happens when they try to authenticate.</span></span> <span data-ttu-id="1aa0b-225">Använda registerinställningen *REQUIRE_USER_MATCH* i registersökvägen *HKLM\Software\Microsoft\AzureMFA* att styra hur funktionen.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-225">Use the registry setting *REQUIRE_USER_MATCH* in the registry path *HKLM\Software\Microsoft\AzureMFA* to control the feature behavior.</span></span> <span data-ttu-id="1aa0b-226">Den här inställningen har ett enda konfigurationsalternativ:</span><span class="sxs-lookup"><span data-stu-id="1aa0b-226">This setting has a single configuration option:</span></span>

| <span data-ttu-id="1aa0b-227">Nyckel</span><span class="sxs-lookup"><span data-stu-id="1aa0b-227">Key</span></span> | <span data-ttu-id="1aa0b-228">Värde</span><span class="sxs-lookup"><span data-stu-id="1aa0b-228">Value</span></span> | <span data-ttu-id="1aa0b-229">Standard</span><span class="sxs-lookup"><span data-stu-id="1aa0b-229">Default</span></span> |
| --- | ----- | ------- |
| <span data-ttu-id="1aa0b-230">REQUIRE_USER_MATCH</span><span class="sxs-lookup"><span data-stu-id="1aa0b-230">REQUIRE_USER_MATCH</span></span> | <span data-ttu-id="1aa0b-231">SANT/FALSKT</span><span class="sxs-lookup"><span data-stu-id="1aa0b-231">TRUE/FALSE</span></span> | <span data-ttu-id="1aa0b-232">Inte har angetts (motsvarar TRUE)</span><span class="sxs-lookup"><span data-stu-id="1aa0b-232">Not set (equivalent to TRUE)</span></span> |

<span data-ttu-id="1aa0b-233">Syftet med den här inställningen är att avgöra vad du gör om en användare inte har registrerats för MFA.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-233">The purpose of this setting is to determine what to do when a user is not enrolled for MFA.</span></span> <span data-ttu-id="1aa0b-234">När nyckeln finns inte, har inte angetts eller är inställd på TRUE, och användaren har inte registrerats sedan tillägget misslyckas MFA-kontrollen.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-234">When the key does not exist, is not set, or is set to TRUE, and the user is not enrolled, then the extension fails the MFA challenge.</span></span> <span data-ttu-id="1aa0b-235">När nyckeln är inställd på FALSE och användaren inte är registrerad, fortsätter autentiseringen utan att utföra MFA.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-235">When the key is set to FALSE and the user is not enrolled, authentication proceeds without performing MFA.</span></span>

<span data-ttu-id="1aa0b-236">Du kan välja att skapa den här nyckeln och ange den till FALSE, medan användarna är onboarding och kan inte registreras för Azure MFA ännu.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-236">You can choose to create this key and set it to FALSE while your users are onboarding, and may not all be enrolled for Azure MFA yet.</span></span> <span data-ttu-id="1aa0b-237">Eftersom inställningen nyckeln tillåter att användare som inte är registrerade för MFA för att logga in, bör du dock ta bort den här nyckeln innan du fortsätter till produktionen.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-237">However, since setting the key permits users that aren't enrolled for MFA to sign in, you should remove this key before going to production.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="1aa0b-238">Felsökning</span><span class="sxs-lookup"><span data-stu-id="1aa0b-238">Troubleshooting</span></span>

### <a name="how-do-i-verify-that-the-client-cert-is-installed-as-expected"></a><span data-ttu-id="1aa0b-239">Hur kan jag bekräfta att klienten certifikat installeras som förväntat?</span><span class="sxs-lookup"><span data-stu-id="1aa0b-239">How do I verify that the client cert is installed as expected?</span></span>

<span data-ttu-id="1aa0b-240">Leta efter ett självsignerat certifikat som skapas av installationsprogrammet i certifikatarkivet och kontrollera att den privata nyckeln har behörigheterna för användaren **NÄTVERKSTJÄNSTEN**.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-240">Look for the self-signed certificate created by the installer in the cert store, and check that the private key has permissions granted to user **NETWORK SERVICE**.</span></span> <span data-ttu-id="1aa0b-241">Certifikatet har ett ämnesnamn **CN \<tenantid\>, OU = Microsoft NPS-tillägg**</span><span class="sxs-lookup"><span data-stu-id="1aa0b-241">The cert has a subject name of **CN \<tenantid\>, OU = Microsoft NPS Extension**</span></span>

-------------------------------------------------------------

### <a name="how-can-i-verify-that-my-client-cert-is-associated-to-my-tenant-in-azure-active-directory"></a><span data-ttu-id="1aa0b-242">Hur kan jag bekräfta att min klient cert är kopplad till min klient i Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="1aa0b-242">How can I verify that my client cert is associated to my tenant in Azure Active Directory?</span></span>

<span data-ttu-id="1aa0b-243">Öppna PowerShell-Kommandotolken och kör följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="1aa0b-243">Open PowerShell command prompt and run the following commands:</span></span>

```
import-module MSOnline
Connect-MsolService
Get-MsolServicePrincipalCredential -AppPrincipalId "981f26a1-7f43-403b-a875-f8b09b8cd720" -ReturnKeyValues 1 
```

<span data-ttu-id="1aa0b-244">Dessa kommandon ut alla certifikat som kopplar din klient till din instans av NPS-tillägget i PowerShell-sessionen.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-244">These commands print all the certificates associating your tenant with your instance of the NPS extension in your PowerShell session.</span></span> <span data-ttu-id="1aa0b-245">Leta efter certifikatet genom att exportera certifikatet klienten som en ”Base64-kodat X.509(.cer)” fil utan den privata nyckeln och jämför den med listan från PowerShell.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-245">Look for your certificate by exporting your client cert as a "Base-64 encoded X.509(.cer)" file without the private key, and compare it with the list from PowerShell.</span></span>

<span data-ttu-id="1aa0b-246">Giltigt-från- och -tills tidsstämplar som är läsbart format, som kan användas för att filtrera ut uppenbara misfits om kommandot returnerar mer än ett certifikat.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-246">Valid-From and Valid-Until timestamps, which are in human-readable form, can be used to filter out obvious misfits if the command returns more than one cert.</span></span>

-------------------------------------------------------------

### <a name="why-are-my-requests-failing-with-adal-token-error"></a><span data-ttu-id="1aa0b-247">Varför är min begäranden inte med ADAL fel?</span><span class="sxs-lookup"><span data-stu-id="1aa0b-247">Why are my requests failing with ADAL token error?</span></span>

<span data-ttu-id="1aa0b-248">Det här felet kan bero på ett av flera skäl.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-248">This error could be due to one of several reasons.</span></span> <span data-ttu-id="1aa0b-249">Följ dessa steg för att felsöka:</span><span class="sxs-lookup"><span data-stu-id="1aa0b-249">Use these steps to help troubleshoot:</span></span>

1. <span data-ttu-id="1aa0b-250">Starta om NPS-servern.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-250">Restart your NPS server.</span></span>
2. <span data-ttu-id="1aa0b-251">Kontrollera att certifikat som klienten är installerad som förväntat.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-251">Verify that that client cert is installed as expected.</span></span>
3. <span data-ttu-id="1aa0b-252">Kontrollera att certifikatet är associerad med din klient på Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-252">Verify that the certificate is associated with your tenant on Azure AD.</span></span>
4. <span data-ttu-id="1aa0b-253">Kontrollera att https://login.microsoftonline.com/ är tillgänglig från den server som kör tillägget.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-253">Verify that https://login.microsoftonline.com/ is accessible from the server running the extension.</span></span>

-------------------------------------------------------------

### <a name="why-does-authentication-fail-with-an-error-in-http-logs-stating-that-the-user-is-not-found"></a><span data-ttu-id="1aa0b-254">Varför misslyckas autentiseringen med ett fel i HTTP-loggar som anger att användaren inte kan hittas?</span><span class="sxs-lookup"><span data-stu-id="1aa0b-254">Why does authentication fail with an error in HTTP logs stating that the user is not found?</span></span>

<span data-ttu-id="1aa0b-255">Kontrollera att AD Connect är igång och att användaren finns i både Windows Active Directory och Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-255">Verify that AD Connect is running, and that the user is present in both Windows Active Directory and Azure Active Directory.</span></span>

------------------------------------------------------------

### <a name="why-do-i-see-http-connect-errors-in-logs-with-all-my-authentications-failing"></a><span data-ttu-id="1aa0b-256">Varför ser HTTP connect-fel i loggarna med min autentiseringar misslyckas?</span><span class="sxs-lookup"><span data-stu-id="1aa0b-256">Why do I see HTTP connect errors in logs with all my authentications failing?</span></span>

<span data-ttu-id="1aa0b-257">Kontrollera att https://adnotifications.windowsazure.com kan nås från servern som kör NPS-tillägget.</span><span class="sxs-lookup"><span data-stu-id="1aa0b-257">Verify that https://adnotifications.windowsazure.com is reachable from the server running the NPS extension.</span></span>


## <a name="next-steps"></a><span data-ttu-id="1aa0b-258">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1aa0b-258">Next steps</span></span>

- <span data-ttu-id="1aa0b-259">Konfigurera alternativa ID för inloggning eller ställa in en undantagslista för IP-adresser som inte får utföra tvåstegsverifiering i [avancerade konfigurationsalternativ för NPS-tillägg för Multifaktorautentisering](nps-extension-advanced-configuration.md)</span><span class="sxs-lookup"><span data-stu-id="1aa0b-259">Configure alternate IDs for login, or set up an exception list for IPs that shouldn't perform two-step verification in [Advanced configuration options for the NPS extension for Multi-Factor Authentication](nps-extension-advanced-configuration.md)</span></span>

- [<span data-ttu-id="1aa0b-260">Åtgärda felmeddelanden från NPS-tillägget för Azure Multi-Factor Authentication</span><span class="sxs-lookup"><span data-stu-id="1aa0b-260">Resolve error messages from the NPS extension for Azure Multi-Factor Authentication</span></span>](multi-factor-authentication-nps-errors.md)
