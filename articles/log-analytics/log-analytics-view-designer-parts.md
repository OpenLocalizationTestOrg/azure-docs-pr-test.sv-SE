---
title: "aaaPart referens för Vydesigner i OMS Log Analytics | Microsoft Docs"
description: "Vydesigner i logganalys kan du toocreate anpassade vyer i hello OMS-konsolen som innehåller olika visuella data i hello OMS-databasen. Den här artikeln innehåller en referens för hello inställningar för varje hello visualiseringen delar tillgängliga toouse i anpassade vyer."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: 
ms.assetid: 5718d620-b96e-4d33-8616-e127ee9379c4
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: bwren
ms.openlocfilehash: 6a19a451cf4cefd2fa5c94e6f61d812c4f820f73
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="log-analytics-view-designer-visualization-part-reference"></a>Log Analytics Vydesigner visualiseringen del referens
hello Vydesigner i logganalys kan toocreate anpassade vyer i hello OMS-konsolen som innehåller olika visuella data från hello OMS-databasen. Den här artikeln innehåller en referens för hello inställningar för varje hello visualiseringen delar tillgängliga toouse i anpassade vyer.

Andra artiklar som är tillgängliga för Vydesigner är:

* [Visa Designer](log-analytics-view-designer.md) -översikt över hello Vydesigner och procedurer för att skapa och redigera anpassade vyer.
* [Panelen referens](log-analytics-view-designer-tiles.md) -referens för hello inställningar för varje hello panelerna tillgängliga toouse i anpassade vyer.

