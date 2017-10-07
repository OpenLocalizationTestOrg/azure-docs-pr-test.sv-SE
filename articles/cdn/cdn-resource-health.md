---
title: "aaaMonitor hello hälsotillståndet för Azure CDN resurser | Microsoft Docs"
description: "Lär dig hur toomonitor hello hälsotillståndet för dina Azure CDN-resurser med hjälp av Azure Resource Health."
services: cdn
documentationcenter: .net
author: zhangmanling
manager: zhangmanling
editor: 
ms.assetid: bf23bd89-35b2-4aca-ac7f-68ee02953f31
ms.service: cdn
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 0a77e56d2fecae4bde6c83730c05375853a6638a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-hello-health-of-azure-cdn-resources"></a>Övervaka hello Azure CDN-resursers hälsa
  
Azure CDN Resource health är en delmängd av [Azure resurshälsa](../resource-health/resource-health-overview.md).  Du kan använda Azure resource health toomonitor hello CDN-resursers hälsa och få tillämplig vägledning tootroubleshoot problem.

>[!IMPORTANT] 
>Azure CDN-resurshälsa svarar bara för närvarande för hello hälsotillståndet för leverans av CDN-globala och API-funktioner.  Azure CDN-resurshälsa verifierar inte enskilda CDN-slutpunkter.
>
>hello signaler som matas Azure CDN resurshälsa kanske in too15 minuter försenad.

## <a name="how-toofind-azure-cdn-resource-health"></a>Hur toofind Azure CDN resurshälsa

1. I hello [Azure-portalen](https://portal.azure.com), bläddra tooyour CDN-profilen.

2. Klicka på hello **inställningar** knappen.

    ![Inställningar för](./media/cdn-resource-health/cdn-profile-settings.png)

3. Under *stöd + felsökning*, klickar du på **Resource health**.

    ![CDN-resurshälsa](./media/cdn-resource-health/cdn-resource-health3.png)

>[!TIP] 
>Du kan också hitta CDN-resurserna som visas i hello *resurshälsa* panelen i hello *hjälp + support* bladet.  Du kan snabbt få för*hjälp + support* genom att klicka på hello inringad **?** i hello övre högra hörnet av hello-portalen.
>
> ![Hjälp + support](./media/cdn-resource-health/cdn-help-support.png)

## <a name="azure-cdn-specific-messages"></a>Azure CDN-specifika meddelanden

Statusvärden relaterade tooAzure CDN resource health finns nedan.

|Meddelande | Rekommenderad åtgärd |
|---|---|
|Om du har stoppats, tas bort eller felkonfigurerad en eller flera av dina CDN-slutpunkter | Du kanske har stoppats, borttagen eller felkonfigurerad en eller flera av dina CDN-slutpunkter.|
|Tyvärr, hello CDN management-tjänsten är inte tillgänglig | Återkom för statusuppdateringar; Kontakta supporten om problemet kvarstår efter hello förväntad Lösningstid.|
|Tyvärr CDN-slutpunkter kan påverkas av pågående problem med några av våra CDN-providers | Återkom för statusuppdateringar; Använd hello felsöka verktyget toolearn hur tootest din ursprung och CDN-slutpunkten; Kontakta supporten om problemet kvarstår efter hello förväntad Lösningstid. |
|Tyvärr konfigurationsändringar för CDN-slutpunkten fördröjs spridning | Återkom för statusuppdateringar; Kontakta supporten om konfigurationsändringarna inte sprids helt i hello förväntad tid.|
|Vi beklagar, men vi har problem läser in hello kompletterande portalen | Återkom för statusuppdateringar; Kontakta supporten om problemet kvarstår efter hello förväntad Lösningstid.|
Tyvärr vi har problem med några av våra CDN-providers | Återkom för statusuppdateringar; Kontakta supporten om problemet kvarstår efter hello förväntad Lösningstid. |

## <a name="next-steps"></a>Nästa steg

- [Läs en översikt över Azure resurshälsa](../resource-health/resource-health-overview.md)
- [Felsökning av problem med CDN komprimering](./cdn-troubleshoot-compression.md)
- [Felsöka problem med 404-fel](./cdn-troubleshoot-endpoint.md)