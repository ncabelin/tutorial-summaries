# All About Sass
This is a compilation of tutorials about Sass

### Nesting
* Nest SELECTORS :
```
.parent {
  font-size: 16px;
  .child {
    color: blue;
  }
}
```

* Nest PROPERTIES :
```
.parent {
  font : {
    family: Roboto, sans-serif;
    size: 12px;
    decoration: none;
  }
}
```

### Variables
* create variables like so :
```
$almond: rgba(240,12,233,0.4);
```
variables can contain :

1. *Numbers*
2. *Strings*
3. *Booleans*
4. *null*

### Pseudo-elements
* create shortcuts to pseudoelement like so with `&`:
```
.container {
  &:hover {
    color: blue;
  }
  color: red;
}
```

### Create mixins
* create mixins by typing `@mixin` :
```
@mixin example {
  color: orange;
}

h1 {
  @include mixin;
}
```

* pass a value by typing `@mixin mixinName($mixinValue)` :
```
@mixin mixMe($value) {
  background-color: $value;
}
// then use it
h1 {
  @include mixMe(rgba(0,0,0,0.8));
}
```
