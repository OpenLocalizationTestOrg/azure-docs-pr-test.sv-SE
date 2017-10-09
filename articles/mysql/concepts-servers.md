---
title: "aaaServer begrepp i Azure-databas för MySQL | Microsoft Docs"
description: "Det här avsnittet innehåller information och riktlinjer för att arbeta med Azure-databas för MySQL-servrar."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 07/06/2017
ms.openlocfilehash: 4d994cbdbde93ac5af3f4a2a7375b5851bebb1cc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="server-concepts-in-azure-database-for-mysql"></a><span data-ttu-id="d00d1-103">Server begrepp i Azure för MySQL-databas</span><span class="sxs-lookup"><span data-stu-id="d00d1-103">Server concepts in Azure Database for MySQL</span></span>
<span data-ttu-id="d00d1-104">Det här avsnittet innehåller information och riktlinjer för att arbeta med Azure-databas för MySQL-servrar.</span><span class="sxs-lookup"><span data-stu-id="d00d1-104">This topic provides considerations and guidelines for working with Azure Database for MySQL servers.</span></span>

## <a name="what-is-an-azure-database-for-mysql-server"></a><span data-ttu-id="d00d1-105">Vad är en Azure-databas för MySQL-servern?</span><span class="sxs-lookup"><span data-stu-id="d00d1-105">What is an Azure Database for MySQL server?</span></span>

<span data-ttu-id="d00d1-106">En Azure-databas för MySQL-server är en central administrativ plats för flera databaser.</span><span class="sxs-lookup"><span data-stu-id="d00d1-106">An Azure Database for MySQL server is a central administrative point for multiple databases.</span></span> <span data-ttu-id="d00d1-107">Det är hello samma MySQL server konstruktion som du kanske känner till lokala hälsningsmeddelande.</span><span class="sxs-lookup"><span data-stu-id="d00d1-107">It is hello same MySQL server construct that you may be familiar with in hello on-premises world.</span></span> <span data-ttu-id="d00d1-108">Mer specifikt hello Azure-databas för MySQL-tjänst som hanteras, ger garanti vad gäller prestanda, visar åtkomst och funktioner på servernivå.</span><span class="sxs-lookup"><span data-stu-id="d00d1-108">Specifically, hello Azure Database for MySQL service is managed, provides performance guarantees, exposes access and features at server-level.</span></span>

<span data-ttu-id="d00d1-109">En Azure-databas för MySQL-server:</span><span class="sxs-lookup"><span data-stu-id="d00d1-109">An Azure Database for MySQL server:</span></span>

- <span data-ttu-id="d00d1-110">Skapas inom en Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="d00d1-110">Is created within an Azure subscription.</span></span>
- <span data-ttu-id="d00d1-111">Är hello överordnade resurs för databaser.</span><span class="sxs-lookup"><span data-stu-id="d00d1-111">Is hello parent resource for databases.</span></span>
- <span data-ttu-id="d00d1-112">Ger ett namnområde för databaser.</span><span class="sxs-lookup"><span data-stu-id="d00d1-112">Provides a namespace for databases.</span></span>
- <span data-ttu-id="d00d1-113">Är en behållare med starka livstid semantik - ta bort en server och tar bort hello finns databaser.</span><span class="sxs-lookup"><span data-stu-id="d00d1-113">Is a container with strong lifetime semantics - delete a server and it deletes hello contained databases.</span></span>
- <span data-ttu-id="d00d1-114">Collocates resurser i en region.</span><span class="sxs-lookup"><span data-stu-id="d00d1-114">Collocates resources in a region.</span></span>
- <span data-ttu-id="d00d1-115">Ger en Anslutningens slutpunkt för server och åtkomst till databasen.</span><span class="sxs-lookup"><span data-stu-id="d00d1-115">Provides a connection endpoint for server and database access.</span></span>
- <span data-ttu-id="d00d1-116">Hello scope för av hanteringsprinciper som gäller tooits databaser: inloggning, brandvägg, användare, roller, konfigurationer och så vidare.</span><span class="sxs-lookup"><span data-stu-id="d00d1-116">Provides hello scope for management policies that apply tooits databases: login, firewall, users, roles, configurations, etc.</span></span>
- <span data-ttu-id="d00d1-117">Är tillgänglig i flera versioner.</span><span class="sxs-lookup"><span data-stu-id="d00d1-117">Is available in multiple versions.</span></span> <span data-ttu-id="d00d1-118">Mer information finns i [stöd för Azure-databas för MySQL-databasversioner](./concepts-supported-versions.md).</span><span class="sxs-lookup"><span data-stu-id="d00d1-118">For more information, see [Supported Azure Database for MySQL database versions](./concepts-supported-versions.md).</span></span>

