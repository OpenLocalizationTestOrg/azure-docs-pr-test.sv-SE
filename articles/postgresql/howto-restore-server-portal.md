---
title: "Hur tooRestore en Server i Azure-databas för PostgreSQL | Microsoft Docs"
description: "Den här artikeln beskriver hur toorestore en server i Azure-databas för att använda PostgreSQL hello Azure-portalen."
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 07/20/2017
ms.openlocfilehash: bc7351f384607397806d837afd3e1d7a26575e0e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toobackup-and-restore-a-server-in-azure-database-for-postgresql-using-hello-azure-portal"></a><span data-ttu-id="d61d8-103">Hur tooBackup och återställa en server i Azure-databas för att använda PostgreSQL hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="d61d8-103">How tooBackup and Restore a server in Azure Database for PostgreSQL using hello Azure portal</span></span>

## <a name="backup-happens-automatically"></a><span data-ttu-id="d61d8-104">Säkerhetskopieringen sker automatiskt</span><span class="sxs-lookup"><span data-stu-id="d61d8-104">Backup happens Automatically</span></span>
<span data-ttu-id="d61d8-105">När du använder Azure-databas för PostgreSQL görs hello databastjänsten en säkerhetskopia av hello tjänsten var femte minut.</span><span class="sxs-lookup"><span data-stu-id="d61d8-105">When using Azure Database for PostgreSQL, hello database service automatically makes a backup of hello service every 5 minutes.</span></span> 

<span data-ttu-id="d61d8-106">hello säkerhetskopior är tillgängliga för 7 dagar vid användning av grundläggande nivån och 35 dagar när du använder standardnivån.</span><span class="sxs-lookup"><span data-stu-id="d61d8-106">hello backups are available for 7 days when using Basic Tier, and 35 days when using Standard Tier.</span></span> <span data-ttu-id="d61d8-107">Mer information finns i [Azure-databas för PostgreSQL-servicenivåer](concepts-service-tiers.md)</span><span class="sxs-lookup"><span data-stu-id="d61d8-107">For more information, see [Azure Database for PostgreSQL service tiers](concepts-service-tiers.md)</span></span>

<span data-ttu-id="d61d8-108">Med hjälp av funktionen för automatisk säkerhetskopiering kan du återställa hello server och alla databaser i en ny server tooan tidigare punkt i tiden.</span><span class="sxs-lookup"><span data-stu-id="d61d8-108">Using this automatic backup feature you may restore hello server and all its databases into a new server tooan earlier point-in-time.</span></span>

## <a name="restore-in-hello-azure-portal"></a><span data-ttu-id="d61d8-109">Återställa hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="d61d8-109">Restore in hello Azure portal</span></span>
<span data-ttu-id="d61d8-110">Azure PostgreSQL-databas kan du toorestore hello servern tillbaka tooa punkt i tiden och till tooa nya kopian av hello server.</span><span class="sxs-lookup"><span data-stu-id="d61d8-110">Azure Database for PostgreSQL allows you toorestore hello server back tooa point in time and into tooa new copy of hello server.</span></span> <span data-ttu-id="d61d8-111">Du kan använda den här nya servern toorecover dina data.</span><span class="sxs-lookup"><span data-stu-id="d61d8-111">You can use this new server toorecover your data.</span></span> 

<span data-ttu-id="d61d8-112">Till exempel om en tabell av misstag kan bort kl. tolv idag, du återställa toohello gång innan på dagen och hämta hello saknas tabellen och data från den nya kopian av hello-server.</span><span class="sxs-lookup"><span data-stu-id="d61d8-112">For example, if a table was accidentally dropped at noon today, you could restore toohello time just before noon and retrieve hello missing table and data from that new copy of hello server.</span></span>

