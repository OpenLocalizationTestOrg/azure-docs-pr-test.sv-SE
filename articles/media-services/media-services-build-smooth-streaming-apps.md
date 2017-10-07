---
title: aaaSmooth Streaming Windows Store-App kursen | Microsoft Docs
description: "Lär dig hur toouse Azure Media Services toocreate ett C# Windows Store-program med en XML-MediaElement styra tooplayback Smooth strömma innehåll."
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
ms.openlocfilehash: b02aa2c7f68fe22a23ea846d72fdd23bfba2b19c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toobuild-a-smooth-streaming-windows-store-application"></a>Hur tooBuild ett Smooth Streaming Windows Store-program

hello Smooth Streaming-klient SDK för Windows 8 kan utvecklare toobuild Windows Store-program som kan spela upp på begäran och live Smooth Streaming-innehåll. Dessutom ger toohello grundläggande spela upp Smooth Streaming innehåll, hello SDK också omfattande funktioner som Microsoft PlayReady skydd, kvaliteten begränsning, Live DVR, ljudström växlar, lyssnar efter statusuppdateringar (till exempel kvalitet ändras ) och felhändelser och så vidare. Mer information hello stöds funktioner finns hello [viktig information](http://www.iis.net/learn/media/smooth-streaming/smooth-streaming-client-sdk-for-windows-8-release-notes). Mer information finns i [Player Framework för Windows 8](http://playerframework.codeplex.com/). 

Den här självstudiekursen innehåller fyra erfarenheter:

1. Skapa ett grundläggande Smooth Streaming Store-program
2. Lägg till en reglaget tooControl hello Media pågår
3. Välj Smooth Streaming-dataströmmar
4. Välj Smooth Streaming spårar

## <a name="prerequisites"></a>Krav
* Windows 8, 32-bitars eller 64-bitars. Du kan hämta [utvärdering av Windows 8 Enterprise](http://msdn.microsoft.com/evalcenter/jj554510.aspx) på MSDN.
* Visual Studio 2012 eller Visual Studio Express 2012 (eller en senare version). Du kan hämta hello utvärderingsversion från [här](http://www.microsoft.com/visualstudio/11/downloads).
* [Microsoft Smooth Streaming klient-SDK för Windows 8](http://visualstudiogallery.msdn.microsoft.com/04423d13-3b3e-4741-a01c-1ae29e84fea6?SRC=Homehttp://visualstudiogallery.msdn.microsoft.com/04423d13-3b3e-4741-a01c-1ae29e84fea6?SRC=Home).

hello slutförts lösning för varje lektionen kan hämtas från MSDN Developer kodexempel (Code Gallery): 

* [Lektion 1](http://code.msdn.microsoft.com/Smooth-Streaming-Client-0bb1471f) – en enkel Windows 8 Smooth Streaming Media Player 
* [Lektion 2](http://code.msdn.microsoft.com/A-simple-Windows-8-Smooth-ee98f63a) – en enkel Windows 8 Smooth Streaming Media Player med en skjutreglaget kontroll, 
* [Lektion 3](http://code.msdn.microsoft.com/A-Windows-8-Smooth-883c3b44) – en Windows 8-Smooth Streaming Media Player med dataströmmen valet  
* [Lektion 4](http://code.msdn.microsoft.com/A-Windows-8-Smooth-aa9e4907) – en Windows 8-Smooth Streaming Media Player med spåra valet.

## <a name="lesson-1-create-a-basic-smooth-streaming-store-application"></a>Lektionen 1: Skapa ett grundläggande Smooth Streaming Store-program

I kursen skapar du en Windows Store-programmet med ett MediaElement kontrollen tooplay Smooth Stream innehåll.  Det ser ut så hello program som körs:

![Exempel på ett Smooth Streaming Windows Store][PlayerApplication]

Mer information om hur du utvecklar Windows Store-programmet finns [utveckla bra appar för Windows 8](http://msdn.microsoft.com/windows/apps/br229512.aspx). Den här lektionen innehåller hello följande procedurer:

1. Skapa ett projekt för Windows Store
2. Utforma användargränssnitt för hello (XAML)
3. Ändra hello koden bakom filen
4. Kompilera och testa programmet hello

**toocreate ett projekt för Windows Store**

1. Kör Visual Studio 2012 eller senare.
2. Från hello **filen** -menyn klickar du på **ny**, och klicka sedan på **projekt**.
3. Från dialogrutan Nytt projekt hello värden typ eller välj hello följande:

| Namn | Värde |
| --- | --- |
| Mallgrupp |Installerat/mallar/Visual C# / Windows Store |
| Mall |Tom App (XAML) |
| Namn |SSPlayer |
| Plats |C:\SSTutorials |
| Lösningens namn |SSPlayer |
| Skapa katalog för lösning |(valt) |

1. Klicka på **OK**.

**tooadd referens-toohello Smooth Streaming klient-SDK**

1. Från Solution Explorer högerklickar du på **SSPlayer**, och klicka sedan på **Lägg till referens**.
2. Ange eller välj hello följande värden:

| Namn | Värde |
| --- | --- |
| Referensgrupp |Windows-tillägg |
| Referens |Välj Microsoft Smooth Streaming klient-SDK för Windows 8 och Microsoft Visual C++ Runtime-paketet |

1. Klicka på **OK**. 

När du lägger till hello referenser måste du välja hello riktade plattform (x64 eller x86), lägga till referenser fungerar inte för någon CPU plattformskonfigurationen.  I solution explorer visas gul varning markering för dessa lagts till referenser.

**användargränssnittet för toodesign hello player**

1. Från Solution Explorer dubbelklickar du på **MainPage.xaml** tooopen visa den i hello design.
2. Leta upp hello  **&lt;rutnätet&gt;**  och  **&lt;/Grid&gt;**  taggar hello XAML-fil och klistra in hello följande kod mellan hello två taggar:

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
   
   hello MediaElement kontroll är används tooplayback media. hello skjutreglaget med namnet sliderProgress används pågår hello nästa lektionen toocontrol hello media.
3. Tryck på **CTRL + S** toosave hello-filen.

hello MediaElement kontrollen stöder inte Smooth Streaming innehåll out-of-box. tooenable hello Smooth Streaming-stöd, måste du registrera hello Smooth Streaming-bytedataströmmen hanterare av filnamnstillägg och MIME-typen.  tooregister, Använd hello MediaExtensionManager.RegisterByteStremHandler metod för hello Windows.Media namnområde.

I XAML-filen är vissa händelsehanterare associerade med hello kontroller.  Du måste definiera dessa händelsehanterare.

**toomodify hello koden bakom filen**

1. Från Solution Explorer högerklickar du på **MainPage.xaml**, och klicka sedan på **Visa kod**.
2. Överst hello i hello-fil, lägger du till hello följande med instruktionen:
   
        using Windows.Media;
3. Hello början av hello **MainPage** klassen och Lägg till följande datamedlemmen hello:
   
         private MediaExtensionManager extensions = new MediaExtensionManager();
4. Hello slutet av hello **MainPage** konstruktor, Lägg till följande två rader hello:
   
        extensions.RegisterByteStreamHandler("Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", ".ism", "text/xml");
        extensions.RegisterByteStreamHandler("Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", ".ism", "application/vnd.ms-sstr+xml");
5. Hello slutet av hello **MainPage** klassen, klistra in följande kod hello:
   
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
             txtStatus.Text = "Click hello Play button tooplay hello media source.";
         }
         }
         private void btnStop_Click(object sender, RoutedEventArgs e)
         {

         mediaElement.Stop();
         txtStatus.Text = "MediaElement is stopped";
         }
         private void sliderProgress_PointerPressed(object sender, PointerRoutedEventArgs e)
         {

         txtStatus.Text = "Seek tooposition " + sliderProgress.Value;
         mediaElement.Position = new TimeSpan(0, 0, (int)(sliderProgress.Value));
         }
         # endregion

Hej sliderProgress_PointerPressed händelsehanteraren definieras här.  Det finns flera fungerar toodo tooget den fungerar som beskrivs i hello kursen i den här kursen.
6. Tryck på **CTRL + S** toosave hello-filen.

hello klar hello koden bakom filen skall se ut så här:

![Codeview i Visual Studio för Smooth Streaming Windows Store-programmet][CodeViewPic]

**toocompile och testa hello-program**

1. Från hello **skapa** -menyn klickar du på **Configuration Manager**.
2. Ändra **aktiv lösning plattform** toomatch din plattform.
3. Tryck på **F6** toocompile hello projektet. 
4. Tryck på **F5** toorun hello program.
5. Överst hello i hello program, kan du använder hello standard URL för Smooth Streaming eller ange en annan. 
6. Klicka på **Ange källa**. Eftersom **Spela automatiskt** är aktiverad som standard hello skall spela upp automatiskt.  Du kan styra hello media med hello **spela upp**, **pausa** och **stoppa** knappar.  Du kan styra hello media volymen med hello lodräta skjutreglaget.  Men hello vågräta skjutreglaget för att styra hello media pågår inte helt implementerat ännu. 

Du har slutfört lesson1.  Nu bör använda du ett MediaElement kontrollen tooplayback Smooth Streaming innehåll.  I nästa lektionen hello du lägga till en skjutreglaget toocontrol hello fortskrider hello Smooth Streaming innehåll.

## <a name="lesson-2-add-a-slider-bar-toocontrol-hello-media-progress"></a>Lektionen 2: Lägg till en reglaget tooControl hello Media pågår

I lektionen 1 skapas en Windows Store-programmet med ett MediaElement XAML kontrollen tooplayback Smooth Streaming medieinnehåll.  Det gäller vissa grundläggande media-funktioner som starta, stoppa och pausa.  Nu bör du lägga till en skjutreglaget fältet toohello kontrollprogrammet.

I den här kursen ska vi använda timer tooupdate hello skjutreglaget position baserat på hello aktuella position hello MediaElement kontroll.  hello skjutreglaget start- och sluttid också behöver toobe uppdateras vid levande innehåll.  Detta kan hanteras bättre i hello anpassningsbar källa uppdatering-händelse.

Mediekällor är objekt som genererar media data.  hello källa matcharen tar en URL eller byte-dataström och skapar hello lämpliga mediekälla för innehållet.  hello källa matcharen kan hello standard hello program toocreate mediekällor. 

Den här lektionen innehåller hello följande procedurer:

1. Registrera hello Smooth Streaming-hanterare 
2. Lägga till händelsehanterare för hello anpassningsbar källa manager nivå
3. Lägga till hello anpassningsbar källa nivån händelsehanterare
4. Lägga till MediaElement händelsehanterare
5. Lägg till skjutreglaget relaterade streckkod
6. Kompilera och testa programmet hello

**tooregister hello Smooth Streaming-bytedataströmmen hanterare och pass hello propertyset**

1. Från Solution Explorer högerklickar du **MainPage.xaml**, och klicka sedan på **Visa kod**.
2. Hello början av hello-fil, lägger du till hello följande med instruktionen:

        using Microsoft.Media.AdaptiveStreaming;
3. Lägg till följande datamedlemmar hello hello början av hello MainPage klass:

         private Windows.Foundation.Collections.PropertySet propertySet = new Windows.Foundation.Collections.PropertySet();             
         private IAdaptiveSourceManager adaptiveSourceManager;
4. I hello **MainPage** konstruktor, Lägg till följande kod efter hello hello **detta. Initiera Components();**  rad och hello registrering kodraderna som skrivits i hello föregående lektionen:

        // Gets hello default instance of AdaptiveSourceManager which manages Smooth 
        //Streaming media sources.
        adaptiveSourceManager = AdaptiveSourceManager.GetDefault();
        // Sets property key value tooAdaptiveSourceManager default instance.
        // {A5CE1DE8-1D00-427B-ACEF-FB9A3C93DE2D}" must be hardcoded.
        propertySet["{A5CE1DE8-1D00-427B-ACEF-FB9A3C93DE2D}"] = adaptiveSourceManager;
5. I hello **MainPage** konstruktor, ändra hello två RegisterByteStreamHandler metoder tooadd hello tillbaka parametrar:

         // Registers Smooth Streaming byte-stream handler for ".ism" extension and, 
         // "text/xml" and "application/vnd.ms-ss" mime-types and pass hello propertyset. 
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
6. Tryck på **CTRL + S** toosave hello-filen.

**tooadd hello nivån händelsehanteraren för anpassningsbar källa manager**

1. Från Solution Explorer högerklickar du **MainPage.xaml**, och klicka sedan på **Visa kod**.
2. I hello **MainPage** klassen och Lägg till följande datamedlemmen hello:
   
     privata AdaptiveSource adaptiveSource = null;
3. Hello slutet av hello **MainPage** klassen och Lägg till följande händelsehanteraren hello:
   
         # region Adaptive Source Manager Level Events
         private void mediaElement_AdaptiveSourceOpened(AdaptiveSource sender, AdaptiveSourceOpenedEventArgs args)
         {

            adaptiveSource = args.AdaptiveSource;
         }

         # endregion Adaptive Source Manager Level Events
4. Hello slutet av hello **MainPage** konstruktor, Lägg till följande rad toosubscribe toohello anpassningsbar källa Öppna händelsen hello:
   
         adaptiveSourceManager.AdaptiveSourceOpenedEvent += 
           new AdaptiveSourceOpenedEventHandler(mediaElement_AdaptiveSourceOpened);
5. Tryck på **CTRL + S** toosave hello-filen.

**tooadd anpassningsbar källa nivån händelsehanterare**

1. Från Solution Explorer högerklickar du **MainPage.xaml**, och klicka sedan på **Visa kod**.
2. I hello **MainPage** klassen och Lägg till följande datamedlemmen hello:
   
     privata AdaptiveSourceStatusUpdatedEventArgs adaptiveSourceStatusUpdate;   privata manifestet manifestObject;
3. Hello slutet av hello **MainPage** klassen och Lägg till följande händelsehanterare hello:

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
4. Hello slutet av hello **mediaElement AdaptiveSourceOpened** metoden Lägg till följande kod toosubscribe toohello händelser hello:
   
         adaptiveSource.ManifestReadyEvent +=

                    mediaElement_ManifestReady;
         adaptiveSource.AdaptiveSourceStatusUpdatedEvent += 

            mediaElement_AdaptiveSourceStatusUpdated;
         adaptiveSource.AdaptiveSourceFailedEvent += 

            mediaElement_AdaptiveSourceFailed;
5. Tryck på **CTRL + S** toosave hello-filen.

hello samma händelser som är tillgängliga på anpassningsbar källa Manager nivå, som kan användas för att hantera funktioner tooall mediaelement som är vanliga i hello app. Varje AdaptiveSource innehåller sina egna händelser och alla AdaptiveSource händelser ska vara överlappande under AdaptiveSourceManager.

**tooadd mediaelement händelsehanterare**

1. Från Solution Explorer högerklickar du **MainPage.xaml**, och klicka sedan på **Visa kod**.
2. Hello slutet av hello **MainPage** klassen och Lägg till följande händelsehanterare hello:

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
3. Hello slutet av hello **MainPage** konstruktor, Lägg till följande kod toosubscript toohello händelser hello:

         mediaElement.MediaOpened += MediaOpened;
         mediaElement.MediaEnded += MediaEnded;
         mediaElement.MediaFailed += MediaFailed;
4. Tryck på **CTRL + S** toosave hello-filen.

**tooadd skjutreglaget relaterade streckkod**

1. Från Solution Explorer högerklickar du **MainPage.xaml**, och klicka sedan på **Visa kod**.
2. Hello början av hello-fil, lägger du till hello följande med instruktionen:
      
        using Windows.UI.Core;
3. I hello **MainPage** klassen och Lägg till följande datamedlemmar hello:
   
         public static CoreDispatcher _dispatcher;
         private DispatcherTimer sliderPositionUpdateDispatcher;
4. Hello slutet av hello **MainPage** konstruktor, Lägg till följande kod hello:
   
         _dispatcher = Window.Current.Dispatcher;
         PointerEventHandler pointerpressedhandler = new PointerEventHandler(sliderProgress_PointerPressed);
         sliderProgress.AddHandler(Control.PointerPressedEvent, pointerpressedhandler, true);    
5. Hello slutet av hello **MainPage** klassen och Lägg till följande kod hello:

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
>CoreDispatcher är används toomake ändringarna toohello UI-tråden från icke-UI-tråden. Utvecklare kan välja toouse dispatcher om han eller hon har för avsikt tooupdate tillhandahålls av UI-element för vid flaskhals i dispatcher-tråden.  Exempel:
   
         await sliderProgress.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () => { TimeSpan 

         timespan = new TimeSpan(adaptiveSourceStatusUpdate.EndTime); 
         double absvalue  = (int)Math.Round(timespan.TotalSeconds, MidpointRounding.AwayFromZero); 

         sliderProgress.Maximum = absvalue; }); 
6. Hello slutet av hello **mediaElement_AdaptiveSourceStatusUpdated** metod, Lägg till följande kod hello:

         setSliderStartTime(args.StartTime);
         setSliderEndTime(args.EndTime);
7. Hello slutet av hello **MediaOpened** metod, Lägg till följande kod hello:

         sliderProgress.StepFrequency = SliderFrequency(mediaElement.NaturalDuration.TimeSpan);
         sliderProgress.Width = mediaElement.Width;
         setupTimer();
8. Tryck på **CTRL + S** toosave hello-filen.

**toocompile och testa hello-program**

1. Tryck på **F6** toocompile hello projektet. 
2. Tryck på **F5** toorun hello program.
3. Överst hello i hello program, kan du använder hello standard URL för Smooth Streaming eller ange en annan. 
4. Klicka på **Ange källa**. 
5. Testa hello skjutreglaget.

Du har slutfört lektionen 2.  Lägga till en skjutreglaget tooapplication i den här lektionen. 

## <a name="lesson-3-select-smooth-streaming-streams"></a>Lektionen 3: Välj Smooth Streaming-dataströmmar
Smooth Streaming är kompatibla toostream innehåll med flera språk ljud spår som är valbara av hello-visningsprogram.  I kursen ska du aktivera visningsprogram tooselect dataströmmar. Den här lektionen innehåller hello följande procedurer:

1. Ändra hello XAML-fil
2. Ändra hello kodfilen behand
3. Kompilera och testa programmet hello

**toomodify hello XAML-fil**

1. Från Solution Explorer högerklickar du på **MainPage.xaml**, och klicka sedan på **Vydesigner**.
2. Leta upp &lt;Grid.RowDefinitions&gt;, och ändra hello RowDefinitions så de ser ut som:
   
         <Grid.RowDefinitions>            
            <RowDefinition Height="20"/>
            <RowDefinition Height="50"/>
            <RowDefinition Height="100"/>
            <RowDefinition Height="80"/>
            <RowDefinition Height="50"/>
         </Grid.RowDefinitions>
3. I hello &lt;rutnätet&gt;&lt;/Grid&gt; taggar, lägga till hello följande kod toodefine en listruta så att användarna kan se hello lista över tillgängliga dataströmmar och välja dataströmmar:

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
4. Tryck på **CTRL + S** toosave hello ändringar.

**toomodify hello koden bakom filen**

1. Från Solution Explorer högerklickar du på **MainPage.xaml**, och klicka sedan på **Visa kod**.
2. Lägg till en ny klass i hello SSPlayer namnområde:
   
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
                    // mMke hello video stream always checked.
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
3. Hello början av hello MainPage klass, lägger du till hello följande definitioner av variabeln:
   
         private List<Stream> availableStreams;
         private List<Stream> availableAudioStreams;
         private List<Stream> availableTextStreams;
         private List<Stream> availableVideoStreams;
4. Lägg till hello efter region i hello MainPage klass:
   
        #region stream selection
        ///<summary>
        ///Functionality tooselect streams from IManifestStream available streams
        /// </summary>
   
        // This function is called from hello mediaElement_ManifestReady event handler 
        // tooretrieve hello streams and populate them toohello local data members.
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
   
                    //populate hello stream lists based on hello types
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
   
                    // Select hello default selected streams from hello manifest.
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
   
        // This function set hello list box ItemSource
        private async void refreshAvailableStreamsListBoxItemSource()
        {
            try
            {
                //update hello stream check box list on hello UI
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
   
            // Select hello frist video stream from hello list if no video stream is selected
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
                    txtStatus.Text = "hello audio stream is changed too" + availableAudioStreams[j].ManifestStream.Name;
                }
            }
   
            // Select hello frist audio stream from hello list if no audio steam is selected.
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
5. Leta upp hello mediaElement_ManifestReady metod, Lägg till följande kod hello slutet av hello funktionen hello:
   
        getStreams(manifestObject);
        refreshAvailableStreamsListBoxItemSource();
   
    Så när MediaElement manifestet är klar, hello kod hämtar en lista över tillgängliga hello-dataströmmar och fyller hello UI listruta med hello lista.
6. Inuti hello MainPage klass, hitta hello UI knappar klickar du på händelser region och Lägg sedan till följande funktionsdefinitionen hello:
   
        private void btnChangeStream_Click(object sender, RoutedEventArgs e)
        {
            List<IManifestStream> selectedStreams = new List<IManifestStream>();
   
            // Create a list of hello selected streams
            createSelectedStreamsList(selectedStreams);
   
            // Change streams on hello presentation
            changeStreams(selectedStreams);
        }

**toocompile och testa hello-program**

1. Tryck på **F6** toocompile hello projektet. 
2. Tryck på **F5** toorun hello program.
3. Överst hello i hello program, kan du använder hello standard URL för Smooth Streaming eller ange en annan. 
4. Klicka på **Ange källa**. 
5. hello standardspråket är audio_eng. Försök tooswitch mellan audio_eng och audio_es. Varje gång, väljer du en ny ström, måste du klicka på knappen Skicka för hello.

Du har slutfört lektionen 3.  Nu bör du lägga till hello funktioner toochoose dataströmmar.

## <a name="lesson-4-select-smooth-streaming-tracks"></a>Lektionen 4: Välj Smooth Streaming spårar
En Smooth Streaming-presentation kan innehålla flera videofiler kodad med olika kvalitet (bithastigheter) och lösningar. I kursen ska du aktivera användare tooselect spår. Den här lektionen innehåller hello följande procedurer:

1. Ändra hello XAML-fil
2. Ändra hello kodfilen behand
3. Kompilera och testa programmet hello

**toomodify hello XAML-fil**

1. Från Solution Explorer högerklickar du på **MainPage.xaml**, och klicka sedan på **Vydesigner**.
2. Hitta hello &lt;rutnätet&gt; tagg med namnet hello **gridStreamAndBitrateSelection**, Lägg till hello följande kod hello slutet av hello-taggen:
   
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
3. Tryck på **CTRL + S** toosave ändrar han

**toomodify hello koden bakom filen**

1. Från Solution Explorer högerklickar du på **MainPage.xaml**, och klicka sedan på **Visa kod**.
2. Lägg till en ny klass i hello SSPlayer namnområde:
   
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
3. Hello början av hello MainPage klass, lägger du till hello följande definitioner av variabeln:
   
        private List<Track> availableTracks;
4. Lägg till hello efter region i hello MainPage klass:
   
        #region track selection
        /// <summary>
        /// Functionality tooselect video streams
        /// </summary>
   
        /// This Function gets hello tracks for hello selected video stream
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
   
        // This function gets hello video stream that is playing
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
   
        // This function set hello UI list box control ItemSource
        private async void refreshAvailableTracksListBoxItemSource()
        {
            try
            {
                // Update hello track check box list on hello UI 
                await _dispatcher.RunAsync(CoreDispatcherPriority.Normal, ()
                    => { lbAvailableVideoTracks.ItemsSource = availableTracks; });
            }
            catch (Exception e)
            {
                txtStatus.Text = "Error: " + e.Message;
            }        
        }
   
        // This function creates a list of hello selected tracks.
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
   
        // This function selects hello tracks based on user selection 
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
5. Leta upp hello mediaElement_ManifestReady metod, Lägg till följande kod hello slutet av hello funktionen hello:
   
         getTracks(manifestObject);
         refreshAvailableTracksListBoxItemSource();
6. Inuti hello MainPage klass, hitta hello UI knappar klickar du på händelser region och Lägg sedan till följande funktionsdefinitionen hello:
   
         private void btnChangeStream_Click(object sender, RoutedEventArgs e)
         {
            List<IManifestStream> selectedStreams = new List<IManifestStream>();

            // Create a list of hello selected streams
            createSelectedStreamsList(selectedStreams);

            // Change streams on hello presentation
            changeStreams(selectedStreams);
         }

**toocompile och testa hello-program**

1. Tryck på **F6** toocompile hello projektet. 
2. Tryck på **F5** toorun hello program.
3. Överst hello i hello program, kan du använder hello standard URL för Smooth Streaming eller ange en annan. 
4. Klicka på **Ange källa**. 
5. Alla hello spårar hello video dataströmmen är markerade som standard. tooexperiment hello bit ändringar, du kan välja hello lägsta bithastighet tillgänglig och sedan välja hello högsta överföringshastighet tillgängliga. Du måste klicka på Skicka efter varje ändring.  Du kan se hello bildkvaliteten ändringar.

Du har slutfört lektionen 4.  Nu bör du lägga till hello funktioner toochoose spårar.

## <a name="media-services-learning-paths"></a>Sökvägar för Media Services-utbildning
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="other-resources"></a>Andra resurser:
* [Hur toobuild ett Smooth Streaming JavaScript för Windows 8-program med avancerade funktioner](http://blogs.iis.net/cenkd/archive/2012/08/10/how-to-build-a-smooth-streaming-windows-8-javascript-application-with-advanced-features.aspx)
* [Smooth Streaming teknisk översikt](http://www.iis.net/learn/media/on-demand-smooth-streaming/smooth-streaming-technical-overview)

[PlayerApplication]: ./media/media-services-build-smooth-streaming-apps/SSClientWin8-1.png
[CodeViewPic]: ./media/media-services-build-smooth-streaming-apps/SSClientWin8-2.png

