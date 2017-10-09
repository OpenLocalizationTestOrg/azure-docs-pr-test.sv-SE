<span data-ttu-id="deccb-101">tooconnect tooan Azure Redis-Cache-instans, cacheklienter måste hello värdnamnet, portarna och nycklarna hello-cache.</span><span class="sxs-lookup"><span data-stu-id="deccb-101">tooconnect tooan Azure Redis Cache instance, cache clients need hello host name, ports, and keys of hello cache.</span></span> <span data-ttu-id="deccb-102">Vissa klienter kan referera toothese objekten med något annorlunda namn.</span><span class="sxs-lookup"><span data-stu-id="deccb-102">Some clients may refer toothese items by slightly different names.</span></span> <span data-ttu-id="deccb-103">Du kan hämta den här informationen i hello Azure-portalen eller med hjälp av kommandoradsverktyg som Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="deccb-103">You can retrieve this information in hello Azure portal or by using command-line tools such as Azure CLI.</span></span>

### <a name="retrieve-host-name-ports-and-access-keys-using-hello-azure-portal"></a><span data-ttu-id="deccb-104">Hämta värdnamnet, portarna och snabbtangenter med hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="deccb-104">Retrieve host name, ports, and access keys using hello Azure Portal</span></span>
<span data-ttu-id="deccb-105">tooretrieve värddatornamn, portar och snabbtangenter med hello Azure-portalen [Bläddra](../articles/redis-cache/cache-configure.md#configure-redis-cache-settings) tooyour cache i hello [Azure-portalen](https://portal.azure.com) och på **åtkomstnycklar** och  **Egenskaper för** i hello **resurs menyn**.</span><span class="sxs-lookup"><span data-stu-id="deccb-105">tooretrieve host name, ports, and access keys using hello Azure Portal, [browse](../articles/redis-cache/cache-configure.md#configure-redis-cache-settings) tooyour cache in hello [Azure portal](https://portal.azure.com) and click **Access keys** and **Properties** in hello **Resource menu**.</span></span> 

![Inställningar för Redis-cache](media/redis-cache-access-keys/redis-cache-hostname-ports-keys.png)

### <a name="retrieve-host-name-ports-and-access-keys-using-azure-cli"></a><span data-ttu-id="deccb-107">Hämta värdnamn, portar och åtkomstnycklar via kommandoradsgränssnittet för Azure</span><span class="sxs-lookup"><span data-stu-id="deccb-107">Retrieve host name, ports, and access keys using Azure CLI</span></span>
<span data-ttu-id="deccb-108">tooretrieve hello-värdnamn och portar med hjälp av Azure CLI 2.0 kan du anropa [az redis visa](https://docs.microsoft.com/cli/azure/redis#show), och tooretrieve hello nycklar kan du anropa [az redis lista nycklar](https://docs.microsoft.com/cli/azure/redis#list-keys).</span><span class="sxs-lookup"><span data-stu-id="deccb-108">tooretrieve hello host name and ports using Azure CLI 2.0 you can call [az redis show](https://docs.microsoft.com/cli/azure/redis#show), and tooretrieve hello keys you can call [az redis list-keys](https://docs.microsoft.com/cli/azure/redis#list-keys).</span></span> <span data-ttu-id="deccb-109">hello följande skript anropar dessa två kommandon och ta bort eko hello värdnamn och portar och nycklar toohello-konsolen.</span><span class="sxs-lookup"><span data-stu-id="deccb-109">hello following script calls these two commands and echos hello hostname, ports, and keys toohello console.</span></span>

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

<span data-ttu-id="deccb-110">Mer information om det här skriptet finns [hämta hello värdnamnet, portarna och nycklarna för Azure Redis-Cache](../articles/redis-cache/scripts/cache-keys-ports.md).</span><span class="sxs-lookup"><span data-stu-id="deccb-110">For more information about this script, see [Get hello hostname, ports, and keys for Azure Redis Cache](../articles/redis-cache/scripts/cache-keys-ports.md).</span></span> <span data-ttu-id="deccb-111">Mer information om Azure CLI 2.0 finns i [Install Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) (Installera Azure CLI 2.0) och [Get started with Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) (Kom igång med Azure CLI 2.0).</span><span class="sxs-lookup"><span data-stu-id="deccb-111">For more information on Azure CLI 2.0, see [Install Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) and [Get started with Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli).</span></span>
