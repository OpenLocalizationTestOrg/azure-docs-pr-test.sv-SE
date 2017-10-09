---
title: "Självstudier: Azure Active Directory-integrering med Citrix GoToMeeting | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Citrix GoToMeeting."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 7d7897f6-b88e-4dd5-8f3a-e612337b1413
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 46a5da7504806202a5ec29f73c504e772c61bc2b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-citrix-gotomeeting"></a>Självstudier: Azure Active Directory-integrering med Citrix GoToMeeting

I kursen får du lära dig hur toointegrate Citrix GoToMeeting med Azure Active Directory (AD Azure).

Integrera Citrix GoToMeeting med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst tooCitrix GoToMeeting
- Du kan aktivera din användare tooautomatically get inloggade tooCitrix GoToMeeting (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med Citrix GoToMeeting måste hello följande objekt:

- En Azure AD-prenumeration
- En Citrix GoToMeeting enkel inloggning på aktiverade prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till Citrix GoToMeeting från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-citrix-gotomeeting-from-hello-gallery"></a>Att lägga till Citrix GoToMeeting från hello-galleriet
tooconfigure hello integrering av Citrix GoToMeeting i Azure AD, behöver du tooadd Citrix GoToMeeting hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd Citrix GoToMeeting från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. Klicka på **nytt program** hello längst upp i hello dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **Citrix GoToMeeting**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_citrix-gotomeeting_search.png)

5. Markera hello resultat på panelen **Citrix GoToMeeting**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_citrix-gotomeeting_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Citrix GoToMeeting baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Citrix GoToMeeting är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Citrix GoToMeeting toobe upprättas.

Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i Citrix GoToMeeting.

tooconfigure och testa Azure AD enkel inloggning med Citrix GoToMeeting, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare Citrix GoToMeeting](#creating-a-citrix-gotomeeting-test-user)**  -toohave en motsvarighet för Britta Simon i Citrix GoToMeeting som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i Citrix GoToMeeting-program.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Citrix GoToMeeting:**

1. I hello Azure-portalen på hello **Citrix GoToMeeting** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_citrix-gotomeeting_samlbase.png)

3. På hello **Citrix GoToMeeting domän och URL: er** avsnitt, utan behov av tooperform några steg.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_citrix-gotomeeting_url.png)


3. På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_citrix-gotomeeting_certificate.png) 

4. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_general_400.png)
    
5. Öppna hello Citrix GoToMeeting SAML-konfigurationsavsnittet, klicka på Konfigurera Citrix GoToMeeting SAML tooopen konfigurera inloggning. Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**

6. I en annan webbläsare och logga in tooyour [Citrix organisation Center](https://account.citrixonline.com/organization/administration/).

7. Klicka på hello **identitetsleverantör** fliken och utför sedan hello följande steg:  
   
    ![SAML-installationsprogrammet](./media/active-directory-saas-citrix-gotomeeting-tutorial/IC6892321.png "SAML-installationen")
   
    a. Välj **manuell**

    b. I hello Azure-portalen på hello **Konfigurera enkel inloggning på Citrix GoToMeeting** dialogrutan sida, kopiera hello **SAML enkel inloggning Tjänstwebbadress** värdet och klistrar in den i hello **inloggningssidan URL: en** textruta. 

    c. I hello Azure-portalen på hello **Konfigurera enkel inloggning på Citrix GoToMeeting** dialogrutan sida, kopiera hello **Sign-Out URL** värdet och klistrar in den i hello **utloggning Sidadress**textruta.

    d. I hello Azure-portalen på hello **Konfigurera enkel inloggning på Citrix GoToMeeting** dialogrutan sida, kopiera hello **SAML enhets-ID** värdet och klistrar in den i hello **identitet providern enhets-ID**  textruta.

    e. tooupload hämtade certifikatet, klicka på **överför certifikat**.

    f. Klicka på **Spara**.

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-citrix-gotomeeting-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-citrix-gotomeeting-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-citrix-gotomeeting-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-citrix-gotomeeting-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-citrix-gotomeeting-test-user"></a>Skapa en testanvändare Citrix GoToMeeting

I det här avsnittet skapas en användare som kallas Britta Simon i Citrix GoToMeeting. Citrix GoToMeeting stöder just-in-time-allokering som är aktiverad som standard.

Det finns ingen åtgärd objekt i det här avsnittet. Om en användare inte redan finns i Citrix GoToMeeting, skapas en ny när du försöker tooaccess Citrix GoToMeeting.

>[!Note]
>Om du behöver en användare manuellt, kontakta toocreate [Citrix GoToMeeting supportteamet](https://care.citrixonline.com/gotomeeting) 


### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooCitrix GoToMeeting.

![Tilldela användare][200] 

**tooassign Britta Simon tooCitrix GoToMeeting, utför följande steg hello:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **Citrix GoToMeeting**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_citrix-gotomeeting_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

Om du vill tootest dina inställningar för enkel inloggning, öppna hello åtkomstpanelen. Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Konfigurera Användaretablering](active-directory-saas-citrixgotomeeting-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_general_203.png

