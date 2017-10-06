---
title: "Självstudier: Azure Active Directory-integrering med Questetra BPM Suite | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Questetra BPM Suite."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: fb6d5b73-e491-4dd2-92d6-94e5aba21465
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/14/2017
ms.author: jeedes
ms.openlocfilehash: 4907e3b5751cd79f994fbd2ebcb7faec4eac34e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-questetra-bpm-suite"></a>Självstudier: Azure Active Directory-integrering med Questetra BPM Suite
hello syftet med den här kursen är tooshow du hur toointegrate Questetra BPM Suite med Azure Active Directory (AD Azure).  
Integrera Questetra BPM Suite med Azure AD ger dig hello följande fördelar: 

* Du kan styra i Azure AD som har åtkomst tooQuestetra BPM Suite 
* Du kan aktivera din användare tooautomatically get inloggade tooQuestetra BPM Suite (Single Sign-On) med sina Azure AD-konton
* Du kan hantera dina konton i en central plats - hello klassiska Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav
tooconfigure Azure AD-integrering med Questetra BPM Suite måste hello följande objekt:

* En Azure AD-prenumeration
* En [Questetra BPM Suite](https://senbon-imadegawa-988.questetra.net/) enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.
> 
> 

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

* Du bör inte använda produktionsmiljön, om det inte är nödvändigt.
* Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/). 

## <a name="scenario-description"></a>Scenario-beskrivning
hello syftet med den här kursen är tooenable du tootest Azure AD enkel inloggning i en testmiljö.  
hello-scenario som beskrivs i den här kursen består av tre huvudsakliga byggblock:

1. Att lägga till Questetra BPM Suite från hello-galleriet 
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-questetra-bpm-suite-from-hello-gallery"></a>Att lägga till Questetra BPM Suite från hello-galleriet
tooconfigure hello integrering av Questetra BPM Suite i Azure AD, behöver du tooadd Questetra BPM Suite hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd Questetra BPM Suite från galleriet hello utför hello följande steg:**

1. I hello **klassiska Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Active Directory**. 
   
    ![Active Directory][1]

2. Från hello **Directory** listan, Välj hello katalog som du vill tooenable katalogintegrering.

3. tooopen hello program i vyn hello directory vyn klickar du på **program** i hello huvudmenyn.
   
    ![Program][2]

4. Klicka på **Lägg till** på hello hello sidans nederkant.
   
    ![Program][3]

5. På hello **vad vill du vill toodo** dialogrutan klickar du på **lägga till ett program från galleriet hello**.
   
    ![Program][4]

6. Skriv i sökrutan hello **Questetra BPM Suite**.
   
    ![Program][5]

7. I resultatfönstret hello väljer **Questetra BPM Suite**, och klicka sedan på **Slutför** tooadd hello program.

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
hello syftet med det här avsnittet är tooshow hur tooconfigure och testa Azure AD enkel inloggning med Questetra BPM Suite baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Questetra BPM Suite tooan användare i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Questetra BPM Suite toobe upprättas.  
Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i Questetra BPM Suite.

