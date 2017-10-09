---
title: "aaaAzure AD v2 Windows Desktop komma igång - inställningar | Microsoft Docs"
description: "Hur Windows Desktop .NET (XAML) program anropar en API som kräver åtkomst-token i Azure Active Directory v2 slutpunkten"
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
ms.openlocfilehash: 097ea99bef01e15edaa5ff914ff4e18392b77c5a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
## <a name="set-up-your-project"></a><span data-ttu-id="ce096-103">Konfigurera ditt projekt</span><span class="sxs-lookup"><span data-stu-id="ce096-103">Set up your project</span></span>

<span data-ttu-id="ce096-104">Det här avsnittet innehåller stegvisa instruktioner för hur toocreate ett nytt projekt toodemonstrate hur toointegrate en Windows-skrivbordet .NET-program (XAML) med *logga In med Microsoft* så att den kan fråga Web API: er som kräver ett token.</span><span class="sxs-lookup"><span data-stu-id="ce096-104">This section provides step-by-step instructions for how toocreate a new project toodemonstrate how toointegrate a Windows Desktop .NET application (XAML) with *Sign-In with Microsoft* so it can query Web APIs that requires a token.</span></span>

<span data-ttu-id="ce096-105">hello-program som skapats av den här guiden visar en knappen toograph och visa resultaten på skärmen och en knapp för utloggning.</span><span class="sxs-lookup"><span data-stu-id="ce096-105">hello application created by this guide exposes a button toograph and show results on screen and a sign-out button.</span></span>

> <span data-ttu-id="ce096-106">Föredrar toodownload det här exemplet Visual Studio-projekt i stället?</span><span class="sxs-lookup"><span data-stu-id="ce096-106">Prefer toodownload this sample's Visual Studio project instead?</span></span> <span data-ttu-id="ce096-107">[Hämta ett projekt](https://github.com/Azure-Samples/active-directory-dotnet-desktop-msgraph-v2/archive/master.zip) och hoppa över toohello [konfigurationssteget](#create-an-application-express) tooconfigure hello kodexemplet innan du kör.</span><span class="sxs-lookup"><span data-stu-id="ce096-107">[Download a project](https://github.com/Azure-Samples/active-directory-dotnet-desktop-msgraph-v2/archive/master.zip) and skip toohello [Configuration step](#create-an-application-express) tooconfigure hello code sample before executing.</span></span>


### <a name="create-your-application"></a><span data-ttu-id="ce096-108">Skapa programmet</span><span class="sxs-lookup"><span data-stu-id="ce096-108">Create your application</span></span>
1. <span data-ttu-id="ce096-109">I Visual Studio:`File` > `New` > `Project`</span><span class="sxs-lookup"><span data-stu-id="ce096-109">In Visual Studio: `File` > `New` > `Project`</span></span><br/>
2. <span data-ttu-id="ce096-110">Under *mallar*väljer`Visual C#`</span><span class="sxs-lookup"><span data-stu-id="ce096-110">Under *Templates*, select `Visual C#`</span></span>
3. <span data-ttu-id="ce096-111">Välj `WPF App` (eller *WPF-program* beroende på hello version av din Visual Studio)</span><span class="sxs-lookup"><span data-stu-id="ce096-111">Select `WPF App` (or *WPF Application* depending on hello version of your Visual Studio)</span></span>

## <a name="add-hello-microsoft-authentication-library-msal-tooyour-project"></a><span data-ttu-id="ce096-112">Lägga till hello Microsoft Authentication Library (MSAL) tooyour projekt</span><span class="sxs-lookup"><span data-stu-id="ce096-112">Add hello Microsoft Authentication Library (MSAL) tooyour project</span></span>
1. <span data-ttu-id="ce096-113">I Visual Studio:`Tools` > `Nuget Package Manager` > `Package Manager Console`</span><span class="sxs-lookup"><span data-stu-id="ce096-113">In Visual Studio: `Tools` > `Nuget Package Manager` > `Package Manager Console`</span></span>
2. <span data-ttu-id="ce096-114">Kopiera och klistra in följande hello i hello fönstret Package Manager-konsolen:</span><span class="sxs-lookup"><span data-stu-id="ce096-114">Copy/paste hello following in hello Package Manager Console window:</span></span>

```powershell
Install-Package Microsoft.Identity.Client -Pre
```

