---
title: "HDInsight-aaaCustomize kluster med skriptåtgärder - Azure | Microsoft Docs"
description: "Lägga till anpassade komponenter tooLinux-baserade HDInsight-kluster med skriptåtgärder. Skriptåtgärder avses Bash-skript som kan använda toocustomize hello klusterkonfigurationen eller lägga till ytterligare tjänster och verktyg som Hue, Solr eller R."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 48e85f53-87c1-474f-b767-ca772238cc13
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: larryfr
ms.openlocfilehash: ff22680a8a50b21985f6941f1edaf1dcf863d13f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="customize-linux-based-hdinsight-clusters-using-script-action"></a><span data-ttu-id="bbfcc-104">Anpassa Linux-baserat HDInsight-kluster med skriptåtgärder</span><span class="sxs-lookup"><span data-stu-id="bbfcc-104">Customize Linux-based HDInsight clusters using Script Action</span></span>

<span data-ttu-id="bbfcc-105">HDInsight tillhandahåller ett konfigurationsalternativ som kallas **skriptåtgärd** som anropar anpassade skript för att anpassa hello klustret.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-105">HDInsight provides a configuration option called **Script Action** that invokes custom scripts that customize hello cluster.</span></span> <span data-ttu-id="bbfcc-106">Dessa skript är används tooinstall ytterligare komponenter och ändra konfigurationsinställningarna.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-106">These scripts are used tooinstall additional components and change configuration settings.</span></span> <span data-ttu-id="bbfcc-107">Skriptåtgärder kan användas under eller efter att klustret har skapats.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-107">Script actions can be used during or after cluster creation.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="bbfcc-108">hello möjlighet toouse skriptåtgärder på ett kluster som redan körs är endast tillgängligt för Linux-baserade HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-108">hello ability toouse script actions on an already running cluster is only available for Linux-based HDInsight clusters.</span></span>
>
> <span data-ttu-id="bbfcc-109">Linux är hello endast operativsystem på HDInsight version 3.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-109">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="bbfcc-110">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="bbfcc-110">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

<span data-ttu-id="bbfcc-111">Skriptåtgärder kan också vara publicerade toohello Azure Marketplace som ett HDInsight-program.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-111">Script actions can also be published toohello Azure Marketplace as an HDInsight application.</span></span> <span data-ttu-id="bbfcc-112">Vissa av hello exemplen i det här dokumentet visar hur du kan installera ett HDInsight-program med hjälp av åtgärden skriptkommandon från PowerShell och hello .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-112">Some of hello examples in this document show how you can install an HDInsight application using script action commands from PowerShell and hello .NET SDK.</span></span> <span data-ttu-id="bbfcc-113">Mer information om HDInsight-program finns [publicera HDInsight-program hello Azure Marketplace](hdinsight-apps-publish-applications.md).</span><span class="sxs-lookup"><span data-stu-id="bbfcc-113">For more information on HDInsight applications, see [Publish HDInsight applications into hello Azure Marketplace](hdinsight-apps-publish-applications.md).</span></span>

## <a name="permissions"></a><span data-ttu-id="bbfcc-114">Behörigheter</span><span class="sxs-lookup"><span data-stu-id="bbfcc-114">Permissions</span></span>

<span data-ttu-id="bbfcc-115">Om du använder en domänansluten HDInsight-kluster, finns det två Ambari-behörigheter som krävs vid användning av skriptåtgärder med hello-kluster:</span><span class="sxs-lookup"><span data-stu-id="bbfcc-115">If you are using a domain-joined HDInsight cluster, there are two Ambari permissions that are required when using script actions with hello cluster:</span></span>

* <span data-ttu-id="bbfcc-116">**AMBARI. KÖR\_anpassad\_kommandot**: hello Ambari administratörsroll har denna behörighet som standard.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-116">**AMBARI.RUN\_CUSTOM\_COMMAND**: hello Ambari Administrator role has this permission by default.</span></span>
* <span data-ttu-id="bbfcc-117">**KLUSTER. KÖR\_anpassad\_kommandot**: båda hello HDInsight-klustrets administratör och Ambari-administratören har denna behörighet som standard.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-117">**CLUSTER.RUN\_CUSTOM\_COMMAND**: Both hello HDInsight Cluster Administrator and Ambari Administrator have this permission by default.</span></span>

<span data-ttu-id="bbfcc-118">Mer information om hur du arbetar med behörigheter med domänanslutna HDInsight finns [hantera domänanslutna HDInsight-kluster](hdinsight-domain-joined-manage.md).</span><span class="sxs-lookup"><span data-stu-id="bbfcc-118">For more information on working with permissions with domain-joined HDInsight, see [Manage domain-joined HDInsight clusters](hdinsight-domain-joined-manage.md).</span></span>

## <a name="access-control"></a><span data-ttu-id="bbfcc-119">Åtkomstkontroll</span><span class="sxs-lookup"><span data-stu-id="bbfcc-119">Access control</span></span>

<span data-ttu-id="bbfcc-120">Om du inte är Hej administratör/ägaren av din Azure-prenumeration, ditt konto måste ha minst **deltagare** åtkomst toohello resursgruppen som innehåller hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-120">If you are not hello administrator/owner of your Azure subscription, your account must have at least **Contributor** access toohello resource group that contains hello HDInsight cluster.</span></span>

