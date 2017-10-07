---
title: "aaaCommon moln Tjänstehantering | Microsoft Docs"
description: "Lär dig hur toomanage cloud services i hello Azure-portalen. Dessa exempel används hello Azure-portalen."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: cb218ad9-77d4-4149-83db-71159c00767e
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: adegeo
ms.openlocfilehash: ade8a18a7754edbaae4903230c26c009fef63ed7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomanage-cloud-services"></a>Hur tooManage Cloud Services
> [!div class="op_single_selector"]
> * [Azure Portal](cloud-services-how-to-manage-portal.md)
> * [Klassisk Azure-portal](cloud-services-how-to-manage.md)
>
>

I hello **molntjänster (klassisk)** område i hello Azure portal, du kan uppdatera en rolltjänst eller en distribution, befordra en stegvis distribution tooproduction, länka resurser tooyour molntjänst så att du kan se hello resurs beroenden och skala hello resurser tillsammans, och ta bort en molnbaserad tjänst eller en distribution.

Mer information om hur tooscale Molntjänsten är tillgänglig [här](cloud-services-how-to-scale-portal.md).

## <a name="how-to-update-a-cloud-service-role-or-deployment"></a>Så här: uppdatera en rolltjänst för molnet eller distribution
Om du behöver tooupdate hello programkod för din molntjänst använder **uppdatering** på hello cloud service-bladet. Du kan uppdatera en roll eller alla roller. tooupdate, kan du överföra ett nytt tjänstpaket eller tjänstkonfigurationsfilen.

1. I hello [Azure-portalen][Azure portal], Välj hello molntjänst som du vill tooupdate. Det här steget öppnas hello cloud service-instans bladet.
2. Hello-bladet klickar du på hello **uppdatering** knappen.

    ![Uppdateringsknappen](./media/cloud-services-how-to-manage-portal/update-button.png)

3. Uppdatera hello distribution med en ny servicepaketfil (.cspkg) och tjänstekonfigurationsfilen (.cscfg).

    ![UpdateDeployment](./media/cloud-services-how-to-manage-portal/update-blade.png)

4. **Du kan också** uppdatera hello distributionsetiketten och hello storage-konto.
5. Om några roller har endast en rollinstans väljer hello **distribuera även om en eller flera roller innehåller en enda instans** tooenable hello uppgradera tooproceed.

    Azure kan bara garantera tjänsttillgänglighet 99,95% under en molnet tjänstuppdatering om rollerna har minst två rollinstanser (virtuella datorer). Med två rollinstanser bearbetar en virtuell dator klientbegäranden medan hello andra uppdateras.

6. Kontrollera **starta distribution** toohave hello uppdateringen när hello uppladdningen av hello paketet är klar.
7. Klicka på **OK** toobegin uppdatera hello-tjänst.

## <a name="how-to-swap-deployments-toopromote-a-staged-deployment-tooproduction"></a>Så här: växla distributioner toopromote tooproduction en stegvis distribution
När du bestämmer dig toodeploy en ny version av en tjänst i molnet, steg och testa den nya versionen i din fristående molntjänstmiljö. Använd **växla** tooswitch hello URL: er av vilka hello två distributioner behandlas och befordra en ny version tooproduction.

Du kan växla distributioner från hello **molntjänster** sida eller hello instrumentpanelen.

1. I hello [Azure-portalen][Azure portal], Välj hello molntjänst som du vill tooupdate. Det här steget öppnas hello cloud service-instans bladet.
2. Hello-bladet klickar du på hello **växla** knappen.

    ![Cloud Services växlingsutrymme](./media/cloud-services-how-to-manage-portal/swap-button.png)

3. hello öppnas följande bekräftelse.

    ![Cloud Services växlingsutrymme](./media/cloud-services-how-to-manage-portal/swap-prompt.png)

4. När du har kontrollerat hello distributionsinformation klickar du på **OK** tooswap hello distributioner.

    hello distribution växlingen sker snabbt eftersom hello enda som ändras är hello virtuella IP-adresser (VIP) för hello-distributioner.

    toosave beräkning kostnader, kan du ta bort hello mellanlagring av distribution när du har kontrollerat att din Produktionsdistribution fungerar som förväntat.

### <a name="common-questions-about-swapping-deployments"></a>Vanliga frågor om växling distributioner

