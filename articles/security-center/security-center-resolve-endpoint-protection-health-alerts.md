---
title: "hälsovarningar för aaaResolve endpoint protection i Azure Security Center | Microsoft Docs"
description: "Det här dokumentet beskrivs hur tooimplement hello Azure Security Center rekommendation ** lösa Endpoint Protection hälsa aviseringar **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 4050f453-98fc-4314-8438-d476469757fb
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/01/2016
ms.author: terrylan
ms.openlocfilehash: 9631d15aa1dfa9003d56332363ae7911061ed0b5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="resolve-endpoint-protection-health-alerts-in-azure-security-center"></a>Lös hälsovarningar för endpoint protection i Azure Security Center
Azure Security Center rekommenderar att du löser identifierade endpoint protection health-aviseringar.  Security Center kan du toosee vilka virtuella datorer (VM) har endpoint protection-fel och hur många fel.

> [!NOTE]
> Det här dokumentet introducerar hello-tjänsten med hjälp av ett exempel på distribution. Det är alltså inte en steg-för-steg-guide.
> 
> 

## <a name="implement-hello-recommendation"></a>Implementera hello rekommendation
1. I hello **rekommendationer bladet**väljer **lösa Endpoint Protection hälsovarningar**.
   ![Lösa slutpunktsskydd för hälsovarningar][1]
2. Då öppnas bladet hello **Endpoint Protection-fel** som visar en lista över virtuella datorer med fel och hello antal fel för varje virtuell dator. Välj en virtuell dator hello-listan.
   ![Endpoint protection-fel][2]
3. En **fel listan** blad öppnas för hello valda VM, visas en lista över fel. Välj ett fel hello listan toolearn mer. Då öppnas ett blad med information om hello valt fel.
   ![Fel listan][3]
   ![felhändelse][4]

## <a name="see-also"></a>Se även
toolearn mer om Security Center finns hello följande:

* [Ange säkerhetsprinciper i Azure Security Center](security-center-policies.md)– Lär dig hur tooconfigure säkerhetsprinciper för dina Azure-prenumerationer och resursgrupper.
* [Hantera säkerhetsrekommendationer i Azure Security Center](security-center-recommendations.md): Här får du lära dig hur du kan skydda resurserna i Azure med hjälp av rekommendationer.
* [Övervakning av säkerhetshälsa i Azure Security Center](security-center-monitoring.md)– Lär dig hur toomonitor hello Azure-resursers hälsa.
* [Hantera och svarar toosecurity aviseringar i Azure Security Center](security-center-managing-and-responding-alerts.md)– Lär dig hur toomanage och svara toosecurity aviseringar.
* [Övervaka partnerlösningar med Azure Security Center](security-center-partner-solutions.md) – Lär dig hur toomonitor hello dina partnerlösningars hälsostatus.
* [Vanliga frågor om Azure Security Center](security-center-faq.md)--finns vanliga frågor om hur du använder hello-tjänsten.
* [Azures säkerhetsblogg](http://blogs.msdn.com/b/azuresecurity/)--hello Azure security nyheter och information.

<!--Image references-->
[1]: ./media/security-center-resolve-endpoint-protection/resolve-endpoint-protection.png
[2]: ./media/security-center-resolve-endpoint-protection/endpoint-protection-failure.png
[3]: ./media/security-center-resolve-endpoint-protection/failure-list.png
[4]: ./media/security-center-resolve-endpoint-protection/failure-event.png
