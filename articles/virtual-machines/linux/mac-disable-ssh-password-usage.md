---
title: "aaaDisable SSH-lösenord på din Linux VM genom att konfigurera SSHD | Microsoft Docs"
description: "Skydda din Linux VM i Azure genom att inaktivera Lösenordsinloggning för SSH."
services: virtual-machines-linux
documentationcenter: 
author: vlivech
manager: timlt
editor: 
tags: 
ms.assetid: 46137640-a7d2-40e5-a1e9-9effef7eb190
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/26/2016
ms.author: v-livech
ms.openlocfilehash: fb67b2f5b8b3bf2ba214858940b04f2ea9013fb5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="disable-ssh-passwords-on-your-linux-vm-by-configuring-sshd"></a><span data-ttu-id="cb5f9-103">Inaktivera SSH-lösenord på Linux-VM genom att konfigurera SSHD</span><span class="sxs-lookup"><span data-stu-id="cb5f9-103">Disable SSH passwords on your Linux VM by configuring SSHD</span></span>
<span data-ttu-id="cb5f9-104">Den här artikeln fokuserar på hur toolock ned hello inloggningssäkerhet av Linux-VM.</span><span class="sxs-lookup"><span data-stu-id="cb5f9-104">This article focuses on how toolock down hello login security of your Linux VM.</span></span>  <span data-ttu-id="cb5f9-105">Så snart hello SSH-port 22 öppnas starta toohello world robotar försök toologin gissa lösenord.</span><span class="sxs-lookup"><span data-stu-id="cb5f9-105">As soon as hello SSH port 22 is opened toohello world bots start trying toologin by guessing passwords.</span></span>  <span data-ttu-id="cb5f9-106">Vad gör vi i den här artikeln är att inaktivera Lösenordsinloggning via SSH.</span><span class="sxs-lookup"><span data-stu-id="cb5f9-106">What we will do in this article is disable password logins over SSH.</span></span>  <span data-ttu-id="cb5f9-107">Genom att ta bort hello möjlighet helt tvinga toouse lösenord som skyddar vi hello Linux VM från den här typen av brute attack.</span><span class="sxs-lookup"><span data-stu-id="cb5f9-107">By completely removing hello ability toouse passwords we protect hello Linux VM from this type of brute force attack.</span></span>  <span data-ttu-id="cb5f9-108">hello lagts till är bara att vi kommer att konfigurera Linux SSHD tooonly Tillåt inloggningar via SSH offentliga och privata nycklar, allra hello säkraste sättet toologin tooLinux.</span><span class="sxs-lookup"><span data-stu-id="cb5f9-108">hello added bonus is we will configure Linux SSHD tooonly allow logins via SSH public & private keys, by far hello most secure way toologin tooLinux.</span></span>  <span data-ttu-id="cb5f9-109">hello möjliga kombinationer av det skulle kräva tooguess hello privata nyckel är stor och därmed förhindrar robotar från även stör tootry toobrute kraft SSH-nycklar.</span><span class="sxs-lookup"><span data-stu-id="cb5f9-109">hello possible combinations of it would require tooguess hello private key is immense and therefore discourages bots from even bothering tootry toobrute force SSH keys.</span></span>

## <a name="goals"></a><span data-ttu-id="cb5f9-110">Mål</span><span class="sxs-lookup"><span data-stu-id="cb5f9-110">Goals</span></span>
* <span data-ttu-id="cb5f9-111">Konfigurera SSHD toodisallow:</span><span class="sxs-lookup"><span data-stu-id="cb5f9-111">Configure SSHD toodisallow:</span></span>
  * <span data-ttu-id="cb5f9-112">Lösenordsinloggning</span><span class="sxs-lookup"><span data-stu-id="cb5f9-112">Password logins</span></span>
  * <span data-ttu-id="cb5f9-113">Användarinloggning för rot</span><span class="sxs-lookup"><span data-stu-id="cb5f9-113">Root user login</span></span>
  * <span data-ttu-id="cb5f9-114">Anrop / svar-autentisering</span><span class="sxs-lookup"><span data-stu-id="cb5f9-114">Challenge-response authentication</span></span>
* <span data-ttu-id="cb5f9-115">Konfigurera SSHD tooallow:</span><span class="sxs-lookup"><span data-stu-id="cb5f9-115">Configure SSHD tooallow:</span></span>
  * <span data-ttu-id="cb5f9-116">endast SSH-nyckel inloggningar</span><span class="sxs-lookup"><span data-stu-id="cb5f9-116">only SSH key logins</span></span>
