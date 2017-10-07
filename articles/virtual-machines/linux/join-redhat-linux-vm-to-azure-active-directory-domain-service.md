---
title: aaaJoin RedHat Linux VM-tooan Azure Active Directory DS | Microsoft Docs
description: Hur toojoin en befintlig RedHat Enterprise Linux 7 VM tooan Azure Active Directory Domain Service.
services: virtual-machines-linux
documentationcenter: virtual-machines-linux
author: vlivech
manager: timlt
editor: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/14/2016
ms.author: v-livech
ms.openlocfilehash: f3ba3c764e253191753f1cc5fc8c3b85c53818af
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="join-a-redhat-linux-vm-tooan-azure-active-directory-domain-service"></a>Ansluta till en RedHat Linux VM tooan Azure Active Directory Domain Service

Den här artikeln visar hur toojoin en Azure Active Directory Domain Services (AADDS) med Red Hat Enterprise Linux (RHEL) 7 virtuella tooan hanterade domän.  hello kraven är:

- [ett Azure-konto](https://azure.microsoft.com/pricing/free-trial/)

- [offentliga och privata SSH-nyckelfiler](mac-create-ssh-keys.md)

- [ett Azure Active Directory Domain Services DC](../../active-directory-domain-services/active-directory-ds-getting-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="quick-commands"></a>Snabbkommandon

_Ersätt alla exempel med dina egna inställningar._

### <a name="switch-hello-azure-cli-tooclassic-deployment-mode"></a>Växla hello azure cli tooclassic distribution läge

```azurecli
azure config mode asm
```

### <a name="search-for-a-rhel-version-and-image"></a>Söka efter en RHEL, version och avbildning

```azurecli
azure vm image list | grep "Red Hat"
```

### <a name="create-a-redhat-linux-vm"></a>Skapa en Redhat Linux VM

```azurecli
azure vm create myVM \
-o a879bbefc56a43abb0ce65052aac09f3__RHEL_7_2_Standard_Azure_RHUI-20161026220742 \
-g ahmet \
-p myPassword \
-e 22 \
-t "~/.ssh/id_rsa.pub" \
-z "Small" \
-l "West US"
```

### <a name="ssh-toohello-vm"></a>SSH toohello VM

```bash
ssh -i ~/.ssh/id_rsa ahmet@myVM
```

### <a name="update-yum-packages"></a>YUM uppdateringspaket

```bash
sudo yum update
```

### <a name="install-packages-needed"></a>Installera paket som krävs

```bash
sudo yum -y install realmd sssd krb5-workstation krb5-libs
```

Nu när hello krävs paket som är installerade på hello virtuell Linux-dator, är hello nästa uppgift toojoin hello virtuella toohello hanterad domän.

### <a name="discover-hello-aad-domain-services-managed-domain"></a>Identifiera hello AAD Domain Services-hanterad domän

```bash
sudo realm discover mydomain.com
```

### <a name="initialize-kerberos"></a>Initiera kerberos

Kontrollera att du anger en användare som tillhör toohello ' AAD DC-administratörsgruppen. Endast dessa användare kan ansluta till datorer toohello hanterade domän.

```bash
kinit ahmet@mydomain.com
```

### <a name="join-hello-machine-toohello-domain"></a>Anslut till hello datorn toohello domän

```bash
sudo realm join --verbose mydomain.com -U 'ahmet@mydomain.com'
```

### <a name="verify-hello-machine-is-joined-toohello-domain"></a>Kontrollera hello datorn är domänansluten toohello domän

```bash
ssh -l ahmet@mydomain.com mydomain.cloudapp.net
```

## <a name="next-steps"></a>Nästa steg

* [Red Hat Update infrastruktur (RHUI) för att på begäran Red Hat Enterprise Linux virtuella datorer i Azure](update-infrastructure-redhat.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Ställ in Key Vault för virtuella datorer i Azure Resource Manager](key-vault-setup.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Distribuera och hantera virtuella datorer med hjälp av Azure Resource Manager-mallar och hello Azure CLI](../linux/create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
