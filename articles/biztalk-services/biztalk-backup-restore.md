---
title: "aaaCreate och återställa en säkerhetskopia i BizTalk-tjänst | Microsoft Docs"
description: "BizTalk-tjänster omfattar säkerhetskopiering och återställning. Lär dig hur toocreate och återställa en säkerhetskopia och bestämma vad säkerhetskopieras. MABS WABS"
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
ms.openlocfilehash: 32356ad870678fa5fd5bbbbf13d9377188f770a1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="biztalk-services-backup-and-restore"></a><span data-ttu-id="6fa35-105">BizTalk Services: Säkerhetskopiering och återställning</span><span class="sxs-lookup"><span data-stu-id="6fa35-105">BizTalk Services: Backup and Restore</span></span>

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

<span data-ttu-id="6fa35-106">Azure BizTalk-tjänster inkluderar funktioner för säkerhetskopiering och återställning.</span><span class="sxs-lookup"><span data-stu-id="6fa35-106">Azure BizTalk Services includes Backup and Restore capabilities.</span></span> <span data-ttu-id="6fa35-107">Det här avsnittet beskrivs hur toobackup och återställning BizTalk-tjänster med hjälp av hello klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="6fa35-107">This topic describes how toobackup and restore BizTalk Services using hello Azure classic portal.</span></span>

