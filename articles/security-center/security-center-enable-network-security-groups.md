---
title: "aaaEnable Nätverkssäkerhetsgrupper i Azure Security Center | Microsoft Docs"
description: "Det här dokumentet beskrivs hur tooimplement hello Azure Security Center rekommendation ** aktivera Network Security grupper **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: f53ed853-ffaf-4530-a019-1906ba6f341b
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/02/2017
ms.author: terrylan
ms.openlocfilehash: 2f70fe432aa452f833a5c322d13102ebbd6dbb69
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enable-network-security-groups-in-azure-security-center"></a>Aktivera Nätverkssäkerhetsgrupper i Azure Security Center
Azure Security Center rekommenderar att du aktiverar en nätverkssäkerhetsgrupp (NSG) om en inte redan är aktiverad. NSG: er innehåller en lista över regler för åtkomstkontrollistan (ACL) som tillåter eller nekar nätverkstrafik tooyour VM-instanser i ett virtuellt nätverk. NSG:er kan antingen associeras med undernät eller individuella VM-instanser inom det undernätet. När en NSG är associerad med ett undernät, gäller hello ACL-regler tooall hello VM-instanser i det undernätet. Dessutom kan trafik tooan enskild VM begränsas ytterligare genom att koppla en NSG direkt toothat VM. Det finns fler toolearn [vad är en Nätverkssäkerhetsgrupp (NSG)?](../virtual-network/virtual-networks-nsg.md)

Om du inte har NSG: er aktiverat Security Center visas två rekommendationer tooyou: aktivera Nätverkssäkerhetsgrupper i undernät och aktivera Nätverkssäkerhetsgrupper på virtuella datorer. Du kan välja vilken nivå, undernät eller Virtuella tooapply NSG: er.

> [!NOTE]
> Det här dokumentet introducerar hello-tjänsten med hjälp av ett exempel på distribution.  Det är alltså inte en steg-för-steg-guide.
>
>

## <a name="implement-hello-recommendation"></a>Implementera hello rekommendation
1. I hello **rekommendationer** bladet väljer **aktivera Nätverkssäkerhetsgrupper** på undernät eller virtuella datorer.
   ![Aktivera nätverkssäkerhetsgrupper][1]
2. Då öppnas bladet hello **konfigurera saknade Nätverkssäkerhetsgrupper** för undernät eller virtuella datorer, beroende på hello rekommenderar att du har valt. Välj ett undernät eller en virtuell dator tooconfigure en NSG på.

   ![Konfigurera NSG för undernätet][2]

   ![Konfigurera NSG för VM][3]
3. På hello **Välj nätverkssäkerhetsgrupp** bladet Välj en befintlig NSG eller **Skapa nytt** toocreate en NSG.

   ![Välj Nätverkssäkerhetsgrupp][4]

Om du skapar en NSG åtgärderna hello i [hur toomanage NSG: er med hjälp av hello Azure-portalen](../virtual-network/virtual-networks-create-nsg-arm-pportal.md) toocreate en NSG och ange säkerhetsregler.

## <a name="see-also"></a>Se även
Den här artikeln visar hur hello tooimplement Security Center rekommendation ”aktivera Nätverkssäkerhetsgrupper” för undernät eller virtuella datorer. toolearn mer om hur du aktiverar NSG: er, se hello följande:

* [Vad är en nätverkssäkerhetsgrupp (NSG)?](../virtual-network/virtual-networks-nsg.md)
* [Hur toomanage NSG: er med hjälp av hello Azure-portalen](../virtual-network/virtual-networks-create-nsg-arm-pportal.md)

toolearn mer om Security Center finns hello följande:

* [Ange säkerhetsprinciper i Azure Security Center](security-center-policies.md) – Lär dig hur tooconfigure säkerhetsprinciper för dina Azure-prenumerationer och resursgrupper.
* [Hantera säkerhetsrekommendationer i Azure Security Center](security-center-recommendations.md) – Lär dig rekommendationer för att skydda dina Azure-resurser.
* [Övervakning av säkerhetshälsa i Azure Security Center](security-center-monitoring.md) – Lär dig hur toomonitor hello Azure-resursers hälsa.
* [Hantera och svarar toosecurity aviseringar i Azure Security Center](security-center-managing-and-responding-alerts.md) – Lär dig hur toomanage och svara toosecurity aviseringar.
* [Övervaka partnerlösningar med Azure Security Center](security-center-partner-solutions.md) – Lär dig hur toomonitor hello dina partnerlösningars hälsostatus.
* [Vanliga frågor om Azure Security Center](security-center-faq.md) --finns vanliga frågor om hur du använder hello-tjänsten.
* [Azures säkerhetsblogg](http://blogs.msdn.com/b/azuresecurity/) --hello Azure security nyheter och information.

<!--Image references-->
[1]: ./media/security-center-enable-nsg/enable-nsg.png
[2]:./media/security-center-enable-nsg/configure-nsg-for-subnet.png
[3]: ./media/security-center-enable-nsg/configure-nsg-for-vm.png
[4]: ./media/security-center-enable-nsg/choose-nsg.png
