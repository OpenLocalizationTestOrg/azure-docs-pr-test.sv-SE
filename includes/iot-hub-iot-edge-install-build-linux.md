## <a name="install-hello-prerequisites"></a><span data-ttu-id="ec701-101">Installera hello förutsättningar</span><span class="sxs-lookup"><span data-stu-id="ec701-101">Install hello prerequisites</span></span>

<span data-ttu-id="ec701-102">hello stegen i den här självstudiekursen förutsätter att du kör Ubuntu Linux.</span><span class="sxs-lookup"><span data-stu-id="ec701-102">hello steps in this tutorial assume you are running Ubuntu Linux.</span></span>

<span data-ttu-id="ec701-103">Öppna ett gränssnitt och kör följande kommandon tooinstall hello nödvändiga paketen hello:</span><span class="sxs-lookup"><span data-stu-id="ec701-103">Open a shell and run hello following commands tooinstall hello prerequisite packages:</span></span>

```bash
sudo apt-get update
sudo apt-get install curl build-essential libcurl4-openssl-dev git cmake libssl-dev uuid-dev valgrind libglib2.0-dev libtool autoconf
```

<span data-ttu-id="ec701-104">Hello shell, kör hello efter kommandot tooclone hello Azure IoT kant GitHub-lagringsplatsen tooyour lokal dator:</span><span class="sxs-lookup"><span data-stu-id="ec701-104">In hello shell, run hello following command tooclone hello Azure IoT Edge GitHub repository tooyour local machine:</span></span>

```bash
git clone https://github.com/Azure/iot-edge.git
```

## <a name="how-toobuild-hello-sample"></a><span data-ttu-id="ec701-105">Hur toobuild hello exempel</span><span class="sxs-lookup"><span data-stu-id="ec701-105">How toobuild hello sample</span></span>

<span data-ttu-id="ec701-106">Du kan nu skapa hello IoT kant runtime och exempel på den lokala datorn:</span><span class="sxs-lookup"><span data-stu-id="ec701-106">You can now build hello IoT Edge runtime and samples on your local machine:</span></span>

1. <span data-ttu-id="ec701-107">Öppna ett gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="ec701-107">Open a shell.</span></span>

1. <span data-ttu-id="ec701-108">Navigera toohello rotmappen i den lokala kopian av hello **iot-edge** databasen.</span><span class="sxs-lookup"><span data-stu-id="ec701-108">Navigate toohello root folder in your local copy of hello **iot-edge** repository.</span></span>

1. <span data-ttu-id="ec701-109">Kör skriptet bygga på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="ec701-109">Run the build script as follows:</span></span>

    ```sh
    tools/build.sh --disable-native-remote-modules
    ```

<span data-ttu-id="ec701-110">Det här skriptet använder den **cmake** verktyget toocreate en mapp som kallas **skapa** i hello rotmappen på den lokala kopian av den **iot-edge** databasen och generera en make-filen.</span><span class="sxs-lookup"><span data-stu-id="ec701-110">This script uses the **cmake** utility toocreate a folder called **build** in hello root folder of your local copy of the **iot-edge** repository and generate a makefile.</span></span> <span data-ttu-id="ec701-111">hello skript och skapar sedan hello-lösning, hoppar över kontroller och end tooend test.</span><span class="sxs-lookup"><span data-stu-id="ec701-111">hello script then builds hello solution, skipping unit tests and end tooend tests.</span></span> <span data-ttu-id="ec701-112">Om du vill toobuild och köra hello kontroller, lägger du till hello `--run-unittests` parameter.</span><span class="sxs-lookup"><span data-stu-id="ec701-112">If you want toobuild and run hello unit tests, add hello `--run-unittests` parameter.</span></span> <span data-ttu-id="ec701-113">Om du vill toobuild och köra hello slutet tooend tester, lägger du till hello `--run-e2e-tests`.</span><span class="sxs-lookup"><span data-stu-id="ec701-113">If you want toobuild and run hello end tooend tests, add hello `--run-e2e-tests`.</span></span>

> [!NOTE]
> <span data-ttu-id="ec701-114">Varje gång du kör hello **build.sh** skript, tas bort och återskapar hello **skapa** mapp i hello rotmappen på den lokala kopian av hello **iot-edge** databasen.</span><span class="sxs-lookup"><span data-stu-id="ec701-114">Every time you run hello **build.sh** script, it deletes and then recreates hello **build** folder in hello root folder of your local copy of hello **iot-edge** repository.</span></span>
