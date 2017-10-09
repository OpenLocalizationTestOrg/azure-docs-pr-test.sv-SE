---
title: aaaBind en befintlig anpassad SSL-certifikatet tooAzure Web Apps | Microsoft Docs
description: "Lär dig tootoobind anpassad SSL-certifikat tooyour webbapp, mobilappsserverdel och API-app i Azure App Service."
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: erikre
editor: 
ms.assetid: 5d5bf588-b0bb-4c6d-8840-1b609cfb5750
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 06/23/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 3503ba9f96c8ea8d18451e8bf9a9b441797ef44d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="bind-an-existing-custom-ssl-certificate-tooazure-web-apps"></a>Bind ett befintligt anpassat SSL-certifikat tooAzure Web Apps

Azure Web Apps ger en mycket skalbar, automatisk uppdatering värdtjänst. Den här kursen visar hur toobind anpassat SSL-certifikatet som du köper från en betrodd certifikatutfärdare för[Azure Web Apps](app-service-web-overview.md). När du är klar, kommer du att kunna tooaccess ditt webbprogram på hello HTTPS-slutpunkten för din anpassade DNS-domän.

![Webbprogram med anpassade SSL-certifikat](./media/app-service-web-tutorial-custom-ssl/app-with-custom-ssl.png)

I den här guiden får du lära dig hur man:

> [!div class="checklist"]
> * Uppgradera prisnivån för din app
> * Binda din anpassade SSL-certifikat tooApp Service
> * Använd HTTPS för din app
> * Automatisera SSL-certifikat-bindning med skript

> [!NOTE]
> Om du behöver tooget ett anpassat SSL-certifikat kan du skaffa en i hello Azure-portalen direkt och binda tooyour webbprogram. Följ hello [Apptjänstcertifikat kursen](web-sites-purchase-ssl-web-site.md).

## <a name="prerequisites"></a>Krav

toocomplete den här kursen:

- [Skapa en Apptjänst-app](/azure/app-service/)
- [Mappa en anpassad DNS-namnet tooyour webbapp](app-service-web-tutorial-custom-domain.md)
- Skaffa ett SSL-certifikat från en betrodd certifikatutfärdare

<a name="requirements"></a>

### <a name="requirements-for-your-ssl-certificate"></a>Krav för SSL-certifikat

toouse ett certifikat i App Service hello certifikat måste uppfylla alla hello följande krav:

* Signerats av en betrodd certifikatutfärdare
* Exporteras som en lösenordsskyddad PFX-fil
* Innehåller privat nyckel minst 2048 bitar långt
* Innehåller alla mellanliggande certifikat i certifikatkedjan hello

> [!NOTE]
> **Elliptic Curve Cryptography (ECC) certifikat** kan arbeta med App Service men omfattas inte av den här artikeln. Arbeta med din certifikatutfärdare på hello hur toocreate ECC-certifikat.

## <a name="prepare-your-web-app"></a>Förbered ditt webbprogram

