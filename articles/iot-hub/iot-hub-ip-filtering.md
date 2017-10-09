---
title: aaaAzure IoT-hubb IP-anslutningsfilter | Microsoft Docs
description: "Hur toouse IP filtrering tooblock anslutningar från särskilda IP-adresser för tooyour Azure IoT-hubb. Du kan blockera anslutningar från enskilda eller intervall med IP-adresser."
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
ms.openlocfilehash: 45e5906a494561b6108895d86d6a04fc3b52b8fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-ip-filters"></a><span data-ttu-id="2374f-104">IP-filter</span><span class="sxs-lookup"><span data-stu-id="2374f-104">Use IP filters</span></span>

<span data-ttu-id="2374f-105">Säkerhet är en viktig del av alla IoT-lösning baserad på Azure IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="2374f-105">Security is an important aspect of any IoT solution based on Azure IoT Hub.</span></span> <span data-ttu-id="2374f-106">Ibland behöver tooexplicitly ange hello IP-adresser från vilka enheter kan anslutas som en del av din konfiguration.</span><span class="sxs-lookup"><span data-stu-id="2374f-106">Sometimes you need tooexplicitly specify hello IP addresses from which devices can connect as part of your security configuration.</span></span> <span data-ttu-id="2374f-107">Hej _IP-filter_ funktionen kan du tooconfigure regler för avvisa eller ta emot trafik från specifika IPv4-adresser.</span><span class="sxs-lookup"><span data-stu-id="2374f-107">hello _IP filter_ feature enables you tooconfigure rules for rejecting or accepting traffic from specific IPv4 addresses.</span></span>

## <a name="when-toouse"></a><span data-ttu-id="2374f-108">När toouse</span><span class="sxs-lookup"><span data-stu-id="2374f-108">When toouse</span></span>

<span data-ttu-id="2374f-109">Det finns två specifika användningsfall när det är användbart tooblock hello IoT-hubbslutpunkter för vissa IP-adresser:</span><span class="sxs-lookup"><span data-stu-id="2374f-109">There are two specific use-cases when it is useful tooblock hello IoT Hub endpoints for certain IP addresses:</span></span>

- <span data-ttu-id="2374f-110">IoT-hubb bör endast ta emot trafik från ett angivet IP-adresser och avvisa allt annat.</span><span class="sxs-lookup"><span data-stu-id="2374f-110">Your IoT hub should receive traffic only from a specified range of IP addresses and reject everything else.</span></span> <span data-ttu-id="2374f-111">Till exempel du använder din IoT-hubb med [Azure Express Route] toocreate privata anslutningar mellan en IoT-hubb och lokal infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="2374f-111">For example, you are using your IoT hub with [Azure Express Route] toocreate private connections between an IoT hub and your on-premises infrastructure.</span></span>
- <span data-ttu-id="2374f-112">Du måste tooreject trafik från IP-adresser som har identifierats som misstänkt av hello IoT hub-administratören.</span><span class="sxs-lookup"><span data-stu-id="2374f-112">You need tooreject traffic from IP addresses that have been identified as suspicious by hello IoT hub administrator.</span></span>

## <a name="how-filter-rules-are-applied"></a><span data-ttu-id="2374f-113">Hur filter regler</span><span class="sxs-lookup"><span data-stu-id="2374f-113">How filter rules are applied</span></span>

<span data-ttu-id="2374f-114">hello IP-filterregler tillämpas på hello IoT-hubb servicenivå.</span><span class="sxs-lookup"><span data-stu-id="2374f-114">hello IP filter rules are applied at hello IoT Hub service level.</span></span> <span data-ttu-id="2374f-115">Hello IP-filterregler gäller därför tooall anslutningar från enheter och backend-appar som använder alla protokoll som stöds.</span><span class="sxs-lookup"><span data-stu-id="2374f-115">Therefore hello IP filter rules apply tooall connections from devices and back-end apps using any supported protocol.</span></span>

<span data-ttu-id="2374f-116">Alla anslutningsförsök från en IP-adress som matchar en rejecting IP-regel i din IoT-hubb tar emot en obehörig 401 statuskoden och en beskrivning.</span><span class="sxs-lookup"><span data-stu-id="2374f-116">Any connection attempt from an IP address that matches a rejecting IP rule in your IoT hub receives an unauthorized 401 status code and description.</span></span> <span data-ttu-id="2374f-117">hello svarsmeddelande nämner inte hello IP-regeln.</span><span class="sxs-lookup"><span data-stu-id="2374f-117">hello response message does not mention hello IP rule.</span></span>

## <a name="default-setting"></a><span data-ttu-id="2374f-118">Standardinställningen</span><span class="sxs-lookup"><span data-stu-id="2374f-118">Default setting</span></span>

