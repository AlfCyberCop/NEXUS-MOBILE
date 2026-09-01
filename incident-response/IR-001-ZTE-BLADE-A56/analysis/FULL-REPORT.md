# IR-001 — Relatório técnico completo

## 1. Resumo executivo

A investigação confirmou compromisso Android ao nível de aplicações. A cadeia reconstruída foi:

`Chrome → Android Package Installer → io.telo.link → dzxl.wpdzac.fnmrmay`

O primeiro componente funcionava como dropper/downloader e instalador auxiliar; o segundo implementava capacidades de `AccessibilityService` suficientemente abrangentes para automação abusiva da interface.

A análise combinou ADB, PackageManager, AppOps, UsageStats, bugreport, preservação de APKs, hashing e análise estática com JADX.

Não foi encontrada evidência de root, alteração do bootloader, compromisso de firmware ou instalação como aplicação de sistema. Após contenção, desativação, remoção, reboot e validação, não foi observada persistência imediata.

## 2. Baseline

- ZTE Blade A56 / Z2473 / P606F21
- Android 15 / API 35
- security patch `2026-06-05`
- Verified Boot `green`
- bootloader bloqueado
- investigação inicialmente em Safe Mode
- validação posterior em modo normal

## 3. Dropper

`io.telo.link`

- versionCode `1`
- versionName `1.0`
- targetSdk `35`
- instalado `2026-08-20 19:39:25`
- installer `com.google.android.packageinstaller`
- originating package `com.android.chrome`

Implementava download/configuração, PackageInstaller, retry, boot receiver e VpnService local.

## 4. Payload

`dzxl.wpdzac.fnmrmay`

- versionCode `238134`
- targetSdk `34`
- versionName `23.81.34 kvvmfuawrungbcznij`
- instalado `2026-08-20 19:39:58`
- installer `io.telo.link`

Componentes relevantes: BootReceiver, ResetServices, DreamService e AccessibilityService obfuscado.

## 5. Cadeia de infeção

O APK residual encontrado em:

`/storage/emulated/0/Download/dzxl.wpdzac.fnmrmay.apk`

tinha nome que sugeria o payload, mas SHA-256 igual ao dropper.

Sequência suportada:

1. Chrome originou o sideload.
2. Package Installer instalou `io.telo.link`.
3. 33 segundos depois `io.telo.link` instalou `dzxl.wpdzac.fnmrmay`.

## 6. Infraestrutura

```text
server_url = http://212.69.5.181
payload_slug = c4chfmzw4d
target_pkg = dzxl.wpdzac.fnmrmay
endpoint = http://212.69.5.181/p/req
```

A aplicação derivava identificador a partir de `ANDROID_ID + Build.FINGERPRINT`, calculava HMAC e enviava JSON por HTTP.

A infraestrutura é tratada como entrega/staging; não se afirma C2 completo sem prova adicional.

## 7. VPN

`NullVpnService` criava TUN, adicionava `0.0.0.0/0` e descartava tráfego. Endereço por defeito `10.8.0.1/32`, MTU 1500.

AppOps confirmou uso real do VpnService por cerca de 19 segundos.

## 8. Accessibility

O payload implementava acesso amplo à árvore de acessibilidade, gestos, cliques, ações globais, screenshots, overlays e automação de UI.

AppOps confirmou:

- `BIND_ACCESSIBILITY_SERVICE=allow`
- `ACCESS_ACCESSIBILITY=allow`
- `CREATE_ACCESSIBILITY_OVERLAY=allow`

Logo, a utilização abusiva de Accessibility não é apenas uma hipótese estática.

## 9. Uso recorrente

UsageStats mostrou execução repetida ao longo de vários dias, incluindo 26, 28, 29 e 30 de agosto.

## 10. Limitações

Não há evidência suficiente para afirmar:

- roubo de credenciais bancárias;
- captura concreta de screenshots;
- leitura efetiva de SMS;
- compromisso de firmware;
- root;
- atribuição a pessoa/grupo específico.

## 11. Evidência

Consultar [`../evidence/EVIDENCE-MANIFEST.md`](../evidence/EVIDENCE-MANIFEST.md).

## 12. Certificados

Dropper:

`a4ce771d988fb94c4bf852846de675fb60ea3f65bf08bd690a9de05b913bb38f`

Payload:

`6215f00baa4bf18bab5792fc796bfc5555917240f14f7c7e672d956888d75c96`

## 13. OSINT passivo

Foi encontrada corroboração pública que associava `212.69.5.181` a infraestrutura de download malicioso pouco antes da infeção observada. Esta correlação não prova propriedade do servidor pelo atacante.

## 14. Contenção

```bash
adb shell am force-stop dzxl.wpdzac.fnmrmay
adb shell pm disable-user --user 0 dzxl.wpdzac.fnmrmay

adb shell am force-stop io.telo.link
adb shell pm disable-user --user 0 io.telo.link

adb shell pm uninstall --user 0 dzxl.wpdzac.fnmrmay
adb shell pm uninstall --user 0 io.telo.link
```

As desativações devolveram `disabled-user` e os uninstalls devolveram `Success`.

## 15. Validação

Depois do reboot:

- packages ausentes de `pm list packages`;
- `enabled_accessibility_services = null`;
- `accessibility_enabled = 0`;
- sem processos/serviços/jobs atribuíveis;
- sem reaparecimento observável.

## 16. Falsos positivos

Foram investigados e não ligados à cadeia:

- `photo.photovault.hidepictures.gallery.lockgallery`;
- `com.video.data.photorecovery.filerecovery.deletedrestore`;
- `com.preff.kb.zx` / `com.preff.kb.LatinIME`;
- `.iGallery`;
- `sd_logs`.

O caso de `com.preff.kb.zx` mostrou por que pesquisas por substrings genéricas (`telo`) podem produzir falsos positivos.

## 17. Avaliação final

**COMPROMISSO ANDROID CONFIRMADO AO NÍVEL DE APLICAÇÃO.**

A cadeia de instalação está fortemente suportada por:

- proveniência PackageManager;
- timestamps;
- hashes;
- artefacto residual;
- código do dropper;
- AppOps;
- UsageStats.

A remoção foi bem-sucedida no nível observável, mas não fornece a mesma garantia de um factory reset/reflash.

## 18. Lições Purple Team

- Preservar antes de remover.
- Distinguir capacidade, evidência runtime e inferência.
- Usar PackageManager provenance + hashes + timeline.
- Usar AppOps para separar permissões pedidas de operações usadas.
- Usar UsageStats como evidência de recorrência, não como audit log de conteúdo.
- Preferir package names exatos nas pesquisas.
- Reconhecer os limites do ADB sem root.
