# IR-001 — Troubleshooting VirtualBox / USB / ADB

## Objetivo inicial

A primeira abordagem tentou usar uma Kali Linux em VirtualBox para ligar o ZTE por USB e executar ADB.

## Estado observado na VM

`lsusb` mostrou inicialmente apenas dispositivos virtuais/básicos, incluindo:

```text
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 002 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub
Bus 002 Device 002: ID 80ee:0021 VirtualBox USB Tablet
```

O ZTE não aparecia corretamente entregue à VM.

## Tentativas

Foram efetuadas tentativas de:

- seleção/captura do dispositivo USB no VirtualBox;
- alteração do modo USB no telefone;
- utilização das opções de USB apresentadas pelo Android;
- repetição da ligação;
- validação dentro da Kali VM.

Em determinada fase apareceram múltiplas entradas/opções USB, mas a ligação ADB funcional não ficou estabelecida de forma fiável.

## Problema operacional

O USB passthrough do VirtualBox introduziu uma camada adicional de instabilidade e impediu uma baseline forense limpa.

Persistir no troubleshooting da VM aumentaria tempo e risco de alterações no dispositivo sem ganho forense.

## Decisão

A investigação passou para **Kali Linux nativo no MSI**, eliminando a camada VirtualBox/USB passthrough.

Esta decisão foi deliberada:

```text
telefone
   ↓ USB direto
Kali nativo
   ↓
ADB
```

em vez de:

```text
telefone
   ↓
Windows host
   ↓ USB passthrough
VirtualBox
   ↓
Kali VM
   ↓
ADB
```

## Resultado

No Kali nativo, ADB tornou-se a interface operacional principal e permitiu prosseguir com PackageManager, AppOps, UsageStats, bugreport, preservação dos APKs, hashing e contenção.

## Lição

Quando o objetivo é aquisição/IR Android e o passthrough USB da VM não é estável, reduzir camadas de virtualização pode ser uma decisão de integridade operacional, não apenas de conveniência.

> Nota: os outputs retidos permitem provar a falha de enumeração inicial da VM, mas não preservam uma sequência completa de todos os cliques/configurações VirtualBox efetuados. Não se inventa essa sequência.
