---
title: "aaaRegister datakällor i Azure Data Catalog | Microsoft Docs"
description: "Den här artikeln visar hur tooregister datakällor i Azure Data Catalog, inklusive hello metadatafält extraheras under registreringen."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: bab89906-186f-4d35-9ffd-61b1d903905d
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: efc8a852ddc9fb4bbacc7b0280477bd47814936f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="register-data-sources-in-azure-data-catalog"></a>Registrera datakällor i Azure Data Catalog
## <a name="introduction"></a>Introduktion
Azure Data Catalog är en helt hanterad molntjänst som fungerar som ett system för registrering och identifiering för företagets datakällor. Med andra ord Data Catalog hjälper användare att identifiera, förstå och använda datakällor och det hjälper organisationer som ger mer värde ur befintliga data. Hej första steg toomaking en datakälla kan identifieras via Data Catalog är tooregister som datakällan.

## <a name="register-data-sources"></a>Registrera datakällor
Registreringen är hello process för att extrahera metadata från hello datakälla och kopiera den data toohello datakatalogstjänsten. hello data kvar där den för tillfället finns och förblir under hello kontroll över hello administratörer och principer för hello nuvarande systemet.

tooregister en datakälla hello följande:
1. Starta hello Data Catalog registreringsverktyget för datakällor i hello Azure Data Catalog-portalen. 
2. Logga in med ditt arbets- eller skolkonto med hello samma Azure Active Directory-autentiseringsuppgifter som du använder toosign i toohello portal.
3. Välj önskade tooregister hello-datakällan.

Mer, finns i hello [Kom igång med Azure Data Catalog](data-catalog-get-started.md) kursen.

När du har registrerat hello datakällan hello katalog spårar sin plats och indexerar dess metadata. Användare kan söka, bläddra, identifiera hello datakällan och sedan använda dess plats tooconnect tooit med hjälp av programmet hello eller verktyget efter eget val.

## <a name="supported-data-sources"></a>Datakällor som stöds
En lista över datakällor som för närvarande stöds, se [Data Catalog DSR](data-catalog-dsr.md).

## <a name="structural-metadata"></a>Strukturella metadata
När du registrerar en datakälla hämtar hello registreringsverktyget information om hello struktur hello-objekt som du väljer. Den här informationen är refererad tooas strukturella metadata.

Den här strukturella metadata innehåller hello objektets plats så att användare som identifierar hello data som kan använda information tooconnect toohello objektet i hello klientverktyg valfri för alla objekt. Andra strukturella metadata innehåller namn och typ och attributet/kolumnnamn och data.

## <a name="descriptive-metadata"></a>Beskrivande metadata
Dessutom toohello viktiga strukturella metadata som extraherats från datakällan hello, hello registreringsverktyget för datakällor extraherar beskrivande metadata. Dessa metadata har hämtats från hello Beskrivningsegenskaper som exponeras av dessa tjänster för SQL Server Analysis Services och SQL Server Reporting Services. För SQL Server, värden med hjälp av hello ms\_beskrivning utökad egenskap ska extraheras. För Oracle-databas, hello hello datakälla registrering verktyget extrakt hello kommentarer kolumn från alla\_FLIKEN\_kommentarer vyn.

Tillägg toohello beskrivande metadata som extraherats från datakällan hello, kan användare ange beskrivande metadata med hjälp av hello registreringsverktyget för datakällor. Användare kan lägga till taggar och de kan identifiera experter för hello objekt registreras. Alla dessa beskrivande metadata kopieras toohello datakatalogstjänsten tillsammans med hello strukturella metadata.

## <a name="include-previews"></a>Ta med förhandsvisningar
Som standard extraheras endast metadata från datakällor och kopierade toohello datakatalogstjänsten men så här fungerar som en datakälla ofta enklare när du kan visa ett prov av hello data som den innehåller.

Du kan inkludera en ögonblicksbild förhandsgranskning av hello data i varje tabell- och som har registrerats med hjälp av registreringsverktyget för hello Data Catalog-datakälla. Om du väljer tooinclude förhandsgranskningar under registreringen innehåller hello registreringsverktyget upp too20 poster från varje tabell och visa. Den här ögonblicksbilden kopieras sedan toohello catalog tillsammans med hello strukturella och beskrivande metadata.

> [!NOTE]
> Wide tabeller med ett stort antal kolumner kan ha färre än 20 poster som ingår i deras preview.
>
>

## <a name="include-data-profiles"></a>Innefattar data profiler
Precis som inklusive förhandsversioner kan ge värdefull kontext för användare som söker efter datakällor i Data Catalog, kan inklusive en data-profil göra det enklare toounderstand identifierade datakällor.

Du kan inkludera en data-profil för varje tabell- och som har registrerats med hjälp av registreringsverktyget för hello Data Catalog-datakälla. Om du väljer tooinclude en data-profil under registreringen hello registreringsverktyget innehåller sammanställd statistik om hello data i varje tabell och visa, inklusive:

* hello antalet rader och storleken på hello data i hello-objektet.
* hello datum för hello senaste uppdateringen av hello data och schema för hello-objektet.
* hello antalet null-poster och distinkta värden för kolumner.
* hello lägsta, högsta, genomsnittlig och standardavvikelsen värden för kolumner.

Statistiken kopieras toohello catalog tillsammans med hello strukturella och beskrivande metadata.

> [!NOTE]
> Text och datum kolumner ingår inte medelvärde eller standardavvikelse statistik i data-profilen.
>
>

## <a name="update-registrations"></a>Uppdatera registreringar
Registrera en datakälla gör det kan identifieras i Data Catalog när du använder hello metadata och valfria preview extraheras under registreringen. Om hello datakällan måste toobe uppdateras i hello katalog (till exempel om hello schemat för ett objekt har ändrats, ursprungligen exkluderade tabellerna ska inkluderas eller om du vill tooupdate hello data som ingår i hello förhandsversioner) hello registreringsverktyget för datakällor Det går att köra.

Registrera en datakälla som redan har registrerats utför en ”upsert” merge-operation: befintliga objekt uppdateras, och nya objekt skapas. Eventuella metadata som tillhandahålls av användare via hello Data Catalog-portalen är kvar.

## <a name="summary"></a>Sammanfattning
Eftersom den kopierat strukturella och beskrivande metadata från en källa toohello katalog datatjänst registrering hello-datakälla i Data Catalog gör hello data enklare toodiscover och förstå. När du har registrerat hello datakälla du kommentera, hantera och identifiera med hjälp av hello Data Catalog-portalen.

## <a name="next-steps"></a>Nästa steg
Mer information om hur du registrerar datakällor finns i hello [Kom igång med Azure Data Catalog](data-catalog-get-started.md) kursen.
