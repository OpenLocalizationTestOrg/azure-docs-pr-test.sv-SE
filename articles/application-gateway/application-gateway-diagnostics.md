---
title: "aaaMonitor åtkomst till loggar, Prestandaloggar, backend-hälsa och mått för Programgateway | Microsoft Docs"
description: "Lär dig hur tooenable och hantera åtkomstloggar och Prestandaloggar för Programgateway"
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
ms.openlocfilehash: 36ebf15c28f776158350ef8e73d617ef68e09266
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="back-end-health-diagnostic-logs-and-metrics-for-application-gateway"></a><span data-ttu-id="e0cd5-103">Backend-hälsotillstånd, diagnostikloggar och mått för Programgateway</span><span class="sxs-lookup"><span data-stu-id="e0cd5-103">Back-end health, diagnostic logs, and metrics for Application Gateway</span></span>

<span data-ttu-id="e0cd5-104">Med hjälp av Azure Application Gateway kan övervaka du resurser i hello följande sätt:</span><span class="sxs-lookup"><span data-stu-id="e0cd5-104">By using Azure Application Gateway, you can monitor resources in hello following ways:</span></span>

* <span data-ttu-id="e0cd5-105">[Backend-hälsa](#back-end-health): Programgateway ger hello kapaciteten toomonitor hello hälsa hello servrar i hello backend-pooler via hello Azure-portalen och via PowerShell.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-105">[Back-end health](#back-end-health): Application Gateway provides hello capability toomonitor hello health of hello servers in hello back-end pools through hello Azure portal and through PowerShell.</span></span> <span data-ttu-id="e0cd5-106">Du kan också hitta hello hälsotillstånd hello backend-pooler via hello prestanda diagnostikloggar.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-106">You can also find hello health of hello back-end pools through hello performance diagnostic logs.</span></span>

* <span data-ttu-id="e0cd5-107">[Loggar](#diagnostic-logs): loggar för prestanda, åtkomst och andra data toobe sparas eller förbrukad från en resurs för övervakning.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-107">[Logs](#diagnostic-logs): Logs allow for performance, access, and other data toobe saved or consumed from a resource for monitoring purposes.</span></span>

* <span data-ttu-id="e0cd5-108">[Mått](#metrics): Application Gateway har för närvarande ett enskilt mått.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-108">[Metrics](#metrics): Application Gateway currently has one metric.</span></span> <span data-ttu-id="e0cd5-109">Mätvärdet mäter hello genomflödet i hello Programgateway i byte per sekund.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-109">This metric measures hello throughput of hello application gateway in bytes per second.</span></span>

## <a name="back-end-health"></a><span data-ttu-id="e0cd5-110">Backend-hälsa</span><span class="sxs-lookup"><span data-stu-id="e0cd5-110">Back-end health</span></span>

<span data-ttu-id="e0cd5-111">Programgateway ger hello kapaciteten toomonitor hello hälsotillståndet för enskilda medlemmar av hello backend-pooler via hello-portalen, PowerShell och hello-kommandoradsgränssnittet (CLI).</span><span class="sxs-lookup"><span data-stu-id="e0cd5-111">Application Gateway provides hello capability toomonitor hello health of individual members of hello back-end pools through hello portal, PowerShell, and hello command-line interface (CLI).</span></span> <span data-ttu-id="e0cd5-112">Du kan också hitta det totala hälsotillståndet för backend-pooler via hello prestanda diagnostikloggar.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-112">You can also find an aggregated health summary of back-end pools through hello performance diagnostic logs.</span></span> 

<span data-ttu-id="e0cd5-113">hello backend-hälsorapport visar hello utdata från hello Programgateway hälsa avsökningen toohello backend-instanser.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-113">hello back-end health report reflects hello output of hello Application Gateway health probe toohello back-end instances.</span></span> <span data-ttu-id="e0cd5-114">När avsökning lyckas och hello tillbaka slutet kan ta emot trafik, är det felfritt.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-114">When probing is successful and hello back end can receive traffic, it's considered healthy.</span></span> <span data-ttu-id="e0cd5-115">Annars anses vara felaktig.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-115">Otherwise, it's considered unhealthy.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e0cd5-116">Om det finns en nätverkssäkerhetsgrupp (NSG) på ett Application Gateway-undernät, öppna portintervall 65503 65534 i hello Programgateway undernät för inkommande trafik.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-116">If there is a network security group (NSG) on an Application Gateway subnet, open port ranges 65503-65534 on hello Application Gateway subnet for inbound traffic.</span></span> <span data-ttu-id="e0cd5-117">Dessa portar är obligatoriska för hello backend-hälsa API toowork.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-117">These ports are required for hello back-end health API toowork.</span></span>


### <a name="view-back-end-health-through-hello-portal"></a><span data-ttu-id="e0cd5-118">Visa backend-hälsa hello-portalen</span><span class="sxs-lookup"><span data-stu-id="e0cd5-118">View back-end health through hello portal</span></span>

<span data-ttu-id="e0cd5-119">Backend-hälsa tillhandahålls automatiskt i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-119">In hello portal, back-end health is provided automatically.</span></span> <span data-ttu-id="e0cd5-120">Välj i en befintlig Programgateway **övervakning** > **Backend hälsa**.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-120">In an existing application gateway, select **Monitoring** > **Backend health**.</span></span> 

<span data-ttu-id="e0cd5-121">Varje medlem i hello backend-pool visas på den här sidan (om det är ett nätverkskort, IP eller FQDN).</span><span class="sxs-lookup"><span data-stu-id="e0cd5-121">Each member in hello back-end pool is listed on this page (whether it's a NIC, IP, or FQDN).</span></span> <span data-ttu-id="e0cd5-122">Backend-namn, port, backend-HTTP-namn och hälsostatus visas.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-122">Back-end pool name, port, back-end HTTP settings name, and health status are shown.</span></span> <span data-ttu-id="e0cd5-123">Giltiga värden för hälsostatus **felfri**, **ohälsosam**, och **okänd**.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-123">Valid values for health status are **Healthy**, **Unhealthy**, and **Unknown**.</span></span>

> [!NOTE]
> <span data-ttu-id="e0cd5-124">Om du ser en backend-hälsostatus **okänd**, kontrollera att åtkomst toohello serverdel inte blockeras av en regel för NSG, en användardefinierad väg (UDR) eller en anpassad DNS i hello virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-124">If you see a back-end health status of **Unknown**, ensure that access toohello back end is not blocked by an NSG rule, a user-defined route (UDR), or a custom DNS in hello virtual network.</span></span>

![Backend-hälsa][10]

### <a name="view-back-end-health-through-powershell"></a><span data-ttu-id="e0cd5-126">Visa backend-hälsa via PowerShell</span><span class="sxs-lookup"><span data-stu-id="e0cd5-126">View back-end health through PowerShell</span></span>

<span data-ttu-id="e0cd5-127">hello följande PowerShell-kod visar hur tooview backend-hälsa genom att använda hello `Get-AzureRmApplicationGatewayBackendHealth` cmdlet:</span><span class="sxs-lookup"><span data-stu-id="e0cd5-127">hello following PowerShell code shows how tooview back-end health by using hello `Get-AzureRmApplicationGatewayBackendHealth` cmdlet:</span></span>

```powershell
Get-AzureRmApplicationGatewayBackendHealth -Name ApplicationGateway1 -ResourceGroupName Contoso
```

### <a name="view-back-end-health-through-azure-cli-20"></a><span data-ttu-id="e0cd5-128">Visa backend-hälsa genom Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="e0cd5-128">View back-end health through Azure CLI 2.0</span></span>

```azurecli
az network application-gateway show-backend-health --resource-group AdatumAppGatewayRG --name AdatumAppGateway
```

### <a name="results"></a><span data-ttu-id="e0cd5-129">Resultat</span><span class="sxs-lookup"><span data-stu-id="e0cd5-129">Results</span></span>

<span data-ttu-id="e0cd5-130">hello följande fragment visas ett exempel på hello svar:</span><span class="sxs-lookup"><span data-stu-id="e0cd5-130">hello following snippet shows an example of hello response:</span></span>

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

## <span data-ttu-id="e0cd5-131"><a name="diagnostic-logging"></a>Diagnostikloggar</span><span class="sxs-lookup"><span data-stu-id="e0cd5-131"><a name="diagnostic-logging"></a>Diagnostic logs</span></span>

<span data-ttu-id="e0cd5-132">Du kan använda olika typer av loggar i Azure toomanage och felsöka programgatewayer.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-132">You can use different types of logs in Azure toomanage and troubleshoot application gateways.</span></span> <span data-ttu-id="e0cd5-133">Du kan komma åt vissa av dessa loggar hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-133">You can access some of these logs through hello portal.</span></span> <span data-ttu-id="e0cd5-134">Alla loggar kan extraheras från Azure Blob storage och visas i olika verktyg som [logganalys](../log-analytics/log-analytics-azure-networking-analytics.md), Excel och Power BI.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-134">All logs can be extracted from Azure Blob storage and viewed in different tools, such as [Log Analytics](../log-analytics/log-analytics-azure-networking-analytics.md), Excel, and Power BI.</span></span> <span data-ttu-id="e0cd5-135">Mer information om hello olika typer av loggar från hello följande lista:</span><span class="sxs-lookup"><span data-stu-id="e0cd5-135">You can learn more about hello different types of logs from hello following list:</span></span>

* <span data-ttu-id="e0cd5-136">**Aktivitetsloggen**: du kan använda [Azure aktivitetsloggar](../monitoring-and-diagnostics/insights-debugging-with-events.md) (hette operativa loggar och granskningsloggar) tooview alla operationer som skickats tooyour Azure-prenumeration och deras status.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-136">**Activity log**: You can use [Azure activity logs](../monitoring-and-diagnostics/insights-debugging-with-events.md) (formerly known as operational logs and audit logs) tooview all operations that are submitted tooyour Azure subscription, and their status.</span></span> <span data-ttu-id="e0cd5-137">Aktiviteten loggposter som samlas in som standard och du kan visa dem i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-137">Activity log entries are collected by default, and you can view them in hello Azure portal.</span></span>
* <span data-ttu-id="e0cd5-138">**Åtkomstlogg**: du kan använda den här loggen tooview Programgateway åtkomstmönster och analysera viktig information, inklusive hello anroparen-IP, begärd URL, svar svarstid, returkod och byte in och ut. En åtkomstlogg samlas in varje 300 sekunder.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-138">**Access log**: You can use this log tooview Application Gateway access patterns and analyze important information, including hello caller's IP, requested URL, response latency, return code, and bytes in and out. An access log is collected every 300 seconds.</span></span> <span data-ttu-id="e0cd5-139">Den här loggfilen innehåller en post per instans av Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-139">This log contains one record per instance of Application Gateway.</span></span> <span data-ttu-id="e0cd5-140">hello Programgateway instans kan identifieras av hello instanceId-egenskapen.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-140">hello Application Gateway instance can be identified by hello instanceId property.</span></span>
* <span data-ttu-id="e0cd5-141">**Prestanda loggen**: du kan använda den här loggen tooview hur Programgateway instanser utför.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-141">**Performance log**: You can use this log tooview how Application Gateway instances are performing.</span></span> <span data-ttu-id="e0cd5-142">Den här loggfilen innehåller prestandainformation om för varje instans, inklusive totalt antal begäranden som betjänats, genomflöde i byte, totalt antal begäranden som betjänats antalet misslyckade begäranden och felfria och feltillstånd backend-instanser.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-142">This log captures performance information for each instance, including total requests served, throughput in bytes, total requests served, failed request count, and healthy and unhealthy back-end instance count.</span></span> <span data-ttu-id="e0cd5-143">En prestandalogg som samlas in var 60: e sekund.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-143">A performance log is collected every 60 seconds.</span></span>
* <span data-ttu-id="e0cd5-144">**Brandväggsloggen**: du kan använda den här loggen tooview hello begäranden som loggas via identifiering eller förhindra läget för en Programgateway som är konfigurerad med hello Brandvägg för webbaserade program.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-144">**Firewall log**: You can use this log tooview hello requests that are logged through either detection or prevention mode of an application gateway that is configured with hello web application firewall.</span></span>

> [!NOTE]
> <span data-ttu-id="e0cd5-145">Loggarna är bara tillgängligt för resurser som har distribuerats i hello Azure Resource Manager-distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-145">Logs are available only for resources deployed in hello Azure Resource Manager deployment model.</span></span> <span data-ttu-id="e0cd5-146">Du kan inte använda loggar för resurser i hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-146">You cannot use logs for resources in hello classic deployment model.</span></span> <span data-ttu-id="e0cd5-147">En bättre förståelse av hello två modeller, finns hello [förstå Resource Manager distribution och klassisk distribution](../azure-resource-manager/resource-manager-deployment-model.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-147">For a better understanding of hello two models, see hello [Understanding Resource Manager deployment and classic deployment](../azure-resource-manager/resource-manager-deployment-model.md) article.</span></span>

<span data-ttu-id="e0cd5-148">Det finns tre alternativ för att lagra dina loggar:</span><span class="sxs-lookup"><span data-stu-id="e0cd5-148">You have three options for storing your logs:</span></span>

* <span data-ttu-id="e0cd5-149">**Lagringskontot**: Storage-konton är bäst för loggar när loggar lagras under en längre tid och granskas vid behov.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-149">**Storage account**: Storage accounts are best used for logs when logs are stored for a longer duration and reviewed when needed.</span></span>
* <span data-ttu-id="e0cd5-150">**Händelsehubbar**: händelsehubbar är ett bra alternativ för att integrera med andra säkerhetsinformation och händelse (SEIM)-hanteringsverktygen tooget aviseringar på dina resurser.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-150">**Event hubs**: Event hubs are a great option for integrating with other security information and event management (SEIM) tools tooget alerts on your resources.</span></span>
* <span data-ttu-id="e0cd5-151">**Logga Analytics**: logganalys passar bäst för allmän övervakning i realtid för programmet eller tittar på trender.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-151">**Log Analytics**: Log Analytics is best used for general real-time monitoring of your application or looking at trends.</span></span>

### <a name="enable-logging-through-powershell"></a><span data-ttu-id="e0cd5-152">Aktivera loggning med PowerShell</span><span class="sxs-lookup"><span data-stu-id="e0cd5-152">Enable logging through PowerShell</span></span>

<span data-ttu-id="e0cd5-153">Aktivitetsloggning aktiveras automatiskt för varje resurs för hanteraren för filserverresurser.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-153">Activity logging is automatically enabled for every Resource Manager resource.</span></span> <span data-ttu-id="e0cd5-154">Du måste aktivera åtkomst och prestanda loggning toostart samlar in hello-data som är tillgängliga via dessa loggar.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-154">You must enable access and performance logging toostart collecting hello data available through those logs.</span></span> <span data-ttu-id="e0cd5-155">tooenable loggning, Använd hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="e0cd5-155">tooenable logging, use hello following steps:</span></span>

1. <span data-ttu-id="e0cd5-156">Observera ditt lagringskonto resurs-ID, där hello loggdata lagras.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-156">Note your storage account's resource ID, where hello log data is stored.</span></span> <span data-ttu-id="e0cd5-157">Det här värdet är formatet hello: /subscriptions/\<subscriptionId\>/resourceGroups/\<resursgruppnamn\>/providers/Microsoft.Storage/storageAccounts/\<lagringskontonamnet\>.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-157">This value is of hello form: /subscriptions/\<subscriptionId\>/resourceGroups/\<resource group name\>/providers/Microsoft.Storage/storageAccounts/\<storage account name\>.</span></span> <span data-ttu-id="e0cd5-158">Du kan använda storage-konto i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-158">You can use any storage account in your subscription.</span></span> <span data-ttu-id="e0cd5-159">Du kan använda hello Azure portal toofind informationen.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-159">You can use hello Azure portal toofind this information.</span></span>

    ![Portal: resurs-ID för lagringskontot](./media/application-gateway-diagnostics/diagnostics1.png)

2. <span data-ttu-id="e0cd5-161">Observera din Programgateway resurs-ID som har aktiverats.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-161">Note your application gateway's resource ID for which logging is enabled.</span></span> <span data-ttu-id="e0cd5-162">Det här värdet är formatet hello: /subscriptions/\<subscriptionId\>/resourceGroups/\<resursgruppnamn\>/providers/Microsoft.Network/applicationGateways/\<Programgateway namnet\>.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-162">This value is of hello form: /subscriptions/\<subscriptionId\>/resourceGroups/\<resource group name\>/providers/Microsoft.Network/applicationGateways/\<application gateway name\>.</span></span> <span data-ttu-id="e0cd5-163">Du kan använda hello portal toofind informationen.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-163">You can use hello portal toofind this information.</span></span>

    ![Portal: resurs-ID för Programgateway](./media/application-gateway-diagnostics/diagnostics2.png)

3. <span data-ttu-id="e0cd5-165">Aktivera diagnostikloggning med hello följande PowerShell-cmdlet:</span><span class="sxs-lookup"><span data-stu-id="e0cd5-165">Enable diagnostic logging by using hello following PowerShell cmdlet:</span></span>

    ```powershell
    Set-AzureRmDiagnosticSetting  -ResourceId /subscriptions/<subscriptionId>/resourceGroups/<resource group name>/providers/Microsoft.Network/applicationGateways/<application gateway name> -StorageAccountId /subscriptions/<subscriptionId>/resourceGroups/<resource group name>/providers/Microsoft.Storage/storageAccounts/<storage account name> -Enabled $true     
    ```
    
> [!TIP] 
><span data-ttu-id="e0cd5-166">Aktivitetsloggar kräver inte ett separat lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-166">Activity logs do not require a separate storage account.</span></span> <span data-ttu-id="e0cd5-167">hello användningen av lagringsutrymme för åtkomst- och prestandaloggning debiteras tjänsten.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-167">hello use of storage for access and performance logging incurs service charges.</span></span>

### <a name="enable-logging-through-hello-azure-portal"></a><span data-ttu-id="e0cd5-168">Aktivera loggning via hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="e0cd5-168">Enable logging through hello Azure portal</span></span>

1. <span data-ttu-id="e0cd5-169">I hello Azure-portalen, söka efter resurs och klickar på **diagnostikloggar**.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-169">In hello Azure portal, find your resource and click **Diagnostic logs**.</span></span>

   <span data-ttu-id="e0cd5-170">För Programgateway är tre loggarna tillgängliga:</span><span class="sxs-lookup"><span data-stu-id="e0cd5-170">For Application Gateway, three logs are available:</span></span>

   * <span data-ttu-id="e0cd5-171">Åtkomstlogg</span><span class="sxs-lookup"><span data-stu-id="e0cd5-171">Access log</span></span>
   * <span data-ttu-id="e0cd5-172">Prestandalogg</span><span class="sxs-lookup"><span data-stu-id="e0cd5-172">Performance log</span></span>
   * <span data-ttu-id="e0cd5-173">Brandväggen logg</span><span class="sxs-lookup"><span data-stu-id="e0cd5-173">Firewall log</span></span>

2. <span data-ttu-id="e0cd5-174">toostart att samla in data, klickar du på **aktivera diagnostiken**.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-174">toostart collecting data, click **Turn on diagnostics**.</span></span>

   ![Aktivera diagnostik][1]

3. <span data-ttu-id="e0cd5-176">Hej **diagnostikinställningar** bladet innehåller hello inställningar för hello diagnostikloggar.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-176">hello **Diagnostics settings** blade provides hello settings for hello diagnostic logs.</span></span> <span data-ttu-id="e0cd5-177">I det här exemplet lagras logganalys hello loggar.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-177">In this example, Log Analytics stores hello logs.</span></span> <span data-ttu-id="e0cd5-178">Klicka på **konfigurera** under **logganalys** tooconfigure din arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-178">Click **Configure** under **Log Analytics** tooconfigure your workspace.</span></span> <span data-ttu-id="e0cd5-179">Du kan också använda händelsehubbar och en storage-konto toosave hello diagnostikloggar.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-179">You can also use event hubs and a storage account toosave hello diagnostic logs.</span></span>

   ![Startar hello konfiguration][2]

4. <span data-ttu-id="e0cd5-181">Välj en befintlig Operations Management Suite (OMS) arbetsyta eller skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-181">Choose an existing Operations Management Suite (OMS) workspace or create a new one.</span></span> <span data-ttu-id="e0cd5-182">Det här exemplet använder en befintlig.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-182">This example uses an existing one.</span></span>

   ![Alternativ för OMS arbetsytor][3]

5. <span data-ttu-id="e0cd5-184">Bekräfta inställningarna för hello och klicka på **spara**.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-184">Confirm hello settings and click **Save**.</span></span>

   ![Diagnostik inställningsbladet med val][4]

### <a name="activity-log"></a><span data-ttu-id="e0cd5-186">Aktivitetslogg</span><span class="sxs-lookup"><span data-stu-id="e0cd5-186">Activity log</span></span>

<span data-ttu-id="e0cd5-187">Azure genererar hello aktivitetsloggen som standard.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-187">Azure generates hello activity log by default.</span></span> <span data-ttu-id="e0cd5-188">hello loggar bevaras under 90 dagar i hello Azure händelseloggar store.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-188">hello logs are preserved for 90 days in hello Azure event logs store.</span></span> <span data-ttu-id="e0cd5-189">Mer information om de här loggarna genom att läsa hello [visa händelser och aktivitetsloggen](../monitoring-and-diagnostics/insights-debugging-with-events.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-189">Learn more about these logs by reading hello [View events and activity log](../monitoring-and-diagnostics/insights-debugging-with-events.md) article.</span></span>

### <a name="access-log"></a><span data-ttu-id="e0cd5-190">Åtkomstlogg</span><span class="sxs-lookup"><span data-stu-id="e0cd5-190">Access log</span></span>

<span data-ttu-id="e0cd5-191">hello åtkomst loggen genereras bara om du har aktiverat på varje Application Gateway-instans enligt anvisningarna i föregående steg hello.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-191">hello access log is generated only if you've enabled it on each Application Gateway instance, as detailed in hello preceding steps.</span></span> <span data-ttu-id="e0cd5-192">hello data lagras i hello storage-konto som du angav när du har aktiverat hello loggning.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-192">hello data is stored in hello storage account that you specified when you enabled hello logging.</span></span> <span data-ttu-id="e0cd5-193">Varje åtkomst för Programgateway loggas i JSON-format, som visas i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="e0cd5-193">Each access of Application Gateway is logged in JSON format, as shown in hello following example:</span></span>


|<span data-ttu-id="e0cd5-194">Värde</span><span class="sxs-lookup"><span data-stu-id="e0cd5-194">Value</span></span>  |<span data-ttu-id="e0cd5-195">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="e0cd5-195">Description</span></span>  |
|---------|---------|
|<span data-ttu-id="e0cd5-196">InstanceId</span><span class="sxs-lookup"><span data-stu-id="e0cd5-196">instanceId</span></span>     | <span data-ttu-id="e0cd5-197">Programgateway instansen hanteras hello begäran.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-197">Application Gateway instance that served hello request.</span></span>        |
|<span data-ttu-id="e0cd5-198">ClientIP</span><span class="sxs-lookup"><span data-stu-id="e0cd5-198">clientIP</span></span>     | <span data-ttu-id="e0cd5-199">Ursprungliga IP-Adressen för hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-199">Originating IP for hello request.</span></span>        |
|<span data-ttu-id="e0cd5-200">clientPort</span><span class="sxs-lookup"><span data-stu-id="e0cd5-200">clientPort</span></span>     | <span data-ttu-id="e0cd5-201">Ursprungliga porten för hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-201">Originating port for hello request.</span></span>       |
|<span data-ttu-id="e0cd5-202">HttpMethod</span><span class="sxs-lookup"><span data-stu-id="e0cd5-202">httpMethod</span></span>     | <span data-ttu-id="e0cd5-203">HTTP-metod som används av hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-203">HTTP method used by hello request.</span></span>       |
|<span data-ttu-id="e0cd5-204">requestUri</span><span class="sxs-lookup"><span data-stu-id="e0cd5-204">requestUri</span></span>     | <span data-ttu-id="e0cd5-205">URI för hello tog emot begäran.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-205">URI of hello received request.</span></span>        |
|<span data-ttu-id="e0cd5-206">RequestQuery</span><span class="sxs-lookup"><span data-stu-id="e0cd5-206">RequestQuery</span></span>     | <span data-ttu-id="e0cd5-207">**Server-dirigerat**: backend-pool-instans som har skickats hello begäran.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-207">**Server-Routed**: Back-end pool instance that was sent hello request.</span></span> </br> <span data-ttu-id="e0cd5-208">**X-AzureApplicationGateway-LOG-ID**: Korrelations-ID som används för hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-208">**X-AzureApplicationGateway-LOG-ID**: Correlation ID used for hello request.</span></span> <span data-ttu-id="e0cd5-209">Det kan vara problem med används tootroubleshoot trafik på hello backend-servrar.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-209">It can be used tootroubleshoot traffic issues on hello back-end servers.</span></span> </br><span data-ttu-id="e0cd5-210">**SERVER-STATUS**: HTTP-svarskoden Programgateway togs emot från hello serverdel.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-210">**SERVER-STATUS**: HTTP response code that Application Gateway received from hello back end.</span></span>       |
|<span data-ttu-id="e0cd5-211">UserAgent</span><span class="sxs-lookup"><span data-stu-id="e0cd5-211">UserAgent</span></span>     | <span data-ttu-id="e0cd5-212">Användaragent från hello HTTP huvudet i begäran.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-212">User agent from hello HTTP request header.</span></span>        |
|<span data-ttu-id="e0cd5-213">httpStatus</span><span class="sxs-lookup"><span data-stu-id="e0cd5-213">httpStatus</span></span>     | <span data-ttu-id="e0cd5-214">HTTP-statuskod returneras toohello klienten från Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-214">HTTP status code returned toohello client from Application Gateway.</span></span>       |
|<span data-ttu-id="e0cd5-215">httpVersion</span><span class="sxs-lookup"><span data-stu-id="e0cd5-215">httpVersion</span></span>     | <span data-ttu-id="e0cd5-216">HTTP-versionen av hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-216">HTTP version of hello request.</span></span>        |
|<span data-ttu-id="e0cd5-217">receivedBytes</span><span class="sxs-lookup"><span data-stu-id="e0cd5-217">receivedBytes</span></span>     | <span data-ttu-id="e0cd5-218">Storleken på paketet tas emot, i byte.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-218">Size of packet received, in bytes.</span></span>        |
|<span data-ttu-id="e0cd5-219">sentBytes</span><span class="sxs-lookup"><span data-stu-id="e0cd5-219">sentBytes</span></span>| <span data-ttu-id="e0cd5-220">Storleken på paketet skickas, i byte.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-220">Size of packet sent, in bytes.</span></span>|
|<span data-ttu-id="e0cd5-221">timeTaken</span><span class="sxs-lookup"><span data-stu-id="e0cd5-221">timeTaken</span></span>| <span data-ttu-id="e0cd5-222">Lång tid (i millisekunder) som det tar för en begäran toobe bearbetas och dess svar toobe skickas.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-222">Length of time (in milliseconds) that it takes for a request toobe processed and its response toobe sent.</span></span> <span data-ttu-id="e0cd5-223">Det här beräknas som hello intervall från hello tid när Programgateway får hello första byten för en HTTP-begäran toohello tid när hello svar skickar åtgärden har slutförts.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-223">This is calculated as hello interval from hello time when Application Gateway receives hello first byte of an HTTP request toohello time when hello response send operation finishes.</span></span> <span data-ttu-id="e0cd5-224">Det är viktigt toonote som vanligtvis hello Time-Taken fältet innehåller hello tid som begäran och svar hälsningspaket reser hello nätverket.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-224">It's important toonote that hello Time-Taken field usually includes hello time that hello request and response packets are traveling over hello network.</span></span> |
|<span data-ttu-id="e0cd5-225">sslEnabled</span><span class="sxs-lookup"><span data-stu-id="e0cd5-225">sslEnabled</span></span>| <span data-ttu-id="e0cd5-226">Om kommunikation toohello backend-pooler används SSL.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-226">Whether communication toohello back-end pools used SSL.</span></span> <span data-ttu-id="e0cd5-227">Giltiga värden är på och av.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-227">Valid values are on and off.</span></span>|
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

### <a name="performance-log"></a><span data-ttu-id="e0cd5-228">Prestandalogg</span><span class="sxs-lookup"><span data-stu-id="e0cd5-228">Performance log</span></span>

<span data-ttu-id="e0cd5-229">hello prestanda loggen genereras bara om du har aktiverat på varje Application Gateway-instans enligt anvisningarna i föregående steg hello.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-229">hello performance log is generated only if you have enabled it on each Application Gateway instance, as detailed in hello preceding steps.</span></span> <span data-ttu-id="e0cd5-230">hello data lagras i hello storage-konto som du angav när du har aktiverat hello loggning.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-230">hello data is stored in hello storage account that you specified when you enabled hello logging.</span></span> <span data-ttu-id="e0cd5-231">loggdata för prestanda för hello genereras på 1 minut.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-231">hello performance log data is generated in 1-minute intervals.</span></span> <span data-ttu-id="e0cd5-232">hello följande data loggas:</span><span class="sxs-lookup"><span data-stu-id="e0cd5-232">hello following data is logged:</span></span>


|<span data-ttu-id="e0cd5-233">Värde</span><span class="sxs-lookup"><span data-stu-id="e0cd5-233">Value</span></span>  |<span data-ttu-id="e0cd5-234">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="e0cd5-234">Description</span></span>  |
|---------|---------|
|<span data-ttu-id="e0cd5-235">InstanceId</span><span class="sxs-lookup"><span data-stu-id="e0cd5-235">instanceId</span></span>     |  <span data-ttu-id="e0cd5-236">Programmet Gateway-instans för vilka data som genereras.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-236">Application Gateway instance for which performance data is being generated.</span></span> <span data-ttu-id="e0cd5-237">För en gateway med flera instans programmet finns en rad per instans.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-237">For a multiple-instance application gateway, there is one row per instance.</span></span>        |
|<span data-ttu-id="e0cd5-238">healthyHostCount</span><span class="sxs-lookup"><span data-stu-id="e0cd5-238">healthyHostCount</span></span>     | <span data-ttu-id="e0cd5-239">Antalet felfri värdar i hello backend-adresspool.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-239">Number of healthy hosts in hello back-end pool.</span></span>        |
|<span data-ttu-id="e0cd5-240">unHealthyHostCount</span><span class="sxs-lookup"><span data-stu-id="e0cd5-240">unHealthyHostCount</span></span>     | <span data-ttu-id="e0cd5-241">Antal felaktiga värdar i hello backend-adresspool.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-241">Number of unhealthy hosts in hello back-end pool.</span></span>        |
|<span data-ttu-id="e0cd5-242">RequestCount</span><span class="sxs-lookup"><span data-stu-id="e0cd5-242">requestCount</span></span>     | <span data-ttu-id="e0cd5-243">Antal begäranden som betjänats.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-243">Number of requests served.</span></span>        |
|<span data-ttu-id="e0cd5-244">Svarstid</span><span class="sxs-lookup"><span data-stu-id="e0cd5-244">latency</span></span> | <span data-ttu-id="e0cd5-245">Svarstid (i millisekunder) för förfrågningar från hello instans toohello serverdel som fungerar hello begäranden.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-245">Latency (in milliseconds) of requests from hello instance toohello back end that serves hello requests.</span></span> |
|<span data-ttu-id="e0cd5-246">failedRequestCount</span><span class="sxs-lookup"><span data-stu-id="e0cd5-246">failedRequestCount</span></span>| <span data-ttu-id="e0cd5-247">Antal misslyckade begäranden.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-247">Number of failed requests.</span></span>|
|<span data-ttu-id="e0cd5-248">Dataflöde</span><span class="sxs-lookup"><span data-stu-id="e0cd5-248">throughput</span></span>| <span data-ttu-id="e0cd5-249">Genomsnittlig genomströmning eftersom hello senaste loggen, mätt i byte per sekund.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-249">Average throughput since hello last log, measured in bytes per second.</span></span>|

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
> <span data-ttu-id="e0cd5-250">Svarstiden är beräknad hello tidpunkt då hello första byten för hello HTTP-begäran är mottagna toohello tid då hello sista byten av hello HTTP-svar skickas.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-250">Latency is calculated from hello time when hello first byte of hello HTTP request is received toohello time when hello last byte of hello HTTP response is sent.</span></span> <span data-ttu-id="e0cd5-251">Dess hello summan av hello Programgateway bearbetningstid plus hello nätverket kostnad toohello tillbaka avsluta plus hello länge hello serverdel tar tooprocess hello begäran.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-251">It's hello sum of hello Application Gateway processing time plus hello network cost toohello back end, plus hello time that hello back end takes tooprocess hello request.</span></span>

### <a name="firewall-log"></a><span data-ttu-id="e0cd5-252">Brandväggen logg</span><span class="sxs-lookup"><span data-stu-id="e0cd5-252">Firewall log</span></span>

<span data-ttu-id="e0cd5-253">hello brandväggsloggen genereras bara om du har aktiverat för varje Programgateway enligt anvisningarna i föregående steg hello.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-253">hello firewall log is generated only if you have enabled it for each application gateway, as detailed in hello preceding steps.</span></span> <span data-ttu-id="e0cd5-254">Den här loggfilen kräver också Brandvägg för webbaserade program som hello har konfigurerats på en Programgateway.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-254">This log also requires that hello web application firewall is configured on an application gateway.</span></span> <span data-ttu-id="e0cd5-255">hello data lagras i hello storage-konto som du angav när du har aktiverat hello loggning.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-255">hello data is stored in hello storage account that you specified when you enabled hello logging.</span></span> <span data-ttu-id="e0cd5-256">hello följande data loggas:</span><span class="sxs-lookup"><span data-stu-id="e0cd5-256">hello following data is logged:</span></span>


|<span data-ttu-id="e0cd5-257">Värde</span><span class="sxs-lookup"><span data-stu-id="e0cd5-257">Value</span></span>  |<span data-ttu-id="e0cd5-258">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="e0cd5-258">Description</span></span>  |
|---------|---------|
|<span data-ttu-id="e0cd5-259">InstanceId</span><span class="sxs-lookup"><span data-stu-id="e0cd5-259">instanceId</span></span>     | <span data-ttu-id="e0cd5-260">Programmet Gateway-instans för vilken brandvägg genereras data.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-260">Application Gateway instance for which firewall data is being generated.</span></span> <span data-ttu-id="e0cd5-261">För en gateway med flera instans programmet finns en rad per instans.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-261">For a multiple-instance application gateway, there is one row per instance.</span></span>         |
|<span data-ttu-id="e0cd5-262">clientIp</span><span class="sxs-lookup"><span data-stu-id="e0cd5-262">clientIp</span></span>     |   <span data-ttu-id="e0cd5-263">Ursprungliga IP-Adressen för hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-263">Originating IP for hello request.</span></span>      |
|<span data-ttu-id="e0cd5-264">clientPort</span><span class="sxs-lookup"><span data-stu-id="e0cd5-264">clientPort</span></span>     |  <span data-ttu-id="e0cd5-265">Ursprungliga porten för hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-265">Originating port for hello request.</span></span>       |
|<span data-ttu-id="e0cd5-266">requestUri</span><span class="sxs-lookup"><span data-stu-id="e0cd5-266">requestUri</span></span>     | <span data-ttu-id="e0cd5-267">URL till hello tog emot begäran.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-267">URL of hello received request.</span></span>       |
|<span data-ttu-id="e0cd5-268">ruleSetType</span><span class="sxs-lookup"><span data-stu-id="e0cd5-268">ruleSetType</span></span>     | <span data-ttu-id="e0cd5-269">Typen regeluppsättning.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-269">Rule set type.</span></span> <span data-ttu-id="e0cd5-270">hello tillgängliga värde är OWASP.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-270">hello available value is OWASP.</span></span>        |
|<span data-ttu-id="e0cd5-271">ruleSetVersion</span><span class="sxs-lookup"><span data-stu-id="e0cd5-271">ruleSetVersion</span></span>     | <span data-ttu-id="e0cd5-272">Regeluppsättning version som används.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-272">Rule set version used.</span></span> <span data-ttu-id="e0cd5-273">Tillgängliga värden är 2.2.9 och 3.0.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-273">Available values are 2.2.9 and 3.0.</span></span>     |
|<span data-ttu-id="e0cd5-274">ruleId</span><span class="sxs-lookup"><span data-stu-id="e0cd5-274">ruleId</span></span>     | <span data-ttu-id="e0cd5-275">Regel-ID för hello som utlöser händelsen.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-275">Rule ID of hello triggering event.</span></span>        |
|<span data-ttu-id="e0cd5-276">Meddelande</span><span class="sxs-lookup"><span data-stu-id="e0cd5-276">message</span></span>     | <span data-ttu-id="e0cd5-277">Användarvänligt meddelande för hello som utlöser händelsen.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-277">User-friendly message for hello triggering event.</span></span> <span data-ttu-id="e0cd5-278">Mer information finns i avsnittet hello.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-278">More details are provided in hello details section.</span></span>        |
|<span data-ttu-id="e0cd5-279">Åtgärden</span><span class="sxs-lookup"><span data-stu-id="e0cd5-279">action</span></span>     |  <span data-ttu-id="e0cd5-280">Åtgärd för hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-280">Action taken on hello request.</span></span> <span data-ttu-id="e0cd5-281">Tillgängliga värden är blockerad och tillåtna.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-281">Available values are Blocked and Allowed.</span></span>      |
|<span data-ttu-id="e0cd5-282">plats</span><span class="sxs-lookup"><span data-stu-id="e0cd5-282">site</span></span>     | <span data-ttu-id="e0cd5-283">Plats för vilken hello loggen skapades.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-283">Site for which hello log was generated.</span></span> <span data-ttu-id="e0cd5-284">Endast globala visas för närvarande eftersom regler är globala.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-284">Currently, only Global is listed because rules are global.</span></span>|
|<span data-ttu-id="e0cd5-285">Information</span><span class="sxs-lookup"><span data-stu-id="e0cd5-285">details</span></span>     | <span data-ttu-id="e0cd5-286">Information om hello som utlöser händelsen.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-286">Details of hello triggering event.</span></span>        |
|<span data-ttu-id="e0cd5-287">details.Message</span><span class="sxs-lookup"><span data-stu-id="e0cd5-287">details.message</span></span>     | <span data-ttu-id="e0cd5-288">Beskrivning av hello regeln.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-288">Description of hello rule.</span></span>        |
|<span data-ttu-id="e0cd5-289">details.data</span><span class="sxs-lookup"><span data-stu-id="e0cd5-289">details.data</span></span>     | <span data-ttu-id="e0cd5-290">Specifika data hittades i begäran matchade hello regeln.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-290">Specific data found in request that matched hello rule.</span></span>         |
|<span data-ttu-id="e0cd5-291">details.File</span><span class="sxs-lookup"><span data-stu-id="e0cd5-291">details.file</span></span>     | <span data-ttu-id="e0cd5-292">Konfigurationsfil som innehåller hello regeln.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-292">Configuration file that contained hello rule.</span></span>        |
|<span data-ttu-id="e0cd5-293">details.Line</span><span class="sxs-lookup"><span data-stu-id="e0cd5-293">details.line</span></span>     | <span data-ttu-id="e0cd5-294">Radnumret i hello-konfigurationsfilen som utlöste hello-händelse.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-294">Line number in hello configuration file that triggered hello event.</span></span>       |

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

### <a name="view-and-analyze-hello-activity-log"></a><span data-ttu-id="e0cd5-295">Visa och analysera hello aktivitetsloggen</span><span class="sxs-lookup"><span data-stu-id="e0cd5-295">View and analyze hello activity log</span></span>

<span data-ttu-id="e0cd5-296">Du kan visa och analysera loggdata för aktiviteten med hjälp av någon av följande metoder hello:</span><span class="sxs-lookup"><span data-stu-id="e0cd5-296">You can view and analyze activity log data by using any of hello following methods:</span></span>

* <span data-ttu-id="e0cd5-297">**Azure-verktyg**: hämta information från hello aktivitetsloggen via Azure PowerShell, hello Azure CLI, hello Azure REST API eller hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-297">**Azure tools**: Retrieve information from hello activity log through Azure PowerShell, hello Azure CLI, hello Azure REST API, or hello Azure portal.</span></span> <span data-ttu-id="e0cd5-298">Stegvisa instruktioner för varje metod beskrivs i hello [aktivitet åtgärder med Resource Manager](../azure-resource-manager/resource-group-audit.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-298">Step-by-step instructions for each method are detailed in hello [Activity operations with Resource Manager](../azure-resource-manager/resource-group-audit.md) article.</span></span>
* <span data-ttu-id="e0cd5-299">**Power BI**: Om du inte redan har en [Power BI](https://powerbi.microsoft.com/pricing) konto, du kan testa det gratis.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-299">**Power BI**: If you don't already have a [Power BI](https://powerbi.microsoft.com/pricing) account, you can try it for free.</span></span> <span data-ttu-id="e0cd5-300">Med hjälp av hello [Azure aktivitetsloggar content pack för Power BI](https://powerbi.microsoft.com/en-us/documentation/powerbi-content-pack-azure-audit-logs/), du kan analysera dina data med förkonfigurerade instrumentpaneler som du kan använda eller anpassa.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-300">By using hello [Azure Activity Logs content pack for Power BI](https://powerbi.microsoft.com/en-us/documentation/powerbi-content-pack-azure-audit-logs/), you can analyze your data with preconfigured dashboards that you can use as is or customize.</span></span>

### <a name="view-and-analyze-hello-access-performance-and-firewall-logs"></a><span data-ttu-id="e0cd5-301">Visa och analysera hello åtkomst, prestanda och brandväggsloggar</span><span class="sxs-lookup"><span data-stu-id="e0cd5-301">View and analyze hello access, performance, and firewall logs</span></span>

<span data-ttu-id="e0cd5-302">Azure [logganalys](../log-analytics/log-analytics-azure-networking-analytics.md) kan samla in hello räknare och händelseloggen filer från Blob storage-konto.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-302">Azure [Log Analytics](../log-analytics/log-analytics-azure-networking-analytics.md) can collect hello counter and event log files from your Blob storage account.</span></span> <span data-ttu-id="e0cd5-303">Det innehåller visualiseringar och kraftfull Sök funktioner tooanalyze loggarna.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-303">It includes visualizations and powerful search capabilities tooanalyze your logs.</span></span>

<span data-ttu-id="e0cd5-304">Du kan också ansluta tooyour storage-konto och hämta hello JSON-loggposter för åtkomst och Prestandaloggar.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-304">You can also connect tooyour storage account and retrieve hello JSON log entries for access and performance logs.</span></span> <span data-ttu-id="e0cd5-305">När du har hämtat hello JSON-filer kan du konvertera dem tooCSV och visa dem i Excel, Power BI eller något annat verktyg för visualisering av data.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-305">After you download hello JSON files, you can convert them tooCSV and view them in Excel, Power BI, or any other data-visualization tool.</span></span>

> [!TIP]
> <span data-ttu-id="e0cd5-306">Om du är bekant med Visual Studio och grundläggande begrepp för att ändra värdena för variabler i C# och konstanter, kan du använda hello [logga konverteraren verktyg](https://github.com/Azure-Samples/networking-dotnet-log-converter) tillgängliga från GitHub.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-306">If you are familiar with Visual Studio and basic concepts of changing values for constants and variables in C#, you can use hello [log converter tools](https://github.com/Azure-Samples/networking-dotnet-log-converter) available from GitHub.</span></span>
> 
> 

## <a name="metrics"></a><span data-ttu-id="e0cd5-307">Mått</span><span class="sxs-lookup"><span data-stu-id="e0cd5-307">Metrics</span></span>

<span data-ttu-id="e0cd5-308">Mått är en funktion för vissa Azure-resurser där du kan visa prestandaräknare i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-308">Metrics are a feature for certain Azure resources where you can view performance counters in hello portal.</span></span> <span data-ttu-id="e0cd5-309">För Programgateway är ett enskilt mått tillgängligt nu.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-309">For Application Gateway, one metric is available now.</span></span> <span data-ttu-id="e0cd5-310">Det här måttet är genomflöde och du kan se den i hello portal.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-310">This metric is throughput, and you can see it in hello portal.</span></span> <span data-ttu-id="e0cd5-311">Bläddra tooan Programgateway och klicka på **mått**.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-311">Browse tooan application gateway and click **Metrics**.</span></span> <span data-ttu-id="e0cd5-312">tooview hello värden, Välj genomflöde i hello **tillgängliga mått** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-312">tooview hello values, select throughput in hello **Available metrics** section.</span></span> <span data-ttu-id="e0cd5-313">I följande bild hello, kan du se ett exempel med hello filter som du kan använda toodisplay hello data i olika tidsintervall.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-313">In hello following image, you can see an example with hello filters that you can use toodisplay hello data in different time ranges.</span></span>

![Måttvy med filter][5]

<span data-ttu-id="e0cd5-315">toosee en aktuell lista över statistik, se [stöds mått med Azure-Monitor](../monitoring-and-diagnostics/monitoring-supported-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="e0cd5-315">toosee a current list of metrics, see [Supported metrics with Azure Monitor](../monitoring-and-diagnostics/monitoring-supported-metrics.md).</span></span>

### <a name="alert-rules"></a><span data-ttu-id="e0cd5-316">Aviseringsregler</span><span class="sxs-lookup"><span data-stu-id="e0cd5-316">Alert rules</span></span>

<span data-ttu-id="e0cd5-317">Du kan starta Varningsregler baserat på mått för en resurs.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-317">You can start alert rules based on metrics for a resource.</span></span> <span data-ttu-id="e0cd5-318">En avisering kan anropa en webhook eller e-administratör om hello dataflödet för hello Programgateway är över, under eller med ett tröskelvärde för en angiven period.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-318">For example, an alert can call a webhook or email an administrator if hello throughput of hello application gateway is above, below, or at a threshold for a specified period.</span></span>

<span data-ttu-id="e0cd5-319">följande exempel hello vägleder dig genom att skapa en aviseringsregel som skickar en e-tooan administratör efter genomströmning överträdelser en tröskel:</span><span class="sxs-lookup"><span data-stu-id="e0cd5-319">hello following example walks you through creating an alert rule that sends an email tooan administrator after throughput breaches a threshold:</span></span>

1. <span data-ttu-id="e0cd5-320">Klicka på **Lägg till mått avisering** tooopen hello **Lägg till regel** bladet.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-320">Click **Add metric alert** tooopen hello **Add rule** blade.</span></span> <span data-ttu-id="e0cd5-321">Du kan också nå det här bladet från hello mått bladet.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-321">You can also reach this blade from hello metrics blade.</span></span>

   ![Knappen ”Lägg till mått aviseringen”][6]

2. <span data-ttu-id="e0cd5-323">På hello **Lägg till regel** bladet fylla hello namn, tillstånd, meddela avsnitt och klickar på **OK**.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-323">On hello **Add rule** blade, fill out hello name, condition, and notify sections, and click **OK**.</span></span>

   * <span data-ttu-id="e0cd5-324">I hello **villkoret** selector, Välj ett av hello fyra värden: **större än**, **större än eller lika med**, **mindre än**, eller  **Mindre än eller lika med**.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-324">In hello **Condition** selector, select one of hello four values: **Greater than**, **Greater than or equal**, **Less than**, or **Less than or equal to**.</span></span>

   * <span data-ttu-id="e0cd5-325">I hello **Period** selector, Välj en tid från 5 minuter too6 timmar.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-325">In hello **Period** selector, select a period from 5 minutes too6 hours.</span></span>

   * <span data-ttu-id="e0cd5-326">Om du väljer **e-ägare, deltagare och läsare**, hello e-post kan vara dynamiska baserat på hello-användare som har åtkomst toothat resurs.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-326">If you select **Email owners, contributors, and readers**, hello email can be dynamic based on hello users who have access toothat resource.</span></span> <span data-ttu-id="e0cd5-327">Annars kan du ange en kommaavgränsad lista över användare i hello **ytterligare administratören email(s)** rutan.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-327">Otherwise, you can provide a comma-separated list of users in hello **Additional administrator email(s)** box.</span></span>

   ![Lägg till regel bladet][7]

<span data-ttu-id="e0cd5-329">Om hello tröskelvärdet överskrids, kommer ett e-postmeddelande som är liknande toohello något i hello följande bild:</span><span class="sxs-lookup"><span data-stu-id="e0cd5-329">If hello threshold is breached, an email that's similar toohello one in hello following image arrives:</span></span>

![E-post för utsatts för intrång tröskelvärde][8]

<span data-ttu-id="e0cd5-331">En lista över aviseringar visas när du skapar en avisering om mått.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-331">A list of alerts appears after you create a metric alert.</span></span> <span data-ttu-id="e0cd5-332">Den innehåller en översikt över alla hello Varningsregler.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-332">It provides an overview of all hello alert rules.</span></span>

![Listan över aviseringar och regler][9]

<span data-ttu-id="e0cd5-334">toolearn mer om aviseringar, se [få aviseringar](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).</span><span class="sxs-lookup"><span data-stu-id="e0cd5-334">toolearn more about alert notifications, see [Receive alert notifications](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).</span></span>

<span data-ttu-id="e0cd5-335">toounderstand mer om webhooks och hur du kan använda dem med aviseringar, besök [konfigurera en webhook på en Azure mått avisering](../monitoring-and-diagnostics/insights-webhooks-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="e0cd5-335">toounderstand more about webhooks and how you can use them with alerts, visit [Configure a webhook on an Azure metric alert](../monitoring-and-diagnostics/insights-webhooks-alerts.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="e0cd5-336">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e0cd5-336">Next steps</span></span>

* <span data-ttu-id="e0cd5-337">Visualisera räknare och händelseloggar med [logganalys](../log-analytics/log-analytics-azure-networking-analytics.md).</span><span class="sxs-lookup"><span data-stu-id="e0cd5-337">Visualize counter and event logs by using [Log Analytics](../log-analytics/log-analytics-azure-networking-analytics.md).</span></span>
* <span data-ttu-id="e0cd5-338">[Visualisera dina Azure-aktivitetsloggen med Power BI](http://blogs.msdn.com/b/powerbi/archive/2015/09/30/monitor-azure-audit-logs-with-power-bi.aspx) blogginlägg.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-338">[Visualize your Azure activity log with Power BI](http://blogs.msdn.com/b/powerbi/archive/2015/09/30/monitor-azure-audit-logs-with-power-bi.aspx) blog post.</span></span>
* <span data-ttu-id="e0cd5-339">[Visa och analysera Azure aktivitetsloggar i Power BI och mycket mer](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/) blogginlägg.</span><span class="sxs-lookup"><span data-stu-id="e0cd5-339">[View and analyze Azure activity logs in Power BI and more](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/) blog post.</span></span>

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
