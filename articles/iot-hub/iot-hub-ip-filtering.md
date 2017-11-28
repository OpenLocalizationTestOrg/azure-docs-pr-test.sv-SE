---
title: Azure IoT-hubb IP-anslutningsfilter | Microsoft Docs
description: "Hur du använder IP-filtrering för som blockerar anslutningar från särskilda IP-adresser för dina Azure IoT-hubb. Du kan blockera anslutningar från enskilda eller intervall med IP-adresser."
services: iot-hub
documentationcenter: 
author: BeatriceOltean
manager: timlt
editor: 
ms.assetid: f833eac3-5b5f-46a7-a47b-f4f6fc927f3f
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/23/2017
ms.author: boltean
ms.openlocfilehash: 85f5f044faddd5180f0c19d3f2c235b20f6373d5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="use-ip-filters"></a><span data-ttu-id="31f8d-104">IP-filter</span><span class="sxs-lookup"><span data-stu-id="31f8d-104">Use IP filters</span></span>

<span data-ttu-id="31f8d-105">Säkerhet är en viktig del av alla IoT-lösning baserad på Azure IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="31f8d-105">Security is an important aspect of any IoT solution based on Azure IoT Hub.</span></span> <span data-ttu-id="31f8d-106">Ibland måste uttryckligen ange IP-adresser från vilka enheter kan anslutas som en del av din konfiguration.</span><span class="sxs-lookup"><span data-stu-id="31f8d-106">Sometimes you need to explicitly specify the IP addresses from which devices can connect as part of your security configuration.</span></span> <span data-ttu-id="31f8d-107">Den _IP-filter_ funktionen kan du konfigurera regler för avvisa eller ta emot trafik från specifika IPv4-adresser.</span><span class="sxs-lookup"><span data-stu-id="31f8d-107">The _IP filter_ feature enables you to configure rules for rejecting or accepting traffic from specific IPv4 addresses.</span></span>

## <a name="when-to-use"></a><span data-ttu-id="31f8d-108">När du ska använda detta</span><span class="sxs-lookup"><span data-stu-id="31f8d-108">When to use</span></span>

<span data-ttu-id="31f8d-109">Det finns två specifika användningsfall när det är praktiskt att blockera IoT-hubbslutpunkter för vissa IP-adresser:</span><span class="sxs-lookup"><span data-stu-id="31f8d-109">There are two specific use-cases when it is useful to block the IoT Hub endpoints for certain IP addresses:</span></span>

- <span data-ttu-id="31f8d-110">IoT-hubb bör endast ta emot trafik från ett angivet IP-adresser och avvisa allt annat.</span><span class="sxs-lookup"><span data-stu-id="31f8d-110">Your IoT hub should receive traffic only from a specified range of IP addresses and reject everything else.</span></span> <span data-ttu-id="31f8d-111">Till exempel du använder din IoT-hubb med [Azure Express Route] att skapa privata anslutningar mellan en IoT-hubb och lokal infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="31f8d-111">For example, you are using your IoT hub with [Azure Express Route] to create private connections between an IoT hub and your on-premises infrastructure.</span></span>
- <span data-ttu-id="31f8d-112">Behöver du avvisa trafik från IP-adresser som har identifierats som misstänkt av IoT hub-administratör.</span><span class="sxs-lookup"><span data-stu-id="31f8d-112">You need to reject traffic from IP addresses that have been identified as suspicious by the IoT hub administrator.</span></span>

## <a name="how-filter-rules-are-applied"></a><span data-ttu-id="31f8d-113">Hur filter regler</span><span class="sxs-lookup"><span data-stu-id="31f8d-113">How filter rules are applied</span></span>

<span data-ttu-id="31f8d-114">IP-filterregler tillämpas på tjänstnivå IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="31f8d-114">The IP filter rules are applied at the IoT Hub service level.</span></span> <span data-ttu-id="31f8d-115">Därför tillämpas IP-filterregler på alla anslutningar från enheter och backend-appar som använder alla protokoll som stöds.</span><span class="sxs-lookup"><span data-stu-id="31f8d-115">Therefore the IP filter rules apply to all connections from devices and back-end apps using any supported protocol.</span></span>

<span data-ttu-id="31f8d-116">Alla anslutningsförsök från en IP-adress som matchar en rejecting IP-regel i din IoT-hubb tar emot en obehörig 401 statuskoden och en beskrivning.</span><span class="sxs-lookup"><span data-stu-id="31f8d-116">Any connection attempt from an IP address that matches a rejecting IP rule in your IoT hub receives an unauthorized 401 status code and description.</span></span> <span data-ttu-id="31f8d-117">Svarsmeddelandet nämner inte IP-regeln.</span><span class="sxs-lookup"><span data-stu-id="31f8d-117">The response message does not mention the IP rule.</span></span>

## <a name="default-setting"></a><span data-ttu-id="31f8d-118">Standardinställningen</span><span class="sxs-lookup"><span data-stu-id="31f8d-118">Default setting</span></span>

