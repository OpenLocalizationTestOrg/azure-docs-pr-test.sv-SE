### <a name="create-a-nodejs-application"></a>Skapa ett Node.js-program

Skapa en ny JavaScript-fil som kallas `listener.js`.

### <a name="add-hello-relay-npm-package"></a>Lägg till hello Relay NPM-paketet

Kör `npm install hyco-ws` från Kommandotolken för en nod i projektmappen.

### <a name="write-some-code-tooreceive-messages"></a>Skriva kod tooreceive meddelanden

1. Lägg till följande konstant toohello överkant hello hello `listener.js` fil.
   
    ```js
    const WebSocket = require('hyco-ws');
    ```
2. Lägg till följande konstanter toohello hello `listener.js` -filen för hello hybrid anslutningsinformation. Ersätt hello platshållare inom parentes med hello-värden som du fick när du skapade hello hybridanslutning.
   
   1. `const ns`-hello Relay namnområde. Vara säker på att toouse hello fullständiga namnområdesnamnet; till exempel `{namespace}.servicebus.windows.net`.
   2. `const path`-hello namnet på hello hybridanslutning.
   3. `const keyrule`-hello namnet på hello SAS-nyckel.
   4. `const key`-hello SAS-nyckelvärdet.

3. Lägg till följande kod toohello hello `listener.js` fil:
   
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
    Så här bör din listener.js-fil se ut:
   
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

