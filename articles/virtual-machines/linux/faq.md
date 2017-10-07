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
# <a name="frequently-asked-question-about-linux-virtual-machines"></a>Vanliga frågor och svar om virtuella Linux-datorer
Den här artikeln tar några vanliga frågor om Linux virtuella datorer som skapats i Azure med hjälp av hello Resource Manager-modellen. Hello Windows version av det här avsnittet finns [vanliga frågor om Windows-datorer](../windows/faq.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="what-can-i-run-on-an-azure-vm"></a>Vad kan jag köra på en virtuell Azure-dator?
Alla prenumeranter kan köra serverprogramvara på en virtuell Azure-dator. Mer information finns i [Linux på Azure-Endorsed distributioner](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="how-much-storage-can-i-use-with-a-virtual-machine"></a>Hur mycket lagringsutrymme kan jag använda med en virtuell dator?
Varje datadisk kan vara upp too1 TB. hello antalet datadiskar som du kan använda beror på hello storleken på hello virtuella datorn. Mer information finns i [Storlekar för virtuella datorer](sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Ett Azure storage-konto innehåller lagring för hello operativsystemdisken och eventuella hårddiskar. Varje disk är en VHD-fil som lagras som en sidblob. Information om priser finns i [Information om lagringspriser](https://azure.microsoft.com/pricing/details/storage/).

## <a name="how-can-i-access-my-virtual-machine"></a>Hur kan jag använda min virtuella dator?
Upprätta en anslutning till toolog på toohello virtuell dator med hjälp av SSH (Secure Shell). Se hello instruktioner om hur tooconnect [från Windows](ssh-from-windows.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) eller [från Linux och Mac](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Som standard tillåter SSH högst 10 samtidiga anslutningar. Du kan öka detta antal genom att redigera hello konfigurationsfilen.

Om du får problem, kolla [felsökning av SSH (Secure Shell) anslutningar](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="can-i-use-hello-temporary-disk-devsdb1-toostore-data"></a>Kan jag använda hello diskutrymme (/ dev/sdb1) toostore data?
Använd inte hello diskutrymme (/ dev/sdb1) toostore data. Det är bara det för tillfällig lagring. Riskerar du att förlora data som inte kan återställas.

## <a name="can-i-copy-or-clone-an-existing-azure-vm"></a>Kan jag kopiera eller klona en befintlig Azure VM?
Ja. Instruktioner finns i [hur hello toocreate en kopia av en virtuell Linux-dator i Resource Manager-distributionsmodellen](copy-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="why-am-i-not-seeing-canada-central-and-canada-east-regions-through-azure-resource-manager"></a>Varför inte visas Kanada Central och Kanada Öst regioner via Azure Resource Manager?
hello två nya regionerna Kanada Central och Kanada Öst registreras inte automatiskt för att skapa en virtuell dator för befintliga Azure-prenumerationer. Denna registrering sker automatiskt när en virtuell dator distribueras via hello Azure portal tooany andra region med Azure Resource Manager. När en virtuell dator är distribuerade tooany andra Azure-region hello nya områden ska vara tillgängligt för följande virtuella datorer.

## <a name="can-i-add-a-nic-toomy-vm-after-its-created"></a>Kan jag lägga till NIC-toomy VM när den har skapats?
Ja, det är nu möjligt. hello VM första behov toobe stoppats frigjord. Sedan kan du lägga till eller ta bort ett nätverkskort (om det inte är hello senaste nätverkskortet på hello VM). 

## <a name="are-there-any-computer-name-requirements"></a>Finns det några kraven för dator?
Ja. hello datornamnet får innehålla högst 64 tecken. Se [Naming conventions regler och begränsningar](/architecture/best-practices/naming-conventions#naming-rules-and-restrictions?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) mer information om namngivning av dina resurser.

## <a name="are-there-any-resource-group-name-requirements"></a>Finns det någon resurs kraven för gruppen?
Ja. hello resursgruppens namn kan vara upp till 90 tecken långt. Se [Naming conventions regler och begränsningar](/architecture/best-practices/naming-conventions#naming-rules-and-restrictions?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) mer information om resursgrupper.

## <a name="what-are-hello-username-requirements-when-creating-a-vm"></a>Vilka är kraven för hello användarnamn när du skapar en virtuell dator?
Användarnamn måste vara 1 och 64 tecken.

hello efter användarnamn är inte tillåtna:

<table>
    <tr>
        <td style="text-align:center">Administratören </td><td style="text-align:center"> Admin </td><td style="text-align:center"> Användaren </td><td style="text-align:center"> Användare1</td>
    </tr>
    <tr>
        <td style="text-align:center">Test </td><td style="text-align:center"> Användare2 </td><td style="text-align:center"> test1 </td><td style="text-align:center"> USER3</td>
    </tr>
    <tr>
        <td style="text-align:center">admin1 </td><td style="text-align:center"> 1 </td><td style="text-align:center"> 123 </td><td style="text-align:center"> A</td>
    </tr>
    <tr>
        <td style="text-align:center">actuser  </td><td style="text-align:center"> ADM </td><td style="text-align:center"> admin2 </td><td style="text-align:center"> ASPNET</td>
    </tr>
    <tr>
        <td style="text-align:center">säkerhetskopiering </td><td style="text-align:center"> Konsolen </td><td style="text-align:center"> David </td><td style="text-align:center"> Gäst</td>
    </tr>
    <tr>
        <td style="text-align:center">John </td><td style="text-align:center"> Ägare </td><td style="text-align:center"> rot </td><td style="text-align:center"> server</td>
    </tr>
    <tr>
        <td style="text-align:center">SQL </td><td style="text-align:center"> Support </td><td style="text-align:center"> support_388945a0 </td><td style="text-align:center"> sys</td>
    </tr>
    <tr>
        <td style="text-align:center">Test2 </td><td style="text-align:center"> test3 </td><td style="text-align:center"> Användare4 lade </td><td style="text-align:center"> user5</td>
    </tr>
</table>


## <a name="what-are-hello-password-requirements-when-creating-a-vm"></a>Vilka är kraven för hello lösenord när du skapar en virtuell dator?
Lösenord måste vara 6 72 tecken och uppfylla 3 av följande 4 krav på komplexitet hello:

* Har lägre tecken
* Övre tecken
* Har en siffra
* Har ett specialtecken (Regex matcha [\W_])

hello efter lösenord tillåts inte:

<table>
    <tr>
        <td style="text-align:center">abc@123</td>
        <td style="text-align:center">P@$$w0rd</td>
        <td style="text-align:center">P@ssw0rd</td>
        <td style="text-align:center">P@ssword123</td>
        <td style="text-align:center">Pa$ $word</td>
    </tr>
    <tr>
        <td style="text-align:center">pass@word1</td>
        <td style="text-align:center">Lösenord!</td>
        <td style="text-align:center">Password1</td>
        <td style="text-align:center">Password22</td>
        <td style="text-align:center">ILOVEYOU!</td>
    </tr>
</table>
