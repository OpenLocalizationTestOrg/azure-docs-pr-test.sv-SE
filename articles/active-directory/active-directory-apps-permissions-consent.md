---
title: "Appar, behörigheter och godkännande i Azure Active Directory.| Microsoft Docs"
description: "Azure AD Connect integrerar dina lokala kataloger med Azure Active Directory. Detta gör att du tooprovide en gemensam identitet för Office 365 och Azure SaaS-program som är integrerade med Azure AD."
keywords: "Introduktion tooAzure AD, appar, vad är Azure AD Connect, installera active directory"
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 25897cc4-7687-49b6-b0d5-71f51302b6b1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/31/2017
ms.author: billmath
ms.reviewer: jesakowi
ms.custom: oldportal;it-pro;
ms.openlocfilehash: af0c2669199736fdb41e85876a7e3a7064e80770
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="apps-permissions-and-consent-in-azure-active-directory"></a><span data-ttu-id="bef6e-105">Appar, behörigheter och godkännande i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="bef6e-105">Apps, permissions, and consent in Azure Active Directory</span></span>
<span data-ttu-id="bef6e-106">Du kan lägga till program tooyour katalog i Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="bef6e-106">Within Azure Active Directory, you can add applications tooyour directory.</span></span>  <span data-ttu-id="bef6e-107">hello program kan variera beroende på hello typ av program.</span><span class="sxs-lookup"><span data-stu-id="bef6e-107">hello applications can vary depending on hello type of application.</span></span>  <span data-ttu-id="bef6e-108">tooview program i hello klassiska portal, väljer du en katalog och Välj program.</span><span class="sxs-lookup"><span data-stu-id="bef6e-108">tooview applications in hello classic portal, select a directory and choose applications.</span></span>

![](media/active-directory-apps-permissions-consent/apps1.png)

