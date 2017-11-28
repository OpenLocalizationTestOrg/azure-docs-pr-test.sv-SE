---
title: "Säkerhetsaspekter för Azure AD Application Proxy | Microsoft Docs"
description: "Beskriver säkerhetsaspekter för att använda Azure AD Application Proxy"
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/03/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: c6ead651133eb17fd55f7567cdb14dc3bcd64245
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="security-considerations-for-accessing-apps-remotely-with-azure-ad-application-proxy"></a><span data-ttu-id="8a668-103">Säkerhetsaspekter för att komma åt appar med Azure AD Application Proxy</span><span class="sxs-lookup"><span data-stu-id="8a668-103">Security considerations for accessing apps remotely with Azure AD Application Proxy</span></span>

<span data-ttu-id="8a668-104">Den här artikeln förklarar hur Azure Active Directory Application Proxy ger en säker tjänst för att publicera och åtkomst till dina program via fjärranslutning.</span><span class="sxs-lookup"><span data-stu-id="8a668-104">This article explains how Azure Active Directory Application Proxy provides a secure service for publishing and accessing your applications remotely.</span></span>

<span data-ttu-id="8a668-105">Följande diagram visar hur Azure AD kan säker fjärråtkomst till lokala program.</span><span class="sxs-lookup"><span data-stu-id="8a668-105">The following diagram shows how Azure AD enables secure remote access to your on-premises applications.</span></span>

 ![Diagram över säker fjärråtkomst via Azure AD Application Proxy](./media/application-proxy-security-considerations/secure-remote-access.png)

## <a name="security-benefits"></a><span data-ttu-id="8a668-107">Säkerhetsfördelarna</span><span class="sxs-lookup"><span data-stu-id="8a668-107">Security benefits</span></span>

<span data-ttu-id="8a668-108">Azure AD Application Proxy fördelar följande säkerhet:</span><span class="sxs-lookup"><span data-stu-id="8a668-108">Azure AD Application Proxy offers the following security benefits:</span></span>

### <a name="authenticated-access"></a><span data-ttu-id="8a668-109">Autentiserad åtkomst</span><span class="sxs-lookup"><span data-stu-id="8a668-109">Authenticated access</span></span> 

<span data-ttu-id="8a668-110">Om du väljer att använda Azure Active Directory förautentisering kan endast autentiserade anslutningar ansluta till nätverket.</span><span class="sxs-lookup"><span data-stu-id="8a668-110">If you choose to use Azure Active Directory preauthentication, then only authenticated connections can access your network.</span></span>

<span data-ttu-id="8a668-111">Azure AD Application Proxy är beroende av Azure AD säkerhetstokentjänsten (STS) för alla autentisering.</span><span class="sxs-lookup"><span data-stu-id="8a668-111">Azure AD Application Proxy relies on the Azure AD security token service (STS) for all authentication.</span></span>  <span data-ttu-id="8a668-112">Förautentisering, blockeras sin natur som ett stort antal anonyma attacker eftersom endast autentiserade identiteter har åtkomst till backend-programmet.</span><span class="sxs-lookup"><span data-stu-id="8a668-112">Preauthentication, by its very nature, blocks a significant number of anonymous attacks, because only authenticated identities can access the back-end application.</span></span>

<span data-ttu-id="8a668-113">Om du väljer genomströmning som metod för förautentisering kan få du inte förmånen.</span><span class="sxs-lookup"><span data-stu-id="8a668-113">If you choose Passthrough as your preauthentication method, you don't get this benefit.</span></span> 

### <a name="conditional-access"></a><span data-ttu-id="8a668-114">Villkorlig åtkomst</span><span class="sxs-lookup"><span data-stu-id="8a668-114">Conditional access</span></span>

<span data-ttu-id="8a668-115">Använd rikare kontrollerna för principer innan anslutningar till nätverket upprättas.</span><span class="sxs-lookup"><span data-stu-id="8a668-115">Apply richer policy controls before connections to your network are established.</span></span>

