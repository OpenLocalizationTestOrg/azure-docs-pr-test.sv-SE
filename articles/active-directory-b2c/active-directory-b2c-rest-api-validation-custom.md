---
title: "Azure Active Directory B2C: REST API-anspråk utbyte som verifiering | Microsoft Docs"
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
ms.date: 04/24/2017
ms.author: joroja
ms.openlocfilehash: cec6c6e110514a8bbe0e0780f36738ff21ae2f00
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="walkthrough-integrate-rest-api-claims-exchanges-in-your-azure-ad-b2c-user-journey-as-validation-on-user-input"></a>Genomgång: Integrera utbyte av REST API-anspråk i din Azure AD B2C användaren resa som autentiserades på indata från användaren

hello identitet upplevelse Framework (IEF) för Azure Active Directory B2C det (Azure AD B2C) gör det möjligt för hello identitet developer toointegrate interaktion med en RESTful-API i en resa för användaren.  

Hello slutet av den här genomgången, kommer du att kunna toocreate en Azure AD B2C användaren resa som interagerar med RESTful-tjänster.

hello IEF skickar data i anspråk och tar emot data tillbaka i anspråk. hello interaktion med hello API:

- Kan utformas som en REST API anspråk exchange eller verifieringsprofil som görs i ett orchestration-steg.
- Vanligtvis verifierar indata från användaren hello. Om hello värdet från hello användaren avvisas hello användaren kan försöka igen tooenter ett giltigt värde med hello möjlighet tooreturn ett felmeddelande.

Du kan också utforma hello interaktion som ett orchestration-steg. Mer information finns i [genomgång: integrera REST API-anspråk utbyte i din Azure AD B2C användaren resa som ett orchestration-steg](active-directory-b2c-rest-api-step-custom.md).

Vi ska använda hello profil Redigera användare resa i hello pack startfil ProfileEdit.xml exempelvis hello validering profil.

Vi kan verifiera att hello namn anges av hello användaren i hello profilen Redigera inte är en del av en undantagslista.

## <a name="prerequisites"></a>Krav