<span data-ttu-id="d61d8-113">hello återställningspunkt följande hello exempel server tooa i tid:</span><span class="sxs-lookup"><span data-stu-id="d61d8-113">hello following steps restore hello sample server tooa point in time:</span></span>
1. <span data-ttu-id="d61d8-114">Logga in på hello [Azure-portalen](https://portal.azure.com/)</span><span class="sxs-lookup"><span data-stu-id="d61d8-114">Sign into hello [Azure portal](https://portal.azure.com/)</span></span>
2. <span data-ttu-id="d61d8-115">Hitta din Azure-databas för PostgreSQL-servern.</span><span class="sxs-lookup"><span data-stu-id="d61d8-115">Locate your Azure Database for PostgreSQL server.</span></span> <span data-ttu-id="d61d8-116">I hello Azure-portalen klickar du på **alla resurser** från hello vänstra menyn och Skriv hello namn som **mypgserver 20170401**, toosearch för din befintliga server.</span><span class="sxs-lookup"><span data-stu-id="d61d8-116">In hello Azure portal, click **All Resources** from hello left-hand menu and type in hello name, such as **mypgserver-20170401**, toosearch for your existing server.</span></span> <span data-ttu-id="d61d8-117">Klicka på hello servernamn som anges i hello sökresultatet.</span><span class="sxs-lookup"><span data-stu-id="d61d8-117">Click hello server name listed in hello search result.</span></span> <span data-ttu-id="d61d8-118">Hej **översikt** sidan för servern öppnas och visar alternativ för ytterligare konfiguration.</span><span class="sxs-lookup"><span data-stu-id="d61d8-118">hello **Overview** page for your server opens and provides options for further configuration.</span></span>

   ![Azure portal – Sök toolocate servern](media/postgresql-howto-restore-server-portal/1-locate.png)

3. <span data-ttu-id="d61d8-120">Klicka på hello överkant hello serverblad översikt **återställa** hello i verktygsfältet.</span><span class="sxs-lookup"><span data-stu-id="d61d8-120">On hello top of hello server overview blade, click **Restore** on hello toolbar.</span></span> <span data-ttu-id="d61d8-121">hello återställning blad öppnas.</span><span class="sxs-lookup"><span data-stu-id="d61d8-121">hello Restore blade opens.</span></span>

   ![Azure-databas för knappen PostgreSQL - översikt – Återställ](./media/postgresql-howto-restore-server-portal/2_server.png)

4. <span data-ttu-id="d61d8-123">Fyll i hello återställning formuläret med hello krävs information:</span><span class="sxs-lookup"><span data-stu-id="d61d8-123">Fill out hello Restore form with hello required information:</span></span>

   ![<span data-ttu-id="d61d8-124">Azure-databas för PostgreSQL - information för återställningspunkter</span><span class="sxs-lookup"><span data-stu-id="d61d8-124">Azure Database for PostgreSQL - Restore information</span></span> ](./media/postgresql-howto-restore-server-portal/3_restore.png)
  - <span data-ttu-id="d61d8-125">**Återställningspunkt**: Välj en i tidpunkt som inträffar innan hello-servern har ändrats</span><span class="sxs-lookup"><span data-stu-id="d61d8-125">**Restore point**: Select a point-in-time that occurs before hello server was changed</span></span>
  - <span data-ttu-id="d61d8-126">**Målservern**: Ange ett nytt servernamn som du vill toorestore till</span><span class="sxs-lookup"><span data-stu-id="d61d8-126">**Target server**: Provide a new server name you want toorestore to</span></span>
  - <span data-ttu-id="d61d8-127">**Plats**: du kan inte välja hello region, som standard är det samma som källservern hello</span><span class="sxs-lookup"><span data-stu-id="d61d8-127">**Location**: You cannot select hello region, by default it is same as hello source server</span></span>
  - <span data-ttu-id="d61d8-128">**Prisnivån**: du kan inte ändra det här värdet när du återställer en server.</span><span class="sxs-lookup"><span data-stu-id="d61d8-128">**Pricing tier**: You cannot change this value when restoring a server.</span></span> <span data-ttu-id="d61d8-129">Det är samma som hello källservern.</span><span class="sxs-lookup"><span data-stu-id="d61d8-129">It is same as hello source server.</span></span> 

5. <span data-ttu-id="d61d8-130">Klicka på **OK** toorestore hello server toorestore tooa tidpunkt.</span><span class="sxs-lookup"><span data-stu-id="d61d8-130">Click **OK** toorestore hello server toorestore tooa point in time.</span></span> 

6. <span data-ttu-id="d61d8-131">Leta upp hello nya server som skapas tooverify hello data har återställts korrekt när hello återställning har slutförts.</span><span class="sxs-lookup"><span data-stu-id="d61d8-131">Once hello restore finishes, locate hello new server that is created tooverify hello data was restored as expected.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d61d8-132">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d61d8-132">Next steps</span></span>
- [<span data-ttu-id="d61d8-133">Anslutningsbibliotek för Azure-databas för PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="d61d8-133">Connection libraries for Azure Database for PostgreSQL</span></span>](concepts-connection-libraries.md)
