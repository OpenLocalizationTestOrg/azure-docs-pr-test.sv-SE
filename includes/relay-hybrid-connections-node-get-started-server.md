### <a name="create-a-nodejs-application"></a><span data-ttu-id="74479-101">Skapa ett Node.js-program</span><span class="sxs-lookup"><span data-stu-id="74479-101">Create a Node.js application</span></span>

<span data-ttu-id="74479-102">Skapa en ny JavaScript-fil som kallas `listener.js`.</span><span class="sxs-lookup"><span data-stu-id="74479-102">Create a new JavaScript file called `listener.js`.</span></span>

### <a name="add-the-relay-npm-package"></a><span data-ttu-id="74479-103">Lägg till Relay NPM-paketet</span><span class="sxs-lookup"><span data-stu-id="74479-103">Add the Relay NPM package</span></span>

<span data-ttu-id="74479-104">Kör `npm install hyco-ws` från Kommandotolken för en nod i projektmappen.</span><span class="sxs-lookup"><span data-stu-id="74479-104">Run `npm install hyco-ws` from a Node command prompt in your project folder.</span></span>

### <a name="write-some-code-to-receive-messages"></a><span data-ttu-id="74479-105">Skriva kod för att ta emot meddelanden</span><span class="sxs-lookup"><span data-stu-id="74479-105">Write some code to receive messages</span></span>

1. <span data-ttu-id="74479-106">Lägg till följande konstant överst i filen `listener.js`.</span><span class="sxs-lookup"><span data-stu-id="74479-106">Add the following constant to the top of the `listener.js` file.</span></span>
   
    ```js
    const WebSocket = require('hyco-ws');
    ```
2. <span data-ttu-id="74479-107">Lägg till följande konstanter i filen `listener.js` för hybridanslutningsinformationen.</span><span class="sxs-lookup"><span data-stu-id="74479-107">Add the following constants to the `listener.js` file for the hybrid connection details.</span></span> <span data-ttu-id="74479-108">Ersätt platshållarna inom hakparentes med de värden du erhöll när du skapade hybridanslutningen.</span><span class="sxs-lookup"><span data-stu-id="74479-108">Replace the placeholders in brackets with the values you obtained when you created the hybrid connection.</span></span>
   
   1. <span data-ttu-id="74479-109">`const ns` – Relay-namnområde.</span><span class="sxs-lookup"><span data-stu-id="74479-109">`const ns` - The Relay namespace.</span></span> <span data-ttu-id="74479-110">Se till att du använder det fullständiga namnområdesnamnet, till exempel `{namespace}.servicebus.windows.net`.</span><span class="sxs-lookup"><span data-stu-id="74479-110">Be sure to use the fully qualified namespace name; for example, `{namespace}.servicebus.windows.net`.</span></span>
   2. <span data-ttu-id="74479-111">`const path` – Namnet på hybridanslutningen.</span><span class="sxs-lookup"><span data-stu-id="74479-111">`const path` - The name of the hybrid connection.</span></span>
   3. <span data-ttu-id="74479-112">`const keyrule` – Namnet på SAS-nyckeln.</span><span class="sxs-lookup"><span data-stu-id="74479-112">`const keyrule` - The name of the SAS key.</span></span>
   4. <span data-ttu-id="74479-113">`const key` – SAS-nyckelvärdet.</span><span class="sxs-lookup"><span data-stu-id="74479-113">`const key` - The SAS key value.</span></span>

3. <span data-ttu-id="74479-114">Lägg till följande kod i `listener.js`-filen:</span><span class="sxs-lookup"><span data-stu-id="74479-114">Add the following code to the `listener.js` file:</span></span>
   
    ```js
    var wss = WebSocket.createRelayedServer(
    {
        server : WebSocket.createRelayListenUri(ns, path),
        token: WebSocket.createRelayToken('http://' + ns, keyrule, key)
    }, 
    function (ws) {
        console.log('connection accepted');
        ws.onmessage = function (event) {
            console.log(event.data);
        };
        ws.on('close', function () {
            console.log('connection closed');
        });       
    });
   
    console.log('listening');
   
    wss.on('error', function(err) {
        console.log('error' + err);
    });
    ```
    <span data-ttu-id="74479-115">Så här bör din listener.js-fil se ut:</span><span class="sxs-lookup"><span data-stu-id="74479-115">Here is what your listener.js file should look like:</span></span>
   
    ```js
    const WebSocket = require('hyco-ws');
   
    const ns = "{RelayNamespace}";
    const path = "{HybridConnectionName}";
    const keyrule = "{SASKeyName}";
    const key = "{SASKeyValue}";
   
    var wss = WebSocket.createRelayedServer(
        {
            server : WebSocket.createRelayListenUri(ns, path),
            token: WebSocket.createRelayToken('http://' + ns, keyrule, key)
        }, 
        function (ws) {
            console.log('connection accepted');
            ws.onmessage = function (event) {
                console.log(event.data);
            };
            ws.on('close', function () {
                console.log('connection closed');
            });       
    });
   
    console.log('listening');
   
    wss.on('error', function(err) {
        console.log('error' + err);
    });
    ```

