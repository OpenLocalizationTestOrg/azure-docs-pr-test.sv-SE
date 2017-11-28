---
title: "aaaConfigure SSHD på Azure Linux Virtual Machines | Microsoft Docs"
description: "Konfigurera SSHD för säkerhetsmetoder och toolockdown SSH tooAzure virtuella Linux-datorer."
services: virtual-machines-linux
documentationcenter: 
author: vlivech
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 11/21/2016
ms.author: v-livech
ms.openlocfilehash: c2361be7199a24b129c06acfc899dd32f6e1d6fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-sshd-on-azure-linux-vms"></a><span data-ttu-id="2ad5d-103">Konfigurera SSHD på virtuella Azure Linux-datorer</span><span class="sxs-lookup"><span data-stu-id="2ad5d-103">Configure SSHD on Azure Linux VMs</span></span>

<span data-ttu-id="2ad5d-104">Den här artikeln visar hur toolockdown hello SSH-Server på Linux, tooprovide bästa praxis säkerhet och även toospeed in hello SSH-inloggningen med hjälp av SSH-nycklar i stället för lösenord.</span><span class="sxs-lookup"><span data-stu-id="2ad5d-104">This article shows how toolockdown hello SSH Server on Linux, tooprovide best practices security and also toospeed up hello SSH login process by using SSH keys instead of passwords.</span></span>  <span data-ttu-id="2ad5d-105">begränsa hello användare som tillåts toologin via godkända grupplistan inaktiverar SSH-protokollversion 1 toofurther låsning SSHD som vi ska toodisable hello rotanvändaren från att kunna toologin, ställa in ett minsta viktiga biten och konfigurera automatiskt logga ut från inaktiva användare.</span><span class="sxs-lookup"><span data-stu-id="2ad5d-105">toofurther lockdown SSHD we are going toodisable hello root user from being able toologin, limit hello users that are allowed toologin via an approved group list, disabling SSH protocol version 1, set a minimum key bit, and configure auto-logout of idle users.</span></span>  <span data-ttu-id="2ad5d-106">hello kraven för den här artikeln är: ett Azure-konto ([skaffa en kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/)) och [SSH offentliga och privata nyckelfiler](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="2ad5d-106">hello requirements for this article are: an Azure account ([get a free trial](https://azure.microsoft.com/pricing/free-trial/)) and [SSH public and private key files](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <a name="quick-commands"></a><span data-ttu-id="2ad5d-107">Snabbkommandon</span><span class="sxs-lookup"><span data-stu-id="2ad5d-107">Quick Commands</span></span>

<span data-ttu-id="2ad5d-108">Konfigurera `/etc/ssh/sshd_config` med hello följande inställningar:</span><span class="sxs-lookup"><span data-stu-id="2ad5d-108">Configure `/etc/ssh/sshd_config` with hello following settings:</span></span>

### <a name="disable-password-logins"></a><span data-ttu-id="2ad5d-109">Inaktivera Lösenordsinloggning</span><span class="sxs-lookup"><span data-stu-id="2ad5d-109">Disable password logins</span></span>

```bash
PasswordAuthentication no
```

### <a name="disable-login-by-hello-root-user"></a><span data-ttu-id="2ad5d-110">Inaktivera inloggning av hello rotanvändaren</span><span class="sxs-lookup"><span data-stu-id="2ad5d-110">Disable login by hello root user</span></span>

```bash
PermitRootLogin no
```

### <a name="allowed-groups-list"></a><span data-ttu-id="2ad5d-111">Grupplistan över tillåtna</span><span class="sxs-lookup"><span data-stu-id="2ad5d-111">Allowed groups list</span></span>

```bash
AllowGroups wheel
```

### <a name="allowed-users-list"></a><span data-ttu-id="2ad5d-112">Användarlistan med tillåtna</span><span class="sxs-lookup"><span data-stu-id="2ad5d-112">Allowed users list</span></span>

```bash
AllowUsers ahmet ralph
```

### <a name="disable-ssh-protocol-version-1"></a><span data-ttu-id="2ad5d-113">Inaktivera SSH-protokollet version 1</span><span class="sxs-lookup"><span data-stu-id="2ad5d-113">Disable SSH protocol version 1</span></span>

```bash
Protocol 2
```

### <a name="minimum-key-bits"></a><span data-ttu-id="2ad5d-114">Minsta viktiga bits</span><span class="sxs-lookup"><span data-stu-id="2ad5d-114">Minimum key bits</span></span>

```bash
ServerKeyBits 2048
```

### <a name="disconnect-idle-users"></a><span data-ttu-id="2ad5d-115">Koppla från inaktiva användare</span><span class="sxs-lookup"><span data-stu-id="2ad5d-115">Disconnect idle users</span></span>

```bash
ClientAliveInterval 300
ClientAliveCountMax 0
```

## <a name="detailed-walkthrough"></a><span data-ttu-id="2ad5d-116">Detaljerad genomgång</span><span class="sxs-lookup"><span data-stu-id="2ad5d-116">Detailed Walkthrough</span></span>

<span data-ttu-id="2ad5d-117">SSHD är hello SSH-Server som körs på hello Linux VM.</span><span class="sxs-lookup"><span data-stu-id="2ad5d-117">SSHD is hello SSH Server that runs on hello Linux VM.</span></span>  <span data-ttu-id="2ad5d-118">SSH är en klient som körs från ett gränssnitt på din MacBook Linux arbetsstation eller från en Bash på Windows.</span><span class="sxs-lookup"><span data-stu-id="2ad5d-118">SSH is a client that runs from a shell on your MacBook, Linux workstation, or from a Bash on Windows.</span></span>  <span data-ttu-id="2ad5d-119">SSH är också hello-protokollet används toosecure och kryptera hello kommunikationen mellan arbetsstation och hello Linux VM skapa SSH även VPN (virtuellt privat nätverk).</span><span class="sxs-lookup"><span data-stu-id="2ad5d-119">SSH is also hello protocol used toosecure and encrypt hello communication between your workstation and hello Linux VM making SSH also a VPN (Virtual Private Network).</span></span>

<span data-ttu-id="2ad5d-120">Det är mycket viktigt tookeep en inloggning tooyour Linux VM öppna för hello hela hanteringspaketen för den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="2ad5d-120">For this article, it is very important tookeep one login tooyour Linux VM open for hello entire walk-through.</span></span>  <span data-ttu-id="2ad5d-121">När en SSH-anslutning har upprättats är den som en öppen session så länge hello fönster inte är stängd.</span><span class="sxs-lookup"><span data-stu-id="2ad5d-121">Once an SSH connection is established, it remains as an open session as long as hello window is not closed.</span></span>  <span data-ttu-id="2ad5d-122">Du har en terminal loggat in kan för ändringar gjorts toobe toohello SSHD tjänsten utan låsas om en ny ändring görs.</span><span class="sxs-lookup"><span data-stu-id="2ad5d-122">Having one terminal logged in, allows for changes toobe made toohello SSHD service without being locked out if a breaking change is made.</span></span>  <span data-ttu-id="2ad5d-123">Om du blir utelåst från ditt Linux VM till en skadad SSHD konfiguration, Azure erbjuder hello möjlighet tooreset en bruten SSHD konfiguration med hello [Access-tillägg för Azure VM](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="2ad5d-123">If you do get locked out of your Linux VM with a broken SSHD configuration, Azure offers hello ability tooreset a broken SSHD configuration with hello [Azure VM Access Extension](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="2ad5d-124">Därför öppna vi två terminaler och SSH toohello Linux VM från båda.</span><span class="sxs-lookup"><span data-stu-id="2ad5d-124">For this reason we open two terminals and SSH toohello Linux VM from both of them.</span></span>  <span data-ttu-id="2ad5d-125">Vi använder hello första terminal toomake hello ändringar tooSSHDs konfigurationsfilen och starta om hello SSHD tjänsten.</span><span class="sxs-lookup"><span data-stu-id="2ad5d-125">We use hello first terminal toomake hello changes tooSSHDs configuration file and restart hello SSHD service.</span></span>  <span data-ttu-id="2ad5d-126">Vi använder hello andra terminal tootest de ändras när hello-tjänsten startas.</span><span class="sxs-lookup"><span data-stu-id="2ad5d-126">We use hello second terminal tootest those changes once hello service is restarted.</span></span>  <span data-ttu-id="2ad5d-127">Eftersom vi inaktiverar SSH-lösenord och beroende strikt SSH-nycklar om SSH-nycklar inte är rätt och stänger hello anslutning toohello VM, hello VM är permanent låst och ingen kommer att kunna toologin tooit kräver det toobe bort och återskapas.</span><span class="sxs-lookup"><span data-stu-id="2ad5d-127">Because we are disabling SSH passwords and relying strictly on SSH keys, if your SSH keys are not correct and you close hello connection toohello VM, hello VM will be permanently locked and no one will be able toologin tooit requiring it toobe deleted and recreated.</span></span>

## <a name="disable-password-logins"></a><span data-ttu-id="2ad5d-128">Inaktivera Lösenordsinloggning</span><span class="sxs-lookup"><span data-stu-id="2ad5d-128">Disable password logins</span></span>

<span data-ttu-id="2ad5d-129">Hej snabbaste sättet toosecure du Linux VM är toodisable lösenord inloggningar.</span><span class="sxs-lookup"><span data-stu-id="2ad5d-129">hello quickest way toosecure you Linux VM is toodisable password logins.</span></span>  <span data-ttu-id="2ad5d-130">När Lösenordsinloggning är aktiverade, tvinga robotar crawlning hello web startar omedelbart försöker toobrute gissning hello lösenordet för ditt Linux-VM via SSH.</span><span class="sxs-lookup"><span data-stu-id="2ad5d-130">When password logins are enabled, bots crawling hello web will immediately start attempting toobrute force guess hello password for your Linux VM using SSH.</span></span>  <span data-ttu-id="2ad5d-131">Inaktivera Lösenordsinloggning helt, kan hello SSH server tooignore alla lösenord inloggningsförsök.</span><span class="sxs-lookup"><span data-stu-id="2ad5d-131">Disabling password logins completely, enables hello SSH server tooignore all password login attempts.</span></span>

```bash
PasswordAuthentication no
```

## <a name="disable-login-by-hello-root-user"></a><span data-ttu-id="2ad5d-132">Inaktivera inloggning av hello rotanvändaren</span><span class="sxs-lookup"><span data-stu-id="2ad5d-132">Disable login by hello root user</span></span>

<span data-ttu-id="2ad5d-133">Följande Linux bästa praxis, hello `root` ska aldrig vara inloggad på användare via SSH eller med hjälp av `sudo su`.</span><span class="sxs-lookup"><span data-stu-id="2ad5d-133">Following Linux best practices, hello `root` user should never be logged into over SSH or using `sudo su`.</span></span>  <span data-ttu-id="2ad5d-134">Alla kommandon som behöver behörigheter för roten på ska alltid köras via hello `sudo` kommando som loggar alla åtgärder för framtida granskning.</span><span class="sxs-lookup"><span data-stu-id="2ad5d-134">All commands needing root level permissions should always be run through hello `sudo` command, which logs all actions for future auditing.</span></span>  <span data-ttu-id="2ad5d-135">Inaktivera hello `root` användaren från att logga in via SSH är en bästa praxis säkerhetssteg som säkerställer att endast auktoriserade användare tillåts tooSSH.</span><span class="sxs-lookup"><span data-stu-id="2ad5d-135">Disabling hello `root` user from logging in via SSH is a security best practices step that ensures only authorized users are allowed tooSSH.</span></span>

```bash
PermitRootLogin no
```

## <a name="allowed-groups-list"></a><span data-ttu-id="2ad5d-136">Grupplistan över tillåtna</span><span class="sxs-lookup"><span data-stu-id="2ad5d-136">Allowed groups list</span></span>

<span data-ttu-id="2ad5d-137">SSH erbjuder en metod för att begränsa användare och grupper som tillåts eller otillåten från att logga in via SSH.</span><span class="sxs-lookup"><span data-stu-id="2ad5d-137">SSH offers a method of restricting users and group that are allowed or disallowed from logging in over SSH.</span></span>  <span data-ttu-id="2ad5d-138">Den här funktionen använder listor tooapprove eller neka specifika användare och grupper från att logga in.</span><span class="sxs-lookup"><span data-stu-id="2ad5d-138">This feature uses lists tooapprove or deny specific users and groups from logging in.</span></span>  <span data-ttu-id="2ad5d-139">Ange hello hjul grupp toohello `AllowGroups` lista begränsar godkända inloggningar över SSH toojust konton som är i hello hjul grupp.</span><span class="sxs-lookup"><span data-stu-id="2ad5d-139">Setting hello wheel group toohello `AllowGroups` list restricts approved logins over SSH toojust user accounts that are in hello wheel group.</span></span>

```bash
AllowGroups wheel
```

## <a name="allowed-users-list"></a><span data-ttu-id="2ad5d-140">Användarlistan med tillåtna</span><span class="sxs-lookup"><span data-stu-id="2ad5d-140">Allowed users list</span></span>

<span data-ttu-id="2ad5d-141">Att begränsa SSH-inloggningar toojust användare är ett mer specifikt sätt tooaccomplish hello samma uppgift som `AllowGroups` är.</span><span class="sxs-lookup"><span data-stu-id="2ad5d-141">Restricting SSH logins toojust users is a more specific way tooaccomplish hello same task that `AllowGroups` is.</span></span>  

```bash
AllowUsers ahmet ralph
```

## <a name="disable-ssh-protocol-version-1"></a><span data-ttu-id="2ad5d-142">Inaktivera SSH-protokollet version 1</span><span class="sxs-lookup"><span data-stu-id="2ad5d-142">Disable SSH protocol version 1</span></span>

<span data-ttu-id="2ad5d-143">SSH-protokollversion 1 är osäker och ska vara inaktiverat.</span><span class="sxs-lookup"><span data-stu-id="2ad5d-143">SSH protocol version 1 is insecure and should be disabled.</span></span>  <span data-ttu-id="2ad5d-144">SSH-protokollversion 2 är aktuella hello-versionen som erbjuder ett säkert sätt tooSSH tooyour server.</span><span class="sxs-lookup"><span data-stu-id="2ad5d-144">SSH protocol version 2 is hello current version that offers a secure way tooSSH tooyour server.</span></span>  <span data-ttu-id="2ad5d-145">Inaktivera SSH version 1 nekar SSH-klienter som försöker tooestablish en anslutning med hello SSH-server med SSH version 1.</span><span class="sxs-lookup"><span data-stu-id="2ad5d-145">Disabling SSH version 1 denies any SSH clients that are attempting tooestablish a connection with hello SSH server using SSH version 1.</span></span>  <span data-ttu-id="2ad5d-146">Endast SSH version 2-anslutningar tillåts toonegotiate en anslutning med hello SSH-server.</span><span class="sxs-lookup"><span data-stu-id="2ad5d-146">Only SSH version 2 connections are allowed toonegotiate a connection with hello SSH server.</span></span>

```bash
Protocol 2
```

## <a name="minimum-key-bits"></a><span data-ttu-id="2ad5d-147">Minsta viktiga bits</span><span class="sxs-lookup"><span data-stu-id="2ad5d-147">Minimum key bits</span></span>

<span data-ttu-id="2ad5d-148">Följande rekommenderade säkerhetsmetoder lösenord SSH-inloggning har inaktiverats och SSH-nycklar är tillåtna toobe används tooauthenticate med hello SSH-server.</span><span class="sxs-lookup"><span data-stu-id="2ad5d-148">Following security best practices, password SSH logins are disabled and only SSH keys are allowed toobe used tooauthenticate with hello SSH server.</span></span>  <span data-ttu-id="2ad5d-149">Dessa SSH-nycklar kan skapas med nyckellängderna är olika, mätt i bitar.</span><span class="sxs-lookup"><span data-stu-id="2ad5d-149">These SSH keys can be created using different length keys, measured in bits.</span></span>  <span data-ttu-id="2ad5d-150">Tillstånd att nycklar på 2 048 bitar långt är hello minsta godtagbara nyckellängd och metodtips.</span><span class="sxs-lookup"><span data-stu-id="2ad5d-150">Best practices states that keys of 2048 bits in length are hello minimum acceptable key strength.</span></span>  <span data-ttu-id="2ad5d-151">Nycklar på mindre än 2 048 bitar kunde teoretiskt brytas.</span><span class="sxs-lookup"><span data-stu-id="2ad5d-151">Keys of less than 2048 bits could theoretically be broken.</span></span>  <span data-ttu-id="2ad5d-152">Inställningen hello `ServerKeyBits` för`2048` tillåter alla anslutningar som använder nycklar på 2 048 bitar eller större och neka anslutningar på mindre än 2 048 bitar.</span><span class="sxs-lookup"><span data-stu-id="2ad5d-152">Setting hello `ServerKeyBits` too`2048` allows any connections using keys of 2048 bits or greater and deny connections of less than 2048 bits.</span></span>

```bash
ServerKeyBits 2048
```

## <a name="disconnect-idle-users"></a><span data-ttu-id="2ad5d-153">Koppla från inaktiva användare</span><span class="sxs-lookup"><span data-stu-id="2ad5d-153">Disconnect idle users</span></span>

<span data-ttu-id="2ad5d-154">SSH har hello möjlighet toodisconnect användare som har öppna anslutningar som har varit inaktiv i mer än en viss tid i sekunder.</span><span class="sxs-lookup"><span data-stu-id="2ad5d-154">SSH has hello ability toodisconnect users that have open connections that have remained idle for more than a set period of seconds.</span></span>  <span data-ttu-id="2ad5d-155">Hålls öppna sessioner tooonly användare som är aktiva gränser hello exponering av hello Linux VM.</span><span class="sxs-lookup"><span data-stu-id="2ad5d-155">Keeping open sessions tooonly those users who are active limits hello exposure of hello Linux VM.</span></span>

```bash
ClientAliveInterval 300
ClientAliveCountMax 0
```

## <a name="restart-sshd"></a><span data-ttu-id="2ad5d-156">Starta om SSHD</span><span class="sxs-lookup"><span data-stu-id="2ad5d-156">Restart SSHD</span></span>

<span data-ttu-id="2ad5d-157">tooenable hello inställningar i `/etc/ssh/sshd_config` starta om processen för hello SSHD som startar om hello SSH-server.</span><span class="sxs-lookup"><span data-stu-id="2ad5d-157">tooenable hello settings in `/etc/ssh/sshd_config` restart hello SSHD process which restarts hello SSH server.</span></span>  <span data-ttu-id="2ad5d-158">hello terminalfönster som du använder SSH-server för toorestart hello förbli öppen utan att förlora hello öppna SSH-session.</span><span class="sxs-lookup"><span data-stu-id="2ad5d-158">hello terminal window you use toorestart hello SSH server remain open without losing hello open SSH session.</span></span>  <span data-ttu-id="2ad5d-159">tootest hello nya SSH serverinställningarna använder en andra terminalfönster eller fliken.  Om du använder en separat terminal tootest hello SSH-anslutningen kan du toogo tillbaka och göra ytterligare ändringar toohello `/etc/ssh/sshd_config` i hello första terminal utan är låst av en ny SSHD ändring.</span><span class="sxs-lookup"><span data-stu-id="2ad5d-159">tootest hello new SSH server settings use a second terminal window or tab.  Using a separate terminal tootest hello SSH connection allows you toogo back and make additional changes toohello `/etc/ssh/sshd_config` in hello first terminal, without being locked out by a breaking SSHD change.</span></span>  

### <a name="on-redhat-centos-and-fedora"></a><span data-ttu-id="2ad5d-160">På Redhat, Centos och Fedora</span><span class="sxs-lookup"><span data-stu-id="2ad5d-160">On Redhat, Centos and Fedora</span></span>

```bash
service sshd restart
```

### <a name="on-debian--ubuntu"></a><span data-ttu-id="2ad5d-161">På Debian och Ubuntu</span><span class="sxs-lookup"><span data-stu-id="2ad5d-161">On Debian & Ubuntu</span></span>

```bash
service ssh restart
```

## <a name="reset-sshd-using-azure-reset-access"></a><span data-ttu-id="2ad5d-162">Återställ SSHD med hjälp av Azure Återställ-åtkomst</span><span class="sxs-lookup"><span data-stu-id="2ad5d-162">Reset SSHD using Azure reset-access</span></span>

<span data-ttu-id="2ad5d-163">Om du är utelåst från en ny ändring toohello SSHD konfiguration kan du använda hello Azure VM-tillägget för åtkomst tooreset hello SSHD tillbaka toohello ursprungliga konfiguration.</span><span class="sxs-lookup"><span data-stu-id="2ad5d-163">If you are locked out from a breaking change toohello SSHD configuration you can use hello Azure VM access-extension tooreset hello SSHD configuration back toohello original configuration.</span></span>

<span data-ttu-id="2ad5d-164">Ersätt alla exempelnamnen med din egen.</span><span class="sxs-lookup"><span data-stu-id="2ad5d-164">Replace any example names with your own.</span></span>

```azurecli
azure vm reset-access \
--resource-group myResourceGroup \
--name myVM \
--reset-ssh
```

## <a name="install-fail2ban"></a><span data-ttu-id="2ad5d-165">Installera Fail2ban</span><span class="sxs-lookup"><span data-stu-id="2ad5d-165">Install Fail2ban</span></span>

<span data-ttu-id="2ad5d-166">Det rekommenderas starkt tooinstall och installationsprogrammet hello öppen källkod app Fail2ban, vilka block upprepade försök toologin tooyour Linux VM via SSH med hjälp av brute force.</span><span class="sxs-lookup"><span data-stu-id="2ad5d-166">It is strongly recommended tooinstall and setup hello open source app Fail2ban, which blocks repeated attempts toologin tooyour Linux VM over SSH using brute force.</span></span>  <span data-ttu-id="2ad5d-167">Fail2ban loggar upprepas misslyckades försök toologin via SSH och skapar brandväggsregler tooblock hello IP-adress som hello försök kommer från.</span><span class="sxs-lookup"><span data-stu-id="2ad5d-167">Fail2ban logs repeated failed attempts toologin over SSH and then creates firewall rules tooblock hello IP address that hello attempts are originating from.</span></span>

* [<span data-ttu-id="2ad5d-168">Fail2ban webbsida</span><span class="sxs-lookup"><span data-stu-id="2ad5d-168">Fail2ban homepage</span></span>](http://www.fail2ban.org/wiki/index.php/Main_Page)

## <a name="next-steps"></a><span data-ttu-id="2ad5d-169">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2ad5d-169">Next Steps</span></span>

<span data-ttu-id="2ad5d-170">Nu när du har konfigurerat och låst hello SSH-server på Linux-VM finns ytterligare säkerhetsmetoder kan du följa.</span><span class="sxs-lookup"><span data-stu-id="2ad5d-170">Now that you have configured and locked down hello SSH server on your Linux VM there are additional security best practices you can follow.</span></span>  

* [<span data-ttu-id="2ad5d-171">Hantera användare, SSH och kontrollera eller reparera diskar på virtuella Azure Linux-datorer med hjälp av hello VMAccess-tillägget</span><span class="sxs-lookup"><span data-stu-id="2ad5d-171">Manage users, SSH, and check or repair disks on Azure Linux VMs using hello VMAccess Extension</span></span>](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

* [<span data-ttu-id="2ad5d-172">Kryptera diskar på en Linux VM som använder hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="2ad5d-172">Encrypt disks on a Linux VM using hello Azure CLI</span></span>](encrypt-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

* [<span data-ttu-id="2ad5d-173">Åtkomst och säkerhet i Azure Resource Manager-mallar</span><span class="sxs-lookup"><span data-stu-id="2ad5d-173">Access and security in Azure Resource Manager templates</span></span>](dotnet-core-3-access-security.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
