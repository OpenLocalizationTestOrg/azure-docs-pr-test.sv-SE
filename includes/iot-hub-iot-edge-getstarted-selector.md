> [!div class="op_single_selector"]
> * [<span data-ttu-id="fa712-101">Linux</span><span class="sxs-lookup"><span data-stu-id="fa712-101">Linux</span></span>](../articles/iot-hub/iot-hub-linux-iot-edge-get-started.md)
> * [<span data-ttu-id="fa712-102">Windows</span><span class="sxs-lookup"><span data-stu-id="fa712-102">Windows</span></span>](../articles/iot-hub/iot-hub-windows-iot-edge-get-started.md)
> 
> 

<span data-ttu-id="fa712-103">Den här artikeln innehåller en detaljerad genomgång av hello [Hello World exempelkod] [ lnk-helloworld-sample] tooillustrate hello grundläggande komponenter i hello [Azure IoT kant] [ lnk-iot-edge] arkitektur.</span><span class="sxs-lookup"><span data-stu-id="fa712-103">This article provides a detailed walkthrough of hello [Hello World sample code][lnk-helloworld-sample] tooillustrate hello fundamental components of hello [Azure IoT Edge][lnk-iot-edge] architecture.</span></span> <span data-ttu-id="fa712-104">hello används hello Azure IoT kant toobuild en enkel gateway som loggar tooa för ”hello world” meddelandefilen var femte sekund.</span><span class="sxs-lookup"><span data-stu-id="fa712-104">hello sample uses hello Azure IoT Edge toobuild a simple gateway that logs a "hello world" message tooa file every five seconds.</span></span>

<span data-ttu-id="fa712-105">Den här genomgången omfattar:</span><span class="sxs-lookup"><span data-stu-id="fa712-105">This walkthrough covers:</span></span>

* <span data-ttu-id="fa712-106">**Hello World exempelarkitektur**: Beskriver hur [Azure IoT kant arkitektur begrepp] [ lnk-edge-concepts] tillämpa toohello Hello World-exempel och hur hello komponenter fungerar ihop.</span><span class="sxs-lookup"><span data-stu-id="fa712-106">**Hello World sample architecture**: Describes how [Azure IoT Edge architectural concepts][lnk-edge-concepts] apply toohello Hello World sample and how hello components fit together.</span></span>
* <span data-ttu-id="fa712-107">**Hur toobuild hello exempel**: hello steg krävs toobuild hello exempel.</span><span class="sxs-lookup"><span data-stu-id="fa712-107">**How toobuild hello sample**: hello steps required toobuild hello sample.</span></span>
* <span data-ttu-id="fa712-108">**Hur toorun hello exempel**: hello steg krävs toorun hello exempel.</span><span class="sxs-lookup"><span data-stu-id="fa712-108">**How toorun hello sample**: hello steps required toorun hello sample.</span></span> 
* <span data-ttu-id="fa712-109">**Vanliga utdata**: ett exempel på hello utdata tooexpect när du kör hello exempel.</span><span class="sxs-lookup"><span data-stu-id="fa712-109">**Typical output**: An example of hello output tooexpect when you run hello sample.</span></span>
* <span data-ttu-id="fa712-110">**Kodfragment**: en samling av koden kodavsnitt tooshow hur hello Hello World-exempel implementerar nyckeln IoT Edge gateway komponenter.</span><span class="sxs-lookup"><span data-stu-id="fa712-110">**Code snippets**: A collection of code snippets tooshow how hello Hello World sample implements key IoT Edge gateway components.</span></span>


