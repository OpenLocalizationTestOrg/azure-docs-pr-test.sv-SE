---
title: "Detaljerade steg för att skapa ett SSH-nyckelpar för virtuella Linux-datorer i Azure | Microsoft Docs"
description: "Lär dig ytterligare steg för att skapa ett offentligt och privat SSH-nyckelpar för virtuella Linux-datorer i Azure, tillsammans med specifika certifikat för olika användningsfall."
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
ms.openlocfilehash: d4548c6f21d04effd57ea36e4fc0d15f77568903
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="detailed-walk-through-to-create-an-ssh-key-pair-and-additional-certificates-for-a-linux-vm-in-azure"></a><span data-ttu-id="6c93f-103">Detaljerad genomgång för att skapa ett SSH-nyckelpar och ytterligare certifikat för en virtuell Linux-dator i Azure</span><span class="sxs-lookup"><span data-stu-id="6c93f-103">Detailed walk through to create an SSH key pair and additional certificates for a Linux VM in Azure</span></span>
<span data-ttu-id="6c93f-104">Med ett SSH-nyckelpar kan du skapa virtuella datorer i Azure som använder SSH-nycklar för autentisering som standard, vilket gör att inga lösenord krävs för att logga in.</span><span class="sxs-lookup"><span data-stu-id="6c93f-104">With an SSH key pair, you can create Virtual Machines on Azure that default to using SSH keys for authentication, eliminating the need for passwords to log in.</span></span> <span data-ttu-id="6c93f-105">Det går att gissa sig fram till lösenord och dina virtuella datorer blir sårbara för råstyrkeattacker som försöker gissa ditt lösenord.</span><span class="sxs-lookup"><span data-stu-id="6c93f-105">Passwords can be guessed, and open your VMs up to relentless brute force attempts to guess your password.</span></span> <span data-ttu-id="6c93f-106">Virtuella datorer som skapas med Azure CLI- eller Resource Manager-mallar kan omfatta din offentliga SSH-nyckel som en del av distributionen, så att inga postdistributionskonfigurationssteg krävs för att inaktivera lösenordsinloggningar för SSH.</span><span class="sxs-lookup"><span data-stu-id="6c93f-106">VMs created with the Azure CLI or Resource Manager templates can include your SSH public key as part of the deployment, removing a post deployment configuration step of disabling password logins for SSH.</span></span> <span data-ttu-id="6c93f-107">Den här artikeln innehåller detaljerade anvisningar och ytterligare exempel på genererar certifikat, såsom för användning med virtuella Linux-datorer.</span><span class="sxs-lookup"><span data-stu-id="6c93f-107">This article provides detailed steps and additional examples of generating certificates, such as for use with Linux virtual machines.</span></span> <span data-ttu-id="6c93f-108">Om du snabbt vill skapa och använda ett SSH-nyckelpar kan du se [Skapa ett offentligt och ett privat SSH-nyckelpar för virtuella Linux-datorer i Azure](mac-create-ssh-keys.md).</span><span class="sxs-lookup"><span data-stu-id="6c93f-108">If you want to quickly create and use an SSH key pair, see [How to create an SSH public and private key pair for Linux VMs in Azure](mac-create-ssh-keys.md).</span></span>

## <a name="understanding-ssh-keys"></a><span data-ttu-id="6c93f-109">Förstå SSH-nycklar</span><span class="sxs-lookup"><span data-stu-id="6c93f-109">Understanding SSH keys</span></span>

