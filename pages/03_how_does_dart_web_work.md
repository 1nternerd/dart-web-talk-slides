# How does Dart Web work?

- it actually compiles to Javascript
  - there is no need for a full fledged VM
- You can access the DOM via `dart:html`&emsp;<span class="font-size-3">(or `package:web` for Dart &ge; 3.3.0 )</span>
- Then you can append _widgets_ rendered into Darts' `HtmlElement` via familiar Javascript methods

Example:

<div style="position:relative">

<div class="absolute top-1 right-1 bg-black/30 rounded-2 px-2 py-1 z-100 font-size-3 text-gray-900">
    Dart {{ $clicks < 1 ? "&lt;" : "&ge;" }} 3.3.0
</div>

````md magic-move
```dart
// main.dart
import 'dart:html';

void main() {
  final root = document.getElementById("#app"); // or document.body

  final app = DivElement()..text = "Hello World";

  root.append(app);
}
```

```dart
// main.dart
import 'package:web/web.dart';

void main() {
  final root = document.getElementById("#app"); // or document.body

  final app = HTMLDivElement()..text = "Hello World";

  root.append(app);
}
```
````

</div>

<!--
Dart Web had some issues handling event listeners on elements.  
If you use the standard element.addEventListener method you won't be able to remove these listeners again.
  - It seemingly creates weird memory references, thus JS does not know which method to remove as event listener
  - Alternatively: You will need to listen to the event stream of the desired event

`dart:html` will likely get deprecated in Dart 3.4.0; projects scaffolded with Dart 3.3.0 or bigger will already use `package:web`

-->
