---
title: "aaaHow tooenable enkel inloggning mellan appar på Android använder ADAL | Microsoft Docs"
description: 'Hur hello toouse hello funktioner i ADAL SDK tooenable enkel inloggning i ditt program. '
services: active-directory
documentationcenter: 
author: danieldobalian
manager: mbaldwin
editor: 
ms.assetid: 40710225-05ab-40a3-9aec-8b4e96b6b5e7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: android
ms.devlang: java
ms.topic: article
ms.date: 04/07/2017
ms.author: dadobali
ms.custom: aaddev
ms.openlocfilehash: 3867e15030e5516464e4dbd92ba35894430daf00
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooenable-cross-app-sso-on-android-using-adal"></a><span data-ttu-id="dc78c-103">Hur tooenable enkel inloggning mellan appar på Android använder ADAL</span><span class="sxs-lookup"><span data-stu-id="dc78c-103">How tooenable cross-app SSO on Android using ADAL</span></span>
<span data-ttu-id="dc78c-104">Att tillhandahålla enkel inloggning (SSO) så att användarna bara behöver tooenter sina autentiseringsuppgifter en gång och har de autentiseringsuppgifterna som automatiskt fungerar över program förväntas nu av kunder.</span><span class="sxs-lookup"><span data-stu-id="dc78c-104">Providing Single Sign-On (SSO) so that users only need tooenter their credentials once and have those credentials automatically work across applications is now expected by customers.</span></span> <span data-ttu-id="dc78c-105">hello svårt att ange sina användarnamn och lösenord på en liten skärm, ofta gånger kombineras med ytterligare en faktor (2FA) som ett telefonsamtal eller en textläge kod leder till att snabbt klagomål om en användare har toodo detta mer än en gång till produkten.</span><span class="sxs-lookup"><span data-stu-id="dc78c-105">hello difficulty in entering their username and password on a small screen, often times combined with an additional factor (2FA) like a phone call or a texted code, results in quick dissatisfaction if a user has toodo this more than one time for your product.</span></span>

<span data-ttu-id="dc78c-106">Dessutom, om du använder en identity-plattform som andra program kan använda, till exempel Microsoft Accounts eller ett arbetskonto från Office365, kunder förväntar sig att dessa autentiseringsuppgifter toobe tillgängliga toouse i alla program, oavsett hello leverantör.</span><span class="sxs-lookup"><span data-stu-id="dc78c-106">In addition, if you apply an identity platform that other applications may use such as Microsoft Accounts or a work account from Office365, customers expect that those credentials toobe available toouse across all their applications no matter hello vendor.</span></span>

<span data-ttu-id="dc78c-107">hello Microsoft Identity-plattformen, tillsammans med vår Microsoft Identity-SDK: er, har den här tunga arbetet för dig och ger du hello möjlighet toodelight dina kunder med enkel inloggning antingen inom din egen suite av program eller som med våra broker kapaciteten och autentiseraren program över hello hela enheten.</span><span class="sxs-lookup"><span data-stu-id="dc78c-107">hello Microsoft Identity platform, along with our Microsoft Identity SDKs, does all this hard work for you and gives you hello ability toodelight your customers with SSO either within your own suite of applications or, as with our broker capability and Authenticator applications, across hello entire device.</span></span>

<span data-ttu-id="dc78c-108">Den här genomgången får du veta hur tooconfigure våra SDK i ditt program tooprovide denna förmån tooyour kunder.</span><span class="sxs-lookup"><span data-stu-id="dc78c-108">This walkthrough will tell you how tooconfigure our SDK within your application tooprovide this benefit tooyour customers.</span></span>

<span data-ttu-id="dc78c-109">Den här genomgången gäller för:</span><span class="sxs-lookup"><span data-stu-id="dc78c-109">This walkthrough applies to:</span></span>

* <span data-ttu-id="dc78c-110">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="dc78c-110">Azure Active Directory</span></span>
* <span data-ttu-id="dc78c-111">Azure Active Directory B2C</span><span class="sxs-lookup"><span data-stu-id="dc78c-111">Azure Active Directory B2C</span></span>
* <span data-ttu-id="dc78c-112">Azure Active Directory B2B</span><span class="sxs-lookup"><span data-stu-id="dc78c-112">Azure Active Directory B2B</span></span>
* <span data-ttu-id="dc78c-113">Villkorsstyrd åtkomst med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="dc78c-113">Azure Active Directory Conditional Access</span></span>

