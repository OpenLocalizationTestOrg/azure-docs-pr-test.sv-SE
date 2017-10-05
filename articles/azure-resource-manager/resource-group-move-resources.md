---
title: Flytta Azure-resurser till nya prenumerationen eller resursen grupp | Microsoft Docs
description: "Använd Azure Resource Manager för att flytta resurser till en ny resursgrupp eller prenumeration."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: ab7d42bd-8434-4026-a892-df4a97b60a9b
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/25/2017
ms.author: tomfitz
ms.openlocfilehash: e138f80e808968ab4bf5c11cfd5fd46fe4a1bcce
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="move-resources-to-new-resource-group-or-subscription"></a><span data-ttu-id="0690c-103">Flytta resurser till en ny resursgrupp eller prenumeration</span><span class="sxs-lookup"><span data-stu-id="0690c-103">Move resources to new resource group or subscription</span></span>
<span data-ttu-id="0690c-104">Det här avsnittet visar hur du flyttar resurser till en ny prenumeration eller en ny resursgrupp med samma prenumeration.</span><span class="sxs-lookup"><span data-stu-id="0690c-104">This topic shows you how to move resources to either a new subscription or a new resource group in the same subscription.</span></span> <span data-ttu-id="0690c-105">Du kan använda portalen, PowerShell, Azure CLI eller REST API för att flytta resursen.</span><span class="sxs-lookup"><span data-stu-id="0690c-105">You can use the portal, PowerShell, Azure CLI, or the REST API to move resource.</span></span> <span data-ttu-id="0690c-106">Flytta åtgärder i det här avsnittet kan du utan hjälp från Azure-supporten.</span><span class="sxs-lookup"><span data-stu-id="0690c-106">The move operations in this topic are available to you without any assistance from Azure support.</span></span>

<span data-ttu-id="0690c-107">När du flyttar resurser, låst både gruppen och målgruppen under åtgärden.</span><span class="sxs-lookup"><span data-stu-id="0690c-107">When moving resources, both the source group and the target group are locked during the operation.</span></span> <span data-ttu-id="0690c-108">Skriva och ta bort blockeras på resursgrupper tills flyttningen är klar.</span><span class="sxs-lookup"><span data-stu-id="0690c-108">Write and delete operations are blocked on the resource groups until the move completes.</span></span> <span data-ttu-id="0690c-109">Låset innebär det går inte att lägga till, uppdatera eller ta bort resurser i resursgrupper, men innebär inte resurserna som är låsta.</span><span class="sxs-lookup"><span data-stu-id="0690c-109">This lock means you cannot add, update, or delete resources in the resource groups, but it does not mean the resources are frozen.</span></span> <span data-ttu-id="0690c-110">Om du flyttar en SQL Server och dess databas till en ny resursgrupp påträffar ett program som använder databasen utan avbrott.</span><span class="sxs-lookup"><span data-stu-id="0690c-110">For example, if you move a SQL Server and its database to a new resource group, an application that uses the database experiences no downtime.</span></span> <span data-ttu-id="0690c-111">Det kan fortfarande läsa och skriva till databasen.</span><span class="sxs-lookup"><span data-stu-id="0690c-111">It can still read and write to the database.</span></span>

<span data-ttu-id="0690c-112">Du kan inte ändra platsen för resursen.</span><span class="sxs-lookup"><span data-stu-id="0690c-112">You cannot change the location of the resource.</span></span> <span data-ttu-id="0690c-113">Flytta en resurs bara flyttas till en ny resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="0690c-113">Moving a resource only moves it to a new resource group.</span></span> <span data-ttu-id="0690c-114">Den nya resursgruppen kan ha en annan plats, men som ändra inte platsen för resursen.</span><span class="sxs-lookup"><span data-stu-id="0690c-114">The new resource group may have a different location, but that does not change the location of the resource.</span></span>

> [!NOTE]
> <span data-ttu-id="0690c-115">Den här artikeln beskrivs hur du flyttar resurser i en befintlig Azure-konto erbjudanden.</span><span class="sxs-lookup"><span data-stu-id="0690c-115">This article describes how to move resources within an existing Azure account offering.</span></span> <span data-ttu-id="0690c-116">Om du vill ändra ditt Azure-konto erbjudande (till exempel uppgraderar från betala per användning före betala) när fortsätter att fungera med din befintliga resurser, se [växla din Azure-prenumeration till ett annat erbjudande](../billing/billing-how-to-switch-azure-offer.md).</span><span class="sxs-lookup"><span data-stu-id="0690c-116">If you actually want to change your Azure account offering (such as upgrading from pay-as-you-go to pre-pay) while continuing to work with your existing resources, see [Switch your Azure subscription to another offer](../billing/billing-how-to-switch-azure-offer.md).</span></span>
>
>

## <a name="checklist-before-moving-resources"></a><span data-ttu-id="0690c-117">Checklista innan du flyttar resurser</span><span class="sxs-lookup"><span data-stu-id="0690c-117">Checklist before moving resources</span></span>
<span data-ttu-id="0690c-118">Några viktiga steg måste utföras innan en resurs flyttas.</span><span class="sxs-lookup"><span data-stu-id="0690c-118">There are some important steps to perform before moving a resource.</span></span> <span data-ttu-id="0690c-119">Du kan undvika fel genom att verifiera dessa villkor.</span><span class="sxs-lookup"><span data-stu-id="0690c-119">By verifying these conditions, you can avoid errors.</span></span>

