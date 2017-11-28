---
title: "Azure AD Connect-synkronisering: driftåtgärder och saker | Microsoft Docs"
description: "Det här avsnittet beskrivs åtgärder för Azure AD Connect-synkronisering och hur tooprepare för användning av den här komponenten."
services: active-directory
documentationcenter: 
author: AndKjell
manager: femila
editor: 
ms.assetid: b29c1790-37a3-470f-ab69-3cee824d220d
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: e6b21262e0924785808888d66f08a04a56581edc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-operational-tasks-and-consideration"></a><span data-ttu-id="73dda-103">Azure AD Connect-synkronisering: driftåtgärder och beräkningen</span><span class="sxs-lookup"><span data-stu-id="73dda-103">Azure AD Connect sync: Operational tasks and consideration</span></span>
<span data-ttu-id="73dda-104">hello syftet med det här avsnittet är toodescribe operativa uppgifter för Azure AD Connect-synkronisering.</span><span class="sxs-lookup"><span data-stu-id="73dda-104">hello objective of this topic is toodescribe operational tasks for Azure AD Connect sync.</span></span>

## <a name="staging-mode"></a><span data-ttu-id="73dda-105">Mellanlagringsläge</span><span class="sxs-lookup"><span data-stu-id="73dda-105">Staging mode</span></span>
<span data-ttu-id="73dda-106">Mellanlagringsläge kan användas för flera olika scenarier, inklusive:</span><span class="sxs-lookup"><span data-stu-id="73dda-106">Staging mode can be used for several scenarios, including:</span></span>

* <span data-ttu-id="73dda-107">Hög tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="73dda-107">High availability.</span></span>
* <span data-ttu-id="73dda-108">Testa och distribuera nya konfigurationsändringar.</span><span class="sxs-lookup"><span data-stu-id="73dda-108">Test and deploy new configuration changes.</span></span>
* <span data-ttu-id="73dda-109">Introducerar en ny server och inaktivera hello gamla.</span><span class="sxs-lookup"><span data-stu-id="73dda-109">Introduce a new server and decommission hello old.</span></span>

<span data-ttu-id="73dda-110">Med en server i fristående läge, kan du ändra ändringar toohello konfiguration och förhandsgranska hello innan du aktivera hello-server.</span><span class="sxs-lookup"><span data-stu-id="73dda-110">With a server in staging mode, you can make changes toohello configuration and preview hello changes before you make hello server active.</span></span> <span data-ttu-id="73dda-111">Du kan också toorun fullständig import och fullständig synkronisering tooverify att alla ändringar förväntas innan du gör dessa ändringar till din produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="73dda-111">It also allows you toorun full import and full synchronization tooverify that all changes are expected before you make these changes into your production environment.</span></span>

<span data-ttu-id="73dda-112">Under installationen kan du välja hello server toobe i **mellanlagringsläge**.</span><span class="sxs-lookup"><span data-stu-id="73dda-112">During installation, you can select hello server toobe in **staging mode**.</span></span> <span data-ttu-id="73dda-113">Den här åtgärden gör hello-server som är aktiva för import och synkronisering, men några exporter körs inte.</span><span class="sxs-lookup"><span data-stu-id="73dda-113">This action makes hello server active for import and synchronization, but it does not run any exports.</span></span> <span data-ttu-id="73dda-114">En server i fristående läge körs inte Lösenordssynkronisering och tillbakaskrivning av lösenord, även om dessa funktioner har valts under installationen.</span><span class="sxs-lookup"><span data-stu-id="73dda-114">A server in staging mode is not running password sync or password writeback, even if you selected these features during installation.</span></span> <span data-ttu-id="73dda-115">När du inaktiverar mellanlagringsläget hello server börjar exporten, aktiverar Lösenordssynkronisering och aktivera tillbakaskrivning av lösenord.</span><span class="sxs-lookup"><span data-stu-id="73dda-115">When you disable staging mode, hello server starts exporting, enables password sync, and enables password writeback.</span></span>

<span data-ttu-id="73dda-116">Du kan fortfarande framtvinga en export med hello synkronisering service manager.</span><span class="sxs-lookup"><span data-stu-id="73dda-116">You can still force an export by using hello synchronization service manager.</span></span>

