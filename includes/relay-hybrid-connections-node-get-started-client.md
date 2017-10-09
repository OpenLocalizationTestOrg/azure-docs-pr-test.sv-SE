### <a name="create-a-nodejs-application"></a>Skapa ett Node.js-program

Skapa en ny JavaScript-fil som kallas `sender.js`.

### <a name="add-hello-relay-npm-package"></a>Lägg till hello Relay NPM-paketet

Kör `npm install hyco-ws` från Kommandotolken för en nod i projektmappen.

### <a name="write-some-code-toosend-messages"></a>Skriva kod toosend meddelanden

1. Lägg till följande hello `constants` toohello överkant hello `sender.js` fil.
   
    ```js
    const WebSocket = require('hyco-ws');
    const readline = require('readline')
        .createInterface({
            input: process.stdin,
            output: process.stdout
        });;
    ```
2. Lägg till följande konstanter toohello hello `sender.js` -filen för hello hybrid anslutningsinformation. Ersätt hello platshållare inom parentes med hello-värden som du fick när du skapade hello hybridanslutning.
   
   1. `const ns`-hello Relay namnområde. Vara säker på att toouse hello fullständiga namnområdesnamnet; till exempel `{namespace}.servicebus.windows.net`.
   2. `const path`-hello namnet på hello hybridanslutning.
   3. `const keyrule`-hello namnet på hello SAS-nyckel.
   4. `const key`-hello SAS-nyckelvärdet.

3. Lägg till följande kod toohello hello `sender.js` fil:
   
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
    Så här bör din sender.js-fil se ut:
   
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