> [!IMPORTANT]
> <span data-ttu-id="bef6e-109">Microsoft rekommenderar att du hanterar Azure AD med hjälp av hello [administrationscentret för Azure AD](https://aad.portal.azure.com) i hello Azure-portalen istället för att använda hello klassiska Azure-portalen som hänvisas till i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="bef6e-109">Microsoft recommends that you manage Azure AD using hello [Azure AD admin center](https://aad.portal.azure.com) in hello Azure portal instead of using hello Azure classic portal referenced in this article.</span></span>

## <a name="types-of-apps"></a><span data-ttu-id="bef6e-110">Typer av appar</span><span class="sxs-lookup"><span data-stu-id="bef6e-110">Types of apps</span></span>

1. <span data-ttu-id="bef6e-111">**Appar för en enda klient**</span><span class="sxs-lookup"><span data-stu-id="bef6e-111">**Single-tenant apps**</span></span> </br>
    - <span data-ttu-id="bef6e-112">**Stöd för en innehavare appar** -ofta kallas tooas branschspecifika (LOB)-appar.</span><span class="sxs-lookup"><span data-stu-id="bef6e-112">**Single-tenant apps** - Often referred tooas line-of-business (LOB) apps.</span></span> <span data-ttu-id="bef6e-113">Detta gäller hello där någon inom din organisation utvecklar egna app och vill att användarna i hello organisation toobe kan toosign i toohello app.</span><span class="sxs-lookup"><span data-stu-id="bef6e-113">This is hello case where someone within your organization develops their own app, and would like users in hello organization toobe able toosign in toohello app.</span></span>
    ![](media/active-directory-apps-permissions-consent/apps2.png)
    - <span data-ttu-id="bef6e-114">**Appen Proxy appar** – när du exponera ett lokalt program med Azure AD App Proxy en enskild klient app har registrerats i din klient (i tillägget toohello tjänsten App Proxy).</span><span class="sxs-lookup"><span data-stu-id="bef6e-114">**App Proxy apps** - When you expose an on-prem application with Azure AD App Proxy, a single-tenant app is registered in your tenant (in addition toohello App Proxy service).</span></span> <span data-ttu-id="bef6e-115">Den här appen är det som representerar ditt lokala program i alla molninteraktioner (till exempel autentisering).</span><span class="sxs-lookup"><span data-stu-id="bef6e-115">This app is what represents your on-prem application for all cloud interactions (for example, authentication).</span></span> <span data-ttu-id="bef6e-116">(App Proxy kräver Azure AD Basic eller högre.)</span><span class="sxs-lookup"><span data-stu-id="bef6e-116">(App Proxy requires Azure AD Basic or higher.)</span></span>


2. <span data-ttu-id="bef6e-117">**Appar för flera klienter**</span><span class="sxs-lookup"><span data-stu-id="bef6e-117">**Multi-tenant apps**</span></span>
    - <span data-ttu-id="bef6e-118">**Flera innehavare appar som andra kan godkänna** - liknande för ”stöd för en innehavare appar som din organisation utvecklar”.</span><span class="sxs-lookup"><span data-stu-id="bef6e-118">**Multi-tenant apps which others can consent to** - similar too“single-tenant apps that your organization develops”.</span></span> <span data-ttu-id="bef6e-119">hello största skillnaden (förutom hello logiken i själva hello appen) är att användare från andra klienter också kan godkänna tooand inloggning toohello app.</span><span class="sxs-lookup"><span data-stu-id="bef6e-119">hello main difference (besides hello logic in hello app itself) is that users from other tenants can also consent tooand sign in toohello app.</span></span></br>
    ![](media/active-directory-apps-permissions-consent/apps4.png)
    - <span data-ttu-id="bef6e-120">**Appar för flera klienter som andra utvecklar, som Contoso kan godkänna**.</span><span class="sxs-lookup"><span data-stu-id="bef6e-120">**Multi-tenant apps others develop, which Contoso can consent to**.</span></span> <span data-ttu-id="bef6e-121">(Eller bara ”godkända appar”.) Detta är hello Vänd sida av ”flera innehavare appar som din organisation utvecklar”.</span><span class="sxs-lookup"><span data-stu-id="bef6e-121">(Or “consented apps”, for short.) This is hello flip side of “multi-tenant apps your organization develops”.</span></span> <span data-ttu-id="bef6e-122">När en annan organisation utvecklar en app för flera innehavare, användare av din organisation medgivande toohello appen och logga in tooit.</span><span class="sxs-lookup"><span data-stu-id="bef6e-122">When another organization develops a multi-tenant app, users of your organization can consent toohello app and sign in tooit.</span></span>
    - <span data-ttu-id="bef6e-123">**Första parts Microsoft-appar** – Appar som representerar Microsoft-tjänster.</span><span class="sxs-lookup"><span data-stu-id="bef6e-123">**Microsoft first-party apps** - Apps that represent Microsoft services.</span></span> <span data-ttu-id="bef6e-124">Medgivande drivs av hello fakta som du registrerar dig för hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="bef6e-124">Consent is driven by hello fact that you sign up for hello service.</span></span> <span data-ttu-id="bef6e-125">Det finns ibland särskilda UX och logik för vissa appar från första part som används ofta när principer runt åtkomst toohello app.</span><span class="sxs-lookup"><span data-stu-id="bef6e-125">There is sometimes special UX and logic for certain first-party apps that is often used when establishing policies around access toohello app.</span></span></br>
    ![](media/active-directory-apps-permissions-consent/apps3.png)
    - <span data-ttu-id="bef6e-126">**Appar förintegrerade** -appar som är tillgängliga i hello Azure AD App-galleri som du kan lägga till tooyour directory tooprovide enkel inloggning (och i vissa fall kan etablera) toopopular SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="bef6e-126">**Pre-integrated apps** - Apps available in hello Azure AD App Gallery, which you can add tooyour directory tooprovide single sign-on (and in some cases, provisioning) toopopular SaaS apps.</span></span>
    - <span data-ttu-id="bef6e-127">**Enkel inloggning i Azure AD**: ”Verklig” enkel inloggning för appar som kan integreras med Azure AD via ett inloggningsprotokoll som stöds, t.ex. SAML 2.0 eller OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="bef6e-127">**Azure AD single sign-on**: “Real” SSO, for apps that can be integrated with Azure AD, through a supported sign-in protocol such as SAML 2.0 or OpenID Connect.</span></span> <span data-ttu-id="bef6e-128">hello guiden vägleder dig genom att ställa in.</span><span class="sxs-lookup"><span data-stu-id="bef6e-128">hello wizard walks you through setting it up.</span></span>
    - <span data-ttu-id="bef6e-129">**Lösenord för enkel inloggning**: Azure AD lagras på ett säkert sätt hello användarens autentiseringsuppgifter för hello app och hello autentiseringsuppgifter är ”matas in” i hello inloggning form av hello webbläsartillägget för Azure AD App-åtkomst.</span><span class="sxs-lookup"><span data-stu-id="bef6e-129">**Password single sign-on**: Azure AD securely stores hello user’s credentials for hello app, and hello credentials are “injected” into hello sign-in form by hello Azure AD App Access browser extension.</span></span> <span data-ttu-id="bef6e-130">Kallas även för ”lösenordsvalv”.</span><span class="sxs-lookup"><span data-stu-id="bef6e-130">Also known as “password vaulting”.</span></span>

## <a name="permissions"></a><span data-ttu-id="bef6e-131">Behörigheter</span><span class="sxs-lookup"><span data-stu-id="bef6e-131">Permissions</span></span>

<span data-ttu-id="bef6e-132">När en app har registrerats definierar hello-användaren som utför hello appregistrering (det vill säga hello developer) vilka behörigheter hello program behöver tillgång till och vilka resurser.</span><span class="sxs-lookup"><span data-stu-id="bef6e-132">When an app is registered, hello user performing hello app registration (that is, hello developer) defines which permissions hello app needs access to, and which resources.</span></span> <span data-ttu-id="bef6e-133">(hello resurser är själva, definierad som andra appar.) Någon skapar en e-post reader-appen skulle till exempel tillstånd att appen kräver hello ”komma åt postlådor som hello inloggade användare” behörighet i hello ”Office 365 Exchange Online” resurs:</span><span class="sxs-lookup"><span data-stu-id="bef6e-133">(hello resources are, themselves, defined as other apps.) For example, someone building a mail reader app, would state that their app requires hello “Access mailboxes as hello signed-in user” permission in hello “Office 365 Exchange Online” resource:</span></span>
    
![](media/active-directory-apps-permissions-consent/apps6.png)

<span data-ttu-id="bef6e-134">För att en app (hello klient) toorequest vissa behörigheter från en annan app (hello resurs) definierar hello utvecklare av hello resurs app hello behörigheter som finns.</span><span class="sxs-lookup"><span data-stu-id="bef6e-134">In order for one app (hello client) toorequest a certain permission from another app (hello resource), hello developer of hello resource app defines hello permissions that exist.</span></span> <span data-ttu-id="bef6e-135">I vårt exempel Microsoft hello ägare hello ”Office 365 Exchange Online” resurs app har definierat en behörighetsgrupp med namnet ”komma åt postlådor som hello inloggade användare”.</span><span class="sxs-lookup"><span data-stu-id="bef6e-135">In our example, Microsoft, hello owner of hello “Office 365 Exchange Online” resource app, have defined a permission named “Access mailboxes as hello signed-in user”.</span></span>

<span data-ttu-id="bef6e-136">När du definierar behörigheter måste hello apputvecklaren definiera om hello behörighet kan vara godkänt för, eller om det krävs admin medgivande.</span><span class="sxs-lookup"><span data-stu-id="bef6e-136">When defining permissions, hello app developer must define if hello permission can be consented to, or if it requires admin consent.</span></span> <span data-ttu-id="bef6e-137">Detta gör att utvecklare tooallow användare tooconsent på sina egna tooapps begär endast Låg känslighet behörigheter, men kräver administratörer tooconsent toomore känsliga behörigheter.</span><span class="sxs-lookup"><span data-stu-id="bef6e-137">This allows developers tooallow users tooconsent on their own tooapps requesting only low-sensitivity permissions, but require admins tooconsent toomore sensitive permissions.</span></span> <span data-ttu-id="bef6e-138">Till exempel Hej ”Azure Active Directory” resurs app, har definierats, så att användarna kan godkänna tooapps, begär begränsad läsbehörighet.</span><span class="sxs-lookup"><span data-stu-id="bef6e-138">For example, hello “Azure Active Directory” resource app, has been defined, so users can consent tooapps, requesting limited read-only permissions.</span></span>  <span data-ttu-id="bef6e-139">För fullständig läsbehörighet och skrivbehörighet krävs administratörens godkännande.</span><span class="sxs-lookup"><span data-stu-id="bef6e-139">However, admin consent is required for full read permissions, and all write permissions.</span></span>

<span data-ttu-id="bef6e-140">Eftersom interna klienter inte autentiseras kan en app som definierats som en intern klientapp endast begära delegerade behörigheter.</span><span class="sxs-lookup"><span data-stu-id="bef6e-140">Because native clients are not authenticated, an app defined as a native client app can only request delegated permissions.</span></span> <span data-ttu-id="bef6e-141">Det innebär att det alltid måste finnas en verklig användare när en token begärs.</span><span class="sxs-lookup"><span data-stu-id="bef6e-141">This means that there must always be an actual user involved when obtaining a token.</span></span> <span data-ttu-id="bef6e-142">Webbappar och webb-API:er (konfidentiella klienter) måste alltid autentisera med Azure AD när en åtkomsttoken begärs.</span><span class="sxs-lookup"><span data-stu-id="bef6e-142">Web apps and web APIs (confidential clients), must always authenticate with Azure AD when getting an access token.</span></span> <span data-ttu-id="bef6e-143">Vilket innebär att de har också hello möjlighet att begära appen endast behörigheter.</span><span class="sxs-lookup"><span data-stu-id="bef6e-143">Meaning they also have hello possibility of requesting app-only permissions.</span></span> <span data-ttu-id="bef6e-144">Om till exempel en backend-tjänst måste tooauthenticate tooanother backend-tjänst.</span><span class="sxs-lookup"><span data-stu-id="bef6e-144">For example, if one back-end service needs tooauthenticate tooanother back-end service.</span></span> <span data-ttu-id="bef6e-145">Program som begär appspecifika behörigheter kräver alltid administratörens godkännande.</span><span class="sxs-lookup"><span data-stu-id="bef6e-145">Applications requesting app-only permissions always require administrator consent.</span></span>

<span data-ttu-id="bef6e-146">Sammanfattningsvis:</span><span class="sxs-lookup"><span data-stu-id="bef6e-146">Summarizing:</span></span>



- <span data-ttu-id="bef6e-147">En app (klient) anger hello behörigheter för andra appar (resurser).</span><span class="sxs-lookup"><span data-stu-id="bef6e-147">An app (client) states hello permissions it needs for other apps (resources).</span></span>
- <span data-ttu-id="bef6e-148">En app (resurs) anger vilka behörigheter som är exponerade tooother appar (klienter).</span><span class="sxs-lookup"><span data-stu-id="bef6e-148">An app (resource) states what permissions are exposed tooother apps (clients).</span></span>
- <span data-ttu-id="bef6e-149">En behörighet kan vara en appspecifik behörighet, eller en delegerad behörighet.</span><span class="sxs-lookup"><span data-stu-id="bef6e-149">A permission can be an app-only permission, or a delegated permission.</span></span>
- <span data-ttu-id="bef6e-150">En delegerad behörighet kan märkas som ”tillåter användargodkännande” eller ”kräver administratörsgodkännande”.</span><span class="sxs-lookup"><span data-stu-id="bef6e-150">A delegated permission can be marked as “allows user consent”, or “requires admin consent”.</span></span>
- <span data-ttu-id="bef6e-151">En app kan fungera som en klient (genom att fastställa att den behöver behörigheter tooa resurs), som en resurs (genom att fastställa vilka behörigheter som den exponerar) eller båda.</span><span class="sxs-lookup"><span data-stu-id="bef6e-151">An app can behave as a client (by declaring that it needs permissions tooa resource), as a resource (by declaring which permissions it exposes), or as both.</span></span>

## <a name="controls"></a><span data-ttu-id="bef6e-152">Kontroller</span><span class="sxs-lookup"><span data-stu-id="bef6e-152">Controls</span></span>

<span data-ttu-id="bef6e-153">hello följer en lista över hello olika admin-kontroller som är tillgängliga för det här problemet.</span><span class="sxs-lookup"><span data-stu-id="bef6e-153">hello following is a list of hello different admin controls available for all this behavior.</span></span> <span data-ttu-id="bef6e-154">Hej administratör kontroller kan användas i hello klassiska portalen från konfigurera hello-katalogen.</span><span class="sxs-lookup"><span data-stu-id="bef6e-154">hello admin controls can be accessed in hello classic portal from configure under hello directory.</span></span>

![](media/active-directory-apps-permissions-consent/apps7.png)

<span data-ttu-id="bef6e-155">I hello Azure portal under **hantera**, **användarinställningar**.</span><span class="sxs-lookup"><span data-stu-id="bef6e-155">In hello Azure portal, under **manage**, **user settings**.</span></span>

![](media/active-directory-apps-permissions-consent/apps11.png)



- <span data-ttu-id="bef6e-156">Du kan styra om användare kan godkänna tooapps:</span><span class="sxs-lookup"><span data-stu-id="bef6e-156">You can control whether users can consent tooapps:</span></span>

<span data-ttu-id="bef6e-157">I hello klassiska portal, väljer **användare kan ge program behörigheter tooaccess sina data.**
![](media/active-directory-apps-permissions-consent/apps8.png)</span><span class="sxs-lookup"><span data-stu-id="bef6e-157">In hello classic portal, select **Users may give applications permissions tooaccess their data.**
![](media/active-directory-apps-permissions-consent/apps8.png)</span></span>

<span data-ttu-id="bef6e-158">Välj i hello Azure-portalen, **användare kan låta appar tooaccess sina data**.</span><span class="sxs-lookup"><span data-stu-id="bef6e-158">In hello Azure portal, select **users can allow apps tooaccess their data**.</span></span>
![](media/active-directory-apps-permissions-consent/apps12.png)



- <span data-ttu-id="bef6e-159">Du kan styra om användare kan registrera sina egna LOB-appar med stöd för en innehavare: hello klassiska portal Välj **användare kan lägga till integrerade program.**
![](media/active-directory-apps-permissions-consent/apps9.png)</span><span class="sxs-lookup"><span data-stu-id="bef6e-159">You can control whether users can register their own single-tenant LOB apps: In hello classic portal select **Users may add integrated applications.**
![](media/active-directory-apps-permissions-consent/apps9.png)</span></span>

<span data-ttu-id="bef6e-160">Välj i hello Azure-portalen, **användare kan låta appar tooaccess sina data**.</span><span class="sxs-lookup"><span data-stu-id="bef6e-160">In hello Azure portal, select **users can allow apps tooaccess their data**.</span></span>
![](media/active-directory-apps-permissions-consent/apps13.png)

>[!NOTE]
><span data-ttu-id="bef6e-161">Även om du tillåter användare tooregister stöd för en innehavare LOB-appar, finns gränser toowhat kan registreras.</span><span class="sxs-lookup"><span data-stu-id="bef6e-161">Even if you do allow users tooregister single-tenant LOB apps, there are limits toowhat can be registered.</span></span>  
><span data-ttu-id="bef6e-162">Ett exempel är utvecklare som inte är katalogadministratörer.</span><span class="sxs-lookup"><span data-stu-id="bef6e-162">For example, developers who are not directory admins.</span></span>
>
>- <span data-ttu-id="bef6e-163">Användare kan inte göra en app för en enda klient till en app för flera klienter.</span><span class="sxs-lookup"><span data-stu-id="bef6e-163">Users cannot make a single-tenant app a multi-tenant app.</span></span>
>- <span data-ttu-id="bef6e-164">När du registrerar en klient LOB-appar kan användarna begära behörigheter endast appen tooother appar.</span><span class="sxs-lookup"><span data-stu-id="bef6e-164">When registering single-tenant LOB apps, users cannot request app-only permissions tooother apps.</span></span>
>- <span data-ttu-id="bef6e-165">När du registrerar en klient LOB-appar kan användarna begära delegerade behörigheter tooother appar om de behörigheterna som kräver godkännande av administratören.</span><span class="sxs-lookup"><span data-stu-id="bef6e-165">When registering single-tenant LOB apps, users cannot request delegated permissions tooother apps if those permissions require admin consent.</span></span>
>- <span data-ttu-id="bef6e-166">Användare kan inte göra ändringar tooapps som de inte är ägare till.</span><span class="sxs-lookup"><span data-stu-id="bef6e-166">Users cannot make changes tooapps that they are not owners of.</span></span>



- <span data-ttu-id="bef6e-167">Du kan styra om användare kan lägga till förintegrerade appar som använder lösenord för enkel inloggning (SSO eller lösenordsvalv) ![](media/active-directory-apps-permissions-consent/apps10.png)</span><span class="sxs-lookup"><span data-stu-id="bef6e-167">You can control whether users can themselves add pre-integrated apps that use password SSO (aka “password vaulting”) ![](media/active-directory-apps-permissions-consent/apps10.png)</span></span>



- <span data-ttu-id="bef6e-168">Du kan styra när program kan nås, så kallad ”villkorlig åtkomst”.</span><span class="sxs-lookup"><span data-stu-id="bef6e-168">You can control under which conditions applications can be accessed (that is, conditional access).</span></span> <span data-ttu-id="bef6e-169">Tänk på detta gäller både toohello klientappen och toohello resurs app.</span><span class="sxs-lookup"><span data-stu-id="bef6e-169">Be aware this applies both toohello client app and toohello resource app.</span></span> <span data-ttu-id="bef6e-170">Så att du ställer in en princip för villkorlig åtkomst som säger hello ”Office 365 Exchange Online” appen endast kan nås från datorer som är kompatibla.</span><span class="sxs-lookup"><span data-stu-id="bef6e-170">So, say you set a conditional access policy that says that hello “Office 365 Exchange Online” app can only be accessed from machines, which are compliant.</span></span>  <span data-ttu-id="bef6e-171">Den här principen startar också när en användare försöker toouse ett klientprogram som begär behörighet tooExchange Online.</span><span class="sxs-lookup"><span data-stu-id="bef6e-171">This policy will also kick in when a user attempts toouse a client app which requests permissions tooExchange Online.</span></span>



- <span data-ttu-id="bef6e-172">Du har insyn i vilka appar har tilllåten tooand vilka som används.</span><span class="sxs-lookup"><span data-stu-id="bef6e-172">You have visibility into which apps have been consented tooand which ones are being used.</span></span>

1.  <span data-ttu-id="bef6e-173">När en användare godkänner tooan app, har en ServicePrincipal-objektet skapats i hello-klient.</span><span class="sxs-lookup"><span data-stu-id="bef6e-173">When a user consents tooan app, a ServicePrincipal object is created in hello tenant.</span></span> <span data-ttu-id="bef6e-174">Skapa en ServicePrincipal ingår i hello kontrollrapport.</span><span class="sxs-lookup"><span data-stu-id="bef6e-174">ServicePrincipal creation is included in hello audit report.</span></span>
2.  <span data-ttu-id="bef6e-175">Inloggningsaktivitet Användarrapporter berätta vilken app hello användare loggar in på.</span><span class="sxs-lookup"><span data-stu-id="bef6e-175">User sign-in activity reports tell you which app hello user is signing in to.</span></span> 

## <a name="example"></a><span data-ttu-id="bef6e-176">Exempel</span><span class="sxs-lookup"><span data-stu-id="bef6e-176">Example</span></span>

<span data-ttu-id="bef6e-177">Exempelvis ta hello ”FabrikamMail för Office 365” app som du har lagt märke till användare i din klient loggar in på.</span><span class="sxs-lookup"><span data-stu-id="bef6e-177">As an example, let’s take hello “FabrikamMail for Office 365” app, which you’ve noticed users in your tenant are signing in to.</span></span> <span data-ttu-id="bef6e-178">”FabrikamMail” är en e-postapp för Android, som publicerats av ”Fabrikam, Inc.”.</span><span class="sxs-lookup"><span data-stu-id="bef6e-178">“FabrikamMail” is a mail reader app for Android, published by “Fabrikam, Inc.”.</span></span> <span data-ttu-id="bef6e-179">Detta hamnar i hello ”flera innehavare appar andra utveckla som Contoso kan samtycker till att”.</span><span class="sxs-lookup"><span data-stu-id="bef6e-179">This falls into hello “multi-tenant apps other develop, which Contoso can consent to”.</span></span>

<span data-ttu-id="bef6e-180">Om användare tillåts tooconsent kan få de en fråga hello medgivande första gången de loggar in:![](media/active-directory-apps-permissions-consent/apps14.png)</span><span class="sxs-lookup"><span data-stu-id="bef6e-180">If users are allowed tooconsent, they get a consent prompt hello first time they sign in: ![](media/active-directory-apps-permissions-consent/apps14.png)</span></span>

<span data-ttu-id="bef6e-181">”Åtkomst till dina postlådor” är hello användarinriktad medgivande sträng för hello ”komma åt postlådor som hello inloggade användare” behörighet som exponeras av ”Office 365 Exchange Online” (det vill säga Exchange).</span><span class="sxs-lookup"><span data-stu-id="bef6e-181">“Access your mailboxes” is hello user-facing consent string for hello “Access mailboxes as hello signed-in user” permission exposed by “Office 365 Exchange Online” (that is, Exchange).</span></span>

<span data-ttu-id="bef6e-182">Du kan se hello behörigheter genom att leta upp hello ServicePrincipal-objekt för Exchange (hello resurs) som har lagts till när du registrerade dig för Office 365.</span><span class="sxs-lookup"><span data-stu-id="bef6e-182">You can see hello permissions by looking up hello ServicePrincipal object for Exchange (hello resource), which was added when you signed up for Office 365.</span></span> <span data-ttu-id="bef6e-183">Du kan se hello ServicePrincipal objekt av en ”instans” hello-appen i din klient som används för att registrera olika alternativ och konfigurationer.</span><span class="sxs-lookup"><span data-stu-id="bef6e-183">You can think of hello ServicePrincipal object of an “instance” of hello app in your tenant, which is used for recording different options and configurations.</span></span>  <span data-ttu-id="bef6e-184">Du kan se detta med hjälp av hello `Get-AzureADServicePrincipal` i PowerShell.</span><span class="sxs-lookup"><span data-stu-id="bef6e-184">You can see this by using hello `Get-AzureADServicePrincipal` in PowerShell.</span></span>

    PS C:\> Get-AzureADServicePrincipal -ObjectId 383f7b97-6754-4d3d-9474-3908ebcba1c6 | fl *
    
    DeletionTimeStamp         : 
    ObjectId                  : 383f7b97-6754-4d3d-9474-3908ebcba1c6
    ObjectType                : ServicePrincipal
    AccountEnabled            : True
    AppDisplayName            : Office 365 Exchange Online
    AppId                     : 00000002-0000-0ff1-ce00-000000000000
    AppOwnerTenantId          : 
    AppRoleAssignmentRequired : False
    AppRoles                  : {...}
    DisplayName               : Microsoft.Exchange
    ErrorUrl                  : 
    Homepage                  : 
    KeyCredentials            : {}
    LogoutUrl                 : 
    Oauth2Permissions         : {...
                                , class OAuth2Permission {
                                  AdminConsentDescription : Allows hello app toohave hello same access toomailboxes as hello signed-in user via Exchange Web Services.
                                  AdminConsentDisplayName : Access mailboxes as hello signed-in user via Exchange Web Services
                                  Id                      : 3b5f3d61-589b-4a3c-a359-5dd4b5ee5bd5
                                  IsEnabled               : True
                                  Type                    : User
                                  UserConsentDescription  : Allows hello app full access tooyour mailboxes on your behalf.
                                  UserConsentDisplayName  : Access your mailboxes
                                  Value                   : full_access_as_user
                                },
                                ...}
    PasswordCredentials       : {}
    PublisherName             : 
    ReplyUrl                  : 
    SamlMetadataUrl           : 
    ServicePrincipalNames     : {00000002-0000-0ff1-ce00-000000000000/outlook.office365.com, 00000002-0000-0ff1-ce00-000000000000/mail.office365.com, 00000002-0000-0ff1-ce00-000000000000/outlook.com, 
                                00000002-0000-0ff1-ce00-000000000000/*.outlook.com...}
    Tags                      : {}

<span data-ttu-id="bef6e-185">Medgivande initieras när hello användaren klickar på ”Acceptera”.</span><span class="sxs-lookup"><span data-stu-id="bef6e-185">Consent is initiated when hello user clicks “Accept”.</span></span> <span data-ttu-id="bef6e-186">Först skapas en ServicePrincipal-objekt för ”FabrikamMail för Office 365” i hello-klient.</span><span class="sxs-lookup"><span data-stu-id="bef6e-186">First, a ServicePrincipal object for “FabrikamMail for Office 365” is created in hello tenant.</span></span> <span data-ttu-id="bef6e-187">Hej ServicePrincipal ser ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="bef6e-187">hello ServicePrincipal looks something like this:</span></span>

    PS C:\> Get-AzureADServicePrincipal -SearchString "FabrikamMail for Office 365" | fl *
    
    DeletionTimeStamp         : 
    ObjectId                  : a8b16333-851d-42e8-acd2-eac155849b37
    ObjectType                : ServicePrincipal
    AccountEnabled            : True
    AppDisplayName            : FabrikamMail for Office 365
    AppId                     : aba7c072-2267-4031-8960-e7a2db6e0590
    AppOwnerTenantId          : 4a4076e0-a70f-41c6-b819-6f9c4a86df89
    AppRoleAssignmentRequired : False
    AppRoles                  : {}
    DisplayName               : FabrikamMail for Office 365
    ErrorUrl                  : 
    Homepage                  : 
    KeyCredentials            : {}
    LogoutUrl                 : 
    Oauth2Permissions         : {}
    PasswordCredentials       : {}
    PublisherName             : Fabrikam, Inc.
    ReplyUrl                  : 
    SamlMetadataUrl           : 
    ServicePrincipalNames     : {aba7c072-2267-4031-8960-e7a2db6e0590}
    Tags                      : {WindowsAzureActiveDirectoryIntegratedApp}

<span data-ttu-id="bef6e-188">Principer tooan app skapar en Oauth2PermissionGrant länk mellan hello följande:</span><span class="sxs-lookup"><span data-stu-id="bef6e-188">Consenting tooan app creates an Oauth2PermissionGrant link between hello following:</span></span>
  
- <span data-ttu-id="bef6e-189">hello användarobjekt</span><span class="sxs-lookup"><span data-stu-id="bef6e-189">hello user object</span></span>
- <span data-ttu-id="bef6e-190">Hej klientappar ServicePrincipalName (SPN)</span><span class="sxs-lookup"><span data-stu-id="bef6e-190">hello client apps ServicePrincipalName (SPN)</span></span>
- <span data-ttu-id="bef6e-191">hello resurs appar ServicePrincipalName (SPN)</span><span class="sxs-lookup"><span data-stu-id="bef6e-191">hello resource apps ServicePrincipalName (SPN)</span></span>
- <span data-ttu-id="bef6e-192">behörigheter i hello resurs app.</span><span class="sxs-lookup"><span data-stu-id="bef6e-192">permissions in hello resource app.</span></span>  

<span data-ttu-id="bef6e-193">Hello gäller FabrikamMail, ser ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="bef6e-193">In hello case of FabrikamMail, it looks something like this:</span></span>

    PS C:\> Get-AzureADUserOAuth2PermissionGrant -ObjectId ddiggle@aadpremiumlab.onmicrosoft.com | fl *
    
    ClientId    : a8b16333-851d-42e8-acd2-eac155849b37
    ConsentType : Principal
    ExpiryTime  : 05/15/2017 07:02:39 AM
    ObjectId    : M2OxqB2F6EKs0urBVYSbN5d7PzhUZz1NlH25COvLocbJjoxkUFfRQauryBKwBWet
    PrincipalId : 648c8ec9-5750-41d1-abab-c812b00567ad
    ResourceId  : 383f7b97-6754-4d3d-9474-3908ebcba1c6
    Scope       : full_access_as_user
    StartTime   : 01/01/0001 12:00:00 AM

<span data-ttu-id="bef6e-194">(**ClientId** är Fabrikammail's service principal objekt-ID (hello som du just har skapat), **PrincipalId** är hello användaren objekt-ID (för hello användare som godkänt), **ResourceId**är Exchange's service principal objekt-ID, Scope är hello behörighet i Exchange som har godkänt för).</span><span class="sxs-lookup"><span data-stu-id="bef6e-194">(**ClientId** is FabrikamMail’s service principal object ID (hello one that just got created), **PrincipalId** is hello user object ID (of hello user who consented), **ResourceId** is Exchange’s service principal object ID, Scope is hello permission in Exchange that was consented to).</span></span>

<span data-ttu-id="bef6e-195">Om användarna inte får tooconsent, visas en skärm som säger behörigheten krävs.</span><span class="sxs-lookup"><span data-stu-id="bef6e-195">If users are not allowed tooconsent, they will see a screen that says that permission is required.</span></span>

