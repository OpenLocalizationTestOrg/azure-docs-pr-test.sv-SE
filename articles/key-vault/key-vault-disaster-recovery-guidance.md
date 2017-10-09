---
title: "aaaWhat toodo hello ett avbrott i Azure-tjänsten som påverkar Azure Key Vault för händelsen | Microsoft Docs"
description: "Lär dig vilka toodo i ett avbrott i Azure-tjänsten som påverkar Azure Key Vault hello-händelse."
services: key-vault
documentationcenter: 
author: adamglick
manager: mbaldwin
editor: 
ms.assetid: 19a9af63-3032-447b-9d1a-b0125f384edb
ms.service: key-vault
ms.workload: key-vault
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2017
ms.author: sumedhb;aglick
ms.openlocfilehash: 88eec82ada401a28323b3eea126168185ba4cdb0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-key-vault-availability-and-redundancy"></a>Azure Key Vault tillgänglighet och redundans
Azure Key Vault har flera lager för redundans toomake till att dina nycklar och hemligheter förblir tillgängliga tooyour programmet även om enskilda komponenter i hello tjänsten misslyckas.

hello innehållet i nyckelvalvet replikeras inom hello och tooa sekundär region minst 150 mil bort men inom hello samma geografisk plats. Detta upprätthåller en hög hållbarhet för dina nycklar och hemligheter. Se hello [Azure länkas regioner](https://docs.microsoft.com/en-us/azure/best-practices-availability-paired-regions) dokumentet för mer information om specifika region par.

Om enskilda komponenter i hello key vault-tjänsten misslyckas, steg alternativa komponenter inom hello region i tooserve din begäran toomake till att det finns ingen försämring av funktioner. Du behöver inte tootake någon åtgärd tootrigger detta. Det sker automatiskt och blir transparent tooyou.

I hello sällsynt händelse att en hela Azure-region är tillgängligt, hello-begäranden som du gör av Azure Key Vault i regionen dirigeras automatiskt (*redundansväxlats*) tooa sekundär region. När hello primära regionen är tillgänglig igen begäranden dirigeras tillbaka (*misslyckades tillbaka*) toohello primär region. Igen och behöver du inte tootake någon åtgärd eftersom detta sker automatiskt.

Det finns några varningar toobe medveten om:

* Det kan ta några minuter för hello service toofail i hello-händelsen för en region växling vid fel. Begäranden som görs under den här tiden kan misslyckas tills hello redundansväxlingen är klar.
* Efter en växling vid fel är nyckelvalvet i skrivskyddat läge. Begäranden som stöds i det här läget är:
  * Lista nyckelvalv
  * Hämta egenskaper för nyckelvalv
  * Lista hemligheter
  * Hämta hemligheter
  * Lista nycklar
  * Hämta nycklar (egenskaperna för)
  * Kryptera
  * Dekryptera
  * Omsluta
  * Packa upp
  * Verifiera
  * Logga
  * Säkerhetskopiering
* Efter en redundansväxling har misslyckats tillbaka, alla typer av begäranden (inklusive Läs *och* skrivåtgärder) är tillgängliga.