> <span data-ttu-id="ce096-115">hello-paketet ovan installerar hello Microsoft Authentication Library (MSAL).</span><span class="sxs-lookup"><span data-stu-id="ce096-115">hello package above installs hello Microsoft Authentication Library (MSAL).</span></span> <span data-ttu-id="ce096-116">MSAL hanterar införskaffa, cachelagring och uppdatera användaren toskens används tooaccess API: er som skyddas av Azure Active Directory v2.</span><span class="sxs-lookup"><span data-stu-id="ce096-116">MSAL handles acquiring, caching and refreshing user toskens used tooaccess APIs protected by Azure Active Directory v2.</span></span>

## <a name="add-hello-code-tooinitialize-msal"></a><span data-ttu-id="ce096-117">Lägg till hello kod tooinitialize MSAL</span><span class="sxs-lookup"><span data-stu-id="ce096-117">Add hello code tooinitialize MSAL</span></span>
<span data-ttu-id="ce096-118">Det här steget hjälper dig att skapa en klass toohandle interaktion med MSAL bibliotek, t.ex hantering av token.</span><span class="sxs-lookup"><span data-stu-id="ce096-118">This step will help you create a class toohandle interaction with MSAL Library, such as handling of tokens.</span></span>

1. <span data-ttu-id="ce096-119">Öppna hello `App.xaml.cs` och Lägg till hello-referens för MSAL biblioteket toohello klass:</span><span class="sxs-lookup"><span data-stu-id="ce096-119">Open hello `App.xaml.cs` file and add hello reference for MSAL library toohello class:</span></span>

```csharp
using Microsoft.Identity.Client;
```
<!-- Workaround for Docs conversion bug -->
<ol start="2">
<li>
<span data-ttu-id="ce096-120">Uppdatera hello App klassen toohello följande:</span><span class="sxs-lookup"><span data-stu-id="ce096-120">Update hello App class toohello following:</span></span>
</li>
</ol>

```csharp
public partial class App : Application
{
    //Below is hello clientId of your app registration. 
    //You have tooreplace hello below with hello Application Id for your app registration
    private static string ClientId = "your_client_id_here";

    public static PublicClientApplication PublicClientApp = new PublicClientApplication(ClientId);

}
```

## <a name="create-your-applications-ui"></a><span data-ttu-id="ce096-121">Skapa ditt programs gränssnitt</span><span class="sxs-lookup"><span data-stu-id="ce096-121">Create your application’s UI</span></span>
<span data-ttu-id="ce096-122">hello nedan visar hur ett program kan fråga skyddade backend-servern som Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="ce096-122">hello section below shows how an application can query a protected backend server like Microsoft Graph.</span></span> <span data-ttu-id="ce096-123">En MainWindow.xaml-fil ska skapas automatiskt som en del av projektmallen för.</span><span class="sxs-lookup"><span data-stu-id="ce096-123">A MainWindow.xaml file should automatically be created as a part of your project template.</span></span> <span data-ttu-id="ce096-124">Öppna filen för den här filen och följ sedan hello instruktionerna nedan:</span><span class="sxs-lookup"><span data-stu-id="ce096-124">Open this file this file and then follow hello instructions below:</span></span>

<span data-ttu-id="ce096-125">Ersätt ditt programs `<Grid>` med vara hello följande:</span><span class="sxs-lookup"><span data-stu-id="ce096-125">Replace your application’s `<Grid>` with be hello following:</span></span>

```xml
<Grid>
    <StackPanel Background="Azure">
        <StackPanel Orientation="Horizontal" HorizontalAlignment="Right">
            <Button x:Name="CallGraphButton" Content="Call Microsoft Graph API" HorizontalAlignment="Right" Padding="5" Click="CallGraphButton_Click" Margin="5" FontFamily="Segoe Ui"/>
            <Button x:Name="SignOutButton" Content="Sign-Out" HorizontalAlignment="Right" Padding="5" Click="SignOutButton_Click" Margin="5" Visibility="Collapsed" FontFamily="Segoe Ui"/>
        </StackPanel>
        <Label Content="API Call Results" Margin="0,0,0,-5" FontFamily="Segoe Ui" />
        <TextBox x:Name="ResultText" TextWrapping="Wrap" MinHeight="120" Margin="5" FontFamily="Segoe Ui"/>
        <Label Content="Token Info" Margin="0,0,0,-5" FontFamily="Segoe Ui" />
        <TextBox x:Name="TokenInfoText" TextWrapping="Wrap" MinHeight="70" Margin="5" FontFamily="Segoe Ui"/>
    </StackPanel>
</Grid>
```