<span data-ttu-id="8a668-116">Med [villkorlig åtkomst](active-directory-conditional-access-azuread-connected-apps.md), kan du ange begränsningar på vilken trafik tillåts åtkomst till backend-program.</span><span class="sxs-lookup"><span data-stu-id="8a668-116">With [conditional access](active-directory-conditional-access-azuread-connected-apps.md), you can define restrictions on what traffic is allowed to access your back-end applications.</span></span> <span data-ttu-id="8a668-117">Du kan skapa principer som begränsar inloggningar baserat på plats, stark autentisering och risken för användarprofil.</span><span class="sxs-lookup"><span data-stu-id="8a668-117">You can create policies that restrict sign-ins based on location, strength of authentication, and user risk profile.</span></span>

<span data-ttu-id="8a668-118">Du kan också använda villkorlig åtkomst för att konfigurera Multi-Factor Authentication-principer, att lägga till ytterligare en säkerhetsnivå till din användarautentisering.</span><span class="sxs-lookup"><span data-stu-id="8a668-118">You can also use conditional access to configure Multi-Factor Authentication policies, adding another layer of security to your user authentications.</span></span> 

### <a name="traffic-termination"></a><span data-ttu-id="8a668-119">Avslutning av trafik</span><span class="sxs-lookup"><span data-stu-id="8a668-119">Traffic termination</span></span>

<span data-ttu-id="8a668-120">All trafik avslutas i molnet.</span><span class="sxs-lookup"><span data-stu-id="8a668-120">All traffic is terminated in the cloud.</span></span>

<span data-ttu-id="8a668-121">Eftersom Azure AD Application Proxy är en omvänd proxy, avslutas all trafik till backend-program i tjänsten.</span><span class="sxs-lookup"><span data-stu-id="8a668-121">Because Azure AD Application Proxy is a reverse-proxy, all traffic to back-end applications is terminated at the service.</span></span> <span data-ttu-id="8a668-122">Sessionen kan hämta återupprättat bara med backend-servern, vilket innebär att backend-servrar inte utsätts för direkt HTTP-trafik.</span><span class="sxs-lookup"><span data-stu-id="8a668-122">The session can get reestablished only with the back-end server, which means that your back-end servers are not exposed to direct HTTP traffic.</span></span> <span data-ttu-id="8a668-123">Den här konfigurationen innebär att du skyddas bättre från riktade attacker.</span><span class="sxs-lookup"><span data-stu-id="8a668-123">This configuration means that you are better protected from targeted attacks.</span></span>

### <a name="all-access-is-outbound"></a><span data-ttu-id="8a668-124">All åtkomst är utgående</span><span class="sxs-lookup"><span data-stu-id="8a668-124">All access is outbound</span></span> 

<span data-ttu-id="8a668-125">Du behöver inte öppna inkommande anslutningar till företagsnätverket.</span><span class="sxs-lookup"><span data-stu-id="8a668-125">You don't need to open inbound connections to the corporate network.</span></span>

<span data-ttu-id="8a668-126">Application Proxy kopplingar kan bara använda utgående anslutningar till Azure AD Application Proxy-tjänst, vilket innebär att det finns ingen anledning att öppna portar i brandväggen för inkommande anslutningar.</span><span class="sxs-lookup"><span data-stu-id="8a668-126">Application Proxy connectors only use outbound connections to the Azure AD Application Proxy service, which means that there is no need to open firewall ports for incoming connections.</span></span> <span data-ttu-id="8a668-127">Traditionella proxyservrar krävs ett perimeternätverk (även kallat *DMZ*, *demilitariserad zon*, eller *avskärmat undernät*) och få tillgång till oautentiserade anslutningar nätverket kant.</span><span class="sxs-lookup"><span data-stu-id="8a668-127">Traditional proxies required a perimeter network (also known as *DMZ*, *demilitarized zone*, or *screened subnet*) and allowed access to unauthenticated connections at the network edge.</span></span> <span data-ttu-id="8a668-128">Det här scenariot krävs för många ytterligare investeringar i web application brandväggsprodukter att analysera trafik och ger ytterligare skydd för miljön.</span><span class="sxs-lookup"><span data-stu-id="8a668-128">This scenario required many additional investments in web application firewall products to analyze traffic and offer addition protections to the environment.</span></span> <span data-ttu-id="8a668-129">Med Application Proxy behöver du inte ett perimeternätverk eftersom alla anslutningar är utgående och sker över en säker kanal.</span><span class="sxs-lookup"><span data-stu-id="8a668-129">With Application Proxy, you don't need a perimeter network because all connections are outbound and take place over a secure channel.</span></span>

