---
title: "Självstudier: Azure Active Directory-integrering med Cisco Webex | Microsoft Docs"
description: "Lär dig hur toouse Cisco Webex med Azure Active Directory tooenable enkel inloggning, Automatisk etablering och mycket mer!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 26704ca7-13ed-4261-bf24-fd6252e2072b
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/10/2017
ms.author: jeedes
ms.openlocfilehash: 9fc11e58a7acaa6fbfb32eeccbfbf85984950e67
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-cisco-webex"></a>Självstudier: Azure Active Directory-integrering med Cisco Webex
hello syftet med den här kursen är tooshow hello integreringen av Azure och Cisco Webex.  
hello-scenario som beskrivs i den här kursen förutsätter att du redan har hello följande objekt:

* En giltig Azure-prenumeration
* En Cisco-Webex-klient

Den här kursen hello Azure AD-användare som du har tilldelat tooCisco Webex blir kan toosingle logga till hello program på din Cisco Webex företagets plats (service provider initierade inloggning) eller använda hello [introduktion toohello Åtkomst till panelen](active-directory-saas-access-panel-introduction.md).

hello-scenario som beskrivs i den här kursen består av följande byggblock hello:

* Aktivera programmet hello-integrering för Cisco Webex
* Konfigurera enkel inloggning (SSO)
* Konfigurera användaretablering
* Tilldela användare

![Scenariot](./media/active-directory-saas-cisco-webex-tutorial/IC777614.png "Scenario")

## <a name="enable-hello-application-integration-for-cisco-webex"></a>Aktivera hello programmet integration för Cisco Webex
hello syftet med det här avsnittet är toooutline hur tooenable hello programintegrationstyp för Cisco Webex.

**tooenable hello programintegrationstyp för Cisco Webex utför hello följande steg:**

1. I hello klassiska Azure-portalen på hello vänstra navigationsfönstret klickar du på **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-cisco-webex-tutorial/IC700993.png "Active Directory")
2. Från hello **Directory** listan, Välj hello katalog som du vill tooenable katalogintegrering.
3. tooopen hello program i vyn hello directory vyn klickar du på **program** i hello huvudmenyn.
   
   ![Program](./media/active-directory-saas-cisco-webex-tutorial/IC700994.png "program")
4. Klicka på **Lägg till** på hello hello sidans nederkant.
   
   ![Lägg till program](./media/active-directory-saas-cisco-webex-tutorial/IC749321.png "lägga till program")
5. På hello **vad vill du vill toodo** dialogrutan klickar du på **lägga till ett program från galleriet hello**.
   
   ![Lägga till ett program från gallerry](./media/active-directory-saas-cisco-webex-tutorial/IC749322.png "lägga till ett program från gallerry")
6. I hello **sökrutan**, typen **Cisco Webex**.
   
   ![Programgalleriet](./media/active-directory-saas-cisco-webex-tutorial/IC777615.png "Programgalleriet")
7. I resultatfönstret hello väljer **Cisco Webex**, och klicka sedan på **Slutför** tooadd hello program.
   
   ![Cisco Webex](./media/active-directory-saas-cisco-webex-tutorial/IC777616.png "Cisco Webex")
   
## <a name="configure-single-sign-on"></a>Konfigurera enkel inloggning

hello syftet med det här avsnittet är toooutline hur tooenable användare tooauthenticate tooCisco Webex med sitt konto i Azure AD med hjälp av federation baserat på hello SAML-protokoll.  

