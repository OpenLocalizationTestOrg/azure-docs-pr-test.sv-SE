---
title: "Självstudier: Azure Active Directory-integrering med PlanMyLeave | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och PlanMyLeave."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: b0d31cbe-7ae2-488b-9cf3-4927391fa744
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/01/2017
ms.author: jeedes
ms.openlocfilehash: ba418a641b339a0d94a3c7b2596d37fbd88a30c5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-planmyleave"></a>Självstudier: Azure Active Directory-integrering med PlanMyLeave

I kursen får lära du att integrera PlanMyLeave med Azure Active Directory (AD Azure).

Integrera PlanMyLeave med Azure AD ger dig följande fördelar:

- Du kan styra i Azure AD som har åtkomst till PlanMyLeave
- Du kan aktivera användarna att automatiskt hämta loggat in på PlanMyLeave (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - till Azure-hanteringsportalen

Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

För att konfigurera Azure AD-integrering med PlanMyLeave, behöver du följande:

- En Azure AD-prenumeration
- En PlanMyLeave enkel inloggning på aktiverade prenumeration


> [!NOTE]
> Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.


Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:

- Du bör inte använda produktionsmiljön, om det inte är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion av en månad [här](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till PlanMyLeave från galleriet
2. Konfigurera och testa Azure AD enkel inloggning


## <a name="adding-planmyleave-from-the-gallery"></a>Att lägga till PlanMyLeave från galleriet
Du måste lägga till PlanMyLeave från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av PlanMyLeave i Azure AD.

**Utför följande steg för att lägga till PlanMyLeave från galleriet:**

1. I den  **[Azure-hanteringsportalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Gå till **företagsprogram**. Gå till **alla program**.

    ![Program][2]
    
3. Klicka på **Lägg till** knappen överst i dialogrutan.

    ![Program][3]

4. I sökrutan skriver **PlanMyLeave**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_001.png)

5. Välj i resultatpanelen **PlanMyLeave**, och klicka sedan på **Lägg till** för att lägga till programmet.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_0001.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med PlanMyLeave baserat på en testanvändare som kallas ”Britta Simon”.

Azure AD måste du känna till användaren i PlanMyLeave motsvarighet till en användare i Azure AD för enkel inloggning ska fungera. Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i PlanMyLeave upprättas.

Den här länken relationen upprättas genom att tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** i PlanMyLeave.

Om du vill konfigurera och testa Azure AD enkel inloggning med PlanMyLeave, måste du utföra följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare PlanMyLeave](#creating-a-planmyleave-test-user)**  – du har en motsvarighet för Britta Simon i PlanMyLeave som är kopplad till Azure AD-representation av henne.
4. **[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-hanteringsportalen och konfigurera enkel inloggning i ditt PlanMyLeave program.

**Utför följande steg för att konfigurera Azure AD enkel inloggning med PlanMyLeave:**

1. I Azure-hanteringsportalen på den **PlanMyLeave** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På den **enkel inloggning** dialogrutan sida som **läge** Välj **SAML-baserade inloggning** att aktivera enkel inloggning på.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_01.png)

3. På den **PlanMyLeave domän och URL: er** avsnittet, utför följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_02.png)

    a. I den **logga URL** textruta Skriv en URL med följande mönster:`https://<company-name>.planmyleave.com/Login.aspx`
    
    b. I den **Identiferare** textruta Skriv en URL med följande mönster:`https://<company-name>.planmyleave.com`

    > [!NOTE] 
    > Observera att detta inte är verkliga värden. Du måste uppdatera dessa värden med den faktiska logga URL och identifierare. Kontakta [PlanMyLeave supportteamet](mailto:support@planmyleave.com) att hämta dessa värden.

4. På den **SAML-signeringscertifikat** klickar du på **Skapa nytt certifikat**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_03.png)     

5. På den **skapa nya certifikat** dialogrutan, klicka på kalenderikonen och välj en **förfallodatum**. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-planmyleave-tutorial/tutorial_general_300.png)

6. På den **SAML-signeringscertifikat** väljer **aktivera nya certifikatet** och på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_04.png)

7. På popup-fönstret **förnyelsecertifikat** -fönstret klickar du på **OK**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-planmyleave-tutorial/tutorial_general_400.png)

8. På den **SAML-signeringscertifikat** klickar du på **certifikat (base64)** och spara certifikatfilen på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_05.png) 

9. På den **PlanMyLeave Configuration** klickar du på **konfigurera PlanMyLeave** att öppna **konfigurera inloggning** fönster.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_06.png) 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_07.png)

10. Logga in på din PlanMyLeave klient som en administratör i en annan webbläsarfönster.

11. Gå till **systeminställningarna**. På den **säkerhetshantering** Klicka på avsnittet **företagets SAML inställningar** .

    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_002.png) 

12. På den **SAML inställningar** klickar du på ikonen för redigeraren.

    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_003.png)

13. På den **SAML uppdateringsinställningar** avsnittet, utför följande steg:

    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_004.png)

    a.  I den **inloggnings-URL** textruta, ange värdet för **SAML inloggning tjänst-URL för enkel** från Azure AD-konfigurationsfönstret.

    b.  Öppna filen hämtat certifikat i anteckningar, kopiera endast innehållet mellan---börjar certifikat--- och---slut certifikatet---för den till Urklipp, och klistra in det till den **certifikat** textruta.

    c. Ange ”**har aktiverats**” till ”**Ja**”.

    d. Klicka på **Spara**.



### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
Syftet med det här avsnittet är att skapa en testanvändare i Azure Management portal kallas Britta Simon.

![Skapa Azure AD-användare][100]

**Utför följande steg för att skapa en testanvändare i Azure AD:**

1. I den **Azure-hanteringsportalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-planmyleave-tutorial/create_aaduser_01.png) 

2. Gå till **användare och grupper** och på **alla användare** att visa en lista över användare.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-planmyleave-tutorial/create_aaduser_02.png) 

3. Klicka på överst i dialogrutan **Lägg till** att öppna den **användaren** dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-planmyleave-tutorial/create_aaduser_03.png) 

4. På den **användaren** dialogrutan utför följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-planmyleave-tutorial/create_aaduser_04.png) 

    a. I den **namn** textruta typen **BrittaSimon**.

    b. I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och anteckna värdet för den **lösenord**.

    d. Klicka på **Skapa**. 



### <a name="creating-a-planmyleave-test-user"></a>Skapa en testanvändare PlanMyLeave

Syftet med det här avsnittet är att skapa en användare som kallas Britta Simon i PlanMyLeave. PlanMyLeave stöder just-in-time-etablering, vilket är aktiverat som standard.

Det finns ingen åtgärd objekt i det här avsnittet. En ny användare skapas vid ett försök att komma åt PlanMyLeave om den inte finns.

> [!NOTE]
> Om du behöver skapa en användare manuellt måste du kontakta [PlanMyLeave supportteamet](mailto:support@planmyleave.com).



### <a name="assigning-the-azure-ad-test-user"></a>Tilldela Azure AD-testanvändare

I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja sin åtkomst till PlanMyLeave.

![Tilldela användare][200] 

**Om du vill tilldela PlanMyLeave Britta Simon utför du följande steg:**

1. Öppna vyn program i Azure-hanteringsportalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program **PlanMyLeave**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_50.png) 

3. Klicka på menyn till vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** på listan användare.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    


### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.

När du klickar på panelen PlanMyLeave på åtkomstpanelen du bör få automatiskt loggat in på ditt PlanMyLeave program.


## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_203.png