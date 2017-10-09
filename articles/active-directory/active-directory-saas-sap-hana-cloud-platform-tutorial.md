---
title: "Självstudier: Azure Active Directory-integrering med SAP HANA Molnplattform | Microsoft Docs"
description: "Lär dig hur toouse SAP HANA Molnplattform med Azure Active Directory tooenable enkel inloggning, Automatisk etablering och mycket mer!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: bd398225-8bd8-4697-9a44-af6e6679113a
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/23/2017
ms.author: jeedes
ms.openlocfilehash: cc6610969b1c7b08f776e6969a5977fc75eb9ab4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sap-hana-cloud-platform"></a>Självstudier: Azure Active Directory-integration med SAP HANA-molnplattformen
hello syftet med den här kursen är tooshow hello integrering av Azure- och Molnplattform för SAP HANA.

hello-scenario som beskrivs i den här kursen förutsätter att du redan har hello följande objekt:

* En giltig Azure-prenumeration
* En Molnplattform för SAP HANA-konto

Den här kursen hello Azure AD-användare som du har tilldelat tooSAP HANA Molnplattform kommer att kunna toosingle logga till hello program med hjälp av hello [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).

>[!IMPORTANT]
>Du måste toodeploy ditt eget program eller prenumerera tooan program på din SAP HANA-Molnplattform konto tootest enkel inloggning. Ett program ska distribueras i hello-konto i den här självstudiekursen.
> 
> 

hello-scenario som beskrivs i den här kursen består av följande byggblock hello:

1. Aktivera hello programintegrationstyp för SAP HANA Molnplattform
2. Konfigurera enkel inloggning (SSO)
3. Tilldela en roll tooa användare
4. Tilldela användare

![Scenariot](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790795.png "Scenario")

## <a name="enabling-hello-application-integration-for-sap-hana-cloud-platform"></a>Aktivera hello programintegrationstyp för SAP HANA Molnplattform
hello syftet med det här avsnittet är toooutline hur tooenable hello programintegrationstyp för SAP HANA Molnplattform.

**tooenable hello programintegrationstyp för SAP HANA Molnplattform, utför hello följande steg:**

1. I hello Azure Management Portal på hello vänstra navigationsfönstret klickar du på **Active Directory**.
   
    ![Active Directory](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC700993.png "Active Directory")
2. Från hello **Directory** listan, Välj hello katalog som du vill tooenable katalogintegrering.
3. tooopen hello program i vyn hello directory vyn klickar du på **program** i hello huvudmenyn.
   
    ![Program](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC700994.png "program")
4. Klicka på **Lägg till** på hello hello sidans nederkant.
   
    ![Lägg till program](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC749321.png "lägga till program")
5. På hello **vad vill du vill toodo** dialogrutan klickar du på **lägga till ett program från galleriet hello**.
   
    ![Lägga till ett program från gallerry](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC749322.png "lägga till ett program från gallerry")
6. I hello **sökrutan**, typen **Molnplattform för SAP HANA**.
   
    ![Programgalleriet](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790796.png "Programgalleriet")
7. I resultatfönstret hello väljer **Molnplattform för SAP HANA**, och klicka sedan på **Slutför** tooadd hello program.
   
    ![SAP Hana](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793929.png "SAP Hana")
   
## <a name="configure-single-sign-on"></a>Konfigurera enkel inloggning

hello syftet med det här avsnittet är toooutline hur tooenable användare tooauthenticate tooSAP HANA Molnplattform med ett konto i Azure AD med hjälp av federation baserat på hello SAML-protokoll.

Som en del av den här proceduren är obligatoriska tooupload en Base64-kodade certifikatet tooyour Molnplattform för SAP HANA-klient.  

