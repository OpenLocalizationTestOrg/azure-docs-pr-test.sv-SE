---
title: "Snabbstart för aaaAzure moln Shell (förhandsversion) | Microsoft Docs"
description: "Snabbstart för hello Azure Cloud Shell"
services: 
documentationcenter: 
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: juluk
ms.openlocfilehash: e60700b92c10c331910dd8bb3c627fe1a024091c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="quickstart-for-using-hello-azure-cloud-shell"></a>Snabbstart för att använda hello Azure Cloud Shell

Det här dokumentet beskriver hur toouse hello Azure Cloud Shell i hello [Azure-portalen](https://ms.portal.azure.com/).

## <a name="start-cloud-shell"></a>Starta molnet Shell
1. Starta **moln Shell** från hello övre navigeringsfältet av hello Azure-portalen <br>
![](media/shell-icon.png)
2. Välj en prenumeration toocreate ett lagringskonto och Azure-filresursen
3. Välj ”Skapa lagring”

> [!TIP]
> Du autentiseras automatiskt i varje sesssion för Azure CLI 2.0.

### <a name="set-your-subscription"></a>Ange din prenumeration
1. Lista över prenumerationer som du har åtkomst till: <br>
`az account list`
2. Ange din önskade prenumeration: <br>
`az account set --subscription my-subscription-name`

> [!TIP]
> Prenumerationen kommer att sparas för framtida sessioner med hjälp av `/home/<user>/.azure/azureProfile.json`.

### <a name="create-a-resource-group"></a>Skapa en resursgrupp
Skapa en ny resursgrupp i WestUS med namnet ”MyRG”: <br>
`az group create -l westus -n MyRG` <br>

### <a name="create-a-linux-vm"></a>Skapa en virtuell Linux-dator
Skapa en Ubuntu VM i din nya resursgrupp. hello Azure CLI 2.0 skapar SSH-nycklar och installationsprogrammet hello VM med dem. <br>
`az vm create -n my_vm_name -g MyRG --image UbuntuLTS --generate-ssh-keys`

> [!NOTE]
> Hej tooauthenticate för offentliga och privata nycklar som används för den virtuella datorn placeras i `/User/.ssh/id_rsa` och `/User/.ssh/id_rsa.pub` av Azure CLI 2.0 som standard. .Ssh-mappen sparas i dina anslutna Azure-filresursen 5 GB avbildningen.

Ditt användarnamn på den här virtuella datorn kommer att ditt användarnamn som används i molnet Shell ($User@Azure:).

### <a name="ssh-into-your-linux-vm"></a>SSH till den virtuella Linux-datorn
1. Sök efter ditt VM-namn i hello Azure portal sökfältet
2. Klicka på ”Anslut” och kör:`ssh username@ipaddress`

![](media/sshcmd-copy.png)

Du bör se hello Ubuntu Välkommen prompt vid hello SSH anslutningen har upprättats.
![](media/ubuntu-welcome.png)

## <a name="cleaning-up"></a>Rensa 
Ta bort resursgruppen och alla resurser i den: <br>
Kör `az group delete -n MyRG`

## <a name="next-steps"></a>Nästa steg
[Lär dig mer om bestående lagring på molnet Shell](persisting-shell-storage.md) <br>
[Lär dig mer om Azure CLI 2.0](https://docs.microsoft.com/cli/azure/) <br>
[Lär dig mer om Azure File storage](../storage/files/storage-files-introduction.md) <br>