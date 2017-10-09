---
title: "Självstudier: Azure Active Directory-integrering med TOPdesk - säker | Microsoft Docs"
description: "Lär dig hur toouse TOPdesk - skydda med Azure Active Directory tooenable enkel inloggning, Automatisk etablering och mer!."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 8e149d2d-7849-48ec-9993-31f4ade5fdb4
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/22/2017
ms.author: jeedes
ms.openlocfilehash: 10fe420d1691c2845b89c779486ffd6fcd736432
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-topdesk---secure"></a>Självstudier: Azure Active Directory-integrering med TOPdesk - säker
hello syftet med den här kursen är tooshow hello integreringen av Azure och TOPdesk - säkra.  
hello-scenario som beskrivs i den här kursen förutsätter att du redan har hello följande objekt:

* En giltig Azure-prenumeration
* En TOPdesk - aktiverad prenumeration säker enkel inloggning

När du har slutfört den här självstudiekursen, hello Azure AD-användare som tilldelats tooTOPdesk - säker kommer att kunna toosingle logga in på hello-programmet på din TOPdesk - säkra företagets webbplats (service provider initierade inloggning) eller använda hello [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).

hello-scenario som beskrivs i den här kursen består av följande byggblock hello:

1. Att aktivera programmet hello-integrering för TOPdesk - säker
2. Konfigurera enkel inloggning
3. Konfigurera användaretablering
4. Tilldela användare

![Scenariot](./media/active-directory-saas-topdesk-secure-tutorial/IC790596.png "Scenario")

## <a name="enabling-hello-application-integration-for-topdesk---secure"></a>Att aktivera programmet hello-integrering för TOPdesk - säker
hello syftet med det här avsnittet är toooutline hur tooenable hello programintegrationstyp för TOPdesk - säker.

### <a name="tooenable-hello-application-integration-for-topdesk---secure-perform-hello-following-steps"></a>tooenable hello programintegrationstyp för TOPdesk - säker, utför hello följande steg:
1. I hello klassiska Azure-portalen på hello vänstra navigationsfönstret klickar du på **Active Directory**.
   
    ![Active Directory](./media/active-directory-saas-topdesk-secure-tutorial/IC700993.png "Active Directory")

2. Från hello **Directory** listan, Välj hello katalog som du vill tooenable katalogintegrering.

3. tooopen hello program i vyn hello directory vyn klickar du på **program** i hello huvudmenyn.
   
    ![Program](./media/active-directory-saas-topdesk-secure-tutorial/IC700994.png "program")

4. Klicka på **Lägg till** på hello hello sidans nederkant.
   
    ![Lägg till program](./media/active-directory-saas-topdesk-secure-tutorial/IC749321.png "lägga till program")

5. På hello **vad vill du vill toodo** dialogrutan klickar du på **lägga till ett program från galleriet hello**.
   
    ![Lägga till ett program från gallerry](./media/active-directory-saas-topdesk-secure-tutorial/IC749322.png "lägga till ett program från gallerry")

6. I hello **sökrutan**, typen **TOPdesk - säker**.
   
    ![Programgalleriet](./media/active-directory-saas-topdesk-secure-tutorial/IC790597.png "Programgalleriet")

7. I resultatfönstret hello väljer **TOPdesk - säker**, och klicka sedan på **Slutför** tooadd hello program.
   
    ![TOPdesk - säker](./media/active-directory-saas-topdesk-secure-tutorial/IC791933.png "TOPdesk - säker")

## <a name="configuring-single-sign-on"></a>Konfigurera enkel inloggning
hello syftet med det här avsnittet är toooutline hur tooenable användare tooauthenticate tooTOPdesk - skydda med sitt konto i Azure AD med hjälp av federation baserat på hello SAML-protokoll.  
Konfigurera enkel inloggning för TOPdesk - säker kräver att du tooupload en logotyp ikonfil. tooget hello ikonfil, kontakta hello TOPdesk supportteamet.

### <a name="tooconfigure-single-sign-on-perform-hello-following-steps"></a>tooconfigure enkel inloggning, utföra hello följande steg:
1. Logga in tooyour **TOPdesk - säker** företagets webbplats som administratör.
2. I hello **TOPdesk** -menyn klickar du på **inställningar**.
   
    ![Inställningar för](./media/active-directory-saas-topdesk-secure-tutorial/IC790598.png "inställningar")

