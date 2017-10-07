---
title: aaaService kartan integrering med System Center Operations Manager | Microsoft Docs
description: "Tjänstkarta är en Operations Management Suite som automatiskt identifierar programkomponenter i Windows och Linux-system och kartor hello kommunikation mellan tjänster. Den här artikeln beskrivs med hjälp av Tjänstkarta tooautomatically skapa diagram för distribuerade program i Operations Manager."
services: operations-management-suite
documentationcenter: 
author: daveirwin1
manager: jwhit
editor: tysonn
ms.assetid: e8614a5a-9cf8-4c81-8931-896d358ad2cb
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/21/2017
ms.author: bwren;dairwin
ms.openlocfilehash: cff9cce2559448ec3a5fd14087b867f314716560
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="service-map-integration-with-system-center-operations-manager"></a><span data-ttu-id="d85f3-104">Tjänstkarta integrering med System Center Operations Manager</span><span class="sxs-lookup"><span data-stu-id="d85f3-104">Service Map integration with System Center Operations Manager</span></span>
  > [!NOTE]
  > <span data-ttu-id="d85f3-105">Den här funktionen är tillgänglig som förhandsversion.</span><span class="sxs-lookup"><span data-stu-id="d85f3-105">This feature is in public preview.</span></span>
  > 
  
<span data-ttu-id="d85f3-106">Operations Management Suite Tjänstkarta automatiskt identifierar programkomponenter på Windows- och Linux-system och mappar hello kommunikation mellan tjänster.</span><span class="sxs-lookup"><span data-stu-id="d85f3-106">Operations Management Suite Service Map automatically discovers application components on Windows and Linux systems and maps hello communication between services.</span></span> <span data-ttu-id="d85f3-107">Tjänstkarta kan tooview servrar hello väg du betrakta dem som sammanlänkade system som levererar kritiska tjänster.</span><span class="sxs-lookup"><span data-stu-id="d85f3-107">Service Map allows you tooview your servers hello way you think of them, as interconnected systems that deliver critical services.</span></span> <span data-ttu-id="d85f3-108">Tjänstkarta visar anslutningar mellan servrar, processer och portar i alla TCP-anslutna arkitektur med ingen konfiguration krävs förutom hello installation av en agent.</span><span class="sxs-lookup"><span data-stu-id="d85f3-108">Service Map shows connections between servers, processes, and ports across any TCP-connected architecture, with no configuration required besides hello installation of an agent.</span></span> <span data-ttu-id="d85f3-109">Mer information finns i hello [Tjänstkarta dokumentationen](operations-management-suite-service-map.md).</span><span class="sxs-lookup"><span data-stu-id="d85f3-109">For more information, see hello [Service Map documentation](operations-management-suite-service-map.md).</span></span>

