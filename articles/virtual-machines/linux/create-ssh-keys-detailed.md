---
title: "aaaDetailed steg toocreate en SSH-nyckelpar för Linux virtuella datorer i Azure | Microsoft Docs"
description: "Läs om ytterligare steg toocreate ett SSH offentliga och privata nyckelpar för Linux virtuella datorer i Azure, tillsammans med specifika certifikat för olika användningsfall."
services: virtual-machines-linux
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 6/28/2017
ms.author: danlep
ms.openlocfilehash: 9ac52ef4dc87e73b9c07ccc323adc955829e2014
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="detailed-walk-through-toocreate-an-ssh-key-pair-and-additional-certificates-for-a-linux-vm-in-azure"></a><span data-ttu-id="c9e78-103">Detaljerad genomgång toocreate ett SSH-nyckelpar och ytterligare certifikat för en Linux-VM i Azure</span><span class="sxs-lookup"><span data-stu-id="c9e78-103">Detailed walk through toocreate an SSH key pair and additional certificates for a Linux VM in Azure</span></span>
<span data-ttu-id="c9e78-104">Du kan använda en SSH-nyckel för att skapa virtuella datorer på Azure som standard toousing SSH-nycklar för autentisering, vilket eliminerar hello behovet av att lösenord toolog i.</span><span class="sxs-lookup"><span data-stu-id="c9e78-104">With an SSH key pair, you can create Virtual Machines on Azure that default toousing SSH keys for authentication, eliminating hello need for passwords toolog in.</span></span> <span data-ttu-id="c9e78-105">Lösenord kan knäckas och öppna dina virtuella datorer in toorelentless brute force försök tooguess ditt lösenord.</span><span class="sxs-lookup"><span data-stu-id="c9e78-105">Passwords can be guessed, and open your VMs up toorelentless brute force attempts tooguess your password.</span></span> <span data-ttu-id="c9e78-106">Virtuella datorer som skapats med hello Azure CLI eller Resource Manager-mallar kan omfatta din offentliga SSH-nyckeln som en del av hello-distribution, ta bort post konfigurationssteg för distribution av att inaktivera Lösenordsinloggning för SSH.</span><span class="sxs-lookup"><span data-stu-id="c9e78-106">VMs created with hello Azure CLI or Resource Manager templates can include your SSH public key as part of hello deployment, removing a post deployment configuration step of disabling password logins for SSH.</span></span> <span data-ttu-id="c9e78-107">Den här artikeln innehåller detaljerade anvisningar och ytterligare exempel på genererar certifikat, såsom för användning med virtuella Linux-datorer.</span><span class="sxs-lookup"><span data-stu-id="c9e78-107">This article provides detailed steps and additional examples of generating certificates, such as for use with Linux virtual machines.</span></span> <span data-ttu-id="c9e78-108">Om du vill tooquickly skapar och använder en SSH-nyckel, se [hur toocreate en SSH-offentliga och privata nyckel koppla för Linux virtuella datorer i Azure](mac-create-ssh-keys.md).</span><span class="sxs-lookup"><span data-stu-id="c9e78-108">If you want tooquickly create and use an SSH key pair, see [How toocreate an SSH public and private key pair for Linux VMs in Azure](mac-create-ssh-keys.md).</span></span>

## <a name="understanding-ssh-keys"></a><span data-ttu-id="c9e78-109">Förstå SSH-nycklar</span><span class="sxs-lookup"><span data-stu-id="c9e78-109">Understanding SSH keys</span></span>

