### <a name="create-a-nodejs-application"></a><span data-ttu-id="03f7a-101">Skapa ett Node.js-program</span><span class="sxs-lookup"><span data-stu-id="03f7a-101">Create a Node.js application</span></span>

<span data-ttu-id="03f7a-102">Skapa en ny JavaScript-fil som kallas `sender.js`.</span><span class="sxs-lookup"><span data-stu-id="03f7a-102">Create a new JavaScript file called `sender.js`.</span></span>

### <a name="add-hello-relay-npm-package"></a><span data-ttu-id="03f7a-103">Lägg till hello Relay NPM-paketet</span><span class="sxs-lookup"><span data-stu-id="03f7a-103">Add hello Relay NPM package</span></span>

<span data-ttu-id="03f7a-104">Kör `npm install hyco-ws` från Kommandotolken för en nod i projektmappen.</span><span class="sxs-lookup"><span data-stu-id="03f7a-104">Run `npm install hyco-ws` from a Node command prompt in your project folder.</span></span>

### <a name="write-some-code-toosend-messages"></a><span data-ttu-id="03f7a-105">Skriva kod toosend meddelanden</span><span class="sxs-lookup"><span data-stu-id="03f7a-105">Write some code toosend messages</span></span>

1. <span data-ttu-id="03f7a-106">Lägg till följande hello `constants` toohello överkant hello `sender.js` fil.</span><span class="sxs-lookup"><span data-stu-id="03f7a-106">Add hello following `constants` toohello top of hello `sender.js` file.</span></span>
   
    ```js
    const WebSocket = require('hyco-ws');
    const readline = require('readline')
        .createInterface({
            input: process.stdin,
            output: process.stdout
        });;
    ```
2. <span data-ttu-id="03f7a-107">Lägg till följande konstanter toohello hello `sender.js` -filen för hello hybrid anslutningsinformation.</span><span class="sxs-lookup"><span data-stu-id="03f7a-107">Add hello following constants toohello `sender.js` file for hello hybrid connection details.</span></span> <span data-ttu-id="03f7a-108">Ersätt hello platshållare inom parentes med hello-värden som du fick när du skapade hello hybridanslutning.</span><span class="sxs-lookup"><span data-stu-id="03f7a-108">Replace hello placeholders in brackets with hello values you obtained when you created hello hybrid connection.</span></span>
   
   1. <span data-ttu-id="03f7a-109">`const ns`-hello Relay namnområde.</span><span class="sxs-lookup"><span data-stu-id="03f7a-109">`const ns` - hello Relay namespace.</span></span> <span data-ttu-id="03f7a-110">Vara säker på att toouse hello fullständiga namnområdesnamnet; till exempel `{namespace}.servicebus.windows.net`.</span><span class="sxs-lookup"><span data-stu-id="03f7a-110">Be sure toouse hello fully qualified namespace name; for example, `{namespace}.servicebus.windows.net`.</span></span>
   2. <span data-ttu-id="03f7a-111">`const path`-hello namnet på hello hybridanslutning.</span><span class="sxs-lookup"><span data-stu-id="03f7a-111">`const path` - hello name of hello hybrid connection.</span></span>
   3. <span data-ttu-id="03f7a-112">`const keyrule`-hello namnet på hello SAS-nyckel.</span><span class="sxs-lookup"><span data-stu-id="03f7a-112">`const keyrule` - hello name of hello SAS key.</span></span>
   4. <span data-ttu-id="03f7a-113">`const key`-hello SAS-nyckelvärdet.</span><span class="sxs-lookup"><span data-stu-id="03f7a-113">`const key` - hello SAS key value.</span></span>

3. <span data-ttu-id="03f7a-114">Lägg till följande kod toohello hello `sender.js` fil:</span><span class="sxs-lookup"><span data-stu-id="03f7a-114">Add hello following code toohello `sender.js` file:</span></span>
   
    ```js
    WebSocket.relayedConnect(
        WebSocket.createRelaySendUri(ns, path),
        WebSocket.createRelayToken('http://'+ns, keyrule, key),
        function (wss) {
            readline.on('line', (input) => {
                wss.send(input, null);
            });
   
            console.log('Started client interval.');
            wss.on('close', function () {
                console.log('stopping client interval');
                process.exit();
            });
        }
    );
    ```
    <span data-ttu-id="03f7a-115">Så här bör din sender.js-fil se ut:</span><span class="sxs-lookup"><span data-stu-id="03f7a-115">Here is what your sender.js file should look like:</span></span>
   
    ```js
    const WebSocket = require('hyco-ws');
    const readline = require('readline')
        .createInterface({
            input: process.stdin,
            output: process.stdout
        });;
   
    const ns = "{RelayNamespace}";
    const path = "{HybridConnectionName}";
    const keyrule = "{SASKeyName}";
    const key = "{SASKeyValue}";
   
    WebSocket.relayedConnect(
        WebSocket.createRelaySendUri(ns, path),
        WebSocket.createRelayToken('http://'+ns, keyrule, key),
        function (wss) {
            readline.on('line', (input) => {
                wss.send(input, null);
            });
   
            console.log('Started client interval.');
            wss.on('close', function () {
                console.log('stopping client interval');
                process.exit();
            });
        }
    );
    ```

