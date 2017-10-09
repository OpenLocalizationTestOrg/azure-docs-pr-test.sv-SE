tooenable profil redigering för ditt program måste toocreate en profil redigerar principen. Den här principen beskriver hello upplevelser som konsumenter går igenom under profil redigering och hello innehållet i token som programmet hello får på lyckades.

[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](active-directory-b2c-portal-navigate-b2c-service.md)]

Välj hello principer under inställningar **redigera principer profiler** och på **+ Lägg till**.

![Välj profil ändra principer och klicka hello Lägg till](media/active-directory-b2c-create-profile-editing-policy/add-b2c-editing-policy.png)

Ange en princip **namn** för ditt program tooreference. Ange till exempel `SiPe`.

Välj **Identitetsprovidrar** och markera **Inloggning från lokalt konto**. Du kan också välja leverantörer via sociala nätverk om det redan har konfigurerats. Klicka på **OK**.

![Välj lokal inloggning för kontot som en identitetsleverantör och klicka på hello OK](media/active-directory-b2c-create-profile-editing-policy/add-b2c-editing-identity-providers.png)

Välj **Profilattribut**. Välj attribut hello konsumenten kan visa och redigera sin profil. Välj till exempel **land/region**, **visningsnamn** och **postnummer**. Klicka på **OK**.

![Välj vissa attribut och klicka på hello OK](media/active-directory-b2c-create-profile-editing-policy/add-b2c-editing-attributes.png)

Välj **Programanspråk**. Välj anspråk som du vill ska returneras i hello auktorisering token skickas tillbaka tooyour program efter en lyckad profil redigeringsmiljön. Välj till exempel **Visningsnamn**, **Postnummer**.

![Välj några programanspråk och klicka på OK](media/active-directory-b2c-create-profile-editing-policy/add-b2c-editing-application-claims.png)

Klicka på **skapa** tooadd hello princip. hello principen anges som **B2C_1_SiPe**. Hej **B2C_1_** prefix är tillagda toohello namn.

Öppna hello princip genom att välja **B2C_1_SiPe**. Kontrollera hello inställningarna i hello tabell och klicka sedan på **kör nu**.

![Välj princip och kör den](media/active-directory-b2c-create-profile-editing-policy/run-b2c-editing-policy.png)

| Inställning      | Värde  |
| ------------ | ------ |
| **Program** | Contoso B2C-app |
| **Välj svarswebbadress** | `https://localhost:44316/` |

En ny webbläsarflik öppnas och du kan kontrollera hello redigera användarfunktioner som konfigurerats för profiler.

> [!NOTE]
> Den tar upp tooa minut för att skapa en princip och uppdaterar tootake effekt.
>