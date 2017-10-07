---
title: aaaProblems inloggning tooan utvecklade anpassade program | Microsoft Docs
description: Vanliga rrors som kan orsaka du toonot vara kan toosign till ett program som du har utvecklat med Azure AD
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: cc302e68ae6c129b74387c6fc5ba4fb45ccb8fb3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="problems-signing-in-tooan-custom-developed-application"></a>Problem med att logga i tooan utvecklade anpassade program

Det finns flera fel som kan orsaka du toonot vara kan toosign i en app. hello största orsaken personer får det här problemet är felkonfigurerad appar.

## <a name="errors-related-too-misconfigured-apps"></a>Fel relaterade för felkonfigurerad appar

* Kontrollera att både hello konfigurationer i hello portal matchar vad du har i din app. Mer specifikt jämför klient/program-ID, svars-URL: er, klienten hemligheter/nycklar och App-ID-URI.

* Jämför hello-resurs som du ska få åtkomst till tooin kod med hello konfigurerats behörigheter i hello **krävs resurser** fliken toomake att du bara begära resurser som du har konfigurerat.

* Se [Azure AD StackOverflow](http://stackoverflow.com/questions/tagged/azure-active-directory) för alla liknande fel eller problem.

## <a name="next-steps"></a>Nästa steg

[Utvecklarhandbok för Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide)<br>

[Medgivande och integrera appar tooAzure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications>)<br>

[Medgivande och Permissioning för Azure AD v2.0 konvergerat appar](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)<br>

[Azure AD-StackOverflow](http://stackoverflow.com/questions/tagged/azure-active-directory>)
