---
title: "aaaUse NPS-servrar tooprovide Azure MFA möjligheterna | Microsoft Docs"
description: "hello NPS-tillägget för Azure Multi-Factor Authentication är en enkel lösning tooadd molnbaserade tvåstegsverifiering vericiation funktioner tooyour befintliga autentisering infrastruktur."
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
ms.openlocfilehash: e9fc495b06873d45f06233f1aa618db9d94f8bd9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-your-existing-nps-infrastructure-with-azure-multi-factor-authentication"></a><span data-ttu-id="c5acf-103">Integrera din befintliga infrastruktur för NPS med Azure Multi-Factor Authentication</span><span class="sxs-lookup"><span data-stu-id="c5acf-103">Integrate your existing NPS infrastructure with Azure Multi-Factor Authentication</span></span>

<span data-ttu-id="c5acf-104">hello Server (NPS)-tillägg för Azure MFA lägger till molnbaserade MFA funktioner tooyour autentisering infrastruktur med hjälp av din befintliga servrar.</span><span class="sxs-lookup"><span data-stu-id="c5acf-104">hello Network Policy Server (NPS) extension for Azure MFA adds cloud-based MFA capabilities tooyour authentication infrastructure using your existing servers.</span></span> <span data-ttu-id="c5acf-105">Med hello NPS-tillägget, kan du lägga till telefonsamtal, SMS eller telefon app verifiering tooyour befintliga autentiseringsflödet utan tooinstall, konfigurerar och underhåller nya servrar.</span><span class="sxs-lookup"><span data-stu-id="c5acf-105">With hello NPS extension, you can add phone call, text message, or phone app verification tooyour existing authentication flow without having tooinstall, configure, and maintain new servers.</span></span> 

<span data-ttu-id="c5acf-106">Det här tillägget har skapats för organisationer som vill tooprotect VPN-anslutningar utan att distribuera hello Azure MFA-Server.</span><span class="sxs-lookup"><span data-stu-id="c5acf-106">This extension was created for organizations that want tooprotect VPN connections without deploying hello Azure MFA Server.</span></span> <span data-ttu-id="c5acf-107">hello NPS-tillägg som fungerar som ett kort mellan RADIUS och molnbaserad Azure MFA tooprovide tvåfaktorsautentisering för federerade eller synkroniserade användare.</span><span class="sxs-lookup"><span data-stu-id="c5acf-107">hello NPS extension acts as an adapter between RADIUS and cloud-based Azure MFA tooprovide a second factor of authentication for federated or synced users.</span></span>

<span data-ttu-id="c5acf-108">När du använder hello NPS-tillägg för Azure MFA innehåller hello autentiseringsflödet hello följande komponenter:</span><span class="sxs-lookup"><span data-stu-id="c5acf-108">When using hello NPS extension for Azure MFA, hello authentication flow includes hello following components:</span></span> 

1. <span data-ttu-id="c5acf-109">**NAS-VPN-Server** tar emot förfrågningar från VPN-klienter och konverterar dem till RADIUS-begäranden tooNPS servrar.</span><span class="sxs-lookup"><span data-stu-id="c5acf-109">**NAS/VPN Server** receives requests from VPN clients and converts them into RADIUS requests tooNPS servers.</span></span> 
2. <span data-ttu-id="c5acf-110">**NPS-servern** ansluter tooActive Directory tooperform hello primär autentisering för hello RADIUS-begäranden och vid lyckades, skickar hello begäran tooany installerat tillägg.</span><span class="sxs-lookup"><span data-stu-id="c5acf-110">**NPS Server** connects tooActive Directory tooperform hello primary authentication for hello RADIUS requests and, upon success, passes hello request tooany installed extensions.</span></span>  
3. <span data-ttu-id="c5acf-111">**NPS-tillägget** utlöser begäran tooAzure MFA för hello sekundära autentiseringen.</span><span class="sxs-lookup"><span data-stu-id="c5acf-111">**NPS Extension** triggers a request tooAzure MFA for hello secondary authentication.</span></span> <span data-ttu-id="c5acf-112">När hello tillägget tar emot hello svar och hello MFA-kontrollen lyckas, Slutför hello autentiseringsbegäran genom att ge säkerhetstoken som innehåller ett MFA-anspråk som utfärdats av Azure STS hello NPS-servern.</span><span class="sxs-lookup"><span data-stu-id="c5acf-112">Once hello extension receives hello response, and if hello MFA challenge succeeds, it completes hello authentication request by providing hello NPS server with security tokens that include an MFA claim, issued by Azure STS.</span></span>  
4. <span data-ttu-id="c5acf-113">**Azure MFA** kommunicerar med Azure Active Directory tooretrieve hello användarens information och utför hello sekundära autentiseringen med hjälp av en verifiering metod konfigurerats toohello användare.</span><span class="sxs-lookup"><span data-stu-id="c5acf-113">**Azure MFA** communicates with Azure Active Directory tooretrieve hello user’s details and performs hello secondary authentication using a verification method configured toohello user.</span></span>

<span data-ttu-id="c5acf-114">hello följande diagram illustrerar det här övergripande autentiseringsflödet för begäran:</span><span class="sxs-lookup"><span data-stu-id="c5acf-114">hello following diagram illustrates this high-level authentication request flow:</span></span> 

![Flödesdiagram för autentisering](./media/multi-factor-authentication-nps-extension/auth-flow.png)

## <a name="plan-your-deployment"></a><span data-ttu-id="c5acf-116">Planera distributionen</span><span class="sxs-lookup"><span data-stu-id="c5acf-116">Plan your deployment</span></span>

<span data-ttu-id="c5acf-117">hello NPS tillägget hanterar automatiskt redundans, så du behöver en särskild konfiguration.</span><span class="sxs-lookup"><span data-stu-id="c5acf-117">hello NPS extension automatically handles redundancy, so you don't need a special configuration.</span></span>