- En Azure AD B2C-klient som konfigurerats toocomplete ett lokalt konto sign-upp/inloggning, enligt beskrivningen i [komma igång](active-directory-b2c-get-started-custom.md).
- En REST API-slutpunkten toointeract med. Den här genomgången vi har konfigurerat en demo-webbplatsen som heter [WingTipGames](https://wingtipgamesb2c.azurewebsites.net/) med en REST API-tjänst.

## <a name="step-1-prepare-hello-rest-api-function"></a>Steg 1: Förbered hello REST API-funktion

> [!NOTE]
> Installationen av REST API-funktioner är utanför hello omfånget för den här artikeln. [Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-reference) ger en utmärkt toolkit toocreate RESTful-tjänster i hello molnet.

Vi har skapat en Azure-funktion som tar emot ett anspråk som den förväntas som `playerTag`. hello funktionen verifierar om detta anspråk finns. Du kan komma åt hello fullständig Azure Funktionskoden i [GitHub](https://github.com/Azure-Samples/active-directory-b2c-advanced-policies/tree/master/AzureFunctionsSamples).

```csharp
if (requestContentAsJObject.playerTag == null)
{
  return request.CreateResponse(HttpStatusCode.BadRequest);
}

var playerTag = ((string) requestContentAsJObject.playerTag).ToLower();

if (playerTag == "mcvinny" || playerTag == "msgates123" || playerTag == "revcottonmarcus")
{
  return request.CreateResponse<ResponseContent>(
    HttpStatusCode.Conflict,
    new ResponseContent
    {
      version = "1.0.0",
      status = (int) HttpStatusCode.Conflict,
      userMessage = $"hello player tag '{requestContentAsJObject.playerTag}' is already used."
    },
    new JsonMediaTypeFormatter(),
    "application/json");
}

return request.CreateResponse(HttpStatusCode.OK);
```

hello IEF förväntar hello `userMessage` anspråk som hello Azure funktionen returnerar. Det här anspråket visas som en sträng toohello användare om hello valideringen misslyckas, till exempel när status 409 konflikt returneras i föregående exempel hello.

## <a name="step-2-configure-hello-restful-api-claims-exchange-as-a-technical-profile-in-your-trustframeworkextensionsxml-file"></a>Steg 2: Konfigurera exchange för hello RESTful-API-anspråk som en teknisk profil i filen TrustFrameworkExtensions.xml

En teknisk profil är hello fullständig konfiguration av hello exchange önskad med hello RESTful-tjänst. Öppna hello TrustFrameworkExtensions.xml filen och Lägg till följande XML-kodstycke i hello hello `<ClaimsProviders>` element.

> [!NOTE]
> I hello följande XML, RESTful-providern `Version=1.0.0.0` beskrivs som hello-protokollet. Överväg att som hello-funktion som kommer att interagera med externa hello-tjänsten. <!-- TODO: A full definition of hello schema can be found...link tooRESTful Provider schema definition>-->

```xml
<ClaimsProvider>
    <DisplayName>REST APIs</DisplayName>
    <TechnicalProfiles>
        <TechnicalProfile Id="AzureFunctions-CheckPlayerTagWebHook">
            <DisplayName>Check Player Tag Web Hook Azure Function</DisplayName>
            <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
            <Metadata>
                <Item Key="ServiceUrl">https://wingtipb2cfuncs.azurewebsites.net/api/CheckPlayerTagWebHook?code=L/05YRSpojU0nECzM4Tp3LjBiA2ZGh3kTwwp1OVV7m0SelnvlRVLCg==</Item>
                <Item Key="AuthenticationType">None</Item>
                <Item Key="SendClaimsIn">Body</Item>
            </Metadata>
            <InputClaims>
                <InputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="playerTag" />
            </InputClaims>
            <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop" />
        </TechnicalProfile>
        <TechnicalProfile Id="SelfAsserted-ProfileUpdate">
            <ValidationTechnicalProfiles>
                <ValidationTechnicalProfile ReferenceId="AzureFunctions-CheckPlayerTagWebHook" />
            </ValidationTechnicalProfiles>
        </TechnicalProfile>
    </TechnicalProfiles>
</ClaimsProvider>
```

Hej `InputClaims` elementet definierar hello anspråk som ska skickas från hello IEF toohello REST-tjänst. I det här exemplet hello innehållet i hello anspråk `givenName` skickas toohello REST-tjänst som `playerTag`. I det här exemplet anspråk hello IEF inte förväntar sig tillbaka. I stället väntar på svar från hello REST-tjänst och åtgärder baserat på hello statuskoder som tas emot.

## <a name="step-3-include-hello-restful-service-claims-exchange-in-self-asserted-technical-profile-where-you-want-toovalidate-hello-user-input"></a>Steg 3: Inkludera hello RESTful-tjänst anspråk exchange i själva uppgavs tekniska profil där du vill att toovalidate hello användarindata

hello används vanligaste hello validering steg i hello interaktion med användaren. All interaktion där hello användaren är förväntade tooprovide indata är *själva vars tekniska profiler*. I det här exemplet ska vi lägga till hello toohello egen Asserted ProfileUpdate tekniska verifieringsprofil. Detta är hello tekniska profil som hello förlitande part (RP) principfil `Profile Edit` använder.

tooadd hello anspråk exchange toohello själva vars tekniska profil:

1. Öppna hello TrustFrameworkBase.xml filen och Sök efter `<TechnicalProfile Id="SelfAsserted-ProfileUpdate">`.
2. Granska hello konfigurationen av den här tekniska profilen. Se hur hello exchange med hello användare har definierats som anspråk som blir ombedd av hello användare (inkommande anspråk) och anspråk som förväntas tillbaka från själva uppgavs hello-providern (utgående anspråk).
3. Sök efter `TechnicalProfileReferenceId="SelfAsserted-ProfileUpdate`, och Lägg märke till att den här profilen anropas som orchestration-steg 6 av `<UserJourney Id="ProfileEdit">`.

## <a name="step-4-upload-and-test-hello-profile-edit-rp-policy-file"></a>Steg 4: Överför och testa hello redigera RP princip profilfil

1. Överför hello ny version av hello TrustFrameworkExtensions.xml filen.
2. Använd **kör nu** tootest hello profil redigera RP principfil.
3. Testa hello-validering genom att tillhandahålla en av hello befintliga (till exempel mcvinny) i hello **Förnamn** fältet. Om allt är korrekt konfigurerat, får ett meddelande som meddelar användaren hello hello player-tagg som redan används.

## <a name="next-steps"></a>Nästa steg

[Ändra hello profil redigera och användaren registrering toogather ytterligare information från användarna](active-directory-b2c-create-custom-attributes-profile-edit-custom.md)

[Genomgång: Integrera utbyte av REST API-anspråk i din Azure AD B2C användaren resa som ett orchestration-steg](active-directory-b2c-rest-api-step-custom.md)
