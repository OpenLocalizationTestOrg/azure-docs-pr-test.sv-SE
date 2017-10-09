---
title: "aaaCreate och hantera Azure-databas för MySQL brandväggsregler med hello Azure-portalen | Microsoft Docs"
description: "Skapa och hantera Azure-databas för MySQL brandväggsregler med hello Azure-portalen"
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 06/13/2017
ms.openlocfilehash: 30dbdde4299df564fbf6581419e908186fe3b823
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-azure-database-for-mysql-firewall-rules-using-hello-azure-portal"></a><span data-ttu-id="569b0-103">Skapa och hantera Azure-databas för MySQL brandväggsregler med hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="569b0-103">Create and manage Azure Database for MySQL firewall rules using hello Azure portal</span></span>
<span data-ttu-id="569b0-104">Brandväggsregler på servernivå aktivera administratörer tooaccess en Azure-databas för MySQL-Server från en angiven IP-adress eller intervall av IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="569b0-104">Server-level firewall rules enable administrators tooaccess an Azure Database for MySQL Server from a specified IP address or range of IP addresses.</span></span> 

## <a name="create-a-server-level-firewall-rule-in-hello-azure-portal"></a><span data-ttu-id="569b0-105">Skapa en brandväggsregel på servernivå i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="569b0-105">Create a server-level firewall rule in hello Azure portal</span></span>

1. <span data-ttu-id="569b0-106">På hello serverblad MySQL, under inställningar rubrik, klickar du på **anslutningssäkerhet** tooopen hello anslutningssäkerhet bladet för hello Azure för MySQL-databas.</span><span class="sxs-lookup"><span data-stu-id="569b0-106">On hello MySQL server blade, under Settings heading, click **Connection Security** tooopen hello Connection Security blade for hello Azure Database for MySQL.</span></span>

   ![Azure portal – Klicka på anslutningssäkerhet](./media/howto-manage-firewall-using-portal/1-connection-security.png)

2. <span data-ttu-id="569b0-108">Klicka på **Lägg till Min IP** på hello verktygsfältet toocreate en regel med hello IP-adressen för datorn som uppfattas av hello Azure system.</span><span class="sxs-lookup"><span data-stu-id="569b0-108">Click **Add My IP** on hello toolbar toocreate a rule with hello IP address of your computer, as perceived by hello Azure system.</span></span>

   ![Azure portal – Klicka på Lägg till Min IP](./media/howto-manage-firewall-using-portal/2-add-my-ip.png)

3. <span data-ttu-id="569b0-110">Kontrollera din IP-adress innan du sparar hello konfiguration.</span><span class="sxs-lookup"><span data-stu-id="569b0-110">Verify your IP address before saving hello configuration.</span></span> <span data-ttu-id="569b0-111">I vissa situationer kan följas av Azure-portalen hello IP-adress skiljer sig från hello IP-adress som används för att komma åt när hello internet och Azure-servrar.</span><span class="sxs-lookup"><span data-stu-id="569b0-111">In some situations, hello IP address observed by Azure portal differs from hello IP address used when accessing hello internet and Azure servers.</span></span> <span data-ttu-id="569b0-112">Därför kanske du måste toochange hello första IP- och slut-IP toomake hello regeln funktion som förväntat.</span><span class="sxs-lookup"><span data-stu-id="569b0-112">Therefore, you may need toochange hello Start IP and End IP toomake hello rule function as expected.</span></span>

   <span data-ttu-id="569b0-113">Använd en sökmotor eller andra online verktyget toocheck din egen IP-adress (till exempel Sök ”vad är IP-adress”).</span><span class="sxs-lookup"><span data-stu-id="569b0-113">Use a search engine or other online tool toocheck your own IP address (for example, search "what is my IP address").</span></span>

   ![Bing för vad som är Min IP](./media/howto-manage-firewall-using-portal/3-what-is-my-ip.png)

