---
title: "aaaRestrict åtkomst via Internet-riktade slutpunkter i Azure Security Center | Microsoft Docs"
description: "Det här dokumentet beskrivs hur tooimplement hello Azure Security Center rekommendation ** begränsa åtkomst via Internetuppkopplad slutpunkt **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 727d88c9-163b-4ea0-a4ce-3be43686599f
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/03/2017
ms.author: terrylan
ms.openlocfilehash: ee72497088618d4db29b5ae4183f4fe77b498423
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="restrict-access-through-internet-facing-endpoints-in-azure-security-center"></a>Begränsa åtkomst via Internet-riktade slutpunkter i Azure Security Center
Azure Security Center rekommenderar att du begränsar åtkomst via Internet-riktade slutpunkter om någon av dina Nätverkssäkerhetsgrupper (NSG: er) har en eller flera regler för inkommande trafik som tillåter åtkomst från ”alla” källans IP-adress. Öppna åtkomst för ”alla” kan aktivera angripare tooaccess dina resurser. Security Center rekommenderar att du redigerar regler för inkommande trafik toorestrict åtkomst toosource IP-adresserna som verkligen behöver åtkomst.

Denna rekommendation genereras för alla icke-web-portar som innehåller ”något” som källa.

> [!NOTE]
> Det här dokumentet introducerar hello-tjänsten med hjälp av ett exempel på distribution. Det är alltså inte en steg-för-steg-guide.
>
>

## <a name="implement-hello-recommendation"></a>Implementera hello rekommendation
1. I hello **rekommendationer bladet**väljer **begränsa åtkomst via Internetuppkopplad slutpunkt**.

   ![Begränsa åtkomst via slutpunkt mot Internet][1]
2. Då öppnas bladet hello **begränsa åtkomst via Internetuppkopplad slutpunkt**. Det här bladet visar hello virtuella datorer (VM) med regler för inkommande trafik som skapar en potentiell säkerhetsrisk. Välj en virtuell dator.

   ![Välj en virtuell dator][2]
3. Hej **NSG** bladet innehåller Nätverkssäkerhetsgruppen, relaterade inkommande trafik och hello associerade VM. Välj **redigera regler för inkommande trafik** tooproceed redigera en inkommande regel.

   ![Nätverkssäkerhetsgruppen bladet][3]
4. På hello **inkommande säkerhetsregler** bladet välj hello inkommande regel tooedit. I det här exemplet ska vi väljer **AllowWeb**.

   ![Inkommande säkerhetsregler][4]

   Observera att du kan också välja **standard regler** toosee hello uppsättning standardregler som finns i alla NSG: er. hello standardreglerna kan inte tas bort, men eftersom de har tilldelats en lägre prioritet, de kan åsidosättas med hello regler som du skapar. Lär dig mer om [standard regler](../virtual-network/virtual-networks-nsg.md#default-rules).

   ![Standardregler][5]
5. På hello **AllowWeb** bladet redigera hello egenskaperna för hello inkommande regel så som hello **källa** är ett IP-adress eller ett block av IP-adresser. toolearn mer om hello egenskaperna för hello inkommande regel, se [NSG-regler](../virtual-network/virtual-networks-nsg.md#nsg-rules).

   ![Redigera regel för inkommande trafik][6]

## <a name="see-also"></a>Se även
Den här artikeln visar hur hello tooimplement Security Center rekommendation ”begränsa åtkomst via Internetuppkopplad slutpunkt”. toolearn mer om hur du aktiverar NSG: er och regler, se hello följande:

* [Vad är en nätverkssäkerhetsgrupp (NSG)?](../virtual-network/virtual-networks-nsg.md)
* [Hur toomanage NSG: er med hjälp av hello Azure-portalen](../virtual-network/virtual-networks-create-nsg-arm-pportal.md)

toolearn mer om Security Center finns hello följande:

* [Ange säkerhetsprinciper i Azure Security Center](security-center-policies.md)– Lär dig hur tooconfigure säkerhetsprinciper för dina Azure-prenumerationer och resursgrupper.
* [Hantera säkerhetsrekommendationer i Azure Security Center](security-center-recommendations.md): Här får du lära dig hur du kan skydda resurserna i Azure med hjälp av rekommendationer.
* [Övervakning av säkerhetshälsa i Azure Security Center](security-center-monitoring.md)– Lär dig hur toomonitor hello Azure-resursers hälsa.
* [Hantera och svarar toosecurity aviseringar i Azure Security Center](security-center-managing-and-responding-alerts.md)– Lär dig hur toomanage och svara toosecurity aviseringar.
* [Övervaka partnerlösningar med Azure Security Center](security-center-partner-solutions.md) – Lär dig hur toomonitor hello dina partnerlösningars hälsostatus.
* [Vanliga frågor om Azure Security Center](security-center-faq.md)--finns vanliga frågor om hur du använder hello-tjänsten.
* [Azures säkerhetsblogg](http://blogs.msdn.com/b/azuresecurity/)--hello Azure security nyheter och information.

<!--Image references-->
[1]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/restrict-access-thru-internet-facing-endpoint.png
[2]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/select-a-vm.png
[3]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/network-security-group-blade.png
[4]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/inbound-security-rules.png
[5]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/default-rules.png
[6]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/edit-inbound-rule.png
