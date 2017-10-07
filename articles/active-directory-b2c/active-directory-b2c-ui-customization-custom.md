---
title: "Anpassa ett gränssnitt med hjälp av anpassade principer - Azure AD B2C | Microsoft Docs"
description: "Lär dig mer om hur du anpassar ett användargränssnitt (UI) samtidigt som du använder anpassade principer i Azure AD B2C."
services: active-directory-b2c
documentationcenter: 
author: saeedakhter-msft
manager: krassk
editor: parakhj
ms.assetid: 658c597e-3787-465e-b377-26aebc94e46d
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 04/04/2017
ms.author: saeedakhter-msft
ms.openlocfilehash: 6f00995e54c9f9ef27cc51e38f3de07cd5817cc1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-configure-ui-customization-in-a-custom-policy"></a>Azure Active Directory B2C: Konfigurera anpassningar i en anpassad princip

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

När du har slutfört den här artikeln har du en anpassad princip för registrering och inloggning med märke och utseende. Med Azure Active Directory B2C (Azure AD B2C), du får nästan full kontroll över hello HTML- och CSS-innehåll som har presenterat toousers. När du använder en anpassad princip kan konfigurera du anpassning av Användargränssnittet i XML i stället för att använda kontroller i hello Azure-portalen. 

## <a name="prerequisites"></a>Krav

Innan du kan slutföra [komma igång med anpassade principer](active-directory-b2c-get-started-custom.md). Du bör ha en fungerande anpassad princip för registrering och inloggning med lokala konton.

## <a name="page-ui-customization"></a>Anpassning av Page UI

Med funktionen hello sidan UI anpassning kan anpassa du hello utseende och känslan av en anpassad princip. Du kan också upprätthålla märke och visual konsekvensen mellan dina program och Azure AD B2C.

