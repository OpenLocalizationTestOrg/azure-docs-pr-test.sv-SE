---
title: "Använd Azure Backup server att säkerhetskopiera en SharePoint-grupp till Azure | Microsoft Docs"
description: "Använda Azure Backup Server för att säkerhetskopiera och återställa SharePoint-data. Den här artikeln innehåller information för att konfigurera SharePoint-servergruppen så att önskade data kan lagras i Azure. Du kan återställa skyddade SharePoint-data från disken eller från Azure."
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
ms.openlocfilehash: 3ed000affd326eb1bd7c99773ec021ad6e03cc3b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="back-up-a-sharepoint-farm-to-azure"></a><span data-ttu-id="45acf-105">Säkerhetskopiera en SharePoint-servergrupp till Azure</span><span class="sxs-lookup"><span data-stu-id="45acf-105">Back up a SharePoint farm to Azure</span></span>
<span data-ttu-id="45acf-106">Du säkerhetskopiera en SharePoint-grupp till Microsoft Azure med hjälp av Microsoft Azure Backup Server (MABS) på samma sätt som du säkerhetskopiera andra datakällor.</span><span class="sxs-lookup"><span data-stu-id="45acf-106">You back up a SharePoint farm to Microsoft Azure by using Microsoft Azure Backup Server (MABS) in much the same way that you back up other data sources.</span></span> <span data-ttu-id="45acf-107">Azure-säkerhetskopiering ger flexibilitet i schemat för säkerhetskopiering att skapa varje dag, varje vecka, månad eller årlig säkerhetskopiering pekar och ger alternativ för kvarhållning av princip för säkerhetskopiering punkter.</span><span class="sxs-lookup"><span data-stu-id="45acf-107">Azure Backup provides flexibility in the backup schedule to create daily, weekly, monthly, or yearly backup points and gives you retention policy options for various backup points.</span></span> <span data-ttu-id="45acf-108">Det ger också möjlighet att lagra lokal diskkopior för snabb återställningstiden mål (RTO) och för att lagra kopior till Azure för ekonomiska och långsiktig kvarhållning.</span><span class="sxs-lookup"><span data-stu-id="45acf-108">It also provides the capability to store local disk copies for quick recovery-time objectives (RTO) and to store copies to Azure for economical, long-term retention.</span></span>

## <a name="sharepoint-supported-versions-and-related-protection-scenarios"></a><span data-ttu-id="45acf-109">SharePoint-versioner som stöds och relaterade skyddsscenarier</span><span class="sxs-lookup"><span data-stu-id="45acf-109">SharePoint supported versions and related protection scenarios</span></span>
<span data-ttu-id="45acf-110">Azure Backup för DPM har stöd för följande scenarier:</span><span class="sxs-lookup"><span data-stu-id="45acf-110">Azure Backup for DPM supports the following scenarios:</span></span>

| <span data-ttu-id="45acf-111">Arbetsbelastning</span><span class="sxs-lookup"><span data-stu-id="45acf-111">Workload</span></span> | <span data-ttu-id="45acf-112">Version</span><span class="sxs-lookup"><span data-stu-id="45acf-112">Version</span></span> | <span data-ttu-id="45acf-113">Distribution av SharePoint</span><span class="sxs-lookup"><span data-stu-id="45acf-113">SharePoint deployment</span></span> | <span data-ttu-id="45acf-114">Skydd och återställning</span><span class="sxs-lookup"><span data-stu-id="45acf-114">Protection and recovery</span></span> |
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="45acf-115">SharePoint</span><span class="sxs-lookup"><span data-stu-id="45acf-115">SharePoint</span></span> |<span data-ttu-id="45acf-116">SharePoint 2013, SharePoint 2010 SharePoint 2007 SharePoint 3.0</span><span class="sxs-lookup"><span data-stu-id="45acf-116">SharePoint 2013, SharePoint 2010, SharePoint 2007, SharePoint 3.0</span></span> |<span data-ttu-id="45acf-117">SharePoint distribueras som en fysisk server eller VMware-Hyper-V virtuell dator</span><span class="sxs-lookup"><span data-stu-id="45acf-117">SharePoint deployed as a physical server or Hyper-V/VMware virtual machine</span></span> <br> -------------- <br> <span data-ttu-id="45acf-118">SQL AlwaysOn</span><span class="sxs-lookup"><span data-stu-id="45acf-118">SQL AlwaysOn</span></span> | <span data-ttu-id="45acf-119">Skydda SharePoint-servergruppen återställningsalternativ: återställningsgruppen, databas och fil- eller listobjekt från diskåterställningspunkter.</span><span class="sxs-lookup"><span data-stu-id="45acf-119">Protect SharePoint Farm recovery options: Recovery farm, database, and file or list item from disk recovery points.</span></span>  <span data-ttu-id="45acf-120">Servergruppen och databasen återställning från återställningspunkter i Azure.</span><span class="sxs-lookup"><span data-stu-id="45acf-120">Farm and database recovery from Azure recovery points.</span></span> |

## <a name="before-you-start"></a><span data-ttu-id="45acf-121">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="45acf-121">Before you start</span></span>
<span data-ttu-id="45acf-122">Det finns några saker du måste bekräfta innan du säkerhetskopierar en SharePoint-grupp till Azure.</span><span class="sxs-lookup"><span data-stu-id="45acf-122">There are a few things you need to confirm before you back up a SharePoint farm to Azure.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="45acf-123">Krav</span><span class="sxs-lookup"><span data-stu-id="45acf-123">Prerequisites</span></span>
<span data-ttu-id="45acf-124">Innan du fortsätter, kontrollera att du har [installerad och förberedda Azure Backup Server](backup-azure-microsoft-azure-backup.md) att skydda arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="45acf-124">Before you proceed, make sure that you have [installed and prepared the Azure Backup Server](backup-azure-microsoft-azure-backup.md) to protect workloads.</span></span>

