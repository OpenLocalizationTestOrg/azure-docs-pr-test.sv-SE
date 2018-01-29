---
title: "Slutföra en åtkomst-granskning av medlemmar i en grupp eller användare åtkomst till ett program med Azure AD | Microsoft Docs"
description: "Lär dig hur du utför en åtkomst-granskning för medlemmar i en grupp eller användare med åtkomst till ett program i Azure Active Directory."
services: active-directory
documentationcenter: 
author: markwahl-msft
manager: mtillman
editor: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/19/2017
ms.author: billmath
ms.openlocfilehash: de853d633aa65c9f08f5e28088d5240c2e4d7fa6
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 12/11/2017
---
# <a name="complete-an-access-review-of-members-of-a-group-or-users-access-to-an-application-in-azure-ad"></a>Slutföra en åtkomst-granskning av medlemmar i en grupp eller användare åtkomst till ett program i Azure AD

Administratörer kan använda Azure Active Directory (Azure AD) att [skapa en åtkomst-granska](active-directory-azure-ad-controls-create-access-review.md) för medlemmar i grupper eller användare som är tilldelade till ett program. Azure AD skickar granskare automatiskt ett e-postmeddelande som uppmanar dem att granska åtkomst. Om en användare inte har fått ett e-postmeddelande, kan du skicka dem instruktionerna [granska din åtkomst](active-directory-azure-ad-controls-perform-access-review.md). Följ stegen i den här artikeln finns och att tillämpa resultaten när omprövningsperioden åtkomst är över eller om en administratör slutar åtkomst granska.

## <a name="view-an-access-review-in-the-azure-portal"></a>Visa en åtkomst-granskning i Azure-portalen

1. Gå till den [åtkomst går igenom sidan](https://portal.azure.com/#blade/Microsoft_AAD_ERM/DashboardBlade/)väljer **program**, och välj det program som innehåller granska åtkomstkontroll.

2. Välj **hantera**, och välj granska åtkomstkontroll. Om det finns många kontroller i programmet, kan du filtrera för kontroller för en viss typ och sortera efter deras status. Du kan också söka efter namnet på åtkomstkontroll för granskning eller visningsnamnet för ägaren som skapade den. 

## <a name="stop-a-review-that-hasnt-finished"></a>Stoppa en granskning inte har slutförts

Om granskningen inte har nått det schemalagda slutdatumet kan en administratör kan välja **stoppa** du avsluta granskningen tidigt. När du stoppar granskningen kan användarna inte längre granskas. Du kan inte starta om en granskning när den har stoppats.

## <a name="apply-the-changes"></a>Tillämpa ändringarna 

När en åtkomst-granskning är klar, eftersom den nått slutdatum eller administratör stoppats manuellt, Välj antingen **tillämpa**. Resultatet av granskningen implementeras genom att uppdatera den grupp eller ett program. Om en användares åtkomst nekades i granskningen, när en administratör väljer det här alternativet, Azure AD tar bort tilldelningen av deras medlemskap eller programmet. 

Att välja **tillämpa** inte har en effekt på en grupp som har sitt ursprung i en lokal katalog eller en dynamisk grupp. Om du vill ändra en grupp som har sitt ursprung lokalt Hämta resultaten och tillämpa ändringarna på representation av gruppen i den katalogen.

## <a name="download-the-results-of-the-review"></a>Hämta resultaten för granska

Om du vill hämta resultaten för granskningen väljer **godkännanden** och välj sedan **hämta**. Den resulterande CSV-filen kan visas i Excel eller andra program som öppnar CSV-filer.

## <a name="optional-delete-a-review"></a>Valfritt: Ta bort ett omdöme
Om du inte längre är intresserad av att granskningen kan du ta bort den. Välj **ta bort** att ta bort granskningen från Azure AD.

> [!IMPORTANT]
> Det finns ingen varning innan borttagningen sker, så Tänk på att du vill ta bort granskningen.
> 
> 

## <a name="next-steps"></a>Nästa steg

- [Hantera användare med Azure AD-åtkomstgranskningar](active-directory-azure-ad-controls-manage-user-access-with-access-reviews.md)
- [Hantera gäståtkomst med Azure AD-åtkomstgranskningar](active-directory-azure-ad-controls-manage-guest-access-with-access-reviews.md)
- [Hantera program och kontroller för Azure AD-åtkomstgranskningar](active-directory-azure-ad-controls-manage-programs-controls.md)
- [Skapa en åtkomstgranskning för medlemmar i en grupp eller åtkomst till ett program](active-directory-azure-ad-controls-create-access-review.md)
- [Skapa en åtkomstgranskning av användarna i en administrativ roll i Azure AD](active-directory-privileged-identity-management-how-to-start-security-review.md)
