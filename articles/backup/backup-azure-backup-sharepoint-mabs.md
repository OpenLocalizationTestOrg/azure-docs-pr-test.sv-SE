---
title: aaaUse Azure Backup server tooback upp en SharePoint-servergrupp tooAzure | Microsoft Docs
description: "Använd Azure Backup Server tooback och återställa SharePoint-data. Den här artikeln innehåller hello information tooconfigure SharePoint-servergruppen så att önskade data kan lagras i Azure. Du kan återställa skyddade SharePoint-data från disken eller från Azure."
services: backup
documentationcenter: 
author: pvrk
manager: shivamg
editor: 
ms.assetid: 34ba87a4-91f1-4054-a4a1-272af1e15496
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: pullabhk
ms.openlocfilehash: 350c1ac0f3518f400062f3b586bbe9710a79915a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-a-sharepoint-farm-tooazure"></a><span data-ttu-id="b57fb-105">Säkerhetskopiera en SharePoint-servergrupp tooAzure</span><span class="sxs-lookup"><span data-stu-id="b57fb-105">Back up a SharePoint farm tooAzure</span></span>
<span data-ttu-id="b57fb-106">Säkerhetskopiera en SharePoint webbservergrupp tooMicrosoft Azure med hjälp av Microsoft Azure Backup Server (MABS) i mycket hello samma sätt som du säkerhetskopiera andra datakällor.</span><span class="sxs-lookup"><span data-stu-id="b57fb-106">You back up a SharePoint farm tooMicrosoft Azure by using Microsoft Azure Backup Server (MABS) in much hello same way that you back up other data sources.</span></span> <span data-ttu-id="b57fb-107">Azure-säkerhetskopiering ger flexibilitet i hello Säkerhetskopieringsschemat toocreate dag, vecka, månad eller årlig säkerhetskopiering punkter och ger alternativ för kvarhållning av princip för säkerhetskopiering punkter.</span><span class="sxs-lookup"><span data-stu-id="b57fb-107">Azure Backup provides flexibility in hello backup schedule toocreate daily, weekly, monthly, or yearly backup points and gives you retention policy options for various backup points.</span></span> <span data-ttu-id="b57fb-108">Det ger också hello kapaciteten toostore lokal diskkopior för snabb återställningstiden mål (RTO) och toostore kopierar tooAzure för ekonomiska och långsiktig kvarhållning.</span><span class="sxs-lookup"><span data-stu-id="b57fb-108">It also provides hello capability toostore local disk copies for quick recovery-time objectives (RTO) and toostore copies tooAzure for economical, long-term retention.</span></span>

## <a name="sharepoint-supported-versions-and-related-protection-scenarios"></a><span data-ttu-id="b57fb-109">SharePoint-versioner som stöds och relaterade skyddsscenarier</span><span class="sxs-lookup"><span data-stu-id="b57fb-109">SharePoint supported versions and related protection scenarios</span></span>
<span data-ttu-id="b57fb-110">Azure Backup för DPM stöder hello följande scenarier:</span><span class="sxs-lookup"><span data-stu-id="b57fb-110">Azure Backup for DPM supports hello following scenarios:</span></span>

| <span data-ttu-id="b57fb-111">Arbetsbelastning</span><span class="sxs-lookup"><span data-stu-id="b57fb-111">Workload</span></span> | <span data-ttu-id="b57fb-112">Version</span><span class="sxs-lookup"><span data-stu-id="b57fb-112">Version</span></span> | <span data-ttu-id="b57fb-113">Distribution av SharePoint</span><span class="sxs-lookup"><span data-stu-id="b57fb-113">SharePoint deployment</span></span> | <span data-ttu-id="b57fb-114">Skydd och återställning</span><span class="sxs-lookup"><span data-stu-id="b57fb-114">Protection and recovery</span></span> |
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="b57fb-115">SharePoint</span><span class="sxs-lookup"><span data-stu-id="b57fb-115">SharePoint</span></span> |<span data-ttu-id="b57fb-116">SharePoint 2013, SharePoint 2010 SharePoint 2007 SharePoint 3.0</span><span class="sxs-lookup"><span data-stu-id="b57fb-116">SharePoint 2013, SharePoint 2010, SharePoint 2007, SharePoint 3.0</span></span> |<span data-ttu-id="b57fb-117">SharePoint distribueras som en fysisk server eller VMware-Hyper-V virtuell dator</span><span class="sxs-lookup"><span data-stu-id="b57fb-117">SharePoint deployed as a physical server or Hyper-V/VMware virtual machine</span></span> <br> -------------- <br> <span data-ttu-id="b57fb-118">SQL AlwaysOn</span><span class="sxs-lookup"><span data-stu-id="b57fb-118">SQL AlwaysOn</span></span> | <span data-ttu-id="b57fb-119">Skydda SharePoint-servergruppen återställningsalternativ: återställningsgruppen, databas och fil- eller listobjekt från diskåterställningspunkter.</span><span class="sxs-lookup"><span data-stu-id="b57fb-119">Protect SharePoint Farm recovery options: Recovery farm, database, and file or list item from disk recovery points.</span></span>  <span data-ttu-id="b57fb-120">Servergruppen och databasen återställning från återställningspunkter i Azure.</span><span class="sxs-lookup"><span data-stu-id="b57fb-120">Farm and database recovery from Azure recovery points.</span></span> |

## <a name="before-you-start"></a><span data-ttu-id="b57fb-121">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="b57fb-121">Before you start</span></span>
<span data-ttu-id="b57fb-122">Det finns några saker du behöver tooconfirm innan du säkerhetskopierar tooAzure en SharePoint-servergruppen.</span><span class="sxs-lookup"><span data-stu-id="b57fb-122">There are a few things you need tooconfirm before you back up a SharePoint farm tooAzure.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="b57fb-123">Krav</span><span class="sxs-lookup"><span data-stu-id="b57fb-123">Prerequisites</span></span>
<span data-ttu-id="b57fb-124">Innan du fortsätter, kontrollera att du har [installerad och förberedda hello Azure Backup Server](backup-azure-microsoft-azure-backup.md) tooprotect arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="b57fb-124">Before you proceed, make sure that you have [installed and prepared hello Azure Backup Server](backup-azure-microsoft-azure-backup.md) tooprotect workloads.</span></span>

