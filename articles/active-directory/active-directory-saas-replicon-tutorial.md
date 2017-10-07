---
title: "Självstudier: Azure Active Directory-integrering med Replicon | Microsoft Docs"
description: "Lär dig hur toouse Replicon med Azure Active Directory tooenable enkel inloggning, Automatisk etablering och mycket mer!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 02a62f15-917c-417c-8d80-fe685e3fd601
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/23/2017
ms.author: jeedes
ms.openlocfilehash: 4949eaf959265cfa4f732a2b73317fffe6312a17
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-replicon"></a>Självstudier: Azure Active Directory-integrering med Replicon
hello syftet med den här kursen är tooshow hello integreringen av Azure och Replicon. hello-scenario som beskrivs i den här kursen förutsätter att du redan har hello följande objekt:

* En giltig Azure-prenumeration
* En Replicon-klient

Den här kursen hello Azure AD-användare som du har tilldelat tooReplicon blir toosingle kan logga in på hello-programmet på din Replicon företagets webbplats (service provider initierade inloggning) eller med hjälp av hello [introduktion toohello åtkomst Panelen](active-directory-saas-access-panel-introduction.md).

hello-scenario som beskrivs i den här kursen består av följande byggblock hello:

1. Aktivera programmet hello-integrering för Replicon
2. Konfigurera enkel inloggning (SSO)
3. Konfigurera användaretablering
4. Tilldela användare

![Scenariot](./media/active-directory-saas-replicon-tutorial/IC777798.png "Scenario")

## <a name="enable-hello-application-integration-for-replicon"></a>Aktivera hello program-integration för Replicon
hello syftet med det här avsnittet är toooutline hur tooenable hello programintegrationstyp för Replicon.

**tooenable hello programintegrationstyp för Replicon, utför hello följande steg:**

1. I hello klassiska Azure-portalen på hello vänstra navigationsfönstret klickar du på **Active Directory**.
   
    ![Active Directory](./media/active-directory-saas-replicon-tutorial/IC700993.png "Active Directory")
2. Från hello **Directory** listan, Välj hello katalog som du vill tooenable katalogintegrering.
3. tooopen hello program i vyn hello directory vyn klickar du på **program** i hello huvudmenyn.
   
    ![Program](./media/active-directory-saas-replicon-tutorial/IC700994.png "program")
4. Klicka på **Lägg till** på hello hello sidans nederkant.
   
    ![Lägg till program](./media/active-directory-saas-replicon-tutorial/IC749321.png "lägga till program")
5. På hello **vad vill du vill toodo** dialogrutan klickar du på **lägga till ett program från galleriet hello**.
   
    ![Lägga till ett program från gallerry](./media/active-directory-saas-replicon-tutorial/IC749322.png "lägga till ett program från gallerry")
6. I hello **sökrutan**, typen **Replicon**.
   
    ![Programgalleriet](./media/active-directory-saas-replicon-tutorial/IC777799.png "programgalleriet")
7. I resultatfönstret hello väljer **Replicon**, och klicka sedan på **Slutför** tooadd hello program.
   
    ![Replicon](./media/active-directory-saas-replicon-tutorial/IC777800.png "Replicon")
   
## <a name="configure-single-sign-on"></a>Konfigurera enkel inloggning

hello syftet med det här avsnittet är toooutline hur tooenable användare tooauthenticate tooReplicon med sitt konto i Azure AD med hjälp av federation baserat på hello SAML-protokoll.

**tooconfigure enkel inloggning, utföra hello följande steg:**

