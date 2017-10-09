---
title: "aaaBest metoder för att skapa Resource Manager-mallar | Microsoft Docs"
description: "Riktlinjer för att förenkla Azure Resource Manager-mallar."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 31b10deb-0183-47ce-a5ba-6d0ff2ae8ab3
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: tomfitz
ms.openlocfilehash: ec9bbe218c4f2c6a92ca44b5e9c9c71029e22151
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="best-practices-for-creating-azure-resource-manager-templates"></a>Metodtips för att skapa Azure Resource Manager-mallar
Dessa riktlinjer kan hjälpa dig att skapa Azure Resource Manager-mallar som är tillförlitlig och enkel toouse. hello riktlinjer är bara förslag. De är inte krav, om inget annat anges. Ditt scenario kan kräva en variant av en av hello följande metoder eller exempel.

## <a name="resource-names"></a>Resursnamn
I allmänhet kan arbeta du med tre typer av resursnamn i Resource Manager:

* Resursnamn som måste vara unika.
* Resursnamn som inte krävs toobe unika, men du väljer tooprovide ett namn som hjälper dig att identifiera en resurs baserat på kontext.
* Resursnamn som kan vara generiska.

 Information om begränsningar för resursen finns [rekommenderas namnkonventionerna för Azure-resurser](../guidance/guidance-naming-conventions.md).

### <a name="unique-resource-names"></a>Unikt namn
Du måste ange ett unikt resursnamn för någon resurstyp av som har en slutpunkt för åtkomst av data. Några vanliga typer av resurser som kräver ett unikt namn är:

* Azure Storage<sup>1</sup> 
* Funktionen Web Apps i Azure App Service
* SQL Server
* Azure Key Vault
* Azure Redis Cache
* Azure Batch
* Azure Traffic Manager
* Azure Search
* Azure HDInsight

<sup>1</sup> lagringskontonamn måste också vara gemener, 24 tecken eller mindre, och inte har någon bindestreck.

