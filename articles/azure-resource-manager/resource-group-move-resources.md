---
title: aaaMove Azure-resurser toonew prenumerationen eller resursen grupp | Microsoft Docs
description: "Använd Azure Resource Manager toomove resurser tooa ny resursgrupp eller prenumeration."
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
ms.openlocfilehash: 09d35f0afbbcdc0c66779f98a982d878f0807497
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="move-resources-toonew-resource-group-or-subscription"></a><span data-ttu-id="3da4b-103">Flytta resurser toonew resursgrupp eller prenumeration</span><span class="sxs-lookup"><span data-stu-id="3da4b-103">Move resources toonew resource group or subscription</span></span>
<span data-ttu-id="3da4b-104">Det här avsnittet visar hur toomove resurser tooeither en ny prenumeration eller en ny resurs gruppera i hello samma prenumeration.</span><span class="sxs-lookup"><span data-stu-id="3da4b-104">This topic shows you how toomove resources tooeither a new subscription or a new resource group in hello same subscription.</span></span> <span data-ttu-id="3da4b-105">Du kan använda hello portal, PowerShell, Azure CLI eller hello REST API toomove resurs.</span><span class="sxs-lookup"><span data-stu-id="3da4b-105">You can use hello portal, PowerShell, Azure CLI, or hello REST API toomove resource.</span></span> <span data-ttu-id="3da4b-106">hello flytta åtgärder i det här avsnittet är tillgängliga tooyou utan hjälp från Azure-supporten.</span><span class="sxs-lookup"><span data-stu-id="3da4b-106">hello move operations in this topic are available tooyou without any assistance from Azure support.</span></span>

<span data-ttu-id="3da4b-107">När du flyttar resurser, låst både hello källgrupp och hello målgruppen under hello igen.</span><span class="sxs-lookup"><span data-stu-id="3da4b-107">When moving resources, both hello source group and hello target group are locked during hello operation.</span></span> <span data-ttu-id="3da4b-108">Skriva och ta bort blockeras på hello resursgrupper tills hello flytta har slutförts.</span><span class="sxs-lookup"><span data-stu-id="3da4b-108">Write and delete operations are blocked on hello resource groups until hello move completes.</span></span> <span data-ttu-id="3da4b-109">Låset innebär det går inte att lägga till, uppdatera eller ta bort hello resursgrupper, men innebär inte hello resurser är låsta.</span><span class="sxs-lookup"><span data-stu-id="3da4b-109">This lock means you cannot add, update, or delete resources in hello resource groups, but it does not mean hello resources are frozen.</span></span> <span data-ttu-id="3da4b-110">Om du flyttar en SQL Server och dess databas tooa ny resursgrupp, påträffar ett program som använder hello databas utan avbrott.</span><span class="sxs-lookup"><span data-stu-id="3da4b-110">For example, if you move a SQL Server and its database tooa new resource group, an application that uses hello database experiences no downtime.</span></span> <span data-ttu-id="3da4b-111">Det kan fortfarande läsa och skriva toohello databas.</span><span class="sxs-lookup"><span data-stu-id="3da4b-111">It can still read and write toohello database.</span></span>

<span data-ttu-id="3da4b-112">Du kan inte ändra hello platsen för hello resurs.</span><span class="sxs-lookup"><span data-stu-id="3da4b-112">You cannot change hello location of hello resource.</span></span> <span data-ttu-id="3da4b-113">Om du flyttar en resurs flyttas det endast tooa ny resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="3da4b-113">Moving a resource only moves it tooa new resource group.</span></span> <span data-ttu-id="3da4b-114">hello ny resursgrupp kan ha en annan plats, men som ändras inte hello platsen för hello resurs.</span><span class="sxs-lookup"><span data-stu-id="3da4b-114">hello new resource group may have a different location, but that does not change hello location of hello resource.</span></span>

> [!NOTE]
> <span data-ttu-id="3da4b-115">Den här artikeln beskriver hur toomove resurser i en befintlig Azure-konto erbjudanden.</span><span class="sxs-lookup"><span data-stu-id="3da4b-115">This article describes how toomove resources within an existing Azure account offering.</span></span> <span data-ttu-id="3da4b-116">Om du verkligen vill toochange ditt Azure-konto erbjudande (till exempel uppgraderar från betalning per användning toopre-pay) medan toowork med din befintliga resurser, se [växla Azure-prenumeration tooanother erbjudandet](../billing/billing-how-to-switch-azure-offer.md).</span><span class="sxs-lookup"><span data-stu-id="3da4b-116">If you actually want toochange your Azure account offering (such as upgrading from pay-as-you-go toopre-pay) while continuing toowork with your existing resources, see [Switch your Azure subscription tooanother offer](../billing/billing-how-to-switch-azure-offer.md).</span></span>
>
>

## <a name="checklist-before-moving-resources"></a><span data-ttu-id="3da4b-117">Checklista innan du flyttar resurser</span><span class="sxs-lookup"><span data-stu-id="3da4b-117">Checklist before moving resources</span></span>
<span data-ttu-id="3da4b-118">Det finns vissa viktiga steg tooperform innan du flyttar en resurs.</span><span class="sxs-lookup"><span data-stu-id="3da4b-118">There are some important steps tooperform before moving a resource.</span></span> <span data-ttu-id="3da4b-119">Du kan undvika fel genom att verifiera dessa villkor.</span><span class="sxs-lookup"><span data-stu-id="3da4b-119">By verifying these conditions, you can avoid errors.</span></span>