### <a name="protection-agent"></a><span data-ttu-id="45acf-125">Skyddsagent</span><span class="sxs-lookup"><span data-stu-id="45acf-125">Protection agent</span></span>
<span data-ttu-id="45acf-126">Skyddsagenten måste installeras på den server som kör SharePoint, servrar som kör SQL Server och alla andra servrar som ingår i SharePoint-servergruppen.</span><span class="sxs-lookup"><span data-stu-id="45acf-126">The Protection agent must be installed on the server that's running SharePoint, the servers that are running SQL Server, and all other servers that are part of the SharePoint farm.</span></span> <span data-ttu-id="45acf-127">Mer information om hur du ställer in protection agent finns [installationsprogrammet Protection Agent](https://technet.microsoft.com/library/hh758034\(v=sc.12\).aspx).</span><span class="sxs-lookup"><span data-stu-id="45acf-127">For more information about how to set up the protection agent, see [Setup Protection Agent](https://technet.microsoft.com/library/hh758034\(v=sc.12\).aspx).</span></span>  <span data-ttu-id="45acf-128">Det enda undantaget är att du installerar agenten endast på en enda webbserver för klientdel (WFE).</span><span class="sxs-lookup"><span data-stu-id="45acf-128">The one exception is that you install the agent only on a single web front end (WFE) server.</span></span> <span data-ttu-id="45acf-129">DPM måste agenten på en WFE-server bara ska fungera som startpunkt för skydd.</span><span class="sxs-lookup"><span data-stu-id="45acf-129">DPM needs the agent on one WFE server only to serve as the entry point for protection.</span></span>

### <a name="sharepoint-farm"></a><span data-ttu-id="45acf-130">SharePoint-grupp</span><span class="sxs-lookup"><span data-stu-id="45acf-130">SharePoint farm</span></span>
<span data-ttu-id="45acf-131">För per 10 miljoner objekt i servergruppen måste det finnas minst 2 GB utrymme på volymen där MABS mappen finns.</span><span class="sxs-lookup"><span data-stu-id="45acf-131">For every 10 million items in the farm, there must be at least 2 GB of space on the volume where the MABS folder is located.</span></span> <span data-ttu-id="45acf-132">Den här utrymme som krävs för kataloggenerering.</span><span class="sxs-lookup"><span data-stu-id="45acf-132">This space is required for catalog generation.</span></span> <span data-ttu-id="45acf-133">För MABS återställa specifika objekt (webbplatssamlingar, platser, listor, dokumentbibliotek, mappar, enskilda dokument och listobjekt) skapas kataloggenerering en lista över URL: er som ingår i varje innehållsdatabas.</span><span class="sxs-lookup"><span data-stu-id="45acf-133">For MABS to recover specific items (site collections, sites, lists, document libraries, folders, individual documents, and list items), catalog generation creates a list of the URLs that are contained within each content database.</span></span> <span data-ttu-id="45acf-134">Du kan visa listan över URL: er i fönstret återställningsbara objekt i den **Recovery** aktivitetsområdet MABS-administratörskonsolen.</span><span class="sxs-lookup"><span data-stu-id="45acf-134">You can view the list of URLs in the recoverable item pane in the **Recovery** task area of MABS Administrator Console.</span></span>

### <a name="sql-server"></a><span data-ttu-id="45acf-135">SQL Server</span><span class="sxs-lookup"><span data-stu-id="45acf-135">SQL Server</span></span>
<span data-ttu-id="45acf-136">MABS körs som LocalSystem-kontot.</span><span class="sxs-lookup"><span data-stu-id="45acf-136">MABS runs as a LocalSystem account.</span></span> <span data-ttu-id="45acf-137">Om du vill säkerhetskopiera SQL Server-databaser, måste MABS sysadmin-behörigheter på det kontot för den server som kör SQL Server.</span><span class="sxs-lookup"><span data-stu-id="45acf-137">To back up SQL Server databases, MABS needs sysadmin privileges on that account for the server that's running SQL Server.</span></span> <span data-ttu-id="45acf-138">Ange NT AUTHORITY\SYSTEM till *sysadmin* på den server som kör SQL Server innan du säkerhetskopiera den.</span><span class="sxs-lookup"><span data-stu-id="45acf-138">Set NT AUTHORITY\SYSTEM to *sysadmin* on the server that's running SQL Server before you back it up.</span></span>

<span data-ttu-id="45acf-139">Om SharePoint-servergruppen har SQL Server-databaser som är konfigurerade med SQL Server-alias, installerar du SQL Server-klientkomponenterna på klientwebbservern som MABS ska skydda.</span><span class="sxs-lookup"><span data-stu-id="45acf-139">If the SharePoint farm has SQL Server databases that are configured with SQL Server aliases, install the SQL Server client components on the front-end Web server that MABS will protect.</span></span>

### <a name="sharepoint-server"></a><span data-ttu-id="45acf-140">SharePoint Server</span><span class="sxs-lookup"><span data-stu-id="45acf-140">SharePoint Server</span></span>
<span data-ttu-id="45acf-141">Medan prestanda beror på många faktorer, till exempel storleken på SharePoint-servergruppen, som en allmän vägledning kan en MABS skydda en SharePoint-grupp med 25 TB.</span><span class="sxs-lookup"><span data-stu-id="45acf-141">While performance depends on many factors such as size of SharePoint farm, as general guidance one MABS can protect a 25 TB SharePoint farm.</span></span>

