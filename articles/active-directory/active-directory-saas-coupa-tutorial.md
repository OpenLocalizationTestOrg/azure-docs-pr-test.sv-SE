---
title: "Självstudier: Azure Active Directory-integrering med Coupa | Microsoft Docs"
description: "Lär dig hur toouse Coupa med Azure Active Directory tooenable enkel inloggning, Automatisk etablering och mycket mer!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 47f27746-9057-4b9c-991e-3abf77710f73
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/10/2017
ms.author: jeedes
ms.openlocfilehash: 87e98573718d27d408c886466a374a987f58faa2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-coupa"></a>Självstudier: Azure Active Directory-integrering med Coupa
hello syftet med den här kursen är tooshow hello integreringen av Azure och Coupa.  
hello-scenario som beskrivs i den här kursen förutsätter att du redan har hello följande objekt:

* En giltig Azure-prenumeration
* En Coupa enkel inloggning (SSO) aktiverat prenumeration

Den här kursen hello Azure AD-användare som du har tilldelat tooCoupa blir toosingle kan logga in på hello program med hjälp av hello [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).

hello-scenario som beskrivs i den här kursen består av följande byggblock hello:

* Aktivera programmet hello-integrering för Coupa
* Konfigurera enkel inloggning
* Konfigurera användaretablering
* Tilldela användare

![Scenariot](./media/active-directory-saas-coupa-tutorial/IC791897.png "Scenario")

## <a name="enable-hello-application-integration-for-coupa"></a>Aktivera hello program-integration för Coupa
hello syftet med det här avsnittet är toooutline hur tooenable hello programintegrationstyp för Coupa.

**tooenable hello programintegrationstyp för Coupa, utför hello följande steg:**

1. I hello klassiska Azure-portalen på hello vänstra navigationsfönstret klickar du på **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-coupa-tutorial/IC700993.png "Active Directory")
2. Från hello **Directory** listan, Välj hello katalog som du vill tooenable katalogintegrering.
3. tooopen hello program i vyn hello directory vyn klickar du på **program** i hello huvudmenyn.
   
   ![Program](./media/active-directory-saas-coupa-tutorial/IC700994.png "program")
4. Klicka på **Lägg till** på hello hello sidans nederkant.
   
   ![Lägg till program](./media/active-directory-saas-coupa-tutorial/IC749321.png "lägga till program")
5. På hello **vad vill du vill toodo** dialogrutan klickar du på **lägga till ett program från galleriet hello**.
   
   ![Lägga till ett program från gallerry](./media/active-directory-saas-coupa-tutorial/IC749322.png "lägga till ett program från gallerry")
6. I hello **sökrutan**, typen **Coupa**.
   
   ![Programgalleriet](./media/active-directory-saas-coupa-tutorial/IC791898.png "Programgalleriet")
7. I resultatfönstret hello väljer **Coupa**, och klicka sedan på **Slutför** tooadd hello program.
   
   ![Coupa](./media/active-directory-saas-coupa-tutorial/IC791899.png "Coupa")
   
## <a name="configure-single-sign-on"></a>Konfigurera enkel inloggning

hello syftet med det här avsnittet är toooutline hur tooenable användare tooauthenticate tooCoupa med sitt konto i Azure AD med hjälp av federation baserat på hello SAML-protokoll.  

