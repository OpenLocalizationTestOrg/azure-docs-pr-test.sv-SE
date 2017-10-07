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
# <a name="detailed-walk-through-toocreate-an-ssh-key-pair-and-additional-certificates-for-a-linux-vm-in-azure"></a>Detaljerad genomgång toocreate ett SSH-nyckelpar och ytterligare certifikat för en Linux-VM i Azure
Du kan använda en SSH-nyckel för att skapa virtuella datorer på Azure som standard toousing SSH-nycklar för autentisering, vilket eliminerar hello behovet av att lösenord toolog i. Lösenord kan knäckas och öppna dina virtuella datorer in toorelentless brute force försök tooguess ditt lösenord. Virtuella datorer som skapats med hello Azure CLI eller Resource Manager-mallar kan omfatta din offentliga SSH-nyckeln som en del av hello-distribution, ta bort post konfigurationssteg för distribution av att inaktivera Lösenordsinloggning för SSH. Den här artikeln innehåller detaljerade anvisningar och ytterligare exempel på genererar certifikat, såsom för användning med virtuella Linux-datorer. Om du vill tooquickly skapar och använder en SSH-nyckel, se [hur toocreate en SSH-offentliga och privata nyckel koppla för Linux virtuella datorer i Azure](mac-create-ssh-keys.md).

## <a name="understanding-ssh-keys"></a>Förstå SSH-nycklar

