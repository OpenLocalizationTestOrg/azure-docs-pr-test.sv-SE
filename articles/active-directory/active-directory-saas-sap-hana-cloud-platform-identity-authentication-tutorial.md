---
title: "Självstudier: Azure Active Directory-integrering med SAP HANA Cloud Platform identitetsverifiering | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och SAP HANA Cloud Platform identitetsverifiering."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 1c1320d1-7ba4-4b5f-926f-4996b44d9b5e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: 7799bf03cc6705f805a48f329a265a3d84bed55f
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sap-hana-cloud-platform-identity-authentication"></a>Självstudier: Azure Active Directory-integrering med SAP HANA Cloud Platform identitetsverifiering

I kursen får lära du att integrera identitetsverifiering för SAP HANA moln plattform med Azure Active Directory (AD Azure). Identitetsverifiering för SAP HANA Cloud Platform används som en proxy IdP åtkomst SAP-program med Azure AD som huvudsakliga IdP.

Integrera identitetsverifiering för SAP HANA moln plattform med Azure AD ger dig följande fördelar:

- Du kan styra i Azure AD som har åtkomst till SAP-program
- Du kan aktivera användarna att automatiskt hämta loggat in på SAP-program enkel inloggning (SSO) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats – den klassiska Azure-portalen

Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).


## <a name="prerequisites"></a>Krav

För att konfigurera Azure AD-integrering med SAP HANA Cloud Platform identitetsverifiering, behöver du följande:

- En Azure AD-prenumeration
- En **identitetsverifiering för SAP HANA Cloud Platform** SSO aktiverad prenumeration


>[!NOTE] 
>Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.
>

Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:

- Du bör inte använda produktionsmiljön, om det inte är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du få en [utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö.

Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till SAP HANA Cloud Platform identitetsverifiering från galleriet
2. Konfigurera och testa Azure AD-SSO

Innan du dyker till teknisk information, är det viktigt att förstå de begrepp som du ska titta på. Identitetsverifiering för SAP HANA Cloud Platform och Azure Active Directory federation gör att du kan implementera enkel inloggning över program eller tjänster som skyddas av AAD (som en IdP) med SAP-program och tjänster som skyddas av SAP HANA Cloud Platform identitet Autentisering.

För närvarande fungerar identitetsverifiering för SAP HANA Cloud Platform som en Proxy-identitetsprovider till SAP-program. Azure Active Directory fungerar i sin tur som inledande identitetsleverantören i den här installationen. 

Följande diagram illustrerar detta:    

![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/architecture-01.png)

Med den här installationen konfigureras din SAP HANA Cloud Platform identitetsverifiering klient som ett betrodda program i Azure Active Directory. 

Alla SAP-program och tjänster som du vill skydda via det här sättet konfigureras senare i hanteringskonsolen för SAP HANA Cloud Platform identitetsverifiering!

Det innebär att auktorisering för att bevilja åtkomst till SAP-program och tjänster måste äga rum i identitetsverifiering för SAP HANA moln plattform för en sådan konfiguration (i stället konfigurerar auktorisering i Azure Active Directory).

Genom att konfigurera identitetsverifiering för SAP HANA moln plattform som ett program via Azure Active Directory Marketplace, behöver du inte ta hand om att konfigurera nödvändiga enskilda anspråk / SAML intyg och omvandlingar som behövs för att producera ett giltigt Autentiseringstoken för SAP-program.

>[!NOTE] 
>För närvarande har Web SSO testats av båda parter endast. Flöden som krävs för App-API och API-API-kommunikation ska fungera men har inte testats, ännu. De kommer att testas som en del av efterföljande aktiviteter.
>

## <a name="add-sap-hana-cloud-platform-identity-authentication-from-the-gallery"></a>Lägg till SAP HANA Cloud Platform identitetsverifiering från galleriet
Du måste lägga till SAP HANA Cloud Platform identitetsverifiering från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av identitetsverifiering för SAP HANA Cloud-plattformen i Azure AD.

**Utför följande steg för att lägga till SAP HANA Cloud Platform identitetsverifiering från galleriet:**

1. I den [ **Azure-hanteringsportalen**](https://portal.azure.com), klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Gå till **företagsprogram**. Gå till **alla program**.

    ![Program][2]
    
3. Klicka på **Lägg till** knappen överst i dialogrutan.

    ![Program][3]

4. I sökrutan skriver **identitetsverifiering för SAP HANA Cloud Platform**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_01.png)

5. Välj i resultatpanelen **identitetsverifiering för SAP HANA Cloud Platform**, och klicka sedan på **Lägg till** för att lägga till programmet.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_02.png)


