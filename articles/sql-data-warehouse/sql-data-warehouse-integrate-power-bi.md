---
title: aaaUse Power BI med SQL Data Warehouse | Microsoft Docs
description: "Tips för att använda Power BI med Azure SQL Data Warehouse för utveckling av lösningar."
services: sql-data-warehouse
documentationcenter: NA
author: mlee3gsd
manager: jhubbard
editor: 
ms.assetid: b12bee87-2268-40c2-81bf-ab27588b32e8
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: integrate
ms.date: 10/31/2016
ms.author: martinle;barbkess
ms.openlocfilehash: a3a347493d07af6824a561567f05894cfe3c0471
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-power-bi-with-sql-data-warehouse"></a>Använda Powerbi med SQL Data Warehouse
Precis som med Azure SQL Database, kan SQL Data Warehouse Direct Connect användaren tooleverage kraftfulla logiska pushdown tillsammans med hello analytiska funktionerna i Power BI.  Med Direct Connect skickas frågor tillbaka tooyour Azure SQL Data Warehouse i realtid att utforska hello data.  Detta kan kombineras med hello skalan för SQL Data Warehouse, kan användare toocreate dynamiska rapporter i minuter mot terabyte data.  Dessutom hello introduktionen av hello öppna i Power BI-knappen kan användare toodirectly ansluta Power BI tootheir SQL Data Warehouse utan att samla in information från andra delar av Azure.

När du använder Direct Connect du Observera:

* Ange hello fullständigt kvalificerade servernamnet vid anslutning (se nedan för mer information)
* Se till att brandväggsreglerna för hello databasen är konfigurerad för ”Tillåt åtkomst tooAzure services”.
* Varje åtgärd som att markera en kolumn eller lägger till ett filter kommer direkt söka hello datalagret
* Paneler uppdateras ungefär var femtonde minut (uppdatera inte behöver toobe schemalagd)
* Frågor och svar är inte tillgängliga för datauppsättningar som Direct Connect
* Schemaändringar har inte plockats automatiskt
* Alla frågor med Direct Connect gör timeout efter två minuter

Dessa begränsningar och anteckningar kan ändras när vi fortsätta tooimprove hello upplevelser. hello steg tooconnect beskrivs nedan.  

## <a name="using-hello-open-in-power-bi-button"></a>Med hjälp av hello ”öppna i Power BI-knappen
hello enklaste sättet toomove mellan din SQL Data Warehouse och Power BI är med hello öppna i Power BI-knappen. Den här knappen om du tooseamlessly börjar skapa nya instrumentpaneler i Power BI.  

1. tooget igång navigera tooyour SQL Data Warehouse-instans i hello klassiska Azure-portalen.
2. Klicka på hello ”öppna i Power BI-knappen.
3. Om vi inte kan toosign i direkt, eller om du inte har en Power BI-konto behöver du toosign i.  
4. Du kommer toohello SQL Data Warehouse-anslutningssidan, med hello information från ditt SQL Data Warehouse ifylld.
5. När du har angett dina autentiseringsuppgifter kommer du att fullständigt anslutna tooyour SQL Data Warehouse.

## <a name="connecting-through-hello-power-bi-portal"></a>Ansluta via hello Power BI-portalen
I tillägg toousing hello öppna i Power BI-knappen, kan användarna också ansluta tootheir SQL Data Warehouse via hello Power BI-portalen.

1. Klicka på ”Hämta uppgifter” längst ned hello hello navigeringsfönstret.
2. Välj 'Databaser'.
3. Välj en gång 'Azure SQL Data Warehouse' hello databaser på sidan och klicka på ”Anslut”.
4. Ange hello nödvändig anslutningsinformation.  Din servernamnet och databasnamnet kan hittas i hello Azure-portalen.
5. Du kommer tillbaka toohello huvudsidan i Power BI och när anslutningen görs en ny post under 'Datauppsättningar' visas med hello namnet på din instans.  
6. Du kan klicka på hello nya dataset tooexplore alla hello tabeller och vyer i databasen. Att markera en kolumn kommer att skicka en fråga tillbaka toohello källa, att dynamiskt skapa din visual. Dessa visuell information kan sparas i en ny rapport och Fäst tillbaka tooyour instrumentpanelen.

<!--Image references-->

<!--Article references-->
[SQL Data Warehouse development overview]:  ./sql-data-warehouse-overview-develop/
[SQL Data Warehouse integration overview]:  ./sql-data-warehouse-overview-integration/

<!--MSDN references-->

<!--Other Web references-->