1. I hello klassiska Azure-portalen på hello **Replicon** integreringssidan för programmet, klickar du på **Konfigurera enkel inloggning** tooopen hello **Konfigurera enkel inloggning** dialogrutan.
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-replicon-tutorial/IC777801.png "Konfigurera enkel inloggning")
2. På hello **hur skulle du som användare toosign på tooReplicon** väljer **Microsoft Azure AD enkel inloggning**, och klicka sedan på **nästa**.
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-replicon-tutorial/IC777802.png "Konfigurera enkel inloggning")
3. På hello **konfigurera App-URL** utför hello följande steg:
   
    ![Konfigurera app-URL](./media/active-directory-saas-replicon-tutorial/IC777803.png "konfigurera app-URL")
  1. I hello **Replicon logga URL** textruta Skriv din Replicon klient-URL (t.ex.: *https://na2.replicon.com/company/saml2/sp-sso/post*).
  2. I hello **Replicon Reply URL** textruta Skriv din Replicon **AssertionConsumerService** URL (t.ex.: *https://global.replicon.com/! / saml2/företag/sso/post*).  
      
     >[!NOTE]
     >Du kan hämta hello URL från hello Replicon metadata på: **https://global.replicon.com/! /saml2/\<YourCompanyKey\>**.
     > 
     > 
 
  3. Klicka på **Nästa**.

4. På hello **Konfigurera enkel inloggning på Replicon** sida, toodownload metadata, klickar du på **hämtar metadata**, och sedan spara hello metadata på datorn.
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-replicon-tutorial/IC777804.png "Konfigurera enkel inloggning")
5. Logga in på webbplatsen Replicon företag som en administratör i en annan webbläsarfönster.

6. tooconfigure SAML 2.0 utför hello följande steg:
   
    ![Aktivera SAML-autentisering](./media/active-directory-saas-replicon-tutorial/IC777805.png "aktivera SAML-autentisering")
  
  1. toodisplay hello **EnableSAML Authentication2** dialogrutan bifoga hello följande tooyour URL efter din nyckel för företag: **/services/SecurityService1.svc/help/test/EnableSAMLAuthentication2**  
    * hello följande visar hello schemat för hello fullständiga URL: en:  
   **https://na2.replicon.com/\<YourCompanyKey\>/services/SecurityService1.svc/help/test/EnableSAMLAuthentication2**
   2. Klicka på hello  **+**  tooexpand hello **v20Configuration** avsnitt.
   3. Klicka på hello  **+**  tooexpand hello **metaDataConfiguration** avsnitt.
   4. Klicka på **Välj fil**, tooselect din identitet providern metadata XML-filen och klicka på **skicka**.

7. Välj hello konfiguration för enkel inloggning bekräftelse på hello klassiska Azure-portalen, och klicka sedan på **Slutför** tooclose hello **Konfigurera enkel inloggning** dialogrutan.
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-replicon-tutorial/IC778418.png "Konfigurera enkel inloggning")
   
## <a name="configure-user-provisioning"></a>Konfigurera användaretablering

I ordning tooenable Azure AD-användare toolog i Replicon, måste de etableras i Replicon.  

Hello gäller Replicon är etablering en manuell aktivitet.

**tooconfigure användaretablering, utför följande steg hello:**

1. Logga in på webbplatsen Replicon företag som en administratör i ett webbläsarfönster.
2. Gå för**Administration \> användare**.
   
    ![Användare](./media/active-directory-saas-replicon-tutorial/IC777806.png "användare")
3. Klicka på **+ Lägg till användaren**.
   
    ![Lägg till användare](./media/active-directory-saas-replicon-tutorial/IC777807.png "lägga till användare")
4. I hello **användarprofil** avsnittet, utföra hello följande steg:
   
    ![Användarprofil](./media/active-directory-saas-replicon-tutorial/IC777808.png "användarprofil")
   
  1. I hello **inloggningsnamnet** textruta typen hello Azure AD e-postadress för hello Azure AD-användare som du vill tooprovision.
  2. Som **autentiseringstyp**väljer **SSO**.
  3. I hello **avdelning** textruta skriver hello användarens avdelning.
  4. Som **typen**väljer **administratör**.
  5. Klicka på **spara användarprofil**.

>[!NOTE]
>Du kan använda något annat Replicon användarens konto skapas verktyg eller API: er som tillhandahålls av Replicon tooprovision AAD-användarkonton.
> 
> 

## <a name="assign-users"></a>Tilldela användare
tootest konfigurationen, måste toogrant hello Azure AD-användare som du vill med hjälp av ditt program åtkomst tooit genom att tilldela dem tooallow.

**tooassign användare tooReplicon utföra hello följande steg:**

1. Skapa ett testkonto i hello klassiska Azure-portalen.

2. På hello **Replicon** integreringssidan för programmet, klickar du på **tilldela användare**.
   
    ![Tilldela användare](./media/active-directory-saas-replicon-tutorial/IC777809.png "tilldela användare")

3. Välj din testanvändare, klicka på **tilldela**, och klicka sedan på **Ja** tooconfirm tilldelningen.
   
    ![Ja](./media/active-directory-saas-replicon-tutorial/IC767830.png "Ja")

Om du vill tootest dina inställningar för enkel inloggning, öppna hello åtkomstpanelen. Mer information om hello åtkomstpanelen finns [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).

