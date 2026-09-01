# IR-001 — Análise estática com JADX

## Workspace

A análise foi feita no Kali em:

```text
~/zte-investigation/apk-evidence
```

Foram mantidas árvores separadas:

```text
jadx-telo/
jadx-dzxl/
```

## Estratégia

A pesquisa foi dirigida por capacidades e APIs, evitando depender apenas de nomes de classes obfuscados.

Exemplos:

```bash
grep -RniE \
'PackageInstaller|createSession|openSession|commit\(|ACTION_INSTALL_PACKAGE|REQUEST_INSTALL_PACKAGES|DownloadManager|OkHttp|URLConnection|https?://' \
jadx-telo/sources/io/telo/link
```

No payload foram pesquisadas referências a:

- `AccessibilityService`;
- `AccessibilityNodeInfo`;
- `GestureDescription`;
- `performGlobalAction`;
- `takeScreenshot`;
- WebView / JavaScript interface;
- `ProcessBuilder`;
- boot receivers;
- serviços;
- operações sobre contactos.

## `io.telo.link`

A análise revelou:

- download/configuração remota;
- HMAC e identificação derivada do dispositivo;
- endpoint `/p/req`;
- interpretação de configuração;
- `PackageInstaller`;
- criação de session;
- escrita de `base.apk`;
- commit para receiver;
- boot/retry;
- `NullVpnService`.

## `dzxl.wpdzac.fnmrmay`

O código estava significativamente obfuscado.

Foram identificadas capacidades de:

- inspeção da árvore de acessibilidade;
- pesquisa/click de nós;
- gestos;
- BACK/HOME/RECENTS;
- screenshots;
- WebView com JavaScript;
- ações automatizadas de UI;
- persistência via componentes de boot/serviços;
- código relacionado com contactos.

## Regra de interpretação

Presença no código = **capacidade estática**.

Só é promovida a comportamento runtime quando corroborada por evidência como AppOps, UsageStats, processos, serviços, logs ou outros artefactos.

Esta distinção foi essencial para evitar alegações não demonstradas sobre SMS, screenshots ou roubo de credenciais.
