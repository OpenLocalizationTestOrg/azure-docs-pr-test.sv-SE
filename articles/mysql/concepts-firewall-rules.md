---
title: "Azure-databas för MySQL serverbrandväggsreglerna | Microsoft Docs"
description: "Beskriver brandväggsregler för din Azure-databas för MySQL-servern."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 05/10/2017
ms.openlocfilehash: 7456cef7a5da665ee3d70df64265b8186a79f9b3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-database-for-mysql-server-firewall-rules"></a><span data-ttu-id="e4394-103">Azure-databas för MySQL serverbrandväggsreglerna</span><span class="sxs-lookup"><span data-stu-id="e4394-103">Azure Database for MySQL server firewall rules</span></span>
<span data-ttu-id="e4394-104">Brandväggar förhindrar all åtkomst till databasservern förrän du anger vilka datorer som har behörighet.</span><span class="sxs-lookup"><span data-stu-id="e4394-104">Firewalls prevent all access to your database server until you specify which computers have permission.</span></span> <span data-ttu-id="e4394-105">Brandväggen ger åtkomst till servern, baserat på den ursprungliga IP-adressen för varje begäran.</span><span class="sxs-lookup"><span data-stu-id="e4394-105">The firewall grants access to the server based on the originating IP address of each request.</span></span>

<span data-ttu-id="e4394-106">Du konfigurerar brandväggen genom att skapa brandväggsregler som anger intervall med godkända IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="e4394-106">To configure your firewall, you create firewall rules that specify ranges of acceptable IP addresses.</span></span> <span data-ttu-id="e4394-107">Du kan skapa brandväggsregler på servernivå.</span><span class="sxs-lookup"><span data-stu-id="e4394-107">You can create firewall rules at the server level.</span></span>

<span data-ttu-id="e4394-108">**Regler i brandväggen:** reglerna att klienter ska kunna komma åt Azure hela databasen för MySQL-server som är alla databaser inom samma logiska server.</span><span class="sxs-lookup"><span data-stu-id="e4394-108">**Firewall rules:** These rules enable clients to access your entire Azure Database for MySQL server, that is, all the databases within the same logical server.</span></span> <span data-ttu-id="e4394-109">Brandväggsregler på servernivå kan konfigureras med hjälp av Azure-portalen eller med Azure CLI-kommandona.</span><span class="sxs-lookup"><span data-stu-id="e4394-109">Server-level firewall rules can be configured by using the Azure portal or using Azure CLI commands.</span></span> <span data-ttu-id="e4394-110">Du måste vara prenumeration ägare eller deltagare i prenumeration för att skapa brandväggsregler på servernivå.</span><span class="sxs-lookup"><span data-stu-id="e4394-110">To create server-level firewall rules, you must be the subscription owner or a subscription contributor.</span></span>

## <a name="firewall-overview"></a><span data-ttu-id="e4394-111">Översikt över brandväggar</span><span class="sxs-lookup"><span data-stu-id="e4394-111">Firewall overview</span></span>
<span data-ttu-id="e4394-112">Alla databasåtkomst till din Azure-databas för MySQL-servern har blockerats av brandväggen som standard.</span><span class="sxs-lookup"><span data-stu-id="e4394-112">All database access to your Azure Database for MySQL server is blocked by the firewall by default.</span></span> <span data-ttu-id="e4394-113">Du måste ange en eller flera servernivå brandväggsregler för att ge åtkomst till servern om du vill börja använda din server från en annan dator.</span><span class="sxs-lookup"><span data-stu-id="e4394-113">To begin using your server from another computer, you need to specify one or more server-level firewall rules to enable access to your server.</span></span> <span data-ttu-id="e4394-114">Använd brandväggsreglerna för att ange vilka IP-adressintervall från Internet för att tillåta.</span><span class="sxs-lookup"><span data-stu-id="e4394-114">Use the firewall rules to specify which IP address ranges from the Internet to allow.</span></span> <span data-ttu-id="e4394-115">Åtkomst till Azure-portalen webbplatsen själva påverkas inte av brandväggsreglerna.</span><span class="sxs-lookup"><span data-stu-id="e4394-115">Access to the Azure portal website itself is not impacted by the firewall rules.</span></span>

<span data-ttu-id="e4394-116">Anslutningsförsök från Internet och Azure måste först passera genom brandväggen innan de kan komma åt Azure-databasen för MySQL-databas som visas i följande diagram:</span><span class="sxs-lookup"><span data-stu-id="e4394-116">Connection attempts from the Internet and Azure must first pass through the firewall before they can reach your Azure Database for MySQL database, as shown in the following diagram:</span></span>

![Exempel flödet av hur brandväggen fungerar](./media/concepts-firewall-rules/1-firewall-concept.png)

## <a name="connecting-from-the-internet"></a><span data-ttu-id="e4394-118">Ansluta från Internet</span><span class="sxs-lookup"><span data-stu-id="e4394-118">Connecting from the Internet</span></span>
<span data-ttu-id="e4394-119">Brandväggsregler på servernivå gäller för alla databaser på Azure-databasen för MySQL-servern.</span><span class="sxs-lookup"><span data-stu-id="e4394-119">Server-level firewall rules apply to all databases on the Azure Database for MySQL server.</span></span>

<span data-ttu-id="e4394-120">Om IP-adressen för begäran ligger inom ett intervall som anges i brandväggsreglerna på servernivå godkänns anslutningen.</span><span class="sxs-lookup"><span data-stu-id="e4394-120">If the IP address of the request is within one of the ranges specified in the server-level firewall rules, the connection is granted.</span></span>

