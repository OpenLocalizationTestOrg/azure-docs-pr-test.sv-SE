### <a name="create-a-console-application"></a><span data-ttu-id="3b345-101">Skapa ett konsolprogram</span><span class="sxs-lookup"><span data-stu-id="3b345-101">Create a console application</span></span>

<span data-ttu-id="3b345-102">Börja med att starta Visual Studio och skapa ett nytt projekt: **Konsolprogram (.NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="3b345-102">First, launch Visual Studio and create a new **Console App (.NET Framework)** project.</span></span>

### <a name="add-the-relay-nuget-package"></a><span data-ttu-id="3b345-103">Lägg till Relay NuGet-paketet</span><span class="sxs-lookup"><span data-stu-id="3b345-103">Add the Relay NuGet package</span></span>

1. <span data-ttu-id="3b345-104">Högerklicka på det nyskapade projektet och klicka sedan på **Hantera NuGet-paket**.</span><span class="sxs-lookup"><span data-stu-id="3b345-104">Right-click the newly created project and then click **Manage NuGet Packages**.</span></span>
2. <span data-ttu-id="3b345-105">Klicka på fliken **Bläddra** och sök sedan efter ”Microsoft.Azure.Relay” och markera posten **Microsoft Azure Relay**.</span><span class="sxs-lookup"><span data-stu-id="3b345-105">Click the **Browse** tab, then search for "Microsoft.Azure.Relay" and select the **Microsoft Azure Relay** item.</span></span> <span data-ttu-id="3b345-106">Klicka på **Installera** för att slutföra installationen och stäng sedan den här dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3b345-106">Click **Install** to complete the installation, then close this dialog box.</span></span>

### <a name="write-some-code-to-send-messages"></a><span data-ttu-id="3b345-107">Skriva kod för att skicka meddelanden</span><span class="sxs-lookup"><span data-stu-id="3b345-107">Write some code to send messages</span></span>

1. <span data-ttu-id="3b345-108">Ersätt de befintliga `using`-satserna överst i filen Program.cs med följande `using`-satser:</span><span class="sxs-lookup"><span data-stu-id="3b345-108">Replace the existing `using` statements at the top of the Program.cs file with the following `using` statements:</span></span>
   
    ```csharp
    using System;
    using System.IO;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.Azure.Relay;
    ```
2. <span data-ttu-id="3b345-109">Lägg till konstanter i klassen `Program` för hybridanslutningsinformationen.</span><span class="sxs-lookup"><span data-stu-id="3b345-109">Add constants to the `Program` class for the hybrid connection details.</span></span> <span data-ttu-id="3b345-110">Ersätt platshållarna inom hakparentes med de värden du erhöll när du skapade hybridanslutningen.</span><span class="sxs-lookup"><span data-stu-id="3b345-110">Replace the placeholders in brackets with the values you obtained when creating the hybrid connection.</span></span> <span data-ttu-id="3b345-111">Se till att du använder det fullständiga namnområdesnamnet:</span><span class="sxs-lookup"><span data-stu-id="3b345-111">Be sure to use the fully qualified namespace name:</span></span>
   
    ```csharp
    private const string RelayNamespace = "{RelayNamespace}.servicebus.windows.net";
    private const string ConnectionName = "{HybridConnectionName}";
    private const string KeyName = "{SASKeyName}";
    private const string Key = "{SASKey}";
    ```
3. <span data-ttu-id="3b345-112">Lägg till följande metod i klassen `Program`:</span><span class="sxs-lookup"><span data-stu-id="3b345-112">Add the following method to the `Program` class:</span></span>
   
    ```csharp
    private static async Task RunAsync()
    {
        Console.WriteLine("Enter lines of text to send to the server with ENTER");
   
        // Create a new hybrid connection client
        var tokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider(KeyName, Key);
        var client = new HybridConnectionClient(new Uri(String.Format("sb://{0}/{1}", RelayNamespace, ConnectionName)), tokenProvider);
   
        // Initiate the connection
        var relayConnection = await client.CreateConnectionAsync();
   
        // We run two concurrent loops on the connection. One 
        // reads input from the console and writes it to the connection 
        // with a stream writer. The other reads lines of input from the 
        // connection with a stream reader and writes them to the console. 
        // Entering a blank line will shut down the write task after 
        // sending it to the server. The server will then cleanly shut down
        // the connection which will terminate the read task.
   
        var reads = Task.Run(async () => {
            // Initialize the stream reader over the connection
            var reader = new StreamReader(relayConnection);
            var writer = Console.Out;
            do
            {
                // Read a full line of UTF-8 text up to newline
                string line = await reader.ReadLineAsync();
                // if the string is empty or null, we are done.
                if (String.IsNullOrEmpty(line))
                    break;
                // Write to the console
                await writer.WriteLineAsync(line);
            }
            while (true);
        });
   
        // Read from the console and write to the hybrid connection
        var writes = Task.Run(async () => {
            var reader = Console.In;
            var writer = new StreamWriter(relayConnection) { AutoFlush = true };
            do
            {
                // Read a line form the console
                string line = await reader.ReadLineAsync();
                // Write the line out, also when it's empty
                await writer.WriteLineAsync(line);
                // Quit when the line was empty
                if (String.IsNullOrEmpty(line))
                    break;
            }
            while (true);
        });
   
        // Wait for both tasks to complete
        await Task.WhenAll(reads, writes);
        await relayConnection.CloseAsync(CancellationToken.None);
    }
    ```
4. <span data-ttu-id="3b345-113">Lägg till följande kodrad till `Main`-metoden i `Program`-klassen.</span><span class="sxs-lookup"><span data-stu-id="3b345-113">Add the following line of code to the `Main` method in the `Program` class.</span></span>
   
    ```csharp
    RunAsync().GetAwaiter().GetResult();
    ```
   
    <span data-ttu-id="3b345-114">Så här bör din Program.cs se ut.</span><span class="sxs-lookup"><span data-stu-id="3b345-114">Here is what your Program.cs should look like.</span></span>
   
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
                Console.WriteLine("Enter lines of text to send to the server with ENTER");
   
                // Create a new hybrid connection client
                var tokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider(KeyName, Key);
                var client = new HybridConnectionClient(new Uri(String.Format("sb://{0}/{1}", RelayNamespace, ConnectionName)), tokenProvider);
   
                // Initiate the connection
                var relayConnection = await client.CreateConnectionAsync();
   
                // We run two conucrrent loops on the connection. One 
                // reads input from the console and writes it to the connection 
                // with a stream writer. The other reads lines of input from the 
                // connection with a stream reader and writes them to the console. 
                // Entering a blank line will shut down the write task after 
                // sending it to the server. The server will then cleanly shut down
                // the connection which will terminate the read task.
   
                var reads = Task.Run(async () => {
                    // Initialize the stream reader over the connection
                    var reader = new StreamReader(relayConnection);
                    var writer = Console.Out;
                    do
                    {
                        // Read a full line of UTF-8 text up to newline
                        string line = await reader.ReadLineAsync();
                        // If the string is empty or null, we are done.
                        if (String.IsNullOrEmpty(line))
                            break;
                        // Write to the console
                        await writer.WriteLineAsync(line);
                    }
                    while (true);
                });
   
                // Read from the console and write to the hybrid connection
                var writes = Task.Run(async () => {
                    var reader = Console.In;
                    var writer = new StreamWriter(relayConnection) { AutoFlush = true };
                    do
                    {
                        // Read a line form the console
                        string line = await reader.ReadLineAsync();
                        // Write the line out, also when it's empty
                        await writer.WriteLineAsync(line);
                        // Quit when the line was empty
                        if (String.IsNullOrEmpty(line))
                            break;
                    }
                    while (true);
                });
   
                // Wait for both tasks to complete
                await Task.WhenAll(reads, writes);
                await relayConnection.CloseAsync(CancellationToken.None);
            }
        }
    }
    ```