<span data-ttu-id="bbfcc-121">Dessutom, om du skapar ett HDInsight-kluster någon med minst **deltagare** åtkomst toohello Azure-prenumeration måste redan har registrerat hello provider för HDInsight.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-121">Additionally, if you are creating an HDInsight cluster, someone with at least **Contributor** access toohello Azure subscription must have previously registered hello provider for HDInsight.</span></span> <span data-ttu-id="bbfcc-122">Providerregistrering sker när en användare med deltagare åtkomst toohello prenumeration skapar en resurs för hello första gången på hello prenumeration.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-122">Provider registration happens when a user with Contributor access toohello subscription creates a resource for hello first time on hello subscription.</span></span> <span data-ttu-id="bbfcc-123">Detta kan också inträffa utan att en resurs skapas, om en [leverantör registreras med REST](https://msdn.microsoft.com/library/azure/dn790548.aspx).</span><span class="sxs-lookup"><span data-stu-id="bbfcc-123">It can also be accomplished without creating a resource by [registering a provider using REST](https://msdn.microsoft.com/library/azure/dn790548.aspx).</span></span>

<span data-ttu-id="bbfcc-124">Mer information om hur du arbetar med access management finns i hello följande dokument:</span><span class="sxs-lookup"><span data-stu-id="bbfcc-124">For more information on working with access management, see hello following documents:</span></span>

* [<span data-ttu-id="bbfcc-125">Kom igång med åtkomsthantering i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="bbfcc-125">Get started with access management in hello Azure portal</span></span>](../active-directory/role-based-access-control-what-is.md)
* [<span data-ttu-id="bbfcc-126">Använda rollen tilldelningar toomanage åtkomst tooyour Azure-prenumerationsresurser</span><span class="sxs-lookup"><span data-stu-id="bbfcc-126">Use role assignments toomanage access tooyour Azure subscription resources</span></span>](../active-directory/role-based-access-control-configure.md)

## <a name="understanding-script-actions"></a><span data-ttu-id="bbfcc-127">Förstå skriptåtgärder</span><span class="sxs-lookup"><span data-stu-id="bbfcc-127">Understanding Script Actions</span></span>

<span data-ttu-id="bbfcc-128">En skriptåtgärd är bara ett Bash-skript som du anger en URI för och parametrar för.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-128">A Script Action is simply a Bash script that you provide a URI to, and parameters for.</span></span> <span data-ttu-id="bbfcc-129">hello skriptet körs på noderna i hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-129">hello script runs on nodes in hello HDInsight cluster.</span></span> <span data-ttu-id="bbfcc-130">hello följande är egenskaper och funktioner för skriptåtgärder.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-130">hello following are characteristics and features of script actions.</span></span>

* <span data-ttu-id="bbfcc-131">Måste vara lagrade på en URI som är tillgänglig från hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-131">Must be stored on a URI that is accessible from hello HDInsight cluster.</span></span> <span data-ttu-id="bbfcc-132">hello följande är möjliga lagringsplatser:</span><span class="sxs-lookup"><span data-stu-id="bbfcc-132">hello following are possible storage locations:</span></span>

    * <span data-ttu-id="bbfcc-133">En **Azure Data Lake Store** konto som kan nås av hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-133">An **Azure Data Lake Store** account that is accessible by hello HDInsight cluster.</span></span> <span data-ttu-id="bbfcc-134">Information om hur du använder Azure Data Lake Store med HDInsight finns i [skapar ett HDInsight-kluster med Data Lake Store](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).</span><span class="sxs-lookup"><span data-stu-id="bbfcc-134">For information on using Azure Data Lake Store with HDInsight, see [Create an HDInsight cluster with Data Lake Store](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).</span></span>

        <span data-ttu-id="bbfcc-135">När du använder ett skript som lagras i Data Lake Store hello URI-formatet är `adl://DATALAKESTOREACCOUNTNAME.azuredatalakestore.net/path_to_file`.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-135">When using a script stored in Data Lake Store, hello URI format is `adl://DATALAKESTOREACCOUNTNAME.azuredatalakestore.net/path_to_file`.</span></span>

        > [!NOTE]
        > <span data-ttu-id="bbfcc-136">hello service principal HDInsight använder tooaccess Data Lake Store måste ha läsbehörighet toohello skript.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-136">hello service principal HDInsight uses tooaccess Data Lake Store must have read access toohello script.</span></span>

    * <span data-ttu-id="bbfcc-137">En blobb i en **Azure Storage-konto** som är antingen hello primära eller ytterligare lagringskonto för hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-137">A blob in an **Azure Storage account** that is either hello primary or additional storage account for hello HDInsight cluster.</span></span> <span data-ttu-id="bbfcc-138">HDInsight har beviljats åtkomst tooboth för dessa typer av lagringskonton när klustret skapas.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-138">HDInsight is granted access tooboth of these types of storage accounts during cluster creation.</span></span>

    * <span data-ttu-id="bbfcc-139">En offentlig fildelning tjänsten Azure Blob, GitHub, OneDrive, Dropbox, t.ex.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-139">A public file sharing service such as Azure Blob, GitHub, OneDrive, Dropbox, etc.</span></span>

        <span data-ttu-id="bbfcc-140">Till exempel URI: er, se hello [exempelskript skript åtgärd](#example-script-action-scripts) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-140">For example URIs, see hello [Example script action scripts](#example-script-action-scripts) section.</span></span>

        > [!WARNING]
        > <span data-ttu-id="bbfcc-141">HDInsight stöder endast __allmänna__ Azure Storage-konton.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-141">HDInsight only supports __General-purpose__ Azure Storage accounts.</span></span> <span data-ttu-id="bbfcc-142">Den stöder för närvarande inte hello __Blob storage__ kontotyp.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-142">It does not currently support hello __Blob storage__ account type.</span></span>

* <span data-ttu-id="bbfcc-143">Kan vara begränsade för**körs på vissa nodtyper**, för exempel huvudnoderna eller arbetsnoderna.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-143">Can be restricted too**run on only certain node types**, for example head nodes or worker nodes.</span></span>

  > [!NOTE]
  > <span data-ttu-id="bbfcc-144">När det används med HDInsight Premium kan du ange att hello skript ska användas på hello kantnod.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-144">When used with HDInsight Premium, you can specify that hello script should be used on hello edge node.</span></span>

* <span data-ttu-id="bbfcc-145">Kan vara **beständiga** eller **ad hoc-**.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-145">Can be **persisted** or **ad hoc**.</span></span>

    <span data-ttu-id="bbfcc-146">**Beständiga** skript är tillämpade tooworker noder tillagda toohello kluster när hello skriptet körs.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-146">**Persisted** scripts are applied tooworker nodes added toohello cluster after hello script runs.</span></span> <span data-ttu-id="bbfcc-147">Till exempel när du ökar hello klustret.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-147">For example, when scaling up hello cluster.</span></span>

    <span data-ttu-id="bbfcc-148">Ett bestående skript kan också använda ändringar nodtypen tooanother, till exempel en huvudnod.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-148">A persisted script might also apply changes tooanother node type, such as a head node.</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="bbfcc-149">Beständiga skriptåtgärder måste ha ett unikt namn.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-149">Persisted script actions must have a unique name.</span></span>

    <span data-ttu-id="bbfcc-150">**Ad hoc-** skript är inte beständiga.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-150">**Ad hoc** scripts are not persisted.</span></span> <span data-ttu-id="bbfcc-151">De är inte tillämpade tooworker noder tillagda toohello kluster när hello skriptet har körts.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-151">They are not applied tooworker nodes added toohello cluster after hello script has ran.</span></span> <span data-ttu-id="bbfcc-152">Du kan därefter befordra en ad hoc-skriptet tooa bestående skript eller nedgradera ett bestående skript tooan ad hoc-skript.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-152">You can subsequently promote an ad hoc script tooa persisted script, or demote a persisted script tooan ad hoc script.</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="bbfcc-153">Skriptåtgärder användas när klustret skapas sparas automatiskt.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-153">Script actions used during cluster creation are automatically persisted.</span></span>
  >
  > <span data-ttu-id="bbfcc-154">Skript som misslyckas inte är beständiga, även om du uttryckligen har angett att de ska.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-154">Scripts that fail are not persisted, even if you specifically indicate that they should be.</span></span>

* <span data-ttu-id="bbfcc-155">Kan acceptera **parametrar** som används av hello skript under körning.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-155">Can accept **parameters** that are used by hello script during execution.</span></span>
* <span data-ttu-id="bbfcc-156">Kör med **rätt behörighet för rotmappen** på hello klusternoder.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-156">Run with **root level privileges** on hello cluster nodes.</span></span>
* <span data-ttu-id="bbfcc-157">Kan användas via hello **Azure-portalen**, **Azure PowerShell**, **Azure CLI**, eller **HDInsight .NET SDK**</span><span class="sxs-lookup"><span data-stu-id="bbfcc-157">Can be used through hello **Azure portal**, **Azure PowerShell**, **Azure CLI**, or **HDInsight .NET SDK**</span></span>

<span data-ttu-id="bbfcc-158">hello klustret sparar en historik över alla skript som har varit kördes.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-158">hello cluster keeps a history of all scripts that have been ran.</span></span> <span data-ttu-id="bbfcc-159">hello historik är användbart när du behöver toofind hello-ID för ett skript för befordran eller degradering åtgärder.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-159">hello history is useful when you need toofind hello ID of a script for promotion or demotion operations.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="bbfcc-160">Det finns inget automatiskt sätt tooundo hello ändringar som gjorts av en skriptåtgärd.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-160">There is no automatic way tooundo hello changes made by a script action.</span></span> <span data-ttu-id="bbfcc-161">Antingen manuellt återställa hello eller ange ett skript som kastar dem.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-161">Either manually reverse hello changes or provide a script that reverses them.</span></span>


### <a name="script-action-in-hello-cluster-creation-process"></a><span data-ttu-id="bbfcc-162">Skriptåtgärder i hello klusterskapandeprocessen</span><span class="sxs-lookup"><span data-stu-id="bbfcc-162">Script Action in hello cluster creation process</span></span>

<span data-ttu-id="bbfcc-163">Används när klustret skapas med skriptåtgärder avses skiljer sig från skriptet åtgärder som kördes på ett befintligt kluster:</span><span class="sxs-lookup"><span data-stu-id="bbfcc-163">Script Actions used during cluster creation are slightly different from script actions ran on an existing cluster:</span></span>

* <span data-ttu-id="bbfcc-164">hello skript är **automatiskt beständiga**.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-164">hello script is **automatically persisted**.</span></span>
* <span data-ttu-id="bbfcc-165">En **fel** i hello skript kan orsaka hello klustret skapas processen toofail.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-165">A **failure** in hello script can cause hello cluster creation process toofail.</span></span>

<span data-ttu-id="bbfcc-166">hello följande diagram illustrerar när skriptet körs under skapandeprocessen hello:</span><span class="sxs-lookup"><span data-stu-id="bbfcc-166">hello following diagram illustrates when Script Action is executed during hello creation process:</span></span>

<span data-ttu-id="bbfcc-167">![HDInsight-kluster anpassning och faserna när klustret skapas][img-hdi-cluster-states]</span><span class="sxs-lookup"><span data-stu-id="bbfcc-167">![HDInsight cluster customization and stages during cluster creation][img-hdi-cluster-states]</span></span>

<span data-ttu-id="bbfcc-168">hello skriptet körs medan HDInsight konfigureras.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-168">hello script runs while HDInsight is being configured.</span></span> <span data-ttu-id="bbfcc-169">I det här skedet hello hello skript körs parallellt på alla noderna i klustret hello och körs med rotprivilegier på hello noder.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-169">At this stage, hello script runs in parallel on all hello specified nodes in hello cluster, and runs with root privileges on hello nodes.</span></span>

> [!NOTE]
> <span data-ttu-id="bbfcc-170">Eftersom hello skriptet körs med rot nivån behörighet på hello klusternoder, kan du utföra åtgärder som att stoppa och starta tjänster, inklusive Hadoop-relaterade tjänster.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-170">Because hello script runs with root level privilege on hello cluster nodes, you can perform operations like stopping and starting services, including Hadoop-related services.</span></span> <span data-ttu-id="bbfcc-171">Om du stoppar tjänster måste du kontrollera att tjänsten för hello Ambari och andra Hadoop-relaterade tjänster är igång innan hello skriptet är klar.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-171">If you stop services, you must ensure that hello Ambari service and other Hadoop-related services are up and running before hello script finishes running.</span></span> <span data-ttu-id="bbfcc-172">De här tjänsterna krävs toosuccessfully fastställa hello hälsa och status för hello klustret medan det skapas.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-172">These services are required toosuccessfully determine hello health and state of hello cluster while it is being created.</span></span>


<span data-ttu-id="bbfcc-173">När klustret skapas, kan du använda flera skriptåtgärder samtidigt.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-173">During cluster creation, you can use multiple script actions at once.</span></span> <span data-ttu-id="bbfcc-174">Dessa skript anropas i hello ordning som de har angetts.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-174">These scripts are invoked in hello order in which they were specified.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="bbfcc-175">Skriptåtgärder måste slutföras inom 60 minuter eller tidsgräns.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-175">Script actions must complete within 60 minutes, or timeout.</span></span> <span data-ttu-id="bbfcc-176">Under klusteretablering, körs hello skriptet samtidigt med andra processer för installation och konfiguration.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-176">During cluster provisioning, hello script runs concurrently with other setup and configuration processes.</span></span> <span data-ttu-id="bbfcc-177">Konkurrens om resurser, till exempel CPU-tid eller nätverket bandbredd kan orsaka hello skriptet tootake längre toofinish än i din utvecklingsmiljö.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-177">Competition for resources such as CPU time or network bandwidth may cause hello script tootake longer toofinish than it does in your development environment.</span></span>
>
> <span data-ttu-id="bbfcc-178">toominimize hello tid det tar toorun hello skript, Undvik aktiviteter som att hämta och sammanställa program från källan.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-178">toominimize hello time it takes toorun hello script, avoid tasks such as downloading and compiling applications from source.</span></span> <span data-ttu-id="bbfcc-179">Före kompilera program och lagra hello binary i Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-179">Pre-compile applications and store hello binary in Azure Storage.</span></span>


### <a name="script-action-on-a-running-cluster"></a><span data-ttu-id="bbfcc-180">Skriptåtgärder på ett kluster som körs</span><span class="sxs-lookup"><span data-stu-id="bbfcc-180">Script action on a running cluster</span></span>

<span data-ttu-id="bbfcc-181">Till skillnad från skriptet åtgärder som används när klustret skapas ett fel i ett skript kördes på ett kluster som redan körs orsakar automatiskt inte hello klustertillstånd toochange tooa misslyckades.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-181">Unlike script actions used during cluster creation, a failure in a script ran on an already running cluster does not automatically cause hello cluster toochange tooa failed state.</span></span> <span data-ttu-id="bbfcc-182">När ett skript är klar ska hello klustret returnera tooa ”körs”.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-182">Once a script completes, hello cluster should return tooa "running" state.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="bbfcc-183">Även om hello klustret har tillståndet 'körs', innehåller hello misslyckade skriptet brutna saker.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-183">Even if hello cluster has a 'running' state, hello failed script may have broken things.</span></span> <span data-ttu-id="bbfcc-184">Ett skript kan till exempel ta bort filer som behövs hello klustret.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-184">For example, a script could delete files needed by hello cluster.</span></span>
>
> <span data-ttu-id="bbfcc-185">Åtgärder som skript köras med rotprivilegier, så bör du se till att du förstår vad ett skript som gör innan den tillämpas tooyour klustret.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-185">Scripts actions run with root privileges, so you should make sure that you understand what a script does before applying it tooyour cluster.</span></span>

<span data-ttu-id="bbfcc-186">När du använder ett skript tooa kluster, hello klustrets tillstånd ändras toofrom **kör** för**godkända**, sedan **HDInsight configuration**, och slutligen tillbaka för**Kör** för lyckad skript.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-186">When applying a script tooa cluster, hello cluster state changes toofrom **Running** too**Accepted**, then **HDInsight configuration**, and finally back too**Running** for successful scripts.</span></span> <span data-ttu-id="bbfcc-187">hello Skriptstatus loggas i historiken för skriptåtgärder hello och du kan använda denna information toodetermine om hello skript har lyckats eller misslyckats.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-187">hello script status is logged in hello script action history, and you can use this information toodetermine whether hello script succeeded or failed.</span></span> <span data-ttu-id="bbfcc-188">Till exempel hello `Get-AzureRmHDInsightScriptActionHistory` PowerShell-cmdlet kan vara används tooview hello status för ett skript.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-188">For example, hello `Get-AzureRmHDInsightScriptActionHistory` PowerShell cmdlet can be used tooview hello status of a script.</span></span> <span data-ttu-id="bbfcc-189">Returnerar information liknande toohello följande text:</span><span class="sxs-lookup"><span data-stu-id="bbfcc-189">It returns information similar toohello following text:</span></span>

    ScriptExecutionId : 635918532516474303
    StartTime         : 8/14/2017 7:40:55 PM
    EndTime           : 8/14/2017 7:41:05 PM
    Status            : Succeeded

> [!NOTE]
> <span data-ttu-id="bbfcc-190">Om du har ändrat hello kluster (admin) användarlösenord när hello klustret har skapats, misslyckas skriptet åtgärder har körts för det här klustret.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-190">If you have changed hello cluster user (admin) password after hello cluster was created, script actions ran against this cluster may fail.</span></span> <span data-ttu-id="bbfcc-191">Om du har några beständiga skriptåtgärder arbetsnoder som mål, misslyckas dessa skript när du skalar hello klustret.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-191">If you have any persisted script actions that target worker nodes, these scripts may fail when you scale hello cluster.</span></span>

## <a name="example-script-action-scripts"></a><span data-ttu-id="bbfcc-192">Exempelskript för skriptåtgärder</span><span class="sxs-lookup"><span data-stu-id="bbfcc-192">Example Script Action scripts</span></span>

<span data-ttu-id="bbfcc-193">Skriptet åtgärd skript kan användas via hello följande verktyg:</span><span class="sxs-lookup"><span data-stu-id="bbfcc-193">Script Action scripts can be used through hello following utilities:</span></span>

* <span data-ttu-id="bbfcc-194">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="bbfcc-194">Azure portal</span></span>
* <span data-ttu-id="bbfcc-195">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="bbfcc-195">Azure PowerShell</span></span>
* <span data-ttu-id="bbfcc-196">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="bbfcc-196">Azure CLI</span></span>
* <span data-ttu-id="bbfcc-197">HDInsight .NET SDK</span><span class="sxs-lookup"><span data-stu-id="bbfcc-197">HDInsight .NET SDK</span></span>

<span data-ttu-id="bbfcc-198">HDInsight tillhandahåller skript tooinstall hello följande komponenter i HDInsight-kluster:</span><span class="sxs-lookup"><span data-stu-id="bbfcc-198">HDInsight provides scripts tooinstall hello following components on HDInsight clusters:</span></span>

| <span data-ttu-id="bbfcc-199">Namn</span><span class="sxs-lookup"><span data-stu-id="bbfcc-199">Name</span></span> | <span data-ttu-id="bbfcc-200">Skript</span><span class="sxs-lookup"><span data-stu-id="bbfcc-200">Script</span></span> |
| --- | --- |
| <span data-ttu-id="bbfcc-201">**Lägg till ett Azure Storage-konto**</span><span class="sxs-lookup"><span data-stu-id="bbfcc-201">**Add an Azure Storage account**</span></span> |<span data-ttu-id="bbfcc-202">https://hdiconfigactions.BLOB.Core.Windows.NET/linuxaddstorageaccountv01/Add-Storage-Account-v01.SH. Se [Lägg till ytterligare lagringsutrymme tooan HDInsight-kluster](hdinsight-hadoop-add-storage.md).</span><span class="sxs-lookup"><span data-stu-id="bbfcc-202">https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh. See [Add additional storage tooan HDInsight cluster](hdinsight-hadoop-add-storage.md).</span></span> |
| <span data-ttu-id="bbfcc-203">**Installera Hue**</span><span class="sxs-lookup"><span data-stu-id="bbfcc-203">**Install Hue**</span></span> |<span data-ttu-id="bbfcc-204">https://hdiconfigactions.BLOB.Core.Windows.NET/linuxhueconfigactionv02/Install-Hue-uber-v02.SH. Se [installerar och använder Hue på HDInsight-kluster](hdinsight-hadoop-hue-linux.md).</span><span class="sxs-lookup"><span data-stu-id="bbfcc-204">https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh. See [Install and use Hue on HDInsight clusters](hdinsight-hadoop-hue-linux.md).</span></span> |
| <span data-ttu-id="bbfcc-205">**Installera Presto**</span><span class="sxs-lookup"><span data-stu-id="bbfcc-205">**Install Presto**</span></span> |<span data-ttu-id="bbfcc-206">https://Raw.githubusercontent.com/hdinsight/Presto-hdinsight/Master/installpresto.SH. Se [installerar och använder Presto på HDInsight-kluster](hdinsight-hadoop-install-presto.md).</span><span class="sxs-lookup"><span data-stu-id="bbfcc-206">https://raw.githubusercontent.com/hdinsight/presto-hdinsight/master/installpresto.sh. See [Install and use Presto on HDInsight clusters](hdinsight-hadoop-install-presto.md).</span></span> |
| <span data-ttu-id="bbfcc-207">**Installera Solr**</span><span class="sxs-lookup"><span data-stu-id="bbfcc-207">**Install Solr**</span></span> |<span data-ttu-id="bbfcc-208">https://hdiconfigactions.BLOB.Core.Windows.NET/linuxsolrconfigactionv01/solr-Installer-v01.SH. Se [installerar och använder Solr på HDInsight-kluster](hdinsight-hadoop-solr-install-linux.md).</span><span class="sxs-lookup"><span data-stu-id="bbfcc-208">https://hdiconfigactions.blob.core.windows.net/linuxsolrconfigactionv01/solr-installer-v01.sh. See [Install and use Solr on HDInsight clusters](hdinsight-hadoop-solr-install-linux.md).</span></span> |
| <span data-ttu-id="bbfcc-209">**Installera Giraph**</span><span class="sxs-lookup"><span data-stu-id="bbfcc-209">**Install Giraph**</span></span> |<span data-ttu-id="bbfcc-210">https://hdiconfigactions.BLOB.Core.Windows.NET/linuxgiraphconfigactionv01/giraph-Installer-v01.SH. Se [installerar och använder Giraph på HDInsight-kluster](hdinsight-hadoop-giraph-install-linux.md).</span><span class="sxs-lookup"><span data-stu-id="bbfcc-210">https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh. See [Install and use Giraph on HDInsight clusters](hdinsight-hadoop-giraph-install-linux.md).</span></span> |
| <span data-ttu-id="bbfcc-211">**Läsa in Hive-bibliotek**</span><span class="sxs-lookup"><span data-stu-id="bbfcc-211">**Pre-load Hive libraries**</span></span> |<span data-ttu-id="bbfcc-212">https://hdiconfigactions.BLOB.Core.Windows.NET/linuxsetupcustomhivelibsv01/Setup-customhivelibs-v01.SH. Se [lägga till Hive-bibliotek i HDInsight-kluster](hdinsight-hadoop-add-hive-libraries.md).</span><span class="sxs-lookup"><span data-stu-id="bbfcc-212">https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh. See [Add Hive libraries on HDInsight clusters](hdinsight-hadoop-add-hive-libraries.md).</span></span> |
| <span data-ttu-id="bbfcc-213">**Installera eller uppdatera Mono**</span><span class="sxs-lookup"><span data-stu-id="bbfcc-213">**Install or update Mono**</span></span> | <span data-ttu-id="bbfcc-214">https://hdiconfigactions.BLOB.Core.Windows.NET/Install-Mono/Install-Mono.Bash.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-214">https://hdiconfigactions.blob.core.windows.net/install-mono/install-mono.bash.</span></span> <span data-ttu-id="bbfcc-215">Se [installera eller uppdatera Mono på HDInsight](hdinsight-hadoop-install-mono.md).</span><span class="sxs-lookup"><span data-stu-id="bbfcc-215">See [Install or update Mono on HDInsight](hdinsight-hadoop-install-mono.md).</span></span> |

## <a name="use-a-script-action-during-cluster-creation"></a><span data-ttu-id="bbfcc-216">Använd en skriptåtgärd när klustret skapas</span><span class="sxs-lookup"><span data-stu-id="bbfcc-216">Use a Script Action during cluster creation</span></span>

<span data-ttu-id="bbfcc-217">Det här avsnittet innehåller exempel på hello olika sätt som du kan använda script actions när du skapar ett HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-217">This section provides examples on hello different ways you can use script actions when creating an HDInsight cluster.</span></span>

### <a name="use-a-script-action-during-cluster-creation-from-hello-azure-portal"></a><span data-ttu-id="bbfcc-218">Använd en skriptåtgärd när klustret skapas från hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="bbfcc-218">Use a Script Action during cluster creation from hello Azure portal</span></span>

1. <span data-ttu-id="bbfcc-219">Skapa ett kluster, enligt beskrivningen i [skapa Hadoop-kluster i HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="bbfcc-219">Start creating a cluster as described at [Create Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span></span> <span data-ttu-id="bbfcc-220">Stoppa när du når hello __klustret sammanfattning__ avsnitt.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-220">Stop when you reach hello __Cluster summary__ section.</span></span>

2. <span data-ttu-id="bbfcc-221">Från hello __klustret sammanfattning__ avsnitt, Välj hello __redigera__ länka för __avancerade inställningar__.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-221">From hello __Cluster summary__ section, select hello __edit__ link for __Advanced settings__.</span></span>

    ![Avancerade inställningar länk](./media/hdinsight-hadoop-customize-cluster-linux/advanced-settings-link.png)

3. <span data-ttu-id="bbfcc-223">Från hello __avancerade inställningar__ väljer __skript åtgärder__.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-223">From hello __Advanced settings__ section, select __Script actions__.</span></span> <span data-ttu-id="bbfcc-224">Från hello __skript åtgärder__ väljer __+ skicka nya__</span><span class="sxs-lookup"><span data-stu-id="bbfcc-224">From hello __Script actions__ section, select __+ Submit new__</span></span>

    ![Skicka en ny skriptåtgärd](./media/hdinsight-hadoop-customize-cluster-linux/add-script-action.png)

4. <span data-ttu-id="bbfcc-226">Använd hello __väljer du ett skript__ post tooselect en fördefinierad skript.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-226">Use hello __Select a script__ entry tooselect a pre-made script.</span></span> <span data-ttu-id="bbfcc-227">Välj toouse ett anpassat skript __anpassade__ och ange sedan hello __namn__ och __Bash skript URI__ för skriptet.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-227">toouse a custom script, select __Custom__ and then provide hello __Name__ and __Bash script URI__ for your script.</span></span>

    ![Lägga till ett skript i hello väljer skriptform](./media/hdinsight-hadoop-customize-cluster-linux/select-script.png)

    <span data-ttu-id="bbfcc-229">hello i den följande tabellen beskrivs hello element i hello formulär:</span><span class="sxs-lookup"><span data-stu-id="bbfcc-229">hello following table describes hello elements on hello form:</span></span>

    | <span data-ttu-id="bbfcc-230">Egenskap</span><span class="sxs-lookup"><span data-stu-id="bbfcc-230">Property</span></span> | <span data-ttu-id="bbfcc-231">Värde</span><span class="sxs-lookup"><span data-stu-id="bbfcc-231">Value</span></span> |
    | --- | --- |
    | <span data-ttu-id="bbfcc-232">Välj ett skript</span><span class="sxs-lookup"><span data-stu-id="bbfcc-232">Select a script</span></span> | <span data-ttu-id="bbfcc-233">toouse egna skript väljer __anpassad__.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-233">toouse your own script, select __Custom__.</span></span> <span data-ttu-id="bbfcc-234">Annars väljer du ett av hello tillhandahålls skript.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-234">Otherwise, select one of hello provided scripts.</span></span> |
    | <span data-ttu-id="bbfcc-235">Namn</span><span class="sxs-lookup"><span data-stu-id="bbfcc-235">Name</span></span> |<span data-ttu-id="bbfcc-236">Ange ett namn för hello skriptåtgärder.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-236">Specify a name for hello script action.</span></span> |
    | <span data-ttu-id="bbfcc-237">Bash-skript URI</span><span class="sxs-lookup"><span data-stu-id="bbfcc-237">Bash script URI</span></span> |<span data-ttu-id="bbfcc-238">Ange hello URI toohello skript som är anropade toocustomize hello-kluster.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-238">Specify hello URI toohello script that is invoked toocustomize hello cluster.</span></span> |
    | <span data-ttu-id="bbfcc-239">Zookeeper-HEAD/Worker</span><span class="sxs-lookup"><span data-stu-id="bbfcc-239">Head/Worker/Zookeeper</span></span> |<span data-ttu-id="bbfcc-240">Ange hello noder (**Head**, **Worker**, eller **ZooKeeper**) på vilken hello anpassning skriptet körs.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-240">Specify hello nodes (**Head**, **Worker**, or **ZooKeeper**) on which hello customization script is run.</span></span> |
    | <span data-ttu-id="bbfcc-241">Parametrar</span><span class="sxs-lookup"><span data-stu-id="bbfcc-241">Parameters</span></span> |<span data-ttu-id="bbfcc-242">Ange hello parametrar, om det krävs av hello skript.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-242">Specify hello parameters, if required by hello script.</span></span> |

    <span data-ttu-id="bbfcc-243">Använd hello __spara den här skriptåtgärden__ post tooensure som hello skript används under skalning åtgärder.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-243">Use hello __Persist this script action__ entry tooensure that hello script is applied during scaling operations.</span></span>

5. <span data-ttu-id="bbfcc-244">Välj __skapa__ toosave hello skript.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-244">Select __Create__ toosave hello script.</span></span> <span data-ttu-id="bbfcc-245">Du kan sedan använda __+ skicka nya__ tooadd ett annat skript.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-245">You can then use __+ Submit new__ tooadd another script.</span></span>

    ![Flera skriptåtgärder](./media/hdinsight-hadoop-customize-cluster-linux/multiple-scripts.png)

    <span data-ttu-id="bbfcc-247">När du är klar att lägga till skript använder hello __Välj__ knappen och sedan hello __nästa__ knappen tooreturn toohello __klustret sammanfattning__ avsnitt.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-247">When you are done adding scripts, use hello __Select__ button, and then hello __Next__ button tooreturn toohello __Cluster summary__ section.</span></span>

3. <span data-ttu-id="bbfcc-248">toocreate hello kluster, Välj __skapa__ från hello __klustret sammanfattning__ val.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-248">toocreate hello cluster, select __Create__ from hello __Cluster summary__ selection.</span></span>

### <a name="use-a-script-action-from-azure-resource-manager-templates"></a><span data-ttu-id="bbfcc-249">Använd en skriptåtgärd från Azure Resource Manager-mallar</span><span class="sxs-lookup"><span data-stu-id="bbfcc-249">Use a Script Action from Azure Resource Manager templates</span></span>

<span data-ttu-id="bbfcc-250">hello exemplen i det här avsnittet visar hur toouse skript åtgärder med Azure Resource Manager-mallar.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-250">hello examples in this section demonstrate how toouse script actions with Azure Resource Manager templates.</span></span>

#### <a name="before-you-begin"></a><span data-ttu-id="bbfcc-251">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="bbfcc-251">Before you begin</span></span>

* <span data-ttu-id="bbfcc-252">Information om hur du konfigurerar en arbetsstation toorun HDInsight Powershell-cmdlets finns i [installera och konfigurera Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="bbfcc-252">For information about configuring a workstation toorun HDInsight Powershell cmdlets, see [Install and configure Azure PowerShell](/powershell/azure/overview).</span></span>
* <span data-ttu-id="bbfcc-253">Anvisningar för hur toocreate mallar, se [redigera Azure Resource Manager-mallar](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="bbfcc-253">For instructions on how toocreate templates, see [Authoring Azure Resource Manager templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="bbfcc-254">Om du inte har använt Azure PowerShell med Resource Manager, se [med hjälp av Azure PowerShell med Azure Resource Manager](../azure-resource-manager/powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="bbfcc-254">If you have not previously used Azure PowerShell with Resource Manager, see [Using Azure PowerShell with Azure Resource Manager](../azure-resource-manager/powershell-azure-resource-manager.md).</span></span>

#### <a name="create-clusters-using-script-action"></a><span data-ttu-id="bbfcc-255">Skapa kluster med skriptåtgärder</span><span class="sxs-lookup"><span data-stu-id="bbfcc-255">Create clusters using Script Action</span></span>

1. <span data-ttu-id="bbfcc-256">Kopiera hello följande mall tooa plats på datorn.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-256">Copy hello following template tooa location on your computer.</span></span> <span data-ttu-id="bbfcc-257">Den här mallen installerar Giraph på hello headnodes- och arbetsroller noder i klustret hello.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-257">This template installs Giraph on hello headnodes and worker nodes in hello cluster.</span></span> <span data-ttu-id="bbfcc-258">Du kan också kontrollera om hello JSON-mall är giltig.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-258">You can also verify if hello JSON template is valid.</span></span> <span data-ttu-id="bbfcc-259">Klistra in innehållet i mallen [JSONLint](http://jsonlint.com/), ett online JSON-verktyget för verifiering.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-259">Paste your template content into [JSONLint](http://jsonlint.com/), an online JSON validation tool.</span></span>

            {
            "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
            "contentVersion": "1.0.0.0",
            "parameters": {
                "clusterLocation": {
                    "type": "string",
                    "defaultValue": "West US",
                    "allowedValues": [ "West US" ]
                },
                "clusterName": {
                    "type": "string"
                },
                "clusterUserName": {
                    "type": "string",
                    "defaultValue": "admin"
                },
                "clusterUserPassword": {
                    "type": "securestring"
                },
                "sshUserName": {
                    "type": "string",
                    "defaultValue": "username"
                },
                "sshPassword": {
                    "type": "securestring"
                },
                "clusterStorageAccountName": {
                    "type": "string"
                },
                "clusterStorageAccountResourceGroup": {
                    "type": "string"
                },
                "clusterStorageType": {
                    "type": "string",
                    "defaultValue": "Standard_LRS",
                    "allowedValues": [
                        "Standard_LRS",
                        "Standard_GRS",
                        "Standard_ZRS"
                    ]
                },
                "clusterStorageAccountContainer": {
                    "type": "string"
                },
                "clusterHeadNodeCount": {
                    "type": "int",
                    "defaultValue": 1
                },
                "clusterWorkerNodeCount": {
                    "type": "int",
                    "defaultValue": 2
                }
            },
            "variables": {
            },
            "resources": [
                {
                    "name": "[parameters('clusterStorageAccountName')]",
                    "type": "Microsoft.Storage/storageAccounts",
                    "location": "[parameters('clusterLocation')]",
                    "apiVersion": "2015-05-01-preview",
                    "dependsOn": [ ],
                    "tags": { },
                    "properties": {
                        "accountType": "[parameters('clusterStorageType')]"
                    }
                },
                {
                    "name": "[parameters('clusterName')]",
                    "type": "Microsoft.HDInsight/clusters",
                    "location": "[parameters('clusterLocation')]",
                    "apiVersion": "2015-03-01-preview",
                    "dependsOn": [
                        "[concat('Microsoft.Storage/storageAccounts/', parameters('clusterStorageAccountName'))]"
                    ],
                    "tags": { },
                    "properties": {
                        "clusterVersion": "3.2",
                        "osType": "Linux",
                        "clusterDefinition": {
                            "kind": "hadoop",
                            "configurations": {
                                "gateway": {
                                    "restAuthCredential.isEnabled": true,
                                    "restAuthCredential.username": "[parameters('clusterUserName')]",
                                    "restAuthCredential.password": "[parameters('clusterUserPassword')]"
                                }
                            }
                        },
                        "storageProfile": {
                            "storageaccounts": [
                                {
                                    "name": "[concat(parameters('clusterStorageAccountName'),'.blob.core.windows.net')]",
                                    "isDefault": true,
                                    "container": "[parameters('clusterStorageAccountContainer')]",
                                    "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('clusterStorageAccountName')), '2015-05-01-preview').key1]"
                                }
                            ]
                        },
                        "computeProfile": {
                            "roles": [
                                {
                                    "name": "headnode",
                                    "targetInstanceCount": "[parameters('clusterHeadNodeCount')]",
                                    "hardwareProfile": {
                                        "vmSize": "Large"
                                    },
                                    "osProfile": {
                                        "linuxOperatingSystemProfile": {
                                            "username": "[parameters('sshUserName')]",
                                            "password": "[parameters('sshPassword')]"
                                        }
                                    },
                                    "scriptActions": [
                                        {
                                            "name": "installGiraph",
                                            "uri": "https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh",
                                            "parameters": ""
                                        }
                                    ]
                                },
                                {
                                    "name": "workernode",
                                    "targetInstanceCount": "[parameters('clusterWorkerNodeCount')]",
                                    "hardwareProfile": {
                                        "vmSize": "Large"
                                    },
                                    "osProfile": {
                                        "linuxOperatingSystemProfile": {
                                            "username": "[parameters('sshUserName')]",
                                            "password": "[parameters('sshPassword')]"
                                        }
                                    },
                                    "scriptActions": [
                                        {
                                            "name": "installR",
                                            "uri": "https://hdiconfigactions.blob.core.windows.net/linuxrconfigactionv01/r-installer-v01.sh",
                                            "parameters": ""
                                        }
                                    ]
                                }
                            ]
                        }
                    }
                }
            ],
            "outputs": {
                "cluster":{
                    "type" : "object",
                    "value" : "[reference(resourceId('Microsoft.HDInsight/clusters',parameters('clusterName')))]"
                }
            }
        }
2. <span data-ttu-id="bbfcc-260">Starta Azure PowerShell och logga in tooyour Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-260">Start Azure PowerShell and Log in tooyour Azure account.</span></span> <span data-ttu-id="bbfcc-261">När du har angett dina autentiseringsuppgifter returnerar hello kommando information om ditt konto.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-261">After providing your credentials, hello command returns information about your account.</span></span>

        Add-AzureRmAccount

        Id                             Type       ...
        --                             ----
        someone@example.com            User       ...
3. <span data-ttu-id="bbfcc-262">Om du har flera prenumerationer, ange hello prenumerations-ID som du vill att toouse för distribution.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-262">If you have multiple subscriptions, provide hello subscription ID you wish toouse for deployment.</span></span>

        Select-AzureRmSubscription -SubscriptionID <YourSubscriptionId>

    > [!NOTE]
    > <span data-ttu-id="bbfcc-263">Du kan använda `Get-AzureRmSubscription` tooget en lista över alla prenumerationer som är kopplade till ditt konto, vilket inkluderar hello prenumerations-ID för varje kriterium.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-263">You can use `Get-AzureRmSubscription` tooget a list of all subscriptions associated with your account, which includes hello subscription ID for each one.</span></span>

4. <span data-ttu-id="bbfcc-264">Om du inte har en befintlig resursgrupp kan du skapa en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-264">If you do not have an existing resource group, create a resource group.</span></span> <span data-ttu-id="bbfcc-265">Ange hello namnet för hello resursgrupp och plats som du behöver för din lösning.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-265">Provide hello name of hello resource group and location that you need for your solution.</span></span> <span data-ttu-id="bbfcc-266">En sammanfattning av hello ny resursgrupp returneras.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-266">A summary of hello new resource group is returned.</span></span>

        New-AzureRmResourceGroup -Name myresourcegroup -Location "West US"

        ResourceGroupName : myresourcegroup
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
                            Actions  NotActions
                            =======  ==========
                            *
        ResourceId        : /subscriptions/######/resourceGroups/ExampleResourceGroup

5. <span data-ttu-id="bbfcc-267">toocreate en distribution för din resursgrupp kör hello **ny AzureRmResourceGroupDeployment** kommando och ange hello nödvändiga parametrar.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-267">toocreate a deployment for your resource group, run hello **New-AzureRmResourceGroupDeployment** command and provide hello necessary parameters.</span></span> <span data-ttu-id="bbfcc-268">hello parametrarna inkluderar hello följande data:</span><span class="sxs-lookup"><span data-stu-id="bbfcc-268">hello parameters include hello following data:</span></span>

    * <span data-ttu-id="bbfcc-269">Ett namn för din distribution</span><span class="sxs-lookup"><span data-stu-id="bbfcc-269">A name for your deployment</span></span>
    * <span data-ttu-id="bbfcc-270">hello namnet på resursgruppen</span><span class="sxs-lookup"><span data-stu-id="bbfcc-270">hello name of your resource group</span></span>
    * <span data-ttu-id="bbfcc-271">hello sökväg eller URL toohello mall du skapade.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-271">hello path or URL toohello template you created.</span></span>

  <span data-ttu-id="bbfcc-272">Om mallen kräver parametrar, måste du skicka dessa parametrar samt.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-272">If your template requires any parameters, you must pass those parameters as well.</span></span> <span data-ttu-id="bbfcc-273">I det här fallet kräver hello skript åtgärd tooinstall R på hello klustret inte några parametrar.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-273">In this case, hello script action tooinstall R on hello cluster does not require any parameters.</span></span>

        New-AzureRmResourceGroupDeployment -Name mydeployment -ResourceGroupName myresourcegroup -TemplateFile <PathOrLinkToTemplate>

    <span data-ttu-id="bbfcc-274">Du kan ange tooprovide värden för hello parametrar som definierats i mallen för hello.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-274">You are prompted tooprovide values for hello parameters defined in hello template.</span></span>

1. <span data-ttu-id="bbfcc-275">När hello resursgruppen har distribuerats, visas en sammanfattning av hello-distribution.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-275">When hello resource group has been deployed, a summary of hello deployment is displayed.</span></span>

          DeploymentName    : mydeployment
          ResourceGroupName : myresourcegroup
          ProvisioningState : Succeeded
          Timestamp         : 8/14/2017 7:00:27 PM
          Mode              : Incremental
          ...

2. <span data-ttu-id="bbfcc-276">Du kan använda hello följande cmdlets tooget information om hello om distributionen misslyckas.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-276">If your deployment fails, you can use hello following cmdlets tooget information about hello failures.</span></span>

        Get-AzureRmResourceGroupDeployment -ResourceGroupName myresourcegroup -ProvisioningState Failed

### <a name="use-a-script-action-during-cluster-creation-from-azure-powershell"></a><span data-ttu-id="bbfcc-277">Använd en skriptåtgärd när klustret skapas från Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="bbfcc-277">Use a Script Action during cluster creation from Azure PowerShell</span></span>

<span data-ttu-id="bbfcc-278">I det här avsnittet ska vi använda hello [Lägg till AzureRmHDInsightScriptAction](https://msdn.microsoft.com/library/mt603527.aspx) cmdlet tooinvoke skript med hjälp av skriptåtgärder toocustomize ett kluster.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-278">In this section, we use hello [Add-AzureRmHDInsightScriptAction](https://msdn.microsoft.com/library/mt603527.aspx) cmdlet tooinvoke scripts by using Script Action toocustomize a cluster.</span></span> <span data-ttu-id="bbfcc-279">Innan du fortsätter bör du kontrollera att du har installerat och konfigurerat Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-279">Before proceeding, make sure you have installed and configured Azure PowerShell.</span></span> <span data-ttu-id="bbfcc-280">Information om hur du konfigurerar en arbetsstation toorun HDInsight PowerShell-cmdlets finns i [installera och konfigurera Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="bbfcc-280">For information about configuring a workstation toorun HDInsight PowerShell cmdlets, see [Install and configure Azure PowerShell](/powershell/azure/overview).</span></span>

<span data-ttu-id="bbfcc-281">hello följande skript visar hur tooapply skriptåtgärder när du skapar ett kluster med hjälp av PowerShell:</span><span class="sxs-lookup"><span data-stu-id="bbfcc-281">hello following script demonstrates how tooapply a script action when creating a cluster using PowerShell:</span></span>

[!code-powershell[main](../../powershell_scripts/hdinsight/use-script-action/use-script-action.ps1?range=5-90)]

<span data-ttu-id="bbfcc-282">Det kan ta flera minuter innan hello klustret har skapats.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-282">It can take several minutes before hello cluster is created.</span></span>

### <a name="use-a-script-action-during-cluster-creation-from-hello-hdinsight-net-sdk"></a><span data-ttu-id="bbfcc-283">Använd en skriptåtgärd när klustret skapas från hello HDInsight .NET SDK</span><span class="sxs-lookup"><span data-stu-id="bbfcc-283">Use a Script Action during cluster creation from hello HDInsight .NET SDK</span></span>

<span data-ttu-id="bbfcc-284">Hej HDInsight .NET SDK innehåller klientbibliotek som gör det enklare toowork med HDInsight från ett .NET-program.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-284">hello HDInsight .NET SDK provides client libraries that makes it easier toowork with HDInsight from a .NET application.</span></span> <span data-ttu-id="bbfcc-285">Kodexempel är finns [skapa Linux-baserade kluster i HDInsight med hello .NET SDK](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md#use-script-action).</span><span class="sxs-lookup"><span data-stu-id="bbfcc-285">For a code sample, see [Create Linux-based clusters in HDInsight using hello .NET SDK](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md#use-script-action).</span></span>

## <a name="apply-a-script-action-tooa-running-cluster"></a><span data-ttu-id="bbfcc-286">Tillämpa en skriptåtgärd tooa kör kluster</span><span class="sxs-lookup"><span data-stu-id="bbfcc-286">Apply a Script Action tooa running cluster</span></span>

<span data-ttu-id="bbfcc-287">I det här avsnittet lär du dig hur tooapply skript åtgärder tooa kör klustret.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-287">In this section, learn how tooapply script actions tooa running cluster.</span></span>

### <a name="apply-a-script-action-tooa-running-cluster-from-hello-azure-portal"></a><span data-ttu-id="bbfcc-288">Tillämpa en skriptåtgärd tooa kör kluster från hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="bbfcc-288">Apply a Script Action tooa running cluster from hello Azure portal</span></span>

1. <span data-ttu-id="bbfcc-289">Från hello [Azure-portalen](https://portal.azure.com), Välj ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-289">From hello [Azure portal](https://portal.azure.com), select your HDInsight cluster.</span></span>

2. <span data-ttu-id="bbfcc-290">Översikt för hello HDInsight-kluster, Välj hello **skriptåtgärder** panelen.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-290">From hello HDInsight cluster overview, select hello **Script Actions** tile.</span></span>

    ![Skriptet åtgärder sida vid sida](./media/hdinsight-hadoop-customize-cluster-linux/scriptactionstile.png)

   > [!NOTE]
   > <span data-ttu-id="bbfcc-292">Du kan också välja **alla inställningar** och välj sedan **skriptåtgärder** från hello inställningar.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-292">You can also select **All settings** and then select **Script Actions** from hello Settings section.</span></span>

3. <span data-ttu-id="bbfcc-293">Välj hello överkant hello skriptåtgärder avsnittet **skicka nya**.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-293">From hello top of hello Script Actions section, select **Submit new**.</span></span>

    ![Lägg till ett skript tooa kör kluster](./media/hdinsight-hadoop-customize-cluster-linux/add-script-running-cluster.png)

4. <span data-ttu-id="bbfcc-295">Använd hello __väljer du ett skript__ post tooselect en fördefinierad skript.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-295">Use hello __Select a script__ entry tooselect a pre-made script.</span></span> <span data-ttu-id="bbfcc-296">Välj toouse ett anpassat skript __anpassade__ och ange sedan hello __namn__ och __Bash skript URI__ för skriptet.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-296">toouse a custom script, select __Custom__ and then provide hello __Name__ and __Bash script URI__ for your script.</span></span>

    ![Lägga till ett skript i hello väljer skriptform](./media/hdinsight-hadoop-customize-cluster-linux/select-script.png)

    <span data-ttu-id="bbfcc-298">hello i den följande tabellen beskrivs hello element i hello formulär:</span><span class="sxs-lookup"><span data-stu-id="bbfcc-298">hello following table describes hello elements on hello form:</span></span>

    | <span data-ttu-id="bbfcc-299">Egenskap</span><span class="sxs-lookup"><span data-stu-id="bbfcc-299">Property</span></span> | <span data-ttu-id="bbfcc-300">Värde</span><span class="sxs-lookup"><span data-stu-id="bbfcc-300">Value</span></span> |
    | --- | --- |
    | <span data-ttu-id="bbfcc-301">Välj ett skript</span><span class="sxs-lookup"><span data-stu-id="bbfcc-301">Select a script</span></span> | <span data-ttu-id="bbfcc-302">toouse egna skript väljer __anpassade__.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-302">toouse your own script, select __custom__.</span></span> <span data-ttu-id="bbfcc-303">Välj annars en skriptet.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-303">Otherwise, select a provided script.</span></span> |
    | <span data-ttu-id="bbfcc-304">Namn</span><span class="sxs-lookup"><span data-stu-id="bbfcc-304">Name</span></span> |<span data-ttu-id="bbfcc-305">Ange ett namn för hello skriptåtgärder.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-305">Specify a name for hello script action.</span></span> |
    | <span data-ttu-id="bbfcc-306">Bash-skript URI</span><span class="sxs-lookup"><span data-stu-id="bbfcc-306">Bash script URI</span></span> |<span data-ttu-id="bbfcc-307">Ange hello URI toohello skript som är anropade toocustomize hello-kluster.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-307">Specify hello URI toohello script that is invoked toocustomize hello cluster.</span></span> |
    | <span data-ttu-id="bbfcc-308">Zookeeper-HEAD/Worker</span><span class="sxs-lookup"><span data-stu-id="bbfcc-308">Head/Worker/Zookeeper</span></span> |<span data-ttu-id="bbfcc-309">Ange hello noder (**Head**, **Worker**, eller **ZooKeeper**) på vilken hello anpassning skriptet körs.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-309">Specify hello nodes (**Head**, **Worker**, or **ZooKeeper**) on which hello customization script is run.</span></span> |
    | <span data-ttu-id="bbfcc-310">Parametrar</span><span class="sxs-lookup"><span data-stu-id="bbfcc-310">Parameters</span></span> |<span data-ttu-id="bbfcc-311">Ange hello parametrar, om det krävs av hello skript.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-311">Specify hello parameters, if required by hello script.</span></span> |

    <span data-ttu-id="bbfcc-312">Använd hello __spara den här skriptåtgärden__ post toomake att hello skript används under skalning åtgärder.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-312">Use hello __Persist this script action__ entry toomake sure hello script is applied during scaling operations.</span></span>

5. <span data-ttu-id="bbfcc-313">Använd slutligen hello **skapa** knappen tooapply hello skriptet toohello klustret.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-313">Finally, use hello **Create** button tooapply hello script toohello cluster.</span></span>

### <a name="apply-a-script-action-tooa-running-cluster-from-azure-powershell"></a><span data-ttu-id="bbfcc-314">Tillämpa en skriptåtgärd tooa köra klustret från Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="bbfcc-314">Apply a Script Action tooa running cluster from Azure PowerShell</span></span>

<span data-ttu-id="bbfcc-315">Innan du fortsätter bör du kontrollera att du har installerat och konfigurerat Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-315">Before proceeding, make sure you have installed and configured Azure PowerShell.</span></span> <span data-ttu-id="bbfcc-316">Information om hur du konfigurerar en arbetsstation toorun HDInsight PowerShell-cmdlets finns i [installera och konfigurera Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="bbfcc-316">For information about configuring a workstation toorun HDInsight PowerShell cmdlets, see [Install and configure Azure PowerShell](/powershell/azure/overview).</span></span>

<span data-ttu-id="bbfcc-317">hello exemplet nedan visar hur tooapply ett skript åtgärd tooa kluster som körs:</span><span class="sxs-lookup"><span data-stu-id="bbfcc-317">hello following example demonstrates how tooapply a script action tooa running cluster:</span></span>

[!code-powershell[main](../../powershell_scripts/hdinsight/use-script-action/use-script-action.ps1?range=105-117)]

<span data-ttu-id="bbfcc-318">När hello-åtgärden har slutförts får du information liknande toohello följande text:</span><span class="sxs-lookup"><span data-stu-id="bbfcc-318">Once hello operation completes, you receive information similar toohello following text:</span></span>

    OperationState  : Succeeded
    ErrorMessage    :
    Name            : Giraph
    Uri             : https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh
    Parameters      :
    NodeTypes       : {HeadNode, WorkerNode}

### <a name="apply-a-script-action-tooa-running-cluster-from-hello-azure-cli"></a><span data-ttu-id="bbfcc-319">Tillämpa en skriptåtgärd tooa kör kluster från hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="bbfcc-319">Apply a Script Action tooa running cluster from hello Azure CLI</span></span>

<span data-ttu-id="bbfcc-320">Innan du fortsätter bör du kontrollera att du har installerat och konfigurerat hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-320">Before proceeding, make sure you have installed and configured hello Azure CLI.</span></span> <span data-ttu-id="bbfcc-321">Mer information finns i [installera hello Azure CLI](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="bbfcc-321">For more information, see [Install hello Azure CLI](../cli-install-nodejs.md).</span></span>

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

1. <span data-ttu-id="bbfcc-322">tooswitch tooAzure Resource Manager-läge med följande kommando på kommandoraden för hello hello:</span><span class="sxs-lookup"><span data-stu-id="bbfcc-322">tooswitch tooAzure Resource Manager mode, use hello following command at hello command line:</span></span>

        azure config mode arm

2. <span data-ttu-id="bbfcc-323">Använd hello följande tooauthenticate tooyour Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-323">Use hello following tooauthenticate tooyour Azure subscription.</span></span>

        azure login

3. <span data-ttu-id="bbfcc-324">Använd hello efter kommandot tooapply ett skript åtgärd tooa kör kluster</span><span class="sxs-lookup"><span data-stu-id="bbfcc-324">Use hello following command tooapply a script action tooa running cluster</span></span>

        azure hdinsight script-action create <clustername> -g <resourcegroupname> -n <scriptname> -u <scriptURI> -t <nodetypes>

    <span data-ttu-id="bbfcc-325">Om du utelämnar parametrar för kommandot uppmanas för dessa.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-325">If you omit parameters for this command, you are prompted for them.</span></span> <span data-ttu-id="bbfcc-326">Om hello skript som du anger med `-u` accepterar parametrar, kan du ange dem med hello `-p` parameter.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-326">If hello script you specify with `-u` accepts parameters, you can specify them using hello `-p` parameter.</span></span>

    <span data-ttu-id="bbfcc-327">Giltig nodtyper är `headnode`, `workernode`, och `zookeeper`.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-327">Valid node types are `headnode`, `workernode`, and `zookeeper`.</span></span> <span data-ttu-id="bbfcc-328">Om hello skript ska vara tillämpade toomultiple nodtyper, ange hello typer avgränsade med ett ';'.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-328">If hello script should be applied toomultiple node types, specify hello types separated by a ';'.</span></span> <span data-ttu-id="bbfcc-329">Till exempel `-n headnode;workernode`.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-329">For example, `-n headnode;workernode`.</span></span>

    <span data-ttu-id="bbfcc-330">toopersist hello skript, lägga till hello `--persistOnSuccess`.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-330">toopersist hello script, add hello `--persistOnSuccess`.</span></span> <span data-ttu-id="bbfcc-331">Du kan också spara skriptet hello senare med hjälp av `azure hdinsight script-action persisted set`.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-331">You can also persist hello script later by using `azure hdinsight script-action persisted set`.</span></span>

    <span data-ttu-id="bbfcc-332">När hello jobbet har slutförts visas utdata liknande toohello följande text:</span><span class="sxs-lookup"><span data-stu-id="bbfcc-332">Once hello job completes, you receive output similar toohello following text:</span></span>

        info:    Executing command hdinsight script-action create
        + Executing Script Action on HDInsight cluster
        data:    Operation Info
        data:    ---------------
        data:    Operation status:
        data:    Operation ID:  b707b10e-e633-45c0-baa9-8aed3d348c13
        info:    hdinsight script-action create command OK

### <a name="apply-a-script-action-tooa-running-cluster-using-rest-api"></a><span data-ttu-id="bbfcc-333">Tillämpa en skriptåtgärd tooa kör kluster med hjälp av REST API</span><span class="sxs-lookup"><span data-stu-id="bbfcc-333">Apply a Script Action tooa running cluster using REST API</span></span>

<span data-ttu-id="bbfcc-334">Se [kör skriptåtgärder på ett kluster som körs](https://msdn.microsoft.com/library/azure/mt668441.aspx).</span><span class="sxs-lookup"><span data-stu-id="bbfcc-334">See [Run Script Actions on a running cluster](https://msdn.microsoft.com/library/azure/mt668441.aspx).</span></span>

### <a name="apply-a-script-action-tooa-running-cluster-from-hello-hdinsight-net-sdk"></a><span data-ttu-id="bbfcc-335">Tillämpa en skriptåtgärd tooa kör kluster från hello HDInsight .NET SDK</span><span class="sxs-lookup"><span data-stu-id="bbfcc-335">Apply a Script Action tooa running cluster from hello HDInsight .NET SDK</span></span>

<span data-ttu-id="bbfcc-336">Ett exempel på hur hello .NET SDK tooapply skript tooa klustret finns [https://github.com/Azure-Samples/hdinsight-dotnet-script-action](https://github.com/Azure-Samples/hdinsight-dotnet-script-action).</span><span class="sxs-lookup"><span data-stu-id="bbfcc-336">For an example of using hello .NET SDK tooapply scripts tooa cluster, see [https://github.com/Azure-Samples/hdinsight-dotnet-script-action](https://github.com/Azure-Samples/hdinsight-dotnet-script-action).</span></span>

## <a name="view-history-promote-and-demote-script-actions"></a><span data-ttu-id="bbfcc-337">Visa historik, uppgradera och degradera skriptåtgärder</span><span class="sxs-lookup"><span data-stu-id="bbfcc-337">View history, promote, and demote Script Actions</span></span>

### <a name="using-hello-azure-portal"></a><span data-ttu-id="bbfcc-338">Med hjälp av hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="bbfcc-338">Using hello Azure portal</span></span>

1. <span data-ttu-id="bbfcc-339">Från hello [Azure-portalen](https://portal.azure.com), Välj ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-339">From hello [Azure portal](https://portal.azure.com), select your HDInsight cluster.</span></span>

2. <span data-ttu-id="bbfcc-340">Översikt för hello HDInsight-kluster, Välj hello **skriptåtgärder** panelen.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-340">From hello HDInsight cluster overview, select hello **Script Actions** tile.</span></span>

    ![Skriptet åtgärder sida vid sida](./media/hdinsight-hadoop-customize-cluster-linux/scriptactionstile.png)

   > [!NOTE]
   > <span data-ttu-id="bbfcc-342">Du kan också välja **alla inställningar** och välj sedan **skriptåtgärder** från hello inställningar.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-342">You can also select **All settings** and then select **Script Actions** from hello Settings section.</span></span>

4. <span data-ttu-id="bbfcc-343">En historik över skript för det här klustret visas på hello skriptåtgärder avsnitt.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-343">A history of scripts for this cluster is displayed on hello Script Actions section.</span></span> <span data-ttu-id="bbfcc-344">Den här informationen innehåller en lista över bestående skript.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-344">This information includes a list of persisted scripts.</span></span> <span data-ttu-id="bbfcc-345">I hello skärmbilden nedan ser du att hello Solr skriptet har körts på det här klustret.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-345">In hello screenshot below, you can see that hello Solr script has been ran on this cluster.</span></span> <span data-ttu-id="bbfcc-346">hello skärmbild visar inte alla beständiga skript.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-346">hello screenshot does not show any persisted scripts.</span></span>

    ![Script Actions-avsnitt](./media/hdinsight-hadoop-customize-cluster-linux/script-action-history.png)

5. <span data-ttu-id="bbfcc-348">Om du väljer ett skript från hello historiken visas hello avsnittet för egenskaper för det här skriptet.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-348">Selecting a script from hello history displays hello Properties section for this script.</span></span> <span data-ttu-id="bbfcc-349">Hello överkant hello-skärmen, kan du kör hello skript eller befordra.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-349">From hello top of hello screen, you can rerun hello script or promote it.</span></span>

    ![Åtgärder skriptegenskaper](./media/hdinsight-hadoop-customize-cluster-linux/promote-script-actions.png)

6. <span data-ttu-id="bbfcc-351">Du kan också använda hello **...**  toohello höger på poster i hello skriptåtgärder avsnittet tooperform åtgärder.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-351">You can also use hello **...** toohello right of entries on hello Script Actions section tooperform actions.</span></span>

    ![Script åtgärder... användning](./media/hdinsight-hadoop-customize-cluster-linux/deletepromoted.png)

### <a name="using-azure-powershell"></a><span data-ttu-id="bbfcc-353">Använda Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="bbfcc-353">Using Azure PowerShell</span></span>

| <span data-ttu-id="bbfcc-354">Använd följande hello...</span><span class="sxs-lookup"><span data-stu-id="bbfcc-354">Use hello following...</span></span> | <span data-ttu-id="bbfcc-355">för...</span><span class="sxs-lookup"><span data-stu-id="bbfcc-355">too...</span></span> |
| --- | --- |
| <span data-ttu-id="bbfcc-356">Get-AzureRmHDInsightPersistedScriptAction</span><span class="sxs-lookup"><span data-stu-id="bbfcc-356">Get-AzureRmHDInsightPersistedScriptAction</span></span> |<span data-ttu-id="bbfcc-357">Hämta information om beständiga skriptåtgärder</span><span class="sxs-lookup"><span data-stu-id="bbfcc-357">Retrieve information on persisted script actions</span></span> |
| <span data-ttu-id="bbfcc-358">Get-AzureRmHDInsightScriptActionHistory</span><span class="sxs-lookup"><span data-stu-id="bbfcc-358">Get-AzureRmHDInsightScriptActionHistory</span></span> |<span data-ttu-id="bbfcc-359">Hämta en historik över skript åtgärder tillämpas toohello kluster eller information för ett visst skript</span><span class="sxs-lookup"><span data-stu-id="bbfcc-359">Retrieve a history of script actions applied toohello cluster, or details for a specific script</span></span> |
| <span data-ttu-id="bbfcc-360">Set-AzureRmHDInsightPersistedScriptAction</span><span class="sxs-lookup"><span data-stu-id="bbfcc-360">Set-AzureRmHDInsightPersistedScriptAction</span></span> |<span data-ttu-id="bbfcc-361">Flyttar upp en ad hoc-skriptet åtgärd tooa beständiga skriptåtgärder</span><span class="sxs-lookup"><span data-stu-id="bbfcc-361">Promotes an ad hoc script action tooa persisted script action</span></span> |
| <span data-ttu-id="bbfcc-362">Remove-AzureRmHDInsightPersistedScriptAction</span><span class="sxs-lookup"><span data-stu-id="bbfcc-362">Remove-AzureRmHDInsightPersistedScriptAction</span></span> |<span data-ttu-id="bbfcc-363">Flyttar ett bestående skript åtgärd tooan ad hoc-åtgärd</span><span class="sxs-lookup"><span data-stu-id="bbfcc-363">Demotes a persisted script action tooan ad hoc action</span></span> |

> [!IMPORTANT]
> <span data-ttu-id="bbfcc-364">Med hjälp av `Remove-AzureRmHDInsightPersistedScriptAction` inte ångra hello-åtgärder som utförs av ett skript.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-364">Using `Remove-AzureRmHDInsightPersistedScriptAction` does not undo hello actions performed by a script.</span></span> <span data-ttu-id="bbfcc-365">Den här cmdleten tar bara bort hello beständiga flaggan.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-365">This cmdlet only removes hello persisted flag.</span></span>

<span data-ttu-id="bbfcc-366">Följande exempelskript hello visas hur du använder hello cmdlets toopromote sedan nedgradera ett skript.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-366">hello following example script demonstrates using hello cmdlets toopromote, then demote a script.</span></span>

[!code-powershell[main](../../powershell_scripts/hdinsight/use-script-action/use-script-action.ps1?range=123-140)]

### <a name="using-hello-azure-cli"></a><span data-ttu-id="bbfcc-367">Med hjälp av hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="bbfcc-367">Using hello Azure CLI</span></span>

| <span data-ttu-id="bbfcc-368">Använd följande hello...</span><span class="sxs-lookup"><span data-stu-id="bbfcc-368">Use hello following...</span></span> | <span data-ttu-id="bbfcc-369">för...</span><span class="sxs-lookup"><span data-stu-id="bbfcc-369">too...</span></span> |
| --- | --- |
| `azure hdinsight script-action persisted list <clustername>` |<span data-ttu-id="bbfcc-370">Hämta en lista över beständiga skriptåtgärder</span><span class="sxs-lookup"><span data-stu-id="bbfcc-370">Retrieve a list of persisted script actions</span></span> |
| `azure hdinsight script-action persisted show <clustername> <scriptname>` |<span data-ttu-id="bbfcc-371">Hämta information om en specifik beständiga skriptåtgärder</span><span class="sxs-lookup"><span data-stu-id="bbfcc-371">Retrieve information on a specific persisted script action</span></span> |
| `azure hdinsight script-action history list <clustername>` |<span data-ttu-id="bbfcc-372">Hämta en historik över skript åtgärder tillämpas toohello kluster</span><span class="sxs-lookup"><span data-stu-id="bbfcc-372">Retrieve a history of script actions applied toohello cluster</span></span> |
| `azure hdinsight script-action history show <clustername> <scriptname>` |<span data-ttu-id="bbfcc-373">Hämta information om en specifik skriptåtgärd</span><span class="sxs-lookup"><span data-stu-id="bbfcc-373">Retrieve information on a specific script action</span></span> |
| `azure hdinsight script action persisted set <clustername> <scriptexecutionid>` |<span data-ttu-id="bbfcc-374">Flyttar upp en ad hoc-skriptet åtgärd tooa beständiga skriptåtgärder</span><span class="sxs-lookup"><span data-stu-id="bbfcc-374">Promotes an ad hoc script action tooa persisted script action</span></span> |
| `azure hdinsight script-action persisted delete <clustername> <scriptname>` |<span data-ttu-id="bbfcc-375">Flyttar ett bestående skript åtgärd tooan ad hoc-åtgärd</span><span class="sxs-lookup"><span data-stu-id="bbfcc-375">Demotes a persisted script action tooan ad hoc action</span></span> |

> [!IMPORTANT]
> <span data-ttu-id="bbfcc-376">Med hjälp av `azure hdinsight script-action persisted delete` inte ångra hello-åtgärder som utförs av ett skript.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-376">Using `azure hdinsight script-action persisted delete` does not undo hello actions performed by a script.</span></span> <span data-ttu-id="bbfcc-377">Den här cmdleten tar bara bort hello beständiga flaggan.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-377">This cmdlet only removes hello persisted flag.</span></span>

### <a name="using-hello-hdinsight-net-sdk"></a><span data-ttu-id="bbfcc-378">Med hjälp av hello HDInsight .NET SDK</span><span class="sxs-lookup"><span data-stu-id="bbfcc-378">Using hello HDInsight .NET SDK</span></span>

<span data-ttu-id="bbfcc-379">Ett exempel på hur hello .NET SDK tooretrieve skript historik från ett kluster, uppgradera eller degradera skript, se [https://github.com/Azure-Samples/hdinsight-dotnet-script-action](https://github.com/Azure-Samples/hdinsight-dotnet-script-action).</span><span class="sxs-lookup"><span data-stu-id="bbfcc-379">For an example of using hello .NET SDK tooretrieve script history from a cluster, promote or demote scripts, see [https://github.com/Azure-Samples/hdinsight-dotnet-script-action](https://github.com/Azure-Samples/hdinsight-dotnet-script-action).</span></span>

> [!NOTE]
> <span data-ttu-id="bbfcc-380">Det här exemplet visar även hur tooinstall ett HDInsight-program med hello .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-380">This example also demonstrates how tooinstall an HDInsight application using hello .NET SDK.</span></span>

## <a name="support-for-open-source-software-used-on-hdinsight-clusters"></a><span data-ttu-id="bbfcc-381">Stöd för öppen källkod programvara som används i HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="bbfcc-381">Support for open-source software used on HDInsight clusters</span></span>

<span data-ttu-id="bbfcc-382">hello Microsoft Azure HDInsight-tjänsten använder ett ekosystem med öppen källkod tekniker formaterat runt Hadoop.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-382">hello Microsoft Azure HDInsight service uses an ecosystem of open-source technologies formed around Hadoop.</span></span> <span data-ttu-id="bbfcc-383">Microsoft Azure tillhandahåller en allmän supportnivå för öppen källkod tekniker.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-383">Microsoft Azure provides a general level of support for open-source technologies.</span></span> <span data-ttu-id="bbfcc-384">Mer information finns i hello **stöd Scope** avsnitt i hello [Azure Support FAQ webbplats](https://azure.microsoft.com/support/faq/).</span><span class="sxs-lookup"><span data-stu-id="bbfcc-384">For more information, see hello **Support Scope** section of hello [Azure Support FAQ website](https://azure.microsoft.com/support/faq/).</span></span> <span data-ttu-id="bbfcc-385">Hej HDInsight-tjänst ger ytterligare en säkerhetsnivå för stöd för inbyggda komponenter.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-385">hello HDInsight service provides an additional level of support for built-in components.</span></span>

<span data-ttu-id="bbfcc-386">Det finns två typer av komponenter som öppen källkod som är tillgängliga i hello HDInsight-tjänst:</span><span class="sxs-lookup"><span data-stu-id="bbfcc-386">There are two types of open-source components that are available in hello HDInsight service:</span></span>

* <span data-ttu-id="bbfcc-387">**Inbyggda komponenter** -komponenterna är förinstallerat på HDInsight-kluster och tillhandahåller huvudfunktionerna i hello klustret.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-387">**Built-in components** - These components are pre-installed on HDInsight clusters and provide core functionality of hello cluster.</span></span> <span data-ttu-id="bbfcc-388">Till exempel tillhör YARN resurshanteraren, hello Hive-frågespråket (HiveQL) och hello Mahout biblioteksresurser toothis kategori.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-388">For example, YARN ResourceManager, hello Hive query language (HiveQL), and hello Mahout library belong toothis category.</span></span> <span data-ttu-id="bbfcc-389">En fullständig lista över komponenter i serverkluster finns i [vad är nytt i hello Hadoop-klusterversioner som tillhandahålls av HDInsight](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="bbfcc-389">A full list of cluster components is available in [What's new in hello Hadoop cluster versions provided by HDInsight](hdinsight-component-versioning.md).</span></span>
* <span data-ttu-id="bbfcc-390">**Anpassade komponenter** -du som användare av hello klustret kan installera eller använda i din arbetsbelastning någon komponent som är tillgängliga i hello community eller skapades av du.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-390">**Custom components** - You, as a user of hello cluster, can install or use in your workload any component available in hello community or created by you.</span></span>

> [!WARNING]
> <span data-ttu-id="bbfcc-391">Komponenter som ingår i hello HDInsight-kluster stöds fullt ut.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-391">Components provided with hello HDInsight cluster are fully supported.</span></span> <span data-ttu-id="bbfcc-392">Microsoft Support hjälper tooisolate och lösa problem relaterade toothese komponenter.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-392">Microsoft Support helps tooisolate and resolve issues related toothese components.</span></span>
>
> <span data-ttu-id="bbfcc-393">Anpassade komponenter får stöd för kommersiellt rimliga toohelp du toofurther hello felsökning.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-393">Custom components receive commercially reasonable support toohelp you toofurther troubleshoot hello issue.</span></span> <span data-ttu-id="bbfcc-394">Microsoft-supporten kanske kan tooresolve hello problemet eller de begära du tooengage tillgängliga kanaler hello öppen källkod tekniker där djup expertis för att teknik finns.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-394">Microsoft support may be able tooresolve hello issue OR they may ask you tooengage available channels for hello open source technologies where deep expertise for that technology is found.</span></span> <span data-ttu-id="bbfcc-395">Det finns till exempel många community-webbplatser som kan användas, t.ex: [MSDN-forum för HDInsight](https://social.msdn.microsoft.com/Forums/azure/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Apache-projekt har också project-webbplatser [http://apache.org](http://apache.org), till exempel: [Hadoop](http://hadoop.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="bbfcc-395">For example, there are many community sites that can be used, like: [MSDN forum for HDInsight](https://social.msdn.microsoft.com/Forums/azure/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Also Apache projects have project sites on [http://apache.org](http://apache.org), for example: [Hadoop](http://hadoop.apache.org/).</span></span>

<span data-ttu-id="bbfcc-396">Hej HDInsight-tjänst finns flera sätt toouse anpassade komponenter.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-396">hello HDInsight service provides several ways toouse custom components.</span></span> <span data-ttu-id="bbfcc-397">hello samma supportnivå gäller oavsett hur en komponent används eller installeras på hello-kluster.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-397">hello same level of support applies, regardless of how a component is used or installed on hello cluster.</span></span> <span data-ttu-id="bbfcc-398">hello följande lista beskrivs hello vanligaste sätten att anpassade komponenter kan användas på HDInsight-kluster:</span><span class="sxs-lookup"><span data-stu-id="bbfcc-398">hello following list describes hello most common ways that custom components can be used on HDInsight clusters:</span></span>

1. <span data-ttu-id="bbfcc-399">Jobbet - Hadoop och andra typer av jobb som kör eller använda anpassade komponenter kan vara skickade toohello.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-399">Job submission - Hadoop or other types of jobs that execute or use custom components can be submitted toohello cluster.</span></span>

2. <span data-ttu-id="bbfcc-400">Anpassning av kluster - när klustret skapas som du kan ange ytterligare inställningar och anpassade komponenter som är installerade på hello klusternoder.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-400">Cluster customization - During cluster creation, you can specify additional settings and custom components that are installed on hello cluster nodes.</span></span>

3. <span data-ttu-id="bbfcc-401">Exempel - för populära anpassade komponenter, Microsoft och andra kan ge exempel på hur du kan använda de här komponenterna på hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-401">Samples - For popular custom components, Microsoft and others may provide samples of how these components can be used on hello HDInsight clusters.</span></span> <span data-ttu-id="bbfcc-402">De här exemplen tillhandahålls utan stöd.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-402">These samples are provided without support.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="bbfcc-403">Felsökning</span><span class="sxs-lookup"><span data-stu-id="bbfcc-403">Troubleshooting</span></span>

<span data-ttu-id="bbfcc-404">Du kan använda Ambari web UI tooview information som loggas av skriptåtgärder.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-404">You can use Ambari web UI tooview information logged by script actions.</span></span> <span data-ttu-id="bbfcc-405">Hello loggar är också tillgängliga i hello standardkontot för lagring som associerats med hello kluster om hello skriptet misslyckas när klustret skapas.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-405">If hello script fails during cluster creation, hello logs are also available in hello default storage account associated with hello cluster.</span></span> <span data-ttu-id="bbfcc-406">Det här avsnittet innehåller information om hur tooretrieve hello loggar med båda alternativen.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-406">This section provides information on how tooretrieve hello logs using both these options.</span></span>

### <a name="using-hello-ambari-web-ui"></a><span data-ttu-id="bbfcc-407">Med hjälp av hello Ambari-Webbgränssnittet</span><span class="sxs-lookup"><span data-stu-id="bbfcc-407">Using hello Ambari Web UI</span></span>

1. <span data-ttu-id="bbfcc-408">Navigera toohttps://CLUSTERNAME.azurehdinsight.net i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-408">In your browser, navigate toohttps://CLUSTERNAME.azurehdinsight.net.</span></span> <span data-ttu-id="bbfcc-409">Ersätt KLUSTERNAMN med hello namnet på ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-409">Replace CLUSTERNAME with hello name of your HDInsight cluster.</span></span>

    <span data-ttu-id="bbfcc-410">När du uppmanas ange hello admin kontonamn (admin) och lösenord för hello klustret.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-410">When prompted, enter hello admin account name (admin) and password for hello cluster.</span></span> <span data-ttu-id="bbfcc-411">Du kan ha administratörsautentiseringsuppgifter för tooreenter hello i ett webbformulär.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-411">You may have tooreenter hello admin credentials in a web form.</span></span>

2. <span data-ttu-id="bbfcc-412">Hello-fältet hello överst på hello sidan Välj hello **ops** post.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-412">From hello bar at hello top of hello page, select hello **ops** entry.</span></span> <span data-ttu-id="bbfcc-413">En lista över aktuella och tidigare åtgärder som utförs på hello klustret via Ambari visas.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-413">A list of current and previous operations performed on hello cluster through Ambari is displayed.</span></span>

    ![Ambari web UI-fältet med ops som valts](./media/hdinsight-hadoop-customize-cluster-linux/ambari-nav.png)

3. <span data-ttu-id="bbfcc-415">Hitta hello poster som har **kör\_customscriptaction** i hello **Operations** kolumn.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-415">Find hello entries that have **run\_customscriptaction** in hello **Operations** column.</span></span> <span data-ttu-id="bbfcc-416">Dessa poster skapas när hello skriptåtgärder kör.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-416">These entries are created when hello Script Actions run.</span></span>

    ![Skärmbild av åtgärder](./media/hdinsight-hadoop-customize-cluster-linux/ambariscriptaction.png)

    <span data-ttu-id="bbfcc-418">tooview hello STDOUT och STDERR utdata, Välj hello run\customscriptaction och detaljvisning hello länkar.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-418">tooview hello STDOUT and STDERR output, select hello run\customscriptaction entry and drill down through hello links.</span></span> <span data-ttu-id="bbfcc-419">Den här utdata skapas när hello skriptet körs, och kan innehålla användbar information.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-419">This output is generated when hello script runs, and may contain useful information.</span></span>

### <a name="access-logs-from-hello-default-storage-account"></a><span data-ttu-id="bbfcc-420">Åtkomstloggar från hello standardkontot för lagring</span><span class="sxs-lookup"><span data-stu-id="bbfcc-420">Access logs from hello default storage account</span></span>

<span data-ttu-id="bbfcc-421">Om hello klustret skapas misslyckas på grund av tooa åtgärd skriptfel kan hello loggar nås från hello klustret storage-konto.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-421">If hello cluster creation fails due tooa script action error, hello logs can be accessed from hello cluster storage account.</span></span>

* <span data-ttu-id="bbfcc-422">hello lagring loggar finns på `\STORAGE_ACCOUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\CLUSTER_NAME\DATE`.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-422">hello storage logs are available at `\STORAGE_ACCOUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\CLUSTER_NAME\DATE`.</span></span>

    ![Skärmbild av åtgärder](./media/hdinsight-hadoop-customize-cluster-linux/script_action_logs_in_storage.png)

    <span data-ttu-id="bbfcc-424">Under den här katalogen ordnas hello loggar separat för headnode, workernode och zookeeper-noder.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-424">Under this directory, hello logs are organized separately for headnode, workernode, and zookeeper nodes.</span></span> <span data-ttu-id="bbfcc-425">Några exempel är:</span><span class="sxs-lookup"><span data-stu-id="bbfcc-425">Some examples are:</span></span>

    * <span data-ttu-id="bbfcc-426">**Headnode** - `<uniqueidentifier>AmbariDb-hn0-<generated_value>.cloudapp.net`</span><span class="sxs-lookup"><span data-stu-id="bbfcc-426">**Headnode** - `<uniqueidentifier>AmbariDb-hn0-<generated_value>.cloudapp.net`</span></span>

    * <span data-ttu-id="bbfcc-427">**Arbetsnoden** - `<uniqueidentifier>AmbariDb-wn0-<generated_value>.cloudapp.net`</span><span class="sxs-lookup"><span data-stu-id="bbfcc-427">**Worker node** - `<uniqueidentifier>AmbariDb-wn0-<generated_value>.cloudapp.net`</span></span>

    * <span data-ttu-id="bbfcc-428">**Zookeeper-nod** - `<uniqueidentifier>AmbariDb-zk0-<generated_value>.cloudapp.net`</span><span class="sxs-lookup"><span data-stu-id="bbfcc-428">**Zookeeper node** - `<uniqueidentifier>AmbariDb-zk0-<generated_value>.cloudapp.net`</span></span>

* <span data-ttu-id="bbfcc-429">Alla stdout och stderr för hello motsvarande värden har överförts toohello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-429">All stdout and stderr of hello corresponding host is uploaded toohello storage account.</span></span> <span data-ttu-id="bbfcc-430">Det finns en **utdata -\*.txt** och **fel -\*.txt** för varje skriptåtgärd.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-430">There is one **output-\*.txt** and **errors-\*.txt** for each script action.</span></span> <span data-ttu-id="bbfcc-431">hello utdata *.txt filen innehåller information om hello URI för hello-skript som får köras på hello värden.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-431">hello output-*.txt file contains information about hello URI of hello script that got run on hello host.</span></span> <span data-ttu-id="bbfcc-432">Exempel</span><span class="sxs-lookup"><span data-stu-id="bbfcc-432">For example</span></span>

        'Start downloading script locally: ', u'https://hdiconfigactions.blob.core.windows.net/linuxrconfigactionv01/r-installer-v01.sh'

* <span data-ttu-id="bbfcc-433">Det är möjligt att du skapar ett skript åtgärd kluster flera gånger med hello samma namn.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-433">It's possible that you repeatedly create a script action cluster with hello same name.</span></span> <span data-ttu-id="bbfcc-434">I så fall kan du skilja hello relevanta loggar baserat på hello datum mappnamn.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-434">In such case, you can distinguish hello relevant logs based on hello DATE folder name.</span></span> <span data-ttu-id="bbfcc-435">Hello mappstrukturen för ett kluster (det här klustret) som skapats på olika datum visas till exempel liknande toohello följande loggposter:</span><span class="sxs-lookup"><span data-stu-id="bbfcc-435">For example, hello folder structure for a cluster (mycluster) created on different dates appears similar toohello following log entries:</span></span>

    <span data-ttu-id="bbfcc-436">`\STORAGE_ACCOUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\mycluster\2015-10-04` `\STORAGE_ACCOUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\mycluster\2015-10-05`</span><span class="sxs-lookup"><span data-stu-id="bbfcc-436">`\STORAGE_ACCOUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\mycluster\2015-10-04` `\STORAGE_ACCOUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\mycluster\2015-10-05`</span></span>

* <span data-ttu-id="bbfcc-437">Om du skapar ett skript åtgärd kluster med hello samma namn på hello samma dag, du kan använda hello unikt prefix tooidentify hello relevanta loggfiler ska kopieras.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-437">If you create a script action cluster with hello same name on hello same day, you can use hello unique prefix tooidentify hello relevant log files.</span></span>

* <span data-ttu-id="bbfcc-438">Om du skapar ett kluster nära 12:00:00 (midnatt), är det möjligt att fördela hello loggfiler mellan två dagar.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-438">If you create a cluster near 12:00AM (midnight), it's possible that hello log files span across two days.</span></span> <span data-ttu-id="bbfcc-439">I sådana fall måste du se två olika datum mappar för hello samma kluster.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-439">In such cases, you see two different date folders for hello same cluster.</span></span>

* <span data-ttu-id="bbfcc-440">Överför loggen filer toohello standardbehållaren kan ta upp too5 minuter, särskilt för stora kluster.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-440">Uploading log files toohello default container can take up too5 mins, especially for large clusters.</span></span> <span data-ttu-id="bbfcc-441">Så om du vill tooaccess hello loggar bör du inte omedelbart bort hello klustret om en skriptåtgärd misslyckas.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-441">So, if you want tooaccess hello logs, you should not immediately delete hello cluster if a script action fails.</span></span>

### <a name="ambari-watchdog"></a><span data-ttu-id="bbfcc-442">Ambari watchdog</span><span class="sxs-lookup"><span data-stu-id="bbfcc-442">Ambari watchdog</span></span>

> [!WARNING]
> <span data-ttu-id="bbfcc-443">Ändra inte hello lösenordet för hello Ambari Watchdog (hdinsightwatchdog) på Linux-baserade HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-443">Do not change hello password for hello Ambari Watchdog (hdinsightwatchdog) on your Linux-based HDInsight cluster.</span></span> <span data-ttu-id="bbfcc-444">Ändra hello lösenordet för detta konto bryts hello möjlighet toorun nya skriptåtgärder på hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-444">Changing hello password for this account breaks hello ability toorun new script actions on hello HDInsight cluster.</span></span>

### <a name="cant-import-name-blobservice"></a><span data-ttu-id="bbfcc-445">Det går inte att importera namn BlobService</span><span class="sxs-lookup"><span data-stu-id="bbfcc-445">Can't import name BlobService</span></span>

<span data-ttu-id="bbfcc-446">__Symptom__: hello skript åtgärden misslyckas.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-446">__Symptoms__: hello script action fails.</span></span> <span data-ttu-id="bbfcc-447">Text liknande toohello följande fel visas när du visar hello åtgärden i Ambari:</span><span class="sxs-lookup"><span data-stu-id="bbfcc-447">Text similar toohello following error is displayed when you view hello operation in Ambari:</span></span>

```
Traceback (most recent call list):
  File "/var/lib/ambari-agent/cache/custom_actions/scripts/run_customscriptaction.py", line 21, in <module>
    from azure.storage.blob import BlobService
ImportError: cannot import name BlobService
```

<span data-ttu-id="bbfcc-448">__Orsak__: det här felet uppstår om du uppgraderar hello Python Azure Storage-klienten som medföljer hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-448">__Cause__: This error occurs if you upgrade hello Python Azure Storage client that is included with hello HDInsight cluster.</span></span> <span data-ttu-id="bbfcc-449">HDInsight förväntar Azure Storage client 0.20.0.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-449">HDInsight expects Azure Storage client 0.20.0.</span></span>

<span data-ttu-id="bbfcc-450">__Lösning__: tooresolve det här felet manuellt ansluta nod tooeach med `ssh` och Använd hello efter kommandot tooreinstall hello rätt lagring klientversion:</span><span class="sxs-lookup"><span data-stu-id="bbfcc-450">__Resolution__: tooresolve this error, manually connect tooeach cluster node using `ssh` and use hello following command tooreinstall hello correct storage client version:</span></span>

```
sudo pip install azure-storage==0.20.0
```

<span data-ttu-id="bbfcc-451">Mer information om anslutande toohello kluster med SSH finns [använda SSH med HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="bbfcc-451">For information on connecting toohello cluster with SSH, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

### <a name="history-doesnt-show-scripts-used-during-cluster-creation"></a><span data-ttu-id="bbfcc-452">Historik visar inte skript som ska användas när klustret skapas</span><span class="sxs-lookup"><span data-stu-id="bbfcc-452">History doesn't show scripts used during cluster creation</span></span>

<span data-ttu-id="bbfcc-453">Om klustret har skapats innan den 15 mars 2016 kan du inte se en post i historiken för skriptåtgärder.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-453">If your cluster was created before March 15, 2016, you may not see an entry in Script Action history.</span></span> <span data-ttu-id="bbfcc-454">Om du ändrar storlek på hello kluster efter den 15 mars 2016 visas hello skript med hjälp av när klustret skapas i historiken som tillämpas de toonew noder i klustret hello som en del av hello att ändra storlek på.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-454">If you resize hello cluster after March 15, 2016, hello scripts using during cluster creation appear in history as they are applied toonew nodes in hello cluster as part of hello resize operation.</span></span>

<span data-ttu-id="bbfcc-455">Det finns två undantag:</span><span class="sxs-lookup"><span data-stu-id="bbfcc-455">There are two exceptions:</span></span>

* <span data-ttu-id="bbfcc-456">Om klustret har skapats före den 1 September 2015.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-456">If your cluster was created before September 1, 2015.</span></span> <span data-ttu-id="bbfcc-457">Det här datumet är när skriptåtgärder introducerades.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-457">This date is when Script Actions were introduced.</span></span> <span data-ttu-id="bbfcc-458">Ett kluster som skapats före det här datumet kunde inte har använt skriptåtgärder för klustret skapas.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-458">Any cluster created before this date could not have used Script Actions for cluster creation.</span></span>

* <span data-ttu-id="bbfcc-459">Om du använde flera skriptåtgärder när klustret skapas och används hello samma namn för flera skript eller hello namn samma, samma URI, men olika parametrar för flera skript.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-459">If you used multiple Script Actions during cluster creation, and used hello same name for multiple scripts, or hello same name, same URI, but different parameters for multiple scripts.</span></span> <span data-ttu-id="bbfcc-460">I dessa fall kan få du hello följande fel:</span><span class="sxs-lookup"><span data-stu-id="bbfcc-460">In these cases, you receive hello following error:</span></span>

    <span data-ttu-id="bbfcc-461">Nya åtgärder kan vara skript kördes på det här klustret på grund av tooconflicting skript namnen i befintliga skript.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-461">No new script actions can be ran on this cluster due tooconflicting script names in existing scripts.</span></span> <span data-ttu-id="bbfcc-462">Skriptnamn på klustret skapa måste vara alla unika.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-462">Script names provided at cluster create must be all unique.</span></span> <span data-ttu-id="bbfcc-463">Befintliga skript kördes på ändring av storlek.</span><span class="sxs-lookup"><span data-stu-id="bbfcc-463">Existing scripts are ran on resize.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bbfcc-464">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="bbfcc-464">Next steps</span></span>

* [<span data-ttu-id="bbfcc-465">Utveckla skriptåtgärd skript för HDInsight</span><span class="sxs-lookup"><span data-stu-id="bbfcc-465">Develop Script Action scripts for HDInsight</span></span>](hdinsight-hadoop-script-actions-linux.md)
* [<span data-ttu-id="bbfcc-466">Installera och använda Solr på HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="bbfcc-466">Install and use Solr on HDInsight clusters</span></span>](hdinsight-hadoop-solr-install-linux.md)
* [<span data-ttu-id="bbfcc-467">Installera och använda Giraph på HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="bbfcc-467">Install and use Giraph on HDInsight clusters</span></span>](hdinsight-hadoop-giraph-install-linux.md)
* [<span data-ttu-id="bbfcc-468">Lägga till ytterligare lagringsutrymme tooan HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="bbfcc-468">Add additional storage tooan HDInsight cluster</span></span>](hdinsight-hadoop-add-storage.md)

[img-hdi-cluster-states]: ./media/hdinsight-hadoop-customize-cluster-linux/HDI-Cluster-state.png "Steg när klustret skapas"
