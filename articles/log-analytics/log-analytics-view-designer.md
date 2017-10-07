---
title: aaaCreate vyer tooanalyze data i OMS Log Analytics | Microsoft Docs
description: "Vydesigner i logganalys kan du toocreate anpassade vyer som visas i hello OMS och Azure portal och innehålla olika visuella data i hello OMS-databasen. Den här artikeln innehåller en översikt över Vydesigner och procedurer för att skapa och redigera anpassade vyer."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: 
ms.assetid: ce41dc30-e568-43c1-97fa-81e5997c946a
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: bwren
ms.openlocfilehash: 40b4bfef50d70e4479b6cae16abfa8ec33d1a2f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-view-designer-toocreate-custom-views-in-log-analytics"></a>Använd Vydesigner toocreate anpassade vyer i logganalys
hello Vydesigner i [logganalys](log-analytics-overview.md) kan du toocreate anpassade vyer i hello OMS-konsolen som innehåller olika visuella data i hello OMS-databasen. Den här artikeln innehåller en översikt över Vydesigner och procedurer för att skapa och redigera anpassade vyer.

Andra artiklar som är tillgängliga för Vydesigner är:

* [Panelen referens](log-analytics-view-designer-tiles.md) -referens för hello inställningar för varje hello panelerna tillgängliga toouse i anpassade vyer.
* [Referens för visualisering del](log-analytics-view-designer-parts.md) -referens för hello inställningar för varje hello panelerna tillgängliga toouse i anpassade vyer.

