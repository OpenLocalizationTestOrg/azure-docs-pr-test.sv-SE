---
title: "aaaHow toofind specifika API: et behövs för ett anpassat utvecklade program | Microsoft Docs"
description: "Hur tooconfigure hello behörigheter som du behöver tooaccess ett visst API i dina anpassade utvecklade Azure AD-program"
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
ms.openlocfilehash: 7331129204d8b34b4ef9671749bd702f893768ff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toofind-a-specific-api-needed-for-a-custom-developed-application"></a>Hur toofind en specifika API: et behövs för ett anpassat utvecklade program

Åtkomst tooAPIs kräver konfiguration av åtkomstscope och roller. Om du vill tooexpose resurs programmet API: er tooclient webbprogrammen, behöver du tooconfigure åtkomstscope och roller för hello API. Om du vill att en klient programmet tooaccess ett webb-API, måste tooconfigure behörigheter tooaccess hello API i hello app registreringen.

## <a name="configuring-a-resource-application-tooexpose-web-apis"></a>Konfigurera en resurs programmet tooexpose-webb-API: er

När du exponera ditt webb-API, hello API visas i hello **väljer en API** listan när du lägger till behörigheter tooan appregistrering. tooadd åtkomstscope följer hello steg som beskrivs i [att lägga till access scope tooyour resursprogram](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#adding-access-scopes-to-your-resource-application).

## <a name="configuring-a-client-application-tooaccess-web-apis"></a>Konfigurera en klient programmet tooaccess-webb-API: er

När du lägger till behörigheter tooyour appregistrering, kan du **lägga till API-åtkomst** tooexposed web API: er. tooaccess webb-API: er, Följ stegen i hello [lägga till autentiseringsuppgifter eller behörigheter tooaccess webb-API: er](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#to-add-credentials-or-permissions-to-access-web-apis).

## <a name="next-steps"></a>Nästa steg

-   [Integrera program med Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications)

-   [Förstå hello Azure Active Directory-programmanifestet](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-manifest)


