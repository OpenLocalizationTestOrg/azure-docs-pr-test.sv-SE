---
title: "Azure Active Directory B2C: Lägga till egna attribut toocustom principer och använda i profilen redigera | Microsoft Docs"
description: "En genomgång av använder tilläggsegenskaper, anpassade attribut och inkludera dem i hello användargränssnittet"
services: active-directory-b2c
documentationcenter: 
author: rojasja
manager: krassk
editor: 
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 08/04/2017
ms.author: joroja
ms.openlocfilehash: 8cc9c6a38d7652797ba54a3e02078ac2bf4a693b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-creating-and-using-custom-attributes-in-a-custom-profile-edit-policy"></a>Azure Active Directory B2C: Skapa och använda anpassade attribut i en anpassad profil redigera principen

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

I den här artikeln, skapa ett anpassat attribut i Azure AD B2C-katalogen och använda den här nya attributet som ett anpassat anspråk i hello profil Redigera användare resa.

## <a name="prerequisites"></a>Krav

Fullständig hello stegen i artikeln hello [komma igång med principer för anpassade](active-directory-b2c-get-started-custom.md).

## <a name="use-custom-attributes-toocollect-information-about-your-customers-in-azure-active-directory-b2c-using-custom-policies"></a>Använd anpassade attribut toocollect information om dina kunder i Azure Active Directory B2C anpassade principer
Din Azure Active Directory (AD Azure) B2C-katalog som levereras med en inbyggd uppsättning attribut: namn, efternamn, ort, postnummer, userPrincipalName får osv.  Du behöver ofta toocreate egna attribut.  Exempel:
* En kund-riktade programmet behöver toopersist ett attribut, till exempel ”LoyaltyNumber”.
* En identitetsleverantör har unika användar-ID som måste till exempel ”uniqueUserGUID” ”.
* En anpassad användare resa måste toopersist hello tillståndet för användaren, till exempel ”migrationStatus”.

Med Azure AD B2C kan du utöka hello uppsättning attribut som lagras för varje användarkonto. Du kan också läsa och skriva dessa attribut med hjälp av hello [Azure AD Graph API](active-directory-b2c-devquickstarts-graph-dotnet.md).

Egenskaper för webbtjänsttillägg utöka hello hello användarobjekt i hello directory-schema.  hello villkoren utökade egenskapen, anpassat attribut och anpassade anspråk refererar toohello samma sak i hello kontexten för den här artikeln och hello varierar beroende på hello kontext (program, objekt, princip).

Egenskaper för webbtjänsttillägg kan endast registreras på ett programobjekt trots att de kan innehålla data för en användare. hello-egenskapen är anslutna toohello program. hello programobjektet måste vara beviljade skrivbehörighet tooregister en egenskap för tillägget. 100 tilläggsegenskaper (över alla typer och alla program) kan skrivas tooany enskilt objekt. Egenskaper för webbtjänsttillägg läggs toohello directory måltypen och blir omedelbart tillgängligt i directory hello Azure AD B2C-klient.
Om programmet hello raderas tas de tilläggsegenskaper tillsammans med alla data i dem för alla användare också bort. Om en egenskap för tillägget tas bort av hello program tas det bort på hello målet katalogobjekt och hello värden tas bort.

Egenskaper för webbtjänsttillägg finns bara i hello registrerade programmet i hello-klient. hello objekt-id för programmet måste inkluderas i hello TechnicalProfile som använder den.

>[!NOTE]
>hello Azure AD B2C-katalogen innehåller vanligtvis ett webbprogram med namnet `b2c-extensions-app`.  Det här programmet används främst av hello b2c inbyggda principer för anpassade hello anspråk som skapats via hello Azure-portalen.  Med det här programmet tooregister tillägg för b2c anpassade principer rekommenderas endast för avancerade användare.  Instruktioner för detta ingår i hello nästa avsnitt i den här artikeln.


