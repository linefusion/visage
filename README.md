# Visage

Visage allows you to write presentational components with just SCSS.

> This is a WIP and the final format is still being discussed and feedback is being collected. Changes are happening all the time. 

# Overview

## SCSS

> components/button.module.scss

```scss
@include "@wolfulus/component" as *;
@include "@wolfulus/props" as props;

@include component(Button) {
  @include element(button, (
    target: "_blank"
  ))
  @include variants {
    @include new(inverted);
  }
  @include stylesheet {
    @apply px-4 py-2 rounded bg-slate-800 text-white;
    &:hover {
      @apply bg-slate-600;
    }

    @include variant(inverted) {
      @apply bg-slate-100 text-black;
      &:hover {
        @apply bg-slate-300;
      }
    }
  }
}
```

## React

> page.tsx

```tsx
import { Button } from "./components/button.module.scss";

export function Page() {
  return <Button inverted />
}

```

## Output

```html
<button class="ButtonComponent" target="_blank" data-inverted="true" />
```

```css
.ButtonComponent {
  @apply px-4 py-2 rounded bg-slate-800 text-white;
}

.ButtonComponent:hover {
  @apply bg-slate-600;
}

.ButtonComponent[data-invertred="true"] {
  @apply bg-slate-100 text-black;
}

.ButtonComponent[data-inverted="true"]:hover {
  @apply bg-slate-300;
}
```

# Components

#### Description

You can define components by including the `component` mixin. If no element is defined for the component, `div` is used by default.

#### Definition

```scss
@include component(MyButton) {
  //
}
```

#### Usage

```tsx
<MyButton>
  Hello
</MyButton>
```

#### Output

```html 
<div>
  Hello
</div>
```

--- 

# Default Element

#### Description

You can specify which element the component will instantiate when rendering the component. This is useful when you want to specify a default element to be used with the component.

> Note that you can **override** the element upon usage, thus it's not recommended to create selectors using element names to select custom components.

#### Definition

```scss
@include component(MyButton) {
  @include element(button);
}
```

#### Usage

```tsx
<MyButton>
  Hello
</MyButton>
<MyButton component={"a"} href="#" role="button">
  Hello
</MyButton>
```

#### Output

```html 
<button>
  Hello
</button>
<a href="#" role="button">
  Hello
</a>
```

# Default Element Attributes

#### Description

You can specify default attribute values of the component element. You can still override those attributes if needed.

#### Definition

```scss
@include component(MyButton) {
  @include element(button);
  @include attribute(type, "button");
}
```

or 

```scss
@include component(MyButton) {
  @include element(button, (
    type: "button",
  ));
}
```

#### Usage

```tsx
<MyButton>
  Hello
</MyButton>
<MyButton type="submit">
  Hello
</MyButton>
```

#### Output

```html 
<button type="button">Hello</button>
<button type="submit">Hello</button>
```

# Styles

#### Description

Components can be stylized using a `stylesheet` mixing block inside the component definition.

The contents of the mixin block will be nested within a simple class selector (e.g. `.component`) that is always added as the first class in component's `className` root element. This means that anything that written `stylesheet` is effectively nested within the root element.  block will be nested within this class and `&` targets the root element class  `.RootElement`.

The stylesheet block generates a class that is applied to the component's root element. Anything inside the stylesheet block is is nested within the component's root class.

#### Definition

```scss
@include component(MyButton) {
  @include element(button);
  @include attribute(type, "button");
  @include stylesheet {
    @apply mx-4 my-2 bg-green-400 text-white;
  }
}
```

#### Usage

```tsx
<MyButton>
  Hello
</MyButton>
<MyButton type="submit">
  Hello
</MyButton>
```

#### Output

```html 
<button type="button">Hello</button>
<button type="submit">Hello</button>
```

```scss
@include component(Button) {
  @include stylesheet {
    @apply mx-4 my-2; 
  }
}
```

## Variants

```scss
@include component(Button) {
  @include variants {
    @include new(success);
  }
  @include stylesheet {
    @include variant(success) {
      @apply bg-green;
    }
  }
}
```

```tsx
<Button success />
```

## Props

```scss
@include component(Button) {
  @include properties {
    @include new(my-color, props.color());
  }
  @include stylesheet {
    border-color: from-property(my-color);
  }
}
```

```tsx
<Button color="#F00" />
```

