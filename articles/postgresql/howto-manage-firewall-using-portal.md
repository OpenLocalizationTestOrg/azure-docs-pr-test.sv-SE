---
title: "aaaCreate och hantera Azure-databas för PostgreSQL brandväggsregler med hello Azure-portalen | Microsoft Docs"
description: "Skapa och hantera Azure-databas för PostgreSQL brandväggsregler med hello Azure-portalen"
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 05/10/2017
ms.openlocfilehash: 6a41a077168657769e442401e9df9931aa809240
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-azure-database-for-postgresql-firewall-rules-using-hello-azure-portal"></a><span data-ttu-id="0ef4a-103">Skapa och hantera Azure-databas för PostgreSQL brandväggsregler med hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="0ef4a-103">Create and manage Azure Database for PostgreSQL firewall rules using hello Azure portal</span></span>
<span data-ttu-id="0ef4a-104">Brandväggsregler på servernivå aktivera administratörer tooaccess en Azure-databas för PostgreSQL-Server från en angiven IP-adress eller intervall av IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="0ef4a-104">Server-level firewall rules enable administrators tooaccess an Azure Database for PostgreSQL Server from a specified IP address or range of IP addresses.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="0ef4a-105">Krav</span><span class="sxs-lookup"><span data-stu-id="0ef4a-105">Prerequisites</span></span>
<span data-ttu-id="0ef4a-106">toostep via den här hur tooguide behöver du:</span><span class="sxs-lookup"><span data-stu-id="0ef4a-106">toostep through this how-tooguide, you need:</span></span>
- <span data-ttu-id="0ef4a-107">En server [skapa en Azure-databas för PostgreSQL](quickstart-create-server-database-portal.md)</span><span class="sxs-lookup"><span data-stu-id="0ef4a-107">A server [Create an Azure Database for PostgreSQL](quickstart-create-server-database-portal.md)</span></span>

## <a name="create-a-server-level-firewall-rule-in-hello-azure-portal"></a><span data-ttu-id="0ef4a-108">Skapa en brandväggsregel på servernivå i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="0ef4a-108">Create a server-level firewall rule in hello Azure portal</span></span>
1. <span data-ttu-id="0ef4a-109">På hello serverblad PostgreSQL, under inställningar rubrik, klickar du på **anslutningssäkerhet** tooopen hello anslutningssäkerhet bladet för hello Azure-databas för PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="0ef4a-109">On hello PostgreSQL server blade, under Settings heading, click **Connection Security** tooopen hello Connection Security blade for hello Azure Database for PostgreSQL.</span></span>

  ![Azure portal – Klicka på anslutningssäkerhet](./media/howto-manage-firewall-using-portal/1-connection-security.png)

2. <span data-ttu-id="0ef4a-111">Klicka på **Lägg till Min IP** hello i verktygsfältet.</span><span class="sxs-lookup"><span data-stu-id="0ef4a-111">Click **Add My IP** on hello toolbar.</span></span> <span data-ttu-id="0ef4a-112">Detta skapar automatiskt en regel med hello IP-adressen för datorn som uppfattade av hello Azure system.</span><span class="sxs-lookup"><span data-stu-id="0ef4a-112">This will create a rule automatically with hello IP address of your computer, as perceived by hello Azure system.</span></span>

  ![Azure portal – Klicka på Lägg till Min IP](./media/howto-manage-firewall-using-portal/2-add-my-ip.png)

3. <span data-ttu-id="0ef4a-114">Kontrollera din IP-adress innan du sparar hello konfiguration.</span><span class="sxs-lookup"><span data-stu-id="0ef4a-114">Verify your IP address before saving hello configuration.</span></span> <span data-ttu-id="0ef4a-115">I vissa situationer kan följas av Azure-portalen hello IP-adress skiljer sig från hello IP-adress som används för att komma åt när hello internet och Azure-servrar.</span><span class="sxs-lookup"><span data-stu-id="0ef4a-115">In some situations, hello IP address observed by Azure portal differs from hello IP address used when accessing hello internet and Azure servers.</span></span> <span data-ttu-id="0ef4a-116">Därför kanske du måste toochange hello första IP- och slut-IP toomake hello regeln funktion som förväntat.</span><span class="sxs-lookup"><span data-stu-id="0ef4a-116">Therefore, you may need toochange hello Start IP and End IP toomake hello rule function as expected.</span></span>
<span data-ttu-id="0ef4a-117">Använd en sökmotor eller andra online verktyget toocheck din egen IP-adress (till exempel Bing search som ”hur är Min IP”).</span><span class="sxs-lookup"><span data-stu-id="0ef4a-117">Use a search engine or other online tool toocheck your own IP address (for example, Bing search "what is my IP").</span></span>

  ![Bing-Sök efter vad som är Min IP](./media/howto-manage-firewall-using-portal/3-what-is-my-ip.png)

