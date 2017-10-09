---
title: "aaaHow toochoose vilket program skriver toouse när du lägger till ett program | Microsoft Docs"
description: "Förstå hello stöds typer av program som du kan integrera med Azure AD och deras relaterade konfigurationsalternativ"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 46e3672e7f5048b0fa54171f0fc169362c9d5ac6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toochoose-which-application-type-toouse-when-adding-an-application"></a><span data-ttu-id="d7de3-103">Hur toochoose vilket program skriver toouse när du lägger till ett program</span><span class="sxs-lookup"><span data-stu-id="d7de3-103">How toochoose which application type toouse when adding an application</span></span>

<span data-ttu-id="d7de3-104">Den här artikeln hjälpa toounderstand hello fyra typer av program som du kan integrera med Azure AD:</span><span class="sxs-lookup"><span data-stu-id="d7de3-104">This article help you toounderstand hello four main types of applications you can integrate with Azure AD:</span></span>

* <span data-ttu-id="d7de3-105">Vad som stöds av var och en av dem.</span><span class="sxs-lookup"><span data-stu-id="d7de3-105">What is supported by each of them</span></span>
* <span data-ttu-id="d7de3-106">Varför du kan välja vilket program</span><span class="sxs-lookup"><span data-stu-id="d7de3-106">Why you might choose which application</span></span>
* <span data-ttu-id="d7de3-107">Hur tooconfigure huvudegenskaper för dessa program, t.ex. hur användare kan **etableras**, eller vad **enkel inloggning** teknik toouse.</span><span class="sxs-lookup"><span data-stu-id="d7de3-107">How tooconfigure those application’s core properties, like how users are **provisioned**, or what **single sign-on** technology toouse.</span></span>

## <a name="supported-application-types-in-azure-ad"></a><span data-ttu-id="d7de3-108">Typer av program som stöds i Azure AD</span><span class="sxs-lookup"><span data-stu-id="d7de3-108">Supported application types in Azure AD</span></span>

<span data-ttu-id="d7de3-109">Azure AD stöder fyra huvudsakliga programtyper som du kan lägga till med hjälp av hello **Lägg till** funktion hittar du under **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="d7de3-109">Azure AD supports four main application types that you can add using hello **Add** feature found under **Enterprise Applications**.</span></span> <span data-ttu-id="d7de3-110">Exempel på dessa är:</span><span class="sxs-lookup"><span data-stu-id="d7de3-110">These include:</span></span>

-   <span data-ttu-id="d7de3-111">**Azure AD-galleriet program** – ett program som har varit förintegrerade för enkel inloggning med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d7de3-111">**Azure AD Gallery Applications** – An application that has been pre-integrated for single sign-on with Azure AD.</span></span>

-   <span data-ttu-id="d7de3-112">**Application Proxy program** – ett program som körs i din lokala miljö som du vill tooprovide säker enkel inloggning på tooexternally.</span><span class="sxs-lookup"><span data-stu-id="d7de3-112">**Application Proxy Applications** – An application running in your on-premises environment that you want tooprovide secure single-sign on tooexternally.</span></span>

-   <span data-ttu-id="d7de3-113">**Anpassad utvecklat program** – ett program som din organisation vill toodevelop på hello Azure AD plattform, men som kanske inte finns ännu.</span><span class="sxs-lookup"><span data-stu-id="d7de3-113">**Custom-developed Applications** – An application that your organization wishes toodevelop on hello Azure AD Application Development Platform, but that may not exist yet.</span></span>

-   <span data-ttu-id="d7de3-114">**Icke-galleriet program** – ta med egna program!</span><span class="sxs-lookup"><span data-stu-id="d7de3-114">**Non-Gallery Applications** – Bring your own applications!</span></span> <span data-ttu-id="d7de3-115">En länk som du vill eller program som visas med ett användarnamn och lösenord fält har stöd för SAML- eller OpenID Connect protokoll eller stöder SCIM som du önskar toointegrate för enkel inloggning med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d7de3-115">Any web link you want, or any application which renders a username and password field, supports SAML or OpenID Connect protocols, or supports SCIM which you wish toointegrate for single sign-on with Azure AD.</span></span>