<span data-ttu-id="8a668-130">Läs mer om kopplingar [förstå Azure AD Application Proxy kopplingar](application-proxy-understand-connectors.md).</span><span class="sxs-lookup"><span data-stu-id="8a668-130">For more information about connectors, see [Understand Azure AD Application Proxy connectors](application-proxy-understand-connectors.md).</span></span>

### <a name="cloud-scale-analytics-and-machine-learning"></a><span data-ttu-id="8a668-131">Skalbar molnlagring analyser och maskininlärning</span><span class="sxs-lookup"><span data-stu-id="8a668-131">Cloud-scale analytics and machine learning</span></span> 

<span data-ttu-id="8a668-132">Hämta senaste säkerhetsskydd.</span><span class="sxs-lookup"><span data-stu-id="8a668-132">Get cutting-edge security protection.</span></span>

<span data-ttu-id="8a668-133">Eftersom det är en del av Azure Active Directory Application Proxy kan utnyttja [Azure AD Identity Protection](active-directory-identityprotection.md), med machine learning-driven intelligence och data från Microsoft Security Response Center och Digital Crimes Unit.</span><span class="sxs-lookup"><span data-stu-id="8a668-133">Because it's part of Azure Active Directory, Application Proxy can leverage [Azure AD Identity Protection](active-directory-identityprotection.md), with machine learning-driven intelligence and data from the Microsoft Security Response Center and Digital Crimes Unit.</span></span> <span data-ttu-id="8a668-134">Tillsammans vi proaktivt identifiera avslöjade konton och erbjuder realtidsskydd från hög risk inloggningar. Vi ta hänsyn till flera faktorer, till exempel åtkomst från infekterade enheter via anonymizing nätverk, och från ovanliga och inte troligt platser.</span><span class="sxs-lookup"><span data-stu-id="8a668-134">Together we proactively identify compromised accounts and offer real-time protection from high-risk sign-ins. We take into account numerous factors, such as access from infected devices, through anonymizing networks, and from atypical and unlikely locations.</span></span>

<span data-ttu-id="8a668-135">Många av dessa rapporter och händelser som är redan tillgängliga via ett API för integrering med din säkerhetsinformation och händelse-hanteringssystem (SIEM).</span><span class="sxs-lookup"><span data-stu-id="8a668-135">Many of these reports and events are already available through an API for integration with your security information and event management (SIEM) systems.</span></span>

### <a name="remote-access-as-a-service"></a><span data-ttu-id="8a668-136">Fjärråtkomst som en tjänst</span><span class="sxs-lookup"><span data-stu-id="8a668-136">Remote access as a service</span></span>

<span data-ttu-id="8a668-137">Du behöver inte bry dig om underhåll och korrigering lokala servrar.</span><span class="sxs-lookup"><span data-stu-id="8a668-137">You don’t have to worry about maintaining and patching on-premises servers.</span></span>

<span data-ttu-id="8a668-138">Okorrigerade programvara fortfarande står för ett stort antal attacker.</span><span class="sxs-lookup"><span data-stu-id="8a668-138">Unpatched software still accounts for a large number of attacks.</span></span> <span data-ttu-id="8a668-139">Azure AD Application Proxy är en Internet-skala som Microsoft äger, så du får alltid senaste säkerhetskorrigeringar och uppgraderingar.</span><span class="sxs-lookup"><span data-stu-id="8a668-139">Azure AD Application Proxy is an Internet-scale service that Microsoft owns, so you always get the latest security patches and upgrades.</span></span>

<span data-ttu-id="8a668-140">Om du vill förbättra säkerheten för program som publicerats av Azure AD Application Proxy blockera vi web crawler robotar från indexering och arkivera dina program.</span><span class="sxs-lookup"><span data-stu-id="8a668-140">To improve the security of applications published by Azure AD Application Proxy, we block web crawler robots from indexing and archiving your applications.</span></span> <span data-ttu-id="8a668-141">Varje gång en web crawler robot försöker hämta av roboten inställningarna för en publicerad appen Application Proxy svarar med en robots.txt-fil som innehåller `User-agent: * Disallow: /`.</span><span class="sxs-lookup"><span data-stu-id="8a668-141">Each time a web crawler robot tries to retrieve the robot's settings for a published app, Application Proxy replies with a robots.txt file that includes `User-agent: * Disallow: /`.</span></span>

