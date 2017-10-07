---
title: aaaAzure Application Insights skorstenar
description: "Lär dig hur du kan använda skorstenar toodiscover hur kunder interagerar med programmet."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: cfreeman
ms.openlocfilehash: 3a90cfd11cb193e303136504df44008ffd04a290
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="discover-how-customers-are-using-your-application-with-hello-application-insights-funnels"></a>Identifiera hur kunder använder ditt program med hello Application Insights skorstenar

Förstå kundupplevelsen är hello yttersta vikt tooyour verksamhet. Om ditt program innefattar flera faser, behöver tooknow om de flesta kunder fortskrider via hello hela processen, eller om de avslutande hello processen vid något tillfälle. hello förlopp i en serie steg i ett webbprogram kallas ”Trattens”. Du kan använda hello Application Insights skorstenar toogain insikter om dina användare och övervaka stegvisa konvertering priser. 

## <a name="get-started-with-hello-funnels-blade"></a>Kom igång med hello skorstenar bladet
hello enklaste sättet toolearn om skorstenar är toowalk även om ett exempel. hello visar följande bilder hello steg ägarna av en e-handel tar toolearn hur kunderna samverkar med deras webbprogram.  

### <a name="create-your-funnel"></a>Skapa din tratten
Innan du skapar din Trattens måste toodecide hello frågan ska tooanswer. Du kanske exempelvis vill tooknow hur många kunder visa startsidan-Klicka på en annons. I det här exemplet vill hello ägare av hello Fabrikam Fiber företagets tooknow hello procentandelen kunder som gör ett inköp när du lägger till objekt tootheir kundvagn under hello senaste månaden.

Här är hello åtgärder de toocreate sina tratten.

1. Klicka på hello-knappen på hello skorstenar bladet på nytt.
1. Välj hello tidsintervall för ”senaste månad” Hej **tidsintervall** listrutan. 
1. Välj hello **produktsidan** händelse från hello **steg 1** listrutan. 
1. Välj hello **Lägg till tooshopping kundvagn** händelse från hello **steg 2** listrutan.
1. Välj hello **Klicka på Köp** händelse från hello **steg3** listrutan.
1. Lägga till ett namn toohello Trattens och på **spara**.

hello följande bild visar hello data hello skorstenar bladet genererar. Här hello Se Fabrikam ägare att 22.7% av sina kunder som lagts till ett objekt tootheir shopping under hello föregående vecka, kundvagn slutförda hello inköp. De kan också se att 1% av hello kunder klickat på en annons innan besöker hello produktsidan och 20% av sina kunder loggas när du har slutfört sin inköp.


![Skorstenar bladet med data](./media/app-insights-understand-usage-patterns/funnel1.png)

## <a name="next-steps"></a>Nästa steg
- Lär dig mer om [användningsanalys](app-insights-usage-overview.md). 
