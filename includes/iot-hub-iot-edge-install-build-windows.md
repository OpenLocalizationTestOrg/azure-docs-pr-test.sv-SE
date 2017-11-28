## <a name="install-the-prerequisites"></a><span data-ttu-id="fedb3-101">Installera nödvändiga komponenter</span><span class="sxs-lookup"><span data-stu-id="fedb3-101">Install the prerequisites</span></span>

1. <span data-ttu-id="fedb3-102">Installera [Visual Studio 2015 eller 2017](https://www.visualstudio.com).</span><span class="sxs-lookup"><span data-stu-id="fedb3-102">Install [Visual Studio 2015 or 2017](https://www.visualstudio.com).</span></span> <span data-ttu-id="fedb3-103">Du kan använda den kostnadsfria Community Edition om du uppfyller licenskraven.</span><span class="sxs-lookup"><span data-stu-id="fedb3-103">You can use the free Community Edition if you meet the licensing requirements.</span></span> <span data-ttu-id="fedb3-104">Se till att inkludera Visual C++ och NuGet Package Manager.</span><span class="sxs-lookup"><span data-stu-id="fedb3-104">Be sure to include Visual C++ and NuGet Package Manager.</span></span>

1. <span data-ttu-id="fedb3-105">Installera [git](http://www.git-scm.com) och kontrollera att du kan köra git.exe från kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="fedb3-105">Install [git](http://www.git-scm.com) and make sure you can run git.exe from the command line.</span></span>

1. <span data-ttu-id="fedb3-106">Installera [CMake](https://cmake.org/download/) och kontrollera att du kan köra cmake.exe från kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="fedb3-106">Install [CMake](https://cmake.org/download/) and make sure you can run cmake.exe from the command line.</span></span> <span data-ttu-id="fedb3-107">CMake version 3.7.2 eller senare rekommenderas.</span><span class="sxs-lookup"><span data-stu-id="fedb3-107">CMake version 3.7.2 or later is recommended.</span></span> <span data-ttu-id="fedb3-108">Den **.msi** installer är det enklaste alternativet i Windows.</span><span class="sxs-lookup"><span data-stu-id="fedb3-108">The **.msi** installer is the easiest option on Windows.</span></span> <span data-ttu-id="fedb3-109">Lägg till CMake minst till SÖKVÄGEN för den aktuella användaren när du uppmanas av installationsprogrammet.</span><span class="sxs-lookup"><span data-stu-id="fedb3-109">Add CMake to the PATH for at least the current user when the installer prompts you.</span></span>

1. <span data-ttu-id="fedb3-110">Installera [Python 2.7](https://www.python.org/downloads/release/python-27).</span><span class="sxs-lookup"><span data-stu-id="fedb3-110">Install [Python 2.7](https://www.python.org/downloads/release/python-27).</span></span> <span data-ttu-id="fedb3-111">Kontrollera att du kan lägga till Python till din `PATH` miljövariabeln i **Kontrollpanelen -> System -> Avancerat systeminställningar -> miljövariabler**.</span><span class="sxs-lookup"><span data-stu-id="fedb3-111">Make sure you add Python to your `PATH` environment variable in **Control Panel -> System -> Advanced system settings -> Environment Variables**.</span></span>

1. <span data-ttu-id="fedb3-112">Kör följande kommando för att klona Azure IoT kant GitHub-lagringsplatsen till den lokala datorn i en kommandotolk:</span><span class="sxs-lookup"><span data-stu-id="fedb3-112">At a command prompt, run the following command to clone the Azure IoT Edge GitHub repository to your local machine:</span></span>

    ```cmd
    git clone https://github.com/Azure/iot-edge.git
    ```

## <a name="how-to-build-the-sample"></a><span data-ttu-id="fedb3-113">Hur du skapar provet</span><span class="sxs-lookup"><span data-stu-id="fedb3-113">How to build the sample</span></span>

<span data-ttu-id="fedb3-114">Du kan nu skapa IoT-Edge runtime och exempel på den lokala datorn:</span><span class="sxs-lookup"><span data-stu-id="fedb3-114">You can now build the IoT Edge runtime and samples on your local machine:</span></span>

1. <span data-ttu-id="fedb3-115">Öppna en **Developer kommandotolk för VS 2015** eller **Developer kommandotolk för VS 2017** kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="fedb3-115">Open a **Developer Command Prompt for VS 2015** or **Developer Command Prompt for VS 2017** command prompt.</span></span>

1. <span data-ttu-id="fedb3-116">Navigera till rotmappen i den lokala kopian av databasen **iot-edge**.</span><span class="sxs-lookup"><span data-stu-id="fedb3-116">Navigate to the root folder in your local copy of the **iot-edge** repository.</span></span>

1. <span data-ttu-id="fedb3-117">Kör skriptet bygga på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="fedb3-117">Run the build script as follows:</span></span>

    ```cmd
    tools\build.cmd --disable-native-remote-modules
    ```

<span data-ttu-id="fedb3-118">Det här skriptet skapar en fil för Visual Studio-lösning och bygger lösningen.</span><span class="sxs-lookup"><span data-stu-id="fedb3-118">This script creates a Visual Studio solution file and builds the solution.</span></span> <span data-ttu-id="fedb3-119">Du kan hitta Visual Studio-lösning i den **skapa** mapp i den lokala kopian av den **iot-edge** databasen.</span><span class="sxs-lookup"><span data-stu-id="fedb3-119">You can find the Visual Studio solution in the **build** folder in your local copy of the **iot-edge** repository.</span></span> <span data-ttu-id="fedb3-120">Om du vill skapa och köra testerna enhet, lägga till den `--run-unittests` parameter.</span><span class="sxs-lookup"><span data-stu-id="fedb3-120">If you want to build and run the unit tests, add the `--run-unittests` parameter.</span></span> <span data-ttu-id="fedb3-121">Om du vill skapa och köra testerna slutpunkt till slutpunkt, lägga till den `--run-e2e-tests`.</span><span class="sxs-lookup"><span data-stu-id="fedb3-121">If you want to build and run the end to end tests, add the `--run-e2e-tests`.</span></span>

> [!NOTE]
> <span data-ttu-id="fedb3-122">Varje gång du kör den **build.cmd** skript, tas bort och återskapas den **skapa** mapp i rotmappen på den lokala kopian av den **iot-edge** databasen.</span><span class="sxs-lookup"><span data-stu-id="fedb3-122">Every time you run the **build.cmd** script, it deletes and then recreates the **build** folder in the root folder of your local copy of the **iot-edge** repository.</span></span>