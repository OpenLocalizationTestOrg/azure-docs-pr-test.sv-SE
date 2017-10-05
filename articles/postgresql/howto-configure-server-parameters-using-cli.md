---
title: "Konfigurera parametrar för tjänsten i Azure-databas för PostgreSQL | Microsoft Docs"
description: "Den här artikeln beskriver hur du konfigurerar parametrar för tjänsten i Azure-databas för PostgreSQL med hjälp av Azure CLI-kommandoraden."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: article
ms.date: 06/13/2017
ms.openlocfilehash: c8a3b5a0225c2cede180d8d57681f2e1a6c6cc3a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="customize-server-configuration-parameters-using-azure-cli"></a><span data-ttu-id="d7f86-103">Anpassa konfigurationsparametrar för servern med hjälp av Azure CLI</span><span class="sxs-lookup"><span data-stu-id="d7f86-103">Customize server configuration parameters using Azure CLI</span></span>
<span data-ttu-id="d7f86-104">Du kan visa, visa och uppdatera konfigurationsparametrar för en Azure PostgreSQL-server via kommandoradsgränssnittet (Azure CLI).</span><span class="sxs-lookup"><span data-stu-id="d7f86-104">You can list, show and update configuration parameters for an Azure PostgreSQL server using the Command Line Interface (Azure CLI).</span></span> <span data-ttu-id="d7f86-105">Endast en delmängd av motorkonfigurationer är tillgängliga på servernivå och kan ändras.</span><span class="sxs-lookup"><span data-stu-id="d7f86-105">However, only a subset of engine configurations are exposed at server-level and can be modified.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="d7f86-106">Krav</span><span class="sxs-lookup"><span data-stu-id="d7f86-106">Prerequisites</span></span>
<span data-ttu-id="d7f86-107">Du behöver följande för att gå igenom den här instruktioner:</span><span class="sxs-lookup"><span data-stu-id="d7f86-107">To step through this how-to guide, you need:</span></span>
- <span data-ttu-id="d7f86-108">En server och databas [skapa en Azure-databas för PostgreSQL](quickstart-create-server-database-azure-cli.md)</span><span class="sxs-lookup"><span data-stu-id="d7f86-108">A server and database [Create an Azure Database for PostgreSQL](quickstart-create-server-database-azure-cli.md)</span></span>
- <span data-ttu-id="d7f86-109">Installera [Azure CLI 2.0](/cli/azure/install-azure-cli) kommandot kommandoradsverktyget eller använda Azure Cloud Shell i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="d7f86-109">Install [Azure CLI 2.0](/cli/azure/install-azure-cli) command line utility or use the Azure Cloud Shell in the browser.</span></span>

