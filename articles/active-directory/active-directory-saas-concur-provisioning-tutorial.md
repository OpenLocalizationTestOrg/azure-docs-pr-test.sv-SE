---
title: "Självstudier: Azure Active Directory-integrering med Concur | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Concur."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: df47f55f-a894-4e01-a82e-0dbf55fc8af1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: 13ba364af26a5ce0f1d2b51aaa0f84a4c353b107
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-concur-for-user-provisioning"></a>Självstudier: Konfigurera verklig för Användaretablering

hello syftet med den här kursen är tooshow du hello stegen tooperform i Concur och Azure AD tooautomatically etablera och avinstallation etablera användarkonton från Azure AD tooConcur.

## <a name="prerequisites"></a>Krav

hello-scenario som beskrivs i den här kursen förutsätter att du redan har hello följande objekt:

*   En Azure Active directory-klient.
*   Aktivera prenumerationen en Concur enkel inloggning.
*   Ett användarkonto i Concur Team administratörsbehörigheter.

## <a name="assigning-users-tooconcur"></a>Tilldela användare tooConcur

Azure Active Directory använder ett begrepp som kallas ”tilldelningar” toodetermine som användarna ska få åtkomst till tooselected appar. Hello gäller automatisk konto användaretablering är är bara hello användare och grupper som har ”tilldelats” tooan program i Azure AD synkroniserad.

Innan du konfigurerar och aktiverar hello etableras, måste toodecide vilka användare och/eller grupper i Azure AD representerar hello användare som behöver åtkomst till tooyour Concur app. När du valt, kan du tilldela dessa användare tooyour Concur app genom att följa hello anvisningarna här:

