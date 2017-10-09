---
title: "aaaMove filer tooand från virtuella Azure Linux-datorer med SCP | Microsoft Docs"
description: "Flytta filer tooand på ett säkert sätt från en Linux VM i Azure med hjälp av SCP och en SSH-nyckel."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: danlep
ms.openlocfilehash: 9e4dce667aa581df74aec3d45a6ba5e52f777d1c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="move-files-tooand-from-a-linux-vm-using-scp"></a>Flytta filer tooand från en Linux VM som använder SCP

Den här artikeln visar hur toomove filer från din arbetsstation in tooan virtuella Azure Linux-datorn, eller från en Azure Linux VM ned tooyour arbetsstation, som använder säker kopia (SCP). Flytta filer mellan din arbetsstation och en Linux VM, snabbt och säkert är viktigt för att hantera dina Azure-infrastrukturen. 

För den här artikeln behöver du Linux VM som distribuerats i Azure med hjälp av [SSH offentliga och privata nyckelfiler](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Du behöver också en SCP-klient för den lokala datorn. Det är byggda på SSH och ingår i hello Bash standardgränssnittet för de flesta Linux och Mac-datorer och vissa Windows-gränssnitt.

## <a name="quick-commands"></a>Snabbkommandon

Kopiera en fil in toohello Linux VM

```bash
scp file azureuser@azurehost:directory/targetfile
```

Kopiera en fil från hello Linux VM

```bash
scp azureuser@azurehost:directory/file targetfile
```

## <a name="detailed-walkthrough"></a>Detaljerad genomgång

Som exempel kan vi flytta en Azure-konfigurationsfil in tooa Linux VM och hämtar en loggfilskatalog både med SCP och SSH-nycklar.   

## <a name="ssh-key-pair-authentication"></a>SSH-nyckelpar autentisering

SCP använder SSH för hello Transportskiktet. SSH handtag hello autentisering på målvärden för hello och flyttas hello-fil i en krypterad som standard med SSH-tunnel. Användarnamn och lösenord kan användas för SSH-autentisering. SSH offentliga och privata nycklar autentisering bör dock som en säkerhetsåtgärd. När SSH har autentiserats hello anslutning börjar SCP sedan kopiera hello-filen. Med en korrekt konfigurerad `~/.ssh/config` och SSH offentliga och privata nycklar, hello SCP-anslutning kan upprättas med bara ett servernamn (eller IP-adress). Om du bara har en SSH-nyckeln SCP söker efter den i hello `~/.ssh/` directory, och använder som standard toolog i toohello VM.

Mer information om hur du konfigurerar din `~/.ssh/config` och offentliga och privata nycklar för SSH, se [skapa SSH-nycklar](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="scp-a-file-tooa-linux-vm"></a>SCP fil-tooa Linux VM

Exempelvis hello första kopiera vi en Azure-konfigurationsfil in tooa Linux VM som används toodeploy automation. Säkerhet är viktigt eftersom den här filen innehåller autentiseringsuppgifter för Azure API, bland annat hemligheter. hello krypterade tunnel som tillhandahålls av SSH skyddar hello innehållet i hello-fil.

Hej följande kommando kopior hello lokala *.azure/config* filen tooan virtuella Azure-datorn med FQDN *myserver.eastus.cloudapp.azure.com*. hello administratörsanvändarnamnet på hello Azure VM är *azureuser* . hello-filen är riktade toohello */home/azureuser/* directory. Ersätt värdena i det här kommandot.

```bash
scp ~/.azure/config azureuser@myserver.eastus.cloudapp.com:/home/azureuser/config
```

## <a name="scp-a-directory-from-a-linux-vm"></a>SCP en katalog Linux VM

I det här exemplet kopierar vi en katalog med loggfiler från hello Linux VM ned tooyour arbetsstation. En loggfil kan eller inte kan innehålla känsliga eller hemliga data. Med Tjänstanslutningspunkten innebär dock hello innehållet i loggfiler hello krypteras. Med hjälp av SCP tootransfer hello filer är hello enklaste sättet tooget hello log-katalogen och filerna ned tooyour arbetsstation samtidigt säker.

hello följande kommando kopierar filer i hello */home/azureuser/logs/* på hello Azure VM toohello lokala tmp:

```bash
scp -r azureuser@myserver.eastus.cloudapp.com:/home/azureuser/logs/. /tmp/
```

Hej `-r` cli flaggan instruerar SCP toorecursively kopiera hello filer och kataloger från hello punkt hello katalogen som anges i hello-kommandot.  Observera att hello kommandoradssyntaxen är också liknande tooa `cp` kopiera kommandot.

## <a name="next-steps"></a>Nästa steg

* [Hantera användare, SSH och kontrollera eller reparera diskar på virtuella Azure Linux-datorer med hjälp av hello VMAccess-tillägget](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