Om du inte är bekant med den här proceduren, se [hur tooconvert en binär certifikat i en textfil](http://youtu.be/PlgrzUZ-Y1o)

**tooconfigure enkel inloggning, utföra hello följande steg:**

1. I hello klassiska Azure-portalen på hello **Molnplattform för SAP HANA** integreringssidan för programmet, klickar du på **Konfigurera enkel inloggning** tooopen hello **Konfigurera enkel inloggning**dialogrutan.
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC778552.png "Konfigurera enkel inloggning")
2. På hello **hur skulle du som användare toosign på tooSAP HANA Molnplattform** väljer **Microsoft Azure AD enkel inloggning**, och klicka sedan på **nästa**.
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790797.png "Konfigurera enkel inloggning")
3. Inloggning i en annan webbläsarfönster toohello SAP HANA Cloud Platform Cockpit på https://account. \<liggande värden\>.ondemand.com/cockpit (t.ex.: *https://account.hanatrial.ondemand.com/cockpit*).
4. Klicka på hello **förtroende** fliken.
   
    ![Förtroende](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790800.png "förtroende")
5. Utför följande hello under förtroende management:
   
    ![Hämta Metadata](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793930.png "hämta Metadata")
   
   1. Klicka på hello **lokal-leverantör** fliken.
   2. toodownload hello Molnplattform för SAP HANA metadatafil klickar du på **hämta Metadata**.
6. I hello Azure Active klassiska portalen på hello **konfigurera App-URL** , utför följande steg hello, och klickar sedan på **nästa**.
   
    ![Konfigurera App-URL](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790798.png "konfigurera App-URL")
   
   1. I hello **logga URL** textruta typen hello URL som används av din toosign för användare i din **Molnplattform för SAP HANA** program. Det här är hello kontospecifikt URL för en skyddad resurs i ditt Molnplattform för SAP HANA-program. hello URL är baserad på hello följer mönstret: *https://\<applicationName\>\<accountName\>.\< liggande värden\>.ondemand.com/\<sökväg\_till\_skyddade\_resurs\>*  (t.ex.: *https://xleavep1941203872trial.hanatrial.ondemand.com/xleave*)
      
     >[!NOTE]
     >Detta är hello URL: en i din Molnplattform för SAP HANA-program som kräver hello användaren tooauthenticate.
     > 

   2. Öppna hello hämtas Molnplattform för SAP HANA-metadatafil och välj sedan hello **ns3:AssertionConsumerService** tagg.
   3. Kopiera hello värdet för hello **plats** attributet och klistra in den i hello **Reply-URL för SAP HANA Cloud Platform** textruta.

7. På hello **Konfigurera enkel inloggning på SAP HANA Molnplattform** sida, toodownload metadata, klickar du på **hämtar metadata**, och spara sedan hello på datorn.
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790799.png "Konfigurera enkel inloggning")
8. På hello SAP HANA Cloud Platform Cockpit i hello **lokal-leverantör** avsnittet, utföra hello följande steg:
   
    ![Förtroende Management](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793931.png "förtroende för hantering")
   
  1. Klicka på **Redigera**.
  2. Som **konfigurationstypen**väljer **anpassad**.
  3. Som **lokala providernamn**, lämna hello standardvärdet.
  4. toogenerate en **signeringsnyckeln** och en **signeringscertifikat** nyckelpar, klickar du på **Generera nyckel**.
  5. Som **huvudnamn spridningen**väljer **inaktiverade**.
  6. Som **kraft autentisering**väljer **inaktiverade**.
  7. Klicka på **Spara**.

9. Klicka på hello **betrodda identitetsleverantör** fliken och klicka sedan på **lägga till betrodda identitetsleverantör**.
   
    ![Förtroende Management](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790802.png "förtroende för hantering")
   
    >[!NOTE]
    >toomanage hello lista över betrodda identitetsleverantörer, behöver du toohave valt hello typen för anpassad konfiguration i hello lokal-leverantör avsnitt. Konfiguration av standardtyp har du en redigeras och implicit förtroende toohello SAP-ID-tjänst. Ingen har du inte för förtroendeinställningar.
    > 
    > 

10. Klicka på hello **allmänna** fliken och klicka sedan på **Bläddra** tooupload hello hämtas metadatafil.
    
    ![Förtroende Management](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793932.png "förtroende för hantering")
    
    >[!NOTE]
    >Efter överföring hello metadatafil hello värden för **URL för enkel inloggning**, **URL för enkel logga ut** och **signeringscertifikat** fylls i automatiskt.
    > 
    > 