<span data-ttu-id="6c93f-110">Användningen av offentliga och privata SSH-nycklar är det enklaste sättet att logga in på dina Linux-servrar.</span><span class="sxs-lookup"><span data-stu-id="6c93f-110">Using SSH public and private keys is the easiest way to log in to your Linux servers.</span></span> <span data-ttu-id="6c93f-111">[Kryptografi med offentliga nycklar](https://en.wikipedia.org/wiki/Public-key_cryptography) är ett säkrare sätt att logga in på en Linux- eller BSD-baserad virtuell dator i Azure än lösenord, som är mer sårbara för råstyrkeattacker.</span><span class="sxs-lookup"><span data-stu-id="6c93f-111">[Public-key cryptography](https://en.wikipedia.org/wiki/Public-key_cryptography) provides a much more secure way to log in to your Linux or BSD VM in Azure than passwords, which can be brute-forced far more easily.</span></span>

<span data-ttu-id="6c93f-112">Din offentliga nyckel kan delas med alla, men bara du (eller din lokala säkerhetsinfrastruktur) har tillgång till den privata nyckeln.</span><span class="sxs-lookup"><span data-stu-id="6c93f-112">Your public key can be shared with anyone; but only you (or your local security infrastructure) possess your private key.</span></span>  <span data-ttu-id="6c93f-113">Den privata SSH-nyckeln bör ha ett [mycket säkert lösenord ](https://www.xkcd.com/936/) (källa:[xkcd.com](https://xkcd.com)) som skyddar den.</span><span class="sxs-lookup"><span data-stu-id="6c93f-113">The SSH private key should have a [very secure password](https://www.xkcd.com/936/) (source:[xkcd.com](https://xkcd.com)) to safeguard it.</span></span>  <span data-ttu-id="6c93f-114">Det här lösenordet används bara för att få åtkomst till den privata SSH-nyckelfilen och **är inte** lösenordet för användarkontot.</span><span class="sxs-lookup"><span data-stu-id="6c93f-114">This password is just to access the private SSH key file and **is not** the user account password.</span></span>  <span data-ttu-id="6c93f-115">När du lägger till ett lösenord för SSH-nyckeln krypteras den privata nyckeln med hjälp av 128-bitars AES, så att den inte kan användas utan lösenordet som krävs för att kryptera upp den.</span><span class="sxs-lookup"><span data-stu-id="6c93f-115">When you add a password to your SSH key, it encrypts the private key using 128-bit AES, so that the private key is useless without the password to decrypt it.</span></span>  <span data-ttu-id="6c93f-116">Om en angripare stjäl din privata nyckel och om nyckeln inte skyddas med ett lösenord, kan angriparen använda den privata nyckeln för att logga in på alla servrar som den offentliga nyckeln används för.</span><span class="sxs-lookup"><span data-stu-id="6c93f-116">If an attacker stole your private key and that key did not have a password, they would be able to use that private key to log in to any servers that have the corresponding public key.</span></span>  <span data-ttu-id="6c93f-117">Om den privata nyckeln är lösenordsskyddad kan den inte användas av angriparen, vilket ger infrastrukturen i Azure ett extra säkerhetslager.</span><span class="sxs-lookup"><span data-stu-id="6c93f-117">If a private key is password protected it cannot be used by that attacker, providing an additional layer of security for your infrastructure on Azure.</span></span>

<span data-ttu-id="6c93f-118">Den här artikeln skapar ett offentligt och privat nyckelfilpar med SSH-protokollversion 2 (kallas även ”ssh-rsa”-nycklar), vilket rekommenderas för distributioner med Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="6c93f-118">This article creates an SSH protocol version 2 RSA public and private key file pair (also referred to as "ssh-rsa" keys), which are recommended for deployments with Azure Resource Manager.</span></span> <span data-ttu-id="6c93f-119">*ssh-rsa*-nycklar krävs på [portalen](https://portal.azure.com) för både klassiska distributioner och Resource Manager-distributioner.</span><span class="sxs-lookup"><span data-stu-id="6c93f-119">*ssh-rsa* keys are required on the [portal](https://portal.azure.com) for both classic and Resource Manager deployments.</span></span>

## <a name="ssh-keys-use-and-benefits"></a><span data-ttu-id="6c93f-120">Användning och fördelar med SSH-nycklar</span><span class="sxs-lookup"><span data-stu-id="6c93f-120">SSH keys use and benefits</span></span>

<span data-ttu-id="6c93f-121">Azure kräver offentliga och privata nycklar med minst 2 048 bitar, SSH-protokollversion 2 i RSA-format. Den offentliga nyckelfilen har behållarformatet `.pub`.</span><span class="sxs-lookup"><span data-stu-id="6c93f-121">Azure requires at least 2048-bit, SSH protocol version 2 RSA format public and private keys; the public key file has the `.pub` container format.</span></span> <span data-ttu-id="6c93f-122">Skapa nycklarna med `ssh-keygen`, som ställer ett antal frågor och sedan skriver en privat nyckel och en matchande offentlig nyckel.</span><span class="sxs-lookup"><span data-stu-id="6c93f-122">To create the keys use `ssh-keygen`, which asks a series of questions and then writes a private key and a matching public key.</span></span> <span data-ttu-id="6c93f-123">När en virtuell Azure-dator skapas kopierar Azure den offentliga nyckeln till mappen `~/.ssh/authorized_keys` på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="6c93f-123">When an Azure VM is created, Azure copies the public key to the `~/.ssh/authorized_keys` folder on the VM.</span></span> <span data-ttu-id="6c93f-124">SSH-nycklar i `~/.ssh/authorized_keys` används för att tvinga klienten att matcha motsvarande privata nyckel vid en SSH-inloggningsanslutning.</span><span class="sxs-lookup"><span data-stu-id="6c93f-124">SSH keys in `~/.ssh/authorized_keys` are used to challenge the client to match the corresponding private key on an SSH login connection.</span></span>  <span data-ttu-id="6c93f-125">När en virtuell Azure Linux-dator har skapats med hjälp av SSH-nycklar för autentisering, så konfigurerar Azure SSHD-servern för att den inte tillåter lösenordsinloggningar, utan enbart SSH-nycklar.</span><span class="sxs-lookup"><span data-stu-id="6c93f-125">When an Azure Linux VM is created using SSH keys for authentication, Azure configures the SSHD server to not allow password logins, only SSH keys.</span></span>  <span data-ttu-id="6c93f-126">Därför kan du genom att skapa virtuella datorer i Azure Linux med SSH-nycklar skydda VM-distributionen och slippa de vanliga konfigurationsstegen efter distribution för att inaktivera lösenord i filen **sshd_config**.</span><span class="sxs-lookup"><span data-stu-id="6c93f-126">Therefore, by creating Azure Linux VMs with SSH keys, you can help secure the VM deployment and save yourself the typical post-deployment configuration step of disabling passwords in the **sshd_config** file.</span></span>

## <a name="using-ssh-keygen"></a><span data-ttu-id="6c93f-127">Använda ssh-keygen</span><span class="sxs-lookup"><span data-stu-id="6c93f-127">Using ssh-keygen</span></span>

<span data-ttu-id="6c93f-128">Med det här kommandot skapas ett lösenordsskyddat (krypterat) SSH-nyckelpar med 2048 bitars RSA. Kommentarer har lagts till som gör det lättare att identifiera det.</span><span class="sxs-lookup"><span data-stu-id="6c93f-128">This command creates a password secured (encrypted) SSH key pair using 2048-bit RSA and it is commented to easily identify it.</span></span>  

<span data-ttu-id="6c93f-129">SSH-nycklarna sparas som standard i `~/.ssh`-katalogen.</span><span class="sxs-lookup"><span data-stu-id="6c93f-129">SSH keys are by default kept in the `~/.ssh` directory.</span></span>  <span data-ttu-id="6c93f-130">Om du inte har någon `~/.ssh`-katalog skapar `ssh-keygen`-kommandot en åt dig med rätt behörigheter.</span><span class="sxs-lookup"><span data-stu-id="6c93f-130">If you do not have a `~/.ssh` directory, the `ssh-keygen` command creates it for you with the correct permissions.</span></span>

```bash
ssh-keygen \
    -t rsa \
    -b 2048 \
    -C "azureuser@myserver" \
    -f ~/.ssh/id_rsa \
    -N mypassword
```

<span data-ttu-id="6c93f-131">*Kommandot förklarat*</span><span class="sxs-lookup"><span data-stu-id="6c93f-131">*Command explained*</span></span>

<span data-ttu-id="6c93f-132">`ssh-keygen` = det program som används för att skapa nycklarna</span><span class="sxs-lookup"><span data-stu-id="6c93f-132">`ssh-keygen` = the program used to create the keys</span></span>

<span data-ttu-id="6c93f-133">`-t rsa`= typen av nyckel ska skapas som är RSA-formatet [wikipedia][parens slutet](`https://en.wikipedia.org/wiki/RSA_(cryptosystem) `) 
 `-b 2048` = bitar i nyckeln</span><span class="sxs-lookup"><span data-stu-id="6c93f-133">`-t rsa` = type of key to create which is the RSA format [wikipedia][parens at end](`https://en.wikipedia.org/wiki/RSA_(cryptosystem) `)
`-b 2048` = bits of the key</span></span>

<span data-ttu-id="6c93f-134">`-C "azureuser@myserver"` = en kommentar i slutet av filen för den offentliga nyckeln som gör det lätt att identifiera den.</span><span class="sxs-lookup"><span data-stu-id="6c93f-134">`-C "azureuser@myserver"` = a comment appended to the end of the public key file to easily identify it.</span></span>  <span data-ttu-id="6c93f-135">Normalt används en e-postadress som kommentaren, men du kan använda det som fungerar bäst för din infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="6c93f-135">Normally an email is used as the comment but you can use whatever works best for your infrastructure.</span></span>

## <a name="classic-deploy-using-asm"></a><span data-ttu-id="6c93f-136">Klassisk distribution med hjälp av `asm`</span><span class="sxs-lookup"><span data-stu-id="6c93f-136">Classic deploy using `asm`</span></span>

<span data-ttu-id="6c93f-137">Om du använder den klassiska distributionsmodellen (`asm` CLI-läge), du kan använda en offentlig SSH-RSA-nyckel eller en RFC4716 formaterad nyckel i en pem-behållare.</span><span class="sxs-lookup"><span data-stu-id="6c93f-137">If you are using the classic deployment model (`asm` mode in the CLI), you can use an SSH-RSA public key or an RFC4716 formatted key in a pem container.</span></span>  <span data-ttu-id="6c93f-138">Den offentliga nyckeln för SSH-RSA är den som har skapats tidigare i den här artikeln med hjälp av `ssh-keygen`.</span><span class="sxs-lookup"><span data-stu-id="6c93f-138">The SSH-RSA public key is what was created earlier in this article using `ssh-keygen`.</span></span>

<span data-ttu-id="6c93f-139">Så här skapar du en RFC4716-formaterad nyckel från en befintlig offentlig SSH-nyckel:</span><span class="sxs-lookup"><span data-stu-id="6c93f-139">To create a RFC4716 formatted key from an existing SSH public key:</span></span>

```bash
ssh-keygen \
-f ~/.ssh/id_rsa.pub \
-e \
-m RFC4716 > ~/.ssh/id_ssh2.pem
```

## <a name="example-of-ssh-keygen"></a><span data-ttu-id="6c93f-140">Exempel på ssh-keygen</span><span class="sxs-lookup"><span data-stu-id="6c93f-140">Example of ssh-keygen</span></span>

```bash
ssh-keygen -t rsa -b 2048 -C "azureuser@myserver"
Generating public/private rsa key pair.
Enter file in which to save the key (/home/azureuser/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/azureuser/.ssh/id_rsa.
Your public key has been saved in /home/azureuser/.ssh/id_rsa.pub.
The key fingerprint is:
14:a3:cb:3e:78:ad:25:cc:55:e9:0c:08:e5:d1:a9:08 azureuser@myserver
The keys randomart image is:
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

<span data-ttu-id="6c93f-141">Sparade nyckelfiler:</span><span class="sxs-lookup"><span data-stu-id="6c93f-141">Saved key files:</span></span>

`Enter file in which to save the key (/home/azureuser/.ssh/id_rsa): ~/.ssh/id_rsa`

<span data-ttu-id="6c93f-142">Namnet på nyckelparet i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="6c93f-142">The key pair name for this article.</span></span>  <span data-ttu-id="6c93f-143">Ett nyckelpar med namnet **id_rsa** är standard och vissa verktyg kan förvänta sig att namnet på filen för den privata nyckeln är **id_rsa**. Därför är det en bra idé att använda det.</span><span class="sxs-lookup"><span data-stu-id="6c93f-143">Having a key pair named **id_rsa** is the default and some tools might expect the **id_rsa** private key file name so having one is a good idea.</span></span> <span data-ttu-id="6c93f-144">Katalogen `~/.ssh/` är standardplatsen för SSH-nyckelpar och SSH-konfigurationsfilen.</span><span class="sxs-lookup"><span data-stu-id="6c93f-144">The directory `~/.ssh/` is the default location for SSH key pairs and the SSH config file.</span></span>  <span data-ttu-id="6c93f-145">Om detta inte anges med en fullständig sökväg skapar `ssh-keygen` nycklarna i den aktuella arbetskatalogen, inte standarden `~/.ssh`.</span><span class="sxs-lookup"><span data-stu-id="6c93f-145">If not specified with a full path, `ssh-keygen` creates the keys in the current working directory, not the default `~/.ssh`.</span></span>

<span data-ttu-id="6c93f-146">En lista över `~/.ssh`-katalogen.</span><span class="sxs-lookup"><span data-stu-id="6c93f-146">A listing of the `~/.ssh` directory.</span></span>

```bash
ls -al ~/.ssh
-rw------- 1 azureuser staff  1675 Aug 25 18:04 id_rsa
-rw-r--r-- 1 azureuser staff   410 Aug 25 18:04 id_rsa.pub
```

<span data-ttu-id="6c93f-147">Nyckellösenord:</span><span class="sxs-lookup"><span data-stu-id="6c93f-147">Key Password:</span></span>

`Enter passphrase (empty for no passphrase):`

<span data-ttu-id="6c93f-148">`ssh-keygen` kallar ett lösenord för den privata nyckeln "en lösenfras".</span><span class="sxs-lookup"><span data-stu-id="6c93f-148">`ssh-keygen` refers to a password for the private key file as "a passphrase."</span></span>  <span data-ttu-id="6c93f-149">Vi rekommenderar *starkt* att du skapar ett lösenord för den privata nyckeln.</span><span class="sxs-lookup"><span data-stu-id="6c93f-149">It is *strongly* recommended to add a password to your private key.</span></span> <span data-ttu-id="6c93f-150">Utan ett lösenord som skyddar nyckelfilen kan vem som helst som har tillgång till den logga in på en server som har motsvarande offentliga nyckel.</span><span class="sxs-lookup"><span data-stu-id="6c93f-150">Without a password protecting the key file, anyone with the file can use it to log in to any server that has the corresponding public key.</span></span> <span data-ttu-id="6c93f-151">Genom att lägga till ett lösenord (lösenfras) har du därför bättre skydd om någon lyckas få åtkomst till filen med din privata nyckel, så att du har tid att ändra nycklarna som används för att autentisera dig.</span><span class="sxs-lookup"><span data-stu-id="6c93f-151">Adding a password (passphrase) offers more protection in case someone is able to gain access to your private key file, given you time to change the keys used to authenticate you.</span></span>

## <a name="using-ssh-agent-to-store-your-private-key-password"></a><span data-ttu-id="6c93f-152">Lagra lösenordet för din privata nyckel med hjälp av ssh-agent</span><span class="sxs-lookup"><span data-stu-id="6c93f-152">Using ssh-agent to store your private key password</span></span>

<span data-ttu-id="6c93f-153">Om du inte vill skriva lösenordet för filen med den privata nyckeln vid varje SSH-inloggning kan du cachelagra lösenordet med hjälp av `ssh-agent`.</span><span class="sxs-lookup"><span data-stu-id="6c93f-153">To avoid typing your private key file password with every SSH login, you can use `ssh-agent` to cache your private key file password.</span></span> <span data-ttu-id="6c93f-154">Om du använder en Mac skyddas lösenorden för privata nycklar i OSX-nyckelringen när du anropar `ssh-agent`.</span><span class="sxs-lookup"><span data-stu-id="6c93f-154">If you are using a Mac, the OSX Keychain securely stores the private key passwords when you invoke `ssh-agent`.</span></span>

<span data-ttu-id="6c93f-155">Kontrollera och använd ssh-agent och ssh-add till för att informera SSH-systemet om viktiga filer så att lösenfrasen inte behöver användas interaktivt.</span><span class="sxs-lookup"><span data-stu-id="6c93f-155">Verify and use ssh-agent and ssh-add to inform the SSH system about the key files so that the passphrase will not need to be used interactively.</span></span>

```bash
eval "$(ssh-agent -s)"
```

<span data-ttu-id="6c93f-156">Lägg nu till den privata nyckeln i `ssh-agent` med hjälp av kommandot `ssh-add`.</span><span class="sxs-lookup"><span data-stu-id="6c93f-156">Now add the private key to `ssh-agent` using the command `ssh-add`.</span></span>

```bash
ssh-add ~/.ssh/id_rsa
```

<span data-ttu-id="6c93f-157">Nu lagras lösenordet för den privata nyckeln i `ssh-agent`.</span><span class="sxs-lookup"><span data-stu-id="6c93f-157">The private key password is now stored in `ssh-agent`.</span></span>

## <a name="using-ssh-copy-id-to-copy-the-key-to-an-existing-vm"></a><span data-ttu-id="6c93f-158">Använda `ssh-copy-id` för att kopiera nyckeln till en befintlig virtuell dator</span><span class="sxs-lookup"><span data-stu-id="6c93f-158">Using `ssh-copy-id` to copy the key to an existing VM</span></span>
<span data-ttu-id="6c93f-159">Om du redan har skapat en virtuell dator kan du installera den nya offentliga nyckeln till din virtuella Linux-dator med:</span><span class="sxs-lookup"><span data-stu-id="6c93f-159">If you have already created a VM you can install the new SSH public key to your Linux VM with:</span></span>

```bash
ssh-copy-id -i ~/.ssh/id_rsa.pub ahmet@myserver
```

## <a name="create-and-configure-an-ssh-config-file"></a><span data-ttu-id="6c93f-160">Skapa och konfigurera en SSH-konfigurationsfil</span><span class="sxs-lookup"><span data-stu-id="6c93f-160">Create and configure an SSH config file</span></span>

<span data-ttu-id="6c93f-161">Som bästa praxis bör du skapa och konfigurera en `~/.ssh/config`-fil för att påskynda inloggningar och för att optimera SSH-klientbeteendet.</span><span class="sxs-lookup"><span data-stu-id="6c93f-161">It is a recommended best practice to create and configure an `~/.ssh/config` file to speed up log ins and for optimizing your SSH client behavior.</span></span>

<span data-ttu-id="6c93f-162">Följande exempel illustrerar en standardkonfiguration.</span><span class="sxs-lookup"><span data-stu-id="6c93f-162">The following example shows a standard configuration.</span></span>

### <a name="create-the-file"></a><span data-ttu-id="6c93f-163">Skapa filen</span><span class="sxs-lookup"><span data-stu-id="6c93f-163">Create the file</span></span>

```bash
touch ~/.ssh/config
```

### <a name="edit-the-file-to-add-the-new-ssh-configuration"></a><span data-ttu-id="6c93f-164">Redigera filen för att lägga till den nya SSH-konfigurationen:</span><span class="sxs-lookup"><span data-stu-id="6c93f-164">Edit the file to add the new SSH configuration:</span></span>

```bash
vim ~/.ssh/config
```

### <a name="example-sshconfig-file"></a><span data-ttu-id="6c93f-165">`~/.ssh/config`-exempelfil:</span><span class="sxs-lookup"><span data-stu-id="6c93f-165">Example `~/.ssh/config` file:</span></span>

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

<span data-ttu-id="6c93f-166">Den här SSH-konfigurationsfilen innehåller avsnitt för varje server så att var och en kan ha ett eget dedikerat nyckelpar.</span><span class="sxs-lookup"><span data-stu-id="6c93f-166">This SSH config gives you sections for each server to enable each to have its own dedicated key pair.</span></span> <span data-ttu-id="6c93f-167">Standardinställningarna (`Host *`) är avsedda för värdar som inte matchar någon av de specifika värdarna högre upp i konfigurationsfilen.</span><span class="sxs-lookup"><span data-stu-id="6c93f-167">The default settings (`Host *`) are for any hosts that do not match any of the specific hosts higher up in the config file.</span></span>

### <a name="config-file-explained"></a><span data-ttu-id="6c93f-168">Konfigurationsfilen förklarad</span><span class="sxs-lookup"><span data-stu-id="6c93f-168">Config file explained</span></span>

<span data-ttu-id="6c93f-169">`Host` = namnet på värddatorn som anropas på terminalen.</span><span class="sxs-lookup"><span data-stu-id="6c93f-169">`Host` = the name of the host being called on the terminal.</span></span>  <span data-ttu-id="6c93f-170">`ssh fedora22` beordrar `SSH` att använda värdena i inställningsblocket med etiketten  `Host fedora22` OBS! Värden kan vara vilken etikett som helst som är logisk för din användning och som inte representerar själva värdnamnet för en server.</span><span class="sxs-lookup"><span data-stu-id="6c93f-170">`ssh fedora22` tells `SSH` to use the values in the settings block labeled `Host fedora22`  NOTE: Host can be any label that is logical for your usage and does not represent the actual hostname of any server.</span></span>

<span data-ttu-id="6c93f-171">`Hostname 102.160.203.241` = IP-adressen eller DNS-namnet för servern som du ansluter till.</span><span class="sxs-lookup"><span data-stu-id="6c93f-171">`Hostname 102.160.203.241` = the IP address or DNS name for the server being accessed.</span></span>

<span data-ttu-id="6c93f-172">`User ahmet` = fjärranvändarkontot som ska användas när du loggar in på servern.</span><span class="sxs-lookup"><span data-stu-id="6c93f-172">`User ahmet` = the remote user account to use when logging in to the server.</span></span>

<span data-ttu-id="6c93f-173">`PubKeyAuthentication yes` = meddelar SSH att du vill använda en SSH-nyckel för att logga in.</span><span class="sxs-lookup"><span data-stu-id="6c93f-173">`PubKeyAuthentication yes` = tells SSH you want to use an SSH key to log in.</span></span>

<span data-ttu-id="6c93f-174">`IdentityFile /home/ahmet/.ssh/id_id_rsa` = den privata SSH-nyckeln och motsvarande offentliga nyckel som ska användas för autentisering.</span><span class="sxs-lookup"><span data-stu-id="6c93f-174">`IdentityFile /home/ahmet/.ssh/id_id_rsa` = the SSH private key and corresponding public key to use for authentication.</span></span>

## <a name="ssh-into-linux-without-a-password"></a><span data-ttu-id="6c93f-175">SSH i Linux utan lösenord</span><span class="sxs-lookup"><span data-stu-id="6c93f-175">SSH into Linux without a password</span></span>

<span data-ttu-id="6c93f-176">Nu när du har ett SSH-nyckelpar och en konfigurerad SSH-konfigurationsfil kan du snabbt och säkert logga in på din virtuella Linux-dator.</span><span class="sxs-lookup"><span data-stu-id="6c93f-176">Now that you have an SSH key pair and a configured SSH config file, you are able to log in to your Linux VM quickly and securely.</span></span> <span data-ttu-id="6c93f-177">Första gången du loggar in till en server med en SSH-nyckel frågar kommandot efter lösenfrasen för nyckelfilen.</span><span class="sxs-lookup"><span data-stu-id="6c93f-177">The first time you log in to a server using an SSH key the command prompts you for the passphrase for that key file.</span></span>

```bash
ssh fedora22
```

### <a name="command-explained"></a><span data-ttu-id="6c93f-178">Kommandot förklarat</span><span class="sxs-lookup"><span data-stu-id="6c93f-178">Command explained</span></span>

<span data-ttu-id="6c93f-179">När `ssh fedora22` körs letar SSH först upp och läser in inställningarna från `Host fedora22`-blocket och läser sedan in de återstående inställningarna från det sista blocket, `Host *`.</span><span class="sxs-lookup"><span data-stu-id="6c93f-179">When `ssh fedora22` is executed SSH first locates and loads any settings from the `Host fedora22` block, and then loads all the remaining settings from the last block, `Host *`.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6c93f-180">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6c93f-180">Next Steps</span></span>

<span data-ttu-id="6c93f-181">Nästa uppgift är att skapa virtuella Azure Linux-datorer med den nya offentliga SSH-nyckeln.</span><span class="sxs-lookup"><span data-stu-id="6c93f-181">Next up is to create Azure Linux VMs using the new SSH public key.</span></span>  <span data-ttu-id="6c93f-182">Virtuella Azure-datorer som skapas med en offentlig SSH-nyckel för inloggning är bättre skyddade än virtuella datorer som skapas med vanliga lösenordsbaserade inloggningsmetoder.</span><span class="sxs-lookup"><span data-stu-id="6c93f-182">Azure VMs that are created with an SSH public key as the login are better secured than VMs created with the default login method, passwords.</span></span>  <span data-ttu-id="6c93f-183">Lösenord är som standard inaktiverade för Virtuella Azure-datorer som skapas med SSH-nycklar för att undvika råstyrkeattacker som försöker gissa lösenord.</span><span class="sxs-lookup"><span data-stu-id="6c93f-183">Azure VMs created using SSH keys are by default configured with passwords disabled, avoiding brute-forced guessing attempts.</span></span>

* [<span data-ttu-id="6c93f-184">Skapa en säker virtuell Linux-dator med hjälp av en Azure-mall</span><span class="sxs-lookup"><span data-stu-id="6c93f-184">Create a secure Linux VM using an Azure template</span></span>](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="6c93f-185">Skapa en säker virtuell Linux-dator med hjälp av Azure Portal</span><span class="sxs-lookup"><span data-stu-id="6c93f-185">Create a secure Linux VM using the Azure portal</span></span>](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="6c93f-186">Skapa en säker virtuell Linux-dator med hjälp av Azure CLI</span><span class="sxs-lookup"><span data-stu-id="6c93f-186">Create a secure Linux VM using the Azure CLI</span></span>](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
