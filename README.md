# NEXUS MOBILE

Investigação, análise forense e resposta a incidentes em dispositivos móveis.

> **Estado do repositório:** privado.
> **Princípio:** preservar evidência antes de remover; documentar durante a investigação; publicar apenas conteúdo sanitizado.

## Casos

| ID | Dispositivo | Classificação | Estado |
|---|---|---|---|
| [IR-001](incident-response/IR-001-ZTE-BLADE-A56/) | ZTE Blade A56 / Z2473 / P606F21 | Compromisso Android confirmado ao nível de aplicação | Encerrado / documentação publicada |

## IR-001 — resumo

A investigação confirmou uma cadeia de sideload iniciada no browser:

`Chrome → Android Package Installer → io.telo.link → dzxl.wpdzac.fnmrmay`

- `io.telo.link`: dropper/downloader/instalador auxiliar.
- `dzxl.wpdzac.fnmrmay`: payload com `AccessibilityService` abusivo e persistência ao nível de aplicação.
- Não foi encontrada evidência de root, bootloader desbloqueado, firmware adulterado ou instalação como aplicação de sistema.
- A evidência foi preservada antes da contenção.
- Após desativação, remoção, reboot e validação, não foi observada persistência imediata.

A documentação detalhada encontra-se em [`incident-response/IR-001-ZTE-BLADE-A56`](incident-response/IR-001-ZTE-BLADE-A56/).

## Política de evidência

Este repositório **não deve conter** dados pessoais desnecessários, credenciais, número de série, conteúdo privado do proprietário ou outros identificadores não necessários à investigação.

APKs e bugreports podem conter informação sensível. O GitHub mantém apenas documentação sanitizada, hashes, indicadores e referências aos artefactos preservados localmente.

## Metodologia

1. Isolar e estabilizar.
2. Criar baseline antes de alterar.
3. Recolher evidência com operações preferencialmente read-only.
4. Preservar artefactos e calcular hashes.
5. Separar capacidade estática, evidência runtime e inferência.
6. Só depois conter/remover.
7. Reiniciar e validar persistência.
8. Documentar falsos positivos e limitações.

**NEXUS / Purple Team**
