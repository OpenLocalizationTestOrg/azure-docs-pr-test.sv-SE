---
title: "Azure Active Directory B2C: REST API-anspråk utbyte som ett orchestration-steg | Microsoft Docs"
description: "Ett ämne på Azure Active Directory B2C anpassade principer som integreras med en API"
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
ms.openlocfilehash: 90a495029f48d70232ef3f99de4ea4d351395aa7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="walkthrough-integrate-rest-api-claims-exchanges-in-your-azure-ad-b2c-user-journey-as-an-orchestration-step"></a>Genomgång: Integrera utbyte av REST API-anspråk i din Azure AD B2C användaren resa som ett orchestration-steg

hello identitet upplevelse Framework (IEF) för Azure Active Directory B2C det (Azure AD B2C) gör det möjligt för hello identitet developer toointegrate interaktion med en RESTful-API i en resa för användaren.  

Hello slutet av den här genomgången, kommer du att kunna toocreate en Azure AD B2C användaren resa som interagerar med RESTful-tjänster.

hello IEF skickar data i anspråk och tar emot data tillbaka i anspråk. hello REST API anspråk exchange:

- Kan utformas som en orchestration-steg.
- Kan utlösa en extern åtgärd. Det kan till exempel logga en händelse i en extern databas.
- Kan använda toofetch ett värde och sedan lagra den i hello användardatabas.

Du kan använda hello emot anspråk senare toochange hello flödet av körningen.

Du kan också utforma hello interaktion som en verifieringsprofil. Mer information finns i [genomgång: integrera REST API-anspråk utbyte i din Azure AD B2C användaren resa som autentiserades på indata från användaren](active-directory-b2c-rest-api-validation-custom.md).

hello scenario är att när användaren utför en profil-redigering, vi vill:

1. Leta upp hello användare i ett externt system.
2. Hämta hello stad där användaren har registrerats.
3. Returnera attributet toohello programmet som ett anspråk.

## <a name="prerequisites"></a>Krav

- En Azure AD B2C-klient som konfigurerats toocomplete ett lokalt konto sign-upp/inloggning, enligt beskrivningen i [komma igång](active-directory-b2c-get-started-custom.md).
- En REST API-slutpunkten toointeract med. Den här genomgången använder en enkel Azure funktionen app webhook som ett exempel.
- *Rekommenderade*: fullständig hello [REST API-anspråk exchange genomgång som en verifiering steg](active-directory-b2c-rest-api-validation-custom.md).

## <a name="step-1-prepare-hello-rest-api-function"></a>Steg 1: Förbered hello REST API-funktion

> [!NOTE]
> Installationen av REST API-funktioner är utanför hello omfånget för den här artikeln. [Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-reference) ger en utmärkt toolkit toocreate RESTful-tjänster i hello molnet.

