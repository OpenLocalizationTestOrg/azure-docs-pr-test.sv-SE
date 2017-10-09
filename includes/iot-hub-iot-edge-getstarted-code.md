## <a name="typical-output"></a><span data-ttu-id="91b52-101">Vanliga utdata</span><span class="sxs-lookup"><span data-stu-id="91b52-101">Typical output</span></span>

<span data-ttu-id="91b52-102">hello visar följande exempel hello utdata skrivs toohello loggfilen av hello Hello World-exempel.</span><span class="sxs-lookup"><span data-stu-id="91b52-102">hello following example shows hello output written toohello log file by hello Hello World sample.</span></span> <span data-ttu-id="91b52-103">hello utdata formateras för läsbarhet:</span><span class="sxs-lookup"><span data-stu-id="91b52-103">hello output is formatted for legibility:</span></span>

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

## <a name="code-snippets"></a><span data-ttu-id="91b52-104">Kodfragment</span><span class="sxs-lookup"><span data-stu-id="91b52-104">Code snippets</span></span>

<span data-ttu-id="91b52-105">Det här avsnittet beskrivs vissa delar av hello kod i hello hello\_world-exempel.</span><span class="sxs-lookup"><span data-stu-id="91b52-105">This section discusses some key sections of hello code in hello hello\_world sample.</span></span>

### <a name="iot-edge-gateway-creation"></a><span data-ttu-id="91b52-106">Skapa en IoT-Edge gateway</span><span class="sxs-lookup"><span data-stu-id="91b52-106">IoT Edge gateway creation</span></span>

<span data-ttu-id="91b52-107">Du måste implementera en *gateway-processen*.</span><span class="sxs-lookup"><span data-stu-id="91b52-107">You must implement a *gateway process*.</span></span> <span data-ttu-id="91b52-108">Det här programmet skapar hello intern infrastruktur (hello broker), laddar hello IoT kant moduler, och konfigurerar hello gateway-processen.</span><span class="sxs-lookup"><span data-stu-id="91b52-108">This program creates hello internal infrastructure (hello broker), loads hello IoT Edge modules, and configures hello gateway process.</span></span> <span data-ttu-id="91b52-109">IoT-Edge tillhandahåller hello **Gateway\_skapa\_från\_JSON** fungerar tooenable toobootstrap en gateway från en JSON-fil.</span><span class="sxs-lookup"><span data-stu-id="91b52-109">IoT Edge provides hello **Gateway\_Create\_From\_JSON** function tooenable you toobootstrap a gateway from a JSON file.</span></span> <span data-ttu-id="91b52-110">toouse hello **Gateway\_skapa\_från\_JSON** fungera, skickar hello sökvägen tooa JSON-fil som anger hello IoT kant moduler tooload.</span><span class="sxs-lookup"><span data-stu-id="91b52-110">toouse hello **Gateway\_Create\_From\_JSON** function, pass it hello path tooa JSON file that specifies hello IoT Edge modules tooload.</span></span>

<span data-ttu-id="91b52-111">Du kan hitta hello koden för hello gateway processen i hello *Hello World* exempel i hello [main.c] [ lnk-main-c] fil.</span><span class="sxs-lookup"><span data-stu-id="91b52-111">You can find hello code for hello gateway process in hello *Hello World* sample in hello [main.c][lnk-main-c] file.</span></span> <span data-ttu-id="91b52-112">För läsbarhet visas hello följande fragment en förkortad version av hello gateway processkod.</span><span class="sxs-lookup"><span data-stu-id="91b52-112">For legibility, hello following snippet shows an abbreviated version of hello gateway process code.</span></span> <span data-ttu-id="91b52-113">Det här exemplet programmet skapar en gateway och väntar sedan hello användaren toopress hello **RETUR** nyckeln innan den river ned hello gateway.</span><span class="sxs-lookup"><span data-stu-id="91b52-113">This example program creates a gateway and then waits for hello user toopress hello **ENTER** key before it tears down hello gateway.</span></span>

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

<span data-ttu-id="91b52-114">hello JSON-inställningsfilen innehåller en lista över IoT kant moduler tooload och hello länkar mellan hello moduler.</span><span class="sxs-lookup"><span data-stu-id="91b52-114">hello JSON settings file contains a list of IoT Edge modules tooload and hello links between hello modules.</span></span> <span data-ttu-id="91b52-115">Varje gräns för IoT-modul måste ange en:</span><span class="sxs-lookup"><span data-stu-id="91b52-115">Each IoT Edge module must specify a:</span></span>

