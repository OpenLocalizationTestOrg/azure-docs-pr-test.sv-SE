Skapa en resursgrupp med hello [az gruppen skapa](/cli/azure/group#create) kommando.

[!INCLUDE [resource group intro text](resource-group.md)]

hello följande exempel skapar en resursgrupp med namnet *myResourceGroup* i hello *westeurope* plats.

```azurecli-interactive
az group create --name myResourceGroup --location westeurope
```

toosee hello tillgängliga platser, kör hello `az appservice list-locations` kommando. Du skapar vanligtvis resurser i en region nära dig.
