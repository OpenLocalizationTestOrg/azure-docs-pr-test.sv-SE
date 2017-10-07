---
title: "Självstudier: Azure Active Directory-integrering med SCC livscykel | Microsoft Docs"
description: "Lär dig hur toouse SCC livscykel med Azure Active Directory tooenable enkel inloggning, Automatisk etablering och mycket mer!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 9748bf38-ffc3-4d51-a1ae-207ce57104fa
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/23/2017
ms.author: jeedes
ms.openlocfilehash: c10c313c5fc157ed70d2ccecfb930a8a765f8444
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-scc-lifecycle"></a>Självstudier: Azure Active Directory-integrering med SCC livscykel
hello syftet med den här kursen är tooshow hello integreringen av Azure och SCC livscykel.  

hello-scenario som beskrivs i den här kursen förutsätter att du redan har hello följande objekt:

* En giltig Azure-prenumeration
* En SCC livscykel enkel inloggning (SSO) aktiverat prenumeration

Den här kursen hello Azure AD-användare som du har tilldelat tooSCC livscykel blir kan toosingle logga till hello program på din SCC livscykel företagets plats (service provider initierade inloggning) eller använda hello [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).

hello-scenario som beskrivs i den här kursen består av följande byggblock hello:

1. Aktivera programmet hello-integrering för SCC livscykel
2. Konfigurera enkel inloggning (SSO)
3. Konfigurera användaretablering
4. Tilldela användare

![Scenariot](./media/active-directory-saas-scc-lifecycle-tutorial/IC794120.png "Scenario")

## <a name="enable-hello-application-integration-for-scc-lifecycle"></a>Aktivera hello programmet integration för SCC livscykel
hello syftet med det här avsnittet är toooutline hur tooenable hello programintegrationstyp för SCC livscykel.

**tooenable hello programintegrationstyp för SCC livscykel, utför hello följande steg:**

1. I hello klassiska Azure-portalen på hello vänstra navigationsfönstret klickar du på **Active Directory**.
   
    ![Active Directory](./media/active-directory-saas-scc-lifecycle-tutorial/IC700993.png "Active Directory")
2. Från hello **Directory** listan, Välj hello katalog som du vill tooenable katalogintegrering.
3. tooopen hello program i vyn hello directory vyn klickar du på **program** i hello huvudmenyn.
   
    ![Program](./media/active-directory-saas-scc-lifecycle-tutorial/IC700994.png "program")
4. Klicka på **Lägg till** på hello hello sidans nederkant.
   
    ![Lägg till program](./media/active-directory-saas-scc-lifecycle-tutorial/IC749321.png "lägga till program")
5. På hello **vad vill du vill toodo** dialogrutan klickar du på **lägga till ett program från galleriet hello**.
   
    ![Lägga till ett program från gallerry](./media/active-directory-saas-scc-lifecycle-tutorial/IC749322.png "lägga till ett program från gallerry")
6. I hello **sökrutan**, typen **SCC livscykel**.
   
    ![Programgalleriet](./media/active-directory-saas-scc-lifecycle-tutorial/IC794121.png "Programgalleriet")
7. I resultatfönstret hello väljer **SCC livscykel**, och klicka sedan på **Slutför** tooadd hello program.
   
    ![SCC livscykel](./media/active-directory-saas-scc-lifecycle-tutorial/IC795082.png "SCC livscykel")
   
## <a name="configure-single-sign-on"></a>Konfigurera enkel inloggning

hello syftet med det här avsnittet är toooutline hur tooenable användare tooauthenticate tooSCC livscykel med sitt konto i Azure AD med hjälp av federation baserat på hello SAML-protokoll.

**tooconfigure enkel inloggning, utföra hello följande steg:**

1. I hello klassiska Azure-portalen på hello **SCC livscykel** integreringssidan för programmet, klickar du på **Konfigurera enkel inloggning** tooopen hello ** Konfigurera enkel inloggning ** dialogrutan.
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-scc-lifecycle-tutorial/IC794122.png "Konfigurera enkel inloggning")
2. På hello **hur skulle du som användare toosign på tooSCC livscykel** väljer **Microsoft Azure AD enkel inloggning**, och klicka sedan på **nästa**.
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-scc-lifecycle-tutorial/IC794123.png "Konfigurera enkel inloggning")
3. På hello **konfigurera App-URL** i hello sidan **logga URL** textruta typen hello URL används av dina användare toosign på tooyour SCC livscykel program med hjälp av hello följa mönstret ”*https:// bs1.SCC.com/lc7/Welcome/Customer/PICTtest.aspx*”, och klicka sedan på **nästa**.
   
    ![Konfigurera App-URL](./media/active-directory-saas-scc-lifecycle-tutorial/IC794124.png "konfigurera App-URL")
4. På hello **Konfigurera enkel inloggning på SCC livscykel** klickar du på **hämtar metadata**, och sedan spara metadatafilen lokalt på datorn.
   
   ![Konfigurera enkel inloggning](./media/active-directory-saas-scc-lifecycle-tutorial/IC795083.png "Konfigurera enkel inloggning")
5. Vidarebefordra som Metadata filen tooSCC livscykel supportteamet.
   
   >[!NOTE]
   >Enkel inloggning har toobe aktiveras med hello SCC livscykel supportteamet.
   > 
   > 

6. Välj hello konfiguration för enkel inloggning bekräftelse på hello klassiska Azure-portalen, och klicka sedan på **Slutför** tooclose hello **Konfigurera enkel inloggning** dialogrutan.
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-scc-lifecycle-tutorial/IC794125.png "Konfigurera enkel inloggning")
   
## <a name="configure-user-provisioning"></a>Konfigurera användaretablering

I ordning tooenable Azure AD-användare toolog i SCC livscykel, måste de etableras i SCC livscykel. Det finns inga uppgiften för du tooconfigure användaretablering tooSCC livscykel.

När en tilldelad användare försöker toolog i SCC livscykel, skapas automatiskt ett SCC livscykel-konto om det behövs.

>[!NOTE]
>Du kan använda något annat SCC livscykel användarens konto skapas verktyg eller API: er som tillhandahålls av SCC livscykel tooprovision AAD användarkonton.
> 
> 

## <a name="assign-users"></a>Tilldela användare
tootest konfigurationen, måste toogrant hello Azure AD-användare som du vill med hjälp av ditt program åtkomst tooit genom att tilldela dem tooallow.

**tooassign användare tooSCC livscykel, utför följande steg hello:**

1. Skapa ett testkonto i hello klassiska Azure-portalen.
2. På hello ** SCC livscykel ** integreringssidan för programmet, klickar du på **tilldela användare**.
   
    ![Tilldela användare](./media/active-directory-saas-scc-lifecycle-tutorial/IC794126.png "tilldela användare")
3. Välj din testanvändare, klicka på **tilldela**, och klicka sedan på **Ja** tooconfirm tilldelningen.
   
    ![Ja](./media/active-directory-saas-scc-lifecycle-tutorial/IC767830.png "Ja")

Om du vill tootest SSO-inställningarna, öppna hello åtkomstpanelen. Mer information om hello åtkomstpanelen finns [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).

