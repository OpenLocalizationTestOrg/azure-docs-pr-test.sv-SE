---
title: "aaaCreate en multi-SID-konfiguration för SAP i Azure | Microsoft Docs"
description: "Guiden toohigh tillgänglighet SAP NetWeaver multi-SID-konfigurationen på Windows-datorer"
services: virtual-machines-windows, virtual-network, storage
documentationcenter: saponazure
author: goraco
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: 0b89b4f8-6d6c-45d7-8d20-fe93430217ca
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 05/05/2017
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e2a4b12928231743c59af86dab72595ad38d364b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-sap-netweaver-multi-sid-configuration"></a>Skapa en konfiguration för SAP NetWeaver multi-SID

[load-balancer-multivip-overview]:../../../load-balancer/load-balancer-multivip-overview.md
[sap-ha-guide]:sap-high-availability-guide.md 
[sap-ha-guide-figure-6001]:./media/virtual-machines-shared-sap-high-availability-guide/6001-sap-multi-sid-ascs-scs-sid1.png
[sap-ha-guide-figure-6002]:./media/virtual-machines-shared-sap-high-availability-guide/6002-sap-multi-sid-ascs-scs.png
[sap-ha-guide-figure-6003]:./media/virtual-machines-shared-sap-high-availability-guide/6003-sap-multi-sid-full-landscape.png
[sap-ha-guide-figure-6004]:./media/virtual-machines-shared-sap-high-availability-guide/6004-sap-multi-sid-dns.png
[sap-ha-guide-figure-6005]:./media/virtual-machines-shared-sap-high-availability-guide/6005-sap-multi-sid-azure-portal.png
[sap-ha-guide-figure-6006]:./media/virtual-machines-shared-sap-high-availability-guide/6006-sap-multi-sid-sios-replication.png
[networking-limits-azure-resource-manager]:../../../azure-subscription-service-limits.md#azure-resource-manager-virtual-networking-limits
[sap-ha-guide-9.1.1]:sap-high-availability-guide.md#a97ad604-9094-44fe-a364-f89cb39bf097 
[sap-ha-guide-8.8]:sap-high-availability-guide.md#f19bd997-154d-4583-a46e-7f5a69d0153c
[sap-ha-guide-8.12.3.3]:sap-high-availability-guide.md#d9c1fc8e-8710-4dff-bec2-1f535db7b006 
[sap-ha-guide-9]:sap-high-availability-guide.md#a06f0b49-8a7a-42bf-8b0d-c12026c5746b
[sap-ha-guide-9.1]:sap-high-availability-guide.md#31c6bd4f-51df-4057-9fdf-3fcbc619c170 
[sap-ha-guide-9.1.2]:sap-high-availability-guide.md#eb5af918-b42f-4803-bb50-eff41f84b0b0 
[sap-ha-guide-9.1.3]:sap-high-availability-guide.md#e4caaab2-e90f-4f2c-bc84-2cd2e12a9556 
[sap-ha-guide-9.1.4]:sap-high-availability-guide.md#10822f4f-32e7-4871-b63a-9b86c76ce761 
[sap-ha-guide-9.4]:sap-high-availability-guide.md#094bc895-31d4-4471-91cc-1513b64e406a 
[sap-ha-guide-9.5]:sap-high-availability-guide.md#2477e58f-c5a7-4a5d-9ae3-7b91022cafb5 
[sap-ha-guide-9.6]:sap-high-availability-guide.md#0ba4a6c1-cc37-4bcf-a8dc-025de4263772 
[sap-ha-guide-10]:sap-high-availability-guide.md#18aa2b9d-92d2-4c0e-8ddd-5acaabda99e9

I September 2016 släppt Microsoft en funktion där du kan hantera flera virtuella IP-adresser med hjälp av en Azure intern belastningsutjämnare. Den här funktionen finns redan i hello Azure extern belastningsutjämnare.

Om du har en SAP-distribution, du kan använda en intern belastningsutjämnare toocreate en Windows-klusterkonfiguration för SAP ASCS/SCS, enligt beskrivningen i hello [guide för hög tillgänglighet SAP NetWeaver på virtuella Windows-datorer] [ sap-ha-guide].

