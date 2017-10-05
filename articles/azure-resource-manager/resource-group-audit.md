---
title: "Visa Azure aktivitetsloggar att övervaka resurser | Microsoft Docs"
description: "Använd aktivitetsloggar till användaråtgärder för granskning och fel. Visar PowerShell för Azure Portal, Azure CLI och REST."
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
ms.openlocfilehash: 9f90bc80c146c6c2da04aacbc110f7d389c0baa2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="view-activity-logs-to-audit-actions-on-resources"></a><span data-ttu-id="9f049-104">Visa aktivitetsloggar granska åtgärder på resurser</span><span class="sxs-lookup"><span data-stu-id="9f049-104">View activity logs to audit actions on resources</span></span>
<span data-ttu-id="9f049-105">Du kan bestämma via aktivitetsloggar:</span><span class="sxs-lookup"><span data-stu-id="9f049-105">Through activity logs, you can determine:</span></span>

* <span data-ttu-id="9f049-106">vilka åtgärder som utförts på resurserna i prenumerationen</span><span class="sxs-lookup"><span data-stu-id="9f049-106">what operations were taken on the resources in your subscription</span></span>
* <span data-ttu-id="9f049-107">vem som initierat åtgärden (även om en serverdelstjänst åtgärder inte returnerar en användare som anroparen)</span><span class="sxs-lookup"><span data-stu-id="9f049-107">who initiated the operation (although operations initiated by a backend service do not return a user as the caller)</span></span>
* <span data-ttu-id="9f049-108">När åtgärden utfördes</span><span class="sxs-lookup"><span data-stu-id="9f049-108">when the operation occurred</span></span>
* <span data-ttu-id="9f049-109">Status för åtgärden</span><span class="sxs-lookup"><span data-stu-id="9f049-109">the status of the operation</span></span>
* <span data-ttu-id="9f049-110">Värdena för andra egenskaper som kan hjälpa dig undersöka igen</span><span class="sxs-lookup"><span data-stu-id="9f049-110">the values of other properties that might help you research the operation</span></span>

[!INCLUDE [resource-manager-audit-limitations](../../includes/resource-manager-audit-limitations.md)]

