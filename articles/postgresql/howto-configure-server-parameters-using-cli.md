---
title: "aaaConfigure hello tjänstparametrar i Azure-databas för PostgreSQL | Microsoft Docs"
description: "Den här artikeln beskriver hur tooconfigure hello tjänstparametrar i Azure-databas för att använda PostgreSQL hello Azure CLI-kommandoraden."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: article
ms.date: 06/13/2017
ms.openlocfilehash: 84a11de24ba87fc0eb6744aaa4b53f65a183903d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="customize-server-configuration-parameters-using-azure-cli"></a><span data-ttu-id="54fb7-103">Anpassa konfigurationsparametrar för servern med hjälp av Azure CLI</span><span class="sxs-lookup"><span data-stu-id="54fb7-103">Customize server configuration parameters using Azure CLI</span></span>
<span data-ttu-id="54fb7-104">Du kan visa, visa och uppdatera konfigurationsparametrar för ett Azure PostgreSQL-server med hello kommandoradsgränssnittet (Azure CLI).</span><span class="sxs-lookup"><span data-stu-id="54fb7-104">You can list, show and update configuration parameters for an Azure PostgreSQL server using hello Command Line Interface (Azure CLI).</span></span> <span data-ttu-id="54fb7-105">Endast en delmängd av motorkonfigurationer är tillgängliga på servernivå och kan ändras.</span><span class="sxs-lookup"><span data-stu-id="54fb7-105">However, only a subset of engine configurations are exposed at server-level and can be modified.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="54fb7-106">Krav</span><span class="sxs-lookup"><span data-stu-id="54fb7-106">Prerequisites</span></span>
<span data-ttu-id="54fb7-107">toostep via den här hur tooguide behöver du:</span><span class="sxs-lookup"><span data-stu-id="54fb7-107">toostep through this how-tooguide, you need:</span></span>
- <span data-ttu-id="54fb7-108">En server och databas [skapa en Azure-databas för PostgreSQL](quickstart-create-server-database-azure-cli.md)</span><span class="sxs-lookup"><span data-stu-id="54fb7-108">A server and database [Create an Azure Database for PostgreSQL](quickstart-create-server-database-azure-cli.md)</span></span>
- <span data-ttu-id="54fb7-109">Installera [Azure CLI 2.0](/cli/azure/install-azure-cli) kommandoraden verktyg eller Använd hello Azure Cloud Shell i hello webbläsare.</span><span class="sxs-lookup"><span data-stu-id="54fb7-109">Install [Azure CLI 2.0](/cli/azure/install-azure-cli) command line utility or use hello Azure Cloud Shell in hello browser.</span></span>

