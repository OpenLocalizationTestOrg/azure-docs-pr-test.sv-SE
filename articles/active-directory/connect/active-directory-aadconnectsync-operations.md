---
title: "Azure AD Connect-synkronisering: driftåtgärder och saker | Microsoft Docs"
description: "Det här avsnittet beskriver åtgärder för Azure AD Connect-synkronisering och hur du förbereder för användning av den här komponenten."
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
ms.openlocfilehash: b7583a1556bb1113f349a78890768451e39c6878
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="azure-ad-connect-sync-operational-tasks-and-consideration"></a><span data-ttu-id="5c448-103">Azure AD Connect-synkronisering: driftåtgärder och beräkningen</span><span class="sxs-lookup"><span data-stu-id="5c448-103">Azure AD Connect sync: Operational tasks and consideration</span></span>
<span data-ttu-id="5c448-104">Syftet med det här avsnittet är att beskriva operativa uppgifter för Azure AD Connect-synkronisering.</span><span class="sxs-lookup"><span data-stu-id="5c448-104">The objective of this topic is to describe operational tasks for Azure AD Connect sync.</span></span>

## <a name="staging-mode"></a><span data-ttu-id="5c448-105">Mellanlagringsläge</span><span class="sxs-lookup"><span data-stu-id="5c448-105">Staging mode</span></span>
<span data-ttu-id="5c448-106">Mellanlagringsläge kan användas för flera olika scenarier, inklusive:</span><span class="sxs-lookup"><span data-stu-id="5c448-106">Staging mode can be used for several scenarios, including:</span></span>

* <span data-ttu-id="5c448-107">Hög tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="5c448-107">High availability.</span></span>
* <span data-ttu-id="5c448-108">Testa och distribuera nya konfigurationsändringar.</span><span class="sxs-lookup"><span data-stu-id="5c448-108">Test and deploy new configuration changes.</span></span>
* <span data-ttu-id="5c448-109">Introducerar en ny server och inaktivera gammalt.</span><span class="sxs-lookup"><span data-stu-id="5c448-109">Introduce a new server and decommission the old.</span></span>

<span data-ttu-id="5c448-110">Du kan göra ändringar i konfigurationen och förhandsgranska ändringarna innan du gör att servern är aktiv med en server i mellanlagringsläge.</span><span class="sxs-lookup"><span data-stu-id="5c448-110">With a server in staging mode, you can make changes to the configuration and preview the changes before you make the server active.</span></span> <span data-ttu-id="5c448-111">Du kan också köra fullständig import och fullständig synkronisering för att kontrollera att alla ändringar som förväntat innan du gör dessa ändringar till din produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="5c448-111">It also allows you to run full import and full synchronization to verify that all changes are expected before you make these changes into your production environment.</span></span>

<span data-ttu-id="5c448-112">Under installationen, väljer du den server som ska vara i **mellanlagringsläge**.</span><span class="sxs-lookup"><span data-stu-id="5c448-112">During installation, you can select the server to be in **staging mode**.</span></span> <span data-ttu-id="5c448-113">Den här åtgärden gör servern aktivt för import och synkronisering, men några exporter körs inte.</span><span class="sxs-lookup"><span data-stu-id="5c448-113">This action makes the server active for import and synchronization, but it does not run any exports.</span></span> <span data-ttu-id="5c448-114">En server i fristående läge körs inte Lösenordssynkronisering och tillbakaskrivning av lösenord, även om dessa funktioner har valts under installationen.</span><span class="sxs-lookup"><span data-stu-id="5c448-114">A server in staging mode is not running password sync or password writeback, even if you selected these features during installation.</span></span> <span data-ttu-id="5c448-115">När du inaktiverar mellanlagringsläget börjar exporten, aktiverar Lösenordssynkronisering och aktiverar tillbakaskrivning av lösenord.</span><span class="sxs-lookup"><span data-stu-id="5c448-115">When you disable staging mode, the server starts exporting, enables password sync, and enables password writeback.</span></span>

<span data-ttu-id="5c448-116">Du kan fortfarande framtvinga en export med hjälp av hanteraren för synkroniseringstjänsten.</span><span class="sxs-lookup"><span data-stu-id="5c448-116">You can still force an export by using the synchronization service manager.</span></span>

