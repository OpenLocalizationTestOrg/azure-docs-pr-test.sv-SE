---
title: "aaaDetailed SSH felsökning för en virtuell dator i Azure | Microsoft Docs"
description: "Mer detaljerad SSH felsökningssteg för problem som ansluter tooan virtuella Azure-datorn"
keywords: SSH anslutningen nekades, ssh fel, azure ssh, SSH-anslutning misslyckades
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: top-support-issue,azure-service-management,azure-resource-manager
ms.assetid: b8e8be5f-e8a6-489d-9922-9df8de32e839
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: support-article
ms.date: 07/06/2017
ms.author: iainfou
ms.openlocfilehash: 3f711e53a8251f8c06dbb589a258222566a4ae1e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="detailed-ssh-troubleshooting-steps-for-issues-connecting-tooa-linux-vm-in-azure"></a>Detaljerad SSH felsökningssteg för problem som ansluter tooa Linux VM i Azure
Det finns många möjliga orsaker att hello SSH-klienten inte kan tooreach hello SSH-tjänsten på hello VM. Om du har följt via hello mer [allmänna SSH felsökningssteg](troubleshoot-ssh-connection.md), behöver du toofurther felsöka hello anslutningsproblem. Den här artikeln hjälper dig att detaljerad felsökning steg toodetermine där hello SSH-anslutningen inte fungerar och hur tooresolve den.

## <a name="take-preliminary-steps"></a>Vidta åtgärder för preliminär
hello visar följande diagram hello-komponenter som ingår.

![Diagram som visar komponenter för SSH-tjänsten](./media/detailed-troubleshoot-ssh-connection/ssh-tshoot1.png)

hello kan följande steg du isolera hello källan för hello fel och lösningar eller lösningar.

