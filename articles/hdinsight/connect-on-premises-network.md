---
title: "aaaConnect HDInsight tooyour lokalt nätverk - Azure HDInsight | Microsoft Docs"
description: "Lär dig hur toocreate ett HDInsight-kluster i en Azure-nätverk och ansluter sedan tooyour lokalt nätverk. Lär dig hur tooconfigure namnmatchning mellan HDInsight och ditt lokala nätverk med hjälp av en anpassad DNS-server."
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/21/2017
ms.author: larryfr
ms.openlocfilehash: 8a3adf0e3df7726d8e6566d723700506baaf89a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-hdinsight-tooyour-on-premise-network"></a><span data-ttu-id="f36b5-104">Ansluta HDInsight tooyour lokalt nätverk</span><span class="sxs-lookup"><span data-stu-id="f36b5-104">Connect HDInsight tooyour on-premise network</span></span>

<span data-ttu-id="f36b5-105">Lär dig hur tooconnect HDInsight tooyour lokalt nätverk med hjälp av virtuella Azure-nätverk och en VPN-gateway.</span><span class="sxs-lookup"><span data-stu-id="f36b5-105">Learn how tooconnect HDInsight tooyour on-premises network by using Azure Virtual Networks and a VPN gateway.</span></span> <span data-ttu-id="f36b5-106">Det här dokumentet innehåller planeringsinformation om:</span><span class="sxs-lookup"><span data-stu-id="f36b5-106">This document provides planning information on:</span></span>

* <span data-ttu-id="f36b5-107">Använda HDInsight i ett virtuellt Azure-nätverk som ansluter tooyour lokalt nätverk.</span><span class="sxs-lookup"><span data-stu-id="f36b5-107">Using HDInsight in an Azure Virtual Network that connects tooyour on-premises network.</span></span>

* <span data-ttu-id="f36b5-108">Konfigurera DNS-namnmatchningen mellan hello virtuella nätverket och ditt lokala nätverk.</span><span class="sxs-lookup"><span data-stu-id="f36b5-108">Configuring DNS name resolution between hello virtual network and your on-premises network.</span></span>

* <span data-ttu-id="f36b5-109">Konfigurera network security grupper toorestrict internet access tooHDInsight.</span><span class="sxs-lookup"><span data-stu-id="f36b5-109">Configuring network security groups toorestrict internet access tooHDInsight.</span></span>

* <span data-ttu-id="f36b5-110">Portar som tillhandahålls av HDInsight hello virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="f36b5-110">Ports provided by HDInsight on hello virtual network.</span></span>

## <a name="create-hello-virtual-network-configuration"></a><span data-ttu-id="f36b5-111">Skapa hello virtuella nätverkskonfigurationen</span><span class="sxs-lookup"><span data-stu-id="f36b5-111">Create hello Virtual network configuration</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f36b5-112">Om du letar efter steg-för-steg-vägledning om hur du ansluter HDInsight tooyour lokalt nätverk via ett virtuellt Azure-nätverk, se hello [ansluta HDInsight tooyour lokalt nätverk](connect-on-premises-network.md) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="f36b5-112">If you are looking for step by step guidance on connecting HDInsight tooyour on-premises network using an Azure Virtual Network, see hello [Connect HDInsight tooyour on-premise network](connect-on-premises-network.md) document.</span></span>

<span data-ttu-id="f36b5-113">Använd hello följande dokument toolearn hur toocreate ett virtuellt Azure-nätverk som är anslutna tooyour lokala nätverk:</span><span class="sxs-lookup"><span data-stu-id="f36b5-113">Use hello following documents toolearn how toocreate an Azure Virtual Network that is connected tooyour on-premises network:</span></span>
    
* [<span data-ttu-id="f36b5-114">Med hjälp av hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="f36b5-114">Using hello Azure portal</span></span>](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)

* [<span data-ttu-id="f36b5-115">Använda Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="f36b5-115">Using Azure PowerShell</span></span>](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)

* [<span data-ttu-id="f36b5-116">Använda Azure CLI</span><span class="sxs-lookup"><span data-stu-id="f36b5-116">Using Azure CLI</span></span>](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-cli.md)

## <a name="configure-name-resolution"></a><span data-ttu-id="f36b5-117">Konfigurera namnmatchning</span><span class="sxs-lookup"><span data-stu-id="f36b5-117">Configure name resolution</span></span>

<span data-ttu-id="f36b5-118">tooallow HDInsight och resurser i hello anslutna nätverket toocommunicate efter namn, måste du utföra följande åtgärder hello:</span><span class="sxs-lookup"><span data-stu-id="f36b5-118">tooallow HDInsight and resources in hello joined network toocommunicate by name, you must perform hello following actions:</span></span>

* <span data-ttu-id="f36b5-119">Skapa en anpassad DNS-server i hello Azure Virtual Network.</span><span class="sxs-lookup"><span data-stu-id="f36b5-119">Create a custom DNS server in hello Azure Virtual Network.</span></span>

* <span data-ttu-id="f36b5-120">Konfigurera hello virtuellt nätverk toouse hello anpassad DNS-server i stället för default hello Azure rekursiv matchning.</span><span class="sxs-lookup"><span data-stu-id="f36b5-120">Configure hello virtual network toouse hello custom DNS server instead of hello default Azure Recursive Resolver.</span></span>

* <span data-ttu-id="f36b5-121">Konfigurera vidarebefordran mellan hello anpassad DNS-server och lokala DNS-servern.</span><span class="sxs-lookup"><span data-stu-id="f36b5-121">Configure forwarding between hello custom DNS server and your on-premises DNS server.</span></span>

