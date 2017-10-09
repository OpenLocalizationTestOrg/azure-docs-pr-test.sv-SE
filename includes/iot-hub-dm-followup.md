## <a name="customize-and-extend-hello-device-management-actions"></a>Anpassa och utöka hello hanteringsåtgärder för enhet

IoT-lösningar kan utöka hello definierad uppsättning device management mönster eller aktivera anpassade mönster med hjälp av hello enheten dubbla och moln till enhet metoden primitiver. Andra exempel på enheten hanteringsåtgärder är fabriksåterställning, firmware-uppdatering, programuppdatering, energisparfunktioner, nätverk och anslutning hantering och kryptering av data.

## <a name="device-maintenance-windows"></a>Underhållsperioder för enhet

Normalt kan du konfigurera enheter tooperform åtgärder vid en tidpunkt som minimerar avbrott och driftstopp. Enheten underhållsfönster är vanliga mönstret toodefine hello tid när en enhet bör uppdatera dess konfiguration. Dina backend-lösningar kan använda hello önskad egenskaperna för hello enheten dubbla toodefine och aktivera en princip på enheten som möjliggör en underhållsperiod. När en enhet tar emot hello principunderhåll fönster, kan den använda hello rapporterade-egenskapen för hello dubbla tooreport hello Enhetsstatus hello princip. hello backend-app kan sedan använda enheten dubbla frågor tooattest toocompliance av enheter och varje princip.

## <a name="next-steps"></a>Nästa steg

I den här kursen används en direkta metoden tootrigger remote omstart på en enhet. Du använt hello rapporterade egenskaper tooreport hello senast omstart tid från hello enhet och efterfrågade hello enheten dubbla toodiscover hello omstart senaste tidpunkten för hello enheten från hello molnet.

komma igång med IoT-hubb och device management mönster, till exempel remote över hello luften firmware-uppdatering toocontinue finns:

[Självstudier: Hur toodo en inbyggd programvara uppdatera][lnk-fwupdate]

toolearn hur tooextend din IoT-lösningen och schema metodanrop på flera enheter, finns i hello [schema och broadcast jobb] [ lnk-tutorial-jobs] kursen.

komma igång med IoT-hubb toocontinue finns [komma igång med IoT kant][lnk-iot-edge].

[lnk-fwupdate]: ../articles/iot-hub/iot-hub-node-node-firmware-update.md
[lnk-tutorial-jobs]: ../articles/iot-hub/iot-hub-node-node-schedule-jobs.md
[lnk-iot-edge]: ../articles/iot-hub/iot-hub-linux-iot-edge-get-started.md