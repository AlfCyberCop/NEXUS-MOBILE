# IR-001 — Timeline

## 2026-08-20

### 19:35
Foi observado posteriormente um APK residual em:

`/storage/emulated/0/Download/dzxl.wpdzac.fnmrmay.apk`

Metadados preservados:

- timestamp: `2026-08-20 19:35`
- tamanho: `6,561,880 bytes`

Apesar do nome sugerir o payload, o SHA-256 deste ficheiro era exatamente igual ao APK preservado de `io.telo.link`.

### 19:39:25
Instalação de `io.telo.link`.

Proveniência PackageManager:

- installer: `com.google.android.packageinstaller`
- originating package: `com.android.chrome`

### 19:39:58
Instalação de `dzxl.wpdzac.fnmrmay`.

- installer: `io.telo.link`
- diferença para o dropper: **33 segundos**

## 2026-08-26, 28, 29 e 30

UsageStats registou execução recorrente do payload, incluindo:

- `ACTIVITY_RESUMED`;
- foreground services;
- interações com `PermissionController`;
- múltiplas sessões/lançamentos.

## 2026-08-30

Às `15:37`, o payload iniciou pelo menos dois fluxos de `GrantPermissionsActivity`.

Foram também observados ANRs, incluindo:

- `No response to onStartJob`;
- input dispatching timeouts.

## Investigação / contenção

A recolha foi feita antes da remoção:

1. baseline;
2. PackageManager;
3. AppOps;
4. UsageStats;
5. bugreport;
6. preservação dos APKs;
7. hashing;
8. JADX;
9. contenção;
10. remoção;
11. reboot;
12. validação pós-remediação.

## Encerramento

Após o reboot:

- os dois package names já não apareciam em `pm list packages`;
- `enabled_accessibility_services = null`;
- `accessibility_enabled = 0`;
- não foram observados processos, serviços ou jobs atribuíveis aos pacotes;
- não houve reaparecimento imediato dos componentes maliciosos.
