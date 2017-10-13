---
title: "Azure Active Directory B2C: Referera: anpassa Användargränssnittet för en användare resa med anpassade principer | Microsoft Docs"
description: "Ett ämne på Azure Active Directory B2C anpassade principer"
services: active-directory-b2c
documentationcenter: 
author: rojasja
manager: krassk
editor: rojasja
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 04/25/2017
ms.author: joroja
ms.openlocfilehash: 68f40aa638a687398512278a0b77d1ba392859cf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="customize-the-ui-of-a-user-journey-with-custom-policies"></a>Anpassa Användargränssnittet för en användare resa med anpassade principer

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

> [!NOTE]
> Den här artikeln är en avancerad beskrivning av hur anpassningar fungerar och hur du aktiverar med B2C anpassade principer, med hjälp av ramverket identitet upplevelse


En smidig användarupplevelse är nyckel för alla business-to-consumer-lösningar. Med sömlös användarupplevelse menar vi en upplevelse på enheten eller webbläsaren, där en användare resa via vår tjänst inte kan särskiljas från som kundservice som de använder.

## <a name="understand-the-cors-way-for-ui-customization"></a>Förstå hur CORS för anpassning av Användargränssnittet

Azure AD B2C kan du anpassa utseende-och-känsla användarupplevelse (UX) på olika sidor som potentiellt kan hanteras och visas i Azure AD B2C via din anpassade principer.

