---
title: "Självstudier: Azure Active Directory-integrering med upptar LMS | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och upptar LMS."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: ba9f1b3d-a4a0-4ff7-b0e7-428e0ed92142
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: jeedes
ms.openlocfilehash: a140a78a3a9474a6372a3ad4fb8251bd2452c990
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-absorb-lms"></a>Självstudier: Azure Active Directory-integrering med upptar LMS

I kursen får du lära dig hur toointegrate Absorb LMS med Azure Active Directory (AD Azure).

Integrera upptar LMS med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst tooAbsorb LMS
- Du kan aktivera din användare tooautomatically get inloggade tooAbsorb LMS (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow finns mer information om SaaS appintegrering med Azure AD. [Vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med upptar LMS måste hello följande objekt:

- En Azure AD-prenumeration
- En upptar LMS enkel inloggning på aktiverade prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till upptar LMS från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-absorb-lms-from-hello-gallery"></a>Att lägga till upptar LMS från hello-galleriet
tooconfigure hello integrering av upptar LMS i tooAzure AD, behöver du tooadd Absorb LMS hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd Absorb LMS från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![hello Azure Active Directory-knappen][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![hello Enterprise program bladet][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![hello-knappen för nytt program][3]

4. Skriv i sökrutan hello **upptar LMS**väljer **upptar LMS** resultatet-panelen klickar **Lägg till** knappen tooadd hello program.

    ![Upptar LMS i hello resultatlistan](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning

I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med upptar LMS baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i upptar LMS är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i upptar LMS toobe upprättas.

Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i upptar LMS.

tooconfigure och testa Azure AD enkel inloggning med upptar LMS, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare upptar LMS](#create-an-absorb-lms-test-user)**  -toohave en motsvarighet för Britta Simon i upptar LMS som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#test-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt program skulle kunna ta emot LMS.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med upptar LMS:**

1. I hello Azure-portalen på hello **upptar LMS** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning länk][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_samlbase.png)

3. På hello **upptar LMS domän och URL: er** avsnittet, utföra hello följande steg:

    ![Upptar LMS domän URL: er och enkel inloggning information](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_url.png)

    a. I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<subdomain>.myabsorb.com/Account/SAML`

    b. I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<subdomain>.myabsorb.com/Account/SAML`
     
    > [!NOTE] 
    > Dessa värden är inte hello verkliga. Uppdatera dessa värden med hello faktiska identifierare och svars-URL. Kontakta [upptar LMS klienten supportteamet](https://www.absorblms.com/support) tooget dessa värden. 

4. På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.

    ![länk för hämtning av hello-certifikat](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_certificate.png) 

6. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-absorblms-tutorial/tutorial_general_400.png)
    
7. På hello **upptar LMS Configuration** klickar du på **konfigurera upptar LMS** tooopen **konfigurera inloggning** fönster. Kopiera hello **Sign-Out Webbadressen och SAML enkel inloggning Service** från hello **Snabbreferens avsnitt.**

    ![Upptar LMS konfiguration](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_configure.png) 

8. Logga in tooyour upptar LMS företagets webbplats som en administratör i en annan webbläsarfönster.

9. Klicka på hello **ikon** på hello administratörsgränssnittet. 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-absorblms-tutorial/1.png)

10. Klicka på **portalinställningar**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-absorblms-tutorial/2.png)
    
11. Klicka på hello **användare** fliken.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-absorblms-tutorial/3.png)

12. Utför följande steg tooaccess hello enkel inloggning konfigurationsfält hello:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-absorblms-tutorial/4.png)

    a. Välj lämplig hello **läge**.

    b. Öppna hello certifikat som du har hämtat från hello Azure-portalen i anteckningar, ta bort hello **---BEGIN CERTIFICATE---** och **---END CERTIFICATE---** tagga och klistra in hello återstående innehåll i Hej **nyckeln** textruta.
    
    c. I hello **Id-egenskapen**, Välj hello lämpligt attribut som du har konfigurerat enligt hello användar-ID i hello Azure AD (till exempel, om hello userprinciplename är valt i Azure AD användarnamn här markeras.)

    d. I hello **inloggnings-URL**, klistra in hello **”SAML inloggning tjänst-URL för enkel”** värde som du har kopierat från hello **konfigurera inloggning** fönster för hello Azure-portalen.

    e. I hello **logga ut URL**, klistra in hello **”Sign-Out URL”** värde som du har kopierat från hello **konfigurera inloggning** fönster för hello Azure-portalen.

13. Aktivera **endast tillåta SSO-inloggning**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-absorblms-tutorial/5.png)

14. Klicka på **”spara”.**

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD

hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa en testanvändare i Azure AD][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-absorblms-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-absorblms-tutorial/create_aaduser_02.png) 

3. Hello överst i dialogrutan hello klickar du på **Lägg till** tooopen hello **användaren** dialogrutan.
 
    ![hello webbinställningar](./media/active-directory-saas-absorblms-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![hello användardialogrutan](./media/active-directory-saas-absorblms-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.

### <a name="create-an-absorb-lms-test-user"></a>Skapa en testanvändare upptar LMS

tooenable Azure AD-användare toolog i tooAbsorb LMS, måste de vara etablerade i tooAbsorb LMS.  
Upptar LMS är etablering en manuell aktivitet.

**tooprovision ett användarkonto, utför följande steg hello:**

1. Logga in tooyour upptar LMS företagets webbplats som administratör.

2. Klicka på **användare** fliken.

    ![Bjud in personer](./media/active-directory-saas-absorblms-tutorial/absorblms_users.png)

3. Klicka på **användare** under hello **användare** fliken.

    ![Bjud in personer](./media/active-directory-saas-absorblms-tutorial/absorblms_userssub.png)

4.  Välj **användare** från **Lägg till ny** listrutan.

    ![Bjud in personer](./media/active-directory-saas-absorblms-tutorial/absorblms_createuser.png)

5. På hello **Lägg till användare** utför hello följande steg:

    ![Bjud in personer](./media/active-directory-saas-absorblms-tutorial/user.png)

    a. I hello **Förnamn** textruta typen hello förnamn som Britta.

    b. I hello **efternamn** textruta Skriv hello efternamn som Simon.
    
    c. I hello **användarnamn** textruta Skriv hello användarnamn som Britta Simon.

    d. I hello **lösenord** textruta hello lösenordstyp av Britta Simon.

    e. I hello **Bekräfta lösenord** textruta typen hello samma lösenord.
    
    f. Gör det som **ACTIVE**.   

6. Klicka på **”spara”.**
 
### <a name="assign-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooAbsorb LMS.

![Tilldela hello användarroll][200]

**tooassign Britta Simon tooAbsorb LMS, utför följande steg hello:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **upptar LMS**.

    ![hello upptar LMS länken i listan med program hello](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Hej ”användare och grupper” länk][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![hello Lägg uppdrag fönstret][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="test-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

Klicka på hello upptar LMS panelen i hello åtkomstpanelen, får du automatiskt inloggade tooyour upptar LMS program. Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](https://msdn.microsoft.com/library/dn308586).

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_203.png

