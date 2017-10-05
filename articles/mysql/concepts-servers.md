---
title: "Server begrepp i Azure-databas för MySQL | Microsoft Docs"
description: "Det här avsnittet innehåller information och riktlinjer för att arbeta med Azure-databas för MySQL-servrar."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 07/06/2017
ms.openlocfilehash: a2556206ac53829fcd6ab92ffe292859349790d7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="server-concepts-in-azure-database-for-mysql"></a><span data-ttu-id="adb68-103">Server begrepp i Azure för MySQL-databas</span><span class="sxs-lookup"><span data-stu-id="adb68-103">Server concepts in Azure Database for MySQL</span></span>
<span data-ttu-id="adb68-104">Det här avsnittet innehåller information och riktlinjer för att arbeta med Azure-databas för MySQL-servrar.</span><span class="sxs-lookup"><span data-stu-id="adb68-104">This topic provides considerations and guidelines for working with Azure Database for MySQL servers.</span></span>

## <a name="what-is-an-azure-database-for-mysql-server"></a><span data-ttu-id="adb68-105">Vad är en Azure-databas för MySQL-servern?</span><span class="sxs-lookup"><span data-stu-id="adb68-105">What is an Azure Database for MySQL server?</span></span>

<span data-ttu-id="adb68-106">En Azure-databas för MySQL-server är en central administrativ plats för flera databaser.</span><span class="sxs-lookup"><span data-stu-id="adb68-106">An Azure Database for MySQL server is a central administrative point for multiple databases.</span></span> <span data-ttu-id="adb68-107">Det är samma MySQL server-konstruktion som du kanske känner till i den lokala världen.</span><span class="sxs-lookup"><span data-stu-id="adb68-107">It is the same MySQL server construct that you may be familiar with in the on-premises world.</span></span> <span data-ttu-id="adb68-108">Mer specifikt Azure-databas för MySQL-tjänst hanteras, ger garanti vad gäller prestanda, visar åtkomst och funktioner på servernivå.</span><span class="sxs-lookup"><span data-stu-id="adb68-108">Specifically, the Azure Database for MySQL service is managed, provides performance guarantees, exposes access and features at server-level.</span></span>

<span data-ttu-id="adb68-109">En Azure-databas för MySQL-server:</span><span class="sxs-lookup"><span data-stu-id="adb68-109">An Azure Database for MySQL server:</span></span>

- <span data-ttu-id="adb68-110">Skapas inom en Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="adb68-110">Is created within an Azure subscription.</span></span>
- <span data-ttu-id="adb68-111">Är den överordnade resursen för databaser.</span><span class="sxs-lookup"><span data-stu-id="adb68-111">Is the parent resource for databases.</span></span>
- <span data-ttu-id="adb68-112">Ger ett namnområde för databaser.</span><span class="sxs-lookup"><span data-stu-id="adb68-112">Provides a namespace for databases.</span></span>
- <span data-ttu-id="adb68-113">Är en behållare med starka livstid semantik - ta bort en server och tar bort de inneslutna databaserna.</span><span class="sxs-lookup"><span data-stu-id="adb68-113">Is a container with strong lifetime semantics - delete a server and it deletes the contained databases.</span></span>
- <span data-ttu-id="adb68-114">Collocates resurser i en region.</span><span class="sxs-lookup"><span data-stu-id="adb68-114">Collocates resources in a region.</span></span>
- <span data-ttu-id="adb68-115">Ger en Anslutningens slutpunkt för server och åtkomst till databasen.</span><span class="sxs-lookup"><span data-stu-id="adb68-115">Provides a connection endpoint for server and database access.</span></span>
- <span data-ttu-id="adb68-116">Scope för av hanteringsprinciper som gäller för dess databaser: inloggning, brandvägg, användare, roller, konfigurationer och så vidare.</span><span class="sxs-lookup"><span data-stu-id="adb68-116">Provides the scope for management policies that apply to its databases: login, firewall, users, roles, configurations, etc.</span></span>
- <span data-ttu-id="adb68-117">Är tillgänglig i flera versioner.</span><span class="sxs-lookup"><span data-stu-id="adb68-117">Is available in multiple versions.</span></span> <span data-ttu-id="adb68-118">Mer information finns i [stöd för Azure-databas för MySQL-databasversioner](./concepts-supported-versions.md).</span><span class="sxs-lookup"><span data-stu-id="adb68-118">For more information, see [Supported Azure Database for MySQL database versions](./concepts-supported-versions.md).</span></span>

<span data-ttu-id="adb68-119">Du kan skapa en eller flera databaser på en Azure Database för MySQL-server.</span><span class="sxs-lookup"><span data-stu-id="adb68-119">Within an Azure Database for MySQL server, you can create one or multiple databases.</span></span> <span data-ttu-id="adb68-120">Du kan välja att skapa en databas per server om du vill använda dig av samtliga resurser, eller skapa flera databaser som får dela på resurserna.</span><span class="sxs-lookup"><span data-stu-id="adb68-120">You can opt to create a single database per server to utilize all the resources, or create multiple databases to share the resources.</span></span> <span data-ttu-id="adb68-121">Prissättning är strukturerade per server, baserat på konfigurationen av prisnivån compute enheter, lagringsutrymme (GB).</span><span class="sxs-lookup"><span data-stu-id="adb68-121">The pricing is structured per-server, based on the configuration of pricing tier, compute units, storage (GB).</span></span> <span data-ttu-id="adb68-122">Mer information finns i [prisnivåer](./concepts-service-tiers.md).</span><span class="sxs-lookup"><span data-stu-id="adb68-122">For more information, see [Pricing tiers](./concepts-service-tiers.md).</span></span>

