---
title: "Problem som konfigurerar federerad enkel inloggning för ett icke-galleriet program | Microsoft Docs"
description: "Vissa av de vanliga problem som kan uppstå när du konfigurerar federerad enkel inloggning till din anpassade SAML-program som inte finns med i Azure AD Application Gallery"
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
ms.openlocfilehash: 4eb2c04c940dd6ad15a491a331aed76c237f0387
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="problem-configuring-federated-single-sign-on-for-a-non-gallery-application"></a>Konfigurera federerad enkel inloggning för ett icke-galleriet program problem

Om du stöter på problem när du konfigurerar ett program. Kontrollera att du har följt alla steg i artikeln [Konfigurera enkel inloggning för program som inte ingår i Azure Active Directory-programgalleriet.](https://docs.microsoft.com/azure/active-directory/active-directory-saas-custom-apps)

## <a name="cant-add-another-instance-of-the-application"></a>Det går inte att lägga till en annan instans av programmet

Om du vill lägga till en andra instans av ett program, måste du kunna:

-   Konfigurera en unik identifierare för den andra instansen. Du kan inte konfigurera den samma identifierare som används för den första instansen.

-   Konfigurera ett annat certifikat än den som används för den första instansen.

Om programmet inte stöder någon av ovanstående. Sedan kan du inte konfigurera en andra instans.

## <a name="where-do-i-set-the-entityid-user-identifier-format"></a>Där anger formatet ID för entiteterna (användar-ID)

Du kommer inte att kunna välja det ID för entiteterna (användar-ID)-format som Azure AD skickar till programmet i svaret efter autentisering av användare.

Azure AD-Välj format för attributet NameID (användar-ID) baserat på värdet valt eller formatet programmet har begärt i SAML-AuthRequest. Mer information finns i artikeln [enkel inloggning SAML protokoll](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) under avsnittet NameIDPolicy,

## <a name="where-do-i-get-the-application-metadata-or-certificate-from-azure-ad"></a>Var hittar jag programmetadata eller certifikat från Azure AD

Följ stegen nedan för att ladda ned programmetadata eller certifikat från Azure AD:

1.  Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**

2.  Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.

3.  Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.

4.  Klicka på **företagsprogram** från Azure Active Directory vänstra navigeringsmenyn.

5.  Klicka på **alla program** att visa en lista över alla program.

   * Om du inte ser programmet som du vill visa här använder du den **Filter** kontrollen längst upp i den **listan med alla program** och ange den **visa** att **alla program.**

6.  Välj det program som du har konfigurerat för enkel inloggning.

7.  När programmet läses in klickar du på den **enkel inloggning** från programmets vänstra navigeringsmenyn.

8.  Gå till **SAML-signeringscertifikat** avsnittet och klicka sedan på **hämta** värde i kolumnen. Beroende på vilka programmet kräver att konfigurera enkel inloggning, finns antingen alternativet för att hämta Metadata XML eller certifikatet.

Azure AD Ange inte en URL för att hämta metadata. Metadata kan endast hämtas som en XML-fil.

## <a name="dont-know-how-to-customize-saml-claims-sent-to-an-application"></a>Vet inte hur du anpassar SAML anspråk skickas till ett program

Information om hur du anpassar SAML attributet anspråk som skickas till ditt program finns [anspråk mappning i Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) för mer information.

## <a name="next-steps"></a>Nästa steg
[Hantera program med Azure Active Directory](active-directory-enable-sso-scenario.md)
