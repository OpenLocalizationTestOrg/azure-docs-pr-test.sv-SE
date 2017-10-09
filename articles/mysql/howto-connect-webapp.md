---
title: "aaaConnect befintliga Azure App Service tooAzure för MySQL-databas | Microsoft Docs"
description: "Instruktioner för hur tooproperly ansluter en befintlig Azure App Service-tooAzure databas för MySQL"
services: mysql
author: v-chenyh
ms.author: v-chenyh
editor: jasonwhowell
manager: jhubbard
ms.service: mysql-database
ms.topic: article
ms.date: 05/23/2017
ms.openlocfilehash: 6d5b16f316e186d665370adcd8b7c7bb38c8d51a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-an-existing-azure-app-service-tooazure-database-for-mysql-server"></a><span data-ttu-id="9da51-103">Anslut en befintlig Azure App Service-tooAzure databas för MySQL-server</span><span class="sxs-lookup"><span data-stu-id="9da51-103">Connect an existing Azure App Service tooAzure Database for MySQL server</span></span>
<span data-ttu-id="9da51-104">Det här dokumentet förklarar hur tooconnect en befintlig Azure App Service-tooyour Azure-databas för MySQL-servern.</span><span class="sxs-lookup"><span data-stu-id="9da51-104">This document explains how tooconnect an existing Azure App Service tooyour Azure Database for MySQL server.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="9da51-105">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="9da51-105">Before you begin</span></span>
<span data-ttu-id="9da51-106">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="9da51-106">Log in toohello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="9da51-107">Skapa en Azure-databas för MySQL-servern.</span><span class="sxs-lookup"><span data-stu-id="9da51-107">Create an Azure Database for MySQL server.</span></span> <span data-ttu-id="9da51-108">Mer information finns för[hur toocreate Azure-databas för MySQL-servern från portalen](quickstart-create-mysql-server-database-using-azure-portal.md) eller [hur toocreate Azure-databas för MySQL-servern med hjälp av CLI](quickstart-create-mysql-server-database-using-azure-cli.md).</span><span class="sxs-lookup"><span data-stu-id="9da51-108">For details, refer too[How toocreate Azure Database for MySQL server from Portal](quickstart-create-mysql-server-database-using-azure-portal.md) or [How toocreate Azure Database for MySQL server using CLI](quickstart-create-mysql-server-database-using-azure-cli.md).</span></span>

<span data-ttu-id="9da51-109">Det finns två lösningar tooenable åtkomst från en Azure App Service-tooan Azure-databas för MySQL.</span><span class="sxs-lookup"><span data-stu-id="9da51-109">Currently there are two solutions tooenable access from an Azure App Service tooan Azure Database for MySQL.</span></span> <span data-ttu-id="9da51-110">Båda lösningarna omfattar att konfigurera brandväggsregler på servernivå.</span><span class="sxs-lookup"><span data-stu-id="9da51-110">Both solutions involve setting up server-level firewall rules.</span></span>

## <a name="solution-1---create-a-firewall-rule-tooallow-all-ips"></a><span data-ttu-id="9da51-111">Lösning 1 - skapa en brandvägg regeln tooallow alla IP-adresser</span><span class="sxs-lookup"><span data-stu-id="9da51-111">Solution 1 - Create a firewall rule tooallow all IPs</span></span>
<span data-ttu-id="9da51-112">Azure-databas för MySQL skyddar åtkomst med hjälp av en brandvägg tooprotect dina data.</span><span class="sxs-lookup"><span data-stu-id="9da51-112">Azure Database for MySQL provides access security using a firewall tooprotect your data.</span></span> <span data-ttu-id="9da51-113">När du ansluter från Azure App Service-tooAzure databas för MySQL-server, Tänk på att hello utgående är IP-adresser Apptjänst dynamiska.</span><span class="sxs-lookup"><span data-stu-id="9da51-113">When connecting from an Azure App Service tooAzure Database for MySQL server, keep in mind that hello outbound IPs of App Service are dynamic in nature.</span></span> 

<span data-ttu-id="9da51-114">tooensure hello tillgängligheten för Azure App Service, bör du använda den här lösningen tooallow alla IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="9da51-114">tooensure hello availability of your Azure App Service, we recommend using this solution tooallow ALL IPs.</span></span>

> [!NOTE]
> <span data-ttu-id="9da51-115">Microsoft arbetar för en långsiktig lösning tooavoid så att alla IP-adresser för Azure-tjänster tooconnect tooAzure databas för MySQL.</span><span class="sxs-lookup"><span data-stu-id="9da51-115">Microsoft is working for a long-term solution tooavoid allowing all IPs for Azure services tooconnect tooAzure Database for MySQL.</span></span>

1. <span data-ttu-id="9da51-116">På hello serverblad MySQL, under inställningar rubrik, klickar du på **anslutningssäkerhet** tooopen hello anslutningssäkerhet bladet för hello Azure för MySQL-databas.</span><span class="sxs-lookup"><span data-stu-id="9da51-116">On hello MySQL server blade, under Settings heading, click **Connection Security** tooopen hello Connection Security blade for hello Azure Database for MySQL.</span></span>

   ![Azure portal – Klicka på anslutningssäkerhet](./media/howto-manage-firewall-using-portal/1-connection-security.png)