tooconfigure och testa Azure AD enkel inloggning med Questetra BPM Suite, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare Questetra BPM Suite](#creating-a-questetra-bpm-suite-test-user)**  -toohave en motsvarighet för Britta Simon i Questetra BPM Suite som är länkade toohello Azure AD-representation av henne.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD-Single Sign-On
hello syftet med det här avsnittet är tooenable Azure AD enkel inloggning i hello klassiska Azure-portalen och tooconfigure enkel inloggning i ditt Questetra BPM Suite-program.

**tooconfigure Azure AD enkel inloggning med Questetra BPM Suite utför hello följande steg:**

1. I hello klassiska Azure-portalen på hello **Questetra BPM Suite** integreringssidan för programmet, klickar du på **Konfigurera enkel inloggning** tooopen hello **Konfigurera enkel inloggning**  dialogrutan.
   
    ![Konfigurera enkel inloggning][8]

2. På hello **hur skulle du som användare toosign på tooQuestetra BPM Suite** väljer **Azure AD enkel inloggning**, och klicka sedan på **nästa**.
   
    ![Azure AD-Single Sign-On][9]

3. Logga in i ett annat webbläsarfönster din **Questetra BPM Suite** företagets webbplats som administratör.

4. Hello-menyn överst hello **systeminställningar**. 
   
    ![Azure AD-Single Sign-On][10]

5. tooopen hello **SingleSignOnSAML** klickar du på **SSO (SAML)**. 
   
    ![Azure AD-Single Sign-On][11]

6. I hello klassiska Azure-portalen på hello **konfigurera Appinställningar** dialogrutan utför hello följande steg: 
   
    ![Konfigurera Appinställningar för][13]
   
    a. Om du **Questetra BPM Suite** företagets plats i hello information om SP, kopiera hello **ACS URL**, och klistra in den i hello **logga URL** textruta.
   
    b. Om du **Questetra BPM Suite** företagets plats i hello information om SP, kopiera hello **enhets-ID**, och klistra in den i hello **utfärdar-URL** textruta.
   
    c. Om du **Questetra BPM Suite** företagets plats i hello information om SP, kopiera hello **ACS URL**, och klistra in den i hello **Reply URL** textruta.
   
    d. Klicka på **Nästa**.

7. På hello **Konfigurera enkel inloggning på Questetra BPM Suite** klickar du på **hämta certifikat**, och sedan spara hello certifikatfilen lokalt på datorn.
   
    ![Konfigurera enkel inloggning][14]

8. Om du **Questetra BPM Suite** företagets plats, utföra hello följande steg: 
   
    ![Konfigurera enkel inloggning][15]
   
    a. Välj **aktivera enkel inloggning**.
   
    b. Kopiera hello på hello klassiska Azure-portalen, **utfärdar-URL** värdet och klistrar in den i hello **enhets-ID** textruta.
   
    c. Kopiera hello på hello klassiska Azure-portalen, **inloggning tjänst-URL för enkel** värdet och klistrar in den i hello **inloggning Sidadress** textruta.
   
    d. Kopiera hello på hello klassiska Azure-portalen, **tjänst-URL för enkel Sign-Out** värdet och klistrar in den i hello **URL för utloggning** textruta.
   
    e. I hello **NameID format** textruta typen **urn: oasis: namn: tc: SAML:1.1:nameid-format: e-postadress**.

    f. Skapa en Base64-kodade filen från din hämtat certifikat. 

    >[!TIP] 
    >Mer information finns i [hur tooconvert en binär certifikat i en textfil](http://youtu.be/PlgrzUZ-Y1o).

    g. Öppna din Base64-kodade certifikatet i anteckningar, kopiera hello innehållet i den i Urklipp, och klistra in den i hello **validering certifikat** textruta. 

    h. Klicka på **Spara**.

1. Välj hello konfiguration för enkel inloggning bekräftelse på hello klassiska Azure-portalen, och klicka sedan på **nästa**. 
   
    ![Vad är Azure AD Connect?][17]

2. På hello **enkel inloggning bekräftelse** klickar du på **Slutför**.  
   
    ![Vad är Azure AD Connect?][18]


### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello klassiska Azure-portalen kallas Britta Simon.

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **klassiska Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Active Directory**.
   
    ![Skapa testanvändare i Azure AD][100] 

2. Från hello **Directory** listan, Välj hello katalog som du vill tooenable katalogintegrering.

3. toodisplay hello lista över användare i hello menyn hello överst, klickar du på **användare**.
   
    ![Skapa testanvändare i Azure AD][101] 

4. tooopen hello **Lägg till användare** i hello verktygsfältet på hello längst ned i dialogrutan klickar du på **Lägg till användare**. 
   
    ![Skapa testanvändare i Azure AD][102] 

5. På hello **berätta om den här användaren** dialogrutan utför hello följande steg:
   
    ![Skapa testanvändare i Azure AD][103]
   
    a. Som **typ av användare**väljer **ny användare i din organisation**.
   
    b. I hello användarnamn **textruta**, typen **BrittaSimon**.
   
    c. Klicka på Nästa.

6. På hello **användarprofil** dialogrutan utför hello följande steg: 
   
    ![Skapa testanvändare i Azure AD][104] 
   
    a. I hello **Förnamn** textruta typen **Britta**. 
   
    b. I hello **efternamn** textruta typ, **Simon**.
   
    c. I hello **visningsnamn** textruta typen **Britta Simon**.
   
    d. I hello **rollen** väljer **användaren**.
   
    e. Klicka på **Nästa**.

7. På hello **skaffa tillfälligt lösenord** dialogrutan sidan, klickar du på **skapa**.
   
    ![Skapa testanvändare i Azure AD][105]  

8. På hello **skaffa tillfälligt lösenord** dialogrutan utför hello följande steg:
   
    ![Skapa testanvändare i Azure AD][106]   
   
    a. Skriv ned hello värdet för hello **nytt lösenord**.
   
    b. Klicka på **Complete** (Slutför).   

### <a name="creating-a-questetra-bpm-suite-test-user"></a>Skapa en testanvändare Questetra BPM Suite
hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i Questetra BPM Suite.

**toocreate en användare som kallas Britta Simon i Questetra BPM Suite utför hello följande steg:**

1. Inloggning tooyour Questetra BPM Suite företagets webbplats som administratör.
2. Gå för**systeminställningar > användarlistan > Ny användare**. 
3. Utför följande hello på hello ny användare dialogrutan: 
   
    ![Skapa testanvändare][300] 
   
    a. I hello **namn** textruta anger Brittas användarnamn i Azure AD.
   
    b. I hello **e-post** textruta anger Brittas användarnamn i Azure AD.
   
    c. I hello **lösenord** textruta, ange ett lösenord.

4. Klicka på **Lägg till nya användare**.

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD
hello syftet med det här avsnittet är tooenabling Britta Simon toouse Azure enkel inloggning genom att ge sina access tooQuestetra BPM Suite.

![Vad är Azure AD Connect?][200]

**tooassign Britta Simon tooQuestetra BPM Suite utför hello följande steg:**

1. På hello Azure klassiska portal, tooopen hello program i vyn hello directory vyn klickar du på **program** i hello huvudmenyn.
   
    ![Vad är Azure AD Connect?][201]
2. Välj i listan med program hello **Questetra BPM Suite**.
   
    ![Vad är Azure AD Connect?][205]
3. Hello-menyn överst hello **användare**.
   
    ![Vad är Azure AD Connect?][202]
4. Markera i hello användare **Britta Simon**.
   
    ![Vad är Azure AD Connect?][203]
5. Klicka i hello verktygsfältet hello längst ned **tilldela**.
   
    ![Vad är Azure AD Connect?][204]

### <a name="testing-single-sign-on"></a>Testa enkel inloggning
hello syftet med det här avsnittet är tootest din Azure AD-konfiguration för enkel inloggning med hello åtkomstpanelen.  
När du klickar på hello Questetra BPM Suite-panelen i hello åtkomstpanelen får automatiskt inloggade tooyour Questetra BPM Suite-program.

## <a name="additional-resources"></a>Ytterligare resurser
* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_01.png


[8]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_06.png
[9]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_02.png
[10]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_03.png
[11]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_04.png
[12]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_05.png
[13]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_06.png
[14]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_07.png
[15]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_08.png
[16]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_09.png
[17]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_10.png
[18]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_08.png


[100]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_09.png 
[101]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_10.png 
[102]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_11.png 
[103]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_12.png 
[104]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_13.png 
[105]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_14.png 
[106]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_15.png 


[200]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_16.png 
[201]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_17.png 
[202]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_18.png
[203]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_19.png
[204]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_20.png
[205]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_12.png

[300]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_11.png 
