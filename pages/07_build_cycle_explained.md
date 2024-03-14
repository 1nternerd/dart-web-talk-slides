---
layout: two-cols
preload: false
class: h-full w-full px-4
---

# How does it render?

<div class="relative" v-if="$clicks < 7 || $clicks >= 14">

  <div
    class="bg-blue-400 rounded-sm color-white px-4 py-3 inline-block"
    v-if="$clicks >= 2"
    v-motion
    :initial="{ x: -80 }"
    :enter="{ x: 0 }">
    Example app
  </div>

  <div
    class="bg-white border-2 border-gray-200 border-dashed text-center rounded-sm color-gray-700 px-4 py-2 inline-block ml-10 absolute"
    v-if="$clicks >= 3"
    v-motion
    :initial="{ x: -80 }"
    :enter="{ x: 0 }">
    BuildContext <br>
    <span class="text-xs">(Example app)</span>
  </div>

  <div
    class="bg-green-500 border-2 border-green-600 text-center color-white rounded-sm px-4 py-2 inline-block ml-10 absolute"
    v-if="$clicks >= 4"o
    v-motion
    :initial="{ y: -40, x:-80}"
    :enter="{ y: 100, x:-80}">
    child <br>
    <em class="text-xs">is HTMLElement</em>
  </div>

  <div
    class="bg-green-500 border-2 border-green-600 text-center color-white rounded-sm px-4 py-2 inline-block ml-10 absolute"
    v-if="$clicks >= 5"
    v-motion
    :initial="{ y: 100, x:-80}"
    :enter="{ y: 220, x:-80}">
    appElement <br>
    <em class="text-xs">is HTMLElement</em>
  </div>

  <div
    class="bg-amber-500 border-2 border-amber-600 text-amber-900 color-white rounded-sm px-4 py-2 inline-block ml-10 absolute"
    v-if="$clicks >= 6"
    v-motion
    :initial="{ y: 200, x:-90}"
    :enter="{ y: 350, x:-90}">
    Browser DOM
  </div>

  <Arrow v-if="$clicks >= 3" v-bind="{ x1:180, y1:30, x2:130, y2:30 }" />

  <Arrow v-if="$clicks >= 4" v-bind="{ x1:130, y1:50, x2:150, y2:100 }" />
  <Arrow v-if="$clicks >= 4" v-bind="{ x1:180, y1:50, x2:160, y2:100 }" />

  <Arrow v-if="$clicks >= 5" v-bind="{ x1:160, y1:170, x2:160, y2:220 }" />

  <Arrow v-if="$clicks >= 6" v-bind="{ x1:160, y1:290, x2:160, y2:350 }" />

</div>

<div v-if="$clicks >= 7 && $clicks < 14">

```dart
void runApp(
    Widget Function(Map<String, String>) builder,
    Element? target
) {
    target ??= document.getElementById("app");

    final app = builder(target.dataset);
    final rootContext = BuildContext();
    final child = app.hydrate(rootContext);
    final appElement = app.render(rootContext, [child]);

    target.append(appElement);
}
```

</div>

::right::

````md magic-move
```dart
// main.dart

void main() {
    runApp(() => ExampleApp());
}

```

```dart {11|13|14|15|16|all}
// main.dart

void main() {
    runApp(() => ExampleApp());
}

void runApp(
    Widget Function(Map<String, String>) builder,
    Element? target
) {
    target ??= document.getElementById("app");

    final app = builder(target.dataset);
    final rootContext = BuildContext();
    final child = app.hydrate(rootContext);
    final appElement = app.render(rootContext, [child]);

    target.append(appElement);
}

```

```dart {all|5|7|9-11|12|14|all}
// stateless hydration


HtmlElement hydrate(BuildContext context) {
    context.widget = this;

    final child = build(context);

    final childCtx = BuildContext.fromParent(context);
    context.child = childCtx;
    
    final c = child.hydrate(childCtx);

    return render(context, [c]);
}

```

```dart {all|6|7-12|13-18|19-21|all}
// widget rendering

HtmlElement render(
    BuildContext context, 
    [List<HtmlElement>? children]) {
    final el = document.createElement(tagName);

    if (children != null) {
      for (var child in children) {
        el.append(child);
      }
    }

    if (context.element != null && 
        context.element?.parentNode != null) {
      context.element!.replaceWith(el);
      return context.element!;
    }

    context.element = el;
    return context.element!;
  }

```


````