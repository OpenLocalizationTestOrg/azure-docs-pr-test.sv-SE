## <a name="typical-output"></a>Vanliga utdata

hello visar följande exempel hello utdata skrivs toohello loggfilen av hello Hello World-exempel. hello utdata formateras för läsbarhet:

```json
[{
    "time": "Mon Apr 11 13:48:07 2016",
    "content": "Log started"
}, {
    "time": "Mon Apr 11 13:48:48 2016",
    "properties": {
        "helloWorld": "from Azure IoT Gateway SDK simple sample!"
    },
    "content": "aGVsbG8gd29ybGQ="
}, {
    "time": "Mon Apr 11 13:48:55 2016",
    "properties": {
        "helloWorld": "from Azure IoT Gateway SDK simple sample!"
    },
    "content": "aGVsbG8gd29ybGQ="
}, {
    "time": "Mon Apr 11 13:49:01 2016",
    "properties": {
        "helloWorld": "from Azure IoT Gateway SDK simple sample!"
    },
    "content": "aGVsbG8gd29ybGQ="
}, {
    "time": "Mon Apr 11 13:49:04 2016",
    "content": "Log stopped"
}]
```

## <a name="code-snippets"></a>Kodfragment

Det här avsnittet beskrivs vissa delar av hello kod i hello hello\_world-exempel.

### <a name="iot-edge-gateway-creation"></a>Skapa en IoT-Edge gateway

Du måste implementera en *gateway-processen*. Det här programmet skapar hello intern infrastruktur (hello broker), laddar hello IoT kant moduler, och konfigurerar hello gateway-processen. IoT-Edge tillhandahåller hello **Gateway\_skapa\_från\_JSON** fungerar tooenable toobootstrap en gateway från en JSON-fil. toouse hello **Gateway\_skapa\_från\_JSON** fungera, skickar hello sökvägen tooa JSON-fil som anger hello IoT kant moduler tooload.

Du kan hitta hello koden för hello gateway processen i hello *Hello World* exempel i hello [main.c] [ lnk-main-c] fil. För läsbarhet visas hello följande fragment en förkortad version av hello gateway processkod. Det här exemplet programmet skapar en gateway och väntar sedan hello användaren toopress hello **RETUR** nyckeln innan den river ned hello gateway.

```c
int main(int argc, char** argv)
{
    GATEWAY_HANDLE gateway;
    if ((gateway = Gateway_Create_From_JSON(argv[1])) == NULL)
    {
        printf("failed toocreate hello gateway from JSON\n");
    }
    else
    {
        printf("gateway successfully created from JSON\n");
        printf("gateway shall run until ENTER is pressed\n");
        (void)getchar();
        Gateway_LL_Destroy(gateway);
    }
    return 0;
}
```

hello JSON-inställningsfilen innehåller en lista över IoT kant moduler tooload och hello länkar mellan hello moduler. Varje gräns för IoT-modul måste ange en:

