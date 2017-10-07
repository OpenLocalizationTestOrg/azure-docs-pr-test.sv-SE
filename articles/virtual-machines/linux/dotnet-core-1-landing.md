---
title: aaaAzure Linux virtuella DotNet Core kursen 1 | Microsoft Docs
description: "Virtuella Azure-datorn DotNet grundläggande självstudierna"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: b3652e86-0c44-4ac9-8cd1-27abdeaea4d4
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/12/2017
ms.author: nepeters
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7e6f047197336de1e93c50413b6eabb718230bc1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="automating-application-deployments-toolinux-virtual-machines"></a>Automatisera program distributioner tooLinux virtuella datorer 

Den här fyra detaljerad information om att distribuera och konfigurera Azure-resurs och program med Azure Resource Manager-mallar. I den här serien en exempelmall distribueras och hello Distributionsmall undersökas. hello syftet med den här serien är tooeducate på hello relation mellan Azure-resurser och tooprovide praktiska erfarenhet av distribution helt integrerad Azure Resource Manager-mallar. Det här dokumentet förutsätter grundläggande kunskaper med Azure Resource Manager, innan du påbörjar den här självstudiekursen bekanta dig med grundläggande koncept i Azure Resource Manager. 

## <a name="music-store-application"></a>Musik store-programmet
hello prov som används i den här serien är en .net Core programmet simulera en musik Store shopping upplevelse. Det här programmet kan vara distribuerade tooeither ett Linux- eller Windows virtuella system, exempel distributioner har skapats för båda. hello program innehåller ett webbprogram och en SQL-databas. Distribuera hello programmet hello distribution knapp hittades på den här sidan innan du läser hello artiklarna i den här serien. När helt distribuerade hello program / Azure arkitektur ser ut som hello följande diagram. 

hello musik Store Resource Manager-mall finns här, [musik Store Linux mall](https://github.com/neilpeterson/nepeters-azure-templates/tree/master/dotnet-core-music-linux-vm-sql-db)

![Musik Store-programmet](./media/dotnet-core-1-landing/music-store.png)

Var och en av dessa komponenter, inklusive hello associera mallen JSON granskas i hello följande fyra artiklar.

* [**Programarkitektur** ](dotnet-core-2-architecture.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) – programkomponenter som webbplatser och databaser måste toobe finns på Azure datorresurser som virtuella datorer och Azure SQL-databaser. Det här dokumentet går igenom mappning beräkning behöver tooAzure resurser och distribuerar dessa resurser med en Azure Resource Manager-mall. 
* [**Åtkomst och säkerhet** ](dotnet-core-3-access-security.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) – när värd för program i Azure, är det nödvändigt tooconsider hur programmet hello används och hur olika programkomponenter åtkomst till varandra. Det här dokumentet beskriver tillhandahåller och skydda Internetprogram åtkomst tooan och åtkomst mellan programkomponenter.
* [**Tillgänglighet och skala** ](dotnet-core-4-availability-scale.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) – tillgänglighet och skala finns toohello program möjlighet toostay körs under infrastrukturen driftstopp och hello möjlighet tooscale beräkna resurser toomeet programmet begäran. Det här dokumentet information hello komponenter behövs toodeploy en belastningsutjämning och hög tillgänglighet program.
* [**Programdistribution** ](dotnet-core-5-app-deployment.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) – när du distribuera program på Azure Virtual Machines, hello-metoden som vilka hello binärfiler installeras på hello virtuell dator måste beaktas. Det här dokumentet beskriver automatiskt programinstallation med hjälp av Azure virtuella anpassade Skriptfilnamnstillägg.

hello målet när du utvecklar Azure Resource Manager-mallar är tooautomate hello distributionen av Azure-infrastrukturen och hello installationen och konfigurationen av alla program som finns på den här Azure-infrastrukturen. Gå igenom dessa artiklar innehåller ett exempel på den här upplevelsen.

## <a name="deploy-hello-music-store-application"></a>Distribuera hello musik store-programmet
hello musik Store-programmet kan distribueras med den här knappen.

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FMicrosoft%2Fdotnet-core-sample-templates%2Fmaster%2Fdotnet-core-music-linux%2Fazuredeploy.json" target="_blank"> <img src="http://azuredeploy.net/deploybutton.png"/>
</a>

hello Azure Resource Manager-mall kräver hello följande parametervärden.

| Parameternamn | Beskrivning |
| --- | --- |
| SSHKEYDATA |SSH viktiga data används toosecure åtkomst toohello virtuella datorn. Information om hur du skapar en SSH-nyckel finns [skapa SSH-nycklar för Linux virtuella datorer i Azure](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). |
| ADMINUSERNAME |Admin-användarnamn som används på hello virtuell dator och hello Azure SQL Database. |
| SQLADMINPASSWORD |Lösenordet som används på hello Azure SQL Database. |
| NUMBEROFINSTANCES |hello antal virtuella datorer toobe skapas. Var och en av dessa virtuella datorer värden hello musik Store webbprogram och all trafik är belastningsutjämnas mellan dem. |
| PUBLICIPADDRESSDNSNAME |Globalt unikt DNS-namnet som associeras med hello offentliga IP-adress. |

När hello malldistribution har slutförts, bläddra toohello offentliga IP-adressen med hjälp av en webbläsare. hello .net Core musik platsen visas.

## <a name="next-steps"></a>Nästa steg
<hr>

[Steg 1 – programarkitektur med Azure Resource Manager-mallar](dotnet-core-2-architecture.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[Steg 2 – åtkomst och säkerhet i Azure Resource Manager-mallar](dotnet-core-3-access-security.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[Steg 3 – tillgänglighet och skala i Azure Resource Manager-mallar](dotnet-core-4-availability-scale.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[Steg 4 – programdistribution med Azure Resource Manager-mallar](dotnet-core-5-app-deployment.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