Den här artikeln fokuserar på hur toomove från en enda ASCS/SCS installation tooan SAP multi-SID-konfigurationen genom att installera ytterligare SAP ASCS SCS klustrade instanser i en befintlig Windows Server Failover Clustering WSFC-kluster. När den här processen är slutförd kan har du konfigurerat ett SAP multi-SID-kluster.


## <a name="prerequisites"></a>Krav
Du redan har konfigurerat ett WSFC-kluster som används för en SAP ASCS/SCS-instans, enligt beskrivningen i hello [guide för hög tillgänglighet SAP NetWeaver på virtuella Windows-datorer] [ sap-ha-guide] och som visas i diagrammet.

![Hög tillgänglighet SAP ASCS/SCS instans][sap-ha-guide-figure-6001]

## <a name="target-architecture"></a>Mål-arkitektur

hello målet är tooinstall flera SAP ABAP ASCS eller SAP Java SCS klustrade instanser i hello samma WSFC-klustret, som visas här:

![Flera SAP ASCS/SCS klustrade instanser i Azure][sap-ha-guide-figure-6002]

> [!NOTE]
>Det finns en gräns toohello antal privata frontend IP-adresser för varje Azure interna belastningsutjämnare.
>
>hello maximalt antal SAP ASCS/SCS instanser i en WSFC-klustret är lika toohello maximalt antal privata frontend IP-adresser för varje Azure interna belastningsutjämnare.
>

Mer information om belastningsutjämnare begränsningar finns i ”privat frontend-IP per belastningsutjämnare i [nätverk gränser: Azure Resource Manager][networking-limits-azure-resource-manager].

hello fullständig liggande med två system med hög tillgänglighet SAP skulle se ut så här:

![SAP hög tillgänglighet multi-SID-installation med två SAP system SID][sap-ha-guide-figure-6003]

> [!IMPORTANT]
> hello installationen måste uppfylla följande villkor hello:
> - hello SAP ASCS/SCS instanser måste dela hello samma WSFC-klustret.
> - Varje DBMS SID måste ha sitt eget dedikerade WSFC-kluster.
> - SAP-programservrar som tillhör tooone SAP system SID måste ha sina egna dedikerade virtuella datorer.


## <a name="prepare-hello-infrastructure"></a>Förbereda hello-infrastrukturen
tooprepare din infrastruktur kan du installera en ytterligare SAP ASCS/SCS-instans med hello följande parametrar:

| Parameternamn | Värde |
| --- | --- |
| SAP ASCS/SCS SID |ascs-lb-PR1 |
| SAP DBMS intern belastningsutjämnare | PR5 |
| SAP virtuella värdnamn | pr5-sap-cl |
| SAP ASCS/SCS virtuell värd IP-adress (ytterligare Azure belastningsutjämnare IP-adress) | 10.0.0.50 |
| SAP ASCS/SCS instansnummer | 50 |
| ILB avsökningsport för ytterligare SAP ASCS/SCS-instans | 62350 |

> [!NOTE]
> För SAP ASCS/SCS-instanser kräver varje IP-adress en unik avsökningsport. Om en IP-adress på en Azure intern belastningsutjämnare använder avsökningsport 62300, kan inga andra IP-adressen på den belastningsutjämnaren använda avsökningsport 62300.
>
>För våra ändamål eftersom avsökningsport 62300 har redan reserverats, använder du avsökningsport 62350.

Du kan installera ytterligare SAP ASCS/SCS-instanser i hello befintliga WSFC-kluster med två noder:

| Rollen virtuell dator | Värdnamn för virtuell dator | Statisk IP-adress |
| --- | --- | --- |
| 1 klusternod för ASCS/SCS-instans |PR1-ascs-0 |10.0.0.10 |
| 2 klusternod för ASCS/SCS-instans |PR1-ascs-1 |10.0.0.9 |

