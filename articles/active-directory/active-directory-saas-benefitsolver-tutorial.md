---
title: "Självstudier: Azure Active Directory-integrering med Benefitsolver | Microsoft Docs"
description: "Lär dig hur toouse Benefitsolver med Azure Active Directory tooenable enkel inloggning, Automatisk etablering och mycket mer!"
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
ms.openlocfilehash: 5bb8511ef9be1e386956188a93e899d6ebe56ed5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-benefitsolver"></a>Självstudier: Azure Active Directory-integrering med Benefitsolver
hello syftet med den här kursen är tooshow hello integreringen av Azure och Benefitsolver.  

hello-scenario som beskrivs i den här kursen förutsätter att du redan har hello följande objekt:

* En giltig Azure-prenumeration
* En Benefitsolver enkel inloggning (SSO) aktiverat prenumeration

Den här kursen hello Azure AD-användare som du har tilldelat tooBenefitsolver blir toosingle kan logga in på hello program med hjälp av hello [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).

hello-scenario som beskrivs i den här kursen består av följande byggblock hello:

1. Aktivera programmet hello-integrering för Benefitsolver
2. Konfigurera enkel inloggning (SSO)
3. Konfigurera användaretablering
4. Tilldela användare

![Scenariot](./media/active-directory-saas-benefitsolver-tutorial/IC804820.png "Scenario")

## <a name="enabling-hello-application-integration-for-benefitsolver"></a>Aktivera programmet hello-integrering för Benefitsolver
hello syftet med det här avsnittet är toooutline hur tooenable hello programintegrationstyp för Benefitsolver.

### <a name="tooenable-hello-application-integration-for-benefitsolver-perform-hello-following-steps"></a>tooenable hello programintegrationstyp för Benefitsolver, utför hello följande steg:
1. I hello klassiska Azure-portalen på hello vänstra navigationsfönstret klickar du på **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-benefitsolver-tutorial/IC700993.png "Active Directory")
2. Från hello **Directory** listan, Välj hello katalog som du vill tooenable katalogintegrering.
3. tooopen hello program i vyn hello directory vyn klickar du på **program** i hello huvudmenyn.
   
   ![Program](./media/active-directory-saas-benefitsolver-tutorial/IC700994.png "program")
4. Klicka på **Lägg till** på hello hello sidans nederkant.
   
   ![Lägg till program](./media/active-directory-saas-benefitsolver-tutorial/IC749321.png "lägga till program")
5. På hello **vad vill du vill toodo** dialogrutan klickar du på **lägga till ett program från galleriet hello**.
   
   ![Lägga till ett program från gallerry](./media/active-directory-saas-benefitsolver-tutorial/IC749322.png "lägga till ett program från gallerry")
6. I hello **sökrutan**, typen **Benefitsolver**.
   
   ![Programgalleriet](./media/active-directory-saas-benefitsolver-tutorial/IC804821.png "Programgalleriet")
7. I resultatfönstret hello väljer **Benefitsolver**, och klicka sedan på **Slutför** tooadd hello program.
   
   ![Benefitssolver](./media/active-directory-saas-benefitsolver-tutorial/IC804822.png "Benefitssolver")
   
## <a name="configure-single-sign-on"></a>Konfigurera enkel inloggning

hello syftet med det här avsnittet är toooutline hur tooenable användare tooauthenticate tooBenefitsolver med sitt konto i Azure AD med hjälp av federation baserat på hello SAML-protokoll.  

Tillämpningsprogrammet Benefitsolver förväntar hello SAML intyg i ett specifikt format, vilket kräver tooadd attributet mappningar tooyour **saml-token attribut** konfiguration. 

hello följande skärmbild visar ett exempel för det här.

![Attribut](./media/active-directory-saas-benefitsolver-tutorial/IC804823.png "attribut")

**tooconfigure enkel inloggning, utföra hello följande steg:**

1. I hello klassiska Azure-portalen på hello **Benefitsolver** integreringssidan för programmet, klickar du på **Konfigurera enkel inloggning** tooopen hello **Konfigurera enkel inloggning** dialogrutan.
   
   ![Konfigurera enkel inloggning](./media/active-directory-saas-benefitsolver-tutorial/IC804824.png "Konfigurera enkel inloggning")
2. På hello **hur skulle du som användare toosign på tooBenefitsolver** väljer **Microsoft Azure AD enkel inloggning**, och klicka sedan på **nästa**.
   
   ![Konfigurera enkel inloggning](./media/active-directory-saas-benefitsolver-tutorial/IC804825.png "Konfigurera enkel inloggning")