För detta ändamål Azure AD B2C kör kod i din klientens webbläsare och använder metoden moderna och standard [Cross-Origin Resource Sharing (CORS)](http://www.w3.org/TR/cors/) att läsa in anpassat innehåll från en specifik URL som du anger i en anpassad princip för att peka till din HTML5/CSS-mallar. CORS är en mekanism som gör att begränsade resurser, t.ex. teckensnitt, på en webbsida begäras från en annan domän utanför domänen som resursen kommer från.

Jämfört med det gamla traditionella sätt, där mallen sidor som äger lösning där du har angett begränsad text och bilder, där begränsad kontroll över layout och känslan erbjöds leder till mer än problem med att uppnå ett smidigt sätt för CORS har stöd för HTML5 och CSS och du kan:

- Värdar för innehåll och lösningen lägger in dess kontroller med hjälp av skript på klienten.
- Ha fullständig kontroll över varje pixel layout och utseende.

Du kan ange så många innehåll sidor som du vill genom att utforma HTML5/CSS-filer vid behov.

> [!NOTE]
> Av säkerhetsskäl bör blockerat användningen av JavaScript för närvarande för anpassning. Användning av ett anpassat domännamn för din Azure AD B2C-klient krävs för att låsa upp JavaScript.

Alla HTML5/CSS-mallar kan du ange en *fästpunkt* element som motsvarar de nödvändiga `<div id=”api”>` element i HTML eller sidan innehåll som visas nedan. Azure AD B2C kräver att alla sidor har den här specifika div.

```
<!DOCTYPE html>
<html>
  <head>
    <title>Your page content’s tile!</title>
  </head>
  <body>
    <div id="api"></div>
  </body>
</html>
```

Azure AD B2C-relaterat innehåll på sidan matas in div, medan resten av sidan är din kontroll. Azure AD B2C JavaScript-kod tar emot innehållet och lägger in våra HTML till den här specifika div-element. Azure AD B2C lägger in följande kontroller efter behov: kontot Väljaren kontroll, inloggning kontroller, Multi-Factor (för närvarande telefon) kontroller och attributet samling kontroller. Azure AD B2C garanterar att alla kontrollerna är HTML5 kompatibla och komma åt alla kontroller kan fullständigt formaterad och som en kontroll version kommer inte affärsmöjlighetens.

Det sammanlagda innehållet visas slutligen som dynamiskt dokument till en förbrukare skapas.

För att säkerställa av ovan fungerar som förväntat, måste du:

- Se till att ditt innehåll är HTML5 kompatibla och tillgänglig
- Kontrollera innehållsservern har aktiverats för CORS.
- Hantera innehåll över HTTPS.
- Använda absoluta URL: er, till exempel https://yourdomain/content för alla länkar och CSS-innehåll.

> [!TIP]
> Om du vill kontrollera att du är värd för ditt innehåll på platsen har CORS är aktiverat och testa CORS begäranden, kan du använda webbplatsen http://test-cors.org/. Tack vare den här platsen kan du bara skicka CORS-begäran till en fjärrserver (för att kontrollera om det finns stöd för CORS), eller skicka CORS-begäran till en testserver (för att utforska vissa funktioner i CORS).

> [!TIP]
> Plats-http://enable-cors.org/ innebär även en mer än användbara resurser på CORS.

Tack vare den här CORS-baserade metoden måste sedan slutanvändarna konsekvent upplevelse mellan ditt program och de sidor som hanteras av Azure AD B2C.

## <a name="create-a-storage-account"></a>skapar ett lagringskonto

Du måste skapa ett lagringskonto som ett krav. Du behöver en Azure-prenumeration att skapa ett Azure Blob Storage-konto. Du kan logga in en kostnadsfri utvärderingsversion på den [Azure-webbplatsen](https://azure.microsoft.com/en-us/pricing/free-trial/).

1. Öppna en webbläsarsession och navigera till den [Azure-portalen](https://portal.azure.com).
2. Logga in med dina administratörsautentiseringsuppgifter.
3. Klicka på **ny** > **Data + lagring** > **lagringskonto**.  En **skapa lagringskonto** blad öppnas.
4. I **namn**, ange ett namn för lagringskontot, till exempel *contoso369b2c*. Det här värdet senare betecknas som *storageAccountName*.
5. Välj lämpliga val för prisnivån, resursgruppen och prenumerationen. Se till att du har den **fäst på startsidan** alternativet som är markerat. Klicka på **Skapa**.
6. Gå tillbaka till startsidan och klicka på det lagringskonto som du nyss skapade.
7. I den **Services** klickar du på **Blobbar**. En **bladet för Blob-tjänsten** öppnas.
8. Klicka på **+ behållare**.
9. I **namn**, ange ett namn för behållare, till exempel *b2c*. Det här värdet kommer senare kallas *containerName*.
9. Välj **Blob** som den **komma åt typen**. Klicka på **Skapa**.
10. Den behållare som du har skapat visas i listan på den **bladet för Blob-tjänsten**.
11. Stäng den **Blobbar** bladet.
12. På den **lagring kontoblad**, klicka på den **nyckeln** ikon. En **åtkomst nycklar bladet** öppnas.  
13. Anteckna värdet för **key1**. Det här värdet kommer senare att anges som *key1*.

## <a name="downloading-the-helper-tool"></a>Hämta helper-verktyget

1.  Hämta helper tool från [GitHub](https://github.com/azureadquickstarts/b2c-azureblobstorage-client/archive/master.zip).
2.  Spara den *B2C-AzureBlobStorage-klient-master.zip* filen på den lokala datorn.
3.  Extrahera innehållet i filen B2C-AzureBlobStorage-klient-master.zip på den lokala hårddisken, till exempel den **UI-anpassning-Pack** mapp. Detta skapar en *B2C-AzureBlobStorage-Client-master* mapp under.
4.  Öppna mappen och extrahera innehållet i arkivfilen *B2CAzureStorageClient.zip* i den.

## <a name="upload-the-ui-customization-pack-sample-files"></a>Överför exempelfiler UI-anpassning-Pack

1.  Utforskaren och navigera till mappen *B2C-AzureBlobStorage-Client-master* finns under den *UI-anpassning-Pack* mappen skapade i föregående avsnitt.
2.  Kör den *B2CAzureStorageClient.exe* fil. Det här programmet kommer bara ladda upp alla filer i katalogen som du anger till ditt lagringskonto och aktiverar CORS-åtkomst för dessa filer.
3.  När du uppmanas, ange: en.  Namnet på ditt lagringskonto *storageAccountName*, till exempel *contoso369b2c*.
    b.  Den primära åtkomstnyckeln för din azure blobblagring *key1*, till exempel *contoso369b2c*.
    c.  Namnet på din lagring blobblagringsbehållare *containerName*, till exempel *b2c*.
    d.  Sökvägen till den *startpaket* exempelfiler, till exempel *... \B2CTemplates\wingtiptoys*.

Om du följde stegen ovan HTML5 och CSS-filer för den *UI-anpassning-Pack* för det fiktiva företaget **VingspetsLeksaker** kommer nu att peka till ditt lagringskonto.  Du kan kontrollera att innehållet har överförts korrekt genom att öppna bladet relaterade behållare i Azure-portalen. Du kan också kontrollera att innehållet har överförts korrekt genom att öppna sidan i en webbläsare. Mer information finns i [Azure Active Directory B2C: ett helper-verktyg som används för att visa användaren användargränssnittet (UI) anpassning funktionssidan](active-directory-b2c-reference-ui-customization-helper-tool.md).

## <a name="ensure-the-storage-account-has-cors-enabled"></a>Se till att lagringskontot har CORS aktiverat

CORS (Cross-Origin Resource Sharing) måste vara aktiverat på slutpunkten för Azure AD B2C Premium för att läsa in ditt innehåll. Det beror på att innehållet finns på en annan domän än domänen Azure AD B2C Premium kommer betjänar sidan från.

Om du vill kontrollera att den lagring som du är värd för ditt innehåll i har CORS är aktiverat, fortsätter du med följande steg:

1. Öppna en webbläsarsession och gå till sidan *unified.html* med dess plats i ditt lagringskonto, fullständig URL `https://<storageAccountName>.blob.core.windows.net/<containerName>/unified.html`. Till exempel https://contoso369b2c.blob.core.windows.net/b2c/unified.html.
2. Gå till http://test-cors.org. Den här platsen kan du kontrollera att den sida som du använder har CORS är aktiverat.  
<!--
![test-cors.org](../../media/active-directory-b2c-customize-ui-of-a-user-journey/test-cors.png)
-->

3. I **fjärr-URL**, ange en fullständig Webbadress för unified.html-innehåll och på **skicka begäran**.
4. Kontrollera att resultatet i den **resultat** avsnittet innehåller *XHR status: 200*. Detta anger att CORS är aktiverat.
<!--
![CORS enabled](../../media/active-directory-b2c-customize-ui-of-a-user-journey/cors-enabled.png)
-->
Lagringskontot ska nu innehålla en blobbbehållare med namnet *b2c* i vår bild som innehåller följande VingspetsLeksaker mallar från den *startpaket*.

<!--
![Correctly configured storage account](../../articles/active-directory-b2c/media/active-directory-b2c-reference-customize-ui-custom/storage-account-final.png)
-->

I följande tabell beskrivs syftet med ovan HTML5-sidor.

| HTML5-mall | Beskrivning |
|----------------|-------------|
| *phonefactor.HTML* | Den här sidan kan användas som en mall för en multifaktorautentiseringssidan. |
| *ResetPassword.HTML* | Den här sidan kan användas som en mall för en glömt lösenord. |
| *selfasserted.HTML* | Den här sidan kan användas som en mall för en sociala konto registreringssidan, registreringssidan för lokalt konto eller ett lokalt konto-inloggningssida. |
| *Unified.HTML* | Den här sidan kan användas som en mall för ett enhetlig registrering eller inloggning. |
| *updateprofile.HTML* | Den här sidan kan användas som en mall för en uppdatering profilsida. |

## <a name="add-a-link-to-your-html5css-templates-to-your-user-journey"></a>Lägga till en länk i HTML5/CSS-mallar för att dina användare resa

Du kan lägga till en länk till HTML5/CSS-mallar resa dina användare genom att redigera en anpassad princip direkt.

Anpassade HTML5/CSS-mallar för användning i din användare resa måste anges i en lista över definitioner av innehåll som kan användas i dessa användare resor. För detta ändamål, en valfri  *<ContentDefinitions>*  XML-elementet måste deklareras den  *<BuildingBlocks>*  avsnitt i anpassade princip-XML-fil.

Följande tabell beskriver uppsättningen innehållsdefinitionen-ID: n som identifieras av Azure AD B2C identitet experience-motorn och vilken typ av sidor som gäller för dem.

| Innehålls-ID: n | Beskrivning |
|-----------------------|-------------|
| *API.Error* | **Felsidan**. Den här sidan visas när ett undantagsfel eller ett fel har påträffats. |
| *API.idpselections* | **Identity-providern på sidan**. Den här sidan innehåller en lista över identitetsleverantörer som användaren kan välja från under inloggningen. Är detta enterprise identitetsleverantörer, sociala identitetsleverantörer, till exempel Facebook och Google + eller lokala konton (baserat på e-adress eller användare). |
| *API.idpselections.Signup* | **Identitet providern val för registrering**. Den här sidan innehåller en lista över identitetsleverantörer som användaren kan välja bland under registreringen. Är detta enterprise identitetsleverantörer, sociala identitetsleverantörer, till exempel Facebook och Google + eller lokala konton (baserat på e-adress eller användare). |
| *API.localaccountpasswordreset* | **Har du glömt lösenordssidan**. Den här sidan innehåller ett formulär med användaren att fylla för att initiera sina återställning av lösenord.  |
| *API.localaccountsignin* | **Lokalt konto inloggningssidan**. Den här sidan innehåller en inloggning formulär med användaren att fylla i när du loggar in med ett lokalt konto som baseras på en e-postadress eller ett användarnamn. Formuläret kan innehålla en textruta och inmatningsfält för lösenord. |
| *API.localaccountsignup* | **Lokalt konto registreringssidan**. Den här sidan innehåller en registreringsformuläret som användaren har för att fylla i när du registrerar dig för ett lokalt konto som baseras på en e-postadress eller ett användarnamn. Formuläret kan innehålla olika inkommande kontroller, till exempel textrutan inmatningsfält för lösenord, knappen, enkelval listrutorna och välja flera kryssrutor. |
| *API.phonefactor* | **Multifaktorautentiseringssidan**. Användare kan verifiera sina telefonnummer (med text eller röst) under registrering eller inloggning på den här sidan. |
| *API.selfasserted* | **Sociala konto registreringssidan**. Den här sidan innehåller en registreringsformuläret som användaren har för att fylla i när du loggar in med ett befintligt konto från en sociala identitetsleverantören, till exempel Facebook eller Google +. Den här sidan liknar ovan social konto registreringssidan med undantag för lösenordet fält. |
| *API.selfasserted.profileupdate* | **Uppdatera profilsida**. Den här sidan innehåller ett formulär som användaren kan använda för att uppdatera sin profil. Den här sidan liknar ovan social konto registreringssidan med undantag för lösenordet fält. |
| *API.signuporsignin* | **Enhetlig registrering eller inloggning sidan**.  Den här sidan hanterar både registrering och inloggning av användare som kan använda enterprise identitetsleverantörer, sociala identitetsleverantörer, till exempel Facebook eller Google + eller lokala konton.

## <a name="next-steps"></a>Nästa steg
[Referens: Förstå hur anpassade principer arbeta med Identity upplevelse ramverk i B2C](active-directory-b2c-reference-custom-policies-understanding-contents.md)