<span data-ttu-id="c5acf-118">Du kan skapa så många Azure MFA-aktiverade NPS-servrar som du behöver.</span><span class="sxs-lookup"><span data-stu-id="c5acf-118">You can create as many Azure MFA-enabled NPS servers as you need.</span></span> <span data-ttu-id="c5acf-119">Om du installerar flera servrar, bör du använda ett certifikat för skillnaden för var och en av dem.</span><span class="sxs-lookup"><span data-stu-id="c5acf-119">If you do install multiple servers, you should use a difference client certificate for each one of them.</span></span> <span data-ttu-id="c5acf-120">När du skapar ett certifikat för varje server innebär att du kan uppdatera varje cert individuellt och oroa dig inte om driftstopp på alla servrar.</span><span class="sxs-lookup"><span data-stu-id="c5acf-120">Creating a cert for each server means that you can update each cert individually, and not worry about downtime across all your servers.</span></span>

<span data-ttu-id="c5acf-121">VPN-servrar vidarebefordra autentiseringsbegäranden, så de måste toobe medveten om hello nya Azure MFA-aktiverade NPS-servrar.</span><span class="sxs-lookup"><span data-stu-id="c5acf-121">VPN servers route authentication requests, so they need toobe aware of hello new Azure MFA-enabled NPS servers.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c5acf-122">Krav</span><span class="sxs-lookup"><span data-stu-id="c5acf-122">Prerequisites</span></span>

<span data-ttu-id="c5acf-123">hello NPS-tillägget är avsedd toowork med den befintliga infrastrukturen.</span><span class="sxs-lookup"><span data-stu-id="c5acf-123">hello NPS extension is meant toowork with your existing infrastructure.</span></span> <span data-ttu-id="c5acf-124">Kontrollera att du har hello följande krav innan du börjar.</span><span class="sxs-lookup"><span data-stu-id="c5acf-124">Make sure you have hello following prerequisites before you begin.</span></span>

### <a name="licenses"></a><span data-ttu-id="c5acf-125">Licenser</span><span class="sxs-lookup"><span data-stu-id="c5acf-125">Licenses</span></span>

<span data-ttu-id="c5acf-126">hello NPS-tillägg för Azure MFA är tillgängliga toocustomers med [licenser för Azure Multi-Factor Authentication](multi-factor-authentication.md) (ingår i Azure AD Premium, EMS eller en MFA-prenumeration).</span><span class="sxs-lookup"><span data-stu-id="c5acf-126">hello NPS Extension for Azure MFA is available toocustomers with [licenses for Azure Multi-Factor Authentication](multi-factor-authentication.md) (included with Azure AD Premium, EMS, or an MFA subscription).</span></span>

### <a name="software"></a><span data-ttu-id="c5acf-127">Programvara</span><span class="sxs-lookup"><span data-stu-id="c5acf-127">Software</span></span>

<span data-ttu-id="c5acf-128">Windows Server 2008 R2 SP1 eller senare.</span><span class="sxs-lookup"><span data-stu-id="c5acf-128">Windows Server 2008 R2 SP1 or above.</span></span>

### <a name="libraries"></a><span data-ttu-id="c5acf-129">Bibliotek</span><span class="sxs-lookup"><span data-stu-id="c5acf-129">Libraries</span></span>

