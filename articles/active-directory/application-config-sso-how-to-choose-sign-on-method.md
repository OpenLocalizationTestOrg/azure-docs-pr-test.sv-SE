---
title: "aaaHow toodetermine vilka enkel inloggning på metoden toouse | Microsoft Docs"
description: "Förstå hello enkel inloggning lägen som stöds av Azure AD och hur toopick som en toochoose för hello programmet du är intresserad av."
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
ms.openlocfilehash: 64f0bc1dc8281d1ab8222fd50eaceaf710704886
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toodetermine-what-single-sign-on-method-toouse"></a><span data-ttu-id="78e20-103">Hur toodetermine vilka enkel inloggning på metoden toouse</span><span class="sxs-lookup"><span data-stu-id="78e20-103">How toodetermine what single-sign on method toouse</span></span>

<span data-ttu-id="78e20-104">Den här artikeln hjälper dig toounderstand hello enkel inloggning lägen som stöds av Azure AD och hur toopick som en toochoose för hello programmet du är intresserad av.</span><span class="sxs-lookup"><span data-stu-id="78e20-104">This article help you toounderstand hello single sign-on modes supported by Azure AD and how toopick which one toochoose for hello application you are interested in.</span></span>

## <a name="single-sign-on-and-provisioning-modes-supported-by-specific-application-types"></a><span data-ttu-id="78e20-105">Enkel inloggning och provisioning lägen som stöds av specifika programtyper</span><span class="sxs-lookup"><span data-stu-id="78e20-105">Single sign-on and provisioning modes supported by specific application types</span></span>

<span data-ttu-id="78e20-106">hello tabellen nedan beskrivs hello olika enkel inloggning och provisioning lägen som stöds av varje hello ovan programtyper.</span><span class="sxs-lookup"><span data-stu-id="78e20-106">hello table below describes hello different single sign-on and provisioning modes supported by each of hello above application types.</span></span> <span data-ttu-id="78e20-107">Du kan använda den här tabellen toohelp toounderstand vilket program som du behöver tooadd toosupport ett specifikt mål.</span><span class="sxs-lookup"><span data-stu-id="78e20-107">You can use this table toohelp you toounderstand which application you need tooadd toosupport a specific goal.</span></span>

  ![Asien/Stillahavsområdet register](./media/application-tables/table1.png)

## <a name="how-toochoose-a-single-sign-on-mode"></a><span data-ttu-id="78e20-109">Hur toochoose ett läge för enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="78e20-109">How toochoose a single sign-on mode</span></span>

<span data-ttu-id="78e20-110">hello stöds **enkel inloggning** lägen för Azure AD-program visas nedan.</span><span class="sxs-lookup"><span data-stu-id="78e20-110">hello supported **single sign-on** modes for Azure AD applications are listed below.</span></span>

-   <span data-ttu-id="78e20-111">**Azure AD enkel inloggning inaktiverad** – Välj Azure AD enkel inloggning inaktiverad **läget för enkel inloggning** om du ännu inte klar toointegrate programmet med enkel inloggning med Azure AD, eller bara testar det.</span><span class="sxs-lookup"><span data-stu-id="78e20-111">**Azure AD single sign-on disabled** – choose Azure AD single sign-on disabled **single sign-on mode** if you are not yet ready toointegrate this application with single sign-on with Azure AD, or are simply testing it out</span></span>

