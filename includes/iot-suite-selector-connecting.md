> [!div class="op_single_selector"]
> * [C i Windows](../articles/iot-suite/iot-suite-connecting-devices.md)
> * [C i Linux](../articles/iot-suite/iot-suite-connecting-devices-linux.md)
> * [Node.js](../articles/iot-suite/iot-suite-connecting-devices-node.md)
> 
> 

## <a name="scenario-overview"></a>Scenarioöversikt
I det här scenariot skapar du en enhet som skickar hello följande telemetri toohello fjärrövervaknings [förkonfigurerade lösningen][lnk-what-are-preconfig-solutions]:

* Extern temperatur
* Intern temperatur
* Fuktighet

För enkelhetens skull hello koden på hello enhet genererar exempelvärden, men vi rekommenderar att du tooextend hello exemplet genom att ansluta verkliga sensorer tooyour enheten och skicka verkliga telemetri.

hello-enheten är också kan toorespond toomethods anropas från hello lösning instrumentpanelen och önskade egenskapsvärden i hello lösning instrumentpanelen.

toocomplete den här självstudiekursen kommer du behöver ett Azure-konto. Om du inte har något konto kan skapa du ett kostnadsfritt utvärderingskonto på bara några minuter. Mer information finns i [kostnadsfri utvärderingsversion av Azure][lnk-free-trial].

## <a name="before-you-start"></a>Innan du börjar
Innan du kan skriva kod för enheten måste du etablera din förkonfigurerade lösning för fjärrövervakning och etablera en ny anpassad enhet i lösningen.

### <a name="provision-your-remote-monitoring-preconfigured-solution"></a>Etablera din förkonfigurerade lösning för fjärrövervakning
hello-enhet som du skapar i den här självstudiekursen skickar data tooan instans av hello [fjärrövervaknings] [ lnk-remote-monitoring] förkonfigurerade lösningen. Om du inte redan har etablerats hello fjärråtkomst övervakning förkonfigurerade lösning i ditt Azure-konto, använder du hello följande steg:

1. På hello <https://www.azureiotsuite.com/> klickar du på  **+**  toocreate en lösning.
2. Klicka på **Välj** på hello **fjärrövervaknings** panelen toocreate din lösning.
3. På hello **skapa Remote övervakningslösning** anger en **lösningsnamn** du själv väljer, Välj hello **Region** toodeploy till, och markera hello Azure prenumerationen toowant toouse. Klicka på **Skapa lösning**.
4. Vänta tills hello etableringsprocessen har slutförts.

> [!WARNING]
> hello förkonfigurerade lösningar använder fakturerbar Azure-tjänster. Glöm inte tooremove hello förkonfigurerade lösningen från prenumerationen när du är klar med den tooavoid eventuella onödiga kostnader. Du kan helt ta bort en förkonfigurerade lösning från prenumerationen genom att besöka hello <https://www.azureiotsuite.com/> sidan.
> 
> 

När hello etableringsprocessen för hello remote övervakningslösning är klar klickar du på **starta** tooopen hello lösning instrumentpanel i din webbläsare.

![Instrumentpanel för lösningen][img-dashboard]

### <a name="provision-your-device-in-hello-remote-monitoring-solution"></a>Etablera din enhet i hello remote övervakningslösning
> [!NOTE]
> Om du redan har etablerat en enhet i din lösning kan du hoppa över det här steget. När du skapar hello klientprogrammet behöver du tooknow hello enheten autentiseringsuppgifter.
> 
> 

För en enhet tooconnect toohello förkonfigurerade lösning, den måste identifiera sig själv tooIoT hubb med giltiga autentiseringsuppgifter. Du kan hämta hello enheten autentiseringsuppgifter från hello lösning instrumentpanelen. Du inkludera hello enheten autentiseringsuppgifter i ditt klientprogram senare i den här kursen.

tooadd en enhet tooyour remote övervakningslösning fullständig hello följa stegen i hello lösning instrumentpanelen:

1. I hello nedre vänstra hörnet av hello instrumentpanelen, klickar du på **lägger till en enhet**.
   
   ![Lägg till en enhet][1]
2. I hello **anpassad enhet** klickar du på **Lägg till ny**.
   
   ![Lägg till en anpassad enhet][2]
3. Välj **Låt mig ange mitt eget enhets-ID**. Ange ett enhets-ID som **mydevice**, klickar du på **kontrollera ID** tooverify namnet inte redan används och klicka sedan på **skapa** tooprovision hello enhet.
   
   ![Lägg till enhets-ID][3]
4. Gör en anteckning hello enheten autentiseringsuppgifter (enhets-ID, IoT Hub-värdnamnet och nyckeln enheten). Klientprogrammet måste dessa värden tooconnect toohello remote övervakningslösning. Klicka sedan på **Klar**.
   
    ![Visa enhetsautentiseringsuppgifter][4]
5. Välj din enhet i listan över enheter hello hello lösning instrumentpanelen. Sedan hello **enhetsinformation** klickar du på **Aktivera enhet**. hello statusen för din enhet är nu **kör**. hello remote övervakningslösning kan nu ta emot telemetri från enheten och anropa metoder i hello enhet.

[img-dashboard]: ./media/iot-suite-selector-connecting/dashboard.png
[1]: ./media/iot-suite-selector-connecting/suite0.png
[2]: ./media/iot-suite-selector-connecting/suite1.png
[3]: ./media/iot-suite-selector-connecting/suite2.png
[4]: ./media/iot-suite-selector-connecting/suite3.png

[lnk-what-are-preconfig-solutions]: ../articles/iot-suite/iot-suite-what-are-preconfigured-solutions.md
[lnk-remote-monitoring]: ../articles/iot-suite/iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/