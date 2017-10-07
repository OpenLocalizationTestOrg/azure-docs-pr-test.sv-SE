---
title: "aaaHow tooRestore en Server i Azure-databas för MySQL | Microsoft Docs"
description: "Den här artikeln beskriver hur toorestore en server i Azure-databas för att använda MySQL hello Azure-portalen."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 05/10/2017
ms.openlocfilehash: 4b990d5b37c5d4924de9571192b923e3c81094ce
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toobackup-and-restore-a-server-in-azure-database-for-mysql-using-hello-azure-portal"></a><span data-ttu-id="06e25-103">Hur tooBackup och återställa en server i Azure-databas för att använda MySQL hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="06e25-103">How tooBackup and Restore a server in Azure Database for MySQL using hello Azure portal</span></span>

## <a name="backup-happens-automatically"></a><span data-ttu-id="06e25-104">Säkerhetskopieringen sker automatiskt</span><span class="sxs-lookup"><span data-stu-id="06e25-104">Backup happens Automatically</span></span>
<span data-ttu-id="06e25-105">När du använder Azure-databas för MySQL görs hello databastjänsten en säkerhetskopia av hello tjänsten var femte minut.</span><span class="sxs-lookup"><span data-stu-id="06e25-105">When using Azure Database for MySQL, hello database service automatically makes a backup of hello service every 5 minutes.</span></span> 

<span data-ttu-id="06e25-106">hello säkerhetskopior är tillgängliga för 7 dagar vid användning av grundläggande nivån och 35 dagar när du använder standardnivån.</span><span class="sxs-lookup"><span data-stu-id="06e25-106">hello backups are available for 7 days when using Basic Tier, and 35 days when using Standard Tier.</span></span> <span data-ttu-id="06e25-107">Mer information finns i [Azure-databas för MySQL-servicenivåer](concepts-service-tiers.md)</span><span class="sxs-lookup"><span data-stu-id="06e25-107">For more information, see [Azure Database for MySQL service tiers](concepts-service-tiers.md)</span></span>

<span data-ttu-id="06e25-108">Med hjälp av funktionen för automatisk säkerhetskopiering kan du återställa hello server och alla databaser i en ny server tooan tidigare punkt i tiden.</span><span class="sxs-lookup"><span data-stu-id="06e25-108">Using this automatic backup feature you may restore hello server and all its databases into a new server tooan earlier point-in-time.</span></span>

## <a name="restore-in-hello-azure-portal"></a><span data-ttu-id="06e25-109">Återställa hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="06e25-109">Restore in hello Azure portal</span></span>
<span data-ttu-id="06e25-110">Azure MySQL-databas kan du toorestore hello servern tillbaka tooa punkt i tiden och till tooa nya kopian av hello-server.</span><span class="sxs-lookup"><span data-stu-id="06e25-110">Azure Database for MySQL allows you toorestore hello server back tooa point in time and into tooa new copy of hello server.</span></span> <span data-ttu-id="06e25-111">Du kan använda den här nya servern toorecover dina data.</span><span class="sxs-lookup"><span data-stu-id="06e25-111">You can use this new server toorecover your data.</span></span> 

<span data-ttu-id="06e25-112">Till exempel om en tabell av misstag kan bort kl. tolv idag, du återställa toohello gång innan på dagen och hämta hello saknas tabellen och data från den nya kopian av hello-server.</span><span class="sxs-lookup"><span data-stu-id="06e25-112">For example, if a table was accidentally dropped at noon today, you could restore toohello time just before noon and retrieve hello missing table and data from that new copy of hello server.</span></span>

<span data-ttu-id="06e25-113">hello återställningspunkt följande hello exempel server tooa i tid:</span><span class="sxs-lookup"><span data-stu-id="06e25-113">hello following steps restore hello sample server tooa point in time:</span></span>