### <a name="whats-not-supported"></a><span data-ttu-id="45acf-142">Vad som inte stöds</span><span class="sxs-lookup"><span data-stu-id="45acf-142">What's not supported</span></span>
* <span data-ttu-id="45acf-143">MABS som skyddar en SharePoint-servergrupp skyddar inte sökindexen eller databaser för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="45acf-143">MABS that protects a SharePoint farm does not protect search indexes or application service databases.</span></span> <span data-ttu-id="45acf-144">Behöver du konfigurera skydd för dessa databaser separat.</span><span class="sxs-lookup"><span data-stu-id="45acf-144">You will need to configure the protection of these databases separately.</span></span>
* <span data-ttu-id="45acf-145">MABS ger inte någon säkerhetskopia av SharePoint SQL Server-databaser som finns på filresurser för skalbar filserver (SOFS).</span><span class="sxs-lookup"><span data-stu-id="45acf-145">MABS does not provide backup of SharePoint SQL Server databases that are hosted on scale-out file server (SOFS) shares.</span></span>

## <a name="configure-sharepoint-protection"></a><span data-ttu-id="45acf-146">Konfigurera SharePoint-skydd</span><span class="sxs-lookup"><span data-stu-id="45acf-146">Configure SharePoint protection</span></span>
<span data-ttu-id="45acf-147">Innan du kan använda MABS för att skydda SharePoint, måste du konfigurera tjänsten SharePoint VSS Writer (WSS Writer-tjänsten) med hjälp av **ConfigureSharePoint.exe**.</span><span class="sxs-lookup"><span data-stu-id="45acf-147">Before you can use MABS to protect SharePoint, you must configure the SharePoint VSS Writer service (WSS Writer service) by using **ConfigureSharePoint.exe**.</span></span>

<span data-ttu-id="45acf-148">Du kan hitta **ConfigureSharePoint.exe** i mappen [MABS installationssökväg] \bin på klientwebbservern.</span><span class="sxs-lookup"><span data-stu-id="45acf-148">You can find **ConfigureSharePoint.exe** in the [MABS Installation Path]\bin folder on the front-end web server.</span></span> <span data-ttu-id="45acf-149">Det här verktyget förser skyddsagenten med autentiseringsuppgifter för SharePoint-servergruppen.</span><span class="sxs-lookup"><span data-stu-id="45acf-149">This tool provides the protection agent with the credentials for the SharePoint farm.</span></span> <span data-ttu-id="45acf-150">Du kan köra den på en enskild WFE-server.</span><span class="sxs-lookup"><span data-stu-id="45acf-150">You run it on a single WFE server.</span></span> <span data-ttu-id="45acf-151">Om du har flera WFE-servrar kan du bara välja en när du konfigurerar en skyddsgrupp.</span><span class="sxs-lookup"><span data-stu-id="45acf-151">If you have multiple WFE servers, select just one when you configure a protection group.</span></span>

### <a name="to-configure-the-sharepoint-vss-writer-service"></a><span data-ttu-id="45acf-152">Att konfigurera tjänsten SharePoint VSS Writer</span><span class="sxs-lookup"><span data-stu-id="45acf-152">To configure the SharePoint VSS Writer service</span></span>
1. <span data-ttu-id="45acf-153">Gå till [MABS installationsplats] \bin\ på WFE-servern i en kommandotolk</span><span class="sxs-lookup"><span data-stu-id="45acf-153">On the WFE server, at a command prompt, go to [MABS installation location]\bin\\</span></span>
2. <span data-ttu-id="45acf-154">Ange ConfigureSharePoint - EnableSharePointProtection.</span><span class="sxs-lookup"><span data-stu-id="45acf-154">Enter ConfigureSharePoint -EnableSharePointProtection.</span></span>
3. <span data-ttu-id="45acf-155">Ange administratörsbehörigheterna för servergruppen.</span><span class="sxs-lookup"><span data-stu-id="45acf-155">Enter the farm administrator credentials.</span></span> <span data-ttu-id="45acf-156">Det här kontot ska vara medlem i den lokala administratörsgruppen på WFE-servern.</span><span class="sxs-lookup"><span data-stu-id="45acf-156">This account should be a member of the local Administrator group on the WFE server.</span></span> <span data-ttu-id="45acf-157">Om servergruppsadministratören är inte en lokal administratör bevilja följande behörigheter på WFE-servern:</span><span class="sxs-lookup"><span data-stu-id="45acf-157">If the farm administrator isn’t a local admin grant the following permissions on the WFE server:</span></span>
   * <span data-ttu-id="45acf-158">Tilldela WSS_Admin_WPG-gruppen fullständig behörighet till DPM-mappen (% Program Files%\Microsoft Azure Backup\DPM).</span><span class="sxs-lookup"><span data-stu-id="45acf-158">Grant the WSS_Admin_WPG group full control to the DPM folder (%Program Files%\Microsoft Azure Backup\DPM).</span></span>
   * <span data-ttu-id="45acf-159">Bevilja läsbehörighet för WSS_Admin_WPG-gruppen till DPM-registernyckeln (HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft Data Protection Manager).</span><span class="sxs-lookup"><span data-stu-id="45acf-159">Grant the WSS_Admin_WPG group read access to the DPM Registry key (HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft Data Protection Manager).</span></span>

> [!NOTE]
> <span data-ttu-id="45acf-160">Du behöver köra ConfigureSharePoint.exe när en ändring i administratörsbehörigheterna för SharePoint-servergruppen.</span><span class="sxs-lookup"><span data-stu-id="45acf-160">You’ll need to rerun ConfigureSharePoint.exe whenever there’s a change in the SharePoint farm administrator credentials.</span></span>
>
>

