## <a name="build-iot-edge"></a><span data-ttu-id="967d4-101">Skapa IoT kant</span><span class="sxs-lookup"><span data-stu-id="967d4-101">Build IoT Edge</span></span>

<span data-ttu-id="967d4-102">Den här kursen använder anpassade IoT kant moduler toocommunicate med hello remote förkonfigurerade övervakningslösning.</span><span class="sxs-lookup"><span data-stu-id="967d4-102">This tutorial uses custom IoT Edge modules toocommunicate with hello remote monitoring preconfigured solution.</span></span> <span data-ttu-id="967d4-103">Du måste därför toobuild hello IoT kant moduler från anpassade källkoden.</span><span class="sxs-lookup"><span data-stu-id="967d4-103">Therefore, you need toobuild hello IoT Edge modules from custom source code.</span></span> <span data-ttu-id="967d4-104">hello följande avsnitt beskrivs hur tooinstall IoT kant och bygga hello anpassad IoT kant-modul.</span><span class="sxs-lookup"><span data-stu-id="967d4-104">hello following sections describe how tooinstall IoT Edge and build hello custom IoT Edge module.</span></span>

### <a name="install-iot-edge"></a><span data-ttu-id="967d4-105">Installera IoT kant</span><span class="sxs-lookup"><span data-stu-id="967d4-105">Install IoT Edge</span></span>

<span data-ttu-id="967d4-106">hello följande steg beskriver hur tooinstall hello före kompileras IoT kant programvara på hello Intel NUC:</span><span class="sxs-lookup"><span data-stu-id="967d4-106">hello following steps describe how tooinstall hello pre-compiled IoT Edge software on hello Intel NUC:</span></span>

1. <span data-ttu-id="967d4-107">Konfigurera hello krävs smart paketet databaser genom att köra följande kommandon på hello Intel NUC hello:</span><span class="sxs-lookup"><span data-stu-id="967d4-107">Configure hello required smart package repositories by running hello following commands on hello Intel NUC:</span></span>

    ```bash
    smart channel --add IoT_Cloud type=rpm-md name="IoT_Cloud" baseurl=http://iotdk.intel.com/repos/iot-cloud/wrlinux7/rcpl13/ -y
    smart channel --add WR_Repo type=rpm-md baseurl=https://distro.windriver.com/release/idp-3-xt/public_feeds/WR-IDP-3-XT-Intel-Baytrail-public-repo/RCPL13/corei7_64/
    ```

    <span data-ttu-id="967d4-108">Ange `y` när hello kommandot uppmanar dig för**innehåller den här kanalen?**.</span><span class="sxs-lookup"><span data-stu-id="967d4-108">Enter `y` when hello command prompts you too**Include this channel?**.</span></span>

1. <span data-ttu-id="967d4-109">Uppdatera hello smart Pakethanteraren genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="967d4-109">Update hello smart package manager by running hello following command:</span></span>

    ```bash
    smart update
    ```

1. <span data-ttu-id="967d4-110">Installera hello Azure IoT kant paketet genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="967d4-110">Install hello Azure IoT Edge package by running hello following command:</span></span>

    ```bash
    smart config --set rpm-check-signatures=false
    smart install packagegroup-cloud-azure -y
    ```

1. <span data-ttu-id="967d4-111">Kontrollera hello installationen körs hello ”Hello world” exempel.</span><span class="sxs-lookup"><span data-stu-id="967d4-111">Verify hello installation by running hello "Hello world" sample.</span></span> <span data-ttu-id="967d4-112">Det här exemplet skriver en hello world meddelandet toohello log.txT fil var femte sekund.</span><span class="sxs-lookup"><span data-stu-id="967d4-112">This sample writes a hello world message toohello log.txT file every five seconds.</span></span> <span data-ttu-id="967d4-113">hello kör följande kommandon hello ”Hello world” exempel:</span><span class="sxs-lookup"><span data-stu-id="967d4-113">hello following commands run hello "Hello world" sample:</span></span>

    ```bash
    cd /usr/share/azureiotgatewaysdk/samples/hello_world/
    ./hello_world hello_world.json
    ```

    <span data-ttu-id="967d4-114">Ignorera alla **ogiltigt argument** meddelanden när du stoppar hello exempel.</span><span class="sxs-lookup"><span data-stu-id="967d4-114">Ignore any **invalid argument** messages when you stop hello sample.</span></span>

    <span data-ttu-id="967d4-115">Använd hello följande kommando tooview hello innehållet i hello loggfil:</span><span class="sxs-lookup"><span data-stu-id="967d4-115">Use hello following command tooview hello contents of hello log file:</span></span>

    ```bash
    cat log.txt | more
    ```

### <a name="troubleshooting"></a><span data-ttu-id="967d4-116">Felsökning</span><span class="sxs-lookup"><span data-stu-id="967d4-116">Troubleshooting</span></span>

<span data-ttu-id="967d4-117">Om felmeddelande hello ”erbjuder ingen util-linux-utveckling”, startar du om datorn hello Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="967d4-117">If you receive hello error "No package provides util-linux-dev", try rebooting hello Intel NUC.</span></span>