<span data-ttu-id="2374f-119">Som standard hello **IP-Filter** rutnät i hello portal för IoT-hubben är tom.</span><span class="sxs-lookup"><span data-stu-id="2374f-119">By default, hello **IP Filter** grid in hello portal for an IoT hub is empty.</span></span> <span data-ttu-id="2374f-120">Den här standardinställningen innebär att din hubb accepterar anslutningar IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="2374f-120">This default setting means that your hub accepts connections any IP address.</span></span> <span data-ttu-id="2374f-121">Den här standardinställningen är likvärdiga tooa regel som accepterar hello 0.0.0.0/0 IP-adressintervall.</span><span class="sxs-lookup"><span data-stu-id="2374f-121">This default setting is equivalent tooa rule that accepts hello 0.0.0.0/0 IP address range.</span></span>

![IoT-hubb standardinställningarna IP-filter][img-ip-filter-default]

## <a name="add-or-edit-an-ip-filter-rule"></a><span data-ttu-id="2374f-123">Lägg till eller redigera en regel för IP-filter</span><span class="sxs-lookup"><span data-stu-id="2374f-123">Add or edit an IP filter rule</span></span>

<span data-ttu-id="2374f-124">När du lägger till en regel för IP-filter efterfrågas hello följande värden:</span><span class="sxs-lookup"><span data-stu-id="2374f-124">When you add an IP filter rule, you are prompted for hello following values:</span></span>

- <span data-ttu-id="2374f-125">En **Regelnamn för IP-filter** som måste vara en unik, skiftlägeskänsliga, alfanumeriska sträng in too128 tecken.</span><span class="sxs-lookup"><span data-stu-id="2374f-125">An **IP filter rule name** that must be a unique, case-insensitive, alphanumeric string up too128 characters long.</span></span> <span data-ttu-id="2374f-126">Endast hello ASCII-7-bitars alfanumeriska tecken plus `{'-', ':', '/', '\', '.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '''}` accepteras.</span><span class="sxs-lookup"><span data-stu-id="2374f-126">Only hello ASCII 7-bit alphanumeric characters plus `{'-', ':', '/', '\', '.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '''}` are accepted.</span></span>
- <span data-ttu-id="2374f-127">Välj en **avvisa** eller **acceptera** som hello **åtgärd** för hello IP-filterregeln.</span><span class="sxs-lookup"><span data-stu-id="2374f-127">Select a **reject** or **accept** as hello **action** for hello IP filter rule.</span></span>
- <span data-ttu-id="2374f-128">Ange en IPv4-adress eller ett block av IP-adresser i CIDR-notation.</span><span class="sxs-lookup"><span data-stu-id="2374f-128">Provide a single IPv4 address or a block of IP addresses in CIDR notation.</span></span> <span data-ttu-id="2374f-129">Till exempel i CIDR notation 192.168.100.0/22 representerar hello 1024 IPv4-adresser från adressen 192.168.100.0 too192.168.103.255.</span><span class="sxs-lookup"><span data-stu-id="2374f-129">For example, in CIDR notation 192.168.100.0/22 represents hello 1024 IPv4 addresses from 192.168.100.0 too192.168.103.255.</span></span>

![Lägg till en IP-filter regeln tooan IoT-hubb][img-ip-filter-add-rule]

<span data-ttu-id="2374f-131">När du har sparat hello regeln visas en varning som meddelar dig om att hello uppdatering pågår.</span><span class="sxs-lookup"><span data-stu-id="2374f-131">After you save hello rule, you see an alert notifying you that hello update is in progress.</span></span>

![Meddelande om att spara en regel för IP-filter][img-ip-filter-save-new-rule]

<span data-ttu-id="2374f-133">Hej **Lägg till** alternativet inaktiveras när du når hello högst 10 IP-filterregler.</span><span class="sxs-lookup"><span data-stu-id="2374f-133">hello **Add** option is disabled when you reach hello maximum of 10 IP filter rules.</span></span>

<span data-ttu-id="2374f-134">Du kan redigera en befintlig regel genom att dubbelklicka på hello rad som innehåller hello regeln.</span><span class="sxs-lookup"><span data-stu-id="2374f-134">You can edit an existing rule by double-clicking hello row that contains hello rule.</span></span>

> [!NOTE]
> <span data-ttu-id="2374f-135">Neka IP adresser kan förhindra att andra Azure-tjänster (till exempel Azure Stream Analytics, virtuella datorer i Azure eller hello enheten Explorer i hello portal) interagerar med hello IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="2374f-135">Rejecting IP addresses can prevent other Azure Services (such as Azure Stream Analytics, Azure Virtual Machines, or hello Device Explorer in hello portal) from interacting with hello IoT hub.</span></span>