##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD SSO med SAP HANA Cloud Platform identitetsverifiering baserat på en testanvändare som kallas ”Britta Simon”.

För SSO ska fungera måste Azure AD du känna till motsvarande användaren i identitetsverifiering för SAP HANA Cloud Platform till en användare i Azure AD. Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i SAP HANA Cloud Platform identitetsverifiering upprättas.

Den här länken relationen upprättas genom att tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** i identitetsverifiering för SAP HANA moln plattform.

Om du vill konfigurera och testa Azure AD SSO med SAP HANA Cloud Platform identitetsverifiering, måste du utföra följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare för SAP HANA Cloud Platform identitetsverifiering](#creating-a-sap-hana-cloud-platform-identity-authentication-test-user)**  – du har en motsvarighet för Britta Simon i SAP HANA Cloud Platform identitetsverifiering som är kopplad till Azure AD-representation av henne.
4. **[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.

### <a name="configuring-azure-ad-sso"></a>Konfigurera Azure AD för enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-hanteringsportalen och konfigurera enkel inloggning i din app för identitetsverifiering för SAP HANA moln plattform.

Identitetsverifiering för SAP HANA Cloud Platform program förväntar SAML-intyg i ett specifikt format. Du kan hantera värden för attributen från den ”**användarattribut**” avsnitt på sidan för integrering av programmet. Följande skärmbild visar ett exempel för det här.

![Konfigurera enkel inloggning](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_03.png)

**Utför följande steg för att konfigurera Azure AD SSO med identitetsverifiering för SAP HANA Cloud Platform:**

1. I Azure-hanteringsportalen på den **identitetsverifiering för SAP HANA Cloud Platform** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På den **enkel inloggning** dialogrutan som **läge** Välj **SAML-baserade inloggning** att aktivera enkel inloggning på.
 
    ![Konfigurera enkel inloggning][5]

3. I den **användarattribut** avsnitt på den **enkel inloggning** dialogrutan om tillämpningsprogrammet SAP förväntar sig ett attribut till exempel ”förnamn”. Lägg till attributet ”förnamn” i dialogrutan för SAML-token attribut.
 1. Klicka på **Lägg till attributet** att öppna den **lägga till attributet** dialogrutan.
 
    ![Konfigurera enkel inloggning][6]

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_05.png)
 2. I den **attributnamn** textruta ange attributet name ”förnamn”.
 3. Från den **attributvärdet** väljer attributvärdet ”user.givenname”.
 4. Klicka på **OK**.

4. På den **SAP HANA Cloud Platform identitet autentisering domän och URL: er** avsnittet, utför följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_06.png)
 1. I den **logga URL** textruta anger tecknet på URL: en för SAP-program.
 2. I den **identifierare** textruta Skriv det värde som följer mönstret:`<entity-id>.accounts.ondemand.com` 
    * Om du inte vet det här värdet, följ dokumentationen för SAP HANA Cloud Platform identitetsverifiering på [klientkonfiguration SAML 2.0](https://help.hana.ondemand.com/cloud_identity/frameset.htm?e81a19b0067f4646982d7200a8dab3ca.html).

5. På den **konfiguration för SAP HANA Cloud Platform identitet verifiering** klickar du på **konfigurera SAP HANA Cloud Platform identitetsverifiering** att öppna **konfigurerainloggning** dialogrutan. Klicka på **SAML XML-Metadata** och spara filen på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_07.png) 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_08.png)

