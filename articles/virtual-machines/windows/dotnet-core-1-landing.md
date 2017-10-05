---
title: "Windows Azure virtuella DotNet Core självstudiekursen 1 | Microsoft Docs"
description: "Virtuella Azure-datorn DotNet grundläggande självstudierna"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 14d5f250-1f76-49d4-898f-07b58fd39e7c
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 05/12/2017
ms.author: nepeters
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: bfb3a27d20e8cdcff8dff75e4dfb2685e2781d45
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="automating-application-deployments-to-windows-virtual-machines"></a>Automatisera distribution av program till Windows-datorer

Den här fyra detaljerad information om att distribuera och konfigurera Azure-resurs och program med Azure Resource Manager-mallar. I den här serien en exempelmall distribueras och distributionsmallen undersökas. Syftet med den här serien är att informera om förhållandet mellan Azure-resurser och ge händerna på erfarenhet av distribution helt integrerad Azure Resource Manager-mallar. Det här dokumentet förutsätter grundläggande kunskaper med Azure Resource Manager, innan du påbörjar den här självstudiekursen bekanta dig med grundläggande koncept i Azure Resource Manager.

## <a name="music-store-application"></a>Musik store-programmet
Prov som används i den här serien är en .net Core programmet simulera en musik Store shopping upplevelse. Det här programmet kan distribueras till en Linux- eller Windows virtuell dator, exempel distributioner har skapats för båda. Programmet innehåller ett webbprogram och en SQL-databas. Distribuera programmet med hjälp av knappen distribution på den här sidan innan du läser artiklar i den här serien. När helt distribuerade program / Azure-arkitektur som ser ut som i följande diagram. 

Musik Store Resource Manager-mallen finns här, [musik Store Windows-mall](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows)

![Musik Store-programmet](./media/dotnet-core-1-landing/music-store.png)

Var och en av dessa komponenter, inklusive associera mallens JSON undersöks i följande fyra artiklar.

* [**Programarkitektur** ](dotnet-core-2-architecture.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) – programkomponenter som webbplatser och databaser måste finnas på Azure datorresurser som virtuella datorer och Azure SQL-databaser. Det här dokumentet går igenom mappning beräkning behovet att Azure-resurser och distribuerar dessa resurser med en Azure Resource Manager-mall. 
* [**Åtkomst och säkerhet** ](dotnet-core-3-access-security.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) – när värd för program i Azure, är det nödvändigt att överväga hur programmet används och hur olika komponenter programåtkomst varandra. Det här dokumentet beskriver tillhandahåller och skydda internet tillgång till ett program och mellan programkomponenter.
* [**Tillgänglighet och skala** ](dotnet-core-4-availability-scale.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) – tillgänglighet och skala avser program möjligheten att hålla körs under infrastrukturen avbrottstid och möjlighet att skala beräkningsresurser för att uppfylla begäran för program. Det här dokumentet information de komponenter som krävs för att distribuera en belastningsutjämnad och hög tillgänglighet program.
* [**Programdistribution** ](dotnet-core-5-app-deployment.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) – när du distribuerar program till Azure Virtual Machines, metoden som programbinärer är installerade på den virtuella datorn måste beaktas. Det här dokumentet beskriver automatiskt programinstallation med hjälp av Azure virtuella anpassade Skriptfilnamnstillägg.

Målet när du utvecklar Azure Resource Manager-mallar är att automatisera distributionen av Azure-infrastrukturen och installationen och konfigurationen av alla program som finns på den här Azure-infrastrukturen. Gå igenom dessa artiklar innehåller ett exempel på den här upplevelsen.

## <a name="deploy-the-music-store-application"></a>Distribuera musik store-programmet
Musik Store-programmet kan distribueras med den här knappen.

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FMicrosoft%2Fdotnet-core-sample-templates%2Fmaster%2Fdotnet-core-music-windows%2Fazuredeploy.json" target="_blank"> <img src="http://azuredeploy.net/deploybutton.png"/>
</a>

Azure Resource Manager-mallen kräver följande parametervärden.

| Parameternamn | Beskrivning |
| --- | --- |
| ADMINUSERNAME |Admin-användarnamn som används på den virtuella datorn och Azure SQL-databasen. |
| ADMINPASSWORD |Lösenordet som används på Azure-dator och SQL-databas. |
| NUMBEROFINSTANCES |Antal virtuella datorer som ska skapas. Var och en av dessa virtuella datorer värden musik Store webbprogram och all trafik är belastningsutjämnas mellan dem. |
| PUBLICIPADDRESSDNSNAME |Globalt unikt DNS-namn som är associerade med den offentliga IP-adressen. |

När malldistributionen är klar, bläddra till den offentliga IP-adressen med hjälp av en webbläsare. .Net Core musik platsen visas.

## <a name="next-steps"></a>Nästa steg
<hr>

[Steg 1 – programarkitektur med Azure Resource Manager-mallar](dotnet-core-2-architecture.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[Steg 2 – åtkomst och säkerhet i Azure Resource Manager-mallar](dotnet-core-3-access-security.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[Steg 3 – tillgänglighet och skala i Azure Resource Manager-mallar](dotnet-core-4-availability-scale.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[Steg 4 – programdistribution med Azure Resource Manager-mallar](dotnet-core-5-app-deployment.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

