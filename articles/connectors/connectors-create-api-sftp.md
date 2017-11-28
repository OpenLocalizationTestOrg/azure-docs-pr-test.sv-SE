---
title: aaaLearn hur toouse hello SFTP connector i dina logic apps | Microsoft Docs
description: "Skapa logikappar med Azure App service. Anslut tooSFTP API toosend och ta emot filer. Du kan utföra olika åtgärder så som att skapa, uppdatera, hämta eller ta bort filer."
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
ms.openlocfilehash: 3f50570191c9b9339fe6584b9056b2549512b789
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-sftp-connector"></a><span data-ttu-id="ec452-105">Kom igång med hello SFTP-koppling</span><span class="sxs-lookup"><span data-stu-id="ec452-105">Get started with hello SFTP connector</span></span>
<span data-ttu-id="ec452-106">Använd hello SFTP connector tooaccess toosend en SFTP-konto och ta emot filer.</span><span class="sxs-lookup"><span data-stu-id="ec452-106">Use hello SFTP connector tooaccess an SFTP account toosend and receive files.</span></span> <span data-ttu-id="ec452-107">Du kan utföra olika åtgärder så som att skapa, uppdatera, hämta eller ta bort filer.</span><span class="sxs-lookup"><span data-stu-id="ec452-107">You can perform various operations such as create, update, get or delete files.</span></span>  

