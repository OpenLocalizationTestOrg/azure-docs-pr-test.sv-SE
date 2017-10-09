---
title: "aaaHow toouse Blob Storage från Xamarin | Microsoft Docs"
description: "hello Azure Storage-klientbibliotek för Xamarin kan utvecklare toocreate iOS, Android och Windows Store-appar med sina ursprungliga användargränssnitt. Den här kursen visar hur toouse Xamarin toocreate ett program som använder Azure Blob storage."
services: storage
documentationcenter: xamarin
author: michaelhauss
manager: vamshik
editor: tysonn
ms.assetid: 44cb845d-cf78-4942-95b8-952da4f9a2c2
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 05/11/2017
ms.author: michaelhauss
ms.openlocfilehash: 751f66d1d2392c8bcf6e5f8d1b185af73582fab3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-blob-storage-from-xamarin"></a><span data-ttu-id="ed8cf-104">Hur toouse Blob Storage från Xamarin</span><span class="sxs-lookup"><span data-stu-id="ed8cf-104">How toouse Blob Storage from Xamarin</span></span>
[!INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]

## <a name="overview"></a><span data-ttu-id="ed8cf-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="ed8cf-105">Overview</span></span>
<span data-ttu-id="ed8cf-106">Xamarin kan utvecklare toouse delade C# codebase toocreate iOS, Android och Windows Store-appar med sina ursprungliga användargränssnitt.</span><span class="sxs-lookup"><span data-stu-id="ed8cf-106">Xamarin enables developers toouse a shared C# codebase toocreate iOS, Android, and Windows Store apps with their native user interfaces.</span></span> <span data-ttu-id="ed8cf-107">De här självstudierna visar hur toouse Azure Blob storage med en Xamarin-App.</span><span class="sxs-lookup"><span data-stu-id="ed8cf-107">This tutorial shows you how toouse Azure Blob storage with a Xamarin application.</span></span> <span data-ttu-id="ed8cf-108">Om du vill toolearn mer om Azure Storage innan du dyker in hello koden Se [introduktion tooMicrosoft Azure Storage](storage-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="ed8cf-108">If you'd like toolearn more about Azure Storage, before diving into hello code, see [Introduction tooMicrosoft Azure Storage](storage-introduction.md).</span></span>

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

[!INCLUDE [storage-mobile-authentication-guidance](../../includes/storage-mobile-authentication-guidance.md)]

## <a name="create-a-new-xamarin-application"></a><span data-ttu-id="ed8cf-109">Skapa en ny Xamarin-App</span><span class="sxs-lookup"><span data-stu-id="ed8cf-109">Create a new Xamarin Application</span></span>
<span data-ttu-id="ed8cf-110">Den här kursen ska vi skapa en app som riktar sig till Android, iOS och Windows.</span><span class="sxs-lookup"><span data-stu-id="ed8cf-110">For this tutorial, we'll be creating an app that targets Android, iOS, and Windows.</span></span> <span data-ttu-id="ed8cf-111">Den här appen kommer bara att skapa en behållare och ladda upp en blobb till den här behållaren.</span><span class="sxs-lookup"><span data-stu-id="ed8cf-111">This app will simply create a container and upload a blob into this container.</span></span> <span data-ttu-id="ed8cf-112">Vi ska använda Visual Studio i Windows, men samma learnings kan användas när du skapar en app med Xamarin Studio på macOS hello.</span><span class="sxs-lookup"><span data-stu-id="ed8cf-112">We'll be using Visual Studio on Windows, but hello same learnings can be applied when creating an app using Xamarin Studio on macOS.</span></span>

<span data-ttu-id="ed8cf-113">Följ dessa steg toocreate ditt program:</span><span class="sxs-lookup"><span data-stu-id="ed8cf-113">Follow these steps toocreate your application:</span></span>

