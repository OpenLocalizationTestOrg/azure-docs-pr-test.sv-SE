---
title: aaaAzure Active Directory v2.0-slutpunkten | Microsoft Docs
description: "En introduktion toobuilding appar med både Account och Azure Active Directory-inloggning."
services: active-directory
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 2dee579f-fdf6-474b-bc2c-016c931eaa27
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/01/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: ae5946b02c50ae5a6137dc1decae66c96647e875
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="sign-in-microsoft-account--azure-ad-users-in-a-single-app"></a>Logga in Account & Azure AD-användare i en enda app
I hello efter en app-utvecklare som vill toosupport både personliga Microsoft-konton och arbetskonton från Azure Active Directory har nödvändiga toointegrate med två separata system.  Hej **Azure AD v2.0-slutpunkten** introducerar en ny autentisering-API-version som du kan använda toosign i båda typerna av konton med hjälp av en enkel integrering.  Appar som använder hello v2.0-slutpunkten kan också använda REST API: er från hello [Microsoft Graph](https://graph.microsoft.io) med hjälp av någon typ av konto.

## <a name="getting-started"></a>Komma igång
Välj din favoritplattform från följande lista toobuild hello en app med våra bibliotek med öppen källkod och ramverk.  Du kan också använda vår OAuth 2.0 & OpenID Connect protokollet dokumentationen toosend och ta emot protokollmeddelanden direkt utan att använda ett auth-bibliotek.

<br />

[!INCLUDE [active-directory-v2-quickstart-table](../../../includes/active-directory-v2-quickstart-table.md)]

## <a name="whats-new"></a>Senaste nytt
hello information här kan vara användbart i förstå vad & vad är inte möjlig med hello v2.0-slutpunkten.

* Lär dig mer om hello [typer av appar som du kan skapa med hello v2.0-slutpunkten](active-directory-v2-flows.md).
* Förstå hello [begränsningar, restriktioner och villkor](active-directory-v2-limitations.md) med hello v2.0-slutpunkten.
* Checka ut den här översikten för hello v2.0-slutpunkten:

>[!VIDEO https://channel9.msdn.com/Events/Build/2017/P4031/player]

## <a name="reference"></a>Referens
Dessa länkar kan vara användbart för att utforska hello-plattformen i mer detalj:

* [Referens för v2.0-protokollet](active-directory-v2-protocols.md)
* [Referens för v2.0-Token](active-directory-v2-tokens.md)
* [Referens för v2.0-bibliotek](active-directory-v2-libraries.md)
* [Scope och medgivande i hello v2.0-slutpunkten](active-directory-v2-scopes.md)
* [hello Microsoft Graph](https://graph.microsoft.io)

## <a name="help--support"></a>Hjälp och support
Dessa är hello bästa platser tooget hjälp med att utveckla på Azure Active Directory.

* [Stackspill`azure-active-directory` och `adal`taggar](http://stackoverflow.com/questions/tagged/azure-active-directory+or+adal)
* [Feedback om Azure Active Directory](https://feedback.azure.com/forums/169401-azure-active-directory/category/164757-developer-experiences)


> [!NOTE]
> Om du bara behöver toosign arbets- och skolkonton konton från Azure Active Directory ska du starta med vår [Azure AD-guide för utvecklare](active-directory-developers-guide.md).  hello v2.0-slutpunkten är avsedd att användas av utvecklare som uttryckligen måste toosign i personliga Microsoft-konton.