Om du anger en parameter för en resurs måste ange du ett unikt namn när du distribuerar hello resurs. Du kan skapa en variabel som använder hello [uniqueString()](resource-group-template-functions-string.md#uniquestring) fungerar toogenerate ett namn. 

Du också vill tooadd ett prefix eller suffix toohello **uniqueString** resultat. Ändra hello unikt namn kan hjälpa dig att enkelt identifiera hello resurstyp från hello namn. Du kan till exempel generera ett unikt namn för ett lagringskonto med hjälp av följande variabel hello:

```json
"variables": {
    "storageAccountName": "[concat(uniqueString(resourceGroup().id),'storage')]"
}
```

### <a name="resource-names-for-identification"></a>Resursnamn för identifiering
Vissa typer av resurser du kanske vill tooname, men deras namn har inte unika toobe. Du kan ange ett namn som identifierar både hello resurstyp och hello resurs sammanhang för dessa typer av resurser. Ange ett beskrivande namn som hjälper dig att identifiera hello resurs i en lista över resurser. Om du behöver toouse ett annat resursnamn för olika distributioner kan använda du en parameter för hello namn:

```json
"parameters": {
    "vmName": { 
        "type": "string",
        "defaultValue": "demoLinuxVM",
        "metadata": {
            "description": "hello name of hello VM toocreate."
        }
    }
}
```

Om du inte behöver toopass i ett namn under distributionen kan du använda en variabel: 

```json
"variables": {
    "vmName": "demoLinuxVM"
}
```

Du kan också använda ett hårdkodat värde:

```json
{
  "type": "Microsoft.Compute/virtualMachines",
  "name": "demoLinuxVM",
  ...
}
```

### <a name="generic-resource-names"></a>Allmänt namn
Du kan använda ett allmänt namn som är hårdkodat i hello mallen för resurstyper som främst öppnas från en annan resurs. Du kan till exempel ange ett standard, allmän namn på brandväggsregler på en SQLServer:

```json
{
    "type": "firewallrules",
    "name": "AllowAllWindowsAzureIps",
    ...
}
```

## <a name="parameters"></a>Parametrar
hello följande information kan vara användbar när du arbetar med parametrar:

* Minimera din användning av parametrar. Använd om möjligt en variabel eller ett litteralvärde. Använd parametrarna endast för dessa scenarier:
   
   * Inställningar som du vill toouse variationer av enligt tooenvironment (SKU, storlek, kapacitet).
   * Resursnamn som du vill toospecify för enkel identifiering.
   * Värden som du använder ofta toocomplete andra aktiviteter (till exempel en administratörsanvändarnamnet).
   * Hemligheter (till exempel lösenord).
   * hello tal eller uppsättning värden toouse när du skapar flera instanser av en resurstyp.
* Slitbanor användningsfall för parameternamn.
* Ange en beskrivning av varje parameter i hello metadata:

   ```json
   "parameters": {
       "storageAccountType": {
           "type": "string",
           "metadata": {
               "description": "hello type of hello new storage account created toostore hello VM disks."
           }
       }
   }
   ```

* Ange standardvärden för parametrar (förutom för lösenord och SSH-nycklar):
   
   ```json
   "parameters": {
        "storageAccountType": {
            "type": "string",
            "defaultValue": "Standard_GRS",
            "metadata": {
                "description": "hello type of hello new storage account created toostore hello VM disks."
            }
        }
   }
   ```

* Använd **SecureString** för alla lösenord och hemligheter: 
   
   ```json
   "parameters": {
       "secretValue": {
           "type": "securestring",
           "metadata": {
               "description": "hello value of hello secret toostore in hello vault."
           }
       }
   }
   ```

* Använd inte en parameter toospecify plats när det är möjligt. Använd i stället hello **plats** egenskapen hello resursgruppen. Med hjälp av hello **resourceGroup () .location** uttryck för alla resurser som resurser i hello mallen har distribuerats i hello samma plats som hello resursgrupp:
   
   ```json
   "resources": [
     {
         "name": "[variables('storageAccountName')]",
         "type": "Microsoft.Storage/storageAccounts",
         "apiVersion": "2016-01-01",
         "location": "[resourceGroup().location]",
         ...
     }
   ]
   ```
   
   Om en resurstyp stöds i endast ett begränsat antal platser, kanske du vill toospecify en giltig plats direkt i hello mallen. Om du måste använda en **plats** parameter, dela att parametervärdet så mycket som möjligt med resurser som är sannolikt toobe i hello samma plats. Detta minimerar hello antalet gånger som användarna ombeds tooprovide platsinformation.
* Undvik att använda en parameter eller variabel för hello API-version för en resurstyp. Egenskaper för resursen och värdena kan variera efter versionsnummer. IntelliSense i en redigerare kan inte fastställa hello rätt schema när hello API-version anges tooa parametern eller variabeln. I stället hello hårdkoda API-versionen i hello mallen.

## <a name="variables"></a>Variabler
hello följande information kan vara användbar när du arbetar med variabler:

* Använda variabler för värden att du behöver toouse mer än en gång i en mall. Om ett värde används bara en gång, gör ett hårdkodat värde din mall enklare tooread.
* Du kan inte använda hello [referens](resource-group-template-functions-resource.md#reference) funktion i hello **variabler** avsnitt i hello mall. Hej **referens** funktionen hämtar sitt värde från hello resursens runtime-tillståndet. Dock matchas variabler under hello inledande tolkning av hello mallen. Skapa värden som behöver hello **referens** funktionen direkt i hello **resurser** eller **matar ut** avsnitt i hello mall.
* Variabler för resursnamn som måste vara unika, enligt beskrivningen i [resursnamn](#resource-names).
* Du kan gruppera variabler i komplexa objekt. Använd hello **variable.subentry** formatera tooreference ett värde från ett komplext objekt. Gruppera variabler kan hjälpa dig att spåra relaterade variabler. Det förbättrar också läsbarhet hello mallen. Här är ett exempel:
   
   ```json
   "variables": {
       "storage": {
           "name": "[concat(uniqueString(resourceGroup().id),'storage')]",
           "type": "Standard_LRS"
       }
   },
   "resources": [
     {
         "type": "Microsoft.Storage/storageAccounts",
         "name": "[variables('storage').name]",
         "apiVersion": "2016-01-01",
         "location": "[resourceGroup().location]",
         "sku": {
             "name": "[variables('storage').type]"
         },
         ...
     }
   ]
   ```
   
   > [!NOTE]
   > Ett komplext objekt får inte innehålla ett uttryck som refererar till ett värde från ett komplext objekt. Definiera en separat variabel för detta ändamål.
   > 
   > 
   
     Avancerade exempel på hur komplexa objekt som variabler finns i [dela tillstånd i Azure Resource Manager-mallar](best-practices-resource-manager-state.md).

## <a name="resources"></a>Resurser
hello följande information kan vara användbar när du arbetar med resurser:

* toohelp andra deltagare förstå hello syftet hello resurs, ange **kommentarer** för varje resurs i mallen för hello:
   
   ```json
   "resources": [
     {
         "name": "[variables('storageAccountName')]",
         "type": "Microsoft.Storage/storageAccounts",
         "apiVersion": "2016-01-01",
         "location": "[resourceGroup().location]",
         "comments": "This storage account is used toostore hello VM disks.",
         ...
     }
   ]
   ```

* Du kan använda taggar tooadd metadata tooresources. Använda metadata tooadd information om dina resurser. Du kan till exempel lägga till metadata toorecord faktureringsinformation för en resurs. Mer information finns i [med hjälp av taggar tooorganize resurserna i Azure](resource-group-using-tags.md).
* Om du använder en *offentlig slutpunkt* i mallen (till exempel Azure Blob storage offentlig slutpunkt), *gör inte hårdkoda* hello namnområde. Använd hello **referens** funktionen toodynamically hämta hello namnområde. Du kan använda den här metoden toodeploy hello mallen toodifferent allmänna namnutrymmet miljöer utan att manuellt ändra hello slutpunkt i hello mallen. Ange hello API-version toohello samma version som du använder för hello storage-konto i mallen:
   
   ```json
   "osDisk": {
       "name": "osdisk",
       "vhd": {
           "uri": "[concat(reference(concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName')), '2016-01-01').primaryEndpoints.blob, variables('vmStorageAccountContainerName'), '/',variables('OSDiskName'),'.vhd')]"
       }
   }
   ```
   
   Om hello storage-konto är distribuerat i hello samma mall som du skapar, behöver du inte toospecify hello leverantörens namnrymd när du refererar till hello resurs. Detta är hello förenklad syntax:
   
   ```json
   "osDisk": {
       "name": "osdisk",
       "vhd": {
           "uri": "[concat(reference(variables('storageAccountName'), '2016-01-01').primaryEndpoints.blob, variables('vmStorageAccountContainerName'), '/',variables('OSDiskName'),'.vhd')]"
       }
   }
   ```
   
   Om du har andra värden i mallen som är konfigurerade toouse offentliga namnområde kan ändra dessa värden tooreflect hello samma **referens** funktion. Du kan till exempel ange hello **storageUri** -egenskapen för hello diagnostikprofilen för virtuell dator:
   
   ```json
   "diagnosticsProfile": {
       "bootDiagnostics": {
           "enabled": "true",
           "storageUri": "[reference(concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName')), '2016-01-01').primaryEndpoints.blob]"
       }
   }
   ```
   
   Du kan också referera till ett befintligt lagringskonto i en annan resursgrupp:

   ```json
   "osDisk": {
       "name": "osdisk", 
       "vhd": {
           "uri":"[concat(reference(resourceId(parameters('existingResourceGroup'), 'Microsoft.Storage/storageAccounts/', parameters('existingStorageAccountName')), '2016-01-01').primaryEndpoints.blob,  variables('vmStorageAccountContainerName'), '/', variables('OSDiskName'),'.vhd')]"
       }
   }
   ```

* Tilldela offentliga IP-adresser tooa virtuella datorn endast när ett program kräver. inkommande NAT-regler, en virtuell nätverksgateway eller en jumpbox Använd tooconnect tooa virtuell dator (VM) för felsökning eller för att hantera administrativa syften.
   
     Mer information om hur du ansluter datorer toovirtual finns:
   
   * [Köra virtuella datorer för en arkitektur med flera nivåer i Azure](../guidance/guidance-compute-n-tier-vm.md)
   * [Konfigurera WinRM-åtkomst för virtuella datorer i Azure Resource Manager](../virtual-machines/windows/winrm.md)
   * [Tillåt extern åtkomst tooyour VM med hjälp av hello Azure-portalen](../virtual-machines/windows/nsg-quickstart-portal.md)
   * [Tillåt extern åtkomst tooyour VM med hjälp av PowerShell](../virtual-machines/windows/nsg-quickstart-powershell.md)
   * [Tillåt extern åtkomst tooyour Linux VM med hjälp av Azure CLI](../virtual-machines/virtual-machines-linux-nsg-quickstart.md)
* Hej **domainNameLabel** -egenskapen för offentliga IP-adresser måste vara unika. Hej **domainNameLabel** värdet måste vara mellan 3 och 63 tecken långt och följa hello regler som anges av den här reguljärt uttryck: `^[a-z][a-z0-9-]{1,61}[a-z0-9]$`. Eftersom hello **uniqueString** funktionen genererar en sträng som 13 tecken, hello **dnsPrefixString** parametern är begränsad too50 tecken:

   ```json
   "parameters": {
       "dnsPrefixString": {
           "type": "string",
           "maxLength": 50,
           "metadata": {
               "description": "hello DNS label for hello public IP address. It must be lowercase. It should match hello following regular expression, or it will raise an error: ^[a-z][a-z0-9-]{1,61}[a-z0-9]$"
           }
       }
   },
   "variables": {
       "dnsPrefix": "[concat(parameters('dnsPrefixString'),uniquestring(resourceGroup().id))]"
   }
   ```

* När du lägger till ett lösenord tooa-tillägget för anpassat skript använder hello **commandToExecute** egenskap i hello **protectedSettings** egenskapen:
   
   ```json
   "properties": {
       "publisher": "Microsoft.Azure.Extensions",
       "type": "CustomScript",
       "typeHandlerVersion": "2.0",
       "autoUpgradeMinorVersion": true,
       "settings": {
           "fileUris": [
               "[concat(variables('template').assets, '/lamp-app/install_lamp.sh')]"
           ]
       },
       "protectedSettings": {
           "commandToExecute": "[concat('sh install_lamp.sh ', parameters('mySqlPassword'))]"
       }
   }
   ```
   
   > [!NOTE]
   > tooensure hemligheter är krypterad när de överförs som parametrar tooVMs-tillägg, använda hello **protectedSettings** -egenskapen för hello relevanta tillägg.
   > 
   > 

## <a name="outputs"></a>utdata
Om du använder en mall toocreate offentliga IP-adresser kan innehålla en **matar ut** avsnitt som returnerar information om hello IP-adress och hello fullständigt kvalificerade domännamnet (FQDN). Du kan använda utdata värden tooeasily hämta information om offentlig IP-adresser och FQDN efter distributionen. När du refererar till hello resurs, använda hello API-version som du använde toocreate den: 

```json
"outputs": {
    "fqdn": {
        "value": "[reference(resourceId('Microsoft.Network/publicIPAddresses',parameters('publicIPAddressName')), '2016-07-01').dnsSettings.fqdn]",
        "type": "string"
    },
    "ipaddress": {
        "value": "[reference(resourceId('Microsoft.Network/publicIPAddresses',parameters('publicIPAddressName')), '2016-07-01').ipAddress]",
        "type": "string"
    }
}
```

## <a name="single-template-vs-nested-templates"></a>Samma mall kontra kapslade mallar
toodeploy din lösning du kan använda en mall eller en Huvudmall med flera kapslade mallar. Kapslade mallar är gemensamma för mer avancerade scenarier. Använda en kapslad mall får du hello följande fördelar:

* Du kan dela upp en lösning i riktade komponenter.
* Du kan återanvända kapslade mallar med olika huvudsakliga mallar.

Om du väljer toouse kapslade mallar hjälper hello följande riktlinjer dig att standardisera utformningen mallen. Dessa riktlinjer är baserade på [mönster för att utforma Azure Resource Manager-mallar](best-practices-resource-manager-design-templates.md). Vi rekommenderar en design som har hello följande mallar:

* **Huvudmall** (azuredeploy.json). Använd för hello indataparametrar.
* **Delade resurser mallen**. Använd toodeploy delade resurser med andra resurser (till exempel virtuella nätverk och tillgänglighet uppsättningar). Använd hello **dependsOn** uttryck tooensure att distribuera den här mallen innan andra mallar.
* **Valfria resurser mallen**. Använd tooconditionally distribuera resurser baserat på en parameter (till exempel jumpbox).
* **Medlemmen resurser mallen**. Varje instans i en program-nivå har sin egen konfiguration. Du kan definiera olika instans typer i en nivå. (Till exempel hello första instansen skapar ett kluster, och ytterligare instanser läggs toohello befintligt kluster.) Varje instans har sin egen Distributionsmall.
* **Skript**. Allmänt återanvändbara skript kan användas för varje instanstyp (till exempel initiera och formatera ytterligare diskar). Anpassade skript som du skapar för en viss anpassning syfte är olika, baserat på hello instanstyp.

![Kapslad mall](./media/resource-manager-template-best-practices/nestedTemplateDesign.png)

Mer information finns i [använda länkade mallar med Azure Resource Manager](resource-group-linked-templates.md).

## <a name="conditionally-link-toonested-templates"></a>Villkorligt länka toonested mallar
Du kan använda parametern tooconditionally länk toonested mallar. hello parametern blir en del av hello URI för hello mallen:

```json
"parameters": {
    "newOrExisting": {
        "type": "String",
        "allowedValues": [
            "new",
            "existing"
        ]
    }
},
"variables": {
    "templatelink": "[concat('https://raw.githubusercontent.com/Contoso/Templates/master/',parameters('newOrExisting'),'StorageAccount.json')]"
},
"resources": [
    {
        "apiVersion": "2015-01-01",
        "name": "nestedTemplate",
        "type": "Microsoft.Resources/deployments",
        "properties": {
            "mode": "incremental",
            "templateLink": {
                "uri": "[variables('templatelink')]",
                "contentVersion": "1.0.0.0"
            },
            "parameters": {
            }
        }
    }
]
```

## <a name="template-format"></a>Mallformat
Det är en bra idé toopass din mall med en JSON-verifieraren. Med hjälp av en systemhälsoverifierare kan du ta bort överflödig kommatecken, parenteser och hakparenteser som kan orsaka fel under distributionen. Försök [JSONLint](http://jsonlint.com/) eller ett linter paket för din favorit redigerar miljö (Visual Studio Code, Atom, Sublime Text, Visual Studio).

Det är också en bra idé tooformat din JSON för bättre läsbarhet. Du kan använda en JSON-Formateraren paketet för din lokala redigeraren. I Visual Studio tooformat hello dokumentet trycker du på **Ctrl + K, Ctrl + D**. I Visual Studio-koden trycker du på **Alt + Skift + F**. Om din lokala redigeraren inte formateras hello dokumentet, kan du använda en [online formaterare](https://www.bing.com/search?q=json+formatter).

## <a name="next-steps"></a>Nästa steg
* Anvisningar om att utforma din lösning för virtuella datorer, se [kör en virtuell Windows-dator i Azure](../guidance/guidance-compute-single-vm.md) och [kör ett Linux-VM i Azure](../guidance/guidance-compute-single-vm-linux.md).
* Anvisningar om hur du skapar ett lagringskonto finns [Azure Storage checklistan för prestanda och skalbarhet](../storage/common/storage-performance-checklist.md).
* toolearn om hur företaget kan använda Resource Manager tooeffectively hantera prenumerationer, se [Azure enterprise kodskelett: normativ prenumeration styrning](resource-manager-subscription-governance.md).

