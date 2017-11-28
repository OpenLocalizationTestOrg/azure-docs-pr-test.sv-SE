---
title: "aaaUse fjärrskrivbord tooa Linux VM i Azure | Microsoft Docs"
description: "Lär dig hur tooinstall och konfigurera fjärrskrivbord (xrdp) tooconnect tooa Linux VM i Azure med hjälp av grafiska verktyg"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: iainfou
ms.openlocfilehash: 64d30be101ceeb49fc05bb10293ad63db358efe3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="install-and-configure-remote-desktop-tooconnect-tooa-linux-vm-in-azure"></a><span data-ttu-id="d331f-103">Installera och konfigurera fjärrskrivbord tooconnect tooa Linux VM i Azure</span><span class="sxs-lookup"><span data-stu-id="d331f-103">Install and configure Remote Desktop tooconnect tooa Linux VM in Azure</span></span>
<span data-ttu-id="d331f-104">Linux-datorer (VM) i Azure hanteras vanligtvis från hello kommandoraden med hjälp av en secure shell (SSH)-anslutning.</span><span class="sxs-lookup"><span data-stu-id="d331f-104">Linux virtual machines (VMs) in Azure are usually managed from hello command line using a secure shell (SSH) connection.</span></span> <span data-ttu-id="d331f-105">När nya tooLinux eller i scenarier med snabb felsökning hello användning av fjärrskrivbord kan vara enklare.</span><span class="sxs-lookup"><span data-stu-id="d331f-105">When new tooLinux, or for quick troubleshooting scenarios, hello use of remote desktop may be easier.</span></span> <span data-ttu-id="d331f-106">Den här artikeln information om hur tooinstall och konfigurera en Skrivbordsmiljö ([xfce](https://www.xfce.org)) och fjärrskrivbord ([xrdp](http://www.xrdp.org)) för dina Linux VM med hjälp av hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="d331f-106">This article details how tooinstall and configure a desktop environment ([xfce](https://www.xfce.org)) and remote desktop ([xrdp](http://www.xrdp.org)) for your Linux VM using hello Resource Manager deployment model.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="d331f-107">Krav</span><span class="sxs-lookup"><span data-stu-id="d331f-107">Prerequisites</span></span>
<span data-ttu-id="d331f-108">Den här artikeln kräver ett befintligt Linux VM i Azure.</span><span class="sxs-lookup"><span data-stu-id="d331f-108">This article requires an existing Linux VM in Azure.</span></span> <span data-ttu-id="d331f-109">Om du behöver toocreate en virtuell dator kan använda en av hello följande metoder:</span><span class="sxs-lookup"><span data-stu-id="d331f-109">If you need toocreate a VM, use one of hello following methods:</span></span>

- <span data-ttu-id="d331f-110">Hej [Azure CLI 2.0](quick-create-cli.md)</span><span class="sxs-lookup"><span data-stu-id="d331f-110">hello [Azure CLI 2.0](quick-create-cli.md)</span></span>
- <span data-ttu-id="d331f-111">Hej [Azure-portalen](quick-create-portal.md)</span><span class="sxs-lookup"><span data-stu-id="d331f-111">hello [Azure portal](quick-create-portal.md)</span></span>


## <a name="install-a-desktop-environment-on-your-linux-vm"></a><span data-ttu-id="d331f-112">Installera en Skrivbordsmiljö på Linux-VM</span><span class="sxs-lookup"><span data-stu-id="d331f-112">Install a desktop environment on your Linux VM</span></span>
<span data-ttu-id="d331f-113">De flesta virtuella Linux-datorer i Azure har inte en Skrivbordsmiljö installerad som standard.</span><span class="sxs-lookup"><span data-stu-id="d331f-113">Most Linux VMs in Azure do not have a desktop environment installed by default.</span></span> <span data-ttu-id="d331f-114">Virtuella Linux-datorer hanteras ofta med hjälp av SSH-anslutningar i stället för en Skrivbordsmiljö.</span><span class="sxs-lookup"><span data-stu-id="d331f-114">Linux VMs are commonly managed using SSH connections rather than a desktop environment.</span></span> <span data-ttu-id="d331f-115">Det finns olika skrivbordsmiljöer i Linux som du kan välja.</span><span class="sxs-lookup"><span data-stu-id="d331f-115">There are various desktop environments in Linux that you can choose.</span></span> <span data-ttu-id="d331f-116">Beroende på ditt val av Skrivbordsmiljö kan den använda en too2 GB diskutrymme, och ta 5 too10 minuter tooinstall och konfigurera alla hello krävs paket.</span><span class="sxs-lookup"><span data-stu-id="d331f-116">Depending on your choice of desktop environment, it may consume one too2 GB of disk space, and take 5 too10 minutes tooinstall and configure all hello required packages.</span></span>

<span data-ttu-id="d331f-117">hello följande exempel installeras hello lightweight [xfce4](https://www.xfce.org/) Skrivbordsmiljö på en Ubuntu VM.</span><span class="sxs-lookup"><span data-stu-id="d331f-117">hello following example installs hello lightweight [xfce4](https://www.xfce.org/) desktop environment on an Ubuntu VM.</span></span> <span data-ttu-id="d331f-118">Kommandon för andra distributioner variera (Använd `yum` tooinstall på Red Hat Enterprise Linux och konfigurera lämplig `selinux` regler eller Använd `zypper` tooinstall på SUSE, till exempel).</span><span class="sxs-lookup"><span data-stu-id="d331f-118">Commands for other distributions vary slightly (use `yum` tooinstall on Red Hat Enterprise Linux and configure appropriate `selinux` rules, or use `zypper` tooinstall on SUSE, for example).</span></span>

<span data-ttu-id="d331f-119">Första, SSH tooyour VM.</span><span class="sxs-lookup"><span data-stu-id="d331f-119">First, SSH tooyour VM.</span></span> <span data-ttu-id="d331f-120">hello följande exempel ansluter toohello virtuella datorn med namnet *myvm.westus.cloudapp.azure.com* med hello användarnamnet för *azureuser*:</span><span class="sxs-lookup"><span data-stu-id="d331f-120">hello following example connects toohello VM named *myvm.westus.cloudapp.azure.com* with hello username of *azureuser*:</span></span>

```bash
ssh azureuser@myvm.westus.cloudapp.azure.com
```

<span data-ttu-id="d331f-121">Om du använder Windows, och du behöver mer information om hur du använder SSH, se [hur toouse SSH-nycklar med Windows](ssh-from-windows.md).</span><span class="sxs-lookup"><span data-stu-id="d331f-121">If you are using Windows and need more information on using SSH, see [How toouse SSH keys with Windows](ssh-from-windows.md).</span></span>

<span data-ttu-id="d331f-122">Därefter installera xfce med `apt` på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="d331f-122">Next, install xfce using `apt` as follows:</span></span>

```bash
sudo apt-get update
sudo apt-get install xfce4
```

## <a name="install-and-configure-a-remote-desktop-server"></a><span data-ttu-id="d331f-123">Installera och konfigurera en stationär dator</span><span class="sxs-lookup"><span data-stu-id="d331f-123">Install and configure a remote desktop server</span></span>
<span data-ttu-id="d331f-124">Nu när du har en Skrivbordsmiljö installerad kan du konfigurera en Fjärrskrivbordstjänster toolisten för inkommande anslutningar.</span><span class="sxs-lookup"><span data-stu-id="d331f-124">Now that you have a desktop environment installed, configure a remote desktop service toolisten for incoming connections.</span></span> <span data-ttu-id="d331f-125">[xrdp](http://xrdp.org) är en öppen källkod Remote Desktop Protocol (RDP)-server som är tillgänglig på de flesta Linux-distributioner och fungerar väl med xfce.</span><span class="sxs-lookup"><span data-stu-id="d331f-125">[xrdp](http://xrdp.org) is an open source Remote Desktop Protocol (RDP) server that is available on most Linux distributions, and works well with xfce.</span></span> <span data-ttu-id="d331f-126">Installera xrdp på din Ubuntu VM på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="d331f-126">Install xrdp on your Ubuntu VM as follows:</span></span>

```bash
sudo apt-get install xrdp
```

<span data-ttu-id="d331f-127">Meddela xrdp vilka Skrivbordsmiljö toouse när du startar sessionen.</span><span class="sxs-lookup"><span data-stu-id="d331f-127">Tell xrdp what desktop environment toouse when you start your session.</span></span> <span data-ttu-id="d331f-128">Konfigurera xrdp toouse xfce som skrivbordsmiljön på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="d331f-128">Configure xrdp toouse xfce as your desktop environment as follows:</span></span>

```bash
echo xfce4-session >~/.xsession
```

<span data-ttu-id="d331f-129">Starta om hello xrdp för hello ändringar tootake effekt på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="d331f-129">Restart hello xrdp service for hello changes tootake effect as follows:</span></span>

```bash
sudo service xrdp restart
```


## <a name="set-a-local-user-account-password"></a><span data-ttu-id="d331f-130">Ange ett lösenord för lokala användare</span><span class="sxs-lookup"><span data-stu-id="d331f-130">Set a local user account password</span></span>
<span data-ttu-id="d331f-131">Hoppa över det här steget om du har skapat ett lösenord för användarkontot när du skapade den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="d331f-131">If you created a password for your user account when you created your VM, skip this step.</span></span> <span data-ttu-id="d331f-132">Om du bara använder SSH-nyckelautentisering och har inte ett lokalt kontolösenord ange, ange ett lösenord innan du använder xrdp toolog i tooyour VM.</span><span class="sxs-lookup"><span data-stu-id="d331f-132">If you only use SSH key authentication and do not have a local account password set, specify a password before you use xrdp toolog in tooyour VM.</span></span> <span data-ttu-id="d331f-133">xrdp kan inte acceptera SSH-nycklar för autentisering.</span><span class="sxs-lookup"><span data-stu-id="d331f-133">xrdp cannot accept SSH keys for authentication.</span></span> <span data-ttu-id="d331f-134">hello följande exempel anger ett lösenord för användarkontot för hello *azureuser*:</span><span class="sxs-lookup"><span data-stu-id="d331f-134">hello following example specifies a password for hello user account *azureuser*:</span></span>

```bash
sudo passwd azureuser
```

> [!NOTE]
> <span data-ttu-id="d331f-135">Ange ett lösenord uppdateras inte SSHD för konfigurationen toopermit Lösenordsinloggning om den för närvarande inte.</span><span class="sxs-lookup"><span data-stu-id="d331f-135">Specifying a password does not update your SSHD configuration toopermit password logins if it currently does not.</span></span> <span data-ttu-id="d331f-136">Du kanske vill tooconnect tooyour VM med en SSH-tunnel med nyckel-baserad autentisering och anslut sedan tooxrdp från ett säkerhetsperspektiv.</span><span class="sxs-lookup"><span data-stu-id="d331f-136">From a security perspective, you may wish tooconnect tooyour VM with an SSH tunnel using key-based authentication and then connect tooxrdp.</span></span> <span data-ttu-id="d331f-137">I så fall, hoppar du över hello följande steg om hur du skapar en säkerhet grupp regeln tooallow remote desktop nätverkstrafik.</span><span class="sxs-lookup"><span data-stu-id="d331f-137">If so, skip hello following step on creating a network security group rule tooallow remote desktop traffic.</span></span>


## <a name="create-a-network-security-group-rule-for-remote-desktop-traffic"></a><span data-ttu-id="d331f-138">Skapa en säkerhetsgrupp för nätverk-regel för Remote Desktop-trafik</span><span class="sxs-lookup"><span data-stu-id="d331f-138">Create a Network Security Group rule for Remote Desktop traffic</span></span>
<span data-ttu-id="d331f-139">tooallow fjärrskrivbord trafik tooreach din Linux VM, en grupp för nätverkssäkerhetsregeln måste toobe skapas som tillåter TCP på port 3389 tooreach den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="d331f-139">tooallow Remote Desktop traffic tooreach your Linux VM, a network security group rule needs toobe created that allows TCP on port 3389 tooreach your VM.</span></span> <span data-ttu-id="d331f-140">Mer information om regler för nätverkssäkerhetsgrupper finns [vad är en Nätverkssäkerhetsgrupp?](../../virtual-network/virtual-networks-nsg.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="d331f-140">For more information about network security group rules, see [What is a Network Security Group?](../../virtual-network/virtual-networks-nsg.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span></span> <span data-ttu-id="d331f-141">Du kan också [Använd hello Azure portal toocreate en grupp för nätverkssäkerhetsregeln](../windows/nsg-quickstart-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="d331f-141">You can also [use hello Azure portal toocreate a network security group rule](../windows/nsg-quickstart-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="d331f-142">hello följande exempel skapar en grupp nätverkssäkerhetsregeln med [az nätverket nsg regeln skapa](/cli/azure/network/nsg/rule#create) med namnet *myNetworkSecurityGroupRule* för*Tillåt* trafik på *tcp* port *3389*.</span><span class="sxs-lookup"><span data-stu-id="d331f-142">hello following examples create a network security group rule with [az network nsg rule create](/cli/azure/network/nsg/rule#create) named *myNetworkSecurityGroupRule* too*allow* traffic on *tcp* port *3389*.</span></span>

```azurecli
az network nsg rule create \
    --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup \
    --name myNetworkSecurityGroupRule \
    --protocol tcp \
    --priority 1010 \
    --destination-port-range 3389
```


## <a name="connect-your-linux-vm-with-a-remote-desktop-client"></a><span data-ttu-id="d331f-143">Anslut din Linux VM med en fjärrskrivbordsklient</span><span class="sxs-lookup"><span data-stu-id="d331f-143">Connect your Linux VM with a Remote Desktop client</span></span>
<span data-ttu-id="d331f-144">Öppna din lokala fjärrskrivbordsklienten och Anslut toohello IP-adress eller DNS-namn för Linux-VM.</span><span class="sxs-lookup"><span data-stu-id="d331f-144">Open your local remote desktop client and connect toohello IP address or DNS name of your Linux VM.</span></span> <span data-ttu-id="d331f-145">Ange hello användarnamn och lösenord för hello användarkonto på den virtuella datorn på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="d331f-145">Enter hello username and password for hello user account on your VM as follows:</span></span>

![Ansluta tooxrdp Remote Desktop-klienten](./media/use-remote-desktop/remote-desktop-client.png)

<span data-ttu-id="d331f-147">Efter autentisering, hello xfce Skrivbordsmiljö läsa in och titta liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="d331f-147">After authenticating, hello xfce desktop environment will load and look similar toohello following example:</span></span>

![xfce Skrivbordsmiljö via xrdp](./media/use-remote-desktop/xfce-desktop-environment.png)


## <a name="troubleshoot"></a><span data-ttu-id="d331f-149">Felsöka</span><span class="sxs-lookup"><span data-stu-id="d331f-149">Troubleshoot</span></span>
<span data-ttu-id="d331f-150">Om du inte kan ansluta tooyour Linux VM med hjälp av en fjärrskrivbordsklient `netstat` på din Linux VM tooverify som den virtuella datorn lyssnar för RDP-anslutningar på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="d331f-150">If you cannot connect tooyour Linux VM using a Remote Desktop client, use `netstat` on your Linux VM tooverify that your VM is listening for RDP connections  as follows:</span></span>

```bash
sudo netstat -plnt | grep rdp
```

<span data-ttu-id="d331f-151">följande exempel visar hello hello VM som lyssnar på TCP-port 3389 som förväntat:</span><span class="sxs-lookup"><span data-stu-id="d331f-151">hello following example shows hello VM listening on TCP port 3389 as expected:</span></span>

```bash
tcp     0     0      127.0.0.1:3350     0.0.0.0:*     LISTEN     53192/xrdp-sesman
tcp     0     0      0.0.0.0:3389       0.0.0.0:*     LISTEN     53188/xrdp
```

<span data-ttu-id="d331f-152">Om hello xrdp tjänsten inte lyssnar på en Ubuntu VM startar du om hello på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="d331f-152">If hello xrdp service is not listening, on an Ubuntu VM restart hello service as follows:</span></span>

```bash
sudo service xrdp restart
```

<span data-ttu-id="d331f-153">Granska loggar in */var/log*Thug på din Ubuntu VM för uppgifter som toowhy hello-tjänsten svarar inte.</span><span class="sxs-lookup"><span data-stu-id="d331f-153">Review logs in */var/log*Thug  on your Ubuntu VM for indications as toowhy hello service may not be responding.</span></span> <span data-ttu-id="d331f-154">Du kan också övervaka hello syslog under en anslutning till fjärrskrivbord försök tooview eventuella fel:</span><span class="sxs-lookup"><span data-stu-id="d331f-154">You can also monitor hello syslog during a remote desktop connection attempt tooview any errors:</span></span>

```bash
tail -f /var/log/syslog
```

<span data-ttu-id="d331f-155">Andra Linux-distributioner, till exempel Red Hat Enterprise Linux och SUSE kan ha olika sätt toorestart tjänster och alternativa log file platser tooreview.</span><span class="sxs-lookup"><span data-stu-id="d331f-155">Other Linux distributions such as Red Hat Enterprise Linux and SUSE may have different ways toorestart services and alternate log file locations tooreview.</span></span>

<span data-ttu-id="d331f-156">Om du inte får något svar i din fjärrskrivbordsklienten och inte ser några händelser i hello systemloggen, anger detta att remote desktop-trafik inte går att nå hello VM.</span><span class="sxs-lookup"><span data-stu-id="d331f-156">If you do not receive any response in your remote desktop client and do not see any events in hello system log, this behavior indicates that remote desktop traffic cannot reach hello VM.</span></span> <span data-ttu-id="d331f-157">Granska dina network security grupp regler tooensure att du har en regel toopermit TCP port 3389.</span><span class="sxs-lookup"><span data-stu-id="d331f-157">Review your network security group rules tooensure that you have a rule toopermit TCP on port 3389.</span></span> <span data-ttu-id="d331f-158">Mer information finns i [felsökning av problem med nätverksanslutningen](../windows/troubleshoot-app-connection.md).</span><span class="sxs-lookup"><span data-stu-id="d331f-158">For more information, see [Troubleshoot application connectivity issues](../windows/troubleshoot-app-connection.md).</span></span>


## <a name="next-steps"></a><span data-ttu-id="d331f-159">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d331f-159">Next steps</span></span>
<span data-ttu-id="d331f-160">Mer information om hur du skapar och använder SSH-nycklar med Linux virtuella datorer finns [skapa SSH-nycklar för Linux virtuella datorer i Azure](mac-create-ssh-keys.md).</span><span class="sxs-lookup"><span data-stu-id="d331f-160">For more information about creating and using SSH keys with Linux VMs, see [Create SSH keys for Linux VMs in Azure](mac-create-ssh-keys.md).</span></span>

<span data-ttu-id="d331f-161">Information om hur du använder SSH från Windows finns [hur toouse SSH-nycklar med Windows](ssh-from-windows.md).</span><span class="sxs-lookup"><span data-stu-id="d331f-161">For information on using SSH from Windows, see [How toouse SSH keys with Windows](ssh-from-windows.md).</span></span>

