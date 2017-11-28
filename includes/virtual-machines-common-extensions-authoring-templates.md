## <a name="overview-of-azure-resource-manager-templates"></a><span data-ttu-id="891df-101">Översikt över Azure Resource Manager-mallar</span><span class="sxs-lookup"><span data-stu-id="891df-101">Overview of Azure Resource Manager templates</span></span>
<span data-ttu-id="891df-102">Azure Resource Manager-mallar kan du ange deklarativt Azure IaaS-infrastruktur i Json-språk genom att definiera beroenden mellan resurser.</span><span class="sxs-lookup"><span data-stu-id="891df-102">Azure Resource Manager templates allow you to declaratively specify the Azure IaaS infrastructure in Json language by defining the dependencies between resources.</span></span> <span data-ttu-id="891df-103">En detaljerad översikt över Azure Resource Manager-mallar finns i artikel nedan:</span><span class="sxs-lookup"><span data-stu-id="891df-103">For a detailed overview of Azure Resource Manager Templates, please refer to the article below:</span></span>

[<span data-ttu-id="891df-104">Översikt över resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="891df-104">Resource Group Overview</span></span>](../articles/azure-resource-manager/resource-group-overview.md)

## <a name="sample-template-snippet-for-vm-extensions"></a><span data-ttu-id="891df-105">Exempel mallen fragment för VM-tillägg</span><span class="sxs-lookup"><span data-stu-id="891df-105">Sample template snippet for VM extensions</span></span>
<span data-ttu-id="891df-106">Distribuera VM-tillägg som en del av en Azure Resource Manager måste mall du ange deklarativt tilläggets konfiguration i mallen.</span><span class="sxs-lookup"><span data-stu-id="891df-106">Deploying VM extensions as part of an Azure Resource Manager template requires you to declaratively specify the extension configuration in the template.</span></span>
<span data-ttu-id="891df-107">Här är formatet för att ange tilläggets konfiguration.</span><span class="sxs-lookup"><span data-stu-id="891df-107">Here is the format for specifying the extension configuration.</span></span>

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

<span data-ttu-id="891df-108">Som du ser i ovanstående innehåller tillägget mallen två delar:</span><span class="sxs-lookup"><span data-stu-id="891df-108">As you can see from the above, the extension template contains two main parts:</span></span>

1. <span data-ttu-id="891df-109">Namn, utgivare och version</span><span class="sxs-lookup"><span data-stu-id="891df-109">Extension name, publisher and version</span></span>
2. <span data-ttu-id="891df-110">Konfiguration av tillägg.</span><span class="sxs-lookup"><span data-stu-id="891df-110">Extension Configuration.</span></span>

## <a name="identifying-the-publisher-type-and-typehandlerversion-for-any-extension"></a><span data-ttu-id="891df-111">Identifiera utgivare, typ och typeHandlerVersion för alla tillägg</span><span class="sxs-lookup"><span data-stu-id="891df-111">Identifying the publisher, type, and typeHandlerVersion for any extension</span></span>
<span data-ttu-id="891df-112">Azure VM-tillägg som är utgivna av Microsoft och tillförlitliga 3 part utgivare och varje tillägg identifieras unikt med dess utgivare, typ och typeHandlerVersion.</span><span class="sxs-lookup"><span data-stu-id="891df-112">Azure VM extensions are published by Microsoft and trusted 3rd party publishers and each extension is uniquely identified by its publisher,type and the typeHandlerVersion.</span></span> <span data-ttu-id="891df-113">Dessa kan fastställas enligt följande:</span><span class="sxs-lookup"><span data-stu-id="891df-113">These can be determined as following:</span></span>  

