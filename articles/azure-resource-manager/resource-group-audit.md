---
title: aaaView Azure aktivitet loggar toomonitor resurser | Microsoft Docs
description: "Använd hello aktivitet loggar tooreview användaråtgärder och fel. Visar PowerShell för Azure Portal, Azure CLI och REST."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: fcdb3125-13ce-4c3b-9087-f514c5e41e73
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: tomfitz
ms.openlocfilehash: 8430ed2a9c1dfe5f13423a55d358e590b0facb22
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="view-activity-logs-tooaudit-actions-on-resources"></a><span data-ttu-id="6849c-104">Visa aktivitet loggar tooaudit åtgärder på resurser</span><span class="sxs-lookup"><span data-stu-id="6849c-104">View activity logs tooaudit actions on resources</span></span>
<span data-ttu-id="6849c-105">Du kan bestämma via aktivitetsloggar:</span><span class="sxs-lookup"><span data-stu-id="6849c-105">Through activity logs, you can determine:</span></span>

* <span data-ttu-id="6849c-106">vilka åtgärder som utförts på hello resurser i din prenumeration</span><span class="sxs-lookup"><span data-stu-id="6849c-106">what operations were taken on hello resources in your subscription</span></span>
* <span data-ttu-id="6849c-107">vem som initierat hello åtgärden (även om en serverdelstjänst åtgärder inte returnerar en användare som hello anroparen)</span><span class="sxs-lookup"><span data-stu-id="6849c-107">who initiated hello operation (although operations initiated by a backend service do not return a user as hello caller)</span></span>
* <span data-ttu-id="6849c-108">När hello åtgärden uppstod</span><span class="sxs-lookup"><span data-stu-id="6849c-108">when hello operation occurred</span></span>
* <span data-ttu-id="6849c-109">hello status för hello åtgärd</span><span class="sxs-lookup"><span data-stu-id="6849c-109">hello status of hello operation</span></span>
* <span data-ttu-id="6849c-110">hello värdena för andra egenskaper som kan hjälpa dig undersöka hello åtgärden</span><span class="sxs-lookup"><span data-stu-id="6849c-110">hello values of other properties that might help you research hello operation</span></span>

[!INCLUDE [resource-manager-audit-limitations](../../includes/resource-manager-audit-limitations.md)]

