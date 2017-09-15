> [!div class="op_single_selector"]
> * [<span data-ttu-id="5df03-101">Linux</span><span class="sxs-lookup"><span data-stu-id="5df03-101">Linux</span></span>](../articles/iot-hub/iot-hub-linux-iot-edge-get-started.md)
> * [<span data-ttu-id="5df03-102">Windows</span><span class="sxs-lookup"><span data-stu-id="5df03-102">Windows</span></span>](../articles/iot-hub/iot-hub-windows-iot-edge-get-started.md)
> 
> 

<span data-ttu-id="5df03-103">Den här artikeln innehåller en detaljerad genomgång av [Hello World-exempelkoden][lnk-helloworld-sample] och illustrerar grundläggande komponenter i [Azure IoT Edge][lnk-iot-edge]-arkitekturen.</span><span class="sxs-lookup"><span data-stu-id="5df03-103">This article provides a detailed walkthrough of the [Hello World sample code][lnk-helloworld-sample] to illustrate the fundamental components of the [Azure IoT Edge][lnk-iot-edge] architecture.</span></span> <span data-ttu-id="5df03-104">I exemplet används Azure IoT Edge för att skapa en enkel gateway som loggar ett meddelande med ”hello world” i en fil var femte sekund.</span><span class="sxs-lookup"><span data-stu-id="5df03-104">The sample uses the Azure IoT Edge to build a simple gateway that logs a "hello world" message to a file every five seconds.</span></span>

<span data-ttu-id="5df03-105">Den här genomgången omfattar:</span><span class="sxs-lookup"><span data-stu-id="5df03-105">This walkthrough covers:</span></span>

* <span data-ttu-id="5df03-106">**Hello World exempelarkitektur**: Beskriver hur [Azure IoT kant arkitektur begrepp] [ lnk-edge-concepts] gäller Hello World-exempel och hur komponenterna fungerar ihop.</span><span class="sxs-lookup"><span data-stu-id="5df03-106">**Hello World sample architecture**: Describes how [Azure IoT Edge architectural concepts][lnk-edge-concepts] apply to the Hello World sample and how the components fit together.</span></span>
* <span data-ttu-id="5df03-107">**Hur du skapar exemplet**: De steg som krävs för att skapa exemplet.</span><span class="sxs-lookup"><span data-stu-id="5df03-107">**How to build the sample**: The steps required to build the sample.</span></span>
* <span data-ttu-id="5df03-108">**Hur du kör exemplet**: De steg som krävs för att köra exemplet.</span><span class="sxs-lookup"><span data-stu-id="5df03-108">**How to run the sample**: The steps required to run the sample.</span></span> 
* <span data-ttu-id="5df03-109">**Vanliga utdata**: Ett exempel på utdata du kan förvänta dig när du kör exemplet.</span><span class="sxs-lookup"><span data-stu-id="5df03-109">**Typical output**: An example of the output to expect when you run the sample.</span></span>
* <span data-ttu-id="5df03-110">**Kodfragment**: en samling med kodstycken att visa hur Hello World-exempel implementerar gateway nyckelkomponenterna IoT kant.</span><span class="sxs-lookup"><span data-stu-id="5df03-110">**Code snippets**: A collection of code snippets to show how the Hello World sample implements key IoT Edge gateway components.</span></span>


## <a name="hello-world-sample-architecture"></a><span data-ttu-id="5df03-111">Arkitektur för Hello World-exempel</span><span class="sxs-lookup"><span data-stu-id="5df03-111">Hello World sample architecture</span></span>
<span data-ttu-id="5df03-112">Hello World-exemplet illustrerar de begrepp som beskrivs i föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="5df03-112">The Hello World sample illustrates the concepts described in the previous section.</span></span> <span data-ttu-id="5df03-113">Hello World-exempel implementerar en IoT-gräns-gatewayen som har en pipeline som består av två IoT kant moduler:</span><span class="sxs-lookup"><span data-stu-id="5df03-113">The Hello World sample implements a IoT Edge gateway that has a pipeline made up of two IoT Edge modules:</span></span>

* <span data-ttu-id="5df03-114">Modulen *hello world* skapar ett meddelande var femte sekund och skickar det till loggningsmodulen.</span><span class="sxs-lookup"><span data-stu-id="5df03-114">The *hello world* module creates a message every five seconds and passes it to the logger module.</span></span>
* <span data-ttu-id="5df03-115">Modulen *loggning* skriver de meddelanden den tar emot till en fil.</span><span class="sxs-lookup"><span data-stu-id="5df03-115">The *logger* module writes the messages it receives to a file.</span></span>

![Arkitekturen i Hello World-exemplet som skapats med Azure IoT Edge][4]

<span data-ttu-id="5df03-117">Enligt beskrivningen i föregående avsnitt, skickar modulen Hello World inte meddelanden direkt till loggningsmodueln var femte sekund.</span><span class="sxs-lookup"><span data-stu-id="5df03-117">As described in the previous section, the Hello World module does not pass messages directly to the logger module every five seconds.</span></span> <span data-ttu-id="5df03-118">I stället publicerar den ett meddelande till den asynkrona meddelandekön var femte sekund.</span><span class="sxs-lookup"><span data-stu-id="5df03-118">Instead, it publishes a message to the broker every five seconds.</span></span>

<span data-ttu-id="5df03-119">Loggningsmodulen får meddelandet från den asynkrona meddelandekön och agerar på meddelandet genom att skriva meddelandets innehåll till en fil.</span><span class="sxs-lookup"><span data-stu-id="5df03-119">The logger module receives the message from the broker and acts upon it, writing the contents of the message to a file.</span></span>

<span data-ttu-id="5df03-120">Loggningsmodulen förbrukar endast meddelanden från den asynkrona meddelandekön, den publicerar aldrig nya meddelanden till den asynkrona meddelandekön.</span><span class="sxs-lookup"><span data-stu-id="5df03-120">The logger module only consumes messages from the broker, it never publishes new messages to the broker.</span></span>

![Hur agenten dirigerar meddelanden mellan moduler i Azure IoT Edge][5]

<span data-ttu-id="5df03-122">Bilden ovan illustrerar arkitekturen i Hello World-exemplet och de relativa sökvägarna till källfilerna som implementerar olika delar av exemplet på [lagringsplatsen][lnk-iot-edge].</span><span class="sxs-lookup"><span data-stu-id="5df03-122">The figure above shows the architecture of the Hello World sample and the relative paths to the source files that implement different portions of the sample in the [repository][lnk-iot-edge].</span></span> <span data-ttu-id="5df03-123">Utforska koden på egen hand eller använd kodfragmenten nedan som vägledning.</span><span class="sxs-lookup"><span data-stu-id="5df03-123">Explore the code on your own, or use the code snippets below as a guide.</span></span>

<!-- Images -->
[4]: media/iot-hub-iot-edge-getstarted-selector/high_level_architecture.png
[5]: media/iot-hub-iot-edge-getstarted-selector/detailed_architecture.png

<!-- Links -->
[lnk-helloworld-sample]: https://github.com/Azure/iot-edge/tree/master/samples/hello_world
[lnk-iot-edge]: https://github.com/Azure/iot-edge
[lnk-edge-concepts]: ../articles/iot-hub/iot-hub-iot-edge-overview.md