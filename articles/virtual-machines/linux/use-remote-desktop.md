---
title: "aaaUse fjärrskrivbord tooa Linux VM i Azure | Microsoft Docs"
description: "Lär dig hur tooinstall och konfigurera fjärrskrivbord (xrdp) tooconnect tooa Linux VM i Azure med hjälp av grafiska verktyg"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: iainfou
ms.openlocfilehash: 64d30be101ceeb49fc05bb10293ad63db358efe3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="install-and-configure-remote-desktop-tooconnect-tooa-linux-vm-in-azure"></a>Installera och konfigurera fjärrskrivbord tooconnect tooa Linux VM i Azure
Linux-datorer (VM) i Azure hanteras vanligtvis från hello kommandoraden med hjälp av en secure shell (SSH)-anslutning. När nya tooLinux eller i scenarier med snabb felsökning hello användning av fjärrskrivbord kan vara enklare. Den här artikeln information om hur tooinstall och konfigurera en Skrivbordsmiljö ([xfce](https://www.xfce.org)) och fjärrskrivbord ([xrdp](http://www.xrdp.org)) för dina Linux VM med hjälp av hello Resource Manager-modellen.


## <a name="prerequisites"></a>Krav
Den här artikeln kräver ett befintligt Linux VM i Azure. Om du behöver toocreate en virtuell dator kan använda en av hello följande metoder:

- Hej [Azure CLI 2.0](quick-create-cli.md)
- Hej [Azure-portalen](quick-create-portal.md)


## <a name="install-a-desktop-environment-on-your-linux-vm"></a>Installera en Skrivbordsmiljö på Linux-VM
De flesta virtuella Linux-datorer i Azure har inte en Skrivbordsmiljö installerad som standard. Virtuella Linux-datorer hanteras ofta med hjälp av SSH-anslutningar i stället för en Skrivbordsmiljö. Det finns olika skrivbordsmiljöer i Linux som du kan välja. Beroende på ditt val av Skrivbordsmiljö kan den använda en too2 GB diskutrymme, och ta 5 too10 minuter tooinstall och konfigurera alla hello krävs paket.

hello följande exempel installeras hello lightweight [xfce4](https://www.xfce.org/) Skrivbordsmiljö på en Ubuntu VM. Kommandon för andra distributioner variera (Använd `yum` tooinstall på Red Hat Enterprise Linux och konfigurera lämplig `selinux` regler eller Använd `zypper` tooinstall på SUSE, till exempel).

Första, SSH tooyour VM. hello följande exempel ansluter toohello virtuella datorn med namnet *myvm.westus.cloudapp.azure.com* med hello användarnamnet för *azureuser*:

```bash
ssh azureuser@myvm.westus.cloudapp.azure.com
```

Om du använder Windows, och du behöver mer information om hur du använder SSH, se [hur toouse SSH-nycklar med Windows](ssh-from-windows.md).

Därefter installera xfce med `apt` på följande sätt:

```bash
sudo apt-get update
sudo apt-get install xfce4
```

## <a name="install-and-configure-a-remote-desktop-server"></a>Installera och konfigurera en stationär dator
Nu när du har en Skrivbordsmiljö installerad kan du konfigurera en Fjärrskrivbordstjänster toolisten för inkommande anslutningar. [xrdp](http://xrdp.org) är en öppen källkod Remote Desktop Protocol (RDP)-server som är tillgänglig på de flesta Linux-distributioner och fungerar väl med xfce. Installera xrdp på din Ubuntu VM på följande sätt:

```bash
sudo apt-get install xrdp
```

Meddela xrdp vilka Skrivbordsmiljö toouse när du startar sessionen. Konfigurera xrdp toouse xfce som skrivbordsmiljön på följande sätt:

```bash
echo xfce4-session >~/.xsession
```

Starta om hello xrdp för hello ändringar tootake effekt på följande sätt:

```bash
sudo service xrdp restart
```


## <a name="set-a-local-user-account-password"></a>Ange ett lösenord för lokala användare
Hoppa över det här steget om du har skapat ett lösenord för användarkontot när du skapade den virtuella datorn. Om du bara använder SSH-nyckelautentisering och har inte ett lokalt kontolösenord ange, ange ett lösenord innan du använder xrdp toolog i tooyour VM. xrdp kan inte acceptera SSH-nycklar för autentisering. hello följande exempel anger ett lösenord för användarkontot för hello *azureuser*:

```bash
sudo passwd azureuser
```

> [!NOTE]
> Ange ett lösenord uppdateras inte SSHD för konfigurationen toopermit Lösenordsinloggning om den för närvarande inte. Du kanske vill tooconnect tooyour VM med en SSH-tunnel med nyckel-baserad autentisering och anslut sedan tooxrdp från ett säkerhetsperspektiv. I så fall, hoppar du över hello följande steg om hur du skapar en säkerhet grupp regeln tooallow remote desktop nätverkstrafik.


## <a name="create-a-network-security-group-rule-for-remote-desktop-traffic"></a>Skapa en säkerhetsgrupp för nätverk-regel för Remote Desktop-trafik
tooallow fjärrskrivbord trafik tooreach din Linux VM, en grupp för nätverkssäkerhetsregeln måste toobe skapas som tillåter TCP på port 3389 tooreach den virtuella datorn. Mer information om regler för nätverkssäkerhetsgrupper finns [vad är en Nätverkssäkerhetsgrupp?](../../virtual-network/virtual-networks-nsg.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) Du kan också [Använd hello Azure portal toocreate en grupp för nätverkssäkerhetsregeln](../windows/nsg-quickstart-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

hello följande exempel skapar en grupp nätverkssäkerhetsregeln med [az nätverket nsg regeln skapa](/cli/azure/network/nsg/rule#create) med namnet *myNetworkSecurityGroupRule* för*Tillåt* trafik på *tcp* port *3389*.

```azurecli
az network nsg rule create \
    --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup \
    --name myNetworkSecurityGroupRule \
    --protocol tcp \
    --priority 1010 \
    --destination-port-range 3389
```


## <a name="connect-your-linux-vm-with-a-remote-desktop-client"></a>Anslut din Linux VM med en fjärrskrivbordsklient
Öppna din lokala fjärrskrivbordsklienten och Anslut toohello IP-adress eller DNS-namn för Linux-VM. Ange hello användarnamn och lösenord för hello användarkonto på den virtuella datorn på följande sätt:

![Ansluta tooxrdp Remote Desktop-klienten](./media/use-remote-desktop/remote-desktop-client.png)

Efter autentisering, hello xfce Skrivbordsmiljö läsa in och titta liknande toohello följande exempel:

![xfce Skrivbordsmiljö via xrdp](./media/use-remote-desktop/xfce-desktop-environment.png)


## <a name="troubleshoot"></a>Felsöka
Om du inte kan ansluta tooyour Linux VM med hjälp av en fjärrskrivbordsklient `netstat` på din Linux VM tooverify som den virtuella datorn lyssnar för RDP-anslutningar på följande sätt:

```bash
sudo netstat -plnt | grep rdp
```

följande exempel visar hello hello VM som lyssnar på TCP-port 3389 som förväntat:

```bash
tcp     0     0      127.0.0.1:3350     0.0.0.0:*     LISTEN     53192/xrdp-sesman
tcp     0     0      0.0.0.0:3389       0.0.0.0:*     LISTEN     53188/xrdp
```

Om hello xrdp tjänsten inte lyssnar på en Ubuntu VM startar du om hello på följande sätt:

```bash
sudo service xrdp restart
```

Granska loggar in */var/log*Thug på din Ubuntu VM för uppgifter som toowhy hello-tjänsten svarar inte. Du kan också övervaka hello syslog under en anslutning till fjärrskrivbord försök tooview eventuella fel:

```bash
tail -f /var/log/syslog
```

Andra Linux-distributioner, till exempel Red Hat Enterprise Linux och SUSE kan ha olika sätt toorestart tjänster och alternativa log file platser tooreview.

Om du inte får något svar i din fjärrskrivbordsklienten och inte ser några händelser i hello systemloggen, anger detta att remote desktop-trafik inte går att nå hello VM. Granska dina network security grupp regler tooensure att du har en regel toopermit TCP port 3389. Mer information finns i [felsökning av problem med nätverksanslutningen](../windows/troubleshoot-app-connection.md).


## <a name="next-steps"></a>Nästa steg
Mer information om hur du skapar och använder SSH-nycklar med Linux virtuella datorer finns [skapa SSH-nycklar för Linux virtuella datorer i Azure](mac-create-ssh-keys.md).

Information om hur du använder SSH från Windows finns [hur toouse SSH-nycklar med Windows](ssh-from-windows.md).