1. <span data-ttu-id="06e25-114">Logga in på hello [Azure-portalen](https://portal.azure.com/)</span><span class="sxs-lookup"><span data-stu-id="06e25-114">Sign into hello [Azure portal](https://portal.azure.com/)</span></span>

2. <span data-ttu-id="06e25-115">Hitta din Azure-databas för MySQL-servern.</span><span class="sxs-lookup"><span data-stu-id="06e25-115">Locate your Azure Database for MySQL server.</span></span> <span data-ttu-id="06e25-116">I hello vänster och välj **alla resurser**, markera din server hello-listan.</span><span class="sxs-lookup"><span data-stu-id="06e25-116">In hello left pane, select **All resources**, then select your server from hello list.</span></span>

3.  <span data-ttu-id="06e25-117">Klicka på hello överkant hello serverblad översikt **återställa** hello i verktygsfältet.</span><span class="sxs-lookup"><span data-stu-id="06e25-117">On hello top of hello server overview blade, click **Restore** on hello toolbar.</span></span> <span data-ttu-id="06e25-118">hello återställning blad öppnas.</span><span class="sxs-lookup"><span data-stu-id="06e25-118">hello Restore blade opens.</span></span>
<span data-ttu-id="06e25-119">![Klicka på återställningsknappen](./media/howto-restore-server-portal/click-restore-button.png)</span><span class="sxs-lookup"><span data-stu-id="06e25-119">![click restore button](./media/howto-restore-server-portal/click-restore-button.png)</span></span>

4. <span data-ttu-id="06e25-120">Fyll i hello återställning formuläret med hello krävs information:</span><span class="sxs-lookup"><span data-stu-id="06e25-120">Fill out hello Restore form with hello required information:</span></span>

- <span data-ttu-id="06e25-121">**Återställningspunkt (UTC)**: använder hello väljaren för datum och tid Väljaren, Välj en tidpunkt i toorestore till.</span><span class="sxs-lookup"><span data-stu-id="06e25-121">**Restore point (UTC)**: Using hello Date picker and time picker, select a point-in-time toorestore to.</span></span> <span data-ttu-id="06e25-122">hello-tid som anges är i UTC-format, så du måste förmodligen tooconvert hello lokaltid till UTC.</span><span class="sxs-lookup"><span data-stu-id="06e25-122">hello time specified is in UTC format, so you likely need tooconvert hello local time into UTC.</span></span>
- <span data-ttu-id="06e25-123">**Återställa toonew server**: Ange en ny server name toorestore hello befintlig server till.</span><span class="sxs-lookup"><span data-stu-id="06e25-123">**Restore toonew server**: Provide a new server name toorestore hello existing server into.</span></span>
- <span data-ttu-id="06e25-124">**Plats**: hello region val automatiskt fylls med hello källa server region och kan inte ändras.</span><span class="sxs-lookup"><span data-stu-id="06e25-124">**Location**: hello region choice automatically populates with hello source server region, and cannot be changed.</span></span>
- <span data-ttu-id="06e25-125">**Prisnivån**: hello priser nivå val automatiskt fylls med hello priser för samma nivå som hello källservern och kan inte ändras här.</span><span class="sxs-lookup"><span data-stu-id="06e25-125">**Pricing tier**: hello pricing tier choice automatically populates with hello same pricing tier as hello source server, and cannot be changed here.</span></span> 
<span data-ttu-id="06e25-126">![PITR-återställning](./media/howto-restore-server-portal/pitr-restore.png)</span><span class="sxs-lookup"><span data-stu-id="06e25-126">![PITR Restore](./media/howto-restore-server-portal/pitr-restore.png)</span></span>

5. <span data-ttu-id="06e25-127">Klicka på **OK** toorestore hello server toorestore tooa tidpunkt.</span><span class="sxs-lookup"><span data-stu-id="06e25-127">Click **OK** toorestore hello server toorestore tooa point in time.</span></span> 

6. <span data-ttu-id="06e25-128">Leta upp hello ny server som har skapats tooverify hello databaser har återställts korrekt när hello återställning har slutförts.</span><span class="sxs-lookup"><span data-stu-id="06e25-128">After hello restore finishes, locate hello new server that was created tooverify hello databases were restored as expected.</span></span>

## <a name="next-steps"></a><span data-ttu-id="06e25-129">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="06e25-129">Next steps</span></span>
- [<span data-ttu-id="06e25-130">Anslutningsbibliotek för Azure-databas för MySQL</span><span class="sxs-lookup"><span data-stu-id="06e25-130">Connection libraries for Azure Database for MySQL</span></span>](concepts-connection-libraries.md)