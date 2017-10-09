## <a name="create-a-resource-group"></a><span data-ttu-id="619a4-101">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="619a4-101">Create a resource group</span></span>

<span data-ttu-id="619a4-102">Skapa en resursgrupp med hello [az gruppen skapa](/cli/azure/group#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="619a4-102">Create a resource group with hello [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="619a4-103">En Azure-resursgrupp är en logisk behållare där Azure-resurser distribueras och hanteras.</span><span class="sxs-lookup"><span data-stu-id="619a4-103">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span> 

<span data-ttu-id="619a4-104">hello följande exempel skapar en resursgrupp med namnet *myResourceGroup* i hello *eastus* plats.</span><span class="sxs-lookup"><span data-stu-id="619a4-104">hello following example creates a resource group named *myResourceGroup* in hello *eastus* location.</span></span>

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-virtual-machine"></a><span data-ttu-id="619a4-105">Skapa en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="619a4-105">Create a virtual machine</span></span>

<span data-ttu-id="619a4-106">Skapa en virtuell dator med hello [az vm skapa](/cli/azure/vm#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="619a4-106">Create a VM with hello [az vm create](/cli/azure/vm#create) command.</span></span> 

<span data-ttu-id="619a4-107">hello följande exempel skapas en virtuell dator med namnet *myVM* och skapar SSH-nycklar, om de inte redan finns på standardplatsen nyckel.</span><span class="sxs-lookup"><span data-stu-id="619a4-107">hello following example creates a VM named *myVM* and creates SSH keys if they do not already exist in a default key location.</span></span> <span data-ttu-id="619a4-108">toouse för en specifik uppsättning nycklar använder hello `--ssh-key-value` alternativet.</span><span class="sxs-lookup"><span data-stu-id="619a4-108">toouse a specific set of keys, use hello `--ssh-key-value` option.</span></span>  

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys
```

<span data-ttu-id="619a4-109">När du har skapat hello VM visar hello Azure CLI information liknande toohello följande exempel.</span><span class="sxs-lookup"><span data-stu-id="619a4-109">When hello VM has been created, hello Azure CLI shows information similar toohello following example.</span></span> <span data-ttu-id="619a4-110">Anteckna hello `publicIpAddress`.</span><span class="sxs-lookup"><span data-stu-id="619a4-110">Take note of hello `publicIpAddress`.</span></span> <span data-ttu-id="619a4-111">Den här adressen är används tooaccess hello VM.</span><span class="sxs-lookup"><span data-stu-id="619a4-111">This address is used tooaccess hello VM.</span></span>

```azurecli-interactive 
{
  "fqdns": "",
  "id": "/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "40.68.254.142",
  "resourceGroup": "myResourceGroup"
}
```



## <a name="open-port-80-for-web-traffic"></a><span data-ttu-id="619a4-112">Öppna port 80 för webbtrafik</span><span class="sxs-lookup"><span data-stu-id="619a4-112">Open port 80 for web traffic</span></span> 

<span data-ttu-id="619a4-113">Som standard tillåts endast SSH-anslutningar till Linux virtuella datorer som distribueras i Azure.</span><span class="sxs-lookup"><span data-stu-id="619a4-113">By default, only SSH connections are allowed into Linux VMs deployed in Azure.</span></span> <span data-ttu-id="619a4-114">Eftersom den här virtuella datorn försätts i toobe en webbserver, måste tooopen port 80 från hello internet.</span><span class="sxs-lookup"><span data-stu-id="619a4-114">Because this VM is going toobe a web server, you need tooopen port 80 from hello internet.</span></span> <span data-ttu-id="619a4-115">Använd hello [az vm öppna port](/cli/azure/vm#open-port) kommandot tooopen hello önskad port.</span><span class="sxs-lookup"><span data-stu-id="619a4-115">Use hello [az vm open-port](/cli/azure/vm#open-port) command tooopen hello desired port.</span></span>  
 
```azurecli-interactive 
az vm open-port --port 80 --resource-group myResourceGroup --name myVM
```
## <a name="ssh-into-your-vm"></a><span data-ttu-id="619a4-116">SSH till den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="619a4-116">SSH into your VM</span></span>


<span data-ttu-id="619a4-117">Om du redan vet Hej offentliga IP-adressen för den virtuella datorn, kör hello [az offentliga ip-lista över](/cli/azure/network/public-ip#list) kommando:</span><span class="sxs-lookup"><span data-stu-id="619a4-117">If you don't already know hello public IP address of your VM, run hello [az network public-ip list](/cli/azure/network/public-ip#list) command:</span></span>


```azurecli-interactive
az network public-ip list --resource-group myResourceGroup --query [].ipAddress
```

<span data-ttu-id="619a4-118">Använd hello följande kommando toocreate en SSH-session med hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="619a4-118">Use hello following command toocreate an SSH session with hello virtual machine.</span></span> <span data-ttu-id="619a4-119">Ersätt hello rätt offentliga IP-adressen för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="619a4-119">Substitute hello correct public IP address of your virtual machine.</span></span> <span data-ttu-id="619a4-120">I det här exemplet hello IP-adressen är *40.68.254.142*.</span><span class="sxs-lookup"><span data-stu-id="619a4-120">In this example, hello IP address is *40.68.254.142*.</span></span>

```bash
ssh azureuser@40.68.254.142
```

