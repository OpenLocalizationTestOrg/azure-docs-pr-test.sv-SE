---
title: "Azure Active Directory B2C: Med språket anpassning | Microsoft Docs"
description: 
services: active-directory-b2c
documentationcenter: 
author: sama
ms.assetid: eec4d418-453f-4755-8b30-5ed997841b56
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 04/25/2017
ms.author: sama
ms.openlocfilehash: a8e4037014f5adf929dac7b5dc4db72ba0f5b292
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-using-language-customization"></a>Azure Active Directory B2C: Med språket anpassning

>[!NOTE] 
>Den här funktionen är tillgänglig som förhandsversion.  Det rekommenderas att du använder en Testklient när du använder den här funktionen.  Vi planerar inte på ändringar bryta från hello förhandsversionen toohello allmän tillgänglighet, men vi reservera hello rätt toomake sådana ändringar tooimprove hello-funktionen.  Ge feedback om din upplevelse och hur vi kan göra den bättre när du har en chans tootry funktion.  Du kan ge feedback via hello Azure-portalen hello ansikte ansikte verktyget på hello uppe till höger.   Om det finns en affärsbehoven för du toogo live med den här funktionen under hello preview fasen, berätta för oss dina scenarier och vi kan tillhandahålla hello rätt information och hjälp.  Du kan kontakta oss på [ aadb2cpreview@microsoft.com ](mailto:aadb2cpreview@microsoft.com).

