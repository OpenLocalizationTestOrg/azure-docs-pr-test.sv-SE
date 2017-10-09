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
# <a name="create-an-ssh-public-and-private-key-pair-for-linux-vms"></a>Skapa ett SSH-nyckelpar med en offentlig och en privat nyckel för virtuella Linux-datorer

Den här artikeln visar hur toogenerate en SSH-protokollet version 2 RSA offentliga och privata nyckeln filen toouse par med virtuella Linux-datorer.  Du kan använda en SSH-nyckel för att skapa virtuella datorer på Azure som standard toousing SSH-nycklar för autentisering, vilket eliminerar hello behovet av att lösenord toolog i.  Lösenord kan knäckas och öppna dina virtuella datorer in toorelentless brute force försök tooguess ditt lösenord. Virtuella datorer som skapats med Azure-mallar eller hello `azure-cli` kan innehålla offentliga SSH-nyckeln som en del av hello-distribution, ta bort post konfigurationssteg för distribution av att inaktivera Lösenordsinloggning för SSH.

## <a name="quick-commands"></a>Snabbkommandon

Kör hello följande kommandon från ett Bash-gränssnitt, ersätta hello exempel med ditt eget val.

Den offentliga SSH-nyckelfilen skapas som standard på `~/.ssh/id_rsa.pub`. När du uppmanas med hjälp av följande kommando hello, bör du skapa en ”lösenfras” toosecure din privata nyckel. (hello lösenfras är ett lösenord används tooencrypt din privata nyckel.)

```bash
ssh-keygen -t rsa -b 2048 
```

Lägg till hello nyskapad nyckel för`ssh-agent`:

```bash
ssh-add ~/.ssh/id_rsa
```

> [!IMPORTANT] 
> Hej ovan kan användas på Linux-operativsystem för nästan alla distributioner, men fungerar inte nödvändigtvis i behållare, som kan vara begränsad radikalt hello miljö. 

## <a name="detailed-walkthrough"></a>Detaljerad genomgång

