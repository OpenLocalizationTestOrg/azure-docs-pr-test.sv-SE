## <a name="overview-of-azure-resource-manager-templates"></a>Översikt över Azure Resource Manager-mallar
Azure Resource Manager-mallar kan du ange toodeclaratively hello Azure IaaS-infrastruktur i Json-språk genom att definiera hello beroenden mellan resurser. En detaljerad översikt över Azure Resource Manager-mallar finns toohello artikel nedan:

[Översikt över resursgruppen.](../articles/azure-resource-manager/resource-group-overview.md)

## <a name="sample-template-snippet-for-vm-extensions"></a>Exempel mallen fragment för VM-tillägg
Distribuera VM-tillägg som en del av en Azure Resource Manager-mall måste du ange toodeclaratively hello tilläggets konfiguration i hello mallen.
Här är hello format för att ange hello tilläggets konfiguration.

      {
      "type": "Microsoft.Compute/virtualMachines/extensions",
      "name": "MyExtension",
      "apiVersion": "2015-05-01-preview",
      "location": "[parameters('location')]",
      "dependsOn": ["[concat('Microsoft.Compute/virtualMachines/',parameters('vmName'))]"],
      "properties":
      {
      "publisher": "Publisher Namespace",
      "type": "extension Name",
      "typeHandlerVersion": "extension version",
      "settings": {
      // Extension specific configuration goes in here.
      }
      }
      }

Som du ser i hello ovan innehåller hello tillägget mallen två delar:

1. Namn, utgivare och version
2. Konfiguration av tillägg.

## <a name="identifying-hello-publisher-type-and-typehandlerversion-for-any-extension"></a>Identifiera hello utgivare, typ och typeHandlerVersion för alla tillägg
Azure VM-tillägg som är utgivna av Microsoft och tillförlitliga 3 part utgivare och varje tillägg identifieras unikt med dess utgivare, typ och hello typeHandlerVersion. Dessa kan fastställas enligt följande:  

