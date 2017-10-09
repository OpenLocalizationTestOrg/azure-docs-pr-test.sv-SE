---
title: "aaaDesired tillstånd Configuration Resource Manager-mall | Microsoft Docs"
description: "Definition av Resource Manager-mall för önskad Tillståndskonfiguration i Azure med exempel och felsökning"
services: virtual-machines-windows
documentationcenter: 
author: zjalexander
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
keywords: 
ms.assetid: b5402e5a-1768-4075-8c19-b7f7402687af
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: na
ms.date: 09/15/2016
ms.author: zachal
ms.openlocfilehash: fc47ac149b1179d9305797eaa8ed8a83c0958c37
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="windows-vmss-and-desired-state-configuration-with-azure-resource-manager-templates"></a>Windows VMSS och Desired State Configuration med Azure Resource Manager-mallar
Den här artikeln beskriver hello Resource Manager-mall för hello [Desired State Configuration-tillägget hanteraren](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

## <a name="template-example-for-a-windows-vm"></a>Exempel för en virtuell Windows-dator
hello försätts följande kodavsnitt i hello resursavsnitt i hello mall.

```json
            "name": "Microsoft.Powershell.DSC",
            "type": "extensions",
             "location": "[resourceGroup().location]",
             "apiVersion": "2015-06-15",
             "dependsOn": [
                  "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
              ],
              "properties": {
                  "publisher": "Microsoft.Powershell",
                  "type": "DSC",
                  "typeHandlerVersion": "2.20",
                  "autoUpgradeMinorVersion": true,
                  "forceUpdateTag": "[parameters('dscExtensionUpdateTagVersion')]",
                  "settings": {
                      "configuration": {
                          "url": "[concat(parameters('_artifactsLocation'), '/', variables('dscExtensionArchiveFolder'), '/', variables('dscExtensionArchiveFileName'))]",
                          "script": "dscExtension.ps1",
                          "function": "Main"
                      },
                      "configurationArguments": {
                          "nodeName": "[variables('vmName')]"
                      }
                  },
                  "protectedSettings": {
                      "configurationUrlSasToken": "[parameters('_artifactsLocationSasToken')]"
                  }
              }

```

## <a name="template-example-for-windows-vmss"></a>Exempel för Windows VMSS
En VMSS-nod har ett ”egenskaper” avsnitt med hello ”VirtualMachineProfile”, ”extensionProfile”-attribut. DSC läggs till under ”tillägg”. 

```json
"extensionProfile": {
            "extensions": [
                {
                    "name": "Microsoft.Powershell.DSC",
                    "properties": {
                        "publisher": "Microsoft.Powershell",
                        "type": "DSC",
                        "typeHandlerVersion": "2.20",
                        "autoUpgradeMinorVersion": true,
                        "forceUpdateTag": "[parameters('DscExtensionUpdateTagVersion')]",
                        "settings": {
                            "configuration": {
                                "url": "[concat(parameters('_artifactsLocation'), '/', variables('DscExtensionArchiveFolder'), '/', variables('DscExtensionArchiveFileName'))]",
                                "script": "DscExtension.ps1",
                                "function": "Main"
                            },
                            "configurationArguments": {
                                "nodeName": "localhost"
                            }
                        },
                        "protectedSettings": {
                            "configurationUrlSasToken": "[parameters('_artifactsLocationSasToken')]"
                        }
                    }
                }
            ]
        }
```

## <a name="detailed-settings-information"></a>Information om detaljerade inställningar
hello är följande schema hello inställningar del av hello Azure DSC-tillägg i en Azure Resource Manager-mall.

```json

"settings": {
  "wmfVersion": "latest",
  "configuration": {
    "url": "http://validURLToConfigLocation",
    "script": "ConfigurationScript.ps1",
    "function": "ConfigurationFunction"
  },
  "configurationArguments": {
    "argument1": "Value1",
    "argument2": "Value2"
  },
  "configurationData": {
    "url": "https://foo.psd1"
  },
  "privacy": {
    "dataCollection": "enable"
  },
  "advancedOptions": {
    "downloadMappings": {
      "customWmfLocation": "http://myWMFlocation"
    }
  } 
},
"protectedSettings": {
  "configurationArguments": {
    "parameterOfTypePSCredential1": {
      "userName": "UsernameValue1",
      "password": "PasswordValue1"
    },
    "parameterOfTypePSCredential2": {
      "userName": "UsernameValue2",
      "password": "PasswordValue2"
    }
  },
  "configurationUrlSasToken": "?g!bber1sht0k3n",
  "configurationDataUrlSasToken": "?dataAcC355T0k3N"
}

```

## <a name="details"></a>Information
| Egenskapsnamn | Typ | Beskrivning |
| --- | --- | --- |
| settings.wmfVersion |Sträng |Anger hello version av hello Windows Management Framework som ska installeras på den virtuella datorn. Ange den här egenskapen too'latest' installerar hello senaste uppdaterade versionen av WMF. hello endast aktuella möjliga värden för den här egenskapen är **'4.0', '5.0', ' 5.0PP' och 'senaste'**. Dessa möjliga värden är ämne tooupdates. hello standardvärdet är ”senaste”. |
| Settings.Configuration.URL |Sträng |Anger hello URL-plats från vilken toodownload DSC-konfiguration zip-filen. Om hello URL: en kräver en SAS-token för åtkomst, måste tooset hello protectedSettings.configurationUrlSasToken toohello egenskapsvärde SAS-token. Den här egenskapen är obligatorisk om settings.configuration.script och/eller settings.configuration.function definieras. |
| Settings.Configuration.Script |Sträng |Anger namnet på hello på hello-skript som innehåller hello definition av DSC-konfigurationen. Det här skriptet måste vara i hello zip-filen som hämtats från hello-URL som anges av egenskapen för hello configuration.url hello rotmapp. Den här egenskapen är obligatorisk om settings.configuration.url och/eller settings.configuration.script definieras. |
| Settings.Configuration.Function |Sträng |Anger hello för DSC-konfigurationen. hello-konfiguration med namnet måste finnas i hello-skript som definierats av configuration.script. Den här egenskapen är obligatorisk om settings.configuration.url och/eller settings.configuration.function definieras. |
| settings.configurationArguments |Samling |Definierar de parametrar som du vill att toopass tooyour DSC-konfigurationen. Den här egenskapen är inte krypterad. |
| settings.configurationData.url |Sträng |Anger hello URL från vilken toodownload filen konfigurationsdata (.psd1) toouse som indata för DSC-konfigurationen. Om hello URL: en kräver en SAS-token för åtkomst, måste tooset hello protectedSettings.configurationDataUrlSasToken toohello egenskapsvärde SAS-token. |
| settings.privacy.dataEnabled |Sträng |Aktiverar eller inaktiverar telemetri samling. hello endast möjliga värden för den här egenskapen är **'Aktivera', inaktivera, '', eller $null**. Om du lämnar den här egenskapen är tomt eller null kan telemetri. hello standardvärdet är ''. [Mer Information](https://blogs.msdn.microsoft.com/powershell/2016/02/02/azure-dsc-extension-data-collection-2/) |
| settings.advancedOptions.downloadMappings |Samling |Definierar alternativa platser från vilka toodownload hello WMF. [Mer Information](http://blogs.msdn.com/b/powershell/archive/2015/10/21/azure-dsc-extension-2-2-amp-how-to-map-downloads-of-the-extension-dependencies-to-your-own-location.aspx) |
| protectedSettings.configurationArguments |Samling |Definierar de parametrar som du vill att toopass tooyour DSC-konfigurationen. Den här egenskapen är krypterad. |
| protectedSettings.configurationUrlSasToken |Sträng |Anger hello SAS-token tooaccess hello URL som definierats av configuration.url. Den här egenskapen är krypterad. |
| protectedSettings.configurationDataUrlSasToken |Sträng |Anger hello SAS-token tooaccess hello URL som definierats av configurationData.url. Den här egenskapen är krypterad. |

## <a name="settings-vs-protectedsettings"></a>Inställningar för vs. ProtectedSettings
Alla inställningarna sparas i en textfil i inställningar på hello VM.
Egenskaper under ”inställningar” är offentliga egenskaper eftersom de inte är krypterade i hello text inställningsfil.
Egenskaperna under 'protectedSettings' krypteras med ett certifikat och visas inte i oformaterad text i den här filen på hello VM.

Om hello konfiguration måste autentiseringsuppgifter, kan de ingå i protectedSettings:

```json
"protectedSettings": {
    "configurationArguments": {
        "parameterOfTypePSCredential1": {
               "userName": "UsernameValue1",
               "password": "PasswordValue1"
        }
    }
}
```

## <a name="example"></a>Exempel
hello följande exempel härleds från hello ”Kom-igång-avsnittet i hello [översiktssidan för DSC-tillägg hanteraren](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
Det här exemplet använder Resource Manager-mallar i stället för cmdlets toodeploy hello tillägg. Spara hello ”IisInstall.ps1” konfiguration, placera den i en. ZIP-fil och överför hello-fil i en URL som kan nås. Det här exemplet använder Azure blob storage, men det är möjligt toodownload. ZIP-filer från en valfri plats.

I mallen för hello Azure Resource Manager hello följande kod instruerar hello VM toodownload hello rätt fil och kör hello önskad funktion i PowerShell:

```json
"settings": {
    "configuration": {
        "url": "https://demo.blob.core.windows.net/",
        "script": "IisInstall.ps1",
        "function": "IISInstall"
    }
    } 
},
"protectedSettings": {
    "configurationUrlSasToken": "odLPL/U1p9lvcnp..."
}
```

## <a name="updating-from-hello-previous-format"></a>Uppdatera från hello tidigare Format
Alla inställningar i hello tidigare format (som innehåller hello offentliga egenskaper ModulesUrl ConfigurationFunction, SasToken eller egenskaper) automatiskt anpassa toohello nuvarande format och kör precis som de gjorde före.

hello följer schemat är vilka hello tidigare inställningar schema kan se ut:

```json
"settings": {
    "WMFVersion": "latest",
    "ModulesUrl": "https://UrlToZipContainingConfigurationScript.ps1.zip",
    "SasToken": "SAS Token if ModulesUrl points tooprivate Azure Blob Storage",
    "ConfigurationFunction": "ConfigurationScript.ps1\\ConfigurationFunction",
    "Properties":  {
        "ParameterToConfigurationFunction1":  "Value1",
        "ParameterToConfigurationFunction2":  "Value2",
        "ParameterOfTypePSCredential1": {
            "UserName": "UsernameValue1",
            "Password": "PrivateSettingsRef:Key1" 
        },
        "ParameterOfTypePSCredential2": {
            "UserName": "UsernameValue2",
            "Password": "PrivateSettingsRef:Key2"
        }
    }
},
"protectedSettings": { 
    "Items": {
        "Key1": "PasswordValue1",
        "Key2": "PasswordValue2"
    },
    "DataBlobUri": "https://UrlToConfigurationDataWithOptionalSasToken.psd1"
}
```

Här är hur hello tidigare formatet anpassas efter dina behov toohello nuvarande format:

| Egenskapsnamn | Tidigare schemat motsvarande |
| --- | --- |
| settings.wmfVersion |inställningar. WMFVersion |
| Settings.Configuration.URL |inställningar. ModulesUrl |
| Settings.Configuration.Script |Första delen av inställningar. ConfigurationFunction (före '\\\\”) |
| Settings.Configuration.Function |Andra delen av inställningar. ConfigurationFunction (när '\\\\”) |
| settings.configurationArguments |inställningar. Egenskaper |
| settings.configurationData.url |protectedSettings.DataBlobUri (utan SAS-token) |
| settings.privacy.dataEnabled |inställningar. Privacy.DataEnabled |
| settings.advancedOptions.downloadMappings |inställningar. AdvancedOptions.DownloadMappings |
| protectedSettings.configurationArguments |protectedSettings.Properties |
| protectedSettings.configurationUrlSasToken |inställningar. SasToken |
| protectedSettings.configurationDataUrlSasToken |SAS-token från protectedSettings.DataBlobUri |

## <a name="troubleshooting---error-code-1100"></a>Felsökning – felkoden 1100
Felkoden 1100 anger att det finns ett problem med hello användarindata toohello DSC-tillägg.
hello texten för dessa fel är variabel och kan ändras.
Här är några av hello fel kan du stöta på och hur du kan åtgärda dem.

### <a name="invalid-values"></a>Ogiltiga värden
”Privacy.dataCollection är: {0}. hello endast möjliga värden är ”,” aktivera ”och” inaktivera ”” ”WmfVersion är: {0}. Endast möjliga värden är... och ”senaste” ”

Problem: Det är inte tillåtet för ett angivet värde.

Lösning: Ändra hello ogiltigt värde tooa giltigt värde. Se tabellen hello i hello informationsavsnittet.

### <a name="invalid-url"></a>Ogiltig URL
”ConfigurationData.url är: {0}. Detta är inte en giltig URL ”” DataBlobUri är: {0}. Detta är inte en giltig URL ”” Configuration.url är: {0}. Detta är inte en giltig URL ”

Problem: A angivna Webbadressen inte är giltig.

Lösning: Kontrollera att alla dina angivna URL: er. Kontrollera att alla URL: er lösa toovalid platser som hello tillägget kan komma åt på hello fjärrdatorn.

### <a name="invalid-configurationargument-type"></a>Ogiltig ConfigurationArgument-typ.
”Ogiltigt configurationArguments typ {0}”

Problem: hello ConfigurationArguments egenskapen kan inte lösa tooa Hashtable-objektet. 

Lösning: Kontrollera ConfigurationArguments-egenskap en hash-tabell. Följ hello-format som tillhandahålls i hello föregående exempel. Se upp för citattecken, kommatecken och klammerparenteser.

### <a name="duplicate-configurationarguments"></a>Duplicera ConfigurationArguments
”Finns duplicerade argument: {0} i både offentliga och skyddade configurationArguments”

Problem: hello ConfigurationArguments i inställningar för offentliga och hello ConfigurationArguments i skyddade inställningarna innehåller egenskaper med hello samma namn.

Lösning: Ta bort en av hello dubblettegenskaper.

### <a name="missing-properties"></a>Egenskaper som saknas
”Configuration.function kräver att configuration.url eller configuration.module har angetts”

”Configuration.url kräver att configuration.script anges”

”Configuration.script kräver att configuration.url anges”

”Configuration.url kräver att configuration.function anges”

”ConfigurationUrlSasToken kräver att configuration.url anges”

”ConfigurationDataUrlSasToken kräver att configurationData.url anges”

Problem: En definierad egenskap måste en annan egenskap som saknas.

Lösningar: 

* Ange hello egenskap som saknas.
* Ta bort hello-egenskap som behöver hello egenskap som saknas.

## <a name="next-steps"></a>Nästa steg
Läs mer om DSC och virtuella datorn anger i [med hjälp av virtuella Skalningsuppsättningarna med hello Azure DSC-tillägg](../../virtual-machine-scale-sets/virtual-machine-scale-sets-dsc.md)

Mer information finns på [hantering av DSC-säkra autentiseringsuppgifter](extensions-dsc-credentials.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

Mer information om hello Azure DSC-tillägg-hanteraren finns [introduktion toohello Azure Desired State Configuration-tillägget hanteraren](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

Mer information om PowerShell DSC [finns hello PowerShell Dokumentationscenter](https://msdn.microsoft.com/powershell/dsc/overview). 

