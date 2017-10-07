---
title: "aaaCustomizing anspråk som utfärdats i hello SAML-token för förintegrerade appar i Azure Active Directory | Microsoft Docs"
description: "Lär dig hur toocustomize hello anspråk utfärdas i hello SAML-token för förintegrerade appar i Azure Active Directory"
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: f1daad62-ac8a-44cd-ac76-e97455e47803
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jeedes
ms.custom: aaddev
ms.openlocfilehash: a376318929472403e799f02fdd3fbddc91d0a70c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="customizing-claims-issued-in-hello-saml-token-for-pre-integrated-apps-in-azure-active-directory"></a>Anpassa anspråk som utfärdats i hello SAML-token för förintegrerade appar i Azure Active Directory
Idag Azure Active Directory har stöd för tusentals förintegrerade program i hello Azure AD Application Gallery, inklusive över 360 som har stöd för enkel inloggning med hello SAML 2.0-protokollet. När en användare autentiseras tooan program via Azure AD med SAML skickar ett token toohello program (via en HTTP POST) i Azure AD. Och sedan programmet hello validerar och använder hello token toolog hello användare i i stället för att fråga efter användarnamn och lösenord. Dessa SAML-token innehålla uppgifter om hello användare som kallas ”anspråk”.

I identity-tala är ett ”anspråk” information som en identitetsleverantör tillstånd om en användare i hello-token som de utfärdar för användaren. I [SAML-token](http://en.wikipedia.org/wiki/SAML_2.0), dessa data finns vanligtvis i hello SAML Attribute-uttryck. hello är användarens unika ID: T vanligtvis representeras i hello SAML ämne även kallas namnidentifierare.

Azure Active Directory utfärdar en SAML-token tooyour program som innehåller ett NameIdentifier-anspråk med ett värde av hello användarens användarnamn (AKA användarens huvudnamn) i Azure AD som standard. Det här värdet kan identifiera hello användare. hello SAML-token innehåller även ytterligare anspråk som innehåller hello användarens e-postadress, Förnamn och efternamn.

tooview eller redigera hello anspråk som utfärdats i hello SAML-token toohello program, öppna hello i Azure-portalen. Välj hello **visa och redigera andra användarattribut** kryssrutan i hello **användarattribut** avsnitt av programmet hello.

![Användaren attribut avsnitt][1]

Det finns två möjliga orsaker till varför du kanske behöver tooedit hello anspråk som utfärdats i hello SAML-token:
* programmet hello har skrivits toorequire en annan uppsättning anspråk URI: er eller anspråksvärden.
* hello-programmet har distribuerats på ett sätt som kräver hello NameIdentifier anspråk toobe något annat än hello användarnamn (AKA användarens huvudnamn) lagras i Azure Active Directory.

Du kan redigera hello standardvärden för anspråk. Välj hello anspråk raden i tabellen för hello SAML-token attribut. Då öppnas hello **Redigera attribut** avsnittet och du sedan kan redigera anspråkets namn, värde och namnområde som hör till hello anspråk.

![Redigera användarattribut][2]

Du kan också ta bort anspråk (andra än NameIdentifier) med hello snabbmeny som öppnas genom att klicka på hello **...**  ikon.  Du kan också lägga till nya anspråk med hello **Lägg till attributet** knappen.

![Redigera användarattribut][3]

## <a name="editing-hello-nameidentifier-claim"></a>Redigera hello NameIdentifier anspråk
toosolve hello problem där hello programmet har distribuerats med hjälp av ett annat användarnamn klickar du på hello **användar-ID** listrutan i hello **användarattribut** avsnitt. Den här åtgärden visar en dialogruta med flera olika alternativ:

![Redigera användarattribut][4]

I hello listrutan, Välj **user.mail** tooset hello NameIdentifier anspråk toobe hello användarens e-postadress i hello directory. Eller välj **user.onpremisessamaccountname** tooset toohello användarens SAM-kontonamn som har synkroniserats från lokala Azure AD.

Du kan också använda hello särskilda **ExtractMailPrefix()** tooremove funktionen hello domänsuffix från hello e-postadress, SAM-kontonamn eller hello huvudnamn. Då extraheras endast hello första delen av hello användaren namn som skickas via (till exempel ”joe_smith” i stället för joe_smith@contoso.com).

![Redigera användarattribut][5]

Vi har nu lagt till hello **join()** funktionen toojoin hello verifierade domän med hello user ID-värde. När du väljer hello join() funktionen i hello **användar-ID** först välja hello användar-ID som t.ex. e-postadress eller användaren huvudnamn och välj sedan din verifierade domän i hello andra listrutan. Om du markerar hello e-postadress med hello verifierad domän och Azure AD extraherar hello användarnamn från hello första värde joe_smith från joe_smith@contoso.com och lägger till dem med contoso.onmicrosoft.com. Se följande exempel hello:

![Redigera användarattribut][6]

## <a name="adding-claims"></a>Lägga till anspråk
När du lägger till ett anspråk kan du ange hello attributnamn (som endast inte behöver toofollow ett URI-mönster enligt hello SAML-specifikationen). Attributet hello värdet tooany användare som är lagrad i hello directory.

![Lägg till användarattribut][7]

Till exempel måste toosend hello avdelningen som hello användaren tillhör tooin organisationen som ett anspråk (till exempel försäljning). Ange hello anspråkets namn som förväntades av programmet hello och välj sedan **user.department** som hello-värde.

> [!NOTE]
> Om det finns inget värde som lagras i ett valt attribut för en viss användare, är inte det anspråket som utfärdas i hello token.

> [!TIP]
> Hej **user.onpremisesecurityidentifier** och **user.onpremisesamaccountname** stöds endast när synkronisering av användardata från lokala Active Directory med hello [Azure AD Connect-verktyget](../active-directory-aadconnect.md).

## <a name="restricted-claims"></a>Begränsat anspråk

Det finns vissa begränsade anspråk i SAML. Om du lägger till dessa anspråk skickas dessa anspråk inte i Azure AD. Följande är hello SAML begränsad anspråksuppsättning:

    | Anspråkstyp (URI) |
    | ------------------- |
    | http://schemas.microsoft.com/ws/2008/06/Identity/Claims/Expiration |
    | http://schemas.microsoft.com/ws/2008/06/Identity/Claims/EXPIRED |
    | http://schemas.microsoft.com/Identity/Claims/accesstoken |
    | http://schemas.microsoft.com/Identity/Claims/openid2_id |
    | http://schemas.microsoft.com/Identity/Claims/identityprovider |
    | http://schemas.microsoft.com/Identity/Claims/objectidentifier |
    | http://schemas.microsoft.com/Identity/Claims/PUID |
    | http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/NameIdentifier [MR1] |
    | http://schemas.microsoft.com/Identity/Claims/tenantid |
    | http://schemas.microsoft.com/ws/2008/06/Identity/Claims/AuthenticationInstant |
    | http://schemas.microsoft.com/ws/2008/06/Identity/Claims/AuthenticationMethod |
    | http://schemas.microsoft.com/AccessControlService/2010/07/Claims/identityprovider |
    | http://schemas.microsoft.com/ws/2008/06/Identity/Claims/groups |
    | http://schemas.microsoft.com/Claims/groups.Link |
    | http://schemas.microsoft.com/ws/2008/06/Identity/Claims/role |
    | http://schemas.microsoft.com/ws/2008/06/Identity/Claims/wids |
    | http://schemas.microsoft.com/2014/09/devicecontext/Claims/iscompliant |
    | http://schemas.microsoft.com/2014/02/devicecontext/Claims/isknown |
    | http://schemas.microsoft.com/2012/01/devicecontext/Claims/ismanaged |
    | http://schemas.microsoft.com/2014/03/psso |
    | http://schemas.microsoft.com/Claims/authnmethodsreferences |
    | http://schemas.xmlsoap.org/ws/2009/09/Identity/Claims/Actor |
    | http://schemas.microsoft.com/ws/2008/06/Identity/Claims/samlissuername |
    | http://schemas.microsoft.com/ws/2008/06/Identity/Claims/confirmationkey |
    | http://schemas.microsoft.com/ws/2008/06/Identity/Claims/windowsaccountname |
    | http://schemas.microsoft.com/ws/2008/06/Identity/Claims/primarygroupsid |
    | http://schemas.microsoft.com/ws/2008/06/Identity/Claims/primarysid |
    | http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/authorizationdecision |
    | http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/Authentication |
    | http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/SID |
    | http://schemas.microsoft.com/ws/2008/06/Identity/Claims/denyonlyprimarygroupsid |
    | http://schemas.microsoft.com/ws/2008/06/Identity/Claims/denyonlyprimarysid |
    | http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/denyonlysid |
    | http://schemas.microsoft.com/ws/2008/06/Identity/Claims/denyonlywindowsdevicegroup |
    | http://schemas.microsoft.com/ws/2008/06/Identity/Claims/windowsdeviceclaim |
    | http://schemas.microsoft.com/ws/2008/06/Identity/Claims/windowsdevicegroup |
    | http://schemas.microsoft.com/ws/2008/06/Identity/Claims/windowsfqbnversion |
    | http://schemas.microsoft.com/ws/2008/06/Identity/Claims/windowssubauthority |
    | http://schemas.microsoft.com/ws/2008/06/Identity/Claims/windowsuserclaim |
    | http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/x500distinguishedname |
    | http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/UPN |
    | http://schemas.microsoft.com/ws/2008/06/Identity/Claims/GroupSID |
    | http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/SPN |
    | http://schemas.microsoft.com/ws/2008/06/Identity/Claims/ispersistent |
    | http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/privatepersonalidentifier |
    | http://schemas.microsoft.com/Identity/Claims/scope |

## <a name="next-steps"></a>Nästa steg
* [Artikelindex för programhantering i Azure Active Directory](../active-directory-apps-index.md)
* [Konfigurera enkel inloggning tooapplications som inte ingår i hello Azure Active Directory-programgalleriet](../active-directory-saas-custom-apps.md)
* [Felsökning av SAML-baserade enkel inloggning](active-directory-saml-debugging.md)

<!--Image references-->
[1]: ./media/active-directory-saml-claims-customization/user-attribute-section.png
[2]: ./media/active-directory-saml-claims-customization/edit-claim-name-value.png
[3]: ./media/active-directory-saml-claims-customization/delete-claim.png
[4]: ./media/active-directory-saml-claims-customization/user-identifier.png
[5]: ./media/active-directory-saml-claims-customization/extractemailprefix-function.png
[6]: ./media/active-directory-saml-claims-customization/join-function.png
[7]: ./media/active-directory-saml-claims-customization/add-attribute.png