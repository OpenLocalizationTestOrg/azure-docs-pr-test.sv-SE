---
title: "aaaHow tooconnect toodata källor | Microsoft Docs"
description: "Hur-tooarticle syntaxmarkering hur tooconnect toodata källor identifieras med Azure Data Catalog."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 4e6b27a5-cf75-4012-b88c-333c1fe638e8
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: 01d659510c8e67c1238ed488f4eebf511aab7217
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconnect-toodata-sources"></a>Hur tooconnect toodata källor
## <a name="introduction"></a>Introduktion
**Microsoft Azure Data Catalog** är en helt hanterad molntjänst som fungerar som ett system för registrering och identifieringssystem för företagets datakällor. Med andra ord **Azure Data Catalog** är allt om hjälpa personer identifiera, förstå och använda datakällor och hjälpa organisationer tooget mer värde från deras befintliga data. En viktig aspekt av det här scenariot använder hello – när en användare identifierar en datakälla och förstår sitt syfte, hello nästa steg är tooconnect toohello datakällan tooput toouse dess data.

## <a name="data-source-locations"></a>Källplatser för data
Under registrering av datakälla, **Azure Data Catalog** tar emot metadata om hello-datakälla. Dessa metadata inkluderar hello information om hello datakällans plats. hello information om hello plats varierar från toodata datakälla, men det kommer alltid att innehålla hello information som behövs för tooconnect. Hello plats för SQL Server-tabellen innehåller till exempel hello servernamnet, databasnamnet, schemanamnet och tabellnamn, medan hello platsen för en SQL Server Reporting Services-rapport innehåller hello servernamn och hello sökvägen toohello rapporten. Andra typer av datakällor har platser som motsvarar hello struktur och funktionerna i källsystemet hello.

## <a name="integrated-client-tools"></a>Integrerad klientverktyg
hello enklaste sättet tooconnect tooa datakällan är toouse hello ”öppna i...” menyn i hello **Azure Data Catalog** portal. Den här menyn visar en lista med alternativ för att ansluta toohello valda datatillgången.
När du använder hello standardvyn för sida vid sida, den här menyn är tillgängliga på hello varje bricka.

 ![Öppna en SQL Server-tabellen i Excel från hello data tillgången sida vid sida](./media/data-catalog-how-to-connect/data-catalog-how-to-connect1.png)

När du använder listvyn hello är hello menyn tillgänglig i hello sökfältet hello överst i portalen hello-fönstret.

 ![Öppna en SQL Server Reporting Services-rapport i Report Manager från hello sökfältet](./media/data-catalog-how-to-connect/data-catalog-how-to-connect2.png)

## <a name="supported-client-applications"></a>Klientprogram som stöds
När du använder hello ”öppna i...” menyn för data datakällor i hello Azure Data Catalog-portalen, hello rätt klientprogrammet måste vara installerad på hello-klientdator.

| Öppna i programmet | Filnamnstillägget / protokoll | Versioner av det program som stöds |
| --- | --- | --- |
| Excel |.odc |Excel 2010 eller senare |
| Excel (överst 1000) |.odc |Excel 2010 eller senare |
| Power Query |.xlsx |Excel 2016 eller Excel 2010 eller Excel 2013 med hello Power Query för Excel-tillägg installeras |
| Power BI Desktop |.pbix |Power BI Desktop juli 2016 eller senare |
| SQL Server Data Tools |vsweb: / / |Visual Studio 2013 uppdatering 4 eller senare med SQL Server tooling installerad |
| Report Manager |http:// |Se [Webbläsarkrav för SQL Server Reporting Services](https://technet.microsoft.com/en-us/library/ms156511.aspx) |

## <a name="your-data-your-tools"></a>Dina data, din verktyg
hello alternativen i menyn för hello beror på hello typ av datatillgång markerade. Naturligtvis inte alla möjliga verktyg ska inkluderas i hello ”öppna i...” menyn, men det är fortfarande enkelt tooconnect toohello datakälla med hjälp av alla klientverktyg. När en datatillgång är markerad i hello **Azure Data Catalog** portalen hello fullständiga placering visas i egenskapsrutan hello.

 ![Anslutningsinformationen för SQL Server-tabell](./media/data-catalog-how-to-connect/data-catalog-how-to-connect3.png)

hello anslutningsinformationen information skiljer sig från typen toodata källa typ av datakälla, men hello informationen i hello portalen får du allt du behöver tooconnect toohello datakälla i alla klientverktyg. Användare kan kopiera hello anslutningsinformation för hello datakällor som de har identifierats med hjälp av **Azure Data Catalog**, så att de toowork med hello data i önskat verktyg.

## <a name="connecting-and-data-source-permissions"></a>Ansluta och data behörigheter för datakällor
Även om **Azure Data Catalog** gör datakällor kan identifieras, åtkomst toohello data förblir under hello kontroll av hello datakälla ägare eller administratör. Identifiera en datakälla i **Azure Data Catalog** ger inte en användare alla behörigheter tooaccess hello-datakällor sig själv.

toomake det enklare för användare som identifierar en data source men har inte behörighet tooaccess data användare kan uppge information i hello åtkomstbegäran egenskapen när kommentera en datakälla. Informationen som visas här – inklusive länkar toohello process eller kontaktpunkt för att få åtkomst till datakällan – visas tillsammans med hello plats informationen om datakällan i hello portal.

 ![Anslutningsinformationen med instruktionerna för begäran om åtkomst](./media/data-catalog-how-to-connect/data-catalog-how-to-connect4.png)

## <a name="summary"></a>Sammanfattning
Registrera en datakälla med **Azure Data Catalog** gör att data kan identifieras genom att kopiera strukturella och beskrivande metadata från hello datakälla till hello katalogtjänsten. När en datakälla har registrerats och identifierade kan användare ansluta toohello datakällan från hello **Azure Data Catalog** portal ”öppna i...” ” menyn eller med hjälp av sina Dataverktyg föredrar.

## <a name="see-also"></a>Se även
* [Kom igång med Azure Data Catalog](data-catalog-get-started.md) självstudiekursens steg för steg-information om hur tooconnect toodata källor.
