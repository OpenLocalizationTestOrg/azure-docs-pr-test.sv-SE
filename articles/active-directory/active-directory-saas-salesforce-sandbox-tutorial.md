---
title: "Självstudier: Azure Active Directory-integrering med Salesforce Sandbox | Microsoft Docs"
description: "Lär dig hur toouse Salesforce Sandbox med Azure Active Directory tooenable enkel inloggning, Automatisk etablering och mycket mer!."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: ee54c39e-ce20-42a4-8531-da7b5f40f57c
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/21/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: deccd6a07b57c8ba56b7e1c3f3ab311813ca017b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-salesforce-sandbox"></a>Självstudier: Azure Active Directory-integrering med Salesforce Sandbox

hello syftet med den här kursen är tooshow hello integreringen av Azure och Salesforce Sandbox.  

>[!TIP]
>Feedback, finns hello [Azure supportsidan](http://go.microsoft.com/fwlink/?LinkId=521878). 
> 

Sandboxar ger du hello möjlighet toocreate flera kopior av din organisation i olika miljöer för olika syften, till exempel utveckling, testning och utbildning utan säkerhetsfunktionerna hello data och program i produktionsmiljön Salesforce organisation.  

Mer information finns i [Sandbox-översikt](https://help.salesforce.com/HTViewHelpDoc?id=create_test_instance.htm&language=en_US)

hello-scenario som beskrivs i den här kursen förutsätter att du redan har hello följande objekt:

* En giltig Azure-prenumeration
* En sandbox i Salesforce.com

Om du inte har en giltig sandbox i Salesforce.com ännu, måste toocontact Salesforce.

hello-scenario som beskrivs i den här kursen består av följande byggblock hello:

1. Att aktivera programmet hello-integrering för Salesforce Sandbox
2. Konfigurera enkel inloggning (SSO)
3. Aktivera din domän
4. Konfigurera användaretablering
5. Tilldela användare

![Scenariot](./media/active-directory-saas-salesforce-sandbox-tutorial/IC769571.png "Scenario")

## <a name="enable-hello-application-integration-for-salesforce-sandbox"></a>Aktivera hello programmet integration för Salesforce Sandbox
hello syftet med det här avsnittet är toooutline hur tooenable hello programintegrationstyp för Salesforce begränsat läge.

**tooenable hello programintegrationstyp för Salesforce sandbox utför hello följande steg:**

1. I hello klassiska Azure-portalen på hello vänstra navigationsfönstret klickar du på **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-salesforce-sandbox-tutorial/IC700993.png "Active Directory")
2. Från hello **Directory** listan, Välj hello katalog som du vill tooenable katalogintegrering.
3. tooopen hello program i vyn hello directory vyn klickar du på **program** i hello huvudmenyn.
   
   ![Program](./media/active-directory-saas-salesforce-sandbox-tutorial/IC700994.png "program")
4. tooopen hello **Programgalleriet**, klickar du på **Lägg till en App**, och klicka sedan på **lägga till ett program för min organisation toouse**.
   
   ![Vad vill du vill toodo? ] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC700995.png "Vad vill du vill toodo?")
5. I hello **sökrutan**, typen **Salesforce Sandbox**.
   
   ![Programgalleriet](./media/active-directory-saas-salesforce-sandbox-tutorial/IC710978.png "Programgalleriet")
6. I resultatfönstret hello väljer **Salesforce Sandbox**, och klicka sedan på **Slutför** tooadd hello program.
   
   ![Salesforce Sandbox](./media/active-directory-saas-salesforce-sandbox-tutorial/IC746474.png "Salesforce Sandbox")
   
## <a name="configur-single-sign-on-sso"></a>Configur enkel inloggning (SSO)

hello syftet med det här avsnittet är toooutline hur tooenable användare tooauthenticate tooSalesforce med sitt konto i Azure AD med hjälp av federation baserat på hello SAML-protokoll.

**tooconfigure enkel inloggning, utföra hello följande steg:**

1. I hello klassiska Azure-portalen på hello **Salesforce Sandbox** integreringssidan för programmet, klickar du på **Konfigurera enkel inloggning** tooopen hello **Konfigurera enkel inloggning** dialogrutan.
   
   ![Konfigurera enkel inloggning](./media/active-directory-saas-salesforce-sandbox-tutorial/IC749323.png "Konfigurera enkel inloggning")