<span data-ttu-id="9f049-111">Du kan hämta information från aktivitetsloggar via portalen, PowerShell, Azure CLI, insikter REST API eller [insikter .NET-bibliotek](https://www.nuget.org/packages/Microsoft.Azure.Insights/).</span><span class="sxs-lookup"><span data-stu-id="9f049-111">You can retrieve information from the activity logs through the portal, PowerShell, Azure CLI, Insights REST API, or [Insights .NET Library](https://www.nuget.org/packages/Microsoft.Azure.Insights/).</span></span>

## <a name="portal"></a><span data-ttu-id="9f049-112">Portalen</span><span class="sxs-lookup"><span data-stu-id="9f049-112">Portal</span></span>
1. <span data-ttu-id="9f049-113">Om du vill visa aktivitetsloggar via portalen **övervakaren**.</span><span class="sxs-lookup"><span data-stu-id="9f049-113">To view the activity logs through the portal, select **Monitor**.</span></span>
   
    ![Välj aktivitetsloggar](./media/resource-group-audit/select-monitor.png)

   <span data-ttu-id="9f049-115">Om du vill filtrera automatiskt aktivitetsloggen för en viss resurs eller en resursgrupp, väljer du **aktivitetsloggen** från den resursbladet.</span><span class="sxs-lookup"><span data-stu-id="9f049-115">Or, to automatically filter the activity log for a particular resource or resource group, select **Activity log** from that resource blade.</span></span> <span data-ttu-id="9f049-116">Observera att aktivitetsloggen filtreras automatiskt av den valda resursen.</span><span class="sxs-lookup"><span data-stu-id="9f049-116">Notice that the activity log is automatically filtered by the selected resource.</span></span>
   
    ![Filtrera efter resurs](./media/resource-group-audit/filtered-by-resource.png)
2. <span data-ttu-id="9f049-118">I den **aktivitetsloggen** bladet visas en sammanfattning av nya åtgärder.</span><span class="sxs-lookup"><span data-stu-id="9f049-118">In the **Activity Log** blade, you see a summary of recent operations.</span></span>
   
    ![Visa åtgärder](./media/resource-group-audit/audit-summary.png)
3. <span data-ttu-id="9f049-120">Välj olika villkor för att begränsa antalet åtgärder visas.</span><span class="sxs-lookup"><span data-stu-id="9f049-120">To restrict the number of operations displayed, select different conditions.</span></span> <span data-ttu-id="9f049-121">Till exempel följande bild visar den **Timespan** och **händelse som initieras av** fält ändras för att visa de åtgärder som vidtas av en viss användare eller ett program för den senaste månaden.</span><span class="sxs-lookup"><span data-stu-id="9f049-121">For example, the following image shows the **Timespan** and **Event initiated by** fields changed to view the actions taken by a particular user or application for the past month.</span></span> <span data-ttu-id="9f049-122">Välj **tillämpa** att visa resultat av frågan.</span><span class="sxs-lookup"><span data-stu-id="9f049-122">Select **Apply** to view the results of your query.</span></span>
   
    ![Ange alternativ för filter](./media/resource-group-audit/set-filter.png)

4. <span data-ttu-id="9f049-124">Om du behöver köra frågan igen senare väljer **spara** och ge ett namn för frågan.</span><span class="sxs-lookup"><span data-stu-id="9f049-124">If you need to run the query again later, select **Save** and give the query a name.</span></span>
   
    ![Spara frågan](./media/resource-group-audit/save-query.png)
5. <span data-ttu-id="9f049-126">Du kan välja något av de inbyggda frågorna, till exempel inte kunde distribueras för att köra en fråga snabbt.</span><span class="sxs-lookup"><span data-stu-id="9f049-126">To quickly run a query, you can select one of the built-in queries, such as failed deployments.</span></span>

    ![Välj fråga](./media/resource-group-audit/select-quick-query.png)

   <span data-ttu-id="9f049-128">Den valda frågan anger automatiskt filtervärdena som krävs.</span><span class="sxs-lookup"><span data-stu-id="9f049-128">The selected query automatically sets the required filter values.</span></span>

    ![Visa distributionsfel](./media/resource-group-audit/view-failed-deployment.png)   

6. <span data-ttu-id="9f049-130">Välj en av åtgärderna för att visa en sammanfattning av händelsen.</span><span class="sxs-lookup"><span data-stu-id="9f049-130">Select one of the operations to see a summary of the event.</span></span>

    ![Visa åtgärden](./media/resource-group-audit/view-operation.png)  

## <a name="powershell"></a><span data-ttu-id="9f049-132">PowerShell</span><span class="sxs-lookup"><span data-stu-id="9f049-132">PowerShell</span></span>
1. <span data-ttu-id="9f049-133">Om du vill hämta loggposter kör den **Get-AzureRmLog** kommando.</span><span class="sxs-lookup"><span data-stu-id="9f049-133">To retrieve log entries, run the **Get-AzureRmLog** command.</span></span> <span data-ttu-id="9f049-134">Du kan ange ytterligare parametrar om du vill filtrera listan med poster.</span><span class="sxs-lookup"><span data-stu-id="9f049-134">You provide additional parameters to filter the list of entries.</span></span> <span data-ttu-id="9f049-135">Om du inte anger en start- och Slutdatum tid returneras poster för den senaste timmen.</span><span class="sxs-lookup"><span data-stu-id="9f049-135">If you do not specify a start and end time, entries for the last hour are returned.</span></span> <span data-ttu-id="9f049-136">Till exempel för att hämta kör åtgärder för en resursgrupp under den senaste timmen:</span><span class="sxs-lookup"><span data-stu-id="9f049-136">For example, to retrieve the operations for a resource group during the past hour run:</span></span>

  ```powershell
  Get-AzureRmLog -ResourceGroup ExampleGroup
  ```
   
    <span data-ttu-id="9f049-137">I följande exempel visas hur du använder aktivitetsloggen till research åtgärder som vidtagits under en angiven tid.</span><span class="sxs-lookup"><span data-stu-id="9f049-137">The following example shows how to use the activity log to research operations taken during a specified time.</span></span> <span data-ttu-id="9f049-138">Start- och slutdatum har angetts i ett datumformat.</span><span class="sxs-lookup"><span data-stu-id="9f049-138">The start and end dates are specified in a date format.</span></span>

  ```powershell
  Get-AzureRmLog -ResourceGroup ExampleGroup -StartTime 2015-08-28T06:00 -EndTime 2015-09-10T06:00
  ```

    <span data-ttu-id="9f049-139">Eller så kan du använda datumfunktioner för att ange datumintervallet, till exempel de senaste 14 dagarna.</span><span class="sxs-lookup"><span data-stu-id="9f049-139">Or, you can use date functions to specify the date range, such as the last 14 days.</span></span>
   
  ```powershell 
  Get-AzureRmLog -ResourceGroup ExampleGroup -StartTime (Get-Date).AddDays(-14)
  ```

2. <span data-ttu-id="9f049-140">Beroende på den starttid som du anger kan de föregående kommandona returnera en lång lista med åtgärder för resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="9f049-140">Depending on the start time you specify, the previous commands can return a long list of operations for the resource group.</span></span> <span data-ttu-id="9f049-141">Du kan filtrera resultaten för vad du letar efter genom att ange sökvillkor.</span><span class="sxs-lookup"><span data-stu-id="9f049-141">You can filter the results for what you are looking for by providing search criteria.</span></span> <span data-ttu-id="9f049-142">Om du vill undersöka hur en webbapp har stoppats, kan du köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="9f049-142">For example, if you are trying to research how a web app was stopped, you could run the following command:</span></span>

  ```powershell
  Get-AzureRmLog -ResourceGroup ExampleGroup -StartTime (Get-Date).AddDays(-14) | Where-Object OperationName -eq Microsoft.Web/sites/stop/action
  ```

    <span data-ttu-id="9f049-143">Vilket det här exemplet visar att en stoppåtgärden utfördes av someone@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="9f049-143">Which for this example shows that a stop action was performed by someone@contoso.com.</span></span> 

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

3. <span data-ttu-id="9f049-144">Du kan söka efter de åtgärder som vidtas för en viss användare, även för en resursgrupp som inte längre finns.</span><span class="sxs-lookup"><span data-stu-id="9f049-144">You can look up the actions taken by a particular user, even for a resource group that no longer exists.</span></span>

  ```powershell 
  Get-AzureRmLog -ResourceGroup deletedgroup -StartTime (Get-Date).AddDays(-14) -Caller someone@contoso.com
  ```

4. <span data-ttu-id="9f049-145">Du kan filtrera efter misslyckade åtgärder.</span><span class="sxs-lookup"><span data-stu-id="9f049-145">You can filter for failed operations.</span></span>

  ```powershell
  Get-AzureRmLog -ResourceGroup ExampleGroup -Status Failed
  ```

5. <span data-ttu-id="9f049-146">Du kan fokusera på ett fel genom att titta på statusmeddelanden för posten.</span><span class="sxs-lookup"><span data-stu-id="9f049-146">You can focus on one error by looking at the status message for that entry.</span></span>
   
        ((Get-AzureRmLog -Status Failed -ResourceGroup ExampleGroup -DetailedOutput).Properties[1].Content["statusMessage"] | ConvertFrom-Json).error
   
    <span data-ttu-id="9f049-147">Som returnerar:</span><span class="sxs-lookup"><span data-stu-id="9f049-147">Which returns:</span></span>
   
        code           message                                                                        
        ----           -------                                                                        
        DnsRecordInUse DNS record dns.westus.cloudapp.azure.com is already used by another public IP. 


## <a name="azure-cli"></a><span data-ttu-id="9f049-148">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="9f049-148">Azure CLI</span></span>
* <span data-ttu-id="9f049-149">Om du vill hämta loggposter måste du köra den **azure-grupp loggen visar** kommando.</span><span class="sxs-lookup"><span data-stu-id="9f049-149">To retrieve log entries, you run the **azure group log show** command.</span></span>

  ```azurecli
  azure group log show ExampleGroup --json
  ```


## <a name="rest-api"></a><span data-ttu-id="9f049-150">REST API</span><span class="sxs-lookup"><span data-stu-id="9f049-150">REST API</span></span>
<span data-ttu-id="9f049-151">REST-åtgärder för att arbeta med aktivitetsloggen är en del av den [insikter REST API](https://msdn.microsoft.com/library/azure/dn931943.aspx).</span><span class="sxs-lookup"><span data-stu-id="9f049-151">The REST operations for working with the activity log are part of the [Insights REST API](https://msdn.microsoft.com/library/azure/dn931943.aspx).</span></span> <span data-ttu-id="9f049-152">Om du vill hämta aktivitet logghändelser finns [management-händelser i en prenumeration](https://msdn.microsoft.com/library/azure/dn931934.aspx).</span><span class="sxs-lookup"><span data-stu-id="9f049-152">To retrieve activity log events, see [List the management events in a subscription](https://msdn.microsoft.com/library/azure/dn931934.aspx).</span></span>

## <a name="next-steps"></a><span data-ttu-id="9f049-153">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9f049-153">Next steps</span></span>
* <span data-ttu-id="9f049-154">Azure aktivitetsloggar kan användas med Power BI och få större insikter om åtgärderna i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="9f049-154">Azure Activity logs can be used with Power BI to gain greater insights about the actions in your subscription.</span></span> <span data-ttu-id="9f049-155">Se [visa och analysera Azure aktivitetsloggar i Power BI och mycket mer](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/).</span><span class="sxs-lookup"><span data-stu-id="9f049-155">See [View and analyze Azure Activity Logs in Power BI and more](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/).</span></span>
* <span data-ttu-id="9f049-156">Mer information om att ställa in säkerhetsprinciper, se [Azure rollbaserad åtkomstkontroll](../active-directory/role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="9f049-156">To learn about setting security policies, see [Azure Role-based Access Control](../active-directory/role-based-access-control-configure.md).</span></span>
* <span data-ttu-id="9f049-157">Läs om kommandona för att visa distributionsåtgärder i [visa distributionsåtgärder](resource-manager-deployment-operations.md).</span><span class="sxs-lookup"><span data-stu-id="9f049-157">To learn about the commands for viewing deployment operations, see [View deployment operations](resource-manager-deployment-operations.md).</span></span>
* <span data-ttu-id="9f049-158">Information om hur du hindrar borttagningar på en resurs för alla användare finns [låsa resurser med Azure Resource Manager](resource-group-lock-resources.md).</span><span class="sxs-lookup"><span data-stu-id="9f049-158">To learn how to prevent deletions on a resource for all users, see [Lock resources with Azure Resource Manager](resource-group-lock-resources.md).</span></span>

