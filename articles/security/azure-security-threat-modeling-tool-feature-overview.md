---
title: Microsoft Threat Modeling verktyget - Azure | Microsoft Docs
description: "Lär dig mer om alla funktioner som finns i verktyget Modeling hot"
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
ms.openlocfilehash: 621ff305d7e782f85eeaae6c3fb02031673549c6
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="threat-modeling-tool-feature-overview"></a>Översikt över hot Modeling verktyget funktioner

Vi är glada över att du väljer att använda verktyget hot modellering för ditt hot modeling behov! Om du inte gjort det, gå  **[komma igång med verktyget Modeling hot](./azure-security-threat-modeling-tool-getting-started.md)**  att lära dig grunderna.

> Vårt verktyg uppdateras ofta, så kontrollera den här guiden ofta om du vill se vår senaste funktioner och förbättringar.

Klicka på knappen ”Skapa en ny modell” öppnar en tom startsida som liknar bilden nedan:

![Tom startsida](./media/azure-security-threat-modeling-tool/tmtstart.png)

Använda modellen hot som skapats av vårt team i den  **[komma igång](./azure-security-threat-modeling-tool-getting-started.md)**  exemplet ska vi ta en titt alla funktioner som finns i verktyget idag.

![Grundläggande Hotmodell](./media/azure-security-threat-modeling-tool/basictmt.png)

## <a name="navigation"></a>Navigering

Innan du dyker in de inbyggda funktionerna ska vi gå igenom huvudkomponenterna i verktyget

### <a name="menu-items"></a>Menyalternativ

Upplevelsen bör likna andra Microsoft-produkter. Vi börjar med att gå igenom de översta menyalternativen:

![Menyalternativ](./media/azure-security-threat-modeling-tool/menuitems.png)

| Etikett                               | Information      |
| --------------------------------------- | ------------ |
| **Fil** | <ul><li>Öppna, spara och stänga filer</li><li>Logga in/ut av OneDrive konton</li><li>Dela länkar (visa + redigera)</li><li>Visa filinformation</li><li>Lägga till nya mall i befintliga modeller</li></ul> |
| **Redigera** | Ångra/Gör om åtgärder, som även en kopia, klistra in och ta bort |
| **Visa** | <ul><li>Växla mellan **Analysis** och **Design** vyer</li><li>Öppna stängd windows (e.g.stencils, egenskaper och meddelanden)</li><li>Återställ layout till standardinställningarna</li></ul> |
| **Diagram** | Lägg till/ta bort diagram och navigera genom ”flikar” av diagram |
| **Rapporter** | Skapa HTML-rapporter för att dela med andra |
| **Hjälp** | Hjälper som hjälper dig att använda verktyget |

Ikonerna är genvägar i översta menyer:

| Ikon                               | Information      |
| --------------------------------------- | ------------ |
| **Öppna** | Öppnar en ny fil |
| **Spara** | Sparar aktuell fil |
| **Design** | Försätts i designvyn, där du kan skapa modeller |
| **Analysera** | Visar genererade hot och deras egenskaper |
| **Lägg till Diagram** | Lägger till nya diagram (liknar nya flikar i Excel) |
| **Ta bort Diagram** | Tar bort aktuella diagrammet |
| **Kopiera och klipp ut/klistra in** | Klistrar in-kopior/delar element |
| **Ångra/Gör om** | Ångrar/gör om åtgärder |
| **Zooma In / Zooma ut** | Zoomar in och ut diagram för en bättre vy |
| **Feedback** | Öppnar MSDN-Forum |

### <a name="canvas"></a>Arbetsytan

Utrymme som du drar och släpper element i. Dra och släpp är det snabbaste och mest effektiva sättet att bygga modeller. Du kan också högerklicka och välj på menyn som lägger till allmän versioner av element som du använder, enligt nedan.

#### <a name="dropping-the-stencil-on-the-canvas"></a>Släppa stencilen på arbetsytan

![Arbetsytan släpp](./media/azure-security-threat-modeling-tool/canvasdrop1.png)

#### <a name="clicking-on-the-stencil"></a>Klicka på stencilen

![Egenskaper](./media/azure-security-threat-modeling-tool/canvasdrop2.png)

### <a name="stencils"></a>Stenciler

Var du hittar alla stenciler som kan användas baserat på den valda mallen. Om du inte hittar rätt element, försök med en annan mall eller ändra en så att de passar dina behov. I allmänhet kan ska du kunna hitta en kombination av kategorier med de nedan:

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
| **Anteckningar** | Manuell anteckningar som lagts till i filen med teknikerna snappa under hela processen design och granska |

### <a name="element-properties"></a>Egenskaper

Dessa varierar beroende på markerade element. Förutom förtroende gränser kan innehålla alla element 3 allmänna inställningar:

| Egenskapen element                               | Information      |
| --------------------------------------- | ------------ |
| **Namn** | Användbar för att namnge din processer, Arkiv, interaktörer och flöden för att enkelt identifiera |
| **Utanför omfånget** | Om markerad hämtas elementet utanför matrisen hot generation (rekommenderas inte) |
| **Orsak till utanför omfånget** | Motiveringen fält så att användarna vet varför täcker har valts |

Egenskaper har ändrats under varje element. Klicka på varje element att granska de tillgängliga alternativen eller öppna mallen om du vill veta mer. Det är dags till funktionerna.

## <a name="welcome-screen"></a>Välkomstskärmen

Välkomstskärmen är det första som visas när du öppnar appen.

### <a name="open-a-model"></a>Öppna en modell

