---
title: aaaMicrosoft hot Modeling verktyget - Azure | Microsoft Docs
description: "Lär dig mer om alla hello-funktionerna i hello hot Modeling verktyget"
services: security
documentationcenter: na
author: RodSan
manager: RodSan
editor: RodSan
ms.assetid: na
ms.service: security
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: rodsan
ms.openlocfilehash: f9ad5e623e7758063084cb7fc723c5735161a846
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="threat-modeling-tool-feature-overview"></a>Översikt över hot Modeling verktyget funktioner

Vi är glada över att du har valt toouse hello hot Modeling verktyget för din hot modeling behov! Om du inte gjort det, gå  **[komma igång med hello hot Modeling verktyget](./azure-security-threat-modeling-tool-getting-started.md)**  toolearn hello grunderna.

> Vårt verktyg uppdateras ofta, så kontrollera detta leder ofta toosee vår nya funktioner och korrigeringar.

Klicka på ”Skapa en ny modell” hello öppnas en tom startsida, liknande toohello bilden nedan:

![Tom startsida](./media/azure-security-threat-modeling-tool/tmtstart.png)

Använda hello hotmodell som skapats av vårt team i hello  **[komma igång](./azure-security-threat-modeling-tool-getting-started.md)**  exemplet ska vi ta en titt alla hello-funktionerna i hello verktyget idag.

![Grundläggande Hotmodell](./media/azure-security-threat-modeling-tool/basictmt.png)

## <a name="navigation"></a>Navigering

Innan du dyker in hello inbyggda funktioner ska vi gå igenom hello huvudkomponenterna i hello-verktyget

### <a name="menu-items"></a>Menyalternativ

hello vara liknande tooother Microsoft-produkter. Vi börjar med att gå igenom hello menyobjekt:

![Menyalternativ](./media/azure-security-threat-modeling-tool/menuitems.png)

| Etikett                               | Information      |
| --------------------------------------- | ------------ |
| **Fil** | <ul><li>Öppna, spara och stänga filer</li><li>Logga in/ut av OneDrive konton</li><li>Dela länkar (visa + redigera)</li><li>Visa filinformation</li><li>Tillämpa mall tooExisting modeller</li></ul> |
| **Redigera** | Ångra/Gör om åtgärder, som även en kopia, klistra in och ta bort |
| **Visa** | <ul><li>Växla mellan **Analysis** och **Design** vyer</li><li>Öppna stängd windows (e.g.stencils, egenskaper och meddelanden)</li><li>Återställ toodefault layoutinställningarna</li></ul> |
| **Diagram** | Lägg till/ta bort diagram och navigera genom ”flikar” av diagram |
| **Rapporter** | Skapa HTML-rapporter tooshare med andra |
| **Hjälp** | Guider toohelp hello verktyget |

hello ikoner är genvägar för hello översta menyer:

| Ikon                               | Information      |
| --------------------------------------- | ------------ |
| **Öppna** | Öppnar en ny fil |
| **Spara** | Sparar aktuell fil |
| **Design** | Försätts i designvyn, där du kan skapa modeller |
| **Analysera** | Visar genererade hot och deras egenskaper |
| **Lägg till Diagram** | Lägger till nya diagram (liknande toonew flikar i Excel) |
| **Ta bort Diagram** | Tar bort aktuella diagrammet |
| **Kopiera och klipp ut/klistra in** | Klistrar in-kopior/delar element |
| **Ångra/Gör om** | Ångrar/gör om åtgärder |
| **Zooma In / Zooma ut** | Zoomar in och ut hello diagram för en bättre vy |
| **Feedback** | Öppnar hello MSDN-Forum |

### <a name="canvas"></a>Arbetsytan

hello diskutrymme där du drar och släpper element i. Dra och släpp är hello snabbaste och mest effektiva sättet toobuild modeller. Du kan också högerklicka och välj hello-menyn, som lägger till allmän versioner av hello-element som du använder, enligt nedan.

#### <a name="dropping-hello-stencil-on-hello-canvas"></a>Släppa hello stencil på hello arbetsytan

![Arbetsytan släpp](./media/azure-security-threat-modeling-tool/canvasdrop1.png)

