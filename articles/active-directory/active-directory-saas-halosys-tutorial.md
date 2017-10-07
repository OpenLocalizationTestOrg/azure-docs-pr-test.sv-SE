---
title: "Självstudier: Azure Active Directory-integrering med Halosys | Microsoft Docs"
description: "Lär dig hur toouse Halosys med Azure Active Directory tooenable enkel inloggning, Automatisk etablering och mycket mer!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 42a0eb7c-5cb7-44a9-b00b-b0e7df4b63e8
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/22/2017
ms.author: jeedes
ms.openlocfilehash: 18043ed1b6f7ab45c59cfd36252bef1621618e51
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-halosys"></a>Självstudier: Azure Active Directory-integrering med Halosys

I kursen får du lära dig hur toointegrate Halosys med Azure Active Directory (AD Azure).

Integrera Halosys med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooHalosys
- Du kan aktivera din användare tooautomatically get inloggade tooHalosys (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello klassiska Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med Halosys, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En Halosys enkel inloggning på aktiverade prenumeration


> [!NOTE] 
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.


tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Du bör inte använda produktionsmiljön, om det inte är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö.

hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till Halosys från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning


## <a name="adding-halosys-from-hello-gallery"></a>Att lägga till Halosys från hello-galleriet
tooconfigure hello integrering av Halosys i Azure AD, behöver du tooadd Halosys hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd Halosys från galleriet hello utför hello följande steg:**

1. I hello **klassiska Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Active Directory**.

    ![Active Directory][1]
2. Från hello **Directory** listan, Välj hello katalog som du vill tooenable katalogintegrering.

3. tooopen hello program i vyn hello directory vyn klickar du på **program** i hello huvudmenyn.

    ![Program][2]

4. Klicka på **Lägg till** på hello hello sidans nederkant.

    ![Program][3]

5. På hello **vad vill du vill toodo** dialogrutan klickar du på **lägga till ett program från galleriet hello**.

    ![Program][4]

6. Skriv i sökrutan hello **Halosys**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-Halosys-tutorial/tutorial_Halosys_01.png)
    
7. I resultatfönstret hello väljer **Halosys**, och klicka sedan på **Slutför** tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-Halosys-tutorial/tutorial_Halosys_011.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Halosys baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Halosys är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Halosys toobe upprättas.

Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i Halosys.

tooconfigure och testa Azure AD enkel inloggning med Halosys, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare Halosys](#creating-a-halosys-test-user)**  -toohave en motsvarighet för Britta Simon i Halosys som är länkade toohello Azure AD-representation av henne.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello klassiska portalen och konfigurera enkel inloggning i ditt Halosys program.


**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Halosys:**

1. I hello klassiska portalen på hello **Halosys** integreringssidan för programmet, klickar du på **Konfigurera enkel inloggning** tooopen hello **Konfigurera enkel inloggning** dialogrutan.
     
    ![Konfigurera enkel inloggning][6] 

2. På hello **hur skulle du som användare toosign på tooHalosys** väljer **Azure AD enkel inloggning**, och klicka sedan på **nästa**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-Halosys-tutorial/tutorial_Halosys_03.png) 

3. På hello **konfigurera Appinställningar** dialogrutan utför hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-Halosys-tutorial/tutorial_Halosys_04.png) 

    a. I hello **logga URL** textruta typen hello URL som används av dina användare toosign på tooyour Halosys program med hjälp av hello följer mönstret: `https://<company-name>.Halosys.com/client-api/api`.

    b.In hello **Identifierare** textruta typen hello URL i hello följer mönstret: `https://<company-name>.Halosys.com`. 
         
4. På hello **Konfigurera enkel inloggning på Halosys** klickar du på **hämtar metadata**, och spara sedan hello på datorn:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-Halosys-tutorial/tutorial_Halosys_05.png)
   
5. tooget SSO konfigurerats för ditt program, Kontakta supportteamet för Halosys och ge dem hello följande:

    • hello hämtas **metadatafil**
    
    • hello **SAML SSO-URL**
    

6. Välj hello konfiguration för enkel inloggning bekräftelse i hello klassiska portalen och klicka sedan på **nästa**.
    
    ![Azure AD-Single Sign-On][10]

7. På hello **enkel inloggning bekräftelse** klickar du på **Slutför**.  
 
    ![Azure AD-Single Sign-On][11]


### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
I det här avsnittet skapar du en testanvändare i hello klassiska portalen kallas Britta Simon.


![Skapa Azure AD-användare][20]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **klassiska Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Active Directory**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-Halosys-tutorial/create_aaduser_09.png) 

2. Från hello **Directory** listan, Välj hello katalog som du vill tooenable katalogintegrering.

3. toodisplay hello lista över användare i hello menyn hello överst, klickar du på **användare**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-Halosys-tutorial/create_aaduser_03.png) 

4. tooopen hello **Lägg till användare** i hello verktygsfältet på hello längst ned i dialogrutan klickar du på **Lägg till användare**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-Halosys-tutorial/create_aaduser_04.png) 

5. På hello **berätta om den här användaren** dialogrutan utför följande steg hello: ![att skapa en testanvändare i Azure AD](./media/active-directory-saas-Halosys-tutorial/create_aaduser_05.png) 

    a. Välj ny användare i din organisation som typ av användare.

    b. I hello användarnamn **textruta**, typen **BrittaSimon**.

    c. Klicka på **Nästa**.

6.  På hello **användarprofil** dialogrutan utför följande steg hello: ![att skapa en testanvändare i Azure AD](./media/active-directory-saas-Halosys-tutorial/create_aaduser_06.png) 

    a. I hello **Förnamn** textruta typen **Britta**.  

    b. I hello **efternamn** textruta typ, **Simon**.

    c. I hello **visningsnamn** textruta typen **Britta Simon**.

    d. I hello **rollen** väljer **användaren**.

    e. Klicka på **Nästa**.

7. På hello **skaffa tillfälligt lösenord** dialogrutan sidan, klickar du på **skapa**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-Halosys-tutorial/create_aaduser_07.png) 

8. På hello **skaffa tillfälligt lösenord** dialogrutan utför hello följande steg:

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-Halosys-tutorial/create_aaduser_08.png) 

    a. Skriv ned hello värdet för hello **nytt lösenord**.

    b. Klicka på **Complete** (Slutför).   



### <a name="creating-a-halosys-test-user"></a>Skapa en testanvändare Halosys

I det här avsnittet skapar du en användare som kallas Britta Simon i Halosys. Se tillsammans med Halosys support-teamet tooadd hello användare i hello Halosys plattform.


### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet Aktivera Britta Simon toouse Azure enkel inloggning genom att tilldela tooHalosys sin åtkomst.

![Tilldela användare][200] 

**tooassign Britta Simon tooHalosys utför hello följande steg:**

1. På hello klassiska portalen tooopen hello program i vyn hello directory vyn klickar du på **program** i hello huvudmenyn.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **Halosys**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-Halosys-tutorial/tutorial_Halosys_50.png) 

3. Hello-menyn överst hello **användare**.

    ![Tilldela användare][203]

4. Markera i hello användare **Britta Simon**.

5. Klicka i hello verktygsfältet hello längst ned **tilldela**.

    ![Tilldela användare][205]


### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

Du bör få automatiskt inloggade tooyour Halosys programmet när du klickar på hello Halosys panelen i hello åtkomstpanelen.


## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_205.png