Som en del av den här proceduren är obligatoriska toocreate en Base64-kodade certifikatet. Om du inte är bekant med den här proceduren, se [hur tooconvert en binär certifikat i en textfil](http://youtu.be/PlgrzUZ-Y1o)

**tooconfigure enkel inloggning, utföra hello följande steg:**

1. I hello klassiska Azure-portalen på hello **Cisco Webex** integreringssidan för programmet, klickar du på **Konfigurera enkel inloggning** tooopen hello **Konfigurera enkel inloggning** dialogrutan.
   
   ![Konfigurera enkel inloggning](./media/active-directory-saas-cisco-webex-tutorial/IC777617.png "Konfigurera enkel inloggning")
2. På hello **hur skulle du som användare toosign på tooCisco Webex** väljer **Microsoft Azure AD enkel inloggning**, och klicka sedan på **nästa**.
   
   ![Konfigurera enkel inloggning](./media/active-directory-saas-cisco-webex-tutorial/IC777618.png "Konfigurera enkel inloggning")
3. På hello **konfigurera App-URL** , utför följande steg hello, och klickar sedan på **nästa**.
   
   ![Konfigurera app-URL](./media/active-directory-saas-cisco-webex-tutorial/IC777619.png "konfigurera app-URL")   
   1. I hello **vaken URL** textruta Skriv din Cisco Webex klient-URL (t.ex.: *http://contoso.webex.com*).
   2. I hello **Cisco Webex Reply URL** textruta typen din **Cisco Webex AssertionConsumerService URL** (t.ex.: *https://company.webex.com/dispatcher/SAML2AuthService?siteurl=company* ).
4. På hello **Konfigurera enkel inloggning på Cisco Webex** sida, toodownload ditt certifikat klickar du på **hämta certifikat**, och sedan spara hello certifikatfilen på datorn.
   
   ![Konfigurera enkel inloggning](./media/active-directory-saas-cisco-webex-tutorial/IC777620.png "Konfigurera enkel inloggning")
5. Logga in på webbplatsen Cisco Webex företag som en administratör i en annan webbläsarfönster.
6. Hello-menyn överst hello **Platsadministration**.
   
   ![Platsadministration](./media/active-directory-saas-cisco-webex-tutorial/IC777621.png "Platsadministration")
7. I hello **Hantera plats** klickar du på **SSO Configuration**.
   
   ![Konfiguration av SSO](./media/active-directory-saas-cisco-webex-tutorial/IC777622.png "SSO-konfiguration")
8. I hello Federerad Web SSO konfigurationsavsnittet, utföra hello följande steg:
   
   ![Federerad enkel inloggning Configuration](./media/active-directory-saas-cisco-webex-tutorial/IC777623.png "federerad enkel inloggning konfiguration")  
   1. Från hello **Federation-protokollet** väljer **SAML 2.0**.
   2. Skapa en **Base64-kodad** fil från din hämtat certifikat.  
    >[!TIP]
    >Mer information finns i [hur tooconvert en binär certifikat i en textfil](http://youtu.be/PlgrzUZ-Y1o)
    >  
   3. Öppna din Base64-kodade certifikatet i anteckningar och kopiera hello innehållet i den.
   4. Klicka på **importera SAML Metadata**, och klistra sedan in din Base64-kodade certifikatet.
   5. I hello klassiska Azure-portalen på hello **Konfigurera enkel inloggning på Cisco Webex** dialogrutan sida, kopiera hello **utfärdar-URL** värdet och klistrar in den i hello **utfärdaren för SAML (IdP-ID)** textruta.
   6. I hello klassiska Azure-portalen på hello **Konfigurera enkel inloggning på Cisco Webex** dialogrutan sida, kopiera hello **Remote inloggnings-URL** värdet och klistrar in den i hello **kunden tjänstinloggning för enkel inloggning URL: en** textruta.
   7. Från hello **NameID Format** väljer **e-postadress**.
   8. I hello **AuthnContextClassRef** textruta typen **urn: oasis: namn: tc: SAML:2.0:ac:classes:Password**.
   9. I hello klassiska Azure-portalen på hello **Konfigurera enkel inloggning på Cisco Webex** dialogrutan sida, kopiera hello **Remote logga ut URL** värdet och klistrar in den i hello **kunden SSO-Service logga ut URL: en** textruta.
   10. Klicka på **uppdatering**.
9. I hello klassiska Azure-portalen på hello **Konfigurera enkel inloggning på Cisco Webex** dialogrutan sidan Välj hello konfiguration för enkel inloggning bekräftelse och klicka sedan på **Slutför**.
   
   ![Konfigurera enkel inloggning](./media/active-directory-saas-cisco-webex-tutorial/IC777624.png "Konfigurera enkel inloggning")
   
## <a name="configure-user-provisioning"></a>Konfigurera användaretablering

I ordning tooenable Azure AD-användare toolog i Cisco Webex, måste de etableras i Cisco Webex.  

* Cisco Webex hello gäller är etablering en manuell aktivitet.

**tooprovision användarkonton, utföra hello följande steg:**

1. Logga in tooyour **Cisco Webex** klient.
2. Gå för**hantera användare \> Lägg till användare**.
   
   ![Lägg till användare](./media/active-directory-saas-cisco-webex-tutorial/IC777625.png "lägga till användare")
3. Utför följande hello på hello Lägg till användare:
   
   ![Lägg till användare](./media/active-directory-saas-cisco-webex-tutorial/IC777626.png "Lägg till användare")   
   1. Som **kontotyp**väljer **värden**.
   2. Skriv hello information för en befintlig Azure AD-användare i hello följande textrutor: **förnamn, efternamn**, **användarnamn**, **e-post**, **lösenord**, **Bekräfta lösenord**.
   3. Klicka på **Lägg till**.

>[!NOTE]
>Du kan använda något annat Cisco Webex användarens konto skapas verktyg eller API: er som tillhandahålls av Cisco Webex tooprovision AAD användarkonton. 
> 

## <a name="assign-users"></a>Tilldela användare
tootest konfigurationen, måste toogrant hello Azure AD-användare som du vill med hjälp av ditt program åtkomst tooit genom att tilldela dem tooallow.

**tooassign användare tooCisco Webex, utför följande steg hello:**

1. Skapa ett testkonto i hello klassiska Azure-portalen.
2. På hello **Cisco Webex** integreringssidan för programmet, klickar du på **tilldela användare**.
   
   ![Tilldela användare](./media/active-directory-saas-cisco-webex-tutorial/IC777627.png "tilldela användare")
3. Välj din testanvändare, klicka på **tilldela**, och klicka sedan på **Ja** tooconfirm tilldelningen.
   
   ![Ja](./media/active-directory-saas-cisco-webex-tutorial/IC767830.png "Ja")

Om du vill tootest dina inställningar för enkel inloggning, öppna hello åtkomstpanelen. Mer information om hello åtkomstpanelen finns [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).

