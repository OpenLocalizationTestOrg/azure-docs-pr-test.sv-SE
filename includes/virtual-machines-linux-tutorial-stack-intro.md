## <a name="create-a-resource-group"></a><span data-ttu-id="d6610-101">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="d6610-101">Create a resource group</span></span>

<span data-ttu-id="d6610-102">Skapa en resursgrupp med kommandot [az group create](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="d6610-102">Create a resource group with the [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="d6610-103">En Azure-resursgrupp är en logisk behållare där Azure-resurser distribueras och hanteras.</span><span class="sxs-lookup"><span data-stu-id="d6610-103">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span> 

<span data-ttu-id="d6610-104">I följande exempel skapas en resursgrupp med namnet *myResourceGroup* på platsen *eastus*.</span><span class="sxs-lookup"><span data-stu-id="d6610-104">The following example creates a resource group named *myResourceGroup* in the *eastus* location.</span></span>

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-virtual-machine"></a><span data-ttu-id="d6610-105">Skapa en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="d6610-105">Create a virtual machine</span></span>

<span data-ttu-id="d6610-106">Skapa en virtuell dator med kommandot [az vm create](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="d6610-106">Create a VM with the [az vm create](/cli/azure/vm#create) command.</span></span> 

<span data-ttu-id="d6610-107">Följande exempel skapar en virtuell dator som heter *myVM*, och SSH-nycklar skapas om de inte redan finns på en standardnyckelplats.</span><span class="sxs-lookup"><span data-stu-id="d6610-107">The following example creates a VM named *myVM* and creates SSH keys if they do not already exist in a default key location.</span></span> <span data-ttu-id="d6610-108">Om du vill använda en specifik uppsättning nycklar använder du alternativet `--ssh-key-value`.</span><span class="sxs-lookup"><span data-stu-id="d6610-108">To use a specific set of keys, use the `--ssh-key-value` option.</span></span>  

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys
```

<span data-ttu-id="d6610-109">När den virtuella datorn har skapats visar Azure CLI information som ser ut ungefär som i följande exempel.</span><span class="sxs-lookup"><span data-stu-id="d6610-109">When the VM has been created, the Azure CLI shows information similar to the following example.</span></span> <span data-ttu-id="d6610-110">Anteckna `publicIpAddress`.</span><span class="sxs-lookup"><span data-stu-id="d6610-110">Take note of the `publicIpAddress`.</span></span> <span data-ttu-id="d6610-111">Den här adressen används för att få åtkomst till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="d6610-111">This address is used to access the VM.</span></span>

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



## <a name="open-port-80-for-web-traffic"></a><span data-ttu-id="d6610-112">Öppna port 80 för webbtrafik</span><span class="sxs-lookup"><span data-stu-id="d6610-112">Open port 80 for web traffic</span></span> 

<span data-ttu-id="d6610-113">Som standard tillåts endast SSH-anslutningar till Linux virtuella datorer som distribueras i Azure.</span><span class="sxs-lookup"><span data-stu-id="d6610-113">By default, only SSH connections are allowed into Linux VMs deployed in Azure.</span></span> <span data-ttu-id="d6610-114">Eftersom den här virtuella datorn kommer att vara en webbserver, måste du öppna port 80 från internet.</span><span class="sxs-lookup"><span data-stu-id="d6610-114">Because this VM is going to be a web server, you need to open port 80 from the internet.</span></span> <span data-ttu-id="d6610-115">Använd kommandot [az vm open-port](/cli/azure/vm#open-port) för att öppna önskad port.</span><span class="sxs-lookup"><span data-stu-id="d6610-115">Use the [az vm open-port](/cli/azure/vm#open-port) command to open the desired port.</span></span>  
 
```azurecli-interactive 
az vm open-port --port 80 --resource-group myResourceGroup --name myVM
```
## <a name="ssh-into-your-vm"></a><span data-ttu-id="d6610-116">SSH till den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="d6610-116">SSH into your VM</span></span>


<span data-ttu-id="d6610-117">Om du inte redan vet offentliga IP-adressen för den virtuella datorn kan köra den [az offentliga ip-lista över](/cli/azure/network/public-ip#list) kommando:</span><span class="sxs-lookup"><span data-stu-id="d6610-117">If you don't already know the public IP address of your VM, run the [az network public-ip list](/cli/azure/network/public-ip#list) command:</span></span>


```azurecli-interactive
az network public-ip list --resource-group myResourceGroup --query [].ipAddress
```

<span data-ttu-id="d6610-118">Använd följande kommando för att skapa en SSH-session med den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="d6610-118">Use the following command to create an SSH session with the virtual machine.</span></span> <span data-ttu-id="d6610-119">Ersätt rätt offentliga IP-adressen för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="d6610-119">Substitute the correct public IP address of your virtual machine.</span></span> <span data-ttu-id="d6610-120">I det här exemplet IP-adressen är *40.68.254.142*.</span><span class="sxs-lookup"><span data-stu-id="d6610-120">In this example, the IP address is *40.68.254.142*.</span></span>

```bash
ssh azureuser@40.68.254.142
```

