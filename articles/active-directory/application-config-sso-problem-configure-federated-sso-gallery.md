---
title: "Konfigurera federerad enkel inloggning för ett program för Azure AD-galleriet aaaProblem | Microsoft Docs"
description: "Vissa av hello vanliga problem som kan uppstå när du konfigurerar federerad enkel inloggning med SAML för program som listas i hello Azure AD Application Gallery"
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
ms.openlocfilehash: 2ae1e511bd49b19159e1ab83cf79a7db5403b9a1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="problem-configuring-federated-single-sign-on-for-an-azure-ad-gallery-application"></a>Problem som konfigurerar federerad enkel inloggning för ett program för Azure AD-galleriet

Om du stöter på problem när du konfigurerar ett program. Kontrollera att du har följt alla steg i hello i hello självstudierna för hello program. I hello programmets konfiguration har infogad dokumentation om hur tooconfigure hello program. Du kan också komma åt hello [lista över självstudier om hur toointegrate SaaS-appar med Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) en stegvisa instruktioner för detaljer.

## <a name="cant-add-another-instance-of-hello-application"></a>Det går inte att lägga till en annan instans av programmet hello

tooadd en andra instans av ett program måste toobe kunna:

-   Konfigurera en unik identifierare för andra instans av hello. Du kommer inte att kunna tooconfigure hello samma ID som används för hello första instansen.

-   Konfigurera ett annat certifikat än hello som används för hello första instansen.

Om hello stöder programmet inte någon av hello ovan. Du kan sedan inte kan tooconfigure en andra instans.

## <a name="cant-add-hello-identifier-or-hello-reply-url"></a>Det går inte att lägga till hello identifierare eller hello Reply-URL

Om du inte kan tooconfigure hello identifierare eller hello Reply URL, bekräfta hello identifierare och Reply URL-värdena matchar hello mönster förkonfigurerade för hello program.

tooknow hello mönster förkonfigurerade för programmet hello:

1.  Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.** Gå toostep 7. Om du redan hello programmet configuration bladet på Azure AD.

2.  Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.

3.  Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.

4.  Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.

5.  Klicka på **alla program** tooview en lista över alla program.

   * Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**

6.  Välj hello-program som du vill tooconfigure enkel inloggning.

7.  När programmet hello läses in klickar du på hello **enkel inloggning** från hello programmet vänstra navigeringsmenyn.

8.  Välj **SAML-baserade inloggning** från hello **läge** listrutan.

9.  Gå toohello **identifierare** eller **Reply URL** textruta under hello **domän och URL: er.**

10. Det finns tre sätt tooknow hello stöds mönster för programmet hello:

   * I hello textruta hello stöds pattern(s) visas som platshållare *exempel:* <https://contoso.com>.

   * Om mönstret hello inte stöds, visas ett rött utropstecken när du försöker tooenter hello värdet i textrutan hello. Om du håller muspekaren över hello rött utropstecken visas hello stöds mönster.

   * Du kan också få information om hello stöds i hello självstudierna för hello program. Under hello **konfigurera Azure AD enkel inloggning** avsnitt. Gå toohello steg för konfigurerade hello värden under hello **domän och URL: er** avsnitt.

Om hello värden stämmer inte överens med hello mönster förkonfigurerade på Azure AD. Du kan:

-   Arbeta med hello programmet leverantör tooget värden som matchar mönstret för hello förkonfigurerade på Azure AD

-   Eller kontakta Azure AD-teamet på < aadapprequest@microsoft.com > eller lämna en kommentar i hello självstudiekursen toorequest hello uppdatering av hello stöds mönster för hello program

## <a name="where-do-i-set-hello-entityid-user-identifier-format"></a>Där anger hello ID för entiteterna (användar-ID)-formatet

Du kommer inte att kunna tooselect hello ID för entiteterna (användar-ID) format att Azure AD skickar toohello programmet hello svar efter autentisering av användare.

Azure AD väljer hello format för hello NameID attribut (användar-ID) baserat på valda hello värdet eller hello format som begärs av programmet hello i hello SAML AuthRequest. Mer information finns i artikeln hello [enkel inloggning SAML protokoll](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) under hello avsnittet NameIDPolicy,

## <a name="cant-find-hello-azure-ad-metadata-toocomplete-hello-configuration-with-hello-application"></a>Det går inte att hitta hello Azure AD metadata toocomplete hello konfigurationsfilen med hello program

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
