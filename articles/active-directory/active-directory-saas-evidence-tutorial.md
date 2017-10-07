---
title: "Självstudier: Azure Active Directory-integrering med Evidence.com | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Evidence.com."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: f9a7cb7c-ff67-40dc-872c-1fa35f9dd03b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: jeedes
ms.openlocfilehash: eed98c15dca8a6a77f0b5d407bb40ee0037fccad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-evidencecom"></a>Självstudier: Azure Active Directory-integrering med Evidence.com

I kursen får du lära dig hur toointegrate Evidence.com med Azure Active Directory (AD Azure).

Integrera Evidence.com med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooEvidence.com.
- Du kan låta dina användare tooautomatically get inloggade tooEvidence.com (Single Sign-On) med sina Azure AD-konton.
- Du kan hantera dina konton i en central plats - hello Azure-portalen.

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med Evidence.com, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En Evidence.com enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till Evidence.com från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-evidencecom-from-hello-gallery"></a>Att lägga till Evidence.com från hello-galleriet
tooconfigure hello integrering av Evidence.com i Azure AD, behöver du tooadd Evidence.com hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd Evidence.com från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![hello Azure Active Directory-knappen][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![hello Enterprise program bladet][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![hello-knappen för nytt program][3]

4. Skriv i sökrutan hello **Evidence.com**väljer **Evidence.com** resultatet-panelen klickar **Lägg till** knappen tooadd hello program.

    ![Evidence.com i hello resultatlistan](./media/active-directory-saas-evidence-tutorial/tutorial_evidence.com_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning

I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Evidence.com baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Evidence.com är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Evidence.com toobe upprättas.

I Evidence.com, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med Evidence.com, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare Evidence.com](#create-a-evidencecom-test-user)**  -toohave en motsvarighet för Britta Simon i Evidence.com som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#test-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Evidence.com program.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Evidence.com:**

1. I hello Azure-portalen på hello **Evidence.com** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning länk][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-evidence-tutorial/tutorial_evidence.com_samlbase.png)

3. På hello **Evidence.com domän och URL: er** avsnittet, utföra hello följande steg:

    ![URL: er och evidence.com domän med enkel inloggning information](./media/active-directory-saas-evidence-tutorial/tutorial_evidence.com_url.png)

    a. I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<yourtenant>.evidence.com`

    b. I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<yourtenant>.evidence.com`

    > [!NOTE] 
    > Dessa värden är inte verkliga. Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare. Kontakta [Evidence.com klienten supportteamet](https://communities.taser.com/support/SupportContactUs?typ=LE) tooget dessa värden. 

4. På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.

    ![länk för hämtning av hello-certifikat](./media/active-directory-saas-evidence-tutorial/tutorial_evidence.com_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-evidence-tutorial/tutorial_general_400.png)

6. På hello **Evidence.com Configuration** klickar du på **konfigurera Evidence.com** tooopen **konfigurera inloggning** fönster. Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**

    ![Evidence.com konfiguration](./media/active-directory-saas-evidence-tutorial/tutorial_evidence.com_configure.png) 

7. I ett separat webbläsarfönster inloggningen tooyour Evidence.com klient som administratör och navigera för**Admin** fliken

8. Klicka på **Agency för enkel inloggning**

9. Välj **SAML utifrån för enkel inloggning**

10. Kopiera hello **SAML enhets-ID**, **SAML enkel inloggning Tjänstwebbadress** och **Sign-Out URL** värden som visas i hello Azure-portalen och toohello motsvarande fält i Evidence.com.

11. Öppna filen hämtade Certificate(Base64) i anteckningar, kopiera hello innehållet i den i Urklipp, och klistra in den toohello **säkerhetscertifikat** rutan. 

12. Spara hello konfigurationen i Evidence.com.

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD

hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

   ![Skapa en testanvändare i Azure AD][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. Klicka på hello i hello Azure-portalen hello vänster **Azure Active Directory** knappen.

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-evidence-tutorial/create_aaduser_01.png)

2. toodisplay hello lista över användare, gå för**användare och grupper**, och klicka sedan på **alla användare**.

    ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-evidence-tutorial/create_aaduser_02.png)

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** hello överst i hello **alla användare** dialogrutan.

    ![hello webbinställningar](./media/active-directory-saas-evidence-tutorial/create_aaduser_03.png)

4. I hello **användaren** dialogrutan utför hello följande steg:

    ![hello användardialogrutan](./media/active-directory-saas-evidence-tutorial/create_aaduser_04.png)

    a. I hello **namn** skriver **BrittaSimon**.

    b. I hello **användarnamn** rutan typen hello användarens e-postadress Britta Simon.

    c. Välj hello **visa lösenordet** kryssrutan och sedan skriva ned hello-värde som visas i hello **lösenord** rutan.

    d. Klicka på **Skapa**.
 
### <a name="create-a-evidencecom-test-user"></a>Skapa en testanvändare Evidence.com

För Azure AD-användare toobe kan toosign i, måste de etableras för åtkomst i hello Evidence.com program. Det här avsnittet beskrivs hur toocreate Azure AD användarkonton i Evidence.com

**tooprovision ett användarkonto i Evidence.com:**

1. Logga in på webbplatsen Evidence.com företag som en administratör i ett webbläsarfönster.

2. Navigera för**Admin** fliken.

3. Klicka på **lägga till användare**.

4. Klicka på hello **Lägg till** knappen.

5. Hej **e-postadress** av hello läggs användare måste matcha hello användarnamnet för hello användare i Azure AD som du önskar toogive åtkomst. Om hello användarnamn och e-postadress inte är hello samma värde i din organisation, kan du använda hello **Evidence.com > attribut > enkel inloggning** avsnitt i hello Azure portal toochange hello nameidenitifer skickas tooEvidence.com toobe hello e-postadress.

### <a name="assign-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooEvidence.com.

![Tilldela hello användarroll][200] 

**tooassign Britta Simon tooEvidence.com utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **Evidence.com**.

    ![Hej Evidence.com länken i listan med program hello](./media/active-directory-saas-evidence-tutorial/tutorial_evidence.com_app.png)  

3. Hello-menyn hello vänster **användare och grupper**.

    ![Hej ”användare och grupper” länk][202]

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![hello Lägg uppdrag fönstret][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="test-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

Du bör få automatiskt inloggade tooyour Evidence.com programmet när du klickar på hello Evidence.com panelen i hello åtkomstpanelen.
Läs mer om åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-evidence-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-evidence-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-evidence-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-evidence-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-evidence-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-evidence-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-evidence-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-evidence-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-evidence-tutorial/tutorial_general_203.png

