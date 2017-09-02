## <a name="overview-of-azure-resource-manager-templates"></a>Översikt över Azure Resource Manager-mallar
Azure Resource Manager-mallar kan du ange deklarativt Azure IaaS-infrastruktur i Json-språk genom att definiera beroenden mellan resurser. En detaljerad översikt över Azure Resource Manager-mallar finns i artikel nedan:

[Översikt över resursgruppen.](../articles/azure-resource-manager/resource-group-overview.md)

## <a name="sample-template-snippet-for-vm-extensions"></a>Exempel mallen fragment för VM-tillägg
Distribuera VM-tillägg som en del av en Azure Resource Manager måste mall du ange deklarativt tilläggets konfiguration i mallen.
Här är formatet för att ange tilläggets konfiguration.

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

Som du ser i ovanstående innehåller tillägget mallen två delar:

1. Namn, utgivare och version
2. Konfiguration av tillägg.

## <a name="identifying-the-publisher-type-and-typehandlerversion-for-any-extension"></a>Identifiera utgivare, typ och typeHandlerVersion för alla tillägg
Azure VM-tillägg som är utgivna av Microsoft och tillförlitliga 3 part utgivare och varje tillägg identifieras unikt med dess utgivare, typ och typeHandlerVersion. Dessa kan fastställas enligt följande:  

