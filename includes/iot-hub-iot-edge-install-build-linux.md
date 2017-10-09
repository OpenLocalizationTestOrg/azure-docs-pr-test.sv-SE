## <a name="install-hello-prerequisites"></a>Installera hello förutsättningar

hello stegen i den här självstudiekursen förutsätter att du kör Ubuntu Linux.

Öppna ett gränssnitt och kör följande kommandon tooinstall hello nödvändiga paketen hello:

```bash
sudo apt-get update
sudo apt-get install curl build-essential libcurl4-openssl-dev git cmake libssl-dev uuid-dev valgrind libglib2.0-dev libtool autoconf
```

Hello shell, kör hello efter kommandot tooclone hello Azure IoT kant GitHub-lagringsplatsen tooyour lokal dator:

```bash
git clone https://github.com/Azure/iot-edge.git
```

## <a name="how-toobuild-hello-sample"></a>Hur toobuild hello exempel

Du kan nu skapa hello IoT kant runtime och exempel på den lokala datorn:

1. Öppna ett gränssnitt.

1. Navigera toohello rotmappen i den lokala kopian av hello **iot-edge** databasen.

1. Kör skriptet bygga på följande sätt:

    ```sh
    tools/build.sh --disable-native-remote-modules
    ```

Det här skriptet använder den **cmake** verktyget toocreate en mapp som kallas **skapa** i hello rotmappen på den lokala kopian av den **iot-edge** databasen och generera en make-filen. hello skript och skapar sedan hello-lösning, hoppar över kontroller och end tooend test. Om du vill toobuild och köra hello kontroller, lägger du till hello `--run-unittests` parameter. Om du vill toobuild och köra hello slutet tooend tester, lägger du till hello `--run-e2e-tests`.

> [!NOTE]
> Varje gång du kör hello **build.sh** skript, tas bort och återskapar hello **skapa** mapp i hello rotmappen på den lokala kopian av hello **iot-edge** databasen.
