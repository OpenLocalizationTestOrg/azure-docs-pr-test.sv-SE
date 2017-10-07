---
title: "aaaDeploy, anropar och autentisera webb-API: er och REST API: er för Azure Logic Apps | Microsoft Docs"
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
ms.openlocfilehash: ca1e4f28196b21f43b7c9ab94029684121e36f63
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-call-and-authenticate-custom-apis-as-connectors-for-logic-apps"></a>Distribuera, anropa och autentisera anpassade API: er som kopplingar för logic apps

När du [skapa anpassade API: er](./logic-apps-create-api-app.md) som tillhandahåller åtgärder eller utlösare toouse i logic apps arbetsflöden, måste du distribuera dina API: er innan du kan anropa dem. Och även om du kan anropa API: er från en logikapp för hello bästa möjliga upplevelse, lägger du till [Swagger-metadata](http://swagger.io/specification/) som beskriver ditt API åtgärder och parametrar. Den här Swagger-filen hjälper din API fungerar bättre och enklare integrera med logic apps.

Du kan distribuera dina API: er som [webbappar](../app-service-web/app-service-web-overview.md), men överväga att distribuera dina API: er som [API apps](../app-service-api/app-service-api-apps-why-best-platform.md), som kan underlätta ditt jobb när du skapar, värd och använda API: er i hello molnet och lokalt. Du saknar toochange någon kod i dina API: er, distribuera bara din kod tooan API-app. Du kan vara värd för dina API: er på [Azure App Service](../app-service/app-service-value-prop-what-is.md), en plattform-som-en tjänst (PaaS) erbjudande som ger hello bästa enklaste och mest skalbara sätt för API-värd.

tooauthenticate anrop från logic apps tooyour API: er, du kan ställa in Azure Active Directory i hello Azure-portalen så att du inte behöver tooupdate din kod. Eller så du behöver och ställer in användarautentisering via din API-koden.

## <a name="deploy-your-api-as-a-web-app-or-api-app"></a>Distribuera din API som en webbapp eller API-app

Innan du kan anropa ditt anpassade API från en logikapp, kan du distribuera din API som en webbapp eller API app tooAzure Apptjänst. Dessutom toomake Swagger-dokument läsas av hello logik App Designer och ange egenskaper för hello API-definition och aktivera [resursdelning för korsande ursprung (CORS)](../app-service-api/app-service-api-cors-consume-javascript.md#corsconfig) för ditt webbprogram eller API-app.

1. Välj ditt webbprogram eller API-app i hello Azure-portalen.

2. Hello-bladet som öppnas under **API**, Välj **API-definition**. Ange hello **plats för API-definition** toohello URL för swagger.json-filen.

   Vanligtvis visas hello URL i följande format:`https://{name}.azurewebsites.net/swagger/docs/v1)`

   ![TooSwagger länkfil för din anpassade API](media/logic-apps-custom-hosted-api/custom-api-swagger-url.png)

3. Under **API**, Välj **CORS**. Skapa princip för hello CORS för **tillåtna ursprung** för**'*'** (Tillåt alla).

   Den här inställningen tillåter begäranden från logik App Designer.

   ![Tillåta begäranden från logik App Designer tooyour anpassade API](media/logic-apps-custom-hosted-api/custom-api-cors.png)

Mer information finns i följande artiklar:

* [Lägg till Swagger-metadata för ASP.NET web API: er](../app-service-api/app-service-api-dotnet-get-started.md#use-swagger-api-metadata-and-ui)
* [Distribuera ASP.NET web API: er tooAzure Apptjänst](../app-service-api/app-service-api-dotnet-get-started.md)

## <a name="call-your-custom-api-from-logic-app-workflows"></a>Anropa ditt anpassade API från logik app arbetsflöden

När du har skapat hello API-egenskaper och CORS ska din anpassade API utlösare och åtgärder bli tillgänglig för tooinclude i logik app arbetsflödet. 

*  Du kan bläddra din prenumeration webbplatser i hello Logic Apps Designer tooview hello webbplatser som har Swagger URL: er.

*  tooview tillgängliga åtgärder och indata genom att peka på ett Swagger-dokument, använda hello [HTTP + Swagger åtgärden](../connectors/connectors-native-http-swagger.md).

*  toocall API: er, inklusive API: er som inte har eller exponera en Swagger-dokument alltid kan du skapa en begäran med hello [HTTP-åtgärden](../connectors/connectors-native-http.md).

## <a name="authenticate-calls-tooyour-custom-api"></a>Autentisera anrop tooyour anpassade API

Du kan skydda anrop tooyour anpassade API på följande sätt:

* [Inga kodändringar](#no-code): skydda din API med [Azure Active Directory (AD Azure)](../active-directory/active-directory-whatis.md) via hello Azure-portalen, så du inte har tooupdate koden eller omdistribuera ditt API.

  > [!NOTE]
  > Som standard ger inte hello Azure AD-autentisering som du aktiverar i hello Azure-portalen detaljerad behörighet. Den här autentiseringen låser exempelvis API-toojust en viss klient, inte tooa specifik användare eller app. 

* [Uppdatera din API-koden](#update-code): skydda din API genom tvingande [certifikatautentisering](#certificate), [grundläggande autentisering](#basic), eller [Azure AD authentication](#azure-ad-code) via kod.

<a name="no-code"></a>

### <a name="authenticate-calls-tooyour-api-without-changing-code"></a>Autentisera anrop tooyour API utan att ändra koden

Här är hello allmänna steg för den här metoden:

1. Skapa två [Azure Active Directory (Azure AD) application identiteter](../app-service-api/app-service-api-dotnet-service-principal-auth.md): en för din logikapp och en för webbprogram (eller API-app).

2. tooauthenticate anrop tooyour API, använda hello autentiseringsuppgifter (klient-ID och hemligt) för hello [tjänstens huvudnamn](../app-service-api/app-service-api-dotnet-service-principal-auth.md) som associeras med hello Azure AD programidentitet för din logikapp.

3. Inkludera hello program-ID: N i din app logik-definition.

#### <a name="part-1-create-an-azure-ad-application-identity-for-your-logic-app"></a>Del 1: Skapa en Azure AD application identitet för din logikapp

Din logikapp använder den här Azure AD application identitet tooauthenticate mot Azure AD. Du har bara tooset upp den här identiteten en gång för din katalog. Du kan till exempel välja toouse hello samma identitet för dina logikappar trots att du kan skapa unika identiteter för varje logikapp. Du kan konfigurera dessa identiteter i hello Azure-portalen [klassiska Azure-portalen](#app-identity-logic-classic), eller Använd [PowerShell](#powershell).

**Skapa hello programidentitet för din logikapp i hello Azure-portalen**

1. I hello [Azure-portalen](https://portal.azure.com "https://portal.azure.com"), Välj **Azure Active Directory**. 

2. Bekräfta att du är i hello samma katalog som ditt webbprogram eller API-app.

   > [!TIP]
   > tooswitch kataloger på din profil och välj en annan katalog. Eller välj **översikt** > **växel directory**.

3. På menyn directory hello under **hantera**, Välj **App registreringar** > **nya appregistrering**.

   > [!TIP]
   > Som standard visar hello registreringar programlistan alla app registreringar i din katalog. tooview endast app-registreringar, nästa toohello Sök-rutan, Välj **Mina appar**. 

   ![Skapa ny appregistrering](./media/logic-apps-custom-hosted-api/new-app-registration-azure-portal.png)

4. Namnge din Programidentitet, lämna **programtyp** ange för**webbapp / API**, ange en unik sträng formaterad som en domän för **inloggnings-URL**, och välj  **Skapa**.

   ![Ange namn och inloggning URL för Programidentitet](./media/logic-apps-custom-hosted-api/logic-app-identity-azure-portal.png)

   hello Programidentitet som du skapade för din logikapp nu visas i hello app registreringar.

   ![Tillämpningsprogrammets identitet för din logikapp](./media/logic-apps-custom-hosted-api/logic-app-identity-created.png)

5. Markera din nya Programidentitet i hello app registreringar. Kopiera och spara hello **program-ID** toouse som hello ”klient-ID” för din logikapp i del 3.

   ![Kopiera och spara program-ID för logikapp](./media/logic-apps-custom-hosted-api/logic-app-application-id.png)

6. Om din identitet programinställningar inte visas, väljer **inställningar** eller **alla inställningar**.

7. Under **API-åtkomst**, Välj **nycklar**. Under **beskrivning**, ange ett namn för din nyckel. Under **Expires**, markera en varaktighet för din nyckel.

   hello-nyckel som du skapar fungerar som hello tillämpningsprogrammets identitet ”hemliga” eller lösenordet för din logikapp.

   ![Skapa en nyckel för logik app identitet](./media/logic-apps-custom-hosted-api/create-logic-app-identity-key-secret-password.png)

8. Välj hello verktygsfältet **spara**. Under **värdet**, nyckeln visas nu. 
**Se till att toocopy och spara din nyckel** för senare användning eftersom hello nyckeln döljs när du lämnar hello viktiga bladet.

   När du konfigurerar din logikapp i del 3, ange den här nyckeln som hello ”hemliga” eller lösenord.

   ![Kopiera och spara nyckeln för senare](./media/logic-apps-custom-hosted-api/logic-app-copy-key-secret-password.png)

<a name="app-identity-logic-classic"></a>

**Skapa hello programidentitet för din logikapp i hello klassiska Azure-portalen**

1. I Hej klassiska Azure-portalen, Välj [ **Active Directory**](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory).

2. Välj hello samma katalog som du använder för ditt webbprogram eller API-app.

3. På hello **program** , Välj **Lägg till** på hello hello sidans nederkant.

4. Namnge din identitet för programmet och välj **nästa** (HÖGERPIL).

5. Under **appegenskaper**, ange en unik sträng formaterad som en domän för **inloggnings-URL** och **App-ID URI**, och välj **Slutför** (markering).

6. På hello **konfigurera** fliken, kopiera och spara hello **klient-ID** för din app toouse för logiken i del 3.

7. Under **nycklar**öppnar hello **Markera varaktighet** lista. Markera en varaktighet för din nyckel.

   hello-nyckel som du skapar fungerar som hello tillämpningsprogrammets identitet ”hemliga” eller lösenordet för din logikapp.

8. Längst ned hello hello-sidan, Välj **spara**. Du kanske toowait några sekunder.

9. Under **nycklar**, se till att toocopy och spara hello-nyckel som nu visas. 

   När du konfigurerar din logikapp i del 3, ange den här nyckeln som hello ”hemliga” eller lösenord.

Mer information lär du dig hur för [konfigurera Azure Active Directory din App tjänstinloggning programmet toouse](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md).

<a name="powershell"></a>

**Skapa hello programidentitet för din logikapp i PowerShell**

Du kan utföra den här uppgiften via Azure Resource Manager med PowerShell. I PowerShell kör du följande kommandon:

1. `Switch-AzureMode AzureResourceManager`

2. `Add-AzureAccount`

3. `New-AzureADApplication -DisplayName "MyLogicAppID" -HomePage "http://mydomain.tld" -IdentifierUris "http://mydomain.tld" -Password "identity-password"`

4. Se till att toocopy hello **klient-ID** (GUID för din Azure AD-klient) hello **program-ID**, och hello-lösenord som du använde.

Mer information lär du dig hur för [skapa ett huvudnamn för tjänsten med PowerShell tooaccess resurser](../azure-resource-manager/resource-group-authenticate-service-principal.md).

#### <a name="part-2-create-an-azure-ad-application-identity-for-your-web-app-or-api-app"></a>Del 2: Skapa en Azure AD-programidentitet för ditt webbprogram eller API-app

Om ditt webbprogram eller API-app redan har distribuerats, kan du aktivera autentisering och skapa hello Programidentitet i hello Azure-portalen. Annars kan du [aktivera autentisering när du distribuerar med en Azure Resource Manager-mall](#authen-deploy). 

**Skapa hello Programidentitet och aktivera autentisering i hello Azure portal för distribuerade appar**

1. I hello [Azure-portalen](https://portal.azure.com "https://portal.azure.com"), söka efter och välj webbprogram eller API-app. 

2. Under **inställningar**, Välj **autentisering/auktorisering**. Under **App autentiseringen av tjänsten**, aktivera autentisering **på**. Under **autentiseringsproviders**, Välj **Azure Active Directory**.

   ![Aktivera autentisering](./media/logic-apps-custom-hosted-api/custom-web-api-app-authentication.png)

3. Nu skapa en programidentitet för ditt webbprogram eller API-app som visas här. På hello **inställningarna för Azure Active Directory** bladet ange **hanteringsläge** för**Express**. Välj **Skapa nytt AD-App**. Namnge din identitet för programmet och välj **OK**. 

   ![Skapa programidentitet för ditt webbprogram eller API-app](./media/logic-apps-custom-hosted-api/custom-api-application-identity.png)

4. På hello **autentisering / auktorisering bladet**, Välj **spara**.

Du måste nu hitta hello klient-ID och klient-ID för hello Programidentitet som är kopplad till ditt webbprogram eller API-app. Du kan använda dessa ID: N i del 3. Så att fortsätta med de här stegen för hello Azure-portalen eller [klassiska Azure-portalen](#find-id-classic).

**Hitta tillämpningsprogrammets identitet klient-ID och klient-ID för ditt webbprogram eller API-app i hello Azure-portalen**

1. Under **autentiseringsproviders**, Välj **Azure Active Directory**. 

   ![Välj ”Azure Active Directory”](./media/logic-apps-custom-hosted-api/custom-api-app-identity-client-id-tenant-id.png)

2. På hello **inställningarna för Azure Active Directory** bladet ange **hanteringsläge** för**Avancerat**.

3. Kopiera hello **klient-ID**, och spara det GUID för användning i del 3.

   > [!TIP] 
   > Om **klient-ID** och **utfärdar-Url** inte visas, försök att uppdatera hello Azure-portalen och upprepa steg 1.

4. Under **utfärdar-Url**, kopiera och spara bara hello GUID för en del 3. Du kan också använda detta GUID i ditt webbprogram eller mallen för distribution av API-app om det behövs.

   Detta GUID är din klient-GUID (”klient-ID”) och som ska visas i den här URL:`https://sts.windows.net/{GUID}`

5. Stäng utan att spara dina ändringar hello **inställningarna för Azure Active Directory** bladet.

<a name="find-id-classic"></a>

**Hitta tillämpningsprogrammets identitet klient-ID och klient-ID för ditt webbprogram eller API-app i hello klassiska Azure-portalen**

1. I Hej klassiska Azure-portalen, Välj [ **Active Directory**](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory).

2.  Välj hello-katalog som du använder för ditt webbprogram eller API-app.

3. I hello **Sök** , söka efter och välj hello programidentitet för ditt webbprogram eller API-app.

4. På hello **konfigurera** fliken, kopiera hello **klient-ID**, och spara det GUID för användning i del 3.

5. När du har fått hello klient-ID, längst ned hello hello **konfigurera** , Välj **visa slutpunkter**.

6. Kopiera hello URL: en för **Federation Metadata dokumentet**, och bläddra toothat URL.

7. I hello metadata dokument som öppnas, hitta hello rot **EntityDescriptor ID** element som har en **ID för entiteterna** attribut i det här formuläret:`https://sts.windows.net/{GUID}` 

      hello GUID i det här attributet är din klient-GUID (klient-ID).

8. Kopiera hello klient-ID och spara detta ID för användning i del 3 och toouse i ditt webbprogram eller mallen för distribution av API-app om det behövs.

Mer information finns i följande avsnitt:

* [Användarautentisering för API apps i Azure App Service](../app-service-api/app-service-api-dotnet-user-principal-auth.md)
* [Autentisering och auktorisering i Azure App Service](../app-service/app-service-authentication-overview.md)

<a name="authen-deploy"></a>

**Aktivera autentisering när du distribuerar med en Azure Resource Manager-mall**

Du måste fortfarande skapa en Azure AD-programidentitet för ditt webbprogram eller API-app som skiljer sig från hello app identitet för din logikapp. toocreate hello Programidentitet, följ hello föregående steg i en del 2 för hello Azure-portalen. Du kan också hello åtgärderna i del 1, men gör att toouse ditt webbprogram eller API-app faktiska `https://{URL}` för **inloggnings-URL** och **App-ID URI**. Från de här stegen har du toosave både hello-klient-ID och klient-ID för användning i din app Distributionsmall och för del 3.

> [!NOTE]
> När du skapar hello Azure AD programidentitet för ditt webbprogram eller API-app måste du använda hello Azure-portalen eller klassiska Azure-portalen i stället PowerShell. hello PowerShell cmdlet ställa inte in hello krävs behörigheter toosign användare till en webbplats.

När du har fått hello klient-ID och klient-ID är dessa ID: N som en subresource av ditt webbprogram eller API-app i mallen för distribution:

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

tooautomatically distribuera ett tomt webbprogram och en logikapp tillsammans med Azure Active Directory-autentisering, [visa hello fullständig mall här](https://github.com/Azure/azure-quickstart-templates/tree/master/201-logic-app-custom-api/azuredeploy.json), eller klicka på **distribuera tooAzure** här:

[![Distribuera tooAzure](media/logic-apps-custom-hosted-api/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-logic-app-custom-api%2Fazuredeploy.json)

#### <a name="part-3-populate-hello-authorization-section-in-your-logic-app"></a>Del 3: Fylla hello auktorisering avsnitt i din logikapp

tidigare hello-mall använder redan det här tillståndet avsnittet Konfigurera, men om du använder direkt hello logikapp, måste du inkludera hello fullständig behörighet avsnitt.

Öppna din app logik-definition i kodvy, gå toohello **HTTP** åtgärd, hitta hello **auktorisering** avsnittet och inkludera den här raden:

`{"tenant": "{tenant-ID}", "audience": "{client-ID-from-Part-2-web-app-or-API app}", "clientId": "{client-ID-from-Part-1-logic-app}", "secret": "{key-from-Part-1-logic-app}", "type": "ActiveDirectoryOAuth" }`

| Element | Beskrivning |
| ------- | ----------- |
| Klient |hello GUID för hello Azure AD-klient |
| målgrupp |Krävs. hello GUID för hello målresurs som du vill tooaccess - hello klient-ID från hello programidentitet för ditt webbprogram eller API-app |
| clientId |hello GUID för hello-klient som begär åtkomst - hello klient-ID från hello programidentitet för din logikapp |
| hemligt |Krävs. hello nyckel eller lösenord från hello programidentitet för hello-klient som begär hello åtkomst-token |
| typ |hello autentiseringstypen. Hello-värdet är för ActiveDirectoryOAuth autentisering `ActiveDirectoryOAuth`. |

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

toovalidate hello inkommande förfrågningar från din logik app tooyour webbapp eller API-app, kan du använda klientcertifikat. Läs tooset upp din kod [hur tooconfigure TLS ömsesidig autentisering](../app-service-web/app-service-web-configure-tls-mutual-auth.md).

I hello **auktorisering** och inkludera den här raden: 

`{"type": "clientcertificate", "password": "password", "pfx": "long-pfx-key"}`

| Element | Beskrivning |
| ------- | ----------- |
| typ |Krävs. hello autentiseringstypen. För SSL-klientcertifikat, hello-värdet måste vara `ClientCertificate`. |
| lösenord |Krävs. hello-lösenord för att komma åt hello-klientcertifikat (PFX-fil) |
| Pfx |Krävs. Base64-kodad innehållet i hello-klientcertifikat (PFX-fil) |

<a name="basic"></a>

#### <a name="basic-authentication"></a>Grundläggande autentisering

toovalidate inkommande förfrågningar från din logik app tooyour webbapp eller API-app, kan du använda grundläggande autentisering, till exempel användarnamn och lösenord. Grundläggande autentisering är ett vanligt mönster och du kan använda den här autentiseringen i alla språk som används för toobuild ditt webbprogram eller API-app.

I hello **auktorisering** och inkludera den här raden:

`{"type": "basic", "username": "username", "password": "password"}`.

| Element | Beskrivning |
| --- | --- |
| typ |Krävs. hello autentiseringstypen. För grundläggande autentisering hello-värdet måste vara `Basic`. |
| användarnamn |Krävs. hello användarnamn för autentisering |
| lösenord |Krävs. hello lösenord för autentisering |

<a name="azure-ad-code"></a>

#### <a name="azure-active-directory-authentication-through-code"></a>Azure Active Directory-autentisering via kod

Som standard ger inte hello Azure AD-autentisering som du aktiverar i hello Azure-portalen detaljerad behörighet. Den här autentiseringen låser exempelvis API-toojust en viss klient, inte tooa specifik användare eller app. 

toorestrict API åtkomst tooyour logikapp via kod, extrahera hello-huvudet som har hello JSON web token (JWT). Kontrollera hello Anroparens identitet och avvisar begäranden som inte matchar.

Dessutom kommer tooimplement den här autentiseringen helt i din egen kod och inte använda hello Azure-portalen Lär dig hur för [autentiseras med lokal Active Directory i din Azure-app](../app-service-web/web-sites-authentication-authorization.md).

toocreate en programidentitet för din logikapp och använda den identitet toocall ditt API, måste du följa hello föregående steg.

## <a name="next-steps"></a>Nästa steg

* [Kontrollera logik app prestanda med diagnostiska loggar och aviseringar](logic-apps-monitor-your-logic-apps.md)