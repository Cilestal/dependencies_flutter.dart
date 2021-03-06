# Flutter dependencies

Simple package to ease the use of the [dependencies](https://github.com/Cilestal/dependencies.dart/) with Flutter
leveraging the power of `InheritedWidget`. 

# Installation
In the `dependencies:` section of your `pubspec.yaml`, add the following line:

```yaml
dependencies:
    dependencies_flutter:
    git:
      url: git://github.com/Cilestal/dependencies_flutter.dart
```

## Usage

```dart
class SomeRootWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return InjectorWidget.bind(
      bindFunc: (binder) {
        binder
          ..install(MyModule())
          ..bindSingleton("api123", name: "api_key");
      },
      child: SomeWidget()
    );
  }
}
```

You can also extend `BindingInjectorWidget` to configure your dependencies:

```dart
class MyBinder extends ModuleWidget {
  MyBinder({Key key, @required Widget child}): super(key: key, child: child);
  @override
  void configure(Binder binder) {
    binder
      ..bindSingleton(Object());
  }
}
```

You can later refer to the injector like any other `InheritedWidget`.

```dart
class SomeWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    final injector = InjectorWidget.of(context);
    final apiKey = injector.get(name: "api_key");
    return SomeContainerNeedingTheKey(apiKey);
  }
}
```

Or using the `InjectorWidgetMixin`:

```dart
class SomeWidget extends StatelessWidget with InjectorWidgetMixin {
  @override
  Widget buildWithInjector(BuildContext context, Injector injector) {
    final object = injector.get<Object>();
    print(object);
    return Container();
  }
}
```
