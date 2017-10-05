---
title: "Appar, behörigheter och godkännande i Azure Active Directory.| Microsoft Docs"
description: "Azure AD Connect integrerar dina lokala kataloger med Azure Active Directory. På så sätt kan du tillhandahålla en gemensam identitet för Office 365-, Azure- och SaaS-program som är integrerade med Azure AD."
keywords: introduction to Azure AD, apps, what is Azure AD Connect, install active directory
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
ms.openlocfilehash: 6f6baf5e1538fb280a899065c64ca5688473c04a
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="apps-permissions-and-consent-in-azure-active-directory"></a><span data-ttu-id="eb985-105">Appar, behörigheter och godkännande i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="eb985-105">Apps, permissions, and consent in Azure Active Directory</span></span>
<span data-ttu-id="eb985-106">I Azure Active Directory kan du lägga till program i din katalog.</span><span class="sxs-lookup"><span data-stu-id="eb985-106">Within Azure Active Directory, you can add applications to your directory.</span></span>  <span data-ttu-id="eb985-107">Programmen kan variera beroende på typen av program.</span><span class="sxs-lookup"><span data-stu-id="eb985-107">The applications can vary depending on the type of application.</span></span>  <span data-ttu-id="eb985-108">Om du vill visa program på den klassiska portalen markerar du en katalog och väljer program.</span><span class="sxs-lookup"><span data-stu-id="eb985-108">To view applications in the classic portal, select a directory and choose applications.</span></span>

![](media/active-directory-apps-permissions-consent/apps1.png)

