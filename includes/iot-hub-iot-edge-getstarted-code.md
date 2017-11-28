## <a name="typical-output"></a><span data-ttu-id="a133e-101">Vanliga utdata</span><span class="sxs-lookup"><span data-stu-id="a133e-101">Typical output</span></span>

<span data-ttu-id="a133e-102">I följande exempel visas utdata skrivs till loggfilen av Hello World-exempel.</span><span class="sxs-lookup"><span data-stu-id="a133e-102">The following example shows the output written to the log file by the Hello World sample.</span></span> <span data-ttu-id="a133e-103">Utdata formateras för att de ska vara enkla att läsa:</span><span class="sxs-lookup"><span data-stu-id="a133e-103">The output is formatted for legibility:</span></span>

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

## <a name="code-snippets"></a><span data-ttu-id="a133e-104">Kodfragment</span><span class="sxs-lookup"><span data-stu-id="a133e-104">Code snippets</span></span>

<span data-ttu-id="a133e-105">I det här avsnittet beskrivs viktiga delar av koden i Hello\_World-exemplet.</span><span class="sxs-lookup"><span data-stu-id="a133e-105">This section discusses some key sections of the code in the hello\_world sample.</span></span>

### <a name="iot-edge-gateway-creation"></a><span data-ttu-id="a133e-106">Skapa en IoT-Edge gateway</span><span class="sxs-lookup"><span data-stu-id="a133e-106">IoT Edge gateway creation</span></span>

<span data-ttu-id="a133e-107">Du måste implementera en *gateway-processen*.</span><span class="sxs-lookup"><span data-stu-id="a133e-107">You must implement a *gateway process*.</span></span> <span data-ttu-id="a133e-108">Det här programmet skapar den interna infrastrukturen (broker), laddar IoT kant-moduler och konfigurerar gateway-processen.</span><span class="sxs-lookup"><span data-stu-id="a133e-108">This program creates the internal infrastructure (the broker), loads the IoT Edge modules, and configures the gateway process.</span></span> <span data-ttu-id="a133e-109">IoT Edge innehåller funktionen **Gateway\_Create\_From\_JSON** för att du ska kunna starta en gateway från en JSON-fil.</span><span class="sxs-lookup"><span data-stu-id="a133e-109">IoT Edge provides the **Gateway\_Create\_From\_JSON** function to enable you to bootstrap a gateway from a JSON file.</span></span> <span data-ttu-id="a133e-110">Att använda den **Gateway\_skapa\_från\_JSON** fungera, ange sökvägen till en JSON-fil som anger IoT kant-moduler för att läsa in.</span><span class="sxs-lookup"><span data-stu-id="a133e-110">To use the **Gateway\_Create\_From\_JSON** function, pass it the path to a JSON file that specifies the IoT Edge modules to load.</span></span>

<span data-ttu-id="a133e-111">Du hittar koden för gateway-processen i den *Hello World* exempel i den [main.c] [ lnk-main-c] fil.</span><span class="sxs-lookup"><span data-stu-id="a133e-111">You can find the code for the gateway process in the *Hello World* sample in the [main.c][lnk-main-c] file.</span></span> <span data-ttu-id="a133e-112">För att göra det enklare visas följande kodfragment som en förkortad version av gatewayprocesskoden.</span><span class="sxs-lookup"><span data-stu-id="a133e-112">For legibility, the following snippet shows an abbreviated version of the gateway process code.</span></span> <span data-ttu-id="a133e-113">Det här exempelprogrammet skapar en gateway och väntar sedan på att användaren ska trycka på tangenten **RETUR** innan den monterar ned gatewayen.</span><span class="sxs-lookup"><span data-stu-id="a133e-113">This example program creates a gateway and then waits for the user to press the **ENTER** key before it tears down the gateway.</span></span>

