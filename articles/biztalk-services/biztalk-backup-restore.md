---
title: "Skapa och återställa en säkerhetskopia i BizTalk-tjänst | Microsoft Docs"
description: "BizTalk-tjänster omfattar säkerhetskopiering och återställning. Lär dig hur du skapar och återställer en säkerhetskopia och Bestäm vad säkerhetskopieras. MABS WABS"
services: biztalk-services
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: 59f91173-4683-48df-abd5-41262bfce6df
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: c55d1ab124441c42101b4ad60924a9ea28231408
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="biztalk-services-backup-and-restore"></a><span data-ttu-id="bcffd-105">BizTalk Services: Säkerhetskopiering och återställning</span><span class="sxs-lookup"><span data-stu-id="bcffd-105">BizTalk Services: Backup and Restore</span></span>

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

<span data-ttu-id="bcffd-106">Azure BizTalk-tjänster inkluderar funktioner för säkerhetskopiering och återställning.</span><span class="sxs-lookup"><span data-stu-id="bcffd-106">Azure BizTalk Services includes Backup and Restore capabilities.</span></span> <span data-ttu-id="bcffd-107">Det här avsnittet beskriver hur du säkerhetskopierar och återställer BizTalk-tjänster med hjälp av den klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="bcffd-107">This topic describes how to backup and restore BizTalk Services using the Azure classic portal.</span></span>

