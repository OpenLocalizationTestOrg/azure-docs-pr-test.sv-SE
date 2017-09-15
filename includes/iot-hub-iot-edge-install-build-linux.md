## <a name="install-the-prerequisites"></a><span data-ttu-id="a78f9-101">Installera nödvändiga komponenter</span><span class="sxs-lookup"><span data-stu-id="a78f9-101">Install the prerequisites</span></span>

<span data-ttu-id="a78f9-102">Stegen i den här kursen förutsätter att du kör Ubuntu Linux.</span><span class="sxs-lookup"><span data-stu-id="a78f9-102">The steps in this tutorial assume you are running Ubuntu Linux.</span></span>

<span data-ttu-id="a78f9-103">Öppna ett gränssnitt och kör följande kommandon för att installera de nödvändiga paketen:</span><span class="sxs-lookup"><span data-stu-id="a78f9-103">Open a shell and run the following commands to install the prerequisite packages:</span></span>

```bash
sudo apt-get update
sudo apt-get install curl build-essential libcurl4-openssl-dev git cmake libssl-dev uuid-dev valgrind libglib2.0-dev libtool autoconf
```

<span data-ttu-id="a78f9-104">Shell, kör du följande kommando för att klona Azure IoT kant GitHub-lagringsplatsen till den lokala datorn:</span><span class="sxs-lookup"><span data-stu-id="a78f9-104">In the shell, run the following command to clone the Azure IoT Edge GitHub repository to your local machine:</span></span>

```bash
git clone https://github.com/Azure/iot-edge.git
```

## <a name="how-to-build-the-sample"></a><span data-ttu-id="a78f9-105">Hur du skapar provet</span><span class="sxs-lookup"><span data-stu-id="a78f9-105">How to build the sample</span></span>

<span data-ttu-id="a78f9-106">Du kan nu skapa IoT-Edge runtime och exempel på den lokala datorn:</span><span class="sxs-lookup"><span data-stu-id="a78f9-106">You can now build the IoT Edge runtime and samples on your local machine:</span></span>

1. <span data-ttu-id="a78f9-107">Öppna ett gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="a78f9-107">Open a shell.</span></span>

1. <span data-ttu-id="a78f9-108">Navigera till rotmappen i den lokala kopian av databasen **iot-edge**.</span><span class="sxs-lookup"><span data-stu-id="a78f9-108">Navigate to the root folder in your local copy of the **iot-edge** repository.</span></span>

1. <span data-ttu-id="a78f9-109">Kör skriptet bygga på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="a78f9-109">Run the build script as follows:</span></span>

    ```sh
    tools/build.sh --disable-native-remote-modules
    ```

<span data-ttu-id="a78f9-110">Det här skriptet använder verktyget **cmake** för att skapa en mapp som kallas **build** i rotmappen på den lokala kopian av databasen **iot-edge** och generera en make-fil.</span><span class="sxs-lookup"><span data-stu-id="a78f9-110">This script uses the **cmake** utility to create a folder called **build** in the root folder of your local copy of the **iot-edge** repository and generate a makefile.</span></span> <span data-ttu-id="a78f9-111">Skriptet bygger sedan lösningen och hoppar över enhetstester och slutpunkt till slutpunkt-tester.</span><span class="sxs-lookup"><span data-stu-id="a78f9-111">The script then builds the solution, skipping unit tests and end to end tests.</span></span> <span data-ttu-id="a78f9-112">Om du vill skapa och köra testerna enhet, lägga till den `--run-unittests` parameter.</span><span class="sxs-lookup"><span data-stu-id="a78f9-112">If you want to build and run the unit tests, add the `--run-unittests` parameter.</span></span> <span data-ttu-id="a78f9-113">Om du vill skapa och köra testerna slutpunkt till slutpunkt, lägga till den `--run-e2e-tests`.</span><span class="sxs-lookup"><span data-stu-id="a78f9-113">If you want to build and run the end to end tests, add the `--run-e2e-tests`.</span></span>

> [!NOTE]
> <span data-ttu-id="a78f9-114">Varje gång du kör skriptet **build.sh** tar det bort och återskapar sedan mappen **build** i rotmappen på den lokala kopian av databasen **iot-edge**.</span><span class="sxs-lookup"><span data-stu-id="a78f9-114">Every time you run the **build.sh** script, it deletes and then recreates the **build** folder in the root folder of your local copy of the **iot-edge** repository.</span></span>