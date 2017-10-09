---
title: "aaaProblem konfigurera federerad enkel inloggning för ett icke-galleriet program | Microsoft Docs"
description: "Vissa av hello vanliga problem som kan uppstå när du konfigurerar federerade tooyour för enkel inloggning anpassat SAML program som inte finns med i hello Azure AD Application Gallery"
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
ms.openlocfilehash: 8c80f0001de01cbc9930ef0441cd804082ee8578
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="problem-configuring-federated-single-sign-on-for-a-non-gallery-application"></a>Konfigurera federerad enkel inloggning för ett icke-galleriet program problem

Om du stöter på problem när du konfigurerar ett program. Kontrollera att du har följt alla hello stegen i artikeln hello [Konfigurera enkel inloggning tooapplications som inte ingår i hello Azure Active Directory-programgalleriet.](https://docs.microsoft.com/azure/active-directory/active-directory-saas-custom-apps)

## <a name="cant-add-another-instance-of-hello-application"></a>Det går inte att lägga till en annan instans av programmet hello

tooadd en andra instans av ett program måste toobe kunna:

-   Konfigurera en unik identifierare för andra instans av hello. Du kommer inte att kunna tooconfigure hello samma ID som används för hello första instansen.

-   Konfigurera ett annat certifikat än hello som används för hello första instansen.

Om hello stöder programmet inte någon av hello ovan. Du kan sedan inte kan tooconfigure en andra instans.

## <a name="where-do-i-set-hello-entityid-user-identifier-format"></a>Där anger hello ID för entiteterna (användar-ID)-formatet

Du kommer inte att kunna tooselect hello ID för entiteterna (användar-ID) format att Azure AD skickar toohello programmet hello svar efter autentisering av användare.

Azure AD väljer hello format för hello NameID attribut (användar-ID) baserat på valda hello värdet eller hello format som begärs av programmet hello i hello SAML AuthRequest. Mer information finns i artikeln hello [enkel inloggning SAML protokoll](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) under hello avsnittet NameIDPolicy,

## <a name="where-do-i-get-hello-application-metadata-or-certificate-from-azure-ad"></a>Var hittar jag hello programmetadata eller certifikat från Azure AD

toodownload hello programmetadata eller certifikat från Azure AD gör hello nedan:

1.  Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**

2.  Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.

3.  Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.

4.  Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.

5.  Klicka på **alla program** tooview en lista över alla program.

   * Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**

6.  Välj hello-program som du har konfigurerat för enkel inloggning.

7.  När programmet hello läses in klickar du på hello **enkel inloggning** från hello programmet vänstra navigeringsmenyn.

8.  Gå för**SAML-signeringscertifikat** avsnittet och klicka sedan på **hämta** värde i kolumnen. Beroende på vilka hello-programmet kräver Konfigurera enkel inloggning, kan du se antingen hello alternativet toodownload hello XML-Metadata eller hello certifikat.

Azure AD Ange inte en URL tooget hello metadata. hello metadata kan endast hämtas som en XML-fil.

## <a name="dont-know-how-toocustomize-saml-claims-sent-tooan-application"></a>Vet inte hur toocustomize SAML anspråk skickas tooan program

toolearn hur toocustomize hello SAML attributet anspråk skickas tooyour program finns i [anspråk mappning i Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) för mer information.

## <a name="next-steps"></a>Nästa steg
[Hantera program med Azure Active Directory](active-directory-enable-sso-scenario.md)
