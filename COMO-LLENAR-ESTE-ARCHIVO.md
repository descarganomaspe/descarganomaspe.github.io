# assetlinks.json — para que el APK abra sin barra de direcciones

Este archivo es la "huella" que le dice a Android:
**esta app y este sitio son del mismo dueño.**

Sin él, el APK abre igual, pero con la barra del navegador arriba.
Con él, abre a pantalla completa, como una app de verdad.

## Qué falta

Falta un dato que todavía no existe: la **huella SHA-256 de la llave
de firma** del APK. Esa llave se genera una sola vez y se guarda para
siempre (si se pierde, la app no se puede volver a actualizar nunca).

## Cómo se llena

Por cada app que se empaquete, se agrega un bloque como este dentro
de los corchetes de `assetlinks.json`:

```json
{
  "relation": ["delegate_permission/common.handle_all_urls"],
  "target": {
    "namespace": "android_app",
    "package_name": "pe.descarganomas.obrasst",
    "sha256_cert_fingerprints": ["AQUI VA LA HUELLA SHA-256"]
  }
}
```

Si son varias apps, van separadas por coma:

```json
[
  { ...obrasst... },
  { ...detective... }
]
```

## De dónde sale la huella

- **Si se publica en Google Play:** Play Console ▸ Release ▸ Setup ▸
  *App signing*. Ahí sale el "SHA-256 certificate fingerprint" ya listo
  para copiar. Este es el que vale cuando la app se baja de la tienda.
- **Si se reparte el APK a mano (WhatsApp):** sale de la llave propia:
  `keytool -list -v -keystore mi-llave.keystore -alias descarganomas`

Se pueden poner **las dos huellas** en la misma lista: la de Play y la
propia. Así funciona tanto la app de la tienda como la que se reparte
por WhatsApp.

## Nombres de paquete sugeridos

| App                  | package_name                       |
|----------------------|------------------------------------|
| OBRASST              | pe.descarganomas.obrasst           |
| Detective            | pe.descarganomas.detective         |
| Contrarreloj         | pe.descarganomas.contrarreloj      |
| Solo Nosotros        | pe.descarganomas.solonosotros      |
| Mi Iglesia           | pe.descarganomas.miiglesia         |
| Talentos de Fe       | pe.descarganomas.talentosdefe      |
| 100 Preguntas        | pe.descarganomas.cienpreguntas     |
| 50 Preguntas         | pe.descarganomas.cincuentapreguntas |

Una vez elegido, **el nombre de paquete no se cambia nunca**: es la
identidad de la app en el celular.

## Cómo comprobar que quedó bien

1. Que el archivo se vea en el navegador:
   https://descarganomaspe.github.io/.well-known/assetlinks.json
2. Con la herramienta de Google:
   https://developers.google.com/digital-asset-links/tools/generator
