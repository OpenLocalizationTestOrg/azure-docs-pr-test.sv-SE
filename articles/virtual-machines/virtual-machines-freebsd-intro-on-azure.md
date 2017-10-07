---
title: aaaIntroduction tooFreeBSD i Azure | Microsoft Docs
description: "Lär dig mer om hur du använder FreeBSD-virtuella datorer på Azure"
services: virtual-machines-linux
documentationcenter: 
author: KylieLiang
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 32b87a5f-d024-4da0-8bf0-77e233d1422b
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/28/2017
ms.author: kyliel
ms.openlocfilehash: eab7aeda7f7ef893740b39c0250aacc29d6fd71b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-toofreebsd-on-azure"></a>Introduktion tooFreeBSD på Azure
Det här avsnittet innehåller en översikt över en FreeBSD virtuell dator som körs i Azure.

## <a name="overview"></a>Översikt
FreeBSD för Microsoft Azure är en avancerad operativsystem används toopower moderna servrar, skrivbord och inbäddade plattformar.

Microsoft Corporation gör avbildningar av FreeBSD tillgängligt på Azure med hello [Azure VM-Gästagent](https://github.com/Azure/WALinuxAgent/) förkonfigurerade. För närvarande erbjuds hello följande FreeBSD versioner som avbildningar av Microsoft:

- FreeBSD 10.3-versionen
- FreeBSD 11.0-versionen

hello-agenten är ansvarig för kommunikation mellan hello FreeBSD VM och hello Azure-strukturen för till exempel etablering hello VM på första användning (användarnamn, lösenord eller SSH-nyckel, värdnamn, etc.) och aktiverar funktioner för selektiv VM-tillägg.

För framtida versioner av FreeBSD hello-strategi är toostay aktuella och tillgängliggöra hello senaste versioner strax efter att de har publicerats av hello FreeBSD versionen Utvecklingsteamet.

## <a name="deploying-a-freebsd-virtual-machine"></a>Distribuera en virtuell dator FreeBSD
Distribuera en virtuell dator FreeBSD är enkelt att använda en bild från hello Azure Marketplace från hello Azure-portalen:

- [FreeBSD 10.3 på hello Azure Marketplace](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd103/)
- [FreeBSD 11.0 på hello Azure Marketplace](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd110/)

### <a name="create-a-freebsd-vm-through-azure-cli-20-on-freebsd"></a>Skapa en FreeBSD-VM via Azure CLI 2.0 på FreeBSD
Du måste först tooinstall [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) trots att följande kommando på en FreeBSD-dator.

```bash 
    curl -L https://aka.ms/InstallAzureCli | bash
```

Om bash inte är installerat på datorn FreeBSD, kör du följande kommando innan hello-installationen. 

```
    sudo pkg install bash
```

Om python inte är installerat på datorn FreeBSD, kör du följande kommandon innan hello-installationen. 

```
    sudo pkg install python35
    cd /usr/local/bin 
    sudo rm /usr/local/bin/python 
    sudo ln -s /usr/local/bin/python3.5 /usr/local/bin/python
```

Under installationen av hello uppmanas du `Modify profile tooupdate your $PATH and enable shell/tab completion now? (Y/n)`. Om du svarar `y` och ange `/etc/rc.conf` som `a path tooan rc file tooupdate`, du kan uppfylla hello problemet `ERROR: [Errno 13] Permission denied`. tooresolve det här problemet bör du bevilja hello skriva rätt toocurrent användaren mot hello filen `etc/rc.conf`.

Du kan nu logga in i Azure och skapa din FreeBSD VM. Nedan visas ett exempel toocreate en virtuell dator 11.0 FreeBSD. Du kan också lägga till parametern hello `--public-ip-address-dns-name` med ett globalt unikt DNS-namn för en nyligen skapad offentliga IP-adress. 

```azurecli
    az login 
    az group create -n myResourceGroup -l westus az vm create -n myFreeBSD11 -g myResourceGroup --image MicrosoftOSTC:FreeBSD:11.0:latest --admin-username azureuser --ssh-key-value /etc/ssh/ssh_host_rsa_key.pub 
```

Logga sedan in tooyour FreeBSD VM via hello ip-adress som skrivs ut i hello utdata från ovan distribution. 

```bash
    ssh azureuser@xx.xx.xx.xx -i /etc/ssh/ssh_host_rsa_key
```   

## <a name="vm-extensions-for-freebsd"></a>VM-tillägg för FreeBSD
Följande är VM-tillägg som stöds i FreeBSD.

### <a name="vmaccess"></a>VMAccess
Hej [VMAccess](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess) tillägg kan:

* Återställa hello lösenord på hello ursprungliga sudo användaren.
* Skapa en ny sudo-användare med hello lösenord har angetts.
* Ange hello offentliga värdnyckeln med hello nyckel anges.
* Återställa hello offentliga värdnyckel som tillhandahålls under etablering om hello värdnyckel inte tillhandahålls av virtuell dator.
* Öppna hello SSH-port (22) och återställa hello sshd_config om reset_ssh anges tootrue.
* Ta bort hello befintliga användare.
* Kontrollera diskar.
* Reparera en lagts till disk.

### <a name="customscript"></a>CustomScript
Hej [CustomScript](https://github.com/Azure/azure-linux-extensions/tree/master/CustomScript) tillägg kan:

* Om hämta hello anpassade skript från Azure Storage eller externa offentlig lagring (till exempel GitHub).
* Kör skriptet för hello post punkt.
* Stöd för infogade kommandon.
* Konvertera automatiskt Windows-typ ny rad i gränssnittet och Python-skript.
* Ta bort BOM i gränssnittet och Python skript automatiskt.
* Skydda känsliga data i CommandToExecute.

> [!NOTE]
> FreeBSD VM stöder endast CustomScript version 1.x nu.  

## <a name="authentication-user-names-passwords-and-ssh-keys"></a>Autentisering: användarnamn, lösenord och SSH-nycklar
När du skapar en FreeBSD virtuell dator med hjälp av hello Azure-portalen, måste du ange ett användarnamn, lösenord eller offentlig SSH-nyckel.
Användarnamn för att distribuera en FreeBSD virtuell dator på Azure måste inte matcha namnen på Systemkonton (UID < 100) finns redan i hello virtuell dator (”rot”, till exempel).
För närvarande stöds endast hello RSA SSH-nyckeln. Flerradig SSH-nyckeln måste börja med `---- BEGIN SSH2 PUBLIC KEY ----` och sluta med `---- END SSH2 PUBLIC KEY ----`.

## <a name="obtaining-superuser-privileges"></a>Hämta superanvändare
hello-användarkonto som anges under distributionen av virtuella datorer instans i Azure är ett konto med privilegier. hello paket med sudo har installerats i hello publicerade FreeBSD-bild.
När du är inloggad med det här användarkontot, kan du köra kommandon som rot med hjälp av hello kommandosyntax.

```
    $ sudo <COMMAND>
```

Du kan du hämta ett rot-gränssnitt med hjälp av `sudo -s`.

## <a name="known-issues"></a>Kända problem
Hej [Azure VM-Gästagent](https://github.com/Azure/WALinuxAgent/) version 2.2.2 har [känt problem] (https://github.com/Azure/WALinuxAgent/pull/517) som orsakar hello etablera fel för FreeBSD VM på Azure. hello korrigering inhämtats via [Azure VM-Gästagent](https://github.com/Azure/WALinuxAgent/) version 2.2.3 och senare versioner. 

## <a name="next-steps"></a>Nästa steg
* Gå för[Azure Marketplace](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd110/) toocreate en FreeBSD VM.
* Om du vill toobring egna FreeBSD tooAzure finns för[skapa och ladda upp en FreeBSD VHD-tooAzure](linux/classic/freebsd-create-upload-vhd.md).
