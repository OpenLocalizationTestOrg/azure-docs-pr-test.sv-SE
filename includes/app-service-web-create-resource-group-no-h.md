<span data-ttu-id="4b09b-101">Skapa en resursgrupp med kommandot [az group create](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="4b09b-101">Create a resource group with the [az group create](/cli/azure/group#create) command.</span></span>

[!INCLUDE [resource group intro text](resource-group.md)]

<span data-ttu-id="4b09b-102">I följande exempel skapas en resursgrupp med namnet *myResourceGroup* på platsen *westeurope*.</span><span class="sxs-lookup"><span data-stu-id="4b09b-102">The following example creates a resource group named *myResourceGroup* in the *westeurope* location.</span></span>

```azurecli-interactive
az group create --name myResourceGroup --location westeurope
```

<span data-ttu-id="4b09b-103">Du skapar vanligtvis din resursgrupp och resurserna i en region nära dig.</span><span class="sxs-lookup"><span data-stu-id="4b09b-103">You generally create your resource group and the resources in a region near you.</span></span> <span data-ttu-id="4b09b-104">Om du vill se alla platser som stöds för Azure Web Apps kör du kommandot `az appservice list-locations`.</span><span class="sxs-lookup"><span data-stu-id="4b09b-104">To see all supported locations for Azure Web Apps, run the `az appservice list-locations` command.</span></span> 