[!INCLUDE [active-directory-b2c-portal-add-application](active-directory-b2c-portal-add-application.md)]

tooregister ditt webbprogram, Använd hello inställningarna i hello tabell.

![Exempel på registreringsinställningar för ny webbapp](./media/active-directory-b2c-register-web-app/b2c-new-app-settings.png)

| Inställning      | Exempelvärde  | Beskrivning                                        |
| ------------ | ------- | -------------------------------------------------- |
| **Namn** | Contoso B2C-app | Ange en **namn** för hello-programmet som beskriver ditt program tooconsumers. | 
| **Ta med webbapp/webb-API** | Ja | Välj **Ja** om det är ett webbprogram. |
| **Tillåt implicit flöde** | Ja | Välj **Ja** om [OpenID Connect-inloggning](../articles/active-directory-b2c/active-directory-b2c-reference-oidc.md) används för programmet |
| **Svarswebbadress** | `https://localhost:44316` | Svarswebbadresser är slutpunkter där Azure AD B2C returnerar de token som programmet begär. Ange [en korrekt](../articles/active-directory-b2c/active-directory-b2c-app-registration.md#choosing-a-web-app-or-api-reply-url) **svarswebbadress**. I det här exemplet är din app lokal och lyssnar på port 44316. |

Klicka på **skapa** tooregister ditt program.

Nyligen registrerade programmet visas i listan med program hello för hello B2C-klient. Välj ditt webbprogram hello-listan. hello webbprogrammets visas egenskapen.

![Webbappegenskaper](./media/active-directory-b2c-register-web-app/b2c-web-app-properties.png)

Anteckna hello globalt unik **klient-ID för programmet**. Du kan använda hello-ID i din programkod.