<span data-ttu-id="6849c-111">Du kan hämta information från hello aktivitetsloggar hello-portalen, PowerShell, Azure CLI, insikter REST API eller [insikter .NET-bibliotek](https://www.nuget.org/packages/Microsoft.Azure.Insights/).</span><span class="sxs-lookup"><span data-stu-id="6849c-111">You can retrieve information from hello activity logs through hello portal, PowerShell, Azure CLI, Insights REST API, or [Insights .NET Library](https://www.nuget.org/packages/Microsoft.Azure.Insights/).</span></span>

## <a name="portal"></a><span data-ttu-id="6849c-112">Portalen</span><span class="sxs-lookup"><span data-stu-id="6849c-112">Portal</span></span>
1. <span data-ttu-id="6849c-113">tooview hello aktivitetsloggar hello-portalen, Välj **övervakaren**.</span><span class="sxs-lookup"><span data-stu-id="6849c-113">tooview hello activity logs through hello portal, select **Monitor**.</span></span>
   
    ![Välj aktivitetsloggar](./media/resource-group-audit/select-monitor.png)

   <span data-ttu-id="6849c-115">Eller tooautomatically filter hello aktivitetsloggen för en viss resurs eller en resursgrupp, Välj **aktivitetsloggen** från den resursbladet.</span><span class="sxs-lookup"><span data-stu-id="6849c-115">Or, tooautomatically filter hello activity log for a particular resource or resource group, select **Activity log** from that resource blade.</span></span> <span data-ttu-id="6849c-116">Observera att hello aktivitetsloggen filtreras automatiskt av hello markerad resurs.</span><span class="sxs-lookup"><span data-stu-id="6849c-116">Notice that hello activity log is automatically filtered by hello selected resource.</span></span>
   
    ![Filtrera efter resurs](./media/resource-group-audit/filtered-by-resource.png)
2. <span data-ttu-id="6849c-118">I hello **aktivitetsloggen** bladet visas en sammanfattning av nya åtgärder.</span><span class="sxs-lookup"><span data-stu-id="6849c-118">In hello **Activity Log** blade, you see a summary of recent operations.</span></span>
   
    ![Visa åtgärder](./media/resource-group-audit/audit-summary.png)
3. <span data-ttu-id="6849c-120">toorestrict hello antal åtgärder som visas, Välj olika villkor.</span><span class="sxs-lookup"><span data-stu-id="6849c-120">toorestrict hello number of operations displayed, select different conditions.</span></span> <span data-ttu-id="6849c-121">Hello följande bild visar exempelvis hello **Timespan** och **händelse som initieras av** fält ändras tooview hello åtgärder som vidtas av en viss användare eller ett program för hello senaste månaden.</span><span class="sxs-lookup"><span data-stu-id="6849c-121">For example, hello following image shows hello **Timespan** and **Event initiated by** fields changed tooview hello actions taken by a particular user or application for hello past month.</span></span> <span data-ttu-id="6849c-122">Välj **tillämpa** tooview hello resultatet av frågan.</span><span class="sxs-lookup"><span data-stu-id="6849c-122">Select **Apply** tooview hello results of your query.</span></span>
   
    ![Ange alternativ för filter](./media/resource-group-audit/set-filter.png)

4. <span data-ttu-id="6849c-124">Om du behöver toorun hello fråga igen senare, Välj **spara** och namnge hello frågan.</span><span class="sxs-lookup"><span data-stu-id="6849c-124">If you need toorun hello query again later, select **Save** and give hello query a name.</span></span>
   
    ![Spara frågan](./media/resource-group-audit/save-query.png)
5. <span data-ttu-id="6849c-126">tooquickly köra en fråga, du kan välja något av hello inbyggda frågor, till exempel inte kunde distribueras.</span><span class="sxs-lookup"><span data-stu-id="6849c-126">tooquickly run a query, you can select one of hello built-in queries, such as failed deployments.</span></span>

    ![Välj fråga](./media/resource-group-audit/select-quick-query.png)

   <span data-ttu-id="6849c-128">hello valda frågan anger automatiskt hello krävs filtervärden.</span><span class="sxs-lookup"><span data-stu-id="6849c-128">hello selected query automatically sets hello required filter values.</span></span>

    ![Visa distributionsfel](./media/resource-group-audit/view-failed-deployment.png)   

6. <span data-ttu-id="6849c-130">Välj en av hello operations toosee en sammanfattning av hello-händelse.</span><span class="sxs-lookup"><span data-stu-id="6849c-130">Select one of hello operations toosee a summary of hello event.</span></span>

    ![Visa åtgärden](./media/resource-group-audit/view-operation.png)  

## <a name="powershell"></a><span data-ttu-id="6849c-132">PowerShell</span><span class="sxs-lookup"><span data-stu-id="6849c-132">PowerShell</span></span>
1. <span data-ttu-id="6849c-133">tooretrieve loggposter kör hello **Get-AzureRmLog** kommando.</span><span class="sxs-lookup"><span data-stu-id="6849c-133">tooretrieve log entries, run hello **Get-AzureRmLog** command.</span></span> <span data-ttu-id="6849c-134">Du kan ange ytterligare parametrar toofilter hello listan med poster.</span><span class="sxs-lookup"><span data-stu-id="6849c-134">You provide additional parameters toofilter hello list of entries.</span></span> <span data-ttu-id="6849c-135">Om du inte anger en start- och Slutdatum tid returneras poster för hello senaste timmen.</span><span class="sxs-lookup"><span data-stu-id="6849c-135">If you do not specify a start and end time, entries for hello last hour are returned.</span></span> <span data-ttu-id="6849c-136">Till exempel köra tooretrieve hello åtgärder för en resursgrupp under hello senaste timmen</span><span class="sxs-lookup"><span data-stu-id="6849c-136">For example, tooretrieve hello operations for a resource group during hello past hour run:</span></span>

  ```powershell
  Get-AzureRmLog -ResourceGroup ExampleGroup
  ```
   
    <span data-ttu-id="6849c-137">hello som följande exempel visar hur toouse hello aktivitet logga tooresearch åtgärder som vidtagits under en angiven tid.</span><span class="sxs-lookup"><span data-stu-id="6849c-137">hello following example shows how toouse hello activity log tooresearch operations taken during a specified time.</span></span> <span data-ttu-id="6849c-138">hello har start- och slutdatum angetts i ett datumformat.</span><span class="sxs-lookup"><span data-stu-id="6849c-138">hello start and end dates are specified in a date format.</span></span>

  ```powershell
  Get-AzureRmLog -ResourceGroup ExampleGroup -StartTime 2015-08-28T06:00 -EndTime 2015-09-10T06:00
  ```

    <span data-ttu-id="6849c-139">Eller så kan du använda funktioner toospecify hello datum datumintervall, till exempel hello senaste 14 dagarna.</span><span class="sxs-lookup"><span data-stu-id="6849c-139">Or, you can use date functions toospecify hello date range, such as hello last 14 days.</span></span>
   
  ```powershell 
  Get-AzureRmLog -ResourceGroup ExampleGroup -StartTime (Get-Date).AddDays(-14)
  ```

2. <span data-ttu-id="6849c-140">Beroende på hello starttid som du anger, kan hello tidigare kommandon returnera en lång lista med åtgärder för hello resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="6849c-140">Depending on hello start time you specify, hello previous commands can return a long list of operations for hello resource group.</span></span> <span data-ttu-id="6849c-141">Du kan filtrera hello resultat för det du söker genom att ange sökvillkor.</span><span class="sxs-lookup"><span data-stu-id="6849c-141">You can filter hello results for what you are looking for by providing search criteria.</span></span> <span data-ttu-id="6849c-142">Om du försöker tooresearch hur en webbapp har stoppats, kan du köra hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="6849c-142">For example, if you are trying tooresearch how a web app was stopped, you could run hello following command:</span></span>

  ```powershell
  Get-AzureRmLog -ResourceGroup ExampleGroup -StartTime (Get-Date).AddDays(-14) | Where-Object OperationName -eq Microsoft.Web/sites/stop/action
  ```

    <span data-ttu-id="6849c-143">Vilket det här exemplet visar att en stoppåtgärden utfördes av someone@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="6849c-143">Which for this example shows that a stop action was performed by someone@contoso.com.</span></span> 

  ```powershell 
  Authorization     :
  Scope     : /subscriptions/xxxxx/resourcegroups/ExampleGroup/providers/Microsoft.Web/sites/ExampleSite
  Action    : Microsoft.Web/sites/stop/action
  Role      : Subscription Admin
  Condition :
  Caller            : someone@contoso.com
  CorrelationId     : 84beae59-92aa-4662-a6fc-b6fecc0ff8da
  EventSource       : Administrative
  EventTimestamp    : 8/28/2015 4:08:18 PM
  OperationName     : Microsoft.Web/sites/stop/action
  ResourceGroupName : ExampleGroup
  ResourceId        : /subscriptions/xxxxx/resourcegroups/ExampleGroup/providers/Microsoft.Web/sites/ExampleSite
  Status            : Succeeded
  SubscriptionId    : xxxxx
  SubStatus         : OK
  ```

3. <span data-ttu-id="6849c-144">Du kan slå upp hello-åtgärder som vidtas av en viss användare, även för en resursgrupp som inte längre finns.</span><span class="sxs-lookup"><span data-stu-id="6849c-144">You can look up hello actions taken by a particular user, even for a resource group that no longer exists.</span></span>

  ```powershell 
  Get-AzureRmLog -ResourceGroup deletedgroup -StartTime (Get-Date).AddDays(-14) -Caller someone@contoso.com
  ```

4. <span data-ttu-id="6849c-145">Du kan filtrera efter misslyckade åtgärder.</span><span class="sxs-lookup"><span data-stu-id="6849c-145">You can filter for failed operations.</span></span>

  ```powershell
  Get-AzureRmLog -ResourceGroup ExampleGroup -Status Failed
  ```

5. <span data-ttu-id="6849c-146">Du kan fokusera på ett fel genom att titta på hello statusmeddelande för posten.</span><span class="sxs-lookup"><span data-stu-id="6849c-146">You can focus on one error by looking at hello status message for that entry.</span></span>
   
        ((Get-AzureRmLog -Status Failed -ResourceGroup ExampleGroup -DetailedOutput).Properties[1].Content["statusMessage"] | ConvertFrom-Json).error
   
    <span data-ttu-id="6849c-147">Som returnerar:</span><span class="sxs-lookup"><span data-stu-id="6849c-147">Which returns:</span></span>
   
        code           message                                                                        
        ----           -------                                                                        
        DnsRecordInUse DNS record dns.westus.cloudapp.azure.com is already used by another public IP. 


## <a name="azure-cli"></a><span data-ttu-id="6849c-148">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="6849c-148">Azure CLI</span></span>
* <span data-ttu-id="6849c-149">tooretrieve loggposter du kör hello **azure-grupp loggen visar** kommando.</span><span class="sxs-lookup"><span data-stu-id="6849c-149">tooretrieve log entries, you run hello **azure group log show** command.</span></span>

  ```azurecli
  azure group log show ExampleGroup --json
  ```


## <a name="rest-api"></a><span data-ttu-id="6849c-150">REST API</span><span class="sxs-lookup"><span data-stu-id="6849c-150">REST API</span></span>
<span data-ttu-id="6849c-151">hello REST-åtgärder för att arbeta med hello aktivitetsloggen är en del av hello [insikter REST API](https://msdn.microsoft.com/library/azure/dn931943.aspx).</span><span class="sxs-lookup"><span data-stu-id="6849c-151">hello REST operations for working with hello activity log are part of hello [Insights REST API](https://msdn.microsoft.com/library/azure/dn931943.aspx).</span></span> <span data-ttu-id="6849c-152">logghändelser för tooretrieve aktivitet finns [hello management händelser i en prenumeration](https://msdn.microsoft.com/library/azure/dn931934.aspx).</span><span class="sxs-lookup"><span data-stu-id="6849c-152">tooretrieve activity log events, see [List hello management events in a subscription](https://msdn.microsoft.com/library/azure/dn931934.aspx).</span></span>

## <a name="next-steps"></a><span data-ttu-id="6849c-153">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6849c-153">Next steps</span></span>
* <span data-ttu-id="6849c-154">Azure aktivitetsloggar kan användas med Power BI toogain större insikter om hello åtgärder i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="6849c-154">Azure Activity logs can be used with Power BI toogain greater insights about hello actions in your subscription.</span></span> <span data-ttu-id="6849c-155">Se [visa och analysera Azure aktivitetsloggar i Power BI och mycket mer](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/).</span><span class="sxs-lookup"><span data-stu-id="6849c-155">See [View and analyze Azure Activity Logs in Power BI and more](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/).</span></span>
* <span data-ttu-id="6849c-156">toolearn om att ställa in säkerhetsprinciper, se [Azure rollbaserad åtkomstkontroll](../active-directory/role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="6849c-156">toolearn about setting security policies, see [Azure Role-based Access Control](../active-directory/role-based-access-control-configure.md).</span></span>
* <span data-ttu-id="6849c-157">toolearn om hello-kommandon för att visa distributionsåtgärder, se [visa distributionsåtgärder](resource-manager-deployment-operations.md).</span><span class="sxs-lookup"><span data-stu-id="6849c-157">toolearn about hello commands for viewing deployment operations, see [View deployment operations](resource-manager-deployment-operations.md).</span></span>
* <span data-ttu-id="6849c-158">hur tooprevent borttagningar på en resurs för alla användare, se toolearn [låsa resurser med Azure Resource Manager](resource-group-lock-resources.md).</span><span class="sxs-lookup"><span data-stu-id="6849c-158">toolearn how tooprevent deletions on a resource for all users, see [Lock resources with Azure Resource Manager](resource-group-lock-resources.md).</span></span>

