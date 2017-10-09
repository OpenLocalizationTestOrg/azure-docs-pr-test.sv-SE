I det här steget skapar du en regel tooopen hello avsökningen brandväggsport för hello belastningsutjämnade slutpunkt (59999 som angavs tidigare) och en annan regel porten för tooopen hello tillgänglighetsgruppens lyssnare. Eftersom du skapade hello Utjämning av nätverksbelastning på hello virtuella datorer som innehåller tillgänglighetsgrupprepliker, behöver du tooopen hello avsökningsport och hello lyssningsport på hello respektive virtuella datorer.

1. Starta på virtuella datorer som är värdar för repliker, **Windows-brandväggen med avancerad säkerhet**.

2. Högerklicka på **regler för inkommande trafik**, och klicka sedan på **ny regel**.

3. På hello **regeltyp** väljer **Port**, och klicka sedan på **nästa**.

4. På hello **protokoll och portar** väljer **TCP**, typen **59999** i hello **specifika lokala portar** rutan och klicka på **Nästa**.

5. På hello **åtgärd** behåller **Tillåt hello anslutning** markerad och klicka sedan på **nästa**.

6. På hello **profil** , acceptera hello standardinställningar, och klickar sedan på **nästa**.

7. På hello **namn** i hello sidan **namn** text anger du ett namn för regeln som **alltid på avsökning lyssningsport**, och klicka sedan på **Slutför**.

8. Upprepa hello föregående steg för hello porten för tillgänglighetsgruppens lyssnare (enligt tidigare hello $EndpointPort parametern hello skriptet) och ange ett lämpligt namn, exempelvis **alltid på lyssningsport**.

