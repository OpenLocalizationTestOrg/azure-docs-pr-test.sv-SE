---
title: aaaTroubleshoot en webbapp i Azure App Service med Visual Studio
description: "Lär dig hur tootroubleshoot en Azure-webbapp med hjälp av fjärrfelsökning, spårning och loggning verktyg som är inbyggda i tooVisual Studio 2013."
services: app-service
documentationcenter: .net
author: tdykstra
manager: erikre
editor: 
ms.assetid: def8e481-7803-4371-aa55-64025d116c97
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/29/2016
ms.author: rachelap
ms.openlocfilehash: 8e3a8a58293f2ebcdc131fbf2534f8ff99b26730
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-a-web-app-in-azure-app-service-using-visual-studio"></a>Felsöka en webbapp i Azure App Service med Visual Studio
## <a name="overview"></a>Översikt
Den här kursen visar hur toouse Visual Studio verktyg för att felsöka en webbapp i [Apptjänst](http://go.microsoft.com/fwlink/?LinkId=529714), genom att köra i [felsökningsläge](http://www.visualstudio.com/get-started/debug-your-app-vs.aspx) via fjärranslutning eller genom att visa programloggarna samt webbserverloggar.

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

Du får lära dig:

* Vilka Azure web app-hanteringsfunktioner är tillgängliga i Visual Studio.
* Hur visar toouse Visual Studio remote toomake ändringar i en fjärransluten webbapp.
* Hur kör toorun felsökningsläge via fjärranslutning när ett projekt i Azure, både för en webbapp och ett Webbjobb.
* Hur spårningsloggar toocreate program och visa dem när programmet hello skapar dem.
* Hur tooview webbserverloggar, inklusive detaljerade felmeddelanden och spårning av misslyckade begäranden.
* Hur diagnostik toosend loggar tooan Azure Storage-konto och visa dem det.

Om du har Visual Studio Ultimate kan du också använda [IntelliTrace](http://msdn.microsoft.com/library/vstudio/dd264915.aspx) för felsökning. IntelliTrace ingår inte i den här självstudiekursen.

## <a name="prerequisites"></a>Förhandskrav
Den här kursen fungerar med hello utvecklingsmiljö, webbprojektet och Azure-webbapp som du ställer in i [Kom igång med Azure och ASP.NET][GetStarted]. För hello WebJobs avsnitt, behöver du hello-program som du skapar i [Kom igång med hello Azure WebJobs SDK][GetStartedWJ].

exempel som visas i den här kursen är för ett webbprogram i C# MVC, men hello felsökningsanvisningar hello-koden är hello samma för Visual Basic och Web Forms-program.

hello kursen förutsätter att du använder Visual Studio 2015 eller 2013. Om du använder Visual Studio 2013 hello WebJobs kräver [uppdatering 4](http://go.microsoft.com/fwlink/?LinkID=510314) eller senare.

Hej direktuppspelningsloggar funktionen fungerar bara för program som är riktade till .NET Framework 4 eller senare.

## <a name="sitemanagement"></a>Hantering och konfiguration av Web app
Visual Studio innehåller åtkomst tooa deluppsättning av hello web app-hanteringsfunktioner och konfigurationsinställningar som är tillgängliga i hello [Azure Portal](http://go.microsoft.com/fwlink/?LinkId=529715). I det här avsnittet får du se vad som är tillgängligt med hjälp av **Server Explorer**. toosee hello senaste Azure integreringsfunktionerna, testa **Cloud Explorer** också. Du kan öppna både windows från hello **visa** menyn.

1. Om du inte redan är inloggad tooAzure i Visual Studio klickar du på hello **ansluta tooAzure** knappen i **Server Explorer**.

    Ett alternativ är tooinstall ett certifikat som kan komma åt tooyour konto. Om du väljer tooinstall ett certifikat, högerklickar du på hello **Azure** nod i **Server Explorer**, och klicka sedan på **hantera och Filter prenumerationer** hello snabbmenyn. I hello **hantera Azure-prenumerationer** dialogrutan klickar du på hello **certifikat** fliken och klicka sedan på **importera**. Följ hello anvisningarna toodownload och sedan importera en fil för prenumerationen (kallas även en *.publishsettings* fil) för din Azure-konto.

   > [!NOTE]
   > Om du hämtar en prenumerations-fil sparar den tooa mappen utanför katalogerna källa kod (till exempel i hello hämtningsmapp) och tar bort den när hello importen har slutförts. En obehörig användare som får åtkomst till toohello prenumeration fil kan redigera, skapa och ta bort Azure-tjänster.
   >
   >

    Mer information om hur du ansluter tooAzure resurser från Visual Studio finns [hantera konton, prenumerationer och administrativa roller](http://go.microsoft.com/fwlink/?LinkId=324796#BKMK_AccountVCert).
2. I **Server Explorer**, expandera **Azure** och expandera **Apptjänst**.
3. Expandera hello resursgruppen som innehåller hello webbprogram som du skapade i [komma igång med Azure och ASP.NET][GetStarted], högerklicka på noden för hello web app och klickar på **visningsinställningarna**.

    ![Visa inställningar i Server Explorer](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-viewsettings.png)

    Hej **Azure Web App** visas och du kan se det hello web app hantering och konfiguration uppgifter som är tillgängliga i Visual Studio.

    ![Azure Web App-fönster](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-configtab.png)

    I den här självstudiekursen ska du använda hello loggning och spårning i listrutorna. Du använder också fjärrfelsökning men använder du en annan metod tooenable den.

    Information om hello Appinställningar och anslutningssträngar rutor i det här fönstret finns [Azure Web Apps: hur programmet strängar och anslutningen strängar arbete](http://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx).

    Om du vill tooperform en hanteringsaktivitet för webb-app som inte kan göras i det här fönstret klickar du på **öppna i hanteringsportalen** tooopen en webbläsare fönstret toohello Azure-portalen.

## <a name="remoteview"></a>Komma åt web app filer i Server Explorer
Normalt distribuerar ett webbprojekt med hello `customErrors` flaggan i hello Web.config filuppsättning för`On` eller `RemoteOnly`, vilket betyder att du inte får en bra felmeddelande vid något går fel. För många fel är alla du får en sida som något av följande viktiga hello.

**Serverfel i programmet '/':**

![Unhelpful felsida](./media/web-sites-dotnet-troubleshoot-visual-studio/genericerror.png)

**Ett fel uppstod:**

![Unhelpful felsida](./media/web-sites-dotnet-troubleshoot-visual-studio/genericerror1.png)

**hello webbplatsen kan inte visa hello sida**

![Unhelpful felsida](./media/web-sites-dotnet-troubleshoot-visual-studio/genericerror2.png)

Ofta hello enklaste sättet toofind hello hello fel beror på tooenable detaljerade felmeddelanden som hello första hello som föregår skärmbilderna förklarar hur toodo. Som kräver en ändring i hello distribuerats Web.config-filen. Du kan redigera hello *Web.config* filen i hello projektet och distribuera om hello, eller skapa en [Web.config-transformering](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/web-config-transformations) och distribuera en felsökningsversion, men det finns ett snabbare sätt: i **lösning Explorer** du direkt kan visa och redigera filer i hello remote webbapp med hjälp av hello *fjärrvy* funktion.

1. I **Server Explorer**, expandera **Azure**, expandera **Apptjänst**expanderar hello resursgrupp som ditt webbprogram finns i, och sedan hello noden för ditt webbprogram.

    Du ser noder som ger dig åtkomst toohello webbprogram innehållsfiler och loggfiler.
2. Expandera hello **filer** nod, och dubbelklicka på hello *Web.config* fil.

    ![Öppna Web.config](./media/web-sites-dotnet-troubleshoot-visual-studio/webconfig.png)

    Visual Studio öppnas hello Web.config-filen från hello remote webbapp och visar [Remote] nästa toohello filnamnet i hello namnlist.
3. Lägg till följande rad toohello hello `system.web` element:

    `<customErrors mode="Off"></customErrors>`

    ![Redigera Web.config](./media/web-sites-dotnet-troubleshoot-visual-studio/webconfigedit.png)
4. Uppdatera hello webbläsare som visar hello unhelpful felmeddelande och nu du få ett detaljerat felmeddelande, till exempel hello följande exempel:

    ![Detaljerat felmeddelande](./media/web-sites-dotnet-troubleshoot-visual-studio/detailederror.png)

    (hello fel visas har skapats genom att lägga till hello rad som visas i rött för*Views\Home\Index.cshtml*.)

Redigera hello Web.config-filen är bara ett exempel på scenarier i vilka hello möjlighet tooread och redigera filer på Azure-webbapp gör felsökningen lättare.

## <a name="remotedebug"></a>Fjärråtkomst felsökning webbprogram
Om hello detaljerat felmeddelande inte ger tillräcklig information, och du kan inte återskapa hello fel lokalt, är ett annat sätt tootroubleshoot toorun i felsökningsläge via fjärranslutning. Du kan ange brytpunkter, ändra minne direkt, stega igenom koden och även ändra hello kodsökvägen.

Fjärrfelsökning fungerar inte i Express-versioner av Visual Studio.

Det här avsnittet visas hur toodebug med hello projekt som du skapar i [komma igång med Azure och ASP.NET][GetStarted].

1. Öppna hello webbprojekt som du skapade i [komma igång med Azure och ASP.NET][GetStarted].
2. Öppna *Controllers\HomeController.cs*.
3. Ta bort hello `About()` metoden och infoga hello följande kod i dess ställe.

        public ActionResult About()
        {
            string currentTime = DateTime.Now.ToLongTimeString();
            ViewBag.Message = "hello current time is " + currentTime;
            return View();
        }
4. [Ange en brytpunkt](http://www.visualstudio.com/get-started/debug-your-app-vs.aspx) på hello `ViewBag.Message` rad.
5. I **Solution Explorer**, högerklicka på hello-projektet och klicka på **publicera**.
6. I hello **profil** listrutan, Välj hello samma profil som du använde i [komma igång med Azure och ASP.NET][GetStarted].
7. Klicka på hello **inställningar** fliken och ändra **Configuration** för**felsöka**, och klicka sedan på **publicera**.

    ![Publicera i felsökningsläge](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-publishdebug.png)
8. Efter distributionen öppnas är klar och webbläsaren toohello Azure URL för ditt webbprogram, Stäng hello webbläsare.
9. I **Server Explorer**, högerklicka på ditt webbprogram och klicka sedan på **koppla felsökare**.

    ![Koppla felsökare](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-attachdebugger.png)

    hello webbläsaren öppnas automatiskt tooyour startsidan körs i Azure. Du kanske har toowait 20 sekunder eller så medan Azure konfigurerar hello server för felsökning. Den här fördröjningen sker endast hello första gången du kör i felsökningsläge på ett webbprogram. Efterföljande gånger inom hello nästa 48 timmar när du startar felsökningen igen det kommer inte att en fördröjning.

    **Obs:** om du har några problem med att starta hello felsökare försök toodo den med hjälp av **Cloud Explorer** i stället för **Server Explorer**.
10. Klicka på **om** hello-menyn.

     Visual Studio stoppar på hello brytpunkt och hello koden körs i Azure, inte på den lokala datorn.
11. Hovra över hello `currentTime` variabeln toosee hello tid-värden.

     ![Visa variabeln i felsökningsläge som körs i Azure](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-debugviewinwa.png)

     hello är tid det du ser hello Azure-servern, vilket kan vara i en annan tidszon än den lokala datorn.
12. Ange ett nytt värde för hello `currentTime` variabel, till exempel ”nu körs i Azure”.
13. Tryck på F5 toocontinue körs.

     hello om sidan som körs i Azure visar hello nytt värde som du angav i hello currentTime variabel.

     ![Om sidan med nytt värde](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-debugchangeinwa.png)

## <a name="remotedebugwj"></a>Fjärråtkomst felsökning WebJobs
Det här avsnittet visas hur toodebug med hello projekt och web app du skapar i [Kom igång med hello Azure WebJobs SDK](websites-dotnet-webjobs-sdk.md).

hello-funktioner som visas i det här avsnittet är endast tillgänglig i Visual Studio 2013 med Update 4 eller senare.

Fjärrfelsökning fungerar bara med kontinuerliga Webbjobb. Schemalagda och på begäran WebJobs saknar stöd för felsökning.

1. Öppna hello webbprojekt som du skapade i [Kom igång med hello Azure WebJobs SDK][GetStartedWJ].
2. Öppna i hello ContosoAdsWebJob project *Functions.cs*.
3. [Ange en brytpunkt](http://www.visualstudio.com/get-started/debug-your-app-vs.aspx) på hello första satsen i hello `GnerateThumbnail` metod.

    ![Ange brytpunkt](./media/web-sites-dotnet-troubleshoot-visual-studio/wjbreakpoint.png)
4. I **Solution Explorer**och högerklicka på hello webbprojekt (inte hello Webbjobb projekt), klickar du på **publicera**.
5. I hello **profil** listrutan, Välj hello samma profil som du använde i [Kom igång med hello Azure WebJobs SDK](websites-dotnet-webjobs-sdk.md).
6. Klicka på hello **inställningar** fliken och ändra **Configuration** för**felsöka**, och klicka sedan på **publicera**.

    Visual Studio distribuerar hello webb- och Webbjobb projekt och webbläsaren öppnar toohello Azure URL för ditt webbprogram.
7. I **Server Explorer** Expandera **Azure > Apptjänst > resursgruppen > ditt webbprogram > WebJobs > kontinuerlig**, högerklicka på **ContosoAdsWebJob**.
8. Klicka på **koppla felsökare**.

    ![Koppla felsökare](./media/web-sites-dotnet-troubleshoot-visual-studio/wjattach.png)

    hello webbläsaren öppnas automatiskt tooyour startsidan körs i Azure. Du kanske har toowait 20 sekunder eller så medan Azure konfigurerar hello server för felsökning. Den här fördröjningen sker endast hello första gången du kör i felsökningsläge på ett webbprogram. hello nästa gång du kopplar det hello felsökare inte en fördröjning om du gör det inom 48 timmar.
9. Skapa en ny annons i hello webbläsare som är öppnade toohello Contoso Ads-startsidan.

    Skapa en ad gör en kön meddelandet toobe skapas, som hämtas av hello Webbjobb och behandlas. När hello WebJobs SDK anropar funktionen tooprocess hello kön hälsningsmeddelande, kommer hello kod till din brytpunkt.
10. När hello felsökare bryts vid din brytpunkt, kan du granska och ändra variabelvärden när hello programmet körs hello molnet. Hello innehållet i hello blobInfo objekt som skickades toohello GenerateThumbnail metoden visas i följande illustration hello felsökare i hello.

     ![blobInfo objekt i Felsökning](./media/web-sites-dotnet-troubleshoot-visual-studio/blobinfo.png)
11. Tryck på F5 toocontinue körs.

     Hej GenerateThumbnail metod har skapat hello miniatyr.
12. Hello webbläsare, uppdatera hello indexsidan och se hello miniatyr.
13. Tryck på SKIFT + F5 toostop felsökning i Visual Studio.
14. I **Server Explorer**, högerklicka på hello ContosoAdsWebJob noden och klickar på **visa instrumentpanelen**.
15. Logga in med dina autentiseringsuppgifter för Azure och klicka på hello Webbjobb toogo toohello sida för din Webbjobb.

     ![Klicka på ContosoAdsWebJob](./media/web-sites-dotnet-troubleshoot-visual-studio/clickcaw.png)

     hello instrumentpanelen visar att hello GenerateThumbnail funktion som kördes nyligen.

     (hello nästa gång du klickar på **visa instrumentpanelen**du inte har toosign i och hello webbläsaren går direkt toohello sidan för din Webbjobb.)
16. Klicka på hello funktionen toosee detaljer om hello funktionen körning.

     ![Funktionsinformation](./media/web-sites-dotnet-troubleshoot-visual-studio/funcdetails.png)

Om din funktion [skrev loggar](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#logs), du kan klicka på **ToggleOutput** toosee dem.

## <a name="notes-about-remote-debugging"></a>Information om fjärrfelsökning
* Du bör inte köra i felsökningsläge i produktion. Om ditt webbprogram för produktion inte skalas ut toomultiple serverinstanser att felsökning hello-webbservern från svarar tooother begäranden. Om du gör flera web server-instanser, när du ansluter toohello felsökare får du en slumpmässig instans, och du har inga sätt tooensure efterföljande webbläsaren begäranden skickas toothat instans. Dessutom normalt distribuerar inte en debug build tooproduction och optimeringar av kompileraren för versionen versioner kan göra det omöjligt tooshow vad som händer rad för rad i källkoden. För att felsöka problem i produktionen bästa resurs är program spårning och web server-loggar.
* Undvika lång stopp vid brytpunkter när remote felsökning. Azure behandlar en process som har stoppats för längre än några minuter som en process för svarar inte och stängs av.
* När du felsöker skickar hello servern data tooVisual Studio som kan påverka kostnader för bandbredd. Information om bandbredd priser finns [priser för Azure](https://azure.microsoft.com/pricing/calculator/).
* Kontrollera att hello `debug` attribut för hello `compilation` element i hello *Web.config* tootrue anges i filen. Tootrue anges som standard när du publicerar en debug build-konfiguration.

        <system.web>
          <compilation debug="true" targetFramework="4.5" />
          <httpRuntime targetFramework="4.5" />
        </system.web>
* Om du hittar den hello-felsökaren inte kommer gå till kod som du vill toodebug kanske toochange hello bara den här inställningen.  Mer information finns i [begränsa version tooJust min kod](http://msdn.microsoft.com/library/vstudio/y740d9d3.aspx#BKMK_Restrict_stepping_to_Just_My_Code).
* En timer startar på hello server när du aktiverar hello remote felsökningsfunktionen och efter 48 timmar hello-funktionen inaktiveras automatiskt. Den här gränsen 48 timmar görs grund för säkerhet och prestanda. Du kan enkelt aktivera hello funktionen igen så många gånger. Vi rekommenderar att du lämnar inaktiverad när du inte aktivt felsökning.
* Du kan manuellt koppla hello felsökare tooany process, inte bara hello web app processen (w3wp.exe). Mer information om hur toouse felsökningsläge i Visual Studio finns [felsökning i Visual Studio](http://msdn.microsoft.com/library/vstudio/sc65sadd.aspx).

## <a name="logsoverview"></a>Översikt över diagnostikloggar
Ett ASP.NET-program som körs i en Azure-app kan skapa hello följande typer av loggar:

* **Programmet spårningsloggar**<br/>
  hello programmet skapar de här loggarna genom att anropa metoder i hello [System.Diagnostics.Trace](http://msdn.microsoft.com/library/system.diagnostics.trace.aspx) klass.
* **Webbserverloggarna**<br/>
  hello webbservern skapar en loggpost för varje HTTP-begäran toohello webbprogrammet.
* **Detaljerade felloggar för meddelandet**<br/>
  hello webbservern skapar en HTML-sida med ytterligare information för misslyckade HTTP-begäranden (de som resulterar i statuskod: 400 eller högre).
* **Det gick inte begäran spårningsloggar**<br/>
  hello webbservern skapar en XML-fil med detaljerad spårningsinformation för misslyckade begäranden för HTTP. hello-webbservern ger också en XSL-filen tooformat hello XML i en webbläsare.

Loggning påverkar web app prestanda, så Azure ger dig hello möjlighet tooenable eller inaktivera varje typ av loggen vid behov. Du kan ange att endast loggar över en viss nivå för allvarlighetsgrad ska skrivas för programloggar. När du skapar ett nytt webbprogram som standard alla loggning har inaktiverats.

Loggarna skrivs toofiles en *loggfiler* mapp i hello filsystemet på din webbapp och är tillgänglig via FTP. Webbserverloggarna- och programloggarna kan också vara skriven tooan Azure Storage-konto. Du kan behålla en större volym loggar i ett lagringskonto än vad som är möjligt i hello-filsystem. När du använder hello filsystemet är du begränsad tooa högst 100 megabyte loggar. (Filen systemloggar är endast för kortsiktig kvarhållning. Azure tar bort gamla loggen filer toomake plats för nya när hello gränsen har nåtts.)  

## <a name="apptracelogs"></a>Skapa och visa spårningsloggar för programmet
I det här avsnittet gör du hello följande uppgifter:

* Lägg till spårning instruktioner toohello-projektet som du skapade i [Kom igång med Azure och ASP.NET][GetStarted].
* Visa hello loggar när du kör hello projektet lokalt.
* Visa hello loggar när de skapas av hello-program som körs i Azure.

Information om hur toocreate programloggar i WebJobs finns [hur toowork med Azure queue storage med hello WebJobs SDK - hur toowrite loggar](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#logs). hello följande instruktioner för att visa loggar och kontrollera hur de lagras i Azure gäller också tooapplication loggarna som skapas i WebJobs.

### <a name="add-tracing-statements-toohello-application"></a>Lägg till spårning instruktioner toohello program
1. Öppna *Controllers\HomeController.cs*, och Ersätt hello `Index`, `About`, och `Contact` metoder med hello följande kod i ordning tooadd `Trace` instruktioner och en `using` instruktionen för `System.Diagnostics`:

        public ActionResult Index()
        {
            Trace.WriteLine("Entering Index method");
            ViewBag.Message = "Modify this template toojump-start your ASP.NET MVC application.";
            Trace.TraceInformation("Displaying hello Index page at " + DateTime.Now.ToLongTimeString());
            Trace.WriteLine("Leaving Index method");
            return View();
        }

        public ActionResult About()
        {
            Trace.WriteLine("Entering About method");
            ViewBag.Message = "Your app description page.";
            Trace.TraceWarning("Transient error on hello About page at " + DateTime.Now.ToShortTimeString());
            Trace.WriteLine("Leaving About method");
            return View();
        }

        public ActionResult Contact()
        {
            Trace.WriteLine("Entering Contact method");
            ViewBag.Message = "Your contact page.";
            Trace.TraceError("Fatal error on hello Contact page at " + DateTime.Now.ToLongTimeString());
            Trace.WriteLine("Leaving Contact method");
            return View();
        }        
2. Lägg till en `using System.Diagnostics;` instruktionen toohello överkant hello-filen.

### <a name="view-hello-tracing-output-locally"></a>Visa hello spårning utdata lokalt
1. Tryck på F5 toorun hello programmet i felsökningsläge.

    hello standard spårningslyssnaren skriver alla trace utdata toohello **utdata** fönster, tillsammans med andra felsökningsresultat. hello följande bild visar hello utdata från hello trace-satser som du lagt till toohello `Index` metod.

    ![Spårning i felsökningsfönstret](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-debugtracing.png)

    hello följande steg visar hur tooview trace utdata i en webbsida, utan att kompilera i felsökningsläge.
2. Öppna hello programmets Web.config-file (hello placerad i projektmappen hello) och Lägg till en `<system.diagnostics>` element hello slutet av hello-filen innan du hello avslutande `</configuration>` element:

          <system.diagnostics>
            <trace>
              <listeners>
                <add name="WebPageTraceListener"
                    type="System.Web.WebPageTraceListener,
                    System.Web,
                    Version=4.0.0.0,
                    Culture=neutral,
                    PublicKeyToken=b03f5f7f11d50a3a" />
              </listeners>
            </trace>
          </system.diagnostics>

    Hej `WebPageTraceListener` kan du visa spårningsutdata genom att bläddra för`/trace.axd`.
3. Lägg till en <a href="http://msdn.microsoft.com/library/vstudio/6915t83k(v=vs.100).aspx">elementet trace</a> under `<system.web>` i hello Web.config-fil, till exempel hello följande exempel:

        <trace enabled="true" writeToDiagnosticsTrace="true" mostRecent="true" pageOutput="false" />
4. Tryck på CTRL + F5 toorun hello program.
5. Lägg till i hello adressfältet i webbläsarfönstret hello, *trace.axd* toohello URL, och tryck sedan på RETUR (hello URL är liknande toohttp://localhost:53370/trace.axd).
6. På hello **programmet Trace** klickar du på **visa information om** på hello första raden (inte hello BrowserLink rad).

    ![Trace.Axd](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-traceaxd1.png)

    Hej **begär information** visas, och i hello **spårningsinformation** avsnitt som du ser hello utdata från hello trace-satser som du lagt till toohello `Index` metod.

    ![Trace.Axd](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-traceaxd2.png)

    Som standard `trace.axd` endast är tillgängligt lokalt. Om du vill toomake den är tillgänglig från en fjärransluten webbapp, kan du lägga till `localOnly="false"` toohello `trace` element i hello *Web.config* -fil, som visas i följande exempel hello:

        <trace enabled="true" writeToDiagnosticsTrace="true" localOnly="false" mostRecent="true" pageOutput="false" />

    Men att aktivera `trace.axd` i en webbapp i allmänhet rekommenderas inte av säkerhetsskäl och i hello följande avsnitt ser du ett enklare sätt tooread spårningsloggarna i en Azure webbapp.

### <a name="view-hello-tracing-output-in-azure"></a>Visa hello spårning utdata i Azure
1. I **Solution Explorer**, högerklicka hello webbprojektet och på **publicera**.
2. I hello **Publicera webbplats** dialogrutan klickar du på **publicera**.

    När Visual Studio publicerar uppdateringen, öppnas en webbläsare fönstret tooyour startsida (förutsatt att du inte avmarkera **Måladress** på hello **anslutning** fliken).
3. I **Server Explorer**, högerklicka på ditt webbprogram och välj **visa strömning loggfiler**.

    ![Visa strömning loggfiler i snabbmenyn](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-viewlogsmenu.png)

    Hej **utdata** fönster visar att du är ansluten toohello log-direktuppspelning tjänsten och lägger till en Meddelanderad varje minut som går utan en logg toodisplay.

    ![Visa strömning loggfiler i snabbmenyn](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-nologsyet.png)
4. I hello webbläsarfönster som visar startsidan program klickar du på **Kontakta**.

    Inom några sekunder hello utdata från hello Felnivån spårning du har lagt till toohello `Contact` metoden visas i hello **utdata** fönster.

    ![Felspårning i utdatafönstret](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-errortrace.png)

    Visual Studio bara visar Felnivån spårningar eftersom det är standardinställningen för hello när du aktiverar hello loggen övervakningstjänsten. När du skapar en ny Azure webbapp är all loggning inaktiverat som standard, som du såg när du har öppnat hello inställningssidan tidigare:

    ![Programmet logga ut](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-apploggingoff.png)

    Men om du har valt **visa strömning loggfiler**, Visual Studio automatiskt ändras **programmet Logging(File System)** för**fel**, vilket innebär att Felnivån loggar Hämta rapporterade. I order toosee alla spårningsloggar för din, du kan ändra den här inställningen för**utförlig**. När du väljer en allvarlighetsgrad som är lägre än fel rapporteras också alla loggar för högre nivåer av allvarlighetsgrad. Så när du väljer utförlig kan se du också information, varning och fel.  

1. I **Server Explorer**högerklickar du på hello webbapp och klicka sedan på **visningsinställningarna** som du gjorde tidigare.
2. Ändra **programloggning (filsystem)** för**utförlig**, och klicka sedan på **spara**.

    ![Inställningen spåra nivå tooVerbose](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-applogverbose.png)
3. I hello webbläsarfönster som nu visas ditt **Kontakta** klickar du på **Start**, klicka på **om**, och klicka sedan på **Kontakta**.

    Inom några sekunder hello **utdata** fönster visas alla spårning-utdata.

    ![Utförlig spårningsutdata](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-verbosetraces.png)

    I det här avsnittet aktiverat och inaktiverat loggning genom att använda Azure web app-inställningar. Du kan också aktivera och inaktivera Samlingsspårlyssnarna genom att ändra hello Web.config-filen. Dock gör ändra hello Web.config-filen hello app domän toorecycle när du aktiverar loggning via hello webbappkonfigurationen inte göra det. Om hello problemet tar en lång tid tooreproduce eller är tillfälligt, kan återvinning hello tillämpningsdomän ”åtgärdas” och tvinga toowait tills det sker igen. Aktivera diagnostik i Azure göra inte detta, så att du kan börja samla in information om fel omedelbart.

### <a name="output-window-features"></a>Utdata fönstret funktioner
Hej **Azure loggar** för hello **utdata** fönstret har flera knappar och en textruta:

![Loggar fliken knappar](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-icons.png)

Dessa utför hello följande funktioner:

* Rensa hello **utdata** fönster.
* Aktivera eller inaktivera automatisk radbyte.
* Starta eller stoppa övervakningen av loggar.
* Ange vilka loggar toomonitor.
* Hämta loggarna.
* Filtrera loggarna baserat på en söksträng eller ett reguljärt uttryck.
* Stäng hello **utdata** fönster.

Om du anger en söksträng eller det reguljära uttrycket filtrerar Visual Studio logginformation på hello-klienten. Det innebär att du kan ange hello kriterier när hello loggarna visas i hello **utdata** fönstret och du kan ändra filtreringskriterier utan tooregenerate hello loggar.

## <a name="webserverlogs"></a>Visa webbserverloggar
Webbserverloggarna registrera alla HTTP-aktivitet för hello webbprogrammet. I ordning toosee dem i hello **utdata** fönster som du har tooenable dem för hello web app och Visual Studio att du vill ange toomonitor dem.

1. I hello **Azure Web App Configuration** fliken som du har öppnat från **Server Explorer**, ändra Web Server-loggning för**på**, och klicka sedan på **spara**.

    ![Aktivera web server-loggning](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-webserverloggingon.png)
2. I hello **utdata** -fönstret klickar du på hello **anger vilka Azure loggar toomonitor** knappen.

    ![Ange vilken Azure loggar toomonitor](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-specifylogs.png)
3. I hello **Azure loggningsalternativ** dialogrutan **Web server-loggar**, och klicka sedan på **OK**.

    ![Övervaka webbserverloggar](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-monitorwslogson.png)
4. Hello webbläsare som visar hello webbprogrammet på fönstret **Start**, klicka på **om**, och klicka sedan på **Kontakta**.

    hello programloggarna vanligtvis visas först, följt av hello webbserverloggar. Du måste kanske toowait ett tag för hello loggar tooappear.

    ![Webbservern loggar i utdatafönstret](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-wslogs.png)

Som standard skriver Azure hello loggar toohello filsystem när du aktiverar webbserverloggar med hjälp av Visual Studio. Alternativt kan du använda hello Azure portal toospecify som web server-loggar ska skrivas tooa blob-behållaren i ett lagringskonto.

Om du använder hello portal tooenable webbserver-loggning tooan Azure storage-konto och sedan inaktivera loggning i Visual Studio när du återaktivera loggning i Visual Studio återställs dina kontoinställningar för lagring.

## <a name="detailederrorlogs"></a>Visa detaljerade felloggar för meddelandet
Detaljerade felloggar ger ytterligare information om HTTP-förfrågningar som resulterar i svaret felkoder (400 eller senare). I ordning toosee dem i hello **utdata** fönstret har tooenable dem för hello web app och Visual Studio att du vill ange toomonitor dem.

1. I hello **Azure Web App Configuration** fliken som du har öppnat från **Server Explorer**, ändra **detaljerade felmeddelanden** för**på**, och Klicka på **spara**.

    ![Aktivera detaljerade felmeddelanden](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-detailedlogson.png)
2. I hello **utdata** -fönstret klickar du på hello **anger vilka Azure loggar toomonitor** knappen.
3. I hello **Azure loggningsalternativ** dialogrutan klickar du på **alla loggar**, och klicka sedan på **OK**.

    ![Övervaka alla loggar](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-monitorall.png)
4. Hello adressfält hello webbläsarfönster, lägga till en extra tecken toohello URL toocause ett 404-fel (till exempel `http://localhost:53370/Home/Contactx`), och tryck på RETUR.

    Efter några sekunder visas hello detaljerad fellogg i hello Visual Studio **utdata** fönster.

    ![Detaljerad fellogg i utdatafönstret](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-detailederrorlog.png)

    CTRL + klicka hello länken toosee hello loggutdata formaterad i en webbläsare:

    ![Detaljerad fellogg i webbläsarfönster](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-detailederrorloginbrowser.png)

## <a name="downloadlogs"></a>Hämta filen systemloggar
Loggar som du kan övervaka i hello **utdata** fönster kan också hämtas som en *.zip* fil.

1. I hello **utdata** -fönstret klickar du på **hämta strömning loggar**.

    ![Loggar fliken knappar](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-downloadicon.png)

    Utforskaren öppnar tooyour *hämtar* mapp med hello hämtade filen har valt.

    ![Hämtade filen](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-downloadedfile.png)
2. Extrahera hello *.zip* fil och du ser hello följande mappstrukturen:

    ![Hämtade filen](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-logfilefolders.png)

   * Programmet spårningsloggar finns i *.txt* filer i hello *LogFiles\Application* mapp.
   * Webbserverloggarna är i *.log* filer i hello *LogFiles\http\RawLogs* mapp. Du kan använda ett verktyg som [Loggparser](http://www.microsoft.com/download/details.aspx?displaylang=en&id=24659) tooview och ändra filerna.
   * Meddelande för detaljerade felloggar finns i *.html* filer i hello *LogFiles\DetailedErrors* mapp.

     (hello *distributioner* mappen är för filer som skapas av källkontrollen publicering; inte är något annat relaterat tooVisual Studio publicering. Hej *Git* mappen är spårningar relaterade toosource kontroll publicering och hello loggfilen streaming service.)  

## <a name="storagelogs"></a>Visa loggfiler för lagring
Spårningsloggar för programmet kan även skickas tooan Azure storage-konto och du kan visa dem i Visual Studio. toodo att du ska skapa ett lagringskonto, aktivera lagring loggar i hello klassiska portalen och visa dem i hello **loggar** för hello **Azure Web App** fönster.

Du kan skicka loggar tooany eller alla tre mål:

* hello-filsystem.
* Storage-konto-tabeller.
* Storage-konto-blobbar.

Du kan ange en annan allvarlighetsgrad för varje mål.

Tabeller är det enkelt tooview information online loggar och stöd för direktuppspelning; Du kan fråga loggar i tabeller och se nya loggar som de skapas. Blobbar gör det enkelt toodownload loggar i filer och tooanalyze dem med HDInsight, eftersom HDInsight vet hur toowork med blob storage. Mer information finns i **Hadoop och MapReduce** i [alternativen för datalagring (skapa verkliga Molnappar med Azure)](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options).

Du har för närvarande loggar set tooverbose filsystemnivå; hello följande steg beskriver hur du konfigurerar information nivån loggar toogo toostorage konto tabeller. Informationsnivå innebär alla loggar som skapats genom att anropa `Trace.TraceInformation`, `Trace.TraceWarning`, och `Trace.TraceError` visas, men inte loggarna som skapas genom att anropa `Trace.WriteLine`.

Storage-konton ger mer lagringsutrymme och längre kvarhållning för loggar jämfört med toohello filsystem. En annan fördel med avsändarprogrammet spårning loggar toostorage är att du får ytterligare information med varje logg som du inte får från filen systemloggar.

1. Högerklicka på **lagring** under hello Azure noden och klicka sedan på **skapa Lagringskonto**.

![Skapa Lagringskonto](./media/web-sites-dotnet-troubleshoot-visual-studio/createstor.png)

1. I hello **skapa Lagringskonto** dialogrutan, ange ett namn för hello storage-konto.

    hello namn måste vara unika (ingen Azure storage-konto kan ha hello samma namn). Om hello namnet används redan får du en chans toochange den.

    Hej URL tooaccess storage-konto kommer att *{name}*. core.windows.net.
2. Ange hello **Region eller Affinitetsgrupp** listrutan toohello region närmaste tooyou.

    Den här inställningen anger vilket Azure-datacenter ska vara värd för ditt lagringskonto. Ditt val inte för den här självstudiekursen gör någon märkbar skillnad, men för en webbapp i produktion du vill webbservern och toobe din storage-konto i hello samma region toominimize svarstid och data utgång avgifter. hello-webbprogram (som du skapar senare) ska köras i en region så nära som möjligt toohello webbläsare åtkomst till ditt webbprogram i ordning toominimize latens.
3. Ange hello **replikering** nedrullningsbara listan för**lokalt redundant**.
   
    När geo-replikering är aktiverat för ett lagringskonto är hello lagras innehållet replikerade tooa sekundärt datacenter tooenable redundans toothat plats vid en större katastrof på hello primär plats. Geo-replikering kan medföra ytterligare kostnader. För testning och utveckling konton vill du normalt inte toopay för geo-replikering. Mer information finns i [Skapa, hantera eller ta bort ett lagringskonto](../storage/common/storage-create-storage-account.md).
4. Klicka på **Skapa**.

    ![Nytt lagringskonto](./media/web-sites-dotnet-troubleshoot-visual-studio/newstorage.png)    
5. I hello Visual Studio **Azure Web App** -fönstret klickar du på hello **loggar** fliken och klicka sedan på **konfigurera loggning i hanteringsportalen**.

    <!-- todo:screenshot of new portal if hello VS page link goes toonew portal -->
    ![Konfigurera loggning](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-configlogging.png)

    Då öppnas hello **konfigurera** fliken i hello klassiska portalen för ditt webbprogram.
6. I hello klassiska portalen **konfigurera** fliken, bläddrar du nedåt toohello programavsnittet diagnostik och ändra **programloggning (Table Storage)** för**på**.
7. Ändra **loggningsnivån** för**Information**.
8. Klicka på **hantera tabellagring**.

    ![Klicka på Hantera TableStorage](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-stgsettingsmgmtportal.png)

    I hello **hantera tabellagring för application diagnostics** rutan du storage-konto om du har mer än ett. Du kan skapa en ny tabell eller använda en befintlig.

    ![Hantera tabellagring](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-choosestorageacct.png)
9. I hello **hantera tabellagring för application diagnostics** rutan klickar du på hello markerat tooclose hello.
10. I hello klassiska portalen **konfigurera** klickar du på **spara**.
11. I hello webbläsarfönster som visar hello webbprogrammet för programmet, klickar du på **Start**, klicka på **om**, och klicka sedan på **Kontakta**.

     hello loggningsinformation genereras genom att bläddra bland dessa webbsidor skrivs toohello storage-konto.
12. I hello **loggar** för hello **Azure Web App** fönstret i Visual Studio klickar du på **uppdatera** under **diagnostiska sammanfattning**.

     ![Klicka på Uppdatera](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-refreshstorage.png)

     Hej **diagnostiska sammanfattning** avsnittet visar loggar för hello senaste 15 minuterna som standard. Du kan ändra hello period toosee fler loggar.

     (Om du får felet ”Det gick inte att hitta tabellen” Kontrollera som besökt toohello sidor som hello spårning när du aktiverat **programloggning (lagring)** och när du klickat på **spara**.)

     ![Storage-loggar](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-storagelogs.png)

     Observera att i den här vyn du se **Process-ID** och **tråd-ID** för varje logg som du inte får i hello systemloggar för filen. Du kan se ytterligare fält genom att visa hello Azure storage tabell direkt.
13. Klicka på **visa alla programloggarna**.

     hello trace log tabellen visas i visningsprogrammet för hello Azure storage-tabellen.

     (Om du får felet ”sekvensen innehåller inga element” öppna **Server Explorer**, expandera hello nod för ditt lagringskonto under hello **Azure** nod och högerklicka sedan **tabeller**och klicka på **uppdatera**.)

     ![Lagring loggar i tabellvy](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-tracelogtableview.png)

     Den här vyn visar ytterligare fält som inte visas i andra vyer. Den här vyn kan du också toofilter loggar med hjälp av särskilda fråga Builder-Gränssnittet för att konstruera en fråga. Mer information finns i Arbeta med resurser för tabell - filtrering entiteter i [surfning lagringsresurser med Server Explorer](http://msdn.microsoft.com/library/ff683677.aspx).
14. toolook hello information för en enda rad, dubbelklickar du på något av hello rader.

     ![Spåra tabellen i Server Explorer](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-tracetablerow.png)

## <a name="failedrequestlogs"></a>Visa spårningsloggar för misslyckade begäranden
Begäran om misslyckade spårningsloggarna är användbara när du behöver toounderstand hello information om hur IIS hanterar en HTTP-begäran i scenarier, till exempel skriva om eller autentisering problem med URL.

Azure web apps Använd hello samma kunde inte begära spårning funktioner som har varit tillgänglig med IIS 7.0 och senare. Du har inte åtkomst toohello IIS-inställningar som konfigurerar vilka fel får loggas men. När du aktiverar spårning av misslyckade begäranden fångas alla fel.

Du kan aktivera spårning av misslyckade begäranden med hjälp av Visual Studio, men du kan inte visa dem i Visual Studio. Dessa loggar är XML-filer. hello strömning Loggtjänsten bara övervakar filer som bedöms vara läsbar i läget oformaterad text: *.txt*, *.html*, och *.log* filer.

Du kan visa spårningsloggar för misslyckade begäranden i en webbläsare direkt via FTP- eller lokalt efter att du använder en FTP-verktyget toodownload dem tooyour lokal dator. I det här avsnittet ska du visa dem i en webbläsare direkt.

1. I hello **Configuration** för hello **Azure Web App** fönster som du öppnade från **Server Explorer**, ändra **spårning av misslyckade begäranden**för**på**, och klicka sedan på **spara**.

    ![Aktivera spårning av misslyckade begäranden](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-failedrequeston.png)
2. Lägg till ett extra tecken toohello URL hello adressfält hello webbläsarfönster som visar hello webbprogram, och klicka på Ange toocause ett 404-fel.

    Detta leder till misslyckad begäran spårning loggen toobe skapas och hello följande steg visar hur tooview eller hämtning hello loggen.
3. I Visual Studio i hello **Configuration** för hello **Azure Web App** -fönstret klickar du på **öppna i hanteringsportalen**.
4. I hello [Azure-portalen](https://portal.azure.com) **inställningar** klickar du på bladet för din webbapp **distributionsbehörigheterna**, och ange sedan ett nytt användarnamn och lösenord.

    ![Ny FTP-användarnamn och lösenord](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-enterftpcredentials.png)

    ** När du loggar in har toouse hello fullständig användarnamn med hello web app name prefixet tooit. Till exempel om du anger ”myid” som ett användarnamn och hello platsen är ”mittexempel” kan logga du in som ”myexample\myid”.
5. I ett nytt webbläsarfönster går toohello URL som visas under **FTP-värdnamn** eller **FTPS hostname** i hello **Web App** bladet för ditt webbprogram.
6. Logga in med hello FTP-autentiseringsuppgifter som du skapade tidigare (inklusive hello web app namnprefix för hello användarnamn).

    hello webbläsaren visar hello rotmapp hello webbprogrammet.
7. Öppna hello *loggfiler* mapp.

    ![Öppna loggfiler mapp](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-logfilesfolder.png)
8. Öppna hello mapp som heter W3SVC plus ett numeriskt värde.

    ![Öppna W3SVC-mappen](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-w3svcfolder.png)

    hello mappen innehåller XML-filer för eventuella fel som loggas när du har aktiverat spårning av misslyckade begäranden och en XSL-fil som en webbläsare kan använda tooformat hello XML.

    ![W3SVC-mappen](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-w3svcfoldercontents.png)
9. Klicka på hello XML-fil för hello misslyckade begäranden som du vill toosee spårningsinformation för.

    hello visar följande bild en del av hello spårningsinformation för ett prov-fel.

    ![Spårning av misslyckade begäranden i webbläsare](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-failedrequestinbrowser.png)

## <a name="nextsteps"></a>Nästa steg
Du har sett hur Visual Studio gör det enkelt tooview loggarna som skapas i en Azure-webbapp. hello följande avsnitt innehåller länkar toomore resurser på Närliggande ämnen:

* Felsökning av Azure web app
* Felsökning i Visual Studio
* Fjärr-felsökning i Azure
* Spårning i ASP.NET-program
* När webbserverloggar
* Det gick inte att analysera spårningsloggar för begäran
* Felsökning av molntjänster

### <a name="azure-web-app-troubleshooting"></a>Felsökning av Azure web app
Mer information om hur du felsöker web apps i Azure App Service finns hello följande resurser:

* [Hur toomonitor webbappar](/manage/services/web-sites/how-to-monitor-websites/)
* [Undersöka minnesläckor i Azure Webbappar med Visual Studio 2013](http://blogs.msdn.com/b/visualstudioalm/archive/2013/12/20/investigating-memory-leaks-in-azure-web-sites-with-visual-studio-2013.aspx). Microsoft ALM blogginlägget om Visual Studio-funktioner för att analysera hanterade minnesproblem.
* [Azure web apps online verktyg du bör känna till om](https://azure.microsoft.com/blog/2014/03/28/windows-azure-websites-online-tools-you-should-know-about-2/). Blogginlägget Amit Apple.

Starta en tråd för hjälp med en specifik felsökning fråga i ett av följande forum hello:

* [hello Azure-forum på hello ASP.NET-webbplats](http://forums.asp.net/1247.aspx/1?Azure+and+ASP+NET).
* [hello Azure-forum på MSDN](http://social.msdn.microsoft.com/Forums/windowsazure/).
* [StackOverflow.com](http://www.stackoverflow.com).

### <a name="debugging-in-visual-studio"></a>Felsökning i Visual Studio
Mer information om hur toouse felsökningsläge i Visual Studio finns hello [felsökning i Visual Studio](http://msdn.microsoft.com/library/vstudio/sc65sadd.aspx) MSDN-avsnitt och [felsökning Tips med Visual Studio 2010](http://weblogs.asp.net/scottgu/archive/2010/08/18/debugging-tips-with-visual-studio-2010.aspx).

### <a name="remote-debugging-in-azure"></a>Fjärr-felsökning i Azure
Mer information om fjärrfelsökning för Azure-webbappar och WebJobs finns hello följande resurser:

* [Introduktion tooRemote felsökning Azure App Service Web Apps](https://azure.microsoft.com/blog/2014/05/06/introduction-to-remote-debugging-on-azure-web-sites/).
* [Introduktion tooRemote felsökning Azure App Service Web Apps del 2 - inuti fjärrfelsökning](https://azure.microsoft.com/blog/2014/05/07/introduction-to-remote-debugging-azure-web-sites-part-2-inside-remote-debugging/)
* [Introduktion tooRemote felsökning på Azure App Service Web Apps del 3 - miljö med flera instanser och GIT](https://azure.microsoft.com/blog/2014/05/08/introduction-to-remote-debugging-on-azure-web-sites-part-3-multi-instance-environment-and-git/)
* [WebJobs felsökning (video)](https://www.youtube.com/watch?v=ncQm9q5ZFZs&list=UU_SjTh-ZltPmTYzAybypB-g&index=1)

Om ditt program använder ett Azure Web API eller Mobile Services backend- och du behöver toodebug som finns i [felsökning .NET-serverdel i Visual Studio](http://blogs.msdn.com/b/azuremobile/archive/2014/03/14/debugging-net-backend-in-visual-studio.aspx).

### <a name="tracing-in-aspnet-applications"></a>Spårning i ASP.NET-program
Det finns inga omfattande och uppdaterad introduktioner tooASP.NET spårning på hello Internet. hello bästa du kan göra är Kom igång med gamla inledande material som skrivits för Web Forms eftersom MVC inte finns ännu och tillägg som med nyare blogg Överför till överföringswebbadressen som fokuserar på specifika problem. Vissa lämpliga platser toostart är hello följande resurser:

* [Övervakning och telemetri (skapa verkliga Molnappar med Azure)](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry).<br>
  E-bok kapitel med rekommendationer för spårning i Azure-molnprogram.
* [ASP.NET-spårning](http://msdn.microsoft.com/library/ms972204.aspx)<br/>
  Gamla men fortfarande en bra resurs för en grundläggande introduktion toohello ämne.
* [Samlingsspårlyssnarna](http://msdn.microsoft.com/library/4y5y10s7.aspx)<br/>
  Information om Samlingsspårlyssnarna men inte nämnt hello [WebPageTraceListener](http://msdn.microsoft.com/library/system.web.webpagetracelistener.aspx).
* [Genomgång: Integrera ASP.NET-spårning med System.Diagnostics spårning](http://msdn.microsoft.com/library/b0ectfxd.aspx)<br/>
  Detta för är gamla, men innehåller ytterligare information hello inledande artikeln täcker inte.
* [Spårning i ASP.NET MVC Razor vyer](http://blogs.msdn.com/b/webdev/archive/2013/07/16/tracing-in-asp-net-mvc-razor-views.aspx)<br/>
  Utöver spårning i Razor vyer beskriver hello post även hur toocreate ett fel filter i ordning toolog alla ohanterade undantag i ett MVC-program. Information om hur alla toolog ohanterat undantag i ett Web Forms-program finns i hello Global.asax under [komplett exempel felhanterare](http://msdn.microsoft.com/library/bb397417.aspx) på MSDN. I MVC eller Web Forms om du vill toolog vissa undantag men låta hello standard framework hantering börja gälla för dem att fånga och rethrow som i följande exempel hello:

        try
        {
           // Your code that might cause an exception toobe thrown.
        }
        catch (Exception ex)
        {
            Trace.TraceError("Exception: " + ex.ToString());
            throw;
        }
* [Strömning diagnostik Trace loggar från hello Azure Command Line (plus uppfattning!)](http://www.hanselman.com/blog/StreamingDiagnosticsTraceLoggingFromTheAzureCommandLinePlusGlimpse.aspx)<br/>
  Hur toouse hello kommandoraden toodo i den här självstudiekursen visas hur toodo i Visual Studio. [Uppfattning](http://www.hanselman.com/blog/IfYoureNotUsingGlimpseWithASPNETForDebuggingAndProfilingYoureMissingOut.aspx) är ett verktyg för felsökning av ASP.NET-program.
* [Med Web Apps-loggning och diagnostik - med David Ebbo](/documentation/videos/azure-web-site-logging-and-diagnostics/) och [Direktuppspelningsloggar från Web Apps - med David Ebbo](/documentation/videos/log-streaming-with-azure-web-sites/)<br>
  Videor av Scott hanselman, så får och David Ebbo.

För felloggning är en alternativ toowriting spårning koden är toouse öppen källkod loggningsramverk som [ELMAH](http://nuget.org/packages/elmah/). Mer information finns i [Scott Hanselman blogginlägg om ELMAH](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx).

Observera också att du inte har toouse ASP.NET eller System.Diagnostics spårning om du vill tooget strömning loggar från Azure. hello Azure-webbapp strömning Loggtjänsten direktuppspelas någon *.txt*, *.html*, eller *.log* filen som hittas i hello *loggfiler* mapp. Därför kan du skapa egna loggning system som skriver toohello filsystem hello webbapp och din fil automatiskt strömmas och hämtas. Du har toodo är skrivåtgärder programkod som skapar filer i hello *d:\home\logfiles* mapp.

### <a name="analyzing-web-server-logs"></a>När webbserverloggar
Mer information om hur du analyserar webbserverloggar finns hello följande resurser:

* [LogParser](http://www.microsoft.com/download/details.aspx?id=24659)<br/>
  Ett verktyg för att visa data i webbserverloggar (*.log* filer).
* [Felsöka IIS prestandaproblem och programfel med LogParser](http://www.iis.net/learn/troubleshoot/performance-issues/troubleshooting-iis-performance-issues-or-application-errors-using-logparser)<br/>
  En introduktion toohello Loggparser verktyg som du kan använda tooanalyze webbservern loggar.
* [Blogginlägg av Robert McMurray med hjälp av LogParser](http://blogs.msdn.com/b/robert_mcmurray/archive/tags/logparser/)<br/>
* [hello HTTP-statuskod i IIS 7.0, IIS 7.5 och IIS 8.0](http://support.microsoft.com/kb/943891)

### <a name="analyzing-failed-request-tracing-logs"></a>Det gick inte att analysera spårningsloggar för begäran
hello Microsoft TechNet-webbplatsen innehåller en [med hjälp av spårning av misslyckade begäranden](http://www.iis.net/learn/troubleshoot/using-failed-request-tracing) avsnitt som kan vara till hjälp för att förstå hur toouse loggarna. Men fokuserar den här dokumentationen huvudsakligen om att konfigurera spårning av misslyckade begäranden i IIS, som inte är möjligt i Azure Web Apps.

[GetStarted]: app-service-web-get-started-dotnet.md
[GetStartedWJ]: websites-dotnet-webjobs-sdk.md