### <a name="create-a-virtual-host-name-for-hello-clustered-sap-ascsscs-instance-on-hello-dns-server"></a>Skapa ett virtuellt värdnamn för hello klustrade SAP ASCS/SCS instans på hello DNS-server

Du kan skapa en DNS-post för hello virtuella värdnamn för hello ASCS/SCS-instansen med hjälp av hello följande parametrar:

| Nya SAP ASCS/SCS virtuella värdnamn | Tillhörande IP-adress |
| --- | --- | --- |
|pr5-sap-cl |10.0.0.50 |

hello nya värdnamnet och IP-adress visas i hello DNS-hanteraren som hello följande skärmbild:

![DNS-hanterarens lista syntaxmarkering hello definierats DNS-post för hello nya SAP ASCS/SCS virtuell klusternamnet och TCP/IP-adress][sap-ha-guide-figure-6004]

hello proceduren för att skapa en DNS-post är också beskrivs i detalj i hello huvudsakliga [guide för hög tillgänglighet SAP NetWeaver på virtuella Windows-datorer][sap-ha-guide-9.1.1].

> [!NOTE]
> hello måste nya IP-adressen som du tilldelar toohello virtuella värdnamn för hello ytterligare ASCS/SCS-instansen vara hello samma som hello ny IP-adress som du tilldelade toohello SAP Azure belastningsutjämnare.
>
>I vårt scenario är hello IP-adressen 10.0.0.50.

### <a name="add-an-ip-address-tooan-existing-azure-internal-load-balancer-by-using-powershell"></a>Lägg till en IP-adress tooan befintliga Azure intern belastningsutjämnare med hjälp av PowerShell

toocreate mer än en SAP ASCS/SCS-instans i hello samma WSFC klustret använder PowerShell tooadd en IP-adress tooan befintliga Azure intern belastningsutjämnare. Varje IP-adress kräver sin egen regler för belastningsutjämning, avsökningsport, frontend IP-adresspool och backend-adresspool.

hello följande skript lägger till en ny IP-adress tooan befintliga belastningsutjämnare. Uppdatera hello PowerShell variabler för din miljö. hello skriptet skapar alla nödvändiga belastningsutjämning regler för alla SAP ASCS/SCS-portar.

