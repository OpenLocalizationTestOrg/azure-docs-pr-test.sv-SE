---
title: "aaaRun Linux på den virtuella datorn datornoder - Azure Batch | Microsoft Docs"
description: "Lär dig hur tooprocess din parallell compute arbetsbelastningar på pooler för Linux-datorer i Azure Batch."
services: batch
documentationcenter: python
author: tamram
manager: timlt
editor: 
ms.assetid: dc6ba151-1718-468a-b455-2da549225ab2
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: na
ms.date: 05/22/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3daabd5c577baaafd0544f9f7913cb7b116d74d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="provision-linux-compute-nodes-in-batch-pools"></a>Etablera Linux compute-noder i Batch-pooler

Du kan använda Azure Batch toorun parallella beräkning av arbetsbelastningar på Linux- och Windows-datorer. Den här artikeln beskrivs hur toocreate pooler för Linux compute-noder i hello Batch-tjänsten med hjälp av både hello [Batch Python] [ py_batch_package] och [Batch .NET] [ api_net] klientbibliotek.

> [!NOTE]
> Programpaket kan användas för alla Batch-pooler som skapats efter 5 juli 2017. De stöds i Batch-adresspooler skapa mellan 10 mars 2016 och 5 juli 2017 om hello poolen har skapats med en tjänst i molnet. Batch-pooler som skapats tidigare too10 mars 2016 stöder inte programpaket. Mer information om hur du använder programmet paket toodeploy program tooyour Batch noderna, finns [distribuera program toocompute noder med Batch-programpaket](batch-application-packages.md).
>
>

## <a name="virtual-machine-configuration"></a>Konfiguration av virtuell dator
När du skapar en pool med compute-noder i Batch har du två alternativ från vilka tooselect hello nod storleken och operativsystemet: moln-tjänsternas konfiguration och konfiguration av virtuell dator.

En **Cloud Services-konfiguration** tillhandahåller *endast*Windows-beräkningsnoder. Tillgängliga compute-nod storlekar listas i [storlekar för molntjänster](../cloud-services/cloud-services-sizes-specs.md), och tillgängliga operativsystem visas i hello [Azure gäst-OS-versioner och SDK-kompatibilitetsmatris](../cloud-services/cloud-services-guestos-update-matrix.md). När du skapar en pool som innehåller Azure Cloud Services noder kan du ange hello nodstorlek och hello OS-familj som beskrivs i hello tidigare nämnts artiklar. Molntjänster används oftast för pooler för Windows-beräkningsnoder.