3. Klicka på **inloggningsinställningar**.
   
    ![Inloggningsinställningar](./media/active-directory-saas-topdesk-secure-tutorial/IC790599.png "inloggningsinställningar")

4. Expandera hello **inloggningsinställningar** -menyn och klicka sedan på **allmänna**.
   
    ![Allmän](./media/active-directory-saas-topdesk-secure-tutorial/IC790600.png "Allmänt")

5. I hello **Secure** avsnitt i hello **SAML inloggningen** configuration avsnittet, utföra hello följande steg:
   
    ![Tekniska inställningar](./media/active-directory-saas-topdesk-secure-tutorial/IC790855.png "tekniska inställningar")
   
    a. Klicka på **hämta** toodownload hello offentliga metadatafil och sedan spara det lokalt på datorn.
   
    b. Öppna hello metadatafil och välj sedan hello **AssertionConsumerService** nod.
    
    ![Assertion kundtjänst](./media/active-directory-saas-topdesk-secure-tutorial/IC790856.png "Assertion kundtjänst")
   
    c. Kopiera hello **AssertionConsumerService** värde.  
      
    > [!NOTE]
    > Du behöver hello värdet i hello **konfigurera App-URL** senare i den här kursen.
    > 
    > 

6. Logga in i ett annat webbläsarfönster din **klassiska Azure-portalen** som administratör.

7. På hello **TOPdesk - säker** integreringssidan för programmet, klickar du på **Konfigurera enkel inloggning** tooopen hello ** Konfigurera enkel inloggning ** dialogrutan.
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-topdesk-secure-tutorial/IC790602.png "Konfigurera enkel inloggning")

8. På hello **hur skulle du som användare toosign på tooTOPdesk - Secure** väljer **Microsoft Azure AD enkel inloggning**, och klicka sedan på **nästa**.
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-topdesk-secure-tutorial/IC790603.png "Konfigurera enkel inloggning")

9. På hello **konfigurera App-URL** utför hello följande steg:
   
    ![Konfigurera App-URL](./media/active-directory-saas-topdesk-secure-tutorial/IC790604.png "konfigurera App-URL")
   
    a. I hello **TOPdesk - säker inloggning på URL: en** textruta typen hello URL som används av dina användare toosign i din TOPdesk - säkert program (t.ex. ”:*https://qssolutions.topdesk.net*”).
   
    b. I hello **TOPdesk – offentliga Reply URL** textruta klistra in hello **TOPdesk - URL för säker AssertionConsumerService** (t.ex. ”:*https://qssolutions.topdesk.net/tas/public/login/saml*")
   
    c. Klicka på **Nästa**.

10. På hello **Konfigurera enkel inloggning på TOPdesk - säker** sida, toodownload metadatafil klickar du på **hämtar metadata**, och sedan spara hello filen lokalt på datorn.
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-topdesk-secure-tutorial/IC790605.png "Konfigurera enkel inloggning")

11. toocreate en certifikatfil utför hello följande steg:
    
    ![Certifikatet](./media/active-directory-saas-topdesk-secure-tutorial/IC790606.png "certifikat")
    
    a. Öppna hello hämtade metadatafil.
    b. Expandera hello **RoleDescriptor** nod som har en **xsi: type** av **aggregeringsdesignprocessen: ApplicationServiceType**.
    c. Kopiera hello värdet för hello **X509Certificate** nod.
    d. Spara hello kopieras **X509Certificate** värdet lokalt på din dator i en fil.

12. Skydda företagets plats i hello på din TOPdesk - **TOPdesk** -menyn klickar du på **inställningar**.
    
    ![Inställningar för](./media/active-directory-saas-topdesk-secure-tutorial/IC790598.png "inställningar")

13. Klicka på **inloggningsinställningar**.
    
    ![Inloggningsinställningar](./media/active-directory-saas-topdesk-secure-tutorial/IC790599.png "inloggningsinställningar")

14. Expandera hello **inloggningsinställningar** -menyn och klicka sedan på **allmänna**.
    
    ![Allmän](./media/active-directory-saas-topdesk-secure-tutorial/IC790600.png "Allmänt")

15. I hello **offentliga** klickar du på **Lägg till**.
    
    ![Lägg till](./media/active-directory-saas-topdesk-secure-tutorial/IC790607.png "Lägg till")

16. På hello **SAML configuration assistenten** dialogrutan utför hello följande steg:
    
    ![SAML Configuration assistenten](./media/active-directory-saas-topdesk-secure-tutorial/IC790608.png "assistenten för SAML-konfiguration")
    
    a. tooupload hämtade metadata filen under **Federationsmetadata**, klickar du på **Bläddra**.

    b. ditt certifikat filen under tooupload **certifikat (RSA)**, klickar du på **Bläddra**.

    c. tooupload hello logotypfilen som du har fått från supportteamet för hello TOPdesk under **logotypen ikonen**, klickar du på **Bläddra**.

    d. I hello **användarnamn** textruta typen **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.

    e. I hello **visningsnamn** textruta, ange ett namn för din konfiguration.

    f. Klicka på **Spara**.

17. Välj hello konfiguration för enkel inloggning bekräftelse på hello klassiska Azure-portalen, och klicka sedan på **Slutför** tooclose hello **Konfigurera enkel inloggning** dialogrutan.
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-topdesk-secure-tutorial/IC790609.png "Konfigurera enkel inloggning")

