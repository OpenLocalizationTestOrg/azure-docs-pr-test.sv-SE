1. Hello-portalen från **alla resurser**, klickar du på **+ Lägg till**. 
2. I hello **allt** bladet sökrutan, Skriv **lokal nätverksgateway**, klicka på toosearch. Detta gör att en lista returneras. Klicka på **lokal nätverksgateway** tooopen hello bladet och klicka sedan på **skapa** tooopen hello **skapa lokal nätverksgateway** bladet.

  ![skapa lokal nätverksgateway](./media/vpn-gateway-add-lng-s2s-rm-portal-include/createlng.png)

3. På hello **skapa lokal nätverksgateway-bladet**, ange hello värden för din lokala nätverksgateway.

  - **Namn:** Ange ett namn för ditt lokala gateway-objekt.
  - **IP-adress:** hello offentliga IP-adress för hello VPN-enhet som du vill använda Azure tooconnect till. Ange en giltig offentlig IP-adress. hello IP-adress får inte vara bakom NAT och har toobe som kan nås av Azure. Om du inte har hello IP-adress nu, du kan använda hello värdena som visas i hello skärmdump, men du behöver toogo tillbaka och ersätta din platshållare IP-adress med hello offentliga IP-adressen för VPN-enhet. Annars blir inte Azure kan tooconnect.
  - **Adressutrymmet** refererar toohello adressintervall för hello-nätverket som representerar den här lokala nätverket. Du kan lägga till flera adressintervall. Kontrollera att hello-intervall som du anger här inte överlappar intervallen för andra nätverk som du vill tooconnect till. Azure dirigerar hello-adressintervall som du anger toohello lokala VPN-enhetens IP-adress. *Använda egna värden här, inte hello värden som visas i skärmbilden hello*.
  - **Prenumerationen:** Kontrollera att korrekt prenumeration visas hello.
  - **Resursgrupp:** väljer hello resursgrupp som du vill toouse. Du kan antingen skapa en ny resursgrupp eller välja en som du redan har skapat.
  - **Plats:** Välj hello-plats som det här objektet kommer att skapas i. Du kanske vill tooselect hello samma plats som din virtuella nätverk finns i, inte men du nödvändiga toodo så.

4. När du är klar med att ange värden för hello klickar du på **skapa** längst hello hello bladet toocreate hello lokala nätverkets gateway.