<span data-ttu-id="ec452-108">toouse [alla anslutningar](apis-list.md), måste du först toocreate en logikapp.</span><span class="sxs-lookup"><span data-stu-id="ec452-108">toouse [any connector](apis-list.md), you first need toocreate a logic app.</span></span> <span data-ttu-id="ec452-109">Du kan komma igång med [att skapa en logikapp nu](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="ec452-109">You can get started by [creating a logic app now](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="connect-toosftp"></a><span data-ttu-id="ec452-110">Ansluta tooSFTP</span><span class="sxs-lookup"><span data-stu-id="ec452-110">Connect tooSFTP</span></span>
<span data-ttu-id="ec452-111">Innan din logikapp kan komma åt någon tjänst, måste du först toocreate en *anslutning* toohello service.</span><span class="sxs-lookup"><span data-stu-id="ec452-111">Before your logic app can access any service, you first need toocreate a *connection* toohello service.</span></span> <span data-ttu-id="ec452-112">En [anslutning](connectors-overview.md) tillhandahåller anslutningen mellan en logikapp och en annan tjänst.</span><span class="sxs-lookup"><span data-stu-id="ec452-112">A [connection](connectors-overview.md) provides connectivity between a logic app and another service.</span></span>  

### <a name="create-a-connection-toosftp"></a><span data-ttu-id="ec452-113">Skapa en anslutning tooSFTP</span><span class="sxs-lookup"><span data-stu-id="ec452-113">Create a connection tooSFTP</span></span>
> [!INCLUDE [Steps toocreate a connection tooSFTP](../../includes/connectors-create-api-sftp.md)]
> 
> 

## <a name="use-an-sftp-trigger"></a><span data-ttu-id="ec452-114">Använd en SFTP-utlösare</span><span class="sxs-lookup"><span data-stu-id="ec452-114">Use an SFTP trigger</span></span>
<span data-ttu-id="ec452-115">En utlösare är en händelse som definierats i en logikapp används toostart hello i arbetsflöden.</span><span class="sxs-lookup"><span data-stu-id="ec452-115">A trigger is an event that can be used toostart hello workflow defined in a logic app.</span></span> <span data-ttu-id="ec452-116">[Mer information om utlösare](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="ec452-116">[Learn more about triggers](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>  

<span data-ttu-id="ec452-117">I det här exemplet hello **SFTP - när en fil har lagts till eller ändrats** utlösare är används tooinitiate en logik app arbetsflödet när en fil har lagts till eller ändrats på en SFTP-server.</span><span class="sxs-lookup"><span data-stu-id="ec452-117">In this example, hello **SFTP - When a file is added or modified** trigger is used tooinitiate a logic app workflow when a file is added to, or modified on, an SFTP server.</span></span> <span data-ttu-id="ec452-118">Du också lägga till ett villkor som kontrollerar hello innehållet i hello nya eller ändrade filen och gör en beslut tooextract hello-fil om innehållet visar ska extraheras innan du använder hello innehållet.</span><span class="sxs-lookup"><span data-stu-id="ec452-118">You also add a condition that checks hello contents of hello new or modified file, and makes a decision tooextract hello file if its contents indicate that it should be extracted before using hello contents.</span></span> <span data-ttu-id="ec452-119">Slutligen lägger du till en åtgärd tooextract hello innehållet i en fil och placerar hello extraherade innehållet i en mapp på hello SFTP-servern.</span><span class="sxs-lookup"><span data-stu-id="ec452-119">Finally, add an action tooextract hello contents of a file, and place hello extracted contents in a folder on hello SFTP server.</span></span> 

<span data-ttu-id="ec452-120">Du kan använda den här utlösaren toomonitor en SFTP-mapp för nya filer som representerar order i ett enterprise-exempel.</span><span class="sxs-lookup"><span data-stu-id="ec452-120">In an enterprise example, you could use this trigger toomonitor an SFTP folder for new files that represent customer orders.</span></span>  <span data-ttu-id="ec452-121">Du kan sedan använda åtgärden SFTP-koppling som **hämta filinnehåll**, tooget hello innehållet i hello ordning för vidare bearbetning och lagring i en order-databas.</span><span class="sxs-lookup"><span data-stu-id="ec452-121">You could then use an SFTP connector action, such as **Get file content**, tooget hello contents of hello order for further processing and storage in an orders database.</span></span>

> [!INCLUDE [Steps toocreate an SFTP trigger](../../includes/connectors-create-api-sftp-trigger.md)]
> 
> 

## <a name="add-a-condition"></a><span data-ttu-id="ec452-122">Lägg till ett villkor</span><span class="sxs-lookup"><span data-stu-id="ec452-122">Add a condition</span></span>
> [!INCLUDE [Steps tooadd a condition](../../includes/connectors-create-api-sftp-condition.md)]
> 
> 

## <a name="use-an-sftp-action"></a><span data-ttu-id="ec452-123">Använd en SFTP-åtgärd</span><span class="sxs-lookup"><span data-stu-id="ec452-123">Use an SFTP action</span></span>
<span data-ttu-id="ec452-124">En åtgärd är en åtgärd som utförs av hello arbetsflöde som definierats i en logikapp.</span><span class="sxs-lookup"><span data-stu-id="ec452-124">An action is an operation carried out by hello workflow defined in a logic app.</span></span> <span data-ttu-id="ec452-125">[Mer information om åtgärder](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="ec452-125">[Learn more about actions](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>  

> [!INCLUDE [Steps toocreate an SFTP action](../../includes/connectors-create-api-sftp-action.md)]
> 
> 

## <a name="connector-specific-details"></a><span data-ttu-id="ec452-126">Connector-specifik information</span><span class="sxs-lookup"><span data-stu-id="ec452-126">Connector-specific details</span></span>

<span data-ttu-id="ec452-127">Visa alla utlösare och åtgärder som definierats i hello swagger och även se några gränser i hello [connector information](/connectors/sftpconnector/).</span><span class="sxs-lookup"><span data-stu-id="ec452-127">View any triggers and actions defined in hello swagger, and also see any limits in hello [connector details](/connectors/sftpconnector/).</span></span>

## <a name="more-connectors"></a><span data-ttu-id="ec452-128">Flera kopplingar</span><span class="sxs-lookup"><span data-stu-id="ec452-128">More connectors</span></span>
<span data-ttu-id="ec452-129">Gå tillbaka toohello [API: er listan](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="ec452-129">Go back toohello [APIs list](apis-list.md).</span></span>
