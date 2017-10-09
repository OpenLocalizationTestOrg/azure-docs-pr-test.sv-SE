[!INCLUDE [active-directory-b2c-portal-add-application](active-directory-b2c-portal-add-application.md)]

tooregister tillämpningsprogrammet mobila och interna hello inställningar anges i hello tabell.

![Exempel på registreringsinställningar för nytt mobilt eller inbyggt program](./media/active-directory-b2c-register-mobile-native-app/b2c-new-mobile-native-app-settings.png)

| Inställning      | Exempelvärde  | Beskrivning                                        |
| ------------ | ------- | -------------------------------------------------- |
| **Namn** | Contoso B2C-app | Ange en **namn** för hello-programmet som beskriver ditt program tooconsumers. |
| **Inbyggd klient** | Ja | Välj **Ja** om du har ett mobilt eller inbyggt program. |
| **Anpassad omdirigerings-URI** | `com.onmicrosoft.contoso.appname://redirect/path` | Ange en omdirigerings-URI med ett anpassat schema. Se till att välja en [bra omdirigerings-URI](../articles/active-directory-b2c/active-directory-b2c-app-registration.md#choosing-a-native-application-redirect-uri) som inte innehåller specialtecken, till exempel understreck. |

Klicka på **skapa** tooregister ditt program.

Nyligen registrerade programmet visas i listan med program hello för hello B2C-klient. Välj appen mobila och interna hello-listan. hello programmet visas egenskapen.

![Egenskaper för program](./media/active-directory-b2c-register-mobile-native-app/b2c-mobile-native-app-properties.png)

Anteckna hello globalt unik **klient-ID för programmet**. Du kan använda hello-ID i din programkod.

Gör så här om det inbyggda programmet anropar ett webb-API som skyddas av Azure AD B2C:
   1. Skapa en programhemlighet genom att gå toohello **nycklar** bladet och klicka på hello **Generera nyckel** knappen. Anteckna hello **appkey** värde. Du kan använda hello-värde som hello programhemlighet i din programkod.
   2. Klicka på **API-åtkomst**, **Lägg till** och välj webb-API och områden (behörigheter).

> [!NOTE]
> En **programhemlighet** är en viktig autentiseringsuppgift och bör skyddas på lämpligt sätt.
> 
