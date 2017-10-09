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
# <a name="listing-your-application-in-hello-azure-active-directory-application-gallery"></a>Visar en lista över dina program i hello Azure Active Directory-programgalleriet
toolist ett program som stöder enkel inloggning med Azure Active Directory i hello [Azure AD-galleriet](https://azure.microsoft.com/marketplace/active-directory/all/), hello programmet måste först tooimplement något av hello följande integration lägen:

* **OpenID Connect** - Direktintegrering med Azure AD med OpenID Connect för autentisering och hello Azure AD medgivande API för konfigurationen. Om du bara startar en integration och programmet har inte stöd för SAML är hello rekommenderar läge.
* **SAML** – ditt program redan har hello möjlighet tooconfigure från tredje part identitetsleverantörer med hello SAML-protokoll.

Lista över kraven för varje läge är nedan.

## <a name="openid-connect-integration"></a>OpenID Connect integrering
toointegrate ditt program med Azure AD, följande hello [developer instruktioner](active-directory-authentication-scenarios.md). Slutför hello frågorna nedan och skicka sedan toowaadpartners@microsoft.com.

* Ange autentiseringsuppgifter för en Testklient eller ett konto med ditt program som kan användas av hello Azure AD-teamet tootest hello-integrering.  
* Ger instruktioner för hur hello Azure AD-teamet kan logga in och ansluta en instans av Azure AD tooyour program med hjälp av hello [Azure AD medgivande framework](active-directory-integrating-applications.md#overview-of-the-consent-framework). 
* Ange eventuella ytterligare instruktioner som krävs för hello Azure AD team tootest enkel inloggning med ditt program. 
* Ange hello information nedan:

> Företagsnamn:
> 
> Företagets webbplats:
> 
> Programnamn:
> 
> Programbeskrivningen (200 tecken):
> 
> Programmet webbplats (information):
> 
> Programmet tekniska avdelningen eller kontaktinformation:
> 
> Program-ID för hello program enligt hello programinformationen https://portal.azure.com:
> 
> URL som programmet där kunder gå toosign för och/eller köpa programmet hello:
> 
> Välj upp toothree kategorier för ditt program toobe visas (för tillgängliga kategorier finns hello Azure Active Directory Marketplace):
> 
> Koppla liten ikon (PNG-fil, 45px av 45px bakgrundsfärg):
> 
> Koppla stora programikonen (PNG-fil, 215px av 215px bakgrundsfärg):
> 
> Koppla programmet logotyp (PNG-fil, 150px av 122px transparent bakgrundsfärg):
> 
> 

## <a name="saml-integration"></a>SAML-integrering
Alla appar som stöder SAML 2.0 kan integreras direkt med en Azure AD-klient med hjälp av [dessa instruktioner tooadd ett anpassat program](../active-directory-saas-custom-apps.md). När du har testat att integrera dina appar fungerar med Azure AD kan skicka hello följande information för<mailto:waadpartners@microsoft.com>.

* Ange autentiseringsuppgifter för en Testklient eller ett konto med ditt program som kan användas av hello Azure AD-teamet tootest hello-integrering.  
* Ange hello SAML inloggnings-URL, utfärdar-URL (enhets-ID) och Reply URL (assertion konsumenten service) som värden för ditt program enligt [här](../active-directory-saas-custom-apps.md). Om du vanligtvis ange dessa värden som en del av en SAML-metadatafil, sedan skicka som också.
* Ange en kort beskrivning av hur tooconfigure Azure AD som en identitetsleverantör i ditt program använder SAML 2.0. Om ditt program har stöd för att konfigurera Azure AD som en identitetsleverantör via en administrativa självbetjäningsportalen, sedan kontrollera hello autentiseringsuppgifter som anges ovan inkluderar hello möjlighet tooset detta upp.
* Ange hello information nedan:

> Företagsnamn:
> 
> Företagets webbplats:
> 
> Programnamn:
> 
> Programbeskrivningen (200 tecken):
> 
> Programmet webbplats (information):
> 
> Programmet tekniska avdelningen eller kontaktinformation:
> 
> URL som programmet där kunder gå toosign för och/eller köpa programmet hello:
> 
> Välj upp toothree kategorier för ditt program toobe visas under (tillgängliga kategorier finns hello [Azure Active Directory Marketplace](https://azure.microsoft.com/marketplace/active-directory/))):
> 
> Koppla liten ikon (PNG-fil, 45px av 45px bakgrundsfärg):
> 
> Koppla stora programikonen (PNG-fil, 215px av 215px bakgrundsfärg):
> 
> Koppla programmet logotyp (PNG-fil, 150px av 122px transparent bakgrundsfärg):
> 
> 

