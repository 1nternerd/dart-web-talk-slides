---
layout: two-col-one-header
preload: false
class: h-full w-full px-4
---
::header::
# Rendering a Dart Web app

::default::

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
// root hydration

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

```dart {all|4|6|8-10|11|13|all}
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

```dart {all|6|7-12|13-17|18-20|all}
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
    }

    context.element = el;
    return context.element!;
  }

```

```dart {all|8|10|12-15|16-18|all}
// stateful components

abstract class State<T extends StatefulWidget> {
  final BuildContext context;

  Widget get widget => context.widget! as T;

  State(this.context);

  Widget build(BuildContext context);

  void setState(VoidCallback callback) {
    // ...
  }

  void render(HtmlElement child) {
    widget.render(context, [child]);
  }
}

```

```dart {all|7|9-13|all}
// stateful components

abstract class State<T extends StatefulWidget> {
  // ...

  void setState(VoidCallback callback) {
    callback.call();

    final child = build(context);
    final childCtx = BuildContext.fromParent(context);
    context.child = childCtx;
    final c = child.hydrate(childCtx);

    return render(c);
  }
}

```


````

<!--
## Root render loop

Now when we want to actually render an HTML document from Dart we need to have a thought about how we can build a DOM tree - being a recursive datastructure - and at some point append the rendered HTMLOutput to the browsers actual DOM.  

We do that by actually building the root widget, and using the aforementioned hydration/inflation methods we can recursively traverse our `Widget` class implementations telling each widget to build itself, hydrate and render itself. (with the given children)  
This is basically everything that we need to have an initial static render of an arbitrarily deep HTML app.

## Widget hydration/inflation

The widget hydration works pretty much the same as the initial root render, but it is handed a BuildContext (which is already the _hydrating widgets_ context). When you hydrate your children make sure that you hand them a childContext and link the parent and children widgets in the BuildContext tree.

Other than that, the loop is pretty much the same as the initial render.

## Rendering widgets

Now rendering a widget actually involves the DOM present in Darts' runtime.  
We access the document we live in and call `createElement` (which maps directly to JSs' `createElement` method). From there on we proceed to append the given children and at the end return our own - now - Node (to allow the parent widget to append our output)

Note: for rendering actual HTMLElements we want to add listeners and attributes to the Dart types, so the render method there will look a little different.

## Setting state

To make our stuff somehwat interactive, we need some state handling, which allow rebuilds of our UI with new data.  

In the most naive case you will end up with an implementation that is basically a hydration call from within a widget tree element, rebuilding _all_* its children and replacing itself in the DOM.

*= That can get very expensive renderwise, so tracking which components actually need to rebuild would be crucial for a production-grade framework.
-->