4. <span data-ttu-id="0ef4a-119">Lägg till ytterligare adressintervall.</span><span class="sxs-lookup"><span data-stu-id="0ef4a-119">Add additional address ranges.</span></span> <span data-ttu-id="0ef4a-120">Du kan ange en IP-adress eller ett adressintervall i hello regler för hello Azure-databas för PostgreSQL-brandväggen.</span><span class="sxs-lookup"><span data-stu-id="0ef4a-120">In hello rules for hello Azure Database for PostgreSQL firewall, you can specify a single IP address, or a range of addresses.</span></span> <span data-ttu-id="0ef4a-121">Om du vill toolimit hello regeln tooone enskild IP-adress, typen hello samma adress i hello-fältet för första IP- och slut-IP.</span><span class="sxs-lookup"><span data-stu-id="0ef4a-121">If you want toolimit hello rule tooone single IP address, type hello same address in hello field for Start IP and End IP.</span></span> <span data-ttu-id="0ef4a-122">Om du öppnar hello brandväggen kan administratörer och användare toologin tooany databasen på hello PostgreSQL server toowhich de har giltiga autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="0ef4a-122">Opening hello firewall enables administrators and users toologin tooany database on hello PostgreSQL server toowhich they have valid credentials.</span></span>

  ![<span data-ttu-id="0ef4a-123">Azure portal – brandväggsregler</span><span class="sxs-lookup"><span data-stu-id="0ef4a-123">Azure portal - firewall rules</span></span> ](./media/howto-manage-firewall-using-portal/4-specify-addresses.png)

5. <span data-ttu-id="0ef4a-124">Klicka på **spara** på hello verktygsfältet toosave den här brandväggsregeln på servernivå.</span><span class="sxs-lookup"><span data-stu-id="0ef4a-124">Click **Save** on hello toolbar toosave this server-level firewall rule.</span></span> <span data-ttu-id="0ef4a-125">Vänta tills hello bekräftelse att hello uppdatering toohello brandväggsregler lyckades.</span><span class="sxs-lookup"><span data-stu-id="0ef4a-125">Wait for hello confirmation that hello update toohello firewall rules was successful.</span></span>

  ![Azure portal – Klicka på Spara](./media/howto-manage-firewall-using-portal/5-save-firewall-rule.png)


## <a name="manage-existing-server-level-firewall-rules-through-hello-azure-portal"></a><span data-ttu-id="0ef4a-127">Hantera befintliga servernivå brandväggsregler via hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="0ef4a-127">Manage existing server-level firewall rules through hello Azure portal</span></span>
<span data-ttu-id="0ef4a-128">Upprepa hello steg toomanage hello brandväggsregler.</span><span class="sxs-lookup"><span data-stu-id="0ef4a-128">Repeat hello steps toomanage hello firewall rules.</span></span>
* <span data-ttu-id="0ef4a-129">tooadd hello aktuella datorn, klicka på hello för + **Lägg till Min IP**.</span><span class="sxs-lookup"><span data-stu-id="0ef4a-129">tooadd hello current computer, click hello button too+ **Add My IP**.</span></span> <span data-ttu-id="0ef4a-130">Klicka på **spara** toosave hello ändringar.</span><span class="sxs-lookup"><span data-stu-id="0ef4a-130">Click **Save** toosave hello changes.</span></span>
* <span data-ttu-id="0ef4a-131">tooadd ytterligare IP-adresser, Skriv i hello Regelnamn, starta IP-adress och sista IP-adress.</span><span class="sxs-lookup"><span data-stu-id="0ef4a-131">tooadd additional IP addresses, type in hello Rule Name, Start IP Address, and End IP Address.</span></span> <span data-ttu-id="0ef4a-132">Klicka på **spara** toosave hello ändringar.</span><span class="sxs-lookup"><span data-stu-id="0ef4a-132">Click **Save** toosave hello changes.</span></span>
* <span data-ttu-id="0ef4a-133">toomodify en befintlig regel, klicka på någon av hello fält i hello regeln och ändra.</span><span class="sxs-lookup"><span data-stu-id="0ef4a-133">toomodify an existing rule, click any of hello fields in hello rule and modify.</span></span> <span data-ttu-id="0ef4a-134">Klicka på **spara** toosave hello ändringar.</span><span class="sxs-lookup"><span data-stu-id="0ef4a-134">Click **Save** toosave hello changes.</span></span>
* <span data-ttu-id="0ef4a-135">toodelete en befintlig regel hello punkter [...] och klicka på Ta bort ta bort hello regel.</span><span class="sxs-lookup"><span data-stu-id="0ef4a-135">toodelete an existing rule, click hello ellipsis […] and click Delete remove hello rule.</span></span> <span data-ttu-id="0ef4a-136">Klicka på **spara** toosave hello ändringar.</span><span class="sxs-lookup"><span data-stu-id="0ef4a-136">Click **Save** toosave hello changes.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0ef4a-137">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0ef4a-137">Next steps</span></span>
- <span data-ttu-id="0ef4a-138">På liknande sätt kan du skapa skript för[skapa och hantera Azure-databas för PostgreSQL brandväggsregler med hjälp av Azure CLI](howto-manage-firewall-using-cli.md)</span><span class="sxs-lookup"><span data-stu-id="0ef4a-138">Similarly, you can script too[Create and manage Azure Database for PostgreSQL firewall rules using Azure CLI](howto-manage-firewall-using-cli.md)</span></span>
- <span data-ttu-id="0ef4a-139">Hjälp med att ansluta tooan Azure-databas för PostgreSQL-servern finns [anslutningsbibliotek för Azure-databas för PostgreSQL](concepts-connection-libraries.md)</span><span class="sxs-lookup"><span data-stu-id="0ef4a-139">For help in connecting tooan Azure Database for PostgreSQL server, see [Connection libraries for Azure Database for PostgreSQL](concepts-connection-libraries.md)</span></span>
