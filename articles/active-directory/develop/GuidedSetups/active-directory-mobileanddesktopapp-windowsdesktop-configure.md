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
## <a name="create-an-application-express"></a><span data-ttu-id="4bcfe-104">Skapa ett program (snabb)</span><span class="sxs-lookup"><span data-stu-id="4bcfe-104">Create an application (Express)</span></span>
<span data-ttu-id="4bcfe-105">Nu måste tooregister ditt program i hello *Microsoft Programregistreringsportalen*:</span><span class="sxs-lookup"><span data-stu-id="4bcfe-105">Now you need tooregister your application in hello *Microsoft Application Registration Portal*:</span></span>
1. <span data-ttu-id="4bcfe-106">Registrera ditt program via hello [Microsoft Programregistreringsportalen](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=windowsDesktop&step=configure)</span><span class="sxs-lookup"><span data-stu-id="4bcfe-106">Register your application via hello [Microsoft Application Registration Portal](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=windowsDesktop&step=configure)</span></span>
2.  <span data-ttu-id="4bcfe-107">Ange ett namn för ditt program och din e-post</span><span class="sxs-lookup"><span data-stu-id="4bcfe-107">Enter a name for your application and your email</span></span>
3.  <span data-ttu-id="4bcfe-108">Kontrollera att hello alternativ för interaktiv installation är markerat</span><span class="sxs-lookup"><span data-stu-id="4bcfe-108">Make sure hello option for Guided Setup is checked</span></span>
4.  <span data-ttu-id="4bcfe-109">Följ hello instruktioner tooobtain hello program-ID och klistra in den i din kod</span><span class="sxs-lookup"><span data-stu-id="4bcfe-109">Follow hello instructions tooobtain hello application ID and paste it into your code</span></span>

### <a name="add-your-application-registration-information-tooyour-solution-advanced"></a><span data-ttu-id="4bcfe-110">Lägg till din registrering information tooyour lösning (Avancerat)</span><span class="sxs-lookup"><span data-stu-id="4bcfe-110">Add your application registration information tooyour solution (Advanced)</span></span>
<span data-ttu-id="4bcfe-111">Nu måste tooregister ditt program i hello *Microsoft Programregistreringsportalen*:</span><span class="sxs-lookup"><span data-stu-id="4bcfe-111">Now you need tooregister your application in hello *Microsoft Application Registration Portal*:</span></span>
1. <span data-ttu-id="4bcfe-112">Gå toohello [Microsoft Programregistreringsportalen](https://apps.dev.microsoft.com/portal/register-app) tooregister ett program</span><span class="sxs-lookup"><span data-stu-id="4bcfe-112">Go toohello [Microsoft Application Registration Portal](https://apps.dev.microsoft.com/portal/register-app) tooregister an application</span></span>
2. <span data-ttu-id="4bcfe-113">Ange ett namn för ditt program och din e-post</span><span class="sxs-lookup"><span data-stu-id="4bcfe-113">Enter a name for your application and your email</span></span> 
3. <span data-ttu-id="4bcfe-114">Kontrollera att hello alternativet för interaktiv installation är markerat</span><span class="sxs-lookup"><span data-stu-id="4bcfe-114">Make sure hello option for Guided Setup is unchecked</span></span>
4. <span data-ttu-id="4bcfe-115">Klicka på `Add Platform`och välj `Native Application` och tryck på Spara</span><span class="sxs-lookup"><span data-stu-id="4bcfe-115">Click `Add Platform`, then select `Native Application` and hit Save</span></span>
5. <span data-ttu-id="4bcfe-116">Kopiera hello GUID i program-ID, gå tillbaka tooVisual Studio, öppna `App.xaml.cs` och Ersätt `your_client_id_here` med hello program-ID som du just har registrerat:</span><span class="sxs-lookup"><span data-stu-id="4bcfe-116">Copy hello GUID in Application ID, go back tooVisual Studio, open `App.xaml.cs` and replace `your_client_id_here` with hello Application ID you just registered:</span></span>

```csharp
private static string ClientId = "your_application_id_here";
```
