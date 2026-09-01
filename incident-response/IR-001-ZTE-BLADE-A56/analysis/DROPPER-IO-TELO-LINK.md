# IR-001 — Análise de `io.telo.link`

## Classificação

**Dropper / downloader / instalador auxiliar.**

## Metadados

- package: `io.telo.link`
- versionCode: `1`
- versionName: `1.0`
- targetSdk: `35`
- firstInstallTime: `2026-08-20 19:39:25`
- installer: `com.google.android.packageinstaller`
- originating package: `com.android.chrome`

## Componentes relevantes

O PackageManager mostrou:

- `MainActivity` capaz de abrir APKs (`application/vnd.android.package-archive`);
- `BootReceiver`;
- `AppUpdatedReceiver`;
- `NullVpnService` com `android.permission.BIND_VPN_SERVICE`.

## AppOps

Foram observados, entre outros:

```text
REQUEST_INSTALL_PACKAGES: allow
ESTABLISH_VPN_SERVICE: allow
ACTIVATE_VPN: allow
```

`ESTABLISH_VPN_SERVICE` registou aproximadamente `19 s` de duração.

## Infraestrutura remota

Recursos/configuração analisados:

```text
server_url = http://212.69.5.181
payload_slug = c4chfmzw4d
target_pkg = dzxl.wpdzac.fnmrmay
```

Endpoint:

```text
http://212.69.5.181/p/req
```

O cliente:

1. derivava um identificador a partir de `ANDROID_ID + Build.FINGERPRINT`;
2. calculava HMAC;
3. enviava JSON por HTTP;
4. recebia duas URLs/campos designados `c` e `b`;
5. tratava `c` como configuração;
6. tratava `b` como APK;
7. escrevia `base.apk` numa `PackageInstaller session`;
8. fazia commit para `InstallReceiver`.

A existência do endpoint demonstra infraestrutura remota de entrega/staging. Não é classificada como C2 completo sem evidência adicional sobre semântica de comando e controlo.

## VPN local

`NullVpnService`:

- cria uma interface TUN;
- endereço por defeito `10.8.0.1/32`;
- adiciona rota `0.0.0.0/0`;
- MTU `1500`;
- lê pacotes para um buffer e descarta-os;
- exclui o próprio dropper e uma whitelist da VPN.

O comportamento observado corresponde a uma **black-hole VPN IPv4**.

Não foi encontrada lógica de exfiltração dentro deste serviço. A VPN era iniciada antes da instalação e o fluxo aguardava até cinco segundos pelo estabelecimento da interface.

## Conclusão

`io.telo.link` não era apenas um APK estranho instalado no telefone: a combinação de proveniência, AppOps e análise estática demonstra um componente de entrega preparado para obter configuração/payload remoto e instalar o pacote seguinte.
