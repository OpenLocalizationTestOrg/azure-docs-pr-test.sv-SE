---
title: "aaaSingle inloggning hantering för företagsappar i hello Azure Active Directory | Microsoft Docs"
description: "Lär dig hur toomanage för enkel inloggning för företagsappar som använder hello Azure Active Directory"
services: active-directory
documentationcenter: 
author: asmalser
manager: femila
editor: 
ms.assetid: bcc954d3-ddbe-4ec2-96cc-3df996cbc899
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/26/2017
ms.author: asmalser
ms.openlocfilehash: b0a8e622ab10517b7b69f786406b6e9b9f2e7eaa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="managing-single-sign-on-for-enterprise-apps"></a>Hantera enkel inloggning för företagsappar i
> [!div class="op_single_selector"]
> * [Azure Portal](active-directory-enterprise-apps-manage-sso.md)
> * [Klassisk Azure-portal](active-directory-sso-integrate-saas-apps.md)
> 

Den här artikeln beskriver hur toouse hello [Azure-portalen](https://portal.azure.com) toomanage enkel inloggning inställningar för företagsprogram. Enterprise-appar är appar som har distribuerats och används i din organisation. Den här artikeln gäller särskilt tooapps som har lagts till från hello [Azure Active Directory-programgalleriet](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery). 

## <a name="finding-your-apps-in-hello-portal"></a>Hitta dina appar i hello-portalen
Alla program som har ställts in för enkel inloggning kan visas och hanteras i hello Azure-portalen. hello program finns i hello **fler tjänster** &gt; **företagsprogram** på hello portal. 

![Företagsprogram bladet][1]

Välj **alla program** tooview en lista över alla appar som har konfigurerats. Markera en app laddas hello resursbladet för appen, där rapporter kan visas för appen och en rad olika inställningar som kan hanteras.

toomanage enkel inloggning inställningar, Välj **enkel inloggning**.

![Programmet resursbladet][2]

## <a name="single-sign-on-modes"></a>Lägen för enkel inloggning
Hej **enkel inloggning** bladet börjar med en **läge** menyn där hello läget för enkel inloggning toobe konfigurerats. hello tillgängliga alternativ inkluderar:

* **SAML-baserade inloggning** -alternativet är tillgängligt om hello program som stöder fullständig federerad enkel inloggning med Azure Active Directory med hello SAML 2.0-protokollet.
* **Lösenordsbaserade inloggning** -det här alternativet är tillgängligt om Azure AD stöder ifyllning av lösenordet för det här programmet.
* **Länkad logga på** -kallades ”befintliga enkel inloggning”, det här alternativet kan administratörer tooplace en länk toothis programmet i startprogrammet för sina användare Azure AD-åtkomstpanelen eller Office 365.

Mer information om dessa lägen finns [hur fungerar enkel inloggning med Azure Active Directory fungerar](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work).

## <a name="saml-based-sign-on"></a>SAML-baserade inloggning
Hej **SAML-baserade inloggning** alternativet visar ett blad som är indelat i fyra avsnitt:

### <a name="domains-and-urls"></a>Domäner och webbadresser
Detta är där all information om domäner och webbadresser hello program läggs tooyour Azure AD-katalog. Alla indata krävs toomake arbete för enkel inloggning app visas direkt på hello-skärmen, medan alla valfria indata kan visas genom att välja hello **visa avancerade inställningar för URL: en** kryssrutan. hello fullständig lista över stöds indata innehåller:

* **Logga URL** – där hello användaren går toothis toosign i programmet. Om programmet hello är konfigurerade tooperform service provider-initierad enkel inloggning, när en användare går toothis URL, hello-leverantör hello nödvändiga hello omdirigering tooAzure AD tooauthenticate och logga användare i. Om det här fältet fylls använder URL toolaunch hello programmet från Office 365 och hello Azure AD-åtkomstpanelen Azure AD. Om det här fältet utelämnas Azure AD i stället och utför sedan identitetsleverantör-initierat inloggning när hello appen startas från Office 365, hello Azure AD-åtkomstpanelen, eller från hello Azure AD enkel inloggning URL.
* **Identifierare** -den här URI: N ska identifiera hello program för vilka enkel inloggning är konfigurerade. Hello värde att Azure AD skickar tillbaka tooapplication som hello målgruppen parametern för hello SAML-token och programmet hello förväntade toovalidate den. Det här värdet visas också som hello enhets-ID i en SAML-metadata som tillhandahålls av programmet hello.
* **Reply URL** -hello svars-URL: en är där programmet hello förväntar tooreceive hello SAML-token. Detta är även hänvisade tooas hello Assertion konsumenten Service (ACS) URL. När dessa har angetts, klickar du på nästa tooproceed toohello nästa skärm. Den här skärmen innehåller information om vilka behov toobe har konfigurerat på hello program på klientsidan tooenable detta tooaccept en SAML-token från Azure AD.
* **Relay tillstånd** -hello relay tillstånd är en valfri parameter som kan hjälpa dig att berätta hello program där tooredirect hello användaren när autentiseringen är slutförd. Vanligtvis hello-värdet är en giltig URL på hello program, men vissa program använder det här fältet på olika sätt (se hello app enkel inloggning på Info i dokumentation för). hello möjlighet tooset hello relay tillstånd är en ny funktion som är unik toohello nya Azure-portalen.

### <a name="user-attributes"></a>Användarattribut
Detta är där administratörer kan visa och redigera hello attribut som ska skickas i hello SAML-token att Azure AD utfärdar toohello programmet varje gång en användare loggar in.

Hej endast redigerbara attribut som stöds är hello **användar-ID** attribut. hello-värdet för det här attributet är hello fält i Azure AD som unikt identifierar varje användare i hello-programmet. Till exempel om hello appen har distribuerats med hello ”e-postadress” hello användarnamn och unik identifierare, skulle sedan hello värde anges toohello ”user.mail” fält i Azure AD.

### <a name="saml-signing-certificate"></a>SAML-signeringscertifikat
Det här avsnittet visar hello detaljerad information om hello certifikat att Azure AD använder toosign hello SAML-token som utfärdas toohello programmet varje gång hello användaren autentiseras. Det gör hello egenskaper för hello aktuella certifikat toobe kontrolleras, inklusive hello upphör att gälla.

### <a name="application-configuration"></a>Konfiguration av program
hello sista avsnittet innehåller hello dokumentationen och/eller kontroller krävs tooconfigure hello själva programmet toouse Azure Active Directory som en identitetsleverantör.

Hej **konfigurera programmet** meny innehåller nya kortfattat och inbäddade instruktioner för att konfigurera programmet hello. Det här är en annan ny funktion unika toohello nya Azure-portalen.

> [!NOTE]
> En komplett exempel på inbäddade dokumentation finns hello Salesforce.com-programmet. Dokumentation för fler appar läggs kontinuerligt.
> 
> 

![Inbäddade dokument][3]

## <a name="password-based-sign-on"></a>Lösenordsbaserade inloggning
Om stöds för programmet hello, välja hello lösenordsbaserade SSO-läge och välja **spara** konfigurerar omedelbart toodo lösenordsbaserade enkel inloggning. Mer information om hur du distribuerar lösenordsbaserade SSO finns [hur fungerar enkel inloggning med Azure Active Directory fungerar](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work).

![Lösenordsbaserade inloggning][4]

## <a name="linked-sign-on"></a>Länkade inloggning
Om stöds för programmet hello kan markerar hello länkade SSO-läge du tooenter hello URL som du vill hello Azure AD-åtkomstpanelen eller Office 365 tooredirect toowhen användare klickar du på den här appen. Läs mer om länkade SSO (kallades tidigare ”befintliga SSO”), [hur fungerar enkel inloggning med Azure Active Directory fungerar](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work).

![Länkade inloggning][5]

##<a name="feedback"></a>Feedback

Vi hoppas att du vill med hjälp av hello förbättrad upplevelse för Azure AD. Skriv ned hello feedback kommer! Publicera din feedback och förslag på förbättringar i hello **administrationsportalen** avsnitt i vår [Feedbackforum](https://feedback.azure.com/forums/169401-azure-active-directory/category/162510-admin-portal).  Vi är glada om hur du skapar nya nya produkter varje dag, och använda vägledning-tooshape och definiera vad vi bygga härnäst.

[1]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-blade.PNG
[2]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-sso-blade.PNG
[3]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-blade-embedded-docs.PNG
[4]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-blade-password-sso.PNG
[5]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-blade-linked-sso.PNG
