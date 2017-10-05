---
title: "Självstudier: Azure Active Directory-integrering med SAP HANA Molnplattform | Microsoft Docs"
description: "Lär dig hur du använder SAP HANA Molnplattform med Azure Active Directory för att aktivera enkel inloggning, Automatisk etablering och mycket mer!"
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
ms.openlocfilehash: e03bc2410a8d57363c558f723b3bfd0e69b3f4c0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sap-hana-cloud-platform"></a>Självstudier: Azure Active Directory-integration med SAP HANA-molnplattformen
Syftet med den här kursen är att visa integreringen av Azure och Molnplattform för SAP HANA.

Det scenario som beskrivs i den här kursen förutsätter att du redan har följande objekt:

* En giltig Azure-prenumeration
* En Molnplattform för SAP HANA-konto

Den här kursen Azure AD-användare som du har tilldelat till SAP HANA Molnplattform kommer att kunna enkel inloggning till programmet med hjälp av den [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).

>[!IMPORTANT]
>Du måste distribuera ditt eget program eller prenumerera på ett program på Molnplattform för SAP HANA-konto för att testa enkel inloggning på. I den här självstudiekursen distribueras ett program i kontot.
> 
> 

Det scenario som beskrivs i den här kursen består av följande byggblock:

1. Aktivera application-integrering för SAP HANA Molnplattform
2. Konfigurera enkel inloggning (SSO)
3. Tilldela en roll till en användare
4. Tilldela användare

![Scenariot](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790795.png "Scenario")

## <a name="enabling-the-application-integration-for-sap-hana-cloud-platform"></a>Aktivera application-integrering för SAP HANA Molnplattform
Syftet med det här avsnittet är att beskriva hur du aktiverar programintegrationstyp för SAP HANA Molnplattform.

**Utför följande steg om du vill aktivera programmet-integrering för SAP HANA Molnplattform:**

1. I Azure-hanteringsportalen på det vänstra navigeringsfönstret klickar du på **Active Directory**.
   
    ![Active Directory](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC700993.png "Active Directory")
2. Från den **Directory** listan, Välj den katalog som du vill aktivera katalogintegrering.
3. Klicka för att öppna vyn program i vyn directory **program** på huvudmenyn.
   
    ![Program](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC700994.png "program")
4. Klicka på **Lägg till** längst ned på sidan.
   
    ![Lägg till program](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC749321.png "lägga till program")
5. På den **vad vill du göra** dialogrutan klickar du på **lägga till ett program från galleriet**.
   
    ![Lägga till ett program från gallerry](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC749322.png "lägga till ett program från gallerry")
6. I den **sökrutan**, typen **Molnplattform för SAP HANA**.
   
    ![Programgalleriet](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790796.png "Programgalleriet")
7. I resultatfönstret, Välj **Molnplattform för SAP HANA**, och klicka sedan på **Slutför** lägga till programmet.
   
    ![SAP Hana](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793929.png "SAP Hana")
   
## <a name="configure-single-sign-on"></a>Konfigurera enkel inloggning

Syftet med det här avsnittet är att beskriva hur användarna att autentisera till SAP HANA Molnplattform med ett konto i Azure AD med hjälp av federation baserat på SAML-protokoll.

Som en del av den här proceduren måste krävs du för att överföra en Base64-kodade certifikat till din Molnplattform för SAP HANA-klient.  

Om du inte är bekant med den här proceduren, se [hur du konverterar ett binärt certifikat till en textfil](http://youtu.be/PlgrzUZ-Y1o)

**Utför följande steg för att konfigurera enkel inloggning:**

1. I den klassiska Azure-portalen på den **Molnplattform för SAP HANA** integreringssidan för programmet, klickar du på **Konfigurera enkel inloggning** att öppna den **Konfigurera enkel inloggning** dialogrutan.
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC778552.png "Konfigurera enkel inloggning")
2. På den **hur vill du att användarna kan logga in på Molnplattform för SAP HANA** väljer **Microsoft Azure AD enkel inloggning**, och klicka sedan på **nästa**.
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790797.png "Konfigurera enkel inloggning")
3. Logga in till SAP HANA Cloud Platform Cockpit på https://account i ett annat webbläsarfönster. \<liggande värden\>.ondemand.com/cockpit (t.ex.: *https://account.hanatrial.ondemand.com/cockpit*).
4. Klicka på den **förtroende** fliken.
   
    ![Förtroende](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790800.png "förtroende")
