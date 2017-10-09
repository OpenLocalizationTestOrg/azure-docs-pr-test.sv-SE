Skapa en resursgrupp med hello [az gruppen skapa](/cli/azure/group#create) kommando.

[!INCLUDE [resource group intro text](resource-group.md)]

hello följande exempel skapar en resursgrupp med namnet *myResourceGroup* i hello *westeurope* plats.

```azurecli-interactive
az group create --name myResourceGroup --location westeurope
```

Du vanligtvis skapa resurs grupp och hello resurser i en region nära dig. toosee alla stöds platser för Azure Web Apps kör hello `az appservice list-locations` kommando. 
