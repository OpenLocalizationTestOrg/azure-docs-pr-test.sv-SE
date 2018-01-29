---
title: "Självstudier: Azure Active Directory-integrering med molntjänster Atlassian | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Atlassian moln."
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 729b8eb6-efc4-47fb-9f34-8998ca2c9545
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/05/2017
ms.author: jeedes
ms.openlocfilehash: db9e9c7ae8380612bac9d0aeaaaf6df78cba523f
ms.sourcegitcommit: d247d29b70bdb3044bff6a78443f275c4a943b11
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 12/13/2017
---
# <a name="tutorial-azure-active-directory-integration-with-atlassian-cloud"></a>Självstudier: Azure Active Directory-integrering med Atlassian moln

I kursen får lära du att integrera Atlassian moln med Azure Active Directory (AD Azure).

Integrera Atlassian moln med Azure AD ger dig följande fördelar:

- Du kan styra i Azure AD som har åtkomst till Atlassian moln.
- Du kan ge användarna vara inloggad automatiskt (enkel inloggning) till Atlassian moln med sina Azure AD-konton.
- Du kan hantera dina konton i en central plats, Azure-portalen.

Mer information om programvara som en tjänst (SaaS) appintegrering med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

För att konfigurera Azure AD-integrering med Atlassian molnet, behöver du följande:

- En Azure AD-prenumeration.
- Om du vill aktivera Security Assertion Markup Language (SAML) enkel inloggning för Atlassian moln produkter måste du konfigurera Identity Manager. Lär dig mer om [Identity Manager]( https://www.atlassian.com/enterprise/cloud/identity-manager).

> [!NOTE]
> När du testar stegen i den här kursen rekommenderar vi att du inte använder en produktionsmiljö.

Följ dessa rekommendationer för att testa stegen i den här självstudiekursen:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. Det scenario som beskrivs i kursen består av två huvudsakliga byggblock:

* Att lägga till Atlassian moln från galleriet
* Konfigurera och testa Azure AD enkel inloggning

## <a name="add-atlassian-cloud-from-the-gallery"></a>Lägg till Atlassian moln från galleriet
För att konfigurera integrering av Atlassian moln med Azure AD, lägga till Atlassian moln från galleriet i listan över hanterade SaaS-appar genom att göra följande:

1. I den [Azure-portalen](https://portal.azure.com), i den vänstra rutan, Välj den **Azure Active Directory** knappen. 

    ![Azure Active Directory-knappen][1]

2. Välj **företagsprogram** > **alla program**.

    ![Fönstret Enterprise program][2]
    
3. Om du vill lägga till ett program, Välj **nytt program**.

    ![”Det nya programmet” knappen][3]

4. I sökrutan skriver **Atlassian moln**, Välj i resultatlistan **Atlassian moln**, och välj sedan **Lägg till**.

    ![Atlassian moln i resultatlistan](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning

I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Atlassian moln, baserat på en användare med namnet *Britta Simon*.

För enkel inloggning ska fungera, måste Azure AD att identifiera användaren Atlassian molnet och dess motsvarighet i Azure AD. Med andra ord måste du upprätta en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Atlassian molnet.

Om du vill upprätta en länk relation, tilldela som Atlassian molnet *användarnamn* samma värde som är tilldelad till Azure AD *användarnamn*.

Du måste utföra byggblock i följande avsnitt för att konfigurera och testa Azure AD enkel inloggning med Atlassian moln.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet Atlassian moln.

För att konfigurera Azure AD enkel inloggning med Atlassian molnet, gör du följande:

1. I Azure-portalen i den **Atlassian moln** programmet integration fönstret, Välj **enkel inloggning**.

    ![Konfigurera enkel inloggning länk][4]

2. I den **enkel inloggning** fönster i den **läget för enkel inloggning** väljer **SAML-baserade inloggning**.
 
    ![Fönster för enkel inloggning](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_samlbase.png)

3. Konfigurera programmet i IDP-initierad läge under **Atlassian moln domän och URL: er**, gör du följande:

    ![URL: er och Atlassian moln domän med enkel inloggning information](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_url.png)
    
    a. I den **identifierare** skriver  **`https://auth.atlassian.com/saml/<unique ID>`** .
    
    b. I den **Reply URL** skriver  **`https://auth.atlassian.com/login/callback?connection=saml-<unique ID>`** .

    c. I den **Relay tillstånd** Skriv en URL med följande syntax:  **`https://<instancename>.atlassian.net`** .

4. För att konfigurera programmet i SP-initierad läge, Välj den **visa avancerade inställningar för URL: en** och sedan, i den **inloggning URL** Skriv en URL med följande syntax:  **`https://<instancename>.atlassian.net`**  .

    ![URL: er och Atlassian moln domän med enkel inloggning information](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_url1.png)

    > [!NOTE] 
    > Föregående värden är inte verkliga. Uppdatera dem med faktiska identifierare, reply-URL och inloggning URL-värden. Du kan hämta de verkliga värden från skärmen Atlassian Molnkonfigurationen SAML. Vi förklarar värdena senare under kursen.

5. Under **SAML-signeringscertifikat**väljer **Certificate(Base64)**, och sedan spara certifikatfilen på datorn.

    ![Länken hämta certifikatet](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_certificate.png) 

6. Tillämpningsprogrammet Atlassian moln förväntas SAML-intyg i ett specifikt format, vilket kräver att du kan lägga till anpassade attributmappning konfigurationen av SAML-Token attribut. 

    Som standard den **användar-ID** värde är mappad till user.userprincipalname. Ändra det här värdet ska mappas till user.mail. Du kan också välja ett annat lämpligt värde enligt konfigurationen i din organisation, men i de flesta fall, e-post ska fungera.

    ![Länken hämta certifikatet](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_attribute.png) 

7. Välj **Spara**.

    ![Den Konfigurera enkel inloggning spara knappen](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_400.png)

8. Öppna den **konfigurera inloggning** fönster i den **Atlassian Molnkonfigurationen** väljer **konfigurera Atlassian moln**. 

9. I den **Snabbreferens** avsnittet, kopiera den **SAML enhets-ID** och **SAML inloggning tjänst-URL för enkel**. 

    ![Atlassian molnkonfigurationen](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_configure.png) 

10. Logga in på Atlassian-portal med administratörsautentiseringsuppgifter för att få SSO konfigurerats för ditt program.

11. Gå till **Atlassian Platsadministration** > **organisationer och säkerhet**. Om du inte redan gjort det, skapa och namnge din organisation och i den vänstra rutan, markera **domäner**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_06.png)

12. Välj hur du vill verifiera din domän: **DNS** eller **HTTPS**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_17.png)

13. Om DNS-kontroll i den **domäner** väljer den **DNS** fliken och gör sedan följande:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_18.png)

    a. Värdet för posten text (TXT-post) och markera **kopiera**.

    b. Gå till inställningssidan i din DNS för att lägga till en post.

    c. Markera alternativet för att lägga till en ny post och klistra in det värde som du kopierade i den **domäner** fönster för att den **värdet** fältet. DNS-post kan även gå till den som **svar** eller **beskrivning**.

    d. DNS-post kan också innehålla följande fält:
    
    * I den **posttyp** ange **TXT**.
    * I den **namn-värd-Alias** rutan, låt standardvärdet (@ eller tomt).
    * I den **tid att live (TTL)** ange **86400**.
    
    e.  Spara posten.