## <a name="list-server-configuration-parameters-for-azure-database-for-postgresql-server"></a><span data-ttu-id="d7f86-110">Lista server konfigurationsparametrar för Azure-databas för PostgreSQL-server</span><span class="sxs-lookup"><span data-stu-id="d7f86-110">List server configuration parameters for Azure Database for PostgreSQL server</span></span>
<span data-ttu-id="d7f86-111">Om du vill visa en lista med alla parametrar som kan ändras i en server och deras värden, kör den [az postgres server konfigurationslistan](/cli/azure/postgres/server/configuration#list) kommando.</span><span class="sxs-lookup"><span data-stu-id="d7f86-111">To list all modifiable parameters in a server and their values, run the [az postgres server configuration list](/cli/azure/postgres/server/configuration#list) command.</span></span>

<span data-ttu-id="d7f86-112">Du kan ange serverns konfigurationsparametrar för servern **mypgserver 20170401.postgres.database.azure.com** under resursgrupp **myresourcegroup**.</span><span class="sxs-lookup"><span data-stu-id="d7f86-112">You can list the server configuration parameters for the server **mypgserver-20170401.postgres.database.azure.com** under resource group **myresourcegroup**.</span></span>
```azurecli-interactive
az postgres server configuration list --resource-group myresourcegroup --server mypgserver-20170401
```
## <a name="show-server-configuration-parameter-details"></a><span data-ttu-id="d7f86-113">Visa serverkonfiguration parameterinformation</span><span class="sxs-lookup"><span data-stu-id="d7f86-113">Show server configuration parameter details</span></span>
<span data-ttu-id="d7f86-114">Om du vill visa information om en specifik konfigurationsparameter för en server som kör den [az postgres server configuration visa](/cli/azure/postgres/server/configuration#show) kommando.</span><span class="sxs-lookup"><span data-stu-id="d7f86-114">To show details about a particular configuration parameter for a server, run the [az postgres server configuration show](/cli/azure/postgres/server/configuration#show)  command.</span></span>

<span data-ttu-id="d7f86-115">Det här exemplet visas information om den **loggen\_min\_meddelanden** server konfigurationsparameter för server **mypgserver 20170401.postgres.database.azure.com** under resursgruppen **myresourcegroup.**</span><span class="sxs-lookup"><span data-stu-id="d7f86-115">This example shows details of the **log\_min\_messages** server configuration parameter for server **mypgserver-20170401.postgres.database.azure.com** under resource group **myresourcegroup.**</span></span>
```azurecli-interactive
az postgres server configuration show --name log_min_messages --resource-group myresourcegroup --server mypgserver-20170401
```
## <a name="modify-server-configuration-parameter-value"></a><span data-ttu-id="d7f86-116">Ändra parametervärdet för server-konfiguration</span><span class="sxs-lookup"><span data-stu-id="d7f86-116">Modify server configuration parameter value</span></span>
<span data-ttu-id="d7f86-117">Du kan också ändra värdet för en viss server konfigurationsparameter och underliggande Konfigurationsvärdet för server-databasmotorn PostgreSQL uppdateras.</span><span class="sxs-lookup"><span data-stu-id="d7f86-117">You can also modify the value of a certain server configuration parameter, and this updates the underlying configuration value for the PostgreSQL server engine.</span></span> <span data-ttu-id="d7f86-118">Uppdatera den med hjälp av [az postgres server configuration set](/cli/azure/postgres/server/configuration#set) kommando.</span><span class="sxs-lookup"><span data-stu-id="d7f86-118">To update the configuration use the [az postgres server configuration set](/cli/azure/postgres/server/configuration#set) command.</span></span> 

<span data-ttu-id="d7f86-119">Uppdatera den **loggen\_min\_meddelanden** server konfigurationsparameter Server **mypgserver 20170401.postgres.database.azure.com** under resursgrupp **myresourcegroup.**</span><span class="sxs-lookup"><span data-stu-id="d7f86-119">To update the **log\_min\_messages** server configuration parameter of server **mypgserver-20170401.postgres.database.azure.com** under resource group **myresourcegroup.**</span></span>
```azurecli-interactive
az postgres server configuration set --name log_min_messages --resource-group myresourcegroup --server mypgserver-20170401 --value INFO
```
<span data-ttu-id="d7f86-120">Om du vill återställa värdet för en konfigurationsparameter du helt enkelt välja att lämna ut den valfria `--value` parameter och tjänsten gäller standardvärdet.</span><span class="sxs-lookup"><span data-stu-id="d7f86-120">If you want to reset the value of a configuration parameter, you simply choose to leave out the optional `--value` parameter, and the service will apply the default value.</span></span> <span data-ttu-id="d7f86-121">I ovanstående exempel ser det ut som:</span><span class="sxs-lookup"><span data-stu-id="d7f86-121">In above example, it would look like:</span></span>
```azurecli-interactive
az postgres server configuration set --name log_min_messages --resource-group myresourcegroup --server mypgserver-20170401
```
<span data-ttu-id="d7f86-122">Det här återställer den **loggen\_min\_meddelanden** konfiguration till standardvärdet **varning**.</span><span class="sxs-lookup"><span data-stu-id="d7f86-122">This will reset the **log\_min\_messages** configuration to the default value **WARNING**.</span></span> <span data-ttu-id="d7f86-123">Mer information om konfiguration och tillåtna värden finns i dokumentationen för PostgreSQL på [serverkonfiguration](https://www.postgresql.org/docs/9.6/static/runtime-config.html).</span><span class="sxs-lookup"><span data-stu-id="d7f86-123">For further details on server configuration and permissible values, see PostgreSQL documentation on [Server Configuration](https://www.postgresql.org/docs/9.6/static/runtime-config.html).</span></span>

## <a name="next-steps"></a><span data-ttu-id="d7f86-124">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d7f86-124">Next steps</span></span>
- <span data-ttu-id="d7f86-125">Om du vill konfigurera och komma åt server-loggar finns [Server-loggar i Azure-databas för PostgreSQL](concepts-server-logs.md)</span><span class="sxs-lookup"><span data-stu-id="d7f86-125">To configure and access server logs, see [Server Logs in Azure Database for PostgreSQL](concepts-server-logs.md)</span></span>
