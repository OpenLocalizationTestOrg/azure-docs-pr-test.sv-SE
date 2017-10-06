---
title: "Självstudier: Azure Active Directory-integrering med | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och rutan."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 1c959595-6e57-4954-9c0d-67ba03ee212b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: e92baabb174642c22c99e2a30bc9c71845b3b75f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-box-for-automatic-user-provisioning"></a>Självstudier: Konfigurera rutan för automatisk Användaretablering

hello syftet med den här kursen är tooshow hello stegen tooperform i rutan och Azure AD tooautomatically etablera och avinstallation etablera användarkonton från Azure AD tooBox.

## <a name="prerequisites"></a>Krav

hello-scenario som beskrivs i den här kursen förutsätter att du redan har hello följande objekt:

*   En Azure Active directory-klient.
*   En ruta enkel inloggning för aktiverade prenumerationen.
*   Ett användarkonto i rutan Team administratörsbehörigheter.

## <a name="assigning-users-toobox"></a>Tilldela användare tooBox 

Azure Active Directory använder ett begrepp som kallas ”tilldelningar” toodetermine som användarna ska få åtkomst till tooselected appar. Hello gäller automatisk konto användaretablering är är bara hello användare och grupper som har ”tilldelats” tooan program i Azure AD synkroniserad.

Innan du konfigurerar och aktiverar hello etableras, måste toodecide vilka användare och/eller grupper i Azure AD representerar hello användare som behöver åtkomst till tooyour Box-app. När du valt, kan du tilldela dessa användare tooyour Box-app genom att följa hello anvisningarna här:

