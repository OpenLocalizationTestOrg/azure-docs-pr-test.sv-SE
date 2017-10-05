---
title: "Självstudier: Azure Active Directory-integrering med Benefitsolver | Microsoft Docs"
description: "Lär dig hur du använder Benefitsolver med Azure Active Directory för att aktivera enkel inloggning, Automatisk etablering och mycket mer!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: cf4529b1-3fb6-4475-82b7-2ceedcb70b3c
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/17/2017
ms.author: jeedes
ms.openlocfilehash: 8a13dd5ebd872f86247158379b28bc291a9c9d83
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-benefitsolver"></a>Självstudier: Azure Active Directory-integrering med Benefitsolver
Syftet med den här kursen är att visa integreringen av Azure och Benefitsolver.  

Det scenario som beskrivs i den här kursen förutsätter att du redan har följande objekt:

* En giltig Azure-prenumeration
* En Benefitsolver enkel inloggning (SSO) aktiverat prenumeration

Den här kursen Azure AD-användare som du har tilldelat Benefitsolver kommer att kunna enkel inloggning till programmet med hjälp av den [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).

Det scenario som beskrivs i den här kursen består av följande byggblock:

1. Aktivera programintegrationstyp för Benefitsolver
2. Konfigurera enkel inloggning (SSO)
3. Konfigurera användaretablering
4. Tilldela användare

![Scenariot](./media/active-directory-saas-benefitsolver-tutorial/IC804820.png "Scenario")

## <a name="enabling-the-application-integration-for-benefitsolver"></a>Aktivera programintegrationstyp för Benefitsolver
Syftet med det här avsnittet är att beskriva hur du aktiverar programintegrationstyp för Benefitsolver.

### <a name="to-enable-the-application-integration-for-benefitsolver-perform-the-following-steps"></a>Utför följande steg om du vill aktivera programmet-integrering för Benefitsolver:
1. I den klassiska Azure-portalen i det vänstra navigeringsfönstret klickar du på **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-benefitsolver-tutorial/IC700993.png "Active Directory")
2. Från den **Directory** listan, Välj den katalog som du vill aktivera katalogintegrering.
3. Klicka för att öppna vyn program i vyn directory **program** på huvudmenyn.
   
   ![Program](./media/active-directory-saas-benefitsolver-tutorial/IC700994.png "program")
4. Klicka på **Lägg till** längst ned på sidan.
   
   ![Lägg till program](./media/active-directory-saas-benefitsolver-tutorial/IC749321.png "lägga till program")
5. På den **vad vill du göra** dialogrutan klickar du på **lägga till ett program från galleriet**.
   
   ![Lägga till ett program från gallerry](./media/active-directory-saas-benefitsolver-tutorial/IC749322.png "lägga till ett program från gallerry")
6. I den **sökrutan**, typen **Benefitsolver**.
   
   ![Programgalleriet](./media/active-directory-saas-benefitsolver-tutorial/IC804821.png "Programgalleriet")
7. I resultatfönstret, Välj **Benefitsolver**, och klicka sedan på **Slutför** lägga till programmet.
   
   ![Benefitssolver](./media/active-directory-saas-benefitsolver-tutorial/IC804822.png "Benefitssolver")
   
## <a name="configure-single-sign-on"></a>Konfigurera enkel inloggning

Syftet med det här avsnittet är att beskriva hur användarna att autentisera till Benefitsolver med sitt konto i Azure AD med hjälp av federation baserat på SAML-protokoll.  

Tillämpningsprogrammet Benefitsolver förväntar SAML-intyg i ett specifikt format, vilket kräver att du kan lägga till attributmappningar till din **saml-token attribut** konfiguration. 

Följande skärmbild visar ett exempel för det här.

![Attribut](./media/active-directory-saas-benefitsolver-tutorial/IC804823.png "attribut")

**Utför följande steg för att konfigurera enkel inloggning:**

1. I den klassiska Azure-portalen på den **Benefitsolver** integreringssidan för programmet, klickar du på **Konfigurera enkel inloggning** att öppna den **Konfigurera enkel inloggning** dialogrutan.
   
   ![Konfigurera enkel inloggning](./media/active-directory-saas-benefitsolver-tutorial/IC804824.png "Konfigurera enkel inloggning")
