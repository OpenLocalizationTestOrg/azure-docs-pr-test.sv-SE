---
title: "aaaHow tooenable enkel inloggning mellan appar på iOS använder ADAL | Microsoft Docs"
description: 'Hur hello toouse hello funktioner i ADAL SDK tooenable enkel inloggning i ditt program. '
services: active-directory
documentationcenter: 
author: brandwe
manager: mbaldwin
editor: 
ms.assetid: d042d6da-7503-4e20-bb55-06917de01fcd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: ios
ms.devlang: objective-c
ms.topic: article
ms.date: 04/07/2017
ms.author: brandwe
ms.custom: aaddev
ms.openlocfilehash: b7b4389a8dcd956211ffa1aaa431aaf21ded8961
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooenable-cross-app-sso-on-ios-using-adal"></a><span data-ttu-id="5e135-103">Hur tooenable enkel inloggning mellan appar på iOS använder ADAL</span><span class="sxs-lookup"><span data-stu-id="5e135-103">How tooenable cross-app SSO on iOS using ADAL</span></span>
<span data-ttu-id="5e135-104">Att tillhandahålla enkel inloggning (SSO) så att användarna bara behöver tooenter sina autentiseringsuppgifter en gång och har de autentiseringsuppgifterna som automatiskt fungerar över program förväntas nu av kunder.</span><span class="sxs-lookup"><span data-stu-id="5e135-104">Providing Single Sign-On (SSO) so that users only need tooenter their credentials once and have those credentials automatically work across applications is now expected by customers.</span></span> <span data-ttu-id="5e135-105">hello svårt att ange sina användarnamn och lösenord på en liten skärm, ofta gånger kombineras med ytterligare en faktor (2FA) som ett telefonsamtal eller en textläge kod leder till att snabbt klagomål om en användare har toodo detta mer än en gång till produkten.</span><span class="sxs-lookup"><span data-stu-id="5e135-105">hello difficulty in entering their username and password on a small screen, often times combined with an additional factor (2FA) like a phone call or a texted code, results in quick dissatisfaction if a user has toodo this more than one time for your product.</span></span>

<span data-ttu-id="5e135-106">Dessutom, om du använder en identity-plattform som andra program kan använda, till exempel Microsoft Accounts eller ett arbetskonto från Office365, kunder förväntar sig att dessa autentiseringsuppgifter toobe tillgängliga toouse i alla program, oavsett hello leverantör.</span><span class="sxs-lookup"><span data-stu-id="5e135-106">In addition, if you apply an identity platform that other applications may use such as Microsoft Accounts or a work account from Office365, customers expect that those credentials toobe available toouse across all their applications no matter hello vendor.</span></span>

<span data-ttu-id="5e135-107">hello Microsoft Identity-plattformen, tillsammans med vår Microsoft Identity-SDK: er, har den här tunga arbetet för dig och ger du hello möjlighet toodelight dina kunder med enkel inloggning antingen inom din egen suite av program eller som med våra broker kapaciteten och autentiseraren program över hello hela enheten.</span><span class="sxs-lookup"><span data-stu-id="5e135-107">hello Microsoft Identity platform, along with our Microsoft Identity SDKs, does all this hard work for you and gives you hello ability toodelight your customers with SSO either within your own suite of applications or, as with our broker capability and Authenticator applications, across hello entire device.</span></span>

<span data-ttu-id="5e135-108">Den här genomgången får du veta hur tooconfigure våra SDK i ditt program tooprovide denna förmån tooyour kunder.</span><span class="sxs-lookup"><span data-stu-id="5e135-108">This walkthrough will tell you how tooconfigure our SDK within your application tooprovide this benefit tooyour customers.</span></span>

<span data-ttu-id="5e135-109">Den här genomgången gäller för:</span><span class="sxs-lookup"><span data-stu-id="5e135-109">This walkthrough applies to:</span></span>

* <span data-ttu-id="5e135-110">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="5e135-110">Azure Active Directory</span></span>
* <span data-ttu-id="5e135-111">Azure Active Directory B2C</span><span class="sxs-lookup"><span data-stu-id="5e135-111">Azure Active Directory B2C</span></span>
* <span data-ttu-id="5e135-112">Azure Active Directory B2B</span><span class="sxs-lookup"><span data-stu-id="5e135-112">Azure Active Directory B2B</span></span>
* <span data-ttu-id="5e135-113">Villkorsstyrd åtkomst med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="5e135-113">Azure Active Directory Conditional Access</span></span>