6. Gå till SAP HANA Cloud Platform identitet autentisering administrationskonsolen för att få SSO konfigurerats för ditt program. URL som har följande mönster:`https://<tenant-id>.accounts.ondemand.com/admin`
 * Följ dokumentationen på identitetsverifiering för SAP HANA Cloud Platform till [konfigurera Microsoft Azure AD som företagets identitetsleverantör vid SAP HANA Cloud Platform identitetsverifiering](https://help.hana.ondemand.com/cloud_identity/frameset.htm?626b17331b4d4014b8790d3aea70b240.html). 

7. Klicka på Azure-hanteringsportalen **spara** knappen.
8. Fortsätt följande steg om du vill lägga till och aktivera enkel inloggning för ett annat SAP-program. Upprepa steg under avsnittet ”lägger till SAP HANA-plattformen identitetsautentisering i molnet från galleriet” att lägga till en annan instans av identitetsverifiering för SAP HANA moln plattform.
9. I Azure-hanteringsportalen på den **identitetsverifiering för SAP HANA Cloud Platform** integreringssidan för programmet, klickar du på **inloggning länkade**.

    ![Konfigurera länkad inloggning](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/linked_sign_on.png)
10. Spara konfigurationen.

>[!NOTE] 
>Det nya programmet använder SSO-konfigurationen för det tidigare SAP-programmet. Kontrollera att du använder samma företagets identitetsleverantörer i SAP HANA Cloud Platform identitet autentisering-administrationskonsolen.
>

### <a name="create-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
Syftet med det här avsnittet är att skapa en testanvändare i den nya portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**Utför följande steg för att skapa en testanvändare i Azure AD:**

1. I den **Azure-hanteringsportalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/create_aaduser_01.png) 

2. Gå till **användare och grupper** och på **alla användare** att visa en lista över användare.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/create_aaduser_02.png) 

3. Klicka på överst i dialogrutan **Lägg till** att öppna den **användaren** dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/create_aaduser_03.png) 

4. På den **användaren** dialogrutan utför följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/create_aaduser_04.png) 
  1. I den **namn** textruta typen **BrittaSimon**.
  2. I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.
  3. Välj **visa lösenordet** och anteckna värdet för den **lösenord**.
  4. Klicka på **Skapa**. 

### <a name="create-a-sap-hana-cloud-platform-identity-authentication-test-user"></a>Skapa en SAP HANA Cloud Platform identitetsverifiering testanvändare

Du behöver inte skapa en användare på identitetsverifiering för SAP HANA moln plattform. Användare i arkivet för Azure AD-användare kan använda funktionen för enkel inloggning.

SAP HANA Cloud Platform identitetsverifiering har stöd för identitetsfederation-alternativet. Det här alternativet innebär att du kan kontrollera om de användare som autentiseras av företagets identitetsleverantören finns i den store för SAP HANA Cloud Platform identitet användarautentisering. 

I standardinställningen, är identitetsfederation-alternativet inaktiverat. Om identitetsfederation är aktiverad kan endast användare som importeras i identitetsverifiering för SAP HANA Cloud Platform åtkomst till programmet. 

Mer information om hur du aktiverar eller inaktiverar identitetsfederation med identitetsverifiering för SAP HANA Cloud Platform finns i Aktivera identitetsfederation med identitetsverifiering för SAP HANA Cloud Platform i [konfigurera identitetsfederation med den Store SAP HANA Cloud Platform identitet användarautentiseringen. ](https://help.hana.ondemand.com/cloud_identity/frameset.htm?c029bbbaefbf4350af15115396ba14e2.html).

### <a name="assign-the-azure-ad-test-user"></a>Tilldela Azure AD-testanvändare

I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja sin åtkomst till SAP HANA Cloud Platform identitetsverifiering.

![Tilldela användare][200] 

**Om du vill tilldela identitetsverifiering för SAP HANA Cloud Platform Britta Simon utför du följande steg:**

1. Öppna vyn program i Azure-hanteringsportalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program **identitetsverifiering för SAP HANA Cloud Platform**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_09.png)

3. Klicka på menyn till vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** på listan användare.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    

### <a name="test-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan du testa din Azure AD SSO-konfiguration med hjälp av åtkomstpanelen.

När du klickar på panelen identitetsverifiering för SAP HANA Cloud Platform på åtkomstpanelen du ska hämta automatiskt loggat in på ditt program för SAP HANA Cloud Platform identitetsverifiering.


## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_06.png

[100]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_203.png