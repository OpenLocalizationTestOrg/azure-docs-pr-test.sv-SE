## <a name="overview-of-azure-resource-manager-templates"></a><span data-ttu-id="455c8-101">Översikt över Azure Resource Manager-mallar</span><span class="sxs-lookup"><span data-stu-id="455c8-101">Overview of Azure Resource Manager templates</span></span>
<span data-ttu-id="455c8-102">Azure Resource Manager-mallar kan du ange toodeclaratively hello Azure IaaS-infrastruktur i Json-språk genom att definiera hello beroenden mellan resurser.</span><span class="sxs-lookup"><span data-stu-id="455c8-102">Azure Resource Manager templates allow you toodeclaratively specify hello Azure IaaS infrastructure in Json language by defining hello dependencies between resources.</span></span> <span data-ttu-id="455c8-103">En detaljerad översikt över Azure Resource Manager-mallar finns toohello artikel nedan:</span><span class="sxs-lookup"><span data-stu-id="455c8-103">For a detailed overview of Azure Resource Manager Templates, please refer toohello article below:</span></span>

[<span data-ttu-id="455c8-104">Översikt över resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="455c8-104">Resource Group Overview</span></span>](../articles/azure-resource-manager/resource-group-overview.md)

## <a name="sample-template-snippet-for-vm-extensions"></a><span data-ttu-id="455c8-105">Exempel mallen fragment för VM-tillägg</span><span class="sxs-lookup"><span data-stu-id="455c8-105">Sample template snippet for VM extensions</span></span>
<span data-ttu-id="455c8-106">Distribuera VM-tillägg som en del av en Azure Resource Manager-mall måste du ange toodeclaratively hello tilläggets konfiguration i hello mallen.</span><span class="sxs-lookup"><span data-stu-id="455c8-106">Deploying VM extensions as part of an Azure Resource Manager template requires you toodeclaratively specify hello extension configuration in hello template.</span></span>
<span data-ttu-id="455c8-107">Här är hello format för att ange hello tilläggets konfiguration.</span><span class="sxs-lookup"><span data-stu-id="455c8-107">Here is hello format for specifying hello extension configuration.</span></span>

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

<span data-ttu-id="455c8-108">Som du ser i hello ovan innehåller hello tillägget mallen två delar:</span><span class="sxs-lookup"><span data-stu-id="455c8-108">As you can see from hello above, hello extension template contains two main parts:</span></span>

1. <span data-ttu-id="455c8-109">Namn, utgivare och version</span><span class="sxs-lookup"><span data-stu-id="455c8-109">Extension name, publisher and version</span></span>
2. <span data-ttu-id="455c8-110">Konfiguration av tillägg.</span><span class="sxs-lookup"><span data-stu-id="455c8-110">Extension Configuration.</span></span>

## <a name="identifying-hello-publisher-type-and-typehandlerversion-for-any-extension"></a><span data-ttu-id="455c8-111">Identifiera hello utgivare, typ och typeHandlerVersion för alla tillägg</span><span class="sxs-lookup"><span data-stu-id="455c8-111">Identifying hello publisher, type, and typeHandlerVersion for any extension</span></span>
<span data-ttu-id="455c8-112">Azure VM-tillägg som är utgivna av Microsoft och tillförlitliga 3 part utgivare och varje tillägg identifieras unikt med dess utgivare, typ och hello typeHandlerVersion.</span><span class="sxs-lookup"><span data-stu-id="455c8-112">Azure VM extensions are published by Microsoft and trusted 3rd party publishers and each extension is uniquely identified by its publisher,type and hello typeHandlerVersion.</span></span> <span data-ttu-id="455c8-113">Dessa kan fastställas enligt följande:</span><span class="sxs-lookup"><span data-stu-id="455c8-113">These can be determined as following:</span></span>  

