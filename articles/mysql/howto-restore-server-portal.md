---
title: "Återställa en Server i Azure-databas för MySQL | Microsoft Docs"
description: "Den här artikeln beskriver hur du återställer en server i Azure-databas för MySQL med Azure-portalen."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 05/10/2017
ms.openlocfilehash: 8c06dce534b628a602127fd02b152c8e04e02cc4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-backup-and-restore-a-server-in-azure-database-for-mysql-using-the-azure-portal"></a><span data-ttu-id="dba83-103">Säkerhetskopiera och återställa en server i Azure-databas för MySQL med Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="dba83-103">How To Backup and Restore a server in Azure Database for MySQL using the Azure portal</span></span>

## <a name="backup-happens-automatically"></a><span data-ttu-id="dba83-104">Säkerhetskopieringen sker automatiskt</span><span class="sxs-lookup"><span data-stu-id="dba83-104">Backup happens Automatically</span></span>
<span data-ttu-id="dba83-105">När du använder Azure-databas för MySQL görs databastjänsten en säkerhetskopia av tjänsten var femte minut.</span><span class="sxs-lookup"><span data-stu-id="dba83-105">When using Azure Database for MySQL, the database service automatically makes a backup of the service every 5 minutes.</span></span> 

<span data-ttu-id="dba83-106">Säkerhetskopiorna är tillgängliga för 7 dagar vid användning av grundläggande nivån och 35 dagar när du använder standardnivån.</span><span class="sxs-lookup"><span data-stu-id="dba83-106">The backups are available for 7 days when using Basic Tier, and 35 days when using Standard Tier.</span></span> <span data-ttu-id="dba83-107">Mer information finns i [Azure-databas för MySQL-servicenivåer](concepts-service-tiers.md)</span><span class="sxs-lookup"><span data-stu-id="dba83-107">For more information, see [Azure Database for MySQL service tiers](concepts-service-tiers.md)</span></span>

<span data-ttu-id="dba83-108">Med hjälp av funktionen för automatisk säkerhetskopiering kan du återställa servern och alla databaser i en ny server till en tidigare punkt i tidpunkt.</span><span class="sxs-lookup"><span data-stu-id="dba83-108">Using this automatic backup feature you may restore the server and all its databases into a new server to an earlier point-in-time.</span></span>

## <a name="restore-in-the-azure-portal"></a><span data-ttu-id="dba83-109">Återställa i Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="dba83-109">Restore in the Azure portal</span></span>
<span data-ttu-id="dba83-110">Azure MySQL-databas kan du återställa servern till en plats och till att en ny kopia av servern.</span><span class="sxs-lookup"><span data-stu-id="dba83-110">Azure Database for MySQL allows you to restore the server back to a point in time and into to a new copy of the server.</span></span> <span data-ttu-id="dba83-111">Du kan använda den här nya servern när du återställer data.</span><span class="sxs-lookup"><span data-stu-id="dba83-111">You can use this new server to recover your data.</span></span> 

<span data-ttu-id="dba83-112">Till exempel om en tabell av misstag kan släppas kl. tolv idag, du återställa tiden innan på dagen och hämta saknas tabellen och data från den nya kopian av servern.</span><span class="sxs-lookup"><span data-stu-id="dba83-112">For example, if a table was accidentally dropped at noon today, you could restore to the time just before noon and retrieve the missing table and data from that new copy of the server.</span></span>

<span data-ttu-id="dba83-113">Följande steg återställa exempelserver till en tidpunkt:</span><span class="sxs-lookup"><span data-stu-id="dba83-113">The following steps restore the sample server to a point in time:</span></span>

