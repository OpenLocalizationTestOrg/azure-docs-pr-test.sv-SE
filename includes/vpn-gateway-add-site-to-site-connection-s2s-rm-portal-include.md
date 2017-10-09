1. Navigera tooand öppna hello bladet för din virtuella nätverksgateway. Det finns flera sätt toonavigate. I vårt exempel vi navigerat toohello gateway 'VNet1GW' genom att gå för**TestVNet1 -> Översikt -> ansluten enheter -> VNet1GW**.
2. Klicka på hello bladet för VNet1GW **anslutningar**. Hello överkant hello anslutningar bladet, klickar du på **+ Lägg till** tooopen hello **Lägg till anslutning** bladet.

    ![Skapa en plats-till-plats-anslutning](./media/vpn-gateway-add-site-to-site-connection-s2s-rm-portal-include/connection1.png)

3. På hello **Lägg till anslutning** bladet, Fyll i hello värden toocreate anslutningen.

  - **Namn:** Namnge din anslutning. Vi använder **VNet1toSite2** i vårt exempel.
  - **Anslutningstyp:** Välj du **Plats-till-plats(IPSec)**.
  - **Virtuell nätverksgateway:** hello värdet låst eftersom du ansluter från den här gatewayen.
  - **Lokal nätverksgateway:** klickar du på **Välj en lokal nätverksgateway** och välj hello lokal nätverksgateway som du vill toouse. I vårt exempel använder vi **Webbplats2**.
  - **Delad nyckel:** hello värdet matcha hello-värde som du använder för din lokala lokala VPN-enhet. I exemplet hello vi använde 'abc123', men du kan (och bör) använda något mer komplicerad. hello viktig sak är att hello-värde som du anger här måste vara hello samma värde som du angav när du konfigurerar VPN-enhet.
  - hello återstående för **prenumeration**, **resursgruppen**, och **plats** korrigeras.

4. Klicka på **OK** toocreate anslutningen. Du ser *skapar anslutningen* flash på hello-skärmen.
5. Du kan visa hello anslutning i hello **anslutningar** bladet för hello virtuell nätverksgateway. hello Status ska gå från *okänd* för*ansluta*, och sedan för*lyckades*.
