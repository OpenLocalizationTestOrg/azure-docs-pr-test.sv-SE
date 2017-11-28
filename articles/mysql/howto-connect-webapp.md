---
title: "Ansluta befintliga Azure App Service till Azure-databas för MySQL | Microsoft Docs"
description: "Instruktioner för hur du ansluter en befintlig Azure App Service korrekt till Azure-databas för MySQL"
services: mysql
author: v-chenyh
ms.author: v-chenyh
editor: jasonwhowell
manager: jhubbard
ms.service: mysql-database
ms.topic: article
ms.date: 05/23/2017
ms.openlocfilehash: 735e21e8135d67405ec6b88d75be1711a2f071f0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="connect-an-existing-azure-app-service-to-azure-database-for-mysql-server"></a><span data-ttu-id="62713-103">Ansluta en befintlig Azure App Service till Azure-databas för MySQL-server</span><span class="sxs-lookup"><span data-stu-id="62713-103">Connect an existing Azure App Service to Azure Database for MySQL server</span></span>
<span data-ttu-id="62713-104">Det här dokumentet förklarar hur du ansluter en befintlig Azure App Service till din Azure-databas för MySQL-servern.</span><span class="sxs-lookup"><span data-stu-id="62713-104">This document explains how to connect an existing Azure App Service to your Azure Database for MySQL server.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="62713-105">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="62713-105">Before you begin</span></span>
<span data-ttu-id="62713-106">Logga in på [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="62713-106">Log in to the [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="62713-107">Skapa en Azure-databas för MySQL-servern.</span><span class="sxs-lookup"><span data-stu-id="62713-107">Create an Azure Database for MySQL server.</span></span> <span data-ttu-id="62713-108">Mer information finns i [skapa Azure-databas för MySQL-servern från portalen](quickstart-create-mysql-server-database-using-azure-portal.md) eller [skapa Azure-databas för MySQL-servern med hjälp av CLI](quickstart-create-mysql-server-database-using-azure-cli.md).</span><span class="sxs-lookup"><span data-stu-id="62713-108">For details, refer to [How to create Azure Database for MySQL server from Portal](quickstart-create-mysql-server-database-using-azure-portal.md) or [How to create Azure Database for MySQL server using CLI](quickstart-create-mysql-server-database-using-azure-cli.md).</span></span>

<span data-ttu-id="62713-109">Det finns för närvarande två lösningar för att aktivera åtkomst från en Azure App Service till en Azure-databas för MySQL.</span><span class="sxs-lookup"><span data-stu-id="62713-109">Currently there are two solutions to enable access from an Azure App Service to an Azure Database for MySQL.</span></span> <span data-ttu-id="62713-110">Båda lösningarna omfattar att konfigurera brandväggsregler på servernivå.</span><span class="sxs-lookup"><span data-stu-id="62713-110">Both solutions involve setting up server-level firewall rules.</span></span>

## <a name="solution-1---create-a-firewall-rule-to-allow-all-ips"></a><span data-ttu-id="62713-111">Lösning 1 - skapa en brandväggsregel för att tillåta alla IP-adresser</span><span class="sxs-lookup"><span data-stu-id="62713-111">Solution 1 - Create a firewall rule to allow all IPs</span></span>
<span data-ttu-id="62713-112">Azure-databas för MySQL skyddar åtkomst med hjälp av en brandvägg för att skydda dina data.</span><span class="sxs-lookup"><span data-stu-id="62713-112">Azure Database for MySQL provides access security using a firewall to protect your data.</span></span> <span data-ttu-id="62713-113">När du ansluter från en Azure App Service till Azure-databas för MySQL-server, Kom ihåg att utgående IP-adresser för App Service är dynamiska.</span><span class="sxs-lookup"><span data-stu-id="62713-113">When connecting from an Azure App Service to Azure Database for MySQL server, keep in mind that the outbound IPs of App Service are dynamic in nature.</span></span> 

<span data-ttu-id="62713-114">För att säkerställa tillgängligheten för Azure App Service, bör du använda den här lösningen så att alla IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="62713-114">To ensure the availability of your Azure App Service, we recommend using this solution to allow ALL IPs.</span></span>

> [!NOTE]
> <span data-ttu-id="62713-115">Microsoft arbetar för en långsiktig lösning för att undvika att tillåta att alla IP-adresser för Azure-tjänster att ansluta till Azure-databas för MySQL.</span><span class="sxs-lookup"><span data-stu-id="62713-115">Microsoft is working for a long-term solution to avoid allowing all IPs for Azure services to connect to Azure Database for MySQL.</span></span>

1. <span data-ttu-id="62713-116">I bladet MySQL-servern under inställningar rubrik, klickar du på **anslutningssäkerhet** att öppna bladet anslutningssäkerhet för Azure-databas för MySQL.</span><span class="sxs-lookup"><span data-stu-id="62713-116">On the MySQL server blade, under Settings heading, click **Connection Security** to open the Connection Security blade for the Azure Database for MySQL.</span></span>

   ![Azure portal – Klicka på anslutningssäkerhet](./media/howto-manage-firewall-using-portal/1-connection-security.png)

2. <span data-ttu-id="62713-118">Ange **REGELNAMN**, **START-IP**, och **sista IP**.</span><span class="sxs-lookup"><span data-stu-id="62713-118">Enter **RULE NAME**, **START IP**, and **END IP**.</span></span> <span data-ttu-id="62713-119">Klicka sedan på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="62713-119">Then click **Save**.</span></span>
   - <span data-ttu-id="62713-120">Regelnamnet: Tillåt-alla-IP-adresser</span><span class="sxs-lookup"><span data-stu-id="62713-120">Rule name: Allow-All-IPs</span></span>
   - <span data-ttu-id="62713-121">Start-IP: 0.0.0.0</span><span class="sxs-lookup"><span data-stu-id="62713-121">Start IP: 0.0.0.0</span></span>
   - <span data-ttu-id="62713-122">Avsluta IP: 255.255.255.255</span><span class="sxs-lookup"><span data-stu-id="62713-122">End IP: 255.255.255.255</span></span>

   ![Azure portal – Lägg till alla IP-adresser](./media/howto-connect-webapp/1_2-add-all-ips.png)

## <a name="solution-2---create-a-firewall-rule-to-explicitly-allow-outbound-ips"></a><span data-ttu-id="62713-124">Lösning 2 – Skapa en brandväggsregel som uttryckligen tillåta utgående IP-adresser</span><span class="sxs-lookup"><span data-stu-id="62713-124">Solution 2 - Create a firewall rule to explicitly allow outbound IPs</span></span>
<span data-ttu-id="62713-125">Du kan uttryckligen lägga till alla utgående IP-adresser i Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="62713-125">You can explicitly add all the outbound IPs of your Azure App Service.</span></span>

1. <span data-ttu-id="62713-126">På bladet App tjänstegenskaper visa din **utgående IP-adress**.</span><span class="sxs-lookup"><span data-stu-id="62713-126">On the App Service Properties blade, view your **OUTBOUND IP ADDRESS**.</span></span>

   ![Azure portal – Visa utgående IP-adresser](./media/howto-connect-webapp/2_1-outbound-ip-address.png)

2. <span data-ttu-id="62713-128">Lägg till utgående IP-adresser i taget på bladet MySQL-anslutning säkerhet.</span><span class="sxs-lookup"><span data-stu-id="62713-128">On the MySQL Connection security blade, add outbound IPs one by one.</span></span>

   ![Azure portal – Lägg till explicita IP-adresser](./media/howto-connect-webapp/2_2-add-explicit-ips.png)

3. <span data-ttu-id="62713-130">Kom ihåg att **spara** brandväggsreglerna.</span><span class="sxs-lookup"><span data-stu-id="62713-130">Remember to **Save** your firewall rules.</span></span>

<span data-ttu-id="62713-131">Även om Azure App service försöker hålla IP-adresser konstant över tid, finns det fall där IP-adresser kan ändras.</span><span class="sxs-lookup"><span data-stu-id="62713-131">Though the Azure App service attempts to keep IP addresses constant over time, there are cases where the IP addresses may change.</span></span> <span data-ttu-id="62713-132">Till exempel när appen återanvänds eller en skalningsåtgärden inträffar eller när nya datorer läggs till i Azure regionala Datacenter för att öka kapaciteten.</span><span class="sxs-lookup"><span data-stu-id="62713-132">For example, when the app recycles or a scale operation occurs, or when new machines are added in Azure regional data centers to increase the capacity.</span></span> <span data-ttu-id="62713-133">När IP-adresser ändras, uppleva driftstopp appen om den inte längre kan ansluta till MySQL-servern.</span><span class="sxs-lookup"><span data-stu-id="62713-133">When the IP addresses change, the app could experience downtime in the event it can no longer connect to the MySQL server.</span></span> <span data-ttu-id="62713-134">Överväg att denna möjlighet när du väljer ett av ovanstående lösningar.</span><span class="sxs-lookup"><span data-stu-id="62713-134">Consider this potential when choosing one of the preceding solutions.</span></span>

## <a name="ssl-configuration"></a><span data-ttu-id="62713-135">SSL-konfiguration</span><span class="sxs-lookup"><span data-stu-id="62713-135">SSL configuration</span></span>
<span data-ttu-id="62713-136">Azure MySQL-databas har SSL aktiverat som standard.</span><span class="sxs-lookup"><span data-stu-id="62713-136">Azure Database for MySQL has SSL Enabled by default.</span></span> <span data-ttu-id="62713-137">Om ditt program inte är använder SSL för att ansluta till databasen, måste du inaktivera SSL på MySQL-servern.</span><span class="sxs-lookup"><span data-stu-id="62713-137">If your application is not using SSL to connect to the database, then you need to disable SSL on MySQL server.</span></span> <span data-ttu-id="62713-138">Mer information om hur du konfigurerar SSL, se [med hjälp av SSL med Azure-databas för MySQL](howto-configure-ssl.md).</span><span class="sxs-lookup"><span data-stu-id="62713-138">For details on how to configure SSL, See [Using SSL with Azure Database for MySQL](howto-configure-ssl.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="62713-139">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="62713-139">Next steps</span></span>
<span data-ttu-id="62713-140">Mer information om anslutningssträngar finns i [anslutningssträngar](howto-connection-string.md).</span><span class="sxs-lookup"><span data-stu-id="62713-140">For more information about connection strings, refer to [Connection Strings](howto-connection-string.md).</span></span>