```powershell

# Select-AzureRmSubscription -SubscriptionId <xxxxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx>
Clear-Host
$ResourceGroupName = "SAP-MULTI-SID-Landscape"      # Existing resource group name
$VNetName = "pr2-vnet"                        # Existing virtual network name
$SubnetName = "Subnet"                        # Existing subnet name
$ILBName = "pr2-lb-ascs"                      # Existing ILB name                      
$ILBIP = "10.0.0.50"                          # New IP address
$VMNames = "pr2-ascs-0","pr2-ascs-1"          # Existing cluster virtual machine names
$SAPInstanceNumber = 50                       # SAP ASCS/SCS instance number: must be a unique value for each cluster
[int]$ProbePort = "623$SAPInstanceNumber"     # Probe port: must be a unique value for each IP and load balancer

$ILB = Get-AzureRmLoadBalancer -Name $ILBName -ResourceGroupName $ResourceGroupName

$count = $ILB.FrontendIpConfigurations.Count + 1
$FrontEndConfigurationName ="lbFrontendASCS$count"
$LBProbeName = "lbProbeASCS$count"

# Get hello Azure VNet and subnet
$VNet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName
$Subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $VNet -Name $SubnetName

# Add second front-end and probe configuration
Write-Host "Adding new front end IP Pool '$FrontEndConfigurationName' ..." -ForegroundColor Green
$ILB | Add-AzureRmLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -PrivateIpAddress $ILBIP -SubnetId $Subnet.Id
$ILB | Add-AzureRmLoadBalancerProbeConfig -Name $LBProbeName  -Protocol Tcp -Port $Probeport -ProbeCount 2 -IntervalInSeconds 10  | Set-AzureRmLoadBalancer

# Get new updated configuration
$ILB = Get-AzureRmLoadBalancer -Name $ILBname -ResourceGroupName $ResourceGroupName
# Get new updated LP FrontendIP COnfig
$FEConfig = Get-AzureRmLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -LoadBalancer $ILB
$HealthProbe  = Get-AzureRmLoadBalancerProbeConfig -Name $LBProbeName -LoadBalancer $ILB

# Add new back-end configuration into existing ILB
$BackEndConfigurationName  = "backendPoolASCS$count"
Write-Host "Adding new backend Pool '$BackEndConfigurationName' ..." -ForegroundColor Green
$BEConfig = Add-AzureRmLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName -LoadBalancer $ILB | Set-AzureRmLoadBalancer

# Get new updated config
$ILB = Get-AzureRmLoadBalancer -Name $ILBname -ResourceGroupName $ResourceGroupName

# Assign VM NICs toobackend pool
$BEPool = Get-AzureRmLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName -LoadBalancer $ILB
foreach($VMName in $VMNames){
        $VM = Get-AzureRmVM -ResourceGroupName $ResourceGroupName -Name $VMName
        $NICName = ($VM.NetworkInterfaceIDs[0].Split('/') | select -last 1)        
        $NIC = Get-AzureRmNetworkInterface -name $NICName -ResourceGroupName $ResourceGroupName                
        $NIC.IpConfigurations[0].LoadBalancerBackendAddressPools += $BEPool
        Write-Host "Assigning network card '$NICName' of hello '$VMName' VM toohello backend pool '$BackEndConfigurationName' ..." -ForegroundColor Green
        Set-AzureRmNetworkInterface -NetworkInterface $NIC
        #start-AzureRmVM -ResourceGroupName $ResourceGroupName -Name $VM.Name
}


# Create load-balancing rules
$Ports = "445","32$SAPInstanceNumber","33$SAPInstanceNumber","36$SAPInstanceNumber","39$SAPInstanceNumber","5985","81$SAPInstanceNumber","5$SAPInstanceNumber`13","5$SAPInstanceNumber`14","5$SAPInstanceNumber`16"
$ILB = Get-AzureRmLoadBalancer -Name $ILBname -ResourceGroupName $ResourceGroupName
$FEConfig = get-AzureRMLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -LoadBalancer $ILB
$BEConfig = Get-AzureRmLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName -LoadBalancer $ILB
$HealthProbe  = Get-AzureRmLoadBalancerProbeConfig -Name $LBProbeName -LoadBalancer $ILB

Write-Host "Creating load balancing rules for hello ports: '$Ports' ... " -ForegroundColor Green

foreach ($Port in $Ports) {

        $LBConfigrulename = "lbrule$Port" + "_$count"
        Write-Host "Creating load balancing rule '$LBConfigrulename' for hello port '$Port' ..." -ForegroundColor Green

        $ILB | Add-AzureRmLoadBalancerRuleConfig -Name $LBConfigRuleName -FrontendIpConfiguration $FEConfig  -BackendAddressPool $BEConfig -Probe $HealthProbe -Protocol tcp -FrontendPort  $Port -BackendPort $Port -IdleTimeoutInMinutes 30 -LoadDistribution Default -EnableFloatingIP   
}

$ILB | Set-AzureRmLoadBalancer

Write-Host "Succesfully added new IP '$ILBIP' toohello internal load balancer '$ILBName'!" -ForegroundColor Green

