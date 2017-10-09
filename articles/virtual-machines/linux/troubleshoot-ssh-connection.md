---
title: "aaaTroubleshoot SSH-anslutningen utfärdar tooan Azure VM | Microsoft Docs"
description: "Hur tootroubleshoot problem, till exempel ”SSH-anslutningen misslyckades' eller 'SSH-anslutningen nekades' för en Azure VM kör Linux."
keywords: SSH anslutningen nekades, ssh fel, azure ssh, SSH-anslutning misslyckades
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: top-support-issue,azure-service-management,azure-resource-manager
ms.assetid: dcb82e19-29b2-47bb-99f2-900d4cfb5bbb
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: iainfou
ms.openlocfilehash: dfb4e75e571c8306edf5f300c4e0f07a5fe7750a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-ssh-connections-tooan-azure-linux-vm-that-fails-errors-out-or-is-refused"></a>Felsökning av SSH-anslutningar tooan virtuella Azure Linux-datorn som misslyckas, fel, eller nekas
Det finns olika orsaker till att det uppstår fel på SSH (Secure Shell), SSH anslutningsfel eller SSH nekas när du försöker tooconnect tooa virtuell Linux-dator (VM). Den här artikeln hjälper dig att hitta och rätt hello problem. Du kan använda hello Azure-portalen, Azure CLI eller tillägg för virtuell dator åtkomst för Linux tootroubleshoot och lösa anslutningsproblem med.

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Om du behöver mer hjälp när som helst i den här artikeln kan du kontakta hello Azure experter på [hello MSDN Azure och Stack Overflow-forum](http://azure.microsoft.com/support/forums/). Alternativt kan du lagra en incident i Azure-supporten. Gå toohello [Azure supportwebbplats](http://azure.microsoft.com/support/options/) och välj **få support**. Information om hur du använder Azure stöder läsa hello [vanliga frågor om Microsoft Azure-supporten](http://azure.microsoft.com/support/faq/).

## <a name="quick-troubleshooting-steps"></a>Snabbsteg för felsökning
Försöka ansluta igen toohello VM efter varje steg i felsökningen.

1. Återställa hello SSH-konfigurationen.
2. Återställa hello autentiseringsuppgifter för hello användaren.
3. Kontrollera hello [Nätverkssäkerhetsgruppen](../../virtual-network/virtual-networks-nsg.md) regler att SSH-trafik.
   * Se till att det finns en Nätverkssäkerhetsgrupp regel toopermit SSH-trafik (som standard, TCP-port 22).
   * Du kan inte använda omdirigering av portar / mappning utan att använda en Azure belastningsutjämnare.
4. Kontrollera hello [VM resurshälsa](../../resource-health/resource-health-overview.md). 
   * Se till att hello VM rapporter som felfritt.
   * Om du har aktiverat startdiagnostikinställningar Kontrollera hello VM inte rapporterar startfel i hello loggar.
5. Starta om hello VM.
6. Omdistribuera hello VM.

Fortsätt läsa för mer detaljerad felsökning och förklaringar.

## <a name="available-methods-tootroubleshoot-ssh-connection-issues"></a>Tillgängliga metoder tootroubleshoot SSH-anslutningsproblem
Du kan återställa autentiseringsuppgifter eller SSH-konfigurationen med någon av följande metoder hello:

* [Azure-portalen](#use-the-azure-portal) – bra om du behöver tooquickly återställa hello SSH-konfigurationen eller SSH-nyckeln och du inte hello Azure-verktyg installerat.
* [Azure CLI 2.0](#use-the-azure-cli-20) - om du redan är på hello kommandoraden snabbt återställa hello SSH-konfigurationen eller autentiseringsuppgifter. Du kan också använda hello [Azure CLI 1.0](#use-the-azure-cli-10)
* [Azure VMAccessForLinux-tillägget](#use-the-vmaccess-extension) – skapa och återanvända json definition filer tooreset hello SSH konfiguration eller autentiseringsuppgifter.

Försök ansluta tooyour VM igen efter varje steg i felsökningen. Om du fortfarande inte kan ansluta försök hello nästa steg.

## <a name="use-hello-azure-portal"></a>Använd hello Azure-portalen
hello Azure-portalen innehåller ett snabbt sätt tooreset hello SSH-konfigurationen eller autentiseringsuppgifter utan att installera verktyg som på den lokala datorn.

Välj den virtuella datorn i hello Azure-portalen. Bläddra nedåt toohello **stöd + felsökning** avsnittet och väljer **Återställ lösenord** som i följande exempel hello:

![Återställ SSH-konfigurationen eller autentiseringsuppgifter i hello Azure-portalen](./media/troubleshoot-ssh-connection/reset-credentials-using-portal.png)

### <a name="reset-hello-ssh-configuration"></a>Återställ hello SSH-konfiguration
Som ett första steg bör du välja `Reset configuration only` från hello **läge** nedrullningsbara menyn som i föregående skärmbilden hello Klicka hello **återställa** knappen. När den här åtgärden har slutförts, försök tooaccess den virtuella datorn igen.

### <a name="reset-ssh-credentials-for-a-user"></a>Återställ SSH-autentiseringsuppgifter för en användare
tooreset hello autentiseringsuppgifterna för en befintlig användare väljer du antingen `Reset SSH public key` eller `Reset password` från hello **läge** nedrullningsbara menyn som hello föregående skärmbild. Ange hello användarnamn och en SSH-nyckel eller ett nytt lösenord och klicka sedan på hello **återställa** knappen.

Du kan också skapa en användare med sudo-behörighet på hello VM från den här menyn. Ange ett nytt användarnamn och tillhörande lösenord eller SSH-nyckel och klicka sedan på hello **återställa** knappen.

## <a name="use-hello-azure-cli-20"></a>Använd hello Azure CLI 2.0
Om du inte redan gjort installera hello senaste [Azure CLI 2.0](/cli/azure/install-az-cli2) och logga in tooan Azure-konto med [az inloggningen](/cli/azure/#login).

Om du har skapat och överföra en anpassad avbildning för Linux-disk, se att hello [Microsoft Azure Linux-agenten](../windows/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) version 2.0.5 eller senare är installerat. För virtuella datorer skapas med galleriavbildningar kan det här tillägget för åtkomst redan installeras och konfigureras för dig.

### <a name="reset-ssh-configuration"></a>Återställ SSH-konfiguration
Du kan först Försök återställa hello SSH toodefault konfigurationsvärden och om hello SSH-server på hello VM. Observera att detta inte ändra hello konto användarnamn, lösenord eller SSH-nycklar.
hello följande exempel används [az vm användare återställa-ssh](/cli/azure/vm/user#reset-ssh) tooreset hello SSH-konfigurationen på hello virtuella datorn med namnet `myVM` i `myResourceGroup`. Använd egna värden enligt följande:

```azurecli
az vm user reset-ssh --resource-group myResourceGroup --name myVM
```

### <a name="reset-ssh-credentials-for-a-user"></a>Återställ SSH-autentiseringsuppgifter för en användare
hello följande exempel används [az vm uppdateringen](/cli/azure/vm/user#update) tooreset hello autentiseringsuppgifter för `myUsername` toohello värdet som anges i `myPassword`, på hello virtuella datorn med namnet `myVM` i `myResourceGroup`. Använd egna värden enligt följande:

```azurecli
az vm user update --resource-group myResourceGroup --name myVM \
     --username myUsername --password myPassword
```

Om du använder SSH-nyckelautentisering, kan du återställa hello SSH-nyckel för en viss användare. hello följande exempel används **az vm komma åt set-linux-användare** tooupdate hello SSH-nyckel som lagras i `~/.ssh/id_rsa.pub` för hello användare med namnet `myUsername`, på hello virtuella datorn med namnet `myVM` i `myResourceGroup`. Använd egna värden enligt följande:

```azurecli
az vm user update --resource-group myResourceGroup --name myVM \
    --username myUsername --ssh-key-value ~/.ssh/id_rsa.pub
```

## <a name="use-hello-vmaccess-extension"></a>Använd hello VMAccess-tillägget
hello VM Access-tillägg för Linux läser i en json-fil som definierar åtgärder toocarry ut. Dessa åtgärder omfattar återställer SSHD, när du återställer en SSH-nyckel eller lägger till en användare. Du fortfarande använda hello Azure CLI toocall hello VMAccess-tillägget, men du kan återanvända hello json-filer över flera virtuella datorer om du vill. Den här metoden kan du toocreate en databas för json-filer som sedan kan anropas för angivna scenarier.

### <a name="reset-sshd"></a>Återställ SSHD
Skapa en fil med namnet `settings.json` med hello följande innehåll:

```json
{  
    "reset_ssh":"True"
}
```

Använda hello Azure CLI kan du sedan anropa hello `VMAccessForLinux` tillägget tooreset SSHD anslutningen genom att ange en json-fil. hello följande exempel används [az vm-tillägget set](/cli/azure/vm/extension#set) tooreset SSHD på hello virtuella datorn med namnet `myVM` i `myResourceGroup`. Använd egna värden enligt följande:

```azurecli
az vm extension set --resource-group philmea --vm-name Ubuntu \
    --name VMAccessForLinux --publisher Microsoft.OSTCExtensions --version 1.2 --settings settings.json
```

### <a name="reset-ssh-credentials-for-a-user"></a>Återställ SSH-autentiseringsuppgifter för en användare
Om SSHD visas toofunction korrekt, kan du återställa hello autentiseringsuppgifter för en x-användare. tooreset hello lösenord för en användare skapar en fil med namnet `settings.json`. hello följande exempel återställs hello autentiseringsuppgifter för `myUsername` toohello värdet som anges i `myPassword`. Ange hello följande rader i din `settings.json` fil, använda egna värden:

```json
{
    "username":"myUsername", "password":"myPassword"
}
```

Eller tooreset Hej SSH-nyckel för en användare, först skapa en fil med namnet `settings.json`. hello följande exempel återställs hello autentiseringsuppgifter för `myUsername` toohello värdet som anges i `myPassword`, på hello virtuella datorn med namnet `myVM` i `myResourceGroup`. Ange hello följande rader i din `settings.json` fil, använda egna värden:

```json
{
    "username":"myUsername", "ssh_key":"mySSHKey"
}
```

När du har skapat en json-fil, använder du hello Azure CLI toocall hello `VMAccessForLinux` tillägget tooreset SSH-användare autentiseringsuppgifter genom att ange en json-fil. hello följande exempel återställs autentiseringsuppgifter på hello virtuella datorn med namnet `myVM` i `myResourceGroup`. Använd egna värden enligt följande:

```azurecli
az vm extension set --resource-group philmea --vm-name Ubuntu \
    --name VMAccessForLinux --publisher Microsoft.OSTCExtensions --version 1.2 --settings settings.json
```

## <a name="use-hello-azure-cli-10"></a>Använd hello Azure CLI 1.0
Om du inte redan har gjort [installera hello Azure CLI 1.0 och ansluta tooyour Azure-prenumeration](../../cli-install-nodejs.md). Kontrollera att du använder Resource Manager-läge på följande sätt:

```azurecli
azure config mode arm
```

Om du har skapat och överföra en anpassad avbildning för Linux-disk, se att hello [Microsoft Azure Linux-agenten](../windows/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) version 2.0.5 eller senare är installerat. För virtuella datorer skapas med galleriavbildningar kan det här tillägget för åtkomst redan installeras och konfigureras för dig.

### <a name="reset-ssh-configuration"></a>Återställ SSH-konfiguration
Hej SSHD configuration själva kan vara felkonfigurerad eller hello tjänsten påträffade ett fel. Du kan återställa SSHD toomake att hello SSH-konfigurationen själva är giltig. Återställer SSHD ska hello första åtgärden du vidta.

hello följande exempel återställs SSHD på en virtuell dator med namnet `myVM` i hello resursgrupp med namnet `myResourceGroup`. Använd din egen VM- och gruppnamn enligt följande:

```azurecli
azure vm reset-access --resource-group myResourceGroup --name myVM \
    --reset-ssh
```

### <a name="reset-ssh-credentials-for-a-user"></a>Återställ SSH-autentiseringsuppgifter för en användare
Du kan återställa hello lösenord för en användare som är om SSHD visas toofunction korrekt. hello följande exempel återställs hello autentiseringsuppgifter för `myUsername` toohello värdet som anges i `myPassword`, på hello virtuella datorn med namnet `myVM` i `myResourceGroup`. Använd egna värden enligt följande:

```azurecli
azure vm reset-access --resource-group myResourceGroup --name myVM \
     --user-name myUsername --password myPassword
```

Om du använder SSH-nyckelautentisering, kan du återställa hello SSH-nyckel för en viss användare. följande exempel uppdateringar hello hello SSH-nyckel som lagras i `~/.ssh/id_rsa.pub` för hello användare med namnet `myUsername`, på hello virtuella datorn med namnet `myVM` i `myResourceGroup`. Använd egna värden enligt följande:

```azurecli
azure vm reset-access --resource-group myResourceGroup --name myVM \
    --user-name myUsername --ssh-key-file ~/.ssh/id_rsa.pub
```


## <a name="restart-a-vm"></a>Starta om en virtuell dator
Om du har återställts hello SSH-konfigurationen och användarautentiseringsuppgifter eller påträffade ett fel när detta sker, kan du omstart hello VM tooaddress underliggande beräkning problem.

### <a name="azure-portal"></a>Azure Portal
toorestart som en virtuell dator med hjälp av hello Azure portal, Välj den virtuella datorn och klicka på hello **starta om** knappen som i följande exempel hello:

![Starta om en virtuell dator i hello Azure-portalen](./media/troubleshoot-ssh-connection/restart-vm-using-portal.png)

### <a name="azure-cli-10"></a>Azure CLI 1.0
följande exempel omstarter hello hello virtuella datorn med namnet `myVM` i hello resursgrupp med namnet `myResourceGroup`. Använd egna värden enligt följande:

```azurecli
azure vm restart --resource-group myResourceGroup --name myVM
```

### <a name="azure-cli-20"></a>Azure CLI 2.0
hello följande exempel används [az vm omstart](/cli/azure/vm#restart) toorestart hello virtuella datorn med namnet `myVM` i hello resursgrupp med namnet `myResourceGroup`. Använd egna värden enligt följande:

```azurecli
az vm restart --resource-group myResourceGroup --name myVM
```


## <a name="redeploy-a-vm"></a>Distribuera om en VM
Du kan distribuera en VM tooanother nod i Azure, vilket kan korrigera eventuella underliggande nätverksproblem. Information om omdistribuera en virtuell dator finns [omdistribuera virtuell dator toonew Azure nod](../windows/redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

> [!NOTE]
> När den här åtgärden har slutförts tillfälliga data kommer att gå förlorade och dynamiska IP-adresser som är associerade med hello virtuella datorn kommer att uppdateras.
> 
> 

### <a name="azure-portal"></a>Azure Portal
tooredeploy som en virtuell dator med hjälp av hello Azure portal, Välj den virtuella datorn och rulla ned toohello **stöd + felsökning** avsnitt. Klicka på hello **omdistribuera** knappen som i följande exempel hello:

![Distribuera en virtuell dator i hello Azure-portalen](./media/troubleshoot-ssh-connection/redeploy-vm-using-portal.png)

### <a name="azure-cli-10"></a>Azure CLI 1.0
följande exempel återdistribuerar hello hello virtuella datorn med namnet `myVM` i hello resursgrupp med namnet `myResourceGroup`. Använd egna värden enligt följande:

```azurecli
azure vm redeploy --resource-group myResourceGroup --name myVM
```

### <a name="azure-cli-20"></a>Azure CLI 2.0
Hej följande exempel används [az vm Omdistributionen](/cli/azure/vm#redeploy) tooredeploy hello virtuella datorn med namnet `myVM` i hello resursgrupp med namnet `myResourceGroup`. Använd egna värden enligt följande:

```azurecli
az vm redeploy --resource-group myResourceGroup --name myVM
```

## <a name="vms-created-by-using-hello-classic-deployment-model"></a>Virtuella datorer som skapats med hjälp av hello klassiska distributionsmodellen
Försök dessa steg tooresolve hello vanligaste SSH anslutningsfel för virtuella datorer som har skapats med hjälp av hello klassiska distributionsmodellen. Försöka ansluta igen toohello VM efter varje steg.

* Återställ fjärråtkomst från hello [Azure-portalen](https://portal.azure.com). På hello Azure-portalen markerar du den virtuella datorn och på hello **Återställ fjärråtkomst...**  knappen.
* Starta om hello VM. På hello [Azure-portalen](https://portal.azure.com), Välj den virtuella datorn och på hello **starta om** knappen.
    
* Omdistribuera hello VM tooa nya Azure noden. Information om hur tooredeploy en virtuell dator finns [omdistribuera virtuell dator toonew Azure nod](../windows/redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
  
    När den här åtgärden har slutförts tillfälliga data kommer att gå förlorade och dynamiska IP-adresser som är associerade med hello virtuella datorn kommer att uppdateras.
* Följ anvisningarna för hello i [hur tooreset lösenord eller SSH för Linux-baserade virtuella datorer](classic/reset-access.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json) till:
  
  * Återställ hello lösenord eller SSH-nyckel.
  * Skapa en *sudo* användarkonto.
  * Återställa hello SSH-konfigurationen.
* Kontrollera hello Virtuella datorresursens hälsotillstånd vid plattformsproblem.<br>
     Välj den virtuella datorn och rulla ned **inställningar** > **Kontrollera hälsan**.

## <a name="additional-resources"></a>Ytterligare resurser
* Om du fortfarande inte tooSSH tooyour VM efter följande hello efter stegen, se [mer detaljerad felsökning](detailed-troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) tooreview ytterligare steg tooresolve problemet.
* Mer information om hur du felsöker programåtkomst finns [Felsök åtkomst tooan program som körs på en virtuell Azure-dator](../windows/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* Mer information om felsökning av virtuella datorer som har skapats med hjälp av hello klassiska distributionsmodellen finns [hur tooreset lösenord eller SSH för Linux-baserade virtuella datorer](classic/reset-access.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