```c
int main(int argc, char** argv)
{
    GATEWAY_HANDLE gateway;
    if ((gateway = Gateway_Create_From_JSON(argv[1])) == NULL)
    {
        printf("failed to create the gateway from JSON\n");
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

<span data-ttu-id="a133e-114">Inställningar för JSON-filen innehåller en lista över IoT kant moduler att läsa in och länkarna mellan moduler.</span><span class="sxs-lookup"><span data-stu-id="a133e-114">The JSON settings file contains a list of IoT Edge modules to load and the links between the modules.</span></span> <span data-ttu-id="a133e-115">Varje gräns för IoT-modul måste ange en:</span><span class="sxs-lookup"><span data-stu-id="a133e-115">Each IoT Edge module must specify a:</span></span>

* <span data-ttu-id="a133e-116">**module_name**: ett unikt namn för modulen.</span><span class="sxs-lookup"><span data-stu-id="a133e-116">**name**: a unique name for the module.</span></span>
* <span data-ttu-id="a133e-117">**loader**: en inläsare som anger hur önskad modul ska läsas in.</span><span class="sxs-lookup"><span data-stu-id="a133e-117">**loader**: a loader that knows how to load the desired module.</span></span> <span data-ttu-id="a133e-118">Inläsare är en tilläggspunkt för inläsning av olika typer av moduler.</span><span class="sxs-lookup"><span data-stu-id="a133e-118">Loaders are an extension point for loading different types of modules.</span></span> <span data-ttu-id="a133e-119">IoT-Edge ger inläsare för användning med moduler som skrivits i interna C, Node.js, Java och .NET.</span><span class="sxs-lookup"><span data-stu-id="a133e-119">IoT Edge provides loaders for use with modules written in native C, Node.js, Java, and .NET.</span></span> <span data-ttu-id="a133e-120">Hello World-exempel endast använder inbyggd C inläsaren eftersom alla moduler i det här exemplet är dynamisk bibliotek som skrivits i C. Mer information om hur du använder IoT kant-moduler som skrivits i olika språk finns i [Node.js](https://github.com/Azure/iot-edge/blob/master/samples/nodejs_simple_sample/), [Java](https://github.com/Azure/iot-edge/tree/master/samples/java_sample), eller [.NET](https://github.com/Azure/iot-edge/tree/master/samples/dotnet_binding_sample) prover.</span><span class="sxs-lookup"><span data-stu-id="a133e-120">The Hello World sample only uses the native C loader because all the modules in this sample are dynamic libraries written in C. For more information about how to use IoT Edge modules written in different languages, see the [Node.js](https://github.com/Azure/iot-edge/blob/master/samples/nodejs_simple_sample/), [Java](https://github.com/Azure/iot-edge/tree/master/samples/java_sample), or [.NET](https://github.com/Azure/iot-edge/tree/master/samples/dotnet_binding_sample) samples.</span></span>
    * <span data-ttu-id="a133e-121">**namnet**: namnet på inläsaren används för att läsa in modulen.</span><span class="sxs-lookup"><span data-stu-id="a133e-121">**name**: the name of the loader used to load the module.</span></span>
    * <span data-ttu-id="a133e-122">**entrypoint**: sökvägen till biblioteket som innehåller modulen.</span><span class="sxs-lookup"><span data-stu-id="a133e-122">**entrypoint**: the path to the library containing the module.</span></span> <span data-ttu-id="a133e-123">För Linux är det här biblioteket en SO-fil, i Windows är det en DLL-fil.</span><span class="sxs-lookup"><span data-stu-id="a133e-123">On Linux this library is a .so file, on Windows this library is a .dll file.</span></span> <span data-ttu-id="a133e-124">Den här registreringspunkten är specifik för den typ av inläsare som används.</span><span class="sxs-lookup"><span data-stu-id="a133e-124">The entry point is specific to the type of loader being used.</span></span> <span data-ttu-id="a133e-125">Startpunkten för Node.js-inläsaren är en JS-fil.</span><span class="sxs-lookup"><span data-stu-id="a133e-125">The Node.js loader entry point is a .js file.</span></span> <span data-ttu-id="a133e-126">Startpunkten för Java-inläsaren är en klassökväg och ett klassnamn.</span><span class="sxs-lookup"><span data-stu-id="a133e-126">The Java loader entry point is a classpath and a class name.</span></span> <span data-ttu-id="a133e-127">Startpunkten för .NET-inläsaren är ett namn på sammansättning och ett klassnamn.</span><span class="sxs-lookup"><span data-stu-id="a133e-127">The .NET loader entry point is an assembly name and a class name.</span></span>

* <span data-ttu-id="a133e-128">**args**: all konfigurationsinformation modulen behöver.</span><span class="sxs-lookup"><span data-stu-id="a133e-128">**args**: any configuration information the module needs.</span></span>

<span data-ttu-id="a133e-129">Följande kod visar JSON som används för att deklarera moduler Hello World-exempel för IoT-gränsen på Linux.</span><span class="sxs-lookup"><span data-stu-id="a133e-129">The following code shows the JSON used to declare all the IoT Edge modules for the Hello World sample on Linux.</span></span> <span data-ttu-id="a133e-130">Huruvida en modul kräver argument eller inte beror på modulens design.</span><span class="sxs-lookup"><span data-stu-id="a133e-130">Whether a module requires any arguments depends on the design of the module.</span></span> <span data-ttu-id="a133e-131">I det här exemplet använder loggningsmodulen ett argument som är sökvägen till utdatafilen och Hello\_World-modulen har inga argument.</span><span class="sxs-lookup"><span data-stu-id="a133e-131">In this example, the logger module takes an argument that is the path to the output file and the hello\_world module has no arguments.</span></span>

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

<span data-ttu-id="a133e-132">JSON-filen innehåller också länkarna mellan de moduler som skickas till den asynkrona meddelandekön.</span><span class="sxs-lookup"><span data-stu-id="a133e-132">The JSON file also contains the links between the modules that are passed to the broker.</span></span> <span data-ttu-id="a133e-133">En länk har två egenskaper:</span><span class="sxs-lookup"><span data-stu-id="a133e-133">A link has two properties:</span></span>

* <span data-ttu-id="a133e-134">**källan**: ett Modulnamn från den `modules` avsnittet eller `\*`.</span><span class="sxs-lookup"><span data-stu-id="a133e-134">**source**: a module name from the `modules` section, or `\*`.</span></span>
* <span data-ttu-id="a133e-135">**mottagare**: ett modulnamn från avsnittet `modules`.</span><span class="sxs-lookup"><span data-stu-id="a133e-135">**sink**: a module name from the `modules` section.</span></span>

<span data-ttu-id="a133e-136">Varje länk definierar en meddelandeväg och -riktning.</span><span class="sxs-lookup"><span data-stu-id="a133e-136">Each link defines a message route and direction.</span></span> <span data-ttu-id="a133e-137">Meddelanden från den **källa** modulen levereras till den **sink** modul.</span><span class="sxs-lookup"><span data-stu-id="a133e-137">Messages from the **source** module are delivered to the **sink** module.</span></span> <span data-ttu-id="a133e-138">Du kan ange den **källa** modulen `\*`, vilket indikerar att den **sink** modulen tar emot meddelanden från en modul.</span><span class="sxs-lookup"><span data-stu-id="a133e-138">You can set the **source** module to `\*`, which indicates that the **sink** module receives messages from any module.</span></span>

<span data-ttu-id="a133e-139">Följande kod visar den JSON som används för att konfigurera länkar mellan moduler som används i Hello\_World-exemplet för Linux.</span><span class="sxs-lookup"><span data-stu-id="a133e-139">The following code shows the JSON used to configure links between the modules used in the hello\_world sample on Linux.</span></span> <span data-ttu-id="a133e-140">Alla meddelanden som genereras av modul `hello_world` används av modul `logger`.</span><span class="sxs-lookup"><span data-stu-id="a133e-140">Every message produced by the `hello_world` module is consumed by the `logger` module.</span></span>

```json
"links":
[
    {
        "source": "hello_world",
        "sink": "logger"
    }
]
```

### <a name="helloworld-module-message-publishing"></a><span data-ttu-id="a133e-141">Meddelandepublicering från Hello\_World-modulen</span><span class="sxs-lookup"><span data-stu-id="a133e-141">Hello\_world module message publishing</span></span>

<span data-ttu-id="a133e-142">Du kan hitta koden som används av modulen Hello\_World för att publicera meddelanden i filen [”hello_world.c”][lnk-helloworld-c].</span><span class="sxs-lookup"><span data-stu-id="a133e-142">You can find the code used by the hello\_world module to publish messages in the ['hello_world.c'][lnk-helloworld-c] file.</span></span> <span data-ttu-id="a133e-143">Kodfragmentet nedan visar en ändrad version med ytterligare kommentarer tillagda och viss felhanteringskod tas bort för att förenkla:</span><span class="sxs-lookup"><span data-stu-id="a133e-143">The following snippet shows an amended version of the code with comments added and some error handling code removed for legibility:</span></span>

```c
int helloWorldThread(void *param)
{
    // create data structures used in function.
    HELLOWORLD_HANDLE_DATA* handleData = param;
    MESSAGE_CONFIG msgConfig;
    MAP_HANDLE propertiesMap = Map_Create(NULL);

    // add a property named "helloWorld" with a value of "from Azure IoT
    // Gateway SDK simple sample!" to a set of message properties that
    // will be appended to the message before publishing it. 
    Map_AddOrUpdate(propertiesMap, "helloWorld", "from Azure IoT Gateway SDK simple sample!")

    // set the content for the message
    msgConfig.size = strlen(HELLOWORLD_MESSAGE);
    msgConfig.source = HELLOWORLD_MESSAGE;

    // set the properties for the message
    msgConfig.sourceProperties = propertiesMap;

    // create a message based on the msgConfig structure
    MESSAGE_HANDLE helloWorldMessage = Message_Create(&msgConfig);

    while (1)
    {
        if (handleData->stopThread)
        {
            (void)Unlock(handleData->lockHandle);
            break; /*gets out of the thread*/
        }
        else
        {
            // publish the message to the broker
            (void)Broker_Publish(handleData->brokerHandle, helloWorldMessage);
            (void)Unlock(handleData->lockHandle);
        }

        (void)ThreadAPI_Sleep(5000); /*every 5 seconds*/
    }

    Message_Destroy(helloWorldMessage);

    return 0;
}
```

### <a name="helloworld-module-message-processing"></a><span data-ttu-id="a133e-144">Meddelandebearbetning från Hello\_World-modulen</span><span class="sxs-lookup"><span data-stu-id="a133e-144">Hello\_world module message processing</span></span>

<span data-ttu-id="a133e-145">Hello\_world modulen bearbetar aldrig meddelanden som andra IoT kant-moduler som publicerar i Service broker.</span><span class="sxs-lookup"><span data-stu-id="a133e-145">The hello\_world module never processes messages that other IoT Edge modules publish to the broker.</span></span> <span data-ttu-id="a133e-146">Detta gör implementeringen av meddelandets återanrop i modulen Hello\_World till en icke-alternativ funktion.</span><span class="sxs-lookup"><span data-stu-id="a133e-146">Therefore, the implementation of the message callback in the hello\_world module is a no-op function.</span></span>

```c
static void HelloWorld_Receive(MODULE_HANDLE moduleHandle, MESSAGE_HANDLE messageHandle)
{
    /* No action, HelloWorld is not interested in any messages. */
}
```

### <a name="logger-module-message-publishing-and-processing"></a><span data-ttu-id="a133e-147">Loggningsmodulens meddelandepublicering och bearbetning</span><span class="sxs-lookup"><span data-stu-id="a133e-147">Logger module message publishing and processing</span></span>

<span data-ttu-id="a133e-148">Loggningsmodulen tar emot meddelanden från den asynkrona meddelandekön och skriver dem till en fil.</span><span class="sxs-lookup"><span data-stu-id="a133e-148">The logger module receives messages from the broker and writes them to a file.</span></span> <span data-ttu-id="a133e-149">Den publicerar aldrig meddelanden.</span><span class="sxs-lookup"><span data-stu-id="a133e-149">It never publishes any messages.</span></span> <span data-ttu-id="a133e-150">Därför anropar koden för loggningsmodulen aldrig funktionen **Broker_Publish**.</span><span class="sxs-lookup"><span data-stu-id="a133e-150">Therefore, the code of the logger module never calls the **Broker_Publish** function.</span></span>

<span data-ttu-id="a133e-151">Den **Logger_Receive** fungera i den [logger.c] [ lnk-logger-c] filen är återanropet Service broker anropar för att leverera meddelanden till modulen meddelandeloggfiler.</span><span class="sxs-lookup"><span data-stu-id="a133e-151">The **Logger_Receive** function in the [logger.c][lnk-logger-c] file is the callback the broker invokes to deliver messages to the logger module.</span></span> <span data-ttu-id="a133e-152">Kodfragmentet nedan visar en ändrad version med kommentarer tillagda och viss felhanteringskod tas bort för att förenkla:</span><span class="sxs-lookup"><span data-stu-id="a133e-152">The following snippet shows an amended version with comments added and some error handling code removed for legibility:</span></span>

```c
static void Logger_Receive(MODULE_HANDLE moduleHandle, MESSAGE_HANDLE messageHandle)
{

    time_t temp = time(NULL);
    struct tm* t = localtime(&temp);
    char timetemp[80] = { 0 };

    // Get the message properties from the message
    CONSTMAP_HANDLE originalProperties = Message_GetProperties(messageHandle); 
    MAP_HANDLE propertiesAsMap = ConstMap_CloneWriteable(originalProperties);

    // Convert the collection of properties into a JSON string
    STRING_HANDLE jsonProperties = Map_ToJSON(propertiesAsMap);

    //  base64 encode the message content
    const CONSTBUFFER * content = Message_GetContent(messageHandle);
    STRING_HANDLE contentAsJSON = Base64_Encode_Bytes(content->buffer, content->size);

    // Start the construction of the final string to be logged by adding
    // the timestamp
    STRING_HANDLE jsonToBeAppended = STRING_construct(",{\"time\":\"");
    STRING_concat(jsonToBeAppended, timetemp);

    // Add the message properties
    STRING_concat(jsonToBeAppended, "\",\"properties\":"); 
    STRING_concat_with_STRING(jsonToBeAppended, jsonProperties);

    // Add the content
    STRING_concat(jsonToBeAppended, ",\"content\":\"");
    STRING_concat_with_STRING(jsonToBeAppended, contentAsJSON);
    STRING_concat(jsonToBeAppended, "\"}]");

    // Write the formatted string
    LOGGER_HANDLE_DATA *handleData = (LOGGER_HANDLE_DATA *)moduleHandle;
    addJSONString(handleData->fout, STRING_c_str(jsonToBeAppended);
}
```

## <a name="next-steps"></a><span data-ttu-id="a133e-153">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a133e-153">Next steps</span></span>

<span data-ttu-id="a133e-154">I den här artikeln får körde du en enkel IoT gräns-gatewayen som skriver meddelanden till en loggfil.</span><span class="sxs-lookup"><span data-stu-id="a133e-154">In this article, you ran a simple IoT Edge gateway that writes messages to a log file.</span></span> <span data-ttu-id="a133e-155">Om du vill köra ett exempel som skickar meddelanden till IoT-hubb finns [IoT kant – skicka meddelanden från enhet till moln med en simulerad enhet med hjälp av Linux] [ lnk-gateway-simulated-linux] eller [IoT kant – skicka meddelanden från enhet till moln med en simulerade enhet med hjälp av Windows][lnk-gateway-simulated-windows].</span><span class="sxs-lookup"><span data-stu-id="a133e-155">To run a sample that sends messages to IoT Hub, see [IoT Edge – send device-to-cloud messages with a simulated device using Linux][lnk-gateway-simulated-linux] or [IoT Edge – send device-to-cloud messages with a simulated device using Windows][lnk-gateway-simulated-windows].</span></span>


<!-- Links -->
[lnk-main-c]: https://github.com/Azure/iot-edge/blob/master/samples/hello_world/src/main.c
[lnk-helloworld-c]: https://github.com/Azure/iot-edge/blob/master/modules/hello_world/src/hello_world.c
[lnk-logger-c]: https://github.com/Azure/iot-edge/blob/master/modules/logger/src/logger.c
[lnk-iot-edge]: https://github.com/Azure/iot-edge/
[lnk-gateway-simulated-linux]: ../articles/iot-hub/iot-hub-linux-iot-edge-simulated-device.md
[lnk-gateway-simulated-windows]: ../articles/iot-hub/iot-hub-windows-iot-edge-simulated-device.md