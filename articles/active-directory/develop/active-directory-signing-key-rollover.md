---
title: "aaaSigning nyckel förnyelse i Azure AD | Microsoft Docs"
description: "Den här artikeln beskrivs hello signering nyckelförnyelse bästa praxis för Azure Active Directory"
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
ms.openlocfilehash: ac6ade7f3ba2fbd22ea6d447aa5d07a2d6bdd451
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="signing-key-rollover-in-azure-active-directory"></a><span data-ttu-id="2852e-103">Signering nyckelförnyelse i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="2852e-103">Signing key rollover in Azure Active Directory</span></span>
<span data-ttu-id="2852e-104">Det här avsnittet beskrivs vad du behöver tooknow om hello offentliga nycklar som används i Azure Active Directory (AD Azure) toosign säkerhetstoken.</span><span class="sxs-lookup"><span data-stu-id="2852e-104">This topic discusses what you need tooknow about hello public keys that are used in Azure Active Directory (Azure AD) toosign security tokens.</span></span> <span data-ttu-id="2852e-105">Det är viktigt toonote som dessa nycklar förnyelse regelbundet och, i nödfall, kan samlas över omedelbart.</span><span class="sxs-lookup"><span data-stu-id="2852e-105">It is important toonote that these keys rollover on a periodic basis and, in an emergency, could be rolled over immediately.</span></span> <span data-ttu-id="2852e-106">Alla program som använder Azure AD ska kunna tooprogrammatically referensen hello nyckelförnyelse processen eller upprätta en process som regelbundet manuell förnyelse.</span><span class="sxs-lookup"><span data-stu-id="2852e-106">All applications that use Azure AD should be able tooprogrammatically handle hello key rollover process or establish a periodic manual rollover process.</span></span> <span data-ttu-id="2852e-107">Fortsätta att läsa toounderstand hur hello fungerar, hur tooassess hello effekten av hello förnyelse tooyour program och tooupdate tillämpningsprogrammet eller upprätta nyckelförnyelse en periodiska manuell förnyelse processen toohandle om det behövs.</span><span class="sxs-lookup"><span data-stu-id="2852e-107">Continue reading toounderstand how hello keys work, how tooassess hello impact of hello rollover tooyour application and how tooupdate your application or establish a periodic manual rollover process toohandle key rollover if necessary.</span></span>