<span data-ttu-id="e4394-121">Om IP-adressen för begäran inte är inom det intervall som anges i någon av databas- eller server-nivå brandväggsreglerna inte anslutningsbegäran.</span><span class="sxs-lookup"><span data-stu-id="e4394-121">If the IP address of the request is not within the ranges specified in any of the database-level or server-level firewall rules, the connection request fails.</span></span>

## <a name="programmatically-managing-firewall-rules"></a><span data-ttu-id="e4394-122">Hantera brandväggsregler via programmering</span><span class="sxs-lookup"><span data-stu-id="e4394-122">Programmatically managing firewall rules</span></span>
<span data-ttu-id="e4394-123">Utöver Azure portal kan brandväggsregler hanteras via programmering med Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="e4394-123">In addition to the Azure portal, firewall rules can be managed programmatically using Azure CLI.</span></span> <span data-ttu-id="e4394-124">Se även [skapa och hantera Azure-databas för MySQL brandväggsregler med hjälp av Azure CLI](./howto-manage-firewall-using-cli.md)</span><span class="sxs-lookup"><span data-stu-id="e4394-124">See also [Create and manage Azure Database for MySQL firewall rules using Azure CLI](./howto-manage-firewall-using-cli.md)</span></span>

## <a name="troubleshooting-the-database-firewall"></a><span data-ttu-id="e4394-125">Felsöka databasbrandväggen</span><span class="sxs-lookup"><span data-stu-id="e4394-125">Troubleshooting the database firewall</span></span>
<span data-ttu-id="e4394-126">Tänk på följande när åtkomst till Microsoft Azure-databas för MySQL server-tjänsten inte fungerar som förväntat:</span><span class="sxs-lookup"><span data-stu-id="e4394-126">Consider the following points when access to the Microsoft Azure Database for MySQL server service does not behave as you expect:</span></span>

* <span data-ttu-id="e4394-127">**Ändringar i listan över tillåtna har inte börjat gälla ännu:** kan det vara så mycket som en fem-minuters fördröjning för ändringar i Azure-databasen för brandväggskonfigurationen för MySQL-servern ska börja gälla.</span><span class="sxs-lookup"><span data-stu-id="e4394-127">**Changes to the allow list have not taken effect yet:** There may be as much as a five-minute delay for changes to the Azure Database for MySQL Server firewall configuration to take effect.</span></span>

* <span data-ttu-id="e4394-128">**Inloggningen har inte behörighet eller ett felaktigt lösenord användes:** om en inloggning har inte behörighet för Azure-databas för MySQL-server eller lösenordet som används är felaktig, anslutningen till Azure-databas för MySQL server nekas.</span><span class="sxs-lookup"><span data-stu-id="e4394-128">**The login is not authorized or an incorrect password was used:** If a login does not have permissions on the Azure Database for MySQL server or the password used is incorrect, the connection to the Azure Database for MySQL server is denied.</span></span> <span data-ttu-id="e4394-129">En brandväggsinställning ger endast klienter möjlighet att försöka ansluta till din server. Varje klient måste fortfarande ange nödvändiga säkerhetsreferenser.</span><span class="sxs-lookup"><span data-stu-id="e4394-129">Creating a firewall setting only provides clients with an opportunity to attempt connecting to your server; each client must provide the necessary security credentials.</span></span>

* <span data-ttu-id="e4394-130">**Dynamisk IP-adress:** Om du har en Internetanslutning med dynamisk IP-adressering och du har problem med att passera brandväggen kan du prova någon av följande lösningar:</span><span class="sxs-lookup"><span data-stu-id="e4394-130">**Dynamic IP address:** If you have an Internet connection with dynamic IP addressing and you are having trouble getting through the firewall, you could try one of the following solutions:</span></span>

* <span data-ttu-id="e4394-131">Be Internet Service Provider (ISP) för IP-adressintervall som tilldelats till klientdatorer som har åtkomst till Azure-databasen för MySQL-servern och sedan lägga till IP-adressintervall som en brandväggsregel.</span><span class="sxs-lookup"><span data-stu-id="e4394-131">Ask your Internet Service Provider (ISP) for the IP address range assigned to your client computers that access the Azure Database for MySQL server, and then add the IP address range as a firewall rule.</span></span>

* <span data-ttu-id="e4394-132">Använd statisk IP-adressering i stället för dina klientdatorer och lägg sedan till IP-adresserna som brandväggsregler.</span><span class="sxs-lookup"><span data-stu-id="e4394-132">Get static IP addressing instead for your client computers, and then add the IP addresses as firewall rules.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e4394-133">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e4394-133">Next steps</span></span>

<span data-ttu-id="e4394-134">[Skapa och hantera Azure-databas för MySQL brandväggsregler med hjälp av Azure portal](./howto-manage-firewall-using-portal.md)
[skapa och hantera Azure-databas för MySQL brandväggsregler med hjälp av Azure CLI](./howto-manage-firewall-using-cli.md)</span><span class="sxs-lookup"><span data-stu-id="e4394-134">[Create and manage Azure Database for MySQL firewall rules using the Azure portal](./howto-manage-firewall-using-portal.md)
[Create and manage Azure Database for MySQL firewall rules using Azure CLI](./howto-manage-firewall-using-cli.md)</span></span>