<span data-ttu-id="73dda-117">En server i mellanlagringsläge fortsätter tooreceive ändringar från Active Directory och Azure AD.</span><span class="sxs-lookup"><span data-stu-id="73dda-117">A server in staging mode continues tooreceive changes from Active Directory and Azure AD.</span></span> <span data-ttu-id="73dda-118">Den har alltid en kopia av hello senaste ändringarna och kan snabbt ta över hello ansvar för en annan server.</span><span class="sxs-lookup"><span data-stu-id="73dda-118">It always has a copy of hello latest changes and can very fast take over hello responsibilities of another server.</span></span> <span data-ttu-id="73dda-119">Om du gör ändringar tooyour primära konfigurationsservern är ditt ansvar toomake hello samma ändringar toohello server i mellanlagringsläge.</span><span class="sxs-lookup"><span data-stu-id="73dda-119">If you make configuration changes tooyour primary server, it is your responsibility toomake hello same changes toohello server in staging mode.</span></span>

<span data-ttu-id="73dda-120">Hello mellanlagringsläge är olika för de med kunskap om äldre sync-tekniker, eftersom hello-servern har en egen SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="73dda-120">For those of you with knowledge of older sync technologies, hello staging mode is different since hello server has its own SQL database.</span></span> <span data-ttu-id="73dda-121">Den här arkitekturen kan hello mellanlagring läge server toobe finns i olika datacenter.</span><span class="sxs-lookup"><span data-stu-id="73dda-121">This architecture allows hello staging mode server toobe located in a different datacenter.</span></span>

### <a name="verify-hello-configuration-of-a-server"></a><span data-ttu-id="73dda-122">Kontrollera hello konfigurationen för en server</span><span class="sxs-lookup"><span data-stu-id="73dda-122">Verify hello configuration of a server</span></span>
<span data-ttu-id="73dda-123">tooapply den här metoden gör du följande:</span><span class="sxs-lookup"><span data-stu-id="73dda-123">tooapply this method, follow these steps:</span></span>

