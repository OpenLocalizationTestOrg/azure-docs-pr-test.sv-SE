---
title: "Så här väljer du vilken typ som programmet ska använda när du lägger till ett program | Microsoft Docs"
description: "Förstå typerna som stöds av program som du kan integrera med Azure AD och deras relaterade konfigurationsalternativ"
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
ms.openlocfilehash: e0d41d1933531c2c633613bcbc1bbcbf075d6a69
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-choose-which-application-type-to-use-when-adding-an-application"></a><span data-ttu-id="16c93-103">Så här väljer du vilken typ som programmet ska använda när du lägger till ett program</span><span class="sxs-lookup"><span data-stu-id="16c93-103">How to choose which application type to use when adding an application</span></span>

<span data-ttu-id="16c93-104">Den här artikeln hjälper dig att förstå de fyra huvudsakliga typerna av program som du kan integrera med Azure AD:</span><span class="sxs-lookup"><span data-stu-id="16c93-104">This article help you to understand the four main types of applications you can integrate with Azure AD:</span></span>

* <span data-ttu-id="16c93-105">Vad som stöds av var och en av dem.</span><span class="sxs-lookup"><span data-stu-id="16c93-105">What is supported by each of them</span></span>
* <span data-ttu-id="16c93-106">Varför du kan välja vilket program</span><span class="sxs-lookup"><span data-stu-id="16c93-106">Why you might choose which application</span></span>
* <span data-ttu-id="16c93-107">Hur du konfigurerar dessa program core egenskaper, t.ex. hur användare kan **etableras**, eller vad **enkel inloggning** teknik som ska användas.</span><span class="sxs-lookup"><span data-stu-id="16c93-107">How to configure those application’s core properties, like how users are **provisioned**, or what **single sign-on** technology to use.</span></span>

## <a name="supported-application-types-in-azure-ad"></a><span data-ttu-id="16c93-108">Typer av program som stöds i Azure AD</span><span class="sxs-lookup"><span data-stu-id="16c93-108">Supported application types in Azure AD</span></span>

<span data-ttu-id="16c93-109">Azure AD stöder fyra huvudsakliga programtyper som du kan lägga till med hjälp av den **Lägg till** funktion hittar du under **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="16c93-109">Azure AD supports four main application types that you can add using the **Add** feature found under **Enterprise Applications**.</span></span> <span data-ttu-id="16c93-110">Exempel på dessa är:</span><span class="sxs-lookup"><span data-stu-id="16c93-110">These include:</span></span>

-   <span data-ttu-id="16c93-111">**Azure AD-galleriet program** – ett program som har varit förintegrerade för enkel inloggning med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="16c93-111">**Azure AD Gallery Applications** – An application that has been pre-integrated for single sign-on with Azure AD.</span></span>

-   <span data-ttu-id="16c93-112">**Application Proxy program** – ett program som körs i din lokala miljö som du vill ge säker enkel inloggning till externt.</span><span class="sxs-lookup"><span data-stu-id="16c93-112">**Application Proxy Applications** – An application running in your on-premises environment that you want to provide secure single-sign on to externally.</span></span>

-   <span data-ttu-id="16c93-113">**Anpassad utvecklat program** – ett program som din organisation vill utveckla på den Azure AD plattform, men som kanske inte finns ännu.</span><span class="sxs-lookup"><span data-stu-id="16c93-113">**Custom-developed Applications** – An application that your organization wishes to develop on the Azure AD Application Development Platform, but that may not exist yet.</span></span>

-   <span data-ttu-id="16c93-114">**Icke-galleriet program** – ta med egna program!</span><span class="sxs-lookup"><span data-stu-id="16c93-114">**Non-Gallery Applications** – Bring your own applications!</span></span> <span data-ttu-id="16c93-115">En länk som du vill eller program som visas med ett användarnamn och lösenord fält har stöd för SAML- eller OpenID Connect protokoll eller stöder SCIM som du vill integrera för enkel inloggning med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="16c93-115">Any web link you want, or any application which renders a username and password field, supports SAML or OpenID Connect protocols, or supports SCIM which you wish to integrate for single sign-on with Azure AD.</span></span>

