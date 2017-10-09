---
title: "aaaInternet av saker säkerhetsmetoder | Microsoft Docs"
description: "hello får en granskad lista över Microsoft Internet saker säkerhetsmetoder och allmänna rekommendationer."
services: security
documentationcenter: na
author: TomShinder
manager: StevenPo
editor: TomSh
ms.assetid: 2d5598c5-4c30-481d-b8f4-51ee024ea9a7
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/04/2017
ms.author: yurid
ms.openlocfilehash: 7ee31c912e8ac230ffa5efcd5b4c2b0b0713584f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="internet-of-things-security-best-practices"></a>Internet av saker säkerhetsmetoder
Infrastruktur för att säkra hello Sakernas Internet (IoT) är en kritisk företag alla som sysslar med IoT-lösningar. På grund av hello antalet enheter som är inblandade och hello relaterade distribuerade uppbyggnad enheterna hello inverkan en säkerhetshändelse toocompromise miljoner IoT-enheter är icke-trivial och kan ha stor inverkan.

Därför säkerhetsbehov IoT säkerhet ingående-information. Data måste toobe säker i hello molnet och det rör sig över privata och offentliga nätverk. Metoderna måste toobe i plats toosecurely etablera hello IoT-enheter själva. Varje lager från enheten, toonetwork, toocloud backend-behöver garantier för hög säkerhet.

Metodtips för IoT kan kategoriseras i hello följande sätt:

* IoT-maskinvarutillverkaren eller integrator
* IoT-lösningen utvecklare
* Distribueraren för IoT-lösning
* IoT-lösningen operator

Den här artikeln sammanfattar [Internet av saker säkerhetsmetoder](../iot-suite/iot-security-best-practices.md). Mer detaljerad information finns i artikel toothat.

## <a name="iot-hardware-manufacturer-or-integrator"></a>IoT-maskinvarutillverkaren eller integrator
Följ hello metodtips nedan om du är en IoT programvaran eller en integrator maskinvara:

* **Omfång toominimum maskinvarukrav**: hello maskinvara design ska innehålla minsta funktioner som krävs för driften av hello maskinvara och inget mer. 
* **Kontrollera maskinvara säkerhetsförsluten bevis**: skapa i mekanismer toodetect fysiska ändring av maskinvara, till exempel öppna hello luckan tar bort en del av hello osv. 
* **Skapa runt säker maskinvara**: om [kostnad för sålda varor](https://en.wikipedia.org/wiki/Cost_of_goods_sold) tillåta, skapa säkerhetsfunktioner, till exempel säker och krypterad lagring och Trusted Platform Module TPM-baserade Start-funktioner.
* **Göra uppgraderingar säker**: uppgradera firmware under livslängden för hello enheten inte är.

## <a name="iot-solution-developer"></a>IoT-lösningen utvecklare
Följ hello metodtips nedan om du utvecklar en IoT-lösningen:

* **Följ säker software development metoder**: utveckla säkra program kräver grunden som du tänker på säkerheten från hello starten av hello projekt alla hello sätt tooits implementering, testning och distribution.
* **Välj programvara med öppen källkod med försiktighet**: programvara med öppen källkod ger en möjlighet tooquickly utveckla lösningar.
* **Integrera med försiktighet**: många hello programvara säkerhetsbrister finns på hello gränsen för bibliotek och API: er. 

## <a name="iot-solution-deployer"></a>Distribueraren för IoT-lösning
Följ hello metodtips nedan om du är en IoT-lösningen deployer:

* **Distribuera maskinvara på ett säkert sätt**: IoT-distributioner kan kräva maskinvara toobe distribuerats i osäkra platser, exempelvis offentliga blanksteg eller oövervakade språk.
* **Skydda autentiseringsnycklar**: under distribution varje enhet kräver enhets-ID och tillhörande autentiseringsnycklar som genererats av hello-Molntjänsten. Skydda de här nycklarna fysiskt även efter hello-distribution. Alla avslöjade nyckeln kan användas av en obehörig enhet toomasquerade som en befintlig enhet.

## <a name="iot-solution-operator"></a>IoT-lösningen operator
Följ hello metodtips nedan om du är operatör IoT-lösningen:

* **Behåll system toodate**: Kontrollera enhetens operativsystem och alla drivrutiner är uppdaterade toohello senaste versionerna. 
* **Skydda mot skadlig aktivitet**: om hello operativsystemet tillåter placera hello senaste antivirus- och spionprogramsskydd funktionerna på varje enhets operativsystem. 
* **Granska ofta**: granskning IoT infrastruktur för säkerhetsrelaterade problem är nyckel när svarar toosecurity incidenter.
* **Fysiskt skydda hello IoT infrastruktur**: hello sämsta attacker mot IoT infrastruktur startas med fysisk åtkomst toodevices.
* **Skydda molnet autentiseringsuppgifter**: molnet autentiseringsuppgifter som används för att konfigurera och använda en IoT-distribution är eventuellt hello enklaste sättet toogain åtkomst och IoT-dator. 