2. På den **hur vill du att användarna kan logga in på Benefitsolver** väljer **Microsoft Azure AD enkel inloggning**, och klicka sedan på **nästa**.
   
   ![Konfigurera enkel inloggning](./media/active-directory-saas-benefitsolver-tutorial/IC804825.png "Konfigurera enkel inloggning")
3. På den **konfigurera Appinställningar** utför följande steg:
   
   ![Konfigurera Appinställningar för](./media/active-directory-saas-benefitsolver-tutorial/IC804826.png "konfigurera App-inställningar")
   
   1. I den **logga URL** textruta typen **http://azure.benefitsolver.com**.
   2. I den **Reply URL** textruta typen **https://www.benefitsolver.com/benefits/BenefitSolverView?page_name=single_signon_saml**.  
   3. Klicka på **Nästa**.
4. På den **Konfigurera enkel inloggning på Benefitsolver** att hämta metadata, klickar du på **hämtar metadata**, och sedan spara metadatafilen lokalt på datorn.
   
   ![Konfigurera enkel inloggning](./media/active-directory-saas-benefitsolver-tutorial/IC804827.png "Konfigurera enkel inloggning")
5. Skicka hämtade metadatafilen till supportteamet Benefitsolver.
   
   >[!NOTE]
   >Supportteamet Benefitsolver har att göra den faktiska SSO-konfigurationen. Du får ett meddelande när enkel inloggning har aktiverats för din prenumeration.
   >

6. Välj bekräftelsen konfiguration för enkel inloggning på den klassiska Azure-portalen och klicka sedan på **Slutför** att stänga den **Konfigurera enkel inloggning** dialogrutan.
   
   ![Konfigurera enkel inloggning](./media/active-directory-saas-benefitsolver-tutorial/IC804828.png "Konfigurera enkel inloggning")
7. Klicka på menyn högst upp **attribut** att öppna den **SAML-Token attribut** dialogrutan.
   
   ![Attribut](./media/active-directory-saas-benefitsolver-tutorial/IC795920.png "attribut")
8. Utför följande steg för att lägga till nödvändiga attributmappning:
   
   ![Attribut](./media/active-directory-saas-benefitsolver-tutorial/IC804823.png "attribut")
   
   | Attributets namn | Attributvärdet |
   | --- | --- |
   | ClientID |Du måste hämta det här värdet från supportteamet Benefitsolver. |
   | ClientKey |Du måste hämta det här värdet från supportteamet Benefitsolver. |
   | LogoutURL |Du måste hämta det här värdet från supportteamet Benefitsolver. |
   | EmployeeID |Du måste hämta det här värdet från supportteamet Benefitsolver. |
   
   1. För varje datarad i tabellen ovan, klickar du på **lägga till användarattribut**.
   2. I den **attributnamn** textruta ange attributets namn visas för den raden.
   3. I den **attributvärdet** textruta Välj attributvärde som visas för den raden.
   4. Klicka på **Complete** (Slutför).
9. Klicka på **tillämpa ändringarna**.

## <a name="configure-user-provisioning"></a>Konfigurera användaretablering
För att aktivera Azure AD-användare att logga in på Benefitsolver etableras de i Benefitsolver.  

När det gäller Benefitsolver är data om anställda i ditt program fyllts via en inventering från personalinformationssystemet (vanligtvis varje natt).  

>[!NOTE]
>Du kan använda något annat Benefitsolver användarens konto skapas verktyg eller API: er som tillhandahålls av Benefitsolver att etablera AAD-användarkonton. 
> 

## <a name="assigning-users"></a>Tilldela användare
Om du vill testa konfigurationen måste du bevilja Azure AD-användare som du vill tillåta med hjälp av ditt programåtkomst till den genom att tilldela dem.

### <a name="to-assign-users-to-benefitsolver-perform-the-following-steps"></a>Utför följande steg om du vill tilldela Benefitsolver användare:
1. Skapa ett testkonto i den klassiska Azure-portalen.
2. På den ** Benefitsolver ** integreringssidan för programmet, klickar du på **tilldela användare**.
   
   ![Tilldela användare](./media/active-directory-saas-benefitsolver-tutorial/IC804829.png "tilldela användare")
3. Välj din testanvändare, klicka på **tilldela**, och klicka sedan på **Ja** att bekräfta din tilldelning.
   
   ![Ja](./media/active-directory-saas-benefitsolver-tutorial/IC767830.png "Ja")

Om du vill testa dina inställningar för enkel inloggning, öppna åtkomstpanelen. Mer information om åtkomstpanelen finns [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).

