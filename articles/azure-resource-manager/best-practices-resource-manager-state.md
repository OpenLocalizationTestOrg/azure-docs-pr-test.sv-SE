---
title: "aaaPass komplexa värden mellan Azure-mallar | Microsoft Docs"
description: "Visar rekommenderade metoder för att använda komplexa objekt tooshare tillståndsdata med Azure Resource Manager-mallar och länkade mallar."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: fd2f5e2d-d56d-4e01-a57d-34f3eaead4a9
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/26/2016
ms.author: tomfitz
ms.openlocfilehash: 72df1dee351446cea6ce15269e6db288b1f1db79
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="share-state-tooand-from-azure-resource-manager-templates"></a>Dela tillstånd tooand från Azure Resource Manager-mallar
Det här avsnittet innehåller metodtips för att hantera och dela tillstånd i mallar. hello parametrar och variabler som visas i det här avsnittet är exempel på hello typen av objekt som du kan definiera tooconveniently ordna dina distributionskrav. Du kan implementera objekten med egenskapsvärden som passar din miljö i dessa exempel.

Det här avsnittet är en del av en större vitbok. Hämta tooread hello fullständig papper [World klassen Resource Manager-mallar överväganden och beprövade metoder](http://download.microsoft.com/download/8/E/1/8E1DBEFA-CECE-4DC9-A813-93520A5D7CFE/World Class ARM Templates - Considerations and Proven Practices.pdf).

## <a name="provide-standard-configuration-settings"></a>Ange standard konfigurationsinställningar
I stället för att erbjuda en mall som ger totalt flexibilitet och oräkneliga variationer är ett vanligt mönster tooprovide ett urval av kända konfigurationer. Användare kan i praktiken välja standard t-shirt storlekar, till exempel sandbox små, medelstora och stora. Andra exempel på t-shirt storlekar är produkterbjudanden, till exempel community edition eller enterprise edition. I andra fall kan det vara belastningsspecifikt konfigurationer av en teknik – som kartan minska eller utan sql.

Du kan skapa variabler som innehåller datasamlingar, kallas ibland för ”egenskapsuppsättningar” och använda data toodrive hello resursdeklarationen i mallen med komplexa objekt. Den här metoden ger bra, kända konfigurationer i olika storlekar förkonfigurerade för kunder. Utan kända konfigurationer användare av hello mall måste fastställa klustret storlek på sina egna, faktor plattform resurs begränsningar och gör matematiska tooidentify hello resulterande partitionering av lagringskonton och andra resurser (på grund av toocluster storlek och resurs-begränsningar). Dessutom toomaking en bättre upplevelse för hello kund några kända konfigurationer är enklare toosupport och kan hjälpa dig att leverera en högre säkerhetsnivå för densitet.

följande exempel visar hur hello toodefine variabler som innehåller komplexa objekt som representerar samlingar av data. hello samlingar Definiera värden som används för storlek på virtuell dator, nätverksinställningar, operativsysteminställningar och tillgänglighetsinställningarna.

    "variables": {
      "tshirtSize": "[variables(concat('tshirtSize', parameters('tshirtSize')))]",
      "tshirtSizeSmall": {
        "vmSize": "Standard_A1",
        "diskSize": 1023,
        "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]",
        "vmCount": 2,
        "storage": {
          "name": "[parameters('storageAccountNamePrefix')]",
          "count": 1,
          "pool": "db",
          "map": [0,0],
          "jumpbox": 0
        }
      },
      "tshirtSizeMedium": {
        "vmSize": "Standard_A3",
        "diskSize": 1023,
        "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-8disk-resources.json')]",
        "vmCount": 2,
        "storage": {
          "name": "[parameters('storageAccountNamePrefix')]",
          "count": 2,
          "pool": "db",
          "map": [0,1],
          "jumpbox": 0
        }
      },
      "tshirtSizeLarge": {
        "vmSize": "Standard_A4",
        "diskSize": 1023,
        "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-16disk-resources.json')]",
        "vmCount": 3,
        "slaveCount": 2,
        "storage": {
          "name": "[parameters('storageAccountNamePrefix')]",
          "count": 2,
          "pool": "db",
          "map": [0,1,1],
          "jumpbox": 0
        }
      },
      "osSettings": {
        "scripts": [
          "[concat(variables('templateBaseUrl'), 'install_postgresql.sh')]",
          "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/shared_scripts/ubuntu/vm-disk-utils-0.1.sh"
        ],
        "imageReference": {
          "publisher": "Canonical",
          "offer": "UbuntuServer",
          "sku": "14.04.2-LTS",
          "version": "latest"
        }
      },
      "networkSettings": {
        "vnetName": "[parameters('virtualNetworkName')]",
        "addressPrefix": "10.0.0.0/16",
        "subnets": {
          "dmz": {
            "name": "dmz",
            "prefix": "10.0.0.0/24",
            "vnet": "[parameters('virtualNetworkName')]"
          },
          "data": {
            "name": "data",
            "prefix": "10.0.1.0/24",
            "vnet": "[parameters('virtualNetworkName')]"
          }
        }
      },
      "availabilitySetSettings": {
        "name": "pgsqlAvailabilitySet",
        "fdCount": 3,
        "udCount": 5
      }
    }

Observera att hello **tshirtSize** variabeln sammanfogar hello t-shirt storlek som du angav via en parameter (**små**, **medel**, **stor**) toohello text **tshirtSize**. Du använder den här variabeln tooretrieve hello associerade komplexa objektvariabeln för att t-shirt storlek.

Sedan kan du referera dessa variabler senare i hello mallen. hello möjlighet tooreference med namnet-variabler och deras egenskaper förenklar hello mallens syntax och gör det enkelt toounderstand kontext. hello följande exempel definierar en resurs toodeploy genom att använda hello-objekt som visas tidigare tooset värden. Till exempel hello VM-storlek anges genom att hämta hello värde för `variables('tshirtSize').vmSize` medan hello värdet för hello diskstorleken hämtas från `variables('tshirtSize').diskSize`. Dessutom hello URI för en länkad mall har angetts med hello värde för `variables('tshirtSize').vmTemplate`.

    "name": "master-node",
    "type": "Microsoft.Resources/deployments",
    "apiVersion": "2015-01-01",
    "dependsOn": [
        "[concat('Microsoft.Resources/deployments/', 'shared')]"
    ],
    "properties": {
        "mode": "Incremental",
        "templateLink": {
          "uri": "[variables('tshirtSize').vmTemplate]",
          "contentVersion": "1.0.0.0"
        },
        "parameters": {
          "adminPassword": {
            "value": "[parameters('adminPassword')]"
          },
          "replicatorPassword": {
            "value": "[parameters('replicatorPassword')]"
          },
          "osSettings": {
            "value": "[variables('osSettings')]"
          },
          "subnet": {
            "value": "[variables('networkSettings').subnets.data]"
          },
          "commonSettings": {
            "value": {
              "region": "[parameters('region')]",
              "adminUsername": "[parameters('adminUsername')]",
              "namespace": "ms"
            }
          },
          "storageSettings": {
            "value":"[variables('tshirtSize').storage]"
          },
          "machineSettings": {
            "value": {
              "vmSize": "[variables('tshirtSize').vmSize]",
              "diskSize": "[variables('tshirtSize').diskSize]",
              "vmCount": 1,
              "availabilitySet": "[variables('availabilitySetSettings').name]"
            }
          },
          "masterIpAddress": {
            "value": "0"
          },
          "dbType": {
            "value": "MASTER"
          }
        }
      }
    }