>[!NOTE]
> Om ditt arbetsområde har uppgraderade toohello [nya Log Analytics-frågespråket](log-analytics-log-search-upgrade.md), och sedan frågor i alla visningslägen måste skrivas i hello [nya frågespråket](https://go.microsoft.com/fwlink/?linkid=856078).  Alla vyer som har skapats innan hello arbetsytan har uppgraderats kommer att automtically konverteras.

## <a name="concepts"></a>Koncept
Vyer som skapats med hello Vydesigner innehålla hello element i hello i den följande tabellen.

| En del | Beskrivning |
|:--- |:--- |
| sida vid sida |Visas på hello Log Analytics översikt huvudinstrumentpanelen.  Innehåller hello informationen i hello sammanfattning visual anpassad vy.  Olika typer av panelen ange olika visuella av poster i hello OMS-databasen.  Klicka på hello panelen tooopen hello anpassad vy. |
| Anpassad vy |Visas när hello användaren klickar på hello sida vid sida.  Innehåller en eller flera visualiseringen delar. |
| Visualiseringen delar |Visualisering av data i hello OMS databas baserat på en eller flera [logga sökningar](log-analytics-log-searches.md).  De flesta delar innehåller ett huvud som anger en hög nivå visualisering och en lista över hello sökresultaten.  Annan deltyper ange olika visuella av poster i hello OMS-databasen.  Klicka på element i hello del tooperform en logg sökning att tillhandahålla detaljerade poster. |

![Översikt över Designer](media/log-analytics-view-designer/overview.png)

## <a name="add-view-designer-tooyour-workspace"></a>Lägg till Vydesigner tooyour arbetsytan
Vydesigner är i förhandsvisning, måste du lägga till den tooyour arbetsytan genom att välja **förhandsgranskningsfunktioner** i hello **inställningar** avsnitt i hello OMS-portalen.

![Aktivera förhandsgranskning](media/log-analytics-view-designer/preview.png)

## <a name="creating-and-editing-views"></a>Skapa och redigera vyer
### <a name="create-a-new-view"></a>Skapa en ny vy
Öppna en ny vy i hello **Vydesigner** panelen i OMS hello huvudinstrumentpanelen genom att klicka på hello View Designer.

![Visa Designer panelen](media/log-analytics-view-designer/view-designer-tile.png)

### <a name="edit-an-existing-view"></a>Redigera en befintlig vy
tooedit en befintlig vy i hello Vydesigner, öppna hello vyn genom att klicka på dess panelen i OMS hello huvudinstrumentpanelen.  Klicka på hello **redigera** knappen tooopen hello vyn i hello View Designer.

![Redigera en vy](media/log-analytics-view-designer/menu-edit.png)

### <a name="clone-an-existing-view"></a>Klona en befintlig vy
När du klonar en vy, skapar en ny vy och öppnas i hello View Designer.  hello ny vy har hello samma namn som hello ursprungliga med ”kopia” tillagda toohello slutet av den.  tooclone en vy, öppna hello befintlig vy genom att klicka på dess panelen i OMS hello huvudinstrumentpanelen.  Klicka på hello **klona** knappen tooopen hello vyn i hello View Designer.

![Klona en vy](media/log-analytics-view-designer/edit-menu-clone.png)

### <a name="delete-an-existing-view"></a>Ta bort en befintlig vy
toodelete en befintlig vy öppna hello vyn genom att klicka på dess panelen i OMS hello huvudinstrumentpanelen.  Klicka på hello **redigera** tooopen hello vyn i hello View Designer och klickar på **ta bort vy**.

![Ta bort en vy](media/log-analytics-view-designer/edit-menu-delete.png)

### <a name="export-an-existing-view"></a>Exportera en befintlig vy
Du kan exportera en vy tooa JSON-fil som du kan importera till en annan arbetsyta eller använda i en [Azure Resource Manager-mall](../azure-resource-manager/resource-group-authoring-templates.md).  tooexport en befintlig vy öppna hello vyn genom att klicka på dess panelen i OMS hello huvudinstrumentpanelen.  Klicka på hello **exportera** knappen toocreate en fil i mappen hello webbläsare.  hello namnet på hello-filen kommer att hello namn hello med hello tillägget *omsview*.

![Exportera en vy](media/log-analytics-view-designer/edit-menu-export.png)

### <a name="import-an-existing-view"></a>Importera en befintlig vy
Du kan importera en *omsview* filen som du exporterade från en annan hanteringsgrupp.  tooimport en befintlig vy först skapa en ny vy.  Klicka på hello **importera** knappen och markera hello *omsview* fil.  hello konfigurationen i hello filen kopieras till hello befintlig vy.

![Exportera en vy](media/log-analytics-view-designer/edit-menu-import.png)

## <a name="working-with-view-designer"></a>Arbeta med Vydesigner
hello Vydesigner har tre rutor.  Hej **Design** rutan representerar hello anpassad vy.  När du lägger till paneler och delar från hello **kontrollen** fönstret toohello **Design** rutan som de lagts till toohello vyn.  Hej **egenskaper** hello egenskaper för hello panelen eller markerade delen visas i fönstret.

![Vydesigner](media/log-analytics-view-designer/view-designer-screenshot.png)

### <a name="configure-view-tile"></a>Konfigurera vyn sida vid sida
En anpassad vy kan ha endast en bricka.  Välj hello **panelen** fliken i hello **kontrollen** tooview hello aktuellt panelen eller välj ett alternativ.  Hej **egenskaper** hello egenskaper för hello aktuella panelen visas i fönstret.  Konfigurera hello panelen Egenskaper enligt toohello detaljerad information i hello [panelen referens](log-analytics-view-designer-tiles.md) och på **tillämpa** toosave ändringar.

### <a name="configure-visualization-parts"></a>Konfigurera visualiseringen delar
En vy kan innehålla valfritt antal visualiseringen delar.  Välj hello **visa** fliken och sedan en visualisering tillhör tooadd toohello vyn.  Hej **egenskaper** rutan visar hello egenskaper för hello valt en del.  Konfigurera hello visa egenskaper enligt toohello detaljerad information i hello [visualiseringen del referens](log-analytics-view-designer-parts.md) och på **tillämpa** toosave ändringar.

### <a name="delete-a-visualization-part"></a>Ta bort en del för visualisering
Du kan ta bort en del av visualiseringen från hello vyn genom att klicka på hello **X** i hello övre högra hörnet av hello del.

### <a name="rearrange-visualization-parts"></a>Ändra visualiseringen delar
Vyer kan bara ha en rad med visualiseringen delar.  Ordna om befintliga delar i en vy genom att klicka och dra dem tooa ny plats.

## <a name="next-steps"></a>Nästa steg
* Lägg till [paneler](log-analytics-view-designer-tiles.md) tooyour anpassad vy.
* Lägg till [visualiseringen delar](log-analytics-view-designer-parts.md) tooyour anpassad vy.
