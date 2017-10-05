---
title: "VPN-integrering med Azure MFA med hjälp av NPS-tillägg | Microsoft Docs"
description: "Den här artikeln beskrivs integrera din VPN-infrastruktur med Azure MFA med hjälp av Server (NPS)-tillägget för Microsoft Azure."
services: active-directory
keywords: "Azure MFA integrera VPN, Azure Active Directory, NPS-tillägg"
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: kgremban
ms.reviewer: jsnow
ms.custom: it-pro
ms.openlocfilehash: 3dfcf25856ede50266336c2ebb057dd3f7b8897e
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="integrate-your-vpn-infrastructure-with-azure-multi-factor-authentication-mfa-using-the-network-policy-server-nps-extension-for-azure"></a><span data-ttu-id="ad867-104">Integrera din VPN-infrastruktur med Azure Multi-Factor Authentication (MFA) med hjälp av Server (NPS)-tillägget för Azure</span><span class="sxs-lookup"><span data-stu-id="ad867-104">Integrate your VPN infrastructure with Azure Multi-Factor Authentication (MFA) using the Network Policy Server (NPS) extension for Azure</span></span>

## <a name="overview"></a><span data-ttu-id="ad867-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="ad867-105">Overview</span></span>

<span data-ttu-id="ad867-106">Tjänsten NPS (Network Policy)-tillägget för Azure gör att organisationer kan skydda RADIUS Remote Authentication Dial-In User Service ()-klientautentisering med hjälp av molnbaserade [Azure Multi-Factor Authentication (MFA)](multi-factor-authentication-get-started-server-rdg.md), som innehåller tvåstegsverifiering.</span><span class="sxs-lookup"><span data-stu-id="ad867-106">The Network Policy Service (NPS) extension for Azure allows organizations to safeguard Remote Authentication Dial-In User Service (RADIUS) client authentication using cloud-based [Azure Multi-Factor Authentication (MFA)](multi-factor-authentication-get-started-server-rdg.md), which provides two-step verification.</span></span>

<span data-ttu-id="ad867-107">Den här artikeln innehåller instruktioner för att integrera NPS-infrastrukturen med Azure MFA med hjälp av NPS-tillägget för Azure för att aktivera säker tvåstegsverifiering för användare som försöker ansluta till nätverket med hjälp av en VPN-anslutning.</span><span class="sxs-lookup"><span data-stu-id="ad867-107">This article provides instructions for integrating the NPS infrastructure with Azure MFA using the NPS extension for Azure to enable secure two-step verification for users attempting to connect to your network using a VPN.</span></span> 

<span data-ttu-id="ad867-108">Nätverksprincip och Access Services (NPS) gör att organisationer följande möjligheter:</span><span class="sxs-lookup"><span data-stu-id="ad867-108">The Network Policy and Access Services (NPS) gives organizations the following abilities:</span></span>

* <span data-ttu-id="ad867-109">Ange centrala platser för hantering och kontroll av begäranden för nätverk att ange vem som kan ansluta, vilka tidpunkter på dagen anslutningar tillåts varaktigheten för anslutningar och säkerhetsnivån som klienter måste använda för att ansluta och så vidare.</span><span class="sxs-lookup"><span data-stu-id="ad867-109">Specify central locations for the management and control of network requests to specify who can connect, what times of day connections are allowed, the duration of connections, and the level of security that clients must use to connect, and so on.</span></span> <span data-ttu-id="ad867-110">I stället för att ange dessa principer på varje server för VPN- eller Gateway för fjärrskrivbord (RD), kan dessa principer anges en gång på en central plats.</span><span class="sxs-lookup"><span data-stu-id="ad867-110">Rather than specify these policies on each VPN or Remote Desktop (RD) Gateway server, these policies can be specified once in a central location.</span></span> <span data-ttu-id="ad867-111">RADIUS-protokollet används för att ge centraliserad autentisering, auktorisering och redovisning.</span><span class="sxs-lookup"><span data-stu-id="ad867-111">The RADIUS protocol is used to provide the centralized Authentication, Authorization, and Accounting (AAA).</span></span> 
* <span data-ttu-id="ad867-112">Skapa och tillämpa hälsopolicys för Network Access Protection (NAP) klienten som avgör om enheter har beviljats obegränsad eller begränsad åtkomst till nätverksresurser.</span><span class="sxs-lookup"><span data-stu-id="ad867-112">Establish and enforce Network Access Protection (NAP) client health policies that determine whether devices are granted unrestricted or restricted access to network resources.</span></span>
* <span data-ttu-id="ad867-113">Ger möjlighet att genomdriva autentisering och auktorisering för åtkomst till 802. 1 x-kompatibla trådlösa åtkomstpunkter och Ethernet-växlar.</span><span class="sxs-lookup"><span data-stu-id="ad867-113">Provide a means to enforce authentication and authorization for access to 802.1x-capable wireless access points and Ethernet switches.</span></span>    

<span data-ttu-id="ad867-114">Mer information finns i [Server (NPS)](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-top).</span><span class="sxs-lookup"><span data-stu-id="ad867-114">For more information, see [Network Policy Server (NPS)](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-top).</span></span> 

<span data-ttu-id="ad867-115">Om du vill förbättra säkerheten och tillhandahålla hög kompatibilitet, företag kan integrera NPS med Azure MFA för att säkerställa att användarna använder tvåstegsverifiering ska kunna ansluta till den virtuella porten på VPN-servern.</span><span class="sxs-lookup"><span data-stu-id="ad867-115">To enhance security and provide high level of compliance, organizations can integrate NPS with Azure MFA to ensure that users use two-step verification to be able connect to the virtual port on the VPN server.</span></span> <span data-ttu-id="ad867-116">De måste ange sina användarnamn/lösenord tillsammans med information som användaren har i deras kontroll för användare att få åtkomst.</span><span class="sxs-lookup"><span data-stu-id="ad867-116">For users to be granted access, they must provide their username/password combination with information that the user has in their control.</span></span> <span data-ttu-id="ad867-117">Den här informationen måste betrodda och inte enkelt dubbleras, till exempel ett mobiltelefonnummer, fast telefon nummer, program på en mobil enhet och så vidare.</span><span class="sxs-lookup"><span data-stu-id="ad867-117">This information must be trusted and not easily duplicated, such as a cell phone number, landline number, application on a mobile device, and so on.</span></span>

<span data-ttu-id="ad867-118">Kunder som vill implementera tvåstegsverifiering för integrerad NPS och Azure MFA-miljöer hade konfigurerar och underhåller en separat MFA-Server i den lokala miljön enligt beskrivningen i fjärrskrivbordsgateway och Azure Multi-Factor Authentication-servern med RADIUS innan tillgängligheten för NPS-tillägget för Azure.</span><span class="sxs-lookup"><span data-stu-id="ad867-118">Prior to the availability of the NPS extension for Azure, customers who wished to implement two-step verification for integrated NPS and Azure MFA environments had to configure and maintain a separate MFA Server in the on-premises environment as documented in Remote Desktop Gateway and Azure Multi-Factor Authentication Server using RADIUS.</span></span>

<span data-ttu-id="ad867-119">Tillgängligheten för NPS-tillägget för Azure nu ger organisationer möjlighet att distribuera en lokalt baserad MFA-lösning eller en molnbaserad lösning för MFA till säker autentisering för RADIUS-klienten.</span><span class="sxs-lookup"><span data-stu-id="ad867-119">The availability of the NPS extension for Azure now gives organizations the choice to deploy either an on-premises based MFA solution or a cloud-based MFA solution to secure RADIUS client authentication.</span></span>
 
## <a name="authentication-flow"></a><span data-ttu-id="ad867-120">Autentiseringsflöde</span><span class="sxs-lookup"><span data-stu-id="ad867-120">Authentication Flow</span></span>
<span data-ttu-id="ad867-121">När en användare ansluter till en virtuell port på en VPN-server, måste de först autentisera med olika protokoll som tillåter användning av en kombination av användarnamn/lösenord och certifikatbaserade autentiseringsmetoder.</span><span class="sxs-lookup"><span data-stu-id="ad867-121">When a user connects to a virtual port on a VPN server, they must first authenticate using a variety of protocols, which allow the use of a combination of user name/password and certificate-based authentication methods.</span></span> 

<span data-ttu-id="ad867-122">Utöver verifiering, och verifiera identitet har användare rätt behörigheter för.</span><span class="sxs-lookup"><span data-stu-id="ad867-122">In addition to authenticating and verifying identity, users must have the appropriate dial-in permissions.</span></span> <span data-ttu-id="ad867-123">I enklare implementationer anges dessa behörighet som tillåter åtkomst direkt i Active Directory-användarobjekt.</span><span class="sxs-lookup"><span data-stu-id="ad867-123">In simple implementations, these dial-in permissions that allow access are set directly on the Active Directory user objects.</span></span> 

 ![Egenskaper för användare](./media/nps-extension-vpn/image1.png)

<span data-ttu-id="ad867-125">Enklare implementationer varje VPN-server som beviljar eller nekar åtkomst baserat på principer som definierats på varje lokala VPN-servern.</span><span class="sxs-lookup"><span data-stu-id="ad867-125">For simple implementations, each VPN server grants or denies access based on policies defined on each local VPN server.</span></span>

<span data-ttu-id="ad867-126">I större och mer skalbar implementeringar av principerna för att bevilja eller neka VPN-åtkomst är centraliserad på RADIUS-servrar.</span><span class="sxs-lookup"><span data-stu-id="ad867-126">In larger and more scalable implementations, the polices that grant or deny VPN access are centralized on RADIUS servers.</span></span> <span data-ttu-id="ad867-127">I det här fallet fungerar VPN-servern som en fjärråtkomstserver (RADIUS-klienten) som vidarebefordrar förfrågningar och kontot meddelanden till en RADIUS-server.</span><span class="sxs-lookup"><span data-stu-id="ad867-127">In this case, the VPN server acts as an access server (RADIUS client) that forwards connection requests and account messages to a RADIUS server.</span></span> <span data-ttu-id="ad867-128">Användare måste autentiseras för att ansluta till den virtuella porten på VPN-servern, och uppfyller de villkor som definieras centralt på RADIUS-servrar.</span><span class="sxs-lookup"><span data-stu-id="ad867-128">To connect to the virtual port on the VPN server, users must be authenticated and meet the conditions defined centrally on RADIUS servers.</span></span> 

<span data-ttu-id="ad867-129">När NPS-tillägget för Azure är integrerad med NPS, är flödet för autentiseringen följande:</span><span class="sxs-lookup"><span data-stu-id="ad867-129">When the NPS extension for Azure is integrated with the NPS, the successful authentication flow is as follows:</span></span>

