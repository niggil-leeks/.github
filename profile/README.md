# OPSEC LEVEL: GETPROP

## COMO FUNCIONA O SISTEMA DE LICENÇA DO SUNSHINE (`SerialAccess`)

O app possui proteção "DRM"/whitelist atrelada ao hardware:

1. Leitura do Serial: Executa via root getprop ro.serialno (ou ro.boot.serialno).
2. Normalização: Converte para maiúsculas e valida via regex [A-Z0-9]{6,64}.
3. Checagem Online: Baixa a lista de seriais autorizados de:
   https://raw.githubusercontent.com/haxiswillow/Sunshine/main/serial.txt
4. Aplicação:
   - ✅ Aprovado: Emite uma lease temporal (AccessLease), grava em /data/adb/sunshineparts-access/state (chmod 600) e define a propriedade persist.sys.sunshineparts.access.
   - ❌ Reprovado: Dispara suspendForAuthorization, desfazendo os patches e bloqueando os recursos do módulo.


## HOW THE SUNSHINE LICENSE SYSTEM (`SerialAccess`) WORKS

The app uses hardware-bound "DRM"/whitelist protection:

1. Serial Number Retrieval: Executes `getprop ro.serialno` (or `ro.boot.serialno`) with root privileges.
2. Normalization: Converts the serial number to uppercase and validates it using the regex pattern `[A-Z0-9]{6,64}`.
3. Online Verification: Downloads the list of authorized serial numbers from:

[https://raw.githubusercontent.com/haxiswillow/Sunshine/main/serial.txt](<https://raw.githubusercontent.com/haxiswillow/Sunshine/main/serial.txt?utm_source=chatgpt.com>)

4. Authorization Process:
- ✅ Approved: Issues a temporary access lease (`AccessLease`), saves it to `/data/adb/sunshineparts-access/state` (with permissions set to `chmod 600`), and sets the `persist.sys.sunshineparts.access` system property.
- ❌ Rejected: Triggers `suspendForAuthorization`, which reverts the applied patches and disables the module's features.
