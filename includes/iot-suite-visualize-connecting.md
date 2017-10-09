## <a name="view-device-telemetry-in-hello-dashboard"></a>Visa enhetstelemetrin hello instrumentpanelen
hello instrumentpanelen i hello fjärråtkomst övervakning lösning aktiverar du tooview hello telemetri dina enheter skicka tooIoT hubb.

1. I webbläsaren returnerade toohello remote lösning instrumentpanelen för övervakning, klickar du på **enheter** i hello vänstra panelen toonavigate toohello **enhetslistan**.
2. I hello **enhetslistan**, bör du se att hello statusen för din enhet är **kör**. Om inte, klickar du på **Aktivera enhet** i hello **enhetsinformation** panelen.
   
    ![Visa enhetsstatus][18]
3. Klicka på **instrumentpanelen** tooreturn toohello instrumentpanelen, Välj din enhet i hello **enhet tooView** nedrullningsbara tooview dess telemetri. hello telemetri från hello exempelprogram är 50 enheter för inre temperatur, 55 enheter för externa temperatur och 50 enheter för fuktighet.
   
    ![Visa enhetstelemetri][img-telemetry]

## <a name="invoke-a-method-on-your-device"></a>Anropa en metod på enheten
hello instrumentpanelen i hello remote övervakningslösning kan tooinvoke metoder på dina enheter via IoT-hubb. Du kan till exempel anropa en metod toosimulate startas om en enhet i hello remote övervakningslösning.

1. Hello remote lösning instrumentpanelen för övervakning, klicka på **enheter** i hello vänstra panelen toonavigate toohello **enhetslistan**.
2. Klicka på **enhets-ID** för din enhet i hello **enhetslistan**.
3. I hello **enhetsinformation** klickar du på **metoder**.
   
    ![Enhetsmetoder][13]
4. I hello **metoden** listrutan, Välj **InitiateFirmwareUpdate**, och klicka sedan på **FWPACKAGEURI** ange en URL för dummy. Klicka på **anropa metoden** toocall hello-metoden på hello enhet.
   
    ![Anropa en enhetsmetod][14]
   

5. Du ser ett meddelande i hello-konsolen körs din kod för enheten när hello enheten hanterar hello-metoden. hello resultaten av hello metoden läggs toohello historik i hello lösning portal:

    ![Visa metodhistorik][img-method-history]

## <a name="next-steps"></a>Nästa steg
hello artikel [anpassa förkonfigurerade lösningar] [ lnk-customize] beskrivs några sätt som du kan utöka det här exemplet. Möjliga tillägg kan vara att använda riktiga sensorer och att implementera ytterligare kommandon.

Du kan lära dig mer om hello [behörigheter på hello azureiotsuite.com platsen][lnk-permissions].

[13]: ./media/iot-suite-visualize-connecting/suite4.png
[14]: ./media/iot-suite-visualize-connecting/suite7-1.png
[18]: ./media/iot-suite-visualize-connecting/suite10.png
[img-telemetry]: ./media/iot-suite-visualize-connecting/telemetry.png
[img-method-history]: ./media/iot-suite-visualize-connecting/history.png
[lnk-customize]: ../articles/iot-suite/iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-permissions]: ../articles/iot-suite/iot-suite-permissions.md
