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
# <a name="configure-sshd-on-azure-linux-vms"></a>Konfigurera SSHD på virtuella Azure Linux-datorer

Den här artikeln visar hur toolockdown hello SSH-Server på Linux, tooprovide bästa praxis säkerhet och även toospeed in hello SSH-inloggningen med hjälp av SSH-nycklar i stället för lösenord.  begränsa hello användare som tillåts toologin via godkända grupplistan inaktiverar SSH-protokollversion 1 toofurther låsning SSHD som vi ska toodisable hello rotanvändaren från att kunna toologin, ställa in ett minsta viktiga biten och konfigurera automatiskt logga ut från inaktiva användare.  hello kraven för den här artikeln är: ett Azure-konto ([skaffa en kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/)) och [SSH offentliga och privata nyckelfiler](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="quick-commands"></a>Snabbkommandon

Konfigurera `/etc/ssh/sshd_config` med hello följande inställningar:

### <a name="disable-password-logins"></a>Inaktivera Lösenordsinloggning

```bash
PasswordAuthentication no
```

### <a name="disable-login-by-hello-root-user"></a>Inaktivera inloggning av hello rotanvändaren

```bash
PermitRootLogin no
```

### <a name="allowed-groups-list"></a>Grupplistan över tillåtna

```bash
AllowGroups wheel
```

### <a name="allowed-users-list"></a>Användarlistan med tillåtna

```bash
AllowUsers ahmet ralph
```

### <a name="disable-ssh-protocol-version-1"></a>Inaktivera SSH-protokollet version 1

```bash
Protocol 2
```

### <a name="minimum-key-bits"></a>Minsta viktiga bits

```bash
ServerKeyBits 2048
```

### <a name="disconnect-idle-users"></a>Koppla från inaktiva användare

```bash
ClientAliveInterval 300
ClientAliveCountMax 0
```

## <a name="detailed-walkthrough"></a>Detaljerad genomgång

SSHD är hello SSH-Server som körs på hello Linux VM.  SSH är en klient som körs från ett gränssnitt på din MacBook Linux arbetsstation eller från en Bash på Windows.  SSH är också hello-protokollet används toosecure och kryptera hello kommunikationen mellan arbetsstation och hello Linux VM skapa SSH även VPN (virtuellt privat nätverk).

Det är mycket viktigt tookeep en inloggning tooyour Linux VM öppna för hello hela hanteringspaketen för den här artikeln.  När en SSH-anslutning har upprättats är den som en öppen session så länge hello fönster inte är stängd.  Du har en terminal loggat in kan för ändringar gjorts toobe toohello SSHD tjänsten utan låsas om en ny ändring görs.  Om du blir utelåst från ditt Linux VM till en skadad SSHD konfiguration, Azure erbjuder hello möjlighet tooreset en bruten SSHD konfiguration med hello [Access-tillägg för Azure VM](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Därför öppna vi två terminaler och SSH toohello Linux VM från båda.  Vi använder hello första terminal toomake hello ändringar tooSSHDs konfigurationsfilen och starta om hello SSHD tjänsten.  Vi använder hello andra terminal tootest de ändras när hello-tjänsten startas.  Eftersom vi inaktiverar SSH-lösenord och beroende strikt SSH-nycklar om SSH-nycklar inte är rätt och stänger hello anslutning toohello VM, hello VM är permanent låst och ingen kommer att kunna toologin tooit kräver det toobe bort och återskapas.

## <a name="disable-password-logins"></a>Inaktivera Lösenordsinloggning

Hej snabbaste sättet toosecure du Linux VM är toodisable lösenord inloggningar.  När Lösenordsinloggning är aktiverade, tvinga robotar crawlning hello web startar omedelbart försöker toobrute gissning hello lösenordet för ditt Linux-VM via SSH.  Inaktivera Lösenordsinloggning helt, kan hello SSH server tooignore alla lösenord inloggningsförsök.

```bash
PasswordAuthentication no
```

## <a name="disable-login-by-hello-root-user"></a>Inaktivera inloggning av hello rotanvändaren

Följande Linux bästa praxis, hello `root` ska aldrig vara inloggad på användare via SSH eller med hjälp av `sudo su`.  Alla kommandon som behöver behörigheter för roten på ska alltid köras via hello `sudo` kommando som loggar alla åtgärder för framtida granskning.  Inaktivera hello `root` användaren från att logga in via SSH är en bästa praxis säkerhetssteg som säkerställer att endast auktoriserade användare tillåts tooSSH.

```bash
PermitRootLogin no
```

## <a name="allowed-groups-list"></a>Grupplistan över tillåtna

SSH erbjuder en metod för att begränsa användare och grupper som tillåts eller otillåten från att logga in via SSH.  Den här funktionen använder listor tooapprove eller neka specifika användare och grupper från att logga in.  Ange hello hjul grupp toohello `AllowGroups` lista begränsar godkända inloggningar över SSH toojust konton som är i hello hjul grupp.

```bash
AllowGroups wheel
```

## <a name="allowed-users-list"></a>Användarlistan med tillåtna

Att begränsa SSH-inloggningar toojust användare är ett mer specifikt sätt tooaccomplish hello samma uppgift som `AllowGroups` är.  

```bash
AllowUsers ahmet ralph
```

## <a name="disable-ssh-protocol-version-1"></a>Inaktivera SSH-protokollet version 1

SSH-protokollversion 1 är osäker och ska vara inaktiverat.  SSH-protokollversion 2 är aktuella hello-versionen som erbjuder ett säkert sätt tooSSH tooyour server.  Inaktivera SSH version 1 nekar SSH-klienter som försöker tooestablish en anslutning med hello SSH-server med SSH version 1.  Endast SSH version 2-anslutningar tillåts toonegotiate en anslutning med hello SSH-server.

```bash
Protocol 2
```

## <a name="minimum-key-bits"></a>Minsta viktiga bits

Följande rekommenderade säkerhetsmetoder lösenord SSH-inloggning har inaktiverats och SSH-nycklar är tillåtna toobe används tooauthenticate med hello SSH-server.  Dessa SSH-nycklar kan skapas med nyckellängderna är olika, mätt i bitar.  Tillstånd att nycklar på 2 048 bitar långt är hello minsta godtagbara nyckellängd och metodtips.  Nycklar på mindre än 2 048 bitar kunde teoretiskt brytas.  Inställningen hello `ServerKeyBits` för`2048` tillåter alla anslutningar som använder nycklar på 2 048 bitar eller större och neka anslutningar på mindre än 2 048 bitar.

```bash
ServerKeyBits 2048
```

## <a name="disconnect-idle-users"></a>Koppla från inaktiva användare

SSH har hello möjlighet toodisconnect användare som har öppna anslutningar som har varit inaktiv i mer än en viss tid i sekunder.  Hålls öppna sessioner tooonly användare som är aktiva gränser hello exponering av hello Linux VM.

```bash
ClientAliveInterval 300
ClientAliveCountMax 0
```

## <a name="restart-sshd"></a>Starta om SSHD

tooenable hello inställningar i `/etc/ssh/sshd_config` starta om processen för hello SSHD som startar om hello SSH-server.  hello terminalfönster som du använder SSH-server för toorestart hello förbli öppen utan att förlora hello öppna SSH-session.  tootest hello nya SSH serverinställningarna använder en andra terminalfönster eller fliken.  Om du använder en separat terminal tootest hello SSH-anslutningen kan du toogo tillbaka och göra ytterligare ändringar toohello `/etc/ssh/sshd_config` i hello första terminal utan är låst av en ny SSHD ändring.  

### <a name="on-redhat-centos-and-fedora"></a>På Redhat, Centos och Fedora

```bash
service sshd restart
```

### <a name="on-debian--ubuntu"></a>På Debian och Ubuntu

```bash
service ssh restart
```

## <a name="reset-sshd-using-azure-reset-access"></a>Återställ SSHD med hjälp av Azure Återställ-åtkomst

Om du är utelåst från en ny ändring toohello SSHD konfiguration kan du använda hello Azure VM-tillägget för åtkomst tooreset hello SSHD tillbaka toohello ursprungliga konfiguration.

Ersätt alla exempelnamnen med din egen.

```azurecli
azure vm reset-access \
--resource-group myResourceGroup \
--name myVM \
--reset-ssh
```

## <a name="install-fail2ban"></a>Installera Fail2ban

Det rekommenderas starkt tooinstall och installationsprogrammet hello öppen källkod app Fail2ban, vilka block upprepade försök toologin tooyour Linux VM via SSH med hjälp av brute force.  Fail2ban loggar upprepas misslyckades försök toologin via SSH och skapar brandväggsregler tooblock hello IP-adress som hello försök kommer från.

* [Fail2ban webbsida](http://www.fail2ban.org/wiki/index.php/Main_Page)

## <a name="next-steps"></a>Nästa steg

Nu när du har konfigurerat och låst hello SSH-server på Linux-VM finns ytterligare säkerhetsmetoder kan du följa.  

* [Hantera användare, SSH och kontrollera eller reparera diskar på virtuella Azure Linux-datorer med hjälp av hello VMAccess-tillägget](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

* [Kryptera diskar på en Linux VM som använder hello Azure CLI](encrypt-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

* [Åtkomst och säkerhet i Azure Resource Manager-mallar](dotnet-core-3-access-security.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
