---
title: aaaTroubleshoot distribution av problem med Windows virtuell dator i Azure | Microsoft Docs
description: "Felsöka distribution problem med Windows virtuell dator i Azurethe Resource Manager-distributionsmodellen."
services: virtual-machines-windows
documentationcenter: 
author: genlin
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
ms.openlocfilehash: 30de25827c050cc266761cfc14548bcc64237dac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-deploying-windows-virtual-machine-issues-in-azure"></a>Felsöka distribution problem med Windows virtuell dator i Azure

tootroubleshoot virtuell dator (VM) distributionsproblem i Azure, granska hello [vanliga problem](#top-issues) för vanliga fel och lösningar.

Om du behöver mer hjälp när som helst i den här artikeln kan du kontakta hello Azure experter på [hello MSDN Azure och Stack Overflow-forum](https://azure.microsoft.com/support/forums/). Alternativt kan du lagra en incident i Azure-supporten. Gå toohello [Azure supportwebbplats](https://azure.microsoft.com/support/options/) och välj **hämta stöder**.

## <a name="top-issues"></a>De främsta problemen
[!INCLUDE [virtual-machines-windows-troubleshoot-deploy-vm-top](../../../includes/virtual-machines-windows-troubleshoot-deploy-vm-top.md)]

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

## <a name="how-can-i-use-and-deploy-a-windows-client-image-into-azure"></a>Hur kan använda och distribuera en avbildning för windows-klienten till Azure?

Du kan använda Windows 7, Windows 8 eller Windows 10 i Azure för utveckling och testning scenarier om du har en lämplig Visual Studio (tidigare MSDN)-prenumeration. Detta [artikel](client-images.md) beskrivs hello behörighetskraven för att köra Windows-klienten i Azure och användning av hello Azure-galleriet bilder.

## <a name="how-can-i-deploy-a-virtual-machine-using-hello-hybrid-use-benefit-hub"></a>Hur kan jag distribuera en virtuell dator med hello Hybrid Använd förmånen (NAV)?

Det finns ett par olika sätt toodeploy virtuella Windows-datorer med hello Azure Hybrid Använd förmånen.

För en prenumeration för Enterprise-avtal:

• Distribuera virtuella datorer från specifika Marketplace-avbildningar som är förkonfigurerad med Azure Hybrid Använd förmån.

För Enterprise-avtal:

• Ladda upp en egen virtuell dator och distribuera med hjälp av en Resource Manager-mall eller Azure PowerShell.

Mer information finns i hello följande resurser:

 - [Översikt över Azure Hybrid Använd förmån](https://azure.microsoft.com/pricing/hybrid-use-benefit/)

 - [Nedladdningsbar vanliga frågor och svar](http://download.microsoft.com/download/4/2/1/4211AC94-D607-4A45-B472-4B30EDF437DE/Windows_Server_Azure_Hybrid_Use_FAQ_EN_US.pdf)

 - [Hybridrapportering i Azure används förmån för Windows Server och Windows-klient](hybrid-use-benefit-licensing.md).

 - [Hur kan jag använda hello Hybrid använda förmån i Azure](https://blogs.msdn.microsoft.com/azureedu/2016/04/13/how-can-i-use-the-hybrid-use-benefit-in-azure)

## <a name="how-do-i-activate-my-monthly-credit-for-visual-studio-enterprise-bizspark"></a>Hur aktiverar jag min månadskredit för Visual studio Enterprise (BizSpark)

tooactivate din månatliga krediter, se den här [artikel](https://azure.microsoft.com/offers/ms-azr-0064p/).

## <a name="how-tooadd-enterprise-devtest-toomy-enterprise-agreement-ea-tooget-access-toowindow-client-images"></a>Hur åt tooadd utveckling och testning Enterprise toomy Enterprise-avtal (EA) tooget tooWindow klientavbildningar?

hello möjlighet toocreate prenumerationer baserat på hello utveckling och testning Enterprise erbjuder är begränsad tooAccount ägare som har tilldelats behörighet toodo så som företagsadministratör. Hej Kontoägaren skapar prenumerationer via hello Azure-Kontoportalen och sedan ska lägga till aktiva Visual Studio-prenumeranter som medadministratörer. Så att de kan hantera och använda hello-resurser som krävs för utveckling och testning. Mer information finns i [utveckling och testning Enterprise](https://azure.microsoft.com/offers/ms-azr-0148p/).

## <a name="my-drivers-are-missing-for-my-windows-n-series-vm"></a>Drivrutinerna saknas för den virtuella datorn N-serien Windows

Drivrutiner för Windows-baserade virtuella datorer är placerade [här](n-series-driver-setup.md).

## <a name="i-cant-find-a-gpu-instance-within-my-n-series-vm"></a>Jag kan inte hitta en GPU-instans i den virtuella datorn N-serien

tootake nytta av hello GPU-funktionerna i Azure N-serien virtuella datorer som kör Windows Server 2016 eller Windows Server 2012 R2, måste du installera drivrutinerna grafik på varje virtuell dator efter distributionen. Drivrutinen installationsinformationen är tillgängliga för [virtuella Windows-datorer](n-series-driver-setup.md) och [virtuella Linux-datorer](../linux/n-series-driver-setup.md).

## <a name="are-client-images-supported-for-n-series"></a>Stöds klientavbildningar för N-serien?

Azure stöder för närvarande endast N-serien på virtuella datorer som kör Windows Server- och Linux-operativsystem.

## <a name="is-n-series-vms-available-in-my-region"></a>Är N-serien virtuella datorer i min region?

Du kan kontrollera hello tillgänglighet från hello [produkter som är tillgängliga efter region tabell](https://azure.microsoft.com/regions/services), och prissättning [här](https://azure.microsoft.com/pricing/details/virtual-machines/series/#n-series).

## <a name="what-client-images-can-i-use-and-deploy-in-azure-and-how-tooi-get-them"></a>Vilka klientavbildningar kan jag använda och distribuera i Azure och hur tooI få dem?

Du kan använda Windows 7, Windows 8 eller Windows 10 i Azure för utveckling och testning scenarier som du har en lämplig Visual Studio (tidigare MSDN)-prenumeration. 

- Windows 10-avbildningar som är tillgängliga från hello Azure-galleriet inom [berättigade utveckling och testning erbjuder](client-images.md#eligible-offers). 
- Visual Studio-prenumeranter inom någon typ av erbjudandet kan också [på lämpligt sätt förbereda och skapa](prepare-for-upload-vhd-image.md) en 64-bitars Windows 7, Windows 8 eller Windows 10-avbildning och sedan [överför tooAzure](upload-generalized-managed.md). hello används fortfarande begränsat toodev och testning av aktiva Visual Studio-prenumeranter.

Detta [artikel](client-images.md) beskrivs hello behörighetskraven för att köra Windows-klienten i Azure och användning av hello Azure-galleriet bilder.

## <a name="i-am-not-able-toosee-vm-size-family-that-i-want-when-resizing-my-vm"></a>Jag vill inte kan toosee VM-storlek familj som jag vill när du ändrar storlek på den virtuella datorn.

När en virtuell dator körs, är det distribuerade tooa fysisk server. hello fysiska servrar i Azure-regioner grupperas i kluster av vanliga fysisk maskinvara. Ändra storlek på en virtuell dator som kräver hello VM toobe flyttas toodifferent maskinvara kluster är olika beroende på vilken distributionsmodell har använt toodeploy hello VM.

- Virtuella datorer som distribueras i klassiska distributionsmodellen hello cloud service-distributionen måste tas bort och toochange hello VMs tooa storlek i en annan familj för storlek på nytt.

- Virtuella datorer som distribueras i Resource Manager-distributionsmodellen, måste du stoppa alla virtuella datorer i hello tillgänglighetsuppsättning innan du ändrar hello storleken på någon virtuell dator i hello tillgänglighetsuppsättning.

## <a name="hello-listed-vm-size-is-not-supported-while-deploying-in-availability-set"></a>hello stöds listade VM-storlek inte vid distribution i Tillgänglighetsuppsättningen.

Välj en storlek som stöds för hello tillgänglighet set-klustret. Det rekommenderas när du skapar en tillgänglighetsuppsättning toochoose hello största VM-storlek du tror att du behöver och ha som ditt första distributionen toohello tillgänglighetsuppsättning.

## <a name="can-i-add-an-existing-classic-vm-tooan-availability-set"></a>Kan jag lägga till en befintlig tillgänglighetsuppsättning för klassiska Virtuella tooan?

Ja. Du kan lägga till en befintlig klassiska Virtuella tooa ny eller befintlig Tillgänglighetsuppsättning. Mer information finns i [lägga till en befintlig virtuell dator tooan tillgänglighetsuppsättning](classic/configure-availability.md#addmachine).


## <a name="next-steps"></a>Nästa steg
Om du behöver mer hjälp när som helst i den här artikeln kan du kontakta hello Azure experter på [hello MSDN Azure och Stack Overflow-forum](https://azure.microsoft.com/support/forums/).

Alternativt kan du lagra en incident i Azure-supporten. Gå toohello [Azure supportwebbplats](https://azure.microsoft.com/support/options/) och välj **hämta stöder**.
