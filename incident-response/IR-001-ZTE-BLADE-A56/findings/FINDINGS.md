# IR-001 — Findings

## F-001 — Compromisso Android ao nível de aplicação

**Severidade:** Alta
**Confiança:** Alta

A combinação de PackageManager provenance, hashes, timestamps e código demonstra uma cadeia de sideload maliciosa.

## F-002 — Dropper com instalação de payload remoto

**Package:** `io.telo.link`
**Confiança:** Alta

O componente implementa download, configuração remota, PackageInstaller e instalação do payload.

## F-003 — AccessibilityService abusivo utilizado em runtime

**Package:** `dzxl.wpdzac.fnmrmay`
**Severidade:** Alta
**Confiança:** Alta

AppOps registou `BIND_ACCESSIBILITY_SERVICE`, `ACCESS_ACCESSIBILITY` e `CREATE_ACCESSIBILITY_OVERLAY` em estado `allow`.

## F-004 — Execução recorrente do payload

**Confiança:** Alta

UsageStats registou múltiplos launches/activities/foreground services durante vários dias.

## F-005 — Persistência ao nível de aplicação

O código inclui componentes de boot e serviços. Contudo, após remoção/reboot não foi observada persistência residual.

## F-006 — Infraestrutura remota de staging/download

**IOC:** `212.69.5.181`
**Endpoint:** `http://212.69.5.181/p/req`

A infraestrutura suportava entrega/configuração do dropper. Não se afirma atribuição nem C2 completo sem evidência adicional.

## Não demonstrado

A investigação **não demonstrou**:

- roubo de credenciais bancárias;
- leitura efetiva de SMS;
- captura concreta de screenshots;
- root;
- compromisso de firmware;
- bootloader adulterado;
- atribuição a ator específico.

## Falsos positivos fechados

- Photo Vault: sem ligação demonstrada.
- Photo Recovery: cronologia incompatível com a cadeia principal.
- `com.preff.kb.zx`: teclado predefinido; hit por substring genérica.
- Facebook em jobs: resultado de regex demasiado ampla.
- `.iGallery`: não classificado sem prova.
- `sd_logs`: tratado como potencial artefacto de fabricante.
