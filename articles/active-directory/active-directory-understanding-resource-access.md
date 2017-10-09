---
title: "aaaUnderstanding resursåtkomst i Azure | Microsoft Docs"
description: "Det här avsnittet beskriver begrepp om att använda prenumerationen administratörer toocontrol resursåtkomst i hello kompletta Azure-portalen"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
ms.assetid: 174f1706-b959-4230-9a75-bf651227ebf6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/24/2017
ms.author: curtand
ms.custom: oldportal;it-pro;
ms.openlocfilehash: 06b9c4166bdea849faae67cae3146c6c278deb97
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="understanding-resource-access-in-azure"></a>Förstå resursåtkomst i Azure
> [!IMPORTANT]
> Microsoft rekommenderar att du hanterar Azure AD med hjälp av hello [administrationscentret för Azure AD](https://aad.portal.azure.com) i hello Azure-portalen istället för att använda hello klassiska Azure-portalen som hänvisas till i den här artikeln. hello Azure-portalen innehåller [rollbaserad åtkomstkontroll](role-based-access-control-configure.md) så att Azure-resurser kan hanteras mer exakt.
> 
> 

I oktober 2013 har hello klassiska Azure-portalen och Service Management API: er integrerat med Azure Active Directory i ordning toolay hello grunden för att förbättra hello användarupplevelsen för att hantera åtkomst tooAzure resurser. Azure Active Directory innehåller redan bra funktioner, till exempel Användarhantering, lokalt katalogsynkronisering, multifaktorautentisering och åtkomstkontroll för programmet. Naturligtvis finns göras dessa också tillgängligt för att hantera Azure-resurser lineär.

Åtkomstkontroll i Azure startar ur faktureringsinformation. hello ägaren till ett Azure-konto som nås genom att besöka hello [Azure Accounts Center](https://account.windowsazure.com/subscriptions), är hello kontot Administratör (AA). Prenumerationer är en behållare för fakturering, men de fungerar som en säkerhetsgräns: varje prenumeration har en Service systemadministratörskontot (SA) som kan lägga till, ta bort och ändra Azure-resurser i den prenumerationen genom att använda hello [klassiska Azure-portalen](https://manage.windowsazure.com/). hello standard SA för en ny prenumeration är hello AA men hello AA kan ändra hello SA i hello Azure Accounts Center.

<br><br>![Azure-konton][1]

Prenumerationer har också en association med en katalog. hello directory definierar en uppsättning användare. Det kan vara användare från hello arbetet eller skolan som skapat hello katalog eller de kan vara externa användare (det vill säga Microsoft Accounts). Prenumerationer är tillgängliga för en delmängd av de directory-användare som har tilldelats som tjänsten systemadministratörskontot (SA) eller Medadministratör (CA); hello endast undantaget är att äldre skäl Microsoft Accounts (tidigare Windows Live ID) kan tilldelas som SA eller Certifikatutfärdare utan att vara närvarande i hello directory.

<br><br>![Åtkomstkontroll i Azure][2]

Funktioner i hello klassiska Azure-portalen kan SA som är inloggad med ett Account toochange hello directory som en prenumeration är kopplad till med hjälp av hello **redigera katalog** på hello **Prenumerationer** sidan i **inställningar**. Observera att den här åtgärden har effekter på hello åtkomstkontroll i den aktuella prenumerationen.

> [!NOTE]
> Hej **redigera katalog** kommandot i hello Azure klassiska portal är inte tillgängliga toousers som är inloggad med ett arbets eller skolans konto eftersom dessa konton kan logga in endast toohello directory toowhich de tillhör.
> 
> 

<br><br>![Flöde för enkel inloggning][3]

En organisation (till exempel Contoso) kommer i hello enkelt fall tillämpa fakturering och åtkomstkontroll över hello samma uppsättning prenumerationer. Hello directory är associerade toosubscriptions som ägs av en enda Azure-konto. Användarna ser två uppsättningar resurser (visas med orange färg i hello föregående bild) efter genomförd inloggning toohello klassiska Azure-portalen:

* Kataloger där användarkontot finns (källkod eller läggas till som ett huvudnamn för sekundärnyckel). Observera att hello-katalog som används för inloggning inte är relevanta toothis beräkning, så att dina kataloger visas alltid oavsett om du har loggat in.
* Resurser som är en del av prenumerationer som är associerade med hello-katalog som används för inloggning och som hello användare kan komma åt (där de är en SA eller CA).

<br><br>![Användare med flera prenumerationer och kataloger][4]

Användare med prenumerationer i flera kataloger har hello möjlighet tooswitch hello aktuell kontext för hello klassiska Azure-portalen med hjälp av hello prenumerationsfiltret. Detta resulterar i en separat inloggning tooa annan katalog under hello försättsbladen, men detta kan åstadkommas sömlöst med enkel inloggning (SSO).

Åtgärder som till exempel flytta resurser mellan prenumerationer kan vara svårare på grund av den här vyn för en katalog över prenumerationer. tooperform hello resurs överföring, kan det vara nödvändigt toofirst använda hello **redigera katalog** på hello prenumerationssidan i **inställningar** tooassociate hello prenumerationer toohello samma katalog .

## <a name="next-steps"></a>Nästa steg
* toolearn mer om hur toochange administratörer för en Azure-prenumeration, se [hur tooadd eller ändra Azure-administratörsroller](../billing/billing-add-change-azure-subscription-administrator.md)
* Mer information om hur Azure Active Directory relaterar tooyour Azure-prenumeration finns [hur Azure-prenumerationer är associerade med Azure Active Directory](active-directory-how-subscriptions-associated-directory.md)
* Mer information om hur tooassign roller i Azure AD finns [Tilldela administratörsroller i Azure Active Directory](active-directory-assign-admin-roles.md)

<!--Image references-->
[1]: ./media/active-directory-understanding-resource-access/IC707931.png
[2]: ./media/active-directory-understanding-resource-access/IC707932.png
[3]: ./media/active-directory-understanding-resource-access/IC707933.png
[4]: ./media/active-directory-understanding-resource-access/IC707934.png
