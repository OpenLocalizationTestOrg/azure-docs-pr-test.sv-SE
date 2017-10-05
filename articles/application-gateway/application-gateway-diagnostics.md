---
title: "Övervaka åtkomstloggar, Prestandaloggar, backend-hälsa och mått för Programgateway | Microsoft Docs"
description: "Lär dig att aktivera och hantera åtkomstloggar och Prestandaloggar för Programgateway"
services: application-gateway
documentationcenter: na
author: amitsriva
manager: rossort
editor: tysonn
tags: azure-resource-manager
ms.assetid: 300628b8-8e3d-40ab-b294-3ecc5e48ef98
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/17/2017
ms.author: amitsriva
ms.openlocfilehash: 12c252340b82aba5ee69b12db83353750782e7c5
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="back-end-health-diagnostic-logs-and-metrics-for-application-gateway"></a><span data-ttu-id="cd367-103">Backend-hälsotillstånd, diagnostikloggar och mått för Programgateway</span><span class="sxs-lookup"><span data-stu-id="cd367-103">Back-end health, diagnostic logs, and metrics for Application Gateway</span></span>

<span data-ttu-id="cd367-104">Med hjälp av Azure Application Gateway kan övervaka du resurser på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="cd367-104">By using Azure Application Gateway, you can monitor resources in the following ways:</span></span>