2. <span data-ttu-id="9da51-118">Ange **REGELNAMN**, **START-IP**, och **sista IP**.</span><span class="sxs-lookup"><span data-stu-id="9da51-118">Enter **RULE NAME**, **START IP**, and **END IP**.</span></span> <span data-ttu-id="9da51-119">Klicka sedan på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="9da51-119">Then click **Save**.</span></span>
   - <span data-ttu-id="9da51-120">Regelnamnet: Tillåt-alla-IP-adresser</span><span class="sxs-lookup"><span data-stu-id="9da51-120">Rule name: Allow-All-IPs</span></span>
   - <span data-ttu-id="9da51-121">Start-IP: 0.0.0.0</span><span class="sxs-lookup"><span data-stu-id="9da51-121">Start IP: 0.0.0.0</span></span>
   - <span data-ttu-id="9da51-122">Avsluta IP: 255.255.255.255</span><span class="sxs-lookup"><span data-stu-id="9da51-122">End IP: 255.255.255.255</span></span>

   ![Azure portal – Lägg till alla IP-adresser](./media/howto-connect-webapp/1_2-add-all-ips.png)

## <a name="solution-2---create-a-firewall-rule-tooexplicitly-allow-outbound-ips"></a><span data-ttu-id="9da51-124">Lösning 2 – Skapa en brandvägg regeln tooexplicitly Tillåt utgående IP-adresser</span><span class="sxs-lookup"><span data-stu-id="9da51-124">Solution 2 - Create a firewall rule tooexplicitly allow outbound IPs</span></span>
<span data-ttu-id="9da51-125">Du kan uttryckligen lägga till alla hello utgående IP-adresser i Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="9da51-125">You can explicitly add all hello outbound IPs of your Azure App Service.</span></span>

1. <span data-ttu-id="9da51-126">På hello App Service egenskapsbladet, visa din **utgående IP-adress**.</span><span class="sxs-lookup"><span data-stu-id="9da51-126">On hello App Service Properties blade, view your **OUTBOUND IP ADDRESS**.</span></span>

   ![Azure portal – Visa utgående IP-adresser](./media/howto-connect-webapp/2_1-outbound-ip-address.png)

2. <span data-ttu-id="9da51-128">Lägg till utgående IP-adresser i taget på hello MySQL-anslutning säkerhet bladet.</span><span class="sxs-lookup"><span data-stu-id="9da51-128">On hello MySQL Connection security blade, add outbound IPs one by one.</span></span>

   ![Azure portal – Lägg till explicita IP-adresser](./media/howto-connect-webapp/2_2-add-explicit-ips.png)

3. <span data-ttu-id="9da51-130">Kom ihåg för**spara** brandväggsreglerna.</span><span class="sxs-lookup"><span data-stu-id="9da51-130">Remember too**Save** your firewall rules.</span></span>

<span data-ttu-id="9da51-131">Även om hello Azure App service försöker tookeep IP-adresser konstant över tid, finns det fall där hello IP-adresser kan ändra.</span><span class="sxs-lookup"><span data-stu-id="9da51-131">Though hello Azure App service attempts tookeep IP addresses constant over time, there are cases where hello IP addresses may change.</span></span> <span data-ttu-id="9da51-132">Till exempel hello när app återanvänder en skalningsåtgärden inträffar eller när nya datorer läggs till i Azure regionala data centers tooincrease hello kapacitet.</span><span class="sxs-lookup"><span data-stu-id="9da51-132">For example, when hello app recycles or a scale operation occurs, or when new machines are added in Azure regional data centers tooincrease hello capacity.</span></span> <span data-ttu-id="9da51-133">När hello IP-adresserna ändras, hello app kan uppleva avbrottstid hello händelsen som den kan inte längre ansluta toohello MySQL-servern.</span><span class="sxs-lookup"><span data-stu-id="9da51-133">When hello IP addresses change, hello app could experience downtime in hello event it can no longer connect toohello MySQL server.</span></span> <span data-ttu-id="9da51-134">Överväg att denna möjlighet när du väljer ett av hello föregående lösningar.</span><span class="sxs-lookup"><span data-stu-id="9da51-134">Consider this potential when choosing one of hello preceding solutions.</span></span>

## <a name="ssl-configuration"></a><span data-ttu-id="9da51-135">SSL-konfiguration</span><span class="sxs-lookup"><span data-stu-id="9da51-135">SSL configuration</span></span>
<span data-ttu-id="9da51-136">Azure MySQL-databas har SSL aktiverat som standard.</span><span class="sxs-lookup"><span data-stu-id="9da51-136">Azure Database for MySQL has SSL Enabled by default.</span></span> <span data-ttu-id="9da51-137">Om programmet inte använder SSL tooconnect toohello databas måste toodisable SSL på MySQL-servern.</span><span class="sxs-lookup"><span data-stu-id="9da51-137">If your application is not using SSL tooconnect toohello database, then you need toodisable SSL on MySQL server.</span></span> <span data-ttu-id="9da51-138">Mer information om hur tooconfigure SSL, se [med hjälp av SSL med Azure-databas för MySQL](howto-configure-ssl.md).</span><span class="sxs-lookup"><span data-stu-id="9da51-138">For details on how tooconfigure SSL, See [Using SSL with Azure Database for MySQL](howto-configure-ssl.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="9da51-139">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9da51-139">Next steps</span></span>
<span data-ttu-id="9da51-140">Mer information om anslutningssträngar finns för[anslutningssträngar](howto-connection-string.md).</span><span class="sxs-lookup"><span data-stu-id="9da51-140">For more information about connection strings, refer too[Connection Strings](howto-connection-string.md).</span></span>