1. <span data-ttu-id="ad867-130">VPN-servern tar emot en begäran om autentisering från en VPN-användare som innehåller användarnamn och lösenord för att ansluta till en resurs, till exempel en fjärrskrivbordssession.</span><span class="sxs-lookup"><span data-stu-id="ad867-130">The VPN server receives an authentication request from a VPN user that includes the username and password to connect to a resource, such as a Remote Desktop session.</span></span> 
2. <span data-ttu-id="ad867-131">Fungerar som en RADIUS-klient, VPN-servern konverterar begäran till en RADIUS-åtkomstbegäran och skickar meddelandet (lösenord krypteras) till RADIUS-(NPS)-servern där NPS-tillägg har installerats.</span><span class="sxs-lookup"><span data-stu-id="ad867-131">Acting as a RADIUS client, VPN server converts the request to a RADIUS Access-Request message and sends the message (password is encrypted) to the RADIUS (NPS) server where the NPS extension is installed.</span></span> 
3. <span data-ttu-id="ad867-132">Kombinationen av användarnamn och lösenord har verifierats i Active Directory.</span><span class="sxs-lookup"><span data-stu-id="ad867-132">The username and password combination is verified in Active Directory.</span></span> <span data-ttu-id="ad867-133">Om användarnamnet / lösenord är felaktigt, RADIUS-servern skickar ett meddelande om nekad åtkomst.</span><span class="sxs-lookup"><span data-stu-id="ad867-133">If the username / password is incorrect, the RADIUS Server sends an Access-Reject message.</span></span> 
4. <span data-ttu-id="ad867-134">Om alla villkor som anges i NPS-anslutningsbegäran och nätverksprinciper uppfylls (till exempel tidpunkt eller grupp medlemskap begränsningar), NPS-tillägget utlöser en begäran om sekundära autentiseringen med Azure MFA.</span><span class="sxs-lookup"><span data-stu-id="ad867-134">If all conditions as specified in the NPS Connection Request and Network Policies are met (for example, time of day or group membership restrictions), the NPS extension triggers a request for secondary authentication with Azure MFA.</span></span> 
5. <span data-ttu-id="ad867-135">Azure MFA kommunicerar med Azure Active Directory, hämtar användarens information och utför sekundära autentiseringen genom att använda metoden konfigureras av användaren (textmeddelande, mobilapp och så vidare).</span><span class="sxs-lookup"><span data-stu-id="ad867-135">Azure MFA communicates with Azure Active Directory, retrieves the user’s details, and performs the secondary authentication using the method configured by the user (text message, mobile app, and so on).</span></span> 
6. <span data-ttu-id="ad867-136">Vid framgången för MFA-kontrollen har kommunicerar Azure MFA resultatet till NPS-tillägget.</span><span class="sxs-lookup"><span data-stu-id="ad867-136">Upon success of the MFA challenge, Azure MFA communicates the result to the NPS extension.</span></span>
7. <span data-ttu-id="ad867-137">När anslutningsförsöket både autentiseras och auktoriseras skickar NPS-servern där tillägget installeras en RADIUS-åtkomst-meddelande till VPN-server (RADIUS-klienten).</span><span class="sxs-lookup"><span data-stu-id="ad867-137">After the connection attempt is both authenticated and authorized, the NPS server where the extension is installed sends a RADIUS Access-Accept message to the VPN server (RADIUS client).</span></span>
8. <span data-ttu-id="ad867-138">Användaren beviljas åtkomst till den virtuella porten på VPN-servern och upprättar en krypterad VPN-tunnel.</span><span class="sxs-lookup"><span data-stu-id="ad867-138">The user is granted access to the virtual port on VPN server and establishes an encrypted VPN tunnel.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ad867-139">Krav</span><span class="sxs-lookup"><span data-stu-id="ad867-139">Prerequisites</span></span>
<span data-ttu-id="ad867-140">Det här avsnittet beskrivs förutsättningarna som krävs innan du integrera Azure MFA med fjärrskrivbordsgateway.</span><span class="sxs-lookup"><span data-stu-id="ad867-140">This section details the prerequisites necessary before integrating Azure MFA with the Remote Desktop Gateway.</span></span> <span data-ttu-id="ad867-141">Innan du börjar måste du ha följande krav på plats.</span><span class="sxs-lookup"><span data-stu-id="ad867-141">Before you begin, you must have the following prerequisites in place.</span></span>

* <span data-ttu-id="ad867-142">VPN-infrastruktur</span><span class="sxs-lookup"><span data-stu-id="ad867-142">VPN infrastructure</span></span>
* <span data-ttu-id="ad867-143">Nätverksprincip och Access Services (NPS)</span><span class="sxs-lookup"><span data-stu-id="ad867-143">Network Policy and Access Services (NPS) role</span></span>
* <span data-ttu-id="ad867-144">Azure MFA-licens</span><span class="sxs-lookup"><span data-stu-id="ad867-144">Azure MFA License</span></span>
* <span data-ttu-id="ad867-145">Windows Server-programvara</span><span class="sxs-lookup"><span data-stu-id="ad867-145">Windows Server software</span></span>
* <span data-ttu-id="ad867-146">Bibliotek</span><span class="sxs-lookup"><span data-stu-id="ad867-146">Libraries</span></span>
* <span data-ttu-id="ad867-147">Azure AD synkroniserade med lokala AD</span><span class="sxs-lookup"><span data-stu-id="ad867-147">Azure AD synched with on-premises AD</span></span> 
* <span data-ttu-id="ad867-148">Azure Active Directory-GUID-ID</span><span class="sxs-lookup"><span data-stu-id="ad867-148">Azure Active Directory GUID ID</span></span>

### <a name="vpn-infrastructure"></a><span data-ttu-id="ad867-149">VPN-infrastruktur</span><span class="sxs-lookup"><span data-stu-id="ad867-149">VPN infrastructure</span></span>
<span data-ttu-id="ad867-150">Den här artikeln förutsätter att du har en fungerande VPN-infrastruktur med hjälp av Microsoft Windows Server 2016 på plats och att VPN-servern för närvarande inte har konfigurerats att vidarebefordra anslutningsförfrågningar till en RADIUS-server.</span><span class="sxs-lookup"><span data-stu-id="ad867-150">This article assumes that you have a working VPN infrastructure using Microsoft Windows Server 2016 in place and that the VPN server is currently not configured to forward connection requests to a RADIUS server.</span></span> <span data-ttu-id="ad867-151">Du konfigurerar VPN-infrastruktur för att använda en central RADIUS-server i den här guiden.</span><span class="sxs-lookup"><span data-stu-id="ad867-151">You will configure the VPN infrastructure to use a central RADIUS server in this guide.</span></span>

<span data-ttu-id="ad867-152">Om du inte har en fungerande infrastruktur på plats kan du snabbt skapa infrastrukturen genom att följa riktlinjerna i flera självstudier för VPN-installationen hittar du i Microsoft och tredje parters webbplatser.</span><span class="sxs-lookup"><span data-stu-id="ad867-152">If you do not have a working infrastructure in place, you can quickly create this infrastructure by following the guidance provided in numerous VPN setup tutorials you can find on the Microsoft and third-party sites.</span></span> 

### <a name="network-policy-and-access-services-nps-role"></a><span data-ttu-id="ad867-153">Nätverksprincip och Access Services (NPS)</span><span class="sxs-lookup"><span data-stu-id="ad867-153">Network Policy and Access Services (NPS) role</span></span>

<span data-ttu-id="ad867-154">NPS-rolltjänsten tillhandahåller RADIUS-server och klient.</span><span class="sxs-lookup"><span data-stu-id="ad867-154">The NPS role service provides the RADIUS server and client functionality.</span></span> <span data-ttu-id="ad867-155">Den här artikeln förutsätter att du har installerat NPS-rollen på en medlemsserver eller domänkontrollant i din miljö.</span><span class="sxs-lookup"><span data-stu-id="ad867-155">This article assumes you have installed the NPS role on a member server or domain controller in your environment.</span></span> <span data-ttu-id="ad867-156">För en VPN-konfiguration i den här guiden konfigurerar du RADIUS.</span><span class="sxs-lookup"><span data-stu-id="ad867-156">You will configure RADIUS for a VPN configuration in this guide.</span></span> <span data-ttu-id="ad867-157">Installera NPS-rollen på en server _andra_ än VPN-servern.</span><span class="sxs-lookup"><span data-stu-id="ad867-157">Install the NPS role on a server _other_ than your VPN server.</span></span>