<span data-ttu-id="dc78c-114">hello dokumentet ovan förutsätter att du vet hur för[etablera program på hello äldre portalen för Azure Active Directory](active-directory-how-to-integrate.md) och integrerad ditt program med hello [Microsoft Identity Android SDK](https://github.com/AzureAD/azure-activedirectory-library-for-android) .</span><span class="sxs-lookup"><span data-stu-id="dc78c-114">hello document preceding assumes you know how too[provision applications in hello legacy portal for Azure Active Directory](active-directory-how-to-integrate.md) and integrated your application with hello [Microsoft Identity Android SDK](https://github.com/AzureAD/azure-activedirectory-library-for-android).</span></span>

## <a name="sso-concepts-in-hello-microsoft-identity-platform"></a><span data-ttu-id="dc78c-115">SSO begrepp i hello Microsoft Identity-plattformen</span><span class="sxs-lookup"><span data-stu-id="dc78c-115">SSO Concepts in hello Microsoft Identity Platform</span></span>
### <a name="microsoft-identity-brokers"></a><span data-ttu-id="dc78c-116">Microsoft Identity mäklare</span><span class="sxs-lookup"><span data-stu-id="dc78c-116">Microsoft Identity Brokers</span></span>
<span data-ttu-id="dc78c-117">Microsoft tillhandahåller program för varje mobil plattform som gör att för hello bryggning av autentiseringsuppgifter i program från olika leverantörer och gör för särskilda funktioner som kräver en enda säker plats varifrån toovalidate autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="dc78c-117">Microsoft provides applications for every mobile platform that allow for hello bridging of credentials across applications from different vendors and allows for special enhanced features that require a single secure place from where toovalidate credentials.</span></span> <span data-ttu-id="dc78c-118">Vi kallar dem **mäklare**.</span><span class="sxs-lookup"><span data-stu-id="dc78c-118">We call these **brokers**.</span></span> <span data-ttu-id="dc78c-119">På iOS och Android tillhandahålls dessa via nedladdningsbara program att kunder installeras fristående eller flyttas toohello enhet av ett företag som hanterar vissa eller alla hello enhet för sina anställda.</span><span class="sxs-lookup"><span data-stu-id="dc78c-119">On iOS and Android these are provided through downloadable applications that customers either install independently or can be pushed toohello device by a company who manages some or all of hello device for their employee.</span></span> <span data-ttu-id="dc78c-120">Dessa mäklare stöder Hantera säkerhet för vissa program eller hello hela enheten baserat på vad IT-administratörer som önskar.</span><span class="sxs-lookup"><span data-stu-id="dc78c-120">These brokers support managing security just for some applications or hello entire device based on what IT Administrators desire.</span></span> <span data-ttu-id="dc78c-121">Den här funktionen tillhandahålls av en inbyggd toohello operativsystemet tekniskt kallas hello Webbautentiseringskoordinatorn väljare av användarkonto i Windows.</span><span class="sxs-lookup"><span data-stu-id="dc78c-121">In Windows, this functionality is provided by an account chooser built in toohello operating system, known technically as hello Web Authentication Broker.</span></span>

<span data-ttu-id="dc78c-122">Mer information om hur använder vi dessa mäklare och hur dina kunder kan se dem i sina inloggningen flödet för hello Microsoft Identity plattform läsas på.</span><span class="sxs-lookup"><span data-stu-id="dc78c-122">For more information on how we use these brokers and how your customers might see them in their login flow for hello Microsoft Identity platform read on.</span></span>

### <a name="patterns-for-logging-in-on-mobile-devices"></a><span data-ttu-id="dc78c-123">Mönster för att logga in på mobila enheter</span><span class="sxs-lookup"><span data-stu-id="dc78c-123">Patterns for logging in on mobile devices</span></span>
<span data-ttu-id="dc78c-124">Åtkomst toocredentials på enheter följer två grundläggande mönster för hello Microsoft Identity plattform:</span><span class="sxs-lookup"><span data-stu-id="dc78c-124">Access toocredentials on devices follow two basic patterns for hello Microsoft Identity platform:</span></span>

* <span data-ttu-id="dc78c-125">Icke-förhandlad assisterad inloggningar</span><span class="sxs-lookup"><span data-stu-id="dc78c-125">Non-broker assisted logins</span></span>
* <span data-ttu-id="dc78c-126">Broker assisterad inloggningar</span><span class="sxs-lookup"><span data-stu-id="dc78c-126">Broker assisted logins</span></span>

#### <a name="non-broker-assisted-logins"></a><span data-ttu-id="dc78c-127">Icke-förhandlad assisterad inloggningar</span><span class="sxs-lookup"><span data-stu-id="dc78c-127">Non-broker assisted logins</span></span>
<span data-ttu-id="dc78c-128">Icke-förhandlad assisterad inloggningar är inloggning upplevelser som inträffa infogade med hello program och Använd hello lokal lagring på hello enhet för programmet.</span><span class="sxs-lookup"><span data-stu-id="dc78c-128">Non-broker assisted logins are login experiences that happen inline with hello application and use hello local storage on hello device for that application.</span></span> <span data-ttu-id="dc78c-129">Lagringen kan delas mellan program men hello autentiseringsuppgifterna är tätt bundna toohello app eller en uppsättning appar som använder denna autentiseringsuppgift.</span><span class="sxs-lookup"><span data-stu-id="dc78c-129">This storage may be shared across applications but hello credentials are tightly bound toohello app or suite of apps using that credential.</span></span> <span data-ttu-id="dc78c-130">Du har förmodligen fått det i många mobila program när du anger ett användarnamn och lösenord i själva hello-programmet.</span><span class="sxs-lookup"><span data-stu-id="dc78c-130">You've most likely experienced this in many mobile applications when you enter a username and password within hello application itself.</span></span>

<span data-ttu-id="dc78c-131">Dessa inloggningar har hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="dc78c-131">These logins have hello following benefits:</span></span>

* <span data-ttu-id="dc78c-132">Användarupplevelsen finns helt i hello-programmet.</span><span class="sxs-lookup"><span data-stu-id="dc78c-132">User experience exists entirely within hello application.</span></span>
* <span data-ttu-id="dc78c-133">Autentiseringsuppgifter kan delas mellan program som är signerade av hello samma certifikat, vilket ger en enkel inloggning tooyour uppsättning program.</span><span class="sxs-lookup"><span data-stu-id="dc78c-133">Credentials can be shared across applications that are signed by hello same certificate, providing a single sign-on experience tooyour suite of applications.</span></span>
* <span data-ttu-id="dc78c-134">Kontrollen runt hello upplevelse av loggning i finns toohello program före och efter inloggning.</span><span class="sxs-lookup"><span data-stu-id="dc78c-134">Control around hello experience of logging in is provided toohello application before and after sign-in.</span></span>

<span data-ttu-id="dc78c-135">Dessa inloggningar har följande nackdelar hello:</span><span class="sxs-lookup"><span data-stu-id="dc78c-135">These logins have hello following drawbacks:</span></span>

* <span data-ttu-id="dc78c-136">Enkel inloggning på kan inte användarupplevelse över alla appar som använder Microsoft-Identity enbart över de Microsoft-Identities som programmet har konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="dc78c-136">User cannot experience single-sign on across all apps that use a Microsoft Identity, only across those Microsoft Identities that your application has configured.</span></span>
* <span data-ttu-id="dc78c-137">Programmet kan inte användas med mer avancerade funktioner för företag till exempel villkorlig åtkomst eller Använd hello InTune-produkter.</span><span class="sxs-lookup"><span data-stu-id="dc78c-137">Your application cannot be used with more advanced business features such as Conditional Access or use hello InTune suite of products.</span></span>
* <span data-ttu-id="dc78c-138">Programmet stöder inte certifikatbaserad autentisering för företagsanvändare.</span><span class="sxs-lookup"><span data-stu-id="dc78c-138">Your application can't support certificate-based authentication for business users.</span></span>

<span data-ttu-id="dc78c-139">Här är en representation av hur hello Microsoft Identity SDK fungerar med hello delad lagring av ditt program tooenable enkel inloggning:</span><span class="sxs-lookup"><span data-stu-id="dc78c-139">Here is a representation of how hello Microsoft Identity SDKs work with hello shared storage of your applications tooenable SSO:</span></span>

```
+------------+ +------------+  +-------------+
|            | |            |  |             |
|   App 1    | |   App 2    |  |   App 3     |
|            | |            |  |             |
|            | |            |  |             |
+------------+ +------------+  +-------------+
| Azure SDK  | | Azure SDK  |  | Azure SDK   |
+------------+-+------------+--+-------------+
|                                            |
|            App Shared Storage              |
+--------------------------------------------+
```

#### <a name="broker-assisted-logins"></a><span data-ttu-id="dc78c-140">Broker assisterad inloggningar</span><span class="sxs-lookup"><span data-stu-id="dc78c-140">Broker assisted logins</span></span>
<span data-ttu-id="dc78c-141">Service Broker-stödd inloggningar är inloggning upplevelser som sker inom hello broker program och använder hello lagring och säkerhet för hello broker tooshare autentiseringsuppgifter för alla program på hello enhet som gäller hello Microsoft Identity-plattformen.</span><span class="sxs-lookup"><span data-stu-id="dc78c-141">Broker-assisted logins are login experiences that occur within hello broker application and use hello storage and security of hello broker tooshare credentials across all applications on hello device that apply hello Microsoft Identity platform.</span></span> <span data-ttu-id="dc78c-142">Detta innebär att dina program som förlitar sig på hello broker toosign användare i.</span><span class="sxs-lookup"><span data-stu-id="dc78c-142">This means that your applications rely on hello broker toosign users in.</span></span> <span data-ttu-id="dc78c-143">På iOS och Android sker dessa mäklare via nedladdningsbara program att kunder installeras fristående eller flyttas toohello enhet av ett företag som hanterar hello enheten för sina användare.</span><span class="sxs-lookup"><span data-stu-id="dc78c-143">On iOS and Android these brokers are provided through downloadable applications that customers either install independently or can be pushed toohello device by a company who manages hello device for their user.</span></span> <span data-ttu-id="dc78c-144">Ett exempel på den här typen av program är hello Microsoft Authenticator programmet på iOS.</span><span class="sxs-lookup"><span data-stu-id="dc78c-144">An example of this type of application is hello Microsoft Authenticator application on iOS.</span></span> <span data-ttu-id="dc78c-145">Den här funktionen tillhandahålls av en inbyggd toohello operativsystemet tekniskt kallas hello Webbautentiseringskoordinatorn väljare av användarkonto i Windows.</span><span class="sxs-lookup"><span data-stu-id="dc78c-145">In Windows this functionality is provided by an account chooser built in toohello operating system, known technically as hello Web Authentication Broker.</span></span>
<span data-ttu-id="dc78c-146">hello upplevelse varierar efter plattform och ibland kan vara störande toousers om de inte hanteras på rätt sätt.</span><span class="sxs-lookup"><span data-stu-id="dc78c-146">hello experience varies by platform and can sometimes be disruptive toousers if not managed correctly.</span></span> <span data-ttu-id="dc78c-147">Du är mest förmodligen är bekant med det här mönstret om du har installerat hello Facebook-program och använder Facebook ansluta från ett annat program.</span><span class="sxs-lookup"><span data-stu-id="dc78c-147">You're probably most familiar with this pattern if you have hello Facebook application installed and use Facebook Connect from another application.</span></span> <span data-ttu-id="dc78c-148">hello hello Microsoft Identity-plattformen använder samma mönster.</span><span class="sxs-lookup"><span data-stu-id="dc78c-148">hello Microsoft Identity platform uses hello same pattern.</span></span>

<span data-ttu-id="dc78c-149">För iOS leder tooa ”övergången” animering där programmet skickas toohello bakgrunden medan hello Microsoft Authenticator program kommer toohello förgrunden för hello användaren tooselect vilket konto som de vill ha toosign med.</span><span class="sxs-lookup"><span data-stu-id="dc78c-149">For iOS this leads tooa "transition" animation where your application is sent toohello background while hello Microsoft Authenticator applications comes toohello foreground for hello user tooselect which account they would like toosign in with.</span></span>  

<span data-ttu-id="dc78c-150">För Android och Windows hello konto visas Väljaren ovanpå ditt program som är mindre störande toohello användare.</span><span class="sxs-lookup"><span data-stu-id="dc78c-150">For Android and Windows hello account chooser is displayed on top of your application which is less disruptive toohello user.</span></span>

#### <a name="how-hello-broker-gets-invoked"></a><span data-ttu-id="dc78c-151">Hur hello broker hämtar anropas</span><span class="sxs-lookup"><span data-stu-id="dc78c-151">How hello broker gets invoked</span></span>
<span data-ttu-id="dc78c-152">Om en kompatibel broker är installerad på hello enhet, som hello Microsoft Authenticator programmet hello Microsoft Identity SDK: er kommer automatiskt hello arbete anropar hello broker för dig när en användare anger de önskar toolog in med ett konto från hello Microsoft Identity plattform.</span><span class="sxs-lookup"><span data-stu-id="dc78c-152">If a compatible broker is installed on hello device, like hello Microsoft Authenticator application, hello Microsoft Identity SDKs will automatically do hello work of invoking hello broker for you when a user indicates they wish toolog in using any account from hello Microsoft Identity platform.</span></span> <span data-ttu-id="dc78c-153">Det här kontot kan vara ett personligt Microsoft-Account, ett arbets eller skolkonto, eller ett konto som du anger och värden i Azure med hjälp av vår B2C och B2B-produkter.</span><span class="sxs-lookup"><span data-stu-id="dc78c-153">This account could be a personal Microsoft Account, a work or school account, or an account that you provide and host in Azure using our B2C and B2B products.</span></span> 
 
 #### <a name="how-we-ensure-hello-application-is-valid"></a><span data-ttu-id="dc78c-154">Hur vi Kontrollera hello program är giltig</span><span class="sxs-lookup"><span data-stu-id="dc78c-154">How we ensure hello application is valid</span></span>
 
 <span data-ttu-id="dc78c-155">hello måste tooensure hello identiteten för ett program anropet hello broker är avgörande toohello säkerhet som vi tillhandahåller broker stödd inloggningar.</span><span class="sxs-lookup"><span data-stu-id="dc78c-155">hello need tooensure hello identity of an application call hello broker is crucial toohello security we provide in broker assisted logins.</span></span> <span data-ttu-id="dc78c-156">Varken iOS eller Android tillämpar unika identifierare är endast giltiga för ett visst program så att skadliga program kan ”förfalska” legitima program-ID och ta emot hello token är avsedda för hello legitima program.</span><span class="sxs-lookup"><span data-stu-id="dc78c-156">Neither iOS nor Android enforces unique identifiers that are valid only for a given application, so malicious applications may "spoof" a legitimate application's identifier and receive hello tokens meant for hello legitimate application.</span></span> <span data-ttu-id="dc78c-157">tooensure vi alltid kommunicerar med hello rätt program vid körning, ber vi hello developer tooprovide anpassade redirectURI när de registrerar sina program med Microsoft.</span><span class="sxs-lookup"><span data-stu-id="dc78c-157">tooensure we are always communicating with hello right application at runtime, we ask hello developer tooprovide a custom redirectURI when registering their application with Microsoft.</span></span> <span data-ttu-id="dc78c-158">**Hur utvecklare ska använda för att skapa den här omdirigerings-URI diskuteras i detalj nedan.**</span><span class="sxs-lookup"><span data-stu-id="dc78c-158">**How developers should craft this redirect URI is discussed in detail below.**</span></span> <span data-ttu-id="dc78c-159">Den här anpassade redirectURI innehåller hello tumavtrycket för programmet hello och toobe unika toohello program säkerställs genom hello Google Play-butiken.</span><span class="sxs-lookup"><span data-stu-id="dc78c-159">This custom redirectURI contains hello certificate thumbprint of hello application and is ensured toobe unique toohello application by hello Google Play Store.</span></span> <span data-ttu-id="dc78c-160">När ett program anropar hello broker hello broker frågar hello operativsystemet Android tooprovide med hello tumavtryck för certifikat som kallas hello broker.</span><span class="sxs-lookup"><span data-stu-id="dc78c-160">When an application calls hello broker, hello broker asks hello Android operating system tooprovide it with hello certificate thumbprint that called hello broker.</span></span> <span data-ttu-id="dc78c-161">hello broker ger tooMicrosoft detta certifikat-tumavtrycket i hello anropet tooour identitetssystem.</span><span class="sxs-lookup"><span data-stu-id="dc78c-161">hello broker provides this certificate thumbprint tooMicrosoft in hello call tooour identity system.</span></span> <span data-ttu-id="dc78c-162">Om hello certifikat tumavtryck hello programmet inte matchar hello certifikatets tumavtryck toous av hello utvecklare under registreringen kommer vi kommer att neka åtkomst toohello token för hello resurs hello program begär.</span><span class="sxs-lookup"><span data-stu-id="dc78c-162">If hello certificate thumbprint of hello application does not match hello certificate thumbprint provided toous by hello developer during registration, we will deny access toohello tokens for hello resource hello application is requesting.</span></span> <span data-ttu-id="dc78c-163">Detta säkerställer att endast hello-program som är registrerat av hello utvecklare tar emot tokens.</span><span class="sxs-lookup"><span data-stu-id="dc78c-163">This check ensures that only hello application registered by hello developer receives tokens.</span></span>

<span data-ttu-id="dc78c-164">**hello utvecklare har hello valet av om hello Microsoft Identity SDK-anrop hello broker eller använder hello icke-förhandlad assisterad flödet.**</span><span class="sxs-lookup"><span data-stu-id="dc78c-164">**hello developer has hello choice of if hello Microsoft Identity SDK calls hello broker or uses hello non-broker assisted flow.**</span></span> <span data-ttu-id="dc78c-165">Men om hello utvecklare väljer inte toouse hello broker-stödd flödet de förlorar hello fördelen med att använda enkel inloggning autentiseringsuppgifter hello användaren kanske redan har lagt till på hello enhet och förhindrar att deras program som används med funktioner för företag Microsoft ger sina kunder som villkorlig åtkomst, Intune-hanteringsfunktioner och certifikatbaserad autentisering.</span><span class="sxs-lookup"><span data-stu-id="dc78c-165">However if hello developer chooses not toouse hello broker-assisted flow they lose hello benefit of using SSO credentials that hello user may have already added on hello device and prevents their application from being used with business features Microsoft provides its customers such as Conditional Access, Intune Management capabilities, and certificate-based authentication.</span></span>

<span data-ttu-id="dc78c-166">Dessa inloggningar har hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="dc78c-166">These logins have hello following benefits:</span></span>

* <span data-ttu-id="dc78c-167">Användaren upplever SSO över sina program oavsett hello leverantör.</span><span class="sxs-lookup"><span data-stu-id="dc78c-167">User experiences SSO across all their applications no matter hello vendor.</span></span>
* <span data-ttu-id="dc78c-168">Programmet kan använda mer avancerade funktioner för företag som villkorlig åtkomst eller använda hello InTune-produkter.</span><span class="sxs-lookup"><span data-stu-id="dc78c-168">Your application can use more advanced business features such as Conditional Access or use hello InTune suite of products.</span></span>
* <span data-ttu-id="dc78c-169">Programmet stöder certifikatbaserad autentisering för användare i verksamheten.</span><span class="sxs-lookup"><span data-stu-id="dc78c-169">Your application can support certificate-based authentication for business users.</span></span>
* <span data-ttu-id="dc78c-170">Mycket säkrare inloggningen som hello identitet hello program- och Hej kontrolleras av hello broker program med ytterligare säkerhetsalgoritmer och kryptering.</span><span class="sxs-lookup"><span data-stu-id="dc78c-170">Much more secure sign-in experience as hello identity of hello application and hello user are verified by hello broker application with additional security algorithms and encryption.</span></span>

<span data-ttu-id="dc78c-171">Dessa inloggningar har följande nackdelar hello:</span><span class="sxs-lookup"><span data-stu-id="dc78c-171">These logins have hello following drawbacks:</span></span>

* <span data-ttu-id="dc78c-172">I iOS övergick hello användare utanför din programupplevelse medan autentiseringsuppgifter är valt.</span><span class="sxs-lookup"><span data-stu-id="dc78c-172">In iOS hello user is transitioned out of your application's experience while credentials are chosen.</span></span>
* <span data-ttu-id="dc78c-173">Förlust av hello möjlighet toomanage hello inloggningen upplevelse för kunderna i ditt program.</span><span class="sxs-lookup"><span data-stu-id="dc78c-173">Loss of hello ability toomanage hello login experience for your customers within your application.</span></span>

<span data-ttu-id="dc78c-174">Här är en representation av hur hello Microsoft Identity SDK fungerar med hello broker program tooenable enkel inloggning:</span><span class="sxs-lookup"><span data-stu-id="dc78c-174">Here is a representation of how hello Microsoft Identity SDKs work with hello broker applications tooenable SSO:</span></span>

```
+------------+ +------------+   +-------------+
|            | |            |   |             |
|   App 1    | |   App 2    |   |   Someone   |
|            | |            |   |    Else's   |
|            | |            |   |     App     |
+------------+ +------------+   +-------------+
|  ADAL SDK  | |  ADAL SDK  |   |  ADAL SDK   |
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

<span data-ttu-id="dc78c-175">Tillsammans med den här bakgrundsinformation ska kunna toobetter att förstå och implementera enkel inloggning i ditt program med hjälp av hello Microsoft Identity plattform och SDK: er.</span><span class="sxs-lookup"><span data-stu-id="dc78c-175">Armed with this background information you should be able toobetter understand and implement SSO within your application using hello Microsoft Identity platform and SDKs.</span></span>

## <a name="enabling-cross-app-sso-using-adal"></a><span data-ttu-id="dc78c-176">Aktivera enkel inloggning mellan appar med hjälp av ADAL</span><span class="sxs-lookup"><span data-stu-id="dc78c-176">Enabling cross-app SSO using ADAL</span></span>
<span data-ttu-id="dc78c-177">Här använder vi hello ADAL Android SDK till:</span><span class="sxs-lookup"><span data-stu-id="dc78c-177">Here we use hello ADAL Android SDK to:</span></span>

* <span data-ttu-id="dc78c-178">Aktivera icke-förhandlad stödd enkel inloggning för en uppsättning appar</span><span class="sxs-lookup"><span data-stu-id="dc78c-178">Turn on non-broker assisted SSO for your suite of apps</span></span>
* <span data-ttu-id="dc78c-179">Aktivera stöd för Service broker-stödd enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="dc78c-179">Turn on support for broker-assisted SSO</span></span>

### <a name="turning-on-sso-for-non-broker-assisted-sso"></a><span data-ttu-id="dc78c-180">Aktivera enkel inloggning för icke-förhandlad stödd SSO</span><span class="sxs-lookup"><span data-stu-id="dc78c-180">Turning on SSO for non-broker assisted SSO</span></span>
<span data-ttu-id="dc78c-181">För icke-förhandlad assisterad SSO över program hantera hello Microsoft Identity SDK mycket hello komplexitet SSO du.</span><span class="sxs-lookup"><span data-stu-id="dc78c-181">For non-broker assisted SSO across applications hello Microsoft Identity SDKs manage much of hello complexity of SSO for you.</span></span> <span data-ttu-id="dc78c-182">Detta inkluderar att hitta hello rätt användare i hello cache och underhålla en lista över inloggade användare för du tooquery.</span><span class="sxs-lookup"><span data-stu-id="dc78c-182">This includes finding hello right user in hello cache and maintaining a list of logged in users for you tooquery.</span></span>

<span data-ttu-id="dc78c-183">tooenable enkel inloggning för program som du äger måste toodo hello följande:</span><span class="sxs-lookup"><span data-stu-id="dc78c-183">tooenable SSO across applications you own you need toodo hello following:</span></span>

1. <span data-ttu-id="dc78c-184">Se till att alla dina program användaren hello samma klient-ID eller program-ID.</span><span class="sxs-lookup"><span data-stu-id="dc78c-184">Ensure all your applications user hello same Client ID or Application ID.</span></span>
2. <span data-ttu-id="dc78c-185">Se till att alla program som har samma SharedUserID ange hello.</span><span class="sxs-lookup"><span data-stu-id="dc78c-185">Ensure all your applications have hello same SharedUserID set.</span></span>
3. <span data-ttu-id="dc78c-186">Kontrollera att alla dina program dela hello samma signeringscertifikat från hello Google Play lagra så att du kan dela lagring.</span><span class="sxs-lookup"><span data-stu-id="dc78c-186">Ensure that all of your applications share hello same signing certificate from hello Google Play store so that you can share storage.</span></span>

#### <a name="step-1-using-hello-same-client-id--application-id-for-all-hello-applications-in-your-suite-of-apps"></a><span data-ttu-id="dc78c-187">Steg 1: Använder hello samma klient-ID eller program-ID för alla hello program i din uppsättning appar</span><span class="sxs-lookup"><span data-stu-id="dc78c-187">Step 1: Using hello same Client ID / Application ID for all hello applications in your suite of apps</span></span>
<span data-ttu-id="dc78c-188">För att hello Microsoft Identity plattform tooknow att tillåts tooshare token för ditt program, behöver var och en av dina program tooshare hello samma klient-ID eller program-ID.</span><span class="sxs-lookup"><span data-stu-id="dc78c-188">In order for hello Microsoft Identity platform tooknow that it's allowed tooshare tokens across your applications, each of your applications will need tooshare hello same Client ID or Application ID.</span></span> <span data-ttu-id="dc78c-189">Detta är hello Unik identifierare som du har fått tooyou när du har registrerat din första programmet hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="dc78c-189">This is hello unique identifier that was provided tooyou when you registered your first application in hello portal.</span></span>

<span data-ttu-id="dc78c-190">Du kanske undrar hur du identifierar olika appar toohello Microsoft Identity-tjänsten om den använder hello samma program-ID.</span><span class="sxs-lookup"><span data-stu-id="dc78c-190">You may be wondering how you will identify different apps toohello Microsoft Identity service if it uses hello same Application ID.</span></span> <span data-ttu-id="dc78c-191">hello svaret är med hello **omdirigerings-URI: er**.</span><span class="sxs-lookup"><span data-stu-id="dc78c-191">hello answer is with hello **Redirect URIs**.</span></span> <span data-ttu-id="dc78c-192">Varje program kan ha flera omdirigerings-URI: er registrerade i hello onboarding-portalen.</span><span class="sxs-lookup"><span data-stu-id="dc78c-192">Each application can have multiple Redirect URIs registered in hello onboarding portal.</span></span> <span data-ttu-id="dc78c-193">Varje app i din suite har en annan omdirigerings-URI.</span><span class="sxs-lookup"><span data-stu-id="dc78c-193">Each app in your suite will have a different redirect URI.</span></span> <span data-ttu-id="dc78c-194">Ett exempel på hur detta ser ut understiger:</span><span class="sxs-lookup"><span data-stu-id="dc78c-194">An example of how this looks is below:</span></span>

<span data-ttu-id="dc78c-195">App1 omdirigerings-URI:`msauth://com.example.userapp/IcB5PxIyvbLkbFVtBI%2FitkW%2Fejk%3D`</span><span class="sxs-lookup"><span data-stu-id="dc78c-195">App1 Redirect URI: `msauth://com.example.userapp/IcB5PxIyvbLkbFVtBI%2FitkW%2Fejk%3D`</span></span>

<span data-ttu-id="dc78c-196">App2 omdirigerings-URI:`msauth://com.example.userapp1/KmB7PxIytyLkbGHuI%2UitkW%2Fejk%4E`</span><span class="sxs-lookup"><span data-stu-id="dc78c-196">App2 Redirect URI: `msauth://com.example.userapp1/KmB7PxIytyLkbGHuI%2UitkW%2Fejk%4E`</span></span>

<span data-ttu-id="dc78c-197">App3 omdirigerings-URI:`msauth://com.example.userapp2/Pt85PxIyvbLkbKUtBI%2SitkW%2Fejk%9F`</span><span class="sxs-lookup"><span data-stu-id="dc78c-197">App3 Redirect URI: `msauth://com.example.userapp2/Pt85PxIyvbLkbKUtBI%2SitkW%2Fejk%9F`</span></span>

<span data-ttu-id="dc78c-198">....</span><span class="sxs-lookup"><span data-stu-id="dc78c-198">....</span></span>

<span data-ttu-id="dc78c-199">Dessa kapslas under hello samma klient-ID / program-ID och slås upp utifrån hello omdirigerings-URI som du kommer tillbaka toous i SDK-konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="dc78c-199">These are nested under hello same client ID / application ID and looked up based on hello redirect URI you return toous in your SDK configuration.</span></span>

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


<span data-ttu-id="dc78c-200">*Observera att hello format dessa omdirigerings-URI: er beskrivs nedan. Du kan använda alla omdirigerings-URI om du vill toosupport hello broker, i vilket fall måste de se ut ungefär så hello ovan*</span><span class="sxs-lookup"><span data-stu-id="dc78c-200">*Note that hello format of these Redirect URIs are explained below. You may use any Redirect URI unless you wish toosupport hello broker, in which case they must look something like hello above*</span></span>

#### <a name="step-2-configuring-shared-storage-in-android"></a><span data-ttu-id="dc78c-201">Steg 2: Konfigurera delad lagring i Android</span><span class="sxs-lookup"><span data-stu-id="dc78c-201">Step 2: Configuring shared storage in Android</span></span>
<span data-ttu-id="dc78c-202">Inställningen hello `SharedUserID` ligger utanför det här dokumentet hello men kan hämtas genom att läsa hello Google Android dokumentation om hello [Manifest](http://developer.android.com/guide/topics/manifest/manifest-element.html).</span><span class="sxs-lookup"><span data-stu-id="dc78c-202">Setting hello `SharedUserID` is beyond hello scope of this document but can be learned by reading hello Google Android documentation on hello [Manifest](http://developer.android.com/guide/topics/manifest/manifest-element.html).</span></span> <span data-ttu-id="dc78c-203">Vad är viktigt är att du bestämmer vad du vill att din sharedUserID anropas och använda det i alla program.</span><span class="sxs-lookup"><span data-stu-id="dc78c-203">What is important is that you decide what you want your sharedUserID will be called and use that across all your applications.</span></span>

<span data-ttu-id="dc78c-204">När du har hello `SharedUserID` i dina program du är klar toouse enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="dc78c-204">Once you have hello `SharedUserID` in all your applications you are ready toouse SSO.</span></span>

> [!WARNING]
> <span data-ttu-id="dc78c-205">När du delar lagring över dina program kan alla program ta bort användare eller värre ta bort alla hello-token för ditt program.</span><span class="sxs-lookup"><span data-stu-id="dc78c-205">When you share storage across your applications any application can delete users, or worse delete all hello tokens across your application.</span></span> <span data-ttu-id="dc78c-206">Detta är särskilt katastrofal om du har program som förlitar sig på hello token toodo bakgrundsjobbet.</span><span class="sxs-lookup"><span data-stu-id="dc78c-206">This is particularly disastrous if you have applications that rely on hello tokens toodo background work.</span></span> <span data-ttu-id="dc78c-207">Dela lagring innebär att du måste vara mycket försiktig i alla remove-åtgärder via hello Microsoft Identity SDK: er.</span><span class="sxs-lookup"><span data-stu-id="dc78c-207">Sharing storage means that you must be very careful in any and all remove operations through hello Microsoft Identity SDKs.</span></span>
> 
> 

<span data-ttu-id="dc78c-208">Klart!</span><span class="sxs-lookup"><span data-stu-id="dc78c-208">That's it!</span></span> <span data-ttu-id="dc78c-209">hello Microsoft Identity SDK kommer nu att dela autentiseringsuppgifter i alla program.</span><span class="sxs-lookup"><span data-stu-id="dc78c-209">hello Microsoft Identity SDK will now share credentials across all your applications.</span></span> <span data-ttu-id="dc78c-210">hello användarlistan också delas mellan programinstanser.</span><span class="sxs-lookup"><span data-stu-id="dc78c-210">hello user list will also be shared across application instances.</span></span>

### <a name="turning-on-sso-for-broker-assisted-sso"></a><span data-ttu-id="dc78c-211">Aktivera enkel inloggning för broker stödd SSO</span><span class="sxs-lookup"><span data-stu-id="dc78c-211">Turning on SSO for broker assisted SSO</span></span>
<span data-ttu-id="dc78c-212">Hej möjligheten för ett program toouse alla broker som är installerade på hello enheten är **inaktiverad som standard**.</span><span class="sxs-lookup"><span data-stu-id="dc78c-212">hello ability for an application toouse any broker that is installed on hello device is **turned off by default**.</span></span> <span data-ttu-id="dc78c-213">Ordna toouse ditt program med hello broker måste du göra ytterligare konfigurering och lägga till vissa kod tooyour program.</span><span class="sxs-lookup"><span data-stu-id="dc78c-213">In order toouse your application with hello broker you must do some additional configuration and add some code tooyour application.</span></span>

<span data-ttu-id="dc78c-214">hello steg toofollow är:</span><span class="sxs-lookup"><span data-stu-id="dc78c-214">hello steps toofollow are:</span></span>

1. <span data-ttu-id="dc78c-215">Aktivera Service broker-läget i din programkod anropet toohello MS SDK</span><span class="sxs-lookup"><span data-stu-id="dc78c-215">Enable broker mode in your application code's call toohello MS SDK</span></span>
2. <span data-ttu-id="dc78c-216">Upprätta en ny omdirigerings-URI och ange tooboth hello appen och appen registreringen</span><span class="sxs-lookup"><span data-stu-id="dc78c-216">Establish a new redirect URI and provide that tooboth hello app and your app registration</span></span>
3. <span data-ttu-id="dc78c-217">Ställa in hello rätt behörigheter i hello Android-manifest</span><span class="sxs-lookup"><span data-stu-id="dc78c-217">Setting up hello correct permissions in hello Android manifest</span></span>

#### <a name="step-1-enable-broker-mode-in-your-application"></a><span data-ttu-id="dc78c-218">Steg 1: Aktivera Service broker-läge i ditt program</span><span class="sxs-lookup"><span data-stu-id="dc78c-218">Step 1: Enable broker mode in your application</span></span>
<span data-ttu-id="dc78c-219">hello möjligheten för ditt program toouse hello broker är aktiverat när du skapar hello ”inställningar” eller installationen av autentisering-instans.</span><span class="sxs-lookup"><span data-stu-id="dc78c-219">hello ability for your application toouse hello broker is turned on when you create hello "settings" or initial setup of your Authentication instance.</span></span> <span data-ttu-id="dc78c-220">Du kan göra detta genom att ange ApplicationSettings-typ i koden:</span><span class="sxs-lookup"><span data-stu-id="dc78c-220">You do this by setting your ApplicationSettings type in your code:</span></span>

```
AuthenticationSettings.Instance.setUseBroker(true);
```


#### <a name="step-2-establish-a-new-redirect-uri-with-your-url-scheme"></a><span data-ttu-id="dc78c-221">Steg 2: Skapa en ny omdirigerings-URI med URL-schema</span><span class="sxs-lookup"><span data-stu-id="dc78c-221">Step 2: Establish a new redirect URI with your URL Scheme</span></span>
<span data-ttu-id="dc78c-222">I ordning tooensure att vi alltid returnera hello autentiseringsuppgifter tokens toohello rätt program, måste vi toomake att vi kallar tillbaka tooyour programmet på ett sätt som hello Android-operativsystemet kan verifiera.</span><span class="sxs-lookup"><span data-stu-id="dc78c-222">In order tooensure that we always return hello credential tokens toohello correct application, we need toomake sure we call back tooyour application in a way that hello Android operating system can verify.</span></span> <span data-ttu-id="dc78c-223">hello Android operativsystem använder hello-hash för certifikatet hello i hello Google Play store.</span><span class="sxs-lookup"><span data-stu-id="dc78c-223">hello Android operating system uses hello hash of hello certificate in hello Google Play store.</span></span> <span data-ttu-id="dc78c-224">Detta kan vara falsk en otillåtna program.</span><span class="sxs-lookup"><span data-stu-id="dc78c-224">This cannot be spoofed by a rogue application.</span></span> <span data-ttu-id="dc78c-225">Därför kan utnyttja vi detta tillsammans med hello URI för våra broker programmet tooensure hello token returneras toohello rätt program.</span><span class="sxs-lookup"><span data-stu-id="dc78c-225">Therefore, we leverage this along with hello URI of our broker application tooensure that hello tokens are returned toohello correct application.</span></span> <span data-ttu-id="dc78c-226">Vi behöver du tooestablish denna unika omdirigerings-URI både i ditt program och anges som en omdirigerings-URI i vår developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="dc78c-226">We require you tooestablish this unique redirect URI both in your application and set as a Redirect URI in our developer portal.</span></span>

<span data-ttu-id="dc78c-227">Omdirigerings-URI måste vara i hello rätt form av:</span><span class="sxs-lookup"><span data-stu-id="dc78c-227">Your redirect URI must be in hello proper form of:</span></span>

`msauth://packagename/Base64UrlencodedSignature`

<span data-ttu-id="dc78c-228">ex: *msauth://com.example.userapp/IcB5PxIyvbLkbFVtBI%2FitkW%2Fejk%3D*</span><span class="sxs-lookup"><span data-stu-id="dc78c-228">ex: *msauth://com.example.userapp/IcB5PxIyvbLkbFVtBI%2FitkW%2Fejk%3D*</span></span>

<span data-ttu-id="dc78c-229">Den här omdirigerings-URI måste toobe som angetts i registreringen app med hello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="dc78c-229">This Redirect URI needs toobe specified in your app registration using hello [Azure portal](https://portal.azure.com/).</span></span> <span data-ttu-id="dc78c-230">Mer information om registrering av Azure AD app finns [integrera med Azure Active Directory](active-directory-how-to-integrate.md).</span><span class="sxs-lookup"><span data-stu-id="dc78c-230">For more information on Azure AD app registration, see [Integrating with Azure Active Directory](active-directory-how-to-integrate.md).</span></span>

#### <a name="step-3-set-up-hello-correct-permissions-in-your-application"></a><span data-ttu-id="dc78c-231">Steg 3: Ställ in hello behörigheter i ditt program</span><span class="sxs-lookup"><span data-stu-id="dc78c-231">Step 3: Set up hello correct permissions in your application</span></span>
<span data-ttu-id="dc78c-232">Vårt broker program i Android använder hello Accounts Manager funktion hello i Android OS toomanage autentiseringsuppgifter i program.</span><span class="sxs-lookup"><span data-stu-id="dc78c-232">Our broker application in Android uses hello Accounts Manager feature of hello Android OS toomanage credentials across applications.</span></span> <span data-ttu-id="dc78c-233">I ordning toouse hello broker i Android måste app-manifest ha behörigheter toouse AccountManager konton.</span><span class="sxs-lookup"><span data-stu-id="dc78c-233">In order toouse hello broker in Android your app manifest must have permissions toouse AccountManager accounts.</span></span> <span data-ttu-id="dc78c-234">Det beskrivs i detalj i hello [Google-dokumentationen för hanteraren för kontosäkerhet här](http://developer.android.com/reference/android/accounts/AccountManager.html)</span><span class="sxs-lookup"><span data-stu-id="dc78c-234">This is discussed in detail in hello [Google documentation for Account Manager here](http://developer.android.com/reference/android/accounts/AccountManager.html)</span></span>

<span data-ttu-id="dc78c-235">I synnerhet är dessa behörigheter:</span><span class="sxs-lookup"><span data-stu-id="dc78c-235">In particular, these permissions are:</span></span>

```
GET_ACCOUNTS
USE_CREDENTIALS
MANAGE_ACCOUNTS
```

### <a name="youve-configured-sso"></a><span data-ttu-id="dc78c-236">Du har konfigurerat SSO!</span><span class="sxs-lookup"><span data-stu-id="dc78c-236">You've configured SSO!</span></span>
<span data-ttu-id="dc78c-237">Nu hello Microsoft Identity SDK automatiskt både dela autentiseringsuppgifter i dina program och anropa hello broker om den finns på sin enhet.</span><span class="sxs-lookup"><span data-stu-id="dc78c-237">Now hello Microsoft Identity SDK will automatically both share credentials across your applications and invoke hello broker if it's present on their device.</span></span>