<span data-ttu-id="d00d1-119">Du kan skapa en eller flera databaser på en Azure Database för MySQL-server.</span><span class="sxs-lookup"><span data-stu-id="d00d1-119">Within an Azure Database for MySQL server, you can create one or multiple databases.</span></span> <span data-ttu-id="d00d1-120">Du kan välja toocreate en enskild databas per server tooutilize alla hello resurser eller skapa flera databaser tooshare hello resurser.</span><span class="sxs-lookup"><span data-stu-id="d00d1-120">You can opt toocreate a single database per server tooutilize all hello resources, or create multiple databases tooshare hello resources.</span></span> <span data-ttu-id="d00d1-121">hello priser är strukturerade per server, baserat på hello konfiguration av prisnivån compute enheter, lagringsutrymme (GB).</span><span class="sxs-lookup"><span data-stu-id="d00d1-121">hello pricing is structured per-server, based on hello configuration of pricing tier, compute units, storage (GB).</span></span> <span data-ttu-id="d00d1-122">Mer information finns i [prisnivåer](./concepts-service-tiers.md).</span><span class="sxs-lookup"><span data-stu-id="d00d1-122">For more information, see [Pricing tiers](./concepts-service-tiers.md).</span></span>

## <a name="how-do-i-connect-and-authenticate-tooan-azure-database-for-mysql-server"></a><span data-ttu-id="d00d1-123">Hur jag för att ansluta och autentisera tooan Azure-databas för MySQL-servern?</span><span class="sxs-lookup"><span data-stu-id="d00d1-123">How do I connect and authenticate tooan Azure Database for MySQL server?</span></span>

<span data-ttu-id="d00d1-124">hello att följande element säkerställa säker åtkomst tooyour databas.</span><span class="sxs-lookup"><span data-stu-id="d00d1-124">hello following elements help ensure safe access tooyour database.</span></span>

