---
title: aaaListing ditt program i hello Azure Active Directory-programgalleriet
description: "Hur toolist ett program som stöder enkel inloggning i hello Azure Active Directory-galleriet | Microsoft Azure"
services: active-directory
documentationcenter: dev-center-name
author: bryanla
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/27/2017
ms.author: bryanla
ms.custom: aaddev
ms.openlocfilehash: 09ccd3b4645a180059b9a9d502e39f1b8933c988
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="listing-your-application-in-hello-azure-active-directory-application-gallery"></a><span data-ttu-id="09c01-103">Visar en lista över dina program i hello Azure Active Directory-programgalleriet</span><span class="sxs-lookup"><span data-stu-id="09c01-103">Listing your application in hello Azure Active Directory application gallery</span></span>
<span data-ttu-id="09c01-104">toolist ett program som stöder enkel inloggning med Azure Active Directory i hello [Azure AD-galleriet](https://azure.microsoft.com/marketplace/active-directory/all/), hello programmet måste först tooimplement något av hello följande integration lägen:</span><span class="sxs-lookup"><span data-stu-id="09c01-104">toolist an application that supports single sign-on with Azure Active Directory in hello [Azure AD gallery](https://azure.microsoft.com/marketplace/active-directory/all/), hello application first needs tooimplement one of hello following integration modes:</span></span>

* <span data-ttu-id="09c01-105">**OpenID Connect** - Direktintegrering med Azure AD med OpenID Connect för autentisering och hello Azure AD medgivande API för konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="09c01-105">**OpenID Connect** - Direct integration with Azure AD using OpenID Connect for authentication and hello Azure AD consent API for configuration.</span></span> <span data-ttu-id="09c01-106">Om du bara startar en integration och programmet har inte stöd för SAML är hello rekommenderar läge.</span><span class="sxs-lookup"><span data-stu-id="09c01-106">If you are just starting an integration and your application does not support SAML, then this is hello recommend mode.</span></span>
* <span data-ttu-id="09c01-107">**SAML** – ditt program redan har hello möjlighet tooconfigure från tredje part identitetsleverantörer med hello SAML-protokoll.</span><span class="sxs-lookup"><span data-stu-id="09c01-107">**SAML** – Your application already has hello ability tooconfigure third-party identity providers using hello SAML protocol.</span></span>

<span data-ttu-id="09c01-108">Lista över kraven för varje läge är nedan.</span><span class="sxs-lookup"><span data-stu-id="09c01-108">Listing requirements for each mode are below.</span></span>

## <a name="openid-connect-integration"></a><span data-ttu-id="09c01-109">OpenID Connect integrering</span><span class="sxs-lookup"><span data-stu-id="09c01-109">OpenID Connect Integration</span></span>
<span data-ttu-id="09c01-110">toointegrate ditt program med Azure AD, följande hello [developer instruktioner](active-directory-authentication-scenarios.md).</span><span class="sxs-lookup"><span data-stu-id="09c01-110">toointegrate your application with Azure AD, following hello [developer instructions](active-directory-authentication-scenarios.md).</span></span> <span data-ttu-id="09c01-111">Slutför hello frågorna nedan och skicka sedan toowaadpartners@microsoft.com.</span><span class="sxs-lookup"><span data-stu-id="09c01-111">Then complete hello questions below and send toowaadpartners@microsoft.com.</span></span>

* <span data-ttu-id="09c01-112">Ange autentiseringsuppgifter för en Testklient eller ett konto med ditt program som kan användas av hello Azure AD-teamet tootest hello-integrering.</span><span class="sxs-lookup"><span data-stu-id="09c01-112">Provide credentials for a test tenant or account with your application that can be used by hello Azure AD team tootest hello integration.</span></span>  
* <span data-ttu-id="09c01-113">Ger instruktioner för hur hello Azure AD-teamet kan logga in och ansluta en instans av Azure AD tooyour program med hjälp av hello [Azure AD medgivande framework](active-directory-integrating-applications.md#overview-of-the-consent-framework).</span><span class="sxs-lookup"><span data-stu-id="09c01-113">Provide instructions on how hello Azure AD team can sign in and connect an instance of Azure AD tooyour application using hello [Azure AD consent framework](active-directory-integrating-applications.md#overview-of-the-consent-framework).</span></span> 
* <span data-ttu-id="09c01-114">Ange eventuella ytterligare instruktioner som krävs för hello Azure AD team tootest enkel inloggning med ditt program.</span><span class="sxs-lookup"><span data-stu-id="09c01-114">Provide any further instructions required for hello Azure AD team tootest single sign-on with your application.</span></span> 
* <span data-ttu-id="09c01-115">Ange hello information nedan:</span><span class="sxs-lookup"><span data-stu-id="09c01-115">Provide hello info below:</span></span>

> <span data-ttu-id="09c01-116">Företagsnamn:</span><span class="sxs-lookup"><span data-stu-id="09c01-116">Company Name:</span></span>
> 
> <span data-ttu-id="09c01-117">Företagets webbplats:</span><span class="sxs-lookup"><span data-stu-id="09c01-117">Company Website:</span></span>
> 
> <span data-ttu-id="09c01-118">Programnamn:</span><span class="sxs-lookup"><span data-stu-id="09c01-118">Application Name:</span></span>
> 
> <span data-ttu-id="09c01-119">Programbeskrivningen (200 tecken):</span><span class="sxs-lookup"><span data-stu-id="09c01-119">Application Description (200 character limit):</span></span>
> 
> <span data-ttu-id="09c01-120">Programmet webbplats (information):</span><span class="sxs-lookup"><span data-stu-id="09c01-120">Application Website (informational):</span></span>
> 
> <span data-ttu-id="09c01-121">Programmet tekniska avdelningen eller kontaktinformation:</span><span class="sxs-lookup"><span data-stu-id="09c01-121">Application Technical Support Website or Contact Info:</span></span>
> 
> <span data-ttu-id="09c01-122">Program-ID för hello program enligt hello programinformationen https://portal.azure.com:</span><span class="sxs-lookup"><span data-stu-id="09c01-122">Application  ID of hello application, as shown in hello application details at https://portal.azure.com:</span></span>
> 
> <span data-ttu-id="09c01-123">URL som programmet där kunder gå toosign för och/eller köpa programmet hello:</span><span class="sxs-lookup"><span data-stu-id="09c01-123">Application Sign-Up URL where customers go toosign up for and /or purchase hello application:</span></span>
> 
> <span data-ttu-id="09c01-124">Välj upp toothree kategorier för ditt program toobe visas (för tillgängliga kategorier finns hello Azure Active Directory Marketplace):</span><span class="sxs-lookup"><span data-stu-id="09c01-124">Choose up toothree categories for your application toobe listed under (for available categories see hello Azure Active Directory Marketplace):</span></span>
> 
> <span data-ttu-id="09c01-125">Koppla liten ikon (PNG-fil, 45px av 45px bakgrundsfärg):</span><span class="sxs-lookup"><span data-stu-id="09c01-125">Attach Application Small Icon (PNG file, 45px by 45px, solid background color):</span></span>
> 
> <span data-ttu-id="09c01-126">Koppla stora programikonen (PNG-fil, 215px av 215px bakgrundsfärg):</span><span class="sxs-lookup"><span data-stu-id="09c01-126">Attach Application Large Icon (PNG file, 215px by 215px, solid background color):</span></span>
> 
> <span data-ttu-id="09c01-127">Koppla programmet logotyp (PNG-fil, 150px av 122px transparent bakgrundsfärg):</span><span class="sxs-lookup"><span data-stu-id="09c01-127">Attach Application Logo (PNG file, 150px by 122px, transparent background color):</span></span>
> 
> 

## <a name="saml-integration"></a><span data-ttu-id="09c01-128">SAML-integrering</span><span class="sxs-lookup"><span data-stu-id="09c01-128">SAML Integration</span></span>
<span data-ttu-id="09c01-129">Alla appar som stöder SAML 2.0 kan integreras direkt med en Azure AD-klient med hjälp av [dessa instruktioner tooadd ett anpassat program](../active-directory-saas-custom-apps.md).</span><span class="sxs-lookup"><span data-stu-id="09c01-129">Any app that supports SAML 2.0 can be integrated directly with an Azure AD tenant using [these instructions tooadd a custom application](../active-directory-saas-custom-apps.md).</span></span> <span data-ttu-id="09c01-130">När du har testat att integrera dina appar fungerar med Azure AD kan skicka hello följande information för<mailto:waadpartners@microsoft.com>.</span><span class="sxs-lookup"><span data-stu-id="09c01-130">Once you have tested that your application integration works with Azure AD, send hello following information too<mailto:waadpartners@microsoft.com>.</span></span>

* <span data-ttu-id="09c01-131">Ange autentiseringsuppgifter för en Testklient eller ett konto med ditt program som kan användas av hello Azure AD-teamet tootest hello-integrering.</span><span class="sxs-lookup"><span data-stu-id="09c01-131">Provide credentials for a test tenant or account with your application that can be used by hello Azure AD team tootest hello integration.</span></span>  
* <span data-ttu-id="09c01-132">Ange hello SAML inloggnings-URL, utfärdar-URL (enhets-ID) och Reply URL (assertion konsumenten service) som värden för ditt program enligt [här](../active-directory-saas-custom-apps.md).</span><span class="sxs-lookup"><span data-stu-id="09c01-132">Provide hello SAML Sign-On URL, Issuer URL (entity ID), and Reply URL (assertion consumer service) values for your application, as described [here](../active-directory-saas-custom-apps.md).</span></span> <span data-ttu-id="09c01-133">Om du vanligtvis ange dessa värden som en del av en SAML-metadatafil, sedan skicka som också.</span><span class="sxs-lookup"><span data-stu-id="09c01-133">If you typically provide these values as part of a SAML metadata file, then please send that as well.</span></span>
* <span data-ttu-id="09c01-134">Ange en kort beskrivning av hur tooconfigure Azure AD som en identitetsleverantör i ditt program använder SAML 2.0.</span><span class="sxs-lookup"><span data-stu-id="09c01-134">Provide a brief description of how tooconfigure Azure AD as an identity provider in your application using SAML 2.0.</span></span> <span data-ttu-id="09c01-135">Om ditt program har stöd för att konfigurera Azure AD som en identitetsleverantör via en administrativa självbetjäningsportalen, sedan kontrollera hello autentiseringsuppgifter som anges ovan inkluderar hello möjlighet tooset detta upp.</span><span class="sxs-lookup"><span data-stu-id="09c01-135">If your application supports configuring Azure AD as an identity provider through a self-service administrative portal, then please ensure hello credentials provided above include hello ability tooset this up.</span></span>
* <span data-ttu-id="09c01-136">Ange hello information nedan:</span><span class="sxs-lookup"><span data-stu-id="09c01-136">Provide hello info below:</span></span>

> <span data-ttu-id="09c01-137">Företagsnamn:</span><span class="sxs-lookup"><span data-stu-id="09c01-137">Company Name:</span></span>
> 
> <span data-ttu-id="09c01-138">Företagets webbplats:</span><span class="sxs-lookup"><span data-stu-id="09c01-138">Company Website:</span></span>
> 
> <span data-ttu-id="09c01-139">Programnamn:</span><span class="sxs-lookup"><span data-stu-id="09c01-139">Application Name:</span></span>
> 
> <span data-ttu-id="09c01-140">Programbeskrivningen (200 tecken):</span><span class="sxs-lookup"><span data-stu-id="09c01-140">Application Description (200 character limit):</span></span>
> 
> <span data-ttu-id="09c01-141">Programmet webbplats (information):</span><span class="sxs-lookup"><span data-stu-id="09c01-141">Application Website (informational):</span></span>
> 
> <span data-ttu-id="09c01-142">Programmet tekniska avdelningen eller kontaktinformation:</span><span class="sxs-lookup"><span data-stu-id="09c01-142">Application Technical Support Website or Contact Info:</span></span>
> 
> <span data-ttu-id="09c01-143">URL som programmet där kunder gå toosign för och/eller köpa programmet hello:</span><span class="sxs-lookup"><span data-stu-id="09c01-143">Application Sign-Up URL where customers go toosign up for and /or purchase hello application:</span></span>
> 
> <span data-ttu-id="09c01-144">Välj upp toothree kategorier för ditt program toobe visas under (tillgängliga kategorier finns hello [Azure Active Directory Marketplace](https://azure.microsoft.com/marketplace/active-directory/))):</span><span class="sxs-lookup"><span data-stu-id="09c01-144">Choose up toothree categories for your application toobe listed under (for available categories see hello [Azure Active Directory Marketplace](https://azure.microsoft.com/marketplace/active-directory/))):</span></span>
> 
> <span data-ttu-id="09c01-145">Koppla liten ikon (PNG-fil, 45px av 45px bakgrundsfärg):</span><span class="sxs-lookup"><span data-stu-id="09c01-145">Attach Application Small Icon (PNG file, 45px by 45px, solid background color):</span></span>
> 
> <span data-ttu-id="09c01-146">Koppla stora programikonen (PNG-fil, 215px av 215px bakgrundsfärg):</span><span class="sxs-lookup"><span data-stu-id="09c01-146">Attach Application Large Icon (PNG file, 215px by 215px, solid background color):</span></span>
> 
> <span data-ttu-id="09c01-147">Koppla programmet logotyp (PNG-fil, 150px av 122px transparent bakgrundsfärg):</span><span class="sxs-lookup"><span data-stu-id="09c01-147">Attach Application Logo (PNG file, 150px by 122px, transparent background color):</span></span>
> 
> 

