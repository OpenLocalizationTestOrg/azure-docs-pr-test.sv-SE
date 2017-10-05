---
title: "Tjänstkarta integrering med System Center Operations Manager | Microsoft Docs"
description: "Tjänstkarta är en Operations Management Suite-lösning som automatiskt identifierar programkomponenter på Windows- och Linux-system och mappar kommunikationen mellan tjänster. Den här artikeln beskrivs med hjälp av en Tjänstkarta att automatiskt skapa diagram för distribuerade program i Operations Manager."
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
ms.openlocfilehash: a7dbe54ffb4daa941c19b51ba263dd3d23b7a98b
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="service-map-integration-with-system-center-operations-manager"></a><span data-ttu-id="1dc57-104">Tjänstkarta integrering med System Center Operations Manager</span><span class="sxs-lookup"><span data-stu-id="1dc57-104">Service Map integration with System Center Operations Manager</span></span>
  > [!NOTE]
  > <span data-ttu-id="1dc57-105">Den här funktionen är tillgänglig som förhandsversion.</span><span class="sxs-lookup"><span data-stu-id="1dc57-105">This feature is in public preview.</span></span>
  > 
  
<span data-ttu-id="1dc57-106">Operations Management Suite Tjänstkarta automatiskt identifierar programkomponenter på Windows- och Linux-system och mappar kommunikationen mellan tjänster.</span><span class="sxs-lookup"><span data-stu-id="1dc57-106">Operations Management Suite Service Map automatically discovers application components on Windows and Linux systems and maps the communication between services.</span></span> <span data-ttu-id="1dc57-107">Tjänstkarta kan du visa dina servrar på sätt som du betrakta dem som sammanlänkade system som levererar kritiska tjänster.</span><span class="sxs-lookup"><span data-stu-id="1dc57-107">Service Map allows you to view your servers the way you think of them, as interconnected systems that deliver critical services.</span></span> <span data-ttu-id="1dc57-108">Tjänstkarta visar anslutningar mellan servrar, processer och portar i alla TCP-anslutna arkitektur med ingen konfiguration krävs förutom installation av en agent.</span><span class="sxs-lookup"><span data-stu-id="1dc57-108">Service Map shows connections between servers, processes, and ports across any TCP-connected architecture, with no configuration required besides the installation of an agent.</span></span> <span data-ttu-id="1dc57-109">Mer information finns i [Tjänstkarta dokumentationen](operations-management-suite-service-map.md).</span><span class="sxs-lookup"><span data-stu-id="1dc57-109">For more information, see the [Service Map documentation](operations-management-suite-service-map.md).</span></span>

