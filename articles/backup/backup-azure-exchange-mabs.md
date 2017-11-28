---
title: "aaaBack in en säkerhetskopiering med Azure Backup Server med Exchange server tooAzure | Microsoft Docs"
description: "Lär dig hur tooback upp en Exchange server-tooAzure säkerhetskopiera med hjälp av Azure Backup Server"
services: backup
documentationcenter: 
author: pvrk
manager: shivamg
editor: 
ms.assetid: e46557e8-2eaf-4ee0-99ea-00fbb8687dca
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: pullabhk
ms.openlocfilehash: db874161151fc57c5b79c41531e18d577f567f66
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-an-exchange-server-tooazure-backup-with-azure-backup-server"></a><span data-ttu-id="e09de-103">Säkerhetskopiera en Exchange server-tooAzure säkerhetskopiering med Azure Backup Server</span><span class="sxs-lookup"><span data-stu-id="e09de-103">Back up an Exchange server tooAzure Backup with Azure Backup Server</span></span>
<span data-ttu-id="e09de-104">Den här artikeln beskriver hur tooconfigure Microsoft Azure Backup Server (MABS) tooback upp en Microsoft Exchange server-tooAzure.</span><span class="sxs-lookup"><span data-stu-id="e09de-104">This article describes how tooconfigure Microsoft Azure Backup Server (MABS) tooback up a Microsoft Exchange server tooAzure.</span></span>  

## <a name="prerequisites"></a><span data-ttu-id="e09de-105">Krav</span><span class="sxs-lookup"><span data-stu-id="e09de-105">Prerequisites</span></span>
<span data-ttu-id="e09de-106">Innan du fortsätter, kontrollera att Azure Backup Server [installerad och förberedda](backup-azure-microsoft-azure-backup.md).</span><span class="sxs-lookup"><span data-stu-id="e09de-106">Before you continue, make sure that Azure Backup Server is [installed and prepared](backup-azure-microsoft-azure-backup.md).</span></span>

## <a name="mabs-protection-agent"></a><span data-ttu-id="e09de-107">MABS skyddsagenten</span><span class="sxs-lookup"><span data-stu-id="e09de-107">MABS protection agent</span></span>
<span data-ttu-id="e09de-108">Hej tooinstall MABS skyddsagenten på hello Exchange-servern, Följ dessa steg:</span><span class="sxs-lookup"><span data-stu-id="e09de-108">tooinstall hello MABS protection agent on hello Exchange server, follow these steps:</span></span>

