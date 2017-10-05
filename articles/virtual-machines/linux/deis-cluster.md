---
title: Distribuera en 3-nod Deis klustret | Microsoft Docs
description: "Den här artikeln beskriver hur du skapar en 3-nod Deis kluster på Azure med hjälp av en Azure Resource Manager-mall"
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
ms.openlocfilehash: 9a0c3dd7562dfb5ce54c2ebfd4665109f59cd8fd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-and-configure-a-3-node-deis-cluster-in-azure"></a>Distribuera och konfigurera en 3-nod Deis kluster i Azure
Den här artikeln vägleder dig genom att etablera en [Deis](http://deis.io/) klustret på Azure. Den omfattar alla steg från att skapa nödvändiga certifikat för att distribuera och skala ett exempel på en **Gå** på nyetablerade klustret.

Följande diagram visar arkitekturen för det distribuerade systemet. En administratör hanterar klustret med hjälp av Deis verktyg som **deis** och **deisctl**. Anslutningar upprättas via en Azure belastningsutjämnare som vidarebefordrar anslutningar till en medlemmen noder i klustret. Klienterna kommer åt distribuerade program via belastningsutjämnaren. I det här fallet belastningsutjämnaren vidarebefordrar trafik till en Deis router nät som ytterligare routs trafik till motsvarande Docker-behållare i värdklustret.

  ![Arkitekturdiagram för distribuerade Desis kluster](./media/deis-cluster/architecture-overview.png)

För att kunna köra igenom följande steg behöver du:

* En aktiv Azure-prenumeration. Om du inte har någon, kan du få en kostnadsfri testversion [azure.com](https://azure.microsoft.com/).
* Ett arbets- eller skolkonto id för att använda Azure-resursgrupper. Om du har ett personligt konto och logga in med ett Microsoft-id kan du behöva [skapa ett arbetsobjekt-id från din personliga](../windows/create-aad-work-id.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Antingen--beroende på dina klientoperativsystem--den [Azure PowerShell](/powershell/azureps-cmdlets-docs) eller [Azure CLI för Mac, Linux och Windows](../../cli-install-nodejs.md).
* [OpenSSL](https://www.openssl.org/). OpenSSL används för att generera nödvändiga certifikat.
* En Git-klient som [Git Bash](https://git-scm.com/).
* Om du vill testa exempelprogrammet måste du också en DNS-server. Du kan använda alla DNS-servrar eller tjänster som har stöd för jokertecken A-poster.
* En dator för att köra Deis klientverktyg. Du kan använda en lokal dator eller en virtuell dator. Du kan köra dessa verktyg på nästan alla Linux-distribution, men följande instruktioner använda Ubuntu.

## <a name="provision-the-cluster"></a>Etablera klustret
I det här avsnittet ska du använda en [Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md) mallen från lagringsplatsen för öppen källkod [azure snabbstartsmallar](https://github.com/Azure/azure-quickstart-templates). Först ska du kopiera ned mallen. Sedan skapar du en ny SSH-nyckel för autentisering. Och sedan konfigurerar du en ny identifierare för kluster. Och slutligen du ska använda Shell-skript eller PowerShell-skript för att etablera klustret.

1. Klona lagringsplatsen: [https://github.com/Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates).
   
        git clone https://github.com/Azure/azure-quickstart-templates
2. Gå till mallmappen:
   
        cd azure-quickstart-templates\deis-cluster-coreos
3. Skapa en ny SSH-nyckel med hjälp av ssh-keygen:
   
        ssh-keygen -t rsa -b 4096 -c "[your_email@domain.com]"
4. Generera ett certifikat med den privata nyckeln ovan:
   
        openssl req -x509 -days 365 -new -key [your private key file] -out [cert file to be generated]
5. Gå till [https://discovery.etcd.io/new](https://discovery.etcd.io/new) att skapa en ny token i klustret, som ser ut ungefär som:
   
        https://discovery.etcd.io/6a28e078895c5ec737174db2419bb2f3
   <br />
   Varje virtuell CoreOS-kluster måste ha en unik token från den här kostnadsfria tjänsten. Se [virtuell CoreOS dokumentationen](https://coreos.com/docs/cluster-management/setup/cluster-discovery/) för mer information.
6. Ändra den **moln config.yaml** ersätta den befintliga filen **identifiering** token med den nya token:
   
        #cloud-config
        ---
        coreos:
          etcd:
            # generate a new token for each unique cluster from https://discovery.etcd.io/new
            # uncomment the following line and replace it with your discovery URL
            discovery: https://discovery.etcd.io/3973057f670770a7628f917d58c2208a
        ...
7. Ändra den **azuredeploy-parameters.json** fil: öppna certifikatet som du skapade i steg 4 i en textredigerare. Kopiera all text mellan `----BEGIN CERTIFICATE-----` och `-----END CERTIFICATE-----` till den **sshKeyData** parametern (du behöver ta bort alla radmatningstecken).
8. Ändra den **newStorageAccountName** parameter. Det här är lagringskontot för VM OS-diskar. Kontonamnet måste vara globalt unika.
9. Ändra den **publicDomainName** parameter. Detta blir en del av DNS-namnet som associeras med load balancer offentlig IP. Den slutliga FQDN har formatet för *[värdet för den här parametern]*. *[region]* . cloudapp.azure.com. Till exempel om du anger namnet som deishbai32 och resursgruppen har distribuerats till regionen västra USA, blir sedan sista FQDN till din belastningsutjämnare deishbai32.westus.cloudapp.azure.com.
10. Spara parameterfilen med. Och sedan kan du etablera klustret med Azure PowerShell:
    
        .\deploy-deis.ps1 -ResourceGroupName [resource group name] -ResourceGroupLocation "West US" -TemplateFile
        .\azuredeploy.json -ParametersFile .\azuredeploy-parameters.json -CloudInitFile .\cloud-config.yaml
    
    eller Azure CLI:
    
        ./deploy-deis.sh -n "[resource group name]" -l "West US" -f ./azuredeploy.json -e ./azuredeploy-parameters.json
        -c ./cloud-config.yaml  
11. När resursgruppen har etablerats kan du se alla resurser i gruppen på klassiska Azure-portalen. I följande skärmbild visas resursgruppen innehåller ett virtuellt nätverk med tre virtuella datorer, som är anslutna till samma tillgänglighetsuppsättning. Gruppen innehåller också en belastningsutjämnare som har en tillhörande offentliga IP-adress.
    
    ![Etablerade resursgruppen på den klassiska Azure-portalen](./media/deis-cluster/resource-group.png)

## <a name="install-the-client"></a>Installera klienten
Du behöver **deisctl** att kontrollera din Deis klustret. Även om deisctl installeras automatiskt på alla noder i klustret, är det en bra idé att använda deisctl på en separat administrativ dator. Eftersom alla noder har konfigurerats med endast privata IP-adresser behöver du dessutom använda SSH-tunnel genom belastningsutjämnare, som har en offentlig IP-adress, att ansluta till noden-datorer. Nedan följer stegen för att konfigurera deisctl på en separat Ubuntu fysiska eller virtuella datorn.

1. Installera deisctl:mkdir deis
   
        cd deis
        curl -sSL http://deis.io/deisctl/install.sh | sh -s 1.6.1
        sudo ln -fs $PWD/deisctl /usr/local/bin/deisctl
2. Lägg till din privata nyckel till ssh agent:
   
        eval `ssh-agent -s`
        ssh-add [path to the private key file, see step 1 in the previous section]
3. Konfigurera deisctl:
   
        export DEISCTL_TUNNEL=[public ip of the load balancer]:2223

Mallen definierar ingående NAT-regler som mappar 2223 till instansen 1, 2224 till 2-instans och 2225 till 3-instans. Detta ger redundans för att använda verktyget deisctl. Du kan granska reglerna på den klassiska Azure-portalen:

![NAT-regler på belastningsutjämnaren](./media/deis-cluster/nat-rules.png)

> [!NOTE]
> Mallen stöder för närvarande endast kluster 3-noder. Detta är på grund av en begränsning i Azure Resource Manager-mall NAT Regeldefinitionen som inte stöder loop-syntax.
> 
> 

## <a name="install-and-start-the-deis-platform"></a>Installera och starta den Deis plattform
Nu kan du använda deisctl för att installera och starta den Deis plattform:

    deisctl config platform set domain=[some domain]
    deisctl config platform set sshPrivateKey=[path to the private key file]
    deisctl install platform
    deisctl start platform

> [!NOTE]
> Startar plattformen som tar ett tag (upp till 10 minuter). Särskilt, kan startar tjänsten builder ta lång tid. Och ibland tar det lite tid ska lyckas: om åtgärden verkar låser sig, försök att skriva `ctrl+c` bryta körning av kommandot och försök sedan igen.
> 
> 

Du kan använda `deisctl list` att kontrollera om alla tjänster körs:

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

Grattis! Nu har du en löpande Deis clsuter i Azure! Nu ska vi distribuerar ett exempel gå program för att se klustret i åtgärden.

## <a name="deploy-and-scale-a-hello-world-application"></a>Distribuera och skala en programmet Hello World
Följande steg visar hur du distribuerar en ”Hello World” gå program i klustret. Anvisningarna är baserade på [Deis dokumentationen](http://docs.deis.io/en/latest/using_deis/using-dockerfiles/#using-dockerfiles).

1. För routning nät ska fungera korrekt måste ha ett jokertecken A-posten för din domän som pekar på offentliga IP-Adressen för belastningsutjämnaren. Följande skärmbild visar A-post för en exempel-domänregistrering på GoDaddy:
   
    ![GoDaddy A-post](./media/deis-cluster/go-daddy.png)
   
   <p />
2. Installera deis:
   
        mkdir deis
        cd deis
        curl -sSL http://deis.io/deis-cli/install.sh | sh
        ln -fs $PWD/deis /usr/local/bin/deis
3. Skapa en ny SSH-nyckel och Lägg sedan till den offentliga nyckeln till GitHub (naturligtvis kan du också återanvända dina befintliga nycklar). Använd för att skapa en ny SSH-nyckel:
   
        cd ~/.ssh
        ssh-keygen (press [Enter]s to use default file names and empty passcode)
4. Lägga till id_rsa.pub eller den offentliga nyckeln för ditt val i GitHub. Du kan göra detta med hjälp av Lägg till SSH key knappen på din skärm för SSH-nycklar konfiguration:
   
   ![GitHub-nyckel](./media/deis-cluster/github-key.png)
   
   <p />
5. Registrera en ny användare:
   
        deis register http://deis.[your domain]
   <p />
6. Lägg till SSH-nyckeln:
   
        deis keys:add [path to your SSH public key]
   <p />      
7. Skapa ett program.
   
        git clone https://github.com/deis/helloworld.git
        cd helloworld
        deis create
        git push deis master
   <p />
8.Git push utlöser Docker avbildningar för skapats och distribuerats, vilket kan ta några minuter. Från min erfarenhet ibland låser steg 10 (Pushing bild privata databasen). När detta inträffar kan du avbryta processen, ta bort programmet med hjälp av ' deis appar: förstör – a <application name> ` to remove the application and try again. You can use `deis apps:list' Ta reda på namnet på ditt program. Om allt fungerar bör du se något som liknar följande i slutet av kommandoutdata:
   
        -----> Launching...
               done, lambda-underdog:v2 deployed to Deis
               http://lambda-underdog.artitrack.com
               To learn more, use `deis help` or visit http://deis.io
        To ssh://git@deis.artitrack.com:2222/lambda-underdog.git
         * [new branch]      master -> master
   <p />
9. Kontrollera att programmet fungerar:
   
        curl -S http://[your application name].[your domain]
   Du bör se:
   
        Welcome to Deis!
        See the documentation at http://docs.deis.io/ for more information.
        (you can use geis apps:list to get the name of your application).
   <p />
10. Skala program till 3 instanser:
    
        deis scale cmd=3
    <p />
11. Du kan använda deis information om du vill granska information om ditt program. Följande utdata är från min programdistributionen:
    
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
Den här artikeln gått du igenom alla stegen för att etablera en ny Deis kluster på Azure med hjälp av en Azure Resource Manager-mall. Mallen har stöd för redundans i tooling anslutningar samt belastningsutjämning för distribuerade program. Mallen undviker du också använda offentliga IP-adresser för medlem noder, vilket sparar värdefull offentliga IP-resurser och ger en säkrare miljö som värd för program. Mer information finns i följande artiklar:

[Översikt över Azure Resource Manager][resource-group-overview]  
[Hur du använder Azure CLI][azure-command-line-tools]  
[Använda Azure PowerShell med Azure Resource Manager][powershell-azure-resource-manager]  

[azure-command-line-tools]: ../../cli-install-nodejs.md
[resource-group-overview]: ../../azure-resource-manager/resource-group-overview.md
[powershell-azure-resource-manager]: ../../azure-resource-manager/powershell-azure-resource-manager.md