## <a name="back-up-a-sharepoint-farm-by-using-mabs"></a><span data-ttu-id="45acf-161">Säkerhetskopiera en SharePoint-servergrupp med hjälp av MABS</span><span class="sxs-lookup"><span data-stu-id="45acf-161">Back up a SharePoint farm by using MABS</span></span>
<span data-ttu-id="45acf-162">När du har konfigurerat MABS och SharePoint-servergruppen som tidigare förklarats, skyddas SharePoint av MABS.</span><span class="sxs-lookup"><span data-stu-id="45acf-162">After you have configured MABS and the SharePoint farm as explained previously, SharePoint can be protected by MABS.</span></span>

### <a name="to-protect-a-sharepoint-farm"></a><span data-ttu-id="45acf-163">Skydda en SharePoint-grupp</span><span class="sxs-lookup"><span data-stu-id="45acf-163">To protect a SharePoint farm</span></span>
1. <span data-ttu-id="45acf-164">Från den **skydd** fliken MABS-administratörskonsolen klickar du på **ny**.</span><span class="sxs-lookup"><span data-stu-id="45acf-164">From the **Protection** tab of the MABS Administrator Console, click **New**.</span></span>
    <span data-ttu-id="45acf-165">![Skyddsfliken](./media/backup-azure-backup-sharepoint/dpm-new-protection-tab.png)</span><span class="sxs-lookup"><span data-stu-id="45acf-165">![New Protection Tab](./media/backup-azure-backup-sharepoint/dpm-new-protection-tab.png)</span></span>
2. <span data-ttu-id="45acf-166">På den **Välj typ av Skyddsgrupp** sida av den **Skapa ny Skyddsgrupp** guiden, Välj **servrar**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="45acf-166">On the **Select Protection Group Type** page of the **Create New Protection Group** wizard, select **Servers**, and then click **Next**.</span></span>

    ![Välj Skyddsgruppen typ](./media/backup-azure-backup-sharepoint/select-protection-group-type.png)
3. <span data-ttu-id="45acf-168">På den **Välj gruppmedlemmar** , väljer du kryssrutan för SharePoint-server som du vill skydda och klickar på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="45acf-168">On the **Select Group Members** screen, select the check box for the SharePoint server you want to protect and click **Next**.</span></span>

    ![Välj gruppmedlemmar](./media/backup-azure-backup-sharepoint/select-group-members2.png)

   > [!NOTE]
   > <span data-ttu-id="45acf-170">Du kan se servern i guiden med skyddsagenten är installerad.</span><span class="sxs-lookup"><span data-stu-id="45acf-170">With the protection agent installed, you can see the server in the wizard.</span></span> <span data-ttu-id="45acf-171">MABS visar även dess struktur.</span><span class="sxs-lookup"><span data-stu-id="45acf-171">MABS also shows its structure.</span></span> <span data-ttu-id="45acf-172">Eftersom du körde ConfigureSharePoint.exe MABS kommunicerar med tjänsten SharePoint VSS Writer och dess motsvarande SQL Server-databaser och identifierar strukturen för SharePoint-servergruppen, associerade innehållsdatabaserna och motsvarande objekt.</span><span class="sxs-lookup"><span data-stu-id="45acf-172">Because you ran ConfigureSharePoint.exe, MABS communicates with the SharePoint VSS Writer service and its corresponding SQL Server databases and recognizes the SharePoint farm structure, the associated content databases, and any corresponding items.</span></span>
   >
   >
4. <span data-ttu-id="45acf-173">På den **Välj Dataskyddsmetod** anger du namnet på den **Skyddsgrupp**, och välj *skyddsmetod*.</span><span class="sxs-lookup"><span data-stu-id="45acf-173">On the **Select Data Protection Method** page, enter the name of the **Protection Group**, and select your preferred *protection methods*.</span></span> <span data-ttu-id="45acf-174">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="45acf-174">Click **Next**.</span></span>

    ![Välj dataskyddsmetod](./media/backup-azure-backup-sharepoint/select-data-protection-method1.png)

   > [!NOTE]
   > <span data-ttu-id="45acf-176">Skyddsmetod disk hjälper till att uppfylla kort återställningstiden mål.</span><span class="sxs-lookup"><span data-stu-id="45acf-176">The disk protection method helps to meet short recovery-time objectives.</span></span>
   >
   >
5. <span data-ttu-id="45acf-177">På den **ange kortvariga mål** sidan, Välj **Kvarhållningsintervall** och identifiera när du vill att säkerhetskopieringen ska göras.</span><span class="sxs-lookup"><span data-stu-id="45acf-177">On the **Specify Short-Term Goals** page, select your preferred **Retention range** and identify when you want backups to occur.</span></span>

    ![Ange kortsiktiga mål](./media/backup-azure-backup-sharepoint/specify-short-term-goals2.png)

   > [!NOTE]
   > <span data-ttu-id="45acf-179">Eftersom återställningen krävs oftast för data som är mindre än fem dagar gammal, vi valt ett Kvarhållningsintervall på fem dagar på disken och kontrollerat att säkerhetskopieringen sker under icke-produktionstider för det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="45acf-179">Because recovery is most often required for data that's less than five days old, we selected a retention range of five days on disk and ensured that the backup happens during non-production hours, for this example.</span></span>
   >
   >
