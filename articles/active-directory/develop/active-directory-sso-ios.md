---
title: "Aktivera enkel inloggning mellan appar på iOS använder ADAL | Microsoft Docs"
description: "Hur du använder funktionerna i ADAL SDK för att aktivera enkel inloggning i ditt program. "
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
ms.openlocfilehash: 73b8ed7e6a153a0790f7eae9bd51bb2e554ae72e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-enable-cross-app-sso-on-ios-using-adal"></a><span data-ttu-id="7233f-103">Aktivera enkel inloggning mellan appar på iOS använder ADAL</span><span class="sxs-lookup"><span data-stu-id="7233f-103">How to enable cross-app SSO on iOS using ADAL</span></span>
<span data-ttu-id="7233f-104">Tillhandahåller enkel inloggning (SSO) så att användarna behöver bara ange sina autentiseringsuppgifter en gång och dessa autentiseringsuppgifter automatiskt fungerar över förväntade program nu av kunder.</span><span class="sxs-lookup"><span data-stu-id="7233f-104">Providing Single Sign-On (SSO) so that users only need to enter their credentials once and have those credentials automatically work across applications is now expected by customers.</span></span> <span data-ttu-id="7233f-105">Svårt att ange sina användarnamn och lösenord på en liten skärm, ofta gånger kombineras med ytterligare en faktor (2FA) som ett telefonsamtal eller en textläge kod leder till att snabbt klagomål om en användare har att göra det mer än en gång till produkten.</span><span class="sxs-lookup"><span data-stu-id="7233f-105">The difficulty in entering their username and password on a small screen, often times combined with an additional factor (2FA) like a phone call or a texted code, results in quick dissatisfaction if a user has to do this more than one time for your product.</span></span>

<span data-ttu-id="7233f-106">Dessutom, om du använder en identity-plattform som andra program kan använda, till exempel Microsoft Accounts eller ett arbetskonto från Office365 kunder förväntar sig att de autentiseringsuppgifter som ska vara tillgängliga för användning i alla program oavsett leverantören.</span><span class="sxs-lookup"><span data-stu-id="7233f-106">In addition, if you apply an identity platform that other applications may use such as Microsoft Accounts or a work account from Office365, customers expect that those credentials to be available to use across all their applications no matter the vendor.</span></span>

<span data-ttu-id="7233f-107">Microsoft Identity-plattformen, tillsammans med vår Microsoft Identity-SDK: er, gör den här tunga arbetet åt dig och ger dig möjlighet att glädje dina kunder med enkel inloggning för antingen inom en egen uppsättning program eller, som i våra broker kapaciteten och autentiseraren program över hela enheten.</span><span class="sxs-lookup"><span data-stu-id="7233f-107">The Microsoft Identity platform, along with our Microsoft Identity SDKs, does all this hard work for you and gives you the ability to delight your customers with SSO either within your own suite of applications or, as with our broker capability and Authenticator applications, across the entire device.</span></span>

<span data-ttu-id="7233f-108">Den här genomgången kommer information om hur du konfigurerar våra SDK i ditt program att tillhandahålla förmånen till dina kunder.</span><span class="sxs-lookup"><span data-stu-id="7233f-108">This walkthrough will tell you how to configure our SDK within your application to provide this benefit to your customers.</span></span>

<span data-ttu-id="7233f-109">Den här genomgången gäller för:</span><span class="sxs-lookup"><span data-stu-id="7233f-109">This walkthrough applies to:</span></span>

* <span data-ttu-id="7233f-110">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="7233f-110">Azure Active Directory</span></span>
* <span data-ttu-id="7233f-111">Azure Active Directory B2C</span><span class="sxs-lookup"><span data-stu-id="7233f-111">Azure Active Directory B2C</span></span>
* <span data-ttu-id="7233f-112">Azure Active Directory B2B</span><span class="sxs-lookup"><span data-stu-id="7233f-112">Azure Active Directory B2B</span></span>
* <span data-ttu-id="7233f-113">Villkorsstyrd åtkomst med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="7233f-113">Azure Active Directory Conditional Access</span></span>

