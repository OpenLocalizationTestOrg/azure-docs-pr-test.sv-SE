---
title: "aaaRegular uttryck i OMS logganalys logga sökningar | Microsoft Docs"
description: "Du kan använda hello RegEx-nyckelord i logganalys loggen sökningar toohello hello filterresultaten enligt tooa reguljärt uttryck.  Den här artikeln innehåller hello syntax för dessa uttryck med flera exempel."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/08/2017
ms.author: bwren
ms.openlocfilehash: 3033593dac2c50e911fc69054947d40d4a74369b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-regular-expressions-toofilter-log-searches-in-log-analytics"></a>Använda reguljära uttryck toofilter loggen söker i logganalys

[Logga sökningar](log-analytics-log-searches.md) Tillåt tooextract information från hello logganalys-databasen.  [Filtrera uttryck](log-analytics-search-reference.md#filter-expressions) Tillåt toofilter hello resultaten av hello enligt toospecific kriterier.  Hej **RegEx** nyckelord kan du toospecify ett reguljärt uttryck för det här filtret.  

Den här artikeln innehåller information om syntax för reguljära uttryck hello används av logganalys.

> [!NOTE]
> Du kan bara använda RegEx med sökbara fält.  Mer information om sökbara fält finns **fälttyp** i [söka efter data med hjälp av loggen sökningar i logganalys](log-analytics-log-searches.md#use-additional-filters).


## <a name="regex-keyword"></a>RegEx-nyckelord

Använd hello följande syntax toouse hello **RegEx** nyckelord i en sökning i loggen.  Du kan använda hello andra avsnitt i den här artikeln toodetermine hello syntaxen för hello reguljära uttrycket.

    field:Regex("Regular Expression")
    field=Regex("Regular Expression")

Till exempel toouse en avisering om reguljära tooreturn registrerar med en typ av *varning* eller *fel*, använder du hello efter logg sökning.

    Type=Alert AlertSeverity=RegEx("Warning|Error")

## <a name="partial-matches"></a>Delmatchningar
Observera att hello reguljärt uttryck måste matcha hello hela texten för hello-egenskapen.  Delmatchningar returneras inte några poster.  Till exempel om du skulle tooreturn poster från en dator med namnet srv01.contoso.com hello efter logg sökning skulle **inte** returnerar några poster.

    Computer=RegEx("srv..")

Det beror på att bara hello första delen av hello namn matchar hello reguljärt uttryck.  hello returnerar följande två loggen genomsöks poster från den här datorn eftersom de matchar hello hela namn.

    Computer=RegEx("srv..@")
    Computer=RegEx("srv...contoso.com")

## <a name="characters"></a>Tecken
Ange olika tecken.

| Tecken | Beskrivning | Exempel | Exempel matchar |
|:--|:--|:--|:--|
| A | En förekomst av hello tecken. | Computer=Regex("Srv01.contoso.com") | Srv01.contoso.com |
| . | Ett enskilt tecken. | Computer=Regex("SRV...contoso.com") | Srv01.contoso.com<br>SRV02.contoso.com<br>srv03.contoso.com |
| en? | Noll eller en förekomst av hello tecken. | Datorn = RegEx (”srv01?. ”Contoso.com”) | srv0.contoso.com<br>Srv01.contoso.com |
| en * | Noll eller flera förekomster av hello tecken. | Computer=Regex("Srv01*.contoso.com") | srv0.contoso.com<br>Srv01.contoso.com<br>srv011.contoso.com<br>srv0111.contoso.com |
| en + | En eller flera förekomster av hello tecken. | Computer=Regex("Srv01+.contoso.com") | Srv01.contoso.com<br>srv011.contoso.com<br>srv0111.contoso.com |
| [*abc*] | Matcha ett enskilt tecken i hello hakparenteser | Computer=Regex("srv0[123].contoso.com") | Srv01.contoso.com<br>SRV02.contoso.com<br>srv03.contoso.com |
| [*en*-*z*] | Matcha ett enskilt tecken i hello intervallet.  Kan innehålla flera adressintervall. | Computer=Regex("srv0[1-3].contoso.com") | Srv01.contoso.com<br>SRV02.contoso.com<br>srv03.contoso.com |
| [^*abc*] | Ingen av hello hakparenteser hello tecken | Computer=Regex("srv0[^123].contoso.com") | srv05.contoso.com<br>SRV06.contoso.com<br>srv07.contoso.com |
| [^*en*-*z*] | Ingen av hello tecken i hello intervallet. | Computer=Regex("srv0[^1-3].contoso.com") | srv05.contoso.com<br>SRV06.contoso.com<br>srv07.contoso.com |
| [*n*-*m*] | Matcha en uppsättning numeriska tecken. | Computer=Regex("SRV[01-03].contoso.com") | Srv01.contoso.com<br>SRV02.contoso.com<br>srv03.contoso.com |
| @ | Valfri sträng med tecken. | Datorn = RegEx (”srv@.contoso.com”) | Srv01.contoso.com<br>SRV02.contoso.com<br>srv03.contoso.com |


## <a name="multiple-occurences-of-character"></a>Flera förekomster av tecken
Ange flera förekomster av en viss tecken.

| Tecken | Beskrivning | Exempel | Exempel matchar |
|:--|:--|:--|:--|
| {n} |  *n*förekomster av hello tecken. | Computer=Regex("BW-Win-sc01{3}.bwren.Lab") | BW-win-sc0111.bwren.lab |
| {n} |  *n*eller flera förekomster av hello tecken. | Computer=Regex("BW-Win-sc01{3,}.bwren.Lab") | BW-win-sc0111.bwren.lab<br>BW-win-sc01111.bwren.lab<br>BW-win-sc011111.bwren.lab<br>BW-win-sc0111111.bwren.lab |
| {n, m} |  *n*för*m* förekomster av hello tecken. | Computer=Regex("BW-Win-sc01{3,5}.bwren.Lab") | BW-win-sc0111.bwren.lab<br>BW-win-sc01111.bwren.lab<br>BW-win-sc011111.bwren.lab |


## <a name="logical-expressions"></a>Logiska uttryck
Välj flera värden.

| Tecken | Beskrivning | Exempel | Exempel matchar |
|:--|:--|:--|:--|
| &#124; | Logiskt OR.  Returnerar resultatet om matchar på något av uttrycken. | Typ = avisering AlertSeverity = RegEx (”varning &#124; Fel ”) | Varning<br>Fel |
| & | Logiskt och.  Returnerar resultatet om matchar på båda uttryck | EventData = regex (”(Security.\* &. \*lyckades. \*)") | Säkerhetsgranskning lyckades |


## <a name="literals"></a>Literaler
Konvertera specialtecken tooliteral tecken.  Det innehåller tecken som innehåller funktioner tooregular uttryck som?-\*^\[\]{}\(\)+\|. &.

| Tecken | Beskrivning | Exempel | Exempel matchar |
|:--|:--|:--|:--|
| \\ | Konverterar ett specialtecken tooa literal. | Status_CF =\\[fel\\] @<br>Status_CF = fel\\-@ | [Fel] Filen hittades inte.<br>Fel-filen hittades inte. |


## <a name="next-steps"></a>Nästa steg

* Bekanta dig med [logga sökningar](log-analytics-log-searches.md) tooview och analysera data i hello logganalys-databasen.
