---
title: aaaManage Azure Cloud Services med Azure Automation | Microsoft Docs
description: "Läs mer om hur hello Azure Automation-tjänsten kan vara används toomanage Azure-molntjänster i större skala."
services: cloud-services, automation
documentationcenter: 
author: jodoglevy
manager: timlt
editor: 
ms.assetid: 3789810a-2892-4eef-bf29-c781c1b5af48
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2016
ms.author: timlt
ms.openlocfilehash: 8e920fb94955466bfec71cc332444f5f0ee497a7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="managing-azure-cloud-services-using-azure-automation"></a>Hantera Azure Cloud Services med Azure Automation
Den här guiden beskrivs toohello Azure Automation-tjänsten och hur det kan vara används toosimplify hantering av Azure-molntjänster.

## <a name="what-is-azure-automation"></a>Vad är Azure Automation?
[Azure Automation](https://azure.microsoft.com/services/automation/) är en Azure-tjänst som förenklar molnhantering via automatisering. Med Azure Automation kan tidskrävande, manuell, felbenägna och ofta återkommande uppgifter vara automatisk tooincrease tillförlitlighet, effektivitet och tid toovalue för din organisation.

Azure Automation ger en Körningsmotor för arbetsflöden för hög tillförlitliga och hög tillgänglighet som kan skalas toomeet dina behov när organisationen växer. I Azure Automation kan processer vara inletts manuellt av 3 part eller med schemalagda intervall så att aktiviteter inträffa exakt vid behov.

Lägre operativ tillsyn och frigöra IT / DevOps personal toofocus på arbete som lägger till affärsvärde genom att flytta dina molntjänster management uppgifter toobe körs automatiskt av Azure Automation.

## <a name="how-can-azure-automation-help-manage-azure-cloud-services"></a>Hur Azure Automation kan hantera Azure-molntjänster?
Azure-molntjänster kan hanteras i Azure Automation med hello PowerShell-cmdlets som är tillgängliga i hello [Azure PowerShell verktygen](https://msdn.microsoft.com/library/azure/jj156055.aspx). Azure Automation har dessa moln PowerShell cmdlet: ar tillgängliga out of box hello, så att du kan utföra alla cloud service management aktiviteter inom hello-tjänsten. Du kan också koppla dessa cmdlets i Azure Automation med hello-cmdlets för andra Azure-tjänster, tooautomate komplicerade uppgifter över Azure-tjänster och 3 part.

Vissa exempel som används av Azure Automation-toomanage Azure Cloud Services inkluderar:

* [Kontinuerlig distribution av en tjänst i molnet när cscfg eller cspkg uppdateras i Azure Blob storage](https://gallery.technet.microsoft.com/scriptcenter/Continuous-Deployment-of-A-eeebf3a6)
* [Starta om Molntjänsten instanser parallellt, en domän i taget](https://gallery.technet.microsoft.com/scriptcenter/Reboot-Cloud-Service-PaaS-b337a06d)

## <a name="next-steps"></a>Nästa steg
Nu när du har lärt dig grunderna hello i Azure Automation och hur det kan vara används toomanage Azure-molntjänster, ska du följa dessa länkar toolearn mer om Azure Automation.

* [Azure Automation-översikt](../automation/automation-intro.md)
* [Min första Runbook](../automation/automation-first-runbook-graphical.md)
* [Azure Automation-inlärningsguide](https://azure.microsoft.com/documentation/learning-paths/automation/)
