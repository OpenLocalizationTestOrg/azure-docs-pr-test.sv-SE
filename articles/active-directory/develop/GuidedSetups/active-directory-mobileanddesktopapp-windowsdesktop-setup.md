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
## <a name="set-up-your-project"></a>Konfigurera ditt projekt

Det här avsnittet innehåller stegvisa instruktioner för hur toocreate ett nytt projekt toodemonstrate hur toointegrate en Windows-skrivbordet .NET-program (XAML) med *logga In med Microsoft* så att den kan fråga Web API: er som kräver ett token.

hello-program som skapats av den här guiden visar en knappen toograph och visa resultaten på skärmen och en knapp för utloggning.

> Föredrar toodownload det här exemplet Visual Studio-projekt i stället? [Hämta ett projekt](https://github.com/Azure-Samples/active-directory-dotnet-desktop-msgraph-v2/archive/master.zip) och hoppa över toohello [konfigurationssteget](#create-an-application-express) tooconfigure hello kodexemplet innan du kör.


### <a name="create-your-application"></a>Skapa programmet
1. I Visual Studio:`File` > `New` > `Project`<br/>
2. Under *mallar*väljer`Visual C#`
3. Välj `WPF App` (eller *WPF-program* beroende på hello version av din Visual Studio)

## <a name="add-hello-microsoft-authentication-library-msal-tooyour-project"></a>Lägga till hello Microsoft Authentication Library (MSAL) tooyour projekt
1. I Visual Studio:`Tools` > `Nuget Package Manager` > `Package Manager Console`
2. Kopiera och klistra in följande hello i hello fönstret Package Manager-konsolen:

```powershell
Install-Package Microsoft.Identity.Client -Pre
```

> hello-paketet ovan installerar hello Microsoft Authentication Library (MSAL). MSAL hanterar införskaffa, cachelagring och uppdatera användaren toskens används tooaccess API: er som skyddas av Azure Active Directory v2.

## <a name="add-hello-code-tooinitialize-msal"></a>Lägg till hello kod tooinitialize MSAL
Det här steget hjälper dig att skapa en klass toohandle interaktion med MSAL bibliotek, t.ex hantering av token.

1. Öppna hello `App.xaml.cs` och Lägg till hello-referens för MSAL biblioteket toohello klass:

```csharp
using Microsoft.Identity.Client;
```
<!-- Workaround for Docs conversion bug -->
<ol start="2">
<li>
Uppdatera hello App klassen toohello följande:
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

## <a name="create-your-applications-ui"></a>Skapa ditt programs gränssnitt
hello nedan visar hur ett program kan fråga skyddade backend-servern som Microsoft Graph. En MainWindow.xaml-fil ska skapas automatiskt som en del av projektmallen för. Öppna filen för den här filen och följ sedan hello instruktionerna nedan:

Ersätt ditt programs `<Grid>` med vara hello följande:

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
