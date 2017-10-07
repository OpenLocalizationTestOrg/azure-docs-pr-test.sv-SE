---
title: "aaaConfigure autentisering och auktorisering för ett anpassat program som anropar hello Azure tid serien Insights API | Microsoft Docs"
description: "Den här självstudiekursen beskrivs hur tooconfigure autentisering och auktorisering för ett anpassat program som anropar hello Azure tid serien Insights API"
keywords: 
services: time-series-insights
documentationcenter: 
author: dmdenmsft
manager: almineev
editor: cgronlun
ms.assetid: 
ms.service: time-series-insights
ms.devlang: na
ms.topic: how-to-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/24/2017
ms.author: dmden
ms.openlocfilehash: 5043468bfc2af3c0d27e8602508d92ba2848409e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="authentication-and-authorization-for-azure-time-series-insights-api"></a>Autentisering och auktorisering för Azure tid serien Insights API

Den här artikeln förklarar hur tooconfigure ett anpassat program som anropar hello Azure tid serien Insights API.

## <a name="service-principal"></a>Tjänstens huvudnamn

Det här avsnittet beskrivs hur tooconfigure ett program tooaccess hello tid serien Insights API uppdrag hello program. hello program kan sedan fråga data eller publicera referensdata i hello tid serien insikter miljö med autentiseringsuppgifter och inte hello användarens autentiseringsuppgifter.

När du har ett program som behöver tooaccess tid serien insikter måste du konfigurera ett program för Azure Active Directory och tilldelar hello principerna dataåtkomst i hello tid serien insikter miljö. Den här metoden är bättre toorunning hello app enligt dina autentiseringsuppgifter eftersom:

* Du kan tilldela behörigheter toohello app identitet som skiljer sig från din egen behörigheter. Dessa behörigheter normalt begränsade tooexactly vilka hello program behöver toodo. Du kan till exempel tillåta hello app tooonly läsa data i en viss tid serien insikter-miljö.
* Du har inte toochange hello app autentiseringsuppgifter ändrar dina ansvarsområden.
* Du kan använda ett certifikat eller en nyckel tooautomate autentisering när du kör ett oövervakat skript.

Den här artikeln visar hur tooperform de går igenom hello Azure-portalen. Den fokuserar på en enskild klient-program där programmet hello är avsedda toorun i en enda organisation. Stöd för en innehavare program används vanligtvis för line-of-business-program som körs i din organisation.

hello installationsprogrammet flödet består av tre huvudsakliga steg:

1. Skapa ett program i Azure Active Directory.
2. Auktorisera den här tooaccess hello tid serien insikter programmiljö.
3. Använd hello program-ID och nyckel tooacquire token toohello `"https://api.timeseries.azure.com/"` publik eller resursen. hello token kan sedan använda toocall hello tid serien Insights API.

Här följer hello detaljerade anvisningar:

1. Markera i hello Azure-portalen, **Azure Active Directory** > **App registreringar** > **nya appregistrering**.

   ![Ny programmet registrering i Azure Active Directory](media/authentication-and-authorization/active-directory-new-application-registration.png)  

2. Ge hello programmet ett namn, Välj hello typen toobe **webbapp / API**, Välj en giltig URI för **inloggnings-URL**, och klicka på **skapa**.

   ![Skapa hello program i Azure Active Directory](media/authentication-and-authorization/active-directory-create-web-api-application.png)

3. Välj ditt nya program och kopiera dess program-ID tooyour favoritprogram för textredigering.

   ![Kopiera hello program-ID](media/authentication-and-authorization/active-directory-copy-application-id.png)

4. Välj **nycklar**, ange hello nyckelnamn, Välj hello förfallodatum, och på **spara**.

   ![Välj programnycklar](media/authentication-and-authorization/active-directory-application-keys.png)

   ![Ange hello nyckelnamn och upphör att gälla och klicka på Spara](media/authentication-and-authorization/active-directory-application-keys-save.png)

5. Kopiera hello viktiga tooyour favoritprogram för textredigering.

   ![Kopiera hello tangent](media/authentication-and-authorization/active-directory-copy-application-key.png)

6. Hello tid serien insikter miljö, Välj **principerna dataåtkomst** och på **Lägg till**.

   ![Lägga till nya data access princip toohello tid serien insikter miljön](media/authentication-and-authorization/time-series-insights-data-access-policies-add.png)

7. I hello **Välj användare** dialogrutan, klistra in hello programnamn (från steg 2) eller program-ID (från steg 3).

   ![Hitta ett program i dialogrutan Välj användare för hello](media/authentication-and-authorization/time-series-insights-data-access-policies-select-user.png)

8. Välj hello roll (**Reader** för datafrågor, **deltagare** för datafrågor och ändra referensdata) och klicka på **Ok**.

   ![Välj läsaren eller deltagare i dialogrutan för hello Välj roll](media/authentication-and-authorization/time-series-insights-data-access-policies-select-role.png)

9. Spara hello princip genom att klicka på **Ok**.

10. Använd hello program-ID (från steg 3) och applikationstoken nyckel (från steg 5) tooacquire hello uppdrag hello program. hello token kan sedan skickas i hello `Authorization` huvud när hello programmet anrop hello tid serien Insights API.

    Om du använder C#, kan du använda följande kod tooacquire hello token uppdrag hello programmet hello. Ett komplett exempel finns [fråga data med hjälp av C#](time-series-insights-query-data-csharp.md).

    ```csharp
    var authenticationContext = new AuthenticationContext(
        "https://login.microsoftonline.com/common",
        TokenCache.DefaultShared);

    AuthenticationResult token = await authenticationContext.AcquireTokenAsync(
        // Set hello resource URI toohello Azure Time Series Insights API
        resource: "https://api.timeseries.azure.com/", 
        clientCredential: new ClientCredential(
            // Application ID of application registered in Azure Active Directory
            clientId: "1bc3af48-7e2f-4845-880a-c7649a6470b8", 
            // Application key of hello application that's registered in Azure Active Directory
            clientSecret: "aBcdEffs4XYxoAXzLB1n3R2meNCYdGpIGBc2YC5D6L2="));

    string accessToken = token.AccessToken;
    ```

## <a name="next-steps"></a>Nästa steg

Använda hello program-ID och nyckel i ditt program. Exempelkod som anropar hello tid serien Insights API finns [fråga data med hjälp av C#](time-series-insights-query-data-csharp.md).

## <a name="see-also"></a>Se även

* [Frågor API](/rest/api/time-series-insights/time-series-insights-reference-queryapi) hello fullständig fråge-API-referens
* [Skapa en tjänstens huvudnamn i hello Azure-portalen](../azure-resource-manager/resource-group-create-service-principal-portal.md)
