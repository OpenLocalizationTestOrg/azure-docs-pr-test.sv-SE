---
title: aaaApply systemuppdateringar i Azure Security Center | Microsoft Docs
description: "Det här dokumentet beskrivs hur tooimplement hello Azure Security Center rekommendationer ** gäller system uppdateringar ** och ** startas om efter system uppdateringar **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: e5bd7f55-38fd-4ebb-84ab-32bd60e9fa7a
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/03/2017
ms.author: terrylan
ms.openlocfilehash: 02024f1558b4758c09141fe1934c2e1a9845cc96
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="apply-system-updates-in-azure-security-center"></a>Tillämpa uppdateringar i Azure Security Center
Azure Security Center övervakar dagligen Windows och Linux virtuella datorer (VM) efter saknade uppdateringar av operativsystemet. Security Center hämtar en lista med tillgängliga säkerhetsuppdateringar och viktiga uppdateringar från Windows Update eller Windows Server Update Services (WSUS), beroende på vilken tjänsten har konfigurerats på en Windows-VM.  Säkerhetscenter kontrollerar också hello senaste uppdateringarna i Linux-system. Om den virtuella datorn saknas för en uppdatering av system, rekommenderar Security Center att du installerar uppdateringar

> [!NOTE]
> Det här dokumentet introducerar hello-tjänsten med hjälp av ett exempel på distribution.  Det är alltså inte en steg-för-steg-guide.
>
>

## <a name="implement-hello-recommendation"></a>Implementera hello rekommendation
1. I hello **rekommendationer** bladet väljer **gäller systemuppdateringar**.

   ![Tillämpa systemuppdateringar][1]
2. Hej **gäller systemuppdateringar** öppnas ett blad med en lista över virtuella datorer som saknar systemuppdateringar. Välj en virtuell dator.

   ![Välj en virtuell dator][2]
3. Då öppnas ett blad med en lista över saknade uppdateringar för den virtuella datorn. Välj en uppdatering av system. I det här exemplet väljer vi KB3156016.

   ![Säkerhetsuppdateringar som saknas][3]

4. Åtgärderna i hello hello **säkerhetsuppdatering** bladet tooapply hello saknad uppdatering.

   ![Säkerhetsuppdatering][4]

## <a name="reboot-after-system-updates"></a>Starta om datorn efter uppdateringarna
1. Returnera toohello **rekommendationer** bladet. En ny post skapades när du använt systemuppdateringar kallas **startas om efter systemuppdateringar**. Den här posten kan du vet att du måste tooreboot hello VM toocomplete hello process för tillämpning av uppdateringar.

   ![Starta om datorn efter uppdateringarna][5]
2. Välj **startas om efter systemuppdateringar**. Då öppnas **omstart är väntande toocomplete systemuppdateringar** bladet visas en lista över virtuella datorer som du behöver toorestart toocomplete hello gäller system uppdateringsprocessen.

   ![Väntande omstart][6]

Starta om hello VM från Azure toocomplete hello processen.

## <a name="see-also"></a>Se även
toolearn mer om Security Center finns hello följande:

* [Ange säkerhetsprinciper i Azure Security Center](security-center-policies.md) – Lär dig hur tooconfigure säkerhetsprinciper för dina Azure-prenumerationer och resursgrupper.
* [Hantera säkerhetsrekommendationer i Azure Security Center](security-center-recommendations.md) – Lär dig rekommendationer för att skydda dina Azure-resurser.
* [Övervakning av säkerhetshälsa i Azure Security Center](security-center-monitoring.md) – Lär dig hur toomonitor hello Azure-resursers hälsa.
* [Hantera och svarar toosecurity aviseringar i Azure Security Center](security-center-managing-and-responding-alerts.md) – Lär dig hur toomanage och svara toosecurity aviseringar.
* [Övervaka partnerlösningar med Azure Security Center](security-center-partner-solutions.md) – Lär dig hur toomonitor hello dina partnerlösningars hälsostatus.
* [Vanliga frågor om Azure Security Center](security-center-faq.md) --finns vanliga frågor om hur du använder hello-tjänsten.
* [Azures säkerhetsblogg](http://blogs.msdn.com/b/azuresecurity/) --hittar du blogginlägg om säkerhet och Azure kompatibilitet.

<!--Image references-->
[1]: ./media/security-center-apply-system-updates/recommendation.png
[2]:./media/security-center-apply-system-updates/select-vm.png
[3]: ./media/security-center-apply-system-updates/missing-security-updates.png
[4]: ./media/security-center-apply-system-updates/security-update.png
[5]: ./media/security-center-apply-system-updates/reboot-after-system-updates.png
[6]: ./media/security-center-apply-system-updates/restart-pending.png
