---
title: "aaaCreate en SSH-nyckelpar för Linux virtuella datorer i Azure | Microsoft Docs"
description: "Skapa säkert ett SSH-nyckelpar med en offentlig och en privat nyckel för virtuella Linux-datorer i Azure."
services: virtual-machines-linux
documentationcenter: 
author: vlivech
manager: timlt
editor: 
tags: 
ms.assetid: 34ae9482-da3e-4b2d-9d0d-9d672aa42498
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/08/2017
ms.author: rasquill
ms.openlocfilehash: c4c7cec77c9b48295f2a28c8179b30a4dc38a555
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-ssh-public-and-private-key-pair-for-linux-vms"></a><span data-ttu-id="8f8ae-103">Skapa ett SSH-nyckelpar med en offentlig och en privat nyckel för virtuella Linux-datorer</span><span class="sxs-lookup"><span data-stu-id="8f8ae-103">Create an SSH public and private key pair for Linux VMs</span></span>

<span data-ttu-id="8f8ae-104">Den här artikeln visar hur toogenerate en SSH-protokollet version 2 RSA offentliga och privata nyckeln filen toouse par med virtuella Linux-datorer.</span><span class="sxs-lookup"><span data-stu-id="8f8ae-104">This article shows you how toogenerate an SSH protocol version 2 RSA public and private key file pair toouse with Linux VMs.</span></span>  <span data-ttu-id="8f8ae-105">Du kan använda en SSH-nyckel för att skapa virtuella datorer på Azure som standard toousing SSH-nycklar för autentisering, vilket eliminerar hello behovet av att lösenord toolog i.</span><span class="sxs-lookup"><span data-stu-id="8f8ae-105">With an SSH key pair, you can create Virtual Machines on Azure that default toousing SSH keys for authentication, eliminating hello need for passwords toolog in.</span></span>  <span data-ttu-id="8f8ae-106">Lösenord kan knäckas och öppna dina virtuella datorer in toorelentless brute force försök tooguess ditt lösenord.</span><span class="sxs-lookup"><span data-stu-id="8f8ae-106">Passwords can be guessed, and open your VMs up toorelentless brute force attempts tooguess your password.</span></span> <span data-ttu-id="8f8ae-107">Virtuella datorer som skapats med Azure-mallar eller hello `azure-cli` kan innehålla offentliga SSH-nyckeln som en del av hello-distribution, ta bort post konfigurationssteg för distribution av att inaktivera Lösenordsinloggning för SSH.</span><span class="sxs-lookup"><span data-stu-id="8f8ae-107">VMs created with Azure Templates or hello `azure-cli` can include your SSH public key as part of hello deployment, removing a post deployment configuration step of disabling password logins for SSH.</span></span>

## <a name="quick-commands"></a><span data-ttu-id="8f8ae-108">Snabbkommandon</span><span class="sxs-lookup"><span data-stu-id="8f8ae-108">Quick Commands</span></span>

<span data-ttu-id="8f8ae-109">Kör hello följande kommandon från ett Bash-gränssnitt, ersätta hello exempel med ditt eget val.</span><span class="sxs-lookup"><span data-stu-id="8f8ae-109">Run hello following commands from a Bash shell, replacing hello examples with your own choices.</span></span>

<span data-ttu-id="8f8ae-110">Den offentliga SSH-nyckelfilen skapas som standard på `~/.ssh/id_rsa.pub`.</span><span class="sxs-lookup"><span data-stu-id="8f8ae-110">Your SSH public key file is by default created at `~/.ssh/id_rsa.pub`.</span></span> <span data-ttu-id="8f8ae-111">När du uppmanas med hjälp av följande kommando hello, bör du skapa en ”lösenfras” toosecure din privata nyckel.</span><span class="sxs-lookup"><span data-stu-id="8f8ae-111">When prompted using hello following command, you should create a "passphrase" toosecure your private key.</span></span> <span data-ttu-id="8f8ae-112">(hello lösenfras är ett lösenord används tooencrypt din privata nyckel.)</span><span class="sxs-lookup"><span data-stu-id="8f8ae-112">(hello passphrase is a password used tooencrypt your private key.)</span></span>

```bash
ssh-keygen -t rsa -b 2048 
```

