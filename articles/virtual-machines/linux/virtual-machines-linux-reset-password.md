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
# <a name="how-tooreset-local-linux-password-on-azure-vms"></a><span data-ttu-id="b2e06-103">Hur tooreset lokala Linux lösenord på Azure Virtual Machines</span><span class="sxs-lookup"><span data-stu-id="b2e06-103">How tooreset local Linux password on Azure VMs</span></span>

<span data-ttu-id="b2e06-104">Den här artikeln innehåller flera metoder tooreset lokala Linux virtuella dator (VM) lösenord.</span><span class="sxs-lookup"><span data-stu-id="b2e06-104">This article introduces several methods tooreset local Linux Virtual Machine (VM) passwords.</span></span> <span data-ttu-id="b2e06-105">Om hello användarkontot har upphört att gälla eller om du bara vill toocreate ett nytt konto, kan du använda följande metoder toocreate ett nytt konto för lokal administratör hello och återfå åtkomst toohello VM.</span><span class="sxs-lookup"><span data-stu-id="b2e06-105">If hello user account is expired or you just want toocreate a new account, you can use hello following methods toocreate a new local admin account and re-gain access toohello VM.</span></span>

## <a name="symptoms"></a><span data-ttu-id="b2e06-106">Symtom</span><span class="sxs-lookup"><span data-stu-id="b2e06-106">Symptoms</span></span>

<span data-ttu-id="b2e06-107">Du kan inte logga in toohello VM och du får ett meddelande som anger att hello-lösenord som du använde är felaktig.</span><span class="sxs-lookup"><span data-stu-id="b2e06-107">You can't log in toohello VM, and you receive a message that indicates that hello password that you used is incorrect.</span></span> <span data-ttu-id="b2e06-108">Du kan dessutom använda VMAgent tooreset lösenordet på hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="b2e06-108">Additionally, you can't use VMAgent tooreset your password on hello Azure Portal.</span></span> 

## <a name="manual-password-reset-procedure"></a><span data-ttu-id="b2e06-109">Proceduren för manuell återställning av lösenord</span><span class="sxs-lookup"><span data-stu-id="b2e06-109">Manual Password Reset procedure</span></span>

1.  <span data-ttu-id="b2e06-110">Ta bort hello VM och hålla hello anslutna diskar.</span><span class="sxs-lookup"><span data-stu-id="b2e06-110">Delete hello VM and keep hello attached disks.</span></span>

2.  <span data-ttu-id="b2e06-111">Koppla hello OS-enhet som en data disk tooanother temporal VM i hello samma plats.</span><span class="sxs-lookup"><span data-stu-id="b2e06-111">Attach hello OS Drive as a data disk tooanother temporal VM in hello same location.</span></span>

3.  <span data-ttu-id="b2e06-112">Kör hello följa SSH-kommando på hello temporal VM toobecome superanvändare.</span><span class="sxs-lookup"><span data-stu-id="b2e06-112">Run hello following SSH command on hello temporal VM toobecome a super-user.</span></span>


    ~~~~
    sudo su
    ~~~~

4.  <span data-ttu-id="b2e06-113">Kör **fdisk -l** eller titta på system loggar toofind hello nyligen ansluten disk.</span><span class="sxs-lookup"><span data-stu-id="b2e06-113">Run **fdisk -l** or look at system logs toofind hello newly attached disk.</span></span> <span data-ttu-id="b2e06-114">Leta upp hello enhet namnet toomount.</span><span class="sxs-lookup"><span data-stu-id="b2e06-114">Locate hello drive name toomount.</span></span> <span data-ttu-id="b2e06-115">Klicka sedan på hello temporal VM, leta i hello relevanta loggfilen.</span><span class="sxs-lookup"><span data-stu-id="b2e06-115">Then on hello temporal VM, look in hello relevant log file.</span></span>

    ~~~~
    grep SCSI /var/log/kern.log (ubuntu)
    grep SCSI /var/log/messages (centos, suse, oracle)
    ~~~~

    <span data-ttu-id="b2e06-116">hello följande är exempel på utdata hello grep-kommando:</span><span class="sxs-lookup"><span data-stu-id="b2e06-116">hello following is example output of hello grep command:</span></span>

    ~~~~
    kernel: [ 9707.100572] sd 3:0:0:0: [sdc] Attached SCSI disk
    ~~~~

5.  <span data-ttu-id="b2e06-117">Skapa en monteringspunkt kallas **tempmount**.</span><span class="sxs-lookup"><span data-stu-id="b2e06-117">Create a mount point called **tempmount**.</span></span>

    ~~~~
    mkdir /tempmount
    ~~~~

6.  <span data-ttu-id="b2e06-118">Montera hello OS-disk på hello monteringspunkt.</span><span class="sxs-lookup"><span data-stu-id="b2e06-118">Mount hello OS disk on hello mount point.</span></span> <span data-ttu-id="b2e06-119">Du behöver vanligtvis toomount sdc1 eller sdc2.</span><span class="sxs-lookup"><span data-stu-id="b2e06-119">You usually need toomount sdc1 or sdc2.</span></span> <span data-ttu-id="b2e06-120">Detta beror på hello som värd för partitionen i katalogen/etc från hello brutna datordisken.</span><span class="sxs-lookup"><span data-stu-id="b2e06-120">This will depend on hello hosting partition in /etc directory from hello broken machine disk.</span></span>

    ~~~~
    mount /dev/sdc1 /tempmount
    ~~~~

7.  <span data-ttu-id="b2e06-121">Utför en säkerhetskopiering innan du gör några ändringar:</span><span class="sxs-lookup"><span data-stu-id="b2e06-121">Perform a backup before making any changes:</span></span>

    ~~~~
    cp /etc/passwd /etc/passwd_orig    
    cp /etc/shadow /etc/shadow_orig    
    cp /tempmount/etc/passwd /etc/passwd
    cp /tempmount/etc/shadow /etc/shadow 
    cp /tempmount/etc/passwd /tempmount/etc/passwd_orig
    cp /tempmount/etc/shadow /tempmount/etc/shadow_orig
    ~~~~

8.  <span data-ttu-id="b2e06-122">Återställa hello användarens lösenord som du behöver:</span><span class="sxs-lookup"><span data-stu-id="b2e06-122">Reset hello user’s password that you need:</span></span>

    ~~~~
    passwd <<USER>> 
    ~~~~

9.  <span data-ttu-id="b2e06-123">Flytta hello ändrade filer toohello rätt plats på hello bruten datorns disk.</span><span class="sxs-lookup"><span data-stu-id="b2e06-123">Move hello modified files toohello correct location on hello broken machine's disk.</span></span>

    ~~~~
    cp /etc/passwd /tempmount/etc/passwd
    cp /etc/shadow /tempmount/etc/shadow
    cp /etc/passwd_orig /etc/passwd
    cp /etc/shadow_orig /etc/shadow
    
10. Go back toohello root and unmount hello disk.

    ~~~~
    <span data-ttu-id="b2e06-124">CD / umount /tempmount</span><span class="sxs-lookup"><span data-stu-id="b2e06-124">cd / umount /tempmount</span></span>
    ~~~~

11. Detach hello disk from hello management portal.

12. Recreate hello VM.

## Next steps

* [Troubleshoot Azure VM by attaching OS disk tooanother Azure VM](http://social.technet.microsoft.com/wiki/contents/articles/18710.troubleshoot-azure-vm-by-attaching-os-disk-to-another-azure-vm.aspx)

* [Azure CLI: How toodelete and re-deploy a VM from VHD](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/azure-cli-how-to-delete-and-re-deploy-a-vm-from-vhd/)
