---
title: "aaaReset åtkomst tooan virtuella Azure Linux-datorer | Microsoft Docs"
description: "Hur toomanage användare och återställa åtkomst på virtuella Linux-datorer med hjälp av hello VMAccess-tillägget och hello Azure CLI 2.0"
services: virtual-machines-linux
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 261a9646-1f93-407e-951e-0be7226b3064
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 08/04/2017
ms.author: danlep
ms.openlocfilehash: 2f8db01b9fac20bf547d8b1926e5c0b3c5d18280
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-users-ssh-and-check-or-repair-disks-on-linux-vms-using-hello-vmaccess-extension-with-hello-azure-cli-20"></a>Hantera användare, SSH och kontrollera eller reparera diskar på virtuella Linux-datorer med hjälp av hello VMAccess-tillägget med hello Azure CLI 2.0
hello disk på Linux-VM visas fel. Du på något sätt återställa hello rotlösenordet för Linux-VM eller tagits bort av misstag din privata SSH-nyckel. Om detta har inträffat i hello dagarnas hello datacenter, skulle du behöver toodrive det och öppna sedan hello KVM tooget hello server-konsolen. Se hello Azure VMAccess-tillägget som den KVM-växel som gör att du tooaccess hello konsolen tooreset åtkomst tooLinux eller genomför diskunderhåll nivå.

Den här artikeln visar hur toouse hello Azure VMAccess-tillägget toocheck eller reparera en disk, återställa användaråtkomst, hantera användarkonton och Återställ hello SSH-konfigurationen på Linux. Du kan också utföra dessa steg med hello [Azure CLI 1.0](using-vmaccess-extension-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).


## <a name="ways-toouse-hello-vmaccess-extension"></a>Sätt toouse hello VMAccess-tillägget
Det finns två sätt som du kan använda hello VMAccess-tillägget på din virtuella Linux-datorer:

