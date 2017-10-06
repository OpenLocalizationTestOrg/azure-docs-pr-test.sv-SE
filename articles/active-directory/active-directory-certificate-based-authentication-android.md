---
title: "aaaAzure Active Directory certifikatbaserad autentisering på Android | Microsoft Docs"
description: "Lär dig mer om hello stöds scenarier och hello kraven för att konfigurera certifikatbaserad autentisering i lösningar med Android-enheter"
services: active-directory
author: MarkusVi
documentationcenter: na
manager: femila
ms.assetid: c6ad7640-8172-4541-9255-770f39ecce0e
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/28/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 148275fa3da610530c278fcd57e02e907f735d9a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-certificate-based-authentication-on-android"></a>Azure Active Directory certifikatbaserad autentisering på Android


Certifikatbaserad autentisering (CBA) kan du toobe autentiseras av Azure Active Directory med ett klientcertifikat på en Windows-, Android eller iOS-enhet när du ansluter din Exchange-onlinekonto till: 

* Mobila Office-program, till exempel Microsoft Outlook och Microsoft Word   
* Exchange ActiveSync (EAS) klienter 

Konfigurera den här funktionen eliminerar hello måste tooenter användarnamnet och lösenordet till vissa e-post och Microsoft Office-program på din mobila enhet. 

Det här avsnittet ger dig hello krav och hello stöds scenarier för att konfigurera CBA på en iOS(Android) enhet för användare av klienter i Office 365 Enterprise, företag, Education, som tillhör amerikanska myndigheter, Kina och Tyskland planer.



Den här funktionen är tillgängliga i förhandsversionen i Office 365 US Government skydd och federala planer.


## <a name="office-mobile-applications-support"></a>Stöd för mobila Office-program
| Program | Support |
| --- | --- |
| Azure Information Protection-app |![Markera][1] |
| Microsoft Teams |![Markera][1] |
| OneNote |![Markera][1] |
| OneDrive |![Markera][1] |
| Outlook |![Markera][1] |
| Power BI-appar |![Markera][1] |
| Skype för företag |![Markera][1] |
| Word / Excel / PowerPoint |![Markera][1] |
| Yammer |![Markera][1] |


### <a name="implementation-requirements"></a>Implementeringskrav för

hello enhetens OS-version måste vara Android 5.0 (Lollipop) och senare. 

En federationsserver konfigureras.  

För Azure Active Directory toorevoke ett klientcertifikat har hello AD FS-token hello följande krav:  

* `http://schemas.microsoft.com/ws/2008/06/identity/claims/<serialnumber>`  
  (hello serienumret för hello klientcertifikat) 
* `http://schemas.microsoft.com/2012/12/certificatecontext/field/<issuer>`  
  (hello sträng för hello utfärdaren av klientcertifikatet hello) 

Azure Active Directory lägger till dessa anspråk toohello uppdateringstoken om de är tillgängliga i hello AD FS-token (eller andra SAML-token). När hello uppdateringstoken måste toobe verifieras, är den här informationen används toocheck hello återkallade certifikat. 

Som bästa praxis bör du uppdatera hello ADFS-felsidor med instruktioner om hur tooget ett användarcertifikat.  
Mer information finns i [anpassa hello AD FS Sign in sidor](https://technet.microsoft.com/library/dn280950.aspx).  

Vissa Office-appar (med modern autentisering är aktiverat) skicka '*uppmana = inloggningen*' tooAzure AD i sin begäran. Standard Azure AD omvandlas i hello begäran tooADFS för '*wauth = usernamepassworduri*' (frågar ADFS toodo U/P auth) och '*wfresh = 0*' (frågar ADFS tooignore SSO tillstånd och gör en ny autentisering) . Om du vill tooenable certifikatbaserad autentisering för dessa appar, måste toomodify hello Azure AD standardbeteendet. Bara ange hello '*PromptLoginBehavior*' i inställningarna för federerade domänen för '*inaktiverad*'. Du kan använda hello [MSOLDomainFederationSettings](/powershell/module/msonline/set-msoldomainfederationsettings?view=azureadps-1.0) cmdlet tooperform aktiviteten:

`Set-MSOLDomainFederationSettings -domainname <domain> -PromptLoginBehavior Disabled`



## <a name="exchange-activesync-clients-support"></a>Stöd för Exchange ActiveSync-klienter
Vissa Exchange ActiveSync-program på Android 5.0 (Lollipop) eller senare stöds. toodetermine om e-postprogrammet stöder den här funktionen kan du kontakta din programutvecklaren. 


## <a name="next-steps"></a>Nästa steg

Om du vill tooconfigure certifikatbaserad autentisering i din miljö, se [komma igång med certifikatbaserad autentisering på Android](active-directory-certificate-based-authentication-get-started.md) anvisningar.

<!--Image references-->
[1]: ./media/active-directory-certificate-based-authentication-android/ic195031.png
