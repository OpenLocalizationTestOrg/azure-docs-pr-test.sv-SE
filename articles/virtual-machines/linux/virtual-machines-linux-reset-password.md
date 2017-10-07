---
title: "aaaHow tooreset lokala Linux lösenord på Azure Virtual Machines | Microsoft Docs"
description: "Introducera hello steg tooreset hello lokala Linux lösenord på Azure VM"
services: virtual-machines-linux
documentationcenter: 
author: Deland-Han
manager: cshepard
editor: 
tags: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 7/3/2017
ms.author: delhan
ms.openlocfilehash: 3827e32186c5f034d9bb6fc502dc26708b52a00a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooreset-local-linux-password-on-azure-vms"></a>Hur tooreset lokala Linux lösenord på Azure Virtual Machines

Den här artikeln innehåller flera metoder tooreset lokala Linux virtuella dator (VM) lösenord. Om hello användarkontot har upphört att gälla eller om du bara vill toocreate ett nytt konto, kan du använda följande metoder toocreate ett nytt konto för lokal administratör hello och återfå åtkomst toohello VM.

## <a name="symptoms"></a>Symtom

Du kan inte logga in toohello VM och du får ett meddelande som anger att hello-lösenord som du använde är felaktig. Du kan dessutom använda VMAgent tooreset lösenordet på hello Azure-portalen. 

## <a name="manual-password-reset-procedure"></a>Proceduren för manuell återställning av lösenord

1.  Ta bort hello VM och hålla hello anslutna diskar.

2.  Koppla hello OS-enhet som en data disk tooanother temporal VM i hello samma plats.

3.  Kör hello följa SSH-kommando på hello temporal VM toobecome superanvändare.


    ~~~~
    sudo su
    ~~~~

4.  Kör **fdisk -l** eller titta på system loggar toofind hello nyligen ansluten disk. Leta upp hello enhet namnet toomount. Klicka sedan på hello temporal VM, leta i hello relevanta loggfilen.

    ~~~~
    grep SCSI /var/log/kern.log (ubuntu)
    grep SCSI /var/log/messages (centos, suse, oracle)
    ~~~~

    hello följande är exempel på utdata hello grep-kommando:

    ~~~~
    kernel: [ 9707.100572] sd 3:0:0:0: [sdc] Attached SCSI disk
    ~~~~

5.  Skapa en monteringspunkt kallas **tempmount**.

    ~~~~
    mkdir /tempmount
    ~~~~

6.  Montera hello OS-disk på hello monteringspunkt. Du behöver vanligtvis toomount sdc1 eller sdc2. Detta beror på hello som värd för partitionen i katalogen/etc från hello brutna datordisken.

    ~~~~
    mount /dev/sdc1 /tempmount
    ~~~~

7.  Utför en säkerhetskopiering innan du gör några ändringar:

    ~~~~
    cp /etc/passwd /etc/passwd_orig    
    cp /etc/shadow /etc/shadow_orig    
    cp /tempmount/etc/passwd /etc/passwd
    cp /tempmount/etc/shadow /etc/shadow 
    cp /tempmount/etc/passwd /tempmount/etc/passwd_orig
    cp /tempmount/etc/shadow /tempmount/etc/shadow_orig
    ~~~~

8.  Återställa hello användarens lösenord som du behöver:

    ~~~~
    passwd <<USER>> 
    ~~~~

9.  Flytta hello ändrade filer toohello rätt plats på hello bruten datorns disk.

    ~~~~
    cp /etc/passwd /tempmount/etc/passwd
    cp /etc/shadow /tempmount/etc/shadow
    cp /etc/passwd_orig /etc/passwd
    cp /etc/shadow_orig /etc/shadow
    
10. Go back toohello root and unmount hello disk.

    ~~~~
    CD / umount /tempmount
    ~~~~

11. Detach hello disk from hello management portal.

12. Recreate hello VM.

## Next steps

* [Troubleshoot Azure VM by attaching OS disk tooanother Azure VM](http://social.technet.microsoft.com/wiki/contents/articles/18710.troubleshoot-azure-vm-by-attaching-os-disk-to-another-azure-vm.aspx)

* [Azure CLI: How toodelete and re-deploy a VM from VHD](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/azure-cli-how-to-delete-and-re-deploy-a-vm-from-vhd/)