* <span data-ttu-id="cd367-105">[Backend-hälsa](#back-end-health): Programgateway ger möjlighet att övervaka hälsotillståndet hos servrarna i backend-pooler via Azure-portalen och via PowerShell.</span><span class="sxs-lookup"><span data-stu-id="cd367-105">[Back-end health](#back-end-health): Application Gateway provides the capability to monitor the health of the servers in the back-end pools through the Azure portal and through PowerShell.</span></span> <span data-ttu-id="cd367-106">Du kan också hitta hälsotillståndet för backend-pooler via prestanda diagnostikloggar.</span><span class="sxs-lookup"><span data-stu-id="cd367-106">You can also find the health of the back-end pools through the performance diagnostic logs.</span></span>

* <span data-ttu-id="cd367-107">[Loggar](#diagnostic-logs): loggar Tillåt för prestanda, åtkomst och andra data sparas eller förbrukad från en resurs för övervakning.</span><span class="sxs-lookup"><span data-stu-id="cd367-107">[Logs](#diagnostic-logs): Logs allow for performance, access, and other data to be saved or consumed from a resource for monitoring purposes.</span></span>

* <span data-ttu-id="cd367-108">[Mått](#metrics): Application Gateway har för närvarande ett enskilt mått.</span><span class="sxs-lookup"><span data-stu-id="cd367-108">[Metrics](#metrics): Application Gateway currently has one metric.</span></span> <span data-ttu-id="cd367-109">Mätvärdet mäter genomflödet av Programgateway i byte per sekund.</span><span class="sxs-lookup"><span data-stu-id="cd367-109">This metric measures the throughput of the application gateway in bytes per second.</span></span>

## <a name="back-end-health"></a><span data-ttu-id="cd367-110">Backend-hälsa</span><span class="sxs-lookup"><span data-stu-id="cd367-110">Back-end health</span></span>

<span data-ttu-id="cd367-111">Programgateway ger möjlighet att övervaka hälsotillståndet för enskilda medlemmarna i backend-pooler via portalen, PowerShell och kommandoradsgränssnittet (CLI).</span><span class="sxs-lookup"><span data-stu-id="cd367-111">Application Gateway provides the capability to monitor the health of individual members of the back-end pools through the portal, PowerShell, and the command-line interface (CLI).</span></span> <span data-ttu-id="cd367-112">Du kan också hitta det totala hälsotillståndet för backend-pooler via prestanda diagnostikloggar.</span><span class="sxs-lookup"><span data-stu-id="cd367-112">You can also find an aggregated health summary of back-end pools through the performance diagnostic logs.</span></span> 

<span data-ttu-id="cd367-113">Backend-hälsorapporten visar utdata för Programgateway hälsoavsökningen till backend-instanser.</span><span class="sxs-lookup"><span data-stu-id="cd367-113">The back-end health report reflects the output of the Application Gateway health probe to the back-end instances.</span></span> <span data-ttu-id="cd367-114">När avsökning lyckas och serverdelen kan ta emot trafik, är det felfritt.</span><span class="sxs-lookup"><span data-stu-id="cd367-114">When probing is successful and the back end can receive traffic, it's considered healthy.</span></span> <span data-ttu-id="cd367-115">Annars anses vara felaktig.</span><span class="sxs-lookup"><span data-stu-id="cd367-115">Otherwise, it's considered unhealthy.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="cd367-116">Om det finns en nätverkssäkerhetsgrupp (NSG) på ett Application Gateway-undernät, öppna portintervall 65503 65534 i Application Gateway-undernät för inkommande trafik.</span><span class="sxs-lookup"><span data-stu-id="cd367-116">If there is a network security group (NSG) on an Application Gateway subnet, open port ranges 65503-65534 on the Application Gateway subnet for inbound traffic.</span></span> <span data-ttu-id="cd367-117">Dessa portar är obligatoriska för backend-hälsa API för att fungera.</span><span class="sxs-lookup"><span data-stu-id="cd367-117">These ports are required for the back-end health API to work.</span></span>


### <a name="view-back-end-health-through-the-portal"></a><span data-ttu-id="cd367-118">Visa backend-hälsa via portalen</span><span class="sxs-lookup"><span data-stu-id="cd367-118">View back-end health through the portal</span></span>

<span data-ttu-id="cd367-119">Backend-hälsa tillhandahålls automatiskt i portalen.</span><span class="sxs-lookup"><span data-stu-id="cd367-119">In the portal, back-end health is provided automatically.</span></span> <span data-ttu-id="cd367-120">Välj i en befintlig Programgateway **övervakning** > **Backend hälsa**.</span><span class="sxs-lookup"><span data-stu-id="cd367-120">In an existing application gateway, select **Monitoring** > **Backend health**.</span></span> 

<span data-ttu-id="cd367-121">Varje medlem i backend-poolen finns på den här sidan (om det är ett nätverkskort, IP eller FQDN).</span><span class="sxs-lookup"><span data-stu-id="cd367-121">Each member in the back-end pool is listed on this page (whether it's a NIC, IP, or FQDN).</span></span> <span data-ttu-id="cd367-122">Backend-namn, port, backend-HTTP-namn och hälsostatus visas.</span><span class="sxs-lookup"><span data-stu-id="cd367-122">Back-end pool name, port, back-end HTTP settings name, and health status are shown.</span></span> <span data-ttu-id="cd367-123">Giltiga värden för hälsostatus **felfri**, **ohälsosam**, och **okänd**.</span><span class="sxs-lookup"><span data-stu-id="cd367-123">Valid values for health status are **Healthy**, **Unhealthy**, and **Unknown**.</span></span>

> [!NOTE]
> <span data-ttu-id="cd367-124">Om du ser en backend-hälsostatus **okänd**, med åtkomst till serverdelen inte blockeras av en regel för NSG, en användardefinierad väg (UDR) eller en anpassad DNS i det virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="cd367-124">If you see a back-end health status of **Unknown**, ensure that access to the back end is not blocked by an NSG rule, a user-defined route (UDR), or a custom DNS in the virtual network.</span></span>

![Backend-hälsa][10]

### <a name="view-back-end-health-through-powershell"></a><span data-ttu-id="cd367-126">Visa backend-hälsa via PowerShell</span><span class="sxs-lookup"><span data-stu-id="cd367-126">View back-end health through PowerShell</span></span>

<span data-ttu-id="cd367-127">PowerShell följande kod visar hur du visar backend-hälsa genom att använda den `Get-AzureRmApplicationGatewayBackendHealth` cmdlet:</span><span class="sxs-lookup"><span data-stu-id="cd367-127">The following PowerShell code shows how to view back-end health by using the `Get-AzureRmApplicationGatewayBackendHealth` cmdlet:</span></span>

```powershell
Get-AzureRmApplicationGatewayBackendHealth -Name ApplicationGateway1 -ResourceGroupName Contoso
```

### <a name="view-back-end-health-through-azure-cli-20"></a><span data-ttu-id="cd367-128">Visa backend-hälsa genom Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="cd367-128">View back-end health through Azure CLI 2.0</span></span>

```azurecli
az network application-gateway show-backend-health --resource-group AdatumAppGatewayRG --name AdatumAppGateway
```

### <a name="results"></a><span data-ttu-id="cd367-129">Resultat</span><span class="sxs-lookup"><span data-stu-id="cd367-129">Results</span></span>

<span data-ttu-id="cd367-130">Följande utdrag visar ett exempel på svaret:</span><span class="sxs-lookup"><span data-stu-id="cd367-130">The following snippet shows an example of the response:</span></span>

```json
{
"BackendAddressPool": {
    "Id": "/subscriptions/00000000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendAddressPools/appGatewayBackendPool"
},
"BackendHttpSettingsCollection": [
    {
    "BackendHttpSettings": {
        "Id": "/00000000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendHttpSettingsCollection/appGatewayBackendHttpSettings"
    },
    "Servers": [
        {
        "Address": "hostname.westus.cloudapp.azure.com",
        "Health": "Healthy"
        },
        {
        "Address": "hostname.westus.cloudapp.azure.com",
        "Health": "Healthy"
        }
    ]
    }
]
}
```

## <span data-ttu-id="cd367-131"><a name="diagnostic-logging"></a>Diagnostikloggar</span><span class="sxs-lookup"><span data-stu-id="cd367-131"><a name="diagnostic-logging"></a>Diagnostic logs</span></span>

<span data-ttu-id="cd367-132">Du kan använda olika typer av loggar i Azure för att hantera och felsöka programgatewayer.</span><span class="sxs-lookup"><span data-stu-id="cd367-132">You can use different types of logs in Azure to manage and troubleshoot application gateways.</span></span> <span data-ttu-id="cd367-133">Du kan komma åt vissa av dessa loggar via portalen.</span><span class="sxs-lookup"><span data-stu-id="cd367-133">You can access some of these logs through the portal.</span></span> <span data-ttu-id="cd367-134">Alla loggar kan extraheras från Azure Blob storage och visas i olika verktyg som [logganalys](../log-analytics/log-analytics-azure-networking-analytics.md), Excel och Power BI.</span><span class="sxs-lookup"><span data-stu-id="cd367-134">All logs can be extracted from Azure Blob storage and viewed in different tools, such as [Log Analytics](../log-analytics/log-analytics-azure-networking-analytics.md), Excel, and Power BI.</span></span> <span data-ttu-id="cd367-135">Du kan lära dig mer om de olika typerna av loggar från följande lista:</span><span class="sxs-lookup"><span data-stu-id="cd367-135">You can learn more about the different types of logs from the following list:</span></span>

* <span data-ttu-id="cd367-136">**Aktivitetsloggen**: du kan använda [Azure aktivitetsloggar](../monitoring-and-diagnostics/insights-debugging-with-events.md) (hette operativa loggar och granskningsloggar) att visa alla åtgärder som skickas till din Azure-prenumeration och deras status.</span><span class="sxs-lookup"><span data-stu-id="cd367-136">**Activity log**: You can use [Azure activity logs](../monitoring-and-diagnostics/insights-debugging-with-events.md) (formerly known as operational logs and audit logs) to view all operations that are submitted to your Azure subscription, and their status.</span></span> <span data-ttu-id="cd367-137">Aktiviteten loggposter som samlas in som standard och du kan visa dem i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="cd367-137">Activity log entries are collected by default, and you can view them in the Azure portal.</span></span>
* <span data-ttu-id="cd367-138">**Åtkomstlogg**: du kan använda den här loggen att visa Programgateway åtkomstmönster och analysera viktiga information, inklusive anroparens IP, begärd URL, svar svarstid, returkod och byte in och ut. En åtkomstlogg samlas in varje 300 sekunder.</span><span class="sxs-lookup"><span data-stu-id="cd367-138">**Access log**: You can use this log to view Application Gateway access patterns and analyze important information, including the caller's IP, requested URL, response latency, return code, and bytes in and out. An access log is collected every 300 seconds.</span></span> <span data-ttu-id="cd367-139">Den här loggfilen innehåller en post per instans av Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="cd367-139">This log contains one record per instance of Application Gateway.</span></span> <span data-ttu-id="cd367-140">Application Gateway-instans kan identifieras av instanceId-egenskapen.</span><span class="sxs-lookup"><span data-stu-id="cd367-140">The Application Gateway instance can be identified by the instanceId property.</span></span>
* <span data-ttu-id="cd367-141">**Prestanda loggen**: du kan använda den här loggen för att visa hur Programgateway instanser utför.</span><span class="sxs-lookup"><span data-stu-id="cd367-141">**Performance log**: You can use this log to view how Application Gateway instances are performing.</span></span> <span data-ttu-id="cd367-142">Den här loggfilen innehåller prestandainformation om för varje instans, inklusive totalt antal begäranden som betjänats, genomflöde i byte, totalt antal begäranden som betjänats antalet misslyckade begäranden och felfria och feltillstånd backend-instanser.</span><span class="sxs-lookup"><span data-stu-id="cd367-142">This log captures performance information for each instance, including total requests served, throughput in bytes, total requests served, failed request count, and healthy and unhealthy back-end instance count.</span></span> <span data-ttu-id="cd367-143">En prestandalogg som samlas in var 60: e sekund.</span><span class="sxs-lookup"><span data-stu-id="cd367-143">A performance log is collected every 60 seconds.</span></span>
* <span data-ttu-id="cd367-144">**Brandväggsloggen**: du kan använda den här loggfilen för att se begäranden som loggas via identifiering eller förhindra läget för en Programgateway som är konfigurerad med Brandvägg för webbaserade program.</span><span class="sxs-lookup"><span data-stu-id="cd367-144">**Firewall log**: You can use this log to view the requests that are logged through either detection or prevention mode of an application gateway that is configured with the web application firewall.</span></span>

> [!NOTE]
> <span data-ttu-id="cd367-145">Loggarna är bara tillgängligt för resurser som har distribuerats i Azure Resource Manager-distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="cd367-145">Logs are available only for resources deployed in the Azure Resource Manager deployment model.</span></span> <span data-ttu-id="cd367-146">Du kan inte använda loggar för resurser i den klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="cd367-146">You cannot use logs for resources in the classic deployment model.</span></span> <span data-ttu-id="cd367-147">En bättre förståelse av de två modellerna, finns det [förstå Resource Manager distribution och klassisk distribution](../azure-resource-manager/resource-manager-deployment-model.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="cd367-147">For a better understanding of the two models, see the [Understanding Resource Manager deployment and classic deployment](../azure-resource-manager/resource-manager-deployment-model.md) article.</span></span>

<span data-ttu-id="cd367-148">Det finns tre alternativ för att lagra dina loggar:</span><span class="sxs-lookup"><span data-stu-id="cd367-148">You have three options for storing your logs:</span></span>

* <span data-ttu-id="cd367-149">**Lagringskontot**: Storage-konton är bäst för loggar när loggar lagras under en längre tid och granskas vid behov.</span><span class="sxs-lookup"><span data-stu-id="cd367-149">**Storage account**: Storage accounts are best used for logs when logs are stored for a longer duration and reviewed when needed.</span></span>
* <span data-ttu-id="cd367-150">**Händelsehubbar**: händelsehubbar är ett bra alternativ för att integrera med andra säkerhetsinformation och händelsehantering (SEIM) verktyg för att få aviseringar för dina resurser.</span><span class="sxs-lookup"><span data-stu-id="cd367-150">**Event hubs**: Event hubs are a great option for integrating with other security information and event management (SEIM) tools to get alerts on your resources.</span></span>
* <span data-ttu-id="cd367-151">**Logga Analytics**: logganalys passar bäst för allmän övervakning i realtid för programmet eller tittar på trender.</span><span class="sxs-lookup"><span data-stu-id="cd367-151">**Log Analytics**: Log Analytics is best used for general real-time monitoring of your application or looking at trends.</span></span>

### <a name="enable-logging-through-powershell"></a><span data-ttu-id="cd367-152">Aktivera loggning med PowerShell</span><span class="sxs-lookup"><span data-stu-id="cd367-152">Enable logging through PowerShell</span></span>

<span data-ttu-id="cd367-153">Aktivitetsloggning aktiveras automatiskt för varje resurs för hanteraren för filserverresurser.</span><span class="sxs-lookup"><span data-stu-id="cd367-153">Activity logging is automatically enabled for every Resource Manager resource.</span></span> <span data-ttu-id="cd367-154">Du måste aktivera åtkomst och prestanda som loggning för att börja samla in data som är tillgängliga via dessa loggar.</span><span class="sxs-lookup"><span data-stu-id="cd367-154">You must enable access and performance logging to start collecting the data available through those logs.</span></span> <span data-ttu-id="cd367-155">Använd följande steg för att aktivera loggning:</span><span class="sxs-lookup"><span data-stu-id="cd367-155">To enable logging, use the following steps:</span></span>

1. <span data-ttu-id="cd367-156">Observera ditt lagringskonto resurs-ID, där loggdata lagras.</span><span class="sxs-lookup"><span data-stu-id="cd367-156">Note your storage account's resource ID, where the log data is stored.</span></span> <span data-ttu-id="cd367-157">Det här värdet är i formatet: /subscriptions/\<subscriptionId\>/resourceGroups/\<resursgruppnamn\>/providers/Microsoft.Storage/storageAccounts/\<lagringskontonamnet\>.</span><span class="sxs-lookup"><span data-stu-id="cd367-157">This value is of the form: /subscriptions/\<subscriptionId\>/resourceGroups/\<resource group name\>/providers/Microsoft.Storage/storageAccounts/\<storage account name\>.</span></span> <span data-ttu-id="cd367-158">Du kan använda storage-konto i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="cd367-158">You can use any storage account in your subscription.</span></span> <span data-ttu-id="cd367-159">Du kan använda Azure-portalen för att hitta den här informationen.</span><span class="sxs-lookup"><span data-stu-id="cd367-159">You can use the Azure portal to find this information.</span></span>

    ![Portal: resurs-ID för lagringskontot](./media/application-gateway-diagnostics/diagnostics1.png)

2. <span data-ttu-id="cd367-161">Observera din Programgateway resurs-ID som har aktiverats.</span><span class="sxs-lookup"><span data-stu-id="cd367-161">Note your application gateway's resource ID for which logging is enabled.</span></span> <span data-ttu-id="cd367-162">Det här värdet är i formatet: /subscriptions/\<subscriptionId\>/resourceGroups/\<resursgruppens namn\>/providers/Microsoft.Network/applicationGateways/\<gateway programnamn\>.</span><span class="sxs-lookup"><span data-stu-id="cd367-162">This value is of the form: /subscriptions/\<subscriptionId\>/resourceGroups/\<resource group name\>/providers/Microsoft.Network/applicationGateways/\<application gateway name\>.</span></span> <span data-ttu-id="cd367-163">Du kan använda portalen för att hitta den här informationen.</span><span class="sxs-lookup"><span data-stu-id="cd367-163">You can use the portal to find this information.</span></span>

    ![Portal: resurs-ID för Programgateway](./media/application-gateway-diagnostics/diagnostics2.png)

3. <span data-ttu-id="cd367-165">Aktivera diagnostikloggning med hjälp av följande PowerShell-cmdlet:</span><span class="sxs-lookup"><span data-stu-id="cd367-165">Enable diagnostic logging by using the following PowerShell cmdlet:</span></span>

    ```powershell
    Set-AzureRmDiagnosticSetting  -ResourceId /subscriptions/<subscriptionId>/resourceGroups/<resource group name>/providers/Microsoft.Network/applicationGateways/<application gateway name> -StorageAccountId /subscriptions/<subscriptionId>/resourceGroups/<resource group name>/providers/Microsoft.Storage/storageAccounts/<storage account name> -Enabled $true     
    ```
    
> [!TIP] 
><span data-ttu-id="cd367-166">Aktivitetsloggar kräver inte ett separat lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="cd367-166">Activity logs do not require a separate storage account.</span></span> <span data-ttu-id="cd367-167">Användningen av lagringsutrymme för åtkomst- och prestandaloggning debiteras tjänsten.</span><span class="sxs-lookup"><span data-stu-id="cd367-167">The use of storage for access and performance logging incurs service charges.</span></span>

### <a name="enable-logging-through-the-azure-portal"></a><span data-ttu-id="cd367-168">Aktivera loggning via Azure portal</span><span class="sxs-lookup"><span data-stu-id="cd367-168">Enable logging through the Azure portal</span></span>

1. <span data-ttu-id="cd367-169">Hitta din resurs i Azure-portalen och på **diagnostikloggar**.</span><span class="sxs-lookup"><span data-stu-id="cd367-169">In the Azure portal, find your resource and click **Diagnostic logs**.</span></span>

   <span data-ttu-id="cd367-170">För Programgateway är tre loggarna tillgängliga:</span><span class="sxs-lookup"><span data-stu-id="cd367-170">For Application Gateway, three logs are available:</span></span>

   * <span data-ttu-id="cd367-171">Åtkomstlogg</span><span class="sxs-lookup"><span data-stu-id="cd367-171">Access log</span></span>
   * <span data-ttu-id="cd367-172">Prestandalogg</span><span class="sxs-lookup"><span data-stu-id="cd367-172">Performance log</span></span>
   * <span data-ttu-id="cd367-173">Brandväggen logg</span><span class="sxs-lookup"><span data-stu-id="cd367-173">Firewall log</span></span>

2. <span data-ttu-id="cd367-174">Om du vill börja samla in data, klickar du på **aktivera diagnostiken**.</span><span class="sxs-lookup"><span data-stu-id="cd367-174">To start collecting data, click **Turn on diagnostics**.</span></span>

   ![Aktivera diagnostik][1]

3. <span data-ttu-id="cd367-176">Den **diagnostikinställningar** bladet innehåller inställningarna för diagnostiska loggar.</span><span class="sxs-lookup"><span data-stu-id="cd367-176">The **Diagnostics settings** blade provides the settings for the diagnostic logs.</span></span> <span data-ttu-id="cd367-177">I det här exemplet lagras logganalys loggarna.</span><span class="sxs-lookup"><span data-stu-id="cd367-177">In this example, Log Analytics stores the logs.</span></span> <span data-ttu-id="cd367-178">Klicka på **konfigurera** under **logganalys** att konfigurera din arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="cd367-178">Click **Configure** under **Log Analytics** to configure your workspace.</span></span> <span data-ttu-id="cd367-179">Du kan också använda händelsehubbar och ett lagringskonto för att spara diagnostikloggarna.</span><span class="sxs-lookup"><span data-stu-id="cd367-179">You can also use event hubs and a storage account to save the diagnostic logs.</span></span>

   ![Starta konfigurationen][2]

4. <span data-ttu-id="cd367-181">Välj en befintlig Operations Management Suite (OMS) arbetsyta eller skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="cd367-181">Choose an existing Operations Management Suite (OMS) workspace or create a new one.</span></span> <span data-ttu-id="cd367-182">Det här exemplet använder en befintlig.</span><span class="sxs-lookup"><span data-stu-id="cd367-182">This example uses an existing one.</span></span>

   ![Alternativ för OMS arbetsytor][3]

5. <span data-ttu-id="cd367-184">Bekräfta inställningarna och klicka på **spara**.</span><span class="sxs-lookup"><span data-stu-id="cd367-184">Confirm the settings and click **Save**.</span></span>

   ![Diagnostik inställningsbladet med val][4]

### <a name="activity-log"></a><span data-ttu-id="cd367-186">Aktivitetslogg</span><span class="sxs-lookup"><span data-stu-id="cd367-186">Activity log</span></span>

<span data-ttu-id="cd367-187">Azure genererar aktivitetsloggen som standard.</span><span class="sxs-lookup"><span data-stu-id="cd367-187">Azure generates the activity log by default.</span></span> <span data-ttu-id="cd367-188">Loggarna finns kvar i 90 dagar i arkivet Azure händelseloggar.</span><span class="sxs-lookup"><span data-stu-id="cd367-188">The logs are preserved for 90 days in the Azure event logs store.</span></span> <span data-ttu-id="cd367-189">Mer information om de här loggarna genom att läsa den [visa händelser och aktivitetsloggen](../monitoring-and-diagnostics/insights-debugging-with-events.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="cd367-189">Learn more about these logs by reading the [View events and activity log](../monitoring-and-diagnostics/insights-debugging-with-events.md) article.</span></span>

### <a name="access-log"></a><span data-ttu-id="cd367-190">Åtkomstlogg</span><span class="sxs-lookup"><span data-stu-id="cd367-190">Access log</span></span>

<span data-ttu-id="cd367-191">Åtkomst-loggen genereras bara om du har aktiverat på varje Application Gateway-instans enligt anvisningarna i föregående steg.</span><span class="sxs-lookup"><span data-stu-id="cd367-191">The access log is generated only if you've enabled it on each Application Gateway instance, as detailed in the preceding steps.</span></span> <span data-ttu-id="cd367-192">Data lagras i storage-konto som du angav när du har aktiverat loggning.</span><span class="sxs-lookup"><span data-stu-id="cd367-192">The data is stored in the storage account that you specified when you enabled the logging.</span></span> <span data-ttu-id="cd367-193">Varje åtkomst för Programgateway loggas i JSON-format, som visas i följande exempel:</span><span class="sxs-lookup"><span data-stu-id="cd367-193">Each access of Application Gateway is logged in JSON format, as shown in the following example:</span></span>


|<span data-ttu-id="cd367-194">Värde</span><span class="sxs-lookup"><span data-stu-id="cd367-194">Value</span></span>  |<span data-ttu-id="cd367-195">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="cd367-195">Description</span></span>  |
|---------|---------|
|<span data-ttu-id="cd367-196">InstanceId</span><span class="sxs-lookup"><span data-stu-id="cd367-196">instanceId</span></span>     | <span data-ttu-id="cd367-197">Programmet Gateway-instans där begäran behandlades.</span><span class="sxs-lookup"><span data-stu-id="cd367-197">Application Gateway instance that served the request.</span></span>        |
|<span data-ttu-id="cd367-198">ClientIP</span><span class="sxs-lookup"><span data-stu-id="cd367-198">clientIP</span></span>     | <span data-ttu-id="cd367-199">Ursprungliga IP-Adressen för begäran.</span><span class="sxs-lookup"><span data-stu-id="cd367-199">Originating IP for the request.</span></span>        |
|<span data-ttu-id="cd367-200">clientPort</span><span class="sxs-lookup"><span data-stu-id="cd367-200">clientPort</span></span>     | <span data-ttu-id="cd367-201">Ursprungliga port för begäran.</span><span class="sxs-lookup"><span data-stu-id="cd367-201">Originating port for the request.</span></span>       |
|<span data-ttu-id="cd367-202">HttpMethod</span><span class="sxs-lookup"><span data-stu-id="cd367-202">httpMethod</span></span>     | <span data-ttu-id="cd367-203">HTTP-metod som används av begäran.</span><span class="sxs-lookup"><span data-stu-id="cd367-203">HTTP method used by the request.</span></span>       |
|<span data-ttu-id="cd367-204">requestUri</span><span class="sxs-lookup"><span data-stu-id="cd367-204">requestUri</span></span>     | <span data-ttu-id="cd367-205">URI för tog emot begäran.</span><span class="sxs-lookup"><span data-stu-id="cd367-205">URI of the received request.</span></span>        |
|<span data-ttu-id="cd367-206">RequestQuery</span><span class="sxs-lookup"><span data-stu-id="cd367-206">RequestQuery</span></span>     | <span data-ttu-id="cd367-207">**Server-dirigerat**: backend-pool-instans som begäran skickades.</span><span class="sxs-lookup"><span data-stu-id="cd367-207">**Server-Routed**: Back-end pool instance that was sent the request.</span></span> </br> <span data-ttu-id="cd367-208">**X-AzureApplicationGateway-LOG-ID**: Korrelations-ID som används för begäran.</span><span class="sxs-lookup"><span data-stu-id="cd367-208">**X-AzureApplicationGateway-LOG-ID**: Correlation ID used for the request.</span></span> <span data-ttu-id="cd367-209">Det kan användas för att felsöka problem med trafik på backend-servrar.</span><span class="sxs-lookup"><span data-stu-id="cd367-209">It can be used to troubleshoot traffic issues on the back-end servers.</span></span> </br><span data-ttu-id="cd367-210">**SERVER-STATUS**: HTTP-svarskoden Programgateway togs emot från serverdelen.</span><span class="sxs-lookup"><span data-stu-id="cd367-210">**SERVER-STATUS**: HTTP response code that Application Gateway received from the back end.</span></span>       |
|<span data-ttu-id="cd367-211">UserAgent</span><span class="sxs-lookup"><span data-stu-id="cd367-211">UserAgent</span></span>     | <span data-ttu-id="cd367-212">Användaragent från HTTP-huvudet i begäran.</span><span class="sxs-lookup"><span data-stu-id="cd367-212">User agent from the HTTP request header.</span></span>        |
|<span data-ttu-id="cd367-213">httpStatus</span><span class="sxs-lookup"><span data-stu-id="cd367-213">httpStatus</span></span>     | <span data-ttu-id="cd367-214">HTTP-statuskod som returneras till klienten från Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="cd367-214">HTTP status code returned to the client from Application Gateway.</span></span>       |
|<span data-ttu-id="cd367-215">httpVersion</span><span class="sxs-lookup"><span data-stu-id="cd367-215">httpVersion</span></span>     | <span data-ttu-id="cd367-216">HTTP-version för begäran.</span><span class="sxs-lookup"><span data-stu-id="cd367-216">HTTP version of the request.</span></span>        |
|<span data-ttu-id="cd367-217">receivedBytes</span><span class="sxs-lookup"><span data-stu-id="cd367-217">receivedBytes</span></span>     | <span data-ttu-id="cd367-218">Storleken på paketet tas emot, i byte.</span><span class="sxs-lookup"><span data-stu-id="cd367-218">Size of packet received, in bytes.</span></span>        |
|<span data-ttu-id="cd367-219">sentBytes</span><span class="sxs-lookup"><span data-stu-id="cd367-219">sentBytes</span></span>| <span data-ttu-id="cd367-220">Storleken på paketet skickas, i byte.</span><span class="sxs-lookup"><span data-stu-id="cd367-220">Size of packet sent, in bytes.</span></span>|
|<span data-ttu-id="cd367-221">timeTaken</span><span class="sxs-lookup"><span data-stu-id="cd367-221">timeTaken</span></span>| <span data-ttu-id="cd367-222">Lång tid (i millisekunder) som det tar för en begäran att bearbetas och dess svar skickas.</span><span class="sxs-lookup"><span data-stu-id="cd367-222">Length of time (in milliseconds) that it takes for a request to be processed and its response to be sent.</span></span> <span data-ttu-id="cd367-223">Det här beräknas som intervall från den tid när Programgateway tar emot de första byten för en HTTP-begäran mot den tid när åtgärden är slutförd skickar i svaret.</span><span class="sxs-lookup"><span data-stu-id="cd367-223">This is calculated as the interval from the time when Application Gateway receives the first byte of an HTTP request to the time when the response send operation finishes.</span></span> <span data-ttu-id="cd367-224">Det är viktigt att Observera att fältet Time-Taken vanligtvis innehåller den tid som begäran och svar paket skickas över nätverk.</span><span class="sxs-lookup"><span data-stu-id="cd367-224">It's important to note that the Time-Taken field usually includes the time that the request and response packets are traveling over the network.</span></span> |
|<span data-ttu-id="cd367-225">sslEnabled</span><span class="sxs-lookup"><span data-stu-id="cd367-225">sslEnabled</span></span>| <span data-ttu-id="cd367-226">Om kommunikationen till backend-pooler används SSL.</span><span class="sxs-lookup"><span data-stu-id="cd367-226">Whether communication to the back-end pools used SSL.</span></span> <span data-ttu-id="cd367-227">Giltiga värden är på och av.</span><span class="sxs-lookup"><span data-stu-id="cd367-227">Valid values are on and off.</span></span>|
```json
{
    "resourceId": "/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/PEERINGTEST/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/{applicationGatewayName}",
    "operationName": "ApplicationGatewayAccess",
    "time": "2017-04-26T19:27:38Z",
    "category": "ApplicationGatewayAccessLog",
    "properties": {
        "instanceId": "ApplicationGatewayRole_IN_0",
        "clientIP": "191.96.249.97",
        "clientPort": 46886,
        "httpMethod": "GET",
        "requestUri": "/phpmyadmin/scripts/setup.php",
        "requestQuery": "X-AzureApplicationGateway-CACHE-HIT=0&SERVER-ROUTED=10.4.0.4&X-AzureApplicationGateway-LOG-ID=874f1f0f-6807-41c9-b7bc-f3cfa74aa0b1&SERVER-STATUS=404",
        "userAgent": "-",
        "httpStatus": 404,
        "httpVersion": "HTTP/1.0",
        "receivedBytes": 65,
        "sentBytes": 553,
        "timeTaken": 205,
        "sslEnabled": "off"
    }
}
```

### <a name="performance-log"></a><span data-ttu-id="cd367-228">Prestandalogg</span><span class="sxs-lookup"><span data-stu-id="cd367-228">Performance log</span></span>

<span data-ttu-id="cd367-229">Prestanda-loggen genereras bara om du har aktiverat på varje Application Gateway-instans enligt anvisningarna i föregående steg.</span><span class="sxs-lookup"><span data-stu-id="cd367-229">The performance log is generated only if you have enabled it on each Application Gateway instance, as detailed in the preceding steps.</span></span> <span data-ttu-id="cd367-230">Data lagras i storage-konto som du angav när du har aktiverat loggning.</span><span class="sxs-lookup"><span data-stu-id="cd367-230">The data is stored in the storage account that you specified when you enabled the logging.</span></span> <span data-ttu-id="cd367-231">Loggdata för prestanda genereras på 1 minut.</span><span class="sxs-lookup"><span data-stu-id="cd367-231">The performance log data is generated in 1-minute intervals.</span></span> <span data-ttu-id="cd367-232">Följande data loggas:</span><span class="sxs-lookup"><span data-stu-id="cd367-232">The following data is logged:</span></span>


|<span data-ttu-id="cd367-233">Värde</span><span class="sxs-lookup"><span data-stu-id="cd367-233">Value</span></span>  |<span data-ttu-id="cd367-234">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="cd367-234">Description</span></span>  |
|---------|---------|
|<span data-ttu-id="cd367-235">InstanceId</span><span class="sxs-lookup"><span data-stu-id="cd367-235">instanceId</span></span>     |  <span data-ttu-id="cd367-236">Programmet Gateway-instans för vilka data som genereras.</span><span class="sxs-lookup"><span data-stu-id="cd367-236">Application Gateway instance for which performance data is being generated.</span></span> <span data-ttu-id="cd367-237">För en gateway med flera instans programmet finns en rad per instans.</span><span class="sxs-lookup"><span data-stu-id="cd367-237">For a multiple-instance application gateway, there is one row per instance.</span></span>        |
|<span data-ttu-id="cd367-238">healthyHostCount</span><span class="sxs-lookup"><span data-stu-id="cd367-238">healthyHostCount</span></span>     | <span data-ttu-id="cd367-239">Antalet felfri värdar i backend-poolen.</span><span class="sxs-lookup"><span data-stu-id="cd367-239">Number of healthy hosts in the back-end pool.</span></span>        |
|<span data-ttu-id="cd367-240">unHealthyHostCount</span><span class="sxs-lookup"><span data-stu-id="cd367-240">unHealthyHostCount</span></span>     | <span data-ttu-id="cd367-241">Antalet felaktiga värdar i backend-poolen.</span><span class="sxs-lookup"><span data-stu-id="cd367-241">Number of unhealthy hosts in the back-end pool.</span></span>        |
|<span data-ttu-id="cd367-242">RequestCount</span><span class="sxs-lookup"><span data-stu-id="cd367-242">requestCount</span></span>     | <span data-ttu-id="cd367-243">Antal begäranden som betjänats.</span><span class="sxs-lookup"><span data-stu-id="cd367-243">Number of requests served.</span></span>        |
|<span data-ttu-id="cd367-244">Svarstid</span><span class="sxs-lookup"><span data-stu-id="cd367-244">latency</span></span> | <span data-ttu-id="cd367-245">Svarstid (i millisekunder) för förfrågningar från instansen till serverdelen som hanterar begäranden.</span><span class="sxs-lookup"><span data-stu-id="cd367-245">Latency (in milliseconds) of requests from the instance to the back end that serves the requests.</span></span> |
|<span data-ttu-id="cd367-246">failedRequestCount</span><span class="sxs-lookup"><span data-stu-id="cd367-246">failedRequestCount</span></span>| <span data-ttu-id="cd367-247">Antal misslyckade begäranden.</span><span class="sxs-lookup"><span data-stu-id="cd367-247">Number of failed requests.</span></span>|
|<span data-ttu-id="cd367-248">Dataflöde</span><span class="sxs-lookup"><span data-stu-id="cd367-248">throughput</span></span>| <span data-ttu-id="cd367-249">Genomsnittlig genomströmning sedan senaste loggen, mätt i byte per sekund.</span><span class="sxs-lookup"><span data-stu-id="cd367-249">Average throughput since the last log, measured in bytes per second.</span></span>|

```json
{
    "resourceId": "/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/{applicationGatewayName}",
    "operationName": "ApplicationGatewayPerformance",
    "time": "2016-04-09T00:00:00Z",
    "category": "ApplicationGatewayPerformanceLog",
    "properties":
    {
        "instanceId":"ApplicationGatewayRole_IN_1",
        "healthyHostCount":"4",
        "unHealthyHostCount":"0",
        "requestCount":"185",
        "latency":"0",
        "failedRequestCount":"0",
        "throughput":"119427"
    }
}
```

> [!NOTE]
> <span data-ttu-id="cd367-250">Svarstiden är beräknad när den första byten i HTTP-begäran tas emot med tiden då den sista byten av HTTP-svar skickas.</span><span class="sxs-lookup"><span data-stu-id="cd367-250">Latency is calculated from the time when the first byte of the HTTP request is received to the time when the last byte of the HTTP response is sent.</span></span> <span data-ttu-id="cd367-251">Det är summan av bearbetningstid Programgateway plus nätverkskostnad för serverdelen plus den tid som serverdelen som krävs för att bearbeta begäran.</span><span class="sxs-lookup"><span data-stu-id="cd367-251">It's the sum of the Application Gateway processing time plus the network cost to the back end, plus the time that the back end takes to process the request.</span></span>

### <a name="firewall-log"></a><span data-ttu-id="cd367-252">Brandväggen logg</span><span class="sxs-lookup"><span data-stu-id="cd367-252">Firewall log</span></span>

<span data-ttu-id="cd367-253">Brandväggsloggen genereras bara om du har aktiverat för varje Programgateway enligt anvisningarna i föregående steg.</span><span class="sxs-lookup"><span data-stu-id="cd367-253">The firewall log is generated only if you have enabled it for each application gateway, as detailed in the preceding steps.</span></span> <span data-ttu-id="cd367-254">Den här loggfilen kräver också att Brandvägg för webbaserade program har konfigurerats på en Programgateway.</span><span class="sxs-lookup"><span data-stu-id="cd367-254">This log also requires that the web application firewall is configured on an application gateway.</span></span> <span data-ttu-id="cd367-255">Data lagras i storage-konto som du angav när du har aktiverat loggning.</span><span class="sxs-lookup"><span data-stu-id="cd367-255">The data is stored in the storage account that you specified when you enabled the logging.</span></span> <span data-ttu-id="cd367-256">Följande data loggas:</span><span class="sxs-lookup"><span data-stu-id="cd367-256">The following data is logged:</span></span>


|<span data-ttu-id="cd367-257">Värde</span><span class="sxs-lookup"><span data-stu-id="cd367-257">Value</span></span>  |<span data-ttu-id="cd367-258">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="cd367-258">Description</span></span>  |
|---------|---------|
|<span data-ttu-id="cd367-259">InstanceId</span><span class="sxs-lookup"><span data-stu-id="cd367-259">instanceId</span></span>     | <span data-ttu-id="cd367-260">Programmet Gateway-instans för vilken brandvägg genereras data.</span><span class="sxs-lookup"><span data-stu-id="cd367-260">Application Gateway instance for which firewall data is being generated.</span></span> <span data-ttu-id="cd367-261">För en gateway med flera instans programmet finns en rad per instans.</span><span class="sxs-lookup"><span data-stu-id="cd367-261">For a multiple-instance application gateway, there is one row per instance.</span></span>         |
|<span data-ttu-id="cd367-262">clientIp</span><span class="sxs-lookup"><span data-stu-id="cd367-262">clientIp</span></span>     |   <span data-ttu-id="cd367-263">Ursprungliga IP-Adressen för begäran.</span><span class="sxs-lookup"><span data-stu-id="cd367-263">Originating IP for the request.</span></span>      |
|<span data-ttu-id="cd367-264">clientPort</span><span class="sxs-lookup"><span data-stu-id="cd367-264">clientPort</span></span>     |  <span data-ttu-id="cd367-265">Ursprungliga port för begäran.</span><span class="sxs-lookup"><span data-stu-id="cd367-265">Originating port for the request.</span></span>       |
|<span data-ttu-id="cd367-266">requestUri</span><span class="sxs-lookup"><span data-stu-id="cd367-266">requestUri</span></span>     | <span data-ttu-id="cd367-267">URL till tog emot begäran.</span><span class="sxs-lookup"><span data-stu-id="cd367-267">URL of the received request.</span></span>       |
|<span data-ttu-id="cd367-268">ruleSetType</span><span class="sxs-lookup"><span data-stu-id="cd367-268">ruleSetType</span></span>     | <span data-ttu-id="cd367-269">Typen regeluppsättning.</span><span class="sxs-lookup"><span data-stu-id="cd367-269">Rule set type.</span></span> <span data-ttu-id="cd367-270">Tillgängliga värdet är OWASP.</span><span class="sxs-lookup"><span data-stu-id="cd367-270">The available value is OWASP.</span></span>        |
|<span data-ttu-id="cd367-271">ruleSetVersion</span><span class="sxs-lookup"><span data-stu-id="cd367-271">ruleSetVersion</span></span>     | <span data-ttu-id="cd367-272">Regeluppsättning version som används.</span><span class="sxs-lookup"><span data-stu-id="cd367-272">Rule set version used.</span></span> <span data-ttu-id="cd367-273">Tillgängliga värden är 2.2.9 och 3.0.</span><span class="sxs-lookup"><span data-stu-id="cd367-273">Available values are 2.2.9 and 3.0.</span></span>     |
|<span data-ttu-id="cd367-274">ruleId</span><span class="sxs-lookup"><span data-stu-id="cd367-274">ruleId</span></span>     | <span data-ttu-id="cd367-275">Regel-ID för den utlösande händelsen.</span><span class="sxs-lookup"><span data-stu-id="cd367-275">Rule ID of the triggering event.</span></span>        |
|<span data-ttu-id="cd367-276">Meddelande</span><span class="sxs-lookup"><span data-stu-id="cd367-276">message</span></span>     | <span data-ttu-id="cd367-277">Användarvänligt meddelande för utlösande händelsen.</span><span class="sxs-lookup"><span data-stu-id="cd367-277">User-friendly message for the triggering event.</span></span> <span data-ttu-id="cd367-278">Mer information finns i informationsavsnittet.</span><span class="sxs-lookup"><span data-stu-id="cd367-278">More details are provided in the details section.</span></span>        |
|<span data-ttu-id="cd367-279">Åtgärden</span><span class="sxs-lookup"><span data-stu-id="cd367-279">action</span></span>     |  <span data-ttu-id="cd367-280">Åtgärder som vidtas på begäran.</span><span class="sxs-lookup"><span data-stu-id="cd367-280">Action taken on the request.</span></span> <span data-ttu-id="cd367-281">Tillgängliga värden är blockerad och tillåtna.</span><span class="sxs-lookup"><span data-stu-id="cd367-281">Available values are Blocked and Allowed.</span></span>      |
|<span data-ttu-id="cd367-282">plats</span><span class="sxs-lookup"><span data-stu-id="cd367-282">site</span></span>     | <span data-ttu-id="cd367-283">Plats för vilken loggen skapades.</span><span class="sxs-lookup"><span data-stu-id="cd367-283">Site for which the log was generated.</span></span> <span data-ttu-id="cd367-284">Endast globala visas för närvarande eftersom regler är globala.</span><span class="sxs-lookup"><span data-stu-id="cd367-284">Currently, only Global is listed because rules are global.</span></span>|
|<span data-ttu-id="cd367-285">Information</span><span class="sxs-lookup"><span data-stu-id="cd367-285">details</span></span>     | <span data-ttu-id="cd367-286">Information om den utlösande händelsen.</span><span class="sxs-lookup"><span data-stu-id="cd367-286">Details of the triggering event.</span></span>        |
|<span data-ttu-id="cd367-287">details.Message</span><span class="sxs-lookup"><span data-stu-id="cd367-287">details.message</span></span>     | <span data-ttu-id="cd367-288">Beskrivning av regeln.</span><span class="sxs-lookup"><span data-stu-id="cd367-288">Description of the rule.</span></span>        |
|<span data-ttu-id="cd367-289">details.data</span><span class="sxs-lookup"><span data-stu-id="cd367-289">details.data</span></span>     | <span data-ttu-id="cd367-290">Specifika data hittades i begäran som matchar regeln.</span><span class="sxs-lookup"><span data-stu-id="cd367-290">Specific data found in request that matched the rule.</span></span>         |
|<span data-ttu-id="cd367-291">details.File</span><span class="sxs-lookup"><span data-stu-id="cd367-291">details.file</span></span>     | <span data-ttu-id="cd367-292">Konfigurationsfil som innehåller regeln.</span><span class="sxs-lookup"><span data-stu-id="cd367-292">Configuration file that contained the rule.</span></span>        |
|<span data-ttu-id="cd367-293">details.Line</span><span class="sxs-lookup"><span data-stu-id="cd367-293">details.line</span></span>     | <span data-ttu-id="cd367-294">Radnumret i konfigurationsfilen som utlöste händelsen.</span><span class="sxs-lookup"><span data-stu-id="cd367-294">Line number in the configuration file that triggered the event.</span></span>       |

```json
{
  "resourceId": "/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/{applicationGatewayName}",
  "operationName": "ApplicationGatewayFirewall",
  "time": "2017-03-20T15:52:09.1494499Z",
  "category": "ApplicationGatewayFirewallLog",
  "properties": {
    "instanceId": "ApplicationGatewayRole_IN_0",
    "clientIp": "104.210.252.3",
    "clientPort": "4835",
    "requestUri": "/?a=%3Cscript%3Ealert(%22Hello%22);%3C/script%3E",
    "ruleSetType": "OWASP",
    "ruleSetVersion": "3.0",
    "ruleId": "941320",
    "message": "Possible XSS Attack Detected - HTML Tag Handler",
    "action": "Blocked",
    "site": "Global",
    "details": {
      "message": "Warning. Pattern match \"<(a|abbr|acronym|address|applet|area|audioscope|b|base|basefront|bdo|bgsound|big|blackface|blink|blockquote|body|bq|br|button|caption|center|cite|code|col|colgroup|comment|dd|del|dfn|dir|div|dl|dt|em|embed|fieldset|fn|font|form|frame|frameset|h1|head|h ...\" at ARGS:a.",
      "data": "Matched Data: <script> found within ARGS:a: <script>alert(\\x22hello\\x22);</script>",
      "file": "rules/REQUEST-941-APPLICATION-ATTACK-XSS.conf",
      "line": "865"
    }
  }
} 

```

### <a name="view-and-analyze-the-activity-log"></a><span data-ttu-id="cd367-295">Visa och analysera aktivitetsloggen</span><span class="sxs-lookup"><span data-stu-id="cd367-295">View and analyze the activity log</span></span>

<span data-ttu-id="cd367-296">Du kan visa och analysera loggdata för aktiviteten genom att använda någon av följande metoder:</span><span class="sxs-lookup"><span data-stu-id="cd367-296">You can view and analyze activity log data by using any of the following methods:</span></span>

* <span data-ttu-id="cd367-297">**Azure-verktyg**: hämta information från aktivitetsloggen via Azure PowerShell, Azure CLI, Azure REST API eller Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="cd367-297">**Azure tools**: Retrieve information from the activity log through Azure PowerShell, the Azure CLI, the Azure REST API, or the Azure portal.</span></span> <span data-ttu-id="cd367-298">Stegvisa instruktioner för varje metod beskrivs i den [aktivitet åtgärder med Resource Manager](../azure-resource-manager/resource-group-audit.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="cd367-298">Step-by-step instructions for each method are detailed in the [Activity operations with Resource Manager](../azure-resource-manager/resource-group-audit.md) article.</span></span>
* <span data-ttu-id="cd367-299">**Power BI**: Om du inte redan har en [Power BI](https://powerbi.microsoft.com/pricing) konto, du kan testa det gratis.</span><span class="sxs-lookup"><span data-stu-id="cd367-299">**Power BI**: If you don't already have a [Power BI](https://powerbi.microsoft.com/pricing) account, you can try it for free.</span></span> <span data-ttu-id="cd367-300">Med hjälp av den [Azure aktivitetsloggar content pack för Power BI](https://powerbi.microsoft.com/en-us/documentation/powerbi-content-pack-azure-audit-logs/), du kan analysera dina data med förkonfigurerade instrumentpaneler som du kan använda eller anpassa.</span><span class="sxs-lookup"><span data-stu-id="cd367-300">By using the [Azure Activity Logs content pack for Power BI](https://powerbi.microsoft.com/en-us/documentation/powerbi-content-pack-azure-audit-logs/), you can analyze your data with preconfigured dashboards that you can use as is or customize.</span></span>

### <a name="view-and-analyze-the-access-performance-and-firewall-logs"></a><span data-ttu-id="cd367-301">Visa och analysera åtkomst-, prestanda- och brandväggsloggar</span><span class="sxs-lookup"><span data-stu-id="cd367-301">View and analyze the access, performance, and firewall logs</span></span>

<span data-ttu-id="cd367-302">Azure [logganalys](../log-analytics/log-analytics-azure-networking-analytics.md) kan samla in prestandaräknare och händelseloggen filer från Blob storage-konto.</span><span class="sxs-lookup"><span data-stu-id="cd367-302">Azure [Log Analytics](../log-analytics/log-analytics-azure-networking-analytics.md) can collect the counter and event log files from your Blob storage account.</span></span> <span data-ttu-id="cd367-303">Det innehåller visualiseringar och avancerade sökfunktioner att analysera dina loggar.</span><span class="sxs-lookup"><span data-stu-id="cd367-303">It includes visualizations and powerful search capabilities to analyze your logs.</span></span>

<span data-ttu-id="cd367-304">Du kan också ansluta till ditt lagringskonto och hämta JSON-loggposter för åtkomst och Prestandaloggar.</span><span class="sxs-lookup"><span data-stu-id="cd367-304">You can also connect to your storage account and retrieve the JSON log entries for access and performance logs.</span></span> <span data-ttu-id="cd367-305">När du har hämtat JSON-filer kan du konvertera dem till CSV och visa dem i Excel, Power BI eller något annat verktyg för visualisering av data.</span><span class="sxs-lookup"><span data-stu-id="cd367-305">After you download the JSON files, you can convert them to CSV and view them in Excel, Power BI, or any other data-visualization tool.</span></span>

> [!TIP]
> <span data-ttu-id="cd367-306">Om du är bekant med Visual Studio och grundläggande begrepp för att ändra värdena för variabler i C# och konstanter, kan du använda den [logga konverteraren verktyg](https://github.com/Azure-Samples/networking-dotnet-log-converter) tillgängliga från GitHub.</span><span class="sxs-lookup"><span data-stu-id="cd367-306">If you are familiar with Visual Studio and basic concepts of changing values for constants and variables in C#, you can use the [log converter tools](https://github.com/Azure-Samples/networking-dotnet-log-converter) available from GitHub.</span></span>
> 
> 

## <a name="metrics"></a><span data-ttu-id="cd367-307">Mått</span><span class="sxs-lookup"><span data-stu-id="cd367-307">Metrics</span></span>

<span data-ttu-id="cd367-308">Mått är en funktion för vissa Azure-resurser där du kan visa prestandaräknare i portalen.</span><span class="sxs-lookup"><span data-stu-id="cd367-308">Metrics are a feature for certain Azure resources where you can view performance counters in the portal.</span></span> <span data-ttu-id="cd367-309">För Programgateway är ett enskilt mått tillgängligt nu.</span><span class="sxs-lookup"><span data-stu-id="cd367-309">For Application Gateway, one metric is available now.</span></span> <span data-ttu-id="cd367-310">Det här måttet är genomflöde och du kan se den i portalen.</span><span class="sxs-lookup"><span data-stu-id="cd367-310">This metric is throughput, and you can see it in the portal.</span></span> <span data-ttu-id="cd367-311">Bläddra till en Programgateway och på **mått**.</span><span class="sxs-lookup"><span data-stu-id="cd367-311">Browse to an application gateway and click **Metrics**.</span></span> <span data-ttu-id="cd367-312">Välj om du vill visa värden genomflöde i den **tillgängliga mått** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="cd367-312">To view the values, select throughput in the **Available metrics** section.</span></span> <span data-ttu-id="cd367-313">I följande bild kan se du ett exempel med de filter som du kan använda för att visa data i olika tidsintervall.</span><span class="sxs-lookup"><span data-stu-id="cd367-313">In the following image, you can see an example with the filters that you can use to display the data in different time ranges.</span></span>

![Måttvy med filter][5]

<span data-ttu-id="cd367-315">Om du vill se en lista över mått finns [stöds mått med Azure-Monitor](../monitoring-and-diagnostics/monitoring-supported-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="cd367-315">To see a current list of metrics, see [Supported metrics with Azure Monitor](../monitoring-and-diagnostics/monitoring-supported-metrics.md).</span></span>

### <a name="alert-rules"></a><span data-ttu-id="cd367-316">Aviseringsregler</span><span class="sxs-lookup"><span data-stu-id="cd367-316">Alert rules</span></span>

<span data-ttu-id="cd367-317">Du kan starta Varningsregler baserat på mått för en resurs.</span><span class="sxs-lookup"><span data-stu-id="cd367-317">You can start alert rules based on metrics for a resource.</span></span> <span data-ttu-id="cd367-318">En avisering kan anropa en webhook eller e-administratör om genomflödet av programgatewayen är över, under eller med ett tröskelvärde för en angiven period.</span><span class="sxs-lookup"><span data-stu-id="cd367-318">For example, an alert can call a webhook or email an administrator if the throughput of the application gateway is above, below, or at a threshold for a specified period.</span></span>

<span data-ttu-id="cd367-319">I följande exempel vägleder dig genom att skapa en aviseringsregel som skickar ett e-postmeddelande till en administratör efter genomströmning överträdelser en tröskel:</span><span class="sxs-lookup"><span data-stu-id="cd367-319">The following example walks you through creating an alert rule that sends an email to an administrator after throughput breaches a threshold:</span></span>

1. <span data-ttu-id="cd367-320">Klicka på **Lägg till mått avisering** att öppna den **Lägg till regel** bladet.</span><span class="sxs-lookup"><span data-stu-id="cd367-320">Click **Add metric alert** to open the **Add rule** blade.</span></span> <span data-ttu-id="cd367-321">Du kan också nå det här bladet från bladet mått.</span><span class="sxs-lookup"><span data-stu-id="cd367-321">You can also reach this blade from the metrics blade.</span></span>

   ![Knappen ”Lägg till mått aviseringen”][6]

2. <span data-ttu-id="cd367-323">På den **Lägg till regel** bladet fylla i namn, tillstånd, meddela avsnitt och klickar på **OK**.</span><span class="sxs-lookup"><span data-stu-id="cd367-323">On the **Add rule** blade, fill out the name, condition, and notify sections, and click **OK**.</span></span>

   * <span data-ttu-id="cd367-324">I den **villkoret** selector, Välj ett av de fyra värdena: **större än**, **större än eller lika med**, **mindre än**, eller **mindre än eller lika med**.</span><span class="sxs-lookup"><span data-stu-id="cd367-324">In the **Condition** selector, select one of the four values: **Greater than**, **Greater than or equal**, **Less than**, or **Less than or equal to**.</span></span>

   * <span data-ttu-id="cd367-325">I den **Period** selector, Välj en period mellan 5 och 6 timmar.</span><span class="sxs-lookup"><span data-stu-id="cd367-325">In the **Period** selector, select a period from 5 minutes to 6 hours.</span></span>

   * <span data-ttu-id="cd367-326">Om du väljer **e-ägare, deltagare och läsare**, e-postmeddelandet kan vara dynamiska baserat på de användare som har åtkomst till resursen.</span><span class="sxs-lookup"><span data-stu-id="cd367-326">If you select **Email owners, contributors, and readers**, the email can be dynamic based on the users who have access to that resource.</span></span> <span data-ttu-id="cd367-327">Annars kan du ange en kommaavgränsad lista över användare i den **ytterligare administratören email(s)** rutan.</span><span class="sxs-lookup"><span data-stu-id="cd367-327">Otherwise, you can provide a comma-separated list of users in the **Additional administrator email(s)** box.</span></span>

   ![Lägg till regel bladet][7]

<span data-ttu-id="cd367-329">Om tröskelvärdet överskrids, kommer ett e-postmeddelande som liknar det i följande bild:</span><span class="sxs-lookup"><span data-stu-id="cd367-329">If the threshold is breached, an email that's similar to the one in the following image arrives:</span></span>

![E-post för utsatts för intrång tröskelvärde][8]

<span data-ttu-id="cd367-331">En lista över aviseringar visas när du skapar en avisering om mått.</span><span class="sxs-lookup"><span data-stu-id="cd367-331">A list of alerts appears after you create a metric alert.</span></span> <span data-ttu-id="cd367-332">Den innehåller en översikt över alla Varningsregler.</span><span class="sxs-lookup"><span data-stu-id="cd367-332">It provides an overview of all the alert rules.</span></span>

![Listan över aviseringar och regler][9]

<span data-ttu-id="cd367-334">Mer information om aviseringar finns [få aviseringar](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).</span><span class="sxs-lookup"><span data-stu-id="cd367-334">To learn more about alert notifications, see [Receive alert notifications](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).</span></span>

<span data-ttu-id="cd367-335">Om du vill veta mer om webhooks och hur du kan använda dem med aviseringar finns [konfigurera en webhook på en Azure mått avisering](../monitoring-and-diagnostics/insights-webhooks-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="cd367-335">To understand more about webhooks and how you can use them with alerts, visit [Configure a webhook on an Azure metric alert](../monitoring-and-diagnostics/insights-webhooks-alerts.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="cd367-336">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="cd367-336">Next steps</span></span>

* <span data-ttu-id="cd367-337">Visualisera räknare och händelseloggar med [logganalys](../log-analytics/log-analytics-azure-networking-analytics.md).</span><span class="sxs-lookup"><span data-stu-id="cd367-337">Visualize counter and event logs by using [Log Analytics](../log-analytics/log-analytics-azure-networking-analytics.md).</span></span>
* <span data-ttu-id="cd367-338">[Visualisera dina Azure-aktivitetsloggen med Power BI](http://blogs.msdn.com/b/powerbi/archive/2015/09/30/monitor-azure-audit-logs-with-power-bi.aspx) blogginlägg.</span><span class="sxs-lookup"><span data-stu-id="cd367-338">[Visualize your Azure activity log with Power BI](http://blogs.msdn.com/b/powerbi/archive/2015/09/30/monitor-azure-audit-logs-with-power-bi.aspx) blog post.</span></span>
* <span data-ttu-id="cd367-339">[Visa och analysera Azure aktivitetsloggar i Power BI och mycket mer](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/) blogginlägg.</span><span class="sxs-lookup"><span data-stu-id="cd367-339">[View and analyze Azure activity logs in Power BI and more](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/) blog post.</span></span>

[1]: ./media/application-gateway-diagnostics/figure1.png
[2]: ./media/application-gateway-diagnostics/figure2.png
[3]: ./media/application-gateway-diagnostics/figure3.png
[4]: ./media/application-gateway-diagnostics/figure4.png
[5]: ./media/application-gateway-diagnostics/figure5.png
[6]: ./media/application-gateway-diagnostics/figure6.png
[7]: ./media/application-gateway-diagnostics/figure7.png
[8]: ./media/application-gateway-diagnostics/figure8.png
[9]: ./media/application-gateway-diagnostics/figure9.png
[10]: ./media/application-gateway-diagnostics/figure10.png
