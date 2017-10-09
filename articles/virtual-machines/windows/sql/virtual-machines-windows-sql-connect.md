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
# <a name="connect-tooa-sql-server-virtual-machine-on-azure-resource-manager"></a><span data-ttu-id="612b1-105">Ansluta tooa SQL Server-dator i Azure (Resource Manager)</span><span class="sxs-lookup"><span data-stu-id="612b1-105">Connect tooa SQL Server Virtual Machine on Azure (Resource Manager)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="612b1-106">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="612b1-106">Resource Manager</span></span>](virtual-machines-windows-sql-connect.md)
> * [<span data-ttu-id="612b1-107">Klassisk</span><span class="sxs-lookup"><span data-stu-id="612b1-107">Classic</span></span>](../classic/sql-connect.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="612b1-108">Översikt</span><span class="sxs-lookup"><span data-stu-id="612b1-108">Overview</span></span>

<span data-ttu-id="612b1-109">Det här avsnittet beskrivs hur tooconnect tooyour SQL Server-instansen körs på en virtuell Azure-dator.</span><span class="sxs-lookup"><span data-stu-id="612b1-109">This topic describes how tooconnect tooyour SQL Server instance running on an Azure virtual machine.</span></span> <span data-ttu-id="612b1-110">Det täcker vissa [allmänna anslutningsproblem scenarier](#connection-scenarios) och ger [beskrivs stegen för att konfigurera SQL Server-anslutningen i en Azure VM](#steps-for-manually-configuring-sql-server-connectivity-in-an-azure-vm).</span><span class="sxs-lookup"><span data-stu-id="612b1-110">It covers some [general connectivity scenarios](#connection-scenarios) and then provides [detailed steps for configuring SQL Server connectivity in an Azure VM](#steps-for-manually-configuring-sql-server-connectivity-in-an-azure-vm).</span></span>

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

<span data-ttu-id="612b1-111">Om du i stället måste en fullständig genomgång av både etablering och anslutningen, se [etablering av en SQL Server-dator på Azure](virtual-machines-windows-portal-sql-server-provision.md).</span><span class="sxs-lookup"><span data-stu-id="612b1-111">If you would rather have a full walk-through of both provisioning and connectivity, see [Provisioning a SQL Server Virtual Machine on Azure](virtual-machines-windows-portal-sql-server-provision.md).</span></span>

## <a name="connection-scenarios"></a><span data-ttu-id="612b1-112">-Scenarier</span><span class="sxs-lookup"><span data-stu-id="612b1-112">Connection scenarios</span></span>

<span data-ttu-id="612b1-113">hello sätt en klient ansluter tooSQL Server som körs på en virtuell dator varierar beroende på vilken hello platsen för hello klienten och hello nätverkskonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="612b1-113">hello way a client connects tooSQL Server running on a Virtual Machine differs depending on hello location of hello client and hello networking configuration.</span></span>

<span data-ttu-id="612b1-114">Om du etablerar en SQL Server-VM i hello Azure-portalen har hello möjlighet att ange hello typ av **SQL-anslutning**.</span><span class="sxs-lookup"><span data-stu-id="612b1-114">If you provision a SQL Server VM in hello Azure portal, you have hello option of specifying hello type of **SQL connectivity**.</span></span>

![Anslutningsmöjlighet för offentliga SQL under etableringen.](./media/virtual-machines-windows-sql-connect/sql-vm-portal-connectivity.png)

<span data-ttu-id="612b1-116">Alternativen för anslutning är:</span><span class="sxs-lookup"><span data-stu-id="612b1-116">Your options for connectivity include:</span></span>

| <span data-ttu-id="612b1-117">Alternativ</span><span class="sxs-lookup"><span data-stu-id="612b1-117">Option</span></span> | <span data-ttu-id="612b1-118">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="612b1-118">Description</span></span> |
|---|---|
| <span data-ttu-id="612b1-119">**Offentliga**</span><span class="sxs-lookup"><span data-stu-id="612b1-119">**Public**</span></span> | <span data-ttu-id="612b1-120">Ansluta tooSQL Server över hello internet</span><span class="sxs-lookup"><span data-stu-id="612b1-120">Connect tooSQL Server over hello internet</span></span> |
| <span data-ttu-id="612b1-121">**Privata**</span><span class="sxs-lookup"><span data-stu-id="612b1-121">**Private**</span></span> | <span data-ttu-id="612b1-122">Ansluta tooSQL Server i hello samma virtuella nätverk</span><span class="sxs-lookup"><span data-stu-id="612b1-122">Connect tooSQL Server in hello same virtual network</span></span> |
| <span data-ttu-id="612b1-123">**Lokala**</span><span class="sxs-lookup"><span data-stu-id="612b1-123">**Local**</span></span> | <span data-ttu-id="612b1-124">Ansluta tooSQL Server lokalt på hello samma virtuella dator</span><span class="sxs-lookup"><span data-stu-id="612b1-124">Connect tooSQL Server locally on hello same virtual machine</span></span> | 

<span data-ttu-id="612b1-125">hello följande avsnitt beskrivs hello **offentliga** och **privata** alternativ i detalj.</span><span class="sxs-lookup"><span data-stu-id="612b1-125">hello following sections explain hello **Public** and **Private** options in more detail.</span></span>

## <a name="connect-toosql-server-over-hello-internet"></a><span data-ttu-id="612b1-126">Ansluta tooSQL Server över hello Internet</span><span class="sxs-lookup"><span data-stu-id="612b1-126">Connect tooSQL Server over hello Internet</span></span>

<span data-ttu-id="612b1-127">Om du vill tooconnect tooyour SQL Server-databasmotorn från hello Internet väljer **offentliga** för hello **SQL-anslutning** typ i hello portal under etableringen.</span><span class="sxs-lookup"><span data-stu-id="612b1-127">If you want tooconnect tooyour SQL Server database engine from hello Internet, select **Public** for hello **SQL connectivity** type in hello portal during provisioning.</span></span> <span data-ttu-id="612b1-128">hello-portalen automatiskt hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="612b1-128">hello portal automatically does hello following steps:</span></span>

* <span data-ttu-id="612b1-129">Aktiverar hello TCP/IP-protokollet för SQL Server.</span><span class="sxs-lookup"><span data-stu-id="612b1-129">Enables hello TCP/IP protocol for SQL Server.</span></span>
* <span data-ttu-id="612b1-130">Konfigurerar en brandvägg regeln tooopen hello SQL Server TCP-porten (standard 1433).</span><span class="sxs-lookup"><span data-stu-id="612b1-130">Configures a firewall rule tooopen hello SQL Server TCP port (default 1433).</span></span>
* <span data-ttu-id="612b1-131">Gör det möjligt för SQL Server-autentisering krävs för allmän åtkomst.</span><span class="sxs-lookup"><span data-stu-id="612b1-131">Enables SQL Server Authentication, required for public access.</span></span>
* <span data-ttu-id="612b1-132">Konfigurerar hello nätverkssäkerhetsgruppen på hello VM tooall TCP-trafik på hello port för SQL Server.</span><span class="sxs-lookup"><span data-stu-id="612b1-132">Configures hello network security group on hello VM tooall TCP traffic on hello SQL Server port.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="612b1-133">hello virtuella avbildningar för hello SQL Server Developer och Express-versioner aktiveras inte hello TCP/IP-protokollet.</span><span class="sxs-lookup"><span data-stu-id="612b1-133">hello virtual machine images for hello SQL Server Developer and Express editions do not automatically enable hello TCP/IP protocol.</span></span> <span data-ttu-id="612b1-134">För utvecklare och Express-versioner måste du använda SQL Server Configuration Manager för[manuellt Aktivera hello TCP/IP-protokollet](#manualtcp) hello VM när du har skapat.</span><span class="sxs-lookup"><span data-stu-id="612b1-134">For Developer and Express editions, you must use SQL Server Configuration Manager too[manually enable hello TCP/IP protocol](#manualtcp) after creating hello VM.</span></span>

<span data-ttu-id="612b1-135">Alla klienter med åtkomst till internet kan ansluta toohello SQL Server-instansen genom att ange hello offentliga IP-adressen för hello virtuell dator eller en DNS-etikett som tilldelats toothat IP-adress.</span><span class="sxs-lookup"><span data-stu-id="612b1-135">Any client with internet access can connect toohello SQL Server instance by specifying either hello public IP address of hello virtual machine or any DNS label assigned toothat IP address.</span></span> <span data-ttu-id="612b1-136">Om hello SQL Server-port 1433, behöver du inte toospecify den i hello anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="612b1-136">If hello SQL Server port is 1433, you do not need toospecify it in hello connection string.</span></span> <span data-ttu-id="612b1-137">hello följande anslutningssträng ansluter tooa SQL VM till en DNS-etikett för `sqlvmlabel.eastus.cloudapp.azure.com` med SQL-autentisering (du kan också använda hello offentlig IP-adress).</span><span class="sxs-lookup"><span data-stu-id="612b1-137">hello following connection string connects tooa SQL VM with a DNS label of `sqlvmlabel.eastus.cloudapp.azure.com` using SQL Authentication (you could also use hello public IP address).</span></span>

```
Server=sqlvmlabel.eastus.cloudapp.azure.com;Integrated Security=false;User ID=<login_name>;Password=<your_password>
```

<span data-ttu-id="612b1-138">Även om detta aktiverar anslutning för klienter över Hej internet, detta innebär inte att någon kan ansluta tooyour SQL Server.</span><span class="sxs-lookup"><span data-stu-id="612b1-138">Although this enables connectivity for clients over hello internet, this does not imply that anyone can connect tooyour SQL Server.</span></span> <span data-ttu-id="612b1-139">Utanför klienter ha toohello rätt användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="612b1-139">Outside clients have toohello correct username and password.</span></span> <span data-ttu-id="612b1-140">För ytterligare säkerhet kan kan du undvika hello välkända port 1433.</span><span class="sxs-lookup"><span data-stu-id="612b1-140">However, for additional security, you can avoid hello well-known port 1433.</span></span> <span data-ttu-id="612b1-141">Till exempel om du har konfigurerat SQL Server toolisten på port 1500 och upprätta rätt brandvägg och regler för nätverkssäkerhetsgrupper kan du ansluta genom att lägga till hello port number toohello servernamn.</span><span class="sxs-lookup"><span data-stu-id="612b1-141">For example, if you configured SQL Server toolisten on port 1500 and established proper firewall and network security group rules, you could connect by appending hello port number toohello Server name.</span></span> <span data-ttu-id="612b1-142">hello följande exempel ändrar hello föregående genom att lägga till ett anpassat portnummer **1500**, toohello servernamn:</span><span class="sxs-lookup"><span data-stu-id="612b1-142">hello following example alters hello previous one by adding a custom port number, **1500**, toohello server name:</span></span>

```
Server=sqlvmlabel.eastus.cloudapp.azure.com,1500;Integrated Security=false;User ID=<login_name>;Password=<your_password>"
```

> [!NOTE]
> <span data-ttu-id="612b1-143">När du frågar SQL Server på en virtuell dator över hello internet, alla utgående data från hello Azure datacenter är ämne toonormal [på utgående dataöverföringar](https://azure.microsoft.com/pricing/details/data-transfers/).</span><span class="sxs-lookup"><span data-stu-id="612b1-143">When you query SQL Server in a VM over hello internet, all outgoing data from hello Azure datacenter is subject toonormal [pricing on outbound data transfers](https://azure.microsoft.com/pricing/details/data-transfers/).</span></span>

## <a name="connect-toosql-server-within-a-virtual-network"></a><span data-ttu-id="612b1-144">Ansluta tooSQL Server inom ett virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="612b1-144">Connect tooSQL Server within a virtual network</span></span>

<span data-ttu-id="612b1-145">När du väljer **privata** för hello **SQL-anslutning** typ i hello-portalen, Azure konfigurerar de flesta av hello inställningarna identiskt för**offentliga**.</span><span class="sxs-lookup"><span data-stu-id="612b1-145">When you choose **Private** for hello **SQL connectivity** type in hello portal, Azure configures most of hello settings identical too**Public**.</span></span> <span data-ttu-id="612b1-146">hello en skillnaden är att det finns inga network security grupp regeln tooallow utanför trafik på hello SQL Server-porten (standard 1433).</span><span class="sxs-lookup"><span data-stu-id="612b1-146">hello one difference is that there is no network security group rule tooallow outside traffic on hello SQL Server port (default 1433).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="612b1-147">hello virtuella avbildningar för hello SQL Server Developer och Express-versioner aktiveras inte hello TCP/IP-protokollet.</span><span class="sxs-lookup"><span data-stu-id="612b1-147">hello virtual machine images for hello SQL Server Developer and Express editions do not automatically enable hello TCP/IP protocol.</span></span> <span data-ttu-id="612b1-148">För utvecklare och Express-versioner måste du använda SQL Server Configuration Manager för[manuellt Aktivera hello TCP/IP-protokollet](#manualtcp) hello VM när du har skapat.</span><span class="sxs-lookup"><span data-stu-id="612b1-148">For Developer and Express editions, you must use SQL Server Configuration Manager too[manually enable hello TCP/IP protocol](#manualtcp) after creating hello VM.</span></span>

<span data-ttu-id="612b1-149">Privata anslutningen används ofta tillsammans med [virtuellt nätverk](../../../virtual-network/virtual-networks-overview.md), vilket gör att flera scenarier.</span><span class="sxs-lookup"><span data-stu-id="612b1-149">Private connectivity is often used in conjunction with [Virtual Network](../../../virtual-network/virtual-networks-overview.md), which enables several scenarios.</span></span> <span data-ttu-id="612b1-150">Du kan ansluta virtuella datorer i samma virtuella nätverk, även om de virtuella datorer finns i olika resursgrupper hello.</span><span class="sxs-lookup"><span data-stu-id="612b1-150">You can connect VMs in hello same virtual network, even if those VMs exist in different resource groups.</span></span> <span data-ttu-id="612b1-151">Och med en [plats-till-plats VPN](../../../vpn-gateway/vpn-gateway-site-to-site-create.md), kan du skapa en hybrid-arkitektur som ansluter virtuella datorer med lokala nätverk och datorer.</span><span class="sxs-lookup"><span data-stu-id="612b1-151">And with a [site-to-site VPN](../../../vpn-gateway/vpn-gateway-site-to-site-create.md), you can create a hybrid architecture that connects VMs with on-premises networks and machines.</span></span>

<span data-ttu-id="612b1-152">Virtuella nätverk kan också du toojoin din virtuella Azure-datorer tooa domän.</span><span class="sxs-lookup"><span data-stu-id="612b1-152">Virtual networks also enable     you toojoin your Azure VMs tooa domain.</span></span> <span data-ttu-id="612b1-153">Detta är hello endast sätt toouse Windows-autentisering tooSQL Server.</span><span class="sxs-lookup"><span data-stu-id="612b1-153">This is hello only way toouse Windows Authentication tooSQL Server.</span></span> <span data-ttu-id="612b1-154">hello kräver andra scenarier SQL-autentisering med användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="612b1-154">hello other connection scenarios require SQL Authentication with user names and passwords.</span></span>

<span data-ttu-id="612b1-155">Förutsatt att du har konfigurerat DNS i ditt virtuella nätverk kan ansluta du tooyour SQL Server-instansen genom att ange hello datornamn för SQL Server-VM hello anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="612b1-155">Assuming that you have configured DNS in your virtual network, you can connect tooyour SQL Server instance by specifying hello SQL Server VM computer name in hello connection string.</span></span> <span data-ttu-id="612b1-156">hello också följande exempel förutsätter att Windows-autentisering har konfigurerats och hello användaren har beviljats åtkomst toohello SQL Server-instansen.</span><span class="sxs-lookup"><span data-stu-id="612b1-156">hello following example also assumes that Windows Authentication has also been configured and that hello user has been granted access toohello SQL Server instance.</span></span>

```
Server=mysqlvm;Integrated Security=true
```

## <span data-ttu-id="612b1-157"><a id="change"></a>Ändra inställningar för SQL-anslutning</span><span class="sxs-lookup"><span data-stu-id="612b1-157"><a id="change"></a> Change SQL connectivity settings</span></span>

<span data-ttu-id="612b1-158">Du kan ändra hello anslutningsinställningar för den virtuella datorn i SQL Server i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="612b1-158">You can change hello connectivity settings for your SQL Server virtual machine in hello Azure portal.</span></span>

1. <span data-ttu-id="612b1-159">Välj i hello Azure-portalen, **virtuella datorer**.</span><span class="sxs-lookup"><span data-stu-id="612b1-159">In hello Azure portal, select **Virtual Machines**.</span></span>

2. <span data-ttu-id="612b1-160">Välj din SQLServer-VM.</span><span class="sxs-lookup"><span data-stu-id="612b1-160">Select your SQL Server VM.</span></span>

3. <span data-ttu-id="612b1-161">Under **inställningar**, klickar du på **SQL Server-konfigurationsfilen**.</span><span class="sxs-lookup"><span data-stu-id="612b1-161">Under **Settings**, click **SQL Server configuration**.</span></span>

4. <span data-ttu-id="612b1-162">Ändra hello **SQL anslutningen nivån** tooyour som krävs för inställningen.</span><span class="sxs-lookup"><span data-stu-id="612b1-162">Change hello **SQL connectivity level** tooyour required setting.</span></span> <span data-ttu-id="612b1-163">Du kan eventuellt använda den här område toochange hello port för SQL Server eller autentiseringsinställningar för hello SQL.</span><span class="sxs-lookup"><span data-stu-id="612b1-163">You can optionally use this area toochange hello SQL Server port or hello SQL Authentication settings.</span></span>

   ![Ändra SQL-anslutning](./media/virtual-machines-windows-sql-connect/sql-vm-portal-connectivity-change.png)

5. <span data-ttu-id="612b1-165">Vänta några minuter för hello uppdatering toocomplete.</span><span class="sxs-lookup"><span data-stu-id="612b1-165">Wait several minutes for hello update toocomplete.</span></span>

   ![SQL-VM uppdateringsmeddelande](./media/virtual-machines-windows-sql-connect/sql-vm-updating-notification.png)

## <span data-ttu-id="612b1-167"><a id="manualtcp"></a>Aktivera TCP/IP för utvecklare och Express-versioner</span><span class="sxs-lookup"><span data-stu-id="612b1-167"><a id="manualtcp"></a> Enable TCP/IP for Developer and Express editions</span></span>

<span data-ttu-id="612b1-168">När du ändrar inställningarna för SQL Server-anslutningen, aktiverar Azure automatiskt inte hello TCP/IP-protokollet för SQL Server Developer och Express Edition.</span><span class="sxs-lookup"><span data-stu-id="612b1-168">When changing SQL Server connectivity settings, Azure does not automatically enable hello TCP/IP protocol for SQL Server Developer and Express editions.</span></span> <span data-ttu-id="612b1-169">hello stegen nedan förklarar hur toomanually aktivera TCP/IP så att du kan fjärransluta efter IP-adress.</span><span class="sxs-lookup"><span data-stu-id="612b1-169">hello steps below explain how toomanually enable TCP/IP so that you can connect remotely by IP address.</span></span>

<span data-ttu-id="612b1-170">Anslut toohello SQL Server-datorn med fjärrskrivbord.</span><span class="sxs-lookup"><span data-stu-id="612b1-170">First, connect toohello SQL Server machine with remote desktop.</span></span>

> [!INCLUDE [Connect tooSQL Server VM with remote desktop](../../../../includes/virtual-machines-sql-server-remote-desktop-connect.md)]

<span data-ttu-id="612b1-171">Aktivera sedan hello TCP/IP-protokollet med **SQL Server Configuration Manager**.</span><span class="sxs-lookup"><span data-stu-id="612b1-171">Next, enable hello TCP/IP protocol with **SQL Server Configuration Manager**.</span></span>

> [!INCLUDE [Connect tooSQL Server VM with remote desktop](../../../../includes/virtual-machines-sql-server-connection-tcp-protocol.md)]

## <a name="connect-with-ssms"></a><span data-ttu-id="612b1-172">Anslut med SSMS</span><span class="sxs-lookup"><span data-stu-id="612b1-172">Connect with SSMS</span></span>

<span data-ttu-id="612b1-173">hello följande steg visar hur toocreate ett valfritt DNS etikett för din virtuella Azure-datorn och ansluter sedan med SQL Server Management Studio (SSMS).</span><span class="sxs-lookup"><span data-stu-id="612b1-173">hello following steps show how toocreate an optional DNS Label for your Azure VM and then connect with SQL Server Management Studio (SSMS).</span></span>

[!INCLUDE [Connect tooSQL Server in a VM Resource Manager](../../../../includes/virtual-machines-sql-server-connection-steps-resource-manager.md)]

## <a name="next-steps"></a><span data-ttu-id="612b1-174">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="612b1-174">Next Steps</span></span>

<span data-ttu-id="612b1-175">toosee etablering instruktioner tillsammans med stegen anslutning finns [etablering av en SQL Server-dator på Azure](virtual-machines-windows-portal-sql-server-provision.md).</span><span class="sxs-lookup"><span data-stu-id="612b1-175">toosee provisioning instructions along with these connectivity steps, see [Provisioning a SQL Server Virtual Machine on Azure](virtual-machines-windows-portal-sql-server-provision.md).</span></span>

<span data-ttu-id="612b1-176">För andra avsnitt relaterade toorunning SQL Server i virtuella Azure-datorer, se [SQL Server på Azure Virtual Machines](virtual-machines-windows-sql-server-iaas-overview.md).</span><span class="sxs-lookup"><span data-stu-id="612b1-176">For other topics related toorunning SQL Server in Azure VMs, see [SQL Server on Azure Virtual Machines](virtual-machines-windows-sql-server-iaas-overview.md).</span></span>
