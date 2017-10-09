---
title: "aaaConfigure alltid på tillgänglighet Tillgänglighetsgruppslyssnarnas – Microsoft Azure | Microsoft Docs"
description: "Konfigurera tillgänglighet Tillgänglighetsgruppslyssnarnas på hello Azure Resource Manager-modellen, med en intern belastningsutjämnare med en eller flera IP-adresser."
services: virtual-machines
documentationcenter: na
author: MikeRayMSFT
manager: jhubbard
editor: monicar
ms.assetid: 14b39cde-311c-4ddf-98f3-8694e01a7d3b
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/22/2017
ms.author: mikeray
ms.openlocfilehash: 81edfe2c2ea536d8dcec466f36fccf8bc0e02c2d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-one-or-more-always-on-availability-group-listeners---resource-manager"></a>Konfigurera en eller flera alltid på tillgänglighet tillgänglighetsgruppslyssnarnas - Resource Manager
Det här avsnittet beskrivs hur du:

* Skapa en intern belastningsutjämnare för SQL Server-Tillgänglighetsgrupper med PowerShell-cmdlets.
* Lägga till ytterligare IP-adresser tooa-belastningsutjämnare för mer än en tillgänglighetsgrupp. 

En tillgänglighetsgruppslyssnare är ett namn för virtuellt nätverk att klienter ansluter toofor åtkomst till databasen. På Azure virtual machines innehåller en belastningsutjämnare hello IP-adress för hello-lyssnaren. hello belastningen belastningsutjämnaren vägar trafik toohello instans av SQL Server som lyssnar på hello avsökningsport. En tillgänglighetsgrupp använder vanligtvis en intern belastningsutjämnare. En Azure intern belastningsutjämnare kan vara värd för en eller flera IP-adresser. Varje IP-adress använder en specifik avsökningsport. Det här dokumentet beskrivs hur toouse PowerShell toocreate en belastningsutjämnare eller Lägg till IP-adresser tooan befintliga belastningsutjämnaren för SQL Server-Tillgänglighetsgrupper. 

Hej möjlighet tooassign flera IP-adresser tooan intern belastningsutjämnare är ny tooAzure och finns bara i Resource Manager-modellen. toocomplete den här uppgiften måste toohave en tillgänglighetsgrupp för SQL Server distribueras på virtuella Azure-datorer i Resource Manager-modellen. Både SQL Server-datorer måste tillhöra toohello samma tillgänglighetsuppsättning. Du kan använda hello [Microsoft mall](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) tooautomatically skapa hello tillgänglighetsgruppen i Azure Resource Manager. Den här mallen skapar automatiskt hello tillgänglighetsgruppen, inklusive hello intern belastningsutjämnare för dig. Om du vill kan du [manuellt konfigurera en Always On-tillgänglighetsgrupp](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).

Det här avsnittet kräver att din Tillgänglighetsgrupper har redan konfigurerats.  

Närliggande ämnen innefattar:

* [Konfigurera AlwaysOn-Tillgänglighetsgrupper på Azure VM (GUI)](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md)   
* [Konfigurera en VNet-till-VNet-anslutning med hjälp av Azure Resource Manager och PowerShell](../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)

[!INCLUDE [Start your PowerShell session](../../../../includes/sql-vm-powershell.md)]

