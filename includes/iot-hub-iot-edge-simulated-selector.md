> [!div class="op_single_selector"]
> * [Linux](../articles/iot-hub/iot-hub-linux-iot-edge-simulated-device.md)
> * [Windows](../articles/iot-hub/iot-hub-windows-iot-edge-simulated-device.md)

Den här genomgången av den [simulerade enheten molnet överför exempel] visar hur du använder [Azure IoT kant] [ lnk-sdk] skicka enhet till moln telemetri till IoT-hubb från simulerade enheter .

Den här genomgången omfattar:

* **Arkitektur för**: arkitektur information om den [simulerade enheten molnet överför exempel].
* **Skapa och kör**: stegen som krävs för att bygga och köra exemplet.

## <a name="architecture"></a>Arkitektur

Den [simulerade enheten molnet överför exempel] visar hur du skapar en gateway som skickar telemetri från simulerade enheter till en IoT-hubb. En enhet kanske inte kan ansluta direkt till IoT-hubb eftersom enheten:

* Använder inte ett kommunikationsprotokoll som tolkas av IoT-hubb.
* Är inte smart komma ihåg identiteten som tilldelats av IoT-hubb.

En IoT-gräns-gatewayen kan lösa dessa problem på följande sätt:

* Gatewayen förstår protokollet som används av enheten tar emot enhet till moln telemetri från enheten och vidarebefordrar meddelandena till IoT-hubb med ett protokoll som tolkas av IoT-hubben.

* Gatewayen mappar IoT-hubb identiteter till enheter och fungerar som proxy när en enhet skickar meddelanden till IoT-hubb.

Följande diagram visar huvudkomponenterna i exemplet, inklusive IoT kant-moduler:

![][1]

Moduler skickar inte meddelanden direkt till varandra. Moduler publicera meddelanden i ett internt Service broker som levererar meddelanden till de moduler som använder en mekanism för prenumerationen. Mer information finns i [Kom igång med Azure IoT kant][lnk-gw-getstarted].

### <a name="protocol-ingestion-module"></a>Modul för protokollinhämtning

Denna modul är startpunkten för att ta emot data från enheter, via gatewayen och i molnet. I det här exemplet modulen:

1. Skapar simulerade temperatur data. Om du använder fysiska enheter läser modulen data från de fysiska enheterna.
1. Skapar ett meddelande.
1. Placerar simulerade temperatur-data i innehållet i meddelandet.
1. Lägger till en egenskap med en MAC-adress som falska meddelandet.
1. Gör meddelandet tillgängligt till nästa modul i kedjan.

Modulen kallas **protokollet X införandet** i föregående diagram kallas **simulerade enheten** i källkoden.

### <a name="mac-lt-gt-iot-hub-id-module"></a>MAC &lt;-&gt; IoT Hub ID-modul

Den här modulen söker efter meddelanden som har en egenskap för Mac-adress. I det här exemplet protokollet införandet modulen lägger du till egenskapen MAC-adressen. Om modulen hittar sådan egenskap, den lägger till en annan egenskap med en IoT-hubb enhet för meddelandet. Modulen sedan gör meddelandet tillgängligt till nästa modul i kedjan.

Utvecklaren anger en mappning mellan MAC-adresser och IoT-hubb identiteter att associera simulerade enheterna med IoT-hubb enheten identiteter. Utvecklaren lägger till mappningen manuellt som en del av modulkonfigurationen.

> [!NOTE]
> I det här exemplet används en MAC-adress som en unik enhetsidentifierare och kopplar den till en IoT Hub-enhetsidentitet. Du kan emellertid skriva din egen modul som använder en annan unik identifierare. Till exempel kan dina enheter har unika serienummer eller telemetridata kan innehålla ett unikt inbäddade enhetsnamn.

### <a name="iot-hub-communication-module"></a>IoT Hub-kommunikationsmodul

Den här modulen tar meddelanden med en IoT-hubb nyckelegenskapen för enheter som har tilldelats av den föregående modulen. Modulen skickar meddelandet till IoT-hubb med hjälp av HTTP-protokollet. HTTP är ett av de tre protokollen som kan tolkas av IoT Hub.

I stället för att öppna en anslutning för varje simulerad enhet öppnar den här modulen en HTTP-anslutning från gateway för IoT-hubben. Modulen multiplexes sedan anslutningar från alla simulerade enheter över den anslutningen. På så sätt kan en enda gateway för att ansluta många fler enheter.

## <a name="before-you-get-started"></a>Innan du börjar

Innan du börjar måste du:

* [Skapa en IoT-hubb] [ lnk-create-hub] i din Azure-prenumeration, måste namnet på din hubb för att slutföra den här genomgången. Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.
* Lägga till två enheter till din IoT-hubb och anteckna deras ID och nycklar för enheten. Du kan använda den [enheten explorer] [ lnk-device-explorer] eller [iothub explorer] [ lnk-iothub-explorer] verktyg för att lägga till dina enheter till IoT-hubb som du skapade i föregående steg och hämta sina nycklar.

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