## <a name="list-server-configuration-parameters-for-azure-database-for-postgresql-server"></a><span data-ttu-id="54fb7-110">Lista server konfigurationsparametrar för Azure-databas för PostgreSQL-server</span><span class="sxs-lookup"><span data-stu-id="54fb7-110">List server configuration parameters for Azure Database for PostgreSQL server</span></span>
<span data-ttu-id="54fb7-111">toolist alla ändringsbar parametrar i en server och deras värden kör hello [az postgres server konfigurationslistan](/cli/azure/postgres/server/configuration#list) kommando.</span><span class="sxs-lookup"><span data-stu-id="54fb7-111">toolist all modifiable parameters in a server and their values, run hello [az postgres server configuration list](/cli/azure/postgres/server/configuration#list) command.</span></span>

<span data-ttu-id="54fb7-112">Du kan ange hello server konfigurationsparametrar för hello server **mypgserver 20170401.postgres.database.azure.com** under resursgrupp **myresourcegroup**.</span><span class="sxs-lookup"><span data-stu-id="54fb7-112">You can list hello server configuration parameters for hello server **mypgserver-20170401.postgres.database.azure.com** under resource group **myresourcegroup**.</span></span>
```azurecli-interactive
az postgres server configuration list --resource-group myresourcegroup --server mypgserver-20170401
```
## <a name="show-server-configuration-parameter-details"></a><span data-ttu-id="54fb7-113">Visa serverkonfiguration parameterinformation</span><span class="sxs-lookup"><span data-stu-id="54fb7-113">Show server configuration parameter details</span></span>
<span data-ttu-id="54fb7-114">tooshow information om en specifik konfigurationsparameter för en server som kör hello [az postgres server configuration visa](/cli/azure/postgres/server/configuration#show) kommando.</span><span class="sxs-lookup"><span data-stu-id="54fb7-114">tooshow details about a particular configuration parameter for a server, run hello [az postgres server configuration show](/cli/azure/postgres/server/configuration#show)  command.</span></span>

<span data-ttu-id="54fb7-115">Det här exemplet visas information om hello **loggen\_min\_meddelanden** server konfigurationsparameter för server **mypgserver 20170401.postgres.database.azure.com** under resursgruppen **myresourcegroup.**</span><span class="sxs-lookup"><span data-stu-id="54fb7-115">This example shows details of hello **log\_min\_messages** server configuration parameter for server **mypgserver-20170401.postgres.database.azure.com** under resource group **myresourcegroup.**</span></span>
```azurecli-interactive
az postgres server configuration show --name log_min_messages --resource-group myresourcegroup --server mypgserver-20170401
```
## <a name="modify-server-configuration-parameter-value"></a><span data-ttu-id="54fb7-116">Ändra parametervärdet för server-konfiguration</span><span class="sxs-lookup"><span data-stu-id="54fb7-116">Modify server configuration parameter value</span></span>
<span data-ttu-id="54fb7-117">Du kan också ändra hello-värdet för en viss server konfigurationsparameter och hello underliggande Konfigurationsvärdet för hello PostgreSQL server engine uppdateras.</span><span class="sxs-lookup"><span data-stu-id="54fb7-117">You can also modify hello value of a certain server configuration parameter, and this updates hello underlying configuration value for hello PostgreSQL server engine.</span></span> <span data-ttu-id="54fb7-118">tooupdate hello configuration Använd hello [az postgres server configuration set](/cli/azure/postgres/server/configuration#set) kommando.</span><span class="sxs-lookup"><span data-stu-id="54fb7-118">tooupdate hello configuration use hello [az postgres server configuration set](/cli/azure/postgres/server/configuration#set) command.</span></span> 

<span data-ttu-id="54fb7-119">tooupdate hello **loggen\_min\_meddelanden** server konfigurationsparameter Server **mypgserver 20170401.postgres.database.azure.com** under resursgrupp **myresourcegroup.**</span><span class="sxs-lookup"><span data-stu-id="54fb7-119">tooupdate hello **log\_min\_messages** server configuration parameter of server **mypgserver-20170401.postgres.database.azure.com** under resource group **myresourcegroup.**</span></span>
```azurecli-interactive
az postgres server configuration set --name log_min_messages --resource-group myresourcegroup --server mypgserver-20170401 --value INFO
```
<span data-ttu-id="54fb7-120">Om du vill tooreset hello värdet för en konfigurationsparameter du bara välja tooleave ut hello valfria `--value` parameter och hello tjänsten gäller hello standardvärdet.</span><span class="sxs-lookup"><span data-stu-id="54fb7-120">If you want tooreset hello value of a configuration parameter, you simply choose tooleave out hello optional `--value` parameter, and hello service will apply hello default value.</span></span> <span data-ttu-id="54fb7-121">I ovanstående exempel ser det ut som:</span><span class="sxs-lookup"><span data-stu-id="54fb7-121">In above example, it would look like:</span></span>
```azurecli-interactive
az postgres server configuration set --name log_min_messages --resource-group myresourcegroup --server mypgserver-20170401
```
<span data-ttu-id="54fb7-122">Det här återställer hello **loggen\_min\_meddelanden** configuration toohello standardvärdet **varning**.</span><span class="sxs-lookup"><span data-stu-id="54fb7-122">This will reset hello **log\_min\_messages** configuration toohello default value **WARNING**.</span></span> <span data-ttu-id="54fb7-123">Mer information om konfiguration och tillåtna värden finns i dokumentationen för PostgreSQL på [serverkonfiguration](https://www.postgresql.org/docs/9.6/static/runtime-config.html).</span><span class="sxs-lookup"><span data-stu-id="54fb7-123">For further details on server configuration and permissible values, see PostgreSQL documentation on [Server Configuration](https://www.postgresql.org/docs/9.6/static/runtime-config.html).</span></span>

## <a name="next-steps"></a><span data-ttu-id="54fb7-124">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="54fb7-124">Next steps</span></span>
- <span data-ttu-id="54fb7-125">tooconfigure och åtkomst server-loggar finns [Server-loggar i Azure-databas för PostgreSQL](concepts-server-logs.md)</span><span class="sxs-lookup"><span data-stu-id="54fb7-125">tooconfigure and access server logs, see [Server Logs in Azure Database for PostgreSQL](concepts-server-logs.md)</span></span>
