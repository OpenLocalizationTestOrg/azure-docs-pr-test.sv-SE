---
title: "Självstudier: Azure Active Directory-integrering med SuccessFactors | Microsoft Docs"
description: "Lär dig hur toouse SuccessFactors med Azure Active Directory tooenable enkel inloggning, Automatisk etablering och mycket mer!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 32bd8898-c2d2-4aa7-8c46-f1f5c2aa05f1
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/21/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: 3f7895d7d5e26fda27f555ae2f14a1645b50dcba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-successfactors"></a>Självstudier: Azure Active Directory-integrering med SuccessFactors
hello syftet med den här kursen är tooshow du hur toointegrate SuccessFactors med Azure Active Directory (AD Azure).

Integrera SuccessFactors med Azure AD ger dig hello följande fördelar:

* Du kan styra i Azure AD som har åtkomst till tooSuccessFactors
* Du kan aktivera din användare tooautomatically get inloggade tooSuccessFactors (Single Sign-On) med sina Azure AD-konton
* Du kan hantera dina konton i en central plats - hello klassiska Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav
tooconfigure Azure AD-integrering med SuccessFactors, behöver du hello följande objekt:

* En giltig Azure-prenumeration
* En klient i SuccessFactors

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.
> 
> 

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

* Du bör inte använda produktionsmiljön, om det inte är nödvändigt.
* Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
hello syftet med den här kursen är tooenable du tootest Azure AD enkel inloggning i en testmiljö.

hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till SuccessFactors från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-successfactors-from-hello-gallery"></a>Att lägga till SuccessFactors från hello-galleriet
tooconfigure hello integrering av SuccessFactors i Azure AD, behöver du tooadd SuccessFactors hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd SuccessFactors från galleriet hello utför hello följande steg:**

1. I hello klassiska Azure-portalen på hello vänstra navigeringsfönstret klickar du på **Active Directory**.
   
    ![Konfigurera enkel inloggning][1]
2. Från hello **Directory** listan, Välj hello katalog som du vill tooenable katalogintegrering.
3. tooopen hello program i vyn hello directory vyn klickar du på **program** i hello huvudmenyn.
   
    ![Konfigurera enkel inloggning][2]
4. Klicka på **Lägg till** på hello hello sidans nederkant.
   
    ![Program][3]
5. På hello **vad vill du vill toodo** dialogrutan klickar du på **lägga till ett program från galleriet hello**.
   
    ![Konfigurera enkel inloggning][4]
6. I hello **sökrutan**, typen **SuccessFactors**.
   
    ![Konfigurera enkel inloggning][5]
7. Markera hello resultat på panelen **SuccessFactors**, och klicka sedan på **Slutför** tooadd hello program.
   
    ![Konfigurera enkel inloggning][6]

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
hello syftet med det här avsnittet är tooshow hur tooconfigure och testa Azure AD enkel inloggning med SuccessFactors baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i SuccessFactors tooan användare i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i SuccessFactors toobe upprättas.

Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i SuccessFactors.