1. [<span data-ttu-id="73dda-124">Förbereda</span><span class="sxs-lookup"><span data-stu-id="73dda-124">Prepare</span></span>](#prepare)
2. [<span data-ttu-id="73dda-125">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="73dda-125">Configuration</span></span>](#configuration)
3. [<span data-ttu-id="73dda-126">Importera och synkronisera</span><span class="sxs-lookup"><span data-stu-id="73dda-126">Import and Synchronize</span></span>](#import-and-synchronize)
4. [<span data-ttu-id="73dda-127">Kontrollera</span><span class="sxs-lookup"><span data-stu-id="73dda-127">Verify</span></span>](#verify)
5. [<span data-ttu-id="73dda-128">Växeln active server</span><span class="sxs-lookup"><span data-stu-id="73dda-128">Switch active server</span></span>](#switch-active-server)

#### <a name="prepare"></a><span data-ttu-id="73dda-129">Förbereda</span><span class="sxs-lookup"><span data-stu-id="73dda-129">Prepare</span></span>
1. <span data-ttu-id="73dda-130">Installera Azure AD Connect, Välj **mellanlagringsläge**, och avmarkera **starta synkroniseringen** på hello sista sidan i guiden för installation av hello.</span><span class="sxs-lookup"><span data-stu-id="73dda-130">Install Azure AD Connect, select **staging mode**, and unselect **start synchronization** on hello last page in hello installation wizard.</span></span> <span data-ttu-id="73dda-131">Det här läget kan du toorun hello Synkroniseringsmotorn manuellt.</span><span class="sxs-lookup"><span data-stu-id="73dda-131">This mode allows you toorun hello sync engine manually.</span></span>
   <span data-ttu-id="73dda-132">![ReadyToConfigure](./media/active-directory-aadconnectsync-operations/readytoconfigure.png)</span><span class="sxs-lookup"><span data-stu-id="73dda-132">![ReadyToConfigure](./media/active-directory-aadconnectsync-operations/readytoconfigure.png)</span></span>
2. <span data-ttu-id="73dda-133">Logga ut/logga in och starta välja från hello **synkroniseringstjänsten**.</span><span class="sxs-lookup"><span data-stu-id="73dda-133">Sign off/sign in and from hello start menu select **Synchronization Service**.</span></span>

#### <a name="configuration"></a><span data-ttu-id="73dda-134">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="73dda-134">Configuration</span></span>
<span data-ttu-id="73dda-135">Om du har gjort ändringar toohello primära servern och vill toocompare hello konfiguration med hello mellanlagring server, använder du [Azure AD Connect configuration Dokumenteraren](https://github.com/Microsoft/AADConnectConfigDocumenter).</span><span class="sxs-lookup"><span data-stu-id="73dda-135">If you have made custom changes toohello primary server and want toocompare hello configuration with hello staging server, then use [Azure AD Connect configuration documenter](https://github.com/Microsoft/AADConnectConfigDocumenter).</span></span>

#### <a name="import-and-synchronize"></a><span data-ttu-id="73dda-136">Importera och synkronisera</span><span class="sxs-lookup"><span data-stu-id="73dda-136">Import and Synchronize</span></span>
1. <span data-ttu-id="73dda-137">Välj **kopplingar**, och välj hello första anslutningstjänsten med hello typen **Active Directory Domain Services**.</span><span class="sxs-lookup"><span data-stu-id="73dda-137">Select **Connectors**, and select hello first Connector with hello type **Active Directory Domain Services**.</span></span> <span data-ttu-id="73dda-138">Klicka på **kör**väljer **fullständig import**, och **OK**.</span><span class="sxs-lookup"><span data-stu-id="73dda-138">Click **Run**, select **Full import**, and **OK**.</span></span> <span data-ttu-id="73dda-139">Utföra de här stegen för alla kopplingar av den här typen.</span><span class="sxs-lookup"><span data-stu-id="73dda-139">Do these steps for all Connectors of this type.</span></span>
2. <span data-ttu-id="73dda-140">Välj hello Connector med typen **Azure Active Directory (Microsoft)**.</span><span class="sxs-lookup"><span data-stu-id="73dda-140">Select hello Connector with type **Azure Active Directory (Microsoft)**.</span></span> <span data-ttu-id="73dda-141">Klicka på **kör**väljer **fullständig import**, och **OK**.</span><span class="sxs-lookup"><span data-stu-id="73dda-141">Click **Run**, select **Full import**, and **OK**.</span></span>
3. <span data-ttu-id="73dda-142">Kontrollera att fliken hello kopplingar är markerad.</span><span class="sxs-lookup"><span data-stu-id="73dda-142">Make sure hello tab Connectors is still selected.</span></span> <span data-ttu-id="73dda-143">För varje koppling med typen **Active Directory Domain Services**, klickar du på **kör**väljer **Deltasynkronisering**, och **OK**.</span><span class="sxs-lookup"><span data-stu-id="73dda-143">For each Connector with type **Active Directory Domain Services**, click **Run**, select **Delta Synchronization**, and **OK**.</span></span>
4. <span data-ttu-id="73dda-144">Välj hello Connector med typen **Azure Active Directory (Microsoft)**.</span><span class="sxs-lookup"><span data-stu-id="73dda-144">Select hello Connector with type **Azure Active Directory (Microsoft)**.</span></span> <span data-ttu-id="73dda-145">Klicka på **kör**väljer **Deltasynkronisering**, och **OK**.</span><span class="sxs-lookup"><span data-stu-id="73dda-145">Click **Run**, select **Delta Synchronization**, and **OK**.</span></span>

<span data-ttu-id="73dda-146">Du har nu stegvis export ändras tooAzure AD och lokala AD (om du använder Exchange-hybridinstallation).</span><span class="sxs-lookup"><span data-stu-id="73dda-146">You have now staged export changes tooAzure AD and on-premises AD (if you are using Exchange hybrid deployment).</span></span> <span data-ttu-id="73dda-147">hello nästa steg kan du tooinspect vad handlar om toochange innan du börjar faktiskt hello exportera toohello kataloger.</span><span class="sxs-lookup"><span data-stu-id="73dda-147">hello next steps allow you tooinspect what is about toochange before you actually start hello export toohello directories.</span></span>

#### <a name="verify"></a><span data-ttu-id="73dda-148">Verifiera</span><span class="sxs-lookup"><span data-stu-id="73dda-148">Verify</span></span>
1. <span data-ttu-id="73dda-149">Starta en kommandotolk och gå för`%ProgramFiles%\Microsoft Azure AD Sync\bin`</span><span class="sxs-lookup"><span data-stu-id="73dda-149">Start a cmd prompt and go too`%ProgramFiles%\Microsoft Azure AD Sync\bin`</span></span>
2. <span data-ttu-id="73dda-150">Kör: `csexport "Name of Connector" %temp%\export.xml /f:x` hello namnet på hello Connector finns i synkroniseringstjänsten.</span><span class="sxs-lookup"><span data-stu-id="73dda-150">Run: `csexport "Name of Connector" %temp%\export.xml /f:x` hello name of hello Connector can be found in Synchronization Service.</span></span> <span data-ttu-id="73dda-151">Den har en liknande too"contoso.com namn – AAD” för Azure AD.</span><span class="sxs-lookup"><span data-stu-id="73dda-151">It has a name similar too"contoso.com – AAD" for Azure AD.</span></span>
3. <span data-ttu-id="73dda-152">Kopiera hello PowerShell-skriptet från avsnittet hello [CSAnalyzer](#appendix-csanalyzer) tooa fil med namnet `csanalyzer.ps1`.</span><span class="sxs-lookup"><span data-stu-id="73dda-152">Copy hello PowerShell script from hello section [CSAnalyzer](#appendix-csanalyzer) tooa file named `csanalyzer.ps1`.</span></span>
4. <span data-ttu-id="73dda-153">Öppna ett PowerShell-fönster och bläddra toohello mappen där du skapade hello PowerShell-skript.</span><span class="sxs-lookup"><span data-stu-id="73dda-153">Open a PowerShell window and browse toohello folder where you created hello PowerShell script.</span></span>
5. <span data-ttu-id="73dda-154">Kör: `.\csanalyzer.ps1 -xmltoimport %temp%\export.xml`.</span><span class="sxs-lookup"><span data-stu-id="73dda-154">Run: `.\csanalyzer.ps1 -xmltoimport %temp%\export.xml`.</span></span>
6. <span data-ttu-id="73dda-155">Nu har du en fil med namnet **processedusers1.csv** som kan granskas i Microsoft Excel.</span><span class="sxs-lookup"><span data-stu-id="73dda-155">You now have a file named **processedusers1.csv** that can be examined in Microsoft Excel.</span></span> <span data-ttu-id="73dda-156">Alla ändringar som mellanlagras toobe exporteras tooAzure AD finns i den här filen.</span><span class="sxs-lookup"><span data-stu-id="73dda-156">All changes staged toobe exported tooAzure AD are found in this file.</span></span>
7. <span data-ttu-id="73dda-157">Gör nödvändiga ändringar toohello data eller konfigurationer och kör stegen igen (importera och synkronisera och kontrollera) tills hello ändringar som är i färd toobe exporteras förväntas.</span><span class="sxs-lookup"><span data-stu-id="73dda-157">Make necessary changes toohello data or configuration and run these steps again (Import and Synchronize and Verify) until hello changes that are about toobe exported are expected.</span></span>

#### <a name="switch-active-server"></a><span data-ttu-id="73dda-158">Växeln active server</span><span class="sxs-lookup"><span data-stu-id="73dda-158">Switch active server</span></span>
1. <span data-ttu-id="73dda-159">Inaktivera hello server (DirSync/FIM/Azure AD Sync) så att den inte exporterar tooAzure AD eller Ställ in den i mellanlagringsläge (Azure AD Connect) på hello aktiva servern.</span><span class="sxs-lookup"><span data-stu-id="73dda-159">On hello currently active server, either turn off hello server (DirSync/FIM/Azure AD Sync) so it is not exporting tooAzure AD or set it in staging mode (Azure AD Connect).</span></span>
2. <span data-ttu-id="73dda-160">Köra installationsguiden för hello på hello server i **mellanlagringsläge** och inaktivera **mellanlagringsläge**.</span><span class="sxs-lookup"><span data-stu-id="73dda-160">Run hello installation wizard on hello server in **staging mode** and disable **staging mode**.</span></span>
   <span data-ttu-id="73dda-161">![ReadyToConfigure](./media/active-directory-aadconnectsync-operations/additionaltasks.png)</span><span class="sxs-lookup"><span data-stu-id="73dda-161">![ReadyToConfigure](./media/active-directory-aadconnectsync-operations/additionaltasks.png)</span></span>

## <a name="disaster-recovery"></a><span data-ttu-id="73dda-162">Haveriberedskap</span><span class="sxs-lookup"><span data-stu-id="73dda-162">Disaster recovery</span></span>
<span data-ttu-id="73dda-163">En del av hello implementering designen är tooplan för vilka toodo om det är en katastrof där du förlorar hello synkroniseringsserver.</span><span class="sxs-lookup"><span data-stu-id="73dda-163">Part of hello implementation design is tooplan for what toodo in case there is a disaster where you lose hello sync server.</span></span> <span data-ttu-id="73dda-164">Det finns olika modeller toouse och vilka en toouse beror på flera faktorer, inklusive:</span><span class="sxs-lookup"><span data-stu-id="73dda-164">There are different models toouse and which one toouse depends on several factors including:</span></span>

* <span data-ttu-id="73dda-165">Vad är din tolerans för ändringar som inte kan se tooobjects i Azure AD under hello driftstopp?</span><span class="sxs-lookup"><span data-stu-id="73dda-165">What is your tolerance for not being able make changes tooobjects in Azure AD during hello downtime?</span></span>
* <span data-ttu-id="73dda-166">Om du använder Lösenordssynkronisering hello användare accepterar de har toouse hello gamla lösenord i Azure AD om de ändra lokalt?</span><span class="sxs-lookup"><span data-stu-id="73dda-166">If you use password synchronization, do hello users accept that they have toouse hello old password in Azure AD in case they change it on-premises?</span></span>
* <span data-ttu-id="73dda-167">Har du ett beroende i realtid åtgärder, till exempel tillbakaskrivning av lösenord?</span><span class="sxs-lookup"><span data-stu-id="73dda-167">Do you have a dependency on real-time operations, such as password writeback?</span></span>

<span data-ttu-id="73dda-168">Beroende på hello svar toothese frågor och organisationens princip, kan en hello följande strategier implementeras:</span><span class="sxs-lookup"><span data-stu-id="73dda-168">Depending on hello answers toothese questions and your organization’s policy, one of hello following strategies can be implemented:</span></span>

* <span data-ttu-id="73dda-169">Återskapa vid behov.</span><span class="sxs-lookup"><span data-stu-id="73dda-169">Rebuild when needed.</span></span>
* <span data-ttu-id="73dda-170">En ledig belägna standby-servern, kallas även **mellanlagringsläge**.</span><span class="sxs-lookup"><span data-stu-id="73dda-170">Have a spare standby server, known as **staging mode**.</span></span>
* <span data-ttu-id="73dda-171">Använda virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="73dda-171">Use virtual machines.</span></span>

<span data-ttu-id="73dda-172">Om du inte använder hello inbyggda SQL Express-databas, så du bör också granska hello [hög tillgänglighet för SQL](#sql-high-availability) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="73dda-172">If you do not use hello built-in SQL Express database, then you should also review hello [SQL High Availability](#sql-high-availability) section.</span></span>

### <a name="rebuild-when-needed"></a><span data-ttu-id="73dda-173">Återskapa vid behov</span><span class="sxs-lookup"><span data-stu-id="73dda-173">Rebuild when needed</span></span>
<span data-ttu-id="73dda-174">En lönsam strategi är tooplan för återskapning av en server när det behövs.</span><span class="sxs-lookup"><span data-stu-id="73dda-174">A viable strategy is tooplan for a server rebuild when needed.</span></span> <span data-ttu-id="73dda-175">Vanligtvis installerar hello Synkroniseringsmotorn och hello inledande import och synkronisering kan slutföras inom några timmar.</span><span class="sxs-lookup"><span data-stu-id="73dda-175">Usually, installing hello sync engine and do hello initial import and sync can be completed within a few hours.</span></span> <span data-ttu-id="73dda-176">Om det inte en extra server, behöver du möjliga tootemporarily en domain controller toohost hello Synkroniseringsmotorn.</span><span class="sxs-lookup"><span data-stu-id="73dda-176">If there isn’t a spare server available, it is possible tootemporarily use a domain controller toohost hello sync engine.</span></span>

<span data-ttu-id="73dda-177">hello motorn synkroniseringsserver lagrar inte någon status om hello objekt så hello databasen kan byggas från hello data i Active Directory och Azure AD.</span><span class="sxs-lookup"><span data-stu-id="73dda-177">hello sync engine server does not store any state about hello objects so hello database can be rebuilt from hello data in Active Directory and Azure AD.</span></span> <span data-ttu-id="73dda-178">Hej **sourceAnchor** attributet är används toojoin hello objekt från lokala och hello molnet.</span><span class="sxs-lookup"><span data-stu-id="73dda-178">hello **sourceAnchor** attribute is used toojoin hello objects from on-premises and hello cloud.</span></span> <span data-ttu-id="73dda-179">Om du återskapar hello server med befintliga objekt lokala och hello molnet, och sedan hello Synkroniseringsmotorn matchar de här objekten tillsammans igen på ominstallation.</span><span class="sxs-lookup"><span data-stu-id="73dda-179">If you rebuild hello server with existing objects on-premises and hello cloud, then hello sync engine matches those objects together again on reinstallation.</span></span> <span data-ttu-id="73dda-180">hello saker du behöver toodocument och spara är hello konfigurationsändringar toohello server, t.ex filtrering och Synkroniseringsregler.</span><span class="sxs-lookup"><span data-stu-id="73dda-180">hello things you need toodocument and save are hello configuration changes made toohello server, such as filtering and synchronization rules.</span></span> <span data-ttu-id="73dda-181">Dessa anpassade konfigurationer måste återställas innan du startar synkroniseringen.</span><span class="sxs-lookup"><span data-stu-id="73dda-181">These custom configurations must be reapplied before you start synchronizing.</span></span>

### <a name="have-a-spare-standby-server---staging-mode"></a><span data-ttu-id="73dda-182">Har en ledig standby server - mellanlagringsläge</span><span class="sxs-lookup"><span data-stu-id="73dda-182">Have a spare standby server - staging mode</span></span>
<span data-ttu-id="73dda-183">Om du har en mer komplex miljö rekommenderas med en eller flera reservservrar.</span><span class="sxs-lookup"><span data-stu-id="73dda-183">If you have a more complex environment, then having one or more standby servers is recommended.</span></span> <span data-ttu-id="73dda-184">Under installationen, kan du aktivera en server toobe i **mellanlagringsläge**.</span><span class="sxs-lookup"><span data-stu-id="73dda-184">During installation, you can enable a server toobe in **staging mode**.</span></span>

<span data-ttu-id="73dda-185">Mer information finns i [mellanlagringsläge](#staging-mode).</span><span class="sxs-lookup"><span data-stu-id="73dda-185">For more information, see [staging mode](#staging-mode).</span></span>

### <a name="use-virtual-machines"></a><span data-ttu-id="73dda-186">Använda virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="73dda-186">Use virtual machines</span></span>
<span data-ttu-id="73dda-187">En gemensam och stöds metod är toorun hello Synkroniseringsmotorn i en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="73dda-187">A common and supported method is toorun hello sync engine in a virtual machine.</span></span> <span data-ttu-id="73dda-188">Om hello värden har ett problem, kan hello avbildningen med hello synkroniseringsserver motorn vara migrerade tooanother server.</span><span class="sxs-lookup"><span data-stu-id="73dda-188">In case hello host has an issue, hello image with hello sync engine server can be migrated tooanother server.</span></span>

### <a name="sql-high-availability"></a><span data-ttu-id="73dda-189">Hög tillgänglighet för SQL</span><span class="sxs-lookup"><span data-stu-id="73dda-189">SQL High Availability</span></span>
<span data-ttu-id="73dda-190">Om du inte använder hello SQL Server Express som medföljer Azure AD Connect, bör sedan hög tillgänglighet för SQL Server också övervägas.</span><span class="sxs-lookup"><span data-stu-id="73dda-190">If you are not using hello SQL Server Express that comes with Azure AD Connect, then high availability for SQL Server should also be considered.</span></span> <span data-ttu-id="73dda-191">hello lösningar för hög tillgänglighet som stöds är SQL-kluster och AOA (alltid på Tillgänglighetsgrupper).</span><span class="sxs-lookup"><span data-stu-id="73dda-191">hello high availability solutions supported include SQL clustering and AOA (Always On Availability Groups).</span></span> <span data-ttu-id="73dda-192">Stöds inte lösningar omfattar spegling.</span><span class="sxs-lookup"><span data-stu-id="73dda-192">Unsupported solutions include mirroring.</span></span>

<span data-ttu-id="73dda-193">Stöd för SQL AOA har lagts till tooAzure AD Connect i version 1.1.524.0.</span><span class="sxs-lookup"><span data-stu-id="73dda-193">Support for SQL AOA was added tooAzure AD Connect in version 1.1.524.0.</span></span> <span data-ttu-id="73dda-194">Du måste aktivera SQL AOA innan du installerar Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="73dda-194">You must enable SQL AOA before installing Azure AD Connect.</span></span> <span data-ttu-id="73dda-195">Under installationen av identifierar Azure AD Connect om angivna hello SQL-instansen är aktiverat för SQL AOA eller inte.</span><span class="sxs-lookup"><span data-stu-id="73dda-195">During installation, Azure AD Connect detects whether hello SQL instance provided is enabled for SQL AOA or not.</span></span> <span data-ttu-id="73dda-196">Om SQL AOA är aktiverat räknat Azure AD Connect ytterligare ut om SQL AOA är konfigurerade toouse synkron eller asynkron replikeringen.</span><span class="sxs-lookup"><span data-stu-id="73dda-196">If SQL AOA is enabled, Azure AD Connect further figures out if SQL AOA is configured toouse synchronous replication or asynchronous replication.</span></span> <span data-ttu-id="73dda-197">När du ställer in hello tillgänglighetsgruppens lyssnare, rekommenderas att du ställer in hello RegisterAllProvidersIP egenskapen too0.</span><span class="sxs-lookup"><span data-stu-id="73dda-197">When setting up hello Availability Group Listener, it is recommended that you set hello RegisterAllProvidersIP property too0.</span></span> <span data-ttu-id="73dda-198">Detta beror på att Azure AD Connect används för närvarande SQL Native Client tooconnect tooSQL och SQL Native Client stöder inte hello användning av MultiSubNetFailover-egenskapen.</span><span class="sxs-lookup"><span data-stu-id="73dda-198">This is because Azure AD Connect currently uses SQL Native Client tooconnect tooSQL and SQL Native Client does not support hello use of MultiSubNetFailover property.</span></span>

## <a name="appendix-csanalyzer"></a><span data-ttu-id="73dda-199">Bilaga CSAnalyzer</span><span class="sxs-lookup"><span data-stu-id="73dda-199">Appendix CSAnalyzer</span></span>
<span data-ttu-id="73dda-200">Avsnittet hello [Kontrollera](#verify) på hur toouse det här skriptet.</span><span class="sxs-lookup"><span data-stu-id="73dda-200">See hello section [verify](#verify) on how toouse this script.</span></span>

```
Param(
    [Parameter(Mandatory=$true, HelpMessage="Must be a file generated using csexport 'Name of Connector' export.xml /f:x)")]
    [string]$xmltoimport="%temp%\exportedStage1a.xml",
    [Parameter(Mandatory=$false, HelpMessage="Maximum number of users per output file")][int]$batchsize=1000,
    [Parameter(Mandatory=$false, HelpMessage="Show console output")][bool]$showOutput=$false
)

#LINQ isn't loaded automatically, so force it
[Reflection.Assembly]::Load("System.Xml.Linq, Version=3.5.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089") | Out-Null

[int]$count=1
[int]$outputfilecount=1
[array]$objOutputUsers=@()

#XML must be generated using "csexport "Name of Connector" export.xml /f:x"
write-host "Importing XML" -ForegroundColor Yellow

#XmlReader.Create won't properly resolve hello file location,
#so expand and then resolve it
$resolvedXMLtoimport=Resolve-Path -Path ([Environment]::ExpandEnvironmentVariables($xmltoimport))

#use an XmlReader toodeal with even large files
$result=$reader = [System.Xml.XmlReader]::Create($resolvedXMLtoimport) 
$result=$reader.ReadToDescendant('cs-object')
do 
{
    #create hello object placeholder
    #adding them up here means we can enforce consistency
    $objOutputUser=New-Object psobject
    Add-Member -InputObject $objOutputUser -MemberType NoteProperty -Name ID -Value ""
    Add-Member -InputObject $objOutputUser -MemberType NoteProperty -Name Type -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name DN -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name operation -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name UPN -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name displayName -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name sourceAnchor -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name alias -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name primarySMTP -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name onPremisesSamAccountName -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name mail -Value ""

    $user = [System.Xml.Linq.XElement]::ReadFrom($reader)
    if ($showOutput) {Write-Host Found an exported object... -ForegroundColor Green}

    #object id
    $outID=$user.Attribute('id').Value
    if ($showOutput) {Write-Host ID: $outID}
    $objOutputUser.ID=$outID

    #object type
    $outType=$user.Attribute('object-type').Value
    if ($showOutput) {Write-Host Type: $outType}
    $objOutputUser.Type=$outType

    #dn
    $outDN= $user.Element('unapplied-export').Element('delta').Attribute('dn').Value
    if ($showOutput) {Write-Host DN: $outDN}
    $objOutputUser.DN=$outDN

    #operation
    $outOperation= $user.Element('unapplied-export').Element('delta').Attribute('operation').Value
    if ($showOutput) {Write-Host Operation: $outOperation}
    $objOutputUser.operation=$outOperation

    #now that we have hello basics, go get hello details

    foreach ($attr in $user.Element('unapplied-export-hologram').Element('entry').Elements("attr"))
    {
        $attrvalue=$attr.Attribute('name').Value
        $internalvalue= $attr.Element('value').Value

        switch ($attrvalue)
        {
            "userPrincipalName"
            {
                if ($showOutput) {Write-Host UPN: $internalvalue}
                $objOutputUser.UPN=$internalvalue
            }
            "displayName"
            {
                if ($showOutput) {Write-Host displayName: $internalvalue}
                $objOutputUser.displayName=$internalvalue
            }
            "sourceAnchor"
            {
                if ($showOutput) {Write-Host sourceAnchor: $internalvalue}
                $objOutputUser.sourceAnchor=$internalvalue
            }
            "alias"
            {
                if ($showOutput) {Write-Host alias: $internalvalue}
                $objOutputUser.alias=$internalvalue
            }
            "proxyAddresses"
            {
                if ($showOutput) {Write-Host primarySMTP: ($internalvalue -replace "SMTP:","")}
                $objOutputUser.primarySMTP=$internalvalue -replace "SMTP:",""
            }
        }
    }

    $objOutputUsers += $objOutputUser

    Write-Progress -activity "Processing ${xmltoimport} in batches of ${batchsize}" -status "Batch ${outputfilecount}: " -percentComplete (($objOutputUsers.Count / $batchsize) * 100)

    #every so often, dump hello processed users in case we blow up somewhere
    if ($count % $batchsize -eq 0)
    {
        Write-Host Hit hello maximum users processed without completion... -ForegroundColor Yellow

        #export hello collection of users as as CSV
        Write-Host Writing processedusers${outputfilecount}.csv -ForegroundColor Yellow
        $objOutputUsers | Export-Csv -path processedusers${outputfilecount}.csv -NoTypeInformation

        #increment hello output file counter
        $outputfilecount+=1

        #reset hello collection and hello user counter
        $objOutputUsers = $null
        $count=0
    }

    $count+=1

    #need toobail out of hello loop if no more users tooprocess
    if ($reader.NodeType -eq [System.Xml.XmlNodeType]::EndElement)
    {
        break
    }

} while ($reader.Read)

#need toowrite out any users that didn't get picked up in a batch of 1000
#export hello collection of users as as CSV
Write-Host Writing processedusers${outputfilecount}.csv -ForegroundColor Yellow
$objOutputUsers | Export-Csv -path processedusers${outputfilecount}.csv -NoTypeInformation
```

## <a name="next-steps"></a><span data-ttu-id="73dda-201">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="73dda-201">Next steps</span></span>
<span data-ttu-id="73dda-202">**Översiktsavsnitt**</span><span class="sxs-lookup"><span data-stu-id="73dda-202">**Overview topics**</span></span>  

* [<span data-ttu-id="73dda-203">Azure AD Connect-synkronisering: Förstå och anpassa synkronisering</span><span class="sxs-lookup"><span data-stu-id="73dda-203">Azure AD Connect sync: Understand and customize synchronization</span></span>](active-directory-aadconnectsync-whatis.md)  
* [<span data-ttu-id="73dda-204">Integrera dina lokala identiteter med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="73dda-204">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)  
