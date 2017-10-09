## <a name="install-hello-prerequisites"></a>Installera hello förutsättningar

1. Installera [Visual Studio 2015 eller 2017](https://www.visualstudio.com). Du kan använda hello kostnadsfri Community Edition om du uppfyller hello licenskraven. Vara säker på att tooinclude Visual C++ och NuGet Package Manager.

1. Installera [git](http://www.git-scm.com) och kontrollera att du kan köra git.exe från hello-kommandoraden.

1. Installera [CMake](https://cmake.org/download/) och kontrollera att du kan köra cmake.exe från hello-kommandoraden. CMake version 3.7.2 eller senare rekommenderas. Hej **.msi** installer är hello enklaste alternativet i Windows. Lägg till CMake toohello SÖKVÄGEN för minst hello aktuell användare när du ombeds hello installer.

1. Installera [Python 2.7](https://www.python.org/downloads/release/python-27). Kontrollera att du lägger till Python tooyour `PATH` miljövariabeln i **Kontrollpanelen -> System -> Avancerat systeminställningar -> miljövariabler**.

1. Kör hello efter kommandot tooclone hello Azure IoT kant GitHub-lagringsplatsen tooyour lokal dator i en kommandotolk:

    ```cmd
    git clone https://github.com/Azure/iot-edge.git
    ```

## <a name="how-toobuild-hello-sample"></a>Hur toobuild hello exempel

Du kan nu skapa hello IoT kant runtime och exempel på den lokala datorn:

1. Öppna en **Developer kommandotolk för VS 2015** eller **Developer kommandotolk för VS 2017** kommandotolk.

1. Navigera toohello rotmappen i den lokala kopian av hello **iot-edge** databasen.

1. Kör skriptet bygga på följande sätt:

    ```cmd
    tools\build.cmd --disable-native-remote-modules
    ```

Det här skriptet skapar en fil för Visual Studio-lösningen och bygger hello lösning. Du hittar hello Visual Studio-lösning i hello **skapa** mapp i den lokala kopian av hello **iot-edge** databasen. Om du vill toobuild och köra hello kontroller, lägger du till hello `--run-unittests` parameter. Om du vill toobuild och köra hello slutet tooend tester, lägger du till hello `--run-e2e-tests`.

> [!NOTE]
> Varje gång du kör hello **build.cmd** skript, tas bort och återskapar hello **skapa** mapp i hello rotmappen på den lokala kopian av hello **iot-edge** databasen.
