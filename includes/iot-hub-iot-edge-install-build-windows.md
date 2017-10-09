## <a name="install-hello-prerequisites"></a><span data-ttu-id="3cd64-101">Installera hello förutsättningar</span><span class="sxs-lookup"><span data-stu-id="3cd64-101">Install hello prerequisites</span></span>

1. <span data-ttu-id="3cd64-102">Installera [Visual Studio 2015 eller 2017](https://www.visualstudio.com).</span><span class="sxs-lookup"><span data-stu-id="3cd64-102">Install [Visual Studio 2015 or 2017](https://www.visualstudio.com).</span></span> <span data-ttu-id="3cd64-103">Du kan använda hello kostnadsfri Community Edition om du uppfyller hello licenskraven.</span><span class="sxs-lookup"><span data-stu-id="3cd64-103">You can use hello free Community Edition if you meet hello licensing requirements.</span></span> <span data-ttu-id="3cd64-104">Vara säker på att tooinclude Visual C++ och NuGet Package Manager.</span><span class="sxs-lookup"><span data-stu-id="3cd64-104">Be sure tooinclude Visual C++ and NuGet Package Manager.</span></span>

1. <span data-ttu-id="3cd64-105">Installera [git](http://www.git-scm.com) och kontrollera att du kan köra git.exe från hello-kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="3cd64-105">Install [git](http://www.git-scm.com) and make sure you can run git.exe from hello command line.</span></span>

1. <span data-ttu-id="3cd64-106">Installera [CMake](https://cmake.org/download/) och kontrollera att du kan köra cmake.exe från hello-kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="3cd64-106">Install [CMake](https://cmake.org/download/) and make sure you can run cmake.exe from hello command line.</span></span> <span data-ttu-id="3cd64-107">CMake version 3.7.2 eller senare rekommenderas.</span><span class="sxs-lookup"><span data-stu-id="3cd64-107">CMake version 3.7.2 or later is recommended.</span></span> <span data-ttu-id="3cd64-108">Hej **.msi** installer är hello enklaste alternativet i Windows.</span><span class="sxs-lookup"><span data-stu-id="3cd64-108">hello **.msi** installer is hello easiest option on Windows.</span></span> <span data-ttu-id="3cd64-109">Lägg till CMake toohello SÖKVÄGEN för minst hello aktuell användare när du ombeds hello installer.</span><span class="sxs-lookup"><span data-stu-id="3cd64-109">Add CMake toohello PATH for at least hello current user when hello installer prompts you.</span></span>

1. <span data-ttu-id="3cd64-110">Installera [Python 2.7](https://www.python.org/downloads/release/python-27).</span><span class="sxs-lookup"><span data-stu-id="3cd64-110">Install [Python 2.7](https://www.python.org/downloads/release/python-27).</span></span> <span data-ttu-id="3cd64-111">Kontrollera att du lägger till Python tooyour `PATH` miljövariabeln i **Kontrollpanelen -> System -> Avancerat systeminställningar -> miljövariabler**.</span><span class="sxs-lookup"><span data-stu-id="3cd64-111">Make sure you add Python tooyour `PATH` environment variable in **Control Panel -> System -> Advanced system settings -> Environment Variables**.</span></span>

1. <span data-ttu-id="3cd64-112">Kör hello efter kommandot tooclone hello Azure IoT kant GitHub-lagringsplatsen tooyour lokal dator i en kommandotolk:</span><span class="sxs-lookup"><span data-stu-id="3cd64-112">At a command prompt, run hello following command tooclone hello Azure IoT Edge GitHub repository tooyour local machine:</span></span>

    ```cmd
    git clone https://github.com/Azure/iot-edge.git
    ```

## <a name="how-toobuild-hello-sample"></a><span data-ttu-id="3cd64-113">Hur toobuild hello exempel</span><span class="sxs-lookup"><span data-stu-id="3cd64-113">How toobuild hello sample</span></span>

<span data-ttu-id="3cd64-114">Du kan nu skapa hello IoT kant runtime och exempel på den lokala datorn:</span><span class="sxs-lookup"><span data-stu-id="3cd64-114">You can now build hello IoT Edge runtime and samples on your local machine:</span></span>

1. <span data-ttu-id="3cd64-115">Öppna en **Developer kommandotolk för VS 2015** eller **Developer kommandotolk för VS 2017** kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="3cd64-115">Open a **Developer Command Prompt for VS 2015** or **Developer Command Prompt for VS 2017** command prompt.</span></span>

1. <span data-ttu-id="3cd64-116">Navigera toohello rotmappen i den lokala kopian av hello **iot-edge** databasen.</span><span class="sxs-lookup"><span data-stu-id="3cd64-116">Navigate toohello root folder in your local copy of hello **iot-edge** repository.</span></span>

1. <span data-ttu-id="3cd64-117">Kör skriptet bygga på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="3cd64-117">Run the build script as follows:</span></span>

    ```cmd
    tools\build.cmd --disable-native-remote-modules
    ```

<span data-ttu-id="3cd64-118">Det här skriptet skapar en fil för Visual Studio-lösningen och bygger hello lösning.</span><span class="sxs-lookup"><span data-stu-id="3cd64-118">This script creates a Visual Studio solution file and builds hello solution.</span></span> <span data-ttu-id="3cd64-119">Du hittar hello Visual Studio-lösning i hello **skapa** mapp i den lokala kopian av hello **iot-edge** databasen.</span><span class="sxs-lookup"><span data-stu-id="3cd64-119">You can find hello Visual Studio solution in hello **build** folder in your local copy of hello **iot-edge** repository.</span></span> <span data-ttu-id="3cd64-120">Om du vill toobuild och köra hello kontroller, lägger du till hello `--run-unittests` parameter.</span><span class="sxs-lookup"><span data-stu-id="3cd64-120">If you want toobuild and run hello unit tests, add hello `--run-unittests` parameter.</span></span> <span data-ttu-id="3cd64-121">Om du vill toobuild och köra hello slutet tooend tester, lägger du till hello `--run-e2e-tests`.</span><span class="sxs-lookup"><span data-stu-id="3cd64-121">If you want toobuild and run hello end tooend tests, add hello `--run-e2e-tests`.</span></span>

> [!NOTE]
> <span data-ttu-id="3cd64-122">Varje gång du kör hello **build.cmd** skript, tas bort och återskapar hello **skapa** mapp i hello rotmappen på den lokala kopian av hello **iot-edge** databasen.</span><span class="sxs-lookup"><span data-stu-id="3cd64-122">Every time you run hello **build.cmd** script, it deletes and then recreates hello **build** folder in hello root folder of your local copy of hello **iot-edge** repository.</span></span>
