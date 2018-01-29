Följ stegen nedan för att skapa en VNet i Resource Manager-distributionsmodellen med hjälp av Azure Portal. Skärmbilderna anges som exempel. Glöm inte att byta ut värdena mot dina egna. Mer information om hur du arbetar med virtuella nätverk finns i [Översikt över virtuella nätverk](../articles/virtual-network/virtual-networks-overview.md).

>[!NOTE]
>För att det här virtuella nätverket ska anslutas till en lokal plats måste du kontakta den lokala nätverksadministratören för att få ett intervall med IP-adresser som du kan använda specifikt för det här virtuella nätverket. Om det finns ett duplicerat adressintervall på båda sidorna av VPN-anslutningen dirigeras inte trafiken som du förväntar dig. Om du dessutom vill ansluta detta VNet till ett annat VNet kan inte adressutrymmet överlappa med andra VNet. Var noga med att planera din nätverkskonfiguration på lämpligt sätt.
>
>

1. Navigera till [Azure-portalen](http://portal.azure.com) från en webbläsare och logga in med ditt Azure-konto vid behov.
2. Klicka på **+**. I fältet **Sök marknadsplats** skriver du "Virtuella nätverk". Leta upp **Virtuellt nätverk** bland sökresultaten och klicka för att öppna sidan **Virtuellt nätverk**.

  ![Sida för att leta upp virtuell nätverksresurs](./media/vpn-gateway-basic-vnet-rm-portal-include/newvnetportal700.png "Sida för att leta upp virtuell nätverksresurs")
3. I listan **Välj en distributionsmodell** nästan längst ned på sidan Virtuellt nätverk väljer du **Resource Manager** och klickar sedan på **Skapa**.

  ![Välj Resource Manager](./media/vpn-gateway-basic-vnet-rm-portal-include/resourcemanager250.png "Välj Resource Manager")
4. Konfigurera VNet-inställningarna på sidan **Skapa virtuellt nätverk**. När du fyller i fälten blir det röda utropstecknet en grön bock om tecknen som anges i fältet är giltiga. Det kan finnas värden som fylls i automatiskt. Om detta är fallet ska du ersätta dessa värden med dina egna. Sidan **Skapa virtuellt nätverk** liknar följande exempel:

  ![Sidan Skapa virtuellt nätverk](./media/vpn-gateway-basic-vnet-rm-portal-include/vnet.png "Sidan Skapa virtuellt nätverk")
5. **Namn**: Namnge ditt virtuella nätverk.
6. **Adressutrymme**: Ange adressutrymmet. Om du vill lägga till flera adressutrymme anger du ditt första adressutrymme. Du kan lägga till ytterligare adressutrymmen senare när du har skapat det virtuella nätverket.
7. **Prenumeration**: Verifiera att prenumerationen som visas är korrekt. Du kan ändra prenumerationer i listrutan.
8. **Resursgrupp**: Välj en befintlig resursgrupp eller skapa en ny genom att skriva ett namn för en ny resursgrupp. Om du skapar en ny grupp, bör du kalla resursgruppen efter dina planerade konfigurationsvärden. Mer information om resursgrupper finns i [Översikt över Azure Resource Manager](../articles/azure-resource-manager/resource-group-overview.md#resource-groups).
9. **Plats**: Välj plats för ditt VNet. Platsen avgör var resurserna som du distribuerar till detta VNet kommer att placeras.
10. **Undernät**: Lägg till undernätsnamn och adressintervall i undernätet. Du kan lägga till ytterligare undernät senare när du har skapat det virtuella nätverket.
11. Välj **Fäst vid instrumentpanelen** om du vill kunna hitta ditt VNet på ett enkelt sätt på instrumentpanelen och klicka sedan på **Skapa**.

  ![Fäst vid instrumentpanelen](./media/vpn-gateway-basic-vnet-rm-portal-include/pintodashboard150.png "Fäst vid instrumentpanelen")