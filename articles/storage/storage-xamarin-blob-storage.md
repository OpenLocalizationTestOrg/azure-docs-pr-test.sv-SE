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
# <a name="how-toouse-blob-storage-from-xamarin"></a>Hur toouse Blob Storage från Xamarin
[!INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]

## <a name="overview"></a>Översikt
Xamarin kan utvecklare toouse delade C# codebase toocreate iOS, Android och Windows Store-appar med sina ursprungliga användargränssnitt. De här självstudierna visar hur toouse Azure Blob storage med en Xamarin-App. Om du vill toolearn mer om Azure Storage innan du dyker in hello koden Se [introduktion tooMicrosoft Azure Storage](storage-introduction.md).

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

[!INCLUDE [storage-mobile-authentication-guidance](../../includes/storage-mobile-authentication-guidance.md)]

## <a name="create-a-new-xamarin-application"></a>Skapa en ny Xamarin-App
Den här kursen ska vi skapa en app som riktar sig till Android, iOS och Windows. Den här appen kommer bara att skapa en behållare och ladda upp en blobb till den här behållaren. Vi ska använda Visual Studio i Windows, men samma learnings kan användas när du skapar en app med Xamarin Studio på macOS hello.

Följ dessa steg toocreate ditt program:

1. Om du inte redan har gjort ladda ned och installera [Xamarin för Visual Studio](https://www.xamarin.com/download).
2. Öppna Visual Studio och skapa en tom App (intern bärbara): **fil > Nytt > Projekt > plattformsoberoende > tom App(Native Portable)**.
3. Högerklicka på lösningen i hello Solution Explorer-fönstret och välj **hantera NuGet-paket för lösningen**. Sök efter **WindowsAzure.Storage** och installera hello senaste stabil version tooall projekten i din lösning.
4. Skapa och köra projektet.

Du bör nu ha ett program som gör att du tooclick en knapp som ökar en räknare.

## <a name="create-container-and-upload-blob"></a>Skapa en behållare och ladda upp blob
Sedan under din `(Portable)` projekt, ska du lägga till kod för`MyClass.cs`. Den här koden skapar en behållare och laddar upp en blob till den här behållaren. `MyClass.cs`bör se ut hello följande:

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

Kontrollera att tooreplace ”your_account_name_here” och ”your_account_key_here” med faktiska kontonamn och kontonyckel. 

Din iOS-, Android och Windows Phone-projekt som alla har referenser tooyour bärbar projekt – vilket innebär att du kan skriva all delad kod i något placera och använda det för alla dina projekt. Du kan nu lägga till följande rad med kod tooeach projekt toostart dra fördel hello:`MyClass.performBlobOperation()`

### <a name="xamarinappdroid--mainactivitycs"></a>XamarinApp.Droid > MainActivity.cs

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

### <a name="xamarinappios--viewcontrollercs"></a>XamarinApp.iOS > ViewController.cs

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

### <a name="xamarinappwinphone--mainpagexaml--mainpagexamlcs"></a>XamarinApp.WinPhone > MainPage.xaml > MainPage.xaml.cs

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

## <a name="run-hello-application"></a>Kör programmet hello
Du kan nu köra det här programmet i en Android eller Windows Phone-emulatorn. Du kan också köra det här programmet i en iOS-emulatorn, men detta kräver en Mac. Specifika anvisningar för hur toodo, se dokumentationen hello för [ansluter Visual Studio tooa Mac](https://developer.xamarin.com/guides/ios/getting_started/installation/windows/connecting-to-mac/)

När du kör din app kommer att skapas hello behållaren `mycontainer` i ditt lagringskonto. Den ska innehålla hello blob `myblob`, som har hello text `Hello, world!`. Du kan kontrollera detta genom att använda hello [Microsoft Azure Lagringsutforskaren](http://storageexplorer.com/).

## <a name="next-steps"></a>Nästa steg
I kursen får du lärt dig hur toocreate ett plattformsoberoende program i Xamarin som använder Azure Storage, särskilt fokusera på ett scenario i Blob Storage. Men kan du göra mycket mer med inte bara Blob Storage, utan även med tabell-, fil- och Queue Storage. Läs följande artiklar toolearn mer hello:

* [Komma igång med Azure Blob Storage med hjälp av .NET](storage-dotnet-how-to-use-blobs.md)
* [Komma igång med Azure Table Storage med hjälp av .NET](storage-dotnet-how-to-use-tables.md)
* [Komma igång med Azure Queue Storage med hjälp av .NET](storage-dotnet-how-to-use-queues.md)
* [Komma igång med Azure File Storage i Windows](storage-dotnet-how-to-use-files.md)

[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

