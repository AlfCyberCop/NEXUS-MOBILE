# IR-001 — Análise de `dzxl.wpdzac.fnmrmay`

## Classificação

**Payload Android malicioso com AccessibilityService abusivo.**

## Metadados

- package: `dzxl.wpdzac.fnmrmay`
- versionCode: `238134`
- versionName: `23.81.34 kvvmfuawrungbcznij`
- targetSdk: `34`
- firstInstallTime: `2026-08-20 19:39:58`
- installer: `io.telo.link`

## Componentes relevantes

Foram identificados:

- `BootReceiver`;
- `ResetServices`;
- `DreamService`;
- `AccessibilityService` obfuscado.

## Permissões/capacidades solicitadas

Entre outras:

- `READ_SMS`;
- `CAMERA`;
- `POST_NOTIFICATIONS`;
- `MANAGE_EXTERNAL_STORAGE`;
- `RECEIVE_BOOT_COMPLETED`;
- `WAKE_LOCK`.

## AccessibilityService

A análise estática mostrou:

- event types extremamente abrangentes;
- ausência de limitação a package names específicos;
- leitura de eventos, classes, texto e notificações;
- interação com `AccessibilityNodeInfo`;
- procura e click de nós;
- `GestureDescription`;
- `performGlobalAction` para BACK, HOME e RECENTS;
- código para `takeScreenshot`;
- WebViews com JavaScript interfaces e opções permissivas de acesso a ficheiros;
- rotinas obfuscadas de tap/back;
- `ProcessBuilder`;
- código relacionado com eliminação de raw rows de contactos.

Estas são **capacidades implementadas**. Não devem ser convertidas automaticamente em afirmações sobre ações concretamente executadas.

## Evidência runtime via AppOps

Foram observados:

```text
BIND_ACCESSIBILITY_SERVICE = allow
ACCESS_ACCESSIBILITY = allow
CREATE_ACCESSIBILITY_OVERLAY = allow
```

Além disso:

- `START_FOREGROUND`;
- `WAKE_LOCK`;
- sessão aproximada de quatro minutos.

`SYSTEM_ALERT_WINDOW` não estava autorizado, mas o overlay de acessibilidade estava.

### Importante

- `CAMERA` em modo foreground = autorização/modo, **não prova captura**.
- `MANAGE_EXTERNAL_STORAGE` = autorização/uso, **não identifica os ficheiros acedidos**.
- `READ_SMS` permaneceu `ignore`; **não foi demonstrado acesso runtime a SMS**.

## UsageStats

Foram observadas execuções repetidas em 26, 28, 29 e 30 de agosto, com múltiplos:

- `ACTIVITY_RESUMED`;
- foreground services;
- fluxos relacionados com permissões;
- app launches.

Em `2026-08-30 15:37` foram iniciados pelo menos dois fluxos de `GrantPermissionsActivity`.

## Conclusão

A capacidade de Accessibility abusiva não ficou apenas no código: AppOps confirma que o serviço foi efetivamente ligado/utilizado. Contudo, os limites de auditoria do Android impedem afirmar que todas as capacidades presentes no código foram executadas sobre dados reais.
