---
title: "Lägga till en användare till en Linux-VM i Azure | Microsoft Docs"
description: "Lägga till en användare till en Linux-VM på Azure."
services: virtual-machines-linux
documentationcenter: 
author: vlivech
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f8aa633b-8b75-45d7-b61d-11ac112cedd5
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/18/2016
ms.author: v-livech
ms.openlocfilehash: a95157f57c0cbd1f2a9ed68a0fe83140d7c9ec40
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="add-a-user-to-an-azure-vm"></a>Lägg till en användare i en Azure VM
En av de första aktiviteterna på alla nya Linux-VM är att skapa en ny användare.  Vi går igenom hur du skapar ett användarkonto med sudo, ställa in lösenordet lägger till SSH offentliga nycklar i den här artikeln och slutligen använder `visudo` att sudo utan lösenord.

Krav är: [ett Azure-konto](https://azure.microsoft.com/pricing/free-trial/), [offentliga och privata nycklar för SSH](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), en Azure-resursgrupp och Azure CLI installerad och växla till Azure Resource Manager med hjälp av `azure config mode arm`.

## <a name="quick-commands"></a>Snabbkommandon
```bash
# Add a new user on RedHat family distros
sudo useradd -G wheel exampleUser

# Add a new user on Debian family distros
sudo useradd -G sudo exampleUser

# Set a password
sudo passwd exampleUser
Enter new UNIX password:
Retype new UNIX password:
passwd: password updated successfully

# Copy the SSH Public Key to the new user
ssh-copy-id -i ~/.ssh/id_rsa exampleuser@exampleserver

# Change sudoers to allow no password
# Execute visudo as root to edit the /etc/sudoers file
visudo

# On RedHat family distros uncomment this line:
## Same thing without a password
# %wheel        ALL=(ALL)       NOPASSWD: ALL

# to this
## Same thing without a password
%wheel        ALL=(ALL)       NOPASSWD: ALL

# On Debian family distros change this line:
# Allow members of group sudo to execute any command
%sudo   ALL=(ALL:ALL) ALL

# to this
%sudo   ALL=(ALL) NOPASSWD:ALL

# Verify everything
# Verify the SSH keys & User account
bill@slackware$ ssh -i ~/.ssh/id_rsa exampleuser@exampleserver

# Verify sudo access
sudo top
```

## <a name="detailed-walkthrough"></a>Detaljerad genomgång
### <a name="introduction"></a>Introduktion
En av de första och de vanligaste uppgiften med en ny server är att lägga till ett användarkonto.  Rot-inloggningar ska vara inaktiverade och rotkontot själva ska inte användas med Linux-servern, endast sudo.  Ge en användarrot eskalering av privilegier med sudo den på rätt sätt att administrera och använda Linux.

Med hjälp av kommandot `useradd` vi lägger till användarkonton till Linux-VM.  Kör `useradd` ändrar `/etc/passwd`, `/etc/shadow`, `/etc/group`, och `/etc/gshadow`.  Vi lägger till en flagga som kommandoraden för att den `useradd` kommando för att lägga till den nya användaren till rätt sudo-gruppen på Linux.  Även du `useradd` skapas en post i `/etc/passwd` inte får det nya användarkontot ett lösenord.  Vi skapar ett initialt lösenord för den nya användaren med hjälp av enkla `passwd` kommando.  Det sista steget är att ändra sudo-regler för att tillåta användaren att köra kommandon med sudo-behörighet utan att behöva ange ett lösenord för varje kommando.  Logga in med den privata nyckeln som vi antar att användarkontot från obehöriga och ska tillåta sudo åtkomst utan lösenord.  

### <a name="adding-a-single-sudo-user-to-an-azure-vm"></a>Lägga till en enda sudo-användare i en Azure VM
Logga in på den virtuella Azure-datorn med hjälp av SSH-nycklar.  Om du inte har installationsprogrammet SSH offentlig nyckel kan slutföra den här artikeln först [med offentlig nyckel autentisering med Azure](http://link.to/article).  

Den `useradd` kommando utför följande aktiviteter:

* Skapa ett nytt användarkonto
* Skapa en ny användargrupp med samma namn
* Lägg till en tom post till`/etc/passwd`
* Lägg till en tom post till`/etc/gpasswd`

Den `-G` kommandoradsverktyget flaggan lägger till det nya användarkontot i rätt Linux gruppen och ge det nya användarkontot rot eskalering av privilegier.

#### <a name="add-the-user"></a>Lägg till användare
```bash
# On RedHat family distros
sudo useradd -G wheel exampleUser

# On Debian family distros
sudo useradd -G sudo exampleUser
```

#### <a name="set-a-password"></a>Ange ett lösenord
Den `useradd` kommando skapar användaren och lägger till en post för både `/etc/passwd` och `/etc/gpasswd` men anger inte lösenordet faktiskt.  Lösenordet har lagts till i en post med hjälp av den `passwd` kommando.

```bash
sudo passwd exampleUser
Enter new UNIX password:
Retype new UNIX password:
passwd: password updated successfully
```

Vi har nu en användare med sudo-behörighet på servern.

### <a name="add-your-ssh-public-key-to-the-new-user-account"></a>Lägg till en offentlig SSH-nyckel i det nya användarkontot
Från datorn, använder du den `ssh-copy-id` med det nya lösenordet.

```bash
ssh-copy-id -i ~/.ssh/id_rsa exampleuser@exampleserver
```

### <a name="using-visudo-to-allow-sudo-usage-without-a-password"></a>Med hjälp av visudo så att sudo-användningen utan lösenord
Med hjälp av `visudo` att redigera den `/etc/sudoers` filen lägger till några skyddslager mot felaktiga ändringar viktiga filen.  Vid körning `visudo`, `/etc/sudoers` är filen låst för att säkerställa att ingen användare kan göra ändringar när det är aktivt som redigeras.  Den `/etc/sudoers` också kontrolleras misstag av `visudo` när du försöker spara eller avsluta så att du kan spara en bruten sudoers-fil.

Vi har redan användare i rätt standardgruppen för sudo-åtkomst.  Nu det dags att aktivera dessa grupper om du vill använda sudo utan lösenord.

```bash
# Execute visudo as root to edit the /etc/sudoers file
visudo

# On RedHat family distros uncomment this line:
## Same thing without a password
# %wheel        ALL=(ALL)       NOPASSWD: ALL

# to this
## Same thing without a password
%wheel        ALL=(ALL)       NOPASSWD: ALL

# On Debian family distros change this line:
# Allow members of group sudo to execute any command
%sudo   ALL=(ALL:ALL) ALL

# to this
%sudo   ALL=(ALL) NOPASSWD:ALL
```

### <a name="verify-the-user-ssh-keys-and-sudo"></a>Verifiera användaren, ssh-nycklar och sudo
```bash
# Verify the SSH keys & User account
ssh -i ~/.ssh/id_rsa exampleuser@exampleserver

# Verify sudo access
sudo top
```
