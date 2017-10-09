#### <a name="toocreate-public-endpoints-on-hello-cloud-appliance"></a>toocreate offentliga slutpunkter på hello moln-enhet

1. Logga in toohello Azure-portalen.
2. Gå för**virtuella datorer**, markera och klickar på hello virtuell dator som används som din enhet för molnet.
    
3. Du måste toocreate en network security group (NSG) regel toocontrol hello trafikflödet och från den virtuella datorn. Utföra hello följande steg toocreate en NSG-regel.
    1. Välj **Nätverkssäkerhetsgrupp**.
        ![](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt1.png)

    2. Klicka på hello standard nätverkssäkerhetsgruppen som visas.
        ![](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt2.png)

    3. Välj **Inkommande säkerhetsregel**.
        ![](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt3.png)

    4. Klicka på **+ Lägg till** toocreate en ingående säkerhetsregel.
        ![](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt4.png)

        I bladet för hello Lägg till inkommande säkerhetsregler regel:

        1. För hello **namn**, typen hello följande namn för hello slutpunkt: WinRMHttps.
        
        2. För hello **prioritet**väljer ett tal som är mindre än 1000 och (vilket är hello prioritet för hello standardregel). Högre hello värde i lägre hello prioritet.

        3. Ange hello **källa** för**alla**.

        4. För hello **Service**väljer **WinRM**. Hej **protokollet** anges automatiskt för**TCP** och hello **portintervall** har angetts för**5986**.

        5. Klicka på **OK** toocreate hello regeln.

            ![](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt5.png)

4. Det sista steget är tooassociate nätverkssäkerheten gruppen med ett undernät eller ett visst nätverksgränssnitt. Utför följande steg tooassociate hello säkerhetsgrupp för nätverk med ett undernät.
    1. Gå för**undernät**.
    2. Klicka på **+ Associera**.
        ![](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt7.png)

    3. Välj det virtuella nätverket och välj sedan hello rätt undernät.
    4. Klicka på **OK** toocreate hello regeln.

        ![](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt11.png)

När hello regeln har skapats kan visa du dess information toodetermine hello offentliga virtuella IP-adresser (VIP)-adress. Anteckna den adressen.