<span data-ttu-id="ad867-158">Information om hur du installerar NPS-rollen tjänst i Windows Server 2012 eller högre, se [installera en NAP-hälsoprincipserver](https://technet.microsoft.com/library/dd296890.aspx).</span><span class="sxs-lookup"><span data-stu-id="ad867-158">For information on installing the NPS role service Windows Server 2012 or higher, see [Install a NAP Health Policy Server](https://technet.microsoft.com/library/dd296890.aspx).</span></span> <span data-ttu-id="ad867-159">Princip för NAP (Network Access) är föråldrad i Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="ad867-159">Network Access Policy (NAP) is deprecated in Windows Server 2016.</span></span> <span data-ttu-id="ad867-160">En beskrivning av bästa praxis för NPS, inklusive rekommendationen att installera NPS på en domänkontrollant finns [bästa praxis för NPS](https://technet.microsoft.com/library/cc771746).</span><span class="sxs-lookup"><span data-stu-id="ad867-160">For a description of best practices for NPS, including the recommendation to install NPS on a domain controller, see [Best Practices for NPS](https://technet.microsoft.com/library/cc771746).</span></span>

### <a name="licenses"></a><span data-ttu-id="ad867-161">Licenser</span><span class="sxs-lookup"><span data-stu-id="ad867-161">Licenses</span></span>

<span data-ttu-id="ad867-162">Krävs är en licens för Azure MFA som finns tillgänglig via en Azure AD Premium, Enterprise Mobility plus Security (EMS) eller en MFA-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="ad867-162">Required is a license for Azure MFA, which is available through an Azure AD Premium, Enterprise Mobility plus Security (EMS), or an MFA subscription.</span></span> <span data-ttu-id="ad867-163">Mer information finns i [hur du hämtar Azure Multi-Factor Authentication](multi-factor-authentication-versions-plans.md).</span><span class="sxs-lookup"><span data-stu-id="ad867-163">For more information, see [How to get Azure Multi-Factor Authentication](multi-factor-authentication-versions-plans.md).</span></span> <span data-ttu-id="ad867-164">I testsyfte kan använda du en utvärderingsprenumeration.</span><span class="sxs-lookup"><span data-stu-id="ad867-164">For testing purposes, you can use a trial subscription.</span></span>

### <a name="software"></a><span data-ttu-id="ad867-165">Programvara</span><span class="sxs-lookup"><span data-stu-id="ad867-165">Software</span></span>

<span data-ttu-id="ad867-166">NPS-tillägget kräver Windows Server 2008 R2 SP1 eller senare med NPS-rolltjänsten installerad.</span><span class="sxs-lookup"><span data-stu-id="ad867-166">The NPS extension requires Windows Server 2008 R2 SP1 or above with the NPS role service installed.</span></span> <span data-ttu-id="ad867-167">Alla steg i den här guiden utfördes med Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="ad867-167">All the steps in this guide were performed using Windows Server 2016.</span></span>

### <a name="libraries"></a><span data-ttu-id="ad867-168">Bibliotek</span><span class="sxs-lookup"><span data-stu-id="ad867-168">Libraries</span></span>

<span data-ttu-id="ad867-169">Följande två bibliotek krävs:</span><span class="sxs-lookup"><span data-stu-id="ad867-169">The following two libraries are required:</span></span>

* [<span data-ttu-id="ad867-170">Visual C++ Redistributable-paket för Visual Studio 2013 (X64)</span><span class="sxs-lookup"><span data-stu-id="ad867-170">Visual C++ Redistributable Packages for Visual Studio 2013 (X64)</span></span>](https://www.microsoft.com/download/details.aspx?id=40784)
* <span data-ttu-id="ad867-171">_Microsoft Azure Active Directory-modulen för Windows PowerShell version 1.1.166.0_ eller högre.</span><span class="sxs-lookup"><span data-stu-id="ad867-171">_Microsoft Azure Active Directory Module for Windows PowerShell version 1.1.166.0_ or higher.</span></span> <span data-ttu-id="ad867-172">Den senaste versionen och installationsanvisningar finns i [Microsoft Azure Active Directory PowerShell-modulen släpper versionshistorik](https://social.technet.microsoft.com/wiki/contents/articles/28552.microsoft-azure-active-directory-powershell-module-version-release-history.aspx).</span><span class="sxs-lookup"><span data-stu-id="ad867-172">For the latest release and installation instructions, see [Microsoft Azure Active Directory PowerShell Module Version Release History](https://social.technet.microsoft.com/wiki/contents/articles/28552.microsoft-azure-active-directory-powershell-module-version-release-history.aspx).</span></span>

<span data-ttu-id="ad867-173">Dessa bibliotek paketeras inte med NPS tillägget installationsfilerna (version 0.9.1.2), trots befintliga dokumentationen som säger annorlunda.</span><span class="sxs-lookup"><span data-stu-id="ad867-173">These libraries are not packaged with the NPS extension setup files (version 0.9.1.2), despite existing documentation that states otherwise.</span></span> <span data-ttu-id="ad867-174">Som minimum måste du installera Visual C++-paket för Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="ad867-174">At a minimum, you must install the Visual C++ Redistributable Packages for Visual Studio 2013.</span></span> <span data-ttu-id="ad867-175">Den Microsoft Azure Active Directory-modulen för Windows PowerShell installeras, om det inte redan finns, via ett konfigurationsskript som du körs som en del av installationen.</span><span class="sxs-lookup"><span data-stu-id="ad867-175">The Microsoft Azure Active Directory Module for Windows PowerShell is installed, if it is not already present, through a configuration script you run as part of the setup process.</span></span> <span data-ttu-id="ad867-176">Det finns inget behov av att installera den här modulen i förväg om den inte redan är installerad.</span><span class="sxs-lookup"><span data-stu-id="ad867-176">There is no need to install this module ahead of time if it is not already installed.</span></span>

### <a name="azure-active-directory-synched-with-on-premises-active-directory"></a><span data-ttu-id="ad867-177">Azure Active Directory är synkroniserade med lokala Active Directory</span><span class="sxs-lookup"><span data-stu-id="ad867-177">Azure Active Directory synched with on-premises Active Directory</span></span> 

<span data-ttu-id="ad867-178">Om du vill använda NPS-tillägget lokala användare synkroniseras med Azure Active Directory och aktiverats för Multifaktorautentisering.</span><span class="sxs-lookup"><span data-stu-id="ad867-178">To use the NPS extension, on-premises users must be synced with Azure Active Directory and enabled for Multi-Factor Authentication.</span></span> <span data-ttu-id="ad867-179">Den här guiden förutsätter att lokala användare är synkroniserade med Azure Active Directory med AD Connect.</span><span class="sxs-lookup"><span data-stu-id="ad867-179">This guide assumes that on-premises users are synched with Azure Active Directory using AD Connect.</span></span> <span data-ttu-id="ad867-180">Instruktioner för att aktivera användare för MFA finns nedan.</span><span class="sxs-lookup"><span data-stu-id="ad867-180">Instructions for enabling users for MFA are provided below.</span></span>
<span data-ttu-id="ad867-181">Information om Azure AD connect, se [integrera dina lokala kataloger med Azure Active Directory](../active-directory/connect/active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="ad867-181">For information on Azure AD connect, see [Integrate your on-premises directories with Azure Active Directory](../active-directory/connect/active-directory-aadconnect.md).</span></span> 

### <a name="azure-active-directory-guid-id"></a><span data-ttu-id="ad867-182">Azure Active Directory-GUID-ID</span><span class="sxs-lookup"><span data-stu-id="ad867-182">Azure Active Directory GUID ID</span></span> 
<span data-ttu-id="ad867-183">Om du vill installera NPS som du behöver veta GUID för Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="ad867-183">To install the NPS, you need to know the GUID of the Azure Active Directory.</span></span> <span data-ttu-id="ad867-184">Instruktioner för att hitta GUID för Azure Active Directory finns i nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="ad867-184">Instructions for finding the GUID of the Azure Active Directory are provided in the next section.</span></span>

## <a name="configure-radius-for-vpn-connections"></a><span data-ttu-id="ad867-185">Konfigurera RADIUS för VPN-anslutningar</span><span class="sxs-lookup"><span data-stu-id="ad867-185">Configure RADIUS for VPN connections</span></span>

<span data-ttu-id="ad867-186">Om du har installerat rollen NPS-server på en medlemsserver, måste du konfigurera för att autentisera och auktorisera VPN-klienten att begäran VPN-anslutningar.</span><span class="sxs-lookup"><span data-stu-id="ad867-186">If you have installed the NPS server role on a member server, you need to configure to authenticate and authorize VPN client that request VPN connections.</span></span> 

<span data-ttu-id="ad867-187">Det här avsnittet förutsätter att du har installerat NPS-rollen men inte har konfigurerat den för användning i din infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="ad867-187">This section assumes that you have installed the Network Policy Server role but have not configured it for use in your infrastructure.</span></span>

>[!NOTE]
><span data-ttu-id="ad867-188">Om du redan har en fungerande VPN-server som använder en centraliserad RADIUS-server för autentisering kan du hoppa över det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="ad867-188">If you already have a working VPN server that uses a centralized RADIUS server for authentication, you can skip this section.</span></span>
>

### <a name="register-server-in-active-directory"></a><span data-ttu-id="ad867-189">Registrera servern i Active Directory</span><span class="sxs-lookup"><span data-stu-id="ad867-189">Register Server in Active Directory</span></span>
<span data-ttu-id="ad867-190">NPS-servern måste registreras i Active Directory för att fungera i det här scenariot.</span><span class="sxs-lookup"><span data-stu-id="ad867-190">To function properly in this scenario, the NPS server needs to be registered in Active Directory.</span></span>

1. <span data-ttu-id="ad867-191">Öppna Serverhanteraren.</span><span class="sxs-lookup"><span data-stu-id="ad867-191">Open Server Manager.</span></span>
2. <span data-ttu-id="ad867-192">I Serverhanteraren klickar du på **verktyg**, och klicka sedan på **Network Policy Server**.</span><span class="sxs-lookup"><span data-stu-id="ad867-192">In Server Manager, click **Tools**, and then click **Network Policy Server**.</span></span> 
3. <span data-ttu-id="ad867-193">I NPS-konsolträdet högerklickar du på **NPS (lokal)**, och klicka sedan på **registrera servern i Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="ad867-193">In the Network Policy Server console, right-click **NPS (Local)**, and then click **Register server in Active Directory**.</span></span> <span data-ttu-id="ad867-194">Klicka på **OK** två gånger.</span><span class="sxs-lookup"><span data-stu-id="ad867-194">Click **OK** two times.</span></span>

 ![Nätverksprincipserver](./media/nps-extension-vpn/image2.png)

4. <span data-ttu-id="ad867-196">Lämna konsolen öppen för nästa procedur.</span><span class="sxs-lookup"><span data-stu-id="ad867-196">Leave the console open for the next procedure.</span></span>

### <a name="use-wizard-to-configure-radius-server"></a><span data-ttu-id="ad867-197">Använd guiden för att konfigurera RADIUS-server</span><span class="sxs-lookup"><span data-stu-id="ad867-197">Use wizard to configure RADIUS server</span></span>
<span data-ttu-id="ad867-198">Du kan använda en standard (guidebaserad) eller avancerade konfigurationsalternativ för att konfigurera RADIUS-servern.</span><span class="sxs-lookup"><span data-stu-id="ad867-198">You can use a standard (wizard-based) or advanced configuration option to configure the RADIUS server.</span></span> <span data-ttu-id="ad867-199">Det här avsnittet förutsätter användningen av alternativet guidebaserad standardkonfiguration.</span><span class="sxs-lookup"><span data-stu-id="ad867-199">This section assumes the use of the wizard-based standard configuration option.</span></span>

1. <span data-ttu-id="ad867-200">NPS-konsolen, klicka på **NPS (lokal)**.</span><span class="sxs-lookup"><span data-stu-id="ad867-200">In the Network Policy Server console, click **NPS (Local)**.</span></span>
2. <span data-ttu-id="ad867-201">Välj under standardkonfiguration **RADIUS-Server för fjärranslutningar eller VPN-anslutningar**, och klicka sedan på **konfigurera VPN eller fjärranslutning**.</span><span class="sxs-lookup"><span data-stu-id="ad867-201">Under Standard Configuration, select **RADIUS Server for Dial-Up or VPN Connections**, and then click **Configure VPN or Dial-Up**.</span></span>

 ![Konfigurera VPN](./media/nps-extension-vpn/image3.png)

3. <span data-ttu-id="ad867-203">På sidan Välj fjärranslutning eller virtuellt privat nätverk anslutningar typ väljer **VPN-anslutningar**, och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="ad867-203">On the Select Dial-up or Virtual Private Network Connections Type page, select **Virtual Private Network Connections**, and click **Next**.</span></span>

 ![Virtuellt privat nätverk](./media/nps-extension-vpn/image4.png)

4. <span data-ttu-id="ad867-205">På sidan Ange fjärranslutning eller VPN-servern **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="ad867-205">On the Specify Dial-Up or VPN Server page, click **Add**.</span></span>
5. <span data-ttu-id="ad867-206">I den **ny RADIUS-klient** dialogrutan, ange ett eget namn, anger du namnet eller IP-adressen för VPN-servern och ange ett delade hemliga lösenord.</span><span class="sxs-lookup"><span data-stu-id="ad867-206">In the **New RADIUS client** dialog box, provide a friendly name, enter the resolvable name or IP address of the VPN server, and enter a shared secret password.</span></span> <span data-ttu-id="ad867-207">Skapa den här delade hemliga lösenordet långa och komplexa.</span><span class="sxs-lookup"><span data-stu-id="ad867-207">Make this shared secret password long and complex.</span></span> <span data-ttu-id="ad867-208">Anteckna det här lösenordet som du behöver för stegen i nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="ad867-208">Record this password, as you need it for steps in the next section.</span></span>

 ![Ny RADIUS-klient](./media/nps-extension-vpn/image5.png)

6. <span data-ttu-id="ad867-210">Klicka på **OK**, och sedan **nästa**.</span><span class="sxs-lookup"><span data-stu-id="ad867-210">Click **OK**, and then **Next**.</span></span>
7. <span data-ttu-id="ad867-211">På den **konfigurera autentiseringsmetoder** godkänner standardvalet (Microsoft krypterad autentisering version 2 (MS-CHAPv2) eller välj ett annat alternativ och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="ad867-211">On the **Configure Authentication Methods** page, accept the default selection (Microsoft Encrypted Authentication version 2 (MS-CHAPv2) or choose another option, and click **Next**.</span></span>

  >[!NOTE]
  ><span data-ttu-id="ad867-212">Om du konfigurerar Extensible Authentication Protocol (EAP), måste du använda MS-CHAPv2 eller PEAP.</span><span class="sxs-lookup"><span data-stu-id="ad867-212">If you configure Extensible Authentication Protocol (EAP), you must use either MS CHAPv2 or PEAP.</span></span> <span data-ttu-id="ad867-213">Inga andra EAP stöds.</span><span class="sxs-lookup"><span data-stu-id="ad867-213">No other EAP is supported.</span></span>
 
8. <span data-ttu-id="ad867-214">På sidan Ange användargrupper **Lägg till** och välj en lämplig grupp, om sådan finns.</span><span class="sxs-lookup"><span data-stu-id="ad867-214">On the Specify User Groups page, click **Add** and select an appropriate group, if one exists.</span></span> <span data-ttu-id="ad867-215">Annars lämna alternativet tomt om du vill bevilja åtkomst till alla användare.</span><span class="sxs-lookup"><span data-stu-id="ad867-215">Otherwise, leave the selection blank to grant access to all users.</span></span>

 ![Ange användargrupper](./media/nps-extension-vpn/image7.png)

9. <span data-ttu-id="ad867-217">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="ad867-217">Click **Next**.</span></span>
10. <span data-ttu-id="ad867-218">På sidan Ange IP-filter **nästa**.</span><span class="sxs-lookup"><span data-stu-id="ad867-218">On the Specify IP Filters page, click **Next**.</span></span>
11. <span data-ttu-id="ad867-219">På sidan Ange krypteringsinställningar acceptera standardinställningarna och klickar på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="ad867-219">On the Specify Encryption Settings page, accept the default settings, and click **Next**.</span></span>

 ![Ange kryptering](./media/nps-extension-vpn/image8.png)

12. <span data-ttu-id="ad867-221">På Ange ett resursnamn är tomt, godkänner du standardinställningen och på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="ad867-221">On the Specify a Realm Name, leave the name blank, accept the default setting, and click **Next**.</span></span>

 ![Ange ett resursnamn](./media/nps-extension-vpn/image9.png)

13. <span data-ttu-id="ad867-223">Klicka på Slutför nya fjärr- eller VPN-anslutningar och RADIUS-klienter sidan **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="ad867-223">On the Completing New Dial-up or Virtual Private Network Connections and RADIUS clients page, click **Finish**.</span></span>

 ![Slutföra anslutningar](./media/nps-extension-vpn/image10.png)

### <a name="verify-radius-configuration"></a><span data-ttu-id="ad867-225">Kontrollera RADIUS-konfiguration</span><span class="sxs-lookup"><span data-stu-id="ad867-225">Verify RADIUS configuration</span></span>
<span data-ttu-id="ad867-226">Det här avsnittet beskrivs den konfiguration som du skapat med hjälp av guiden.</span><span class="sxs-lookup"><span data-stu-id="ad867-226">This section details the configuration you created using the wizard.</span></span>

1. <span data-ttu-id="ad867-227">Expandera RADIUS-klienter på NPS-servern i konsolen NPS (lokal) och välj **RADIUS-klienter**.</span><span class="sxs-lookup"><span data-stu-id="ad867-227">On the NPS server, in the NPS (Local) console, expand RADIUS Clients, and select **RADIUS Clients**.</span></span>
2. <span data-ttu-id="ad867-228">I informationsfönstret högerklickar du på RADIUS-klienten som du skapat med hjälp av guiden och klicka på **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="ad867-228">In the details pane, right-click the RADIUS client you created using wizard, and click **Properties**.</span></span> <span data-ttu-id="ad867-229">Egenskaperna för RADIUS-klienten (VPN-server) ska vara samma som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="ad867-229">The properties for your RADIUS client (the VPN server) should be similar to those shown below.</span></span>

 ![VPN-egenskaper](./media/nps-extension-vpn/image11.png)

3. <span data-ttu-id="ad867-231">Klicka på **Avbryt**.</span><span class="sxs-lookup"><span data-stu-id="ad867-231">Click **Cancel**.</span></span>
4. <span data-ttu-id="ad867-232">NPS-servern i konsolen NPS (lokal) utöka **principer**, och välj **principer för anslutningsbegäran**.</span><span class="sxs-lookup"><span data-stu-id="ad867-232">On the NPS server, in the NPS (Local) console, expand **Policies**, and select **Connection Request Policies**.</span></span> <span data-ttu-id="ad867-233">Du bör se den princip för VPN-anslutningar som liknar bilden nedan.</span><span class="sxs-lookup"><span data-stu-id="ad867-233">You should see the VPN Connections policy that resembles the image below.</span></span>

 ![Anslutningsbegäranden](./media/nps-extension-vpn/image12.png)

5. <span data-ttu-id="ad867-235">Enligt principer, Välj **nätverksprinciper**.</span><span class="sxs-lookup"><span data-stu-id="ad867-235">Under Policies, select **Network Policies**.</span></span> <span data-ttu-id="ad867-236">Du bör en princip för virtuella privata nätverk (VPN) anslutningar som liknar bilden nedan.</span><span class="sxs-lookup"><span data-stu-id="ad867-236">You should a Virtual Private Network (VPN) Connections policy that resembles the image below.</span></span>

 ![Egenskaper för nätverk](./media/nps-extension-vpn/image13.png)

## <a name="configure-vpn-server-to-use-radius-authentication"></a><span data-ttu-id="ad867-238">Konfigurera VPN-servern för RADIUS-autentisering</span><span class="sxs-lookup"><span data-stu-id="ad867-238">Configure VPN Server to use RADIUS authentication</span></span>
<span data-ttu-id="ad867-239">I det här avsnittet kan du konfigurera VPN-servern för RADIUS-autentisering.</span><span class="sxs-lookup"><span data-stu-id="ad867-239">In this section, you configure the VPN server to use RADIUS authentication.</span></span> <span data-ttu-id="ad867-240">Det här avsnittet förutsätter att du har en fungerande konfiguration för VPN-server men inte har konfigurerat VPN-servern för RADIUS-autentisering.</span><span class="sxs-lookup"><span data-stu-id="ad867-240">This section assumes that you have a working configuration of VPN server but have not configured the VPN server to use RADIUS authentication.</span></span> <span data-ttu-id="ad867-241">När du konfigurerar VPN-servern måste bekräfta du att konfigurationen fungerar som förväntat.</span><span class="sxs-lookup"><span data-stu-id="ad867-241">After configuring the VPN server, you confirm that your configuration is working as expected.</span></span>

>[!NOTE]
><span data-ttu-id="ad867-242">Om du redan har en fungerande VPN-serverkonfiguration som använder RADIUS-autentisering kan du hoppa över det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="ad867-242">If you already have a working VPN server configuration that uses RADIUS authentication, you can skip this section.</span></span>
>

### <a name="configure-authentication-provider"></a><span data-ttu-id="ad867-243">Konfigurera autentiseringsprovider</span><span class="sxs-lookup"><span data-stu-id="ad867-243">Configure authentication provider</span></span>
1. <span data-ttu-id="ad867-244">Öppna Serverhanteraren på VPN-servern.</span><span class="sxs-lookup"><span data-stu-id="ad867-244">On the VPN server, open Server Manager.</span></span>
2. <span data-ttu-id="ad867-245">I Serverhanteraren klickar du på **verktyg**, och sedan **Routning och fjärråtkomst**.</span><span class="sxs-lookup"><span data-stu-id="ad867-245">In Server Manager, click **Tools**, and then **Routing and Remote Access**.</span></span>
3. <span data-ttu-id="ad867-246">Högerklicka i konsolen Routning och fjärråtkomst  **\[servernamn\] (lokal)**, och klicka sedan på **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="ad867-246">In the Routing and Remote Access console, right-click **\[Server Name\] (local)**, and then click **Properties**.</span></span>

 ![Routning och fjärråtkomst](./media/nps-extension-vpn/image14.png)
 
4. <span data-ttu-id="ad867-248">I den **[servernamn} (lokal) egenskaper** dialogrutan klickar du på den **säkerhet** fliken.</span><span class="sxs-lookup"><span data-stu-id="ad867-248">In the **[Server Name} (local) Properties** dialog box, click the **Security** tab.</span></span> 
5. <span data-ttu-id="ad867-249">På den **säkerhet** under autentiseringsprovider fliken klickar du på **RADIUS-autentisering**, och sedan **konfigurera**.</span><span class="sxs-lookup"><span data-stu-id="ad867-249">On the **Security** tab, under Authentication provider, click **RADIUS Authentication**, and then **Configure**.</span></span>

 ![RADIUS-autentisering](./media/nps-extension-vpn/image15.png)
 
6. <span data-ttu-id="ad867-251">I dialogrutan RADIUS-autentisering klickar du på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="ad867-251">In the RADIUS Authentication dialog box, click **Add**.</span></span>
7. <span data-ttu-id="ad867-252">Lägg till namnet eller IP-adressen för RADIUS-server som du konfigurerade i föregående avsnitt i Lägg till RADIUS-servern i servernamn.</span><span class="sxs-lookup"><span data-stu-id="ad867-252">In the Add RADIUS Server, in Server name, add the name or the IP address of the RADIUS server you configured in the previous section.</span></span>
8. <span data-ttu-id="ad867-253">Klicka på delad hemlighet **ändra** och lägga till delade hemliga lösenordet du skapade och registrerats tidigare.</span><span class="sxs-lookup"><span data-stu-id="ad867-253">In Shared secret, click **Change** and add the shared secret password you created and recorded earlier.</span></span>
9. <span data-ttu-id="ad867-254">I timeout (sekunder), ändrar du värdet till ett värde mellan **30** och **60**.</span><span class="sxs-lookup"><span data-stu-id="ad867-254">In Time-out (seconds), change the value to a value between **30** and **60**.</span></span> <span data-ttu-id="ad867-255">Detta är nödvändigt att tillräckligt med tid att slutföra ytterligare autentiseringsfaktor.</span><span class="sxs-lookup"><span data-stu-id="ad867-255">This is necessary to allow enough time to complete the second authentication factor.</span></span>
 
 ![Lägg till RADIUS-Server](./media/nps-extension-vpn/image16.png)
 
10. <span data-ttu-id="ad867-257">Klicka på **OK** tills du är klar stänger du alla dialogrutor.</span><span class="sxs-lookup"><span data-stu-id="ad867-257">Click **OK** until you complete closing all dialog boxes.</span></span>

### <a name="test-vpn-connectivity"></a><span data-ttu-id="ad867-258">Testa en VPN-anslutning</span><span class="sxs-lookup"><span data-stu-id="ad867-258">Test VPN connectivity</span></span>
<span data-ttu-id="ad867-259">I det här avsnittet kan bekräfta du att VPN-klienten autentiseras och auktoriseras RADIUS-servern när du försöker ansluta till VPN-virtuell port.</span><span class="sxs-lookup"><span data-stu-id="ad867-259">In this section, you confirm that the VPN client is authenticated and authorized by the RADIUS server when you attempt to connect to VPN virtual port.</span></span> <span data-ttu-id="ad867-260">Det här avsnittet förutsätter att du använder Windows 10 som en VPN-klient.</span><span class="sxs-lookup"><span data-stu-id="ad867-260">This section assumes you are using Windows 10 as a VPN client.</span></span> 

>[!NOTE]
><span data-ttu-id="ad867-261">Om du redan har konfigurerat ett VPN-klienten för att ansluta till VPN-servern och har sparat inställningarna kan du hoppa över de steg som rör konfigurera och spara ett objekt för VPN-anslutning.</span><span class="sxs-lookup"><span data-stu-id="ad867-261">If you already configured a VPN client to connect to the VPN server and have saved the settings, you can skip the steps related to configuring and saving a VPN connection object.</span></span>
>

1. <span data-ttu-id="ad867-262">Klicka på din VPN-klientdatorn **starta**, och sedan **inställningar** (kugghjulsikon).</span><span class="sxs-lookup"><span data-stu-id="ad867-262">On your VPN client computer, click **Start**, and then **Settings** (gear icon).</span></span>
2. <span data-ttu-id="ad867-263">I fönstret Inställningar, klickar du på **nätverk och Internet**.</span><span class="sxs-lookup"><span data-stu-id="ad867-263">In Window Settings, click **Network & Internet**.</span></span>
3. <span data-ttu-id="ad867-264">Klicka på **VPN**.</span><span class="sxs-lookup"><span data-stu-id="ad867-264">Click **VPN**.</span></span>
4. <span data-ttu-id="ad867-265">Klicka på **lägga till en VPN-anslutning**.</span><span class="sxs-lookup"><span data-stu-id="ad867-265">Click **Add a VPN connection**.</span></span>
5. <span data-ttu-id="ad867-266">I en VPN-anslutning, ange Windows (inbyggt) som VPN-provider och sedan Fyll i de återstående fälten efter behov, och sedan **spara**.</span><span class="sxs-lookup"><span data-stu-id="ad867-266">In Add a VPN connection, specify Windows (built-in) as the VPN provider, then complete the remaining fields, as appropriate, and click **Save**.</span></span> 

 ![Lägg till VPN-anslutning](./media/nps-extension-vpn/image17.png)
 
6. <span data-ttu-id="ad867-268">Öppna den **nätverks- och delningscenter** på Kontrollpanelen.</span><span class="sxs-lookup"><span data-stu-id="ad867-268">Open the **Network and Sharing Center** in Control Panel.</span></span>
7. <span data-ttu-id="ad867-269">Klicka på **ändra Kortinställningar**.</span><span class="sxs-lookup"><span data-stu-id="ad867-269">Click **Change adapter settings**.</span></span>

 ![Ändra inställningar för nätverkskort](./media/nps-extension-vpn/image18.png)

8. <span data-ttu-id="ad867-271">Högerklicka på VPN-nätverksanslutningen och klicka på Egenskaper.</span><span class="sxs-lookup"><span data-stu-id="ad867-271">Right-click the VPN network connection, and click Properties.</span></span> 

 ![Egenskaper för VPN-nätverk](./media/nps-extension-vpn/image19.png)

9. <span data-ttu-id="ad867-273">I egenskapsdialogrutan för VPN klickar du på den **säkerhet** fliken.</span><span class="sxs-lookup"><span data-stu-id="ad867-273">In the VPN properties dialog box, click the **Security** tab.</span></span> 
10. <span data-ttu-id="ad867-274">På fliken säkerhet, kontrollera att endast **Microsoft CHAP Version 2 (MS-CHAP v2)** är markerad och klicka på OK.</span><span class="sxs-lookup"><span data-stu-id="ad867-274">On the Security tab, ensure that only **Microsoft CHAP Version 2 (MS-CHAP v2)** is selected, and click OK.</span></span>

 ![Tillåt protokoll](./media/nps-extension-vpn/image20.png)

11. <span data-ttu-id="ad867-276">Högerklicka på VPN-anslutningen och på **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="ad867-276">Right-click the VPN connection, and click **Connect**.</span></span>
12. <span data-ttu-id="ad867-277">På sidan Inställningar **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="ad867-277">On the Settings page, click **Connect**.</span></span>

<span data-ttu-id="ad867-278">En lyckad anslutning visas i säkerhetsloggen på RADIUS-server som händelse-ID 6272 enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="ad867-278">A successful connection appears in the Security log on the RADIUS server as Event ID 6272, as shown below.</span></span>

 ![Egenskaper för händelse](./media/nps-extension-vpn/image21.png)

## <a name="troubleshoot-guide"></a><span data-ttu-id="ad867-280">Felsöka programguiden</span><span class="sxs-lookup"><span data-stu-id="ad867-280">Troubleshoot Guide</span></span>
<span data-ttu-id="ad867-281">Anta att VPN-konfiguration fungerade innan du har konfigurerat VPN-servern för att använda en centraliserad RADIUS-server för autentisering och auktorisering.</span><span class="sxs-lookup"><span data-stu-id="ad867-281">Assume that your VPN configuration was working before you configured the VPN server to use a centralized RADIUS server for authentication and authorization.</span></span> <span data-ttu-id="ad867-282">I det här fallet är det troligt att problemet kan bero på en felaktig konfiguration av RADIUS-servern eller användning av ett ogiltigt användarnamn eller lösenord.</span><span class="sxs-lookup"><span data-stu-id="ad867-282">In this case, it is likely that the issue may be caused by a misconfiguration of the RADIUS Server or the use of an invalid username or password.</span></span> <span data-ttu-id="ad867-283">Till exempel om du använder alternativt UPN-suffix i användarnamnet inloggningsförsöket misslyckas (du måste använda samma kontonamn för bästa resultat).</span><span class="sxs-lookup"><span data-stu-id="ad867-283">For example, if you use the alternate UPN suffix in the username, the login attempt might fail (you should use the same Account name for best results).</span></span> 

<span data-ttu-id="ad867-284">Felsökning av problem med dessa är användbar om du vill starta för att granska säkerhetshändelser på RADIUS-servern.</span><span class="sxs-lookup"><span data-stu-id="ad867-284">To troubleshoot these issues, an ideal place to start is to examine the Security event logs on the RADIUS server.</span></span> <span data-ttu-id="ad867-285">För att spara tid på att söka efter händelser, kan du använda den rollbaserade nätverksprincip och åtkomst till servern anpassad vy i Loggboken, som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="ad867-285">To save time searching for events, you can use the role-based Network Policy and Access Server custom view in Event Viewer, as show below.</span></span> <span data-ttu-id="ad867-286">Händelse-ID 6273 anger händelser där nätverksprincipservern nekas åtkomst till en användare.</span><span class="sxs-lookup"><span data-stu-id="ad867-286">Event ID 6273 indicates events where the Network Policy Server denied access to a user.</span></span> 

 ![Loggboken](./media/nps-extension-vpn/image22.png)
 
## <a name="configure-multi-factor-authentication"></a><span data-ttu-id="ad867-288">Konfigurera Multi-Factor Authentication</span><span class="sxs-lookup"><span data-stu-id="ad867-288">Configure Multi-Factor Authentication</span></span>
<span data-ttu-id="ad867-289">Det här avsnittet innehåller instruktioner för att aktivera användare för MFA och konfigurera konton för tvåstegsverifiering.</span><span class="sxs-lookup"><span data-stu-id="ad867-289">This section provides instructions for enabling users for MFA and for setting up accounts for two-step verification.</span></span> 

### <a name="enable-multi-factor-authentication"></a><span data-ttu-id="ad867-290">Aktivera multifaktorautentisering</span><span class="sxs-lookup"><span data-stu-id="ad867-290">Enable multi-factor authentication</span></span>
<span data-ttu-id="ad867-291">I det här avsnittet kan aktivera du Azure AD-konton för MFA.</span><span class="sxs-lookup"><span data-stu-id="ad867-291">In this section, you enable Azure AD accounts for MFA.</span></span> <span data-ttu-id="ad867-292">Använd den **klassiska portalen** användarna för MFA.</span><span class="sxs-lookup"><span data-stu-id="ad867-292">Use the **classic portal** to enable users for MFA.</span></span> 

1. <span data-ttu-id="ad867-293">Öppna en webbläsare och gå till [https://manage.windowsazure.com](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="ad867-293">Open a browser, and navigate to [https://manage.windowsazure.com](https://manage.windowsazure.com).</span></span> 
2. <span data-ttu-id="ad867-294">Logga in som administratör.</span><span class="sxs-lookup"><span data-stu-id="ad867-294">Log on as the administrator.</span></span>
3. <span data-ttu-id="ad867-295">I portalen i det vänstra navigeringsfönstret klickar du på **ACTIVE DIRECTORY**.</span><span class="sxs-lookup"><span data-stu-id="ad867-295">In the portal, in the left navigation, click **ACTIVE DIRECTORY**.</span></span>

 ![Standardkatalogen](./media/nps-extension-vpn/image23.png)

4. <span data-ttu-id="ad867-297">I kolumnen namn klickar du på **standardkatalog** (eller en annan katalog, vid behov).</span><span class="sxs-lookup"><span data-stu-id="ad867-297">In the NAME column, click **Default Directory** (or another directory, if appropriate).</span></span>
5. <span data-ttu-id="ad867-298">På sidan Snabbstart **konfigurera**.</span><span class="sxs-lookup"><span data-stu-id="ad867-298">On the Quick Start page, click **Configure**.</span></span>

 ![Konfigurera standard](./media/nps-extension-vpn/image24.png)

6. <span data-ttu-id="ad867-300">Rulla ned på sidan Konfigurera och klicka på under multifaktorautentisering **hantera tjänstinställningar**.</span><span class="sxs-lookup"><span data-stu-id="ad867-300">On the CONFIGURE page, scroll down and, in the multi-factor authentication section, click **Manage service settings**.</span></span>

 ![Hantera inställningar för MFA](./media/nps-extension-vpn/image25.png)
 
7. <span data-ttu-id="ad867-302">På sidan multifaktorautentisering granska standardinställningarna för tjänsten och klicka sedan på **användare**.</span><span class="sxs-lookup"><span data-stu-id="ad867-302">On the multi-factor authentication page, review the default service settings, and then click **Users**.</span></span> 

 ![MFA-användare](./media/nps-extension-vpn/image26.png)
 
8. <span data-ttu-id="ad867-304">På sidan användare väljer du de användare som du vill aktivera för MFA och klicka sedan på **aktivera**.</span><span class="sxs-lookup"><span data-stu-id="ad867-304">On the Users page, select the users that you wish to enable for MFA, and then click **Enable**.</span></span>

 ![Egenskaper](./media/nps-extension-vpn/image27.png)
 
9. <span data-ttu-id="ad867-306">När du uppmanas, klickar du på **aktivera multifaktorautentisering**.</span><span class="sxs-lookup"><span data-stu-id="ad867-306">When prompted, click **Enable multi-factor auth**.</span></span>

 ![Aktivera MFA](./media/nps-extension-vpn/image28.png)
 
10. <span data-ttu-id="ad867-308">Klicka på **Stäng**.</span><span class="sxs-lookup"><span data-stu-id="ad867-308">Click **Close**.</span></span> 
11. <span data-ttu-id="ad867-309">Uppdatera sidan.</span><span class="sxs-lookup"><span data-stu-id="ad867-309">Refresh the page.</span></span> <span data-ttu-id="ad867-310">MFA-statusen ändras till aktiverad.</span><span class="sxs-lookup"><span data-stu-id="ad867-310">The MFA status is changed to Enabled.</span></span>

<span data-ttu-id="ad867-311">Information om hur du aktiverar användare för Multifaktorautentisering finns i [komma igång med Azure Multi-Factor Authentication i molnet](multi-factor-authentication-get-started-cloud.md).</span><span class="sxs-lookup"><span data-stu-id="ad867-311">For information on how to enable users for Multi-Factor Authentication, see [Getting started with Azure Multi-Factor Authentication in the cloud](multi-factor-authentication-get-started-cloud.md).</span></span> 

### <a name="configure-accounts-for-two-step-verification"></a><span data-ttu-id="ad867-312">Konfigurera konton för tvåstegsverifiering</span><span class="sxs-lookup"><span data-stu-id="ad867-312">Configure accounts for two-step verification</span></span>
<span data-ttu-id="ad867-313">När ett konto har aktiverats för MFA, kan användare inte logga in till resurser som omfattas av principen MFA förrän de har konfigurerat en betrodd enhet ska användas för andra autentiseringsfaktor använt tvåstegsverifiering.</span><span class="sxs-lookup"><span data-stu-id="ad867-313">Once an account has been enabled for MFA, users are not able to sign in to resources governed by the MFA policy until they have successfully configured a trusted device to use for the second authentication factor having used two-step verification.</span></span>

<span data-ttu-id="ad867-314">I det här avsnittet kan konfigurera du en betrodd enhet för användning med tvåstegsverifiering.</span><span class="sxs-lookup"><span data-stu-id="ad867-314">In this section, you configure a trusted device for use with two-step verification.</span></span> <span data-ttu-id="ad867-315">Det finns flera alternativ att konfigurera dessa, inklusive följande:</span><span class="sxs-lookup"><span data-stu-id="ad867-315">There are several options available for you to configure these, including the following:</span></span>

* <span data-ttu-id="ad867-316">**Mobilapp**.</span><span class="sxs-lookup"><span data-stu-id="ad867-316">**Mobile app**.</span></span> <span data-ttu-id="ad867-317">Du installerar Microsoft Authenticator-appen på en Windows Phone, Android eller iOS-enhet.</span><span class="sxs-lookup"><span data-stu-id="ad867-317">You install the Microsoft Authenticator app on a Windows Phone, Android, or iOS device.</span></span> <span data-ttu-id="ad867-318">Beroende på din organisations principer kan du behöver använda appen i ett av två lägen: ta emot meddelanden för verifieringar (skickas ett meddelande till din enhet) eller Använd verifieringskoden (du måste ange en Verifieringskod som uppdateras var 30: e sekund).</span><span class="sxs-lookup"><span data-stu-id="ad867-318">Depending on your organization’s policies, you are required to use the app in one of two modes: Receive notifications for verifications (a notification is pushed to your device) or Use verification code (you are required to enter a verification code that updates every 30 seconds).</span></span> 
* <span data-ttu-id="ad867-319">**Mobiltelefon samtal eller textmeddelande**.</span><span class="sxs-lookup"><span data-stu-id="ad867-319">**Mobile phone call or text**.</span></span> <span data-ttu-id="ad867-320">Du kan antingen ta emot en automatisk telefonsamtal eller SMS.</span><span class="sxs-lookup"><span data-stu-id="ad867-320">You can either receive an automated phone call or text message.</span></span> <span data-ttu-id="ad867-321">Med alternativet telefonsamtal besvara samtalet och tryck på #-tangenten för att autentisera.</span><span class="sxs-lookup"><span data-stu-id="ad867-321">With the phone call option, you answer the call and press the # sign to authenticate.</span></span> <span data-ttu-id="ad867-322">Med alternativet text kan du svara på textmeddelandet eller ange verifieringskoden i inloggning-gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="ad867-322">With the text option, you can either reply to the text message or enter the verification code into the sign-in interface.</span></span>
* <span data-ttu-id="ad867-323">**Telefonsamtal till arbete**.</span><span class="sxs-lookup"><span data-stu-id="ad867-323">**Office phone call**.</span></span> <span data-ttu-id="ad867-324">Den här processen är densamma som beskrivs för automatisk telefonsamtal ovan.</span><span class="sxs-lookup"><span data-stu-id="ad867-324">This process is the same as that described for automated phone calls above.</span></span>

<span data-ttu-id="ad867-325">Följ dessa instruktioner för att skapa en enhet som använder mobilappen för att ta emot push-meddelanden för verifiering.</span><span class="sxs-lookup"><span data-stu-id="ad867-325">Follow these instructions for setting up a device to use the mobile app to receive push notification for verification.</span></span>

1. <span data-ttu-id="ad867-326">Logga in på [https://aka.ms/mfasetup](https://aka.ms/mfasetup) eller en plats som [https://portal.azure.com](https://portal.azure.com), där du krävs för att autentisera med dina inloggningsuppgifter för MFA-aktiverade.</span><span class="sxs-lookup"><span data-stu-id="ad867-326">Log on to [https://aka.ms/mfasetup](https://aka.ms/mfasetup) or any site, such as [https://portal.azure.com](https://portal.azure.com), where you required to authenticate using your MFA-enabled credentials.</span></span> 
2. <span data-ttu-id="ad867-327">När du loggar in med ditt användarnamn och lösenord kan visas du en skärm där du uppmanas att konfigurera kontot för ytterligare säkerhetsverifiering.</span><span class="sxs-lookup"><span data-stu-id="ad867-327">Upon signing in with your username and password, you are presented with a screen that prompts you to set up the account for additional security verification.</span></span>

 ![Ökad säkerhet](./media/nps-extension-vpn/image29.png)

3. <span data-ttu-id="ad867-329">Klicka på **ställa in nu**.</span><span class="sxs-lookup"><span data-stu-id="ad867-329">Click **Set it up now**.</span></span>
4. <span data-ttu-id="ad867-330">Välj en Kontakta typ (telefon för autentisering, arbetstelefon eller mobilapp) på sidan ytterligare säkerhet verifiering.</span><span class="sxs-lookup"><span data-stu-id="ad867-330">On the Additional security verification page, select a contact type (authentication phone, office phone, or mobile app).</span></span> <span data-ttu-id="ad867-331">Sedan väljer ett land eller region och välj en metod.</span><span class="sxs-lookup"><span data-stu-id="ad867-331">Then select a country or region, and select a method.</span></span> <span data-ttu-id="ad867-332">Metoden varierar beroende på typ av du väljer.</span><span class="sxs-lookup"><span data-stu-id="ad867-332">The method varies by contact type you select.</span></span> <span data-ttu-id="ad867-333">Om du väljer mobilappar kan du till exempel väljer om du vill ta emot meddelanden för verifiering eller använda en Verifieringskod.</span><span class="sxs-lookup"><span data-stu-id="ad867-333">For example, if you choose Mobile app, you can select whether to receive notifications for verification or to use a verification code.</span></span> <span data-ttu-id="ad867-334">De steg som följer förutsätter att du väljer **mobilappen** kontakta typen.</span><span class="sxs-lookup"><span data-stu-id="ad867-334">The steps that follow assume you choose **Mobile app** as the contact type.</span></span>

 ![Phone autentisering](./media/nps-extension-vpn/image30.png)

5. <span data-ttu-id="ad867-336">Välj mobilappar, klicka på **ta emot meddelanden för verifiering**, och sedan **konfigurera**.</span><span class="sxs-lookup"><span data-stu-id="ad867-336">Select Mobile app, click **Receive notifications for verification**, and then **Set up**.</span></span> 

 ![Verifiering av Mobilappar](./media/nps-extension-vpn/image31.png)
 
6. <span data-ttu-id="ad867-338">Om du inte redan gjort det installerar du authenticator-mobilappen på din enhet.</span><span class="sxs-lookup"><span data-stu-id="ad867-338">If you haven’t done so already, install the authenticator mobile app on your device.</span></span> 
7. <span data-ttu-id="ad867-339">Följ instruktionerna i mobilappen för att skanna in streckkoden som presenterades eller ange informationen manuellt och klickar sedan på **klar**.</span><span class="sxs-lookup"><span data-stu-id="ad867-339">Follow the instructions in the mobile app to scan the presented bar code or enter the information manually, and then click **Done**.</span></span>

 ![Konfigurera Mobilappen](./media/nps-extension-vpn/image32.png)

8. <span data-ttu-id="ad867-341">På sidan för verifiering av ytterligare säkerhet **kontakta mig** och svara på meddelanden som skickas till din enhet.</span><span class="sxs-lookup"><span data-stu-id="ad867-341">On the Additional security verification page, click **Contact me** and reply to notification sent to your device.</span></span>
9. <span data-ttu-id="ad867-342">På sidan för ytterligare säkerhet verifiering anger du ett kontaktnummer ifall du skulle förlora åtkomsten till mobilappen och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="ad867-342">On the Additional security verification page, enter a contact number in case you lose access to the mobile app, and click **Next**.</span></span>

 ![Mobiltelefonnummer](./media/nps-extension-vpn/image33.png)
 
10. <span data-ttu-id="ad867-344">Klicka på ytterligare säkerhetsverifiering **klar**.</span><span class="sxs-lookup"><span data-stu-id="ad867-344">On the Additional security verification, click **Done**.</span></span>

<span data-ttu-id="ad867-345">Enheten har nu konfigurerats för att tillhandahålla en andra metod för verifiering.</span><span class="sxs-lookup"><span data-stu-id="ad867-345">The device is now configured to provide a second method of verification.</span></span> <span data-ttu-id="ad867-346">Information om hur du konfigurerar konton för tvåstegsverifiering finns [Konfigurera mitt konto för tvåstegsverifiering](./end-user/multi-factor-authentication-end-user-first-time.md).</span><span class="sxs-lookup"><span data-stu-id="ad867-346">For information on setting up accounts for two-step verification, see [Set up my account for two-step verification](./end-user/multi-factor-authentication-end-user-first-time.md).</span></span>

## <a name="install-and-configure-nps-extension"></a><span data-ttu-id="ad867-347">Installera och konfigurera NPS-tillägget</span><span class="sxs-lookup"><span data-stu-id="ad867-347">Install and configure NPS extension</span></span>

<span data-ttu-id="ad867-348">Det här avsnittet innehåller instruktioner för att konfigurera VPN för att använda Azure MFA för klientautentisering med VPN-servern.</span><span class="sxs-lookup"><span data-stu-id="ad867-348">This section provides instructions for configuring VPN to use Azure MFA for client authentication with the VPN Server.</span></span>

<span data-ttu-id="ad867-349">När du installerar och konfigurerar tillägget NPS krävs alla RADIUS-baserad klientautentisering som bearbetas av den här servern att använda Azure MFA.</span><span class="sxs-lookup"><span data-stu-id="ad867-349">Once you install and configure the NPS extension, all RADIUS-based client authentication that is processed by this server is required to use Azure MFA.</span></span> <span data-ttu-id="ad867-350">Om inte alla VPN-användare är registrerade i Azure MFA, du kan ställa in en annan RADIUS-server för att autentisera användare som inte har konfigurerats för att använda Multifaktorautentisering.</span><span class="sxs-lookup"><span data-stu-id="ad867-350">If not all your VPN users are enrolled in Azure MFA, you can set up another RADIUS server to authenticate users who are not configured to use MFA.</span></span> <span data-ttu-id="ad867-351">Eller skapa en post i registret som användarna handlingar kan tillhandahålla en andra autentiseringsfaktor endast om de har registrerats i MFA.</span><span class="sxs-lookup"><span data-stu-id="ad867-351">Or you can create a registry entry that allows challenged users to provide a second authentication factor, only if they are enrolled in MFA.</span></span> 

<span data-ttu-id="ad867-352">Skapa ett nytt strängvärde med namnet _REQUIRE_USER_MATCH i HKLM\SOFTWARE\Microsoft\AzureMfa_, och ange värdet till TRUE eller FALSE.</span><span class="sxs-lookup"><span data-stu-id="ad867-352">Create a new string value named _REQUIRE_USER_MATCH in HKLM\SOFTWARE\Microsoft\AzureMfa_, and set the value to TRUE or FALSE.</span></span> 

 ![Kräv Användarmatchning](./media/nps-extension-vpn/image34.png)
 
<span data-ttu-id="ad867-354">Om värdet är SANT eller inte har angetts, regleras alla autentiseringsbegäranden av MFA-kontrollen.</span><span class="sxs-lookup"><span data-stu-id="ad867-354">If the value is set to TRUE or not set, all authentication requests are subject to an MFA challenge.</span></span> <span data-ttu-id="ad867-355">Om värdet anges till FALSE, utfärdas MFA utmaningar enbart till användare som är registrerade i MFA.</span><span class="sxs-lookup"><span data-stu-id="ad867-355">If the value is set to FALSE, MFA challenges are issued only to users that are enrolled in MFA.</span></span> <span data-ttu-id="ad867-356">Endast använda FALSKT vid testning eller i produktionsmiljöer under en tid för onboarding.</span><span class="sxs-lookup"><span data-stu-id="ad867-356">Only use the FALSE setting in testing or in production environments during an onboarding period.</span></span>

### <a name="acquire-azure-active-directory-guid-id"></a><span data-ttu-id="ad867-357">Hämta Azure Active Directory-GUID-ID</span><span class="sxs-lookup"><span data-stu-id="ad867-357">Acquire Azure Active Directory GUID ID</span></span>

<span data-ttu-id="ad867-358">Som en del av konfigurationen av NPS-tillägget måste du ange administratörsautentiseringsuppgifter och Azure Active Directory-ID för Azure AD-klienten.</span><span class="sxs-lookup"><span data-stu-id="ad867-358">As part of the configuration of the NPS extension, you need to supply admin credentials and the Azure Active Directory ID for your Azure AD tenant.</span></span> <span data-ttu-id="ad867-359">Stegen nedan visar hur du kan hämta klient-ID.</span><span class="sxs-lookup"><span data-stu-id="ad867-359">The steps below show you how to get the tenant ID.</span></span>

1. <span data-ttu-id="ad867-360">Logga in på Azure-portalen på [https://portal.azure.com](https://portal.azure.com) som global administratör för Azure-klient.</span><span class="sxs-lookup"><span data-stu-id="ad867-360">Sign in to the Azure portal at [https://portal.azure.com](https://portal.azure.com) as the global administrator of the Azure tenant.</span></span>
2. <span data-ttu-id="ad867-361">I det vänstra navigeringsfönstret klickar du på den **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="ad867-361">In the left navigation, click the **Azure Active Directory** icon.</span></span>
3. <span data-ttu-id="ad867-362">Klicka på **Egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="ad867-362">Click **Properties**.</span></span>
4. <span data-ttu-id="ad867-363">Katalog-ID till Urklipp och markera den **kopiera** ikon.</span><span class="sxs-lookup"><span data-stu-id="ad867-363">To copy your Directory ID to the clipboard, select the **Copy** icon.</span></span>
 
 ![Katalog-ID](./media/nps-extension-vpn/image35.png)

### <a name="install-the-nps-extension"></a><span data-ttu-id="ad867-365">Installera NPS-tillägg</span><span class="sxs-lookup"><span data-stu-id="ad867-365">Install the NPS extension</span></span>
<span data-ttu-id="ad867-366">NPS-tillägget måste vara installerad på en server som har serverrollen för nätverksprincip och Access Services (NPS) installerad och som fungerar som RADIUS-server i din design.</span><span class="sxs-lookup"><span data-stu-id="ad867-366">The NPS extension needs to be installed on a server that has the Network Policy and Access Services (NPS) role installed and that functions as the RADIUS server in your design.</span></span> <span data-ttu-id="ad867-367">Installera inte tillägget NPS på fjärrservern skrivbordet.</span><span class="sxs-lookup"><span data-stu-id="ad867-367">Don't install the NPS extension on your Remote Desktop Server.</span></span>

1. <span data-ttu-id="ad867-368">Hämta NPS-tillägget från [https://aka.ms/npsmfa](https://aka.ms/npsmfa).</span><span class="sxs-lookup"><span data-stu-id="ad867-368">Download the NPS extension from [https://aka.ms/npsmfa](https://aka.ms/npsmfa).</span></span> 
2. <span data-ttu-id="ad867-369">Kopiera den körbara filen för installationsprogrammet (NpsExtnForAzureMfaInstaller.exe) till NPS-servern.</span><span class="sxs-lookup"><span data-stu-id="ad867-369">Copy the setup executable file (NpsExtnForAzureMfaInstaller.exe) to the NPS server.</span></span>
3. <span data-ttu-id="ad867-370">Dubbelklicka på NPS-servern **NpsExtnForAzureMfaInstaller.exe**.</span><span class="sxs-lookup"><span data-stu-id="ad867-370">On the NPS server, double-click **NpsExtnForAzureMfaInstaller.exe**.</span></span> <span data-ttu-id="ad867-371">Om du uppmanas, klickar du på **kör**.</span><span class="sxs-lookup"><span data-stu-id="ad867-371">If prompted, click **Run**.</span></span>
4. <span data-ttu-id="ad867-372">Granska licensvillkoren för programvara i NPS-tillägget för Azure MFA dialogrutan Kontrollera **jag godkänner licensvillkoren och villkor**, och klicka på **installera**.</span><span class="sxs-lookup"><span data-stu-id="ad867-372">In the NPS Extension for Azure MFA dialog box, review the software license terms, check **I agree to the license terms and conditions**, and click **Install**.</span></span>

 ![NPS-tillägg](./media/nps-extension-vpn/image36.png)
 
5. <span data-ttu-id="ad867-374">Klicka på i NPS-tillägget för Azure MFA dialogrutan **Stäng**.</span><span class="sxs-lookup"><span data-stu-id="ad867-374">In the NPS Extension for Azure MFA dialog box, click **Close**.</span></span>  

 ![Installationen lyckades](./media/nps-extension-vpn/image37.png) 
 
### <a name="configure-certificates-for-use-with-the-nps-extension-using-a-powershell-script"></a><span data-ttu-id="ad867-376">Konfigurera certifikat för användning med NPS-tillägget med hjälp av ett PowerShell-skript</span><span class="sxs-lookup"><span data-stu-id="ad867-376">Configure certificates for use with the NPS extension using a PowerShell script</span></span>
<span data-ttu-id="ad867-377">För att säkerställa säker kommunikation och säkerhet, måste du konfigurera certifikat för användning av NPS-tillägget.</span><span class="sxs-lookup"><span data-stu-id="ad867-377">To ensure secure communications and assurance, you need to configure certificates for use by the NPS extension.</span></span> <span data-ttu-id="ad867-378">NPS-komponenter är ett Windows PowerShell-skript som konfigurerar ett självsignerat certifikat för användning med NPS.</span><span class="sxs-lookup"><span data-stu-id="ad867-378">The NPS components include a Windows PowerShell script that configures a self-signed certificate for use with NPS.</span></span> 

<span data-ttu-id="ad867-379">Skriptet utförs följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="ad867-379">The script performs the following actions:</span></span>

* <span data-ttu-id="ad867-380">Skapar ett självsignerat certifikat</span><span class="sxs-lookup"><span data-stu-id="ad867-380">Creates a self-signed certificate</span></span>
* <span data-ttu-id="ad867-381">Associerar offentliga nyckeln för certifikatet till tjänstens huvudnamn på Azure AD</span><span class="sxs-lookup"><span data-stu-id="ad867-381">Associates public key of certificate to service principal on Azure AD</span></span>
* <span data-ttu-id="ad867-382">Lagrar cert i lokala datorns Arkiv</span><span class="sxs-lookup"><span data-stu-id="ad867-382">Stores the cert in the local machine store</span></span>
* <span data-ttu-id="ad867-383">Beviljar åtkomst till certifikatets privata nyckel till nätverksanvändaren</span><span class="sxs-lookup"><span data-stu-id="ad867-383">Grants access to the certificate’s private key to the Network User</span></span>
* <span data-ttu-id="ad867-384">NPS-tjänsten har startats om</span><span class="sxs-lookup"><span data-stu-id="ad867-384">Restarts Network Policy Server service</span></span>

<span data-ttu-id="ad867-385">Om du vill använda dina egna certifikat måste du koppla allmänheten om ditt certifikat från tjänsten principen på Azure AD och så vidare.</span><span class="sxs-lookup"><span data-stu-id="ad867-385">If you want to use your own certificates, you need to associate the public of your certificate to the service principle on Azure AD, and so on.</span></span>
<span data-ttu-id="ad867-386">Om du vill använda skriptet ger tillägget dina administrativa autentiseringsuppgifter för Azure Active Directory och Azure Active Directory-klient-ID som du kopierade tidigare.</span><span class="sxs-lookup"><span data-stu-id="ad867-386">To use the script, provide the extension with your Azure Active Directory administrative credentials and the Azure Active Directory tenant ID you copied earlier.</span></span> <span data-ttu-id="ad867-387">Kör skriptet på varje NPS-server där du installerar NPS-tillägget.</span><span class="sxs-lookup"><span data-stu-id="ad867-387">Run the script on each NPS server where you install the NPS extension.</span></span>

1. <span data-ttu-id="ad867-388">Öppna en administrativ Windows PowerShell-Kommandotolken.</span><span class="sxs-lookup"><span data-stu-id="ad867-388">Open an administrative Windows PowerShell prompt.</span></span>
2. <span data-ttu-id="ad867-389">I PowerShell-Kommandotolken skriver _cd ”c:\Program Files\Microsoft\AzureMfa\Config'_, och tryck på **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="ad867-389">At the PowerShell prompt, type _cd ‘c:\Program Files\Microsoft\AzureMfa\Config’_, and press **ENTER**.</span></span>
3. <span data-ttu-id="ad867-390">Typen _.\AzureMfsNpsExtnConfigSetup.ps1_, och tryck på **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="ad867-390">Type _.\AzureMfsNpsExtnConfigSetup.ps1_, and press **ENTER**.</span></span> 
 * <span data-ttu-id="ad867-391">Skriptet kontrollerar om Azure Active Directory PowerShell-modulen är installerad.</span><span class="sxs-lookup"><span data-stu-id="ad867-391">The script checks to see if the Azure Active Directory PowerShell module is installed.</span></span> <span data-ttu-id="ad867-392">Om det inte är installerat installeras skriptet modulen.</span><span class="sxs-lookup"><span data-stu-id="ad867-392">If it is not installed, the script installs the module for you.</span></span>
 
 ![PowerShell](./media/nps-extension-vpn/image38.png)
 
4. <span data-ttu-id="ad867-394">När skriptet har kontrollerat installationen av PowerShell-modulen, visas dialogrutan för Azure Active Directory PowerShell-modulen.</span><span class="sxs-lookup"><span data-stu-id="ad867-394">After the script verifies the installation of the PowerShell module, it displays the Azure Active Directory PowerShell module dialog box.</span></span> <span data-ttu-id="ad867-395">I dialogrutan, ange autentiseringsuppgifter för Azure AD-administratör och lösenord och klicka på **logga in**.</span><span class="sxs-lookup"><span data-stu-id="ad867-395">In the dialog box, enter your Azure AD admin credentials and password, and click **Sign in**.</span></span> 
 
 ![PowerShell-logga In](./media/nps-extension-vpn/image39.png)
 
5. <span data-ttu-id="ad867-397">När du uppmanas, klistra in klient-ID som du kopierade tidigare till Urklipp och tryck på **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="ad867-397">When prompted, paste the tenant ID you copied to the clipboard earlier, and press **ENTER**.</span></span> 

 ![Klient-ID:t](./media/nps-extension-vpn/image40.png)

6. <span data-ttu-id="ad867-399">Skriptet skapar ett självsignerat certifikat och utför andra ändringar i konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="ad867-399">The script creates a self-signed certificate and performs other configuration changes.</span></span> <span data-ttu-id="ad867-400">Utdata är som på bilden nedan.</span><span class="sxs-lookup"><span data-stu-id="ad867-400">The output is like the image shown below.</span></span>

 ![Automatiskt signerat certifikat](./media/nps-extension-vpn/image41.png)

7. <span data-ttu-id="ad867-402">Starta om servern.</span><span class="sxs-lookup"><span data-stu-id="ad867-402">Reboot the server.</span></span>
 
### <a name="verify-configuration"></a><span data-ttu-id="ad867-403">Verifiera konfigurationen</span><span class="sxs-lookup"><span data-stu-id="ad867-403">Verify configuration</span></span>
<span data-ttu-id="ad867-404">Om du vill verifiera konfigurationen måste du upprätta en ny VPN-anslutning med VPN-servern.</span><span class="sxs-lookup"><span data-stu-id="ad867-404">To verify the configuration, you need to establish a new VPN connection with VPN server.</span></span> <span data-ttu-id="ad867-405">När du har angett dina autentiseringsuppgifter för primär autentisering, väntar VPN-anslutningen på den sekundära autentiseringen ska lyckas innan anslutningen har upprättats, enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="ad867-405">Upon successfully entering your credentials for primary authentication, the VPN connection waits for the secondary authentication to succeed before the connection is established, as shown below.</span></span> 

 ![Verifiera konfigurationen](./media/nps-extension-vpn/image42.png)

<span data-ttu-id="ad867-407">Om du har autentiseras med sekundära verifieringsmetod som du tidigare har konfigurerat i Azure MFA är du ansluten till resursen.</span><span class="sxs-lookup"><span data-stu-id="ad867-407">If you successfully authenticate with the secondary verification method you previously configured in Azure MFA, you are connected to the resource.</span></span> <span data-ttu-id="ad867-408">Om den sekundära autentiseringen inte lyckas nekas åtkomst till resursen.</span><span class="sxs-lookup"><span data-stu-id="ad867-408">However, if the secondary authentication is not successful, you are denied access to resource.</span></span> 

<span data-ttu-id="ad867-409">I exemplet nedan används Authenticator-appen på en Windows phone för att tillhandahålla den sekundära autentiseringen.</span><span class="sxs-lookup"><span data-stu-id="ad867-409">In the example below, the Authenticator app on a Windows phone is used to provide the secondary authentication.</span></span>

 ![Verifiera konto](./media/nps-extension-vpn/image43.png)

<span data-ttu-id="ad867-411">När du har autentiserats med metoden sekundär, beviljas åtkomst till den virtuella porten på VPN-servern.</span><span class="sxs-lookup"><span data-stu-id="ad867-411">Once you have successfully authenticated using the secondary method, you are granted access to the virtual port on the VPN server.</span></span> <span data-ttu-id="ad867-412">Eftersom du inte behövs för att använda en sekundär autentiseringsmetod med en mobil app på en betrodd enhet loggen i processen är dock säkrare än att det skulle använda bara ett användarnamn / lösenord kombination.</span><span class="sxs-lookup"><span data-stu-id="ad867-412">However, because you were required to use a secondary authentication method using a mobile app on a trusted device, the log in process is more secure than it would be using only a username / password combination.</span></span>

### <a name="view-event-viewer-logs-for-successful-logon-events"></a><span data-ttu-id="ad867-413">Visa loggarna i Loggboken för lyckade inloggningshändelser</span><span class="sxs-lookup"><span data-stu-id="ad867-413">View Event Viewer logs for successful logon events</span></span>
<span data-ttu-id="ad867-414">Om du vill visa lyckade inloggningshändelser loggas i Windows Loggboken, kan du utfärda följande Windows PowerShell-kommando för att fråga Windows säkerhetsloggen på NPS-servern.</span><span class="sxs-lookup"><span data-stu-id="ad867-414">To view the successful logon events in the Windows Event Viewer logs, you can issue the following Windows PowerShell command to query the Windows Security log on the NPS server.</span></span>

<span data-ttu-id="ad867-415">Om du vill fråga lyckade inloggningshändelser i visningsprogrammet säkerhetshändelser, använder du följande kommando,</span><span class="sxs-lookup"><span data-stu-id="ad867-415">To query successful logon events in the Security event viewer logs, use the following command,</span></span>
* <span data-ttu-id="ad867-416">_Get-WinEvent - loggnamn säkerhet_ | där {$_.ID - eq '6272'} | FL</span><span class="sxs-lookup"><span data-stu-id="ad867-416">_Get-WinEvent -Logname Security_ | where {$_.ID -eq '6272'} | FL</span></span> 

 ![Loggboken för säkerhet](./media/nps-extension-vpn/image44.png)
 
<span data-ttu-id="ad867-418">Du kan också visa säkerhetsloggen eller anpassad vy nätverksprincip och åtkomsttjänster enligt nedan:</span><span class="sxs-lookup"><span data-stu-id="ad867-418">You can also view the Security log or the Network Policy and Access Services custom view, as shown below:</span></span>

 ![Princip för nätverksåtkomst](./media/nps-extension-vpn/image45.png)

<span data-ttu-id="ad867-420">På den server där du installerade NPS-tillägget för Azure MFA, hittar du loggarna i Loggboken program specifika tillägget på **program och tjänster Logs\Microsoft\AzureMfa**.</span><span class="sxs-lookup"><span data-stu-id="ad867-420">On the server where you installed the NPS extension for Azure MFA, you can find Event Viewer application logs specific to the extension at **Application and Services Logs\Microsoft\AzureMfa**.</span></span> 

* <span data-ttu-id="ad867-421">_Get-WinEvent - loggnamn säkerhet_ | där {$_.ID - eq '6272'} | FL</span><span class="sxs-lookup"><span data-stu-id="ad867-421">_Get-WinEvent -Logname Security_ | where {$_.ID -eq '6272'} | FL</span></span>

 ![Antal händelser](./media/nps-extension-vpn/image46.png)

## <a name="troubleshoot-guide"></a><span data-ttu-id="ad867-423">Felsöka programguiden</span><span class="sxs-lookup"><span data-stu-id="ad867-423">Troubleshoot Guide</span></span>
<span data-ttu-id="ad867-424">Om konfigurationen inte fungerar som förväntat, är en bra plats att börja felsöka att kontrollera att användaren är konfigurerad för att använda Azure MFA.</span><span class="sxs-lookup"><span data-stu-id="ad867-424">If the configuration is not working as expected, a good place to start to troubleshoot is to verify that the user is configured to use Azure MFA.</span></span> <span data-ttu-id="ad867-425">Behörighet att ansluta till [https://portal.azure.com](https://portal.azure.com). Om de tillfrågas om sekundära autentiseringen och kan autentisera, vet du att en felaktig konfiguration av Azure MFA.</span><span class="sxs-lookup"><span data-stu-id="ad867-425">Have the user connect to [https://portal.azure.com](https://portal.azure.com). If they are prompted for secondary authentication and can successfully authenticate, you can eliminate an incorrect configuration of Azure MFA.</span></span>

<span data-ttu-id="ad867-426">Om Azure MFA fungerar för användare, bör du granska relevant händelseloggar.</span><span class="sxs-lookup"><span data-stu-id="ad867-426">If Azure MFA is working for the user(s), you should review the relevant Event logs.</span></span> <span data-ttu-id="ad867-427">Dessa inkluderar säkerhetshändelse, Gateway fungerar och Azure MFA-loggar som beskrivs i föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="ad867-427">These include the Security Event, Gateway operational, and Azure MFA logs that are discussed in the previous section.</span></span> 

<span data-ttu-id="ad867-428">Nedan visas ett exempel på utdata för säkerhetsloggen visar misslyckade inloggningsförsök-händelser (händelse-ID 6273):</span><span class="sxs-lookup"><span data-stu-id="ad867-428">Below is an example output of Security log showing a failed logon event (Event ID 6273):</span></span>

 ![Säkerhetsloggen](./media/nps-extension-vpn/image47.png)

<span data-ttu-id="ad867-430">Nedan visas en relaterad händelse från AzureMFA loggar:</span><span class="sxs-lookup"><span data-stu-id="ad867-430">Below is a related event from the AzureMFA logs:</span></span>

 ![Azure MFA-loggar](./media/nps-extension-vpn/image48.png)

<span data-ttu-id="ad867-432">Att utföra avancerad felsökning av alternativ, kontakta NPS format loggfiler där NPS-tjänsten är installerad.</span><span class="sxs-lookup"><span data-stu-id="ad867-432">To perform advanced troubleshoot options, consult the NPS database format log files where the NPS service is installed.</span></span> <span data-ttu-id="ad867-433">Dessa loggfiler skapas i _%SystemRoot%\System32\Logs_ mapp som kommaavgränsad textfiler.</span><span class="sxs-lookup"><span data-stu-id="ad867-433">These log files are created in _%SystemRoot%\System32\Logs_ folder as comma-delimited text files.</span></span> <span data-ttu-id="ad867-434">En beskrivning av dessa loggfiler finns i avsnittet [tolka NPS loggfilerna](https://technet.microsoft.com/library/cc771748.aspx).</span><span class="sxs-lookup"><span data-stu-id="ad867-434">For a description of these log files, see [Interpret NPS Database Format Log Files](https://technet.microsoft.com/library/cc771748.aspx).</span></span> 

<span data-ttu-id="ad867-435">Posterna i dessa loggfiler är svåra att tolka utan att importera dem till ett kalkylblad eller en databas.</span><span class="sxs-lookup"><span data-stu-id="ad867-435">The entries in these log files are difficult to interpret without importing them into a spreadsheet or a database.</span></span> <span data-ttu-id="ad867-436">Du hittar ett antal IAS-Parser online som hjälper dig att tolka loggfilerna.</span><span class="sxs-lookup"><span data-stu-id="ad867-436">You can find a number of IAS parsers online to assist you in interpreting the log files.</span></span> <span data-ttu-id="ad867-437">Nedan visas utdata i en sådan nedladdningsbara [shareware-programmet](http://www.deepsoftware.com/iasviewer):</span><span class="sxs-lookup"><span data-stu-id="ad867-437">Below is the output of one such downloadable [shareware application](http://www.deepsoftware.com/iasviewer):</span></span> 

 ![Shareware-programmet](./media/nps-extension-vpn/image49.png)

<span data-ttu-id="ad867-439">Slutligen för ytterligare felsökning av alternativ, kan du använda en protokollanalysator, till exempel Wireshark eller [Microsoft Message Analyzer](https://technet.microsoft.com/library/jj649776.aspx).</span><span class="sxs-lookup"><span data-stu-id="ad867-439">Finally, for additional troubleshoot options, you can use a protocol analyzer such as Wireshark or [Microsoft Message Analyzer](https://technet.microsoft.com/library/jj649776.aspx).</span></span> <span data-ttu-id="ad867-440">Följande bild från Wireshark visar RADIUS-meddelanden mellan VPN-servern och NPS-servern.</span><span class="sxs-lookup"><span data-stu-id="ad867-440">The following image from Wireshark shows the RADIUS messages between the VPN server and the NPS server.</span></span>

 ![Microsoft Message Analyzer](./media/nps-extension-vpn/image50.png)

<span data-ttu-id="ad867-442">Mer information finns i [integrera din befintliga infrastruktur för NPS med Azure Multi-Factor Authentication](multi-factor-authentication-nps-extension.md).</span><span class="sxs-lookup"><span data-stu-id="ad867-442">For more information, see [Integrate your existing NPS infrastructure with Azure Multi-Factor Authentication](multi-factor-authentication-nps-extension.md).</span></span>  

## <a name="next-steps"></a><span data-ttu-id="ad867-443">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ad867-443">Next steps</span></span>
[<span data-ttu-id="ad867-444">Skaffa Azure Multi-Factor Authentication</span><span class="sxs-lookup"><span data-stu-id="ad867-444">How to get Azure Multi-Factor Authentication</span></span>](multi-factor-authentication-versions-plans.md)

[<span data-ttu-id="ad867-445">Fjärrskrivbordsgateway och Azure Multi-Factor Authentication Server med RADIUS</span><span class="sxs-lookup"><span data-stu-id="ad867-445">Remote Desktop Gateway and Azure Multi-Factor Authentication Server using RADIUS</span></span>](multi-factor-authentication-get-started-server-rdg.md)

[<span data-ttu-id="ad867-446">Integrera dina lokala kataloger med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ad867-446">Integrate your on-premises directories with Azure Active Directory</span></span>](../active-directory/connect/active-directory-aadconnect.md)

