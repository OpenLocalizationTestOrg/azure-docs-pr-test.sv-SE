---
title: "Azure AD v2 Windows Desktop komma igång - Config | Microsoft Docs"
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
ms.openlocfilehash: 1dfaa7ade664e43dcb9aa788b0197ca17e6ec4cc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
## <a name="create-an-application-express"></a><span data-ttu-id="89248-104">Skapa ett program (snabb)</span><span class="sxs-lookup"><span data-stu-id="89248-104">Create an application (Express)</span></span>
<span data-ttu-id="89248-105">Nu måste du registrera ditt program i den *Microsoft Programregistreringsportalen*:</span><span class="sxs-lookup"><span data-stu-id="89248-105">Now you need to register your application in the *Microsoft Application Registration Portal*:</span></span>
1. <span data-ttu-id="89248-106">Registrera ditt program via den [Microsoft Programregistreringsportalen](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=windowsDesktop&step=configure)</span><span class="sxs-lookup"><span data-stu-id="89248-106">Register your application via the [Microsoft Application Registration Portal](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=windowsDesktop&step=configure)</span></span>
2.  <span data-ttu-id="89248-107">Ange ett namn för ditt program och din e-post</span><span class="sxs-lookup"><span data-stu-id="89248-107">Enter a name for your application and your email</span></span>
3.  <span data-ttu-id="89248-108">Kontrollera att alternativet för interaktiv installation är markerat</span><span class="sxs-lookup"><span data-stu-id="89248-108">Make sure the option for Guided Setup is checked</span></span>
4.  <span data-ttu-id="89248-109">Följ instruktionerna för att hämta program-ID och klistra in den i din kod</span><span class="sxs-lookup"><span data-stu-id="89248-109">Follow the instructions to obtain the application ID and paste it into your code</span></span>

### <a name="add-your-application-registration-information-to-your-solution-advanced"></a><span data-ttu-id="89248-110">Lägga till registreringsinformationen program i lösningen (Avancerat)</span><span class="sxs-lookup"><span data-stu-id="89248-110">Add your application registration information to your solution (Advanced)</span></span>
<span data-ttu-id="89248-111">Nu måste du registrera ditt program i den *Microsoft Programregistreringsportalen*:</span><span class="sxs-lookup"><span data-stu-id="89248-111">Now you need to register your application in the *Microsoft Application Registration Portal*:</span></span>
1. <span data-ttu-id="89248-112">Gå till den [Microsoft Programregistreringsportalen](https://apps.dev.microsoft.com/portal/register-app) registrera ett program</span><span class="sxs-lookup"><span data-stu-id="89248-112">Go to the [Microsoft Application Registration Portal](https://apps.dev.microsoft.com/portal/register-app) to register an application</span></span>
2. <span data-ttu-id="89248-113">Ange ett namn för ditt program och din e-post</span><span class="sxs-lookup"><span data-stu-id="89248-113">Enter a name for your application and your email</span></span> 
3. <span data-ttu-id="89248-114">Kontrollera att alternativet för interaktiv installation är markerat</span><span class="sxs-lookup"><span data-stu-id="89248-114">Make sure the option for Guided Setup is unchecked</span></span>
4. <span data-ttu-id="89248-115">Klicka på `Add Platform`och välj `Native Application` och tryck på Spara</span><span class="sxs-lookup"><span data-stu-id="89248-115">Click `Add Platform`, then select `Native Application` and hit Save</span></span>
5. <span data-ttu-id="89248-116">Kopiera GUID i program-ID, gå tillbaka till Visual Studio, öppna `App.xaml.cs` och Ersätt `your_client_id_here` med program-ID som du just har registrerat:</span><span class="sxs-lookup"><span data-stu-id="89248-116">Copy the GUID in Application ID, go back to Visual Studio, open `App.xaml.cs` and replace `your_client_id_here` with the Application ID you just registered:</span></span>

```csharp
private static string ClientId = "your_application_id_here";
```
