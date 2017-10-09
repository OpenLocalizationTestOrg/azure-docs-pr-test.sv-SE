## <a name="create-an-iot-hub"></a>Skapa en IoT Hub
Skapa en IoT-hubb som din simulerade enhet app tooconnect till. hello följande steg visar hur toocomplete detta uppgift med hjälp av hello Azure-portalen.

1. Logga in toohello [Azure-portalen][lnk-portal].
1. I hello Jumpbar klickar du på **ny** > **Sakernas Internet** > **IoT-hubb**.
   
    ![Jumpbar i Azure Portal][1]
1. I hello **IoT-hubb** bladet välj hello konfiguration för din IoT-hubb.
   
    ![IoT Hub-bladet][2]
   
   1. I hello **namn** anger du ett namn för din IoT-hubb. Om hello **namn** är giltig och tillgänglig, visas en grön bock i hello **namn** rutan.
    [!INCLUDE [iot-hub-pii-note-naming-hub](iot-hub-pii-note-naming-hub.md)]
   
   1. Välj en [pris- och skalningsnivå][lnk-pricing]. Den här guiden kräver inte en specifik nivå. Använd hello kostnadsfria F1-nivån för den här kursen.
   1. I **Resursgrupp** skapar du antingen en resursgrupp eller väljer en befintlig. Mer information finns i [resursnamnet grupper toomanage resurserna i Azure][lnk-resource-groups].
   1. I **plats**, Välj hello plats toohost din IoT-hubb. Välj den plats som är närmast dig för den här guiden.
1. När du har valt dina konfigurationsalternativ för IoT Hub, klickar du på **Skapa**.  Det kan ta några minuter för Azure toocreate din IoT-hubb. Du kan övervaka hello förlopp på hello startsidan eller i meddelandepanelen hello toocheck hello status.
   
    ![Status för ny IoT Hub][3]
1. När hello IoT-hubben har skapats, klickar du på hello nya ikonen för din IoT-hubb i hello Azure portal tooopen hello bladet för hello ny IoT-hubb. Anteckna hello **värdnamn**, och klicka sedan på **principer för delad åtkomst**.
   
    ![Ny IoT Hub-blad][4]
1. I hello **principer för delad åtkomst** bladet, klickar du på hello **iothubowner** princip, kopiera och anteckna hello IoT-hubb anslutningssträngen hello **iothubowner** bladet. Mer information finns i [åtkomstkontroll] [ lnk-access-control] i hello ”IoT-hubb guide för utvecklare”.
   
    ![Bladet Principer för delad åtkomst][5]

<!-- Images. -->
[1]: ./media/iot-hub-get-started-create-hub/create-iot-hub1.png
[2]: ./media/iot-hub-get-started-create-hub/create-iot-hub2.png
[3]: ./media/iot-hub-get-started-create-hub/create-iot-hub3.png
[4]: ./media/iot-hub-get-started-create-hub/create-iot-hub4.png
[5]: ./media/iot-hub-get-started-create-hub/create-iot-hub5.png

<!-- Links -->
[lnk-resource-groups]: ../articles/azure-resource-manager/resource-group-portal.md
[lnk-portal]: https://portal.azure.com/
[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub/
[lnk-access-control]: ../articles/iot-hub/iot-hub-devguide-security.md
