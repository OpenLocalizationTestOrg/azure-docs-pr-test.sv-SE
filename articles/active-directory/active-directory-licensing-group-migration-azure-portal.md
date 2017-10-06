---
title: "aaaHow toomigrate enskilda licensierade användare-tooa grupp i Azure Active Directory | Microsoft Docs"
description: "Hur tooswitch från enskilda användare licenser toogroup-baserade licensiering med Azure Active Directory"
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
ms.openlocfilehash: 25e5c760b7e632ba71edde10d937fe580aa6ed35
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooadd-licensed-users-tooa-group-for-licensing-in-azure-active-directory"></a>Hur tooadd licensierad tooa användargruppen för licensiering i Azure Active Directory

Du kan ha befintliga licenser som distribuerats toousers i hello organisationer via ”direkttilldelning”; det vill säga med PowerShell-skript eller andra verktyg tooassign enskilda användarlicenser. Om du vill att toostart använder gruppbaserade licensiering toomanage licenser i din organisation, behöver du en migrering plan tooseamlessly ersätta befintliga lösningar med gruppbaserade licensiering.

hello viktigaste sak tookeep i åtanke är att du bör undvika att en situation där migrera toogroup-baserade licensiering leder till att användare tillfälligt förlorar sin aktuella tilldelade licenser. En process som kan leda till borttagningen av licenserna ska undvikas tooremove hello risken för användare att förlora åtkomst tooservices och deras data.

## <a name="recommended-migration-process"></a>Rekommenderade migreringsprocessen

1. Du har befintliga automation (till exempel PowerShell) hantera licenstilldelning och borttagning för användare. Låt den körs som är.

2. Skapa en ny grupp för licensiering (eller bestämma vilka befintliga grupper toouse) och se till att alla nödvändiga användare läggs till som medlemmar.

3. Tilldela hello krävs licenser toothose grupper. målet ska vara tooreflect hello samma licensiering tillstånd (till exempel PowerShell) för din befintliga automation tillämpar toothose användare.

4. Kontrollera att licenser har tillämpade tooall användare i dessa grupper. Detta kan göras genom att kontrollera hello bearbetningstillstånd på varje grupp och genom att kontrollera granskningsloggar.

  - Du kan plats enskilda användare genom att titta på deras licensinformation. Du ser har hello samma användarlicenser ”direkt” och ”ärvt” från grupper.

  - Du kan köra ett PowerShell-skript för[kontrollera hur licenser tilldelas toousers](active-directory-licensing-group-advanced.md#use-powershell-to-see-who-has-inherited-and-direct-licenses).

  - När hello samma produktlicensen tilldelas toohello användare både direkt och via en grupp, används endast en licens av hello användare. Inga ytterligare licenser är därför krävs tooperform migrering.

5. Kontrollera att inga licenstilldelning inte genom att kontrollera varje grupp för användare i feltillstånd. Mer information finns i [identifiera och lösa problem med licens för en grupp](active-directory-licensing-group-problem-resolution-azure-portal.md).

6. Överväg att ta bort hello ursprungliga direkt tilldelningar; Du kanske vill toodo gradvis i ”waves”, toomonitor hello resultatet för en delmängd användare först.

  Du kan också lämna hello ursprungliga direkt tilldelningar på användare, men när hello användarna lämnar deras licensierade grupper som de fortfarande behåller hello ursprungliga licens, som är kanske vill inte att du vill.

## <a name="an-example"></a>Ett exempel

Vi har en organisation med 1 000 användare. Alla användare kräver Enterprise Mobility + Security (EMS) licenser. 200 användare i hello ekonomiavdelningen och kräver Office 365 Enterprise E3 licenser. Vi har ett PowerShell-skript som körs på lokala att lägga till och ta bort licenser från användare eftersom de kommer och går. Vi vill tooreplace hello skriptet med gruppbaserade licensing så licenser hanteras automatiskt av Azure AD.

Här är vad hello migreringsprocessen kan se ut:

1. Med hjälp av hello Azure portal tilldela hello EMS-licens toohello **alla användare** i Azure AD. Tilldela hello E3 licens toohello **ekonomiavdelningen** grupp som innehåller alla användare i hello krävs.

2. Bekräfta att licenstilldelning har slutförts för alla användare för varje grupp. Gå toohello bladet för varje grupp markerar **licenser**, och kontrollera hello Bearbetningsstatus hello överst i hello **licenser** bladet.

  - Sök för ”senaste licens ändringar har tillämpade tooall användare” tooconfirm bearbetningen har slutförts.

  - Leta efter ett meddelande längst upp om någon användare för vilka licenser inte kan har tilldelats. Körde vi utanför licenser för vissa användare? Har några användare motstridiga licens-SKU: er som hindrar dem från arv grupp licenser?

3. Platsen kontrollera vissa användare tooverify som de har både hello direkt och gruppen licenser tillämpas. Gå toohello bladet för en användare väljer **licenser**, och undersöka hello tillståndet för licenser.

  - Detta är förväntat hello användartillstånd under migreringen:

      ![förväntade användartillstånd](media/active-directory-licensing-group-migration-azure-portal/expected-user-state.png)

  Det här bekräftar hello användaren har både direkt och ärvda licenser. Vi se att båda **EMS** och **E3** tilldelas.

  - Markera varje licens tooshow detaljer om hello aktiverade tjänster. Detta kan vara används toocheck om hello direkt och gruppen licenser aktivera exakt hello samma serviceplaner för hello användare.

      ![Kontrollera service-planer](media/active-directory-licensing-group-migration-azure-portal/check-service-plans.png)

4. När du har bekräftat att både direkt och gruppen licenser är likvärdiga, kan du börja ta bort direkt licenser från användare. Du kan testa detta genom att ta bort dem för enskilda användare i hello-portalen och sedan köra automation skript toohave dem bort gruppvis. Här är ett exempel på hello samma användare med hello direkt licenser tas bort via hello-portalen. Observera att hello licensen förblir oförändrat, men vi kan inte längre se direkt tilldelningar.

  ![direkt licenser tas bort](media/active-directory-licensing-group-migration-azure-portal/direct-licenses-removed.png)


## <a name="next-steps"></a>Nästa steg

toolearn mer information om andra scenarier för hantering av programvarulicenser genom grupper, läsa

* [Tilldela licenser tooa grupp i Azure Active Directory](active-directory-licensing-group-assignment-azure-portal.md)
* [Vad är gruppbaserade licensiering i Azure Active Directory?](active-directory-licensing-whatis-azure-portal.md)
* [Identifiera och lösa eventuella problem med licens för en grupp i Azure Active Directory](active-directory-licensing-group-problem-resolution-azure-portal.md)
* [Azure Active Directory gruppbaserade licensiering fler scenarier](active-directory-licensing-group-advanced.md)