### <a name="protection-agent"></a><span data-ttu-id="b57fb-125">Skyddsagent</span><span class="sxs-lookup"><span data-stu-id="b57fb-125">Protection agent</span></span>
<span data-ttu-id="b57fb-126">Hej skyddsagenten måste installeras på hello-server som kör SharePoint hello-servrar som kör SQL Server och alla andra servrar som ingår i hello SharePoint-servergruppen.</span><span class="sxs-lookup"><span data-stu-id="b57fb-126">hello Protection agent must be installed on hello server that's running SharePoint, hello servers that are running SQL Server, and all other servers that are part of hello SharePoint farm.</span></span> <span data-ttu-id="b57fb-127">Mer information om hur tooset in hello protection agent finns [installationsprogrammet Protection Agent](https://technet.microsoft.com/library/hh758034\(v=sc.12\).aspx).</span><span class="sxs-lookup"><span data-stu-id="b57fb-127">For more information about how tooset up hello protection agent, see [Setup Protection Agent](https://technet.microsoft.com/library/hh758034\(v=sc.12\).aspx).</span></span>  <span data-ttu-id="b57fb-128">hello enda undantaget är att du installerar hello agenten endast på en enda webbserver för klientdel (WFE).</span><span class="sxs-lookup"><span data-stu-id="b57fb-128">hello one exception is that you install hello agent only on a single web front end (WFE) server.</span></span> <span data-ttu-id="b57fb-129">DPM måste hello-agenten på en WFE-servern endast tooserve som hello startpunkt för skydd.</span><span class="sxs-lookup"><span data-stu-id="b57fb-129">DPM needs hello agent on one WFE server only tooserve as hello entry point for protection.</span></span>

### <a name="sharepoint-farm"></a><span data-ttu-id="b57fb-130">SharePoint-grupp</span><span class="sxs-lookup"><span data-stu-id="b57fb-130">SharePoint farm</span></span>
<span data-ttu-id="b57fb-131">För per 10 miljoner objekt i hello servergruppen måste det finnas minst 2 GB utrymme på hello volym där hello MABS mappen finns.</span><span class="sxs-lookup"><span data-stu-id="b57fb-131">For every 10 million items in hello farm, there must be at least 2 GB of space on hello volume where hello MABS folder is located.</span></span> <span data-ttu-id="b57fb-132">Den här utrymme som krävs för kataloggenerering.</span><span class="sxs-lookup"><span data-stu-id="b57fb-132">This space is required for catalog generation.</span></span> <span data-ttu-id="b57fb-133">För MABS toorecover specifika objekt (webbplatssamlingar, platser, listor, dokumentbibliotek, mappar, enskilda dokument och listobjekt) skapas kataloggenerering en lista över hello webbadresser som ingår i varje innehållsdatabas.</span><span class="sxs-lookup"><span data-stu-id="b57fb-133">For MABS toorecover specific items (site collections, sites, lists, document libraries, folders, individual documents, and list items), catalog generation creates a list of hello URLs that are contained within each content database.</span></span> <span data-ttu-id="b57fb-134">Du kan visa hello lista över URL: er hello återställningsbara objekt i fönstret i hello **Recovery** aktivitetsområdet MABS-administratörskonsolen.</span><span class="sxs-lookup"><span data-stu-id="b57fb-134">You can view hello list of URLs in hello recoverable item pane in hello **Recovery** task area of MABS Administrator Console.</span></span>

### <a name="sql-server"></a><span data-ttu-id="b57fb-135">SQL Server</span><span class="sxs-lookup"><span data-stu-id="b57fb-135">SQL Server</span></span>
<span data-ttu-id="b57fb-136">MABS körs som LocalSystem-kontot.</span><span class="sxs-lookup"><span data-stu-id="b57fb-136">MABS runs as a LocalSystem account.</span></span> <span data-ttu-id="b57fb-137">tooback in SQL Server-databaser MABS behöver sysadmin-behörigheter på det kontot för hello-server som kör SQL Server.</span><span class="sxs-lookup"><span data-stu-id="b57fb-137">tooback up SQL Server databases, MABS needs sysadmin privileges on that account for hello server that's running SQL Server.</span></span> <span data-ttu-id="b57fb-138">Ställa in NT AUTHORITY\SYSTEM också*sysadmin* på hello-server som kör SQL Server innan du säkerhetskopiera den.</span><span class="sxs-lookup"><span data-stu-id="b57fb-138">Set NT AUTHORITY\SYSTEM too*sysadmin* on hello server that's running SQL Server before you back it up.</span></span>

<span data-ttu-id="b57fb-139">Om hello SharePoint-servergruppen har SQL Server-databaser som är konfigurerade med SQL Server-alias, installerar du hello SQL Server-klientkomponenterna på hello frontwebbserver som MABS ska skydda.</span><span class="sxs-lookup"><span data-stu-id="b57fb-139">If hello SharePoint farm has SQL Server databases that are configured with SQL Server aliases, install hello SQL Server client components on hello front-end Web server that MABS will protect.</span></span>

### <a name="sharepoint-server"></a><span data-ttu-id="b57fb-140">SharePoint Server</span><span class="sxs-lookup"><span data-stu-id="b57fb-140">SharePoint Server</span></span>
<span data-ttu-id="b57fb-141">Medan prestanda beror på många faktorer, till exempel storleken på SharePoint-servergruppen, som en allmän vägledning kan en MABS skydda en SharePoint-grupp med 25 TB.</span><span class="sxs-lookup"><span data-stu-id="b57fb-141">While performance depends on many factors such as size of SharePoint farm, as general guidance one MABS can protect a 25 TB SharePoint farm.</span></span>