1. <span data-ttu-id="ed8cf-114">Om du inte redan har gjort ladda ned och installera [Xamarin för Visual Studio](https://www.xamarin.com/download).</span><span class="sxs-lookup"><span data-stu-id="ed8cf-114">If you haven't already, download and install [Xamarin for Visual Studio](https://www.xamarin.com/download).</span></span>
2. <span data-ttu-id="ed8cf-115">Öppna Visual Studio och skapa en tom App (intern bärbara): **fil > Nytt > Projekt > plattformsoberoende > tom App(Native Portable)**.</span><span class="sxs-lookup"><span data-stu-id="ed8cf-115">Open Visual Studio, and create a Blank App (Native Portable): **File > New > Project > Cross-Platform > Blank App(Native Portable)**.</span></span>
3. <span data-ttu-id="ed8cf-116">Högerklicka på lösningen i hello Solution Explorer-fönstret och välj **hantera NuGet-paket för lösningen**.</span><span class="sxs-lookup"><span data-stu-id="ed8cf-116">Right-click your solution in hello Solution Explorer pane and select **Manage NuGet Packages for Solution**.</span></span> <span data-ttu-id="ed8cf-117">Sök efter **WindowsAzure.Storage** och installera hello senaste stabil version tooall projekten i din lösning.</span><span class="sxs-lookup"><span data-stu-id="ed8cf-117">Search for **WindowsAzure.Storage** and install hello latest stable version tooall projects in your solution.</span></span>
4. <span data-ttu-id="ed8cf-118">Skapa och köra projektet.</span><span class="sxs-lookup"><span data-stu-id="ed8cf-118">Build and run your project.</span></span>

<span data-ttu-id="ed8cf-119">Du bör nu ha ett program som gör att du tooclick en knapp som ökar en räknare.</span><span class="sxs-lookup"><span data-stu-id="ed8cf-119">You should now have an application that allows you tooclick a button which increments a counter.</span></span>

## <a name="create-container-and-upload-blob"></a><span data-ttu-id="ed8cf-120">Skapa en behållare och ladda upp blob</span><span class="sxs-lookup"><span data-stu-id="ed8cf-120">Create container and upload blob</span></span>
<span data-ttu-id="ed8cf-121">Sedan under din `(Portable)` projekt, ska du lägga till kod för`MyClass.cs`.</span><span class="sxs-lookup"><span data-stu-id="ed8cf-121">Next, under your `(Portable)` project, you'll add some code too`MyClass.cs`.</span></span> <span data-ttu-id="ed8cf-122">Den här koden skapar en behållare och laddar upp en blob till den här behållaren.</span><span class="sxs-lookup"><span data-stu-id="ed8cf-122">This code creates a container and uploads a blob into this container.</span></span> <span data-ttu-id="ed8cf-123">`MyClass.cs`bör se ut hello följande:</span><span class="sxs-lookup"><span data-stu-id="ed8cf-123">`MyClass.cs` should look like hello following:</span></span>

```csharp
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Blob;
using System.Threading.Tasks;

namespace XamarinApp
{
    public class MyClass
    {
        public MyClass ()
        {
        }

        public static async Task performBlobOperation()
        {
            // Retrieve storage account from connection string.
            CloudStorageAccount storageAccount = CloudStorageAccount.Parse("DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here");

            // Create hello blob client.
            CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

            // Retrieve reference tooa previously created container.
            CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

            // Create hello container if it doesn't already exist.
            await container.CreateIfNotExistsAsync();

            // Retrieve reference tooa blob named "myblob".
            CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

            // Create hello "myblob" blob with hello text "Hello, world!"
            await blockBlob.UploadTextAsync("Hello, world!");
        }
    }
}
```

<span data-ttu-id="ed8cf-124">Kontrollera att tooreplace ”your_account_name_here” och ”your_account_key_here” med faktiska kontonamn och kontonyckel.</span><span class="sxs-lookup"><span data-stu-id="ed8cf-124">Make sure tooreplace "your_account_name_here" and "your_account_key_here" with your actual account name and account key.</span></span> 

<span data-ttu-id="ed8cf-125">Din iOS-, Android och Windows Phone-projekt som alla har referenser tooyour bärbar projekt – vilket innebär att du kan skriva all delad kod i något placera och använda det för alla dina projekt.</span><span class="sxs-lookup"><span data-stu-id="ed8cf-125">Your iOS, Android, and Windows Phone projects all have references tooyour Portable project - meaning you can write all of your shared code in one place and use it across all of your projects.</span></span> <span data-ttu-id="ed8cf-126">Du kan nu lägga till följande rad med kod tooeach projekt toostart dra fördel hello:`MyClass.performBlobOperation()`</span><span class="sxs-lookup"><span data-stu-id="ed8cf-126">You can now add hello following line of code tooeach project toostart taking advantage: `MyClass.performBlobOperation()`</span></span>

### <a name="xamarinappdroid--mainactivitycs"></a><span data-ttu-id="ed8cf-127">XamarinApp.Droid > MainActivity.cs</span><span class="sxs-lookup"><span data-stu-id="ed8cf-127">XamarinApp.Droid > MainActivity.cs</span></span>

```csharp
using Android.App;
using Android.Widget;
using Android.OS;

namespace XamarinApp.Droid
{
    [Activity (Label = "XamarinApp.Droid", MainLauncher = true, Icon = "@drawable/icon")]
    public class MainActivity : Activity
    {
        int count = 1;

        protected override async void OnCreate (Bundle bundle)
        {
            base.OnCreate (bundle);

            // Set our view from hello "main" layout resource
            SetContentView (Resource.Layout.Main);

            // Get our button from hello layout resource,
            // and attach an event tooit
            Button button = FindViewById<Button> (Resource.Id.myButton);

            button.Click += delegate {
                button.Text = string.Format ("{0} clicks!", count++);
            };

            await MyClass.performBlobOperation();
            }
        }
    }
}
```

### <a name="xamarinappios--viewcontrollercs"></a><span data-ttu-id="ed8cf-128">XamarinApp.iOS > ViewController.cs</span><span class="sxs-lookup"><span data-stu-id="ed8cf-128">XamarinApp.iOS > ViewController.cs</span></span>

```csharp
using System;
using UIKit;

namespace XamarinApp.iOS
{
    public partial class ViewController : UIViewController
    {
        int count = 1;

        public ViewController (IntPtr handle) : base (handle)
        {
        }

        public override async void ViewDidLoad ()
        {
            int count = 1;

            public ViewController (IntPtr handle) : base (handle)
            {
            }

            public override async void ViewDidLoad ()
            {
                base.ViewDidLoad ();
                // Perform any additional setup after loading hello view, typically from a nib.
                Button.AccessibilityIdentifier = "myButton";
                Button.TouchUpInside += delegate {
                    var title = string.Format ("{0} clicks!", count++);
                    Button.SetTitle (title, UIControlState.Normal);
                };

                await MyClass.performBlobOperation();
            }

            public override void DidReceiveMemoryWarning ()
            {
                base.DidReceiveMemoryWarning ();
                // Release any cached data, images, etc that aren't in use.
            }
        }
    }
}
```

### <a name="xamarinappwinphone--mainpagexaml--mainpagexamlcs"></a><span data-ttu-id="ed8cf-129">XamarinApp.WinPhone > MainPage.xaml > MainPage.xaml.cs</span><span class="sxs-lookup"><span data-stu-id="ed8cf-129">XamarinApp.WinPhone > MainPage.xaml > MainPage.xaml.cs</span></span>

```csharp
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Navigation;

// hello Blank Page item template is documented at http://go.microsoft.com/fwlink/?LinkId=391641

namespace XamarinApp.WinPhone
{
    /// <summary>
    /// An empty page that can be used on its own or navigated toowithin a Frame.
    /// </summary>
    public sealed partial class MainPage : Page
    {
        int count = 1;

        public MainPage()
        {
            this.InitializeComponent();

            this.NavigationCacheMode = NavigationCacheMode.Required;
        }

        /// <summary>
        /// Invoked when this page is about toobe displayed in a Frame.
        /// </summary>
        /// <param name="e">Event data that describes how this page was reached.
        /// This parameter is typically used tooconfigure hello page.</param>
        protected override async void OnNavigatedTo(NavigationEventArgs e)
        {
            int count = 1;

            public MainPage()
            {
                this.InitializeComponent();

                this.NavigationCacheMode = NavigationCacheMode.Required;
            }

            /// <summary>
            /// Invoked when this page is about toobe displayed in a Frame.
            /// </summary>
            /// <param name="e">Event data that describes how this page was reached.
            /// This parameter is typically used tooconfigure hello page.</param>
            protected override async void OnNavigatedTo(NavigationEventArgs e)
            {
                // TODO: Prepare page for display here.

                // TODO: If your application contains multiple pages, ensure that you are
                // handling hello hardware Back button by registering for the
                // Windows.Phone.UI.Input.HardwareButtons.BackPressed event.
                // If you are using hello NavigationHelper provided by some templates,
                // this event is handled for you.
                Button.Click += delegate {
                    var title = string.Format("{0} clicks!", count++);
                    Button.Content = title;
                };

                await MyClass.performBlobOperation();
            }
        }
    }
}
```

## <a name="run-hello-application"></a><span data-ttu-id="ed8cf-130">Kör programmet hello</span><span class="sxs-lookup"><span data-stu-id="ed8cf-130">Run hello application</span></span>
<span data-ttu-id="ed8cf-131">Du kan nu köra det här programmet i en Android eller Windows Phone-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="ed8cf-131">You can now run this application in an Android or Windows Phone emulator.</span></span> <span data-ttu-id="ed8cf-132">Du kan också köra det här programmet i en iOS-emulatorn, men detta kräver en Mac.</span><span class="sxs-lookup"><span data-stu-id="ed8cf-132">You can also run this application in an iOS emulator, but this will require a Mac.</span></span> <span data-ttu-id="ed8cf-133">Specifika anvisningar för hur toodo, se dokumentationen hello för [ansluter Visual Studio tooa Mac](https://developer.xamarin.com/guides/ios/getting_started/installation/windows/connecting-to-mac/)</span><span class="sxs-lookup"><span data-stu-id="ed8cf-133">For specific instructions on how toodo this, please read hello documentation for [connecting Visual Studio tooa Mac](https://developer.xamarin.com/guides/ios/getting_started/installation/windows/connecting-to-mac/)</span></span>

<span data-ttu-id="ed8cf-134">När du kör din app kommer att skapas hello behållaren `mycontainer` i ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="ed8cf-134">Once you run your app, it will create hello container `mycontainer` in your Storage account.</span></span> <span data-ttu-id="ed8cf-135">Den ska innehålla hello blob `myblob`, som har hello text `Hello, world!`.</span><span class="sxs-lookup"><span data-stu-id="ed8cf-135">It should contain hello blob, `myblob`, which has hello text, `Hello, world!`.</span></span> <span data-ttu-id="ed8cf-136">Du kan kontrollera detta genom att använda hello [Microsoft Azure Lagringsutforskaren](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="ed8cf-136">You can verify this by using hello [Microsoft Azure Storage Explorer](http://storageexplorer.com/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="ed8cf-137">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ed8cf-137">Next steps</span></span>
<span data-ttu-id="ed8cf-138">I kursen får du lärt dig hur toocreate ett plattformsoberoende program i Xamarin som använder Azure Storage, särskilt fokusera på ett scenario i Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="ed8cf-138">In this tutorial, you learned how toocreate a cross-platform application in Xamarin that uses Azure Storage, specifically focusing on one scenario in Blob Storage.</span></span> <span data-ttu-id="ed8cf-139">Men kan du göra mycket mer med inte bara Blob Storage, utan även med tabell-, fil- och Queue Storage.</span><span class="sxs-lookup"><span data-stu-id="ed8cf-139">However, you can do a lot more with not only Blob Storage, but also with Table, File, and Queue Storage.</span></span> <span data-ttu-id="ed8cf-140">Läs följande artiklar toolearn mer hello:</span><span class="sxs-lookup"><span data-stu-id="ed8cf-140">Please check out hello following articles toolearn more:</span></span>

* [<span data-ttu-id="ed8cf-141">Komma igång med Azure Blob Storage med hjälp av .NET</span><span class="sxs-lookup"><span data-stu-id="ed8cf-141">Get started with Azure Blob storage using .NET</span></span>](storage-dotnet-how-to-use-blobs.md)
* [<span data-ttu-id="ed8cf-142">Komma igång med Azure Table Storage med hjälp av .NET</span><span class="sxs-lookup"><span data-stu-id="ed8cf-142">Get started with Azure Table storage using .NET</span></span>](storage-dotnet-how-to-use-tables.md)
* [<span data-ttu-id="ed8cf-143">Komma igång med Azure Queue Storage med hjälp av .NET</span><span class="sxs-lookup"><span data-stu-id="ed8cf-143">Get started with Azure Queue storage using .NET</span></span>](storage-dotnet-how-to-use-queues.md)
* [<span data-ttu-id="ed8cf-144">Komma igång med Azure File Storage i Windows</span><span class="sxs-lookup"><span data-stu-id="ed8cf-144">Get started with Azure File storage on Windows</span></span>](storage-dotnet-how-to-use-files.md)

[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

