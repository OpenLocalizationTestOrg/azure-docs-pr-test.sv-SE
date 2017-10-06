---
title: "aaaCreate en Internetriktade belastningsutjämnare - Azure PowerShell klassiska | Microsoft Docs"
description: "Lär dig hur toocreate Internet-riktade belastningsutjämnare i klassiskt läge med hjälp av PowerShell"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
tags: azure-service-management
ms.assetid: 73e8bfa4-8086-4ef0-9e35-9e00b24be319
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: 76d9a712a0acda223fc86b80be9c35c0ed9f3a50
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-creating-an-internet-facing-load-balancer-classic-in-powershell"></a>Komma igång med att skapa en Internetuppkopplad belastningsutjämnare (klassisk) i PowerShell

> [!div class="op_single_selector"]
> * [Klassisk Azure-portal](../load-balancer/load-balancer-get-started-internet-classic-portal.md)
> * [PowerShell](../load-balancer/load-balancer-get-started-internet-classic-ps.md)
> * [Azure CLI](../load-balancer/load-balancer-get-started-internet-classic-cli.md)
> * [Azure Cloud Services](../load-balancer/load-balancer-get-started-internet-classic-cloud.md)

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

> [!IMPORTANT]
> Innan du börjar arbeta med Azure-resurser, är det viktigt toounderstand att Azure har två distributionsmodeller: Azure Resource Manager och klassisk. Se till att du förstår [distributionsmodeller och verktyg](../azure-classic-rm.md) innan du börjar arbeta med Azure-resurser. Du kan visa hello dokumentationen för olika verktyg genom att klicka på flikarna hello hello överst i den här artikeln. Den här artikeln beskriver hello klassiska distributionsmodellen. Du kan också [Lär dig hur toocreate Internet-riktade belastningsutjämnaren med Azure Resource Manager](load-balancer-get-started-internet-arm-ps.md).

[!INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="set-up-load-balancer-using-powershell"></a>Konfigurera en belastningsutjämnare med hjälp av PowerShell

tooset in belastningsutjämning med hjälp av powershell så hello nedan:

1. Om du aldrig har använt Azure PowerShell, se [hur tooInstall och konfigurera Azure PowerShell](/powershell/azure/overview) och följer instruktionerna för hello alla hello sätt toohello avslutas toosign till Azure och välja din prenumeration.
2. När du har skapat en virtuell dator kan du använda PowerShell-cmdlets tooadd en load balancer tooa virtuell dator i hello samma molntjänst.

I hello följande exempel lägger du till en belastningen belastningsutjämnaren uppsättning kallas ”webbservergrupp” toocloud service ”mytestcloud” (eller myctestcloud.cloudapp.net), lägger till hello slutpunkter för hello läsa in belastningsutjämning toovirtual datorer kallas ”web1” och ”web2”. hello belastningsutjämnaren tar emot trafik på port 80 och belastningsutjämning mellan hello virtuella datorer som definieras av hello lokal slutpunkt (i det här fallet port 80) med TCP.

### <a name="step-1"></a>Steg 1

Skapa en belastningsutjämnad slutpunkt för web1 ”hello första VM”

```powershell
Get-AzureVM -ServiceName "mytestcloud" -Name "web1" | Add-AzureEndpoint -Name "HttpIn" -Protocol "tcp" -PublicPort 80 -LocalPort 80 -LBSetName "WebFarm" -ProbePort 80 -ProbeProtocol "http" -ProbePath '/' | Update-AzureVM
```

### <a name="step-2"></a>Steg 2

Skapa en annan slutpunkt för hello andra VM ”web2” med hjälp av hello samma läsa in belastningsutjämning namn

```powershell
Get-AzureVM -ServiceName "mytestcloud" -Name "web2" | Add-AzureEndpoint -Name "HttpIn" -Protocol "tcp" -PublicPort 80 -LocalPort 80 -LBSetName "WebFarm" -ProbePort 80 -ProbeProtocol "http" -ProbePath '/' | Update-AzureVM
```

## <a name="remove-a-virtual-machine-from-a-load-balancer"></a>Ta bort en virtuell dator från en belastningsutjämnare

Du kan använda Remove-AzureEndpoint tooremove virtuell datorslutpunkt från hello belastningsutjämnaren

```powershell
Get-azureVM -ServiceName mytestcloud  -Name web1 |Remove-AzureEndpoint -Name httpin | Update-AzureVM
```

## <a name="next-steps"></a>Nästa steg

Du kan också läsa mer om hur du [kommer igång med att skapa en intern belastningsutjämnare](load-balancer-get-started-ilb-classic-ps.md) och hur du konfigurerar typen av [distributionsläge](load-balancer-distribution-mode.md) för ett specifikt trafikbeteende i ett belastningsutjämnat nätverk.

Om ditt program måste tookeep anslutningar alive för servrar bakom en belastningsutjämnare, du veta mer om [inaktiv TCP timeout-inställningar för en belastningsutjämnare](load-balancer-tcp-idle-timeout.md). Det hjälper toolearn om inaktiv anslutning beteende när du använder Azure belastningsutjämnare.