## <a name="under-the-hood"></a><span data-ttu-id="8a668-142">Under huven</span><span class="sxs-lookup"><span data-stu-id="8a668-142">Under the hood</span></span>

<span data-ttu-id="8a668-143">Azure AD Application Proxy består av två delar:</span><span class="sxs-lookup"><span data-stu-id="8a668-143">Azure AD Application Proxy consists of two parts:</span></span>

* <span data-ttu-id="8a668-144">Molnbaserad tjänst: den här tjänsten körs i Azure och är där externa klientanvändare anslutningar görs.</span><span class="sxs-lookup"><span data-stu-id="8a668-144">The cloud-based service: This service runs in Azure, and is where the external client/user connections are made.</span></span>
* <span data-ttu-id="8a668-145">[Lokal anslutning](application-proxy-understand-connectors.md): komponenten lokalt kopplingen lyssnar efter förfrågningar från Azure AD Application Proxy-tjänsten och hanterar anslutningar till de interna program.</span><span class="sxs-lookup"><span data-stu-id="8a668-145">[The on-premises connector](application-proxy-understand-connectors.md): An on-premises component, the connector listens for requests from the Azure AD Application Proxy service and handles connections to the internal applications.</span></span> 

<span data-ttu-id="8a668-146">Ett flöde mellan kopplingen och tjänsten Application Proxy upprättat när:</span><span class="sxs-lookup"><span data-stu-id="8a668-146">A flow between the connector and the Application Proxy service is established when:</span></span>

* <span data-ttu-id="8a668-147">Anslutningen konfigureras först.</span><span class="sxs-lookup"><span data-stu-id="8a668-147">The connector is first set up.</span></span>
* <span data-ttu-id="8a668-148">Kopplingen hämtar konfigurationsinformation från tjänsten Application Proxy.</span><span class="sxs-lookup"><span data-stu-id="8a668-148">The connector pulls configuration information from the Application Proxy service.</span></span>
* <span data-ttu-id="8a668-149">En användare kommer åt ett publicerat program.</span><span class="sxs-lookup"><span data-stu-id="8a668-149">A user accesses a published application.</span></span>

>[!NOTE]
><span data-ttu-id="8a668-150">All kommunikation som sker över SSL och de kommer alltid vid anslutningen till tjänsten Application Proxy.</span><span class="sxs-lookup"><span data-stu-id="8a668-150">All communications occur over SSL, and they always originate at the connector to the Application Proxy service.</span></span> <span data-ttu-id="8a668-151">Tjänsten är endast utgående.</span><span class="sxs-lookup"><span data-stu-id="8a668-151">The service is outbound only.</span></span>

<span data-ttu-id="8a668-152">Anslutningen används ett klientcertifikat för att autentisera till Application Proxy-tjänst för nästan alla samtal.</span><span class="sxs-lookup"><span data-stu-id="8a668-152">The connector uses a client certificate to authenticate to the Application Proxy service for nearly all calls.</span></span> <span data-ttu-id="8a668-153">Det enda undantaget till detta är det första installation-steget där Klientcertifikatet har upprättats.</span><span class="sxs-lookup"><span data-stu-id="8a668-153">The only exception to this process is the initial setup step, where the client certificate is established.</span></span>

### <a name="installing-the-connector"></a><span data-ttu-id="8a668-154">Installerar connector</span><span class="sxs-lookup"><span data-stu-id="8a668-154">Installing the connector</span></span>

<span data-ttu-id="8a668-155">När kopplingen först har konfigurerats ske på följande händelser för flödet:</span><span class="sxs-lookup"><span data-stu-id="8a668-155">When the connector is first set up, the following flow events take place:</span></span>

