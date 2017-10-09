tooenable detaljerade för återställning av lösenord för ditt program, behöver du toocreate en princip för lösenordsåterställning. Observera att hello klient hela lösenordsåterställning alternativ har angetts [här](../articles/active-directory-b2c/active-directory-b2c-reference-sspr.md). Den här principen beskriver hello upplevelser som hello konsumenter går igenom under återställning av lösenord och hello innehållet i token som hello programmet får genomförts på rätt sätt.

[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](active-directory-b2c-portal-navigate-b2c-service.md)]

Välj hello principer under inställningar **principer för lösenordsåterställning** och på **+ Lägg till**.

![Välj principer för registrering eller inloggning och klicka på knappen hello](media/active-directory-b2c-create-password-reset-policy/add-b2c-password-reset-policy.png)

Ange en princip **namn** för ditt program tooreference. Ange till exempel `SSPR`.

Välj **Identitetsprovidrar** och markera **Återställ lösenord med e-postadress**. Klicka på **OK**.

![Välj återställning av lösenord med hjälp av e-postadress som en identitetsleverantör och klicka på hello OK](media/active-directory-b2c-create-password-reset-policy/add-b2c-password-reset-identity-providers.png)

Välj **Programanspråk**. Välj anspråk som du vill ska returneras i hello auktorisering token skickas tillbaka tooyour program efter en lyckad lösenordsåterställning upplevelse. Välj till exempel **Användarobjekt-id**.

![Välj några programanspråk och klicka på OK](media/active-directory-b2c-create-password-reset-policy/add-b2c-password-reset-application-claims.png)

Klicka på **skapa** tooadd hello princip. hello principen anges som **B2C_1_SSPR**. Hej **B2C_1_** prefix är tillagda toohello namn.

Öppna hello princip genom att välja **B2C_1_SSPR**. Kontrollera hello inställningarna i hello tabell och klicka sedan på **kör nu**.

![Välj princip och kör den](media/active-directory-b2c-create-password-reset-policy/run-b2c-password-reset-policy.png)

| Inställning      | Värde  |
| ------------ | ------ |
| **Program** | Contoso B2C-app |
| **Välj svarswebbadress** | `https://localhost:44316/` |

En ny webbläsarflik öppnas och du kan kontrollera hello lösenordsåterställning användarfunktioner i ditt program.

> [!NOTE]
> Den tar upp tooa minut för att skapa en princip och uppdaterar tootake effekt.
>
