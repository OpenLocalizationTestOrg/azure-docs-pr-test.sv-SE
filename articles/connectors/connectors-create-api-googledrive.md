---
title: "Lägg till Google Drive-koppling i logikappar | Microsoft Docs"
description: "Översikt över Google Drive-anslutningen med REST API-parametrar"
services: 
suite: 
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: b2bcebc5-02d2-435b-b0da-ef53bc51c4b6
ms.service: multiple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/07/2016
ms.author: mandia; ladocs
ms.openlocfilehash: c066a10b33e172eb5f16eede43ec407794000c90
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-google-drive-connector"></a><span data-ttu-id="c880a-103">Kom igång med Google Drive-koppling</span><span class="sxs-lookup"><span data-stu-id="c880a-103">Get started with the Google Drive connector</span></span>
<span data-ttu-id="c880a-104">Ansluta till Google-enhet för att skapa filer och hämta rader.</span><span class="sxs-lookup"><span data-stu-id="c880a-104">Connect to Google Drive to create files, get rows, and more.</span></span> <span data-ttu-id="c880a-105">Med Google Drive kan du:</span><span class="sxs-lookup"><span data-stu-id="c880a-105">With Google Drive, you can:</span></span> 

* <span data-ttu-id="c880a-106">Skapa ditt företag flödet som baseras på de data som du får från din sökning.</span><span class="sxs-lookup"><span data-stu-id="c880a-106">Build your business flow based on the data you get from your search.</span></span> 
* <span data-ttu-id="c880a-107">Använd åtgärder för att söka igenom bilder, söka nyheter och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="c880a-107">Use actions to search images, search the news, and more.</span></span> <span data-ttu-id="c880a-108">De här åtgärderna få svar och utdata gör tillgängligt för andra åtgärder.</span><span class="sxs-lookup"><span data-stu-id="c880a-108">These actions get a response, and then make the output available for other actions.</span></span> <span data-ttu-id="c880a-109">Du kan exempelvis söka efter en video och sedan använda Twitter för att anslå som video till ett Twitter-flöde.</span><span class="sxs-lookup"><span data-stu-id="c880a-109">For example, you can search for a video, and then use Twitter to post that video to a Twitter feed.</span></span>

<span data-ttu-id="c880a-110">Du kan komma igång genom att skapa en logikapp nu, se [skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="c880a-110">You can get started by creating a logic app now, see [Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="create-the-connection-to-google-drive"></a><span data-ttu-id="c880a-111">Skapa en anslutning till Google Drive</span><span class="sxs-lookup"><span data-stu-id="c880a-111">Create the connection to Google Drive</span></span>
<span data-ttu-id="c880a-112">När du lägger till den här anslutningen dina logic apps måste du godkänna logikappar att ansluta till Google-enhet.</span><span class="sxs-lookup"><span data-stu-id="c880a-112">When you add this connector to your logic apps, you must authorize logic apps to connect to your Google Drive.</span></span>

> [!INCLUDE [Steps to create a connection to googledrive](../../includes/connectors-create-api-googledrive.md)]
> 
> 

<span data-ttu-id="c880a-113">När du har skapat anslutningen kan du ange egenskaper för Google enhet, t.ex. mappen sökvägen eller filnamnet.</span><span class="sxs-lookup"><span data-stu-id="c880a-113">After you create the connection, you enter the Google Drive properties, like the folder path or file name.</span></span> 

## <a name="connector-specific-details"></a><span data-ttu-id="c880a-114">Connector-specifik information</span><span class="sxs-lookup"><span data-stu-id="c880a-114">Connector-specific details</span></span>

<span data-ttu-id="c880a-115">Visa alla utlösare och åtgärder som definierats i swagger och även se några gränser i den [connector information](/connectors/googledrive/).</span><span class="sxs-lookup"><span data-stu-id="c880a-115">View any triggers and actions defined in the swagger, and also see any limits in the [connector details](/connectors/googledrive/).</span></span>

## <a name="more-connectors"></a><span data-ttu-id="c880a-116">Flera kopplingar</span><span class="sxs-lookup"><span data-stu-id="c880a-116">More connectors</span></span>
<span data-ttu-id="c880a-117">Gå tillbaka till den [API: er listan](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="c880a-117">Go back to the [APIs list](apis-list.md).</span></span>