1. <span data-ttu-id="8a668-156">Registreringen av anslutningsverktyget för tjänsten sker som en del av installationen av anslutningen.</span><span class="sxs-lookup"><span data-stu-id="8a668-156">The connector registration to the service happens as part of the installation of the connector.</span></span> <span data-ttu-id="8a668-157">Användarna uppmanas att ange sina autentiseringsuppgifter för Azure AD-administratör.</span><span class="sxs-lookup"><span data-stu-id="8a668-157">Users are prompted to enter their Azure AD admin credentials.</span></span> <span data-ttu-id="8a668-158">Den token som erhållits från den här autentiseringen sedan presenteras för Azure AD Application Proxy-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="8a668-158">The token acquired from this authentication is then presented to the Azure AD Application Proxy service.</span></span>
2. <span data-ttu-id="8a668-159">Tjänsten Application Proxy utvärderar token.</span><span class="sxs-lookup"><span data-stu-id="8a668-159">The Application Proxy service evaluates the token.</span></span> <span data-ttu-id="8a668-160">Det garanterar att användaren är en företagsadministratör inom den klient som token har utfärdats för.</span><span class="sxs-lookup"><span data-stu-id="8a668-160">It ensures that the user is a company administrator within the tenant that the token was issued for.</span></span> <span data-ttu-id="8a668-161">Om användaren inte är administratör avslutas processen.</span><span class="sxs-lookup"><span data-stu-id="8a668-161">If the user is not an administrator, the process is terminated.</span></span>
3. <span data-ttu-id="8a668-162">Kopplingen genererar en certifikatbegäran för klienten och skickar den, tillsammans med denna token till Application Proxy-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="8a668-162">The connector generates a client certificate request and passes it, along with the token, to the Application Proxy service.</span></span> <span data-ttu-id="8a668-163">Tjänsten i sin tur verifierar token och signerar certifikat klientbegäran.</span><span class="sxs-lookup"><span data-stu-id="8a668-163">The service in turn verifies the token and signs the client certificate request.</span></span>
4. <span data-ttu-id="8a668-164">Anslutningen används klientcertifikatet för framtida kommunikation med tjänsten Application Proxy.</span><span class="sxs-lookup"><span data-stu-id="8a668-164">The connector uses the client certificate for future communication with the Application Proxy service.</span></span>
5. <span data-ttu-id="8a668-165">Kopplingen utför en inledande pull system konfigurationsdata från tjänsten med hjälp av dess klientcertifikatet och är nu redo att ta begäranden.</span><span class="sxs-lookup"><span data-stu-id="8a668-165">The connector performs an initial pull of the system configuration data from the service using its client certificate, and it is now ready to take requests.</span></span>

### <a name="updating-the-configuration-settings"></a><span data-ttu-id="8a668-166">Uppdaterar konfigurationsinställningarna</span><span class="sxs-lookup"><span data-stu-id="8a668-166">Updating the configuration settings</span></span>

<span data-ttu-id="8a668-167">När tjänsten Application Proxy uppdaterar konfigurationsinställningarna, äga rum flödet följande händelser:</span><span class="sxs-lookup"><span data-stu-id="8a668-167">Whenever the Application Proxy service updates the configuration settings, the following flow events take place:</span></span>

1. <span data-ttu-id="8a668-168">Kopplingen ansluter till slutpunkten konfigurationen i tjänsten Application Proxy genom att använda dess klientcertifikat.</span><span class="sxs-lookup"><span data-stu-id="8a668-168">The connector connects to the configuration endpoint within the Application Proxy service by using its client certificate.</span></span>
2. <span data-ttu-id="8a668-169">När klientens certifikat har verifierats, returnerar tjänsten Application Proxy konfigurationsdata för koppling (till exempel connector gruppen som anslutningen ska vara en del av).</span><span class="sxs-lookup"><span data-stu-id="8a668-169">After the client certificate has been validated, the Application Proxy service returns configuration data to the connector (for example, the connector group that the connector should be part of).</span></span>
3. <span data-ttu-id="8a668-170">Om det aktuella certifikatet är mer än 180 dagar, genererar kopplingen en ny certifikatbegäran som effektivt uppdaterar klientcertifikatet varje 180 dagar.</span><span class="sxs-lookup"><span data-stu-id="8a668-170">If the current certificate is more than 180 days old, the connector generates a new certificate request, which effectively updates the client certificate every 180 days.</span></span>