<span data-ttu-id="31f8d-119">Som standard den **IP-Filter** rutnätet för en IoT-hubb i portalen är tomt.</span><span class="sxs-lookup"><span data-stu-id="31f8d-119">By default, the **IP Filter** grid in the portal for an IoT hub is empty.</span></span> <span data-ttu-id="31f8d-120">Den här standardinställningen innebär att din hubb accepterar anslutningar IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="31f8d-120">This default setting means that your hub accepts connections any IP address.</span></span> <span data-ttu-id="31f8d-121">Den här standardinställningen motsvarar en regel som accepterar 0.0.0.0/0 IP-adressintervall.</span><span class="sxs-lookup"><span data-stu-id="31f8d-121">This default setting is equivalent to a rule that accepts the 0.0.0.0/0 IP address range.</span></span>

![IoT-hubb standardinställningarna IP-filter][img-ip-filter-default]

## <a name="add-or-edit-an-ip-filter-rule"></a><span data-ttu-id="31f8d-123">Lägg till eller redigera en regel för IP-filter</span><span class="sxs-lookup"><span data-stu-id="31f8d-123">Add or edit an IP filter rule</span></span>

<span data-ttu-id="31f8d-124">När du lägger till en regel för IP-filter tillfrågas du om följande värden:</span><span class="sxs-lookup"><span data-stu-id="31f8d-124">When you add an IP filter rule, you are prompted for the following values:</span></span>

- <span data-ttu-id="31f8d-125">En **Regelnamn för IP-filter** som måste vara en unik, skiftlägeskänsliga, alfanumeriska sträng upp till 128 tecken.</span><span class="sxs-lookup"><span data-stu-id="31f8d-125">An **IP filter rule name** that must be a unique, case-insensitive, alphanumeric string up to 128 characters long.</span></span> <span data-ttu-id="31f8d-126">Endast de ASCII-7-bitars alfanumeriska tecken plus `{'-', ':', '/', '\', '.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '''}` accepteras.</span><span class="sxs-lookup"><span data-stu-id="31f8d-126">Only the ASCII 7-bit alphanumeric characters plus `{'-', ':', '/', '\', '.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '''}` are accepted.</span></span>
- <span data-ttu-id="31f8d-127">Välj en **avvisa** eller **acceptera** som den **åtgärd** för IP-filterregeln.</span><span class="sxs-lookup"><span data-stu-id="31f8d-127">Select a **reject** or **accept** as the **action** for the IP filter rule.</span></span>
- <span data-ttu-id="31f8d-128">Ange en IPv4-adress eller ett block av IP-adresser i CIDR-notation.</span><span class="sxs-lookup"><span data-stu-id="31f8d-128">Provide a single IPv4 address or a block of IP addresses in CIDR notation.</span></span> <span data-ttu-id="31f8d-129">Till exempel i CIDR representerar notation 192.168.100.0/22 1024 IPv4-adresser från adressen 192.168.100.0 192.168.103.255.</span><span class="sxs-lookup"><span data-stu-id="31f8d-129">For example, in CIDR notation 192.168.100.0/22 represents the 1024 IPv4 addresses from 192.168.100.0 to 192.168.103.255.</span></span>

![Lägga till en regel för IP-filter i en IoT-hubb][img-ip-filter-add-rule]

<span data-ttu-id="31f8d-131">När du har sparat regeln kan se du en avisering om att uppdateringen pågår.</span><span class="sxs-lookup"><span data-stu-id="31f8d-131">After you save the rule, you see an alert notifying you that the update is in progress.</span></span>

![Meddelande om att spara en regel för IP-filter][img-ip-filter-save-new-rule]

<span data-ttu-id="31f8d-133">Den **Lägg till** alternativet inaktiveras när du når högst 10 IP-filterregler.</span><span class="sxs-lookup"><span data-stu-id="31f8d-133">The **Add** option is disabled when you reach the maximum of 10 IP filter rules.</span></span>

<span data-ttu-id="31f8d-134">Du kan redigera en befintlig regel genom att dubbelklicka på den rad som innehåller regeln.</span><span class="sxs-lookup"><span data-stu-id="31f8d-134">You can edit an existing rule by double-clicking the row that contains the rule.</span></span>

> [!NOTE]
> <span data-ttu-id="31f8d-135">Neka IP adresser kan förhindra att andra Azure-tjänster (till exempel Azure Stream Analytics, Azure virtuella datorer eller enheter Explorer i portalen) interagera med IoT-hubben.</span><span class="sxs-lookup"><span data-stu-id="31f8d-135">Rejecting IP addresses can prevent other Azure Services (such as Azure Stream Analytics, Azure Virtual Machines, or the Device Explorer in the portal) from interacting with the IoT hub.</span></span>

