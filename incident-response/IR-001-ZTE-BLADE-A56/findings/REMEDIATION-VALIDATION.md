# IR-001 — Contenção, remoção e validação

## Pré-condição

A contenção só foi executada **depois** de preservar:

- APKs;
- hashes;
- AppOps;
- bugreport;
- timestamps e proveniência.

## Contenção

```bash
adb shell am force-stop dzxl.wpdzac.fnmrmay
adb shell pm disable-user --user 0 dzxl.wpdzac.fnmrmay

adb shell am force-stop io.telo.link
adb shell pm disable-user --user 0 io.telo.link
```

Resultado das desativações: `disabled-user`.

## Remoção

```bash
adb shell pm uninstall --user 0 dzxl.wpdzac.fnmrmay
adb shell pm uninstall --user 0 io.telo.link
```

Resultado: `Success` para ambos.

O APK residual em `Download` foi eliminado apenas após `adb pull` e SHA-256.

## Reboot e validação

Depois do reboot:

- `pm list packages` deixou de devolver os dois packages;
- `enabled_accessibility_services = null`;
- `accessibility_enabled = 0`;
- não foram observados serviços/processos/jobs dos dois pacotes;
- não houve reaparecimento imediato após recuperação da rede.

## Avaliação

**Remoção observavelmente bem-sucedida.**

Isto não equivale à garantia superior de um factory reset/reflash. A decisão de não efetuar reset total foi suportada pela ausência de evidência de root, sistema, bootloader ou firmware comprometido.

## Recomendação de risco residual

Devido ao período em que Accessibility esteve operacional, credenciais sensíveis utilizadas no dispositivo durante o incidente devem ser avaliadas com prudência e, quando apropriado, alteradas a partir de um dispositivo limpo.