## <a name="how-do-i-connect-and-authenticate-to-an-azure-database-for-mysql-server"></a><span data-ttu-id="adb68-123">Hur gör ansluta och autentisera till en Azure-databas för MySQL-servern?</span><span class="sxs-lookup"><span data-stu-id="adb68-123">How do I connect and authenticate to an Azure Database for MySQL server?</span></span>

<span data-ttu-id="adb68-124">Följande element att säkerställa säker åtkomst till databasen.</span><span class="sxs-lookup"><span data-stu-id="adb68-124">The following elements help ensure safe access to your database.</span></span>

|||
| :-- | :-- |
| <span data-ttu-id="adb68-125">**Autentisering och auktorisering**</span><span class="sxs-lookup"><span data-stu-id="adb68-125">**Authentication and authorization**</span></span> | <span data-ttu-id="adb68-126">Azure-databas för MySQL-server stöder interna MySQL-autentisering.</span><span class="sxs-lookup"><span data-stu-id="adb68-126">Azure Database for MySQL server supports native MySQL authentication.</span></span> <span data-ttu-id="adb68-127">Du kan ansluta och autentisera till servern med serverns administratör för inloggning.</span><span class="sxs-lookup"><span data-stu-id="adb68-127">You can connect and authenticate to server with the server's admin login.</span></span> |
| <span data-ttu-id="adb68-128">**Protokoll**</span><span class="sxs-lookup"><span data-stu-id="adb68-128">**Protocol**</span></span> | <span data-ttu-id="adb68-129">Tjänsten stöder en message-baserat protokoll som används av MySQL.</span><span class="sxs-lookup"><span data-stu-id="adb68-129">The service supports a message-based protocol used by MySQL.</span></span> |
| <span data-ttu-id="adb68-130">**TCP/IP**</span><span class="sxs-lookup"><span data-stu-id="adb68-130">**TCP/IP**</span></span> | <span data-ttu-id="adb68-131">Protokollet stöds via TCP/IP och över sockets för Unix-domän.</span><span class="sxs-lookup"><span data-stu-id="adb68-131">The protocol is supported over TCP/IP, and over Unix-domain sockets.</span></span> |
| <span data-ttu-id="adb68-132">**Brandvägg**</span><span class="sxs-lookup"><span data-stu-id="adb68-132">**Firewall**</span></span> | <span data-ttu-id="adb68-133">För att skydda dina data, förhindrar en brandväggsregel all åtkomst till databasservern eller till dess databaser förrän du anger vilka datorer som har behörighet.</span><span class="sxs-lookup"><span data-stu-id="adb68-133">To help protect your data, a firewall rule prevents all access to your database server, or to its databases, until you specify which computers have permission.</span></span> <span data-ttu-id="adb68-134">Se [Azure-databas för MySQL serverbrandväggsreglerna](./concepts-firewall-rules.md).</span><span class="sxs-lookup"><span data-stu-id="adb68-134">See [Azure Database for MySQL Server firewall rules](./concepts-firewall-rules.md).</span></span> |
| <span data-ttu-id="adb68-135">**SSL**</span><span class="sxs-lookup"><span data-stu-id="adb68-135">**SSL**</span></span> | <span data-ttu-id="adb68-136">Tjänsten stöder att framtvinga SSL-anslutningar mellan dina program och databasservern.</span><span class="sxs-lookup"><span data-stu-id="adb68-136">The service supports enforcing SSL connections between your applications and your database server.</span></span>  <span data-ttu-id="adb68-137">Se [Configure SSL connectivity in your application to securely connect to Azure Database for MySQL](./howto-configure-ssl.md) (Konfigurera SSL-anslutning i ditt program för att säkert ansluta till Azure Database för MySQL).</span><span class="sxs-lookup"><span data-stu-id="adb68-137">See [Configure SSL connectivity in your application to securely connect to Azure Database for MySQL](./howto-configure-ssl.md).</span></span> |
|||

## <a name="how-do-i-manage-a-server"></a><span data-ttu-id="adb68-138">Hur hanterar ett server?</span><span class="sxs-lookup"><span data-stu-id="adb68-138">How do I manage a server?</span></span>
<span data-ttu-id="adb68-139">Du kan hantera Azure-databas för MySQL-servrar med hjälp av Azure-portalen eller Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="adb68-139">You can manage Azure Database for MySQL servers by using the Azure portal or the Azure CLI.</span></span>

## <a name="next-steps"></a><span data-ttu-id="adb68-140">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="adb68-140">Next steps</span></span>
- <span data-ttu-id="adb68-141">En översikt över tjänsten finns [Azure Database MySQL-översikt](./overview.md)</span><span class="sxs-lookup"><span data-stu-id="adb68-141">For an overview of the service, see [Azure Database for MySQL Overview](./overview.md)</span></span>
- <span data-ttu-id="adb68-142">Information om specifik resurs kvoter och begränsningar baserat på din **tjänstnivån**, se [tjänstnivåer](./concepts-service-tiers.md)</span><span class="sxs-lookup"><span data-stu-id="adb68-142">For information about specific resource quotas and limitations based on your **service tier**, see [Service tiers](./concepts-service-tiers.md)</span></span>
- <span data-ttu-id="adb68-143">Information om hur du ansluter till tjänsten finns [anslutningsbibliotek för Azure-databas för MySQL](./concepts-connection-libraries.md).</span><span class="sxs-lookup"><span data-stu-id="adb68-143">For information about connecting to the service, see [Connection libraries for Azure Database for MySQL](./concepts-connection-libraries.md).</span></span>