### <a name="accessing-published-applications"></a><span data-ttu-id="8a668-171">Åtkomst till publicerade program</span><span class="sxs-lookup"><span data-stu-id="8a668-171">Accessing published applications</span></span>

<span data-ttu-id="8a668-172">När användare har åtkomst till ett publicerat program äga rum mellan Application Proxy-tjänsten och Application Proxy connector följande händelser:</span><span class="sxs-lookup"><span data-stu-id="8a668-172">When users access a published application, the following events take place between the Application Proxy service and the Application Proxy connector:</span></span>

1. [<span data-ttu-id="8a668-173">Tjänsten autentiserar användaren för appen</span><span class="sxs-lookup"><span data-stu-id="8a668-173">The service authenticates the user for the app</span></span>](#the-service-checks-the-configuration-settings-for-the-app)
2. [<span data-ttu-id="8a668-174">Tjänsten placerar en begäran i kö för kopplingen</span><span class="sxs-lookup"><span data-stu-id="8a668-174">The service places a request in the connector queue</span></span>](#The-service-places-a-request-in-the-connector-queue)
3. [<span data-ttu-id="8a668-175">En koppling bearbetar begäran från kön</span><span class="sxs-lookup"><span data-stu-id="8a668-175">A connector processes the request from the queue</span></span>](#the-connector-receives-the-request-from-the-queue)
4. [<span data-ttu-id="8a668-176">Anslutningen väntar på svar</span><span class="sxs-lookup"><span data-stu-id="8a668-176">The connector waits for a response</span></span>](#the-connector-waits-for-a-response)
5. [<span data-ttu-id="8a668-177">Tjänsten strömmar data till användaren</span><span class="sxs-lookup"><span data-stu-id="8a668-177">The service streams data to the user</span></span>](#the-service-streams-data-to-the-user)

<span data-ttu-id="8a668-178">Om du vill veta mer om vad sker i var och en av de här stegen kan du fortsätta läsa.</span><span class="sxs-lookup"><span data-stu-id="8a668-178">To learn more about what takes place in each of these steps, keep reading.</span></span>


#### <a name="1-the-service-authenticates-the-user-for-the-app"></a><span data-ttu-id="8a668-179">1. Tjänsten autentiserar användaren för appen</span><span class="sxs-lookup"><span data-stu-id="8a668-179">1. The service authenticates the user for the app</span></span>

<span data-ttu-id="8a668-180">Om du har konfigurerat appen för att använda genomströmning som dess förautentiseringsmetoden hoppas över stegen i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="8a668-180">If you configured the app to use Passthrough as its preauthentication method, the steps in this section are skipped.</span></span>

<span data-ttu-id="8a668-181">Om du har konfigurerat appen att preauthenticate med Azure AD, omdirigeras användarna till Azure AD-STS att autentisera och ske i följande steg:</span><span class="sxs-lookup"><span data-stu-id="8a668-181">If you configured the app to preauthenticate with Azure AD, users are redirected to the Azure AD STS to authenticate, and the following steps take place:</span></span>

1. <span data-ttu-id="8a668-182">Application Proxy kontrollerar alla krav på villkorlig åtkomst för det specifika programmet.</span><span class="sxs-lookup"><span data-stu-id="8a668-182">Application Proxy checks for any conditional access policy requirements for the specific application.</span></span> <span data-ttu-id="8a668-183">Det här steget säkerställer att användaren har tilldelats till programmet.</span><span class="sxs-lookup"><span data-stu-id="8a668-183">This step ensures that the user has been assigned to the application.</span></span> <span data-ttu-id="8a668-184">Om tvåstegsverifiering krävs efterfrågar autentisering sekvensen en andra autentiseringsmetod.</span><span class="sxs-lookup"><span data-stu-id="8a668-184">If two-step verification is required, the authentication sequence prompts the user for a second authentication method.</span></span>

2. <span data-ttu-id="8a668-185">När alla kontroller har klarat STS Azure AD utfärdar en signerade token för programmet och omdirigeras användaren till Application Proxy-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="8a668-185">After all checks have passed, the Azure AD STS issues a signed token for the application and redirects the user back to the Application Proxy service.</span></span>

3. <span data-ttu-id="8a668-186">Programproxy verifierar att token har utfärdats för att åtgärda programmet.</span><span class="sxs-lookup"><span data-stu-id="8a668-186">Application Proxy verifies that the token was issued to correct the application.</span></span> <span data-ttu-id="8a668-187">Den genomför också andra kontroller som säkerställer att token har signerats av Azure AD och att det är fortfarande i fönstret giltig.</span><span class="sxs-lookup"><span data-stu-id="8a668-187">It performs other checks also, such as ensuring that the token was signed by Azure AD, and that it is still within the valid window.</span></span>

4. <span data-ttu-id="8a668-188">Application Proxy anger en krypterad autentiseringscookie som anger att autentiseringen till programmet har uppstått.</span><span class="sxs-lookup"><span data-stu-id="8a668-188">Application Proxy sets an encrypted authentication cookie to indicate that authentication to the application has occurred.</span></span> <span data-ttu-id="8a668-189">Cookien innehåller ett förfallodatum tidsstämpel som baseras på token från Azure AD och andra data, till exempel användarnamn som autentiseringen baseras på.</span><span class="sxs-lookup"><span data-stu-id="8a668-189">The cookie includes an expiration timestamp that's based on the token from Azure AD and other data, such as the user name that the authentication is based on.</span></span> <span data-ttu-id="8a668-190">Cookien krypteras med en privat nyckel som känner till Application Proxy-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="8a668-190">The cookie is encrypted with a private key known only to the Application Proxy service.</span></span>

5. <span data-ttu-id="8a668-191">Application Proxy dirigerar användaren till den ursprungligen begärda URL: en.</span><span class="sxs-lookup"><span data-stu-id="8a668-191">Application Proxy redirects the user back to the originally requested URL.</span></span>

<span data-ttu-id="8a668-192">Om någon del av förautentisering steg misslyckas, användarens begäran nekas och användaren visas ett meddelande om orsaken till problemet.</span><span class="sxs-lookup"><span data-stu-id="8a668-192">If any part of the preauthentication steps fails, the user’s request is denied, and the user is shown a message indicating the source of the problem.</span></span>


#### <a name="2-the-service-places-a-request-in-the-connector-queue"></a><span data-ttu-id="8a668-193">2. Tjänsten placerar en begäran i kö för kopplingen</span><span class="sxs-lookup"><span data-stu-id="8a668-193">2. The service places a request in the connector queue</span></span>

<span data-ttu-id="8a668-194">Kopplingar hålla en utgående anslutning öppen till Application Proxy-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="8a668-194">Connectors keep an outbound connection open to the Application Proxy service.</span></span> <span data-ttu-id="8a668-195">När en begäran kommer, köar tjänsten in den på en av de anslutningar som är öppna för kopplingen för att hämta.</span><span class="sxs-lookup"><span data-stu-id="8a668-195">When a request comes in, the service queues up the request on one of the open connections for the connector to pick up.</span></span>

<span data-ttu-id="8a668-196">Begäran innehåller objekt från programmet, till exempel begärandehuvuden data från den kryptera cookien användaren att göra begäran och begärande-ID.</span><span class="sxs-lookup"><span data-stu-id="8a668-196">The request includes items from the application, such as the request headers, data from the encrypted cookie, the user making the request, and the request ID.</span></span> <span data-ttu-id="8a668-197">Även om data från den kryptera cookien skickas med förfrågan är inte autentiseringscookien sig själv.</span><span class="sxs-lookup"><span data-stu-id="8a668-197">Although data from the encrypted cookie is sent with the request, the authentication cookie itself is not.</span></span>

#### <a name="3-the-connector-processes-the-request-from-the-queue"></a><span data-ttu-id="8a668-198">3. Kopplingen bearbetar begäran från kön.</span><span class="sxs-lookup"><span data-stu-id="8a668-198">3. The connector processes the request from the queue.</span></span> 

<span data-ttu-id="8a668-199">Programproxy utför baserat på begäran, någon av följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="8a668-199">Based on the request, Application Proxy performs one of the following actions:</span></span>

* <span data-ttu-id="8a668-200">Om begäran är en enkel åtgärd (t.ex, det finns inga data i meddelandets brödtext som med en RESTful *hämta* begäran), kopplingen gör en anslutning till den interna målresursen och sedan väntar på svar.</span><span class="sxs-lookup"><span data-stu-id="8a668-200">If the request is a simple operation (for example, there is no data within the body as is with a RESTful *GET* request), the connector makes a connection to the target internal resource and then waits for a response.</span></span>

* <span data-ttu-id="8a668-201">Om begäran har data som är associerade med den i meddelandetexten (till exempel en RESTful *POST* åtgärden), kopplingen gör en utgående anslutning med hjälp av klientcertifikatet till Application Proxy-instans.</span><span class="sxs-lookup"><span data-stu-id="8a668-201">If the request has data associated with it in the body (for example, a RESTful *POST* operation), the connector makes an outbound connection by using the client certificate to the Application Proxy instance.</span></span> <span data-ttu-id="8a668-202">Det gör den här anslutningen att begära data och öppna en anslutning till den interna resursen.</span><span class="sxs-lookup"><span data-stu-id="8a668-202">It makes this connection to request the data and open a connection to the internal resource.</span></span> <span data-ttu-id="8a668-203">När den tar emot en begäran från kopplingen Application Proxy-tjänsten börjar ta emot innehåll från användaren och vidarebefordrar data till anslutningstjänsten.</span><span class="sxs-lookup"><span data-stu-id="8a668-203">After it receives the request from the connector, the Application Proxy service begins accepting content from the user and forwards data to the connector.</span></span> <span data-ttu-id="8a668-204">Anslutningen kan i sin tur vidarebefordrar data till den interna resursen.</span><span class="sxs-lookup"><span data-stu-id="8a668-204">The connector, in turn, forwards the data to the internal resource.</span></span>

#### <a name="4-the-connector-waits-for-a-response"></a><span data-ttu-id="8a668-205">4. Anslutningen väntar på svar.</span><span class="sxs-lookup"><span data-stu-id="8a668-205">4. The connector waits for a response.</span></span>

<span data-ttu-id="8a668-206">När begäran och överföring av allt innehåll serverdelen är klar väntar anslutningen på svar.</span><span class="sxs-lookup"><span data-stu-id="8a668-206">After the request and transmission of all content to the back end is complete, the connector waits for a response.</span></span>

<span data-ttu-id="8a668-207">När den tar emot ett svar gör kopplingen en utgående anslutning till tjänsten Application Proxy att returnera information för sidhuvud och börja strömning returnerade data.</span><span class="sxs-lookup"><span data-stu-id="8a668-207">After it receives a response, the connector makes an outbound connection to the Application Proxy service, to return the header details and begin streaming the return data.</span></span>

#### <a name="5-the-service-streams-data-to-the-user"></a><span data-ttu-id="8a668-208">5. Tjänsten strömmas data för användaren.</span><span class="sxs-lookup"><span data-stu-id="8a668-208">5. The service streams data to the user.</span></span> 

<span data-ttu-id="8a668-209">Vissa bearbetningen av programmet kan inträffa här.</span><span class="sxs-lookup"><span data-stu-id="8a668-209">Some processing of the application may occur here.</span></span> <span data-ttu-id="8a668-210">Om du har konfigurerat Application Proxy att översätta huvuden eller URL: er i ditt program sker bearbetningen som behövs för det här steget.</span><span class="sxs-lookup"><span data-stu-id="8a668-210">If you configured Application Proxy to translate headers or URLs in your application, that processing happens as needed during this step.</span></span>


## <a name="next-steps"></a><span data-ttu-id="8a668-211">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8a668-211">Next steps</span></span>

[<span data-ttu-id="8a668-212">Topologiöverväganden nätverk när du använder Azure AD Application Proxy</span><span class="sxs-lookup"><span data-stu-id="8a668-212">Network topology considerations when using Azure AD Application Proxy</span></span>](application-proxy-network-topology-considerations.md)

[<span data-ttu-id="8a668-213">Förstå Azure AD Application Proxy-kopplingar</span><span class="sxs-lookup"><span data-stu-id="8a668-213">Understand Azure AD Application Proxy connectors</span></span>](application-proxy-understand-connectors.md)
