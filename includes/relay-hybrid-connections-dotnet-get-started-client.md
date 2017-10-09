### <a name="create-a-console-application"></a>Skapa ett konsolprogram

Börja med att starta Visual Studio och skapa ett nytt projekt: **Konsolprogram (.NET Framework)**.

### <a name="add-hello-relay-nuget-package"></a>Lägg till hello Relay NuGet-paketet

1. Högerklicka på hello nyskapad projektet och klicka sedan på **hantera NuGet-paket**.
2. Klicka på hello **Bläddra** fliken och Sök efter ”Microsoft.Azure.Relay” och välj hello **Microsoft Azure-relä** objekt. Klicka på **installera** toocomplete hello installationen och sedan stänga den här dialogrutan.

### <a name="write-some-code-toosend-messages"></a>Skriva kod toosend meddelanden

1. Ersätta befintliga hello `using` instruktioner hello överst i filen för hello Program.cs med följande hello `using` instruktioner:
   
    ```csharp
    using System;
    using System.IO;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.Azure.Relay;
    ```
2. Lägg till konstanter toohello `Program` klass för hello hybrid anslutningsinformation. Ersätt hello platshållare inom parentes med hello-värden som du fick när du skapar hello hybridanslutning. Vara säker på att toouse hello fullständiga namnområdesnamn:
   
    ```csharp
    private const string RelayNamespace = "{RelayNamespace}.servicebus.windows.net";
    private const string ConnectionName = "{HybridConnectionName}";
    private const string KeyName = "{SASKeyName}";
    private const string Key = "{SASKey}";
    ```
3. Lägg till följande metod toohello hello `Program` klass:
   
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
4. Lägg till följande rad med kod toohello hello `Main` metod i hello `Program` klass.
   
    ```csharp
    RunAsync().GetAwaiter().GetResult();
    ```
   
    Så här bör din Program.cs se ut.
   
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

