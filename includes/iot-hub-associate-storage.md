## <a name="associate-an-azure-storage-account-tooiot-hub"></a>Associera en Azure Storage-konto tooIoT Hub

Eftersom hello simulerade enhetsapp laddar upp en fil tooa blob, måste du ha en [Azure Storage](../articles/storage/common/storage-create-storage-account.md#create-a-storage-account) associerat konto tooIoT hubb. När du kopplar ett Azure Storage-konto med en IoT-hubb genererar hello IoT-hubb en SAS-URI. En enhet kan använda denna SAS URI toosecurely överför en fil tooa blob-behållare. Hej IoT-hubb-tjänsten och hello samordna enheten SDK hello process som genererar hello SAS-URI och gör den tillgänglig tooa enhet toouse tooupload en fil.

Följ anvisningarna för hello i [konfigurera filöverföringar med hjälp av hello Azure-portalen](../articles/iot-hub/iot-hub-configure-file-upload.md) tooassociate en Azure Storage-konto tooyour IoT-hubb. Se till att en blob-behållare är associerad med din IoT-hubb och att filen meddelanden är aktiverade.

![Aktivera meddelanden för filen i portalen](media/iot-hub-associate-storage/enable-file-notifications.png)