[Tilldela en användare eller grupp tooan enterprise app](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-tooconcur"></a>Viktiga tips för att tilldela användare tooConcur

*   Vi rekommenderar att en enda Azure AD-användare tilldelas tooConcur tootest hello etablering konfiguration. Ytterligare användare och/eller grupper kan tilldelas senare.

*   Du måste välja en giltig användarroll när du tilldelar en tooConcur för användaren. Hej ”standard” rollen fungerar inte för etablering.

## <a name="enable-user-provisioning"></a>Aktivera användaretablering

Det här avsnittet hjälper dig att ansluta din Azure AD-tooConcur användarkonto API-etablering och konfigurerar hello etablering service toocreate, uppdatera och inaktivera tilldelade användarkonton i Concur baserat på tilldelning av användare och grupper i Azure AD.

> [!Tip] 
> Du kan också välja tooenabled SAML-baserade enkel inloggning för Concur, följa instruktionerna i hello [Azure-portalen](https://portal.azure.com). Enkel inloggning kan konfigureras oberoende av Automatisk etablering, även om dessa två funktioner komplettera varandra.

### <a name="tooconfigure-user-account-provisioning"></a>tooconfigure användarens konto-etablering:

hello syftet med det här avsnittet är toooutline hur tooenable etablering av Active Directory-användare konton tooConcur.

tooenable appar i Hej utgifter Service, har det rätt inställning för toobe och användning av en Web Service Admin-profil. Lägg inte till hello WS Admin rollen tooyour befintliga administratörsprofilen som du använder för T & E administrativa funktioner.

Verklig konsulter eller Hej administratör måste skapa en egen Web tjänstadministratör profil och Hej administratör måste använda den här profilen för hello Web Services Administrator funktioner (till exempel aktiverar appar). Dessa profiler måste hållas separat från hello klienten administratör dagliga T & E admin-profil (hello T & E admin-profil ska inte ha tilldelats hello WSAdmin rollen).

När du skapar hello profil toobe används för att aktivera hello app måste ange hello klienten administratörens namn i hello Användarfält profil. Den här tilldelas ägarskap toohello profil. När du har skapat en eller flera profiler hello klienten måste logga in med den här profilen tooclick hello ”*aktivera*” knappen för en Partner App hello Web Services-menyn.

För hello följande orsaker, utföras åtgärden inte med hello-profil som de använder för normal T & E-administration.

* hello klienten har toobe hello som klickar på ”*Ja*” hello dialog-fönstret som visas när en app är aktiverad. Detta klickar du på bekräftar hello klienten beredda för hello Partner program tooaccess sina data så att du eller hello Partner inte kan klicka på som Ja knappar.

* Om en administratör som har aktiverat en app med hello T & E admin-profil slutar hello företaget (vilket ger hello-profil som inaktiverat), aktiverade appar med hjälp av den här profilen fungerar inte förrän hello app aktiveras med en annan active WS-administratör profil. Det är därför du borde toocreate distinkta WS Admin-profiler.

* Om en administratör slutar hello företaget, associerade hello namn toohello WS Admin-profil kan vara ändrade toohello ersättning administratör om du vill utan att påverka hello aktiverat appen eftersom den här profilen inte behöver inaktiverat.

**tooconfigure användaretablering, utför följande steg hello:**

1. Logga in tooyour **Concur** klient.

2. Från hello **Administration** väljer du **Web Services**.
   
    ![Concur klient](./media/active-directory-saas-concur-provisioning-tutorial/IC721729.png "Concur klient")

3. På vänster sida, från hello hello **Web Services** väljer **aktivera Partner program**.
   
    ![Aktivera Partner program](./media/active-directory-saas-concur-provisioning-tutorial/ic721730.png "gör att Partner-program")

4. Från hello **aktivera programmet** väljer **Azure Active Directory**, och klicka sedan på **aktivera**.
   
    ![Microsoft Azure Active Directory](./media/active-directory-saas-concur-provisioning-tutorial/ic721731.png "Microsoft Azure Active Directory")

5. Klicka på **Ja** tooclose hello **bekräfta åtgärden** dialogrutan.
   
    ![Bekräfta åtgärden](./media/active-directory-saas-concur-provisioning-tutorial/ic721732.png "bekräfta åtgärden")

6. I hello [Azure-portalen](https://portal.azure.com), bläddra toohello **Azure Active Directory > Företagsappar > alla program** avsnitt.

7. Om du redan har konfigurerat Concur för enkel inloggning, söka efter din instans av Concur hjälp hello sökfältet. Annars väljer **Lägg till** och Sök efter **Concur** i hello programgalleriet. Välj Concur från hello sökresultaten och lägga till den tooyour listan med program.

8. Välj din instans av Concur och sedan hello **etablering** fliken.

9. Ange hello **etablering läge** för**automatisk**. 
 
    ![Etablering](./media/active-directory-saas-concur-provisioning-tutorial/provisioning.png)

10. Under hello **administratörsautentiseringsuppgifter** ange hello **användarnamn** och hello **lösenord** av administratören Concur.

11. I hello Azure-portalen klickar du på **Testanslutningen** tooensure Azure AD kan ansluta tooyour Concur app. Om hello anslutning misslyckas kan du kontrollera att ditt Concur-konto har teamet administratörsbehörigheter.

12. Ange hello e-postadress för en person eller grupp som ska få meddelanden om etablering fel i hello **e-postmeddelande** fältet och hello kryssrutan.

13. Klicka på **spara.**

14. Välj under hello mappningar avsnitt, **tooConcur synkronisera Azure Active Directory-användare.**

15. I hello **attributmappning** avsnittet kan du granska hello användarattribut som synkroniseras från Azure AD tooConcur. Hej attribut som valts som **matchande** egenskaper är används toomatch hello användarkonton i Concur för uppdateringsåtgärder. Välj hello spara knappen toocommit ändringar.

16. tooenable hello Azure AD-etablering tjänsten för Concur, ändra hello **Status för etablering** för**på** i hello **inställningar** avsnitt

17. Klicka på **spara.**

Du kan nu skapa ett testkonto. Vänta tills in tooverify hello kontot har synkroniserats tooConcur too20 i minuter.

## <a name="additional-resources"></a>Ytterligare resurser

* [Hantera användare konto-etablering för företag-appar](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Konfigurera enkel inloggning](active-directory-saas-concur-tutorial.md)