|||
| :-- | :-- |
| <span data-ttu-id="d00d1-125">**Autentisering och auktorisering**</span><span class="sxs-lookup"><span data-stu-id="d00d1-125">**Authentication and authorization**</span></span> | <span data-ttu-id="d00d1-126">Azure-databas för MySQL-server stöder interna MySQL-autentisering.</span><span class="sxs-lookup"><span data-stu-id="d00d1-126">Azure Database for MySQL server supports native MySQL authentication.</span></span> <span data-ttu-id="d00d1-127">Du kan ansluta och autentisera tooserver med hello server admin-inloggning.</span><span class="sxs-lookup"><span data-stu-id="d00d1-127">You can connect and authenticate tooserver with hello server's admin login.</span></span> |
| <span data-ttu-id="d00d1-128">**Protokoll**</span><span class="sxs-lookup"><span data-stu-id="d00d1-128">**Protocol**</span></span> | <span data-ttu-id="d00d1-129">hello tjänsten stöder en message-baserat protokoll som används av MySQL.</span><span class="sxs-lookup"><span data-stu-id="d00d1-129">hello service supports a message-based protocol used by MySQL.</span></span> |
| <span data-ttu-id="d00d1-130">**TCP/IP**</span><span class="sxs-lookup"><span data-stu-id="d00d1-130">**TCP/IP**</span></span> | <span data-ttu-id="d00d1-131">hello-protokollet stöds via TCP/IP och över sockets för Unix-domän.</span><span class="sxs-lookup"><span data-stu-id="d00d1-131">hello protocol is supported over TCP/IP, and over Unix-domain sockets.</span></span> |
| <span data-ttu-id="d00d1-132">**Brandvägg**</span><span class="sxs-lookup"><span data-stu-id="d00d1-132">**Firewall**</span></span> | <span data-ttu-id="d00d1-133">toohelp skydda dina data, en brandväggsregel förhindrar alla åtkomst tooyour database-server eller tooits databaser, förrän du anger vilka datorer som har behörighet.</span><span class="sxs-lookup"><span data-stu-id="d00d1-133">toohelp protect your data, a firewall rule prevents all access tooyour database server, or tooits databases, until you specify which computers have permission.</span></span> <span data-ttu-id="d00d1-134">Se [Azure-databas för MySQL serverbrandväggsreglerna](./concepts-firewall-rules.md).</span><span class="sxs-lookup"><span data-stu-id="d00d1-134">See [Azure Database for MySQL Server firewall rules](./concepts-firewall-rules.md).</span></span> |
| <span data-ttu-id="d00d1-135">**SSL**</span><span class="sxs-lookup"><span data-stu-id="d00d1-135">**SSL**</span></span> | <span data-ttu-id="d00d1-136">hello-tjänsten har stöd för att framtvinga SSL-anslutningar mellan dina program och databasservern.</span><span class="sxs-lookup"><span data-stu-id="d00d1-136">hello service supports enforcing SSL connections between your applications and your database server.</span></span>  <span data-ttu-id="d00d1-137">Se [Konfigurera SSL-anslutning i ditt program toosecurely ansluta tooAzure databas för MySQL](./howto-configure-ssl.md).</span><span class="sxs-lookup"><span data-stu-id="d00d1-137">See [Configure SSL connectivity in your application toosecurely connect tooAzure Database for MySQL](./howto-configure-ssl.md).</span></span> |
|||

## <a name="how-do-i-manage-a-server"></a><span data-ttu-id="d00d1-138">Hur hanterar ett server?</span><span class="sxs-lookup"><span data-stu-id="d00d1-138">How do I manage a server?</span></span>
<span data-ttu-id="d00d1-139">Du kan hantera Azure-databas för MySQL-servrar med hjälp av hello Azure-portalen eller hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="d00d1-139">You can manage Azure Database for MySQL servers by using hello Azure portal or hello Azure CLI.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d00d1-140">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d00d1-140">Next steps</span></span>
- <span data-ttu-id="d00d1-141">En översikt över hello-tjänsten finns [Azure Database MySQL-översikt](./overview.md)</span><span class="sxs-lookup"><span data-stu-id="d00d1-141">For an overview of hello service, see [Azure Database for MySQL Overview](./overview.md)</span></span>
- <span data-ttu-id="d00d1-142">Information om specifik resurs kvoter och begränsningar baserat på din **tjänstnivån**, se [tjänstnivåer](./concepts-service-tiers.md)</span><span class="sxs-lookup"><span data-stu-id="d00d1-142">For information about specific resource quotas and limitations based on your **service tier**, see [Service tiers](./concepts-service-tiers.md)</span></span>
- <span data-ttu-id="d00d1-143">Information om anslutande toohello tjänsten finns [anslutningsbibliotek för Azure-databas för MySQL](./concepts-connection-libraries.md).</span><span class="sxs-lookup"><span data-stu-id="d00d1-143">For information about connecting toohello service, see [Connection libraries for Azure Database for MySQL](./concepts-connection-libraries.md).</span></span>