### <a name="whats-not-supported"></a><span data-ttu-id="b57fb-142">Vad som inte stöds</span><span class="sxs-lookup"><span data-stu-id="b57fb-142">What's not supported</span></span>
* <span data-ttu-id="b57fb-143">MABS som skyddar en SharePoint-servergrupp skyddar inte sökindexen eller databaser för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="b57fb-143">MABS that protects a SharePoint farm does not protect search indexes or application service databases.</span></span> <span data-ttu-id="b57fb-144">Du behöver tooconfigure hello skyddet av databaserna separat.</span><span class="sxs-lookup"><span data-stu-id="b57fb-144">You will need tooconfigure hello protection of these databases separately.</span></span>
* <span data-ttu-id="b57fb-145">MABS ger inte någon säkerhetskopia av SharePoint SQL Server-databaser som finns på filresurser för skalbar filserver (SOFS).</span><span class="sxs-lookup"><span data-stu-id="b57fb-145">MABS does not provide backup of SharePoint SQL Server databases that are hosted on scale-out file server (SOFS) shares.</span></span>

## <a name="configure-sharepoint-protection"></a><span data-ttu-id="b57fb-146">Konfigurera SharePoint-skydd</span><span class="sxs-lookup"><span data-stu-id="b57fb-146">Configure SharePoint protection</span></span>
<span data-ttu-id="b57fb-147">Innan du kan använda MABS tooprotect SharePoint, måste du konfigurera hello SharePoint VSS Writer-tjänsten (WSS Writer) med hjälp av **ConfigureSharePoint.exe**.</span><span class="sxs-lookup"><span data-stu-id="b57fb-147">Before you can use MABS tooprotect SharePoint, you must configure hello SharePoint VSS Writer service (WSS Writer service) by using **ConfigureSharePoint.exe**.</span></span>

<span data-ttu-id="b57fb-148">Du kan hitta **ConfigureSharePoint.exe** i hello [MABS installationssökväg] \bin mappen på hello frontwebbservern.</span><span class="sxs-lookup"><span data-stu-id="b57fb-148">You can find **ConfigureSharePoint.exe** in hello [MABS Installation Path]\bin folder on hello front-end web server.</span></span> <span data-ttu-id="b57fb-149">Det här verktyget innehåller hello skyddsagenten med hello autentiseringsuppgifter för hello SharePoint-servergruppen.</span><span class="sxs-lookup"><span data-stu-id="b57fb-149">This tool provides hello protection agent with hello credentials for hello SharePoint farm.</span></span> <span data-ttu-id="b57fb-150">Du kan köra den på en enskild WFE-server.</span><span class="sxs-lookup"><span data-stu-id="b57fb-150">You run it on a single WFE server.</span></span> <span data-ttu-id="b57fb-151">Om du har flera WFE-servrar kan du bara välja en när du konfigurerar en skyddsgrupp.</span><span class="sxs-lookup"><span data-stu-id="b57fb-151">If you have multiple WFE servers, select just one when you configure a protection group.</span></span>

### <a name="tooconfigure-hello-sharepoint-vss-writer-service"></a><span data-ttu-id="b57fb-152">tooconfigure hello SharePoint VSS Writer-tjänsten</span><span class="sxs-lookup"><span data-stu-id="b57fb-152">tooconfigure hello SharePoint VSS Writer service</span></span>
1. <span data-ttu-id="b57fb-153">Hello WFE-serven i Kommandotolken, gå för \bin\ [MABS installationsplats]</span><span class="sxs-lookup"><span data-stu-id="b57fb-153">On hello WFE server, at a command prompt, go too[MABS installation location]\bin\\</span></span>
2. <span data-ttu-id="b57fb-154">Ange ConfigureSharePoint - EnableSharePointProtection.</span><span class="sxs-lookup"><span data-stu-id="b57fb-154">Enter ConfigureSharePoint -EnableSharePointProtection.</span></span>
3. <span data-ttu-id="b57fb-155">Ange hello-autentiseringsuppgifter som gruppadministratör.</span><span class="sxs-lookup"><span data-stu-id="b57fb-155">Enter hello farm administrator credentials.</span></span> <span data-ttu-id="b57fb-156">Det här kontot ska vara medlem i hello lokala administratörsgruppen på hello WFE-servern.</span><span class="sxs-lookup"><span data-stu-id="b57fb-156">This account should be a member of hello local Administrator group on hello WFE server.</span></span> <span data-ttu-id="b57fb-157">Om hello servergruppsadministratören är inte en lokal administratör bevilja hello följande behörigheter på WFE-serven hello:</span><span class="sxs-lookup"><span data-stu-id="b57fb-157">If hello farm administrator isn’t a local admin grant hello following permissions on hello WFE server:</span></span>
   * <span data-ttu-id="b57fb-158">Bevilja hello WSS_Admin_WPG-gruppen fullständig kontroll toohello DPM mappen (% Program Files%\Microsoft Azure Backup\DPM).</span><span class="sxs-lookup"><span data-stu-id="b57fb-158">Grant hello WSS_Admin_WPG group full control toohello DPM folder (%Program Files%\Microsoft Azure Backup\DPM).</span></span>
   * <span data-ttu-id="b57fb-159">Bevilja hello WSS_Admin_WPG-gruppen läsbehörighet toohello DPM registernyckeln (HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft Data Protection Manager).</span><span class="sxs-lookup"><span data-stu-id="b57fb-159">Grant hello WSS_Admin_WPG group read access toohello DPM Registry key (HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft Data Protection Manager).</span></span>

