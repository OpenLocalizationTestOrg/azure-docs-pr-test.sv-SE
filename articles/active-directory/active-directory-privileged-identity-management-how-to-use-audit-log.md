---
title: aaaHow toouse hello granskningsloggen i Azure AD Privileged Identity Management | Microsoft Docs
description: "Lär dig hur toouse hello granskningsloggen i hello Azure Privileged Identity Management – tillägget."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 5d13a6dd-1fcb-4e76-82fb-cb2f4f0e4357
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/14/2017
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: 36987eaab9fe02c5dd7b4f4705e487299430745d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-hello-audit-log-in-pim"></a>Med hjälp av hello granskningsloggen i PIM
Du kan använda hello Privileged Identity Management (PIM) Granska loggen toosee alla hello användartilldelningar och aktiveringar inom en viss tidsperiod. Om du vill toosee hello fullständig granskningshistorik för aktiviteten i din klientorganisation, inklusive administratör, slutanvändare och synkroniseringsåtgärden, kan du använda hello [rapporter för åtkomst till och användning av Azure Active Directory.](active-directory-view-access-usage-reports.md)

## <a name="navigate-toohello-audit-log"></a>Navigera toohello granskningsloggen
Från hello [Azure-portalen](https://portal.azure.com) instrumentpanelen, väljer hello **Azure AD Privileged Identity Management** app. Därifrån kan komma åt hello granskningsloggen genom att klicka på **hantera Privilegierade roller** > **granskningshistorik** i hello PIM-instrumentpanelen.

## <a name="hello-audit-log-graph"></a>hello Granska loggen diagram
Du kan använda hello Granska loggen tooview hello Totalt antal aktiveringar, max aktiveringar per dag och genomsnittlig aktiveringar per dag i ett linjediagram.  Du kan också filtrera hello data av rollen om det finns fler än en roll i hello granskningshistorik.

Använd hello **tid**, **åtgärd**, och **rollen** knappar toosort hello loggen.

## <a name="hello-audit-log-list"></a>listan över hello granskning
hello kolumner i listan för hello granskning loggen är:

* **Begärande** -hello användare som begärde hello rollaktivering eller ändra.  Kontrollera hello Azure granskningsloggen för mer information om hello-värdet är ”Azure System”.
* **Användaren** -hello användare aktiveras eller tooa rollen.
* **Rollen** -hello rollen tilldelas eller aktiveras av hello användare.
* **Åtgärden** - hello-åtgärder som vidtas av hello begärande. Detta kan inkludera tilldelning vid ej tilldelning, aktivering eller inaktivering.
* **Tid** – när hello åtgärd har utförts.
* **Skäl till** -om text har angetts i hello orsak fältet under aktiveringen, visas här.
* **Förfallodatum** – endast relevant för aktivering av roller.

## <a name="filter-hello-audit-log"></a>Filtrera hello granskningsloggen
Du kan filtrera hello information som visas i hello granskningsloggen genom att klicka på hello **Filter** knappen.  Hej **uppdatering diagram parametrar bladet** visas.

När du anger hello filter, klickar du på **uppdatering** toofilter hello data i hello-loggen.  Om hello data inte visas direkt, uppdatera hello-sidan.

### <a name="change-hello-date-range"></a>Ändra hello datumintervall
Använd hello **idag**, **gångna veckan**, **senaste månaden**, eller **anpassad** knappar toochange hello tidsintervall för hello granskningslogg.

När du väljer hello **anpassad** knappen, får du en **från** datum och en **till** datum fältet toospecify ett datumintervall för hello logg.  Du kan ange hello datum i formatet ÅÅÅÅ-MM-DD eller klicka på hello **kalender** ikon och väljer en kalender hello datum.

### <a name="change-hello-roles-included-in-hello-log"></a>Ändra hello roller som ingår i hello-loggen
Markera eller avmarkera hello **rollen** kryssrutan nästa tooeach rollen tooinclude eller Uteslut den från hello logga.

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="next-steps"></a>Nästa steg
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

