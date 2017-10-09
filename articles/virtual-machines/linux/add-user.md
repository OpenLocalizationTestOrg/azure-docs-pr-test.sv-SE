---
title: "aaaAdd användaren-tooa Linux VM i Azure | Microsoft Docs"
description: "Lägg till en användare tooa Linux VM på Azure."
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
ms.openlocfilehash: eed050154adf0cbed2c168e7aa675bd3ded85fcd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-a-user-tooan-azure-vm"></a>Lägg till en användare tooan Azure VM
En av hello första uppgifter på alla nya Linux-VM är toocreate en ny användare.  Vi går igenom skapar ett användarkonto för sudo, ange hello lösenord att lägga till SSH offentliga nycklar i den här artikeln och slutligen använder `visudo` tooallow sudo utan lösenord.

Krav är: [ett Azure-konto](https://azure.microsoft.com/pricing/free-trial/), [offentliga och privata nycklar för SSH](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), en Azure-resursgrupp och hello Azure CLI installerade och växlade tooAzure Resource Manager med hjälp av `azure config mode arm`.

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

# Copy hello SSH Public Key toohello new user
ssh-copy-id -i ~/.ssh/id_rsa exampleuser@exampleserver

# Change sudoers tooallow no password
# Execute visudo as root tooedit hello /etc/sudoers file
visudo

# On RedHat family distros uncomment this line:
## Same thing without a password
# %wheel        ALL=(ALL)       NOPASSWD: ALL

# toothis
## Same thing without a password
%wheel        ALL=(ALL)       NOPASSWD: ALL

# On Debian family distros change this line:
# Allow members of group sudo tooexecute any command
%sudo   ALL=(ALL:ALL) ALL

# toothis
%sudo   ALL=(ALL) NOPASSWD:ALL

# Verify everything
# Verify hello SSH keys & User account
bill@slackware$ ssh -i ~/.ssh/id_rsa exampleuser@exampleserver

# Verify sudo access
sudo top
```

## <a name="detailed-walkthrough"></a>Detaljerad genomgång
### <a name="introduction"></a>Introduktion
En av hello första och de vanligaste uppgiften med en ny server är tooadd ett användarkonto.  Rot-inloggningar ska vara inaktiverade och hello rotkontot själva ska inte användas med Linux-servern, endast sudo.  Ger en användare rot eskalering privilegier med sudo den hello tooadminister på rätt sätt och använda Linux.

Kommandot hello `useradd` vi lägger till användare konton toohello Linux VM.  Kör `useradd` ändrar `/etc/passwd`, `/etc/shadow`, `/etc/group`, och `/etc/gshadow`.  Vi lägger till en kommandorad flaggan toohello `useradd` kommandot tooalso lägga till hello ny toohello rätt sudo användargrupp på Linux.  Även du `useradd` skapas en post i `/etc/passwd` inte får hello nytt användarkonto ett lösenord.  Vi skapar ett initialt lösenord för hello nya användare med enkel hello `passwd` kommando.  hello sista steget är toomodify hello sudo regler tooallow som användaren tooexecute kommandon med sudo-behörighet utan tooenter ett lösenord för varje kommando.  Logga in med hello privata nyckel vi antar att användarkontot från obehöriga och ska tooallow sudo åtkomst utan lösenord.  

### <a name="adding-a-single-sudo-user-tooan-azure-vm"></a>Lägga till en enda sudo användaren tooan Azure VM
Logga in toohello Azure VM med hjälp av SSH-nycklar.  Om du inte har installationsprogrammet SSH offentlig nyckel kan slutföra den här artikeln först [med offentlig nyckel autentisering med Azure](http://link.to/article).  

Hej `useradd` kommandot har slutförts hello följande uppgifter:

* Skapa ett nytt användarkonto
* Skapa en ny användargrupp med hello samma namn
* Lägg till en tom post för`/etc/passwd`
* Lägg till en tom post för`/etc/gpasswd`

Hej `-G` kommandoradsverktyget flaggan lägger till hello ny konto toohello rätt Linux användargrupp ger hello nytt användarkonto rot eskalering av privilegier.

#### <a name="add-hello-user"></a>Lägg till hello användare
```bash
# On RedHat family distros
sudo useradd -G wheel exampleUser

# On Debian family distros
sudo useradd -G sudo exampleUser
```

#### <a name="set-a-password"></a>Ange ett lösenord
Hej `useradd` kommando skapar hello användare och lägger till en post tooboth `/etc/passwd` och `/etc/gpasswd` men anger inte faktiskt hello lösenord.  hello lösenord läggs toohello posten med hello `passwd` kommando.

```bash
sudo passwd exampleUser
Enter new UNIX password:
Retype new UNIX password:
passwd: password updated successfully
```

Vi har nu en användare med sudo-behörighet på hello-servern.

### <a name="add-your-ssh-public-key-toohello-new-user-account"></a>Lägga till en offentlig SSH-nyckel toohello nytt användarkonto
Använd hello från din dator `ssh-copy-id` med hello nytt lösenord.

```bash
ssh-copy-id -i ~/.ssh/id_rsa exampleuser@exampleserver
```

### <a name="using-visudo-tooallow-sudo-usage-without-a-password"></a>Med visudo tooallow sudo användning utan lösenord
Med hjälp av `visudo` tooedit hello `/etc/sudoers` filen lägger till några skyddslager mot felaktiga ändringar viktiga filen.  Vid körning `visudo`, hello `/etc/sudoers` filen är låst tooensure ingen användare kan göra ändringar när det är aktivt som redigeras.  Hej `/etc/sudoers` också kontrolleras misstag av `visudo` när du försöker toosave eller avsluta så att du kan spara en bruten sudoers-fil.

Vi har redan användare i hello rätt standardgruppen för sudo-åtkomst.  Nu vi tooenable dessa grupper toouse sudo utan lösenord.

```bash
# Execute visudo as root tooedit hello /etc/sudoers file
visudo

# On RedHat family distros uncomment this line:
## Same thing without a password
# %wheel        ALL=(ALL)       NOPASSWD: ALL

# toothis
## Same thing without a password
%wheel        ALL=(ALL)       NOPASSWD: ALL

# On Debian family distros change this line:
# Allow members of group sudo tooexecute any command
%sudo   ALL=(ALL:ALL) ALL

# toothis
%sudo   ALL=(ALL) NOPASSWD:ALL
```

### <a name="verify-hello-user-ssh-keys-and-sudo"></a>Kontrollera hello användaren ssh-nycklar och sudo
```bash
# Verify hello SSH keys & User account
ssh -i ~/.ssh/id_rsa exampleuser@exampleserver

# Verify sudo access
sudo top
```
