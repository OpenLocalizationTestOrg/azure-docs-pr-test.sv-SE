---
title: "Distribuera, anropa och autentisera webb-API: er och REST API: er för Azure Logic Apps | Microsoft Docs"
description: "Distribuera, autentisera och anropa webb-API: er och REST API: er i arbetsflöden för system-integration med Azure Logikappar"
keywords: "webb-API: er, REST API: er, kopplingar, arbetsflöden, system integreringar, autentisera"
services: logic-apps
author: stepsic-microsoft-com
manager: anneta
editor: 
documentationcenter: 
ms.assetid: f113005d-0ba6-496b-8230-c1eadbd6dbb9
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/26/2017
ms.author: LADocs; stepsic
ms.openlocfilehash: 88c62d5ab046d8cf4bce5d23b776e517bb0e1d87
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-call-and-authenticate-custom-apis-as-connectors-for-logic-apps"></a>Distribuera, anropa och autentisera anpassade API: er som kopplingar för logic apps

När du [skapa anpassade API: er](./logic-apps-create-api-app.md) som tillhandahåller åtgärder eller utlösare i logic apps arbetsflöden, måste du distribuera dina API: er innan du kan anropa dem. Och även om du kan anropa API: er från en logikapp för bästa möjliga upplevelse, lägger du till [Swagger-metadata](http://swagger.io/specification/) som beskriver ditt API åtgärder och parametrar. Den här Swagger-filen hjälper din API fungerar bättre och enklare integrera med logic apps.

Du kan distribuera dina API: er som [webbappar](../app-service-web/app-service-web-overview.md), men överväga att distribuera dina API: er som [API apps](../app-service-api/app-service-api-apps-why-best-platform.md), som kan underlätta ditt jobb när du skapar, värd och använda API: er i molnet och lokalt. Du behöver ändra någon kod i dina API: er, distribuera bara din kod till API-app. Du kan vara värd för dina API: er på [Azure App Service](../app-service/app-service-value-prop-what-is.md), en plattform-som-en tjänst (PaaS) erbjudande som ger ett sätt bästa enklaste och mest skalbara för API-värd.

Om du vill autentisera anrop från logikappar till dina API: er, kan du ställa in Azure Active Directory i Azure-portalen så du behöver uppdatera din kod. Eller så du behöver och ställer in användarautentisering via din API-koden.

## <a name="deploy-your-api-as-a-web-app-or-api-app"></a>Distribuera din API som en webbapp eller API-app

Innan du kan anropa ditt anpassade API från en logikapp, kan du distribuera din API som en webbapp eller API-app till Azure App Service. Även om du vill göra Swagger-dokument läsas av logik App Designer ange egenskaper för API-definition och aktivera [resursdelning för korsande ursprung (CORS)](../app-service-api/app-service-api-cors-consume-javascript.md#corsconfig) för ditt webbprogram eller API-app.

1. Välj ditt webbprogram eller API-app i Azure-portalen.

2. I bladet som öppnas under **API**, Välj **API-definition**. Ange den **plats för API-definition** till URL: en för swagger.json-filen.

   Webbadressen visas vanligtvis i det här formatet:`https://{name}.azurewebsites.net/swagger/docs/v1)`

   ![Länka till Swagger-filen för din anpassade API](media/logic-apps-custom-hosted-api/custom-api-swagger-url.png)

3. Under **API**, Välj **CORS**. Skapa princip för CORS för **tillåtna ursprung** till  **'*'** (Tillåt alla).

   Den här inställningen tillåter begäranden från logik App Designer.

   ![Tillåt begäranden från logik App Designer för ditt anpassade API](media/logic-apps-custom-hosted-api/custom-api-cors.png)

Mer information finns i följande artiklar:

* [Lägg till Swagger-metadata för ASP.NET web API: er](../app-service-api/app-service-api-dotnet-get-started.md#use-swagger-api-metadata-and-ui)
* [Distribuera ASP.NET web API: er till Azure App Service](../app-service-api/app-service-api-dotnet-get-started.md)

## <a name="call-your-custom-api-from-logic-app-workflows"></a>Anropa ditt anpassade API från logik app arbetsflöden

När du ställer in egenskaperna för API-definition och CORS ska din anpassade API utlösare och åtgärder bli tillgänglig att inkludera i logik app arbetsflödet. 

*  Om du vill visa de webbplatser som har Swagger URL: er kan du bläddra din prenumeration webbplatser i Logic Apps Designer.

*  Du kan visa tillgängliga åtgärder och indata genom att peka på en Swagger-dokument i [HTTP + Swagger åtgärden](../connectors/connectors-native-http-swagger.md).

*  För att anropa API: er, inklusive API: er som inte har eller exponera en Swagger-dokument kan du alltid skapa en begäran med den [HTTP-åtgärden](../connectors/connectors-native-http.md).

## <a name="authenticate-calls-to-your-custom-api"></a>Autentisera anrop till din anpassade API

Du kan skydda anrop till din anpassade API på följande sätt:

* [Inga kodändringar](#no-code): skydda din API med [Azure Active Directory (AD Azure)](../active-directory/active-directory-whatis.md) via Azure-portalen så du behöver uppdatera din kod eller distribuera din API.

  > [!NOTE]
  > Som standard ger inte Azure AD-autentisering som du aktiverar i Azure-portalen detaljerad behörighet. Den här autentiseringen låser exempelvis din API till bara en viss klient, inte till en specifik användare eller en app. 

* [Uppdatera din API-koden](#update-code): skydda din API genom tvingande [certifikatautentisering](#certificate), [grundläggande autentisering](#basic), eller [Azure AD authentication](#azure-ad-code) via kod.

<a name="no-code"></a>

### <a name="authenticate-calls-to-your-api-without-changing-code"></a>Autentisera anrop till ditt API utan att ändra koden

Här är de allmänna stegen för den här metoden:

1. Skapa två [Azure Active Directory (Azure AD) application identiteter](../app-service-api/app-service-api-dotnet-service-principal-auth.md): en för din logikapp och en för webbprogram (eller API-app).

2. Om du vill autentisera anrop till ditt API, använda autentiseringsuppgifter (klient-ID och hemligt) för den [tjänstens huvudnamn](../app-service-api/app-service-api-dotnet-service-principal-auth.md) som associeras med Azure AD-programidentitet för din logikapp.

3. Inkludera program-ID: N i din app logik-definition.

#### <a name="part-1-create-an-azure-ad-application-identity-for-your-logic-app"></a>Del 1: Skapa en Azure AD application identitet för din logikapp

Din logikapp använder den här identiteten för Azure AD-program för att autentisera mot Azure AD. Du behöver bara konfigurera den här identiteten en gång för din katalog. Exempelvis kan du använda samma identitet för dina logikappar trots att du kan skapa unika identiteter för varje logikapp. Du kan konfigurera dessa identiteter i Azure-portalen [klassiska Azure-portalen](#app-identity-logic-classic), eller Använd [PowerShell](#powershell).

**Skapa programidentiteten för logikappen i Azure-portalen**

1. I den [Azure-portalen](https://portal.azure.com "https://portal.azure.com"), Välj **Azure Active Directory**. 

2. Bekräfta att du är i samma katalog som ditt webbprogram eller API-app.

   > [!TIP]
   > Om du vill växla kataloger på din profil och välj en annan katalog. Eller välj **översikt** > **växel directory**.

3. På menyn directory under **hantera**, Välj **App registreringar** > **nya appregistrering**.

   > [!TIP]
   > Som standard visar listan över appar registreringar alla app registreringar i din katalog. Om du vill visa endast appen registreringar, bredvid sökrutan **Mina appar**. 

   ![Skapa ny appregistrering](./media/logic-apps-custom-hosted-api/new-app-registration-azure-portal.png)

4. Namnge din Programidentitet, lämna **programtyp** inställd på **webbapp / API**, ange en unik sträng formaterad som en domän för **inloggnings-URL**, och välj **skapa**.

   ![Ange namn och inloggning URL för Programidentitet](./media/logic-apps-custom-hosted-api/logic-app-identity-azure-portal.png)

   Programidentiteten som du skapade för din logikapp nu visas i listan över appar registreringar.

   ![Tillämpningsprogrammets identitet för din logikapp](./media/logic-apps-custom-hosted-api/logic-app-identity-created.png)

5. Välj din nya Programidentitet i programlistan registreringar. Kopiera och spara den **program-ID** ska användas som ”klient-ID” för din logikapp i del 3.

   ![Kopiera och spara program-ID för logikapp](./media/logic-apps-custom-hosted-api/logic-app-application-id.png)

6. Om din identitet programinställningar inte visas, väljer **inställningar** eller **alla inställningar**.

7. Under **API-åtkomst**, Välj **nycklar**. Under **beskrivning**, ange ett namn för din nyckel. Under **Expires**, markera en varaktighet för din nyckel.

   Den nyckel som du skapar fungerar som programidentiteten ”hemliga” eller lösenordet för din logikapp.

   ![Skapa en nyckel för logik app identitet](./media/logic-apps-custom-hosted-api/create-logic-app-identity-key-secret-password.png)

8. I verktygsfältet, välja **spara**. Under **värdet**, nyckeln visas nu. 
**Se till att kopiera och spara din nyckel** för senare användning eftersom nyckeln döljs när du lämnar bladet nycklar.

   När du konfigurerar din logikapp i del 3 kan ange du den här nyckeln ”secret” eller lösenord.

   ![Kopiera och spara nyckeln för senare](./media/logic-apps-custom-hosted-api/logic-app-copy-key-secret-password.png)

<a name="app-identity-logic-classic"></a>

**Skapa programidentiteten för din logikapp i den klassiska Azure-portalen**

1. I den klassiska Azure-portalen väljer [ **Active Directory**](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory).

2. Välj den katalog som du använder för ditt webbprogram eller API-app.

3. På den **program** , Välj **Lägg till** längst ned på sidan.

4. Namnge din identitet för programmet och välj **nästa** (HÖGERPIL).

5. Under **appegenskaper**, ange en unik sträng formaterad som en domän för **inloggnings-URL** och **App-ID URI**, och välj **Slutför** (markering).

6. På den **konfigurera** fliken, kopiera och spara den **klient-ID** för din logikapp ska användas i en del 3.

7. Under **nycklar**öppnar den **Markera varaktighet** lista. Markera en varaktighet för din nyckel.

   Den nyckel som du skapar fungerar som programidentiteten ”hemliga” eller lösenordet för din logikapp.

8. Längst ned på sidan Välj **spara**. Du kan behöva vänta några sekunder.

9. Under **nycklar**, Kom ihåg att kopiera och spara krypteringsnyckeln som nu visas. 

   När du konfigurerar din logikapp i del 3 kan ange du den här nyckeln ”secret” eller lösenord.

Mer information lär du dig hur du [konfigurera din App Service-program att använda Azure Active Directory-inloggningen](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md).

<a name="powershell"></a>

**Skapa programidentiteten för din logikapp i PowerShell**

Du kan utföra den här uppgiften via Azure Resource Manager med PowerShell. I PowerShell kör du följande kommandon:

1. `Switch-AzureMode AzureResourceManager`

2. `Add-AzureAccount`

3. `New-AzureADApplication -DisplayName "MyLogicAppID" -HomePage "http://mydomain.tld" -IdentifierUris "http://mydomain.tld" -Password "identity-password"`

4. Kom ihåg att kopiera den **klient-ID** (GUID för din Azure AD-klient), den **program-ID**, och det lösenord som du använde.

Mer information lär du dig hur du [skapar ett huvudnamn för tjänsten med PowerShell för att komma åt resurser](../azure-resource-manager/resource-group-authenticate-service-principal.md).

#### <a name="part-2-create-an-azure-ad-application-identity-for-your-web-app-or-api-app"></a>Del 2: Skapa en Azure AD-programidentitet för ditt webbprogram eller API-app

Om ditt webbprogram eller API-app redan har distribuerats, kan du aktivera autentisering och skapa programidentiteten i Azure-portalen. Annars kan du [aktivera autentisering när du distribuerar med en Azure Resource Manager-mall](#authen-deploy). 

**Skapa programidentiteten och aktivera autentisering i Azure portal för distribuerade appar**

1. I den [Azure-portalen](https://portal.azure.com "https://portal.azure.com"), söka efter och välj webbprogram eller API-app. 

2. Under **inställningar**, Välj **autentisering/auktorisering**. Under **App autentiseringen av tjänsten**, aktivera autentisering **på**. Under **autentiseringsproviders**, Välj **Azure Active Directory**.

   ![Aktivera autentisering](./media/logic-apps-custom-hosted-api/custom-web-api-app-authentication.png)

3. Nu skapa en programidentitet för ditt webbprogram eller API-app som visas här. På den **inställningarna för Azure Active Directory** bladet ange **hanteringsläge** till **Express**. Välj **Skapa nytt AD-App**. Namnge din identitet för programmet och välj **OK**. 

   ![Skapa programidentitet för ditt webbprogram eller API-app](./media/logic-apps-custom-hosted-api/custom-api-application-identity.png)

4. På den **autentisering / auktorisering bladet**, Välj **spara**.

Nu måste du hittar klient-ID och klient-ID för Programidentitet som är kopplad till ditt webbprogram eller API-app. Du kan använda dessa ID: N i del 3. Så att fortsätta med de här stegen för Azure-portalen eller [klassiska Azure-portalen](#find-id-classic).

**Hitta tillämpningsprogrammets identitet klient-ID och klient-ID för ditt webbprogram eller API-app i Azure-portalen**

1. Under **autentiseringsproviders**, Välj **Azure Active Directory**. 

   ![Välj ”Azure Active Directory”](./media/logic-apps-custom-hosted-api/custom-api-app-identity-client-id-tenant-id.png)

2. På den **inställningarna för Azure Active Directory** bladet ange **hanteringsläge** till **Avancerat**.

3. Kopiera den **klient-ID**, och spara det GUID för användning i del 3.

   > [!TIP] 
   > Om **klient-ID** och **utfärdar-Url** inte visas, försök att uppdatera Azure-portalen och upprepa steg 1.

4. Under **utfärdar-Url**, kopiera och spara bara GUID för en del 3. Du kan också använda detta GUID i ditt webbprogram eller mallen för distribution av API-app om det behövs.

   Detta GUID är din klient-GUID (”klient-ID”) och som ska visas i den här URL:`https://sts.windows.net/{GUID}`

5. Utan att spara ändringarna, Stäng av **inställningarna för Azure Active Directory** bladet.

<a name="find-id-classic"></a>

**Hitta tillämpningsprogrammets identitet klient-ID och klient-ID för ditt webbprogram eller API-app i den klassiska Azure-portalen**

1. I den klassiska Azure-portalen väljer [ **Active Directory**](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory).

2.  Välj en katalog som du använder för ditt webbprogram eller API-app.

3. I den **Sök** , söka efter och välj programidentiteten för ditt webbprogram eller API-app.

4. På den **konfigurera** fliken, kopiera den **klient-ID**, och spara det GUID för användning i del 3.

5. När du har fått klient-ID längst ned i den **konfigurera** , Välj **visa slutpunkter**.

6. Kopiera URL-Adressen för **Federation Metadata dokumentet**, och bläddra till URL: en.

7. Hitta roten i metadata-dokument som öppnar **EntityDescriptor ID** element som har en **ID för entiteterna** attribut i det här formuläret:`https://sts.windows.net/{GUID}` 

      GUID i det här attributet är din klient-GUID (klient-ID).

8. Kopiera klient-ID och spara detta ID för användning i en del 3 och även för att använda i ditt webbprogram eller mallen för distribution av API-app om det behövs.

Mer information finns i följande avsnitt:

* [Användarautentisering för API apps i Azure App Service](../app-service-api/app-service-api-dotnet-user-principal-auth.md)
* [Autentisering och auktorisering i Azure App Service](../app-service/app-service-authentication-overview.md)

<a name="authen-deploy"></a>

**Aktivera autentisering när du distribuerar med en Azure Resource Manager-mall**

Du måste fortfarande skapa en Azure AD-programidentitet för ditt webbprogram eller API-app som skiljer sig från appen identitet för din logikapp. Följ föregående steg i en del 2 för Azure-portalen för att skapa programidentiteten. Du kan också följa stegen i del 1, men se till att använda din webbapp eller API-app faktiska `https://{URL}` för **inloggnings-URL** och **App-ID URI**. Du måste spara både klient-ID och klient-ID för användning i din app Distributionsmall och för del 3 från de här stegen.

> [!NOTE]
> När du skapar Azure AD-programidentitet för ditt webbprogram eller API-app måste du använda den Azure-portalen eller klassiska Azure-portalen i stället PowerShell. PowerShell-cmdlet ställa inte in behörigheterna som krävs för att logga in användare på en webbplats.

När du har fått klient-ID och klient-ID är dessa ID: N som en subresource av ditt webbprogram eller API-app i mallen för distribution:

   ```
   "resources": [
       {
           "apiVersion": "2015-08-01",
           "name": "web",
           "type": "config",
           "dependsOn": ["[concat('Microsoft.Web/sites/','parameters('webAppName'))]"],
           "properties": {
               "siteAuthEnabled": true,
               "siteAuthSettings": {
                 "clientId": "{client-ID}",
                 "issuer": "https://sts.windows.net/{tenant-ID}/",
               }
           }
       }
   ]
   ```

Du distribuerar automatiskt en tom webbapp och en logikapp tillsammans med Azure Active Directory-autentisering, [visa den fullständiga mallen](https://github.com/Azure/azure-quickstart-templates/tree/master/201-logic-app-custom-api/azuredeploy.json), eller klicka på **till Azure** här:

[![Distribuera till Azure](media/logic-apps-custom-hosted-api/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-logic-app-custom-api%2Fazuredeploy.json)

#### <a name="part-3-populate-the-authorization-section-in-your-logic-app"></a>Del 3: Lägg till i avsnittet om auktorisering i din logikapp

Den tidigare mallen har redan det här tillståndet avsnittet Konfigurera, men om du använder direkt logikappen, måste du inkludera avsnittet fullständig behörighet.

Öppna din app definition för logiken i kodvy, gå till den **HTTP** delen åtgärd att hitta den **auktorisering** avsnittet och inkludera den här raden:

`{"tenant": "{tenant-ID}", "audience": "{client-ID-from-Part-2-web-app-or-API app}", "clientId": "{client-ID-from-Part-1-logic-app}", "secret": "{key-from-Part-1-logic-app}", "type": "ActiveDirectoryOAuth" }`

| Element | Beskrivning |
| ------- | ----------- |
| Klient |GUID för Azure AD-klient |
| målgrupp |Krävs. GUID för målresursen som du vill komma åt - klient-ID från programidentiteten för ditt webbprogram eller API-app |
| clientId |GUID för klienten som begär åtkomst till - klient-ID från programidentiteten för din logikapp |
| hemligt |Krävs. Nyckel eller lösenord från programidentiteten för klienten som begär åtkomst-token |
| typ |Autentiseringstypen. Värdet är för ActiveDirectoryOAuth autentisering `ActiveDirectoryOAuth`. |

Exempel:

```
{
   ...
   "actions": {
      "some-action": {
         "conditions": [],
         "inputs": {
            "method": "post",
            "uri": "https://your-api-azurewebsites.net/api/your-method",
            "authentication": {
               "tenant": "tenant-ID",
               "audience": "client-ID-from-azure-ad-app-for-web-app-or-api-app",
               "clientId": "client-ID-from-azure-ad-app-for-logic-app",
               "secret": "key-from-azure-ad-app-for-logic-app",
               "type": "ActiveDirectoryOAuth"
            }
         },
      }
   }
}
```

<a name="update-code"></a>

### <a name="secure-api-calls-through-code"></a>Säker API-anrop med kod

<a name="certificate"></a>

#### <a name="certificate-authentication"></a>Certifikatautentisering

Du kan använda klientcertifikat för att validera inkommande förfrågningar från din logikapp till ditt webbprogram eller API-app. Om du vill konfigurera din kod Läs [hur du konfigurerar TLS ömsesidig autentisering](../app-service-web/app-service-web-configure-tls-mutual-auth.md).

I den **auktorisering** och inkludera den här raden: 

`{"type": "clientcertificate", "password": "password", "pfx": "long-pfx-key"}`

| Element | Beskrivning |
| ------- | ----------- |
| typ |Krävs. Autentiseringstypen. För SSL-klientcertifikat, värdet måste vara `ClientCertificate`. |
| lösenord |Krävs. Lösenord för att komma åt klientcertifikatet (PFX-fil) |
| Pfx |Krävs. Base64-kodad innehållet i klientens certifikat (PFX-fil) |

<a name="basic"></a>

#### <a name="basic-authentication"></a>Grundläggande autentisering

Du kan använda grundläggande autentisering, till exempel användarnamn och lösenord för att validera inkommande förfrågningar från din logikapp till ditt webbprogram eller API-app. Grundläggande autentisering är ett vanligt mönster och du kan använda den här autentiseringen på alla språk som används för att skapa ditt webbprogram eller API-app.

I den **auktorisering** och inkludera den här raden:

`{"type": "basic", "username": "username", "password": "password"}`.

| Element | Beskrivning |
| --- | --- |
| typ |Krävs. Autentiseringstypen. För grundläggande autentisering, värdet måste vara `Basic`. |
| användarnamn |Krävs. Användarnamn för autentisering |
| lösenord |Krävs. Lösenordet för autentisering |

<a name="azure-ad-code"></a>

#### <a name="azure-active-directory-authentication-through-code"></a>Azure Active Directory-autentisering via kod

Som standard ger inte Azure AD-autentisering som du aktiverar i Azure-portalen detaljerad behörighet. Den här autentiseringen låser exempelvis din API till bara en viss klient, inte till en specifik användare eller en app. 

För att begränsa API-åtkomst till din logikapp med kod, extrahera det huvud som innehåller JSON web token (JWT). Kontrollera Anroparens identitet och avvisar begäranden som inte matchar.

Lär dig fortsätter att implementera den här autentiseringen helt i din kod och inte använda Azure-portalen så [autentiseras med lokal Active Directory i din Azure-app](../app-service-web/web-sites-authentication-authorization.md).

Du måste följa de här stegen för att skapa en programidentitet för din logikapp och använda identitet för att anropa ditt API.

## <a name="next-steps"></a>Nästa steg

* [Kontrollera logik app prestanda med diagnostiska loggar och aviseringar](logic-apps-monitor-your-logic-apps.md)