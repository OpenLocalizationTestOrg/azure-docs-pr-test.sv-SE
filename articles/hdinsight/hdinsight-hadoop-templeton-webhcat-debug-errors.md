---
title: "aaaUnderstand och Lös WebHCat-fel i HDInsight - Azure | Microsoft Docs"
description: "Lär dig hur tooabout vanliga fel som returneras av WebHCat i HDInsight och tooresolve dem."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 1b3d94b1-207d-4550-aece-21dc45485549
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/26/2017
ms.author: larryfr
ms.openlocfilehash: 0071a1e9ed448ae146b93c8f4f518e31b95d27c9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="understand-and-resolve-errors-received-from-webhcat-on-hdinsight"></a>Förstå och åtgärda fel togs emot från WebHCat på HDInsight

Lär dig mer om felmeddelanden när du använder WebHCat med HDInsight och hur tooresolve dem. WebHCat används internt av klientsidan verktyg som Azure PowerShell och hello Data Lake-verktyg för Visual Studio.

## <a name="what-is-webhcat"></a>Vad är WebHCat

[WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) är en REST-API för [HCatalog](https://cwiki.apache.org/confluence/display/Hive/HCatalog)en tabell och lagring lagringshanteringslager för Hadoop. WebHCat är aktiverat som standard i HDInsight-kluster och används av olika verktyg toosubmit jobb, hämta jobbstatus osv utan att logga i toohello kluster.

## <a name="modifying-configuration"></a>Ändra konfigurationen

> [!IMPORTANT]
> Flera hello-fel som beskrivs i det här dokumentet inträffa konfigurerade maximalt har överskridits. När hello upplösning steg nämns att du kan ändra ett värde, måste du använda något av följande tooperform hello ändra hello:

* För **Windows** kluster: använda ett skript åtgärd tooconfigure hello värde när klustret skapas. Mer information finns i [utveckla skriptåtgärder](hdinsight-hadoop-script-actions.md).

* För **Linux** kluster: Använd Ambari (web eller REST API) toomodify hello värde. Mer information finns i [hantera HDInsight med Ambari](hdinsight-hadoop-manage-ambari.md)

> [!IMPORTANT]
> Linux är hello endast operativsystem på HDInsight version 3.4 eller senare. Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

### <a name="default-configuration"></a>Standardkonfigurationen

Om hello följande standardvärden överskrids den WebHCat prestanda försämras eller orsaka fel:

| Inställning | Vad verktyget gör | Standardvärde |
| --- | --- | --- |
| [yarn.Scheduler.Capacity.maximum-program][maximum-applications] |Hej maximalt antal jobb som kan vara aktiva samtidigt (väntande eller körs) |10 000 |
| [templeton.Exec.Max procs][max-procs] |hello maximala antalet förfrågningar som hanteras samtidigt |20 |
| [mapreduce.jobhistory.Max-ålder-ms][max-age-ms] |hello antalet dagar som jobbhistorik bevaras |7 dagar |

## <a name="too-many-requests"></a>För många begäranden

**HTTP-statuskod**: 429

| Orsak | Lösning |
| --- | --- |
| Du har överskridit hello högsta antal samtidiga begäranden hanteras av WebHCat per minut (standard är 20) |Minska din arbetsbelastning tooensure att du inte skicka fler än hello maximalt antal samtidiga begäranden eller öka hello samtidig förfrågan genom att ändra `templeton.exec.max-procs`. Mer information finns i [ändra konfiguration](#modifying-configuration) |

## <a name="server-unavailable"></a>Servern är inte tillgänglig

**HTTP-statuskod**: 503

| Orsak | Lösning |
| --- | --- |
| Statuskoden genereras vanligtvis under växling mellan hello primära och sekundära HeadNode för hello-kluster |Vänta två minuter och försök igen hello |

## <a name="bad-request-content-could-not-find-job"></a>Felaktig begäran innehåll: Det gick inte att hitta jobbet

**HTTP-statuskod**: 400

| Orsak | Lösning |
| --- | --- |
| Jobbinformation har rensats av hello jobbhistorik rengöringsband |hello loggperioden för jobbhistorik är 7 dagar. hello loggperioden kan ändras genom att ändra `mapreduce.jobhistory.max-age-ms`. Mer information finns i [ändra konfiguration](#modifying-configuration) |
| Jobbet har avslutats på grund av tooa växling vid fel |Försök jobbet för in tootwo i minuter |
| Ett ogiltigt jobb-id har använts |Kontrollera om hello jobb-id är korrekt |

## <a name="bad-gateway"></a>Ogiltig gateway

**HTTP-statuskod**: 502

| Orsak | Lösning |
| --- | --- |
| Internt skräpinsamling sker inom hello WebHCat-processen |Vänta tills skräp samling toofinish eller hello WebHCat-tjänsten |
| Tidsgränsen nåddes för väntar på svar från hello ResourceManager-tjänsten. Det här felet kan inträffa om hello antalet aktiva program går hello konfigurerade maximala (standard 10 000-tal) |Vänta tills pågående jobb toocomplete eller öka hello samtidiga jobb genom att ändra `yarn.scheduler.capacity.maximum-applications`. Mer information finns i hello [ändra configuration](#modifying-configuration) avsnitt. |
| Försök tooretrieve alla jobb via hello [GET /jobs](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference+Jobs) anrop när `Fields` har angetts för`*` |Inte hämtar *alla* Jobbdetaljer. I stället använda `jobid` tooretrieve information om jobb som endast är större än vissa jobb-id. Eller Använd inte`Fields` |
| Hej WebHCat-tjänsten är inte tillgänglig under HeadNode växling vid fel |Vänta i två minuter och försök sedan hello åtgärden |
| Det finns mer än 500 väntande jobb skicka via WebHCat |Vänta tills väntar jobben har slutförts innan du skickar flera jobb |

[maximum-applications]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.3/bk_system-admin-guide/content/setting_application_limits.html
[max-procs]: https://hive.apache.org/javadocs/hcat-r0.5.0/configuration.html
[max-age-ms]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.6.0/ds_Hadoop/hadoop-mapreduce-client/hadoop-mapreduce-client-core/mapred-default.xml