>[!NOTE]
> Om ditt arbetsområde har uppgraderade toohello [nya Log Analytics-frågespråket](log-analytics-log-search-upgrade.md), och sedan frågor i alla visningslägen måste skrivas i hello [nya frågespråket](https://go.microsoft.com/fwlink/?linkid=856078).  Alla vyer som har skapats innan hello arbetsytan har uppgraderats kommer att automtically konverteras.

hello beskrivs följande tabell hello olika typer av paneler som är tillgängliga i hello View Designer.  hello avsnitten nedan beskrivs varje typ av panelen i detalj och deras egenskaper.

| Typ av vy | Beskrivning |
|:--- |:--- |
| [Listan med frågor](#list-of-queries-part) |Visar en lista över loggen sökfrågor.  hello användaren kan klicka på varje fråga toodisplay resultaten. |
| [Antal & lista](#number-amp-list-part) |Rubriken har enda antalet visar antalet poster från en sökfråga för loggen.  Listan visar hello översta tio resultat från en fråga med ett diagram som visar hello relativa värdet för en numerisk kolumn eller dess ändra över tid. |
| [Två siffror & lista](#two-numbers-amp-list-part) |Rubriken har två tal visar antalet poster från separat loggen sökfrågor.  Listan visar hello översta tio resultat från en fråga med ett diagram som visar hello relativa värdet för en numerisk kolumn eller dess ändra över tid. |
| [Ring & lista](#donut-amp-list-part) |Huvudet visar ett tal sammanfattats från en med en kolumn i en fråga i loggen.  hello ringen visar grafiskt resultatet av hello översta tre poster. |
| [Två tidslinjer & lista](#two-timelines-amp-list-part) |Huvudet visar hello resultatet av två loggen frågor när kolumndiagram med en uppmaning visas ett tal sammanfattats från en med en kolumn i en fråga i loggen.  Listan visar hello översta tio resultat från en fråga med ett diagram som visar hello relativa värdet för en numerisk kolumn eller dess ändra över tid. |
| [Information](#information-part) |Huvudet visar statisk text och en valfri länk.  Listan visar ett eller flera objekt med statisk text och rubrik. |
| [Linjediagram, callout & lista](#line-chart-callout-amp-list-part) |Huvudet visar ett linjediagram med flera serier från en fråga logg över tid och en kommentar med ett summerat värde.  Listan visar hello översta tio resultat från en fråga med ett diagram som visar hello relativa värdet för en numerisk kolumn eller dess ändra över tid. |
| [Linjediagram & lista](#line-chart-amp-list-part) |Huvudet visar ett linjediagram med flera serier från en fråga logg över tid.  Listan visar hello översta tio resultat från en fråga med ett diagram som visar hello relativa värdet för en numerisk kolumn eller dess ändra över tid. |
| [Stacken för rad diagram del](#stack-of-line-charts-part) |Visar tre separata linjediagram med flera serier från en fråga logg över tid. |

## <a name="list-of-queries-part"></a>Lista över en del frågor
Visar en lista över loggen sökfrågor.  hello användaren kan klicka på varje fråga toodisplay resultaten.  hello-vyn innehåller en enda fråga som standard och du kan klicka på **+ frågan** tooadd ytterligare frågor.

![Lista över frågor vy](media/log-analytics-view-designer/view-list-queries.png)

| Inställning | Beskrivning |
|:--- |:--- |
| **Allmänt** | |
| Rubrik |Text toodisplay hello överst i hello vyn. |
| Ny grupp |Välj toocreate en ny grupp i hello vyn från hello aktuell vy. |
| Förvald filter |Kommaavgränsad lista över egenskaper tooinclude i hello vänstra filterfönstret när hello användare väljer en fråga. |
| Återge läge |Inledande vyn som visas när hello fråga är markerad.  hello användaren kan välja alla vyer som är tillgängliga när du har öppnat hello frågan. |
| **Frågor** | |
| Sökfråga |Fråga toorun. |
| Eget namn |Beskrivande namn på hello frågan toodisplay toohello användare. |

## <a name="number--list-part"></a>Antal & lista del
Rubriken har enda antalet visar antalet poster från en sökfråga för loggen.  Listan visar hello översta tio resultat från en fråga med ett diagram som visar hello relativa värdet för en numerisk kolumn eller dess ändra över tid.

![Lista över frågor vy](media/log-analytics-view-designer/view-number-list.png)

| Inställning | Beskrivning |
|:--- |:--- |
| **Allmänt** | |
| Gruppnamn |Text toodisplay hello överst i hello vyn. |
| Ny grupp |Välj toocreate en ny grupp i hello vyn från hello aktuell vy. |
| Ikon |Bild filen toodisplay nästa toohello resultat i hello-huvudet. |
| Använd ikon |Välj toohave hello ikon visas. |
| **Rubrik** | |
| Förklaring |Text toodisplay hello överst i hello-huvud. |
| Fråga |Fråga toorun hello-huvud.  hello antal hello antalet poster som returneras av frågan hello visas. |
| **Lista** | |
| Fråga |Fråga toorun hello lista.  Hej först två egenskaper för hello första tio poster i hello resultaten ska visas.  första hello-egenskapen ska vara ett värde och hello andra textegenskap ett numeriskt värde.  Staplar skapas automatiskt utifrån hello relativa värdet för hello numerisk kolumn.<br><br>Kommandot hello sortering i hello frågan toosort hello poster i hello-listan.  hello användaren kan klicka på Visa alla toorun hello fråga och returnerar alla poster. |
| Dölj diagram |Välj toodisable hello diagram toohello höger för hello numerisk kolumn. |
| Aktivera miniatyrdiagram |Välj toodisplay miniatyrdiagram i stället för vågräta fältet.  Se [gemensamma inställningar för](#sparklines) mer information. |
| Färg |Färgen på hello staplar miniatyrdiagram. |
| Namn och Värdesavgränsare |Enskild avgränsaren om du vill tooparse hello text-egenskap i flera värden.  Se [gemensamma inställningar för](#name-value-separator) mer information. |
| Navigering fråga |Fråga toorun när hello användare väljer ett objekt i hello-listan.  Se [gemensamma inställningar för](#navigation-query) mer information. |
| **Lista** |**> Kolumnrubriker** |
| Namn |Text toodisplay hello överst i hello första kolumnen i hello-listan. |
| Värde |Text toodisplay hello överst i hello andra kolumnen i hello-listan. |
| **Lista** |**> Tröskelvärden** |
| Aktivera tröskelvärden |Välj tooenable tröskelvärden.  Se [gemensamma inställningar för](#thresholds) mer information. |

## <a name="two-numbers--list-part"></a>Två tal s & lista
Rubriken har två tal visar antalet poster från separat loggen sökfrågor.  Listan visar hello översta tio resultat från en fråga med ett diagram som visar hello relativa värdet för en numerisk kolumn eller dess ändra över tid.

![Två tal & listvy](media/log-analytics-view-designer/view-two-numbers-list.png)

| Inställning | Beskrivning |
|:--- |:--- |
| **Allmänt** | |
| Gruppnamn |Text toodisplay hello överst i hello vyn. |
| Ny grupp |Välj toocreate en ny grupp i hello vyn från hello aktuell vy. |
| Ikon |Bild filen toodisplay nästa toohello resultat i hello-huvudet. |
| Använd ikon |Välj toohave hello ikon visas. |
| **Rubrik** | |
| Förklaring |Text toodisplay hello överst i hello-huvud. |
| Fråga |Fråga toorun hello-huvud.  hello antal hello antalet poster som returneras av frågan hello visas. |
| **Lista** | |
| Fråga |Fråga toorun hello lista.  Hej först två egenskaper för hello första tio poster i hello resultaten ska visas.  första hello-egenskapen ska vara ett värde och hello andra textegenskap ett numeriskt värde.  Staplar skapas automatiskt utifrån hello relativa värdet för hello numerisk kolumn.<br><br>Kommandot hello sortering i hello frågan toosort hello poster i hello-listan.  hello användaren kan klicka på Visa alla toorun hello fråga och returnerar alla poster. |
| Dölj diagram |Välj toodisable hello diagram toohello höger för hello numerisk kolumn. |
| Aktivera miniatyrdiagram |Välj toodisplay miniatyrdiagram i stället för vågräta fältet.  Se [gemensamma inställningar för](#sparklines) mer information. |
| Färg |Färgen på hello staplar miniatyrdiagram. |
| Åtgärd |Åtgärden tooperform för hello miniatyrdiagram.  Se [gemensamma inställningar för](#sparklines) mer information. |
| Namn och Värdesavgränsare |Enskild avgränsaren om du vill tooparse hello text-egenskap i flera värden.  Se [gemensamma inställningar för](#name-value-separator) mer information. |
| Navigering fråga |Fråga toorun när hello användare väljer ett objekt i hello-listan.  Se [gemensamma inställningar för](#navigation-query) mer information. |
| **Lista** |**> Kolumnrubriker** |
| Namn |Text toodisplay hello överst i hello första kolumnen i hello-listan. |
| Värde |Text toodisplay hello överst i hello andra kolumnen i hello-listan. |
| **Lista** |**> Tröskelvärden** |
| Aktivera tröskelvärden |Välj tooenable tröskelvärden.  Se [gemensamma inställningar för](#thresholds) mer information. |

## <a name="donut--list-part"></a>En del ringen & lista
Huvudet visar ett tal sammanfattats från en med en kolumn i en fråga i loggen.  hello ringen visar grafiskt resultatet av hello översta tre poster.

![Ring & lista vy](media/log-analytics-view-designer/view-donut-list.png)

| Inställning | Beskrivning |
|:--- |:--- |
| **Allmänt** | |
| Gruppnamn |Text toodisplay hello överst i hello panelen. |
| Ny grupp |Välj toocreate en ny grupp i hello vyn från hello aktuell vy. |
| Ikon |Bild filen toodisplay nästa toohello resultat i hello-huvudet. |
| Använd ikon |Välj toohave hello ikon visas. |
| **Huvudet** | |
| Rubrik |Text toodisplay hello överst i hello-huvud. |
| Underrubrik |Text toodisplay under hello rubrik överst hello i hello-huvud. |
| **Ring** | |
| Fråga |Fråga toorun för hello ringen.  första hello-egenskapen ska vara ett värde och hello andra textegenskap ett numeriskt värde. |
| **Ring** |**> Center** |
| Text |Text toodisplay under hello värde i hello ringen. |
| Åtgärd |hello åtgärden tooperform på hello värdet toosummarize tooa enda egenskapsvärde.<br><br>-Summan: Lägga till hello värden för alla poster.<br>-Procent: Procentandelen hello-poster som returneras av hello värden i **leda till att värden som används i center åtgärden** toohello Totalt antal poster i hello-frågan. |
| Resultatvärden som används i center åtgärd |Du kan också klicka på hello plustecknet tooadd ett eller flera värden.  hello resultaten av hello frågan kommer att vara begränsade toorecords med hello egenskapsvärden som du anger.  Om inga värden läggs till, med alla poster i hello frågan. |
| **Ytterligare alternativ** |**> Färger** |
| Färg 1<br>Färg 2<br>Färg 3 |Välj hello färg för hello hello värden visas i hello ringen. |
| **Ytterligare alternativ** |**> Avancerade färgmappning** |
| Fältvärde |Ange hello namnet på ett fält toodisplay den som en annan färg om den ingår i hello ringen. |
| Färg |Välj hello färg för hello unikt fält. |
| **Lista** | |
| Fråga |Fråga toorun hello lista.  hello antal hello antalet poster som returneras av frågan hello visas. |
| Dölj diagram |Välj toodisable hello diagram toohello höger för hello numerisk kolumn. |
| Aktivera miniatyrdiagram |Välj toodisplay miniatyrdiagram i stället för vågräta fältet.  Se [gemensamma inställningar för](#sparklines) mer information. |
| Färg |Färgen på hello staplar miniatyrdiagram. |
| Åtgärd |Åtgärden tooperform för hello miniatyrdiagram.  Se [gemensamma inställningar för](#sparklines) mer information. |
| Namn och Värdesavgränsare |Enskild avgränsaren om du vill tooparse hello text-egenskap i flera värden.  Se [gemensamma inställningar för](#name-value-separator) mer information. |
| Navigering fråga |Fråga toorun när hello användare väljer ett objekt i hello-listan.  Se [gemensamma inställningar för](#navigation-query) mer information. |
| **Lista** |**> Kolumnrubriker** |
| Namn |Text toodisplay hello överst i hello första kolumnen i hello-listan. |
| Värde |Text toodisplay hello överst i hello andra kolumnen i hello-listan. |
| **Lista** |**> Tröskelvärden** |
| Aktivera tröskelvärden |Välj tooenable tröskelvärden.  Se [gemensamma inställningar för](#thresholds) mer information. |

## <a name="two-timelines--list-part"></a>Två tidslinjer & lista del
Huvudet visar hello resultatet av två loggen frågor när kolumndiagram med en uppmaning visas ett tal sammanfattats från en med en kolumn i en fråga i loggen.  Listan visar hello översta tio resultat från en fråga med ett diagram som visar hello relativa värdet för en numerisk kolumn eller dess ändra över tid.

![Visa två tidslinjer & lista](media/log-analytics-view-designer/view-two-timelines-list.png)

| Inställning | Beskrivning |
|:--- |:--- |
| **Allmänt** | |
| Gruppnamn |Text toodisplay hello överst i hello panelen. |
| Ny grupp |Välj toocreate en ny grupp i hello vyn från hello aktuell vy. |
| Ikon |Bild filen toodisplay nästa toohello resultat i hello-huvudet. |
| Använd ikon |Välj toohave hello ikon visas. |
| **Först diagrammets<br>andra diagrammet** | |
| Förklaring |Text toodisplay under hello callout för hello första serien. |
| Färg |Färg toouse för hello kolumner i hello-serien. |
| Fråga |Fråga toorun för hello första serien.  hello antal hello antalet poster under varje tidsintervall et representeras av hello diagram kolumner. |
| Åtgärd |hello åtgärden tooperform på hello värdet egenskapen toosummarize tooa enskilt värde för hello bildtexter.<br><br>-Summan: Summan av hello värdet från alla poster.<br>-Genomsnittlig: Medelvärdet av hello värdet från alla poster.<br>-Sista: Exempelvärde från hello senaste intervall som ingår i hello diagram.<br>-Första: Exempelvärde från hello första intervall som ingår i hello diagram.<br>-Antal: Antalet poster som returneras av hello frågan. |
| **Lista** | |
| Fråga |Fråga toorun hello lista.  hello antal hello antalet poster som returneras av frågan hello visas. |
| Dölj diagram |Välj toodisable hello diagram toohello höger för hello numerisk kolumn. |
| Aktivera miniatyrdiagram |Välj toodisplay miniatyrdiagram i stället för vågräta fältet.  Se [gemensamma inställningar för](#sparklines) mer information. |
| Färg |Färgen på hello staplar miniatyrdiagram. |
| Åtgärd |Åtgärden tooperform för hello miniatyrdiagram.  Se [gemensamma inställningar för](#sparklines) mer information. |
| Navigering fråga |Fråga toorun när hello användare väljer ett objekt i hello-listan.  Se [gemensamma inställningar för](#navigation-query) mer information. |
| **Lista** |**> Kolumnrubriker** |
| Namn |Text toodisplay hello överst i hello första kolumnen i hello-listan. |
| Värde |Text toodisplay hello överst i hello andra kolumnen i hello-listan. |
| **Lista** |**> Tröskelvärden** |
| Aktivera tröskelvärden |Välj tooenable tröskelvärden.  Se [gemensamma inställningar för](#thresholds) mer information. |

## <a name="information-part"></a>En del information
Huvudet visar statisk text och en valfri länk.  Listan visar ett eller flera objekt med statisk text och rubrik.

![Visa information](media/log-analytics-view-designer/view-information.png)

| Inställning | Beskrivning |
|:--- |:--- |
| **Allmänt** | |
| Gruppnamn |Text toodisplay hello överst i hello panelen. |
| Ny grupp |Välj toocreate en ny grupp i hello vyn från hello aktuell vy. |
| Färg |Bakgrundsfärgen för hello-huvudet. |
| **Huvudet** | |
| Bild |Bild filen toodisplay i hello-huvudet. |
| Etikett |Texten toodisplay i hello-huvudet. |
| **Huvudet** |**> Länk** |
| Etikett |Länktext. |
| URL |URL för länken. |
| **Information som** | |
| Rubrik |Text toodisplay rubriken för varje objekt. |
| Innehåll |Text toodisplay för varje objekt. |

## <a name="line-chart-callout--list-part"></a>Linjediagram, callout & lista del
Huvudet visar ett linjediagram med flera serier från en fråga logg över tid och en kommentar med ett summerat värde.  Listan visar hello översta tio resultat från en fråga med ett diagram som visar hello relativa värdet för en numerisk kolumn eller dess ändra över tid.

![Linjediagram, callout & listvy](media/log-analytics-view-designer/view-line-chart-callout-list.png)

| Inställning | Beskrivning |
|:--- |:--- |
| **Allmänt** | |
| Gruppnamn |Text toodisplay hello överst i hello panelen. |
| Ny grupp |Välj toocreate en ny grupp i hello vyn från hello aktuell vy. |
| Ikon |Bild filen toodisplay nästa toohello resultat i hello-huvudet. |
| Använd ikon |Välj toohave hello ikon visas. |
| **Huvudet** | |
| Rubrik |Text toodisplay hello överst i hello-huvud. |
| Underrubrik |Text toodisplay under hello rubrik överst hello i hello-huvud. |
| **Linjediagram** | |
| Fråga |Fråga toorun hello linjediagram.  första hello-egenskapen ska vara ett värde och hello andra textegenskap ett numeriskt värde.  Detta är vanligtvis en fråga som använder hello **mått** nyckelordet toosummarize resultat.  Om hello frågan använder hello **intervall** nyckelordet sedan hello x-axeln i diagrammet hello kommer att använda detta tidsintervall.  Om hello frågan inte innehåller hello **intervall** nyckelord och varje timme sedan intervall används för hello x-axeln. |
| **Linjediagram** |**> Callout** |
| Callout-rubrik |Text toodisplay över hello callout värde. |
| Serienamn |Egenskapsvärdet för hello serien toouse för hello callout-värdet.  Om inga serier anges, används alla poster från hello fråga. |
| Åtgärd |hello åtgärden tooperform på hello värdet egenskapen toosummarize tooa enskilt värde för hello bildtexter.<br><br>-Genomsnittlig: Medelvärdet av hello värdet från alla poster.<br>-Räkna antalet poster som returneras av hello frågan.<br>-Sista: Exempelvärde från hello senaste intervall som ingår i hello diagram.<br>-Max: Maximalt värde från hello-intervall som ingår i hello diagram.<br>-Min: Minsta värde från hello-intervall som ingår i hello diagram.<br>-Summan: Summan av hello värdet från alla poster. |
| **Linjediagram** |**> Y-axeln** |
| Använd en logaritmisk skala |Välj toouse en logaritmisk skala för hello y-axeln. |
| Enheter |Ange hello enheter för hello-värden som returneras av hello frågan.  Den här informationen kan använda toodisplay etiketter i hello diagram som visar hello värdetyper och eventuellt för konvertering av hello värden.  hello enhetstyp anger hello hello enhet och definierar hello aktuella enhetstypen värden som är tillgängliga.  Om du väljer ett värde i konvertera toothen hello numeriska konverteras värden från hello aktuell enhet typen toohello konvertera tootype. |
| Anpassad etikett |Text toodisplay för hello Y-axeln nästa toohello etikett för hello enhetstyp.  Om ingen etikett anges visas endast hello enhetstyp. |
| **Lista** | |
| Fråga |Fråga toorun hello lista.  hello antal hello antalet poster som returneras av frågan hello visas. |
| Dölj diagram |Välj toodisable hello diagram toohello höger för hello numerisk kolumn. |
| Aktivera miniatyrdiagram |Välj toodisplay miniatyrdiagram i stället för vågräta fältet.  Se [gemensamma inställningar för](#sparklines) mer information. |
| Färg |Färgen på hello staplar miniatyrdiagram. |
| Åtgärd |Åtgärden tooperform för hello miniatyrdiagram.  Se [gemensamma inställningar för](#sparklines) mer information. |
| Namn och Värdesavgränsare |Enskild avgränsaren om du vill tooparse hello text-egenskap i flera värden.  Se [gemensamma inställningar för](#name-value-separator) mer information. |
| Navigering fråga |Fråga toorun när hello användare väljer ett objekt i hello-listan.  Se [gemensamma inställningar för](#navigation-query) mer information. |
| **Lista** |**> Kolumnrubriker** |
| Namn |Text toodisplay hello överst i hello första kolumnen i hello-listan. |
| Värde |Text toodisplay hello överst i hello andra kolumnen i hello-listan. |
| **Lista** |**> Tröskelvärden** |
| Aktivera tröskelvärden |Välj tooenable tröskelvärden.  Se [gemensamma inställningar för](#thresholds) mer information. |

## <a name="line-chart--list-part"></a>En del diagram & lista av raden
Huvudet visar ett linjediagram med flera serier från en fråga logg över tid.  Listan visar hello översta tio resultat från en fråga med ett diagram som visar hello relativa värdet för en numerisk kolumn eller dess ändra över tid.

![Vy för rad diagram & lista](media/log-analytics-view-designer/view-line-chart-callout-list.png)

| Inställning | Beskrivning |
|:--- |:--- |
| **Allmänt** | |
| Gruppnamn |Text toodisplay hello överst i hello panelen. |
| Ny grupp |Välj toocreate en ny grupp i hello vyn från hello aktuell vy. |
| Ikon |Bild filen toodisplay nästa toohello resultat i hello-huvudet. |
| Använd ikon |Välj toohave hello ikon visas. |
| **Huvudet** | |
| Rubrik |Text toodisplay hello överst i hello-huvud. |
| Underrubrik |Text toodisplay under hello rubrik överst hello i hello-huvud. |
| **Linjediagram** | |
| Fråga |Fråga toorun hello linjediagram.  första hello-egenskapen ska vara ett värde och hello andra textegenskap ett numeriskt värde.  Detta är vanligtvis en fråga som använder hello **mått** nyckelordet toosummarize resultat.  Om hello frågan använder hello **intervall** nyckelordet sedan hello x-axeln i diagrammet hello kommer att använda detta tidsintervall.  Om hello frågan inte innehåller hello **intervall** nyckelord och varje timme sedan intervall används för hello x-axeln. |
| **Linjediagram** |**> Y-axeln** |
| Använd en logaritmisk skala |Välj toouse en logaritmisk skala för hello y-axeln. |
| Enheter |Ange hello enheter för hello-värden som returneras av hello frågan.  Den här informationen kan använda toodisplay etiketter i hello diagram som visar hello värdetyper och eventuellt för konvertering av hello värden.  hello enhetstyp anger hello hello enhet och definierar hello aktuella enhetstypen värden som är tillgängliga.  Om du väljer ett värde i konvertera toothen hello numeriska konverteras värden från hello aktuell enhet typen toohello konvertera tootype. |
| Anpassad etikett |Text toodisplay för hello Y-axeln nästa toohello etikett för hello enhetstyp.  Om ingen etikett anges visas endast hello enhetstyp. |
| **Lista** | |
| Fråga |Fråga toorun hello lista.  hello antal hello antalet poster som returneras av frågan hello visas. |
| Dölj diagram |Välj toodisable hello diagram toohello höger för hello numerisk kolumn. |
| Aktivera miniatyrdiagram |Välj toodisplay miniatyrdiagram i stället för vågräta fältet.  Se [gemensamma inställningar för](#sparklines) mer information. |
| Färg |Färgen på hello staplar miniatyrdiagram. |
| Åtgärd |Åtgärden tooperform för hello miniatyrdiagram.  Se [gemensamma inställningar för](#sparklines) mer information. |
| Namn och Värdesavgränsare |Enskild avgränsaren om du vill tooparse hello text-egenskap i flera värden.  Se [gemensamma inställningar för](#name-value-separator) mer information. |
| Navigering fråga |Fråga toorun när hello användare väljer ett objekt i hello-listan.  Se [gemensamma inställningar för](#navigation-query) mer information. |
| **Lista** |**> Kolumnrubriker** |
| Namn |Text toodisplay hello överst i hello första kolumnen i hello-listan. |
| Värde |Text toodisplay hello överst i hello andra kolumnen i hello-listan. |
| **Lista** |**> Tröskelvärden** |
| Aktivera tröskelvärden |Välj tooenable tröskelvärden.  Se [gemensamma inställningar för](#thresholds) mer information. |

## <a name="stack-of-line-charts-part"></a>Stacken för rad diagram del
Visar tre separata linjediagram med flera serier från en fråga logg över tid.

![Stack för linjediagram](media/log-analytics-view-designer/view-stack-line-charts.png)

| Inställning | Beskrivning |
|:--- |:--- |
| **Allmänt** | |
| Gruppnamn |Text toodisplay hello överst i hello panelen. |
| Ny grupp |Välj toocreate en ny grupp i hello vyn från hello aktuell vy. |
| Ikon |Bild filen toodisplay nästa toohello resultat i hello-huvudet. |
| **Diagram 1<br>diagram 2<br>diagram 3** |**> Sidhuvud** |
| Rubrik |Text toodisplay hello överst i hello diagram. |
| Underrubrik |Text toodisplay under hello rubrik överst hello i hello diagram. |
| **Diagram 1<br>diagram 2<br>diagram 3** |**Linjediagram** |
| Fråga |Fråga toorun hello linjediagram.  första hello-egenskapen ska vara ett värde och hello andra textegenskap ett numeriskt värde.  Detta är vanligtvis en fråga som använder hello **mått** nyckelordet toosummarize resultat.  Om hello frågan använder hello **intervall** nyckelordet sedan hello x-axeln i diagrammet hello kommer att använda detta tidsintervall.  Om hello frågan inte innehåller hello **intervall** nyckelord och varje timme sedan intervall används för hello x-axeln. |
| **Diagrammet** |**> Y-axeln** |
| Använd en logaritmisk skala |Välj toouse en logaritmisk skala för hello y-axeln. |
| Enheter |Ange hello enheter för hello-värden som returneras av hello frågan.  Den här informationen kan använda toodisplay etiketter i hello diagram som visar hello värdetyper och eventuellt för konvertering av hello värden.  hello enhetstyp anger hello hello enhet och definierar hello aktuella enhetstypen värden som är tillgängliga.  Om du väljer ett värde i konvertera toothen hello numeriska konverteras värden från hello aktuell enhet typen toohello konvertera tootype. |
| Anpassad etikett |Text toodisplay för hello Y-axeln nästa toohello etikett för hello enhetstyp.  Om ingen etikett anges visas endast hello enhetstyp. |

## <a name="common-settings"></a>Vanliga inställningar
hello följande avsnitt beskrivs inställningarna vanliga tooseveral visualiseringen delar.

### <a name="name-value-separator">Namn och Värdesavgränsare</a>
Enskild avgränsaren om du vill tooparse hello text-egenskap från en fråga till flera värden.  Om du anger en avgränsare, kan du ange namn för varje fält avgränsade med hello samma avgränsare i hello namn.

Anta till exempel att en egenskap som kallas *plats* som ingår värden såsom *Redmond byggnad 41* och *Bellevue Building12*.  Du kan ange – för hello namn och Värdesavgränsare och *Stad Byggnad* för hello namn.  Detta skulle parsas varje värde i två egenskaper kallas *Stad* och *byggnad*.

### <a name="navigation-query">Navigering fråga</a>
Fråga toorun när hello användare väljer ett objekt i hello-listan.  Använd *{valda objektet}* tooinclude hello syntax för objekt som hello användare har valts.

Till exempel om hello frågan har en kolumn med namnet *datorn* och hello navigering frågan är *{valda objektet}*, en fråga som *datorn = ”den här datorn”* skulle köras när hello användaren har valt en dator.  Om hello navigering query *typ = händelsen {valda objektet}* sedan hello frågan *typ = händelse datorn = ”den här datorn”* skulle köras.

### <a name="sparklines">Miniatyrdiagram</a>
Ett miniatyrdiagram är ett litet linjediagram som visar hello-värdet för en lista över tid.  För visualisering delar med en lista, kan du välja om toodisplay ett vågrätt streck som anger hello relativa värdet för en numerisk kolumn eller miniatyrdiagram som anger dess värde över tid.

hello i den följande tabellen beskrivs hello inställningarna för miniatyrdiagram.

| Inställning | Beskrivning |
|:--- |:--- |
| Aktivera miniatyrdiagram |Välj toodisplay miniatyrdiagram i stället för vågräta fältet. |
| Åtgärd |Om miniatyrdiagram är aktiverade är hello åtgärden tooperform på varje egenskap i hello toocalculate hello listvärden för hello miniatyrdiagram.<br><br>-Sista: Sista exempelvärde för hello serien under hello tidsintervall.<br>-Max: Maximalt värde för hello serien under hello tidsintervall.<br>-Min: Minsta värde för hello serien under hello tidsintervall.<br>-Summan: Summan av värdena för hello serie under hello tidsintervall.<br>-Sammanfattning: Hello använder samma mått kommando som hello fråga i hello-huvudet. |

### <a name="thresholds">Tröskelvärden</a>
Tröskelvärden för Tillåt toodisplay en ikon för färgade nästa tooeach objekt i en lista som ger dig en snabb visual indikator för objekt som överskrider ett visst värde eller faller inom ett visst intervall.  Du kan till exempel visa en grön ikon för artiklar med ett giltigt värde, gult om hello-värdet är inom ett intervall som indikerar en varning och rött om det överstiger ett felaktigt värde.

När du aktiverar tröskelvärden för en del, måste du ange en eller flera tröskelvärden.  Om hello-värdet för ett objekt är större än ett tröskelvärde och lägre än tröskelvärdet för hello nästa, används den färgen.  Om hello-objektet är större än sedan högsta tröskelvärde har som färg angetts.   

Varje uppsättning tröskelvärde har ett tröskelvärde med värdet **standard**.  Detta är hello färg anges om inga andra värden har överskridits.  Du kan lägga till eller ta bort tröskelvärden genom att klicka på hello  **+**  eller **x** knappen.

hello i den följande tabellen beskrivs hello inställningarna för gränstal.

| Inställning | Beskrivning |
|:--- |:--- |
| Aktivera tröskelvärden |Välj en färg ikonen toohello till vänster i varje värde som anger dess hälsa relativa toospecified tröskelvärden toodisplay. |
| Namn |Namnge tooidentify hello tröskelvärdet. |
| Tröskelvärde |Värdet för hello tröskelvärdet.  hello hälsa färg för varje listobjekt anges toohello färg för hello högsta tröskelvärde överskridits av hello elementvärdet.  Det finns en standard-tröskelvärde som är hello färg om inget tröskelvärden överskrids. |
| Färg |Färg för hello tröskelvärdet. |

## <a name="next-steps"></a>Nästa steg
* Lär dig mer om [logga sökningar](log-analytics-log-searches.md) toosupport hello frågor i visualiseringen delar.
