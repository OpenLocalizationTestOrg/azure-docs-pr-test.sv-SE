---
title: aaaVertically skala virtuella Azure-datorn med Azure Automation | Microsoft Docs
description: Hur toovertically skala en Linux-dator i svaret toomonitoring aviseringar med Azure Automation
services: virtual-machines-linux
documentationcenter: 
author: singhkays
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: dcee199e-fa25-44d5-9b25-df564cee9b45
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/29/2016
ms.author: singhkay
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ee4c1c33a588bd907d107f1828380a8afdaa725e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="vertically-scale-azure-linux-virtual-machine-with-azure-automation"></a>Lodrätt skala Azure Linux-dator med Azure Automation
Lodrät skalning är hello process för att öka eller minska hello resurser för en dator i svaret toohello arbetsbelastning. I Azure kan detta åstadkommas genom att ändra hello storleken på hello virtuell dator. Detta hjälper i följande scenarier hello

* Om hello virtuella datorn inte används ofta, du kan ändra storlek på den ned tooa mindre storlek tooreduce månatliga kostnaderna
* Om hello virtuella datorn ser en belastning, kan det vara storleksändrade tooa större storlek tooincrease sin kapacitet

hello disposition för hello steg tooaccomplish detta är som nedan

1. Konfigurera Azure Automation tooaccess dina virtuella datorer
2. Importera hello Azure Automation Lodrät skala runbooks till din prenumeration
3. Lägga till en webhook tooyour runbook
4. Lägg till en avisering tooyour virtuell dator

> [!NOTE]
> På grund av hello storleken på hello första virtuella datorn, hello storlek kan skalas till, kan begränsas på grund av toohello tillgängligheten för hello andra storlekar i hello kluster aktuella virtuella datorn distribueras i. I hello publicerade automation-runbooks som används i denna artikel vi tar hand om det här fallet och kan endast skalas inom hello nedan par för VM-storlek. Det innebär att en virtuell dator för Standard_D1v2 inte plötsligt vara som skalats ut tooStandard_G5 eller skala ned tooBasic_A0.
> 
> | VM-storlekar skalning par |  |
> | --- | --- |
> | Basic_A0 |Basic_A4 |
> | Standard_A0 |Standard_A4 |
> | Standard_A5 |Standard_A7 |
> | Standard_A8 |Standard_A9 |
> | Standard_A10 |Standard_A11 |
> | Standard_D1 |Standard_D4 |
> | Standard_D11 |Standard_D14 |
> | Standard_DS1 |Standard_DS4 |
> | Standard_DS11 |Standard_DS14 |
> | Standard_D1v2 |Standard_D5v2 |
> | Standard_D11v2 |Standard_D14v2 |
> | Standard_G1 |Standard_G5 |
> | Standard_GS1 |Standard_GS5 |
> 
> 

## <a name="setup-azure-automation-tooaccess-your-virtual-machines"></a>Konfigurera Azure Automation tooaccess dina virtuella datorer
hello måste du först toodo är att skapa ett Azure Automation-konto som ska vara värd för hello runbooks används tooscale hello VM Scale Set instanser. Nyligen introducerade hello Automation-tjänsten hello ”kör som-konto” funktionen som gör att konfigurera hello tjänstens huvudnamn för att automatiskt köra hello runbooks på hello användares vägnar mycket enkelt. Du kan läsa mer om detta i hello artikel nedan:

* [Autentisera runbooks med ett ”Kör som”-konto i Azure](../../automation/automation-sec-configure-azure-runas-account.md)

## <a name="import-hello-azure-automation-vertical-scale-runbooks-into-your-subscription"></a>Importera hello Azure Automation Lodrät skala runbooks till din prenumeration
Hej runbooks som behövs för lodrätt skalning den virtuella datorn redan har publicerats i hello Azure Automation Runbook-galleriet. Du behöver tooimport dem i din prenumeration. Du kan lära dig hur tooimport runbooks genom att läsa hello följande artikel.

* [Azure Automation Runbook- och stänga](../../automation/automation-runbook-gallery.md)

Hej runbooks som måste importeras toobe visas i hello bilden nedan

![Importera runbooks](./media/vertical-scaling-automation/scale-runbooks.png)

## <a name="add-a-webhook-tooyour-runbook"></a>Lägga till en webhook tooyour runbook
När du har importerat hello runbooks måste tooadd en webhook toohello runbook så att den kan aktiveras av en avisering från en virtuell dator. hello information om hur du skapar en webhook för din Runbook kan läsas här

* [Azure Automation-webhooks](../../automation/automation-webhooks.md)

Kontrollera att du kopierar hello webhook innan du stänger hello webhook dialogrutan som du behöver det i nästa avsnitt om hello.

## <a name="add-an-alert-tooyour-virtual-machine"></a>Lägg till en avisering tooyour virtuell dator
1. Välj inställningar för virtuell dator
2. Välj ”Varningsregler”
3. Välj ”Lägg till avisering”
4. Välj en avisering om mått toofire hello på
5. Välj ett villkor som under uppfyllda kan orsaka hello avisering toofire
6. Välj ett tröskelvärde för hello villkoret i steg 5. toobe uppfyllda
7. Välj en period under vilken hello övervakningstjänsten kontrollerar för hello villkor och tröskelvärdet i steg 5 och 6
8. Klistra in i hello webhook som du kopierade från hello föregående avsnitt.

![Lägg till avisering tooVirtual Machine 1](./media/vertical-scaling-automation/add-alert-webhook-1.png)

![Lägg till avisering tooVirtual Machine 2](./media/vertical-scaling-automation/add-alert-webhook-2.png)

