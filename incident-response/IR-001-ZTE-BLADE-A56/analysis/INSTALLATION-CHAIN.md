# IR-001 — Cadeia de instalação

## Cadeia reconstruída

```text
Chrome
  │
  ├─ originatingPackageName = com.android.chrome
  ▼
APK residual em Download
  ▼
Google / Android Package Installer
  │
  ├─ installerPackageName = com.google.android.packageinstaller
  ▼
io.telo.link
  │
  ├─ instalado 2026-08-20 19:39:25
  ├─ REQUEST_INSTALL_PACKAGES = allow
  ├─ PackageInstaller session
  ▼
dzxl.wpdzac.fnmrmay
     ├─ instalado 2026-08-20 19:39:58
     └─ installerPackageName = io.telo.link
```

## Evidência convergente

A reconstrução não depende de um único indicador. É suportada por:

1. `originatingPackageName` de `io.telo.link`: `com.android.chrome`;
2. `installerPackageName` do dropper: `com.google.android.packageinstaller`;
3. `installerPackageName` do payload: `io.telo.link`;
4. diferença temporal de 33 segundos;
5. APK residual em `Download`;
6. igualdade de SHA-256 entre esse APK residual e `telo-base.apk`;
7. código do dropper que usa `PackageInstaller`;
8. código de download/configuração e relançamento do payload.

## Artefacto residual

`/storage/emulated/0/Download/dzxl.wpdzac.fnmrmay.apk`

- timestamp: `2026-08-20 19:35`;
- tamanho: `6,561,880 bytes`;
- SHA-256: igual ao dropper `io.telo.link`.

O nome do ficheiro era enganador: sugeria o payload, mas o conteúdo correspondia ao dropper.

## Grau de confiança

**Elevado.**

PackageManager provenance + timestamps + hashes + artefacto residual + análise de código permitem reconstruir a cadeia com forte suporte técnico.