* Använd hello Azure CLI 2.0 och hello krävs parametrar.
* [Använd rådata JSON-filer som hello VMAccess-tillägget processen](#use-json-files-and-the-vmaccess-extension) och sedan vidta åtgärder för.

Hej följande exempel används [az vm användaren](/cli/azure/vm/user) kommandon. tooperform dessa steg, behöver du hello senaste [Azure CLI 2.0](/cli/azure/install-az-cli2) installerad och logga in tooan Azure-konto med [az inloggningen](/cli/azure/#login).

## <a name="reset-ssh-key"></a>Återställ SSH-nyckel
hello följande exempel återställs hello SSH-nyckeln för hello användare `azureuser` på hello virtuella datorn med namnet `myVM`:

```azurecli
az vm user update \
  --resource-group myResourceGroup \
  --name myVM \
  --username azureuser \
  --ssh-key-value ~/.ssh/id_rsa.pub
```

## <a name="reset-password"></a>Återställa lösenord
hello följande exempel återställs hello användarlösenord hello `azureuser` på hello virtuella datorn med namnet `myVM`:

```azurecli
az vm user update \
  --resource-group myResourceGroup \
  --name myVM \
  --username azureuser \
  --password myNewPassword
```

## <a name="restart-ssh"></a>Starta om SSH
hello följande exempel startar hello SSH-daemon och återställer hello SSH toodefault konfigurationsvärden på en virtuell dator med namnet `myVM`:

```azurecli
az vm user reset-ssh \
  --resource-group myResourceGroup \
  --name myVM
```

## <a name="create-a-user"></a>Skapa en användare
hello följande exempel skapas en användare med namnet `myNewUser` använder en SSH-nyckel för autentisering på hello virtuella datorn med namnet `myVM`:

```azurecli
az vm user update \
  --resource-group myResourceGroup \
  --name myVM \
  --username myNewUser \
  --ssh-key-value ~/.ssh/id_rsa.pub
```

## <a name="delete-a-user"></a>Tar bort en användare
hello följande exempel tar bort en användare med namnet `myNewUser` på hello virtuella datorn med namnet `myVM`:

```azurecli
az vm user delete \
  --resource-group myResourceGroup \
  --name myVM \
  --username myNewUser
```


## <a name="use-json-files-and-hello-vmaccess-extension"></a>Använd JSON-filer och hello VMAccess-tillägget
följande exempel hello använda raw JSON-filer. Använd [az vm-tillägget set](/cli/azure/vm/extension#set) toothen anropa JSON-filer. Filerna JSON kallas även från Azure-mallar. 

### <a name="reset-user-access"></a>Återställ användaråtkomst
Om du har förlorat åtkomst tooroot på Linux-VM, kan du starta en VMAccess skriptet tooreset SSH-nyckeln eller lösenordet för en användare.

tooreset hello offentlig SSH-nyckel för en användare skapar en fil med namnet `reset_ssh_key.json` och lägga till inställningarna i hello följande format. Ersätt värdena för hello `username` och `ssh_key` parametrar:

```json
{
  "username":"azureuser",
  "ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNGxxxxxx2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7xxxxxxwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5wxxtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== azureuser@myVM"
}
```

Kör hello VMAccess skriptet med:

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings reset_ssh_key.json
```

tooreset ett lösenord, skapa en fil med namnet `reset_user_password.json` och lägga till inställningarna i hello följande format. Ersätt värdena för hello `username` och `password` parametrar:

```json
{
  "username":"azureuser",
  "password":"myNewPassword" 
}
```

Kör hello VMAccess skriptet med:

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings reset_user_password.json
```

### <a name="restart-ssh"></a>Starta om SSH
toorestart hello SSH-daemon och återställa hello SSH konfigurationsvärden toodefault, skapa en fil med namnet `reset_sshd.json`. Lägg till hello följande innehåll:

```json
{
  "reset_ssh": true
}
```

Kör hello VMAccess skriptet med:

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings reset_sshd.json
```

### <a name="manage-users"></a>Hantera användare

toocreate en användare som använder en SSH-nyckel för autentisering, skapa en fil med namnet `create_new_user.json` och lägga till inställningarna i hello följande format. Ersätt värdena för hello `username` och `ssh_key` parametrar:

```json
{
  "username":"myNewUser",
  "ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNG1vHY7P2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7iUo5IdwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5woYtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== myNewUser@myVM",
  "password":"myNewUserPassword"
}
```

Kör hello VMAccess skriptet med:

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings create_new_user.json
```

toodelete en användare skapar en fil med namnet `delete_user.json` och Lägg till hello följande innehåll. Ersätt värdet för hello `remove_user` parameter:

```json
{
  "remove_user":"myNewUser"
}
```

Kör hello VMAccess skriptet med:

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings delete_user.json
```

### <a name="check-or-repair-hello-disk"></a>Kontrollera eller reparera hello disk
Med hjälp av VMAccess kan också kontrollera och reparera en disk som du lagt till toohello Linux VM.

toocheck och reparera hello disk, skapa en fil med namnet `disk_check_repair.json` och lägga till inställningarna i hello följande format. Ersätt värdet för hello namnet på `repair_disk`:

```json
{
  "check_disk": "true",
  "repair_disk": "true, mydiskname"
}
```

Kör hello VMAccess skriptet med:

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings disk_check_repair.json
```

## <a name="next-steps"></a>Nästa steg
Uppdatera Linux är med hjälp av hello Azure VMAccess-tillägget en metod toomake ändringar på en Linux-VM som körs. Du kan också använda verktyg som molntjänster init och Azure Resource Manager mallar toomodify din Linux VM på Start.

[Tillägg för virtuell dator och funktioner för Linux](extensions-features.md)

[Skapa Azure Resource Manager-mallar med Linux VM-tillägg](../windows/template-description.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[Med hjälp av molnet init toocustomize en Linux VM under skapande av](using-cloud-init.md)

