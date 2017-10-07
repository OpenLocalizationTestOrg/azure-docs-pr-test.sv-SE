---
title: "Azure Active Directory B2C: Referera: anpassa hello Användargränssnittet för en användare resa med anpassade principer | Microsoft Docs"
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
ms.openlocfilehash: 11f2a7575b95a186399d83266850fe44d650371b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="customize-hello-ui-of-a-user-journey-with-custom-policies"></a>Anpassa hello Användargränssnittet för en användare resa med anpassade principer

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

> [!NOTE]
> Den här artikeln är en avancerad beskrivning av hur anpassningar fungerar och hur tooenable med B2C anpassade principer, med hjälp av hello identitet upplevelse Framework


En smidig användarupplevelse är nyckel för alla business-to-consumer-lösningar. Med sömlös användarupplevelse menar vi en upplevelse på enheten eller webbläsaren, där en användare resa via vår tjänst inte kan särskiljas från som hello kundservice som de använder.

## <a name="understand-hello-cors-way-for-ui-customization"></a>Förstå hello CORS sätt för anpassning av Användargränssnittet

Azure AD B2C kan du toocustomize hello utseende, känsla användarupplevelse (UX) på hello olika sidor som potentiellt kan hanteras och visas i Azure AD B2C via din anpassade principer.

