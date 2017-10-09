---
title: "aaaManage enkel inloggning för Azure AD Application Proxy | Microsoft Docs"
description: "Lär dig mer om hello grunderna för enkel inloggning med Application Proxy"
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/23/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: a278751a5cb1bf98c970a4e5d2eb3edc3b784096
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-does-azure-ad-application-proxy-provide-single-sign-on"></a>Hur tillhandahåller Azure AD Application Proxy enkel inloggning?

Enkel inloggning är en nyckelfaktor för Azure AD Application Proxy.  Det ger hello bästa användarupplevelsen eftersom användarna bara har toosign i tooAzure Active Directory i hello molnet. När de autentiserar tooAzure Active Directory hanterar hello Application Proxy connector hello autentisering toohello lokalt program. hello backend-programmet kan inte se hello skillnaden mellan en fjärransluten användare logga in via Application Proxy och en vanlig användning på en domänansluten enhet. 

toouse Azure Active Directory för enkel inloggning tooyour program behöver du tooselect **Azure Active Directory** som hello före autentiseringsmetod. Om du väljer **Passthrough** användarna inte autentisera tooAzure Active Directory alls, men är riktat raka toohello program. Du kan konfigurera den här inställningen när du först publicera ett program eller navigera tooyour programmet hello Azure-portalen och redigera hello Application Proxy-inställningar. 

toosee enkel inloggning alternativen, gör du följande:

1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Navigera för**Azure Active Directory** > **företagsprogram** > **alla program**.
3. Välj hello app vars enkel inloggning alternativen du vill toomanage.
4. Välj **enkel inloggning**.

   ![Listrutan för enkel inloggning](./media/application-proxy-sso-overview/single-sign-on-mode.png)

hello listrutan visas fem alternativ för enkel inloggning tooyour program:

* Azure AD enkel inloggning inaktiverad
* Lösenordsbaserade inloggning
* Länkade inloggning
* Integrerad Windows-autentisering
* Rubrik-baserade inloggning

## <a name="azure-ad-single-sign-on-disabled"></a>Azure AD enkel inloggning inaktiverad

Om du inte vill toouse Azure Active Directory-integrering för enkel inloggning tooyour program, Välj **Azure AD enkel inloggning inaktiverat**. Med det här alternativet kan dina användare kan autentiseras två gånger. Först de autentisera tooAzure Active Directory och logga sedan in själva toohello programmet. 

Det här alternativet är bra om lokala program kräver inte användare tooauthenticate, men du vill ha tooadd Azure Active Directory som en säkerhetsnivå för fjärråtkomst. 

## <a name="password-based-sign-on"></a>Lösenordsbaserade inloggning

Om du vill toouse Azure Active Directory som ett valv lösenord för lokala program väljer **lösenordsbaserade inloggning**. Det här alternativet är bra om programmet autentiserar med användarnamn/lösenord kombinationsruta i stället för åtkomst-token eller huvuden. Med lösenordsbaserade inloggning måste användarna toosign i toohello programmet hello första gången de komma åt den. Efter det tillhandahåller Azure Active Directory hello användarnamn och lösenord för hello användares räkning. 

Information om hur du konfigurerar lösenordsbaserade inloggning finns i [lösenord vaulting för enkel inloggning med Application Proxy](application-proxy-sso-azure-portal.md).

## <a name="linked-sign-on"></a>Länkade inloggning

Om du redan har en enkel inloggning lösning har ställts in för dina lokala identiteter väljer **länkade inloggning**. Det här alternativet kan Azure Active Directory tooleverage befintliga SSO lösningar, men fortfarande ger användarna fjärråtkomst toohello program. 

Information om länkade inloggning (tidigare kallat befintliga enkel inloggning) finns [vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work).

## <a name="integrated-windows-authentication"></a>Integrerad Windows-autentisering

Välj om lokala program använder integrerad Windows-Authentication(IWA) eller om du vill toouse Kerberos-begränsad delegering (KCD) för enkel inloggning **integrerad Windows-autentisering**. Användarna behöver bara tooauthenticate tooAzure Active Directory med det här alternativet och sedan hello Application Proxy connector personifierar hello användaren tooget Kerberos-token och logga in toohello program. 

Information om hur du konfigurerar integrerad Windows-autentisering finns i [Kerberos-begränsad delegering för enkel inloggning med Application Proxy](active-directory-application-proxy-sso-using-kcd.md).

## <a name="header-based-sign-on"></a>Rubrik-baserade inloggning 

Om dina program använder huvuden för autentisering, Välj **huvud-baserade inloggning**. Med det här alternativet behöver användarna bara tooauthentication hello Azure Active Directory. Microsoft-partner med en autentiseringstjänst från tredje part kallas PingAccess som översättas hello Azure Active Directory-åtkomsttoken till en huvudformat för hello program. 

Information om hur du konfigurerar huvud-baserad autentisering finns [huvud-baserad autentisering för enkel inloggning med Application Proxy](application-proxy-ping-access.md).

## <a name="next-steps"></a>Nästa steg

- [Lösenord vaulting för enkel inloggning med Application Proxy](application-proxy-sso-azure-portal.md)
- [Kerberos-begränsad delegering för enkel inloggning med Application Proxy](active-directory-application-proxy-sso-using-kcd.md)
- [Rubrik-baserad autentisering för enkel inloggning med Application Proxy](application-proxy-ping-access.md) 
