<span data-ttu-id="82e92-101">Skapa en resursgrupp med hello [az gruppen skapa](/cli/azure/group#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="82e92-101">Create a resource group with hello [az group create](/cli/azure/group#create) command.</span></span>

[!INCLUDE [resource group intro text](resource-group.md)]

<span data-ttu-id="82e92-102">hello följande exempel skapar en resursgrupp med namnet *myResourceGroup* i hello *westeurope* plats.</span><span class="sxs-lookup"><span data-stu-id="82e92-102">hello following example creates a resource group named *myResourceGroup* in hello *westeurope* location.</span></span>

```azurecli-interactive
az group create --name myResourceGroup --location westeurope
```

<span data-ttu-id="82e92-103">toosee hello tillgängliga platser, kör hello `az appservice list-locations` kommando.</span><span class="sxs-lookup"><span data-stu-id="82e92-103">toosee hello available locations, run hello `az appservice list-locations` command.</span></span> <span data-ttu-id="82e92-104">Du skapar vanligtvis resurser i en region nära dig.</span><span class="sxs-lookup"><span data-stu-id="82e92-104">You generally create resources in a region near you.</span></span>
