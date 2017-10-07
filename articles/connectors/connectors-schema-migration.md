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
# <a name="how-toomigrate-logic-apps-tooschema-version-2015-08-01-preview"></a>Hur toomigrate logic apps tooschema version 2015-08-01-preview
toomove din befintliga logic apps toohello nya schemat, hello följande:  

1. Öppna logikappen i hello Azure-portalen  
2. Klicka på Uppdatera schema:
   
   ![API-ikon][step1]   
   hello uppdatera Schema visas och innehåller en länk tooa dokument som innehåller detaljerad information om hello förbättringar i hello nya schemat: ![API-ikon][step2]

> [!NOTE]
> När du väljer **uppdatera Schema**, vi automatiskt köra hello migreringssteg och tillhandahålla hello felkoden utdata för dig. Du kan använda den här tooupdate din definition, men måste du kontrollera att du följa bra kodningsmetoder, till exempel de som beskrivs i hello **metodtips** nedan.
> 
> 

## <a name="best-practices-when-migrating-your-logic-apps-toohello-latest-schema-version"></a>Bästa metoder när du migrerar dina Logic apps toohello senaste schemaversionen:
* Kopiera hello migrerade skriptet tooa nya logiken App – Skriv inte över hello gamla förrän du har slutfört din testning och bekräftat hello migrerade appen fungerar som förväntat.
* Testa logikappen **innan** du använder den
* När migreringen har slutförts kan du börja uppdatera dina Logic apps toouse hello [hanterade API: er](apis-list.md) där det är möjligt. Du kan till exempel börja använda Dropbox v2 på samma sätt som DropBox v1.

## <a name="whats-next"></a>Nästa steg
* [Lär dig hur toomanually migrera dina logikappar](../logic-apps/logic-apps-schema-2015-08-01.md)

<!--Icon references-->
[step1]: ./media/connectors-schema-migration/migrateschema1.png
[step2]: ./media/connectors-schema-migration/migrateschema2.png






