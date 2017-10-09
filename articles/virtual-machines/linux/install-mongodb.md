---
title: "aaaInstall MongoDB på en Linux-VM med hello Azure CLI | Microsoft Docs"
description: "Lär dig hur tooinstall och konfigurera MongoDB på en Linux virtuella iusing hello Azure CLI 2.0"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 3f55b546-86df-4442-9ef4-8a25fae7b96e
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/23/2017
ms.author: iainfou
ms.openlocfilehash: 97a4d9913f0eeaf7b8bf15d7fc81befe538cdc8d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooinstall-and-configure-mongodb-on-a-linux-vm"></a>Hur tooinstall och konfigurera MongoDB på en Linux-VM
[MongoDB](http://www.mongodb.org) är en populär öppen källkod, högpresterande NoSQL-databas. Den här artikeln beskrivs hur du tooinstall och konfigurera MongoDB på en Linux-VM med hello Azure CLI 2.0. Du kan också utföra dessa steg med hello [Azure CLI 1.0](install-mongodb-nodejs.md). Exempel visas hur detaljer till:

* [Installera och konfigurera en grundläggande MongoDB-instansen manuellt](#manually-install-and-configure-mongodb-on-a-vm)
* [Skapa en grundläggande MongoDB-instans med en Resource Manager-mall](#create-basic-mongodb-instance-on-centos-using-a-template)
* [Skapa en komplex MongoDB delat kluster med replik anger med hjälp av en Resource Manager-mall](#create-a-complex-mongodb-sharded-cluster-on-centos-using-a-template)


## <a name="manually-install-and-configure-mongodb-on-a-vm"></a>Installera och konfigurera MongoDB på en virtuell dator manuellt
MongoDB [ange installationsinstruktioner](https://docs.mongodb.com/manual/administration/install-on-linux/) för Linux-distributioner inklusive Red Hat / CentOS, SUSE, Ubuntu och Debian. hello följande exempel skapas en *CentOS* VM. toocreate miljö, behöver du hello senaste [Azure CLI 2.0](/cli/azure/install-az-cli2) installerad och logga in tooan Azure-konto med [az inloggningen](/cli/azure/#login).

Skapa en resursgrupp med [az group create](/cli/azure/group#create). hello följande exempel skapar en resursgrupp med namnet *myResourceGroup* i hello *eastus* plats:

```azurecli
az group create --name myResourceGroup --location eastus
```

Skapa en virtuell dator med [az vm create](/cli/azure/vm#create). hello följande exempel skapas en virtuell dator med namnet *myVM* med en användare med namnet *azureuser* med SSH-autentisering för offentlig nyckel

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image CentOS \
    --admin-username azureuser \
    --generate-ssh-keys
```

SSH toohello VM med hjälp av användarnamn och hello `publicIpAddress` som anges i hello utdata från hello föregående steg:

```bash
ssh azureuser@<publicIpAddress>
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

Som standard aktiveras SELinux på CentOS bilder som hindrar dig från att komma åt MongoDB. Installera hanteringsverktyg och konfigurera SELinux tooallow MongoDB toooperate på dess TCP-standardporten 27017 på följande sätt:

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
Du kan skapa en grundläggande MongoDB-instans på en enda CentOS VM som använder hello följande Azure quickstart-mall från GitHub. Den här mallen använder hello tillägget för anpassat skript för Linux tooadd en **yum** databasen tooyour nyskapad CentOS VM och sedan installera MongoDB.

* [Grundläggande MongoDB-instansen på CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-on-centos) -https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json

toocreate miljö, behöver du hello senaste [Azure CLI 2.0](/cli/azure/install-az-cli2) installerad och logga in tooan Azure-konto med [az inloggningen](/cli/azure/#login). Börja med att skapa en resursgrupp med [az gruppen skapa](/cli/azure/group#create). hello följande exempel skapar en resursgrupp med namnet *myResourceGroup* i hello *eastus* plats:

```azurecli
az group create --name myResourceGroup --location eastus
```

Distribuera hello MongoDB mall med [az distribution skapa](/cli/azure/group/deployment#create). Definiera en egen resurs namn och storlekar vid behov, till exempel som för *newStorageAccountName*, *virtualNetworkName*, och *vmSize*:

```azurecli
az group deployment create --resource-group myResourceGroup \
  --parameters '{"newStorageAccountName": {"value": "mystorageaccount"},
    "adminUsername": {"value": "azureuser"},
    "adminPassword": {"value": "P@ssw0rd!"},
    "dnsNameForPublicIP": {"value": "mypublicdns"},
    "virtualNetworkName": {"value": "myVnet"},
    "vmSize": {"value": "Standard_DS2_v2"},
    "vmName": {"value": "myVM"},
    "publicIPAddressName": {"value": "myPublicIP"},
    "nicName": {"value": "myNic"}}' \
  --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json
```

Logga in toohello VM med hello offentlig DNS-adress på den virtuella datorn. Du kan visa hello offentlig DNS-adress med [az vm visa](/cli/azure/vm#show):

```azurecli
az vm show -g myResourceGroup -n myVM -d --query [fqdns] -o tsv
```

SSH tooyour VM med hjälp av användarnamn och offentliga DNS-adress:

```bash
ssh azureuser@mypublicdns.eastus.cloudapp.azure.com
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

toocreate miljö, behöver du hello senaste [Azure CLI 2.0](/cli/azure/install-az-cli2) installerad och logga in tooan Azure-konto med [az inloggningen](/cli/azure/#login). Börja med att skapa en resursgrupp med [az gruppen skapa](/cli/azure/group#create). hello följande exempel skapar en resursgrupp med namnet *myResourceGroup* i hello *eastus* plats:

```azurecli
az group create --name myResourceGroup --location eastus
```

Distribuera hello MongoDB mall med [az distribution skapa](/cli/azure/group/deployment#create). Definiera en egen resurs namn och storlekar vid behov, till exempel som för *mongoAdminUsername*, *sizeOfDataDiskInGB*, och *configNodeVmSize*:

```azurecli
az group deployment create --resource-group myResourceGroup \
  --parameters '{"adminUsername": {"value": "azureuser"},
    "adminPassword": {"value": "P@ssw0rd!"},
    "mongoAdminUsername": {"value": "mongoadmin"},
    "mongoAdminPassword": {"value": "P@ssw0rd!"},
    "dnsNamePrefix": {"value": "mypublicdns"},
    "environment": {"value": "AzureCloud"},
    "numDataDisks": {"value": "4"},
    "sizeOfDataDiskInGB": {"value": 20},
    "centOsVersion": {"value": "7.0"},
    "routerNodeVmSize": {"value": "Standard_DS3_v2"},
    "configNodeVmSize": {"value": "Standard_DS3_v2"},
    "replicaNodeVmSize": {"value": "Standard_DS3_v2"},
    "zabbixServerIPAddress": {"value": "Null"}}' \
  --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-sharding-centos/azuredeploy.json \
  --name myMongoDBCluster \
  --no-wait
```

Den här distributionen kan ta över en timme toodeploy och konfigurera alla hello VM-instanser. Hej `--no-wait` flaggan används hello slutet av hello före kommandot tooreturn kontrollen toohello Kommandotolken när hello malldistribution har godkänts av hello Azure-plattformen. Du kan sedan visa hello Distributionsstatus med [az grupp distribution visa](/cli/azure/group/deployment#show). hello följande exempel vyer hello status för hello *myMongoDBCluster* distribution i hello *myResourceGroup* resursgrupp:

```azurecli
az group deployment show \
    --resource-group myResourceGroup \
    --name myMongoDBCluster \
    --query [properties.provisioningState] \
    --output tsv
```

## <a name="next-steps"></a>Nästa steg
I dessa fall måste ansluta du toohello MongoDB-instansen lokalt från hello VM. Om du vill tooconnect toohello MongoDB-instansen från en annan virtuell dator eller nätverk, se till att lämpliga hello [Nätverkssäkerhetsgruppen regler skapas](nsg-quickstart.md).

De här exemplen distribuera hello core MongoDB-miljö för utveckling. Tillämpa hello krävs säkerhetskonfigurationsalternativ för din miljö. Mer information finns i hello [MongoDB säkerhet docs](https://docs.mongodb.com/manual/security/).

Mer information om hur du skapar med hjälp av mallar finns hello [översikt över Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).

hello Azure Resource Manager-Mallar Använd hello toodownload för tillägget för anpassat skript och köra skript på din virtuella dator. Mer information finns i [Using hello tillägget för Azure anpassat skript med Linux-datorer](extensions-customscript.md).

