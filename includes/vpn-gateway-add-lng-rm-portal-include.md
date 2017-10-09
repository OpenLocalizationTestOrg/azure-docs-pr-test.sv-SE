1. Hello-portalen från **alla resurser**, klickar du på **+ Lägg till**. I hello **allt** bladet sökrutan, Skriv **lokal nätverksgateway**, klicka på tooreturn en lista över resurser. Klicka på **lokal nätverksgateway** tooopen hello bladet och klicka sedan på **skapa** tooopen hello **skapa lokal nätverksgateway** bladet.
   
    ![skapa lokal nätverksgateway](./media/vpn-gateway-add-lng-rm-portal-include/lng.png)

2. På hello **skapa lokal nätverksgateway-bladet**, ange en **namn** för din lokala gateway-objekt. Använd om möjligt något intuitiva som **ClassicVNetLocal** eller **TestVNet1Local**. Detta gör det enklare för du tooidentify hello lokal nätverksgateway hello-portalen.
3. Ange en giltig offentlig **IP-adress** hello VPN-enhet eller virtuellt gateway toowhich du vill ha tooconnect.<br>**Om den här lokala nätverket representerar en lokal plats:** ange hello offentliga IP-adress för hello VPN-enhet som du vill tooconnect till. Det får inte vara bakom NAT och har toobe som kan nås av Azure.<br>**Om den här lokala nätverket motsvarar ett annat VNet:** ange hello offentliga IP-adress som har tilldelats toohello virtuella nätverks-gatewayen för det virtuella nätverket.<br>**Om du ännu inte har hello IP-adress:** du utgör en giltig platshållare IP-adress och sedan gå tillbaka och ändra den här inställningen innan du ansluter.
4. **Adressutrymmet** refererar toohello adressintervall för hello-nätverket som representerar den här lokala nätverket. Du kan lägga till flera adressintervall. Kontrollera att hello-intervall som du anger här inte överlappar intervallen för andra toowhich för nätverk som du ansluter.
5. För **prenumeration**, kontrollera att korrekt prenumeration visas hello.
6. För **resursgruppen**väljer hello resursgrupp som du vill toouse. Du kan antingen skapa en ny resursgrupp eller välja en som du redan har skapat.
7. För **plats**, Välj hello plats där den här resursen kommer att skapas. Du kanske vill tooselect hello samma plats som din virtuella nätverk finns i, inte men du nödvändiga toodo så.
8. Klicka på **skapa** toocreate hello lokal nätverksgateway.

