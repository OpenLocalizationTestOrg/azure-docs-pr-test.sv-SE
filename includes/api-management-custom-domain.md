## <a name="how-apim-proxy-server-responds-with-ssl-certificates-in-the-tls-handshake"></a>Hur APIM proxyserver svarar med SSL-certifikat i TLS-handskakning

### <a name="clients-calling-with-sni-header"></a>Klienter som anropar med SNI-huvud
Om kunden har en eller flera anpassade domäner som konfigurerats för Proxy, APIM kan svara på HTTPS-begäranden från anpassade domäner (t.ex, contoso.com) samt standarddomän (till exempel apim-tjänsten-name.azure-api.net). Baserat på informationen i Server Servernamnsindikation (SNI)-huvudet svarar APIM med lämplig servercertifikat.

### <a name="clients-calling-without-sni-header"></a>Klienter som anropar utan SNI-huvud
Om kunden använder en klient som inte skickar den [SNI](https://tools.ietf.org/html/rfc6066#section-3) huvudet APIM skapar svar baserat på följande logik:

* Om tjänsten har en anpassad domän som konfigurerats för Proxy, är standard-certifikatet det certifikat som utfärdades till den anpassade domänen för Proxy.
* Om tjänsten har konfigurerats flera anpassade domäner för Proxy (stöds bara i den **Premium** nivån), kunden kan välja vilket certifikat som ska vara standardcertifikatet. Ange standardcertifikatet i [defaultSslBinding](https://docs.microsoft.com/rest/api/apimanagement/apimanagementservice/createorupdate#hostnameconfiguration) egenskapen ska anges till true (”defaultSslBinding”: ”true”). Om kunden inte har angetts för egenskapen är standardcertifikatet det certifikat som utfärdats till Proxy standarddomän *.azure api.net som värd.

## <a name="support-for-putpost-request-with-large-payload"></a>Stöd för PUT/POST-begäran med stora nyttolast

APIM proxyservern stöder begäran med stora nyttolast när klientens certifikat i HTTPS (till exempel nyttolast > 40 KB). Om du vill förhindra att begäran serverns frysa kunder kan ställa in egenskapen [”negotiateClientCertificate”: ”true”](https://docs.microsoft.com/rest/api/apimanagement/ApiManagementService/CreateOrUpdate#hostnameconfiguration) på Proxy-värdnamnet. Om egenskapen är inställd på true och klienten certifikatet har begärts samtidigt SSL/TLS-anslutning innan exchange alla HTTP-begäran. Eftersom inställningen på den **Proxy Hostname** genom alla anslutningsbegäranden be om klientcertifikatet. Kunder kan konfigurera upp till 20 anpassade domäner för Proxy (stöds bara i den **Premium** nivån) och undviker du den här begränsningen.

