tooenable logga in på ditt program behöver du toocreate loggar in principen. Den här principen beskriver hello upplevelser som konsumenter går igenom under inloggning och hello innehållet i token som hello programmet får på lyckade inloggningar.

[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](active-directory-b2c-portal-navigate-b2c-service.md)]

Välj hello principer under inställningar **principer för registrering eller inloggning** och på **+ Lägg till**.

![Välj registrerings- eller inloggningsprinciper och klicka på Lägg till](media/active-directory-b2c-create-sign-in-sign-up-policy/add-b2c-signup-signin-policy.png)

Ange en princip **namn** för ditt program tooreference. Ange till exempel `SiUpIn`.

Välj **Identitetsprovidrar** och markera **Registrering via e-post**. Du kan också välja leverantörer via sociala nätverk om det redan har konfigurerats. Klicka på **OK**.

![Välj e-registreringen som en identitetsleverantör och klicka på hello OK](media/active-directory-b2c-create-sign-in-sign-up-policy/add-b2c-signup-signin-identity-providers.png)

Välj **Registreringsattribut**. Välj attribut du vill toocollect från hello konsumenten under registreringen. Välj till exempel **land/region**, **visningsnamn** och **postnummer**. Klicka på **OK**.

![Välj vissa attribut och klicka på hello OK](media/active-directory-b2c-create-sign-in-sign-up-policy/add-b2c-signup-signin-sign-up-attributes.png)

Välj **Programanspråk**. Välj anspråk som du vill ska returneras i hello auktorisering token skickas tillbaka tooyour program efter en lyckad registrering eller inloggning upplevelse. Välj till exempel **visningsnamn**, **identitetsprovidrar**, **postnummer**, **ny användare** och **användarobjekt-id**.

![Välj några programanspråk och klicka på OK](media/active-directory-b2c-create-sign-in-sign-up-policy/add-b2c-signup-signin-application-claims.png)

Klicka på **skapa** tooadd hello princip. hello principen anges som **B2C_1_SiUpIn**. Hej **B2C_1_** prefix är tillagda toohello namn.

Öppna hello princip genom att välja **B2C_1_SiUpIn**. Kontrollera hello inställningarna i hello tabell och klicka sedan på **kör nu**.

![Välj princip och kör den](media/active-directory-b2c-create-sign-in-sign-up-policy/run-b2c-signup-signin-policy.png)

| Inställning      | Värde  |
| ------------ | ------ |
| **Program** | Contoso B2C-app |
| **Välj svarswebbadress** | `https://localhost:44316/` |

En ny webbläsarflik öppnas och du kan kontrollera hello registrering eller inloggning användarfunktioner som konfigurerats.

> [!NOTE]
> Den tar upp tooa minut för att skapa en princip och uppdaterar tootake effekt.
>