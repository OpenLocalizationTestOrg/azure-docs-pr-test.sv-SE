---
title: "aaaManage datatillgångar i Azure Data Catalog | Microsoft Docs"
description: "hello artikeln visar hur toocontrol synlighet och ägarskapet för datatillgångar som är registrerade i Azure Data Catalog."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 623f5ed4-8da7-48f5-943a-448d0b7cba69
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: 48a634b92d7da19c32c9e551f295eec257f54f1d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-data-assets-in-azure-data-catalog"></a>Hantera datatillgångar i Azure Data Catalog
## <a name="introduction"></a>Introduktion
Azure Data Catalog är utformad för identifiering av datakällan, så att du lätt kan identifiera och förstå hello datakällor du behöver tooperform analyser och fatta beslut. Dessa funktioner för identifiering av gör hello största effekten när du och andra användare kan hitta och förstå hello mycket stort antal typer av datakällor. Med de här elementen i åtanke är hello standardbeteendet för Data Catalog för alla registrerade datakällor toobe synliga tooand kan upptäckas av alla användare i katalogen.

Data Catalog inte ge dig åtkomst till data i toohello sig själv. Dataåtkomst styrs av hello ägare hello-datakälla. Du kan identifiera datakällor och visa hello metadata som är relaterade toohello källor som är registrerade i hello-katalogen med Data Catalog.

Det kan finnas situationer, men där datakällor ska bara synliga toospecific användare eller toomembers för specifika grupper. I sådana fall kan användare bli ägare till registrerade datatillgångar hello Catalog och sedan kontrollera hello visning av hello tillgångar som de äger.

> [!NOTE]
> hello-funktioner som beskrivs i den här artikeln är endast tillgänglig i hello Standard Edition av Azure Data Catalog. hello ger Free Edition inte några funktioner för ägare och begränsa datatillgång synlighet.
>
>

## <a name="manage-ownership-of-data-assets"></a>Hantera ägarskapet för datatillgångar
Som standard är datatillgångar som är registrerade i Data Catalog oägd. Alla användare med behörighet tooaccess hello katalog kan identifiera och kommentera dessa tillgångar. Användare kan bli ägare till oägd datatillgångar och begränsar hello synligheten för hello tillgångar som de äger.

När en datatillgång i Data Catalog ägs endast användare som är godkända av hello ägare kan identifiera hello tillgången och visa dess metadata och endast hello ägare kan ta bort hello tillgång hello-katalogen.

> [!NOTE]
> Ägarskap i Data Catalog påverkar endast hello metadata som lagras i hello-katalogen. Ägarskap ger inte några behörigheter på hello underliggande data.
>
>

### <a name="take-ownership"></a>Bli ägare
Användare kan ta över ägarskapet för datatillgångar genom att välja hello **bli ägare** alternativ i hello Data Catalog-portalen. Inga särskilda behörigheter är obligatoriska tootake ägarskap för en datatillgång oägd. Alla användare kan bli ägare för en datatillgång oägd.

### <a name="add-owners-and-co-owners"></a>Lägg till ägare och Medägare
Om en datatillgång ägs redan, kan inte andra användare bara bli ägare. De måste läggas till som delägare av en befintlig ägare. Alla ägare kan lägga till fler användare eller säkerhetsgrupper som delägare.

> [!NOTE]
> Det är en bästa praxis toohave minst två personer som ägare för någon ägs datatillgång.
>
>

### <a name="remove-owners"></a>Ta bort ägare
Precis som alla tillgångens ägare kan lägga till delägare måste alla tillgångens ägare kan ta bort alla Medägare.

En tillgångsägare som tar bort honom eller sig själv som ägare kan inte längre hantera hello tillgången. Om hello tillgångens ägare tar bort honom eller sig själv som en ägare och det finns inga andra delägare, återgår hello tillgången tooan Ej ägd tillstånd.

## <a name="control-visibility"></a>Kontrollen synlighet
Datatillgång ägare kan styra hello synlighet för hello datatillgångar som de äger. toorestrict synlighet som hello standard, där alla Data Catalog användare kan identifiera och visa hello datatillgång, hello tillgångens ägare kan växla hello inställningar från **alla** för**ägare och dessa användare** i hello egenskaper för hello tillgång. Ägare kan sedan lägga till specifika användare och säkerhetsgrupper.

> [!NOTE]
> När det är möjligt ska tillgången ägarskap och synlighet behörigheter tilldelas toosecurity grupper och inte tooindividual användare.
>
>

## <a name="catalog-administrators"></a>Katalogadministratörer
Data Catalog administratörer är implicit delägare av alla tillgångar i hello-katalogen. Tillgångsinformation ägare kan inte ta bort synlighet från administratörer och administratörer kan hantera ägarskap och synlighet för alla datatillgångar i hello-katalogen.

## <a name="summary"></a>Sammanfattning
hello Data Catalog gemensamt skapade modellen toometadata och data tillgången discovery gör att alla toocontribute för användare av katalogen och identifiera. hello Standard Edition av Data Catalog är utformad för ägare och ledning toolimit hello synlighet och användning av specifika datatillgångar.