Hovra över knappen ”Öppna en modell” visar 2 dolda alternativ: ”Öppna från den här datorn” och ”öppna från OneDrive”. Först öppnar öppna skärmen, medan andra tar dig igenom processen inloggningen för OneDrive, så att du kan välja mappar och filer efter en lyckad autentisering.

![Öppna modellen](./media/azure-security-threat-modeling-tool/openmodel.png)

![Öppna från datorn eller OneDrive](./media/azure-security-threat-modeling-tool/openmodel2.png)

### <a name="feedback-suggestions-and-issues"></a>Feedback, förslag och problem

Det här alternativet tar dig vidare till MSDN-forum för SDL-verktyg. Det är ett bra sätt att Kolla in vad andra säger om verktyget, inklusive lösningar och nya idéer.

![Feedback](./media/azure-security-threat-modeling-tool/feedback.png)

## <a name="design-view"></a>Designvyn

När du öppnar eller skapa en ny modell, vidarebefordras du till designvyn.

### <a name="adding-elements"></a>Att lägga till element

Det finns 2 sätt att lägga till element i rutnätet:

- **Dra och släpp** – dra det önskade elementet i rutnätet och sedan använda elementegenskaper för att ange ytterligare information.
- **Högerklicka på** – högerklickar du någonstans på rutnätet och välj den nedrullningsbara menyn. En allmän representation av elementet visas på skärmen.

### <a name="connecting-elements"></a>Ansluta element

Det finns 2 sätt att ansluta element i verktyget:

- **Dra och släpp** – dra önskade dataflöde i rutnätet och ansluta båda ändarna till lämplig elementen.
- **Klicka på + SKIFT** – Klicka på det första elementet (skicka data), tryck och håll ned SKIFT och markerar sedan det andra elementet (mottagning av data). Högerklicka och Välj ”Anslut”. Ordningen är inte lika viktigt om du använder en dubbelriktad dataflöde.

### <a name="properties"></a>Egenskaper

Visar alla egenskaper som kan ändras på stencilerna placeras i diagrammet. Om du vill visa egenskaper, klicka på stencilen och informationen fylls i enlighet med detta. Exemplet nedan visar före och efter ett ”databas” stencil dras till diagrammet:

#### <a name="before"></a>Innan du

![Innan du](./media/azure-security-threat-modeling-tool/properties1.png)

#### <a name="after"></a>Efter

![Efter](./media/azure-security-threat-modeling-tool/properties2.png)

### <a name="messages"></a>Meddelanden

Om du skapar en hotmodell och glömt att ansluta dataflöden till element meddelar dig att agera i meddelandefönstret. Du kan välja att ignorera det och följ instruktionerna för att åtgärda problemet. 

![Meddelanden](./media/azure-security-threat-modeling-tool/messages.png)

### <a name="notes"></a>Anteckningar

Växla flikar från meddelanden till anteckningar kan du lägga till anteckningar i diagrammet för att avbilda dina tankar

## <a name="analysis-view"></a>Analysen

När du är klar skapar diagrammet växla till analys genom att gå till de översta menyn Inställningar och välja förstoringsglaset bredvid paletten paint.

![Analysen](./media/azure-security-threat-modeling-tool/analysisview.png)

### <a name="generated-threat-selection"></a>Val av genererade hot

När du klickar på ett hot kan du utnyttja tre unika funktioner:

| Funktion                               | Information      |
| --------------------------------------- | ------------ |
| **Läs indikator** | <p>Hot har nu markerats som läst som enkelt kan hjälpa dig att hålla reda på de objekt som du redan har gått igenom</p><p>![Läs oläst indikator](./media/azure-security-threat-modeling-tool/readmode.png)</p> |
| **Interaktion fokus** | <p>Interaktion i diagrammet som hör till det hotet är markerat</p><p>![Interaktion fokus](./media/azure-security-threat-modeling-tool/interactionfocus.png)</p> |
| **Egenskaper för hot** | <p>Mer information om hot fylls i egenskapsfönstret för hot</p><p>![Egenskaper för hot](./media/azure-security-threat-modeling-tool/threatproperties.png)</p> |

### <a name="priority-change"></a>Ändra prioritet

Ändrar prioritetsnivån för varje genererade hot ändras deras färger så att den blir lättare att identifiera hot som hög, medel och låg prioritet.

![Ändra prioritet](./media/azure-security-threat-modeling-tool/prioritychange.png)

### <a name="threat-properties-editable-fields"></a>Redigerbart fält för hot egenskaper

Som det visas i bilden ovan användare kan ändra den information som genereras av verktyget en också lägga till information i vissa fält, till exempel justering. De här fälten genereras av mallen, så att om du behöver mer information för varje hot kan du uppmanas att göra ändringar.

![Egenskaper för hot](./media/azure-security-threat-modeling-tool/threatproperties.png)

## <a name="reports"></a>Rapporter

När du är klar ändra prioriteringarna och uppdaterar statusen för varje genererade hot, du kan spara filen och/eller skriva ut rapporten genom att gå till ”rapport” och sedan ”skapa fullständiga”. Blir du ombedd att namnge rapporten och när du gör det, bör du se något som liknar bilden nedan:

![Rapport](./media/azure-security-threat-modeling-tool/report.png)

## <a name="next-steps"></a>Nästa steg

Om du vill bidra med en mall för gemenskapen, gå till vår  **[GitHub](https://github.com/Microsoft/threat-modeling-templates)**  sidan. **[Hämta](https://aka.ms/tmtpreview)**  verktyget för att starta redan idag.