6. <span data-ttu-id="45acf-180">Granska lagringspoolen diskutrymme som allokerats för skyddsgruppen och sedan **nästa**.</span><span class="sxs-lookup"><span data-stu-id="45acf-180">Review the storage pool disk space allocated for the protection group, and click then **Next**.</span></span>
7. <span data-ttu-id="45acf-181">För varje skyddsgrupp allokerar MABS diskutrymme för att lagra och hantera repliker.</span><span class="sxs-lookup"><span data-stu-id="45acf-181">For every protection group, MABS allocates disk space to store and manage replicas.</span></span> <span data-ttu-id="45acf-182">MABS måste nu skapa en kopia av valda data.</span><span class="sxs-lookup"><span data-stu-id="45acf-182">At this point, MABS must create a copy of the selected data.</span></span> <span data-ttu-id="45acf-183">Välj hur och när du vill använda den replik som har skapats och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="45acf-183">Select how and when you want the replica created, and then click **Next**.</span></span>

    ![Välj metod för skapande av replik](./media/backup-azure-backup-sharepoint/choose-replica-creation-method.png)

   > [!NOTE]
   > <span data-ttu-id="45acf-185">Om du vill säkerställa att nätverkstrafiken genomförs inte och välj en tid utanför produktionstider.</span><span class="sxs-lookup"><span data-stu-id="45acf-185">To make sure that network traffic is not effected, select a time outside production hours.</span></span>
   >
   >
8. <span data-ttu-id="45acf-186">MABS garanterar dataintegriteten genom att utföra en konsekvenskontroll på repliken.</span><span class="sxs-lookup"><span data-stu-id="45acf-186">MABS ensures data integrity by performing consistency checks on the replica.</span></span> <span data-ttu-id="45acf-187">Det finns två tillgängliga alternativ.</span><span class="sxs-lookup"><span data-stu-id="45acf-187">There are two available options.</span></span> <span data-ttu-id="45acf-188">Du kan definiera ett schema för att köra konsekvenskontroll eller DPM kan köra konsekvenskontroll på repliken automatiskt varje gång den blir inkonsekvent.</span><span class="sxs-lookup"><span data-stu-id="45acf-188">You can define a schedule to run consistency checks, or DPM can run consistency checks automatically on the replica whenever it becomes inconsistent.</span></span> <span data-ttu-id="45acf-189">Välj önskade alternativ och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="45acf-189">Select your preferred option, and then click **Next**.</span></span>

    ![Konsekvenskontroll](./media/backup-azure-backup-sharepoint/consistency-check.png)
9. <span data-ttu-id="45acf-191">På den **ange Onlineskyddsdata** markerar du den SharePoint-grupp som du vill skydda och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="45acf-191">On the **Specify Online Protection Data** page, select the SharePoint farm that you want to protect, and then click **Next**.</span></span>

    ![DPM SharePoint Protection1](./media/backup-azure-backup-sharepoint/select-online-protection1.png)