## <a name="overview-of-signing-keys-in-azure-ad"></a><span data-ttu-id="2852e-108">Översikt över Signeringsnycklar i Azure AD</span><span class="sxs-lookup"><span data-stu-id="2852e-108">Overview of signing keys in Azure AD</span></span>
<span data-ttu-id="2852e-109">Azure AD använder offentliga nycklar som bygger på branschen standarder tooestablish förtroende mellan sig själv och hello program som använder den.</span><span class="sxs-lookup"><span data-stu-id="2852e-109">Azure AD uses public-key cryptography built on industry standards tooestablish trust between itself and hello applications that use it.</span></span> <span data-ttu-id="2852e-110">I praktiken detta fungerar hello följande sätt: Azure AD använder en signeringsnyckel som består av en offentlig och privat nyckel.</span><span class="sxs-lookup"><span data-stu-id="2852e-110">In practical terms, this works in hello following way: Azure AD uses a signing key that consists of a public and private key pair.</span></span> <span data-ttu-id="2852e-111">När en användare loggar in tooan program som använder Azure AD för autentisering, skapar en säkerhetstoken som innehåller information om hello användare i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="2852e-111">When a user signs in tooan application that uses Azure AD for authentication, Azure AD creates a security token that contains information about hello user.</span></span> <span data-ttu-id="2852e-112">Denna token är signerat av Azure AD med hjälp av den privata nyckeln innan den skickas tillbaka toohello program.</span><span class="sxs-lookup"><span data-stu-id="2852e-112">This token is signed by Azure AD using its private key before it is sent back toohello application.</span></span> <span data-ttu-id="2852e-113">tooverify hello token är giltig och faktiskt har sitt ursprung från Azure AD, hello programmet måste verifiera hello token signatur med hjälp av hello offentlig nyckel som exponeras av Azure AD som ingår i hello klient [OpenID Connect discovery-dokumentet](http://openid.net/specs/openid-connect-discovery-1_0.html) eller SAML/WS-Fed [federation Metadatadokumentet](active-directory-federation-metadata.md).</span><span class="sxs-lookup"><span data-stu-id="2852e-113">tooverify that hello token is valid and actually originated from Azure AD, hello application must validate hello token’s signature using hello public key exposed by Azure AD that is contained in hello tenant’s [OpenID Connect discovery document](http://openid.net/specs/openid-connect-discovery-1_0.html) or SAML/WS-Fed [federation metadata document](active-directory-federation-metadata.md).</span></span>

<span data-ttu-id="2852e-114">Av säkerhetsskäl kunde Azure AD-signering nyckel samlar regelbundet och hello gäller en nödsituation, återställas omedelbart.</span><span class="sxs-lookup"><span data-stu-id="2852e-114">For security purposes, Azure AD’s signing key rolls on a periodic basis and, in hello case of an emergency, could be rolled over immediately.</span></span> <span data-ttu-id="2852e-115">Alla program som kan integreras med Azure AD bör förberedas toohandle en nyckelförnyelse händelse, oavsett hur ofta kan det uppstå.</span><span class="sxs-lookup"><span data-stu-id="2852e-115">Any application that integrates with Azure AD should be prepared toohandle a key rollover event no matter how frequently it may occur.</span></span> <span data-ttu-id="2852e-116">Om det inte, och försöker toouse en utgången viktiga tooverify hello signatur på en token för ditt program, misslyckas hello inloggning begäran.</span><span class="sxs-lookup"><span data-stu-id="2852e-116">If it doesn’t, and your application attempts toouse an expired key tooverify hello signature on a token, hello sign-in request will fail.</span></span>

<span data-ttu-id="2852e-117">Det finns alltid mer än en giltig nyckel i hello OpenID Connect discovery-dokumentet och hello federation metadata.</span><span class="sxs-lookup"><span data-stu-id="2852e-117">There is always more than one valid key available in hello OpenID Connect discovery document and hello federation metadata document.</span></span> <span data-ttu-id="2852e-118">Ditt program bör förberedas toouse någon hello nycklar anges i hello dokument, eftersom en nyckel kan återställas snart, en annan kan dess ersättare, och så vidare.</span><span class="sxs-lookup"><span data-stu-id="2852e-118">Your application should be prepared toouse any of hello keys specified in hello document, since one key may be rolled soon, another may be its replacement, and so forth.</span></span>

## <a name="how-tooassess-if-your-application-will-be-affected-and-what-toodo-about-it"></a><span data-ttu-id="2852e-119">Hur tooassess om ditt program kommer att påverkas och vilka toodo om det.</span><span class="sxs-lookup"><span data-stu-id="2852e-119">How tooassess if your application will be affected and what toodo about it</span></span>
<span data-ttu-id="2852e-120">Hur programmet hanterar nyckelförnyelse beror på olika faktorer, till exempel hello typ av program eller vilka identity-protokollet och biblioteket har använts.</span><span class="sxs-lookup"><span data-stu-id="2852e-120">How your application handles key rollover depends on variables such as hello type of application or what identity protocol and library was used.</span></span> <span data-ttu-id="2852e-121">hello avsnitten nedan bedöma om hello de vanligaste typerna av program som påverkas av hello nyckelförnyelse och ger riktlinjer för hur tooupdate hello programmet toosupport automatisk förnyelse eller manuellt uppdatera hello nyckeln.</span><span class="sxs-lookup"><span data-stu-id="2852e-121">hello sections below assess whether hello most common types of applications are impacted by hello key rollover and provide guidance on how tooupdate hello application toosupport automatic rollover or manually update hello key.</span></span>

* [<span data-ttu-id="2852e-122">Intern klientprogram att komma åt resurser</span><span class="sxs-lookup"><span data-stu-id="2852e-122">Native client applications accessing resources</span></span>](#nativeclient)
* [<span data-ttu-id="2852e-123">Webbprogram / API: er som har åtkomst till resurser</span><span class="sxs-lookup"><span data-stu-id="2852e-123">Web applications / APIs accessing resources</span></span>](#webclient)
* [<span data-ttu-id="2852e-124">Webbprogram / API: er skydda resurser och skapats med Azure App Service</span><span class="sxs-lookup"><span data-stu-id="2852e-124">Web applications / APIs protecting resources and built using Azure App Services</span></span>](#appservices)
* [<span data-ttu-id="2852e-125">Webbprogram / skydda resurser med hjälp av .NET OWIN OpenID Connect, WS-Fed eller WindowsAzureActiveDirectoryBearerAuthentication mellanprogram-API: er</span><span class="sxs-lookup"><span data-stu-id="2852e-125">Web applications / APIs protecting resources using .NET OWIN OpenID Connect, WS-Fed or WindowsAzureActiveDirectoryBearerAuthentication middleware</span></span>](#owin)
* [<span data-ttu-id="2852e-126">Webbprogram / skydda resurser med hjälp av .NET Core OpenID Connect eller JwtBearerAuthentication mellanprogram-API: er</span><span class="sxs-lookup"><span data-stu-id="2852e-126">Web applications / APIs protecting resources using .NET Core OpenID Connect or  JwtBearerAuthentication middleware</span></span>](#owincore)
* [<span data-ttu-id="2852e-127">Webbprogram / skydda resurser med hjälp av passport-azure-ad-modulen för Node.js-API: er</span><span class="sxs-lookup"><span data-stu-id="2852e-127">Web applications / APIs protecting resources using Node.js passport-azure-ad module</span></span>](#passport)
* [<span data-ttu-id="2852e-128">Webbprogram / API: er skydda resurser och skapats med Visual Studio 2015 eller Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="2852e-128">Web applications / APIs protecting resources and created with Visual Studio 2015 or Visual Studio 2017</span></span>](#vs2015)
* [<span data-ttu-id="2852e-129">Webbprogram skydda resurser och skapas med Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="2852e-129">Web applications protecting resources and created with Visual Studio 2013</span></span>](#vs2013)
* [<span data-ttu-id="2852e-130">Webb-API: er skydda resurser och skapas med Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="2852e-130">Web APIs protecting resources and created with Visual Studio 2013</span></span>](#vs2013_webapi)
* [<span data-ttu-id="2852e-131">Webbprogram skydda resurser och skapas med Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="2852e-131">Web applications protecting resources and created with Visual Studio 2012</span></span>](#vs2012)
* [<span data-ttu-id="2852e-132">Webbprogram skydda resurser och skapas med Visual Studio 2010, 2008-o med hjälp av Windows Identity Foundation</span><span class="sxs-lookup"><span data-stu-id="2852e-132">Web applications protecting resources and created with Visual Studio 2010, 2008 o using Windows Identity Foundation</span></span>](#vs2010)
* [<span data-ttu-id="2852e-133">Webbprogram / API: er som skyddar resurser med andra bibliotek eller implementera manuellt någon hello stöds protokoll</span><span class="sxs-lookup"><span data-stu-id="2852e-133">Web applications / APIs protecting resources using any other libraries or manually implementing any of hello supported protocols</span></span>](#other)

<span data-ttu-id="2852e-134">Den här vägledningen är **inte** gäller för:</span><span class="sxs-lookup"><span data-stu-id="2852e-134">This guidance is **not** applicable for:</span></span>

* <span data-ttu-id="2852e-135">Program som lagts till från Azure AD Application Gallery (inklusive anpassade) ha separata vägledning för angående toosigning nycklar.</span><span class="sxs-lookup"><span data-stu-id="2852e-135">Applications added from Azure AD Application Gallery (including Custom) have separate guidance with regards toosigning keys.</span></span> [<span data-ttu-id="2852e-136">Mer information.</span><span class="sxs-lookup"><span data-stu-id="2852e-136">More information.</span></span>](../active-directory-sso-certs.md)
* <span data-ttu-id="2852e-137">Lokalt program som publicerats via programproxy inte har tooworry om Signeringsnycklar.</span><span class="sxs-lookup"><span data-stu-id="2852e-137">On-premises applications published via application proxy don't have tooworry about signing keys.</span></span>

### <span data-ttu-id="2852e-138"><a name="nativeclient"></a>Intern klientprogram att komma åt resurser</span><span class="sxs-lookup"><span data-stu-id="2852e-138"><a name="nativeclient"></a>Native client applications accessing resources</span></span>
<span data-ttu-id="2852e-139">Program som kommer endast åt resurser (dvs</span><span class="sxs-lookup"><span data-stu-id="2852e-139">Applications that are only accessing resources (i.e</span></span> <span data-ttu-id="2852e-140">Microsoft Graph, KeyVault, API för Outlook och andra Microsoft-APIs) vanligtvis endast hämta en token och skickar vidare toohello resurs-ägare.</span><span class="sxs-lookup"><span data-stu-id="2852e-140">Microsoft Graph, KeyVault, Outlook API and other Microsoft APIs) generally only obtain a token and pass it along toohello resource owner.</span></span> <span data-ttu-id="2852e-141">Med tanke på att de inte skyddar några resurser, inspektera inte hello token och därför behöver inte tooensure som den är korrekt signerad.</span><span class="sxs-lookup"><span data-stu-id="2852e-141">Given that they are not protecting any resources, they do not inspect hello token and therefore do not need tooensure it is properly signed.</span></span>

<span data-ttu-id="2852e-142">Native client-program, om desktop eller mobile, tillhör den här kategorin och därför påverkas inte av hello förnyelse.</span><span class="sxs-lookup"><span data-stu-id="2852e-142">Native client applications, whether desktop or mobile, fall into this category and are thus not impacted by hello rollover.</span></span>

### <span data-ttu-id="2852e-143"><a name="webclient"></a>Webbprogram / API: er som har åtkomst till resurser</span><span class="sxs-lookup"><span data-stu-id="2852e-143"><a name="webclient"></a>Web applications / APIs accessing resources</span></span>
<span data-ttu-id="2852e-144">Program som kommer endast åt resurser (dvs</span><span class="sxs-lookup"><span data-stu-id="2852e-144">Applications that are only accessing resources (i.e</span></span> <span data-ttu-id="2852e-145">Microsoft Graph, KeyVault, API för Outlook och andra Microsoft-APIs) vanligtvis endast hämta en token och skickar vidare toohello resurs-ägare.</span><span class="sxs-lookup"><span data-stu-id="2852e-145">Microsoft Graph, KeyVault, Outlook API and other Microsoft APIs) generally only obtain a token and pass it along toohello resource owner.</span></span> <span data-ttu-id="2852e-146">Med tanke på att de inte skyddar några resurser, inspektera inte hello token och därför behöver inte tooensure som den är korrekt signerad.</span><span class="sxs-lookup"><span data-stu-id="2852e-146">Given that they are not protecting any resources, they do not inspect hello token and therefore do not need tooensure it is properly signed.</span></span>

<span data-ttu-id="2852e-147">Webbprogram och webb-API: er som använder hello endast app-flöde (klientens autentiseringsuppgifter / klientcertifikat), tillhör den här kategorin och därför inte påverkas av hello förnyelse.</span><span class="sxs-lookup"><span data-stu-id="2852e-147">Web applications and web APIs that are using hello app-only flow (client credentials / client certificate), fall into this category and are thus not impacted by hello rollover.</span></span>

### <span data-ttu-id="2852e-148"><a name="appservices"></a>Webbprogram / API: er skydda resurser och skapats med Azure App Service</span><span class="sxs-lookup"><span data-stu-id="2852e-148"><a name="appservices"></a>Web applications / APIs protecting resources and built using Azure App Services</span></span>
<span data-ttu-id="2852e-149">Azure Apptjänst autentisering / auktorisering (EasyAuth)-funktioner redan har nödvändiga logiken hello toohandle nyckelförnyelse automatiskt.</span><span class="sxs-lookup"><span data-stu-id="2852e-149">Azure App Services' Authentication / Authorization (EasyAuth) functionality already has hello necessary logic toohandle key rollover automatically.</span></span>

### <span data-ttu-id="2852e-150"><a name="owin"></a>Webbprogram / skydda resurser med hjälp av .NET OWIN OpenID Connect, WS-Fed eller WindowsAzureActiveDirectoryBearerAuthentication mellanprogram-API: er</span><span class="sxs-lookup"><span data-stu-id="2852e-150"><a name="owin"></a>Web applications / APIs protecting resources using .NET OWIN OpenID Connect, WS-Fed or WindowsAzureActiveDirectoryBearerAuthentication middleware</span></span>
<span data-ttu-id="2852e-151">Om ditt program använder hello .NET OWIN OpenID Connect, WS-Fed eller WindowsAzureActiveDirectoryBearerAuthentication mellanprogram har den redan nödvändiga logiken hello toohandle nyckelförnyelse automatiskt.</span><span class="sxs-lookup"><span data-stu-id="2852e-151">If your application is using hello .NET OWIN OpenID Connect, WS-Fed or WindowsAzureActiveDirectoryBearerAuthentication middleware, it already has hello necessary logic toohandle key rollover automatically.</span></span>

<span data-ttu-id="2852e-152">Du kan bekräfta att programmet använder någon av dessa genom att söka efter någon av följande kodavsnitt i ditt program Startup.cs eller Startup.Auth.cs hello</span><span class="sxs-lookup"><span data-stu-id="2852e-152">You can confirm that your application is using any of these by looking for any of hello following snippets in your application's Startup.cs or Startup.Auth.cs</span></span>

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

### <span data-ttu-id="2852e-153"><a name="owincore"></a>Webbprogram / skydda resurser med hjälp av .NET Core OpenID Connect eller JwtBearerAuthentication mellanprogram-API: er</span><span class="sxs-lookup"><span data-stu-id="2852e-153"><a name="owincore"></a>Web applications / APIs protecting resources using .NET Core OpenID Connect or  JwtBearerAuthentication middleware</span></span>
<span data-ttu-id="2852e-154">Om ditt program använder hello .NET Core OWIN OpenID Connect eller JwtBearerAuthentication mellanprogram, har den redan nödvändiga logiken hello toohandle nyckelförnyelse automatiskt.</span><span class="sxs-lookup"><span data-stu-id="2852e-154">If your application is using hello .NET Core OWIN OpenID Connect  or JwtBearerAuthentication middleware, it already has hello necessary logic toohandle key rollover automatically.</span></span>

<span data-ttu-id="2852e-155">Du kan bekräfta att programmet använder någon av dessa genom att söka efter någon av följande kodavsnitt i ditt program Startup.cs eller Startup.Auth.cs hello</span><span class="sxs-lookup"><span data-stu-id="2852e-155">You can confirm that your application is using any of these by looking for any of hello following snippets in your application's Startup.cs or Startup.Auth.cs</span></span>

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

### <span data-ttu-id="2852e-156"><a name="passport"></a>Webbprogram / skydda resurser med hjälp av passport-azure-ad-modulen för Node.js-API: er</span><span class="sxs-lookup"><span data-stu-id="2852e-156"><a name="passport"></a>Web applications / APIs protecting resources using Node.js passport-azure-ad module</span></span>
<span data-ttu-id="2852e-157">Om ditt program använder hello Node.js passport ad-modulen, har den redan nödvändiga logiken hello toohandle nyckelförnyelse automatiskt.</span><span class="sxs-lookup"><span data-stu-id="2852e-157">If your application is using hello Node.js passport-ad module, it already has hello necessary logic toohandle key rollover automatically.</span></span>

<span data-ttu-id="2852e-158">Du kan bekräfta att din ansökan passport-ad genom att söka efter hello följande kodavsnitt i ditt program app.js</span><span class="sxs-lookup"><span data-stu-id="2852e-158">You can confirm that your application passport-ad by searching for hello following snippet in your application's app.js</span></span>

```
var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

passport.use(new OIDCStrategy({
    //...
));
```

### <span data-ttu-id="2852e-159"><a name="vs2015"></a>Webbprogram / API: er skydda resurser och skapats med Visual Studio 2015 eller Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="2852e-159"><a name="vs2015"></a>Web applications / APIs protecting resources and created with Visual Studio 2015 or Visual Studio 2017</span></span>
<span data-ttu-id="2852e-160">Om programmet har skapats med en mall för program i Visual Studio 2015 eller Visual Studio 2017 och du har valt **arbets-och Skolkonton** från hello **ändra autentisering** menyn redan har nödvändiga logiken hello toohandle nyckelförnyelse automatiskt.</span><span class="sxs-lookup"><span data-stu-id="2852e-160">If your application was built using a web application template in Visual Studio 2015 or Visual Studio 2017 and you selected **Work And School Accounts** from hello **Change Authentication** menu, it already has hello necessary logic toohandle key rollover automatically.</span></span> <span data-ttu-id="2852e-161">Den här logiken inbäddat i hello OWIN OpenID Connect mellanprogram hämtar och cachelagrar hello nycklar från hello OpenID Connect discovery-dokumentet och uppdaterar regelbundet hur dem.</span><span class="sxs-lookup"><span data-stu-id="2852e-161">This logic, embedded in hello OWIN OpenID Connect middleware, retrieves and caches hello keys from hello OpenID Connect discovery document and periodically refreshes them.</span></span>

<span data-ttu-id="2852e-162">Om du har lagt till tooyour autentiseringslösning manuellt, kanske inte programmet hello nödvändiga nyckelförnyelse logik.</span><span class="sxs-lookup"><span data-stu-id="2852e-162">If you added authentication tooyour solution manually, your application might not have hello necessary key rollover logic.</span></span> <span data-ttu-id="2852e-163">Du behöver toowrite den själv eller hello Följ stegen i [webbprogram / API: er med hjälp av andra bibliotek eller implementera manuellt någon hello stöds protokoll.](#other).</span><span class="sxs-lookup"><span data-stu-id="2852e-163">You will need toowrite it yourself, or follow hello steps in [Web applications / APIs using any other libraries or manually implementing any of hello supported protocols.](#other).</span></span>

### <span data-ttu-id="2852e-164"><a name="vs2013"></a>Webbprogram skydda resurser och skapas med Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="2852e-164"><a name="vs2013"></a>Web applications protecting resources and created with Visual Studio 2013</span></span>
<span data-ttu-id="2852e-165">Om programmet har skapats med en mall för program i Visual Studio 2013 och du har valt **Organisationskonton** från hello **ändra autentisering** menyn har redan hello behov logik toohandle key förnyelse automatiskt.</span><span class="sxs-lookup"><span data-stu-id="2852e-165">If your application was built using a web application template in Visual Studio 2013 and you selected **Organizational Accounts** from hello **Change Authentication** menu, it already has hello necessary logic toohandle key rollover automatically.</span></span> <span data-ttu-id="2852e-166">Den här logiken lagrar Unik identifierare för din organisation och hello signering nyckelinformation i två databastabeller som är associerade med hello-projekt.</span><span class="sxs-lookup"><span data-stu-id="2852e-166">This logic stores your organization’s unique identifier and hello signing key information in two database tables associated with hello project.</span></span> <span data-ttu-id="2852e-167">Du hittar hello anslutningssträngen för databasen hello i hello projektet Web.config-filen.</span><span class="sxs-lookup"><span data-stu-id="2852e-167">You can find hello connection string for hello database in hello project’s Web.config file.</span></span>

<span data-ttu-id="2852e-168">Om du har lagt till tooyour autentiseringslösning manuellt, kanske inte programmet hello nödvändiga nyckelförnyelse logik.</span><span class="sxs-lookup"><span data-stu-id="2852e-168">If you added authentication tooyour solution manually, your application might not have hello necessary key rollover logic.</span></span> <span data-ttu-id="2852e-169">Du behöver toowrite den själv eller hello Följ stegen i [webbprogram / API: er med hjälp av andra bibliotek eller implementera manuellt någon hello stöds protokoll.](#other).</span><span class="sxs-lookup"><span data-stu-id="2852e-169">You will need toowrite it yourself, or follow hello steps in [Web applications / APIs using any other libraries or manually implementing any of hello supported protocols.](#other).</span></span>

<span data-ttu-id="2852e-170">hello följande steg hjälper dig att kontrollera att hello logik fungerar korrekt i ditt program.</span><span class="sxs-lookup"><span data-stu-id="2852e-170">hello following steps will help you verify that hello logic is working properly in your application.</span></span>

1. <span data-ttu-id="2852e-171">Öppna hello lösningen i Visual Studio 2013 och klicka sedan på hello **Server Explorer** fliken hello högra fönstret.</span><span class="sxs-lookup"><span data-stu-id="2852e-171">In Visual Studio 2013, open hello solution, and then click on hello **Server Explorer** tab on hello right window.</span></span>
2. <span data-ttu-id="2852e-172">Expandera **dataanslutningar**, **DefaultConnection**, och sedan **tabeller**.</span><span class="sxs-lookup"><span data-stu-id="2852e-172">Expand **Data Connections**, **DefaultConnection**, and then **Tables**.</span></span> <span data-ttu-id="2852e-173">Leta upp hello **IssuingAuthorityKeys** tabell, högerklicka på den och klicka sedan på **visa tabelldata**.</span><span class="sxs-lookup"><span data-stu-id="2852e-173">Locate hello **IssuingAuthorityKeys** table, right-click it, and then click **Show Table Data**.</span></span>
3. <span data-ttu-id="2852e-174">I hello **IssuingAuthorityKeys** tabell, ska det finnas minst en rad som motsvarar toohello tumavtrycksvärde för hello nyckeln.</span><span class="sxs-lookup"><span data-stu-id="2852e-174">In hello **IssuingAuthorityKeys** table, there will be at least one row, which corresponds toohello thumbprint value for hello key.</span></span> <span data-ttu-id="2852e-175">Ta bort alla rader i tabellen hello.</span><span class="sxs-lookup"><span data-stu-id="2852e-175">Delete any rows in hello table.</span></span>
4. <span data-ttu-id="2852e-176">Högerklicka på hello **klienter** tabell och klicka sedan på **visa tabelldata**.</span><span class="sxs-lookup"><span data-stu-id="2852e-176">Right-click hello **Tenants** table, and then click **Show Table Data**.</span></span>
5. <span data-ttu-id="2852e-177">I hello **hyresgäster** tabell, kommer det att minst en rad som motsvarar tooa unik katalog klient identifierare.</span><span class="sxs-lookup"><span data-stu-id="2852e-177">In hello **Tenants** table, there will be at least one row, which corresponds tooa unique directory tenant identifier.</span></span> <span data-ttu-id="2852e-178">Ta bort alla rader i tabellen hello.</span><span class="sxs-lookup"><span data-stu-id="2852e-178">Delete any rows in hello table.</span></span> <span data-ttu-id="2852e-179">Om du inte tar bort hello rader i båda hello **klienter** tabell och **IssuingAuthorityKeys** tabell, du får ett fel vid körning.</span><span class="sxs-lookup"><span data-stu-id="2852e-179">If you don't delete hello rows in both hello **Tenants** table and **IssuingAuthorityKeys** table, you will get an error at runtime.</span></span>
6. <span data-ttu-id="2852e-180">Skapa och köra programmet hello.</span><span class="sxs-lookup"><span data-stu-id="2852e-180">Build and run hello application.</span></span> <span data-ttu-id="2852e-181">När du har loggat in tooyour konto kan stoppa du hello program.</span><span class="sxs-lookup"><span data-stu-id="2852e-181">After you have logged in tooyour account, you can stop hello application.</span></span>
7. <span data-ttu-id="2852e-182">Returnera toohello **Server Explorer** och titta på hello värden i hello **IssuingAuthorityKeys** och **hyresgäster** tabell.</span><span class="sxs-lookup"><span data-stu-id="2852e-182">Return toohello **Server Explorer** and look at hello values in hello **IssuingAuthorityKeys** and **Tenants** table.</span></span> <span data-ttu-id="2852e-183">Lägg märke till att de har varit automatiskt fylls med hello lämplig information från hello federation metadata dokument.</span><span class="sxs-lookup"><span data-stu-id="2852e-183">You’ll notice that they have been automatically repopulated with hello appropriate information from hello federation metadata document.</span></span>

### <span data-ttu-id="2852e-184"><a name="vs2013"></a>Webb-API: er skydda resurser och skapas med Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="2852e-184"><a name="vs2013"></a>Web APIs protecting resources and created with Visual Studio 2013</span></span>
<span data-ttu-id="2852e-185">Om du har skapat ett webb-API-program i Visual Studio 2013 med hello Web API-mall och sedan markerade **Organisationskonton** från hello **ändra autentisering** menyn du redan har hello nödvändiga logiken i ditt program.</span><span class="sxs-lookup"><span data-stu-id="2852e-185">If you created a web API application in Visual Studio 2013 using hello Web API template, and then selected **Organizational Accounts** from hello **Change Authentication** menu, you already have hello necessary logic in your application.</span></span>

<span data-ttu-id="2852e-186">Om du har manuellt konfigurerade autentisering, följer du anvisningarna för hello nedan toolearn hur tooconfigure Web API-tooautomatically uppdatera dess viktig information.</span><span class="sxs-lookup"><span data-stu-id="2852e-186">If you manually configured authentication, follow hello instructions below toolearn how tooconfigure your Web API tooautomatically update its key information.</span></span>

<span data-ttu-id="2852e-187">hello följande kodavsnitt visar hur tooget hello senaste nycklar från hello federation metadata dokument och sedan använda hello [JWT-Token hanteraren](https://msdn.microsoft.com/library/dn205065.aspx) toovalidate hello-token.</span><span class="sxs-lookup"><span data-stu-id="2852e-187">hello following code snippet demonstrates how tooget hello latest keys from hello federation metadata document, and then use hello [JWT Token Handler](https://msdn.microsoft.com/library/dn205065.aspx) toovalidate hello token.</span></span> <span data-ttu-id="2852e-188">hello kodstycke förutsätter att du ska använda egna cachelagring för bestående hello viktiga toovalidate framtida token från Azure AD oavsett om den är i en databas, konfigurationsfilen eller någon annanstans.</span><span class="sxs-lookup"><span data-stu-id="2852e-188">hello code snippet assumes that you will use your own caching mechanism for persisting hello key toovalidate future tokens from Azure AD, whether it be in a database, configuration file, or elsewhere.</span></span>

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

        // Validates hello JWT Token that's part of hello Authorization header in an HTTP request.
        public void ValidateJwtToken(string token)
        {
            JwtSecurityTokenHandler tokenHandler = new JwtSecurityTokenHandler()
            {
                // Do not disable for production code
                CertificateValidator = X509CertificateValidator.None
            };

            TokenValidationParameters validationParams = new TokenValidationParameters()
            {
                AllowedAudience = "[Your App ID URI goes here, as registered in hello Azure Classic Portal]",
                ValidIssuer = "[hello issuer for hello token goes here, such as https://sts.windows.net/68b98905-130e-4d7c-b6e1-a158a9ed8449/]",
                SigningTokens = GetSigningCertificates(MetadataAddress)

                // Cache hello signing tokens by your desired mechanism
            };

            Thread.CurrentPrincipal = tokenHandler.ValidateToken(token, validationParams);
        }

        // Returns a list of certificates from hello specified metadata document.
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
                        throw new InvalidOperationException("There is no RoleDescriptor of type SecurityTokenServiceType in hello metadata");
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

### <span data-ttu-id="2852e-189"><a name="vs2012"></a>Webbprogram skydda resurser och skapas med Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="2852e-189"><a name="vs2012"></a>Web applications protecting resources and created with Visual Studio 2012</span></span>
<span data-ttu-id="2852e-190">Om programmet har skapats i Visual Studio 2012 används du förmodligen hello identitet och åtkomst verktyget tooconfigure ditt program.</span><span class="sxs-lookup"><span data-stu-id="2852e-190">If your application was built in Visual Studio 2012, you probably used hello Identity and Access Tool tooconfigure your application.</span></span> <span data-ttu-id="2852e-191">Det är också troligt att du använder hello [verifierar Utfärdarens namn registret (VINR)](https://msdn.microsoft.com/library/dn205067.aspx).</span><span class="sxs-lookup"><span data-stu-id="2852e-191">It’s also likely that you are using hello [Validating Issuer Name Registry (VINR)](https://msdn.microsoft.com/library/dn205067.aspx).</span></span> <span data-ttu-id="2852e-192">Hej VINR ansvarar för att underhålla information om betrodda identitetsleverantörer (Azure AD) och hello nycklar används toovalidate token som utfärdats av dem.</span><span class="sxs-lookup"><span data-stu-id="2852e-192">hello VINR is responsible for maintaining information about trusted identity providers (Azure AD) and hello keys used toovalidate tokens issued by them.</span></span> <span data-ttu-id="2852e-193">Hej VINR gör det också enkelt tooautomatically uppdatering hello viktig information lagras i en Web.config-fil genom att hämta hello senaste federation metadata dokument som är associerat med din katalog, kontrollerar om hello konfigurationen är inaktuell med hello senaste dokumentet och uppdaterar hello programmet toouse hello ny nyckel som behövs.</span><span class="sxs-lookup"><span data-stu-id="2852e-193">hello VINR also makes it easy tooautomatically update hello key information stored in a Web.config file by downloading hello latest federation metadata document associated with your directory, checking if hello configuration is out of date with hello latest document, and updating hello application toouse hello new key as necessary.</span></span>

<span data-ttu-id="2852e-194">Om du har skapat ditt program med hjälp av hello kodexempel eller genomgången dokumentation som tillhandahålls av Microsoft ingår hello nyckelförnyelse logik redan i projektet.</span><span class="sxs-lookup"><span data-stu-id="2852e-194">If you created your application using any of hello code samples or walkthrough documentation provided by Microsoft, hello key rollover logic is already included in your project.</span></span> <span data-ttu-id="2852e-195">Du kommer att märka att hello koden nedan finns redan i projektet.</span><span class="sxs-lookup"><span data-stu-id="2852e-195">You will notice that hello code below already exists in your project.</span></span> <span data-ttu-id="2852e-196">Om programmet inte redan har denna logik åtgärderna hello nedan tooadd den och tooverify fungerar korrekt.</span><span class="sxs-lookup"><span data-stu-id="2852e-196">If your application does not already have this logic, follow hello steps below tooadd it and tooverify that it’s working correctly.</span></span>

1. <span data-ttu-id="2852e-197">I **Solution Explorer**, lägga till en referens toohello **System.IdentityModel** sammansättningen för hello rätt projekt.</span><span class="sxs-lookup"><span data-stu-id="2852e-197">In **Solution Explorer**, add a reference toohello **System.IdentityModel** assembly for hello appropriate project.</span></span>
2. <span data-ttu-id="2852e-198">Öppna hello **Global.asax.cs** och Lägg till följande hello med direktiven:</span><span class="sxs-lookup"><span data-stu-id="2852e-198">Open hello **Global.asax.cs** file and add hello following using directives:</span></span>
   ```
   using System.Configuration;
   using System.IdentityModel.Tokens;
   ```
3. <span data-ttu-id="2852e-199">Lägg till följande metod toohello hello **Global.asax.cs** fil:</span><span class="sxs-lookup"><span data-stu-id="2852e-199">Add hello following method toohello **Global.asax.cs** file:</span></span>
   ```
   protected void RefreshValidationSettings()
   {
    string configPath = AppDomain.CurrentDomain.BaseDirectory + "\\" + "Web.config";
    string metadataAddress =
                  ConfigurationManager.AppSettings["ida:FederationMetadataLocation"];
    ValidatingIssuerNameRegistry.WriteToConfig(metadataAddress, configPath);
   }
   ```
4. <span data-ttu-id="2852e-200">Anropa hello **RefreshValidationSettings()** metod i hello **Application_Start()** metod i **Global.asax.cs** som visas:</span><span class="sxs-lookup"><span data-stu-id="2852e-200">Invoke hello **RefreshValidationSettings()** method in hello **Application_Start()** method in **Global.asax.cs** as shown:</span></span>
   ```
   protected void Application_Start()
   {
    AreaRegistration.RegisterAllAreas();
    ...
    RefreshValidationSettings();
   }
   ```

<span data-ttu-id="2852e-201">När du har följt de här stegen kan uppdateras programmets Web.config med hello senaste informationen från hello federation metadata dokument, inklusive hello senaste nycklar.</span><span class="sxs-lookup"><span data-stu-id="2852e-201">Once you have followed these steps, your application’s Web.config will be updated with hello latest information from hello federation metadata document, including hello latest keys.</span></span> <span data-ttu-id="2852e-202">Den här uppdateringen sker varje gång en programpool återanvänds i IIS. IIS är som standard toorecycle program var 29: e timme.</span><span class="sxs-lookup"><span data-stu-id="2852e-202">This update will occur every time your application pool recycles in IIS; by default IIS is set toorecycle applications every 29 hours.</span></span>

<span data-ttu-id="2852e-203">Följ hello steg nedan tooverify hello nyckelförnyelse logiken fungerar.</span><span class="sxs-lookup"><span data-stu-id="2852e-203">Follow hello steps below tooverify that hello key rollover logic is working.</span></span>

1. <span data-ttu-id="2852e-204">När du har kontrollerat att programmet använder hello koden ovan, öppna hello **Web.config** fil och navigera toohello  **<issuerNameRegistry>**  block, särskilt söker efter hello följa några få rader:</span><span class="sxs-lookup"><span data-stu-id="2852e-204">After you have verified that your application is using hello code above, open hello **Web.config** file and navigate toohello **<issuerNameRegistry>** block, specifically looking for hello following few lines:</span></span>
   ```
   <issuerNameRegistry type="System.IdentityModel.Tokens.ValidatingIssuerNameRegistry, System.IdentityModel.Tokens.ValidatingIssuerNameRegistry">
        <authority name="https://sts.windows.net/ec4187af-07da-4f01-b18f-64c2f5abecea/">
          <keys>
            <add thumbprint="3A38FA984E8560F19AADC9F86FE9594BB6AD049B" />
          </keys>
   ```
2. <span data-ttu-id="2852e-205">I hello  **<add thumbprint=””>**  ändra hello tumavtrycksvärde genom att ersätta ett tecken med ett annat namn.</span><span class="sxs-lookup"><span data-stu-id="2852e-205">In hello **<add thumbprint=””>** setting, change hello thumbprint value by replacing any character with a different one.</span></span> <span data-ttu-id="2852e-206">Spara hello **Web.config** fil.</span><span class="sxs-lookup"><span data-stu-id="2852e-206">Save hello **Web.config** file.</span></span>
3. <span data-ttu-id="2852e-207">Skapa hello program och sedan köra den.</span><span class="sxs-lookup"><span data-stu-id="2852e-207">Build hello application, and then run it.</span></span> <span data-ttu-id="2852e-208">Om du kan slutföra hello inloggningsprocessen, uppdaterar har programmet hello nyckel genom att hämta hello krävs information från din katalog federation metadata dokument.</span><span class="sxs-lookup"><span data-stu-id="2852e-208">If you can complete hello sign-in process, your application is successfully updating hello key by downloading hello required information from your directory’s federation metadata document.</span></span> <span data-ttu-id="2852e-209">Om du har problem att logga in, kontrollera hello ändringar i ditt program är korrekta genom att läsa hello [att lägga till inloggning tooYour Web program med hjälp av Azure AD](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) avsnittet eller ladda ned och inspektera hello följande kodexempel: [ Flera innehavare molnet program för Azure Active Directory](https://code.msdn.microsoft.com/multi-tenant-cloud-8015b84b).</span><span class="sxs-lookup"><span data-stu-id="2852e-209">If you are having issues signing in, ensure hello changes in your application are correct by reading hello [Adding Sign-On tooYour Web Application Using Azure AD](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) topic, or downloading and inspecting hello following code sample: [Multi-Tenant Cloud Application for Azure Active Directory](https://code.msdn.microsoft.com/multi-tenant-cloud-8015b84b).</span></span>

### <span data-ttu-id="2852e-210"><a name="vs2010"></a>Webbprogram skydda resurser och skapas med Visual Studio 2008 eller 2010 och Windows Identity Foundation (WIF) version 1.0 för .NET 3.5</span><span class="sxs-lookup"><span data-stu-id="2852e-210"><a name="vs2010"></a>Web applications protecting resources and created with Visual Studio 2008 or 2010 and Windows Identity Foundation (WIF) v1.0 for .NET 3.5</span></span>
<span data-ttu-id="2852e-211">Om du har skapat ett program på WIF v1.0 finns inga angivna mekanism tooautomatically uppdatera ditt programs konfiguration toouse en ny nyckel.</span><span class="sxs-lookup"><span data-stu-id="2852e-211">If you built an application on WIF v1.0, there is no provided mechanism tooautomatically refresh your application’s configuration toouse a new key.</span></span>

* <span data-ttu-id="2852e-212">*Enklast* använda hello FedUtil verktygsuppsättning som ingår i hello WIF SDK, som kan hämta hello senaste metadata dokument och uppdatera din konfiguration.</span><span class="sxs-lookup"><span data-stu-id="2852e-212">*Easiest way* Use hello FedUtil tooling included in hello WIF SDK, which can retrieve hello latest metadata document and update your configuration.</span></span>
* <span data-ttu-id="2852e-213">Uppdatera ditt program too.NET 4.5, vilket innefattar hello senaste versionen av WIF finns i hello-namnområde.</span><span class="sxs-lookup"><span data-stu-id="2852e-213">Update your application too.NET 4.5, which includes hello newest version of WIF located in hello System namespace.</span></span> <span data-ttu-id="2852e-214">Du kan sedan använda hello [verifierar Utfärdarens namn registret (VINR)](https://msdn.microsoft.com/library/dn205067.aspx) tooperform automatiska uppdateringar av hello programkonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="2852e-214">You can then use hello [Validating Issuer Name Registry (VINR)](https://msdn.microsoft.com/library/dn205067.aspx) tooperform automatic updates of hello application’s configuration.</span></span>
* <span data-ttu-id="2852e-215">Utföra en manuell förnyelse enligt hello instruktioner hello slutet av dokumentet vägledning.</span><span class="sxs-lookup"><span data-stu-id="2852e-215">Perform a manual rollover as per hello instructions at hello end of this guidance document.</span></span>

<span data-ttu-id="2852e-216">Instruktioner toouse hello FedUtil tooupdate konfigurationen:</span><span class="sxs-lookup"><span data-stu-id="2852e-216">Instructions toouse hello FedUtil tooupdate your configuration:</span></span>

1. <span data-ttu-id="2852e-217">Kontrollera att du har hello WIF v1.0 SDK är installerat på utvecklingsdatorn för Visual Studio 2008 eller 2010.</span><span class="sxs-lookup"><span data-stu-id="2852e-217">Verify that you have hello WIF v1.0 SDK installed on your development machine for Visual Studio 2008 or 2010.</span></span> <span data-ttu-id="2852e-218">Du kan [ladda ned det från den här](https://www.microsoft.com/en-us/download/details.aspx?id=4451) om du ännu inte har installerat den.</span><span class="sxs-lookup"><span data-stu-id="2852e-218">You can [download it from here](https://www.microsoft.com/en-us/download/details.aspx?id=4451) if you have not yet installed it.</span></span>
2. <span data-ttu-id="2852e-219">Öppna hello lösningen i Visual Studio och högerklicka hello tillämpliga projektet och välj **uppdatera federationsmetadata**.</span><span class="sxs-lookup"><span data-stu-id="2852e-219">In Visual Studio, open hello solution, and then right-click hello applicable project and select **Update federation metadata**.</span></span> <span data-ttu-id="2852e-220">Om det här alternativet inte är tillgängligt har FedUtil och/eller hello WIF v1.0 SDK inte installerats.</span><span class="sxs-lookup"><span data-stu-id="2852e-220">If this option is not available, FedUtil and/or hello WIF v1.0 SDK has not been installed.</span></span>
3. <span data-ttu-id="2852e-221">Hello-Kommandotolken, Välj **uppdatering** toobegin uppdaterar din federationsmetadata.</span><span class="sxs-lookup"><span data-stu-id="2852e-221">From hello prompt, select **Update** toobegin updating your federation metadata.</span></span> <span data-ttu-id="2852e-222">Om du har åtkomst toohello servermiljö där programmet hello finns kan du också använda Fedutil's [uppdateringsschema för automatisk metadata](https://msdn.microsoft.com/library/ee517272.aspx).</span><span class="sxs-lookup"><span data-stu-id="2852e-222">If you have access toohello server environment where hello application is hosted, you can optionally use FedUtil’s [automatic metadata update scheduler](https://msdn.microsoft.com/library/ee517272.aspx).</span></span>
4. <span data-ttu-id="2852e-223">Klicka på **Slutför** toocomplete hello uppdateringsprocessen.</span><span class="sxs-lookup"><span data-stu-id="2852e-223">Click **Finish** toocomplete hello update process.</span></span>

### <span data-ttu-id="2852e-224"><a name="other"></a>Webbprogram / API: er som skyddar resurser med andra bibliotek eller implementera manuellt någon hello stöds protokoll</span><span class="sxs-lookup"><span data-stu-id="2852e-224"><a name="other"></a>Web applications / APIs protecting resources using any other libraries or manually implementing any of hello supported protocols</span></span>
<span data-ttu-id="2852e-225">Om du använder vissa andra bibliotek eller implementeras manuellt hello stöds protokollen, behöver du tooreview hello biblioteket eller din implementering tooensure som hello nyckeln hämtas från hello OpenID Connect discovery-dokumentet eller hello Federation metadata dokument.</span><span class="sxs-lookup"><span data-stu-id="2852e-225">If you are using some other library or manually implemented any of hello supported protocols, you'll need tooreview hello library or your implementation tooensure that hello key is being retrieved from either hello OpenID Connect discovery document or hello federation metadata document.</span></span> <span data-ttu-id="2852e-226">Enkelriktade toocheck för det här är toodo en sökning i koden eller hello biblioteket kod för varje anrop ut tooeither hello OpenID identifiering eller hello federation metadata dokument.</span><span class="sxs-lookup"><span data-stu-id="2852e-226">One way toocheck for this is toodo a search in your code or hello library's code for any calls out tooeither hello OpenID discovery document or hello federation metadata document.</span></span>

<span data-ttu-id="2852e-227">Hämta hello nyckel och uppdatera den i enlighet med detta genom att utföra en manuell förnyelse enligt hello instruktioner hello slutet av dokumentet vägledning om de nycklar lagras någonstans eller hårdkodad i ditt program kan du manuellt.</span><span class="sxs-lookup"><span data-stu-id="2852e-227">If they key is being stored somewhere or hardcoded in your application, you can manually retrieve hello key and update it accordingly by perform a manual rollover as per hello instructions at hello end of this guidance document.</span></span> <span data-ttu-id="2852e-228">**Det rekommenderas starkt att du förbättra ditt program toosupport automatisk förnyelse** med hjälp av hello närmar sig disposition i den här artikeln tooavoid framtida avbrott och omkostnader om Azure AD ökar dess förnyade takt eller har ett nödfall out-of-band-förnyelse.</span><span class="sxs-lookup"><span data-stu-id="2852e-228">**It is strongly encouraged that you enhance your application toosupport automatic rollover** using any of hello approaches outline in this article tooavoid future disruptions and overhead if Azure AD increases it's rollover cadence or has an emergency out-of-band rollover.</span></span>

## <a name="how-tootest-your-application-toodetermine-if-it-will-be-affected"></a><span data-ttu-id="2852e-229">Hur tootest ditt program toodetermine om kommer att påverkas</span><span class="sxs-lookup"><span data-stu-id="2852e-229">How tootest your application toodetermine if it will be affected</span></span>
<span data-ttu-id="2852e-230">Du kan kontrollera om ditt program stöder automatisk nyckelförnyelse genom att hämta hello skript och hello instruktionerna i [GitHub-lagringsplatsen.](https://github.com/AzureAD/azure-activedirectory-powershell-tokenkey)</span><span class="sxs-lookup"><span data-stu-id="2852e-230">You can validate whether your application supports automatic key rollover by downloading hello scripts and following hello instructions in [this GitHub repository.](https://github.com/AzureAD/azure-activedirectory-powershell-tokenkey)</span></span>

## <a name="how-tooperform-a-manual-rollover-if-you-application-does-not-support-automatic-rollover"></a><span data-ttu-id="2852e-231">Hur tooperform en manuell förnyelse om du programmet inte stöder automatisk förnyelse</span><span class="sxs-lookup"><span data-stu-id="2852e-231">How tooperform a manual rollover if you application does not support automatic rollover</span></span>
<span data-ttu-id="2852e-232">Om programmet inte **inte** stöd för automatisk förnyelse, måste tooestablish en process som regelbundet övervakar Azure AD signering nycklar och utför en manuell förnyelse därefter.</span><span class="sxs-lookup"><span data-stu-id="2852e-232">If your application does **not** support automatic rollover, you will need tooestablish a process that periodically monitors Azure AD's signing keys and performs a manual rollover accordingly.</span></span> <span data-ttu-id="2852e-233">[Den här GitHub-lagringsplatsen](https://github.com/AzureAD/azure-activedirectory-powershell-tokenkey) innehåller skript och instruktioner om hur toodo detta.</span><span class="sxs-lookup"><span data-stu-id="2852e-233">[This GitHub repository](https://github.com/AzureAD/azure-activedirectory-powershell-tokenkey) contains scripts and instructions on how toodo this.</span></span>