<span data-ttu-id="d85f3-110">Med den här integreringen mellan Tjänstkarta och System Center Operations Manager kan automatiskt skapa diagram för distribuerade program i Operations Manager som är baserade på hello dynamiskt beroende maps i Tjänstkartan.</span><span class="sxs-lookup"><span data-stu-id="d85f3-110">With this integration between Service Map and System Center Operations Manager, you can automatically create distributed application diagrams in Operations Manager that are based on hello dynamic dependency maps in Service Map.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d85f3-111">Krav</span><span class="sxs-lookup"><span data-stu-id="d85f3-111">Prerequisites</span></span>
* <span data-ttu-id="d85f3-112">En Operations Manager-hanteringsgrupp som hanterar en uppsättning servrar.</span><span class="sxs-lookup"><span data-stu-id="d85f3-112">An Operations Manager management group that is managing a set of servers.</span></span>
* <span data-ttu-id="d85f3-113">En Operations Management Suite-arbetsyta med hello Tjänstkarta lösning som är aktiverad.</span><span class="sxs-lookup"><span data-stu-id="d85f3-113">An Operations Management Suite workspace with hello Service Map solution enabled.</span></span>
* <span data-ttu-id="d85f3-114">En uppsättning servrar (minst) som hanteras av Operations Manager och skicka data tooService kartan.</span><span class="sxs-lookup"><span data-stu-id="d85f3-114">A set of servers (at least one) that are being managed by Operations Manager and sending data tooService Map.</span></span> <span data-ttu-id="d85f3-115">Windows- och Linux-servrar stöds.</span><span class="sxs-lookup"><span data-stu-id="d85f3-115">Windows and Linux servers are supported.</span></span>
* <span data-ttu-id="d85f3-116">Ett huvudnamn för tjänsten med åtkomst toohello Azure-prenumeration som är associerad med hello Operations Management Suite-arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="d85f3-116">A service principal with access toohello Azure subscription that is associated with hello Operations Management Suite workspace.</span></span> <span data-ttu-id="d85f3-117">Mer information finns för[skapa ett huvudnamn för tjänsten](#creating-a-service-principal).</span><span class="sxs-lookup"><span data-stu-id="d85f3-117">For more information, go too[Create a service principal](#creating-a-service-principal).</span></span>

## <a name="install-hello-service-map-management-pack"></a><span data-ttu-id="d85f3-118">Installera hello Tjänstkarta management pack</span><span class="sxs-lookup"><span data-stu-id="d85f3-118">Install hello Service Map management pack</span></span>
<span data-ttu-id="d85f3-119">Du kan aktivera hello integrering mellan Operations Manager och Tjänstkarta genom att importera hello Microsoft.SystemCenter.ServiceMap management pack-samling (Microsoft.SystemCenter.ServiceMap.mpb).</span><span class="sxs-lookup"><span data-stu-id="d85f3-119">You enable hello integration between Operations Manager and Service Map by importing hello Microsoft.SystemCenter.ServiceMap management pack bundle (Microsoft.SystemCenter.ServiceMap.mpb).</span></span> <span data-ttu-id="d85f3-120">hello paket innehåller hello följande hanteringspaket:</span><span class="sxs-lookup"><span data-stu-id="d85f3-120">hello bundle contains hello following management packs:</span></span>
* <span data-ttu-id="d85f3-121">Microsoft Service kartvy program</span><span class="sxs-lookup"><span data-stu-id="d85f3-121">Microsoft Service Map Application Views</span></span>
* <span data-ttu-id="d85f3-122">Microsoft System Center Service Map internt</span><span class="sxs-lookup"><span data-stu-id="d85f3-122">Microsoft System Center Service Map Internal</span></span>
* <span data-ttu-id="d85f3-123">Microsoft System Center Service Map åsidosättningar</span><span class="sxs-lookup"><span data-stu-id="d85f3-123">Microsoft System Center Service Map Overrides</span></span>
* <span data-ttu-id="d85f3-124">Microsoft System Center-Tjänstkarta</span><span class="sxs-lookup"><span data-stu-id="d85f3-124">Microsoft System Center Service Map</span></span>

## <a name="configure-hello-service-map-integration"></a><span data-ttu-id="d85f3-125">Konfigurera hello Tjänstkarta-integrering</span><span class="sxs-lookup"><span data-stu-id="d85f3-125">Configure hello Service Map integration</span></span>
<span data-ttu-id="d85f3-126">När du har installerat hello Tjänstkarta hanteringspaket, en ny nod **Tjänstkarta**, visas under **Operations Management Suite** i hello **Administration** fönstret.</span><span class="sxs-lookup"><span data-stu-id="d85f3-126">After you install hello Service Map management pack, a new node, **Service Map**, is displayed under **Operations Management Suite** in hello **Administration** pane.</span></span> 

<span data-ttu-id="d85f3-127">tooconfigure Tjänstkarta integration hello följande:</span><span class="sxs-lookup"><span data-stu-id="d85f3-127">tooconfigure Service Map integration, do hello following:</span></span>

1. <span data-ttu-id="d85f3-128">tooopen hello-konfigurationsguiden i hello **översikt över tjänsten** rutan klickar du på **lägger du till arbetsytan**.</span><span class="sxs-lookup"><span data-stu-id="d85f3-128">tooopen hello configuration wizard, in hello **Service Map Overview** pane, click **Add workspace**.</span></span>  

    ![Översikt över tjänsten fönstret](media/oms-service-map/scom-configuration.png)

2. <span data-ttu-id="d85f3-130">I hello **anslutningskonfiguration** , ange hello innehavarens namn eller ID, program-ID (även kallat hello användarnamn eller clientID) och lösenord för hello tjänstens huvudnamn och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="d85f3-130">In hello **Connection Configuration** window, enter hello tenant name or ID, application ID (also known as hello username or clientID), and password of hello service principal, and then click **Next**.</span></span> <span data-ttu-id="d85f3-131">Mer information finns för[skapa ett huvudnamn för tjänsten](#creating-a-service-principal).</span><span class="sxs-lookup"><span data-stu-id="d85f3-131">For more information, go too[Create a service principal](#creating-a-service-principal).</span></span>

    ![hello anslutningskonfiguration fönster](media/oms-service-map/scom-config-spn.png)

3. <span data-ttu-id="d85f3-133">I hello **prenumeration markeringen** fönstret Välj hello Azure-prenumeration, Azure-resursgrupp (hello en som innehåller hello Operations Management Suite-arbetsyta) och Operations Management Suite-arbetsyta och klicka sedan på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="d85f3-133">In hello **Subscription Selection** window, select hello Azure subscription, Azure resource group (hello one that contains hello Operations Management Suite workspace), and Operations Management Suite workspace, and then click **Next**.</span></span>

    ![hello Operations Manager Configuration-arbetsyta](media/oms-service-map/scom-config-workspace.png)

4. <span data-ttu-id="d85f3-135">I hello **val av datorn** fönstret kan du välja vilka Service Map datorgrupper du vill toosync tooOperations Manager.</span><span class="sxs-lookup"><span data-stu-id="d85f3-135">In hello **Machine Group Selection** window, you choose which Service Map Machine Groups you want toosync tooOperations Manager.</span></span> <span data-ttu-id="d85f3-136">Klicka på **Lägg till/ta bort datorgrupper**, Välj grupper hello listan över **tillgängliga datorgrupper**, och klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="d85f3-136">Click **Add/Remove Machine Groups**, choose groups from hello list of **Available Machine Groups**, and click **Add**.</span></span>  <span data-ttu-id="d85f3-137">När du har valt grupper klickar du på **Ok** toofinish.</span><span class="sxs-lookup"><span data-stu-id="d85f3-137">When you are finished selecting groups, click **Ok** toofinish.</span></span>
    
    ![hello datorgrupper för Operations Manager konfiguration](media/oms-service-map/scom-config-machine-groups.png)
    
5. <span data-ttu-id="d85f3-139">I hello **Serverval** fönstret du konfigurerar hello tjänstgruppen kartan servrar med hello-servrar som du vill toosync mellan Operations Manager och Tjänstkartan.</span><span class="sxs-lookup"><span data-stu-id="d85f3-139">In hello **Server Selection** window, you configure hello Service Map Servers Group with hello servers that you want toosync between Operations Manager and Service Map.</span></span> <span data-ttu-id="d85f3-140">Klicka på **Lägg till/ta bort servrar**.</span><span class="sxs-lookup"><span data-stu-id="d85f3-140">Click **Add/Remove Servers**.</span></span>   
    
    <span data-ttu-id="d85f3-141">Hello-server måste vara för hello integration toobuild ett distribuerat program diagram för en server:</span><span class="sxs-lookup"><span data-stu-id="d85f3-141">For hello integration toobuild a distributed application diagram for a server, hello server must be:</span></span>

    * <span data-ttu-id="d85f3-142">Hanteras av Operations Manager</span><span class="sxs-lookup"><span data-stu-id="d85f3-142">Managed by Operations Manager</span></span>
    * <span data-ttu-id="d85f3-143">Hanteras av en Tjänstkarta</span><span class="sxs-lookup"><span data-stu-id="d85f3-143">Managed by Service Map</span></span>
    * <span data-ttu-id="d85f3-144">Som anges i hello tjänstgruppen kartan servrar</span><span class="sxs-lookup"><span data-stu-id="d85f3-144">Listed in hello Service Map Servers Group</span></span>

    ![hello konfigurationsgruppen för Operations Manager](media/oms-service-map/scom-config-group.png)

6. <span data-ttu-id="d85f3-146">Valfritt: Välj hello Management Server resource pool toocommunicate med Operations Management Suite och klicka sedan på **lägger du till arbetsytan**.</span><span class="sxs-lookup"><span data-stu-id="d85f3-146">Optional: Select hello Management Server resource pool toocommunicate with Operations Management Suite, and then click **Add Workspace**.</span></span>

    ![hello resurspool för Operations Manager-konfiguration](media/oms-service-map/scom-config-pool.png)

    <span data-ttu-id="d85f3-148">Det kan ta en minut tooconfigure och registrera hello Operations Management Suite-arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="d85f3-148">It might take a minute tooconfigure and register hello Operations Management Suite workspace.</span></span> <span data-ttu-id="d85f3-149">När den är konfigurerad initieras Operations Manager hello första Tjänstkarta synkronisering från Operations Management Suite.</span><span class="sxs-lookup"><span data-stu-id="d85f3-149">After it is configured, Operations Manager initiates hello first Service Map sync from Operations Management Suite.</span></span>

    ![hello resurspool för Operations Manager-konfiguration](media/oms-service-map/scom-config-success.png)


## <a name="monitor-service-map"></a><span data-ttu-id="d85f3-151">Övervakare för Tjänstmappning</span><span class="sxs-lookup"><span data-stu-id="d85f3-151">Monitor Service Map</span></span>
<span data-ttu-id="d85f3-152">Efter hello Operations Management Suite-arbetsyta är ansluten i hello visas en ny mapp Tjänstkartan **övervakning** rutan hello Operations Manager-konsolen.</span><span class="sxs-lookup"><span data-stu-id="d85f3-152">After hello Operations Management Suite workspace is connected, a new folder, Service Map, is displayed in hello **Monitoring** pane of hello Operations Manager console.</span></span>

![hello övervakning för Operations Manager-fönstret](media/oms-service-map/scom-monitoring.png)

<span data-ttu-id="d85f3-154">Hej Tjänstkarta mapp har fyra noder:</span><span class="sxs-lookup"><span data-stu-id="d85f3-154">hello Service Map folder has four nodes:</span></span>
* <span data-ttu-id="d85f3-155">**Aktiva aviseringar**: Visar en lista över alla hello aktiva aviseringar om hello kommunikation mellan Operations Manager och Tjänstkartan.</span><span class="sxs-lookup"><span data-stu-id="d85f3-155">**Active Alerts**: Lists all hello active alerts about hello communication between Operations Manager and Service Map.</span></span>  <span data-ttu-id="d85f3-156">Observera att dessa varningar inte är Operations Management Suite-aviseringar som synkroniserade tooOperations Manager.</span><span class="sxs-lookup"><span data-stu-id="d85f3-156">Note that these alerts are not Operations Management Suite alerts being synced tooOperations Manager.</span></span> 

* <span data-ttu-id="d85f3-157">**Servrar**: Visar hello övervakade servrar som är konfigurerade toosync från Tjänstkarta.</span><span class="sxs-lookup"><span data-stu-id="d85f3-157">**Servers**: Lists hello monitored servers that are configured toosync from Service Map.</span></span>

    ![hello övervakning servrar för Operations Manager-fönstret](media/oms-service-map/scom-monitoring-servers.png)

* <span data-ttu-id="d85f3-159">**Datorn beroende Gruppvyer**: Visar en lista över alla datorgrupper som synkroniseras från Tjänstkartan.</span><span class="sxs-lookup"><span data-stu-id="d85f3-159">**Machine Group Dependency Views**: Lists all machine groups that are synced from Service Map.</span></span> <span data-ttu-id="d85f3-160">Du kan klicka på någon grupp tooview diagram dess distribuerade program.</span><span class="sxs-lookup"><span data-stu-id="d85f3-160">You can click any group tooview its distributed application diagram.</span></span>

    ![diagram över hello Operations Manager-distribuerade program](media/oms-service-map/scom-group-dad.png)

* <span data-ttu-id="d85f3-162">**Servern beroende vyer**: Visar en lista över alla servrar som synkroniseras från Tjänstkartan.</span><span class="sxs-lookup"><span data-stu-id="d85f3-162">**Server Dependency Views**: Lists all servers that are synced from Service Map.</span></span> <span data-ttu-id="d85f3-163">Du kan klicka på varje server tooview diagram dess distribuerade program.</span><span class="sxs-lookup"><span data-stu-id="d85f3-163">You can click any server tooview its distributed application diagram.</span></span>

    ![diagram över hello Operations Manager-distribuerade program](media/oms-service-map/scom-dad.png)

## <a name="edit-or-delete-hello-workspace"></a><span data-ttu-id="d85f3-165">Redigera eller ta bort hello-arbetsytan</span><span class="sxs-lookup"><span data-stu-id="d85f3-165">Edit or delete hello workspace</span></span>
<span data-ttu-id="d85f3-166">Du kan redigera eller ta bort hello konfigurerats arbetsytan via hello **översikt över tjänsten** fönstret (**Administration** fönstret > **Operations Management Suite**  >  **Service Map**).</span><span class="sxs-lookup"><span data-stu-id="d85f3-166">You can edit or delete hello configured workspace through hello **Service Map Overview** pane (**Administration** pane > **Operations Management Suite** > **Service Map**).</span></span> <span data-ttu-id="d85f3-167">Du kan konfigurera en enda Operations Management Suite-arbetsyta för tillfället.</span><span class="sxs-lookup"><span data-stu-id="d85f3-167">You can configure only one Operations Management Suite workspace for now.</span></span>

![hello Operations Manager redigera arbetsyta](media/oms-service-map/scom-edit-workspace.png)

## <a name="configure-rules-and-overrides"></a><span data-ttu-id="d85f3-169">Konfigurera regler och åsidosättningar</span><span class="sxs-lookup"><span data-stu-id="d85f3-169">Configure rules and overrides</span></span>
<span data-ttu-id="d85f3-170">En regel _Microsoft.SystemCenter.ServiceMapImport.Rule_, skapas tooperiodically hämtning av information från Tjänstkartan.</span><span class="sxs-lookup"><span data-stu-id="d85f3-170">A rule, _Microsoft.SystemCenter.ServiceMapImport.Rule_, is created tooperiodically fetch information from Service Map.</span></span> <span data-ttu-id="d85f3-171">toochange sync tidsinställningar som du kan konfigurera åsidosättningar av hello regel (**redigering** fönstret > **regler** > **Microsoft.SystemCenter.ServiceMapImport.Rule**).</span><span class="sxs-lookup"><span data-stu-id="d85f3-171">toochange sync timings, you can configure overrides of hello rule (**Authoring** pane > **Rules** > **Microsoft.SystemCenter.ServiceMapImport.Rule**).</span></span>

![egenskapsfönstret om hello åsidosättningar i Operations Manager](media/oms-service-map/scom-overrides.png)

* <span data-ttu-id="d85f3-173">**Aktiverad**: aktivera eller inaktivera automatiska uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="d85f3-173">**Enabled**: Enable or disable automatic updates.</span></span> 
* <span data-ttu-id="d85f3-174">**IntervalMinutes**: återställa hello tiden mellan uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="d85f3-174">**IntervalMinutes**: Reset hello time between updates.</span></span> <span data-ttu-id="d85f3-175">hello Standardintervallet är en timme.</span><span class="sxs-lookup"><span data-stu-id="d85f3-175">hello default interval is one hour.</span></span> <span data-ttu-id="d85f3-176">Om du vill toosync server maps oftare, kan du ändra hello-värdet.</span><span class="sxs-lookup"><span data-stu-id="d85f3-176">If you want toosync server maps more frequently, you can change hello value.</span></span>
* <span data-ttu-id="d85f3-177">**TimeoutSeconds**: återställa hello lång tid innan tidsgränsen uppnås för hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="d85f3-177">**TimeoutSeconds**: Reset hello length of time before hello request times out.</span></span> 
* <span data-ttu-id="d85f3-178">**TimeWindowMinutes**: Återställ hello tidsfönstret för datafrågor.</span><span class="sxs-lookup"><span data-stu-id="d85f3-178">**TimeWindowMinutes**: Reset hello time window for querying data.</span></span> <span data-ttu-id="d85f3-179">Standardvärdet är 60 minuter-fönstret.</span><span class="sxs-lookup"><span data-stu-id="d85f3-179">Default is a 60-minute window.</span></span> <span data-ttu-id="d85f3-180">hello största tillåtna värdet av Tjänstkarta är 60 minuter.</span><span class="sxs-lookup"><span data-stu-id="d85f3-180">hello maximum value allowed by Service Map is 60 minutes.</span></span>

## <a name="known-issues-and-limitations"></a><span data-ttu-id="d85f3-181">Kända problem och begränsningar</span><span class="sxs-lookup"><span data-stu-id="d85f3-181">Known issues and limitations</span></span>

<span data-ttu-id="d85f3-182">hello aktuella designen visar hello följande problem och begränsningar:</span><span class="sxs-lookup"><span data-stu-id="d85f3-182">hello current design presents hello following issues and limitations:</span></span>
* <span data-ttu-id="d85f3-183">Du kan bara ansluta tooa enstaka Operations Management Suite-arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="d85f3-183">You can only connect tooa single Operations Management Suite workspace.</span></span>
* <span data-ttu-id="d85f3-184">Du kan lägga till servrar toohello tjänstgruppen kartan servrar manuellt via hello **redigering** rutan hello kartor för servrarna synkroniseras inte omedelbart.</span><span class="sxs-lookup"><span data-stu-id="d85f3-184">Although you can add servers toohello Service Map Servers Group manually through hello **Authoring** pane, hello maps for those servers are not synced immediately.</span></span>  <span data-ttu-id="d85f3-185">De kommer att synkroniseras från Tjänstkarta hello nästa synkronisering cykel.</span><span class="sxs-lookup"><span data-stu-id="d85f3-185">They will be synced from Service Map during hello next sync cycle.</span></span>
* <span data-ttu-id="d85f3-186">Om du gör några ändringar toohello distribuerade program diagram som har skapats av hello hanteringspaket ändringarna sannolikt att skrivas över på hello nästa synkronisering med Tjänstkartan.</span><span class="sxs-lookup"><span data-stu-id="d85f3-186">If you make any changes toohello Distributed Application Diagrams created by hello management pack, those changes will likely be overwritten on hello next sync with Service Map.</span></span>

## <a name="create-a-service-principal"></a><span data-ttu-id="d85f3-187">Skapa ett huvudnamn för tjänsten</span><span class="sxs-lookup"><span data-stu-id="d85f3-187">Create a service principal</span></span>
<span data-ttu-id="d85f3-188">Officiell Azure-dokumentation om hur du skapar en tjänst huvudnamn finns:</span><span class="sxs-lookup"><span data-stu-id="d85f3-188">For official Azure documentation about creating a service principal, see:</span></span>
* [<span data-ttu-id="d85f3-189">Skapa ett huvudnamn för tjänsten med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="d85f3-189">Create a service principal by using PowerShell</span></span>](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authenticate-service-principal)
* [<span data-ttu-id="d85f3-190">Skapa ett huvudnamn för tjänsten med hjälp av Azure CLI</span><span class="sxs-lookup"><span data-stu-id="d85f3-190">Create a service principal by using Azure CLI</span></span>](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authenticate-service-principal-cli)
* [<span data-ttu-id="d85f3-191">Skapa ett huvudnamn för tjänsten med hjälp av hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="d85f3-191">Create a service principal by using hello Azure portal</span></span>](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal)

### <a name="feedback"></a><span data-ttu-id="d85f3-192">Feedback</span><span class="sxs-lookup"><span data-stu-id="d85f3-192">Feedback</span></span>
<span data-ttu-id="d85f3-193">Du har feedback för oss om Tjänstkarta eller den här dokumentationen?</span><span class="sxs-lookup"><span data-stu-id="d85f3-193">Do you have any feedback for us about Service Map or this documentation?</span></span> <span data-ttu-id="d85f3-194">Besök vår [User Voice sidan](https://feedback.azure.com/forums/267889-log-analytics/category/184492-service-map), där du kan föreslår funktioner eller rösta på förslag befintliga.</span><span class="sxs-lookup"><span data-stu-id="d85f3-194">Visit our [User Voice page](https://feedback.azure.com/forums/267889-log-analytics/category/184492-service-map), where you can suggest features or vote on existing suggestions.</span></span>
