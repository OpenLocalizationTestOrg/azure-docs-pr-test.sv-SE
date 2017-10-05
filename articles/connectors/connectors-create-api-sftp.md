---
title: "Lär dig hur du använder SFTP-anslutningen i dina logic apps | Microsoft Docs"
description: "Skapa logikappar med Azure App service. Anslut till SFTP API för att skicka och ta emot. Du kan utföra olika åtgärder så som att skapa, uppdatera, hämta eller ta bort filer."
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: 697eb8b0-4a66-40c7-be7b-6aa6b131c7ad
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 07/20/2016
ms.author: mandia; ladocs
ms.openlocfilehash: 31253d8daee1581167a96a20ba8ad529a04b3e92
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-sftp-connector"></a><span data-ttu-id="f9e5e-105">Kom igång med SFTP-koppling</span><span class="sxs-lookup"><span data-stu-id="f9e5e-105">Get started with the SFTP connector</span></span>
<span data-ttu-id="f9e5e-106">Använda SFTP-anslutningen för att komma åt en SFTP-konto för att skicka och ta emot filer.</span><span class="sxs-lookup"><span data-stu-id="f9e5e-106">Use the SFTP connector to access an SFTP account to send and receive files.</span></span> <span data-ttu-id="f9e5e-107">Du kan utföra olika åtgärder så som att skapa, uppdatera, hämta eller ta bort filer.</span><span class="sxs-lookup"><span data-stu-id="f9e5e-107">You can perform various operations such as create, update, get or delete files.</span></span>  