```
När hello skriptet har körts hello hello resultat visas i Azure-portalen som visas i följande skärmbild hello:

![Nya frontend IP-pool i hello Azure-portalen][sap-ha-guide-figure-6005]

### <a name="add-disks-toocluster-machines-and-configure-hello-sios-cluster-share-disk"></a>Lägg till diskar toocluster datorer och konfigurera hello SIOS klusterdisk resursen

Du måste lägga till en ny resurs för klusterdisk för varje ytterligare SAP ASCS/SCS-instans. Hej WSFC klusterdisken resursen används för tillfället är hello SIOS DataKeeper programvarulösning för Windows Server 2012 R2.

Hej du följande:
1. Lägg till ytterligare diskar av hello samma storlek (som du behöver toostripe) tooeach av hello klusternoder och formateras.
2. Konfigurera storage-replikering med SIOS DataKeeper.

Den här proceduren förutsätter att du redan har installerat SIOS DataKeeper på datorer som hello WSFC-klustret. Om du har installerat det, måste du konfigurera replikering mellan hello datorer. hello process beskrivs i detalj i hello huvudsakliga [guide för hög tillgänglighet SAP NetWeaver på virtuella Windows-datorer][sap-ha-guide-8.12.3.3].  

![DataKeeper synkron spegling för hello nya SAP ASCS/SCS dela disken][sap-ha-guide-figure-6006]

### <a name="deploy-vms-for-sap-application-servers-and-dbms-cluster"></a>Distribuera virtuella datorer för SAP-programservrar och DBMS-kluster

toocomplete hello infrastruktur inför hello andra SAP-systemet, hello följande:

1. Distribuera dedikerade virtuella datorer för SAP-programservrar och placera dem i sina egna dedikerade tillgänglighetsgruppen.
2. Distribuera dedikerade virtuella datorer för DBMS-kluster och placera dem i sina egna dedikerade tillgänglighetsgruppen.


## <a name="install-hello-second-sap-sid2-netweaver-system"></a>Installera hello andra SAP NetWeaver för SID2

hello klar att installera en andra SAP SID2 system beskrivs i hello huvudsakliga [guide för hög tillgänglighet SAP NetWeaver på virtuella Windows-datorer][sap-ha-guide-9].

hello övergripande procedur är följande:

1. [Installera hello SAP första klusternoden][sap-ha-guide-9.1.2].  
 I det här steget du installerar SAP med en hög tillgänglighet ASCS/SCS-instans på hello **befintliga WSFC-klusternoden 1**.

2. [Ändra hello SAP-profil för hello ASCS/SCS instansen][sap-ha-guide-9.1.3].

3. [Konfigurera en avsökningsport][sap-ha-guide-9.1.4].  
 I det här steget konfigurerar du en SAP klusterresurs SAP-SID2-IP-avsökningsport med hjälp av PowerShell. Köra den här konfigurationen på en av klusternoderna för hello SAP ASCS/SCS.

4. [Install hello-databasinstans] [sap-hög tillgänglighet-guide-9.2].  
 I det här steget installerar om du DBMS på en dedikerad WSFC-klustret.

5. [Install hello andra klusternod] [sap-hög tillgänglighet-guide-9.3].  
 I det här steget installerar om du SAP med en hög tillgänglighet ASCS/SCS-instans på hello befintliga WSFC klusternoden 2.

6. Öppna portar i Windows-brandväggen för hello SAP ASCS/SCS-instansen och ProbePort.  
 På båda klusternoderna som används för SAP ASCS/SCS instanser, öppnar du alla portar i Windows-brandväggen som används av SAP ASCS/SCS. De här portarna visas i hello [guide för hög tillgänglighet SAP NetWeaver på virtuella Windows-datorer][sap-ha-guide-8.8].  
 Också öppna hello Azure interna belastningen belastningsutjämnaren avsökningsport, vilket är 62350 i vårt scenario.

7. [Ändra hello starttypen till hello SAP ÄNDARE Windows tjänstinstans][sap-ha-guide-9.4].

8. [Installera hello SAP primära programserver] [ sap-ha-guide-9.5] på hello nya dedikerade VM.

9. [Installera hello SAP ytterligare programserver] [ sap-ha-guide-9.6] på hello nya dedikerade VM.

10. [Testa hello SAP ASCS/SCS instans redundans och replikering av SIOS][sap-ha-guide-10].

## <a name="next-steps"></a>Nästa steg

- [Nätverk gränser: Azure Resource Manager][networking-limits-azure-resource-manager]
- [Flera VIP: er för Azure belastningsutjämnare][load-balancer-multivip-overview]
- [Guide för hög tillgänglighet SAP NetWeaver på virtuella Windows-datorer][sap-ha-guide]