* <span data-ttu-id="cb5f9-117">Starta om SSHD fortfarande logga in</span><span class="sxs-lookup"><span data-stu-id="cb5f9-117">Restart SSHD while still logged in</span></span>
* <span data-ttu-id="cb5f9-118">Hello nya SSHD testkonfiguration</span><span class="sxs-lookup"><span data-stu-id="cb5f9-118">Test hello new SSHD configuration</span></span>

## <a name="introduction"></a><span data-ttu-id="cb5f9-119">Introduktion</span><span class="sxs-lookup"><span data-stu-id="cb5f9-119">Introduction</span></span>
[<span data-ttu-id="cb5f9-120">SSH definierats</span><span class="sxs-lookup"><span data-stu-id="cb5f9-120">SSH defined</span></span>](https://en.wikipedia.org/wiki/Secure_Shell)

<span data-ttu-id="cb5f9-121">SSHD är hello SSH-Server som körs på hello Linux VM.</span><span class="sxs-lookup"><span data-stu-id="cb5f9-121">SSHD is hello SSH Server that runs on hello Linux VM.</span></span>  <span data-ttu-id="cb5f9-122">SSH är en klient som körs från ett gränssnitt på arbetsstationen MacBook- eller Linux.</span><span class="sxs-lookup"><span data-stu-id="cb5f9-122">SSH is a client that runs from a shell on your MacBook or Linux workstation.</span></span>  <span data-ttu-id="cb5f9-123">SSH är också hello-protokollet används toosecure och kryptera hello kommunikationen mellan arbetsstation och hello Linux VM.</span><span class="sxs-lookup"><span data-stu-id="cb5f9-123">SSH is also hello protocol used toosecure and encrypt hello communication between your workstation and hello Linux VM.</span></span>

<span data-ttu-id="cb5f9-124">Den här artikeln är det mycket viktigt tookeep en inloggning tooyour Linux VM öppen för hela hello igenom.</span><span class="sxs-lookup"><span data-stu-id="cb5f9-124">For this article it is very important tookeep one login tooyour Linux VM open for hello entire walk through.</span></span>  <span data-ttu-id="cb5f9-125">Därför kommer vi öppna två terminaler och SSH toohello Linux VM från båda.</span><span class="sxs-lookup"><span data-stu-id="cb5f9-125">For this reason we will open two terminals and SSH toohello Linux VM from both of them.</span></span>  <span data-ttu-id="cb5f9-126">Vi använder hello första terminal toomake hello ändringar tooSSHDs konfigurationsfilen och starta om hello SSHD-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="cb5f9-126">We will use hello first terminal toomake hello changes tooSSHDs configuration file and restart hello SSHD service.</span></span>  <span data-ttu-id="cb5f9-127">Vi använder hello andra terminal tootest de ändras när hello-tjänsten startas.</span><span class="sxs-lookup"><span data-stu-id="cb5f9-127">We will use hello second terminal tootest those changes once hello service is restarted.</span></span>  <span data-ttu-id="cb5f9-128">Eftersom vi inaktiverar SSH-lösenord och beroende strikt SSH-nycklar om SSH-nycklar inte är rätt och stänger hello anslutning toohello VM, hello VM är permanent låst och ingen kommer att kunna toologin tooit kräver det toobe bort och återskapas.</span><span class="sxs-lookup"><span data-stu-id="cb5f9-128">Because we are disabling SSH passwords and relying strictly on SSH keys, if your SSH keys are not correct and you close hello connection toohello VM, hello VM will be permanently locked and no one will be able toologin tooit requiring it toobe deleted and recreated.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cb5f9-129">Krav</span><span class="sxs-lookup"><span data-stu-id="cb5f9-129">Prerequisites</span></span>
* [<span data-ttu-id="cb5f9-130">Skapa SSH-nycklar för Linux och Mac för virtuella Linux-datorer i Azure</span><span class="sxs-lookup"><span data-stu-id="cb5f9-130">Create SSH keys on Linux and Mac for Linux VMs in Azure</span></span>](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* <span data-ttu-id="cb5f9-131">Azure-konto</span><span class="sxs-lookup"><span data-stu-id="cb5f9-131">Azure account</span></span>
  * [<span data-ttu-id="cb5f9-132">gratis utvärderingsversion registrering</span><span class="sxs-lookup"><span data-stu-id="cb5f9-132">free trial signup</span></span>](https://azure.microsoft.com/pricing/free-trial/)
  * [<span data-ttu-id="cb5f9-133">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="cb5f9-133">Azure portal</span></span>](http://portal.azure.com)
* <span data-ttu-id="cb5f9-134">Linux VM körs på azure</span><span class="sxs-lookup"><span data-stu-id="cb5f9-134">Linux VM running on azure</span></span>
* <span data-ttu-id="cb5f9-135">SSH offentliga och privata nyckelpar i`~/.ssh/`</span><span class="sxs-lookup"><span data-stu-id="cb5f9-135">SSH public & private key pair in `~/.ssh/`</span></span>
* <span data-ttu-id="cb5f9-136">Offentlig SSH-nyckel i `~/.ssh/authorized_keys` på hello Linux VM</span><span class="sxs-lookup"><span data-stu-id="cb5f9-136">SSH public key in `~/.ssh/authorized_keys` on hello Linux VM</span></span>
* <span data-ttu-id="cb5f9-137">Sudo-behörighet på hello VM</span><span class="sxs-lookup"><span data-stu-id="cb5f9-137">Sudo rights on hello VM</span></span>
* <span data-ttu-id="cb5f9-138">Port 22 öppna</span><span class="sxs-lookup"><span data-stu-id="cb5f9-138">Port 22 open</span></span>

## <a name="quick-commands"></a><span data-ttu-id="cb5f9-139">Snabbkommandon</span><span class="sxs-lookup"><span data-stu-id="cb5f9-139">Quick Commands</span></span>
<span data-ttu-id="cb5f9-140">*Erfarna Linux-administratörer som vill bara hello TLDR version börja här.  Hoppa över det här avsnittet för alla andra som vill hello detaljerad förklaring och gå igenom.*</span><span class="sxs-lookup"><span data-stu-id="cb5f9-140">*Seasoned Linux Admins who just want hello TLDR version start here.  For everyone else that wants hello detailed explanation and walk through skip this section.*</span></span>

```bash
sudo vim /etc/ssh/sshd_config
```

<span data-ttu-id="cb5f9-141">Redigera hello config-fil på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="cb5f9-141">Edit hello config file as follows:</span></span>

```sh
# Change PasswordAuthentication toothis:
PasswordAuthentication no

# Change PubkeyAuthentication toothis:
PubkeyAuthentication yes

# Change PermitRootLogin toothis:
PermitRootLogin no

# Change ChallengeResponseAuthentication toothis:
ChallengeResponseAuthentication no
```

<span data-ttu-id="cb5f9-142">Starta om hello SSHD.</span><span class="sxs-lookup"><span data-stu-id="cb5f9-142">Restart hello SSHD service.</span></span> <span data-ttu-id="cb5f9-143">På Debian-baserade distributioner:</span><span class="sxs-lookup"><span data-stu-id="cb5f9-143">On Debian-based distros:</span></span>

```bash
sudo service ssh restart
```

<span data-ttu-id="cb5f9-144">På Red Hat-baserade distributioner:</span><span class="sxs-lookup"><span data-stu-id="cb5f9-144">On Red Hat-based distros:</span></span>

```bash
sudo service sshd restart
```

## <a name="detailed-walk-through"></a><span data-ttu-id="cb5f9-145">Detaljerad genomgång</span><span class="sxs-lookup"><span data-stu-id="cb5f9-145">Detailed Walk Through</span></span>
<span data-ttu-id="cb5f9-146">Inloggningen toohello Linux VM på terminal 1 (T1).</span><span class="sxs-lookup"><span data-stu-id="cb5f9-146">Login toohello Linux VM on terminal 1 (T1).</span></span>  <span data-ttu-id="cb5f9-147">Inloggningen toohello Linux VM terminal 2 (T2).</span><span class="sxs-lookup"><span data-stu-id="cb5f9-147">Login toohello Linux VM on terminal 2 (T2).</span></span>

<span data-ttu-id="cb5f9-148">På T2 vi tooedit hello SSHD konfigurationsfilen.</span><span class="sxs-lookup"><span data-stu-id="cb5f9-148">On T2 we are going tooedit hello SSHD configuration file.</span></span>  

```bash
sudo vim /etc/ssh/sshd_config
```

<span data-ttu-id="cb5f9-149">Här kommer redigera bara hello inställningar toodisable lösenord och aktivera SSH-nyckel inloggningar.</span><span class="sxs-lookup"><span data-stu-id="cb5f9-149">From here we will edit just hello settings toodisable passwords and enable SSH key logins.</span></span>  <span data-ttu-id="cb5f9-150">Det finns många inställningar i den här filen som du bör undersöka och ändra toomake Linux & SSH så säkert som du behöver.</span><span class="sxs-lookup"><span data-stu-id="cb5f9-150">There are many settings in this file that you should research and change toomake Linux & SSH as secure as you need.</span></span>

#### <a name="disable-password-logins"></a><span data-ttu-id="cb5f9-151">Inaktivera Lösenordsinloggning</span><span class="sxs-lookup"><span data-stu-id="cb5f9-151">Disable Password logins</span></span>

```sh
# Change PasswordAuthentication toothis:
PasswordAuthentication no
```

#### <a name="enable-public-key-authentication"></a><span data-ttu-id="cb5f9-152">Aktivera autentisering med offentlig nyckel</span><span class="sxs-lookup"><span data-stu-id="cb5f9-152">Enable Public Key Authentication</span></span>

```sh
# Change PubkeyAuthentication toothis:
PubkeyAuthentication yes
```

#### <a name="disable-root-login"></a><span data-ttu-id="cb5f9-153">Inaktivera rot-inloggning</span><span class="sxs-lookup"><span data-stu-id="cb5f9-153">Disable Root Login</span></span>

```sh
# Change PermitRootLogin toothis:
PermitRootLogin no
```

#### <a name="disable-challenge-response-authentication"></a><span data-ttu-id="cb5f9-154">Inaktivera anrop / svar-autentisering</span><span class="sxs-lookup"><span data-stu-id="cb5f9-154">Disable Challenge-response Authentication</span></span>
```sh
# Change ChallengeResponseAuthentication toothis:
ChallengeResponseAuthentication no
```

### <a name="restart-sshd"></a><span data-ttu-id="cb5f9-155">Starta om SSHD</span><span class="sxs-lookup"><span data-stu-id="cb5f9-155">Restart SSHD</span></span>
<span data-ttu-id="cb5f9-156">Kontrollera att du fortfarande är inloggad från hello T1-gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="cb5f9-156">From hello T1 shell verify that you are still logged in.</span></span>  <span data-ttu-id="cb5f9-157">Detta är viktigt, så att du inte blir utelåst från den virtuella datorn om SSH-nycklar inte är korrekt eftersom lösenord är nu inaktiverad.</span><span class="sxs-lookup"><span data-stu-id="cb5f9-157">This is critical so you do not get locked out of your VM if your SSH keys are not correct since passwords are now disabled.</span></span>  <span data-ttu-id="cb5f9-158">Om alla inställningar är felaktiga på din Linux VM som du kan använda T1 toofix sshd_config som du fortfarande logga in och SSH kommer att hålla hello anslutning alive under hello service SSHD starta om.</span><span class="sxs-lookup"><span data-stu-id="cb5f9-158">If any setting are incorrect on your Linux VM you can use T1 toofix sshd_config as you will still be logged in and SSH will keep hello connection alive during hello SSHD service restart.</span></span>

<span data-ttu-id="cb5f9-159">Från T2 kör:</span><span class="sxs-lookup"><span data-stu-id="cb5f9-159">From T2 run:</span></span>

##### <a name="on-hello-debian-family"></a><span data-ttu-id="cb5f9-160">På hello Debian-familjen</span><span class="sxs-lookup"><span data-stu-id="cb5f9-160">On hello Debian Family</span></span>
```bash
sudo service ssh restart
```

##### <a name="on-hello-redhat-family"></a><span data-ttu-id="cb5f9-161">På hello RedHat familj</span><span class="sxs-lookup"><span data-stu-id="cb5f9-161">On hello RedHat Family</span></span>
```bash
sudo service sshd restart
```

<span data-ttu-id="cb5f9-162">Lösenord har nu inaktiverats på den virtuella datorn skyddas mot brute force lösenord inloggningsförsök.</span><span class="sxs-lookup"><span data-stu-id="cb5f9-162">Passwords are now disabled on your VM protecting it from brute force password login attempts.</span></span>  <span data-ttu-id="cb5f9-163">Du kommer att kan toologin snabbare och säkrare med endast SSH-nycklar tillåts.</span><span class="sxs-lookup"><span data-stu-id="cb5f9-163">With only SSH Keys allowed you will be able toologin faster and much more secure.</span></span>

