---
title: "Säkerhetskopiera en Exchange-server till Azure Backup med System Center 2012 R2 DPM | Microsoft Docs"
description: "Lär dig hur du säkerhetskopierar en Exchange-server till Azure Backup med System Center 2012 R2 DPM"
services: backup
documentationcenter: 
author: MaanasSaran
manager: NKolli1
editor: 
ms.assetid: 13f32256-888e-416e-a78b-40c2a26a5939
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2016
ms.author: masaran;jimpark;delhan;trinadhk;markgal
ms.openlocfilehash: 2a0e416440e55cfde70cbd20d40c99fb29b4229c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="back-up-an-exchange-server-to-azure-backup-with-system-center-2012-r2-dpm"></a><span data-ttu-id="776e2-103">Säkerhetskopiera en Exchange-server till Azure Backup med System Center 2012 R2 DPM</span><span class="sxs-lookup"><span data-stu-id="776e2-103">Back up an Exchange server to Azure Backup with System Center 2012 R2 DPM</span></span>
<span data-ttu-id="776e2-104">Den här artikeln beskriver hur du konfigurerar en server för System Center 2012 R2 Data Protection Manager (DPM) att säkerhetskopiera en Microsoft Exchange-server till Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="776e2-104">This article describes how to configure a System Center 2012 R2 Data Protection Manager (DPM) server to back up a Microsoft Exchange server to  Azure Backup.</span></span>  

