---
title: "aaaSave sökningar och PIN-kod datatillgångar i Azure Data Catalog | Microsoft Docs"
description: "Hur tooarticle färgmarkera funktioner i Azure Data Catalog för att spara datakällor och datatillgångar för senare användning."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 6bd00a81-820d-4b7c-91fa-ab09e575474c
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: 0ad0a31d4b84782fed9d80acc2734912eecd6d74
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="save-searches-and-pin-data-assets-in-azure-data-catalog"></a>Spara sökningar och PIN-kod datatillgångar i Azure Data Catalog
## <a name="introduction"></a>Introduktion
Azure Data Catalog innehåller funktioner för upptäckt av datakälla. Du kan snabbt söka och filtrera hello katalogen toolocate datakällor och förstå deras syfte, vilket gör det enklare toofind hello rätt data för hello jobbet till hands.

Men vad händer om du behöver tooregularly arbeta med hello samma data? Och vad händer om du och andra användare regelbundet bidra knowledge-toohello samma datakällor i hello katalogen? I sådana situationer med toorepeatedly problemet hello samma sökningar kan vara ineffektiv. Detta är där med hjälp av sparad sökning och Fäst datatillgångar.

## <a name="saved-searches"></a>Sparade sökningar
En sparad sökning i Data Catalog är en återanvändbar per användare Sök definition. Du kan definiera en sökning, till exempel sökvillkor, taggar och andra filter och sparar den. Du kan köra hello sparade sökningen definition igen senare tooreturn alla datatillgångar som matchar sökvillkoren.

### <a name="create-a-saved-search"></a>Skapa en sparad sökning
toocreate en sparad sökning hello följande:
1. I hello Azure Data Catalog-portalen i hello **aktuella sökningen** -fönstret klickar du på **spara**. 

    ![Aktuella Sök inställningar spara länken](./media/data-catalog-how-to-save-pin/01-save-option.png) 

2. Ange hello sökkriterier du vill tooreuse och klicka sedan på **spara**.

    ![Aktuella sökinställningar sparade söknamn](./media/data-catalog-how-to-save-pin/02-name.png)

3. När du uppmanas ange ett namn för hello sparad sökning. Välj ett beskrivande namn och som beskriver hello datatillgångar som returneras av hello sökning.

### <a name="manage-saved-searches"></a>Hantera sparade sökningar
När du har sparat en eller flera sökningar, en **sparade sökningar** alternativet visas under hello **aktuella sökningen** rutan. När hello listan är expanderad visas alla sparade sökningar.

 ![Listan över sparade sökningar](./media/data-catalog-how-to-save-pin/03-list.png)

Gör något av följande hello:

* tooexecute en sökning, markera en sparad sökning hello.

* tooview en lista med alternativ för en sparad sökning klickar du på hello ned pilen Nästa toohello Söknamn.

    ![Alternativ för att hantera sparade sökningar](./media/data-catalog-how-to-save-pin/04-managing.png)

* Välj tooenter ett nytt namn för hello sparade sökningen **Byt namn på**. hello Sök definition ändras inte.

* tooremove hello sparade sökningen från listan, Välj **ta bort**, och sedan bekräfta hello borttagning.

* toomark hello sparade sökningen som standardsökning, Välj **sparar som standard**. Om du utför en ”tom” sökning från startsidan för hello Azure Data Catalog körs standardsökningen. Dessutom hello som har markerats som hello standardsökningen visas överst hello i hello **sparade sökningar** lista.

### <a name="organizational-saved-searches"></a>Organisationens sparade sökningar
Alla användare i din organisation kan spara sökningar för eget bruk. Data Catalog administratörer kan också spara sökningar för alla användare inom hello organisation. När administratörer spara en sökning, de kan välja mellan en **resurs inom hello företag** alternativet. Om du väljer det här alternativet resurser hello sparad sökning för alla användare i hello organisation.

 ![Organisationens sparade sökningar](./media/data-catalog-how-to-save-pin/08-organizational-saved-search.png)

## <a name="pinned-data-assets"></a>Fäst datatillgångar
Du kan spara och återanvända Sök definitioner med sparade sökningar. Hej datatillgångar som returneras av hello sökningar kan ändras med tiden som hello hello katalogen ändras. När du fäster datatillgångar du explicit kan identifiera specifika data tillgångar toomake dem lättare tooaccess utan att behöva toouse en sökning.

Det är enkelt att fästa en datatillgång. tooadd hello data tooyour Fäst lista med tillgångsinformation, klickar du på hello **PIN-kod** ikon. hello ikon för hello hörn hello tillgången panelen i vyn sida vid sida för hello och i hello vänstra kolumnen i hello listvyn i hello Azure Data Catalog-portalen.

![ikon för hello datatillgång PIN-kod](./media/data-catalog-how-to-save-pin/05-pinning.png)

Det är lika enkelt att ta bort en datatillgång. Klicka bara på hello **ner** ikonen tootoggle hello inställning för hello markerade tillgången.

![hello datatillgång ner ikon](./media/data-catalog-how-to-save-pin/06-unpinning.png)

## <a name="hello-my-assets-section"></a>hello avsnittet Mina tillgångar
hello Data Catalog portalens startsida innehåller en **Mina tillgångar** avsnitt som visar tillgångar intresse toohello aktuella användarens. Det här avsnittet innehåller både fasta tillgångar och sparade sökningar.

![hello Mina tillgångar avsnitt på hello startsida](./media/data-catalog-how-to-save-pin/07-my-assets.png)

## <a name="summary"></a>Sammanfattning
Azure Data Catalog innehåller funktioner som gör det enklare toodiscover hello datakällor som du behöver, så att du och andra organisationsmedlemmar i kan ägna mindre tid som söker efter data och mer tid arbeta med den. Sparade sökningar och Fäst data tillgångar som bygger på dessa kärnfunktioner så att användarna enkelt kan identifiera datakällor som de arbetar med flera gånger.
