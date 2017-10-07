---
title: aaaAssign licenser tooa grupp i Azure Active Directory | Microsoft Docs
description: "Hur tooassign licenser toousers med hjälp av Azure Active Directory-grupp licensiering"
services: active-directory
keywords: Azure AD-licensiering
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/05/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 148fe1bdd6c7f477a00c1f76bd8fa7d29c7b1f2c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="assign-licenses-toousers-by-group-membership-in-azure-active-directory"></a>Tilldela licenser toousers genom medlemskap i Azure Active Directory

Den här artikeln beskriver hur du tilldelar produktgruppen licenser tooa för användare i Azure Active Directory (Azure AD) och verifiera att de är korrekt licensierade.

I det här exemplet hello-klienten innehåller en säkerhetsgrupp med namnet **personalavdelningen**. Den här gruppen innehåller alla medlemmar i hello personalavdelningen (cirka 1 000 användare). Vill du tooassign Office 365 Enterprise E3 licenser toohello hela avdelningen. hello Yammer Enterprise-tjänst som ingår i produkten hello måste inaktiveras tillfälligt tills hello avdelning är klar toostart som använder den. Du bör också toodeploy Enterprise Mobility + Security licenser toohello samma grupp av användare.

> [!NOTE]
> Vissa Microsoft-tjänster är inte tillgängliga på alla platser. Innan en licens kan tilldelas tooa användaren, har Hej administratör toospecify hello användning Platsegenskapen på hello användare.

> För gruppen licenstilldelning ärver alla användare utan en användningsplats anges hello platsen för hello katalog. Om du har användare på flera platser rekommenderar vi att du alltid ange användningsplats som en del av din användare skapa flödet i Azure AD (t.ex. via AAD Connect-konfiguration) – som innebär att hello resultatet av licenstilldelning alltid är korrekt inte och användarna får tjänster på platser som inte tillåts.

## <a name="step-1-assign-hello-required-licenses"></a>Steg 1: Tilldela hello licenser som krävs

