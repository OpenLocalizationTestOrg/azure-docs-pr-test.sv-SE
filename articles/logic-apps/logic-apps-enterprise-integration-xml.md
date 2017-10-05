---
title: "Arbeta med XML-meddelanden i dina arbetsflöden - Azure Logic Apps | Microsoft Docs"
description: "Bearbeta, verifiera, transformera och utöka XML-meddelanden i logikappar och business-scenarier med Enterprise-Integrationspaket"
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: 47672dc4-1caa-44e5-b8cb-68ec3a76b7dc
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 02/27/2017
ms.author: LADocs; mandia
ms.openlocfilehash: 3fec4935f5317be4bf8c9e05f1c24a7c05381b1e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="validate-and-transform-xml-encode-and-decode-flat-files-and-enrich-messages-features-in-logic-apps"></a><span data-ttu-id="dc1d2-103">Validera och transformera XML, koda och avkoda flat-filer och utöka meddelanden funktioner i logic apps</span><span class="sxs-lookup"><span data-stu-id="dc1d2-103">Validate and transform XML, encode and decode flat files, and enrich messages features in logic apps</span></span>

<span data-ttu-id="dc1d2-104">Med logic apps kan ha du möjlighet att bearbeta XML-meddelanden som du skickar och tar emot.</span><span class="sxs-lookup"><span data-stu-id="dc1d2-104">Using logic apps, you have the ability to process XML messages that you send and receive.</span></span> <span data-ttu-id="dc1d2-105">Den här funktionen ingår i Enterprise-Integrationspaket.</span><span class="sxs-lookup"><span data-stu-id="dc1d2-105">This feature is included with the Enterprise Integration Pack.</span></span> <span data-ttu-id="dc1d2-106">Användare med bakgrund BizTalk Server ger Enterprise-Integrationspaket dig liknande förmåga att transformera och validera meddelanden kan arbeta med flat-filer och även använda XPath för att utöka eller extrahera specifika egenskaper från ett meddelande.</span><span class="sxs-lookup"><span data-stu-id="dc1d2-106">For those users with a BizTalk Server background, the Enterprise Integration Pack gives you similar abilities to transform and validate messages, work with flat files, and even use XPath to enrich or extract specific properties from a message.</span></span> 

<span data-ttu-id="dc1d2-107">För de användare som är nya i detta område, utöka dessa funktioner hur du bearbetar meddelanden i arbetsflödet.</span><span class="sxs-lookup"><span data-stu-id="dc1d2-107">For those users who are new to this space, these features expand how you process messages within your workflow.</span></span> <span data-ttu-id="dc1d2-108">Till exempel om du är i ett scenario med business-to-business och arbeta med specifika XML-scheman kan du använda Enterprise-Integrationspaket för att förbättra hur företaget bearbetar dessa meddelanden.</span><span class="sxs-lookup"><span data-stu-id="dc1d2-108">For example, if you are in a business-to-business scenario, and work with specific XML schemas, then you can use the Enterprise Integration Pack to enhance how your company processes these messages.</span></span> 

<span data-ttu-id="dc1d2-109">Enterprise-Integrationspaket innehåller:</span><span class="sxs-lookup"><span data-stu-id="dc1d2-109">The Enterprise Integration Pack includes:</span></span> 

* <span data-ttu-id="dc1d2-110">[XML-verifiering](logic-apps-enterprise-integration-xml-validation.md "Lär dig mer om XML-meddelandevalidering") -validera en XML-meddelande för inkommande eller utgående mot ett visst schema.</span><span class="sxs-lookup"><span data-stu-id="dc1d2-110">[XML validation](logic-apps-enterprise-integration-xml-validation.md "Learn about XML message validation") - Validate an incoming or outgoing XML message against a specific schema.</span></span>
* <span data-ttu-id="dc1d2-111">[XML-transformeringen](../logic-apps/logic-apps-enterprise-integration-transform.md "Lär dig mer om XML-meddelande omvandlingar och kartor") – konvertera eller anpassa en XML-meddelande utifrån dina behov eller kraven i en partner.</span><span class="sxs-lookup"><span data-stu-id="dc1d2-111">[XML transform](../logic-apps/logic-apps-enterprise-integration-transform.md "Learn about XML message transformations and maps") - Convert or customize an XML message based on your requirements, or the requirements of a partner.</span></span>
* <span data-ttu-id="dc1d2-112">[En platt Filkodning och flat fil avkodning](logic-apps-enterprise-integration-flatfile.md "Lär dig mer om kodning/avkodning flat fil") – koda eller avkoda en flat-fil.</span><span class="sxs-lookup"><span data-stu-id="dc1d2-112">[Flat file encoding and flat file decoding](logic-apps-enterprise-integration-flatfile.md "Learn about flat file encoding/decoding") - Encode or decode a flat file.</span></span> <span data-ttu-id="dc1d2-113">Till exempel SAP accepterar och skickar IDOC filer i platt format.</span><span class="sxs-lookup"><span data-stu-id="dc1d2-113">For example, SAP accepts and sends IDOC files in flat file format.</span></span> <span data-ttu-id="dc1d2-114">Flera olika plattformar integration skapa XML-meddelanden, inklusive Logic Apps.</span><span class="sxs-lookup"><span data-stu-id="dc1d2-114">Many integration platforms create XML messages, including Logic Apps.</span></span> <span data-ttu-id="dc1d2-115">Så kan du skapa en logikapp som använder flat fil-kodaren ”konvertera” XML-filer till flat-filer.</span><span class="sxs-lookup"><span data-stu-id="dc1d2-115">So, you can create a logic app that uses the flat file encoder to "convert" XML files to flat files.</span></span> 
* <span data-ttu-id="dc1d2-116">[XPath](https://msdn.microsoft.com/library/mt643789.aspx) – utöka ett meddelande och extrahera specifika egenskaper från meddelandet.</span><span class="sxs-lookup"><span data-stu-id="dc1d2-116">[XPath](https://msdn.microsoft.com/library/mt643789.aspx) - Enrich a message and extract specific properties from the message.</span></span> <span data-ttu-id="dc1d2-117">Du kan sedan använda de extraherade egenskaperna för att vidarebefordra meddelandet till en mål- eller en mellanliggande slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="dc1d2-117">You can then use the extracted properties to route the message to a destination, or an intermediary endpoint.</span></span>

## <a name="try-it-out"></a><span data-ttu-id="dc1d2-118">Prova</span><span class="sxs-lookup"><span data-stu-id="dc1d2-118">Try it out</span></span>
<span data-ttu-id="dc1d2-119">[Distribuera en fullt fungerande logikapp ](https://github.com/Azure/azure-quickstart-templates/tree/master/201-logic-app-veter-pipeline) (GitHub-prov) med hjälp av XML-funktionerna i Logikappar i Azure.</span><span class="sxs-lookup"><span data-stu-id="dc1d2-119">[Deploy a fully operational logic app ](https://github.com/Azure/azure-quickstart-templates/tree/master/201-logic-app-veter-pipeline) (GitHub sample) by using the XML features in Azure Logic Apps.</span></span>

## <a name="learn-more"></a><span data-ttu-id="dc1d2-120">Läs mer</span><span class="sxs-lookup"><span data-stu-id="dc1d2-120">Learn more</span></span>
[<span data-ttu-id="dc1d2-121">Mer information om Enterprise-Integrationspaket</span><span class="sxs-lookup"><span data-stu-id="dc1d2-121">Learn more about the Enterprise Integration Pack</span></span>](../logic-apps/logic-apps-enterprise-integration-overview.md "Lär dig mer om Enterprise-Integrationspaket")
