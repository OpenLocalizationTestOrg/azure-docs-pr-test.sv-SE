---
title: "Självstudier: Azure Active Directory-integrering med arbetsplats av Facebook | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och arbetsplats med Facebook."
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
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 551ec353a5ec1da936373587688c299a6f4acca7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-workplace-by-facebook-for-user-provisioning"></a>Självstudier: Konfigurera arbetsplats av Facebook för Användaretablering

hello syftet med den här kursen är tooshow du hello stegen tooperform arbetsplatsen av Facebook och Azure AD tooautomatically etablera och avinstallation etablera användarkonton från Azure AD tooWorkplace av Facebook.

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med arbetsplats av Facebook, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En arbetsplats av Facebook enkel inloggning på aktiverade prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="assigning-users-tooworkplace-by-facebook"></a>Tilldela användare tooWorkplace av Facebook

Azure Active Directory använder ett begrepp som kallas ”tilldelningar” toodetermine som användarna ska få åtkomst till tooselected appar. Hello gäller automatisk konto användaretablering är är bara hello användare och grupper som har ”tilldelats” tooan program i Azure AD synkroniserad.

Innan du konfigurerar och aktiverar hello etableras, måste toodecide vilka användare och/eller grupper i Azure AD som representerar hello-användare som behöver åtkomst till tooyour arbetsplats av Facebook-app. När du valt, kan du tilldela dessa användare tooyour arbetsplats av Facebook-app genom att följa hello anvisningarna här:

[Tilldela en användare eller grupp tooan enterprise app](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-tooworkplace-by-facebook"></a>Viktiga tips för att tilldela användare tooWorkplace av Facebook

*   Vi rekommenderar att en enda Azure AD-användare är tilldelad tooWorkplace av Facebook tootest hello etablering konfiguration. Ytterligare användare och/eller grupper kan tilldelas senare.

*   Du måste välja en giltig användarroll när du tilldelar en användare tooWorkplace av Facebook. Hej ”standard” rollen fungerar inte för etablering.

## <a name="enable-user-provisioning"></a>Aktivera etablering av användare

Det här avsnittet hjälper dig att ansluta din Azure AD-tooWorkplace av Facebooks användarkontot API-etablering och konfigurerar hello etablering service toocreate, uppdatera och inaktivera tilldelade användarkonton i arbetsplats av Facebook baserat på användare och grupper tilldelning i Azure AD.

>[!Tip]
>Du kan också välja tooenabled SAML-baserade enkel inloggning för arbetsplats av Facebook, följa instruktionerna i hello [Azure-portalen](https://portal.azure.com). Enkel inloggning kan konfigureras oberoende av Automatisk etablering, även om dessa två funktioner komplettera varandra.

### <a name="tooconfigure-user-account-provisioning-tooworkplace-by-facebook-in-azure-ad"></a>tooconfigure användarkonto tooWorkplace av Facebook-etablering i Azure AD:

hello syftet med det här avsnittet är toooutline hur tooenable etablering av Active Directory-användare konton tooWorkplace av Facebook.

Azure AD stöder hello möjlighet tooautomatically synkronisera hello kontoinformation för tilldelade användare tooWorkplace av Facebook. Den automatiska synkroniseringen kan arbetsplatsen Facebook tooget hello data måste tooauthorize användare för åtkomst, innan du dem och försök toosign i för hello första gången. Den också etablerar Frigör användare från arbetsplats av Facebook när åtkomst har återkallats i Azure AD.

1. I hello [Azure-portalen](https://portal.azure.com), bläddra toohello **Azure Active Directory** > **Företagsappar** > **alla program** avsnitt.

2. Om du redan har konfigurerat arbetsplats av Facebook för enkel inloggning, söka efter din instans av arbetsplatsen av Facebook hjälp hello sökfältet. Annars väljer **Lägg till** och Sök efter **arbetsplats av Facebook** i hello programgalleriet. Välj arbetsplats av Facebook från hello sökresultaten och lägga till den tooyour listan med program.

3. Välj din arbetsplats av Facebook-instans och sedan hello **etablering** fliken.

4. Ange hello **etablering läge** för**automatisk**. 

    ![Etablering](./media/active-directory-saas-workplacebyfacebook-provisioning-tutorial/provisioning.png)

5. Under hello **administratörsautentiseringsuppgifter** avsnittet anger hello hemlighet Token och hello klient-URL för din arbetsplats av Facebook-administratör.

6. I hello Azure-portalen klickar du på **Testanslutningen** tooensure Azure AD kan ansluta tooyour arbetsplats av Facebook-app. Om hello anslutningen misslyckas, kontrollera din arbetsplats av Facebook-konto har teamet administratörsbehörigheter.

7. Ange hello e-postadress för en person eller grupp som ska få meddelanden om etablering fel i hello **e-postmeddelande** fältet och hello kryssrutan.

8. Klicka på **spara.**

9. Välj under hello mappningar avsnitt, **synkronisera Azure Active Directory-användare tooWorkplace av Facebook.**

10. I hello **attributmappning** avsnittet kan du granska hello användarattribut som synkroniseras från Azure AD tooWorkplace av Facebook. Hej attribut som valts som **matchande** egenskaper är används toomatch hello användarkonton i arbetsplats av Facebook för uppdateringsåtgärder. Välj hello spara knappen toocommit ändringar.

11. tooenable hello Azure AD-etablering tjänsten för arbetsplats av Facebook, ändra hello **Status för etablering** för**på** i hello **inställningar** avsnitt

12. Klicka på **spara.**

Mer information om hur tooconfigure Automatisk etablering, se [https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers](https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers)

Du kan nu skapa ett testkonto. Vänta tills in too20 minuter tooverify hello kontot har synkroniserats tooWorkplace av Facebook.

## <a name="additional-resources"></a>Ytterligare resurser

* [Hantera användare konto-etablering för företag-appar](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Konfigurera enkel inloggning](active-directory-saas-workplacebyfacebook-tutorial.md)

