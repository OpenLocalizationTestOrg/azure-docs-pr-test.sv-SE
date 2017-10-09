---
title: "aaaReset åtkomst på virtuella Azure Linux-datorer med hjälp av hello VMAccess-tillägget | Microsoft Docs"
description: "Återställa åtkomst på virtuella Azure Linux-datorer med hjälp av hello VMAccess-tillägget."
services: virtual-machines-linux
documentationcenter: 
author: vlivech
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 261a9646-1f93-407e-951e-0be7226b3064
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 10/25/2016
ms.author: v-livech
ms.openlocfilehash: 2636655f3f7d14ba30e1dc62c319e4e278521ead
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-users-ssh-and-check-or-repair-disks-on-azure-linux-vms-using-hello-vmaccess-extension-with-hello-azure-cli-10"></a>Hantera användare, SSH och kontrollera eller reparera diskar på virtuella Azure Linux-datorer med hjälp av hello VMAccess-tillägget med hello Azure CLI 1.0
Den här artikeln visar hur toouse hello Azure VMAcesss tillägget toocheck eller reparera en disk, återställa användaråtkomst, hantera användarkonton eller återställa hello SSHD konfigurationen på Linux. hello artikel kräver:

* ett Azure-konto ([hämta en kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/)).
* Hej [Azure CLI](../../cli-install-nodejs.md) loggat in med `azure login`.
* hello Azure CLI *måste vara i* läget Azure Resource Manager `azure config mode arm`.


## <a name="cli-versions-toocomplete-hello-task"></a>CLI versioner toocomplete hello aktivitet
Du kan göra hello med hjälp av något av följande versioner av CLI hello:

