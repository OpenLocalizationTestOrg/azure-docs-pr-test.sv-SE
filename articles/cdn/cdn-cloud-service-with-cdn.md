---
title: "aaaIntegrate en Azure-molntjänst med Azure CDN | Microsoft Docs"
description: "Lär dig hur toodeploy en molnbaserad tjänst som hanterar innehåll från en integrerad Azure CDN-slutpunkt"
services: cdn, cloud-services
documentationcenter: .net
author: zhangmanling
manager: erikre
editor: tysonn
ms.assetid: b3c0108f-9ec5-43a8-8fd0-40eafbd32637
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: f20d60b0b5edc133adf06d010633a15f62e2b8de
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="intro"></a>Integrera en tjänst i molnet med Azure CDN
En tjänst i molnet kan integreras med Azure CDN betjänar allt innehåll från plats hello cloud service. Den här metoden ger du hello följande fördelar:

* Enkelt distribuera och uppdatera avbildningar, skript och matmallar i din molntjänst projekt kataloger
* Lätt att uppgradera hello NuGet-paket i din molntjänst, t.ex jQuery eller Bootstrap versioner
* Hantera ditt webbprogram och din CDN-hanterat innehåll från hello samma Visual Studio-gränssnittet
* Arbetsflöde för enhetlig distribution för webbprogrammet och din CDN-hanterat innehåll
* Integrera ASP.NET paketering och minification med Azure CDN

## <a name="what-you-will-learn"></a>Vad får du lära dig
I den här kursen får du lära dig hur du:

* [Integrera Azure CDN-slutpunkten med Molntjänsten och hantera statiskt innehåll i webbsidor från Azure CDN](#deploy)
* [Konfigurera inställningar för cachelagring för statiskt innehåll i Molntjänsten](#caching)
* [Hantera innehåll från domänkontrollanten åtgärder via Azure CDN](#controller)
* [Fungera tillsammans och minified innehåll via Azure CDN samtidigt som hello skript upplevelse i Visual Studio-felsökning](#bundling)
* [Konfigurera reserv skripten och CSS när Azure CDN är offline](#fallback)

## <a name="what-you-will-build"></a>Vad du ska skapa
Du distribuerar en cloud service-webbroll med hello standard ASP.NET MVC-mallen, lägga till kod tooserve innehåll från en integrerad Azure CDN, till exempel en bild, domänkontrollant åtgärd resultat och hello standard JavaScript och CSS-filer, och även skriva koden tooconfigure hello fallback mekanism för paket som hanteras i hello händelse som hello CDN är offline.

## <a name="what-you-will-need"></a>Vad du behöver
Den här självstudiekursen har hello följande krav:

* En aktiv [Microsoft Azure-konto](/account/)
* Visual Studio 2015 med [Azure SDK](http://go.microsoft.com/fwlink/?linkid=518003&clcid=0x409)

> [!NOTE]
> Du behöver ett Azure-konto toocomplete den här kursen:
> 
> * Du kan [öppna ett Azure-konto gratis](https://azure.microsoft.com/pricing/free-trial/) – du får kredit du kan använda tootry ut betald Azure-tjänster och även när de används upp du kan behålla hello kontot och använda kostnadsfria Azure-tjänster, till exempel Websites.
> * Du kan [aktivera MSDN-prenumerantförmåner](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) -din MSDN-prenumeration ger dig krediter varje månad som du kan använda för Azure-betaltjänster.
> 
> 

<a name="deploy"></a>

## <a name="deploy-a-cloud-service"></a>Distribuera en tjänst i molnet
I det här avsnittet ska du distribuera hello standard mall för ASP.NET MVC-program i Visual Studio 2015 tooa cloud service-webbroll och integrera den med en ny CDN-slutpunkt. Följ anvisningarna nedan för hello:

1. I Visual Studio 2015, skapa en ny Azure-molnet hello menyraden genom att gå för**Arkiv > Nytt > Projekt > molntjänster > Azure Cloud Service**. Ge det ett namn och klicka på **OK**.
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-1-new-project.PNG)
2. Välj **ASP.NET Web Role** och klicka på hello  **>**  knappen. Klicka på OK.
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-2-select-role.PNG)
3. Välj **MVC** och på **OK**.
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-3-mvc-template.PNG)
4. Nu kan publicera den här Web rollen tooan Azure-molntjänst. Högerklicka på hello molntjänstprojektet och välj **publicera**.
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-4-publish-a.png)
5. Om du ännu inte signerats i Microsoft Azure klickar du på hello **Lägg till ett konto...**  listrutan och klicka på hello **Lägg till ett konto** menyalternativet.
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-5-publish-signin.png)
6. Logga in med hello Microsoft-konto som du använde tooactivate ditt Azure-konto i hello inloggningssida visas.
7. När du har loggat in, klickar du på **nästa**.
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-6-publish-signedin.png)
8. Förutsatt att du inte har skapat ett cloud service eller storage-konto kan du skapa både i Visual Studio. I hello **skapa Molntjänsten och kontot** dialogrutan, hello önskade tjänstnamn och välj hello önskad region. Klicka på **Skapa**.
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-7-publish-createserviceandstorage.png)
9. I hello publicera inställningssidan, verifiera hello konfigurationen och klickar på **publicera**.
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-8-publish-finalize.png)
   
   > [!NOTE]
   > hello publiceringsprocessen för molntjänster tar lång tid. hello aktivera webbdistribution för alla roller alternativet kan göra felsökning Molntjänsten mycket snabbare genom att tillhandahålla uppdateringar för snabb (men tillfälliga) tooyour Web-roller. Mer information om det här alternativet, se [publicering av en tjänst i molnet med hjälp av hello Azure verktyg](http://msdn.microsoft.com/library/ff683672.aspx).
   > 
   > 
   
    När hello **Microsoft Azure-aktivitetsloggen** ser att Publiceringsstatus **slutförd**, skapar du en CDN-slutpunkt som är integrerad med den här Molntjänsten.
   
   > [!WARNING]
   > Om hello distribuerad tjänst i molnet visas ett felmeddelande när du har publicerat, är förmodligen eftersom hello molntjänst som du har distribuerat en [gästen OS som inte innehåller .NET 4.5.2](../cloud-services/cloud-services-guestos-update-matrix.md#news-updates).  Du kan undvika det här problemet genom att [distribuerar .NET 4.5.2 som en startåtgärd](../cloud-services/cloud-services-dotnet-install-dotnet.md).
   > 
   > 

## <a name="create-a-new-cdn-profile"></a>Skapa en ny CDN-profil
En CDN-profil är en samling CDN-slutpunkter.  Varje profil innehåller en eller flera CDN-slutpunkter.  Du kanske vill toouse flera profiler tooorganize CDN-slutpunkter med internet-domän, webbprogram eller andra kriterier.

> [!TIP]
> Om du redan har en CDN-profil som du vill toouse för den här kursen kan fortsätta för[skapa en ny CDN-slutpunkt](#create-a-new-cdn-endpoint).
> 
> 

[!INCLUDE [cdn-create-profile](../../includes/cdn-create-profile.md)]

## <a name="create-a-new-cdn-endpoint"></a>Skapa en ny CDN-slutpunkt
**toocreate en ny CDN-slutpunkt för ditt lagringskonto**

1. I hello [Azure-hanteringsportalen](https://portal.azure.com), navigera tooyour CDN-profilen.  Du kanske har Fäst det toohello instrumentpanelen i hello föregående steg.  Om du inte hittar du den genom att klicka på **Bläddra**, sedan **CDN profiler**, och klicka på hello profilen du planerar tooadd till slutpunkten.
   
    hello CDN-profilbladet visas.
   
    ![CDN-profil][cdn-profile-settings]
2. Klicka på hello **Lägg till slutpunkt** knappen.
   
    ![Knappen Lägg till slutpunkt][cdn-new-endpoint-button]
   
    Hej **lägga till en slutpunkt** bladet visas.
   
    ![Bladet Lägg till slutpunkt][cdn-add-endpoint]
3. Ange ett **namn** för den här CDN-slutpunkten.  Det här namnet kommer att använda tooaccess cachelagrade resurserna på hello domän `<EndpointName>.azureedge.net`.
4. I hello **typ av ursprung** listrutan *Molntjänsten*.  
5. I hello **ursprungsvärdnamn** listrutan, Välj din molntjänst.
6. Lämna hello standardvärden för **ursprungssökväg**, **ursprungsvärdadress**, och **protokollet ursprung port**.  Du måste ange minst ett protokoll (HTTP eller HTTPS).
7. Klicka på hello **Lägg till** knappen toocreate hello ny slutpunkt.
8. När hello slutpunkten har skapats visas den i en lista över slutpunkter för hello-profilen. hello listvyn visar hello URL toouse tooaccess cachelagrat innehåll, samt hello ursprungsdomän.
   
    ![CDN-slutpunkt][cdn-endpoint-success]
   
   > [!NOTE]
   > hello endpoint blir omedelbart inte tillgängliga för användning.  Det kan ta upp too90 minuter för hello registrering toopropagate via hello CDN nätverk. Användare som försöker toouse hello CDN domännamn direkt få statuskod 404 tills hello innehållet är tillgängligt via hello CDN.
   > 
   > 

## <a name="test-hello-cdn-endpoint"></a>Testa hello CDN-slutpunkten
När hello Publiceringsstatus är **slutförd**, öppna ett webbläsarfönster och navigera för**http://<cdnName>*.azureedge.net/Content/bootstrap.css**. URL: en är i min installationsprogrammet:

    http://camservice.azureedge.net/Content/bootstrap.css

Som motsvarar toohello följande ursprung URL vid hello CDN-slutpunkten:

    http://camcdnservice.cloudapp.net/Content/bootstrap.css

När du navigerar för**http://*&lt;cdnName >*.azureedge.net/Content/bootstrap.css**, beroende på din webbläsare kommer du att ange toodownload eller öppna hello bootstrap.css som Kom från ditt publicerade webbprogram.

![](media/cdn-cloud-service-with-cdn/cdn-1-browser-access.PNG)

På samma sätt kan du komma åt valfri offentligt tillgänglig URL på  **http://*&lt;serviceName >*.cloudapp.net/** direkt från din CDN-slutpunkten. Exempel:

* En JS-fil från hello/Script sökväg
* Varje innehållsfil från hello /Content sökväg
* En domänkontrollant/åtgärd
* Om hello frågesträngen är aktiverad på en URL med frågesträngar din CDN-slutpunkten

Med hello ovan konfiguration du i själva verket värd hello hela molnbaserad tjänst från  **http://*&lt;cdnName >*.azureedge.net/**. Om jag navigerar för**http://camservice.azureedge.net/ ** jag hello åtgärd resultatet från Home-Index.

![](media/cdn-cloud-service-with-cdn/cdn-2-home-page.PNG)

Detta innebär inte, men att det alltid är en bra idé tooserve en hel molntjänst via Azure CDN. 

En CDN med statiska leveransoptimering inte nödvändigtvis snabba upp överföringen av dynamisk tillgångar som inte är avsedda toobe cachelagras eller uppdateras väldigt ofta eftersom hello CDN måste hämta en ny version av hello tillgång från hello ursprungsservern så ofta. I det här scenariot kan du aktivera [dynamiska plats Acceleration](cdn-dynamic-site-acceleration.md) optimering (DSA) på din CDN-slutpunkt som använder olika tekniker toospeed upp överföringen av ej Cacheable ställs dynamiska tillgångar. 

Om du har en plats med en blandning av statiska och dynamiska innehåll du tooserve dina statiskt innehåll från CDN med en statisk optimering typ (till exempel Internet leverans) och tooserve dynamiskt innehåll direkt från hello ursprungsservern eller via en CDN slutpunkten med DSA optimering aktiverat-fall till fall. toothat end du redan har sett hur tooaccess enskilda innehåll filer från hello CDN-slutpunkten. Jag visar hur tooserve en specifik domänkontrollant åtgärd via en specifik CDN-slutpunkt i Hantera innehåll från domänkontrollanten åtgärder via Azure CDN.

hello alternativ är toodetermine som innehåll tooserve från Azure CDN för fall till fall i Molntjänsten. toothat end du redan har sett hur tooaccess enskilda innehåll filer från hello CDN-slutpunkten. Jag får du lära dig hur tooserve en specifik domänkontrollant åtgärd via hello CDN-slutpunkten i [innehåll från domänkontrollanten åtgärder via Azure CDN](#controller).

<a name="caching"></a>

## <a name="configure-caching-options-for-static-files-in-your-cloud-service"></a>Konfigurera cachelagringsalternativ för statiska filer i Molntjänsten
Du kan ange hur du vill att statiskt innehåll toobe som cachelagrats i hello CDN-slutpunkten med Azure CDN-integration i Molntjänsten. toodo detta, öppna *Web.config* från din webbroll projekt (t.ex. WebRole1) och Lägg till en `<staticContent>` element för`<system.webServer>`. hello XML nedan konfigurerar hello cache tooexpire i tre dagar.  

    <system.webServer>
      <staticContent>
        <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="3.00:00:00"/>
      </staticContent>
      ...
    </system.webServer>

När du gör detta kommer alla statiska filer i din molntjänst Se hello samma regel i CDN-cachen. För mer detaljerad kontroll över inställningar för cachelagring för att lägga till en *Web.config* till en mapp och Lägg till dina inställningar. Till exempel lägga till en *Web.config* filen toohello *\Content* mapp och Ersätt hello innehållet med följande XML hello:

    <?xml version="1.0"?>
    <configuration>
      <system.webServer>
        <staticContent>
          <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="15.00:00:00"/>
        </staticContent>
      </system.webServer>
    </configuration>

Den här inställningen gör att alla statiska filer från hello *\Content* mappen toobe som cachelagrats för 15 dagar.

Mer information om hur tooconfigure hello `<clientCache>` element, se [klientcachen &lt;clientCache >](http://www.iis.net/configreference/system.webserver/staticcontent/clientcache).

I [innehåll från domänkontrollanten åtgärder via Azure CDN](#controller), jag visas också hur du kan konfigurera inställningar för cachelagring för domänkontrollant åtgärden resulterar i hello CDN cache.

<a name="controller"></a>

## <a name="serve-content-from-controller-actions-through-azure-cdn"></a>Hantera innehåll från domänkontrollanten åtgärder via Azure CDN
När du integrerar en cloud service-webbroll med Azure CDN är relativt enkelt tooserve innehåll från domänkontrollanten åtgärder via hello Azure CDN. Tjänsten direkt via Azure CDN (som visas ovan) än betjänar ditt moln [Maarten Balliauw](https://twitter.com/maartenballiauw) visas hur toodo den med ett roligt MemeGenerator domänkontrollant i [minskar svarstiden på hello web med hello Azure CDN ](http://channel9.msdn.com/events/TechDays/Techdays-2014-the-Netherlands/Reducing-latency-on-the-web-with-the-Windows-Azure-CDN). Jag kommer att återskapa den här.

Anta att i Molntjänsten du toogenerate memes baserat på ett barn Chuck Norris bild (foto av [Alan Light](http://www.flickr.com/photos/alan-light/218493788/)) så här:

![](media/cdn-cloud-service-with-cdn/cdn-5-memegenerator.PNG)

Du har en enkel `Index` åtgärd som kunder hello toospecify hello superlativ hello bild sedan genererar hello meme när de efter toohello åtgärd. Eftersom det är Chuck Norris förväntar du dig den här sidan toobecome wildly populära globalt. Detta är ett bra exempel på betjänar delvis dynamiskt innehåll med Azure CDN.

Följ hello stegen ovan toosetup åtgärden domänkontrollant:

1. I hello *\Controllers* mapp, skapa en ny .cs-fil som kallas *MemeGeneratorController.cs* och Ersätt hello innehåll med hello följande kod. Vara säker på att tooreplace hello markerade delen med CDN-namn.  
   
        using System;
        using System.Collections.Generic;
        using System.Diagnostics;
        using System.Drawing;
        using System.IO;
        using System.Net;
        using System.Web.Hosting;
        using System.Web.Mvc;
        using System.Web.UI;
   
        namespace WebRole1.Controllers
        {
            public class MemeGeneratorController : Controller
            {
                static readonly Dictionary<string, Tuple<string ,string>> Memes = new Dictionary<string, Tuple<string, string>>();
   
                public ActionResult Index()
                {
                    return View();
                }
   
                [HttpPost, ActionName("Index")]
                public ActionResult Index_Post(string top, string bottom)
                {
                    var identifier = Guid.NewGuid().ToString();
                    if (!Memes.ContainsKey(identifier))
                    {
                        Memes.Add(identifier, new Tuple<string, string>(top, bottom));
                    }
   
                    return Content("<a href=\"" + Url.Action("Show", new {id = identifier}) + "\">here's your meme</a>");
                }
   
                [OutputCache(VaryByParam = "*", Duration = 1, Location = OutputCacheLocation.Downstream)]
                public ActionResult Show(string id)
                {
                    Tuple<string, string> data = null;
                    if (!Memes.TryGetValue(id, out data))
                    {
                        return new HttpStatusCodeResult(HttpStatusCode.NotFound);
                    }
   
                    if (Debugger.IsAttached) // Preserve hello debug experience
                    {
                        return Redirect(string.Format("/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
                    }
                    else // Get content from Azure CDN
                    {
                        return Redirect(string.Format("http://<yourCdnName>.azureedge.net/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
                    }
                }
   
                [OutputCache(VaryByParam = "*", Duration = 3600, Location = OutputCacheLocation.Downstream)]
                public ActionResult Generate(string top, string bottom)
                {
                    string imageFilePath = HostingEnvironment.MapPath("~/Content/chuck.bmp");
                    Bitmap bitmap = (Bitmap)Image.FromFile(imageFilePath);
   
                    using (Graphics graphics = Graphics.FromImage(bitmap))
                    {
                        SizeF size = new SizeF();
                        using (Font arialFont = FindBestFitFont(bitmap, graphics, top.ToUpperInvariant(), new Font("Arial Narrow", 100), out size))
                        {
                            graphics.DrawString(top.ToUpperInvariant(), arialFont, Brushes.White, new PointF(((bitmap.Width - size.Width) / 2), 10f));
                        }
                        using (Font arialFont = FindBestFitFont(bitmap, graphics, bottom.ToUpperInvariant(), new Font("Arial Narrow", 100), out size))
                        {
                            graphics.DrawString(bottom.ToUpperInvariant(), arialFont, Brushes.White, new PointF(((bitmap.Width - size.Width) / 2), bitmap.Height - 10f - arialFont.Height));
                        }
                    }
   
                    MemoryStream ms = new MemoryStream();
                    bitmap.Save(ms, System.Drawing.Imaging.ImageFormat.Png);
                    return File(ms.ToArray(), "image/png");
                }
   
                private Font FindBestFitFont(Image i, Graphics g, String text, Font font, out SizeF size)
                {
                    // Compute actual size, shrink if needed
                    while (true)
                    {
                        size = g.MeasureString(text, font);
   
                        // It fits, back out
                        if (size.Height < i.Height &&
                             size.Width < i.Width) { return font; }
   
                        // Try a smaller font (90% of old size)
                        Font oldFont = font;
                        font = new Font(font.Name, (float)(font.Size * .9), font.Style);
                        oldFont.Dispose();
                    }
                }
            }
        }
2. Högerklicka i hello standard `Index()` och väljer **Lägg till vy**.
   
    ![](media/cdn-cloud-service-with-cdn/cdn-6-addview.PNG)
3. Godkänn hello inställningarna nedan och klicka på **Lägg till**.
   
   ![](media/cdn-cloud-service-with-cdn/cdn-7-configureview.PNG)
4. Öppna ny hello *Views\MemeGenerator\Index.cshtml* och Ersätt hello innehållet med följande enkelt HTML-format för att skicka hello superlativ hello:
   
        <h2>Meme Generator</h2>
   
        <form action="" method="post">
            <input type="text" name="top" placeholder="Enter top text here" />
            <br />
            <input type="text" name="bottom" placeholder="Enter bottom text here" />
            <br />
            <input class="btn" type="submit" value="Generate meme" />
        </form>
5. Publicera hello tjänst i molnet igen och gå för**http://*&lt;serviceName >*.cloudapp.net/MemeGenerator/Index** i webbläsaren.

När du skickar hello formulärvärden för`/MemeGenerator/Index`, hello `Index_Post` åtgärdsmetod returnerar en länk toohello `Show` åtgärdsmetod med hello respektive inkommande identifierare. När du klickar på länken hello nå hello följande kod:  

    [OutputCache(VaryByParam = "*", Duration = 1, Location = OutputCacheLocation.Downstream)]
    public ActionResult Show(string id)
    {
        Tuple<string, string> data = null;
        if (!Memes.TryGetValue(id, out data))
        {
            return new HttpStatusCodeResult(HttpStatusCode.NotFound);
        }

        if (Debugger.IsAttached) // Preserve hello debug experience
        {
            return Redirect(string.Format("/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
        }
        else // Get content from Azure CDN
        {
            return Redirect(string.Format("http://<yourCDNName>.azureedge.net/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
        }
    }

Om din lokala felsökare, får du hello reguljära debug-upplevelse med en lokal omdirigering. Om den körs i hello Molntjänsten ska den omdirigera till:

    http://<yourCDNName>.azureedge.net/MemeGenerator/Generate?top=<formInput>&bottom=<formInput>

Som motsvarar toohello följande ursprung URL vid CDN-slutpunkten:

    http://<youCloudServiceName>.cloudapp.net/MemeGenerator/Generate?top=<formInput>&bottom=<formInput>


Du kan sedan använda hello `OutputCacheAttribute` -attributet på hello `Generate` metoden toospecify hur hello åtgärd resultatet ska cachelagras, som hanterar Azure CDN. hello koden nedan ange en giltighetstid för cache 1 timme (3 600 sekunder).

    [OutputCache(VaryByParam = "*", Duration = 3600, Location = OutputCacheLocation.Downstream)]

På samma sätt kan du hantera innehåll från något domänkontrollant i Molntjänsten via din Azure CDN med alternativet för cachelagring av hello önskad.

I nästa avsnitt om hello visar jag hur tooserve hello paketerade och minified skript och CSS via Azure CDN.

<a name="bundling"></a>

## <a name="integrate-aspnet-bundling-and-minification-with-azure-cdn"></a>Integrera ASP.NET paketering och minification med Azure CDN
Skript och CSS matmallar ändras sällan och är särskilda kandidater för hello Azure CDN cache. Betjänar hello hela webbroll via Azure CDN är hello enklaste sättet toointegrate paketering och minification med Azure CDN. Men du kanske inte vill toodo detta, visas I hur toodo den samtidigt som hello önskad develper upplevelse av ASP.NET paketering och minification, som:

* Bra debug problem
* Effektiv distribution
* Omedelbar uppdateringar tooclients för skriptet/CSS versionsuppgraderingar
* Fallback mekanism när CDN-slutpunkten misslyckas
* Minimera kodändring

I hello **WebRole1** projekt som du skapade i [integrera Azure CDN-slutpunkten med din Azure-webbplatsen och hantera statiskt innehåll i webbsidor från Azure CDN](#deploy)öppnar *App_Start\ BundleConfig.cs* och ta en titt på hello `bundles.Add()` metodanrop.

    public static void RegisterBundles(BundleCollection bundles)
    {
        bundles.Add(new ScriptBundle("~/bundles/jquery").Include(
                    "~/Scripts/jquery-{version}.js"));
        ...
    }

Hej först `bundles.Add()` instruktionen lägger till ett skript-paket på hello virtuella katalogen `~/bundles/jquery`. Öppna *Views\Shared\_Layout.cshtml* toosee hur hello paket skripttypen återges. Du ska kunna toofind hello följande Razor kodrad:

    @Scripts.Render("~/bundles/jquery")

När den här Razor koden körs i hello Azure-webbroll blir en `<script>` taggen för hello skript paket liknande toohello följande:

    <script src="/bundles/jquery?v=FVs3ACwOLIVInrAl5sdzR2jrCDmVOWFbZMY6g6Q0ulE1"></script>

Men när det körs i Visual Studio genom att skriva `F5`, den kommer att visas varje skriptfilen i hello paket individuellt (i hello fallet ovan endast en skriptfil finns i hello-paket):

    <script src="/Scripts/jquery-1.10.2.js"></script>

Detta gör att du toodebug hello JavaScript-kod i din utvecklingsmiljö medan vilket minskar antalet samtidiga klientanslutningar (paketering) och förbättra filen hämta prestanda (minification) i produktion. Det är en bra funktionen toopreserve med Azure CDN-integration. Dessutom eftersom hello renderas paketet innehåller redan en automatiskt genererad versionssträng måste du vill tooreplicate funktioner hello så när du uppdaterar din jQuery version via NuGet, den kan uppdateras på klientsidan för hello så snart möjligt.

Följ hello steg nedan toointegration ASP.NET paketering och minification med CDN-slutpunkten.

1. Tillbaka i *App_Start\BundleConfig.cs*, ändra hello `bundles.Add()` metoder toouse en annan [paket konstruktorn](http://msdn.microsoft.com/library/jj646464.aspx), som anger en CDN-adress. toodo detta, Ersätt hello `RegisterBundles` metoddefinition med hello följande kod:  
   
        public static void RegisterBundles(BundleCollection bundles)
        {
            bundles.UseCdn = true;
            var version = System.Reflection.Assembly.GetAssembly(typeof(Controllers.HomeController))
                .GetName().Version.ToString();
            var cdnUrl = "http://<yourCDNName>.azureedge.net/{0}?v=" + version;
   
            bundles.Add(new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "bundles/jquery")).Include(
                        "~/Scripts/jquery-{version}.js"));
   
            bundles.Add(new ScriptBundle("~/bundles/jqueryval", string.Format(cdnUrl, "bundles/jqueryval")).Include(
                        "~/Scripts/jquery.validate*"));
   
            // Use hello development version of Modernizr toodevelop with and learn from. Then, when you're
            // ready for production, use hello build tool at http://modernizr.com toopick only hello tests you need.
            bundles.Add(new ScriptBundle("~/bundles/modernizr", string.Format(cdnUrl, "bundles/modernizer")).Include(
                        "~/Scripts/modernizr-*"));
   
            bundles.Add(new ScriptBundle("~/bundles/bootstrap", string.Format(cdnUrl, "bundles/bootstrap")).Include(
                        "~/Scripts/bootstrap.js",
                        "~/Scripts/respond.js"));
   
            bundles.Add(new StyleBundle("~/Content/css", string.Format(cdnUrl, "Content/css")).Include(
                        "~/Content/bootstrap.css",
                        "~/Content/site.css"));
        }
   
    Vara säker på att tooreplace `<yourCDNName>` med namnet hello Azure CDN.
   
    Inställningen i klartext, `bundles.UseCdn = true` och lägga till ett noggrant utformad CDN URL tooeach paket. Till exempel hello första konstruktorn i hello kod:
   
        new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "bundles/jquery"))
   
    är hello samma som:
   
        new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "http://<yourCDNName>.azureedge.net/bundles/jquery?v=<W.X.Y.Z>"))
   
    Den här konstruktorn instruerar ASP.NET paketering och minification toorender enskilda filer när felsöks lokalt, men Använd hello angetts CDN adress tooaccess hello skript i fråga. Observera dock två viktiga egenskaper med den här noggrant utformad CDN-URL:
   
   * hello ursprung för denna CDN-URL är `http://<yourCloudService>.cloudapp.net/bundles/jquery?v=<W.X.Y.Z>`, vilket är faktiskt hello virtuell katalog för hello skript paket i Molntjänsten.
   * Eftersom du använder CDN konstruktorn innehåller hello CDN skripttypen för hello paket inte längre hello genereras automatiskt Versionsträngen i hello renderas URL. Manuellt måste du generera en unik versionssträng varje gång hello skript paket är ändrade tooforce en cache missar på Azure CDN. AT hello samma tid unik versionssträngen måste vara konstant via hello livslängd hello distribution toomaximize cacheträffar vid Azure CDN när hello-paket har distribuerats.
   * Hej frågesträngen v = < W.X.Y.Z > hämtar från *Properties\AssemblyInfo.cs* i din webbrollsprojektet. Du kan ha ett arbetsflöde för distribution som innehåller ökar hello Sammansättningsversionen varje gång du publicerar tooAzure. Eller, du kan bara ändra *Properties\AssemblyInfo.cs* i ditt projekt tooautomatically ökning hello versionssträng varje gång du skapar med hjälp av hello jokertecknet ' *'. Exempel:
     
        [sammansättningen: AssemblyVersion("1.0.0.*")]
     
     Alla andra strategi toostreamline Generera en unik sträng för hello livslängden för en distribution fungerar här.
2. Publicera hello cloud service och åtkomst hello startsidan.
3. Visa hello HTML-koden för hello-sidan. Du ska kunna toosee hello CDN-URL som renderas med en unik versionssträng varje gång du publicera ändringar tooyour Molntjänsten. Exempel:  
   
        ...
   
        <link href="http://camservice.azureedge.net/Content/css?v=1.0.0.25449" rel="stylesheet"/>
   
        <script src="http://camservice.azureedge.net/bundles/modernizer?v=1.0.0.25449"></script>
   
        ...
   
        <script src="http://camservice.azureedge.net/bundles/jquery?v=1.0.0.25449"></script>
   
        <script src="http://camservice.azureedge.net/bundles/bootstrap?v=1.0.0.25449"></script>
   
        ...
4. I Visual Studio debug hello Molntjänsten i Visual Studio genom att skriva `F5`.,
5. Visa hello HTML-koden för hello-sidan. Du kommer fortfarande att se varje skriptfilen individuellt återges så att du får en konsekvent debug upplevelse i Visual Studio.  
   
        ...
   
            <link href="/Content/bootstrap.css" rel="stylesheet"/>
        <link href="/Content/site.css" rel="stylesheet"/>
   
            <script src="/Scripts/modernizr-2.6.2.js"></script>
   
        ...
   
            <script src="/Scripts/jquery-1.10.2.js"></script>
   
            <script src="/Scripts/bootstrap.js"></script>
        <script src="/Scripts/respond.js"></script>
   
        ...   

<a name="fallback"></a>

## <a name="fallback-mechanism-for-cdn-urls"></a>Fallback mekanism för CDN-URL: er
När Azure CDN-slutpunkten misslyckas av någon anledning, vill du din webbsida toobe smarta tillräckligt med tooaccess webbservern ursprung som hello återställningsalternativ för inläsning av JavaScript eller starttjänsten. Det är tillräckligt allvarligt toolose bilder på din webbplats på grund av tooCDN inte finns, men mycket allvarligare toolose avgörande funktionalitet som tillhandahålls av skripten och formatmallar.

Hej [paket](http://msdn.microsoft.com/library/system.web.optimization.bundle.aspx) klassen innehåller en egenskap som kallas [CdnFallbackExpression](http://msdn.microsoft.com/library/system.web.optimization.bundle.cdnfallbackexpression.aspx) som du kan använda tooconfigure hello återställningsplats mekanism för CDN-fel. toouse den här egenskapen följer hello stegen nedan:

1. Öppna i din webbrollsprojektet *App_Start\BundleConfig.cs*, där du lade till en CDN-URL i varje [paket konstruktorn](http://msdn.microsoft.com/library/jj646464.aspx), och se hello följande markerade ändras tooadd återställningsplats mekanism toohello standard-paket:  
   
        public static void RegisterBundles(BundleCollection bundles)
        {
            var version = System.Reflection.Assembly.GetAssembly(typeof(BundleConfig))
                .GetName().Version.ToString();
            var cdnUrl = "http://cdnurl.azureedge.net/.../{0}?" + version;
            bundles.UseCdn = true;
   
            bundles.Add(new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "bundles/jquery"))
                        { CdnFallbackExpression = "window.jquery" }
                        .Include("~/Scripts/jquery-{version}.js"));
   
            bundles.Add(new ScriptBundle("~/bundles/jqueryval", string.Format(cdnUrl, "bundles/jqueryval"))
                        { CdnFallbackExpression = "$.validator" }
                        .Include("~/Scripts/jquery.validate*"));
   
            // Use hello development version of Modernizr toodevelop with and learn from. Then, when you&#39;re
            // ready for production, use hello build tool at http://modernizr.com toopick only hello tests you need.
            bundles.Add(new ScriptBundle("~/bundles/modernizr", string.Format(cdnUrl, "bundles/modernizer"))
                        { CdnFallbackExpression = "window.Modernizr" }
                        .Include("~/Scripts/modernizr-*"));
   
            bundles.Add(new ScriptBundle("~/bundles/bootstrap", string.Format(cdnUrl, "bundles/bootstrap"))     
                        { CdnFallbackExpression = "$.fn.modal" }
                        .Include(
                                  "~/Scripts/bootstrap.js",
                                  "~/Scripts/respond.js"));
   
            bundles.Add(new StyleBundle("~/Content/css", string.Format(cdnUrl, "Content/css")).Include(
                        "~/Content/bootstrap.css",
                        "~/Content/site.css"));
        }
   
    När `CdnFallbackExpression` är null, skript är injekteras i hello HTML tootest om hello-paket har lästs in och, om inte, kommer du åt hello paket direkt från hello ursprung webbservern. Den här egenskapen måste toobe tooa JavaScript mängduttryck som testar om hello respektive CDN-paket har lästs in korrekt. hello uttryck behövs tootest varje paket skiljer sig bl.a toohello innehåll. För hello standard paket ovan:
   
   * `window.jquery`har definierats i jquery-{version} .js
   * `$.validator`har definierats i jquery.validate.js
   * `window.Modernizr`har definierats i modernizer-{version} .js
   * `$.fn.modal`har definierats i bootstrap.js
     
     Du kanske har lagt märke till att jag inte har angett CdnFallbackExpression för hello `~/Cointent/css` paket. Detta beror på att det finns för närvarande en [programfel i System.Web.Optimization](https://aspnetoptimization.codeplex.com/workitem/104) som lägger in en `<script>` taggen för hello återställningsplats CSS i stället för hello förväntat `<link>` tagg.
     
     Det är emellertid en bra [Style paket reserv](https://github.com/EmberConsultingGroup/StyleBundleFallback) erbjuds av [Ember samråd grupp](https://github.com/EmberConsultingGroup).
2. toouse hello lösning för CSS, skapa en ny .cs-fil i din webbrollsprojektet *App_Start* mapp med namnet *StyleBundleExtensions.cs*, och Ersätt innehållet med hello [kod från GitHub](https://github.com/EmberConsultingGroup/StyleBundleFallback/blob/master/Website/App_Start/StyleBundleExtensions.cs).
3. I *App_Start\StyleFundleExtensions.cs*, byta namn på hello namnområde tooyour Web rollnamn (t.ex. **WebRole1**).
4. Gå tillbaka för`App_Start\BundleConfig.cs` och ändra hello senast `bundles.Add` instruktion med hello följande markerade koden:  
   
        bundles.Add(new StyleBundle("~/Content/css", string.Format(cdnUrl, "Content/css"))
            <mark>.IncludeFallback("~/Content/css", "sr-only", "width", "1px")</mark>
            .Include(
                  "~/Content/bootstrap.css",
                  "~/Content/site.css"));
   
    Den här nya tilläggsmetoden använder hello samma uppfattning tooinject skript i hello HTML toocheck hello DOM för hello en matchande klassnamn och Regelnamn regeln värdet som definierats i hello CSS paket och faller tillbaka toohello ursprung webbserver om det misslyckas toofind hello matchning.
5. Publicera hello Molntjänsten igen och åtkomst hello-startsidan.
6. Visa hello HTML-koden för hello-sidan. Du ska hitta inmatat skript liknande toohello följande:    
   
        ...
   
        <link href="http://az632148.azureedge.net/Content/css?v=1.0.0.25474" rel="stylesheet"/>
        <script>(function() {
                        var loadFallback,
                            len = document.styleSheets.length;
                        for (var i = 0; i < len; i++) {
                            var sheet = document.styleSheets[i];
                            if (sheet.href.indexOf('http://camservice.azureedge.net/Content/css?v=1.0.0.25474') !== -1) {
                                var meta = document.createElement('meta');
                                meta.className = 'sr-only';
                                document.head.appendChild(meta);
                                var value = window.getComputedStyle(meta).getPropertyValue('width');
                                document.head.removeChild(meta);
                                if (value !== '1px') {
                                    document.write('<link href="/Content/css" rel="stylesheet" type="text/css" />');
                                }
                            }
                        }
                        return true;
                    }())||document.write('<script src="/Content/css"><\/script>');</script>
   
            <script src="http://camservice.azureedge.net/bundles/modernizer?v=1.0.0.25474"></script>
        <script>(window.Modernizr)||document.write('<script src="/bundles/modernizr"><\/script>');</script>
   
        ...
   
            <script src="http://camservice.azureedge.net/bundles/jquery?v=1.0.0.25474"></script>
        <script>(window.jquery)||document.write('<script src="/bundles/jquery"><\/script>');</script>
   
            <script src="http://camservice.azureedge.net/bundles/bootstrap?v=1.0.0.25474"></script>
        <script>($.fn.modal)||document.write('<script src="/bundles/bootstrap"><\/script>');</script>
   
        ...

    Observera att inmatat skript för hello CSS paket fortfarande innehåller hello hänga kvarleva från hello `CdnFallbackExpression` egenskap i hello rad:

        }())||document.write('<script src="/Content/css"><\/script>');</script>

    Men eftersom hello första delen av hello. uttrycket returnerar alltid true (i hello rad ovanför som), hello document.write() funktionen aldrig ska köras.

## <a name="more-information"></a>Mer information
* [Översikt över hello Azure Content Delivery Network (CDN)](http://msdn.microsoft.com/library/azure/ff919703.aspx)
* [Med hjälp av Azure CDN](cdn-create-new-endpoint.md)
* [ASP.NET paketering och Minification](http://www.asp.net/mvc/tutorials/mvc-4/bundling-and-minification)

[new-cdn-profile]: ./media/cdn-cloud-service-with-cdn/cdn-new-profile.png
[cdn-profile-settings]: ./media/cdn-cloud-service-with-cdn/cdn-profile-settings.png
[cdn-new-endpoint-button]: ./media/cdn-cloud-service-with-cdn/cdn-new-endpoint-button.png
[cdn-add-endpoint]: ./media/cdn-cloud-service-with-cdn/cdn-add-endpoint.png
[cdn-endpoint-success]: ./media/cdn-cloud-service-with-cdn/cdn-endpoint-success.png
