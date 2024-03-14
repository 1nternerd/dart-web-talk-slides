---
layout: two-cols
class: px-2
clicks: 3
---

# Dart Web frameworks

There is already a variety of web frameworks written in Dart out there.  

<div class="grid grid-cols-1 gap-2">
    <a class="link-card" href="https://github.com/schultek/jaspr" v-show="$clicks >= 0">
        <h3>Jaspr</h3>
        <span>Flutter-like web framework for SPAs</span>
    </a>
    <a class="link-card" href="https://github.com/Blimster/deact" v-show="$clicks >= 1">
        <h3>Deact</h3>
        <span>React inspired Dart framework</span>
    </a>
    <a class="link-card" href="https://github.com/famedly/wupper" v-show="$clicks >= 2">
        <h3>Wupper</h3>
        <span>OG Flutter inspired Web Framework</span>
    </a>
    <a class="link-card" v-show="$clicks >= 3">
        <h3>Flart</h3>
        <span>Wupper knock-off implementation for this talk</span>
    </a>
</div>

::right::

<div style="--slidev-code-font-size: 10px; --slidev-code-line-height: 14px;">

````md magic-move
```dart
import 'package:jaspr/jaspr.dart';

void main() => runApp(App());

class App extends StatefulComponent {
  const App({super.key});

  @override
  State<App> createState() => _AppState();
}

class _AppState extends State<App> {
  int count = 0;

  @override
  Iterable<Component> build(BuildContext context) sync* {
    yield text('Count is $count');

    yield button(
      onClick: () {
        setState(() => count++);
      },
      [text('Press Me')],
    );
  }
}
```

```dart
import 'package:deact/deact.dart';
import 'package:deact/deact_html52.dart';

void main() => deact(
    '#root',
    (_) => globalState<int>(
        name: 'counter',
        initialValue: 0,
        children: [
            incrementor(),
            display(),
        ],
    ));

DeactNode incrementor() => fc((ctx) {
      final counter = ctx.globalState<int>('counter');
      return button(
          onclick: (_) => counter.set((c) => c + 1),
          children: [txt('Click me to increment to counter')]);
    });

DeactNode display() => fc((ctx) {
      final counter = ctx.globalState<int>('counter');
      return div(children: [txt('Counter: ${counter.value}')]);
    });
```

```dart
import "package:wupper/wupper.dart";

void main() => runApp((Map<String, String> args) => CounterPage());

class CounterPage extends StatefulWidget {
  const CounterPage({Key? key, }): super(key: key);
  
  @override
  StateWidget<CounterPage> createState() => CounterPageState();
}

class CounterPageState extends StateWidget<CounterPage>{
  int count = 0;

  @override
  Widget build(BuildContext context) {
    return DivElementWidget(
      children: [
        HeadingElementH1Widget(text: title),
        ParagraphElementWidget(text: 'Counter: $count')),
        ButtonElementWidget(
          text: 'Counter +',
          onClick: (_) => setState((){ count++; }) ,
        ),
      ],
    );
  }
}
```

```dart
import 'dart:html';
import 'package:flart/flart.dart';

void main() => runApp((Map<String, String> args) => Counter());

class Counter extends StatefulWidget {
  @override
  State<Counter> createState(BuildContext context) => CounterState(context);
}

class CounterState extends State<Counter> {
  int _count = 0;

  CounterState(super.context);

  @override
  Widget build(BuildContext context) {
    return El<DivElement>(
      children: [
        El<DivElement>(
          children: [
            El<DivElement>(
              attributes: { "text": "Count: $_count", },
            ),
            El<ButtonElement>(
              attributes: { "text": "Increment", },
              listeners: { "click": (ev) => setState(() { _count++; }), },
            ),
          ],
        ),
      ],
    );
  }
}
```
````

</div>


<style>
.link-card {
    --uno: bg-gray-200 rounded-2 p-4 border-0 hover:text-gray-600;
}

.link-card h3 {
    --uno: font-size-4 font-bold line-height-2;
}

.link-card span {
    --uno: font-size-3 text-gray-500;
}
</style>
