---
title: "aaaAdd nästa generations brandvägg i Azure Security Center | Microsoft Docs"
description: "Det här dokumentet beskrivs hur tooimplement hello Azure Security Center rekommendationer ** lägga till en nästa generations brandvägg ** och ** väg traffice via nästa generations Brandvägg endast **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 48b99015-4db8-4ce8-85e4-b544c0fa203e
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/02/2017
ms.author: terrylan
ms.openlocfilehash: 9a80f12571ba08eadf3361728c6321388c863235
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-a-next-generation-firewall-in-azure-security-center"></a>Lägg till nästa generations brandvägg i Azure Security Center
Azure Security Center kan rekommenderar att du lägger till nästa generations brandvägg (nästa generations Brandvägg) från en Microsoft-partner tooincrease din säkerhetsskydd. Det här dokumentet vägleder dig genom ett exempel på hur toodo detta.

> [!NOTE]
> Det här dokumentet introducerar hello-tjänsten med hjälp av ett exempel på distribution.  Det är alltså inte en steg-för-steg-guide.
>
>

## <a name="implement-hello-recommendation"></a>Implementera hello rekommendation
1. I hello **rekommendationer** bladet väljer **lägger du till nästa generations brandvägg**.
   ![Lägga till en nästa generations brandvägg][1]
2. I hello **lägger du till nästa generations brandvägg** bladet väljer du en slutpunkt.
   ![Välj en slutpunkt][2]
3. En andra **lägger du till nästa generations brandvägg** blad öppnas. Du kan välja toouse en befintlig lösning om det är tillgängligt eller du kan skapa en ny. I det här exemplet finns det inga befintliga lösningar så skapar vi ett nästa generations Brandvägg.
   ![Skapa nästa generations brandvägg][3]
4. toocreate en nästa generations Brandvägg, markera en lösning hello listan med integrerade partnerleverantörer. I det här exemplet väljer vi **Check Point**.
   ![Välj nästa generations brandvägg lösning][4]
5. Hej **Check Point** öppnas ett blad med information om hello partnerlösning. Välj **skapa** hello information-bladet.
   ![Brandväggen information bladet][5]
6. Hej **Skapa virtuell dator** blad öppnas. Här kan du ange information som krävs toospin av en virtuell dator (VM) som kör hello nästa generations Brandvägg. Följ hello steg och ange hello nästa generations Brandvägg information som krävs. Välj OK tooapply.
   ![Skapa virtuella toorun nästa generations Brandvägg][6]

## <a name="route-traffic-through-ngfw-only"></a>Vidarebefordra trafik enbart via NGFW
Returnera toohello **rekommendationer** bladet. En ny post skapades när du har lagt till en nästa generations Brandvägg via Security Center, som kallas **dirigera trafik via nästa generations Brandvägg endast**. Denna rekommendation skapas bara om du har installerat din nästa generations Brandvägg via Security Center. Om du har mot Internet slutpunkter rekommenderar Security Center att du konfigurerar Network Security Group regler som tvingar inkommande trafik tooyour VM via din nästa generations Brandvägg.

1. I hello **rekommendationer bladet**väljer **dirigera trafik via nästa generations Brandvägg endast**.
   ![Dirigera trafiken endast via NGFW][7]
2. Då öppnas bladet hello **dirigera trafik via nästa generations Brandvägg endast**, som visar en lista över virtuella datorer som du kan vidarebefordra trafiken till. Välj en virtuell dator hello-listan.
   ![Välj en virtuell dator][8]
3. Ett blad för hello valt VM öppnas Visa relaterade regler för inkommande trafik. En beskrivning får du mer information om möjliga nästa steg. Välj **redigera regler för inkommande trafik** tooproceed redigera en inkommande regel. hello förväntningen är att **källa** har inte angetts för**alla** för hello mot Internet slutpunkter som är kopplad till hello nästa generations Brandvägg. toolearn mer om hello egenskaperna för hello inkommande regel, se [NSG-regler](../virtual-network/virtual-networks-nsg.md#nsg-rules).
   ![Konfigurera regler toolimit åtkomst][9]
   ![Redigera inkommande regel][10]

## <a name="see-also"></a>Se även
Det här dokumentet visar dig hur hello tooimplement Security Center rekommendation ”Lägg till nästa generations brandvägg”. toolearn mer om NGFWs och hello Check Point partnerlösning finns hello följande:

* [Nästa generations brandvägg](https://en.wikipedia.org/wiki/Next-Generation_Firewall)
* [Check Point vSEC](https://azure.microsoft.com/marketplace/partners/checkpoint/check-point-r77-10/)

toolearn mer om Security Center finns hello följande:

* [Ange säkerhetsprinciper i Azure Security Center](security-center-policies.md) – Lär dig hur tooconfigure säkerhetsprinciper.
* [Hantera säkerhetsrekommendationer i Azure Security Center](security-center-recommendations.md) – Lär dig rekommendationer för att skydda dina Azure-resurser.
* [Övervakning av säkerhetshälsa i Azure Security Center](security-center-monitoring.md) – Lär dig hur toomonitor hello Azure-resursers hälsa.
* [Hantera och svarar toosecurity aviseringar i Azure Security Center](security-center-managing-and-responding-alerts.md) – Lär dig hur toomanage och svara toosecurity aviseringar.
* [Övervaka partnerlösningar med Azure Security Center](security-center-partner-solutions.md) – Lär dig hur toomonitor hello dina partnerlösningars hälsostatus.
* [Vanliga frågor om Azure Security Center](security-center-faq.md) --finns vanliga frågor om hur du använder hello-tjänsten.
* [Azures säkerhetsblogg](http://blogs.msdn.com/b/azuresecurity/) --hittar du blogginlägg om säkerhet och Azure kompatibilitet.

<!--Image references-->
[1]: ./media/security-center-add-next-gen-firewall/add-next-gen-firewall.png
[2]: ./media/security-center-add-next-gen-firewall/select-an-endpoint.png
[3]: ./media/security-center-add-next-gen-firewall/create-new-next-gen-firewall.png
[4]: ./media/security-center-add-next-gen-firewall/select-next-gen-firewall.png
[5]: ./media/security-center-add-next-gen-firewall/firewall-solution-info-blade.png
[6]: ./media/security-center-add-next-gen-firewall/create-virtual-machine.png
[7]: ./media/security-center-add-next-gen-firewall/route-traffic-through-ngfw.png
[8]: ./media/security-center-add-next-gen-firewall/select-vm.png
[9]: ./media/security-center-add-next-gen-firewall/configure-rules-to-limit-access.png
[10]: ./media/security-center-add-next-gen-firewall/edit-inbound-rule.png
