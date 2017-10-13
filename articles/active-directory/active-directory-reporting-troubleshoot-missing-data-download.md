---
title: "Felsökning: Saknade data i de nedladdade Azure Active Directory-aktivitetsloggarna | Microsoft Docs"
description: "Ger en lösning till saknade data i nedladdade Azure Active Directory-aktivitetsloggar."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: ffce7eb1-99da-4ea7-9c4d-2322b755c8ce
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/15/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 3d56f89035da4d1a0074256b165663f81fc2b01e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/11/2017
---
# <a name="i-cant-find-any-data-in-the-azure-active-directory-activity-logs-i-have-downloaded"></a>Jag kan inte hitta några data i Azure Active Directory-aktivitetsloggar som jag har hämtat


## <a name="symptoms"></a>Symtom

Jag har hämtat aktivitetsloggarna (granskning eller inloggningar) och kan inte se alla poster för den tid som jag har valt. Varför? 

 ![Rapportering](./media/active-directory-reporting-troubleshoot-missing-data-download/01.png)
 

## <a name="cause"></a>Orsak

När du hämtar aktivitetsloggar i Azure Portal begränsar vi omfattningen till 120 000 poster, sorterade efter de senaste. 

## <a name="resolution"></a>Lösning

Du kan använda [rapporterings-API:er för Azure AD](active-directory-reporting-api-getting-started.md) att hämta upp till en miljoner poster när som helst. Vår rekommenderade metod är att köra ett skript enligt ett schema som anropar rapporterings-API: er för att hämta poster på ett inkrementellt sätt under en viss tidsperiod (t.ex. varje dag eller varje vecka).

## <a name="next-steps"></a>Nästa steg
Se guiden med [vanliga frågor om Azure Active Directory-rapportering](active-directory-reporting-faq.md).

