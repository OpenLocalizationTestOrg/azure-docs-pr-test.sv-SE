---
title: "aaaConstructing filtersträngar för hello tabelldesignern | Microsoft Docs"
description: "Hur du skapar filtersträngar för hello tabelldesign"
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: a1a10ea1-687a-4ee1-a952-6b24c2fe1a22
ms.service: storage
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/18/2016
ms.author: kraigb
ms.openlocfilehash: 48b38d27b97936064daa875e41881d51546bc11f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="constructing-filter-strings-for-hello-table-designer"></a>Hur du skapar Filtersträngar för hello tabelldesign
## <a name="overview"></a>Översikt
toofilter data i en Azure-tabell som visas i hello Visual Studio **tabelldesignern**, du skapar en Filtersträng och ange det i hello filterfältet. hello filter Strängsyntaxen definieras av hello WCF Data Services och är liknande tooa SQL WHERE-sats, men toohello tabelltjänsten via en HTTP-begäran skickas. Hej **tabelldesignern** handtag hello rätt kodning för dig, så toofilter på önskad egenskapsvärde, du behöver endast ange hello egenskapsnamn, jämförelseoperator värde, och du kan också filtrera booleska operatorn i hello fält. Du behöver inte tooinclude hello $filter frågealternativet precis som om du man skapar en tabell URL tooquery hello via hello [Storage Services REST API-referens](http://go.microsoft.com/fwlink/p/?LinkId=400447).

hello WCF Data Services baseras på hello [Open Data Protocol](http://go.microsoft.com/fwlink/p/?LinkId=214805) (OData). Mer information om hello filter system frågealternativet (**$filter**), se hello [konventioner för OData-URI-specifikationen](http://go.microsoft.com/fwlink/p/?LinkId=214806).

## <a name="comparison-operators"></a>Jämförelseoperatorer
hello stöds följande logiska operatorer för alla egenskapstyper av:

| Logisk operator | Beskrivning | Exempel Filtersträngen |
| --- | --- | --- |
| EQ |Lika med |Stad eq 'Redmond' |
| gt |Större än |Priset gt 20 |
| ge |Större än eller lika för|Pris-ge 10 |
| lt |Mindre än |Priset lt 20 |
| le |Mindre än eller lika med |Priset le 100 |
| ne |Inte lika med |Stad ne ”London” |
| och |Och |Priset le 200 och pris gt 3.5 |
| eller |Eller |Priset le 3.5 eller pris gt 200 |
| inte |inte |inte isAvailable |

När man skapar en Filtersträng är hello följande regler viktiga:

* Använd hello logiska operatorer toocompare ett värde för egenskapen tooa. Observera att det inte är möjligt toocompare ett tooa dynamiska egenskapsvärde; en sida av hello uttrycket måste vara en konstant.
* Alla delar av hello filtersträngen är skiftlägeskänsliga.
* hello konstant värde måste vara av hello samma datatyp som hello egenskap för hello tooreturn giltig filterresultaten. Läs mer om stöds egenskapstyperna [hello förstå tabelltjänst-datamodellen](http://go.microsoft.com/fwlink/p/?LinkId=400448).

## <a name="filtering-on-string-properties"></a>Filtrering på egenskaperna för anslutningssträngen
När du filtrerar på egenskaperna för anslutningssträngen omslutas hello strängkonstant enkla citattecken.

följande exempel filter på hello hello **PartitionKey** och **RowKey** egenskaper; ytterligare icke-nyckeltyp egenskaper kan också läggas toohello Filtersträngen:

    PartitionKey eq 'Partition1' and RowKey eq '00001'

Du kan sätta varje filteruttrycket inom parentes, även om det inte krävs:

    (PartitionKey eq 'Partition1') and (RowKey eq '00001')

Observera att hello tabelltjänsten inte har stöd för jokertecken används i frågor och de stöds inte i hello tabelldesignern antingen. Du kan dock utföra matchning med jämförelseoperatorer på hello önskade prefix prefix. hello returnerar följande exempel entiteter med ett efternamn egenskapen börjar med hello bokstaven ”A”:

    LastName ge 'A' and LastName lt 'B'

## <a name="filtering-on-numeric-properties"></a>Filtrering på numeriska egenskaper
toofilter på ett heltal eller flyttal, ange hello många utan citattecken.

Det här exemplet returnerar alla entiteter med ålder egenskapen vars värde är större än 30:

    Age gt 30

Det här exemplet returnerar alla entiteter med egenskapen AmountDue vars värde är mindre än eller lika med too100.25:

    AmountDue le 100.25

## <a name="filtering-on-boolean-properties"></a>Filtrering på booleska egenskaper
toofilter på ett booleskt värde som anger **SANT** eller **FALSKT** utan citattecken.

hello följande exempel returneras alla entiteter där hello IsActive egenskapen har angetts för**SANT**:

    IsActive eq true

Du kan också skriva det här filteruttrycket utan hello logisk operator. I följande exempel hello, hello tabelltjänsten också returneras alla entiteter där IsActive är **SANT**:

    IsActive

alla enheter om IsActive är false, kan du använda inte hello tooreturn operator:

    not IsActive

## <a name="filtering-on-datetime-properties"></a>Filtrering på datum/tid-egenskaper
toofilter på ett DateTime-värde, ange hello **datetime** följt av hello tidsvärdet konstant inom enkla citattecken. hello tidsvärdet konstanten måste vara i kombinerade UTC-format, enligt beskrivningen i [formatering DateTime egenskapsvärden](http://go.microsoft.com/fwlink/p/?LinkId=400449).

hello följande exempel returnerar entiteter där hello CustomerSince egenskapen är lika tooJuly 10, 2008:

    CustomerSince eq datetime'2008-07-10T00:00:00Z'
