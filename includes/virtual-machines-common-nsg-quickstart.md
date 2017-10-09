Du öppnar en port eller skapa en slutpunkt tooa virtuell dator (VM) i Azure genom att skapa ett filter för nätverk på ett undernät eller Virtuella datorns nätverksgränssnitt. Du placera dessa filter som styr både inkommande och utgående trafik på en Nätverkssäkerhetsgrupp kopplad toohello resurs som tar emot hello trafik.

Nu ska vi använda ett vanligt exempel på Internet-trafik på port 80. När du har en virtuell dator som är konfigurerade tooserve webbförfrågningar på hello standard TCP-port 80 (Kom ihåg toostart hello rätt tjänster och öppna alla OS brandväggsregler på hello VM samt) du:

1. Skapa en säkerhetsgrupp för nätverk.
2. Skapa en inkommande regel som tillåter trafik med:
   * Hej Målportintervall ”80”
   * Hej Källportintervall av ”*” (vilket innebär att alla källportar)
   * ett prioritetsvärde för mindre 65 500 (toobe högre upp i prioritet än hello standard fångar in alla regel för att neka inkommande)
3. Associera hello Nätverkssäkerhetsgrupp med hello Virtuella datorns nätverksgränssnitt eller undernät.

Du kan skapa komplexa nätverk konfigurationer toosecure miljön med Nätverkssäkerhetsgrupper och regler. Vårt exempel används bara en eller två regler som tillåter HTTP-trafik eller för fjärrhantering. Mer information finns i följande hello [”mer Information”](#more-information-on-network-security-groups) avsnittet eller [vad är en Nätverkssäkerhetsgrupp?](../articles/virtual-network/virtual-networks-nsg.md)