1. <span data-ttu-id="e09de-109">Kontrollera att hello brandväggar är korrekt konfigurerade.</span><span class="sxs-lookup"><span data-stu-id="e09de-109">Make sure that hello firewalls are correctly configured.</span></span> <span data-ttu-id="e09de-110">Se [konfigurera brandväggsundantag för agenten hello](https://technet.microsoft.com/library/Hh758204.aspx).</span><span class="sxs-lookup"><span data-stu-id="e09de-110">See [Configure firewall exceptions for hello agent](https://technet.microsoft.com/library/Hh758204.aspx).</span></span>
2. <span data-ttu-id="e09de-111">Installera hello-agenten på hello Exchange-servern genom att klicka på **Management > agenter > installera** i MABS-administratörskonsolen.</span><span class="sxs-lookup"><span data-stu-id="e09de-111">Install hello agent on hello Exchange server by clicking **Management > Agents > Install** in MABS Administrator Console.</span></span> <span data-ttu-id="e09de-112">Se [installera hello MABS skyddsagenten](https://technet.microsoft.com/library/hh758186.aspx?f=255&MSPPError=-2147217396) detaljerade anvisningar.</span><span class="sxs-lookup"><span data-stu-id="e09de-112">See [Install hello MABS protection agent](https://technet.microsoft.com/library/hh758186.aspx?f=255&MSPPError=-2147217396) for detailed steps.</span></span>

## <a name="create-a-protection-group-for-hello-exchange-server"></a><span data-ttu-id="e09de-113">Skapa en skyddsgrupp för hello Exchange-server</span><span class="sxs-lookup"><span data-stu-id="e09de-113">Create a protection group for hello Exchange server</span></span>
1. <span data-ttu-id="e09de-114">Hello MABS administratörskonsolen, klicka på **skydd**, och klicka sedan på **ny** på hello verktyget menyfliksområdet tooopen hello **Skapa ny Skyddsgrupp** guiden.</span><span class="sxs-lookup"><span data-stu-id="e09de-114">In hello MABS Administrator Console, click **Protection**, and then click **New** on hello tool ribbon tooopen hello **Create New Protection Group** wizard.</span></span>
2. <span data-ttu-id="e09de-115">På hello **Välkommen** skärmen hello guiden Klicka **nästa**.</span><span class="sxs-lookup"><span data-stu-id="e09de-115">On hello **Welcome** screen of hello wizard click **Next**.</span></span>
3. <span data-ttu-id="e09de-116">På hello **Välj typ av skyddsgrupp** , väljer **servrar** och på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="e09de-116">On hello **Select protection group type** screen, select **Servers** and click **Next**.</span></span>
4. <span data-ttu-id="e09de-117">Välj hello Exchange server-databas som du vill använda tooprotect och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="e09de-117">Select hello Exchange server database that you want tooprotect and click **Next**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="e09de-118">Om du skyddar Exchange 2013 Kontrollera hello [krav för Exchange 2013](https://technet.microsoft.com/library/dn751029.aspx).</span><span class="sxs-lookup"><span data-stu-id="e09de-118">If you are protecting Exchange 2013, check hello [Exchange 2013 prerequisites](https://technet.microsoft.com/library/dn751029.aspx).</span></span>
   >
   >

    <span data-ttu-id="e09de-119">I följande exempel hello, har hello Exchange 2010-databas valts.</span><span class="sxs-lookup"><span data-stu-id="e09de-119">In hello following example, hello Exchange 2010 database is selected.</span></span>

    ![Välj gruppmedlemmar](./media/backup-azure-backup-exchange-server/select-group-members.png)
5. <span data-ttu-id="e09de-121">Välj dataskyddsmetod hello.</span><span class="sxs-lookup"><span data-stu-id="e09de-121">Select hello data protection method.</span></span>

    <span data-ttu-id="e09de-122">Namnet hello skyddsgrupp och välj sedan båda hello följande alternativ:</span><span class="sxs-lookup"><span data-stu-id="e09de-122">Name hello protection group, and then select both of hello following options:</span></span>

   * <span data-ttu-id="e09de-123">Jag vill ha kortvarigt skydd med Disk.</span><span class="sxs-lookup"><span data-stu-id="e09de-123">I want short-term protection using Disk.</span></span>
   * <span data-ttu-id="e09de-124">Jag vill använda onlineskydd.</span><span class="sxs-lookup"><span data-stu-id="e09de-124">I want online protection.</span></span>
6. <span data-ttu-id="e09de-125">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="e09de-125">Click **Next**.</span></span>
7. <span data-ttu-id="e09de-126">Välj hello **kör Eseutil toocheck dataintegritet** om du vill toocheck hello integriteten hos hello Exchange Server-databaser.</span><span class="sxs-lookup"><span data-stu-id="e09de-126">Select hello **Run Eseutil toocheck data integrity** option if you want toocheck hello integrity of hello Exchange Server databases.</span></span>

    <span data-ttu-id="e09de-127">När du har valt det här alternativet säkerhetskopiering konsekvenskontroll kommer att köras på MABS tooavoid hello i/o-trafik som genereras genom att köra hello **eseutil** på hello Exchange server.</span><span class="sxs-lookup"><span data-stu-id="e09de-127">After you select this option, backup consistency checking will be run on MABS tooavoid hello I/O traffic that’s generated by running hello **eseutil** command on hello Exchange server.</span></span>

   > [!NOTE]
   > <span data-ttu-id="e09de-128">toouse detta alternativ, måste du kopiera hello Ese.dll och Eseutil.exe filer toohello C:\Program Files\Microsoft Azure Backup\DPM\DPM\bin katalog på hello MAB-servern.</span><span class="sxs-lookup"><span data-stu-id="e09de-128">toouse this option, you must copy hello Ese.dll and Eseutil.exe files toohello C:\Program Files\Microsoft Azure Backup\DPM\DPM\bin directory on hello MAB server.</span></span> <span data-ttu-id="e09de-129">Annars utlöses hello följande fel:</span><span class="sxs-lookup"><span data-stu-id="e09de-129">Otherwise, hello following error is triggered:</span></span>  
   > <span data-ttu-id="e09de-130">![Eseutil-fel](./media/backup-azure-backup-exchange-server/eseutil-error.png)</span><span class="sxs-lookup"><span data-stu-id="e09de-130">![eseutil error](./media/backup-azure-backup-exchange-server/eseutil-error.png)</span></span>
   >
   >
8. <span data-ttu-id="e09de-131">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="e09de-131">Click **Next**.</span></span>
9. <span data-ttu-id="e09de-132">Välj hello databasen för **kopiering av säkerhetskopia**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="e09de-132">Select hello database for **Copy Backup**, and then click **Next**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="e09de-133">Om du inte väljer ”fullständiga säkerhetskopieringen” för minst en DAG kopia av en databas trunkeras inte loggar.</span><span class="sxs-lookup"><span data-stu-id="e09de-133">If you do not select “Full backup” for at least one DAG copy of a database, logs will not be truncated.</span></span>
   >
   >
10. <span data-ttu-id="e09de-134">Konfigurera hello mål för **kortsiktig säkerhetskopiering**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="e09de-134">Configure hello goals for **Short-Term backup**, and then click **Next**.</span></span>
11. <span data-ttu-id="e09de-135">Granska hello tillgängligt diskutrymme och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="e09de-135">Review hello available disk space, and then click **Next**.</span></span>
12. <span data-ttu-id="e09de-136">Välj hello tid vid vilken hello MAB Server skapar hello inledande replikering, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="e09de-136">Select hello time at which hello MAB Server will create hello initial replication, and then click **Next**.</span></span>
13. <span data-ttu-id="e09de-137">Välj alternativ för konsekvenskontroll hello och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="e09de-137">Select hello consistency check options, and then click **Next**.</span></span>
14. <span data-ttu-id="e09de-138">Välj hello-databas som du vill tooback in tooAzure och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="e09de-138">Choose hello database that you want tooback up tooAzure, and then click **Next**.</span></span> <span data-ttu-id="e09de-139">Exempel:</span><span class="sxs-lookup"><span data-stu-id="e09de-139">For example:</span></span>

    ![Ange onlineskyddsdata](./media/backup-azure-backup-exchange-server/specify-online-protection-data.png)
15. <span data-ttu-id="e09de-141">Definiera hello schema för **Azure Backup**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="e09de-141">Define hello schedule for **Azure Backup**, and then click **Next**.</span></span> <span data-ttu-id="e09de-142">Exempel:</span><span class="sxs-lookup"><span data-stu-id="e09de-142">For example:</span></span>

    ![Ange onlinesäkerhetskopieringsschema](./media/backup-azure-backup-exchange-server/specify-online-backup-schedule.png)

    > [!NOTE]
    > <span data-ttu-id="e09de-144">Observera onlineåterställningspunkter baseras på snabb och fullständig återställningspunkter.</span><span class="sxs-lookup"><span data-stu-id="e09de-144">Note Online recovery points are based on express full recovery points.</span></span> <span data-ttu-id="e09de-145">Därför måste du schemalägga hello onlineåterställningspunkt stund hello som har angetts för hello express fullständig återställningspunkt.</span><span class="sxs-lookup"><span data-stu-id="e09de-145">Therefore, you must schedule hello online recovery point after hello time that’s specified for hello express full recovery point.</span></span>
    >
    >
16. <span data-ttu-id="e09de-146">Konfigurera hello bevarandeprincipen för **Azure Backup**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="e09de-146">Configure hello retention policy for **Azure Backup**, and then click **Next**.</span></span>
17. <span data-ttu-id="e09de-147">Välj ett alternativ för onlinereplikeringsmetoden och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="e09de-147">Choose an online replication option and click **Next**.</span></span>

    <span data-ttu-id="e09de-148">Det kan ta lång tid för hello första säkerhetskopiering toobe skapade hello nätverket om du har en stor databas.</span><span class="sxs-lookup"><span data-stu-id="e09de-148">If you have a large database, it could take a long time for hello initial backup toobe created over hello network.</span></span> <span data-ttu-id="e09de-149">tooavoid problemet bör du skapa en offlinesäkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="e09de-149">tooavoid this issue, you can create an offline backup.</span></span>  

    ![Ange bevarandeprincip](./media/backup-azure-backup-exchange-server/specify-online-retention-policy.png)
18. <span data-ttu-id="e09de-151">Bekräfta inställningarna för hello och klicka sedan på **Skapa grupp**.</span><span class="sxs-lookup"><span data-stu-id="e09de-151">Confirm hello settings, and then click **Create Group**.</span></span>
19. <span data-ttu-id="e09de-152">Klicka på **Stäng**.</span><span class="sxs-lookup"><span data-stu-id="e09de-152">Click **Close**.</span></span>

## <a name="recover-hello-exchange-database"></a><span data-ttu-id="e09de-153">Återställa hello Exchange-databas</span><span class="sxs-lookup"><span data-stu-id="e09de-153">Recover hello Exchange database</span></span>
1. <span data-ttu-id="e09de-154">toorecover Exchange-databaser, klicka på **Recovery** i hello MABS-administratörskonsolen.</span><span class="sxs-lookup"><span data-stu-id="e09de-154">toorecover an Exchange database, click **Recovery** in hello MABS Administrator Console.</span></span>
2. <span data-ttu-id="e09de-155">Leta upp hello Exchange-databas som du vill toorecover.</span><span class="sxs-lookup"><span data-stu-id="e09de-155">Locate hello Exchange database that you want toorecover.</span></span>
3. <span data-ttu-id="e09de-156">Välj en onlineåterställningspunkt hello *återställningstid* listrutan.</span><span class="sxs-lookup"><span data-stu-id="e09de-156">Select an online recovery point from hello *recovery time* drop-down list.</span></span>
4. <span data-ttu-id="e09de-157">Klicka på **återställa** toostart hello **Återställningsguiden**.</span><span class="sxs-lookup"><span data-stu-id="e09de-157">Click **Recover** toostart hello **Recovery Wizard**.</span></span>

<span data-ttu-id="e09de-158">Onlineåterställningspunkter finns det fem typer av återställning:</span><span class="sxs-lookup"><span data-stu-id="e09de-158">For online recovery points, there are five recovery types:</span></span>

* <span data-ttu-id="e09de-159">**Återställa toooriginal Exchange-serverplatsen:** hello data kommer att återställda toohello ursprungliga Exchange-servern.</span><span class="sxs-lookup"><span data-stu-id="e09de-159">**Recover toooriginal Exchange Server location:** hello data will be recovered toohello original Exchange server.</span></span>
* <span data-ttu-id="e09de-160">**Återställa tooanother databas på en Exchange Server:** hello data kommer att återställda tooanother databas på en annan Exchange server.</span><span class="sxs-lookup"><span data-stu-id="e09de-160">**Recover tooanother database on an Exchange Server:** hello data will be recovered tooanother database on another Exchange server.</span></span>
* <span data-ttu-id="e09de-161">**Återställa tooa Återställningsdatabas:** hello data kommer att återställda tooan Exchange Recovery databasen (RDB).</span><span class="sxs-lookup"><span data-stu-id="e09de-161">**Recover tooa Recovery Database:** hello data will be recovered tooan Exchange Recovery Database (RDB).</span></span>
* <span data-ttu-id="e09de-162">**Kopiera tooa nätverksmapp:** hello data kommer att återställda tooa nätverksmapp.</span><span class="sxs-lookup"><span data-stu-id="e09de-162">**Copy tooa network folder:** hello data will be recovered tooa network folder.</span></span>
* <span data-ttu-id="e09de-163">**Kopiera tootape:** om du har ett bandbibliotek eller en fristående bandenhet som är anslutna och konfigurerat på MABS hello återställningspunkt kommer att kopieras tooa ledigt band.</span><span class="sxs-lookup"><span data-stu-id="e09de-163">**Copy tootape:** If you have a tape library or a stand-alone tape drive attached and configured on MABS, hello recovery point will be copied tooa free tape.</span></span>

    ![Välj online replikering](./media/backup-azure-backup-exchange-server/choose-online-replication.png)

## <a name="next-steps"></a><span data-ttu-id="e09de-165">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e09de-165">Next steps</span></span>
* [<span data-ttu-id="e09de-166">Azure Backup vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="e09de-166">Azure Backup FAQ</span></span>](backup-azure-backup-faq.md)
