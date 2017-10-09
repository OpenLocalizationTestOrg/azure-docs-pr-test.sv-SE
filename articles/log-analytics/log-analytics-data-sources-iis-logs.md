---
title: aaaIIS loggar in Log Analytics | Microsoft Docs
description: "Internet Information Services (IIS) lagrar användaraktivitet i loggfiler som kan samlas in av logganalys.  Den här artikeln beskriver hur tooconfigure samling av IIS-loggar och information om hello poster skapas i hello OMS-databasen."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: cec5ff0a-01f5-4262-b2e8-e3db7b7467d2
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/12/2017
ms.author: bwren
ms.openlocfilehash: c5575351090cdabaf651bcb49867794ee3a4b6e6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="iis-logs-in-log-analytics"></a>IIS-loggar i logganalys
Internet Information Services (IIS) lagrar användaraktivitet i loggfiler som kan samlas in av logganalys.  

![IIS-loggar](media/log-analytics-data-sources-iis-logs/overview.png)

## <a name="configuring-iis-logs"></a>Konfigurera IIS-loggar
Logganalys samlar in poster från loggfiler skapas i IIS, så du måste [konfigurera IIS för att logga](https://technet.microsoft.com/library/hh831775.aspx).

Logganalys endast har stöd för IIS-loggfiler i W3C-format och stöder inte anpassade fält eller avancerade IIS-loggningen.  
Logganalys samlar inte in loggar i NCSA eller IIS native-format.

Konfigurera IIS-loggar i logganalys från hello [Data-menyn i logganalys-inställningar](log-analytics-data-sources.md#configuring-data-sources).  Det finns ingen konfiguration krävs för andra än att välja **samla in W3C format IIS-loggfiler**.

Vi rekommenderar att du ska konfigurera hello IIS logg förnyelse inställningen på varje server när du aktiverar IIS Logginsamling.

## <a name="data-collection"></a>Datainsamling
Logganalys samlar in en IIS-loggposter från varje anslutna källa ungefär var 15: e minut.  hello agent registrerar dess plats i varje händelseloggen som samlas in från.  Om hello agent frånkopplas sedan logganalys samlar in händelser från där den senast slutade, även om de händelserna som skapades när hello-agenten var offline.

## <a name="iis-log-record-properties"></a>Egenskaper för IIS-post
IIS-loggposter har en typ av **W3CIISLog** och ha hello egenskaper i den följande tabellen hello:

| Egenskap | Beskrivning |
|:--- |:--- |
| Dator |Namnet på hello-dator som hello händelse som samlats in från. |
| cIP |Hello klientens IP-adress. |
| csMethod |Metod för hello begäran som GET eller POST. |
| csReferer |Platsen som hello användaren följt en länk från toohello aktuella plats. |
| csUserAgent |Webbläsartyp av hello-klienten. |
| csUserName |Namnet på hello autentiserade användare som anslöt hello-servern. Anonyma användare anges med ett bindestreck. |
| csUriStem |Mål för hello begäran som en webbsida. |
| csUriQuery |Fråga om eventuella hello klienten försöker tooperform. |
| ManagementGroupName |Namnet på hello hanteringsgruppen för Operations Manager-agenter.  För andra agenter är AOI -\<arbetsyte-ID\> |
| RemoteIPCountry |Land hello hello klientens IP-adress. |
| RemoteIPLatitude |Latitud för hello klientens IP-adress. |
| RemoteIPLongitude |Longituden för hello klientens IP-adress. |
| scStatus |HTTP-statuskod. |
| scSubStatus |Understatus. |
| scWin32Status |Windows-statuskod. |
| sIP |IP-adressen på hello-server. |
| SourceSystem |OpsMgr |
| sPort |Porten på hello server hello-klienten är ansluten till. |
| sSiteName |Namnet på hello IIS-webbplatsen. |
| TimeGenerated |Datum och tid hello-transaktionen loggades. |
| timeTaken |Längden på tid tooprocess hello begäran i millisekunder. |

## <a name="log-searches-with-iis-logs"></a>Loggen sökningar med IIS-loggar
hello innehåller följande tabell olika exempel på loggen frågor som hämtar IIS-loggposter.

| Fråga | Beskrivning |
|:--- |:--- |
| Typ = W3CIISLog |Alla IIS-loggposter. |
| Typ = W3CIISLog scStatus = 500 |Alla IIS-loggposter med returstatus 500. |
| Typ = W3CIISLog &#124; Måttet count() av cIP |Antal av IIS-loggposter av klientens IP-adress. |
| Typ = W3CIISLog csHost = ”www.contoso.com” &#124; Måttet count() av csUriStem |Antal IIS loggposter URL för hello värden www.contoso.com. |
| Typ = W3CIISLog &#124; Måttet Sum(csBytes) av datorn &#124; TOP 500000 |Totalt antal byte som tagits emot av varje IIS-dator. |

>[!NOTE]
> Om ditt arbetsområde har uppgraderade toohello [nya Log Analytics-frågespråket](log-analytics-log-search-upgrade.md), hello senare frågorna skulle ändra toohello följande.

> | Fråga | Beskrivning |
|:--- |:--- |
| W3CIISLog |Alla IIS-loggposter. |
| W3CIISLog &#124; där scStatus == 500 |Alla IIS-loggposter med returstatus 500. |
| W3CIISLog &#124; Sammanfatta count() av cIP |Antal av IIS-loggposter av klientens IP-adress. |
| W3CIISLog &#124; där csHost == ”www.contoso.com” &#124; Sammanfatta count() av csUriStem |Antal IIS loggposter URL för hello värden www.contoso.com. |
| W3CIISLog &#124; Sammanfatta sum(csBytes) av datorn &#124; ta 500000 |Totalt antal byte som tagits emot av varje IIS-dator. |

## <a name="next-steps"></a>Nästa steg
* Konfigurera logganalys toocollect andra [datakällor](log-analytics-data-sources.md) för analys.
* Lär dig mer om [logga sökningar](log-analytics-log-searches.md) tooanalyze hello data som samlas in från datakällor och lösningar.
* Konfigurera aviseringar i logganalys tooproactively meddelar dig om viktiga villkor finns i IIS-loggar.
