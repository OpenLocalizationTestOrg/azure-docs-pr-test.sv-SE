---
title: "Självstudier: Azure Active Directory-integrering med ClickTime | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och ClickTime."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: d437b5ab-4d71-4c13-96d0-79018cebbbd4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: jeedes
ms.openlocfilehash: a0259e31164cad6c6c77ed8aac1c50cd9a3e46ce
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-clicktime"></a>Självstudier: Azure Active Directory-integrering med ClickTime

I kursen får du lära dig hur toointegrate ClickTime med Azure Active Directory (AD Azure).

Integrera ClickTime med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooClickTime
- Du kan aktivera din användare tooautomatically get inloggade tooClickTime (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med ClickTime, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En ClickTime enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till ClickTime från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-clicktime-from-hello-gallery"></a>Att lägga till ClickTime från hello-galleriet
tooconfigure hello integrering av ClickTime i Azure AD, behöver du tooadd ClickTime hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd ClickTime från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![hello Azure Active Directory-knappen][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![hello Enterprise program bladet][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![hello-knappen för nytt program][3]

4. Skriv i sökrutan hello **ClickTime**väljer **ClickTime** resultatet-panelen klickar **Lägg till** knappen tooadd hello program.

    ![ClickTime i hello resultatlistan](./media/active-directory-saas-clicktime-tutorial/tutorial_clicktime_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning

I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med ClickTime baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i ClickTime är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i ClickTime toobe upprättas.

I ClickTime, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med ClickTime, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare ClickTime](#create-a-clicktime-test-user)**  -toohave en motsvarighet för Britta Simon i ClickTime som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#test-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt ClickTime program.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med ClickTime:**

1. I hello Azure-portalen på hello **ClickTime** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning länk][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-clicktime-tutorial/tutorial_clicktime_samlbase.png)

3. På hello **ClickTime domän och URL: er** avsnittet, utföra hello följande steg:

    ![URL: er och ClickTime domän med enkel inloggning information](./media/active-directory-saas-clicktime-tutorial/tutorial_clicktime_url.png)

    a. I hello **identifierare** textruta Skriv en URL som:`https://app.clicktime.com/sp/`
    
    b. I hello **Reply URL** textruta, ange en Webbadress med hello följande mönster: 

    | |
    |--|
    | `https://app.clicktime.com/Login/` |
    | `https://app.clicktime.com/App/Login/Consume.aspx` |

4. På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.

    ![länk för hämtning av hello-certifikat](./media/active-directory-saas-clicktime-tutorial/tutorial_clicktime_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-clicktime-tutorial/tutorial_general_400.png)

6. På hello **ClickTime Configuration** klickar du på **konfigurera ClickTime** tooopen **konfigurera inloggning** fönster. Kopiera hello **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**

    ![ClickTime konfiguration](./media/active-directory-saas-clicktime-tutorial/tutorial_clicktime_configure.png) 

7. Logga in på webbplatsen ClickTime företag som en administratör i en annan webbläsarfönster.

8. Klicka i hello verktygsfältet hello längst upp **inställningar**, och klicka sedan på **säkerhetsinställningar**.

9. I hello **inställningar för enkel inloggning** konfigurationen och utföra hello följande steg:
   
    ![Säkerhetsinställningar](./media/active-directory-saas-clicktime-tutorial/tic777280.png "säkerhetsinställningar")
   
    a.  Välj **Tillåt** logga in med enkel inloggning (SSO) med **Azure AD**.
   
    b. I hello **identitet Leverantörsslutpunkt** textruta klistra in **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen.
   
    c.  Öppna hello **Base64-kodat certifikat** hämtas från Azure-portalen i **anteckningar**, kopiera hello innehållet och klistra in den i hello **X.509-certifikat** textruta.
   
    d.  Klicka på **Spara**.

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa en testanvändare i Azure AD][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. Klicka på hello i hello Azure-portalen hello vänster **Azure Active Directory** knappen.

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-clicktime-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper**, och klicka sedan på **alla användare**.
    
    ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-clicktime-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** hello överst i hello **alla användare** dialogrutan.
 
    ![hello webbinställningar](./media/active-directory-saas-clicktime-tutorial/create_aaduser_03.png) 

4. I hello **användaren** dialogrutan utför hello följande steg:
 
    ![hello användardialogrutan](./media/active-directory-saas-clicktime-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="create-a-clicktime-test-user"></a>Skapa en testanvändare ClickTime

I ordning tooenable Azure AD-användare toolog i ClickTime, måste de etableras i ClickTime.  
Hello gäller ClickTime är etablering en manuell aktivitet.

> [!NOTE]
> Du kan använda något annat ClickTime användarens konto skapas verktyg eller API: er som tillhandahålls av ClickTime tooprovision användarkonton i Azure AD.

**tooprovision ett användarkonto, utför följande steg hello:**
1. Logga in tooyour **ClickTime** klient.
2. Klicka i hello verktygsfältet hello längst upp **företagets**, och klicka sedan på **personer**.
   
    ![Personer](./media/active-directory-saas-clicktime-tutorial/tic777282.png "personer")
3. Klicka på **lägga till personen**.
   
    ![Lägga till personen](./media/active-directory-saas-clicktime-tutorial/tic777283.png "lägga till Person")
4. I hello ny Person avsnitt, utföra hello följande steg:
   
    ![Personer](./media/active-directory-saas-clicktime-tutorial/tic777284.png "personer")
   
    a.  I hello **fullständigt namn** textruta typen fullständigt namn för användaren som **Britta Simon**. 
  
    b.  I hello **e-postadress** textruta hello e-post för användare som  **brittasimon@contoso.com** .
       
    > [!NOTE]
    > Om du vill kan ange du ytterligare egenskaper hello nytt person objekt.
   
    c.  Klicka på **Spara**.

### <a name="assign-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooClickTime.

![Tilldela hello användarroll][200] 

**tooassign Britta Simon tooClickTime utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **ClickTime**.

    ![ClickTimne länken i listan med program hello](./media/active-directory-saas-clicktime-tutorial/tutorial_clicktime_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Hej ”användare och grupper” länk][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![hello Lägg uppdrag fönstret][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="test-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

Du bör få automatiskt inloggade tooyour ClickTime programmet när du klickar på hello ClickTime panelen i hello åtkomstpanelen.
Läs mer om åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_203.png