<span data-ttu-id="c9e78-110">Använda SSH offentliga och privata nycklar är hello enklaste sättet toolog i tooyour Linux-servrar.</span><span class="sxs-lookup"><span data-stu-id="c9e78-110">Using SSH public and private keys is hello easiest way toolog in tooyour Linux servers.</span></span> <span data-ttu-id="c9e78-111">[Offentliga nycklar](https://en.wikipedia.org/wiki/Public-key_cryptography) ger en mycket säkrare sätt toolog i tooyour Linux eller BSD VM i Azure än med lösenord, som kan vara nyckelsökningsattacker mycket enklare.</span><span class="sxs-lookup"><span data-stu-id="c9e78-111">[Public-key cryptography](https://en.wikipedia.org/wiki/Public-key_cryptography) provides a much more secure way toolog in tooyour Linux or BSD VM in Azure than passwords, which can be brute-forced far more easily.</span></span>

<span data-ttu-id="c9e78-112">Din offentliga nyckel kan delas med alla, men bara du (eller din lokala säkerhetsinfrastruktur) har tillgång till den privata nyckeln.</span><span class="sxs-lookup"><span data-stu-id="c9e78-112">Your public key can be shared with anyone; but only you (or your local security infrastructure) possess your private key.</span></span>  <span data-ttu-id="c9e78-113">hello SSH privat nyckel ska ha en [mycket säkert lösenord](https://www.xkcd.com/936/) (källa:[xkcd.com](https://xkcd.com)) toosafeguard den.</span><span class="sxs-lookup"><span data-stu-id="c9e78-113">hello SSH private key should have a [very secure password](https://www.xkcd.com/936/) (source:[xkcd.com](https://xkcd.com)) toosafeguard it.</span></span>  <span data-ttu-id="c9e78-114">Lösenordet är bara tooaccess hello privata SSH-nyckelfilen och **är inte** hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="c9e78-114">This password is just tooaccess hello private SSH key file and **is not** hello user account password.</span></span>  <span data-ttu-id="c9e78-115">När du lägger till ett lösenord tooyour SSH-nyckeln krypteras hello privata nyckeln med 128-bitars AES, så att hello privata nyckel är oanvändbar utan hello lösenord toodecrypt den.</span><span class="sxs-lookup"><span data-stu-id="c9e78-115">When you add a password tooyour SSH key, it encrypts hello private key using 128-bit AES, so that hello private key is useless without hello password toodecrypt it.</span></span>  <span data-ttu-id="c9e78-116">Om en angripare stal din privata nyckel och att nyckeln har inte ett lösenord, de skulle vara kan toouse som privat nyckel toolog i tooany servrar som har hello motsvarande offentliga nyckel.</span><span class="sxs-lookup"><span data-stu-id="c9e78-116">If an attacker stole your private key and that key did not have a password, they would be able toouse that private key toolog in tooany servers that have hello corresponding public key.</span></span>  <span data-ttu-id="c9e78-117">Om den privata nyckeln är lösenordsskyddad kan den inte användas av angriparen, vilket ger infrastrukturen i Azure ett extra säkerhetslager.</span><span class="sxs-lookup"><span data-stu-id="c9e78-117">If a private key is password protected it cannot be used by that attacker, providing an additional layer of security for your infrastructure on Azure.</span></span>

<span data-ttu-id="c9e78-118">Den här artikeln skapar en SSH-protokollversion 2 RSA offentliga och privata nyckelfilen par (även hänvisade tooas ”ssh-rsa” nycklar), som rekommenderas för distributioner med Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="c9e78-118">This article creates an SSH protocol version 2 RSA public and private key file pair (also referred tooas "ssh-rsa" keys), which are recommended for deployments with Azure Resource Manager.</span></span> <span data-ttu-id="c9e78-119">*SSH-rsa* nycklar krävs på hello [portal](https://portal.azure.com) för både klassiska och Resource Manager distributioner.</span><span class="sxs-lookup"><span data-stu-id="c9e78-119">*ssh-rsa* keys are required on hello [portal](https://portal.azure.com) for both classic and Resource Manager deployments.</span></span>

## <a name="ssh-keys-use-and-benefits"></a><span data-ttu-id="c9e78-120">Användning och fördelar med SSH-nycklar</span><span class="sxs-lookup"><span data-stu-id="c9e78-120">SSH keys use and benefits</span></span>

<span data-ttu-id="c9e78-121">Azure kräver minst 2048-bitars SSH-protokollet version 2 RSA format offentliga och privata nycklar; hello offentlig nyckelfil har hello `.pub` behållares format.</span><span class="sxs-lookup"><span data-stu-id="c9e78-121">Azure requires at least 2048-bit, SSH protocol version 2 RSA format public and private keys; hello public key file has hello `.pub` container format.</span></span> <span data-ttu-id="c9e78-122">toocreate hello nycklar används `ssh-keygen`, som ställer ett antal frågor och sedan skriver en privat nyckel och en matchande offentlig nyckel.</span><span class="sxs-lookup"><span data-stu-id="c9e78-122">toocreate hello keys use `ssh-keygen`, which asks a series of questions and then writes a private key and a matching public key.</span></span> <span data-ttu-id="c9e78-123">När en virtuell dator i Azure skapas Azure kopior hello offentliga nyckel toohello `~/.ssh/authorized_keys` mapp på hello VM.</span><span class="sxs-lookup"><span data-stu-id="c9e78-123">When an Azure VM is created, Azure copies hello public key toohello `~/.ssh/authorized_keys` folder on hello VM.</span></span> <span data-ttu-id="c9e78-124">SSH-nycklar i `~/.ssh/authorized_keys` är används toochallenge hello klienten toomatch hello motsvarande privata nyckel på en SSH-anslutning för inloggning.</span><span class="sxs-lookup"><span data-stu-id="c9e78-124">SSH keys in `~/.ssh/authorized_keys` are used toochallenge hello client toomatch hello corresponding private key on an SSH login connection.</span></span>  <span data-ttu-id="c9e78-125">När en Azure Linux VM skapas med hjälp av SSH-nycklar för autentisering, Azure konfigurerar hello SSHD server toonot Tillåt Lösenordsinloggning SSH-nycklar.</span><span class="sxs-lookup"><span data-stu-id="c9e78-125">When an Azure Linux VM is created using SSH keys for authentication, Azure configures hello SSHD server toonot allow password logins, only SSH keys.</span></span>  <span data-ttu-id="c9e78-126">Därför genom att skapa virtuella Azure Linux-datorer med SSH-nycklar kan du säkra hello VM-distribution och sparar hello vanliga efter distributionen konfigurationssteget inaktivera lösenord i hello **sshd_config** fil.</span><span class="sxs-lookup"><span data-stu-id="c9e78-126">Therefore, by creating Azure Linux VMs with SSH keys, you can help secure hello VM deployment and save yourself hello typical post-deployment configuration step of disabling passwords in hello **sshd_config** file.</span></span>

## <a name="using-ssh-keygen"></a><span data-ttu-id="c9e78-127">Använda ssh-keygen</span><span class="sxs-lookup"><span data-stu-id="c9e78-127">Using ssh-keygen</span></span>

<span data-ttu-id="c9e78-128">Det här kommandot skapas ett lösenordsskyddat (krypterat) SSH-nyckel som använder RSA 2048-bitars och den har kommenterats tooeasily identifiera den.</span><span class="sxs-lookup"><span data-stu-id="c9e78-128">This command creates a password secured (encrypted) SSH key pair using 2048-bit RSA and it is commented tooeasily identify it.</span></span>  

<span data-ttu-id="c9e78-129">SSH nycklar är som standard sparas i hello `~/.ssh` directory.</span><span class="sxs-lookup"><span data-stu-id="c9e78-129">SSH keys are by default kept in hello `~/.ssh` directory.</span></span>  <span data-ttu-id="c9e78-130">Om du inte har en `~/.ssh` directory, hello `ssh-keygen` kommando skapar den du med hello rätt behörigheter.</span><span class="sxs-lookup"><span data-stu-id="c9e78-130">If you do not have a `~/.ssh` directory, hello `ssh-keygen` command creates it for you with hello correct permissions.</span></span>

```bash
ssh-keygen \
    -t rsa \
    -b 2048 \
    -C "azureuser@myserver" \
    -f ~/.ssh/id_rsa \
    -N mypassword
```

<span data-ttu-id="c9e78-131">*Kommandot förklarat*</span><span class="sxs-lookup"><span data-stu-id="c9e78-131">*Command explained*</span></span>

<span data-ttu-id="c9e78-132">`ssh-keygen`= hello används toocreate hello nycklar</span><span class="sxs-lookup"><span data-stu-id="c9e78-132">`ssh-keygen` = hello program used toocreate hello keys</span></span>

<span data-ttu-id="c9e78-133">`-t rsa`= typen av nyckel toocreate som är hello RSA format [wikipedia][parens slutet](`https://en.wikipedia.org/wiki/RSA_(cryptosystem) `) 
 `-b 2048` = bitar i hello nyckel</span><span class="sxs-lookup"><span data-stu-id="c9e78-133">`-t rsa` = type of key toocreate which is hello RSA format [wikipedia][parens at end](`https://en.wikipedia.org/wiki/RSA_(cryptosystem) `)
`-b 2048` = bits of hello key</span></span>

<span data-ttu-id="c9e78-134">`-C "azureuser@myserver"`= en kommentar som läggs toohello slutet av hello offentlig nyckelfil tooeasily identifiera den.</span><span class="sxs-lookup"><span data-stu-id="c9e78-134">`-C "azureuser@myserver"` = a comment appended toohello end of hello public key file tooeasily identify it.</span></span>  <span data-ttu-id="c9e78-135">Normalt används ett e-postmeddelande som hello kommentar men du kan använda det som fungerar bäst för din infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="c9e78-135">Normally an email is used as hello comment but you can use whatever works best for your infrastructure.</span></span>

## <a name="classic-deploy-using-asm"></a><span data-ttu-id="c9e78-136">Klassisk distribution med hjälp av `asm`</span><span class="sxs-lookup"><span data-stu-id="c9e78-136">Classic deploy using `asm`</span></span>

<span data-ttu-id="c9e78-137">Om du använder hello klassiska distributionsmodellen (`asm` hello CLI-läge), du kan använda en offentlig SSH-RSA-nyckel eller en RFC4716 formaterad nyckel i en pem-behållare.</span><span class="sxs-lookup"><span data-stu-id="c9e78-137">If you are using hello classic deployment model (`asm` mode in hello CLI), you can use an SSH-RSA public key or an RFC4716 formatted key in a pem container.</span></span>  <span data-ttu-id="c9e78-138">hello SSH-RSA-offentliga nyckel är vad som skapats tidigare i den här artikeln använder `ssh-keygen`.</span><span class="sxs-lookup"><span data-stu-id="c9e78-138">hello SSH-RSA public key is what was created earlier in this article using `ssh-keygen`.</span></span>

<span data-ttu-id="c9e78-139">toocreate en RFC4716 formaterad nyckeln från en befintlig offentlig SSH-nyckel:</span><span class="sxs-lookup"><span data-stu-id="c9e78-139">toocreate a RFC4716 formatted key from an existing SSH public key:</span></span>

```bash
ssh-keygen \
-f ~/.ssh/id_rsa.pub \
-e \
-m RFC4716 > ~/.ssh/id_ssh2.pem
```

## <a name="example-of-ssh-keygen"></a><span data-ttu-id="c9e78-140">Exempel på ssh-keygen</span><span class="sxs-lookup"><span data-stu-id="c9e78-140">Example of ssh-keygen</span></span>

```bash
ssh-keygen -t rsa -b 2048 -C "azureuser@myserver"
Generating public/private rsa key pair.
Enter file in which toosave hello key (/home/azureuser/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/azureuser/.ssh/id_rsa.
Your public key has been saved in /home/azureuser/.ssh/id_rsa.pub.
hello key fingerprint is:
14:a3:cb:3e:78:ad:25:cc:55:e9:0c:08:e5:d1:a9:08 azureuser@myserver
hello keys randomart image is:
+--[ RSA 2048]----+
|        o o. .   |
|      E. = .o    |
|      ..o...     |
|     . o....     |
|      o S =      |
|     . + O       |
|      + = =      |
|       o +       |
|        .        |
+-----------------+
```

<span data-ttu-id="c9e78-141">Sparade nyckelfiler:</span><span class="sxs-lookup"><span data-stu-id="c9e78-141">Saved key files:</span></span>

`Enter file in which toosave hello key (/home/azureuser/.ssh/id_rsa): ~/.ssh/id_rsa`

<span data-ttu-id="c9e78-142">hello nyckelpar namn för den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="c9e78-142">hello key pair name for this article.</span></span>  <span data-ttu-id="c9e78-143">Ett nyckelpar med namnet **id_rsa** är hello standard och vissa verktyg kan förvänta sig hello **id_rsa** privata nyckel filnamn så är det en bra idé.</span><span class="sxs-lookup"><span data-stu-id="c9e78-143">Having a key pair named **id_rsa** is hello default and some tools might expect hello **id_rsa** private key file name so having one is a good idea.</span></span> <span data-ttu-id="c9e78-144">hello directory `~/.ssh/` är hello standardplatsen för SSH-nyckelpar och hello SSH-konfigurationsfil.</span><span class="sxs-lookup"><span data-stu-id="c9e78-144">hello directory `~/.ssh/` is hello default location for SSH key pairs and hello SSH config file.</span></span>  <span data-ttu-id="c9e78-145">Om den inte anges med en fullständig sökväg `ssh-keygen` skapar hello nycklar i hello aktuella arbetskatalogen, inte hello standard `~/.ssh`.</span><span class="sxs-lookup"><span data-stu-id="c9e78-145">If not specified with a full path, `ssh-keygen` creates hello keys in hello current working directory, not hello default `~/.ssh`.</span></span>

<span data-ttu-id="c9e78-146">En lista över hello `~/.ssh` directory.</span><span class="sxs-lookup"><span data-stu-id="c9e78-146">A listing of hello `~/.ssh` directory.</span></span>

```bash
ls -al ~/.ssh
-rw------- 1 azureuser staff  1675 Aug 25 18:04 id_rsa
-rw-r--r-- 1 azureuser staff   410 Aug 25 18:04 id_rsa.pub
```

<span data-ttu-id="c9e78-147">Nyckellösenord:</span><span class="sxs-lookup"><span data-stu-id="c9e78-147">Key Password:</span></span>

`Enter passphrase (empty for no passphrase):`

<span data-ttu-id="c9e78-148">`ssh-keygen`refererar tooa lösenord för hello-fil för privat nyckel som ”en lösenfras”.</span><span class="sxs-lookup"><span data-stu-id="c9e78-148">`ssh-keygen` refers tooa password for hello private key file as "a passphrase."</span></span>  <span data-ttu-id="c9e78-149">Det är *starkt* rekommenderas tooadd lösenord tooyour privat nyckel.</span><span class="sxs-lookup"><span data-stu-id="c9e78-149">It is *strongly* recommended tooadd a password tooyour private key.</span></span> <span data-ttu-id="c9e78-150">Utan ett lösenord skyddar hello nyckelfilen, kan alla med hello-fil använda det toolog i tooany-server som har hello motsvarande offentlig nyckel.</span><span class="sxs-lookup"><span data-stu-id="c9e78-150">Without a password protecting hello key file, anyone with hello file can use it toolog in tooany server that has hello corresponding public key.</span></span> <span data-ttu-id="c9e78-151">Lägga till ett lösenord (lösenfras) ger bättre skydd om någon kan toogain åtkomst tooyour fil för privat nyckel, gett dig tid toochange hello nycklar används tooauthenticate du.</span><span class="sxs-lookup"><span data-stu-id="c9e78-151">Adding a password (passphrase) offers more protection in case someone is able toogain access tooyour private key file, given you time toochange hello keys used tooauthenticate you.</span></span>

## <a name="using-ssh-agent-toostore-your-private-key-password"></a><span data-ttu-id="c9e78-152">Med hjälp av ssh-agent toostore lösenordet för privat nyckel</span><span class="sxs-lookup"><span data-stu-id="c9e78-152">Using ssh-agent toostore your private key password</span></span>

<span data-ttu-id="c9e78-153">tooavoid lösenordet fil för privat nyckel med varje SSH-inloggning kan du använda `ssh-agent` toocache lösenordet fil för privat nyckel.</span><span class="sxs-lookup"><span data-stu-id="c9e78-153">tooavoid typing your private key file password with every SSH login, you can use `ssh-agent` toocache your private key file password.</span></span> <span data-ttu-id="c9e78-154">Om du använder en Mac hello OS x-nyckelringen på ett säkert sätt lagrar hello privata nycklar som lösenord när du anropar `ssh-agent`.</span><span class="sxs-lookup"><span data-stu-id="c9e78-154">If you are using a Mac, hello OSX Keychain securely stores hello private key passwords when you invoke `ssh-agent`.</span></span>

<span data-ttu-id="c9e78-155">Kontrollera och använder ssh-agent och ssh-Lägg till tooinform hello SSH system om hello viktiga filer så att hello lösenfras inte behöver toobe används interaktivt.</span><span class="sxs-lookup"><span data-stu-id="c9e78-155">Verify and use ssh-agent and ssh-add tooinform hello SSH system about hello key files so that hello passphrase will not need toobe used interactively.</span></span>

```bash
eval "$(ssh-agent -s)"
```

<span data-ttu-id="c9e78-156">Lägg nu till hello privata nyckeln för`ssh-agent` hello kommandot `ssh-add`.</span><span class="sxs-lookup"><span data-stu-id="c9e78-156">Now add hello private key too`ssh-agent` using hello command `ssh-add`.</span></span>

```bash
ssh-add ~/.ssh/id_rsa
```

<span data-ttu-id="c9e78-157">lösenordet för hello privata nyckeln lagras nu på `ssh-agent`.</span><span class="sxs-lookup"><span data-stu-id="c9e78-157">hello private key password is now stored in `ssh-agent`.</span></span>

## <a name="using-ssh-copy-id-toocopy-hello-key-tooan-existing-vm"></a><span data-ttu-id="c9e78-158">Med hjälp av `ssh-copy-id` toocopy hello viktiga tooan befintliga VM</span><span class="sxs-lookup"><span data-stu-id="c9e78-158">Using `ssh-copy-id` toocopy hello key tooan existing VM</span></span>
<span data-ttu-id="c9e78-159">Om du redan har skapat en virtuell dator kan du installera hello nya SSH offentlig nyckel tooyour Linux VM med:</span><span class="sxs-lookup"><span data-stu-id="c9e78-159">If you have already created a VM you can install hello new SSH public key tooyour Linux VM with:</span></span>

```bash
ssh-copy-id -i ~/.ssh/id_rsa.pub ahmet@myserver
```

## <a name="create-and-configure-an-ssh-config-file"></a><span data-ttu-id="c9e78-160">Skapa och konfigurera en SSH-konfigurationsfil</span><span class="sxs-lookup"><span data-stu-id="c9e78-160">Create and configure an SSH config file</span></span>

<span data-ttu-id="c9e78-161">Det är en rekommenderad bästa praxis toocreate och konfigurera en `~/.ssh/config` filen toospeed upp inloggningar och för att optimera din beteende för SSH-klienten.</span><span class="sxs-lookup"><span data-stu-id="c9e78-161">It is a recommended best practice toocreate and configure an `~/.ssh/config` file toospeed up log ins and for optimizing your SSH client behavior.</span></span>

<span data-ttu-id="c9e78-162">hello följande exempel illustrerar en standardkonfiguration.</span><span class="sxs-lookup"><span data-stu-id="c9e78-162">hello following example shows a standard configuration.</span></span>

### <a name="create-hello-file"></a><span data-ttu-id="c9e78-163">Skapa hello-fil</span><span class="sxs-lookup"><span data-stu-id="c9e78-163">Create hello file</span></span>

```bash
touch ~/.ssh/config
```

### <a name="edit-hello-file-tooadd-hello-new-ssh-configuration"></a><span data-ttu-id="c9e78-164">Redigera hello filen tooadd hello nya SSH-konfigurationen:</span><span class="sxs-lookup"><span data-stu-id="c9e78-164">Edit hello file tooadd hello new SSH configuration:</span></span>

```bash
vim ~/.ssh/config
```

### <a name="example-sshconfig-file"></a><span data-ttu-id="c9e78-165">`~/.ssh/config`-exempelfil:</span><span class="sxs-lookup"><span data-stu-id="c9e78-165">Example `~/.ssh/config` file:</span></span>

```bash
# Azure Keys
Host fedora22
  Hostname 102.160.203.241
  User ahmet
# ./Azure Keys
# Default Settings
Host *
  PubkeyAuthentication=yes
  IdentitiesOnly=yes
  ServerAliveInterval=60
  ServerAliveCountMax=30
  ControlMaster auto
  ControlPath ~/.ssh/SSHConnections/ssh-%r@%h:%p
  ControlPersist 4h
  IdentityFile ~/.ssh/id_rsa
```

<span data-ttu-id="c9e78-166">Den här SSH config ger du avsnitten för varje server tooenable varje toohave eget dedikerade nyckelpar.</span><span class="sxs-lookup"><span data-stu-id="c9e78-166">This SSH config gives you sections for each server tooenable each toohave its own dedicated key pair.</span></span> <span data-ttu-id="c9e78-167">Hej standardinställningar (`Host *`) är för alla värdar som inte matchar någon av hello specifika värdar högre upp i hello config-fil.</span><span class="sxs-lookup"><span data-stu-id="c9e78-167">hello default settings (`Host *`) are for any hosts that do not match any of hello specific hosts higher up in hello config file.</span></span>

### <a name="config-file-explained"></a><span data-ttu-id="c9e78-168">Konfigurationsfilen förklarad</span><span class="sxs-lookup"><span data-stu-id="c9e78-168">Config file explained</span></span>

<span data-ttu-id="c9e78-169">`Host`= hello namnet på hello värddatorn som anropas på hello terminal.</span><span class="sxs-lookup"><span data-stu-id="c9e78-169">`Host` = hello name of hello host being called on hello terminal.</span></span>  <span data-ttu-id="c9e78-170">`ssh fedora22`Anger `SSH` toouse hello värden i hello inställningsblocket med etiketten `Host fedora22` Obs: värden kan vara vilken etikett som är logiska för din användning och som inte representerar hello faktiska värdnamnet för en server.</span><span class="sxs-lookup"><span data-stu-id="c9e78-170">`ssh fedora22` tells `SSH` toouse hello values in hello settings block labeled `Host fedora22`  NOTE: Host can be any label that is logical for your usage and does not represent hello actual hostname of any server.</span></span>

<span data-ttu-id="c9e78-171">`Hostname 102.160.203.241`= hello IP-adress eller DNS-namn för hello-server som används.</span><span class="sxs-lookup"><span data-stu-id="c9e78-171">`Hostname 102.160.203.241` = hello IP address or DNS name for hello server being accessed.</span></span>

<span data-ttu-id="c9e78-172">`User ahmet`= hello fjärranvändare konto toouse när du loggar in toohello server.</span><span class="sxs-lookup"><span data-stu-id="c9e78-172">`User ahmet` = hello remote user account toouse when logging in toohello server.</span></span>

<span data-ttu-id="c9e78-173">`PubKeyAuthentication yes`= meddelar SSH du vill ha toouse toolog en SSH-nyckel.</span><span class="sxs-lookup"><span data-stu-id="c9e78-173">`PubKeyAuthentication yes` = tells SSH you want toouse an SSH key toolog in.</span></span>

<span data-ttu-id="c9e78-174">`IdentityFile /home/ahmet/.ssh/id_id_rsa`= hello SSH privat nyckel och motsvarande toouse med offentlig nyckel för autentisering.</span><span class="sxs-lookup"><span data-stu-id="c9e78-174">`IdentityFile /home/ahmet/.ssh/id_id_rsa` = hello SSH private key and corresponding public key toouse for authentication.</span></span>

## <a name="ssh-into-linux-without-a-password"></a><span data-ttu-id="c9e78-175">SSH i Linux utan lösenord</span><span class="sxs-lookup"><span data-stu-id="c9e78-175">SSH into Linux without a password</span></span>

<span data-ttu-id="c9e78-176">Nu när du har en SSH-nyckel och en konfigurerad SSH-konfigurationsfil är kan toolog i tooyour Linux VM snabbt och säkert.</span><span class="sxs-lookup"><span data-stu-id="c9e78-176">Now that you have an SSH key pair and a configured SSH config file, you are able toolog in tooyour Linux VM quickly and securely.</span></span> <span data-ttu-id="c9e78-177">hello första gången du loggar i tooa servern med hjälp av en SSH-nyckel hello kommandotolkar du för hello lösenfras för denna nyckel.</span><span class="sxs-lookup"><span data-stu-id="c9e78-177">hello first time you log in tooa server using an SSH key hello command prompts you for hello passphrase for that key file.</span></span>

```bash
ssh fedora22
```

### <a name="command-explained"></a><span data-ttu-id="c9e78-178">Kommandot förklarat</span><span class="sxs-lookup"><span data-stu-id="c9e78-178">Command explained</span></span>

<span data-ttu-id="c9e78-179">När `ssh fedora22` körs SSH först upp och läser in inställningarna från hello `Host fedora22` blocket och läser sedan alla hello återstående inställningarna från hello sista blocket, `Host *`.</span><span class="sxs-lookup"><span data-stu-id="c9e78-179">When `ssh fedora22` is executed SSH first locates and loads any settings from hello `Host fedora22` block, and then loads all hello remaining settings from hello last block, `Host *`.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c9e78-180">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c9e78-180">Next Steps</span></span>

<span data-ttu-id="c9e78-181">Nästa in toocreate virtuella Azure Linux-datorer använder hello nya offentliga SSH-nyckeln.</span><span class="sxs-lookup"><span data-stu-id="c9e78-181">Next up is toocreate Azure Linux VMs using hello new SSH public key.</span></span>  <span data-ttu-id="c9e78-182">Virtuella Azure-datorer som har skapats med en offentlig SSH-nyckel som hello inloggning är bättre skyddade än virtuella datorer som skapats med hello inloggningen standardmetoden, lösenord.</span><span class="sxs-lookup"><span data-stu-id="c9e78-182">Azure VMs that are created with an SSH public key as hello login are better secured than VMs created with hello default login method, passwords.</span></span>  <span data-ttu-id="c9e78-183">Lösenord är som standard inaktiverade för Virtuella Azure-datorer som skapas med SSH-nycklar för att undvika råstyrkeattacker som försöker gissa lösenord.</span><span class="sxs-lookup"><span data-stu-id="c9e78-183">Azure VMs created using SSH keys are by default configured with passwords disabled, avoiding brute-forced guessing attempts.</span></span>

* [<span data-ttu-id="c9e78-184">Skapa en säker virtuell Linux-dator med hjälp av en Azure-mall</span><span class="sxs-lookup"><span data-stu-id="c9e78-184">Create a secure Linux VM using an Azure template</span></span>](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="c9e78-185">Skapa en säker Linux VM som använder hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="c9e78-185">Create a secure Linux VM using hello Azure portal</span></span>](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="c9e78-186">Skapa en säker Linux VM som använder hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="c9e78-186">Create a secure Linux VM using hello Azure CLI</span></span>](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