## <a name="updates"></a><span data-ttu-id="776e2-105">Uppdateringar</span><span class="sxs-lookup"><span data-stu-id="776e2-105">Updates</span></span>
<span data-ttu-id="776e2-106">För att kunna registrera DPM-servern med Azure Backup, måste du installera den senaste samlade uppdateringen för System Center 2012 R2 DPM och den senaste versionen av Azure Backup Agent.</span><span class="sxs-lookup"><span data-stu-id="776e2-106">To successfully register the DPM server with Azure Backup, you must install the latest update rollup for System Center 2012 R2 DPM and the latest version of the Azure Backup Agent.</span></span> <span data-ttu-id="776e2-107">Hämta den senaste samlade uppdateringen från den [Microsoft Catalog](http://catalog.update.microsoft.com/v7/site/Search.aspx?q=System%20Center%202012%20R2%20Data%20protection%20manager).</span><span class="sxs-lookup"><span data-stu-id="776e2-107">Get the latest update rollup from the [Microsoft Catalog](http://catalog.update.microsoft.com/v7/site/Search.aspx?q=System%20Center%202012%20R2%20Data%20protection%20manager).</span></span>

> [!NOTE]
> <span data-ttu-id="776e2-108">I exemplen i den här artikeln, version 2.0.8719.0 av Azure Backup-agenten är installerad och Samlad uppdatering 6 installeras på System Center 2012 R2 DPM.</span><span class="sxs-lookup"><span data-stu-id="776e2-108">For the examples in this article, version 2.0.8719.0 of the Azure Backup Agent is installed, and Update Rollup 6 is installed on System Center 2012 R2 DPM.</span></span>
>
>

## <a name="prerequisites"></a><span data-ttu-id="776e2-109">Krav</span><span class="sxs-lookup"><span data-stu-id="776e2-109">Prerequisites</span></span>
<span data-ttu-id="776e2-110">Innan du fortsätter, kontrollera att alla de [krav](backup-azure-dpm-introduction.md#prerequisites) för att använda Microsoft Azure Backup för att skydda arbetsbelastningar har uppfyllts.</span><span class="sxs-lookup"><span data-stu-id="776e2-110">Before you continue, make sure that all the [prerequisites](backup-azure-dpm-introduction.md#prerequisites) for using Microsoft Azure Backup to protect workloads have been met.</span></span> <span data-ttu-id="776e2-111">Dessa krav är följande:</span><span class="sxs-lookup"><span data-stu-id="776e2-111">These prerequisites include the following:</span></span>

* <span data-ttu-id="776e2-112">Ett säkerhetskopieringsvalv på Azure-webbplatsen har skapats.</span><span class="sxs-lookup"><span data-stu-id="776e2-112">A backup vault on the Azure site has been created.</span></span>
* <span data-ttu-id="776e2-113">Agent och valvautentiseringsuppgifterna har laddats ned till DPM-servern.</span><span class="sxs-lookup"><span data-stu-id="776e2-113">Agent and vault credentials have been downloaded to the DPM server.</span></span>
* <span data-ttu-id="776e2-114">Agenten har installerats på DPM-servern.</span><span class="sxs-lookup"><span data-stu-id="776e2-114">The agent is installed on the DPM server.</span></span>
* <span data-ttu-id="776e2-115">Valvautentiseringsuppgifter som användes för att registrera DPM-servern.</span><span class="sxs-lookup"><span data-stu-id="776e2-115">The vault credentials were used to register the DPM server.</span></span>
* <span data-ttu-id="776e2-116">Om du skyddar Exchange 2016 kan du uppgradera till DPM 2012 R2 UR9 eller senare</span><span class="sxs-lookup"><span data-stu-id="776e2-116">If you are protecting Exchange 2016, please upgrade to DPM 2012 R2 UR9 or later</span></span>

## <a name="dpm-protection-agent"></a><span data-ttu-id="776e2-117">DPM-skyddsagenten</span><span class="sxs-lookup"><span data-stu-id="776e2-117">DPM protection agent</span></span>
<span data-ttu-id="776e2-118">Följ dessa steg om du vill installera DPM-skyddsagenten på Exchange-servern:</span><span class="sxs-lookup"><span data-stu-id="776e2-118">To install the DPM protection agent on the Exchange server, follow these steps:</span></span>

1. <span data-ttu-id="776e2-119">Kontrollera att brandväggarna är korrekt konfigurerade.</span><span class="sxs-lookup"><span data-stu-id="776e2-119">Make sure that the firewalls are correctly configured.</span></span> <span data-ttu-id="776e2-120">Se [konfigurera brandväggsundantag för agenten](https://technet.microsoft.com/library/Hh758204.aspx).</span><span class="sxs-lookup"><span data-stu-id="776e2-120">See [Configure firewall exceptions for the agent](https://technet.microsoft.com/library/Hh758204.aspx).</span></span>
2. <span data-ttu-id="776e2-121">Installera agenten på Exchange-servern genom att klicka på **Management > agenter > installera** i DPM-administratörskonsolen.</span><span class="sxs-lookup"><span data-stu-id="776e2-121">Install the agent on the Exchange server by clicking **Management > Agents > Install** in DPM Administrator Console.</span></span> <span data-ttu-id="776e2-122">Se [installera DPM-skyddsagenten](https://technet.microsoft.com/library/hh758186.aspx?f=255&MSPPError=-2147217396) detaljerade anvisningar.</span><span class="sxs-lookup"><span data-stu-id="776e2-122">See [Install the DPM protection agent](https://technet.microsoft.com/library/hh758186.aspx?f=255&MSPPError=-2147217396) for detailed steps.</span></span>

## <a name="create-a-protection-group-for-the-exchange-server"></a><span data-ttu-id="776e2-123">Skapa en skyddsgrupp för Exchange server</span><span class="sxs-lookup"><span data-stu-id="776e2-123">Create a protection group for the Exchange server</span></span>
1. <span data-ttu-id="776e2-124">I DPM-administratörskonsolen klickar du på **skydd**, och klicka sedan på **ny** på verktygsfliken för att öppna den **Skapa ny Skyddsgrupp** guiden.</span><span class="sxs-lookup"><span data-stu-id="776e2-124">In the DPM Administrator Console, click **Protection**, and then click **New** on the tool ribbon to open the **Create New Protection Group** wizard.</span></span>
2. <span data-ttu-id="776e2-125">På den **Välkommen** skärmen i guiden klickar du på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="776e2-125">On the **Welcome** screen of the wizard click **Next**.</span></span>
3. <span data-ttu-id="776e2-126">På den **Välj typ av skyddsgrupp** , väljer **servrar** och på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="776e2-126">On the **Select protection group type** screen, select **Servers** and click **Next**.</span></span>
4. <span data-ttu-id="776e2-127">Välj den Exchange server-databas som du vill skydda och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="776e2-127">Select the Exchange server database that you want to protect and click **Next**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="776e2-128">Om du skyddar Exchange 2013, kontrollera den [krav för Exchange 2013](https://technet.microsoft.com/library/dn751029.aspx).</span><span class="sxs-lookup"><span data-stu-id="776e2-128">If you are protecting Exchange 2013, check the [Exchange 2013 prerequisites](https://technet.microsoft.com/library/dn751029.aspx).</span></span>
   >
   >

    <span data-ttu-id="776e2-129">I följande exempel har Exchange 2010-databas valts.</span><span class="sxs-lookup"><span data-stu-id="776e2-129">In the following example, the Exchange 2010 database is selected.</span></span>

    ![Välj gruppmedlemmar](./media/backup-azure-backup-exchange-server/select-group-members.png)
5. <span data-ttu-id="776e2-131">Välj dataskyddsmetod.</span><span class="sxs-lookup"><span data-stu-id="776e2-131">Select the data protection method.</span></span>

    <span data-ttu-id="776e2-132">Namn på skyddsgrupp och välj sedan båda av följande alternativ:</span><span class="sxs-lookup"><span data-stu-id="776e2-132">Name the protection group, and then select both of the following options:</span></span>

   * <span data-ttu-id="776e2-133">Jag vill ha kortvarigt skydd med Disk.</span><span class="sxs-lookup"><span data-stu-id="776e2-133">I want short-term protection using Disk.</span></span>
   * <span data-ttu-id="776e2-134">Jag vill använda onlineskydd.</span><span class="sxs-lookup"><span data-stu-id="776e2-134">I want online protection.</span></span>
6. <span data-ttu-id="776e2-135">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="776e2-135">Click **Next**.</span></span>
7. <span data-ttu-id="776e2-136">Välj den **kör Eseutil för att kontrollera dataintegriteten** om du vill kontrollera integriteten hos Exchange Server-databaser.</span><span class="sxs-lookup"><span data-stu-id="776e2-136">Select the **Run Eseutil to check data integrity** option if you want to check the integrity of the Exchange Server databases.</span></span>

    <span data-ttu-id="776e2-137">När du har valt det här alternativet säkerhetskopiering konsekvenskontroll körs på DPM-servern för att undvika att i/o-trafik som genereras genom att köra den **eseutil** på Exchange-servern.</span><span class="sxs-lookup"><span data-stu-id="776e2-137">After you select this option, backup consistency checking will be run on the DPM server to avoid the I/O traffic that’s generated by running the **eseutil** command on the Exchange server.</span></span>

   > [!NOTE]
   > <span data-ttu-id="776e2-138">Om du vill använda det här alternativet måste du kopiera filerna Ese.dll och Eseutil.exe till katalogen C:\Program Files\Microsoft System Center 2012 R2\DPM\DPM\bin på DPM-servern.</span><span class="sxs-lookup"><span data-stu-id="776e2-138">To use this option, you must copy the Ese.dll and Eseutil.exe files to the C:\Program Files\Microsoft System Center 2012 R2\DPM\DPM\bin directory on the DPM server.</span></span> <span data-ttu-id="776e2-139">Annars utlöses av följande fel:</span><span class="sxs-lookup"><span data-stu-id="776e2-139">Otherwise, the following error is triggered:</span></span>  
   > <span data-ttu-id="776e2-140">![Eseutil-fel](./media/backup-azure-backup-exchange-server/eseutil-error.png)</span><span class="sxs-lookup"><span data-stu-id="776e2-140">![eseutil error](./media/backup-azure-backup-exchange-server/eseutil-error.png)</span></span>
   >
   >
8. <span data-ttu-id="776e2-141">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="776e2-141">Click **Next**.</span></span>
9. <span data-ttu-id="776e2-142">Välj databas för **kopiering av säkerhetskopia**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="776e2-142">Select the database for **Copy Backup**, and then click **Next**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="776e2-143">Om du inte väljer ”fullständiga säkerhetskopieringen” för minst en DAG kopia av en databas trunkeras inte loggar.</span><span class="sxs-lookup"><span data-stu-id="776e2-143">If you do not select “Full backup” for at least one DAG copy of a database, logs will not be truncated.</span></span>
   >
   >
10. <span data-ttu-id="776e2-144">Konfigurera mål för **kortsiktig säkerhetskopiering**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="776e2-144">Configure the goals for **Short-Term backup**, and then click **Next**.</span></span>
11. <span data-ttu-id="776e2-145">Granska tillgängligt diskutrymme och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="776e2-145">Review the available disk space, and then click **Next**.</span></span>
12. <span data-ttu-id="776e2-146">Välj den tid då DPM-servern skapar den inledande replikeringen, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="776e2-146">Select the time at which the DPM server will create the initial replication, and then click **Next**.</span></span>
13. <span data-ttu-id="776e2-147">Välj alternativ för konsekvenskontroll och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="776e2-147">Select the consistency check options, and then click **Next**.</span></span>
14. <span data-ttu-id="776e2-148">Välj den databas som du vill säkerhetskopiera till Azure och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="776e2-148">Choose the database that you want to back up to Azure, and then click **Next**.</span></span> <span data-ttu-id="776e2-149">Exempel:</span><span class="sxs-lookup"><span data-stu-id="776e2-149">For example:</span></span>

    ![Ange onlineskyddsdata](./media/backup-azure-backup-exchange-server/specify-online-protection-data.png)
15. <span data-ttu-id="776e2-151">Definiera schemat för **Azure Backup**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="776e2-151">Define the schedule for **Azure Backup**, and then click **Next**.</span></span> <span data-ttu-id="776e2-152">Exempel:</span><span class="sxs-lookup"><span data-stu-id="776e2-152">For example:</span></span>

    ![Ange onlinesäkerhetskopieringsschema](./media/backup-azure-backup-exchange-server/specify-online-backup-schedule.png)

    > [!NOTE]
    > <span data-ttu-id="776e2-154">Observera onlineåterställningspunkter baseras på snabb och fullständig återställningspunkter.</span><span class="sxs-lookup"><span data-stu-id="776e2-154">Note Online recovery points are based on express full recovery points.</span></span> <span data-ttu-id="776e2-155">Därför måste du schemalägga onlineåterställningspunkt efter den tid som har angetts för snabb och fullständig återställningspunkt.</span><span class="sxs-lookup"><span data-stu-id="776e2-155">Therefore, you must schedule the online recovery point after the time that’s specified for the express full recovery point.</span></span>
    >
    >
16. <span data-ttu-id="776e2-156">Konfigurera bevarandeprincipen för **Azure Backup**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="776e2-156">Configure the retention policy for **Azure Backup**, and then click **Next**.</span></span>
17. <span data-ttu-id="776e2-157">Välj ett alternativ för onlinereplikeringsmetoden och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="776e2-157">Choose an online replication option and click **Next**.</span></span>

    <span data-ttu-id="776e2-158">Om du har en stor databas, kan det ta lång tid för den första säkerhetskopian ska skapas via nätverket.</span><span class="sxs-lookup"><span data-stu-id="776e2-158">If you have a large database, it could take a long time for the initial backup to be created over the network.</span></span> <span data-ttu-id="776e2-159">För att undvika det här problemet kan skapa du en offlinesäkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="776e2-159">To avoid this issue, you can create an offline backup.</span></span>  

    ![Ange bevarandeprincip](./media/backup-azure-backup-exchange-server/specify-online-retention-policy.png)
18. <span data-ttu-id="776e2-161">Bekräfta inställningarna och klicka sedan på **Skapa grupp**.</span><span class="sxs-lookup"><span data-stu-id="776e2-161">Confirm the settings, and then click **Create Group**.</span></span>
19. <span data-ttu-id="776e2-162">Klicka på **Stäng**.</span><span class="sxs-lookup"><span data-stu-id="776e2-162">Click **Close**.</span></span>

## <a name="recover-the-exchange-database"></a><span data-ttu-id="776e2-163">Återställa Exchange-databasen</span><span class="sxs-lookup"><span data-stu-id="776e2-163">Recover the Exchange database</span></span>
1. <span data-ttu-id="776e2-164">Om du vill återställa en Exchange-databas, klickar du på **Recovery** i DPM-administratörskonsolen.</span><span class="sxs-lookup"><span data-stu-id="776e2-164">To recover an Exchange database, click **Recovery** in the DPM Administrator Console.</span></span>
2. <span data-ttu-id="776e2-165">Leta reda på Exchange-databasen som du vill återställa.</span><span class="sxs-lookup"><span data-stu-id="776e2-165">Locate the Exchange database that you want to recover.</span></span>
3. <span data-ttu-id="776e2-166">Välj en onlineåterställningspunkt från den *återställningstid* listrutan.</span><span class="sxs-lookup"><span data-stu-id="776e2-166">Select an online recovery point from the *recovery time* drop-down list.</span></span>
4. <span data-ttu-id="776e2-167">Klicka på **återställa** att starta den **Återställningsguiden**.</span><span class="sxs-lookup"><span data-stu-id="776e2-167">Click **Recover** to start the **Recovery Wizard**.</span></span>

<span data-ttu-id="776e2-168">Onlineåterställningspunkter finns det fem typer av återställning:</span><span class="sxs-lookup"><span data-stu-id="776e2-168">For online recovery points, there are five recovery types:</span></span>

* <span data-ttu-id="776e2-169">**Återställ till den ursprungliga Exchange-serverplatsen:** data kommer att återställas till den ursprungliga Exchange-servern.</span><span class="sxs-lookup"><span data-stu-id="776e2-169">**Recover to original Exchange Server location:** The data will be recovered to the original Exchange server.</span></span>
* <span data-ttu-id="776e2-170">**Återställ till en annan databas på en Exchange Server:** data kommer att återställas till en annan databas på en annan Exchange server.</span><span class="sxs-lookup"><span data-stu-id="776e2-170">**Recover to another database on an Exchange Server:** The data will be recovered to another database on another Exchange server.</span></span>
* <span data-ttu-id="776e2-171">**Återställ till Återställningsdatabas:** data kommer att återställas till en Exchange Recovery databasen (RDB).</span><span class="sxs-lookup"><span data-stu-id="776e2-171">**Recover to a Recovery Database:** The data will be recovered to an Exchange Recovery Database (RDB).</span></span>
* <span data-ttu-id="776e2-172">**Kopiera till en nätverksmapp:** data kommer att återställas till en nätverksmapp.</span><span class="sxs-lookup"><span data-stu-id="776e2-172">**Copy to a network folder:** The data will be recovered to a network folder.</span></span>
* <span data-ttu-id="776e2-173">**Kopiera till band:** om du har ett bandbibliotek eller en fristående bandenhet ansluten och konfigurerats på DPM-servern återställningspunkten ska kopieras till ett ledigt band.</span><span class="sxs-lookup"><span data-stu-id="776e2-173">**Copy to tape:** If you have a tape library or a stand-alone tape drive attached and configured on the DPM server, the recovery point will be copied to a free tape.</span></span>

    ![Välj online replikering](./media/backup-azure-backup-exchange-server/choose-online-replication.png)

## <a name="next-steps"></a><span data-ttu-id="776e2-175">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="776e2-175">Next steps</span></span>
* [<span data-ttu-id="776e2-176">Azure Backup vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="776e2-176">Azure Backup FAQ</span></span>](backup-azure-backup-faq.md)