> [!NOTE]
> <span data-ttu-id="b57fb-160">Du behöver toorerun ConfigureSharePoint.exe när en ändring i hello SharePoint-autentiseringsuppgifter som gruppadministratör.</span><span class="sxs-lookup"><span data-stu-id="b57fb-160">You’ll need toorerun ConfigureSharePoint.exe whenever there’s a change in hello SharePoint farm administrator credentials.</span></span>
>
>

## <a name="back-up-a-sharepoint-farm-by-using-mabs"></a><span data-ttu-id="b57fb-161">Säkerhetskopiera en SharePoint-servergrupp med hjälp av MABS</span><span class="sxs-lookup"><span data-stu-id="b57fb-161">Back up a SharePoint farm by using MABS</span></span>
<span data-ttu-id="b57fb-162">När du har konfigurerat MABS och hello som tidigare förklarats SharePoint-servergrupp, skyddas SharePoint av MABS.</span><span class="sxs-lookup"><span data-stu-id="b57fb-162">After you have configured MABS and hello SharePoint farm as explained previously, SharePoint can be protected by MABS.</span></span>

### <a name="tooprotect-a-sharepoint-farm"></a><span data-ttu-id="b57fb-163">tooprotect en SharePoint-grupp</span><span class="sxs-lookup"><span data-stu-id="b57fb-163">tooprotect a SharePoint farm</span></span>
1. <span data-ttu-id="b57fb-164">Från hello **skydd** av hello MABS administratörskonsolen, klicka på **ny**.</span><span class="sxs-lookup"><span data-stu-id="b57fb-164">From hello **Protection** tab of hello MABS Administrator Console, click **New**.</span></span>
    <span data-ttu-id="b57fb-165">![Skyddsfliken](./media/backup-azure-backup-sharepoint/dpm-new-protection-tab.png)</span><span class="sxs-lookup"><span data-stu-id="b57fb-165">![New Protection Tab](./media/backup-azure-backup-sharepoint/dpm-new-protection-tab.png)</span></span>
2. <span data-ttu-id="b57fb-166">På hello **Välj typ av Skyddsgrupp** sidan hello **Skapa ny Skyddsgrupp** guiden, Välj **servrar**, och klicka sedan på **nästa** .</span><span class="sxs-lookup"><span data-stu-id="b57fb-166">On hello **Select Protection Group Type** page of hello **Create New Protection Group** wizard, select **Servers**, and then click **Next**.</span></span>

    ![Välj Skyddsgruppen typ](./media/backup-azure-backup-sharepoint/select-protection-group-type.png)
3. <span data-ttu-id="b57fb-168">På hello **Välj gruppmedlemmar** skärmen, Välj hello kryssrutan för hello SharePoint-server du vill tooprotect och på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="b57fb-168">On hello **Select Group Members** screen, select hello check box for hello SharePoint server you want tooprotect and click **Next**.</span></span>

    ![Välj gruppmedlemmar](./media/backup-azure-backup-sharepoint/select-group-members2.png)

   > [!NOTE]
   > <span data-ttu-id="b57fb-170">Du kan se hello server i hello guiden med hello protection agent har installerats.</span><span class="sxs-lookup"><span data-stu-id="b57fb-170">With hello protection agent installed, you can see hello server in hello wizard.</span></span> <span data-ttu-id="b57fb-171">MABS visar även dess struktur.</span><span class="sxs-lookup"><span data-stu-id="b57fb-171">MABS also shows its structure.</span></span> <span data-ttu-id="b57fb-172">Eftersom du körde ConfigureSharePoint.exe MABS kommunicerar med hello SharePoint VSS Writer-tjänsten och dess motsvarande SQL Server-databaser och identifierar hello SharePoint-servergrupp struktur, hello associerade innehållsdatabaser och motsvarande objekt.</span><span class="sxs-lookup"><span data-stu-id="b57fb-172">Because you ran ConfigureSharePoint.exe, MABS communicates with hello SharePoint VSS Writer service and its corresponding SQL Server databases and recognizes hello SharePoint farm structure, hello associated content databases, and any corresponding items.</span></span>
   >
   >
4. <span data-ttu-id="b57fb-173">På hello **Välj Dataskyddsmetod** anger hello namnet på hello **Skyddsgrupp**, och välj *skyddsmetod*.</span><span class="sxs-lookup"><span data-stu-id="b57fb-173">On hello **Select Data Protection Method** page, enter hello name of hello **Protection Group**, and select your preferred *protection methods*.</span></span> <span data-ttu-id="b57fb-174">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="b57fb-174">Click **Next**.</span></span>

    ![Välj dataskyddsmetod](./media/backup-azure-backup-sharepoint/select-data-protection-method1.png)

   > [!NOTE]
   > <span data-ttu-id="b57fb-176">hello disk skyddsmetod hjälper toomeet kort återställningstiden mål.</span><span class="sxs-lookup"><span data-stu-id="b57fb-176">hello disk protection method helps toomeet short recovery-time objectives.</span></span>
   >
   >
5. <span data-ttu-id="b57fb-177">På hello **ange kortvariga mål** sidan, Välj **Kvarhållningsintervall** och identifiera när du vill att säkerhetskopieringar toooccur.</span><span class="sxs-lookup"><span data-stu-id="b57fb-177">On hello **Specify Short-Term Goals** page, select your preferred **Retention range** and identify when you want backups toooccur.</span></span>

    ![Ange kortsiktiga mål](./media/backup-azure-backup-sharepoint/specify-short-term-goals2.png)

   > [!NOTE]
   > <span data-ttu-id="b57fb-179">Eftersom återställningen krävs oftast för data som är mindre än fem dagar gammal, vi valt ett Kvarhållningsintervall på fem dagar på disken och kontrollerat att hello säkerhetskopiering sker under icke-produktionstider för det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="b57fb-179">Because recovery is most often required for data that's less than five days old, we selected a retention range of five days on disk and ensured that hello backup happens during non-production hours, for this example.</span></span>
   >
   >