-   <span data-ttu-id="78e20-112">**Länkad inloggning** – Välj hello [inloggning länkade](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) **läget för enkel inloggning** om du har ett program som redan är kopplad till en befintlig enkel inloggning lösning, eller om du bara vill toopublish en enkel länka för användarna i deras [programmet åtkomstpanelen](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) eller [startprogrammet för Office 365](https://login.microsoftonline.com/common/oauth2/authorize?response_mode=form_post&response_type=id_token&scope=openid&nonce=d508a995-f6d6-4b8a-81b8-825c71f1be46.636253878097046923&state=https%3a%2f%2fsupport.office.com%2farticle%2fMeet-the-Office-365-app-launcher-79f12104-6fed-442f-96a0-eb089a3f476a%3fui%3den-US%26rs%3den-US%26ad%3dUS&client_id=4b233688-031c-404b-9a80-a4f3f2351f90&redirect_uri=https%3a%2f%2fsupport.office.com%2fauth%2fsignin&login_hint=asteen%40microsoft.com&prompt=none)</span><span class="sxs-lookup"><span data-stu-id="78e20-112">**Linked Sign-on** – choose hello [Linked Sign-on](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) **single sign-on mode** if you have an application that is already connected with an existing single sign-on solution, or if you just want toopublish a simple link for your users in their [Application Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) or [Office 365 application launcher](https://login.microsoftonline.com/common/oauth2/authorize?response_mode=form_post&response_type=id_token&scope=openid&nonce=d508a995-f6d6-4b8a-81b8-825c71f1be46.636253878097046923&state=https%3a%2f%2fsupport.office.com%2farticle%2fMeet-the-Office-365-app-launcher-79f12104-6fed-442f-96a0-eb089a3f476a%3fui%3den-US%26rs%3den-US%26ad%3dUS&client_id=4b233688-031c-404b-9a80-a4f3f2351f90&redirect_uri=https%3a%2f%2fsupport.office.com%2fauth%2fsignin&login_hint=asteen%40microsoft.com&prompt=none)</span></span>

-   <span data-ttu-id="78e20-113">**Lösenordsbaserade inloggning** – Välj hello [lösenordsbaserade inloggning](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) **läget för enkel inloggning** om ditt program visas med ett HTML-fält för användarnamn och lösenord och du vill toostore som användarnamn och lösenord på ett säkert sätt toobe spelas toohello programmet senare</span><span class="sxs-lookup"><span data-stu-id="78e20-113">**Password-based Sign-on** – choose hello [Password-based Sign-on](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) **single sign-on mode** if your application renders an HTML username and password field and you want toostore that username and password securely toobe replayed toohello application later</span></span>

-   <span data-ttu-id="78e20-114">**SAML-baserade inloggning** – Välj hello [SAML-baserade inloggning](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) enkel inloggning på läge om programmet stöder hello SAML eller OpenID Connect protokoll eller om du vill toobe kan toomap användare toospecific programroller baserat regler som du definierar i din SAML anspråk *(**Obs:** det här alternativet är inte tillgängligt när hello programproxy konfigureras för ett program) *</span><span class="sxs-lookup"><span data-stu-id="78e20-114">**SAML-based Sign-on** – choose hello [SAML-based Sign-on](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) single-sign on mode if your application supports hello SAML or OpenID Connect protocols, or you want toobe able toomap users toospecific application roles based on rules you define in your SAML claims *(**Note:** this option is not available when hello application proxy is configured for an application)*</span></span>

-   <span data-ttu-id="78e20-115">**Rubrik-baserade inloggning** – Välj det här [huvud-baserade inloggning](https://docs.microsoft.com/azure/active-directory/application-proxy-ping-access#what-is-pingaccess-for-azure-ad) enkel inloggning läge om du har ett program med PingAccess som stöder HTTP-huvudet baseras autentisering som du vill att tooperform enkel inloggning på för *(**Obs:** det här alternativet är endast tillgängligt när hello programproxy och PingAccess konfigureras för ett program) *</span><span class="sxs-lookup"><span data-stu-id="78e20-115">**Header-based Sign-on** – choose this [Header-based Sign-on](https://docs.microsoft.com/azure/active-directory/application-proxy-ping-access#what-is-pingaccess-for-azure-ad) single sign-on mode if you have an application using PingAccess that supports HTTP-header based authentication that you wish tooperform single-sign on too*(**Note:** this option is only available when hello application proxy and PingAccess is configured for an application)*</span></span>

-   <span data-ttu-id="78e20-116">**Integrerad Windows-autentisering** – Välj hello [integrerad Windows-autentisering](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-sso-using-kcd) enkel inloggning på läge när exponera ett lokalt WIA program som du vill att tooperform enkel inloggning på för*()*  *Obs:** det här alternativet är endast tillgängligt när hello programproxy konfigureras för ett program) *</span><span class="sxs-lookup"><span data-stu-id="78e20-116">**Integrated Windows Authentication** – choose hello [Integrated Windows Authentication](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-sso-using-kcd) single-sign on mode when exposing an on-premises WIA application that you wish tooperform single-sign on too*(**Note:** this option is only available when hello application proxy is configured for an application)*</span></span>

## <a name="single-sign-on-modes-for-custom-developed-applications"></a><span data-ttu-id="78e20-117">Enkel inloggning lägen för anpassad utvecklade program</span><span class="sxs-lookup"><span data-stu-id="78e20-117">Single sign-on modes for custom-developed applications</span></span>

<span data-ttu-id="78e20-118">Program som du har anpassat utvecklas genom hello [anpassad utvecklat program](#_Custom-Developed_Applications) upplevelse stöder också ytterligare enkel inloggning lägen inte listas här ovan.</span><span class="sxs-lookup"><span data-stu-id="78e20-118">Applications you have custom developed through hello [Custom-developed application](#_Custom-Developed_Applications) experience also support additional single sign-on modes not listed above.</span></span> <span data-ttu-id="78e20-119">Exempel på dessa är:</span><span class="sxs-lookup"><span data-stu-id="78e20-119">These include:</span></span>

-   <span data-ttu-id="78e20-120">[OAuth 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code) baserat inloggning</span><span class="sxs-lookup"><span data-stu-id="78e20-120">[OAuth 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code) based sign-on</span></span>

-   <span data-ttu-id="78e20-121">[OpenID Connect 1.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-openid-connect-code) baserat inloggning</span><span class="sxs-lookup"><span data-stu-id="78e20-121">[OpenID Connect 1.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-openid-connect-code) based sign-on</span></span>

-   <span data-ttu-id="78e20-122">[WS-Federation 1.2](http://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html) baserat inloggning</span><span class="sxs-lookup"><span data-stu-id="78e20-122">[WS-Federation 1.2](http://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html) based sign-on</span></span>

-   <span data-ttu-id="78e20-123">[SAML 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-protocol-reference) baserat inloggning</span><span class="sxs-lookup"><span data-stu-id="78e20-123">[SAML 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-protocol-reference) based sign-on</span></span>

<span data-ttu-id="78e20-124">Läs hello [Utvecklarhandbok för Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide) toolearn mer om hur toocreate en anpassad utvecklade program som stöder dessa enkel inloggning lägen.</span><span class="sxs-lookup"><span data-stu-id="78e20-124">Read hello [Azure Active Directory developer’s guide](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide) toolearn more about how toocreate a custom-developed application which supports these single sign-on modes.</span></span>

## <a name="how-tooset-an-applications-single-sign-on-mode"></a><span data-ttu-id="78e20-125">Hur tooset ett program har enkel inloggning läge</span><span class="sxs-lookup"><span data-stu-id="78e20-125">How tooset an application’s single sign-on mode</span></span>

<span data-ttu-id="78e20-126">tooset ett program **enkel inloggning** läge, följ hello instruktionerna nedan:</span><span class="sxs-lookup"><span data-stu-id="78e20-126">tooset an application’s **single sign-on** mode, follow hello instructions below:</span></span>

1.  <span data-ttu-id="78e20-127">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**</span><span class="sxs-lookup"><span data-stu-id="78e20-127">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="78e20-128">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="78e20-128">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="78e20-129">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="78e20-129">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="78e20-130">Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="78e20-130">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="78e20-131">Klicka på **alla program** tooview en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="78e20-131">click **All Applications** tooview a list of all your applications.</span></span>

   * <span data-ttu-id="78e20-132">Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**</span><span class="sxs-lookup"><span data-stu-id="78e20-132">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="78e20-133">Välj hello-program som du vill tooconfigure enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="78e20-133">Select hello application you want tooconfigure single sign-on</span></span>

7.  <span data-ttu-id="78e20-134">När programmet hello läses in klickar du på **enkel inloggning** från hello programmet vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="78e20-134">Once hello application loads, click **Single sign-on** from hello application’s left hand navigation menu.</span></span>

## <a name="next-steps"></a><span data-ttu-id="78e20-135">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="78e20-135">Next steps</span></span>
[<span data-ttu-id="78e20-136">Tillhandahålla enkel inloggning tooyour appar med Application Proxy</span><span class="sxs-lookup"><span data-stu-id="78e20-136">Provide single sign-on tooyour apps with Application Proxy</span></span>](active-directory-application-proxy-sso-using-kcd.md)

