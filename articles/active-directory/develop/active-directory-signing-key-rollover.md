---
title: "Signering nyckelförnyelse i Azure AD | Microsoft Docs"
description: "Den här artikeln beskrivs signering nyckelförnyelse bästa praxis för Azure Active Directory"
services: active-directory
documentationcenter: .net
author: dstrockis
manager: krassk
editor: 
ms.assetid: ed964056-0723-42fe-bb69-e57323b9407f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2016
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 228bb9058537af1e4eb38207c376c2eb86aee68c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="signing-key-rollover-in-azure-active-directory"></a><span data-ttu-id="85be4-103">Signering nyckelförnyelse i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="85be4-103">Signing key rollover in Azure Active Directory</span></span>
<span data-ttu-id="85be4-104">Det här avsnittet beskrivs vad du behöver känna till om de offentliga nycklarna som används i Azure Active Directory (Azure AD) för att signera säkerhetstoken.</span><span class="sxs-lookup"><span data-stu-id="85be4-104">This topic discusses what you need to know about the public keys that are used in Azure Active Directory (Azure AD) to sign security tokens.</span></span> <span data-ttu-id="85be4-105">Det är viktigt att Observera att dessa nycklar förnyelse regelbundet och, i nödfall, kan återställas omedelbart.</span><span class="sxs-lookup"><span data-stu-id="85be4-105">It is important to note that these keys rollover on a periodic basis and, in an emergency, could be rolled over immediately.</span></span> <span data-ttu-id="85be4-106">Alla program som använder Azure AD ska kunna hantera nyckelförnyelse processen eller upprätta en process som regelbundet manuell förnyelse programmässigt.</span><span class="sxs-lookup"><span data-stu-id="85be4-106">All applications that use Azure AD should be able to programmatically handle the key rollover process or establish a periodic manual rollover process.</span></span> <span data-ttu-id="85be4-107">Fortsätt läsa för att förstå hur nycklarna fungerar, hur du utvärdera effekten av förnyelsen ditt program och att uppdatera ditt program eller upprätta en periodiska manuell förnyelse process för att hantera nyckelförnyelse om det behövs.</span><span class="sxs-lookup"><span data-stu-id="85be4-107">Continue reading to understand how the keys work, how to assess the impact of the rollover to your application and how to update your application or establish a periodic manual rollover process to handle key rollover if necessary.</span></span>

