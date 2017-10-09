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
# <a name="windows-vmss-and-desired-state-configuration-with-azure-resource-manager-templates"></a><span data-ttu-id="76839-103">Windows VMSS och Desired State Configuration med Azure Resource Manager-mallar</span><span class="sxs-lookup"><span data-stu-id="76839-103">Windows VMSS and Desired State Configuration with Azure Resource Manager templates</span></span>
<span data-ttu-id="76839-104">Den här artikeln beskriver hello Resource Manager-mall för hello [Desired State Configuration-tillägget hanteraren](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="76839-104">This article describes hello Resource Manager template for hello [Desired State Configuration extension handler](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> 

## <a name="template-example-for-a-windows-vm"></a><span data-ttu-id="76839-105">Exempel för en virtuell Windows-dator</span><span class="sxs-lookup"><span data-stu-id="76839-105">Template example for a Windows VM</span></span>
<span data-ttu-id="76839-106">hello försätts följande kodavsnitt i hello resursavsnitt i hello mall.</span><span class="sxs-lookup"><span data-stu-id="76839-106">hello following snippet goes into hello Resource section of hello template.</span></span>

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

## <a name="template-example-for-windows-vmss"></a><span data-ttu-id="76839-107">Exempel för Windows VMSS</span><span class="sxs-lookup"><span data-stu-id="76839-107">Template example for Windows VMSS</span></span>
<span data-ttu-id="76839-108">En VMSS-nod har ett ”egenskaper” avsnitt med hello ”VirtualMachineProfile”, ”extensionProfile”-attribut.</span><span class="sxs-lookup"><span data-stu-id="76839-108">A VMSS node has a "properties" section with hello "VirtualMachineProfile", "extensionProfile" attribute.</span></span> <span data-ttu-id="76839-109">DSC läggs till under ”tillägg”.</span><span class="sxs-lookup"><span data-stu-id="76839-109">DSC is added under "extensions".</span></span> 

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

## <a name="detailed-settings-information"></a><span data-ttu-id="76839-110">Information om detaljerade inställningar</span><span class="sxs-lookup"><span data-stu-id="76839-110">Detailed Settings Information</span></span>
<span data-ttu-id="76839-111">hello är följande schema hello inställningar del av hello Azure DSC-tillägg i en Azure Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="76839-111">hello following schema is for hello settings portion of hello Azure DSC extension in an Azure Resource Manager template.</span></span>

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

## <a name="details"></a><span data-ttu-id="76839-112">Information</span><span class="sxs-lookup"><span data-stu-id="76839-112">Details</span></span>
| <span data-ttu-id="76839-113">Egenskapsnamn</span><span class="sxs-lookup"><span data-stu-id="76839-113">Property Name</span></span> | <span data-ttu-id="76839-114">Typ</span><span class="sxs-lookup"><span data-stu-id="76839-114">Type</span></span> | <span data-ttu-id="76839-115">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="76839-115">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="76839-116">settings.wmfVersion</span><span class="sxs-lookup"><span data-stu-id="76839-116">settings.wmfVersion</span></span> |<span data-ttu-id="76839-117">Sträng</span><span class="sxs-lookup"><span data-stu-id="76839-117">string</span></span> |<span data-ttu-id="76839-118">Anger hello version av hello Windows Management Framework som ska installeras på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="76839-118">Specifies hello version of hello Windows Management Framework that should be installed on your VM.</span></span> <span data-ttu-id="76839-119">Ange den här egenskapen too'latest' installerar hello senaste uppdaterade versionen av WMF.</span><span class="sxs-lookup"><span data-stu-id="76839-119">Setting this property too'latest' installs hello most updated version of WMF.</span></span> <span data-ttu-id="76839-120">hello endast aktuella möjliga värden för den här egenskapen är **'4.0', '5.0', ' 5.0PP' och 'senaste'**.</span><span class="sxs-lookup"><span data-stu-id="76839-120">hello only current possible values for this property are **'4.0', '5.0', '5.0PP', and 'latest'**.</span></span> <span data-ttu-id="76839-121">Dessa möjliga värden är ämne tooupdates.</span><span class="sxs-lookup"><span data-stu-id="76839-121">These possible values are subject tooupdates.</span></span> <span data-ttu-id="76839-122">hello standardvärdet är ”senaste”.</span><span class="sxs-lookup"><span data-stu-id="76839-122">hello default value is 'latest'.</span></span> |
| <span data-ttu-id="76839-123">Settings.Configuration.URL</span><span class="sxs-lookup"><span data-stu-id="76839-123">settings.configuration.url</span></span> |<span data-ttu-id="76839-124">Sträng</span><span class="sxs-lookup"><span data-stu-id="76839-124">string</span></span> |<span data-ttu-id="76839-125">Anger hello URL-plats från vilken toodownload DSC-konfiguration zip-filen.</span><span class="sxs-lookup"><span data-stu-id="76839-125">Specifies hello URL location from which toodownload your DSC configuration zip file.</span></span> <span data-ttu-id="76839-126">Om hello URL: en kräver en SAS-token för åtkomst, måste tooset hello protectedSettings.configurationUrlSasToken toohello egenskapsvärde SAS-token.</span><span class="sxs-lookup"><span data-stu-id="76839-126">If hello URL provided requires a SAS token for access, you need tooset hello protectedSettings.configurationUrlSasToken property toohello value of your SAS token.</span></span> <span data-ttu-id="76839-127">Den här egenskapen är obligatorisk om settings.configuration.script och/eller settings.configuration.function definieras.</span><span class="sxs-lookup"><span data-stu-id="76839-127">This property is required if settings.configuration.script and/or settings.configuration.function are defined.</span></span> |
| <span data-ttu-id="76839-128">Settings.Configuration.Script</span><span class="sxs-lookup"><span data-stu-id="76839-128">settings.configuration.script</span></span> |<span data-ttu-id="76839-129">Sträng</span><span class="sxs-lookup"><span data-stu-id="76839-129">string</span></span> |<span data-ttu-id="76839-130">Anger namnet på hello på hello-skript som innehåller hello definition av DSC-konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="76839-130">Specifies hello file name of hello script that contains hello definition of your DSC configuration.</span></span> <span data-ttu-id="76839-131">Det här skriptet måste vara i hello zip-filen som hämtats från hello-URL som anges av egenskapen för hello configuration.url hello rotmapp.</span><span class="sxs-lookup"><span data-stu-id="76839-131">This script must be in hello root folder of hello zip file downloaded from hello URL specified by hello configuration.url property.</span></span> <span data-ttu-id="76839-132">Den här egenskapen är obligatorisk om settings.configuration.url och/eller settings.configuration.script definieras.</span><span class="sxs-lookup"><span data-stu-id="76839-132">This property is required if settings.configuration.url and/or settings.configuration.script are defined.</span></span> |
| <span data-ttu-id="76839-133">Settings.Configuration.Function</span><span class="sxs-lookup"><span data-stu-id="76839-133">settings.configuration.function</span></span> |<span data-ttu-id="76839-134">Sträng</span><span class="sxs-lookup"><span data-stu-id="76839-134">string</span></span> |<span data-ttu-id="76839-135">Anger hello för DSC-konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="76839-135">Specifies hello name of your DSC configuration.</span></span> <span data-ttu-id="76839-136">hello-konfiguration med namnet måste finnas i hello-skript som definierats av configuration.script.</span><span class="sxs-lookup"><span data-stu-id="76839-136">hello configuration named must be contained in hello script defined by configuration.script.</span></span> <span data-ttu-id="76839-137">Den här egenskapen är obligatorisk om settings.configuration.url och/eller settings.configuration.function definieras.</span><span class="sxs-lookup"><span data-stu-id="76839-137">This property is required if settings.configuration.url and/or settings.configuration.function are defined.</span></span> |
| <span data-ttu-id="76839-138">settings.configurationArguments</span><span class="sxs-lookup"><span data-stu-id="76839-138">settings.configurationArguments</span></span> |<span data-ttu-id="76839-139">Samling</span><span class="sxs-lookup"><span data-stu-id="76839-139">Collection</span></span> |<span data-ttu-id="76839-140">Definierar de parametrar som du vill att toopass tooyour DSC-konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="76839-140">Defines any parameters you would like toopass tooyour DSC configuration.</span></span> <span data-ttu-id="76839-141">Den här egenskapen är inte krypterad.</span><span class="sxs-lookup"><span data-stu-id="76839-141">This property is not encrypted.</span></span> |
| <span data-ttu-id="76839-142">settings.configurationData.url</span><span class="sxs-lookup"><span data-stu-id="76839-142">settings.configurationData.url</span></span> |<span data-ttu-id="76839-143">Sträng</span><span class="sxs-lookup"><span data-stu-id="76839-143">string</span></span> |<span data-ttu-id="76839-144">Anger hello URL från vilken toodownload filen konfigurationsdata (.psd1) toouse som indata för DSC-konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="76839-144">Specifies hello URL from which toodownload your configuration data (.psd1) file toouse as input for your DSC configuration.</span></span> <span data-ttu-id="76839-145">Om hello URL: en kräver en SAS-token för åtkomst, måste tooset hello protectedSettings.configurationDataUrlSasToken toohello egenskapsvärde SAS-token.</span><span class="sxs-lookup"><span data-stu-id="76839-145">If hello URL provided requires a SAS token for access, you need tooset hello protectedSettings.configurationDataUrlSasToken property toohello value of your SAS token.</span></span> |
| <span data-ttu-id="76839-146">settings.privacy.dataEnabled</span><span class="sxs-lookup"><span data-stu-id="76839-146">settings.privacy.dataEnabled</span></span> |<span data-ttu-id="76839-147">Sträng</span><span class="sxs-lookup"><span data-stu-id="76839-147">string</span></span> |<span data-ttu-id="76839-148">Aktiverar eller inaktiverar telemetri samling.</span><span class="sxs-lookup"><span data-stu-id="76839-148">Enables or disables telemetry collection.</span></span> <span data-ttu-id="76839-149">hello endast möjliga värden för den här egenskapen är **'Aktivera', inaktivera, '', eller $null**.</span><span class="sxs-lookup"><span data-stu-id="76839-149">hello only possible values for this property are **'Enable', 'Disable', '', or $null**.</span></span> <span data-ttu-id="76839-150">Om du lämnar den här egenskapen är tomt eller null kan telemetri.</span><span class="sxs-lookup"><span data-stu-id="76839-150">Leaving this property blank or null enables telemetry.</span></span> <span data-ttu-id="76839-151">hello standardvärdet är ''.</span><span class="sxs-lookup"><span data-stu-id="76839-151">hello default value is ''.</span></span> [<span data-ttu-id="76839-152">Mer Information</span><span class="sxs-lookup"><span data-stu-id="76839-152">More Information</span></span>](https://blogs.msdn.microsoft.com/powershell/2016/02/02/azure-dsc-extension-data-collection-2/) |
| <span data-ttu-id="76839-153">settings.advancedOptions.downloadMappings</span><span class="sxs-lookup"><span data-stu-id="76839-153">settings.advancedOptions.downloadMappings</span></span> |<span data-ttu-id="76839-154">Samling</span><span class="sxs-lookup"><span data-stu-id="76839-154">Collection</span></span> |<span data-ttu-id="76839-155">Definierar alternativa platser från vilka toodownload hello WMF.</span><span class="sxs-lookup"><span data-stu-id="76839-155">Defines alternate locations from which toodownload hello WMF.</span></span> [<span data-ttu-id="76839-156">Mer Information</span><span class="sxs-lookup"><span data-stu-id="76839-156">More Information</span></span>](http://blogs.msdn.com/b/powershell/archive/2015/10/21/azure-dsc-extension-2-2-amp-how-to-map-downloads-of-the-extension-dependencies-to-your-own-location.aspx) |
| <span data-ttu-id="76839-157">protectedSettings.configurationArguments</span><span class="sxs-lookup"><span data-stu-id="76839-157">protectedSettings.configurationArguments</span></span> |<span data-ttu-id="76839-158">Samling</span><span class="sxs-lookup"><span data-stu-id="76839-158">Collection</span></span> |<span data-ttu-id="76839-159">Definierar de parametrar som du vill att toopass tooyour DSC-konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="76839-159">Defines any parameters you would like toopass tooyour DSC configuration.</span></span> <span data-ttu-id="76839-160">Den här egenskapen är krypterad.</span><span class="sxs-lookup"><span data-stu-id="76839-160">This property is encrypted.</span></span> |
| <span data-ttu-id="76839-161">protectedSettings.configurationUrlSasToken</span><span class="sxs-lookup"><span data-stu-id="76839-161">protectedSettings.configurationUrlSasToken</span></span> |<span data-ttu-id="76839-162">Sträng</span><span class="sxs-lookup"><span data-stu-id="76839-162">string</span></span> |<span data-ttu-id="76839-163">Anger hello SAS-token tooaccess hello URL som definierats av configuration.url.</span><span class="sxs-lookup"><span data-stu-id="76839-163">Specifies hello SAS token tooaccess hello URL defined by configuration.url.</span></span> <span data-ttu-id="76839-164">Den här egenskapen är krypterad.</span><span class="sxs-lookup"><span data-stu-id="76839-164">This property is encrypted.</span></span> |
| <span data-ttu-id="76839-165">protectedSettings.configurationDataUrlSasToken</span><span class="sxs-lookup"><span data-stu-id="76839-165">protectedSettings.configurationDataUrlSasToken</span></span> |<span data-ttu-id="76839-166">Sträng</span><span class="sxs-lookup"><span data-stu-id="76839-166">string</span></span> |<span data-ttu-id="76839-167">Anger hello SAS-token tooaccess hello URL som definierats av configurationData.url.</span><span class="sxs-lookup"><span data-stu-id="76839-167">Specifies hello SAS token tooaccess hello URL defined by configurationData.url.</span></span> <span data-ttu-id="76839-168">Den här egenskapen är krypterad.</span><span class="sxs-lookup"><span data-stu-id="76839-168">This property is encrypted.</span></span> |

## <a name="settings-vs-protectedsettings"></a><span data-ttu-id="76839-169">Inställningar för vs. ProtectedSettings</span><span class="sxs-lookup"><span data-stu-id="76839-169">Settings vs. ProtectedSettings</span></span>
<span data-ttu-id="76839-170">Alla inställningarna sparas i en textfil i inställningar på hello VM.</span><span class="sxs-lookup"><span data-stu-id="76839-170">All settings are saved in a settings text file on hello VM.</span></span>
<span data-ttu-id="76839-171">Egenskaper under ”inställningar” är offentliga egenskaper eftersom de inte är krypterade i hello text inställningsfil.</span><span class="sxs-lookup"><span data-stu-id="76839-171">Properties under 'settings' are public properties because they are not encrypted in hello settings text file.</span></span>
<span data-ttu-id="76839-172">Egenskaperna under 'protectedSettings' krypteras med ett certifikat och visas inte i oformaterad text i den här filen på hello VM.</span><span class="sxs-lookup"><span data-stu-id="76839-172">Properties under 'protectedSettings' are encrypted with a certificate and are not shown in plain text in this file on hello VM.</span></span>

<span data-ttu-id="76839-173">Om hello konfiguration måste autentiseringsuppgifter, kan de ingå i protectedSettings:</span><span class="sxs-lookup"><span data-stu-id="76839-173">If hello configuration needs credentials, they can be included in protectedSettings:</span></span>

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

## <a name="example"></a><span data-ttu-id="76839-174">Exempel</span><span class="sxs-lookup"><span data-stu-id="76839-174">Example</span></span>
<span data-ttu-id="76839-175">hello följande exempel härleds från hello ”Kom-igång-avsnittet i hello [översiktssidan för DSC-tillägg hanteraren](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="76839-175">hello following example derives from hello "Getting Started" section of hello [DSC Extension Handler Overview page](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
<span data-ttu-id="76839-176">Det här exemplet använder Resource Manager-mallar i stället för cmdlets toodeploy hello tillägg.</span><span class="sxs-lookup"><span data-stu-id="76839-176">This example uses Resource Manager templates instead of cmdlets toodeploy hello extension.</span></span> <span data-ttu-id="76839-177">Spara hello ”IisInstall.ps1” konfiguration, placera den i en. ZIP-fil och överför hello-fil i en URL som kan nås.</span><span class="sxs-lookup"><span data-stu-id="76839-177">Save hello "IisInstall.ps1" configuration, place it in a .ZIP file, and upload hello file in an accessible URL.</span></span> <span data-ttu-id="76839-178">Det här exemplet använder Azure blob storage, men det är möjligt toodownload. ZIP-filer från en valfri plats.</span><span class="sxs-lookup"><span data-stu-id="76839-178">This example uses Azure blob storage, but it is possible toodownload .ZIP files from any arbitrary location.</span></span>

<span data-ttu-id="76839-179">I mallen för hello Azure Resource Manager hello följande kod instruerar hello VM toodownload hello rätt fil och kör hello önskad funktion i PowerShell:</span><span class="sxs-lookup"><span data-stu-id="76839-179">In hello Azure Resource Manager template, hello following code instructs hello VM toodownload hello correct file and run hello appropriate PowerShell function:</span></span>

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

## <a name="updating-from-hello-previous-format"></a><span data-ttu-id="76839-180">Uppdatera från hello tidigare Format</span><span class="sxs-lookup"><span data-stu-id="76839-180">Updating from hello Previous Format</span></span>
<span data-ttu-id="76839-181">Alla inställningar i hello tidigare format (som innehåller hello offentliga egenskaper ModulesUrl ConfigurationFunction, SasToken eller egenskaper) automatiskt anpassa toohello nuvarande format och kör precis som de gjorde före.</span><span class="sxs-lookup"><span data-stu-id="76839-181">Any settings in hello previous format (containing hello public properties ModulesUrl, ConfigurationFunction, SasToken, or Properties) automatically adapt toohello current format and run just as they did before.</span></span>

<span data-ttu-id="76839-182">hello följer schemat är vilka hello tidigare inställningar schema kan se ut:</span><span class="sxs-lookup"><span data-stu-id="76839-182">hello following schema is what hello previous settings schema looked like:</span></span>

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

<span data-ttu-id="76839-183">Här är hur hello tidigare formatet anpassas efter dina behov toohello nuvarande format:</span><span class="sxs-lookup"><span data-stu-id="76839-183">Here's how hello previous format adapts toohello current format:</span></span>

| <span data-ttu-id="76839-184">Egenskapsnamn</span><span class="sxs-lookup"><span data-stu-id="76839-184">Property Name</span></span> | <span data-ttu-id="76839-185">Tidigare schemat motsvarande</span><span class="sxs-lookup"><span data-stu-id="76839-185">Previous Schema Equivalent</span></span> |
| --- | --- |
| <span data-ttu-id="76839-186">settings.wmfVersion</span><span class="sxs-lookup"><span data-stu-id="76839-186">settings.wmfVersion</span></span> |<span data-ttu-id="76839-187">inställningar. WMFVersion</span><span class="sxs-lookup"><span data-stu-id="76839-187">settings.WMFVersion</span></span> |
| <span data-ttu-id="76839-188">Settings.Configuration.URL</span><span class="sxs-lookup"><span data-stu-id="76839-188">settings.configuration.url</span></span> |<span data-ttu-id="76839-189">inställningar. ModulesUrl</span><span class="sxs-lookup"><span data-stu-id="76839-189">settings.ModulesUrl</span></span> |
| <span data-ttu-id="76839-190">Settings.Configuration.Script</span><span class="sxs-lookup"><span data-stu-id="76839-190">settings.configuration.script</span></span> |<span data-ttu-id="76839-191">Första delen av inställningar. ConfigurationFunction (före '\\\\”)</span><span class="sxs-lookup"><span data-stu-id="76839-191">First part of settings.ConfigurationFunction (before '\\\\')</span></span> |
| <span data-ttu-id="76839-192">Settings.Configuration.Function</span><span class="sxs-lookup"><span data-stu-id="76839-192">settings.configuration.function</span></span> |<span data-ttu-id="76839-193">Andra delen av inställningar. ConfigurationFunction (när '\\\\”)</span><span class="sxs-lookup"><span data-stu-id="76839-193">Second part of settings.ConfigurationFunction (after '\\\\')</span></span> |
| <span data-ttu-id="76839-194">settings.configurationArguments</span><span class="sxs-lookup"><span data-stu-id="76839-194">settings.configurationArguments</span></span> |<span data-ttu-id="76839-195">inställningar. Egenskaper</span><span class="sxs-lookup"><span data-stu-id="76839-195">settings.Properties</span></span> |
| <span data-ttu-id="76839-196">settings.configurationData.url</span><span class="sxs-lookup"><span data-stu-id="76839-196">settings.configurationData.url</span></span> |<span data-ttu-id="76839-197">protectedSettings.DataBlobUri (utan SAS-token)</span><span class="sxs-lookup"><span data-stu-id="76839-197">protectedSettings.DataBlobUri (without SAS token)</span></span> |
| <span data-ttu-id="76839-198">settings.privacy.dataEnabled</span><span class="sxs-lookup"><span data-stu-id="76839-198">settings.privacy.dataEnabled</span></span> |<span data-ttu-id="76839-199">inställningar. Privacy.DataEnabled</span><span class="sxs-lookup"><span data-stu-id="76839-199">settings.Privacy.DataEnabled</span></span> |
| <span data-ttu-id="76839-200">settings.advancedOptions.downloadMappings</span><span class="sxs-lookup"><span data-stu-id="76839-200">settings.advancedOptions.downloadMappings</span></span> |<span data-ttu-id="76839-201">inställningar. AdvancedOptions.DownloadMappings</span><span class="sxs-lookup"><span data-stu-id="76839-201">settings.AdvancedOptions.DownloadMappings</span></span> |
| <span data-ttu-id="76839-202">protectedSettings.configurationArguments</span><span class="sxs-lookup"><span data-stu-id="76839-202">protectedSettings.configurationArguments</span></span> |<span data-ttu-id="76839-203">protectedSettings.Properties</span><span class="sxs-lookup"><span data-stu-id="76839-203">protectedSettings.Properties</span></span> |
| <span data-ttu-id="76839-204">protectedSettings.configurationUrlSasToken</span><span class="sxs-lookup"><span data-stu-id="76839-204">protectedSettings.configurationUrlSasToken</span></span> |<span data-ttu-id="76839-205">inställningar. SasToken</span><span class="sxs-lookup"><span data-stu-id="76839-205">settings.SasToken</span></span> |
| <span data-ttu-id="76839-206">protectedSettings.configurationDataUrlSasToken</span><span class="sxs-lookup"><span data-stu-id="76839-206">protectedSettings.configurationDataUrlSasToken</span></span> |<span data-ttu-id="76839-207">SAS-token från protectedSettings.DataBlobUri</span><span class="sxs-lookup"><span data-stu-id="76839-207">SAS token from protectedSettings.DataBlobUri</span></span> |

## <a name="troubleshooting---error-code-1100"></a><span data-ttu-id="76839-208">Felsökning – felkoden 1100</span><span class="sxs-lookup"><span data-stu-id="76839-208">Troubleshooting - Error Code 1100</span></span>
<span data-ttu-id="76839-209">Felkoden 1100 anger att det finns ett problem med hello användarindata toohello DSC-tillägg.</span><span class="sxs-lookup"><span data-stu-id="76839-209">Error Code 1100 indicates that there is a problem with hello user input toohello DSC extension.</span></span>
<span data-ttu-id="76839-210">hello texten för dessa fel är variabel och kan ändras.</span><span class="sxs-lookup"><span data-stu-id="76839-210">hello text of these errors is variable and may change.</span></span>
<span data-ttu-id="76839-211">Här är några av hello fel kan du stöta på och hur du kan åtgärda dem.</span><span class="sxs-lookup"><span data-stu-id="76839-211">Here are some of hello errors you may run into and how you can fix them.</span></span>

### <a name="invalid-values"></a><span data-ttu-id="76839-212">Ogiltiga värden</span><span class="sxs-lookup"><span data-stu-id="76839-212">Invalid Values</span></span>
<span data-ttu-id="76839-213">”Privacy.dataCollection är: {0}.</span><span class="sxs-lookup"><span data-stu-id="76839-213">"Privacy.dataCollection is '{0}'.</span></span> <span data-ttu-id="76839-214">hello endast möjliga värden är ”,” aktivera ”och” inaktivera ”” ”WmfVersion är: {0}.</span><span class="sxs-lookup"><span data-stu-id="76839-214">hello only possible values are '', 'Enable', and 'Disable'" "WmfVersion is '{0}'.</span></span> <span data-ttu-id="76839-215">Endast möjliga värden är...</span><span class="sxs-lookup"><span data-stu-id="76839-215">Only possible values are …</span></span> <span data-ttu-id="76839-216">och ”senaste” ”</span><span class="sxs-lookup"><span data-stu-id="76839-216">and 'latest'"</span></span>

<span data-ttu-id="76839-217">Problem: Det är inte tillåtet för ett angivet värde.</span><span class="sxs-lookup"><span data-stu-id="76839-217">Problem: A provided value is not allowed.</span></span>

<span data-ttu-id="76839-218">Lösning: Ändra hello ogiltigt värde tooa giltigt värde.</span><span class="sxs-lookup"><span data-stu-id="76839-218">Solution: Change hello invalid value tooa valid value.</span></span> <span data-ttu-id="76839-219">Se tabellen hello i hello informationsavsnittet.</span><span class="sxs-lookup"><span data-stu-id="76839-219">See hello table in hello Details section.</span></span>

### <a name="invalid-url"></a><span data-ttu-id="76839-220">Ogiltig URL</span><span class="sxs-lookup"><span data-stu-id="76839-220">Invalid URL</span></span>
<span data-ttu-id="76839-221">”ConfigurationData.url är: {0}.</span><span class="sxs-lookup"><span data-stu-id="76839-221">"ConfigurationData.url is '{0}'.</span></span> <span data-ttu-id="76839-222">Detta är inte en giltig URL ”” DataBlobUri är: {0}.</span><span class="sxs-lookup"><span data-stu-id="76839-222">This is not a valid URL" "DataBlobUri is '{0}'.</span></span> <span data-ttu-id="76839-223">Detta är inte en giltig URL ”” Configuration.url är: {0}.</span><span class="sxs-lookup"><span data-stu-id="76839-223">This is not a valid URL" "Configuration.url is '{0}'.</span></span> <span data-ttu-id="76839-224">Detta är inte en giltig URL ”</span><span class="sxs-lookup"><span data-stu-id="76839-224">This is not a valid URL"</span></span>

<span data-ttu-id="76839-225">Problem: A angivna Webbadressen inte är giltig.</span><span class="sxs-lookup"><span data-stu-id="76839-225">Problem: A provided URL is not valid.</span></span>

<span data-ttu-id="76839-226">Lösning: Kontrollera att alla dina angivna URL: er.</span><span class="sxs-lookup"><span data-stu-id="76839-226">Solution: Check all your provided URLs.</span></span> <span data-ttu-id="76839-227">Kontrollera att alla URL: er lösa toovalid platser som hello tillägget kan komma åt på hello fjärrdatorn.</span><span class="sxs-lookup"><span data-stu-id="76839-227">Make sure all URLs resolve toovalid locations that hello extension can access on hello remote machine.</span></span>

### <a name="invalid-configurationargument-type"></a><span data-ttu-id="76839-228">Ogiltig ConfigurationArgument-typ.</span><span class="sxs-lookup"><span data-stu-id="76839-228">Invalid ConfigurationArgument Type</span></span>
<span data-ttu-id="76839-229">”Ogiltigt configurationArguments typ {0}”</span><span class="sxs-lookup"><span data-stu-id="76839-229">"Invalid configurationArguments type {0}"</span></span>

<span data-ttu-id="76839-230">Problem: hello ConfigurationArguments egenskapen kan inte lösa tooa Hashtable-objektet.</span><span class="sxs-lookup"><span data-stu-id="76839-230">Problem: hello ConfigurationArguments property cannot resolve tooa Hashtable object.</span></span> 

<span data-ttu-id="76839-231">Lösning: Kontrollera ConfigurationArguments-egenskap en hash-tabell.</span><span class="sxs-lookup"><span data-stu-id="76839-231">Solution: Make your ConfigurationArguments property a Hashtable.</span></span> <span data-ttu-id="76839-232">Följ hello-format som tillhandahålls i hello föregående exempel.</span><span class="sxs-lookup"><span data-stu-id="76839-232">Follow hello format provided in hello preceeding example.</span></span> <span data-ttu-id="76839-233">Se upp för citattecken, kommatecken och klammerparenteser.</span><span class="sxs-lookup"><span data-stu-id="76839-233">Watch out for quotes, commas, and braces.</span></span>

### <a name="duplicate-configurationarguments"></a><span data-ttu-id="76839-234">Duplicera ConfigurationArguments</span><span class="sxs-lookup"><span data-stu-id="76839-234">Duplicate ConfigurationArguments</span></span>
<span data-ttu-id="76839-235">”Finns duplicerade argument: {0} i både offentliga och skyddade configurationArguments”</span><span class="sxs-lookup"><span data-stu-id="76839-235">"Found duplicate arguments '{0}' in both public and protected configurationArguments"</span></span>

<span data-ttu-id="76839-236">Problem: hello ConfigurationArguments i inställningar för offentliga och hello ConfigurationArguments i skyddade inställningarna innehåller egenskaper med hello samma namn.</span><span class="sxs-lookup"><span data-stu-id="76839-236">Problem: hello ConfigurationArguments in public settings and hello ConfigurationArguments in protected settings contain properties with hello same name.</span></span>

<span data-ttu-id="76839-237">Lösning: Ta bort en av hello dubblettegenskaper.</span><span class="sxs-lookup"><span data-stu-id="76839-237">Solution: Remove one of hello duplicate properties.</span></span>

### <a name="missing-properties"></a><span data-ttu-id="76839-238">Egenskaper som saknas</span><span class="sxs-lookup"><span data-stu-id="76839-238">Missing Properties</span></span>
<span data-ttu-id="76839-239">”Configuration.function kräver att configuration.url eller configuration.module har angetts”</span><span class="sxs-lookup"><span data-stu-id="76839-239">"Configuration.function requires that configuration.url or configuration.module is specified"</span></span>

<span data-ttu-id="76839-240">”Configuration.url kräver att configuration.script anges”</span><span class="sxs-lookup"><span data-stu-id="76839-240">"Configuration.url requires that configuration.script is specified"</span></span>

<span data-ttu-id="76839-241">”Configuration.script kräver att configuration.url anges”</span><span class="sxs-lookup"><span data-stu-id="76839-241">"Configuration.script requires that configuration.url is specified"</span></span>

<span data-ttu-id="76839-242">”Configuration.url kräver att configuration.function anges”</span><span class="sxs-lookup"><span data-stu-id="76839-242">"Configuration.url requires that configuration.function is specified"</span></span>

<span data-ttu-id="76839-243">”ConfigurationUrlSasToken kräver att configuration.url anges”</span><span class="sxs-lookup"><span data-stu-id="76839-243">"ConfigurationUrlSasToken requires that configuration.url is specified"</span></span>

<span data-ttu-id="76839-244">”ConfigurationDataUrlSasToken kräver att configurationData.url anges”</span><span class="sxs-lookup"><span data-stu-id="76839-244">"ConfigurationDataUrlSasToken requires that configurationData.url is specified"</span></span>

<span data-ttu-id="76839-245">Problem: En definierad egenskap måste en annan egenskap som saknas.</span><span class="sxs-lookup"><span data-stu-id="76839-245">Problem: A defined property needs another property that is missing.</span></span>

<span data-ttu-id="76839-246">Lösningar:</span><span class="sxs-lookup"><span data-stu-id="76839-246">Solutions:</span></span> 

* <span data-ttu-id="76839-247">Ange hello egenskap som saknas.</span><span class="sxs-lookup"><span data-stu-id="76839-247">Provide hello missing property.</span></span>
* <span data-ttu-id="76839-248">Ta bort hello-egenskap som behöver hello egenskap som saknas.</span><span class="sxs-lookup"><span data-stu-id="76839-248">Remove hello property that needs hello missing property.</span></span>

## <a name="next-steps"></a><span data-ttu-id="76839-249">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="76839-249">Next Steps</span></span>
<span data-ttu-id="76839-250">Läs mer om DSC och virtuella datorn anger i [med hjälp av virtuella Skalningsuppsättningarna med hello Azure DSC-tillägg](../../virtual-machine-scale-sets/virtual-machine-scale-sets-dsc.md)</span><span class="sxs-lookup"><span data-stu-id="76839-250">Learn about DSC and virtual machine scale sets in [Using Virtual Machine Scale Sets with hello Azure DSC Extension](../../virtual-machine-scale-sets/virtual-machine-scale-sets-dsc.md)</span></span>

<span data-ttu-id="76839-251">Mer information finns på [hantering av DSC-säkra autentiseringsuppgifter](extensions-dsc-credentials.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="76839-251">Find more details on [DSC's secure credential management](extensions-dsc-credentials.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> 

<span data-ttu-id="76839-252">Mer information om hello Azure DSC-tillägg-hanteraren finns [introduktion toohello Azure Desired State Configuration-tillägget hanteraren](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="76839-252">For more information on hello Azure DSC extension handler, see [Introduction toohello Azure Desired State Configuration extension handler](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> 

<span data-ttu-id="76839-253">Mer information om PowerShell DSC [finns hello PowerShell Dokumentationscenter](https://msdn.microsoft.com/powershell/dsc/overview).</span><span class="sxs-lookup"><span data-stu-id="76839-253">For more information about PowerShell DSC, [visit hello PowerShell documentation center](https://msdn.microsoft.com/powershell/dsc/overview).</span></span> 