<span data-ttu-id="5c448-117">En server i mellanlagringsläge fortsätter att ta emot ändringar från Active Directory och Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5c448-117">A server in staging mode continues to receive changes from Active Directory and Azure AD.</span></span> <span data-ttu-id="5c448-118">Det har alltid en kopia av de senaste ändringarna och kan snabbt ta över ansvar för en annan server.</span><span class="sxs-lookup"><span data-stu-id="5c448-118">It always has a copy of the latest changes and can very fast take over the responsibilities of another server.</span></span> <span data-ttu-id="5c448-119">Om du gör konfigurationsändringar till den primära servern är ditt ansvar att göra samma ändringar till servern i mellanlagringsläge.</span><span class="sxs-lookup"><span data-stu-id="5c448-119">If you make configuration changes to your primary server, it is your responsibility to make the same changes to the server in staging mode.</span></span>

<span data-ttu-id="5c448-120">I mellanlagringsläge skiljer sig för de med kunskap om äldre sync-tekniker, eftersom servern har en egen SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="5c448-120">For those of you with knowledge of older sync technologies, the staging mode is different since the server has its own SQL database.</span></span> <span data-ttu-id="5c448-121">Den här arkitekturen kan testservern läge att ska finnas i olika datacenter.</span><span class="sxs-lookup"><span data-stu-id="5c448-121">This architecture allows the staging mode server to be located in a different datacenter.</span></span>

### <a name="verify-the-configuration-of-a-server"></a><span data-ttu-id="5c448-122">Kontrollera konfigurationen av en server</span><span class="sxs-lookup"><span data-stu-id="5c448-122">Verify the configuration of a server</span></span>
<span data-ttu-id="5c448-123">Följ dessa steg om du vill använda den här metoden:</span><span class="sxs-lookup"><span data-stu-id="5c448-123">To apply this method, follow these steps:</span></span>

