tooenable registrering för ditt program, behöver du toocreate en princip för registrering. Den här principen beskriver hello upplevelser som konsumenter gå igenom under registreringen och hello innehållet i token som hello programmet tar emot på lyckats logga in.

[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](active-directory-b2c-portal-navigate-b2c-service.md)]

Klicka på **Registreringsprinciper**.

Klicka på **+ Lägg till** hello överst i hello-bladet.

Hej **namn** anger hello registreringsprincipen namn som används av ditt program. Ange till exempel **SiUp**.

Klicka på **Identitetsprovidrar** och välj **Registrering via e-post**. Du kan också välja leverantörer via sociala nätverk om det redan har konfigurerats. Klicka på **OK**.

Klicka på **Registreringsattribut**. Här välja attribut som du vill toocollect från hello konsumenten under registreringen. Välj till exempel **Land/region**, **Visningsnamn** och **Postnummer**. Klicka på **OK**.

Klicka på **Programanspråk**. Du väljer här anspråk som du vill ska returneras i hello token skickas tillbaka tooyour program efter en lyckad registrering upplevelse. Välj till exempel **Visningsnamn**, **Identitetsprovidrar**, **Postnummer**, **Ny användare** och **Användarobjekt-id**.

Klicka på **Skapa**. hello-princip som skapas visas som **B2C_1_SiUp** (hello **B2C\_1\_**  fragment läggs till automatiskt) i hello **principer för registrering** bladet.

Öppna hello princip genom att klicka på **B2C_1_SiUp**.

Välj **Contoso B2C-app** i hello **program** listrutan och `https://localhost:44321/` i hello **Reply URL / omdirigerings-URI** listrutan.

Klicka på **Kör nu**. En ny webbläsarflik öppnas och du kan köra via hello användarfunktioner registrerar dig för ditt program.

> [!NOTE]
> Den tar upp tooa minut för att skapa en princip och uppdaterar tootake effekt.
>