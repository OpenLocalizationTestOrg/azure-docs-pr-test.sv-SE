---
title: "aaaSet en anpassad hemsida för publicerade appar med hjälp av Azure AD Application Proxy | Microsoft Docs"
description: Beskriver hello grunderna om Azure AD Application Proxy-kopplingar
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 5bb695e904d285c3b440520f107c7bf63ba5cac9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="set-a-custom-home-page-for-published-apps-by-using-azure-ad-application-proxy"></a>Ange en anpassad hemsida för publicerade appar med hjälp av Azure AD Application Proxy

Den här artikeln beskrivs hur tooconfigure appar toodirect användare tooa anpassad startsida. När du publicerar ett program med Application Proxy kan du ställa in en intern URL men ibland som är inte hello sidan som användarna ska se först. Ange en anpassad hemsida så att användarna gå toohello höger sida när de kommer åt hello appar från hello Azure Active Directory-åtkomstpanelen eller hello Office 365 app starta.

När användarna startar hello app, är de riktade som standard toohello rot domän-URL för hello publicerade app. hello landningssida anges vanligtvis som hello URL-Adressen. Använd hello Azure AD PowerShell-modulen toodefine anpassad startsida webbadresser när du vill app användare tooland på en viss sida i hello app. 

Exempel:
- I företagsnätverket, användarna gå för*https://ExpenseApp/login/login.aspx* toosign i och åtkomst till din app.
- Eftersom du har andra tillgångar som bilder att Application Proxy måste tooaccess toppnivå hello för hello mappstrukturen du publicerar hello app med *https://ExpenseApp* som hello Intern URL.
- Hej standard externa URL: en är *https://ExpenseApp-contoso.msappproxy.net*, som tar inte användare toohello inloggningen på sidan.  
- Ange *https://ExpenseApp-contoso.msappproxy.net/login/login.aspx* som hello startsidan URL toogive användarna smidigt. 

>[!NOTE]
>När du ge användare åtkomst toopublished appar hello appar visas i hello [Azure AD-åtkomstpanelen](active-directory-saas-access-panel-introduction.md) och hello [Office 365 app starta](https://blogs.office.com/2016/09/27/introducing-the-new-office-365-app-launcher).

## <a name="before-you-start"></a>Innan du börjar

Innan du kan ange hello URL-Adressen måste ha i åtanke hello följande krav:

* Se till att hello sökvägen som du anger är en underdomän sökväg av hello rot domän-URL.

  Om hello rotdomänen Webbadress är, till exempel https://apps.contoso.com/app1/, hello URL-Adressen som du konfigurerar måste börja med https://apps.contoso.com/app1/.

* Om du ändrar toohello publicerade app, hello ändring kan återställa hello värdet för hello URL-Adressen. När du uppdaterar hello app i hello framtida bör du kontrollera och, om det behövs, uppdatera hello URL-Adressen.

## <a name="change-hello-home-page-in-hello-azure-portal"></a>Ändra hello startsida i hello Azure-portalen

1. Logga in toohello [Azure-portalen](https://portal.azure.com) som administratör.
2. Navigera för**Azure Active Directory** > **App registreringar** och välja programmet hello-listan. 
3. Välj **egenskaper** från hello inställningar.
4. Uppdatera hello **URL-Adressen** med den nya sökvägen. 

   ![Ange ny URL-Adressen](./media/application-proxy-office365-app-launcher/homepage.png)

5. Välj **spara**

## <a name="change-hello-home-page-with-powershell"></a>Ändra hello startsida med PowerShell

### <a name="install-hello-azure-ad-powershell-module"></a>Installera hello Azure AD PowerShell-modulen

Innan du definierar en anpassad URL-Adressen med hjälp av PowerShell installera hello Azure AD PowerShell-modulen. Du kan hämta hello paketet från hello [PowerShell-galleriet](https://www.powershellgallery.com/packages/AzureAD/2.0.0.131), som använder hello Graph API-slutpunkt. 

tooinstall hello paketet, gör du följande:

1. Öppna ett PowerShell-fönster som standard och kör sedan följande kommando hello:

    ```
     Install-Module -Name AzureAD
    ```
    Om du använder kommandot hello som icke-administratörer kan använda hello `-scope currentuser` alternativet.
2. Under installationen av hello väljer **Y** tooinstall två paket från Nuget.org. Båda paketen krävs. 

### <a name="find-hello-objectid-of-hello-app"></a>Hitta hello ObjectID hello-appen

Hämta hello ObjectID hello-appen och sök sedan efter hello app av dess hemsida.

1. Öppna PowerShell och importera hello Azure AD-modulen.

    ```
    Import-Module AzureAD
    ```

2. Logga in toohello Azure AD-modulen som hello Innehavaradministratör.

    ```
    Connect-AzureAD
    ```
3. Hitta hello app baserat på dess URL-Adressen. Du kan hitta hello URL i hello portal genom att gå för**Azure Active Directory** > **företagsprogram** > **alla program**. Det här exemplet används *sharepoint iddemo*.

    ```
    Get-AzureADApplication | where { $_.Homepage -like “sharepoint-iddemo” } | fl DisplayName, Homepage, ObjectID
    ```
4. Du bör få ett resultat som är liknande toohello som visas här. Kopiera hello ObjectID GUID toouse i hello nästa avsnitt.

    ```
    DisplayName : SharePoint
    Homepage    : https://sharepoint-iddemo.msappproxy.net/
    ObjectId    : 8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4
    ```

### <a name="update-hello-home-page-url"></a>Uppdatera hello URL-Adressen

I hello utföra samma PowerShell-modul som du använde för steg 1, hello följande steg:

1. Bekräfta att du har hello korrigera app och Ersätt *8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4* med hello ObjectID som du kopierade i föregående steg hello.

    ```
    Get-AzureADApplication -ObjectId 8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4.
    ```

 Nu när du har bekräftat hello app, är du redo tooupdate hello startsidan på följande sätt.

2. Skapa en tom programmet objektet toohold hello toomake önskade ändringar. Den här variabeln innehåller hello värden som du vill tooupdate. Inget har skapats i det här steget.

    ```
    $appnew = New-Object “Microsoft.Open.AzureAD.Model.Application”
    ```

3. Ange hello startsidan URL toohello-värdet som du vill. hello-värdet måste vara en underdomän sökvägen hello publicerade appen. Till exempel om du ändrar hello URL-Adressen från *https://sharepoint-iddemo.msappproxy.net/* för*https://sharepoint-iddemo.msappproxy.net/hybrid/*, appanvändare gå direkt toohello anpassad startsidan.

    ```
    $homepage = “https://sharepoint-iddemo.msappproxy.net/hybrid/”
    ```
4. Se hello uppdateringen med hjälp av hello GUID (objekt-ID) som du kopierade i ”steg 1: hitta hello ObjectID hello-appen”.

    ```
    Set-AzureADApplication -ObjectId 8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4 -Homepage $homepage
    ```
5. tooconfirm att ändra hello lyckades, starta om hello app.

    ```
    Get-AzureADApplication -ObjectId 8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4
    ```

>[!NOTE]
>Alla ändringar du gör toohello app kan återställa hello URL-Adressen. Om din startsida URL återställer upprepar du steg 2.

## <a name="next-steps"></a>Nästa steg

- [Aktivera fjärråtkomst tooSharePoint med Azure AD Application Proxy](application-proxy-enable-remote-access-sharepoint.md)
- [Aktivera Application Proxy i hello Azure-portalen](active-directory-application-proxy-enable.md)
