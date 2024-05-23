# Matrix SDK

Matrix (matrix.org) SDK written in dart.

## Native libraries

For E2EE, libolm must be provided.

Additionally, OpenSSL (libcrypto) must be provided on native platforms for E2EE.

For flutter apps you can easily import it with the [flutter_olm](https://pub.dev/packages/flutter_olm) and the [flutter_openssl_crypto](https://pub.dev/packages/flutter_openssl_crypto) packages.

```sh
flutter pub add matrix
flutter pub add flutter_olm
flutter pub add flutter_openssl_crypto
```

## Get started

See the API documentation for details:

```dart
final client = Client("HappyChat");
```

The SDK works better with a database. Otherwise it has no persistence. For this you need to provide a databaseBuilder like this:

```dart
final client = Client(
  "HappyChat",
  databaseBuilder: (_) async {
    final dir = await getApplicationSupportDirectory(); // Recommend path_provider package
    final db = HiveCollectionsDatabase('matrix_example_chat', dir.path);
    await db.open();
    return db;
  },
);
```

3. Connect to a Matrix Homeserver and listen to the streams:

```dart
client.onLoginStateChanged.stream.listen((bool loginState){ 
  print("LoginState: ${loginState.toString()}");
});

client.onEvent.stream.listen((EventUpdate eventUpdate){ 
  print("New event update!");
});

client.onRoomUpdate.stream.listen((RoomUpdate eventUpdate){ 
  print("New room update!");
});

await client.checkHomeserver("https://yourhomeserver.abc");
await client.login(
  identifier: AuthenticationUserIdentifier(user: 'alice'),
  password: '123456',
);
```

4. Send a message to a Room:

```dart
await client.getRoomById('your_room_id').sendTextEvent('Hello world');
```

# Folked supported
This is a forked repository. We are trying to maintain this repo for our vision of our project, but we still need to contribute to the upstream repository, which is a problem that also impacts the community. And we try to keep track of the updates of the upstream repository version to version.

It is the process to keep track with the `main` branch in upstream repository

1. Sync folk `main` branch here with upstream `main`
2. Checkout new branch `twake-supported-0.x.x` from latest main
3. Cherry pick and resolve confliction from the previous `twake-supported-0.y.z`
