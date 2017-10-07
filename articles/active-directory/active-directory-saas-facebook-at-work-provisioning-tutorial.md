---
title: "Självstudier: Konfigurera arbetsplats av Facebook för användaretablering | Microsoft Docs"
description: "Lär dig hur tooautomatically etablera och avinstallation etablera användarkontona från Azure AD tooWorkplace av Facebook."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 6341e67e-8ce6-42dc-a4ea-7295904a53ef
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 33d294dbc8f441b29138408b3c9ca41f2141f8af
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configure-workplace-by-facebook-for-user-provisioning"></a>Självstudier: Konfigurera arbetsplats av Facebook för användaretablering

Den här självstudiekursen visas du hello steg nödvändiga tooautomatically etablera och avetablera användarkonton från Azure Active Directory (AD Azure) tooWorkplace av Facebook.

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med arbetsplats av Facebook, behöver du hello följande:

- En Azure AD-prenumeration
- En arbetsplats av Facebook enkel inloggning (SSO) aktiverat prenumeration

tootest hello stegen i den här självstudiekursen, Följ dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du få en [utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).

## <a name="assign-users-tooworkplace-by-facebook"></a>Tilldela användare tooWorkplace av Facebook

Azure AD använder ett begrepp som kallas ”tilldelningar” toodetermine som användarna ska få åtkomst till tooselected appar. Hello gäller automatisk konto användaretablering är är bara hello användare och grupper som har tilldelats tooan program i Azure AD synkroniserade.

Innan du konfigurerar och aktiverar hello etableras, bestämma vilka användare och grupper i Azure AD representera hello-användare som behöver åtkomst till tooyour arbetsplats av Facebook-app. Du kan tilldela dessa användare tooyour arbetsplats av Facebook-app genom att följa anvisningarna i hello [tilldela en användare eller grupp tooan enterprise app](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal).

>[!IMPORTANT]
>*   Testa hello etablering konfiguration genom att tilldela en enda tooWorkplace för Azure AD-användare med Facebook. Tilldela ytterligare användare och grupper senare.
>*   Du måste välja en giltig användarroll när du tilldelar en användare tooWorkplace av Facebook. hello standard Access roll fungerar inte för etablering.

## <a name="enable-automated-user-provisioning"></a>Aktivera automatisk användaretablering

Det här avsnittet hjälper dig att ansluta din Azure AD toohello användarkonto etablering API för arbetsplats med Facebook. Du också lära dig hur tooconfigure hello etablering service toocreate, uppdatera och inaktivera tilldelade användarkonton i arbetsplats med Facebook. Detta baseras på användare och grupptilldelning i Azure AD.

>[!Tip]
>Du kan också välja tooenabled SAML-baserade SSO för arbetsplats av Facebook, av följande hello instruktionerna i hello [Azure-portalen](https://portal.azure.com). Enkel inloggning kan konfigureras oberoende av Automatisk etablering, även om dessa två funktioner kompletterar varandra.

### <a name="configure-user-account-provisioning-tooworkplace-by-facebook-in-azure-ad"></a>Konfigurera användarkonto tooWorkplace av Facebook-etablering i Azure AD

Azure AD stöder hello möjlighet tooautomatically synkronisera hello kontoinformation för tilldelade användare tooWorkplace av Facebook. Den automatiska synkroniseringen kan arbetsplatsen Facebook tooget hello data måste tooauthorize användare för åtkomst, innan du dem och försök toosign i för hello första gången. Den också etablerar Frigör användare från arbetsplats av Facebook när åtkomst har återkallats i Azure AD.

1. I hello [Azure-portalen](https://portal.azure.com)väljer **Azure Active Directory** > **Företagsappar** > **alla program**.

2. Om du redan har konfigurerat arbetsplats av Facebook för enkel inloggning kan du söka efter din arbetsplats av Facebook-instans med hjälp av hello sökfältet. Annars väljer **Lägg till** och Sök efter **arbetsplats av Facebook** i hello programgalleriet. Välj **arbetsplats av Facebook** från hello sökresultat och lägga till den tooyour listan med program.

3. Välj din instans av arbetsplatsen av Facebook och välj sedan hello **etablering** fliken.

4. Ange **etablering läge** för**automatisk**. 

    ![Skärmbild av arbetsplatsen av Facebook alternativ för etablering](./media/active-directory-saas-facebook-at-work-provisioning-tutorial/provisioning.png)

5. Under hello **administratörsautentiseringsuppgifter** ange hello **hemlighet Token** och hello **klient URL** för din arbetsplats av Facebook-administratör.

6. Välj i hello Azure-portalen, **Testanslutningen** tooensure Azure AD kan ansluta tooyour arbetsplats av Facebook-app. Om hello anslutning misslyckas kan du kontrollera att din arbetsplats av Facebook-konto har teamet administratörsbehörigheter.

7. Ange hello e-postadress för en person eller grupp som ska få meddelanden om etablering fel i hello **e-postmeddelande** fältet och hello kryssrutan.

8. Välj **Spara**.

9. Välj under hello mappningar avsnitt, **synkronisera Azure Active Directory-användare tooWorkplace av Facebook**.

10. I hello **attributmappning** avsnittet kan du granska hello användarattribut som synkroniseras från Azure AD tooWorkplace av Facebook. Hej attribut som valts som **matchande** egenskaper är används toomatch hello användarkonton i arbetsplats av Facebook för uppdateringsåtgärder. toocommit ändringar, Välj **spara**.

11. tooenable hello Azure AD-etablering tjänsten för arbetsplats av Facebook, i hello **inställningar** ändrar hello **Status för etablering** för**på**.

12. Välj **Spara**.

Mer information om hur tooconfigure Automatisk etablering, se [hello Facebook dokumentationen](https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers).

Du kan nu skapa ett testkonto. Vänta tills in too20 minuter tooverify hello kontot har synkroniserats tooWorkplace av Facebook.

## <a name="additional-resources"></a>Ytterligare resurser

* [Hantera användare konto-etablering för företagsappar](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Konfigurera enkel inloggning](active-directory-saas-facebook-at-work-tutorial.md)

