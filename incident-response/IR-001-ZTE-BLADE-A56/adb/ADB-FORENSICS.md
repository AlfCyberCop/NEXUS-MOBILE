# IR-001 — ADB Forensics

ADB foi a interface operacional principal entre Kali Linux e o ZTE.

## Princípio

`adb shell` **não significa root**. Os comandos são executados com as permissões do utilizador Android `shell`, sujeitos ao sandbox e SELinux.

## Identificação e ligação

```bash
adb devices
adb shell
```

## Package paths

```bash
adb shell pm path dzxl.wpdzac.fnmrmay
adb shell pm path io.telo.link
```

Paths observados:

```text
package:/data/app/.../dzxl.wpdzac.fnmrmay-.../base.apk
package:/data/app/.../io.telo.link-.../base.apk
```

## PackageManager

```bash
adb shell dumpsys package io.telo.link
adb shell dumpsys package dzxl.wpdzac.fnmrmay
```

Campos especialmente úteis:

- `versionCode`
- `versionName`
- `firstInstallTime`
- `lastUpdateTime`
- `installerPackageName`
- `originatingPackageName`
- requested/granted permissions
- receivers/services
- data/code paths

## AppOps

```bash
adb shell appops get io.telo.link
adb shell appops get dzxl.wpdzac.fnmrmay
```

Exemplos relevantes observados para o dropper:

```text
REQUEST_INSTALL_PACKAGES: allow
ESTABLISH_VPN_SERVICE: allow
ACTIVATE_VPN: allow
```

No payload, AppOps confirmou operações de Accessibility e overlay de acessibilidade.

## UsageStats

```bash
adb shell dumpsys usagestats
```

Foi usado para demonstrar execução recorrente e fluxos de permissões.

## Processos, serviços e jobs

```bash
adb shell dumpsys activity processes
adb shell dumpsys activity services
adb shell dumpsys jobscheduler
```

## Armazenamento partilhado

`/sdcard` resolveu para:

```text
/storage/emulated/0
```

O APK residual foi encontrado sob `Download`.

## Preservação

Exemplo de fluxo:

```bash
mkdir -p ~/zte-investigation/apk-evidence
adb pull <path-do-apk> ~/zte-investigation/apk-evidence/
sha256sum ~/zte-investigation/apk-evidence/*
```

## Contenção

A contenção ocorreu apenas após preservação:

```bash
adb shell am force-stop dzxl.wpdzac.fnmrmay
adb shell pm disable-user --user 0 dzxl.wpdzac.fnmrmay
adb shell am force-stop io.telo.link
adb shell pm disable-user --user 0 io.telo.link
```

## Remoção

```bash
adb shell pm uninstall --user 0 dzxl.wpdzac.fnmrmay
adb shell pm uninstall --user 0 io.telo.link
```

Ambos os `uninstall` devolveram `Success`.

## Validação

Após reboot:

```bash
adb shell pm list packages | grep -E 'io\.telo\.link|dzxl\.wpdzac\.fnmrmay'
adb shell settings get secure enabled_accessibility_services
adb shell settings get secure accessibility_enabled
```

Resultado observado:

```text
enabled_accessibility_services = null
accessibility_enabled = 0
```

Nenhum dos dois packages voltou a aparecer.