## <a name="configuring-user-provisioning"></a>Konfigurera användaretablering
I ordning tooenable Azure AD-användare toolog i TOPdesk - säker, de måste etableras i TOPdesk - säker.  
Hello gäller TOPdesk - säker, etablering är en manuell aktivitet.

### <a name="tooconfigure-user-provisioning-perform-hello-following-steps"></a>tooconfigure användaretablering, utför följande steg hello:
1. Logga in tooyour **TOPdesk - säker** företagets webbplats som administratör.
2. Hello-menyn överst hello **TOPdesk \> ny \> stödfiler \> operatorn**.
   
    ![Operatorn](./media/active-directory-saas-topdesk-secure-tutorial/IC790610.png "Operator")

3. På hello **New-operatorn** dialogrutan utföra hello följande steg:
   
    ![New-operatorn](./media/active-directory-saas-topdesk-secure-tutorial/IC790611.png "New-operatorn")
   
    a. Klicka på hello på fliken Allmänt.
   
    b. I hello **efternamn** textruta för hello **allmänna** avsnitt, Skriv hello efternamn på en giltig Azure Active Directory-konto som du vill tooprovision.
   
    c. Välj en **plats** för hello konto i hello **plats** avsnitt.
   
    d. I hello **inloggningsnamnet** textruta för hello **TOPdesk inloggning** skriver ett inloggningsnamn för användaren.
   
    e. Klicka på **Spara**.

> [!NOTE]
> Du kan använda alla andra TOPdesk - säker användarkonto verktyg för att skapa eller API: er som tillhandahålls av TOPdesk - Secure tooprovision AAD användarkonton.
> 
> 

## <a name="assigning-users"></a>Tilldela användare
tootest konfigurationen, måste toogrant hello Azure AD-användare som du vill med hjälp av ditt program åtkomst tooit genom att tilldela dem tooallow.

### <a name="tooassign-users-tootopdesk---secure-perform-hello-following-steps"></a>tooassign användare tooTOPdesk - säker, utföra hello följande steg:
1. Skapa ett testkonto i hello klassiska Azure-portalen.
2. På hello ** TOPdesk - säker ** integreringssidan för programmet, klickar du på **tilldela användare**.
   
    ![Tilldela användare](./media/active-directory-saas-topdesk-secure-tutorial/IC790612.png "tilldela användare")

3. Välj din testanvändare, klicka på **tilldela**, och klicka sedan på **Ja** tooconfirm tilldelningen.
   
    ![Ja](./media/active-directory-saas-topdesk-secure-tutorial/IC767830.png "Ja")

Om du vill tootest dina inställningar för enkel inloggning, öppna hello åtkomstpanelen. Mer information om hello åtkomstpanelen finns [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).

