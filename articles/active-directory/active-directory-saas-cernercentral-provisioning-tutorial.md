---
title: "Självstudier: Konfigurera Cerner Central för automatisk användaretablering med Azure Active Directory | Microsoft Docs"
description: "Lär dig hur tooconfigure Azure Active Directory tooautomatically etablera användare tooa listan i Cerner Central."
services: active-directory
documentationcenter: 
author: asmalser-msft
writer: asmalser-msft
manager: stevenpo
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/26/2017
ms.author: asmalser-msft
ms.openlocfilehash: e96da98e783d24e7f34ae924824f909eead75f54
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-cerner-central-for-automatic-user-provisioning"></a>Självstudier: Konfigurera Cerner Central för automatisk Användaretablering

hello syftet med den här kursen är tooshow du hello stegen tooperform i Cerner Central och Azure AD tooautomatically etablera och avinstallation etablera användarkonton från Azure AD tooa användaren listan i Cerner Central. 


## <a name="prerequisites"></a>Krav

hello-scenario som beskrivs i den här kursen förutsätter att du redan har hello följande objekt:

*   En Azure Active Directory-klient
*   En Cerner Central-klient 

> [!NOTE]
> Azure Active Directory kan integreras med Cerner Central med hello [SCIM](http://www.simplecloud.info/) protokoll.

## <a name="assigning-users-toocerner-central"></a>Tilldela användare tooCerner Central

Azure Active Directory använder ett begrepp som kallas ”tilldelningar” toodetermine som användarna ska få åtkomst till tooselected appar. Hello gäller automatisk konto användaretablering är är bara hello användare och grupper som har ”tilldelats” tooan program i Azure AD synkroniserade. 

Innan du konfigurerar och aktiverar hello etableras, bör du bestämma vilka användare och/eller grupper i Azure AD representerar hello-användare som behöver åtkomst till tooCerner Central. När du valt, kan du tilldela dessa användare tooCerner Central genom att följa hello anvisningarna här:

[Tilldela en användare eller grupp tooan enterprise app](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-toocerner-central"></a>Viktiga tips för att tilldela användare tooCerner Central

*   Vi rekommenderar att en enda Azure AD-användare tilldelas tooCerner centrala tootest hello etablering konfiguration. Ytterligare användare och/eller grupper kan tilldelas senare.

* När det första testet är klart för en enskild användare, rekommenderar Cerner Central tilldela hello hela listan över användare som är avsedda tooaccess alla Cerner lösningen (inte bara Cerner Central) toobe etableras Toocerner's användaren listan.  Andra Cerner lösningar utnyttja den här listan över användare i listan för hello-användare.

*   När du tilldelar en användare tooCerner Central, måste du välja hello **användaren** roll i dialogrutan för hello tilldelning. Användare med hello ”standard” rollen som är undantagna från etablering.


## <a name="configuring-user-provisioning-toocerner-central"></a>Konfigurera användaretablering tooCerner Central

Det här avsnittet hjälper dig att ansluta din Azure AD tooCerner Central användaren listan med Cerner's SCIM användarkonto API-etablering och konfigurerar hello etablering service toocreate, uppdatera och inaktivera tilldelade användarbaserad konton i Cerner Central på tilldelning av användare och grupper i Azure AD.

> [!TIP]
> Du kan också välja tooenabled SAML-baserade enkel inloggning för Cerner centrala, hello instruktionerna som anges i [Azure-portalen (https://portal.azure.com). Enkel inloggning kan konfigureras oberoende av Automatisk etablering, även om dessa två funktioner kompletterar varandra. Mer information finns i hello [Cerner Central enkel inloggning kursen](active-directory-saas-cernercentral-tutorial.md).


### <a name="tooconfigure-automatic-user-account-provisioning-toocerner-central-in-azure-ad"></a>tooconfigure automatisk användarkonto etablering tooCerner Central i Azure AD:


I ordning tooprovision användaren konton tooCerner Central du behöver toorequest Cerner Central system-kontot från Cerner, och skapa en OAuth ägar-token som Azure AD kan använda tooconnect tooCerner SCIM slutpunkt. Vi rekommenderar också att hello integration utförs innan du distribuerar tooproduction i Cerner begränsat läge.

1.  hello första steget är tooensure hello personer hantera hello Cerner och Azure AD-integrering har ett CernerCare konto, som är nödvändiga tooaccess hello nödvändiga toocomplete hello instruktioner. Om du Använd hello URL: er nedan toocreate CernerCare konton i varje tillämplig miljö.

   * Sandbox: https://sandboxcernercare.com/accounts/create

   * Produktion: https://cernercare.com/accounts/create  

2.  Därefter måste en system-kontot skapas för Azure AD. Använd hello instruktionerna nedan toorequest ett systemkonto för din sandbox och produktionsmiljöer.

   * Anvisningar: https://wiki.ucern.com/display/CernerCentral/Requesting+A+System+Account

   * Sandbox: https://sandboxcernercentral.com/system-accounts/

   * Produktion: https://cernercentral.com/system-accounts/

3.  Därefter Generera en OAuth ägar-token för var och en av dina Systemkonton. toodo detta, följ hello anvisningarna nedan.

   * Anvisningar: https://wiki.ucern.com/display/public/reference/Accessing+Cerner%27s+Web+Services+Using+A+System+Account+Bearer+Token

   * Sandbox: https://sandboxcernercentral.com/system-accounts/

   * Produktion: https://cernercentral.com/system-accounts/

4. Slutligen måste tooacquire användaridentiteter listan sfär för båda hello sandbox och produktionsmiljöer i Cerner toocomplete hello konfiguration. Mer information om hur tooacquire detta, se: https://wiki.ucern.com/display/public/reference/Publishing+Identity+Data+Using+SCIM. 

5. Du kan nu konfigurera Azure AD tooprovision användaren konton tooCerner. Logga in toohello [Azure-portalen](https://portal.azure.com), och bläddra toohello **Azure Active Directory > Företagsappar > alla program** avsnitt.

6. Om du redan har konfigurerat Cerner Central för enkel inloggning, söka efter din instans av Cerner Central hjälp hello sökfältet. Annars väljer **Lägg till** och Sök efter **Cerner Central** i hello programgalleriet. Välj Cerner Central från hello sökresultaten och lägga till den tooyour listan med program.

7.  Välj din instans av Cerner Central och sedan hello **etablering** fliken.

8.  Ange hello **etablering läge** för**automatisk**.

   ![Cerner Central etablering](./media/active-directory-saas-cernercentral-provisioning-tutorial/Cerner.PNG)

9.  Fyll i följande fält under hello **administratörsautentiseringsuppgifter**:

   * I hello **klient URL** , ange en URL i formatet hello nedan, ersätter ”användare-listan-sfär-ID” med hello sfär ID som du har införskaffade i steg #4.

> Sandbox: https://user-roster-api.sandboxcernercentral.com/scim/v1/Realms/User-Roster-Realm-ID/ 

> Produktion: https://user-roster-api.cernercentral.com/scim/v1/Realms/User-Roster-Realm-ID/ 

   * I hello **hemlighet Token** skriver hello OAuth ägar-token som du genererade i steg #3 och klicka på **Testanslutningen**.

   * Du bör se ett meddelande om lyckade på hello upperright sida av portalen.

10. Ange hello e-postadress för en person eller grupp som ska få meddelanden om etablering fel i hello **e-postmeddelande** fält och markera kryssrutan hello nedan.

11. Klicka på **Spara**. 

12. I hello **attributmappning** avsnittet, granska hello användare och grupp attribut toobe synkroniseras från Azure AD tooCerner Central. Hej attribut som valts som **matchande** egenskaper är används toomatch hello användarkonton och grupper i Cerner Central för uppdateringsåtgärder. Välj hello spara knappen toocommit ändringar.

13. tooenable hello Azure AD-etablering tjänsten för Cerner centrala, ändra hello **Status för etablering** för**på** i hello **inställningar** avsnitt

14. Klicka på **Spara**. 

Detta startar hello inledande synkronisering av alla användare och/eller grupper som har tilldelats tooCerner Central under hello användare och grupper. hello inledande synkronisering tar längre tid tooperform än efterföljande synkroniseringar som sker ungefär var tjugonde minut så länge hello etablering Azure AD-tjänsten körs. Du kan använda hello **synkroniseringsinformation** avsnittet toomonitor förlopp och följ länkarna tooprovisioning aktivitetsrapporter, som beskriver alla åtgärder som utförs av hello etableras Cerner Central appen.

Mer information om hur tooread hello Azure AD-etablering loggar finns [rapportering om automatisk konto användaretablering](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).

## <a name="additional-resources"></a>Ytterligare resurser

* [Cerner Central: Publicering av identitetsdata med Azure AD](https://wiki.ucern.com/display/public/reference/Publishing+Identity+Data+Using+Azure+AD)
* [Självstudier: Konfigurera Cerner Central för enkel inloggning med Azure Active Directory](active-directory-saas-cernercentral-tutorial.md)
* [Hantera användare konto-etablering för företag-appar](active-directory-enterprise-apps-manage-provisioning.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)

## <a name="next-steps"></a>Nästa steg
* [Lär dig hur tooreview loggar och få rapporter om etablering aktiviteten](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).