## <a name="configure-hello-windows-firewall"></a>Konfigurera hello Windows-brandväggen
Konfigurera hello Windows-brandväggen tooallow SQL Server-åtkomst. hello brandväggsreglerna tillåter TCP-anslutningar toohello-portar som används av hello SQL Server-instansen och hello lyssnare avsökning. Detaljerade instruktioner finns [konfigurera Windows-brandväggen för Databasmotoråtkomst](http://msdn.microsoft.com/library/ms175043.aspx#Anchor_1). Skapa en regel för inkommande trafik för hello SQL Server-porten och hello avsökningsport.

## <a name="example-script-create-an-internal-load-balancer-with-powershell"></a>Exempelskript: Skapa en intern belastningsutjämnare med PowerShell
> [!NOTE]
> Om du har skapat tillgänglighetsgruppen med hello [Microsoft mall](virtual-machines-windows-portal-sql-alwayson-availability-groups.md), hello intern belastningsutjämnare har redan skapats. 
> 
> 

hello följande PowerShell-skript skapar en intern belastningsutjämnare, konfigurerar hello belastningsutjämningsregler och anger en IP-adress för hello belastningsutjämnaren. toorun hello skript, öppna Windows PowerShell ISE och klistra in hello skript i hello skriptfönster. Använd `Login-AzureRMAccount` toolog i tooPowerShell. Om du har flera Azure-prenumerationer, Använd `Select-AzureRmSubscription ` tooset hello prenumeration. 

```powershell
# Login-AzureRmAccount
# Select-AzureRmSubscription -SubscriptionId <xxxxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx>

$ResourceGroupName = "<Resource Group Name>" # Resource group name
$VNetName = "<Virtual Network Name>"         # Virtual network name
$SubnetName = "<Subnet Name>"                # Subnet name
$ILBName = "<Load Balancer Name>"            # ILB name
$Location = "<Azure Region>"                 # Azure location
$VMNames = "<VM1>","<VM2>"                   # Virtual machine names

$ILBIP = "<n.n.n.n>"                         # IP address
[int]$ListenerPort = "<nnnn>"                # AG listener port
[int]$ProbePort = "<nnnn>"                   # Probe port

$LBProbeName ="ILBPROBE_$ListenerPort"       # hello Load balancer Probe Object Name              
$LBConfigRuleName = "ILBCR_$ListenerPort"    # hello Load Balancer Rule Object Name

$FrontEndConfigurationName = "FE_SQLAGILB_1" # Object name for hello front-end configuration 
$BackEndConfigurationName ="BE_SQLAGILB_1"   # Object name for hello back-end configuration

$VNet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName 

$Subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $VNet -Name $SubnetName 

$FEConfig = New-AzureRMLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -PrivateIpAddress $ILBIP -SubnetId $Subnet.id

$BEConfig = New-AzureRMLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName 

$SQLHealthProbe = New-AzureRmLoadBalancerProbeConfig -Name $LBProbeName -Protocol tcp -Port $ProbePort -IntervalInSeconds 15 -ProbeCount 2

$ILBRule = New-AzureRmLoadBalancerRuleConfig -Name $LBConfigRuleName -FrontendIpConfiguration $FEConfig -BackendAddressPool $BEConfig -Probe $SQLHealthProbe -Protocol tcp -FrontendPort $ListenerPort -BackendPort $ListenerPort -LoadDistribution Default -EnableFloatingIP 

$ILB= New-AzureRmLoadBalancer -Location $Location -Name $ILBName -ResourceGroupName $ResourceGroupName -FrontendIpConfiguration $FEConfig -BackendAddressPool $BEConfig -LoadBalancingRule $ILBRule -Probe $SQLHealthProbe 

$bepool = Get-AzureRmLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName -LoadBalancer $ILB 

foreach($VMName in $VMNames)
    {
        $VM = Get-AzureRmVM -ResourceGroupName $ResourceGroupName -Name $VMName 
        $NICName = ($vm.NetworkProfile.NetworkInterfaces.Id.split('/') | select -last 1)
        $NIC = Get-AzureRmNetworkInterface -name $NICName -ResourceGroupName $ResourceGroupName
        $NIC.IpConfigurations[0].LoadBalancerBackendAddressPools = $BEPool
        Set-AzureRmNetworkInterface -NetworkInterface $NIC
        start-AzureRmVM -ResourceGroupName $ResourceGroupName -Name $VM.Name 
    }
```

## <a name="Add-IP"></a>Exempelskript: lägga till en IP-adress tooan befintliga belastningsutjämnare med PowerShell
Lägg till en ytterligare belastningsutjämnare för IP-adress toohello toouse mer än en tillgänglighetsgrupp. Varje IP-adress kräver sin egen belastningsutjämning regeln, avsökningsport och främre port.

hello frontend porten är hello att program använder tooconnect toohello SQL Server-instansen. IP-adresser för olika tillgänglighet grupper kan använda hello samma frontend-port.

> [!NOTE]
> För SQL Server-Tillgänglighetsgrupper kräver varje IP-adress en viss avsökningsport. Om en IP-adress för en belastningsutjämnare använder avsökningsport 59999, kan inga andra IP-adresser på den belastningsutjämnaren använda avsökningsport 59999.

* Information om belastningen belastningsutjämnaren gränser finns **klientdelen privata IP-adress per belastningsutjämnaren** under [nätverk gränser - Azure Resource Manager](../../../azure-subscription-service-limits.md#azure-resource-manager-virtual-networking-limits).
* Information om tillgänglighet grupp gränser finns [begränsningar (Tillgänglighetsgrupper)](http://msdn.microsoft.com/library/ff878487.aspx#RestrictionsAG).

hello följande skript lägger till en ny IP-adress tooan befintliga belastningsutjämnare. Hej ILB använder hello lyssningsport för hello frontend-port för belastningsutjämning. Den här porten kan vara hello-porten som SQL-servern lyssnar på. För standardinstanser av SQL Server är hello port 1433. Hej belastningsutjämningsregeln för en tillgänglighetsgrupp kräver en flytande IP (direkt serverreturnering) så hello backend-porten är hello samma som hello frontend-port. Uppdatera hello variabler för din miljö. 

```powershell
# Login-AzureRmAccount
# Select-AzureRmSubscription -SubscriptionId <xxxxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx>

$ResourceGroupName = "<ResourceGroup>"          # Resource group name
$VNetName = "<VirtualNetwork>"                  # Virtual network name
$SubnetName = "<Subnet>"                        # Subnet name
$ILBName = "<ILBName>"                          # ILB name                      

$ILBIP = "<n.n.n.n>"                            # IP address
[int]$ListenerPort = "<nnnn>"                   # AG listener port
[int]$ProbePort = "<nnnnn>"                     # Probe port 

$ILB = Get-AzureRmLoadBalancer -Name $ILBName -ResourceGroupName $ResourceGroupName 

$count = $ILB.FrontendIpConfigurations.Count+1
$FrontEndConfigurationName ="FE_SQLAGILB_$count"  

$LBProbeName = "ILBPROBE_$count"
$LBConfigrulename = "ILBCR_$count"

$VNet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName 
$Subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $VNet -Name $SubnetName

$ILB | Add-AzureRmLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -PrivateIpAddress $ILBIP -SubnetId $Subnet.Id 

$ILB | Add-AzureRmLoadBalancerProbeConfig -Name $LBProbeName  -Protocol Tcp -Port $Probeport -ProbeCount 2 -IntervalInSeconds 15  | Set-AzureRmLoadBalancer 

$ILB = Get-AzureRmLoadBalancer -Name $ILBname -ResourceGroupName $ResourceGroupName

$FEConfig = get-AzureRMLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -LoadBalancer $ILB

$SQLHealthProbe  = Get-AzureRmLoadBalancerProbeConfig -Name $LBProbeName -LoadBalancer $ILB

$BEConfig = Get-AzureRmLoadBalancerBackendAddressPoolConfig -Name $ILB.BackendAddressPools[0].Name -LoadBalancer $ILB 

$ILB | Add-AzureRmLoadBalancerRuleConfig -Name $LBConfigRuleName -FrontendIpConfiguration $FEConfig  -BackendAddressPool $BEConfig -Probe $SQLHealthProbe -Protocol tcp -FrontendPort  $ListenerPort -BackendPort $ListenerPort -LoadDistribution Default -EnableFloatingIP | Set-AzureRmLoadBalancer   
```

## <a name="configure-hello-listener"></a>Konfigurera hello-lyssnare

[!INCLUDE [ag-listener-configure](../../../../includes/virtual-machines-ag-listener-configure.md)]

## <a name="set-hello-listener-port-in-sql-server-management-studio"></a>Ange hello lyssningsport i SQL Server Management Studio

1. Starta SQL Server Management Studio och Anslut toohello primära repliken.

1. Navigera för**AlwaysOn hög tillgänglighet** | **Tillgänglighetsgrupper** | **tillgänglighet Tillgänglighetsgruppslyssnarnas**. 

1. Du bör nu se hello grupplyssnarens namn som du skapade i hanteraren för redundanskluster. Högerklicka på hello grupplyssnarens namn och på **egenskaper**.

1. I hello **Port** ange hello portnummer för hello tillgänglighetsgruppens lyssnare med hjälp av hello $EndpointPort du använde tidigare (1433 var hello standard), klicka på **OK**.

## <a name="test-hello-connection-toohello-listener"></a>Testa hello anslutning toohello lyssnare

tootest hello anslutning:

1. RDP-tooa SQL Server som är i hello samma virtuella nätverk, men inte egna hello replik. Detta kan vara hello andra SQLServer i hello-klustret.

1. Använd **sqlcmd** verktyget tootest hello anslutning. Till exempel följande skript hello upprättar en **sqlcmd** anslutning toohello primära repliken via hello lyssnare med Windows-autentisering:
   
    ```
    sqlmd -S <listenerName> -E
    ```
   
    Om hello lyssnare använder en port än hello standard port (1433), ange hello port i hello anslutningssträngen. Till exempel ansluter hello följande kommando med sqlcmd tooa lyssnaren på port 1435: 
   
    ```
    sqlcmd -S <listenerName>,1435 -E
    ```

hello SQLCMD anslutning ansluter automatiskt toowhichever instans av SQL Server-värdar hello primära repliken. 

> [!NOTE]
> Kontrollera att hello-port som du anger är öppen hello-brandväggen för både SQL-servrar. Båda servrarna kräver en inkommande regel för hello TCP-port som du använder. Se [Lägg till eller redigera brandväggsregel](http://technet.microsoft.com/library/cc753558.aspx) för mer information. 
> 
> 

## <a name="guidelines-and-limitations"></a>Riktlinjer och begränsningar
Observera följande riktlinjer för tillgänglighetsgruppens lyssnare i Azure med interna belastningsutjämnare hello:

* En intern belastningsutjämnare du bara komma åt hello-lyssnaren från hello samma virtuella nätverk.


## <a name="for-more-information"></a>Mer information
Mer information finns i [konfigurera alltid på tillgänglighetsgruppen i Azure VM manuellt](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).

## <a name="powershell-cmdlets"></a>PowerShell-cmdletar
Använd hello följande PowerShell-cmdlets toocreate en intern belastningsutjämnare för virtuella Azure-datorer.

* [Nya AzureRmLoadBalancer](http://msdn.microsoft.com/library/mt619450.aspx) skapar en belastningsutjämnare. 
* [Nya AzureRMLoadBalancerFrontendIpConfig](http://msdn.microsoft.com/library/mt603510.aspx) skapar en frontend IP-konfiguration för en belastningsutjämnare. 
* [Nya AzureRmLoadBalancerRuleConfig](http://msdn.microsoft.com/library/mt619391.aspx) skapar en Regelkonfiguration för en belastningsutjämnare. 
* [Nya AzureRmLoadBalancerBackendAddressPoolConfig](http://msdn.microsoft.com/library/mt603791.aspx) skapar en backend-pool-adresskonfiguration för en belastningsutjämnare. 
* [Nya AzureRmLoadBalancerProbeConfig](http://msdn.microsoft.com/library/mt603847.aspx) skapar en avsökning konfiguration för en belastningsutjämnare.
* [Ta bort AzureRmLoadBalancer](http://msdn.microsoft.com/library/mt603862.aspx) tar bort en belastningsutjämnare från en Azure-resursgrupp.
