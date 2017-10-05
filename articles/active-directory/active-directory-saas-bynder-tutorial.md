---
title: "Självstudier: Azure Active Directory-integrering med Bynder | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Bynder."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 4fb0ab26-b3b9-420a-8072-a0be80ea021e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/17/2017
ms.author: jeedes
ms.openlocfilehash: 6786d7eb6a11405278ef7267f25279f9e39b3bde
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-bynder"></a>Självstudier: Azure Active Directory-integrering med Bynder
Syftet med den här kursen är att visa dig hur du integrerar Bynder med Azure Active Directory (AD Azure).

Integrera Bynder med Azure AD ger dig följande fördelar:

* Du kan styra i Azure AD som har åtkomst till Bynder
* Du kan aktivera användarna att automatiskt hämta loggat in på Bynder enkel inloggning (SSO) med sina Azure AD-konton
* Du kan hantera dina konton i en central plats – den klassiska Azure-portalen

Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav
För att konfigurera Azure AD-integrering med Bynder, behöver du följande:

* En Azure AD-prenumeration
* En Bynder enkel inloggning (SSO) aktiverat prenumeration

>[!NOTE]
>Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö. 
> 

Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:

* Du bör inte använda produktionsmiljön, om det inte är nödvändigt.
* Om du inte har en utvärderingsversion Azure AD-miljö kan du få en [utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
Syftet med den här kursen är att du ska testa Microsoft Azure AD SSO i en testmiljö.

Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till Bynder från galleriet
2. Konfigurera och testa Microsoft Azure AD-SSO

## <a name="add-bynder-from-the-gallery"></a>Lägg till Bynder från galleriet
Du måste lägga till Bynder från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Bynder i Azure AD.

**Utför följande steg för att lägga till Bynder från galleriet:**

1. I den **klassiska Azure-portalen**, klicka på det vänstra navigeringsfönstret **Active Directory**. 
   
    ![Active Directory][1]
2. Från den **Directory** listan, Välj den katalog som du vill aktivera katalogintegrering.
3. Klicka för att öppna vyn program i vyn directory **program** på huvudmenyn.
   
    ![Program][2]
4. Klicka på **Lägg till** längst ned på sidan.
   
    ![Program][3]
5. På den **vad vill du göra** dialogrutan klickar du på **lägga till ett program från galleriet**.
   
    ![Program][4]
6. I sökrutan skriver **Bynder**.
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_01.png)
7. Välj i resultatpanelen **Bynder**, och klicka sedan på **Slutför** lägga till programmet.
   
    ![Markera appen i galleriet](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_001.png)

## <a name="configure-and-test-microsoft-azure-ad-sso"></a>Konfigurera och testa Microsoft Azure AD-SSO
Syftet med det här avsnittet är att visa dig hur du konfigurerar och testa Microsoft Azure AD SSO med Bynder baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning ska fungera, måste Azure AD motsvarighet användaren i Bynder till en användare i Azure AD är okänt. Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Bynder upprättas.

Den här länken relationen upprättas genom att tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** i Bynder.

Om du vill konfigurera och testa Microsoft Azure AD SSO med Bynder, måste du utföra följande byggblock:

1. **[Konfigurera Microsoft Azure AD enkel inloggning](#configuring-azure-ad-single-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  – för att testa Microsoft Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare Bynder](#creating-a-bynder-test-user)**  – du har en motsvarighet för Britta Simon i Bynder som är kopplad till Azure AD-representation av henne.
4. **[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Microsoft Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.

### <a name="configuring-microsoft-azure-ad-sso"></a>Konfigurera Microsoft Azure AD-SSO
I det här avsnittet att aktivera Microsoft Azure AD SSO i den klassiska portalen och konfigurera enkel inloggning i tillämpningsprogrammet Bynder.

**Utför följande steg för att konfigurera Microsoft Azure AD SSO med Bynder:**

1. I den klassiska portalen på den **Bynder** integreringssidan för programmet, klickar du på **Konfigurera enkel inloggning** att öppna den **Konfigurera enkel inloggning** dialogrutan.
   
    ![Konfigurera enkel inloggning][6] 
2. På den **hur vill du att användarna kan logga in på Bynder** väljer **Microsoft Azure AD enkel inloggning**, och klicka sedan på **nästa**.
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_03.png)
3. På den **konfigurera Appinställningar** dialogrutan sidan om du vill konfigurera programmet i **IDP initierade läge**, utför följande steg och klickar på **nästa**:
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_04.png)
  1. I den **Reply URL** textruta Skriv en URL med följande mönster:`https://<company name>.getbynder.com/sso/SAML/authenticate/`
  2. Klicka på **Nästa**.