<span data-ttu-id="c5acf-130">Dessa bibliotek installeras automatiskt med hello tillägg.</span><span class="sxs-lookup"><span data-stu-id="c5acf-130">These libraries are installed automatically with hello extension.</span></span>
-   [<span data-ttu-id="c5acf-131">Visual C++ Redistributable-paket för Visual Studio 2013 (X64)</span><span class="sxs-lookup"><span data-stu-id="c5acf-131">Visual C++ Redistributable Packages for Visual Studio 2013 (X64)</span></span>](https://www.microsoft.com/download/details.aspx?id=40784)
-   [<span data-ttu-id="c5acf-132">Microsoft Azure Active Directory-modulen för Windows PowerShell version 1.1.166.0</span><span class="sxs-lookup"><span data-stu-id="c5acf-132">Microsoft Azure Active Directory Module for Windows PowerShell version 1.1.166.0</span></span>](https://connect.microsoft.com/site1164/Downloads/DownloadDetails.aspx?DownloadID=59185)

### <a name="azure-active-directory"></a><span data-ttu-id="c5acf-133">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="c5acf-133">Azure Active Directory</span></span>

<span data-ttu-id="c5acf-134">Alla som använder hello NPS tillägget måste vara synkroniserade tooAzure Active Directory med Azure AD Connect och måste vara registrerad för Multifaktorautentisering.</span><span class="sxs-lookup"><span data-stu-id="c5acf-134">Everyone using hello NPS extension must be synced tooAzure Active Directory using Azure AD Connect, and must be registered for MFA.</span></span>

<span data-ttu-id="c5acf-135">När du installerar tillägget för hello behöver du hello-directory-ID och admin-autentiseringsuppgifter för Azure AD-klienten.</span><span class="sxs-lookup"><span data-stu-id="c5acf-135">When you install hello extension, you need hello directory ID and admin credentials for your Azure AD tenant.</span></span> <span data-ttu-id="c5acf-136">Du hittar din katalog-ID i hello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="c5acf-136">You can find your directory ID in hello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="c5acf-137">Logga in som administratör, Välj hello **Azure Active Directory** ikonen hello vänster, välj sedan **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="c5acf-137">Sign in as an administrator, select hello **Azure Active Directory** icon on hello left, then select **Properties**.</span></span> <span data-ttu-id="c5acf-138">Kopiera hello GUID i hello **katalog-ID** och spara den.</span><span class="sxs-lookup"><span data-stu-id="c5acf-138">Copy hello GUID in hello **Directory ID** box and save it.</span></span> <span data-ttu-id="c5acf-139">Använd detta GUID som hello klient-ID när du installerar hello NPS-tillägget.</span><span class="sxs-lookup"><span data-stu-id="c5acf-139">You use this GUID as hello tenant ID when you install hello NPS extension.</span></span>

![Hitta din katalog-ID under Azure Active Directory-egenskaper](./media/multi-factor-authentication-nps-extension/find-directory-id.png)

## <a name="prepare-your-environment"></a><span data-ttu-id="c5acf-141">Förbered din miljö</span><span class="sxs-lookup"><span data-stu-id="c5acf-141">Prepare your environment</span></span>

<span data-ttu-id="c5acf-142">Innan du installerar hello NPS tillägg du vill tooprepare du miljön toohandle hello autentiseringstrafik.</span><span class="sxs-lookup"><span data-stu-id="c5acf-142">Before you install hello NPS extension, you want tooprepare you environment toohandle hello authentication traffic.</span></span>

### <a name="enable-hello-nps-role-on-a-domain-joined-server"></a><span data-ttu-id="c5acf-143">Aktivera hello NPS-rollen på en domänansluten server</span><span class="sxs-lookup"><span data-stu-id="c5acf-143">Enable hello NPS role on a domain-joined server</span></span>

<span data-ttu-id="c5acf-144">hello NPS-servern ansluter tooAzure Active Directory och autentiserar hello MFA begäranden.</span><span class="sxs-lookup"><span data-stu-id="c5acf-144">hello NPS server connects tooAzure Active Directory and authenticates hello MFA requests.</span></span> <span data-ttu-id="c5acf-145">Välj en server för den här rollen.</span><span class="sxs-lookup"><span data-stu-id="c5acf-145">Choose one server for this role.</span></span> <span data-ttu-id="c5acf-146">Vi rekommenderar att du väljer en server som inte hanterar förfrågningar från andra tjänster, eftersom hello NPS tillägget genererar fel för alla begäranden som inte är RADIUS.</span><span class="sxs-lookup"><span data-stu-id="c5acf-146">We recommend choosing a server that doesn't handle requests from other services, because hello NPS extension throws errors for any requests that aren't RADIUS.</span></span>

1. <span data-ttu-id="c5acf-147">Öppna på servern, hello **guiden Lägg till roller och funktioner** från hello Serverhanteraren Quickstart-menyn.</span><span class="sxs-lookup"><span data-stu-id="c5acf-147">On your server, open hello **Add Roles and Features Wizard** from hello Server Manager Quickstart menu.</span></span>
2. <span data-ttu-id="c5acf-148">Välj **rollbaserad eller funktionsbaserad installation** för din installation.</span><span class="sxs-lookup"><span data-stu-id="c5acf-148">Choose **Role-based or feature-based installation** for your installation type.</span></span>
3. <span data-ttu-id="c5acf-149">Välj hello **nätverksprincip och åtkomsttjänster** -serverrollen.</span><span class="sxs-lookup"><span data-stu-id="c5acf-149">Select hello **Network Policy and Access Services** server role.</span></span> <span data-ttu-id="c5acf-150">Ett fönster kan visas tooinform du toorun nödvändiga funktioner för den här rollen.</span><span class="sxs-lookup"><span data-stu-id="c5acf-150">A window may pop up tooinform you of required features toorun this role.</span></span>
4. <span data-ttu-id="c5acf-151">Fortsätta hello guiden förrän hello bekräftelsesidan.</span><span class="sxs-lookup"><span data-stu-id="c5acf-151">Continue through hello wizard until hello Confirmation page.</span></span> <span data-ttu-id="c5acf-152">Välj **installera**.</span><span class="sxs-lookup"><span data-stu-id="c5acf-152">Select **Install**.</span></span>

<span data-ttu-id="c5acf-153">Nu när du har en server som är avsedd för NPS bör du också konfigurera den här servern toohandle inkommande RADIUS-begäranden från hello VPN-lösning.</span><span class="sxs-lookup"><span data-stu-id="c5acf-153">Now that you have a server designated for NPS, you should also configure this server toohandle incoming RADIUS requests from hello VPN solution.</span></span>

### <a name="configure-your-vpn-solution-toocommunicate-with-hello-nps-server"></a><span data-ttu-id="c5acf-154">Konfigurera din VPN-lösningen toocommunicate med hello NPS-server</span><span class="sxs-lookup"><span data-stu-id="c5acf-154">Configure your VPN solution toocommunicate with hello NPS server</span></span>

<span data-ttu-id="c5acf-155">Hej steg tooconfigure din princip för RADIUS-autentisering varierar beroende på vilken VPN-lösning som du använder.</span><span class="sxs-lookup"><span data-stu-id="c5acf-155">Depending on which VPN solution you use, hello steps tooconfigure your RADIUS authentication policy vary.</span></span> <span data-ttu-id="c5acf-156">Konfigurera den här principen toopoint tooyour RADIUS NPS-servern.</span><span class="sxs-lookup"><span data-stu-id="c5acf-156">Configure this policy toopoint tooyour RADIUS NPS server.</span></span>

### <a name="sync-domain-users-toohello-cloud"></a><span data-ttu-id="c5acf-157">Synkronisera domänen användare toohello moln</span><span class="sxs-lookup"><span data-stu-id="c5acf-157">Sync domain users toohello cloud</span></span>

<span data-ttu-id="c5acf-158">Det här steget kan redan vara klar på din klient, men det är bra toodouble Kontrollera att Azure AD Connect har synkroniserat dina databaser nyligen.</span><span class="sxs-lookup"><span data-stu-id="c5acf-158">This step may already be complete on your tenant, but it's good toodouble-check that Azure AD Connect has synchronized your databases recently.</span></span>

1. <span data-ttu-id="c5acf-159">Logga in toohello [Azure-portalen](https://portal.azure.com) som administratör.</span><span class="sxs-lookup"><span data-stu-id="c5acf-159">Sign in toohello [Azure portal](https://portal.azure.com) as an administrator.</span></span>
2. <span data-ttu-id="c5acf-160">Välj **Azure Active Directory** > **Azure AD Connect**</span><span class="sxs-lookup"><span data-stu-id="c5acf-160">Select **Azure Active Directory** > **Azure AD Connect**</span></span>
3. <span data-ttu-id="c5acf-161">Kontrollera att din synkroniseringsstatus **aktiverad** och att dina senaste synkronisering har mindre än en timme sedan.</span><span class="sxs-lookup"><span data-stu-id="c5acf-161">Verify that your sync status is **Enabled** and that your last sync was less than an hour ago.</span></span>

<span data-ttu-id="c5acf-162">Om du behöver tookick en ny runda för synkronisering av oss hello instruktionerna i [Azure AD Connect-synkronisering: Schemaläggaren](../active-directory/connect/active-directory-aadconnectsync-feature-scheduler.md#start-the-scheduler).</span><span class="sxs-lookup"><span data-stu-id="c5acf-162">If you need tookick off a new round of synchronization, us hello instructions in [Azure AD Connect sync: Scheduler](../active-directory/connect/active-directory-aadconnectsync-feature-scheduler.md#start-the-scheduler).</span></span>

### <a name="determine-which-authentication-methods-your-users-can-use"></a><span data-ttu-id="c5acf-163">Bestämma vilka autentiseringsmetoder som användarna kan använda</span><span class="sxs-lookup"><span data-stu-id="c5acf-163">Determine which authentication methods your users can use</span></span>

<span data-ttu-id="c5acf-164">Det finns två faktorer som påverkar vilka autentiseringsmetoder som är tillgängliga med en NPS-tillägg-distribution:</span><span class="sxs-lookup"><span data-stu-id="c5acf-164">There are two factors that affect which authentication methods are available with an NPS extension deployment:</span></span>

1. <span data-ttu-id="c5acf-165">hello lösenord krypteringsalgoritm som används mellan hello RADIUS-klient (VPN, Netscaler server eller andra) och hello NPS-servrar.</span><span class="sxs-lookup"><span data-stu-id="c5acf-165">hello password encryption algorithm used between hello RADIUS client (VPN, Netscaler server, or other) and hello NPS servers.</span></span>
   - <span data-ttu-id="c5acf-166">**PAP** stöder alla hello-autentiseringsmetoder för Azure MFA i molnet hello: telefonsamtal, enkelriktade SMS, mobilappavisering och Verifieringskod för mobila appar.</span><span class="sxs-lookup"><span data-stu-id="c5acf-166">**PAP** supports all hello authentication methods of Azure MFA in hello cloud: phone call, one-way text message, mobile app notification, and mobile app verification code.</span></span>
   - <span data-ttu-id="c5acf-167">**CHAPv2** och **EAP** stöd för telefonsamtal och mobilappavisering.</span><span class="sxs-lookup"><span data-stu-id="c5acf-167">**CHAPV2** and **EAP** support phone call and mobile app notification.</span></span>
2. <span data-ttu-id="c5acf-168">hello inkommande metoder som hello klientprogram (VPN, Netscaler server eller andra) kan hantera.</span><span class="sxs-lookup"><span data-stu-id="c5acf-168">hello input methods that hello client application (VPN, Netscaler server, or other) can handle.</span></span> <span data-ttu-id="c5acf-169">Till exempel har hello VPN-klienten vissa innebär tooallow hello användaren tootype i en Verifieringskod från text- eller mobilapp?</span><span class="sxs-lookup"><span data-stu-id="c5acf-169">For example, does hello VPN client have some means tooallow hello user tootype in a verification code from a text or mobile app?</span></span>

<span data-ttu-id="c5acf-170">När du distribuerar hello NPS tillägget kan använda dessa faktorer tooevaluate vilka metoder som är tillgängliga för användarna.</span><span class="sxs-lookup"><span data-stu-id="c5acf-170">When you deploy hello NPS extension, use these factors tooevaluate which methods are available for your users.</span></span> <span data-ttu-id="c5acf-171">Om RADIUS-klienten stöder PAP, men hello klienten UX saknar inmatningsfält för en Verifieringskod, sedan telefonsamtal och mobilappavisering är hello två alternativ som stöds.</span><span class="sxs-lookup"><span data-stu-id="c5acf-171">If your RADIUS client supports PAP, but hello client UX doesn't have input fields for a verification code, then phone call and mobile app notification are hello two supported options.</span></span>

<span data-ttu-id="c5acf-172">Du kan [inaktivera stöds inte autentiseringsmetoder](multi-factor-authentication-whats-next.md#selectable-verification-methods) i Azure.</span><span class="sxs-lookup"><span data-stu-id="c5acf-172">You can [disable unsupported authentication methods](multi-factor-authentication-whats-next.md#selectable-verification-methods) in Azure.</span></span>

### <a name="enable-users-for-mfa"></a><span data-ttu-id="c5acf-173">Aktivera användare för MFA</span><span class="sxs-lookup"><span data-stu-id="c5acf-173">Enable users for MFA</span></span>

<span data-ttu-id="c5acf-174">Innan du distribuerar hello fullständig NPS tillägg måste tooenable MFA för hello användare som du vill tooperform tvåstegsverifiering.</span><span class="sxs-lookup"><span data-stu-id="c5acf-174">Before you deploy hello full NPS extension, you need tooenable MFA for hello users that you want tooperform two-step verification.</span></span> <span data-ttu-id="c5acf-175">Fler omedelbart tootest hello tillägg som du distribuerar det, behöver du minst ett test-konto som är fullständigt registrerad för Multifaktorautentisering.</span><span class="sxs-lookup"><span data-stu-id="c5acf-175">More immediately, tootest hello extension as you deploy it, you need at least one test account that is fully registered for Multi-Factor Authentication.</span></span>

<span data-ttu-id="c5acf-176">Använd de här stegen tooget ett testkonto igång:</span><span class="sxs-lookup"><span data-stu-id="c5acf-176">Use these steps tooget a test account started:</span></span>
1. <span data-ttu-id="c5acf-177">Logga in för[https://aka.ms/mfasetup](https://aka.ms/mfasetup) med ett testkonto.</span><span class="sxs-lookup"><span data-stu-id="c5acf-177">Sign in too[https://aka.ms/mfasetup](https://aka.ms/mfasetup) with a test account.</span></span> 
2. <span data-ttu-id="c5acf-178">Följ hello prompter tooset upp en verifieringsmetod.</span><span class="sxs-lookup"><span data-stu-id="c5acf-178">Follow hello prompts tooset up a verification method.</span></span>
3. <span data-ttu-id="c5acf-179">Skapa en princip för villkorlig åtkomst eller [ändra hello användartillstånd](multi-factor-authentication-get-started-user-states.md) toorequire tvåstegsverifiering för hello testkonto.</span><span class="sxs-lookup"><span data-stu-id="c5acf-179">Either create a conditional access policy or [change hello user state](multi-factor-authentication-get-started-user-states.md) toorequire two-step verification for hello test account.</span></span> 

<span data-ttu-id="c5acf-180">Användarna måste också toofollow dessa steg tooenroll innan de kan autentisera med hello NPS-tillägg.</span><span class="sxs-lookup"><span data-stu-id="c5acf-180">Your users also need toofollow these steps tooenroll before they can authenticate with hello NPS extension.</span></span>

## <a name="install-hello-nps-extension"></a><span data-ttu-id="c5acf-181">Installera hello NPS-tillägg</span><span class="sxs-lookup"><span data-stu-id="c5acf-181">Install hello NPS extension</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c5acf-182">Installera hello NPS tillägg på en annan server än hello VPN åtkomstpunkt.</span><span class="sxs-lookup"><span data-stu-id="c5acf-182">Install hello NPS extension on a different server than hello VPN access point.</span></span>

### <a name="download-and-install-hello-nps-extension-for-azure-mfa"></a><span data-ttu-id="c5acf-183">Hämta och installera hello NPS-tillägg för Azure MFA</span><span class="sxs-lookup"><span data-stu-id="c5acf-183">Download and install hello NPS extension for Azure MFA</span></span>

1.  <span data-ttu-id="c5acf-184">[Hämta hello NPS tillägget](https://aka.ms/npsmfa) från hello Microsoft Download Center.</span><span class="sxs-lookup"><span data-stu-id="c5acf-184">[Download hello NPS Extension](https://aka.ms/npsmfa) from hello Microsoft Download Center.</span></span>
2.  <span data-ttu-id="c5acf-185">Kopiera hello binära toohello Network Policy Server du vill tooconfigure.</span><span class="sxs-lookup"><span data-stu-id="c5acf-185">Copy hello binary toohello Network Policy Server you want tooconfigure.</span></span>
3.  <span data-ttu-id="c5acf-186">Kör *setup.exe* och följ anvisningarna för installation av hello.</span><span class="sxs-lookup"><span data-stu-id="c5acf-186">Run *setup.exe* and follow hello installation instructions.</span></span> <span data-ttu-id="c5acf-187">Om det uppstår fel kontrollerar du att hello två bibliotek från hello nödvändiga avsnitt har installerats.</span><span class="sxs-lookup"><span data-stu-id="c5acf-187">If you encounter errors, double-check that hello two libraries from hello prerequisite section were successfully installed.</span></span>

### <a name="run-hello-powershell-script"></a><span data-ttu-id="c5acf-188">Kör hello PowerShell-skript</span><span class="sxs-lookup"><span data-stu-id="c5acf-188">Run hello PowerShell script</span></span>

<span data-ttu-id="c5acf-189">hello installationsprogrammet skapar ett PowerShell-skript på den här platsen: `C:\Program Files\Microsoft\AzureMfa\Config` (där C:\ är installationsenheten).</span><span class="sxs-lookup"><span data-stu-id="c5acf-189">hello installer creates a PowerShell script in this location: `C:\Program Files\Microsoft\AzureMfa\Config` (where C:\ is your installation drive).</span></span> <span data-ttu-id="c5acf-190">Detta PowerShell-skript utför hello följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="c5acf-190">This PowerShell script performs hello following actions:</span></span>

-   <span data-ttu-id="c5acf-191">Skapa ett självsignerat certifikat.</span><span class="sxs-lookup"><span data-stu-id="c5acf-191">Create a self-signed certificate.</span></span>
-   <span data-ttu-id="c5acf-192">Associera hello offentliga nyckeln för hello certifikat toohello tjänstens huvudnamn på Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c5acf-192">Associate hello public key of hello certificate toohello service principal on Azure AD.</span></span>
-   <span data-ttu-id="c5acf-193">Lagra hello cert i hello lokala datorns certifikatarkiv.</span><span class="sxs-lookup"><span data-stu-id="c5acf-193">Store hello cert in hello local machine cert store.</span></span>
-   <span data-ttu-id="c5acf-194">Bevilja åtkomst toohello certifikatets privata nyckel tooNetwork användare.</span><span class="sxs-lookup"><span data-stu-id="c5acf-194">Grant access toohello certificate’s private key tooNetwork User.</span></span>
-   <span data-ttu-id="c5acf-195">Starta om hello NPS.</span><span class="sxs-lookup"><span data-stu-id="c5acf-195">Restart hello NPS.</span></span>

<span data-ttu-id="c5acf-196">Om du inte vill toouse egna kör certifikat (i stället för hello självsignerade certifikat som hello PowerShell-skriptet genererar), hello PowerShell-skript toocomplete hello installation.</span><span class="sxs-lookup"><span data-stu-id="c5acf-196">Unless you want toouse your own certificates (instead of hello self-signed certificates that hello PowerShell script generates), run hello PowerShell Script toocomplete hello installation.</span></span> <span data-ttu-id="c5acf-197">Om du installerar tillägget för hello på flera servrar, ha var och en sina egna certifikat.</span><span class="sxs-lookup"><span data-stu-id="c5acf-197">If you install hello extension on multiple servers, each one should have its own certificate.</span></span>

1. <span data-ttu-id="c5acf-198">Kör Windows PowerShell som administratör.</span><span class="sxs-lookup"><span data-stu-id="c5acf-198">Run Windows PowerShell as an administrator.</span></span>
2. <span data-ttu-id="c5acf-199">Ändra kataloger.</span><span class="sxs-lookup"><span data-stu-id="c5acf-199">Change directories.</span></span>

   `cd "C:\Program Files\Microsoft\AzureMfa\Config"`

3. <span data-ttu-id="c5acf-200">Kör hello PowerShell-skript som skapats av hello installer.</span><span class="sxs-lookup"><span data-stu-id="c5acf-200">Run hello PowerShell script created by hello installer.</span></span>

   `.\AzureMfaNpsExtnConfigSetup.ps1`

4. <span data-ttu-id="c5acf-201">PowerShell frågar efter klient-ID.</span><span class="sxs-lookup"><span data-stu-id="c5acf-201">PowerShell prompts for your tenant ID.</span></span> <span data-ttu-id="c5acf-202">Använd hello Directory ID GUID som du kopierade från hello Azure-portalen i avsnittet förutsättningar för hello.</span><span class="sxs-lookup"><span data-stu-id="c5acf-202">Use hello Directory ID GUID that you copied from hello Azure portal in hello prerequisites section.</span></span>
5. <span data-ttu-id="c5acf-203">Logga in som administratör tooAzure AD.</span><span class="sxs-lookup"><span data-stu-id="c5acf-203">Sign in tooAzure AD as an administrator.</span></span>
6. <span data-ttu-id="c5acf-204">PowerShell visar ett meddelande när hello skriptet har slutförts.</span><span class="sxs-lookup"><span data-stu-id="c5acf-204">PowerShell shows a success message when hello script is finished.</span></span>  

<span data-ttu-id="c5acf-205">Upprepa dessa steg på ytterligare NPS-servrar som du vill tooset för belastningsutjämning.</span><span class="sxs-lookup"><span data-stu-id="c5acf-205">Repeat these steps on any additional NPS servers that you want tooset up for load balancing.</span></span>

>[!NOTE]
><span data-ttu-id="c5acf-206">Om du använder egna certifikat i stället för att certifikat med hello PowerShell-skript, se till att de matchar toohello NPS namngivningskonvention.</span><span class="sxs-lookup"><span data-stu-id="c5acf-206">If you use your own certificates instead of generating certificates with hello PowerShell script, make sure that they align toohello NPS naming convention.</span></span> <span data-ttu-id="c5acf-207">hello ämnesnamn måste vara **CN =\<TenantID\>, OU = Microsoft NPS tillägget**.</span><span class="sxs-lookup"><span data-stu-id="c5acf-207">hello subject name must be **CN=\<TenantID\>,OU=Microsoft NPS Extension**.</span></span> 

## <a name="configure-your-nps-extension"></a><span data-ttu-id="c5acf-208">Konfigurera NPS-tillägget</span><span class="sxs-lookup"><span data-stu-id="c5acf-208">Configure your NPS extension</span></span>

<span data-ttu-id="c5acf-209">Det här avsnittet innehåller överväganden vid utformning och förslag för lyckad distribution för NPS-tillägget.</span><span class="sxs-lookup"><span data-stu-id="c5acf-209">This section includes design considerations and suggestions for successful NPS extension deployments.</span></span>

### <a name="configuration-limitations"></a><span data-ttu-id="c5acf-210">Begränsningar för konfiguration</span><span class="sxs-lookup"><span data-stu-id="c5acf-210">Configuration limitations</span></span>

- <span data-ttu-id="c5acf-211">hello NPS-tillägg för Azure MFA inkluderar inte verktyg toomigrate användare och inställningar från MFA-servern toohello moln.</span><span class="sxs-lookup"><span data-stu-id="c5acf-211">hello NPS extension for Azure MFA does not include tools toomigrate users and settings from MFA Server toohello cloud.</span></span> <span data-ttu-id="c5acf-212">Därför föreslår vi använder hello-tillägg för nya distributioner i stället för befintlig distribution.</span><span class="sxs-lookup"><span data-stu-id="c5acf-212">For this reason, we suggest using hello extension for new deployments, rather than existing deployment.</span></span> <span data-ttu-id="c5acf-213">Om du använder hello tillägg på en befintlig distribution kan användarna har tooperform bevis upp igen toopopulate sina MFA information i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="c5acf-213">If you use hello extension on an existing deployment, your users have tooperform proof-up again toopopulate their MFA details in hello cloud.</span></span>  
- <span data-ttu-id="c5acf-214">hello NPS tillägget använder hello UPN från hello lokala Active directory tooidentify hello-användare på Azure MFA för att utföra hello sekundära auktor hello tillägget kan vara konfigurerade toouse en annan identifierare som alternativt inloggnings-ID eller anpassade Active Directory fältet än UPN.</span><span class="sxs-lookup"><span data-stu-id="c5acf-214">hello NPS extension uses hello UPN from hello on-premises Active directory tooidentify hello user on Azure MFA for performing hello Secondary Auth. hello extension can be configured toouse a different identifier like alternate login ID or custom Active Directory field other than UPN.</span></span> <span data-ttu-id="c5acf-215">Se [avancerade konfigurationsalternativ för hello NPS-tillägget för Multifaktorautentisering](multi-factor-authentication-advanced-vpn-configurations.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="c5acf-215">See [Advanced configuration options for hello NPS extension for Multi-Factor Authentication](multi-factor-authentication-advanced-vpn-configurations.md) for more information.</span></span>
- <span data-ttu-id="c5acf-216">Inte alla krypteringsprotokoll stöder alla verifieringsmetoderna.</span><span class="sxs-lookup"><span data-stu-id="c5acf-216">Not all encryption protocols support all verification methods.</span></span>
   - <span data-ttu-id="c5acf-217">**PAP** stöder telefonsamtal, enkelriktade SMS, mobilappavisering och Verifieringskod för mobila appar</span><span class="sxs-lookup"><span data-stu-id="c5acf-217">**PAP** supports phone call, one-way text message, mobile app notification, and mobile app verification code</span></span>
   - <span data-ttu-id="c5acf-218">**CHAPv2** och **EAP** stöd för telefonsamtal och mobilappavisering</span><span class="sxs-lookup"><span data-stu-id="c5acf-218">**CHAPV2** and **EAP** support phone call and mobile app notification</span></span>

### <a name="control-radius-clients-that-require-mfa"></a><span data-ttu-id="c5acf-219">Kontrollen RADIUS-klienter som kräver MFA</span><span class="sxs-lookup"><span data-stu-id="c5acf-219">Control RADIUS clients that require MFA</span></span>

<span data-ttu-id="c5acf-220">När du aktiverar Multifaktorautentisering för en RADIUS-klient som använder hello NPS tillägget är alla autentiseringar för den här klienten nödvändiga tooperform MFA.</span><span class="sxs-lookup"><span data-stu-id="c5acf-220">Once you enable MFA for a RADIUS client using hello NPS Extension, all authentications for this client are required tooperform MFA.</span></span> <span data-ttu-id="c5acf-221">Om du vill tooenable MFA för vissa RADIUS-klienter kan du konfigurera två NPS-servrar och installera hello tillägg på endast en av dem.</span><span class="sxs-lookup"><span data-stu-id="c5acf-221">If you want tooenable MFA for some RADIUS clients but not others, you can configure two NPS servers and install hello extension on only one of them.</span></span> <span data-ttu-id="c5acf-222">Konfigurera RADIUS-klienter som du vill toorequire MFA toosend begäranden toohello NPS-server konfigurerad med hello tillägg och andra RADIUS-klienter toohello NPS-servern inte har konfigurerats med hello-tillägget.</span><span class="sxs-lookup"><span data-stu-id="c5acf-222">Configure RADIUS clients that you want toorequire MFA toosend requests toohello NPS server configured with hello extension, and other RADIUS clients toohello NPS server not configured with hello extension.</span></span>

### <a name="prepare-for-users-that-arent-enrolled-for-mfa"></a><span data-ttu-id="c5acf-223">Förbereda för användare som inte är registrerade för MFA</span><span class="sxs-lookup"><span data-stu-id="c5acf-223">Prepare for users that aren't enrolled for MFA</span></span>

<span data-ttu-id="c5acf-224">Om du har användare som inte är registrerade för MFA, kan du bestämma vad som händer när de försöker tooauthenticate.</span><span class="sxs-lookup"><span data-stu-id="c5acf-224">If you have users that aren't enrolled for MFA, you can determine what happens when they try tooauthenticate.</span></span> <span data-ttu-id="c5acf-225">Använd hello registerinställning *REQUIRE_USER_MATCH* i hello registersökväg *HKLM\Software\Microsoft\AzureMFA* toocontrol hello funktionen beteende.</span><span class="sxs-lookup"><span data-stu-id="c5acf-225">Use hello registry setting *REQUIRE_USER_MATCH* in hello registry path *HKLM\Software\Microsoft\AzureMFA* toocontrol hello feature behavior.</span></span> <span data-ttu-id="c5acf-226">Den här inställningen har ett enda konfigurationsalternativ:</span><span class="sxs-lookup"><span data-stu-id="c5acf-226">This setting has a single configuration option:</span></span>

| <span data-ttu-id="c5acf-227">Nyckel</span><span class="sxs-lookup"><span data-stu-id="c5acf-227">Key</span></span> | <span data-ttu-id="c5acf-228">Värde</span><span class="sxs-lookup"><span data-stu-id="c5acf-228">Value</span></span> | <span data-ttu-id="c5acf-229">Standard</span><span class="sxs-lookup"><span data-stu-id="c5acf-229">Default</span></span> |
| --- | ----- | ------- |
| <span data-ttu-id="c5acf-230">REQUIRE_USER_MATCH</span><span class="sxs-lookup"><span data-stu-id="c5acf-230">REQUIRE_USER_MATCH</span></span> | <span data-ttu-id="c5acf-231">SANT/FALSKT</span><span class="sxs-lookup"><span data-stu-id="c5acf-231">TRUE/FALSE</span></span> | <span data-ttu-id="c5acf-232">Ange inte (motsvarande tooTRUE)</span><span class="sxs-lookup"><span data-stu-id="c5acf-232">Not set (equivalent tooTRUE)</span></span> |

<span data-ttu-id="c5acf-233">Hej syftet med den här inställningen är toodetermine vilka toodo när en användare inte har registrerats för MFA.</span><span class="sxs-lookup"><span data-stu-id="c5acf-233">hello purpose of this setting is toodetermine what toodo when a user is not enrolled for MFA.</span></span> <span data-ttu-id="c5acf-234">När hello nyckeln finns inte, har inte angetts eller har angetts tooTRUE, och hello användaren har registrerats inte och sedan hello tillägget misslyckas hello MFA-kontrollen.</span><span class="sxs-lookup"><span data-stu-id="c5acf-234">When hello key does not exist, is not set, or is set tooTRUE, and hello user is not enrolled, then hello extension fails hello MFA challenge.</span></span> <span data-ttu-id="c5acf-235">När hello nyckeln anges tooFALSE och hello användaren inte har registrerats, fortsätter autentiseringen utan att utföra MFA.</span><span class="sxs-lookup"><span data-stu-id="c5acf-235">When hello key is set tooFALSE and hello user is not enrolled, authentication proceeds without performing MFA.</span></span>

<span data-ttu-id="c5acf-236">Du kan välja toocreate den här nyckeln och ange den tooFALSE medan användarna är onboarding och kan inte registreras för Azure MFA ännu.</span><span class="sxs-lookup"><span data-stu-id="c5acf-236">You can choose toocreate this key and set it tooFALSE while your users are onboarding, and may not all be enrolled for Azure MFA yet.</span></span> <span data-ttu-id="c5acf-237">Eftersom inställningen hello nyckeln tillåter att användare som inte är registrerade för MFA toosign i, bör du dock ta bort den här nyckeln innan pågående tooproduction.</span><span class="sxs-lookup"><span data-stu-id="c5acf-237">However, since setting hello key permits users that aren't enrolled for MFA toosign in, you should remove this key before going tooproduction.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="c5acf-238">Felsökning</span><span class="sxs-lookup"><span data-stu-id="c5acf-238">Troubleshooting</span></span>

### <a name="how-do-i-verify-that-hello-client-cert-is-installed-as-expected"></a><span data-ttu-id="c5acf-239">Hur kan jag bekräfta att hello klientens certifikat har installerats som förväntat?</span><span class="sxs-lookup"><span data-stu-id="c5acf-239">How do I verify that hello client cert is installed as expected?</span></span>

<span data-ttu-id="c5acf-240">Titta efter hello självsignerade certifikat som skapas av hello installer i hello certifikatarkiv och kontrollera att hello privat nyckel har behörigheterna toouser **NÄTVERKSTJÄNSTEN**.</span><span class="sxs-lookup"><span data-stu-id="c5acf-240">Look for hello self-signed certificate created by hello installer in hello cert store, and check that hello private key has permissions granted toouser **NETWORK SERVICE**.</span></span> <span data-ttu-id="c5acf-241">hello certifikat har ett ämnesnamn **CN \<tenantid\>, OU = Microsoft NPS-tillägg**</span><span class="sxs-lookup"><span data-stu-id="c5acf-241">hello cert has a subject name of **CN \<tenantid\>, OU = Microsoft NPS Extension**</span></span>

-------------------------------------------------------------

### <a name="how-can-i-verify-that-my-client-cert-is-associated-toomy-tenant-in-azure-active-directory"></a><span data-ttu-id="c5acf-242">Hur kan jag bekräfta att Mina objekt är kopplade toomy klient i Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="c5acf-242">How can I verify that my client cert is associated toomy tenant in Azure Active Directory?</span></span>

<span data-ttu-id="c5acf-243">Öppna PowerShell-Kommandotolken och kör hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="c5acf-243">Open PowerShell command prompt and run hello following commands:</span></span>

```
import-module MSOnline
Connect-MsolService
Get-MsolServicePrincipalCredential -AppPrincipalId "981f26a1-7f43-403b-a875-f8b09b8cd720" -ReturnKeyValues 1 
```

<span data-ttu-id="c5acf-244">Dessa kommandon ut alla hello certifikat associera din klient med din instans av hello NPS-tillägget i PowerShell-sessionen.</span><span class="sxs-lookup"><span data-stu-id="c5acf-244">These commands print all hello certificates associating your tenant with your instance of hello NPS extension in your PowerShell session.</span></span> <span data-ttu-id="c5acf-245">Leta efter certifikatet genom att exportera certifikatet klienten som en ”Base64-kodat X.509(.cer)” fil utan hello privata nyckeln och jämför den med hello lista från PowerShell.</span><span class="sxs-lookup"><span data-stu-id="c5acf-245">Look for your certificate by exporting your client cert as a "Base-64 encoded X.509(.cer)" file without hello private key, and compare it with hello list from PowerShell.</span></span>

<span data-ttu-id="c5acf-246">Giltigt-från- och -tills tidsstämplar som är läsbart format, kan bara använda toofilter ut uppenbara misfits om hello kommando returnerar mer än ett certifikat.</span><span class="sxs-lookup"><span data-stu-id="c5acf-246">Valid-From and Valid-Until timestamps, which are in human-readable form, can be used toofilter out obvious misfits if hello command returns more than one cert.</span></span>

-------------------------------------------------------------

### <a name="why-are-my-requests-failing-with-adal-token-error"></a><span data-ttu-id="c5acf-247">Varför är min begäranden inte med ADAL fel?</span><span class="sxs-lookup"><span data-stu-id="c5acf-247">Why are my requests failing with ADAL token error?</span></span>

<span data-ttu-id="c5acf-248">Detta fel kan bero på grund av tooone av flera skäl.</span><span class="sxs-lookup"><span data-stu-id="c5acf-248">This error could be due tooone of several reasons.</span></span> <span data-ttu-id="c5acf-249">Följ dessa steg toohelp felsöka:</span><span class="sxs-lookup"><span data-stu-id="c5acf-249">Use these steps toohelp troubleshoot:</span></span>

1. <span data-ttu-id="c5acf-250">Starta om NPS-servern.</span><span class="sxs-lookup"><span data-stu-id="c5acf-250">Restart your NPS server.</span></span>
2. <span data-ttu-id="c5acf-251">Kontrollera att certifikat som klienten är installerad som förväntat.</span><span class="sxs-lookup"><span data-stu-id="c5acf-251">Verify that that client cert is installed as expected.</span></span>
3. <span data-ttu-id="c5acf-252">Kontrollera att hello-certifikatet är kopplat till din klient på Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c5acf-252">Verify that hello certificate is associated with your tenant on Azure AD.</span></span>
4. <span data-ttu-id="c5acf-253">Kontrollera att https://login.microsoftonline.com/ är tillgänglig från hello-server som kör hello-tillägget.</span><span class="sxs-lookup"><span data-stu-id="c5acf-253">Verify that https://login.microsoftonline.com/ is accessible from hello server running hello extension.</span></span>

-------------------------------------------------------------

### <a name="why-does-authentication-fail-with-an-error-in-http-logs-stating-that-hello-user-is-not-found"></a><span data-ttu-id="c5acf-254">Varför misslyckas autentiseringen med ett fel i HTTP-loggarna som om det gick inte att hitta användaren hello?</span><span class="sxs-lookup"><span data-stu-id="c5acf-254">Why does authentication fail with an error in HTTP logs stating that hello user is not found?</span></span>

<span data-ttu-id="c5acf-255">Kontrollera att AD Connect är igång och att användaren hello finns i både Windows Active Directory och Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="c5acf-255">Verify that AD Connect is running, and that hello user is present in both Windows Active Directory and Azure Active Directory.</span></span>

------------------------------------------------------------

### <a name="why-do-i-see-http-connect-errors-in-logs-with-all-my-authentications-failing"></a><span data-ttu-id="c5acf-256">Varför ser HTTP connect-fel i loggarna med min autentiseringar misslyckas?</span><span class="sxs-lookup"><span data-stu-id="c5acf-256">Why do I see HTTP connect errors in logs with all my authentications failing?</span></span>

<span data-ttu-id="c5acf-257">Kontrollera att https://adnotifications.windowsazure.com kan nås från hello-server som kör hello NPS-tillägget.</span><span class="sxs-lookup"><span data-stu-id="c5acf-257">Verify that https://adnotifications.windowsazure.com is reachable from hello server running hello NPS extension.</span></span>


## <a name="next-steps"></a><span data-ttu-id="c5acf-258">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c5acf-258">Next steps</span></span>

- <span data-ttu-id="c5acf-259">Konfigurera alternativa ID för inloggning eller ställa in en undantagslista för IP-adresser som inte får utföra tvåstegsverifiering i [avancerade konfigurationsalternativ för hello NPS-tillägg för Multifaktorautentisering](nps-extension-advanced-configuration.md)</span><span class="sxs-lookup"><span data-stu-id="c5acf-259">Configure alternate IDs for login, or set up an exception list for IPs that shouldn't perform two-step verification in [Advanced configuration options for hello NPS extension for Multi-Factor Authentication](nps-extension-advanced-configuration.md)</span></span>

- [<span data-ttu-id="c5acf-260">Lösa felmeddelanden från hello NPS-tillägg för Azure Multi-Factor Authentication</span><span class="sxs-lookup"><span data-stu-id="c5acf-260">Resolve error messages from hello NPS extension for Azure Multi-Factor Authentication</span></span>](multi-factor-authentication-nps-errors.md)
