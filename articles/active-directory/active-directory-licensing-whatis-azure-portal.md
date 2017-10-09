---
title: "aaaWhat är gruppbaserade licensiering i Azure Active Directory? | Microsoft Docs"
description: "Beskrivning av Azure Active Directory gruppbaserade licensiering, hur det fungerar och bästa praxis"
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
ms.date: 06/29/2017
ms.author: curtand
ms.reviewer: piotrci
ms.custom: H1Hack27Feb2017;it-pro
ms.openlocfilehash: 11647de6b76022cd2393751fcafc67ce671aeba6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="group-based-licensing-basics-in-azure-active-directory"></a>Gruppbaserade licensiering grunderna i Azure Active Directory

Med hjälp av Microsoft betald molntjänster, till exempel Office 365 Enterprise Mobility + Security, Dynamics CRM och andra liknande produkter kräver licenser. Dessa licenser tilldelas tooeach användare som behöver åtkomst till toothese tjänster. toomanage licenser administratörer använda en av hello hanteringsportalerna (Office eller Azure) och PowerShell-cmdlets. Azure Active Directory (AD Azure) är hello underliggande infrastruktur som stöder Identitetshantering för alla Microsoft-molntjänster. Azure AD lagrar information om tilldelning av licenstillstånd för användare.

Fram till nu kan licenser endast tilldelas på hello enskilda användarnivå som kan göra storskaliga management svårt. Till exempel tooadd eller ta bort användarlicenser som är baserat på organisatoriska förändringar som användarna kan ansluta till eller lämnar hello organisation eller en avdelning, administratör ofta måste skriva ett komplext PowerShell-skript. Det här skriptet anropar enskilda toohello Molntjänsten.

de utmaningar tooaddress nu Azure AD innehåller gruppbaserade licensiering. Du kan tilldela en eller flera licenser tooa produktgrupp. Azure AD säkerställer att hello licenser tilldelas tooall medlemmar i gruppen för hello. Alla nya medlemmar som ansluta hello gruppen tilldelas hello lämpliga licenser. När de lämnar hello gruppen bort dessa licenser. Detta eliminerar hello behovet av att automatisera licenshanteringen av via PowerShell tooreflect ändringar i hello organisation och avdelningsnivå strukturen på per användare.

## <a name="features"></a>Funktioner

Här följer hello huvuddragen i gruppbaserade licensiering:

- Licenserna kan tilldelas tooany säkerhetsgrupp i Azure AD. Säkerhetsgrupper kan vara synkroniserade lokalt, med hjälp av Azure AD Connect. Du kan också skapa säkerhetsgrupper direkt i Azure AD (kallas även endast molnbaserad grupper) eller automatiskt via hello Azure AD dynamisk grupp-funktionen.

- När en licens tilldelas tooa grupp, kan Hej administratör inaktivera en eller flera serviceplaner i hello produkten. Detta görs vanligtvis när hello organisation inte är ännu klar toostart med hjälp av en tjänst som ingår i en produkt. Hej administratör kan till exempel tilldela Office 365 tooa avdelning men tillfälligt inaktivera hello Yammer-tjänsten.

- Alla Microsoft-molntjänster som kräver användarnivå licensiering stöds. Detta omfattar alla Office 365 produkter Enterprise Mobility + Security och Dynamics CRM.

- Gruppbaserade licensiering finns för närvarande endast via [hello Azure-portalen](https://portal.azure.com). Om du använder andra hanteringsportalerna främst för användare och grupp management, till exempel hello Office 365-portalen kan du fortsätta toodo så. Men du bör använda hello Azure portal toomanage licenser på gruppnivå.

- Licens-ändringar som uppstår när ändringar i gruppmedlemskap hanteras automatiskt i Azure AD. Licens ändringar normalt effektiva minuter efter en ändring av gruppmedlemskap.

- En användare kan vara medlem i flera grupper med licens principer som har angetts. En användare kan också ha vissa licenser som direkt har tilldelats, utanför grupper. hello resulterande användartillstånd är en kombination av alla tilldelade produkt och tjänstlicenser.

- I vissa fall kan licenser inte tilldelas tooa användare. Exempelvis kanske det inte tillräckligt många tillgängliga licenser i hello-klient eller tjänster i konflikt kanske har tilldelats på hello samma tid. Administratörer har åtkomst tooinformation om användare för vilka Azure AD inte kunde fullständigt bearbeta grupp licenser. De kan sedan vidta åtgärder baserat på informationen.

- En betald eller utvärderingsversioner prenumeration på Azure AD Basic eller Premium krävs i hello klient toouse gruppbaserade licenshantering offentliga förhandsversionen.

## <a name="next-steps"></a>Nästa steg

toolearn mer information om andra scenarier för licenshantering via gruppbaserade licensiering, se:

* [Kom igång med Azure Active Directory-licenser](active-directory-licensing-get-started-azure-portal.md)
* [Tilldela licenser tooa grupp i Azure Active Directory](active-directory-licensing-group-assignment-azure-portal.md)
* [Identifiera och lösa eventuella problem med licens för en grupp i Azure Active Directory](active-directory-licensing-group-problem-resolution-azure-portal.md)
* [Hur toomigrate individuella licensierade användare toogroup-baserade licensiering i Azure Active Directory](active-directory-licensing-group-migration-azure-portal.md)
* [Azure Active Directory gruppbaserade licensiering fler scenarier](active-directory-licensing-group-advanced.md)