<span data-ttu-id="1dc57-110">Med den här integreringen mellan Tjänstkarta och System Center Operations Manager kan automatiskt skapa diagram för distribuerade program i Operations Manager som är baserade på dynamiskt beroende maps i Tjänstkartan.</span><span class="sxs-lookup"><span data-stu-id="1dc57-110">With this integration between Service Map and System Center Operations Manager, you can automatically create distributed application diagrams in Operations Manager that are based on the dynamic dependency maps in Service Map.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1dc57-111">Krav</span><span class="sxs-lookup"><span data-stu-id="1dc57-111">Prerequisites</span></span>
* <span data-ttu-id="1dc57-112">En Operations Manager-hanteringsgrupp som hanterar en uppsättning servrar.</span><span class="sxs-lookup"><span data-stu-id="1dc57-112">An Operations Manager management group that is managing a set of servers.</span></span>
* <span data-ttu-id="1dc57-113">En Operations Management Suite-arbetsyta med Tjänstkarta lösningen aktiverad.</span><span class="sxs-lookup"><span data-stu-id="1dc57-113">An Operations Management Suite workspace with the Service Map solution enabled.</span></span>
* <span data-ttu-id="1dc57-114">En uppsättning servrar (minst) som hanteras av Operations Manager och skicka data till Tjänstkartan.</span><span class="sxs-lookup"><span data-stu-id="1dc57-114">A set of servers (at least one) that are being managed by Operations Manager and sending data to Service Map.</span></span> <span data-ttu-id="1dc57-115">Windows- och Linux-servrar stöds.</span><span class="sxs-lookup"><span data-stu-id="1dc57-115">Windows and Linux servers are supported.</span></span>
* <span data-ttu-id="1dc57-116">Ett huvudnamn för tjänsten med åtkomst till Azure-prenumerationen som är associerad med Operations Management Suite-arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="1dc57-116">A service principal with access to the Azure subscription that is associated with the Operations Management Suite workspace.</span></span> <span data-ttu-id="1dc57-117">Mer information finns på [skapa ett huvudnamn för tjänsten](#creating-a-service-principal).</span><span class="sxs-lookup"><span data-stu-id="1dc57-117">For more information, go to [Create a service principal](#creating-a-service-principal).</span></span>

## <a name="install-the-service-map-management-pack"></a><span data-ttu-id="1dc57-118">Installera Tjänstkarta management pack</span><span class="sxs-lookup"><span data-stu-id="1dc57-118">Install the Service Map management pack</span></span>
<span data-ttu-id="1dc57-119">Du kan aktivera integrering mellan Operations Manager och Tjänstkarta genom att importera det Microsoft.SystemCenter.ServiceMap management Pack (Microsoft.SystemCenter.ServiceMap.mpb).</span><span class="sxs-lookup"><span data-stu-id="1dc57-119">You enable the integration between Operations Manager and Service Map by importing the Microsoft.SystemCenter.ServiceMap management pack bundle (Microsoft.SystemCenter.ServiceMap.mpb).</span></span> <span data-ttu-id="1dc57-120">Paketet innehåller följande hanteringspaket:</span><span class="sxs-lookup"><span data-stu-id="1dc57-120">The bundle contains the following management packs:</span></span>
* <span data-ttu-id="1dc57-121">Microsoft Service kartvy program</span><span class="sxs-lookup"><span data-stu-id="1dc57-121">Microsoft Service Map Application Views</span></span>
* <span data-ttu-id="1dc57-122">Microsoft System Center Service Map internt</span><span class="sxs-lookup"><span data-stu-id="1dc57-122">Microsoft System Center Service Map Internal</span></span>
* <span data-ttu-id="1dc57-123">Microsoft System Center Service Map åsidosättningar</span><span class="sxs-lookup"><span data-stu-id="1dc57-123">Microsoft System Center Service Map Overrides</span></span>
* <span data-ttu-id="1dc57-124">Microsoft System Center-Tjänstkarta</span><span class="sxs-lookup"><span data-stu-id="1dc57-124">Microsoft System Center Service Map</span></span>

## <a name="configure-the-service-map-integration"></a><span data-ttu-id="1dc57-125">Konfigurera Tjänstkarta-integrering</span><span class="sxs-lookup"><span data-stu-id="1dc57-125">Configure the Service Map integration</span></span>
<span data-ttu-id="1dc57-126">När du har installerat en ny nod i hanteringspaketet Tjänstkarta **Tjänstkarta**, visas under **Operations Management Suite** i den **Administration** fönstret.</span><span class="sxs-lookup"><span data-stu-id="1dc57-126">After you install the Service Map management pack, a new node, **Service Map**, is displayed under **Operations Management Suite** in the **Administration** pane.</span></span> 

<span data-ttu-id="1dc57-127">Om du vill konfigurera Tjänstkarta-integrering gör du följande:</span><span class="sxs-lookup"><span data-stu-id="1dc57-127">To configure Service Map integration, do the following:</span></span>

1. <span data-ttu-id="1dc57-128">Öppna guiden Konfigurera i den **översikt över tjänsten** rutan klickar du på **lägger du till arbetsytan**.</span><span class="sxs-lookup"><span data-stu-id="1dc57-128">To open the configuration wizard, in the **Service Map Overview** pane, click **Add workspace**.</span></span>  

    ![Översikt över tjänsten fönstret](media/oms-service-map/scom-configuration.png)

2. <span data-ttu-id="1dc57-130">I den **anslutningskonfiguration** , ange innehavarens namn eller ID, program-ID (användarnamn eller clientID) och lösenordet för tjänstens huvudnamn och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="1dc57-130">In the **Connection Configuration** window, enter the tenant name or ID, application ID (also known as the username or clientID), and password of the service principal, and then click **Next**.</span></span> <span data-ttu-id="1dc57-131">Mer information finns på [skapa ett huvudnamn för tjänsten](#creating-a-service-principal).</span><span class="sxs-lookup"><span data-stu-id="1dc57-131">For more information, go to [Create a service principal](#creating-a-service-principal).</span></span>

    ![Fönstret anslutningskonfiguration](media/oms-service-map/scom-config-spn.png)

3. <span data-ttu-id="1dc57-133">I den **prenumeration markeringen** fönstret Välj Azure-prenumeration, Azure-resursgrupp (en som innehåller Operations Management Suite-arbetsyta) och Operations Management Suite-arbetsyta och klicka sedan på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="1dc57-133">In the **Subscription Selection** window, select the Azure subscription, Azure resource group (the one that contains the Operations Management Suite workspace), and Operations Management Suite workspace, and then click **Next**.</span></span>

    ![Operations Manager Configuration-arbetsyta](media/oms-service-map/scom-config-workspace.png)

4. <span data-ttu-id="1dc57-135">I den **val av datorn** fönstret väljer vilka kartan datorgrupper som du vill synkronisera till Operations Manager.</span><span class="sxs-lookup"><span data-stu-id="1dc57-135">In the **Machine Group Selection** window, you choose which Service Map Machine Groups you want to sync to Operations Manager.</span></span> <span data-ttu-id="1dc57-136">Klicka på **Lägg till/ta bort datorgrupper**, Välj grupper i listan över **tillgängliga datorgrupper**, och klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="1dc57-136">Click **Add/Remove Machine Groups**, choose groups from the list of **Available Machine Groups**, and click **Add**.</span></span>  <span data-ttu-id="1dc57-137">När du har valt grupper klickar du på **Ok** ska slutföras.</span><span class="sxs-lookup"><span data-stu-id="1dc57-137">When you are finished selecting groups, click **Ok** to finish.</span></span>
    
    ![Operations Manager Configuration dator grupper](media/oms-service-map/scom-config-machine-groups.png)
    
5. <span data-ttu-id="1dc57-139">I den **Serverval** fönstret du konfigurerar gruppen Service Map servrar med de servrar som du vill synkronisera mellan Operations Manager och Tjänstkartan.</span><span class="sxs-lookup"><span data-stu-id="1dc57-139">In the **Server Selection** window, you configure the Service Map Servers Group with the servers that you want to sync between Operations Manager and Service Map.</span></span> <span data-ttu-id="1dc57-140">Klicka på **Lägg till/ta bort servrar**.</span><span class="sxs-lookup"><span data-stu-id="1dc57-140">Click **Add/Remove Servers**.</span></span>   
    
    <span data-ttu-id="1dc57-141">Servern för integrering skapa ett distribuerat program diagram för en server måste vara:</span><span class="sxs-lookup"><span data-stu-id="1dc57-141">For the integration to build a distributed application diagram for a server, the server must be:</span></span>

    * <span data-ttu-id="1dc57-142">Hanteras av Operations Manager</span><span class="sxs-lookup"><span data-stu-id="1dc57-142">Managed by Operations Manager</span></span>
    * <span data-ttu-id="1dc57-143">Hanteras av en Tjänstkarta</span><span class="sxs-lookup"><span data-stu-id="1dc57-143">Managed by Service Map</span></span>
    * <span data-ttu-id="1dc57-144">Som anges i gruppen Service Map servrar</span><span class="sxs-lookup"><span data-stu-id="1dc57-144">Listed in the Service Map Servers Group</span></span>

    ![Gruppen för Operations Manager-konfiguration](media/oms-service-map/scom-config-group.png)

6. <span data-ttu-id="1dc57-146">Valfritt: Välj resurspoolen hanteringsservern kan kommunicera med Operations Management Suite och klicka sedan på **lägger du till arbetsytan**.</span><span class="sxs-lookup"><span data-stu-id="1dc57-146">Optional: Select the Management Server resource pool to communicate with Operations Management Suite, and then click **Add Workspace**.</span></span>

    ![Operations Manager Configuration resurspoolen](media/oms-service-map/scom-config-pool.png)

    <span data-ttu-id="1dc57-148">Det kan ta någon minut att konfigurera och registrera Operations Management Suite-arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="1dc57-148">It might take a minute to configure and register the Operations Management Suite workspace.</span></span> <span data-ttu-id="1dc57-149">När den har konfigurerats, initierar den första synkroniseringen Tjänstkarta från Operations Management Suite Operations Manager.</span><span class="sxs-lookup"><span data-stu-id="1dc57-149">After it is configured, Operations Manager initiates the first Service Map sync from Operations Management Suite.</span></span>

    ![Operations Manager Configuration resurspoolen](media/oms-service-map/scom-config-success.png)


## <a name="monitor-service-map"></a><span data-ttu-id="1dc57-151">Övervakare för Tjänstmappning</span><span class="sxs-lookup"><span data-stu-id="1dc57-151">Monitor Service Map</span></span>
<span data-ttu-id="1dc57-152">När Operations Management Suite-arbetsyta är anslutet visas en ny mapp på Tjänstkartan, i den **övervakning** rutan i Operations Manager-konsolen.</span><span class="sxs-lookup"><span data-stu-id="1dc57-152">After the Operations Management Suite workspace is connected, a new folder, Service Map, is displayed in the **Monitoring** pane of the Operations Manager console.</span></span>

![Övervakning av Operations Manager-fönstret](media/oms-service-map/scom-monitoring.png)

<span data-ttu-id="1dc57-154">Mappen Tjänstkarta har fyra noder:</span><span class="sxs-lookup"><span data-stu-id="1dc57-154">The Service Map folder has four nodes:</span></span>
* <span data-ttu-id="1dc57-155">**Aktiva aviseringar**: Visar en lista över alla aktiva aviseringar om kommunikation mellan Operations Manager och Tjänstkartan.</span><span class="sxs-lookup"><span data-stu-id="1dc57-155">**Active Alerts**: Lists all the active alerts about the communication between Operations Manager and Service Map.</span></span>  <span data-ttu-id="1dc57-156">Observera att dessa varningar inte är Operations Management Suite aviseringar som synkroniseras till Operations Manager.</span><span class="sxs-lookup"><span data-stu-id="1dc57-156">Note that these alerts are not Operations Management Suite alerts being synced to Operations Manager.</span></span> 

* <span data-ttu-id="1dc57-157">**Servrar**: Visar de övervakade servrarna som är konfigurerade för synkronisering från Tjänstkartan.</span><span class="sxs-lookup"><span data-stu-id="1dc57-157">**Servers**: Lists the monitored servers that are configured to sync from Service Map.</span></span>

    ![Övervaka servrar för Operations Manager-fönstret](media/oms-service-map/scom-monitoring-servers.png)

* <span data-ttu-id="1dc57-159">**Datorn beroende Gruppvyer**: Visar en lista över alla datorgrupper som synkroniseras från Tjänstkartan.</span><span class="sxs-lookup"><span data-stu-id="1dc57-159">**Machine Group Dependency Views**: Lists all machine groups that are synced from Service Map.</span></span> <span data-ttu-id="1dc57-160">Du kan klicka på valfri grupp om du vill visa dess distribuerade program-diagram.</span><span class="sxs-lookup"><span data-stu-id="1dc57-160">You can click any group to view its distributed application diagram.</span></span>

    ![Diagram för Operations Manager-distribuerade program](media/oms-service-map/scom-group-dad.png)

* <span data-ttu-id="1dc57-162">**Servern beroende vyer**: Visar en lista över alla servrar som synkroniseras från Tjänstkartan.</span><span class="sxs-lookup"><span data-stu-id="1dc57-162">**Server Dependency Views**: Lists all servers that are synced from Service Map.</span></span> <span data-ttu-id="1dc57-163">Du kan klicka på en server om du vill visa dess distribuerade program-diagram.</span><span class="sxs-lookup"><span data-stu-id="1dc57-163">You can click any server to view its distributed application diagram.</span></span>

    ![Diagram för Operations Manager-distribuerade program](media/oms-service-map/scom-dad.png)

## <a name="edit-or-delete-the-workspace"></a><span data-ttu-id="1dc57-165">Redigera eller ta bort arbetsytan</span><span class="sxs-lookup"><span data-stu-id="1dc57-165">Edit or delete the workspace</span></span>
<span data-ttu-id="1dc57-166">Du kan redigera eller ta bort arbetsytan konfigurerade via den **översikt över tjänsten** fönstret (**Administration** fönstret > **Operations Management Suite**  >  **Service Map**).</span><span class="sxs-lookup"><span data-stu-id="1dc57-166">You can edit or delete the configured workspace through the **Service Map Overview** pane (**Administration** pane > **Operations Management Suite** > **Service Map**).</span></span> <span data-ttu-id="1dc57-167">Du kan konfigurera en enda Operations Management Suite-arbetsyta för tillfället.</span><span class="sxs-lookup"><span data-stu-id="1dc57-167">You can configure only one Operations Management Suite workspace for now.</span></span>

![Redigera arbetsytan för Operations Manager](media/oms-service-map/scom-edit-workspace.png)

## <a name="configure-rules-and-overrides"></a><span data-ttu-id="1dc57-169">Konfigurera regler och åsidosättningar</span><span class="sxs-lookup"><span data-stu-id="1dc57-169">Configure rules and overrides</span></span>
<span data-ttu-id="1dc57-170">En regel _Microsoft.SystemCenter.ServiceMapImport.Rule_, skapas för att regelbundet hämta information från Tjänstkartan.</span><span class="sxs-lookup"><span data-stu-id="1dc57-170">A rule, _Microsoft.SystemCenter.ServiceMapImport.Rule_, is created to periodically fetch information from Service Map.</span></span> <span data-ttu-id="1dc57-171">Om du vill ändra tidsinställningar för synkronisering, kan du konfigurera åsidosättningar av regeln (**redigering** fönstret > **regler** > **Microsoft.SystemCenter.ServiceMapImport.Rule**) .</span><span class="sxs-lookup"><span data-stu-id="1dc57-171">To change sync timings, you can configure overrides of the rule (**Authoring** pane > **Rules** > **Microsoft.SystemCenter.ServiceMapImport.Rule**).</span></span>

![Egenskapsfönstret för åsidosättningar i Operations Manager](media/oms-service-map/scom-overrides.png)

* <span data-ttu-id="1dc57-173">**Aktiverad**: aktivera eller inaktivera automatiska uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="1dc57-173">**Enabled**: Enable or disable automatic updates.</span></span> 
* <span data-ttu-id="1dc57-174">**IntervalMinutes**: återställa tiden mellan uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="1dc57-174">**IntervalMinutes**: Reset the time between updates.</span></span> <span data-ttu-id="1dc57-175">Standardintervallet är en timme.</span><span class="sxs-lookup"><span data-stu-id="1dc57-175">The default interval is one hour.</span></span> <span data-ttu-id="1dc57-176">Du kan ändra värdet om du vill synkronisera server maps oftare.</span><span class="sxs-lookup"><span data-stu-id="1dc57-176">If you want to sync server maps more frequently, you can change the value.</span></span>
* <span data-ttu-id="1dc57-177">**TimeoutSeconds**: återställa hur lång tid innan tidsgränsen uppnås för begäran.</span><span class="sxs-lookup"><span data-stu-id="1dc57-177">**TimeoutSeconds**: Reset the length of time before the request times out.</span></span> 
* <span data-ttu-id="1dc57-178">**TimeWindowMinutes**: återställa tidsfönstret för datafrågor.</span><span class="sxs-lookup"><span data-stu-id="1dc57-178">**TimeWindowMinutes**: Reset the time window for querying data.</span></span> <span data-ttu-id="1dc57-179">Standardvärdet är 60 minuter-fönstret.</span><span class="sxs-lookup"><span data-stu-id="1dc57-179">Default is a 60-minute window.</span></span> <span data-ttu-id="1dc57-180">Det maximala värdet som tillåts av Tjänstkarta är 60 minuter.</span><span class="sxs-lookup"><span data-stu-id="1dc57-180">The maximum value allowed by Service Map is 60 minutes.</span></span>

## <a name="known-issues-and-limitations"></a><span data-ttu-id="1dc57-181">Kända problem och begränsningar</span><span class="sxs-lookup"><span data-stu-id="1dc57-181">Known issues and limitations</span></span>

<span data-ttu-id="1dc57-182">Den aktuella designen innehåller följande problem och begränsningar:</span><span class="sxs-lookup"><span data-stu-id="1dc57-182">The current design presents the following issues and limitations:</span></span>
* <span data-ttu-id="1dc57-183">Du kan bara ansluta till en enda Operations Management Suite-arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="1dc57-183">You can only connect to a single Operations Management Suite workspace.</span></span>
* <span data-ttu-id="1dc57-184">Du kan lägga till servrar i kartan servrar tjänstgruppen manuellt via den **redigering** rutan kartor för servrarna synkroniseras inte omedelbart.</span><span class="sxs-lookup"><span data-stu-id="1dc57-184">Although you can add servers to the Service Map Servers Group manually through the **Authoring** pane, the maps for those servers are not synced immediately.</span></span>  <span data-ttu-id="1dc57-185">De kommer att synkroniseras från Tjänstkarta under nästa synkronisering cykel.</span><span class="sxs-lookup"><span data-stu-id="1dc57-185">They will be synced from Service Map during the next sync cycle.</span></span>
* <span data-ttu-id="1dc57-186">Om du gör några ändringar i diagrammet för distribuerade program som skapats av management pack ändringarna sannolikt att skrivas över på nästa synkronisering med Tjänstkartan.</span><span class="sxs-lookup"><span data-stu-id="1dc57-186">If you make any changes to the Distributed Application Diagrams created by the management pack, those changes will likely be overwritten on the next sync with Service Map.</span></span>

## <a name="create-a-service-principal"></a><span data-ttu-id="1dc57-187">Skapa ett huvudnamn för tjänsten</span><span class="sxs-lookup"><span data-stu-id="1dc57-187">Create a service principal</span></span>
<span data-ttu-id="1dc57-188">Officiell Azure-dokumentation om hur du skapar en tjänst huvudnamn finns:</span><span class="sxs-lookup"><span data-stu-id="1dc57-188">For official Azure documentation about creating a service principal, see:</span></span>
* [<span data-ttu-id="1dc57-189">Skapa ett huvudnamn för tjänsten med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="1dc57-189">Create a service principal by using PowerShell</span></span>](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authenticate-service-principal)
* [<span data-ttu-id="1dc57-190">Skapa ett huvudnamn för tjänsten med hjälp av Azure CLI</span><span class="sxs-lookup"><span data-stu-id="1dc57-190">Create a service principal by using Azure CLI</span></span>](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authenticate-service-principal-cli)
* [<span data-ttu-id="1dc57-191">Skapa ett huvudnamn för tjänsten med hjälp av Azure portal</span><span class="sxs-lookup"><span data-stu-id="1dc57-191">Create a service principal by using the Azure portal</span></span>](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal)

### <a name="feedback"></a><span data-ttu-id="1dc57-192">Feedback</span><span class="sxs-lookup"><span data-stu-id="1dc57-192">Feedback</span></span>
<span data-ttu-id="1dc57-193">Du har feedback för oss om Tjänstkarta eller den här dokumentationen?</span><span class="sxs-lookup"><span data-stu-id="1dc57-193">Do you have any feedback for us about Service Map or this documentation?</span></span> <span data-ttu-id="1dc57-194">Besök vår [User Voice sidan](https://feedback.azure.com/forums/267889-log-analytics/category/184492-service-map), där du kan föreslår funktioner eller rösta på förslag befintliga.</span><span class="sxs-lookup"><span data-stu-id="1dc57-194">Visit our [User Voice page](https://feedback.azure.com/forums/267889-log-analytics/category/184492-service-map), where you can suggest features or vote on existing suggestions.</span></span>