1. <span data-ttu-id="0690c-120">Käll- och -prenumerationer måste finnas inom samma [Azure Active Directory-klient](../active-directory/active-directory-howto-tenant.md).</span><span class="sxs-lookup"><span data-stu-id="0690c-120">The source and destination subscriptions must exist within the same [Azure Active Directory tenant](../active-directory/active-directory-howto-tenant.md).</span></span> <span data-ttu-id="0690c-121">Använd Azure PowerShell eller Azure CLI för att kontrollera att båda prenumerationer har samma klient-ID.</span><span class="sxs-lookup"><span data-stu-id="0690c-121">To check that both subscriptions have the same tenant ID, use Azure PowerShell or Azure CLI.</span></span>

  <span data-ttu-id="0690c-122">Använd för Azure PowerShell:</span><span class="sxs-lookup"><span data-stu-id="0690c-122">For Azure PowerShell, use:</span></span>

  ```powershell
  (Get-AzureRmSubscription -SubscriptionName "Example Subscription").TenantId
  ```

  <span data-ttu-id="0690c-123">Använd för Azure CLI 2.0:</span><span class="sxs-lookup"><span data-stu-id="0690c-123">For Azure CLI 2.0, use:</span></span>

  ```azurecli
  az account show --subscription "Example Subscription" --query tenantId
  ```

  <span data-ttu-id="0690c-124">Om klient-ID: N för käll- och -prenumerationer inte är samma, kan du försöka ändra katalogen för prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="0690c-124">If the tenant IDs for the source and destination subscriptions are not the same, you can attempt to change the directory for the subscription.</span></span> <span data-ttu-id="0690c-125">Det här alternativet är endast tillgänglig för administratörer som har loggat in med ett Microsoft-konto (inte ett organisationskonto).</span><span class="sxs-lookup"><span data-stu-id="0690c-125">However, this option is only available to Service Administrators who are signed in with a Microsoft account (not an organizational account).</span></span> <span data-ttu-id="0690c-126">Logga in för att försöka ändra katalogen till den [klassiska portalen](https://manage.windowsazure.com/), och välj **inställningar**, och välj prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="0690c-126">To attempt changing the directory, log in to the [classic portal](https://manage.windowsazure.com/), and select **Settings**, and select the subscription.</span></span> <span data-ttu-id="0690c-127">Om den **redigera katalog** ikonen är tillgänglig kan du välja att ändra de associerade Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="0690c-127">If the **Edit Directory** icon is available, select it to change the associated Azure Active Directory.</span></span>

  ![Redigera katalog](./media/resource-group-move-resources/edit-directory.png)

  <span data-ttu-id="0690c-129">Om ikonen inte är tillgängligt måste du kontakta supporten om du vill flytta resurserna till en ny klient.</span><span class="sxs-lookup"><span data-stu-id="0690c-129">If that icon is not available, you must contact support to move the resources to a new tenant.</span></span>

2. <span data-ttu-id="0690c-130">Tjänsten måste göra det möjligt att flytta resurser.</span><span class="sxs-lookup"><span data-stu-id="0690c-130">The service must enable the ability to move resources.</span></span> <span data-ttu-id="0690c-131">Det här avsnittet visas vilka tjänster kan flytta resurser och tjänster som gör inte flytta resurser.</span><span class="sxs-lookup"><span data-stu-id="0690c-131">This topic lists which services enable moving resources and which services do not enable moving resources.</span></span>
3. <span data-ttu-id="0690c-132">Målprenumerationen måste vara registrerad för resursprovidern för den resurs som flyttas.</span><span class="sxs-lookup"><span data-stu-id="0690c-132">The destination subscription must be registered for the resource provider of the resource being moved.</span></span> <span data-ttu-id="0690c-133">Om inte, du får ett felmeddelande om att den **prenumerationen har inte registrerats för en resurstyp**.</span><span class="sxs-lookup"><span data-stu-id="0690c-133">If not, you receive an error stating that the **subscription is not registered for a resource type**.</span></span> <span data-ttu-id="0690c-134">Du kan stöta på detta problem när en resurs flyttas till en ny prenumeration, men prenumerationen aldrig har använts med den resurstypen.</span><span class="sxs-lookup"><span data-stu-id="0690c-134">You might encounter this problem when moving a resource to a new subscription, but that subscription has never been used with that resource type.</span></span> <span data-ttu-id="0690c-135">Information om hur du kontrollerar registreringsstatus och registrerar resursprovidrar finns i [Resursprovidrar och typer](resource-manager-supported-services.md).</span><span class="sxs-lookup"><span data-stu-id="0690c-135">To learn how to check the registration status and register resource providers, see [Resource providers and types](resource-manager-supported-services.md).</span></span>

## <a name="when-to-call-support"></a><span data-ttu-id="0690c-136">När anropet stöd</span><span class="sxs-lookup"><span data-stu-id="0690c-136">When to call support</span></span>
<span data-ttu-id="0690c-137">Du kan flytta de flesta resurser via självbetjäning åtgärder visas i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="0690c-137">You can move most resources through the self-service operations shown in this topic.</span></span> <span data-ttu-id="0690c-138">Använd åtgärderna självbetjäning till:</span><span class="sxs-lookup"><span data-stu-id="0690c-138">Use the self-service operations to:</span></span>

* <span data-ttu-id="0690c-139">Flytta resurshanterarens resurser.</span><span class="sxs-lookup"><span data-stu-id="0690c-139">Move Resource Manager resources.</span></span>
* <span data-ttu-id="0690c-140">Flytta klassiska resurser enligt den [klassisk distribution begränsningar](#classic-deployment-limitations).</span><span class="sxs-lookup"><span data-stu-id="0690c-140">Move classic resources according to the [classic deployment limitations](#classic-deployment-limitations).</span></span>

<span data-ttu-id="0690c-141">Ring supporten när du behöver:</span><span class="sxs-lookup"><span data-stu-id="0690c-141">Call support when you need to:</span></span>

* <span data-ttu-id="0690c-142">Flytta dina resurser till en ny Azure-konto (och Azure Active Directory-klient).</span><span class="sxs-lookup"><span data-stu-id="0690c-142">Move your resources to a new Azure account (and Azure Active Directory tenant).</span></span>
* <span data-ttu-id="0690c-143">Flytta klassiska resurser men har problem med begränsningar.</span><span class="sxs-lookup"><span data-stu-id="0690c-143">Move classic resources but are having trouble with the limitations.</span></span>

## <a name="services-that-enable-move"></a><span data-ttu-id="0690c-144">Tjänster som gör att flytta</span><span class="sxs-lookup"><span data-stu-id="0690c-144">Services that enable move</span></span>
<span data-ttu-id="0690c-145">För tillfället är de tjänster som gör att du flyttar till en ny resursgrupp och en prenumeration</span><span class="sxs-lookup"><span data-stu-id="0690c-145">For now, the services that enable moving to both a new resource group and subscription are:</span></span>

* <span data-ttu-id="0690c-146">API Management</span><span class="sxs-lookup"><span data-stu-id="0690c-146">API Management</span></span>
* <span data-ttu-id="0690c-147">Apptjänst-appar (webbprogram) - finns [Apptjänst begränsningar](#app-service-limitations)</span><span class="sxs-lookup"><span data-stu-id="0690c-147">App Service apps (web apps) - see [App Service limitations](#app-service-limitations)</span></span>
* <span data-ttu-id="0690c-148">Application Insights</span><span class="sxs-lookup"><span data-stu-id="0690c-148">Application Insights</span></span>
* <span data-ttu-id="0690c-149">Automation</span><span class="sxs-lookup"><span data-stu-id="0690c-149">Automation</span></span>
* <span data-ttu-id="0690c-150">Batch</span><span class="sxs-lookup"><span data-stu-id="0690c-150">Batch</span></span>
* <span data-ttu-id="0690c-151">Bing Maps</span><span class="sxs-lookup"><span data-stu-id="0690c-151">Bing Maps</span></span>
* <span data-ttu-id="0690c-152">CDN</span><span class="sxs-lookup"><span data-stu-id="0690c-152">CDN</span></span>
* <span data-ttu-id="0690c-153">Molntjänster - Se [klassisk distribution begränsningar](#classic-deployment-limitations)</span><span class="sxs-lookup"><span data-stu-id="0690c-153">Cloud Services - see [Classic deployment limitations](#classic-deployment-limitations)</span></span>
* <span data-ttu-id="0690c-154">Cognitive Services</span><span class="sxs-lookup"><span data-stu-id="0690c-154">Cognitive Services</span></span>
* <span data-ttu-id="0690c-155">Content Moderator</span><span class="sxs-lookup"><span data-stu-id="0690c-155">Content Moderator</span></span>
* <span data-ttu-id="0690c-156">Data Catalog</span><span class="sxs-lookup"><span data-stu-id="0690c-156">Data Catalog</span></span>
* <span data-ttu-id="0690c-157">Data Factory</span><span class="sxs-lookup"><span data-stu-id="0690c-157">Data Factory</span></span>
* <span data-ttu-id="0690c-158">Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="0690c-158">Data Lake Analytics</span></span>
* <span data-ttu-id="0690c-159">Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="0690c-159">Data Lake Store</span></span>
* <span data-ttu-id="0690c-160">DNS</span><span class="sxs-lookup"><span data-stu-id="0690c-160">DNS</span></span>
* <span data-ttu-id="0690c-161">Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="0690c-161">Azure Cosmos DB</span></span>
* <span data-ttu-id="0690c-162">Händelsehubbar</span><span class="sxs-lookup"><span data-stu-id="0690c-162">Event Hubs</span></span>
* <span data-ttu-id="0690c-163">HDInsight-kluster - finns [HDInsight begränsningar](#hdinsight-limitations)</span><span class="sxs-lookup"><span data-stu-id="0690c-163">HDInsight clusters - see [HDInsight limitations](#hdinsight-limitations)</span></span>
* <span data-ttu-id="0690c-164">IoT-hubbar</span><span class="sxs-lookup"><span data-stu-id="0690c-164">IoT Hubs</span></span>
* <span data-ttu-id="0690c-165">Key Vault</span><span class="sxs-lookup"><span data-stu-id="0690c-165">Key Vault</span></span>
* <span data-ttu-id="0690c-166">Belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="0690c-166">Load Balancers</span></span>
* <span data-ttu-id="0690c-167">Logic Apps</span><span class="sxs-lookup"><span data-stu-id="0690c-167">Logic Apps</span></span>
* <span data-ttu-id="0690c-168">Machine Learning</span><span class="sxs-lookup"><span data-stu-id="0690c-168">Machine Learning</span></span>
* <span data-ttu-id="0690c-169">Media Services</span><span class="sxs-lookup"><span data-stu-id="0690c-169">Media Services</span></span>
* <span data-ttu-id="0690c-170">Mobile Engagement</span><span class="sxs-lookup"><span data-stu-id="0690c-170">Mobile Engagement</span></span>
* <span data-ttu-id="0690c-171">Notification Hubs</span><span class="sxs-lookup"><span data-stu-id="0690c-171">Notification Hubs</span></span>
* <span data-ttu-id="0690c-172">Åtgärdsinformation</span><span class="sxs-lookup"><span data-stu-id="0690c-172">Operational Insights</span></span>
* <span data-ttu-id="0690c-173">Operations Management</span><span class="sxs-lookup"><span data-stu-id="0690c-173">Operations Management</span></span>
* <span data-ttu-id="0690c-174">Power BI</span><span class="sxs-lookup"><span data-stu-id="0690c-174">Power BI</span></span>
* <span data-ttu-id="0690c-175">Redis Cache</span><span class="sxs-lookup"><span data-stu-id="0690c-175">Redis Cache</span></span>
* <span data-ttu-id="0690c-176">Scheduler</span><span class="sxs-lookup"><span data-stu-id="0690c-176">Scheduler</span></span>
* <span data-ttu-id="0690c-177">Söka</span><span class="sxs-lookup"><span data-stu-id="0690c-177">Search</span></span>
* <span data-ttu-id="0690c-178">Serverhantering</span><span class="sxs-lookup"><span data-stu-id="0690c-178">Server Management</span></span>
* <span data-ttu-id="0690c-179">Service Bus</span><span class="sxs-lookup"><span data-stu-id="0690c-179">Service Bus</span></span>
* <span data-ttu-id="0690c-180">Service Fabric</span><span class="sxs-lookup"><span data-stu-id="0690c-180">Service Fabric</span></span>
* <span data-ttu-id="0690c-181">Lagring</span><span class="sxs-lookup"><span data-stu-id="0690c-181">Storage</span></span>
* <span data-ttu-id="0690c-182">Storage (klassisk) - finns [klassisk distribution begränsningar](#classic-deployment-limitations)</span><span class="sxs-lookup"><span data-stu-id="0690c-182">Storage (classic) - see [Classic deployment limitations](#classic-deployment-limitations)</span></span>
* <span data-ttu-id="0690c-183">Stream Analytics - Stream Analytics-jobb inte kan flyttas när du kör i läget.</span><span class="sxs-lookup"><span data-stu-id="0690c-183">Stream Analytics - Stream Analytics jobs cannot be moved when in running state.</span></span>
* <span data-ttu-id="0690c-184">SQL Database-server - databasen och servern måste finnas i samma resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="0690c-184">SQL Database server - The database and server must reside in the same resource group.</span></span> <span data-ttu-id="0690c-185">När du flyttar en SQLServer, flyttas även alla databaser.</span><span class="sxs-lookup"><span data-stu-id="0690c-185">When you move a SQL server, all its databases are also moved.</span></span>
* <span data-ttu-id="0690c-186">Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="0690c-186">Traffic Manager</span></span>
* <span data-ttu-id="0690c-187">Virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="0690c-187">Virtual Machines</span></span>
* <span data-ttu-id="0690c-188">Virtuella datorer med certifikat som lagras i Key Vault - flytta till ny resurs gruppen i samma prenumeration är aktiverad men flytta mellan prenumeration har inte aktiverats.</span><span class="sxs-lookup"><span data-stu-id="0690c-188">Virtual Machines with certificate stored in Key Vault - Move to new resource group in same subscription is enabled, but cross subscription move is not enabled.</span></span>
* <span data-ttu-id="0690c-189">Virtuella datorer (klassisk) - finns [klassisk distribution begränsningar](#classic-deployment-limitations)</span><span class="sxs-lookup"><span data-stu-id="0690c-189">Virtual Machines (classic) - see [Classic deployment limitations](#classic-deployment-limitations)</span></span>
* <span data-ttu-id="0690c-190">Skalningsuppsättningar för Virtual Machines</span><span class="sxs-lookup"><span data-stu-id="0690c-190">Virtual Machine Scale Sets</span></span>
* <span data-ttu-id="0690c-191">Virtuella nätverk - just nu, ett peered virtuellt nätverk går inte att flytta tills VNet-peering har inaktiverats.</span><span class="sxs-lookup"><span data-stu-id="0690c-191">Virtual Networks - Currently, a peered Virtual Network cannot be moved until VNet peering has been disabled.</span></span> <span data-ttu-id="0690c-192">När inaktiverat, det virtuella nätverket kan flyttas utan problem och VNet-peering kan aktiveras.</span><span class="sxs-lookup"><span data-stu-id="0690c-192">Once disabled, the Virtual Network can be moved successfully and the VNet peering can be enabled.</span></span> <span data-ttu-id="0690c-193">Ett virtuellt nätverk kan dessutom inte flyttas till en annan prenumeration om det virtuella nätverket innehåller inget undernät med resursnavigeringslänkar.</span><span class="sxs-lookup"><span data-stu-id="0690c-193">In addition, a Virtual Network cannot be moved to a different subscription if the Virtual Network contains any subnet with resource navigation links.</span></span> <span data-ttu-id="0690c-194">Ett undernät för virtuellt nätverk har till exempel en resurslänk för navigering när en resurs för Microsoft.Cache redis har distribuerats i det här undernätet.</span><span class="sxs-lookup"><span data-stu-id="0690c-194">For example, a Virtual Network subnet has a resource navigation link when a Microsoft.Cache redis resource is deployed into this subnet.</span></span>
* <span data-ttu-id="0690c-195">VPN-gateway</span><span class="sxs-lookup"><span data-stu-id="0690c-195">VPN Gateway</span></span>


## <a name="services-that-do-not-enable-move"></a><span data-ttu-id="0690c-196">Tjänster som inte gör flytta</span><span class="sxs-lookup"><span data-stu-id="0690c-196">Services that do not enable move</span></span>
<span data-ttu-id="0690c-197">De tjänster som för närvarande inte aktiverar flytta en resurs är:</span><span class="sxs-lookup"><span data-stu-id="0690c-197">The services that currently do not enable moving a resource are:</span></span>

* <span data-ttu-id="0690c-198">AD DS</span><span class="sxs-lookup"><span data-stu-id="0690c-198">AD Domain Services</span></span>
* <span data-ttu-id="0690c-199">AD-Hybrid-tjänsten för hälsotillstånd</span><span class="sxs-lookup"><span data-stu-id="0690c-199">AD Hybrid Health Service</span></span>
* <span data-ttu-id="0690c-200">Application Gateway</span><span class="sxs-lookup"><span data-stu-id="0690c-200">Application Gateway</span></span>
* <span data-ttu-id="0690c-201">Tillgänglighetsuppsättningar med virtuella datorer med hanterade diskar</span><span class="sxs-lookup"><span data-stu-id="0690c-201">Availability sets with Virtual Machines with Managed Disks</span></span>
* <span data-ttu-id="0690c-202">BizTalk Services</span><span class="sxs-lookup"><span data-stu-id="0690c-202">BizTalk Services</span></span>
* <span data-ttu-id="0690c-203">Container Service</span><span class="sxs-lookup"><span data-stu-id="0690c-203">Container Service</span></span>
* <span data-ttu-id="0690c-204">Express Route</span><span class="sxs-lookup"><span data-stu-id="0690c-204">Express Route</span></span>
* <span data-ttu-id="0690c-205">DevTest Labs - flyttar till en ny resursgrupp i samma prenumeration har aktiverats men flytta mellan prenumeration har inte aktiverats.</span><span class="sxs-lookup"><span data-stu-id="0690c-205">DevTest Labs - Move to new resource group in same subscription is enabled, but cross subscription move is not enabled.</span></span>
* <span data-ttu-id="0690c-206">Dynamics LCS</span><span class="sxs-lookup"><span data-stu-id="0690c-206">Dynamics LCS</span></span>
* <span data-ttu-id="0690c-207">Bilder som har skapats från hanterade diskar</span><span class="sxs-lookup"><span data-stu-id="0690c-207">Images created from Managed Disks</span></span>
* <span data-ttu-id="0690c-208">Managed Disks</span><span class="sxs-lookup"><span data-stu-id="0690c-208">Managed Disks</span></span>
* <span data-ttu-id="0690c-209">Hanterade program</span><span class="sxs-lookup"><span data-stu-id="0690c-209">Managed Applications</span></span>
* <span data-ttu-id="0690c-210">Recovery Services-ventilen - också vill inte flytta beräknings-, nätverks- och resurser som är associerade med Recovery Services-valvet finns [återställningstjänster begränsningar](#recovery-services-limitations).</span><span class="sxs-lookup"><span data-stu-id="0690c-210">Recovery Services vault - also do not move the Compute, Network, and Storage resources associated with the Recovery Services vault, see [Recovery Services limitations](#recovery-services-limitations).</span></span>
* <span data-ttu-id="0690c-211">Säkerhet</span><span class="sxs-lookup"><span data-stu-id="0690c-211">Security</span></span>
* <span data-ttu-id="0690c-212">Ögonblicksbilder som skapats från hanterade diskar</span><span class="sxs-lookup"><span data-stu-id="0690c-212">Snapshots created from Managed Disks</span></span>
* <span data-ttu-id="0690c-213">StorSimple Enhetshanteraren</span><span class="sxs-lookup"><span data-stu-id="0690c-213">StorSimple Device Manager</span></span>
* <span data-ttu-id="0690c-214">Virtuella datorer med hanterade diskar</span><span class="sxs-lookup"><span data-stu-id="0690c-214">Virtual Machines with Managed Disks</span></span>
* <span data-ttu-id="0690c-215">Virtuella nätverk (klassiskt) - finns [klassisk distribution begränsningar](#classic-deployment-limitations)</span><span class="sxs-lookup"><span data-stu-id="0690c-215">Virtual Networks (classic) - see [Classic deployment limitations](#classic-deployment-limitations)</span></span>
* <span data-ttu-id="0690c-216">Virtuella datorer som skapats från Marketplace resurser - kan inte flyttas mellan prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="0690c-216">Virtual Machines created from Marketplace resources - cannot be moved across subscriptions.</span></span> <span data-ttu-id="0690c-217">Resursen måste avetableras i den aktuella prenumerationen och distribuerade igen på den nya prenumerationen</span><span class="sxs-lookup"><span data-stu-id="0690c-217">Resource needs to be deprovisioned in the current subscription and deployed again in the new subscription</span></span>

## <a name="app-service-limitations"></a><span data-ttu-id="0690c-218">Begränsningar för App Service</span><span class="sxs-lookup"><span data-stu-id="0690c-218">App Service limitations</span></span>
<span data-ttu-id="0690c-219">När du arbetar med Apptjänst-appar kan flytta du inte endast en App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="0690c-219">When working with App Service apps, you cannot move only an App Service plan.</span></span> <span data-ttu-id="0690c-220">Om du vill flytta Apptjänst-appar är alternativen:</span><span class="sxs-lookup"><span data-stu-id="0690c-220">To move App Service apps, your options are:</span></span>

* <span data-ttu-id="0690c-221">Flytta App Service-plan och alla andra resurser i Apptjänst i resursgruppen till en ny resursgrupp som inte redan har App Service-resurser.</span><span class="sxs-lookup"><span data-stu-id="0690c-221">Move the App Service plan and all other App Service resources in that resource group to a new resource group that does not already have App Service resources.</span></span> <span data-ttu-id="0690c-222">Det här kravet innebär måste du flytta även Apptjänst resurser som inte är associerad med App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="0690c-222">This requirement means you must move even the App Service resources that are not associated with the App Service plan.</span></span>
* <span data-ttu-id="0690c-223">Flytta apparna till en annan resursgrupp men behålla alla programtjänstplaner i den ursprungliga resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="0690c-223">Move the apps to a different resource group, but keep all App Service plans in the original resource group.</span></span>

<span data-ttu-id="0690c-224">Programtjänstplanen behöver inte finnas i samma resursgrupp som appen för appen ska fungera korrekt.</span><span class="sxs-lookup"><span data-stu-id="0690c-224">The App Service plan does not need to reside in the same resource group as the app for the app to function correctly.</span></span>

<span data-ttu-id="0690c-225">Om till exempel din resursgrupp innehåller:</span><span class="sxs-lookup"><span data-stu-id="0690c-225">For example, if your resource group contains:</span></span>

* <span data-ttu-id="0690c-226">**Web-a** som är associerad med **planera en**</span><span class="sxs-lookup"><span data-stu-id="0690c-226">**web-a** which is associated with **plan-a**</span></span>
* <span data-ttu-id="0690c-227">**Web-b** som är associerad med **plan b**</span><span class="sxs-lookup"><span data-stu-id="0690c-227">**web-b** which is associated with **plan-b**</span></span>

<span data-ttu-id="0690c-228">Alternativen är:</span><span class="sxs-lookup"><span data-stu-id="0690c-228">Your options are:</span></span>

* <span data-ttu-id="0690c-229">Flytta **web-a**, **planera en**, **web-b**, och **plan b**</span><span class="sxs-lookup"><span data-stu-id="0690c-229">Move **web-a**, **plan-a**, **web-b**, and **plan-b**</span></span>
* <span data-ttu-id="0690c-230">Flytta **web-a** och **web-b**</span><span class="sxs-lookup"><span data-stu-id="0690c-230">Move **web-a** and **web-b**</span></span>
* <span data-ttu-id="0690c-231">Flytta **web-a**</span><span class="sxs-lookup"><span data-stu-id="0690c-231">Move **web-a**</span></span>
* <span data-ttu-id="0690c-232">Flytta **web-b**</span><span class="sxs-lookup"><span data-stu-id="0690c-232">Move **web-b**</span></span>

<span data-ttu-id="0690c-233">Alla andra kombinationer involverar lämnar bakom en resurstyp som kan finnas kvar när du flyttar en apptjänstplan (någon typ av App Service-resurs).</span><span class="sxs-lookup"><span data-stu-id="0690c-233">All other combinations involve leaving behind a resource type that can't be left behind when moving an App Service plan (any type of App Service resource).</span></span>

<span data-ttu-id="0690c-234">Om ditt webbprogram finns i en annan resursgrupp än dess App Service-plan, men du vill flytta både en ny resursgrupp, måste du flytta i två steg.</span><span class="sxs-lookup"><span data-stu-id="0690c-234">If your web app resides in a different resource group than its App Service plan but you want to move both to a new resource group, you must perform the move in two steps.</span></span> <span data-ttu-id="0690c-235">Exempel:</span><span class="sxs-lookup"><span data-stu-id="0690c-235">For example:</span></span>

* <span data-ttu-id="0690c-236">**Web-a** finns i **web-grupp**</span><span class="sxs-lookup"><span data-stu-id="0690c-236">**web-a** resides in **web-group**</span></span>
* <span data-ttu-id="0690c-237">**Planera en** finns i **plan-grupp**</span><span class="sxs-lookup"><span data-stu-id="0690c-237">**plan-a** resides in **plan-group**</span></span>
* <span data-ttu-id="0690c-238">Du vill **web-a** och **planera en** finns i **kombineras grupp**</span><span class="sxs-lookup"><span data-stu-id="0690c-238">You want **web-a** and **plan-a** to reside in **combined-group**</span></span>

<span data-ttu-id="0690c-239">För att åstadkomma detta steg, utför du två separata flytta åtgärderna i följande ordning:</span><span class="sxs-lookup"><span data-stu-id="0690c-239">To accomplish this move, perform two separate move operations in the following sequence:</span></span>

1. <span data-ttu-id="0690c-240">Flytta den **web-a** till **plan-grupp**</span><span class="sxs-lookup"><span data-stu-id="0690c-240">Move the **web-a** to **plan-group**</span></span>
2. <span data-ttu-id="0690c-241">Flytta **web-a** och **planera en** till **kombineras grupp**.</span><span class="sxs-lookup"><span data-stu-id="0690c-241">Move **web-a** and **plan-a** to **combined-group**.</span></span>

<span data-ttu-id="0690c-242">Du kan flytta ett certifikat för App Service till en ny resursgrupp eller prenumeration utan problem.</span><span class="sxs-lookup"><span data-stu-id="0690c-242">You can move an App Service Certificate to a new resource group or subscription without any issues.</span></span> <span data-ttu-id="0690c-243">Om webbappen innehåller ett SSL-certifikat som du köpt externt och överförs till appen, måste du radera certifikatet innan du flyttar webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="0690c-243">However, if your web app includes an SSL certificate that you purchased externally and uploaded to the app, you must delete the certificate before moving the web app.</span></span> <span data-ttu-id="0690c-244">Exempelvis kan du utföra följande steg:</span><span class="sxs-lookup"><span data-stu-id="0690c-244">For example, you can perform the following steps:</span></span>

1. <span data-ttu-id="0690c-245">Ta bort det överförda certifikatet från webbappen</span><span class="sxs-lookup"><span data-stu-id="0690c-245">Delete the uploaded certificate from the web app</span></span>
2. <span data-ttu-id="0690c-246">Flytta webbappen</span><span class="sxs-lookup"><span data-stu-id="0690c-246">Move the web app</span></span>
3. <span data-ttu-id="0690c-247">Överför certifikatet till webbappen</span><span class="sxs-lookup"><span data-stu-id="0690c-247">Upload the certificate to the web app</span></span>

## <a name="recovery-services-limitations"></a><span data-ttu-id="0690c-248">Recovery Services-begränsningar</span><span class="sxs-lookup"><span data-stu-id="0690c-248">Recovery Services limitations</span></span>
<span data-ttu-id="0690c-249">Flytta inte har aktiverats för lagring, nätverk, eller beräkningsresurser som används för att ställa in katastrofåterställning med Azure Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="0690c-249">Move is not enabled for Storage, Network, or Compute resources used to set up disaster recovery with Azure Site Recovery.</span></span>

<span data-ttu-id="0690c-250">Anta att du har ställt in replikering av din lokala datorer till ett lagringskonto (Storage1) och vill att den skydda datorn att starta efter en redundansväxling till Azure som en virtuell dator (VM1) ansluten till ett virtuellt nätverk (Network1).</span><span class="sxs-lookup"><span data-stu-id="0690c-250">For example, suppose you have set up replication of your on-premises machines to a storage account (Storage1) and want the protected machine to come up after failover to Azure as a virtual machine (VM1) attached to a virtual network (Network1).</span></span> <span data-ttu-id="0690c-251">Du kan inte flytta resurserna Azure - Storage1 VM1 och Network1 - över resursgrupper inom samma prenumeration eller alla prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="0690c-251">You cannot move any of these Azure resources - Storage1, VM1, and Network1 - across resource groups within the same subscription or across subscriptions.</span></span>

## <a name="hdinsight-limitations"></a><span data-ttu-id="0690c-252">HDInsight-begränsningar</span><span class="sxs-lookup"><span data-stu-id="0690c-252">HDInsight limitations</span></span>

<span data-ttu-id="0690c-253">Du kan flytta HDInsight-kluster till en ny prenumeration eller resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="0690c-253">You can move HDInsight clusters to a new subscription or resource group.</span></span> <span data-ttu-id="0690c-254">Men kan inte du flytta alla prenumerationer som nätverksresurser som är kopplad till HDInsight-klustret (till exempel virtuella nätverk, nätverkskort eller belastningsutjämning).</span><span class="sxs-lookup"><span data-stu-id="0690c-254">However, you cannot move across subscriptions the networking resources linked to the HDInsight cluster (such as the virtual network, NIC, or load balancer).</span></span> <span data-ttu-id="0690c-255">Dessutom kan flytta du inte till en ny resursgrupp ett nätverkskort som är kopplad till en virtuell dator för klustret.</span><span class="sxs-lookup"><span data-stu-id="0690c-255">In addition, you cannot move to a new resource group a NIC that is attached to a virtual machine for the cluster.</span></span>

<span data-ttu-id="0690c-256">När du flyttar ett HDInsight-kluster till en ny prenumeration kan du först flytta andra resurser (till exempel storage-konto).</span><span class="sxs-lookup"><span data-stu-id="0690c-256">When moving an HDInsight cluster to a new subscription, first move other resources (like the storage account).</span></span> <span data-ttu-id="0690c-257">Flytta sedan HDInsight-kluster av sig själv.</span><span class="sxs-lookup"><span data-stu-id="0690c-257">Then, move the HDInsight cluster by itself.</span></span>

## <a name="classic-deployment-limitations"></a><span data-ttu-id="0690c-258">Klassisk distribution begränsningar</span><span class="sxs-lookup"><span data-stu-id="0690c-258">Classic deployment limitations</span></span>
<span data-ttu-id="0690c-259">Alternativ för att flytta resurser har distribuerats via den klassiska modellen variera beroende på om du flyttar resurser inom en prenumeration eller till en ny prenumeration.</span><span class="sxs-lookup"><span data-stu-id="0690c-259">The options for moving resources deployed through the classic model differ based on whether you are moving the resources within a subscription or to a new subscription.</span></span>

### <a name="same-subscription"></a><span data-ttu-id="0690c-260">Samma prenumeration</span><span class="sxs-lookup"><span data-stu-id="0690c-260">Same subscription</span></span>
<span data-ttu-id="0690c-261">När du flyttar resurser från en resursgrupp till en annan resursgrupp inom samma prenumeration, gäller följande begränsningar:</span><span class="sxs-lookup"><span data-stu-id="0690c-261">When moving resources from one resource group to another resource group within the same subscription, the following restrictions apply:</span></span>

* <span data-ttu-id="0690c-262">Virtuella nätverk (klassiskt) kan inte flyttas.</span><span class="sxs-lookup"><span data-stu-id="0690c-262">Virtual networks (classic) cannot be moved.</span></span>
* <span data-ttu-id="0690c-263">Virtuella datorer (klassisk) måste flyttas med Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="0690c-263">Virtual machines (classic) must be moved with the cloud service.</span></span>
* <span data-ttu-id="0690c-264">Molntjänsten kan bara flyttas när flytten omfattar alla virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="0690c-264">Cloud service can only be moved when the move includes all its virtual machines.</span></span>
* <span data-ttu-id="0690c-265">Endast en molnbaserad tjänst kan flyttas åt gången.</span><span class="sxs-lookup"><span data-stu-id="0690c-265">Only one cloud service can be moved at a time.</span></span>
* <span data-ttu-id="0690c-266">Endast en storage-konto (klassiskt) kan flyttas åt gången.</span><span class="sxs-lookup"><span data-stu-id="0690c-266">Only one storage account (classic) can be moved at a time.</span></span>
* <span data-ttu-id="0690c-267">Storage-konto (klassiskt) kan inte flyttas på samma gång med en virtuell dator eller en tjänst i molnet.</span><span class="sxs-lookup"><span data-stu-id="0690c-267">Storage account (classic) cannot be moved in the same operation with a virtual machine or a cloud service.</span></span>

<span data-ttu-id="0690c-268">Om du vill flytta klassiska resurser till en ny resursgrupp inom samma prenumeration, använder du standard move-åtgärder via den [portal](#use-portal), [Azure PowerShell](#use-powershell), [Azure CLI](#use-azure-cli), eller [REST API](#use-rest-api).</span><span class="sxs-lookup"><span data-stu-id="0690c-268">To move classic resources to a new resource group within the same subscription, use the standard move operations through the [portal](#use-portal), [Azure PowerShell](#use-powershell), [Azure CLI](#use-azure-cli), or [REST API](#use-rest-api).</span></span> <span data-ttu-id="0690c-269">Du kan använda samma åtgärder som du använder för att flytta Resource Manager-resurser.</span><span class="sxs-lookup"><span data-stu-id="0690c-269">You use the same operations as you use for moving Resource Manager resources.</span></span>

### <a name="new-subscription"></a><span data-ttu-id="0690c-270">Ny prenumeration</span><span class="sxs-lookup"><span data-stu-id="0690c-270">New subscription</span></span>
<span data-ttu-id="0690c-271">När resurserna flyttas till en ny prenumeration, gäller följande begränsningar:</span><span class="sxs-lookup"><span data-stu-id="0690c-271">When moving resources to a new subscription, the following restrictions apply:</span></span>

* <span data-ttu-id="0690c-272">Alla klassiska resurser i prenumerationen måste flyttas samtidigt.</span><span class="sxs-lookup"><span data-stu-id="0690c-272">All classic resources in the subscription must be moved in the same operation.</span></span>
* <span data-ttu-id="0690c-273">Målprenumerationen får inte innehålla andra klassiska resurser.</span><span class="sxs-lookup"><span data-stu-id="0690c-273">The target subscription must not contain any other classic resources.</span></span>
* <span data-ttu-id="0690c-274">Flyttningen kan endast begäras via en separat REST-API för klassiska flyttar.</span><span class="sxs-lookup"><span data-stu-id="0690c-274">The move can only be requested through a separate REST API for classic moves.</span></span> <span data-ttu-id="0690c-275">Kommandona flytta standard Resource Manager fungerar inte när du flyttar klassiska resurser till en ny prenumeration.</span><span class="sxs-lookup"><span data-stu-id="0690c-275">The standard Resource Manager move commands do not work when moving classic resources to a new subscription.</span></span>

<span data-ttu-id="0690c-276">Flytta klassiska resurser till en ny prenumeration genom att använda REST-åtgärder som är specifika för klassiska resurser.</span><span class="sxs-lookup"><span data-stu-id="0690c-276">To move classic resources to a new subscription, use the REST operations that are specific to classic resources.</span></span> <span data-ttu-id="0690c-277">Utför följande steg om du vill använda REST:</span><span class="sxs-lookup"><span data-stu-id="0690c-277">To use REST, perform the following steps:</span></span>

1. <span data-ttu-id="0690c-278">Kontrollera om källprenumerationen kan delta i en flytt mellan prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="0690c-278">Check if the source subscription can participate in a cross-subscription move.</span></span> <span data-ttu-id="0690c-279">Använd följande åtgärd:</span><span class="sxs-lookup"><span data-stu-id="0690c-279">Use the following operation:</span></span>

  ```HTTP   
  POST https://management.azure.com/subscriptions/{sourceSubscriptionId}/providers/Microsoft.ClassicCompute/validateSubscriptionMoveAvailability?api-version=2016-04-01
  ```

     <span data-ttu-id="0690c-280">I frågans brödtext är:</span><span class="sxs-lookup"><span data-stu-id="0690c-280">In the request body, include:</span></span>

  ```json
  {
    "role": "source"
  }
  ```

     <span data-ttu-id="0690c-281">Svaret för valideringen har följande format:</span><span class="sxs-lookup"><span data-stu-id="0690c-281">The response for the validation operation is in the following format:</span></span>

  ```json
  {
    "status": "{status}",
    "reasons": [
      "reason1",
      "reason2"
    ]
  }
  ```

2. <span data-ttu-id="0690c-282">Kontrollera om målprenumerationen kan delta i en flytt mellan prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="0690c-282">Check if the destination subscription can participate in a cross-subscription move.</span></span> <span data-ttu-id="0690c-283">Använd följande åtgärd:</span><span class="sxs-lookup"><span data-stu-id="0690c-283">Use the following operation:</span></span>

  ```HTTP
  POST https://management.azure.com/subscriptions/{destinationSubscriptionId}/providers/Microsoft.ClassicCompute/validateSubscriptionMoveAvailability?api-version=2016-04-01
  ```

     <span data-ttu-id="0690c-284">I frågans brödtext är:</span><span class="sxs-lookup"><span data-stu-id="0690c-284">In the request body, include:</span></span>

  ```json
  {
    "role": "target"
  }
  ```

     <span data-ttu-id="0690c-285">Svaret är i samma format som källa prenumeration verifieringen.</span><span class="sxs-lookup"><span data-stu-id="0690c-285">The response is in the same format as the source subscription validation.</span></span>
3. <span data-ttu-id="0690c-286">Om båda prenumerationer har klarat valideringen, flytta alla klassiska resurser från en prenumeration till en annan prenumeration med följande åtgärd:</span><span class="sxs-lookup"><span data-stu-id="0690c-286">If both subscriptions pass validation, move all classic resources from one subscription to another subscription with the following operation:</span></span>

  ```HTTP
  POST https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.ClassicCompute/moveSubscriptionResources?api-version=2016-04-01
  ```

    <span data-ttu-id="0690c-287">I frågans brödtext är:</span><span class="sxs-lookup"><span data-stu-id="0690c-287">In the request body, include:</span></span>

  ```json
  {
    "target": "/subscriptions/{target-subscription-id}"
  }
  ```

<span data-ttu-id="0690c-288">Åtgärden kan ta flera minuter.</span><span class="sxs-lookup"><span data-stu-id="0690c-288">The operation may run for several minutes.</span></span>

## <a name="use-portal"></a><span data-ttu-id="0690c-289">Använda portalen</span><span class="sxs-lookup"><span data-stu-id="0690c-289">Use portal</span></span>
<span data-ttu-id="0690c-290">Välj den resursgrupp som innehåller resurserna för att flytta resurser och välj sedan den **flytta** knappen.</span><span class="sxs-lookup"><span data-stu-id="0690c-290">To move resources, select the resource group containing those resources, and then select the **Move** button.</span></span>

![Flytta resurser](./media/resource-group-move-resources/select-move.png)

<span data-ttu-id="0690c-292">Välj om du flyttar resurser till en ny resursgrupp eller en ny prenumeration.</span><span class="sxs-lookup"><span data-stu-id="0690c-292">Select whether you are moving the resources to a new resource group or a new subscription.</span></span>

<span data-ttu-id="0690c-293">Välj att flytta resurserna och resursgruppen mål.</span><span class="sxs-lookup"><span data-stu-id="0690c-293">Select the resources to move and the destination resource group.</span></span> <span data-ttu-id="0690c-294">Bekräfta att du behöver uppdatera skript för dessa resurser och välj **OK**.</span><span class="sxs-lookup"><span data-stu-id="0690c-294">Acknowledge that you need to update scripts for these resources and select **OK**.</span></span> <span data-ttu-id="0690c-295">Om du valde ikonen Redigera prenumeration i föregående steg, måste du också välja målprenumerationen.</span><span class="sxs-lookup"><span data-stu-id="0690c-295">If you selected the edit subscription icon in the previous step, you must also select the destination subscription.</span></span>

![Välj mål](./media/resource-group-move-resources/select-destination.png)

<span data-ttu-id="0690c-297">I **meddelanden**, du ser att flyttåtgärden körs.</span><span class="sxs-lookup"><span data-stu-id="0690c-297">In **Notifications**, you see that the move operation is running.</span></span>

![Visa flytta status](./media/resource-group-move-resources/show-status.png)

<span data-ttu-id="0690c-299">När det har slutförts, meddelas du om resultatet.</span><span class="sxs-lookup"><span data-stu-id="0690c-299">When it has completed, you are notified of the result.</span></span>

![Visa flytta resultat](./media/resource-group-move-resources/show-result.png)

## <a name="use-powershell"></a><span data-ttu-id="0690c-301">Använd PowerShell</span><span class="sxs-lookup"><span data-stu-id="0690c-301">Use PowerShell</span></span>
<span data-ttu-id="0690c-302">Flytta befintliga resurser till en annan resursgrupp eller prenumeration genom att använda den `Move-AzureRmResource` kommando.</span><span class="sxs-lookup"><span data-stu-id="0690c-302">To move existing resources to another resource group or subscription, use the `Move-AzureRmResource` command.</span></span>

<span data-ttu-id="0690c-303">Det första exemplet visar hur du flyttar en resurs till en ny resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="0690c-303">The first example shows how to move one resource to a new resource group.</span></span>

```powershell
$resource = Get-AzureRmResource -ResourceName ExampleApp -ResourceGroupName OldRG
Move-AzureRmResource -DestinationResourceGroupName NewRG -ResourceId $resource.ResourceId
```

<span data-ttu-id="0690c-304">Det andra exemplet visar hur du flyttar flera resurser till en ny resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="0690c-304">The second example shows how to move multiple resources to a new resource group.</span></span>

```powershell
$webapp = Get-AzureRmResource -ResourceGroupName OldRG -ResourceName ExampleSite
$plan = Get-AzureRmResource -ResourceGroupName OldRG -ResourceName ExamplePlan
Move-AzureRmResource -DestinationResourceGroupName NewRG -ResourceId $webapp.ResourceId, $plan.ResourceId
```

<span data-ttu-id="0690c-305">Om du vill flytta till en ny prenumeration, innehåller ett värde för den `DestinationSubscriptionId` parameter.</span><span class="sxs-lookup"><span data-stu-id="0690c-305">To move to a new subscription, include a value for the `DestinationSubscriptionId` parameter.</span></span>

<span data-ttu-id="0690c-306">Du uppmanas att bekräfta att du vill flytta de angivna resurserna.</span><span class="sxs-lookup"><span data-stu-id="0690c-306">You are asked to confirm that you want to move the specified resources.</span></span>

```powershell
Confirm
Are you sure you want to move these resources to the resource group
'/subscriptions/{guid}/resourceGroups/newRG' the resources:

/subscriptions/{guid}/resourceGroups/destinationgroup/providers/Microsoft.Web/serverFarms/exampleplan
/subscriptions/{guid}/resourceGroups/destinationgroup/providers/Microsoft.Web/sites/examplesite
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): y
```

## <a name="use-azure-cli-20"></a><span data-ttu-id="0690c-307">Använda Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="0690c-307">Use Azure CLI 2.0</span></span>
<span data-ttu-id="0690c-308">Flytta befintliga resurser till en annan resursgrupp eller prenumeration genom att använda den `az resource move` kommando.</span><span class="sxs-lookup"><span data-stu-id="0690c-308">To move existing resources to another resource group or subscription, use the `az resource move` command.</span></span> <span data-ttu-id="0690c-309">Ange resurs-ID för resurserna för att flytta.</span><span class="sxs-lookup"><span data-stu-id="0690c-309">Provide the resource IDs of the resources to move.</span></span> <span data-ttu-id="0690c-310">Du kan få resurs-ID med följande kommando:</span><span class="sxs-lookup"><span data-stu-id="0690c-310">You can get resource IDs with the following command:</span></span>

```azurecli
az resource show -g sourceGroup -n storagedemo --resource-type "Microsoft.Storage/storageAccounts" --query id
```

<span data-ttu-id="0690c-311">I följande exempel visas hur du flyttar ett storage-konto till en ny resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="0690c-311">The following example shows how to move a storage account to a new resource group.</span></span> <span data-ttu-id="0690c-312">I den `--ids` parameter, ange en blankstegsavgränsad lista över resurs-ID att flytta.</span><span class="sxs-lookup"><span data-stu-id="0690c-312">In the `--ids` parameter, provide a space-separated list of the resource IDs to move.</span></span>

```azurecli
az resource move --destination-group newgroup --ids "/subscriptions/{guid}/resourceGroups/sourceGroup/providers/Microsoft.Storage/storageAccounts/storagedemo"
```

<span data-ttu-id="0690c-313">Om du vill flytta till en ny prenumeration, ange den `--destination-subscription-id` parameter.</span><span class="sxs-lookup"><span data-stu-id="0690c-313">To move to a new subscription, provide the `--destination-subscription-id` parameter.</span></span>

## <a name="use-azure-cli-10"></a><span data-ttu-id="0690c-314">Använda Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="0690c-314">Use Azure CLI 1.0</span></span>
<span data-ttu-id="0690c-315">Flytta befintliga resurser till en annan resursgrupp eller prenumeration genom att använda den `azure resource move` kommando.</span><span class="sxs-lookup"><span data-stu-id="0690c-315">To move existing resources to another resource group or subscription, use the `azure resource move` command.</span></span> <span data-ttu-id="0690c-316">Ange resurs-ID för resurserna för att flytta.</span><span class="sxs-lookup"><span data-stu-id="0690c-316">Provide the resource IDs of the resources to move.</span></span> <span data-ttu-id="0690c-317">Du kan få resurs-ID med följande kommando:</span><span class="sxs-lookup"><span data-stu-id="0690c-317">You can get resource IDs with the following command:</span></span>

```azurecli
azure resource list -g sourceGroup --json
```

<span data-ttu-id="0690c-318">Som returnerar följande format:</span><span class="sxs-lookup"><span data-stu-id="0690c-318">Which returns the following format:</span></span>

```azurecli
[
  {
    "id": "/subscriptions/{guid}/resourceGroups/sourceGroup/providers/Microsoft.Storage/storageAccounts/storagedemo",
    "name": "storagedemo",
    "type": "Microsoft.Storage/storageAccounts",
    "location": "southcentralus",
    "tags": {},
    "kind": "Storage",
    "sku": {
      "name": "Standard_RAGRS",
      "tier": "Standard"
    }
  }
]
```

<span data-ttu-id="0690c-319">I följande exempel visas hur du flyttar ett storage-konto till en ny resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="0690c-319">The following example shows how to move a storage account to a new resource group.</span></span> <span data-ttu-id="0690c-320">I den `-i` parameter, ange en kommaavgränsad lista över resurs-ID att flytta.</span><span class="sxs-lookup"><span data-stu-id="0690c-320">In the `-i` parameter, provide a comma-separated list of the resource IDs to move.</span></span>

```azurecli
azure resource move -i "/subscriptions/{guid}/resourceGroups/sourceGroup/providers/Microsoft.Storage/storageAccounts/storagedemo" -d "destinationGroup"
```

<span data-ttu-id="0690c-321">Du uppmanas att bekräfta att du vill flytta den angivna resursen.</span><span class="sxs-lookup"><span data-stu-id="0690c-321">You are asked to confirm that you want to move the specified resource.</span></span>

## <a name="use-rest-api"></a><span data-ttu-id="0690c-322">Använd REST-API</span><span class="sxs-lookup"><span data-stu-id="0690c-322">Use REST API</span></span>
<span data-ttu-id="0690c-323">För att flytta befintliga resurser till en annan resursgrupp eller prenumeration, kör du:</span><span class="sxs-lookup"><span data-stu-id="0690c-323">To move existing resources to another resource group or subscription, run:</span></span>

```HTTP
POST https://management.azure.com/subscriptions/{source-subscription-id}/resourcegroups/{source-resource-group-name}/moveResources?api-version={api-version}
```

<span data-ttu-id="0690c-324">I begärandetexten anger du målresursgruppen och resurser för att flytta.</span><span class="sxs-lookup"><span data-stu-id="0690c-324">In the request body, you specify the target resource group and the resources to move.</span></span> <span data-ttu-id="0690c-325">Mer information om REST-flyttåtgärden finns [flyttar resurser](https://msdn.microsoft.com/library/azure/mt218710.aspx).</span><span class="sxs-lookup"><span data-stu-id="0690c-325">For more information about the move REST operation, see [Move resources](https://msdn.microsoft.com/library/azure/mt218710.aspx).</span></span>

## <a name="next-steps"></a><span data-ttu-id="0690c-326">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0690c-326">Next steps</span></span>
* <span data-ttu-id="0690c-327">Mer information om PowerShell-cmdletar för att hantera din prenumeration, se [med hjälp av Azure PowerShell med Resource Manager](powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="0690c-327">To learn about PowerShell cmdlets for managing your subscription, see [Using Azure PowerShell with Resource Manager](powershell-azure-resource-manager.md).</span></span>
* <span data-ttu-id="0690c-328">Mer information om Azure CLI-kommandon för att hantera din prenumeration, se [med hjälp av Azure CLI med Resource Manager](xplat-cli-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="0690c-328">To learn about Azure CLI commands for managing your subscription, see [Using the Azure CLI with Resource Manager](xplat-cli-azure-resource-manager.md).</span></span>
* <span data-ttu-id="0690c-329">Mer information om Företagsportalen funktioner för att hantera din prenumeration, se [med Azure-portalen för att hantera resurser](resource-group-portal.md).</span><span class="sxs-lookup"><span data-stu-id="0690c-329">To learn about portal features for managing your subscription, see [Using the Azure portal to manage resources](resource-group-portal.md).</span></span>
* <span data-ttu-id="0690c-330">Läs om hur du kopplar logiskt till resurser i [med taggar för att organisera dina resurser](resource-group-using-tags.md).</span><span class="sxs-lookup"><span data-stu-id="0690c-330">To learn about applying a logical organization to your resources, see [Using tags to organize your resources](resource-group-using-tags.md).</span></span>
