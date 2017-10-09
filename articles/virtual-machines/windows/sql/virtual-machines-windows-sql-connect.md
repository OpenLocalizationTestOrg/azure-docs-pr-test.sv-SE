---
title: aaaConnect tooa virtuell dator med SQL Server (Resource Manager) | Microsoft Docs
description: "Lär dig hur tooconnect tooSQL Server körs på en virtuell dator i Azure. Det här avsnittet använder hello klassiska distributionsmodellen. hello scenarier varierar beroende på hello nätverkskonfigurationen och hello placering för hello-klienten."
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
tags: azure-resource-manager
ms.assetid: aa5bf144-37a3-4781-892d-e0e300913d03
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 08/14/2017
ms.author: jroth
ms.openlocfilehash: 7b127c14c37b9a72c19ed17f8b1dad61c7bc2d38
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-tooa-sql-server-virtual-machine-on-azure-resource-manager"></a>Ansluta tooa SQL Server-dator i Azure (Resource Manager)
> [!div class="op_single_selector"]
> * [Resource Manager](virtual-machines-windows-sql-connect.md)
> * [Klassisk](../classic/sql-connect.md)
> 
> 

## <a name="overview"></a>Översikt

Det här avsnittet beskrivs hur tooconnect tooyour SQL Server-instansen körs på en virtuell Azure-dator. Det täcker vissa [allmänna anslutningsproblem scenarier](#connection-scenarios) och ger [beskrivs stegen för att konfigurera SQL Server-anslutningen i en Azure VM](#steps-for-manually-configuring-sql-server-connectivity-in-an-azure-vm).

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

Om du i stället måste en fullständig genomgång av både etablering och anslutningen, se [etablering av en SQL Server-dator på Azure](virtual-machines-windows-portal-sql-server-provision.md).

## <a name="connection-scenarios"></a>-Scenarier

hello sätt en klient ansluter tooSQL Server som körs på en virtuell dator varierar beroende på vilken hello platsen för hello klienten och hello nätverkskonfigurationen.

Om du etablerar en SQL Server-VM i hello Azure-portalen har hello möjlighet att ange hello typ av **SQL-anslutning**.

![Anslutningsmöjlighet för offentliga SQL under etableringen.](./media/virtual-machines-windows-sql-connect/sql-vm-portal-connectivity.png)

Alternativen för anslutning är:

| Alternativ | Beskrivning |
|---|---|
| **Offentliga** | Ansluta tooSQL Server över hello internet |
| **Privata** | Ansluta tooSQL Server i hello samma virtuella nätverk |
| **Lokala** | Ansluta tooSQL Server lokalt på hello samma virtuella dator | 

hello följande avsnitt beskrivs hello **offentliga** och **privata** alternativ i detalj.

## <a name="connect-toosql-server-over-hello-internet"></a>Ansluta tooSQL Server över hello Internet

Om du vill tooconnect tooyour SQL Server-databasmotorn från hello Internet väljer **offentliga** för hello **SQL-anslutning** typ i hello portal under etableringen. hello-portalen automatiskt hello följande steg:

* Aktiverar hello TCP/IP-protokollet för SQL Server.
* Konfigurerar en brandvägg regeln tooopen hello SQL Server TCP-porten (standard 1433).
* Gör det möjligt för SQL Server-autentisering krävs för allmän åtkomst.
* Konfigurerar hello nätverkssäkerhetsgruppen på hello VM tooall TCP-trafik på hello port för SQL Server.

> [!IMPORTANT]
> hello virtuella avbildningar för hello SQL Server Developer och Express-versioner aktiveras inte hello TCP/IP-protokollet. För utvecklare och Express-versioner måste du använda SQL Server Configuration Manager för[manuellt Aktivera hello TCP/IP-protokollet](#manualtcp) hello VM när du har skapat.

Alla klienter med åtkomst till internet kan ansluta toohello SQL Server-instansen genom att ange hello offentliga IP-adressen för hello virtuell dator eller en DNS-etikett som tilldelats toothat IP-adress. Om hello SQL Server-port 1433, behöver du inte toospecify den i hello anslutningssträngen. hello följande anslutningssträng ansluter tooa SQL VM till en DNS-etikett för `sqlvmlabel.eastus.cloudapp.azure.com` med SQL-autentisering (du kan också använda hello offentlig IP-adress).

```
Server=sqlvmlabel.eastus.cloudapp.azure.com;Integrated Security=false;User ID=<login_name>;Password=<your_password>
```

Även om detta aktiverar anslutning för klienter över Hej internet, detta innebär inte att någon kan ansluta tooyour SQL Server. Utanför klienter ha toohello rätt användarnamn och lösenord. För ytterligare säkerhet kan kan du undvika hello välkända port 1433. Till exempel om du har konfigurerat SQL Server toolisten på port 1500 och upprätta rätt brandvägg och regler för nätverkssäkerhetsgrupper kan du ansluta genom att lägga till hello port number toohello servernamn. hello följande exempel ändrar hello föregående genom att lägga till ett anpassat portnummer **1500**, toohello servernamn:

```
Server=sqlvmlabel.eastus.cloudapp.azure.com,1500;Integrated Security=false;User ID=<login_name>;Password=<your_password>"
```

> [!NOTE]
> När du frågar SQL Server på en virtuell dator över hello internet, alla utgående data från hello Azure datacenter är ämne toonormal [på utgående dataöverföringar](https://azure.microsoft.com/pricing/details/data-transfers/).

## <a name="connect-toosql-server-within-a-virtual-network"></a>Ansluta tooSQL Server inom ett virtuellt nätverk

När du väljer **privata** för hello **SQL-anslutning** typ i hello-portalen, Azure konfigurerar de flesta av hello inställningarna identiskt för**offentliga**. hello en skillnaden är att det finns inga network security grupp regeln tooallow utanför trafik på hello SQL Server-porten (standard 1433).

> [!IMPORTANT]
> hello virtuella avbildningar för hello SQL Server Developer och Express-versioner aktiveras inte hello TCP/IP-protokollet. För utvecklare och Express-versioner måste du använda SQL Server Configuration Manager för[manuellt Aktivera hello TCP/IP-protokollet](#manualtcp) hello VM när du har skapat.

Privata anslutningen används ofta tillsammans med [virtuellt nätverk](../../../virtual-network/virtual-networks-overview.md), vilket gör att flera scenarier. Du kan ansluta virtuella datorer i samma virtuella nätverk, även om de virtuella datorer finns i olika resursgrupper hello. Och med en [plats-till-plats VPN](../../../vpn-gateway/vpn-gateway-site-to-site-create.md), kan du skapa en hybrid-arkitektur som ansluter virtuella datorer med lokala nätverk och datorer.

Virtuella nätverk kan också du toojoin din virtuella Azure-datorer tooa domän. Detta är hello endast sätt toouse Windows-autentisering tooSQL Server. hello kräver andra scenarier SQL-autentisering med användarnamn och lösenord.

Förutsatt att du har konfigurerat DNS i ditt virtuella nätverk kan ansluta du tooyour SQL Server-instansen genom att ange hello datornamn för SQL Server-VM hello anslutningssträngen. hello också följande exempel förutsätter att Windows-autentisering har konfigurerats och hello användaren har beviljats åtkomst toohello SQL Server-instansen.

```
Server=mysqlvm;Integrated Security=true
```

## <a id="change"></a>Ändra inställningar för SQL-anslutning

Du kan ändra hello anslutningsinställningar för den virtuella datorn i SQL Server i hello Azure-portalen.

1. Välj i hello Azure-portalen, **virtuella datorer**.

2. Välj din SQLServer-VM.

3. Under **inställningar**, klickar du på **SQL Server-konfigurationsfilen**.

4. Ändra hello **SQL anslutningen nivån** tooyour som krävs för inställningen. Du kan eventuellt använda den här område toochange hello port för SQL Server eller autentiseringsinställningar för hello SQL.

   ![Ändra SQL-anslutning](./media/virtual-machines-windows-sql-connect/sql-vm-portal-connectivity-change.png)

5. Vänta några minuter för hello uppdatering toocomplete.

   ![SQL-VM uppdateringsmeddelande](./media/virtual-machines-windows-sql-connect/sql-vm-updating-notification.png)

## <a id="manualtcp"></a>Aktivera TCP/IP för utvecklare och Express-versioner

När du ändrar inställningarna för SQL Server-anslutningen, aktiverar Azure automatiskt inte hello TCP/IP-protokollet för SQL Server Developer och Express Edition. hello stegen nedan förklarar hur toomanually aktivera TCP/IP så att du kan fjärransluta efter IP-adress.

Anslut toohello SQL Server-datorn med fjärrskrivbord.

> [!INCLUDE [Connect tooSQL Server VM with remote desktop](../../../../includes/virtual-machines-sql-server-remote-desktop-connect.md)]

Aktivera sedan hello TCP/IP-protokollet med **SQL Server Configuration Manager**.

> [!INCLUDE [Connect tooSQL Server VM with remote desktop](../../../../includes/virtual-machines-sql-server-connection-tcp-protocol.md)]

## <a name="connect-with-ssms"></a>Anslut med SSMS

hello följande steg visar hur toocreate ett valfritt DNS etikett för din virtuella Azure-datorn och ansluter sedan med SQL Server Management Studio (SSMS).

[!INCLUDE [Connect tooSQL Server in a VM Resource Manager](../../../../includes/virtual-machines-sql-server-connection-steps-resource-manager.md)]

## <a name="next-steps"></a>Nästa steg

toosee etablering instruktioner tillsammans med stegen anslutning finns [etablering av en SQL Server-dator på Azure](virtual-machines-windows-portal-sql-server-provision.md).

För andra avsnitt relaterade toorunning SQL Server i virtuella Azure-datorer, se [SQL Server på Azure Virtual Machines](virtual-machines-windows-sql-server-iaas-overview.md).