> [!WARNING]
> <span data-ttu-id="31f8d-136">Om du använder Azure Stream Analytics (ASA) för att läsa meddelanden från en IoT-hubb med IP-filtrering, Använd Event Hub-kompatibelt namn och slutpunkten för din IoT-hubb i anslutningssträngen ASA.</span><span class="sxs-lookup"><span data-stu-id="31f8d-136">If you use Azure Stream Analytics (ASA) to read messages from an IoT hub with IP filtering enabled, use the Event Hub-compatible name and endpoint of your IoT Hub in the ASA connection string.</span></span>

## <a name="delete-an-ip-filter-rule"></a><span data-ttu-id="31f8d-137">Ta bort en regel för IP-filter</span><span class="sxs-lookup"><span data-stu-id="31f8d-137">Delete an IP filter rule</span></span>

<span data-ttu-id="31f8d-138">Välj en eller flera regler i rutnätet och klicka om du vill ta bort en regel för IP-filter, **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="31f8d-138">To delete an IP filter rule, select one or more rules in the grid and click **Delete**.</span></span>

![Ta bort en regel för IoT-hubb IP-filter][img-ip-filter-delete-rule]

## <a name="ip-filter-rule-evaluation"></a><span data-ttu-id="31f8d-140">IP-filter regeln utvärdering</span><span class="sxs-lookup"><span data-stu-id="31f8d-140">IP filter rule evaluation</span></span>

<span data-ttu-id="31f8d-141">IP-filterregler tillämpas i ordning och den första regeln som matchar IP-adressen bestämmer åtgärden Godkänn eller avvisa.</span><span class="sxs-lookup"><span data-stu-id="31f8d-141">IP filter rules are applied in order and the first rule that matches the IP address determines the accept or reject action.</span></span>

<span data-ttu-id="31f8d-142">Om du vill acceptera adresser i intervallet 192.168.100.0/22 och avvisa allt annat ska den första regeln i rutnätet acceptera adressintervallet 192.168.100.0/22.</span><span class="sxs-lookup"><span data-stu-id="31f8d-142">For example, if you want to accept addresses in the range 192.168.100.0/22 and reject everything else, the first rule in the grid should accept the address range 192.168.100.0/22.</span></span> <span data-ttu-id="31f8d-143">Nästa regel bör Ignorera alla adresser med hjälp av intervallet 0.0.0.0/0.</span><span class="sxs-lookup"><span data-stu-id="31f8d-143">The next rule should reject all addresses by using the range 0.0.0.0/0.</span></span>

<span data-ttu-id="31f8d-144">Du kan ändra ordning på IP-filter reglerna i rutnätet genom att klicka tre lodräta punkter i början av en rad med dra och släpp.</span><span class="sxs-lookup"><span data-stu-id="31f8d-144">You can change the order of your IP filter rules in the grid by clicking the three vertical dots at the start of a row and using drag and drop.</span></span>

<span data-ttu-id="31f8d-145">Om du vill spara din nya Regelordningen för IP-filter, klickar du på **spara**.</span><span class="sxs-lookup"><span data-stu-id="31f8d-145">To save your new IP filter rule order, click **Save**.</span></span>

![Ändra ordning på din IoT-hubb IP filterregler][img-ip-filter-rule-order]

## <a name="next-steps"></a><span data-ttu-id="31f8d-147">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="31f8d-147">Next steps</span></span>

<span data-ttu-id="31f8d-148">Om du vill utforska ytterligare funktionerna i IoT-hubb, se:</span><span class="sxs-lookup"><span data-stu-id="31f8d-148">To further explore the capabilities of IoT Hub, see:</span></span>

- <span data-ttu-id="31f8d-149">[Åtgärder som övervakning][lnk-monitor]</span><span class="sxs-lookup"><span data-stu-id="31f8d-149">[Operations monitoring][lnk-monitor]</span></span>
- <span data-ttu-id="31f8d-150">[IoT-hubb mått][lnk-metrics]</span><span class="sxs-lookup"><span data-stu-id="31f8d-150">[IoT Hub metrics][lnk-metrics]</span></span>

<!-- Images -->
[img-ip-filter-default]: ./media/iot-hub-ip-filtering/ip-filter-default.png
[img-ip-filter-add-rule]: ./media/iot-hub-ip-filtering/ip-filter-add-rule.png
[img-ip-filter-save-new-rule]: ./media/iot-hub-ip-filtering/ip-filter-save-new-rule.png
[img-ip-filter-delete-rule]: ./media/iot-hub-ip-filtering/ip-filter-delete-rule.png
[img-ip-filter-rule-order]: ./media/iot-hub-ip-filtering/ip-filter-rule-order.png


<!-- Links -->

[IoT Hub developer guide]: iot-hub-devguide.md
<span data-ttu-id="31f8d-151">[Azure Express Route]:  https://azure.microsoft.com/en-us/documentation/articles/expressroute-faqs/#supported-services</span><span class="sxs-lookup"><span data-stu-id="31f8d-151">[Azure Express Route]:  https://azure.microsoft.com/en-us/documentation/articles/expressroute-faqs/#supported-services</span></span>

[lnk-monitor]: iot-hub-operations-monitoring.md
[lnk-metrics]: iot-hub-metrics.md