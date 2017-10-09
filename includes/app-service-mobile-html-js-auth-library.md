### <a name="server-auth"></a>Gör så här för att: autentisera med en provider (Server Flow)
toohave Mobile Apps hantera hello autentiseringsprocessen i din app, måste du registrera din app med din identitetsprovider. I din Azure App Service måste tooconfigure hello program-ID och hemlighet som tillhandahålls av leverantören.
Mer information finns i självstudiekursen hello [Lägg till autentisering tooyour app](../articles/app-service-mobile/app-service-mobile-cordova-get-started-users.md).

När du har registrerat din identitetsleverantör anropa hello `.login()` metod med hello namnet på leverantören. Till exempel använda toologin med Facebook hello följande kod:

```
client.login("facebook").done(function (results) {
     alert("You are now logged in as: " + results.userId);
}, function (err) {
     alert("Error: " + err);
});
```

hello giltiga värden för hello-providern är 'aad', 'facebook', 'google', 'microsoftaccount' och 'twitter'.

> [!NOTE]
> Google-autentisering fungerar för närvarande inte via Server Flow.  tooauthenticate med Google, måste du använda en [-flödet metoden](#client-auth).

I detta fall hanterar Azure App Service hello OAuth 2.0-autentiseringsflöde.  Den visar hello inloggningssidan för hello vald leverantör och genererar en Apptjänst-autentiseringstoken efter genomförd inloggning med hello identitetsprovider. hello returneras inloggning, när du är klar, ett JSON-objekt som exponerar både hello användar-ID och Apptjänst autentiseringstoken i hello användar-ID- och authenticationToken respektive. Den här token kan cachelagras och återanvändas tills den upphör att gälla.

###<a name="client-auth"></a>Gör så här för att: autentisera med en provider (Client Flow)

Appen kan även oberoende kontakta hello identitetsleverantör och ange sedan hello returnerade token tooyour Apptjänst för autentisering. Det här flödet kan tooprovide en enkel inloggning för användare eller tooretrieve ytterligare användardata från hello identitetsleverantören.

#### <a name="social-authentication-basic-example"></a>Grundläggande exempel på social autentisering

I det här exemplet används Facebook-klientens SDK för autentisering:

```
client.login(
     "facebook",
     {"access_token": token})
.done(function (results) {
     alert("You are now logged in as: " + results.userId);
}, function (err) {
     alert("Error: " + err);
});

```
Det här exemplet förutsätter att hello-token under förutsättning av hello respektive providern SDK lagras i hello token variabeln.

#### <a name="microsoft-account-example"></a>Exempel med Microsoft-konto

följande exempel använder hello hello Live SDK, som stöder enkel inloggning för Windows Store-appar med hjälp av Account:

```
WL.login({ scope: "wl.basic"}).then(function (result) {
      client.login(
            "microsoftaccount",
            {"authenticationToken": result.session.authentication_token})
      .done(function(results){
            alert("You are now logged in as: " + results.userId);
      },
      function(error){
            alert("Error: " + err);
      });
});

```

Det här exemplet får en token från Live Connect som är angivna tooyour Apptjänst genom att anropa hello inloggningen funktion.

###<a name="auth-getinfo"></a>Så här: hämta information om hello autentiserade användare

hello autentiseringsinformation kan hämtas från hello `/.auth/me` slutpunkten med hjälp av en HTTP anropa med något AJAX-bibliotek.  Se till att du ställer in hello `X-ZUMO-AUTH` huvud tooyour autentiseringstoken.  hello autentiseringstoken lagras i `client.currentUser.mobileServiceAuthenticationToken`.  Till exempel toouse hello fetch API:

```
var url = client.applicationUrl + '/.auth/me';
var headers = new Headers();
headers.append('X-ZUMO-AUTH', client.currentUser.mobileServiceAuthenticationToken);
fetch(url, { headers: headers })
    .then(function (data) {
        return data.json()
    }).then(function (user) {
        // hello user object contains hello claims for hello authenticated user
    });
```

Fetch är tillgängligt som [ett npm-paket](https://www.npmjs.com/package/whatwg-fetch) eller som nedladdning via en webbläsare från [CDNJS](https://cdnjs.com/libraries/fetch). Du kan också använda jQuery eller en annan AJAX API toofetch hello information.  Data tas emot som ett JSON-objekt.