2. På hello **hur skulle du som användare toosign på tooSalesforce Sandbox** väljer **Microsoft Azure AD enkel inloggning**, och klicka sedan på **nästa**.
   
   ![Salesforce Sandbox](./media/active-directory-saas-salesforce-sandbox-tutorial/IC746479.png "Salesforce Sandbox")
3. På hello **konfigurera App-URL** i hello sidan **logga URL** textruta Skriv URL: en med hjälp av följande mönster hello `http://company.my.salesforce.com`, och klicka sedan på **nästa**.
   
   ![Konfigurera App-URL](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781022.png "konfigurera App-URL")
4. Om du redan har konfigurerat enkel inloggning för en annan Salesforce Sandbox-instans i katalogen, så du måste också konfigurera hello **identifierare** toohave hello samma värde som hello **inloggning URL**. 
 * Hej **identifierare** fältet finns genom att kontrollera hello **visa avancerade inställningar** kryssruta på hello **konfigurera App-URL** sidan hello dialogrutan.
5. På hello **Konfigurera enkel inloggning på Salesforce Sandbox** klickar du på **hämta certifikat**, och sedan spara hello certifikatfilen på datorn.
   
   ![Konfigurera enkel inloggning](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781023.png "Konfigurera enkel inloggning")
6. Logga in på ditt Salesforce-sandbox som en administratör i en annan webbläsarfönster.
7. Hello-menyn överst hello **installationsprogrammet**.
   
   ![Installationsprogrammet](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781024.png "installationen")
8. Hello navigeringsfönstret hello vänster, klicka på **säkerhetsåtgärder**, och klicka sedan på **inställningar för enkel inloggning**.
   
   ![Enkel inloggning inställningar](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781025.png "enkel inloggning inställningar")
9. Utför följande hello på hello inställningar för enkel inloggning:
   
   ![Enkel inloggning inställningar](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781026.png "enkel inloggning inställningar")  
 1.  Välj **SAML aktiverat**. 
 2.  Klicka på **Ny**.
10. Utför följande hello på hello SAML enkel inloggning inställningar:
    
    ![SAML enkel inloggning inställningar](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781027.png "SAML enkel inloggning inställningar")  
 1. I textrutan hello, skriver hello namnet på hello-konfiguration (t.ex.: *SPSSOWAAD\_Test*). 
 2. I hello klassiska Azure-portalen på hello **Konfigurera enkel inloggning på Salesforce Sandbox** dialog sida, kopiera hello **utfärdar-URL** värdet och klistrar in den i hello **utfärdaren**textruta.
 3. I hello **enhets-Id** textruta typen **https://test.salesforce.com** om det är hello första Salesforce Sandbox-instans som du lägger till tooyour directory. Om du redan har lagt till en instans av Salesforce begränsat, och därefter för hello **enhets-ID** typen i hello **logga URL**, som ska vara i formatet:`http://company.my.salesforce.com`   
 4. Klicka på **Bläddra** tooupload hello hämtat certifikat.  
 5. Som **SAML identitetstyp**väljer **Assertion innehåller hello Federation ID från hello användarobjektet**. 
 6. Som **SAML identitet plats**väljer **identitet är i hello NameIdentifier elementet i hello ämne instruktionen**.
 7. I hello klassiska Azure-portalen på hello **Konfigurera enkel inloggning på Salesforce Sandbox** dialog sida, kopiera hello **Remote inloggnings-URL** värdet och klistrar in den i hello **identitetsleverantören. Inloggnings-URL** textruta. 
 8. SFDC har inte stöd för SAML logga ut.  Som en tillfällig lösning kan du klistra in 'https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.0' till hello **identitet providern logga ut URL** textruta.
 9. Som **providern initierade begära Tjänstbindning**väljer **HTTP POST**. 
 10. Klicka på **Spara**.
11. Välj hello konfiguration för enkel inloggning bekräftelse på hello klassiska Azure-portalen, och klicka sedan på **Slutför** tooclose hello **Konfigurera enkel inloggning** dialogrutan.
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781028.png "Konfigurera enkel inloggning")

