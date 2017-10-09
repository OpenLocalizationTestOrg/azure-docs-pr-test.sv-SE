[!INCLUDE [active-directory-b2c-portal-add-application](active-directory-b2c-portal-add-application.md)]

tooregister ditt webb-API, Använd hello inställningarna i hello tabell.

![Exempel på registreringsinställningar för nytt webb-api](./media/active-directory-b2c-register-web-api/b2c-new-web-api-settings.png)

| Inställning      | Exempelvärde  | Beskrivning                                        |
| ------------ | ------- | -------------------------------------------------- |
| **Namn** | Contoso B2C-API | Ange en **namn** för hello-programmet som beskriver ditt API-tooconsumers. | 
| **Ta med webbapp/webb-API** | Ja | Välj **Ja** om det är ett webb-API. |
| **Tillåt implicit flöde** | Ja | Välj **Ja** om [OpenID Connect-inloggning](../articles/active-directory-b2c/active-directory-b2c-reference-oidc.md) används för programmet |
| **Svarswebbadress** | `https://localhost:44316/` | Svarswebbadresser är slutpunkter där Azure AD B2C returnerar de token som programmet begär. Ange [en korrekt](../articles/active-directory-b2c/active-directory-b2c-app-registration.md#choosing-a-web-app-or-api-reply-url) **svarswebbadress**. I det här exemplet är ditt webb-API lokalt och lyssnar på port 44316. |
| **URI för app-id** | api | hello App-ID URI är hello-identifierare som används för ditt webb-API. Hej fullständiga identifieraren URI inklusive hello domän genereras. |

Klicka på **skapa** tooregister ditt program.

Nyligen registrerade programmet visas i listan med program hello för hello B2C-klient. Välj ditt webb-API hello-listan. hello API: er visas egenskapen.

![Egenskaper för webb-API](./media/active-directory-b2c-register-web-api/b2c-web-api-properties.png)

Anteckna hello globalt unik **klient-ID för programmet**. Du kan använda hello-ID i din programkod.
