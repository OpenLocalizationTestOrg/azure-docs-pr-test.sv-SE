---
title: "Självstudier: Azure Active Directory-integrering med Splunk Enterprise och Splunk molntjänster | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Splunk Enterprise och Splunk moln."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: b3e2b4a9-749c-4895-813d-db46f8dfdbf8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/09/2017
ms.author: jeedes
ms.openlocfilehash: 9bb6817cb31dce684cd9cc1c567fa3efc8906ad6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-splunk-enterprise-and-splunk-cloud"></a>Självstudier: Azure Active Directory-integrering med Splunk Enterprise och Splunk moln

I kursen får du lära dig hur toointegrate Splunk Enterprise och Splunk moln med Azure Active Directory (AD Azure).

Integrera Splunk Enterprise och Splunk moln med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst tooSplunk Enterprise och Splunk moln
- Du kan aktivera din användare tooautomatically get inloggade tooSplunk Enterprise och Splunk moln enkel inloggning (SSO) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello klassiska Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med Splunk Enterprise och Splunk molnet, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En Splunk Enterprise eller Splunk moln SSO aktiverad prenumeration


>[!NOTE]
>tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.
>

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Du bör inte använda produktionsmiljön, om det inte är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du få en [utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö.

hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Lägga till Splunk Enterprise och Splunk moln från hello-galleriet
2. Konfigurera och testa Azure AD-SSO


## <a name="add-splunk-enterprise-and-splunk-cloud-from-hello-gallery"></a>Lägg till Splunk Enterprise och Splunk moln från hello-galleriet
tooconfigure hello integrering av Splunk Enterprise och Splunk moln i Azure AD, behöver du tooadd Splunk Enterprise och Splunk moln hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd Splunk Enterprise och Splunk moln från galleriet hello utför hello följande steg:**

1. I hello **klassiska Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Active Directory**.

    ![Active Directory][1]

2. Från hello **Directory** listan, Välj hello katalog som du vill tooenable katalogintegrering.

3. tooopen hello program i vyn hello directory vyn klickar du på **program** i hello huvudmenyn.

    ![Program][2]

4. Klicka på **Lägg till** på hello hello sidans nederkant.

    ![Program][3]

5. På hello **vad vill du vill toodo** dialogrutan klickar du på **lägga till ett program från galleriet hello**.

    ![Program][4]

6. Skriv i sökrutan hello **Splunk Enterprise eller Splunk moln**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_splunk_01.png)

7. I resultatfönstret hello väljer **Splunk Enterprise och Splunk moln**, och klicka sedan på **Slutför** tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_splunk_02.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Splunk Enterprise och Splunk molnet baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Splunk Enterprise och Splunk molnet är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användaren i Splunk Enterprise och Splunk molnet toobe upprätta.

Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i Splunk Enterprise och Splunk moln.

tooconfigure och testa Azure AD enkel inloggning med Splunk Enterprise och Splunk molnet, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare Splunk Enterprise och Splunk moln](#creating-a-splunk-enterprise-and-splunk-cloud-test-user)**  -toohave en motsvarighet för Britta Simon i Splunk företag och Splunk moln som är länkade toohello Azure AD-representation av henne.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello klassiska portalen och konfigurera enkel inloggning i tillämpningsprogrammet Splunk Enterprise och Splunk moln.


**tooconfigure Azure AD enkel inloggning med Splunk Enterprise och Splunk moln, utför följande steg hello:**

1. I hello klassiska portalen på hello **Splunk Enterprise och Splunk moln** integreringssidan för programmet, klickar du på **Konfigurera enkel inloggning** tooopen hello **Konfigurera enkel inloggning** dialogrutan.
     
    ![Konfigurera enkel inloggning][6] 

2. På hello **hur vill du användare toosign på tooSplunk Enterprise och Splunk moln** väljer **Azure AD enkel inloggning**, och klicka sedan på **nästa**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_splunk_03.png) 