<span data-ttu-id="5e135-114">hello dokumentet ovan förutsätter att du vet hur för[etablera program på hello äldre portalen för Azure Active Directory](active-directory-how-to-integrate.md) och integrerad ditt program med hello [Microsoft Identity iOS SDK](https://github.com/AzureAD/azure-activedirectory-library-for-objc).</span><span class="sxs-lookup"><span data-stu-id="5e135-114">hello document preceding assumes you know how too[provision applications in hello legacy portal for Azure Active Directory](active-directory-how-to-integrate.md) and integrated your application with hello [Microsoft Identity iOS SDK](https://github.com/AzureAD/azure-activedirectory-library-for-objc).</span></span>

## <a name="sso-concepts-in-hello-microsoft-identity-platform"></a><span data-ttu-id="5e135-115">SSO begrepp i hello Microsoft Identity-plattformen</span><span class="sxs-lookup"><span data-stu-id="5e135-115">SSO Concepts in hello Microsoft Identity Platform</span></span>
### <a name="microsoft-identity-brokers"></a><span data-ttu-id="5e135-116">Microsoft Identity mäklare</span><span class="sxs-lookup"><span data-stu-id="5e135-116">Microsoft Identity Brokers</span></span>
<span data-ttu-id="5e135-117">Microsoft tillhandahåller program för varje mobil plattform som gör att för hello bryggning av autentiseringsuppgifter i program från olika leverantörer och gör för särskilda funktioner som kräver en enda säker plats varifrån toovalidate autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="5e135-117">Microsoft provides applications for every mobile platform that allow for hello bridging of credentials across applications from different vendors and allows for special enhanced features that require a single secure place from where toovalidate credentials.</span></span> <span data-ttu-id="5e135-118">Vi kallar dem **mäklare**.</span><span class="sxs-lookup"><span data-stu-id="5e135-118">We call these **brokers**.</span></span> <span data-ttu-id="5e135-119">På iOS och Android sker dessa mäklare via nedladdningsbara program att kunder installeras fristående eller flyttas toohello enhet av ett företag som hanterar vissa eller alla hello enhet för sina anställda.</span><span class="sxs-lookup"><span data-stu-id="5e135-119">On iOS and Android these brokers are provided through downloadable applications that customers either install independently or can be pushed toohello device by a company who manages some or all of hello device for their employee.</span></span> <span data-ttu-id="5e135-120">Dessa mäklare stöder Hantera säkerhet för vissa program eller hello hela enheten baserat på vad IT-administratörer som önskar.</span><span class="sxs-lookup"><span data-stu-id="5e135-120">These brokers support managing security just for some applications or hello entire device based on what IT Administrators desire.</span></span> <span data-ttu-id="5e135-121">Den här funktionen tillhandahålls av en inbyggd toohello operativsystemet tekniskt kallas hello Webbautentiseringskoordinatorn väljare av användarkonto i Windows.</span><span class="sxs-lookup"><span data-stu-id="5e135-121">In Windows, this functionality is provided by an account chooser built in toohello operating system, known technically as hello Web Authentication Broker.</span></span>

<span data-ttu-id="5e135-122">Mer information om hur använder vi dessa mäklare och hur dina kunder kan se dem i sina inloggningen flödet för hello Microsoft Identity plattform läsas på.</span><span class="sxs-lookup"><span data-stu-id="5e135-122">For more information on how we use these brokers and how your customers might see them in their login flow for hello Microsoft Identity platform read on.</span></span>

### <a name="patterns-for-logging-in-on-mobile-devices"></a><span data-ttu-id="5e135-123">Mönster för att logga in på mobila enheter</span><span class="sxs-lookup"><span data-stu-id="5e135-123">Patterns for logging in on mobile devices</span></span>
<span data-ttu-id="5e135-124">Åtkomst toocredentials på enheter följer två grundläggande mönster för hello Microsoft Identity plattform:</span><span class="sxs-lookup"><span data-stu-id="5e135-124">Access toocredentials on devices follow two basic patterns for hello Microsoft Identity platform:</span></span>

* <span data-ttu-id="5e135-125">Icke-förhandlad assisterad inloggningar</span><span class="sxs-lookup"><span data-stu-id="5e135-125">Non-broker assisted logins</span></span>
* <span data-ttu-id="5e135-126">Broker assisterad inloggningar</span><span class="sxs-lookup"><span data-stu-id="5e135-126">Broker assisted logins</span></span>

#### <a name="non-broker-assisted-logins"></a><span data-ttu-id="5e135-127">Icke-förhandlad assisterad inloggningar</span><span class="sxs-lookup"><span data-stu-id="5e135-127">Non-broker assisted logins</span></span>
<span data-ttu-id="5e135-128">Icke-förhandlad assisterad inloggningar är inloggning upplevelser som inträffa infogade med hello program och Använd hello lokal lagring på hello enhet för programmet.</span><span class="sxs-lookup"><span data-stu-id="5e135-128">Non-broker assisted logins are login experiences that happen inline with hello application and use hello local storage on hello device for that application.</span></span> <span data-ttu-id="5e135-129">Lagringen kan delas mellan program men hello autentiseringsuppgifterna är tätt bundna toohello app eller en uppsättning appar som använder denna autentiseringsuppgift.</span><span class="sxs-lookup"><span data-stu-id="5e135-129">This storage may be shared across applications but hello credentials are tightly bound toohello app or suite of apps using that credential.</span></span> <span data-ttu-id="5e135-130">Du har förmodligen fått det i många mobila program när du anger ett användarnamn och lösenord i själva hello-programmet.</span><span class="sxs-lookup"><span data-stu-id="5e135-130">You've most likely experienced this in many mobile applications when you enter a username and password within hello application itself.</span></span>

<span data-ttu-id="5e135-131">Dessa inloggningar har hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="5e135-131">These logins have hello following benefits:</span></span>

* <span data-ttu-id="5e135-132">Användarupplevelsen finns helt i hello-programmet.</span><span class="sxs-lookup"><span data-stu-id="5e135-132">User experience exists entirely within hello application.</span></span>
* <span data-ttu-id="5e135-133">Autentiseringsuppgifter kan delas mellan program som är signerade av hello samma certifikat, vilket ger en enkel inloggning tooyour uppsättning program.</span><span class="sxs-lookup"><span data-stu-id="5e135-133">Credentials can be shared across applications that are signed by hello same certificate, providing a single sign-on experience tooyour suite of applications.</span></span>
* <span data-ttu-id="5e135-134">Kontrollen runt hello upplevelse av loggning i finns toohello program före och efter inloggning.</span><span class="sxs-lookup"><span data-stu-id="5e135-134">Control around hello experience of logging in is provided toohello application before and after sign-in.</span></span>

<span data-ttu-id="5e135-135">Dessa inloggningar har följande nackdelar hello:</span><span class="sxs-lookup"><span data-stu-id="5e135-135">These logins have hello following drawbacks:</span></span>

* <span data-ttu-id="5e135-136">Enkel inloggning på kan inte användarupplevelse över alla appar som använder Microsoft-Identity enbart över de Microsoft-Identities som programmet har konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="5e135-136">User cannot experience single-sign on across all apps that use a Microsoft Identity, only across those Microsoft Identities that your application has configured.</span></span>
* <span data-ttu-id="5e135-137">Programmet kan inte användas med mer avancerade funktioner för företag till exempel villkorlig åtkomst eller Använd hello InTune-produkter.</span><span class="sxs-lookup"><span data-stu-id="5e135-137">Your application cannot be used with more advanced business features such as Conditional Access or use hello InTune suite of products.</span></span>
* <span data-ttu-id="5e135-138">Programmet stöder inte certifikatbaserad autentisering för företagsanvändare.</span><span class="sxs-lookup"><span data-stu-id="5e135-138">Your application can't support certificate-based authentication for business users.</span></span>

<span data-ttu-id="5e135-139">Här är en representation av hur hello Microsoft Identity SDK fungerar med hello delad lagring av ditt program tooenable enkel inloggning:</span><span class="sxs-lookup"><span data-stu-id="5e135-139">Here is a representation of how hello Microsoft Identity SDKs work with hello shared storage of your applications tooenable SSO:</span></span>

```
+------------+ +------------+  +-------------+
|            | |            |  |             |
|   App 1    | |   App 2    |  |   App 3     |
|            | |            |  |             |
|            | |            |  |             |
+------------+ +------------+  +-------------+
| ADAL SDK  |  |  ADAL SDK  |  |  ADAK SDK   |
+------------+-+------------+--+-------------+
|                                            |
|            App Shared Storage              |
+--------------------------------------------+
```

#### <a name="broker-assisted-logins"></a><span data-ttu-id="5e135-140">Broker assisterad inloggningar</span><span class="sxs-lookup"><span data-stu-id="5e135-140">Broker assisted logins</span></span>
<span data-ttu-id="5e135-141">Service Broker-stödd inloggningar är inloggning upplevelser som sker inom hello broker program och använder hello lagring och säkerhet för hello broker tooshare autentiseringsuppgifter för alla program på hello enhet som gäller hello Microsoft Identity-plattformen.</span><span class="sxs-lookup"><span data-stu-id="5e135-141">Broker-assisted logins are login experiences that occur within hello broker application and use hello storage and security of hello broker tooshare credentials across all applications on hello device that apply hello Microsoft Identity platform.</span></span> <span data-ttu-id="5e135-142">Detta innebär att dina program som förlitar sig på hello broker toosign användare i.</span><span class="sxs-lookup"><span data-stu-id="5e135-142">This means that your applications rely on hello broker toosign users in.</span></span> <span data-ttu-id="5e135-143">På iOS och Android sker dessa mäklare via nedladdningsbara program att kunder installeras fristående eller flyttas toohello enhet av ett företag som hanterar hello enheten för sina användare.</span><span class="sxs-lookup"><span data-stu-id="5e135-143">On iOS and Android these brokers are provided through downloadable applications that customers either install independently or can be pushed toohello device by a company who manages hello device for their user.</span></span> <span data-ttu-id="5e135-144">Ett exempel på den här typen av program är hello Microsoft Authenticator programmet på iOS.</span><span class="sxs-lookup"><span data-stu-id="5e135-144">An example of this type of application is hello Microsoft Authenticator application on iOS.</span></span> <span data-ttu-id="5e135-145">Den här funktionen tillhandahålls av en inbyggd toohello operativsystemet tekniskt kallas hello Webbautentiseringskoordinatorn väljare av användarkonto i Windows.</span><span class="sxs-lookup"><span data-stu-id="5e135-145">In Windows this functionality is provided by an account chooser built in toohello operating system, known technically as hello Web Authentication Broker.</span></span>
<span data-ttu-id="5e135-146">hello upplevelse varierar efter plattform och ibland kan vara störande toousers om de inte hanteras på rätt sätt.</span><span class="sxs-lookup"><span data-stu-id="5e135-146">hello experience varies by platform and can sometimes be disruptive toousers if not managed correctly.</span></span> <span data-ttu-id="5e135-147">Du är mest förmodligen är bekant med det här mönstret om du har installerat hello Facebook-program och använder Facebook ansluta från ett annat program.</span><span class="sxs-lookup"><span data-stu-id="5e135-147">You're probably most familiar with this pattern if you have hello Facebook application installed and use Facebook Connect from another application.</span></span> <span data-ttu-id="5e135-148">hello hello Microsoft Identity-plattformen använder samma mönster.</span><span class="sxs-lookup"><span data-stu-id="5e135-148">hello Microsoft Identity platform uses hello same pattern.</span></span>

<span data-ttu-id="5e135-149">För iOS leder tooa ”övergången” animering där programmet skickas toohello bakgrunden medan hello Microsoft Authenticator program kommer toohello förgrunden för hello användaren tooselect vilket konto som de vill ha toosign med.</span><span class="sxs-lookup"><span data-stu-id="5e135-149">For iOS this leads tooa "transition" animation where your application is sent toohello background while hello Microsoft Authenticator applications comes toohello foreground for hello user tooselect which account they would like toosign in with.</span></span>  

<span data-ttu-id="5e135-150">För Android och Windows hello konto visas Väljaren ovanpå ditt program som är mindre störande toohello användare.</span><span class="sxs-lookup"><span data-stu-id="5e135-150">For Android and Windows hello account chooser is displayed on top of your application which is less disruptive toohello user.</span></span>

#### <a name="how-hello-broker-gets-invoked"></a><span data-ttu-id="5e135-151">Hur hello broker hämtar anropas</span><span class="sxs-lookup"><span data-stu-id="5e135-151">How hello broker gets invoked</span></span>
<span data-ttu-id="5e135-152">Om en kompatibel broker är installerad på hello enhet, som hello Microsoft Authenticator programmet hello Microsoft Identity SDK: er kommer automatiskt hello arbete anropar hello broker för dig när en användare anger de önskar toolog in med ett konto från hello Microsoft Identity plattform.</span><span class="sxs-lookup"><span data-stu-id="5e135-152">If a compatible broker is installed on hello device, like hello Microsoft Authenticator application, hello Microsoft Identity SDKs will automatically do hello work of invoking hello broker for you when a user indicates they wish toolog in using any account from hello Microsoft Identity platform.</span></span> <span data-ttu-id="5e135-153">Det här kontot kan vara ett personligt Microsoft-Account, ett arbets eller skolkonto, eller ett konto som du anger och värden i Azure med hjälp av vår B2C och B2B-produkter.</span><span class="sxs-lookup"><span data-stu-id="5e135-153">This account could be a personal Microsoft Account, a work or school account, or an account that you provide and host in Azure using our B2C and B2B products.</span></span> 

 #### <a name="how-we-ensure-hello-application-is-valid"></a><span data-ttu-id="5e135-154">Hur vi Kontrollera hello program är giltig</span><span class="sxs-lookup"><span data-stu-id="5e135-154">How we ensure hello application is valid</span></span>
 
 <span data-ttu-id="5e135-155">hello måste tooensure hello identiteten för ett program anropet hello broker är avgörande toohello säkerhet som vi tillhandahåller broker stödd inloggningar.</span><span class="sxs-lookup"><span data-stu-id="5e135-155">hello need tooensure hello identity of an application call hello broker is crucial toohello security we provide in broker assisted logins.</span></span> <span data-ttu-id="5e135-156">Varken iOS eller Android tillämpar unika identifierare är endast giltiga för ett visst program så att skadliga program kan ”förfalska” legitima program-ID och ta emot hello token är avsedda för hello legitima program.</span><span class="sxs-lookup"><span data-stu-id="5e135-156">Neither iOS nor Android enforces unique identifiers that are valid only for a given application, so malicious applications may "spoof" a legitimate application's identifier and receive hello tokens meant for hello legitimate application.</span></span> <span data-ttu-id="5e135-157">tooensure vi alltid kommunicerar med hello rätt program vid körning, ber vi hello developer tooprovide anpassade redirectURI när de registrerar sina program med Microsoft.</span><span class="sxs-lookup"><span data-stu-id="5e135-157">tooensure we are always communicating with hello right application at runtime, we ask hello developer tooprovide a custom redirectURI when registering their application with Microsoft.</span></span> <span data-ttu-id="5e135-158">**Hur utvecklare ska använda för att skapa den här omdirigerings-URI diskuteras i detalj nedan.**</span><span class="sxs-lookup"><span data-stu-id="5e135-158">**How developers should craft this redirect URI is discussed in detail below.**</span></span> <span data-ttu-id="5e135-159">Den här anpassade redirectURI innehåller hello paket-ID för programmet hello och toobe unika toohello program säkerställs genom hello Apple App Store.</span><span class="sxs-lookup"><span data-stu-id="5e135-159">This custom redirectURI contains hello Bundle ID of hello application and is ensured toobe unique toohello application by hello Apple App Store.</span></span> <span data-ttu-id="5e135-160">När ett program anropar hello broker hello broker ber hello iOS fungerar system tooprovide med hello paket-ID som kallas hello broker.</span><span class="sxs-lookup"><span data-stu-id="5e135-160">When an application calls hello broker, hello broker asks hello iOS operating system tooprovide it with hello Bundle ID that called hello broker.</span></span> <span data-ttu-id="5e135-161">hello broker ger detta paket-ID tooMicrosoft i hello anropet tooour identitetssystem.</span><span class="sxs-lookup"><span data-stu-id="5e135-161">hello broker provides this Bundle ID tooMicrosoft in hello call tooour identity system.</span></span> <span data-ttu-id="5e135-162">Om inte hello paket-ID för programmet hello matchar hello paket-ID angavs toous av hello utvecklare under registreringen vi kommer att neka åtkomst toohello token för hello resurs hello program begär.</span><span class="sxs-lookup"><span data-stu-id="5e135-162">If hello Bundle ID of hello application does not match hello Bundle ID provided toous by hello developer during registration, we will deny access toohello tokens for hello resource hello application is requesting.</span></span> <span data-ttu-id="5e135-163">Detta säkerställer att endast hello-program som är registrerat av hello utvecklare tar emot tokens.</span><span class="sxs-lookup"><span data-stu-id="5e135-163">This check ensures that only hello application registered by hello developer receives tokens.</span></span>

<span data-ttu-id="5e135-164">**hello utvecklare har hello valet av om hello Microsoft Identity SDK-anrop hello broker eller använder hello icke-förhandlad assisterad flödet.**</span><span class="sxs-lookup"><span data-stu-id="5e135-164">**hello developer has hello choice of if hello Microsoft Identity SDK calls hello broker or uses hello non-broker assisted flow.**</span></span> <span data-ttu-id="5e135-165">Men om hello utvecklare väljer inte toouse hello broker-stödd flödet de förlorar hello fördelen med att använda enkel inloggning autentiseringsuppgifter hello användaren kanske redan har lagt till på hello enhet och förhindrar att deras program som används med funktioner för företag Microsoft ger sina kunder som villkorlig åtkomst, Intune-hanteringsfunktioner och certifikatbaserad autentisering.</span><span class="sxs-lookup"><span data-stu-id="5e135-165">However if hello developer chooses not toouse hello broker-assisted flow they lose hello benefit of using SSO credentials that hello user may have already added on hello device and prevents their application from being used with business features Microsoft provides its customers such as Conditional Access, Intune Management capabilities, and certificate-based authentication.</span></span>

<span data-ttu-id="5e135-166">Dessa inloggningar har hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="5e135-166">These logins have hello following benefits:</span></span>

* <span data-ttu-id="5e135-167">Användaren upplever SSO över sina program oavsett hello leverantör.</span><span class="sxs-lookup"><span data-stu-id="5e135-167">User experiences SSO across all their applications no matter hello vendor.</span></span>
* <span data-ttu-id="5e135-168">Programmet kan använda mer avancerade funktioner för företag som villkorlig åtkomst eller använda hello InTune-produkter.</span><span class="sxs-lookup"><span data-stu-id="5e135-168">Your application can use more advanced business features such as Conditional Access or use hello InTune suite of products.</span></span>
* <span data-ttu-id="5e135-169">Programmet stöder certifikatbaserad autentisering för användare i verksamheten.</span><span class="sxs-lookup"><span data-stu-id="5e135-169">Your application can support certificate-based authentication for business users.</span></span>
* <span data-ttu-id="5e135-170">Mycket säkrare inloggningen som hello identitet hello program- och Hej kontrolleras av hello broker program med ytterligare säkerhetsalgoritmer och kryptering.</span><span class="sxs-lookup"><span data-stu-id="5e135-170">Much more secure sign-in experience as hello identity of hello application and hello user are verified by hello broker application with additional security algorithms and encryption.</span></span>

<span data-ttu-id="5e135-171">Dessa inloggningar har följande nackdelar hello:</span><span class="sxs-lookup"><span data-stu-id="5e135-171">These logins have hello following drawbacks:</span></span>

* <span data-ttu-id="5e135-172">I iOS övergick hello användare utanför din programupplevelse medan autentiseringsuppgifter är valt.</span><span class="sxs-lookup"><span data-stu-id="5e135-172">In iOS hello user is transitioned out of your application's experience while credentials are chosen.</span></span>
* <span data-ttu-id="5e135-173">Förlust av hello möjlighet toomanage hello inloggningen upplevelse för kunderna i ditt program.</span><span class="sxs-lookup"><span data-stu-id="5e135-173">Loss of hello ability toomanage hello login experience for your customers within your application.</span></span>

<span data-ttu-id="5e135-174">Här är en representation av hur hello Microsoft Identity SDK fungerar med hello broker program tooenable enkel inloggning:</span><span class="sxs-lookup"><span data-stu-id="5e135-174">Here is a representation of how hello Microsoft Identity SDKs work with hello broker applications tooenable SSO:</span></span>

```
+------------+ +------------+   +-------------+
|            | |            |   |             |
|   App 1    | |   App 2    |   |   Someone   |
|            | |            |   |    Else's   |
|            | |            |   |     App     |
+------------+ +------------+   +-------------+
| Azure SDK  | | Azure SDK  |   | Azure SDK   |
+-----+------+-+-----+------+-  +-------+-----+
      |              |                  |
      |       +------v------+           |
      |       |             |           |
      |       | Microsoft   |           |
      +-------> Broker      |^----------+
              | Application
              |             |
              +-------------+
              |             |
              |   Broker    |
              |   Storage   |
              |             |
              +-------------+
```

<span data-ttu-id="5e135-175">Tillsammans med den här bakgrundsinformation ska kunna toobetter att förstå och implementera enkel inloggning i ditt program med hjälp av hello Microsoft Identity plattform och SDK: er.</span><span class="sxs-lookup"><span data-stu-id="5e135-175">Armed with this background information you should be able toobetter understand and implement SSO within your application using hello Microsoft Identity platform and SDKs.</span></span>

## <a name="enabling-cross-app-sso-using-adal"></a><span data-ttu-id="5e135-176">Aktivera enkel inloggning mellan appar med hjälp av ADAL</span><span class="sxs-lookup"><span data-stu-id="5e135-176">Enabling cross-app SSO using ADAL</span></span>
<span data-ttu-id="5e135-177">Här använder vi hello ADAL iOS SDK till:</span><span class="sxs-lookup"><span data-stu-id="5e135-177">Here we use hello ADAL iOS SDK to:</span></span>

* <span data-ttu-id="5e135-178">Aktivera icke-förhandlad stödd enkel inloggning för en uppsättning appar</span><span class="sxs-lookup"><span data-stu-id="5e135-178">Turn on non-broker assisted SSO for your suite of apps</span></span>
* <span data-ttu-id="5e135-179">Aktivera stöd för Service broker-stödd enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="5e135-179">Turn on support for broker-assisted SSO</span></span>

### <a name="turning-on-sso-for-non-broker-assisted-sso"></a><span data-ttu-id="5e135-180">Aktivera enkel inloggning för icke-förhandlad stödd SSO</span><span class="sxs-lookup"><span data-stu-id="5e135-180">Turning on SSO for non-broker assisted SSO</span></span>
<span data-ttu-id="5e135-181">För icke-förhandlad assisterad SSO över program hantera hello Microsoft Identity SDK mycket hello komplexitet SSO du.</span><span class="sxs-lookup"><span data-stu-id="5e135-181">For non-broker assisted SSO across applications hello Microsoft Identity SDKs manage much of hello complexity of SSO for you.</span></span> <span data-ttu-id="5e135-182">Detta inkluderar att hitta hello rätt användare i hello cache och underhålla en lista över inloggade användare för du tooquery.</span><span class="sxs-lookup"><span data-stu-id="5e135-182">This includes finding hello right user in hello cache and maintaining a list of logged in users for you tooquery.</span></span>

<span data-ttu-id="5e135-183">tooenable enkel inloggning för program som du äger måste toodo hello följande:</span><span class="sxs-lookup"><span data-stu-id="5e135-183">tooenable SSO across applications you own you need toodo hello following:</span></span>

1. <span data-ttu-id="5e135-184">Se till att alla dina program användaren hello samma klient-ID eller program-ID.</span><span class="sxs-lookup"><span data-stu-id="5e135-184">Ensure all your applications user hello same Client ID or Application ID.</span></span>
2. <span data-ttu-id="5e135-185">Kontrollera att alla dina program dela hello samma signering av certifikat från Apple så att du kan dela nyckelringar</span><span class="sxs-lookup"><span data-stu-id="5e135-185">Ensure that all of your applications share hello same signing certificate from Apple so that you can share keychains</span></span>
3. <span data-ttu-id="5e135-186">Begära hello samma nyckelringar rätt för var och en av dina program.</span><span class="sxs-lookup"><span data-stu-id="5e135-186">Request hello same keychain entitlement for each of your applications.</span></span>
4. <span data-ttu-id="5e135-187">Hello Microsoft Identity SDK: er om hello delade nyckelringen när du vill ange oss toouse.</span><span class="sxs-lookup"><span data-stu-id="5e135-187">Tell hello Microsoft Identity SDKs about hello shared keychain you want us toouse.</span></span>

#### <a name="using-hello-same-client-id--application-id-for-all-hello-applications-in-your-suite-of-apps"></a><span data-ttu-id="5e135-188">Med hjälp av hello samma klient-ID eller program-ID för alla hello program i din uppsättning appar</span><span class="sxs-lookup"><span data-stu-id="5e135-188">Using hello same Client ID / Application ID for all hello applications in your suite of apps</span></span>
<span data-ttu-id="5e135-189">För att hello Microsoft Identity plattform tooknow att tillåts tooshare token för ditt program, behöver var och en av dina program tooshare hello samma klient-ID eller program-ID.</span><span class="sxs-lookup"><span data-stu-id="5e135-189">In order for hello Microsoft Identity platform tooknow that it's allowed tooshare tokens across your applications, each of your applications will need tooshare hello same Client ID or Application ID.</span></span> <span data-ttu-id="5e135-190">Detta är hello Unik identifierare som du har fått tooyou när du har registrerat din första programmet hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="5e135-190">This is hello unique identifier that was provided tooyou when you registered your first application in hello portal.</span></span>

<span data-ttu-id="5e135-191">Du kanske undrar hur du identifierar olika appar toohello Microsoft Identity-tjänsten om den använder hello samma program-ID.</span><span class="sxs-lookup"><span data-stu-id="5e135-191">You may be wondering how you will identify different apps toohello Microsoft Identity service if it uses hello same Application ID.</span></span> <span data-ttu-id="5e135-192">hello svaret är med hello **omdirigerings-URI: er**.</span><span class="sxs-lookup"><span data-stu-id="5e135-192">hello answer is with hello **Redirect URIs**.</span></span> <span data-ttu-id="5e135-193">Varje program kan ha flera omdirigerings-URI: er registrerade i hello onboarding-portalen.</span><span class="sxs-lookup"><span data-stu-id="5e135-193">Each application can have multiple Redirect URIs registered in hello onboarding portal.</span></span> <span data-ttu-id="5e135-194">Varje app i din suite har en annan omdirigerings-URI.</span><span class="sxs-lookup"><span data-stu-id="5e135-194">Each app in your suite will have a different redirect URI.</span></span> <span data-ttu-id="5e135-195">Ett exempel på hur detta ser ut understiger:</span><span class="sxs-lookup"><span data-stu-id="5e135-195">An example of how this looks is below:</span></span>

<span data-ttu-id="5e135-196">App1 omdirigerings-URI:`x-msauth-mytestiosapp://com.myapp.mytestapp`</span><span class="sxs-lookup"><span data-stu-id="5e135-196">App1 Redirect URI: `x-msauth-mytestiosapp://com.myapp.mytestapp`</span></span>

<span data-ttu-id="5e135-197">App2 omdirigerings-URI:`x-msauth-mytestiosapp://com.myapp.mytestapp2`</span><span class="sxs-lookup"><span data-stu-id="5e135-197">App2 Redirect URI: `x-msauth-mytestiosapp://com.myapp.mytestapp2`</span></span>

<span data-ttu-id="5e135-198">App3 omdirigerings-URI:`x-msauth-mytestiosapp://com.myapp.mytestapp3`</span><span class="sxs-lookup"><span data-stu-id="5e135-198">App3 Redirect URI: `x-msauth-mytestiosapp://com.myapp.mytestapp3`</span></span>

<span data-ttu-id="5e135-199">....</span><span class="sxs-lookup"><span data-stu-id="5e135-199">....</span></span>

<span data-ttu-id="5e135-200">Dessa kapslas under hello samma klient-ID / program-ID och slås upp utifrån hello omdirigerings-URI som du kommer tillbaka toous i SDK-konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="5e135-200">These are nested under hello same client ID / application ID and looked up based on hello redirect URI you return toous in your SDK configuration.</span></span>

```
+-------------------+
|                   |
|  Client ID        |
+---------+---------+
          |
          |           +-----------------------------------+
          |           |  App 1 Redirect URI               |
          +----------^+                                   |
          |           +-----------------------------------+
          |
          |           +-----------------------------------+
          +----------^+  App 2 Redirect URI               |
          |           |                                   |
          |           +-----------------------------------+
          |
          +----------^+-----------------------------------+
                      |  App 3 Redirect URI               |
                      |                                   |
                      +-----------------------------------+

```


<span data-ttu-id="5e135-201">*Observera att hello format dessa omdirigerings-URI: er beskrivs nedan. Du kan använda alla omdirigerings-URI om du vill toosupport hello broker, i vilket fall måste de se ut ungefär så hello ovan*</span><span class="sxs-lookup"><span data-stu-id="5e135-201">*Note that hello format of these Redirect URIs are explained below. You may use any Redirect URI unless you wish toosupport hello broker, in which case they must look something like hello above*</span></span>

#### <a name="create-keychain-sharing-between-applications"></a><span data-ttu-id="5e135-202">Skapa mellan program för delning av nyckelringar</span><span class="sxs-lookup"><span data-stu-id="5e135-202">Create keychain sharing between applications</span></span>
<span data-ttu-id="5e135-203">Aktivera delning av nyckelringar ligger utanför det här dokumentet hello och omfattas av Apple i dokumentet [lägga till funktioner](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html).</span><span class="sxs-lookup"><span data-stu-id="5e135-203">Enabling keychain sharing is beyond hello scope of this document and covered by Apple in their document [Adding Capabilities](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html).</span></span> <span data-ttu-id="5e135-204">Vad är viktigt är att du bestämma vad du vill att din nyckelringar toobe kallas och lägga till den här funktionen i alla program.</span><span class="sxs-lookup"><span data-stu-id="5e135-204">What is important is that you decide what you want your keychain toobe called and add that capability across all your applications.</span></span>

<span data-ttu-id="5e135-205">När du har rättigheter som är konfigurerat på rätt sätt bör du se en fil i projektkatalogen rätt `entitlements.plist` som innehåller något som ser ut som följande hello:</span><span class="sxs-lookup"><span data-stu-id="5e135-205">When you do have entitlements set up correctly you should see a file in your project directory entitled `entitlements.plist` that contains something that looks like hello following:</span></span>

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>keychain-access-groups</key>
    <array>
        <string>$(AppIdentifierPrefix)com.myapp.mytestapp</string>
        <string>$(AppIdentifierPrefix)com.myapp.mycache</string>
    </array>
</dict>
</plist>
```

<span data-ttu-id="5e135-206">När du har hello nyckelringar rätt aktiverat i var och en av dina program och du är redo toouse SSO, berätta hello Microsoft Identity SDK nyckelringen med hjälp av hello följande inställning i din `ADAuthenticationSettings` med hello följande inställning:</span><span class="sxs-lookup"><span data-stu-id="5e135-206">Once you have hello keychain entitlement enabled in each of your applications, and you are ready toouse SSO, tell hello Microsoft Identity SDK about your keychain by using hello following setting in your `ADAuthenticationSettings` with hello following setting:</span></span>

```
defaultKeychainSharingGroup=@"com.myapp.mycache";
```

> [!WARNING]
> <span data-ttu-id="5e135-207">När du delar en nyckelring över dina program kan alla program ta bort användare eller värre ta bort alla hello-token för ditt program.</span><span class="sxs-lookup"><span data-stu-id="5e135-207">When you share a keychain across your applications any application can delete users or worse delete all hello tokens across your application.</span></span> <span data-ttu-id="5e135-208">Detta är särskilt katastrofal om du har program som förlitar sig på hello token toodo bakgrundsjobbet.</span><span class="sxs-lookup"><span data-stu-id="5e135-208">This is particularly disastrous if you have applications that rely on hello tokens toodo background work.</span></span> <span data-ttu-id="5e135-209">Dela en nyckelring remove innebär att du måste vara mycket försiktig i alla-åtgärder via hello Microsoft Identity SDK: er.</span><span class="sxs-lookup"><span data-stu-id="5e135-209">Sharing a keychain means that you must be very careful in any and all remove operations through hello Microsoft Identity SDKs.</span></span>
> 
> 

<span data-ttu-id="5e135-210">Klart!</span><span class="sxs-lookup"><span data-stu-id="5e135-210">That's it!</span></span> <span data-ttu-id="5e135-211">hello Microsoft Identity SDK kommer nu att dela autentiseringsuppgifter i alla program.</span><span class="sxs-lookup"><span data-stu-id="5e135-211">hello Microsoft Identity SDK will now share credentials across all your applications.</span></span> <span data-ttu-id="5e135-212">hello användarlistan också delas mellan programinstanser.</span><span class="sxs-lookup"><span data-stu-id="5e135-212">hello user list will also be shared across application instances.</span></span>

### <a name="turning-on-sso-for-broker-assisted-sso"></a><span data-ttu-id="5e135-213">Aktivera enkel inloggning för broker stödd SSO</span><span class="sxs-lookup"><span data-stu-id="5e135-213">Turning on SSO for broker assisted SSO</span></span>
<span data-ttu-id="5e135-214">Hej möjligheten för ett program toouse alla broker som är installerade på hello enheten är **inaktiverad som standard**.</span><span class="sxs-lookup"><span data-stu-id="5e135-214">hello ability for an application toouse any broker that is installed on hello device is **turned off by default**.</span></span> <span data-ttu-id="5e135-215">Ordna toouse ditt program med hello broker måste du göra ytterligare konfigurering och lägga till vissa kod tooyour program.</span><span class="sxs-lookup"><span data-stu-id="5e135-215">In order toouse your application with hello broker you must do some additional configuration and add some code tooyour application.</span></span>

<span data-ttu-id="5e135-216">hello steg toofollow är:</span><span class="sxs-lookup"><span data-stu-id="5e135-216">hello steps toofollow are:</span></span>

1. <span data-ttu-id="5e135-217">Aktivera Service broker-läget i din programkod anropet toohello MS SDK.</span><span class="sxs-lookup"><span data-stu-id="5e135-217">Enable broker mode in your application code's call toohello MS SDK.</span></span>
2. <span data-ttu-id="5e135-218">Upprätta en ny omdirigerings-URI och anger tooboth hello appen och appen registreringen.</span><span class="sxs-lookup"><span data-stu-id="5e135-218">Establish a new redirect URI and provide that tooboth hello app and your app registration.</span></span>
3. <span data-ttu-id="5e135-219">Registrera en URL-schema.</span><span class="sxs-lookup"><span data-stu-id="5e135-219">Registering a URL Scheme.</span></span>
4. <span data-ttu-id="5e135-220">Stöd för iOS9: lägga till en behörighet tooyour info.plist-fil.</span><span class="sxs-lookup"><span data-stu-id="5e135-220">iOS9 Support: Add a permission tooyour info.plist file.</span></span>

#### <a name="step-1-enable-broker-mode-in-your-application"></a><span data-ttu-id="5e135-221">Steg 1: Aktivera Service broker-läge i ditt program</span><span class="sxs-lookup"><span data-stu-id="5e135-221">Step 1: Enable broker mode in your application</span></span>
<span data-ttu-id="5e135-222">hello möjligheten för ditt program toouse hello broker är aktiverat när du skapar hello ”kontexten” eller installationen av autentisering-objektet.</span><span class="sxs-lookup"><span data-stu-id="5e135-222">hello ability for your application toouse hello broker is turned on when you create hello "context" or initial setup of your Authentication object.</span></span> <span data-ttu-id="5e135-223">Du kan göra detta genom att ange vilken typ av autentiseringsuppgifter i koden:</span><span class="sxs-lookup"><span data-stu-id="5e135-223">You do this by setting your credentials type in your code:</span></span>

```
/*! See hello ADCredentialsType enumeration definition for details */
@propertyADCredentialsType credentialsType;
```
<span data-ttu-id="5e135-224">Hej `AD_CREDENTIALS_AUTO` inställningen medger hello Microsoft Identity SDK tootry toocall ut toohello broker `AD_CREDENTIALS_EMBEDDED` förhindrar hello Microsoft Identity SDK anropar toohello broker.</span><span class="sxs-lookup"><span data-stu-id="5e135-224">hello `AD_CREDENTIALS_AUTO` setting will allow hello Microsoft Identity SDK tootry toocall out toohello broker, `AD_CREDENTIALS_EMBEDDED` will prevent hello Microsoft Identity SDK from calling toohello broker.</span></span>

#### <a name="step-2-registering-a-url-scheme"></a><span data-ttu-id="5e135-225">Steg 2: Registrera en URL-schema</span><span class="sxs-lookup"><span data-stu-id="5e135-225">Step 2: Registering a URL Scheme</span></span>
<span data-ttu-id="5e135-226">hello Microsoft Identity-plattformen använder URL: er tooinvoke hello broker och gå sedan tillbaka tooyour kontrollprogrammet.</span><span class="sxs-lookup"><span data-stu-id="5e135-226">hello Microsoft Identity platform uses URLs tooinvoke hello broker and then return control back tooyour application.</span></span> <span data-ttu-id="5e135-227">toofinish serveranrop som du behöver ett URL-schema registrerats för ditt program att hello Microsoft Identity plattform vet om.</span><span class="sxs-lookup"><span data-stu-id="5e135-227">toofinish that round trip you need a URL scheme registered for your application that hello Microsoft Identity platform will know about.</span></span> <span data-ttu-id="5e135-228">Detta kan vara dessutom tooany andra app-scheman som du kanske redan har registrerat med ditt program.</span><span class="sxs-lookup"><span data-stu-id="5e135-228">This can be in addition tooany other app schemes you may have previously registered with your application.</span></span>

> [!WARNING]
> <span data-ttu-id="5e135-229">Vi rekommenderar att du gör hello URL-schema ganska unika toominimize hello risken för en annan app med hello samma URL-schema.</span><span class="sxs-lookup"><span data-stu-id="5e135-229">We recommend making hello URL scheme fairly unique toominimize hello chances of another app using hello same URL scheme.</span></span> <span data-ttu-id="5e135-230">Apple tillämpar inte hello unika URL-scheman som är registrerade i hello app store.</span><span class="sxs-lookup"><span data-stu-id="5e135-230">Apple does not enforce hello uniqueness of URL schemes that are registered in hello app store.</span></span>
> 
> 

<span data-ttu-id="5e135-231">Nedan visas ett exempel på hur den visas i projektkonfigurationen av.</span><span class="sxs-lookup"><span data-stu-id="5e135-231">Below is an example of how this appears in your project configuration.</span></span> <span data-ttu-id="5e135-232">Du kan också göra detta i XCode samt:</span><span class="sxs-lookup"><span data-stu-id="5e135-232">You may also do this in XCode as well:</span></span>

```
<key>CFBundleURLTypes</key>
<array>
    <dict>
        <key>CFBundleTypeRole</key>
        <string>Editor</string>
        <key>CFBundleURLName</key>
        <string>com.myapp.mytestapp</string>
        <key>CFBundleURLSchemes</key>
        <array>
            <string>x-msauth-mytestiosapp</string>
        </array>
    </dict>
</array>
```

#### <a name="step-3-establish-a-new-redirect-uri-with-your-url-scheme"></a><span data-ttu-id="5e135-233">Steg 3: Upprätta en ny omdirigerings-URI med URL-schema</span><span class="sxs-lookup"><span data-stu-id="5e135-233">Step 3: Establish a new redirect URI with your URL Scheme</span></span>
<span data-ttu-id="5e135-234">I ordning tooensure att vi alltid returnera hello autentiseringsuppgifter tokens toohello rätt program, måste vi toomake att vi kallar tillbaka tooyour programmet på ett sätt som hello iOS-operativsystemet kan verifiera.</span><span class="sxs-lookup"><span data-stu-id="5e135-234">In order tooensure that we always return hello credential tokens toohello correct application, we need toomake sure we call back tooyour application in a way that hello iOS operating system can verify.</span></span> <span data-ttu-id="5e135-235">hello iOS operativsystemet rapporter toohello Microsoft broker program hello paket-ID för hello programmet anropas.</span><span class="sxs-lookup"><span data-stu-id="5e135-235">hello iOS operating system reports toohello Microsoft broker applications hello Bundle ID of hello application calling it.</span></span> <span data-ttu-id="5e135-236">Detta kan vara falsk en otillåtna program.</span><span class="sxs-lookup"><span data-stu-id="5e135-236">This cannot be spoofed by a rogue application.</span></span> <span data-ttu-id="5e135-237">Därför kan utnyttja vi detta tillsammans med hello URI för våra broker programmet tooensure hello token returneras toohello rätt program.</span><span class="sxs-lookup"><span data-stu-id="5e135-237">Therefore, we leverage this along with hello URI of our broker application tooensure that hello tokens are returned toohello correct application.</span></span> <span data-ttu-id="5e135-238">Vi behöver du tooestablish denna unika omdirigerings-URI både i ditt program och anges som en omdirigerings-URI i vår developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="5e135-238">We require you tooestablish this unique redirect URI both in your application and set as a Redirect URI in our developer portal.</span></span>

<span data-ttu-id="5e135-239">Omdirigerings-URI måste vara i hello rätt form av:</span><span class="sxs-lookup"><span data-stu-id="5e135-239">Your redirect URI must be in hello proper form of:</span></span>

`<app-scheme>://<your.bundle.id>`

<span data-ttu-id="5e135-240">ex: *x-msauth-mytestiosapp://com.myapp.mytestapp*</span><span class="sxs-lookup"><span data-stu-id="5e135-240">ex: *x-msauth-mytestiosapp://com.myapp.mytestapp*</span></span>

<span data-ttu-id="5e135-241">Den här omdirigerings-URI måste toobe som angetts i registreringen app med hello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="5e135-241">This Redirect URI needs toobe specified in your app registration using hello [Azure portal](https://portal.azure.com/).</span></span> <span data-ttu-id="5e135-242">Mer information om registrering av Azure AD app finns [integrera med Azure Active Directory](active-directory-how-to-integrate.md).</span><span class="sxs-lookup"><span data-stu-id="5e135-242">For more information on Azure AD app registration, see [Integrating with Azure Active Directory](active-directory-how-to-integrate.md).</span></span>

##### <a name="step-3a-add-a-redirect-uri-in-your-app-and-dev-portal-toosupport-certificate-based-authentication"></a><span data-ttu-id="5e135-243">Steg 3a: lägga till en omdirigerings-URI i din app och dev portal toosupport certifikatbaserad autentisering</span><span class="sxs-lookup"><span data-stu-id="5e135-243">Step 3a: Add a redirect URI in your app and dev portal toosupport certificate based authentication</span></span>
<span data-ttu-id="5e135-244">en andra ”msauth”-toosupport certifikatbaserad autentisering måste toobe som registrerats i programmet och hello [Azure-portalen](https://portal.azure.com/) toohandle certifikatautentisering om du inte vill tooadd som stöder i ditt program.</span><span class="sxs-lookup"><span data-stu-id="5e135-244">toosupport cert based authentication a second "msauth"  needs toobe registered in your application and hello [Azure portal](https://portal.azure.com/) toohandle certificate authentication if you wish tooadd that support in your application.</span></span>

`msauth://code/<broker-redirect-uri-in-url-encoded-form>`

<span data-ttu-id="5e135-245">ex: *msauth://code/x-msauth-mytestiosapp%3A%2F%2Fcom.myapp.mytestapp*</span><span class="sxs-lookup"><span data-stu-id="5e135-245">ex: *msauth://code/x-msauth-mytestiosapp%3A%2F%2Fcom.myapp.mytestapp*</span></span>

#### <a name="step-4-ios9-add-a-configuration-parameter-tooyour-app"></a><span data-ttu-id="5e135-246">Steg 4: iOS9: Lägg till en konfiguration parametern tooyour app</span><span class="sxs-lookup"><span data-stu-id="5e135-246">Step 4: iOS9: Add a configuration parameter tooyour app</span></span>
<span data-ttu-id="5e135-247">ADAL använder – canOpenURL: toocheck om hello broker är installerad på hello enhet.</span><span class="sxs-lookup"><span data-stu-id="5e135-247">ADAL uses –canOpenURL: toocheck if hello broker is installed on hello device.</span></span> <span data-ttu-id="5e135-248">I iOS låsta 9 Apple scheman ett program kan fråga efter.</span><span class="sxs-lookup"><span data-stu-id="5e135-248">In iOS 9 Apple locked down what schemes an application can query for.</span></span> <span data-ttu-id="5e135-249">Du behöver tooadd ”msauth” toohello LSApplicationQueriesSchemes avsnitt i din `info.plist file`.</span><span class="sxs-lookup"><span data-stu-id="5e135-249">You will need tooadd “msauth” toohello LSApplicationQueriesSchemes section of your `info.plist file`.</span></span>

<span data-ttu-id="5e135-250"><key>LSApplicationQueriesSchemes</key></span><span class="sxs-lookup"><span data-stu-id="5e135-250"><key>LSApplicationQueriesSchemes</key></span></span>

<span data-ttu-id="5e135-251"><array><string>msauth</string>
</array></span><span class="sxs-lookup"><span data-stu-id="5e135-251"><array> <string>msauth</string>
</array></span></span>

### <a name="youve-configured-sso"></a><span data-ttu-id="5e135-252">Du har konfigurerat SSO!</span><span class="sxs-lookup"><span data-stu-id="5e135-252">You've configured SSO!</span></span>
<span data-ttu-id="5e135-253">Nu hello Microsoft Identity SDK automatiskt både dela autentiseringsuppgifter i dina program och anropa hello broker om den finns på sin enhet.</span><span class="sxs-lookup"><span data-stu-id="5e135-253">Now hello Microsoft Identity SDK will automatically both share credentials across your applications and invoke hello broker if it's present on their device.</span></span>