## <a name="enable-your-domain"></a>Aktivera din domän
Det här avsnittet förutsätter att du redan har skapat en domän.  Mer information finns i [definierar domännamn](https://help.salesforce.com/HTViewHelpDoc?id=domain_name_define.htm&language=en_US).

**tooenable din domän, utföra hello följande steg:**

1. Hello vänstra navigeringsfönstret, klicka på **domänhantering**, och klicka sedan på **min domän.**
   
   ![Min domän](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781029.png "min domän")
   
   >[!NOTE]
   >Kontrollera att din domän har konfigurerats korrekt. 
   > 
2. I hello **sidan inloggningsinställningar** klickar du på **redigera**, sedan som **Autentiseringstjänsten**väljer hello namnet på hello SAML enkel inloggning inställningen från hello tidigare avsnittet och klicka slutligen på **spara**.
   
   ![Min domän](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781030.png "min domän")

Så snart du har en domän som har konfigurerats, bör användarna använda hello domän URL toologin toohello Salesforce sandbox.  

tooget hello värdet för hello URL på hello SSO profil du har skapat i föregående avsnitt i hello.

## <a name="configure-user-provisioning"></a>Konfigurera användaretablering
hello syftet med det här avsnittet är toooutline hur tooenable användaretablering Active Directory-användare konton tooSalesforce Sandbox.

**tooconfigure användaretablering, utför följande steg hello:**

1. Välj namn-tooexpand användare-menyn i hello Salesforce-portalen i hello övre navigeringsfältet:
   
   ![Mina inställningar](./media/active-directory-saas-salesforce-sandbox-tutorial/IC698773.png "Mina inställningar")
2. Användare-menyn, Välj **Mina inställningar** tooopen din **Mina inställningar** sidan.
3. Hello vänster klickar du på **personliga** tooexpand hello personliga avsnittet och klicka sedan på **Återställ mina säkerhetstoken**:
   
   ![Mina inställningar](./media/active-directory-saas-salesforce-sandbox-tutorial/IC698774.png "Mina inställningar")
4. På hello **Återställ mina säkerhetstoken** klickar du på **återställa säkerhetstoken** toorequest ett e-postmeddelande som innehåller din Salesforce.com-säkerhetstoken.
   
   ![Ny Token](./media/active-directory-saas-salesforce-sandbox-tutorial/IC698776.png "ny Token")
5. Kontrollera din inkorg för e-post från Salesforce.com med ”**salesforce.com.com säkerhet bekräftelse**” som ämne.
6. Granska den här e-post och kopiera hello säkerhet token-värde.
7. I hello klassiska Azure-portalen på hello **salesforce Sandbox** integreringssidan för programmet, klickar du på **konfigurera användaretablering** tooopen hello **konfigurera Användaretablering**dialogrutan.
   
   ![Konfigurera användaretablering](./media/active-directory-saas-salesforce-sandbox-tutorial/IC769573.png "konfigurera användaretablering")
8. På hello **ange ditt Salesforce Sandbox autentiseringsuppgifter tooenable automatisk användaretablering** anger hello följande inställningar:
   
   ![Salesforce Sandbox](./media/active-directory-saas-salesforce-sandbox-tutorial/IC746476.png "Salesforce Sandbox")   
 1. I hello **Salesforce Sandbox administratörsanvändarnamnet** textruta typen en Salesforce sandbox kontonamn som har hello **systemadministratören** profil i Salesforce.com som tilldelats.
 2. I hello **Salesforce Sandbox adminlösenord** textruta typen hello lösenordet för kontot.
 3. I hello **användartoken säkerhet** textruta klistra in hello säkerhet token-värde.
 4. Klicka på **verifiera** tooverify din konfiguration.
 5. Klicka på hello **nästa** knappen tooopen hello **bekräftelse** sidan.
9. På hello **bekräftelse** klickar du på **Slutför** toosave din konfiguration.
   
## <a name="assigning-users"></a>Tilldela användare

tootest konfigurationen, måste toogrant hello Azure AD-användare som du vill med hjälp av ditt program åtkomst tooit genom att tilldela dem tooallow.

**tooassign användare tooSalesforce Sandbox, utför hello följande steg:**

1. Skapa ett testkonto i hello klassiska Azure-portalen.
2. På hello ** Salesforce Sandbox ** integreringssidan för programmet, klickar du på **tilldela användare**.
   
   ![Tilldela användare](./media/active-directory-saas-salesforce-sandbox-tutorial/IC769574.png "tilldela användare")
3. Välj din testanvändare, klicka på **tilldela**, och klicka sedan på **Ja** tooconfirm tilldelningen.
   
   ![Ja](./media/active-directory-saas-salesforce-sandbox-tutorial/IC767830.png "Ja")

Nu bör du vänta i 10 minuter och kontrollera att hello kontot har synkroniserat tooSalesforce Sandbox.

Om du vill tootest SSO-inställningarna, öppna hello åtkomstpanelen. Mer information om hello åtkomstpanelen finns [introduktion toohello åtkomstpanelen](https://msdn.microsoft.com/library/dn308586).

