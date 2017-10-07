---
title: aaaStream Analytics Data Lake Store utdata | Microsoft Docs
description: Konfigurationen av autentisering och auktorisering av ett Azure Data Lake Store i ett Stream Analytics-jobb
keywords: 
services: stream-analytics
documentationcenter: 
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: ea5baafa-0054-4c70-973a-6a3a8c6eaffc
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 03/28/2017
ms.author: samacha
ms.openlocfilehash: 183cf51edb2e49ac3e42257e67a8077b95777258
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="stream-analytics-data-lake-store-output"></a>Stream Analytics Data Lake Store-utdata
Stream Analytics-jobb stöder flera metoder för utdata, en som en [Azure Data Lake Store](https://azure.microsoft.com/services/data-lake-store/). Azure Data Lake Store är en företagsomfattande storskalig lagringsplats för analytiska arbetsbelastningar för stordata. Data Lake Store kan du toostore data för alla storlek, typ och införandet hastighet för drifts- och undersökande analyser.

## <a name="authorize-a-data-lake-store-account"></a>Godkänna ett Data Lake Store-konto
1. När Data Lake Store är markerad som utdata i hello Azure-portalen, uppmanas du att tooauthorize användning av dina befintliga Data Lake Store eller toorequest åtkomst till toohello Data Lake Store via hello klassiska portalen.
   
   ![](media/stream-analytics-data-lake-output/stream-analytics-data-lake-output-authorization.png)  
   
2. Om du redan har åtkomst till tooData Lake Store och klicka på ”Verifiera nu” under en kort tid en sida visas som anger ”omdirigera tooauthorization”. hello sidan stängs automatiskt och visas med hello som skulle låta tooconfigure hello Data Lake Store-utdata.

Om du inte har registrerat dig för Data Lake Store kan du följa hello ”logga nu” link tooinitiate hello begäran eller följa hello [igång instruktioner](../data-lake-store/data-lake-store-get-started-portal.md).

## <a name="configure-hello-data-lake-store-output-properties"></a>Konfigurera egenskaper för hello Data Lake Store-utdata
När du har autentiserad hello Data Lake Store-konto kan konfigurera du hello egenskaper för Data Lake Store-utdata. hello tabellen nedan är hello lista med egenskapsnamn och deras beskrivning tooconfigure din Data Lake Store-utdata.

<table>
<tbody>
<tr>
<td><B>EGENSKAPSNAMN</B></td>
<td><B>BESKRIVNING</B></td>
</tr>
<tr>
<td>Kolumnalias</td>
<td>Detta är ett eget namn som används i frågor toodirect hello frågan utdata toothis Data Lake Store.</td>
</tr>
<tr>
<td>Data Lake Store-konto</td>
<td>hello namnet på hello storage-konto där du skickar din utdata. Visas med en lista över datasjölagerkonton hello inloggad användare har åtkomst till.</td>
</tr>
<tr>
<td>Prefixet sökvägar [<I>valfria</I>]</td>
<td>Hej filen sökvägen toowrite filerna i hello angivna Data Lake Store-konto. <BR>{date} {time}<BR>Exempel 1: mapp1/logs / {date} / {time}<BR>Exempel 2: mapp1/logs / {date}</td>
</tr>
<tr>
<td>Datumformat [<I>valfria</I>]</td>
<td>Du kan välja hello datumformat där filerna ordnas om hello datumtoken används i hello prefix sökväg. Exempel: ÅÅÅÅ/MM/DD</td>
</tr>
<tr>
<td>Tidsformat [<I>valfria</I>]</td>
<td>Ange hello tidsformat där filerna ordnas om hello tid token används i hello prefix sökväg. Hello stöds endast värdet är för närvarande HH.</td>
</tr>
<tr>
<td>Händelsen serialiseringsformat</td>
<td>Serialiseringsformat för utdata. JSON-, CSV- och Avro stöds.</td>
</tr>
<tr>
<td>Encoding</td>
<td>Om CSV- eller JSON-format, måste kodning anges. UTF-8 är hello stöds endast kodningsformat just nu.</td>
</tr>
<tr>
<td>Avgränsare</td>
<td>Gäller endast för CSV-serialisering. Stream Analytics stöder ett antal olika avgränsare för serialisering CSV. Värden som stöds är kommatecken, semikolon, blanksteg, TABB och vertikalstreck.</td>
</tr>
<tr>
<td>Format</td>
<td>Gäller endast för JSON-serialisering. Radseparering innebär att hello utdata formateras genom att varje JSON-objekt avgränsas med en ny rad. Matrisen anger hello utdata formateras som en matris av JSON-objekt.</td>
</tr>
</tbody>
</table>

## <a name="renew-data-lake-store-authorization"></a>Förnya auktorisering för Data Lake Store
För närvarande finns en begränsning där hello autentiseringstoken måste toobe manuellt uppdateras efter 90 dagar för alla jobb med Data Lake Store-utdata. Du måste också toore-autentisera ditt Data Lake Store-konto om du har ändrat ditt lösenord eftersom utskriftsjobbet skapades eller senast autentiserad. Ett symtom på det här problemet är inga jobbutdata och ett fel i hello Åtgärdsloggar som anger behovet av återauktorisering.

tooresolve det här problemet stoppa körs jobbet och gå tooyour Data Lake Store-utdata. Klicka på hello ”förnya auktorisering” länk och under en kort tid en sida visas som anger ”omdirigera tooauthorization..”. hello sidan stängs automatiskt och om detta lyckas visar ”tillstånd har förnyats”. Sedan måste tooclick ”spara” längst ned hello hello sida och kan fortsätta genom att starta om jobbet från hello stoppats senast tooavoid data går förlorade.

![](media/stream-analytics-data-lake-output/stream-analytics-data-lake-output-renew-authorization.png)

