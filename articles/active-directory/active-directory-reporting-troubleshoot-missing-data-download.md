---
title: "Felsökning: Hämtade data saknas i hello Azure Active Directory aktivitetsloggar | Microsoft Docs"
description: "Ger dig en lösning toomissing data i hämtade aktivitetsloggar för Azure Active Directory."
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
ms.openlocfilehash: 027b70e6efc570f81d3c836f50ee52aaa89be71a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="i-cant-find-any-data-in-hello-azure-active-directory-activity-logs-i-have-downloaded"></a>Det går inte att hitta några data i hello Azure Active Directory aktivitetsloggar jag har hämtat


## <a name="symptoms"></a>Symtom

Jag har hämtat hello aktivitetsloggar (granskning eller inloggningar) och visas inte alla hello-poster för hello gången som du har valt. Varför? 

 ![Rapportering](./media/active-directory-reporting-troubleshoot-missing-data-download/01.png)
 

## <a name="cause"></a>Orsak

När du hämtar aktivitetsloggar i hello Azure-portalen, vi begränsa hello skala too120K poster, sorterade efter senaste. 

## <a name="resolution"></a>Lösning

Du kan utnyttja [Azure AD Reporting API: erna](active-directory-reporting-api-getting-started.md) toofetch upp tooa miljoner poster vid en given tidpunkt. Vår rekommendation är toorun ett skript enligt ett schema som anropar hello reporting API: er toofetch registrerar på en inkrementell sätt under en viss tidsperiod (t.ex. varje dag eller varje vecka).

## <a name="next-steps"></a>Nästa steg
Se hello [Azure Active Directory reporting vanliga frågor och svar](active-directory-reporting-faq.md).