## <a name="pass-state-tooa-template"></a>Skicka tillstånd tooa mall
Du kan dela tillstånd till en mall med parametrar som du anger direkt under distributionen.

hello följande tabell listas vanliga parametrar i mallar.

| Namn | Värde | Beskrivning |
| --- | --- | --- |
| location |Sträng från en begränsad lista med Azure-regioner |hello plats där hello resurser har distribuerats. |
| storageAccountNamePrefix |Sträng |Unikt DNS-namn för hello Storage-konto där hello Virtuella diskar placeras |
| Domännamn |Sträng |Domännamnet för hello offentligt tillgänglig jumpbox VM hello format: **{domainName}. { Location}.cloudapp.com** till exempel: **mydomainname.westus.cloudapp.azure.com** |
| adminUsername |Sträng |Användarnamn för hello virtuella datorer |
| adminPassword |Sträng |Lösenordet för hello virtuella datorer |
| tshirtSize |Sträng från en begränsad lista över erbjuds t-shirt storlekar |hello namngivna skala enhet storlek tooprovision. Till exempel ”liten”, ”medel”, ”stor” |
| virtualNetworkName |Sträng |Namnet på hello virtuellt nätverk som hello konsumenten vill toouse. |
| enableJumpbox |Sträng från en begränsad lista (aktiverat/inaktiverat) |Parametern som identifierar om tooenable en jumpbox för hello-miljö. Värden: ”aktiverad”, ”inaktiverad” |

