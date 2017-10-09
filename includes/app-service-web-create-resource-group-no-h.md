<span data-ttu-id="c0cad-101">Skapa en resursgrupp med hello [az gruppen skapa](/cli/azure/group#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="c0cad-101">Create a resource group with hello [az group create](/cli/azure/group#create) command.</span></span>

[!INCLUDE [resource group intro text](resource-group.md)]

<span data-ttu-id="c0cad-102">hello följande exempel skapar en resursgrupp med namnet *myResourceGroup* i hello *westeurope* plats.</span><span class="sxs-lookup"><span data-stu-id="c0cad-102">hello following example creates a resource group named *myResourceGroup* in hello *westeurope* location.</span></span>

```azurecli-interactive
az group create --name myResourceGroup --location westeurope
```

<span data-ttu-id="c0cad-103">Du vanligtvis skapa resurs grupp och hello resurser i en region nära dig.</span><span class="sxs-lookup"><span data-stu-id="c0cad-103">You generally create your resource group and hello resources in a region near you.</span></span> <span data-ttu-id="c0cad-104">toosee alla stöds platser för Azure Web Apps kör hello `az appservice list-locations` kommando.</span><span class="sxs-lookup"><span data-stu-id="c0cad-104">toosee all supported locations for Azure Web Apps, run hello `az appservice list-locations` command.</span></span> 