För detta ändamål Azure AD B2C kör kod i din klientens webbläsare och använder hello moderna och standard metoden [Cross-Origin Resource Sharing (CORS)](http://www.w3.org/TR/cors/) tooload anpassat innehåll från en specifik URL som du anger i en anpassad princip toopoint tooyour HTML5/CSS-mallar. CORS är en mekanism som gör att begränsade resurser, t.ex. teckensnitt, på en webbsida toobe som begärts från en annan domän utanför hello-domän som hello resursen kommer från.

Jämfört med toohello gamla traditionella sätt, där mallen sidor som äger hello lösning där du har angett begränsad text och bilder, begränsad kontroll över layout och känslan erbjöds där inledande toomore än svårigheter tooachieve smidigt, hello CORS sätt stöder HTML5 och CSS och du kan:

- Värd för hello innehållet och hello lösning lägger in dess kontroller med hjälp av skript på klienten.
- Ha fullständig kontroll över varje pixel layout och utseende.

Du kan ange så många innehåll sidor som du vill genom att utforma HTML5/CSS-filer vid behov.

> [!NOTE]
> Av säkerhetsskäl är hello användning av JavaScript blockerad för anpassning. toounblock JavaScript, användning av ett anpassat domännamn för din Azure AD B2C-klient krävs.

Alla HTML5/CSS-mallar kan du ange en *fästpunkt* element som motsvarar toohello krävs `<div id=”api”>` element i hello HTML eller hello innehållssida som visas nedan. Azure AD B2C kräver att alla sidor har den här specifika div.

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

Azure AD B2C-relaterat innehåll för hello sidan matas i den här div när hello resten av hello sida är ditt toocontrol. hello Azure AD B2C JavaScript-kod tar emot innehållet och lägger in våra HTML till den här specifika div-element. Azure AD B2C lägger in hello följande kontroller efter behov: kontot Väljaren kontroll, inloggning kontroller, Multi-Factor (för närvarande telefon) kontroller och attributet samling kontroller. Azure AD B2C säkerställer att alla hello kontroller är HTML5 kompatibla och komma åt alla hello kontroller kan fullständigt formaterad och att en kontroll version inte kommer affärsmöjlighetens.

hello visas kopplade innehåll slutligen som hello dynamiskt dokument tooyour konsumenten.

tooensure av hello ovan fungerar som förväntat, måste du:

- Se till att ditt innehåll är HTML5 kompatibla och tillgänglig
- Kontrollera innehållsservern har aktiverats för CORS.
- Hantera innehåll över HTTPS.
- Använda absoluta URL: er, till exempel https://yourdomain/content för alla länkar och CSS-innehåll.

> [!TIP]
> tooverify som hello plats som du är värd för ditt innehåll i har CORS är aktiverat och testa CORS begäranden, kan du använda hello plats http://test-cors.org/. Tack toothis plats kan du kan bara antingen skicka hello CORS begäran tooa fjärrserver (tootest om det finns stöd för CORS), eller skicka hello CORS begäran tooa testserver (tooexplore vissa funktioner i CORS).

> [!TIP]
> hello plats http://enable-cors.org/ innebär även en mer än användbara resurser på CORS.

Tack vare toothis CORS-baserade metoden hello slutanvändare måste sedan konsekvent upplevelse mellan programmet och hello sidor som hanteras av Azure AD B2C.

## <a name="create-a-storage-account"></a>skapar ett lagringskonto

En förutsättning är måste toocreate ett lagringskonto. Du behöver ett Azure-prenumeration toocreate ett Azure Blob Storage-konto. Du kan logga in en kostnadsfri utvärderingsversion på hello [Azure-webbplatsen](https://azure.microsoft.com/en-us/pricing/free-trial/).

1. Öppna en webbläsarsession och navigera toohello [Azure-portalen](https://portal.azure.com).
2. Logga in med dina administratörsautentiseringsuppgifter.
3. Klicka på **ny** > **Data + lagring** > **lagringskonto**.  En **skapa lagringskonto** blad öppnas.
4. I **namn**, ange ett namn för hello storage-konto, till exempel *contoso369b2c*. Det här värdet kommer senare att anges som för*storageAccountName*.
5. Välj hello lämpliga val för hello prisnivå, hello resursgruppen och hello prenumeration. Se till att du har hello **PIN-kod tooStartboard** alternativet som är markerat. Klicka på **Skapa**.
6. Gå tillbaka toohello startsidan och klicka på hello storage-konto som du nyss skapade.
7. I hello **Services** klickar du på **Blobbar**. En **bladet för Blob-tjänsten** öppnas.
8. Klicka på **+ behållare**.
9. I **namn**, ange ett namn för hello behållare, till exempel *b2c*. Det här värdet kommer senare att hänvisade tooas *containerName*.
9. Välj **Blob** som hello **komma åt typen**. Klicka på **Skapa**.
10. hello-behållare som du har skapat visas i hello listan på hello **bladet för Blob-tjänsten**.
11. Stäng hello **Blobbar** bladet.
12. På hello **lagring kontoblad**, klicka på hello **nyckeln** ikon. En **åtkomst nycklar bladet** öppnas.  
13. Skriv ned hello värdet för **key1**. Det här värdet kommer senare att anges som *key1*.

## <a name="downloading-hello-helper-tool"></a>Hämta hello helper tool

1.  Hämta hello helper tool från [GitHub](https://github.com/azureadquickstarts/b2c-azureblobstorage-client/archive/master.zip).
2.  Spara hello *B2C-AzureBlobStorage-klient-master.zip* filen på den lokala datorn.
3.  Extrahera hello innehållet hello B2C-AzureBlobStorage-klient-master.zip filen på den lokala hårddisken, till exempel under hello **UI-anpassning-Pack** mapp. Detta skapar en *B2C-AzureBlobStorage-Client-master* mapp under.
4.  Öppna mappen och extrahera hello innehållet i hello arkivfilen *B2CAzureStorageClient.zip* i den.

## <a name="upload-hello-ui-customization-pack-sample-files"></a>Överför hello UI-anpassning-Pack exempelfiler

1.  Utforskaren och navigera toohello mappen *B2C-AzureBlobStorage-Client-master* finns under hello *UI-anpassning-Pack* mappen hello föregående avsnitt.
2.  Kör hello *B2CAzureStorageClient.exe* fil. Det här programmet kommer bara att överföra alla hello-filer i hello katalog du anger tooyour storage-konto och aktiverar CORS-åtkomst för dessa filer.
3.  När du uppmanas, ange: en.  hello namnet på ditt lagringskonto *storageAccountName*, till exempel *contoso369b2c*.
    b.  hello primärnyckeln för din azure blobblagring *key1*, till exempel *contoso369b2c*.
    c.  hello namnet på din lagring blobblagringsbehållare *containerName*, till exempel *b2c*.
    d.  hello sökvägen hello *startpaket* exempelfiler, till exempel *... \B2CTemplates\wingtiptoys*.

Om du har följt hello stegen ovan hello HTML5 och CSS-filer med hello *UI-anpassning-Pack* för hello fiktiva företaget **VingspetsLeksaker** kommer nu att peka tooyour storage-konto.  Du kan kontrollera att hello innehåll har överförts korrekt genom att öppna hello relaterade behållaren bladet i hello Azure-portalen. Du kan också kontrollera att hello innehåll har överförts korrekt genom att öppna sidan hello från en webbläsare. Mer information finns i [Azure Active Directory B2C: ett helper-verktyg som används toodemonstrate hello sidan användargränssnittet (UI) anpassning användarfunktionen](active-directory-b2c-reference-ui-customization-helper-tool.md).

## <a name="ensure-hello-storage-account-has-cors-enabled"></a>Kontrollera hello storage-konto har CORS aktiverat

CORS (Cross-Origin Resource Sharing) måste vara aktiverat på slutpunkten för Azure AD B2C Premium tooload ditt innehåll. Det beror på att innehållet finns på en annan domän än hello domän Azure AD B2C Premium kommer betjänar hello sida från.

tooverify hello lagring som du är värd för ditt innehåll i med CORS aktiverat kan fortsätta med hello följande steg:

1. Öppna en webbläsarsession och navigera toohello sidan *unified.html* med hello fullständiga URL: en av dess plats i ditt lagringskonto `https://<storageAccountName>.blob.core.windows.net/<containerName>/unified.html`. Till exempel https://contoso369b2c.blob.core.windows.net/b2c/unified.html.
2. Navigera toohttp://test-cors.org. Den här webbplatsen kan du tooverify som hello sida som du använder har CORS är aktiverat.  
<!--
![test-cors.org](../../media/active-directory-b2c-customize-ui-of-a-user-journey/test-cors.png)
-->

3. I **fjärr-URL**, ange hello fullständig URL för ditt unified.html innehåll och på **skicka begäran**.
4. Kontrollera att hello utdata i hello **resultat** avsnittet innehåller *XHR status: 200*. Detta anger att CORS är aktiverat.
<!--
![CORS enabled](../../media/active-directory-b2c-customize-ui-of-a-user-journey/cors-enabled.png)
-->
hello storage-konto nu ska innehålla en blobbbehållare med namnet *b2c* hello i vår bild som innehåller följande VingspetsLeksaker mallar från hello *startpaket*.

<!--
![Correctly configured storage account](../../articles/active-directory-b2c/media/active-directory-b2c-reference-customize-ui-custom/storage-account-final.png)
-->

hello beskrivs följande tabell hello syftet hello ovan HTML5-sidor.

| HTML5-mall | Beskrivning |
|----------------|-------------|
| *phonefactor.HTML* | Den här sidan kan användas som en mall för en multifaktorautentiseringssidan. |
| *ResetPassword.HTML* | Den här sidan kan användas som en mall för en glömt lösenord. |
| *selfasserted.HTML* | Den här sidan kan användas som en mall för en sociala konto registreringssidan, registreringssidan för lokalt konto eller ett lokalt konto-inloggningssida. |
| *Unified.HTML* | Den här sidan kan användas som en mall för ett enhetlig registrering eller inloggning. |
| *updateprofile.HTML* | Den här sidan kan användas som en mall för en uppdatering profilsida. |

## <a name="add-a-link-tooyour-html5css-templates-tooyour-user-journey"></a>Lägg till en länk tooyour HTML5/CSS mallar tooyour användaren resa

Du kan lägga till en länk tooyour HTML5/CSS mallar tooyour användaren resa genom att redigera en anpassad princip direkt.

hello anpassade HTML5/CSS mallar toouse i användar-transporten har toobe som anges i en lista över definitioner av innehåll som kan användas i dessa användare resor. För detta ändamål, en valfri  *<ContentDefinitions>*  XML-elementet måste deklareras under hello  *<BuildingBlocks>*  avsnitt i anpassade princip-XML-fil.

hello beskrivs följande tabell hello uppsättning innehållsdefinitionen-ID: n som identifieras av hello Azure AD B2C identitet upplevelse och hello skriver över sidor som rör toothem.

| Innehålls-ID: n | Beskrivning |
|-----------------------|-------------|
| *API.Error* | **Felsidan**. Den här sidan visas när ett undantagsfel eller ett fel har påträffats. |
| *API.idpselections* | **Identity-providern på sidan**. Den här sidan innehåller en lista över providers som hello användaren kan välja bland under inloggning identitet. Är detta enterprise identitetsleverantörer, sociala identitetsleverantörer, till exempel Facebook och Google + eller lokala konton (baserat på e-adress eller användare). |
| *API.idpselections.Signup* | **Identitet providern val för registrering**. Den här sidan innehåller en lista över providers som hello användaren kan välja bland under registreringen identitet. Är detta enterprise identitetsleverantörer, sociala identitetsleverantörer, till exempel Facebook och Google + eller lokala konton (baserat på e-adress eller användare). |
| *API.localaccountpasswordreset* | **Har du glömt lösenordssidan**. Den här sidan innehåller ett formulär hello användaren har toofill tooinitiate återställa sina lösenord.  |
| *API.localaccountsignin* | **Lokalt konto inloggningssidan**. Den här sidan innehåller en inloggning formuläret hello användaren har toofill när du loggar in med ett lokalt konto som baseras på en e-postadress eller ett användarnamn. hello formulär kan innehålla en inkommande textruta och inmatningsfält för lösenord. |
| *API.localaccountsignup* | **Lokalt konto registreringssidan**. Den här sidan innehåller en registreringsformuläret hello användaren har toofill när du registrerar dig för ett lokalt konto som baseras på en e-postadress eller ett användarnamn. hello formulär kan innehålla olika inkommande kontroller, till exempel textrutan inmatningsfält för lösenord, knappen, enkelval listrutorna och välja flera kryssrutor. |
| *API.phonefactor* | **Multifaktorautentiseringssidan**. Användare kan verifiera sina telefonnummer (med text eller röst) under registrering eller inloggning på den här sidan. |
| *API.selfasserted* | **Sociala konto registreringssidan**. Den här sidan innehåller en registreringsformuläret hello användaren har toofill i när logga med ett befintligt konto från en sociala identitetsleverantör, till exempel Facebook eller Google +. Den här sidan är liknande toohello ovan sociala konto registreringssidan med hello undantag av hello lösenordsfält. |
| *API.selfasserted.profileupdate* | **Uppdatera profilsida**. Den här sidan innehåller ett formulär hello användaren kan använda tooupdate sin profil. Den här sidan är liknande toohello ovan sociala konto registreringssidan med hello undantag av hello lösenordsfält. |
| *API.signuporsignin* | **Enhetlig registrering eller inloggning sidan**.  Den här sidan hanterar både registrering och inloggning av användare som kan använda enterprise identitetsleverantörer, sociala identitetsleverantörer, till exempel Facebook eller Google + eller lokala konton.

## <a name="next-steps"></a>Nästa steg
[Referens: Förstå hur anpassade principer fungerar med hello identitet upplevelse ramverk i B2C](active-directory-b2c-reference-custom-policies-understanding-contents.md)
