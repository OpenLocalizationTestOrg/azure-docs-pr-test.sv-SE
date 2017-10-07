---
title: "aaaMobile Engagement exportera API-översikt"
description: "Läs hello grunderna om hur du exporterar dina rådata genereras av användarnas enheter tooleverage det i dina egna verktyg"
services: mobile-engagement
documentationcenter: mobile
author: kpiteira
manager: erikre
editor: 
ms.assetid: 9380d47b-d7fa-4d4c-888f-97e6482196bb
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 04/26/2016
ms.author: kapiteir
ms.openlocfilehash: f55be29a29878e74f6a33419f08a5574a07a7478
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="mobile-engagement-export-api-overview"></a>Översikt över Mobile Engagement Export-API
## <a name="introduction"></a>Introduktion
I detta dokument kan du lära dig hello grunderna om hur du exporterar dina rådata genereras av användarnas enheter tooleverage det i dina egna verktyg.

## <a name="pre-requisites"></a>Förutsättningar
Exportera hello rådata från Mobile Engagement kräver:

* API-autentisering installationsprogrammet toobe kan toouse hello API: er (se [autentisering manuell installation](mobile-engagement-api-authentication-manual.md)),
* Använda hello REST API: er eller hello [.net SDK](mobile-engagement-dotnet-sdk-service-api.md),
* Ett Azure Storage-konto.

> [!NOTE]
> Rekommenderar vi också utmärkt hello [Microsoft Azure Lagringsutforskaren](http://storageexplorer.com/), minst under hello utvecklingsfasen eftersom den ger ett enkelt toouse användargränssnitt för att interagera med Azure Storage.
> 
> 

## <a name="what-can-be-exported"></a>Vad kan exporteras?
Mobile Engagement kan dess användare toocollect många typer av data och därför har flera typer av export lämpliga toothese olika datatyper.
Det finns 2 grundläggande typer av export:

* Ögonblicksbild: används normalt tooexport data som representerar ett tillstånd och där Mobile Engagement inte är en historik. Detta omfattar taggar (app-info)-tokens eller push kampanj feedback till exempel. Följaktligen dessa typer av export är inte relaterade tooa datum.
* historiska: den här exporten typen används för data som samlas över tid, till exempel händelser eller aktiviteter till exempel.

hello tabellen nedan beskrivs omfattande all hello möjliga export:

| Exportera typ | Datatyp | Beskrivning |
| --- | --- | --- |
| Ögonblicksbild |Push |Genererar en export av Push-kampanjer feedback på grundval av per deviceid/användar-ID |
| Ögonblicksbild |Tagga |Genererar en export av hello taggar (app-info) som är associerade tooeach enheter |
| Ögonblicksbild |Enhet |Genererar en export av de flesta hello data om enheter som hello technicals (modell, språk, tidszon,...), hello taggar, visas första gången... |
| Ögonblicksbild |Token |Genererar en export av alla giltiga hello-tokens |
| Historiska |Aktivitet |Genererar en export av alla hello aktiviteter för alla enheter på en viss tidsperiod |
| Historiska |Händelse |Genererar en export av alla hello aktiviteter för alla enheter på en viss tidsperiod |
| Historiska |Jobb |Genererar en export av alla hello jobb för alla enheter på en viss tidsperiod |
| Historiska |Fel |Genererar en export av alla hello-fel för varje enheter på en viss tidsperiod |

## <a name="how-does-it-work"></a>Hur fungerar det?
Export är långa pågående aktiviteter som kan ge stora datafiler. Därför måste de anropade tooreturn omedelbart filen toodownload.
I ordning tooexport data från Mobile Engagement, har du toocreate en **exportera jobbet** via API anger du vanligtvis:

* hello typ av export (ögonblicksbild eller tidigare)
* hello-datatyp
* Hej **Azure Storage-behållare** (inklusive en giltig SAS med skrivbehörighet) där hello resultatet exporten hello ska skrivas.
* t.ex. exempel behållaren URL-parametern skulle bli https://[StorageAccountName].blob.core.windows.net/[ContainerName]? [SASWritePermissionsToken]  

Här är ett exempel på verkliga världen. https://testazmeexport.BLOB.Core.Windows.NET/test1234azme?SV=2015-12-11&ss=b&SRT=SCO&SP=rwdlac&Se=2016-12-17T04:59:26Z & st = 2016-12-16T20:59:26Z & spr = https & sig = KRF3aVWjp2NEJDzjlmoplmu0M9HHlLdkBWRPAFmw90Q % 3D

Observera att det kan ta några minuter för ditt jobb toobe igång och sedan den kan köras några sekunder för små appar tooseveral timmarna för appar med många användare eller aktivitet.

När hello jobbet har skapats, det är möjligt toocheck dess status toosee om det är fortfarande körs eller har slutförts.

Hello resulterande datafilen är tillgänglig på hello tillhandahålls lagringsbehållaren när hello jobbet är lyckades.