[Tilldela en användare eller grupp tooan enterprise app](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

## <a name="assign-users-and-groups"></a>Tilldela användare och grupper
Hej **rutan > användare och grupper** fliken i hello Azure-portalen kan du toospecify vilka användare och grupper som ska beviljas åtkomst tooBox. Tilldelning av en användare eller grupp gör följande saker toooccur hello:

* Azure AD tillåter hello som tilldelats användaren (antingen direkt av tilldelning av eller gruppmedlemskap) tooauthenticate tooBox. Om en användare inte har tilldelats, Azure AD tillåter inte dem toosign i tooBox och returnerar ett fel på hello Azure AD-inloggningssida.
* En app-panelen för läggs toohello användarens [startprogrammet](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users).
* Om automatisk etablering aktiveras sedan läggs hello tilldelade användare och/eller grupper toohello etablering kön toobe etableras automatiskt.
  
  * Om bara användarobjekt konfigurerade toobe etableras, sedan alla direkt tilldelade användare placeras i kö för hello etablering och alla användare som är medlemmar i de tilldelade grupperna placeras i hello etablering kön. 
  * Om gruppobjekt konfigurerade toobe etableras, är alla tilldelade gruppobjekt etablerade tooBox och alla användare som är medlemmar i dessa grupper. hello grupp och användare medlemskap bevaras vid skrivs tooBox.

Du kan använda hello **attribut > enkel inloggning** fliken tooconfigure vilka användarattribut (eller anspråk) visas tooBox under SAML-baserad autentisering och hello **attribut > etablering** fliken tooconfigure hur användar- och Gruppattribut flödar från Azure AD tooBox under etableringen åtgärder.

### <a name="important-tips-for-assigning-users-toobox"></a>Viktiga tips för att tilldela användare tooBox 

*   Vi rekommenderar att en enda Azure AD användartilldelade tooBox tootest hello etablering konfiguration. Ytterligare användare och/eller grupper kan tilldelas senare.

*   Du måste välja en giltig användarroll när du tilldelar en toobox för användaren. Hej ”standard” rollen fungerar inte för etablering.

## <a name="enable-automated-user-provisioning"></a>Aktivera automatiserad etablering av användare

Det här avsnittet hjälper att ansluta din Azure AD-tooBox användarkonto API-etablering och konfigurerar hello etablering service toocreate, uppdatera och inaktivera tilldelade användarkonton i rutan utifrån tilldelning av användare och grupper i Azure AD.

Om automatisk etablering aktiveras sedan läggs hello tilldelade användare och/eller grupper toohello etablering kön toobe etableras automatiskt.
    
 * Om endast användarobjekt är konfigurerade toobe etableras direkt tilldelade användare är placerade i kö för hello etablering och alla användare som är medlemmar i de tilldelade grupperna placeras i hello etablering kön. 
    
 * Om gruppobjekt konfigurerade toobe etableras, är alla tilldelade gruppobjekt etablerade tooBox och alla användare som är medlemmar i dessa grupper. hello grupp och användare medlemskap bevaras vid skrivs tooBox.

> [!TIP] 
> Du kan också välja tooenabled SAML-baserade enkel inloggning för Box, följa instruktionerna i hello [Azure-portalen](https://portal.azure.com). Enkel inloggning kan konfigureras oberoende av Automatisk etablering, även om dessa två funktioner komplettera varandra.

### <a name="tooconfigure-automatic-user-account-provisioning"></a>tooconfigure automatisk användarens konto-etablering:

hello syftet med det här avsnittet är toooutline hur tooenable etablering av Active Directory-användare konton tooBox.

1. I hello [Azure-portalen](https://portal.azure.com), bläddra toohello **Azure Active Directory > Företagsappar > alla program** avsnitt.

2. Om du redan har konfigurerat rutan för enkel inloggning, söka efter din instans av rutan med hjälp av hello sökfältet. Annars väljer **Lägg till** och Sök efter **rutan** i hello programgalleriet. Markera kryssrutan från hello sökresultaten och lägga till den tooyour listan med program.

3. Välj din instans av rutan och sedan hello **etablering** fliken.

4. Ange hello **etablering läge** för**automatisk**. 

    ![Etablering](./media/active-directory-saas-box-userprovisioning-tutorial/provisioning.png)

5. Under hello **administratörsautentiseringsuppgifter** klickar du på **auktorisera** tooopen en dialogruta för inloggning i ett nytt webbläsarfönster.

6. På hello **inloggning toogrant åtkomst tooBox** anger hello krävs autentiseringsuppgifter, och klicka sedan på **auktorisera**. 
   
    ![Aktivera automatisk användaretablering](./media/active-directory-saas-box-userprovisioning-tutorial/IC769546.png "aktivera automatisk användaretablering")

7. Klicka på **bevilja åtkomst tooBox** tooauthorize den här åtgärden och tooreturn toohello Azure-portalen. 
   
    ![Aktivera automatisk användaretablering](./media/active-directory-saas-box-userprovisioning-tutorial/IC769549.png "aktivera automatisk användaretablering")

8. I hello Azure-portalen klickar du på **Testanslutningen** tooensure Azure AD kan ansluta tooyour Box-app. Om hello anslutningen misslyckas, kontrollera Box-konto har administratörsbehörigheter för teamet och försök hello **”Godkänn”** steg igen.

9. Ange hello e-postadress för en person eller grupp som ska få meddelanden om etablering fel i hello **e-postmeddelande** fältet och hello kryssrutan.

10. Klicka på **spara.**

11. Välj under hello mappningar avsnitt, **tooBox synkronisera Azure Active Directory-användare.**

12. I hello **attributmappning** avsnittet kan du granska hello användarattribut som synkroniseras från Azure AD tooBox. Hej attribut som valts som **matchande** egenskaper är används toomatch hello användarkonton i rutan för uppdateringsåtgärder. Välj hello spara knappen toocommit ändringar.

13. tooenable hello Azure AD-etablering tjänsten efter, ändra hello **Status för etablering** för**på** i hello inställningar

14. Klicka på **spara.**

Som startar hello den första synkroniseringen av användare och/eller grupper som har tilldelats tooBox i hello användare och grupper avsnitt. hello inledande synkronisering tar längre tid tooperform än efterföljande synkroniseringar som sker ungefär var tjugonde minut så länge hello-tjänsten körs. Du kan använda hello **synkroniseringsinformation** avsnittet toomonitor förlopp och följ länkarna tooprovisioning aktivitetsrapporter, som beskriver alla åtgärder som utförs av hello etableras på Box-app.

Du kan nu skapa ett testkonto. Vänta tills in tooverify hello kontot har synkroniserats toobox too20 i minuter.

I rutan-klient synkroniserade användare visas under **hanterade användare** i hello **administratörskonsolen**.

![Integration status](./media/active-directory-saas-box-userprovisioning-tutorial/IC769556.png "integrering status")


## <a name="additional-resources"></a>Ytterligare resurser

* [Hantera användare konto-etablering för företag-appar](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Konfigurera enkel inloggning](active-directory-saas-box-tutorial.md)