Anpassning av språk kan du toochange användaren transport tooa olika språk toosuit måste kunden.  Vi tillhandahåller översättningar för 36 språk (se [ytterligare information](#additional-information)).  Även om din upplevelse finns bara för ett språk, kan du anpassa text på hello sidor toosuit dina behov.  

## <a name="how-does-language-customization-work"></a>Hur fungerar språk anpassning?
Anpassning av språk kan du tooselect vilka språk användaren-transporten finns i.  När hello-funktionen har aktiverats måste ange du hello frågesträngparametern, ui_locales, från ditt program.  När du anropar i Azure AD B2C översätta vi sidan toohello nationella inställningar som du har angett.  Med hjälp av typen av konfiguration ger fullständig kontroll över hello språk i dina användare resa och ignorerar hello språkinställningarna hello kundens webbläsare. Alternativt kanske du inte behöver nivån kontroll över vilka språk som kunden finns.  Om du inte anger en parameter för ui_locales styrs hello kundupplevelsen av sina webbläsarinställningar.  Du kan styra vilka språk som användaren-transporten är översatt tooby lägger till den som ett språk som stöds.  Om en kund webbläsaren set tooshow ett språk som du inte vill toosupport sedan hello språk du valt som standard i stöds kulturer visas i stället.

1. **UI-språk angett språk** -när du har aktiverat språket anpassning användaren-transporten är översatt toohello språk som anges här
2. **Webbläsaren begärda språket** -om ingen ui-språk angavs det för att översätta toohello webbläsare begärda språk, **om den ingick i språk som stöds**
3. **Principen standardspråk** -om hello webbläsare inte ange ett språk eller anger en som inte stöds, den översätter toohello standardspråk för principen

>[!NOTE]
>Om du använder anpassade användarattribut måste tooprovide egna översättningar. Se ”[anpassa din strängar](#customize-your-strings)” mer information.
>

## <a name="support-uilocales-requested-languages"></a>Stöd för ui_locales begärda språk 
Du kan nu styra hello språket för hello användaren resa genom att lägga till hello ui_locales parameter genom att aktivera språk anpassning på en princip.
1. [Följ dessa steg toonavigate toohello B2C-funktionsbladet på hello Azure-portalen.](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-app-registration#navigate-to-b2c-settings)
2. Navigera tooa princip som du vill tooenable för översättningar.
3. Klicka på **språk anpassning**.
4. Läsa hello varning noggrant.  När du har aktiverat, kan du inaktivera 'Språk anpassning'.
5. Aktivera hello funktion och klicka på **OK**.
6. Spara din princip på hello övre vänstra hörnet i din **Redigera princip** bladet.

## <a name="select-which-languages-your-user-journey-supports"></a>Välj vilka språk som har stöd för dina användare resa 
Skapa en lista över tillåtna språk för dina användare resa toobe översättas i när hello ui_locales parameter inte har angetts.
1. Kontrollera din princip har 'Språk anpassning' har aktiverat från föregående anvisningarna.
2. Från din **Redigera princip** bladet väljer **språk anpassning**.
3. Du har vidtagit tooyour **språk som stöds** bladet.  Här kan du välja **lägga till språk**.
4. Välj alla hello-språk som du vill använda toobe som stöds.  

>[!NOTE]
>Om en ui_locales-parameter inte har angetts, är hello sida översatt toohello kundens webbläsarens språk om det finns i listan
>

5. Klicka på **Ok** längst ned hello
6. Stäng hello **språk anpassning** bladet och **spara** din princip.

## <a name="customize-your-strings"></a>Anpassa din strängar
'Språk anpassning, kan du toocustomize någon sträng i användar-transporten.
1. Kontrollera din princip har 'Språk anpassning' har aktiverat från hello föregående anvisningarna.
2. Från din **Redigera princip** bladet väljer **språk anpassning**.
3. Välj hello vänstra navigeringsmenyn **hämtningar**.
4. Välj hello sida som du vill tooedit.
5. Välj hello språk som du vill använda tooedit för i listrutan hello.
6. Klicka på **Hämta**. 

Här får du en JSON-fil som du kan använda toostart redigera din strängar.

### <a name="changing-any-string-on-hello-page"></a>Ändra valfri sträng på hello sida
1. Öppna hello JSON-filen laddas ned från föregående anvisningarna i en JSON-redigerare.
2. Hitta hello element som du vill använda toochange.  Du kan hitta hello `StringId` strängens hello du letar efter eller leta efter hello `Value` du vill toochange.
3. Uppdatera hello `Value` attribut med vad som ska visas.
4. Spara hello-filen och överföra dina ändringar.

### <a name="changing-extension-attributes"></a>Ändra tilläggsattribut
Om du söker toochange hello sträng för en anpassad användarattribut eller vill tooadd en toohello JSON, är det i hello följande format:
```JSON
{
  "LocalizedStrings": [
    {
      "ElementType": "ClaimType",
      "ElementId": "extension_<ExtensionAttribute>",
      "StringId": "DisplayName",
      "Override": false,
      "Value": "<ExtensionAttributeValue>"
    }
    [...]
}
```
>[!IMPORTANT]
>Om du behöver toooverride en sträng, se till att tooset hello `Override` värdet för`true`.  Hello posten ignoreras om hello värdet inte ändras. 
>

Ersätt `<ExtensionAttribute>` med hello namnet på din anpassade attribut.  
Ersätt `<ExtensionAttributeValue>` med hello nya sträng toobe visas.

### <a name="using-localizedcollections"></a>Med hjälp av LocalizedCollections
Om du vill tooprovide en fast lista med värden för svar, behöver du toocreate en `LocalizedCollections`.  En `LocalizedCollections` är en matris med `Name` och `Value` par.  Hej `Name` är vad som visas och hello `Value` är det som returneras i hello anspråket.  tooadd en `LocalizedCollections`, den har hello följande format:

```JSON
{
  "LocalizedStrings": [...],
  "LocalizedCollections": [{
      "ElementType":"ClaimType", 
      "ElementId":"<UserAttribute>",
      "TargetCollection":"Restriction",
      "Override": false,
      "Items":[
           {
                "Name":"<Response1>",
                "Value":"<Value1>"
           },
           {
                "Name":"<Response2>",
                "Value":"<Value2>"
           }
     ]
  }]
}
```
>[!IMPORTANT]
>Om du behöver toooverride en sträng, se till att tooset hello `Override` värdet för`true`.  Hello posten ignoreras om hello värdet inte ändras. 
>

* `ElementId`är hello användaren attribut som den här `LocalizedCollections` är ett svar på
* `Name`hello användare värde visas toohello
* `Value`är det som returneras i hello anspråket när det här alternativet är markerat

### <a name="upload-your-changes"></a>Ladda upp dina ändringar
1. När du har slutfört hello ändringar tooyour JSON-fil, gå tillbaka tooyour B2C-klient.
2. Från din **Redigera princip** bladet väljer **språk anpassning**.
3. Välj hello vänstra navigeringsmenyn **överföra innehåll**.
4. Välj önskade tooupload ändringarna för hello-sidan.
5. Om du vill tooupload ett språk som du tidigare har angett en JSON för måste toodelete hello föregående post.  Du kan ta bort den genom att klicka på hello `...` hello höger för respektive språk och välj **ta bort**.
6. Klicka på **Lägg till** i hello övre vänstra hörnet.
7. Välj hello språket i JSON-filen.
8. Klicka hello mapp i hello högra och Bläddra för JSON-filen.
9. Klicka på hello **överför** hello längst ned på bladet hello-knappen.
10. Gå tillbaka tooyour **Redigera princip** bladet och klicka på **spara**.

## <a name="additional-information"></a>Ytterligare information
### <a name="recommendations-for-language-customization"></a>Rekommendationer för 'Språk anpassning'
Vi rekommenderar att du endast lägga in i poster tooyour språkresurser för strängar du uttryckligen vill tooreplace.  Vi framtvinga en storlek gränsen toohello-fil som är kompilerad utanför alla JSON-översättningar.  Om filerna blir för stor, påverkar hello prestanda för dina användare resa.
### <a name="page-ui-customization-labels-are-removed-once-language-customization-is-enabled"></a>Page UI anpassning etiketter tas bort när 'Språk anpassning' har aktiverats
När du aktiverar 'Språk anpassning' bort tidigare ändringar för etiketter med Page UI anpassning förutom anpassade attribut.  Den här ändringen görs tooavoid konflikter i där du kan redigera din strängar.  Du kan fortsätta toochange etiketterna och andra strängar genom att överföra språkresurser i 'Språk anpassning'.
### <a name="microsoft-is-committed-tooprovide-hello-most-up-to-date-translations-for-your-use"></a>Microsoft är allokerat tooprovide hello senaste översättningar för din användning
Vi kontinuerligt förbättra översättningar och lagra dem på efterlevnad för dig.  Kommer att identifiera buggar och ändringar i globala terminologi och göra hello uppdateringar som kommer att fungera sömlöst i användar-transporten.
### <a name="support-for-right-to-left-languages"></a>Stöd för höger-till-vänster-språk
Det finns inget stöd för höger-till-vänster-språk, om du behöver den här funktionen Ta rösta för den här funktionen på [Azure Feedback](https://feedback.azure.com/forums/169401-azure-active-directory/suggestions/19393000-provide-language-support-for-right-to-left-languag).
### <a name="social-identity-provider-translations"></a>Sociala identitet providern översättningar
Vi ger hello ui_locales OIDC parametern toosocial inloggningar men de hanteras inte av vissa sociala identitetsleverantörer, inklusive Facebook och Google. 
### <a name="browser-behavior"></a>Webbläsarens beteende
Chrome och Firefox både begäran för sina Ange språk och om det är ett språk som stöds, visas innan hello standard.  Gräns för närvarande begär inte ett språk och går raka toohello standardspråk.
### <a name="known-issues"></a>Kända problem
* Överför språkresurser för hello MFA sida i en profil Redigera princip är inte tillgänglig.
* `LocalizedCollections`inte genereras för värden när det krävs av hello svarstyp
### <a name="what-if-i-want-a-language-that-isnt-supported"></a>Vad händer om jag vill ha ett språk som inte stöds?
Vi planerar tooprovide en utökning av den här funktionen som gör att du tooupload en JSON-resurs mot 'anpassade språk'.  hello-funktionen kan du toospecify hello namn och språk för alla språk och ange *alla* hello översättningar för det språket.  Om du behöver den här funktionen kan skicka oss din situation på [ aadb2cpreview@microsoft.com ](mailto:aadb2cpreview@microsoft.com).  

### <a name="what-languages-are-supported"></a>Vilka språk som stöds?

| Språk              | Språkkod |
|-----------------------|---------------|
| Bangla                | bn            |
| Tjeckiska                 | cs            |
| Danska                | da            |
| Tyska                | de            |
| Grekiska                 | el            |
| Svenska               | en            |
| Spanska               | es            |
| Finska               | fi            |
| Franska                | fr            |
| Gujarati              | Gu            |
| Hindi                 | Hej            |
| Kroatiska              | hr            |
| Ungerska             | hu            |
| Italienska               | it            |
| Japanska              | ja            |
| Kannada               | KN            |
| Koreanska                | ko            |
| Malayalam             | ml            |
| Marathi               | MR            |
| Malajiska                 | MS            |
| Norska bokmål      | nb            |
| Nederländska                 | nl            |
| Punjabi               | pa            |
| Polska                | pl            |
| Portugisiska – Brasilien   | pt-br         |
| Portugisiska – Portugal | pt-pt         |
| Rumänska              | ro            |
| Ryska               | ru            |
| Slovakiska                | Sk            |
| Svenska               | sv            |
| Tamilska                 | ta            |
| Telugu                | te            |
| Thai                  | TH            |
| Turkiska               | TR            |
| Kinesiska – förenklad  | zh-hans       |
| Kinesiska – traditionell | zh-hant       |
