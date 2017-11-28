---
title: aaaHow toomigrate logic apps tooschema version 2015-08-01-preview | Microsoft Docs
description: "Du kan enkelt migrera dina logic apps toohello senaste schemaversionen. Följ bara de här stegen."
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
ms.openlocfilehash: c7b42aaec547eddd28b0c649a3c0625047f9f805
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomigrate-logic-apps-tooschema-version-2015-08-01-preview"></a><span data-ttu-id="122f7-104">Hur toomigrate logic apps tooschema version 2015-08-01-preview</span><span class="sxs-lookup"><span data-stu-id="122f7-104">How toomigrate logic apps tooschema version 2015-08-01-preview</span></span>
<span data-ttu-id="122f7-105">toomove din befintliga logic apps toohello nya schemat, hello följande:</span><span class="sxs-lookup"><span data-stu-id="122f7-105">toomove your existing logic apps toohello new schema, do hello following:</span></span>  

1. <span data-ttu-id="122f7-106">Öppna logikappen i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="122f7-106">Open your logic app in hello Azure portal</span></span>  
2. <span data-ttu-id="122f7-107">Klicka på Uppdatera schema:</span><span class="sxs-lookup"><span data-stu-id="122f7-107">Click Update Schema:</span></span>
   
   <span data-ttu-id="122f7-108">![API-ikon][step1] </span><span class="sxs-lookup"><span data-stu-id="122f7-108">![API Icon][step1] </span></span>  
   <span data-ttu-id="122f7-109">hello uppdatera Schema visas och innehåller en länk tooa dokument som innehåller detaljerad information om hello förbättringar i hello nya schemat: ![API-ikon][step2]</span><span class="sxs-lookup"><span data-stu-id="122f7-109">hello Update Schema page displays and provides a link tooa document that provide details on hello improvements in hello new schema: ![API Icon][step2]</span></span>

> [!NOTE]
> <span data-ttu-id="122f7-110">När du väljer **uppdatera Schema**, vi automatiskt köra hello migreringssteg och tillhandahålla hello felkoden utdata för dig.</span><span class="sxs-lookup"><span data-stu-id="122f7-110">When you select **Update Schema**, we automatically run hello migration steps and provide hello code output for you.</span></span> <span data-ttu-id="122f7-111">Du kan använda den här tooupdate din definition, men måste du kontrollera att du följa bra kodningsmetoder, till exempel de som beskrivs i hello **metodtips** nedan.</span><span class="sxs-lookup"><span data-stu-id="122f7-111">You can use this tooupdate your definition, however, ensure you follow good coding practices such as those outlined in hello **Best practices** section below.</span></span>
> 
> 

## <a name="best-practices-when-migrating-your-logic-apps-toohello-latest-schema-version"></a><span data-ttu-id="122f7-112">Bästa metoder när du migrerar dina Logic apps toohello senaste schemaversionen:</span><span class="sxs-lookup"><span data-stu-id="122f7-112">Best practices when migrating your Logic apps toohello latest schema version:</span></span>
* <span data-ttu-id="122f7-113">Kopiera hello migrerade skriptet tooa nya logiken App – Skriv inte över hello gamla förrän du har slutfört din testning och bekräftat hello migrerade appen fungerar som förväntat.</span><span class="sxs-lookup"><span data-stu-id="122f7-113">Copy hello migrated script tooa new Logic App - don't overwrite hello old one until you've completed your testing and confirmed hello migrated app works as expected.</span></span>
* <span data-ttu-id="122f7-114">Testa logikappen **innan** du använder den</span><span class="sxs-lookup"><span data-stu-id="122f7-114">Test your Logic app **before** putting in production</span></span>
* <span data-ttu-id="122f7-115">När migreringen har slutförts kan du börja uppdatera dina Logic apps toouse hello [hanterade API: er](apis-list.md) där det är möjligt.</span><span class="sxs-lookup"><span data-stu-id="122f7-115">After migration completes, start updating your Logic apps toouse hello [managed APIs](apis-list.md) where possible.</span></span> <span data-ttu-id="122f7-116">Du kan till exempel börja använda Dropbox v2 på samma sätt som DropBox v1.</span><span class="sxs-lookup"><span data-stu-id="122f7-116">For example, you can start using Dropbox v2, whereever you are using DropBox v1.</span></span>

## <a name="whats-next"></a><span data-ttu-id="122f7-117">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="122f7-117">What's next</span></span>
* [<span data-ttu-id="122f7-118">Lär dig hur toomanually migrera dina logikappar</span><span class="sxs-lookup"><span data-stu-id="122f7-118">Learn how toomanually migrate your Logic apps</span></span>](../logic-apps/logic-apps-schema-2015-08-01.md)

<!--Icon references-->
[step1]: ./media/connectors-schema-migration/migrateschema1.png
[step2]: ./media/connectors-schema-migration/migrateschema2.png