4. Om du vill konfigurera programmet i **SP initierade läge** på den **konfigurera Appinställningar** dialogrutan sidan, klickar på den **”visa avancerade inställningar (valfritt)”** och ange sedan den **logga URL** och klicka på **nästa**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_10.png)
  1. I den **logga URL** textruta Skriv en URL med följande mönster:`https://<company name>.getbynder.com/login/`
  2. Klicka på **Nästa**.
  
   >[!NOTE]
   >Värdet för inloggning på URL: en i den här kursen är bara en placeholfer. Kontakta Bynder för att få det faktiska värdet för din miljö.
   >

5. På den **Konfigurera enkel inloggning på Bynder** , utför följande steg och klickar på **nästa**:
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_05.png)  
  1. Klicka på **hämtar metadata**, och sedan spara filen på datorn.
  2. Klicka på **Nästa**.
6. Kontakta supportteamet Bynder för att få SSO konfigurerats för ditt program. Koppla hämtade metadatafilen och dela den med Bynder-teamet för att konfigurera enkel inloggning på sidan.
7. I den klassiska portalen markerar du konfigurationen för enkel inloggning bekräftelsen och klicka sedan på **nästa**.
   
    ![Azure AD-Single Sign-On][10]
8. På den **enkel inloggning bekräftelse** klickar du på **Slutför**.  
   
    ![Azure AD-Single Sign-On][11]

### <a name="create-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
Syftet med det här avsnittet är att skapa en testanvändare i den klassiska portalen kallas Britta Simon.

![Skapa Azure AD-användare][20]

**Utför följande steg för att skapa en testanvändare i Azure AD:**

1. I den **klassiska Azure-portalen**, klicka på det vänstra navigeringsfönstret **Active Directory**.
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bynder-tutorial/create_aaduser_09.png)
2. Från den **Directory** listan, Välj den katalog som du vill aktivera katalogintegrering.
3. Klicka för att visa en lista över användare, på menyn upp **användare**.
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bynder-tutorial/create_aaduser_03.png)
4. Öppna den **Lägg till användare** i verktygsfältet längst ned i dialogrutan klickar du på **Lägg till användare**.
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bynder-tutorial/create_aaduser_04.png)
5. På den **berätta om den här användaren** dialogrutan utför följande steg:
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bynder-tutorial/create_aaduser_05.png)
  1. Välj ny användare i din organisation som typ av användare.
  2. I användarnamnet **textruta**, typen **BrittaSimon**.
  3. Klicka på **Nästa**.
6. På den **användarprofil** dialogrutan utför följande steg:
   
   ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bynder-tutorial/create_aaduser_06.png)
  1. I den **Förnamn** textruta typen **Britta**.  
  2. I den **efternamn** textruta typ, **Simon**. 
  3. I den **visningsnamn** textruta typen **Britta Simon**.
  4. I den **rollen** väljer **användaren**.
  5. Klicka på **Nästa**.
7. På den **skaffa tillfälligt lösenord** dialogrutan sidan, klickar du på **skapa**.
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bynder-tutorial/create_aaduser_07.png)
8. På den **skaffa tillfälligt lösenord** dialogrutan utför följande steg:
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bynder-tutorial/create_aaduser_08.png)
   1. Anteckna värdet för den **nytt lösenord**.
   2. Klicka på **Complete** (Slutför).   

### <a name="create-a-bynder-test-user"></a>Skapa en testanvändare Bynder
Syftet med det här avsnittet är att skapa en användare som kallas Britta Simon i Bynder. Bynder stöder just-in-time-etablering, vilket är aktiverat som standard.

Det finns ingen åtgärd objekt i det här avsnittet. En ny användare skapas vid ett försök att komma åt Bynder om den inte finns.

>[!NOTE]
>Om du behöver skapa en användare manuellt måste du kontakta supportteamet Bynder. 
> 

### <a name="assign-the-azure-ad-test-user"></a>Tilldela Azure AD-testanvändare
Syftet med det här avsnittet är att aktivera Britta Simon att använda Azure SSO genom att bevilja sin åtkomst till Bynder.

   ![Tilldela användare][200]

**Om du vill tilldela Bynder Britta Simon utför du följande steg:**

1. På den klassiska portalen för att öppna vyn program i katalogen vyn klickar du på **program** på huvudmenyn.
   
    ![Tilldela användare][201]
2. Välj i listan med program **Bynder**.
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_50.png)
3. Klicka på menyn högst upp **användare**.
   
    ![Tilldela användare][203]
4. Välj i listan användare **Britta Simon**.
5. Klicka på i verktygsfältet längst ned i **tilldela**.
   
    ![Tilldela användare][205]

### <a name="test-single-sign-on"></a>Testa enkel inloggning
Syftet med det här avsnittet är att testa Microsoft Azure AD SSO-konfigurationen med hjälp av panelen åtkomst.

När du klickar på panelen Bynder på åtkomstpanelen du bör få automatiskt loggat in på ditt Bynder program.

## <a name="additional-resources"></a>Ytterligare resurser
* [Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_205.png