14. Gå tillbaka till den **domäner** fönstret Organisationsadministration och välj sedan **verifiera domän**. I den **domän** rutan, skriver du ditt domännamn och välj sedan **verifiera domän**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_19.png)  

    > [!NOTE]
    > Eftersom det kan ta upp till 72 timmar för TXT-posten ändringarna ska börja gälla, vet du inte direkt om domänen verifieringen lyckades. Om du vill visa status för verifiering, kontrollera den **domäner** fönstret strax efter att du har slutfört den här proceduren. Uppdaterade status visas som *verifierad*, enligt följande bild:
    > 
    > ![Konfigurera enkel inloggning](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_20.png)
    > 
    > 

15. För HTTPS-kontrollen i den **domäner** väljer den **HTTPS** fliken och gör sedan följande:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_21.png)

    a. Om du vill hämta HTML-fil, Välj **filen**.

    b. Överför HTML-fil till rotkatalogen för din domän.

16. Gå tillbaka till den **domäner** i Organisationsadministration och väljer **verifiera domän**. I den **verifiera domän** fönster i den **domän** Skriv din **domännamn**, och välj sedan **verifiera domän**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_22.png)

17. Om verifieringsprocessen kan hitta den fil som du har överfört i rotkatalogen kan uppdateras status för domänen till *verifierad*, som visas här:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_23.png)

    > [!NOTE]
    > Mer information finns i [Atlassian domänverifiering](https://confluence.atlassian.com/cloud/domain-verification-873871234.html).

18. I den vänstra rutan, Välj **SAML enkel inloggning**. Om du inte redan har gjort prenumerera till Atlassian Identity Manager.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_11.png)

19. I den **lägga till SAML-konfiguration** fönster, gör du följande:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_12.png)

    a. I den **identitetsleverantör enhets-ID** klistra in SAML enhets-ID som du kopierade från Azure-portalen.

    b. I den **identitetsleverantör SSO URL** klistra in URL för SAML-tjänst för enkel inloggning som du kopierade från Azure-portalen.

    c. Öppna hämtat certifikat från Azure-portalen i en txt-fil, Kopiera värdet (utan den *börjar certifikat* och *End Certificate* rader), och klistra in den i den **offentliga X509 certifikatet** rutan.
    
    d. Välj **spara konfigurationen**.
     
