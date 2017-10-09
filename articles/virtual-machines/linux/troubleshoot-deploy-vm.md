---
title: aaaTroubleshoot distribuera Linux virtuella problem i Azure | Microsoft Docs
description: "Felsöka distribuera Linux virtuella problem i Azurethe Resource Manager-distributionsmodellen."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 4e383427-4aff-4bf3-a0f4-dbff5c6f0c81
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: genli
ms.openlocfilehash: d1092ca3d9d51af7510b19c8c9a79e6dda588697
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-deploying-linux-virtual-machine-issues-in-azure"></a>Felsöka distribuera virtuella Linux-problem i Azure

tootroubleshoot virtuell dator (VM) distributionsproblem i Azure, granska hello [vanliga problem](#top-issues) för vanliga fel och lösningar.

Om du behöver mer hjälp när som helst i den här artikeln kan du kontakta hello Azure experter på [hello MSDN Azure och Stack Overflow-forum](https://azure.microsoft.com/support/forums/). Alternativt kan du lagra en incident i Azure-supporten. Gå toohello [Azure supportwebbplats](https://azure.microsoft.com/support/options/) och välj **hämta stöder**.

## <a name="top-issues"></a>De främsta problemen
[!INCLUDE [virtual-machines-linux-troubleshoot-deploy-vm-top](../../../includes/virtual-machines-linux-troubleshoot-deploy-vm-top.md)]

## <a name="hello-cluster-cannot-support-hello-requested-vm-size"></a>hello kluster stöder inte hello begärda VM-storlek
<properties
supportTopicIds="123456789"
resourceTags="windows"
productPesIds="1234, 5678"
/>
- Försök hello förfrågan med en mindre VM-storlek.
- Om hello efterfrågade storleken hello VM inte kan ändras:
    - Stoppa alla hello virtuella datorer i hello tillgänglighetsuppsättning. Klicka på **resursgrupper** > resursgruppen > **resurser** > din tillgänglighetsuppsättning > **virtuella datorer** > din virtuella dator >  **Stoppa**.
    - När alla hello stoppa virtuella datorer, skapa hello VM hello önskad storlek.
    - Starta hello nya virtuella datorn först och sedan väljer av hello stoppade virtuella datorer och klicka på Start.


## <a name="hello-cluster-does-not-have-free-resources"></a>hello klustret har inte lediga resurser
<properties
supportTopicIds="123456789"
resourceTags="windows"
productPesIds="1234, 5678"
/>
- Försök igen med hello begäran senare.
- Om hello ny virtuell dator kan vara en del av en annan tillgänglighetsuppsättning in
    - Skapa en virtuell dator i en annan tillgänglighetsuppsättning (i hello samma region).
    - Lägg till hello nya VM toohello samma virtuella nätverk.

## <a name="how-do-i-activate-my-monthly-credit-for-visual-studio-enterprise-bizspark"></a>Hur aktiverar jag min månadskredit för Visual studio Enterprise (BizSpark)

tooactivate din månatliga krediter, se den här [artikel](https://azure.microsoft.com/offers/ms-azr-0064p/).

## <a name="why-can-i-not-install-hello-gpu-driver-for-an-ubuntu-nv-vm"></a>Varför kan jag inte installera hello GPU-drivrutinen för en virtuell dator NV Ubuntu?

För närvarande finns endast stöd för Linux GPU i Azure NC virtuella datorer som kör Ubuntu Server 16.04 LTS. Mer information finns i [ställa in GPU drivrutiner för N-serien virtuella datorer som kör Linux](n-series-driver-setup.md).

## <a name="my-drivers-are-missing-for-my-linux-n-series-vm"></a>Drivrutinerna saknas för den virtuella datorn N-serien Linux

Drivrutiner för Linux-baserade virtuella datorer är placerade [här](n-series-driver-setup.md). 

## <a name="i-cant-find-a-gpu-instance-within-my-n-series-vm"></a>Jag kan inte hitta en GPU-instans i den virtuella datorn N-serien

tootake nytta av hello GPU-funktionerna i Azure N-serien virtuella datorer som kör Windows Server 2016 eller Windows Server 2012 R2, måste du installera drivrutinerna grafik på varje virtuell dator efter distributionen. Drivrutinen installationsinformationen är tillgängliga för [virtuella Windows-datorer](../windows/n-series-driver-setup.md) och [virtuella Linux-datorer](n-series-driver-setup.md).

## <a name="is-n-series-vms-available-in-my-region"></a>Är N-serien virtuella datorer i min region?

Du kan kontrollera hello tillgänglighet från hello [produkter som är tillgängliga efter region tabell](https://azure.microsoft.com/regions/services), och prissättning [här](https://azure.microsoft.com/pricing/details/virtual-machines/series/#n-series).

## <a name="i-am-not-able-toosee-vm-size-family-that-i-want-when-resizing-my-vm"></a>Jag vill inte kan toosee VM-storlek familj som jag vill när du ändrar storlek på den virtuella datorn.

När en virtuell dator körs, är det distribuerade tooa fysisk server. hello fysiska servrar i Azure-regioner grupperas i kluster av vanliga fysisk maskinvara. Ändra storlek på en virtuell dator som kräver hello VM toobe flyttas toodifferent maskinvara kluster är olika beroende på vilken distributionsmodell har använt toodeploy hello VM.

- Virtuella datorer som distribueras i klassiska distributionsmodellen hello cloud service-distributionen måste tas bort och toochange hello VMs tooa storlek i en annan familj för storlek på nytt.

- Virtuella datorer som distribueras i Resource Manager-distributionsmodellen, måste du stoppa alla virtuella datorer i hello tillgänglighetsuppsättning innan du ändrar hello storleken på någon virtuell dator i hello tillgänglighetsuppsättning.

## <a name="hello-listed-vm-size-is-not-supported-while-deploying-in-availability-set"></a>hello stöds listade VM-storlek inte vid distribution i Tillgänglighetsuppsättningen.

Välj en storlek som stöds för hello tillgänglighet set-klustret. Det rekommenderas när du skapar en tillgänglighetsuppsättning toochoose hello största VM-storlek du tror att du behöver och ha som ditt första distributionen toohello tillgänglighetsuppsättning.

## <a name="what-linux-distributionsversions-are-supported-on-azure"></a>Vilka Linux-distributioner /-versioner som stöds på Azure?

Du hittar Linux hello listan på [Azure-Endorsed distributioner](endorsed-distros.md).

## <a name="can-i-add-an-existing-classic-vm-tooan-availability-set"></a>Kan jag lägga till en befintlig tillgänglighetsuppsättning för klassiska Virtuella tooan?

Ja. Du kan lägga till en befintlig klassiska Virtuella tooa ny eller befintlig Tillgänglighetsuppsättning. Mer information finns i [lägga till en befintlig virtuell dator tooan tillgänglighetsuppsättning](../windows/classic/configure-availability.md#addmachine).


## <a name="next-steps"></a>Nästa steg
Om du behöver mer hjälp när som helst i den här artikeln kan du kontakta hello Azure experter på [hello MSDN Azure och Stack Overflow-forum](https://azure.microsoft.com/support/forums/).

Alternativt kan du lagra en incident i Azure-supporten. Gå toohello [Azure supportwebbplats](https://azure.microsoft.com/support/options/) och välj **hämta stöder**.
