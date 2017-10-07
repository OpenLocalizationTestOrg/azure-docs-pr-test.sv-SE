---
title: "aaaBack upp en Exchange server-tooAzure säkerhetskopiering med System Center 2012 R2 DPM | Microsoft Docs"
description: "Lär dig hur tooback upp en Exchange server-tooAzure säkerhetskopia med System Center 2012 R2 DPM"
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
ms.openlocfilehash: fa99296d095c180333474b6d419ebc5ec727547a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-an-exchange-server-tooazure-backup-with-system-center-2012-r2-dpm"></a><span data-ttu-id="06f97-103">Säkerhetskopiera en Exchange server-tooAzure säkerhetskopiering med System Center 2012 R2 DPM</span><span class="sxs-lookup"><span data-stu-id="06f97-103">Back up an Exchange server tooAzure Backup with System Center 2012 R2 DPM</span></span>
<span data-ttu-id="06f97-104">Den här artikeln beskriver hur tooconfigure en System Center 2012 R2 Data Protection Manager (DPM) server tooback in en Microsoft Exchange-server för Azure-säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="06f97-104">This article describes how tooconfigure a System Center 2012 R2 Data Protection Manager (DPM) server tooback up a Microsoft Exchange server too Azure Backup.</span></span>  

## <a name="updates"></a><span data-ttu-id="06f97-105">Uppdateringar</span><span class="sxs-lookup"><span data-stu-id="06f97-105">Updates</span></span>
<span data-ttu-id="06f97-106">Du måste installera hello senaste samlade uppdateringen för System Center 2012 R2 DPM och hello senaste versionen av hello Azure Backup-agenten toosuccessfully registrera hello DPM-servern med Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="06f97-106">toosuccessfully register hello DPM server with Azure Backup, you must install hello latest update rollup for System Center 2012 R2 DPM and hello latest version of hello Azure Backup Agent.</span></span> <span data-ttu-id="06f97-107">Hämta hello senaste samlade uppdateringen från hello [Microsoft Catalog](http://catalog.update.microsoft.com/v7/site/Search.aspx?q=System%20Center%202012%20R2%20Data%20protection%20manager).</span><span class="sxs-lookup"><span data-stu-id="06f97-107">Get hello latest update rollup from hello [Microsoft Catalog](http://catalog.update.microsoft.com/v7/site/Search.aspx?q=System%20Center%202012%20R2%20Data%20protection%20manager).</span></span>

> [!NOTE]
> <span data-ttu-id="06f97-108">För hello exemplen i den här artikeln, version 2.0.8719.0 av hello Azure Backup-agenten är installerad och Samlad uppdatering 6 installeras på System Center 2012 R2 DPM.</span><span class="sxs-lookup"><span data-stu-id="06f97-108">For hello examples in this article, version 2.0.8719.0 of hello Azure Backup Agent is installed, and Update Rollup 6 is installed on System Center 2012 R2 DPM.</span></span>
>
>

## <a name="prerequisites"></a><span data-ttu-id="06f97-109">Krav</span><span class="sxs-lookup"><span data-stu-id="06f97-109">Prerequisites</span></span>
<span data-ttu-id="06f97-110">Innan du fortsätter, kontrollera att alla hello [krav](backup-azure-dpm-introduction.md#prerequisites) för att använda Microsoft Azure Backup tooprotect arbetsbelastningar har uppfyllts.</span><span class="sxs-lookup"><span data-stu-id="06f97-110">Before you continue, make sure that all hello [prerequisites](backup-azure-dpm-introduction.md#prerequisites) for using Microsoft Azure Backup tooprotect workloads have been met.</span></span> <span data-ttu-id="06f97-111">Dessa krav är hello följande:</span><span class="sxs-lookup"><span data-stu-id="06f97-111">These prerequisites include hello following:</span></span>

* <span data-ttu-id="06f97-112">Ett säkerhetskopieringsvalv på hello Azure webbplats har skapats.</span><span class="sxs-lookup"><span data-stu-id="06f97-112">A backup vault on hello Azure site has been created.</span></span>
* <span data-ttu-id="06f97-113">Agent och valvautentiseringsuppgifterna har hämtat toohello DPM-servern.</span><span class="sxs-lookup"><span data-stu-id="06f97-113">Agent and vault credentials have been downloaded toohello DPM server.</span></span>
* <span data-ttu-id="06f97-114">hello-agenten är installerad på hello DPM-servern.</span><span class="sxs-lookup"><span data-stu-id="06f97-114">hello agent is installed on hello DPM server.</span></span>
* <span data-ttu-id="06f97-115">Hej valvautentiseringsuppgifter har använt tooregister hello DPM-servern.</span><span class="sxs-lookup"><span data-stu-id="06f97-115">hello vault credentials were used tooregister hello DPM server.</span></span>
* <span data-ttu-id="06f97-116">Uppgradera tooDPM 2012 R2 UR9 om du skyddar Exchange 2016 eller senare</span><span class="sxs-lookup"><span data-stu-id="06f97-116">If you are protecting Exchange 2016, please upgrade tooDPM 2012 R2 UR9 or later</span></span>

## <a name="dpm-protection-agent"></a><span data-ttu-id="06f97-117">DPM-skyddsagenten</span><span class="sxs-lookup"><span data-stu-id="06f97-117">DPM protection agent</span></span>
<span data-ttu-id="06f97-118">tooinstall hello DPM-skyddsagenten på hello Exchange-servern, Följ dessa steg:</span><span class="sxs-lookup"><span data-stu-id="06f97-118">tooinstall hello DPM protection agent on hello Exchange server, follow these steps:</span></span>

1. <span data-ttu-id="06f97-119">Kontrollera att hello brandväggar är korrekt konfigurerade.</span><span class="sxs-lookup"><span data-stu-id="06f97-119">Make sure that hello firewalls are correctly configured.</span></span> <span data-ttu-id="06f97-120">Se [konfigurera brandväggsundantag för agenten hello](https://technet.microsoft.com/library/Hh758204.aspx).</span><span class="sxs-lookup"><span data-stu-id="06f97-120">See [Configure firewall exceptions for hello agent](https://technet.microsoft.com/library/Hh758204.aspx).</span></span>
2. <span data-ttu-id="06f97-121">Installera hello-agenten på hello Exchange-servern genom att klicka på **Management > agenter > installera** i DPM-administratörskonsolen.</span><span class="sxs-lookup"><span data-stu-id="06f97-121">Install hello agent on hello Exchange server by clicking **Management > Agents > Install** in DPM Administrator Console.</span></span> <span data-ttu-id="06f97-122">Se [installera hello DPM-skyddsagenten](https://technet.microsoft.com/library/hh758186.aspx?f=255&MSPPError=-2147217396) detaljerade anvisningar.</span><span class="sxs-lookup"><span data-stu-id="06f97-122">See [Install hello DPM protection agent](https://technet.microsoft.com/library/hh758186.aspx?f=255&MSPPError=-2147217396) for detailed steps.</span></span>

## <a name="create-a-protection-group-for-hello-exchange-server"></a><span data-ttu-id="06f97-123">Skapa en skyddsgrupp för hello Exchange-server</span><span class="sxs-lookup"><span data-stu-id="06f97-123">Create a protection group for hello Exchange server</span></span>
1. <span data-ttu-id="06f97-124">Hello DPM-administratörskonsolen, klicka på **skydd**, och klicka sedan på **ny** på hello verktyget menyfliksområdet tooopen hello **Skapa ny Skyddsgrupp** guiden.</span><span class="sxs-lookup"><span data-stu-id="06f97-124">In hello DPM Administrator Console, click **Protection**, and then click **New** on hello tool ribbon tooopen hello **Create New Protection Group** wizard.</span></span>
2. <span data-ttu-id="06f97-125">På hello **Välkommen** skärmen hello guiden Klicka **nästa**.</span><span class="sxs-lookup"><span data-stu-id="06f97-125">On hello **Welcome** screen of hello wizard click **Next**.</span></span>
3. <span data-ttu-id="06f97-126">På hello **Välj typ av skyddsgrupp** , väljer **servrar** och på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="06f97-126">On hello **Select protection group type** screen, select **Servers** and click **Next**.</span></span>
4. <span data-ttu-id="06f97-127">Välj hello Exchange server-databas som du vill använda tooprotect och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="06f97-127">Select hello Exchange server database that you want tooprotect and click **Next**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="06f97-128">Om du skyddar Exchange 2013 Kontrollera hello [krav för Exchange 2013](https://technet.microsoft.com/library/dn751029.aspx).</span><span class="sxs-lookup"><span data-stu-id="06f97-128">If you are protecting Exchange 2013, check hello [Exchange 2013 prerequisites](https://technet.microsoft.com/library/dn751029.aspx).</span></span>
   >
   >

    <span data-ttu-id="06f97-129">I följande exempel hello, har hello Exchange 2010-databas valts.</span><span class="sxs-lookup"><span data-stu-id="06f97-129">In hello following example, hello Exchange 2010 database is selected.</span></span>

    ![Välj gruppmedlemmar](./media/backup-azure-backup-exchange-server/select-group-members.png)
5. <span data-ttu-id="06f97-131">Välj dataskyddsmetod hello.</span><span class="sxs-lookup"><span data-stu-id="06f97-131">Select hello data protection method.</span></span>

    <span data-ttu-id="06f97-132">Namnet hello skyddsgrupp och välj sedan båda hello följande alternativ:</span><span class="sxs-lookup"><span data-stu-id="06f97-132">Name hello protection group, and then select both of hello following options:</span></span>

   * <span data-ttu-id="06f97-133">Jag vill ha kortvarigt skydd med Disk.</span><span class="sxs-lookup"><span data-stu-id="06f97-133">I want short-term protection using Disk.</span></span>
   * <span data-ttu-id="06f97-134">Jag vill använda onlineskydd.</span><span class="sxs-lookup"><span data-stu-id="06f97-134">I want online protection.</span></span>
6. <span data-ttu-id="06f97-135">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="06f97-135">Click **Next**.</span></span>
7. <span data-ttu-id="06f97-136">Välj hello **kör Eseutil toocheck dataintegritet** om du vill toocheck hello integriteten hos hello Exchange Server-databaser.</span><span class="sxs-lookup"><span data-stu-id="06f97-136">Select hello **Run Eseutil toocheck data integrity** option if you want toocheck hello integrity of hello Exchange Server databases.</span></span>

    <span data-ttu-id="06f97-137">När du har valt det här alternativet säkerhetskopiering konsekvenskontroll kommer att köras på hello DPM server tooavoid hello i/o-trafik som genereras genom att köra hello **eseutil** på hello Exchange server.</span><span class="sxs-lookup"><span data-stu-id="06f97-137">After you select this option, backup consistency checking will be run on hello DPM server tooavoid hello I/O traffic that’s generated by running hello **eseutil** command on hello Exchange server.</span></span>

   > [!NOTE]
   > <span data-ttu-id="06f97-138">toouse detta alternativ, måste du kopiera hello Ese.dll och Eseutil.exe toohello C:\Program Files\Microsoft System Center 2012 R2\DPM\DPM\bin katalogen filer på hello DPM-servern.</span><span class="sxs-lookup"><span data-stu-id="06f97-138">toouse this option, you must copy hello Ese.dll and Eseutil.exe files toohello C:\Program Files\Microsoft System Center 2012 R2\DPM\DPM\bin directory on hello DPM server.</span></span> <span data-ttu-id="06f97-139">Annars utlöses hello följande fel:</span><span class="sxs-lookup"><span data-stu-id="06f97-139">Otherwise, hello following error is triggered:</span></span>  
   > <span data-ttu-id="06f97-140">![Eseutil-fel](./media/backup-azure-backup-exchange-server/eseutil-error.png)</span><span class="sxs-lookup"><span data-stu-id="06f97-140">![eseutil error](./media/backup-azure-backup-exchange-server/eseutil-error.png)</span></span>
   >
   >
8. <span data-ttu-id="06f97-141">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="06f97-141">Click **Next**.</span></span>
9. <span data-ttu-id="06f97-142">Välj hello databasen för **kopiering av säkerhetskopia**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="06f97-142">Select hello database for **Copy Backup**, and then click **Next**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="06f97-143">Om du inte väljer ”fullständiga säkerhetskopieringen” för minst en DAG kopia av en databas trunkeras inte loggar.</span><span class="sxs-lookup"><span data-stu-id="06f97-143">If you do not select “Full backup” for at least one DAG copy of a database, logs will not be truncated.</span></span>
   >
   >
10. <span data-ttu-id="06f97-144">Konfigurera hello mål för **kortsiktig säkerhetskopiering**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="06f97-144">Configure hello goals for **Short-Term backup**, and then click **Next**.</span></span>
11. <span data-ttu-id="06f97-145">Granska hello tillgängligt diskutrymme och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="06f97-145">Review hello available disk space, and then click **Next**.</span></span>
12. <span data-ttu-id="06f97-146">Välj hello tid vid vilken hello DPM server skapar hello inledande replikering, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="06f97-146">Select hello time at which hello DPM server will create hello initial replication, and then click **Next**.</span></span>
13. <span data-ttu-id="06f97-147">Välj alternativ för konsekvenskontroll hello och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="06f97-147">Select hello consistency check options, and then click **Next**.</span></span>
14. <span data-ttu-id="06f97-148">Välj hello-databas som du vill tooback in tooAzure och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="06f97-148">Choose hello database that you want tooback up tooAzure, and then click **Next**.</span></span> <span data-ttu-id="06f97-149">Exempel:</span><span class="sxs-lookup"><span data-stu-id="06f97-149">For example:</span></span>

    ![Ange onlineskyddsdata](./media/backup-azure-backup-exchange-server/specify-online-protection-data.png)
15. <span data-ttu-id="06f97-151">Definiera hello schema för **Azure Backup**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="06f97-151">Define hello schedule for **Azure Backup**, and then click **Next**.</span></span> <span data-ttu-id="06f97-152">Exempel:</span><span class="sxs-lookup"><span data-stu-id="06f97-152">For example:</span></span>

    ![Ange onlinesäkerhetskopieringsschema](./media/backup-azure-backup-exchange-server/specify-online-backup-schedule.png)

    > [!NOTE]
    > <span data-ttu-id="06f97-154">Observera onlineåterställningspunkter baseras på snabb och fullständig återställningspunkter.</span><span class="sxs-lookup"><span data-stu-id="06f97-154">Note Online recovery points are based on express full recovery points.</span></span> <span data-ttu-id="06f97-155">Därför måste du schemalägga hello onlineåterställningspunkt stund hello som har angetts för hello express fullständig återställningspunkt.</span><span class="sxs-lookup"><span data-stu-id="06f97-155">Therefore, you must schedule hello online recovery point after hello time that’s specified for hello express full recovery point.</span></span>
    >
    >
16. <span data-ttu-id="06f97-156">Konfigurera hello bevarandeprincipen för **Azure Backup**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="06f97-156">Configure hello retention policy for **Azure Backup**, and then click **Next**.</span></span>
17. <span data-ttu-id="06f97-157">Välj ett alternativ för onlinereplikeringsmetoden och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="06f97-157">Choose an online replication option and click **Next**.</span></span>

    <span data-ttu-id="06f97-158">Det kan ta lång tid för hello första säkerhetskopiering toobe skapade hello nätverket om du har en stor databas.</span><span class="sxs-lookup"><span data-stu-id="06f97-158">If you have a large database, it could take a long time for hello initial backup toobe created over hello network.</span></span> <span data-ttu-id="06f97-159">tooavoid problemet bör du skapa en offlinesäkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="06f97-159">tooavoid this issue, you can create an offline backup.</span></span>  

    ![Ange bevarandeprincip](./media/backup-azure-backup-exchange-server/specify-online-retention-policy.png)
18. <span data-ttu-id="06f97-161">Bekräfta inställningarna för hello och klicka sedan på **Skapa grupp**.</span><span class="sxs-lookup"><span data-stu-id="06f97-161">Confirm hello settings, and then click **Create Group**.</span></span>
19. <span data-ttu-id="06f97-162">Klicka på **Stäng**.</span><span class="sxs-lookup"><span data-stu-id="06f97-162">Click **Close**.</span></span>

## <a name="recover-hello-exchange-database"></a><span data-ttu-id="06f97-163">Återställa hello Exchange-databas</span><span class="sxs-lookup"><span data-stu-id="06f97-163">Recover hello Exchange database</span></span>
1. <span data-ttu-id="06f97-164">toorecover Exchange-databaser, klicka på **Recovery** i hello DPM-administratörskonsolen.</span><span class="sxs-lookup"><span data-stu-id="06f97-164">toorecover an Exchange database, click **Recovery** in hello DPM Administrator Console.</span></span>
2. <span data-ttu-id="06f97-165">Leta upp hello Exchange-databas som du vill toorecover.</span><span class="sxs-lookup"><span data-stu-id="06f97-165">Locate hello Exchange database that you want toorecover.</span></span>
3. <span data-ttu-id="06f97-166">Välj en onlineåterställningspunkt hello *återställningstid* listrutan.</span><span class="sxs-lookup"><span data-stu-id="06f97-166">Select an online recovery point from hello *recovery time* drop-down list.</span></span>
4. <span data-ttu-id="06f97-167">Klicka på **återställa** toostart hello **Återställningsguiden**.</span><span class="sxs-lookup"><span data-stu-id="06f97-167">Click **Recover** toostart hello **Recovery Wizard**.</span></span>

<span data-ttu-id="06f97-168">Onlineåterställningspunkter finns det fem typer av återställning:</span><span class="sxs-lookup"><span data-stu-id="06f97-168">For online recovery points, there are five recovery types:</span></span>

* <span data-ttu-id="06f97-169">**Återställa toooriginal Exchange-serverplatsen:** hello data kommer att återställda toohello ursprungliga Exchange-servern.</span><span class="sxs-lookup"><span data-stu-id="06f97-169">**Recover toooriginal Exchange Server location:** hello data will be recovered toohello original Exchange server.</span></span>
* <span data-ttu-id="06f97-170">**Återställa tooanother databas på en Exchange Server:** hello data kommer att återställda tooanother databas på en annan Exchange server.</span><span class="sxs-lookup"><span data-stu-id="06f97-170">**Recover tooanother database on an Exchange Server:** hello data will be recovered tooanother database on another Exchange server.</span></span>
* <span data-ttu-id="06f97-171">**Återställa tooa Återställningsdatabas:** hello data kommer att återställda tooan Exchange Recovery databasen (RDB).</span><span class="sxs-lookup"><span data-stu-id="06f97-171">**Recover tooa Recovery Database:** hello data will be recovered tooan Exchange Recovery Database (RDB).</span></span>
* <span data-ttu-id="06f97-172">**Kopiera tooa nätverksmapp:** hello data kommer att återställda tooa nätverksmapp.</span><span class="sxs-lookup"><span data-stu-id="06f97-172">**Copy tooa network folder:** hello data will be recovered tooa network folder.</span></span>
* <span data-ttu-id="06f97-173">**Kopiera tootape:** om du har ett bandbibliotek eller en fristående bandenhet som är anslutna och konfigurerat på hello hello återställningspunkt på DPM-servern kommer att kopieras tooa ledigt band.</span><span class="sxs-lookup"><span data-stu-id="06f97-173">**Copy tootape:** If you have a tape library or a stand-alone tape drive attached and configured on hello DPM server, hello recovery point will be copied tooa free tape.</span></span>

    ![Välj online replikering](./media/backup-azure-backup-exchange-server/choose-online-replication.png)

## <a name="next-steps"></a><span data-ttu-id="06f97-175">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="06f97-175">Next steps</span></span>
* [<span data-ttu-id="06f97-176">Azure Backup vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="06f97-176">Azure Backup FAQ</span></span>](backup-azure-backup-faq.md)