20. Uppdatera Azure AD-inställningar för att säkerställa att du har konfigurerat rätt URL-adresser, genom att göra följande:
  
    ![Konfigurera enkel inloggning](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_13.png)

    a. I fönstret SAML kopiera den **SP identitet ID** och sedan, i Azure-portalen under Atlassian moln **domän och URL: er**, klistra in den i den **identifierare** rutan.
    
    b. I fönstret SAML kopiera den **SP Assertion konsument-tjänstens URL** och sedan, i Azure-portalen under Atlassian moln **domän och URL: er**, klistra in den i den **Reply URL** rutan.  
        URL för inloggning är klient-URL för ditt Atlassian moln. 

    > [!NOTE]
    > Om du är en befintlig kund när du har uppdaterat den **SP identitet ID** och **SP Assertion konsument-tjänstens URL** värden i Azure-portalen, markera **Ja, uppdatera konfigurationen**. Om du är en ny kund kan du hoppa över det här steget. 
    
21. Välj i Azure-portalen **spara**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_400.png)

> [!TIP]
> När du konfigurerar appen, kan du läsa en kortare version av föregående anvisningarna i den [Azure-portalen](https://portal.azure.com). När du lägger till den här appen från den **Active Directory** > **företagsprogram** väljer den **enkel inloggning** fliken och sedan använda den inbäddade dokumentation i den **Configuration** avsnittet längst ned i fönstret. Mer information finns i [Azure AD inbäddade dokumentationen]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD

I det här avsnittet skapar du testanvändare Britta Simon i Azure-portalen genom att göra följande:

   ![Skapa en testanvändare i Azure AD][100]

1. I Azure-portalen i den vänstra rutan, Välj den **Azure Active Directory** knappen.

    ![Azure Active Directory-knappen](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_01.png)

2. Om du vill visa en lista över användare, Välj **användare och grupper** > **alla användare**.

    ![”Användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_02.png)

3. I den **alla användare** väljer **Lägg till**.

    ![Knappen Lägg till](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_03.png)

4. I den **användaren** fönster, gör du följande:

    ![Fönstret användare](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_04.png)

    a. I den **namn** skriver **BrittaSimon**.

    b. I den **användarnamn** Skriv användarens Britta Simon e-postadress.

    c. Välj den **visa lösenordet** kryssrutan och sedan skriva ned det värde som visas i den **lösenord** rutan.

    d. Välj **Skapa**.
  
### <a name="create-an-atlassian-cloud-test-user"></a>Skapa en testanvändare Atlassian moln

Om du vill aktivera Azure AD-användare att logga in på molnet Atlassian etablera användarkonton manuellt i Atlassian molnet genom att göra följande:

1. I den **Administration** väljer **användare**.

    ![Länken Atlassian Molnanvändare](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_14.png) 

2. Om du vill skapa en användare i Atlassian molnet **bjuda in användare**.

    ![Skapa en Atlassian moln-användare](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_15.png) 

3. I den **e-postadress** rutan Ange användarens e-postadress och sedan tilldela åtkomst till program. 

    ![Skapa en Atlassian moln-användare](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_16.png)
 
4. Om du vill skicka ett e-postinbjudan för användaren, Välj **bjuda in användare**.  
    En e-postinbjudan skickas till användaren och användaren är aktiv i systemet efter att acceptera inbjudan. 

>[!NOTE] 
>Du kan också massimportera-skapa användare genom att välja den **Bulk skapa** knappen i den **användare** avsnitt.

### <a name="assign-the-azure-ad-test-user"></a>Tilldela Azure AD-testanvändare

I det här avsnittet kan du låta användare Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Atlassian moln. Det gör du genom att göra följande:

![Tilldela rollen][200] 

1. I Azure-portalen öppnar den **program** visa, gå till vyn directory och välj sedan **företagsprogram** > **alla program**.

    ![Tilldela användare][201] 

2. I den **program** väljer **Atlassian moln**.

    ![Länken Atlassian moln i listan med program](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_app.png)  

3. I den vänstra rutan, Välj **användare och grupper**.

    ![Länken ”användare och grupper”][202]

4. Välj **Lägg till** och sedan, i den **Lägg uppdrag** väljer **användare och grupper**.

    ![Fönstret Lägg till tilldelning][203]

5. I den **användare och grupper** fönster i den **användare** väljer **Britta Simon**.

6. I den **användare och grupper** väljer **Välj**.

7. I den **Lägg uppdrag** väljer **tilldela**.
    
### <a name="test-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet testa du Azure AD enkel inloggning konfigurationen med hjälp av åtkomstpanelen.

När du väljer den **Atlassian moln** panelen i panelen åtkomst du bör vara inloggad automatiskt i tillämpningsprogrammet Atlassian moln.
Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_203.png