<span data-ttu-id="bcffd-108">Du kan också säkerhetskopiera BizTalk-tjänst med hjälp av den [BizTalk Services REST API](http://go.microsoft.com/fwlink/p/?LinkID=325584).</span><span class="sxs-lookup"><span data-stu-id="bcffd-108">You can also back up BizTalk Services using the [BizTalk Services REST API](http://go.microsoft.com/fwlink/p/?LinkID=325584).</span></span> 

> [!NOTE]
> <span data-ttu-id="bcffd-109">Hybridanslutningar säkerhetskopieras inte, oavsett vilken version.</span><span class="sxs-lookup"><span data-stu-id="bcffd-109">Hybrid Connections are NOT backed up, regardless of the Edition.</span></span> <span data-ttu-id="bcffd-110">Du måste återskapa din hybridanslutningar.</span><span class="sxs-lookup"><span data-stu-id="bcffd-110">You must recreate your hybrid connections.</span></span>


## <a name="before-you-begin"></a><span data-ttu-id="bcffd-111">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="bcffd-111">Before you Begin</span></span>
* <span data-ttu-id="bcffd-112">Säkerhetskopiering och återställning kanske inte tillgänglig för alla versioner.</span><span class="sxs-lookup"><span data-stu-id="bcffd-112">Backup and Restore may not be available for all editions.</span></span> <span data-ttu-id="bcffd-113">Se [BizTalk-tjänst: utgåvor diagram](biztalk-editions-feature-chart.md).</span><span class="sxs-lookup"><span data-stu-id="bcffd-113">See [BizTalk Services: Editions Chart](biztalk-editions-feature-chart.md).</span></span>
* <span data-ttu-id="bcffd-114">Med den klassiska Azure-portalen kan du skapa en säkerhetskopiering på begäran eller skapa en schemalagd säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="bcffd-114">Using the Azure classic portal, you can create an On Demand backup or create a scheduled backup.</span></span> 
* <span data-ttu-id="bcffd-115">Säkerhetskopiering innehåll kan återställas till samma BizTalk Service eller till en ny BizTalk Service.</span><span class="sxs-lookup"><span data-stu-id="bcffd-115">Backup content can be restored to the same BizTalk Service or to a new BizTalk Service.</span></span> <span data-ttu-id="bcffd-116">Om du vill återställa den BizTalk Service med samma namn, befintlig BizTalk Service måste tas bort och namnet måste vara tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="bcffd-116">To restore the BizTalk Service using the same name, the existing BizTalk Service must be deleted and the name must be available.</span></span> <span data-ttu-id="bcffd-117">Det kan ta längre tid än vill om samma namn ska vara tillgängliga när du har tagit bort en BizTalk Service.</span><span class="sxs-lookup"><span data-stu-id="bcffd-117">After you delete a BizTalk Service, it can take longer time than wanted for the same name to be available.</span></span> <span data-ttu-id="bcffd-118">Om du inte kan vänta på samma namn ska vara tillgängliga, återställer du sedan till ett nytt BizTalk-Service.</span><span class="sxs-lookup"><span data-stu-id="bcffd-118">If you cannot wait for the same name to be available, then restore to a new BizTalk Service.</span></span>
* <span data-ttu-id="bcffd-119">BizTalk-tjänst kan återställas till samma version eller en senare version.</span><span class="sxs-lookup"><span data-stu-id="bcffd-119">BizTalk Services can be restored to the same edition or a higher edition.</span></span> <span data-ttu-id="bcffd-120">Återställa BizTalk-tjänst till en lägre version stöds från när säkerhetskopian skapades, inte.</span><span class="sxs-lookup"><span data-stu-id="bcffd-120">Restoring BizTalk Services to a lower edition, from when the backup was taken, is not supported.</span></span>
  
    <span data-ttu-id="bcffd-121">Exempelvis kan du återställa en säkerhetskopia med Basic-versionen till Premium Edition.</span><span class="sxs-lookup"><span data-stu-id="bcffd-121">For example, a backup using the Basic Edition can be restored to the Premium Edition.</span></span> <span data-ttu-id="bcffd-122">En säkerhetskopiering med Premium Edition kan inte återställas till Standard Edition.</span><span class="sxs-lookup"><span data-stu-id="bcffd-122">A backup using the Premium Edition cannot be restored to the Standard Edition.</span></span>
* <span data-ttu-id="bcffd-123">EDI-kontrollen siffror säkerhetskopieras upprätthålla kontinuitet av.</span><span class="sxs-lookup"><span data-stu-id="bcffd-123">The EDI Control numbers are backed up to maintain continuity of the control numbers.</span></span> <span data-ttu-id="bcffd-124">Meddelanden som bearbetas efter den senaste säkerhetskopieringen, kan återställa säkerhetskopiering innehållet orsaka dubbla kontrollen siffror.</span><span class="sxs-lookup"><span data-stu-id="bcffd-124">If messages are processed after the last backup, restoring this backup content can cause duplicate control numbers.</span></span>
* <span data-ttu-id="bcffd-125">Om en batch har aktiva meddelanden, bearbetar batchen **innan** köra en säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="bcffd-125">If a batch has active messages, process the batch **before** running a backup.</span></span> <span data-ttu-id="bcffd-126">När du skapar en säkerhetskopia (som krävs eller är schemalagda) lagras aldrig meddelanden i batchar.</span><span class="sxs-lookup"><span data-stu-id="bcffd-126">When creating a backup (as needed or scheduled), messages in batches are never stored.</span></span> 
  
    <span data-ttu-id="bcffd-127">**Om en säkerhetskopia görs med aktiva meddelanden i en grupp, säkerhetskopieras inte dessa meddelanden och därför går förlorade.**</span><span class="sxs-lookup"><span data-stu-id="bcffd-127">**If a backup is taken with active messages in a batch, these messages are not backed up and are therefore lost.**</span></span>
* <span data-ttu-id="bcffd-128">Valfritt: I BizTalk-Services-portalen stoppa några hanteringsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="bcffd-128">Optional: In the BizTalk Services Portal, stop any management operations.</span></span>

## <a name="create-a-backup"></a><span data-ttu-id="bcffd-129">Skapa en säkerhetskopia</span><span class="sxs-lookup"><span data-stu-id="bcffd-129">Create a backup</span></span>
<span data-ttu-id="bcffd-130">En säkerhetskopia kan vidtas när som helst och är helt styrs av du.</span><span class="sxs-lookup"><span data-stu-id="bcffd-130">A backup can be taken at any time and is completely controlled by you.</span></span> <span data-ttu-id="bcffd-131">Det här avsnittet innehåller anvisningar för att skapa säkerhetskopior med den klassiska Azure-portalen, inklusive:</span><span class="sxs-lookup"><span data-stu-id="bcffd-131">This section lists the steps to create backups using the Azure classic portal, including:</span></span>

[<span data-ttu-id="bcffd-132">På begäran-säkerhetskopia</span><span class="sxs-lookup"><span data-stu-id="bcffd-132">On Demand backup</span></span>](#backupnow)

[<span data-ttu-id="bcffd-133">Schemalägga en säkerhetskopia</span><span class="sxs-lookup"><span data-stu-id="bcffd-133">Schedule a backup</span></span>](#backupschedule)

#### <span data-ttu-id="bcffd-134"><a name="backupnow"></a>På begäran-säkerhetskopia</span><span class="sxs-lookup"><span data-stu-id="bcffd-134"><a name="backupnow"></a>On Demand backup</span></span>
1. <span data-ttu-id="bcffd-135">I den klassiska Azure-portalen väljer **BizTalk-tjänst**, och välj sedan den BizTalk Service du vill säkerhetskopiera.</span><span class="sxs-lookup"><span data-stu-id="bcffd-135">In the Azure classic portal, select **BizTalk Services**, and then select the BizTalk Service you want to backup.</span></span>
2. <span data-ttu-id="bcffd-136">I den **instrumentpanelen** väljer **säkerhetskopiera** längst ned på sidan.</span><span class="sxs-lookup"><span data-stu-id="bcffd-136">In the **Dashboard** tab, select **Back up** at the bottom of the page.</span></span>
3. <span data-ttu-id="bcffd-137">Ange ett namn på säkerhetskopia.</span><span class="sxs-lookup"><span data-stu-id="bcffd-137">Enter a backup name.</span></span> <span data-ttu-id="bcffd-138">Ange till exempel *myBizTalkService*för*datum*.</span><span class="sxs-lookup"><span data-stu-id="bcffd-138">For example, enter *myBizTalkService*BU*Date*.</span></span>
4. <span data-ttu-id="bcffd-139">Väljer ett blob storage-konto och sedan på kryssmarkeringen för att starta säkerhetskopieringen.</span><span class="sxs-lookup"><span data-stu-id="bcffd-139">Choose a blob storage account and select the checkmark to start the backup.</span></span>

<span data-ttu-id="bcffd-140">När säkerhetskopieringen är klar skapas en behållare med det namn du anger i lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="bcffd-140">Once the backup completes, a container with the backup name you enter is created in the storage account.</span></span> <span data-ttu-id="bcffd-141">Den här behållaren innehåller BizTalk Service konfigurationen för säkerhetskopieringen.</span><span class="sxs-lookup"><span data-stu-id="bcffd-141">This container contains your BizTalk Service backup configuration.</span></span>

#### <span data-ttu-id="bcffd-142"><a name="backupschedule"></a>Schemalägga en säkerhetskopia</span><span class="sxs-lookup"><span data-stu-id="bcffd-142"><a name="backupschedule"></a>Schedule a backup</span></span>
1. <span data-ttu-id="bcffd-143">I den klassiska Azure-portalen väljer **BizTalk-tjänst**, Välj BizTalk Service-namnet som du vill schemalägga säkerhetskopieringen och välj sedan den **konfigurera** fliken.</span><span class="sxs-lookup"><span data-stu-id="bcffd-143">In the Azure classic portal, select **BizTalk Services**, select the BizTalk Service name you want to schedule the backup, and then select the **Configure** tab.</span></span>
2. <span data-ttu-id="bcffd-144">Ange den **säkerhetskopiera Status** till **automatisk**.</span><span class="sxs-lookup"><span data-stu-id="bcffd-144">Set the **Backup Status** to **Automatic**.</span></span> 
3. <span data-ttu-id="bcffd-145">Välj den **Lagringskonto** för att lagra säkerhetskopian, ange den **frekvens** skapa säkerhetskopior, och hur lång tid att behålla säkerhetskopieringar (**kvarhållning dagar**):</span><span class="sxs-lookup"><span data-stu-id="bcffd-145">Select the **Storage Account** to store the backup, enter the **Frequency** to create the backups, and how long to keep the backups (**Retention Days**):</span></span>
   
    ![][AutomaticBU]
   
    <span data-ttu-id="bcffd-146">**Anteckningar**</span><span class="sxs-lookup"><span data-stu-id="bcffd-146">**Notes**</span></span>     
   
   * <span data-ttu-id="bcffd-147">I **kvarhållning dagar**, kvarhållningsperioden måste vara större än säkerhetskopieringsfrekvensen.</span><span class="sxs-lookup"><span data-stu-id="bcffd-147">In **Retention Days**, the retention period must be greater than the backup frequency.</span></span>
   * <span data-ttu-id="bcffd-148">Välj **alltid ha minst en säkerhetskopia**, även om den tidigare kvarhållningsperioden.</span><span class="sxs-lookup"><span data-stu-id="bcffd-148">Select **Always keep at least one backup**, even if it is past the retention period.</span></span>
4. <span data-ttu-id="bcffd-149">Välj **Spara**.</span><span class="sxs-lookup"><span data-stu-id="bcffd-149">Select **Save**.</span></span>

<span data-ttu-id="bcffd-150">När en schemalagd säkerhetskopiering körs skapas en behållare (för att lagra säkerhetskopierade data) i storage-konto som du har angett.</span><span class="sxs-lookup"><span data-stu-id="bcffd-150">When a scheduled backup job runs, it creates a container (to store backup data) in the storage account you entered.</span></span> <span data-ttu-id="bcffd-151">Namnet på behållaren heter *BizTalk Service Name-tid*.</span><span class="sxs-lookup"><span data-stu-id="bcffd-151">The name of the container is named *BizTalk Service Name-date-time*.</span></span> 

<span data-ttu-id="bcffd-152">Om BizTalk Service instrumentpanelen visar en **misslyckades** status:</span><span class="sxs-lookup"><span data-stu-id="bcffd-152">If the BizTalk Service dashboard shows a **Failed** status:</span></span>

![Senaste status för schemalagd säkerhetskopiering][BackupStatus] 

<span data-ttu-id="bcffd-154">Länken öppnar Management Services Åtgärdsloggar för felsökning.</span><span class="sxs-lookup"><span data-stu-id="bcffd-154">The link opens the Management Services Operation Logs to help troubleshoot.</span></span> <span data-ttu-id="bcffd-155">Se [BizTalk-tjänst: felsökning med åtgärdsloggar](http://go.microsoft.com/fwlink/p/?LinkId=391211).</span><span class="sxs-lookup"><span data-stu-id="bcffd-155">See [BizTalk Services: Troubleshoot using operation logs](http://go.microsoft.com/fwlink/p/?LinkId=391211).</span></span>

## <a name="restore"></a><span data-ttu-id="bcffd-156">Återställ</span><span class="sxs-lookup"><span data-stu-id="bcffd-156">Restore</span></span>
<span data-ttu-id="bcffd-157">Du kan återställa säkerhetskopior från den klassiska Azure-portalen eller från den [återställa BizTalk Service REST API](http://go.microsoft.com/fwlink/p/?LinkID=325582).</span><span class="sxs-lookup"><span data-stu-id="bcffd-157">You can restore backups from the Azure classic portal or from the [Restore BizTalk Service REST API](http://go.microsoft.com/fwlink/p/?LinkID=325582).</span></span> <span data-ttu-id="bcffd-158">Det här avsnittet innehåller instruktioner om hur du återställer med den klassiska portalen.</span><span class="sxs-lookup"><span data-stu-id="bcffd-158">This section lists the steps to restore using the classic portal.</span></span>

#### <a name="before-restoring-a-backup"></a><span data-ttu-id="bcffd-159">Innan du återställer en säkerhetskopia</span><span class="sxs-lookup"><span data-stu-id="bcffd-159">Before restoring a backup</span></span>
* <span data-ttu-id="bcffd-160">Ny spårning, arkivering och övervakning lagrar kan anges när en BizTalk Service.</span><span class="sxs-lookup"><span data-stu-id="bcffd-160">New tracking, archiving, and monitoring stores can be entered while restoring a BizTalk Service.</span></span>
* <span data-ttu-id="bcffd-161">Samma EDI-Körningsdata har återställts.</span><span class="sxs-lookup"><span data-stu-id="bcffd-161">The same EDI Runtime data is restored.</span></span> <span data-ttu-id="bcffd-162">EDI-Runtime-säkerhetskopian lagras kontrollen tal.</span><span class="sxs-lookup"><span data-stu-id="bcffd-162">The EDI Runtime backup stores the control numbers.</span></span> <span data-ttu-id="bcffd-163">Talen återställda kontrollen är i ordning från tidpunkten för säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="bcffd-163">The restored control numbers are in sequence from the time of the backup.</span></span> <span data-ttu-id="bcffd-164">Meddelanden som bearbetas efter den senaste säkerhetskopieringen, kan återställa säkerhetskopiering innehållet orsaka dubbla kontrollen siffror.</span><span class="sxs-lookup"><span data-stu-id="bcffd-164">If messages are processed after the last backup, restoring this backup content can cause duplicate control numbers.</span></span>

#### <a name="restore-a-backup"></a><span data-ttu-id="bcffd-165">Återställ en säkerhetskopia</span><span class="sxs-lookup"><span data-stu-id="bcffd-165">Restore a backup</span></span>
1. <span data-ttu-id="bcffd-166">I den klassiska Azure-portalen väljer **ny** > **Apptjänster** > **BizTalk Service** > **återställa**:</span><span class="sxs-lookup"><span data-stu-id="bcffd-166">In the Azure classic portal, select **New** > **App Services** > **BizTalk Service** > **Restore**:</span></span>
   
    ![Återställ en säkerhetskopia][Restore]
2. <span data-ttu-id="bcffd-168">I **säkerhetskopiering URL**väljer du mappikonen och expandera Azure storage-konto som lagrar konfigurationssäkerhetskopia BizTalk Service.</span><span class="sxs-lookup"><span data-stu-id="bcffd-168">In **Backup URL**, select the folder icon and expand the Azure storage account that stores the BizTalk Service configuration backup.</span></span> <span data-ttu-id="bcffd-169">Expandera behållaren och välj den motsvarande säkerhetskopierade txt-fil i den högra rutan.</span><span class="sxs-lookup"><span data-stu-id="bcffd-169">Expand the container and in the right pane, select the corresponding back up .txt file.</span></span> 
   <br/><br/>
   <span data-ttu-id="bcffd-170">Välj **öppna**.</span><span class="sxs-lookup"><span data-stu-id="bcffd-170">Select **Open**.</span></span>
3. <span data-ttu-id="bcffd-171">På den **återställa BizTalk-tjänst** anger en **BizTalk tjänstnamnet** och kontrollera den **domän-URL**, **Edition**, och **Region** för återställda BizTalk Service.</span><span class="sxs-lookup"><span data-stu-id="bcffd-171">On the **Restore BizTalk Service** page, enter a **BizTalk Service Name** and verify the **Domain URL**, **Edition**, and **Region** for the restored BizTalk Service.</span></span> <span data-ttu-id="bcffd-172">**Skapa en ny SQL-databasinstans** för spårning av databasen:</span><span class="sxs-lookup"><span data-stu-id="bcffd-172">**Create a new SQL database instance** for the tracking database:</span></span>
   
    ![][RestoreBizTalkService]
   
    <span data-ttu-id="bcffd-173">Välj nästa-pilen.</span><span class="sxs-lookup"><span data-stu-id="bcffd-173">Select the next arrow.</span></span>
4. <span data-ttu-id="bcffd-174">Kontrollera namnet på SQL-databasen, ange den fysiska servern där SQL-databasen skapas och ett användarnamn/lösenord för servern.</span><span class="sxs-lookup"><span data-stu-id="bcffd-174">Verify the name of the SQL database, enter the physical server where the SQL database will be created, and a username/password for that server.</span></span>

    <span data-ttu-id="bcffd-175">Om du vill konfigurera SQL database edition, storlek och andra egenskaper väljer **konfigurera avancerade databasinställningar**.</span><span class="sxs-lookup"><span data-stu-id="bcffd-175">If you want to configure the SQL database edition, size, and other properties, select  **Configure Advanced Database Settings**.</span></span> 

    <span data-ttu-id="bcffd-176">Välj nästa-pilen.</span><span class="sxs-lookup"><span data-stu-id="bcffd-176">Select the next arrow.</span></span>

1. <span data-ttu-id="bcffd-177">Skapa ett nytt lagringskonto eller ange ett befintligt lagringskonto för BizTalk Service.</span><span class="sxs-lookup"><span data-stu-id="bcffd-177">Create a new storage account or enter an existing storage account for the BizTalk Service.</span></span>
2. <span data-ttu-id="bcffd-178">Välj på bockmarkeringen för att starta återställningen.</span><span class="sxs-lookup"><span data-stu-id="bcffd-178">Select the checkmark to start the restore.</span></span>

<span data-ttu-id="bcffd-179">När återställningen är klar, visas en ny BizTalk Service i ett pausat tillstånd på sidan BizTalk-tjänst i den klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="bcffd-179">Once the restoration successfully completes, a new BizTalk Service is listed in a suspended state on the BizTalk Services page in the Azure classic portal.</span></span>

### <span data-ttu-id="bcffd-180"><a name="postrestore"></a>När du återställer en säkerhetskopia</span><span class="sxs-lookup"><span data-stu-id="bcffd-180"><a name="postrestore"></a>After restoring a backup</span></span>
<span data-ttu-id="bcffd-181">BizTalk Service återställs alltid i en **pausad** tillstånd.</span><span class="sxs-lookup"><span data-stu-id="bcffd-181">The BizTalk Service is always restored in a **Suspended** state.</span></span> <span data-ttu-id="bcffd-182">I det här tillståndet kan du göra några konfigurationsändringar innan den nya miljön är fungerar, inklusive:</span><span class="sxs-lookup"><span data-stu-id="bcffd-182">In this state, you can make any configuration changes before the new environment is functional, including:</span></span>

* <span data-ttu-id="bcffd-183">Om du har skapat BizTalk Service program med Azure BizTalk Services SDK kan behöva du uppdatera autentiseringsuppgifterna med dessa program ska fungera med den återställda miljön Access Control (ACS).</span><span class="sxs-lookup"><span data-stu-id="bcffd-183">If you created BizTalk Service applications using the Azure BizTalk Services SDK, you may need to to update the Access Control (ACS) credentials in those applications to work with the restored environment.</span></span>
* <span data-ttu-id="bcffd-184">Du kan återställa en BizTalk Service för att replikera en befintlig BizTalk Service-miljö.</span><span class="sxs-lookup"><span data-stu-id="bcffd-184">You restore a BizTalk Service to replicate an existing BizTalk Service environment.</span></span> <span data-ttu-id="bcffd-185">I det här fallet om det finns avtal som konfigurerats i den ursprungliga BizTalk-Services-portalen och som använder en källmapp av FTP-kan du behöva uppdatera avtalen i den nyligen återställda miljön att använda en annan källa FTP-mapp.</span><span class="sxs-lookup"><span data-stu-id="bcffd-185">In this situation, if there are agreements configured in the original BizTalk Services portal that use a source FTP folder, you may need to update the agreements in the newly restored environment to use a different source FTP folder.</span></span> <span data-ttu-id="bcffd-186">Annars kan finnas det två olika avtal som försöker hämta samma meddelande.</span><span class="sxs-lookup"><span data-stu-id="bcffd-186">Otherwise, there may be two different agreements trying to pull the same message.</span></span>
* <span data-ttu-id="bcffd-187">Om du har återställt för miljöer med flera BizTalk Service, kontrollera att du inrikta dig på rätt miljön i Visual Studio-program, PowerShell-cmdlets, REST API: er eller handel Partner hantering OM API: er.</span><span class="sxs-lookup"><span data-stu-id="bcffd-187">If you restored to have multiple BizTalk Service environments, make sure you target the correct environment in the Visual Studio applications, PowerShell cmdlets, REST APIs, or Trading Partner Management OM APIs.</span></span>
* <span data-ttu-id="bcffd-188">Det är en bra idé att konfigurera automatisk säkerhetskopiering på den nyligen återställda BizTalk Service-miljön.</span><span class="sxs-lookup"><span data-stu-id="bcffd-188">It's a good practice to configure automated backups on the newly restored BizTalk Service environment.</span></span>

<span data-ttu-id="bcffd-189">Om du vill starta BizTalk Service i den klassiska Azure-portalen, Välj den återställda BizTalk Service och välj **återuppta** i Aktivitetsfältet.</span><span class="sxs-lookup"><span data-stu-id="bcffd-189">To start the BizTalk Service in the Azure classic portal, select the restored BizTalk Service and select **Resume** in the task bar.</span></span> 

## <a name="what-gets-backed-up"></a><span data-ttu-id="bcffd-190">Vad säkerhetskopieras</span><span class="sxs-lookup"><span data-stu-id="bcffd-190">What gets backed up</span></span>
<span data-ttu-id="bcffd-191">När en säkerhetskopia skapas följande objekt som ska säkerhetskopieras:</span><span class="sxs-lookup"><span data-stu-id="bcffd-191">When a backup is created, the following items are backed up:</span></span>

<table border="1"> 
<tr bgcolor="FAF9F9">
<th> </th>
<TH><span data-ttu-id="bcffd-192">Objekt som har säkerhetskopierats</span><span class="sxs-lookup"><span data-stu-id="bcffd-192">Items backed up</span></span></TH> 
</tr> 
<tr>
<td colspan="2"><span data-ttu-id="bcffd-193">
 <strong>Azure BizTalk-Services-portalen</strong></span><span class="sxs-lookup"><span data-stu-id="bcffd-193">
 <strong>Azure BizTalk Services Portal</strong></span></span></td>
</tr> 
<tr>
<td><span data-ttu-id="bcffd-194">Konfiguration och körning</span><span class="sxs-lookup"><span data-stu-id="bcffd-194">Configuration and Runtime</span></span></td> 
<td>
<ul>
<li><span data-ttu-id="bcffd-195">Information för partner och profil</span><span class="sxs-lookup"><span data-stu-id="bcffd-195">Partner and profile details</span></span></li>
<li><span data-ttu-id="bcffd-196">Partner avtal</span><span class="sxs-lookup"><span data-stu-id="bcffd-196">Partner Agreements</span></span></li>
<li><span data-ttu-id="bcffd-197">Anpassade sammansättningar som har distribuerats</span><span class="sxs-lookup"><span data-stu-id="bcffd-197">Custom assemblies deployed</span></span></li>
<li><span data-ttu-id="bcffd-198">Bryggor distribueras</span><span class="sxs-lookup"><span data-stu-id="bcffd-198">Bridges deployed</span></span></li>
<li><span data-ttu-id="bcffd-199">Certifikat</span><span class="sxs-lookup"><span data-stu-id="bcffd-199">Certificates</span></span></li>
<li><span data-ttu-id="bcffd-200">Transformeringar distribueras</span><span class="sxs-lookup"><span data-stu-id="bcffd-200">Transforms deployed</span></span></li>
<li><span data-ttu-id="bcffd-201">Pipelines</span><span class="sxs-lookup"><span data-stu-id="bcffd-201">Pipelines</span></span></li>
<li><span data-ttu-id="bcffd-202">Mallar som har skapats och sparats i BizTalk-Services-portalen</span><span class="sxs-lookup"><span data-stu-id="bcffd-202">Templates created and saved in the BizTalk Services Portal</span></span></li>
<li><span data-ttu-id="bcffd-203">X12 ST01 och GS01 mappningar</span><span class="sxs-lookup"><span data-stu-id="bcffd-203">X12 ST01 and GS01 mappings</span></span></li>
<li><span data-ttu-id="bcffd-204">Kontrollen siffror (EDI)</span><span class="sxs-lookup"><span data-stu-id="bcffd-204">Control numbers (EDI)</span></span></li>
<li><span data-ttu-id="bcffd-205">AS2 meddelandet MIC värden</span><span class="sxs-lookup"><span data-stu-id="bcffd-205">AS2 Message MIC values</span></span></li>
</ul>
</td>
</tr> 

<tr>
<td colspan="2"><span data-ttu-id="bcffd-206">
 <strong>Azure BizTalk-tjänst</strong></span><span class="sxs-lookup"><span data-stu-id="bcffd-206">
 <strong>Azure BizTalk Service</strong></span></span></td>
</tr> 
<tr>
<td><span data-ttu-id="bcffd-207">SSL-certifikat</span><span class="sxs-lookup"><span data-stu-id="bcffd-207">SSL Certificate</span></span></td> 
<td>
<ul>
<li><span data-ttu-id="bcffd-208">Data för SSL-certifikat</span><span class="sxs-lookup"><span data-stu-id="bcffd-208">SSL Certificate Data</span></span></li>
<li><span data-ttu-id="bcffd-209">Lösenordet för SSL-certifikat</span><span class="sxs-lookup"><span data-stu-id="bcffd-209">SSL Certificate Password</span></span></li>
</ul>
</td>
</tr> 
<tr>
<td><span data-ttu-id="bcffd-210">BizTalk-tjänst-inställningar</span><span class="sxs-lookup"><span data-stu-id="bcffd-210">BizTalk Service Settings</span></span></td> 
<td>
<ul>
<li><span data-ttu-id="bcffd-211">Skalningsantalet för enhet</span><span class="sxs-lookup"><span data-stu-id="bcffd-211">Scale unit count</span></span></li>
<li><span data-ttu-id="bcffd-212">Utgåva</span><span class="sxs-lookup"><span data-stu-id="bcffd-212">Edition</span></span></li>
<li><span data-ttu-id="bcffd-213">Produktversion</span><span class="sxs-lookup"><span data-stu-id="bcffd-213">Product Version</span></span></li>
<li><span data-ttu-id="bcffd-214">Region/Datacenter</span><span class="sxs-lookup"><span data-stu-id="bcffd-214">Region/Datacenter</span></span></li>
<li><span data-ttu-id="bcffd-215">Access Control Service (ACS) namnområde och nyckel</span><span class="sxs-lookup"><span data-stu-id="bcffd-215">Access Control Service (ACS) namespace and key</span></span></li>
<li><span data-ttu-id="bcffd-216">Spåra databasanslutningssträng</span><span class="sxs-lookup"><span data-stu-id="bcffd-216">Tracking database connection string</span></span></li>
<li><span data-ttu-id="bcffd-217">Arkivering lagringsanslutningssträng för kontot</span><span class="sxs-lookup"><span data-stu-id="bcffd-217">Archiving Storage account connection string</span></span></li>
<li><span data-ttu-id="bcffd-218">Övervaka lagringsanslutningssträng för kontot</span><span class="sxs-lookup"><span data-stu-id="bcffd-218">Monitoring storage account connection string</span></span></li>
</ul>
</td>
</tr> 
<tr>
<td colspan="2"><span data-ttu-id="bcffd-219">
 <strong>Ytterligare objekt</strong></span><span class="sxs-lookup"><span data-stu-id="bcffd-219">
 <strong>Additional Items</strong></span></span></td>
</tr> 
<tr>
<td><span data-ttu-id="bcffd-220">Spårning av databasen</span><span class="sxs-lookup"><span data-stu-id="bcffd-220">Tracking Database</span></span></td> 
<td><span data-ttu-id="bcffd-221">När BizTalk Service skapas anges spårning databasinformationen, inklusive Azure SQL-databasservern och spåra databasnamnet.</span><span class="sxs-lookup"><span data-stu-id="bcffd-221">When the BizTalk Service is created, the Tracking Database details are entered, including the Azure SQL Database Server and the Tracking Database name.</span></span> <span data-ttu-id="bcffd-222">Spårning av databasen säkerhetskopieras automatiskt inte.</span><span class="sxs-lookup"><span data-stu-id="bcffd-222">The Tracking Database is not automatically backed up.</span></span>
<br/><br/><span data-ttu-id="bcffd-223">
<strong>Viktigt</strong></span><span class="sxs-lookup"><span data-stu-id="bcffd-223">
<strong>Important</strong></span></span><br/>
<span data-ttu-id="bcffd-224">Om spårning av databasen tas bort och databas måste återställas, måste det finnas en tidigare säkerhetskopia.</span><span class="sxs-lookup"><span data-stu-id="bcffd-224">If the Tracking Database is deleted and the database needs recovered, a previous backup must exist.</span></span> <span data-ttu-id="bcffd-225">Om en säkerhetskopiering inte finns, är inte spårning-databasen och dess data återställas.</span><span class="sxs-lookup"><span data-stu-id="bcffd-225">If a backup does not exist, the Tracking Database and its data are not recoverable.</span></span> <span data-ttu-id="bcffd-226">I så fall kan du skapa en ny spårning databas med samma databasnamn.</span><span class="sxs-lookup"><span data-stu-id="bcffd-226">In this situation, create a new Tracking Database with the same database name.</span></span> <span data-ttu-id="bcffd-227">GEO-replikering rekommenderas.</span><span class="sxs-lookup"><span data-stu-id="bcffd-227">Geo-Replication is recommended.</span></span></td>
</tr> 
</table>

## <a name="next"></a><span data-ttu-id="bcffd-228">Nästa</span><span class="sxs-lookup"><span data-stu-id="bcffd-228">Next</span></span>
<span data-ttu-id="bcffd-229">Om du vill skapa Azure BizTalk-tjänst i den klassiska Azure-portalen, går du till [BizTalk-tjänst: etablering med hjälp av Azure klassiska portal](http://go.microsoft.com/fwlink/p/?LinkID=302280).</span><span class="sxs-lookup"><span data-stu-id="bcffd-229">To create Azure BizTalk Services in the Azure classic portal, go to [BizTalk Services: Provisioning Using Azure classic portal](http://go.microsoft.com/fwlink/p/?LinkID=302280).</span></span> <span data-ttu-id="bcffd-230">Om du vill börja skapa program går du till [Azure BizTalk Services](http://go.microsoft.com/fwlink/p/?LinkID=235197).</span><span class="sxs-lookup"><span data-stu-id="bcffd-230">To start creating applications, go to [Azure BizTalk Services](http://go.microsoft.com/fwlink/p/?LinkID=235197).</span></span>

## <a name="see-also"></a><span data-ttu-id="bcffd-231">Se även</span><span class="sxs-lookup"><span data-stu-id="bcffd-231">See Also</span></span>
* [<span data-ttu-id="bcffd-232">Säkerhetskopiera BizTalk-tjänst</span><span class="sxs-lookup"><span data-stu-id="bcffd-232">Backup BizTalk Service</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=325584)
* [<span data-ttu-id="bcffd-233">Återställa BizTalk-tjänst från en säkerhetskopia</span><span class="sxs-lookup"><span data-stu-id="bcffd-233">Restore BizTalk Service from Backup</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=325582)
* [<span data-ttu-id="bcffd-234">BizTalk-tjänst: Utvecklare, Basic, Standard och Premium-utgåvor diagram</span><span class="sxs-lookup"><span data-stu-id="bcffd-234">BizTalk Services: Developer, Basic, Standard and Premium Editions Chart</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302279)
* [<span data-ttu-id="bcffd-235">BizTalk-tjänst: Etablering med hjälp av Azure klassiska portal</span><span class="sxs-lookup"><span data-stu-id="bcffd-235">BizTalk Services: Provisioning Using Azure classic portal</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302280)
* [<span data-ttu-id="bcffd-236">BizTalk Services: Etablering av statusdiagram</span><span class="sxs-lookup"><span data-stu-id="bcffd-236">BizTalk Services: Provisioning Status Chart</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=329870)
* [<span data-ttu-id="bcffd-237">BizTalk Services: Flikarna Instrumentpanel, Övervakare och Skalning</span><span class="sxs-lookup"><span data-stu-id="bcffd-237">BizTalk Services: Dashboard, Monitor and Scale tabs</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302281)
* [<span data-ttu-id="bcffd-238">BizTalk Services: Begränsning</span><span class="sxs-lookup"><span data-stu-id="bcffd-238">BizTalk Services: Throttling</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302282)
* [<span data-ttu-id="bcffd-239">BizTalk Services: Utfärdarens namn och nyckel</span><span class="sxs-lookup"><span data-stu-id="bcffd-239">BizTalk Services: Issuer Name and Issuer Key</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=303941)
* [<span data-ttu-id="bcffd-240">Hur gör jag för att börja använda Azure BizTalk Services SDK?</span><span class="sxs-lookup"><span data-stu-id="bcffd-240">How do I Start Using the Azure BizTalk Services SDK</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302335)

[BackupStatus]: ./media/biztalk-backup-restore/status-last-backup.png
[Restore]: ./media/biztalk-backup-restore/restore-ui.png
[AutomaticBU]: ./media/biztalk-backup-restore/AutomaticBU.png
[RestoreBizTalkService]: ./media/biztalk-backup-restore/RestoreBizTalkServiceWindow.png

