---
title: aaaAzure AD v2 JS SPA interaktiv installation - introduktion | Microsoft Docs
description: "Hur JavaScript SPA program anropar en API som kräver åtkomst-token i Azure Active Directory v2 slutpunkten"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/01/2017
ms.author: andret
ms.openlocfilehash: 7e4250ccca837a17489a99603da167009cc2e3d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="call-hello-microsoft-graph-api-from-a-javascript-single-page-application-spa"></a>Anropa hello Microsoft Graph API från ett JavaScript enda sidan program (SPA)

Den här guiden visar hur en JavaScript enda sidan program (SPA) kan logga in personliga arbete och skolkonton, hämta en åtkomst-token och anropa hello Microsoft Graph API eller andra API: er som kräver åtkomst-token från hello Azure Active Directory v2 slutpunkt.

### <a name="how-this-guide-works"></a>Hur den här guiden fungerar

![Så här fungerar hello sample-appen som genererats av den här guiden](media/active-directory-singlepageapp-javascriptspa-introduction/javascriptspa-intro.png)

<!--start-collapse-->
### <a name="more-information"></a>Mer information

hello exempelprogram som skapats av den här guiden kan en JavaScript SPA tooquery hello Microsoft Graph API eller ett webb-API som accepterar token från Azure Active Directory v2-slutpunkten. I det här scenariot är när en användare loggar in, en åtkomst-token begärda och tillagda tooHTTP begäranden via hello authorization-huvud. Token förvärv och förnyelse hanteras av hello Microsoft Authentication Library (MSAL).

<!--end-collapse-->

<!--start-collapse-->
### <a name="libraries"></a>Bibliotek

Den här guiden använder hello följande bibliotek:

|Bibliotek|Beskrivning|
|---|---|
|[msal.js](https://github.com/AzureAD/microsoft-authentication-library-for-js)|Microsoft-Autentiseringsbibliotek för JavaScript-förhandsgranskning|

> [!NOTE]
> *msal.js* mål hello *Azure Active Directory v2 endpoint* -som aktiverar personliga, skolan och arbetsrelaterade konton toosign i och hämta token. Hej *Azure Active Directory v2 endpoint* har [vissa begränsningar](..\active-directory-v2-limitations.md). Om du bara vill skolan och arbetsrelaterade konton använder *adal.js* och hello *V1 endpoint*. toounderstand skillnaderna mellan hello v1 och v2 slutpunkter läsa hello [v1 v2 jämförelse](..\active-directory-v2-compare.md).

<!--end-collapse-->
