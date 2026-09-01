# NEXUS MOBILE INCIDENT RESPONSE #001

## ZTE Blade A56 — Android Malware Forensics

**Classificação:** `COMPROMISSO ANDROID CONFIRMADO AO NÍVEL DE APLICAÇÃO`
**Estado:** encerrado, com evidência principal preservada e validação pós-remediação concluída.

## Resumo executivo

A investigação confirmou uma cadeia de instalação maliciosa ao nível de aplicações:

```text
Chrome
  ↓
ficheiro APK / sideload
  ↓
Android Package Installer
  ↓
io.telo.link
  ↓
PackageInstaller
  ↓
dzxl.wpdzac.fnmrmay
```

`io.telo.link` funcionava como dropper/downloader e instalador auxiliar. O payload `dzxl.wpdzac.fnmrmay` implementava um `AccessibilityService` muito abrangente, capacidade de gestos, ações globais, screenshots, overlays de acessibilidade e outros mecanismos de automação abusiva da interface.

A atividade não ficou apenas no plano potencial: AppOps demonstrou que o `AccessibilityService` foi ligado/utilizado e que foi criado `accessibility overlay`. UsageStats mostrou execução recorrente do payload ao longo de vários dias.

Não foi encontrada evidência de:

- root;
- bootloader desbloqueado;
- firmware comprometido;
- instalação como aplicação de sistema.

Depois da preservação de evidência, os dois pacotes foram parados, desativados e removidos. Após reboot, não foi observada persistência imediata.

## Baseline

| Campo | Valor |
|---|---|
| Dispositivo | ZTE Blade A56 |
| Identificadores de modelo | Z2473 / P606F21 |
| Android | 15 |
| API | 35 |
| Security patch | 2026-06-05 |
| Verified Boot | green |
| Bootloader | bloqueado |
| Interface forense principal | ADB a partir de Kali Linux |

A investigação começou em Safe Mode e foi posteriormente validada em modo normal.

## Aplicações maliciosas

### `io.telo.link`

- versionCode: `1`
- versionName: `1.0`
- targetSdk: `35`
- firstInstallTime: `2026-08-20 19:39:25`
- installerPackageName: `com.google.android.packageinstaller`
- originatingPackageName: `com.android.chrome`

### `dzxl.wpdzac.fnmrmay`

- versionCode: `238134`
- versionName: `23.81.34 kvvmfuawrungbcznij`
- targetSdk: `34`
- firstInstallTime: `2026-08-20 19:39:58`
- installerPackageName: `io.telo.link`

O payload foi instalado **33 segundos** depois do dropper.

## IOCs principais

| Tipo | Indicador |
|---|---|
| Package | `io.telo.link` |
| Package | `dzxl.wpdzac.fnmrmay` |
| IPv4 | `212.69.5.181` |
| Endpoint | `http://212.69.5.181/p/req` |
| Payload slug | `c4chfmzw4d` |
| SHA-256 dropper | `f5be3063dabe7cab0f5b6dbac8cbcead3983fe6aa3878677ad4b0cd031ac890a` |
| SHA-256 payload | `e5259b0c70aba6b9427719a18bf5f5354d37422cc4119e095bd3963b3fd7aeab` |

## Navegação

- [Relatório técnico completo](analysis/FULL-REPORT.md)
- [Cadeia de instalação](analysis/INSTALLATION-CHAIN.md)
- [Dropper `io.telo.link`](analysis/DROPPER-IO-TELO-LINK.md)
- [Payload `dzxl.wpdzac.fnmrmay`](analysis/PAYLOAD-DZXL.md)
- [ADB e recolha forense](adb/ADB-FORENSICS.md)
- [Análise JADX](jadx/STATIC-ANALYSIS.md)
- [Manifesto de evidência](evidence/EVIDENCE-MANIFEST.md)
- [Timeline](timeline/TIMELINE.md)
- [Findings e conclusões](findings/FINDINGS.md)
- [Remediação e validação](findings/REMEDIATION-VALIDATION.md)
- [Troubleshooting VirtualBox/USB/ADB](troubleshooting/VIRTUALBOX-USB-ADB.md)

## Nota de privacidade

Este caso está documentado de forma sanitizada. Não são publicados número de série, dados pessoais, credenciais nem conteúdo privado do proprietário.
