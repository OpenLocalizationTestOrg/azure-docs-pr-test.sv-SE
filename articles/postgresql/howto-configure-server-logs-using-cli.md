---
title: "Konfigurera och komma åt serverloggen för PostgreSQL med Azure CLI | Microsoft Docs"
description: "Den här artikeln beskriver hur du konfigurerar och komma åt de server-loggarna i Azure-databas för PostgreSQL med hjälp av Azure CLI-kommandoraden."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: article
ms.date: 06/13/2017
ms.openlocfilehash: 26f8e12c493904f722cad5191ee053feff20f7fc
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="configure-and-access-server-logs-using-azure-cli"></a><span data-ttu-id="56566-103">Konfigurera och komma åt server-loggar med hjälp av Azure CLI</span><span class="sxs-lookup"><span data-stu-id="56566-103">Configure and access server logs using Azure CLI</span></span>
<span data-ttu-id="56566-104">Du kan hämta felloggar för PostgreSQL-servern med hjälp av kommandoradsgränssnittet (Azure CLI).</span><span class="sxs-lookup"><span data-stu-id="56566-104">You can download the PostgreSQL server error logs using the Command Line Interface (Azure CLI).</span></span> <span data-ttu-id="56566-105">Åtkomst till transaktionsloggar stöds dock inte.</span><span class="sxs-lookup"><span data-stu-id="56566-105">However, access to transaction logs is not supported.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="56566-106">Krav</span><span class="sxs-lookup"><span data-stu-id="56566-106">Prerequisites</span></span>
<span data-ttu-id="56566-107">Du behöver följande för att gå igenom den här instruktioner:</span><span class="sxs-lookup"><span data-stu-id="56566-107">To step through this how-to guide, you need:</span></span>
- <span data-ttu-id="56566-108">En [Azure-databas för PostgreSQL-server](quickstart-create-server-database-azure-cli.md)</span><span class="sxs-lookup"><span data-stu-id="56566-108">An [Azure Database for PostgreSQL server](quickstart-create-server-database-azure-cli.md)</span></span>
- <span data-ttu-id="56566-109">Installera [Azure CLI 2.0](/cli/azure/install-azure-cli) -kommandoradsverktyget eller Använd Azure Cloud-gränssnittet i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="56566-109">Install [Azure CLI 2.0](/cli/azure/install-azure-cli) command-line utility or use the Azure Cloud Shell in the browser.</span></span>

## <a name="configure-logging-for-azure-database-for-postgresql"></a><span data-ttu-id="56566-110">Konfigurera loggning för Azure-databas för PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="56566-110">Configure logging for Azure Database for PostgreSQL</span></span>
<span data-ttu-id="56566-111">Du kan konfigurera servern för att få åtkomst till frågeloggar och i felloggarna.</span><span class="sxs-lookup"><span data-stu-id="56566-111">You can configure the server to access query logs and error logs.</span></span> <span data-ttu-id="56566-112">Felloggarna kan innehålla information om automatisk vakuum, anslutning och kontrollpunkter.</span><span class="sxs-lookup"><span data-stu-id="56566-112">Error logs can contain auto-vacuum, connection, and checkpoints information.</span></span>
1. <span data-ttu-id="56566-113">Aktivera loggning</span><span class="sxs-lookup"><span data-stu-id="56566-113">Turn on logging</span></span>
2. <span data-ttu-id="56566-114">Update-loggen\_-instruktionen och logga\_min\_varaktighet\_instruktionen för att aktivera loggning av frågor</span><span class="sxs-lookup"><span data-stu-id="56566-114">Update log\_statement and log\_min\_duration\_statement to enable query logging</span></span>
3. <span data-ttu-id="56566-115">Uppdatera kvarhållningsperioden</span><span class="sxs-lookup"><span data-stu-id="56566-115">Update retention period</span></span>

<span data-ttu-id="56566-116">Mer information finns i [anpassa server konfigurationsparametrar](howto-configure-server-parameters-using-cli.md).</span><span class="sxs-lookup"><span data-stu-id="56566-116">For more information, see [customizing server configuration parameters](howto-configure-server-parameters-using-cli.md).</span></span>

## <a name="list-logs-for-azure-database-for-postgresql-server"></a><span data-ttu-id="56566-117">Lista loggar för Azure-databas för PostgreSQL-server</span><span class="sxs-lookup"><span data-stu-id="56566-117">List logs for Azure Database for PostgreSQL server</span></span>
<span data-ttu-id="56566-118">Om du vill visa tillgängliga loggfilerna för din server kör den [az postgres serverloggen listan](/cli/azure/postgres/server-logs#list) kommando.</span><span class="sxs-lookup"><span data-stu-id="56566-118">To list the available log files for your server, run the [az postgres server-logs list](/cli/azure/postgres/server-logs#list) command.</span></span>

<span data-ttu-id="56566-119">Du kan visa loggfilerna för server **mypgserver 20170401.postgres.database.azure.com** under resursgrupp **myresourcegroup**, och direkt till en textfil med namnet **loggen\_filer\_list.txt.**</span><span class="sxs-lookup"><span data-stu-id="56566-119">You can list the log files for server **mypgserver-20170401.postgres.database.azure.com** under Resource Group **myresourcegroup**, and direct it to a text file called **log\_files\_list.txt.**</span></span>
```azurecli-interactive
az postgres server-logs list --resource-group myresourcegroup --server mypgserver-20170401 > log_files_list.txt
```
## <a name="download-logs-locally-from-the-server"></a><span data-ttu-id="56566-120">Hämta loggarna lokalt från servern</span><span class="sxs-lookup"><span data-stu-id="56566-120">Download logs locally from the server</span></span>
<span data-ttu-id="56566-121">Den [az postgres serverloggen hämta](/cli/azure/postgres/server-logs#download) kommandot kan du hämta individuella loggfiler för servern.</span><span class="sxs-lookup"><span data-stu-id="56566-121">The [az postgres server-logs download](/cli/azure/postgres/server-logs#download) command allows you to download individual log files for your server.</span></span> 

<span data-ttu-id="56566-122">Det här exemplet hämtas specifik loggfil för servern **mypgserver 20170401.postgres.database.azure.com** under resursgrupp **myresourcegroup** till din lokala miljö.</span><span class="sxs-lookup"><span data-stu-id="56566-122">This example downloads the specific log file for the server **mypgserver-20170401.postgres.database.azure.com** under Resource Group **myresourcegroup** to your local environment.</span></span>
```azurecli-interactive
az postgres server-logs download --name 20170414-mypgserver-20170401-postgresql.log --resource-group myresourcegroup --server mypgserver-20170401
```
## <a name="next-steps"></a><span data-ttu-id="56566-123">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="56566-123">Next steps</span></span>
- <span data-ttu-id="56566-124">Mer information om server-loggar finns [Server-loggar i Azure-databas för PostgreSQL](concepts-server-logs.md)</span><span class="sxs-lookup"><span data-stu-id="56566-124">To learn more about server logs, see [Server Logs in Azure Database for PostgreSQL](concepts-server-logs.md)</span></span>
- <span data-ttu-id="56566-125">Mer information om serverparametrar finns [anpassa konfigurationsparametrar för servern med hjälp av Azure CLI](howto-configure-server-parameters-using-cli.md)</span><span class="sxs-lookup"><span data-stu-id="56566-125">For more information on server parameters, see [Customize server configuration parameters using Azure CLI](howto-configure-server-parameters-using-cli.md)</span></span>