5. Utför följande steg i förtroende i avsnittet:
   
    ![Hämta Metadata](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793930.png "hämta Metadata")
   
   1. Klicka på den **lokal-leverantör** fliken.
   2. Hämta Molnplattform för SAP HANA-metadatafil Klicka **hämta Metadata**.
6. I Azure Active-klassiska portalen på den **konfigurera App-URL** , utför följande steg, och klickar sedan på **nästa**.
   
    ![Konfigurera App-URL](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790798.png "konfigurera App-URL")
   
   1. I den **logga URL** textruta, Skriv URL-Adressen används av användarna att logga in på ditt **Molnplattform för SAP HANA** program. Detta är en skyddad resurs i ditt program för SAP HANA Molnplattform kontospecifikt URL. URL: en är baserad på följande mönster: *https://\<applicationName\>\<accountName\>.\< liggande värden\>.ondemand.com/\<sökväg\_till\_skyddade\_resurs\>*  (t.ex.: *https://xleavep1941203872trial.hanatrial.ondemand.com/xleave*)
      
     >[!NOTE]
     >Det här är URL-Adressen i din Molnplattform för SAP HANA-program som kräver att användaren autentiseras.
     > 

   2. Öppna den hämta filen för SAP HANA Molnplattform metadata och välj sedan den **ns3:AssertionConsumerService** tagg.
   3. Kopiera värdet för den **plats** attributet och klistrar in det i den **Reply-URL för SAP HANA Cloud Platform** textruta.

7. På den **Konfigurera enkel inloggning på SAP HANA Molnplattform** att hämta metadata, klickar du på **hämtar metadata**, och sedan spara filen på datorn.
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790799.png "Konfigurera enkel inloggning")
8. På SAP HANA Cloud Platform Cockpit i den **lokal-leverantör** avsnittet, utför följande steg:
   
    ![Förtroende Management](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793931.png "förtroende för hantering")
   
  1. Klicka på **Redigera**.
  2. Som **konfigurationstypen**väljer **anpassad**.
  3. Som **lokala providernamn**, låt standardvärdet.
  4. Generera en **signeringsnyckeln** och en **signeringscertifikat** nyckelpar, klickar du på **Generera nyckel**.
  5. Som **huvudnamn spridningen**väljer **inaktiverade**.
  6. Som **kraft autentisering**väljer **inaktiverade**.
  7. Klicka på **Spara**.

9. Klicka på den **betrodda identitetsleverantör** fliken och klicka sedan på **lägga till betrodda identitetsleverantör**.
   
    ![Förtroende Management](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790802.png "förtroende för hantering")
   
    >[!NOTE]
    >Om du vill hantera listan över betrodda identitetsleverantörer, måste du har valt typen för anpassad konfiguration i avsnittet lokal-leverantör. Standard konfigurationstypen har du en redigeras och implicit förtroende till SAP-ID-tjänst. Ingen har du inte för förtroendeinställningar.
    > 
    > 

10. Klicka på den **allmänna** fliken och klicka sedan på **Bläddra** att överföra metadatafilen hämtade.
    
    ![Förtroende Management](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793932.png "förtroende för hantering")
    
    >[!NOTE]
    >Efter överföring metadatafil värdena för **URL för enkel inloggning**, **URL för enkel logga ut** och **signeringscertifikat** fylls i automatiskt.
    > 
    > 