<span data-ttu-id="7233f-114">Föregående dokumentet förutsätter att du vet hur du [etablera program i den äldra portalen för Azure Active Directory](active-directory-how-to-integrate.md) och integrerade programmet med den [Microsoft Identity iOS SDK](https://github.com/AzureAD/azure-activedirectory-library-for-objc).</span><span class="sxs-lookup"><span data-stu-id="7233f-114">The document preceding assumes you know how to [provision applications in the legacy portal for Azure Active Directory](active-directory-how-to-integrate.md) and integrated your application with the [Microsoft Identity iOS SDK](https://github.com/AzureAD/azure-activedirectory-library-for-objc).</span></span>

## <a name="sso-concepts-in-the-microsoft-identity-platform"></a><span data-ttu-id="7233f-115">SSO begrepp i Microsoft Identity-plattformen</span><span class="sxs-lookup"><span data-stu-id="7233f-115">SSO Concepts in the Microsoft Identity Platform</span></span>
### <a name="microsoft-identity-brokers"></a><span data-ttu-id="7233f-116">Microsoft Identity mäklare</span><span class="sxs-lookup"><span data-stu-id="7233f-116">Microsoft Identity Brokers</span></span>
<span data-ttu-id="7233f-117">Microsoft tillhandahåller program för varje mobil plattform som gör att för bryggning av autentiseringsuppgifter i program från olika leverantörer och gör för särskilda funktioner som kräver en enda säker plats varifrån att verifiera autentiseringsuppgifterna.</span><span class="sxs-lookup"><span data-stu-id="7233f-117">Microsoft provides applications for every mobile platform that allow for the bridging of credentials across applications from different vendors and allows for special enhanced features that require a single secure place from where to validate credentials.</span></span> <span data-ttu-id="7233f-118">Vi kallar dem **mäklare**.</span><span class="sxs-lookup"><span data-stu-id="7233f-118">We call these **brokers**.</span></span> <span data-ttu-id="7233f-119">På iOS och Android sker dessa mäklare via nedladdningsbara program att kunder installeras fristående eller pushas till enheten av ett företag som hanterar vissa eller alla av enheten för sina anställda.</span><span class="sxs-lookup"><span data-stu-id="7233f-119">On iOS and Android these brokers are provided through downloadable applications that customers either install independently or can be pushed to the device by a company who manages some or all of the device for their employee.</span></span> <span data-ttu-id="7233f-120">Dessa mäklare stöder Hantera säkerhet för vissa program eller hela enheten baserat på vad IT-administratörer som önskar.</span><span class="sxs-lookup"><span data-stu-id="7233f-120">These brokers support managing security just for some applications or the entire device based on what IT Administrators desire.</span></span> <span data-ttu-id="7233f-121">Den här funktionen tillhandahålls av en inbyggd i operativsystemet, kända tekniskt Webbautentiseringskoordinatorn väljare av användarkonto i Windows.</span><span class="sxs-lookup"><span data-stu-id="7233f-121">In Windows, this functionality is provided by an account chooser built in to the operating system, known technically as the Web Authentication Broker.</span></span>

<span data-ttu-id="7233f-122">Mer information om hur använder vi dessa mäklare och hur dina kunder kan se dem i sina inloggningen flödet för Microsoft Identity-plattformen finns.</span><span class="sxs-lookup"><span data-stu-id="7233f-122">For more information on how we use these brokers and how your customers might see them in their login flow for the Microsoft Identity platform read on.</span></span>

### <a name="patterns-for-logging-in-on-mobile-devices"></a><span data-ttu-id="7233f-123">Mönster för att logga in på mobila enheter</span><span class="sxs-lookup"><span data-stu-id="7233f-123">Patterns for logging in on mobile devices</span></span>
<span data-ttu-id="7233f-124">Åtkomst till autentiseringsuppgifter på enheter följer två grundläggande mönster för Microsoft Identity-plattform:</span><span class="sxs-lookup"><span data-stu-id="7233f-124">Access to credentials on devices follow two basic patterns for the Microsoft Identity platform:</span></span>

* <span data-ttu-id="7233f-125">Icke-förhandlad assisterad inloggningar</span><span class="sxs-lookup"><span data-stu-id="7233f-125">Non-broker assisted logins</span></span>
* <span data-ttu-id="7233f-126">Broker assisterad inloggningar</span><span class="sxs-lookup"><span data-stu-id="7233f-126">Broker assisted logins</span></span>

#### <a name="non-broker-assisted-logins"></a><span data-ttu-id="7233f-127">Icke-förhandlad assisterad inloggningar</span><span class="sxs-lookup"><span data-stu-id="7233f-127">Non-broker assisted logins</span></span>
<span data-ttu-id="7233f-128">Icke-förhandlad assisterad inloggningar är inloggning upplevelser som inträffa infogade med programmet och använder lokal lagring på enheten för programmet.</span><span class="sxs-lookup"><span data-stu-id="7233f-128">Non-broker assisted logins are login experiences that happen inline with the application and use the local storage on the device for that application.</span></span> <span data-ttu-id="7233f-129">Lagringen kan delas mellan program men autentiseringsuppgifterna är tätt kopplade till appen eller paket med hjälp av denna autentiseringsuppgift.</span><span class="sxs-lookup"><span data-stu-id="7233f-129">This storage may be shared across applications but the credentials are tightly bound to the app or suite of apps using that credential.</span></span> <span data-ttu-id="7233f-130">Du har förmodligen fått det i många mobila program när du anger ett användarnamn och lösenord själva programmet.</span><span class="sxs-lookup"><span data-stu-id="7233f-130">You've most likely experienced this in many mobile applications when you enter a username and password within the application itself.</span></span>

<span data-ttu-id="7233f-131">Dessa inloggningar har följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="7233f-131">These logins have the following benefits:</span></span>

* <span data-ttu-id="7233f-132">Användarupplevelsen finns helt i programmet.</span><span class="sxs-lookup"><span data-stu-id="7233f-132">User experience exists entirely within the application.</span></span>
* <span data-ttu-id="7233f-133">Autentiseringsuppgifter kan delas mellan program som har signerats med samma certifikat, tillhandahåller en enkel inloggning till en uppsättning program.</span><span class="sxs-lookup"><span data-stu-id="7233f-133">Credentials can be shared across applications that are signed by the same certificate, providing a single sign-on experience to your suite of applications.</span></span>
* <span data-ttu-id="7233f-134">Kontrollen runt upplevelse av loggning i har angetts för programmet före och efter inloggning.</span><span class="sxs-lookup"><span data-stu-id="7233f-134">Control around the experience of logging in is provided to the application before and after sign-in.</span></span>

<span data-ttu-id="7233f-135">Dessa inloggningar har följande nackdelar:</span><span class="sxs-lookup"><span data-stu-id="7233f-135">These logins have the following drawbacks:</span></span>

* <span data-ttu-id="7233f-136">Enkel inloggning på kan inte användarupplevelse över alla appar som använder Microsoft-Identity enbart över de Microsoft-Identities som programmet har konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="7233f-136">User cannot experience single-sign on across all apps that use a Microsoft Identity, only across those Microsoft Identities that your application has configured.</span></span>
* <span data-ttu-id="7233f-137">Programmet kan inte användas med mer avancerade funktioner för företag, till exempel villkorlig åtkomst eller Använd InTune-produkter.</span><span class="sxs-lookup"><span data-stu-id="7233f-137">Your application cannot be used with more advanced business features such as Conditional Access or use the InTune suite of products.</span></span>
* <span data-ttu-id="7233f-138">Programmet stöder inte certifikatbaserad autentisering för företagsanvändare.</span><span class="sxs-lookup"><span data-stu-id="7233f-138">Your application can't support certificate-based authentication for business users.</span></span>

<span data-ttu-id="7233f-139">Här är en representation av hur Microsoft Identity SDK fungerar med delad lagring av ditt program för att aktivera enkel inloggning:</span><span class="sxs-lookup"><span data-stu-id="7233f-139">Here is a representation of how the Microsoft Identity SDKs work with the shared storage of your applications to enable SSO:</span></span>

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

#### <a name="broker-assisted-logins"></a><span data-ttu-id="7233f-140">Broker assisterad inloggningar</span><span class="sxs-lookup"><span data-stu-id="7233f-140">Broker assisted logins</span></span>
<span data-ttu-id="7233f-141">Service Broker-stödd inloggningar är inloggning upplevelser som inträffar i Service broker-programmet och använder lagring och säkerhet för Service broker för att dela autentiseringsuppgifter för alla program på enheten som gäller Microsoft Identity-plattformen.</span><span class="sxs-lookup"><span data-stu-id="7233f-141">Broker-assisted logins are login experiences that occur within the broker application and use the storage and security of the broker to share credentials across all applications on the device that apply the Microsoft Identity platform.</span></span> <span data-ttu-id="7233f-142">Detta innebär att dina program som förlitar sig på Service broker för inloggning av användare.</span><span class="sxs-lookup"><span data-stu-id="7233f-142">This means that your applications rely on the broker to sign users in.</span></span> <span data-ttu-id="7233f-143">På iOS och Android sker dessa mäklare via nedladdningsbara program att kunder installeras fristående eller pushas till enheten av ett företag som hanterar enheten för sina användare.</span><span class="sxs-lookup"><span data-stu-id="7233f-143">On iOS and Android these brokers are provided through downloadable applications that customers either install independently or can be pushed to the device by a company who manages the device for their user.</span></span> <span data-ttu-id="7233f-144">Ett exempel på den här typen av program är Microsoft Authenticator-appen på iOS.</span><span class="sxs-lookup"><span data-stu-id="7233f-144">An example of this type of application is the Microsoft Authenticator application on iOS.</span></span> <span data-ttu-id="7233f-145">Den här funktionen tillhandahålls av en inbyggd i operativsystemet, kända tekniskt Webbautentiseringskoordinatorn väljare av användarkonto i Windows.</span><span class="sxs-lookup"><span data-stu-id="7233f-145">In Windows this functionality is provided by an account chooser built in to the operating system, known technically as the Web Authentication Broker.</span></span>
<span data-ttu-id="7233f-146">Upplevelsen varierar efter plattform och ibland kan vara störande för användarna om de inte hanteras på rätt sätt.</span><span class="sxs-lookup"><span data-stu-id="7233f-146">The experience varies by platform and can sometimes be disruptive to users if not managed correctly.</span></span> <span data-ttu-id="7233f-147">Du är mest förmodligen är bekant med det här mönstret om du har installerat Facebook-program och använder Facebook ansluta från ett annat program.</span><span class="sxs-lookup"><span data-stu-id="7233f-147">You're probably most familiar with this pattern if you have the Facebook application installed and use Facebook Connect from another application.</span></span> <span data-ttu-id="7233f-148">Microsoft Identity-plattformen använder samma mönster.</span><span class="sxs-lookup"><span data-stu-id="7233f-148">The Microsoft Identity platform uses the same pattern.</span></span>

<span data-ttu-id="7233f-149">För iOS som leder till att en ”övergång” kommer animering där programmet skickas till bakgrunden medan Microsoft Authenticator-program i förgrunden för användaren att välja vilket konto som de vill logga in med.</span><span class="sxs-lookup"><span data-stu-id="7233f-149">For iOS this leads to a "transition" animation where your application is sent to the background while the Microsoft Authenticator applications comes to the foreground for the user to select which account they would like to sign in with.</span></span>  

<span data-ttu-id="7233f-150">För Android och Windows visas väljare av användarkonto ovanpå ditt program som är mindre störande för användaren.</span><span class="sxs-lookup"><span data-stu-id="7233f-150">For Android and Windows the account chooser is displayed on top of your application which is less disruptive to the user.</span></span>

#### <a name="how-the-broker-gets-invoked"></a><span data-ttu-id="7233f-151">Hur Service broker hämtar anropas</span><span class="sxs-lookup"><span data-stu-id="7233f-151">How the broker gets invoked</span></span>
<span data-ttu-id="7233f-152">Om en kompatibel broker är installerad på enheten som programmet Microsoft Authenticator SDK: er för Microsoft Identity automatiskt att göra arbetet med att aktivera Service broker för dig när en användare anger de vill logga in med ett konto från Microsoft Identity-plattformen.</span><span class="sxs-lookup"><span data-stu-id="7233f-152">If a compatible broker is installed on the device, like the Microsoft Authenticator application, the Microsoft Identity SDKs will automatically do the work of invoking the broker for you when a user indicates they wish to log in using any account from the Microsoft Identity platform.</span></span> <span data-ttu-id="7233f-153">Det här kontot kan vara ett personligt Microsoft-Account, ett arbets eller skolkonto, eller ett konto som du anger och värden i Azure med hjälp av vår B2C och B2B-produkter.</span><span class="sxs-lookup"><span data-stu-id="7233f-153">This account could be a personal Microsoft Account, a work or school account, or an account that you provide and host in Azure using our B2C and B2B products.</span></span> 

 #### <a name="how-we-ensure-the-application-is-valid"></a><span data-ttu-id="7233f-154">Hur vi Kontrollera att programmet är giltigt</span><span class="sxs-lookup"><span data-stu-id="7233f-154">How we ensure the application is valid</span></span>
 
 <span data-ttu-id="7233f-155">Behovet av att kontrollera identiteten för ett program anrop Service broker är avgörande för säkerheten som vi tillhandahåller i broker stödd inloggningar.</span><span class="sxs-lookup"><span data-stu-id="7233f-155">The need to ensure the identity of an application call the broker is crucial to the security we provide in broker assisted logins.</span></span> <span data-ttu-id="7233f-156">Tillämpar unika identifierare är endast giltiga för ett visst program så att skadliga program kan ”förfalska” legitima program-ID och ta emot token som är avsedd för programmets legitima varken iOS eller Android.</span><span class="sxs-lookup"><span data-stu-id="7233f-156">Neither iOS nor Android enforces unique identifiers that are valid only for a given application, so malicious applications may "spoof" a legitimate application's identifier and receive the tokens meant for the legitimate application.</span></span> <span data-ttu-id="7233f-157">För att säkerställa att vi alltid kommunicerar med rätt programmet vid körning, ber vi utvecklare tillhandahålla anpassade redirectURI när de registrerar sina program med Microsoft.</span><span class="sxs-lookup"><span data-stu-id="7233f-157">To ensure we are always communicating with the right application at runtime, we ask the developer to provide a custom redirectURI when registering their application with Microsoft.</span></span> <span data-ttu-id="7233f-158">**Hur utvecklare ska använda för att skapa den här omdirigerings-URI diskuteras i detalj nedan.**</span><span class="sxs-lookup"><span data-stu-id="7233f-158">**How developers should craft this redirect URI is discussed in detail below.**</span></span> <span data-ttu-id="7233f-159">Den här anpassade redirectURI innehåller paket-ID för programmet och säkerställs för att vara unikt för programmet med Apple App Store.</span><span class="sxs-lookup"><span data-stu-id="7233f-159">This custom redirectURI contains the Bundle ID of the application and is ensured to be unique to the application by the Apple App Store.</span></span> <span data-ttu-id="7233f-160">När ett program anropar Service broker, frågar Service broker iOS-operativsystem för att tillhandahålla paket-ID som kallas Service broker.</span><span class="sxs-lookup"><span data-stu-id="7233f-160">When an application calls the broker, the broker asks the iOS operating system to provide it with the Bundle ID that called the broker.</span></span> <span data-ttu-id="7233f-161">Service broker ger detta paket-ID till Microsoft i anropet till vår identitetssystem.</span><span class="sxs-lookup"><span data-stu-id="7233f-161">The broker provides this Bundle ID to Microsoft in the call to our identity system.</span></span> <span data-ttu-id="7233f-162">Om paket-ID för programmet inte matchar det paket-ID som angetts för oss av utvecklaren under registreringen, kommer vi att neka åtkomst till token för resursen programmet begär.</span><span class="sxs-lookup"><span data-stu-id="7233f-162">If the Bundle ID of the application does not match the Bundle ID provided to us by the developer during registration, we will deny access to the tokens for the resource the application is requesting.</span></span> <span data-ttu-id="7233f-163">Försäkra dig om att det program som registrerats av utvecklaren tar emot tokens.</span><span class="sxs-lookup"><span data-stu-id="7233f-163">This check ensures that only the application registered by the developer receives tokens.</span></span>

<span data-ttu-id="7233f-164">**Utvecklaren kan välja mellan om Microsoft Identity SDK anropar Service broker eller använder icke-förhandlad assisterad flödet.**</span><span class="sxs-lookup"><span data-stu-id="7233f-164">**The developer has the choice of if the Microsoft Identity SDK calls the broker or uses the non-broker assisted flow.**</span></span> <span data-ttu-id="7233f-165">Men om utvecklaren väljer att inte använda flödet broker-stödd de förlora fördelen med att använda enkel inloggning autentiseringsuppgifter att användaren har redan lagt på enheten och förhindrar att deras program som används med funktioner för företag Microsoft ger sina kunder som villkorlig åtkomst, Intune-hanteringsfunktioner och certifikatbaserad autentisering.</span><span class="sxs-lookup"><span data-stu-id="7233f-165">However if the developer chooses not to use the broker-assisted flow they lose the benefit of using SSO credentials that the user may have already added on the device and prevents their application from being used with business features Microsoft provides its customers such as Conditional Access, Intune Management capabilities, and certificate-based authentication.</span></span>

<span data-ttu-id="7233f-166">Dessa inloggningar har följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="7233f-166">These logins have the following benefits:</span></span>

* <span data-ttu-id="7233f-167">Användaren upplever SSO över sina program oavsett leverantören.</span><span class="sxs-lookup"><span data-stu-id="7233f-167">User experiences SSO across all their applications no matter the vendor.</span></span>
* <span data-ttu-id="7233f-168">Programmet kan använda mer avancerade funktioner för företag som villkorlig åtkomst eller Använd InTune-produkter.</span><span class="sxs-lookup"><span data-stu-id="7233f-168">Your application can use more advanced business features such as Conditional Access or use the InTune suite of products.</span></span>
* <span data-ttu-id="7233f-169">Programmet stöder certifikatbaserad autentisering för användare i verksamheten.</span><span class="sxs-lookup"><span data-stu-id="7233f-169">Your application can support certificate-based authentication for business users.</span></span>
* <span data-ttu-id="7233f-170">Mycket mer säker inloggning som identitet för programmet och användaren har verifierats av broker programmet med ytterligare säkerhetsalgoritmer och kryptering.</span><span class="sxs-lookup"><span data-stu-id="7233f-170">Much more secure sign-in experience as the identity of the application and the user are verified by the broker application with additional security algorithms and encryption.</span></span>

<span data-ttu-id="7233f-171">Dessa inloggningar har följande nackdelar:</span><span class="sxs-lookup"><span data-stu-id="7233f-171">These logins have the following drawbacks:</span></span>

* <span data-ttu-id="7233f-172">I iOS övergick användaren utanför din programupplevelse medan autentiseringsuppgifter är valt.</span><span class="sxs-lookup"><span data-stu-id="7233f-172">In iOS the user is transitioned out of your application's experience while credentials are chosen.</span></span>
* <span data-ttu-id="7233f-173">Förlust av möjligheten att hantera inloggningen upplevelsen för kunderna i ditt program.</span><span class="sxs-lookup"><span data-stu-id="7233f-173">Loss of the ability to manage the login experience for your customers within your application.</span></span>

<span data-ttu-id="7233f-174">Här är en representation av hur Microsoft Identity SDK fungerar med broker-program för att aktivera enkel inloggning:</span><span class="sxs-lookup"><span data-stu-id="7233f-174">Here is a representation of how the Microsoft Identity SDKs work with the broker applications to enable SSO:</span></span>

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

<span data-ttu-id="7233f-175">Tillsammans med den här bakgrundsinformation som du ska kunna bättre förstå och implementera enkel inloggning i ditt program med hjälp av Microsoft Identity plattform och SDK: er.</span><span class="sxs-lookup"><span data-stu-id="7233f-175">Armed with this background information you should be able to better understand and implement SSO within your application using the Microsoft Identity platform and SDKs.</span></span>

## <a name="enabling-cross-app-sso-using-adal"></a><span data-ttu-id="7233f-176">Aktivera enkel inloggning mellan appar med hjälp av ADAL</span><span class="sxs-lookup"><span data-stu-id="7233f-176">Enabling cross-app SSO using ADAL</span></span>
<span data-ttu-id="7233f-177">Här vi använder ADAL iOS SDK till:</span><span class="sxs-lookup"><span data-stu-id="7233f-177">Here we use the ADAL iOS SDK to:</span></span>

* <span data-ttu-id="7233f-178">Aktivera icke-förhandlad stödd enkel inloggning för en uppsättning appar</span><span class="sxs-lookup"><span data-stu-id="7233f-178">Turn on non-broker assisted SSO for your suite of apps</span></span>
* <span data-ttu-id="7233f-179">Aktivera stöd för Service broker-stödd enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7233f-179">Turn on support for broker-assisted SSO</span></span>

### <a name="turning-on-sso-for-non-broker-assisted-sso"></a><span data-ttu-id="7233f-180">Aktivera enkel inloggning för icke-förhandlad stödd SSO</span><span class="sxs-lookup"><span data-stu-id="7233f-180">Turning on SSO for non-broker assisted SSO</span></span>
<span data-ttu-id="7233f-181">För icke-förhandlad assisterad SSO över program hantera SDK: er för Microsoft Identity mycket komplex enkel inloggning för dig.</span><span class="sxs-lookup"><span data-stu-id="7233f-181">For non-broker assisted SSO across applications the Microsoft Identity SDKs manage much of the complexity of SSO for you.</span></span> <span data-ttu-id="7233f-182">Detta inkluderar att hitta rätt användaren i cachen och underhålla en lista över inloggade användare att fråga.</span><span class="sxs-lookup"><span data-stu-id="7233f-182">This includes finding the right user in the cache and maintaining a list of logged in users for you to query.</span></span>

<span data-ttu-id="7233f-183">Att aktivera enkel inloggning för program som du äger måste du göra följande:</span><span class="sxs-lookup"><span data-stu-id="7233f-183">To enable SSO across applications you own you need to do the following:</span></span>

1. <span data-ttu-id="7233f-184">Se till att alla program användare samma klient-ID eller program-ID.</span><span class="sxs-lookup"><span data-stu-id="7233f-184">Ensure all your applications user the same Client ID or Application ID.</span></span>
2. <span data-ttu-id="7233f-185">Kontrollera att alla dina program delar samma signeringscertifikat från Apple så att du kan dela nyckelringar</span><span class="sxs-lookup"><span data-stu-id="7233f-185">Ensure that all of your applications share the same signing certificate from Apple so that you can share keychains</span></span>
3. <span data-ttu-id="7233f-186">Begära berättigandet samma nyckelringar för var och en av dina program.</span><span class="sxs-lookup"><span data-stu-id="7233f-186">Request the same keychain entitlement for each of your applications.</span></span>
4. <span data-ttu-id="7233f-187">Tala om SDK: er för Microsoft Identity delade nyckelringen du vill att vi ska använda.</span><span class="sxs-lookup"><span data-stu-id="7233f-187">Tell the Microsoft Identity SDKs about the shared keychain you want us to use.</span></span>

#### <a name="using-the-same-client-id--application-id-for-all-the-applications-in-your-suite-of-apps"></a><span data-ttu-id="7233f-188">Med hjälp av samma klient-ID eller program-ID för alla program i din uppsättning appar</span><span class="sxs-lookup"><span data-stu-id="7233f-188">Using the same Client ID / Application ID for all the applications in your suite of apps</span></span>
<span data-ttu-id="7233f-189">För Microsoft Identity-plattformen vet att den har tillåtelse för att dela token i dina program, måste var och en av dina program du dela samma klient-ID eller program-ID.</span><span class="sxs-lookup"><span data-stu-id="7233f-189">In order for the Microsoft Identity platform to know that it's allowed to share tokens across your applications, each of your applications will need to share the same Client ID or Application ID.</span></span> <span data-ttu-id="7233f-190">Det här är den unika identifieraren som angavs för dig när du har registrerat din första program i portalen.</span><span class="sxs-lookup"><span data-stu-id="7233f-190">This is the unique identifier that was provided to you when you registered your first application in the portal.</span></span>

<span data-ttu-id="7233f-191">Du kanske undrar hur du identifierar olika appar till tjänsten Microsoft Identity om den använder den samma program-ID.</span><span class="sxs-lookup"><span data-stu-id="7233f-191">You may be wondering how you will identify different apps to the Microsoft Identity service if it uses the same Application ID.</span></span> <span data-ttu-id="7233f-192">Svaret är med i **omdirigerings-URI: er**.</span><span class="sxs-lookup"><span data-stu-id="7233f-192">The answer is with the **Redirect URIs**.</span></span> <span data-ttu-id="7233f-193">Varje program kan ha flera omdirigerings-URI: er registrerade i onboarding-portalen.</span><span class="sxs-lookup"><span data-stu-id="7233f-193">Each application can have multiple Redirect URIs registered in the onboarding portal.</span></span> <span data-ttu-id="7233f-194">Varje app i din suite har en annan omdirigerings-URI.</span><span class="sxs-lookup"><span data-stu-id="7233f-194">Each app in your suite will have a different redirect URI.</span></span> <span data-ttu-id="7233f-195">Ett exempel på hur detta ser ut understiger:</span><span class="sxs-lookup"><span data-stu-id="7233f-195">An example of how this looks is below:</span></span>

<span data-ttu-id="7233f-196">App1 omdirigerings-URI:`x-msauth-mytestiosapp://com.myapp.mytestapp`</span><span class="sxs-lookup"><span data-stu-id="7233f-196">App1 Redirect URI: `x-msauth-mytestiosapp://com.myapp.mytestapp`</span></span>

<span data-ttu-id="7233f-197">App2 omdirigerings-URI:`x-msauth-mytestiosapp://com.myapp.mytestapp2`</span><span class="sxs-lookup"><span data-stu-id="7233f-197">App2 Redirect URI: `x-msauth-mytestiosapp://com.myapp.mytestapp2`</span></span>

<span data-ttu-id="7233f-198">App3 omdirigerings-URI:`x-msauth-mytestiosapp://com.myapp.mytestapp3`</span><span class="sxs-lookup"><span data-stu-id="7233f-198">App3 Redirect URI: `x-msauth-mytestiosapp://com.myapp.mytestapp3`</span></span>

<span data-ttu-id="7233f-199">....</span><span class="sxs-lookup"><span data-stu-id="7233f-199">....</span></span>

<span data-ttu-id="7233f-200">Dessa kapslas under samma klient-ID / program-ID och slås upp utifrån omdirigerings-URI som du kommer tillbaka till oss i SDK-konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="7233f-200">These are nested under the same client ID / application ID and looked up based on the redirect URI you return to us in your SDK configuration.</span></span>

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


<span data-ttu-id="7233f-201">*Observera att formatet för dessa omdirigerings-URI: er beskrivs nedan. Du kan använda alla omdirigerings-URI om du vill stödja broker, då de måste se ut ungefär som anges ovan*</span><span class="sxs-lookup"><span data-stu-id="7233f-201">*Note that the format of these Redirect URIs are explained below. You may use any Redirect URI unless you wish to support the broker, in which case they must look something like the above*</span></span>

#### <a name="create-keychain-sharing-between-applications"></a><span data-ttu-id="7233f-202">Skapa mellan program för delning av nyckelringar</span><span class="sxs-lookup"><span data-stu-id="7233f-202">Create keychain sharing between applications</span></span>
<span data-ttu-id="7233f-203">Aktivering av nyckelringsdelning är utanför omfattningen för det här dokumentet och omfattas av Apple i dokumentet [lägga till funktioner](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html).</span><span class="sxs-lookup"><span data-stu-id="7233f-203">Enabling keychain sharing is beyond the scope of this document and covered by Apple in their document [Adding Capabilities](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html).</span></span> <span data-ttu-id="7233f-204">Vad är viktigt är att du bestämmer vad du vill att nyckelringen anropas och Lägg till den här funktionen i alla program.</span><span class="sxs-lookup"><span data-stu-id="7233f-204">What is important is that you decide what you want your keychain to be called and add that capability across all your applications.</span></span>

<span data-ttu-id="7233f-205">När du har rättigheter som är konfigurerat på rätt sätt bör du se en fil i projektkatalogen rätt `entitlements.plist` som innehåller något som liknar följande:</span><span class="sxs-lookup"><span data-stu-id="7233f-205">When you do have entitlements set up correctly you should see a file in your project directory entitled `entitlements.plist` that contains something that looks like the following:</span></span>

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

<span data-ttu-id="7233f-206">När du har berättigandet nyckelringar aktiverat i var och en av dina program och du är redo att använda enkel inloggning kan berätta för Microsoft Identity SDK om nyckelringen med hjälp av följande inställning i din `ADAuthenticationSettings` med följande inställning:</span><span class="sxs-lookup"><span data-stu-id="7233f-206">Once you have the keychain entitlement enabled in each of your applications, and you are ready to use SSO, tell the Microsoft Identity SDK about your keychain by using the following setting in your `ADAuthenticationSettings` with the following setting:</span></span>

```
defaultKeychainSharingGroup=@"com.myapp.mycache";
```

> [!WARNING]
> <span data-ttu-id="7233f-207">När du delar en nyckelring över dina program kan alla program ta bort användare eller värre ta bort alla token för ditt program.</span><span class="sxs-lookup"><span data-stu-id="7233f-207">When you share a keychain across your applications any application can delete users or worse delete all the tokens across your application.</span></span> <span data-ttu-id="7233f-208">Detta är särskilt katastrofal om du har program som förlitar sig på tokens för att background arbete.</span><span class="sxs-lookup"><span data-stu-id="7233f-208">This is particularly disastrous if you have applications that rely on the tokens to do background work.</span></span> <span data-ttu-id="7233f-209">Dela en nyckelring remove innebär att du måste vara mycket försiktig i alla-åtgärder via SDK: er för Microsoft Identity.</span><span class="sxs-lookup"><span data-stu-id="7233f-209">Sharing a keychain means that you must be very careful in any and all remove operations through the Microsoft Identity SDKs.</span></span>
> 
> 

<span data-ttu-id="7233f-210">Klart!</span><span class="sxs-lookup"><span data-stu-id="7233f-210">That's it!</span></span> <span data-ttu-id="7233f-211">Microsoft Identity SDK kommer nu att dela autentiseringsuppgifter i alla program.</span><span class="sxs-lookup"><span data-stu-id="7233f-211">The Microsoft Identity SDK will now share credentials across all your applications.</span></span> <span data-ttu-id="7233f-212">Lista över användare också delas mellan programinstanser.</span><span class="sxs-lookup"><span data-stu-id="7233f-212">The user list will also be shared across application instances.</span></span>

### <a name="turning-on-sso-for-broker-assisted-sso"></a><span data-ttu-id="7233f-213">Aktivera enkel inloggning för broker stödd SSO</span><span class="sxs-lookup"><span data-stu-id="7233f-213">Turning on SSO for broker assisted SSO</span></span>
<span data-ttu-id="7233f-214">Möjligheten för ett program att använda alla broker som är installerad på enheten är **inaktiverad som standard**.</span><span class="sxs-lookup"><span data-stu-id="7233f-214">The ability for an application to use any broker that is installed on the device is **turned off by default**.</span></span> <span data-ttu-id="7233f-215">För att kunna använda ditt program med Service broker måste du göra ytterligare konfigurering och Lägg till lite kod i ditt program.</span><span class="sxs-lookup"><span data-stu-id="7233f-215">In order to use your application with the broker you must do some additional configuration and add some code to your application.</span></span>

<span data-ttu-id="7233f-216">Steg att följa är:</span><span class="sxs-lookup"><span data-stu-id="7233f-216">The steps to follow are:</span></span>

1. <span data-ttu-id="7233f-217">Aktivera Service broker-läge i din programkod anrop till MS-SDK.</span><span class="sxs-lookup"><span data-stu-id="7233f-217">Enable broker mode in your application code's call to the MS SDK.</span></span>
2. <span data-ttu-id="7233f-218">Upprätta en ny omdirigerings-URI och ange som både appen och appen registreringen.</span><span class="sxs-lookup"><span data-stu-id="7233f-218">Establish a new redirect URI and provide that to both the app and your app registration.</span></span>
3. <span data-ttu-id="7233f-219">Registrera en URL-schema.</span><span class="sxs-lookup"><span data-stu-id="7233f-219">Registering a URL Scheme.</span></span>
4. <span data-ttu-id="7233f-220">Stöd för iOS9: lägga till en behörighet i filen info.plist.</span><span class="sxs-lookup"><span data-stu-id="7233f-220">iOS9 Support: Add a permission to your info.plist file.</span></span>

#### <a name="step-1-enable-broker-mode-in-your-application"></a><span data-ttu-id="7233f-221">Steg 1: Aktivera Service broker-läge i ditt program</span><span class="sxs-lookup"><span data-stu-id="7233f-221">Step 1: Enable broker mode in your application</span></span>
<span data-ttu-id="7233f-222">Möjligheten för programmet ska använda Service broker är aktiverat när du skapar ”kontexten” eller installationen av autentisering-objektet.</span><span class="sxs-lookup"><span data-stu-id="7233f-222">The ability for your application to use the broker is turned on when you create the "context" or initial setup of your Authentication object.</span></span> <span data-ttu-id="7233f-223">Du kan göra detta genom att ange vilken typ av autentiseringsuppgifter i koden:</span><span class="sxs-lookup"><span data-stu-id="7233f-223">You do this by setting your credentials type in your code:</span></span>

```
/*! See the ADCredentialsType enumeration definition for details */
@propertyADCredentialsType credentialsType;
```
<span data-ttu-id="7233f-224">Den `AD_CREDENTIALS_AUTO` inställningen tillåter att Microsoft Identity SDK att anropa till Service broker, `AD_CREDENTIALS_EMBEDDED` förhindrar Microsoft Identity SDK anropar till Service broker.</span><span class="sxs-lookup"><span data-stu-id="7233f-224">The `AD_CREDENTIALS_AUTO` setting will allow the Microsoft Identity SDK to try to call out to the broker, `AD_CREDENTIALS_EMBEDDED` will prevent the Microsoft Identity SDK from calling to the broker.</span></span>

#### <a name="step-2-registering-a-url-scheme"></a><span data-ttu-id="7233f-225">Steg 2: Registrera en URL-schema</span><span class="sxs-lookup"><span data-stu-id="7233f-225">Step 2: Registering a URL Scheme</span></span>
<span data-ttu-id="7233f-226">Microsoft Identity-plattformen använder URL: er för att anropa Service broker och sedan komma tillbaka till ditt program.</span><span class="sxs-lookup"><span data-stu-id="7233f-226">The Microsoft Identity platform uses URLs to invoke the broker and then return control back to your application.</span></span> <span data-ttu-id="7233f-227">Slutför den serveranrop behöver du ett URL-schema som registrerats för ditt program som Microsoft Identity-plattformen vet om.</span><span class="sxs-lookup"><span data-stu-id="7233f-227">To finish that round trip you need a URL scheme registered for your application that the Microsoft Identity platform will know about.</span></span> <span data-ttu-id="7233f-228">Detta kan vara utöver eventuella andra app-system du kanske redan har registrerat med ditt program.</span><span class="sxs-lookup"><span data-stu-id="7233f-228">This can be in addition to any other app schemes you may have previously registered with your application.</span></span>

> [!WARNING]
> <span data-ttu-id="7233f-229">Rekommenderar vi att URL-schemat ganska unika för att minimera risken att en annan app som använder samma URL-schemat.</span><span class="sxs-lookup"><span data-stu-id="7233f-229">We recommend making the URL scheme fairly unique to minimize the chances of another app using the same URL scheme.</span></span> <span data-ttu-id="7233f-230">Apple tillämpar inte unika URL-scheman som är registrerade i app store.</span><span class="sxs-lookup"><span data-stu-id="7233f-230">Apple does not enforce the uniqueness of URL schemes that are registered in the app store.</span></span>
> 
> 

<span data-ttu-id="7233f-231">Nedan visas ett exempel på hur den visas i projektkonfigurationen av.</span><span class="sxs-lookup"><span data-stu-id="7233f-231">Below is an example of how this appears in your project configuration.</span></span> <span data-ttu-id="7233f-232">Du kan också göra detta i XCode samt:</span><span class="sxs-lookup"><span data-stu-id="7233f-232">You may also do this in XCode as well:</span></span>

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

#### <a name="step-3-establish-a-new-redirect-uri-with-your-url-scheme"></a><span data-ttu-id="7233f-233">Steg 3: Upprätta en ny omdirigerings-URI med URL-schema</span><span class="sxs-lookup"><span data-stu-id="7233f-233">Step 3: Establish a new redirect URI with your URL Scheme</span></span>
<span data-ttu-id="7233f-234">Vi behöver kontrollera vi kallar tillbaka till ditt program på ett sätt som operativsystemet iOS kan kontrollera för att säkerställa att vi alltid returnera credential-token till rätt program.</span><span class="sxs-lookup"><span data-stu-id="7233f-234">In order to ensure that we always return the credential tokens to the correct application, we need to make sure we call back to your application in a way that the iOS operating system can verify.</span></span> <span data-ttu-id="7233f-235">IOS-operativsystemet som rapporterar till Microsoft-program för broker paket-ID för programmet anropas.</span><span class="sxs-lookup"><span data-stu-id="7233f-235">The iOS operating system reports to the Microsoft broker applications the Bundle ID of the application calling it.</span></span> <span data-ttu-id="7233f-236">Detta kan vara falsk en otillåtna program.</span><span class="sxs-lookup"><span data-stu-id="7233f-236">This cannot be spoofed by a rogue application.</span></span> <span data-ttu-id="7233f-237">Därför kan utnyttja vi detta tillsammans med URI för appen broker för att säkerställa att token som returneras till rätt program.</span><span class="sxs-lookup"><span data-stu-id="7233f-237">Therefore, we leverage this along with the URI of our broker application to ensure that the tokens are returned to the correct application.</span></span> <span data-ttu-id="7233f-238">Vi måste du upprätta detta unika omdirigerings-URI för både i ditt program och ange som en omdirigerings-URI i vår developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="7233f-238">We require you to establish this unique redirect URI both in your application and set as a Redirect URI in our developer portal.</span></span>

<span data-ttu-id="7233f-239">Omdirigerings-URI måste vara i rätt form av:</span><span class="sxs-lookup"><span data-stu-id="7233f-239">Your redirect URI must be in the proper form of:</span></span>

`<app-scheme>://<your.bundle.id>`

<span data-ttu-id="7233f-240">ex: *x-msauth-mytestiosapp://com.myapp.mytestapp*</span><span class="sxs-lookup"><span data-stu-id="7233f-240">ex: *x-msauth-mytestiosapp://com.myapp.mytestapp*</span></span>

<span data-ttu-id="7233f-241">Den här omdirigerings-URI måste anges i din app registrering med hjälp av den [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="7233f-241">This Redirect URI needs to be specified in your app registration using the [Azure portal](https://portal.azure.com/).</span></span> <span data-ttu-id="7233f-242">Mer information om registrering av Azure AD app finns [integrera med Azure Active Directory](active-directory-how-to-integrate.md).</span><span class="sxs-lookup"><span data-stu-id="7233f-242">For more information on Azure AD app registration, see [Integrating with Azure Active Directory](active-directory-how-to-integrate.md).</span></span>

##### <a name="step-3a-add-a-redirect-uri-in-your-app-and-dev-portal-to-support-certificate-based-authentication"></a><span data-ttu-id="7233f-243">Steg 3a: lägga till en omdirigerings-URI i din app och dev portal till stöd för certifikatbaserad autentisering</span><span class="sxs-lookup"><span data-stu-id="7233f-243">Step 3a: Add a redirect URI in your app and dev portal to support certificate based authentication</span></span>
<span data-ttu-id="7233f-244">Till stöd för certifikatbaserad autentisering en andra ”msauth” måste vara registrerad i ditt program och [Azure-portalen](https://portal.azure.com/) att hantera autentisering med datorcertifikat om du vill lägga till som stöder i ditt program.</span><span class="sxs-lookup"><span data-stu-id="7233f-244">To support cert based authentication a second "msauth"  needs to be registered in your application and the [Azure portal](https://portal.azure.com/) to handle certificate authentication if you wish to add that support in your application.</span></span>

`msauth://code/<broker-redirect-uri-in-url-encoded-form>`

<span data-ttu-id="7233f-245">ex: *msauth://code/x-msauth-mytestiosapp%3A%2F%2Fcom.myapp.mytestapp*</span><span class="sxs-lookup"><span data-stu-id="7233f-245">ex: *msauth://code/x-msauth-mytestiosapp%3A%2F%2Fcom.myapp.mytestapp*</span></span>

#### <a name="step-4-ios9-add-a-configuration-parameter-to-your-app"></a><span data-ttu-id="7233f-246">Steg 4: iOS9: Lägg till en konfigurationsparameter i appen</span><span class="sxs-lookup"><span data-stu-id="7233f-246">Step 4: iOS9: Add a configuration parameter to your app</span></span>
<span data-ttu-id="7233f-247">ADAL använder – canOpenURL: Kontrollera om Service broker är installerad på enheten.</span><span class="sxs-lookup"><span data-stu-id="7233f-247">ADAL uses –canOpenURL: to check if the broker is installed on the device.</span></span> <span data-ttu-id="7233f-248">I iOS låsta 9 Apple scheman ett program kan fråga efter.</span><span class="sxs-lookup"><span data-stu-id="7233f-248">In iOS 9 Apple locked down what schemes an application can query for.</span></span> <span data-ttu-id="7233f-249">Du behöver lägga till ”msauth” i avsnittet LSApplicationQueriesSchemes i din `info.plist file`.</span><span class="sxs-lookup"><span data-stu-id="7233f-249">You will need to add “msauth” to the LSApplicationQueriesSchemes section of your `info.plist file`.</span></span>

<span data-ttu-id="7233f-250"><key>LSApplicationQueriesSchemes</key></span><span class="sxs-lookup"><span data-stu-id="7233f-250"><key>LSApplicationQueriesSchemes</key></span></span>

<span data-ttu-id="7233f-251"><array><string>msauth</string>
</array></span><span class="sxs-lookup"><span data-stu-id="7233f-251"><array> <string>msauth</string>
</array></span></span>

### <a name="youve-configured-sso"></a><span data-ttu-id="7233f-252">Du har konfigurerat SSO!</span><span class="sxs-lookup"><span data-stu-id="7233f-252">You've configured SSO!</span></span>
<span data-ttu-id="7233f-253">Nu Microsoft Identity SDK automatiskt både dela autentiseringsuppgifter i dina program och anropa Service broker om den finns på sin enhet.</span><span class="sxs-lookup"><span data-stu-id="7233f-253">Now the Microsoft Identity SDK will automatically both share credentials across your applications and invoke the broker if it's present on their device.</span></span>