* **namnet**: ett unikt namn för hello-modulen.
* **inläsaren**: en inläsaren som vet hur tooload hello önskad modul. Inläsare är en tilläggspunkt för inläsning av olika typer av moduler. IoT-Edge ger inläsare för användning med moduler som skrivits i interna C, Node.js, Java och .NET. hello Hello World-exempel endast använder hello interna C inläsaren eftersom alla hello moduler i det här exemplet är dynamisk bibliotek som skrivits i C. Mer information om hur toouse IoT kant moduler som skrivits i olika språk, se hello [Node.js](https://github.com/Azure/iot-edge/blob/master/samples/nodejs_simple_sample/), [Java](https://github.com/Azure/iot-edge/tree/master/samples/java_sample), eller [.NET](https://github.com/Azure/iot-edge/tree/master/samples/dotnet_binding_sample) prover.
    * **namnet**: hello namnet hello inläsaren används tooload hello modulen.
    * **EntryPoint**: hello sökvägen toohello bibliotek som innehåller hello-modulen. För Linux är det här biblioteket en SO-fil, i Windows är det en DLL-fil. hello startpunkt är specifik toohello inläsaren används. hello startpunkten för Node.js inläsaren är en JS-fil. hello startpunkten för Java-inläsaren är en klassökväg och ett klassnamn. hello startpunkten för .NET-inläsaren är ett namn på sammansättning och ett klassnamn.

* **argument**: configuration information hello modul måste.

följande kod visar hello JSON används toodeclare alla hello hello IoT kant moduler för hello Hello World-exempel på Linux. Om en modul kräver argument beror på hello utformning av hello-modulen. I det här exemplet hello loggaren modul har ett argument som är hello sökvägen toohello utdatafilen och hello hello\_world modulen har inga argument.

```json
"modules" :
[
    {
        "name" : "logger",
        "loader": {
          "name": "native",
          "entrypoint": {
            "module.path": "./modules/logger/liblogger.so"
        }
        },
        "args" : {"filename":"log.txt"}
    },
    {
        "name" : "hello_world",
        "loader": {
          "name": "native",
          "entrypoint": {
            "module.path": "./modules/hello_world/libhello_world.so"
        }
        },
        "args" : null
    }
]
```

hello JSON-fil innehåller också hello länkar mellan hello-moduler som har skickats toohello broker. En länk har två egenskaper:

* **källan**: ett Modulnamn från hello `modules` avsnittet eller `\*`.
* **sink**: ett Modulnamn från hello `modules` avsnitt.

Varje länk definierar en meddelandeväg och -riktning. Meddelanden från hello **källa** modulen levereras toohello **sink** modul. Du kan ange hello **källa** modul för`\*`, vilket anger att hello **sink** modulen tar emot meddelanden från en modul.

hello följande kod visar hello JSON används tooconfigure länkar mellan hello-moduler som används i hello hello\_world-exempel på Linux. Varje meddelande som genereras av hello `hello_world` modulen förbrukas av hello `logger` modul.

```json
"links":
[
    {
        "source": "hello_world",
        "sink": "logger"
    }
]
```

### <a name="helloworld-module-message-publishing"></a>Meddelandepublicering från Hello\_World-modulen

Du kan hitta hello koden som hello hello\_world modulen toopublish meddelanden i hello ['hello_world.c'] [ lnk-helloworld-c] fil. hello visas följande fragment en ändrad version av hello kod med kommentarer som lagts till och vissa felhantering kod tas bort för läsbarhet:

```c
int helloWorldThread(void *param)
{
    // create data structures used in function.
    HELLOWORLD_HANDLE_DATA* handleData = param;
    MESSAGE_CONFIG msgConfig;
    MAP_HANDLE propertiesMap = Map_Create(NULL);

    // add a property named "helloWorld" with a value of "from Azure IoT
    // Gateway SDK simple sample!" tooa set of message properties that
    // will be appended toohello message before publishing it. 
    Map_AddOrUpdate(propertiesMap, "helloWorld", "from Azure IoT Gateway SDK simple sample!")

    // set hello content for hello message
    msgConfig.size = strlen(HELLOWORLD_MESSAGE);
    msgConfig.source = HELLOWORLD_MESSAGE;

    // set hello properties for hello message
    msgConfig.sourceProperties = propertiesMap;

    // create a message based on hello msgConfig structure
    MESSAGE_HANDLE helloWorldMessage = Message_Create(&msgConfig);

    while (1)
    {
        if (handleData->stopThread)
        {
            (void)Unlock(handleData->lockHandle);
            break; /*gets out of hello thread*/
        }
        else
        {
            // publish hello message toohello broker
            (void)Broker_Publish(handleData->brokerHandle, helloWorldMessage);
            (void)Unlock(handleData->lockHandle);
        }

        (void)ThreadAPI_Sleep(5000); /*every 5 seconds*/
    }

    Message_Destroy(helloWorldMessage);

    return 0;
}
```

### <a name="helloworld-module-message-processing"></a>Meddelandebearbetning från Hello\_World-modulen

hello hello\_world modulen bearbetar aldrig meddelanden att andra IoT kant moduler publicera toohello broker. Därför hello implementering av hello-meddelande återanrop i hello hello\_world modul är en alternativ funktion.

```c
static void HelloWorld_Receive(MODULE_HANDLE moduleHandle, MESSAGE_HANDLE messageHandle)
{
    /* No action, HelloWorld is not interested in any messages. */
}
```

### <a name="logger-module-message-publishing-and-processing"></a>Loggningsmodulens meddelandepublicering och bearbetning

hello loggaren modulen tar emot meddelanden från hello broker och skriver dem tooa fil. Den publicerar aldrig meddelanden. Därför hello koden för hello loggaren modul anropar hello aldrig **Broker_Publish** funktion.

Hej **Logger_Receive** funktion i hello [logger.c] [ lnk-logger-c] filen är hello återanrop hello broker anropar toodeliver meddelanden toohello loggaren modulen. hello visas följande fragment en ändrad version med kommentarer som lagts till och vissa felhantering kod tas bort för läsbarhet:

```c
static void Logger_Receive(MODULE_HANDLE moduleHandle, MESSAGE_HANDLE messageHandle)
{

    time_t temp = time(NULL);
    struct tm* t = localtime(&temp);
    char timetemp[80] = { 0 };

    // Get hello message properties from hello message
    CONSTMAP_HANDLE originalProperties = Message_GetProperties(messageHandle); 
    MAP_HANDLE propertiesAsMap = ConstMap_CloneWriteable(originalProperties);

    // Convert hello collection of properties into a JSON string
    STRING_HANDLE jsonProperties = Map_ToJSON(propertiesAsMap);

    //  base64 encode hello message content
    const CONSTBUFFER * content = Message_GetContent(messageHandle);
    STRING_HANDLE contentAsJSON = Base64_Encode_Bytes(content->buffer, content->size);

    // Start hello construction of hello final string toobe logged by adding
    // hello timestamp
    STRING_HANDLE jsonToBeAppended = STRING_construct(",{\"time\":\"");
    STRING_concat(jsonToBeAppended, timetemp);

    // Add hello message properties
    STRING_concat(jsonToBeAppended, "\",\"properties\":"); 
    STRING_concat_with_STRING(jsonToBeAppended, jsonProperties);

    // Add hello content
    STRING_concat(jsonToBeAppended, ",\"content\":\"");
    STRING_concat_with_STRING(jsonToBeAppended, contentAsJSON);
    STRING_concat(jsonToBeAppended, "\"}]");

    // Write hello formatted string
    LOGGER_HANDLE_DATA *handleData = (LOGGER_HANDLE_DATA *)moduleHandle;
    addJSONString(handleData->fout, STRING_c_str(jsonToBeAppended);
}
```

## <a name="next-steps"></a>Nästa steg

I den här artikeln får körde du en enkel IoT gräns-gatewayen som skriver meddelanden tooa loggfilen. toorun ett exempel som skickar meddelanden tooIoT hubben, se [IoT kant – skicka meddelanden från enhet till moln med en simulerad enhet med hjälp av Linux] [ lnk-gateway-simulated-linux] eller [IoT kant – skicka meddelanden från enhet till moln med en simulerade enhet med hjälp av Windows][lnk-gateway-simulated-windows].


<!-- Links -->
[lnk-main-c]: https://github.com/Azure/iot-edge/blob/master/samples/hello_world/src/main.c
[lnk-helloworld-c]: https://github.com/Azure/iot-edge/blob/master/modules/hello_world/src/hello_world.c
[lnk-logger-c]: https://github.com/Azure/iot-edge/blob/master/modules/logger/src/logger.c
[lnk-iot-edge]: https://github.com/Azure/iot-edge/
[lnk-gateway-simulated-linux]: ../articles/iot-hub/iot-hub-linux-iot-edge-simulated-device.md
[lnk-gateway-simulated-windows]: ../articles/iot-hub/iot-hub-windows-iot-edge-simulated-device.md