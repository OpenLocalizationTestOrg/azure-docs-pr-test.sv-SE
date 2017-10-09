1. Klicka på vänster sida av hello Portalsida hello,  **+**  och Skriv virtuell nätverksgateway i sökningen. I **Resultat** letar du upp och klickar på **Virtuell nätverksgateway**.
2. Hello längst ned på hello virtuell nätverksgateway-bladet, klickar du på **skapa**. Då öppnas hello **Skapa virtuell nätverksgateway** bladet.

    ![Fält på bladet Skapa virtuell nätverksgateway](./media/vpn-gateway-add-gw-s2s-rm-portal-include/vnet_gw.png "Ny gateway")

3. På hello **Skapa virtuell nätverksgateway** bladet ange hello värden för din virtuella nätverksgateway.

  - **Namn**: namnge din gateway. Detta är inte hello samma som att namnge ett gateway-undernät. Det är hello namnet på hello gateway-objekt som du skapar.
  - **Gatewaytyp**: välj **VPN**. VPN-gatewayer Använd hello virtuellt nätverkstypen **VPN**. 
  - **VPN-typ**: Välj hello VPN-typ som har angetts för din konfiguration. De flesta konfigurationer kräver en ruttbaserad VPN-typ.
  - **SKU**: Välj hello gateway SKU hello listrutan. hello SKU: er som anges i hello dropdown är beroende av hello VPN-typ du väljer. Se [Gateway-SKU:er](../articles/vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md#gwsku) för information om gateway-SKU:er.
  - **Plats**: du kanske måste tooscroll toosee plats. Justera hello **plats** fältet toopoint toohello plats där det virtuella nätverket finns. Om hello plats inte pekar toohello region där det virtuella nätverket finns, visas hello virtuella nätverk inte i hello nästa steg, välja ett virtuellt nätverk' listrutan.
  - **Virtuellt nätverk**: Välj hello virtuellt nätverk toowhich önskade tooadd denna gateway. Klicka på **för virtuella nätverk** tooopen hello ”Välj ett virtuellt nätverk-bladet. Välj hello virtuella nätverk. Om du inte ser ditt VNet, kontrollerar du hello Platsfältet pekar toohello region som det virtuella nätverket finns.
  - **Offentliga IP-adressen**: hello ”skapa offentlig IP-adress-bladet skapar en offentlig IP-adress-objekt. hello offentliga IP-adressen tilldelas dynamiskt när hello VPN-gateway har skapats. VPN-gateway stöder för närvarande endast *dynamisk* offentlig IP-adressallokering. Detta innebär dock inte att hello IP-adressen ändras när den har tilldelats tooyour VPN-gateway. hello endast tid hello offentliga IP-adressändringarna är när hello gateway bort och återskapas. Den ändras inte vid storleksändring, återställning eller annat internt underhåll/uppgraderingar av din VPN-gateway.

    - Klicka först på **offentliga IP-adressen** tooopen hello ”Välj offentlig IP-adress-bladet och klicka sedan på **+ skapa nya** tooopen hello” skapa offentlig IP-adress-bladet.
    - Ange sedan en **namn** för din offentliga IP-adress, klicka sedan på **OK** på hello längst ned i det här bladet toosave ändringarna.

      ![Skapa offentlig IP](./media/vpn-gateway-add-gw-s2s-rm-portal-include/pip.png "Skapa offentlig IP")

4. Verifiera inställningarna för hello. Du kan välja **PIN-kod toodashboard** längst hello hello bladet om du vill att din gateway tooappear på hello instrumentpanel. 
5. Klicka på **skapa** toobegin skapar hello VPN-gateway. hello inställningar kommer att valideras och ser du hello ”distribuerar virtuell nätverksgateway”-rutan på hello instrumentpanel. Skapa en gateway kan ta upp too45 minuter. Du kan behöva toorefresh din Portalsida toosee hello slutförd status.

Visa hello IP-adress som har tilldelats tooit genom att titta på hello virtuellt nätverk i hello portal när hello gateway har skapats. hello gatewayen visas som en ansluten enhet. Du kan klicka på hello anslutna enheten (din virtuella nätverksgateway) tooview mer information.