Här är hur det fungerar: Azure AD B2C Kör koden i kundens webbläsare och använder en modern metod som kallas [Cross-Origin Resource Sharing (CORS)](http://www.w3.org/TR/cors/). Först måste ange du en URL i hello anpassad princip med anpassade HTML-innehåll. Azure AD B2C sammanfogas UI-element med hello HTML-innehåll som har lästs in från URL: en och sedan visar hello sidan toohello kunden.

## <a name="create-your-html5-content"></a>Skapa din HTML5 innehåll

Skapa HTML innehåll med varumärken Produktnamn i hello rubrik.

1. Kopiera hello följande HTML-fragment. Det är korrekt strukturerad HTML5 med ett tomt element kallas  *\<div id = ”api”\>\</div\>*  i hello  *\<brödtext\>*  taggar. Det här elementet anger där Azure AD B2C innehåll är toobe infogas.

   ```html
   <!DOCTYPE html>
   <html>
   <head>
       <title>My Product Brand Name</title>
   </head>
   <body>
       <div id="api"></div>
   </body>
   </html>
   ```

   >[!NOTE]
   >Av säkerhetsskäl är hello användning av JavaScript blockerad för anpassning.

2. Klistra in hello kopieras fragment i en textredigerare och sedan spara hello som *anpassa ui.html*.

## <a name="create-an-azure-blob-storage-account"></a>Skapa ett Azure Blob storage-konto

>[!NOTE]
> I den här artikeln använder vi Azure Blob storage toohost innehållet. Du kan välja toohost ditt innehåll på en webbserver, men du måste [aktivera CORS på din webbserver](https://enable-cors.org/server.html).

toohost den här HTML-innehåll i Blob storage hello följande:

1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. På hello **hubb** väljer du **ny** > **lagring** > **lagringskonto**.
3. Ange ett unikt **namn** för ditt lagringskonto.
4. **Distributionsmodell** kan vara **Resource Manager**.
5. Ändra **konto typ** för**Blob storage**.
6. **Prestanda** kan vara **Standard**.
7. **Replikering** kan vara **RA-GRS**.
8. **Åtkomstnivå** kan vara **frekvent**.
9. **Lagringstjänstens kryptering** kan vara **inaktiverade**.
10. Välj en **prenumeration** för ditt lagringskonto.
11. Skapa en **resursgruppen** eller välj en befintlig.
12. Välj hello **geografisk plats** för ditt lagringskonto.
13. Klicka på **skapa** toocreate hello storage-konto.  
    När hello distributionen är klar hello **lagringskonto** blad öppnas automatiskt.

## <a name="create-a-container"></a>Skapa en behållare

toocreate en offentlig behållare i Blob storage hello följande:

1. Klicka på hello **översikt** fliken.
2. Klicka på **behållare**.
3. För **namn**, typen **$root**.
4. Ange **komma åt typen** för**Blob**.
5. Klicka på **$root** tooopen hello ny behållare.
6. Klicka på **Överför**.
7. Klicka på hello mappikon visas bredvid för**Välj en fil**.
8. Gå för**anpassa ui.html**, som du skapade tidigare i hello [Page UI anpassning](#the-page-ui-customization-feature) avsnitt.
9. Klicka på **Överför**.
10. Välj hello anpassa ui.html blob som du har överfört.
11. Nästa för**URL**, klickar du på **kopiera**.
12. Klistra in hello kopieras URL i en webbläsare och gå toohello plats. Om hello platsen är tillgänglig, kontrollera hello behållartypen åtkomst anges för**blob**.

## <a name="configure-cors"></a>Konfigurera CORS

Konfigurera Blob storage för Cross-Origin Resource Sharing hello följande:

>[!NOTE]
>Vill tootry ut hello gränssnittsfunktionen anpassning med hjälp av vår HTML- och CSS innehåll? Vi har samlat [ett enkelt kommandoradsverktyg](active-directory-b2c-reference-ui-customization-helper-tool.md) som överför och konfigurerar våra exemplen på Blob storage-konto. Om du använder hello-verktyget kan du gå vidare för[ändra en anpassad princip för registrering eller inloggning](#modify-your-sign-up-or-sign-in-custom-policy).

1. På hello **lagring** bladet under **inställningar**öppnar **CORS**.
2. Klicka på **Lägg till**.
3. För **tillåtna ursprung**, skriver du en asterisk (\*).
4. I hello **tillåtna verb** listrutan, Välj **hämta** och **alternativ**.
5. För **tillåtna huvuden**, skriver du en asterisk (\*).
6. För **exponeras huvuden**, skriver du en asterisk (\*).
7. För **högsta ålder (sekunder)**, typen **200**.
8. Klicka på **Lägg till**.

## <a name="test-cors"></a>Testa CORS

Verifiera att du är redo hello följande:

1. Gå toohello [test cors.org](http://test-cors.org/) webbplats och klistra in hello URL i hello **fjärr-URL** rutan.
2. Klicka på **skicka begäran**.  
    Om du får ett felmeddelande, kontrollerar du att din [CORS-inställningarna](#configure-cors) är korrekta. Du kanske också måste tooclear webbläsarens cache eller öppna ett privat sessionen genom att trycka på Ctrl + Skift + P.

## <a name="modify-your-sign-up-or-sign-in-custom-policy"></a>Ändra en anpassad princip för registrering eller inloggning

Under hello översta  *\<TrustFrameworkPolicy\>*  tagg, bör du hitta  *\<BuildingBlocks\>*  tagg. Inom hello  *\<BuildingBlocks\>*  taggar, lägga till en  *\<ContentDefinitions\>*  taggen genom att kopiera hello följande exempel. Ersätt *your_storage_account* med hello namnet på ditt lagringskonto.

  ```xml
  <BuildingBlocks>
    <ContentDefinitions>
      <ContentDefinition Id="api.idpselections">
        <LoadUri>https://{your_storage_account}.blob.core.windows.net/customize-ui.html</LoadUri>
      </ContentDefinition>
    </ContentDefinitions>
  </BuildingBlocks>
  ```

## <a name="upload-your-updated-custom-policy"></a>Ladda upp din uppdaterade anpassad princip

1. I hello [Azure-portalen](https://portal.azure.com), [växla till hello kontext för din Azure AD B2C-klient](active-directory-b2c-navigate-to-b2c-context.md), och sedan öppna hello **Azure AD B2C** bladet.
2. Klicka på **alla principer**.
3. Klicka på **överföra princip**.
4. Överför `SignUpOrSignin.xml` med hello  *\<ContentDefinitions\>*  tagg som du har lagt till tidigare.

## <a name="test-hello-custom-policy-by-using-run-now"></a>Testa hello anpassad princip med **kör nu**

1. På hello **Azure AD B2C** bladet går för**alla principer**.
2. Välj hello anpassad princip som du överfört och klicka på hello **kör nu** knappen.
3. Du ska kunna toosign upp med hjälp av en e-postadress.

## <a name="reference"></a>Referens

Du kan hitta exempelmallarna för anpassning av Användargränssnittet här:

```
git clone https://github.com/azureadquickstarts/b2c-azureblobstorage-client
```

Hej sample_templates/wingtip mappen innehåller hello följande HTML-filer:

| HTML5-mall | Beskrivning |
|----------------|-------------|
| *phonefactor.HTML* | Använd den här filen som en mall för en multifaktorautentiseringssidan. |
| *ResetPassword.HTML* | Använd den här filen som en mall för en glömt lösenord. |
| *selfasserted.HTML* | Använd den här filen som en mall för en sociala konto registreringssidan, registreringssidan för lokalt konto eller ett lokalt konto-inloggningssida. |
| *Unified.HTML* | Använd den här filen som en mall för ett enhetlig registrering eller inloggning. |
| *updateprofile.HTML* | Använd den här filen som en mall för en uppdatering profilsida. |

I hello [ändra anpassad princip för registrering eller inloggning-avsnittet](#modify-your-sign-up-or-sign-in-custom-policy), du har konfigurerat hello innehållsdefinitionen för `api.idpselections`. hello fullständig uppsättning innehållsdefinitionen ID: N som identifieras av hello Azure AD B2C identitet upplevelse framework och deras beskrivningar finns i hello följande tabell:

| Innehålls-ID: N | Beskrivning | 
|-----------------------|-------------|
| *API.Error* | **Felsidan**. Den här sidan visas när ett undantagsfel eller ett fel har påträffats. |
| *API.idpselections* | **Identity-providern på sidan**. Den här sidan innehåller en lista över providers som hello användaren kan välja bland under inloggning identitet. Dessa alternativ är enterprise identitetsleverantörer, sociala identitetsleverantörer, till exempel Facebook och Google + eller lokala konton. |
| *API.idpselections.Signup* | **Identitet providern val för registrering**. Den här sidan innehåller en lista över providers som hello användaren kan välja bland under registreringen identitet. Dessa alternativ är enterprise identitetsleverantörer, sociala identitetsleverantörer, till exempel Facebook och Google + eller lokala konton. |
| *API.localaccountpasswordreset* | **Har du glömt lösenordssidan**. Den här sidan innehåller ett formulär hello användaren måste slutföra tooinitiate en lösenordsåterställning.  |
| *API.localaccountsignin* | **Lokalt konto inloggningssidan**. Den här sidan innehåller ett formulär för att logga in med ett lokalt konto som baseras på en e-postadress eller ett användarnamn. hello formulär kan innehålla en inkommande textruta och inmatningsfält för lösenord. |
| *API.localaccountsignup* | **Lokalt konto registreringssidan**. Den här sidan innehåller en registreringsformuläret för att registrera dig för ett lokalt konto som baseras på en e-postadress eller ett användarnamn. hello formulär kan innehålla olika inkommande kontroller, till exempel en textruta, inmatningsfält för lösenord, en alternativknapp, enkelval listrutorna och välja flera kryssrutor. |
| *API.phonefactor* | **Multifaktorautentiseringssidan**. På den här sidan verifiera användare sina telefonnummer (med hjälp av text- eller röst) under registrering eller inloggning. |
| *API.selfasserted* | **Sociala konto registreringssidan**. Den här sidan innehåller en registreringsformuläret som användare måste slutföra när de loggar med ett befintligt konto från en sociala identitetsleverantören, till exempel Facebook eller Google +. Den här sidan är liknande toohello föregående sociala konto registreringssidan, förutom hello lösenordsfält. |
| *API.selfasserted.profileupdate* | **Uppdatera profilsida**. Den här sidan innehåller ett formulär som användare kan använda tooupdate sin profil. Den här sidan är liknande toohello sociala konto registreringssidan, förutom hello lösenordsfält. |
| *API.signuporsignin* | **Enhetlig registrering eller inloggning sidan**. Den här sidan hanterar både hello registrering och inloggning av användare som kan använda enterprise identitetsleverantörer, sociala identitetsleverantörer, till exempel Facebook eller Google + eller lokala konton.  |

## <a name="next-steps"></a>Nästa steg

Mer information om UI-element som kan anpassas finns [referenshandbok för anpassning av Användargränssnittet för inbyggda principer](active-directory-b2c-reference-ui-customization.md).