1. Logga in toohello [ **Azure-portalen** ](https://portal.azure.com) med ett administratörskonto. toomanage licenser, hello-kontot måste vara en global administratör roll eller användargrupp kontoadministratör.

2. Välj **fler tjänster** i hello vänstra navigeringsfönstret och välj sedan **Azure Active Directory**. Du kan lägga till det här bladet tooFavorites eller fästa den toohello portalens instrumentpanel.

3. På hello **Azure Active Directory** bladet väljer **licenser**. Då öppnas ett blad där du kan se och hantera alla licensierbart produkter i hello-klient.

4. Under **alla produkter**, Välj Office 365 Enterprise E3 och Enterprise Mobility + Security genom att välja hello produktnamn. toostart hello tilldelning, Välj **tilldela** hello överst i hello-bladet.

   ![Alla produkter, tilldela en licens](media/active-directory-licensing-group-assignment-azure-portal/all-products-assign.png)

5. På hello **tilldela licens** bladet, klickar du på **användare och grupper** tooopen hello **användare och grupper** bladet. Sök efter hello gruppnamn *personalavdelningen*hello gruppen och välj sedan vara säker på att tooconfirm genom att klicka på **Välj** längst hello hello-bladet.

   ![Välj en grupp](media/active-directory-licensing-group-assignment-azure-portal/select-a-group.png)

6. På hello **tilldela licens** bladet, klickar du på **tilldelning alternativ (valfritt)**, som visar alla serviceplaner ingår i hello två produkter som vi tidigare markerade certifikatmallen. Hitta **Yammer Enterprise** och stänga den **av** toodisable från hello produktlicensen. Bekräfta genom att klicka på **OK** längst hello **tilldelning alternativ**.

   ![Alternativ för tilldelning](media/active-directory-licensing-group-assignment-azure-portal/assignment-options.png)

7. toocomplete hello tilldelningen på hello **tilldela licens** bladet, klickar du på **tilldela** längst hello hello-bladet.

8. Ett meddelande visas i hello övre högra hörnet som visar hello status och resultatet av hello-processen. Om hello tilldelningsgrupp toohello inte gick att slutföra (t.ex, på grund av befintliga licenser i hello grupp), klickar du på hello tooview detaljer för hello felet.

Nu har vi angett en licensmall för hello personalavdelningen grupp. Bakgrunden i Azure AD har startat tooprocess alla befintliga medlemmar i gruppen. Inledande åtgärden kan ta en stund, beroende på hello aktuella storlek i hello-gruppen. I hello nästa steg ska vi beskriver hur tooverify hello processen har slutförts och avgöra om ytterligare uppmärksamhet är obligatoriska tooresolve problem.

> [!NOTE]
> Du kan starta hello samma tilldelning från en annan plats: **användare och grupper** i Azure AD. Gå för**Azure Active Directory** > **användare och grupper** > **alla grupper**. Hitta hello grupp markerar du den och gå toohello **licenser** fliken hello **tilldela** knappen ovanpå hello bladet öppnar hello licens tilldelning bladet.

## <a name="step-2-verify-that-hello-initial-assignment-has-finished"></a>Steg 2: Verifiera att hello första tilldelning är klar

1. Gå för**Azure Active Directory** > **användare och grupper** > **alla grupper**. Hitta hello **personalavdelningen** grupp som har tilldelats licenser.

2. På hello **personalavdelningen** grupp bladet väljer **licenser**. På så sätt kan du snabbt bekräfta om licenser har fullständigt tilldelats toousers och om det finns några fel som du behöver toolook till. hello följande information är tillgänglig:

   - Lista över produktlicenser som för tillfället är tilldelad toohello grupp. Välj en post tooshow hello specifika tjänster som har aktiverats och toomake ändras.

   - Status för hello senaste licens ändringar som gjorts toohello grupp (hello ändringar bearbetas eller om bearbetningen har slutförts för alla medlemmar i användare).

   - Information om användare som är i ett feltillstånd eftersom licenser inte gick att koppla toothem.

   ![Alternativ för tilldelning](media/active-directory-licensing-group-assignment-azure-portal/assignment-errors.png)

3. Se mer detaljerad information om licens bearbetning under **Azure Active Directory** > **användare och grupper** > *gruppnamn*  >  **Granskningsloggar**. Observera hello följande aktiviteter:

   - Aktiviteten: **starta tillämpa grupp baserad licens toousers**. Detta loggas när hello system hämtar hello licenstilldelning ändring hello grupp och börjar tillämpas tooall användaren medlemmar. Innehåller information om hello ändringen som gjordes.

   - Aktiviteten: **avslutar arbetet grupp baserad licens toousers**. Detta loggas när hello system har avslutat bearbetningen alla användare i hello-gruppen. Den innehåller en sammanfattning av hur många användare bearbetades och hur många användare gick inte att tilldela licenser för gruppen.

   [Det här avsnittet](./active-directory-licensing-group-advanced.md#use-audit-logs-to-monitor-group-based-licensing-activity) toolearn mer om hur granskningsloggar kan vara används tooanalyze ändringar som gjorts av gruppbaserade licensiering.

## <a name="step-3-check-for-license-problems-and-resolve-them"></a>Steg 3: Kontrollera problem med licensen och löser dem.

1. Gå för**Azure Active Directory** > **användare och grupper** > **alla grupper**, och hitta hello **personalavdelningen**grupp som har tilldelats licenser.
2. På hello **personalavdelningen** grupp bladet väljer **licenser**. hello-meddelande ovanpå hello bladet visar att det finns 10 användare inte gick att tilldela licenser till. Om du klickar på den öppnas en lista över alla användare i en licensiering feltillstånd för den här gruppen.
3. Hej **misslyckades tilldelningar** kolumnen talar om för oss att båda produktlicenser inte gick att koppla toohello användare. Hej **de främsta orsaken till felet** kolumnen innehåller hello orsaken till hello felet. I det här fallet har **i konflikt serviceplaner**.

   ![Misslyckade tilldelningar](media/active-directory-licensing-group-assignment-azure-portal/failed-assignments.png)

4. Välj en användare tooopen hello **licenser** bladet. Det här bladet visar alla licenser som är tilldelade toohello användare. I det här exemplet hello användare har hello Office 365 Enterprise E1 licens som har ärvts från hello **helskärmsläge användare** grupp. Detta står i konflikt med hello E3-licens som hello system försökte tooapply från hello **personalavdelningen** grupp. Därför har ingen hälsningspaket licenser från gruppen tilldelats toohello användare.

   ![Visa licenser för en användare](media/active-directory-licensing-group-assignment-azure-portal/user-license-view.png)

5. toosolve konflikten, ta bort hello användaren från hello **helskärmsläge användare** grupp. När Azure AD bearbetar hello ändring, hello **personalavdelningen** licenser tilldelas korrekt.

   ![Korrekt licens](media/active-directory-licensing-group-assignment-azure-portal/license-correctly-assigned.png)

## <a name="next-steps"></a>Nästa steg

toolearn hello funktioner har angetts för licenshantering via grupper, se hello följande artiklar:

* [Vad är gruppbaserade licensiering i Azure Active Directory?](active-directory-licensing-whatis-azure-portal.md)
* [Identifiera och lösa eventuella problem med licens för en grupp i Azure Active Directory](active-directory-licensing-group-problem-resolution-azure-portal.md)
* [Hur toomigrate individuella licensierade användare toogroup-baserade licensiering i Azure Active Directory](active-directory-licensing-group-migration-azure-portal.md)
* [Azure Active Directory gruppbaserade licensiering fler scenarier](active-directory-licensing-group-advanced.md)
