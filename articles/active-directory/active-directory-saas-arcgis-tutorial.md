---
title: "Självstudier: Azure Active Directory-integrering med ArcGIS Online | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och ArcGIS Online."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a9e132a4-29e7-48bf-beb9-4148e617c8a9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: jeedes
ms.openlocfilehash: f3dd55d798cf3256fb2758e011f33946baa405ce
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-arcgis-online"></a>Självstudier: Azure Active Directory-integrering med ArcGIS Online

I kursen får du lära dig hur toointegrate ArcGIS Online med Azure Active Directory (AD Azure).

Integrera ArcGIS Online med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst tooArcGIS Online
- Du kan aktivera din användare tooautomatically get inloggade tooArcGIS Online (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

<!--## Overview

tooenable single sign-on with ArcGIS Online, it must be configured toouse Azure Active Directory as an identity provider. This guide provides information and tips on how tooperform this configuration in ArcGIS Online.

>[!Note]: 
>This embedded guide is brand new in hello new Azure portal, and we’d love toohear your thoughts. Use hello Feedback ? button at hello top of hello portal tooprovide feedback. hello older guide for using hello [Azure classic portal](https://manage.windowsazure.com) tooconfigure this application can be found [here](https://github.com/Azure/AzureAD-App-Docs/blob/master/articles/en-us/_/sso_configure.md).-->


## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med ArcGIS Online, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En ArcGIS Online enkel inloggning på aktiverade prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till ArcGIS Online från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-arcgis-online-from-hello-gallery"></a>Att lägga till ArcGIS Online från hello-galleriet
tooconfigure hello integrering av ArcGIS Online i Azure AD, behöver du tooadd ArcGIS Online hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd ArcGIS Online från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. Klicka på **nytt program** hello längst upp i hello dialogrutan tooadd nytt program.

    ![Program][3]

4. Skriv i sökrutan hello **ArcGIS Online**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_search.png)

5. Markera hello resultat på panelen **ArcGIS Online**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med ArcGIS Online baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i ArcGIS Online är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användaren i ArcGIS Online toobe upprättas.

Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i ArcGIS Online.

tooconfigure och testa Azure AD enkel inloggning med ArcGIS Online, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en ArcGIS Online testanvändare](#creating-an-arcgis-online-test-user)**  -toohave en motsvarighet för Britta Simon i ArcGIS Online som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet ArcGIS Online.

**Utför följande hello tooconfigure Azure AD enkel inloggning med ArcGIS Online:**

1. I hello Azure-portalen på hello **ArcGIS Online** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_samlbase.png)

3. På hello **ArcGIS Online domän och URL: er** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_url.png)

    I hello **inloggnings-URL** textruta hello TYPVÄRDE med hello följande mönster:`https://<company>.maps.arcgis.com`

    > [!NOTE] 
    > Det här värdet är inte hello verkliga. Uppdatera det här värdet med hello faktiska inloggnings-URL. Kontakta [ArcGIS Online klienten supportteamet](http://support.esri.com/) tooget det här värdet. 

4. På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara hello XML-filen på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-arcgis-tutorial/tutorial_general_400.png)

6. Logga in på webbplatsen ArcGIS företag som en administratör i en annan webbläsarfönster.

7. Klicka på **redigera inställningar för**.

    ![Redigera inställningar för](./media/active-directory-saas-arcgis-tutorial/ic784742.png "redigera inställningar")

8. Klicka på **säkerhet**.

    ![Säkerhet](./media/active-directory-saas-arcgis-tutorial/ic784743.png "säkerhet")

9. Under **Enterprise inloggningar**, klickar du på **ange IDENTITETSLEVERANTÖR**.

    ![Enterprise-inloggningar](./media/active-directory-saas-arcgis-tutorial/ic784744.png "Enterprise-inloggningar")

10. På hello **ange identitetsleverantör** konfigurationen utför hello följande steg:
   
    ![Ange identitetsleverantör](./media/active-directory-saas-arcgis-tutorial/ic784745.png "ange identitetsleverantören.")
   
    a. I hello **namn** textruta Skriv ditt organisationsnamn.

    b. För **Metadata för hello Enterprise identitetsleverantör ska anges med hjälp av**väljer **A filen**.

    c. tooupload metadata för hämtade filer, klickar du på **Välj fil**.

    d. Klicka på **SET IDENTITETSLEVERANTÖR**.

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)


### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-arcgis-tutorial/create_aaduser_01.png) 

2. Gå för**användare och grupper** och på **alla användare** toodisplay hello lista över användare.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-arcgis-tutorial/create_aaduser_02.png) 

3. Hello överst i dialogrutan hello klickar du på **Lägg till** tooopen hello **användaren** dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-arcgis-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-arcgis-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av Britta Simon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-an-arcgis-online-test-user"></a>Skapa en ArcGIS Online testanvändare

I ordning tooenable Azure AD-användare toolog till ArcGIS Online, måste de etableras i ArcGIS Online.  
Etablering är en manuell aktivitet i hello fallet ArcGIS online.

**tooprovision ett användarkonto, utför följande steg hello:**

1. Logga in tooyour **ArcGIS** klient.

2. Klicka på **bjuda in MEDLEMMAR**.
   
    ![Bjud in medlemmar](./media/active-directory-saas-arcgis-tutorial/ic784747.png "bjuda in medlemmar")

3. Välj **lägga till medlemmar automatiskt utan att skicka ett e-postmeddelande**, och klicka sedan på **nästa**.
   
    ![Lägga till medlemmar automatiskt](./media/active-directory-saas-arcgis-tutorial/ic784748.png "automatiskt lägga till medlemmar")

4. På hello **medlemmar** dialogrutan utför hello följande steg:
   
     ![Lägg till och granska](./media/active-directory-saas-arcgis-tutorial/ic784749.png "Lägg till och granska")
    
     a. Ange hello **e-post**, **Förnamn**, och **efternamn** på en giltig AAD-konto som du vill tooprovision.
  
     b. Klicka på **Lägg till och granska**.
5. Granska hello data du har angett och klicka sedan på **lägga till MEDLEMMAR**.
   
    ![Lägg till medlem](./media/active-directory-saas-arcgis-tutorial/ic784750.png "Lägg till medlem")
        
    > [!NOTE]
    > hello Azure Active Directory-konto innehavaren kommer ett e-postmeddelande och följ en länk tooconfirm sitt konto innan den aktiveras.

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooArcGIS Online.

![Tilldela användare][200] 

**tooassign Britta Simon tooArcGIS Online, utför följande steg hello:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **ArcGIS Online**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

Du bör få automatiskt inloggade tooyour ArcGIS Online programmet när du klickar på hello ArcGIS Online panelen i hello åtkomstpanelen.
Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_203.png

