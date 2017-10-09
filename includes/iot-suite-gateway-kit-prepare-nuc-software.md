## <a name="build-iot-edge"></a>Skapa IoT kant

Den här kursen använder anpassade IoT kant moduler toocommunicate med hello remote förkonfigurerade övervakningslösning. Du måste därför toobuild hello IoT kant moduler från anpassade källkoden. hello följande avsnitt beskrivs hur tooinstall IoT kant och bygga hello anpassad IoT kant-modul.

### <a name="install-iot-edge"></a>Installera IoT kant

hello följande steg beskriver hur tooinstall hello före kompileras IoT kant programvara på hello Intel NUC:

1. Konfigurera hello krävs smart paketet databaser genom att köra följande kommandon på hello Intel NUC hello:

    ```bash
    smart channel --add IoT_Cloud type=rpm-md name="IoT_Cloud" baseurl=http://iotdk.intel.com/repos/iot-cloud/wrlinux7/rcpl13/ -y
    smart channel --add WR_Repo type=rpm-md baseurl=https://distro.windriver.com/release/idp-3-xt/public_feeds/WR-IDP-3-XT-Intel-Baytrail-public-repo/RCPL13/corei7_64/
    ```

    Ange `y` när hello kommandot uppmanar dig för**innehåller den här kanalen?**.

1. Uppdatera hello smart Pakethanteraren genom att köra följande kommando hello:

    ```bash
    smart update
    ```

1. Installera hello Azure IoT kant paketet genom att köra följande kommando hello:

    ```bash
    smart config --set rpm-check-signatures=false
    smart install packagegroup-cloud-azure -y
    ```

1. Kontrollera hello installationen körs hello ”Hello world” exempel. Det här exemplet skriver en hello world meddelandet toohello log.txT fil var femte sekund. hello kör följande kommandon hello ”Hello world” exempel:

    ```bash
    cd /usr/share/azureiotgatewaysdk/samples/hello_world/
    ./hello_world hello_world.json
    ```

    Ignorera alla **ogiltigt argument** meddelanden när du stoppar hello exempel.

    Använd hello följande kommando tooview hello innehållet i hello loggfil:

    ```bash
    cat log.txt | more
    ```

### <a name="troubleshooting"></a>Felsökning

Om felmeddelande hello ”erbjuder ingen util-linux-utveckling”, startar du om datorn hello Intel NUC.
