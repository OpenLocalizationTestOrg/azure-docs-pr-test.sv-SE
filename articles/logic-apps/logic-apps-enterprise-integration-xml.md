---
title: "aaaWorking med XML-meddelanden i dina arbetsflöden - Azure Logic Apps | Microsoft Docs"
description: "Bearbeta, verifiera, transformera och utöka XML meddelanden i logikappar och business-tooscenarios med hello Enterprise-Integrationspaket"
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
ms.openlocfilehash: f90ae89fef0a4bd17286adbce398e573940bb790
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="validate-and-transform-xml-encode-and-decode-flat-files-and-enrich-messages-features-in-logic-apps"></a><span data-ttu-id="0cc84-103">Validera och transformera XML, koda och avkoda flat-filer och utöka meddelanden funktioner i logic apps</span><span class="sxs-lookup"><span data-stu-id="0cc84-103">Validate and transform XML, encode and decode flat files, and enrich messages features in logic apps</span></span>

<span data-ttu-id="0cc84-104">Med logic apps kan ha du hello möjlighet tooprocess XML-meddelanden som du skickar och tar emot.</span><span class="sxs-lookup"><span data-stu-id="0cc84-104">Using logic apps, you have hello ability tooprocess XML messages that you send and receive.</span></span> <span data-ttu-id="0cc84-105">Den här funktionen ingår i hello Enterprise-Integrationspaket.</span><span class="sxs-lookup"><span data-stu-id="0cc84-105">This feature is included with hello Enterprise Integration Pack.</span></span> <span data-ttu-id="0cc84-106">För de användarna som har en bakgrund för BizTalk Server hello Enterprise-Integrationspaket ger liknande förmågor tootransform och validera meddelanden kan arbeta med flat-filer, och även använda XPath tooenrich eller extrahera specifika egenskaper från ett meddelande.</span><span class="sxs-lookup"><span data-stu-id="0cc84-106">For those users with a BizTalk Server background, hello Enterprise Integration Pack gives you similar abilities tootransform and validate messages, work with flat files, and even use XPath tooenrich or extract specific properties from a message.</span></span> 

<span data-ttu-id="0cc84-107">För användare som är nya toothis utrymme, utöka dessa funktioner hur du bearbetar meddelanden i arbetsflödet.</span><span class="sxs-lookup"><span data-stu-id="0cc84-107">For those users who are new toothis space, these features expand how you process messages within your workflow.</span></span> <span data-ttu-id="0cc84-108">Till exempel om du är i ett scenario med business-to-business och arbeta med specifika XML-scheman kan du använda hello Enterprise-Integrationspaket tooenhance hur företaget bearbetar dessa meddelanden.</span><span class="sxs-lookup"><span data-stu-id="0cc84-108">For example, if you are in a business-to-business scenario, and work with specific XML schemas, then you can use hello Enterprise Integration Pack tooenhance how your company processes these messages.</span></span> 

<span data-ttu-id="0cc84-109">hello innehåller Enterprise-Integrationspaket:</span><span class="sxs-lookup"><span data-stu-id="0cc84-109">hello Enterprise Integration Pack includes:</span></span> 

* <span data-ttu-id="0cc84-110">[XML-verifiering](logic-apps-enterprise-integration-xml-validation.md "Lär dig mer om XML-meddelandevalidering") -validera en XML-meddelande för inkommande eller utgående mot ett visst schema.</span><span class="sxs-lookup"><span data-stu-id="0cc84-110">[XML validation](logic-apps-enterprise-integration-xml-validation.md "Learn about XML message validation") - Validate an incoming or outgoing XML message against a specific schema.</span></span>
* <span data-ttu-id="0cc84-111">[XML-transformeringen](../logic-apps/logic-apps-enterprise-integration-transform.md "Lär dig mer om XML-meddelande omvandlingar och kartor") – konvertera eller anpassa en XML-meddelande baserat på dina krav eller hello kraven i en partner.</span><span class="sxs-lookup"><span data-stu-id="0cc84-111">[XML transform](../logic-apps/logic-apps-enterprise-integration-transform.md "Learn about XML message transformations and maps") - Convert or customize an XML message based on your requirements, or hello requirements of a partner.</span></span>
* <span data-ttu-id="0cc84-112">[En platt Filkodning och flat fil avkodning](logic-apps-enterprise-integration-flatfile.md "Lär dig mer om kodning/avkodning flat fil") – koda eller avkoda en flat-fil.</span><span class="sxs-lookup"><span data-stu-id="0cc84-112">[Flat file encoding and flat file decoding](logic-apps-enterprise-integration-flatfile.md "Learn about flat file encoding/decoding") - Encode or decode a flat file.</span></span> <span data-ttu-id="0cc84-113">Till exempel SAP accepterar och skickar IDOC filer i platt format.</span><span class="sxs-lookup"><span data-stu-id="0cc84-113">For example, SAP accepts and sends IDOC files in flat file format.</span></span> <span data-ttu-id="0cc84-114">Flera olika plattformar integration skapa XML-meddelanden, inklusive Logic Apps.</span><span class="sxs-lookup"><span data-stu-id="0cc84-114">Many integration platforms create XML messages, including Logic Apps.</span></span> <span data-ttu-id="0cc84-115">Så kan du skapa en logikapp som använder hello flat fil-kodaren för ”konvertera” XML-filer tooflat filer.</span><span class="sxs-lookup"><span data-stu-id="0cc84-115">So, you can create a logic app that uses hello flat file encoder too"convert" XML files tooflat files.</span></span> 
* <span data-ttu-id="0cc84-116">[XPath](https://msdn.microsoft.com/library/mt643789.aspx) – utöka ett meddelande och extrahera specifika egenskaper från hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="0cc84-116">[XPath](https://msdn.microsoft.com/library/mt643789.aspx) - Enrich a message and extract specific properties from hello message.</span></span> <span data-ttu-id="0cc84-117">Du kan sedan använda hello extraheras egenskaper tooroute hello meddelandet tooa mål- eller en mellanliggande slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="0cc84-117">You can then use hello extracted properties tooroute hello message tooa destination, or an intermediary endpoint.</span></span>

## <a name="try-it-out"></a><span data-ttu-id="0cc84-118">Prova</span><span class="sxs-lookup"><span data-stu-id="0cc84-118">Try it out</span></span>
<span data-ttu-id="0cc84-119">[Distribuera en fullt fungerande logikapp ](https://github.com/Azure/azure-quickstart-templates/tree/master/201-logic-app-veter-pipeline) (GitHub-prov) med hjälp av hello XML-funktionerna i Logikappar i Azure.</span><span class="sxs-lookup"><span data-stu-id="0cc84-119">[Deploy a fully operational logic app ](https://github.com/Azure/azure-quickstart-templates/tree/master/201-logic-app-veter-pipeline) (GitHub sample) by using hello XML features in Azure Logic Apps.</span></span>

## <a name="learn-more"></a><span data-ttu-id="0cc84-120">Läs mer</span><span class="sxs-lookup"><span data-stu-id="0cc84-120">Learn more</span></span>
[<span data-ttu-id="0cc84-121">Mer information om hello Enterprise-Integrationspaket</span><span class="sxs-lookup"><span data-stu-id="0cc84-121">Learn more about hello Enterprise Integration Pack</span></span>](../logic-apps/logic-apps-enterprise-integration-overview.md "Lär dig mer om Enterprise-Integrationspaket")
