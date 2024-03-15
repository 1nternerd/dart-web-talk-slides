---
layout: two-cols
layoutClass: gap-16
clicks: 4
---

# Building a Dart web app
<em class="text-gray-500 font-size-3">applies to Dart &ge; 3.3.0</em>

<ol class="my-2">
    <li>Create a dart web project</li>
    <li v-if="$clicks >= 2">Run <code>dart run build_runner serve</code> or <code>webdev serve</code></li>
</ol>

<div v-click="4" class="bg-gray-200 rounded-2 p-4 mt-8">
Yes it's <strong>that</strong> easy 🎉
</div>




::right::

<div v-if="$clicks <= 2">

````md magic-move
```shell
$ dart create -t web dart-web-example
```

```shell
$ dart create -t web dart-web-example

# results in

dart-web-example/
    .gitignore
    analysis_options.yaml
    CHANGELOG.md
    pubspec.yaml
    README.md
    web/
        index.html
        main.dart
        styles.css
```
````

</div>
<div class="h-full w-full" v-else>
<iframe class="h-full w-full" src="https://dartpad.dev/?id=0d47c7c36a6711660608921954f54919&theme=light&run=true">
</iframe>

</div>

<!--
`dart create` works similarly to `flutter create` and has a `-t/--template` flag.  

Note: you will likely need the `build_web_compilers` pacakge in order for the build_runner to be able to build actual JS from your dart source.

```yaml
dependencies:
  build_runner: <VERSION>
  build_web_compilers: <VERSION>
```

You can also run the web application with `webdev` - a dart global binary doing multiple tasks for you.  
To use it you need to `activate` it via:  

```sh
dart pub global activate webdev
```
-->
