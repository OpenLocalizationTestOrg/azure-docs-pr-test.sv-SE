## <a name="view-hello-solution-dashboard"></a>Visa hello lösning instrumentpanelen

hello lösning instrumentpanelen kan du toomanage hello distribueras lösning. Du kan till exempel visa telemetri, Lägg till enheter och anropa metoder.

1. När hello etableringen har slutförts och hello panelen för din förkonfigurerade lösning anger **klar**, Välj **starta** tooopen fjärråtkomst övervakning lösning portalen på en ny flik.

    ![Starta hello förkonfigurerade lösningen][img-launch-solution]

1. Som standard visar hello lösning portal hello *instrumentpanelen*. Navigera tooother områden i hello lösning portal med hello menyn hello vänster hello-sidan.

    ![Instrumentpanel för den förkonfigurerade fjärrövervakningslösningen][img-menu]

## <a name="add-a-device"></a>Lägg till en enhet

För en enhet tooconnect toohello förkonfigurerade lösning, den måste identifiera sig själv tooIoT hubb med giltiga autentiseringsuppgifter. Du kan hämta hello enheten autentiseringsuppgifter från hello lösning instrumentpanelen. Du inkludera hello enheten autentiseringsuppgifter i ditt klientprogram senare i den här kursen.

tooadd en enhet tooyour remote övervakningslösning fullständig hello följa stegen i hello lösning instrumentpanelen:

1. I hello nedre vänstra hörnet av hello instrumentpanelen, klickar du på **lägger till en enhet**.

   ![Lägg till en enhet][1]

1. I hello **anpassad enhet** klickar du på **Lägg till ny**.

   ![Lägg till en anpassad enhet][2]

1. Välj **Låt mig ange mitt eget enhets-ID**. Ange ett enhets-ID som **device01**, klickar du på **kontrollera ID** tooverify du inte redan använt hello namn i din lösning och klicka sedan på **skapa** tooprovision hello enhet.

   ![Lägg till enhets-ID][3]

1. Göra en anteckning hello enhet autentiseringsuppgifter (**enhets-ID**, **IoT-hubb Hostname**, och **enhetsnyckel**). Ditt klientprogram på hello Intel NUC måste dessa värden tooconnect toohello remote övervakningslösning. Klicka sedan på **Klar**.

    ![Visa enhetsautentiseringsuppgifter][4]

1. Välj din enhet i listan över enheter hello hello lösning instrumentpanelen. Sedan hello **enhetsinformation** klickar du på **Aktivera enhet**. hello statusen för din enhet är nu **kör**. hello remote övervakningslösning kan nu ta emot telemetri från enheten och anropa metoder i hello enhet.

[img-launch-solution]: media/iot-suite-gateway-kit-view-solution/launch.png
[img-menu]: media/iot-suite-gateway-kit-view-solution/menu.png
[1]: media/iot-suite-gateway-kit-view-solution/suite0.png
[2]: media/iot-suite-gateway-kit-view-solution/suite1.png
[3]: media/iot-suite-gateway-kit-view-solution/suite2.png
[4]: media/iot-suite-gateway-kit-view-solution/suite3.png