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
# <a name="how-toobuild-a-smooth-streaming-windows-store-application"></a><span data-ttu-id="07879-103">Hur tooBuild ett Smooth Streaming Windows Store-program</span><span class="sxs-lookup"><span data-stu-id="07879-103">How tooBuild a Smooth Streaming Windows Store Application</span></span>

<span data-ttu-id="07879-104">hello Smooth Streaming-klient SDK för Windows 8 kan utvecklare toobuild Windows Store-program som kan spela upp på begäran och live Smooth Streaming-innehåll.</span><span class="sxs-lookup"><span data-stu-id="07879-104">hello Smooth Streaming Client SDK for Windows 8 enables developers toobuild Windows Store applications that can play on-demand and live Smooth Streaming content.</span></span> <span data-ttu-id="07879-105">Dessutom ger toohello grundläggande spela upp Smooth Streaming innehåll, hello SDK också omfattande funktioner som Microsoft PlayReady skydd, kvaliteten begränsning, Live DVR, ljudström växlar, lyssnar efter statusuppdateringar (till exempel kvalitet ändras ) och felhändelser och så vidare.</span><span class="sxs-lookup"><span data-stu-id="07879-105">In addition toohello basic playback of Smooth Streaming content, hello SDK also provides rich features like Microsoft PlayReady protection, quality level restriction, Live DVR, audio stream switching, listening for status updates (such as quality level changes) and error events, and so on.</span></span> <span data-ttu-id="07879-106">Mer information hello stöds funktioner finns hello [viktig information](http://www.iis.net/learn/media/smooth-streaming/smooth-streaming-client-sdk-for-windows-8-release-notes).</span><span class="sxs-lookup"><span data-stu-id="07879-106">For more information of hello supported features, see hello [release notes](http://www.iis.net/learn/media/smooth-streaming/smooth-streaming-client-sdk-for-windows-8-release-notes).</span></span> <span data-ttu-id="07879-107">Mer information finns i [Player Framework för Windows 8](http://playerframework.codeplex.com/).</span><span class="sxs-lookup"><span data-stu-id="07879-107">For more information, see [Player Framework for Windows 8](http://playerframework.codeplex.com/).</span></span> 

<span data-ttu-id="07879-108">Den här självstudiekursen innehåller fyra erfarenheter:</span><span class="sxs-lookup"><span data-stu-id="07879-108">This tutorial contains four lessons:</span></span>

1. <span data-ttu-id="07879-109">Skapa ett grundläggande Smooth Streaming Store-program</span><span class="sxs-lookup"><span data-stu-id="07879-109">Create a Basic Smooth Streaming Store Application</span></span>
2. <span data-ttu-id="07879-110">Lägg till en reglaget tooControl hello Media pågår</span><span class="sxs-lookup"><span data-stu-id="07879-110">Add a Slider Bar tooControl hello Media Progress</span></span>
3. <span data-ttu-id="07879-111">Välj Smooth Streaming-dataströmmar</span><span class="sxs-lookup"><span data-stu-id="07879-111">Select Smooth Streaming Streams</span></span>
4. <span data-ttu-id="07879-112">Välj Smooth Streaming spårar</span><span class="sxs-lookup"><span data-stu-id="07879-112">Select Smooth Streaming Tracks</span></span>

## <a name="prerequisites"></a><span data-ttu-id="07879-113">Krav</span><span class="sxs-lookup"><span data-stu-id="07879-113">Prerequisites</span></span>
* <span data-ttu-id="07879-114">Windows 8, 32-bitars eller 64-bitars.</span><span class="sxs-lookup"><span data-stu-id="07879-114">Windows 8 32-bit or 64-bit.</span></span> <span data-ttu-id="07879-115">Du kan hämta [utvärdering av Windows 8 Enterprise](http://msdn.microsoft.com/evalcenter/jj554510.aspx) på MSDN.</span><span class="sxs-lookup"><span data-stu-id="07879-115">You can get [Windows 8 Enterprise Evaluation](http://msdn.microsoft.com/evalcenter/jj554510.aspx) from MSDN.</span></span>
* <span data-ttu-id="07879-116">Visual Studio 2012 eller Visual Studio Express 2012 (eller en senare version).</span><span class="sxs-lookup"><span data-stu-id="07879-116">Visual Studio 2012 or Visual Studio Express 2012 (or a later version).</span></span> <span data-ttu-id="07879-117">Du kan hämta hello utvärderingsversion från [här](http://www.microsoft.com/visualstudio/11/downloads).</span><span class="sxs-lookup"><span data-stu-id="07879-117">You can get hello trial version from [here](http://www.microsoft.com/visualstudio/11/downloads).</span></span>
* <span data-ttu-id="07879-118">[Microsoft Smooth Streaming klient-SDK för Windows 8](http://visualstudiogallery.msdn.microsoft.com/04423d13-3b3e-4741-a01c-1ae29e84fea6?SRC=Homehttp://visualstudiogallery.msdn.microsoft.com/04423d13-3b3e-4741-a01c-1ae29e84fea6?SRC=Home).</span><span class="sxs-lookup"><span data-stu-id="07879-118">[Microsoft Smooth Streaming Client SDK for Windows 8](http://visualstudiogallery.msdn.microsoft.com/04423d13-3b3e-4741-a01c-1ae29e84fea6?SRC=Homehttp://visualstudiogallery.msdn.microsoft.com/04423d13-3b3e-4741-a01c-1ae29e84fea6?SRC=Home).</span></span>

<span data-ttu-id="07879-119">hello slutförts lösning för varje lektionen kan hämtas från MSDN Developer kodexempel (Code Gallery):</span><span class="sxs-lookup"><span data-stu-id="07879-119">hello completed solution for each lesson can be downloaded from MSDN Developer Code Samples (Code Gallery):</span></span> 

* <span data-ttu-id="07879-120">[Lektion 1](http://code.msdn.microsoft.com/Smooth-Streaming-Client-0bb1471f) – en enkel Windows 8 Smooth Streaming Media Player</span><span class="sxs-lookup"><span data-stu-id="07879-120">[Lesson 1](http://code.msdn.microsoft.com/Smooth-Streaming-Client-0bb1471f) - A Simple Windows 8 Smooth Streaming Media Player,</span></span> 
* <span data-ttu-id="07879-121">[Lektion 2](http://code.msdn.microsoft.com/A-simple-Windows-8-Smooth-ee98f63a) – en enkel Windows 8 Smooth Streaming Media Player med en skjutreglaget kontroll,</span><span class="sxs-lookup"><span data-stu-id="07879-121">[Lesson 2](http://code.msdn.microsoft.com/A-simple-Windows-8-Smooth-ee98f63a) - A Simple Windows 8 Smooth Streaming Media Player with a Slider Bar Control,</span></span> 
* <span data-ttu-id="07879-122">[Lektion 3](http://code.msdn.microsoft.com/A-Windows-8-Smooth-883c3b44) – en Windows 8-Smooth Streaming Media Player med dataströmmen valet</span><span class="sxs-lookup"><span data-stu-id="07879-122">[Lesson 3](http://code.msdn.microsoft.com/A-Windows-8-Smooth-883c3b44) - A Windows 8 Smooth Streaming Media Player with Stream Selection,</span></span>  
* <span data-ttu-id="07879-123">[Lektion 4](http://code.msdn.microsoft.com/A-Windows-8-Smooth-aa9e4907) – en Windows 8-Smooth Streaming Media Player med spåra valet.</span><span class="sxs-lookup"><span data-stu-id="07879-123">[Lesson 4](http://code.msdn.microsoft.com/A-Windows-8-Smooth-aa9e4907)  - A Windows 8 Smooth Streaming Media Player with Track Selection.</span></span>

## <a name="lesson-1-create-a-basic-smooth-streaming-store-application"></a><span data-ttu-id="07879-124">Lektionen 1: Skapa ett grundläggande Smooth Streaming Store-program</span><span class="sxs-lookup"><span data-stu-id="07879-124">Lesson 1: Create a Basic Smooth Streaming Store Application</span></span>

<span data-ttu-id="07879-125">I kursen skapar du en Windows Store-programmet med ett MediaElement kontrollen tooplay Smooth Stream innehåll.</span><span class="sxs-lookup"><span data-stu-id="07879-125">In this lesson, you will create a Windows Store application with a MediaElement control tooplay Smooth Stream content.</span></span>  <span data-ttu-id="07879-126">Det ser ut så hello program som körs:</span><span class="sxs-lookup"><span data-stu-id="07879-126">hello running application looks like:</span></span>

![Exempel på ett Smooth Streaming Windows Store][PlayerApplication]

<span data-ttu-id="07879-128">Mer information om hur du utvecklar Windows Store-programmet finns [utveckla bra appar för Windows 8](http://msdn.microsoft.com/windows/apps/br229512.aspx).</span><span class="sxs-lookup"><span data-stu-id="07879-128">For more information on developing Windows Store application, see [Develop Great Apps for Windows 8](http://msdn.microsoft.com/windows/apps/br229512.aspx).</span></span> <span data-ttu-id="07879-129">Den här lektionen innehåller hello följande procedurer:</span><span class="sxs-lookup"><span data-stu-id="07879-129">This lesson contains hello following procedures:</span></span>

1. <span data-ttu-id="07879-130">Skapa ett projekt för Windows Store</span><span class="sxs-lookup"><span data-stu-id="07879-130">Create a Windows Store project</span></span>
2. <span data-ttu-id="07879-131">Utforma användargränssnitt för hello (XAML)</span><span class="sxs-lookup"><span data-stu-id="07879-131">Design hello user interface (XAML)</span></span>
3. <span data-ttu-id="07879-132">Ändra hello koden bakom filen</span><span class="sxs-lookup"><span data-stu-id="07879-132">Modify hello code behind file</span></span>
4. <span data-ttu-id="07879-133">Kompilera och testa programmet hello</span><span class="sxs-lookup"><span data-stu-id="07879-133">Compile and test hello application</span></span>

<span data-ttu-id="07879-134">**toocreate ett projekt för Windows Store**</span><span class="sxs-lookup"><span data-stu-id="07879-134">**toocreate a Windows Store project**</span></span>

1. <span data-ttu-id="07879-135">Kör Visual Studio 2012 eller senare.</span><span class="sxs-lookup"><span data-stu-id="07879-135">Run Visual Studio 2012 or later.</span></span>
2. <span data-ttu-id="07879-136">Från hello **filen** -menyn klickar du på **ny**, och klicka sedan på **projekt**.</span><span class="sxs-lookup"><span data-stu-id="07879-136">From hello **FILE** menu, click **New**, and then click **Project**.</span></span>
3. <span data-ttu-id="07879-137">Från dialogrutan Nytt projekt hello värden typ eller välj hello följande:</span><span class="sxs-lookup"><span data-stu-id="07879-137">From hello New Project dialog, type or select  hello following values:</span></span>

| <span data-ttu-id="07879-138">Namn</span><span class="sxs-lookup"><span data-stu-id="07879-138">Name</span></span> | <span data-ttu-id="07879-139">Värde</span><span class="sxs-lookup"><span data-stu-id="07879-139">Value</span></span> |
| --- | --- |
| <span data-ttu-id="07879-140">Mallgrupp</span><span class="sxs-lookup"><span data-stu-id="07879-140">Template group</span></span> |<span data-ttu-id="07879-141">Installerat/mallar/Visual C# / Windows Store</span><span class="sxs-lookup"><span data-stu-id="07879-141">Installed/Templates/Visual C#/Windows Store</span></span> |
| <span data-ttu-id="07879-142">Mall</span><span class="sxs-lookup"><span data-stu-id="07879-142">Template</span></span> |<span data-ttu-id="07879-143">Tom App (XAML)</span><span class="sxs-lookup"><span data-stu-id="07879-143">Blank App (XAML)</span></span> |
| <span data-ttu-id="07879-144">Namn</span><span class="sxs-lookup"><span data-stu-id="07879-144">Name</span></span> |<span data-ttu-id="07879-145">SSPlayer</span><span class="sxs-lookup"><span data-stu-id="07879-145">SSPlayer</span></span> |
| <span data-ttu-id="07879-146">Plats</span><span class="sxs-lookup"><span data-stu-id="07879-146">Location</span></span> |<span data-ttu-id="07879-147">C:\SSTutorials</span><span class="sxs-lookup"><span data-stu-id="07879-147">C:\SSTutorials</span></span> |
| <span data-ttu-id="07879-148">Lösningens namn</span><span class="sxs-lookup"><span data-stu-id="07879-148">Solution Name</span></span> |<span data-ttu-id="07879-149">SSPlayer</span><span class="sxs-lookup"><span data-stu-id="07879-149">SSPlayer</span></span> |
| <span data-ttu-id="07879-150">Skapa katalog för lösning</span><span class="sxs-lookup"><span data-stu-id="07879-150">Create directory for solution</span></span> |<span data-ttu-id="07879-151">(valt)</span><span class="sxs-lookup"><span data-stu-id="07879-151">(selected)</span></span> |

1. <span data-ttu-id="07879-152">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="07879-152">Click **OK**.</span></span>

<span data-ttu-id="07879-153">**tooadd referens-toohello Smooth Streaming klient-SDK**</span><span class="sxs-lookup"><span data-stu-id="07879-153">**tooadd a reference toohello Smooth Streaming Client SDK**</span></span>

1. <span data-ttu-id="07879-154">Från Solution Explorer högerklickar du på **SSPlayer**, och klicka sedan på **Lägg till referens**.</span><span class="sxs-lookup"><span data-stu-id="07879-154">From Solution Explorer, right-click **SSPlayer**, and then click **Add Reference**.</span></span>
2. <span data-ttu-id="07879-155">Ange eller välj hello följande värden:</span><span class="sxs-lookup"><span data-stu-id="07879-155">Type or select hello following values:</span></span>

| <span data-ttu-id="07879-156">Namn</span><span class="sxs-lookup"><span data-stu-id="07879-156">Name</span></span> | <span data-ttu-id="07879-157">Värde</span><span class="sxs-lookup"><span data-stu-id="07879-157">Value</span></span> |
| --- | --- |
| <span data-ttu-id="07879-158">Referensgrupp</span><span class="sxs-lookup"><span data-stu-id="07879-158">Reference group</span></span> |<span data-ttu-id="07879-159">Windows-tillägg</span><span class="sxs-lookup"><span data-stu-id="07879-159">Windows/Extensions</span></span> |
| <span data-ttu-id="07879-160">Referens</span><span class="sxs-lookup"><span data-stu-id="07879-160">Reference</span></span> |<span data-ttu-id="07879-161">Välj Microsoft Smooth Streaming klient-SDK för Windows 8 och Microsoft Visual C++ Runtime-paketet</span><span class="sxs-lookup"><span data-stu-id="07879-161">Select Microsoft Smooth Streaming Client SDK for Windows 8 and Microsoft Visual C++ Runtime Package</span></span> |

1. <span data-ttu-id="07879-162">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="07879-162">Click **OK**.</span></span> 

<span data-ttu-id="07879-163">När du lägger till hello referenser måste du välja hello riktade plattform (x64 eller x86), lägga till referenser fungerar inte för någon CPU plattformskonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="07879-163">After adding hello references, you must select hello targeted platform (x64 or x86), adding references will not work for Any CPU platform configuration.</span></span>  <span data-ttu-id="07879-164">I solution explorer visas gul varning markering för dessa lagts till referenser.</span><span class="sxs-lookup"><span data-stu-id="07879-164">In solution explorer, you will see yellow warning mark for these added references.</span></span>

<span data-ttu-id="07879-165">**användargränssnittet för toodesign hello player**</span><span class="sxs-lookup"><span data-stu-id="07879-165">**toodesign hello player user interface**</span></span>

1. <span data-ttu-id="07879-166">Från Solution Explorer dubbelklickar du på **MainPage.xaml** tooopen visa den i hello design.</span><span class="sxs-lookup"><span data-stu-id="07879-166">From Solution Explorer, double click **MainPage.xaml** tooopen it in hello design view.</span></span>
2. <span data-ttu-id="07879-167">Leta upp hello  **&lt;rutnätet&gt;**  och  **&lt;/Grid&gt;**  taggar hello XAML-fil och klistra in hello följande kod mellan hello två taggar:</span><span class="sxs-lookup"><span data-stu-id="07879-167">Locate hello **&lt;Grid&gt;** and **&lt;/Grid&gt;**  tags hello XAML file, and paste hello following code between hello two tags:</span></span>

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
   
   <span data-ttu-id="07879-168">hello MediaElement kontroll är används tooplayback media.</span><span class="sxs-lookup"><span data-stu-id="07879-168">hello MediaElement control is used tooplayback media.</span></span> <span data-ttu-id="07879-169">hello skjutreglaget med namnet sliderProgress används pågår hello nästa lektionen toocontrol hello media.</span><span class="sxs-lookup"><span data-stu-id="07879-169">hello slider control named sliderProgress will be used in hello next lesson toocontrol hello media progress.</span></span>
3. <span data-ttu-id="07879-170">Tryck på **CTRL + S** toosave hello-filen.</span><span class="sxs-lookup"><span data-stu-id="07879-170">Press **CTRL+S** toosave hello file.</span></span>

<span data-ttu-id="07879-171">hello MediaElement kontrollen stöder inte Smooth Streaming innehåll out-of-box.</span><span class="sxs-lookup"><span data-stu-id="07879-171">hello MediaElement control does not support Smooth Streaming content out-of-box.</span></span> <span data-ttu-id="07879-172">tooenable hello Smooth Streaming-stöd, måste du registrera hello Smooth Streaming-bytedataströmmen hanterare av filnamnstillägg och MIME-typen.</span><span class="sxs-lookup"><span data-stu-id="07879-172">tooenable hello Smooth Streaming support, you must register hello Smooth Streaming byte-stream handler by file name extension and MIME type.</span></span>  <span data-ttu-id="07879-173">tooregister, Använd hello MediaExtensionManager.RegisterByteStremHandler metod för hello Windows.Media namnområde.</span><span class="sxs-lookup"><span data-stu-id="07879-173">tooregister, you use hello MediaExtensionManager.RegisterByteStremHandler method of hello Windows.Media namespace.</span></span>

<span data-ttu-id="07879-174">I XAML-filen är vissa händelsehanterare associerade med hello kontroller.</span><span class="sxs-lookup"><span data-stu-id="07879-174">In this XAML file, some event handlers are associated with hello controls.</span></span>  <span data-ttu-id="07879-175">Du måste definiera dessa händelsehanterare.</span><span class="sxs-lookup"><span data-stu-id="07879-175">You must define those event handlers.</span></span>

<span data-ttu-id="07879-176">**toomodify hello koden bakom filen**</span><span class="sxs-lookup"><span data-stu-id="07879-176">**toomodify hello code behind file**</span></span>

1. <span data-ttu-id="07879-177">Från Solution Explorer högerklickar du på **MainPage.xaml**, och klicka sedan på **Visa kod**.</span><span class="sxs-lookup"><span data-stu-id="07879-177">From Solution Explorer, right-click **MainPage.xaml**, and then click **View Code**.</span></span>
2. <span data-ttu-id="07879-178">Överst hello i hello-fil, lägger du till hello följande med instruktionen:</span><span class="sxs-lookup"><span data-stu-id="07879-178">At hello top of hello file, add hello following using statement:</span></span>
   
        using Windows.Media;
3. <span data-ttu-id="07879-179">Hello början av hello **MainPage** klassen och Lägg till följande datamedlemmen hello:</span><span class="sxs-lookup"><span data-stu-id="07879-179">At hello beginning of hello **MainPage** class, add hello following data member:</span></span>
   
         private MediaExtensionManager extensions = new MediaExtensionManager();
4. <span data-ttu-id="07879-180">Hello slutet av hello **MainPage** konstruktor, Lägg till följande två rader hello:</span><span class="sxs-lookup"><span data-stu-id="07879-180">At hello end of hello **MainPage** constructor, add hello following two lines:</span></span>
   
        extensions.RegisterByteStreamHandler("Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", ".ism", "text/xml");
        extensions.RegisterByteStreamHandler("Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", ".ism", "application/vnd.ms-sstr+xml");
5. <span data-ttu-id="07879-181">Hello slutet av hello **MainPage** klassen, klistra in följande kod hello:</span><span class="sxs-lookup"><span data-stu-id="07879-181">At hello end of hello **MainPage** class, paste hello following code:</span></span>
   
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

<span data-ttu-id="07879-182">Hej sliderProgress_PointerPressed händelsehanteraren definieras här.</span><span class="sxs-lookup"><span data-stu-id="07879-182">hello sliderProgress_PointerPressed event handler is defined here.</span></span>  <span data-ttu-id="07879-183">Det finns flera fungerar toodo tooget den fungerar som beskrivs i hello kursen i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="07879-183">There are more works toodo tooget it working, which will be covered in hello next lesson of this tutorial.</span></span>
6. <span data-ttu-id="07879-184">Tryck på **CTRL + S** toosave hello-filen.</span><span class="sxs-lookup"><span data-stu-id="07879-184">Press **CTRL+S** toosave hello file.</span></span>

<span data-ttu-id="07879-185">hello klar hello koden bakom filen skall se ut så här:</span><span class="sxs-lookup"><span data-stu-id="07879-185">hello finished hello code behind file shall look like this:</span></span>

![Codeview i Visual Studio för Smooth Streaming Windows Store-programmet][CodeViewPic]

<span data-ttu-id="07879-187">**toocompile och testa hello-program**</span><span class="sxs-lookup"><span data-stu-id="07879-187">**toocompile and test hello application**</span></span>

1. <span data-ttu-id="07879-188">Från hello **skapa** -menyn klickar du på **Configuration Manager**.</span><span class="sxs-lookup"><span data-stu-id="07879-188">From hello **BUILD** menu, click **Configuration Manager**.</span></span>
2. <span data-ttu-id="07879-189">Ändra **aktiv lösning plattform** toomatch din plattform.</span><span class="sxs-lookup"><span data-stu-id="07879-189">Change **Active solution platform** toomatch your development platform.</span></span>
3. <span data-ttu-id="07879-190">Tryck på **F6** toocompile hello projektet.</span><span class="sxs-lookup"><span data-stu-id="07879-190">Press **F6** toocompile hello project.</span></span> 
4. <span data-ttu-id="07879-191">Tryck på **F5** toorun hello program.</span><span class="sxs-lookup"><span data-stu-id="07879-191">Press **F5** toorun hello application.</span></span>
5. <span data-ttu-id="07879-192">Överst hello i hello program, kan du använder hello standard URL för Smooth Streaming eller ange en annan.</span><span class="sxs-lookup"><span data-stu-id="07879-192">At hello top of hello application, you can either use hello default Smooth Streaming URL or enter a different one.</span></span> 
6. <span data-ttu-id="07879-193">Klicka på **Ange källa**.</span><span class="sxs-lookup"><span data-stu-id="07879-193">Click **Set Source**.</span></span> <span data-ttu-id="07879-194">Eftersom **Spela automatiskt** är aktiverad som standard hello skall spela upp automatiskt.</span><span class="sxs-lookup"><span data-stu-id="07879-194">Because **Auto Play** is enabled by default, hello media shall play automatically.</span></span>  <span data-ttu-id="07879-195">Du kan styra hello media med hello **spela upp**, **pausa** och **stoppa** knappar.</span><span class="sxs-lookup"><span data-stu-id="07879-195">You can control hello media using hello **Play**, **Pause** and **Stop** buttons.</span></span>  <span data-ttu-id="07879-196">Du kan styra hello media volymen med hello lodräta skjutreglaget.</span><span class="sxs-lookup"><span data-stu-id="07879-196">You can control hello media volume using hello vertical slider.</span></span>  <span data-ttu-id="07879-197">Men hello vågräta skjutreglaget för att styra hello media pågår inte helt implementerat ännu.</span><span class="sxs-lookup"><span data-stu-id="07879-197">However hello horizontal slider for controlling hello media progress is not fully implemented yet.</span></span> 

<span data-ttu-id="07879-198">Du har slutfört lesson1.</span><span class="sxs-lookup"><span data-stu-id="07879-198">You have completed lesson1.</span></span>  <span data-ttu-id="07879-199">Nu bör använda du ett MediaElement kontrollen tooplayback Smooth Streaming innehåll.</span><span class="sxs-lookup"><span data-stu-id="07879-199">In this lesson, you use a MediaElement control tooplayback Smooth Streaming content.</span></span>  <span data-ttu-id="07879-200">I nästa lektionen hello du lägga till en skjutreglaget toocontrol hello fortskrider hello Smooth Streaming innehåll.</span><span class="sxs-lookup"><span data-stu-id="07879-200">In hello next lesson, you will add a slider toocontrol hello progress of hello Smooth Streaming content.</span></span>

## <a name="lesson-2-add-a-slider-bar-toocontrol-hello-media-progress"></a><span data-ttu-id="07879-201">Lektionen 2: Lägg till en reglaget tooControl hello Media pågår</span><span class="sxs-lookup"><span data-stu-id="07879-201">Lesson 2: Add a Slider Bar tooControl hello Media Progress</span></span>

<span data-ttu-id="07879-202">I lektionen 1 skapas en Windows Store-programmet med ett MediaElement XAML kontrollen tooplayback Smooth Streaming medieinnehåll.</span><span class="sxs-lookup"><span data-stu-id="07879-202">In lesson 1, you created a Windows Store application with a MediaElement XAML control tooplayback Smooth Streaming media content.</span></span>  <span data-ttu-id="07879-203">Det gäller vissa grundläggande media-funktioner som starta, stoppa och pausa.</span><span class="sxs-lookup"><span data-stu-id="07879-203">It comes some basic media functions like start, stop and pause.</span></span>  <span data-ttu-id="07879-204">Nu bör du lägga till en skjutreglaget fältet toohello kontrollprogrammet.</span><span class="sxs-lookup"><span data-stu-id="07879-204">In this lesson, you will add a slider bar control toohello application.</span></span>

<span data-ttu-id="07879-205">I den här kursen ska vi använda timer tooupdate hello skjutreglaget position baserat på hello aktuella position hello MediaElement kontroll.</span><span class="sxs-lookup"><span data-stu-id="07879-205">In this tutorial, we will use a timer tooupdate hello slider position based on hello current position of hello MediaElement control.</span></span>  <span data-ttu-id="07879-206">hello skjutreglaget start- och sluttid också behöver toobe uppdateras vid levande innehåll.</span><span class="sxs-lookup"><span data-stu-id="07879-206">hello slider start and end time also need toobe updated in case of live content.</span></span>  <span data-ttu-id="07879-207">Detta kan hanteras bättre i hello anpassningsbar källa uppdatering-händelse.</span><span class="sxs-lookup"><span data-stu-id="07879-207">This can be better handled in hello adaptive source update event.</span></span>

<span data-ttu-id="07879-208">Mediekällor är objekt som genererar media data.</span><span class="sxs-lookup"><span data-stu-id="07879-208">Media sources are objects that generate media data.</span></span>  <span data-ttu-id="07879-209">hello källa matcharen tar en URL eller byte-dataström och skapar hello lämpliga mediekälla för innehållet.</span><span class="sxs-lookup"><span data-stu-id="07879-209">hello source resolver takes a URL or byte stream and creates hello appropriate media source for that content.</span></span>  <span data-ttu-id="07879-210">hello källa matcharen kan hello standard hello program toocreate mediekällor.</span><span class="sxs-lookup"><span data-stu-id="07879-210">hello source resolver is hello standard way for hello applications toocreate media sources.</span></span> 

<span data-ttu-id="07879-211">Den här lektionen innehåller hello följande procedurer:</span><span class="sxs-lookup"><span data-stu-id="07879-211">This lesson contains hello following procedures:</span></span>

1. <span data-ttu-id="07879-212">Registrera hello Smooth Streaming-hanterare</span><span class="sxs-lookup"><span data-stu-id="07879-212">Register hello Smooth Streaming handler</span></span> 
2. <span data-ttu-id="07879-213">Lägga till händelsehanterare för hello anpassningsbar källa manager nivå</span><span class="sxs-lookup"><span data-stu-id="07879-213">Add hello adaptive source manager level event handlers</span></span>
3. <span data-ttu-id="07879-214">Lägga till hello anpassningsbar källa nivån händelsehanterare</span><span class="sxs-lookup"><span data-stu-id="07879-214">Add hello adaptive source level event handlers</span></span>
4. <span data-ttu-id="07879-215">Lägga till MediaElement händelsehanterare</span><span class="sxs-lookup"><span data-stu-id="07879-215">Add MediaElement event handlers</span></span>
5. <span data-ttu-id="07879-216">Lägg till skjutreglaget relaterade streckkod</span><span class="sxs-lookup"><span data-stu-id="07879-216">Add slider bar related code</span></span>
6. <span data-ttu-id="07879-217">Kompilera och testa programmet hello</span><span class="sxs-lookup"><span data-stu-id="07879-217">Compile and test hello application</span></span>

<span data-ttu-id="07879-218">**tooregister hello Smooth Streaming-bytedataströmmen hanterare och pass hello propertyset**</span><span class="sxs-lookup"><span data-stu-id="07879-218">**tooregister hello Smooth Streaming byte-stream handler and pass hello propertyset**</span></span>

1. <span data-ttu-id="07879-219">Från Solution Explorer högerklickar du **MainPage.xaml**, och klicka sedan på **Visa kod**.</span><span class="sxs-lookup"><span data-stu-id="07879-219">From Solution Explorer, right click **MainPage.xaml**, and then click **View Code**.</span></span>
2. <span data-ttu-id="07879-220">Hello början av hello-fil, lägger du till hello följande med instruktionen:</span><span class="sxs-lookup"><span data-stu-id="07879-220">At hello beginning of hello file, add hello following using statement:</span></span>

        using Microsoft.Media.AdaptiveStreaming;
3. <span data-ttu-id="07879-221">Lägg till följande datamedlemmar hello hello början av hello MainPage klass:</span><span class="sxs-lookup"><span data-stu-id="07879-221">At hello beginning of hello MainPage class, add hello following data members:</span></span>

         private Windows.Foundation.Collections.PropertySet propertySet = new Windows.Foundation.Collections.PropertySet();             
         private IAdaptiveSourceManager adaptiveSourceManager;
4. <span data-ttu-id="07879-222">I hello **MainPage** konstruktor, Lägg till följande kod efter hello hello **detta. Initiera Components();**  rad och hello registrering kodraderna som skrivits i hello föregående lektionen:</span><span class="sxs-lookup"><span data-stu-id="07879-222">Inside hello **MainPage** constructor, add hello following code after hello **this.Initialize Components();** line and hello registration code lines written in hello previous lesson:</span></span>

        // Gets hello default instance of AdaptiveSourceManager which manages Smooth 
        //Streaming media sources.
        adaptiveSourceManager = AdaptiveSourceManager.GetDefault();
        // Sets property key value tooAdaptiveSourceManager default instance.
        // {A5CE1DE8-1D00-427B-ACEF-FB9A3C93DE2D}" must be hardcoded.
        propertySet["{A5CE1DE8-1D00-427B-ACEF-FB9A3C93DE2D}"] = adaptiveSourceManager;
5. <span data-ttu-id="07879-223">I hello **MainPage** konstruktor, ändra hello två RegisterByteStreamHandler metoder tooadd hello tillbaka parametrar:</span><span class="sxs-lookup"><span data-stu-id="07879-223">Inside hello **MainPage** constructor, modify hello two RegisterByteStreamHandler methods tooadd hello forth parameters:</span></span>

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
6. <span data-ttu-id="07879-224">Tryck på **CTRL + S** toosave hello-filen.</span><span class="sxs-lookup"><span data-stu-id="07879-224">Press **CTRL+S** toosave hello file.</span></span>

<span data-ttu-id="07879-225">**tooadd hello nivån händelsehanteraren för anpassningsbar källa manager**</span><span class="sxs-lookup"><span data-stu-id="07879-225">**tooadd hello adaptive source manager level event handler**</span></span>

1. <span data-ttu-id="07879-226">Från Solution Explorer högerklickar du **MainPage.xaml**, och klicka sedan på **Visa kod**.</span><span class="sxs-lookup"><span data-stu-id="07879-226">From Solution Explorer, right click **MainPage.xaml**, and then click **View Code**.</span></span>
2. <span data-ttu-id="07879-227">I hello **MainPage** klassen och Lägg till följande datamedlemmen hello:</span><span class="sxs-lookup"><span data-stu-id="07879-227">Inside hello **MainPage** class, add hello following data member:</span></span>
   
     <span data-ttu-id="07879-228">privata AdaptiveSource adaptiveSource = null;</span><span class="sxs-lookup"><span data-stu-id="07879-228">private AdaptiveSource adaptiveSource = null;</span></span>
3. <span data-ttu-id="07879-229">Hello slutet av hello **MainPage** klassen och Lägg till följande händelsehanteraren hello:</span><span class="sxs-lookup"><span data-stu-id="07879-229">At hello end of hello **MainPage** class, add hello following event handler:</span></span>
   
         # region Adaptive Source Manager Level Events
         private void mediaElement_AdaptiveSourceOpened(AdaptiveSource sender, AdaptiveSourceOpenedEventArgs args)
         {

            adaptiveSource = args.AdaptiveSource;
         }

         # endregion Adaptive Source Manager Level Events
4. <span data-ttu-id="07879-230">Hello slutet av hello **MainPage** konstruktor, Lägg till följande rad toosubscribe toohello anpassningsbar källa Öppna händelsen hello:</span><span class="sxs-lookup"><span data-stu-id="07879-230">At hello end of hello **MainPage** constructor, add hello following line toosubscribe toohello adaptive source open event:</span></span>
   
         adaptiveSourceManager.AdaptiveSourceOpenedEvent += 
           new AdaptiveSourceOpenedEventHandler(mediaElement_AdaptiveSourceOpened);
5. <span data-ttu-id="07879-231">Tryck på **CTRL + S** toosave hello-filen.</span><span class="sxs-lookup"><span data-stu-id="07879-231">Press **CTRL+S** toosave hello file.</span></span>

<span data-ttu-id="07879-232">**tooadd anpassningsbar källa nivån händelsehanterare**</span><span class="sxs-lookup"><span data-stu-id="07879-232">**tooadd adaptive source level event handlers**</span></span>

1. <span data-ttu-id="07879-233">Från Solution Explorer högerklickar du **MainPage.xaml**, och klicka sedan på **Visa kod**.</span><span class="sxs-lookup"><span data-stu-id="07879-233">From Solution Explorer, right click **MainPage.xaml**, and then click **View Code**.</span></span>
2. <span data-ttu-id="07879-234">I hello **MainPage** klassen och Lägg till följande datamedlemmen hello:</span><span class="sxs-lookup"><span data-stu-id="07879-234">Inside hello **MainPage** class, add hello following data member:</span></span>
   
     <span data-ttu-id="07879-235">privata AdaptiveSourceStatusUpdatedEventArgs adaptiveSourceStatusUpdate;   privata manifestet manifestObject;</span><span class="sxs-lookup"><span data-stu-id="07879-235">private AdaptiveSourceStatusUpdatedEventArgs adaptiveSourceStatusUpdate;   private Manifest manifestObject;</span></span>
3. <span data-ttu-id="07879-236">Hello slutet av hello **MainPage** klassen och Lägg till följande händelsehanterare hello:</span><span class="sxs-lookup"><span data-stu-id="07879-236">At hello end of hello **MainPage** class, add hello following event handlers:</span></span>

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
4. <span data-ttu-id="07879-237">Hello slutet av hello **mediaElement AdaptiveSourceOpened** metoden Lägg till följande kod toosubscribe toohello händelser hello:</span><span class="sxs-lookup"><span data-stu-id="07879-237">At hello end of hello **mediaElement AdaptiveSourceOpened** method, add hello following code toosubscribe toohello events:</span></span>
   
         adaptiveSource.ManifestReadyEvent +=

                    mediaElement_ManifestReady;
         adaptiveSource.AdaptiveSourceStatusUpdatedEvent += 

            mediaElement_AdaptiveSourceStatusUpdated;
         adaptiveSource.AdaptiveSourceFailedEvent += 

            mediaElement_AdaptiveSourceFailed;
5. <span data-ttu-id="07879-238">Tryck på **CTRL + S** toosave hello-filen.</span><span class="sxs-lookup"><span data-stu-id="07879-238">Press **CTRL+S** toosave hello file.</span></span>

<span data-ttu-id="07879-239">hello samma händelser som är tillgängliga på anpassningsbar källa Manager nivå, som kan användas för att hantera funktioner tooall mediaelement som är vanliga i hello app.</span><span class="sxs-lookup"><span data-stu-id="07879-239">hello same events are available on Adaptive Source manger level as well, which can be used for handling functionality common tooall media elements in hello app.</span></span> <span data-ttu-id="07879-240">Varje AdaptiveSource innehåller sina egna händelser och alla AdaptiveSource händelser ska vara överlappande under AdaptiveSourceManager.</span><span class="sxs-lookup"><span data-stu-id="07879-240">Each AdaptiveSource includes its own events and all AdaptiveSource events will be cascaded under AdaptiveSourceManager.</span></span>

<span data-ttu-id="07879-241">**tooadd mediaelement händelsehanterare**</span><span class="sxs-lookup"><span data-stu-id="07879-241">**tooadd Media Element event handlers**</span></span>

1. <span data-ttu-id="07879-242">Från Solution Explorer högerklickar du **MainPage.xaml**, och klicka sedan på **Visa kod**.</span><span class="sxs-lookup"><span data-stu-id="07879-242">From Solution Explorer, right click **MainPage.xaml**, and then click **View Code**.</span></span>
2. <span data-ttu-id="07879-243">Hello slutet av hello **MainPage** klassen och Lägg till följande händelsehanterare hello:</span><span class="sxs-lookup"><span data-stu-id="07879-243">At hello end of hello **MainPage** class, add hello following event handlers:</span></span>

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
3. <span data-ttu-id="07879-244">Hello slutet av hello **MainPage** konstruktor, Lägg till följande kod toosubscript toohello händelser hello:</span><span class="sxs-lookup"><span data-stu-id="07879-244">At hello end of hello **MainPage** constructor, add hello following code toosubscript toohello events:</span></span>

         mediaElement.MediaOpened += MediaOpened;
         mediaElement.MediaEnded += MediaEnded;
         mediaElement.MediaFailed += MediaFailed;
4. <span data-ttu-id="07879-245">Tryck på **CTRL + S** toosave hello-filen.</span><span class="sxs-lookup"><span data-stu-id="07879-245">Press **CTRL+S** toosave hello file.</span></span>

<span data-ttu-id="07879-246">**tooadd skjutreglaget relaterade streckkod**</span><span class="sxs-lookup"><span data-stu-id="07879-246">**tooadd slider bar related code**</span></span>

1. <span data-ttu-id="07879-247">Från Solution Explorer högerklickar du **MainPage.xaml**, och klicka sedan på **Visa kod**.</span><span class="sxs-lookup"><span data-stu-id="07879-247">From Solution Explorer, right click **MainPage.xaml**, and then click **View Code**.</span></span>
2. <span data-ttu-id="07879-248">Hello början av hello-fil, lägger du till hello följande med instruktionen:</span><span class="sxs-lookup"><span data-stu-id="07879-248">At hello beginning of hello file, add hello following using statement:</span></span>
      
        using Windows.UI.Core;
3. <span data-ttu-id="07879-249">I hello **MainPage** klassen och Lägg till följande datamedlemmar hello:</span><span class="sxs-lookup"><span data-stu-id="07879-249">Inside hello **MainPage** class, add hello following data members:</span></span>
   
         public static CoreDispatcher _dispatcher;
         private DispatcherTimer sliderPositionUpdateDispatcher;
4. <span data-ttu-id="07879-250">Hello slutet av hello **MainPage** konstruktor, Lägg till följande kod hello:</span><span class="sxs-lookup"><span data-stu-id="07879-250">At hello end of hello **MainPage** constructor, add hello following code:</span></span>
   
         _dispatcher = Window.Current.Dispatcher;
         PointerEventHandler pointerpressedhandler = new PointerEventHandler(sliderProgress_PointerPressed);
         sliderProgress.AddHandler(Control.PointerPressedEvent, pointerpressedhandler, true);    
5. <span data-ttu-id="07879-251">Hello slutet av hello **MainPage** klassen och Lägg till följande kod hello:</span><span class="sxs-lookup"><span data-stu-id="07879-251">At hello end of hello **MainPage** class, add hello following code:</span></span>

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
><span data-ttu-id="07879-252">CoreDispatcher är används toomake ändringarna toohello UI-tråden från icke-UI-tråden.</span><span class="sxs-lookup"><span data-stu-id="07879-252">CoreDispatcher is used toomake changes toohello UI thread from non UI Thread.</span></span> <span data-ttu-id="07879-253">Utvecklare kan välja toouse dispatcher om han eller hon har för avsikt tooupdate tillhandahålls av UI-element för vid flaskhals i dispatcher-tråden.</span><span class="sxs-lookup"><span data-stu-id="07879-253">In case of bottleneck on dispatcher thread, developer can choose toouse dispatcher provided by UI-element he/she intends tooupdate.</span></span>  <span data-ttu-id="07879-254">Exempel:</span><span class="sxs-lookup"><span data-stu-id="07879-254">For example:</span></span>
   
         await sliderProgress.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () => { TimeSpan 

         timespan = new TimeSpan(adaptiveSourceStatusUpdate.EndTime); 
         double absvalue  = (int)Math.Round(timespan.TotalSeconds, MidpointRounding.AwayFromZero); 

         sliderProgress.Maximum = absvalue; }); 
6. <span data-ttu-id="07879-255">Hello slutet av hello **mediaElement_AdaptiveSourceStatusUpdated** metod, Lägg till följande kod hello:</span><span class="sxs-lookup"><span data-stu-id="07879-255">At hello end of hello **mediaElement_AdaptiveSourceStatusUpdated** method, add hello following code:</span></span>

         setSliderStartTime(args.StartTime);
         setSliderEndTime(args.EndTime);
7. <span data-ttu-id="07879-256">Hello slutet av hello **MediaOpened** metod, Lägg till följande kod hello:</span><span class="sxs-lookup"><span data-stu-id="07879-256">At hello end of hello **MediaOpened** method, add hello following code:</span></span>

         sliderProgress.StepFrequency = SliderFrequency(mediaElement.NaturalDuration.TimeSpan);
         sliderProgress.Width = mediaElement.Width;
         setupTimer();
8. <span data-ttu-id="07879-257">Tryck på **CTRL + S** toosave hello-filen.</span><span class="sxs-lookup"><span data-stu-id="07879-257">Press **CTRL+S** toosave hello file.</span></span>

<span data-ttu-id="07879-258">**toocompile och testa hello-program**</span><span class="sxs-lookup"><span data-stu-id="07879-258">**toocompile and test hello application**</span></span>

1. <span data-ttu-id="07879-259">Tryck på **F6** toocompile hello projektet.</span><span class="sxs-lookup"><span data-stu-id="07879-259">Press **F6** toocompile hello project.</span></span> 
2. <span data-ttu-id="07879-260">Tryck på **F5** toorun hello program.</span><span class="sxs-lookup"><span data-stu-id="07879-260">Press **F5** toorun hello application.</span></span>
3. <span data-ttu-id="07879-261">Överst hello i hello program, kan du använder hello standard URL för Smooth Streaming eller ange en annan.</span><span class="sxs-lookup"><span data-stu-id="07879-261">At hello top of hello application, you can either use hello default Smooth Streaming URL or enter a different one.</span></span> 
4. <span data-ttu-id="07879-262">Klicka på **Ange källa**.</span><span class="sxs-lookup"><span data-stu-id="07879-262">Click **Set Source**.</span></span> 
5. <span data-ttu-id="07879-263">Testa hello skjutreglaget.</span><span class="sxs-lookup"><span data-stu-id="07879-263">Test hello slider bar.</span></span>

<span data-ttu-id="07879-264">Du har slutfört lektionen 2.</span><span class="sxs-lookup"><span data-stu-id="07879-264">You have completed lesson 2.</span></span>  <span data-ttu-id="07879-265">Lägga till en skjutreglaget tooapplication i den här lektionen.</span><span class="sxs-lookup"><span data-stu-id="07879-265">In this lesson you added a slider tooapplication.</span></span> 

## <a name="lesson-3-select-smooth-streaming-streams"></a><span data-ttu-id="07879-266">Lektionen 3: Välj Smooth Streaming-dataströmmar</span><span class="sxs-lookup"><span data-stu-id="07879-266">Lesson 3: Select Smooth Streaming Streams</span></span>
<span data-ttu-id="07879-267">Smooth Streaming är kompatibla toostream innehåll med flera språk ljud spår som är valbara av hello-visningsprogram.</span><span class="sxs-lookup"><span data-stu-id="07879-267">Smooth Streaming is capable toostream content with multiple language audio tracks that are selectable by hello viewers.</span></span>  <span data-ttu-id="07879-268">I kursen ska du aktivera visningsprogram tooselect dataströmmar.</span><span class="sxs-lookup"><span data-stu-id="07879-268">In this lesson, you will enable viewers tooselect streams.</span></span> <span data-ttu-id="07879-269">Den här lektionen innehåller hello följande procedurer:</span><span class="sxs-lookup"><span data-stu-id="07879-269">This lesson contains hello following procedures:</span></span>

1. <span data-ttu-id="07879-270">Ändra hello XAML-fil</span><span class="sxs-lookup"><span data-stu-id="07879-270">Modify hello XAML file</span></span>
2. <span data-ttu-id="07879-271">Ändra hello kodfilen behand</span><span class="sxs-lookup"><span data-stu-id="07879-271">Modify hello code behand file</span></span>
3. <span data-ttu-id="07879-272">Kompilera och testa programmet hello</span><span class="sxs-lookup"><span data-stu-id="07879-272">Compile and test hello application</span></span>

<span data-ttu-id="07879-273">**toomodify hello XAML-fil**</span><span class="sxs-lookup"><span data-stu-id="07879-273">**toomodify hello XAML file**</span></span>

1. <span data-ttu-id="07879-274">Från Solution Explorer högerklickar du på **MainPage.xaml**, och klicka sedan på **Vydesigner**.</span><span class="sxs-lookup"><span data-stu-id="07879-274">From Solution Explorer, right-click **MainPage.xaml**, and then click **View Designer**.</span></span>
2. <span data-ttu-id="07879-275">Leta upp &lt;Grid.RowDefinitions&gt;, och ändra hello RowDefinitions så de ser ut som:</span><span class="sxs-lookup"><span data-stu-id="07879-275">Locate &lt;Grid.RowDefinitions&gt;, and modify hello RowDefinitions so they looks like:</span></span>
   
         <Grid.RowDefinitions>            
            <RowDefinition Height="20"/>
            <RowDefinition Height="50"/>
            <RowDefinition Height="100"/>
            <RowDefinition Height="80"/>
            <RowDefinition Height="50"/>
         </Grid.RowDefinitions>
3. <span data-ttu-id="07879-276">I hello &lt;rutnätet&gt;&lt;/Grid&gt; taggar, lägga till hello följande kod toodefine en listruta så att användarna kan se hello lista över tillgängliga dataströmmar och välja dataströmmar:</span><span class="sxs-lookup"><span data-stu-id="07879-276">Inside hello &lt;Grid&gt;&lt;/Grid&gt; tags, add hello following code toodefine a listbox control, so users can see hello list of available streams, and select streams:</span></span>

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
4. <span data-ttu-id="07879-277">Tryck på **CTRL + S** toosave hello ändringar.</span><span class="sxs-lookup"><span data-stu-id="07879-277">Press **CTRL+S** toosave hello changes.</span></span>

<span data-ttu-id="07879-278">**toomodify hello koden bakom filen**</span><span class="sxs-lookup"><span data-stu-id="07879-278">**toomodify hello code behind file**</span></span>

1. <span data-ttu-id="07879-279">Från Solution Explorer högerklickar du på **MainPage.xaml**, och klicka sedan på **Visa kod**.</span><span class="sxs-lookup"><span data-stu-id="07879-279">From Solution Explorer, right-click **MainPage.xaml**, and then click **View Code**.</span></span>
2. <span data-ttu-id="07879-280">Lägg till en ny klass i hello SSPlayer namnområde:</span><span class="sxs-lookup"><span data-stu-id="07879-280">Inside hello SSPlayer namespace, add a new class:</span></span>
   
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
3. <span data-ttu-id="07879-281">Hello början av hello MainPage klass, lägger du till hello följande definitioner av variabeln:</span><span class="sxs-lookup"><span data-stu-id="07879-281">At hello beginning of hello MainPage class, add hello following variable definitions:</span></span>
   
         private List<Stream> availableStreams;
         private List<Stream> availableAudioStreams;
         private List<Stream> availableTextStreams;
         private List<Stream> availableVideoStreams;
4. <span data-ttu-id="07879-282">Lägg till hello efter region i hello MainPage klass:</span><span class="sxs-lookup"><span data-stu-id="07879-282">Inside hello MainPage class, add hello following region:</span></span>
   
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
5. <span data-ttu-id="07879-283">Leta upp hello mediaElement_ManifestReady metod, Lägg till följande kod hello slutet av hello funktionen hello:</span><span class="sxs-lookup"><span data-stu-id="07879-283">Locate hello mediaElement_ManifestReady method, append hello following code at hello end of hello function:</span></span>
   
        getStreams(manifestObject);
        refreshAvailableStreamsListBoxItemSource();
   
    <span data-ttu-id="07879-284">Så när MediaElement manifestet är klar, hello kod hämtar en lista över tillgängliga hello-dataströmmar och fyller hello UI listruta med hello lista.</span><span class="sxs-lookup"><span data-stu-id="07879-284">So when MediaElement manifest is ready, hello code gets a list of hello available streams, and populates hello UI list box with hello list.</span></span>
6. <span data-ttu-id="07879-285">Inuti hello MainPage klass, hitta hello UI knappar klickar du på händelser region och Lägg sedan till följande funktionsdefinitionen hello:</span><span class="sxs-lookup"><span data-stu-id="07879-285">Inside hello MainPage class, locate hello UI buttons click events region, and then add hello following function definition:</span></span>
   
        private void btnChangeStream_Click(object sender, RoutedEventArgs e)
        {
            List<IManifestStream> selectedStreams = new List<IManifestStream>();
   
            // Create a list of hello selected streams
            createSelectedStreamsList(selectedStreams);
   
            // Change streams on hello presentation
            changeStreams(selectedStreams);
        }

<span data-ttu-id="07879-286">**toocompile och testa hello-program**</span><span class="sxs-lookup"><span data-stu-id="07879-286">**toocompile and test hello application**</span></span>

1. <span data-ttu-id="07879-287">Tryck på **F6** toocompile hello projektet.</span><span class="sxs-lookup"><span data-stu-id="07879-287">Press **F6** toocompile hello project.</span></span> 
2. <span data-ttu-id="07879-288">Tryck på **F5** toorun hello program.</span><span class="sxs-lookup"><span data-stu-id="07879-288">Press **F5** toorun hello application.</span></span>
3. <span data-ttu-id="07879-289">Överst hello i hello program, kan du använder hello standard URL för Smooth Streaming eller ange en annan.</span><span class="sxs-lookup"><span data-stu-id="07879-289">At hello top of hello application, you can either use hello default Smooth Streaming URL or enter a different one.</span></span> 
4. <span data-ttu-id="07879-290">Klicka på **Ange källa**.</span><span class="sxs-lookup"><span data-stu-id="07879-290">Click **Set Source**.</span></span> 
5. <span data-ttu-id="07879-291">hello standardspråket är audio_eng.</span><span class="sxs-lookup"><span data-stu-id="07879-291">hello default language is audio_eng.</span></span> <span data-ttu-id="07879-292">Försök tooswitch mellan audio_eng och audio_es.</span><span class="sxs-lookup"><span data-stu-id="07879-292">Try tooswitch between audio_eng and audio_es.</span></span> <span data-ttu-id="07879-293">Varje gång, väljer du en ny ström, måste du klicka på knappen Skicka för hello.</span><span class="sxs-lookup"><span data-stu-id="07879-293">Everytime, you select a new stream, you must click hello Submit button.</span></span>

<span data-ttu-id="07879-294">Du har slutfört lektionen 3.</span><span class="sxs-lookup"><span data-stu-id="07879-294">You have completed lesson 3.</span></span>  <span data-ttu-id="07879-295">Nu bör du lägga till hello funktioner toochoose dataströmmar.</span><span class="sxs-lookup"><span data-stu-id="07879-295">In this lesson, you add hello functionality toochoose streams.</span></span>

## <a name="lesson-4-select-smooth-streaming-tracks"></a><span data-ttu-id="07879-296">Lektionen 4: Välj Smooth Streaming spårar</span><span class="sxs-lookup"><span data-stu-id="07879-296">Lesson 4: Select Smooth Streaming Tracks</span></span>
<span data-ttu-id="07879-297">En Smooth Streaming-presentation kan innehålla flera videofiler kodad med olika kvalitet (bithastigheter) och lösningar.</span><span class="sxs-lookup"><span data-stu-id="07879-297">A Smooth Streaming presentation can contain multiple video files encoded with different quality levels (bit rates) and resolutions.</span></span> <span data-ttu-id="07879-298">I kursen ska du aktivera användare tooselect spår.</span><span class="sxs-lookup"><span data-stu-id="07879-298">In this lesson, you will enable users tooselect tracks.</span></span> <span data-ttu-id="07879-299">Den här lektionen innehåller hello följande procedurer:</span><span class="sxs-lookup"><span data-stu-id="07879-299">This lesson contains hello following procedures:</span></span>

1. <span data-ttu-id="07879-300">Ändra hello XAML-fil</span><span class="sxs-lookup"><span data-stu-id="07879-300">Modify hello XAML file</span></span>
2. <span data-ttu-id="07879-301">Ändra hello kodfilen behand</span><span class="sxs-lookup"><span data-stu-id="07879-301">Modify hello code behand file</span></span>
3. <span data-ttu-id="07879-302">Kompilera och testa programmet hello</span><span class="sxs-lookup"><span data-stu-id="07879-302">Compile and test hello application</span></span>

<span data-ttu-id="07879-303">**toomodify hello XAML-fil**</span><span class="sxs-lookup"><span data-stu-id="07879-303">**toomodify hello XAML file**</span></span>

1. <span data-ttu-id="07879-304">Från Solution Explorer högerklickar du på **MainPage.xaml**, och klicka sedan på **Vydesigner**.</span><span class="sxs-lookup"><span data-stu-id="07879-304">From Solution Explorer, right-click **MainPage.xaml**, and then click **View Designer**.</span></span>
2. <span data-ttu-id="07879-305">Hitta hello &lt;rutnätet&gt; tagg med namnet hello **gridStreamAndBitrateSelection**, Lägg till hello följande kod hello slutet av hello-taggen:</span><span class="sxs-lookup"><span data-stu-id="07879-305">Locate hello &lt;Grid&gt; tag with hello name **gridStreamAndBitrateSelection**, append hello following code at hello end of hello tag:</span></span>
   
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
3. <span data-ttu-id="07879-306">Tryck på **CTRL + S** toosave ändrar han</span><span class="sxs-lookup"><span data-stu-id="07879-306">Press **CTRL+S** toosave he changes</span></span>

<span data-ttu-id="07879-307">**toomodify hello koden bakom filen**</span><span class="sxs-lookup"><span data-stu-id="07879-307">**toomodify hello code behind file**</span></span>

1. <span data-ttu-id="07879-308">Från Solution Explorer högerklickar du på **MainPage.xaml**, och klicka sedan på **Visa kod**.</span><span class="sxs-lookup"><span data-stu-id="07879-308">From Solution Explorer, right-click **MainPage.xaml**, and then click **View Code**.</span></span>
2. <span data-ttu-id="07879-309">Lägg till en ny klass i hello SSPlayer namnområde:</span><span class="sxs-lookup"><span data-stu-id="07879-309">Inside hello SSPlayer namespace, add a new class:</span></span>
   
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
3. <span data-ttu-id="07879-310">Hello början av hello MainPage klass, lägger du till hello följande definitioner av variabeln:</span><span class="sxs-lookup"><span data-stu-id="07879-310">At hello beginning of hello MainPage class, add hello following variable definitions:</span></span>
   
        private List<Track> availableTracks;
4. <span data-ttu-id="07879-311">Lägg till hello efter region i hello MainPage klass:</span><span class="sxs-lookup"><span data-stu-id="07879-311">Inside hello MainPage class, add hello following region:</span></span>
   
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
5. <span data-ttu-id="07879-312">Leta upp hello mediaElement_ManifestReady metod, Lägg till följande kod hello slutet av hello funktionen hello:</span><span class="sxs-lookup"><span data-stu-id="07879-312">Locate hello mediaElement_ManifestReady method, append hello following code at hello end of hello function:</span></span>
   
         getTracks(manifestObject);
         refreshAvailableTracksListBoxItemSource();
6. <span data-ttu-id="07879-313">Inuti hello MainPage klass, hitta hello UI knappar klickar du på händelser region och Lägg sedan till följande funktionsdefinitionen hello:</span><span class="sxs-lookup"><span data-stu-id="07879-313">Inside hello MainPage class, locate hello UI buttons click events region, and then add hello following function definition:</span></span>
   
         private void btnChangeStream_Click(object sender, RoutedEventArgs e)
         {
            List<IManifestStream> selectedStreams = new List<IManifestStream>();

            // Create a list of hello selected streams
            createSelectedStreamsList(selectedStreams);

            // Change streams on hello presentation
            changeStreams(selectedStreams);
         }

<span data-ttu-id="07879-314">**toocompile och testa hello-program**</span><span class="sxs-lookup"><span data-stu-id="07879-314">**toocompile and test hello application**</span></span>

1. <span data-ttu-id="07879-315">Tryck på **F6** toocompile hello projektet.</span><span class="sxs-lookup"><span data-stu-id="07879-315">Press **F6** toocompile hello project.</span></span> 
2. <span data-ttu-id="07879-316">Tryck på **F5** toorun hello program.</span><span class="sxs-lookup"><span data-stu-id="07879-316">Press **F5** toorun hello application.</span></span>
3. <span data-ttu-id="07879-317">Överst hello i hello program, kan du använder hello standard URL för Smooth Streaming eller ange en annan.</span><span class="sxs-lookup"><span data-stu-id="07879-317">At hello top of hello application, you can either use hello default Smooth Streaming URL or enter a different one.</span></span> 
4. <span data-ttu-id="07879-318">Klicka på **Ange källa**.</span><span class="sxs-lookup"><span data-stu-id="07879-318">Click **Set Source**.</span></span> 
5. <span data-ttu-id="07879-319">Alla hello spårar hello video dataströmmen är markerade som standard.</span><span class="sxs-lookup"><span data-stu-id="07879-319">By default, all of hello tracks of hello video stream are selected.</span></span> <span data-ttu-id="07879-320">tooexperiment hello bit ändringar, du kan välja hello lägsta bithastighet tillgänglig och sedan välja hello högsta överföringshastighet tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="07879-320">tooexperiment hello bit rate changes, you can select hello lowest bit rate available, and then select hello highest bit rate available.</span></span> <span data-ttu-id="07879-321">Du måste klicka på Skicka efter varje ändring.</span><span class="sxs-lookup"><span data-stu-id="07879-321">You must click Submit after each change.</span></span>  <span data-ttu-id="07879-322">Du kan se hello bildkvaliteten ändringar.</span><span class="sxs-lookup"><span data-stu-id="07879-322">You can see hello video quality changes.</span></span>

<span data-ttu-id="07879-323">Du har slutfört lektionen 4.</span><span class="sxs-lookup"><span data-stu-id="07879-323">You have completed lesson 4.</span></span>  <span data-ttu-id="07879-324">Nu bör du lägga till hello funktioner toochoose spårar.</span><span class="sxs-lookup"><span data-stu-id="07879-324">In this lesson, you add hello functionality toochoose tracks.</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="07879-325">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="07879-325">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="07879-326">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="07879-326">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="other-resources"></a><span data-ttu-id="07879-327">Andra resurser:</span><span class="sxs-lookup"><span data-stu-id="07879-327">Other Resources:</span></span>
* [<span data-ttu-id="07879-328">Hur toobuild ett Smooth Streaming JavaScript för Windows 8-program med avancerade funktioner</span><span class="sxs-lookup"><span data-stu-id="07879-328">How toobuild a Smooth Streaming Windows 8 JavaScript application with advanced features</span></span>](http://blogs.iis.net/cenkd/archive/2012/08/10/how-to-build-a-smooth-streaming-windows-8-javascript-application-with-advanced-features.aspx)
* [<span data-ttu-id="07879-329">Smooth Streaming teknisk översikt</span><span class="sxs-lookup"><span data-stu-id="07879-329">Smooth Streaming Technical Overview</span></span>](http://www.iis.net/learn/media/on-demand-smooth-streaming/smooth-streaming-technical-overview)

[PlayerApplication]: ./media/media-services-build-smooth-streaming-apps/SSClientWin8-1.png
[CodeViewPic]: ./media/media-services-build-smooth-streaming-apps/SSClientWin8-2.png

