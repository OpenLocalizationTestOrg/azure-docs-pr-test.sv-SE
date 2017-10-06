---
title: "aaaHow toostart en åtkomst-granskning | Microsoft Docs"
description: "Lär dig hur toocreate åtkomsten granska för privilegierade identiteter med hello Azure Privileged Identity Management-program."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 3e52b731-55f4-4c8a-ba87-9fd34033f52f
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/04/2017
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: 24feac307f77c69b5d68d6ae0623dbcb52416b01
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toostart-an-access-review-in-azure-ad-privileged-identity-management"></a>Hur toostart åtkomst finns i Azure AD Privileged Identity Management
Rolltilldelningar blir ”inaktiva” när användare privilegierad åtkomst som de inte behöver längre. Privilegierade rollen administratörer bör regelbundet granska hello roller som användarna har fått i ordning tooreduce hello risk som är kopplade till dessa inaktuella rolltilldelningar. Det här dokumentet beskriver hello steg för att starta en åtkomst-granskning i Azure AD Privileged Identity Management (PIM).

## <a name="start-an-access-review"></a>Starta en åtkomst-granskning
> [!NOTE]
> Om du inte har lagt till hello PIM programmet tooyour instrumentpanelen i hello Azure-portalen, se hello stegen i [komma igång med Azure Privileged Identity Management](active-directory-privileged-identity-management-getting-started.md)
> 
> 

Från hello PIM programmet huvudsidan finns det tre sätt toostart en åtkomst-granskning:

* **Åtkomst till granskningar** > **Lägg till**
* **Roller** > **granska** knappen
* Välj hello serverroll toobe granskat hello roller listan > **granska** knappen

Om du klickar på hello **granska** knappen hello **startar en åtkomst-granskning** bladet visas. På det här bladet du är pågående tooconfigure hello granska med ett namn och tid gränsen, Välj en roll tooreview och bestämma vem som ska utföra hello, granska.

![Starta en åtkomst-granskning – skärmbild][1]

### <a name="configure-hello-review"></a>Konfigurera hello, granska
Granska toocreate-åtkomst måste du tooname den och ange en start-och slutdatum.

![Konfigurera granskning – skärmbild][2]

Se hello tidslängd hello granska tillräckligt länge för att användare toocomplete den. Om du är klar innan hello slutdatum stoppar du alltid hello, granska tidigt.

### <a name="choose-a-role-tooreview"></a>Välj en roll tooreview
Varje granska fokuserar på endast en roll. Såvida du inte startade hello åtkomst granska från en viss roll-bladet, behöver du toochoose en roll nu.

1. Navigera för**granska rollmedlemskap**
   
    ![Granska rollmedlemskap – skärmbild][3]
2. Välj en roll i hello lista.

### <a name="decide-who-will-perform-hello-review"></a>Bestämma vem som ska utföra hello, granska
Det finns tre alternativ för att utföra en granskning. Du kan tilldela hello granska toosomeone annan toocomplete göra det själv eller du kan låta användarna granska sina egna åtkomst.

1. Navigera för**Markera granskare**
   
    ![Välj granskare – skärmbild][4]
2. Välj något av hello alternativ:
   
   * **Välj granskare**: Använd det här alternativet när du inte vet vem som behöver åtkomst. Med det här alternativet kan du tilldela hello granska tooa resursägare eller grupp manager toocomplete.
   * **Mig**: användbart om du vill toopreview hur åtkomst granskar arbete eller om du vill tooreview för personer som inte.
   * **Medlemmar Läs själva**: Använd det här alternativet toohave hello användarna granska sina egna rolltilldelningar.

### <a name="start-hello-review"></a>Starta hello granskning
Slutligen har du hello alternativet toorequire att användare måste ange en orsak till om de godkänner deras åtkomst. Om du vill lägga till en beskrivning av hello, granska och välj **starta**.

Kontrollera att du ger användarna om att det finns en åtkomst-granskning väntar på dem och visa dem [hur tooperform åtkomsten granska](active-directory-privileged-identity-management-how-to-perform-security-review.md).

## <a name="manage-hello-access-review"></a>Hantera hello åtkomst granskning
Du kan följa förloppet för hello när hello granskare Slutför granskningarna i hello Azure AD PIM-instrumentpanelen, i hello åtkomst går igenom avsnittet. Ingen behörighet kommer att ändras i hello directory förrän [hello, granska är klar](active-directory-privileged-identity-management-how-to-complete-review.md).

Tills hello omprövningsperioden är över, kan du påminna användare toocomplete granskning eller stoppa hello granska från hello åtkomst går igenom tidigt avsnittet.

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="pim-table-of-contents"></a>PIM innehållsförteckning
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-how-to-start-security-review/PIM_start_review.png
[2]: ./media/active-directory-privileged-identity-management-how-to-start-security-review/PIM_review_configure.png
[3]: ./media/active-directory-privileged-identity-management-how-to-start-security-review/PIM_review_role.png
[4]: ./media/active-directory-privileged-identity-management-how-to-start-security-review/PIM_review_reviewers.png