## <a name="creating-a-new-application-toostore-hello-extension-properties"></a>Skapa en ny toostore programmet hello tilläggsegenskaper

1. Öppna en webbläsarsession och navigera toohello [Azure får](https://portal.azure.com) och logga in med administratörsbehörighet på hello B2C-katalog som du vill tooconfigure.
1. Klicka på **Azure Active Directory** på hello vänstra navigeringsmenyn. Du kan behöva toofind den genom att välja fler tjänster >.
1. Välj **App registreringar** och på **nya appregistrering**
1. Ange hello följande rekommenderade poster:
  * Ange ett namn för hello-webbprogram: **WebApp-GraphAPI-DirectoryExtensions**
  * Programtyp: webb-app/API
  * URL:https://{tenantName}.onmicrosoft.com/WebApp-GraphAPI-DirectoryExtensions inloggning
1. Välj ** Skapa. Åtgärden lyckades visas i hello **meddelanden**
1. Välj hello nyskapad webbprogram: **WebApp-GraphAPI-DirectoryExtensions**
1. Välj inställningar: **nödvändiga behörigheter**
1. Välj API **Windows Active Directory**
1. Markera programbehörigheter: **läsning och skrivning katalogdata**, och **spara**
1. Välj **bevilja behörigheter** och bekräfta **Ja**.
1. Kopiera tooyour Urklipp och spara hello följande identifierare från WebApp-GraphAPI-DirectoryExtensions > Inställningar > Egenskaper >
*  **Program-ID** . Exempel:`103ee0e6-f92d-4183-b576-8c3739027780`
* **Objekt-ID**. Exempel:`80d8296a-da0a-49ee-b6ab-fd232aa45201`



## <a name="modifying-your-custom-policy-tooadd-hello-applicationobjectid"></a>Ändra en anpassad princip tooadd hello ApplicationObjectId

```xml
    <ClaimsProviders>
        <ClaimsProvider>
              <DisplayName>Azure Active Directory</DisplayName>
            <TechnicalProfile Id="AAD-Common">
              <DisplayName>Azure Active Directory</DisplayName>
              <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.AzureActiveDirectoryProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
              <!-- Provide objectId and appId before using extension properties. -->
              <Metadata>
                <Item Key="ApplicationObjectId">insert objectId here</Item>
                <Item Key="ClientId">insert appId here</Item>
              </Metadata>
            <!-- End of changes -->
              <CryptographicKeys>
                <Key Id="issuer_secret" StorageReferenceId="TokenSigningKeyContainer" />
              </CryptographicKeys>
              <IncludeInSso>false</IncludeInSso>
              <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop" />
            </TechnicalProfile>
        </ClaimsProvider>
    </ClaimsProviders>
```

>[!NOTE]
>Hej <TechnicalProfile Id="AAD-Common"> är refererad tooas ”gemensamma” eftersom dess element ingår i och återanvändas i alla hello Azure Active Directory TechnicalProfiles med hjälp av hello element:`<IncludeTechnicalProfile ReferenceId="AAD-Common" />`

>[!NOTE]
>När hello TechnicalProfile skriver för hello första gången toohello nyskapad tilläggsegenskapen, kan det uppstå ett enstaka fel.  Hej tilläggsegenskapen skapas hello första gången den används.  

## <a name="using-hello-new-extension-property--custom-attribute-in-a-user-journey"></a>Med hjälp av hello nya tilläggsegenskapen / anpassade attribut i en användare resa


1. Öppna hello förlitande Party(RP)-fil som beskriver principen Redigera användare resa.  Om du startar kan det vara lämpligt toodownload din redan konfigurerade version av hello RP PolicyEdit filen direkt från hello anpassad princip för Azure B2C-avsnittet i hello Azure-portalen.  Du kan också öppna XML-filen från lagringsmappen.
2. Lägg till ett anpassat anspråk `loyaltyId`.  Genom att inkludera hello anpassade anspråk i hello `<RelyingParty>` element den skickas som en parameter toohello UserJourney TechnicalProfiles och ingår i hello token för hello program.
```xml
<RelyingParty>
   <DefaultUserJourney ReferenceId="ProfileEdit" />
   <TechnicalProfile Id="PolicyProfile">
     <DisplayName>PolicyProfile</DisplayName>
     <Protocol Name="OpenIdConnect" />
     <OutputClaims>
       <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub"/>
       <OutputClaim ClaimTypeReferenceId="city" />

       <OutputClaim ClaimTypeReferenceId="extension_loyaltyId" />

     </OutputClaims>
     <SubjectNamingInfo ClaimType="sub" />
   </TechnicalProfile>
 </RelyingParty>
 ```
3. Lägg till en principfil för anspråk definition toohello tillägget `TrustFrameworkExtensions.xml` inuti hello `<ClaimsSchema>` element som visas.
```xml
<ClaimsSchema>
        <ClaimType Id="extension_loyaltyId">
            <DisplayName>Loyalty Identification Tag</DisplayName>
            <DataType>string</DataType>
            <UserHelpText>Your loyalty number from your membership card</UserHelpText>
            <UserInputType>TextBox</UserInputType>
        </ClaimType>
</ClaimsSchema>
```
4. Lägg till hello samma anspråk definitionsfilen toohello grundläggande princip `TrustFrameworkBase.xml`.  
>Lägga till en `ClaimType` definition i både hello bas- och hello tillägg är normalt inte nödvändigt, men eftersom hello nästa steg läggs hello extension_loyaltyId tooTechnicalProfiles i grundläggande hello-filen, hello princip verifieraren avvisar hello överför hello grundläggande filen utan att den.
>Det kan vara användbart tootrace hello körningen av hello användaren resa med namnet ”ProfileEdit” i hello TrustFrameworkBase.xml-filen.  Sök efter hello användaren resa av hello samma namn i redigeringsprogrammet och Observera att Orchestration steg 5 anropar hello TechnicalProfileReferenceID = ”SelfAsserted ProfileUpdate”.  Söka och inspektera den här TechnicalProfile toofamiliarize själv med hello flödet.
5. Lägg till loyaltyId som inkommande och utgående anspråk i hello TechnicalProfile ”SelfAsserted ProfileUpdate”
```xml
<TechnicalProfile Id="SelfAsserted-ProfileUpdate">
          <DisplayName>User ID signup</DisplayName>
          <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.SelfAssertedAttributeProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
          <Metadata>
            <Item Key="ContentDefinitionReferenceId">api.selfasserted.profileupdate</Item>
          </Metadata>
          <IncludeInSso>false</IncludeInSso>
          <InputClaims>

            <InputClaim ClaimTypeReferenceId="alternativeSecurityId" />
            <InputClaim ClaimTypeReferenceId="userPrincipalName" />

            <!-- Optional claims. These claims are collected from hello user and can be modified. Any claim added here should be updated in the
                 ValidationTechnicalProfile referenced below so it can be written toodirectory after being updateed by hello user, i.e. AAD-UserWriteProfileUsingObjectId. -->
            <InputClaim ClaimTypeReferenceId="givenName" />
            <InputClaim ClaimTypeReferenceId="surname" />
            <InputClaim ClaimTypeReferenceId="extension_loyaltyId"/>
          </InputClaims>
          <OutputClaims>
            <!-- Required claims -->
            <OutputClaim ClaimTypeReferenceId="executed-SelfAsserted-Input" DefaultValue="true" />

            <!-- Optional claims. These claims are collected from hello user and can be modified. Any claim added here should be updated in the
                 ValidationTechnicalProfile referenced below so it can be written toodirectory after being updateed by hello user, i.e. AAD-UserWriteProfileUsingObjectId. -->
            <OutputClaim ClaimTypeReferenceId="givenName" />
            <OutputClaim ClaimTypeReferenceId="surname" />
            <OutputClaim ClaimTypeReferenceId="extension_loyaltyId"/>
          </OutputClaims>
          <ValidationTechnicalProfiles>
            <ValidationTechnicalProfile ReferenceId="AAD-UserWriteProfileUsingObjectId" />
          </ValidationTechnicalProfiles>
        </TechnicalProfile>
```
6. Lägga till anspråk i TechnicalProfile ”AAD-UserWriteProfileUsingObjectId” toopersist hello värdet för hello anspråk i hello tilläggsegenskapen för hello aktuella användare i hello-katalogen.
```xml
<TechnicalProfile Id="AAD-UserWriteProfileUsingObjectId">
          <Metadata>
            <Item Key="Operation">Write</Item>
            <Item Key="RaiseErrorIfClaimsPrincipalAlreadyExists">false</Item>
            <Item Key="RaiseErrorIfClaimsPrincipalDoesNotExist">true</Item>
          </Metadata>
          <IncludeInSso>false</IncludeInSso>
          <InputClaims>
            <InputClaim ClaimTypeReferenceId="objectId" Required="true" />
          </InputClaims>
          <PersistedClaims>
            <!-- Required claims -->
            <PersistedClaim ClaimTypeReferenceId="objectId" />

            <!-- Optional claims -->
            <PersistedClaim ClaimTypeReferenceId="givenName" />
            <PersistedClaim ClaimTypeReferenceId="surname" />
            <PersistedClaim ClaimTypeReferenceId="extension_loyaltyId" />

          </PersistedClaims>
          <IncludeTechnicalProfile ReferenceId="AAD-Common" />
        </TechnicalProfile>
```
7. Lägga till anspråk i TechnicalProfile ”AAD-UserReadUsingObjectId” tooread hello värdet för hello tillägget attributet varje gång en användare loggar in. Hittills hello TechnicalProfiles har ändrats i hello flödet för lokala konton.  Om hello nytt attribut önskas i hello flödet för social/federerat konto måste en annan uppsättning TechnicalProfiles toobe ändras. Se nästa steg.

```xml
<!-- hello following technical profile is used tooread data after user authenticates. -->
     <TechnicalProfile Id="AAD-UserReadUsingObjectId">
       <Metadata>
         <Item Key="Operation">Read</Item>
         <Item Key="RaiseErrorIfClaimsPrincipalDoesNotExist">true</Item>
       </Metadata>
       <IncludeInSso>false</IncludeInSso>
       <InputClaims>
         <InputClaim ClaimTypeReferenceId="objectId" Required="true" />
       </InputClaims>
       <OutputClaims>
         <!-- Optional claims -->
         <OutputClaim ClaimTypeReferenceId="signInNames.emailAddress" />
         <OutputClaim ClaimTypeReferenceId="displayName" />
         <OutputClaim ClaimTypeReferenceId="otherMails" />
         <OutputClaim ClaimTypeReferenceId="givenName" />
         <OutputClaim ClaimTypeReferenceId="surname" />
         <OutputClaim ClaimTypeReferenceId="extension_loyaltyId" />
       </OutputClaims>
       <IncludeTechnicalProfile ReferenceId="AAD-Common" />
     </TechnicalProfile>
```


>[!IMPORTANT]
>Hej IncludeTechnicalProfile element lägger till alla hello-element i AAD-gemensamma toothis TechnicalProfile.

## <a name="test-hello-custom-policy-using-run-now"></a>Testa hello anpassad princip med ”Kör nu”
1. Öppna hello **Azure AD B2C bladet** och navigera för**identitet upplevelse Framework > anpassade principer**.
1. Välj hello anpassad princip som du överfört och klicka på hello **kör nu** knappen.
1. Du ska kunna toosign med en e-postadress.

hello-ID-token som skickas tillbaka tooyour program innehåller hello nya tilläggsegenskapen som ett anpassat anspråk som föregås av extension_loyaltyId. Visa exempel.

```
{
  "exp": 1493585187,
  "nbf": 1493581587,
  "ver": "1.0",
  "iss": "https://login.microsoftonline.com/f06c2fe8-709f-4030-85dc-38a4bfd9e82d/v2.0/",
  "sub": "a58e7c6c-7535-4074-93da-b0023fbaf3ac",
  "aud": "4e87c1dd-e5f5-4ac8-8368-bc6a98751b8b",
  "acr": "b2c_1a_trustframeworkprofileedit",
  "nonce": "defaultNonce",
  "iat": 1493581587,
  "auth_time": 1493581587,
  "extension_loyaltyId": "abc",
  "city": "Redmond"
}
```

## <a name="next-steps"></a>Nästa steg

Lägg till hello nya anspråk toohello flöden sociala inloggningsnamn genom att ändra hello TechnicalProfiles visas. Dessa två TechnicalProfiles används av sociala/federerat konto inloggningar toowrite och läsa hello användardata med hello alternativeSecurityId som hello lokaliserare för hello användarobjekt.
```
  <TechnicalProfile Id="AAD-UserWriteUsingAlternativeSecurityId">

  <TechnicalProfile Id="AAD-UserReadUsingAlternativeSecurityId">
```

Med hjälp av hello samma tilläggsattribut mellan inbyggda och anpassade principer.
När du lägger till tilläggsattribut (aka anpassade attribut) via hello-portaler attributen registreras med hjälp av hello ** b2c-tillägg-app som finns i varje b2c-klient.  toouse dessa tilläggsattribut i en egen princip:
1. Navigera för i din b2c-klient i portal.azure.com**Azure Active Directory** och välj **App registreringar**
2. Hitta din **b2c-tillägg-app** och markera den
3. Under 'Essentials' post hello **program-ID** och hello **objekt-ID**
4. Inkludera dem i din AAD-gemensamma tekniska Profilmetadata som på följande sätt:

```xml
    <ClaimsProviders>
        <ClaimsProvider>
              <DisplayName>Azure Active Directory</DisplayName>
            <TechnicalProfile Id="AAD-Common">
              <DisplayName>Azure Active Directory</DisplayName>
              <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.AzureActiveDirectoryProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
              <!-- Provide objectId and appId before using extension properties. -->
              <Metadata>
                <Item Key="ApplicationObjectId">insert objectId here</Item> <!-- This is hello "Object ID" from hello "b2c-extensions-app"-->
                <Item Key="ClientId">insert appId here</Item> <!--This is hello "Application ID" from hello "b2c-extensions-app"-->
              </Metadata>
```

tookeep konsekvens med hello-portaler skapa dessa attribut med hello portalens användargränssnitt *innan* du använder dem i din anpassade principer.  När du skapar ett attribut ”ActivationStatus” hello-portalen måste du referera tooit på följande sätt:

```
extension_ActivationStatus in hello custom policy
extension_<app-guid>_ActivationStatus via hello Graph API.
```


## <a name="reference"></a>Referens

* En **tekniska profil (TP)** är en elementtyp som kan betraktas som en *funktionen* som definierar en slutpunkt namn, dess metadata, dess protokoll och information hello utbyten av anspråk som hello identitet Upplevelse Framework ska utföra.  När detta *funktionen* anropas i en orchestration-steg eller från en annan TechnicalProfile, hello InputClaims och OutputClaims tillhandahålls som parametrar av hello anroparen.


* Fullständig behandling på tilläggsegenskaper finns hello artikel [DIRECTORY-SCHEMAUTÖKNINGAR | GRAPH API-BEGREPP](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions)

>[!NOTE]
>Tilläggsattribut i Graph API är namngivna med hello konventionen `extension_ApplicationObjectID_attributename`. Anpassade principer refererar tooextensions attribut som extension_attributename, vilket utesluter hello ApplicationObjectId i hello XML