11. Klicka på fliken **Attribut**.
12. På den **attribut** fliken, utför följande steg:
    
    ![Attribut](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790804.png "attribut") 
  * Klicka på **Add Assertion-Based attributet**, och Lägg sedan till följande assertion-baserade attribut:
       
    | Attributet för kontrollen | Huvudnamn attribut |
    | --- | --- |
    | http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/givenName |Förnamn |
    | http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/surname |Efternamn |
    | http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/emailaddress |E-post 
   
     >[!NOTE]
     >Konfigurationen av attributen beror på hur program på HCP utvecklas, d.v.s. vilka attribut som de förväntas i SAML-svaret och under vilka namn (Principal attribut) de har åtkomst till det här attributet i koden.
     > 
     >  

    1.  Den **standard attributet** i skärmbilden är precis som illustration. Det krävs inte för att göra scenariot som fungerar.   
    2.  Namn och värden för **Principal attributet** visas i skärmbilden beror på hur programmet utvecklas. Det är möjligt att ditt program kräver olika mappningar.
     
13. I den klassiska Azure-portalen på den **Konfigurera enkel inloggning på SAP HANA Molnplattform** dialog sidan Välj konfiguration för enkel inloggning bekräftelsen och klicka sedan på **Slutför**.
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC796933.png "Konfigurera enkel inloggning")

###<a name="assertion-based-groups"></a>Assertion-baserade grupper
Som ett valfritt steg kan du konfigurera assertion-baserade grupper för identitetsprovider Azure Active Directory.

Om du använder grupper på Molnplattform för SAP HANA kan du dynamiskt tilldela en eller flera användare till en eller flera roller i din Molnplattform för SAP HANA-program, fastställs av värdena på attributen i SAML 2.0-kontroll. 

Om kontrollen innehåller attributet exempelvis ”*kontraktet = temporära*”, kan du vill att alla berörda användare som ska läggas till i gruppen ”*tillfälliga*”. Gruppen ”*tillfälliga*” kan innehålla en eller flera roller från en eller flera program som distribueras i Molnplattform för SAP HANA-konto.
 
Använda assertion-baserade grupper när du vill tilldela många användare samtidigt till en eller flera roller för program i Molnplattform för SAP HANA-konto. Om du vill tilldela specifika roller bara ett enda eller litet antal användare, rekommenderar vi att tilldela dem direkt i den ”**tillstånd**” för SAP HANA Molnplattform cockpit.

## <a name="assign-a-role-to-a-user"></a>Tilldela en roll till en användare
För att aktivera Azure AD-användare att logga in på Molnplattform för SAP HANA, måste du tilldela roller i Molnplattform för SAP HANA dem.

**Utför följande steg om du vill tilldela en roll till en användare:**

1. Logga in på ditt **Molnplattform för SAP HANA** cockpit.
2. Utför följande:
   
   ![Tillstånd](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790805.png "tillstånd")
   
  1. Klicka på **auktorisering**.
  2. Klicka på den **användare** fliken.
  3. I den **användaren** textruta Skriv användarens e-postadress.
  4. Klicka på **tilldela** tilldela användaren till en roll.
  5. Klicka på **Spara**.

## <a name="assign-users"></a>Tilldela användare
Om du vill testa konfigurationen måste du bevilja Azure AD-användare som du vill tillåta med hjälp av ditt programåtkomst till den genom att tilldela dem.

**Utför följande steg för att tilldela användare till SAP HANA Molnplattform:**

1. Skapa ett testkonto i den klassiska Azure-portalen.
2. På den **Molnplattform för SAP HANA** integreringssidan för programmet, klickar du på **tilldela användare**.
   
   ![Tilldela användare](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790806.png "tilldela användare")
3. Välj din testanvändare, klicka på **tilldela**, och klicka sedan på **Ja** att bekräfta din tilldelning.
   
   ![Ja](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC767830.png "Ja")

Om du vill testa inställningarna för enkel inloggning, öppna åtkomstpanelen. Mer information om åtkomstpanelen finns [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).

