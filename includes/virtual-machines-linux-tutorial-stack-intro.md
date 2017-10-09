## <a name="create-a-resource-group"></a>Skapa en resursgrupp

Skapa en resursgrupp med hello [az gruppen skapa](/cli/azure/group#create) kommando. En Azure-resursgrupp är en logisk behållare där Azure-resurser distribueras och hanteras. 

hello följande exempel skapar en resursgrupp med namnet *myResourceGroup* i hello *eastus* plats.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-virtual-machine"></a>Skapa en virtuell dator

Skapa en virtuell dator med hello [az vm skapa](/cli/azure/vm#create) kommando. 

hello följande exempel skapas en virtuell dator med namnet *myVM* och skapar SSH-nycklar, om de inte redan finns på standardplatsen nyckel. toouse för en specifik uppsättning nycklar använder hello `--ssh-key-value` alternativet.  

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys
```

När du har skapat hello VM visar hello Azure CLI information liknande toohello följande exempel. Anteckna hello `publicIpAddress`. Den här adressen är används tooaccess hello VM.

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



## <a name="open-port-80-for-web-traffic"></a>Öppna port 80 för webbtrafik 

Som standard tillåts endast SSH-anslutningar till Linux virtuella datorer som distribueras i Azure. Eftersom den här virtuella datorn försätts i toobe en webbserver, måste tooopen port 80 från hello internet. Använd hello [az vm öppna port](/cli/azure/vm#open-port) kommandot tooopen hello önskad port.  
 
```azurecli-interactive 
az vm open-port --port 80 --resource-group myResourceGroup --name myVM
```
## <a name="ssh-into-your-vm"></a>SSH till den virtuella datorn


Om du redan vet Hej offentliga IP-adressen för den virtuella datorn, kör hello [az offentliga ip-lista över](/cli/azure/network/public-ip#list) kommando:


```azurecli-interactive
az network public-ip list --resource-group myResourceGroup --query [].ipAddress
```

Använd hello följande kommando toocreate en SSH-session med hello virtuell dator. Ersätt hello rätt offentliga IP-adressen för den virtuella datorn. I det här exemplet hello IP-adressen är *40.68.254.142*.

```bash
ssh azureuser@40.68.254.142
```

