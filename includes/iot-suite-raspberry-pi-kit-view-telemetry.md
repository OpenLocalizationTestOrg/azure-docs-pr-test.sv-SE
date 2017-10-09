## <a name="view-hello-telemetry"></a>Visa hello telemetri

hello hallon Pi nu skicka telemetri toohello remote övervakningslösning. Du kan visa hello telemetri på instrumentpanelen för hello-lösning. Du kan också skicka meddelanden tooyour hallon Pi hello lösning instrumentpanel.

- Navigera toohello lösning instrumentpanelen.
- Välj din enhet i hello **enhet tooView** listrutan.
- hello telemetri från hello hallon Pi visar hello instrumentpanelen.

![Visa telemetri från hello hallon Pi][img-telemetry-display]

## <a name="act-on-hello-device"></a>Fungerar på hello-enhet

Du kan anropa metoder på din hallon Pi hello lösning instrumentpanel. När hello hallon Pi ansluter toohello remote övervakningslösning, skickar information om hello-metoder stöder.

- I hello lösning instrumentpanelen, klickar du på **enheter** toovisit hello **enheter** sidan. Välj din hallon Pi i hello **enhetslistan**. Välj **metoder**:

    ![Lista över enheter i instrumentpanelen][img-list-devices]

- På hello **anropa metoden** väljer **LightBlink** i hello **metoden** listrutan.

- Välj **InvokeMethod**. hello Indikator ansluten toohello hallon Pi blinkar flera gånger. hello-appen på hello hallon Pi skickar en bekräftelse tillbaka toohello lösning instrumentpanel:

    ![Visa historiken för metoden][img-method-history]

- Du kan växla hello Indikator och inaktivera med hello **ChangeLightStatus** metod med en **LightStatusValue** ställa in också**1** för på eller **0** för av.

> [!WARNING]
> Om du lämnar hello remote övervakningslösning som körs i ditt Azure-konto, debiteras du för hello gång den körs. Läs mer om att minskas när hello fjärråtkomst övervakning lösningen körs [konfigurerar Azure IoT Suite förkonfigurerade lösningar för demonstration][lnk-demo-config]. Ta bort hello förkonfigurerade lösningen från ditt Azure-konto när du är klar.


[img-telemetry-display]: media/iot-suite-raspberry-pi-kit-view-telemetry/telemetry.png
[img-list-devices]: media/iot-suite-raspberry-pi-kit-view-telemetry/listdevices.png
[img-method-history]: media/iot-suite-raspberry-pi-kit-view-telemetry/methodhistory.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md