<span data-ttu-id="f36b5-122">Den här konfigurationen kan hello följande beteende:</span><span class="sxs-lookup"><span data-stu-id="f36b5-122">This configuration enables hello following behavior:</span></span>

* <span data-ttu-id="f36b5-123">Begäranden om fullständiga domännamn som har hello DNS-suffix __för hello virtuella nätverket__ vidarebefordras toohello anpassad DNS-server.</span><span class="sxs-lookup"><span data-stu-id="f36b5-123">Requests for fully qualified domain names that have hello DNS suffix __for hello virtual network__ are forwarded toohello custom DNS server.</span></span> <span data-ttu-id="f36b5-124">hello anpassade DNS-servern vidarebefordrar sedan dessa begäranden toohello Azure rekursiv matcharen som returnerar hello IP-adress.</span><span class="sxs-lookup"><span data-stu-id="f36b5-124">hello custom DNS server then forwards these requests toohello Azure Recursive Resolver, which returns hello IP address.</span></span>

* <span data-ttu-id="f36b5-125">Alla andra begäranden vidarebefordras toohello lokala DNS-servern.</span><span class="sxs-lookup"><span data-stu-id="f36b5-125">All other requests are forwarded toohello on-premises DNS server.</span></span> <span data-ttu-id="f36b5-126">Även begäranden för offentliga internet-resurser, till exempel microsoft.com vidarebefordras toohello lokala DNS-server för namnmatchning.</span><span class="sxs-lookup"><span data-stu-id="f36b5-126">Even requests for public internet resources such as microsoft.com are forwarded toohello on-premises DNS server for name resolution.</span></span>

<span data-ttu-id="f36b5-127">I följande diagram hello, är gröna linjer begäranden för resurser som slutar i hello DNS-suffix hello virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="f36b5-127">In hello following diagram, green lines are requests for resources that end in hello DNS suffix of hello virtual network.</span></span> <span data-ttu-id="f36b5-128">Blå raderna är begäranden för resurser i hello lokala nätverk eller på hello offentliga internet.</span><span class="sxs-lookup"><span data-stu-id="f36b5-128">Blue lines are requests for resources in hello on-premises network or on hello public internet.</span></span>

![Diagram över hur DNS-förfrågningar har lösts i hello konfiguration används i det här dokumentet](./media/connect-on-premises-network/on-premises-to-cloud-dns.png)

### <a name="create-a-custom-dns-server"></a><span data-ttu-id="f36b5-130">Skapa en anpassad DNS-server</span><span class="sxs-lookup"><span data-stu-id="f36b5-130">Create a custom DNS server</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f36b5-131">Du måste skapa och konfigurera hello DNS-servern innan du installerar HDInsight i hello virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="f36b5-131">You must create and configure hello DNS server before installing HDInsight into hello virtual network.</span></span>