## <a name="features-and-capabilities-supported-by-all-hello-above-application-types"></a><span data-ttu-id="d7de3-116">Funktioner som stöds av alla hello ovan programtyper</span><span class="sxs-lookup"><span data-stu-id="d7de3-116">Features and capabilities supported by all hello above application types</span></span>

<span data-ttu-id="d7de3-117">hello följande funktioner som stöds av någon av hello ovan 4 programtyper i Azure AD:</span><span class="sxs-lookup"><span data-stu-id="d7de3-117">hello following features are supported by any of hello above 4 application types in Azure AD:</span></span>

-   <span data-ttu-id="d7de3-118">**Snabbstart** – komma igång med ett program snabbare genom att följa [steg för enkel distribution](https://docs.microsoft.com/azure/active-directory/active-directory-integrating-applications-getting-started)</span><span class="sxs-lookup"><span data-stu-id="d7de3-118">**Quick start** – get going with an application quickly by following [simple deployment steps](https://docs.microsoft.com/azure/active-directory/active-directory-integrating-applications-getting-started)</span></span>

-   <span data-ttu-id="d7de3-119">**Allmänna egenskaper för management** – hämta en [direkt djuplänken](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#deploying-azure-ad-integrated-applications-to-users) tooan programmet [anpassa profilering hello](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-change-app-logo-user-azure-portal) för ett program eller [inaktivera programmet hello](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-disable-app-azure-portal) för alla användare.</span><span class="sxs-lookup"><span data-stu-id="d7de3-119">**General properties management** – get a [direct deeplink](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#deploying-azure-ad-integrated-applications-to-users) tooan application, [customize hello branding](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-change-app-logo-user-azure-portal) of an application, or [disable hello application](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-disable-app-azure-portal) for all users.</span></span>

-   <span data-ttu-id="d7de3-120">**Hantering av användare och grupp** – [tilldela](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) eller [ta bort](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-remove-assignment-azure-portal) användare och grupper tooan program och du kan också tilldela hello specifika programroller har åtkomst till dessa användare och grupper</span><span class="sxs-lookup"><span data-stu-id="d7de3-120">**User and group management** – [assign](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) or [remove](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-remove-assignment-azure-portal) users and groups tooan application, and optionally assign hello specific application roles these users and groups have access to</span></span>

-   <span data-ttu-id="d7de3-121">**Självbetjäning programåtkomst** – aktivera din användare toorequest [självbetjäning programåtkomst](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) tooan programmet från sina programåtkomst paneler antingen genom att lägga till ett program direkt eller [ ansluter till en aktiverad grupp med självbetjäning](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management), kan du alternativt som kräver godkännande för business längs hello sätt</span><span class="sxs-lookup"><span data-stu-id="d7de3-121">**Self-service application access** – enable your users toorequest [self-service application access](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) tooan application from their Application Access Panels either by adding an application directly or [joining a self-service enabled group](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management), optionally requiring business approval along hello way</span></span>

-   <span data-ttu-id="d7de3-122">**Logga in loggar** – Se [alla hello inloggningar tooan programmet](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-sign-ins), eller alla program</span><span class="sxs-lookup"><span data-stu-id="d7de3-122">**Sign-in logs** – see [all hello sign-ins tooan application](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-sign-ins), or all your applications</span></span>

-   <span data-ttu-id="d7de3-123">**Granskningsloggar** – Se [detaljerad granskningsloggar om ändringar tooan programmet](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs), eller tooall dina program</span><span class="sxs-lookup"><span data-stu-id="d7de3-123">**Audit logs** – see [detailed audit logs about modifications tooan application](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs), or tooall your applications</span></span>

-   <span data-ttu-id="d7de3-124">**Villkorlig och risker-baserad åtkomst** – Ställ kraftfulla [villkor-baserade åtkomstregler](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access) som tillämpas när en användare försöker toosign i tooa specifika program</span><span class="sxs-lookup"><span data-stu-id="d7de3-124">**Conditional and risk-based access** – set powerful [condition-based access rules](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access) that are enforced when users attempt toosign in tooa specific application</span></span>

-   <span data-ttu-id="d7de3-125">**Visa behörigheter** – visa alla hello [OAuth2 behörigheter](https://docs.microsoft.com/azure/active-directory/active-directory-apps-permissions-consent) ett program som har åtkomst tooin din katalog från en enda plats</span><span class="sxs-lookup"><span data-stu-id="d7de3-125">**Permissions view** – view any of hello [OAuth2 permissions](https://docs.microsoft.com/azure/active-directory/active-directory-apps-permissions-consent) an application has access tooin your directory from a single place</span></span>

## <a name="single-sign-on-and-provisioning-modes-supported-by-specific-application-types"></a><span data-ttu-id="d7de3-126">Enkel inloggning och provisioning lägen som stöds av specifika programtyper</span><span class="sxs-lookup"><span data-stu-id="d7de3-126">Single sign-on and provisioning modes supported by specific application types</span></span>

<span data-ttu-id="d7de3-127">hello tabellen nedan beskrivs hello olika enkel inloggning och provisioning lägen som stöds av varje hello ovan programtyper.</span><span class="sxs-lookup"><span data-stu-id="d7de3-127">hello table below describes hello different single sign-on and provisioning modes supported by each of hello above application types.</span></span> <span data-ttu-id="d7de3-128">Du kan använda den här tabellen toohelp toounderstand vilket program som du behöver tooadd toosupport ett specifikt mål.</span><span class="sxs-lookup"><span data-stu-id="d7de3-128">You can use this table toohelp you toounderstand which application you need tooadd toosupport a specific goal.</span></span>

  ![Tabell för App-typer](./media/application-tables/table1.png)

## <a name="how-toochoose-a-single-sign-on-mode"></a><span data-ttu-id="d7de3-130">Hur toochoose ett läge för enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d7de3-130">How toochoose a single sign-on mode</span></span>

<span data-ttu-id="d7de3-131">hello stöds **enkel inloggning** lägen för Azure AD-program visas nedan.</span><span class="sxs-lookup"><span data-stu-id="d7de3-131">hello supported **single sign-on** modes for Azure AD applications are listed below.</span></span>

-   <span data-ttu-id="d7de3-132">**Azure AD enkel inloggning inaktiverad** – Välj Azure AD enkel inloggning inaktiverad **läget för enkel inloggning** om du ännu inte klar toointegrate programmet med enkel inloggning med Azure AD, eller bara testar det.</span><span class="sxs-lookup"><span data-stu-id="d7de3-132">**Azure AD single sign-on disabled** – choose Azure AD single sign-on disabled **single sign-on mode** if you are not yet ready toointegrate this application with single sign-on with Azure AD, or are simply testing it out</span></span>

-   <span data-ttu-id="d7de3-133">**Länkad inloggning** – Välj hello [inloggning länkade](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) **läget för enkel inloggning** om du har ett program som redan är kopplad till en befintlig enkel inloggning lösning, eller om du bara vill toopublish en enkel länka för användarna i deras [programmet åtkomstpanelen](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) eller [startprogrammet för Office 365](https://login.microsoftonline.com/common/oauth2/authorize?response_mode=form_post&response_type=id_token&scope=openid&nonce=d508a995-f6d6-4b8a-81b8-825c71f1be46.636253878097046923&state=https%3a%2f%2fsupport.office.com%2farticle%2fMeet-the-Office-365-app-launcher-79f12104-6fed-442f-96a0-eb089a3f476a%3fui%3den-US%26rs%3den-US%26ad%3dUS&client_id=4b233688-031c-404b-9a80-a4f3f2351f90&redirect_uri=https%3a%2f%2fsupport.office.com%2fauth%2fsignin&login_hint=asteen%40microsoft.com&prompt=none)</span><span class="sxs-lookup"><span data-stu-id="d7de3-133">**Linked Sign-on** – choose hello [Linked Sign-on](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) **single sign-on mode** if you have an application that is already connected with an existing single sign-on solution, or if you just want toopublish a simple link for your users in their [Application Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) or [Office 365 application launcher](https://login.microsoftonline.com/common/oauth2/authorize?response_mode=form_post&response_type=id_token&scope=openid&nonce=d508a995-f6d6-4b8a-81b8-825c71f1be46.636253878097046923&state=https%3a%2f%2fsupport.office.com%2farticle%2fMeet-the-Office-365-app-launcher-79f12104-6fed-442f-96a0-eb089a3f476a%3fui%3den-US%26rs%3den-US%26ad%3dUS&client_id=4b233688-031c-404b-9a80-a4f3f2351f90&redirect_uri=https%3a%2f%2fsupport.office.com%2fauth%2fsignin&login_hint=asteen%40microsoft.com&prompt=none)</span></span>

-   <span data-ttu-id="d7de3-134">**Lösenordsbaserade inloggning** – Välj hello [lösenordsbaserade inloggning](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) **läget för enkel inloggning** om ditt program visas med ett HTML-fält för användarnamn och lösenord och du vill toostore som användarnamn och lösenord på ett säkert sätt toobe spelas toohello programmet senare</span><span class="sxs-lookup"><span data-stu-id="d7de3-134">**Password-based Sign-on** – choose hello [Password-based Sign-on](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) **single sign-on mode** if your application renders an HTML username and password field and you want toostore that username and password securely toobe replayed toohello application later</span></span>

-   <span data-ttu-id="d7de3-135">**SAML-baserade inloggning** – Välj hello [SAML-baserade inloggning](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) enkel inloggning på läge om programmet stöder hello SAML eller OpenID Connect protokoll eller om du vill toobe kan toomap användare toospecific programroller baserat regler som du definierar i din SAML anspråk *</span><span class="sxs-lookup"><span data-stu-id="d7de3-135">**SAML-based Sign-on** – choose hello [SAML-based Sign-on](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) single-sign on mode if your application supports hello SAML or OpenID Connect protocols, or you want toobe able toomap users toospecific application roles based on rules you define in your SAML claims *</span></span>

   >[!NOTE]
   ><span data-ttu-id="d7de3-136">Det här alternativet är inte tillgängligt när hello programproxy konfigureras för ett program.</span><span class="sxs-lookup"><span data-stu-id="d7de3-136">This option is not available when hello application proxy is configured for an application.</span></span>
   >
   >

-   <span data-ttu-id="d7de3-137">**Rubrik-baserade inloggning** – Välj det här [huvud-baserade inloggning](https://docs.microsoft.com/azure/active-directory/application-proxy-ping-access#what-is-pingaccess-for-azure-ad) enkel inloggning läge om du har ett program med PingAccess som stöder HTTP-huvudet baseras autentisering som du vill att tooperform enkel inloggning på för</span><span class="sxs-lookup"><span data-stu-id="d7de3-137">**Header-based Sign-on** – choose this [Header-based Sign-on](https://docs.microsoft.com/azure/active-directory/application-proxy-ping-access#what-is-pingaccess-for-azure-ad) single sign-on mode if you have an application using PingAccess that supports HTTP-header based authentication that you wish tooperform single-sign on too</span></span>

   >[!NOTE]
   ><span data-ttu-id="d7de3-138">Det här alternativet är bara tillgänglig när hello programproxy och PingAccess konfigureras för ett program.</span><span class="sxs-lookup"><span data-stu-id="d7de3-138">This option is only available when hello application proxy and PingAccess is configured for an application.</span></span>
   >
   >

-   <span data-ttu-id="d7de3-139">**Integrerad Windows-autentisering** – Välj hello [integrerad Windows-autentisering](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-sso-using-kcd) enkel inloggning på läge när exponera ett lokalt WIA program som du vill att tooperform enkel inloggning på för</span><span class="sxs-lookup"><span data-stu-id="d7de3-139">**Integrated Windows Authentication** – choose hello [Integrated Windows Authentication](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-sso-using-kcd) single-sign on mode when exposing an on-premises WIA application that you wish tooperform single-sign on too</span></span>

   >[!NOTE]
   ><span data-ttu-id="d7de3-140">Det här alternativet är bara tillgänglig när hello programproxy konfigureras för ett program.</span><span class="sxs-lookup"><span data-stu-id="d7de3-140">This option is only available when hello application proxy is configured for an application.</span></span>
   >
   >

## <a name="single-sign-on-modes-for-custom-developed-applications"></a><span data-ttu-id="d7de3-141">Enkel inloggning lägen för anpassad utvecklade program</span><span class="sxs-lookup"><span data-stu-id="d7de3-141">Single sign-on modes for custom-developed applications</span></span>

<span data-ttu-id="d7de3-142">Program som du har anpassat utvecklas genom hello [anpassad utvecklat program](#_Custom-Developed_Applications) upplevelse stöder också ytterligare enkel inloggning lägen inte listas här ovan.</span><span class="sxs-lookup"><span data-stu-id="d7de3-142">Applications you have custom developed through hello [Custom-developed application](#_Custom-Developed_Applications) experience also support additional single sign-on modes not listed above.</span></span> <span data-ttu-id="d7de3-143">Exempel på dessa är:</span><span class="sxs-lookup"><span data-stu-id="d7de3-143">These include:</span></span>

-   <span data-ttu-id="d7de3-144">[OAuth 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code) baserat inloggning</span><span class="sxs-lookup"><span data-stu-id="d7de3-144">[OAuth 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code) based sign-on</span></span>

-   <span data-ttu-id="d7de3-145">[OpenID Connect 1.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-openid-connect-code) baserat inloggning</span><span class="sxs-lookup"><span data-stu-id="d7de3-145">[OpenID Connect 1.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-openid-connect-code) based sign-on</span></span>

-   <span data-ttu-id="d7de3-146">[WS-Federation 1.2](http://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html) baserat inloggning</span><span class="sxs-lookup"><span data-stu-id="d7de3-146">[WS-Federation 1.2](http://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html) based sign-on</span></span>

-   <span data-ttu-id="d7de3-147">[SAML 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-protocol-reference) baserat inloggning</span><span class="sxs-lookup"><span data-stu-id="d7de3-147">[SAML 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-protocol-reference) based sign-on</span></span>

<span data-ttu-id="d7de3-148">Läs hello [Utvecklarhandbok för Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide) toolearn mer om hur toocreate en anpassad utvecklade program som stöder dessa enkel inloggning lägen.</span><span class="sxs-lookup"><span data-stu-id="d7de3-148">Read hello [Azure Active Directory developer’s guide](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide) toolearn more about how toocreate a custom-developed application which supports these single sign-on modes.</span></span>

## <a name="how-tooset-an-applications-single-sign-on-mode"></a><span data-ttu-id="d7de3-149">Hur tooset ett program har enkel inloggning läge</span><span class="sxs-lookup"><span data-stu-id="d7de3-149">How tooset an application’s single sign-on mode</span></span>

<span data-ttu-id="d7de3-150">tooset ett program **enkel inloggning** läge, följ hello instruktionerna nedan:</span><span class="sxs-lookup"><span data-stu-id="d7de3-150">tooset an application’s **single sign-on** mode, follow hello instructions below:</span></span>

1.  <span data-ttu-id="d7de3-151">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**</span><span class="sxs-lookup"><span data-stu-id="d7de3-151">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="d7de3-152">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="d7de3-152">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="d7de3-153">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="d7de3-153">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="d7de3-154">Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="d7de3-154">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="d7de3-155">Klicka på **alla program** tooview en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="d7de3-155">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="d7de3-156">Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**</span><span class="sxs-lookup"><span data-stu-id="d7de3-156">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="d7de3-157">Välj hello-program som du vill tooconfigure enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="d7de3-157">Select hello application for which you want tooconfigure single sign-on.</span></span>

7.  <span data-ttu-id="d7de3-158">När programmet hello läses in klickar du på **enkel inloggning** från hello programmet vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="d7de3-158">Once hello application loads, click **Single sign-on** from hello application’s left hand navigation menu.</span></span>

## <a name="how-toochoose-a-provisioning-mode"></a><span data-ttu-id="d7de3-159">Hur toochoose ett Etableringsläge</span><span class="sxs-lookup"><span data-stu-id="d7de3-159">How toochoose a provisioning mode</span></span>

-   <span data-ttu-id="d7de3-160">**Manuell etablering** – Välj hello [manuell](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-manage-provisioning#provisioning-modes) Etableringsläge om du har befintliga konton eller vill toomanage konton för det här programmet utanför Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d7de3-160">**Manual Provisioning** – choose hello [Manual](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-manage-provisioning#provisioning-modes) provisioning mode if you have existing accounts, or wish toomanage accounts for this application outside of Azure AD.</span></span>

-   <span data-ttu-id="d7de3-161">**Automatisk etablering** – Välj hello [automatisk](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-manage-provisioning#configuring-automatic-user-account-provisioning) **Etableringsläge** om du vill tooenable automatisk API-baserad etablering och/eller ta bort etableringen av användarkonton toothis programmet</span><span class="sxs-lookup"><span data-stu-id="d7de3-161">**Automatic Provisioning** – choose hello [Automatic](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-manage-provisioning#configuring-automatic-user-account-provisioning) **provisioning mode** if you want tooenable automatic API-based provisioning and/or de-provisioning of user accounts toothis application</span></span> 

   >[!NOTE]
   ><span data-ttu-id="d7de3-162">Det här alternativet är bara tillgängligt för applikationer inom hello **aktuella** kategori av hello [Azure AD Application Gallery](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-whats-new-azure-portal#the-new-and-improved-application-gallery).</span><span class="sxs-lookup"><span data-stu-id="d7de3-162">This option is available only for applications within hello **featured** category of hello [Azure AD Application Gallery](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-whats-new-azure-portal#the-new-and-improved-application-gallery).</span></span>
   >
   >

-   <span data-ttu-id="d7de3-163">**SCIM-baserade Automatisk etablering** – använda [SCIM-baserade Automatisk etablering](https://docs.microsoft.com/azure/active-directory/active-directory-scim-provisioning) om programmet stöder hello SCIM protokoll för att upptäcka ändringar toousers och grupper som släpps automatiskt efter ändringar tooany program som är integrerade med Azure AD</span><span class="sxs-lookup"><span data-stu-id="d7de3-163">**SCIM-based Automatic Provisioning** – use [SCIM-based Automatic Provisioning](https://docs.microsoft.com/azure/active-directory/active-directory-scim-provisioning) if your application supports hello SCIM protocol for detecting changes toousers and groups, which are automatically emitted for changes tooany application integrated with Azure AD</span></span> 

   >[!NOTE]
   ><span data-ttu-id="d7de3-164">Det här alternativet anges inte som en specifik Etableringsläge, men är aktiverad som standard för alla program som är integrerade med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d7de3-164">This option is not listed as a specific provisioning mode, but is enabled by default for all applications that are integrated with Azure AD.</span></span>
   >
   >

## <a name="how-tooset-an-applications-provisioning-mode"></a><span data-ttu-id="d7de3-165">Hur tooset ett program Etableringsläge</span><span class="sxs-lookup"><span data-stu-id="d7de3-165">How tooset an application’s provisioning mode</span></span>

<span data-ttu-id="d7de3-166">tooset ett program **etablering** läge, följ hello instruktionerna nedan:</span><span class="sxs-lookup"><span data-stu-id="d7de3-166">tooset an application’s **provisioning** mode, follow hello instructions below:</span></span>

<span data-ttu-id="d7de3-167">tooset ett program **enkel inloggning** läge, följ hello instruktionerna nedan:</span><span class="sxs-lookup"><span data-stu-id="d7de3-167">tooset an application’s **single sign-on** mode, follow hello instructions below:</span></span>

1.  <span data-ttu-id="d7de3-168">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**</span><span class="sxs-lookup"><span data-stu-id="d7de3-168">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="d7de3-169">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="d7de3-169">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="d7de3-170">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="d7de3-170">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="d7de3-171">Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="d7de3-171">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="d7de3-172">Klicka på **alla program** tooview en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="d7de3-172">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="d7de3-173">Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**</span><span class="sxs-lookup"><span data-stu-id="d7de3-173">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="d7de3-174">Välj hello-program som du vill tooconfigure etablering.</span><span class="sxs-lookup"><span data-stu-id="d7de3-174">Select hello application for which you want tooconfigure provisioning.</span></span>

7.  <span data-ttu-id="d7de3-175">När programmet hello läses in klickar du på **etablering** från hello programmet vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="d7de3-175">Once hello application loads, click **Provisioning** from hello application’s left hand navigation menu.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d7de3-176">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d7de3-176">Next steps</span></span>
[<span data-ttu-id="d7de3-177">Hantera program med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="d7de3-177">Managing Applications with Azure Active Directory</span></span>](active-directory-enable-sso-scenario.md)