<span data-ttu-id="6fa35-108">Du kan också säkerhetskopiera BizTalk-tjänster med hjälp av hello [BizTalk Services REST API](http://go.microsoft.com/fwlink/p/?LinkID=325584).</span><span class="sxs-lookup"><span data-stu-id="6fa35-108">You can also back up BizTalk Services using hello [BizTalk Services REST API](http://go.microsoft.com/fwlink/p/?LinkID=325584).</span></span> 

> [!NOTE]
> <span data-ttu-id="6fa35-109">Hybridanslutningar säkerhetskopieras inte, oavsett hello Edition.</span><span class="sxs-lookup"><span data-stu-id="6fa35-109">Hybrid Connections are NOT backed up, regardless of hello Edition.</span></span> <span data-ttu-id="6fa35-110">Du måste återskapa din hybridanslutningar.</span><span class="sxs-lookup"><span data-stu-id="6fa35-110">You must recreate your hybrid connections.</span></span>


## <a name="before-you-begin"></a><span data-ttu-id="6fa35-111">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="6fa35-111">Before you Begin</span></span>
* <span data-ttu-id="6fa35-112">Säkerhetskopiering och återställning kanske inte tillgänglig för alla versioner.</span><span class="sxs-lookup"><span data-stu-id="6fa35-112">Backup and Restore may not be available for all editions.</span></span> <span data-ttu-id="6fa35-113">Se [BizTalk-tjänst: utgåvor diagram](biztalk-editions-feature-chart.md).</span><span class="sxs-lookup"><span data-stu-id="6fa35-113">See [BizTalk Services: Editions Chart](biztalk-editions-feature-chart.md).</span></span>
* <span data-ttu-id="6fa35-114">Med hello klassiska Azure-portalen kan du skapa en säkerhetskopiering på begäran eller skapa en schemalagd säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="6fa35-114">Using hello Azure classic portal, you can create an On Demand backup or create a scheduled backup.</span></span> 
* <span data-ttu-id="6fa35-115">Säkerhetskopiera innehåll kan vara återställda toohello samma BizTalk Service eller tooa ny BizTalk Service.</span><span class="sxs-lookup"><span data-stu-id="6fa35-115">Backup content can be restored toohello same BizTalk Service or tooa new BizTalk Service.</span></span> <span data-ttu-id="6fa35-116">toorestore hello BizTalk Service med samma namn, hello befintlig BizTalk Service måste du ta bort hello och hello namn måste vara tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="6fa35-116">toorestore hello BizTalk Service using hello same name, hello existing BizTalk Service must be deleted and hello name must be available.</span></span> <span data-ttu-id="6fa35-117">När du tar bort en BizTalk Service kan det ta längre tid än ville för hello samma namn toobe tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="6fa35-117">After you delete a BizTalk Service, it can take longer time than wanted for hello same name toobe available.</span></span> <span data-ttu-id="6fa35-118">Om du inte kan vänta hello samma namn toobe tillgängliga, och sedan återställer tooa ny BizTalk Service.</span><span class="sxs-lookup"><span data-stu-id="6fa35-118">If you cannot wait for hello same name toobe available, then restore tooa new BizTalk Service.</span></span>
* <span data-ttu-id="6fa35-119">BizTalk-tjänst kan vara återställda toohello samma edition eller högre.</span><span class="sxs-lookup"><span data-stu-id="6fa35-119">BizTalk Services can be restored toohello same edition or a higher edition.</span></span> <span data-ttu-id="6fa35-120">Återställa BizTalk-tjänst tooa stöds lägre utgåva från när hello säkerhetskopian skapades, inte.</span><span class="sxs-lookup"><span data-stu-id="6fa35-120">Restoring BizTalk Services tooa lower edition, from when hello backup was taken, is not supported.</span></span>
  
    <span data-ttu-id="6fa35-121">Till exempel återställs med hello Basic-versionen kan vara en säkerhetskopia toohello Premium Edition.</span><span class="sxs-lookup"><span data-stu-id="6fa35-121">For example, a backup using hello Basic Edition can be restored toohello Premium Edition.</span></span> <span data-ttu-id="6fa35-122">En säkerhetskopiering med hello Premium Edition måste återställas toohello Standard Edition.</span><span class="sxs-lookup"><span data-stu-id="6fa35-122">A backup using hello Premium Edition cannot be restored toohello Standard Edition.</span></span>
* <span data-ttu-id="6fa35-123">hello EDI-kontrollen siffror säkerhetskopieras toomaintain kontinuitet i hello kontrollen siffror.</span><span class="sxs-lookup"><span data-stu-id="6fa35-123">hello EDI Control numbers are backed up toomaintain continuity of hello control numbers.</span></span> <span data-ttu-id="6fa35-124">Meddelanden som bearbetas efter hello senaste säkerhetskopieringen, kan återställa säkerhetskopiering innehållet orsaka dubbla kontrollen siffror.</span><span class="sxs-lookup"><span data-stu-id="6fa35-124">If messages are processed after hello last backup, restoring this backup content can cause duplicate control numbers.</span></span>
* <span data-ttu-id="6fa35-125">Om en batch har aktiva meddelanden, bearbeta hello batch **innan** köra en säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="6fa35-125">If a batch has active messages, process hello batch **before** running a backup.</span></span> <span data-ttu-id="6fa35-126">När du skapar en säkerhetskopia (som krävs eller är schemalagda) lagras aldrig meddelanden i batchar.</span><span class="sxs-lookup"><span data-stu-id="6fa35-126">When creating a backup (as needed or scheduled), messages in batches are never stored.</span></span> 
  
    <span data-ttu-id="6fa35-127">**Om en säkerhetskopia görs med aktiva meddelanden i en grupp, säkerhetskopieras inte dessa meddelanden och därför går förlorade.**</span><span class="sxs-lookup"><span data-stu-id="6fa35-127">**If a backup is taken with active messages in a batch, these messages are not backed up and are therefore lost.**</span></span>
* <span data-ttu-id="6fa35-128">Valfritt: I hello BizTalk-Services-portalen, stoppa några hanteringsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="6fa35-128">Optional: In hello BizTalk Services Portal, stop any management operations.</span></span>

## <a name="create-a-backup"></a><span data-ttu-id="6fa35-129">Skapa en säkerhetskopia</span><span class="sxs-lookup"><span data-stu-id="6fa35-129">Create a backup</span></span>
<span data-ttu-id="6fa35-130">En säkerhetskopia kan vidtas när som helst och är helt styrs av du.</span><span class="sxs-lookup"><span data-stu-id="6fa35-130">A backup can be taken at any time and is completely controlled by you.</span></span> <span data-ttu-id="6fa35-131">Det här avsnittet innehåller hello steg toocreate säkerhetskopior med hello Azure klassiska portal, inklusive:</span><span class="sxs-lookup"><span data-stu-id="6fa35-131">This section lists hello steps toocreate backups using hello Azure classic portal, including:</span></span>

[<span data-ttu-id="6fa35-132">På begäran-säkerhetskopia</span><span class="sxs-lookup"><span data-stu-id="6fa35-132">On Demand backup</span></span>](#backupnow)

[<span data-ttu-id="6fa35-133">Schemalägga en säkerhetskopia</span><span class="sxs-lookup"><span data-stu-id="6fa35-133">Schedule a backup</span></span>](#backupschedule)

#### <span data-ttu-id="6fa35-134"><a name="backupnow"></a>På begäran-säkerhetskopia</span><span class="sxs-lookup"><span data-stu-id="6fa35-134"><a name="backupnow"></a>On Demand backup</span></span>
1. <span data-ttu-id="6fa35-135">Välj i hello klassiska Azure-portalen, **BizTalk-tjänst**, och sedan väljer hello BizTalk Service du vill toobackup.</span><span class="sxs-lookup"><span data-stu-id="6fa35-135">In hello Azure classic portal, select **BizTalk Services**, and then select hello BizTalk Service you want toobackup.</span></span>
2. <span data-ttu-id="6fa35-136">I hello **instrumentpanelen** väljer **säkerhetskopiera** på hello hello sidans nederkant.</span><span class="sxs-lookup"><span data-stu-id="6fa35-136">In hello **Dashboard** tab, select **Back up** at hello bottom of hello page.</span></span>
3. <span data-ttu-id="6fa35-137">Ange ett namn på säkerhetskopia.</span><span class="sxs-lookup"><span data-stu-id="6fa35-137">Enter a backup name.</span></span> <span data-ttu-id="6fa35-138">Ange till exempel *myBizTalkService*för*datum*.</span><span class="sxs-lookup"><span data-stu-id="6fa35-138">For example, enter *myBizTalkService*BU*Date*.</span></span>
4. <span data-ttu-id="6fa35-139">Välj ett blob storage-konto och välj hello markering toostart hello säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="6fa35-139">Choose a blob storage account and select hello checkmark toostart hello backup.</span></span>

<span data-ttu-id="6fa35-140">När hello säkerhetskopieringen är klar skapas en behållare med hello namn du anger i hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="6fa35-140">Once hello backup completes, a container with hello backup name you enter is created in hello storage account.</span></span> <span data-ttu-id="6fa35-141">Den här behållaren innehåller BizTalk Service konfigurationen för säkerhetskopieringen.</span><span class="sxs-lookup"><span data-stu-id="6fa35-141">This container contains your BizTalk Service backup configuration.</span></span>

#### <span data-ttu-id="6fa35-142"><a name="backupschedule"></a>Schemalägga en säkerhetskopia</span><span class="sxs-lookup"><span data-stu-id="6fa35-142"><a name="backupschedule"></a>Schedule a backup</span></span>
1. <span data-ttu-id="6fa35-143">Välj i hello klassiska Azure-portalen, **BizTalk-tjänst**, Välj hello BizTalk Service namn du vill tooschedule hello säkerhetskopiering och välj sedan hello **konfigurera** fliken.</span><span class="sxs-lookup"><span data-stu-id="6fa35-143">In hello Azure classic portal, select **BizTalk Services**, select hello BizTalk Service name you want tooschedule hello backup, and then select hello **Configure** tab.</span></span>
2. <span data-ttu-id="6fa35-144">Ange hello **Status för säkerhetskopiering** för**automatisk**.</span><span class="sxs-lookup"><span data-stu-id="6fa35-144">Set hello **Backup Status** too**Automatic**.</span></span> 
3. <span data-ttu-id="6fa35-145">Välj hello **Lagringskonto** toostore Hej säkerhetskopiering, ange hello **frekvens** toocreate hello, och hur länge tookeep hello säkerhetskopior (**kvarhållning dagar**):</span><span class="sxs-lookup"><span data-stu-id="6fa35-145">Select hello **Storage Account** toostore hello backup, enter hello **Frequency** toocreate hello backups, and how long tookeep hello backups (**Retention Days**):</span></span>
   
    ![][AutomaticBU]
   
    <span data-ttu-id="6fa35-146">**Anteckningar**</span><span class="sxs-lookup"><span data-stu-id="6fa35-146">**Notes**</span></span>     
   
   * <span data-ttu-id="6fa35-147">I **kvarhållning dagar**, hello kvarhållningsperioden måste vara större än säkerhetskopieringsfrekvensen för hello.</span><span class="sxs-lookup"><span data-stu-id="6fa35-147">In **Retention Days**, hello retention period must be greater than hello backup frequency.</span></span>
   * <span data-ttu-id="6fa35-148">Välj **alltid ha minst en säkerhetskopia**, även om den tidigare hello Bevarandeperiod.</span><span class="sxs-lookup"><span data-stu-id="6fa35-148">Select **Always keep at least one backup**, even if it is past hello retention period.</span></span>
4. <span data-ttu-id="6fa35-149">Välj **Spara**.</span><span class="sxs-lookup"><span data-stu-id="6fa35-149">Select **Save**.</span></span>

<span data-ttu-id="6fa35-150">När en schemalagd säkerhetskopiering körs skapas en behållare (toostore säkerhetskopieringsdata) i hello storage-konto som du har angett.</span><span class="sxs-lookup"><span data-stu-id="6fa35-150">When a scheduled backup job runs, it creates a container (toostore backup data) in hello storage account you entered.</span></span> <span data-ttu-id="6fa35-151">hello namnet på behållaren hello heter *BizTalk Service Name-tid*.</span><span class="sxs-lookup"><span data-stu-id="6fa35-151">hello name of hello container is named *BizTalk Service Name-date-time*.</span></span> 

<span data-ttu-id="6fa35-152">Om hello BizTalk Service instrumentpanelen visar en **misslyckades** status:</span><span class="sxs-lookup"><span data-stu-id="6fa35-152">If hello BizTalk Service dashboard shows a **Failed** status:</span></span>

![Senaste status för schemalagd säkerhetskopiering][BackupStatus] 

<span data-ttu-id="6fa35-154">hello länken öppnar hello Management åtgärden tjänstloggar toohelp felsöka.</span><span class="sxs-lookup"><span data-stu-id="6fa35-154">hello link opens hello Management Services Operation Logs toohelp troubleshoot.</span></span> <span data-ttu-id="6fa35-155">Se [BizTalk-tjänst: felsökning med åtgärdsloggar](http://go.microsoft.com/fwlink/p/?LinkId=391211).</span><span class="sxs-lookup"><span data-stu-id="6fa35-155">See [BizTalk Services: Troubleshoot using operation logs](http://go.microsoft.com/fwlink/p/?LinkId=391211).</span></span>

## <a name="restore"></a><span data-ttu-id="6fa35-156">Återställ</span><span class="sxs-lookup"><span data-stu-id="6fa35-156">Restore</span></span>
<span data-ttu-id="6fa35-157">Du kan återställa säkerhetskopior från hello klassiska Azure-portalen eller hello [återställa BizTalk Service REST API](http://go.microsoft.com/fwlink/p/?LinkID=325582).</span><span class="sxs-lookup"><span data-stu-id="6fa35-157">You can restore backups from hello Azure classic portal or from hello [Restore BizTalk Service REST API](http://go.microsoft.com/fwlink/p/?LinkID=325582).</span></span> <span data-ttu-id="6fa35-158">Det här avsnittet innehåller hello steg toorestore med hello klassiska portalen.</span><span class="sxs-lookup"><span data-stu-id="6fa35-158">This section lists hello steps toorestore using hello classic portal.</span></span>

#### <a name="before-restoring-a-backup"></a><span data-ttu-id="6fa35-159">Innan du återställer en säkerhetskopia</span><span class="sxs-lookup"><span data-stu-id="6fa35-159">Before restoring a backup</span></span>
* <span data-ttu-id="6fa35-160">Ny spårning, arkivering och övervakning lagrar kan anges när en BizTalk Service.</span><span class="sxs-lookup"><span data-stu-id="6fa35-160">New tracking, archiving, and monitoring stores can be entered while restoring a BizTalk Service.</span></span>
* <span data-ttu-id="6fa35-161">hello samma EDI Körningsdata har återställts.</span><span class="sxs-lookup"><span data-stu-id="6fa35-161">hello same EDI Runtime data is restored.</span></span> <span data-ttu-id="6fa35-162">hello EDI Runtime säkerhetskopiering lagrar hello kontrollen tal.</span><span class="sxs-lookup"><span data-stu-id="6fa35-162">hello EDI Runtime backup stores hello control numbers.</span></span> <span data-ttu-id="6fa35-163">hello återställts kontrollen siffror finns i sekvens från hello tid för hello säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="6fa35-163">hello restored control numbers are in sequence from hello time of hello backup.</span></span> <span data-ttu-id="6fa35-164">Meddelanden som bearbetas efter hello senaste säkerhetskopieringen, kan återställa säkerhetskopiering innehållet orsaka dubbla kontrollen siffror.</span><span class="sxs-lookup"><span data-stu-id="6fa35-164">If messages are processed after hello last backup, restoring this backup content can cause duplicate control numbers.</span></span>

#### <a name="restore-a-backup"></a><span data-ttu-id="6fa35-165">Återställ en säkerhetskopia</span><span class="sxs-lookup"><span data-stu-id="6fa35-165">Restore a backup</span></span>
1. <span data-ttu-id="6fa35-166">Välj i hello klassiska Azure-portalen, **ny** > **Apptjänster** > **BizTalk Service** > **återställa** :</span><span class="sxs-lookup"><span data-stu-id="6fa35-166">In hello Azure classic portal, select **New** > **App Services** > **BizTalk Service** > **Restore**:</span></span>
   
    ![Återställ en säkerhetskopia][Restore]
2. <span data-ttu-id="6fa35-168">I **säkerhetskopiering URL**hello mappikon och välj utöka hello Azure storage-konto som lagrar hello BizTalk Service konfigurationssäkerhetskopia.</span><span class="sxs-lookup"><span data-stu-id="6fa35-168">In **Backup URL**, select hello folder icon and expand hello Azure storage account that stores hello BizTalk Service configuration backup.</span></span> <span data-ttu-id="6fa35-169">Utöka hello behållare och välj hello motsvarande säkerhetskopiera txt-fil i hello högra rutan.</span><span class="sxs-lookup"><span data-stu-id="6fa35-169">Expand hello container and in hello right pane, select hello corresponding back up .txt file.</span></span> 
   <br/><br/>
   <span data-ttu-id="6fa35-170">Välj **öppna**.</span><span class="sxs-lookup"><span data-stu-id="6fa35-170">Select **Open**.</span></span>
3. <span data-ttu-id="6fa35-171">På hello **återställa BizTalk-tjänst** anger en **BizTalk tjänstnamnet** och kontrollera hello **domän-URL**, **Edition**, och **Region** för hello återställts BizTalk Service.</span><span class="sxs-lookup"><span data-stu-id="6fa35-171">On hello **Restore BizTalk Service** page, enter a **BizTalk Service Name** and verify hello **Domain URL**, **Edition**, and **Region** for hello restored BizTalk Service.</span></span> <span data-ttu-id="6fa35-172">**Skapa en ny SQL-databasinstans** för hello spårning av databasen:</span><span class="sxs-lookup"><span data-stu-id="6fa35-172">**Create a new SQL database instance** for hello tracking database:</span></span>
   
    ![][RestoreBizTalkService]
   
    <span data-ttu-id="6fa35-173">Välj hello nästa-pilen.</span><span class="sxs-lookup"><span data-stu-id="6fa35-173">Select hello next arrow.</span></span>
4. <span data-ttu-id="6fa35-174">Verifierar hello namn hello SQL-databas, ange hello fysisk server där hello SQL-databasen skapas och ett användarnamn/lösenord för servern.</span><span class="sxs-lookup"><span data-stu-id="6fa35-174">Verify hello name of hello SQL database, enter hello physical server where hello SQL database will be created, and a username/password for that server.</span></span>

    <span data-ttu-id="6fa35-175">Om du vill tooconfigure hello SQL database edition, storlek och andra egenskaper, Välj **konfigurera avancerade databasinställningar**.</span><span class="sxs-lookup"><span data-stu-id="6fa35-175">If you want tooconfigure hello SQL database edition, size, and other properties, select  **Configure Advanced Database Settings**.</span></span> 

    <span data-ttu-id="6fa35-176">Välj hello nästa-pilen.</span><span class="sxs-lookup"><span data-stu-id="6fa35-176">Select hello next arrow.</span></span>

1. <span data-ttu-id="6fa35-177">Skapa ett nytt lagringskonto eller ange ett befintligt lagringskonto för hello BizTalk Service.</span><span class="sxs-lookup"><span data-stu-id="6fa35-177">Create a new storage account or enter an existing storage account for hello BizTalk Service.</span></span>
2. <span data-ttu-id="6fa35-178">Välj hello markering toostart hello återställning.</span><span class="sxs-lookup"><span data-stu-id="6fa35-178">Select hello checkmark toostart hello restore.</span></span>

<span data-ttu-id="6fa35-179">När hello återställningen har slutförts visas en ny BizTalk Service i ett pausat tillstånd på hello BizTalk-tjänst sida i hello klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="6fa35-179">Once hello restoration successfully completes, a new BizTalk Service is listed in a suspended state on hello BizTalk Services page in hello Azure classic portal.</span></span>

### <span data-ttu-id="6fa35-180"><a name="postrestore"></a>När du återställer en säkerhetskopia</span><span class="sxs-lookup"><span data-stu-id="6fa35-180"><a name="postrestore"></a>After restoring a backup</span></span>
<span data-ttu-id="6fa35-181">hello BizTalk Service alltid återställs i ett **pausad** tillstånd.</span><span class="sxs-lookup"><span data-stu-id="6fa35-181">hello BizTalk Service is always restored in a **Suspended** state.</span></span> <span data-ttu-id="6fa35-182">I det här tillståndet kan göra ändringar i konfigurationen innan nya hello-miljön är fungerar, inklusive:</span><span class="sxs-lookup"><span data-stu-id="6fa35-182">In this state, you can make any configuration changes before hello new environment is functional, including:</span></span>

* <span data-ttu-id="6fa35-183">Om du har skapat BizTalk Service program med hjälp av hello Azure BizTalk Services SDK behöva tootooupdate hello Access Control (ACS) autentiseringsuppgifter i dessa program toowork med hello återställts-miljön.</span><span class="sxs-lookup"><span data-stu-id="6fa35-183">If you created BizTalk Service applications using hello Azure BizTalk Services SDK, you may need tootooupdate hello Access Control (ACS) credentials in those applications toowork with hello restored environment.</span></span>
* <span data-ttu-id="6fa35-184">Du kan återställa en BizTalk Service tooreplicate en befintlig BizTalk Service-miljö.</span><span class="sxs-lookup"><span data-stu-id="6fa35-184">You restore a BizTalk Service tooreplicate an existing BizTalk Service environment.</span></span> <span data-ttu-id="6fa35-185">I det här fallet om det finns avtal som konfigurerats i hello ursprungliga BizTalk-Services-portalen och som använder källmappen FTP, kanske du måste tooupdate hello avtal i hello nyligen återställts miljö toouse en annan FTP-källmapp.</span><span class="sxs-lookup"><span data-stu-id="6fa35-185">In this situation, if there are agreements configured in hello original BizTalk Services portal that use a source FTP folder, you may need tooupdate hello agreements in hello newly restored environment toouse a different source FTP folder.</span></span> <span data-ttu-id="6fa35-186">Annars kan det finnas två olika avtal försök toopull hello samma meddelande.</span><span class="sxs-lookup"><span data-stu-id="6fa35-186">Otherwise, there may be two different agreements trying toopull hello same message.</span></span>
* <span data-ttu-id="6fa35-187">Om du har återställt toohave miljöer med flera BizTalk Service, kontrollera att du anpassar hello rätt miljö i hello Visual Studio-program, PowerShell-cmdlets, REST API: er eller handel Partner hantering OM API: er.</span><span class="sxs-lookup"><span data-stu-id="6fa35-187">If you restored toohave multiple BizTalk Service environments, make sure you target hello correct environment in hello Visual Studio applications, PowerShell cmdlets, REST APIs, or Trading Partner Management OM APIs.</span></span>
* <span data-ttu-id="6fa35-188">Det är en bra idé tooconfigure automatisk säkerhetskopieringar på hello nyligen återställts BizTalk-miljö.</span><span class="sxs-lookup"><span data-stu-id="6fa35-188">It's a good practice tooconfigure automated backups on hello newly restored BizTalk Service environment.</span></span>

<span data-ttu-id="6fa35-189">toostart hello BizTalk Service i hello klassiska Azure-portalen väljer hello återställts BizTalk Service och välj **återuppta** i hello Aktivitetsfältet.</span><span class="sxs-lookup"><span data-stu-id="6fa35-189">toostart hello BizTalk Service in hello Azure classic portal, select hello restored BizTalk Service and select **Resume** in hello task bar.</span></span> 

## <a name="what-gets-backed-up"></a><span data-ttu-id="6fa35-190">Vad säkerhetskopieras</span><span class="sxs-lookup"><span data-stu-id="6fa35-190">What gets backed up</span></span>
<span data-ttu-id="6fa35-191">När en säkerhetskopia skapas säkerhetskopieras hello följande:</span><span class="sxs-lookup"><span data-stu-id="6fa35-191">When a backup is created, hello following items are backed up:</span></span>

<table border="1"> 
<tr bgcolor="FAF9F9">
<th> </th>
<TH><span data-ttu-id="6fa35-192">Objekt som har säkerhetskopierats</span><span class="sxs-lookup"><span data-stu-id="6fa35-192">Items backed up</span></span></TH> 
</tr> 
<tr>
<td colspan="2"><span data-ttu-id="6fa35-193">
 <strong>Azure BizTalk-Services-portalen</strong></span><span class="sxs-lookup"><span data-stu-id="6fa35-193">
 <strong>Azure BizTalk Services Portal</strong></span></span></td>
</tr> 
<tr>
<td><span data-ttu-id="6fa35-194">Konfiguration och körning</span><span class="sxs-lookup"><span data-stu-id="6fa35-194">Configuration and Runtime</span></span></td> 
<td>
<ul>
<li><span data-ttu-id="6fa35-195">Information för partner och profil</span><span class="sxs-lookup"><span data-stu-id="6fa35-195">Partner and profile details</span></span></li>
<li><span data-ttu-id="6fa35-196">Partner avtal</span><span class="sxs-lookup"><span data-stu-id="6fa35-196">Partner Agreements</span></span></li>
<li><span data-ttu-id="6fa35-197">Anpassade sammansättningar som har distribuerats</span><span class="sxs-lookup"><span data-stu-id="6fa35-197">Custom assemblies deployed</span></span></li>
<li><span data-ttu-id="6fa35-198">Bryggor distribueras</span><span class="sxs-lookup"><span data-stu-id="6fa35-198">Bridges deployed</span></span></li>
<li><span data-ttu-id="6fa35-199">Certifikat</span><span class="sxs-lookup"><span data-stu-id="6fa35-199">Certificates</span></span></li>
<li><span data-ttu-id="6fa35-200">Transformeringar distribueras</span><span class="sxs-lookup"><span data-stu-id="6fa35-200">Transforms deployed</span></span></li>
<li><span data-ttu-id="6fa35-201">Pipelines</span><span class="sxs-lookup"><span data-stu-id="6fa35-201">Pipelines</span></span></li>
<li><span data-ttu-id="6fa35-202">Mallar som har skapats och sparats i hello BizTalk-Services-portalen</span><span class="sxs-lookup"><span data-stu-id="6fa35-202">Templates created and saved in hello BizTalk Services Portal</span></span></li>
<li><span data-ttu-id="6fa35-203">X12 ST01 och GS01 mappningar</span><span class="sxs-lookup"><span data-stu-id="6fa35-203">X12 ST01 and GS01 mappings</span></span></li>
<li><span data-ttu-id="6fa35-204">Kontrollen siffror (EDI)</span><span class="sxs-lookup"><span data-stu-id="6fa35-204">Control numbers (EDI)</span></span></li>
<li><span data-ttu-id="6fa35-205">AS2 meddelandet MIC värden</span><span class="sxs-lookup"><span data-stu-id="6fa35-205">AS2 Message MIC values</span></span></li>
</ul>
</td>
</tr> 

<tr>
<td colspan="2"><span data-ttu-id="6fa35-206">
 <strong>Azure BizTalk-tjänst</strong></span><span class="sxs-lookup"><span data-stu-id="6fa35-206">
 <strong>Azure BizTalk Service</strong></span></span></td>
</tr> 
<tr>
<td><span data-ttu-id="6fa35-207">SSL-certifikat</span><span class="sxs-lookup"><span data-stu-id="6fa35-207">SSL Certificate</span></span></td> 
<td>
<ul>
<li><span data-ttu-id="6fa35-208">Data för SSL-certifikat</span><span class="sxs-lookup"><span data-stu-id="6fa35-208">SSL Certificate Data</span></span></li>
<li><span data-ttu-id="6fa35-209">Lösenordet för SSL-certifikat</span><span class="sxs-lookup"><span data-stu-id="6fa35-209">SSL Certificate Password</span></span></li>
</ul>
</td>
</tr> 
<tr>
<td><span data-ttu-id="6fa35-210">BizTalk-tjänst-inställningar</span><span class="sxs-lookup"><span data-stu-id="6fa35-210">BizTalk Service Settings</span></span></td> 
<td>
<ul>
<li><span data-ttu-id="6fa35-211">Skalningsantalet för enhet</span><span class="sxs-lookup"><span data-stu-id="6fa35-211">Scale unit count</span></span></li>
<li><span data-ttu-id="6fa35-212">Utgåva</span><span class="sxs-lookup"><span data-stu-id="6fa35-212">Edition</span></span></li>
<li><span data-ttu-id="6fa35-213">Produktversion</span><span class="sxs-lookup"><span data-stu-id="6fa35-213">Product Version</span></span></li>
<li><span data-ttu-id="6fa35-214">Region/Datacenter</span><span class="sxs-lookup"><span data-stu-id="6fa35-214">Region/Datacenter</span></span></li>
<li><span data-ttu-id="6fa35-215">Access Control Service (ACS) namnområde och nyckel</span><span class="sxs-lookup"><span data-stu-id="6fa35-215">Access Control Service (ACS) namespace and key</span></span></li>
<li><span data-ttu-id="6fa35-216">Spåra databasanslutningssträng</span><span class="sxs-lookup"><span data-stu-id="6fa35-216">Tracking database connection string</span></span></li>
<li><span data-ttu-id="6fa35-217">Arkivering lagringsanslutningssträng för kontot</span><span class="sxs-lookup"><span data-stu-id="6fa35-217">Archiving Storage account connection string</span></span></li>
<li><span data-ttu-id="6fa35-218">Övervaka lagringsanslutningssträng för kontot</span><span class="sxs-lookup"><span data-stu-id="6fa35-218">Monitoring storage account connection string</span></span></li>
</ul>
</td>
</tr> 
<tr>
<td colspan="2"><span data-ttu-id="6fa35-219">
 <strong>Ytterligare objekt</strong></span><span class="sxs-lookup"><span data-stu-id="6fa35-219">
 <strong>Additional Items</strong></span></span></td>
</tr> 
<tr>
<td><span data-ttu-id="6fa35-220">Spårning av databasen</span><span class="sxs-lookup"><span data-stu-id="6fa35-220">Tracking Database</span></span></td> 
<td><span data-ttu-id="6fa35-221">När hello BizTalk Service skapas registreras hello spårning databasinformation, inklusive hello Azure SQL-databasservern och hello spårning databasnamnet.</span><span class="sxs-lookup"><span data-stu-id="6fa35-221">When hello BizTalk Service is created, hello Tracking Database details are entered, including hello Azure SQL Database Server and hello Tracking Database name.</span></span> <span data-ttu-id="6fa35-222">hello spårningsdatabasen automatiskt säkerhetskopieras inte.</span><span class="sxs-lookup"><span data-stu-id="6fa35-222">hello Tracking Database is not automatically backed up.</span></span>
<br/><br/><span data-ttu-id="6fa35-223">
<strong>Viktigt</strong></span><span class="sxs-lookup"><span data-stu-id="6fa35-223">
<strong>Important</strong></span></span><br/>
<span data-ttu-id="6fa35-224">Om hello spårningsdatabasen tas bort och hello databasen måste återställas, måste det finnas en tidigare säkerhetskopia.</span><span class="sxs-lookup"><span data-stu-id="6fa35-224">If hello Tracking Database is deleted and hello database needs recovered, a previous backup must exist.</span></span> <span data-ttu-id="6fa35-225">Om en säkerhetskopiering inte finns, är inte hello spårningsdatabasen och dess data återställas.</span><span class="sxs-lookup"><span data-stu-id="6fa35-225">If a backup does not exist, hello Tracking Database and its data are not recoverable.</span></span> <span data-ttu-id="6fa35-226">I det här fallet skapar du en ny databas för spårning med hello samma databasnamn.</span><span class="sxs-lookup"><span data-stu-id="6fa35-226">In this situation, create a new Tracking Database with hello same database name.</span></span> <span data-ttu-id="6fa35-227">GEO-replikering rekommenderas.</span><span class="sxs-lookup"><span data-stu-id="6fa35-227">Geo-Replication is recommended.</span></span></td>
</tr> 
</table>

## <a name="next"></a><span data-ttu-id="6fa35-228">Nästa</span><span class="sxs-lookup"><span data-stu-id="6fa35-228">Next</span></span>
<span data-ttu-id="6fa35-229">toocreate Azure BizTalk-tjänst i hello Azure klassiska portal, gå för[BizTalk-tjänst: etablering med hjälp av Azure klassiska portal](http://go.microsoft.com/fwlink/p/?LinkID=302280).</span><span class="sxs-lookup"><span data-stu-id="6fa35-229">toocreate Azure BizTalk Services in hello Azure classic portal, go too[BizTalk Services: Provisioning Using Azure classic portal](http://go.microsoft.com/fwlink/p/?LinkID=302280).</span></span> <span data-ttu-id="6fa35-230">för att skapa program, gå toostart[Azure BizTalk Services](http://go.microsoft.com/fwlink/p/?LinkID=235197).</span><span class="sxs-lookup"><span data-stu-id="6fa35-230">toostart creating applications, go too[Azure BizTalk Services](http://go.microsoft.com/fwlink/p/?LinkID=235197).</span></span>

## <a name="see-also"></a><span data-ttu-id="6fa35-231">Se även</span><span class="sxs-lookup"><span data-stu-id="6fa35-231">See Also</span></span>
* [<span data-ttu-id="6fa35-232">Säkerhetskopiera BizTalk-tjänst</span><span class="sxs-lookup"><span data-stu-id="6fa35-232">Backup BizTalk Service</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=325584)
* [<span data-ttu-id="6fa35-233">Återställa BizTalk-tjänst från en säkerhetskopia</span><span class="sxs-lookup"><span data-stu-id="6fa35-233">Restore BizTalk Service from Backup</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=325582)
* [<span data-ttu-id="6fa35-234">BizTalk-tjänst: Utvecklare, Basic, Standard och Premium-utgåvor diagram</span><span class="sxs-lookup"><span data-stu-id="6fa35-234">BizTalk Services: Developer, Basic, Standard and Premium Editions Chart</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302279)
* [<span data-ttu-id="6fa35-235">BizTalk-tjänst: Etablering med hjälp av Azure klassiska portal</span><span class="sxs-lookup"><span data-stu-id="6fa35-235">BizTalk Services: Provisioning Using Azure classic portal</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302280)
* [<span data-ttu-id="6fa35-236">BizTalk Services: Etablering av statusdiagram</span><span class="sxs-lookup"><span data-stu-id="6fa35-236">BizTalk Services: Provisioning Status Chart</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=329870)
* [<span data-ttu-id="6fa35-237">BizTalk Services: Flikarna Instrumentpanel, Övervakare och Skalning</span><span class="sxs-lookup"><span data-stu-id="6fa35-237">BizTalk Services: Dashboard, Monitor and Scale tabs</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302281)
* [<span data-ttu-id="6fa35-238">BizTalk Services: Begränsning</span><span class="sxs-lookup"><span data-stu-id="6fa35-238">BizTalk Services: Throttling</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302282)
* [<span data-ttu-id="6fa35-239">BizTalk Services: Utfärdarens namn och nyckel</span><span class="sxs-lookup"><span data-stu-id="6fa35-239">BizTalk Services: Issuer Name and Issuer Key</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=303941)
* [<span data-ttu-id="6fa35-240">Hur jag börja använda hello Azure BizTalk Services SDK</span><span class="sxs-lookup"><span data-stu-id="6fa35-240">How do I Start Using hello Azure BizTalk Services SDK</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302335)

[BackupStatus]: ./media/biztalk-backup-restore/status-last-backup.png
[Restore]: ./media/biztalk-backup-restore/restore-ui.png
[AutomaticBU]: ./media/biztalk-backup-restore/AutomaticBU.png
[RestoreBizTalkService]: ./media/biztalk-backup-restore/RestoreBizTalkServiceWindow.png

