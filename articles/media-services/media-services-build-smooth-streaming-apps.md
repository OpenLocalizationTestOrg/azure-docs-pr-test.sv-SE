---
title: "Utjämna strömning vägledning för Windows Store-App | Microsoft Docs"
description: "Lär dig hur du använder Azure Media Services för att skapa ett C# Windows Store-program med en XML-MediaElement kontroll för att spela upp Smooth Stream innehåll."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 0fa5d8c5-3d5f-4886-ae55-fb6de4f5256d
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/11/2017
ms.author: juliako
ms.openlocfilehash: b4f8855fe6480bc58acfbbb53819f6eabe362bdb
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-build-a-smooth-streaming-windows-store-application"></a>Hur du skapar en Smooth Streaming Windows Store-programmet

Smooth Streaming-klient SDK för Windows 8 kan utvecklare skapa Windows Store-program som kan spela upp på begäran och live Smooth Streaming-innehåll. Förutom grundläggande uppspelning av Smooth Streaming innehåll SDK innehåller också omfattande funktioner som Microsoft PlayReady skydd, kvaliteten begränsning, Live DVR ljud strömma växlar, lyssnar efter statusuppdateringar (till exempel kvalitet ändras) och felhändelser och så vidare. Mer information för funktionerna som stöds finns i [viktig information](http://www.iis.net/learn/media/smooth-streaming/smooth-streaming-client-sdk-for-windows-8-release-notes). Mer information finns i [Player Framework för Windows 8](http://playerframework.codeplex.com/). 

Den här självstudiekursen innehåller fyra erfarenheter:

1. Skapa ett grundläggande Smooth Streaming Store-program
2. Lägg till en skjutreglaget för att styra Media pågår
3. Välj Smooth Streaming-dataströmmar
4. Välj Smooth Streaming spårar

## <a name="prerequisites"></a>Krav
> [!NOTE]
> Windows Store projekt version 8.1 och tidigare stöds inte i Visual Studio-2017.  Mer information finns i [Visual Studio 2017 Platform Targeting and Compatibility](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs) (Visual Studio 2017 – målplattform och plattformskompatibilitet).

* Windows 8, 32-bitars eller 64-bitars.
* Versioner för Visual Studio 2012 via 2015.
* [Microsoft Smooth Streaming klient-SDK för Windows 8](http://visualstudiogallery.msdn.microsoft.com/04423d13-3b3e-4741-a01c-1ae29e84fea6?SRC=Homehttp://visualstudiogallery.msdn.microsoft.com/04423d13-3b3e-4741-a01c-1ae29e84fea6?SRC=Home).

Den färdiga lösningen för varje lektionen kan hämtas från MSDN Developer kodexempel (Code Gallery): 

* [Lektion 1](http://code.msdn.microsoft.com/Smooth-Streaming-Client-0bb1471f) – en enkel Windows 8 Smooth Streaming Media Player 
* [Lektion 2](http://code.msdn.microsoft.com/A-simple-Windows-8-Smooth-ee98f63a) – en enkel Windows 8 Smooth Streaming Media Player med en skjutreglaget kontroll, 
* [Lektion 3](http://code.msdn.microsoft.com/A-Windows-8-Smooth-883c3b44) – en Windows 8-Smooth Streaming Media Player med dataströmmen valet  
* [Lektion 4](http://code.msdn.microsoft.com/A-Windows-8-Smooth-aa9e4907) – en Windows 8-Smooth Streaming Media Player med spåra valet.

## <a name="lesson-1-create-a-basic-smooth-streaming-store-application"></a>Lektionen 1: Skapa ett grundläggande Smooth Streaming Store-program

I kursen skapar du en Windows Store-programmet med en MediaElement kontroll att spela upp Smooth Stream innehåll.  Det ser ut så körs programmet:

![Exempel på ett Smooth Streaming Windows Store][PlayerApplication]

Mer information om hur du utvecklar Windows Store-programmet finns [utveckla bra appar för Windows 8](http://msdn.microsoft.com/windows/apps/br229512.aspx). Den här lektionen innehåller följande procedurer:

1. Skapa ett projekt för Windows Store
2. Utforma användargränssnitt (XAML)
3. Ändra koden bakom filen
4. Kompilera och testa programmet

**Skapa ett projekt för Windows Store**

1. Kör Visual Studio; 2012 via 2015-versioner stöds.
2. Klicka på **Nytt** på menyn **ARKIV** och klicka sedan på **Projekt**.
3. I dialogrutan Nytt projekt ange eller Välj följande värden:

| Namn | Värde |
| --- | --- |
| Mallgrupp |Installerat/mallar/Visual C# / Windows Store |
| Mall |Tom App (XAML) |
| Namn |SSPlayer |
| Plats |C:\SSTutorials |
| Lösningens namn |SSPlayer |
| Skapa katalog för lösning |(valt) |

1. Klicka på **OK**.

**Lägga till en referens till Smooth Streaming-klient SDK**

1. Från Solution Explorer högerklickar du på **SSPlayer**, och klicka sedan på **Lägg till referens**.
2. Ange eller välj följande värden:

| Namn | Värde |
| --- | --- |
| Referensgrupp |Windows-tillägg |
| Referens |Välj Microsoft Smooth Streaming klient-SDK för Windows 8 och Microsoft Visual C++ Runtime-paketet |

1. Klicka på **OK**. 

När du lägger till referenserna, måste du välja målplattformen (x64 eller x86), lägga till referenser fungerar inte för någon CPU plattformskonfigurationen.  I solution explorer visas gul varning markering för dessa lagts till referenser.

**Att utforma användargränssnittet player**

1. Från Solution Explorer dubbelklickar du på **MainPage.xaml** att öppna den i designvyn.
2. Leta upp den  **&lt;rutnätet&gt;**  och  **&lt;/Grid&gt;**  taggar XAML-fil och klistra in följande kod mellan de två taggarna:

         <Grid.RowDefinitions>

            <RowDefinition Height="20"/>    <!-- spacer -->
            <RowDefinition Height="50"/>    <!-- media controls -->
            <RowDefinition Height="100*"/>  <!-- media element -->
            <RowDefinition Height="80*"/>   <!-- media stream and track selection -->
            <RowDefinition Height="50"/>    <!-- status bar -->
         </Grid.RowDefinitions>

         <StackPanel Name="spMediaControl" Grid.Row="1" Orientation="Horizontal">
            <TextBlock x:Name="tbSource" Text="Source :  " FontSize="16" FontWeight="Bold" VerticalAlignment="Center" />
            <TextBox x:Name="txtMediaSource" Text="http://ecn.channel9.msdn.com/o9/content/smf/smoothcontent/elephantsdream/Elephants_Dream_1024-h264-st-aac.ism/manifest" FontSize="10" Width="700" Margin="0,4,0,10" />
            <Button x:Name="btnSetSource" Content="Set Source" Width="111" Height="43" Click="btnSetSource_Click"/>
            <Button x:Name="btnPlay" Content="Play" Width="111" Height="43" Click="btnPlay_Click"/>
            <Button x:Name="btnPause" Content="Pause"  Width="111" Height="43" Click="btnPause_Click"/>
            <Button x:Name="btnStop" Content="Stop"  Width="111" Height="43" Click="btnStop_Click"/>
            <CheckBox x:Name="chkAutoPlay" Content="Auto Play" Height="55" Width="Auto" IsChecked="{Binding AutoPlay, ElementName=mediaElement, Mode=TwoWay}"/>
            <CheckBox x:Name="chkMute" Content="Mute" Height="55" Width="67" IsChecked="{Binding IsMuted, ElementName=mediaElement, Mode=TwoWay}"/>
         </StackPanel>

         <StackPanel Name="spMediaElement" Grid.Row="2" Height="435" Width="1072"
                    HorizontalAlignment="Center" VerticalAlignment="Center">
            <MediaElement x:Name="mediaElement" Height="356" Width="924" MinHeight="225"
                          HorizontalAlignment="Center" VerticalAlignment="Center" 
                          AudioCategory="BackgroundCapableMedia" />
            <StackPanel Orientation="Horizontal">
                <Slider x:Name="sliderProgress" Width="924" Height="44"
                        HorizontalAlignment="Center" VerticalAlignment="Center"
                        PointerPressed="sliderProgress_PointerPressed"/>
                <Slider x:Name="sliderVolume" 
                        HorizontalAlignment="Right" VerticalAlignment="Center" Orientation="Vertical" 
                        Height="79" Width="148" Minimum="0" Maximum="1" StepFrequency="0.1" 
                        Value="{Binding Volume, ElementName=mediaElement, Mode=TwoWay}" 
                        ToolTipService.ToolTip="{Binding Value, RelativeSource={RelativeSource Mode=Self}}"/>
            </StackPanel>
         </StackPanel>

         <StackPanel Name="spStatus" Grid.Row="4" Orientation="Horizontal">
            <TextBlock x:Name="tbStatus" Text="Status :  " 
               FontSize="16" FontWeight="Bold" VerticalAlignment="Center" HorizontalAlignment="Center" />
            <TextBox x:Name="txtStatus" FontSize="10" Width="700" VerticalAlignment="Center"/>
         </StackPanel>
   
   MediaElement kontrollen används för att spela upp media. Skjutreglaget med namnet sliderProgress används i kursen för att styra media förloppet.
3. Tryck på **CTRL + S** att spara filen.

MediaElement-kontrollen stöder inte Smooth Streaming innehåll out-of-box. Om du vill aktivera stöd för Smooth Streaming, måste du registrera Smooth Streaming byte-dataström hanteraren av filnamnstillägg och MIME-typen.  Om du vill registrera, kan du använda metoden MediaExtensionManager.RegisterByteStremHandler i namnområdet Windows.Media.

I XAML-filen är vissa händelsehanterare associerad med kontroller.  Du måste definiera dessa händelsehanterare.

**Att ändra koden bakom filen**

1. Från Solution Explorer högerklickar du på **MainPage.xaml**, och klicka sedan på **Visa kod**.
2. Längst upp i filen, lägger du till följande med instruktionen:
   
        using Windows.Media;
3. I början av den **MainPage** class, Lägg till följande datamedlemmen:
   
         private MediaExtensionManager extensions = new MediaExtensionManager();
4. I slutet av den **MainPage** konstruktor, Lägg till följande två rader:
   
        extensions.RegisterByteStreamHandler("Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", ".ism", "text/xml");
        extensions.RegisterByteStreamHandler("Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", ".ism", "application/vnd.ms-sstr+xml");
5. I slutet av den **MainPage** klassen, klistra in följande kod:
   
         # region UI Button Click Events
         private void btnPlay_Click(object sender, RoutedEventArgs e)
         {

         mediaElement.Play();
         txtStatus.Text = "MediaElement is playing ...";
         }
         private void btnPause_Click(object sender, RoutedEventArgs e)
         {

         mediaElement.Pause();
         txtStatus.Text = "MediaElement is paused";
         }
         private void btnSetSource_Click(object sender, RoutedEventArgs e)
         {

         sliderProgress.Value = 0;
         mediaElement.Source = new Uri(txtMediaSource.Text);

         if (chkAutoPlay.IsChecked == true)
         {
             txtStatus.Text = "MediaElement is playing ...";
         }
         else
         {
             txtStatus.Text = "Click the Play button to play the media source.";
         }
         }
         private void btnStop_Click(object sender, RoutedEventArgs e)
         {

         mediaElement.Stop();
         txtStatus.Text = "MediaElement is stopped";
         }
         private void sliderProgress_PointerPressed(object sender, PointerRoutedEventArgs e)
         {

         txtStatus.Text = "Seek to position " + sliderProgress.Value;
         mediaElement.Position = new TimeSpan(0, 0, (int)(sliderProgress.Value));
         }
         # endregion

Händelsehanteraren sliderProgress_PointerPressed definieras här.  Det finns flera fungerar för att det ska fungera, vilket beskrivs i kursen i den här kursen.
6. Tryck på **CTRL + S** att spara filen.

Den färdiga koden bakom filen skall se ut så här:

![Codeview i Visual Studio för Smooth Streaming Windows Store-programmet][CodeViewPic]

**Att kompilera och testa programmet**

1. Från den **skapa** -menyn klickar du på **Configuration Manager**.
2. Ändra **aktiv lösning plattform** som matchar din plattform.
3. Tryck på **F6** att kompilera projektet. 
4. Tryck på **F5** för att köra programmet.
5. Längst upp i programmet du använder standard-URL för Smooth Streaming eller ange ett annat namn. 
6. Klicka på **Ange källa**. Eftersom **Spela automatiskt** är aktiverad som standard mediet skall spelas upp automatiskt.  Du kan styra media med hjälp av den **spela upp**, **pausa** och **stoppa** knappar.  Du kan styra media volymen med lodräta skjutreglaget.  Men vågräta skjutreglaget för förloppet media är ännu inte helt implementerat. 

Du har slutfört lesson1.  Nu bör använda du en MediaElement kontroll att spela upp Smooth Streaming innehåll.  I kursen läggs ett skjutreglage för att kontrollera förloppet för Smooth Streaming-innehåll.

## <a name="lesson-2-add-a-slider-bar-to-control-the-media-progress"></a>Lektionen 2: Lägg till en skjutreglaget för att styra Media pågår

I lektionen 1 skapat du ett Windows Store-program med en MediaElement XAML-kontroll för att spela upp Smooth Streaming medieinnehåll.  Det gäller vissa grundläggande media-funktioner som starta, stoppa och pausa.  Nu bör du till en kontroll för skjutreglage fältet till programmet.

I den här självstudiekursen kommer använder vi en timer för att uppdatera skjutreglaget positionen baserat på den aktuella positionen i kontrollen MediaElement.  Skjutreglaget start- och -tid som också behöver uppdateras vid levande innehåll.  Detta kan hanteras bättre i händelsen anpassningsbar källa uppdateringen.

Mediekällor är objekt som genererar media data.  Käll-matcharen tar en URL eller byte-dataström och skapar lämpligt mediekälla för innehållet.  Käll-matcharen är standardmetoden för program för att skapa media källor. 

Den här lektionen innehåller följande procedurer:

1. Registrera Smooth Streaming-hanteraren 
2. Lägga till anpassade källan manager nivån händelsehanterare
3. Lägga till de anpassade källan nivån händelsehanterarna
4. Lägga till MediaElement händelsehanterare
5. Lägg till skjutreglaget relaterade streckkod
6. Kompilera och testa programmet

**Att registrera hanteraren Smooth Streaming byte-dataström och skicka propertyset**

1. Från Solution Explorer högerklickar du **MainPage.xaml**, och klicka sedan på **Visa kod**.
2. I början av filen lägger du till följande med instruktionen:

        using Microsoft.Media.AdaptiveStreaming;
3. Lägg till följande datamedlemmar i början av klassen MainPage:

         private Windows.Foundation.Collections.PropertySet propertySet = new Windows.Foundation.Collections.PropertySet();             
         private IAdaptiveSourceManager adaptiveSourceManager;
4. I den **MainPage** konstruktor, Lägg till följande kod efter den **detta. Initiera Components();**  rad och registreringen Platskod raderna skrivna i förra lektionen:

        // Gets the default instance of AdaptiveSourceManager which manages Smooth 
        //Streaming media sources.
        adaptiveSourceManager = AdaptiveSourceManager.GetDefault();
        // Sets property key value to AdaptiveSourceManager default instance.
        // {A5CE1DE8-1D00-427B-ACEF-FB9A3C93DE2D}" must be hardcoded.
        propertySet["{A5CE1DE8-1D00-427B-ACEF-FB9A3C93DE2D}"] = adaptiveSourceManager;
5. I den **MainPage** konstruktor, ändra de två RegisterByteStreamHandler-metoderna för att lägga till den tillbaka parametrar:

         // Registers Smooth Streaming byte-stream handler for ".ism" extension and, 
         // "text/xml" and "application/vnd.ms-ss" mime-types and pass the propertyset. 
         // http://*.ism/manifest URI resources will be resolved by Byte-stream handler.
         extensions.RegisterByteStreamHandler(

            "Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", 
            ".ism", 
            "text/xml", 
            propertySet );
         extensions.RegisterByteStreamHandler(

            "Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", 
            ".ism", 
            "application/vnd.ms-sstr+xml", 
         propertySet);
6. Tryck på **CTRL + S** att spara filen.

**Att lägga till händelsehanteraren anpassningsbar källa manager nivå**

1. Från Solution Explorer högerklickar du **MainPage.xaml**, och klicka sedan på **Visa kod**.
2. I den **MainPage** class, Lägg till följande datamedlemmen:
   
     privata AdaptiveSource adaptiveSource = null;
3. I slutet av den **MainPage** class, Lägg till följande händelsehanteraren:
   
         # region Adaptive Source Manager Level Events
         private void mediaElement_AdaptiveSourceOpened(AdaptiveSource sender, AdaptiveSourceOpenedEventArgs args)
         {

            adaptiveSource = args.AdaptiveSource;
         }

         # endregion Adaptive Source Manager Level Events
4. I slutet av den **MainPage** konstruktor, Lägg till följande rad att prenumerera på anpassningsbar källa Öppna händelsen:
   
         adaptiveSourceManager.AdaptiveSourceOpenedEvent += 
           new AdaptiveSourceOpenedEventHandler(mediaElement_AdaptiveSourceOpened);
5. Tryck på **CTRL + S** att spara filen.

**Att lägga till anpassade källan nivån händelsehanterare**

1. Från Solution Explorer högerklickar du **MainPage.xaml**, och klicka sedan på **Visa kod**.
2. I den **MainPage** class, Lägg till följande datamedlemmen:
   
     privata AdaptiveSourceStatusUpdatedEventArgs adaptiveSourceStatusUpdate;   privata manifestet manifestObject;
3. I slutet av den **MainPage** class, lägga till följande händelsehanterare:

         # region Adaptive Source Level Events
         private void mediaElement_ManifestReady(AdaptiveSource sender, ManifestReadyEventArgs args)
         {

            adaptiveSource = args.AdaptiveSource;
            manifestObject = args.AdaptiveSource.Manifest;
         }

         private void mediaElement_AdaptiveSourceStatusUpdated(AdaptiveSource sender, AdaptiveSourceStatusUpdatedEventArgs args)
         {

            adaptiveSourceStatusUpdate = args;
         }

         private void mediaElement_AdaptiveSourceFailed(AdaptiveSource sender, AdaptiveSourceFailedEventArgs args)
         {

            txtStatus.Text = "Error: " + args.HttpResponse;
         }

         # endregion Adaptive Source Level Events
4. I slutet av den **mediaElement AdaptiveSourceOpened** metoden Lägg till följande kod för att prenumerera på händelser:
   
         adaptiveSource.ManifestReadyEvent +=

                    mediaElement_ManifestReady;
         adaptiveSource.AdaptiveSourceStatusUpdatedEvent += 

            mediaElement_AdaptiveSourceStatusUpdated;
         adaptiveSource.AdaptiveSourceFailedEvent += 

            mediaElement_AdaptiveSourceFailed;
5. Tryck på **CTRL + S** att spara filen.

Samma händelser som finns på anpassningsbar källa Manager nivå, som kan användas för att hantera funktioner som är gemensamma för alla mediaelement i appen. Varje AdaptiveSource innehåller sina egna händelser och alla AdaptiveSource händelser ska vara överlappande under AdaptiveSourceManager.

**Att lägga till mediaelement händelsehanterare**

1. Från Solution Explorer högerklickar du **MainPage.xaml**, och klicka sedan på **Visa kod**.
2. I slutet av den **MainPage** class, lägga till följande händelsehanterare:

         # region Media Element Event Handlers
         private void MediaOpened(object sender, RoutedEventArgs e)
         {

            txtStatus.Text = "MediaElement opened";
         }

         private void MediaFailed(object sender, ExceptionRoutedEventArgs e)
         {

            txtStatus.Text= "MediaElement failed: " + e.ErrorMessage;
         }

         private void MediaEnded(object sender, RoutedEventArgs e)
         {

            txtStatus.Text ="MediaElement ended.";
         }

         # endregion Media Element Event Handlers
3. I slutet av den **MainPage** konstruktor, Lägg till följande kod i nedsänkt till händelser:

         mediaElement.MediaOpened += MediaOpened;
         mediaElement.MediaEnded += MediaEnded;
         mediaElement.MediaFailed += MediaFailed;
4. Tryck på **CTRL + S** att spara filen.

**Att lägga till skjutreglaget relaterade kod**

1. Från Solution Explorer högerklickar du **MainPage.xaml**, och klicka sedan på **Visa kod**.
2. I början av filen lägger du till följande med instruktionen:
      
        using Windows.UI.Core;
3. I den **MainPage** class, Lägg till följande datamedlemmar:
   
         public static CoreDispatcher _dispatcher;
         private DispatcherTimer sliderPositionUpdateDispatcher;
4. I slutet av den **MainPage** konstruktor, Lägg till följande kod:
   
         _dispatcher = Window.Current.Dispatcher;
         PointerEventHandler pointerpressedhandler = new PointerEventHandler(sliderProgress_PointerPressed);
         sliderProgress.AddHandler(Control.PointerPressedEvent, pointerpressedhandler, true);    
5. I slutet av den **MainPage** klassen och Lägg till följande kod:

         # region sliderMediaPlayer
         private double SliderFrequency(TimeSpan timevalue)
         {

            long absvalue = 0;
            double stepfrequency = -1;

            if (manifestObject != null)
            {
                absvalue = manifestObject.Duration - (long)manifestObject.StartTime;
            }
            else
            {
                absvalue = mediaElement.NaturalDuration.TimeSpan.Ticks;
            }

            TimeSpan totalDVRDuration = new TimeSpan(absvalue);

            if (totalDVRDuration.TotalMinutes >= 10 && totalDVRDuration.TotalMinutes < 30)
            {
               stepfrequency = 10;
            }
            else if (totalDVRDuration.TotalMinutes >= 30 
                     && totalDVRDuration.TotalMinutes < 60)
            {
                stepfrequency = 30;
            }
            else if (totalDVRDuration.TotalHours >= 1)
            {
                stepfrequency = 60;
            }

            return stepfrequency;
         }

         void updateSliderPositionoNTicks(object sender, object e)
         {

            sliderProgress.Value = mediaElement.Position.TotalSeconds;
         }

         public void setupTimer()
         {

            sliderPositionUpdateDispatcher = new DispatcherTimer();
            sliderPositionUpdateDispatcher.Interval = new TimeSpan(0, 0, 0, 0, 300);
            startTimer();
         }

         public void startTimer()
         {

            sliderPositionUpdateDispatcher.Tick += updateSliderPositionoNTicks;
            sliderPositionUpdateDispatcher.Start();
         }

         // Slider start and end time must be updated in case of live content
         public async void setSliderStartTime(long startTime)
         {

            await _dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
            {
                TimeSpan timespan = new TimeSpan(adaptiveSourceStatusUpdate.StartTime);
                double absvalue = (int)Math.Round(timespan.TotalSeconds, MidpointRounding.AwayFromZero);
                sliderProgress.Minimum = absvalue;
            });
         }

         // Slider start and end time must be updated in case of live content
         public async void setSliderEndTime(long startTime)
         {

            await _dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
            {
                TimeSpan timespan = new TimeSpan(adaptiveSourceStatusUpdate.EndTime);
                double absvalue = (int)Math.Round(timespan.TotalSeconds, MidpointRounding.AwayFromZero);
                sliderProgress.Maximum = absvalue;
            });
         }

         # endregion sliderMediaPlayer
      
>[!NOTE]
>CoreDispatcher används för att göra ändringar i UI-tråden från icke-UI-tråden. Utvecklare kan välja att använda dispatcher som tillhandahålls av UI-element som han eller hon har för avsikt att uppdatera vid flaskhals i dispatcher-tråden.  Exempel:
   
         await sliderProgress.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () => { TimeSpan 

         timespan = new TimeSpan(adaptiveSourceStatusUpdate.EndTime); 
         double absvalue  = (int)Math.Round(timespan.TotalSeconds, MidpointRounding.AwayFromZero); 

         sliderProgress.Maximum = absvalue; }); 
6. I slutet av den **mediaElement_AdaptiveSourceStatusUpdated** metod, Lägg till följande kod:

         setSliderStartTime(args.StartTime);
         setSliderEndTime(args.EndTime);
7. I slutet av den **MediaOpened** metod, Lägg till följande kod:

         sliderProgress.StepFrequency = SliderFrequency(mediaElement.NaturalDuration.TimeSpan);
         sliderProgress.Width = mediaElement.Width;
         setupTimer();
8. Tryck på **CTRL + S** att spara filen.

**Att kompilera och testa programmet**

1. Tryck på **F6** att kompilera projektet. 
2. Tryck på **F5** för att köra programmet.
3. Längst upp i programmet du använder standard-URL för Smooth Streaming eller ange ett annat namn. 
4. Klicka på **Ange källa**. 
5. Testa skjutreglaget.

Du har slutfört lektionen 2.  I den här lektionen läggs en skjutreglaget till programmet. 

## <a name="lesson-3-select-smooth-streaming-streams"></a>Lektionen 3: Välj Smooth Streaming-dataströmmar
Smooth Streaming är kapabel att strömma innehåll med flera språk ljud spår som är valbara av visningsprogram.  I kursen ska du aktivera visningsprogram att välja dataströmmar. Den här lektionen innehåller följande procedurer:

1. Ändra XAML-fil
2. Ändra behand kodfilen
3. Kompilera och testa programmet

**Att ändra XAML-fil**

1. Från Solution Explorer högerklickar du på **MainPage.xaml**, och klicka sedan på **Vydesigner**.
2. Leta upp &lt;Grid.RowDefinitions&gt;, och ändra RowDefinitions så de ser ut som:
   
         <Grid.RowDefinitions>            
            <RowDefinition Height="20"/>
            <RowDefinition Height="50"/>
            <RowDefinition Height="100"/>
            <RowDefinition Height="80"/>
            <RowDefinition Height="50"/>
         </Grid.RowDefinitions>
3. I den &lt;rutnätet&gt;&lt;/Grid&gt; taggar, Lägg till följande kod för att definiera en listruta så att användarna kan se en lista över tillgängliga dataströmmar och välja dataströmmar:

         <Grid Name="gridStreamAndBitrateSelection" Grid.Row="3">
            <Grid.RowDefinitions>
                <RowDefinition Height="300"/>
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="250*"/>
                <ColumnDefinition Width="250*"/>
            </Grid.ColumnDefinitions>
            <StackPanel Name="spStreamSelection" Grid.Row="1" Grid.Column="0">
                <StackPanel Orientation="Horizontal">
                    <TextBlock Name="tbAvailableStreams" Text="Available Streams:" FontSize="16" VerticalAlignment="Center"></TextBlock>
                    <Button Name="btnChangeStreams" Content="Submit" Click="btnChangeStream_Click"/>
                </StackPanel>
                <ListBox x:Name="lbAvailableStreams" Height="200" Width="200" Background="Transparent" HorizontalAlignment="Left" 
                    ScrollViewer.VerticalScrollMode="Enabled" ScrollViewer.VerticalScrollBarVisibility="Visible">
                    <ListBox.ItemTemplate>
                        <DataTemplate>
                            <CheckBox Content="{Binding Name}" IsChecked="{Binding isChecked, Mode=TwoWay}" />
                        </DataTemplate>
                    </ListBox.ItemTemplate>
                </ListBox>
            </StackPanel>
         </Grid>
4. Tryck på **CTRL + S** spara ändringarna.

**Att ändra koden bakom filen**

1. Från Solution Explorer högerklickar du på **MainPage.xaml**, och klicka sedan på **Visa kod**.
2. Lägg till en ny klass i namnområdet SSPlayer:
   
        #region class Stream
   
        public class Stream
        {
            private IManifestStream stream;
            public bool isCheckedValue;
            public string name;
   
            public string Name
            {
                get { return name; }
                set { name = value; }
            }
   
            public IManifestStream ManifestStream
            {
                get { return stream; }
                set { stream = value; }
            }
   
            public bool isChecked
            {
                get { return isCheckedValue; }
                set
                {
                    // mMke the video stream always checked.
                    if (stream.Type == MediaStreamType.Video)
                    {
                        isCheckedValue = true;
                    }
                    else
                    {
                        isCheckedValue = value;
                    }
                }
            }
   
            public Stream(IManifestStream streamIn)
            {
                stream = streamIn;
                name = stream.Name;
            }
        }
        #endregion class Stream
3. Lägg till följande definitioner för variabeln i början av klassen MainPage:
   
         private List<Stream> availableStreams;
         private List<Stream> availableAudioStreams;
         private List<Stream> availableTextStreams;
         private List<Stream> availableVideoStreams;
4. Lägg till följande region i MainPage-klassen:
   
        #region stream selection
        ///<summary>
        ///Functionality to select streams from IManifestStream available streams
        /// </summary>
   
        // This function is called from the mediaElement_ManifestReady event handler 
        // to retrieve the streams and populate them to the local data members.
        public void getStreams(Manifest manifestObject)
        {
            availableStreams = new List<Stream>();
            availableVideoStreams = new List<Stream>();
            availableAudioStreams = new List<Stream>();
            availableTextStreams = new List<Stream>();
   
            try
            {
                for (int i = 0; i<manifestObject.AvailableStreams.Count; i++)
                {
                    Stream newStream = new Stream(manifestObject.AvailableStreams[i]);
                    newStream.isChecked = false;
   
                    //populate the stream lists based on the types
                    availableStreams.Add(newStream);
   
                    switch (newStream.ManifestStream.Type)
                    {
                        case MediaStreamType.Video:
                            availableVideoStreams.Add(newStream);
                            break;
                        case MediaStreamType.Audio:
                            availableAudioStreams.Add(newStream);
                            break;
                        case MediaStreamType.Text:
                            availableTextStreams.Add(newStream);
                            break;
                    }
   
                    // Select the default selected streams from the manifest.
                    for (int j = 0; j<manifestObject.SelectedStreams.Count; j++)
                    {
                        string selectedStreamName = manifestObject.SelectedStreams[j].Name;
                        if (selectedStreamName.Equals(newStream.Name))
                        {
                            newStream.isChecked = true;
                            break;
                        }
                    }
                }
            }
            catch (Exception e)
            {
                txtStatus.Text = "Error: " + e.Message;
            }
        }
   
        // This function set the list box ItemSource
        private async void refreshAvailableStreamsListBoxItemSource()
        {
            try
            {
                //update the stream check box list on the UI
                await _dispatcher.RunAsync(CoreDispatcherPriority.Normal, ()
                    => { lbAvailableStreams.ItemsSource = availableStreams; });
            }
            catch (Exception e)
            {
                txtStatus.Text = "Error: " + e.Message;
            }
        }
   
        // This function creates a selected streams list
        private void createSelectedStreamsList(List<IManifestStream> selectedStreams)
        {
            bool isOneVideoSelected = false;
            bool isOneAudioSelected = false;
   
            // Only one video stream can be selected
            for (int j = 0; j<availableVideoStreams.Count; j++)
            {
                if (availableVideoStreams[j].isChecked && (!isOneVideoSelected))
                {
                    selectedStreams.Add(availableVideoStreams[j].ManifestStream);
                    isOneVideoSelected = true;
                }
            }
   
            // Select the frist video stream from the list if no video stream is selected
            if (!isOneVideoSelected)
            {
                availableVideoStreams[0].isChecked = true;
                selectedStreams.Add(availableVideoStreams[0].ManifestStream);
            }
   
            // Only one audio stream can be selected
            for (int j = 0; j<availableAudioStreams.Count; j++)
            {
                if (availableAudioStreams[j].isChecked && (!isOneAudioSelected))
                {
                    selectedStreams.Add(availableAudioStreams[j].ManifestStream);
                    isOneAudioSelected = true;
                    txtStatus.Text = "The audio stream is changed to " + availableAudioStreams[j].ManifestStream.Name;
                }
            }
   
            // Select the frist audio stream from the list if no audio steam is selected.
            if (!isOneAudioSelected)
            {
                availableAudioStreams[0].isChecked = true;
                selectedStreams.Add(availableAudioStreams[0].ManifestStream);
            }
   
            // Multiple text streams are supported.
            for (int j = 0; j < availableTextStreams.Count; j++)
            {
                if (availableTextStreams[j].isChecked)
                {
                    selectedStreams.Add(availableTextStreams[j].ManifestStream);
                }
            }
        }
   
        // Change streams on a smooth streaming presentation with multiple video streams.
        private async void changeStreams(List<IManifestStream> selectStreams)
        {
            try
            {
                IReadOnlyList<IStreamChangedResult> returnArgs =
                    await manifestObject.SelectStreamsAsync(selectStreams);
            }
            catch (Exception e)
            {
                txtStatus.Text = "Error: " + e.Message;
            }
        }
        #endregion stream selection
5. Leta upp metoden mediaElement_ManifestReady, Lägg till följande kod i slutet av funktionen:
   
        getStreams(manifestObject);
        refreshAvailableStreamsListBoxItemSource();
   
    Så när MediaElement manifestet är klar, koden hämtar en lista över tillgängliga strömmar och fylls listrutan användargränssnitt med listan.
6. Hitta Gränssnittet i klassen MainPage knappar klickar du på händelser region och Lägg sedan till följande funktionsdefinitionen:
   
        private void btnChangeStream_Click(object sender, RoutedEventArgs e)
        {
            List<IManifestStream> selectedStreams = new List<IManifestStream>();
   
            // Create a list of the selected streams
            createSelectedStreamsList(selectedStreams);
   
            // Change streams on the presentation
            changeStreams(selectedStreams);
        }

**Att kompilera och testa programmet**

1. Tryck på **F6** att kompilera projektet. 
2. Tryck på **F5** för att köra programmet.
3. Längst upp i programmet du använder standard-URL för Smooth Streaming eller ange ett annat namn. 
4. Klicka på **Ange källa**. 
5. Standardspråket är audio_eng. Försök att växla mellan audio_eng och audio_es. Varje gång, väljer du en ny ström, du måste klicka på knappen Skicka.

Du har slutfört lektionen 3.  Nu bör du lägga till funktioner för att välja dataströmmar.

## <a name="lesson-4-select-smooth-streaming-tracks"></a>Lektionen 4: Välj Smooth Streaming spårar
En Smooth Streaming-presentation kan innehålla flera videofiler kodad med olika kvalitet (bithastigheter) och lösningar. I kursen kan användare välja spår. Den här lektionen innehåller följande procedurer:

1. Ändra XAML-fil
2. Ändra behand kodfilen
3. Kompilera och testa programmet

**Att ändra XAML-fil**

1. Från Solution Explorer högerklickar du på **MainPage.xaml**, och klicka sedan på **Vydesigner**.
2. Leta upp den &lt;rutnätet&gt; tagg med namnet **gridStreamAndBitrateSelection**, Lägg till följande kod i slutet av taggen:
   
         <StackPanel Name="spBitRateSelection" Grid.Row="1" Grid.Column="1">
         <StackPanel Orientation="Horizontal">
             <TextBlock Name="tbBitRate" Text="Available Bitrates:" FontSize="16" VerticalAlignment="Center"/>
             <Button Name="btnChangeTracks" Content="Submit" Click="btnChangeTrack_Click" />
         </StackPanel>
         <ListBox x:Name="lbAvailableVideoTracks" Height="200" Width="200" Background="Transparent" HorizontalAlignment="Left" 
                  ScrollViewer.VerticalScrollMode="Enabled" ScrollViewer.VerticalScrollBarVisibility="Visible">
             <ListBox.ItemTemplate>
                 <DataTemplate>
                     <CheckBox Content="{Binding Bitrate}" IsChecked="{Binding isChecked, Mode=TwoWay}"/>
                 </DataTemplate>
             </ListBox.ItemTemplate>
         </ListBox>
         </StackPanel>
3. Tryck på **CTRL + S** spara han ändringar

**Att ändra koden bakom filen**

1. Från Solution Explorer högerklickar du på **MainPage.xaml**, och klicka sedan på **Visa kod**.
2. Lägg till en ny klass i namnområdet SSPlayer:
   
        #region class Track
        public class Track
        {
            private IManifestTrack trackInfo;
            public string _bitrate;
            public bool isCheckedValue;
   
            public IManifestTrack TrackInfo
            {
                get { return trackInfo; }
                set { trackInfo = value; }
            }
   
            public string Bitrate
            {
                get { return _bitrate; }
                set { _bitrate = value; }
            }
   
            public bool isChecked
            {
                get { return isCheckedValue; }
                set
                {
                    isCheckedValue = value;
                }
            }
   
            public Track(IManifestTrack trackInfoIn)
            {
                trackInfo = trackInfoIn;
                _bitrate = trackInfoIn.Bitrate.ToString();
            }
            //public Track() { }
        }
        #endregion class Track
3. Lägg till följande definitioner för variabeln i början av klassen MainPage:
   
        private List<Track> availableTracks;
4. Lägg till följande region i MainPage-klassen:
   
        #region track selection
        /// <summary>
        /// Functionality to select video streams
        /// </summary>
   
        /// This Function gets the tracks for the selected video stream
        public void getTracks(Manifest manifestObject)
        {
            availableTracks = new List<Track>();
   
            IManifestStream videoStream = getVideoStream();
            IReadOnlyList<IManifestTrack> availableTracksLocal = videoStream.AvailableTracks;
            IReadOnlyList<IManifestTrack> selectedTracksLocal = videoStream.SelectedTracks;
   
            try
            {
                for (int i = 0; i < availableTracksLocal.Count; i++)
                {
                    Track thisTrack = new Track(availableTracksLocal[i]);
                    thisTrack.isChecked = true;
   
                    for (int j = 0; j < selectedTracksLocal.Count; j++)
                    {
                        string selectedTrackName = selectedTracksLocal[j].Bitrate.ToString();
                        if (selectedTrackName.Equals(thisTrack.Bitrate))
                        {
                            thisTrack.isChecked = true;
                            break;
                        }
                    }
                    availableTracks.Add(thisTrack);
                }
            }
            catch (Exception e)
            {
                txtStatus.Text = e.Message;
            }
        }
   
        // This function gets the video stream that is playing
        private IManifestStream getVideoStream()
        {
            IManifestStream videoStream = null;
            for (int i = 0; i < manifestObject.SelectedStreams.Count; i++)
            {
                if (manifestObject.SelectedStreams[i].Type == MediaStreamType.Video)
                {
                    videoStream = manifestObject.SelectedStreams[i];
                    break;
                }
            }
            return videoStream;
        }
   
        // This function set the UI list box control ItemSource
        private async void refreshAvailableTracksListBoxItemSource()
        {
            try
            {
                // Update the track check box list on the UI 
                await _dispatcher.RunAsync(CoreDispatcherPriority.Normal, ()
                    => { lbAvailableVideoTracks.ItemsSource = availableTracks; });
            }
            catch (Exception e)
            {
                txtStatus.Text = "Error: " + e.Message;
            }        
        }
   
        // This function creates a list of the selected tracks.
        private void createSelectedTracksList(List<IManifestTrack> selectedTracks)
        {
            // Create a list of selected tracks
            for (int j = 0; j < availableTracks.Count; j++)
            {
                if (availableTracks[j].isCheckedValue == true)
                {
                    selectedTracks.Add(availableTracks[j].TrackInfo);
                }
            }
        }
   
        // This function selects the tracks based on user selection 
        private void changeTracks(List<IManifestTrack> selectedTracks)
        {
            IManifestStream videoStream = getVideoStream();
            try
            {
                videoStream.SelectTracks(selectedTracks);
            }
            catch (Exception ex)
            {
                txtStatus.Text = ex.Message;
            }
        }
        #endregion track selection
5. Leta upp metoden mediaElement_ManifestReady, Lägg till följande kod i slutet av funktionen:
   
         getTracks(manifestObject);
         refreshAvailableTracksListBoxItemSource();
6. Hitta Gränssnittet i klassen MainPage knappar klickar du på händelser region och Lägg sedan till följande funktionsdefinitionen:
   
         private void btnChangeStream_Click(object sender, RoutedEventArgs e)
         {
            List<IManifestStream> selectedStreams = new List<IManifestStream>();

            // Create a list of the selected streams
            createSelectedStreamsList(selectedStreams);

            // Change streams on the presentation
            changeStreams(selectedStreams);
         }

**Att kompilera och testa programmet**

1. Tryck på **F6** att kompilera projektet. 
2. Tryck på **F5** för att köra programmet.
3. Längst upp i programmet du använder standard-URL för Smooth Streaming eller ange ett annat namn. 
4. Klicka på **Ange källa**. 
5. Som standard markeras alla spår av video-ström. Om du vill experimentera bitars hastighet ändringarna, kan du välja den lägsta bithastigheten och sedan välja den högsta bithastigheten. Du måste klicka på Skicka efter varje ändring.  Du kan se ändringarna bildkvaliteten.

Du har slutfört lektionen 4.  Nu bör du lägga till funktioner för att välja spår.

## <a name="media-services-learning-paths"></a>Sökvägar för Media Services-utbildning
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="other-resources"></a>Andra resurser:
* [Hur du skapar en Smooth Streaming JavaScript för Windows 8-program med avancerade funktioner](http://blogs.iis.net/cenkd/archive/2012/08/10/how-to-build-a-smooth-streaming-windows-8-javascript-application-with-advanced-features.aspx)
* [Smooth Streaming teknisk översikt](http://www.iis.net/learn/media/on-demand-smooth-streaming/smooth-streaming-technical-overview)

[PlayerApplication]: ./media/media-services-build-smooth-streaming-apps/SSClientWin8-1.png
[CodeViewPic]: ./media/media-services-build-smooth-streaming-apps/SSClientWin8-2.png