6. <span data-ttu-id="b57fb-180">Granska hello lagring lagringspoolens allokerade diskutrymme för hello skyddsgrupp och sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="b57fb-180">Review hello storage pool disk space allocated for hello protection group, and click then **Next**.</span></span>
7. <span data-ttu-id="b57fb-181">MABS allokerar disk space toostore och hantera repliker för varje skyddsgrupp.</span><span class="sxs-lookup"><span data-stu-id="b57fb-181">For every protection group, MABS allocates disk space toostore and manage replicas.</span></span> <span data-ttu-id="b57fb-182">MABS måste nu skapa en kopia av hello valda data.</span><span class="sxs-lookup"><span data-stu-id="b57fb-182">At this point, MABS must create a copy of hello selected data.</span></span> <span data-ttu-id="b57fb-183">Välj hur och när du vill hello replik som har skapats och klickar sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="b57fb-183">Select how and when you want hello replica created, and then click **Next**.</span></span>

    ![Välj metod för skapande av replik](./media/backup-azure-backup-sharepoint/choose-replica-creation-method.png)

   > [!NOTE]
   > <span data-ttu-id="b57fb-185">toomake till att inte sker nätverkstrafik, Välj en tid utanför produktionstider.</span><span class="sxs-lookup"><span data-stu-id="b57fb-185">toomake sure that network traffic is not effected, select a time outside production hours.</span></span>
   >
   >
8. <span data-ttu-id="b57fb-186">MABS garanterar dataintegriteten genom att utföra konsekvenskontroller på hello replik.</span><span class="sxs-lookup"><span data-stu-id="b57fb-186">MABS ensures data integrity by performing consistency checks on hello replica.</span></span> <span data-ttu-id="b57fb-187">Det finns två tillgängliga alternativ.</span><span class="sxs-lookup"><span data-stu-id="b57fb-187">There are two available options.</span></span> <span data-ttu-id="b57fb-188">Du kan definiera ett schema toorun konsekvenskontroller eller DPM kan köra konsekvenskontroller automatiskt på hello replik när det blir inkonsekvent.</span><span class="sxs-lookup"><span data-stu-id="b57fb-188">You can define a schedule toorun consistency checks, or DPM can run consistency checks automatically on hello replica whenever it becomes inconsistent.</span></span> <span data-ttu-id="b57fb-189">Välj önskade alternativ och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="b57fb-189">Select your preferred option, and then click **Next**.</span></span>

    ![Konsekvenskontroll](./media/backup-azure-backup-sharepoint/consistency-check.png)
9. <span data-ttu-id="b57fb-191">På hello **ange Onlineskyddsdata** väljer hello SharePoint-grupp som du vill tooprotect och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="b57fb-191">On hello **Specify Online Protection Data** page, select hello SharePoint farm that you want tooprotect, and then click **Next**.</span></span>

    ![DPM SharePoint Protection1](./media/backup-azure-backup-sharepoint/select-online-protection1.png)
