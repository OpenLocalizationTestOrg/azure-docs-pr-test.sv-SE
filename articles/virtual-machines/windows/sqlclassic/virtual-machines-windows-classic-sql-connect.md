---
title: aaaConnect tooa SQL Server-dator i Azure (klassisk) | Microsoft Docs
description: "Lär dig hur tooconnect tooSQL Server körs på en virtuell dator i Azure. Det här avsnittet använder hello klassiska distributionsmodellen. hello scenarier varierar beroende på hello nätverkskonfigurationen och hello placering för hello-klienten."
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
tags: azure-service-management
ms.assetid: 416948af-454f-4cfe-8fd2-7cf971cbd3e9
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 01/31/2017
ms.author: jroth
experimental_id: d51f3cc6-753b-4e
ms.openlocfilehash: 4fff3936ad0bcfd3a56855a8436991e19b92522b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-tooa-sql-server-virtual-machine-on-azure-classic-deployment"></a>Ansluta tooa SQL Server-dator i Azure (klassisk distribution)
> [!div class="op_single_selector"]
> * [Resource Manager](../sql/virtual-machines-windows-sql-connect.md)
> * [Klassisk](../classic/sql-connect.md)
> 
> 

## <a name="overview"></a>Översikt
Det här avsnittet beskrivs hur tooconnect tooyour SQL Server-instansen körs på en virtuell Azure-dator. Det täcker vissa [allmänna anslutningsproblem scenarier](#connection-scenarios) och ger [beskrivs stegen för att konfigurera SQL Server-anslutningen i en Azure VM](#steps-for-configuring-sql-server-connectivity-in-an-azure-vm).

> [!IMPORTANT] 
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../azure-resource-manager/resource-manager-deployment-model.md). Den här artikeln täcker hello klassiska distributionsmodellen. Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen. Om du använder Hanteraren för virtuella datorer, se [ansluta tooa virtuell dator med SQL Server på Azure med hjälp av hanteraren för filserverresurser](../sql/virtual-machines-windows-sql-connect.md).

## <a name="connection-scenarios"></a>-Scenarier
hello sätt en klient ansluter tooSQL Server som körs på en virtuell dator varierar beroende på vilken hello platsen för hello klienten och hello dator/nätverkskonfiguration. Dessa scenarier är:

* [Ansluta tooSQL Server i hello samma molntjänst](#connect-to-sql-server-in-the-same-cloud-service)
* [Ansluta tooSQL Server över hello internet](#connect-to-sql-server-over-the-internet)
* [Ansluta tooSQL Server i hello samma virtuella nätverk](#connect-to-sql-server-in-the-same-virtual-network)

> [!NOTE]
> Innan du ansluter med någon av dessa metoder, måste du följa hello [stegen i den här artikeln tooconfigure anslutningen](#steps-for-configuring-sql-server-connectivity-in-an-azure-vm).
> 
> 

### <a name="connect-toosql-server-in-hello-same-cloud-service"></a>Ansluta tooSQL Server i hello samma molntjänst
Flera virtuella datorer kan skapas i hello samma molntjänst. toounderstand virtuell datorer scenariot finns [hur tooconnect virtuella datorer med virtuella nätverk eller molnet](../classic/connect-vms.md#connect-vms-in-a-standalone-cloud-service). Det här scenariot är när en klient på en virtuell dator försöker tooconnect tooSQL Server körs på en annan virtuell dator i hello samma molntjänst.

I det här scenariot kan du ansluta med hello VM **namn** (visas även som **datornamn** eller **värdnamn** i hello portal). Detta är hello namn du angav för hello VM under skapandet. Till exempel om du har gett din SQL-VM **mysqlvm**, en klient VM i samma molntjänst kan använda hello hello efter anslutning sträng tooconnect:

    "Server=mysqlvm;Integrated Security=false;User ID=<login_name>;Password=<your_password>"

### <a name="connect-toosql-server-over-hello-internet"></a>Ansluta tooSQL Server över hello Internet
Om du vill tooconnect tooyour SQL Server-databasmotorn från hello Internet, måste du skapa en virtuell dator-slutpunkt för inkommande TCP-kommunikation. Den här Azure konfigurationssteget dirigerar inkommande TCP-port trafik tooa TCP-port som är tillgänglig toohello virtuell dator.

tooconnect över Hej internet, måste du använda hello VM DNS-namn och hello VM endpoint portnummer (konfigurerat nedan). toofind hello DNS-namn, navigera toohello Azure portal och välj **virtuella datorer (klassisk)**. Välj sedan den virtuella datorn. Hej **DNS-namnet** visas i hello **översikt** avsnitt.

Anta till exempel att en klassisk virtuell dator med namnet **mysqlvm** med DNS-namnet **mysqlvm7777.cloudapp.net** och en VM-slutpunkten för **57500**. Under förutsättning att korrekt konfigurerad anslutning, hello följande anslutningssträng kunde använda tooaccess hello virtuell dator från var som helst på internet hello:

    "Server=mycloudservice.cloudapp.net,57500;Integrated Security=false;User ID=<login_name>;Password=<your_password>"

Även om detta aktiverar anslutning för klienter över Hej internet, detta innebär inte att någon kan ansluta tooyour SQL Server. Utanför klienter ha toohello rätt användarnamn och lösenord. För ytterligare säkerhet kan inte använda hello välkända port 1433 för hello offentliga virtuella datorslutpunkten. Och om möjligt bör du lägga till en ACL på slutpunkten toorestrict trafik endast toohello klienterna du tillåter. Anvisningar om hur du använder ACL: er med slutpunkter finns [hantera hello ACL på en slutpunkt](../classic/setup-endpoints.md#manage-the-acl-on-an-endpoint).

> [!NOTE]
> Det är viktigt att alla utgående data från när du använder den här tekniken toocommunicate med SQL Server hello Azure-datacenter toonote är ämne toonormal [på utgående dataöverföringar](https://azure.microsoft.com/pricing/details/data-transfers/).
> 
> 

### <a name="connect-toosql-server-in-hello-same-virtual-network"></a>Ansluta tooSQL Server i hello samma virtuella nätverk
[Virtuellt nätverk](../../../virtual-network/virtual-networks-overview.md) möjliggör ytterligare scenarier. Du kan ansluta virtuella datorer i samma virtuella nätverk, även om de virtuella datorer finns i olika molntjänster hello. Och med en [plats-till-plats VPN](../../../vpn-gateway/vpn-gateway-site-to-site-create.md), kan du skapa en hybrid-arkitektur som ansluter virtuella datorer med lokala nätverk och datorer.

Virtuella nätverk kan du toojoin din virtuella Azure-datorer tooa domän. Detta är hello endast sätt toouse Windows-autentisering tooSQL Server. hello kräver andra scenarier SQL-autentisering med användarnamn och lösenord.

Om du ska tooconfigure en domänmiljö och Windows-autentisering, behöver inte toouse hello stegen i den här artikeln tooconfigure hello offentlig slutpunkt eller hello SQL-autentisering och inloggningar. Du kan ansluta tooyour SQL Server-instansen genom att ange hello SQL Server-VM namn i hello anslutningssträngen i det här scenariot. hello följande exempel förutsätter att Windows-autentisering har konfigurerats och hello användaren har beviljats åtkomst toohello SQL Server-instansen.

    "Server=mysqlvm;Integrated Security=true"

## <a name="steps-for-configuring-sql-server-connectivity-in-an-azure-vm"></a>Steg för att konfigurera SQL Server-anslutningen i en Azure VM
hello följande steg visar hur hello tooconnect toohello SQL Server-instansen över internet med hjälp av SQL Server Management Studio (SSMS). Dock hello samma steg gäller toomaking din virtuella dator med SQL Server tillgänglig för ditt program som körs både lokalt och i Azure.

Innan du kan ansluta toohello instans av SQL Server från en annan virtuell dator eller hello internet, måste du utföra följande uppgifter som beskrivs i hello avsnitten som följer hello:

* [Skapa en TCP-slutpunkt för hello virtuell dator](#create-a-tcp-endpoint-for-the-virtual-machine)
* [Öppna TCP-portar i hello Windows-brandväggen](#open-tcp-ports-in-the-windows-firewall-for-the-default-instance-of-the-database-engine)
* [Konfigurera SQL Server toolisten på hello TCP-protokoll](#configure-sql-server-to-listen-on-the-tcp-protocol)
* [Konfigurera SQL Server för verifiering i blandat läge](#configure-sql-server-for-mixed-mode-authentication)
* [Skapa SQL Server-autentisering-inloggningar](#create-sql-server-authentication-logins)
* [Fastställa hello hello virtuella datorns DNS-namn](#determine-the-dns-name-of-the-virtual-machine)
* [Ansluta toohello databasmotorn från en annan dator](#connect-to-the-database-engine-from-another-computer)

hello anslutningssökväg sammanfattning av hello följande diagram:

![Ansluta tooa SQL Server-dator](../../../../includes/media/virtual-machines-sql-server-connection-steps/SQLServerinVMConnectionMap.png)

[!INCLUDE [Connect tooSQL Server in a VM Classic TCP Endpoint](../../../../includes/virtual-machines-sql-server-connection-steps-classic-tcp-endpoint.md)]

[!INCLUDE [Connect tooSQL Server in a VM](../../../../includes/virtual-machines-sql-server-connection-steps.md)]

[!INCLUDE [Connect tooSQL Server in a VM Classic Steps](../../../../includes/virtual-machines-sql-server-connection-steps-classic.md)]

## <a name="next-steps"></a>Nästa steg
Om du dessutom planerar toouse AlwaysOn-Tillgänglighetsgrupper för hög tillgänglighet och katastrofåterställning, bör du överväga att implementera en lyssnare. Databas-klienter ansluter toohello lyssnare i stället för direkt tooone av hello SQL Server-instanser. hello lyssnare vägar klienter toohello primära repliken i hello-tillgänglighetsgrupp. Mer information finns i [konfigurera en ILB-lyssnare för AlwaysOn-Tillgänglighetsgrupper i Azure](../classic/ps-sql-int-listener.md).

Det är viktigt tooreview alla hello säkerhet för bästa praxis för SQL Server körs på en virtuell Azure-dator. Mer information finns i [Säkerhetsöverväganden för SQL Server på Azure Virtual Machines](../sql/virtual-machines-windows-sql-security.md).

[Utforska hello Utbildningsväg](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/) för SQL Server på virtuella Azure-datorer. 

För andra avsnitt relaterade toorunning SQL Server i virtuella Azure-datorer, se [SQL Server på Azure Virtual Machines](../sql/virtual-machines-windows-sql-server-iaas-overview.md).

