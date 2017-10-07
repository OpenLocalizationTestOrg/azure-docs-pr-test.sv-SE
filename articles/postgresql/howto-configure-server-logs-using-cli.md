---
title: "aaaConfigure och åtkomst serverloggen PostgreSQL med Azure CLI | Microsoft Docs"
description: "Den här artikeln beskriver hur tooconfigure och åtkomst hello loggas i Azure-databas för PostgreSQL med hjälp av Azure CLI-kommandoraden."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: article
ms.date: 06/13/2017
ms.openlocfilehash: 924d4e7634cc48405bcb0f813cf61d8b72ad8aa0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-and-access-server-logs-using-azure-cli"></a><span data-ttu-id="fdc90-103">Konfigurera och komma åt server-loggar med hjälp av Azure CLI</span><span class="sxs-lookup"><span data-stu-id="fdc90-103">Configure and access server logs using Azure CLI</span></span>
<span data-ttu-id="fdc90-104">Du kan hämta hello PostgreSQL server-felloggarna med hello kommandoradsgränssnittet (Azure CLI).</span><span class="sxs-lookup"><span data-stu-id="fdc90-104">You can download hello PostgreSQL server error logs using hello Command Line Interface (Azure CLI).</span></span> <span data-ttu-id="fdc90-105">Tootransaction åtkomstloggar stöds dock inte.</span><span class="sxs-lookup"><span data-stu-id="fdc90-105">However, access tootransaction logs is not supported.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="fdc90-106">Krav</span><span class="sxs-lookup"><span data-stu-id="fdc90-106">Prerequisites</span></span>
<span data-ttu-id="fdc90-107">toostep via den här hur tooguide behöver du:</span><span class="sxs-lookup"><span data-stu-id="fdc90-107">toostep through this how-tooguide, you need:</span></span>
- <span data-ttu-id="fdc90-108">En [Azure-databas för PostgreSQL-server](quickstart-create-server-database-azure-cli.md)</span><span class="sxs-lookup"><span data-stu-id="fdc90-108">An [Azure Database for PostgreSQL server](quickstart-create-server-database-azure-cli.md)</span></span>
- <span data-ttu-id="fdc90-109">Installera [Azure CLI 2.0](/cli/azure/install-azure-cli) kommandoradsverktyget verktyg eller Använd hello Azure Cloud Shell i hello webbläsare.</span><span class="sxs-lookup"><span data-stu-id="fdc90-109">Install [Azure CLI 2.0](/cli/azure/install-azure-cli) command-line utility or use hello Azure Cloud Shell in hello browser.</span></span>

## <a name="configure-logging-for-azure-database-for-postgresql"></a><span data-ttu-id="fdc90-110">Konfigurera loggning för Azure-databas för PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="fdc90-110">Configure logging for Azure Database for PostgreSQL</span></span>
<span data-ttu-id="fdc90-111">Du kan konfigurera hello serverloggen tooaccess frågan och felloggarna.</span><span class="sxs-lookup"><span data-stu-id="fdc90-111">You can configure hello server tooaccess query logs and error logs.</span></span> <span data-ttu-id="fdc90-112">Felloggarna kan innehålla information om automatisk vakuum, anslutning och kontrollpunkter.</span><span class="sxs-lookup"><span data-stu-id="fdc90-112">Error logs can contain auto-vacuum, connection, and checkpoints information.</span></span>
1. <span data-ttu-id="fdc90-113">Aktivera loggning</span><span class="sxs-lookup"><span data-stu-id="fdc90-113">Turn on logging</span></span>
2. <span data-ttu-id="fdc90-114">Update-loggen\_-instruktionen och logga\_min\_varaktighet\_instruktionen tooenable loggning av frågor</span><span class="sxs-lookup"><span data-stu-id="fdc90-114">Update log\_statement and log\_min\_duration\_statement tooenable query logging</span></span>
3. <span data-ttu-id="fdc90-115">Uppdatera kvarhållningsperioden</span><span class="sxs-lookup"><span data-stu-id="fdc90-115">Update retention period</span></span>