> [!WARNING]
> <span data-ttu-id="2374f-136">Om du använder Azure Stream Analytics (ASA) tooread meddelanden från en IoT-hubb med IP-filtrering, Använd hello Event Hub-kompatibelt namn och slutpunkten för din IoT-hubb i hello ASA anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="2374f-136">If you use Azure Stream Analytics (ASA) tooread messages from an IoT hub with IP filtering enabled, use hello Event Hub-compatible name and endpoint of your IoT Hub in hello ASA connection string.</span></span>

## <a name="delete-an-ip-filter-rule"></a><span data-ttu-id="2374f-137">Ta bort en regel för IP-filter</span><span class="sxs-lookup"><span data-stu-id="2374f-137">Delete an IP filter rule</span></span>

<span data-ttu-id="2374f-138">toodelete en regel för IP-filter, Välj en eller flera regler i hello rutnätet och klicka på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="2374f-138">toodelete an IP filter rule, select one or more rules in hello grid and click **Delete**.</span></span>

![Ta bort en regel för IoT-hubb IP-filter][img-ip-filter-delete-rule]

## <a name="ip-filter-rule-evaluation"></a><span data-ttu-id="2374f-140">IP-filter regeln utvärdering</span><span class="sxs-lookup"><span data-stu-id="2374f-140">IP filter rule evaluation</span></span>

<span data-ttu-id="2374f-141">IP-filterregler tillämpas i ordning och hello första regeln att matchar hello IP-adressen bestämmer hello godkänna eller avvisa åtgärd.</span><span class="sxs-lookup"><span data-stu-id="2374f-141">IP filter rules are applied in order and hello first rule that matches hello IP address determines hello accept or reject action.</span></span>

<span data-ttu-id="2374f-142">Om du vill tooaccept adresser i hello intervallet 192.168.100.0/22 och avvisa allt annat ska hello första regeln i rutnätet hello acceptera hello adressintervallet 192.168.100.0/22.</span><span class="sxs-lookup"><span data-stu-id="2374f-142">For example, if you want tooaccept addresses in hello range 192.168.100.0/22 and reject everything else, hello first rule in hello grid should accept hello address range 192.168.100.0/22.</span></span> <span data-ttu-id="2374f-143">hello nästa regel bör Ignorera alla adresser med hjälp av hello intervallet 0.0.0.0/0.</span><span class="sxs-lookup"><span data-stu-id="2374f-143">hello next rule should reject all addresses by using hello range 0.0.0.0/0.</span></span>

<span data-ttu-id="2374f-144">Du kan ändra hello ordning av IP-filter reglerna i hello rutnätet genom att klicka tre lodräta punkter hello hello början av en rad med dra och släpp.</span><span class="sxs-lookup"><span data-stu-id="2374f-144">You can change hello order of your IP filter rules in hello grid by clicking hello three vertical dots at hello start of a row and using drag and drop.</span></span>

<span data-ttu-id="2374f-145">toosave din nya IP-filtrera regel ordning, klickar du på **spara**.</span><span class="sxs-lookup"><span data-stu-id="2374f-145">toosave your new IP filter rule order, click **Save**.</span></span>

![Ändra din IoT-hubb IP filterregler hello ordning][img-ip-filter-rule-order]

## <a name="next-steps"></a><span data-ttu-id="2374f-147">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2374f-147">Next steps</span></span>

<span data-ttu-id="2374f-148">toofurther utforska hello funktionerna i IoT Hub, se:</span><span class="sxs-lookup"><span data-stu-id="2374f-148">toofurther explore hello capabilities of IoT Hub, see:</span></span>

- <span data-ttu-id="2374f-149">[Åtgärder som övervakning][lnk-monitor]</span><span class="sxs-lookup"><span data-stu-id="2374f-149">[Operations monitoring][lnk-monitor]</span></span>
- <span data-ttu-id="2374f-150">[IoT-hubb mått][lnk-metrics]</span><span class="sxs-lookup"><span data-stu-id="2374f-150">[IoT Hub metrics][lnk-metrics]</span></span>

<!-- Images -->
[img-ip-filter-default]: ./media/iot-hub-ip-filtering/ip-filter-default.png
[img-ip-filter-add-rule]: ./media/iot-hub-ip-filtering/ip-filter-add-rule.png
[img-ip-filter-save-new-rule]: ./media/iot-hub-ip-filtering/ip-filter-save-new-rule.png
[img-ip-filter-delete-rule]: ./media/iot-hub-ip-filtering/ip-filter-delete-rule.png
[img-ip-filter-rule-order]: ./media/iot-hub-ip-filtering/ip-filter-rule-order.png


<!-- Links -->

[IoT Hub developer guide]: iot-hub-devguide.md
[Azure Express Route]:  https://azure.microsoft.com/en-us/documentation/articles/expressroute-faqs/#supported-services

[lnk-monitor]: iot-hub-operations-monitoring.md
[lnk-metrics]: iot-hub-metrics.md