* <span data-ttu-id="91b52-116">**namnet**: ett unikt namn för hello-modulen.</span><span class="sxs-lookup"><span data-stu-id="91b52-116">**name**: a unique name for hello module.</span></span>
* <span data-ttu-id="91b52-117">**inläsaren**: en inläsaren som vet hur tooload hello önskad modul.</span><span class="sxs-lookup"><span data-stu-id="91b52-117">**loader**: a loader that knows how tooload hello desired module.</span></span> <span data-ttu-id="91b52-118">Inläsare är en tilläggspunkt för inläsning av olika typer av moduler.</span><span class="sxs-lookup"><span data-stu-id="91b52-118">Loaders are an extension point for loading different types of modules.</span></span> <span data-ttu-id="91b52-119">IoT-Edge ger inläsare för användning med moduler som skrivits i interna C, Node.js, Java och .NET.</span><span class="sxs-lookup"><span data-stu-id="91b52-119">IoT Edge provides loaders for use with modules written in native C, Node.js, Java, and .NET.</span></span> <span data-ttu-id="91b52-120">hello Hello World-exempel endast använder hello interna C inläsaren eftersom alla hello moduler i det här exemplet är dynamisk bibliotek som skrivits i C. Mer information om hur toouse IoT kant moduler som skrivits i olika språk, se hello [Node.js](https://github.com/Azure/iot-edge/blob/master/samples/nodejs_simple_sample/), [Java](https://github.com/Azure/iot-edge/tree/master/samples/java_sample), eller [.NET](https://github.com/Azure/iot-edge/tree/master/samples/dotnet_binding_sample) prover.</span><span class="sxs-lookup"><span data-stu-id="91b52-120">hello Hello World sample only uses hello native C loader because all hello modules in this sample are dynamic libraries written in C. For more information about how toouse IoT Edge modules written in different languages, see hello [Node.js](https://github.com/Azure/iot-edge/blob/master/samples/nodejs_simple_sample/), [Java](https://github.com/Azure/iot-edge/tree/master/samples/java_sample), or [.NET](https://github.com/Azure/iot-edge/tree/master/samples/dotnet_binding_sample) samples.</span></span>
    * <span data-ttu-id="91b52-121">**namnet**: hello namnet hello inläsaren används tooload hello modulen.</span><span class="sxs-lookup"><span data-stu-id="91b52-121">**name**: hello name of hello loader used tooload hello module.</span></span>
    * <span data-ttu-id="91b52-122">**EntryPoint**: hello sökvägen toohello bibliotek som innehåller hello-modulen.</span><span class="sxs-lookup"><span data-stu-id="91b52-122">**entrypoint**: hello path toohello library containing hello module.</span></span> <span data-ttu-id="91b52-123">För Linux är det här biblioteket en SO-fil, i Windows är det en DLL-fil.</span><span class="sxs-lookup"><span data-stu-id="91b52-123">On Linux this library is a .so file, on Windows this library is a .dll file.</span></span> <span data-ttu-id="91b52-124">hello startpunkt är specifik toohello inläsaren används.</span><span class="sxs-lookup"><span data-stu-id="91b52-124">hello entry point is specific toohello type of loader being used.</span></span> <span data-ttu-id="91b52-125">hello startpunkten för Node.js inläsaren är en JS-fil.</span><span class="sxs-lookup"><span data-stu-id="91b52-125">hello Node.js loader entry point is a .js file.</span></span> <span data-ttu-id="91b52-126">hello startpunkten för Java-inläsaren är en klassökväg och ett klassnamn.</span><span class="sxs-lookup"><span data-stu-id="91b52-126">hello Java loader entry point is a classpath and a class name.</span></span> <span data-ttu-id="91b52-127">hello startpunkten för .NET-inläsaren är ett namn på sammansättning och ett klassnamn.</span><span class="sxs-lookup"><span data-stu-id="91b52-127">hello .NET loader entry point is an assembly name and a class name.</span></span>

* <span data-ttu-id="91b52-128">**argument**: configuration information hello modul måste.</span><span class="sxs-lookup"><span data-stu-id="91b52-128">**args**: any configuration information hello module needs.</span></span>

<span data-ttu-id="91b52-129">följande kod visar hello JSON används toodeclare alla hello hello IoT kant moduler för hello Hello World-exempel på Linux.</span><span class="sxs-lookup"><span data-stu-id="91b52-129">hello following code shows hello JSON used toodeclare all hello IoT Edge modules for hello Hello World sample on Linux.</span></span> <span data-ttu-id="91b52-130">Om en modul kräver argument beror på hello utformning av hello-modulen.</span><span class="sxs-lookup"><span data-stu-id="91b52-130">Whether a module requires any arguments depends on hello design of hello module.</span></span> <span data-ttu-id="91b52-131">I det här exemplet hello loggaren modul har ett argument som är hello sökvägen toohello utdatafilen och hello hello\_world modulen har inga argument.</span><span class="sxs-lookup"><span data-stu-id="91b52-131">In this example, hello logger module takes an argument that is hello path toohello output file and hello hello\_world module has no arguments.</span></span>

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

<span data-ttu-id="91b52-132">hello JSON-fil innehåller också hello länkar mellan hello-moduler som har skickats toohello broker.</span><span class="sxs-lookup"><span data-stu-id="91b52-132">hello JSON file also contains hello links between hello modules that are passed toohello broker.</span></span> <span data-ttu-id="91b52-133">En länk har två egenskaper:</span><span class="sxs-lookup"><span data-stu-id="91b52-133">A link has two properties:</span></span>

* <span data-ttu-id="91b52-134">**källan**: ett Modulnamn från hello `modules` avsnittet eller `\*`.</span><span class="sxs-lookup"><span data-stu-id="91b52-134">**source**: a module name from hello `modules` section, or `\*`.</span></span>
* <span data-ttu-id="91b52-135">**sink**: ett Modulnamn från hello `modules` avsnitt.</span><span class="sxs-lookup"><span data-stu-id="91b52-135">**sink**: a module name from hello `modules` section.</span></span>

<span data-ttu-id="91b52-136">Varje länk definierar en meddelandeväg och -riktning.</span><span class="sxs-lookup"><span data-stu-id="91b52-136">Each link defines a message route and direction.</span></span> <span data-ttu-id="91b52-137">Meddelanden från hello **källa** modulen levereras toohello **sink** modul.</span><span class="sxs-lookup"><span data-stu-id="91b52-137">Messages from hello **source** module are delivered toohello **sink** module.</span></span> <span data-ttu-id="91b52-138">Du kan ange hello **källa** modul för`\*`, vilket anger att hello **sink** modulen tar emot meddelanden från en modul.</span><span class="sxs-lookup"><span data-stu-id="91b52-138">You can set hello **source** module too`\*`, which indicates that hello **sink** module receives messages from any module.</span></span>

<span data-ttu-id="91b52-139">hello följande kod visar hello JSON används tooconfigure länkar mellan hello-moduler som används i hello hello\_world-exempel på Linux.</span><span class="sxs-lookup"><span data-stu-id="91b52-139">hello following code shows hello JSON used tooconfigure links between hello modules used in hello hello\_world sample on Linux.</span></span> <span data-ttu-id="91b52-140">Varje meddelande som genereras av hello `hello_world` modulen förbrukas av hello `logger` modul.</span><span class="sxs-lookup"><span data-stu-id="91b52-140">Every message produced by hello `hello_world` module is consumed by hello `logger` module.</span></span>

```json
"links":
[
    {
        "source": "hello_world",
        "sink": "logger"
    }
]
```

### <a name="helloworld-module-message-publishing"></a><span data-ttu-id="91b52-141">Meddelandepublicering från Hello\_World-modulen</span><span class="sxs-lookup"><span data-stu-id="91b52-141">Hello\_world module message publishing</span></span>

<span data-ttu-id="91b52-142">Du kan hitta hello koden som hello hello\_world modulen toopublish meddelanden i hello ['hello_world.c'] [ lnk-helloworld-c] fil.</span><span class="sxs-lookup"><span data-stu-id="91b52-142">You can find hello code used by hello hello\_world module toopublish messages in hello ['hello_world.c'][lnk-helloworld-c] file.</span></span> <span data-ttu-id="91b52-143">hello visas följande fragment en ändrad version av hello kod med kommentarer som lagts till och vissa felhantering kod tas bort för läsbarhet:</span><span class="sxs-lookup"><span data-stu-id="91b52-143">hello following snippet shows an amended version of hello code with comments added and some error handling code removed for legibility:</span></span>

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

### <a name="helloworld-module-message-processing"></a><span data-ttu-id="91b52-144">Meddelandebearbetning från Hello\_World-modulen</span><span class="sxs-lookup"><span data-stu-id="91b52-144">Hello\_world module message processing</span></span>

<span data-ttu-id="91b52-145">hello hello\_world modulen bearbetar aldrig meddelanden att andra IoT kant moduler publicera toohello broker.</span><span class="sxs-lookup"><span data-stu-id="91b52-145">hello hello\_world module never processes messages that other IoT Edge modules publish toohello broker.</span></span> <span data-ttu-id="91b52-146">Därför hello implementering av hello-meddelande återanrop i hello hello\_world modul är en alternativ funktion.</span><span class="sxs-lookup"><span data-stu-id="91b52-146">Therefore, hello implementation of hello message callback in hello hello\_world module is a no-op function.</span></span>

```c
static void HelloWorld_Receive(MODULE_HANDLE moduleHandle, MESSAGE_HANDLE messageHandle)
{
    /* No action, HelloWorld is not interested in any messages. */
}
```

### <a name="logger-module-message-publishing-and-processing"></a><span data-ttu-id="91b52-147">Loggningsmodulens meddelandepublicering och bearbetning</span><span class="sxs-lookup"><span data-stu-id="91b52-147">Logger module message publishing and processing</span></span>

<span data-ttu-id="91b52-148">hello loggaren modulen tar emot meddelanden från hello broker och skriver dem tooa fil.</span><span class="sxs-lookup"><span data-stu-id="91b52-148">hello logger module receives messages from hello broker and writes them tooa file.</span></span> <span data-ttu-id="91b52-149">Den publicerar aldrig meddelanden.</span><span class="sxs-lookup"><span data-stu-id="91b52-149">It never publishes any messages.</span></span> <span data-ttu-id="91b52-150">Därför hello koden för hello loggaren modul anropar hello aldrig **Broker_Publish** funktion.</span><span class="sxs-lookup"><span data-stu-id="91b52-150">Therefore, hello code of hello logger module never calls hello **Broker_Publish** function.</span></span>

<span data-ttu-id="91b52-151">Hej **Logger_Receive** funktion i hello [logger.c] [ lnk-logger-c] filen är hello återanrop hello broker anropar toodeliver meddelanden toohello loggaren modulen.</span><span class="sxs-lookup"><span data-stu-id="91b52-151">hello **Logger_Receive** function in hello [logger.c][lnk-logger-c] file is hello callback hello broker invokes toodeliver messages toohello logger module.</span></span> <span data-ttu-id="91b52-152">hello visas följande fragment en ändrad version med kommentarer som lagts till och vissa felhantering kod tas bort för läsbarhet:</span><span class="sxs-lookup"><span data-stu-id="91b52-152">hello following snippet shows an amended version with comments added and some error handling code removed for legibility:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="91b52-153">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="91b52-153">Next steps</span></span>

<span data-ttu-id="91b52-154">I den här artikeln får körde du en enkel IoT gräns-gatewayen som skriver meddelanden tooa loggfilen.</span><span class="sxs-lookup"><span data-stu-id="91b52-154">In this article, you ran a simple IoT Edge gateway that writes messages tooa log file.</span></span> <span data-ttu-id="91b52-155">toorun ett exempel som skickar meddelanden tooIoT hubben, se [IoT kant – skicka meddelanden från enhet till moln med en simulerad enhet med hjälp av Linux] [ lnk-gateway-simulated-linux] eller [IoT kant – skicka meddelanden från enhet till moln med en simulerade enhet med hjälp av Windows][lnk-gateway-simulated-windows].</span><span class="sxs-lookup"><span data-stu-id="91b52-155">toorun a sample that sends messages tooIoT Hub, see [IoT Edge – send device-to-cloud messages with a simulated device using Linux][lnk-gateway-simulated-linux] or [IoT Edge – send device-to-cloud messages with a simulated device using Windows][lnk-gateway-simulated-windows].</span></span>


<!-- Links -->
[lnk-main-c]: https://github.com/Azure/iot-edge/blob/master/samples/hello_world/src/main.c
[lnk-helloworld-c]: https://github.com/Azure/iot-edge/blob/master/modules/hello_world/src/hello_world.c
[lnk-logger-c]: https://github.com/Azure/iot-edge/blob/master/modules/logger/src/logger.c
[lnk-iot-edge]: https://github.com/Azure/iot-edge/
[lnk-gateway-simulated-linux]: ../articles/iot-hub/iot-hub-linux-iot-edge-simulated-device.md
[lnk-gateway-simulated-windows]: ../articles/iot-hub/iot-hub-windows-iot-edge-simulated-device.md