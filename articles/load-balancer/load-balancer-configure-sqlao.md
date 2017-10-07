---
title: "aaaConfigure belastningsutjämnaren för SQL alltid på | Microsoft Docs"
description: "Konfigurera Load balancer toowork med SQL alltid på och hur tooleverage powershell toocreate belastningsutjämnare för hello SQL-implementering"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
ms.assetid: d7bc3790-47d3-4e95-887c-c533011e4afd
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/24/2016
ms.author: kumud
ms.openlocfilehash: ac6200b18f725dadee2555b80055327d379417d4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-load-balancer-for-sql-always-on"></a>Konfigurera belastningsutjämning för SQL alltid på

SQL Server AlwaysOn-Tillgänglighetsgrupper kan nu köra med ILB. Tillgänglighetsgruppen är SQL Server bästa lösning för hög tillgänglighet och haveriberedskap. hello tillgänglighetsgruppens lyssnare gör att klientprogram tooseamlessly ansluta toohello primära repliken, oavsett hello antal hello repliker i hello konfiguration.

hello lyssnare (DNS) namn är mappade tooa belastningsutjämnad IP-adress och belastningsutjämnaren för Azures dirigerar hello inkommande trafik tooonly hello primära servern i hello replikuppsättningen.

Du kan använda ILB stöd för SQL Server AlwaysOn (lyssnare) slutpunkter. Du har kontroll över hello hjälpmedel för hello lyssnare nu och kan välja hello belastningsutjämnad IP-adress från ett specifikt undernät i ditt virtuella nätverk (VNet).

Hello med ILB hello-lyssnare för slutpunkten för SQL server (t.ex. Server = tcp:ListenerName, 1433; Database = DatabaseName) endast är tillgänglig för:

* Tjänster och virtuella datorer i hello samma virtuella nätverk
* Tjänster och virtuella datorer från anslutna lokalt nätverk
* Tjänster och virtuella datorer från sammankopplade Vnet

![ILB_SQLAO_NewPic](./media/load-balancer-configure-sqlao/sqlao1.png)

Bild 1 – SQL AlwaysOn har konfigurerats med Internetriktade belastningsutjämnare

## <a name="add-internal-load-balancer-toohello-service"></a>Lägga till interna belastningsutjämnare toohello tjänst

1. I följande exempel hello, kommer vi konfigurera ett virtuellt nätverk som innehåller ett undernät som kallas Undernät-1:

    ```powershell
    Add-AzureInternalLoadBalancer -InternalLoadBalancerName ILB_SQL_AO -SubnetName Subnet-1 -ServiceName SqlSvc
    ```
2. Lägga till belastningsutjämnade slutpunkter för ILB på varje virtuell dator

    ```powershell
    Get-AzureVM -ServiceName SqlSvc -Name sqlsvc1 | Add-AzureEndpoint -Name "LisEUep" -LBSetName "ILBSet1" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 -
    DirectServerReturn $true -InternalLoadBalancerName ILB_SQL_AO | Update-AzureVM

    Get-AzureVM -ServiceName SqlSvc -Name sqlsvc2 | Add-AzureEndpoint -Name "LisEUep" -LBSetName "ILBSet1" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 -DirectServerReturn $true -InternalLoadBalancerName ILB_SQL_AO | Update-AzureVM
    ```

    Hello-exemplet ovan kan du har 2 VM kallas ”sqlsvc1” och ”sqlsvc2” som körs i molnet hello-tjänst ”Sqlsvc”. När du har skapat hello ILB med `DirectServerReturn` växlar kan du lägga till ladda belastningsutjämnade slutpunkter toohello ILB tooallow SQL tooconfigure hello-lyssnare för hello Tillgänglighetsgrupper.

Mer information om SQL AlwaysOn finns [konfigurera en intern belastningsutjämnare för en AlwaysOn-tillgänglighetsgrupp i Azure](../virtual-machines/windows/sql/virtual-machines-windows-portal-sql-alwayson-int-listener.md).

## <a name="see-also"></a>Se även
[Kom igång med att konfigurera en Internetuppkopplad belastningsutjämnare](load-balancer-get-started-internet-arm-ps.md)

[Kom igång med att konfigurera en intern belastningsutjämnare](load-balancer-get-started-ilb-arm-ps.md)

[Konfigurera ett distributionsläge för belastningsutjämnare](load-balancer-distribution-mode.md)

[Konfigurera timeout-inställningar för inaktiv TCP för en belastningsutjämnare](load-balancer-tcp-idle-timeout.md)
