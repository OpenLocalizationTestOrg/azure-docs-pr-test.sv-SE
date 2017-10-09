---
title: aaaFAQ om virtuella Windows-datorer i Azure | Microsoft Docs
description: "Ger svar toosome hello vanliga frågor om Windows-datorer som skapats med hello Resource Manager-modellen."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-management
ms.assetid: 757da816-a050-4889-a010-6f75d7978eb7
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: cynthn
ms.openlocfilehash: ee366a04bda347ce2be07bde4fc6bad306cc1da9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="frequently-asked-question-about-windows-virtual-machines"></a>Vanliga frågor och svar om virtuella Windows-datorer
Den här artikeln tar några vanliga frågor om Windows-datorer som skapats i Azure med hjälp av hello Resource Manager-modellen. Hello Linux version av det här avsnittet finns [vanliga frågor om virtuella Linux-datorer](../linux/faq.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="what-can-i-run-on-an-azure-vm"></a>Vad kan jag köra på en virtuell Azure-dator?
Alla prenumeranter kan köra serverprogramvara på en virtuell Azure-dator. Information om hello Supportpolicy för körs Microsoft server-program i Azure finns [support för Microsoft server-programvara för virtuella datorer i Azure](https://support.microsoft.com/kb/2721672)

Vissa versioner av Windows 7, Windows 8.1 och Windows 10 är tillgängliga tooMSDN Azure förmånen prenumeranter och MSDN Dev / Test-betala per användning prenumeranter för utveckling och testning uppgifter. Mer information, bland annat instruktioner och begränsningar, finns i [Windows-klientavbildningar för MSDN-prenumeranter](http://azure.microsoft.com/blog/2014/05/29/windows-client-images-on-azure/). 

## <a name="how-much-storage-can-i-use-with-a-virtual-machine"></a>Hur mycket lagringsutrymme kan jag använda med en virtuell dator?
Varje datadisk kan vara upp too1 TB. hello antalet datadiskar som du kan använda beror på hello storleken på hello virtuella datorn. Mer information finns i [Storlekar för virtuella datorer](sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Azure hanterade diskar är hello nya och rekommenderade disk lagring erbjudanden för användning med Azure Virtual Machines för beständig lagring av data. Du kan använda flera Managed Disks till varje virtuell dator. Managed Disks tillhandahåller två lagringsalternativ: Premium och Standard. Information om priser finns [hanterade diskar priser](https://azure.microsoft.com/pricing/details/managed-disks).

Azure storage-konton kan också ge lagring för hello operativsystemdisken och eventuella hårddiskar. Varje disk är en VHD-fil som lagras som en sidblob. Information om priser finns i [Information om lagringspriser](https://azure.microsoft.com/pricing/details/storage/).

## <a name="how-can-i-access-my-virtual-machine"></a>Hur kan jag använda min virtuella dator?
Upprätta en anslutning med anslutning till fjärrskrivbord (RDP) för en virtuell Windows-dator. Instruktioner finns i [hur tooconnect och logga in tooan Azure virtuella datorn kör Windows](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Högst två samtidiga anslutningar stöds, om inte hello-servern är konfigurerad som en värd för Remote Desktop Services-sessionen.  

Om du har problem med fjärrskrivbord, se [Felsöka fjärrskrivbord anslutningar tooa Windows-baserade virtuella Azure-datorn](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

Om du är bekant med Hyper-V, kan du söker efter en liknande tooVMConnect för verktyget. Azure erbjuder inte en liknande verktyg eftersom konsolen åtkomst tooa virtuella datorn inte stöds.

## <a name="can-i-use-hello-temporary-disk-hello-d-drive-by-default-toostore-data"></a>Kan jag använda hello diskutrymme (hello D: enheten som standard) toostore data?
Använd inte hello tillfälliga toostore diskdata. Det är bara tillfällig lagring, så riskerar du att förlora data som inte kan återställas. Dataförlust kan inträffa när hello virtuella datorn flyttas tooa annan värd. Ändra storlek på en virtuell dator, är uppdatera hello värd eller ett maskinvarufel på hello värden några av hello orsaker kan flytta en virtuell dator.

Om du har ett program som behöver toouse hello D: enhetsbeteckning kan tilldela du enhetsbeteckningar så att hello diskutrymme använder något annat än D:. Instruktioner finns i [ändra hello enhetsbeteckningen för hello Windows diskutrymme](change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).


## <a name="how-can-i-change-hello-drive-letter-of-hello-temporary-disk"></a>Hur kan jag ändra hello enhetsbeteckningen för hello diskutrymme?
Du kan ändra hello enhetsbeteckning genom att flytta filen till sidan hello och omtilldela enhetsbeteckningar, men du måste toomake att du hello stegen i en viss ordning. Instruktioner finns i [ändra hello enhetsbeteckningen för hello Windows diskutrymme](change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

## <a name="can-i-add-an-existing-vm-tooan-availability-set"></a>Kan jag lägga till en befintlig tillgänglighetsuppsättning för VM-tooan?
Nej. Om du vill att din VM toobe tillhör tillgänglighetsuppsättningen måste toocreate hello VM inom hello. Det finns för närvarande inte en sätt tooadd en VM tooan tillgänglighetsuppsättning när den har skapats.

## <a name="can-i-upload-a-virtual-machine-tooazure"></a>Kan överföra en virtuell dator tooAzure?
Ja. Instruktioner finns i [migrera lokala virtuella datorer tooAzure](on-prem-to-azure.md).

## <a name="can-i-resize-hello-os-disk"></a>Kan jag ändra storlek på hello OS-disken?
Ja. Instruktioner finns i [hur tooexpand hello OS-enhet på en virtuell dator i Azure-resursgrupp](expand-os-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="can-i-copy-or-clone-an-existing-azure-vm"></a>Kan jag kopiera eller klona en befintlig Azure VM?
Ja. Använda hanterade avbildningar kan du skapa en avbildning av en virtuell dator och sedan använda hello avbildningen toobuild flera nya virtuella datorer. Instruktioner finns i [skapa en anpassad avbildning av en virtuell dator](tutorial-custom-images.md).

## <a name="why-am-i-not-seeing-canada-central-and-canada-east-regions-through-azure-resource-manager"></a>Varför inte visas Kanada Central och Kanada Öst regioner via Azure Resource Manager?

hello två nya regionerna Kanada Central och Kanada Öst registreras inte automatiskt för att skapa en virtuell dator för befintliga Azure-prenumerationer. Denna registrering sker automatiskt när en virtuell dator distribueras via hello Azure portal tooany andra region med Azure Resource Manager. När en virtuell dator är distribuerade tooany andra Azure-region hello nya områden ska vara tillgängligt för följande virtuella datorer.

## <a name="does-azure-support-linux-vms"></a>Stöder virtuella Linux-datorer i Azure?
Ja. tooquickly skapa ett Linux VM tootry ut Se [skapar en Linux-VM på Azure med hjälp av hello Portal](../linux/quick-create-portal.md).

## <a name="can-i-add-a-nic-toomy-vm-after-its-created"></a>Kan jag lägga till NIC-toomy VM när den har skapats?
Ja, det är nu möjligt. hello VM första behov toobe stoppats frigjord. Sedan kan du lägga till eller ta bort ett nätverkskort (om det inte är hello senaste nätverkskortet på hello VM). 

## <a name="are-there-any-computer-name-requirements"></a>Finns det några kraven för dator?
Ja. hello datornamnet får innehålla högst 15 tecken. Se [Naming conventions regler och begränsningar](/architecture/best-practices/naming-conventions#naming-rules-and-restrictions?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) mer information om namngivning av dina resurser.

## <a name="are-there-any-resource-group-name-requirements"></a>Finns det någon resurs kraven för gruppen?
Ja. hello resursgruppens namn kan vara upp till 90 tecken långt. Se [Naming conventions regler och begränsningar](/architecture/best-practices/naming-conventions#naming-rules-and-restrictions?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) mer information om resursgrupper.

## <a name="what-are-hello-username-requirements-when-creating-a-vm"></a>Vilka är kraven för hello användarnamn när du skapar en virtuell dator?

Användarnamn kan vara högst 20 tecken långt och får inte sluta med en punkt (””.). 


hello efter användarnamn är inte tillåtna:
<table>
    <tr>
        <td style="text-align:center">Administratören </td><td style="text-align:center"> Admin </td><td style="text-align:center"> Användaren </td><td style="text-align:center"> Användare1</td>
    </tr>
    <tr>
        <td style="text-align:center">Test </td><td style="text-align:center"> Användare2 </td><td style="text-align:center"> test1 </td><td style="text-align:center"> USER3</td>
    </tr>    <tr>
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
Lösenord måste bestå av 12 och 123 tecken och uppfylla 3 av följande 4 krav på komplexitet hello:

* Har lägre tecken
* Övre tecken
* Har en siffra
* Har ett specialtecken (Regex matcha [\W_])

hello efter lösenord tillåts inte:

<table>
    <tr>
        <td>abc@123 </td>
        <td>P@$$w0rd </td>
        <td>P@ssw0rd </td>
        <td>P@ssword123 </td>
        <td>Pa$ $word </td>
    </tr>
    <tr>
        <td>pass@word1 </td>
        <td>Lösenord! </td>
        <td>Password1 </td>
        <td>Password22 </td>
        <td>ILOVEYOU! </td>
    </tr>
</table>