## <a name="features-and-capabilities-supported-by-all-the-above-application-types"></a><span data-ttu-id="16c93-116">Funktioner som stöds av de ovanstående programtyperna</span><span class="sxs-lookup"><span data-stu-id="16c93-116">Features and capabilities supported by all the above application types</span></span>

<span data-ttu-id="16c93-117">Följande funktioner stöds av något av ovanstående 4 programtyper i Azure AD:</span><span class="sxs-lookup"><span data-stu-id="16c93-117">The following features are supported by any of the above 4 application types in Azure AD:</span></span>

-   <span data-ttu-id="16c93-118">**Snabbstart** – komma igång med ett program snabbare genom att följa [steg för enkel distribution](https://docs.microsoft.com/azure/active-directory/active-directory-integrating-applications-getting-started)</span><span class="sxs-lookup"><span data-stu-id="16c93-118">**Quick start** – get going with an application quickly by following [simple deployment steps](https://docs.microsoft.com/azure/active-directory/active-directory-integrating-applications-getting-started)</span></span>

-   <span data-ttu-id="16c93-119">**Allmänna egenskaper för management** – hämta en [direkt djuplänken](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#deploying-azure-ad-integrated-applications-to-users) till ett program [anpassa](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-change-app-logo-user-azure-portal) för ett program eller [inaktivera programmet](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-disable-app-azure-portal)för alla användare.</span><span class="sxs-lookup"><span data-stu-id="16c93-119">**General properties management** – get a [direct deeplink](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#deploying-azure-ad-integrated-applications-to-users) to an application, [customize the branding](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-change-app-logo-user-azure-portal) of an application, or [disable the application](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-disable-app-azure-portal) for all users.</span></span>

-   <span data-ttu-id="16c93-120">**Hantering av användare och grupp** – [tilldela](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) eller [ta bort](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-remove-assignment-azure-portal) användare och grupper till ett program, och du kan också tilldela programroller för specifika dessa användare och grupper har åtkomst till</span><span class="sxs-lookup"><span data-stu-id="16c93-120">**User and group management** – [assign](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) or [remove](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-remove-assignment-azure-portal) users and groups to an application, and optionally assign the specific application roles these users and groups have access to</span></span>

-   <span data-ttu-id="16c93-121">**Självbetjäning programåtkomst** – att användarna kan begära [självbetjäning programåtkomst](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) till ett program från sina program åtkomst paneler antingen genom att lägga till ett program direkt eller [ ansluter till en aktiverad grupp med självbetjäning](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management), kan du alternativt som kräver godkännande av företag på vägen</span><span class="sxs-lookup"><span data-stu-id="16c93-121">**Self-service application access** – enable your users to request [self-service application access](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) to an application from their Application Access Panels either by adding an application directly or [joining a self-service enabled group](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management), optionally requiring business approval along the way</span></span>

-   <span data-ttu-id="16c93-122">**Logga in loggar** – Se [alla inloggningarna till ett program](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-sign-ins), eller alla program</span><span class="sxs-lookup"><span data-stu-id="16c93-122">**Sign-in logs** – see [all the sign-ins to an application](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-sign-ins), or all your applications</span></span>

-   <span data-ttu-id="16c93-123">**Granskningsloggar** – Se [detaljerad granskningsloggar om ändringar i ett program](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs), eller för alla program</span><span class="sxs-lookup"><span data-stu-id="16c93-123">**Audit logs** – see [detailed audit logs about modifications to an application](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs), or to all your applications</span></span>

-   <span data-ttu-id="16c93-124">**Villkorlig och risker-baserad åtkomst** – Ställ kraftfulla [villkor-baserade åtkomstregler](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access) som tillämpas när användarna försöker logga in på ett visst program</span><span class="sxs-lookup"><span data-stu-id="16c93-124">**Conditional and risk-based access** – set powerful [condition-based access rules](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access) that are enforced when users attempt to sign in to a specific application</span></span>

-   <span data-ttu-id="16c93-125">**Visa behörigheter** – ta del av den [OAuth2 behörigheter](https://docs.microsoft.com/azure/active-directory/active-directory-apps-permissions-consent) ett program har åtkomst till i din katalog från en enda placera</span><span class="sxs-lookup"><span data-stu-id="16c93-125">**Permissions view** – view any of the [OAuth2 permissions](https://docs.microsoft.com/azure/active-directory/active-directory-apps-permissions-consent) an application has access to in your directory from a single place</span></span>

## <a name="single-sign-on-and-provisioning-modes-supported-by-specific-application-types"></a><span data-ttu-id="16c93-126">Enkel inloggning och provisioning lägen som stöds av specifika programtyper</span><span class="sxs-lookup"><span data-stu-id="16c93-126">Single sign-on and provisioning modes supported by specific application types</span></span>

<span data-ttu-id="16c93-127">I tabellen nedan beskrivs olika enkel inloggning och allokering lägen som stöds av varje ovan programtyper.</span><span class="sxs-lookup"><span data-stu-id="16c93-127">The table below describes the different single sign-on and provisioning modes supported by each of the above application types.</span></span> <span data-ttu-id="16c93-128">Du kan använda den här tabellen hjälper dig att förstå vilket program som du behöver lägga till stöd för ett specifikt mål.</span><span class="sxs-lookup"><span data-stu-id="16c93-128">You can use this table to help you to understand which application you need to add to support a specific goal.</span></span>

  ![Tabell för App-typer](./media/application-tables/table1.png)

## <a name="how-to-choose-a-single-sign-on-mode"></a><span data-ttu-id="16c93-130">Hur du väljer ett läge för enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="16c93-130">How to choose a single sign-on mode</span></span>

<span data-ttu-id="16c93-131">Den stöds **enkel inloggning** lägen för Azure AD-program visas nedan.</span><span class="sxs-lookup"><span data-stu-id="16c93-131">The supported **single sign-on** modes for Azure AD applications are listed below.</span></span>

-   <span data-ttu-id="16c93-132">**Azure AD enkel inloggning inaktiverat** – Välj Azure AD enkel inloggning inaktiverad **läget för enkel inloggning** om du ännu inte är redo att integrera programmet med enkel inloggning med Azure AD eller bara testar det.</span><span class="sxs-lookup"><span data-stu-id="16c93-132">**Azure AD single sign-on disabled** – choose Azure AD single sign-on disabled **single sign-on mode** if you are not yet ready to integrate this application with single sign-on with Azure AD, or are simply testing it out</span></span>

-   <span data-ttu-id="16c93-133">**Länkad inloggning** – Välj den [inloggning länkade](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) **läget för enkel inloggning** om du har ett program som redan är kopplad till en befintlig enkel inloggning lösning eller om du vill publicera en enkel länk för användarna i deras [programmet åtkomstpanelen](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) eller [startprogrammet för Office 365](https://login.microsoftonline.com/common/oauth2/authorize?response_mode=form_post&response_type=id_token&scope=openid&nonce=d508a995-f6d6-4b8a-81b8-825c71f1be46.636253878097046923&state=https%3a%2f%2fsupport.office.com%2farticle%2fMeet-the-Office-365-app-launcher-79f12104-6fed-442f-96a0-eb089a3f476a%3fui%3den-US%26rs%3den-US%26ad%3dUS&client_id=4b233688-031c-404b-9a80-a4f3f2351f90&redirect_uri=https%3a%2f%2fsupport.office.com%2fauth%2fsignin&login_hint=asteen%40microsoft.com&prompt=none)</span><span class="sxs-lookup"><span data-stu-id="16c93-133">**Linked Sign-on** – choose the [Linked Sign-on](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) **single sign-on mode** if you have an application that is already connected with an existing single sign-on solution, or if you just want to publish a simple link for your users in their [Application Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) or [Office 365 application launcher](https://login.microsoftonline.com/common/oauth2/authorize?response_mode=form_post&response_type=id_token&scope=openid&nonce=d508a995-f6d6-4b8a-81b8-825c71f1be46.636253878097046923&state=https%3a%2f%2fsupport.office.com%2farticle%2fMeet-the-Office-365-app-launcher-79f12104-6fed-442f-96a0-eb089a3f476a%3fui%3den-US%26rs%3den-US%26ad%3dUS&client_id=4b233688-031c-404b-9a80-a4f3f2351f90&redirect_uri=https%3a%2f%2fsupport.office.com%2fauth%2fsignin&login_hint=asteen%40microsoft.com&prompt=none)</span></span>

-   <span data-ttu-id="16c93-134">**Lösenordsbaserade inloggning** – Välj den [lösenordsbaserade inloggning](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) **läget för enkel inloggning** om ditt program visas med ett HTML-fält för användarnamn och lösenord och du vill lagra det användarnamn och lösenord på ett säkert sätt ska återupprepas till programmet senare</span><span class="sxs-lookup"><span data-stu-id="16c93-134">**Password-based Sign-on** – choose the [Password-based Sign-on](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) **single sign-on mode** if your application renders an HTML username and password field and you want to store that username and password securely to be replayed to the application later</span></span>

-   <span data-ttu-id="16c93-135">**SAML-baserade inloggning** – Välj den [SAML-baserade inloggning](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) enkel inloggning på läge om programmet stöder SAML- eller OpenID Connect-protokoll eller om du vill kunna mappa användare till programroller för specifika baserat på regler Du definierar i SAML-anspråk *</span><span class="sxs-lookup"><span data-stu-id="16c93-135">**SAML-based Sign-on** – choose the [SAML-based Sign-on](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) single-sign on mode if your application supports the SAML or OpenID Connect protocols, or you want to be able to map users to specific application roles based on rules you define in your SAML claims *</span></span>

   >[!NOTE]
   ><span data-ttu-id="16c93-136">Det här alternativet är inte tillgängligt när application proxy har konfigurerats för ett program.</span><span class="sxs-lookup"><span data-stu-id="16c93-136">This option is not available when the application proxy is configured for an application.</span></span>
   >
   >

-   <span data-ttu-id="16c93-137">**Rubrik-baserade inloggning** – Välj det här [huvud-baserade inloggning](https://docs.microsoft.com/azure/active-directory/application-proxy-ping-access#what-is-pingaccess-for-azure-ad) enkel inloggning läge om du har ett program med PingAccess som stöder HTTP-huvudet baseras autentisering som du vill utföra enkel inloggning till</span><span class="sxs-lookup"><span data-stu-id="16c93-137">**Header-based Sign-on** – choose this [Header-based Sign-on](https://docs.microsoft.com/azure/active-directory/application-proxy-ping-access#what-is-pingaccess-for-azure-ad) single sign-on mode if you have an application using PingAccess that supports HTTP-header based authentication that you wish to perform single-sign on to</span></span> 

   >[!NOTE]
   ><span data-ttu-id="16c93-138">Det här alternativet är bara tillgänglig när programproxy och PingAccess konfigureras för ett program.</span><span class="sxs-lookup"><span data-stu-id="16c93-138">This option is only available when the application proxy and PingAccess is configured for an application.</span></span>
   >
   >

-   <span data-ttu-id="16c93-139">**Integrerad Windows-autentisering** – Välj den [integrerad Windows-autentisering](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-sso-using-kcd) enkel inloggning på läge när exponera ett lokalt WIA program som du vill utföra enkel inloggning till</span><span class="sxs-lookup"><span data-stu-id="16c93-139">**Integrated Windows Authentication** – choose the [Integrated Windows Authentication](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-sso-using-kcd) single-sign on mode when exposing an on-premises WIA application that you wish to perform single-sign on to</span></span> 

   >[!NOTE]
   ><span data-ttu-id="16c93-140">Det här alternativet är bara tillgänglig när application proxy har konfigurerats för ett program.</span><span class="sxs-lookup"><span data-stu-id="16c93-140">This option is only available when the application proxy is configured for an application.</span></span>
   >
   >

## <a name="single-sign-on-modes-for-custom-developed-applications"></a><span data-ttu-id="16c93-141">Enkel inloggning lägen för anpassad utvecklade program</span><span class="sxs-lookup"><span data-stu-id="16c93-141">Single sign-on modes for custom-developed applications</span></span>

<span data-ttu-id="16c93-142">Program som du har anpassat utvecklas genom den [anpassad utvecklat program](#_Custom-Developed_Applications) upplevelse stöder också ytterligare enkel inloggning lägen inte listas här ovan.</span><span class="sxs-lookup"><span data-stu-id="16c93-142">Applications you have custom developed through the [Custom-developed application](#_Custom-Developed_Applications) experience also support additional single sign-on modes not listed above.</span></span> <span data-ttu-id="16c93-143">Exempel på dessa är:</span><span class="sxs-lookup"><span data-stu-id="16c93-143">These include:</span></span>

-   <span data-ttu-id="16c93-144">[OAuth 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code) baserat inloggning</span><span class="sxs-lookup"><span data-stu-id="16c93-144">[OAuth 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code) based sign-on</span></span>

-   <span data-ttu-id="16c93-145">[OpenID Connect 1.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-openid-connect-code) baserat inloggning</span><span class="sxs-lookup"><span data-stu-id="16c93-145">[OpenID Connect 1.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-openid-connect-code) based sign-on</span></span>

-   <span data-ttu-id="16c93-146">[WS-Federation 1.2](http://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html) baserat inloggning</span><span class="sxs-lookup"><span data-stu-id="16c93-146">[WS-Federation 1.2](http://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html) based sign-on</span></span>

-   <span data-ttu-id="16c93-147">[SAML 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-protocol-reference) baserat inloggning</span><span class="sxs-lookup"><span data-stu-id="16c93-147">[SAML 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-protocol-reference) based sign-on</span></span>

<span data-ttu-id="16c93-148">Läs den [Utvecklarhandbok för Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide) vill veta mer om hur du skapar en anpassad utvecklat program som stöder dessa lägen för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="16c93-148">Read the [Azure Active Directory developer’s guide](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide) to learn more about how to create a custom-developed application which supports these single sign-on modes.</span></span>

## <a name="how-to-set-an-applications-single-sign-on-mode"></a><span data-ttu-id="16c93-149">Hur du ställer in ett program enkel inloggning läge</span><span class="sxs-lookup"><span data-stu-id="16c93-149">How to set an application’s single sign-on mode</span></span>

<span data-ttu-id="16c93-150">Så här anger du ett program **enkel inloggning** läge, följ instruktionerna nedan:</span><span class="sxs-lookup"><span data-stu-id="16c93-150">To set an application’s **single sign-on** mode, follow the instructions below:</span></span>

1.  <span data-ttu-id="16c93-151">Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**</span><span class="sxs-lookup"><span data-stu-id="16c93-151">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="16c93-152">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="16c93-152">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="16c93-153">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="16c93-153">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="16c93-154">Klicka på **företagsprogram** från Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="16c93-154">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="16c93-155">Klicka på **alla program** att visa en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="16c93-155">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="16c93-156">Om du inte ser programmet som du vill visa här använder du den **Filter** kontrollen längst upp i den **listan med alla program** och ange den **visa** att **alla program.**</span><span class="sxs-lookup"><span data-stu-id="16c93-156">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="16c93-157">Välj det program som du vill konfigurera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="16c93-157">Select the application for which you want to configure single sign-on.</span></span>

7.  <span data-ttu-id="16c93-158">När programmet läses in klickar du på **enkel inloggning** från programmets vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="16c93-158">Once the application loads, click **Single sign-on** from the application’s left hand navigation menu.</span></span>

## <a name="how-to-choose-a-provisioning-mode"></a><span data-ttu-id="16c93-159">Så här väljer du ett Etableringsläge</span><span class="sxs-lookup"><span data-stu-id="16c93-159">How to choose a provisioning mode</span></span>

-   <span data-ttu-id="16c93-160">**Manuell etablering** – Välj den [manuell](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-manage-provisioning#provisioning-modes) Etableringsläge om du har befintliga konton eller om du vill hantera konton för det här programmet utanför Azure AD.</span><span class="sxs-lookup"><span data-stu-id="16c93-160">**Manual Provisioning** – choose the [Manual](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-manage-provisioning#provisioning-modes) provisioning mode if you have existing accounts, or wish to manage accounts for this application outside of Azure AD.</span></span>

-   <span data-ttu-id="16c93-161">**Automatisk etablering** – Välj den [automatisk](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-manage-provisioning#configuring-automatic-user-account-provisioning) **Etableringsläge** om du vill aktivera automatisk API-baserad etablering och avetablering av användarkonton till den här programmet</span><span class="sxs-lookup"><span data-stu-id="16c93-161">**Automatic Provisioning** – choose the [Automatic](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-manage-provisioning#configuring-automatic-user-account-provisioning) **provisioning mode** if you want to enable automatic API-based provisioning and/or de-provisioning of user accounts to this application</span></span> 

   >[!NOTE]
   ><span data-ttu-id="16c93-162">Det här alternativet är bara tillgängligt för program i den **aktuella** kategorin för den [Azure AD Application Gallery](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-whats-new-azure-portal#the-new-and-improved-application-gallery).</span><span class="sxs-lookup"><span data-stu-id="16c93-162">This option is available only for applications within the **featured** category of the [Azure AD Application Gallery](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-whats-new-azure-portal#the-new-and-improved-application-gallery).</span></span>
   >
   >

-   <span data-ttu-id="16c93-163">**SCIM-baserade Automatisk etablering** – använda [SCIM-baserade Automatisk etablering](https://docs.microsoft.com/azure/active-directory/active-directory-scim-provisioning) om programmet stöder SCIM-protokollet för att upptäcka ändringar av användare och grupper som släpps automatiskt för att ändringarna ska alla program som är integrerade med Azure AD</span><span class="sxs-lookup"><span data-stu-id="16c93-163">**SCIM-based Automatic Provisioning** – use [SCIM-based Automatic Provisioning](https://docs.microsoft.com/azure/active-directory/active-directory-scim-provisioning) if your application supports the SCIM protocol for detecting changes to users and groups, which are automatically emitted for changes to any application integrated with Azure AD</span></span> 

   >[!NOTE]
   ><span data-ttu-id="16c93-164">Det här alternativet anges inte som en specifik Etableringsläge, men är aktiverad som standard för alla program som är integrerade med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="16c93-164">This option is not listed as a specific provisioning mode, but is enabled by default for all applications that are integrated with Azure AD.</span></span>
   >
   >

## <a name="how-to-set-an-applications-provisioning-mode"></a><span data-ttu-id="16c93-165">Etableringsläge för hur du ställer in ett program</span><span class="sxs-lookup"><span data-stu-id="16c93-165">How to set an application’s provisioning mode</span></span>

<span data-ttu-id="16c93-166">Så här anger du ett program **etablering** läge, följ instruktionerna nedan:</span><span class="sxs-lookup"><span data-stu-id="16c93-166">To set an application’s **provisioning** mode, follow the instructions below:</span></span>

<span data-ttu-id="16c93-167">Så här anger du ett program **enkel inloggning** läge, följ instruktionerna nedan:</span><span class="sxs-lookup"><span data-stu-id="16c93-167">To set an application’s **single sign-on** mode, follow the instructions below:</span></span>

1.  <span data-ttu-id="16c93-168">Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**</span><span class="sxs-lookup"><span data-stu-id="16c93-168">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="16c93-169">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="16c93-169">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="16c93-170">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="16c93-170">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="16c93-171">Klicka på **företagsprogram** från Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="16c93-171">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="16c93-172">Klicka på **alla program** att visa en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="16c93-172">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="16c93-173">Om du inte ser programmet som du vill visa här använder du den **Filter** kontrollen längst upp i den **listan med alla program** och ange den **visa** att **alla program.**</span><span class="sxs-lookup"><span data-stu-id="16c93-173">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="16c93-174">Välj det program som du vill konfigurera etablering.</span><span class="sxs-lookup"><span data-stu-id="16c93-174">Select the application for which you want to configure provisioning.</span></span>

7.  <span data-ttu-id="16c93-175">När programmet läses in klickar du på **etablering** från programmets vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="16c93-175">Once the application loads, click **Provisioning** from the application’s left hand navigation menu.</span></span>

## <a name="next-steps"></a><span data-ttu-id="16c93-176">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="16c93-176">Next steps</span></span>
[<span data-ttu-id="16c93-177">Hantera program med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="16c93-177">Managing Applications with Azure Active Directory</span></span>](active-directory-enable-sso-scenario.md)
