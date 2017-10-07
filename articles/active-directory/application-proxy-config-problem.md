---
title: aaaProblem skapar ett program med Application Proxy | Microsoft Docs
description: "Hur tootroubleshoot utfärdar skapar programproxyn program i hello Azure AD Admin portal"
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
ms.openlocfilehash: 24fab83c38a49a9e5754854acf2f9711e374e559
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="problem-creating-an-application-proxy-application"></a>Skapa ett program med Application Proxy 

Nedan visas några vanliga problem med hello personer står inför när du skapar ett nytt program för application proxy.

## <a name="recommended-documents"></a>Rekommenderade dokument 

toolearn mer om hur du skapar ett program för Application Proxy via hello Admin Portal finns [publicera program med Azure AD Application Proxy](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-azure-portal).

Om du följande hello i detta dokument och får ett fel när programmet hello finns hello felinformationen och förslag på hur toofix hello program. De vanligaste felmeddelanden som innehåller en föreslagen åtgärd. 

## <a name="specific-things-toocheck"></a>Vissa saker toocheck

tooavoid vanliga fel, kontrollera:

-   Du är administratör med behörigheten toocreate ett program med Application Proxy

-   hello Intern URL är unikt

-   hello externa URL: en är unikt

-   Hej URL: er starta med http eller https och sluta med en ”/”

-   hello Webbadress måste vara ett domännamn och inte en IP-adress

hello felmeddelande ska visas i hello övre högra hörnet när du skapar hello program. Du kan också välja hello ikonen toosee hello felmeddelanden.

   ![Meddelande-fråga](./media/application-proxy-config-problem/error-message.png)

## <a name="next-steps"></a>Nästa steg
[Aktivera Application Proxy i hello Azure-portalen](active-directory-application-proxy-enable.md)
