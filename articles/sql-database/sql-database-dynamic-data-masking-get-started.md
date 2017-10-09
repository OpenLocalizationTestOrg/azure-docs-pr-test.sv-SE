---
title: aaaAzure SQL-databas dynamisk datamaskning | Microsoft docs
description: "SQL-databas dynamisk datamaskning begränsar exponering av känsliga data genom att kombinera den toonon-Privilegierade användare"
services: sql-database
documentationcenter: 
author: ronitr
manager: jhubbard
editor: 
ms.assetid: 4b36d78e-7749-4f26-9774-eed1120a9182
ms.service: sql-database
ms.custom: security
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 03/09/2017
ms.author: ronitr; ronmat
ms.openlocfilehash: 68b55128dc096f7e3dd0e5ed1427b39da5d64736
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="sql-database-dynamic-data-masking"></a>SQL-databas dynamisk datamaskning

SQL-databas dynamisk datamaskning begränsar exponering av känsliga data genom att kombinera den toonon-Privilegierade användare. 

Dynamisk datamaskning hjälper till att förhindra obehörig åtkomst toosensitive data genom att aktivera kunder toodesignate hur mycket av hello känsliga data tooreveal med minimal påverkan på hello programnivå. Det är en funktion för principbaserad säkerhet som döljer hello känsliga data i hello resultatet av en fråga över avsedda databasfält medan hello data i hello databas inte ändras.

Till exempel en representant på ett Callcenter kan identifiera anropare med flera siffrorna i sina kreditkortsnummer, men dessa data objekt inte får vara helt exponeras toohello representant. En maskningsregel kan definieras som masker alla men hello sista fyra siffrorna i alla kreditkortsnummer i hello resultatet av en fråga. Ett annat exempel är vara en lämplig mask definierade tooprotect personligt identifierbar information (PII) data, så att utvecklare kan fråga produktionsmiljöer i felsökningssyfte utan brott mot förordningar.

## <a name="sql-database-dynamic-data-masking-basics"></a>SQL-databas dynamisk datamaskering grunderna
Du ställa in en dynamisk datamaskering princip i hello Azure-portalen genom att välja hello dynamisk datamaskering åtgärden i bladet för konfiguration av SQL-databas eller inställningsbladet.

### <a name="dynamic-data-masking-permissions"></a>Dynamiska datamaskering behörigheter
Dynamisk datamaskning kan konfigureras med hello Azure Database admin-administratören eller ansvarig säkerhetsroller.

### <a name="dynamic-data-masking-policy"></a>Dynamiska datamaskering princip
* **SQL-användare uteslutna från maskering** – en uppsättning SQL-användare eller AAD identiteter som hämtar omaskerat data i hello SQL frågeresultat. Användare med administratörsbehörighet är alltid uteslutna från maskering och se hello ursprungliga data utan någon mask.
* **Maskering regler** – en uppsättning regler som definierar hello avses fält toobe maskeras och hello maskering funktion som används. hello avses fält kan definieras med hjälp av ett databasnamn schemat, tabellnamnet och kolumnnamnet.
* **Maskering funktioner** – en uppsättning metoder som styr hello exponering av data för olika scenarier.

| Maskningsfunktion | Maskering logik |
| --- | --- |
| **Standard** |**Fullständig maskering bl.a toohello data typer av hello avses fält**<br/><br/>• Använd XXXX eller färre Xs om hello fältet hello storlek är mindre än 4 tecken för strängdatatyper (nchar, ntext, nvarchar).<br/>• Använda värdet noll för numeriska datatyper (bigint, bit, decimal, int, money, numeriska, smallint, smallmoney, tinyint, float, real).<br/>• Använda 1900-01-01 för datum/tid-datatyper (datum, datetime2, datetime, datetimeoffset, smalldatetime, tid).<br/>• För SQL variant, hello standardvärdet hello aktuella typen används.<br/>• För XML-dokumentet för hello <masked/> används.<br/>• Använda ett tomt värde för speciella datatyper (tidsstämpel tabell, hierarchyid, GUID, binary, image, varbinary spatialtyper). |
| **Kreditkort** |**Maskering av-metoden, som visar hello sista fyra siffrorna i hello avses fält** och lägger till en konstant sträng som prefix i hello form av ett kreditkort.<br/><br/>XXXX-XXXX-XXXX-1234 |
| **E-post** |**Maskering av-metoden, som visar hello första bokstaven och ersätter hello domän med XXX.com** med ett konstant sträng-prefix i hello form av en e-postadress.<br/><br/>aXX@XXXX.com |
| **Slumptal** |**Maskering av metoden, vilket genererar ett slumptal** enligt toohello valda gränser och faktiska datatyper. Om hello avses gränser är lika är hello maskering funktionen ett tal.<br/><br/>![Navigeringsfönstret](./media/sql-database-dynamic-data-masking-get-started/1_DDM_Random_number.png) |
| **Anpassad text** |**Maskering av metoden, som visar hello första och sista tecknen** och lägger till en anpassad utfyllnad sträng hello mitten. Om ursprungliga hello-strängen är kortare än hello exponeras prefix och suffix, används endast hello utfyllnad sträng. <br/>suffix för prefixet [utfyllnad]<br/><br/>![Navigeringsfönstret](./media/sql-database-dynamic-data-masking-get-started/2_DDM_Custom_text.png) |

<a name="Anchor1"></a>

### <a name="recommended-fields-toomask"></a>Rekommenderade fält toomask
Hej flaggor DDM rekommendationer motorn vissa fält från databasen som potentiellt känsliga, vilket kan vara bra kandidater för maskering. I hello dynamisk Datamaskering bladet i hello portal visas hello rekommenderas kolumner för din databas. Allt du behöver toodo är klickar du på **Lägg till Mask** för en eller flera kolumner och sedan **spara** tooapply en mask för dessa fält.

## <a name="set-up-dynamic-data-masking-for-your-database-using-powershell-cmdlets"></a>Konfigurera dynamisk datamaskering för databasen med hjälp av Powershell-cmdlets
Se [Azure SQL Database-Cmdlets](https://msdn.microsoft.com/library/azure/mt574084.aspx).

## <a name="set-up-dynamic-data-masking-for-your-database-using-rest-api"></a>Konfigurera dynamisk datamaskning för databasen med hjälp av REST API
Se [åtgärder för Azure SQL-databaser](https://msdn.microsoft.com/library/dn505719.aspx).