10. <span data-ttu-id="45acf-193">På den **Ange schema för Online säkerhetskopiering** , väljer önskat schema, och klickar sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="45acf-193">On the **Specify Online Backup Schedule** page, select your preferred schedule, and then click **Next**.</span></span>

    ![Online_backup_schedule](./media/backup-azure-backup-sharepoint/specify-online-backup-schedule.png)

    > [!NOTE]
    > <span data-ttu-id="45acf-195">MABS ger högst två daglig säkerhetskopiering till Azure från den sedan tillgänglig säkerhetskopieringspunkt för senaste disk.</span><span class="sxs-lookup"><span data-stu-id="45acf-195">MABS provides a maximum of two daily backups to Azure from the then available latest disk backup point.</span></span> <span data-ttu-id="45acf-196">Azure Backup kan också styra hur mycket av WAN-bandbredd som kan användas för säkerhetskopieringar i belastning och låg belastning via [Azure Backup nätverksbegränsning](https://azure.microsoft.com/documentation/articles/backup-configure-vault/#enable-network-throttling).</span><span class="sxs-lookup"><span data-stu-id="45acf-196">Azure Backup can also control the amount of WAN bandwidth that can be used for backups in peak and off-peak hours by using [Azure Backup Network Throttling](https://azure.microsoft.com/documentation/articles/backup-configure-vault/#enable-network-throttling).</span></span>
    >
    >
11. <span data-ttu-id="45acf-197">Beroende på schemat för säkerhetskopiering som du valde på den **ange bevarandeprincip** väljer bevarandeprincipen för dagliga, veckovisa, månatliga och årliga säkerhetskopiering punkter.</span><span class="sxs-lookup"><span data-stu-id="45acf-197">Depending on the backup schedule that you selected, on the **Specify Online Retention Policy** page, select the retention policy for daily, weekly, monthly, and yearly backup points.</span></span>

    ![Online_retention_policy](./media/backup-azure-backup-sharepoint/specify-online-retention.png)

    > [!NOTE]
    > <span data-ttu-id="45acf-199">MABS använder ett farfar-pappa-son kvarhållning-schema som du kan välja en annan bevarandeprincip för olika tidpunkter som säkerhetskopieringen.</span><span class="sxs-lookup"><span data-stu-id="45acf-199">MABS uses a grandfather-father-son retention scheme in which a different retention policy can be chosen for different backup points.</span></span>
    >
    >
12. <span data-ttu-id="45acf-200">Liknande till disk, en referens för inledande punkt replik måste skapas i Azure.</span><span class="sxs-lookup"><span data-stu-id="45acf-200">Similar to disk, an initial reference point replica needs to be created in Azure.</span></span> <span data-ttu-id="45acf-201">Välj önskade alternativ att skapa en första säkerhetskopiering till Azure och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="45acf-201">Select your preferred option to create an initial backup copy to Azure, and then click **Next**.</span></span>

    ![Online_replica](./media/backup-azure-backup-sharepoint/online-replication.png)
13. <span data-ttu-id="45acf-203">Granska de angivna inställningarna på den **sammanfattning** , och klickar sedan på **Skapa grupp**.</span><span class="sxs-lookup"><span data-stu-id="45acf-203">Review your selected settings on the **Summary** page, and then click **Create Group**.</span></span> <span data-ttu-id="45acf-204">Ett meddelande visas när skyddsgruppen har skapats.</span><span class="sxs-lookup"><span data-stu-id="45acf-204">You will see a success message after the protection group has been created.</span></span>

    ![Sammanfattning](./media/backup-azure-backup-sharepoint/summary.png)

## <a name="restore-a-sharepoint-item-from-disk-by-using-mabs"></a><span data-ttu-id="45acf-206">Återställa SharePoint-objektet från disken med hjälp av MABS</span><span class="sxs-lookup"><span data-stu-id="45acf-206">Restore a SharePoint item from disk by using MABS</span></span>
<span data-ttu-id="45acf-207">I följande exempel visas den *återställa SharePoint-objektet* har tagits bort av misstag och behöver återställas.</span><span class="sxs-lookup"><span data-stu-id="45acf-207">In the following example, the *Recovering SharePoint item* has been accidentally deleted and needs to be recovered.</span></span>
<span data-ttu-id="45acf-208">![MABS SharePoint Protection4](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection5.png)</span><span class="sxs-lookup"><span data-stu-id="45acf-208">![MABS SharePoint Protection4](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection5.png)</span></span>

1. <span data-ttu-id="45acf-209">Öppna den **DPM-administratörskonsolen**.</span><span class="sxs-lookup"><span data-stu-id="45acf-209">Open the **DPM Administrator Console**.</span></span> <span data-ttu-id="45acf-210">Alla SharePoint-servergrupper som skyddas av DPM som visas i den **skydd** fliken.</span><span class="sxs-lookup"><span data-stu-id="45acf-210">All SharePoint farms that are protected by DPM are shown in the **Protection** tab.</span></span>

    ![MABS SharePoint Protection3](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection4.png)
2. <span data-ttu-id="45acf-212">Om du vill börja att återskapa objektet, Välj den **Recovery** fliken.</span><span class="sxs-lookup"><span data-stu-id="45acf-212">To begin to recover the item, select the **Recovery** tab.</span></span>

    ![MABS SharePoint Protection5](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection6.png)
3. <span data-ttu-id="45acf-214">Du kan söka SharePoint för *återställa SharePoint-objektet* peka intervall genom att använda en sökning med jokertecken-baserade inom en återställning.</span><span class="sxs-lookup"><span data-stu-id="45acf-214">You can search SharePoint for *Recovering SharePoint item* by using a wildcard-based search within a recovery point range.</span></span>

    ![MABS SharePoint Protection6](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection7.png)
4. <span data-ttu-id="45acf-216">Välj lämplig återställningspunkt i sökresultatet, högerklicka på objektet och välj sedan **återställa**.</span><span class="sxs-lookup"><span data-stu-id="45acf-216">Select the appropriate recovery point from the search results, right-click the item, and then select **Recover**.</span></span>
5. <span data-ttu-id="45acf-217">Du kan också bläddra igenom olika återställningspunkter och välja en databas eller objektet om du vill återställa.</span><span class="sxs-lookup"><span data-stu-id="45acf-217">You can also browse through various recovery points and select a database or item to recover.</span></span> <span data-ttu-id="45acf-218">Välj **datum > återställningstid**, och välj rätt **databasen > SharePoint-servergruppen > återställningspunkt > objektet**.</span><span class="sxs-lookup"><span data-stu-id="45acf-218">Select **Date > Recovery time**, and then select the correct **Database > SharePoint farm > Recovery point > Item**.</span></span>

    ![MABS SharePoint Protection7](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection8.png)
6. <span data-ttu-id="45acf-220">Högerklicka på objektet och välj sedan **återställa** att öppna den **Återställningsguiden**.</span><span class="sxs-lookup"><span data-stu-id="45acf-220">Right-click the item, and then select **Recover** to open the **Recovery Wizard**.</span></span> <span data-ttu-id="45acf-221">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="45acf-221">Click **Next**.</span></span>

    ![Granska val av återställning](./media/backup-azure-backup-sharepoint/review-recovery-selection.png)
7. <span data-ttu-id="45acf-223">Välj typ av återställning som du vill utföra och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="45acf-223">Select the type of recovery that you want to perform, and then click **Next**.</span></span>

    ![Typ av återställning](./media/backup-azure-backup-sharepoint/select-recovery-type.png)

   > [!NOTE]
   > <span data-ttu-id="45acf-225">Valet av **återställa till ursprungliga** i exempel återställer objekt till den ursprungliga SharePoint-webbplatsen.</span><span class="sxs-lookup"><span data-stu-id="45acf-225">The selection of **Recover to original** in the example recovers the item to the original SharePoint site.</span></span>
   >
   >
8. <span data-ttu-id="45acf-226">Välj den **återställningsprocessen** som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="45acf-226">Select the **Recovery Process** that you want to use.</span></span>

   * <span data-ttu-id="45acf-227">Välj **återställa utan en återställningsgrupp** om SharePoint-servergruppen inte har ändrats och är samma som den återställningspunkt som återställs.</span><span class="sxs-lookup"><span data-stu-id="45acf-227">Select **Recover without using a recovery farm** if the SharePoint farm has not changed and is the same as the recovery point that is being restored.</span></span>
   * <span data-ttu-id="45acf-228">Välj **återställa en återställningsgrupp** om SharePoint-servergruppen har ändrats sedan återställningspunkten skapades.</span><span class="sxs-lookup"><span data-stu-id="45acf-228">Select **Recover using a recovery farm** if the SharePoint farm has changed since the recovery point was created.</span></span>

     ![Återställningsprocessen](./media/backup-azure-backup-sharepoint/recovery-process.png)
9. <span data-ttu-id="45acf-230">Ange en SQL Server-instansen mellanlagringsplatsen för att återställa databasen tillfälligt och ger en fristående filresurs MABS och den server som kör SharePoint om du vill återställa objektet.</span><span class="sxs-lookup"><span data-stu-id="45acf-230">Provide a staging SQL Server instance location to recover the database temporarily, and provide a staging file share on MABS and the server that's running SharePoint to recover the item.</span></span>

    ![Mellanlagring Location1](./media/backup-azure-backup-sharepoint/staging-location1.png)

    <span data-ttu-id="45acf-232">MABS bifogar innehållsdatabasen som är värd för SharePoint-objektet till den tillfälliga SQL-serverinstansen.</span><span class="sxs-lookup"><span data-stu-id="45acf-232">MABS attaches the content database that is hosting the SharePoint item to the temporary SQL Server instance.</span></span> <span data-ttu-id="45acf-233">I innehållsdatabasen återställer objektet och placeras på mellanlagringsplatsen fil på MABS.</span><span class="sxs-lookup"><span data-stu-id="45acf-233">From the content database, it recovers the item and puts it on the staging file location on MABS.</span></span> <span data-ttu-id="45acf-234">Det återställda objektet som nu är på mellanlagringsplatsen måste exporteras till mellanlagringsplatsen på SharePoint-servergruppen.</span><span class="sxs-lookup"><span data-stu-id="45acf-234">The recovered item that's on the staging location now needs to be exported to the staging location on the SharePoint farm.</span></span>

    ![Mellanlagring Location2](./media/backup-azure-backup-sharepoint/staging-location2.png)
10. <span data-ttu-id="45acf-236">Välj **Ange återställningsalternativ**, och tillämpa säkerhetsinställningar till SharePoint-servergrupp eller Använd säkerhetsinställningar för återställningspunkten.</span><span class="sxs-lookup"><span data-stu-id="45acf-236">Select **Specify recovery options**, and apply security settings to the SharePoint farm or apply the security settings of the recovery point.</span></span> <span data-ttu-id="45acf-237">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="45acf-237">Click **Next**.</span></span>

    ![Återställningsalternativ](./media/backup-azure-backup-sharepoint/recovery-options.png)

    > [!NOTE]
    > <span data-ttu-id="45acf-239">Du kan välja att begränsa nätverkets bandbredd.</span><span class="sxs-lookup"><span data-stu-id="45acf-239">You can choose to throttle the network bandwidth usage.</span></span> <span data-ttu-id="45acf-240">Detta minskar inverkan på produktionsservern under produktionstider.</span><span class="sxs-lookup"><span data-stu-id="45acf-240">This minimizes impact to the production server during production hours.</span></span>
    >
    >
11. <span data-ttu-id="45acf-241">Granska sammanfattningen och klicka sedan på **återställa** att börja återställa filen.</span><span class="sxs-lookup"><span data-stu-id="45acf-241">Review the summary information, and then click **Recover** to begin recovery of the file.</span></span>

    ![Översikt över återställning](./media/backup-azure-backup-sharepoint/recovery-summary.png)
12. <span data-ttu-id="45acf-243">Nu välja de **övervakning** fliken i den **MABS administratörskonsolen** att visa den **Status** av återställningspunkter.</span><span class="sxs-lookup"><span data-stu-id="45acf-243">Now select the **Monitoring** tab in the **MABS Administrator Console** to view the **Status** of the recovery.</span></span>

    ![Status för återställning](./media/backup-azure-backup-sharepoint/recovery-monitoring.png)

    > [!NOTE]
    > <span data-ttu-id="45acf-245">Filen återställs nu.</span><span class="sxs-lookup"><span data-stu-id="45acf-245">The file is now restored.</span></span> <span data-ttu-id="45acf-246">Du kan uppdatera SharePoint-webbplatsen om du vill kontrollera den återställda filen.</span><span class="sxs-lookup"><span data-stu-id="45acf-246">You can refresh the SharePoint site to check the restored file.</span></span>
    >
    >

## <a name="restore-a-sharepoint-database-from-azure-by-using-dpm"></a><span data-ttu-id="45acf-247">Återställa en SharePoint-databas från Azure med hjälp av DPM</span><span class="sxs-lookup"><span data-stu-id="45acf-247">Restore a SharePoint database from Azure by using DPM</span></span>
1. <span data-ttu-id="45acf-248">Om du vill återställa en SharePoint-innehållsdatabas Bläddra igenom olika återställningspunkter (som visas tidigare) och välj den återställningspunkt som du vill återställa.</span><span class="sxs-lookup"><span data-stu-id="45acf-248">To recover a SharePoint content database, browse through various recovery points (as shown previously), and select the recovery point that you want to restore.</span></span>

    ![MABS SharePoint Protection8](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection9.png)
2. <span data-ttu-id="45acf-250">Dubbelklicka på SharePoint-återställningspunkt för att visa tillgängliga kataloginformationen för SharePoint.</span><span class="sxs-lookup"><span data-stu-id="45acf-250">Double-click the SharePoint recovery point to show the available SharePoint catalog information.</span></span>

   > [!NOTE]
   > <span data-ttu-id="45acf-251">Ingen kataloginformation (metadata) är tillgängligt på MABS eftersom SharePoint-servergruppen skyddas för långsiktig kvarhållning i Azure.</span><span class="sxs-lookup"><span data-stu-id="45acf-251">Because the SharePoint farm is protected for long-term retention in Azure, no catalog information (metadata) is available on MABS.</span></span> <span data-ttu-id="45acf-252">Därför när en tidpunkt i SharePoint-innehållsdatabas måste återställas, måste du katalogisera SharePoint-gruppen igen.</span><span class="sxs-lookup"><span data-stu-id="45acf-252">As a result, whenever a point-in-time SharePoint content database needs to be recovered, you need to catalog the SharePoint farm again.</span></span>
   >
   >
3. <span data-ttu-id="45acf-253">Klicka på **omkatalogiseringen**.</span><span class="sxs-lookup"><span data-stu-id="45acf-253">Click **Re-catalog**.</span></span>

    ![MABS SharePoint Protection10](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection12.png)

    <span data-ttu-id="45acf-255">Den **moln katalogisera** status öppnas.</span><span class="sxs-lookup"><span data-stu-id="45acf-255">The **Cloud Recatalog** status window opens.</span></span>

    ![MABS SharePoint Protection11](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection13.png)

    <span data-ttu-id="45acf-257">När omkatalogiseras har slutförts ändras statusen till *lyckade*.</span><span class="sxs-lookup"><span data-stu-id="45acf-257">After cataloging is finished, the status changes to *Success*.</span></span> <span data-ttu-id="45acf-258">Klicka på **Stäng**.</span><span class="sxs-lookup"><span data-stu-id="45acf-258">Click **Close**.</span></span>

    ![MABS SharePoint Protection12](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection14.png)
4. <span data-ttu-id="45acf-260">Klicka på SharePoint-objektet som visas i MABS **Recovery** fliken för att få innehållsdatabasen strukturen.</span><span class="sxs-lookup"><span data-stu-id="45acf-260">Click the SharePoint object shown in the MABS **Recovery** tab to get the content database structure.</span></span> <span data-ttu-id="45acf-261">Högerklicka på objektet och klicka sedan på **återställa**.</span><span class="sxs-lookup"><span data-stu-id="45acf-261">Right-click the item, and then click **Recover**.</span></span>

    ![MABS SharePoint Protection13](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection15.png)
5. <span data-ttu-id="45acf-263">Nu följer den [recovery stegen ovan](#restore-a-sharepoint-item-from-disk-using-dpm) att återställa en SharePoint-innehållsdatabas från disken.</span><span class="sxs-lookup"><span data-stu-id="45acf-263">At this point, follow the [recovery steps earlier in this article](#restore-a-sharepoint-item-from-disk-using-dpm) to recover a SharePoint content database from disk.</span></span>

## <a name="faqs"></a><span data-ttu-id="45acf-264">Vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="45acf-264">FAQs</span></span>
<span data-ttu-id="45acf-265">F: kan jag återställa SharePoint-objektet till den ursprungliga platsen om SharePoint konfigureras med hjälp av SQL AlwaysOn (med skydd på disk)?</span><span class="sxs-lookup"><span data-stu-id="45acf-265">Q: Can I recover a SharePoint item to the original location if SharePoint is configured by using SQL AlwaysOn (with protection on disk)?</span></span><br>
<span data-ttu-id="45acf-266">S: Ja kan objektet återställas till den ursprungliga SharePoint-webbplatsen.</span><span class="sxs-lookup"><span data-stu-id="45acf-266">A: Yes, the item can be recovered to the original SharePoint site.</span></span>

<span data-ttu-id="45acf-267">F: kan jag återställa SharePoint-databasen till den ursprungliga platsen om SharePoint konfigureras med hjälp av SQL AlwaysOn?</span><span class="sxs-lookup"><span data-stu-id="45acf-267">Q: Can I recover a SharePoint database to the original location if SharePoint is configured by using SQL AlwaysOn?</span></span><br>
<span data-ttu-id="45acf-268">S: eftersom SharePoint-databaserna har konfigurerats i SQL AlwaysOn, kan de inte ändras om inte tillgänglighetsgruppen tas bort.</span><span class="sxs-lookup"><span data-stu-id="45acf-268">A: Because SharePoint databases are configured in SQL AlwaysOn, they cannot be modified unless the availability group is removed.</span></span> <span data-ttu-id="45acf-269">Därför kan inte MABS återställa en databas till den ursprungliga platsen.</span><span class="sxs-lookup"><span data-stu-id="45acf-269">As a result, MABS cannot restore a database to the original location.</span></span> <span data-ttu-id="45acf-270">Du kan återställa en SQL Server-databas till en annan SQL Server-instans.</span><span class="sxs-lookup"><span data-stu-id="45acf-270">You can recover a SQL Server database to another SQL Server instance.</span></span>

## <a name="next-steps"></a><span data-ttu-id="45acf-271">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="45acf-271">Next steps</span></span>
* <span data-ttu-id="45acf-272">Mer information om MABS skydd av SharePoint - Se [Video serie - DPM-skydd av SharePoint](http://channel9.msdn.com/Series/Azure-Backup/Microsoft-SCDPM-Protection-of-SharePoint-1-of-2-How-to-create-a-SharePoint-Protection-Group)</span><span class="sxs-lookup"><span data-stu-id="45acf-272">Learn more about MABS Protection of SharePoint - see [Video Series - DPM Protection of SharePoint](http://channel9.msdn.com/Series/Azure-Backup/Microsoft-SCDPM-Protection-of-SharePoint-1-of-2-How-to-create-a-SharePoint-Protection-Group)</span></span>