**Vad är hello förutsättningarna för att byta distributioner?**

Det finns två viktiga krav för en lyckad distribution växling:

- Om du vill att toouse en statisk IP-adress för din produktionsplatsen, måste du reservera en för din mellanlagringsplatsen. Annars misslyckas hello växlingen.

- Alla instanser av dina roller måste köras innan du kan utföra hello växlingen. Du kan kontrollera hello status för dina instanser i bladet för hello översikt av hello Azure-portalen. Du kan också använda hello [Get-AzureRole](/powershell/module/azure/get-azurerole?view=azuresmps-3.7.0) i Windows PowerShell.

Observera att Gästoperativsystem uppdateringar och service återställning åtgärder kan också orsakas av distributionen växlingar toofail. Mer information finns i [distribution felsöka cloud service](cloud-services-troubleshoot-deployment-problems.md).

**Medför en växling driftstopp för Mina program? Hur får jag hantera den?**

Enligt beskrivningen i hello sista avsnittet är en distribution växling vanligtvis snabbt eftersom det är bara en konfigurationsändring i hello Azure belastningsutjämnare. I vissa fall kan det dock ta tio eller fler sekunder och resultera i tillfälliga anslutningsfel. toolimit påverkan tooyour kunder, Överväg att implementera [klienten logik](../best-practices-retry-general.md).

## <a name="how-to-link-a-resource-tooa-cloud-service"></a>Så här: länka en resurs tooa molntjänst
hello Azure-portalen länkar inte resurser tillsammans som har hello aktuella klassiska Azure-portalen. Distribuera ytterligare resurser toohello i stället samma resursgrupp som används av hello tjänst i molnet.

## <a name="how-to-delete-deployments-and-a-cloud-service"></a>Så här: ta bort distributioner och en tjänst i molnet
Innan du kan ta bort en molnbaserad tjänst, måste du ta bort alla befintliga distributionen.

toosave beräkning kostnader, kan du ta bort hello mellanlagring av distribution när du har kontrollerat att din Produktionsdistribution fungerar som förväntat. Du debiteras för beräkning kostnader för distribuerade rollinstanser som har stoppats.

Använd följande procedur toodelete hello en distribution eller Molntjänsten.

1. I hello [Azure-portalen][Azure portal], Välj hello molntjänst som du vill toodelete. Det här steget öppnas hello cloud service-instans bladet.
2. Hello-bladet klickar du på hello **ta bort** knappen.

    ![Cloud Services växlingsutrymme](./media/cloud-services-how-to-manage-portal/delete-button.png)

3. Du kan ta bort hello hela molntjänst genom att kontrollera **Cloud service och dess distributioner** eller välj antingen hello **Produktionsdistribution** eller hello **mellanlagring av distribution**.

    ![Cloud Services växlingsutrymme](./media/cloud-services-how-to-manage-portal/delete-blade.png)

4. Klicka på hello **ta bort** knappen längst ned hello.
5. toodelete hello-Molntjänsten, klicka på **ta bort Molntjänsten**. Klicka sedan på hello bekräftelse på **Ja**.

> [!NOTE]
> När en tjänst i molnet har tagits bort och utförlig övervakning är konfigurerad, måste du ta bort hello data manuellt från ditt lagringskonto. Information om där toofind hello mått tabeller finns [detta](cloud-services-how-to-monitor.md) artikel.


## <a name="how-to-find-more-information-about-failed-deployments"></a>Så här: Mer information om misslyckade installationer
Hej **översikt** bladet har ett statusfält hello överst. När du klickar på hello fältet ett nytt blad öppnas och visar den felinformation. Om hello distributionen inte innehåller några fel, är hello information bladet tomt.

![Cloud Services växlingsutrymme](./media/cloud-services-how-to-manage-portal/status-info.png)



[Azure portal]: https://portal.azure.com

## <a name="next-steps"></a>Nästa steg
* [Allmän konfiguration av Molntjänsten](cloud-services-how-to-configure-portal.md).
* Lär dig hur för[distribuera en tjänst i molnet](cloud-services-how-to-create-deploy-portal.md).
* Konfigurera en [domännamn](cloud-services-custom-domain-name-portal.md).
* Konfigurera [ssl-certifikat](cloud-services-configure-ssl-certificate-portal.md).
