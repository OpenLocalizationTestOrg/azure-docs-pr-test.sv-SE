### <a name="create-a-nodejs-application"></a><span data-ttu-id="7f6f3-101">Skapa ett Node.js-program</span><span class="sxs-lookup"><span data-stu-id="7f6f3-101">Create a Node.js application</span></span>

<span data-ttu-id="7f6f3-102">Skapa en ny JavaScript-fil som kallas `listener.js`.</span><span class="sxs-lookup"><span data-stu-id="7f6f3-102">Create a new JavaScript file called `listener.js`.</span></span>

### <a name="add-hello-relay-npm-package"></a><span data-ttu-id="7f6f3-103">Lägg till hello Relay NPM-paketet</span><span class="sxs-lookup"><span data-stu-id="7f6f3-103">Add hello Relay NPM package</span></span>

<span data-ttu-id="7f6f3-104">Kör `npm install hyco-ws` från Kommandotolken för en nod i projektmappen.</span><span class="sxs-lookup"><span data-stu-id="7f6f3-104">Run `npm install hyco-ws` from a Node command prompt in your project folder.</span></span>

### <a name="write-some-code-tooreceive-messages"></a><span data-ttu-id="7f6f3-105">Skriva kod tooreceive meddelanden</span><span class="sxs-lookup"><span data-stu-id="7f6f3-105">Write some code tooreceive messages</span></span>

1. <span data-ttu-id="7f6f3-106">Lägg till följande konstant toohello överkant hello hello `listener.js` fil.</span><span class="sxs-lookup"><span data-stu-id="7f6f3-106">Add hello following constant toohello top of hello `listener.js` file.</span></span>
   
    ```js
    const WebSocket = require('hyco-ws');
    ```
2. <span data-ttu-id="7f6f3-107">Lägg till följande konstanter toohello hello `listener.js` -filen för hello hybrid anslutningsinformation.</span><span class="sxs-lookup"><span data-stu-id="7f6f3-107">Add hello following constants toohello `listener.js` file for hello hybrid connection details.</span></span> <span data-ttu-id="7f6f3-108">Ersätt hello platshållare inom parentes med hello-värden som du fick när du skapade hello hybridanslutning.</span><span class="sxs-lookup"><span data-stu-id="7f6f3-108">Replace hello placeholders in brackets with hello values you obtained when you created hello hybrid connection.</span></span>
   
   1. <span data-ttu-id="7f6f3-109">`const ns`-hello Relay namnområde.</span><span class="sxs-lookup"><span data-stu-id="7f6f3-109">`const ns` - hello Relay namespace.</span></span> <span data-ttu-id="7f6f3-110">Vara säker på att toouse hello fullständiga namnområdesnamnet; till exempel `{namespace}.servicebus.windows.net`.</span><span class="sxs-lookup"><span data-stu-id="7f6f3-110">Be sure toouse hello fully qualified namespace name; for example, `{namespace}.servicebus.windows.net`.</span></span>
   2. <span data-ttu-id="7f6f3-111">`const path`-hello namnet på hello hybridanslutning.</span><span class="sxs-lookup"><span data-stu-id="7f6f3-111">`const path` - hello name of hello hybrid connection.</span></span>
   3. <span data-ttu-id="7f6f3-112">`const keyrule`-hello namnet på hello SAS-nyckel.</span><span class="sxs-lookup"><span data-stu-id="7f6f3-112">`const keyrule` - hello name of hello SAS key.</span></span>
   4. <span data-ttu-id="7f6f3-113">`const key`-hello SAS-nyckelvärdet.</span><span class="sxs-lookup"><span data-stu-id="7f6f3-113">`const key` - hello SAS key value.</span></span>

3. <span data-ttu-id="7f6f3-114">Lägg till följande kod toohello hello `listener.js` fil:</span><span class="sxs-lookup"><span data-stu-id="7f6f3-114">Add hello following code toohello `listener.js` file:</span></span>
   
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
    <span data-ttu-id="7f6f3-115">Så här bör din listener.js-fil se ut:</span><span class="sxs-lookup"><span data-stu-id="7f6f3-115">Here is what your listener.js file should look like:</span></span>
   
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

