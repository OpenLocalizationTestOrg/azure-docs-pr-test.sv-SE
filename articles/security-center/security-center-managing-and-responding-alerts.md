---
title: "aaaManage säkerhetsaviseringar i Azure Security Center | Microsoft Docs"
description: "Det här dokumentet hjälper du toouse Azure Security Center funktioner toomanage och svara toosecurity aviseringar."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: b88a8df7-6979-479b-8039-04da1b8737a7
ms.service: security-center
ms.topic: hero-article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/19/2017
ms.author: yurid
ms.openlocfilehash: f1cb7e4770776827b75ed15893914678c1f44216
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="managing-and-responding-toosecurity-alerts-in-azure-security-center"></a>Hantera och åtgärda toosecurity aviseringar i Azure Security Center
Det här dokumentet hjälper dig att använda Azure Security Center toomanage och svara toosecurity aviseringar.

> [!NOTE]
> tooenable avancerade identifieringar, uppgradera tooAzure Security Center Standard. En kostnadsfri 60-dagars utvärderingsversion är tillgänglig. tooupgrade, Välj prisnivån i hello [säkerhetsprincip](security-center-policies.md). Se [priser för Azure Security Center](security-center-pricing.md) toolearn mer.
>
>

## <a name="what-are-security-alerts"></a>Vad är säkerhetsaviseringar?
Security Center automatiskt samlar in, analyserar, och integrerar loggdata från resurserna i Azure, hello nätverk och anslutna partnerlösningar som brandväggen och endpoint protection lösningar, toodetect verkliga hot och falsklarm. En lista med prioritetssorterade säkerhetsaviseringar visas i Security Center tillsammans med hello information du behöver tooquickly undersöka hello problem och rekommendationer för hur tooremediate angreppet.


> [!NOTE]
> Mer information om hur identifieringsfunktionerna i Security Center fungerar finns i [Identifieringsfunktioner i Azure Security Center](security-center-detection-capabilities.md).
>
>

## <a name="managing-security-alerts"></a>Hantera säkerhetsaviseringar
Du kan granska aktuella aviseringar genom att titta på hello **säkerhetsaviseringar** panelen. Öppna Azure-portalen och gör hello nedan toosee mer information om varje avisering:

1. På instrumentpanelen för hello Security Center ser du hello **säkerhetsaviseringar** panelen.

    ![Panelen Säkerhetsaviseringar i Security Center](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig1-ga.png)

2. Klicka på hello panelen tooopen hello **säkerhetsaviseringar** blad som innehåller mer information om hello aviseringar som visas nedan.

   ![hello bladet med säkerhetsaviseringar i Security Center](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig2-ga.png)

Hello längst ned i det här bladet är hello information för varje avisering. toosort, klicka på hello-kolumn som du vill toosort av. hello definition för varje kolumn anges nedan:

* **Beskrivning**: en kort förklaring av hello avisering.
* **Count (Antal)**: antalet problem av just den här typen som upptäckts en specifik dag
* **Upptäckt av**: hello tjänst som utlöste aviseringen hello.
* **Datum**: hello datum hello händelsen inträffade.
* **Tillstånd**: hello aktuell status för den här aviseringen. Det finns två tillstånd:
  * **Aktiva**: hello Säkerhetsvarning har upptäckts.
* **Allvarlighetsgrad**: hello allvarlighetsgrad, vilket kan vara hög, medel eller låg.

### <a name="filtering-alerts"></a>Filtrera varningar
Aviseringarna kan filtreras efter datum, status och allvarlighetsgrad. Filtrera aviseringarna kan vara användbart för scenarier där du behöver toonarrow hello omfattning säkerhet aviseringar visas. Exempelvis kanske du vill tooaddress säkerhetsaviseringar som uppstått i hello senaste dygnet eftersom du undersöker ett potentiellt angrepp i hello system.

1. Klicka på **Filter** på hello **säkerhetsaviseringar** bladet. Hej **Filter** blad öppnas och du väljer hello datum, status och allvarlighetsgrad värden som du vill toosee.

    ![Filtrera varningar i Security Center](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig3-2017.png)

### <a name="respond-toosecurity-alerts"></a>Svara toosecurity aviseringar
Välj en avisering toolearn säkerhet mer om hello var som utlöste hello avisering och vad, om sådant finns, hur du behöver tootake tooremediate angreppet. Säkerhetsaviseringarna är indelade i grupper efter typ och datum. Klicka på en säkerhetsavisering öppnas ett blad som innehåller en lista över hello grupperade aviseringar.

![Svara toosecurity aviseringar i Azure Security Center](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig5-ga.png)

I det här fallet finns hello aviseringar som har utlösts toosuspicious Remote Desktop Protocol (RDP)-aktivitet. hello första kolumnen ser du vilka resurser som är angripna; hello andra visar hur många gånger hello resurs angripna; hello tredje visar hello tid hello attack; hello fjärde visar tillståndet för hello avisering; och hello femte visar hello allvarlighetsgraden hello-attack. När du har granskat informationen klickar du på hello angripna resursen och ett nytt blad öppnas.

![Förslag på vad toodo om säkerhet aviseringar i Azure Security Center](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig6-ga.png)

I hello **beskrivning** på det här bladet hittar du mer information om den här händelsen. Dessa ytterligare information kan du se vilka utlösta hello säkerhet aviseringen hello målresurs, när tillämpliga hello käll-IP-adress och rekommendationer om hur tooremediate.  I vissa fall ska hello källans IP-adress vara tomt (inte tillgängligt) eftersom inte alla säkerhetshändelseloggar i Windows hello IP-adress.

hello reparation föreslås av Security Center varierar bl.a toohello säkerhetsavisering. I vissa fall kan du kanske toouse andra funktioner i Azure-tooimplement hello rekommenderade åtgärder. Till exempel hello reparation för det här angreppet är tooblacklist hello IP-adress som angreppet kommer ifrån med hjälp av en [nätverks-ACL](../virtual-network/virtual-networks-acl.md) eller en [nätverkssäkerhetsgruppen](../virtual-network/virtual-networks-nsg.md) regeln.

> [!NOTE]
> Mer information om hello olika typer av aviseringar [säkerhetsaviseringar efter typ i Azure Security Center](security-center-alerts-type.md).
>
>

## <a name="see-also"></a>Se även
I det här dokumentet du lärt dig hur tooconfigure säkerhetsprinciper i Security Center. toolearn mer om Security Center finns hello följande:

* [Hantera säkerhetsincidenter i Azure Security Center](security-center-incident.md)
* [Identifieringsfunktioner i Azure Security Center](security-center-detection-capabilities.md)
* [Planerings- och bruksanvisning för Azure Security Center](security-center-planning-and-operations-guide.md)
* [Vanliga frågor om Azure Security Center](security-center-faq.md) – finns vanliga frågor om hur du använder hello-tjänsten.
* [Azures säkerhetsblogg](http://blogs.msdn.com/b/azuresecurity/) – Här hittar du blogginlägg om säkerhet och regelefterlevnad i Azure.
