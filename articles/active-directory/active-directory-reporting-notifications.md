---
title: aaaAzure Rapportaviseringar i Active Directory
description: "Hur toouse hello Azure Active Directory reporting meddelanden för misstänkt tecken moduler."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: ae6d4b0e-5931-4cb3-98bf-9be91b381c92
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/15/2017
ms.author: dhanyahk;markvi
ms.custom: oldportal
ms.reviewer: dhanyahk
ms.openlocfilehash: 3843c45eaf9d68e671943bfdbc7ab68933f38fbb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-reporting-notifications"></a>Rapportaviseringar i Azure Active Directory
## <a name="what-reports-generate-email-notifications"></a>Vilka rapporter Generera e-postaviseringar
E-postmeddelanden för tillfället endast hello oregelbundna logga aktivitet rapporten utlösare.

## <a name="what-is-an-irregular-sign-in"></a>Vad är ett ”oregelbundna inloggning”?
Oregelbundna inloggningar är de som har identifierats av våra maskininlärningsalgoritmer hello baserat på ett ”omöjligt att resa” villkor som kombineras med ett avvikande inloggning plats och en enhet. Detta kan tyda på att en hackare har försöker toosign in med det här kontot.

## <a name="who-receives-hello-email-notifications"></a>Vem som får e-postmeddelanden som hello?
hello e-postmeddelande skickas tooall globala administratörer som har tilldelats en Active Directory Premium-licens. tooensure det levererats, vi skicka den samt toohello administratörer alternativ e-postadress. Administratörer bör innehålla aad-alerts-noreply@mail.windowsazure.com i deras betrodda avsändare så att de inte missar hello e-post.

## <a name="how-often-are-these-emails-sent"></a>Hur ofta uppdateras dessa e-postmeddelanden skickas?
hello e-postmeddelande om 10 nya oregelbundna inloggning aktiviteter som inträffar i hello senaste 30 dagarna eller eftersom hello senaste e-postmeddelandet skickades, beroende på vilket som är mindre.

## <a name="how-do-i-access-hello-report-mentioned-in-hello-email"></a>Hur kommer jag åt hello-rapport som nämns i hello e-post?
När du klickar på hello länken kommer du att omdirigerade toohello rapportsidan inom hello klassiska Azure-portalen. I ordning tooaccess hello rapport, måste du toobe båda:

* En administratör eller medadministratör för Azure-prenumerationen
* En global administratör i hello directory, och tilldelas en Active Directory Premium-licens. Mer information finns i [Azure Active Directory-versioner](active-directory-editions.md).

## <a name="can-i-turn-off-these-emails"></a>Kan jag inaktivera dessa e-postmeddelanden?
Ja, tooturn av meddelanden relaterade tooanomalous inloggningar inom hello klassiska Azure-portalen klickar du på **konfigurera**, och välj sedan **inaktiverad** under hello **meddelanden**avsnitt.

## <a name="whats-next"></a>Nästa steg
* Är du nyfiken på vilka säkerhets-, gransknings- och aktivitet rapporter är tillgängliga? Checka ut [Azure AD-säkerhetsgrupp, granskning och aktivitetsrapporter](active-directory-view-access-usage-reports.md)
* [Komma igång med Azure Active Directory Premium](active-directory-get-started-premium.md)
* [Lägga till företagsanpassning tooyour inloggnings- och åtkomstpanel-sidor](active-directory-add-company-branding.md)

