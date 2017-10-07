---
title: "Självstudier: Azure Active Directory-integrering med Netsuite | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Netsuite."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 8a6d3994-ee33-4a6f-b0a2-9d0389467f16
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 5bb2989c1296b9f2abc9e8c84855731adc484aab
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-netsuite-for-automatic-user-provisioning"></a>Självstudier: Konfigurera Netsuite för automatisk Användaretablering

hello syftet med den här kursen är tooshow du hello stegen tooperform i Netsuite och Azure AD tooautomatically etablera och avinstallation etablera användarkonton från Azure AD tooNetsuite.

## <a name="prerequisites"></a>Krav

hello-scenario som beskrivs i den här kursen förutsätter att du redan har hello följande objekt:

*   En Azure Active directory-klient.
*   En Netsuite enkel inloggning för aktiverade prenumerationen.
*   Ett användarkonto i Netsuite Team administratörsbehörigheter.

## <a name="assigning-users-toonetsuite"></a>Tilldela användare tooNetsuite

Azure Active Directory använder ett begrepp som kallas ”tilldelningar” toodetermine som användarna ska få åtkomst till tooselected appar. Hello gäller automatisk konto användaretablering är är bara hello användare och grupper som har ”tilldelats” tooan program i Azure AD synkroniserade.

Innan du konfigurerar och aktiverar hello etableras, måste toodecide vilka användare och/eller grupper i Azure AD representerar hello användare som behöver åtkomst till tooyour Netsuite app. När du valt, kan du tilldela dessa användare tooyour Netsuite app genom att följa hello anvisningarna här:

[Tilldela en användare eller grupp tooan enterprise app](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-toonetsuite"></a>Viktiga tips för att tilldela användare tooNetsuite

*   Vi rekommenderar att en enda Azure AD-användare är tilldelad tooNetsuite tootest hello etablering konfiguration. Ytterligare användare och/eller grupper kan tilldelas senare.

*   Du måste välja en giltig användarroll när du tilldelar en tooNetsuite för användaren. Hej ”standard” rollen fungerar inte för etablering.

## <a name="enable-user-provisioning"></a>Aktivera etablering av användare

Det här avsnittet hjälper dig att ansluta din Azure AD-tooNetsuite användarkonto API-etablering och konfigurerar hello etablering service toocreate, uppdatera och inaktivera tilldelade användarkonton i Netsuite baserat på tilldelning av användare och grupper i Azure AD.

> [!TIP] 
> Du kan också välja tooenabled SAML-baserade enkel inloggning för Netsuite, följa instruktionerna i hello [Azure-portalen](https://portal.azure.com). Enkel inloggning kan konfigureras oberoende av Automatisk etablering, även om dessa två funktioner komplettera varandra.

### <a name="tooconfigure-user-account-provisioning"></a>tooconfigure användarens konto-etablering:

hello syftet med det här avsnittet är toooutline hur tooenable användaretablering Active Directory-användare konton tooNetsuite.

1. I hello [Azure-portalen](https://portal.azure.com), bläddra toohello **Azure Active Directory > Företagsappar > alla program** avsnitt.

2. Om du redan har konfigurerat Netsuite för enkel inloggning, söka efter din instans av Netsuite hjälp hello sökfältet. Annars väljer **Lägg till** och Sök efter **Netsuite** i hello programgalleriet. Välj Netsuite från hello sökresultaten och lägga till den tooyour listan med program.

3. Välj din instans av Netsuite och sedan hello **etablering** fliken.

4. Ange hello **etablering läge** för**automatisk**. 

    ![Etablering](./media/active-directory-saas-netsuite-provisioning-tutorial/provisioning.png)

5. Under hello **administratörsautentiseringsuppgifter** avsnittet, ange hello följande inställningar:
   
    a. I hello **administratörsanvändarnamnet** textruta typen en Netsuite kontonamn som har hello **systemadministratören** profil i Netsuite.com som tilldelats.
   
    b. I hello **adminlösenord** textruta typen hello lösenordet för kontot.
      
6. I hello Azure-portalen klickar du på **Testanslutningen** tooensure Azure AD kan ansluta tooyour Netsuite app.

7. I hello **e-postmeddelande** anger hello e-postadressen för en person eller grupp som ska ta emot meddelanden om etablering fel och hello kryssrutan.

8. Klicka på **spara.**

9. Välj under hello mappningar avsnitt, **tooNetsuite synkronisera Azure Active Directory-användare.**

10. I hello **attributmappning** avsnittet kan du granska hello användarattribut som synkroniseras från Azure AD tooNetsuite. Observera att hello attribut som valts som **matchande** egenskaper är används toomatch hello användarkonton i Netsuite för uppdateringsåtgärder. Välj hello spara knappen toocommit ändringar.

11. tooenable hello Azure AD-etablering tjänsten för Netsuite, ändra hello **Status för etablering** för**på** i hello inställningar

12. Klicka på **spara.**

Den startar hello den första synkroniseringen av användare och/eller grupper som har tilldelats tooNetsuite i hello användare och grupper avsnitt. Observera att hello inledande synkronisering tar längre tid tooperform än efterföljande synkroniseringar som sker ungefär var tjugonde minut så länge hello-tjänsten körs. Du kan använda hello **synkroniseringsinformation** avsnittet toomonitor förlopp och följ länkarna tooprovisioning aktivitetsrapporter, som beskriver alla åtgärder som utförs av hello etableras Netsuite appen.

Du kan nu skapa ett testkonto. Vänta tills in tooverify hello kontot har synkroniserats tooNetsuite too20 i minuter.

## <a name="additional-resources"></a>Ytterligare resurser

* [Hantera användare konto-etablering för företag-appar](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Konfigurera enkel inloggning](active-directory-saas-netsuite-tutorial.md)