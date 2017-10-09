---
title: "aaaAzure AD v2 Windows Desktop komma igång - Config | Microsoft Docs"
description: "Hur ett program för Windows Desktop .NET (XAML) hämta en åtkomst-token och anropa ett API som skyddas av Azure Active Directory v2 slutpunkt. | Microsoft Azure | Microsoft Azure"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: 39c257e3e0cb09491f6fe005877601bd46824d12
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
## <a name="create-an-application-express"></a>Skapa ett program (snabb)
Nu måste tooregister ditt program i hello *Microsoft Programregistreringsportalen*:
1. Registrera ditt program via hello [Microsoft Programregistreringsportalen](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=windowsDesktop&step=configure)
2.  Ange ett namn för ditt program och din e-post
3.  Kontrollera att hello alternativ för interaktiv installation är markerat
4.  Följ hello instruktioner tooobtain hello program-ID och klistra in den i din kod

### <a name="add-your-application-registration-information-tooyour-solution-advanced"></a>Lägg till din registrering information tooyour lösning (Avancerat)
Nu måste tooregister ditt program i hello *Microsoft Programregistreringsportalen*:
1. Gå toohello [Microsoft Programregistreringsportalen](https://apps.dev.microsoft.com/portal/register-app) tooregister ett program
2. Ange ett namn för ditt program och din e-post 
3. Kontrollera att hello alternativet för interaktiv installation är markerat
4. Klicka på `Add Platform`och välj `Native Application` och tryck på Spara
5. Kopiera hello GUID i program-ID, gå tillbaka tooVisual Studio, öppna `App.xaml.cs` och Ersätt `your_client_id_here` med hello program-ID som du just har registrerat:

```csharp
private static string ClientId = "your_application_id_here";
```
