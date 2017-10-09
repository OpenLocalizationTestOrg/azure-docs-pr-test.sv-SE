## <a name="create-an-iot-hub"></a>Skapa en IoT Hub

1. I hello [Azure-portalen](https://portal.azure.com/), klickar du på **ny** > **Sakernas Internet** > **IoT-hubb**.

   ![Skapa en IoT-hubb i hello Azure-portalen](../articles/iot-hub/media/iot-hub-create-hub-and-device/1_create-azure-iot-hub-portal.png)
2. I hello **IoT-hubb** rutan Ange följande information för din IoT-hubb hello:

     **Namnet**: Ange hello namn för din IoT-hubb. Om hello-namn som du anger är giltigt, visas en grön bock.

     **Pris-och skala**: Välj hello **F1 - ledigt** nivå. Det här alternativet är tillräckligt för den här demonstrationen. Mer information finns i hello [priser och skalnivå](https://azure.microsoft.com/pricing/details/iot-hub/).

     **Resursgruppen**: skapa en resurs grupp toohost hello IoT-hubb eller använda en befintlig. Mer information finns i [Använd resursgrupper toomanage resurserna i Azure](../articles/azure-resource-manager/resource-group-portal.md).

     **Plats**: Välj hello den närmaste platsen tooyou där hello IoT-hubben har skapats.

     **PIN-kod toodashboard**: Välj det här alternativet för enkel åtkomst tooyour IoT-hubb hello instrumentpanel.

   ![Ange information toocreate din IoT-hubb](../articles/iot-hub/media/iot-hub-create-hub-and-device/2_fill-in-fields-for-azure-iot-hub-portal.png)

   [!INCLUDE [iot-hub-pii-note-naming-hub](iot-hub-pii-note-naming-hub.md)]

3. Klicka på **Skapa**. IoT-hubb kan ta några minuter toocreate. Du kan se förloppet på hello **meddelanden** fönstret.

   ![Visa meddelanden om IoT-hubbens förlopp](../articles/iot-hub/media/iot-hub-create-hub-and-device/3_notification-azure-iot-hub-creation-progress-portal.png)

4. När du har skapat din IoT-hubb klickar du på hello instrumentpanel. Anteckna hello **värdnamn**, och klicka sedan på **principer för delad åtkomst**.

   ![Hämta hello värdnamnet för din IoT-hubb](../articles/iot-hub/media/iot-hub-create-hub-and-device/4_get-azure-iot-hub-hostname-portal.png)

5. I hello **principer för delad åtkomst** rutan klickar du på hello **iothubowner** princip, och sedan kopiera och gjort en notering om hello **anslutningssträngen** för din IoT-hubb. Mer information finns i [kontroll åtkomst tooIoT hubb](../articles/iot-hub/iot-hub-devguide-security.md).

> [!NOTE] 
Du behöver inte den här iothubowner-anslutningssträngen i den här konfigurationskursen. Men kanske du måste den för några av hello självstudier om olika IoT-scenarier när du har slutfört den här uppsättningen.

   ![Hämta IoT-hubbens anslutningssträng](../articles/iot-hub/media/iot-hub-create-hub-and-device/5_get-azure-iot-hub-connection-string-portal.png)

## <a name="register-a-device-in-hello-iot-hub-for-your-device"></a>Registrera en enhet i hello IoT-hubb för din enhet

1. I hello [Azure-portalen](https://portal.azure.com/), öppna din IoT-hubb.

2. Klicka på **Enhetsutforskare**.
3. I hello enheten Explorer-fönstret klickar du på **Lägg till** tooadd enhet tooyour IoT-hubb. Sedan hello följande:

   **Enhets-ID**: Ange hello ID hello ny enhet. Enhets-ID är skiftlägeskänsliga.

   **Autentiseringstyp**: Välj **Symmetrisk nyckel**.

   **Generera nycklar automatiskt**: Markera den här kryssrutan.

   **Ansluta enheten tooIoT hubb**: Klicka på **aktivera**.

   ![Lägga till en enhet i hello enheten Explorer för din IoT-hubb](../articles/iot-hub/media/iot-hub-create-hub-and-device/6_add-device-in-azure-iot-hub-device-explorer-portal.png)

   [!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

4. Klicka på **Spara**.
5. När du har skapat hello enheten öppna hello enhet i hello **enheten Explorer** fönstret.
6. Anteckna hello primärnyckel för hello anslutningssträngen.

   ![Hämta anslutningssträngen för hello-enhet](../articles/iot-hub/media/iot-hub-create-hub-and-device/7_get-device-connection-string-in-device-explorer-portal.png)