4. <span data-ttu-id="569b0-115">Lägg till ytterligare adressintervall.</span><span class="sxs-lookup"><span data-stu-id="569b0-115">Add additional address ranges.</span></span> <span data-ttu-id="569b0-116">Du kan ange en IP-adress eller ett adressintervall i hello regler för hello Azure-databas för MySQL-brandväggen.</span><span class="sxs-lookup"><span data-stu-id="569b0-116">In hello rules for hello Azure Database for MySQL firewall, you can specify a single IP address, or a range of addresses.</span></span> <span data-ttu-id="569b0-117">Om du vill toolimit hello regeln tooone enskild IP-adress, typen hello samma adress i hello-fältet för första IP- och slut-IP.</span><span class="sxs-lookup"><span data-stu-id="569b0-117">If you want toolimit hello rule tooone single IP address, type hello same address in hello field for Start IP and End IP.</span></span> <span data-ttu-id="569b0-118">Öppna hello brandväggen kan administratörer och användare tooaccess alla databaser på hello MySQL server toowhich de har giltiga autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="569b0-118">Opening hello firewall enables administrators and users tooaccess any database on hello MySQL server toowhich they have valid credentials.</span></span>

   ![<span data-ttu-id="569b0-119">Azure portal – brandväggsregler</span><span class="sxs-lookup"><span data-stu-id="569b0-119">Azure portal - firewall rules</span></span> ](./media/howto-manage-firewall-using-portal/5-specify-addresses.png)


5. <span data-ttu-id="569b0-120">Klicka på **spara** på hello verktygsfältet toosave den här brandväggsregeln på servernivå.</span><span class="sxs-lookup"><span data-stu-id="569b0-120">Click **Save** on hello toolbar toosave this server-level firewall rule.</span></span> <span data-ttu-id="569b0-121">Vänta tills hello bekräftelse att hello uppdatering toohello brandväggsregler lyckades.</span><span class="sxs-lookup"><span data-stu-id="569b0-121">Wait for hello confirmation that hello update toohello firewall rules was successful.</span></span>

   ![Azure portal – Klicka på Spara](./media/howto-manage-firewall-using-portal/4-save-firewall-rule.png)

## <a name="manage-existing-server-level-firewall-rules-through-hello-azure-portal"></a><span data-ttu-id="569b0-123">Hantera befintliga servernivå brandväggsregler via hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="569b0-123">Manage existing server-level firewall rules through hello Azure portal</span></span>
<span data-ttu-id="569b0-124">Upprepa hello steg toomanage hello brandväggsregler.</span><span class="sxs-lookup"><span data-stu-id="569b0-124">Repeat hello steps toomanage hello firewall rules.</span></span>
* <span data-ttu-id="569b0-125">tooadd hello aktuella datorn, klickar du på **+ Lägg till Min IP**.</span><span class="sxs-lookup"><span data-stu-id="569b0-125">tooadd hello current computer, click **+ Add My IP**.</span></span>
* <span data-ttu-id="569b0-126">tooadd ytterligare IP-adresser, Skriv i hello **REGELNAMN**, **första IP-**, och **sista IP**.</span><span class="sxs-lookup"><span data-stu-id="569b0-126">tooadd additional IP addresses, type in hello **RULE NAME**, **START IP**, and **END IP**.</span></span>
* <span data-ttu-id="569b0-127">toomodify en befintlig regel, klicka på någon av hello fält i hello regeln och ändra.</span><span class="sxs-lookup"><span data-stu-id="569b0-127">toomodify an existing rule, click any of hello fields in hello rule and modify.</span></span>
* <span data-ttu-id="569b0-128">toodelete en befintlig regel på hello punkter [...] och klicka på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="569b0-128">toodelete an existing rule, click hello ellipsis […] and click **Delete**.</span></span>
* <span data-ttu-id="569b0-129">Klicka på **spara** toosave hello ändringar.</span><span class="sxs-lookup"><span data-stu-id="569b0-129">Click **Save** toosave hello changes.</span></span>

## <a name="next-steps"></a><span data-ttu-id="569b0-130">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="569b0-130">Next steps</span></span>
- <span data-ttu-id="569b0-131">Hjälp med att ansluta tooan Azure-databas för MySQL-servern finns [anslutningsbibliotek för Azure-databas för MySQL](./concepts-connection-libraries.md)</span><span class="sxs-lookup"><span data-stu-id="569b0-131">For help in connecting tooan Azure Database for MySQL server, see [Connection libraries for Azure Database for MySQL](./concepts-connection-libraries.md)</span></span>
