---
title: aaaEnable VM-agenten i Azure Security Center | Microsoft Docs
description: "Det här dokumentet beskrivs hur tooimplement hello Azure Security Center rekommendation ** aktivera VM Agent **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 5b431c25-4241-45b7-9556-cf2a1956f3da
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/02/2017
ms.author: terrylan
ms.openlocfilehash: 9bd71e638b020780537da25fd4cf7baf34d3e11a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enable-vm-agent-in-azure-security-center"></a>Aktivera VM-agenten i Azure Security Center
hello VM-agenten måste installeras på virtuella datorer (VM) i ordning för[Aktivera datainsamling](security-center-enable-data-collection.md).  Azure Security Center aktiverar du toosee som virtuella datorer kräver hello VM-agenten och rekommenderar att du aktiverar hello VM-agenten på dessa virtuella datorer.

hello VM-agenten installeras som standard för virtuella datorer som distribueras från hello Azure Marketplace. hello artikel [VM-Agent och tillägg – del 2](https://azure.microsoft.com/blog/vm-agent-and-extensions-part-2/) innehåller information om hur tooinstall hello VM-agenten.

> [!NOTE]
> Det här dokumentet introducerar hello-tjänsten med hjälp av ett exempel på distribution. Det är alltså inte en steg-för-steg-guide.
>
>

## <a name="implement-hello-recommendation"></a>Implementera hello rekommendation
1. I hello **rekommendationer bladet**väljer **aktivera VM-agenten**.
   ![Aktivera VM-Agent][1]
2. Då öppnas bladet hello **VM-agenten saknas eller inte svarar**. Det här bladet visar hello virtuella datorer som kräver hello VM-agenten. Följ instruktionerna för hello på hello bladet tooinstall hello VM-agenten.
   ![VM-agenten saknas][2]

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
[1]: ./media/security-center-enable-vm-agent/enable-vm-agent.png
[2]: ./media/security-center-enable-vm-agent/vm-agent-is-missing.png
