## <a name="configure-hello-nodejs-simulated-device"></a>Konfigurera hello Node.js simulerade enhet
1. Klicka på hello remote instrumentpanelen för övervakning **+ Lägg till en enhet** och Lägg sedan till en *anpassade*. Anteckna hello IoT-hubb värdnamn, enhets-id och nyckeln för enheten. Behöver du dem senare i den här självstudiekursen när du förbereder hello remote_monitoring.js enheten klientprogrammet.
2. Se till att Node.js-version 0.12.x eller senare är installerat på utvecklingsdatorn. Kör `node --version` i Kommandotolken eller i en shell toocheck hello-version. Information om hur du använder en package manager tooinstall Node.js på Linux finns [installera Node.js via Pakethanteraren][node-linux].
3. När du har installerat Node.js klona hello senaste versionen av hello [azure iot-sdk-nod] [ lnk-github-repo] databasen tooyour utvecklingsdator. Använd alltid hello **master** grenen för hello senaste versionen av hello bibliotek och exempel.
4. Från den lokala kopian av hello [azure iot-sdk-nod] [ lnk-github-repo] databasen, kopiera hello följande två filer från hello nod-enhet-exempel mappen tooan tom mapp på utvecklingsdatorn:
   
   * Packages.JSON
   * remote_monitoring.js
5. Öppna hello remote_monitoring.js filen och leta efter hello som följer variabeldefinitionen:
   
    ```
    var connectionString = "[IoT Hub device connection string]";
    ```
6. Ersätt **[anslutningssträngen för IoT-hubb enheten]** med anslutningssträngen enhet. Använda hello värden för din IoT-hubb värdnamn, enhets-id och nyckeln för enheten som du antecknade i steg 1. En anslutningssträng för enheten har hello följande format:
   
    ```
    HostName={your IoT Hub hostname};DeviceId={your device id};SharedAccessKey={your device key}
    ```
   
    Om din IoT Hub-värdnamnet är **contoso** och enhets-id är **mydevice**, anslutningssträngen ser ut som följande fragment hello:
   
    ```
    var connectionString = "HostName=contoso.azure-devices.net;DeviceId=mydevice;SharedAccessKey=2s ... =="
    ```
7. Spara hello-filen. Kör följande kommandon i ett gränssnitt eller en kommandotolk i hello-mapp som innehåller filer tooinstall hello nödvändiga paketen hello och kör sedan hello exempelprogrammet:
   
    ```
    npm install
    node remote_monitoring.js
    ```

## <a name="observe-dynamic-telemetry-in-action"></a>Se dynamiska telemetri i praktiken
hello instrumentpanelen visar hello temperatur- och fuktighetskonsekvens telemetri från hello befintliga simulerade enheter:

![hello standardinstrumentpanelen][image1]

Om du väljer hello Node.js simulerade enhet du körde hello föregående avsnitt visas temperatur, fuktighet och externa temperatur telemetri:

![Lägga till externa temperatur toohello instrumentpanel][image2]

hello remote övervakningslösning identifierar hello ytterligare externa temperatur telemetri typ och lägger till den toohello diagram på instrumentpanelen för hello automatiskt.

[node-linux]: https://github.com/nodejs/node-v0.x-archive/wiki/Installing-Node.js-via-package-manager
[lnk-github-repo]: https://github.com/Azure/azure-iot-sdk-node
[image1]: media/iot-suite-send-external-temperature/image1.png
[image2]: media/iot-suite-send-external-temperature/image2.png