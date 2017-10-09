
Varje slutpunkt har en *offentlig port* och en *privat port*:

* hello offentliga porten används av hello Azure load balancer toolisten för inkommande trafik toohello virtuell dator från hello Internet.
* hello privat port används av hello virtuella toolisten för inkommande trafik, vanligtvis är avsedda tooan program eller tjänst som körs på hello virtuell dator.

Standardvärden för hello IP-protokollet och TCP eller UDP-portar för välkända nätverksprotokoll tillhandahålls när du skapar slutpunkter med hello Azure-portalen. För anpassade slutpunkter behöver toospecify hello rätt IP-protokoll (TCP och UDP) och hello offentliga och privata portar. toodistribute inkommande trafik slumpmässigt över flera virtuella datorer, måste du ha toocreate en belastningsutjämnad uppsättning som består av flera slutpunkter.

När du har skapat en slutpunkt kan du använda en åtkomstkontrollistan (ACL) toodefine regler för åtkomstkontroll som tillåter eller nekar hello inkommande trafik toohello offentlig port för hello slutpunkten baserat på dess IP-adress. Du bör dock använda nätverkssäkerhetsgrupper i stället om hello virtuell dator i Azure-nätverk. Mer information finns i [om nätverkssäkerhetsgrupper](../articles/virtual-network/virtual-networks-nsg.md).

> [!NOTE]
> Brandväggskonfigurationen för virtuella Azure-datorer görs automatiskt för portar som är associerade med fjärranslutningar slutpunkter som Azure ställer in automatiskt. För portar som angetts för alla slutpunkter, ingen konfiguration görs automatiskt toohello brandvägg hello virtuell dator. När du skapar en slutpunkt för hello virtuell dator, måste du tooensure som hello brandvägg hello virtuell dator kan också hello trafik för hello protokoll och privat port motsvarande toohello slutpunktskonfigurationen. tooconfigure hello brandväggen, se hello dokumentation eller Onlinehjälp för hello-operativsystem som körs på hello virtuell dator.
>
>

## <a name="create-an-endpoint"></a>Skapa en slutpunkt
1. Om du inte redan har gjort det loggar du in toohello [Azure-portalen](https://portal.azure.com).
2. Klicka på **virtuella datorer**, och klicka sedan på hello namnet på hello virtuella dator som du vill tooconfigure.
3. Klicka på **slutpunkter** i hello **inställningar** grupp. Hej **slutpunkter** sidan listar alla aktuella hello-slutpunkter för hello virtuella datorn. (Det här exemplet är en virtuell Windows-dator. Linux VM visas som standard en slutpunkt för SSH.)

   <!-- ![Endpoints](./media/virtual-machines-common-classic-setup-endpoints/endpointswindows.png) -->
   ![Slutpunkter](./media/virtual-machines-common-classic-setup-endpoints/endpointsblade.png)

4. Klicka på hello kommandofältet ovan hello endpoint poster **Lägg till**.
5. På hello **lägga till slutpunkten** anger du ett namn för hello slutpunkten i **namn**.
6. I **protokollet**, Välj antingen **TCP** eller **UDP**.
7. I **offentlig Port**, Skriv portnumret för hello hello inkommande trafik från hello Internet. I **privat Port**, Skriv portnumret för hello vilka hello virtuella lyssnar. Dessa portnummer kan vara olika. Se till att hello brandväggen på hello virtuella datorn har konfigurerade tooallow hello trafik motsvarande toohello protokoll (i steg 6) och privat port.
10. Klicka på **OK**.

hello ny slutpunkt visas på hello **slutpunkter** sidan.

![Skapa en slutpunkt lyckades](./media/virtual-machines-common-classic-setup-endpoints/endpointcreated.png)

## <a name="manage-hello-acl-on-an-endpoint"></a>Hantera hello ACL på en slutpunkt
toodefine hello uppsättning datorer som kan skicka trafik, hello ACL på en slutpunkt kan begränsa trafik baserat på källans IP-adress. Följ dessa steg tooadd, ändra eller ta bort en ACL för en slutpunkt.

> [!NOTE]
> Om hello slutpunkten är del av en belastningsutjämnad uppsättning, alla ändringar du gör toohello ACL på en slutpunkt är tillämpade tooall slutpunkter i hello uppsättningen.
>
>

Om hello virtuella datorn är i ett virtuellt Azure-nätverk, rekommenderar vi nätverkssäkerhetsgrupper i stället för ACL: er. Mer information finns i [om nätverkssäkerhetsgrupper](../articles/virtual-network/virtual-networks-nsg.md).

1. Om du inte redan har gjort logga in toohello Azure-portalen.
2. Klicka på **virtuella datorer**, och klicka sedan på hello namnet på hello virtuella dator som du vill tooconfigure.
3. Klicka på **Slutpunkter**. Välj lämplig hello-slutpunkt hello listan. hello åtkomstkontrollistan är på hello hello sidans nederkant.

   ![Ange ACL-information](./media/virtual-machines-common-classic-setup-endpoints/aclpreentry.png)

4. Använd rader i hello listan tooadd, ta bort eller redigera regler för en ACL och ändra deras inbördes ordning. Hej **fjärrundernät** värde är ett IP-adressintervall för inkommande trafik från hello Internet som hello Azure belastningen belastningsutjämnare använder toopermit eller neka hello trafik baserat på dess IP-adress. Vara säker på att toospecify hello IP-adressintervall i CIDR-format, även kallat prefix adressformat. Ett exempel är `10.1.0.0/8`.

 ![Ny ACL-post](./media/virtual-machines-common-classic-setup-endpoints/newaclentry.png)


Du kan använda regler tooallow endast trafik från vissa datorer motsvarande tooyour datorer i hello Internet eller toodeny trafik från specifika, kända adressintervall.

hello reglerna utvärderas i ordning från och med hello första regeln och slutar med hello sista regeln. Detta innebär att regler ska sorteras från minst restriktiva toomost restriktiva. Mer information och exempel finns i [vad är en lista över åtkomstkontroll för](../articles/virtual-network/virtual-networks-acl.md).