11. Klicka på hello **attribut** fliken.
12. På hello **attribut** fliken, utföra hello följande steg:
    
    ![Attribut](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790804.png "attribut") 
  * Klicka på **Add Assertion-Based attributet**, och Lägg sedan till hello följande assertion-baserade attribut:
       
    | Attributet för kontrollen | Huvudnamn attribut |
    | --- | --- |
    | http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/givenName |Förnamn |
    | http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/surname |Efternamn |
    | http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/emailaddress |E-post 
   
     >[!NOTE]
     >hello konfiguration av hello attribut beror på hur hello program på HCP utvecklas, d.v.s. vilka attribut som de förväntar sig i hello SAML-svar och under vilka namn (Principal attribut) de har åtkomst till det här attributet i hello kod.
     > 
     >  

    1.  Hej **standard attributet** i hello skärmbild är precis som illustration. Det är inte krävs toomake hello scenariot arbete.   
    2.  Hej namn och värden för **Principal attributet** visas i hello skärmbild beror på hur hello program utvecklas. Det är möjligt att ditt program kräver olika mappningar.
     
13. I hello klassiska Azure-portalen på hello **Konfigurera enkel inloggning på SAP HANA Molnplattform** dialog sidan Välj hello konfiguration för enkel inloggning bekräftelse och klicka sedan på **Slutför**.
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC796933.png "Konfigurera enkel inloggning")

###<a name="assertion-based-groups"></a>Assertion-baserade grupper
Som ett valfritt steg kan du konfigurera assertion-baserade grupper för identitetsprovider Azure Active Directory.

Använda grupper för SAP HANA-Molnplattform kan du toodynamically tilldela en eller flera användare tooone eller flera roller i dina Molnplattform för SAP HANA-program som bestäms av värdena på attributen i hello SAML 2.0-kontroll. 

Innehåller till exempel om hello assertion hello attributet ”*kontraktet = temporära*”, kan du vill att alla berörda toobe tillagda toohello användargruppen ”*tillfälliga*”. Hej grupp ”*tillfälliga*” kan innehålla en eller flera roller från en eller flera program som distribueras i Molnplattform för SAP HANA-konto.
 
Använda assertion-baserade grupper när du vill tilldela toosimultaneously många användare tooone eller flera roller av program i Molnplattform för SAP HANA-konto. Om du vill att tooassign bara ett enda eller litet antal användare toospecific roller, rekommenderar vi att tilldela dem direkt i hello ”**tillstånd**” Hej Molnplattform för SAP HANA cockpit fliken.

## <a name="assign-a-role-tooa-user"></a>Tilldela en roll tooa användare
I ordning tooenable Azure AD-användare toolog till SAP HANA Molnplattform, måste du tilldela roller i hello SAP HANA Molnplattform toothem.

**tooassign en roll tooa användare utför hello följande steg:**

1. Logga in tooyour **Molnplattform för SAP HANA** cockpit.
2. Utför hello följande:
   
   ![Tillstånd](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790805.png "tillstånd")
   
  1. Klicka på **auktorisering**.
  2. Klicka på hello **användare** fliken.
  3. I hello **användaren** textruta typen hello användarens e-postadress.
  4. Klicka på **tilldela** tooassign hello tooa användarroll.
  5. Klicka på **Spara**.

## <a name="assign-users"></a>Tilldela användare
tootest konfigurationen, måste toogrant hello Azure AD-användare som du vill med hjälp av ditt program åtkomst tooit genom att tilldela dem tooallow.

**tooassign användare tooSAP HANA Molnplattform utför hello följande steg:**

1. Skapa ett testkonto i hello klassiska Azure-portalen.
2. På hello **Molnplattform för SAP HANA** integreringssidan för programmet, klickar du på **tilldela användare**.
   
   ![Tilldela användare](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790806.png "tilldela användare")
3. Välj din testanvändare, klicka på **tilldela**, och klicka sedan på **Ja** tooconfirm tilldelningen.
   
   ![Ja](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC767830.png "Ja")

Om du vill tootest SSO-inställningarna, öppna hello åtkomstpanelen. Mer information om hello åtkomstpanelen finns [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).

