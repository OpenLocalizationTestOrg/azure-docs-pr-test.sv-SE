---
title: "aaaManaging partnerlösningar i Azure Security Center | Microsoft Docs"
description: "Det här dokumentet vägleder dig igenom hur Azure Security Center enkelt kan övervaka på en snabb överblick över hello hälsostatus på de partnerlösningar som är integrerade i azureprenumerationen."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 70c076ef-3ad4-4000-a0c1-0ac0c9796ff1
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/17/2017
ms.author: terrylan
ms.openlocfilehash: fc97aedf709b9044bfd3d4ecae0b58d5fa716bbb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitoring-partner-solutions-with-azure-security-center"></a>Övervaka partnerlösningar med Azure Security Center
Det här dokumentet vägleder dig igenom hur toomonitor hello hälsostatusen på partnerlösningar i Azure Security Center.

> [!NOTE]
> Det här dokumentet introducerar hello-tjänsten med hjälp av ett exempel på distribution. Det här dokumentet är inte en stegvis guide.
>
>

## <a name="monitoring-partner-solutions"></a>Övervaka partnerlösningar
Hej **partnerlösningar** panelen på hello **Security Center** bladet kan du snabbt en överblick över hello hälsostatus på de partnerlösningar som är integrerade i azureprenumerationen.

![Partnerlösningsrutan][1]

Hej **partnerlösningar** panelen visar hello antalet partnerlösningar som är integrerad med din prenumeration. Om det inte finns några lösningar integrerade, visar hello panelen hello siffran noll.

tooview hello hälsa på partnerlösningarna:

1. Välj hello **partnerlösningar** panelen. Hej **partnerlösningar** blad öppnas visas en lista över partnerlösningarna anslutna tooSecurity Center.

   ![Partnerlösningar][3]

   hello status för en partnerlösning kan vara:

   * Väl skyddad (grön): Det finns inga problem med säkerheten.
   * Inte väl skyddad (röd): Det finns säkerhetsproblem som måste åtgärdas omedelbart.
   * Stoppad reporting (orange): hello lösning har stoppats reporting dess hälsa.
   * Okänd skyddsstatus (orange): hello lösning hello hälsa är okänd just nu på grund av tooa misslyckades processen att lägga till en ny resurs toohello befintliga lösning.
   * Inga rapporter (grå) - hello lösning har inte rapporterat något ännu, status för en lösning kan vara orapporterad om det nyligen har anslutits och fortfarande distribuera.

2. Markera en partnerlösning. I det här exemplet kan du väljer hello **Qualys** lösning.  Då öppnas ett blad visar hello status för Partnerlösningen hello och hello lösning associerade resurser. Välj **lösning konsolen** tooopen hello partner partnerhanteringsfunktionen för lösningen.

   ![Partnerlösningsinformation][4]
3. Gå tillbaka toohello **Qualys** och välj **länk VM**. Hej **länken program** blad öppnas. Här kan du ansluta resurser toohello partnerlösning.

   ![Länken resurser toopartner lösning][5]

## <a name="next-steps"></a>Nästa steg
I detta dokument, som var introducerades toohello **partnerlösningar** i Security Center. toolearn mer om Security Center finns hello följande artiklar:

* [Ange säkerhetsprinciper i Azure Security Center](security-center-policies.md) – Lär dig hur tooconfigure säkerhetsprinciper för dina Azure-prenumerationer och resursgrupper.
* [Hantera säkerhetsrekommendationer i Azure Security Center](security-center-recommendations.md) – Lär dig rekommendationer för att skydda dina Azure-resurser.
* [Övervakning av säkerhetshälsa i Azure Security Center](security-center-monitoring.md) – Lär dig hur toomonitor hello Azure-resursers hälsa.
* [Hantera och svarar toosecurity aviseringar i Azure Security Center](security-center-managing-and-responding-alerts.md) – Lär dig hur toomanage och svara toosecurity aviseringar.
* [Vanliga frågor om Azure Security Center](security-center-faq.md) – finns vanliga frågor om hur du använder hello-tjänsten.
* [Azures säkerhetsblogg](http://blogs.msdn.com/b/azuresecurity/) – hello Azure security nyheter och information.

<!--Image references-->
[1]: ./media/security-center-partner-solutions/partner-solutions-tile.png
[3]: ./media/security-center-partner-solutions/partner-solutions.png
[4]: ./media/security-center-partner-solutions/partner-solutions-detail.png
[5]: ./media/security-center-partner-solutions/link-applications.png