## <a name="hello-world-sample-architecture"></a><span data-ttu-id="fa712-111">Arkitektur för Hello World-exempel</span><span class="sxs-lookup"><span data-stu-id="fa712-111">Hello World sample architecture</span></span>
<span data-ttu-id="fa712-112">hello Hello World-exempel illustrerar hello begrepp som beskrivs i föregående avsnitt i hello.</span><span class="sxs-lookup"><span data-stu-id="fa712-112">hello Hello World sample illustrates hello concepts described in hello previous section.</span></span> <span data-ttu-id="fa712-113">hello Hello World-exempel implementerar en IoT-gräns-gatewayen som har en pipeline som består av två IoT kant moduler:</span><span class="sxs-lookup"><span data-stu-id="fa712-113">hello Hello World sample implements a IoT Edge gateway that has a pipeline made up of two IoT Edge modules:</span></span>

* <span data-ttu-id="fa712-114">Hej *hello world* modulen och skapar ett meddelande var femte sekund och skickar den toohello loggaren modulen.</span><span class="sxs-lookup"><span data-stu-id="fa712-114">hello *hello world* module creates a message every five seconds and passes it toohello logger module.</span></span>
* <span data-ttu-id="fa712-115">Hej *loggaren* modulen skrivningar hello meddelanden som tas emot tooa fil.</span><span class="sxs-lookup"><span data-stu-id="fa712-115">hello *logger* module writes hello messages it receives tooa file.</span></span>

![Arkitekturen i Hello World-exemplet som skapats med Azure IoT Edge][4]

<span data-ttu-id="fa712-117">Enligt beskrivningen i föregående avsnitt i hello meddelanden hello Hello World modulen inte klarar direkt toohello loggaren modulen var femte sekund.</span><span class="sxs-lookup"><span data-stu-id="fa712-117">As described in hello previous section, hello Hello World module does not pass messages directly toohello logger module every five seconds.</span></span> <span data-ttu-id="fa712-118">I stället publicerar den förhandlare toohello meddelande var femte sekund.</span><span class="sxs-lookup"><span data-stu-id="fa712-118">Instead, it publishes a message toohello broker every five seconds.</span></span>

<span data-ttu-id="fa712-119">hello loggaren modulen tar emot hello-meddelande från hello broker och agerar på, skriver hello innehållet i hello meddelandefilen tooa.</span><span class="sxs-lookup"><span data-stu-id="fa712-119">hello logger module receives hello message from hello broker and acts upon it, writing hello contents of hello message tooa file.</span></span>

<span data-ttu-id="fa712-120">hello loggaren modulen förbrukar endast meddelanden från hello broker, den publicerar aldrig nya meddelanden toohello broker.</span><span class="sxs-lookup"><span data-stu-id="fa712-120">hello logger module only consumes messages from hello broker, it never publishes new messages toohello broker.</span></span>

![Hur hello broker skickar meddelanden mellan moduler i Azure IoT kant][5]

<span data-ttu-id="fa712-122">hello ovanstående bild visar hello och arkitekturen för hello Hello World-exempel hello relativa sökvägar toohello källfiler som implementerar olika delar av hello exemplet i hello [databasen][lnk-iot-edge].</span><span class="sxs-lookup"><span data-stu-id="fa712-122">hello figure above shows hello architecture of hello Hello World sample and hello relative paths toohello source files that implement different portions of hello sample in hello [repository][lnk-iot-edge].</span></span> <span data-ttu-id="fa712-123">Utforska hello koden på egen hand eller Använd hello kodavsnitten nedan som vägledning.</span><span class="sxs-lookup"><span data-stu-id="fa712-123">Explore hello code on your own, or use hello code snippets below as a guide.</span></span>

<!-- Images -->
[4]: media/iot-hub-iot-edge-getstarted-selector/high_level_architecture.png
[5]: media/iot-hub-iot-edge-getstarted-selector/detailed_architecture.png

<!-- Links -->
[lnk-helloworld-sample]: https://github.com/Azure/iot-edge/tree/master/samples/hello_world
[lnk-iot-edge]: https://github.com/Azure/iot-edge
[lnk-edge-concepts]: ../articles/iot-hub/iot-hub-iot-edge-overview.md