## <a name="overview-of-signing-keys-in-azure-ad"></a><span data-ttu-id="85be4-108">Översikt över Signeringsnycklar i Azure AD</span><span class="sxs-lookup"><span data-stu-id="85be4-108">Overview of signing keys in Azure AD</span></span>
<span data-ttu-id="85be4-109">Azure AD använder offentliga nycklar som bygger på branschstandard för att upprätta förtroende mellan sig själv och de program som använder den.</span><span class="sxs-lookup"><span data-stu-id="85be4-109">Azure AD uses public-key cryptography built on industry standards to establish trust between itself and the applications that use it.</span></span> <span data-ttu-id="85be4-110">Rent praktiskt detta fungerar på följande sätt: Azure AD använder en signeringsnyckel som består av en offentlig och privat nyckel.</span><span class="sxs-lookup"><span data-stu-id="85be4-110">In practical terms, this works in the following way: Azure AD uses a signing key that consists of a public and private key pair.</span></span> <span data-ttu-id="85be4-111">När en användare loggar in på ett program som använder Azure AD för autentisering, skapar en säkerhetstoken som innehåller information om användaren i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="85be4-111">When a user signs in to an application that uses Azure AD for authentication, Azure AD creates a security token that contains information about the user.</span></span> <span data-ttu-id="85be4-112">Denna token är signerat av Azure AD med hjälp av den privata nyckeln innan den skickas tillbaka till programmet.</span><span class="sxs-lookup"><span data-stu-id="85be4-112">This token is signed by Azure AD using its private key before it is sent back to the application.</span></span> <span data-ttu-id="85be4-113">Verifiera att token är giltig och faktiskt har sitt ursprung från Azure AD, måste programmet Validera token signatur med hjälp av den offentliga nyckeln som exponeras av Azure AD som ingår i klientens [OpenID Connect discovery-dokumentet](http://openid.net/specs/openid-connect-discovery-1_0.html) eller SAML/WS-Fed [federation Metadatadokumentet](active-directory-federation-metadata.md).</span><span class="sxs-lookup"><span data-stu-id="85be4-113">To verify that the token is valid and actually originated from Azure AD, the application must validate the token’s signature using the public key exposed by Azure AD that is contained in the tenant’s [OpenID Connect discovery document](http://openid.net/specs/openid-connect-discovery-1_0.html) or SAML/WS-Fed [federation metadata document](active-directory-federation-metadata.md).</span></span>

<span data-ttu-id="85be4-114">Av säkerhetsskäl kan Azure AD-signering nyckel samlar regelbundet och, i nödfall, återställas omedelbart.</span><span class="sxs-lookup"><span data-stu-id="85be4-114">For security purposes, Azure AD’s signing key rolls on a periodic basis and, in the case of an emergency, could be rolled over immediately.</span></span> <span data-ttu-id="85be4-115">Alla program som kan integreras med Azure AD bör vara beredd att hantera en nyckelförnyelse händelse oavsett hur ofta kan det uppstå.</span><span class="sxs-lookup"><span data-stu-id="85be4-115">Any application that integrates with Azure AD should be prepared to handle a key rollover event no matter how frequently it may occur.</span></span> <span data-ttu-id="85be4-116">Om det inte, och programmet försöker använda en har upphört att gälla för att verifiera signaturen på ett token, misslyckas inloggningen begäran.</span><span class="sxs-lookup"><span data-stu-id="85be4-116">If it doesn’t, and your application attempts to use an expired key to verify the signature on a token, the sign-in request will fail.</span></span>

<span data-ttu-id="85be4-117">Det finns alltid mer än en giltig nyckel i discovery-dokumentet OpenID Connect och federation Metadatadokumentet.</span><span class="sxs-lookup"><span data-stu-id="85be4-117">There is always more than one valid key available in the OpenID Connect discovery document and the federation metadata document.</span></span> <span data-ttu-id="85be4-118">Ditt program bör vara beredd på att använda någon av nycklar som har angetts i dokumentet, eftersom en nyckel kan återställas snart, en annan kan dess ersättare, och så vidare.</span><span class="sxs-lookup"><span data-stu-id="85be4-118">Your application should be prepared to use any of the keys specified in the document, since one key may be rolled soon, another may be its replacement, and so forth.</span></span>

## <a name="how-to-assess-if-your-application-will-be-affected-and-what-to-do-about-it"></a><span data-ttu-id="85be4-119">Hur du utvärdera om ditt program kommer att påverkas och vad du gör om den</span><span class="sxs-lookup"><span data-stu-id="85be4-119">How to assess if your application will be affected and what to do about it</span></span>
<span data-ttu-id="85be4-120">Hur programmet hanterar nyckelförnyelse beror på olika faktorer, till exempel typ av program eller vilka identity-protokollet och biblioteket har använts.</span><span class="sxs-lookup"><span data-stu-id="85be4-120">How your application handles key rollover depends on variables such as the type of application or what identity protocol and library was used.</span></span> <span data-ttu-id="85be4-121">I avsnitten nedan bedöma om de vanligaste typerna av program som påverkas av nyckelförnyelse och ge vägledning om hur du uppdaterar programmet för att stödja automatisk förnyelse eller manuellt uppdatera nyckeln.</span><span class="sxs-lookup"><span data-stu-id="85be4-121">The sections below assess whether the most common types of applications are impacted by the key rollover and provide guidance on how to update the application to support automatic rollover or manually update the key.</span></span>

* [<span data-ttu-id="85be4-122">Intern klientprogram att komma åt resurser</span><span class="sxs-lookup"><span data-stu-id="85be4-122">Native client applications accessing resources</span></span>](#nativeclient)
* [<span data-ttu-id="85be4-123">Webbprogram / API: er som har åtkomst till resurser</span><span class="sxs-lookup"><span data-stu-id="85be4-123">Web applications / APIs accessing resources</span></span>](#webclient)
* [<span data-ttu-id="85be4-124">Webbprogram / API: er skydda resurser och skapats med Azure App Service</span><span class="sxs-lookup"><span data-stu-id="85be4-124">Web applications / APIs protecting resources and built using Azure App Services</span></span>](#appservices)
* [<span data-ttu-id="85be4-125">Webbprogram / skydda resurser med hjälp av .NET OWIN OpenID Connect, WS-Fed eller WindowsAzureActiveDirectoryBearerAuthentication mellanprogram-API: er</span><span class="sxs-lookup"><span data-stu-id="85be4-125">Web applications / APIs protecting resources using .NET OWIN OpenID Connect, WS-Fed or WindowsAzureActiveDirectoryBearerAuthentication middleware</span></span>](#owin)
* [<span data-ttu-id="85be4-126">Webbprogram / skydda resurser med hjälp av .NET Core OpenID Connect eller JwtBearerAuthentication mellanprogram-API: er</span><span class="sxs-lookup"><span data-stu-id="85be4-126">Web applications / APIs protecting resources using .NET Core OpenID Connect or  JwtBearerAuthentication middleware</span></span>](#owincore)
* [<span data-ttu-id="85be4-127">Webbprogram / skydda resurser med hjälp av passport-azure-ad-modulen för Node.js-API: er</span><span class="sxs-lookup"><span data-stu-id="85be4-127">Web applications / APIs protecting resources using Node.js passport-azure-ad module</span></span>](#passport)
* [<span data-ttu-id="85be4-128">Webbprogram / API: er skydda resurser och skapats med Visual Studio 2015 eller Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="85be4-128">Web applications / APIs protecting resources and created with Visual Studio 2015 or Visual Studio 2017</span></span>](#vs2015)
* [<span data-ttu-id="85be4-129">Webbprogram skydda resurser och skapas med Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="85be4-129">Web applications protecting resources and created with Visual Studio 2013</span></span>](#vs2013)
* [<span data-ttu-id="85be4-130">Webb-API: er skydda resurser och skapas med Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="85be4-130">Web APIs protecting resources and created with Visual Studio 2013</span></span>](#vs2013_webapi)
* [<span data-ttu-id="85be4-131">Webbprogram skydda resurser och skapas med Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="85be4-131">Web applications protecting resources and created with Visual Studio 2012</span></span>](#vs2012)
* [<span data-ttu-id="85be4-132">Webbprogram skydda resurser och skapas med Visual Studio 2010, 2008-o med hjälp av Windows Identity Foundation</span><span class="sxs-lookup"><span data-stu-id="85be4-132">Web applications protecting resources and created with Visual Studio 2010, 2008 o using Windows Identity Foundation</span></span>](#vs2010)
* [<span data-ttu-id="85be4-133">Webbprogram / API: er som skyddar resurser med andra bibliotek eller manuellt använda några av protokoll som stöds</span><span class="sxs-lookup"><span data-stu-id="85be4-133">Web applications / APIs protecting resources using any other libraries or manually implementing any of the supported protocols</span></span>](#other)

<span data-ttu-id="85be4-134">Den här vägledningen är **inte** gäller för:</span><span class="sxs-lookup"><span data-stu-id="85be4-134">This guidance is **not** applicable for:</span></span>

* <span data-ttu-id="85be4-135">Program som har lagts till från Azure AD Application Gallery (inklusive anpassade) har särskilda riktlinjer med avseende på Signeringsnycklar.</span><span class="sxs-lookup"><span data-stu-id="85be4-135">Applications added from Azure AD Application Gallery (including Custom) have separate guidance with regards to signing keys.</span></span> [<span data-ttu-id="85be4-136">Mer information.</span><span class="sxs-lookup"><span data-stu-id="85be4-136">More information.</span></span>](../active-directory-sso-certs.md)
* <span data-ttu-id="85be4-137">Lokalt program som publicerats via programproxy inte behöver bry dig om Signeringsnycklar.</span><span class="sxs-lookup"><span data-stu-id="85be4-137">On-premises applications published via application proxy don't have to worry about signing keys.</span></span>

### <span data-ttu-id="85be4-138"><a name="nativeclient"></a>Intern klientprogram att komma åt resurser</span><span class="sxs-lookup"><span data-stu-id="85be4-138"><a name="nativeclient"></a>Native client applications accessing resources</span></span>
<span data-ttu-id="85be4-139">Program som kommer endast åt resurser (dvs</span><span class="sxs-lookup"><span data-stu-id="85be4-139">Applications that are only accessing resources (i.e</span></span> <span data-ttu-id="85be4-140">Microsoft Graph, KeyVault, API för Outlook och andra Microsoft-APIs) vanligtvis bara hämta en token och skickar den längs till resursägare.</span><span class="sxs-lookup"><span data-stu-id="85be4-140">Microsoft Graph, KeyVault, Outlook API and other Microsoft APIs) generally only obtain a token and pass it along to the resource owner.</span></span> <span data-ttu-id="85be4-141">Med tanke på att de inte skyddar alla resurser kan kontrollera inte token och därför behöver inte se till att den är korrekt signerad.</span><span class="sxs-lookup"><span data-stu-id="85be4-141">Given that they are not protecting any resources, they do not inspect the token and therefore do not need to ensure it is properly signed.</span></span>

<span data-ttu-id="85be4-142">Native client-program, om desktop eller mobile, tillhör den här kategorin och därför påverkas inte av förnyelsen.</span><span class="sxs-lookup"><span data-stu-id="85be4-142">Native client applications, whether desktop or mobile, fall into this category and are thus not impacted by the rollover.</span></span>

### <span data-ttu-id="85be4-143"><a name="webclient"></a>Webbprogram / API: er som har åtkomst till resurser</span><span class="sxs-lookup"><span data-stu-id="85be4-143"><a name="webclient"></a>Web applications / APIs accessing resources</span></span>
<span data-ttu-id="85be4-144">Program som kommer endast åt resurser (dvs</span><span class="sxs-lookup"><span data-stu-id="85be4-144">Applications that are only accessing resources (i.e</span></span> <span data-ttu-id="85be4-145">Microsoft Graph, KeyVault, API för Outlook och andra Microsoft-APIs) vanligtvis bara hämta en token och skickar den längs till resursägare.</span><span class="sxs-lookup"><span data-stu-id="85be4-145">Microsoft Graph, KeyVault, Outlook API and other Microsoft APIs) generally only obtain a token and pass it along to the resource owner.</span></span> <span data-ttu-id="85be4-146">Med tanke på att de inte skyddar alla resurser kan kontrollera inte token och därför behöver inte se till att den är korrekt signerad.</span><span class="sxs-lookup"><span data-stu-id="85be4-146">Given that they are not protecting any resources, they do not inspect the token and therefore do not need to ensure it is properly signed.</span></span>

<span data-ttu-id="85be4-147">Webbprogram och webb-API: er som använder endast app-flöde (klientens autentiseringsuppgifter / klientcertifikat), tillhör den här kategorin och därför inte påverkas av förnyelsen.</span><span class="sxs-lookup"><span data-stu-id="85be4-147">Web applications and web APIs that are using the app-only flow (client credentials / client certificate), fall into this category and are thus not impacted by the rollover.</span></span>

### <span data-ttu-id="85be4-148"><a name="appservices"></a>Webbprogram / API: er skydda resurser och skapats med Azure App Service</span><span class="sxs-lookup"><span data-stu-id="85be4-148"><a name="appservices"></a>Web applications / APIs protecting resources and built using Azure App Services</span></span>
<span data-ttu-id="85be4-149">Azure Apptjänst autentisering / auktorisering (EasyAuth)-funktioner redan har den nödvändiga logiken för att hantera nyckelförnyelse automatiskt.</span><span class="sxs-lookup"><span data-stu-id="85be4-149">Azure App Services' Authentication / Authorization (EasyAuth) functionality already has the necessary logic to handle key rollover automatically.</span></span>

### <span data-ttu-id="85be4-150"><a name="owin"></a>Webbprogram / skydda resurser med hjälp av .NET OWIN OpenID Connect, WS-Fed eller WindowsAzureActiveDirectoryBearerAuthentication mellanprogram-API: er</span><span class="sxs-lookup"><span data-stu-id="85be4-150"><a name="owin"></a>Web applications / APIs protecting resources using .NET OWIN OpenID Connect, WS-Fed or WindowsAzureActiveDirectoryBearerAuthentication middleware</span></span>
<span data-ttu-id="85be4-151">Om ditt program använder de .NET OWIN OpenID Connect, WS-Fed eller WindowsAzureActiveDirectoryBearerAuthentication mellanprogram har redan den nödvändiga logiken för att hantera nyckelförnyelse automatiskt.</span><span class="sxs-lookup"><span data-stu-id="85be4-151">If your application is using the .NET OWIN OpenID Connect, WS-Fed or WindowsAzureActiveDirectoryBearerAuthentication middleware, it already has the necessary logic to handle key rollover automatically.</span></span>

<span data-ttu-id="85be4-152">Du kan bekräfta att programmet använder någon av dessa genom att söka efter någon av följande kodavsnitt i ditt program Startup.cs eller Startup.Auth.cs</span><span class="sxs-lookup"><span data-stu-id="85be4-152">You can confirm that your application is using any of these by looking for any of the following snippets in your application's Startup.cs or Startup.Auth.cs</span></span>

```
app.UseOpenIdConnectAuthentication(
     new OpenIdConnectAuthenticationOptions
     {
         // ...
     });
```
```
app.UseWsFederationAuthentication(
    new WsFederationAuthenticationOptions
    {
     // ...
     });
```
```
 app.UseWindowsAzureActiveDirectoryBearerAuthentication(
     new WindowsAzureActiveDirectoryBearerAuthenticationOptions
     {
     // ...
     });
```

### <span data-ttu-id="85be4-153"><a name="owincore"></a>Webbprogram / skydda resurser med hjälp av .NET Core OpenID Connect eller JwtBearerAuthentication mellanprogram-API: er</span><span class="sxs-lookup"><span data-stu-id="85be4-153"><a name="owincore"></a>Web applications / APIs protecting resources using .NET Core OpenID Connect or  JwtBearerAuthentication middleware</span></span>
<span data-ttu-id="85be4-154">Om ditt program använder .NET Core OWIN OpenID Connect eller JwtBearerAuthentication mellanprogram, har redan den nödvändiga logiken för att hantera nyckelförnyelse automatiskt.</span><span class="sxs-lookup"><span data-stu-id="85be4-154">If your application is using the .NET Core OWIN OpenID Connect  or JwtBearerAuthentication middleware, it already has the necessary logic to handle key rollover automatically.</span></span>

<span data-ttu-id="85be4-155">Du kan bekräfta att programmet använder någon av dessa genom att söka efter någon av följande kodavsnitt i ditt program Startup.cs eller Startup.Auth.cs</span><span class="sxs-lookup"><span data-stu-id="85be4-155">You can confirm that your application is using any of these by looking for any of the following snippets in your application's Startup.cs or Startup.Auth.cs</span></span>

```
app.UseOpenIdConnectAuthentication(
     new OpenIdConnectAuthenticationOptions
     {
         // ...
     });
```
```
app.UseJwtBearerAuthentication(
    new JwtBearerAuthenticationOptions
    {
     // ...
     });
```

### <span data-ttu-id="85be4-156"><a name="passport"></a>Webbprogram / skydda resurser med hjälp av passport-azure-ad-modulen för Node.js-API: er</span><span class="sxs-lookup"><span data-stu-id="85be4-156"><a name="passport"></a>Web applications / APIs protecting resources using Node.js passport-azure-ad module</span></span>
<span data-ttu-id="85be4-157">Om ditt program använder Node.js passport-ad-modulen har redan den nödvändiga logiken för att hantera nyckelförnyelse automatiskt.</span><span class="sxs-lookup"><span data-stu-id="85be4-157">If your application is using the Node.js passport-ad module, it already has the necessary logic to handle key rollover automatically.</span></span>

<span data-ttu-id="85be4-158">Du kan bekräfta att din ansökan passport-ad genom att söka efter följande kodavsnitt i ditt program app.js</span><span class="sxs-lookup"><span data-stu-id="85be4-158">You can confirm that your application passport-ad by searching for the following snippet in your application's app.js</span></span>

```
var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

passport.use(new OIDCStrategy({
    //...
));
```

### <span data-ttu-id="85be4-159"><a name="vs2015"></a>Webbprogram / API: er skydda resurser och skapats med Visual Studio 2015 eller Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="85be4-159"><a name="vs2015"></a>Web applications / APIs protecting resources and created with Visual Studio 2015 or Visual Studio 2017</span></span>
<span data-ttu-id="85be4-160">Om programmet har skapats med en mall för program i Visual Studio 2015 eller Visual Studio 2017 och du har valt **arbets-och Skolkonton** från den **ändra autentisering** menyn den redan har den nödvändiga logiken för att hantera nyckelförnyelse automatiskt.</span><span class="sxs-lookup"><span data-stu-id="85be4-160">If your application was built using a web application template in Visual Studio 2015 or Visual Studio 2017 and you selected **Work And School Accounts** from the **Change Authentication** menu, it already has the necessary logic to handle key rollover automatically.</span></span> <span data-ttu-id="85be4-161">Den här logiken inbäddat i OWIN OpenID Connect-mellanprogram hämtar och cachelagrar nycklarna från OpenID Connect discovery-dokumentet och uppdaterar regelbundet hur dem.</span><span class="sxs-lookup"><span data-stu-id="85be4-161">This logic, embedded in the OWIN OpenID Connect middleware, retrieves and caches the keys from the OpenID Connect discovery document and periodically refreshes them.</span></span>

<span data-ttu-id="85be4-162">Om autentisering manuellt läggas till i din lösning kanske inte nödvändiga nyckelförnyelse logiken i ditt program.</span><span class="sxs-lookup"><span data-stu-id="85be4-162">If you added authentication to your solution manually, your application might not have the necessary key rollover logic.</span></span> <span data-ttu-id="85be4-163">Du behöver skriva den själv eller följer du stegen i [webbprogram / API: er med hjälp av andra bibliotek eller manuellt använda några av protokoll som stöds.](#other).</span><span class="sxs-lookup"><span data-stu-id="85be4-163">You will need to write it yourself, or follow the steps in [Web applications / APIs using any other libraries or manually implementing any of the supported protocols.](#other).</span></span>

### <span data-ttu-id="85be4-164"><a name="vs2013"></a>Webbprogram skydda resurser och skapas med Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="85be4-164"><a name="vs2013"></a>Web applications protecting resources and created with Visual Studio 2013</span></span>
<span data-ttu-id="85be4-165">Om programmet har skapats med en mall för program i Visual Studio 2013 och du har valt **Organisationskonton** från den **ändra autentisering** menyn den redan har den nödvändiga logiken för att hantera nyckelförnyelse automatiskt.</span><span class="sxs-lookup"><span data-stu-id="85be4-165">If your application was built using a web application template in Visual Studio 2013 and you selected **Organizational Accounts** from the **Change Authentication** menu, it already has the necessary logic to handle key rollover automatically.</span></span> <span data-ttu-id="85be4-166">Den här logiken lagrar Unik identifierare för din organisation och signering nyckelinformation i två databastabeller som är kopplade till projektet.</span><span class="sxs-lookup"><span data-stu-id="85be4-166">This logic stores your organization’s unique identifier and the signing key information in two database tables associated with the project.</span></span> <span data-ttu-id="85be4-167">Du hittar anslutningssträngen för databasen i projektets Web.config-fil.</span><span class="sxs-lookup"><span data-stu-id="85be4-167">You can find the connection string for the database in the project’s Web.config file.</span></span>

<span data-ttu-id="85be4-168">Om autentisering manuellt läggas till i din lösning kanske inte nödvändiga nyckelförnyelse logiken i ditt program.</span><span class="sxs-lookup"><span data-stu-id="85be4-168">If you added authentication to your solution manually, your application might not have the necessary key rollover logic.</span></span> <span data-ttu-id="85be4-169">Du behöver skriva den själv eller följer du stegen i [webbprogram / API: er med hjälp av andra bibliotek eller manuellt använda några av protokoll som stöds.](#other).</span><span class="sxs-lookup"><span data-stu-id="85be4-169">You will need to write it yourself, or follow the steps in [Web applications / APIs using any other libraries or manually implementing any of the supported protocols.](#other).</span></span>

<span data-ttu-id="85be4-170">Följande steg hjälper dig att kontrollera att logiken som fungerar i ditt program.</span><span class="sxs-lookup"><span data-stu-id="85be4-170">The following steps will help you verify that the logic is working properly in your application.</span></span>

1. <span data-ttu-id="85be4-171">Öppna lösningen i Visual Studio 2013 och klicka sedan på den **Server Explorer** fliken i det högra fönstret.</span><span class="sxs-lookup"><span data-stu-id="85be4-171">In Visual Studio 2013, open the solution, and then click on the **Server Explorer** tab on the right window.</span></span>
2. <span data-ttu-id="85be4-172">Expandera **dataanslutningar**, **DefaultConnection**, och sedan **tabeller**.</span><span class="sxs-lookup"><span data-stu-id="85be4-172">Expand **Data Connections**, **DefaultConnection**, and then **Tables**.</span></span> <span data-ttu-id="85be4-173">Leta upp den **IssuingAuthorityKeys** tabell, högerklicka på den och klicka sedan på **visa tabelldata**.</span><span class="sxs-lookup"><span data-stu-id="85be4-173">Locate the **IssuingAuthorityKeys** table, right-click it, and then click **Show Table Data**.</span></span>
3. <span data-ttu-id="85be4-174">I den **IssuingAuthorityKeys** tabell, ska det finnas minst en rad som motsvarar tumavtrycket värdet för nyckeln.</span><span class="sxs-lookup"><span data-stu-id="85be4-174">In the **IssuingAuthorityKeys** table, there will be at least one row, which corresponds to the thumbprint value for the key.</span></span> <span data-ttu-id="85be4-175">Ta bort alla rader i tabellen.</span><span class="sxs-lookup"><span data-stu-id="85be4-175">Delete any rows in the table.</span></span>
4. <span data-ttu-id="85be4-176">Högerklicka på den **klienter** tabell och klicka sedan på **visa tabelldata**.</span><span class="sxs-lookup"><span data-stu-id="85be4-176">Right-click the **Tenants** table, and then click **Show Table Data**.</span></span>
5. <span data-ttu-id="85be4-177">I den **hyresgäster** tabell, kommer det att minst en rad som motsvarar en unik katalog klient identifierare.</span><span class="sxs-lookup"><span data-stu-id="85be4-177">In the **Tenants** table, there will be at least one row, which corresponds to a unique directory tenant identifier.</span></span> <span data-ttu-id="85be4-178">Ta bort alla rader i tabellen.</span><span class="sxs-lookup"><span data-stu-id="85be4-178">Delete any rows in the table.</span></span> <span data-ttu-id="85be4-179">Om du inte tar bort raderna i både den **klienter** tabell och **IssuingAuthorityKeys** tabell, du får ett fel vid körning.</span><span class="sxs-lookup"><span data-stu-id="85be4-179">If you don't delete the rows in both the **Tenants** table and **IssuingAuthorityKeys** table, you will get an error at runtime.</span></span>
6. <span data-ttu-id="85be4-180">Skapa och köra programmet.</span><span class="sxs-lookup"><span data-stu-id="85be4-180">Build and run the application.</span></span> <span data-ttu-id="85be4-181">Du kan stoppa programmet när du har loggat in till ditt konto.</span><span class="sxs-lookup"><span data-stu-id="85be4-181">After you have logged in to your account, you can stop the application.</span></span>
7. <span data-ttu-id="85be4-182">Gå tillbaka till den **Server Explorer** och titta på värdena i den **IssuingAuthorityKeys** och **hyresgäster** tabell.</span><span class="sxs-lookup"><span data-stu-id="85be4-182">Return to the **Server Explorer** and look at the values in the **IssuingAuthorityKeys** and **Tenants** table.</span></span> <span data-ttu-id="85be4-183">Lägg märke till att de har varit automatiskt fylls med lämplig information från federation Metadatadokumentet.</span><span class="sxs-lookup"><span data-stu-id="85be4-183">You’ll notice that they have been automatically repopulated with the appropriate information from the federation metadata document.</span></span>

### <span data-ttu-id="85be4-184"><a name="vs2013"></a>Webb-API: er skydda resurser och skapas med Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="85be4-184"><a name="vs2013"></a>Web APIs protecting resources and created with Visual Studio 2013</span></span>
<span data-ttu-id="85be4-185">Om du har skapat ett webb-API-program i Visual Studio 2013 med hjälp av Web API-mall och sedan markerade **Organisationskonton** från den **ändra autentisering** menyn du redan har den nödvändiga logiken i ditt program.</span><span class="sxs-lookup"><span data-stu-id="85be4-185">If you created a web API application in Visual Studio 2013 using the Web API template, and then selected **Organizational Accounts** from the **Change Authentication** menu, you already have the necessary logic in your application.</span></span>

<span data-ttu-id="85be4-186">Om du har manuellt konfigurerade autentisering, följer du anvisningarna nedan för att lära dig hur du konfigurerar dina webb-API för att automatiskt uppdatera dess viktig information.</span><span class="sxs-lookup"><span data-stu-id="85be4-186">If you manually configured authentication, follow the instructions below to learn how to configure your Web API to automatically update its key information.</span></span>

<span data-ttu-id="85be4-187">Följande kodavsnitt visar hur du hämta de senaste nycklarna från federation metadata dokument och sedan använda den [JWT-Token hanteraren](https://msdn.microsoft.com/library/dn205065.aspx) att validera token.</span><span class="sxs-lookup"><span data-stu-id="85be4-187">The following code snippet demonstrates how to get the latest keys from the federation metadata document, and then use the [JWT Token Handler](https://msdn.microsoft.com/library/dn205065.aspx) to validate the token.</span></span> <span data-ttu-id="85be4-188">Kodfragmentet förutsätter att du ska använda egna cachelagringsmekanism för för att spara nyckeln för att validera framtida token från Azure AD, oavsett om den är i en databas, konfigurationsfilen eller någon annanstans.</span><span class="sxs-lookup"><span data-stu-id="85be4-188">The code snippet assumes that you will use your own caching mechanism for persisting the key to validate future tokens from Azure AD, whether it be in a database, configuration file, or elsewhere.</span></span>

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.IdentityModel.Tokens;
using System.Configuration;
using System.Security.Cryptography.X509Certificates;
using System.Xml;
using System.IdentityModel.Metadata;
using System.ServiceModel.Security;
using System.Threading;

namespace JWTValidation
{
    public class JWTValidator
    {
        private string MetadataAddress = "[Your Federation Metadata document address goes here]";

        // Validates the JWT Token that's part of the Authorization header in an HTTP request.
        public void ValidateJwtToken(string token)
        {
            JwtSecurityTokenHandler tokenHandler = new JwtSecurityTokenHandler()
            {
                // Do not disable for production code
                CertificateValidator = X509CertificateValidator.None
            };

            TokenValidationParameters validationParams = new TokenValidationParameters()
            {
                AllowedAudience = "[Your App ID URI goes here, as registered in the Azure Classic Portal]",
                ValidIssuer = "[The issuer for the token goes here, such as https://sts.windows.net/68b98905-130e-4d7c-b6e1-a158a9ed8449/]",
                SigningTokens = GetSigningCertificates(MetadataAddress)

                // Cache the signing tokens by your desired mechanism
            };

            Thread.CurrentPrincipal = tokenHandler.ValidateToken(token, validationParams);
        }

        // Returns a list of certificates from the specified metadata document.
        public List<X509SecurityToken> GetSigningCertificates(string metadataAddress)
        {
            List<X509SecurityToken> tokens = new List<X509SecurityToken>();

            if (metadataAddress == null)
            {
                throw new ArgumentNullException(metadataAddress);
            }

            using (XmlReader metadataReader = XmlReader.Create(metadataAddress))
            {
                MetadataSerializer serializer = new MetadataSerializer()
                {
                    // Do not disable for production code
                    CertificateValidationMode = X509CertificateValidationMode.None
                };

                EntityDescriptor metadata = serializer.ReadMetadata(metadataReader) as EntityDescriptor;

                if (metadata != null)
                {
                    SecurityTokenServiceDescriptor stsd = metadata.RoleDescriptors.OfType<SecurityTokenServiceDescriptor>().First();

                    if (stsd != null)
                    {
                        IEnumerable<X509RawDataKeyIdentifierClause> x509DataClauses = stsd.Keys.Where(key => key.KeyInfo != null && (key.Use == KeyType.Signing || key.Use == KeyType.Unspecified)).
                                                             Select(key => key.KeyInfo.OfType<X509RawDataKeyIdentifierClause>().First());

                        tokens.AddRange(x509DataClauses.Select(token => new X509SecurityToken(new X509Certificate2(token.GetX509RawData()))));
                    }
                    else
                    {
                        throw new InvalidOperationException("There is no RoleDescriptor of type SecurityTokenServiceType in the metadata");
                    }
                }
                else
                {
                    throw new Exception("Invalid Federation Metadata document");
                }
            }
            return tokens;
        }
    }
}
```

### <span data-ttu-id="85be4-189"><a name="vs2012"></a>Webbprogram skydda resurser och skapas med Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="85be4-189"><a name="vs2012"></a>Web applications protecting resources and created with Visual Studio 2012</span></span>
<span data-ttu-id="85be4-190">Om programmet har skapats i Visual Studio 2012, används du förmodligen identitet och åtkomst för att konfigurera ditt program.</span><span class="sxs-lookup"><span data-stu-id="85be4-190">If your application was built in Visual Studio 2012, you probably used the Identity and Access Tool to configure your application.</span></span> <span data-ttu-id="85be4-191">Det är också troligt att du använder den [verifierar Utfärdarens namn registret (VINR)](https://msdn.microsoft.com/library/dn205067.aspx).</span><span class="sxs-lookup"><span data-stu-id="85be4-191">It’s also likely that you are using the [Validating Issuer Name Registry (VINR)](https://msdn.microsoft.com/library/dn205067.aspx).</span></span> <span data-ttu-id="85be4-192">VINR ansvarar för att underhålla information om betrodda identitetsleverantörer (Azure AD) och nycklarna som används för att validera token som utfärdats av dem.</span><span class="sxs-lookup"><span data-stu-id="85be4-192">The VINR is responsible for maintaining information about trusted identity providers (Azure AD) and the keys used to validate tokens issued by them.</span></span> <span data-ttu-id="85be4-193">VINR gör det också enkelt att automatiskt uppdatera viktig information lagras i en Web.config-fil genom att hämta den senaste federation metadata dokument som är kopplade till din katalog kontrollerar om konfigurationen är inaktuell med senaste dokumentet och uppdatera programmet så att den nya nyckeln som behövs.</span><span class="sxs-lookup"><span data-stu-id="85be4-193">The VINR also makes it easy to automatically update the key information stored in a Web.config file by downloading the latest federation metadata document associated with your directory, checking if the configuration is out of date with the latest document, and updating the application to use the new key as necessary.</span></span>

<span data-ttu-id="85be4-194">Om du har skapat ditt program med hjälp av kodexempel eller genomgången dokumentationen från Microsoft ingår redan nyckelförnyelse logiken i projektet.</span><span class="sxs-lookup"><span data-stu-id="85be4-194">If you created your application using any of the code samples or walkthrough documentation provided by Microsoft, the key rollover logic is already included in your project.</span></span> <span data-ttu-id="85be4-195">Du kommer att märka att det redan finns koden nedan i projektet.</span><span class="sxs-lookup"><span data-stu-id="85be4-195">You will notice that the code below already exists in your project.</span></span> <span data-ttu-id="85be4-196">Om programmet inte redan har denna logik följer du stegen nedan för att lägga till den och kontrollera att den fungerar korrekt.</span><span class="sxs-lookup"><span data-stu-id="85be4-196">If your application does not already have this logic, follow the steps below to add it and to verify that it’s working correctly.</span></span>

1. <span data-ttu-id="85be4-197">I **Solution Explorer**, lägga till en referens till den **System.IdentityModel** sammansättningen för rätt projekt.</span><span class="sxs-lookup"><span data-stu-id="85be4-197">In **Solution Explorer**, add a reference to the **System.IdentityModel** assembly for the appropriate project.</span></span>
2. <span data-ttu-id="85be4-198">Öppna den **Global.asax.cs** och Lägg till följande med hjälp av direktiven:</span><span class="sxs-lookup"><span data-stu-id="85be4-198">Open the **Global.asax.cs** file and add the following using directives:</span></span>
   ```
   using System.Configuration;
   using System.IdentityModel.Tokens;
   ```
3. <span data-ttu-id="85be4-199">Lägg till följande metod för att den **Global.asax.cs** fil:</span><span class="sxs-lookup"><span data-stu-id="85be4-199">Add the following method to the **Global.asax.cs** file:</span></span>
   ```
   protected void RefreshValidationSettings()
   {
    string configPath = AppDomain.CurrentDomain.BaseDirectory + "\\" + "Web.config";
    string metadataAddress =
                  ConfigurationManager.AppSettings["ida:FederationMetadataLocation"];
    ValidatingIssuerNameRegistry.WriteToConfig(metadataAddress, configPath);
   }
   ```
4. <span data-ttu-id="85be4-200">Anropa den **RefreshValidationSettings()** metod i den **Application_Start()** metod i **Global.asax.cs** som visas:</span><span class="sxs-lookup"><span data-stu-id="85be4-200">Invoke the **RefreshValidationSettings()** method in the **Application_Start()** method in **Global.asax.cs** as shown:</span></span>
   ```
   protected void Application_Start()
   {
    AreaRegistration.RegisterAllAreas();
    ...
    RefreshValidationSettings();
   }
   ```

<span data-ttu-id="85be4-201">När du har följt de här stegen kan uppdateras programmets Web.config med den senaste informationen från federation metadata dokument, inklusive de senaste nycklarna.</span><span class="sxs-lookup"><span data-stu-id="85be4-201">Once you have followed these steps, your application’s Web.config will be updated with the latest information from the federation metadata document, including the latest keys.</span></span> <span data-ttu-id="85be4-202">Den här uppdateringen sker varje gång en programpool återanvänds i IIS. IIS är som standard att återanvända program var 29: e timme.</span><span class="sxs-lookup"><span data-stu-id="85be4-202">This update will occur every time your application pool recycles in IIS; by default IIS is set to recycle applications every 29 hours.</span></span>

<span data-ttu-id="85be4-203">Följ stegen nedan för att kontrollera att nyckelförnyelse logiken fungerar.</span><span class="sxs-lookup"><span data-stu-id="85be4-203">Follow the steps below to verify that the key rollover logic is working.</span></span>

1. <span data-ttu-id="85be4-204">När du har kontrollerat att programmet använder koden ovan, öppna den **Web.config** fil och navigera till den  **<issuerNameRegistry>**  block, särskilt söker efter följande några rader:</span><span class="sxs-lookup"><span data-stu-id="85be4-204">After you have verified that your application is using the code above, open the **Web.config** file and navigate to the **<issuerNameRegistry>** block, specifically looking for the following few lines:</span></span>
   ```
   <issuerNameRegistry type="System.IdentityModel.Tokens.ValidatingIssuerNameRegistry, System.IdentityModel.Tokens.ValidatingIssuerNameRegistry">
        <authority name="https://sts.windows.net/ec4187af-07da-4f01-b18f-64c2f5abecea/">
          <keys>
            <add thumbprint="3A38FA984E8560F19AADC9F86FE9594BB6AD049B" />
          </keys>
   ```
2. <span data-ttu-id="85be4-205">I den  **<add thumbprint=””>**  inställningen genom att ändra värdet tumavtryck genom att ersätta ett tecken med ett annat namn.</span><span class="sxs-lookup"><span data-stu-id="85be4-205">In the **<add thumbprint=””>** setting, change the thumbprint value by replacing any character with a different one.</span></span> <span data-ttu-id="85be4-206">Spara den **Web.config** fil.</span><span class="sxs-lookup"><span data-stu-id="85be4-206">Save the **Web.config** file.</span></span>
3. <span data-ttu-id="85be4-207">Skapa programmet och sedan köra den.</span><span class="sxs-lookup"><span data-stu-id="85be4-207">Build the application, and then run it.</span></span> <span data-ttu-id="85be4-208">Om du kan slutföra processen inloggning, uppdateras har nyckeln genom att hämta nödvändig information från din katalog federation metadata dokument i ditt program.</span><span class="sxs-lookup"><span data-stu-id="85be4-208">If you can complete the sign-in process, your application is successfully updating the key by downloading the required information from your directory’s federation metadata document.</span></span> <span data-ttu-id="85be4-209">Om du har problem att logga in, kontrollera ändringarna i ditt program är korrekta genom att läsa den [att lägga till inloggning till din webbserver program med hjälp av Azure AD](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) avsnittet eller hämta och kontrollera följande kodexempel: [Molnapp för flera innehavare för Azure Active Directory](https://code.msdn.microsoft.com/multi-tenant-cloud-8015b84b).</span><span class="sxs-lookup"><span data-stu-id="85be4-209">If you are having issues signing in, ensure the changes in your application are correct by reading the [Adding Sign-On to Your Web Application Using Azure AD](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) topic, or downloading and inspecting the following code sample: [Multi-Tenant Cloud Application for Azure Active Directory](https://code.msdn.microsoft.com/multi-tenant-cloud-8015b84b).</span></span>

### <span data-ttu-id="85be4-210"><a name="vs2010"></a>Webbprogram skydda resurser och skapas med Visual Studio 2008 eller 2010 och Windows Identity Foundation (WIF) version 1.0 för .NET 3.5</span><span class="sxs-lookup"><span data-stu-id="85be4-210"><a name="vs2010"></a>Web applications protecting resources and created with Visual Studio 2008 or 2010 and Windows Identity Foundation (WIF) v1.0 for .NET 3.5</span></span>
<span data-ttu-id="85be4-211">Om du har skapat ett program på WIF v1.0 finns inga angivna mekanism för att automatiskt uppdatera ditt programs konfiguration om du vill använda en ny nyckel.</span><span class="sxs-lookup"><span data-stu-id="85be4-211">If you built an application on WIF v1.0, there is no provided mechanism to automatically refresh your application’s configuration to use a new key.</span></span>

* <span data-ttu-id="85be4-212">*Enklast* använder FedUtil-verktygsuppsättning som ingår i WIF SDK, som kan hämta den senaste Metadatadokumentet och uppdatera din konfiguration.</span><span class="sxs-lookup"><span data-stu-id="85be4-212">*Easiest way* Use the FedUtil tooling included in the WIF SDK, which can retrieve the latest metadata document and update your configuration.</span></span>
* <span data-ttu-id="85be4-213">Uppdatera ditt program till .NET 4.5, som innehåller den senaste versionen av WIF finns i System-namnområdet.</span><span class="sxs-lookup"><span data-stu-id="85be4-213">Update your application to .NET 4.5, which includes the newest version of WIF located in the System namespace.</span></span> <span data-ttu-id="85be4-214">Du kan sedan använda den [verifierar Utfärdarens namn registret (VINR)](https://msdn.microsoft.com/library/dn205067.aspx) att utföra automatiska uppdateringar av programmets konfiguration.</span><span class="sxs-lookup"><span data-stu-id="85be4-214">You can then use the [Validating Issuer Name Registry (VINR)](https://msdn.microsoft.com/library/dn205067.aspx) to perform automatic updates of the application’s configuration.</span></span>
* <span data-ttu-id="85be4-215">Utföra en manuell förnyelse enligt anvisningarna i slutet av dokumentet vägledning.</span><span class="sxs-lookup"><span data-stu-id="85be4-215">Perform a manual rollover as per the instructions at the end of this guidance document.</span></span>

<span data-ttu-id="85be4-216">Instruktioner för att använda FedUtil för att uppdatera konfigurationen:</span><span class="sxs-lookup"><span data-stu-id="85be4-216">Instructions to use the FedUtil to update your configuration:</span></span>

1. <span data-ttu-id="85be4-217">Kontrollera att du har WIF v1.0 SDK är installerat på utvecklingsdatorn för Visual Studio 2008 eller 2010.</span><span class="sxs-lookup"><span data-stu-id="85be4-217">Verify that you have the WIF v1.0 SDK installed on your development machine for Visual Studio 2008 or 2010.</span></span> <span data-ttu-id="85be4-218">Du kan [ladda ned det från den här](https://www.microsoft.com/en-us/download/details.aspx?id=4451) om du ännu inte har installerat den.</span><span class="sxs-lookup"><span data-stu-id="85be4-218">You can [download it from here](https://www.microsoft.com/en-us/download/details.aspx?id=4451) if you have not yet installed it.</span></span>
2. <span data-ttu-id="85be4-219">Öppna en lösning i Visual Studio och högerklicka på tillämpliga projektet och välj **uppdatera federationsmetadata**.</span><span class="sxs-lookup"><span data-stu-id="85be4-219">In Visual Studio, open the solution, and then right-click the applicable project and select **Update federation metadata**.</span></span> <span data-ttu-id="85be4-220">Om det här alternativet inte är tillgängligt har FedUtil och/eller WIF v1.0 SDK inte installerats.</span><span class="sxs-lookup"><span data-stu-id="85be4-220">If this option is not available, FedUtil and/or the WIF v1.0 SDK has not been installed.</span></span>
3. <span data-ttu-id="85be4-221">Välj prompten **uppdatering** ska börja uppdatera din federationsmetadata.</span><span class="sxs-lookup"><span data-stu-id="85be4-221">From the prompt, select **Update** to begin updating your federation metadata.</span></span> <span data-ttu-id="85be4-222">Om du har åtkomst till servermiljö där programmet körs kan du också använda Fedutil's [uppdateringsschema för automatisk metadata](https://msdn.microsoft.com/library/ee517272.aspx).</span><span class="sxs-lookup"><span data-stu-id="85be4-222">If you have access to the server environment where the application is hosted, you can optionally use FedUtil’s [automatic metadata update scheduler](https://msdn.microsoft.com/library/ee517272.aspx).</span></span>
4. <span data-ttu-id="85be4-223">Klicka på **Slutför** att slutföra uppdateringsprocessen.</span><span class="sxs-lookup"><span data-stu-id="85be4-223">Click **Finish** to complete the update process.</span></span>

### <span data-ttu-id="85be4-224"><a name="other"></a>Webbprogram / API: er som skyddar resurser med andra bibliotek eller manuellt använda några av protokoll som stöds</span><span class="sxs-lookup"><span data-stu-id="85be4-224"><a name="other"></a>Web applications / APIs protecting resources using any other libraries or manually implementing any of the supported protocols</span></span>
<span data-ttu-id="85be4-225">Om du använder vissa andra bibliotek eller manuellt implementerats någon av protokoll som stöds, behöver du granska biblioteket eller implementeringen så att nyckeln hämtas från OpenID Connect discovery-dokumentet eller federation Metadatadokumentet.</span><span class="sxs-lookup"><span data-stu-id="85be4-225">If you are using some other library or manually implemented any of the supported protocols, you'll need to review the library or your implementation to ensure that the key is being retrieved from either the OpenID Connect discovery document or the federation metadata document.</span></span> <span data-ttu-id="85be4-226">Ett sätt att kontrollera om det är att göra en sökning i koden eller bibliotekets kod efter samtal ut till OpenID discovery-dokumentet eller federation Metadatadokumentet.</span><span class="sxs-lookup"><span data-stu-id="85be4-226">One way to check for this is to do a search in your code or the library's code for any calls out to either the OpenID discovery document or the federation metadata document.</span></span>

<span data-ttu-id="85be4-227">Hämta i nyckeln och uppdatera den i enlighet med detta genom att utföra en manuell förnyelse enligt anvisningarna i slutet av dokumentet vägledning om de nycklar lagras någonstans eller hårdkodad i ditt program kan du manuellt.</span><span class="sxs-lookup"><span data-stu-id="85be4-227">If they key is being stored somewhere or hardcoded in your application, you can manually retrieve the key and update it accordingly by perform a manual rollover as per the instructions at the end of this guidance document.</span></span> <span data-ttu-id="85be4-228">**Det rekommenderas starkt att du förbättra programmet att stödja automatisk förnyelse** med någon av metoderna disposition i den här artikeln för att undvika framtida avbrott och omkostnader om Azure AD ökar dess förnyade takt eller har ett nödfall out-of-band-förnyelse.</span><span class="sxs-lookup"><span data-stu-id="85be4-228">**It is strongly encouraged that you enhance your application to support automatic rollover** using any of the approaches outline in this article to avoid future disruptions and overhead if Azure AD increases it's rollover cadence or has an emergency out-of-band rollover.</span></span>

## <a name="how-to-test-your-application-to-determine-if-it-will-be-affected"></a><span data-ttu-id="85be4-229">Hur du testar programmet att fastställa om det kommer att påverkas</span><span class="sxs-lookup"><span data-stu-id="85be4-229">How to test your application to determine if it will be affected</span></span>
<span data-ttu-id="85be4-230">Du kan kontrollera om ditt program stöder automatisk nyckelförnyelse genom att hämta skripten och följa anvisningarna i [GitHub-lagringsplatsen.](https://github.com/AzureAD/azure-activedirectory-powershell-tokenkey)</span><span class="sxs-lookup"><span data-stu-id="85be4-230">You can validate whether your application supports automatic key rollover by downloading the scripts and following the instructions in [this GitHub repository.](https://github.com/AzureAD/azure-activedirectory-powershell-tokenkey)</span></span>

## <a name="how-to-perform-a-manual-rollover-if-you-application-does-not-support-automatic-rollover"></a><span data-ttu-id="85be4-231">Hur du utför en manuell förnyelse om du programmet inte stöder automatisk förnyelse</span><span class="sxs-lookup"><span data-stu-id="85be4-231">How to perform a manual rollover if you application does not support automatic rollover</span></span>
<span data-ttu-id="85be4-232">Om programmet inte **inte** stöd för automatisk förnyelse, måste du upprätta en process som regelbundet övervakar Azure AD signering nycklar och utför en manuell förnyelse därefter.</span><span class="sxs-lookup"><span data-stu-id="85be4-232">If your application does **not** support automatic rollover, you will need to establish a process that periodically monitors Azure AD's signing keys and performs a manual rollover accordingly.</span></span> <span data-ttu-id="85be4-233">[Den här GitHub-lagringsplatsen](https://github.com/AzureAD/azure-activedirectory-powershell-tokenkey) innehåller skript och instruktioner om hur du gör detta.</span><span class="sxs-lookup"><span data-stu-id="85be4-233">[This GitHub repository](https://github.com/AzureAD/azure-activedirectory-powershell-tokenkey) contains scripts and instructions on how to do this.</span></span>

