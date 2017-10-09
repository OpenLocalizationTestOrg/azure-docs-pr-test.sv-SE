tooenable logga in på ditt program behöver du toocreate loggar in principen. Den här principen beskriver hello upplevelser som konsumenter går igenom under inloggning och hello innehållet i token som hello programmet får på lyckade inloggningar.

[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](active-directory-b2c-portal-navigate-b2c-service.md)] Klicka på **Inloggningsprinciper**.

Klicka på **+ Lägg till** hello överst i hello-bladet.

Hej **namn** anger hello inloggningsprincip namn som används av ditt program. Ange till exempel **SiIn**.

Klicka på **Identitetsprovidrar** och välj **Inloggning från lokalt konto**. Du kan också välja leverantörer via sociala nätverk om det redan har konfigurerats. Klicka på **OK**.

Klicka på **Programanspråk**. Du väljer här anspråk som du vill ska returneras i hello token skickas tillbaka tooyour program efter en lyckad inloggning. Välj till exempel **Visningsnamn**, **Identitetsprovider**, **Postnummer** och **Användarobjekt-id**. Klicka på **OK**.

Klicka på **Skapa**. Observera att hello princip just har skapat visas som **B2C_1_SiIn** (hello **B2C\_1\_**  fragment läggs till automatiskt) i hello **inloggning principer**bladet.

Öppna hello princip genom att klicka på **B2C_1_SiIn**.

Välj **Contoso B2C-app** i hello **program** listrutan och `https://localhost:44321/` i hello **Reply URL / omdirigerings-URI** listrutan.

Klicka på **Kör nu**. En ny webbläsarflik öppnas och du kan köra via hello användarfunktioner av du loggar in på ditt program.

> [!NOTE]
> Den tar upp tooa minut för att skapa en princip och uppdaterar tootake effekt.
>