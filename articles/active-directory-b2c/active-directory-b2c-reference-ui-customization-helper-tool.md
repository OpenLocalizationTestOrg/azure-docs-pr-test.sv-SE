---
title: 'Azure Active Directory B2C: Page UI anpassning helper tool | Microsoft Docs'
description: "Ett kommandoradsverktyg används anpassning för toodemonstrate hello sidan gränssnittsfunktionen i Azure Active Directory B2C"
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: bryanla
ms.assetid: ae935d52-3520-4a94-b66e-b35bb40e7514
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2017
ms.author: swkrish
ms.openlocfilehash: 5137ac112019369b4244a925df1acb96fefb758f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-a-helper-tool-used-toodemonstrate-hello-page-user-interface-ui-customization-feature"></a>Azure Active Directory B2C: Ett helper-verktyg som används toodemonstrate hello sidan användargränssnittet (UI) anpassning användarfunktionen
Den här artikeln är en tillhörande toohello [Huvudartikel för UI-anpassning](active-directory-b2c-reference-ui-customization.md) i Azure Active Directory (AD Azure) B2C. hello följande steg beskriver hur tooexercise hello sidan gränssnittsfunktionen anpassning med hjälp av HTML- och CSS exemplen som vi tillhandahåller.

## <a name="get-an-azure-ad-b2c-tenant"></a>Skaffa en Azure AD B2C-klient
Innan du kan anpassa allt du behöver för[skaffa en Azure AD B2C-klient](active-directory-b2c-get-started.md) om du inte redan har ett.

