---
title: "Självstudier: Azure Active Directory-integrering med OfficeSpace programvara | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan OfficeSpace programvaran och Azure Active Directory."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 95d8413f-db98-4e2c-8097-9142ef1af823
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: b53afb648b8a6057c32c782d857e34c06e152c67
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-officespace-software"></a>Självstudier: Azure Active Directory-integrering med OfficeSpace programvara

I kursen får du lära dig hur toointegrate OfficeSpace programvara med Azure Active Directory (AD Azure).

Integrera OfficeSpace programvara med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst tooOfficeSpace programvara.
- Du kan låta dina användare tooautomatically get inloggade tooOfficeSpace programvara (Single Sign-On) med sina Azure AD-konton.
- Du kan hantera dina konton i en central plats - hello Azure-portalen.

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med OfficeSpace programvara, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En OfficeSpace programvara enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Lägga till OfficeSpace programvara från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-officespace-software-from-hello-gallery"></a>Lägga till OfficeSpace programvara från hello-galleriet
tooconfigure hello integrering av OfficeSpace programvara i Azure AD, behöver du tooadd OfficeSpace programvara från hello galleriet tooyour lista över hanterade SaaS-appar.

**tooadd OfficeSpace programvara från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![hello Azure Active Directory-knappen][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![hello Enterprise program bladet][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![hello-knappen för nytt program][3]

4. Skriv i sökrutan hello **OfficeSpace programvara**väljer **OfficeSpace programvara** resultatet-panelen klickar **Lägg till** knappen tooadd hello program.

    ![OfficeSpace programvara i hello resulterar lista](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning

Du konfigurera och testa Azure AD enkel inloggning med OfficeSpace programvara baserat på en testanvändare som kallas ”Britta Simon” i det här avsnittet.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i OfficeSpace programvara är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i OfficeSpace programvara toobe upprätta.

I OfficeSpace programvara, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med OfficeSpace programvara, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare OfficeSpace programvara](#create-a-officespace-software-test-user)**  -toohave en motsvarighet för Britta Simon OfficeSpace programvara som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#test-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i OfficeSpace programmet.

**Utför följande hello tooconfigure Azure AD enkel inloggning med OfficeSpace programvara:**

1. I hello Azure-portalen på hello **OfficeSpace programvara** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning länk][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_samlbase.png)

3. På hello **OfficeSpace programvara domän och URL: er** avsnittet, utföra hello följande steg:

    ![URL: er och OfficeSpace programvara domän med enkel inloggning information](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_url.png)

    a. I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<company name>.officespacesoftware.com/users/sign_in/saml`

    b. I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`<company name>.officespacesoftware.com`

    > [!NOTE] 
    > Dessa värden är inte verkliga. Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare. Kontakta [OfficeSpace Programvaruklienten supportteamet](mailto:support@officespacesoftware.com) tooget dessa värden. 

4. OfficeSpace program förväntar hello SAML intyg i ett specifikt format. Konfigurera hello följande anspråk för det här programmet. Du kan hantera hello värden för attributen från hello ”**användarattribut**” avsnitt på sidan för integrering av programmet. hello följande skärmbild visar ett exempel för det här.
    
    ![Konfigurera attribut](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_attribute.png)

5. I hello **användarattribut** avsnittet hello **enkel inloggning** markerar **user.mail** som **användar-ID** och för varje rad som visas i hello tabellen nedan, utföra hello följande steg:
    
    | Attributets namn | Attributvärdet |
    | --- | --- |    
    | E-post | User.Mail |
    | namn | User.DisplayName |
    | Förnamn | User.givenName |
    | Efternamn | User.surname |

    a. Klicka på **Lägg till attributet** tooopen hello **lägga till attributet** dialogrutan.

    ![Konfigurera Lägg till ](./media/active-directory-saas-officespace-tutorial/tutorial_attribute_04.png)

    ![Konfigurera attribut](./media/active-directory-saas-officespace-tutorial/tutorial_attribute_05.png)
    
    b. I hello **namn** textruta hello attributnamn visas för den raden.
    
    c. Från hello **värdet** listan attributvärde för typ hello visas för den raden.
    
    d. Klicka på **Ok**
 
6. På hello **SAML-signeringscertifikat** avsnitt, kopiera hello **TUMAVTRYCKET** värdet för hello certifikat.

    ![länk för hämtning av hello-certifikat](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_certificate.png) 

7. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-officespace-tutorial/tutorial_general_400.png)

8. På hello **OfficeSpace programvarukonfiguration** klickar du på **konfigurera OfficeSpace programvara** tooopen **konfigurera inloggning** fönster. Kopiera hello **Sign-Out URL och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**

    ![Konfiguration av OfficeSpace programvara](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_configure.png) 

9. Logga in på din OfficeSpace programvara klient som en administratör i en annan webbläsarfönster.

10. Gå för**inställningar** och på **kopplingar**.

    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_002.png)

11. Klicka på **SAML-autentisering**.

    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_003.png)

12. I hello **SAML-autentisering** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_004.png)

    a. I hello **logga ut providern url** textruta klistra in hello värdet för **Sign-Out URL** som du har kopierat från Azure-portalen.

    b. I hello **klienten idp mål-url** textruta klistra in hello värdet för **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen.

    c. Klistra in hello **tumavtrycket** värde som du har kopierat från Azure-portalen i hello **klienten IDP certifikat fingeravtryck** textruta. 

    d. Klicka på **Spara inställningar**.


> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD

hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

   ![Skapa en testanvändare i Azure AD][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. Klicka på hello i hello Azure-portalen hello vänster **Azure Active Directory** knappen.

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-officespace-tutorial/create_aaduser_01.png)

2. toodisplay hello lista över användare, gå för**användare och grupper**, och klicka sedan på **alla användare**.

    ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-officespace-tutorial/create_aaduser_02.png)

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** hello överst i hello **alla användare** dialogrutan.

    ![hello webbinställningar](./media/active-directory-saas-officespace-tutorial/create_aaduser_03.png)

4. I hello **användaren** dialogrutan utför hello följande steg:

    ![hello användardialogrutan](./media/active-directory-saas-officespace-tutorial/create_aaduser_04.png)

    a. I hello **namn** skriver **BrittaSimon**.

    b. I hello **användarnamn** rutan typen hello användarens e-postadress Britta Simon.

    c. Välj hello **visa lösenordet** kryssrutan och sedan skriva ned hello-värde som visas i hello **lösenord** rutan.

    d. Klicka på **Skapa**.
 
### <a name="create-a-officespace-software-test-user"></a>Skapa en testanvändare OfficeSpace programvara

hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon OfficeSpace programvaran. OfficeSpace programvara stöder just-in-time-etablering, vilket är aktiverat som standard.

Det finns ingen åtgärd objekt i det här avsnittet. En ny användare skapas under ett försök tooaccess OfficeSpace programvara om det inte finns.

> [!NOTE]
> Om du behöver toocreate en användare manuellt, måste tooContact [OfficeSpace programvara supportteamet](mailto:support@officespacesoftware.com).

### <a name="assign-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooOfficeSpace programvara.

![Tilldela hello användarroll][200] 

**tooassign Britta Simon tooOfficeSpace programvara, utföra hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **OfficeSpace programvara**.

    ![Hej OfficeSpace programvarulänk i listan med program hello](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_app.png)  

3. Hello-menyn hello vänster **användare och grupper**.

    ![Hej ”användare och grupper” länk][202]

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![hello Lägg uppdrag fönstret][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="test-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

När du klickar på hello OfficeSpace programvara panelen i hello åtkomstpanelen får automatiskt inloggade tooyour OfficeSpace program.

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_203.png