10. <span data-ttu-id="b57fb-193">På hello **Ange schema för Online säkerhetskopiering** , väljer önskat schema, och klickar sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="b57fb-193">On hello **Specify Online Backup Schedule** page, select your preferred schedule, and then click **Next**.</span></span>

    ![Online_backup_schedule](./media/backup-azure-backup-sharepoint/specify-online-backup-schedule.png)

    > [!NOTE]
    > <span data-ttu-id="b57fb-195">MABS ger högst två dagliga säkerhetskopior tooAzure från hello sedan tillgänglig senaste disk säkerhetskopieringspunkt.</span><span class="sxs-lookup"><span data-stu-id="b57fb-195">MABS provides a maximum of two daily backups tooAzure from hello then available latest disk backup point.</span></span> <span data-ttu-id="b57fb-196">Azure Backup kan också styra hello mängden WAN-bandbredd som kan användas för säkerhetskopieringar i belastning och låg belastning via [Azure Backup nätverksbegränsning](https://azure.microsoft.com/documentation/articles/backup-configure-vault/#enable-network-throttling).</span><span class="sxs-lookup"><span data-stu-id="b57fb-196">Azure Backup can also control hello amount of WAN bandwidth that can be used for backups in peak and off-peak hours by using [Azure Backup Network Throttling](https://azure.microsoft.com/documentation/articles/backup-configure-vault/#enable-network-throttling).</span></span>
    >
    >
11. <span data-ttu-id="b57fb-197">Beroende på hello säkerhetskopieringsschema som du valde på hello **ange bevarandeprincip** sidan Välj hello bevarandeprincip för dagliga, veckovisa, månatliga och årliga säkerhetskopiering punkter.</span><span class="sxs-lookup"><span data-stu-id="b57fb-197">Depending on hello backup schedule that you selected, on hello **Specify Online Retention Policy** page, select hello retention policy for daily, weekly, monthly, and yearly backup points.</span></span>

    ![Online_retention_policy](./media/backup-azure-backup-sharepoint/specify-online-retention.png)

    > [!NOTE]
    > <span data-ttu-id="b57fb-199">MABS använder ett farfar-pappa-son kvarhållning-schema som du kan välja en annan bevarandeprincip för olika tidpunkter som säkerhetskopieringen.</span><span class="sxs-lookup"><span data-stu-id="b57fb-199">MABS uses a grandfather-father-son retention scheme in which a different retention policy can be chosen for different backup points.</span></span>
    >
    >
12. <span data-ttu-id="b57fb-200">Liknande toodisk en referens för inledande punkt replik måste toobe som skapats i Azure.</span><span class="sxs-lookup"><span data-stu-id="b57fb-200">Similar toodisk, an initial reference point replica needs toobe created in Azure.</span></span> <span data-ttu-id="b57fb-201">Välj din önskade alternativ toocreate tooAzure en första säkerhetskopiering och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="b57fb-201">Select your preferred option toocreate an initial backup copy tooAzure, and then click **Next**.</span></span>

    ![Online_replica](./media/backup-azure-backup-sharepoint/online-replication.png)
13. <span data-ttu-id="b57fb-203">Granska de angivna inställningarna på hello **sammanfattning** , och klickar sedan på **Skapa grupp**.</span><span class="sxs-lookup"><span data-stu-id="b57fb-203">Review your selected settings on hello **Summary** page, and then click **Create Group**.</span></span> <span data-ttu-id="b57fb-204">Ett meddelande visas när hello skyddsgruppen har skapats.</span><span class="sxs-lookup"><span data-stu-id="b57fb-204">You will see a success message after hello protection group has been created.</span></span>

    ![Sammanfattning](./media/backup-azure-backup-sharepoint/summary.png)

## <a name="restore-a-sharepoint-item-from-disk-by-using-mabs"></a><span data-ttu-id="b57fb-206">Återställa SharePoint-objektet från disken med hjälp av MABS</span><span class="sxs-lookup"><span data-stu-id="b57fb-206">Restore a SharePoint item from disk by using MABS</span></span>
<span data-ttu-id="b57fb-207">I följande exempel hello, hello *återställa SharePoint-objektet* har tagits bort av misstag och måste toobe återställs.</span><span class="sxs-lookup"><span data-stu-id="b57fb-207">In hello following example, hello *Recovering SharePoint item* has been accidentally deleted and needs toobe recovered.</span></span>
<span data-ttu-id="b57fb-208">![MABS SharePoint Protection4](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection5.png)</span><span class="sxs-lookup"><span data-stu-id="b57fb-208">![MABS SharePoint Protection4](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection5.png)</span></span>

1. <span data-ttu-id="b57fb-209">Öppna hello **DPM-administratörskonsolen**.</span><span class="sxs-lookup"><span data-stu-id="b57fb-209">Open hello **DPM Administrator Console**.</span></span> <span data-ttu-id="b57fb-210">Alla SharePoint-servergrupper som skyddas av DPM visas i hello **skydd** fliken.</span><span class="sxs-lookup"><span data-stu-id="b57fb-210">All SharePoint farms that are protected by DPM are shown in hello **Protection** tab.</span></span>

    ![MABS SharePoint Protection3](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection4.png)
2. <span data-ttu-id="b57fb-212">toobegin toorecover hello artikel, Välj hello **Recovery** fliken.</span><span class="sxs-lookup"><span data-stu-id="b57fb-212">toobegin toorecover hello item, select hello **Recovery** tab.</span></span>

    ![MABS SharePoint Protection5](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection6.png)
3. <span data-ttu-id="b57fb-214">Du kan söka SharePoint för *återställa SharePoint-objektet* peka intervall genom att använda en sökning med jokertecken-baserade inom en återställning.</span><span class="sxs-lookup"><span data-stu-id="b57fb-214">You can search SharePoint for *Recovering SharePoint item* by using a wildcard-based search within a recovery point range.</span></span>

    ![MABS SharePoint Protection6](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection7.png)
4. <span data-ttu-id="b57fb-216">Markera hello lämpliga återställningspunkt från hello sökresultat, högerklicka hello objektet och välj sedan **återställa**.</span><span class="sxs-lookup"><span data-stu-id="b57fb-216">Select hello appropriate recovery point from hello search results, right-click hello item, and then select **Recover**.</span></span>
5. <span data-ttu-id="b57fb-217">Du kan också bläddra igenom olika återställningspunkter och välja en databas eller objektet toorecover.</span><span class="sxs-lookup"><span data-stu-id="b57fb-217">You can also browse through various recovery points and select a database or item toorecover.</span></span> <span data-ttu-id="b57fb-218">Välj **datum > återställningstid**, och välj sedan hello rätt **databasen > SharePoint-servergruppen > återställningspunkt > objektet**.</span><span class="sxs-lookup"><span data-stu-id="b57fb-218">Select **Date > Recovery time**, and then select hello correct **Database > SharePoint farm > Recovery point > Item**.</span></span>

    ![MABS SharePoint Protection7](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection8.png)
6. <span data-ttu-id="b57fb-220">Högerklicka på hello objekt och välj sedan **återställa** tooopen hello **Återställningsguiden**.</span><span class="sxs-lookup"><span data-stu-id="b57fb-220">Right-click hello item, and then select **Recover** tooopen hello **Recovery Wizard**.</span></span> <span data-ttu-id="b57fb-221">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="b57fb-221">Click **Next**.</span></span>

    ![Granska val av återställning](./media/backup-azure-backup-sharepoint/review-recovery-selection.png)
7. <span data-ttu-id="b57fb-223">Välj hello typ av återställning du vill tooperform och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="b57fb-223">Select hello type of recovery that you want tooperform, and then click **Next**.</span></span>

    ![Typ av återställning](./media/backup-azure-backup-sharepoint/select-recovery-type.png)

   > [!NOTE]
   > <span data-ttu-id="b57fb-225">Hej val av **återställa toooriginal** i hello exempel återställer hello-objektet toohello ursprungliga SharePoint-webbplatsen.</span><span class="sxs-lookup"><span data-stu-id="b57fb-225">hello selection of **Recover toooriginal** in hello example recovers hello item toohello original SharePoint site.</span></span>
   >
   >
8. <span data-ttu-id="b57fb-226">Välj hello **återställningsprocessen** som du vill toouse.</span><span class="sxs-lookup"><span data-stu-id="b57fb-226">Select hello **Recovery Process** that you want toouse.</span></span>

   * <span data-ttu-id="b57fb-227">Välj **återställa utan en återställningsgrupp** om hello SharePoint-servergruppen inte har ändrats och hello samma som hello recovery peka som återställs.</span><span class="sxs-lookup"><span data-stu-id="b57fb-227">Select **Recover without using a recovery farm** if hello SharePoint farm has not changed and is hello same as hello recovery point that is being restored.</span></span>
   * <span data-ttu-id="b57fb-228">Välj **återställa en återställningsgrupp** om hello SharePoint-servergruppen har ändrats sedan hello återställningspunkten skapades.</span><span class="sxs-lookup"><span data-stu-id="b57fb-228">Select **Recover using a recovery farm** if hello SharePoint farm has changed since hello recovery point was created.</span></span>

     ![Återställningsprocessen](./media/backup-azure-backup-sharepoint/recovery-process.png)
9. <span data-ttu-id="b57fb-230">Ange en fristående SQL Server-instansen plats toorecover hello-databas tillfälligt och ger en fristående filresurs MABS och hello-server som kör SharePoint toorecover hello-objektet.</span><span class="sxs-lookup"><span data-stu-id="b57fb-230">Provide a staging SQL Server instance location toorecover hello database temporarily, and provide a staging file share on MABS and hello server that's running SharePoint toorecover hello item.</span></span>

    ![Mellanlagring Location1](./media/backup-azure-backup-sharepoint/staging-location1.png)

    <span data-ttu-id="b57fb-232">MABS bifogar hello innehållsdatabasen som är värd för hello SharePoint objektet toohello tillfällig SQL Server-instans.</span><span class="sxs-lookup"><span data-stu-id="b57fb-232">MABS attaches hello content database that is hosting hello SharePoint item toohello temporary SQL Server instance.</span></span> <span data-ttu-id="b57fb-233">Från hello innehållsdatabasen, återställer hello-objekt och placeras på hello mellanlagringsplatsen fil på MABS.</span><span class="sxs-lookup"><span data-stu-id="b57fb-233">From hello content database, it recovers hello item and puts it on hello staging file location on MABS.</span></span> <span data-ttu-id="b57fb-234">hello återställa objekt som finns på hello mellanlagringsplatsen nu behov toobe exporteras toohello mellanlagringsplatsen på hello SharePoint-servergruppen.</span><span class="sxs-lookup"><span data-stu-id="b57fb-234">hello recovered item that's on hello staging location now needs toobe exported toohello staging location on hello SharePoint farm.</span></span>

    ![Mellanlagring Location2](./media/backup-azure-backup-sharepoint/staging-location2.png)
10. <span data-ttu-id="b57fb-236">Välj **Ange återställningsalternativ**, och säkerhet inställningar toohello SharePoint-servergrupp eller använda hello säkerhetsinställningar hello återställningspunkt.</span><span class="sxs-lookup"><span data-stu-id="b57fb-236">Select **Specify recovery options**, and apply security settings toohello SharePoint farm or apply hello security settings of hello recovery point.</span></span> <span data-ttu-id="b57fb-237">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="b57fb-237">Click **Next**.</span></span>

    ![Återställningsalternativ](./media/backup-azure-backup-sharepoint/recovery-options.png)

    > [!NOTE]
    > <span data-ttu-id="b57fb-239">Du kan välja toothrottle hello användningen av nätverksbandbredd.</span><span class="sxs-lookup"><span data-stu-id="b57fb-239">You can choose toothrottle hello network bandwidth usage.</span></span> <span data-ttu-id="b57fb-240">Detta minskar inverkan toohello produktionsservern under produktionstider.</span><span class="sxs-lookup"><span data-stu-id="b57fb-240">This minimizes impact toohello production server during production hours.</span></span>
    >
    >
11. <span data-ttu-id="b57fb-241">Granska hello sammanfattningsinformation och klicka sedan på **återställa** toobegin återställning av hello-filen.</span><span class="sxs-lookup"><span data-stu-id="b57fb-241">Review hello summary information, and then click **Recover** toobegin recovery of hello file.</span></span>

    ![Översikt över återställning](./media/backup-azure-backup-sharepoint/recovery-summary.png)
12. <span data-ttu-id="b57fb-243">Nu välja hello **övervakning** fliken i hello **MABS administratörskonsolen** tooview hello **Status** av hello återställning.</span><span class="sxs-lookup"><span data-stu-id="b57fb-243">Now select hello **Monitoring** tab in hello **MABS Administrator Console** tooview hello **Status** of hello recovery.</span></span>

    ![Status för återställning](./media/backup-azure-backup-sharepoint/recovery-monitoring.png)

    > [!NOTE]
    > <span data-ttu-id="b57fb-245">hello filen återställs nu.</span><span class="sxs-lookup"><span data-stu-id="b57fb-245">hello file is now restored.</span></span> <span data-ttu-id="b57fb-246">Du kan uppdatera hello SharePoint site toocheck hello återställa filen.</span><span class="sxs-lookup"><span data-stu-id="b57fb-246">You can refresh hello SharePoint site toocheck hello restored file.</span></span>
    >
    >

## <a name="restore-a-sharepoint-database-from-azure-by-using-dpm"></a><span data-ttu-id="b57fb-247">Återställa en SharePoint-databas från Azure med hjälp av DPM</span><span class="sxs-lookup"><span data-stu-id="b57fb-247">Restore a SharePoint database from Azure by using DPM</span></span>
1. <span data-ttu-id="b57fb-248">toorecover en SharePoint-innehållsdatabas Bläddra igenom olika återställningspunkter (som visas tidigare) och välj hello återställningspunkt som du vill toorestore.</span><span class="sxs-lookup"><span data-stu-id="b57fb-248">toorecover a SharePoint content database, browse through various recovery points (as shown previously), and select hello recovery point that you want toorestore.</span></span>

    ![MABS SharePoint Protection8](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection9.png)
2. <span data-ttu-id="b57fb-250">Dubbelklicka på hello SharePoint punkt tooshow hello tillgängliga SharePoint-katalogen återställningsinformation.</span><span class="sxs-lookup"><span data-stu-id="b57fb-250">Double-click hello SharePoint recovery point tooshow hello available SharePoint catalog information.</span></span>

   > [!NOTE]
   > <span data-ttu-id="b57fb-251">Ingen kataloginformation (metadata) är tillgängligt på MABS eftersom hello SharePoint-servergruppen skyddas för långsiktig kvarhållning i Azure.</span><span class="sxs-lookup"><span data-stu-id="b57fb-251">Because hello SharePoint farm is protected for long-term retention in Azure, no catalog information (metadata) is available on MABS.</span></span> <span data-ttu-id="b57fb-252">Därför när en tidpunkt i SharePoint-innehållsdatabas måste toobe återställas, måste toocatalog hello SharePoint-gruppen igen.</span><span class="sxs-lookup"><span data-stu-id="b57fb-252">As a result, whenever a point-in-time SharePoint content database needs toobe recovered, you need toocatalog hello SharePoint farm again.</span></span>
   >
   >
3. <span data-ttu-id="b57fb-253">Klicka på **omkatalogiseringen**.</span><span class="sxs-lookup"><span data-stu-id="b57fb-253">Click **Re-catalog**.</span></span>

    ![MABS SharePoint Protection10](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection12.png)

    <span data-ttu-id="b57fb-255">Hej **moln katalogisera** status öppnas.</span><span class="sxs-lookup"><span data-stu-id="b57fb-255">hello **Cloud Recatalog** status window opens.</span></span>

    ![MABS SharePoint Protection11](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection13.png)

    <span data-ttu-id="b57fb-257">När omkatalogiseras är klar, hello status ändras för*lyckade*.</span><span class="sxs-lookup"><span data-stu-id="b57fb-257">After cataloging is finished, hello status changes too*Success*.</span></span> <span data-ttu-id="b57fb-258">Klicka på **Stäng**.</span><span class="sxs-lookup"><span data-stu-id="b57fb-258">Click **Close**.</span></span>

    ![MABS SharePoint Protection12](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection14.png)
4. <span data-ttu-id="b57fb-260">Klicka på hello SharePoint-objekt som visas i hello MABS **Recovery** fliken tooget hello innehållsdatabasen struktur.</span><span class="sxs-lookup"><span data-stu-id="b57fb-260">Click hello SharePoint object shown in hello MABS **Recovery** tab tooget hello content database structure.</span></span> <span data-ttu-id="b57fb-261">Högerklicka på hello objekt och klicka sedan på **återställa**.</span><span class="sxs-lookup"><span data-stu-id="b57fb-261">Right-click hello item, and then click **Recover**.</span></span>

    ![MABS SharePoint Protection13](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection15.png)
5. <span data-ttu-id="b57fb-263">Nu följer hello [recovery stegen ovan](#restore-a-sharepoint-item-from-disk-using-dpm) toorecover en SharePoint-innehållsdatabas från disken.</span><span class="sxs-lookup"><span data-stu-id="b57fb-263">At this point, follow hello [recovery steps earlier in this article](#restore-a-sharepoint-item-from-disk-using-dpm) toorecover a SharePoint content database from disk.</span></span>

## <a name="faqs"></a><span data-ttu-id="b57fb-264">Vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="b57fb-264">FAQs</span></span>
<span data-ttu-id="b57fb-265">F: kan jag återställa en SharePoint objektet toohello ursprungliga plats om SharePoint konfigureras med hjälp av SQL AlwaysOn (med skydd på disk)?</span><span class="sxs-lookup"><span data-stu-id="b57fb-265">Q: Can I recover a SharePoint item toohello original location if SharePoint is configured by using SQL AlwaysOn (with protection on disk)?</span></span><br>
<span data-ttu-id="b57fb-266">S: Ja kan hello-objekt vara återställda toohello ursprungliga SharePoint-webbplatsen.</span><span class="sxs-lookup"><span data-stu-id="b57fb-266">A: Yes, hello item can be recovered toohello original SharePoint site.</span></span>

<span data-ttu-id="b57fb-267">F: kan jag återställa en SharePoint-databasen toohello ursprungliga plats om SharePoint konfigureras med hjälp av SQL AlwaysOn?</span><span class="sxs-lookup"><span data-stu-id="b57fb-267">Q: Can I recover a SharePoint database toohello original location if SharePoint is configured by using SQL AlwaysOn?</span></span><br>
<span data-ttu-id="b57fb-268">S: eftersom SharePoint-databaserna har konfigurerats i SQL AlwaysOn, kan de inte ändras om inte hello tillgänglighetsgruppen tas bort.</span><span class="sxs-lookup"><span data-stu-id="b57fb-268">A: Because SharePoint databases are configured in SQL AlwaysOn, they cannot be modified unless hello availability group is removed.</span></span> <span data-ttu-id="b57fb-269">Därför kan inte MABS återställa en databas toohello ursprungliga plats.</span><span class="sxs-lookup"><span data-stu-id="b57fb-269">As a result, MABS cannot restore a database toohello original location.</span></span> <span data-ttu-id="b57fb-270">Du kan återställa en SQL Server-databasen tooanother SQL Server-instans.</span><span class="sxs-lookup"><span data-stu-id="b57fb-270">You can recover a SQL Server database tooanother SQL Server instance.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b57fb-271">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b57fb-271">Next steps</span></span>
* <span data-ttu-id="b57fb-272">Mer information om MABS skydd av SharePoint - Se [Video serie - DPM-skydd av SharePoint](http://channel9.msdn.com/Series/Azure-Backup/Microsoft-SCDPM-Protection-of-SharePoint-1-of-2-How-to-create-a-SharePoint-Protection-Group)</span><span class="sxs-lookup"><span data-stu-id="b57fb-272">Learn more about MABS Protection of SharePoint - see [Video Series - DPM Protection of SharePoint](http://channel9.msdn.com/Series/Azure-Backup/Microsoft-SCDPM-Protection-of-SharePoint-1-of-2-How-to-create-a-SharePoint-Protection-Group)</span></span>
