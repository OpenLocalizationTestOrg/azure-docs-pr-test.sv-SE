> [!div class="op_single_selector"]
> * [Linux](../articles/iot-hub/iot-hub-linux-iot-edge-get-started.md)
> * [Windows](../articles/iot-hub/iot-hub-windows-iot-edge-get-started.md)
> 
> 

Den här artikeln innehåller en detaljerad genomgång av hello [Hello World exempelkod] [ lnk-helloworld-sample] tooillustrate hello grundläggande komponenter i hello [Azure IoT kant] [ lnk-iot-edge] arkitektur. hello används hello Azure IoT kant toobuild en enkel gateway som loggar tooa för ”hello world” meddelandefilen var femte sekund.

Den här genomgången omfattar:

* **Hello World exempelarkitektur**: Beskriver hur [Azure IoT kant arkitektur begrepp] [ lnk-edge-concepts] tillämpa toohello Hello World-exempel och hur hello komponenter fungerar ihop.
* **Hur toobuild hello exempel**: hello steg krävs toobuild hello exempel.
* **Hur toorun hello exempel**: hello steg krävs toorun hello exempel. 
* **Vanliga utdata**: ett exempel på hello utdata tooexpect när du kör hello exempel.
* **Kodfragment**: en samling av koden kodavsnitt tooshow hur hello Hello World-exempel implementerar nyckeln IoT Edge gateway komponenter.


## <a name="hello-world-sample-architecture"></a>Arkitektur för Hello World-exempel
hello Hello World-exempel illustrerar hello begrepp som beskrivs i föregående avsnitt i hello. hello Hello World-exempel implementerar en IoT-gräns-gatewayen som har en pipeline som består av två IoT kant moduler:

* Hej *hello world* modulen och skapar ett meddelande var femte sekund och skickar den toohello loggaren modulen.
* Hej *loggaren* modulen skrivningar hello meddelanden som tas emot tooa fil.

![Arkitekturen i Hello World-exemplet som skapats med Azure IoT Edge][4]

Enligt beskrivningen i föregående avsnitt i hello meddelanden hello Hello World modulen inte klarar direkt toohello loggaren modulen var femte sekund. I stället publicerar den förhandlare toohello meddelande var femte sekund.

hello loggaren modulen tar emot hello-meddelande från hello broker och agerar på, skriver hello innehållet i hello meddelandefilen tooa.

hello loggaren modulen förbrukar endast meddelanden från hello broker, den publicerar aldrig nya meddelanden toohello broker.

![Hur hello broker skickar meddelanden mellan moduler i Azure IoT kant][5]

hello ovanstående bild visar hello och arkitekturen för hello Hello World-exempel hello relativa sökvägar toohello källfiler som implementerar olika delar av hello exemplet i hello [databasen][lnk-iot-edge]. Utforska hello koden på egen hand eller Använd hello kodavsnitten nedan som vägledning.

<!-- Images -->
[4]: media/iot-hub-iot-edge-getstarted-selector/high_level_architecture.png
[5]: media/iot-hub-iot-edge-getstarted-selector/detailed_architecture.png

<!-- Links -->
[lnk-helloworld-sample]: https://github.com/Azure/iot-edge/tree/master/samples/hello_world
[lnk-iot-edge]: https://github.com/Azure/iot-edge
[lnk-edge-concepts]: ../articles/iot-hub/iot-hub-iot-edge-overview.md