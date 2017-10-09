## <a name="view-hello-telemetry"></a>Visa hello telemetri

hello hallon Pi nu skicka telemetri toohello remote övervakningslösning. Du kan visa hello telemetri på instrumentpanelen för hello-lösning. Du kan också skicka meddelanden tooyour hallon Pi hello lösning instrumentpanel.

- Navigera toohello lösning instrumentpanelen.
- Välj din enhet i hello **enhet tooView** listrutan.
- hello telemetri från hello hallon Pi visar hello instrumentpanelen.

![Visa telemetri från hello hallon Pi][img-telemetry-display]

## <a name="initiate-hello-firmware-update"></a>Initiera hello firmware-uppdatering

hello firmware-uppdateringen hämtas och installeras en uppdaterad version av klientprogrammet för hello enheten på hello hallon Pi. Mer information om uppdateringen för hello inbyggd programvara finns hello beskrivningen av mönstret av inbyggd programvara för hello i [översikt över hantering av enheter med IoT-hubben][lnk-update-pattern].

Du startar hello firmware-uppdateringen genom att anropa en metod i hello enhet. Den här metoden är asynkron och returnerar så snart hello uppdateringen påbörjas. hello enheten använder rapporterade egenskaper toonotify hello lösning om hello fortskrider hello uppdateringen.

Du kan anropa metoder på din hallon Pi hello lösning instrumentpanel. När hello hallon Pi först ansluter toohello remote övervakningslösning, skickar information om hello-metoder stöder. 

[img-telemetry-display]: media/iot-suite-raspberry-pi-kit-view-telemetry-advanced/telemetry.png
[lnk-update-pattern]: ../articles/iot-hub/iot-hub-device-management-overview.md
