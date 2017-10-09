---
title: 3-nod Deis aaaDeploy klustret | Microsoft Docs
description: "Den här artikeln beskriver hur toocreate 3-nod Deis kluster på Azure med hjälp av en Azure Resource Manager-mall"
services: virtual-machines-linux
documentationcenter: 
author: HaishiBai
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 5eb67eb7-95d4-461d-8eac-44925224ba5f
ms.service: virtual-machines-linux
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 06/24/2015
ms.author: hbai
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a4c0fb8cbb849264e64b433540157c9afecd184e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-and-configure-a-3-node-deis-cluster-in-azure"></a>Distribuera och konfigurera en 3-nod Deis kluster i Azure
Den här artikeln vägleder dig genom att etablera en [Deis](http://deis.io/) klustret på Azure. Den omfattar alla hello steg från att skapa hello nödvändiga certifikat toodeploying och skala ett exempel på en **Gå** på hello nyetablerade klustret.

hello visar följande diagram hello arkitekturen för hello distribuerade system. En administratör hanterar hello med Deis verktyg som **deis** och **deisctl**. Anslutningar upprättas via en belastningsutjämnare som vidarebefordrar hello anslutningar tooone hello medlem noder i klustret hello Azure. hello-klienter åtkomst till distribuerade program via hello belastningsutjämnare samt. I det här fallet hello belastningsutjämning vidarebefordrar hello trafik tooa Deis router nät som ytterligare routs trafik toocorresponding Docker behållare hello kluster som värd.

  ![Arkitekturdiagram för distribuerade Desis kluster](./media/deis-cluster/architecture-overview.png)

I ordning toorun via följande hello, behöver du:

* En aktiv Azure-prenumeration. Om du inte har någon, kan du få en kostnadsfri testversion [azure.com](https://azure.microsoft.com/).
* En arbetet eller skolan id toouse Azure-resursgrupper. Om du har ett personligt konto och logga in med ett Microsoft-id, du behöver för[skapa ett arbetsobjekt-id från din personliga](../windows/create-aad-work-id.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Antingen--beroende på dina klientoperativsystem--hello [Azure PowerShell](/powershell/azureps-cmdlets-docs) eller hello [Azure CLI för Mac, Linux och Windows](../../cli-install-nodejs.md).
* [OpenSSL](https://www.openssl.org/). OpenSSL är används toogenerate hello nödvändiga certifikat.
* En Git-klient som [Git Bash](https://git-scm.com/).
* tootest hello exempelprogrammet, måste du också en DNS-server. Du kan använda alla DNS-servrar eller tjänster som har stöd för jokertecken A-poster.
* En dator toorun Deis klientverktyg. Du kan använda en lokal dator eller en virtuell dator. Du kan köra dessa verktyg på nästan alla Linux-distribution, men hello följande anvisningar använder Ubuntu.

## <a name="provision-hello-cluster"></a>Etablera hello kluster
I det här avsnittet ska du använda en [Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md) mall från hello öppen källkod databasen [azure snabbstartsmallar](https://github.com/Azure/azure-quickstart-templates). Först ska du kopiera ned hello mallen. Sedan skapar du en ny SSH-nyckel för autentisering. Och sedan konfigurerar du en ny identifierare för kluster. Och slutligen hello Shell-skript eller hello PowerShell-skriptet tooprovision hello klustret ska använda.

1. Klona hello databasen: [https://github.com/Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates).
   
        git clone https://github.com/Azure/azure-quickstart-templates
2. Gå toohello mallmapp:
   
        cd azure-quickstart-templates\deis-cluster-coreos
3. Skapa en ny SSH-nyckel med hjälp av ssh-keygen:
   
        ssh-keygen -t rsa -b 4096 -c "[your_email@domain.com]"
4. Generera ett certifikat med hello ovan privat nyckel:
   
        openssl req -x509 -days 365 -new -key [your private key file] -out [cert file toobe generated]
5. Gå för[https://discovery.etcd.io/new](https://discovery.etcd.io/new) toogenerate en ny token i klustret som ser ut ungefär som:
   
        https://discovery.etcd.io/6a28e078895c5ec737174db2419bb2f3
   <br />
   Varje virtuell CoreOS-kluster måste toohave en unik token från den här kostnadsfria tjänsten. Se [virtuell CoreOS dokumentationen](https://coreos.com/docs/cluster-management/setup/cluster-discovery/) för mer information.
6. Ändra hello **moln config.yaml** filen tooreplace hello befintliga **identifiering** hello ny token-token:
   
        #cloud-config
        ---
        coreos:
          etcd:
            # generate a new token for each unique cluster from https://discovery.etcd.io/new
            # uncomment hello following line and replace it with your discovery URL
            discovery: https://discovery.etcd.io/3973057f670770a7628f917d58c2208a
        ...
7. Ändra hello **azuredeploy-parameters.json** fil: öppna hello-certifikat som du skapade i steg 4 i en textredigerare. Kopiera all text mellan `----BEGIN CERTIFICATE-----` och `-----END CERTIFICATE-----` till hello **sshKeyData** parametern (du behöver tooremove alla radmatningstecken).
8. Ändra hello **newStorageAccountName** parameter. Detta är hello storage-konto för VM OS-diskar. Kontonamnet har toobe globalt unika.
9. Ändra hello **publicDomainName** parameter. Detta blir en del av hello DNS-namnet som associeras med hello load balancer offentlig IP. hello slutliga FQDN har hello formatet för *[värdet för den här parametern]*. *[region]* . cloudapp.azure.com. Om du anger hello namn som deishbai32 och hello resursgruppen är distribuerade toohello västra USA region och sedan hello slutliga kommer FQDN tooyour belastningsutjämnaren vara deishbai32.westus.cloudapp.azure.com.
10. Spara hello parameterfil. Och sedan kan du etablera hello-kluster med hjälp av Azure PowerShell:
    
        .\deploy-deis.ps1 -ResourceGroupName [resource group name] -ResourceGroupLocation "West US" -TemplateFile
        .\azuredeploy.json -ParametersFile .\azuredeploy-parameters.json -CloudInitFile .\cloud-config.yaml
    
    eller Azure CLI:
    
        ./deploy-deis.sh -n "[resource group name]" -l "West US" -f ./azuredeploy.json -e ./azuredeploy-parameters.json
        -c ./cloud-config.yaml  
11. När hello resursgruppen har etablerats visas alla hello resurser i hello grupp på den klassiska Azure-portalen. Som det visas i följande skärmbild, hello hello resursgruppen innehåller ett virtuellt nätverk med tre virtuella datorer, som är anslutna toohello samma tillgänglighetsuppsättning. hello gruppen innehåller också en belastningsutjämnare som har en tillhörande offentliga IP-adress.
    
    ![hello etablerade resursgrupp klassiska Azure-portalen](./media/deis-cluster/resource-group.png)

## <a name="install-hello-client"></a>Installera hello-klienten
Du behöver **deisctl** toocontrol din Deis klustret. Även om deisctl installeras automatiskt på alla noder i klustret hello, är det en bra idé toouse deisctl på en separat administrativ dator. Dessutom, eftersom alla noder har konfigurerats med endast privata IP-adresser, måste toouse SSH-tunnel via hello belastningsutjämnare, som har en offentlig IP-adress, tooconnect toohello nod datorer. hello är följande hello stegen för att konfigurera deisctl på en separat Ubuntu fysisk eller virtuell dator.

1. Installera deisctl:mkdir deis
   
        cd deis
        curl -sSL http://deis.io/deisctl/install.sh | sh -s 1.6.1
        sudo ln -fs $PWD/deisctl /usr/local/bin/deisctl
2. Lägg till din privata nyckel toossh agent:
   
        eval `ssh-agent -s`
        ssh-add [path toohello private key file, see step 1 in hello previous section]
3. Konfigurera deisctl:
   
        export DEISCTL_TUNNEL=[public ip of hello load balancer]:2223

hello mallen definierar ingående NAT-regler som mappar 2223 tooinstance 1, 2224 tooinstance 2 och 2225 tooinstance 3. Detta ger redundans för hello deisctl verktyget. Du kan granska reglerna på den klassiska Azure-portalen:

![NAT-regler för hello belastningsutjämnare](./media/deis-cluster/nat-rules.png)

> [!NOTE]
> Hello mallen stöder för närvarande endast kluster 3-noder. Detta är på grund av en begränsning i Azure Resource Manager-mall NAT Regeldefinitionen som inte stöder loop-syntax.
> 
> 

## <a name="install-and-start-hello-deis-platform"></a>Installera och starta hello Deis plattform
Nu kan du använda deisctl tooinstall och starta hello Deis plattform:

    deisctl config platform set domain=[some domain]
    deisctl config platform set sshPrivateKey=[path toohello private key file]
    deisctl install platform
    deisctl start platform

> [!NOTE]
> Första hello plattform tar ett tag (upp till 10 minuter). Särskilt, kan startar hello builder-tjänst ta lång tid. Och ibland tar några försöker toosucceed: om hello-åtgärden verkar toohang, försök att skriva `ctrl+c` toobreak körning av hello kommandot och försök igen.
> 
> 

Du kan använda `deisctl list` tooverify om alla tjänster körs:

    deisctl list
    UNIT                            MACHINE                 LOAD    ACTIVE          SUB
    deis-builder.service            ebe3005e.../10.0.0.6    loaded  active          running
    deis-controller.service         ebe3005e.../10.0.0.6    loaded  active          running
    deis-database.service           9c79bbdd.../10.0.0.5    loaded  active          running
    deis-logger.service             9c79bbdd.../10.0.0.5    loaded  active          running
    deis-logspout.service           8d658d5a.../10.0.0.4    loaded  active          running
    deis-logspout.service           9c79bbdd.../10.0.0.5    loaded  active          running
    deis-logspout.service           ebe3005e.../10.0.0.6    loaded  active          running
    deis-publisher.service          8d658d5a.../10.0.0.4    loaded  active          running
    deis-publisher.service          9c79bbdd.../10.0.0.5    loaded  active          running
    deis-publisher.service          ebe3005e.../10.0.0.6    loaded  active          running
    deis-registry@1.service         ebe3005e.../10.0.0.6    loaded  active          running
    deis-router@1.service           ebe3005e.../10.0.0.6    loaded  active          running
    deis-router@2.service           8d658d5a.../10.0.0.4    loaded  active          running
    deis-router@3.service           9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-daemon.service       8d658d5a.../10.0.0.4    loaded  active          running
    deis-store-daemon.service       9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-daemon.service       ebe3005e.../10.0.0.6    loaded  active          running
    deis-store-gateway@1.service    9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-metadata.service     8d658d5a.../10.0.0.4    loaded  active          running
    deis-store-metadata.service     9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-metadata.service     ebe3005e.../10.0.0.6    loaded  active          running
    deis-store-monitor.service      8d658d5a.../10.0.0.4    loaded  active          running
    deis-store-monitor.service      9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-monitor.service      ebe3005e.../10.0.0.6    loaded  active          running
    deis-store-volume.service       8d658d5a.../10.0.0.4    loaded  active          running
    deis-store-volume.service       9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-volume.service       ebe3005e.../10.0.0.6    loaded  active          running

Grattis! Nu har du en löpande Deis clsuter i Azure! Nu ska vi distribuerar ett exempel gå programmet toosee hello klustret i åtgärden.

## <a name="deploy-and-scale-a-hello-world-application"></a>Distribuera och skala en programmet Hello World
hello följande steg visar hur toodeploy ”Hello World” gå programmet toohello klustret. hello steg baseras på [Deis dokumentationen](http://docs.deis.io/en/latest/using_deis/using-dockerfiles/#using-dockerfiles).

1. För hello routning nät toowork korrekt, behöver du toohave en jokertecken A-post för din domän pekar toohello offentliga IP-Adressen för hello belastningsutjämnare. hello följande skärmbild visar hello A-post för en exempel-domänregistrering på GoDaddy:
   
    ![GoDaddy A-post](./media/deis-cluster/go-daddy.png)
   
   <p />
2. Installera deis:
   
        mkdir deis
        cd deis
        curl -sSL http://deis.io/deis-cli/install.sh | sh
        ln -fs $PWD/deis /usr/local/bin/deis
3. Skapa en ny SSH-nyckel och Lägg sedan till hello offentliga nyckel tooGitHub (naturligtvis kan du också återanvända dina befintliga nycklar). toocreate ett nytt SSH nyckelpar, Använd:
   
        cd ~/.ssh
        ssh-keygen (press [Enter]s toouse default file names and empty passcode)
4. Lägga till id_rsa.pub eller hello offentliga nyckeln för ditt val tooGitHub. Du kan göra detta med hjälp av hello lägga till SSH key-knappen i skärmen SSH-nycklar konfiguration:
   
   ![GitHub-nyckel](./media/deis-cluster/github-key.png)
   
   <p />
5. Registrera en ny användare:
   
        deis register http://deis.[your domain]
   <p />
6. Lägg till hello SSH-nyckel:
   
        deis keys:add [path tooyour SSH public key]
   <p />      
7. Skapa ett program.
   
        git clone https://github.com/deis/helloworld.git
        cd helloworld
        deis create
        git push deis master
   <p />
8.Hej git push utlöser Docker bilder toobe skapats och distribuerats, vilket kan ta några minuter. Från min erfarenhet ibland låser steg 10 (Pushing tooprivate avbildningslagringsplatsen). Då kan du stoppa hello process, ta bort hello använder ' deis appar: förstör – a <application name> ` tooremove hello application and try again. You can use `deis apps:list' toofind ut hello namnet på ditt program. Om allt fungerar bör du se något liknande följande hello hello slutet av kommandoutdata:
   
        -----> Launching...
               done, lambda-underdog:v2 deployed tooDeis
               http://lambda-underdog.artitrack.com
               toolearn more, use `deis help` or visit http://deis.io
        toossh://git@deis.artitrack.com:2222/lambda-underdog.git
         * [new branch]      master -> master
   <p />
9. Kontrollera om hello programmet fungerar:
   
        curl -S http://[your application name].[your domain]
   Du bör se:
   
        Welcome tooDeis!
        See hello documentation at http://docs.deis.io/ for more information.
        (you can use geis apps:list tooget hello name of your application).
   <p />
10. Skala hello too3 programinstanser:
    
        deis scale cmd=3
    <p />
11. Du kan använda deis info tooexamine information om ditt program. hello är följande utdata från min programdistributionen:
    
        deis info
        === lambda-underdog Application
        {
          "updated": "2015-05-22T06:14:10UTC",
          "uuid": "10c74ee7-b7ff-4786-967a-7e65af7eabc3",
          "created": "2015-05-22T06:07:55UTC",
          "url": "lambda-underdog.artitrack.com",
          "owner": "haishi",
          "id": "lambda-underdog",
          "structure": {
            "cmd": 3
          }
        }
    
        === lambda-underdog Processes
        --- cmd:
        cmd.1 up (v2)
        cmd.2 up (v2)
        cmd.3 up (v2)
    
        === lambda-underdog Domains
        No domains

## <a name="next-steps"></a>Nästa steg
Den här artikeln gått du igenom alla hello steg tooprovision Deis ett nytt kluster på Azure med hjälp av en Azure Resource Manager-mall. hello mallen har stöd för redundans i tooling anslutningar samt belastningsutjämning för distribuerade program. hello mallen undviker du också använda offentliga IP-adresser för medlem noder, vilket sparar värdefull offentliga IP-resurser och ger en säkrare miljö toohost program. toolearn se fler hello följande artiklar:

[Översikt över Azure Resource Manager][resource-group-overview]  
[Hur toouse hello Azure CLI][azure-command-line-tools]  
[Använda Azure PowerShell med Azure Resource Manager][powershell-azure-resource-manager]  

[azure-command-line-tools]: ../../cli-install-nodejs.md
[resource-group-overview]: ../../azure-resource-manager/resource-group-overview.md
[powershell-azure-resource-manager]: ../../azure-resource-manager/powershell-azure-resource-manager.md
