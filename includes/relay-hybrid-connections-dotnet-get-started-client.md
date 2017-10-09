### <a name="create-a-console-application"></a><span data-ttu-id="485b3-101">Skapa ett konsolprogram</span><span class="sxs-lookup"><span data-stu-id="485b3-101">Create a console application</span></span>

<span data-ttu-id="485b3-102">Börja med att starta Visual Studio och skapa ett nytt projekt: **Konsolprogram (.NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="485b3-102">First, launch Visual Studio and create a new **Console App (.NET Framework)** project.</span></span>

### <a name="add-hello-relay-nuget-package"></a><span data-ttu-id="485b3-103">Lägg till hello Relay NuGet-paketet</span><span class="sxs-lookup"><span data-stu-id="485b3-103">Add hello Relay NuGet package</span></span>

1. <span data-ttu-id="485b3-104">Högerklicka på hello nyskapad projektet och klicka sedan på **hantera NuGet-paket**.</span><span class="sxs-lookup"><span data-stu-id="485b3-104">Right-click hello newly created project and then click **Manage NuGet Packages**.</span></span>
2. <span data-ttu-id="485b3-105">Klicka på hello **Bläddra** fliken och Sök efter ”Microsoft.Azure.Relay” och välj hello **Microsoft Azure-relä** objekt.</span><span class="sxs-lookup"><span data-stu-id="485b3-105">Click hello **Browse** tab, then search for "Microsoft.Azure.Relay" and select hello **Microsoft Azure Relay** item.</span></span> <span data-ttu-id="485b3-106">Klicka på **installera** toocomplete hello installationen och sedan stänga den här dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="485b3-106">Click **Install** toocomplete hello installation, then close this dialog box.</span></span>

### <a name="write-some-code-toosend-messages"></a><span data-ttu-id="485b3-107">Skriva kod toosend meddelanden</span><span class="sxs-lookup"><span data-stu-id="485b3-107">Write some code toosend messages</span></span>

1. <span data-ttu-id="485b3-108">Ersätta befintliga hello `using` instruktioner hello överst i filen för hello Program.cs med följande hello `using` instruktioner:</span><span class="sxs-lookup"><span data-stu-id="485b3-108">Replace hello existing `using` statements at hello top of hello Program.cs file with hello following `using` statements:</span></span>
   
    ```csharp
    using System;
    using System.IO;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.Azure.Relay;
    ```
2. <span data-ttu-id="485b3-109">Lägg till konstanter toohello `Program` klass för hello hybrid anslutningsinformation.</span><span class="sxs-lookup"><span data-stu-id="485b3-109">Add constants toohello `Program` class for hello hybrid connection details.</span></span> <span data-ttu-id="485b3-110">Ersätt hello platshållare inom parentes med hello-värden som du fick när du skapar hello hybridanslutning.</span><span class="sxs-lookup"><span data-stu-id="485b3-110">Replace hello placeholders in brackets with hello values you obtained when creating hello hybrid connection.</span></span> <span data-ttu-id="485b3-111">Vara säker på att toouse hello fullständiga namnområdesnamn:</span><span class="sxs-lookup"><span data-stu-id="485b3-111">Be sure toouse hello fully qualified namespace name:</span></span>
   
    ```csharp
    private const string RelayNamespace = "{RelayNamespace}.servicebus.windows.net";
    private const string ConnectionName = "{HybridConnectionName}";
    private const string KeyName = "{SASKeyName}";
    private const string Key = "{SASKey}";
    ```
3. <span data-ttu-id="485b3-112">Lägg till följande metod toohello hello `Program` klass:</span><span class="sxs-lookup"><span data-stu-id="485b3-112">Add hello following method toohello `Program` class:</span></span>
   
    ```csharp
    private static async Task RunAsync()
    {
        Console.WriteLine("Enter lines of text toosend toohello server with ENTER");
   
        // Create a new hybrid connection client
        var tokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider(KeyName, Key);
        var client = new HybridConnectionClient(new Uri(String.Format("sb://{0}/{1}", RelayNamespace, ConnectionName)), tokenProvider);
   
        // Initiate hello connection
        var relayConnection = await client.CreateConnectionAsync();
   
        // We run two concurrent loops on hello connection. One 
        // reads input from hello console and writes it toohello connection 
        // with a stream writer. hello other reads lines of input from hello 
        // connection with a stream reader and writes them toohello console. 
        // Entering a blank line will shut down hello write task after 
        // sending it toohello server. hello server will then cleanly shut down
        // hello connection which will terminate hello read task.
   
        var reads = Task.Run(async () => {
            // Initialize hello stream reader over hello connection
            var reader = new StreamReader(relayConnection);
            var writer = Console.Out;
            do
            {
                // Read a full line of UTF-8 text up toonewline
                string line = await reader.ReadLineAsync();
                // if hello string is empty or null, we are done.
                if (String.IsNullOrEmpty(line))
                    break;
                // Write toohello console
                await writer.WriteLineAsync(line);
            }
            while (true);
        });
   
        // Read from hello console and write toohello hybrid connection
        var writes = Task.Run(async () => {
            var reader = Console.In;
            var writer = new StreamWriter(relayConnection) { AutoFlush = true };
            do
            {
                // Read a line form hello console
                string line = await reader.ReadLineAsync();
                // Write hello line out, also when it's empty
                await writer.WriteLineAsync(line);
                // Quit when hello line was empty
                if (String.IsNullOrEmpty(line))
                    break;
            }
            while (true);
        });
   
        // Wait for both tasks toocomplete
        await Task.WhenAll(reads, writes);
        await relayConnection.CloseAsync(CancellationToken.None);
    }
    ```
4. <span data-ttu-id="485b3-113">Lägg till följande rad med kod toohello hello `Main` metod i hello `Program` klass.</span><span class="sxs-lookup"><span data-stu-id="485b3-113">Add hello following line of code toohello `Main` method in hello `Program` class.</span></span>
   
    ```csharp
    RunAsync().GetAwaiter().GetResult();
    ```
   
    <span data-ttu-id="485b3-114">Så här bör din Program.cs se ut.</span><span class="sxs-lookup"><span data-stu-id="485b3-114">Here is what your Program.cs should look like.</span></span>
   
    ```csharp
    using System;
    using System.IO;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.Azure.Relay;
   
    namespace Client
    {
        class Program
        {
            private const string RelayNamespace = "{RelayNamespace}.servicebus.windows.net";
            private const string ConnectionName = "{HybridConnectionName}";
            private const string KeyName = "{SASKeyName}";
            private const string Key = "{SASKey}";
   
            static void Main(string[] args)
            {
                RunAsync().GetAwaiter().GetResult();
            }
   
            private static async Task RunAsync()
            {
                Console.WriteLine("Enter lines of text toosend toohello server with ENTER");
   
                // Create a new hybrid connection client
                var tokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider(KeyName, Key);
                var client = new HybridConnectionClient(new Uri(String.Format("sb://{0}/{1}", RelayNamespace, ConnectionName)), tokenProvider);
   
                // Initiate hello connection
                var relayConnection = await client.CreateConnectionAsync();
   
                // We run two conucrrent loops on hello connection. One 
                // reads input from hello console and writes it toohello connection 
                // with a stream writer. hello other reads lines of input from hello 
                // connection with a stream reader and writes them toohello console. 
                // Entering a blank line will shut down hello write task after 
                // sending it toohello server. hello server will then cleanly shut down
                // hello connection which will terminate hello read task.
   
                var reads = Task.Run(async () => {
                    // Initialize hello stream reader over hello connection
                    var reader = new StreamReader(relayConnection);
                    var writer = Console.Out;
                    do
                    {
                        // Read a full line of UTF-8 text up toonewline
                        string line = await reader.ReadLineAsync();
                        // If hello string is empty or null, we are done.
                        if (String.IsNullOrEmpty(line))
                            break;
                        // Write toohello console
                        await writer.WriteLineAsync(line);
                    }
                    while (true);
                });
   
                // Read from hello console and write toohello hybrid connection
                var writes = Task.Run(async () => {
                    var reader = Console.In;
                    var writer = new StreamWriter(relayConnection) { AutoFlush = true };
                    do
                    {
                        // Read a line form hello console
                        string line = await reader.ReadLineAsync();
                        // Write hello line out, also when it's empty
                        await writer.WriteLineAsync(line);
                        // Quit when hello line was empty
                        if (String.IsNullOrEmpty(line))
                            break;
                    }
                    while (true);
                });
   
                // Wait for both tasks toocomplete
                await Task.WhenAll(reads, writes);
                await relayConnection.CloseAsync(CancellationToken.None);
            }
        }
    }
    ```

