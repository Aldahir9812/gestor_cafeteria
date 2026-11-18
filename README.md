# gestor_cafeteria

App Flutter para gestionar los pedidos de una cafetería. Consume la API REST de
[mini_backend](https://github.com/DMI-2026/mini_backend) mediante un cliente HTTP
(`ApiClient`) que convierte cada respuesta de error en un `Failure` tipado
(`NotFoundFailure`, `ConflictFailure`, `ValidationFailure`, `ServerFailure`,
`NetworkFailure`, `UnexpectedFailure`), y cuenta con pruebas unitarias del cliente.

## Requisitos

- Flutter estable con Dart SDK 3.13.2 (lo que pide `pubspec.yaml`).
- Node.js 22.13 o superior (para ejecutar mini_backend).
- Git.

## Backend: mini_backend

mini_backend vive en **otro repositorio** y no se copia dentro de este proyecto:
<https://github.com/DMI-2026/mini_backend>

Versión con la que se probó: `mini-backend` 0.1.0, commit
[`94d2533`](https://github.com/DMI-2026/mini_backend/commit/94d2533)
(rama `master`, 8 de octubre de 2026), con Node.js 22.14.0.

Para levantarlo (en una carpeta aparte, fuera de este proyecto):

```bash
git clone https://github.com/DMI-2026/mini_backend.git
cd mini_backend
node server.js      # escucha en http://127.0.0.1:8000
```

No necesita `npm install`. Para comprobar que responde: `curl http://localhost:8000/health`.

## Ejecutar la app

```bash
flutter pub get
flutter run
```

Por defecto el cliente apunta a `http://localhost:8000` en web y a
`http://10.0.2.2:8000` en el emulador de Android. Para usar otra URL se pasa
`API_URL` con `--dart-define`:

```bash
flutter run --dart-define=API_URL=http://192.168.1.50:8000
```

## Pruebas

```bash
flutter test
```

Las pruebas de `test/core/http_client_test.dart` usan `MockClient` del paquete
`http`, así que **no** necesitan el backend encendido.

## Estructura de carpetas

```
lib/
  main.dart                 # punto de entrada; consulta GET /orders
  core/
    failure.dart            # jerarquía sellada de errores (Failure)
    http_client.dart        # ApiClient: GET/POST/PATCH/DELETE y mapeo de errores
    token_storage.dart      # almacenamiento seguro del token de sesión
test/
  core/
    failure_test.dart       # pruebas de la jerarquía Failure
    http_client_test.dart   # pruebas del ApiClient con MockClient
android/, web/              # plataformas de destino
```