Använda SSH offentliga och privata nycklar är hello enklaste sättet toolog i tooyour Linux-servrar. [Offentliga nycklar](https://en.wikipedia.org/wiki/Public-key_cryptography) ger en mycket säkrare sätt toolog i tooyour Linux eller BSD VM i Azure än med lösenord, som kan vara nyckelsökningsattacker mycket enklare.

Din offentliga nyckel kan delas med alla, men bara du (eller din lokala säkerhetsinfrastruktur) har tillgång till den privata nyckeln.  hello SSH privat nyckel ska ha en [mycket säkert lösenord](https://www.xkcd.com/936/) (källa:[xkcd.com](https://xkcd.com)) toosafeguard den.  Lösenordet är bara tooaccess hello privata SSH-nyckelfilen och **är inte** hello lösenord.  När du lägger till ett lösenord tooyour SSH-nyckeln krypteras hello privata nyckeln med 128-bitars AES, så att hello privata nyckel är oanvändbar utan hello lösenord toodecrypt den.  Om en angripare stal din privata nyckel och att nyckeln har inte ett lösenord, de skulle vara kan toouse som privat nyckel toolog i tooany servrar som har hello motsvarande offentliga nyckel.  Om den privata nyckeln är lösenordsskyddad kan den inte användas av angriparen, vilket ger infrastrukturen i Azure ett extra säkerhetslager.

Den här artikeln skapar en SSH-protokollversion 2 RSA offentliga och privata nyckelfilen par (även hänvisade tooas ”ssh-rsa” nycklar), som rekommenderas för distributioner med Azure Resource Manager. *SSH-rsa* nycklar krävs på hello [portal](https://portal.azure.com) för både klassiska och Resource Manager distributioner.

## <a name="ssh-keys-use-and-benefits"></a>Användning och fördelar med SSH-nycklar

Azure kräver minst 2048-bitars SSH-protokollet version 2 RSA format offentliga och privata nycklar; hello offentlig nyckelfil har hello `.pub` behållares format. toocreate hello nycklar används `ssh-keygen`, som ställer ett antal frågor och sedan skriver en privat nyckel och en matchande offentlig nyckel. När en virtuell dator i Azure skapas Azure kopior hello offentliga nyckel toohello `~/.ssh/authorized_keys` mapp på hello VM. SSH-nycklar i `~/.ssh/authorized_keys` är används toochallenge hello klienten toomatch hello motsvarande privata nyckel på en SSH-anslutning för inloggning.  När en Azure Linux VM skapas med hjälp av SSH-nycklar för autentisering, Azure konfigurerar hello SSHD server toonot Tillåt Lösenordsinloggning SSH-nycklar.  Därför genom att skapa virtuella Azure Linux-datorer med SSH-nycklar kan du säkra hello VM-distribution och sparar hello vanliga efter distributionen konfigurationssteget inaktivera lösenord i hello **sshd_config** fil.

## <a name="using-ssh-keygen"></a>Använda ssh-keygen

Det här kommandot skapas ett lösenordsskyddat (krypterat) SSH-nyckel som använder RSA 2048-bitars och den har kommenterats tooeasily identifiera den.  

SSH nycklar är som standard sparas i hello `~/.ssh` directory.  Om du inte har en `~/.ssh` directory, hello `ssh-keygen` kommando skapar den du med hello rätt behörigheter.

```bash
ssh-keygen \
    -t rsa \
    -b 2048 \
    -C "azureuser@myserver" \
    -f ~/.ssh/id_rsa \
    -N mypassword
```

*Kommandot förklarat*

`ssh-keygen`= hello används toocreate hello nycklar

`-t rsa`= typen av nyckel toocreate som är hello RSA format [wikipedia][parens slutet](`https://en.wikipedia.org/wiki/RSA_(cryptosystem) `) 
 `-b 2048` = bitar i hello nyckel

`-C "azureuser@myserver"`= en kommentar som läggs toohello slutet av hello offentlig nyckelfil tooeasily identifiera den.  Normalt används ett e-postmeddelande som hello kommentar men du kan använda det som fungerar bäst för din infrastruktur.

## <a name="classic-deploy-using-asm"></a>Klassisk distribution med hjälp av `asm`

Om du använder hello klassiska distributionsmodellen (`asm` hello CLI-läge), du kan använda en offentlig SSH-RSA-nyckel eller en RFC4716 formaterad nyckel i en pem-behållare.  hello SSH-RSA-offentliga nyckel är vad som skapats tidigare i den här artikeln använder `ssh-keygen`.

toocreate en RFC4716 formaterad nyckeln från en befintlig offentlig SSH-nyckel:

```bash
ssh-keygen \
-f ~/.ssh/id_rsa.pub \
-e \
-m RFC4716 > ~/.ssh/id_ssh2.pem
```

## <a name="example-of-ssh-keygen"></a>Exempel på ssh-keygen

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

Sparade nyckelfiler:

`Enter file in which toosave hello key (/home/azureuser/.ssh/id_rsa): ~/.ssh/id_rsa`

hello nyckelpar namn för den här artikeln.  Ett nyckelpar med namnet **id_rsa** är hello standard och vissa verktyg kan förvänta sig hello **id_rsa** privata nyckel filnamn så är det en bra idé. hello directory `~/.ssh/` är hello standardplatsen för SSH-nyckelpar och hello SSH-konfigurationsfil.  Om den inte anges med en fullständig sökväg `ssh-keygen` skapar hello nycklar i hello aktuella arbetskatalogen, inte hello standard `~/.ssh`.

En lista över hello `~/.ssh` directory.

```bash
ls -al ~/.ssh
-rw------- 1 azureuser staff  1675 Aug 25 18:04 id_rsa
-rw-r--r-- 1 azureuser staff   410 Aug 25 18:04 id_rsa.pub
```

Nyckellösenord:

`Enter passphrase (empty for no passphrase):`

`ssh-keygen`refererar tooa lösenord för hello-fil för privat nyckel som ”en lösenfras”.  Det är *starkt* rekommenderas tooadd lösenord tooyour privat nyckel. Utan ett lösenord skyddar hello nyckelfilen, kan alla med hello-fil använda det toolog i tooany-server som har hello motsvarande offentlig nyckel. Lägga till ett lösenord (lösenfras) ger bättre skydd om någon kan toogain åtkomst tooyour fil för privat nyckel, gett dig tid toochange hello nycklar används tooauthenticate du.

## <a name="using-ssh-agent-toostore-your-private-key-password"></a>Med hjälp av ssh-agent toostore lösenordet för privat nyckel

tooavoid lösenordet fil för privat nyckel med varje SSH-inloggning kan du använda `ssh-agent` toocache lösenordet fil för privat nyckel. Om du använder en Mac hello OS x-nyckelringen på ett säkert sätt lagrar hello privata nycklar som lösenord när du anropar `ssh-agent`.

Kontrollera och använder ssh-agent och ssh-Lägg till tooinform hello SSH system om hello viktiga filer så att hello lösenfras inte behöver toobe används interaktivt.

```bash
eval "$(ssh-agent -s)"
```

Lägg nu till hello privata nyckeln för`ssh-agent` hello kommandot `ssh-add`.

```bash
ssh-add ~/.ssh/id_rsa
```

lösenordet för hello privata nyckeln lagras nu på `ssh-agent`.

## <a name="using-ssh-copy-id-toocopy-hello-key-tooan-existing-vm"></a>Med hjälp av `ssh-copy-id` toocopy hello viktiga tooan befintliga VM
Om du redan har skapat en virtuell dator kan du installera hello nya SSH offentlig nyckel tooyour Linux VM med:

```bash
ssh-copy-id -i ~/.ssh/id_rsa.pub ahmet@myserver
```

## <a name="create-and-configure-an-ssh-config-file"></a>Skapa och konfigurera en SSH-konfigurationsfil

Det är en rekommenderad bästa praxis toocreate och konfigurera en `~/.ssh/config` filen toospeed upp inloggningar och för att optimera din beteende för SSH-klienten.

hello följande exempel illustrerar en standardkonfiguration.

### <a name="create-hello-file"></a>Skapa hello-fil

```bash
touch ~/.ssh/config
```

### <a name="edit-hello-file-tooadd-hello-new-ssh-configuration"></a>Redigera hello filen tooadd hello nya SSH-konfigurationen:

```bash
vim ~/.ssh/config
```

### <a name="example-sshconfig-file"></a>`~/.ssh/config`-exempelfil:

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

Den här SSH config ger du avsnitten för varje server tooenable varje toohave eget dedikerade nyckelpar. Hej standardinställningar (`Host *`) är för alla värdar som inte matchar någon av hello specifika värdar högre upp i hello config-fil.

### <a name="config-file-explained"></a>Konfigurationsfilen förklarad

`Host`= hello namnet på hello värddatorn som anropas på hello terminal.  `ssh fedora22`Anger `SSH` toouse hello värden i hello inställningsblocket med etiketten `Host fedora22` Obs: värden kan vara vilken etikett som är logiska för din användning och som inte representerar hello faktiska värdnamnet för en server.

`Hostname 102.160.203.241`= hello IP-adress eller DNS-namn för hello-server som används.

`User ahmet`= hello fjärranvändare konto toouse när du loggar in toohello server.

`PubKeyAuthentication yes`= meddelar SSH du vill ha toouse toolog en SSH-nyckel.

`IdentityFile /home/ahmet/.ssh/id_id_rsa`= hello SSH privat nyckel och motsvarande toouse med offentlig nyckel för autentisering.

## <a name="ssh-into-linux-without-a-password"></a>SSH i Linux utan lösenord

Nu när du har en SSH-nyckel och en konfigurerad SSH-konfigurationsfil är kan toolog i tooyour Linux VM snabbt och säkert. hello första gången du loggar i tooa servern med hjälp av en SSH-nyckel hello kommandotolkar du för hello lösenfras för denna nyckel.

```bash
ssh fedora22
```

### <a name="command-explained"></a>Kommandot förklarat

När `ssh fedora22` körs SSH först upp och läser in inställningarna från hello `Host fedora22` blocket och läser sedan alla hello återstående inställningarna från hello sista blocket, `Host *`.

## <a name="next-steps"></a>Nästa steg

Nästa in toocreate virtuella Azure Linux-datorer använder hello nya offentliga SSH-nyckeln.  Virtuella Azure-datorer som har skapats med en offentlig SSH-nyckel som hello inloggning är bättre skyddade än virtuella datorer som skapats med hello inloggningen standardmetoden, lösenord.  Lösenord är som standard inaktiverade för Virtuella Azure-datorer som skapas med SSH-nycklar för att undvika råstyrkeattacker som försöker gissa lösenord.

* [Skapa en säker virtuell Linux-dator med hjälp av en Azure-mall](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Skapa en säker Linux VM som använder hello Azure-portalen](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Skapa en säker Linux VM som använder hello Azure CLI](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
