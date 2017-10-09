Azure fastställer hello version av Python toouse för sin virtuella miljö med hello efter prioritet:

1. versionen som anges i runtime.txt i rotmappen för hello
2. versionen som anges i Python-inställningen i webbappkonfigurationen hello (hello **inställningar** > **programinställningar** bladet för din webbapp i hello Azure Portal)
3. Python-2.7 är hello standard om inget av ovanstående hello har angetts

Giltiga värden för hello innehållet i 

    \runtime.txt

är:

* python-2.7
* python-3.4

Om hello mikroversion (tredje siffran) har angetts, ignoreras.

