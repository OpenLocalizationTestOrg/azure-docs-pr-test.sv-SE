> [!div class="op_single_selector"]
> * [Linux](../articles/iot-hub/iot-hub-linux-iot-edge-simulated-device.md)
> * [Windows](../articles/iot-hub/iot-hub-windows-iot-edge-simulated-device.md)

Den här genomgången av hello [simulerade enheten molnet överför exempel] visas hur toouse [Azure IoT kant] [ lnk-sdk] toosend enhet till moln telemetri tooIoT hubb från simulerade enheter.

Den här genomgången omfattar:

* **Arkitektur för**: arkitektur information om hello [simulerade enheten molnet överför exempel].
* **Skapa och köra**: hello steg krävs toobuild och kör hello exempel.

## <a name="architecture"></a>Arkitektur

Hej [simulerade enheten molnet överför exempel] visar hur toocreate en gateway som skickar telemetri från simulerade enheter tooan IoT-hubb. En enhet kanske inte kan tooconnect direkt tooIoT hubb eftersom hello enhet:

* Använder inte ett kommunikationsprotokoll som tolkas av IoT-hubb.
* Är inte tillräckligt smart tooremember hello identitet som tilldelades tooit i IoT Hub.

En IoT-gräns-gatewayen kan lösa dessa problem i hello följande sätt:

* hello gateway förstår hello-protokoll som används av hello enheten tar emot enhet till moln telemetri från hello enhet och vidarebefordrar dessa meddelanden tooIoT hubb med ett protokoll som tolkas av hello IoT-hubb.

* hello gateway mappar IoT-hubb identiteter toodevices och fungerar som proxy när en enhet skickar meddelanden tooIoT hubb.

hello följande diagram visar hello huvudkomponenter hello exemplet, inklusive hello IoT kant moduler:

![][1]

hello moduler inte klarar meddelanden direkt tooeach andra. hello moduler publicera meddelanden tooan internt Service broker som levererar hello meddelanden toohello andra moduler med hjälp av en mekanism för prenumerationen. Mer information finns i [Kom igång med Azure IoT kant][lnk-gw-getstarted].

### <a name="protocol-ingestion-module"></a>Modul för protokollinhämtning

Denna modul är hello som utgångspunkt för att ta emot data från enheter, via hello gateway och i hello moln. I exemplet hello hello modulen:

1. Skapar simulerade temperatur data. Om du använder fysiska enheter läser hello modulen data från de fysiska enheterna.
1. Skapar ett meddelande.
1. Placerar hello simulerade temperatur data i hello meddelandeinnehåll.
1. Lägger till en egenskap med ett falska toohello meddelande för MAC-adress.
1. Gör hello meddelandet tillgängliga toohello nästa modul i hello kedja.

hello modulen kallas **protokollet X införandet** i hello föregående diagram kallas **simulerade enheten** i hello källkod.

### <a name="mac-lt-gt-iot-hub-id-module"></a>MAC &lt;-&gt; IoT Hub ID-modul

Den här modulen söker efter meddelanden som har en egenskap för Mac-adress. I exemplet hello lägger hello protokollet införandet modulen hello MAC-Adressegenskapen. Om modulen hello hittar sådan egenskap, läggs en annan egenskap med ett IoT-hubb enheten viktiga toohello meddelande. hello modulen gör sedan hello meddelandet tillgängliga toohello nästa modul i hello kedja.

hello developer ställer in en mappning mellan MAC-adresser och IoT-hubb identiteter tooassociate hello simulerade enheter med IoT-hubb enheten identiteter. hello utvecklare lägger till hello mappning manuellt som en del av hello konfiguration.

> [!NOTE]
> I det här exemplet används en MAC-adress som en unik enhetsidentifierare och kopplar den till en IoT Hub-enhetsidentitet. Du kan emellertid skriva din egen modul som använder en annan unik identifierare. Till exempel dina enheter eller kan ha unika serienummer hello telemetridata kan innehålla ett unikt inbäddade enhetsnamn.

### <a name="iot-hub-communication-module"></a>IoT Hub-kommunikationsmodul

Den här modulen tar meddelanden med en IoT-hubb nyckelegenskapen för enheter som har tilldelats av föregående hello-modulen. hello modulen skickar hälsningsmeddelande innehåll tooIoT hubb med hello HTTP-protokollet. HTTP är en av hello tre protokoll tolkas av IoT-hubb.

I stället för att öppna en anslutning för varje simulerad enhet öppnar den här modulen en enskild HTTP-anslutning från hello gateway toohello IoT-hubb. hello modulen multiplexes sedan anslutningar från alla hello simulerade enheter över den anslutningen. På så sätt kan en enda gateway-tooconnect många fler enheter.

## <a name="before-you-get-started"></a>Innan du börjar

Innan du börjar måste du:

* [Skapa en IoT-hubb] [ lnk-create-hub] i din Azure-prenumeration behöver du hello namnet på din hubb toocomplete den här genomgången. Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.
* Lägg till två enheter tooyour IoT-hubb och anteckna deras ID och nycklar för enheten. Du kan använda hello [enheten explorer] [ lnk-device-explorer] eller [iothub explorer] [ lnk-iothub-explorer] verktyget tooadd din enheter toohello IoT-hubb som du skapade i hello föregående steg och hämta sina nycklar.

![][2]

<!-- Images -->
[1]: media/iot-hub-iot-edge-simulated-selector/image1.png
[2]: media/iot-hub-iot-edge-simulated-selector/image2.png

<!-- Links -->
[simulerade enheten molnet överför exempel]: https://github.com/Azure/iot-edge/blob/master/samples/simulated_device_cloud_upload/README.md
[lnk-sdk]: https://github.com/Azure/iot-edge
[lnk-gw-getstarted]: ../articles/iot-hub/iot-hub-linux-iot-edge-get-started.md
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer
[lnk-iothub-explorer]: https://github.com/Azure/iothub-explorer/blob/master/readme.md
[lnk-create-hub]: ../articles/iot-hub/iot-hub-create-through-portal.md