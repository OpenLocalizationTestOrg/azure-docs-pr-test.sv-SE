---
title: "Hur du avgör vilken enkel inloggning på metod för att använda | Microsoft Docs"
description: "Förstå enkel inloggning lägen stöds av Azure AD och hur du väljer vilka som ska användas för programmet som du är intresserad av."
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
ms.openlocfilehash: 80f4a965920fec9cb578c1bee235c7857f37431e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-determine-what-single-sign-on-method-to-use"></a><span data-ttu-id="d33a0-103">Hur du avgör vilken enkel inloggning på metod som ska användas</span><span class="sxs-lookup"><span data-stu-id="d33a0-103">How to determine what single-sign on method to use</span></span>

<span data-ttu-id="d33a0-104">Den här artikeln hjälper dig att förstå enkel inloggning lägen stöds av Azure AD och hur du väljer vilka som ska användas för programmet som du är intresserad av.</span><span class="sxs-lookup"><span data-stu-id="d33a0-104">This article help you to understand the single sign-on modes supported by Azure AD and how to pick which one to choose for the application you are interested in.</span></span>

## <a name="single-sign-on-and-provisioning-modes-supported-by-specific-application-types"></a><span data-ttu-id="d33a0-105">Enkel inloggning och provisioning lägen som stöds av specifika programtyper</span><span class="sxs-lookup"><span data-stu-id="d33a0-105">Single sign-on and provisioning modes supported by specific application types</span></span>

<span data-ttu-id="d33a0-106">I tabellen nedan beskrivs olika enkel inloggning och allokering lägen som stöds av varje ovan programtyper.</span><span class="sxs-lookup"><span data-stu-id="d33a0-106">The table below describes the different single sign-on and provisioning modes supported by each of the above application types.</span></span> <span data-ttu-id="d33a0-107">Du kan använda den här tabellen hjälper dig att förstå vilket program som du behöver lägga till stöd för ett specifikt mål.</span><span class="sxs-lookup"><span data-stu-id="d33a0-107">You can use this table to help you to understand which application you need to add to support a specific goal.</span></span>

  ![Asien/Stillahavsområdet register](./media/application-tables/table1.png)

## <a name="how-to-choose-a-single-sign-on-mode"></a><span data-ttu-id="d33a0-109">Hur du väljer ett läge för enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d33a0-109">How to choose a single sign-on mode</span></span>

<span data-ttu-id="d33a0-110">Den stöds **enkel inloggning** lägen för Azure AD-program visas nedan.</span><span class="sxs-lookup"><span data-stu-id="d33a0-110">The supported **single sign-on** modes for Azure AD applications are listed below.</span></span>

-   <span data-ttu-id="d33a0-111">**Azure AD enkel inloggning inaktiverat** – Välj Azure AD enkel inloggning inaktiverad **läget för enkel inloggning** om du ännu inte är redo att integrera programmet med enkel inloggning med Azure AD eller bara testar det.</span><span class="sxs-lookup"><span data-stu-id="d33a0-111">**Azure AD single sign-on disabled** – choose Azure AD single sign-on disabled **single sign-on mode** if you are not yet ready to integrate this application with single sign-on with Azure AD, or are simply testing it out</span></span>

