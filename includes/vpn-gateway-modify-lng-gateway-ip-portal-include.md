### <a name="gwipnoconnection"></a>toomodify hello lokala nätverkets gateway IP-adress - ingen gateway-anslutningen

Använd hello exempel toomodify en lokal nätverksgateway som inte har en gateway-anslutningen. När du ändrar det här värdet kan du också ändra hello adressprefix på hello samtidigt.

1. På hello lokala nätverksgateway resurs i hello **inställningar** klickar du på **Configuration**.
2. I hello **IP-adress** ändrar hello IP-adress.
3. Klicka på **spara** toosave hello inställningar.

### <a name="gwipwithconnection"></a>toomodify hello lokala nätverkets gateway gateway IP-adress - befintlig gateway-anslutningen

toomodify en lokal nätverksgateway som har en anslutning, behöver du toofirst ta bort hello anslutning. Du kan ändra hello gateway IP-adress och skapa en ny anslutning när hello anslutningen tas bort. Du kan också ändra hello adressprefix på hello samtidigt. Det medför en del avbrott för din VPN-anslutning. När du ändrar hello IP-adressen för gateway, behöver du inte toodelete hello VPN-gateway. Du behöver bara tooremove hello anslutning.
 
#### <a name="1-remove-hello-connection"></a>1. Ta bort hello anslutningen.

1. På hello lokala nätverksgateway resurs i hello **inställningar** klickar du på **anslutningar**.
2. Klicka på hello **...**  på hello rad för hello-anslutning, klicka sedan på **ta bort**.
3. Klicka på **spara** toosave dina inställningar.

#### <a name="2-modify-hello-ip-address"></a>2. Ändra hello IP-adress.

Du kan också ändra hello adressprefix på hello samtidigt.

1. I hello **IP-adress** ändrar hello IP-adress.
2. Klicka på **spara** toosave hello inställningar.

#### <a name="3-recreate-hello-connection"></a>3. Återskapa hello-anslutning.

1. Navigera toohello virtuell nätverksgateway för din VNet. (Inte hello lokala nätverksgateway kvar.)
2. På hello virtuell nätverksgateway i hello **inställningar** klickar du på **anslutningar**.
3. Klicka på hello **+ Lägg till** tooopen hello **Lägg till anslutning** bladet.
4. Återskapa din anslutning.
5. Klicka på **OK** toocreate hello anslutning.