#### <a name="clicking-on-hello-stencil"></a>Klicka på hello stencil

![Egenskaper](./media/azure-security-threat-modeling-tool/canvasdrop2.png)

### <a name="stencils"></a>Stenciler

Om du hittar alla stenciler tillgängliga toouse baserat på valda hello-mallen. Om du inte hittar rätt hello-element, försök med en annan mall eller ändra en toofit dina behov. I allmänhet bör du vara kan toofind en kombination av kategorier som hello de nedan:

| Stencilens namn                               | Information      |
| --------------------------------------- | ------------ |
| **Processen** | Program, webbläsare plugin-program, trådar, virtuella datorer |
| **Extern kontakt** | Autentiseringsproviders, webbläsare, användare, webbprogram |
| **Datalager** | Cache, lagring, Configuration-filer, databaser, registret |
| **Dataflöde** | Binär, ALPC, HTTP, HTTPS/TLS/SSL, IOCTL, IPSec, namngivna Pipe, RPC/DCOM, SMB, UDP |
| **Förtroende kantlinje/gräns** | Företagets nätverk, Internet, dator, Sandbox, användar-/ Kernel-läge |

### <a name="notesmessages"></a>Anteckningar/meddelanden

| Komponent                               | Information      |
| --------------------------------------- | ------------ |
| **Meddelanden** | Interna verktyget logik som varnar användare när det uppstår ett fel, till exempel inga data som flödar mellan element |
| **Anteckningar** | Manuell anteckningar tillagda toohello filen med engineering team hela hello design och granska process |

### <a name="element-properties"></a>Egenskaper

Dessa varierar beroende på hello-element som valts. Förutom förtroende gränser kan innehålla alla element 3 allmänna inställningar:

| Egenskapen element                               | Information      |
| --------------------------------------- | ------------ |
| **Namn** | Användbar för att namnge din processer, interaktörer och andra flöden toobe som är lätt att känna igen |
| **Utanför omfånget** | Om markerad tas hello element ur hello hot generation matris (rekommenderas inte) |
| **Orsak till utanför omfånget** | Motiveringen fältet toolet användarna vet varför täcker har valts |

Egenskaper har ändrats under varje element. Klicka på varje element tooinspect hello tillgängliga alternativ eller öppna hello mallen toolearn mer. Nu ska vi gå in hello funktioner.

## <a name="welcome-screen"></a>Välkomstskärmen

hello välkomstskärmen är hello första som visas när du öppnar hello app.

### <a name="open-a-model"></a>Öppna en modell

Hovra över knappen ”Öppna en modell” visar 2 dolda alternativ: ”Öppna från den här datorn” och ”öppna från OneDrive”. hello öppnas först öppna hello-skärmen, medan hello andra tar dig igenom hello logga i processen för OneDrive, så att du toopick mappar och filer efter en lyckad autentisering.

![Öppna modellen](./media/azure-security-threat-modeling-tool/openmodel.png)

![Öppna från datorn eller OneDrive](./media/azure-security-threat-modeling-tool/openmodel2.png)

### <a name="feedback-suggestions-and-issues"></a>Feedback, förslag och problem

Det här alternativet tar du toohello MSDN-forum för SDL-verktyg. Det är ett bra sätt toocheck reda på vad andra säger om hello verktyget, inklusive lösningar och nya idéer.

![Feedback](./media/azure-security-threat-modeling-tool/feedback.png)

## <a name="design-view"></a>Designvyn

När du öppnar eller skapa en ny modell, vidarebefordras du toohello designvyn.

### <a name="adding-elements"></a>Att lägga till element

Det finns 2 sätt tooadd element i hello rutnät:

- **Dra och släpp** – dra hello önskade elementet toohello rutnätet och sedan använda hello elementet egenskaper tooprovide ytterligare information.
- **Högerklicka på** – högerklickar du någonstans på hello rutnätet och välja från hello listrutan. En allmän representation av elementet visas på hello-skärmen.

### <a name="connecting-elements"></a>Ansluta element

Det finns 2 sätt tooconnect element i hello-verktyget:

- **Dra och släpp** – dra hello önskade dataflöde toohello rutnätet och ansluta båda ändarna toohello lämplig elementen.
- **Klicka på + SKIFT** – klickar du på hello första elementet (skicka data), tryck och håll hello SKIFT-tangenten och sedan väljer hello andra elementet (mottagning av data). Högerklicka och Välj ”Anslut”. Om du använder en dubbelriktad dataflöde är hello ordningen inte lika viktigt.

### <a name="properties"></a>Egenskaper

Visar alla hello-egenskaper som kan ändras i hello stenciler placeras i hello diagram. toosee hello egenskaper, klicka på hello stencil och hello information fylls i enlighet med detta. hello exemplet nedan visar före och efter ett ”databas” stencil dras till hello diagram:

#### <a name="before"></a>Innan du

![Innan du](./media/azure-security-threat-modeling-tool/properties1.png)

#### <a name="after"></a>Efter

![Efter](./media/azure-security-threat-modeling-tool/properties2.png)

### <a name="messages"></a>Meddelanden

Om du skapar en hotmodell och glömmer tooconnect data flödar tooelements meddelar hello meddelandefönstret tooact. Du kan välja tooignore den eller följ hello instruktioner toofix hello problemet. 

![Meddelanden](./media/azure-security-threat-modeling-tool/messages.png)

### <a name="notes"></a>Anteckningar

Växla flikar från meddelanden tooNotes tillåter du tooadd anteckningar tooyour diagram toocapture dina tankar

## <a name="analysis-view"></a>Analysen

När du är klar skapar diagrammet växla över tooanalysis vyn genom att gå toohello översta menyn Inställningar och välja hello förstoringsglas nästa toohello paint palett.

![Analysen](./media/azure-security-threat-modeling-tool/analysisview.png)

### <a name="generated-threat-selection"></a>Val av genererade hot

När du klickar på ett hot kan du utnyttja tre unika funktioner:

| Funktion                               | Information      |
| --------------------------------------- | ------------ |
| **Läs indikator** | <p>Hot har nu markerats som läst som enkelt kan hjälpa dig att hålla reda på hello-objekt som du redan har gått igenom</p><p>![Läs oläst indikator](./media/azure-security-threat-modeling-tool/readmode.png)</p> |
| **Interaktion fokus** | <p>Interaktion i hello diagram tillhörande toothat hot är markerat</p><p>![Interaktion fokus](./media/azure-security-threat-modeling-tool/interactionfocus.png)</p> |
| **Egenskaper för hot** | <p>Mer information om hello hot fylls i egenskapsfönstret för hello hot</p><p>![Egenskaper för hot](./media/azure-security-threat-modeling-tool/threatproperties.png)</p> |

### <a name="priority-change"></a>Ändra prioritet

Ändrar hello prioritetsnivån för varje genererade hot ändras deras färger toomake den enkelt tooidentify hög, medel och låg prioritet hot.

![Ändra prioritet](./media/azure-security-threat-modeling-tool/prioritychange.png)

### <a name="threat-properties-editable-fields"></a>Redigerbart fält för hot egenskaper

Som det visas i hello bilden ovan, kan användare ändra hello information som genererats av hello verktyget en också lägga till information toocertain fält, till exempel justering. Fälten genereras av hello mallen så att om du behöver mer information för varje hot du rekommenderar toomake ändringar.

![Egenskaper för hot](./media/azure-security-threat-modeling-tool/threatproperties.png)

## <a name="reports"></a>Rapporter

När du är klar ändra prioriteringarna och uppdaterar hello status för alla genererade hot, du kan spara hello-filen och/eller skriva ut rapporten genom att gå för ”rapportera” och sedan ”skapa fullständiga rapporten”. Blir du ombedd tooname hello rapporten och när du gör det, bör du se något liknande toohello bilden nedan:

![Rapport](./media/azure-security-threat-modeling-tool/report.png)

## <a name="next-steps"></a>Nästa steg

toocontribute en mall för hello community gå tooour  **[GitHub](https://github.com/Microsoft/threat-modeling-templates)**  sidan. **[Hämta](https://aka.ms/tmtpreview)**  hello verktyget tooget redan idag.
