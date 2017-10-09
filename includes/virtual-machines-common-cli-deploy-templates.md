
* [Snabbskapa en virtuell dator i Azure](#quick-create-a-vm-in-azure)
* [Distribuera en virtuell dator i Azure från en mall](#deploy-a-vm-in-azure-from-a-template)
* [Skapa en virtuell dator från en anpassad avbildning](#create-a-custom-vm-image)
* [Distribuera en virtuell dator som använder ett virtuellt nätverk och en belastningsutjämnare](#deploy-a-multi-vm-application-that-uses-a-virtual-network-and-an-external-load-balancer)
* [Ta bort en resursgrupp](#remove-a-resource-group)
* [Visa hello logg för en resurs för gruppdistributionen](#show-the-log-for-a-resource-group-deployment)
* [Visa information om en virtuell dator](#display-information-about-a-virtual-machine)
* [Ansluta tooa Linux-baserade virtuella datorer](#log-on-to-a-linux-based-virtual-machine)
* [Stoppa en virtuell dator](#stop-a-virtual-machine)
* [Starta en virtuell dator](#start-a-virtual-machine)
* [Anslut en datadisk](#attach-a-data-disk)

## <a name="getting-ready"></a>Komma igång
Innan du kan använda hello Azure CLI med Azure-resursgrupper, behöver du toohave hello rätt Azure CLI version och ett Azure-konto. Om du inte har hello Azure CLI [installera den](../articles/cli-install-nodejs.md).

### <a name="update-your-azure-cli-version-too090-or-later"></a>Uppdatera din Azure CLI version too0.9.0 eller senare
Typen `azure --version` toosee om du redan har installerat version 0.9.0 eller senare.

```azurecli
azure --version
0.9.0 (node: 0.10.25)
```

Om din version är inte 0.9.0 eller senare behöver du tooupdate den med något av hello interna installationsprogram eller via **npm** genom att skriva `npm update -g azure-cli`.

Du kan också köra Azure CLI som en dockerbehållare med hjälp av följande hello [Docker bild](https://registry.hub.docker.com/u/microsoft/azure-cli/). Kör hello följande kommando från en Docker-värd:

```bash
docker run -it microsoft/azure-cli
```

### <a name="set-your-azure-account-and-subscription"></a>Konfigurera Azure-kontot och Azure-prenumerationen
Om du inte har någon Azure-prenumeration men en MSDN-prenumeration kan du aktivera dina [MSDN-prenumerantförmåner](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/). Du kan även anmäla dig för en [kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/).

Nu [logga in tooyour Azure-konto interaktivt](../articles/xplat-cli-connect.md#scenario-1-azure-login-with-interactive-login) genom att skriva `azure login` och följa hello efterfrågas en interaktiv inloggning upplevelse tooyour Azure-konto. 

> [!NOTE]
> Om du har ett arbets- eller skolkonto ID och du vet du inte har tvåfaktorsautentisering aktiverad, kan du **också** använder `azure login -u` tillsammans med hello arbetsplats eller skola ID toolog i *utan* en interaktiv session. Om du inte har ett arbets- eller skolkonto ID, kan du [skapar ett arbets- eller skolkonto id från ditt personliga Microsoft-konto](../articles/virtual-machines/windows/create-aad-work-id.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) toolog i hello samma sätt.
>
>

Ditt konto kan innehålla fler än en prenumeration. Du kan visa prenumerationerna genom att skriva `azure account list`, som kan se ut ungefär så här:

```azurecli
azure account list
info:    Executing command account list
data:    Name                              Id                                    Tenant Id                            Current
data:    --------------------------------  ------------------------------------  ------------------------------------  -------
data:    Contoso Admin                     xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  true
data:    Fabrikam dev                      xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  false  
data:    Fabrikam test                     xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  false  
data:    Contoso production                xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  false  
```

Du kan ange hello aktuella Azure-prenumeration genom att skriva följande hello. Använd hello prenumeration namn eller hello-ID som har hello-resurser som du vill toomanage.

```azurecli
azure account set <subscription name or ID> true
```

### <a name="switch-toohello-azure-cli-resource-group-mode"></a>Växla toohello Azure CLI resurs Grupperingsläge
Som standard startar hello Azure CLI i läget för hello-hantering (**asm** läge). Ange hello följande tooswitch tooresource Grupperingsläge.

```azurecli
azure config mode arm
```

## <a name="understanding-azure-resource-templates-and-resource-groups"></a>Förstå Azure-resursmallar och -resursgrupper
De flesta program skapas med en kombination av olika resurstyper (t.ex. en eller flera virtuella datorer och lagringskonton, en SQL-databas, ett virtuellt nätverk eller ett innehållsleveransnätverk). Hej standard Azure service management API och hello klassiska Azure-portalen representeras dessa objekt med hjälp av en metod av tjänsten. Den här metoden kräver toodeploy och hantera hello enskilda tjänster individuellt (eller andra verktyg som gör att hitta), och inte som en logisk enhet för distributionen.

*Azure Resource Manager-mallar*, men gör det möjligt för du toodeploy och hantera dessa olika resurser som en logisk distributionsenhet deklarativ överskrids. I stället för imperatively uppmanar Azure vilka toodeploy ett kommando efter en annan, beskrivs i en JSON-fil – alla hello resurser och associerade konfigurationen och distributionen parametrar--hela distributionen och berätta Azure toodeploy resurserna som en grupp.

Du kan sedan hantera hello övergripande livscykel hello gruppera resurser med hjälp av Azure CLI resurs management kommandon för att:

* Stoppa, starta eller ta bort alla hello resurser inom hello grupp samtidigt.
* Använda rollbaserad åtkomstkontroll (RBAC) regler toolock ned säkerhetsbehörigheter på dem.
* Granskningsåtgärder.
* Tagga resurser med ytterligare metadata för bättre spårning.

Du kan lära dig mer om Azure-resursgrupper och vad de kan du göra i hello många [översikt över Azure Resource Manager](../articles/azure-resource-manager/resource-group-overview.md). Om du är intresserad av att skapa mallar, se [Skapa Azure Resource Manager-mallar](../articles/resource-group-authoring-templates.md).

## <a id="quick-create-a-vm-in-azure"></a>Uppgift: Snabbt skapa en virtuell dator i Azure
Du vet ibland vilken bild som du behöver, och du behöver en virtuell dator från den avbildningen just nu och du bryr dig inte så mycket om hello infrastruktur – du kanske har tootest något på en ren VM. Det är då du vill toouse hello `azure vm quick-create` kommandot och skicka hello argument nödvändiga toocreate en virtuell dator och sin infrastruktur.

Skapa först en resursgrupp.

```azurecli
azure group create coreos-quick westus
info:    Executing command group create
+ Getting resource group coreos-quick
+ Creating resource group coreos-quick
info:    Created resource group coreos-quick
data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/coreos-quick
data:    Name:                coreos-quick
data:    Location:            westus
data:    Provisioning State:  Succeeded
data:    Tags:
data:
info:    group create command OK
```

Sedan behöver du en avbildning. toofind en bild med hello Azure CLI, se [navigering och välja avbildningar för virtuella Azure-datorn med PowerShell och hello Azure CLI](../articles/virtual-machines/linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Men här är en kort lista med populära avbildningar för den här artikeln. Vi använder CoreOS stabila avbildning för den här snabbövningen.

> [!NOTE]
> För ComputeImageVersion, du kan också bara kan ange ”senaste” som hello parameter i båda hello mallspråk och hello Azure CLI. Detta gör att du tooalways använder hello senaste och uppdaterad version av hello bild utan toomodify skript och mallar. Detta visas nedan.
>
>

| PublisherName | Erbjudande | Sku | Version |
|:--- |:--- |:--- |:--- |
| OpenLogic |CentOS |7 |7.0.201503 |
| OpenLogic |CentOS |7.1 |7.1.201504 |
| CoreOS |CoreOS |Beta |647.0.0 |
| CoreOS |CoreOS |Stable |633.1.0 |
| MicrosoftDynamicsNAV |DynamicsNAV |2015 |8.0.40459 |
| MicrosoftSharePoint |MicrosoftSharePointServer |2013 |1.0.0 |
| msopentech |Oracle-Database-12c-Weblogic-Server-12c |Standard |1.0.0 |
| msopentech |Oracle-Database-12c-Weblogic-Server-12c |Enterprise |1.0.0 |
| MicrosoftSQLServer |SQL2014-WS2012R2 |Enterprise-Optimized-for-DW |12.0.2430 |
| MicrosoftSQLServer |SQL2014-WS2012R2 |Enterprise-Optimized-for-OLTP |12.0.2430 |
| Canonical |UbuntuServer |12.04.5-LTS |12.04.201504230 |
| Canonical |UbuntuServer |14.04.2-LTS |14.04.201503090 |
| MicrosoftWindowsServer |WindowsServer |2012-Datacenter |3.0.201503 |
| MicrosoftWindowsServer |WindowsServer |2012-R2-Datacenter |4.0.201503 |
| MicrosoftWindowsServer |WindowsServer |Windows-Server-Technical-Preview |5.0.201504 |
| MicrosoftWindowsServerEssentials |WindowsServerEssentials |WindowsServerEssentials |1.0.141204 |
| MicrosoftWindowsServerHPCPack |WindowsServerHPCPack |2012R2 |4.3.4665 |

Skapa den virtuella datorn genom att ange hello `azure vm quick-create` kommandot och är redo för hello anvisningarna. Det bör se ut ungefär så här:

```azurecli
azure vm quick-create
info:    Executing command vm quick-create
Resource group name: coreos-quick
Virtual machine name: coreos
Location name: westus
Operating system Type [Windows, Linux]: linux
ImageURN (format: "publisherName:offer:skus:version"): coreos:coreos:stable:latest
User name: ops
Password: *********
Confirm password: *********
+ Looking up hello VM "coreos"
info:    Using hello VM Size "Standard_A1"
info:    hello [OS, Data] Disk or image configuration requires storage account
+ Retrieving storage accounts
info:    Could not find any storage accounts in hello region "westus", trying toocreate new one
+ Creating storage account "cli9fd3fce49e9a9b3d14302" in "westus"
+ Looking up hello storage account cli9fd3fce49e9a9b3d14302
+ Looking up hello NIC "coreo-westu-1430261891570-nic"
info:    An nic with given name "coreo-westu-1430261891570-nic" not found, creating a new one
+ Looking up hello virtual network "coreo-westu-1430261891570-vnet"
info:    Preparing toocreate new virtual network and subnet
/ Creating a new virtual network "coreo-westu-1430261891570-vnet" [address prefix: "10.0.0.0/16"] with subnet "coreo-westu-1430261891570-sne+" [address prefix: "10.0.1.0/24"]
+ Looking up hello virtual network "coreo-westu-1430261891570-vnet"
+ Looking up hello subnet "coreo-westu-1430261891570-snet" under hello virtual network "coreo-westu-1430261891570-vnet"
info:    Found public ip parameters, trying toosetup PublicIP profile
+ Looking up hello public ip "coreo-westu-1430261891570-pip"
info:    PublicIP with given name "coreo-westu-1430261891570-pip" not found, creating a new one
+ Creating public ip "coreo-westu-1430261891570-pip"
+ Looking up hello public ip "coreo-westu-1430261891570-pip"
+ Creating NIC "coreo-westu-1430261891570-nic"
+ Looking up hello NIC "coreo-westu-1430261891570-nic"
+ Creating VM "coreos"
+ Looking up hello VM "coreos"
+ Looking up hello NIC "coreo-westu-1430261891570-nic"
+ Looking up hello public ip "coreo-westu-1430261891570-pip"
data:    Id                              :/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/coreos-quick/providers/Microsoft.Compute/virtualMachines/coreos
data:    ProvisioningState               :Succeeded
data:    Name                            :coreos
data:    Location                        :westus
data:    FQDN                            :coreo-westu-1430261891570-pip.westus.cloudapp.azure.com
data:    Type                            :Microsoft.Compute/virtualMachines
data:
data:    Hardware Profile:
data:      Size                          :Standard_A1
data:
data:    Storage Profile:
data:      Image reference:
data:        Publisher                   :coreos
data:        Offer                       :coreos
data:        Sku                         :stable
data:        Version                     :633.1.0
data:
data:      OS Disk:
data:        OSType                      :Linux
data:        Name                        :cli9fd3fce49e9a9b3d-os-1430261892283
data:        Caching                     :ReadWrite
data:        CreateOption                :FromImage
data:        Vhd:
data:          Uri                       :https://cli9fd3fce49e9a9b3d14302.blob.core.windows.net/vhds/cli9fd3fce49e9a9b3d-os-1430261892283.vhd
data:
data:    OS Profile:
data:      Computer Name                 :coreos
data:      User Name                     :ops
data:      Linux Configuration:
data:        Disable Password Auth       :false
data:
data:    Network Profile:
data:      Network Interfaces:
data:        Network Interface #1:
data:          Id                        :/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/coreos-quick/providers/Microsoft.Network/networkInterfaces/coreo-westu-1430261891570-nic
data:          Primary                   :true
data:          MAC Address               :00-0D-3A-30-72-E3
data:          Provisioning State        :Succeeded
data:          Name                      :coreo-westu-1430261891570-nic
data:          Location                  :westus
data:            Private IP alloc-method :Dynamic
data:            Private IP address      :10.0.1.4
data:            Public IP address       :104.40.24.124
data:            FQDN                    :coreo-westu-1430261891570-pip.westus.cloudapp.azure.com
info:    vm quick-create command OK
```

Och så fortsätter du med din nya virtuella datorn.

## <a id="deploy-a-vm-in-azure-from-a-template"></a>Uppgift: Distribuera en virtuell dator i Azure från en mall
Använd hello instruktioner i dessa avsnitt toodeploy en ny virtuell Azure-dator med hjälp av en mall med hello Azure CLI. Den här mallen skapar en virtuell dator i ett nytt virtuellt nätverk med ett enda undernät och till skillnad från `azure vm quick-create`, aktiverar du toodescribe vad du vill exakt och upprepa utan fel. Mallen skapar följande:

![](./media/virtual-machines-common-cli-deploy-templates/new-vm.png)

### <a name="step-1-examine-hello-json-file-for-hello-template-parameters"></a>Steg 1: Kontrollera hello JSON-filen för hello mallparametrar
Här följer hello innehållet i hello JSON-fil för hello mall. (hello mall finns också i [GitHub](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-linux/azuredeploy.json).)

Mallar är flexibla, så kan ha valt toogive många parametrar eller valt toooffer endast några få genom att skapa en mall som är mer fast hello designer. Öppna hello mallfilen (det här avsnittet innehåller en mall infogad nedan) i ordning toocollect hello information du behöver toopass hello mall som parametrar och undersöka hello **parametrar** värden.

I det här fallet att hello mallen nedan uppmanas:

* Ett unikt namn på lagringskontot.
* En administratör användarnamn för hello VM.
* Ett lösenord.
* Ett domännamn för hello utanför world toouse.
* Ett versionsnummer till Ubuntu Server – men den accepterar endast en i en lista.

Läs mer om [krav för användarnamn och lösenord](../articles/virtual-machines/linux/faq.md#what-are-the-username-requirements-when-creating-a-vm).

När du har bestämt på dessa värden och är redo toocreate en grupp för och distribuera den här mallen till din Azure-prenumeration.

```json
{
"$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
"contentVersion": "1.0.0.0",
"parameters": {
    "newStorageAccountName": {
    "type": "string",
    "metadata": {
        "description": "Unique DNS name for hello storage account where hello virtual machine's disks will be placed."
    }
    },
    "adminUsername": {
    "type": "string",
    "metadata": {
        "description": "User name for hello virtual machine."
    }
    },
    "adminPassword": {
    "type": "securestring",
    "metadata": {
        "description": "Password for hello virtual machine."
    }
    },
    "dnsNameForPublicIP": {
    "type": "string",
    "metadata": {
        "description": "Unique DNS name for hello public IP used tooaccess hello virtual machine."
    }
    },
    "ubuntuOSVersion": {
    "type": "string",
    "defaultValue": "14.04.2-LTS",
    "allowedValues": [
        "12.04.5-LTS",
        "14.04.2-LTS",
        "15.04"
    ],
    "metadata": {
        "description": "hello Ubuntu version for hello VM. This will pick a fully patched image of this given Ubuntu version. Allowed values: 12.04.5-LTS, 14.04.2-LTS, 15.04."
    }
    }
},
"variables": {
    "location": "West US",
    "imagePublisher": "Canonical",
    "imageOffer": "UbuntuServer",
    "OSDiskName": "osdiskforlinuxsimple",
    "nicName": "myVMNic",
    "addressPrefix": "10.0.0.0/16",
    "subnetName": "Subnet",
    "subnetPrefix": "10.0.0.0/24",
    "storageAccountType": "Standard_LRS",
    "publicIPAddressName": "myPublicIP",
    "publicIPAddressType": "Dynamic",
    "vmStorageAccountContainerName": "vhds",
    "vmName": "MyUbuntuVM",
    "vmSize": "Standard_D1",
    "virtualNetworkName": "MyVNET",
    "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',variables('virtualNetworkName'))]",
    "subnetRef": "[concat(variables('vnetID'),'/subnets/',variables('subnetName'))]"
},
"resources": [
    {
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[parameters('newStorageAccountName')]",
    "apiVersion": "2015-05-01-preview",
    "location": "[variables('location')]",
    "properties": {
        "accountType": "[variables('storageAccountType')]"
    }
    },
    {
    "apiVersion": "2015-05-01-preview",
    "type": "Microsoft.Network/publicIPAddresses",
    "name": "[variables('publicIPAddressName')]",
    "location": "[variables('location')]",
    "properties": {
        "publicIPAllocationMethod": "[variables('publicIPAddressType')]",
        "dnsSettings": {
        "domainNameLabel": "[parameters('dnsNameForPublicIP')]"
        }
    }
    },
    {
    "apiVersion": "2015-05-01-preview",
    "type": "Microsoft.Network/virtualNetworks",
    "name": "[variables('virtualNetworkName')]",
    "location": "[variables('location')]",
    "properties": {
        "addressSpace": {
        "addressPrefixes": [
            "[variables('addressPrefix')]"
        ]
        },
        "subnets": [
        {
            "name": "[variables('subnetName')]",
            "properties": {
            "addressPrefix": "[variables('subnetPrefix')]"
            }
        }
        ]
    }
    },
    {
    "apiVersion": "2015-05-01-preview",
    "type": "Microsoft.Network/networkInterfaces",
    "name": "[variables('nicName')]",
    "location": "[variables('location')]",
    "dependsOn": [
        "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'))]",
        "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
    ],
    "properties": {
        "ipConfigurations": [
        {
            "name": "ipconfig1",
            "properties": {
            "privateIPAllocationMethod": "Dynamic",
            "publicIPAddress": {
                "id": "[resourceId('Microsoft.Network/publicIPAddresses',variables('publicIPAddressName'))]"
            },
            "subnet": {
                "id": "[variables('subnetRef')]"
            }
            }
        }
        ]
    }
    },
    {
    "apiVersion": "2015-05-01-preview",
    "type": "Microsoft.Compute/virtualMachines",
    "name": "[variables('vmName')]",
    "location": "[variables('location')]",
    "dependsOn": [
        "[concat('Microsoft.Storage/storageAccounts/', parameters('newStorageAccountName'))]",
        "[concat('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
    ],
    "properties": {
        "hardwareProfile": {
        "vmSize": "[variables('vmSize')]"
        },
        "osProfile": {
        "computername": "[variables('vmName')]",
        "adminUsername": "[parameters('adminUsername')]",
        "adminPassword": "[parameters('adminPassword')]"
        },
        "storageProfile": {
        "imageReference": {
            "publisher": "[variables('imagePublisher')]",
            "offer": "[variables('imageOffer')]",
            "sku": "[parameters('ubuntuOSVersion')]",
            "version": "latest"
        },
        "osDisk": {
            "name": "osdisk",
            "vhd": {
            "uri": "[concat('http://',parameters('newStorageAccountName'),'.blob.core.windows.net/',variables('vmStorageAccountContainerName'),'/',variables('OSDiskName'),'.vhd')]"
            },
            "caching": "ReadWrite",
            "createOption": "FromImage"
        }
        },
        "networkProfile": {
        "networkInterfaces": [
            {
            "id": "[resourceId('Microsoft.Network/networkInterfaces',variables('nicName'))]"
            }
        ]
        }
    }
    }
]
}
```

### <a name="step-2-create-hello-virtual-machine-by-using-hello-template"></a>Steg 2: Skapa hello virtuell dator med hjälp av hello mall
När du har din parametervärden som är klar måste du skapa en resursgrupp för distributionen mall och sedan distribuera hello mallen.

toocreate hello resursgrupp, typen `azure group create <group name> <location>` med hello namnet hello-grupp som du vill och hello datacenter plats som du vill toodeploy. Detta händer snabbt:

```azurecli
azure group create myResourceGroup westus
info:    Executing command group create
+ Getting resource group myResourceGroup
+ Creating resource group myResourceGroup
info:    Created resource group myResourceGroup
data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myResourceGroup
data:    Name:                myResourceGroup
data:    Location:            westus
data:    Provisioning State:  Succeeded
data:    Tags:
data:
info:    group create command OK
```

Nu toocreate hello distribution, anropet `azure group deployment create` och skicka:

* hello mallfilen (om du har sparat hello ovan JSON tooa lokala mallfilen).
* En mall URI (om du vill toopoint på hello-filen i GitHub eller några andra webbadressen).
* hello resursgrupp som du vill toodeploy.
* Ett valfritt distributionsnamn.

Du kommer att tillfrågas toosupply hello värdena för parametrarna i hello ”parametrar” avsnittet i hello JSON-fil. När du har angett alla hello parametervärden börjar din distribution.

Här är ett exempel:

```azurecli
azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-linux/azuredeploy.json myResourceGroup firstDeployment
info:    Executing command group deployment create
info:    Supply values for hello following parameters
newStorageAccountName: storageaccount
adminUsername: ops
adminPassword: password
dnsNameForPublicIP: newdomainname
```

Du får hello efter typ av information:

```azurecli
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "firstDeployment"
+ Registering providers
info:    Registering provider microsoft.storage
info:    Registering provider microsoft.network
info:    Registering provider microsoft.compute
+ Waiting for deployment toocomplete
data:    DeploymentName     : firstDeployment
data:    ResourceGroupName  : myResourceGroup
data:    ProvisioningState  : Succeeded
data:    Timestamp          : 2015-04-28T07:53:55.1828878Z
data:    Mode               : Incremental
data:    TemplateLink       : https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-simple-linux-vm/azuredeploy.json
data:    ContentVersion     : 1.0.0.0
data:    Name                   Type          Value
data:    ---------------------  ------------  -------------
data:    newStorageAccountName  String        storageaccount
data:    adminUsername          String        ops
data:    adminPassword          SecureString  undefined
data:    dnsNameForPublicIP     String        newdomainname
data:    ubuntuOSVersion        String        14.10
info:    group deployment create command OK
```


## <a id="create-a-custom-vm-image"></a>Uppgift: Skapa en anpassad avbildning av en virtuell dator
Du har sett hello grundläggande användningen av mallar som ovan, så vi kan nu använda liknande instruktioner toocreate en egen virtuell dator från en viss VHD-fil i Azure med hjälp av en mall via hello Azure CLI. här hello skillnaden är att den här mallen skapar en virtuell dator från en angiven virtuell hårddisk (VHD).

### <a name="step-1-examine-hello-json-file-for-hello-template"></a>Steg 1: Granska hello JSON-fil för hello mall
Här följer hello innehållet i hello JSON-fil för hello-mall som det här avsnittet används som exempel. (hello mall finns också i [GitHub](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-from-user-image/azuredeploy.json).)

Igen, måste toofind hello värden tooenter för hello-parametrar som inte har standardvärden. När du kör hello `azure group deployment create` kommandot hello Azure CLI uppmanas du tooenter dessa värden.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json",
    "contentVersion": "1.0.0.0",
    "parameters" : {
        "userImageStorageAccountName": {
            "type": "string",
            "defaultValue" : "userImageStorageAccountName"
        },
        "userImageStorageContainerName" : {
            "type" : "string",
            "defaultValue" : "userImageStorageContainerName"
        },
        "userImageVhdName" : {
            "type" : "string",
            "defaultValue" : "userImageVhdName"
        },
        "dnsNameForPublicIP" : {
            "type" : "string",
            "defaultValue": "uniqueDnsNameForPublicIP"
        },
        "adminUserName": {
            "type": "string"
        },
        "adminPassword": {
            "type": "securestring"
        },
        "osType" : {
            "type" : "string",
            "allowedValues" : [
                "windows",
                "linux"
            ]
        },
        "subscriptionId":{
            "type" : "string"
        },
        "location": {
            "type": "String",
            "defaultValue" : "West US"
        },
        "vmSize": {
            "type": "string",
            "defaultValue": "Standard_A2"
        },
        "publicIPAddressName": {
            "type": "string",
            "defaultValue" : "myPublicIP"
        },
        "vmName": {
            "type": "string",
            "defaultValue" : "myVM"
        },
        "virtualNetworkName":{
            "type" : "string",
            "defaultValue" : "myVNET"
        },
        "nicName":{
            "type" : "string",
            "defaultValue":"myNIC"
        }
    },
    "variables": {
        "addressPrefix":"10.0.0.0/16",
        "subnet1Name": "Subnet-1",
        "subnet2Name": "Subnet-2",
        "subnet1Prefix" : "10.0.0.0/24",
        "subnet2Prefix" : "10.0.1.0/24",
        "publicIPAddressType" : "Dynamic",
        "vnetID":"[resourceId('Microsoft.Network/virtualNetworks',parameters('virtualNetworkName'))]",
        "subnet1Ref" : "[concat(variables('vnetID'),'/subnets/',variables('subnet1Name'))]",
        "userImageName" : "[concat('http://',parameters('userImageStorageAccountName'),'.blob.core.windows.net/',parameters('userImageStorageContainerName'),'/',parameters('userImageVhdName'))]",
        "osDiskVhdName" : "[concat('http://',parameters('userImageStorageAccountName'),'.blob.core.windows.net/',parameters('userImageStorageContainerName'),'/',parameters('vmName'),'osDisk.vhd')]"
    },
    "resources": [
    {
        "apiVersion": "2014-12-01-preview",
        "type": "Microsoft.Network/publicIPAddresses",
        "name": "[parameters('publicIPAddressName')]",
        "location": "[parameters('location')]",
        "properties": {
            "publicIPAllocationMethod": "[variables('publicIPAddressType')]",
            "dnsSettings": {
                "domainNameLabel": "[parameters('dnsNameForPublicIP')]"
            }
        }
    },
    {
        "apiVersion": "2014-12-01-preview",
        "type": "Microsoft.Network/virtualNetworks",
        "name": "[parameters('virtualNetworkName')]",
        "location": "[parameters('location')]",
        "properties": {
        "addressSpace": {
            "addressPrefixes": [
            "[variables('addressPrefix')]"
            ]
        },
        "subnets": [
            {
            "name": "[variables('subnet1Name')]",
            "properties" : {
                "addressPrefix": "[variables('subnet1Prefix')]"
            }
            },
            {
            "name": "[variables('subnet2Name')]",
            "properties" : {
                "addressPrefix": "[variables('subnet2Prefix')]"
            }
            }
        ]
        }
    },
    {
        "apiVersion": "2014-12-01-preview",
        "type": "Microsoft.Network/networkInterfaces",
        "name": "[parameters('nicName')]",
        "location": "[parameters('location')]",
        "dependsOn": [
            "[concat('Microsoft.Network/publicIPAddresses/', parameters('publicIPAddressName'))]",
            "[concat('Microsoft.Network/virtualNetworks/', parameters('virtualNetworkName'))]"
        ],
        "properties": {
            "ipConfigurations": [
            {
                "name": "ipconfig1",
                "properties": {
                    "privateIPAllocationMethod": "Dynamic",
                    "publicIPAddress": {
                        "id": "[resourceId('Microsoft.Network/publicIPAddresses',parameters('publicIPAddressName'))]"
                    },
                    "subnet": {
                        "id": "[variables('subnet1Ref')]"
                    }
                }
            }
            ]
        }
    },
    {
        "apiVersion": "2014-12-01-preview",
        "type": "Microsoft.Compute/virtualMachines",
        "name": "[parameters('vmName')]",
        "location": "[parameters('location')]",
        "dependsOn": [
            "[concat('Microsoft.Network/networkInterfaces/', parameters('nicName'))]"
        ],
        "properties": {
            "hardwareProfile": {
                "vmSize": "[parameters('vmSize')]"
            },
            "osProfile": {
                "computername": "[parameters('vmName')]",
                "adminUsername": "[parameters('adminUsername')]",
                "adminPassword": "[parameters('adminPassword')]"
            },
            "storageProfile": {
                "osDisk" : {
                    "name" : "[concat(parameters('vmName'),'-osDisk')]",
                    "osType" : "[parameters('osType')]",
                    "caching" : "ReadWrite",
                    "image" : {
                        "uri" : "[variables('userImageName')]"
                    },
                    "vhd" : {
                        "uri" : "[variables('osDiskVhdName')]"
                    }
                }
            },
            "networkProfile": {
                "networkInterfaces" : [
                {
                    "id": "[resourceId('Microsoft.Network/networkInterfaces',parameters('nicName'))]"
                }
                ]
            }
        }
    }
    ]
}
```

### <a name="step-2-obtain-hello-vhd"></a>Steg 2: Hämta hello VHD
Du behöver givetvis ha en VHD-fil för detta. Du kan använda en som du redan har i Azure eller också kan du ladda upp en.

En Windows-baserad virtuell dator, se [skapa och ladda upp en Windows Server VHD-tooAzure](../articles/virtual-machines/windows/classic/createupload-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

En Linux-baserade virtuella datorer, se [skapa och ladda upp en virtuell hårddisk som innehåller hello Linux-operativsystem](../articles/virtual-machines/linux/classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

### <a name="step-3-create-hello-virtual-machine-by-using-hello-template"></a>Steg 3: Skapa hello virtuell dator med hjälp av hello mall
Nu är du redo toocreate en ny virtuell dator baserat på hello VHD. Skapa en grupp toodeploy, med hjälp av `azure group create <location>`:

```azurecli
azure group create myResourceGroupUser eastus
info:    Executing command group create
+ Getting resource group myResourceGroupUser
+ Creating resource group myResourceGroupUser
info:    Created resource group myResourceGroupUser
data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myResourceGroupUser
data:    Name:                myResourceGroupUser
data:    Location:            eastus
data:    Provisioning State:  Succeeded
data:    Tags:
data:
info:    group create command OK
```

Sedan skapar hello distribution med hjälp av hello `--template-uri` alternativet toocall i direkt hello-mall (eller så kan du använda hello `--template-file` alternativet toouse en fil som du har sparat lokalt). Observera att eftersom hello mall har standardvärden som angetts kan du tillfrågas om endast några saker. Om du distribuerar hello mallen på olika ställen hända att vissa namngivning kollisioner inträffar med standardvärden för hello (särskilt hello DNS-namn du skapar).

```azurecli
azure group deployment create \
> --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-from-user-image/azuredeploy.json \
> myResourceGroup \
> customVhdDeployment
info:    Executing command group deployment create
info:    Supply values for hello following parameters
adminUserName: ops
adminPassword: password
osType: linux
subscriptionId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

Utdata ser ut ungefär så hello följande:

```azurecli
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "customVhdDeployment"
+ Registering providers
info:    Registering provider microsoft.network
info:    Registering provider microsoft.compute
+ Waiting for deployment toocomplete
error:   Deployment provisioning state was not successful
data:    DeploymentName     : customVhdDeployment
data:    ResourceGroupName  : myResourceGroupUser
data:    ProvisioningState  : Succeeded
data:    Timestamp          : 2015-04-28T14:55:48.0963829Z
data:    Mode               : Incremental
data:    TemplateLink       : https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-from-user-image/azuredeploy.json
data:    ContentVersion     : 1.0.0.0
data:    Name                           Type          Value
data:    -----------------------------  ------------  ------------------------------------
data:    userImageStorageAccountName    String        userImageStorageAccountName
data:    userImageStorageContainerName  String        userImageStorageContainerName
data:    userImageVhdName               String        userImageVhdName
data:    dnsNameForPublicIP             String        uniqueDnsNameForPublicIP
data:    adminUserName                  String        ops
data:    adminPassword                  SecureString  undefined
data:    osType                         String        linux
data:    subscriptionId                 String        xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
data:    location                       String        West US
data:    vmSize                         String        Standard_A2
data:    publicIPAddressName            String        myPublicIP
data:    vmName                         String        myVM
data:    virtualNetworkName             String        myVNET
data:    nicName                        String        myNIC
info:    group deployment create command OK
```

## <a id="deploy-a-multi-vm-application-that-uses-a-virtual-network-and-an-external-load-balancer"></a>Uppgift: Distribuera ett VM-multiprogram som använder ett virtuellt nätverk och en extern belastningsutjämnare
Den här mallen kan du toocreate två virtuella datorer under en belastningsutjämnare och konfigurera en regel för belastningsutjämning på Port 80. Den här mallen distribuerar också ett lagringskonto, ett virtuellt nätverk, en offentlig IP-adress, en tillgänglighetsuppsättning och nätverksgränssnitt.

![](./media/virtual-machines-common-cli-deploy-templates/multivmextlb.png)

Följ dessa steg toodeploy en multi-VM-program som använder ett virtuellt nätverk och en belastningsutjämnare med hjälp av en Resource Manager-mall i hello GitHub mall databasen via Azure PowerShell-kommandon.

### <a name="step-1-examine-hello-json-file-for-hello-template"></a>Steg 1: Granska hello JSON-fil för hello mall
Här följer hello innehållet i hello JSON-fil för hello mall. Om du vill att hello senaste versionen, det finns [på hello GitHub-lagringsplatsen för mallar](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.json). Det här avsnittet använder hello `--template-uri` växel toocall i hello mallen, men du kan också använda hello `--template-file` växla toopass en lokal version.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "location": {
            "type": "string",
            "metadata": {
                "description": "Location of resources"
            }
        },
        "storageAccountName": {
            "type": "string",
            "metadata": {
                "description": "Name of storage account"
            }
        },
        "adminUsername": {
            "type": "string",
            "metadata": {
                "description": "Admin user name"
            }
        },
        "adminPassword": {
            "type": "securestring",
            "metadata": {
                "description": "Admin password"
            }
        },
        "dnsNameforLBIP": {
            "type": "string",
            "metadata": {
                "description": "DNS for load balancer IP"
            }
        },
        "backendPort": {
            "type": "int",
            "defaultValue": 3389,
            "metadata": {
                "description": "Back-end port"
            }
        },
        "vmNamePrefix": {
            "type": "string",
            "defaultValue": "myVM",
            "metadata": {
                "description": "Prefix toouse for VM names"
            }
        },
        "vmSourceImageName": {
            "type": "string",
            "defaultValue": "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-201412.01-en.us-127GB.vhd"
        },
        "lbName": {
            "type": "string",
            "defaultValue": "myLB",
            "metadata": {
                "description": "Load balancer name"
            }
        },
        "nicNamePrefix": {
            "type": "string",
            "defaultValue": "nic",
            "metadata": {
                "description": "Network interface name prefix"
            }
        },
        "publicIPAddressName": {
            "type": "string",
            "defaultValue": "myPublicIP",
            "metadata": {
                "description": "Public IP name"
            }
        },
        "vnetName": {
            "type": "string",
            "defaultValue": "myVNET",
            "metadata": {
                "description": "Virtual network name"
            }
        },
        "vmSize": {
            "type": "string",
            "defaultValue": "Standard_A1",
            "metadata": {
                "description": "Size of hello VM"
            }
        }
    },
    "variables": {
        "storageAccountType": "Standard_LRS",
        "vmStorageAccountContainerName": "vhds",
        "availabilitySetName": "myAvSet",
        "addressPrefix": "10.0.0.0/16",
        "subnetName": "Subnet-1",
        "subnetPrefix": "10.0.0.0/24",
        "publicIPAddressType": "Dynamic",
        "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',parameters('vnetName'))]",
        "subnetRef": "[concat(variables('vnetID'),'/subnets/',variables ('subnetName'))]",
        "publicIPAddressID": "[resourceId('Microsoft.Network/publicIPAddresses',parameters('publicIPAddressName'))]",
        "lbID": "[resourceId('Microsoft.Network/loadBalancers',parameters('lbName'))]",
        "numberOfInstances": 2,
        "nicId1": "[resourceId('Microsoft.Network/networkInterfaces',concat(parameters('nicNamePrefix'), 0))]",
        "nicId2": "[resourceId('Microsoft.Network/networkInterfaces',concat(parameters('nicNamePrefix'), 1))]",
        "frontEndIPConfigID": "[concat(variables('lbID'),'/frontendIPConfigurations/LBFE')]",
        "backEndIPConfigID1": "[concat(variables('nicId1'),'/ipConfigurations/ipconfig1')]",
        "backEndIPConfigID2": "[concat(variables('nicId2'),'/ipConfigurations/ipconfig1')]",
        "sourceImageName": "[concat('/', subscription().subscriptionId,'/services/images/',parameters('vmSourceImageName'))]",
        "lbPoolID": "[concat(variables('lbID'),'/backendAddressPools/LBBE')]",
        "lbProbeID": "[concat(variables('lbID'),'/probes/tcpProbe')]"
    },
    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[parameters('storageAccountName')]",
            "apiVersion": "2015-05-01-preview",
            "location": "[parameters('location')]",
            "properties": {
                "accountType": "[variables('storageAccountType')]"
            }
        },
        {
            "type": "Microsoft.Compute/availabilitySets",
            "name": "[variables('availabilitySetName')]",
            "apiVersion": "2015-05-01-preview",
            "location": "[parameters('location')]",
            "properties": { }
        },
        {
            "apiVersion": "2015-05-01-preview",
            "type": "Microsoft.Network/publicIPAddresses",
            "name": "[parameters('publicIPAddressName')]",
            "location": "[parameters('location')]",
            "properties": {
                "publicIPAllocationMethod": "[variables('publicIPAddressType')]",
                "dnsSettings": {
                    "domainNameLabel": "[parameters('dnsNameforLBIP')]"
                }
            }
        },
        {
            "apiVersion": "2015-05-01-preview",
            "type": "Microsoft.Network/virtualNetworks",
            "name": "[parameters('vnetName')]",
            "location": "[parameters('location')]",
            "properties": {
                "addressSpace": {
                    "addressPrefixes": [
                        "[variables('addressPrefix')]"
                    ]
                },
                "subnets": [
                    {
                        "name": "[variables('subnetName')]",
                        "properties": {
                            "addressPrefix": "[variables('subnetPrefix')]"
                        }
                    }
                ]
            }
        },
        {
            "apiVersion": "2015-05-01-preview",
            "type": "Microsoft.Network/networkInterfaces",
            "name": "[concat(parameters('nicNamePrefix'), copyindex())]",
            "location": "[parameters('location')]",
            "copy": {
                "name": "nicLoop",
                "count": "[variables('numberOfInstances')]"
            },
            "dependsOn": [
                "[concat('Microsoft.Network/virtualNetworks/', parameters('vnetName'))]"
            ],
            "properties": {
                "ipConfigurations": [
                    {
                        "name": "ipconfig1",
                        "properties": {
                            "privateIPAllocationMethod": "Dynamic",
                            "subnet": {
                                "id": "[variables('subnetRef')]"
                            }
                        },
                        "loadBalancerBackendAddressPools": [
                            {
                                "id": "[concat('Microsoft.Network/loadBalancers/',parameters('lbName'),'/backendAddressPools/LBBE')]"
                            }
                        ],
                        "loadBalancerInboundNatRules": [
                            {
                                "id": "[concat('Microsoft.Network/loadBalancers/',parameters('lbName'),'/inboundNatRule/RDP-VM', copyindex())]"
                            }
                        ]


                    }
                ]

            }
        },
        {
            "apiVersion": "2015-05-01-preview",
            "name": "[parameters('lbName')]",
            "type": "Microsoft.Network/loadBalancers",
            "location": "[parameters('location')]",
            "dependsOn": [
                "nicLoop",
                "[concat('Microsoft.Network/publicIPAddresses/', parameters('publicIPAddressName'))]"
            ],
            "properties": {
                "frontendIPConfigurations": [
                    {
                        "name": "LBFE",
                        "properties": {
                            "publicIPAddress": {
                                "id": "[variables('publicIPAddressID')]"
                            }
                        }
                    }
                ],
                "backendAddressPools": [
                    {
                        "name": "LBBE"

                    }
                ],
                "inboundNatRules": [
                    {
                        "name": "RDP-VM1",
                        "properties": {
                            "frontendIPConfiguration":
                                {
                                    "id": "[variables('frontEndIPConfigID')]"
                                },
                            "protocol": "tcp",
                            "frontendPort": 50001,
                            "backendPort": 3389,
                            "enableFloatingIP": false
                        }
                    },
                    {
                        "name": "RDP-VM2",
                        "properties": {
                            "frontendIPConfiguration":
                                {
                                    "id": "[variables('frontEndIPConfigID')]"
                                },
                            "protocol": "tcp",
                            "frontendPort": 50002,
                            "backendPort": 3389,
                            "enableFloatingIP": false
                        }
                    }
                ],
                "loadBalancingRules": [
                    {
                        "name": "LBRule",
                        "properties": {
                            "frontendIPConfiguration": {
                                "id": "[variables('frontEndIPConfigID')]"
                            },
                            "backendAddressPool": {
                                "id": "[variables('lbPoolID')]"
                            },
                            "protocol": "tcp",
                            "frontendPort": 80,
                            "backendPort": 80,
                            "enableFloatingIP": false,
                            "idleTimeoutInMinutes": 5,
                            "probe": {
                                "id": "[variables('lbProbeID')]"
                            }
                        }
                    }
                ],
                "probes": [
                    {
                        "name": "tcpProbe",
                        "properties": {
                            "protocol": "tcp",
                            "port": 80,
                            "intervalInSeconds": "5",
                            "numberOfProbes": "2"
                        }
                    }
                ]
            }
        },
        {
            "apiVersion": "2015-05-01-preview",
            "type": "Microsoft.Compute/virtualMachines",
            "name": "[concat(parameters('vmNamePrefix'), copyindex())]",
            "copy": {
                "name": "virtualMachineLoop",
                "count": "[variables('numberOfInstances')]"
            },
            "location": "[parameters('location')]",
            "dependsOn": [
                "[concat('Microsoft.Storage/storageAccounts/', parameters('storageAccountName'))]",
                "[concat('Microsoft.Network/networkInterfaces/', parameters('nicNamePrefix'), copyindex())]",
                "[concat('Microsoft.Compute/availabilitySets/', variables('availabilitySetName'))]"
            ],
            "properties": {
                "availabilitySet": {
                    "id": "[resourceId('Microsoft.Compute/availabilitySets',variables('availabilitySetName'))]"
                },
                "hardwareProfile": {
                    "vmSize": "[parameters('vmSize')]"
                },
                "osProfile": {
                    "computername": "[concat(parameters('vmNamePrefix'), copyIndex())]",
                    "adminUsername": "[parameters('adminUsername')]",
                    "adminPassword": "[parameters('adminPassword')]"
                },
                "storageProfile": {
                    "sourceImage": {
                        "id": "[variables('sourceImageName')]"
                    },
                    "destinationVhdsContainer": "[concat('http://',parameters('storageAccountName'),'.blob.core.windows.net/',variables('vmStorageAccountContainerName'),'/')]"
                },
                "networkProfile": {
                    "networkInterfaces": [
                        {
                            "id": "[resourceId('Microsoft.Network/networkInterfaces',concat(parameters('nicNamePrefix'),copyindex()))]"
                        }
                    ]
                }
            }
        }
    ]
}
```

### <a name="step-2-create-hello-deployment-by-using-hello-template"></a>Steg 2: Skapa hello distribution med hjälp av hello mall
Skapa en resursgrupp för hello mallen med hjälp av `azure group create <location>`. Skapa sedan en distribution i den resursgruppen med hjälp av `azure group deployment create` och skicka hello resursgrupp, skickar du ett distributionsnamn och svara på hello anvisningarna för parametrar i hello-mall som inte har standardvärden.

```azurecli
azure group create lbgroup westus
info:    Executing command group create
+ Getting resource group lbgroup
+ Creating resource group lbgroup
info:    Created resource group lbgroup
data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/lbgroup
data:    Name:                lbgroup
data:    Location:            westus
data:    Provisioning State:  Succeeded
data:    Tags:
data:
info:    group create command OK
```

Nu använda hello `azure group deployment create` kommandot och hello `--template-uri` alternativet toodeploy hello mallen. Vara beredd med dina parametervärden när du uppmanas till det, enligt nedan.

```azurecli
azure group deployment create \
> --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.json \
> lbgroup \
> newdeployment
info:    Executing command group deployment create
info:    Supply values for hello following parameters
location: westus
newStorageAccountName: storagename
adminUsername: ops
adminPassword: password
dnsNameforLBIP: lbdomainname
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "newdeployment"
+ Registering providers
info:    Registering provider microsoft.storage
info:    Registering provider microsoft.compute
info:    Registering provider microsoft.network
+ Waiting for deployment toocomplete
data:    DeploymentName     : newdeployment
data:    ResourceGroupName  : lbgroup
data:    ProvisioningState  : Succeeded
data:    Timestamp          : 2015-04-28T20:58:40.1678876Z
data:    Mode               : Incremental
data:    TemplateLink       : https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.json
data:    ContentVersion     : 1.0.0.0
data:    Name                   Type          Value
data:    ---------------------  ------------  ----------------------
data:    location               String        westus
data:    newStorageAccountName  String        storagename
data:    adminUsername          String        ops
data:    adminPassword          SecureString  undefined
data:    dnsNameforLBIP         String        lbdomainname
data:    backendPort            Int           3389
data:    vmNamePrefix           String        myVM
data:    imagePublisher         String        MicrosoftWindowsServer
data:    imageOffer             String        WindowsServer
data:    imageSKU               String        2012-R2-Datacenter
data:    lbName                 String        myLB
data:    nicNamePrefix          String        nic
data:    publicIPAddressName    String        myPublicIP
data:    vnetName               String        myVNET
data:    vmSize                 String        Standard_A1
info:    group deployment create command OK
```

Observera att den här mallen distribuerar en Windows Server-avbildning. Den kan dock enkelt ersättas av en Linux-avbildning. Vill toocreate ett Docker-kluster med flera swarm cheferna? [Det kan du göra](https://azure.microsoft.com/documentation/templates/docker-swarm-cluster/).

## <a id="remove-a-resource-group"></a>Uppgift: Ta bort en resursgrupp
Kom ihåg att du kan distribuera tooa resursgrupp, men om du är klar med en kan du ta bort den med hjälp av `azure group delete <group name>`.

```azurecli
azure group delete myResourceGroup
info:    Executing command group delete
Delete resource group myResourceGroup? [y/n] y
+ Deleting resource group myResourceGroup
info:    group delete command OK
```

## <a id="show-the-log-for-a-resource-group-deployment"></a>Uppgift: Visa hello logg för en resurs för gruppdistributionen
Det här är vanligt när man skapar eller använder mallar. hello anropet toodisplay hello-distributionsloggar för en grupp är `azure group log show <groupname>`, som visar lite information som är användbar för att förstå varför något hände--inte eller. (Mer information om hur du felsöker dina distributioner, samt annan information om problem finns i avsnittet [Troubleshoot common Azure deployment errors with Azure Resource Manager (Felsöka vanliga Azure-distributionsfel med Azure Resource Manager)](../articles/azure-resource-manager/resource-manager-common-deployment-errors.md).)

tootarget specifika problem, till exempel kan du använda verktyg som **jq** tooquery saker lite mer exakt, till exempel vilka enskilda fel måste toocorrect. hello följande exempel används **jq** tooparse en distribution i felloggen **lbgroup**, som söker efter fel.

```azurecli
azure group log show lbgroup -l --json | jq '.[] | select(.status.value == "Failed") | .properties'
```
Du kan snabbt identifiera vad som har gått fel, åtgärda felet och försöka igen. I följande fall hello, hello mallen hade skapar två virtuella datorer på hello samtidigt som skapas ett lås på hello VHD. (När vi har ändrats hello mallen hello distributionen lyckades snabbt.)

```json
{
    "statusCode": "Conflict",
    "statusMessage": "{\"status\":\"Failed\",\"error\":{\"code\":\"ResourceDeploymentFailure\",\"message\":\"hello resource operation completed with terminal provisioning state 'Failed'.\",\"details\":[{\"code\":\"AcquireDiskLeaseFailed\",\"message\":\"Failed tooacquire lease while creating disk 'osdisk' using blob with URI http://storage.blob.core.windows.net/vhds/osdisk.vhd.\"}]}}"
}
```

## <a id="display-information-about-a-virtual-machine"></a>Uppgift: Visa information om en virtuell dator
Du kan se information om specifika virtuella datorer i resursgruppen med hjälp av hello `azure vm show <groupname> <vmname>` kommando. Om du har mer än en virtuell dator i din grupp, måste du kanske först toolist hello virtuella datorer i en grupp med hjälp av `azure vm list <groupname>`.

```azurecli
azure vm list zoo
info:    Executing command vm list
+ Getting virtual machines
data:    Name   ProvisioningState  Location  Size
data:    -----  -----------------  --------  -----------
data:    myVM0  Succeeded          westus    Standard_A1
data:    myVM1  Failed             westus    Standard_A1
```

Och sedan söka efter myVM1:

```azurecli
azure vm show zoo myVM1
info:    Executing command vm show
+ Looking up hello VM "myVM1"
+ Looking up hello NIC "nic1"
data:    Id                              :/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/zoo/providers/Microsoft.Compute/virtualMachines/myVM1
data:    ProvisioningState               :Failed
data:    Name                            :myVM1
data:    Location                        :westus
data:    Type                            :Microsoft.Compute/virtualMachines
data:
data:    Hardware Profile:
data:      Size                          :Standard_A1
data:
data:    Storage Profile:
data:      Image reference:
data:        Publisher                   :MicrosoftWindowsServer
data:        Offer                       :WindowsServer
data:        Sku                         :2012-R2-Datacenter
data:        Version                     :latest
data:
data:      OS Disk:
data:        OSType                      :Windows
data:        Name                        :osdisk
data:        Caching                     :ReadWrite
data:        CreateOption                :FromImage
data:        Vhd:
data:          Uri                       :http://zoostorageralph.blob.core.windows.net/vhds/osdisk.vhd
data:
data:    OS Profile:
data:      Computer Name                 :myVM1
data:      User Name                     :ops
data:      Windows Configuration:
data:        Provision VM Agent          :true
data:        Enable automatic updates    :true
data:
data:    Network Profile:
data:      Network Interfaces:
data:        Network Interface #1:
data:          Id                        :/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/zoo/providers/Microsoft.Network/networkInterfaces/nic1
data:          Primary                   :false
data:          Provisioning State        :Succeeded
data:          Name                      :nic1
data:          Location                  :westus
data:            Private IP alloc-method :Dynamic
data:            Private IP address      :10.0.0.5
data:
data:    AvailabilitySet:
data:      Id                            :/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/zoo/providers/Microsoft.Compute/availabilitySets/MYAVSET
info:    vm show command OK
```

> [!NOTE]
> Om du vill tooprogrammatically store och manipulera hello utdata från konsolkommandon du toouse parsning av JSON-verktyget som  **[jq](https://github.com/stedolan/jq)**  eller  **[jsawk](https://github.com/micha/jsawk)** , eller språk bibliotek som är bra för hello-aktivitet.
>
>

## <a id="log-on-to-a-linux-based-virtual-machine"></a>Uppgift: Logga in tooa Linux-baserade virtuella datorer
Linux-datorer är vanligtvis anslutna toothrough SSH. Mer information finns i [hur toouse SSH med Linux på Azure](../articles/virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a id="stop-a-virtual-machine"></a>Uppgift: Stoppa en virtuell dator
Kör följande kommando:

```azurecli
azure vm stop <group name> <virtual machine name>
```

> [!IMPORTANT]
> Använd den här parametern tookeep hello virtuella IP-Adressen (VIP) för hello vnet om det är hello sista virtuella datorn i det virtuella nätverket. <br><br> Om du använder hello `StayProvisioned` parametern du faktureras fortfarande för hello VM.
>
>

## <a id="start-a-virtual-machine"></a>Uppgift: Starta en virtuell dator
Kör följande kommando:

```azurecli
azure vm start <group name> <virtual machine name>
```

## <a id="attach-a-data-disk"></a>Uppgift: Anslut en datadisk
Du måste också toodecide om tooattach en ny disk eller en som innehåller data. För en ny disk hello skapar hello VHD-filen och bifogar i hello samma kommando.

tooattach kör det här kommandot för en ny disk:

```azurecli
    azure vm disk attach-new <resource-group> <vm-name> <size-in-gb>
```

tooattach en befintlig datadisk kör detta kommando:

```azurecli
azure vm disk attach <resource-group> <vm-name> [vhd-url]
```

Sedan måste toomount hello disk som vanligt i Linux.

## <a name="next-steps"></a>Nästa steg
Långt fler exempel på användning av Azure CLI med hello **arm** läge, se [Using hello Azure CLI för Mac, Linux och Windows med Azure Resource Manager](../articles/xplat-cli-azure-resource-manager.md). toolearn mer om Azure-resurser och deras begrepp finns [översikt över Azure Resource Manager](../articles/azure-resource-manager/resource-group-overview.md).

Fler mallar som du kan använda finns i [Azure Quickstart-mallar](https://azure.microsoft.com/documentation/templates/) och [Programramverk med hjälp av mallar](../articles/virtual-machines/linux/app-frameworks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