Hej **tshirtSize** parameter används i föregående avsnitt i hello definieras som:

    "parameters": {
      "tshirtSize": {
        "type": "string",
        "defaultValue": "Small",
        "allowedValues": [
          "Small",
          "Medium",
          "Large"
        ],
        "metadata": {
          "Description": "T-shirt size of hello MongoDB deployment"
        }
      }
    }


## <a name="pass-state-toolinked-templates"></a>Skicka tillstånd toolinked mallar
När du ansluter toolinked mallar kan du ofta använder en blandning av statiska och genereras variabler.

### <a name="static-variables"></a>Statiska variabler
Statiska variabler är ofta använda tooprovide grundläggande värden, till exempel URL: er som används i en mall.

I följande mall utdrag hello `templateBaseUrl` anger hello rotplats hello mallen i GitHub. hello nästa rad skapar en ny variabel `sharedTemplateUrl` som sammanfogar hello bas-URL med hello kända namnet på mallen för hello delade resurser. Under den raden är en variabel med komplexa objekt används toostore en t-shirt storlek, där hello bas-URL är sammanfogad toohello fungerande konfiguration mallplats och lagras i hello `vmTemplate` egenskapen.

hello fördelen med den här metoden är att om platsen för hello mall ändras, behöver du bara toochange hello statisk variabel på en plats som passerar i hela hello länkade mallar.

    "variables": {
      "templateBaseUrl": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/postgresql-on-ubuntu/",
      "sharedTemplateUrl": "[concat(variables('templateBaseUrl'), 'shared-resources.json')]",
      "tshirtSizeSmall": {
        "vmSize": "Standard_A1",
        "diskSize": 1023,
        "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]",
        "vmCount": 2,
        "slaveCount": 1,
        "storage": {
          "name": "[parameters('storageAccountNamePrefix')]",
          "count": 1,
          "pool": "db",
          "map": [0,0],
          "jumpbox": 0
        }
      }
    }

### <a name="generated-variables"></a>Genererade variabler
I tillägg toostatic variabler genereras flera variabler dynamiskt. Det här avsnittet beskrivs några av hello vanliga typer av genererade variabler.

#### <a name="tshirtsize"></a>tshirtSize
Du är bekant med den här variabeln som genereras från hello exemplen ovan.

#### <a name="networksettings"></a>networkSettings
I en kapacitet, kapaciteten eller lösningsmall för slutpunkt till slutpunkt begränsade hello Skapa länkade mallar normalt resurser som finns i ett nätverk. Ett enkelt sätt är toouse komplexa objektet toostore nätverksinställningar och skicka dem toolinked mallar.

Du kan se ett exempel på kommunikation nätverksinställningar nedan.

    "networkSettings": {
      "vnetName": "[parameters('virtualNetworkName')]",
      "addressPrefix": "10.0.0.0/16",
      "subnets": {
        "dmz": {
          "name": "dmz",
          "prefix": "10.0.0.0/24",
          "vnet": "[parameters('virtualNetworkName')]"
        },
        "data": {
          "name": "data",
          "prefix": "10.0.1.0/24",
          "vnet": "[parameters('virtualNetworkName')]"
        }
      }
    }