1. <span data-ttu-id="dba83-114">Logga in på den [Azure-portalen](https://portal.azure.com/)</span><span class="sxs-lookup"><span data-stu-id="dba83-114">Sign into the [Azure portal](https://portal.azure.com/)</span></span>

2. <span data-ttu-id="dba83-115">Hitta din Azure-databas för MySQL-servern.</span><span class="sxs-lookup"><span data-stu-id="dba83-115">Locate your Azure Database for MySQL server.</span></span> <span data-ttu-id="dba83-116">I den vänstra rutan, Välj **alla resurser**, markera din server i listan.</span><span class="sxs-lookup"><span data-stu-id="dba83-116">In the left pane, select **All resources**, then select your server from the list.</span></span>

3.  <span data-ttu-id="dba83-117">Överst översikt serverblad klickar du på **återställa** i verktygsfältet.</span><span class="sxs-lookup"><span data-stu-id="dba83-117">On the top of the server overview blade, click **Restore** on the toolbar.</span></span> <span data-ttu-id="dba83-118">Återställ blad öppnas.</span><span class="sxs-lookup"><span data-stu-id="dba83-118">The Restore blade opens.</span></span>
<span data-ttu-id="dba83-119">![Klicka på återställningsknappen](./media/howto-restore-server-portal/click-restore-button.png)</span><span class="sxs-lookup"><span data-stu-id="dba83-119">![click restore button](./media/howto-restore-server-portal/click-restore-button.png)</span></span>

4. <span data-ttu-id="dba83-120">Fyll i formuläret återställning med informationen som krävs:</span><span class="sxs-lookup"><span data-stu-id="dba83-120">Fill out the Restore form with the required information:</span></span>

- <span data-ttu-id="dba83-121">**Återställningspunkt (UTC)**: med hjälp av Väljaren för datum och tid Väljaren väljer du en punkt i tid att återställa till.</span><span class="sxs-lookup"><span data-stu-id="dba83-121">**Restore point (UTC)**: Using the Date picker and time picker, select a point-in-time to restore to.</span></span> <span data-ttu-id="dba83-122">Den angivna tiden är i UTC-format, så du behöver förmodligen konvertera lokal tid till UTC.</span><span class="sxs-lookup"><span data-stu-id="dba83-122">The time specified is in UTC format, so you likely need to convert the local time into UTC.</span></span>
- <span data-ttu-id="dba83-123">**Återställ till en ny server**: Ange ett nytt servernamn att återställa den befintliga servern till.</span><span class="sxs-lookup"><span data-stu-id="dba83-123">**Restore to new server**: Provide a new server name to restore the existing server into.</span></span>
- <span data-ttu-id="dba83-124">**Plats**: region valet automatiskt fylls med källan server region och kan inte ändras.</span><span class="sxs-lookup"><span data-stu-id="dba83-124">**Location**: The region choice automatically populates with the source server region, and cannot be changed.</span></span>
- <span data-ttu-id="dba83-125">**Prisnivån**: valet av prisnivå nivå automatiskt fylls med samma prisnivån som källservern och kan inte ändras här.</span><span class="sxs-lookup"><span data-stu-id="dba83-125">**Pricing tier**: The pricing tier choice automatically populates with the same pricing tier as the source server, and cannot be changed here.</span></span> 
<span data-ttu-id="dba83-126">![PITR-återställning](./media/howto-restore-server-portal/pitr-restore.png)</span><span class="sxs-lookup"><span data-stu-id="dba83-126">![PITR Restore](./media/howto-restore-server-portal/pitr-restore.png)</span></span>

5. <span data-ttu-id="dba83-127">Klicka på **OK** att återställa servern att återställa till en tidpunkt.</span><span class="sxs-lookup"><span data-stu-id="dba83-127">Click **OK** to restore the server to restore to a point in time.</span></span> 

6. <span data-ttu-id="dba83-128">Leta upp den nya servern som skapades för att kontrollera databaserna har återställts korrekt när återställningen är klar.</span><span class="sxs-lookup"><span data-stu-id="dba83-128">After the restore finishes, locate the new server that was created to verify the databases were restored as expected.</span></span>

## <a name="next-steps"></a><span data-ttu-id="dba83-129">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="dba83-129">Next steps</span></span>
- [<span data-ttu-id="dba83-130">Anslutningsbibliotek för Azure-databas för MySQL</span><span class="sxs-lookup"><span data-stu-id="dba83-130">Connection libraries for Azure Database for MySQL</span></span>](concepts-connection-libraries.md)