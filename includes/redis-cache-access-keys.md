tooconnect tooan Azure Redis-Cache-instans, cacheklienter måste hello värdnamnet, portarna och nycklarna hello-cache. Vissa klienter kan referera toothese objekten med något annorlunda namn. Du kan hämta den här informationen i hello Azure-portalen eller med hjälp av kommandoradsverktyg som Azure CLI.

### <a name="retrieve-host-name-ports-and-access-keys-using-hello-azure-portal"></a>Hämta värdnamnet, portarna och snabbtangenter med hello Azure-portalen
tooretrieve värddatornamn, portar och snabbtangenter med hello Azure-portalen [Bläddra](../articles/redis-cache/cache-configure.md#configure-redis-cache-settings) tooyour cache i hello [Azure-portalen](https://portal.azure.com) och på **åtkomstnycklar** och  **Egenskaper för** i hello **resurs menyn**. 

![Inställningar för Redis-cache](media/redis-cache-access-keys/redis-cache-hostname-ports-keys.png)

### <a name="retrieve-host-name-ports-and-access-keys-using-azure-cli"></a>Hämta värdnamn, portar och åtkomstnycklar via kommandoradsgränssnittet för Azure
tooretrieve hello-värdnamn och portar med hjälp av Azure CLI 2.0 kan du anropa [az redis visa](https://docs.microsoft.com/cli/azure/redis#show), och tooretrieve hello nycklar kan du anropa [az redis lista nycklar](https://docs.microsoft.com/cli/azure/redis#list-keys). hello följande skript anropar dessa två kommandon och ta bort eko hello värdnamn och portar och nycklar toohello-konsolen.

```azurecli
#/bin/bash

# Retrieve hello hostname, ports, and keys for contosoCache located in contosoGroup

# Retrieve hello hostname and ports for an Azure Redis Cache instance
redis=($(az redis show --name contosoCache --resource-group contosoGroup --query [hostName,enableNonSslPort,port,sslPort] --output tsv))

# Retrieve hello keys for an Azure Redis Cache instance
keys=($(az redis list-keys --name contosoCache --resource-group contosoGroup --query [primaryKey,secondaryKey] --output tsv))

# Display hello retrieved hostname, keys, and ports
echo "Hostname:" ${redis[0]}
echo "Non SSL Port:" ${redis[2]}
echo "Non SSL Port Enabled:" ${redis[1]}
echo "SSL Port:" ${redis[3]}
echo "Primary Key:" ${keys[0]}
echo "Secondary Key:" ${keys[1]}
```

Mer information om det här skriptet finns [hämta hello värdnamnet, portarna och nycklarna för Azure Redis-Cache](../articles/redis-cache/scripts/cache-keys-ports.md). Mer information om Azure CLI 2.0 finns i [Install Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) (Installera Azure CLI 2.0) och [Get started with Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) (Kom igång med Azure CLI 2.0).
