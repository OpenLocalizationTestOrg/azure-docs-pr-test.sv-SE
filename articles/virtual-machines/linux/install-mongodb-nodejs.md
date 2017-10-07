---
title: "aaaInstall MongoDB på en Linux VM som använder hello Azure CLI 1.0 | Microsoft Docs"
description: "Lär dig hur tooinstall och konfigurera MongoDB på en Linux-dator i Azure med hjälp av hello Resource Manager-modellen."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 3f55b546-86df-4442-9ef4-8a25fae7b96e
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: 4ce21a2c63da7d00a4422e0a6766e2103e7f12d7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooinstall-and-configure-mongodb-on-a-linux-vm-using-hello-azure-cli-10"></a>Hur tooinstall och konfigurera MongoDB på en Linux VM som använder hello Azure CLI 1.0
[MongoDB](http://www.mongodb.org) är en populär öppen källkod, högpresterande NoSQL-databas. Den här artikeln beskrivs hur du tooinstall och konfigurera MongoDB på en Linux-VM i Azure med hjälp av hello Resource Manager-modellen. Exempel visas hur detaljer till:

* [Installera och konfigurera en grundläggande MongoDB-instansen manuellt](#manually-install-and-configure-mongodb-on-a-vm)
* [Skapa en grundläggande MongoDB-instans med en Resource Manager-mall](#create-basic-mongodb-instance-on-centos-using-a-template)
* [Skapa en komplex MongoDB delat kluster med replik anger med hjälp av en Resource Manager-mall](#create-a-complex-mongodb-sharded-cluster-on-centos-using-a-template)


## <a name="cli-versions-toocomplete-hello-task"></a>CLI versioner toocomplete hello aktivitet
Du kan göra hello med hjälp av något av följande versioner av CLI hello:

- Azure CLI 1.0 – våra CLI för hello klassisk och resurs management distributionsmodeller (den här artikeln)
- [Azure CLI 2.0](create-cli-complete-nodejs.md) -vår nästa generations CLI för hello resursdistributionsmodell för hantering


## <a name="manually-install-and-configure-mongodb-on-a-vm"></a>Installera och konfigurera MongoDB på en virtuell dator manuellt
MongoDB [ange installationsinstruktioner](https://docs.mongodb.com/manual/administration/install-on-linux/) för Linux-distributioner inklusive Red Hat / CentOS, SUSE, Ubuntu och Debian. hello följande exempel skapas en *CentOS* VM som använder en SSH-nyckel som lagras på *~/.ssh/id_rsa.pub*. Svaret hello uppmanar för lagringskontonamn, DNS-namn och autentiseringsuppgifter som administratör:

```azurecli
azure vm quick-create \
    --image-urn CentOS \
    --ssh-publickey-file ~/.ssh/id_rsa.pub 
```

Logga in toohello VM med hello offentliga IP-adress visas hello slutet av hello föregående steg i att skapa VM:

```bash
ssh azureuser@40.78.23.145
```

tooadd hello installationskällor för MongoDB, skapa en **yum** fil i databasen på följande sätt:

```bash
sudo touch /etc/yum.repos.d/mongodb-org-3.4.repo
```

Öppna hello MongoDB-repo-filen för redigering. Lägg till hello följande rader:

```sh
[mongodb-org-3.4]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/3.4/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-3.4.asc
```

Installera MongoDB med **yum** på följande sätt:

```bash
sudo yum install -y mongodb-org
```

Som standard aktiveras SELinux på CentOS bilder som hindrar dig från att komma åt MongoDB. Installera hanteringsverktyg och konfigurera SELinux tooallow MongoDB toooperate på dess TCP-standardporten 27017 på följande sätt. 

```bash
sudo yum install -y policycoreutils-python
sudo semanage port -a -t mongod_port_t -p tcp 27017
```

Starta hello MongoDB-tjänsten på följande sätt:

```bash
sudo service mongod start
```

Kontrollera hello MongoDB installationen genom att ansluta med hello lokala `mongo` klient:

```bash
mongo
```

Nu testa hello MongoDB-instansen genom att lägga till vissa data och sedan söka:

```sh
> db
test
> db.foo.insert( { a : 1 } )  
> db.foo.find()  
{ "_id" : ObjectId("57ec477cd639891710b90727"), "a" : 1 }
> exit
```

Om du vill konfigurera MongoDB toostart automatiskt under en omstart av systemet:

```bash
sudo chkconfig mongod on
```


## <a name="create-basic-mongodb-instance-on-centos-using-a-template"></a>Skapa en grundläggande MongoDB-instans på CentOS med en mall
Du kan skapa en grundläggande MongoDB-instans på en enda CentOS VM som använder hello följande Azure quickstart-mall från GitHub. Den här mallen använder hello tillägget för anpassat skript för Linux tooadd en `yum` databasen tooyour nyskapad CentOS VM och sedan installera MongoDB.

* [Grundläggande MongoDB-instansen på CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-on-centos) -https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json

hello följande exempel skapar en resursgrupp med namnet hello `myResourceGroup` i hello `eastus` region. Ange egna värden enligt följande:

```azurecli
azure group create \
    --name myResourceGroup \
    --location eastus \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json
```

> [!NOTE]
> hello Azure CLI returnerar du tooa prompt inom några sekunder för att skapa hello distributionen, men hello installation och konfiguration tar några minuter toocomplete. Kontrollera hello status hello-distribution med `azure group deployment show myResourceGroup`, därför att ange hello namnet på resursgruppen. Vänta tills hello **ProvisioningState** visar *lyckades* innan du försöker tooSSH toohello VM.

När hello distribution är Slutför SSH toohello VM. Hämta hello IP-adressen för den virtuella datorn med hjälp av hello `azure vm show` kommandot som i följande exempel hello:

```azurecli
azure vm show --resource-group myResourceGroup --name myLinuxVM
```

Nära hello ände hello utdata visas hello offentlig IP-adress. SSH tooyour VM med hello IP-adressen för den virtuella datorn:

```bash
ssh azureuser@138.91.149.74
```

Kontrollera hello MongoDB installationen genom att ansluta med hello lokala `mongo` klienten på följande sätt:

```bash
mongo
```

Nu testa hello-instans genom att lägga till vissa data och söka på följande sätt:

```sh
> db
test
> db.foo.insert( { a : 1 } )  
> db.foo.find()  
{ "_id" : ObjectId("57ec477cd639891710b90727"), "a" : 1 }
> exit
```


## <a name="create-a-complex-mongodb-sharded-cluster-on-centos-using-a-template"></a>Skapa ett komplext delat MongoDB-kluster på CentOS med en mall
Du kan skapa komplexa delat MongoDB-kluster med hjälp av hello följande Azure quickstart-mall från GitHub. Den här mallen följer hello [MongoDB delat kluster metodtips](https://docs.mongodb.com/manual/core/sharded-cluster-components/) tooprovide redundans och hög tillgänglighet. hello mallen skapar två delar med tre noder i varje replik. En replik för config server med tre noder skapas också, plus två **mongos** router servrar tooprovide konsekvenskontroll tooapplications från över hello delar.

* [MongoDB horisontell partitionering kluster på CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-sharding-centos) -https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-sharding-centos/azuredeploy.json

> [!WARNING]
> Distribuera det här komplexa delat MongoDB-kluster kräver mer än 20 kärnor, som vanligtvis är antal hello standard kärnor per region för en prenumeration. Öppna ett Azure-supporten begäran tooincrease din antal kärnor.

hello följande exempel skapar en resursgrupp med namnet hello *myResourceGroup* i hello *eastus* region. Ange egna värden enligt följande:

```azurecli
azure group create \
    --name myResourceGroup \
    --location eastus \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-sharding-centos/azuredeploy.json
```

> [!NOTE]
> hello Azure CLI returnerar du tooa prompt inom några sekunder för att skapa hello distributionen, men hello installation och konfiguration kan ta över en timme toocomplete. Kontrollera hello status hello-distribution med `azure group deployment show myResourceGroup`, justeras efter hello namnet på resursgruppen. Vänta tills hello **ProvisioningState** visar *lyckades* innan du ansluter toohello virtuella datorer.


## <a name="next-steps"></a>Nästa steg
I dessa fall måste ansluta du toohello MongoDB-instansen lokalt från hello VM. Om du vill tooconnect toohello MongoDB-instansen från en annan virtuell dator eller nätverk, se till att lämpliga hello [Nätverkssäkerhetsgruppen regler skapas](nsg-quickstart.md).

Mer information om hur du skapar med hjälp av mallar finns hello [översikt över Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).

hello Azure Resource Manager-Mallar Använd hello toodownload för tillägget för anpassat skript och köra skript på din virtuella dator. Mer information finns i [Using hello tillägget för Azure anpassat skript med Linux-datorer](extensions-customscript.md).