1. [<span data-ttu-id="5c448-124">Förbereda</span><span class="sxs-lookup"><span data-stu-id="5c448-124">Prepare</span></span>](#prepare)
2. [<span data-ttu-id="5c448-125">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="5c448-125">Configuration</span></span>](#configuration)
3. [<span data-ttu-id="5c448-126">Importera och synkronisera</span><span class="sxs-lookup"><span data-stu-id="5c448-126">Import and Synchronize</span></span>](#import-and-synchronize)
4. [<span data-ttu-id="5c448-127">Kontrollera</span><span class="sxs-lookup"><span data-stu-id="5c448-127">Verify</span></span>](#verify)
5. [<span data-ttu-id="5c448-128">Växeln active server</span><span class="sxs-lookup"><span data-stu-id="5c448-128">Switch active server</span></span>](#switch-active-server)

#### <a name="prepare"></a><span data-ttu-id="5c448-129">Förbereda</span><span class="sxs-lookup"><span data-stu-id="5c448-129">Prepare</span></span>
1. <span data-ttu-id="5c448-130">Installera Azure AD Connect, Välj **mellanlagringsläge**, och avmarkera **starta synkroniseringen** på den sista sidan i installationsguiden.</span><span class="sxs-lookup"><span data-stu-id="5c448-130">Install Azure AD Connect, select **staging mode**, and unselect **start synchronization** on the last page in the installation wizard.</span></span> <span data-ttu-id="5c448-131">Det här läget kan du köra Synkroniseringsmotorn manuellt.</span><span class="sxs-lookup"><span data-stu-id="5c448-131">This mode allows you to run the sync engine manually.</span></span>
   <span data-ttu-id="5c448-132">![ReadyToConfigure](./media/active-directory-aadconnectsync-operations/readytoconfigure.png)</span><span class="sxs-lookup"><span data-stu-id="5c448-132">![ReadyToConfigure](./media/active-directory-aadconnectsync-operations/readytoconfigure.png)</span></span>
2. <span data-ttu-id="5c448-133">Logga ut/logga i och från start-menyn väljer **synkroniseringstjänsten**.</span><span class="sxs-lookup"><span data-stu-id="5c448-133">Sign off/sign in and from the start menu select **Synchronization Service**.</span></span>

#### <a name="configuration"></a><span data-ttu-id="5c448-134">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="5c448-134">Configuration</span></span>
<span data-ttu-id="5c448-135">Om du har gjort egna ändringar till den primära servern och vill jämföra konfigurationen med fristående server, använder du [Azure AD Connect configuration Dokumenteraren](https://github.com/Microsoft/AADConnectConfigDocumenter).</span><span class="sxs-lookup"><span data-stu-id="5c448-135">If you have made custom changes to the primary server and want to compare the configuration with the staging server, then use [Azure AD Connect configuration documenter](https://github.com/Microsoft/AADConnectConfigDocumenter).</span></span>

#### <a name="import-and-synchronize"></a><span data-ttu-id="5c448-136">Importera och synkronisera</span><span class="sxs-lookup"><span data-stu-id="5c448-136">Import and Synchronize</span></span>
1. <span data-ttu-id="5c448-137">Välj **kopplingar**, och välj den första anslutningen med typen **Active Directory Domain Services**.</span><span class="sxs-lookup"><span data-stu-id="5c448-137">Select **Connectors**, and select the first Connector with the type **Active Directory Domain Services**.</span></span> <span data-ttu-id="5c448-138">Klicka på **kör**väljer **fullständig import**, och **OK**.</span><span class="sxs-lookup"><span data-stu-id="5c448-138">Click **Run**, select **Full import**, and **OK**.</span></span> <span data-ttu-id="5c448-139">Utföra de här stegen för alla kopplingar av den här typen.</span><span class="sxs-lookup"><span data-stu-id="5c448-139">Do these steps for all Connectors of this type.</span></span>
2. <span data-ttu-id="5c448-140">Välj kopplingen med typen **Azure Active Directory (Microsoft)**.</span><span class="sxs-lookup"><span data-stu-id="5c448-140">Select the Connector with type **Azure Active Directory (Microsoft)**.</span></span> <span data-ttu-id="5c448-141">Klicka på **kör**väljer **fullständig import**, och **OK**.</span><span class="sxs-lookup"><span data-stu-id="5c448-141">Click **Run**, select **Full import**, and **OK**.</span></span>
3. <span data-ttu-id="5c448-142">Kontrollera att fliken kopplingar fortfarande är markerad.</span><span class="sxs-lookup"><span data-stu-id="5c448-142">Make sure the tab Connectors is still selected.</span></span> <span data-ttu-id="5c448-143">För varje koppling med typen **Active Directory Domain Services**, klickar du på **kör**väljer **Deltasynkronisering**, och **OK**.</span><span class="sxs-lookup"><span data-stu-id="5c448-143">For each Connector with type **Active Directory Domain Services**, click **Run**, select **Delta Synchronization**, and **OK**.</span></span>
4. <span data-ttu-id="5c448-144">Välj kopplingen med typen **Azure Active Directory (Microsoft)**.</span><span class="sxs-lookup"><span data-stu-id="5c448-144">Select the Connector with type **Azure Active Directory (Microsoft)**.</span></span> <span data-ttu-id="5c448-145">Klicka på **kör**väljer **Deltasynkronisering**, och **OK**.</span><span class="sxs-lookup"><span data-stu-id="5c448-145">Click **Run**, select **Delta Synchronization**, and **OK**.</span></span>

<span data-ttu-id="5c448-146">Du har nu mellanlagrade export ändringar till Azure AD och lokala AD (om du använder Exchange-hybridinstallation).</span><span class="sxs-lookup"><span data-stu-id="5c448-146">You have now staged export changes to Azure AD and on-premises AD (if you are using Exchange hybrid deployment).</span></span> <span data-ttu-id="5c448-147">Nästa steg kan du granska vad är på väg att ändra innan du startar export till kataloger.</span><span class="sxs-lookup"><span data-stu-id="5c448-147">The next steps allow you to inspect what is about to change before you actually start the export to the directories.</span></span>

#### <a name="verify"></a><span data-ttu-id="5c448-148">Verifiera</span><span class="sxs-lookup"><span data-stu-id="5c448-148">Verify</span></span>
1. <span data-ttu-id="5c448-149">Starta en kommandotolk och gå till`%ProgramFiles%\Microsoft Azure AD Sync\bin`</span><span class="sxs-lookup"><span data-stu-id="5c448-149">Start a cmd prompt and go to `%ProgramFiles%\Microsoft Azure AD Sync\bin`</span></span>
2. <span data-ttu-id="5c448-150">Kör: `csexport "Name of Connector" %temp%\export.xml /f:x` namnet på anslutningen finns i synkroniseringstjänsten.</span><span class="sxs-lookup"><span data-stu-id="5c448-150">Run: `csexport "Name of Connector" %temp%\export.xml /f:x` The name of the Connector can be found in Synchronization Service.</span></span> <span data-ttu-id="5c448-151">Den har ett namn som liknar ”contoso.com – AAD” för Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5c448-151">It has a name similar to "contoso.com – AAD" for Azure AD.</span></span>
3. <span data-ttu-id="5c448-152">Kopiera PowerShell-skriptet från avsnittet [CSAnalyzer](#appendix-csanalyzer) till en fil med namnet `csanalyzer.ps1`.</span><span class="sxs-lookup"><span data-stu-id="5c448-152">Copy the PowerShell script from the section [CSAnalyzer](#appendix-csanalyzer) to a file named `csanalyzer.ps1`.</span></span>
4. <span data-ttu-id="5c448-153">Öppna ett PowerShell-fönster och bläddra till mappen där du skapade PowerShell-skript.</span><span class="sxs-lookup"><span data-stu-id="5c448-153">Open a PowerShell window and browse to the folder where you created the PowerShell script.</span></span>
5. <span data-ttu-id="5c448-154">Kör: `.\csanalyzer.ps1 -xmltoimport %temp%\export.xml`.</span><span class="sxs-lookup"><span data-stu-id="5c448-154">Run: `.\csanalyzer.ps1 -xmltoimport %temp%\export.xml`.</span></span>
6. <span data-ttu-id="5c448-155">Nu har du en fil med namnet **processedusers1.csv** som kan granskas i Microsoft Excel.</span><span class="sxs-lookup"><span data-stu-id="5c448-155">You now have a file named **processedusers1.csv** that can be examined in Microsoft Excel.</span></span> <span data-ttu-id="5c448-156">Alla ändringar som mellanlagras för att exporteras till Azure AD finns i den här filen.</span><span class="sxs-lookup"><span data-stu-id="5c448-156">All changes staged to be exported to Azure AD are found in this file.</span></span>
7. <span data-ttu-id="5c448-157">Gör nödvändiga ändringar i informationen eller konfigurationen och köra de här stegen igen (importera och synkronisera och kontrollera) tills de ändringar som är på väg att exporteras förväntas.</span><span class="sxs-lookup"><span data-stu-id="5c448-157">Make necessary changes to the data or configuration and run these steps again (Import and Synchronize and Verify) until the changes that are about to be exported are expected.</span></span>

#### <a name="switch-active-server"></a><span data-ttu-id="5c448-158">Växeln active server</span><span class="sxs-lookup"><span data-stu-id="5c448-158">Switch active server</span></span>
1. <span data-ttu-id="5c448-159">Inaktivera server (DirSync/FIM/Azure AD Sync) så att den inte exporterar till Azure AD eller Ställ in den i mellanlagringsläge (Azure AD Connect) på servern för närvarande är aktiva.</span><span class="sxs-lookup"><span data-stu-id="5c448-159">On the currently active server, either turn off the server (DirSync/FIM/Azure AD Sync) so it is not exporting to Azure AD or set it in staging mode (Azure AD Connect).</span></span>
2. <span data-ttu-id="5c448-160">Kör installationsguiden på servern i **mellanlagringsläge** och inaktivera **mellanlagringsläge**.</span><span class="sxs-lookup"><span data-stu-id="5c448-160">Run the installation wizard on the server in **staging mode** and disable **staging mode**.</span></span>
   <span data-ttu-id="5c448-161">![ReadyToConfigure](./media/active-directory-aadconnectsync-operations/additionaltasks.png)</span><span class="sxs-lookup"><span data-stu-id="5c448-161">![ReadyToConfigure](./media/active-directory-aadconnectsync-operations/additionaltasks.png)</span></span>

## <a name="disaster-recovery"></a><span data-ttu-id="5c448-162">Haveriberedskap</span><span class="sxs-lookup"><span data-stu-id="5c448-162">Disaster recovery</span></span>
<span data-ttu-id="5c448-163">En del av implementeringen designen är att planera för vad du ska göra om det är en katastrof där du förlorar synkroniseringsservern.</span><span class="sxs-lookup"><span data-stu-id="5c448-163">Part of the implementation design is to plan for what to do in case there is a disaster where you lose the sync server.</span></span> <span data-ttu-id="5c448-164">Det finns olika modeller ska användas och vilket som ska användas beror på flera faktorer, inklusive:</span><span class="sxs-lookup"><span data-stu-id="5c448-164">There are different models to use and which one to use depends on several factors including:</span></span>

* <span data-ttu-id="5c448-165">Vad är din tolerans för som inte kan göra ändringar för objekt i Azure AD under avbrottstiden?</span><span class="sxs-lookup"><span data-stu-id="5c448-165">What is your tolerance for not being able make changes to objects in Azure AD during the downtime?</span></span>
* <span data-ttu-id="5c448-166">Om du använder Lösenordssynkronisering måste användarna accepterar de behöver använda det gamla lösenordet i Azure AD, om de ändra den lokala?</span><span class="sxs-lookup"><span data-stu-id="5c448-166">If you use password synchronization, do the users accept that they have to use the old password in Azure AD in case they change it on-premises?</span></span>
* <span data-ttu-id="5c448-167">Har du ett beroende i realtid åtgärder, till exempel tillbakaskrivning av lösenord?</span><span class="sxs-lookup"><span data-stu-id="5c448-167">Do you have a dependency on real-time operations, such as password writeback?</span></span>

<span data-ttu-id="5c448-168">Beroende på svaren på dessa frågor och organisationens princip, kan något av följande strategier implementeras:</span><span class="sxs-lookup"><span data-stu-id="5c448-168">Depending on the answers to these questions and your organization’s policy, one of the following strategies can be implemented:</span></span>

* <span data-ttu-id="5c448-169">Återskapa vid behov.</span><span class="sxs-lookup"><span data-stu-id="5c448-169">Rebuild when needed.</span></span>
* <span data-ttu-id="5c448-170">En ledig belägna standby-servern, kallas även **mellanlagringsläge**.</span><span class="sxs-lookup"><span data-stu-id="5c448-170">Have a spare standby server, known as **staging mode**.</span></span>
* <span data-ttu-id="5c448-171">Använda virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="5c448-171">Use virtual machines.</span></span>

<span data-ttu-id="5c448-172">Om du inte använder inbyggda SQL Express-databasen, så du bör även se den [hög tillgänglighet för SQL](#sql-high-availability) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="5c448-172">If you do not use the built-in SQL Express database, then you should also review the [SQL High Availability](#sql-high-availability) section.</span></span>

### <a name="rebuild-when-needed"></a><span data-ttu-id="5c448-173">Återskapa vid behov</span><span class="sxs-lookup"><span data-stu-id="5c448-173">Rebuild when needed</span></span>
<span data-ttu-id="5c448-174">En lönsam strategi är att planera för återskapning av en server när det behövs.</span><span class="sxs-lookup"><span data-stu-id="5c448-174">A viable strategy is to plan for a server rebuild when needed.</span></span> <span data-ttu-id="5c448-175">Installera vanligtvis Synkroniseringsmotorn och gör den inledande importen och synkroniseringen kan slutföras inom några timmar.</span><span class="sxs-lookup"><span data-stu-id="5c448-175">Usually, installing the sync engine and do the initial import and sync can be completed within a few hours.</span></span> <span data-ttu-id="5c448-176">Om det inte en extra server, är det möjligt att tillfälligt använda en domänkontrollant som värd för Synkroniseringsmotorn.</span><span class="sxs-lookup"><span data-stu-id="5c448-176">If there isn’t a spare server available, it is possible to temporarily use a domain controller to host the sync engine.</span></span>

<span data-ttu-id="5c448-177">Motorn synkroniseringsserver lagrar inte någon status om objekt så att databasen kan byggas från data i Active Directory och Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5c448-177">The sync engine server does not store any state about the objects so the database can be rebuilt from the data in Active Directory and Azure AD.</span></span> <span data-ttu-id="5c448-178">Den **sourceAnchor** attributet används för att ansluta till objekt från molnet och lokalt.</span><span class="sxs-lookup"><span data-stu-id="5c448-178">The **sourceAnchor** attribute is used to join the objects from on-premises and the cloud.</span></span> <span data-ttu-id="5c448-179">Om du återskapa servern med befintliga objekt lokalt och i molnet matchar Synkroniseringsmotorn de här objekten tillsammans igen på ominstallation.</span><span class="sxs-lookup"><span data-stu-id="5c448-179">If you rebuild the server with existing objects on-premises and the cloud, then the sync engine matches those objects together again on reinstallation.</span></span> <span data-ttu-id="5c448-180">Saker du behöver dokumentera och spara är konfigurationsändringar på servern, till exempel filtrering och Synkroniseringsregler.</span><span class="sxs-lookup"><span data-stu-id="5c448-180">The things you need to document and save are the configuration changes made to the server, such as filtering and synchronization rules.</span></span> <span data-ttu-id="5c448-181">Dessa anpassade konfigurationer måste återställas innan du startar synkroniseringen.</span><span class="sxs-lookup"><span data-stu-id="5c448-181">These custom configurations must be reapplied before you start synchronizing.</span></span>

### <a name="have-a-spare-standby-server---staging-mode"></a><span data-ttu-id="5c448-182">Har en ledig standby server - mellanlagringsläge</span><span class="sxs-lookup"><span data-stu-id="5c448-182">Have a spare standby server - staging mode</span></span>
<span data-ttu-id="5c448-183">Om du har en mer komplex miljö rekommenderas med en eller flera reservservrar.</span><span class="sxs-lookup"><span data-stu-id="5c448-183">If you have a more complex environment, then having one or more standby servers is recommended.</span></span> <span data-ttu-id="5c448-184">Under installationen, kan du aktivera en server i **mellanlagringsläge**.</span><span class="sxs-lookup"><span data-stu-id="5c448-184">During installation, you can enable a server to be in **staging mode**.</span></span>

<span data-ttu-id="5c448-185">Mer information finns i [mellanlagringsläge](#staging-mode).</span><span class="sxs-lookup"><span data-stu-id="5c448-185">For more information, see [staging mode](#staging-mode).</span></span>

### <a name="use-virtual-machines"></a><span data-ttu-id="5c448-186">Använda virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="5c448-186">Use virtual machines</span></span>
<span data-ttu-id="5c448-187">En gemensam och stöds metod är att köra Synkroniseringsmotorn i en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="5c448-187">A common and supported method is to run the sync engine in a virtual machine.</span></span> <span data-ttu-id="5c448-188">Om värden har ett problem, kan avbildningen med motorn synkroniseringsserver migreras till en annan server.</span><span class="sxs-lookup"><span data-stu-id="5c448-188">In case the host has an issue, the image with the sync engine server can be migrated to another server.</span></span>

### <a name="sql-high-availability"></a><span data-ttu-id="5c448-189">Hög tillgänglighet för SQL</span><span class="sxs-lookup"><span data-stu-id="5c448-189">SQL High Availability</span></span>
<span data-ttu-id="5c448-190">Om du inte använder den SQL Server Express som medföljer Azure AD Connect, bör sedan hög tillgänglighet för SQL Server också övervägas.</span><span class="sxs-lookup"><span data-stu-id="5c448-190">If you are not using the SQL Server Express that comes with Azure AD Connect, then high availability for SQL Server should also be considered.</span></span> <span data-ttu-id="5c448-191">Lösningar för hög tillgänglighet som stöds är SQL-kluster och AOA (alltid på Tillgänglighetsgrupper).</span><span class="sxs-lookup"><span data-stu-id="5c448-191">The high availability solutions supported include SQL clustering and AOA (Always On Availability Groups).</span></span> <span data-ttu-id="5c448-192">Stöds inte lösningar omfattar spegling.</span><span class="sxs-lookup"><span data-stu-id="5c448-192">Unsupported solutions include mirroring.</span></span>

<span data-ttu-id="5c448-193">Stöd för SQL AOA har lagts till Azure AD Connect i version 1.1.524.0.</span><span class="sxs-lookup"><span data-stu-id="5c448-193">Support for SQL AOA was added to Azure AD Connect in version 1.1.524.0.</span></span> <span data-ttu-id="5c448-194">Du måste aktivera SQL AOA innan du installerar Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="5c448-194">You must enable SQL AOA before installing Azure AD Connect.</span></span> <span data-ttu-id="5c448-195">Under installationen av identifierar Azure AD Connect om den angivna SQL-instansen är aktiverat för SQL AOA eller inte.</span><span class="sxs-lookup"><span data-stu-id="5c448-195">During installation, Azure AD Connect detects whether the SQL instance provided is enabled for SQL AOA or not.</span></span> <span data-ttu-id="5c448-196">Om SQL AOA är aktiverat räknat Azure AD Connect ytterligare ut om SQL AOA är konfigurerad för att använda synkron eller asynkron replikeringen.</span><span class="sxs-lookup"><span data-stu-id="5c448-196">If SQL AOA is enabled, Azure AD Connect further figures out if SQL AOA is configured to use synchronous replication or asynchronous replication.</span></span> <span data-ttu-id="5c448-197">När du ställer in tillgänglighetsgruppens lyssnare, rekommenderas att du anger egenskapen RegisterAllProvidersIP till 0.</span><span class="sxs-lookup"><span data-stu-id="5c448-197">When setting up the Availability Group Listener, it is recommended that you set the RegisterAllProvidersIP property to 0.</span></span> <span data-ttu-id="5c448-198">Detta beror på att Azure AD Connect används för närvarande SQL Native Client för anslutning till SQL och SQL Native Client stöder inte användning av MultiSubNetFailover-egenskapen.</span><span class="sxs-lookup"><span data-stu-id="5c448-198">This is because Azure AD Connect currently uses SQL Native Client to connect to SQL and SQL Native Client does not support the use of MultiSubNetFailover property.</span></span>

## <a name="appendix-csanalyzer"></a><span data-ttu-id="5c448-199">Bilaga CSAnalyzer</span><span class="sxs-lookup"><span data-stu-id="5c448-199">Appendix CSAnalyzer</span></span>
<span data-ttu-id="5c448-200">Se avsnittet [Kontrollera](#verify) om hur du använder det här skriptet.</span><span class="sxs-lookup"><span data-stu-id="5c448-200">See the section [verify](#verify) on how to use this script.</span></span>

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

#XmlReader.Create won't properly resolve the file location,
#so expand and then resolve it
$resolvedXMLtoimport=Resolve-Path -Path ([Environment]::ExpandEnvironmentVariables($xmltoimport))

#use an XmlReader to deal with even large files
$result=$reader = [System.Xml.XmlReader]::Create($resolvedXMLtoimport) 
$result=$reader.ReadToDescendant('cs-object')
do 
{
    #create the object placeholder
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

    #now that we have the basics, go get the details

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

    #every so often, dump the processed users in case we blow up somewhere
    if ($count % $batchsize -eq 0)
    {
        Write-Host Hit the maximum users processed without completion... -ForegroundColor Yellow

        #export the collection of users as as CSV
        Write-Host Writing processedusers${outputfilecount}.csv -ForegroundColor Yellow
        $objOutputUsers | Export-Csv -path processedusers${outputfilecount}.csv -NoTypeInformation

        #increment the output file counter
        $outputfilecount+=1

        #reset the collection and the user counter
        $objOutputUsers = $null
        $count=0
    }

    $count+=1

    #need to bail out of the loop if no more users to process
    if ($reader.NodeType -eq [System.Xml.XmlNodeType]::EndElement)
    {
        break
    }

} while ($reader.Read)

#need to write out any users that didn't get picked up in a batch of 1000
#export the collection of users as as CSV
Write-Host Writing processedusers${outputfilecount}.csv -ForegroundColor Yellow
$objOutputUsers | Export-Csv -path processedusers${outputfilecount}.csv -NoTypeInformation
```

## <a name="next-steps"></a><span data-ttu-id="5c448-201">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5c448-201">Next steps</span></span>
<span data-ttu-id="5c448-202">**Översiktsavsnitt**</span><span class="sxs-lookup"><span data-stu-id="5c448-202">**Overview topics**</span></span>  

* [<span data-ttu-id="5c448-203">Azure AD Connect-synkronisering: Förstå och anpassa synkronisering</span><span class="sxs-lookup"><span data-stu-id="5c448-203">Azure AD Connect sync: Understand and customize synchronization</span></span>](active-directory-aadconnectsync-whatis.md)  
* [<span data-ttu-id="5c448-204">Integrera dina lokala identiteter med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="5c448-204">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)  