#### <a name="availabilitysettings"></a>availabilitySettings
Resurser som skapades i länkade mallar placeras ofta i en tillgänglighetsuppsättning. I följande exempel hello, hello tillgänglighetsuppsättningen anges också hello feldomän och uppdatera domän antal toouse.

    "availabilitySetSettings": {
      "name": "pgsqlAvailabilitySet",
      "fdCount": 3,
      "udCount": 5
    }

Om du behöver flera tillgänglighetsuppsättningar (till exempel en för överordnade noder) och en annan för datanoder, kan du använda ett namn som ett prefix kan ange flera tillgänglighetsuppsättningar eller följa hello-modell som visades tidigare för att skapa en variabel för en viss t-shirt storlek.

#### <a name="storagesettings"></a>storageSettings
Lagringsinformation om som ofta delas med länkade mallar. I hello exemplet nedan är ett *storageSettings* objekt innehåller information om hello storage-konto och en behållare namn.

    "storageSettings": {
        "vhdStorageAccountName": "[parameters('storageAccountName')]",
        "vhdContainerName": "[variables('vmStorageAccountContainerName')]",
        "destinationVhdsContainer": "[concat('https://', parameters('storageAccountName'), variables('vmStorageAccountDomain'), '/', variables('vmStorageAccountContainerName'), '/')]"
    }

#### <a name="ossettings"></a>osSettings
Länkade mallar kan behöva du toopass inställningar toovarious noder av typen operativsystem över olika för kända konfigurationstyper. Ett komplext objekt är ett enkelt sätt toostore och dela information om operativsystem och gör det också lättare toosupport flera operativsystem alternativ för distributionen.

hello följande exempel visas ett objekt för *osSettings*:

    "osSettings": {
      "imageReference": {
        "publisher": "Canonical",
        "offer": "UbuntuServer",
        "sku": "14.04.2-LTS",
        "version": "latest"
      }
    }

#### <a name="machinesettings"></a>machineSettings
En genererad variabel *machineSettings* är ett komplext objekt som innehåller en blandning av kärnor variabler för att skapa en virtuell dator. hello variabler är administratörsanvändarnamn och lösenord, ett prefix för hello VM-namn och en referens för avbildningen av operativsystemet.

    "machineSettings": {
        "adminUsername": "[parameters('adminUsername')]",
        "adminPassword": "[parameters('adminPassword')]",
        "machineNamePrefix": "mongodb-",
        "osImageReference": {
            "publisher": "[variables('osFamilySpec').imagePublisher]",
            "offer": "[variables('osFamilySpec').imageOffer]",
            "sku": "[variables('osFamilySpec').imageSKU]",
            "version": "latest"
        }
    },

Observera att *osImageReference* hämtar hello värden från hello *osSettings* variabel som definierats i hello Huvudmall. Det innebär att du kan enkelt ändra hello operativsystem för en virtuell dator, helt eller baserat på hello inställningar mallen konsumenter.

#### <a name="vmscripts"></a>vmScripts
Hej *vmScripts* objekt innehåller information om hello skript toodownload och köra på en VM-instans, inklusive utanför och inom referenser. Utanför innehåller referenser hello infrastruktur.
I referenser omfattar hello installerad programvara som är installerad och konfiguration.

Du använder hello *scriptsToDownload* egenskapen toolist hello skript toodownload toohello VM. Det här objektet innehåller också referenser toocommand-argument för olika typer av åtgärder. Dessa åtgärder omfattar köra hello standardinstallationen för varje enskild nod, en installation som körs när alla noder har distribuerats och ytterligare skript som kan vara specifika tooa som anges i mallen.

Det här exemplet är från en mall som används toodeploy MongoDB, som kräver en avgörandet toodeliver hög tillgänglighet. Hej *arbiterNodeInstallCommand* har lagts till för*vmScripts* tooinstall hello avgörandet.

