---
title: "Självstudier: Azure Active Directory-integrering med Citrix GoToMeeting | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Citrix GoToMeeting."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 0f59fedb-2cf8-48d2-a5fb-222ed943ff78
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 3c6eed5309dfa384c292b0cf63f8aa58988add81
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-citrix-gotomeeting-for-automatic-user-provisioning"></a>Självstudier: Konfigurera Citrix GoToMeeting för automatisk Användaretablering

hello syftet med den här kursen är tooshow du hello stegen tooperform i Citrix GoToMeeting och Azure AD tooautomatically etablera och avinstallation etablera användarkonton från Azure AD tooCitrix GoToMeeting.

## <a name="prerequisites"></a>Krav

hello-scenario som beskrivs i den här kursen förutsätter att du redan har hello följande objekt:

*   En Azure Active directory-klient.
*   En Citrix GoToMeeting enkel inloggning för aktiverade prenumerationen.
*   Ett användarkonto i Citrix GoToMeeting Team administratörsbehörigheter.

## <a name="assigning-users-toocitrix-gotomeeting"></a>Tilldela användare tooCitrix GoToMeeting

Azure Active Directory använder ett begrepp som kallas ”tilldelningar” toodetermine som användarna ska få åtkomst till tooselected appar. Hello gäller automatisk konto användaretablering är är bara hello användare och grupper som har ”tilldelats” tooan program i Azure AD synkroniserad.

Innan du konfigurerar och aktiverar hello etableras, måste toodecide vilka användare och/eller grupper i Azure AD som representerar hello-användare som behöver åtkomst till tooyour Citrix GoToMeeting app. När du valt, kan du tilldela dessa användare tooyour Citrix GoToMeeting app genom att följa hello anvisningarna här:

[Tilldela en användare eller grupp tooan enterprise app](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-toocitrix-gotomeeting"></a>Viktiga tips för att tilldela användare tooCitrix GoToMeeting

*   Vi rekommenderar att en enda Azure AD-användare är tilldelad tooCitrix GoToMeeting tootest hello etablering konfiguration. Ytterligare användare och/eller grupper kan tilldelas senare.

*   Du måste välja en giltig användarroll när du tilldelar en användare tooCitrix GoToMeeting. Hej ”standard” rollen fungerar inte för etablering.

## <a name="enable-automated-user-provisioning"></a>Aktivera automatiserad etablering av användare

Det här avsnittet hjälper dig att ansluta din Azure AD tooCitrix GoToMeeting användarkontot API-etablering och konfigurerar hello etablering service toocreate, uppdatera och inaktivera tilldelad användare konton i Citrix GoToMeeting baserat på användare och grupper tilldelning i Azure AD.

> [!TIP]
> Du kan också välja tooenabled SAML-baserade enkel inloggning för Citrix GoToMeeting, följa instruktionerna i hello [Azure-portalen](https://portal.azure.com). Enkel inloggning kan konfigureras oberoende av Automatisk etablering, även om dessa två funktioner komplettera varandra.

### <a name="tooconfigure-automatic-user-account-provisioning"></a>tooconfigure automatisk användarens konto-etablering:

1. I hello [Azure-portalen](https://portal.azure.com), bläddra toohello **Azure Active Directory > Företagsappar > alla program** avsnitt.

2. Om du redan har konfigurerat Citrix GoToMeeting för enkel inloggning, söka efter din instans av Citrix GoToMeeting hjälp hello sökfältet. Annars väljer **Lägg till** och Sök efter **Citrix GoToMeeting** i hello programgalleriet. Välj Citrix GoToMeeting från hello sökresultaten och lägga till den tooyour listan med program.

3. Välj din instans av Citrix GoToMeeting och sedan hello **etablering** fliken.

4. Ange hello **etablering** läge för**automatisk**. 

    ![Etablering](./media/active-directory-saas-citrixgotomeeting-provisioning-tutorial/provisioning.png)

5. Gör följande hello under hello administratörsautentiseringsuppgifter avsnitt:
   
    a. I hello **Citrix GoToMeeting administratörsanvändarnamnet** textruta Skriv hello användarnamn för en administratör.

    b. I hello **Citrix GoToMeeting adminlösenord** textruta hello administratörslösenord.

6. I hello Azure-portalen klickar du på **Testanslutningen** tooensure Azure AD kan ansluta tooyour Citrix GoToMeeting app. Om hello anslutningen misslyckas, kontrollera kontots Citrix GoToMeeting har administratörsbehörigheter för Team och försök hello **”administratörsautentiseringsuppgifter”** steg igen.

7. Ange hello e-postadress för en person eller grupp som ska få meddelanden om etablering fel i hello **e-postmeddelande** fältet och hello kryssrutan.

8. Klicka på **spara.**

9. Välj under hello mappningar avsnitt, **synkronisera Azure Active Directory-användare tooCitrix GoToMeeting.**

10. I hello **attributmappning** avsnittet kan du granska hello användarattribut som synkroniseras från Azure AD tooCitrix GoToMeeting. Hej attribut som valts som **matchande** egenskaper är används toomatch hello användarkonton i Citrix GoToMeeting för uppdateringsåtgärder. Välj hello spara knappen toocommit ändringar.

11. tooenable hello Azure AD-etablering tjänsten för Citrix GoToMeeting, ändra hello **Status för etablering** för**på** i hello inställningar

12. Klicka på **spara.**

Den startar hello inledande synkronisering av alla användare och/eller grupper som har tilldelats tooCitrix GoToMeeting under hello användare och grupper. hello inledande synkronisering tar längre tid tooperform än efterföljande synkroniseringar som sker ungefär var tjugonde minut så länge hello-tjänsten körs. Du kan använda hello **synkroniseringsinformation** avsnittet toomonitor förlopp och följ länkarna tooprovisioning aktivitetsrapporter, som beskriver alla åtgärder som utförs av hello etableras Citrix GoToMeeting appen.

## <a name="additional-resources"></a>Ytterligare resurser

* [Hantera användare konto-etablering för företag-appar](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Konfigurera enkel inloggning](active-directory-saas-citrix-gotomeeting-tutorial.md)