3. På hello **konfigurera Appinställningar** dialogrutan utför hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_splunk_04.png) 
  1. I hello **logga URL** textruta typen hello URL som används av dina användare toosign på tooyour Splunk Enterprise och Splunk moln program med hjälp av hello följande mönster:`https://<splunkserverUrl>/en-US/app/launcher/home`
  2. I hello **identifierare** textruta typen hello URL-Adressen till din Splunk.
  3. I hello **Reply URL** textruta typen hello URL med hello följande mönster:`https://<splunkserver>/saml/acs`
  4. Klicka på **Nästa**.
 
4. På hello **Konfigurera enkel inloggning på Splunk Enterprise och Splunk moln** utför hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_splunk_05.png)
  1. Klicka på **hämtar metadata**, och spara sedan hello på datorn.
  2. Klicka på **Nästa**.

5. tooget SSO konfigurerats för ditt program, kontakta Splunk Enterprise och Splunk moln supportteamet och ge dem hello följande:

    * hello hämtas **federaton metadata**
6. Välj hello konfiguration för enkel inloggning bekräftelse i hello klassiska portalen och klicka sedan på **nästa**.
    
    ![Azure AD-Single Sign-On][10]

7. På hello **enkel inloggning bekräftelse** klickar du på **Slutför**.  
 
    ![Azure AD-Single Sign-On][11]

### <a name="create-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
I det här avsnittet skapar du en testanvändare i hello klassiska portalen kallas Britta Simon.

![Skapa Azure AD-användare][20]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **klassiska Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Active Directory**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_09.png) 

2. Från hello **Directory** listan, Välj hello katalog som du vill tooenable katalogintegrering.

3. toodisplay hello lista över användare i hello menyn hello överst, klickar du på **användare**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_03.png) 

4. tooopen hello **Lägg till användare** i hello verktygsfältet på hello längst ned i dialogrutan klickar du på **Lägg till användare**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_04.png) 

5. På hello **berätta om den här användaren** dialogrutan utför hello följande steg:

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_05.png) 
  1. Välj ny användare i din organisation som typ av användare.
  2. I hello användarnamn **textruta**, typen **BrittaSimon**.
  3. Klicka på **Nästa**.

6.  På hello **användarprofil** dialogrutan utför hello följande steg:
  
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_06.png) 
  1. I hello **Förnamn** textruta typen **Britta**.  
  2. I hello **efternamn** textruta typ, **Simon**.
  3. I hello **visningsnamn** textruta typen **Britta Simon**.
  4. I hello **rollen** väljer **användaren**.
  5. Klicka på **Nästa**.

7. På hello **skaffa tillfälligt lösenord** dialogrutan sidan, klickar du på **skapa**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_07.png) 

8. På hello **skaffa tillfälligt lösenord** dialogrutan utför hello följande steg:

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_08.png) 
  1. Skriv ned hello värdet för hello **nytt lösenord**.
  2. Klicka på **Complete** (Slutför).   

### <a name="create-a-splunk-enterprise-and-splunk-cloud-test-user"></a>Skapa en Splunk Enterprise och Splunk moln testanvändare

I det här avsnittet kan du skapa en användare som kallas Britta Simon i Splunk företag och Splunk moln. Se tillsammans med Splunk Enterprise och Splunk Cloud support-teamet tooadd hello användare i hello Splunk Enterprise och Splunk molnplattform.


### <a name="assign-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan du aktivera Britta Simon toouse Azure SSOy bevilja sin åtkomst tooSplunk Enterprise och Splunk moln.

![Tilldela användare][200] 

**tooassign Britta Simon tooSplunk Enterprise och Splunk moln, utför hello följande steg:**

1. På hello klassiska portalen tooopen hello program i vyn hello directory vyn klickar du på **program** i hello huvudmenyn.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **Splunk Enterprise och Splunk moln**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_splunk_50.png) 

3. Hello-menyn överst hello **användare**.

    ![Tilldela användare][203]

4. Markera i hello användare **Britta Simon**.

5. Klicka i hello verktygsfältet hello längst ned **tilldela**.

    ![Tilldela användare][205]

### <a name="test-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan du testa din Azure AD-SSOonfiguration med hello åtkomstpanelen.

Du bör få automatiskt inloggade tooyour Splunk Enterprise och Splunk moln när du klickar på hello Splunk Enterprise och Splunk moln-panelen i hello åtkomstpanelen.


## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_205.png