hello variabler avsnittet är hittar du hello variabler som definierar hello viss text tooexecute hello skriptet med hello rätt värden.

    "vmScripts": {
        "scriptsToDownload": [
            "[concat(variables('scriptUrl'), 'mongodb-', variables('osFamilySpec').osName, '-install.sh')]",
            "[concat(variables('sharedScriptUrl'), 'vm-disk-utils-0.1.sh')]"
        ],
        "regularNodeInstallCommand": "[variables('installCommand')]",
        "lastNodeInstallCommand": "[concat(variables('installCommand'), ' -l')]",
        "arbiterNodeInstallCommand": "[concat(variables('installCommand'), ' -a')]"
    },


## <a name="return-state-from-a-template"></a>Returnera tillstånd från en mall
Inte bara kan du skicka data till en mall, du kan också dela data tillbaka toohello anropa mall. I hello **matar ut** avsnitt av en länkad mall kan du ange nyckel/värde-par som kan användas av hello källmallen.

hello följande exempel visas hur toopass hello privat IP-adress som genereras i en länkad mall.

    "outputs": {
        "masterip": {
            "value": "[reference(concat(variables('nicName'),0)).ipConfigurations[0].properties.privateIPAddress]",
            "type": "string"
         }
    }

Inom hello Huvudmall, kan du använda dessa data med hello följande syntax:

    "[reference('master-node').outputs.masterip.value]"

Du kan använda det här uttrycket i hello utdata avsnittet eller hello resurser avsnitt i hello huvudsakliga mall. Du kan inte använda hello uttryck i hello variabler avsnittet eftersom det är beroende av hello runtime-tillståndet. tooreturn detta värde från hello Huvudmall, Använd:

    "outputs": {
      "masterIpAddress": {
        "value": "[reference('master-node').outputs.masterip.value]",
        "type": "string"
      }

Ett exempel på hur hello matar ut avsnitt i en länkad mall tooreturn datadiskar för en virtuell dator, finns [skapar flera datadiskar för en virtuell dator](resource-group-create-multiple.md).

## <a name="define-authentication-settings-for-virtual-machine"></a>Definiera autentiseringsinställningar för den virtuella datorn
Du kan använda hello samma mönster tidigare inställningar toospecify hello autentisering konfigurationsinställningar för en virtuell dator. Du skapar en parameter för överföring i hello typ av autentisering.

    "parameters": {
      "authenticationType": {
        "allowedValues": [
          "password",
          "sshPublicKey"
        ],
        "defaultValue": "password",
        "metadata": {
          "description": "Authentication type"
        },
        "type": "string"
      }
    }

Du lägger till variabler för hello olika typer av autentisering och en variabel toostore vilken typ som används för den här distributionen baserat på hello hello parameterns värde.

    "variables": {
      "osProfile": "[variables(concat('osProfile', parameters('authenticationType')))]",
      "osProfilepassword": {
        "adminPassword": "[parameters('adminPassword')]",
        "adminUsername": "notused",
        "computerName": "[parameters('vmName')]",
        "customData": "[base64(variables('customData'))]"
      },
      "osProfilesshPublicKey": {
        "adminUsername": "notused",
        "computerName": "[parameters('vmName')]",
        "customData": "[base64(variables('customData'))]",
        "linuxConfiguration": {
          "disablePasswordAuthentication": "true",
          "ssh": {
            "publicKeys": [
              {
                "keyData": "[parameters('sshPublicKey')]",
                "path": "/home/notused/.ssh/authorized_keys"
              }
            ]
          }
        }
      }
    }

När definiera hello virtuell dator kan du ställa in hello **osProfile** toohello variabeln som du skapat.

    {
      "type": "Microsoft.Compute/virtualMachines",
      ...
      "osProfile": "[variables('osProfile')]"
    }


## <a name="next-steps"></a>Nästa steg
* toolearn om hello avsnitt i hello-mallen finns [redigera Azure Resource Manager-mallar](resource-group-authoring-templates.md)
* toosee hello funktioner som är tillgängliga i en mall finns [Azure Resource Manager Mallfunktioner](resource-group-template-functions.md)