3. På hello **konfigurera Appinställningar** utför hello följande steg:
   
   ![Konfigurera Appinställningar för](./media/active-directory-saas-benefitsolver-tutorial/IC804826.png "konfigurera App-inställningar")
   
   1. I hello **logga URL** textruta typen **http://azure.benefitsolver.com**.
   2. I hello **Reply URL** textruta typen **https://www.benefitsolver.com/benefits/BenefitSolverView?page_name=single_signon_saml**.  
   3. Klicka på **Nästa**.
4. På hello **Konfigurera enkel inloggning på Benefitsolver** sida, toodownload metadata, klickar du på **hämtar metadata**, och sedan spara hello metadatafil lokalt på datorn.
   
   ![Konfigurera enkel inloggning](./media/active-directory-saas-benefitsolver-tutorial/IC804827.png "Konfigurera enkel inloggning")
5. Skicka hello hämtas metadata filen tooyour Benefitsolver supportteamet.
   
   >[!NOTE]
   >Supportteamet Benefitsolver har toodo hello faktiska SSO konfiguration. Du får ett meddelande när enkel inloggning har aktiverats för din prenumeration.
   >

6. Välj hello konfiguration för enkel inloggning bekräftelse på hello klassiska Azure-portalen, och klicka sedan på **Slutför** tooclose hello **Konfigurera enkel inloggning** dialogrutan.
   
   ![Konfigurera enkel inloggning](./media/active-directory-saas-benefitsolver-tutorial/IC804828.png "Konfigurera enkel inloggning")
7. Hello-menyn överst hello **attribut** tooopen hello **SAML-Token attribut** dialogrutan.
   
   ![Attribut](./media/active-directory-saas-benefitsolver-tutorial/IC795920.png "attribut")
8. mappningar av tooadd hello krävs, utför hello följande steg:
   
   ![Attribut](./media/active-directory-saas-benefitsolver-tutorial/IC804823.png "attribut")
   
   | Attributets namn | Attributvärdet |
   | --- | --- |
   | ClientID |Du måste tooget det här värdet från supportteamet Benefitsolver. |
   | ClientKey |Du måste tooget det här värdet från supportteamet Benefitsolver. |
   | LogoutURL |Du måste tooget det här värdet från supportteamet Benefitsolver. |
   | EmployeeID |Du måste tooget det här värdet från supportteamet Benefitsolver. |
   
   1. För varje datarad i hello tabellen ovan klickar du på **lägga till användarattribut**.
   2. I hello **attributnamn** textruta hello attributnamn visas för den raden.
   3. I hello **attributvärdet** textruta väljer hello-attributvärde som visas för den raden.
   4. Klicka på **Complete** (Slutför).
9. Klicka på **tillämpa ändringarna**.

## <a name="configure-user-provisioning"></a>Konfigurera användaretablering
I ordning tooenable Azure AD-användare toolog i Benefitsolver, måste de etableras i Benefitsolver.  

Hello gäller Benefitsolver är data om anställda i ditt program fyllts via en inventering från personalinformationssystemet (vanligtvis varje natt).  

>[!NOTE]
>Du kan använda något annat Benefitsolver användarens konto skapas verktyg eller API: er som tillhandahålls av Benefitsolver tooprovision AAD-användarkonton. 
> 

## <a name="assigning-users"></a>Tilldela användare
tootest konfigurationen, måste toogrant hello Azure AD-användare som du vill med hjälp av ditt program åtkomst tooit genom att tilldela dem tooallow.

### <a name="tooassign-users-toobenefitsolver-perform-hello-following-steps"></a>tooassign användare tooBenefitsolver utföra hello följande steg:
1. Skapa ett testkonto i hello klassiska Azure-portalen.
2. På hello ** Benefitsolver ** integreringssidan för programmet, klickar du på **tilldela användare**.
   
   ![Tilldela användare](./media/active-directory-saas-benefitsolver-tutorial/IC804829.png "tilldela användare")
3. Välj din testanvändare, klicka på **tilldela**, och klicka sedan på **Ja** tooconfirm tilldelningen.
   
   ![Ja](./media/active-directory-saas-benefitsolver-tutorial/IC767830.png "Ja")

Om du vill tootest dina inställningar för enkel inloggning, öppna hello åtkomstpanelen. Mer information om hello åtkomstpanelen finns [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).