## <a name="create-a-sign-up-or-sign-in-policy"></a>Skapa en princip för registrering eller inloggning
hello exemplen som vi tillhandahåller kan vara används toocustomze två sidor i en [princip för registrering eller inloggning](active-directory-b2c-reference-policies.md): hello [enhetlig inloggningssidan](active-directory-b2c-reference-ui-customization.md) och hello [själva uppgavs attribut sidan](active-directory-b2c-reference-ui-customization.md). När [skapar din princip för registrering eller inloggning](active-directory-b2c-reference-policies.md#create-a-sign-up-or-sign-in-policy), lägga till lokalt konto (e-postadress), Facebook, Google och Microsoft som **identitetsleverantörer**. Dessa hello IDPs som tar emot i vårt exempel HTML-innehåll.  Du kan också lägga till en delmängd av dessa IDPs om du vill.

## <a name="register-an-application"></a>Registrera ett program
Du behöver för[registrera ett program](active-directory-b2c-app-registration.md) i din B2C-klient som kan vara används tooexecute din princip. Efter registrering tillämpningsprogrammet har du några alternativ som du kan använda tooactually kör principen för registrering:

* Skapa en hello Azure AD B2C Snabbkurs program visas i hello ”Kom igång” avsnittet för [registrera och logga in användare i dina program](active-directory-b2c-overview.md#get-started).
* Använd hello fördefinierade [Azure AD B2C Playground](https://aadb2cplayground.azurewebsites.net) program. Om du väljer toouse hello playground, måste du registrera ett program i B2C-klienten med hjälp av hello **omdirigerings-URI** `https://aadb2cplayground.azurewebsites.net/`.
* Använd hello **kör nu** principen i hello-knappen [Azure-portalen](https://portal.azure.com/).

## <a name="customize-your-policy"></a>Anpassa din princip
toocustomize hello utseende och känslan av en princip, behöver du toofirst skapa HTML- och CSS-filer med hjälp av hello särskilda konventioner för Azure AD B2C. Du kan sedan överföra statiskt innehåll tooa offentligt tillgängliga platsen så att Azure AD B2C kan komma åt den. Detta kan vara dina egna dedikerade webbservern, Azure Blob Storage, Azure Content Delivery Network eller någon annan statisk värd för resursen provider. hello endast krav är att innehållet är tillgängligt via HTTPS och kan nås med hjälp av CORS. När du har exponerad din statiskt innehåll på webb-hello kan redigera du princip toopoint toothis plats och som innehåll tooyour kunder. Hej [Huvudartikel för UI-anpassning](active-directory-b2c-reference-ui-customization.md) beskrivs i detalj hur hello Azure AD B2C anpassning av funktionen fungerar.

För hello i den här kursen är vi redan skapat några exemplen och finns på Azure Blob Storage. hello exemplen är en grundläggande anpassning hello-tema för vårt fiktiva företag, ”Wingtip Toys”. tootry den i din egen policy, gör du följande:

1. Logga in tooyour klient på hello [Azure-portalen](https://portal.azure.com/) och navigera toohello B2C-funktionsbladet.
2. Klicka på **principer för registrering eller inloggning** och klicka sedan på din princip (till exempel ”b2c\_1\_logga\_in\_logga\_i”).
3. Klicka på **Page UI anpassning** och sedan **Unified registrering eller inloggning sidan**.
4. Växla hello **Använd anpassad sida** växla för**Ja**. I hello **anpassad sidan URI** anger `https://wingtiptoysb2c.blob.core.windows.net/b2c/wingtip/unified.html`. Klicka på **OK**.
5. Klicka på **registreringssidan för lokalt konto**. Växla hello **Använd anpassad mall** växla för**Ja**. I hello **anpassad sidan URI** anger `https://wingtiptoysb2c.blob.core.windows.net/b2c/wingtip/selfasserted.html`.
6. Upprepa hello samma steg för hello **sociala konto registreringssidan**.
   Klicka på **OK** två gånger tooclose hello UI anpassning blad.
7. Klicka på **Spara**.

Nu kan du testa den anpassade principen. Du kan använda ditt eget program eller hello Azure AD B2C-playground om du vill, men du kan också helt enkelt klicka hello **kör nu** i hello principbladet. Markera programmet i hello listrutan och välj hello lämpliga omdirigerings-URI. Klicka på hello **kör nu** knappen. En ny webbläsarflik öppnas och du kan köra via hello användarupplevelse registrerar dig för ditt program med hello nytt innehåll på plats!

## <a name="upload-hello-sample-content-tooazure-blob-storage"></a>Överför hello exempel innehåll tooAzure Blob Storage
Om du vill att toouse Azure Blob Storage toohost sidan innehåll, du kan skapa egna storage-konto och använda vår B2C helper tool tooupload dina filer.

### <a name="create-a-storage-account"></a>skapar ett lagringskonto
1. Logga in toohello [Azure-portalen](https://portal.azure.com/).
2. Klicka på **+ ny** > **Data + lagring** > **lagringskonto**. Du behöver ett Azure-prenumeration toocreate ett Azure Blob Storage-konto. Du kan logga in en kostnadsfri utvärderingsversion på hello [Azure-webbplatsen](https://azure.microsoft.com/pricing/free-trial/).
3. Ange en **namn** för hello storage-konto (till exempel ”contoso”) och välj hello lämpliga val för **prisnivå**, **resursgruppen** och  **Prenumerationen**. Se till att du har hello **PIN-kod tooStartboard** alternativet som är markerat. Klicka på **Skapa**.
4. Gå tillbaka toohello startsidan och klicka på hello storage-konto som du nyss skapade.
5. I hello **sammanfattning** klickar du på **behållare**, och klicka sedan på **+ Lägg till**.
6. Ange en **namn** hello behållare (till exempel ”b2c”) och välj **Blob** som hello **komma åt typen**. Klicka på **OK**.
7. hello-behållare som du har skapat visas i hello listan på hello **Blobbar** bladet. Skriv ned hello URL för hello behållaren. till exempel det bör se ut för`https://contoso.blob.core.windows.net/b2c`. Stäng hello **Blobbar** bladet.
8. På hello lagring-kontoblad klickar du på **nycklar** och Skriv ned hello värdena för hello **Lagringskontonamnet** och **primära åtkomstnyckeln** fält.

> [!NOTE]
> **Primära åtkomstnyckeln** är en viktig säkerhetsuppgift för autentisering.
> 
> 

### <a name="download-hello-helper-tool-and-sample-files"></a>Hämta hello helper tool och exempel
Du kan hämta hello [Azure Blob Storage helper-verktyget och exempel filer som en .zip-fil](https://github.com/azureadquickstarts/b2c-azureblobstorage-client/archive/master.zip) eller klona från GitHub:

```
git clone https://github.com/azureadquickstarts/b2c-azureblobstorage-client
```

Den här databasen innehåller en `sample_templates\wingtip` directory, som innehåller exempel HTML, CSS och bilder. För dessa mallar tooreference din egen Azure Blob Storage-konto behöver du tooedit hello HTML-filer. Öppna `unified.html` och `selfasserted.html` och Ersätt alla förekomster av `https://localhost` med din egen behållare som du skrev ned i föregående steg i hello hello URL. Du måste använda hello absoluta sökvägen till hello HTML-filer eftersom i det här fallet hello HTML hanteras av Azure AD under hello domän `https://login.microsoftonline.com`.

### <a name="upload-hello-sample-files"></a>Överför hello exempelfiler
I Hej samma databas, packa `B2CAzureStorageClient.zip` och kör hello `B2CAzureStorageClient.exe` filen i. Det här programmet kommer bara att överföra alla hello-filer i hello katalog du anger tooyour storage-konto och aktiverar CORS-åtkomst för dessa filer. Om du har följt hello stegen ovan kommer hello HTML och CSS-filer nu att peka tooyour storage-konto. Observera att hello namnet på ditt lagringskonto ingår hello som föregår `blob.core.windows.net`, till exempel `contoso`. Du kan verifiera att hello innehåll har överförts korrekt genom att försöka tooaccess `https://{storage-account-name}.blob.core.windows.net/{container-name}/wingtip/unified.html` i en webbläsare. Också använda [http://test-cors.org/](http://test-cors.org/) toomake till att hello innehåll är nu CORS är aktiverat. (Sök efter ”XHR status: 200” i hello resultat.)

### <a name="customize-your-policy-again"></a>Anpassa din princip igen
Nu när du har överfört hello exempel innehåll tooyour egna storage-konto, måste du redigera din registreringsprincipen tooreference den. Upprepa steg hello från hello [”anpassa principen”](#customize-your-policy) avsnittet ovan är nu använda ditt eget lagringskonto URL-adresser. Till exempel hello platsen för din `unified.html` filen skulle vara `<url-of-your-container>/wingtip/unified.html`.

Nu kan du använda hello **kör nu** knapp eller egna program tooexecute principen igen. Hej resultatet bör titta nästan exakt hello samma--som du använde hello samma exempel HTML och CSS i båda fallen. Men dina principer nu refererar till en egen instans av Azure Blob Storage och du ledigt tooedit och ladda upp hello filer igen som du. För mer information om hur du anpassar hello HTML- och CSS, se toohello [Huvudartikel för UI-anpassning](active-directory-b2c-reference-ui-customization.md).

