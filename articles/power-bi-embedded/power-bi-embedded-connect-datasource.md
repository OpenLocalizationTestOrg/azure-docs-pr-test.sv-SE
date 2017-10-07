---
title: "aaaMicrosoft Power BI Embedded - anslutande tooa-datakälla"
description: "Power BI Embedded, ansluta toodata källor"
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ms.assetid: 2a4caeb3-255d-4215-9554-0ca8e3568c13
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 01/06/2017
ms.author: asaxton
ms.openlocfilehash: b1aad6e638104716d90f7e1d060eefcbc9daedbc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-tooa-data-source"></a>Ansluta tooa datakällan
Med **Power BI Embedded**, du kan bädda in rapporter i din egen app. När du bäddar in en Power BI-rapport i din app hello rapporten ansluter toohello underliggande data av **importera** en kopia av hello data eller av **ansluta direkt** datakälla toohello med  **DirectQuery**.

Här följer hello skillnaderna mellan **importera** och **DirectQuery**.

| Importera | DirectQuery |
| --- | --- |
| Tabeller, kolumner *och data* importeras eller kopieras till hello rapporten dataset. toosee ändras uppstod toohello underliggande data, du måste uppdatera eller importera en fullständig, aktuell datauppsättning igen. |Endast *tabeller och kolumner* importeras eller kopieras till hello rapporten dataset. Du kan alltid visa hello aktuella data. |

Med Power BI Embedded, du kan använda DirectQuery med datakällor i molnet men inte lokala datakällor just nu.

> [!NOTE]
> hello stöds lokala Data Gateway inte med Power BI Embedded just nu. Det innebär att du inte kan använda DirectQuery med lokala datakällor.

## <a name="supported-data-sources"></a>Datakällor som stöds

**DirectQuery**
* Azure SQL-databas
* Azure SQL Data Warehouse

**Importera**

Du kan importera med hjälp av alla hello tillgängliga datakällor i Power BI Desktop. Du kommer **inte** vara kan toorefresh data i Power BI Embedded. Du måste tooupload ändringar tooyour PBIX filen tooPower BI Embedded. Detta är på grund av toono tillgänglig gateway. 

## <a name="benefits-of-using-directquery"></a>Fördelarna med att använda DirectQuery
Det finns två primära fördelar när du använder **DirectQuery**:

* **DirectQuery** kan du skapa visualiseringar över mycket stora datamängder, om det annars skulle vara unfeasible toofirst importera alla hello data.
* Underliggande data ändras kan kräva en uppdatering av data och för vissa rapporter hello måste toodisplay aktuella data kan kräva stora dataöverföringar, vilket gör att importera data unfeasible. Däremot **DirectQuery** rapporter alltid använda aktuella data.

## <a name="limitations-of-directquery"></a>Begränsningar av DirectQuery
   Det finns några begränsningar toousing **DirectQuery**:

* Alla tabeller måste komma från en enskild databas.
* Om hello frågan är alltför komplex, inträffar ett fel. tooremedy hello fel du måste refactor hello frågan så att den är mindre komplex. Om hello fråga måste vara komplex, behöver du tooimport hello data istället för att använda **DirectQuery**.
* Relationen filtrering är begränsad tooa riktning i stället för i båda riktningarna.
* Du kan inte ändra hello datatypen för en kolumn.
* Som standard begränsningar DAX-uttryck som tillåts i mått. Se [DirectQuery åtgärder och](#measures).

<a name="measures"/>

## <a name="directquery-and-measures"></a>DirectQuery och åtgärder
tooensure frågor skickas toohello underliggande datakällan har acceptabel prestanda, begränsningar gäller för åtgärder. När du använder **Power BI Desktop**, avancerade användare kan välja toobypass den här begränsningen genom att välja **fil > Alternativ och inställningar > alternativ**. I hello **alternativ** dialogrutan Välj **DirectQuery**, och välj alternativet för hello **tillåta obegränsad åtgärder i DirectQuery-läge**. När alternativet väljs, kan du använda DAX-uttryck som gäller för ett mått. Användare måste vara medvetna om; men att vissa uttryck som utför mycket bra när du importerar hello data kan resultera i mycket långsamt frågor toohello backend datakälla när i **DirectQuery** läge. 

## <a name="see-also"></a>Se även
* [Kom igång med Microsoft Power BI Embedded](power-bi-embedded-get-started.md)
* [Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)

Fler frågor? [Försök hello Power BI-communityn](http://community.powerbi.com/)

