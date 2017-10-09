---
title: "Självstudier: Azure Active Directory-integrering med Qualtrics | Microsoft Docs"
description: "Lär dig hur toouse Qualtrics med Azure Active Directory tooenable enkel inloggning, Automatisk etablering och mycket mer!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 4df889ab-2685-4d15-a163-1ba26567eeda
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/23/2017
ms.author: jeedes
ms.openlocfilehash: 941642e74b90bb13a5ce37ce6665cfa32cd86016
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-qualtrics"></a>Självstudier: Azure Active Directory-integrering med Qualtrics
hello syftet med den här kursen är tooshow hello integreringen av Azure och Qualtrics.  

hello-scenario som beskrivs i den här kursen förutsätter att du redan har hello följande objekt:

* En giltig Azure-prenumeration
* En Qualtrics enkel inloggning (SSO) aktiverat prenumeration

Den här kursen hello Azure AD-användare som du har tilldelat tooQualtrics blir toosingle kan logga in på hello-programmet på din Qualtrics företagets webbplats (service provider initierade inloggning) eller med hjälp av hello [introduktion toohello Åtkomst till panelen](active-directory-saas-access-panel-introduction.md).

hello-scenario som beskrivs i den här kursen består av följande byggblock hello:

1. Aktivera programmet hello-integrering för Qualtrics
2. Konfigurera enkel inloggning (SSO)
3. Konfigurera användaretablering
4. Tilldela användare

![Scenariot](./media/active-directory-saas-qualtrics-tutorial/IC789542.png "Scenario")

## <a name="enabling-hello-application-integration-for-qualtrics"></a>Aktivera programmet hello-integrering för Qualtrics
hello syftet med det här avsnittet är toooutline hur tooenable hello programintegrationstyp för Qualtrics.

**tooenable hello programintegrationstyp för Qualtrics, utför hello följande steg:**

1. I hello klassiska Azure-portalen på hello vänstra navigationsfönstret klickar du på **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-qualtrics-tutorial/IC700993.png "Active Directory")
2. Från hello **Directory** listan, Välj hello katalog som du vill tooenable katalogintegrering.
3. tooopen hello program i vyn hello directory vyn klickar du på **program** i hello huvudmenyn.
   
   ![Program](./media/active-directory-saas-qualtrics-tutorial/IC700994.png "program")
4. Klicka på **Lägg till** på hello hello sidans nederkant.
   
   ![Lägg till program](./media/active-directory-saas-qualtrics-tutorial/IC749321.png "lägga till program")
5. På hello **vad vill du vill toodo** dialogrutan klickar du på **lägga till ett program från galleriet hello**.
   
   ![Lägga till ett program från gallerry](./media/active-directory-saas-qualtrics-tutorial/IC749322.png "lägga till ett program från gallerry")
6. I hello **sökrutan**, typen **Qualtrics**.
   
   ![Programgalleriet](./media/active-directory-saas-qualtrics-tutorial/IC789543.png "Programgalleriet")
7. I resultatfönstret hello väljer **Qualtrics**, och klicka sedan på **Slutför** tooadd hello program.
   
   ![Qualtrics](./media/active-directory-saas-qualtrics-tutorial/IC789544.png "Qualtrics")
   
## <a name="configure-single-sign-on"></a>Konfigurera enkel inloggning

hello syftet med det här avsnittet är toooutline hur tooenable användare tooauthenticate tooQualtrics med sitt konto i Azure AD med hjälp av federation baserat på hello SAML-protokoll.

**tooconfigure enkel inloggning, utföra hello följande steg:**

1. I hello klassiska Azure-portalen på hello **Qualtrics** integreringssidan för programmet, klickar du på **Konfigurera enkel inloggning** tooopen hello **Konfigurera enkel inloggning** dialogrutan.
   
   ![Konfigurera enkel inloggning](./media/active-directory-saas-qualtrics-tutorial/IC789545.png "Konfigurera enkel inloggning")
2. På hello **hur skulle du som användare toosign på tooQualtrics** väljer **Microsoft Azure AD enkel inloggning**, och klicka sedan på **nästa**.
   
   ![Konfigurera enkel inloggning](./media/active-directory-saas-qualtrics-tutorial/IC789546.png "Konfigurera enkel inloggning")
3. På hello **konfigurera App-URL** i hello sidan **Qualtrics logga URL** textruta Skriv URL: en (t.ex. ”:*https://ssotest2ut1.qualtrics.com*”), och klicka sedan på **Nästa**.
   
   ![Konfigurera App-URL](./media/active-directory-saas-qualtrics-tutorial/IC789547.png "konfigurera App-URL")
4. På hello **Konfigurera enkel inloggning på Qualtrics** klickar du på **hämtar metadata**, och sedan spara hello metadatafil på datorn.
   
   ![Konfigurera enkel inloggning](./media/active-directory-saas-qualtrics-tutorial/IC789548.png "Konfigurera enkel inloggning")
5. Skicka hello metadata filen toohello Qualtrics supportteamet.
   
   >[!NOTE]
   >hello SSO konfigurationen har toobe som utförs av hello Qualtrics supportteamet. Du får ett meddelande som hello konfigurationen har slutförts.
   > 
   > 
6. Välj hello konfiguration för enkel inloggning bekräftelse på hello klassiska Azure-portalen, och klicka sedan på **Slutför** tooclose hello **Konfigurera enkel inloggning** dialogrutan.
   
   ![Konfigurera enkel inloggning](./media/active-directory-saas-qualtrics-tutorial/IC789549.png "Konfigurera enkel inloggning")
   
## <a name="configure-user-provisioning"></a>Konfigurera användaretablering

Det finns inga uppgiften för du tooconfigure användaretablering tooQualtrics. När en tilldelad användare försöker toolog till Qualtrics med hello åtkomstpanelen, kontrollerar Qualtrics om hello användaren finns.  

Om det finns inget användarkonto ännu, skapas den automatiskt av Qualtrics.

## <a name="assign-users"></a>Tilldela användare
tootest konfigurationen, måste toogrant hello Azure AD-användare som du vill med hjälp av ditt program åtkomst tooit genom att tilldela dem tooallow.

**tooassign användare tooQualtrics utför hello följande steg:**

1. Skapa ett testkonto i hello klassiska Azure-portalen.
2. På hello **Qualtrics** integreringssidan för programmet, klickar du på **tilldela användare**.
   
   ![Tilldela användare](./media/active-directory-saas-qualtrics-tutorial/IC789550.png "tilldela användare")
3. Välj din testanvändare, klicka på **tilldela**, och klicka sedan på **Ja** tooconfirm tilldelningen.
   
   ![Ja](./media/active-directory-saas-qualtrics-tutorial/IC767830.png "Ja")

Om du vill tootest SSO-inställningarna, öppna hello åtkomstpanelen. Mer information om hello åtkomstpanelen finns [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).