toobind anpassat SSL-certifikatet tooyour webbapp din [programtjänstplanen](https://azure.microsoft.com/pricing/details/app-service/) måste vara i hello **grundläggande**, **Standard**, eller **Premium** nivå. I det här steget ska kontrollera du att ditt webbprogram i hello stöds prisnivån.

### <a name="log-in-tooazure"></a>Logga in tooAzure

Öppna hello [Azure-portalen](https://portal.azure.com).

### <a name="navigate-tooyour-web-app"></a>Navigera tooyour webbprogram

Hello vänstra menyn klickar du på **Apptjänster**, och klicka sedan på hello namnet på ditt webbprogram.

![Välj webbprogram](./media/app-service-web-tutorial-custom-ssl/select-app.png)

Du har landat i hello sidan för hantering av ditt webbprogram.  

### <a name="check-hello-pricing-tier"></a>Kontrollera hello prisnivån

Bläddra i hello vänstra navigeringsfönstret på webbsidan app toohello **inställningar** avsnittet och väljer **skala upp (apptjänstplan)**.

![Skala-menyn](./media/app-service-web-tutorial-custom-ssl/scale-up-menu.png)

Kontrollera toomake till att ditt webbprogram inte hello **lediga** eller **delade** nivå. Ditt webbprogram aktuell nivå markeras med en mörk ruta.

![Kontrollera prisnivå](./media/app-service-web-tutorial-custom-ssl/check-pricing-tier.png)

Anpassat SSL stöds inte i hello **lediga** eller **delade** nivå. Om du behöver tooscale upp åtgärderna hello i hello nästa avsnitt. I annat fall stänger hello **Välj din prisnivå** sidan och hoppa över för[överför och binda SSL-certifikat](#upload).

### <a name="scale-up-your-app-service-plan"></a>Skala upp din programtjänstplan

Välj en av hello **grundläggande**, **Standard**, eller **Premium** nivåer.

Klicka på **Välj**.

![Välj prisnivå](./media/app-service-web-tutorial-custom-ssl/choose-pricing-tier.png)

När du ser följande meddelande hello har hello skalningsåtgärden slutförts.

![Skala upp meddelande](./media/app-service-web-tutorial-custom-ssl/scale-notification.png)

<a name="upload"></a>

## <a name="bind-your-ssl-certificate"></a>Binda SSL-certifikat

Du är klar tooupload ditt webbprogram för SSL-certifikat tooyour.

### <a name="merge-intermediate-certificates"></a>Sammanfoga mellanliggande certifikat

Om din certifikatutfärdare ger flera certifikat i certifikatkedjan hello, behöver du toomerge hello certifikat i ordning. 

toodo detta, öppna varje certifikat du tog emot i en textredigerare. 

Skapa en fil för hello kopplade certifikat, kallat _mergedcertificate.crt_. Kopiera hello innehållet i varje certifikat till den här filen i en textredigerare. hello ordning certifikat bör se ut som följande mall hello:

```
-----BEGIN CERTIFICATE-----
<your Base64 encoded SSL certificate>
-----END CERTIFICATE-----

-----BEGIN CERTIFICATE-----
<Base64 encoded intermediate certificate 1>
-----END CERTIFICATE-----

-----BEGIN CERTIFICATE-----
<Base64 encoded intermediate certificate 2>
-----END CERTIFICATE-----

-----BEGIN CERTIFICATE-----
<Base64 encoded root certificate>
-----END CERTIFICATE-----
```

### <a name="export-certificate-toopfx"></a>Exportera certifikatet tooPFX

Exportera sammanfogade SSL-certifikat med hello privat nyckel som genererades certifikatbegäran med.

Om du genererade din certifikatbegäran med hjälp av OpenSSL har du skapat en fil för privat nyckel. tooexport tooPFX ditt certifikat, kör följande kommando hello. Ersätt platshållarna hello  _&lt;fil för privat nyckel >_ och  _&lt;samman certifikatfil >_.

```
openssl pkcs12 -export -out myserver.pfx -inkey <private-key-file> -in <merged-certificate-file>  
```

När du uppmanas ange ett lösenord för export. När du överför dina SSL-certifikat tooApp tjänsten senare ska du använda det här lösenordet.

Om du använder IIS eller _Certreq.exe_ toogenerate din certifikatbegäran, installera hello certifikat tooyour lokala dator, och sedan [exportera hello certifikat tooPFX](https://technet.microsoft.com/library/cc754329(v=ws.11).aspx).

### <a name="upload-your-ssl-certificate"></a>Överför SSL-certifikat

tooupload SSL-certifikat, klickar du på **SSL-certifikat** i hello vänster navigeringsfält av ditt webbprogram.

Klicka på **överför certifikat**.

I **PFX-certifikatsfilen**, Välj din PFX-fil. I **certifikatlösenord**, ange hello lösenord som du skapade när du exporterade hello PFX-filen.

Klicka på **Överför**.

![Överför certifikat](./media/app-service-web-tutorial-custom-ssl/upload-certificate.png)

När Apptjänst har överförts certifikatet, visas den i hello **SSL-certifikat** sidan.

![Certifikat som har överförts](./media/app-service-web-tutorial-custom-ssl/certificate-uploaded.png)

### <a name="bind-your-ssl-certificate"></a>Binda SSL-certifikat

I hello **SSL-bindningar** klickar du på **bindning**.

I hello **lägger till SSL-bindningen** använder hello nedrullningsbara listorna tooselect hello domain name toosecure och hello certifikat toouse.

> [!NOTE]
> Om du har överfört ett certifikat, men inte ser hello domänens namn i hello **värdnamn** listrutan Försök uppdatera hello Webbläsarsida.
>
>

I **SSL typen**, Välj om toouse  **[Server Servernamnsindikation (SNI)](http://en.wikipedia.org/wiki/Server_Name_Indication)**  eller IP-baserade SSL.

- **SNI-baserade SSL** -flera SNI-baserade SSL-bindningar kan läggas till. Det här alternativet kan flera SSL-certifikat toosecure flera domäner på hello samma IP-adress. De flesta moderna webbläsare (inklusive Internet Explorer, Chrome, Firefox och Opera) stöder SNI (supportinformation finns mer omfattande webbläsare på [Servernamnindikator](http://wikipedia.org/wiki/Server_Name_Indication)).
- **IP-baserade SSL** – en enda IP-baserade SSL-bindning kan läggas till. Det här alternativet kan endast en SSL-certifikat toosecure dedikerade offentlig IP-adress. toosecure flera domäner, du måste skydda dem med alla hello samma SSL-certifikat. Detta är hello traditionella alternativ för SSL-bindning.

Klicka på **bindning**.

![Binda SSL-certifikat](./media/app-service-web-tutorial-custom-ssl/bind-certificate.png)

När Apptjänst har överförts certifikatet, visas den i hello **SSL-bindningar** avsnitt.

![Certifikatet bundet tooweb app](./media/app-service-web-tutorial-custom-ssl/certificate-bound.png)

## <a name="remap-a-record-for-ip-ssl"></a>Mappa om en post för IP-SSL

Om du inte använder IP-baserade SSL i ditt webbprogram, hoppar du över för[Test HTTPS för den anpassade domänen](#test).

Som standard används ditt webbprogram delad offentlig IP-adress. När du binda ett certifikat med IP-baserade SSL skapar App Service en ny, dedicerade IP-adress för din webbapp.

Om du har mappat en A-poster tooyour webbapp, Uppdatera registret domän med den här nya, dedikerade IP-adress.

Ditt webbprogram **anpassad domän** sidan uppdateras med hello nya, dedikerade IP-adress. [Kopiera den här IP-adressen](app-service-web-tutorial-custom-domain.md#info), sedan [mappa om hello en post](app-service-web-tutorial-custom-domain.md#map-an-a-record) toothis nya IP-adressen.

<a name="test"></a>

## <a name="test-https"></a>Testa HTTPS

Alla som har lämnat toodo nu är toomake att HTTPS fungerar för domänen. Olika webbläsare bläddrar för`https://<your.custom.domain>` toosee som den hanterar upp ditt webbprogram.

![Portalen navigering tooAzure app](./media/app-service-web-tutorial-custom-ssl/app-with-custom-ssl.png)

> [!NOTE]
> Om ditt webbprogram ger du certifikat verifieringsfel, använder du förmodligen ett självsignerat certifikat.
>
> Om detta inte är fallet hello har du lämnat ut mellanliggande certifikat när du exporterar certifikatet toohello PFX-filen.

<a name="bkmk_enforce"></a>

## <a name="enforce-https"></a>Använda HTTPS

Apptjänst har *inte* genomdriva HTTPS, så att alla kan fortfarande komma åt ditt webbprogram med hjälp av HTTP. tooenforce HTTPS för ditt webbprogram, definiera en regel för omarbetning i hello _web.config_ -filen för ditt webbprogram. Apptjänst använder den här filen, oavsett hello språkramverket av ditt webbprogram.

> [!NOTE]
> Det finns språkspecifika omdirigering av begäranden. ASP.NET MVC kan använda hello [RequireHttps](http://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) filter i stället för hello omarbetning regeln i _web.config_.

Om du är en .NET-utvecklare kan vara du relativt bekant med den här filen. Det är i hello rot i lösningen.

Alternativt, om du utvecklar med PHP, Node.js, Python eller Java, ökar risken skapade vi den här filen för din räkning i App Service.

Ansluta tooyour webbapp FTP-slutpunkten genom att följa anvisningarna hello på [distribuera din app tooAzure App Service med FTP/S](app-service-deploy-ftp.md).

Den här filen måste finnas i _/home/site/wwwroot_. Om inte, skapar du en _web.config_ filen i mappen med följande XML hello:

```xml   
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <system.webServer>
    <rewrite>
      <rules>
        <!-- BEGIN rule ELEMENT FOR HTTPS REDIRECT -->
        <rule name="Force HTTPS" enabled="true">
          <match url="(.*)" ignoreCase="false" />
          <conditions>
            <add input="{HTTPS}" pattern="off" />
          </conditions>
          <action type="Redirect" url="https://{HTTP_HOST}/{R:1}" appendQueryString="true" redirectType="Permanent" />
        </rule>
        <!-- END rule ELEMENT FOR HTTPS REDIRECT -->
      </rules>
    </rewrite>
  </system.webServer>
</configuration>
```

För en befintlig _web.config_ fil, kopiera hello hela `<rule>` element i din _web.config_'s `configuration/system.webServer/rewrite/rules` element. Om det finns andra `<rule>` element i din _web.config_, plats hello kopieras `<rule>` elementet innan hello andra `<rule>` element.

Den här regeln returnerar en HTTP 301 (permanent omdirigering) toohello HTTPS-protokollet när hello användaren gör en HTTP-begäran tooyour webbapp. Till exempel Serverproxyn från `http://contoso.com` för`https://contoso.com`.

Mer information om hello-modulen för omarbetning av IIS-URL finns hello [URL-omskrivning om](http://www.iis.net/downloads/microsoft/url-rewrite) dokumentation.

## <a name="enforce-https-for-web-apps-on-linux"></a>Använd HTTPS för webbprogram på Linux

App-tjänsten på Linux *inte* genomdriva HTTPS, så att alla kan fortfarande komma åt ditt webbprogram med hjälp av HTTP. tooenforce HTTPS för ditt webbprogram, definiera en regel för omarbetning i hello _.htaccess_ -filen för ditt webbprogram. 

Ansluta tooyour webbapp FTP-slutpunkten genom att följa anvisningarna hello på [distribuera din app tooAzure App Service med FTP/S](app-service-deploy-ftp.md).

I _/home/site/wwwroot_, skapa en _.htaccess_ fil med hello följande kod:

```
RewriteEngine On
RewriteCond %{HTTP:X-ARR-SSL} ^$
RewriteRule ^(.*)$ https://%{HTTP_HOST}%{REQUEST_URI} [L,R=301]
```

Den här regeln returnerar en HTTP 301 (permanent omdirigering) toohello HTTPS-protokollet när hello användaren gör en HTTP-begäran tooyour webbapp. Till exempel Serverproxyn från `http://contoso.com` för`https://contoso.com`.

## <a name="automate-with-scripts"></a>Automatisera med skript

Du kan automatisera SSL-bindningar för ditt webbprogram med skript, med hjälp av hello [Azure CLI](/cli/azure/install-azure-cli) eller [Azure PowerShell](/powershell/azure/overview).

### <a name="azure-cli"></a>Azure CLI

följande kommando hello överför exporterade PFX-filen och hämtar hello tumavtryck.

```bash
thumbprint=$(az appservice web config ssl upload \
    --name <app_name> \
    --resource-group <resource_group_name> \
    --certificate-file <path_to_PFX_file> \
    --certificate-password <PFX_password> \
    --query thumbprint \
    --output tsv)
```

hello följande kommando lägger till en SNI-baserade SSL-bindning med hello tumavtryck från hello föregående kommando.

```bash
az appservice web config ssl bind \
    --name <app_name> \
    --resource-group <resource_group_name>
    --certificate-thumbprint $thumbprint \
    --ssl-type SNI \
```

### <a name="azure-powershell"></a>Azure PowerShell

hello följande kommando överför exporterade PFX-filen och lägger till en SNI-baserade SSL-bindning.

```PowerShell
New-AzureRmWebAppSSLBinding `
    -WebAppName <app_name> `
    -ResourceGroupName <resource_group_name> `
    -Name <dns_name> `
    -CertificateFilePath <path_to_PFX_file> `
    -CertificatePassword <PFX_password> `
    -SslState SniEnabled
```

## <a name="next-steps"></a>Nästa steg

I den här självstudiekursen lärde du dig att:

> [!div class="checklist"]
> * Uppgradera prisnivån för din app
> * Binda din anpassade SSL-certifikat tooApp Service
> * Använd HTTPS för din app
> * Automatisera SSL-certifikat-bindning med skript

I förväg toohello nästa självstudiekurs toolearn hur toouse Azure Content Delivery Network.

> [!div class="nextstepaction"]
> [Lägg till en innehåll innehållsleveransnätverk (CDN) tooan Azure App Service](app-service-web-tutorial-content-delivery-network.md)
