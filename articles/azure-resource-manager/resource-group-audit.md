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
# <a name="view-activity-logs-tooaudit-actions-on-resources"></a>Visa aktivitet loggar tooaudit åtgärder på resurser
Du kan bestämma via aktivitetsloggar:

* vilka åtgärder som utförts på hello resurser i din prenumeration
* vem som initierat hello åtgärden (även om en serverdelstjänst åtgärder inte returnerar en användare som hello anroparen)
* När hello åtgärden uppstod
* hello status för hello åtgärd
* hello värdena för andra egenskaper som kan hjälpa dig undersöka hello åtgärden

[!INCLUDE [resource-manager-audit-limitations](../../includes/resource-manager-audit-limitations.md)]

Du kan hämta information från hello aktivitetsloggar hello-portalen, PowerShell, Azure CLI, insikter REST API eller [insikter .NET-bibliotek](https://www.nuget.org/packages/Microsoft.Azure.Insights/).

## <a name="portal"></a>Portalen
1. tooview hello aktivitetsloggar hello-portalen, Välj **övervakaren**.
   
    ![Välj aktivitetsloggar](./media/resource-group-audit/select-monitor.png)

   Eller tooautomatically filter hello aktivitetsloggen för en viss resurs eller en resursgrupp, Välj **aktivitetsloggen** från den resursbladet. Observera att hello aktivitetsloggen filtreras automatiskt av hello markerad resurs.
   
    ![Filtrera efter resurs](./media/resource-group-audit/filtered-by-resource.png)
2. I hello **aktivitetsloggen** bladet visas en sammanfattning av nya åtgärder.
   
    ![Visa åtgärder](./media/resource-group-audit/audit-summary.png)
3. toorestrict hello antal åtgärder som visas, Välj olika villkor. Hello följande bild visar exempelvis hello **Timespan** och **händelse som initieras av** fält ändras tooview hello åtgärder som vidtas av en viss användare eller ett program för hello senaste månaden. Välj **tillämpa** tooview hello resultatet av frågan.
   
    ![Ange alternativ för filter](./media/resource-group-audit/set-filter.png)

4. Om du behöver toorun hello fråga igen senare, Välj **spara** och namnge hello frågan.
   
    ![Spara frågan](./media/resource-group-audit/save-query.png)
5. tooquickly köra en fråga, du kan välja något av hello inbyggda frågor, till exempel inte kunde distribueras.

    ![Välj fråga](./media/resource-group-audit/select-quick-query.png)

   hello valda frågan anger automatiskt hello krävs filtervärden.

    ![Visa distributionsfel](./media/resource-group-audit/view-failed-deployment.png)   

6. Välj en av hello operations toosee en sammanfattning av hello-händelse.

    ![Visa åtgärden](./media/resource-group-audit/view-operation.png)  

## <a name="powershell"></a>PowerShell
1. tooretrieve loggposter kör hello **Get-AzureRmLog** kommando. Du kan ange ytterligare parametrar toofilter hello listan med poster. Om du inte anger en start- och Slutdatum tid returneras poster för hello senaste timmen. Till exempel köra tooretrieve hello åtgärder för en resursgrupp under hello senaste timmen

  ```powershell
  Get-AzureRmLog -ResourceGroup ExampleGroup
  ```
   
    hello som följande exempel visar hur toouse hello aktivitet logga tooresearch åtgärder som vidtagits under en angiven tid. hello har start- och slutdatum angetts i ett datumformat.

  ```powershell
  Get-AzureRmLog -ResourceGroup ExampleGroup -StartTime 2015-08-28T06:00 -EndTime 2015-09-10T06:00
  ```

    Eller så kan du använda funktioner toospecify hello datum datumintervall, till exempel hello senaste 14 dagarna.
   
  ```powershell 
  Get-AzureRmLog -ResourceGroup ExampleGroup -StartTime (Get-Date).AddDays(-14)
  ```

2. Beroende på hello starttid som du anger, kan hello tidigare kommandon returnera en lång lista med åtgärder för hello resursgruppen. Du kan filtrera hello resultat för det du söker genom att ange sökvillkor. Om du försöker tooresearch hur en webbapp har stoppats, kan du köra hello följande kommando:

  ```powershell
  Get-AzureRmLog -ResourceGroup ExampleGroup -StartTime (Get-Date).AddDays(-14) | Where-Object OperationName -eq Microsoft.Web/sites/stop/action
  ```

    Vilket det här exemplet visar att en stoppåtgärden utfördes av someone@contoso.com. 

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

3. Du kan slå upp hello-åtgärder som vidtas av en viss användare, även för en resursgrupp som inte längre finns.

  ```powershell 
  Get-AzureRmLog -ResourceGroup deletedgroup -StartTime (Get-Date).AddDays(-14) -Caller someone@contoso.com
  ```

4. Du kan filtrera efter misslyckade åtgärder.

  ```powershell
  Get-AzureRmLog -ResourceGroup ExampleGroup -Status Failed
  ```

5. Du kan fokusera på ett fel genom att titta på hello statusmeddelande för posten.
   
        ((Get-AzureRmLog -Status Failed -ResourceGroup ExampleGroup -DetailedOutput).Properties[1].Content["statusMessage"] | ConvertFrom-Json).error
   
    Som returnerar:
   
        code           message                                                                        
        ----           -------                                                                        
        DnsRecordInUse DNS record dns.westus.cloudapp.azure.com is already used by another public IP. 


## <a name="azure-cli"></a>Azure CLI
* tooretrieve loggposter du kör hello **azure-grupp loggen visar** kommando.

  ```azurecli
  azure group log show ExampleGroup --json
  ```


## <a name="rest-api"></a>REST API
hello REST-åtgärder för att arbeta med hello aktivitetsloggen är en del av hello [insikter REST API](https://msdn.microsoft.com/library/azure/dn931943.aspx). logghändelser för tooretrieve aktivitet finns [hello management händelser i en prenumeration](https://msdn.microsoft.com/library/azure/dn931934.aspx).

## <a name="next-steps"></a>Nästa steg
* Azure aktivitetsloggar kan användas med Power BI toogain större insikter om hello åtgärder i din prenumeration. Se [visa och analysera Azure aktivitetsloggar i Power BI och mycket mer](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/).
* toolearn om att ställa in säkerhetsprinciper, se [Azure rollbaserad åtkomstkontroll](../active-directory/role-based-access-control-configure.md).
* toolearn om hello-kommandon för att visa distributionsåtgärder, se [visa distributionsåtgärder](resource-manager-deployment-operations.md).
* hur tooprevent borttagningar på en resurs för alla användare, se toolearn [låsa resurser med Azure Resource Manager](resource-group-lock-resources.md).

