---
title: "Självstudier: Azure Active Directory-integrering med Questetra BPM Suite | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Questetra BPM Suite."
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
ms.openlocfilehash: 7ae75446c9d19ce15a82caa9604658a528ab9941
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-questetra-bpm-suite"></a>Självstudier: Azure Active Directory-integrering med Questetra BPM Suite
Syftet med den här kursen är att visa dig hur du integrerar Questetra BPM Suite med Azure Active Directory (AD Azure).  
Integrera Questetra BPM Suite med Azure AD ger dig följande fördelar: 

* Du kan styra i Azure AD som har åtkomst till Questetra BPM Suite 
* Du kan aktivera användarna att automatiskt hämta loggat in på Questetra BPM Suite (Single Sign-On) med sina Azure AD-konton
* Du kan hantera dina konton i en central plats – den klassiska Azure-portalen

Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav
För att konfigurera Azure AD-integrering med Questetra BPM Suite, behöver du följande:

* En Azure AD-prenumeration
* En [Questetra BPM Suite](https://senbon-imadegawa-988.questetra.net/) enkel inloggning aktiverad prenumeration

> [!NOTE]
> Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.
> 
> 

Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:

* Du bör inte använda produktionsmiljön, om det inte är nödvändigt.
* Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/). 

## <a name="scenario-description"></a>Scenario-beskrivning
Syftet med den här kursen är att du ska testa Azure AD enkel inloggning i en testmiljö.  
Det scenario som beskrivs i den här kursen består av tre huvudsakliga byggblock:

1. Att lägga till Questetra BPM Suite från galleriet 
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-questetra-bpm-suite-from-the-gallery"></a>Att lägga till Questetra BPM Suite från galleriet
Du måste lägga till Questetra BPM Suite från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Questetra BPM Suite i Azure AD.

**Utför följande steg för att lägga till Questetra BPM Suite från galleriet:**

1. I den **klassiska Azure-portalen**, klicka på det vänstra navigeringsfönstret **Active Directory**. 
   
    ![Active Directory][1]

2. Från den **Directory** listan, Välj den katalog som du vill aktivera katalogintegrering.

3. Klicka för att öppna vyn program i vyn directory **program** på huvudmenyn.
   
    ![Program][2]

4. Klicka på **Lägg till** längst ned på sidan.
   
    ![Program][3]

5. På den **vad vill du göra** dialogrutan klickar du på **lägga till ett program från galleriet**.
   
    ![Program][4]

6. I sökrutan skriver **Questetra BPM Suite**.
   
    ![Program][5]

7. I resultatfönstret, Välj **Questetra BPM Suite**, och klicka sedan på **Slutför** lägga till programmet.

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
Syftet med det här avsnittet är att visa dig hur du konfigurerar och testa Azure AD enkel inloggning med Questetra BPM Suite baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning ska fungera, måste Azure AD motsvarighet användaren i Questetra BPM Suite till en användare i Azure AD är okänt. Med andra ord måste en länk mellan en Azure AD-användare och relaterade användaren i Questetra BPM Suite upprättas.  
Den här länken relationen upprättas genom att tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** i Questetra BPM Suite.