Konfigurera enkel inloggning för Coupa måste du tooretrieve en tumavtrycksvärde från ett certifikat. Om du inte är bekant med den här proceduren, se [hur tooretrieve en certifikatets tumavtrycksvärde](http://youtu.be/YKQF266SAxI).

**tooconfigure enkel inloggning, utföra hello följande steg:**

1. Inloggning tooyour Coupa företagets webbplats som administratör.
2. Gå för**installationsprogrammet \> säkerhetskontroll**.
   
   ![Säkerhetsåtgärder](./media/active-directory-saas-coupa-tutorial/IC791900.png "säkerhetsåtgärder")
3. toodownload hello Coupa metadata filen tooyour dator, klicka på **hämta och importera SP metadata**.
   
   ![Coupa SP metadata](./media/active-directory-saas-coupa-tutorial/IC791901.png "Coupa SP metadata")
4. Inloggning i en annan webbläsarfönster toohello klassiska Azure-portalen.
5. På hello **Coupa** integreringssidan för programmet, klickar du på **Konfigurera enkel inloggning** tooopen hello **Konfigurera enkel inloggning** dialogrutan.
   
   ![Konfigurera enkel inloggning](./media/active-directory-saas-coupa-tutorial/IC791902.png "Konfigurera enkel inloggning")
6. På hello **hur skulle du som användare toosign på tooCoupa** väljer **Microsoft Azure AD enkel inloggning**, och klicka sedan på **nästa**.
   
   ![Konfigurera enkel inloggning](./media/active-directory-saas-coupa-tutorial/IC791903.png "Konfigurera enkel inloggning")
7. På hello **konfigurera App-URL** utför hello följande steg:
   
   ![Konfigurera App-URL](./media/active-directory-saas-coupa-tutorial/IC791904.png "konfigurera App-URL")   
   1. I hello **logga URL** textruta typen URL som används av dina användare toosign på tooyour Coupa program (t.ex. ”:*http://company.Coupa.com*”).
   2. Öppna din hämtade Coupa metadatafilen och kopiera hello **AssertionConsumerService index-URL**.
   3. I hello **Coupa Reply URL** textruta klistra in hello **AssertionConsumerService index-URL** värde.
   4. Klicka på **Nästa**.
8. På hello **Konfigurera enkel inloggning på Coupa** sida, toodownload metadatafil klickar du på **hämtar metadata**, och sedan spara hello filen lokalt på datorn.
   
   ![Konfigurera enkel inloggning](./media/active-directory-saas-coupa-tutorial/IC791905.png "Konfigurera enkel inloggning")
9. Hej Coupa företagets webbplats gå för**installationsprogrammet \> säkerhetskontroll**.
   
   ![Säkerhetsåtgärder](./media/active-directory-saas-coupa-tutorial/IC791900.png "säkerhetsåtgärder")
10. I hello **logga in med autentiseringsuppgifterna för Coupa** avsnittet, utföra hello följande steg:  

   ![Logga in med autentiseringsuppgifterna för Coupa](./media/active-directory-saas-coupa-tutorial/IC791906.png "logga in med autentiseringsuppgifterna för Coupa") 
   1. Välj **logga in med SAML**.
   2. Klicka på **Bläddra** tooupload din hämtade Azure Active metadatafil.
   3. Klicka på **Spara**.
11. Välj hello konfiguration för enkel inloggning bekräftelse på hello klassiska Azure-portalen, och klicka sedan på **Slutför** tooclose hello **Konfigurera enkel inloggning** dialogrutan.
    
   ![Konfigurera enkel inloggning](./media/active-directory-saas-coupa-tutorial/IC791907.png "Konfigurera enkel inloggning")
    
## <a name="configure-user-provisioning"></a>Konfigurera användaretablering

I ordning tooenable Azure AD-användare toolog i Coupa, måste de etableras i Coupa.  

* Hello gäller Coupa är etablering en manuell aktivitet.

**tooconfigure användaretablering, utför följande steg hello:**

1. Logga in tooyour **Coupa** företagets webbplats som administratör.
2. Hello-menyn överst hello **installationsprogrammet**, och klicka sedan på **användare**.
   
   ![Användare](./media/active-directory-saas-coupa-tutorial/IC791908.png "användare")
3. Klicka på **Skapa**.
   
   ![Skapa användare](./media/active-directory-saas-coupa-tutorial/IC791909.png "skapa användare")
4. I hello **användare skapa** avsnittet, utföra hello följande steg:
   
   ![Användarinformation](./media/active-directory-saas-coupa-tutorial/IC791910.png "användarinformation")
   
   1. Typen hello **inloggning**, **Förnamn**, **efternamn**, **ID för enkel inloggning**, **e-post** attribut för en giltigt Azure Active Directory-konto som du vill använda tooprovision hello relaterade textrutor.
   2. Klicka på **Skapa**.   
   >[!NOTE]
   >hello Azure Active Directory användare får ett e-postmeddelande med en länk tooconfirm hello-konto innan den aktiveras. 
   > 

>[!NOTE]
>Du kan använda något annat Coupa användarens konto skapas verktyg eller API: er som tillhandahålls av Coupa tooprovision AAD-användarkonton. 
> 

## <a name="assign-users"></a>Tilldela användare
tootest konfigurationen, måste toogrant hello Azure AD-användare som du vill med hjälp av ditt program åtkomst tooit genom att tilldela dem tooallow.

**tooassign användare tooCoupa utföra hello följande steg:**

1. Skapa ett testkonto i hello klassiska Azure-portalen.
2. På hello ** Coupa ** integreringssidan för programmet, klickar du på **tilldela användare**.
   
   ![Tilldela användare](./media/active-directory-saas-coupa-tutorial/IC791911.png "tilldela användare")
3. Välj din testanvändare, klicka på **tilldela**, och klicka sedan på **Ja** tooconfirm tilldelningen.
   
   ![Ja](./media/active-directory-saas-coupa-tutorial/IC767830.png "Ja")

Om du vill tootest dina inställningar för enkel inloggning, öppna hello åtkomstpanelen. Mer information om hello åtkomstpanelen finns [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).

