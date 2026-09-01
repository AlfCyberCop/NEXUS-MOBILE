# IR-001 — Evidence Manifest

> A evidência binária principal permanece preservada localmente. Este repositório publica apenas metadados e hashes sanitizados.

| Artefacto | SHA-256 | Estado / nota |
|---|---|---|
| `dzxl-base.apk` | `e5259b0c70aba6b9427719a18bf5f5354d37422cc4119e095bd3963b3fd7aeab` | Payload preservado |
| `telo-base.apk` | `f5be3063dabe7cab0f5b6dbac8cbcead3983fe6aa3878677ad4b0cd031ac890a` | Dropper preservado |
| `dzxl-download.apk` | `f5be3063dabe7cab0f5b6dbac8cbcead3983fe6aa3878677ad4b0cd031ac890a` | APK residual de Download; conteúdo igual ao dropper |
| `appops-dzxl.txt` | `05fcf53608d0e224cd7812208af56657e423600097af07a6aae415ac534c2d0e` | Estado AppOps preservado |
| `appops-telo.txt` | `7acc6e2ca3d4f80cdd46301664c547fa762f9b678116dfcc7b76de000147bfba` | Estado AppOps preservado |
| `zte-before-cleanup.zip` | `515e99eb2b9f2a817f59caac48e00642b3b0c16429295b95b472c044295be4c8` | Bugreport pré-cleanup |

## Certificados

### `io.telo.link`

- Subject: `C=US, O=Android, CN=Android Debug`
- Cert SHA-256: `a4ce771d988fb94c4bf852846de675fb60ea3f65bf08bd690a9de05b913bb38f`

### `dzxl.wpdzac.fnmrmay`

- Subject: `CN=editor`
- Cert SHA-256: `6215f00baa4bf18bab5792fc796bfc5555917240f14f7c7e672d956888d75c96`

Os APKs usam chaves diferentes, compatível com um dropper que instala um payload assinado separadamente.

## Cadeia de custódia operacional

- O APK residual em `Download` foi copiado para Kali antes de ser eliminado do telefone.
- Os hashes foram calculados antes da remoção.
- AppOps e bugreport foram preservados antes do cleanup.
- Não publicar artefactos binários sem nova revisão de privacidade.