Vi har konfigurerat en Azure-funktion som tar emot ett anspråk som kallas `email`, och sedan returnerar hello anspråk `city` med hello tilldelas värdet `Redmond`. hello exempel Azure funktionen finns på [GitHub](https://github.com/Azure-Samples/active-directory-b2c-advanced-policies/tree/master/AzureFunctionsSamples).

Hej `userMessage` anspråk som hello Azure returnerar är valfri i den här kontexten och hello IEF ignoreras den. Du kan eventuellt använda den som ett meddelande skickades toohello program och presenteras toohello användaren senare.

```csharp
if (requestContentAsJObject.email == null)
{
    return request.CreateResponse(HttpStatusCode.BadRequest);
}

var email = ((string) requestContentAsJObject.email).ToLower();

return request.CreateResponse<ResponseContent>(
    HttpStatusCode.OK,
    new ResponseContent
    {
        version = "1.0.0",
        status = (int) HttpStatusCode.OK,
        userMessage = "User Found",
        city = "Redmond"
    },
    new JsonMediaTypeFormatter(),
    "application/json");
```

En funktionsapp i Azure-gör det enkelt tooget hello funktions-URL, vilket innefattar hello identifierare för hello specifik funktion. I det här fallet hello URL: en är: https://wingtipb2cfuncs.azurewebsites.net/api/LookUpLoyaltyWebHook?code=MQuG7BIE3eXBaCZ/YCfY1SHabm55HEphpNLmh1OP3hdfHkvI2QwPrw==. Du kan använda den för att testa.

## <a name="step-2-configure-hello-restful-api-claims-exchange-as-a-technical-profile-in-your-trustframeworextensionsxml-file"></a>Steg 2: Konfigurera exchange för hello RESTful-API-anspråk som en teknisk profil i filen TrustFrameworExtensions.xml

En teknisk profil är hello fullständig konfiguration av hello exchange önskad med hello RESTful-tjänst. Öppna hello TrustFrameworkExtensions.xml filen och Lägg till följande XML-kodstycke i hello hello `<ClaimsProvider>` element.

> [!NOTE]
> I hello följande XML, RESTful-providern `Version=1.0.0.0` beskrivs som hello-protokollet. Överväg att som hello-funktion som kommer att interagera med externa hello-tjänsten. <!-- TODO: A full definition of hello schema can be found...link tooRESTful Provider schema definition>-->

```XML
<ClaimsProvider>
    <DisplayName>REST APIs</DisplayName>
    <TechnicalProfiles>
        <TechnicalProfile Id="AzureFunctions-LookUpLoyaltyWebHook">
            <DisplayName>Check LookUpLoyalty Web Hook Azure Function</DisplayName>
            <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
            <Metadata>
                <Item Key="ServiceUrl">https://wingtipb2cfuncs.azurewebsites.net/api/LookUpLoyaltyWebHook?code=MQuG7BIE3eXBaCZ/YCfY1SHabm55HEphpNLmh1OP3hdfHkvI2QwPrw==</Item>
                <Item Key="AuthenticationType">None</Item>
                <Item Key="SendClaimsIn">Body</Item>
            </Metadata>
            <InputClaims>
                <InputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="email" />
            </InputClaims>
            <OutputClaims>
                <OutputClaim ClaimTypeReferenceId="city" PartnerClaimType="city" />
            </OutputClaims>
            <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop" />
        </TechnicalProfile>
    </TechnicalProfiles>
</ClaimsProvider>
```

Hej `<InputClaims>` elementet definierar hello anspråk som ska skickas från hello IEF toohello REST-tjänst. I det här exemplet hello innehållet i hello anspråk `givenName` skickas toohello REST-tjänst som hello anspråk `email`.  

Hej `<OutputClaims>` elementet definierar hello anspråk som hello IEF förväntar sig från hello REST-tjänst. Oavsett hello antal anspråk som tas emot hello IEF kommer att använda dem som anges här. I det här exemplet ett anspråk som tas emot som `city` mappade tooan IEF anspråk anropas `city`.

## <a name="step-3-add-hello-new-claim-city-toohello-schema-of-your-trustframeworkextensionsxml-file"></a>Steg 3: Lägg till nytt anspråk för hello `city` toohello schemat för TrustFrameworkExtensions.xml-fil

hello anspråk `city` är ännu inte definierats var som helst i vår schemat. Lägg därför till en definition i hello element `<BuildingBlocks>`. Du hittar det här elementet hello början av hello TrustFrameworkExtensions.xml fil.

```XML
<BuildingBlocks>
    <!--hello claimtype city must be added toohello TrustFrameworkPolicy-->
    <!-- You can add new claims in hello BASE file Section III, or in hello extensions file-->
    <ClaimsSchema>
        <ClaimType Id="city">
            <DisplayName>City</DisplayName>
            <DataType>string</DataType>
            <UserHelpText>Your city</UserHelpText>
            <UserInputType>TextBox</UserInputType>
        </ClaimType>
    </ClaimsSchema>
</BuildingBlocks>
```

## <a name="step-4-include-hello-rest-service-claims-exchange-as-an-orchestration-step-in-your-profile-edit-user-journey-in-trustframeworkextensionsxml"></a>Steg 4: Ta hello REST service anspråk exchange som ett orchestration-steg i din profil Redigera användare resa i TrustFrameworkExtensions.xml

Lägga till ett steg toohello profil Redigera användare resa efter hello användaren har autentiserats (orchestration-steg 1 – 4 i hello följande XML) och hello användaren har angett hello uppdateras profilinformation (steg 5).

> [!NOTE]
> Det finns många användningsområden där hello REST API-anrop kan användas som ett orchestration-steg. Som ett orchestration-steg den kan användas som ett externt system update tooan när en användare har slutfört en uppgift som första gången registrering eller som en profil uppdatering tookeep information som synkroniseras. I det här fallet är det använda tooaugment hello informationen toohello programmet när hello profil redigera.

Kopiera hello profil Redigera användare resa XML-koden från hello TrustFrameworkBase.xml tooyour TrustFrameworkExtensions.xml fil i hello `<UserJourneys>` element. Gör hello ändras i steg 6.

```XML
<OrchestrationStep Order="6" Type="ClaimsExchange">
    <ClaimsExchanges>
        <ClaimsExchange Id="GetLoyaltyData" TechnicalProfileReferenceId="AzureFunctions-LookUpLoyaltyWebHook" />
    </ClaimsExchanges>
</OrchestrationStep>
```

> [!IMPORTANT]
> Om din version inte matchar hello ordning se till att du infogar hello kod som hello steg innan hello `ClaimsExchange` typen `SendClaims`.

hello sista XML för hello användaren resa bör se ut så här:

```XML
<UserJourney Id="ProfileEdit">
    <OrchestrationSteps>
        <OrchestrationStep Order="1" Type="ClaimsProviderSelection" ContentDefinitionReferenceId="api.idpselections">
            <ClaimsProviderSelections>
                <ClaimsProviderSelection TargetClaimsExchangeId="FacebookExchange" />
                <ClaimsProviderSelection TargetClaimsExchangeId="LocalAccountSigninEmailExchange" />
            </ClaimsProviderSelections>
        </OrchestrationStep>
        <OrchestrationStep Order="2" Type="ClaimsExchange">
            <ClaimsExchanges>
                <ClaimsExchange Id="FacebookExchange" TechnicalProfileReferenceId="Facebook-OAUTH" />
                <ClaimsExchange Id="LocalAccountSigninEmailExchange" TechnicalProfileReferenceId="SelfAsserted-LocalAccountSignin-Email" />
            </ClaimsExchanges>
        </OrchestrationStep>
        <OrchestrationStep Order="3" Type="ClaimsExchange">
            <Preconditions>
                <Precondition Type="ClaimEquals" ExecuteActionsIf="true">
                    <Value>authenticationSource</Value>
                    <Value>localAccountAuthentication</Value>
                    <Action>SkipThisOrchestrationStep</Action>
                </Precondition>
            </Preconditions>
            <ClaimsExchanges>
                <ClaimsExchange Id="AADUserRead" TechnicalProfileReferenceId="AAD-UserReadUsingAlternativeSecurityId" />
            </ClaimsExchanges>
        </OrchestrationStep>
        <OrchestrationStep Order="4" Type="ClaimsExchange">
            <Preconditions>
                <Precondition Type="ClaimEquals" ExecuteActionsIf="true">
                    <Value>authenticationSource</Value>
                    <Value>socialIdpAuthentication</Value>
                    <Action>SkipThisOrchestrationStep</Action>
                </Precondition>
            </Preconditions>
            <ClaimsExchanges>
                <ClaimsExchange Id="AADUserReadWithObjectId" TechnicalProfileReferenceId="AAD-UserReadUsingObjectId" />
            </ClaimsExchanges>
        </OrchestrationStep>
        <OrchestrationStep Order="5" Type="ClaimsExchange">
            <ClaimsExchanges>
                <ClaimsExchange Id="B2CUserProfileUpdateExchange" TechnicalProfileReferenceId="SelfAsserted-ProfileUpdate" />
            </ClaimsExchanges>
        </OrchestrationStep>
        <!-- Add a step 6 toohello user journey before hello JWT token is created-->
        <OrchestrationStep Order="6" Type="ClaimsExchange">
            <ClaimsExchanges>
                <ClaimsExchange Id="GetLoyaltyData" TechnicalProfileReferenceId="AzureFunctions-LookUpLoyaltyWebHook" />
            </ClaimsExchanges>
        </OrchestrationStep>
        <OrchestrationStep Order="7" Type="SendClaims" CpimIssuerTechnicalProfileReferenceId="JwtIssuer" />
    </OrchestrationSteps>
    <ClientDefinition ReferenceId="DefaultWeb" />
</UserJourney>
```

## <a name="step-5-add-hello-claim-city-tooyour-relying-party-policy-file-so-hello-claim-is-sent-tooyour-application"></a>Steg 5: Lägg till hello anspråk `city` tooyour förlitande part princip filen så hello anspråk skickas tooyour program

Redigera filen ProfileEdit.xml förlitande part (RP) och ändra hello `<TechnicalProfile Id="PolicyProfile">` elementet tooadd hello följande: `<OutputClaim ClaimTypeReferenceId="city" />`.

När du har lagt till hello nytt anspråk hello tekniska profil ser ut så här:

```XML
<DisplayName>PolicyProfile</DisplayName>
    <Protocol Name="OpenIdConnect" />
    <OutputClaims>
      <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub"/>
      <OutputClaim ClaimTypeReferenceId="city" />
    </OutputClaims>
    <SubjectNamingInfo ClaimType="sub" />
</TechnicalProfile>
```

## <a name="step-6-upload-your-changes-and-test"></a>Steg 6: Överföra dina ändringar och testa

Skriv över hello befintliga versioner av hello princip.

1.  (Valfritt:) Spara hello befintlig version (genom att ladda ned) av tilläggsfilen innan du fortsätter. tookeep hello inledande komplexitet låg, rekommenderar vi att du inte överföra flera versioner av hello-tillägg-fil.
2.  (Valfritt:) Byt namn på hello ny version av hello princip-ID för hello princip redigera filen genom att ändra `PolicyId="B2C_1A_TrustFrameworkProfileEdit"`.
3.  Överför hello-tillägg-fil.
4.  Överför hello principfil redigera RP.
5.  Använd **kör nu** tootest hello princip. Granska hello token som hello IEF returnerar toohello program.

Om allt är korrekt konfigurerat, hello token tas hello nytt anspråk `city`, med hello värdet `Redmond`.

```JSON
{
  "exp": 1493053292,
  "nbf": 1493049692,
  "ver": "1.0",
  "iss": "https://login.microsoftonline.com/f06c2fe8-709f-4030-85dc-38a4bfd9e82d/v2.0/",
  "sub": "a58e7c6c-7535-4074-93da-b0023fbaf3ac",
  "aud": "4e87c1dd-e5f5-4ac8-8368-bc6a98751b8b",
  "acr": "b2c_1a_trustframeworkprofileedit",
  "nonce": "defaultNonce",
  "iat": 1493049692,
  "auth_time": 1493049692,
  "city": "Redmond"
}
```

## <a name="next-steps"></a>Nästa steg

[Använd en REST-API som en verifiering steg](active-directory-b2c-rest-api-validation-custom.md)

[Ändra hello profil redigera toogather ytterligare information från användarna](active-directory-b2c-create-custom-attributes-profile-edit-custom.md)
