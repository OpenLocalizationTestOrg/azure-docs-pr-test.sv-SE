De flesta av hello tid autentiseringsfel bero på felaktig eller inkonsekvent konfigurationsinställningar. Här är några förslag till saker toocheck.

* Kontrollera att du inte missar hello **spara** knappen var som helst. Det här är ofta enkelt toodo och hello resultatet är att du ska titta på hello rätt värden på en Portalsida men de faktiskt inte har sparats i hello Azure-miljö eller Azure AD-program.
* För inställningar som konfigurerats i hello **programinställningar** bladet för hello Azure-portalen, se till att hello rätt API-app eller webbapp valdes när hello inställningar har angetts.  Kontrollera också att hello inställningar har angetts som **appinställningar** och inte **anslutningssträngar**eftersom hello format hello två avsnitt är liknande.
* För autentisering tooa JavaScript klientdelen download hello manifest igen tooverify som `oauth2AllowImplicitFlow` har ändrats för`true`.
* Kontrollera att du har använt HTTPS när du har konfigurerat URL: er:
  
  * I Projektkod
  * I CORS
  * I Azure App miljöinställningar för varje API-appen och webb-app
  * I inställningar för Azure AD-program.
    
    Observera att om du kopierar en API-app-URL från hello portal den ofta har `http://` och du har toomanually ändra också`https://`.
* Se till att ändringar koden har distribuerats. Till exempel i en lösning för flera projekt är det möjligt toochange ett projekt har code och av misstag väljer något av hello andra när du planerar toodeploy hello ändringen.
* Kontrollera att du ska tooHTTPS URL: er i din webbläsare inte HTTP-URL: er. Visual Studio skapar som standard publicera profiler med HTTP-URL: er och som är vad öppnas i hello webbläsare när du distribuerar ett projekt.
* Kontrollera att CORS är korrekt konfigurerad på hello API-app som hello JavaScript-koden anropar för autentisering tooa JavaScript klientdelen. Om du tvekar om hello problem är CORS-relaterade försök ”*” som hello tillåtna ursprung URL. 
* Öppna din webbläsare verktyg Utvecklarkonsolen fliken tooget mer felinformation för klientdelen för JavaScript och undersöka HTTP-förfrågningar på hello nätverk. Konsolen felmeddelanden kan emellertid vilseledande. Om du får ett meddelande om ett fel för CORS kanske hello verkliga problemet autentisering. Du kan kontrollera om så är fallet för hello genom att köra hello app med autentisering inaktiveras tillfälligt tillfälligt.
* För en .NET-API-app, kontrollerar du att du får så mycket information i felmeddelanden som möjligt genom att ange [customErrors mode tooOff](../articles/app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#remoteview).
* En .NET-API-app, starta en [remote felsökningssessionen](../articles/app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug), och undersöka hello värdena för variabler som hello som skickas toocode som använder ADAL tooacquire en ägartoken eller kod som kontrollerar fordringar hello förväntades service principal-ID. Observera att din kod kan hämta konfigurationsvärden från olika källor, så det är möjligt toofind överraskningar det här sättet. Till exempel om uppgifterna `ida:ClientId` som `ida:ClientID` när du konfigurerar Azure Apptjänst-miljöinställningar hello kod kan hämta hello `ida:ClientId` värde som är ute efter från hello Web.config-fil, ignorerar hello Azure App Service-inställningen. 
* Om saker inte fungerar i ett normalt fönster i Internet Explorer, kan en befintlig inloggning hindra; InPrivate och försök Chrome eller Firefox.