> [!IMPORTANT]
> <span data-ttu-id="eb985-109">Microsoft rekommenderar att du hanterar Azure AD via [Azure AD administratörscenter](https://aad.portal.azure.com) på Azure Portal istället för via den klassiska Azure-portalen som nämns i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="eb985-109">Microsoft recommends that you manage Azure AD using the [Azure AD admin center](https://aad.portal.azure.com) in the Azure portal instead of using the Azure classic portal referenced in this article.</span></span>

## <a name="types-of-apps"></a><span data-ttu-id="eb985-110">Typer av appar</span><span class="sxs-lookup"><span data-stu-id="eb985-110">Types of apps</span></span>

1. <span data-ttu-id="eb985-111">**Appar för en enda klient**</span><span class="sxs-lookup"><span data-stu-id="eb985-111">**Single-tenant apps**</span></span> </br>
    - <span data-ttu-id="eb985-112">**Appar för en enda klient** – Kallas ofta för verksamhetsspecifika appar (LOB, Line-Of-Business).</span><span class="sxs-lookup"><span data-stu-id="eb985-112">**Single-tenant apps** - Often referred to as line-of-business (LOB) apps.</span></span> <span data-ttu-id="eb985-113">Det här är fallet när någon i din organisation utvecklar sin egen app och vill att användare i organisationen ska kunna logga in i appen.</span><span class="sxs-lookup"><span data-stu-id="eb985-113">This is the case where someone within your organization develops their own app, and would like users in the organization to be able to sign in to the app.</span></span>
    ![](media/active-directory-apps-permissions-consent/apps2.png)
    - <span data-ttu-id="eb985-114">**App Proxy-appar** – När du exponerar ett lokalt program med Azure AD App Proxy registreras en app för en enda klient i din klientorganisation (förutom App Proxy-tjänsten).</span><span class="sxs-lookup"><span data-stu-id="eb985-114">**App Proxy apps** - When you expose an on-prem application with Azure AD App Proxy, a single-tenant app is registered in your tenant (in addition to the App Proxy service).</span></span> <span data-ttu-id="eb985-115">Den här appen är det som representerar ditt lokala program i alla molninteraktioner (till exempel autentisering).</span><span class="sxs-lookup"><span data-stu-id="eb985-115">This app is what represents your on-prem application for all cloud interactions (for example, authentication).</span></span> <span data-ttu-id="eb985-116">(App Proxy kräver Azure AD Basic eller högre.)</span><span class="sxs-lookup"><span data-stu-id="eb985-116">(App Proxy requires Azure AD Basic or higher.)</span></span>


2. <span data-ttu-id="eb985-117">**Appar för flera klienter**</span><span class="sxs-lookup"><span data-stu-id="eb985-117">**Multi-tenant apps**</span></span>
    - <span data-ttu-id="eb985-118">**Appar för flera klienter som andra kan godkänna** – Påminner om ”appar för en enda klient som din organisation utvecklar”.</span><span class="sxs-lookup"><span data-stu-id="eb985-118">**Multi-tenant apps which others can consent to** - similar to “single-tenant apps that your organization develops”.</span></span> <span data-ttu-id="eb985-119">Den största skillnaden (förutom logiken i själva appen) är att användare från andra klientorganisationer också kan godkänna och logga in i appen.</span><span class="sxs-lookup"><span data-stu-id="eb985-119">The main difference (besides the logic in the app itself) is that users from other tenants can also consent to and sign in to the app.</span></span></br>
    ![](media/active-directory-apps-permissions-consent/apps4.png)
    - <span data-ttu-id="eb985-120">**Appar för flera klienter som andra utvecklar, som Contoso kan godkänna**.</span><span class="sxs-lookup"><span data-stu-id="eb985-120">**Multi-tenant apps others develop, which Contoso can consent to**.</span></span> <span data-ttu-id="eb985-121">(Eller bara ”godkända appar”.) Det här är den andra sidan av ”appar för flera klienter som din organisation utvecklar”.</span><span class="sxs-lookup"><span data-stu-id="eb985-121">(Or “consented apps”, for short.) This is the flip side of “multi-tenant apps your organization develops”.</span></span> <span data-ttu-id="eb985-122">När en annan organisation utvecklar en app för flera klienter kan användarna i din organisation godkänna appen och logga in i den.</span><span class="sxs-lookup"><span data-stu-id="eb985-122">When another organization develops a multi-tenant app, users of your organization can consent to the app and sign in to it.</span></span>
    - <span data-ttu-id="eb985-123">**Första parts Microsoft-appar** – Appar som representerar Microsoft-tjänster.</span><span class="sxs-lookup"><span data-stu-id="eb985-123">**Microsoft first-party apps** - Apps that represent Microsoft services.</span></span> <span data-ttu-id="eb985-124">Du godkänner tjänsten genom att registrera dig för den.</span><span class="sxs-lookup"><span data-stu-id="eb985-124">Consent is driven by the fact that you sign up for the service.</span></span> <span data-ttu-id="eb985-125">Ibland finns det särskilda användargränssnitt och logik för vissa förstapartsappar som ofta används när appåtkomstprinciper etableras.</span><span class="sxs-lookup"><span data-stu-id="eb985-125">There is sometimes special UX and logic for certain first-party apps that is often used when establishing policies around access to the app.</span></span></br>
    ![](media/active-directory-apps-permissions-consent/apps3.png)
    - <span data-ttu-id="eb985-126">**Förintegrerade appar** – Appar som är tillgängliga i Azure AD App-galleriet, som du kan lägga till i din katalog för att tillhandahålla enkel inloggning (och i vissa fall etablering) i populära SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="eb985-126">**Pre-integrated apps** - Apps available in the Azure AD App Gallery, which you can add to your directory to provide single sign-on (and in some cases, provisioning) to popular SaaS apps.</span></span>
    - <span data-ttu-id="eb985-127">**Enkel inloggning i Azure AD**: ”Verklig” enkel inloggning för appar som kan integreras med Azure AD via ett inloggningsprotokoll som stöds, t.ex. SAML 2.0 eller OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="eb985-127">**Azure AD single sign-on**: “Real” SSO, for apps that can be integrated with Azure AD, through a supported sign-in protocol such as SAML 2.0 or OpenID Connect.</span></span> <span data-ttu-id="eb985-128">Guiden vägleder dig genom konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="eb985-128">The wizard walks you through setting it up.</span></span>
    - <span data-ttu-id="eb985-129">**Lösenord för enkel inloggning**: Azure AD lagrar säkert användarens autentiseringsuppgifter för appen, och autentiseringsuppgifterna matas in i inloggningsformuläret av webbläsartillägget Azure AD App Access.</span><span class="sxs-lookup"><span data-stu-id="eb985-129">**Password single sign-on**: Azure AD securely stores the user’s credentials for the app, and the credentials are “injected” into the sign-in form by the Azure AD App Access browser extension.</span></span> <span data-ttu-id="eb985-130">Kallas även för ”lösenordsvalv”.</span><span class="sxs-lookup"><span data-stu-id="eb985-130">Also known as “password vaulting”.</span></span>

## <a name="permissions"></a><span data-ttu-id="eb985-131">Behörigheter</span><span class="sxs-lookup"><span data-stu-id="eb985-131">Permissions</span></span>

<span data-ttu-id="eb985-132">När en app registreras definierar användaren som utför registreringen (dvs. utvecklaren) vilka behörigheter appen behöver åtkomst till, och vilka resurser.</span><span class="sxs-lookup"><span data-stu-id="eb985-132">When an app is registered, the user performing the app registration (that is, the developer) defines which permissions the app needs access to, and which resources.</span></span> <span data-ttu-id="eb985-133">(Själva resurserna definieras som andra appar.) Någon som till exempel bygger en app för e-postläsning anger att appen behöver behörigheten ”Access mailboxes as the signed-in user” (Komma åt postlådor som den inloggade användaren) i resursen ”Office 365 Exchange Online”:</span><span class="sxs-lookup"><span data-stu-id="eb985-133">(The resources are, themselves, defined as other apps.) For example, someone building a mail reader app, would state that their app requires the “Access mailboxes as the signed-in user” permission in the “Office 365 Exchange Online” resource:</span></span>
    
![](media/active-directory-apps-permissions-consent/apps6.png)

<span data-ttu-id="eb985-134">För att en app (klienten) ska kunna begära en viss behörighet från en annan app (resurs) definierar resursappens utvecklaren de behörigheter som är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="eb985-134">In order for one app (the client) to request a certain permission from another app (the resource), the developer of the resource app defines the permissions that exist.</span></span> <span data-ttu-id="eb985-135">I vårt exempel har Microsoft, ägaren av resursappen ”Office 365 Exchange Online”, definierat en behörighet med namnet ”Access mailboxes as the signed-in user” (Komma åt postlådor som den inloggade användaren).</span><span class="sxs-lookup"><span data-stu-id="eb985-135">In our example, Microsoft, the owner of the “Office 365 Exchange Online” resource app, have defined a permission named “Access mailboxes as the signed-in user”.</span></span>

<span data-ttu-id="eb985-136">När behörigheterna definieras måste apputvecklaren ange om användaren kan godkänna behörigheten, eller om administratörens godkännande krävs.</span><span class="sxs-lookup"><span data-stu-id="eb985-136">When defining permissions, the app developer must define if the permission can be consented to, or if it requires admin consent.</span></span> <span data-ttu-id="eb985-137">På så sätt kan utvecklaren tillåta att användarna själva godkänner appar som bara kräver låg behörighet, men kräva att administratörer godkänner känsligare behörigheter.</span><span class="sxs-lookup"><span data-stu-id="eb985-137">This allows developers to allow users to consent on their own to apps requesting only low-sensitivity permissions, but require admins to consent to more sensitive permissions.</span></span> <span data-ttu-id="eb985-138">Till exempel har resursappen ”Azure Active Directory” definierats så att användarna kan godkänna appar, men med begränsad läsbehörighet.</span><span class="sxs-lookup"><span data-stu-id="eb985-138">For example, the “Azure Active Directory” resource app, has been defined, so users can consent to apps, requesting limited read-only permissions.</span></span>  <span data-ttu-id="eb985-139">För fullständig läsbehörighet och skrivbehörighet krävs administratörens godkännande.</span><span class="sxs-lookup"><span data-stu-id="eb985-139">However, admin consent is required for full read permissions, and all write permissions.</span></span>

<span data-ttu-id="eb985-140">Eftersom interna klienter inte autentiseras kan en app som definierats som en intern klientapp endast begära delegerade behörigheter.</span><span class="sxs-lookup"><span data-stu-id="eb985-140">Because native clients are not authenticated, an app defined as a native client app can only request delegated permissions.</span></span> <span data-ttu-id="eb985-141">Det innebär att det alltid måste finnas en verklig användare när en token begärs.</span><span class="sxs-lookup"><span data-stu-id="eb985-141">This means that there must always be an actual user involved when obtaining a token.</span></span> <span data-ttu-id="eb985-142">Webbappar och webb-API:er (konfidentiella klienter) måste alltid autentisera med Azure AD när en åtkomsttoken begärs.</span><span class="sxs-lookup"><span data-stu-id="eb985-142">Web apps and web APIs (confidential clients), must always authenticate with Azure AD when getting an access token.</span></span> <span data-ttu-id="eb985-143">Det innebär att de också har möjlighet att begära appspecifika behörigheter.</span><span class="sxs-lookup"><span data-stu-id="eb985-143">Meaning they also have the possibility of requesting app-only permissions.</span></span> <span data-ttu-id="eb985-144">Om en backend-tjänst till exempel måste autentisera till en annan backend-tjänst.</span><span class="sxs-lookup"><span data-stu-id="eb985-144">For example, if one back-end service needs to authenticate to another back-end service.</span></span> <span data-ttu-id="eb985-145">Program som begär appspecifika behörigheter kräver alltid administratörens godkännande.</span><span class="sxs-lookup"><span data-stu-id="eb985-145">Applications requesting app-only permissions always require administrator consent.</span></span>

<span data-ttu-id="eb985-146">Sammanfattningsvis:</span><span class="sxs-lookup"><span data-stu-id="eb985-146">Summarizing:</span></span>



- <span data-ttu-id="eb985-147">En app (klient) anger de behörigheter som den behöver för andra appar (resurser).</span><span class="sxs-lookup"><span data-stu-id="eb985-147">An app (client) states the permissions it needs for other apps (resources).</span></span>
- <span data-ttu-id="eb985-148">En app (resurs) anger vilka behörigheter som exponeras för andra appar (klienter).</span><span class="sxs-lookup"><span data-stu-id="eb985-148">An app (resource) states what permissions are exposed to other apps (clients).</span></span>
- <span data-ttu-id="eb985-149">En behörighet kan vara en appspecifik behörighet, eller en delegerad behörighet.</span><span class="sxs-lookup"><span data-stu-id="eb985-149">A permission can be an app-only permission, or a delegated permission.</span></span>
- <span data-ttu-id="eb985-150">En delegerad behörighet kan märkas som ”tillåter användargodkännande” eller ”kräver administratörsgodkännande”.</span><span class="sxs-lookup"><span data-stu-id="eb985-150">A delegated permission can be marked as “allows user consent”, or “requires admin consent”.</span></span>
- <span data-ttu-id="eb985-151">En app kan fungera som en klient (genom att deklarera att den behöver behörigheter till en resurs), som en resurs (genom att deklarera vilka behörigheter som den exponerar) eller som båda.</span><span class="sxs-lookup"><span data-stu-id="eb985-151">An app can behave as a client (by declaring that it needs permissions to a resource), as a resource (by declaring which permissions it exposes), or as both.</span></span>

## <a name="controls"></a><span data-ttu-id="eb985-152">Kontroller</span><span class="sxs-lookup"><span data-stu-id="eb985-152">Controls</span></span>

<span data-ttu-id="eb985-153">Följande är en lista över de olika administratörskontroller som är tillgängliga för detta beteende.</span><span class="sxs-lookup"><span data-stu-id="eb985-153">The following is a list of the different admin controls available for all this behavior.</span></span> <span data-ttu-id="eb985-154">Administratörskontrollerna kan användas på den klassiska portalen från Konfigurera under katalogen.</span><span class="sxs-lookup"><span data-stu-id="eb985-154">The admin controls can be accessed in the classic portal from configure under the directory.</span></span>

![](media/active-directory-apps-permissions-consent/apps7.png)

<span data-ttu-id="eb985-155">På Azure Portal, under **Hantera**, **Användarinställningar**.</span><span class="sxs-lookup"><span data-stu-id="eb985-155">In the Azure portal, under **manage**, **user settings**.</span></span>

![](media/active-directory-apps-permissions-consent/apps11.png)



- <span data-ttu-id="eb985-156">Du kan styra om användare kan godkänna appar eller inte:</span><span class="sxs-lookup"><span data-stu-id="eb985-156">You can control whether users can consent to apps:</span></span>

<span data-ttu-id="eb985-157">På den klassiska portalen väljer du **Users may give applications permissions to access their data (Användare kan ge program behörighet att komma åt deras data).**
![](media/active-directory-apps-permissions-consent/apps8.png)</span><span class="sxs-lookup"><span data-stu-id="eb985-157">In the classic portal, select **Users may give applications permissions to access their data.**
![](media/active-directory-apps-permissions-consent/apps8.png)</span></span>

<span data-ttu-id="eb985-158">På Azure Portal väljer du **Användare kan bevilja appar åtkomst till sina data**.</span><span class="sxs-lookup"><span data-stu-id="eb985-158">In the Azure portal, select **users can allow apps to access their data**.</span></span>
![](media/active-directory-apps-permissions-consent/apps12.png)



- <span data-ttu-id="eb985-159">Du kan styra om användare kan registrera sina egna LOB-appar för en enda klient: På den klassiska portalen väljer du **Users may add integrated applications (Användare kan lägga till integrerade program).**
![](media/active-directory-apps-permissions-consent/apps9.png)</span><span class="sxs-lookup"><span data-stu-id="eb985-159">You can control whether users can register their own single-tenant LOB apps: In the classic portal select **Users may add integrated applications.**
![](media/active-directory-apps-permissions-consent/apps9.png)</span></span>

<span data-ttu-id="eb985-160">På Azure Portal väljer du **Användare kan bevilja appar åtkomst till sina data**.</span><span class="sxs-lookup"><span data-stu-id="eb985-160">In the Azure portal, select **users can allow apps to access their data**.</span></span>
![](media/active-directory-apps-permissions-consent/apps13.png)

>[!NOTE]
><span data-ttu-id="eb985-161">Även om du tillåter att användare registrerar LOB-appar för en enda klient finns det gränser för vad som kan registreras.</span><span class="sxs-lookup"><span data-stu-id="eb985-161">Even if you do allow users to register single-tenant LOB apps, there are limits to what can be registered.</span></span>  
><span data-ttu-id="eb985-162">Ett exempel är utvecklare som inte är katalogadministratörer.</span><span class="sxs-lookup"><span data-stu-id="eb985-162">For example, developers who are not directory admins.</span></span>
>
>- <span data-ttu-id="eb985-163">Användare kan inte göra en app för en enda klient till en app för flera klienter.</span><span class="sxs-lookup"><span data-stu-id="eb985-163">Users cannot make a single-tenant app a multi-tenant app.</span></span>
>- <span data-ttu-id="eb985-164">När användare registrerar LOB-appar för en enda klient kan de inte begära appspecifika behörigheter till andra appar.</span><span class="sxs-lookup"><span data-stu-id="eb985-164">When registering single-tenant LOB apps, users cannot request app-only permissions to other apps.</span></span>
>- <span data-ttu-id="eb985-165">När användare registrerar LOB-appar för en enda klient kan de inte begära delegerade behörigheter till andra appar om dessa behörigheter kräver administratörens godkännande.</span><span class="sxs-lookup"><span data-stu-id="eb985-165">When registering single-tenant LOB apps, users cannot request delegated permissions to other apps if those permissions require admin consent.</span></span>
>- <span data-ttu-id="eb985-166">Användare kan inte göra ändringar i appar som de inte äger.</span><span class="sxs-lookup"><span data-stu-id="eb985-166">Users cannot make changes to apps that they are not owners of.</span></span>



- <span data-ttu-id="eb985-167">Du kan styra om användare kan lägga till förintegrerade appar som använder lösenord för enkel inloggning (SSO eller lösenordsvalv) ![](media/active-directory-apps-permissions-consent/apps10.png)</span><span class="sxs-lookup"><span data-stu-id="eb985-167">You can control whether users can themselves add pre-integrated apps that use password SSO (aka “password vaulting”) ![](media/active-directory-apps-permissions-consent/apps10.png)</span></span>



- <span data-ttu-id="eb985-168">Du kan styra när program kan nås, så kallad ”villkorlig åtkomst”.</span><span class="sxs-lookup"><span data-stu-id="eb985-168">You can control under which conditions applications can be accessed (that is, conditional access).</span></span> <span data-ttu-id="eb985-169">Tänk på att detta gäller både klientappen och resursappen.</span><span class="sxs-lookup"><span data-stu-id="eb985-169">Be aware this applies both to the client app and to the resource app.</span></span> <span data-ttu-id="eb985-170">Anta till exempel att du skapar en princip för villkorlig åtkomst som anger att appen ”Office 365 Exchange Online” endast kan nås från datorer som följer standard.</span><span class="sxs-lookup"><span data-stu-id="eb985-170">So, say you set a conditional access policy that says that the “Office 365 Exchange Online” app can only be accessed from machines, which are compliant.</span></span>  <span data-ttu-id="eb985-171">Den här principen tillämpas också när en användare försöker använda en klientapp som begär behörighet till Exchange Online.</span><span class="sxs-lookup"><span data-stu-id="eb985-171">This policy will also kick in when a user attempts to use a client app which requests permissions to Exchange Online.</span></span>



- <span data-ttu-id="eb985-172">Du kan se vilka appar som har godkänts och vilka som används.</span><span class="sxs-lookup"><span data-stu-id="eb985-172">You have visibility into which apps have been consented to and which ones are being used.</span></span>

1.  <span data-ttu-id="eb985-173">När en användare godkänner en app skapas ett ServicePrincipal-objekt i klientorganisationen.</span><span class="sxs-lookup"><span data-stu-id="eb985-173">When a user consents to an app, a ServicePrincipal object is created in the tenant.</span></span> <span data-ttu-id="eb985-174">Genereringen av ServicePrincipal-objektet visas i granskningsrapporten.</span><span class="sxs-lookup"><span data-stu-id="eb985-174">ServicePrincipal creation is included in the audit report.</span></span>
2.  <span data-ttu-id="eb985-175">I rapporterna över användarnas inloggningsaktivitet kan du se vilken app som användaren är inloggad i.</span><span class="sxs-lookup"><span data-stu-id="eb985-175">User sign-in activity reports tell you which app the user is signing in to.</span></span> 

## <a name="example"></a><span data-ttu-id="eb985-176">Exempel</span><span class="sxs-lookup"><span data-stu-id="eb985-176">Example</span></span>

<span data-ttu-id="eb985-177">Anta till exempel att du har lagt märke till att användare i din klientorganisation loggar in i appen ”FabrikamMail för Office 365”.</span><span class="sxs-lookup"><span data-stu-id="eb985-177">As an example, let’s take the “FabrikamMail for Office 365” app, which you’ve noticed users in your tenant are signing in to.</span></span> <span data-ttu-id="eb985-178">”FabrikamMail” är en e-postapp för Android, som publicerats av ”Fabrikam, Inc.”.</span><span class="sxs-lookup"><span data-stu-id="eb985-178">“FabrikamMail” is a mail reader app for Android, published by “Fabrikam, Inc.”.</span></span> <span data-ttu-id="eb985-179">Appen hör till kategorin ”appar för flera klienter som andra utvecklar, som Contoso kan godkänna”.</span><span class="sxs-lookup"><span data-stu-id="eb985-179">This falls into the “multi-tenant apps other develop, which Contoso can consent to”.</span></span>

<span data-ttu-id="eb985-180">Om användare har tillåtelse att godkänna appen uppmanas de att godkänna den första gången de loggar in: ![](media/active-directory-apps-permissions-consent/apps14.png)</span><span class="sxs-lookup"><span data-stu-id="eb985-180">If users are allowed to consent, they get a consent prompt the first time they sign in: ![](media/active-directory-apps-permissions-consent/apps14.png)</span></span>

<span data-ttu-id="eb985-181">”Access your mailboxes” (Åtkomst till dina postlådor) är en sträng som ber om användarens godkännande för behörigheten ”Access mailboxes as the signed-in user” (Åtkomst till postlådor som den inloggade användaren) som exponeras av ”Office 365 Exchange Online” (dvs. Exchange).</span><span class="sxs-lookup"><span data-stu-id="eb985-181">“Access your mailboxes” is the user-facing consent string for the “Access mailboxes as the signed-in user” permission exposed by “Office 365 Exchange Online” (that is, Exchange).</span></span>

<span data-ttu-id="eb985-182">Du kan se behörigheterna genom att leta upp ServicePrincipal-objektet för Exchange (resursen), som lades till när du registrerade dig för Office 365.</span><span class="sxs-lookup"><span data-stu-id="eb985-182">You can see the permissions by looking up the ServicePrincipal object for Exchange (the resource), which was added when you signed up for Office 365.</span></span> <span data-ttu-id="eb985-183">Tänk dig ServicePrincipal-objektet som en ”instans” av appen i din klientorganisation, som används för att registrera olika alternativ och konfigurationer.</span><span class="sxs-lookup"><span data-stu-id="eb985-183">You can think of the ServicePrincipal object of an “instance” of the app in your tenant, which is used for recording different options and configurations.</span></span>  <span data-ttu-id="eb985-184">Du kan visa detta med hjälp av `Get-AzureADServicePrincipal` i PowerShell.</span><span class="sxs-lookup"><span data-stu-id="eb985-184">You can see this by using the `Get-AzureADServicePrincipal` in PowerShell.</span></span>

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
                                  AdminConsentDescription : Allows the app to have the same access to mailboxes as the signed-in user via Exchange Web Services.
                                  AdminConsentDisplayName : Access mailboxes as the signed-in user via Exchange Web Services
                                  Id                      : 3b5f3d61-589b-4a3c-a359-5dd4b5ee5bd5
                                  IsEnabled               : True
                                  Type                    : User
                                  UserConsentDescription  : Allows the app full access to your mailboxes on your behalf.
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

<span data-ttu-id="eb985-185">Godkännandet initieras när användaren klickar på ”Acceptera”.</span><span class="sxs-lookup"><span data-stu-id="eb985-185">Consent is initiated when the user clicks “Accept”.</span></span> <span data-ttu-id="eb985-186">Först skapas ett ServicePrincipal-objekt för ”FabrikamMail for Office 365” i klientorganisationen.</span><span class="sxs-lookup"><span data-stu-id="eb985-186">First, a ServicePrincipal object for “FabrikamMail for Office 365” is created in the tenant.</span></span> <span data-ttu-id="eb985-187">ServicePrincipal-objektet ser ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="eb985-187">The ServicePrincipal looks something like this:</span></span>

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

<span data-ttu-id="eb985-188">När en app godkänns skapas en Oauth2PermissionGrant-länk mellan följande:</span><span class="sxs-lookup"><span data-stu-id="eb985-188">Consenting to an app creates an Oauth2PermissionGrant link between the following:</span></span>
  
- <span data-ttu-id="eb985-189">användarobjektet</span><span class="sxs-lookup"><span data-stu-id="eb985-189">the user object</span></span>
- <span data-ttu-id="eb985-190">klientapparnas ServicePrincipalName (SPN)</span><span class="sxs-lookup"><span data-stu-id="eb985-190">the client apps ServicePrincipalName (SPN)</span></span>
- <span data-ttu-id="eb985-191">resursapparnas ServicePrincipalName (SPN)</span><span class="sxs-lookup"><span data-stu-id="eb985-191">the resource apps ServicePrincipalName (SPN)</span></span>
- <span data-ttu-id="eb985-192">behörigheter i resursappen.</span><span class="sxs-lookup"><span data-stu-id="eb985-192">permissions in the resource app.</span></span>  

<span data-ttu-id="eb985-193">I exemplet med FabrikamMail ser det ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="eb985-193">In the case of FabrikamMail, it looks something like this:</span></span>

    PS C:\> Get-AzureADUserOAuth2PermissionGrant -ObjectId ddiggle@aadpremiumlab.onmicrosoft.com | fl *
    
    ClientId    : a8b16333-851d-42e8-acd2-eac155849b37
    ConsentType : Principal
    ExpiryTime  : 05/15/2017 07:02:39 AM
    ObjectId    : M2OxqB2F6EKs0urBVYSbN5d7PzhUZz1NlH25COvLocbJjoxkUFfRQauryBKwBWet
    PrincipalId : 648c8ec9-5750-41d1-abab-c812b00567ad
    ResourceId  : 383f7b97-6754-4d3d-9474-3908ebcba1c6
    Scope       : full_access_as_user
    StartTime   : 01/01/0001 12:00:00 AM

<span data-ttu-id="eb985-194">(**ClientId** är ID:t för FabrikamMails ServicePrincipal-objekt (det som precis skapades), **PrincipalId** är ID:t för användarobjektet (för användaren som godkänt) och **ResourceId** är ID:t för Exchanges ServicePrincipal-objekt (Scope är behörigheten i Exchange som godkänts).</span><span class="sxs-lookup"><span data-stu-id="eb985-194">(**ClientId** is FabrikamMail’s service principal object ID (the one that just got created), **PrincipalId** is the user object ID (of the user who consented), **ResourceId** is Exchange’s service principal object ID, Scope is the permission in Exchange that was consented to).</span></span>

<span data-ttu-id="eb985-195">Om användarna inte har tillåtelse att ge sitt godkännande visas en skärm som anger att behörighet krävs.</span><span class="sxs-lookup"><span data-stu-id="eb985-195">If users are not allowed to consent, they will see a screen that says that permission is required.</span></span>