- [Azure CLI 1.0](#quick-commands)– våra CLI för hello klassisk och resurs management distributionsmodeller (den här artikeln)
- [Azure CLI 2.0](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) -vår nästa generations CLI för hello resursdistributionsmodell för hantering


## <a name="quick-commands"></a>Snabbkommandon
Det finns två sätt toouse VMAccess på din virtuella Linux-datorer:

* Med hjälp av hello Azure CLI 1.0 och hello obligatoriska parametrar.
* Använder rådata JSON-filer som VMAccess bearbetar och sedan vidta åtgärder för.

För hello snabbkommando avsnittet vi toouse hello Azure CLI 1.0 `azure vm reset-access` metod. Följande kommandoexempel Ersätt i hello hello värden som innehåller ”exempel” Hej värden från din egen miljö.

## <a name="create-a-resource-group-and-linux-vm"></a>Skapa en resursgrupp och en virtuell Linux-dator
```bash
azure group create myResourceGroup westus
```

## <a name="create-a-debian-vm"></a>Skapa en Debian virtuell dator
```azurecli
azure vm quick-create \
  -M ~/.ssh/id_rsa.pub \
  -u myAdminUser \
  -g myResourceGroup \
  -l westus \
  -y Linux \
  -n myVM \
  -Q Debian
```

## <a name="reset-root-password"></a>Återställ rotlösenordet
tooreset hello rotlösenordet:

```azurecli
azure vm reset-access \
  -g myResourceGroup \
  -n myVM \
  -u root \
  -p myNewPassword
```

## <a name="ssh-key-reset"></a>Återställning av SSH-nyckel
tooreset hello SSH-nyckel för en rot-användare:

```azurecli
azure vm reset-access \
  -g myResourceGroup \
  -n myVM \
  -u myAdminUser \
  -M ~/.ssh/id_rsa.pub
```

## <a name="create-a-user"></a>Skapa en användare
toocreate en användare:

```azurecli
azure vm reset-access \
  -g myResourceGroup \
  -n myVM \
  -u myAdminUser \
  -p myAdminUserPassword
```

## <a name="remove-a-user"></a>Ta bort en användare
```azurecli
azure vm reset-access \
  -g myResourceGroup \
  -n myVM \
  -R myRemovedUser
```

## <a name="reset-sshd"></a>Återställ SSHD
tooreset hello SSHD konfiguration:

```azurecli
azure vm reset-access \
  -g myResourceGroup \
  -n myVM
  -r
```


## <a name="detailed-walkthrough"></a>Detaljerad genomgång
### <a name="vmaccess-defined"></a>VMAccess definierats:
hello disk på Linux-VM visas fel. Du på något sätt återställa hello rotlösenordet för Linux-VM eller tagits bort av misstag din privata SSH-nyckel. Om detta har inträffat i hello dagarnas hello datacenter, skulle du behöver toodrive det och öppna sedan hello KVM tooget hello server-konsolen. Se hello Azure VMAccess-tillägget som den KVM-växel som gör att du tooaccess hello konsolen tooreset åtkomst tooLinux eller genomför diskunderhåll nivå.

Detaljerad genomgång för hello vi toouse hello långt format för VMAccess som använder rådata JSON-filer.  Filerna VMAccess JSON kallas även från Azure-mallar.

### <a name="using-vmaccess-toocheck-or-repair-hello-disk-of-a-linux-vm"></a>Med hjälp av VMAccess toocheck eller reparera hello disk för en Linux-VM
Med hjälp av VMAccess kan du göra en fsck körs på hello disk under Linux-VM.  Du kan också göra en kontroll och en diskreparationen med hjälp av en VMAccess.

Använd skriptet VMAccess toocheck och reparera hello disk:

`disk_check_repair.json`

```json
{
  "check_disk": "true",
  "repair_disk": "true, user-disk-name"
}
```

Kör hello VMAccess skriptet med:

```azurecli
azure vm extension set \
  myResourceGroup \
  myVM \
  VMAccessForLinux \
  Microsoft.OSTCExtensions * \
  --private-config-path disk_check_repair.json
```

### <a name="using-vmaccess-tooreset-user-access-toolinux"></a>Med hjälp av VMAccess tooreset användaren åtkomst tooLinux
Om du har förlorat åtkomst tooroot på Linux-VM, kan du starta ett VMAccess skript tooreset hello rotlösenord.

tooreset hello rotlösenordet, Använd det här VMAccess-skriptet:

`reset_root_password.json`

```json
{
  "username":"root",
  "password":"myNewPassword"
}
```

Kör hello VMAccess skriptet med:

```azurecli
azure vm extension set \
  myResourceGroup \
  myVM \
  VMAccessForLinux \
  Microsoft.OSTCExtensions * \
  --private-config-path reset_root_password.json
```

tooreset hello SSH-nyckel för en rot-användare, Använd VMAccess skriptet:

`reset_ssh_key.json`

```json
{
  "username":"myAdminUser",
  "ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNG1vHY7P2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7iUo5IdwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5woYtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== myAdminUser@myVM" 
}
```

Kör hello VMAccess skriptet med:

```azurecli
azure vm extension set \
  myResourceGroup \
  myVM \
  VMAccessForLinux \
  Microsoft.OSTCExtensions * \
  --private-config-path reset_ssh_key.json
```

### <a name="using-vmaccess-toomanage-user-accounts-on-linux"></a>Med hjälp av VMAccess toomanage användarkonton på Linux
VMAccess är ett Python-skript som kan använda toomanage användare på ditt Linux VM utan att logga in och använda sudo eller hello rotkontot.

toocreate en användare kan använda det här VMAccess-skriptet:

`create_new_user.json`

```json
{
  "username":"myNewUser",
  "ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNG1vHY7P2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7iUo5IdwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5woYtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== myNewUser@myVM",
  "password":"myNewUserPassword"
}
```

Kör hello VMAccess skriptet med:

```azurecli
azure vm extension set \
  myResourceGroup \
  myVM \
  VMAccessForLinux \
  Microsoft.OSTCExtensions * \
  --private-config-path create_new_user.json
```

toodelete en användare kan använda det här VMAccess-skriptet:

`remove_user.json`

```json
{
  "remove_user":"myDeletedUser"
}
```

Kör hello VMAccess skriptet med:

```azurecli
azure vm extension set \
  myResourceGroup \
  myVM \
  VMAccessForLinux \
  Microsoft.OSTCExtensions * \
  --private-config-path remove_user.json
```

### <a name="using-vmaccess-tooreset-hello-sshd-configuration"></a>Med VMAccess tooreset hello SSHD konfiguration
Om du gör ändringar toohello Linux virtuella datorer SSHD konfiguration och Stäng hello SSH-anslutning innan du kontrollerar hello ändringar du kan förhindras att SSH'ing igen.  VMAccess kan vara används tooreset hello SSHD configuration tillbaka tooa fungerande konfiguration utan loggas via SSH.

tooreset hello SSHD konfiguration använder det här VMAccess-skriptet:

`reset_sshd.json`

```json
{
  "reset_ssh": true
}
```

Kör hello VMAccess skriptet med:

```azurecli
azure vm extension set \
  myResourceGroup \
  myVM \
  VMAccessForLinux \
  Microsoft.OSTCExtensions * \
  --private-config-path reset_sshd.json
```

## <a name="next-steps"></a>Nästa steg
Uppdatera Linux är med hjälp av Azure VMAccess tillägg en metod toomake ändringar på en Linux-VM som körs.  Du kan också använda verktyg som molntjänster init och Azure-mallar toomodify din Linux VM på Start.

[Om virtuella datortillägg och funktioner](../windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[Skapa Azure Resource Manager-mallar med Linux VM-tillägg](../windows/template-description.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[Med hjälp av molnet init toocustomize en Linux VM under skapande av](using-cloud-init.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

