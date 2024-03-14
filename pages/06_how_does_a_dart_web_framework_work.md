# How does a Dart web frame-_work_?

<v-clicks depth="2">

- Most implementations use a `BuildContext` for keeping state/info around a widget/component
    - Used to navigate the widget tree
    - Often used to recursively render a Dart widget tree into an HTML representation
- Widgets will depend on a separate render method that will turn the `Widget.build()` output in HTML elements
- Stateful widgets will often rely on separate classes (`State`, `StateWidget` etc.)
    - `setState` will then rebuild the whole child tree (_naive implementation_)

</v-clicks>


<!--
As a consensus of most web frameworks out there, there are some similarities that repeat themselves.  

The BuildContext is a very common implementation detail, that is used for navigating up the widget tree (and sometimes also downwards).  
It allows to keep information about the related HTML elements and prepares the tree to share information between instances for better rebuilding etc.  

When building Widgets we want to have the comfort to only return our business logic `widgets` and not actual HTML Elements.  
Therefore most frameworks will abstract away methods like `render`/`inflate`/`hydrate`.  
These methods take care of linking the widgets in the widget/context tree, `build`ing the widget and returning a renderable HTML element.

To allow our applications to keep state we utilise `StatefulWidget`s that - through their `setState` method - rebuild the widgets that are affected by such a call.

-->