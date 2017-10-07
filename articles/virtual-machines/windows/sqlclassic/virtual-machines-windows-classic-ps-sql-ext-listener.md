---
title: "aaaConfigure en extern lyssnare för Always On-Tillgänglighetsgrupper | Microsoft Docs"
description: "Den här kursen får du hjälp med att skapa en alltid på tillgänglighetsgruppens lyssnare i Azure som är externt tillgänglig med hello offentliga virtuella IP-adressen för hello associerade Molntjänsten."
services: virtual-machines-windows
documentationcenter: na
author: MikeRayMSFT
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: a2453032-94ab-4775-b976-c74d24716728
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/31/2017
ms.author: mikeray
ms.openlocfilehash: f8e2110bcc25d9cb7653675cb4ae5c8d717b6902
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-an-external-listener-for-always-on-availability-groups-in-azure"></a>Konfigurera en extern lyssnare för Always On-Tillgänglighetsgrupper i Azure
> [!div class="op_single_selector"]
> * [Internt lyssnare](../classic/ps-sql-int-listener.md)
> * [Externa lyssnare](../classic/ps-sql-ext-listener.md)
> 
> 

Det här avsnittet visar hur hello tooconfigure en lyssnare för en alltid på Tillgänglighetsgruppen som är tillgänglig externt på internet. Detta är möjligt genom att associera hello molnbaserad tjänst **offentliga virtuella IP-adresser (VIP)** adress med hello-lyssnaren.

> [!IMPORTANT] 
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../azure-resource-manager/resource-manager-deployment-model.md). Den här artikeln täcker hello klassiska distributionsmodellen. Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.

Tillgänglighetsgruppen kan innehålla repliker som finns lokalt, Azure, eller omfattar både lokalt och Azure för hybrid-konfigurationer. Azure repliker kan finnas inom hello samma region eller över flera regioner med flera virtuella nätverk (Vnet). hello stegen nedan förutsätter att du redan har [konfigureras en tillgänglighetsgrupp](../classic/portal-sql-alwayson-availability-groups.md) men inte har konfigurerat en lyssnare.

## <a name="guidelines-and-limitations-for-external-listeners"></a>Riktlinjer och begränsningar för externa lyssnare
Observera följande riktlinjer om hello tillgänglighetsgruppens lyssnare i Azure när du distribuerar med hello cloud service blygdbenets adressen hello:

* hello tillgänglighetsgruppens lyssnare stöds på Windows Server 2008 R2, Windows Server 2012 och Windows Server 2012 R2.
* hello-klientprogrammet måste finnas på en annan molntjänst än hello en som innehåller tillgänglighetsgruppen virtuella datorer. Azure stöder inte direkt serverreturnering med klienten och servern i hello samma molntjänst.
* Som standard visar hello stegen i den här artikeln hur tooconfigure en lyssnare toouse hello cloud service virtuella IP-adressen. Det är dock möjligt tooreserve och skapa flera VIP-adresser för Molntjänsten. Detta gör att du toouse hello stegen i den här artikeln toocreate flera lyssnare som var associerad med en annan VIP. Mer information om hur toocreate flera VIP-adresser, se [flera VIP: er per Molntjänsten](../../../load-balancer/load-balancer-multivip.md).
* Om du skapar en lyssnare för en hybridmiljö hello lokalt nätverk måste ha anslutningen toohello offentliga Internet i tillägg toohello plats-till-plats VPN-anslutningar med hello virtuella Azure-nätverket. Hello tillgänglighetsgruppens lyssnare är tillgängligt endast hello offentliga IP-adressen för hello respektive Molntjänsten i hello Azure-undernät.
* Det går inte toocreate en extern lyssnare i hello samma molntjänst där du även har en intern lyssnare med hello interna belastningen belastningsutjämnare (ILB).

## <a name="determine-hello-accessibility-of-hello-listener"></a>Fastställa hello hjälpmedel för hello-lyssnare
[!INCLUDE [ag-listener-accessibility](../../../../includes/virtual-machines-ag-listener-determine-accessibility.md)]

Den här artikeln fokuserar på hur du skapar en lyssnare som använder **extern belastningsutjämning**. Om du vill att en lyssnare som är privata tooyour virtuella nätverk finns i hello-versionen av den här artikeln innehåller steg för att skapa en [lyssnare med ILB](../classic/ps-sql-int-listener.md)

## <a name="create-load-balanced-vm-endpoints-with-direct-server-return"></a>Skapa belastningsutjämnade slutpunkter för virtuell dator med direkt servern returnerade
Externa nätverksbelastning använder hello virtuella hello offentliga virtuella IP-adressen för hello molntjänst som är värd för dina virtuella datorer. Så att du inte behöver toocreate eller konfigurera hello belastningsutjämnare i det här fallet.

