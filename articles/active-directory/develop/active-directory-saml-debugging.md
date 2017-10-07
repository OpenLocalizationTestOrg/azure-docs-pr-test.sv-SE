---
title: aaaHow toodebug SAML-baserade enkel inloggning tooapplications i Azure Active Directory | Microsoft Docs
description: "Lär dig hur toodebug SAML-baserade enkel inloggning tooapplications i Azure Active Directory "
services: active-directory
author: asmalser-msft
documentationcenter: na
manager: femila
ms.assetid: edbe492b-1050-4fca-a48a-d1fa97d47815
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/20/2017
ms.author: asmalser
ms.custom: aaddev
ms.reviewer: dastrock
ms.openlocfilehash: 846c7b3497c8842947c5b406f4362b9e06785b14
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toodebug-saml-based-single-sign-on-tooapplications-in-azure-active-directory"></a>Hur toodebug SAML-baserade enkel inloggning tooapplications i Azure Active Directory
När du felsöker en SAML-baserade program-integrering är ofta användbara toouse ett verktyg som [Fiddler](http://www.telerik.com/fiddler) toosee hello SAML begäran hello SAML-svar och hello faktiska SAML-token som utfärdas toohello program. Genom att undersöka hello SAML-token, se till att alla hello begärda attribut, hello användarnamn i hello SAML ämne och hello utfärdaren URI kommer via som förväntat.

![][1]

hello svaret från Azure AD som innehåller hello SAML-token är vanligtvis hello som inträffar efter att en HTTP 302-omdirigering från https://login.windows.net och konfigureras skickade toohello **Reply URL** av programmet hello. 

Du kan visa hello SAML-token genom att välja den här raden och sedan välja hello **kontrollanter > WebForms** fliken i hello högra panelen. Högerklicka på hello därifrån **SAMLResponse** värdet och välj **skicka tooTextWizard**. Välj sedan **från Base64** från hello **transformera** menyn toodecode hello token och se innehållet.

**Obs**: toosee hello innehållet i den här HTTP-begäran, Fiddler kan medföra att du tooconfigure dekryptering av HTTPS-trafik som du behöver toodo.

## <a name="related-articles"></a>Relaterade artiklar
* [Artikelindex för programhantering i Azure Active Directory](../active-directory-apps-index.md)
* [Konfigurera enkel inloggning tooapplications som inte ingår i hello Azure Active Directory-programgalleriet](../active-directory-saas-custom-apps.md)
* [Hur tooCustomize anspråk som utfärdats i hello SAML-Token för Pre-Integrated appar](active-directory-saml-claims-customization.md)

<!--Image references-->
[1]: ../media/active-directory-saml-debugging/fiddler.png