---
title: "aaaGet igång med Azure AD i Visual Studio-WebApi projekt | Microsoft Docs"
description: "Hur tooget igång med Azure Active Directory i WebApi projekt efter anslutning tooor skapa en Azure AD med hjälp av Visual Studio anslutna tjänster"
services: active-directory
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: bf1eb32d-25cd-4abf-8679-2ead299fedaa
ms.service: active-directory
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 03/19/2017
ms.author: kraigb
ms.custom: aaddev
ms.openlocfilehash: 21413a71a2fd61f31268bf6d5e4d86b8be5bd16a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-active-directory-and-visual-studio-connected-services-webapi-projects"></a>Komma igång med Azure Active Directory och Visual Studio anslutna tjänster (WebApi-projekt)
> [!div class="op_single_selector"]
> * [Komma igång](vs-active-directory-webapi-getting-started.md)
> * [Vad hände](vs-active-directory-webapi-what-happened.md)
> 
> 

## <a name="requiring-authentication-tooaccess-controllers"></a>Kräver autentisering tooaccess domänkontrollanter
Alla domänkontrollanter i ditt projekt har adorned med hello **auktorisera** attribut. Det här attributet kräver hello användaren toobe autentiserad åtkomst till hello API: er definieras av dessa domänkontrollanter. tooallow hello controller toobe ansluta anonymt, ta bort skrivskyddsattributet från hello domänkontrollant. Om du vill tooset hello behörigheter på en mer detaljerad nivå gäller hello attributet tooeach metoden som kräver tillstånd i stället för att tillämpa den toohello kontrollantklassen.

## <a name="next-steps"></a>Nästa steg
[Lär dig mer om Azure Active Directory](https://azure.microsoft.com/services/active-directory/)