1. Kontrollera hello status hello VM i hello-portalen.
   I hello [Azure-portalen](https://portal.azure.com)väljer **virtuella datorer** > *namn på virtuell*.

   hello statusruta för hello VM ska visa **kör**. Rulla ned tooshow senaste aktiviteten för bearbetning, lagring och nätverksresurser.

2. Välj **inställningar** tooexamine slutpunkter, IP-adresser, nätverkssäkerhetsgrupper och andra inställningar.

   hello VM ska ha en slutpunkt som definierats för SSH-trafik som du kan visa i **slutpunkter** eller  **[nätverkssäkerhetsgruppen](../../virtual-network/virtual-networks-nsg.md)**. Slutpunkter i virtuella datorer som har skapats med hjälp av hanteraren för filserverresurser lagras i en nätverkssäkerhetsgrupp. Kontrollera också att hello regler har tillämpade toohello nätverkssäkerhetsgruppen och att de är refereras i hello undernät.

tooverify nätverksanslutningen hello konfigurerade slutpunkter och se om du kan nå hello VM via ett annat protokoll, till exempel http- eller en annan tjänst.

Försök hello SSH-anslutningen igen efter de här stegen.

## <a name="find-hello-source-of-hello-issue"></a>Hitta hello källan till hello problem
hello SSH-klienten på datorn kan misslyckas tooreach hello SSH-tjänsten på hello Azure VM på grund av tooissues eller felaktiga konfigurationer i hello följande områden:

* [SSH-klientdatorn](#source-1-ssh-client-computer)
* [Gränsenheten för organisationen](#source-2-organization-edge-device)
* [Molnet tjänstslutpunkten och åtkomstkontrollista (ACL)](#source-3-cloud-service-endpoint-and-acl)
* [Nätverkssäkerhetsgrupper](#source-4-network-security-groups)
* [Linux-baserade Azure VM](#source-5-linux-based-azure-virtual-machine)

## <a name="source-1-ssh-client-computer"></a>Datakällan 1: SSH klientdatorn
tooeliminate datorn som hello källa för hello fel, kontrollera att det kan göra att SSH-anslutningar tooanother lokalt, Linux-baserade datorn.

![Diagram som visar SSH komponenter för klientdatorer](./media/detailed-troubleshoot-ssh-connection/ssh-tshoot2.png)

Om hello anslutningen misslyckas, kontrollera hello efter problem på datorn:

* En inställning för lokal brandvägg som blockerar inkommande eller utgående SSH-trafik (TCP 22)
* Lokalt installerat klientprogramvaran för proxy som hindrar SSH-anslutningar
* Lokalt installerad programvara som hindrar SSH-anslutningar för nätverksövervakning
* Andra typer av säkerhetsprogram som övervaka trafik eller tillåta/neka vissa typer av trafik

Om något av dessa villkor gäller tillfälligt inaktivera hello programvara och försök en SSH-anslutning tooan lokala dator toofind ut hello orsak hello anslutningen blockeras på datorn. Arbeta med din administratör toocorrect hello programvara inställningar tooallow SSH nätverksanslutningar.

Om du använder certifikatautentisering kan du kontrollera att du har dessa behörigheter toohello .ssh-mappen i din hemkatalog:

* Chmod 700 ~/.ssh
* Chmod 644 ~/.ssh/\*pub
* Chmod 600 ~/.ssh/id_rsa (eller andra filer som har dina privata nycklar lagras i dem)
* Chmod 644 ~/.ssh/known_hosts (innehåller värdar som du har anslutit toovia SSH)

## <a name="source-2-organization-edge-device"></a>Källan 2: Gränsenheten organisation
tooeliminate din organisation insticksenhet som hello källa för hello fel, kontrollera att en dator som har direkt ansluten toohello Internet kan göra SSH-anslutningar tooyour Azure VM. Om du ansluter till hello VM via ett plats-till-plats-VPN eller Azure ExpressRoute-anslutning, hoppar du över för[källa 4: Nätverkssäkerhetsgrupper](#nsg).

![Diagram som visar gränsenheten för organisationen](./media/detailed-troubleshoot-ssh-connection/ssh-tshoot3.png)

Om du inte har en dator som är direkt anslutna toohello Internet, skapa en ny virtuell Azure-dator i en egen resurs eller molnomfång tjänst och använda den. Mer information finns i [skapa en virtuell dator som kör Linux i Azure](quick-create-cli.md). Ta bort hello resurs grupp eller virtuell dator och moln tjänsten när du är klar med din testning.

Om du kan skapa en SSH-anslutning med en dator som har direktanslutna toohello Internet, kontrollera din organisation insticksenhet för:

* En intern brandvägg som blockerar trafik SSH med hello Internet
* En proxyserver som förhindrar att SSH-anslutningar
* Intrångsidentifiering eller program som körs på enheter i nätverket kant som hindrar SSH-anslutningar för nätverksövervakning

Arbeta med din administratör toocorrect hello nätverksinställningarna för din organisation edge enheter tooallow SSH trafik med hello Internet.

## <a name="source-3-cloud-service-endpoint-and-acl"></a>Källa 3: Molnet tjänstslutpunkten och ACL
> [!NOTE]
> Den här källan gäller tooVMs som har skapats med hjälp av hello klassiska distributionsmodellen. För virtuella datorer som har skapats med hjälp av Resource Manager kan du hoppa över för[datakällan 4: Nätverkssäkerhetsgrupper](#nsg).

tooeliminate hello molnet tjänstslutpunkten och ACL som hello källa för hello fel, kontrollera att en annan Azure VM i hello samma virtuella nätverk kan göra SSH-anslutningar tooyour VM.

![Diagram som illustrerar molnet tjänstslutpunkten och ACL](./media/detailed-troubleshoot-ssh-connection/ssh-tshoot4.png)

Om du inte har en annan virtuell dator i hello samma virtuella nätverk, kan du enkelt skapa en. Mer information finns i [skapar en Linux-VM på Azure med hjälp av hello CLI](quick-create-cli.md). Ta bort hello extra VM när du är klar med din testning.

Om du kan skapa en SSH-anslutning med en virtuell dator i hello samma virtuella nätverk, kontrollera hello följande områden:

* **hello slutpunktskonfigurationen för SSH-trafik på hello målet VM.** hello privata TCP-port för hello slutpunkten ska matcha hello TCP-port på vilken hello SSH-tjänsten på hello VM lyssnar. (hello-standardporten är 22). Kontrollera hello SSH TCP-portnummer i hello Azure-portalen genom att välja **virtuella datorer** > *namn på virtuell* > **inställningar**  >  **Slutpunkter**.
* **hello ACL för hello SSH trafik slutpunkt på hello virtuella måldatorn.** En ACL aktiverar toospecify tillåts eller nekas inkommande trafik från hello Internet, baserat på dess IP-adress. Felkonfigurerad ACL: er kan förhindra att inkommande SSH trafik toohello slutpunkt. Kontrollera din ACL: er tooensure som inkommande trafik från hello offentliga IP-adresser i din proxyserver eller andra gränsservern tillåts. Mer information finns i [om nätverksåtkomst åtkomstkontrollistor (ACL)](../../virtual-network/virtual-networks-acl.md).

tooeliminate hello-slutpunkt som en källa för hello problem, ta bort hello aktuella slutpunkt, skapa en annan slutpunkt och ange hello SSH namn (TCP-port 22 hello offentliga och privata portnummer). Mer information finns i [konfigurera slutpunkter på en virtuell dator i Azure](../windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

<a id="nsg"></a>

## <a name="source-4-network-security-groups"></a>Datakällan 4: Nätverkssäkerhetsgrupper
Nätverkssäkerhetsgrupper aktivera toohave mer detaljerad kontroll över tillåtna inkommande och utgående trafik. Du kan skapa regler som sträcker sig över undernät och molntjänster i Azure-nätverk. Kontrollera din network security group regler tooensure tooand som SSH-trafik från hello Internet tillåts.
Mer information finns i [om nätverkssäkerhetsgrupper](../../virtual-network/virtual-networks-nsg.md).

Du kan också använda IP-Kontrollera toovalidate hello NSG-konfigurationen. Mer information finns i [Azure nätverksövervakning översikt](https://docs.microsoft.com/en-us/azure/network-watcher/network-watcher-monitoring-overview). 

## <a name="source-5-linux-based-azure-virtual-machine"></a>Källa 5: Linux-baserade virtuell Azure-dator
hello senaste källan för möjliga problem är hello virtuella Azure-datorn sig själv.

![Diagram som visar Linux-baserade virtuell Azure-dator](./media/detailed-troubleshoot-ssh-connection/ssh-tshoot5.png)

Om du inte redan gjort det, följer du anvisningarna för hello [tooreset lösenord eller SSH för Linux-baserade virtuella datorer](classic/reset-access.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

Försök ansluta igen från datorn. Om det fortfarande inte visas hello nedan några av hello möjliga problem:

* hello SSH-tjänsten körs inte på hello virtuella måldatorn.
* hello SSH-tjänsten lyssnar inte på TCP-port 22. tootest, installera en telnet-klienten på den lokala datorn och kör ”telnet *cloudServiceName*. cloudapp.net 22”. Det här steget anger om hello virtuella kan inkommande och utgående kommunikation toohello SSH-slutpunkten.
* hello lokala brandväggen på hello virtuella måldatorn har regler som hindrar inkommande eller utgående SSH-trafik.
* Intrångsidentifiering eller programvara som körs på hello Azure-dator för nätverksövervakning som hindrar SSH-anslutningar.

## <a name="additional-resources"></a>Ytterligare resurser
Mer information om hur du felsöker programåtkomst finns [Felsök åtkomst tooan program som körs på en virtuell Azure-dator](troubleshoot-app-connection.md)