För varje virtuell dator som värd för en replik av en Azure måste du skapa en slutpunkt för Utjämning av nätverksbelastning. Om du har repliker i flera områden, varje replik för området måste vara i samma molntjänst i hello hello samma virtuella nätverk. Skapa Tillgänglighetsgruppen repliker som sträcker sig över flera Azure-regioner kräver konfigurera flera virtuella nätverk. Mer information om hur du konfigurerar mellan VNet-anslutningen finns [konfigurera VNet tooVNet anslutning](../../../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md).

1. Navigera tooeach VM som värd för en replik och visa hello-information i hello Azure-portalen.
2. Klicka på hello **slutpunkter** fliken för varje hello virtuella datorer.
3. Kontrollera att hello **namn** och **offentlig Port** hello lyssnare slutpunkt som du vill toouse inte är redan används. I hello exemplet nedan hello namn är ”MyEndpoint” och hello porten ”1433”.
4. På din lokala klient, ladda ned och installera [hello senaste PowerShell-modulen](https://azure.microsoft.com/downloads/).
5. Starta **Azure PowerShell**. En ny PowerShell-sessionen öppen med hello Azure administrativa moduler som lästs in.
6. Kör **Get-AzurePublishSettingsFile**. Denna cmdlet dirigerar tooa webbläsare toodownload en publicera inställningar tooa lokala katalog. Du kan uppmanas att dina autentiseringsuppgifter för inloggning för din Azure-prenumeration.
7. Kör hello **importera AzurePublishSettingsFile** kommandot med hello sökvägen hello Publicera fil med inställningar som du hämtade:
   
        Import-AzurePublishSettingsFile -PublishSettingsFile <PublishSettingsFilePath>
   
    När hello publicera inställningsfilen importeras, kan du hantera Azure-prenumerationen i hello PowerShell-session.
    
1. Kopiera hello PowerShell-skriptet nedan i en textredigerare och ange hello variabelvärden toosuit miljön (standardvärden har angetts för vissa parametrar). Observera att om tillgänglighetsgruppen sträcker sig över Azure-regioner, måste du köra hello skriptet en gång i varje datacenter för hello Molntjänsten och noder som tillhör det datacentret.
   
        # Define variables
        $ServiceName = "<MyCloudService>" # hello name of hello cloud service that contains hello availability group nodes
        $AGNodes = "<VM1>","<VM2>","<VM3>" # all availability group nodes containing replicas in hello same cloud service, separated by commas
   
        # Configure a load balanced endpoint for each node in $AGNodes, with direct server return enabled
        ForEach ($node in $AGNodes)
        {
            Get-AzureVM -ServiceName $ServiceName -Name $node | Add-AzureEndpoint -Name "ListenerEndpoint" -Protocol "TCP" -PublicPort 1433 -LocalPort 1433 -LBSetName "ListenerEndpointLB" -ProbePort 59999 -ProbeProtocol "TCP" -DirectServerReturn $true | Update-AzureVM
        }

2. När du har angett hello variabler, kopiera hello skript från hello textredigerare till din Azure PowerShell-session toorun den. Om hello uppmaningen visas fortfarande >>, skriver du ange igen toomake att hello skriptet börjar köras.

## <a name="verify-that-kb2854082-is-installed-if-necessary"></a>Kontrollera att KB2854082 installeras vid behov
[!INCLUDE [kb2854082](../../../../includes/virtual-machines-ag-listener-kb2854082.md)]

## <a name="open-hello-firewall-ports-in-availability-group-nodes"></a>Öppna portar i brandväggen hello tillgänglighet grupp noder
[!INCLUDE [firewall](../../../../includes/virtual-machines-ag-listener-open-firewall.md)]

## <a name="create-hello-availability-group-listener"></a>Skapa hello tillgänglighetsgruppens lyssnare

Skapa hello tillgänglighetsgruppens lyssnare i två steg. Först skapar hello klienten åtkomst punkt klusterresurs och konfigurera beroenden. Konfigurera andra hello klusterresurser med PowerShell.

### <a name="create-hello-client-access-point-and-configure-hello-cluster-dependencies"></a>Skapa hello klientåtkomstpunkt och konfigurera hello klustret beroenden
[!INCLUDE [firewall](../../../../includes/virtual-machines-ag-listener-create-listener.md)]

### <a name="configure-hello-cluster-resources-in-powershell"></a>Konfigurera hello klusterresurser i PowerShell
1. Extern belastningsutjämning, måste du skaffa hello offentliga virtuella IP-adressen för hello Molntjänsten som innehåller repliker. Logga in på hello Azure-portalen. Navigera toohello molntjänst som innehåller tillgänglighetsgruppen VM. Öppna hello **instrumentpanelen** vyn.
2. Observera hello-adress som anges under **offentliga virtuella IP-adressen**. Om din lösning sträcker sig över Vnet, kan du upprepa proceduren för varje tjänst i molnet som innehåller en virtuell dator som är värd för en replik.
3. På en av hello VMs, kopiera hello PowerShell-skriptet nedan i en textredigerare och ange hello variabler toohello värden som du antecknade tidigare.
   
        # Define variables
        $ClusterNetworkName = "<ClusterNetworkName>" # hello cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher toofind hello name)
        $IPResourceName = "<IPResourceName>" # hello IP Address resource name
        $CloudServiceIP = "<X.X.X.X>" # Public Virtual IP (VIP) address of your cloud service
   
        Import-Module FailoverClusters
   
        # If you are using Windows Server 2012 or higher, use hello Get-Cluster Resource command. If you are using Windows Server 2008 R2, use hello cluster res command. Both commands are commented out. Choose hello one applicable tooyour environment and remove hello # at hello beginning of hello line tooconvert hello comment tooan executable line of code.
   
        # Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$CloudServiceIP";"ProbePort"="59999";"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"OverrideAddressMatch"=1;"EnableDhcp"=0}
        # cluster res $IPResourceName /priv enabledhcp=0 overrideaddressmatch=1 address=$CloudServiceIP probeport=59999  subnetmask=255.255.255.255
4. När du har lagt hello variabler, öppna ett upphöjt Windows PowerShell-fönster och sedan kopiera hello skript från hello textredigerare och klistra in i din Azure PowerShell-session toorun den. Om hello uppmaningen visas fortfarande >>, skriver du ange igen toomake att hello skriptet börjar köras.
5. Upprepa detta för varje virtuell dator. Det här skriptet konfigurerar hello IP-adressresurs med hello IP-adress för hello Molntjänsten och anger andra parametrar som hello avsökningsport. När hello IP-adressresurs är online, svarar den sedan toohello avsökning på hello avsökningsport från hello belastningsutjämnade slutpunkten skapade tidigare i den här kursen.

## <a name="bring-hello-listener-online"></a>Ta hello-lyssnare
[!INCLUDE [Bring-Listener-Online](../../../../includes/virtual-machines-ag-listener-bring-online.md)]

## <a name="follow-up-items"></a>Uppföljning av objekt
[!INCLUDE [Follow-up](../../../../includes/virtual-machines-ag-listener-follow-up.md)]

## <a name="test-hello-availability-group-listener-within-hello-same-vnet"></a>Testa hello tillgänglighetsgruppens lyssnare (hello i samma virtuella nätverk)
[!INCLUDE [Test-Listener-Within-VNET](../../../../includes/virtual-machines-ag-listener-test.md)]

## <a name="test-hello-availability-group-listener-over-hello-internet"></a>Testa hello tillgänglighetsgruppens lyssnare (via hello internet)
I ordning tooaccess hello lyssnare från utanför hello virtuellt nätverk, måste du använda externa och offentliga belastningsutjämning (beskrivs i det här avsnittet) i stället för ILB, vilket är endast tillgänglig i hello samma virtuella nätverk. Ange hello molntjänstnamnet i hello anslutningssträngen. Till exempel om du har en molntjänst med namnet hello *mycloudservice*, hello sqlcmd instruktionen blir som följer:

    sqlcmd -S "mycloudservice.cloudapp.net,<EndpointPort>" -d "<DatabaseName>" -U "<LoginId>" -P "<Password>"  -Q "select @@servername, db_name()" -l 15

Till skillnad från föregående exempel hello SQL-autentisering måste användas eftersom hello anroparen inte kan använda windows-autentisering över hello internet. Mer information finns i [alltid på Tillgänglighetsgruppen i Azure VM: klienten anslutning scenarier](http://blogs.msdn.com/b/sqlcat/archive/2014/02/03/alwayson-availability-group-in-windows-azure-vm-client-connectivity-scenarios.aspx). När du använder SQL-autentisering, se till att du skapar hello samma inloggningen på båda replikerna. Läs mer om hur du felsöker inloggningar med Tillgänglighetsgrupper [hur toomap inloggningar eller Använd finns SQL databasrepliker användare tooconnect tooother och mappa tooavailability databaser](http://blogs.msdn.com/b/alwaysonpro/archive/2014/02/19/how-to-map-logins-or-use-contained-sql-database-user-to-connect-to-other-replicas-and-map-to-availability-databases.aspx).

Om hello alltid på repliker är i olika undernät, måste klienter ange **MultisubnetFailover = True** i hello anslutningssträngen. Detta resulterar i parallella anslutning försök tooreplicas i hello olika undernät. Observera att det här scenariot innehåller en distribution för cross-region Always On-Tillgänglighetsgruppen.

## <a name="next-steps"></a>Nästa steg
[!INCLUDE [Listener-Next-Steps](../../../../includes/virtual-machines-ag-listener-next-steps.md)]