1. <span data-ttu-id="3da4b-120">hello käll- och prenumerationer måste finnas inom hello samma [Azure Active Directory-klient](../active-directory/active-directory-howto-tenant.md).</span><span class="sxs-lookup"><span data-stu-id="3da4b-120">hello source and destination subscriptions must exist within hello same [Azure Active Directory tenant](../active-directory/active-directory-howto-tenant.md).</span></span> <span data-ttu-id="3da4b-121">toocheck att båda prenumerationer har hello samma klient-ID, använda Azure PowerShell eller Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="3da4b-121">toocheck that both subscriptions have hello same tenant ID, use Azure PowerShell or Azure CLI.</span></span>

  <span data-ttu-id="3da4b-122">Använd för Azure PowerShell:</span><span class="sxs-lookup"><span data-stu-id="3da4b-122">For Azure PowerShell, use:</span></span>

  ```powershell
  (Get-AzureRmSubscription -SubscriptionName "Example Subscription").TenantId
  ```

  <span data-ttu-id="3da4b-123">Använd för Azure CLI 2.0:</span><span class="sxs-lookup"><span data-stu-id="3da4b-123">For Azure CLI 2.0, use:</span></span>

  ```azurecli
  az account show --subscription "Example Subscription" --query tenantId
  ```

  <span data-ttu-id="3da4b-124">Om hello klient-ID för är hello käll- och prenumerationer inte hello samma försök toochange hello katalogen för hello-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="3da4b-124">If hello tenant IDs for hello source and destination subscriptions are not hello same, you can attempt toochange hello directory for hello subscription.</span></span> <span data-ttu-id="3da4b-125">Det här alternativet är dock endast tillgängliga tooService administratörer som har loggat in med ett Microsoft-konto (inte ett organisationskonto).</span><span class="sxs-lookup"><span data-stu-id="3da4b-125">However, this option is only available tooService Administrators who are signed in with a Microsoft account (not an organizational account).</span></span> <span data-ttu-id="3da4b-126">Ändra hello directory, logga in toohello tooattempt [klassiska portalen](https://manage.windowsazure.com/), och välj **inställningar**, och välj hello-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="3da4b-126">tooattempt changing hello directory, log in toohello [classic portal](https://manage.windowsazure.com/), and select **Settings**, and select hello subscription.</span></span> <span data-ttu-id="3da4b-127">Om hello **redigera katalog** ikonen är tillgänglig, markerar du den toochange hello som är associerade med Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="3da4b-127">If hello **Edit Directory** icon is available, select it toochange hello associated Azure Active Directory.</span></span>

  ![Redigera katalog](./media/resource-group-move-resources/edit-directory.png)

  <span data-ttu-id="3da4b-129">Om ikonen inte är tillgängligt måste du kontakta supporten toomove hello resurser tooa nya innehavaren.</span><span class="sxs-lookup"><span data-stu-id="3da4b-129">If that icon is not available, you must contact support toomove hello resources tooa new tenant.</span></span>

2. <span data-ttu-id="3da4b-130">hello-tjänsten måste aktivera hello möjlighet toomove resurser.</span><span class="sxs-lookup"><span data-stu-id="3da4b-130">hello service must enable hello ability toomove resources.</span></span> <span data-ttu-id="3da4b-131">Det här avsnittet visas vilka tjänster kan flytta resurser och tjänster som gör inte flytta resurser.</span><span class="sxs-lookup"><span data-stu-id="3da4b-131">This topic lists which services enable moving resources and which services do not enable moving resources.</span></span>
3. <span data-ttu-id="3da4b-132">Hej målprenumerationen måste registreras för hello resursprovidern hello resurs som flyttas.</span><span class="sxs-lookup"><span data-stu-id="3da4b-132">hello destination subscription must be registered for hello resource provider of hello resource being moved.</span></span> <span data-ttu-id="3da4b-133">Om inte, du får ett felmeddelande om att hello **prenumerationen har inte registrerats för en resurstyp**.</span><span class="sxs-lookup"><span data-stu-id="3da4b-133">If not, you receive an error stating that hello **subscription is not registered for a resource type**.</span></span> <span data-ttu-id="3da4b-134">Det här problemet kan uppstå när du flyttar en resurs tooa ny prenumeration, men den prenumerationen aldrig har använts med den resurstypen.</span><span class="sxs-lookup"><span data-stu-id="3da4b-134">You might encounter this problem when moving a resource tooa new subscription, but that subscription has never been used with that resource type.</span></span> <span data-ttu-id="3da4b-135">toolearn hur toocheck hello registreringsstatus och registrera resursprovidrar finns [resursproviders och typer](resource-manager-supported-services.md).</span><span class="sxs-lookup"><span data-stu-id="3da4b-135">toolearn how toocheck hello registration status and register resource providers, see [Resource providers and types](resource-manager-supported-services.md).</span></span>

## <a name="when-toocall-support"></a><span data-ttu-id="3da4b-136">När toocall stöd</span><span class="sxs-lookup"><span data-stu-id="3da4b-136">When toocall support</span></span>
<span data-ttu-id="3da4b-137">Du kan flytta de flesta resurser via hello självbetjäning åtgärder visas i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="3da4b-137">You can move most resources through hello self-service operations shown in this topic.</span></span> <span data-ttu-id="3da4b-138">Använd hello självbetjäning åtgärder:</span><span class="sxs-lookup"><span data-stu-id="3da4b-138">Use hello self-service operations to:</span></span>

* <span data-ttu-id="3da4b-139">Flytta resurshanterarens resurser.</span><span class="sxs-lookup"><span data-stu-id="3da4b-139">Move Resource Manager resources.</span></span>
* <span data-ttu-id="3da4b-140">Flytta klassiska resurser enligt toohello [klassisk distribution begränsningar](#classic-deployment-limitations).</span><span class="sxs-lookup"><span data-stu-id="3da4b-140">Move classic resources according toohello [classic deployment limitations](#classic-deployment-limitations).</span></span>

<span data-ttu-id="3da4b-141">Ring supporten när du behöver:</span><span class="sxs-lookup"><span data-stu-id="3da4b-141">Call support when you need to:</span></span>

* <span data-ttu-id="3da4b-142">Flytta resurser tooa nya Azure-konto (och Azure Active Directory-klient).</span><span class="sxs-lookup"><span data-stu-id="3da4b-142">Move your resources tooa new Azure account (and Azure Active Directory tenant).</span></span>
* <span data-ttu-id="3da4b-143">Flytta klassiska resurser men har problem med hello begränsningar.</span><span class="sxs-lookup"><span data-stu-id="3da4b-143">Move classic resources but are having trouble with hello limitations.</span></span>

## <a name="services-that-enable-move"></a><span data-ttu-id="3da4b-144">Tjänster som gör att flytta</span><span class="sxs-lookup"><span data-stu-id="3da4b-144">Services that enable move</span></span>
<span data-ttu-id="3da4b-145">Hello-tjänster som möjliggör glidande tooboth en ny resursgrupp och prenumerationen är för tillfället:</span><span class="sxs-lookup"><span data-stu-id="3da4b-145">For now, hello services that enable moving tooboth a new resource group and subscription are:</span></span>

* <span data-ttu-id="3da4b-146">API Management</span><span class="sxs-lookup"><span data-stu-id="3da4b-146">API Management</span></span>
* <span data-ttu-id="3da4b-147">Apptjänst-appar (webbprogram) - finns [Apptjänst begränsningar](#app-service-limitations)</span><span class="sxs-lookup"><span data-stu-id="3da4b-147">App Service apps (web apps) - see [App Service limitations](#app-service-limitations)</span></span>
* <span data-ttu-id="3da4b-148">Application Insights</span><span class="sxs-lookup"><span data-stu-id="3da4b-148">Application Insights</span></span>
* <span data-ttu-id="3da4b-149">Automation</span><span class="sxs-lookup"><span data-stu-id="3da4b-149">Automation</span></span>
* <span data-ttu-id="3da4b-150">Batch</span><span class="sxs-lookup"><span data-stu-id="3da4b-150">Batch</span></span>
* <span data-ttu-id="3da4b-151">Bing Maps</span><span class="sxs-lookup"><span data-stu-id="3da4b-151">Bing Maps</span></span>
* <span data-ttu-id="3da4b-152">CDN</span><span class="sxs-lookup"><span data-stu-id="3da4b-152">CDN</span></span>
* <span data-ttu-id="3da4b-153">Molntjänster - Se [klassisk distribution begränsningar](#classic-deployment-limitations)</span><span class="sxs-lookup"><span data-stu-id="3da4b-153">Cloud Services - see [Classic deployment limitations](#classic-deployment-limitations)</span></span>
* <span data-ttu-id="3da4b-154">Cognitive Services</span><span class="sxs-lookup"><span data-stu-id="3da4b-154">Cognitive Services</span></span>
* <span data-ttu-id="3da4b-155">Content Moderator</span><span class="sxs-lookup"><span data-stu-id="3da4b-155">Content Moderator</span></span>
* <span data-ttu-id="3da4b-156">Data Catalog</span><span class="sxs-lookup"><span data-stu-id="3da4b-156">Data Catalog</span></span>
* <span data-ttu-id="3da4b-157">Data Factory</span><span class="sxs-lookup"><span data-stu-id="3da4b-157">Data Factory</span></span>
* <span data-ttu-id="3da4b-158">Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="3da4b-158">Data Lake Analytics</span></span>
* <span data-ttu-id="3da4b-159">Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="3da4b-159">Data Lake Store</span></span>
* <span data-ttu-id="3da4b-160">DNS</span><span class="sxs-lookup"><span data-stu-id="3da4b-160">DNS</span></span>
* <span data-ttu-id="3da4b-161">Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="3da4b-161">Azure Cosmos DB</span></span>
* <span data-ttu-id="3da4b-162">Händelsehubbar</span><span class="sxs-lookup"><span data-stu-id="3da4b-162">Event Hubs</span></span>
* <span data-ttu-id="3da4b-163">HDInsight-kluster - finns [HDInsight begränsningar](#hdinsight-limitations)</span><span class="sxs-lookup"><span data-stu-id="3da4b-163">HDInsight clusters - see [HDInsight limitations](#hdinsight-limitations)</span></span>
* <span data-ttu-id="3da4b-164">IoT-hubbar</span><span class="sxs-lookup"><span data-stu-id="3da4b-164">IoT Hubs</span></span>
* <span data-ttu-id="3da4b-165">Key Vault</span><span class="sxs-lookup"><span data-stu-id="3da4b-165">Key Vault</span></span>
* <span data-ttu-id="3da4b-166">Belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="3da4b-166">Load Balancers</span></span>
* <span data-ttu-id="3da4b-167">Logic Apps</span><span class="sxs-lookup"><span data-stu-id="3da4b-167">Logic Apps</span></span>
* <span data-ttu-id="3da4b-168">Machine Learning</span><span class="sxs-lookup"><span data-stu-id="3da4b-168">Machine Learning</span></span>
* <span data-ttu-id="3da4b-169">Media Services</span><span class="sxs-lookup"><span data-stu-id="3da4b-169">Media Services</span></span>
* <span data-ttu-id="3da4b-170">Mobile Engagement</span><span class="sxs-lookup"><span data-stu-id="3da4b-170">Mobile Engagement</span></span>
* <span data-ttu-id="3da4b-171">Notification Hubs</span><span class="sxs-lookup"><span data-stu-id="3da4b-171">Notification Hubs</span></span>
* <span data-ttu-id="3da4b-172">Åtgärdsinformation</span><span class="sxs-lookup"><span data-stu-id="3da4b-172">Operational Insights</span></span>
* <span data-ttu-id="3da4b-173">Operations Management</span><span class="sxs-lookup"><span data-stu-id="3da4b-173">Operations Management</span></span>
* <span data-ttu-id="3da4b-174">Power BI</span><span class="sxs-lookup"><span data-stu-id="3da4b-174">Power BI</span></span>
* <span data-ttu-id="3da4b-175">Redis Cache</span><span class="sxs-lookup"><span data-stu-id="3da4b-175">Redis Cache</span></span>
* <span data-ttu-id="3da4b-176">Scheduler</span><span class="sxs-lookup"><span data-stu-id="3da4b-176">Scheduler</span></span>
* <span data-ttu-id="3da4b-177">Söka</span><span class="sxs-lookup"><span data-stu-id="3da4b-177">Search</span></span>
* <span data-ttu-id="3da4b-178">Serverhantering</span><span class="sxs-lookup"><span data-stu-id="3da4b-178">Server Management</span></span>
* <span data-ttu-id="3da4b-179">Service Bus</span><span class="sxs-lookup"><span data-stu-id="3da4b-179">Service Bus</span></span>
* <span data-ttu-id="3da4b-180">Service Fabric</span><span class="sxs-lookup"><span data-stu-id="3da4b-180">Service Fabric</span></span>
* <span data-ttu-id="3da4b-181">Lagring</span><span class="sxs-lookup"><span data-stu-id="3da4b-181">Storage</span></span>
* <span data-ttu-id="3da4b-182">Storage (klassisk) - finns [klassisk distribution begränsningar](#classic-deployment-limitations)</span><span class="sxs-lookup"><span data-stu-id="3da4b-182">Storage (classic) - see [Classic deployment limitations](#classic-deployment-limitations)</span></span>
* <span data-ttu-id="3da4b-183">Stream Analytics - Stream Analytics-jobb inte kan flyttas när du kör i läget.</span><span class="sxs-lookup"><span data-stu-id="3da4b-183">Stream Analytics - Stream Analytics jobs cannot be moved when in running state.</span></span>
* <span data-ttu-id="3da4b-184">SQL-databasservern - hello databasen och servern måste finnas i hello samma resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="3da4b-184">SQL Database server - hello database and server must reside in hello same resource group.</span></span> <span data-ttu-id="3da4b-185">När du flyttar en SQLServer, flyttas även alla databaser.</span><span class="sxs-lookup"><span data-stu-id="3da4b-185">When you move a SQL server, all its databases are also moved.</span></span>
* <span data-ttu-id="3da4b-186">Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="3da4b-186">Traffic Manager</span></span>
* <span data-ttu-id="3da4b-187">Virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="3da4b-187">Virtual Machines</span></span>
* <span data-ttu-id="3da4b-188">Virtuella datorer med certifikat som lagras i Key Vault - flytta toonew resursgrupp i samma prenumeration har aktiverats men flytta mellan prenumeration har inte aktiverats.</span><span class="sxs-lookup"><span data-stu-id="3da4b-188">Virtual Machines with certificate stored in Key Vault - Move toonew resource group in same subscription is enabled, but cross subscription move is not enabled.</span></span>
* <span data-ttu-id="3da4b-189">Virtuella datorer (klassisk) - finns [klassisk distribution begränsningar](#classic-deployment-limitations)</span><span class="sxs-lookup"><span data-stu-id="3da4b-189">Virtual Machines (classic) - see [Classic deployment limitations](#classic-deployment-limitations)</span></span>
* <span data-ttu-id="3da4b-190">Skalningsuppsättningar för Virtual Machines</span><span class="sxs-lookup"><span data-stu-id="3da4b-190">Virtual Machine Scale Sets</span></span>
* <span data-ttu-id="3da4b-191">Virtuella nätverk - just nu, ett peered virtuellt nätverk går inte att flytta tills VNet-peering har inaktiverats.</span><span class="sxs-lookup"><span data-stu-id="3da4b-191">Virtual Networks - Currently, a peered Virtual Network cannot be moved until VNet peering has been disabled.</span></span> <span data-ttu-id="3da4b-192">När inaktiverad hello virtuellt nätverk kan flyttas utan problem och hello VNet-peering kan aktiveras.</span><span class="sxs-lookup"><span data-stu-id="3da4b-192">Once disabled, hello Virtual Network can be moved successfully and hello VNet peering can be enabled.</span></span> <span data-ttu-id="3da4b-193">Dessutom får inte ett virtuellt nätverk vara flyttade tooa annan prenumeration om hello virtuellt nätverk innehåller inget undernät med resursnavigeringslänkar.</span><span class="sxs-lookup"><span data-stu-id="3da4b-193">In addition, a Virtual Network cannot be moved tooa different subscription if hello Virtual Network contains any subnet with resource navigation links.</span></span> <span data-ttu-id="3da4b-194">Ett undernät för virtuellt nätverk har till exempel en resurslänk för navigering när en resurs för Microsoft.Cache redis har distribuerats i det här undernätet.</span><span class="sxs-lookup"><span data-stu-id="3da4b-194">For example, a Virtual Network subnet has a resource navigation link when a Microsoft.Cache redis resource is deployed into this subnet.</span></span>
* <span data-ttu-id="3da4b-195">VPN-gateway</span><span class="sxs-lookup"><span data-stu-id="3da4b-195">VPN Gateway</span></span>


## <a name="services-that-do-not-enable-move"></a><span data-ttu-id="3da4b-196">Tjänster som inte gör flytta</span><span class="sxs-lookup"><span data-stu-id="3da4b-196">Services that do not enable move</span></span>
<span data-ttu-id="3da4b-197">hello-tjänster som för närvarande inte aktiverar flytta en resurs är:</span><span class="sxs-lookup"><span data-stu-id="3da4b-197">hello services that currently do not enable moving a resource are:</span></span>

* <span data-ttu-id="3da4b-198">AD DS</span><span class="sxs-lookup"><span data-stu-id="3da4b-198">AD Domain Services</span></span>
* <span data-ttu-id="3da4b-199">AD-Hybrid-tjänsten för hälsotillstånd</span><span class="sxs-lookup"><span data-stu-id="3da4b-199">AD Hybrid Health Service</span></span>
* <span data-ttu-id="3da4b-200">Application Gateway</span><span class="sxs-lookup"><span data-stu-id="3da4b-200">Application Gateway</span></span>
* <span data-ttu-id="3da4b-201">Tillgänglighetsuppsättningar med virtuella datorer med hanterade diskar</span><span class="sxs-lookup"><span data-stu-id="3da4b-201">Availability sets with Virtual Machines with Managed Disks</span></span>
* <span data-ttu-id="3da4b-202">BizTalk Services</span><span class="sxs-lookup"><span data-stu-id="3da4b-202">BizTalk Services</span></span>
* <span data-ttu-id="3da4b-203">Container Service</span><span class="sxs-lookup"><span data-stu-id="3da4b-203">Container Service</span></span>
* <span data-ttu-id="3da4b-204">Express Route</span><span class="sxs-lookup"><span data-stu-id="3da4b-204">Express Route</span></span>
* <span data-ttu-id="3da4b-205">DevTest Labs - flytta toonew resursgrupp i samma prenumeration har aktiverats men mellan flytta för prenumerationen har inte aktiverats.</span><span class="sxs-lookup"><span data-stu-id="3da4b-205">DevTest Labs - Move toonew resource group in same subscription is enabled, but cross subscription move is not enabled.</span></span>
* <span data-ttu-id="3da4b-206">Dynamics LCS</span><span class="sxs-lookup"><span data-stu-id="3da4b-206">Dynamics LCS</span></span>
* <span data-ttu-id="3da4b-207">Bilder som har skapats från hanterade diskar</span><span class="sxs-lookup"><span data-stu-id="3da4b-207">Images created from Managed Disks</span></span>
* <span data-ttu-id="3da4b-208">Managed Disks</span><span class="sxs-lookup"><span data-stu-id="3da4b-208">Managed Disks</span></span>
* <span data-ttu-id="3da4b-209">Hanterade program</span><span class="sxs-lookup"><span data-stu-id="3da4b-209">Managed Applications</span></span>
* <span data-ttu-id="3da4b-210">Recovery Services-valvet – även hello återställningstjänster av inte flytta hello beräknings-, nätverks- och resurser som är associerade med valvet, se [återställningstjänster begränsningar](#recovery-services-limitations).</span><span class="sxs-lookup"><span data-stu-id="3da4b-210">Recovery Services vault - also do not move hello Compute, Network, and Storage resources associated with hello Recovery Services vault, see [Recovery Services limitations](#recovery-services-limitations).</span></span>
* <span data-ttu-id="3da4b-211">Säkerhet</span><span class="sxs-lookup"><span data-stu-id="3da4b-211">Security</span></span>
* <span data-ttu-id="3da4b-212">Ögonblicksbilder som skapats från hanterade diskar</span><span class="sxs-lookup"><span data-stu-id="3da4b-212">Snapshots created from Managed Disks</span></span>
* <span data-ttu-id="3da4b-213">StorSimple Enhetshanteraren</span><span class="sxs-lookup"><span data-stu-id="3da4b-213">StorSimple Device Manager</span></span>
* <span data-ttu-id="3da4b-214">Virtuella datorer med hanterade diskar</span><span class="sxs-lookup"><span data-stu-id="3da4b-214">Virtual Machines with Managed Disks</span></span>
* <span data-ttu-id="3da4b-215">Virtuella nätverk (klassiskt) - finns [klassisk distribution begränsningar](#classic-deployment-limitations)</span><span class="sxs-lookup"><span data-stu-id="3da4b-215">Virtual Networks (classic) - see [Classic deployment limitations](#classic-deployment-limitations)</span></span>
* <span data-ttu-id="3da4b-216">Virtuella datorer som skapats från Marketplace resurser - kan inte flyttas mellan prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="3da4b-216">Virtual Machines created from Marketplace resources - cannot be moved across subscriptions.</span></span> <span data-ttu-id="3da4b-217">Resursen behöver toobe avetableras i hello aktuell prenumeration och distribueras igen i hello ny prenumeration</span><span class="sxs-lookup"><span data-stu-id="3da4b-217">Resource needs toobe deprovisioned in hello current subscription and deployed again in hello new subscription</span></span>

## <a name="app-service-limitations"></a><span data-ttu-id="3da4b-218">Begränsningar för App Service</span><span class="sxs-lookup"><span data-stu-id="3da4b-218">App Service limitations</span></span>
<span data-ttu-id="3da4b-219">När du arbetar med Apptjänst-appar kan flytta du inte endast en App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="3da4b-219">When working with App Service apps, you cannot move only an App Service plan.</span></span> <span data-ttu-id="3da4b-220">toomove Apptjänst-appar, alternativen är:</span><span class="sxs-lookup"><span data-stu-id="3da4b-220">toomove App Service apps, your options are:</span></span>

* <span data-ttu-id="3da4b-221">Flytta hello App Service-plan och alla andra resurser i Apptjänst i som gruppen tooa ny resurs resursgruppen som inte redan har App Service-resurser.</span><span class="sxs-lookup"><span data-stu-id="3da4b-221">Move hello App Service plan and all other App Service resources in that resource group tooa new resource group that does not already have App Service resources.</span></span> <span data-ttu-id="3da4b-222">Det här kravet innebär måste du flytta även hello Apptjänst resurser som inte är associerad med hello App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="3da4b-222">This requirement means you must move even hello App Service resources that are not associated with hello App Service plan.</span></span>
* <span data-ttu-id="3da4b-223">Flytta hello appar tooa annan resursgrupp, men behålla alla programtjänstplaner i hello ursprungliga resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="3da4b-223">Move hello apps tooa different resource group, but keep all App Service plans in hello original resource group.</span></span>

<span data-ttu-id="3da4b-224">hello hello App Service-plan inte behöver tooreside i samma resursgrupp som hello app för hello app toofunction korrekt.</span><span class="sxs-lookup"><span data-stu-id="3da4b-224">hello App Service plan does not need tooreside in hello same resource group as hello app for hello app toofunction correctly.</span></span>

<span data-ttu-id="3da4b-225">Om till exempel din resursgrupp innehåller:</span><span class="sxs-lookup"><span data-stu-id="3da4b-225">For example, if your resource group contains:</span></span>

* <span data-ttu-id="3da4b-226">**Web-a** som är associerad med **planera en**</span><span class="sxs-lookup"><span data-stu-id="3da4b-226">**web-a** which is associated with **plan-a**</span></span>
* <span data-ttu-id="3da4b-227">**Web-b** som är associerad med **plan b**</span><span class="sxs-lookup"><span data-stu-id="3da4b-227">**web-b** which is associated with **plan-b**</span></span>

<span data-ttu-id="3da4b-228">Alternativen är:</span><span class="sxs-lookup"><span data-stu-id="3da4b-228">Your options are:</span></span>

* <span data-ttu-id="3da4b-229">Flytta **web-a**, **planera en**, **web-b**, och **plan b**</span><span class="sxs-lookup"><span data-stu-id="3da4b-229">Move **web-a**, **plan-a**, **web-b**, and **plan-b**</span></span>
* <span data-ttu-id="3da4b-230">Flytta **web-a** och **web-b**</span><span class="sxs-lookup"><span data-stu-id="3da4b-230">Move **web-a** and **web-b**</span></span>
* <span data-ttu-id="3da4b-231">Flytta **web-a**</span><span class="sxs-lookup"><span data-stu-id="3da4b-231">Move **web-a**</span></span>
* <span data-ttu-id="3da4b-232">Flytta **web-b**</span><span class="sxs-lookup"><span data-stu-id="3da4b-232">Move **web-b**</span></span>

<span data-ttu-id="3da4b-233">Alla andra kombinationer involverar lämnar bakom en resurstyp som kan finnas kvar när du flyttar en apptjänstplan (någon typ av App Service-resurs).</span><span class="sxs-lookup"><span data-stu-id="3da4b-233">All other combinations involve leaving behind a resource type that can't be left behind when moving an App Service plan (any type of App Service resource).</span></span>

<span data-ttu-id="3da4b-234">Om ditt webbprogram finns i en annan resursgrupp än dess App Service-plan, men toomove båda tooa ny resursgrupp, måste du flytta hello i två steg.</span><span class="sxs-lookup"><span data-stu-id="3da4b-234">If your web app resides in a different resource group than its App Service plan but you want toomove both tooa new resource group, you must perform hello move in two steps.</span></span> <span data-ttu-id="3da4b-235">Exempel:</span><span class="sxs-lookup"><span data-stu-id="3da4b-235">For example:</span></span>

* <span data-ttu-id="3da4b-236">**Web-a** finns i **web-grupp**</span><span class="sxs-lookup"><span data-stu-id="3da4b-236">**web-a** resides in **web-group**</span></span>
* <span data-ttu-id="3da4b-237">**Planera en** finns i **plan-grupp**</span><span class="sxs-lookup"><span data-stu-id="3da4b-237">**plan-a** resides in **plan-group**</span></span>
* <span data-ttu-id="3da4b-238">Du vill **web-a** och **planera en** tooreside i **kombineras grupp**</span><span class="sxs-lookup"><span data-stu-id="3da4b-238">You want **web-a** and **plan-a** tooreside in **combined-group**</span></span>

<span data-ttu-id="3da4b-239">tooaccomplish detta flytta utföra åtgärder för två separata flytta i hello följande ordning:</span><span class="sxs-lookup"><span data-stu-id="3da4b-239">tooaccomplish this move, perform two separate move operations in hello following sequence:</span></span>

1. <span data-ttu-id="3da4b-240">Flytta hello **web-a** för**plan-grupp**</span><span class="sxs-lookup"><span data-stu-id="3da4b-240">Move hello **web-a** too**plan-group**</span></span>
2. <span data-ttu-id="3da4b-241">Flytta **web-a** och **planera en** för**kombineras grupp**.</span><span class="sxs-lookup"><span data-stu-id="3da4b-241">Move **web-a** and **plan-a** too**combined-group**.</span></span>

<span data-ttu-id="3da4b-242">Du kan flytta en App tjänstcertifikat tooa ny resursgrupp eller prenumeration utan problem.</span><span class="sxs-lookup"><span data-stu-id="3da4b-242">You can move an App Service Certificate tooa new resource group or subscription without any issues.</span></span> <span data-ttu-id="3da4b-243">Om webbappen innehåller ett SSL-certifikat som du köpt externt och överföra toohello app, måste du radera hello certifikat innan glidande hello-webbprogram.</span><span class="sxs-lookup"><span data-stu-id="3da4b-243">However, if your web app includes an SSL certificate that you purchased externally and uploaded toohello app, you must delete hello certificate before moving hello web app.</span></span> <span data-ttu-id="3da4b-244">Du kan exempelvis utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="3da4b-244">For example, you can perform hello following steps:</span></span>

1. <span data-ttu-id="3da4b-245">Ta bort hello upp certifikatet från hello webbapp</span><span class="sxs-lookup"><span data-stu-id="3da4b-245">Delete hello uploaded certificate from hello web app</span></span>
2. <span data-ttu-id="3da4b-246">Flytta hello-webbprogram</span><span class="sxs-lookup"><span data-stu-id="3da4b-246">Move hello web app</span></span>
3. <span data-ttu-id="3da4b-247">Överföra hello certifikat toohello webbapp</span><span class="sxs-lookup"><span data-stu-id="3da4b-247">Upload hello certificate toohello web app</span></span>

## <a name="recovery-services-limitations"></a><span data-ttu-id="3da4b-248">Recovery Services-begränsningar</span><span class="sxs-lookup"><span data-stu-id="3da4b-248">Recovery Services limitations</span></span>
<span data-ttu-id="3da4b-249">Flytta har inte aktiverats för lagring, nätverk, eller beräkningsresurser används tooset in katastrofåterställning med Azure Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="3da4b-249">Move is not enabled for Storage, Network, or Compute resources used tooset up disaster recovery with Azure Site Recovery.</span></span>

<span data-ttu-id="3da4b-250">Till exempel anta att du har ställt in replikering av ditt lokala datorer tooa storage-konto (Storage1) och vill att hello skyddad dator toocome efter redundans tooAzure som en virtuell dator (VM1) kopplad tooa virtuella nätverk (Network1).</span><span class="sxs-lookup"><span data-stu-id="3da4b-250">For example, suppose you have set up replication of your on-premises machines tooa storage account (Storage1) and want hello protected machine toocome up after failover tooAzure as a virtual machine (VM1) attached tooa virtual network (Network1).</span></span> <span data-ttu-id="3da4b-251">Du kan inte flytta någon av dessa Azure-resurser - Storage1, VM1, och Network1 - över resurs grupper inom hello samma prenumeration eller mellan prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="3da4b-251">You cannot move any of these Azure resources - Storage1, VM1, and Network1 - across resource groups within hello same subscription or across subscriptions.</span></span>

## <a name="hdinsight-limitations"></a><span data-ttu-id="3da4b-252">HDInsight-begränsningar</span><span class="sxs-lookup"><span data-stu-id="3da4b-252">HDInsight limitations</span></span>

<span data-ttu-id="3da4b-253">Du kan flytta HDInsight-kluster tooa ny prenumeration eller resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="3da4b-253">You can move HDInsight clusters tooa new subscription or resource group.</span></span> <span data-ttu-id="3da4b-254">Men kan inte du flytta mellan prenumerationer hello nätverk resurser länkade toohello HDInsight-kluster (till exempel hello virtuellt nätverk, nätverkskort eller belastningsutjämnare).</span><span class="sxs-lookup"><span data-stu-id="3da4b-254">However, you cannot move across subscriptions hello networking resources linked toohello HDInsight cluster (such as hello virtual network, NIC, or load balancer).</span></span> <span data-ttu-id="3da4b-255">Dessutom kan du flytta tooa ny resursgrupp ett nätverkskort som är anslutna tooa virtuell dator för hello-kluster.</span><span class="sxs-lookup"><span data-stu-id="3da4b-255">In addition, you cannot move tooa new resource group a NIC that is attached tooa virtual machine for hello cluster.</span></span>

<span data-ttu-id="3da4b-256">När du flyttar en ny prenumeration för HDInsight-kluster tooa först flytta andra resurser (till exempel hello storage-konto).</span><span class="sxs-lookup"><span data-stu-id="3da4b-256">When moving an HDInsight cluster tooa new subscription, first move other resources (like hello storage account).</span></span> <span data-ttu-id="3da4b-257">Flytta sedan hello HDInsight-kluster av sig själv.</span><span class="sxs-lookup"><span data-stu-id="3da4b-257">Then, move hello HDInsight cluster by itself.</span></span>

## <a name="classic-deployment-limitations"></a><span data-ttu-id="3da4b-258">Klassisk distribution begränsningar</span><span class="sxs-lookup"><span data-stu-id="3da4b-258">Classic deployment limitations</span></span>
<span data-ttu-id="3da4b-259">hello variera alternativ för att flytta resurser har distribuerats via hello klassiska modellen beroende på om du flyttar hello resurser inom en prenumeration eller tooa ny prenumeration.</span><span class="sxs-lookup"><span data-stu-id="3da4b-259">hello options for moving resources deployed through hello classic model differ based on whether you are moving hello resources within a subscription or tooa new subscription.</span></span>

### <a name="same-subscription"></a><span data-ttu-id="3da4b-260">Samma prenumeration</span><span class="sxs-lookup"><span data-stu-id="3da4b-260">Same subscription</span></span>
<span data-ttu-id="3da4b-261">När du flyttar resurser från en grupp tooanother resurs resursgrupp inom samma prenumeration, hello följande begränsningar gäller hello:</span><span class="sxs-lookup"><span data-stu-id="3da4b-261">When moving resources from one resource group tooanother resource group within hello same subscription, hello following restrictions apply:</span></span>

* <span data-ttu-id="3da4b-262">Virtuella nätverk (klassiskt) kan inte flyttas.</span><span class="sxs-lookup"><span data-stu-id="3da4b-262">Virtual networks (classic) cannot be moved.</span></span>
* <span data-ttu-id="3da4b-263">Virtuella datorer (klassisk) måste flyttas med hello-Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="3da4b-263">Virtual machines (classic) must be moved with hello cloud service.</span></span>
* <span data-ttu-id="3da4b-264">Molntjänsten kan bara flyttas när hello flytta innehåller alla virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="3da4b-264">Cloud service can only be moved when hello move includes all its virtual machines.</span></span>
* <span data-ttu-id="3da4b-265">Endast en molnbaserad tjänst kan flyttas åt gången.</span><span class="sxs-lookup"><span data-stu-id="3da4b-265">Only one cloud service can be moved at a time.</span></span>
* <span data-ttu-id="3da4b-266">Endast en storage-konto (klassiskt) kan flyttas åt gången.</span><span class="sxs-lookup"><span data-stu-id="3da4b-266">Only one storage account (classic) can be moved at a time.</span></span>
* <span data-ttu-id="3da4b-267">Storage-konto (klassisk) inte kan flyttas i hello samma igen med en virtuell dator eller en tjänst i molnet.</span><span class="sxs-lookup"><span data-stu-id="3da4b-267">Storage account (classic) cannot be moved in hello same operation with a virtual machine or a cloud service.</span></span>

<span data-ttu-id="3da4b-268">toomove klassiska resurser tooa ny resursgrupp i Hej samma prenumeration, använda hello standard flytta åtgärder med hjälp av hello [portal](#use-portal), [Azure PowerShell](#use-powershell), [Azure CLI](#use-azure-cli), eller [REST API](#use-rest-api).</span><span class="sxs-lookup"><span data-stu-id="3da4b-268">toomove classic resources tooa new resource group within hello same subscription, use hello standard move operations through hello [portal](#use-portal), [Azure PowerShell](#use-powershell), [Azure CLI](#use-azure-cli), or [REST API](#use-rest-api).</span></span> <span data-ttu-id="3da4b-269">Du använder hello samma åtgärder som du använder för att flytta Resource Manager-resurser.</span><span class="sxs-lookup"><span data-stu-id="3da4b-269">You use hello same operations as you use for moving Resource Manager resources.</span></span>

### <a name="new-subscription"></a><span data-ttu-id="3da4b-270">Ny prenumeration</span><span class="sxs-lookup"><span data-stu-id="3da4b-270">New subscription</span></span>
<span data-ttu-id="3da4b-271">När du flyttar resurser tooa ny prenumeration gäller hello följande begränsningar:</span><span class="sxs-lookup"><span data-stu-id="3da4b-271">When moving resources tooa new subscription, hello following restrictions apply:</span></span>

* <span data-ttu-id="3da4b-272">Alla klassiska resurser i hello prenumerationen måste flyttas i hello samma åtgärd.</span><span class="sxs-lookup"><span data-stu-id="3da4b-272">All classic resources in hello subscription must be moved in hello same operation.</span></span>
* <span data-ttu-id="3da4b-273">Hej målprenumerationen får inte innehålla andra klassiska resurser.</span><span class="sxs-lookup"><span data-stu-id="3da4b-273">hello target subscription must not contain any other classic resources.</span></span>
* <span data-ttu-id="3da4b-274">hello flytta kan endast begäras via en separat REST-API för klassiska flyttar.</span><span class="sxs-lookup"><span data-stu-id="3da4b-274">hello move can only be requested through a separate REST API for classic moves.</span></span> <span data-ttu-id="3da4b-275">hello Resource Manager flytta standardkommandon fungerar inte när du flyttar klassiska resurser tooa ny prenumeration.</span><span class="sxs-lookup"><span data-stu-id="3da4b-275">hello standard Resource Manager move commands do not work when moving classic resources tooa new subscription.</span></span>

<span data-ttu-id="3da4b-276">toomove klassiska resurser tooa ny prenumeration, Använd hello REST-åtgärder som är specifika tooclassic resurser.</span><span class="sxs-lookup"><span data-stu-id="3da4b-276">toomove classic resources tooa new subscription, use hello REST operations that are specific tooclassic resources.</span></span> <span data-ttu-id="3da4b-277">toouse vila, utför följande steg hello:</span><span class="sxs-lookup"><span data-stu-id="3da4b-277">toouse REST, perform hello following steps:</span></span>

1. <span data-ttu-id="3da4b-278">Kontrollera om hello källprenumerationen kan delta i över prenumerationer flytta.</span><span class="sxs-lookup"><span data-stu-id="3da4b-278">Check if hello source subscription can participate in a cross-subscription move.</span></span> <span data-ttu-id="3da4b-279">Använd hello följande åtgärd:</span><span class="sxs-lookup"><span data-stu-id="3da4b-279">Use hello following operation:</span></span>

  ```HTTP   
  POST https://management.azure.com/subscriptions/{sourceSubscriptionId}/providers/Microsoft.ClassicCompute/validateSubscriptionMoveAvailability?api-version=2016-04-01
  ```

     <span data-ttu-id="3da4b-280">I hello frågans brödtext är:</span><span class="sxs-lookup"><span data-stu-id="3da4b-280">In hello request body, include:</span></span>

  ```json
  {
    "role": "source"
  }
  ```

     <span data-ttu-id="3da4b-281">hello-svar för hello valideringen har hello följande format:</span><span class="sxs-lookup"><span data-stu-id="3da4b-281">hello response for hello validation operation is in hello following format:</span></span>

  ```json
  {
    "status": "{status}",
    "reasons": [
      "reason1",
      "reason2"
    ]
  }
  ```

2. <span data-ttu-id="3da4b-282">Kontrollera om hello målprenumerationen kan delta i över prenumerationer flytta.</span><span class="sxs-lookup"><span data-stu-id="3da4b-282">Check if hello destination subscription can participate in a cross-subscription move.</span></span> <span data-ttu-id="3da4b-283">Använd hello följande åtgärd:</span><span class="sxs-lookup"><span data-stu-id="3da4b-283">Use hello following operation:</span></span>

  ```HTTP
  POST https://management.azure.com/subscriptions/{destinationSubscriptionId}/providers/Microsoft.ClassicCompute/validateSubscriptionMoveAvailability?api-version=2016-04-01
  ```

     <span data-ttu-id="3da4b-284">I hello frågans brödtext är:</span><span class="sxs-lookup"><span data-stu-id="3da4b-284">In hello request body, include:</span></span>

  ```json
  {
    "role": "target"
  }
  ```

     <span data-ttu-id="3da4b-285">hello svaret är i hello samma format som hello källa prenumeration validering.</span><span class="sxs-lookup"><span data-stu-id="3da4b-285">hello response is in hello same format as hello source subscription validation.</span></span>
3. <span data-ttu-id="3da4b-286">Om båda prenumerationer har klarat valideringen, flytta alla klassiska resurser från en prenumeration tooanother prenumeration med hello följande åtgärd:</span><span class="sxs-lookup"><span data-stu-id="3da4b-286">If both subscriptions pass validation, move all classic resources from one subscription tooanother subscription with hello following operation:</span></span>

  ```HTTP
  POST https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.ClassicCompute/moveSubscriptionResources?api-version=2016-04-01
  ```

    <span data-ttu-id="3da4b-287">I hello frågans brödtext är:</span><span class="sxs-lookup"><span data-stu-id="3da4b-287">In hello request body, include:</span></span>

  ```json
  {
    "target": "/subscriptions/{target-subscription-id}"
  }
  ```

<span data-ttu-id="3da4b-288">hello-åtgärden kan ta flera minuter.</span><span class="sxs-lookup"><span data-stu-id="3da4b-288">hello operation may run for several minutes.</span></span>

## <a name="use-portal"></a><span data-ttu-id="3da4b-289">Använda portalen</span><span class="sxs-lookup"><span data-stu-id="3da4b-289">Use portal</span></span>
<span data-ttu-id="3da4b-290">toomove resurser, Välj hello resursgruppen som innehåller de resurserna och välj sedan hello **flytta** knappen.</span><span class="sxs-lookup"><span data-stu-id="3da4b-290">toomove resources, select hello resource group containing those resources, and then select hello **Move** button.</span></span>

![Flytta resurser](./media/resource-group-move-resources/select-move.png)

<span data-ttu-id="3da4b-292">Välj om du flyttar hello resurser tooa ny resursgrupp eller en ny prenumeration.</span><span class="sxs-lookup"><span data-stu-id="3da4b-292">Select whether you are moving hello resources tooa new resource group or a new subscription.</span></span>

<span data-ttu-id="3da4b-293">Välj hello resurser toomove och hello mål resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="3da4b-293">Select hello resources toomove and hello destination resource group.</span></span> <span data-ttu-id="3da4b-294">Bekräfta att du behöver tooupdate skript för dessa resurser och välj **OK**.</span><span class="sxs-lookup"><span data-stu-id="3da4b-294">Acknowledge that you need tooupdate scripts for these resources and select **OK**.</span></span> <span data-ttu-id="3da4b-295">Om du har valt hello redigeringsikonen prenumeration i hello föregående steg, måste du också välja hello målprenumerationen.</span><span class="sxs-lookup"><span data-stu-id="3da4b-295">If you selected hello edit subscription icon in hello previous step, you must also select hello destination subscription.</span></span>

![Välj mål](./media/resource-group-move-resources/select-destination.png)

<span data-ttu-id="3da4b-297">I **meddelanden**, du ser att hello flytta åtgärd körs.</span><span class="sxs-lookup"><span data-stu-id="3da4b-297">In **Notifications**, you see that hello move operation is running.</span></span>

![Visa flytta status](./media/resource-group-move-resources/show-status.png)

<span data-ttu-id="3da4b-299">När det har slutförts, meddelas du om hello resultat.</span><span class="sxs-lookup"><span data-stu-id="3da4b-299">When it has completed, you are notified of hello result.</span></span>

![Visa flytta resultat](./media/resource-group-move-resources/show-result.png)

## <a name="use-powershell"></a><span data-ttu-id="3da4b-301">Använd PowerShell</span><span class="sxs-lookup"><span data-stu-id="3da4b-301">Use PowerShell</span></span>
<span data-ttu-id="3da4b-302">toomove befintliga resurser tooanother resursgrupp eller prenumeration, använda hello `Move-AzureRmResource` kommando.</span><span class="sxs-lookup"><span data-stu-id="3da4b-302">toomove existing resources tooanother resource group or subscription, use hello `Move-AzureRmResource` command.</span></span>

<span data-ttu-id="3da4b-303">Hej det första exemplet visas hur toomove en resurs tooa ny resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="3da4b-303">hello first example shows how toomove one resource tooa new resource group.</span></span>

```powershell
$resource = Get-AzureRmResource -ResourceName ExampleApp -ResourceGroupName OldRG
Move-AzureRmResource -DestinationResourceGroupName NewRG -ResourceId $resource.ResourceId
```

<span data-ttu-id="3da4b-304">hello andra exempel visar hur toomove flera resurser tooa ny resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="3da4b-304">hello second example shows how toomove multiple resources tooa new resource group.</span></span>

```powershell
$webapp = Get-AzureRmResource -ResourceGroupName OldRG -ResourceName ExampleSite
$plan = Get-AzureRmResource -ResourceGroupName OldRG -ResourceName ExamplePlan
Move-AzureRmResource -DestinationResourceGroupName NewRG -ResourceId $webapp.ResourceId, $plan.ResourceId
```

<span data-ttu-id="3da4b-305">toomove tooa ny prenumeration, innehåller ett värde för hello `DestinationSubscriptionId` parameter.</span><span class="sxs-lookup"><span data-stu-id="3da4b-305">toomove tooa new subscription, include a value for hello `DestinationSubscriptionId` parameter.</span></span>

<span data-ttu-id="3da4b-306">Du uppmanas tooconfirm som du vill toomove hello anges resurser.</span><span class="sxs-lookup"><span data-stu-id="3da4b-306">You are asked tooconfirm that you want toomove hello specified resources.</span></span>

```powershell
Confirm
Are you sure you want toomove these resources toohello resource group
'/subscriptions/{guid}/resourceGroups/newRG' hello resources:

/subscriptions/{guid}/resourceGroups/destinationgroup/providers/Microsoft.Web/serverFarms/exampleplan
/subscriptions/{guid}/resourceGroups/destinationgroup/providers/Microsoft.Web/sites/examplesite
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): y
```

## <a name="use-azure-cli-20"></a><span data-ttu-id="3da4b-307">Använda Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="3da4b-307">Use Azure CLI 2.0</span></span>
<span data-ttu-id="3da4b-308">toomove befintliga resurser tooanother resursgrupp eller prenumeration, använda hello `az resource move` kommando.</span><span class="sxs-lookup"><span data-stu-id="3da4b-308">toomove existing resources tooanother resource group or subscription, use hello `az resource move` command.</span></span> <span data-ttu-id="3da4b-309">Ange hello resurs-ID för hello resurser toomove.</span><span class="sxs-lookup"><span data-stu-id="3da4b-309">Provide hello resource IDs of hello resources toomove.</span></span> <span data-ttu-id="3da4b-310">Du kan få resurs-ID med hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="3da4b-310">You can get resource IDs with hello following command:</span></span>

```azurecli
az resource show -g sourceGroup -n storagedemo --resource-type "Microsoft.Storage/storageAccounts" --query id
```

<span data-ttu-id="3da4b-311">hello som följande exempel visar hur toomove en lagringsplats för kontot tooa ny resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="3da4b-311">hello following example shows how toomove a storage account tooa new resource group.</span></span> <span data-ttu-id="3da4b-312">I hello `--ids` parameter, ange en blankstegsavgränsad lista över hello resurs-ID: N toomove.</span><span class="sxs-lookup"><span data-stu-id="3da4b-312">In hello `--ids` parameter, provide a space-separated list of hello resource IDs toomove.</span></span>

```azurecli
az resource move --destination-group newgroup --ids "/subscriptions/{guid}/resourceGroups/sourceGroup/providers/Microsoft.Storage/storageAccounts/storagedemo"
```

<span data-ttu-id="3da4b-313">toomove tooa ny prenumeration, ange hello `--destination-subscription-id` parameter.</span><span class="sxs-lookup"><span data-stu-id="3da4b-313">toomove tooa new subscription, provide hello `--destination-subscription-id` parameter.</span></span>

## <a name="use-azure-cli-10"></a><span data-ttu-id="3da4b-314">Använda Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="3da4b-314">Use Azure CLI 1.0</span></span>
<span data-ttu-id="3da4b-315">toomove befintliga resurser tooanother resursgrupp eller prenumeration, använda hello `azure resource move` kommando.</span><span class="sxs-lookup"><span data-stu-id="3da4b-315">toomove existing resources tooanother resource group or subscription, use hello `azure resource move` command.</span></span> <span data-ttu-id="3da4b-316">Ange hello resurs-ID för hello resurser toomove.</span><span class="sxs-lookup"><span data-stu-id="3da4b-316">Provide hello resource IDs of hello resources toomove.</span></span> <span data-ttu-id="3da4b-317">Du kan få resurs-ID med hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="3da4b-317">You can get resource IDs with hello following command:</span></span>

```azurecli
azure resource list -g sourceGroup --json
```

<span data-ttu-id="3da4b-318">Som returnerar hello följande format:</span><span class="sxs-lookup"><span data-stu-id="3da4b-318">Which returns hello following format:</span></span>

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

<span data-ttu-id="3da4b-319">hello som följande exempel visar hur toomove en lagringsplats för kontot tooa ny resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="3da4b-319">hello following example shows how toomove a storage account tooa new resource group.</span></span> <span data-ttu-id="3da4b-320">I hello `-i` parameter, ange en kommaavgränsad lista över hello resurs-ID: N toomove.</span><span class="sxs-lookup"><span data-stu-id="3da4b-320">In hello `-i` parameter, provide a comma-separated list of hello resource IDs toomove.</span></span>

```azurecli
azure resource move -i "/subscriptions/{guid}/resourceGroups/sourceGroup/providers/Microsoft.Storage/storageAccounts/storagedemo" -d "destinationGroup"
```

<span data-ttu-id="3da4b-321">Du uppmanas tooconfirm som du vill toomove hello angivna resursen.</span><span class="sxs-lookup"><span data-stu-id="3da4b-321">You are asked tooconfirm that you want toomove hello specified resource.</span></span>

## <a name="use-rest-api"></a><span data-ttu-id="3da4b-322">Använd REST-API</span><span class="sxs-lookup"><span data-stu-id="3da4b-322">Use REST API</span></span>
<span data-ttu-id="3da4b-323">toomove befintliga resurser tooanother resursgrupp eller prenumeration, kör:</span><span class="sxs-lookup"><span data-stu-id="3da4b-323">toomove existing resources tooanother resource group or subscription, run:</span></span>

```HTTP
POST https://management.azure.com/subscriptions/{source-subscription-id}/resourcegroups/{source-resource-group-name}/moveResources?api-version={api-version}
```

<span data-ttu-id="3da4b-324">I hello begärantext anger du hello målresursgruppen och hello resurser toomove.</span><span class="sxs-lookup"><span data-stu-id="3da4b-324">In hello request body, you specify hello target resource group and hello resources toomove.</span></span> <span data-ttu-id="3da4b-325">Läs mer om hello flytta REST-åtgärden [flyttar resurser](https://msdn.microsoft.com/library/azure/mt218710.aspx).</span><span class="sxs-lookup"><span data-stu-id="3da4b-325">For more information about hello move REST operation, see [Move resources](https://msdn.microsoft.com/library/azure/mt218710.aspx).</span></span>

## <a name="next-steps"></a><span data-ttu-id="3da4b-326">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3da4b-326">Next steps</span></span>
* <span data-ttu-id="3da4b-327">toolearn om PowerShell-cmdletar för att hantera din prenumeration, se [med hjälp av Azure PowerShell med Resource Manager](powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="3da4b-327">toolearn about PowerShell cmdlets for managing your subscription, see [Using Azure PowerShell with Resource Manager](powershell-azure-resource-manager.md).</span></span>
* <span data-ttu-id="3da4b-328">toolearn om Azure CLI-kommandon för att hantera din prenumeration, se [Using hello Azure CLI med Resource Manager](xplat-cli-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="3da4b-328">toolearn about Azure CLI commands for managing your subscription, see [Using hello Azure CLI with Resource Manager](xplat-cli-azure-resource-manager.md).</span></span>
* <span data-ttu-id="3da4b-329">toolearn om portalen funktioner för att hantera din prenumeration, se [använder hello Azure portal toomanage resurser](resource-group-portal.md).</span><span class="sxs-lookup"><span data-stu-id="3da4b-329">toolearn about portal features for managing your subscription, see [Using hello Azure portal toomanage resources](resource-group-portal.md).</span></span>
* <span data-ttu-id="3da4b-330">toolearn om hur du kopplar ett logiskt tooyour resurser, se [med hjälp av taggar tooorganize dina resurser](resource-group-using-tags.md).</span><span class="sxs-lookup"><span data-stu-id="3da4b-330">toolearn about applying a logical organization tooyour resources, see [Using tags tooorganize your resources](resource-group-using-tags.md).</span></span>