-   <span data-ttu-id="d33a0-112">**Länkad inloggning** – Välj den [inloggning länkade](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) **läget för enkel inloggning** om du har ett program som redan är kopplad till en befintlig enkel inloggning lösning eller om du vill publicera en enkel länk för användarna i deras [programmet åtkomstpanelen](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) eller [startprogrammet för Office 365](https://login.microsoftonline.com/common/oauth2/authorize?response_mode=form_post&response_type=id_token&scope=openid&nonce=d508a995-f6d6-4b8a-81b8-825c71f1be46.636253878097046923&state=https%3a%2f%2fsupport.office.com%2farticle%2fMeet-the-Office-365-app-launcher-79f12104-6fed-442f-96a0-eb089a3f476a%3fui%3den-US%26rs%3den-US%26ad%3dUS&client_id=4b233688-031c-404b-9a80-a4f3f2351f90&redirect_uri=https%3a%2f%2fsupport.office.com%2fauth%2fsignin&login_hint=asteen%40microsoft.com&prompt=none)</span><span class="sxs-lookup"><span data-stu-id="d33a0-112">**Linked Sign-on** – choose the [Linked Sign-on](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) **single sign-on mode** if you have an application that is already connected with an existing single sign-on solution, or if you just want to publish a simple link for your users in their [Application Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) or [Office 365 application launcher](https://login.microsoftonline.com/common/oauth2/authorize?response_mode=form_post&response_type=id_token&scope=openid&nonce=d508a995-f6d6-4b8a-81b8-825c71f1be46.636253878097046923&state=https%3a%2f%2fsupport.office.com%2farticle%2fMeet-the-Office-365-app-launcher-79f12104-6fed-442f-96a0-eb089a3f476a%3fui%3den-US%26rs%3den-US%26ad%3dUS&client_id=4b233688-031c-404b-9a80-a4f3f2351f90&redirect_uri=https%3a%2f%2fsupport.office.com%2fauth%2fsignin&login_hint=asteen%40microsoft.com&prompt=none)</span></span>

-   <span data-ttu-id="d33a0-113">**Lösenordsbaserade inloggning** – Välj den [lösenordsbaserade inloggning](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) **läget för enkel inloggning** om ditt program visas med ett HTML-fält för användarnamn och lösenord och du vill lagra det användarnamn och lösenord på ett säkert sätt ska återupprepas till programmet senare</span><span class="sxs-lookup"><span data-stu-id="d33a0-113">**Password-based Sign-on** – choose the [Password-based Sign-on](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) **single sign-on mode** if your application renders an HTML username and password field and you want to store that username and password securely to be replayed to the application later</span></span>

-   <span data-ttu-id="d33a0-114">**SAML-baserade inloggning** – Välj den [SAML-baserade inloggning](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) enkel inloggning på läge om programmet stöder SAML- eller OpenID Connect-protokoll eller om du vill kunna mappa användare till programroller för specifika baserat på regler som du definierar i SAML-anspråk *(**Obs:** det här alternativet är inte tillgängligt när application proxy har konfigurerats för ett program) *</span><span class="sxs-lookup"><span data-stu-id="d33a0-114">**SAML-based Sign-on** – choose the [SAML-based Sign-on](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) single-sign on mode if your application supports the SAML or OpenID Connect protocols, or you want to be able to map users to specific application roles based on rules you define in your SAML claims *(**Note:** this option is not available when the application proxy is configured for an application)*</span></span>

-   <span data-ttu-id="d33a0-115">**Rubrik-baserade inloggning** – Välj det här [huvud-baserade inloggning](https://docs.microsoft.com/azure/active-directory/application-proxy-ping-access#what-is-pingaccess-for-azure-ad) enkel inloggning läge om du har ett program med PingAccess som stöder HTTP-huvudet baseras autentisering som du vill utföra enkel inloggning till *(**Obs:** det här alternativet är bara tillgängligt om application proxy- och PingAccess är konfigurerad för ett program) *</span><span class="sxs-lookup"><span data-stu-id="d33a0-115">**Header-based Sign-on** – choose this [Header-based Sign-on](https://docs.microsoft.com/azure/active-directory/application-proxy-ping-access#what-is-pingaccess-for-azure-ad) single sign-on mode if you have an application using PingAccess that supports HTTP-header based authentication that you wish to perform single-sign on to *(**Note:** this option is only available when the application proxy and PingAccess is configured for an application)*</span></span>

-   <span data-ttu-id="d33a0-116">**Integrerad Windows-autentisering** – Välj den [integrerad Windows-autentisering](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-sso-using-kcd) enkel inloggning på läge när exponera ett lokalt WIA program som du vill utföra enkel inloggning till *(**Obs:** det här alternativet är endast tillgängligt när application proxy har konfigurerats för ett program) *</span><span class="sxs-lookup"><span data-stu-id="d33a0-116">**Integrated Windows Authentication** – choose the [Integrated Windows Authentication](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-sso-using-kcd) single-sign on mode when exposing an on-premises WIA application that you wish to perform single-sign on to *(**Note:** this option is only available when the application proxy is configured for an application)*</span></span>

## <a name="single-sign-on-modes-for-custom-developed-applications"></a><span data-ttu-id="d33a0-117">Enkel inloggning lägen för anpassad utvecklade program</span><span class="sxs-lookup"><span data-stu-id="d33a0-117">Single sign-on modes for custom-developed applications</span></span>

<span data-ttu-id="d33a0-118">Program som du har anpassat utvecklas genom den [anpassad utvecklat program](#_Custom-Developed_Applications) upplevelse stöder också ytterligare enkel inloggning lägen inte listas här ovan.</span><span class="sxs-lookup"><span data-stu-id="d33a0-118">Applications you have custom developed through the [Custom-developed application](#_Custom-Developed_Applications) experience also support additional single sign-on modes not listed above.</span></span> <span data-ttu-id="d33a0-119">Exempel på dessa är:</span><span class="sxs-lookup"><span data-stu-id="d33a0-119">These include:</span></span>

-   <span data-ttu-id="d33a0-120">[OAuth 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code) baserat inloggning</span><span class="sxs-lookup"><span data-stu-id="d33a0-120">[OAuth 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code) based sign-on</span></span>

-   <span data-ttu-id="d33a0-121">[OpenID Connect 1.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-openid-connect-code) baserat inloggning</span><span class="sxs-lookup"><span data-stu-id="d33a0-121">[OpenID Connect 1.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-openid-connect-code) based sign-on</span></span>

-   <span data-ttu-id="d33a0-122">[WS-Federation 1.2](http://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html) baserat inloggning</span><span class="sxs-lookup"><span data-stu-id="d33a0-122">[WS-Federation 1.2](http://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html) based sign-on</span></span>

-   <span data-ttu-id="d33a0-123">[SAML 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-protocol-reference) baserat inloggning</span><span class="sxs-lookup"><span data-stu-id="d33a0-123">[SAML 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-protocol-reference) based sign-on</span></span>

<span data-ttu-id="d33a0-124">Läs den [Utvecklarhandbok för Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide) vill veta mer om hur du skapar en anpassad utvecklat program som stöder dessa lägen för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="d33a0-124">Read the [Azure Active Directory developer’s guide](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide) to learn more about how to create a custom-developed application which supports these single sign-on modes.</span></span>

## <a name="how-to-set-an-applications-single-sign-on-mode"></a><span data-ttu-id="d33a0-125">Hur du ställer in ett program enkel inloggning läge</span><span class="sxs-lookup"><span data-stu-id="d33a0-125">How to set an application’s single sign-on mode</span></span>

<span data-ttu-id="d33a0-126">Så här anger du ett program **enkel inloggning** läge, följ instruktionerna nedan:</span><span class="sxs-lookup"><span data-stu-id="d33a0-126">To set an application’s **single sign-on** mode, follow the instructions below:</span></span>

1.  <span data-ttu-id="d33a0-127">Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**</span><span class="sxs-lookup"><span data-stu-id="d33a0-127">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="d33a0-128">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="d33a0-128">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="d33a0-129">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="d33a0-129">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="d33a0-130">Klicka på **företagsprogram** från Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="d33a0-130">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="d33a0-131">Klicka på **alla program** att visa en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="d33a0-131">click **All Applications** to view a list of all your applications.</span></span>

   * <span data-ttu-id="d33a0-132">Om du inte ser programmet som du vill visa här använder du den **Filter** kontrollen längst upp i den **listan med alla program** och ange den **visa** att **alla program.**</span><span class="sxs-lookup"><span data-stu-id="d33a0-132">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="d33a0-133">Välj det program som du vill konfigurera enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d33a0-133">Select the application you want to configure single sign-on</span></span>

7.  <span data-ttu-id="d33a0-134">När programmet läses in klickar du på **enkel inloggning** från programmets vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="d33a0-134">Once the application loads, click **Single sign-on** from the application’s left hand navigation menu.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d33a0-135">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d33a0-135">Next steps</span></span>
[<span data-ttu-id="d33a0-136">Tillhandahålla enkel inloggning till dina appar med Application Proxy</span><span class="sxs-lookup"><span data-stu-id="d33a0-136">Provide single sign-on to your apps with Application Proxy</span></span>](active-directory-application-proxy-sso-using-kcd.md)