<span data-ttu-id="fdc90-116">Mer information finns i [anpassa server konfigurationsparametrar](howto-configure-server-parameters-using-cli.md).</span><span class="sxs-lookup"><span data-stu-id="fdc90-116">For more information, see [customizing server configuration parameters](howto-configure-server-parameters-using-cli.md).</span></span>

## <a name="list-logs-for-azure-database-for-postgresql-server"></a><span data-ttu-id="fdc90-117">Lista loggar för Azure-databas för PostgreSQL-server</span><span class="sxs-lookup"><span data-stu-id="fdc90-117">List logs for Azure Database for PostgreSQL server</span></span>
<span data-ttu-id="fdc90-118">loggfiler för toolist hello tillgänglig för din server kör hello [az postgres serverloggen listan](/cli/azure/postgres/server-logs#list) kommando.</span><span class="sxs-lookup"><span data-stu-id="fdc90-118">toolist hello available log files for your server, run hello [az postgres server-logs list](/cli/azure/postgres/server-logs#list) command.</span></span>

<span data-ttu-id="fdc90-119">Du kan ange hello loggfiler för server **mypgserver 20170401.postgres.database.azure.com** under resursgrupp **myresourcegroup**, och ange tooa text fil med namnet **loggen\_filer\_list.txt.**</span><span class="sxs-lookup"><span data-stu-id="fdc90-119">You can list hello log files for server **mypgserver-20170401.postgres.database.azure.com** under Resource Group **myresourcegroup**, and direct it tooa text file called **log\_files\_list.txt.**</span></span>
```azurecli-interactive
az postgres server-logs list --resource-group myresourcegroup --server mypgserver-20170401 > log_files_list.txt
```
## <a name="download-logs-locally-from-hello-server"></a><span data-ttu-id="fdc90-120">Hämta loggarna lokalt från hello-server</span><span class="sxs-lookup"><span data-stu-id="fdc90-120">Download logs locally from hello server</span></span>
<span data-ttu-id="fdc90-121">Hej [az postgres serverloggen hämta](/cli/azure/postgres/server-logs#download) med kommandot kan du toodownload individuella loggfiler för servern.</span><span class="sxs-lookup"><span data-stu-id="fdc90-121">hello [az postgres server-logs download](/cli/azure/postgres/server-logs#download) command allows you toodownload individual log files for your server.</span></span> 

<span data-ttu-id="fdc90-122">Det här exemplet hämtningar hello specifika loggfilen för hello server **mypgserver 20170401.postgres.database.azure.com** under resursgrupp **myresourcegroup** tooyour lokal miljö.</span><span class="sxs-lookup"><span data-stu-id="fdc90-122">This example downloads hello specific log file for hello server **mypgserver-20170401.postgres.database.azure.com** under Resource Group **myresourcegroup** tooyour local environment.</span></span>
```azurecli-interactive
az postgres server-logs download --name 20170414-mypgserver-20170401-postgresql.log --resource-group myresourcegroup --server mypgserver-20170401
```
## <a name="next-steps"></a><span data-ttu-id="fdc90-123">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fdc90-123">Next steps</span></span>
- <span data-ttu-id="fdc90-124">toolearn mer information om server-loggar finns [Server-loggar i Azure-databas för PostgreSQL](concepts-server-logs.md)</span><span class="sxs-lookup"><span data-stu-id="fdc90-124">toolearn more about server logs, see [Server Logs in Azure Database for PostgreSQL](concepts-server-logs.md)</span></span>
- <span data-ttu-id="fdc90-125">Mer information om serverparametrar finns [anpassa konfigurationsparametrar för servern med hjälp av Azure CLI](howto-configure-server-parameters-using-cli.md)</span><span class="sxs-lookup"><span data-stu-id="fdc90-125">For more information on server parameters, see [Customize server configuration parameters using Azure CLI](howto-configure-server-parameters-using-cli.md)</span></span>