Om du vill konfigurera och testa Azure AD enkel inloggning med Questetra BPM Suite, måste du utföra följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare Questetra BPM Suite](#creating-a-questetra-bpm-suite-test-user)**  – har en motsvarighet för Britta Simon Questetra BPM Suite som är kopplad till Azure AD-representation av henne.
4. **[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD-Single Sign-On
Syftet med det här avsnittet är att aktivera Azure AD enkel inloggning i den klassiska Azure-portalen och konfigurera enkel inloggning i ditt Questetra BPM Suite-program.

**Utför följande steg för att konfigurera Azure AD enkel inloggning med Questetra BPM Suite:**

1. I den klassiska Azure-portalen på den **Questetra BPM Suite** integreringssidan för programmet, klickar du på **Konfigurera enkel inloggning** att öppna den **Konfigurera enkel inloggning** dialogrutan.
   
    ![Konfigurera enkel inloggning][8]

2. På den **hur vill du att användarna kan logga in på Questetra BPM Suite** väljer **Azure AD enkel inloggning**, och klicka sedan på **nästa**.
   
    ![Azure AD-Single Sign-On][9]

3. Logga in i ett annat webbläsarfönster din **Questetra BPM Suite** företagets webbplats som administratör.

4. Klicka på menyn högst upp **systeminställningar**. 
   
    ![Azure AD-Single Sign-On][10]

5. Öppna den **SingleSignOnSAML** klickar du på **SSO (SAML)**. 
   
    ![Azure AD-Single Sign-On][11]

6. I den klassiska Azure-portalen på den **konfigurera Appinställningar** dialogrutan utför följande steg: 
   
    ![Konfigurera Appinställningar för][13]
   
    a. Om du **Questetra BPM Suite** företagets webbplats, i avsnittet SP Information, kopiera den **ACS URL**, och klistrar in det i den **logga URL** textruta.
   
    b. Om du **Questetra BPM Suite** företagets webbplats, i avsnittet SP Information, kopiera den **enhets-ID**, och klistrar in det i den **utfärdar-URL** textruta.
   
    c. Om du **Questetra BPM Suite** företagets webbplats, i avsnittet SP Information, kopiera den **ACS URL**, och klistrar in det i den **Reply URL** textruta.
   
    d. Klicka på **Nästa**.

7. På den **Konfigurera enkel inloggning på Questetra BPM Suite** klickar du på **hämta certifikat**, och sedan spara certifikatfilen lokalt på datorn.
   
    ![Konfigurera enkel inloggning][14]

8. Om du **Questetra BPM Suite** företagets webbplats, utför följande steg: 
   
    ![Konfigurera enkel inloggning][15]
   
    a. Välj **aktivera enkel inloggning**.
   
    b. På den klassiska Azure-portalen, kopiera den **utfärdar-URL** värdet och klistrar in det i den **enhets-ID** textruta.
   
    c. På den klassiska Azure-portalen, kopiera den **inloggning tjänst-URL för enkel** värdet och klistrar in det i den **inloggning Sidadress** textruta.
   
    d. På den klassiska Azure-portalen, kopiera den **tjänst-URL för enkel Sign-Out** värdet och klistrar in det i den **URL för utloggning** textruta.
   
    e. I den **NameID format** textruta typen **urn: oasis: namn: tc: SAML:1.1:nameid-format: e-postadress**.

    f. Skapa en Base64-kodade filen från din hämtat certifikat. 

    >[!TIP] 
    >Mer information finns i [hur du konverterar ett binärt certifikat till en textfil](http://youtu.be/PlgrzUZ-Y1o).

    g. Öppna din Base64-kodade certifikatet i anteckningar, kopiera innehållet i den till Urklipp och klistrar in det i den **validering certifikat** textruta. 

    h. Klicka på **Spara**.

1. Välj bekräftelsen konfiguration för enkel inloggning på den klassiska Azure-portalen och klicka sedan på **nästa**. 
   
    ![Vad är Azure AD Connect?][17]

2. På den **enkel inloggning bekräftelse** klickar du på **Slutför**.  
   
    ![Vad är Azure AD Connect?][18]


### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
Syftet med det här avsnittet är att skapa en testanvändare i den klassiska Azure-portalen kallas Britta Simon.

**Utför följande steg för att skapa en testanvändare i Azure AD:**

1. I den **klassiska Azure-portalen**, klicka på det vänstra navigeringsfönstret **Active Directory**.
   
    ![Skapa testanvändare i Azure AD][100] 

2. Från den **Directory** listan, Välj den katalog som du vill aktivera katalogintegrering.

3. Klicka för att visa en lista över användare, på menyn upp **användare**.
   
    ![Skapa testanvändare i Azure AD][101] 

4. Öppna den **Lägg till användare** i verktygsfältet längst ned i dialogrutan klickar du på **Lägg till användare**. 
   
    ![Skapa testanvändare i Azure AD][102] 

5. På den **berätta om den här användaren** dialogrutan utför följande steg:
   
    ![Skapa testanvändare i Azure AD][103]
   
    a. Som **typ av användare**väljer **ny användare i din organisation**.
   
    b. I användarnamnet **textruta**, typen **BrittaSimon**.
   
    c. Klicka på Nästa.

6. På den **användarprofil** dialogrutan utför följande steg: 
   
    ![Skapa testanvändare i Azure AD][104] 
   
    a. I den **Förnamn** textruta typen **Britta**. 
   
    b. I den **efternamn** textruta typ, **Simon**.
   
    c. I den **visningsnamn** textruta typen **Britta Simon**.
   
    d. I den **rollen** väljer **användaren**.
   
    e. Klicka på **Nästa**.

7. På den **skaffa tillfälligt lösenord** dialogrutan sidan, klickar du på **skapa**.
   
    ![Skapa testanvändare i Azure AD][105]  

8. På den **skaffa tillfälligt lösenord** dialogrutan utför följande steg:
   
    ![Skapa testanvändare i Azure AD][106]   
   
    a. Anteckna värdet för den **nytt lösenord**.
   
    b. Klicka på **Complete** (Slutför).   

### <a name="creating-a-questetra-bpm-suite-test-user"></a>Skapa en testanvändare Questetra BPM Suite
Syftet med det här avsnittet är att skapa en användare som kallas Britta Simon i Questetra BPM Suite.

**Utför följande steg för att skapa en användare som kallas Britta Simon i Questetra BPM Suite:**

1. Inloggning på webbplatsen Questetra BPM Suite företag som administratör.
2. Gå till **systeminställningar > användarlistan > Ny användare**. 
3. I dialogrutan Ny användare utför du följande steg: 
   
    ![Skapa testanvändare][300] 
   
    a. I den **namn** textruta anger Brittas användarnamn i Azure AD.
   
    b. I den **e-post** textruta anger Brittas användarnamn i Azure AD.
   
    c. I den **lösenord** textruta, ange ett lösenord.

4. Klicka på **Lägg till nya användare**.

### <a name="assigning-the-azure-ad-test-user"></a>Tilldela Azure AD-testanvändare
Syftet med det här avsnittet är att aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja sin åtkomst till Questetra BPM Suite.

![Vad är Azure AD Connect?][200]

**Om du vill tilldela Questetra BPM Suite Britta Simon utför du följande steg:**

1. På den klassiska Azure-portalen för att öppna vyn program i katalogen vyn klickar du på **program** på huvudmenyn.
   
    ![Vad är Azure AD Connect?][201]
2. Välj i listan med program **Questetra BPM Suite**.
   
    ![Vad är Azure AD Connect?][205]
3. Klicka på menyn högst upp **användare**.
   
    ![Vad är Azure AD Connect?][202]
4. Välj i listan användare **Britta Simon**.
   
    ![Vad är Azure AD Connect?][203]
5. Klicka på i verktygsfältet längst ned i **tilldela**.
   
    ![Vad är Azure AD Connect?][204]

### <a name="testing-single-sign-on"></a>Testa enkel inloggning
Syftet med det här avsnittet är att testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.  
När du klickar på panelen Questetra BPM Suite på åtkomstpanelen du bör få automatiskt loggat in på ditt Questetra BPM Suite-program.

## <a name="additional-resources"></a>Ytterligare resurser
* [Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
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
