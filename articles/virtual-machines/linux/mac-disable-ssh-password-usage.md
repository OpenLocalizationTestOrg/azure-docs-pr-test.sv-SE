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
# <a name="disable-ssh-passwords-on-your-linux-vm-by-configuring-sshd"></a>Inaktivera SSH-lösenord på Linux-VM genom att konfigurera SSHD
Den här artikeln fokuserar på hur toolock ned hello inloggningssäkerhet av Linux-VM.  Så snart hello SSH-port 22 öppnas starta toohello world robotar försök toologin gissa lösenord.  Vad gör vi i den här artikeln är att inaktivera Lösenordsinloggning via SSH.  Genom att ta bort hello möjlighet helt tvinga toouse lösenord som skyddar vi hello Linux VM från den här typen av brute attack.  hello lagts till är bara att vi kommer att konfigurera Linux SSHD tooonly Tillåt inloggningar via SSH offentliga och privata nycklar, allra hello säkraste sättet toologin tooLinux.  hello möjliga kombinationer av det skulle kräva tooguess hello privata nyckel är stor och därmed förhindrar robotar från även stör tootry toobrute kraft SSH-nycklar.

## <a name="goals"></a>Mål
* Konfigurera SSHD toodisallow:
  * Lösenordsinloggning
  * Användarinloggning för rot
  * Anrop / svar-autentisering
* Konfigurera SSHD tooallow:
  * endast SSH-nyckel inloggningar
* Starta om SSHD fortfarande logga in
* Hello nya SSHD testkonfiguration

## <a name="introduction"></a>Introduktion
[SSH definierats](https://en.wikipedia.org/wiki/Secure_Shell)

SSHD är hello SSH-Server som körs på hello Linux VM.  SSH är en klient som körs från ett gränssnitt på arbetsstationen MacBook- eller Linux.  SSH är också hello-protokollet används toosecure och kryptera hello kommunikationen mellan arbetsstation och hello Linux VM.

Den här artikeln är det mycket viktigt tookeep en inloggning tooyour Linux VM öppen för hela hello igenom.  Därför kommer vi öppna två terminaler och SSH toohello Linux VM från båda.  Vi använder hello första terminal toomake hello ändringar tooSSHDs konfigurationsfilen och starta om hello SSHD-tjänsten.  Vi använder hello andra terminal tootest de ändras när hello-tjänsten startas.  Eftersom vi inaktiverar SSH-lösenord och beroende strikt SSH-nycklar om SSH-nycklar inte är rätt och stänger hello anslutning toohello VM, hello VM är permanent låst och ingen kommer att kunna toologin tooit kräver det toobe bort och återskapas.

## <a name="prerequisites"></a>Krav
* [Skapa SSH-nycklar för Linux och Mac för virtuella Linux-datorer i Azure](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* Azure-konto
  * [gratis utvärderingsversion registrering](https://azure.microsoft.com/pricing/free-trial/)
  * [Azure Portal](http://portal.azure.com)
* Linux VM körs på azure
* SSH offentliga och privata nyckelpar i`~/.ssh/`
* Offentlig SSH-nyckel i `~/.ssh/authorized_keys` på hello Linux VM
* Sudo-behörighet på hello VM
* Port 22 öppna

## <a name="quick-commands"></a>Snabbkommandon
*Erfarna Linux-administratörer som vill bara hello TLDR version börja här.  Hoppa över det här avsnittet för alla andra som vill hello detaljerad förklaring och gå igenom.*

```bash
sudo vim /etc/ssh/sshd_config
```

Redigera hello config-fil på följande sätt:

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

Starta om hello SSHD. På Debian-baserade distributioner:

```bash
sudo service ssh restart
```

På Red Hat-baserade distributioner:

```bash
sudo service sshd restart
```

## <a name="detailed-walk-through"></a>Detaljerad genomgång
Inloggningen toohello Linux VM på terminal 1 (T1).  Inloggningen toohello Linux VM terminal 2 (T2).

På T2 vi tooedit hello SSHD konfigurationsfilen.  

```bash
sudo vim /etc/ssh/sshd_config
```

Här kommer redigera bara hello inställningar toodisable lösenord och aktivera SSH-nyckel inloggningar.  Det finns många inställningar i den här filen som du bör undersöka och ändra toomake Linux & SSH så säkert som du behöver.

#### <a name="disable-password-logins"></a>Inaktivera Lösenordsinloggning

```sh
# Change PasswordAuthentication toothis:
PasswordAuthentication no
```

#### <a name="enable-public-key-authentication"></a>Aktivera autentisering med offentlig nyckel

```sh
# Change PubkeyAuthentication toothis:
PubkeyAuthentication yes
```

#### <a name="disable-root-login"></a>Inaktivera rot-inloggning

```sh
# Change PermitRootLogin toothis:
PermitRootLogin no
```

#### <a name="disable-challenge-response-authentication"></a>Inaktivera anrop / svar-autentisering
```sh
# Change ChallengeResponseAuthentication toothis:
ChallengeResponseAuthentication no
```

### <a name="restart-sshd"></a>Starta om SSHD
Kontrollera att du fortfarande är inloggad från hello T1-gränssnittet.  Detta är viktigt, så att du inte blir utelåst från den virtuella datorn om SSH-nycklar inte är korrekt eftersom lösenord är nu inaktiverad.  Om alla inställningar är felaktiga på din Linux VM som du kan använda T1 toofix sshd_config som du fortfarande logga in och SSH kommer att hålla hello anslutning alive under hello service SSHD starta om.

Från T2 kör:

##### <a name="on-hello-debian-family"></a>På hello Debian-familjen
```bash
sudo service ssh restart
```

##### <a name="on-hello-redhat-family"></a>På hello RedHat familj
```bash
sudo service sshd restart
```

Lösenord har nu inaktiverats på den virtuella datorn skyddas mot brute force lösenord inloggningsförsök.  Du kommer att kan toologin snabbare och säkrare med endast SSH-nycklar tillåts.

