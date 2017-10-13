---
title: "Självstudier: Azure Active Directory-integrering med arbetsplats av Facebook | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och arbetsplats med Facebook."
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
ms.openlocfilehash: 9b22679c304248ed7ba7a6bd9eaf82b64f7143cf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-configuring-workplace-by-facebook-for-user-provisioning"></a>Självstudier: Konfigurera arbetsplats av Facebook för Användaretablering

Syftet med den här kursen är att visa de steg som du behöver utföra på arbetsplats av Facebook och Azure AD för att automatiskt etablera och avetablera användarkonton från Azure AD till arbetsplatsen med Facebook.

## <a name="prerequisites"></a>Krav

För att konfigurera Azure AD-integrering med arbetsplats av Facebook, behöver du följande:

- En Azure AD-prenumeration
- En arbetsplats av Facebook enkel inloggning på aktiverade prenumeration

> [!NOTE]
> Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.

Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="assigning-users-to-workplace-by-facebook"></a>Tilldela användare till arbetsplatsen av Facebook

Azure Active Directory använder ett begrepp som kallas ”tilldelningar” för att avgöra vilka användare ska få åtkomst till valda appar. I samband med automatisk konto användaretablering, synkroniseras de användare och grupper som har ”tilldelats” till ett program i Azure AD.

Innan du konfigurerar och aktiverar tjänsten etablering, måste du bestämma vilka användare och/eller grupper i Azure AD representerar de användare som behöver åtkomst till din arbetsplats av Facebook-app. När bestämt, kan du tilldela dessa användare till din arbetsplats av Facebook-app genom att följa anvisningarna här:

[Tilldela en användare eller grupp till en enterprise-app](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-workplace-by-facebook"></a>Viktiga tips för att tilldela användare till arbetsplatsen av Facebook

*   Vi rekommenderar att en enda Azure AD-användare har tilldelats till arbetsplatsen av Facebook testa allokering konfigurationen. Ytterligare användare och/eller grupper kan tilldelas senare.

*   Du måste välja en giltig användarroll när du tilldelar en användare till arbetsplatsen med Facebook. Rollen ”standard åtkomst” fungerar inte för etablering.

## <a name="enable-user-provisioning"></a>Aktivera etablering av användare

Det här avsnittet hjälper dig att ansluta din Azure AD till arbetsplatsen via Facebooks användarkontot etablering API och konfigurera tjänsten etablering för att skapa, uppdatera och inaktivera tilldelade användarkonton i arbetsplats av Facebook baserat på tilldelning av användare och grupper i Azure AD.

>[!Tip]
>Du kan också välja att aktivera SAML-baserade enkel inloggning för arbetsplats med Facebook, följer du instruktionerna som anges i [Azure-portalen](https://portal.azure.com). Enkel inloggning kan konfigureras oberoende av Automatisk etablering, även om dessa två funktioner komplettera varandra.

### <a name="to-configure-user-account-provisioning-to-workplace-by-facebook-in-azure-ad"></a>Så här konfigurerar du kontot användaretablering till arbetsplatsen av Facebook i Azure AD:

Syftet med det här avsnittet är att beskriva hur du aktiverar etablering av Active Directory-användarkonton till arbetsplatsen med Facebook.

Azure AD stöder möjligheten att synkronisera automatiskt kontoinformation för tilldelade användare till arbetsplatsen med Facebook. Den automatiska synkroniseringen kan arbetsplatsen av Facebook och hämta data som behövs för att auktorisera användare för åtkomst, innan de försöker logga in för första gången. Den också etablerar Frigör användare från arbetsplats av Facebook när åtkomst har återkallats i Azure AD.

1. I den [Azure-portalen](https://portal.azure.com), bläddra till den **Azure Active Directory** > **Företagsappar** > **alla program** avsnitt.

2. Om du redan har konfigurerat arbetsplats av Facebook för enkel inloggning, söka efter din arbetsplats med hjälp av sökfältet Facebook-instans. Annars väljer **Lägg till** och Sök efter **arbetsplats av Facebook** i programgalleriet. Välj arbetsplats av Facebook i sökresultatet och lägga till den i listan med program.

3. Välj din arbetsplats av Facebook-instans och sedan den **etablering** fliken.

4. Ange den **Etableringsläge** till **automatisk**. 

    ![Etablering](./media/active-directory-saas-workplacebyfacebook-provisioning-tutorial/provisioning.png)

5. Under den **administratörsautentiseringsuppgifter** Ange hemlighet-Token och klient-URL för din arbetsplats av Facebook-administratör.

6. I Azure-portalen klickar du på **Testanslutningen** så Azure AD kan ansluta till din arbetsplats med Facebook-app. Om anslutningen misslyckas, kontrollera din arbetsplats av Facebook-konto har teamet administratörsbehörigheter.

7. Ange e-postadressen för en person eller grupp som ska få meddelanden om etablering fel i den **e-postmeddelande** fält och markera kryssrutan.

8. Klicka på **spara.**

9. Välj under avsnittet mappningar **synkronisera Azure Active Directory-användare till arbetsplatsen med Facebook.**

10. I den **attributmappning** avsnittet kan du granska användarattribut som synkroniseras från Azure AD till arbetsplatsen med Facebook. De attribut som valts som **matchande** egenskaper används för att matcha användarkonton i arbetsplats av Facebook för uppdateringsåtgärder. Välj knappen Spara för att genomföra ändringarna.

11. Om du vill aktivera Azure AD etableras för arbetsplats av Facebook, ändra den **Status för etablering** till **på** i den **inställningar** avsnitt

12. Klicka på **spara.**

Mer information om hur du konfigurerar automatisk etablering finns [https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers](https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers)

Du kan nu skapa ett testkonto. Vänta i upp till 20 minuter att verifiera att kontot har synkroniserats till arbetsplatsen med Facebook.

## <a name="additional-resources"></a>Ytterligare resurser

* [Hantera användare konto-etablering för företag-appar](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Konfigurera enkel inloggning](active-directory-saas-workplacebyfacebook-tutorial.md)

