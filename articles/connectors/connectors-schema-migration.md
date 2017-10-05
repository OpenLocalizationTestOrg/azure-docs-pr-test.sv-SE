---
title: "Så här migrerar du logikappar till förhandsschemaversionen från 2015-08-01 | Microsoft Docs"
description: "Du kan enkelt migrera dina logikappar till den senaste schemaversionen. Följ bara de här stegen."
services: logic-apps
documentationcenter: 
author: MSFTMAN
manager: erikre
editor: 
tags: connectors
ms.assetid: 3e177e49-fd69-43e9-9b9b-218abb250c31
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/23/2016
ms.author: deonhe
ms.openlocfilehash: a5a73a9f124e5339b61dbc49021444a208a471f0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-migrate-logic-apps-to-schema-version-2015-08-01-preview"></a><span data-ttu-id="fa0a1-104">Så här migrerar du logikappar till förhandsschemaversionen från 2015-08-01</span><span class="sxs-lookup"><span data-stu-id="fa0a1-104">How to migrate logic apps to schema version 2015-08-01-preview</span></span>
<span data-ttu-id="fa0a1-105">Flytta dina befintliga logikappar till det nya schemat genom att göra följande:</span><span class="sxs-lookup"><span data-stu-id="fa0a1-105">To move your existing logic apps to the new schema, do the following:</span></span>  

1. <span data-ttu-id="fa0a1-106">Öppna logikappen i Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="fa0a1-106">Open your logic app in the Azure portal</span></span>  
2. <span data-ttu-id="fa0a1-107">Klicka på Uppdatera schema:</span><span class="sxs-lookup"><span data-stu-id="fa0a1-107">Click Update Schema:</span></span>
   
   <span data-ttu-id="fa0a1-108">![API-ikon][step1] </span><span class="sxs-lookup"><span data-stu-id="fa0a1-108">![API Icon][step1] </span></span>  
   <span data-ttu-id="fa0a1-109">Sidan Uppdatera schema visas och där finns en länk till ett dokument som innehåller detaljerad information om förbättringarna i det nya schemat: ![API-ikon][step2]</span><span class="sxs-lookup"><span data-stu-id="fa0a1-109">The Update Schema page displays and provides a link to a document that provide details on the improvements in the new schema: ![API Icon][step2]</span></span>

> [!NOTE]
> <span data-ttu-id="fa0a1-110">När du väljer **Uppdatera schema**kör vi automatiskt migreringsstegen och visar dig kodens utdata.</span><span class="sxs-lookup"><span data-stu-id="fa0a1-110">When you select **Update Schema**, we automatically run the migration steps and provide the code output for you.</span></span> <span data-ttu-id="fa0a1-111">Du kan använda dessa för att uppdatera din definition, men var noga med att följa bra kodningsmetoder, t.ex. de som beskrivs i avsnittet **Bästa metoder** nedan.</span><span class="sxs-lookup"><span data-stu-id="fa0a1-111">You can use this to update your definition, however, ensure you follow good coding practices such as those outlined in the **Best practices** section below.</span></span>
> 
> 

## <a name="best-practices-when-migrating-your-logic-apps-to-the-latest-schema-version"></a><span data-ttu-id="fa0a1-112">Bästa metoder när du migrerar dina logikappar till den senaste schemaversionen:</span><span class="sxs-lookup"><span data-stu-id="fa0a1-112">Best practices when migrating your Logic apps to the latest schema version:</span></span>
* <span data-ttu-id="fa0a1-113">Kopiera det migrerade skriptet till en ny logikapp – skriv inte över den gamla förrän du har slutfört testerna och bekräftat att den migrerade appen fungerar som förväntat.</span><span class="sxs-lookup"><span data-stu-id="fa0a1-113">Copy the migrated script to a new Logic App - don't overwrite the old one until you've completed your testing and confirmed the migrated app works as expected.</span></span>
* <span data-ttu-id="fa0a1-114">Testa logikappen **innan** du använder den</span><span class="sxs-lookup"><span data-stu-id="fa0a1-114">Test your Logic app **before** putting in production</span></span>
* <span data-ttu-id="fa0a1-115">När migreringen har slutförts kan du börja uppdatera logikapparna och använda de [hanterade API:erna](apis-list.md) där det är möjligt.</span><span class="sxs-lookup"><span data-stu-id="fa0a1-115">After migration completes, start updating your Logic apps to use the [managed APIs](apis-list.md) where possible.</span></span> <span data-ttu-id="fa0a1-116">Du kan till exempel börja använda Dropbox v2 på samma sätt som DropBox v1.</span><span class="sxs-lookup"><span data-stu-id="fa0a1-116">For example, you can start using Dropbox v2, whereever you are using DropBox v1.</span></span>

## <a name="whats-next"></a><span data-ttu-id="fa0a1-117">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fa0a1-117">What's next</span></span>
* [<span data-ttu-id="fa0a1-118">Lär dig att migrera dina logikappar manuellt</span><span class="sxs-lookup"><span data-stu-id="fa0a1-118">Learn how to manually migrate your Logic apps</span></span>](../logic-apps/logic-apps-schema-2015-08-01.md)

<!--Icon references-->
[step1]: ./media/connectors-schema-migration/migrateschema1.png
[step2]: ./media/connectors-schema-migration/migrateschema2.png






