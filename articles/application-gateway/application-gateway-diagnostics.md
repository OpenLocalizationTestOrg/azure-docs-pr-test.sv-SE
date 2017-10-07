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
# <a name="back-end-health-diagnostic-logs-and-metrics-for-application-gateway"></a>Backend-hälsotillstånd, diagnostikloggar och mått för Programgateway

Med hjälp av Azure Application Gateway kan övervaka du resurser i hello följande sätt:

* [Backend-hälsa](#back-end-health): Programgateway ger hello kapaciteten toomonitor hello hälsa hello servrar i hello backend-pooler via hello Azure-portalen och via PowerShell. Du kan också hitta hello hälsotillstånd hello backend-pooler via hello prestanda diagnostikloggar.

* [Loggar](#diagnostic-logs): loggar för prestanda, åtkomst och andra data toobe sparas eller förbrukad från en resurs för övervakning.

* [Mått](#metrics): Application Gateway har för närvarande ett enskilt mått. Mätvärdet mäter hello genomflödet i hello Programgateway i byte per sekund.

## <a name="back-end-health"></a>Backend-hälsa

Programgateway ger hello kapaciteten toomonitor hello hälsotillståndet för enskilda medlemmar av hello backend-pooler via hello-portalen, PowerShell och hello-kommandoradsgränssnittet (CLI). Du kan också hitta det totala hälsotillståndet för backend-pooler via hello prestanda diagnostikloggar. 

hello backend-hälsorapport visar hello utdata från hello Programgateway hälsa avsökningen toohello backend-instanser. När avsökning lyckas och hello tillbaka slutet kan ta emot trafik, är det felfritt. Annars anses vara felaktig.

> [!IMPORTANT]
> Om det finns en nätverkssäkerhetsgrupp (NSG) på ett Application Gateway-undernät, öppna portintervall 65503 65534 i hello Programgateway undernät för inkommande trafik. Dessa portar är obligatoriska för hello backend-hälsa API toowork.


### <a name="view-back-end-health-through-hello-portal"></a>Visa backend-hälsa hello-portalen

Backend-hälsa tillhandahålls automatiskt i hello-portalen. Välj i en befintlig Programgateway **övervakning** > **Backend hälsa**. 

Varje medlem i hello backend-pool visas på den här sidan (om det är ett nätverkskort, IP eller FQDN). Backend-namn, port, backend-HTTP-namn och hälsostatus visas. Giltiga värden för hälsostatus **felfri**, **ohälsosam**, och **okänd**.

> [!NOTE]
> Om du ser en backend-hälsostatus **okänd**, kontrollera att åtkomst toohello serverdel inte blockeras av en regel för NSG, en användardefinierad väg (UDR) eller en anpassad DNS i hello virtuellt nätverk.

![Backend-hälsa][10]

### <a name="view-back-end-health-through-powershell"></a>Visa backend-hälsa via PowerShell

hello följande PowerShell-kod visar hur tooview backend-hälsa genom att använda hello `Get-AzureRmApplicationGatewayBackendHealth` cmdlet:

```powershell
Get-AzureRmApplicationGatewayBackendHealth -Name ApplicationGateway1 -ResourceGroupName Contoso
```

### <a name="view-back-end-health-through-azure-cli-20"></a>Visa backend-hälsa genom Azure CLI 2.0

```azurecli
az network application-gateway show-backend-health --resource-group AdatumAppGatewayRG --name AdatumAppGateway
```

### <a name="results"></a>Resultat

hello följande fragment visas ett exempel på hello svar:

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

## <a name="diagnostic-logging"></a>Diagnostikloggar

Du kan använda olika typer av loggar i Azure toomanage och felsöka programgatewayer. Du kan komma åt vissa av dessa loggar hello-portalen. Alla loggar kan extraheras från Azure Blob storage och visas i olika verktyg som [logganalys](../log-analytics/log-analytics-azure-networking-analytics.md), Excel och Power BI. Mer information om hello olika typer av loggar från hello följande lista:

* **Aktivitetsloggen**: du kan använda [Azure aktivitetsloggar](../monitoring-and-diagnostics/insights-debugging-with-events.md) (hette operativa loggar och granskningsloggar) tooview alla operationer som skickats tooyour Azure-prenumeration och deras status. Aktiviteten loggposter som samlas in som standard och du kan visa dem i hello Azure-portalen.
* **Åtkomstlogg**: du kan använda den här loggen tooview Programgateway åtkomstmönster och analysera viktig information, inklusive hello anroparen-IP, begärd URL, svar svarstid, returkod och byte in och ut. En åtkomstlogg samlas in varje 300 sekunder. Den här loggfilen innehåller en post per instans av Application Gateway. hello Programgateway instans kan identifieras av hello instanceId-egenskapen.
* **Prestanda loggen**: du kan använda den här loggen tooview hur Programgateway instanser utför. Den här loggfilen innehåller prestandainformation om för varje instans, inklusive totalt antal begäranden som betjänats, genomflöde i byte, totalt antal begäranden som betjänats antalet misslyckade begäranden och felfria och feltillstånd backend-instanser. En prestandalogg som samlas in var 60: e sekund.
* **Brandväggsloggen**: du kan använda den här loggen tooview hello begäranden som loggas via identifiering eller förhindra läget för en Programgateway som är konfigurerad med hello Brandvägg för webbaserade program.

> [!NOTE]
> Loggarna är bara tillgängligt för resurser som har distribuerats i hello Azure Resource Manager-distributionsmodellen. Du kan inte använda loggar för resurser i hello klassiska distributionsmodellen. En bättre förståelse av hello två modeller, finns hello [förstå Resource Manager distribution och klassisk distribution](../azure-resource-manager/resource-manager-deployment-model.md) artikel.

Det finns tre alternativ för att lagra dina loggar:

* **Lagringskontot**: Storage-konton är bäst för loggar när loggar lagras under en längre tid och granskas vid behov.
* **Händelsehubbar**: händelsehubbar är ett bra alternativ för att integrera med andra säkerhetsinformation och händelse (SEIM)-hanteringsverktygen tooget aviseringar på dina resurser.
* **Logga Analytics**: logganalys passar bäst för allmän övervakning i realtid för programmet eller tittar på trender.

### <a name="enable-logging-through-powershell"></a>Aktivera loggning med PowerShell

Aktivitetsloggning aktiveras automatiskt för varje resurs för hanteraren för filserverresurser. Du måste aktivera åtkomst och prestanda loggning toostart samlar in hello-data som är tillgängliga via dessa loggar. tooenable loggning, Använd hello följande steg:

1. Observera ditt lagringskonto resurs-ID, där hello loggdata lagras. Det här värdet är formatet hello: /subscriptions/\<subscriptionId\>/resourceGroups/\<resursgruppnamn\>/providers/Microsoft.Storage/storageAccounts/\<lagringskontonamnet\>. Du kan använda storage-konto i din prenumeration. Du kan använda hello Azure portal toofind informationen.

    ![Portal: resurs-ID för lagringskontot](./media/application-gateway-diagnostics/diagnostics1.png)

2. Observera din Programgateway resurs-ID som har aktiverats. Det här värdet är formatet hello: /subscriptions/\<subscriptionId\>/resourceGroups/\<resursgruppnamn\>/providers/Microsoft.Network/applicationGateways/\<Programgateway namnet\>. Du kan använda hello portal toofind informationen.

    ![Portal: resurs-ID för Programgateway](./media/application-gateway-diagnostics/diagnostics2.png)

3. Aktivera diagnostikloggning med hello följande PowerShell-cmdlet:

    ```powershell
    Set-AzureRmDiagnosticSetting  -ResourceId /subscriptions/<subscriptionId>/resourceGroups/<resource group name>/providers/Microsoft.Network/applicationGateways/<application gateway name> -StorageAccountId /subscriptions/<subscriptionId>/resourceGroups/<resource group name>/providers/Microsoft.Storage/storageAccounts/<storage account name> -Enabled $true     
    ```
    
> [!TIP] 
>Aktivitetsloggar kräver inte ett separat lagringskonto. hello användningen av lagringsutrymme för åtkomst- och prestandaloggning debiteras tjänsten.

### <a name="enable-logging-through-hello-azure-portal"></a>Aktivera loggning via hello Azure-portalen

1. I hello Azure-portalen, söka efter resurs och klickar på **diagnostikloggar**.

   För Programgateway är tre loggarna tillgängliga:

   * Åtkomstlogg
   * Prestandalogg
   * Brandväggen logg

2. toostart att samla in data, klickar du på **aktivera diagnostiken**.

   ![Aktivera diagnostik][1]

3. Hej **diagnostikinställningar** bladet innehåller hello inställningar för hello diagnostikloggar. I det här exemplet lagras logganalys hello loggar. Klicka på **konfigurera** under **logganalys** tooconfigure din arbetsyta. Du kan också använda händelsehubbar och en storage-konto toosave hello diagnostikloggar.

   ![Startar hello konfiguration][2]

4. Välj en befintlig Operations Management Suite (OMS) arbetsyta eller skapa en ny. Det här exemplet använder en befintlig.

   ![Alternativ för OMS arbetsytor][3]

5. Bekräfta inställningarna för hello och klicka på **spara**.

   ![Diagnostik inställningsbladet med val][4]

### <a name="activity-log"></a>Aktivitetslogg

Azure genererar hello aktivitetsloggen som standard. hello loggar bevaras under 90 dagar i hello Azure händelseloggar store. Mer information om de här loggarna genom att läsa hello [visa händelser och aktivitetsloggen](../monitoring-and-diagnostics/insights-debugging-with-events.md) artikel.

### <a name="access-log"></a>Åtkomstlogg

hello åtkomst loggen genereras bara om du har aktiverat på varje Application Gateway-instans enligt anvisningarna i föregående steg hello. hello data lagras i hello storage-konto som du angav när du har aktiverat hello loggning. Varje åtkomst för Programgateway loggas i JSON-format, som visas i följande exempel hello:


|Värde  |Beskrivning  |
|---------|---------|
|InstanceId     | Programgateway instansen hanteras hello begäran.        |
|ClientIP     | Ursprungliga IP-Adressen för hello-begäran.        |
|clientPort     | Ursprungliga porten för hello-begäran.       |
|HttpMethod     | HTTP-metod som används av hello-begäran.       |
|requestUri     | URI för hello tog emot begäran.        |
|RequestQuery     | **Server-dirigerat**: backend-pool-instans som har skickats hello begäran. </br> **X-AzureApplicationGateway-LOG-ID**: Korrelations-ID som används för hello-begäran. Det kan vara problem med används tootroubleshoot trafik på hello backend-servrar. </br>**SERVER-STATUS**: HTTP-svarskoden Programgateway togs emot från hello serverdel.       |
|UserAgent     | Användaragent från hello HTTP huvudet i begäran.        |
|httpStatus     | HTTP-statuskod returneras toohello klienten från Application Gateway.       |
|httpVersion     | HTTP-versionen av hello-begäran.        |
|receivedBytes     | Storleken på paketet tas emot, i byte.        |
|sentBytes| Storleken på paketet skickas, i byte.|
|timeTaken| Lång tid (i millisekunder) som det tar för en begäran toobe bearbetas och dess svar toobe skickas. Det här beräknas som hello intervall från hello tid när Programgateway får hello första byten för en HTTP-begäran toohello tid när hello svar skickar åtgärden har slutförts. Det är viktigt toonote som vanligtvis hello Time-Taken fältet innehåller hello tid som begäran och svar hälsningspaket reser hello nätverket. |
|sslEnabled| Om kommunikation toohello backend-pooler används SSL. Giltiga värden är på och av.|
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

### <a name="performance-log"></a>Prestandalogg

hello prestanda loggen genereras bara om du har aktiverat på varje Application Gateway-instans enligt anvisningarna i föregående steg hello. hello data lagras i hello storage-konto som du angav när du har aktiverat hello loggning. loggdata för prestanda för hello genereras på 1 minut. hello följande data loggas:


|Värde  |Beskrivning  |
|---------|---------|
|InstanceId     |  Programmet Gateway-instans för vilka data som genereras. För en gateway med flera instans programmet finns en rad per instans.        |
|healthyHostCount     | Antalet felfri värdar i hello backend-adresspool.        |
|unHealthyHostCount     | Antal felaktiga värdar i hello backend-adresspool.        |
|RequestCount     | Antal begäranden som betjänats.        |
|Svarstid | Svarstid (i millisekunder) för förfrågningar från hello instans toohello serverdel som fungerar hello begäranden. |
|failedRequestCount| Antal misslyckade begäranden.|
|Dataflöde| Genomsnittlig genomströmning eftersom hello senaste loggen, mätt i byte per sekund.|

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
> Svarstiden är beräknad hello tidpunkt då hello första byten för hello HTTP-begäran är mottagna toohello tid då hello sista byten av hello HTTP-svar skickas. Dess hello summan av hello Programgateway bearbetningstid plus hello nätverket kostnad toohello tillbaka avsluta plus hello länge hello serverdel tar tooprocess hello begäran.

### <a name="firewall-log"></a>Brandväggen logg

hello brandväggsloggen genereras bara om du har aktiverat för varje Programgateway enligt anvisningarna i föregående steg hello. Den här loggfilen kräver också Brandvägg för webbaserade program som hello har konfigurerats på en Programgateway. hello data lagras i hello storage-konto som du angav när du har aktiverat hello loggning. hello följande data loggas:


|Värde  |Beskrivning  |
|---------|---------|
|InstanceId     | Programmet Gateway-instans för vilken brandvägg genereras data. För en gateway med flera instans programmet finns en rad per instans.         |
|clientIp     |   Ursprungliga IP-Adressen för hello-begäran.      |
|clientPort     |  Ursprungliga porten för hello-begäran.       |
|requestUri     | URL till hello tog emot begäran.       |
|ruleSetType     | Typen regeluppsättning. hello tillgängliga värde är OWASP.        |
|ruleSetVersion     | Regeluppsättning version som används. Tillgängliga värden är 2.2.9 och 3.0.     |
|ruleId     | Regel-ID för hello som utlöser händelsen.        |
|Meddelande     | Användarvänligt meddelande för hello som utlöser händelsen. Mer information finns i avsnittet hello.        |
|Åtgärden     |  Åtgärd för hello-begäran. Tillgängliga värden är blockerad och tillåtna.      |
|plats     | Plats för vilken hello loggen skapades. Endast globala visas för närvarande eftersom regler är globala.|
|Information     | Information om hello som utlöser händelsen.        |
|details.Message     | Beskrivning av hello regeln.        |
|details.data     | Specifika data hittades i begäran matchade hello regeln.         |
|details.File     | Konfigurationsfil som innehåller hello regeln.        |
|details.Line     | Radnumret i hello-konfigurationsfilen som utlöste hello-händelse.       |

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

### <a name="view-and-analyze-hello-activity-log"></a>Visa och analysera hello aktivitetsloggen

Du kan visa och analysera loggdata för aktiviteten med hjälp av någon av följande metoder hello:

* **Azure-verktyg**: hämta information från hello aktivitetsloggen via Azure PowerShell, hello Azure CLI, hello Azure REST API eller hello Azure-portalen. Stegvisa instruktioner för varje metod beskrivs i hello [aktivitet åtgärder med Resource Manager](../azure-resource-manager/resource-group-audit.md) artikel.
* **Power BI**: Om du inte redan har en [Power BI](https://powerbi.microsoft.com/pricing) konto, du kan testa det gratis. Med hjälp av hello [Azure aktivitetsloggar content pack för Power BI](https://powerbi.microsoft.com/en-us/documentation/powerbi-content-pack-azure-audit-logs/), du kan analysera dina data med förkonfigurerade instrumentpaneler som du kan använda eller anpassa.

### <a name="view-and-analyze-hello-access-performance-and-firewall-logs"></a>Visa och analysera hello åtkomst, prestanda och brandväggsloggar

Azure [logganalys](../log-analytics/log-analytics-azure-networking-analytics.md) kan samla in hello räknare och händelseloggen filer från Blob storage-konto. Det innehåller visualiseringar och kraftfull Sök funktioner tooanalyze loggarna.

Du kan också ansluta tooyour storage-konto och hämta hello JSON-loggposter för åtkomst och Prestandaloggar. När du har hämtat hello JSON-filer kan du konvertera dem tooCSV och visa dem i Excel, Power BI eller något annat verktyg för visualisering av data.

> [!TIP]
> Om du är bekant med Visual Studio och grundläggande begrepp för att ändra värdena för variabler i C# och konstanter, kan du använda hello [logga konverteraren verktyg](https://github.com/Azure-Samples/networking-dotnet-log-converter) tillgängliga från GitHub.
> 
> 

## <a name="metrics"></a>Mått

Mått är en funktion för vissa Azure-resurser där du kan visa prestandaräknare i hello-portalen. För Programgateway är ett enskilt mått tillgängligt nu. Det här måttet är genomflöde och du kan se den i hello portal. Bläddra tooan Programgateway och klicka på **mått**. tooview hello värden, Välj genomflöde i hello **tillgängliga mått** avsnitt. I följande bild hello, kan du se ett exempel med hello filter som du kan använda toodisplay hello data i olika tidsintervall.

![Måttvy med filter][5]

toosee en aktuell lista över statistik, se [stöds mått med Azure-Monitor](../monitoring-and-diagnostics/monitoring-supported-metrics.md).

### <a name="alert-rules"></a>Aviseringsregler

Du kan starta Varningsregler baserat på mått för en resurs. En avisering kan anropa en webhook eller e-administratör om hello dataflödet för hello Programgateway är över, under eller med ett tröskelvärde för en angiven period.

följande exempel hello vägleder dig genom att skapa en aviseringsregel som skickar en e-tooan administratör efter genomströmning överträdelser en tröskel:

1. Klicka på **Lägg till mått avisering** tooopen hello **Lägg till regel** bladet. Du kan också nå det här bladet från hello mått bladet.

   ![Knappen ”Lägg till mått aviseringen”][6]

2. På hello **Lägg till regel** bladet fylla hello namn, tillstånd, meddela avsnitt och klickar på **OK**.

   * I hello **villkoret** selector, Välj ett av hello fyra värden: **större än**, **större än eller lika med**, **mindre än**, eller  **Mindre än eller lika med**.

   * I hello **Period** selector, Välj en tid från 5 minuter too6 timmar.

   * Om du väljer **e-ägare, deltagare och läsare**, hello e-post kan vara dynamiska baserat på hello-användare som har åtkomst toothat resurs. Annars kan du ange en kommaavgränsad lista över användare i hello **ytterligare administratören email(s)** rutan.

   ![Lägg till regel bladet][7]

Om hello tröskelvärdet överskrids, kommer ett e-postmeddelande som är liknande toohello något i hello följande bild:

![E-post för utsatts för intrång tröskelvärde][8]

En lista över aviseringar visas när du skapar en avisering om mått. Den innehåller en översikt över alla hello Varningsregler.

![Listan över aviseringar och regler][9]

toolearn mer om aviseringar, se [få aviseringar](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).

toounderstand mer om webhooks och hur du kan använda dem med aviseringar, besök [konfigurera en webhook på en Azure mått avisering](../monitoring-and-diagnostics/insights-webhooks-alerts.md).

## <a name="next-steps"></a>Nästa steg

* Visualisera räknare och händelseloggar med [logganalys](../log-analytics/log-analytics-azure-networking-analytics.md).
* [Visualisera dina Azure-aktivitetsloggen med Power BI](http://blogs.msdn.com/b/powerbi/archive/2015/09/30/monitor-azure-audit-logs-with-power-bi.aspx) blogginlägg.
* [Visa och analysera Azure aktivitetsloggar i Power BI och mycket mer](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/) blogginlägg.

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