**Konfiguration av virtuell dator** innehåller både Linux och Windows-avbildningar för compute-noder. Tillgängliga compute-nod storlekar listas i [storlekar för virtuella datorer i Azure](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (Linux) och [storlekar för virtuella datorer i Azure](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (Windows). När du skapar en pool som innehåller konfiguration av virtuell dator noder, måste du ange hello storleken på hello noder och hello virtuella bildreferens hello Batch nod agent SKU toobe har installerats på hello noder.

### <a name="virtual-machine-image-reference"></a>Bildreferens för virtuell dator
Hej Batch-tjänsten används [skalningsuppsättningar i virtuella](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) tooprovide Linux compute-noder. Du kan ange en bild från hello [Azure Marketplace][vm_marketplace], eller ange en anpassad avbildning som du har förberett. Mer information om anpassade avbildningar finns i [Utveckla storskaliga parallella beräkningslösningar med Batch](batch-api-basics.md#pool).

När du konfigurerar en virtuell dator en bildreferens kan ange du hello egenskaper för hello avbildning av virtuell dator. När du skapar en virtuell dator bildreferens krävs hello följande egenskaper:

| **Referens för Bildegenskaper** | **Exempel** |
| --- | --- |
| Utgivare |Canonical |
| Erbjudande |UbuntuServer |
| SKU |14.04.4-LTS |
| Version |senaste |

> [!TIP]
> Du kan lära dig mer om de här egenskaperna och hur toolist Marketplace bilder i [navigera och välj Linux virtuella avbildningar i Azure med CLI eller PowerShell](../virtual-machines/linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Observera att inte alla Marketplace-bilder är kompatibelt med Batch. Mer information finns i [nod agent SKU](#node-agent-sku).
>
>

### <a name="node-agent-sku"></a>Noden agent SKU
hello Batch nod agent är ett program som körs på varje nod i hello pool och tillhandahåller hello och kommandokontroll gränssnitt mellan hello nod och hello Batch-tjänsten. Det finns olika implementeringar av hello nod agent kallas SKU: er, för olika operativsystem. I princip när du skapar en konfiguration av virtuell dator du första ange hello bildreferens för virtuell dator och du sedan ange hello nod agent tooinstall på hello avbildningen. Normalt varje nod agent SKU är kompatibel med flera virtuella avbildningar. Här följer några exempel på noden agent SKU: er:

* batch.node.Ubuntu 14.04
* batch.node.centos 7
* batch.node.Windows amd64

> [!IMPORTANT]
> Inte alla virtuella avbildningar som är tillgängliga i hello Marketplace är kompatibla med hello tillgängliga Batch nod agenter. Använd hello Batch SDK toolist hello tillgängliga noden agent SKU: er och hello avbildningar av virtuella datorer som de är kompatibla. Se hello [lista virtuella avbildningar](#list-of-virtual-machine-images) senare i den här artikeln för mer information och exempel på hur tooretrieve en lista över giltiga bilder vid körning.
>
>

## <a name="create-a-linux-pool-batch-python"></a>Skapa en pool med Linux: Batch Python
hello följande kodavsnitt visar ett exempel på hur toouse hello [Microsoft Azure Batch-klientbibliotek för Python] [ py_batch_package] toocreate en pool med Ubuntu Server compute-noder. Referera till dokumentationen för hello Batch Python-modulen finns på [azure.batch paketet] [ py_batch_docs] på Läs hello dokumenten.

Den här kodutdrag skapar en [ImageReference] [ py_imagereference] explicit och anger var och en av dess egenskaper (utgivare, erbjudande, SKU och version). I produktionskod, men vi rekommenderar att du använder hello [list_node_agent_skus] [ py_list_skus] metoden toodetermine och välj från hello tillgängliga avbildningen och noden agent SKU kombinationer vid körning.

```python
# Import hello required modules from the
# Azure Batch Client Library for Python
import azure.batch.batch_service_client as batch
import azure.batch.batch_auth as batchauth
import azure.batch.models as batchmodels

# Specify Batch account credentials
account = "<batch-account-name>"
key = "<batch-account-key>"
batch_url = "<batch-account-url>"

# Pool settings
pool_id = "LinuxNodesSamplePoolPython"
vm_size = "STANDARD_A1"
node_count = 1

# Initialize hello Batch client
creds = batchauth.SharedKeyCredentials(account, key)
config = batch.BatchServiceClientConfiguration(creds, base_url = batch_url)
client = batch.BatchServiceClient(config)

# Create hello unbound pool
new_pool = batchmodels.PoolAddParameter(id = pool_id, vm_size = vm_size)
new_pool.target_dedicated = node_count

# Configure hello start task for hello pool
start_task = batchmodels.StartTask()
start_task.run_elevated = True
start_task.command_line = "printenv AZ_BATCH_NODE_STARTUP_DIR"
new_pool.start_task = start_task

# Create an ImageReference which specifies hello Marketplace
# virtual machine image tooinstall on hello nodes.
ir = batchmodels.ImageReference(
    publisher = "Canonical",
    offer = "UbuntuServer",
    sku = "14.04.2-LTS",
    version = "latest")

# Create hello VirtualMachineConfiguration, specifying
# hello VM image reference and hello Batch node agent to
# be installed on hello node.
vmc = batchmodels.VirtualMachineConfiguration(
    image_reference = ir,
    node_agent_sku_id = "batch.node.ubuntu 14.04")

# Assign hello virtual machine configuration toohello pool
new_pool.virtual_machine_configuration = vmc

# Create pool in hello Batch service
client.pool.add(new_pool)
```

Som tidigare nämnts, rekommenderar vi att i stället för att skapa hello [ImageReference] [ py_imagereference] uttryckligen, du kan använda hello [list_node_agent_skus] [ py_list_skus] metoden toodynamically väljer från hello stöds för närvarande nod agent/Marketplace avbildningen kombinationer. Hej följande Python fragment visas hur toouse den här metoden.

```python
# Get hello list of node agents from hello Batch service
nodeagents = client.account.list_node_agent_skus()

# Obtain hello desired node agent
ubuntu1404agent = next(agent for agent in nodeagents if "ubuntu 14.04" in agent.id)

# Pick hello first image reference from hello list of verified references
ir = ubuntu1404agent.verified_image_references[0]

# Create hello VirtualMachineConfiguration, specifying hello VM image
# reference and hello Batch node agent toobe installed on hello node.
vmc = batchmodels.VirtualMachineConfiguration(
    image_reference = ir,
    node_agent_sku_id = ubuntu1404agent.id)
```

## <a name="create-a-linux-pool-batch-net"></a>Skapa en pool med Linux: Batch .NET
hello följande kodavsnitt visar ett exempel på hur toouse hello [Batch .NET] [ nuget_batch_net] klienten biblioteket toocreate en pool med Ubuntu Server compute-noder. Du kan hitta hello [Batch .NET referensdokumentationen] [ api_net] på MSDN.

hello följande kodavsnitt använder hello [PoolOperations][net_pool_ops].[ ListNodeAgentSkus] [ net_list_skus] metoden tooselect hello listan över för närvarande Marketplace-avbildning och noden agent SKU kombinationer som stöds. Den här tekniken är önskvärt eftersom hello lista över kombinationer som stöds kan ändras från tid tootime. Oftast läggs kombinationer som stöds.

```csharp
// Pool settings
const string poolId = "LinuxNodesSamplePoolDotNet";
const string vmSize = "STANDARD_A1";
const int nodeCount = 1;

// Obtain a collection of all available node agent SKUs.
// This allows us tooselect from a list of supported
// VM image/node agent combinations.
List<NodeAgentSku> nodeAgentSkus =
    batchClient.PoolOperations.ListNodeAgentSkus().ToList();

// Define a delegate specifying properties of hello VM image
// that we wish toouse.
Func<ImageReference, bool> isUbuntu1404 = imageRef =>
    imageRef.Publisher == "Canonical" &&
    imageRef.Offer == "UbuntuServer" &&
    imageRef.Sku.Contains("14.04");

// Obtain hello first node agent SKU in hello collection that matches
// Ubuntu Server 14.04. Note that there are one or more image
// references associated with this node agent SKU.
NodeAgentSku ubuntuAgentSku = nodeAgentSkus.First(sku =>
    sku.VerifiedImageReferences.Any(isUbuntu1404));

// Select an ImageReference from those available for node agent.
ImageReference imageReference =
    ubuntuAgentSku.VerifiedImageReferences.First(isUbuntu1404);

// Create hello VirtualMachineConfiguration for use when actually
// creating hello pool
VirtualMachineConfiguration virtualMachineConfiguration =
    new VirtualMachineConfiguration(imageReference, ubuntuAgentSku.Id);

// Create hello unbound pool object using hello VirtualMachineConfiguration
// created above
CloudPool pool = batchClient.PoolOperations.CreatePool(
    poolId: poolId,
    virtualMachineSize: vmSize,
    virtualMachineConfiguration: virtualMachineConfiguration,
    targetDedicatedComputeNodes: nodeCount);

// Commit hello pool toohello Batch service
await pool.CommitAsync();
```

Även om hello utdraget använder hello [PoolOperations][net_pool_ops].[ ListNodeAgentSkus] [ net_list_skus] metoden toodynamically lista och välj från stöds avbildningen och noden agent SKU kombinationer (rekommenderas), du kan också konfigurera en [ImageReference] [ net_imagereference] explicit:

```csharp
ImageReference imageReference = new ImageReference(
    publisher: "Canonical",
    offer: "UbuntuServer",
    sku: "14.04.2-LTS",
    version: "latest");
```

## <a name="list-of-virtual-machine-images"></a>Lista över avbildningar av virtuella datorer
hello visar följande tabell hello Marketplace avbildningar av virtuella datorer som är kompatibla med hello tillgängliga Batch nod agenter när den här artikeln senast uppdaterades. Det är viktigt toonote att den här listan inte är slutgiltig eftersom bilder och nod-agenter kan läggas till eller tas bort när som helst. Vi rekommenderar att dina Batch-program och tjänster alltid använder [list_node_agent_skus] [ py_list_skus] (Python) och [ListNodeAgentSkus] [ net_list_skus] Toodetermine (batch .NET) och välj bland hello tillgängliga SKU: er.

> [!WARNING]
> hello efter listan kan ändras när som helst. Använd alltid hello **lista nod agent SKU** metoder som är tillgängliga i hello Batch-API: er toolist hello kompatibla virtuella datorn och noden agent SKU: er när du kör Batch-jobb.
>
>

| **Publisher** | **Erbjudande** | **Bild SKU** | **Version** | **Noden agent SKU-ID** |
| ------------- | --------- | ------------- | ----------- | --------------------- |
| Canonical | UbuntuServer | 14.04.5-LTS | senaste | batch.node.Ubuntu 14.04 |
| Canonical | UbuntuServer | 16.04.0-LTS | senaste | batch.node.Ubuntu 16.04 |
| credativ | Debian | 8 | senaste | batch.node.debian 8 |
| OpenLogic | CentOS | 7.0 | senaste | batch.node.centos 7 |
| OpenLogic | CentOS | 7.1 | senaste | batch.node.centos 7 |
| OpenLogic | CentOS HPC | 7.1 | senaste | batch.node.centos 7 |
| OpenLogic | CentOS | 7.2 | senaste | batch.node.centos 7 |
| Oracle | Oracle Linux | 7.0 | senaste | batch.node.centos 7 |
| Oracle | Oracle Linux | 7.2 | senaste | batch.node.centos 7 |
| SUSE | openSUSE | 13.2 | senaste | batch.node.OpenSuSE 13.2 |
| SUSE | openSUSE Leap | 42.1 | senaste | batch.node.OpenSuSE 42.1 |
| SUSE | SLES | 12-SP1 | senaste | batch.node.OpenSuSE 42.1 |
| SUSE | SLES HPC | 12-SP1 | senaste | batch.node.OpenSuSE 42.1 |
| Microsoft-annonser | Linux-data-vetenskap-vm | linuxdsvm | senaste | batch.node.centos 7 |
| Microsoft-annonser | standard-data-vetenskap-vm | standard-data-vetenskap-vm | senaste | batch.node.Windows amd64 |
| MicrosoftWindowsServer | WindowsServer | 2008 R2 SP1 | senaste | batch.node.Windows amd64 |
| MicrosoftWindowsServer | WindowsServer | 2012-Datacenter | senaste | batch.node.Windows amd64 |
| MicrosoftWindowsServer | WindowsServer | 2012-R2-Datacenter | senaste | batch.node.Windows amd64 |
| MicrosoftWindowsServer | WindowsServer | 2016 Datacenter | senaste | batch.node.Windows amd64 |
| MicrosoftWindowsServer | WindowsServer | 2016 Datacenter med behållare | senaste | batch.node.Windows amd64 |

## <a name="connect-toolinux-nodes-using-ssh"></a>Ansluta tooLinux noder via SSH
Under utveckling eller vid felsökning kan det vara nödvändigt toosign i toohello noder i din pool. Till skillnad från Windows compute-noder kan du inte använda Remote Desktop Protocol (RDP) tooconnect tooLinux noder. Hello Batch-tjänsten kan i stället SSH-åtkomst på varje nod för fjärranslutning.

hello skapar följande kodavsnitt i Python en användare på varje nod i en programpool, vilket krävs för anslutning. Hello secure shell (SSH) anslutningsinformationen för varje nod skriver sedan ut.

```python
import datetime
import getpass
import azure.batch.batch_service_client as batch
import azure.batch.batch_auth as batchauth
import azure.batch.models as batchmodels

# Specify your own account credentials
batch_account_name = ''
batch_account_key = ''
batch_account_url = ''

# Specify hello ID of an existing pool containing Linux nodes
# currently in hello 'idle' state
pool_id = ''

# Specify hello username and prompt for a password
username = 'linuxuser'
password = getpass.getpass()

# Create a BatchClient
credentials = batchauth.SharedKeyCredentials(
    batch_account_name,
    batch_account_key
)
batch_client = batch.BatchServiceClient(
        credentials,
        base_url=batch_account_url
)

# Create hello user that will be added tooeach node in hello pool
user = batchmodels.ComputeNodeUser(username)
user.password = password
user.is_admin = True
user.expiry_time = \
    (datetime.datetime.today() + datetime.timedelta(days=30)).isoformat()

# Get hello list of nodes in hello pool
nodes = batch_client.compute_node.list(pool_id)

# Add hello user tooeach node in hello pool and print
# hello connection information for hello node
for node in nodes:
    # Add hello user toohello node
    batch_client.compute_node.add_user(pool_id, node.id, user)

    # Obtain SSH login information for hello node
    login = batch_client.compute_node.get_remote_login_settings(pool_id,
                                                                node.id)

    # Print hello connection info for hello node
    print("{0} | {1} | {2} | {3}".format(node.id,
                                         node.state,
                                         login.remote_login_ip_address,
                                         login.remote_login_port))
```

Här är exempel på utdata hello föregående kod för en pool som innehåller fyra Linux noder:

```
Password:
tvm-1219235766_1-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50000
tvm-1219235766_2-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50003
tvm-1219235766_3-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50002
tvm-1219235766_4-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50001
```

Du kan ange en offentlig SSH-nyckel när du skapar en användare på en nod i stället för ett lösenord. Använd hello i hello Python SDK, **ssh_public_key** parameter på [ComputeNodeUser][py_computenodeuser]. Använd hello i .NET, [ComputeNodeUser][net_computenodeuser].[ SshPublicKey] [ net_ssh_key] egenskapen.

## <a name="pricing"></a>Prissättning
Azure Batch bygger på Azure-molntjänster och virtuella datorer i Azure-teknik. hello själva Batch-tjänsten erbjuds utan kostnad, vilket betyder att debiteras du bara för hello beräkningsresurser som Batch-lösningar använder. När du väljer **Services Molnkonfigurationen**, debiteras du baserat på hello [molntjänster priser] [ cloud_services_pricing] struktur. När du väljer **konfiguration av virtuell dator**, debiteras du baserat på hello [virtuella datorer priser] [ vm_pricing] struktur. 

Om du distribuerar program tooyour Batch-noder som använder [programpaket](batch-application-packages.md), även debiteras du för hello Azure Storage-resurser att använda din programpaket. I allmänhet är hello Azure lagringskostnader minimal. 

## <a name="next-steps"></a>Nästa steg
### <a name="batch-python-tutorial"></a>Batch Python-självstudier
För en mer detaljerad vägledning om hur toowork med Batch med hjälp av Python, checka ut [Kom igång med hello Azure Batch Python klienten](batch-python-tutorial.md). Dess tillhörande [kodexemplet] [ github_samples_pyclient] innehåller en hjälpfunktion `get_vm_config_for_distro`, som visar ett annat tekniken tooobtain konfiguration av virtuell dator.

### <a name="batch-python-code-samples"></a>Batch Python-kodexempel
Hej [Python kodexempel] [ github_samples_py] i hello [azure-batch-samples] [ github_samples] databasen på GitHub innehåller skript som visar hur tooperform vanliga Batch-åtgärder, till exempel pool, jobb och skapa aktiviteter. Hej [viktigt] [ github_py_readme] som medföljer hello Python-exempel innehåller information om hur tooinstall hello krävs paket.

### <a name="batch-forum"></a>Batch-forum
Hej [Azure Batch-Forum] [ forum] är utmärkt placera toodiscuss Batch och ställa frågor om hello-tjänsten på MSDN. Läs användbara ”fasta” anslår och publicera dina frågor när de uppstår när du skapar Batch-lösningar.

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_mgmt]: https://msdn.microsoft.com/library/azure/mt463120.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[cloud_services_pricing]: https://azure.microsoft.com/pricing/details/cloud-services/
[forum]: https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=azurebatch
[github_py_readme]: https://github.com/Azure/azure-batch-samples/blob/master/Python/Batch/README.md
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_samples_py]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch
[github_samples_pyclient]: https://github.com/Azure/azure-batch-samples/blob/master/Python/Batch/article_samples/python_tutorial_client.py
[portal]: https://portal.azure.com
[net_cloudpool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_computenodeuser]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenodeuser.aspx
[net_imagereference]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.imagereference.aspx
[net_list_skus]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listnodeagentskus.aspx
[net_pool_ops]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.aspx
[net_ssh_key]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenodeuser.sshpublickey.aspx
[nuget_batch_net]: https://www.nuget.org/packages/Azure.Batch/
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[py_account_ops]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.operations.html#azure.batch.operations.AccountOperations
[py_azure_sdk]: https://pypi.python.org/pypi/azure
[py_batch_docs]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.html
[py_batch_package]: https://pypi.python.org/pypi/azure-batch
[py_computenodeuser]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.models.html#azure.batch.models.ComputeNodeUser
[py_imagereference]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.models.html#azure.batch.models.ImageReference
[py_list_skus]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.operations.html#azure.batch.operations.AccountOperations.list_node_agent_skus
[vm_marketplace]: https://azure.microsoft.com/marketplace/virtual-machines/
[vm_pricing]: https://azure.microsoft.com/pricing/details/virtual-machines/