<span data-ttu-id="f9e5e-108">Att använda [alla anslutningar](apis-list.md), måste du först skapa en logikapp.</span><span class="sxs-lookup"><span data-stu-id="f9e5e-108">To use [any connector](apis-list.md), you first need to create a logic app.</span></span> <span data-ttu-id="f9e5e-109">Du kan komma igång med [att skapa en logikapp nu](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="f9e5e-109">You can get started by [creating a logic app now](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="connect-to-sftp"></a><span data-ttu-id="f9e5e-110">Anslut till SFTP</span><span class="sxs-lookup"><span data-stu-id="f9e5e-110">Connect to SFTP</span></span>
<span data-ttu-id="f9e5e-111">Innan din logikapp kan komma åt någon tjänst, måste du först skapa en *anslutning* till tjänsten.</span><span class="sxs-lookup"><span data-stu-id="f9e5e-111">Before your logic app can access any service, you first need to create a *connection* to the service.</span></span> <span data-ttu-id="f9e5e-112">En [anslutning](connectors-overview.md) tillhandahåller anslutningen mellan en logikapp och en annan tjänst.</span><span class="sxs-lookup"><span data-stu-id="f9e5e-112">A [connection](connectors-overview.md) provides connectivity between a logic app and another service.</span></span>  

### <a name="create-a-connection-to-sftp"></a><span data-ttu-id="f9e5e-113">Skapa en anslutning till SFTP</span><span class="sxs-lookup"><span data-stu-id="f9e5e-113">Create a connection to SFTP</span></span>
> [!INCLUDE [Steps to create a connection to SFTP](../../includes/connectors-create-api-sftp.md)]
> 
> 

## <a name="use-an-sftp-trigger"></a><span data-ttu-id="f9e5e-114">Använd en SFTP-utlösare</span><span class="sxs-lookup"><span data-stu-id="f9e5e-114">Use an SFTP trigger</span></span>
<span data-ttu-id="f9e5e-115">En utlösare är en händelse som kan användas för att starta arbetsflödet som definierats i en logikapp.</span><span class="sxs-lookup"><span data-stu-id="f9e5e-115">A trigger is an event that can be used to start the workflow defined in a logic app.</span></span> <span data-ttu-id="f9e5e-116">[Mer information om utlösare](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="f9e5e-116">[Learn more about triggers](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>  

<span data-ttu-id="f9e5e-117">I det här exemplet i **SFTP - när en fil har lagts till eller ändrats** utlösare används för att initiera en logik app arbetsflödet när en fil har lagts till eller ändras på en SFTP-server.</span><span class="sxs-lookup"><span data-stu-id="f9e5e-117">In this example, the **SFTP - When a file is added or modified** trigger is used to initiate a logic app workflow when a file is added to, or modified on, an SFTP server.</span></span> <span data-ttu-id="f9e5e-118">Du också lägga till ett villkor som kontrollerar du innehållet i den nya eller ändrade filen och gör ett beslut att extrahera filen om innehållet visar ska extraheras innan du använder innehållet.</span><span class="sxs-lookup"><span data-stu-id="f9e5e-118">You also add a condition that checks the contents of the new or modified file, and makes a decision to extract the file if its contents indicate that it should be extracted before using the contents.</span></span> <span data-ttu-id="f9e5e-119">Slutligen lägger du till en åtgärd för att extrahera innehållet i en fil och placerar du extraherade innehållet i en mapp på SFTP-servern.</span><span class="sxs-lookup"><span data-stu-id="f9e5e-119">Finally, add an action to extract the contents of a file, and place the extracted contents in a folder on the SFTP server.</span></span> 

<span data-ttu-id="f9e5e-120">Du kan använda den här utlösaren för att övervaka en SFTP-mapp för nya filer som representerar order i ett enterprise-exempel.</span><span class="sxs-lookup"><span data-stu-id="f9e5e-120">In an enterprise example, you could use this trigger to monitor an SFTP folder for new files that represent customer orders.</span></span>  <span data-ttu-id="f9e5e-121">Du kan sedan använda åtgärden SFTP-koppling som **hämta filinnehåll**, för att hämta innehållet i ordningen för vidare bearbetning och lagring i en order-databas.</span><span class="sxs-lookup"><span data-stu-id="f9e5e-121">You could then use an SFTP connector action, such as **Get file content**, to get the contents of the order for further processing and storage in an orders database.</span></span>

> [!INCLUDE [Steps to create an SFTP trigger](../../includes/connectors-create-api-sftp-trigger.md)]
> 
> 

## <a name="add-a-condition"></a><span data-ttu-id="f9e5e-122">Lägg till ett villkor</span><span class="sxs-lookup"><span data-stu-id="f9e5e-122">Add a condition</span></span>
> [!INCLUDE [Steps to add a condition](../../includes/connectors-create-api-sftp-condition.md)]
> 
> 

## <a name="use-an-sftp-action"></a><span data-ttu-id="f9e5e-123">Använd en SFTP-åtgärd</span><span class="sxs-lookup"><span data-stu-id="f9e5e-123">Use an SFTP action</span></span>
<span data-ttu-id="f9e5e-124">En åtgärd är en åtgärd som utförs av arbetsflödet som definierats i en logikapp.</span><span class="sxs-lookup"><span data-stu-id="f9e5e-124">An action is an operation carried out by the workflow defined in a logic app.</span></span> <span data-ttu-id="f9e5e-125">[Mer information om åtgärder](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="f9e5e-125">[Learn more about actions](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>  

> [!INCLUDE [Steps to create an SFTP action](../../includes/connectors-create-api-sftp-action.md)]
> 
> 

## <a name="connector-specific-details"></a><span data-ttu-id="f9e5e-126">Connector-specifik information</span><span class="sxs-lookup"><span data-stu-id="f9e5e-126">Connector-specific details</span></span>

<span data-ttu-id="f9e5e-127">Visa alla utlösare och åtgärder som definierats i swagger och även se några gränser i den [connector information](/connectors/sftpconnector/).</span><span class="sxs-lookup"><span data-stu-id="f9e5e-127">View any triggers and actions defined in the swagger, and also see any limits in the [connector details](/connectors/sftpconnector/).</span></span>

## <a name="more-connectors"></a><span data-ttu-id="f9e5e-128">Flera kopplingar</span><span class="sxs-lookup"><span data-stu-id="f9e5e-128">More connectors</span></span>
<span data-ttu-id="f9e5e-129">Gå tillbaka till den [API: er listan](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="f9e5e-129">Go back to the [APIs list](apis-list.md).</span></span>