<span data-ttu-id="f36b5-132">toocreate en Linux VM som använder hello [binda](https://www.isc.org/downloads/bind/) DNS-programvara, Använd hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="f36b5-132">toocreate a Linux VM that uses hello [Bind](https://www.isc.org/downloads/bind/) DNS software, use hello following steps:</span></span>

> [!NOTE]
> <span data-ttu-id="f36b5-133">hello följande använda hello [Azure-portalen](https://portal.azure.com) toocreate en virtuell dator i Azure.</span><span class="sxs-lookup"><span data-stu-id="f36b5-133">hello following steps use hello [Azure portal](https://portal.azure.com) toocreate an Azure Virtual Machine.</span></span> <span data-ttu-id="f36b5-134">Andra sätt toocreate en virtuell dator finns hello [skapa VM - Azure CLI](../virtual-machines/linux/quick-create-cli.md) och [skapa VM - Azure PowerShell](../virtual-machines/linux/quick-create-portal.md) dokument.</span><span class="sxs-lookup"><span data-stu-id="f36b5-134">For other ways toocreate a virtual machine, see hello [Create VM - Azure CLI](../virtual-machines/linux/quick-create-cli.md) and [Create VM - Azure PowerShell](../virtual-machines/linux/quick-create-portal.md) documents.</span></span>

1. <span data-ttu-id="f36b5-135">Från hello [Azure-portalen](https://portal.azure.com)väljer  __+__ , __Compute__, och __Ubuntu Server 16.04 LTS__.</span><span class="sxs-lookup"><span data-stu-id="f36b5-135">From hello [Azure portal](https://portal.azure.com), select __+__, __Compute__, and __Ubuntu Server 16.04 LTS__.</span></span>

    ![Skapa en virtuell Ubuntu-dator](./media/connect-on-premises-network/create-ubuntu-vm.png)

2. <span data-ttu-id="f36b5-137">Från hello __grunderna__ ange hello följande information:</span><span class="sxs-lookup"><span data-stu-id="f36b5-137">From hello __Basics__ section, enter hello following information:</span></span>

    * <span data-ttu-id="f36b5-138">__Namnet__: ett eget namn som identifierar den här virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="f36b5-138">__Name__: A friendly name that identifies this virtual machine.</span></span> <span data-ttu-id="f36b5-139">Till exempel __DNSProxy__.</span><span class="sxs-lookup"><span data-stu-id="f36b5-139">For example, __DNSProxy__.</span></span>
    * <span data-ttu-id="f36b5-140">__Användarnamnet__: hello namnet på hello SSH-konto.</span><span class="sxs-lookup"><span data-stu-id="f36b5-140">__User name__: hello name of hello SSH account.</span></span>
    * <span data-ttu-id="f36b5-141">__Offentlig SSH-nyckel__ eller __lösenord__: hello autentiseringsmetod för hello SSH-konto.</span><span class="sxs-lookup"><span data-stu-id="f36b5-141">__SSH public key__ or __Password__: hello authentication method for hello SSH account.</span></span> <span data-ttu-id="f36b5-142">Vi rekommenderar att du använder offentliga nycklar som de är säkrare.</span><span class="sxs-lookup"><span data-stu-id="f36b5-142">We recommend using public keys, as they are more secure.</span></span> <span data-ttu-id="f36b5-143">Mer information finns i hello [skapa och använda SSH-nycklar för Linux virtuella datorer](../virtual-machines/linux/mac-create-ssh-keys.md) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="f36b5-143">For more information, see hello [Create and use SSH keys for Linux VMs](../virtual-machines/linux/mac-create-ssh-keys.md) document.</span></span>
    * <span data-ttu-id="f36b5-144">__Resursgruppen__: Välj __använda befintliga__, och välj sedan hello resursgruppen som innehåller hello virtuella nätverk som skapades tidigare.</span><span class="sxs-lookup"><span data-stu-id="f36b5-144">__Resource group__: Select __Use existing__, and then select hello resource group that contains hello virtual network created earlier.</span></span>
    * <span data-ttu-id="f36b5-145">__Plats__: Välj hello samma plats som hello virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="f36b5-145">__Location__: Select hello same location as hello virtual network.</span></span>

    ![Grundläggande konfiguration av virtuell dator](./media/connect-on-premises-network/vm-basics.png)

    <span data-ttu-id="f36b5-147">Lämna andra transaktioner på hello standardvärden och välj sedan __OK__.</span><span class="sxs-lookup"><span data-stu-id="f36b5-147">Leave other entries at hello default values and then select __OK__.</span></span>

3. <span data-ttu-id="f36b5-148">Från hello __välja en storlek__ avsnittet väljer hello VM-storlek.</span><span class="sxs-lookup"><span data-stu-id="f36b5-148">From hello __Choose a size__ section, select hello VM size.</span></span> <span data-ttu-id="f36b5-149">Välj hello minsta och lägsta kostnaden alternativet för den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="f36b5-149">For this tutorial, select hello smallest and lowest cost option.</span></span> <span data-ttu-id="f36b5-150">toocontinue, Använd hello __Välj__ knappen.</span><span class="sxs-lookup"><span data-stu-id="f36b5-150">toocontinue, use hello __Select__ button.</span></span>

4. <span data-ttu-id="f36b5-151">Från hello __inställningar__ ange hello följande information:</span><span class="sxs-lookup"><span data-stu-id="f36b5-151">From hello __Settings__ section, enter hello following information:</span></span>

    * <span data-ttu-id="f36b5-152">__Virtuellt nätverk__: Välj hello virtuella nätverk som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="f36b5-152">__Virtual network__: Select hello virtual network that you created earlier.</span></span>

    * <span data-ttu-id="f36b5-153">__Undernät__: Välj hello standardundernät för hello virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="f36b5-153">__Subnet__: Select hello default subnet for hello virtual network.</span></span> <span data-ttu-id="f36b5-154">Gör __inte__ Välj hello undernät som används av hello VPN-gateway.</span><span class="sxs-lookup"><span data-stu-id="f36b5-154">Do __not__ select hello subnet used by hello VPN gateway.</span></span>

    * <span data-ttu-id="f36b5-155">__Diagnostiklagringskonto__: Välj ett befintligt lagringskonto eller skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="f36b5-155">__Diagnostics storage account__: Either select an existing storage account or create a new one.</span></span>

    ![Inställningarna för virtuella nätverk](./media/connect-on-premises-network/virtual-network-settings.png)

    <span data-ttu-id="f36b5-157">Lämna hello andra transaktioner på hello standardvärdet, och välj sedan __OK__ toocontinue.</span><span class="sxs-lookup"><span data-stu-id="f36b5-157">Leave hello other entries at hello default value, then select __OK__ toocontinue.</span></span>

5. <span data-ttu-id="f36b5-158">Från hello __inköp__ avsnitt, Välj hello __inköp__ knappen toocreate hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="f36b5-158">From hello __Purchase__ section, select hello __Purchase__ button toocreate hello virtual machine.</span></span>

6. <span data-ttu-id="f36b5-159">När hello virtuella datorn har skapats, dess __översikt__ avsnitt visas.</span><span class="sxs-lookup"><span data-stu-id="f36b5-159">Once hello virtual machine has been created, its __Overview__ section is displayed.</span></span> <span data-ttu-id="f36b5-160">Välj hello listan hello vänster __egenskaper__.</span><span class="sxs-lookup"><span data-stu-id="f36b5-160">From hello list on hello left, select __Properties__.</span></span> <span data-ttu-id="f36b5-161">Spara hello __offentliga IP-adressen__ och __privata IP-adressen__ värden.</span><span class="sxs-lookup"><span data-stu-id="f36b5-161">Save hello __Public IP address__ and __Private IP address__ values.</span></span> <span data-ttu-id="f36b5-162">Den används i nästa avsnitt om hello.</span><span class="sxs-lookup"><span data-stu-id="f36b5-162">It will be used in hello next section.</span></span>

    ![Offentliga och privata IP-adresser](./media/connect-on-premises-network/vm-ip-addresses.png)

### <a name="install-and-configure-bind-dns-software"></a><span data-ttu-id="f36b5-164">Installera och konfigurera Bind (DNS-programvara)</span><span class="sxs-lookup"><span data-stu-id="f36b5-164">Install and configure Bind (DNS software)</span></span>

1. <span data-ttu-id="f36b5-165">Använda SSH tooconnect toohello __offentliga IP-adressen__ för hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="f36b5-165">Use SSH tooconnect toohello __public IP address__ of hello virtual machine.</span></span> <span data-ttu-id="f36b5-166">följande exempel hello ansluter tooa virtuell dator på 40.68.254.142:</span><span class="sxs-lookup"><span data-stu-id="f36b5-166">hello following example connects tooa virtual machine at 40.68.254.142:</span></span>

    ```bash
    ssh sshuser@40.68.254.142
    ```

    <span data-ttu-id="f36b5-167">Ersätt `sshuser` med hello SSH-användarkonto du angav när du skapar hello kluster.</span><span class="sxs-lookup"><span data-stu-id="f36b5-167">Replace `sshuser` with hello SSH user account you specified when creating hello cluster.</span></span>

    > [!NOTE]
    > <span data-ttu-id="f36b5-168">Det finns en mängd olika sätt tooobtain hello `ssh` verktyget.</span><span class="sxs-lookup"><span data-stu-id="f36b5-168">There are a variety of ways tooobtain hello `ssh` utility.</span></span> <span data-ttu-id="f36b5-169">Linux-, Unix- och macOS anges som en del av hello-operativsystemet.</span><span class="sxs-lookup"><span data-stu-id="f36b5-169">On Linux, Unix, and macOS, it is provided as part of hello operating system.</span></span> <span data-ttu-id="f36b5-170">Om du använder Windows, bör du något av följande alternativ för hello:</span><span class="sxs-lookup"><span data-stu-id="f36b5-170">If you are using Windows, consider one of hello following options:</span></span>
    >
    > * [<span data-ttu-id="f36b5-171">Azure-molnet Shell</span><span class="sxs-lookup"><span data-stu-id="f36b5-171">Azure Cloud Shell</span></span>](../cloud-shell/quickstart.md)
    > * [<span data-ttu-id="f36b5-172">Bash på Ubuntu på Windows 10</span><span class="sxs-lookup"><span data-stu-id="f36b5-172">Bash on Ubuntu on Windows 10</span></span>](https://msdn.microsoft.com/commandline/wsl/about)
    > * [<span data-ttu-id="f36b5-173">Git (https://git-scm.com/)</span><span class="sxs-lookup"><span data-stu-id="f36b5-173">Git (https://git-scm.com/)</span></span>](https://git-scm.com/)
    > * [<span data-ttu-id="f36b5-174">OpenSSH (https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH)</span><span class="sxs-lookup"><span data-stu-id="f36b5-174">OpenSSH (https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH)</span></span>](https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH)

2. <span data-ttu-id="f36b5-175">tooinstall bindning, Använd följande kommandon från hello SSH-session hello:</span><span class="sxs-lookup"><span data-stu-id="f36b5-175">tooinstall Bind, use hello following commands from hello SSH session:</span></span>

    ```bash
    sudo apt-get update -y
    sudo apt-get install bind9 -y
    ```

3. <span data-ttu-id="f36b5-176">tooconfigure Bind tooforward namn upplösning begäranden tooyour lokala DNS-servern använder hello följande text som hello hello `/etc/bind/named.conf.options` fil:</span><span class="sxs-lookup"><span data-stu-id="f36b5-176">tooconfigure Bind tooforward name resolution requests tooyour on-prem DNS server, use hello following text as hello contents of hello `/etc/bind/named.conf.options` file:</span></span>

        acl goodclients {
            10.0.0.0/16; # Replace with hello IP address range of hello virtual network
            10.1.0.0/16; # Replace with hello IP address range of hello on-premises network
            localhost;
            localnets;
        };

        options {
                directory "/var/cache/bind";

                recursion yes;

                allow-query { goodclients; };

                forwarders {
                192.168.0.1; # Replace with hello IP address of hello on-premises DNS server
                };

                dnssec-validation auto;

                auth-nxdomain no;    # conform tooRFC1035
                listen-on { any; };
        };

    > [!IMPORTANT]
    > <span data-ttu-id="f36b5-177">Ersätt hello värden i hello `goodclients` avsnitt med hello IP-adressintervall i hello virtuella nätverk och lokala nätverk.</span><span class="sxs-lookup"><span data-stu-id="f36b5-177">Replace hello values in hello `goodclients` section with hello IP address range of hello virtual network and on-premises network.</span></span> <span data-ttu-id="f36b5-178">Det här avsnittet definierar hello-adresser som DNS-servern accepterar begäranden från.</span><span class="sxs-lookup"><span data-stu-id="f36b5-178">This section defines hello addresses that this DNS server accepts requests from.</span></span>
    >
    > <span data-ttu-id="f36b5-179">Ersätt hello `192.168.0.1` post i hello `forwarders` avsnitt med hello IP-adress i lokala DNS-servern.</span><span class="sxs-lookup"><span data-stu-id="f36b5-179">Replace hello `192.168.0.1` entry in hello `forwarders` section with hello IP address of your on-premises DNS server.</span></span> <span data-ttu-id="f36b5-180">Den här posten vägar DNS-förfrågningar tooyour lokala DNS-servern för namnmatchning.</span><span class="sxs-lookup"><span data-stu-id="f36b5-180">This entry routes DNS requests tooyour on-premises DNS server for resolution.</span></span>

    <span data-ttu-id="f36b5-181">tooedit den här filen, Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="f36b5-181">tooedit this file, use hello following command:</span></span>

    ```bash
    sudo nano /etc/bind/named.conf.options
    ```

    <span data-ttu-id="f36b5-182">toosave hello-fil, Använd __Ctrl + X__, __Y__, och sedan __RETUR__.</span><span class="sxs-lookup"><span data-stu-id="f36b5-182">toosave hello file, use __Ctrl+X__, __Y__, and then __Enter__.</span></span>

4. <span data-ttu-id="f36b5-183">Använd följande kommando hello från hello SSH-sessionen:</span><span class="sxs-lookup"><span data-stu-id="f36b5-183">From hello SSH session, use hello following command:</span></span>

    ```bash
    hostname -f
    ```

    <span data-ttu-id="f36b5-184">Det här kommandot returnerar ett värde liknande toohello följande text:</span><span class="sxs-lookup"><span data-stu-id="f36b5-184">This command returns a value similar toohello following text:</span></span>

        dnsproxy.icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net

    <span data-ttu-id="f36b5-185">Hej `icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net` texten är hello __DNS-suffix__ för den här virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="f36b5-185">hello `icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net` text is hello __DNS suffix__ for this virtual network.</span></span> <span data-ttu-id="f36b5-186">Spara det här värdet eftersom det används senare.</span><span class="sxs-lookup"><span data-stu-id="f36b5-186">Save this value, as it is used later.</span></span>

5. <span data-ttu-id="f36b5-187">tooconfigure Bind tooresolve DNS-namn för resurser inom hello virtuellt nätverk, använder hello följande text som hello hello `/etc/bind/named.conf.local` fil:</span><span class="sxs-lookup"><span data-stu-id="f36b5-187">tooconfigure Bind tooresolve DNS names for resources within hello virtual network, use hello following text as hello contents of hello `/etc/bind/named.conf.local` file:</span></span>

        // Replace hello following with hello DNS suffix for your virtual network
        zone "icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net" {
            type forward;
            forwarders {168.63.129.16;}; # hello Azure recursive resolver
        };

    > [!IMPORTANT]
    > <span data-ttu-id="f36b5-188">Du måste ersätta hello `icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net` med hello DNS-suffix som du hämtade tidigare.</span><span class="sxs-lookup"><span data-stu-id="f36b5-188">You must replace hello `icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net` with hello DNS suffix you retrieved earlier.</span></span>

    <span data-ttu-id="f36b5-189">tooedit den här filen, Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="f36b5-189">tooedit this file, use hello following command:</span></span>

    ```bash
    sudo nano /etc/bind/named.conf.local
    ```

    <span data-ttu-id="f36b5-190">toosave hello-fil, Använd __Ctrl + X__, __Y__, och sedan __RETUR__.</span><span class="sxs-lookup"><span data-stu-id="f36b5-190">toosave hello file, use __Ctrl+X__, __Y__, and then __Enter__.</span></span>

6. <span data-ttu-id="f36b5-191">toostart bindning, Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="f36b5-191">toostart Bind, use hello following command:</span></span>

    ```bash
    sudo service bind9 restart
    ```

7. <span data-ttu-id="f36b5-192">tooverify binda kan lösa hello namnen på de resurser i ditt lokala nätverk, Använd hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="f36b5-192">tooverify that bind can resolve hello names of resources in your on-premises network, use hello following commands:</span></span>

    ```bash
    sudo apt install dnsutils
    nslookup dns.mynetwork.net 10.0.0.4
    ```

    > [!IMPORTANT]
    > <span data-ttu-id="f36b5-193">Ersätt `dns.mynetwork.net` med hello fullständigt kvalificerade domännamnet (FQDN) för en resurs i ditt lokala nätverk.</span><span class="sxs-lookup"><span data-stu-id="f36b5-193">Replace `dns.mynetwork.net` with hello fully qualified domain name (FQDN) of a resource in your on-premises network.</span></span>
    >
    > <span data-ttu-id="f36b5-194">Ersätt `10.0.0.4` med hello __interna IP-adress__ på anpassade DNS-servern i hello virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="f36b5-194">Replace `10.0.0.4` with hello __internal IP address__ of your custom DNS server in hello virtual network.</span></span>

    <span data-ttu-id="f36b5-195">hello svar visas liknande toohello följande text:</span><span class="sxs-lookup"><span data-stu-id="f36b5-195">hello response appears similar toohello following text:</span></span>

        Server:         10.0.0.4
        Address:        10.0.0.4#53

        Non-authoritative answer:
        Name:   dns.mynetwork.net
        Address: 192.168.0.4

### <a name="configure-hello-virtual-network-toouse-hello-custom-dns-server"></a><span data-ttu-id="f36b5-196">Konfigurera hello virtuellt nätverk toouse hello anpassade DNS-server</span><span class="sxs-lookup"><span data-stu-id="f36b5-196">Configure hello virtual network toouse hello custom DNS server</span></span>

<span data-ttu-id="f36b5-197">tooconfigure hello virtuellt nätverk toouse hello anpassade DNS-servern i stället för hello Azure rekursiv matcharen använda hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="f36b5-197">tooconfigure hello virtual network toouse hello custom DNS server instead of hello Azure recursive resolver, use hello following steps:</span></span>

1. <span data-ttu-id="f36b5-198">I hello [Azure-portalen](https://portal.azure.com), Välj hello virtuella nätverk och väljer sedan __DNS-servrar__.</span><span class="sxs-lookup"><span data-stu-id="f36b5-198">In hello [Azure portal](https://portal.azure.com), select hello virtual network, and then select __DNS Servers__.</span></span>

2. <span data-ttu-id="f36b5-199">Välj __anpassade__, och ange hello __interna IP-adress__ för hello anpassade DNS-server.</span><span class="sxs-lookup"><span data-stu-id="f36b5-199">Select __Custom__, and enter hello __internal IP address__ of hello custom DNS server.</span></span> <span data-ttu-id="f36b5-200">Välj slutligen __spara__.</span><span class="sxs-lookup"><span data-stu-id="f36b5-200">Finally, select __Save__.</span></span>

    ![Ange hello anpassade DNS-server för hello nätverket](./media/connect-on-premises-network/configure-custom-dns.png)

### <a name="configure-hello-on-premises-dns-server"></a><span data-ttu-id="f36b5-202">Konfigurera hello lokala DNS-server</span><span class="sxs-lookup"><span data-stu-id="f36b5-202">Configure hello on-premises DNS server</span></span>

<span data-ttu-id="f36b5-203">I föregående avsnitt hello konfigurerat du hello anpassade DNS-server tooforward begäranden toohello lokala DNS-server.</span><span class="sxs-lookup"><span data-stu-id="f36b5-203">In hello previous section, you configured hello custom DNS server tooforward requests toohello on-premises DNS server.</span></span> <span data-ttu-id="f36b5-204">Därefter måste du konfigurera hello lokala DNS-server tooforward begäranden toohello anpassade DNS-server.</span><span class="sxs-lookup"><span data-stu-id="f36b5-204">Next, you must configure hello on-premises DNS server tooforward requests toohello custom DNS server.</span></span>

<span data-ttu-id="f36b5-205">Specifika anvisningar för hur tooconfigure DNS-servern, hello finns i dokumentationen för din DNS-server-program.</span><span class="sxs-lookup"><span data-stu-id="f36b5-205">For specific steps on how tooconfigure your DNS server, consult hello documentation for your DNS server software.</span></span> <span data-ttu-id="f36b5-206">Leta efter hello instruktioner för hur tooconfigure en __villkorlig vidarebefordrare__.</span><span class="sxs-lookup"><span data-stu-id="f36b5-206">Look for hello steps on how tooconfigure a __conditional forwarder__.</span></span>

<span data-ttu-id="f36b5-207">Villkorlig vidarebefordran vidarebefordrar bara begäranden för en specifik DNS-suffix.</span><span class="sxs-lookup"><span data-stu-id="f36b5-207">A conditional forward only forwards requests for a specific DNS suffix.</span></span> <span data-ttu-id="f36b5-208">I det här fallet måste du konfigurera en vidarebefordrare för hello DNS-suffix hello virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="f36b5-208">In this case, you must configure a forwarder for hello DNS suffix of hello virtual network.</span></span> <span data-ttu-id="f36b5-209">Begäranden för det här suffixet ska vidarebefordras toohello hello anpassade DNS-serverns IP-adress.</span><span class="sxs-lookup"><span data-stu-id="f36b5-209">Requests for this suffix should be forwarded toohello IP address of hello custom DNS server.</span></span> 

<span data-ttu-id="f36b5-210">hello följande är ett exempel på en villkorlig vidarebefordrare konfiguration för hello **binda** DNS-programvara:</span><span class="sxs-lookup"><span data-stu-id="f36b5-210">hello following text is an example of a conditional forwarder configuration for hello **Bind** DNS software:</span></span>

    zone "icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net" {
        type forward;
        forwarders {10.0.0.4;}; # hello custom DNS server's internal IP address
    };

<span data-ttu-id="f36b5-211">Information om hur du använder DNS på **Windows Server 2016**, se hello [Lägg till DnsServerConditionalForwarderZone](https://technet.microsoft.com/itpro/powershell/windows/dnsserver/add-dnsserverconditionalforwarderzone) dokumentationen...</span><span class="sxs-lookup"><span data-stu-id="f36b5-211">For information on using DNS on **Windows Server 2016**, see hello [Add-DnsServerConditionalForwarderZone](https://technet.microsoft.com/itpro/powershell/windows/dnsserver/add-dnsserverconditionalforwarderzone) documentation...</span></span>

<span data-ttu-id="f36b5-212">När du har konfigurerat hello lokala DNS-server, kan du använda `nslookup` från hello lokalt nätverk tooverify att du kan matcha namnen i hello virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="f36b5-212">Once you have configured hello on-premises DNS server, you can use `nslookup` from hello on-premises network tooverify that you can resolve names in hello virtual network.</span></span> <span data-ttu-id="f36b5-213">följande exempel hello</span><span class="sxs-lookup"><span data-stu-id="f36b5-213">hello following example</span></span> 

```bash
nslookup dnsproxy.icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net 196.168.0.4
```

<span data-ttu-id="f36b5-214">Det här exemplet använder hello lokala DNS-servern vid 196.168.0.4 tooresolve hello namnet på hello anpassad DNS-server.</span><span class="sxs-lookup"><span data-stu-id="f36b5-214">This example uses hello on-premises DNS server at 196.168.0.4 tooresolve hello name of hello custom DNS server.</span></span> <span data-ttu-id="f36b5-215">Ersätt hello IP-adress med hello en för hello lokala DNS-server.</span><span class="sxs-lookup"><span data-stu-id="f36b5-215">Replace hello IP address with hello one for hello on-premises DNS server.</span></span> <span data-ttu-id="f36b5-216">Ersätt hello `dnsproxy` adress med hello fullständigt kvalificerade domännamnet för hello anpassad DNS-server.</span><span class="sxs-lookup"><span data-stu-id="f36b5-216">Replace hello `dnsproxy` address with hello fully qualified domain name of hello custom DNS server.</span></span>

## <a name="optional-control-network-traffic"></a><span data-ttu-id="f36b5-217">Valfritt: Kontrollen nätverkstrafik</span><span class="sxs-lookup"><span data-stu-id="f36b5-217">Optional: Control network traffic</span></span>

<span data-ttu-id="f36b5-218">Du kan använda nätverkssäkerhetsgrupper (NSG) eller användardefinierade vägar (UDR) toocontrol nätverkstrafik.</span><span class="sxs-lookup"><span data-stu-id="f36b5-218">You can use network security groups (NSG) or user-defined routes (UDR) toocontrol network traffic.</span></span> <span data-ttu-id="f36b5-219">NSG: er kan du toofilter inkommande och utgående trafik, och tillåta eller neka hello trafik.</span><span class="sxs-lookup"><span data-stu-id="f36b5-219">NSGs allow you toofilter inbound and outbound traffic, and allow or deny hello traffic.</span></span> <span data-ttu-id="f36b5-220">Udr: er kan du toocontrol hur trafiken flödar mellan resurser i hello virtuellt nätverk, hello internet och hello lokalt nätverk.</span><span class="sxs-lookup"><span data-stu-id="f36b5-220">UDRs allow you toocontrol how traffic flows between resources in hello virtual network, hello internet, and hello on-premises network.</span></span>

> [!WARNING]
> <span data-ttu-id="f36b5-221">HDInsight kräver inkommande åtkomst från särskilda IP-adresser i hello Azure-molnet och obegränsad utgående åtkomst.</span><span class="sxs-lookup"><span data-stu-id="f36b5-221">HDInsight requires inbound access from specific IP addresses in hello Azure cloud, and unrestricted outbound access.</span></span> <span data-ttu-id="f36b5-222">När du använder NSG: er eller udr: er toocontrol trafik, måste du utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="f36b5-222">When using NSGs or UDRs toocontrol traffic, you must perform hello following steps:</span></span>
>
> 1. <span data-ttu-id="f36b5-223">Hitta hello IP-adresser för hello-plats som innehåller det virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="f36b5-223">Find hello IP addresses for hello location that contains your virtual network.</span></span> <span data-ttu-id="f36b5-224">En lista över nödvändiga IP-adresser per plats, se [krävs IP-adresser](./hdinsight-extend-hadoop-virtual-network.md#hdinsight-ip).</span><span class="sxs-lookup"><span data-stu-id="f36b5-224">For a list of required IPs by location, see [Required IP addresses](./hdinsight-extend-hadoop-virtual-network.md#hdinsight-ip).</span></span>
>
> 2. <span data-ttu-id="f36b5-225">Tillåta inkommande trafik från hello IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="f36b5-225">Allow inbound traffic from hello IP addresses.</span></span>
>
>    * <span data-ttu-id="f36b5-226">__NSG__: Tillåt __inkommande__ trafik på port __443__ från hello __Internet__.</span><span class="sxs-lookup"><span data-stu-id="f36b5-226">__NSG__: Allow __inbound__ traffic on port __443__ from hello __Internet__.</span></span>
>    * <span data-ttu-id="f36b5-227">__UDR__: Ange hello __nästa hopp__ typ av hello flödet too__Internet__.</span><span class="sxs-lookup"><span data-stu-id="f36b5-227">__UDR__: Set hello __Next Hop__ type of hello route too__Internet__.</span></span>

<span data-ttu-id="f36b5-228">Ett exempel på hur Azure PowerShell eller hello Azure CLI toocreate NSG: er finns i hello [utöka HDInsight med Azure Virtual Networks](./hdinsight-extend-hadoop-virtual-network.md#hdinsight-nsg) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="f36b5-228">For an example of using Azure PowerShell or hello Azure CLI toocreate NSGs, see hello [Extend HDInsight with Azure Virtual Networks](./hdinsight-extend-hadoop-virtual-network.md#hdinsight-nsg) document.</span></span>

## <a name="create-hello-hdinsight-cluster"></a><span data-ttu-id="f36b5-229">Skapa hello HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="f36b5-229">Create hello HDInsight cluster</span></span>

> [!WARNING]
> <span data-ttu-id="f36b5-230">Innan du installerar HDInsight i hello virtuella nätverk måste du konfigurera hello anpassad DNS-server.</span><span class="sxs-lookup"><span data-stu-id="f36b5-230">You must configure hello custom DNS server before installing HDInsight in hello virtual network.</span></span>

<span data-ttu-id="f36b5-231">Använd hello stegen i hello [skapar ett HDInsight-kluster med hjälp av hello Azure-portalen](./hdinsight-hadoop-create-linux-clusters-portal.md) dokument toocreate ett HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="f36b5-231">Use hello steps in hello [Create an HDInsight cluster using hello Azure portal](./hdinsight-hadoop-create-linux-clusters-portal.md) document toocreate an HDInsight cluster.</span></span>

> [!WARNING]
> * <span data-ttu-id="f36b5-232">Du måste välja hello-plats som innehåller det virtuella nätverket när klustret skapas.</span><span class="sxs-lookup"><span data-stu-id="f36b5-232">During cluster creation, you must choose hello location that contains your virtual network.</span></span>
>
> * <span data-ttu-id="f36b5-233">I hello __avancerade inställningar__ en del av konfigurationen, måste du välja hello virtuella nätverk och undernät som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="f36b5-233">In hello __Advanced settings__ part of configuration, you must select hello virtual network and subnet that you created earlier.</span></span>

## <a name="connecting-toohdinsight"></a><span data-ttu-id="f36b5-234">Ansluta tooHDInsight</span><span class="sxs-lookup"><span data-stu-id="f36b5-234">Connecting tooHDInsight</span></span>

<span data-ttu-id="f36b5-235">De flesta dokumentation på HDInsight förutsätter att du har åtkomst toohello klustret via hello internet.</span><span class="sxs-lookup"><span data-stu-id="f36b5-235">Most documentation on HDInsight assumes that you have access toohello cluster over hello internet.</span></span> <span data-ttu-id="f36b5-236">Till exempel att du kan ansluta toohello klustret på https://CLUSTERNAME.azurehdinsight.net.</span><span class="sxs-lookup"><span data-stu-id="f36b5-236">For example, that you can connect toohello cluster at https://CLUSTERNAME.azurehdinsight.net.</span></span> <span data-ttu-id="f36b5-237">Den här adressen använder offentliga hello-gatewayen, som inte är tillgängligt om du har använt NSG: er eller udr: er toorestrict åtkomst från hello internet.</span><span class="sxs-lookup"><span data-stu-id="f36b5-237">This address uses hello public gateway, which is not available if you have used NSGs or UDRs toorestrict access from hello internet.</span></span>

<span data-ttu-id="f36b5-238">toodirectly ansluta tooHDInsight via hello virtuellt nätverk, använder hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="f36b5-238">toodirectly connect tooHDInsight through hello virtual network, use hello following steps:</span></span>

1. <span data-ttu-id="f36b5-239">toodiscover hello interna fullständigt kvalificerade domännamn för hello HDInsight-klusternoder med någon av följande metoder hello:</span><span class="sxs-lookup"><span data-stu-id="f36b5-239">toodiscover hello internal fully qualified domain names of hello HDInsight cluster nodes, use one of hello following methods:</span></span>

    ```powershell
    $resourceGroupName = "hello resource group that contains hello virtual network used with HDInsight"

    $clusterNICs = Get-AzureRmNetworkInterface -ResourceGroupName $resourceGroupName | where-object {$_.Name -like "*node*"}

    $nodes = @()
    foreach($nic in $clusterNICs) {
        $node = new-object System.Object
        $node | add-member -MemberType NoteProperty -name "Type" -value $nic.Name.Split('-')[1]
        $node | add-member -MemberType NoteProperty -name "InternalIP" -value $nic.IpConfigurations.PrivateIpAddress
        $node | add-member -MemberType NoteProperty -name "InternalFQDN" -value $nic.DnsSettings.InternalFqdn
        $nodes += $node
    }
    $nodes | sort-object Type
    ```

    ```azurecli
    az network nic list --resource-group <resourcegroupname> --output table --query "[?contains(name,'node')].{NICname:name,InternalIP:ipConfigurations[0].privateIpAddress,InternalFQDN:dnsSettings.internalFqdn}"
    ```

2. <span data-ttu-id="f36b5-240">toodetermine hello port som en tjänst är tillgänglig, finns hello [portar som används av Hadoop-tjänster på HDInsight](./hdinsight-hadoop-port-settings-for-services.md) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="f36b5-240">toodetermine hello port that a service is available on, see hello [Ports used by Hadoop services on HDInsight](./hdinsight-hadoop-port-settings-for-services.md) document.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="f36b5-241">Vissa tjänster som finns på hello huvudnoderna är bara aktiva på en nod i taget.</span><span class="sxs-lookup"><span data-stu-id="f36b5-241">Some services hosted on hello head nodes are only active on one node at a time.</span></span> <span data-ttu-id="f36b5-242">Om du försöker att komma åt en tjänst på en huvudnod och det misslyckas, växla toohello andra huvudnod.</span><span class="sxs-lookup"><span data-stu-id="f36b5-242">If you try accessing a service on one head node and it fails, switch toohello other head node.</span></span>
    >
    > <span data-ttu-id="f36b5-243">Till exempel är Ambari endast aktiv i en huvudnod i taget.</span><span class="sxs-lookup"><span data-stu-id="f36b5-243">For example, Ambari is only active on one head node at a time.</span></span> <span data-ttu-id="f36b5-244">Om du försöker komma åt Ambari på en huvudnod och returnerar ett 404-fel, och sedan den körs på hello andra huvudnod.</span><span class="sxs-lookup"><span data-stu-id="f36b5-244">If you try accessing Ambari on one head node and it returns a 404 error, then it is running on hello other head node.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f36b5-245">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f36b5-245">Next steps</span></span>

* <span data-ttu-id="f36b5-246">Mer information om hur du använder HDInsight i ett virtuellt nätverk finns [utöka HDInsight med hjälp av Azure Virtual Networks](./hdinsight-extend-hadoop-virtual-network.md).</span><span class="sxs-lookup"><span data-stu-id="f36b5-246">For more information on using HDInsight in a virtual network, see [Extend HDInsight by using Azure Virtual Networks](./hdinsight-extend-hadoop-virtual-network.md).</span></span>

* <span data-ttu-id="f36b5-247">Mer information om virtuella Azure-nätverk finns hello [översikt över Azure Virtual Network](../virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="f36b5-247">For more information on Azure virtual networks, see hello [Azure Virtual Network overview](../virtual-network/virtual-networks-overview.md).</span></span>

* <span data-ttu-id="f36b5-248">Mer information om nätverkssäkerhetsgrupper finns [Nätverkssäkerhetsgrupper](../virtual-network/virtual-networks-nsg.md).</span><span class="sxs-lookup"><span data-stu-id="f36b5-248">For more information on network security groups, see [Network security groups](../virtual-network/virtual-networks-nsg.md).</span></span>

* <span data-ttu-id="f36b5-249">Mer information om användardefinierade vägar finns [användardefinierade vägar och IP-vidarebefordring](../virtual-network/virtual-networks-udr-overview.md).</span><span class="sxs-lookup"><span data-stu-id="f36b5-249">For more information on user-defined routes, see [USer-defined routes and IP forwarding](../virtual-network/virtual-networks-udr-overview.md).</span></span>
