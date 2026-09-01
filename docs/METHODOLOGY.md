# Metodologia NEXUS MOBILE — Incident Response Android

## Regra principal

**Preservar antes de remover.**

Uma aplicação suspeita não deve ser desinstalada antes de recolher, quando possível:

- identidade e proveniência via PackageManager;
- timestamps de instalação/atualização;
- permissões e AppOps;
- processos, serviços e jobs;
- UsageStats;
- APKs instalados e artefactos residuais;
- bugreport;
- hashes;
- elementos necessários à análise estática.

## Estados de conhecimento

A documentação distingue sempre:

- **Evidência runtime:** o sistema registou utilização/execução.
- **Capacidade estática:** o código implementa determinada função.
- **Inferência:** conclusão suportada por várias evidências, mas não observação direta.
- **Não demonstrado:** existe possibilidade técnica, mas falta prova da execução concreta.

## Limites do ADB sem root

`adb shell` opera com as permissões do utilizador Android `shell`. Não fornece acesso irrestrito ao armazenamento privado de todas as aplicações e continua sujeito ao sandbox Android e SELinux.

## Privacidade

Os relatórios publicados devem omitir:

- número de série;
- contas e credenciais;
- mensagens, fotos e conteúdo privado;
- identificadores pessoais desnecessários.

Preservam-se IOCs e metadados técnicos necessários à análise.
