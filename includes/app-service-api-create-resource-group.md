<span data-ttu-id="b9147-101">Skapa en resursgrupp med kommandot [az group create](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="b9147-101">Create a resource group with the [az group create](/cli/azure/group#create) command.</span></span>

[!INCLUDE [resource group intro text](resource-group.md)]

<span data-ttu-id="b9147-102">I följande exempel skapas en resursgrupp med namnet *myResourceGroup* på platsen *westeurope*.</span><span class="sxs-lookup"><span data-stu-id="b9147-102">The following example creates a resource group named *myResourceGroup* in the *westeurope* location.</span></span>

```azurecli-interactive
az group create --name myResourceGroup --location westeurope
```

<span data-ttu-id="b9147-103">Om du vill se tillgängliga platser kör du kommandot `az appservice list-locations`.</span><span class="sxs-lookup"><span data-stu-id="b9147-103">To see the available locations, run the `az appservice list-locations` command.</span></span> <span data-ttu-id="b9147-104">Du skapar vanligtvis resurser i en region nära dig.</span><span class="sxs-lookup"><span data-stu-id="b9147-104">You generally create resources in a region near you.</span></span>