tooconfigure och testa Azure AD enkel inloggning med SuccessFactors, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-single-sign-on) ** -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user) ** -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare SuccessFactors](#creating-a-successfactors-test-user) ** -toohave en motsvarighet för Britta Simon i SuccessFactors som är länkade toohello Azure AD-representation av henne.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user) ** -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on) ** -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning
I det här avsnittet Aktivera Azure AD enkel inloggning i hello klassiska portalen och konfigurera enkel inloggning i ditt SuccessFactors program.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med SuccessFactors:**

1. I hello klassiska Azure-portalen på hello **SuccessFactors** integreringssidan för programmet, klickar du på **Konfigurera enkel inloggning** tooopen hello **Konfigurera enkel inloggning** dialogrutan.
   
    ![Konfigurera enkel inloggning][7]
2. På hello **hur skulle du som användare toosign på tooSuccessFactors** väljer **Microsoft Azure AD enkel inloggning**, och klicka sedan på **nästa**.
   
    ![Konfigurera enkel inloggning][8]
3. På hello **konfigurera App-URL** , utför följande steg hello, och klickar sedan på **nästa**.
   
    ![Konfigurera enkel inloggning][9]
   
    a. I hello **logga URL** textruta, ange ett URL-Adressen med något av följande mönster hello: 
   
    |  |
    | --- |
    | `https://<company name>.successfactors.com/<company name>` |
    | `https://<company name>.sapsf.com/<company name>` |
    | `https://<company name>.successfactors.eu/<company name>` |
    | `https://<company name>.sapsf.eu` |
   
    b. I hello **Reply URL** textruta, ange ett URL-Adressen med något av följande mönster hello: 
   
    |  |
    | --- |
    | `https://<company name>.successfactors.com/<company name>` |
    | `https://<company name>.sapsf.com/<company name>` |
    | `https://<company name>.successfactors.eu/<company name>` |
    | `https://<company name>.sapsf.eu` |
    | `https://<company name>.sapsf.eu/<company name>` |
   
    c. Klicka på **Nästa**. 

    > [!NOTE]
    > Observera att detta inte är hello verkliga värden. Du har tooupdate dessa värden med hello faktiska logga URL och svars-URL. tooget dessa värden, kontakta [SuccessFactors supportteam](https://www.successfactors.com/en_us/support.html).

1. På hello **Konfigurera enkel inloggning på SuccessFactors** klickar du på **hämta certifikat**, och sedan spara hello certifikatfilen lokalt på datorn.
   
    ![Konfigurera enkel inloggning][10]

2. Logga in i ett annat webbläsarfönster din **SuccessFactors administrationsportalen** som administratör.

3. Besök **programsäkerhet** och egna för**enkel inloggning på funktionen**. 

4. Placera alla värden i hello **återställa Token** och på **spara Token** tooenable SAML SSO.
   
    ![Konfigurera enkel inloggning på app-sida][11]

    > [!NOTE] 
    > Det här värdet används bara som hello strömbrytare. Om inget värde har sparats är hello SAML SSO Aktiverat. Om ett tomt värde har sparats är hello SAML SSO OFF.

1. Skärmbild av interna toobelow och utför följande åtgärder hello.
   
    ![Konfigurera enkel inloggning på app-sida][12]
   
    a. Välj hello **SAML v2 SSO** alternativknapp
   
    b. Ange hello SAML garanterar part Name(e.g. SAml issuer + company name).
   
    c. I hello **SAML utfärdaren** textruta placera hello värdet för **utfärdar-URL** från guiden Konfigurera program för Azure AD.
   
    d. Välj **svar (kunden genereras/IdP/AP)** som **kräver obligatorisk signatur**.
   
    e. Välj **aktiverat** som **aktivera SAML-flaggan**.
   
    f. Välj **nr** som **signatur på förfrågan inloggning (SA genereras/SP/RP)**.
   
    g. Välj **webbläsare/Post-profilen** som **SAML profil**.
   
    h. Välj **nr** som **genomdriva giltighetsperioden för certifikatet**.
   
    Jag. Kopiera hello innehållet hello hämtat certifikatfilen och klistra in den i hello **SAML verifiera certifikatet** textruta.

    > [!NOTE] 
    > Hej certifikatinnehåll måste börja certifikat och certifikat sluttaggar.

1. Navigera tooSAML V2 och utför sedan hello följande steg:
   
    ![Konfigurera enkel inloggning på app-sida][13]
   
    a. Välj **Ja** som **stöder SP-initierad globala logga ut**.
   
    b. I hello **globala logga ut tjänst-URL (LogoutRequest mål)** textruta placera hello värdet för **Remote logga ut URL** från guiden Konfigurera program för Azure AD.
   
    c. Välj **nr** som **kräver sp måste kryptera alla NameID elementet**.
   
    d. Välj **Ospecificerad** som **NameID Format**.
   
    e. Välj **Ja** som **aktivera sp initierade inloggning (AuthnRequest)**.
   
    f. I hello **begäran om att skicka som företagsomfattande utgivaren** textruta placera hello värdet för **Remote inloggnings-URL** från guiden Konfigurera program för Azure AD.
2. Utför de här stegen om du vill toomake hello inloggning användarnamn inte skiftlägeskänsligt.
   
    a. Besök **Företagsinställningar**(nära hello längst ned).
   
    b. Markera kryssrutan bredvid **aktivera icke skiftlägeskänslig användarnamn**.
   
    c.Click **spara**.
   
    ![Konfigurera enkel inloggning][29]

    > [!NOTE] 
    > Om du försöker tooenable detta, kontrolleras Hej om det skapar en dubblett SAML-inloggningsnamn. Om exempelvis hello kunden har användarnamn Användare1 och Användare1. Tar bort skiftlägeskänslighet gör dessa dubbletter. hello system får du ett felmeddelande och kommer inte att aktivera hello-funktionen. hello kunden behöver toochange en hello användarnamn så att det verkligen är stavat olika. 

1. Välj hello konfiguration för enkel inloggning bekräftelse på hello klassiska Azure-portalen, och klicka sedan på **Slutför** tooclose hello **Konfigurera enkel inloggning** dialogrutan.
   
    ![Program][14]
2. På hello **enkel inloggning bekräftelse** klickar du på **Slutför**.
   
    ![Program][15]

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello klassiska portalen kallas Britta Simon.

![Skapa Azure AD-användare][16]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **klassiska Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Active Directory**.
   
    ![Skapa en testanvändare i Azure AD][17]
2. Från hello **Directory** listan, Välj hello katalog som du vill tooenable katalogintegrering.
3. toodisplay hello lista över användare i hello menyn hello överst, klickar du på **användare**.
   
    ![Skapa en testanvändare i Azure AD][18]
4. tooopen hello **Lägg till användare** i hello verktygsfältet på hello längst ned i dialogrutan klickar du på **Lägg till användare**.
   
    ![Skapa en testanvändare i Azure AD][19]
5. På hello **berätta om den här användaren** dialogrutan utför hello följande steg:
   
    ![Skapa en testanvändare i Azure AD][20]
   
    a. Välj ny användare i din organisation som typ av användare.
   
    b. I hello användarnamn **textruta**, typen **BrittaSimon**.
   
    c. Klicka på **Nästa**.
6. På hello **användarprofil** dialogrutan utför hello följande steg:
   
    ![Skapa en testanvändare i Azure AD][21]
   
    a. I hello **Förnamn** textruta typen **Britta**.  
   
    b. I hello **efternamn** textruta typ, **Simon**.
   
    c. I hello **visningsnamn** textruta typen **Britta Simon**.
   
    d. I hello **rollen** väljer **användaren**.
   
    e. Klicka på **Nästa**.
7. På hello **skaffa tillfälligt lösenord** dialogrutan sidan, klickar du på **skapa**.
   
    ![Skapa en testanvändare i Azure AD][22]
8. På hello **skaffa tillfälligt lösenord** dialogrutan utför hello följande steg:
   
    ![Skapa en testanvändare i Azure AD][23]
   
    a. Skriv ned hello värdet för hello **nytt lösenord**.
   
    b. Klicka på **Complete** (Slutför).  

### <a name="creating-a-successfactors-test-user"></a>Skapa en testanvändare SuccessFactors
I ordning tooenable Azure AD-användare toolog i SuccessFactors, måste de etableras i SuccessFactors.  
Hello gäller SuccessFactors är etablering en manuell aktivitet.

tooget-användare som har skapats i SuccessFactors måste toocontact hello [SuccessFactors supportteam](https://www.successfactors.com/en_us/support.html).

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD
hello syftet med det här avsnittet är tooenabling Britta Simon toouse Azure enkel inloggning genom att tilldela tooSuccessFactors sin åtkomst.

![Tilldela användare][24]

**tooassign Britta Simon tooSuccessFactors utför hello följande steg:**

1. På hello klassiska portalen tooopen hello program i vyn hello directory vyn klickar du på **program** i hello huvudmenyn.
   
    ![Tilldela användare][25]
2. Välj i listan med program hello **SuccessFactors**.
   
    ![Konfigurera enkel inloggning][26]
3. Hello-menyn överst hello **användare**.
   
    ![Tilldela användare][27]
4. Markera i hello användare **Britta Simon**.
5. Klicka i hello verktygsfältet hello längst ned **tilldela**.
   
    ![Tilldela användare][28]

### <a name="testing-single-sign-on"></a>Testa enkel inloggning
hello syftet med det här avsnittet är tootest din Azure AD-konfiguration för enkel inloggning med hello åtkomstpanelen.

Du bör få automatiskt inloggade tooyour SuccessFactors programmet när du klickar på hello SuccessFactors panelen i hello åtkomstpanelen.

## <a name="additional-resources"></a>Ytterligare resurser
* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[0]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_00.png
[1]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_01.png
[6]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_02.png
[7]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_03.png
[8]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_04.png
[9]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_05.png
[10]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_06.png

[11]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_07.png
[12]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_08.png
[13]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_09.png
[14]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_05.png
[15]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_06.png

[16]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_00.png
[17]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_01.png
[18]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_02.png
[19]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_03.png
[20]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_04.png
[21]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_05.png
[22]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_06.png
[23]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_07.png

[24]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_07.png
[25]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_08.png
[26]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_11.png
[27]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_09.png
[28]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_10.png
[29]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_10.png