<span data-ttu-id="8f8ae-113">Lägg till hello nyskapad nyckel för`ssh-agent`:</span><span class="sxs-lookup"><span data-stu-id="8f8ae-113">Add hello newly created key too`ssh-agent`:</span></span>

```bash
ssh-add ~/.ssh/id_rsa
```

> [!IMPORTANT] 
> <span data-ttu-id="8f8ae-114">Hej ovan kan användas på Linux-operativsystem för nästan alla distributioner, men fungerar inte nödvändigtvis i behållare, som kan vara begränsad radikalt hello miljö.</span><span class="sxs-lookup"><span data-stu-id="8f8ae-114">hello above commands work on Linux operating systems of almost all distributions, but do not necessarily work in containers, as hello environment can be radically constrained.</span></span> 

## <a name="detailed-walkthrough"></a><span data-ttu-id="8f8ae-115">Detaljerad genomgång</span><span class="sxs-lookup"><span data-stu-id="8f8ae-115">Detailed Walkthrough</span></span>

<span data-ttu-id="8f8ae-116">Använda SSH offentliga och privata nycklar är hello enklaste sättet toolog i tooyour Linux-servrar.</span><span class="sxs-lookup"><span data-stu-id="8f8ae-116">Using SSH public and private keys is hello easiest way toolog in tooyour Linux servers.</span></span> <span data-ttu-id="8f8ae-117">[Offentliga nycklar](https://en.wikipedia.org/wiki/Public-key_cryptography) ger en mycket säkrare sätt toolog i tooyour Linux eller BSD VM i Azure än med lösenord, som kan vara nyckelsökningsattacker mycket enklare.</span><span class="sxs-lookup"><span data-stu-id="8f8ae-117">[Public-key cryptography](https://en.wikipedia.org/wiki/Public-key_cryptography) provides a much more secure way toolog in tooyour Linux or BSD VM in Azure than passwords, which can be brute-forced far more easily.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="8f8ae-118">Din offentliga nyckel kan delas med alla, men bara du (eller din lokala säkerhetsinfrastruktur) har tillgång till den privata nyckeln.</span><span class="sxs-lookup"><span data-stu-id="8f8ae-118">Your public key can be shared with anyone; but only you (or your local security infrastructure) possess your private key.</span></span>  <span data-ttu-id="8f8ae-119">hello SSH privat nyckel ska ha en [mycket säkert lösenord](https://www.xkcd.com/936/) (källa:[xkcd.com](https://xkcd.com)) toosafeguard den.</span><span class="sxs-lookup"><span data-stu-id="8f8ae-119">hello SSH private key should have a [very secure password](https://www.xkcd.com/936/) (source:[xkcd.com](https://xkcd.com)) toosafeguard it.</span></span>  <span data-ttu-id="8f8ae-120">Lösenordet är bara tooaccess hello privata SSH-nyckeln och **är inte** hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="8f8ae-120">This password is just tooaccess hello private SSH key and **is not** hello user account password.</span></span>  <span data-ttu-id="8f8ae-121">När du lägger till ett lösenord tooyour SSH-nyckeln krypteras hello privata nyckeln med 128-bitars AES, så att hello privata nyckel är oanvändbar utan hello lösenord toodecrypt den.</span><span class="sxs-lookup"><span data-stu-id="8f8ae-121">When you add a password tooyour SSH key, it encrypts hello private key using 128-bit AES, so that hello private key is useless without hello password toodecrypt it.</span></span>  <span data-ttu-id="8f8ae-122">Om en angripare stal din privata nyckel och att nyckeln har inte ett lösenord, de skulle vara kan toouse som privat nyckel toolog i tooany servrar som har hello motsvarande offentliga nyckel.</span><span class="sxs-lookup"><span data-stu-id="8f8ae-122">If an attacker stole your private key and that key did not have a password, they would be able toouse that private key toolog in tooany servers that have hello corresponding public key.</span></span>  <span data-ttu-id="8f8ae-123">Om den privata nyckeln är lösenordsskyddad kan den inte användas av angriparen, vilket ger infrastrukturen i Azure ett extra säkerhetslager.</span><span class="sxs-lookup"><span data-stu-id="8f8ae-123">If a private key is password protected it cannot be used by that attacker, providing an additional layer of security for your infrastructure on Azure.</span></span>

<span data-ttu-id="8f8ae-124">Den här artikeln skapar SSH-protokollversion 2 RSA offentliga och privata nyckelfiler som rekommenderas för distributioner på hello Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="8f8ae-124">This article creates SSH protocol version 2 RSA public and private key files, which are recommended for deployments on hello Resource Manager.</span></span>  <span data-ttu-id="8f8ae-125">*SSH-rsa* nycklar krävs på hello [portal](https://portal.azure.com) för både klassiska och Resource Manager distributioner.</span><span class="sxs-lookup"><span data-stu-id="8f8ae-125">*ssh-rsa* keys are required on hello [portal](https://portal.azure.com) for both classic and Resource Manager deployments.</span></span>

## <a name="disable-ssh-passwords-by-using-ssh-keys"></a><span data-ttu-id="8f8ae-126">Inaktivera SSH-lösenord genom att använda SSH-nycklar</span><span class="sxs-lookup"><span data-stu-id="8f8ae-126">Disable SSH passwords by using SSH keys</span></span>

<span data-ttu-id="8f8ae-127">Azure kräver offentliga och privata nycklar med minst 2048 bitar i ssh-rsa-format.</span><span class="sxs-lookup"><span data-stu-id="8f8ae-127">Azure requires at least 2048-bit, ssh-rsa format public and private keys.</span></span> <span data-ttu-id="8f8ae-128">toocreate hello nycklar används `ssh-keygen`, som ställer ett antal frågor och sedan skriver en privat nyckel och en matchande offentlig nyckel.</span><span class="sxs-lookup"><span data-stu-id="8f8ae-128">toocreate hello keys use `ssh-keygen`, which asks a series of questions and then writes a private key and a matching public key.</span></span> <span data-ttu-id="8f8ae-129">När en virtuell dator i Azure skapas kopieras hello offentlig nyckel för`~/.ssh/authorized_keys`.</span><span class="sxs-lookup"><span data-stu-id="8f8ae-129">When an Azure VM is created, hello public key is copied too`~/.ssh/authorized_keys`.</span></span>  <span data-ttu-id="8f8ae-130">SSH-nycklar i `~/.ssh/authorized_keys` är används toochallenge hello klienten toomatch hello motsvarande privata nyckel på en SSH-anslutning för inloggning.</span><span class="sxs-lookup"><span data-stu-id="8f8ae-130">SSH keys in `~/.ssh/authorized_keys` are used toochallenge hello client toomatch hello corresponding private key on an SSH login connection.</span></span>  <span data-ttu-id="8f8ae-131">När en Azure Linux VM skapas med hjälp av SSH-nycklar för autentisering, Azure konfigurerar hello SSHD server toonot Tillåt Lösenordsinloggning SSH-nycklar.</span><span class="sxs-lookup"><span data-stu-id="8f8ae-131">When an Azure Linux VM is created using SSH keys for authentication, Azure configures hello SSHD server toonot allow password logins, only SSH keys.</span></span>  <span data-ttu-id="8f8ae-132">Genom att skapa virtuella Azure Linux-datorer med SSH-nycklar du därför att säkra hello distribution av Virtuella datorer och spara hello vanliga efter distributionen konfigurationssteget inaktivera lösenord i hello sshd_config config-fil.</span><span class="sxs-lookup"><span data-stu-id="8f8ae-132">Therefore, by creating Azure Linux VMs with SSH keys, you can help secure hello VM deployment and save yourself hello typical post-deployment configuration step of disabling passwords in hello sshd_config config file.</span></span>

## <a name="using-ssh-keygen"></a><span data-ttu-id="8f8ae-133">Använda ssh-keygen</span><span class="sxs-lookup"><span data-stu-id="8f8ae-133">Using ssh-keygen</span></span>

<span data-ttu-id="8f8ae-134">Det här kommandot skapas ett lösenordsskyddat (krypterat) SSH-nyckel som använder RSA 2048-bitars och den har kommenterats tooeasily identifiera den.</span><span class="sxs-lookup"><span data-stu-id="8f8ae-134">This command creates a password secured (encrypted) SSH key pair using 2048-bit RSA and it is commented tooeasily identify it.</span></span>  

<span data-ttu-id="8f8ae-135">SSH nycklar är som standard sparas i hello `~/.ssh` directory.</span><span class="sxs-lookup"><span data-stu-id="8f8ae-135">SSH keys are by default kept in hello `~/.ssh` directory.</span></span>  <span data-ttu-id="8f8ae-136">Om du inte har en `~/.ssh` directory, hello `ssh-keygen` kommando skapar den du med hello rätt behörigheter.</span><span class="sxs-lookup"><span data-stu-id="8f8ae-136">If you do not have a `~/.ssh` directory, hello `ssh-keygen` command creates it for you with hello correct permissions.</span></span>

```bash
ssh-keygen \
-t rsa \
-b 2048 
```

<span data-ttu-id="8f8ae-137">*Kommandot förklarat*</span><span class="sxs-lookup"><span data-stu-id="8f8ae-137">*Command explained*</span></span>

<span data-ttu-id="8f8ae-138">`ssh-keygen`= hello används toocreate hello nycklar</span><span class="sxs-lookup"><span data-stu-id="8f8ae-138">`ssh-keygen` = hello program used toocreate hello keys</span></span>

<span data-ttu-id="8f8ae-139">`-t rsa`typ av nyckel toocreate som är hello RSA format = [wikipedia](https://en.wikipedia.org/wiki/RSA_(cryptosystem)</span><span class="sxs-lookup"><span data-stu-id="8f8ae-139">`-t rsa` = type of key toocreate which is hello RSA format [wikipedia](https://en.wikipedia.org/wiki/RSA_(cryptosystem)</span></span>

<span data-ttu-id="8f8ae-140">`-b 2048`= bitar i hello nyckel</span><span class="sxs-lookup"><span data-stu-id="8f8ae-140">`-b 2048` = bits of hello key</span></span>


## <a name="classic-portal-and-x509-certs"></a><span data-ttu-id="8f8ae-141">Klassisk portal och X.509-certifikat</span><span class="sxs-lookup"><span data-stu-id="8f8ae-141">Classic portal and X.509 certs</span></span>

<span data-ttu-id="8f8ae-142">Om du använder hello Azure [klassiska portalen](https://manage.windowsazure.com/), X.509-certifikat krävs för hello SSH-nycklar.</span><span class="sxs-lookup"><span data-stu-id="8f8ae-142">If you are using hello Azure [classic portal](https://manage.windowsazure.com/), it requires X.509 certs for hello SSH keys.</span></span>  <span data-ttu-id="8f8ae-143">Inga andra typer av offentliga SSH-nycklar tillåts, de *måste* vara X.509-certifikat.</span><span class="sxs-lookup"><span data-stu-id="8f8ae-143">No other types of SSH public keys are allowed, they *must* be X.509 certs.</span></span>

<span data-ttu-id="8f8ae-144">toocreate ett X.509-certifikat från din befintliga privata SSH-RSA-nyckel:</span><span class="sxs-lookup"><span data-stu-id="8f8ae-144">toocreate an X.509 cert from your existing SSH-RSA private key:</span></span>

```bash
openssl req -x509 \
-key ~/.ssh/id_rsa \
-nodes \
-days 365 \
-newkey rsa:2048 \
-out ~/.ssh/id_rsa.pem
```

## <a name="classic-deploy-using-asm"></a><span data-ttu-id="8f8ae-145">Klassisk distribution med hjälp av `asm`</span><span class="sxs-lookup"><span data-stu-id="8f8ae-145">Classic deploy using `asm`</span></span>

<span data-ttu-id="8f8ae-146">Om du använder hello klassisk distribuera modellen (Azure service management CLI `asm`), du kan använda en offentlig SSH-RSA-nyckel eller en RFC4716 formaterad nyckel i en **.pem** behållare.</span><span class="sxs-lookup"><span data-stu-id="8f8ae-146">If you are using hello classic deploy model (Azure service management CLI `asm`), you can use an SSH-RSA public key or an RFC4716 formatted key in a **.pem** container.</span></span>  <span data-ttu-id="8f8ae-147">hello SSH-RSA-offentliga nyckel är vad som skapats tidigare i den här artikeln använder `ssh-keygen`.</span><span class="sxs-lookup"><span data-stu-id="8f8ae-147">hello SSH-RSA public key is what was created earlier in this article using `ssh-keygen`.</span></span>

<span data-ttu-id="8f8ae-148">toocreate en RFC4716 formaterad nyckeln från en befintlig offentlig SSH-nyckel:</span><span class="sxs-lookup"><span data-stu-id="8f8ae-148">toocreate a RFC4716 formatted key from an existing SSH public key:</span></span>

```bash
ssh-keygen \
-f ~/.ssh/id_rsa.pub \
-e \
-m RFC4716 > ~/.ssh/id_ssh2.pem
```

## <a name="example-of-ssh-keygen"></a><span data-ttu-id="8f8ae-149">Exempel på ssh-keygen</span><span class="sxs-lookup"><span data-stu-id="8f8ae-149">Example of ssh-keygen</span></span>

```bash
ssh-keygen -t rsa -b 2048 -C "ahmet@myserver"
Generating public/private rsa key pair.
Enter file in which toosave hello key (/home/ahmet/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/ahmet/.ssh/id_rsa.
Your public key has been saved in /home/ahmet/.ssh/id_rsa.pub.
hello key fingerprint is:
14:a3:cb:3e:78:ad:25:cc:55:e9:0c:08:e5:d1:a9:08 ahmet@myserver
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

<span data-ttu-id="8f8ae-150">Sparade nyckelfiler:</span><span class="sxs-lookup"><span data-stu-id="8f8ae-150">Saved key files:</span></span>

`Enter file in which toosave hello key (/home/ahmet/.ssh/id_rsa): ~/.ssh/id_rsa`

<span data-ttu-id="8f8ae-151">hello nyckelpar namn för den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="8f8ae-151">hello key pair name for this article.</span></span>  <span data-ttu-id="8f8ae-152">Ett nyckelpar med namnet **id_rsa** är hello standard och vissa verktyg kan förvänta sig hello **id_rsa** privata nyckel filnamn så är det en bra idé.</span><span class="sxs-lookup"><span data-stu-id="8f8ae-152">Having a key pair named **id_rsa** is hello default and some tools might expect hello **id_rsa** private key file name so having one is a good idea.</span></span> <span data-ttu-id="8f8ae-153">hello directory `~/.ssh/` är hello standardplatsen för SSH-nyckelpar och hello SSH-konfigurationsfil.</span><span class="sxs-lookup"><span data-stu-id="8f8ae-153">hello directory `~/.ssh/` is hello default location for SSH key pairs and hello SSH config file.</span></span>  <span data-ttu-id="8f8ae-154">Om den inte anges med en fullständig sökväg `ssh-keygen` ska skapa hello nycklar i hello aktuella arbetskatalogen, inte hello standard `~/.ssh`.</span><span class="sxs-lookup"><span data-stu-id="8f8ae-154">If not specified with a full path, `ssh-keygen` will create hello keys in hello current working directory, not hello default `~/.ssh`.</span></span>

<span data-ttu-id="8f8ae-155">En lista över hello `~/.ssh` directory.</span><span class="sxs-lookup"><span data-stu-id="8f8ae-155">A listing of hello `~/.ssh` directory.</span></span>

```bash
ls -al ~/.ssh
-rw------- 1 ahmet staff  1675 Aug 25 18:04 id_rsa
-rw-r--r-- 1 ahmet staff   410 Aug 25 18:04 rsa.pub
```

<span data-ttu-id="8f8ae-156">Nyckellösenord:</span><span class="sxs-lookup"><span data-stu-id="8f8ae-156">Key Password:</span></span>

`Enter passphrase (empty for no passphrase):`

<span data-ttu-id="8f8ae-157">`ssh-keygen`refererar tooa lösenord som används för tooencrypt hello privat nyckel som ”en lösenfras”.</span><span class="sxs-lookup"><span data-stu-id="8f8ae-157">`ssh-keygen` refers tooa password used tooencrypt hello private key as "a passphrase."</span></span>  <span data-ttu-id="8f8ae-158">Det är *starkt* rekommenderas tooadd en lösenfras tooyour nyckelpar.</span><span class="sxs-lookup"><span data-stu-id="8f8ae-158">It is *strongly* recommended tooadd a passphrase tooyour key pairs.</span></span> <span data-ttu-id="8f8ae-159">Utan en lösenfras skyddar hello privata nyckel, kan alla med hello nyckelfilen använda den toolog i tooany-server som har hello motsvarande offentlig nyckel.</span><span class="sxs-lookup"><span data-stu-id="8f8ae-159">Without a passphrase protecting hello private key, anyone with hello key file can use it toolog in tooany server that has hello corresponding public key.</span></span> <span data-ttu-id="8f8ae-160">Lägga till en lösenfras erbjuder bättre skydd om någon kan toogain åtkomst tooyour fil för privat nyckel, vilket ger dig tid toochange hello nycklar används tooauthenticate du.</span><span class="sxs-lookup"><span data-stu-id="8f8ae-160">Adding a passphrase offers more protection in case someone is able toogain access tooyour private key file, giving you time toochange hello keys used tooauthenticate you.</span></span>

## <a name="using-ssh-agent-toostore-your-private-key-password"></a><span data-ttu-id="8f8ae-161">Med hjälp av ssh-agent toostore lösenordet för privat nyckel</span><span class="sxs-lookup"><span data-stu-id="8f8ae-161">Using ssh-agent toostore your private key password</span></span>

<span data-ttu-id="8f8ae-162">tooavoid att skriva din privata nyckel filen lösenfras med varje SSH-inloggning kan du använda `ssh-agent` toocache lösenordet fil för privat nyckel.</span><span class="sxs-lookup"><span data-stu-id="8f8ae-162">tooavoid typing your private key file passphrase with every SSH login, you can use `ssh-agent` toocache your private key file password.</span></span> <span data-ttu-id="8f8ae-163">Om du använder en Mac hello OS x-nyckelringen på ett säkert sätt lagrar hello privata nycklar som lösenord när du anropar `ssh-agent`.</span><span class="sxs-lookup"><span data-stu-id="8f8ae-163">If you are using a Mac, hello OSX Keychain securely stores hello private key passwords when you invoke `ssh-agent`.</span></span>

<span data-ttu-id="8f8ae-164">Kontrollera och använda `ssh-agent` och `ssh-add` tooinform hello SSH system om hello viktiga filer så att hello lösenfras inte behöver toobe används interaktivt.</span><span class="sxs-lookup"><span data-stu-id="8f8ae-164">Verify and use `ssh-agent` and `ssh-add` tooinform hello SSH system about hello key files so that hello passphrase will not need toobe used interactively.</span></span>

```bash
eval "$(ssh-agent -s)"
```

<span data-ttu-id="8f8ae-165">Lägg nu till hello privata nyckeln för`ssh-agent` hello kommandot `ssh-add`.</span><span class="sxs-lookup"><span data-stu-id="8f8ae-165">Now add hello private key too`ssh-agent` using hello command `ssh-add`.</span></span>

```bash
ssh-add ~/.ssh/id_rsa
```

<span data-ttu-id="8f8ae-166">lösenordet för hello privata nyckeln lagras nu på `ssh-agent`.</span><span class="sxs-lookup"><span data-stu-id="8f8ae-166">hello private key password is now stored in `ssh-agent`.</span></span>

## <a name="using-ssh-copy-id-tooinstall-hello-new-key"></a><span data-ttu-id="8f8ae-167">Med hjälp av `ssh-copy-id` tooinstall hello ny nyckel</span><span class="sxs-lookup"><span data-stu-id="8f8ae-167">Using `ssh-copy-id` tooinstall hello new key</span></span>
<span data-ttu-id="8f8ae-168">Om du redan har skapat en virtuell dator kan du installera hello nya SSH offentlig nyckel tooyour Linux VM med hello följande kommando, ersätter hello VM användarnamn och hello serveradress med egna värden:</span><span class="sxs-lookup"><span data-stu-id="8f8ae-168">If you have already created a VM you can install hello new SSH public key tooyour Linux VM with hello following command, replacing hello VM username and hello server address with your own values:</span></span>

```bash
ssh-copy-id -i ~/.ssh/id_rsa.pub ahmet@myserver
```

## <a name="create-and-configure-an-ssh-config-file"></a><span data-ttu-id="8f8ae-169">Skapa och konfigurera en SSH-konfigurationsfil</span><span class="sxs-lookup"><span data-stu-id="8f8ae-169">Create and configure an SSH config file</span></span>

<span data-ttu-id="8f8ae-170">Det är en bästa praxis toocreate och konfigurera en `~/.ssh/config` filen toospeed upp inloggningar och för att optimera din beteende för SSH-klienten.</span><span class="sxs-lookup"><span data-stu-id="8f8ae-170">It is a best practice toocreate and configure an `~/.ssh/config` file toospeed up log ins and for optimizing your SSH client behavior.</span></span>

<span data-ttu-id="8f8ae-171">hello följande exempel illustrerar en standardkonfiguration.</span><span class="sxs-lookup"><span data-stu-id="8f8ae-171">hello following example shows a standard configuration.</span></span>

### <a name="create-hello-file"></a><span data-ttu-id="8f8ae-172">Skapa hello-fil</span><span class="sxs-lookup"><span data-stu-id="8f8ae-172">Create hello file</span></span>

```bash
touch ~/.ssh/config
```

### <a name="edit-hello-file-tooadd-hello-new-ssh-configuration"></a><span data-ttu-id="8f8ae-173">Redigera hello filen tooadd hello nya SSH-konfigurationen:</span><span class="sxs-lookup"><span data-stu-id="8f8ae-173">Edit hello file tooadd hello new SSH configuration:</span></span>

```bash
vim ~/.ssh/config
```

### <a name="example-sshconfig-file"></a><span data-ttu-id="8f8ae-174">`~/.ssh/config`-exempelfil:</span><span class="sxs-lookup"><span data-stu-id="8f8ae-174">Example `~/.ssh/config` file:</span></span>

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

<span data-ttu-id="8f8ae-175">Den här SSH config ger du avsnitten för varje server tooenable varje toohave eget dedikerade nyckelpar.</span><span class="sxs-lookup"><span data-stu-id="8f8ae-175">This SSH config gives you sections for each server tooenable each toohave its own dedicated key pair.</span></span> <span data-ttu-id="8f8ae-176">Hej standardinställningar (`Host *`) är för alla värdar som inte matchar någon av hello specifika värdar högre upp i hello config-fil.</span><span class="sxs-lookup"><span data-stu-id="8f8ae-176">hello default settings (`Host *`) are for any hosts that do not match any of hello specific hosts higher up in hello config file.</span></span>

### <a name="config-file-explained"></a><span data-ttu-id="8f8ae-177">Konfigurationsfilen förklarad</span><span class="sxs-lookup"><span data-stu-id="8f8ae-177">Config file explained</span></span>

<span data-ttu-id="8f8ae-178">`Host`= hello namnet på hello värddatorn som anropas på hello terminal.</span><span class="sxs-lookup"><span data-stu-id="8f8ae-178">`Host` = hello name of hello host being called on hello terminal.</span></span>  <span data-ttu-id="8f8ae-179">`ssh fedora22`Anger `SSH` toouse hello värden i hello inställningsblocket med etiketten `Host fedora22` Obs: värden kan vara vilken etikett som är logiska för din användning och som inte representerar hello faktiska värdnamnet för en server.</span><span class="sxs-lookup"><span data-stu-id="8f8ae-179">`ssh fedora22` tells `SSH` toouse hello values in hello settings block labeled `Host fedora22`  NOTE: Host can be any label that is logical for your usage and does not represent hello actual hostname of any server.</span></span>

<span data-ttu-id="8f8ae-180">`Hostname 102.160.203.241`= hello IP-adress eller DNS-namn för hello-server som används.</span><span class="sxs-lookup"><span data-stu-id="8f8ae-180">`Hostname 102.160.203.241` = hello IP address or DNS name for hello server being accessed.</span></span>

<span data-ttu-id="8f8ae-181">`User ahmet`= hello fjärranvändare konto toouse när du loggar in toohello server.</span><span class="sxs-lookup"><span data-stu-id="8f8ae-181">`User ahmet` = hello remote user account toouse when logging in toohello server.</span></span>

<span data-ttu-id="8f8ae-182">`PubKeyAuthentication yes`= meddelar SSH du vill ha toouse toolog en SSH-nyckel.</span><span class="sxs-lookup"><span data-stu-id="8f8ae-182">`PubKeyAuthentication yes` = tells SSH you want toouse an SSH key toolog in.</span></span>

<span data-ttu-id="8f8ae-183">`IdentityFile /home/ahmet/.ssh/id_id_rsa`= hello SSH privat nyckel och motsvarande toouse med offentlig nyckel för autentisering.</span><span class="sxs-lookup"><span data-stu-id="8f8ae-183">`IdentityFile /home/ahmet/.ssh/id_id_rsa` = hello SSH private key and corresponding public key toouse for authentication.</span></span>

## <a name="ssh-into-linux-without-a-password"></a><span data-ttu-id="8f8ae-184">SSH i Linux utan lösenord</span><span class="sxs-lookup"><span data-stu-id="8f8ae-184">SSH into Linux without a password</span></span>

<span data-ttu-id="8f8ae-185">Nu när du har en SSH-nyckel och en konfigurerad SSH-konfigurationsfil är kan toolog i tooyour Linux VM snabbt och säkert.</span><span class="sxs-lookup"><span data-stu-id="8f8ae-185">Now that you have an SSH key pair and a configured SSH config file, you are able toolog in tooyour Linux VM quickly and securely.</span></span> <span data-ttu-id="8f8ae-186">hello första gången du loggar i tooa servern med hjälp av en SSH-nyckel hello kommandotolkar du för hello lösenfras för denna nyckel.</span><span class="sxs-lookup"><span data-stu-id="8f8ae-186">hello first time you log in tooa server using an SSH key hello command prompts you for hello passphrase for that key file.</span></span>

```bash
ssh fedora22
```

### <a name="command-explained"></a><span data-ttu-id="8f8ae-187">Kommandot förklarat</span><span class="sxs-lookup"><span data-stu-id="8f8ae-187">Command explained</span></span>

<span data-ttu-id="8f8ae-188">När `ssh fedora22` körs SSH först upp och läser in inställningarna från hello `Host fedora22` blocket och läser sedan alla hello återstående inställningarna från hello sista blocket, `Host *`.</span><span class="sxs-lookup"><span data-stu-id="8f8ae-188">When `ssh fedora22` is executed SSH first locates and loads any settings from hello `Host fedora22` block, and then loads all hello remaining settings from hello last block, `Host *`.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8f8ae-189">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8f8ae-189">Next Steps</span></span>

<span data-ttu-id="8f8ae-190">Nästa in toocreate virtuella Azure Linux-datorer använder hello nya offentliga SSH-nyckeln.</span><span class="sxs-lookup"><span data-stu-id="8f8ae-190">Next up is toocreate Azure Linux VMs using hello new SSH public key.</span></span>  <span data-ttu-id="8f8ae-191">Virtuella Azure-datorer som har skapats med en offentlig SSH-nyckel som hello inloggning är bättre skyddade än virtuella datorer som skapats med hello inloggningen standardmetoden, lösenord.</span><span class="sxs-lookup"><span data-stu-id="8f8ae-191">Azure VMs that are created with an SSH public key as hello login are better secured than VMs created with hello default login method, passwords.</span></span>  <span data-ttu-id="8f8ae-192">Lösenord är som standard inaktiverade för Virtuella Azure-datorer som skapas med SSH-nycklar för att undvika råstyrkeattacker som försöker gissa lösenord.</span><span class="sxs-lookup"><span data-stu-id="8f8ae-192">Azure VMs created using SSH keys are by default configured with passwords disabled, avoiding brute-forced guessing attempts.</span></span> <span data-ttu-id="8f8ae-193">Om du behöver mer hjälp med att skapa SSH-nyckel eller kräver ytterligare certifikat, till exempel för användning med hello klassiska portalen finns [detaljerade steg toocreate SSH-nyckelpar och certifikat](create-ssh-keys-detailed.md).</span><span class="sxs-lookup"><span data-stu-id="8f8ae-193">If you need more assistance in creating your SSH key pair or require additional certificates, such as for use with hello classic portal, see [Detailed steps toocreate SSH key pairs and certificates](create-ssh-keys-detailed.md).</span></span>

* [<span data-ttu-id="8f8ae-194">Skapa en säker virtuell Linux-dator med hjälp av en Azure-mall</span><span class="sxs-lookup"><span data-stu-id="8f8ae-194">Create a secure Linux VM using an Azure template</span></span>](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="8f8ae-195">Skapa en säker Linux VM som använder hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="8f8ae-195">Create a secure Linux VM using hello Azure portal</span></span>](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="8f8ae-196">Skapa en säker Linux VM som använder hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="8f8ae-196">Create a secure Linux VM using hello Azure CLI</span></span>](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
