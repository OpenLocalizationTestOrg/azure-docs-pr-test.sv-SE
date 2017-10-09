### <a name="noconnection"></a>toomodify lokala nätverket gateway IP-adressprefix - ingen gateway-anslutningen

#### <a name="tooadd-additional-address-prefixes"></a>tooadd ytterligare adressprefix:

1. På hello lokala nätverksgateway resurs i hello **inställningar** klickar du på **Configuration**.
2. Lägg till hello IP-adressutrymme i hello *Lägg till ytterligare adressintervall* rutan.
3. Klicka på **spara** toosave dina inställningar.

#### <a name="tooremove-address-prefixes"></a>tooremove adressprefix:

1. På hello lokala nätverksgateway resurs i hello **inställningar** klickar du på **Configuration**.
2. Klicka på hello **”...”** hello rad som innehåller hello prefix du vill tooremove.
3. Klicka på **ta bort**.
4. Klicka på **spara** toosave dina inställningar.

### <a name="withconnection"></a>toomodify lokala nätverket gateway IP-adressprefix - befintlig gateway-anslutningen

Om du har en gateway-anslutning och vill tooadd eller ta bort hello IP-adressprefixet som ingår i din lokala nätverksgateway, behöver toodo hello följa stegen i ordning. Det medför en del avbrott för din VPN-anslutning. När du ändrar IP-adressprefix, behöver du inte toodelete hello VPN-gateway. Du behöver bara tooremove hello anslutning.

#### <a name="1-remove-hello-connection"></a>1. Ta bort hello anslutningen.

1. På hello lokala nätverksgateway resurs i hello **inställningar** klickar du på **anslutningar**.
2. Klicka på hello **...**  på hello rad för varje anslutning, klicka sedan på **ta bort**.
3. Klicka på **spara** toosave dina inställningar.

#### <a name="2-modify-hello-address-prefixes"></a>2. Ändra hello-adressprefix.

tooadd ytterligare adressprefix:

1. På hello lokala nätverksgateway resurs i hello **inställningar** klickar du på **Configuration**.
2. Lägg till hello IP-adressutrymme.
3. Klicka på **spara** toosave dina inställningar.

tooremove adressprefix:

1. På hello lokala nätverksgateway resurs i hello **inställningar** klickar du på **Configuration**.
2. Klicka på hello **...**  hello rad som innehåller hello prefix du vill tooremove.
3. Klicka på **ta bort**.
4. Klicka på **spara** toosave dina inställningar.

#### <a name="3-recreate-hello-connection"></a>3. Återskapa hello-anslutning.

1. Navigera toohello virtuell nätverksgateway för din VNet. (Inte hello lokala nätverksgateway kvar.)
2. På hello virtuell nätverksgateway i hello **inställningar** klickar du på **anslutningar**.
3. Klicka på hello **+ Lägg till** tooopen hello **Lägg till anslutning** bladet.
4. Återskapa din anslutning.
5. Klicka på **OK** toocreate hello anslutning.
