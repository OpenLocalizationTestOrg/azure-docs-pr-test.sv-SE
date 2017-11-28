---
title: "frågor för Linux virtuella datorer i Azure och aaaFrequently | Microsoft Docs"
description: "Ger svar toosome hello vanliga frågor om Linux virtuella datorer som skapats med hello Resource Manager-modellen."
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-management
ms.assetid: 3648e09c-1115-4818-93c6-688d7a54a353
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: cynthn
ms.openlocfilehash: 0afd08123dddc408851065c46deedc3146dbec20
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="frequently-asked-question-about-linux-virtual-machines"></a><span data-ttu-id="e25f9-103">Vanliga frågor och svar om virtuella Linux-datorer</span><span class="sxs-lookup"><span data-stu-id="e25f9-103">Frequently asked question about Linux Virtual Machines</span></span>
<span data-ttu-id="e25f9-104">Den här artikeln tar några vanliga frågor om Linux virtuella datorer som skapats i Azure med hjälp av hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="e25f9-104">This article addresses some common questions about Linux virtual machines created in Azure using hello Resource Manager deployment model.</span></span> <span data-ttu-id="e25f9-105">Hello Windows version av det här avsnittet finns [vanliga frågor om Windows-datorer](../windows/faq.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="e25f9-105">For hello Windows version of this topic, see [Frequently asked question about Windows Virtual Machines](../windows/faq.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)</span></span>

## <a name="what-can-i-run-on-an-azure-vm"></a><span data-ttu-id="e25f9-106">Vad kan jag köra på en virtuell Azure-dator?</span><span class="sxs-lookup"><span data-stu-id="e25f9-106">What can I run on an Azure VM?</span></span>
<span data-ttu-id="e25f9-107">Alla prenumeranter kan köra serverprogramvara på en virtuell Azure-dator.</span><span class="sxs-lookup"><span data-stu-id="e25f9-107">All subscribers can run server software on an Azure virtual machine.</span></span> <span data-ttu-id="e25f9-108">Mer information finns i [Linux på Azure-Endorsed distributioner](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="e25f9-108">For more information, see [Linux on Azure-Endorsed Distributions](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span></span>

## <a name="how-much-storage-can-i-use-with-a-virtual-machine"></a><span data-ttu-id="e25f9-109">Hur mycket lagringsutrymme kan jag använda med en virtuell dator?</span><span class="sxs-lookup"><span data-stu-id="e25f9-109">How much storage can I use with a virtual machine?</span></span>
<span data-ttu-id="e25f9-110">Varje datadisk kan vara upp too1 TB.</span><span class="sxs-lookup"><span data-stu-id="e25f9-110">Each data disk can be up too1 TB.</span></span> <span data-ttu-id="e25f9-111">hello antalet datadiskar som du kan använda beror på hello storleken på hello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="e25f9-111">hello number of data disks you can use depends on hello size of hello virtual machine.</span></span> <span data-ttu-id="e25f9-112">Mer information finns i [Storlekar för virtuella datorer](sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="e25f9-112">For details, see [Sizes for Virtual Machines](sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="e25f9-113">Ett Azure storage-konto innehåller lagring för hello operativsystemdisken och eventuella hårddiskar.</span><span class="sxs-lookup"><span data-stu-id="e25f9-113">An Azure storage account provides storage for hello operating system disk and any data disks.</span></span> <span data-ttu-id="e25f9-114">Varje disk är en VHD-fil som lagras som en sidblob.</span><span class="sxs-lookup"><span data-stu-id="e25f9-114">Each disk is a .vhd file stored as a page blob.</span></span> <span data-ttu-id="e25f9-115">Information om priser finns i [Information om lagringspriser](https://azure.microsoft.com/pricing/details/storage/).</span><span class="sxs-lookup"><span data-stu-id="e25f9-115">For pricing details, see [Storage Pricing Details](https://azure.microsoft.com/pricing/details/storage/).</span></span>

## <a name="how-can-i-access-my-virtual-machine"></a><span data-ttu-id="e25f9-116">Hur kan jag använda min virtuella dator?</span><span class="sxs-lookup"><span data-stu-id="e25f9-116">How can I access my virtual machine?</span></span>
<span data-ttu-id="e25f9-117">Upprätta en anslutning till toolog på toohello virtuell dator med hjälp av SSH (Secure Shell).</span><span class="sxs-lookup"><span data-stu-id="e25f9-117">Establish a remote connection toolog on toohello virtual machine, using Secure Shell (SSH).</span></span> <span data-ttu-id="e25f9-118">Se hello instruktioner om hur tooconnect [från Windows](ssh-from-windows.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) eller [från Linux och Mac](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="e25f9-118">See hello instructions on how tooconnect [from Windows](ssh-from-windows.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) or [from Linux and Mac](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="e25f9-119">Som standard tillåter SSH högst 10 samtidiga anslutningar.</span><span class="sxs-lookup"><span data-stu-id="e25f9-119">By default, SSH allows a maximum of 10 concurrent connections.</span></span> <span data-ttu-id="e25f9-120">Du kan öka detta antal genom att redigera hello konfigurationsfilen.</span><span class="sxs-lookup"><span data-stu-id="e25f9-120">You can increase this number by editing hello configuration file.</span></span>

<span data-ttu-id="e25f9-121">Om du får problem, kolla [felsökning av SSH (Secure Shell) anslutningar](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="e25f9-121">If you’re having problems, check out [Troubleshoot Secure Shell (SSH) connections](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <a name="can-i-use-hello-temporary-disk-devsdb1-toostore-data"></a><span data-ttu-id="e25f9-122">Kan jag använda hello diskutrymme (/ dev/sdb1) toostore data?</span><span class="sxs-lookup"><span data-stu-id="e25f9-122">Can I use hello temporary disk (/dev/sdb1) toostore data?</span></span>
<span data-ttu-id="e25f9-123">Använd inte hello diskutrymme (/ dev/sdb1) toostore data.</span><span class="sxs-lookup"><span data-stu-id="e25f9-123">Don't use hello temporary disk (/dev/sdb1) toostore data.</span></span> <span data-ttu-id="e25f9-124">Det är bara det för tillfällig lagring.</span><span class="sxs-lookup"><span data-stu-id="e25f9-124">It is only there for temporary storage.</span></span> <span data-ttu-id="e25f9-125">Riskerar du att förlora data som inte kan återställas.</span><span class="sxs-lookup"><span data-stu-id="e25f9-125">You risk losing data that can’t be recovered.</span></span>

## <a name="can-i-copy-or-clone-an-existing-azure-vm"></a><span data-ttu-id="e25f9-126">Kan jag kopiera eller klona en befintlig Azure VM?</span><span class="sxs-lookup"><span data-stu-id="e25f9-126">Can I copy or clone an existing Azure VM?</span></span>
<span data-ttu-id="e25f9-127">Ja.</span><span class="sxs-lookup"><span data-stu-id="e25f9-127">Yes.</span></span> <span data-ttu-id="e25f9-128">Instruktioner finns i [hur hello toocreate en kopia av en virtuell Linux-dator i Resource Manager-distributionsmodellen](copy-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="e25f9-128">For instructions, see [How toocreate a copy of a Linux virtual machine in hello Resource Manager deployment model](copy-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <a name="why-am-i-not-seeing-canada-central-and-canada-east-regions-through-azure-resource-manager"></a><span data-ttu-id="e25f9-129">Varför inte visas Kanada Central och Kanada Öst regioner via Azure Resource Manager?</span><span class="sxs-lookup"><span data-stu-id="e25f9-129">Why am I not seeing Canada Central and Canada East regions through Azure Resource Manager?</span></span>
<span data-ttu-id="e25f9-130">hello två nya regionerna Kanada Central och Kanada Öst registreras inte automatiskt för att skapa en virtuell dator för befintliga Azure-prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="e25f9-130">hello two new regions of Canada Central and Canada East are not automatically registered for virtual machine creation for existing Azure subscriptions.</span></span> <span data-ttu-id="e25f9-131">Denna registrering sker automatiskt när en virtuell dator distribueras via hello Azure portal tooany andra region med Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="e25f9-131">This registration is done automatically when a virtual machine is deployed through hello Azure portal tooany other region using Azure Resource Manager.</span></span> <span data-ttu-id="e25f9-132">När en virtuell dator är distribuerade tooany andra Azure-region hello nya områden ska vara tillgängligt för följande virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="e25f9-132">After a virtual machine is deployed tooany other Azure region, hello new regions should be available for subsequent virtual machines.</span></span>

## <a name="can-i-add-a-nic-toomy-vm-after-its-created"></a><span data-ttu-id="e25f9-133">Kan jag lägga till NIC-toomy VM när den har skapats?</span><span class="sxs-lookup"><span data-stu-id="e25f9-133">Can I add a NIC toomy VM after it's created?</span></span>
<span data-ttu-id="e25f9-134">Ja, det är nu möjligt.</span><span class="sxs-lookup"><span data-stu-id="e25f9-134">Yes, this is now possible.</span></span> <span data-ttu-id="e25f9-135">hello VM första behov toobe stoppats frigjord.</span><span class="sxs-lookup"><span data-stu-id="e25f9-135">hello VM first needs toobe stopped deallocated.</span></span> <span data-ttu-id="e25f9-136">Sedan kan du lägga till eller ta bort ett nätverkskort (om det inte är hello senaste nätverkskortet på hello VM).</span><span class="sxs-lookup"><span data-stu-id="e25f9-136">Then you can add or remove a NIC (unless it's hello last NIC on hello VM).</span></span> 

## <a name="are-there-any-computer-name-requirements"></a><span data-ttu-id="e25f9-137">Finns det några kraven för dator?</span><span class="sxs-lookup"><span data-stu-id="e25f9-137">Are there any computer name requirements?</span></span>
<span data-ttu-id="e25f9-138">Ja.</span><span class="sxs-lookup"><span data-stu-id="e25f9-138">Yes.</span></span> <span data-ttu-id="e25f9-139">hello datornamnet får innehålla högst 64 tecken.</span><span class="sxs-lookup"><span data-stu-id="e25f9-139">hello computer name can be a maximum of 64 characters in length.</span></span> <span data-ttu-id="e25f9-140">Se [Naming conventions regler och begränsningar](/architecture/best-practices/naming-conventions#naming-rules-and-restrictions?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) mer information om namngivning av dina resurser.</span><span class="sxs-lookup"><span data-stu-id="e25f9-140">See [Naming conventions rules and restrictions](/architecture/best-practices/naming-conventions#naming-rules-and-restrictions?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) for more information around naming your resources.</span></span>

## <a name="are-there-any-resource-group-name-requirements"></a><span data-ttu-id="e25f9-141">Finns det någon resurs kraven för gruppen?</span><span class="sxs-lookup"><span data-stu-id="e25f9-141">Are there any resource group name requirements?</span></span>
<span data-ttu-id="e25f9-142">Ja.</span><span class="sxs-lookup"><span data-stu-id="e25f9-142">Yes.</span></span> <span data-ttu-id="e25f9-143">hello resursgruppens namn kan vara upp till 90 tecken långt.</span><span class="sxs-lookup"><span data-stu-id="e25f9-143">hello resource group name can be a maximum of 90 characters in length.</span></span> <span data-ttu-id="e25f9-144">Se [Naming conventions regler och begränsningar](/architecture/best-practices/naming-conventions#naming-rules-and-restrictions?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) mer information om resursgrupper.</span><span class="sxs-lookup"><span data-stu-id="e25f9-144">See [Naming conventions rules and restrictions](/architecture/best-practices/naming-conventions#naming-rules-and-restrictions?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) for more information about resource groups.</span></span>

## <a name="what-are-hello-username-requirements-when-creating-a-vm"></a><span data-ttu-id="e25f9-145">Vilka är kraven för hello användarnamn när du skapar en virtuell dator?</span><span class="sxs-lookup"><span data-stu-id="e25f9-145">What are hello username requirements when creating a VM?</span></span>
<span data-ttu-id="e25f9-146">Användarnamn måste vara 1 och 64 tecken.</span><span class="sxs-lookup"><span data-stu-id="e25f9-146">Usernames must be 1 - 64 characters in length.</span></span>

<span data-ttu-id="e25f9-147">hello efter användarnamn är inte tillåtna:</span><span class="sxs-lookup"><span data-stu-id="e25f9-147">hello following usernames are not allowed:</span></span>

<table>
    <tr>
        <td style="text-align:center"><span data-ttu-id="e25f9-148">Administratören</span><span class="sxs-lookup"><span data-stu-id="e25f9-148">administrator</span></span> </td><td style="text-align:center"> <span data-ttu-id="e25f9-149">Admin</span><span class="sxs-lookup"><span data-stu-id="e25f9-149">admin</span></span> </td><td style="text-align:center"> <span data-ttu-id="e25f9-150">Användaren</span><span class="sxs-lookup"><span data-stu-id="e25f9-150">user</span></span> </td><td style="text-align:center"> <span data-ttu-id="e25f9-151">Användare1</span><span class="sxs-lookup"><span data-stu-id="e25f9-151">user1</span></span></td>
    </tr>
    <tr>
        <td style="text-align:center"><span data-ttu-id="e25f9-152">Test</span><span class="sxs-lookup"><span data-stu-id="e25f9-152">test</span></span> </td><td style="text-align:center"> <span data-ttu-id="e25f9-153">Användare2</span><span class="sxs-lookup"><span data-stu-id="e25f9-153">user2</span></span> </td><td style="text-align:center"> <span data-ttu-id="e25f9-154">test1</span><span class="sxs-lookup"><span data-stu-id="e25f9-154">test1</span></span> </td><td style="text-align:center"> <span data-ttu-id="e25f9-155">USER3</span><span class="sxs-lookup"><span data-stu-id="e25f9-155">user3</span></span></td>
    </tr>
    <tr>
        <td style="text-align:center"><span data-ttu-id="e25f9-156">admin1</span><span class="sxs-lookup"><span data-stu-id="e25f9-156">admin1</span></span> </td><td style="text-align:center"> <span data-ttu-id="e25f9-157">1</span><span class="sxs-lookup"><span data-stu-id="e25f9-157">1</span></span> </td><td style="text-align:center"> <span data-ttu-id="e25f9-158">123</span><span class="sxs-lookup"><span data-stu-id="e25f9-158">123</span></span> </td><td style="text-align:center"> <span data-ttu-id="e25f9-159">A</span><span class="sxs-lookup"><span data-stu-id="e25f9-159">a</span></span></td>
    </tr>
    <tr>
        <td style="text-align:center"><span data-ttu-id="e25f9-160">actuser</span><span class="sxs-lookup"><span data-stu-id="e25f9-160">actuser</span></span>  </td><td style="text-align:center"> <span data-ttu-id="e25f9-161">ADM</span><span class="sxs-lookup"><span data-stu-id="e25f9-161">adm</span></span> </td><td style="text-align:center"> <span data-ttu-id="e25f9-162">admin2</span><span class="sxs-lookup"><span data-stu-id="e25f9-162">admin2</span></span> </td><td style="text-align:center"> <span data-ttu-id="e25f9-163">ASPNET</span><span class="sxs-lookup"><span data-stu-id="e25f9-163">aspnet</span></span></td>
    </tr>
    <tr>
        <td style="text-align:center"><span data-ttu-id="e25f9-164">säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="e25f9-164">backup</span></span> </td><td style="text-align:center"> <span data-ttu-id="e25f9-165">Konsolen</span><span class="sxs-lookup"><span data-stu-id="e25f9-165">console</span></span> </td><td style="text-align:center"> <span data-ttu-id="e25f9-166">David</span><span class="sxs-lookup"><span data-stu-id="e25f9-166">david</span></span> </td><td style="text-align:center"> <span data-ttu-id="e25f9-167">Gäst</span><span class="sxs-lookup"><span data-stu-id="e25f9-167">guest</span></span></td>
    </tr>
    <tr>
        <td style="text-align:center"><span data-ttu-id="e25f9-168">John</span><span class="sxs-lookup"><span data-stu-id="e25f9-168">john</span></span> </td><td style="text-align:center"> <span data-ttu-id="e25f9-169">Ägare</span><span class="sxs-lookup"><span data-stu-id="e25f9-169">owner</span></span> </td><td style="text-align:center"> <span data-ttu-id="e25f9-170">rot</span><span class="sxs-lookup"><span data-stu-id="e25f9-170">root</span></span> </td><td style="text-align:center"> <span data-ttu-id="e25f9-171">server</span><span class="sxs-lookup"><span data-stu-id="e25f9-171">server</span></span></td>
    </tr>
    <tr>
        <td style="text-align:center"><span data-ttu-id="e25f9-172">SQL</span><span class="sxs-lookup"><span data-stu-id="e25f9-172">sql</span></span> </td><td style="text-align:center"> <span data-ttu-id="e25f9-173">Support</span><span class="sxs-lookup"><span data-stu-id="e25f9-173">support</span></span> </td><td style="text-align:center"> <span data-ttu-id="e25f9-174">support_388945a0</span><span class="sxs-lookup"><span data-stu-id="e25f9-174">support_388945a0</span></span> </td><td style="text-align:center"> <span data-ttu-id="e25f9-175">sys</span><span class="sxs-lookup"><span data-stu-id="e25f9-175">sys</span></span></td>
    </tr>
    <tr>
        <td style="text-align:center"><span data-ttu-id="e25f9-176">Test2</span><span class="sxs-lookup"><span data-stu-id="e25f9-176">test2</span></span> </td><td style="text-align:center"> <span data-ttu-id="e25f9-177">test3</span><span class="sxs-lookup"><span data-stu-id="e25f9-177">test3</span></span> </td><td style="text-align:center"> <span data-ttu-id="e25f9-178">Användare4 lade</span><span class="sxs-lookup"><span data-stu-id="e25f9-178">user4</span></span> </td><td style="text-align:center"> <span data-ttu-id="e25f9-179">user5</span><span class="sxs-lookup"><span data-stu-id="e25f9-179">user5</span></span></td>
    </tr>
</table>


## <a name="what-are-hello-password-requirements-when-creating-a-vm"></a><span data-ttu-id="e25f9-180">Vilka är kraven för hello lösenord när du skapar en virtuell dator?</span><span class="sxs-lookup"><span data-stu-id="e25f9-180">What are hello password requirements when creating a VM?</span></span>
<span data-ttu-id="e25f9-181">Lösenord måste vara 6 72 tecken och uppfylla 3 av följande 4 krav på komplexitet hello:</span><span class="sxs-lookup"><span data-stu-id="e25f9-181">Passwords must be 6 - 72 characters in length and meet 3 out of hello following 4 complexity requirements:</span></span>

* <span data-ttu-id="e25f9-182">Har lägre tecken</span><span class="sxs-lookup"><span data-stu-id="e25f9-182">Have lower characters</span></span>
* <span data-ttu-id="e25f9-183">Övre tecken</span><span class="sxs-lookup"><span data-stu-id="e25f9-183">Have upper characters</span></span>
* <span data-ttu-id="e25f9-184">Har en siffra</span><span class="sxs-lookup"><span data-stu-id="e25f9-184">Have a digit</span></span>
* <span data-ttu-id="e25f9-185">Har ett specialtecken (Regex matcha [\W_])</span><span class="sxs-lookup"><span data-stu-id="e25f9-185">Have a special character (Regex match [\W_])</span></span>

<span data-ttu-id="e25f9-186">hello efter lösenord tillåts inte:</span><span class="sxs-lookup"><span data-stu-id="e25f9-186">hello following passwords are not allowed:</span></span>

<table>
    <tr>
        <td style="text-align:center">abc@123</td>
        <td style="text-align:center"><span data-ttu-id="e25f9-187">P@$$w0rd</span><span class="sxs-lookup"><span data-stu-id="e25f9-187">P@$$w0rd</span></span></td>
        <td style="text-align:center">P@ssw0rd</td>
        <td style="text-align:center">P@ssword123</td>
        <td style="text-align:center"><span data-ttu-id="e25f9-188">Pa$ $word</span><span class="sxs-lookup"><span data-stu-id="e25f9-188">Pa$$word</span></span></td>
    </tr>
    <tr>
        <td style="text-align:center">pass@word1</td>
        <td style="text-align:center"><span data-ttu-id="e25f9-189">Lösenord!</span><span class="sxs-lookup"><span data-stu-id="e25f9-189">Password!</span></span></td>
        <td style="text-align:center"><span data-ttu-id="e25f9-190">Password1</span><span class="sxs-lookup"><span data-stu-id="e25f9-190">Password1</span></span></td>
        <td style="text-align:center"><span data-ttu-id="e25f9-191">Password22</span><span class="sxs-lookup"><span data-stu-id="e25f9-191">Password22</span></span></td>
        <td style="text-align:center"><span data-ttu-id="e25f9-192">ILOVEYOU!</span><span class="sxs-lookup"><span data-stu-id="e25f9-192">iloveyou!</span></span></td>
    </tr>
</table>