Använda SSH offentliga och privata nycklar är hello enklaste sättet toolog i tooyour Linux-servrar. [Offentliga nycklar](https://en.wikipedia.org/wiki/Public-key_cryptography) ger en mycket säkrare sätt toolog i tooyour Linux eller BSD VM i Azure än med lösenord, som kan vara nyckelsökningsattacker mycket enklare.

> [!IMPORTANT]
> Din offentliga nyckel kan delas med alla, men bara du (eller din lokala säkerhetsinfrastruktur) har tillgång till den privata nyckeln.  hello SSH privat nyckel ska ha en [mycket säkert lösenord](https://www.xkcd.com/936/) (källa:[xkcd.com](https://xkcd.com)) toosafeguard den.  Lösenordet är bara tooaccess hello privata SSH-nyckeln och **är inte** hello lösenord.  När du lägger till ett lösenord tooyour SSH-nyckeln krypteras hello privata nyckeln med 128-bitars AES, så att hello privata nyckel är oanvändbar utan hello lösenord toodecrypt den.  Om en angripare stal din privata nyckel och att nyckeln har inte ett lösenord, de skulle vara kan toouse som privat nyckel toolog i tooany servrar som har hello motsvarande offentliga nyckel.  Om den privata nyckeln är lösenordsskyddad kan den inte användas av angriparen, vilket ger infrastrukturen i Azure ett extra säkerhetslager.

Den här artikeln skapar SSH-protokollversion 2 RSA offentliga och privata nyckelfiler som rekommenderas för distributioner på hello Resource Manager.  *SSH-rsa* nycklar krävs på hello [portal](https://portal.azure.com) för både klassiska och Resource Manager distributioner.

## <a name="disable-ssh-passwords-by-using-ssh-keys"></a>Inaktivera SSH-lösenord genom att använda SSH-nycklar

Azure kräver offentliga och privata nycklar med minst 2048 bitar i ssh-rsa-format. toocreate hello nycklar används `ssh-keygen`, som ställer ett antal frågor och sedan skriver en privat nyckel och en matchande offentlig nyckel. När en virtuell dator i Azure skapas kopieras hello offentlig nyckel för`~/.ssh/authorized_keys`.  SSH-nycklar i `~/.ssh/authorized_keys` är används toochallenge hello klienten toomatch hello motsvarande privata nyckel på en SSH-anslutning för inloggning.  När en Azure Linux VM skapas med hjälp av SSH-nycklar för autentisering, Azure konfigurerar hello SSHD server toonot Tillåt Lösenordsinloggning SSH-nycklar.  Genom att skapa virtuella Azure Linux-datorer med SSH-nycklar du därför att säkra hello distribution av Virtuella datorer och spara hello vanliga efter distributionen konfigurationssteget inaktivera lösenord i hello sshd_config config-fil.

## <a name="using-ssh-keygen"></a>Använda ssh-keygen

Det här kommandot skapas ett lösenordsskyddat (krypterat) SSH-nyckel som använder RSA 2048-bitars och den har kommenterats tooeasily identifiera den.  

SSH nycklar är som standard sparas i hello `~/.ssh` directory.  Om du inte har en `~/.ssh` directory, hello `ssh-keygen` kommando skapar den du med hello rätt behörigheter.

```bash
ssh-keygen \
-t rsa \
-b 2048 
```

*Kommandot förklarat*

`ssh-keygen`= hello används toocreate hello nycklar

`-t rsa`typ av nyckel toocreate som är hello RSA format = [wikipedia](https://en.wikipedia.org/wiki/RSA_(cryptosystem)

`-b 2048`= bitar i hello nyckel


## <a name="classic-portal-and-x509-certs"></a>Klassisk portal och X.509-certifikat

Om du använder hello Azure [klassiska portalen](https://manage.windowsazure.com/), X.509-certifikat krävs för hello SSH-nycklar.  Inga andra typer av offentliga SSH-nycklar tillåts, de *måste* vara X.509-certifikat.

toocreate ett X.509-certifikat från din befintliga privata SSH-RSA-nyckel:

```bash
openssl req -x509 \
-key ~/.ssh/id_rsa \
-nodes \
-days 365 \
-newkey rsa:2048 \
-out ~/.ssh/id_rsa.pem
```

## <a name="classic-deploy-using-asm"></a>Klassisk distribution med hjälp av `asm`

Om du använder hello klassisk distribuera modellen (Azure service management CLI `asm`), du kan använda en offentlig SSH-RSA-nyckel eller en RFC4716 formaterad nyckel i en **.pem** behållare.  hello SSH-RSA-offentliga nyckel är vad som skapats tidigare i den här artikeln använder `ssh-keygen`.

toocreate en RFC4716 formaterad nyckeln från en befintlig offentlig SSH-nyckel:

```bash
ssh-keygen \
-f ~/.ssh/id_rsa.pub \
-e \
-m RFC4716 > ~/.ssh/id_ssh2.pem
```

## <a name="example-of-ssh-keygen"></a>Exempel på ssh-keygen

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

Sparade nyckelfiler:

`Enter file in which toosave hello key (/home/ahmet/.ssh/id_rsa): ~/.ssh/id_rsa`

hello nyckelpar namn för den här artikeln.  Ett nyckelpar med namnet **id_rsa** är hello standard och vissa verktyg kan förvänta sig hello **id_rsa** privata nyckel filnamn så är det en bra idé. hello directory `~/.ssh/` är hello standardplatsen för SSH-nyckelpar och hello SSH-konfigurationsfil.  Om den inte anges med en fullständig sökväg `ssh-keygen` ska skapa hello nycklar i hello aktuella arbetskatalogen, inte hello standard `~/.ssh`.

En lista över hello `~/.ssh` directory.

```bash
ls -al ~/.ssh
-rw------- 1 ahmet staff  1675 Aug 25 18:04 id_rsa
-rw-r--r-- 1 ahmet staff   410 Aug 25 18:04 rsa.pub
```

Nyckellösenord:

`Enter passphrase (empty for no passphrase):`

`ssh-keygen`refererar tooa lösenord som används för tooencrypt hello privat nyckel som ”en lösenfras”.  Det är *starkt* rekommenderas tooadd en lösenfras tooyour nyckelpar. Utan en lösenfras skyddar hello privata nyckel, kan alla med hello nyckelfilen använda den toolog i tooany-server som har hello motsvarande offentlig nyckel. Lägga till en lösenfras erbjuder bättre skydd om någon kan toogain åtkomst tooyour fil för privat nyckel, vilket ger dig tid toochange hello nycklar används tooauthenticate du.

## <a name="using-ssh-agent-toostore-your-private-key-password"></a>Med hjälp av ssh-agent toostore lösenordet för privat nyckel

tooavoid att skriva din privata nyckel filen lösenfras med varje SSH-inloggning kan du använda `ssh-agent` toocache lösenordet fil för privat nyckel. Om du använder en Mac hello OS x-nyckelringen på ett säkert sätt lagrar hello privata nycklar som lösenord när du anropar `ssh-agent`.

Kontrollera och använda `ssh-agent` och `ssh-add` tooinform hello SSH system om hello viktiga filer så att hello lösenfras inte behöver toobe används interaktivt.

```bash
eval "$(ssh-agent -s)"
```

Lägg nu till hello privata nyckeln för`ssh-agent` hello kommandot `ssh-add`.

```bash
ssh-add ~/.ssh/id_rsa
```

lösenordet för hello privata nyckeln lagras nu på `ssh-agent`.

## <a name="using-ssh-copy-id-tooinstall-hello-new-key"></a>Med hjälp av `ssh-copy-id` tooinstall hello ny nyckel
Om du redan har skapat en virtuell dator kan du installera hello nya SSH offentlig nyckel tooyour Linux VM med hello följande kommando, ersätter hello VM användarnamn och hello serveradress med egna värden:

```bash
ssh-copy-id -i ~/.ssh/id_rsa.pub ahmet@myserver
```

## <a name="create-and-configure-an-ssh-config-file"></a>Skapa och konfigurera en SSH-konfigurationsfil

Det är en bästa praxis toocreate och konfigurera en `~/.ssh/config` filen toospeed upp inloggningar och för att optimera din beteende för SSH-klienten.

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

Nästa in toocreate virtuella Azure Linux-datorer använder hello nya offentliga SSH-nyckeln.  Virtuella Azure-datorer som har skapats med en offentlig SSH-nyckel som hello inloggning är bättre skyddade än virtuella datorer som skapats med hello inloggningen standardmetoden, lösenord.  Lösenord är som standard inaktiverade för Virtuella Azure-datorer som skapas med SSH-nycklar för att undvika råstyrkeattacker som försöker gissa lösenord. Om du behöver mer hjälp med att skapa SSH-nyckel eller kräver ytterligare certifikat, till exempel för användning med hello klassiska portalen finns [detaljerade steg toocreate SSH-nyckelpar och certifikat](create-ssh-keys-detailed.md).

* [Skapa en säker virtuell Linux-dator med hjälp av en Azure-mall](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Skapa en säker Linux VM som använder hello Azure-portalen](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Skapa en säker Linux VM som använder hello Azure CLI](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
