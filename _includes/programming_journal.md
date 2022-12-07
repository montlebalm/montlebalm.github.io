## Type checking props with `forwardRef`

<div class="post-date">10/12/22</div>

The generic props parameter passed to `forwardRef` is not enforced on the prop object 😱

```
type Props = {
  shape: 'round' | 'square';
}

const Thing = forwardRef<HTMLDivElement, Props>((
  { shape = "anything!" },    // 🐞 This is allowed!?!?
  ref
) => {
  // ...
})
```

This occurs because the desctuctured props object satisfies _and extends_ the `Props` type. A necessary feature of static typing that we're not happy to see in this particular instance.

The solution is to explicitly type the props and ref params:

```
type Props = {
  shape: 'round' | 'square';
}

const Thing = forwardRef((
  { shape = "anything!" }: Props,    // ✅ Type error!
  ref: ForwardedRef<HTMLDivElement>
) => {
  // ...
})
```

## Submitting forms with `<enter>`

<div class="post-date">10/7/22</div>

Forms will only naturally submit on `<enter>` button if:

- An input with the "submit" type is present
- There's only 1 input

If you want to allow `<enter>` to submit the form without a visible button then you can add a hidden submit button:

```
<input style={{ display: 'none' }} type="submit" />
```

Reference: <https://www.tjvantoll.com/2013/01/01/enter-should-submit-forms-stop-messing-with-that/>

## Auto-resize `<textarea>`

<div class="post-date">9/22/22</div>

You can easily resize `<textarea>` elements based on their contents with a couple simple handlers.

```
const inputRef = useRef();

useLayoutEffect(() => {
  const input = inputRef.current;
  input.style.height = `${input.scrollHeight}px`;
}, []);

return (
  <textarea
    onChange={(e) => {
      const input = e.currentTarget;
      // Shrink to minimum height
      input.style.height = `var(--minHeight)`;
      // Grow to match content
      input.style.height = `${input.scrollHeight}px`;
    }}
    ref={inputRef}
  />
);
```

## Typing variadic params

<div class="post-date">9/9/22</div>

You can specify the number and shape of variadic params in TypeScript using conditional typing:

```
type Things = {
  'foo.event': never;
  'bar.event': {
    isEnabled: boolean;
  }
}

function logThing<TEvent extends keyof Things>(
  name: string,
  ...params: Things[TEvent] extends never
    ? [undefined?]
    : [Things[TEvent]]
) {
  // ...
}
```

This allows you to enforce both of these calls through the typechecker:

```
// Pass
logThing('foo.event')
logThing('bar.event', { isEnabled: true })

// Fail
logThing('foo.event', { other: false })
logThing('bar.event')
logThing('bar.event', { other: false })
```

The result is that we get a succinct function signature while maintaining full type safety.

## Typesafe Object iteration

<div class="post-date">8/14/22</div>

TypeScript won't correctly type object iteration through `Object.keys`, but you _can_ with `Object.entries`.

```
for (const [k, v] of Object.entries(abc)) {
  // ...
}
```

Reference: <https://effectivetypescript.com/2020/05/26/iterate-objects/>

## React's `<button>` autoFocus

<div class="post-date">7/26/22</div>

React has special handling around the `autoFocus` when applied to `<button>` elements. Instead of using the HTML attribute, they call `.focus()` on mount.

Reference: <https://github.com/facebook/react/issues/11851>

## React `children` props

<div class="post-date">7/25/22</div>

You can reference React `children` props from within the parent component (assuming 1 child):

```
function Foo({ children }) {
  Children.only(children);

  return cloneElement(children, {
    onClick: (e) => {
      // Do something smart

      children.props.onClick?.(e);
    },
  })
}
```

## CSS text gradient

<div class="post-date">7/1/22</div>

You can apply a color gradient to text by using a few choice `-webkit` properties:

```
h1 {
  background: -webkit-linear-gradient(#eee, #333);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}
```

Reference: <https://css-tricks.com/snippets/css/gradient-text/>

## React component prop variations

<div class="post-date">6/28/22</div>

You can make mutually exclusive variants of a component's props using TypeScript.

```
type CommonProps = {
  className?: string;
}

type ExclusiveProps = {
  onClick?: never;
  url?: never;
}

type LinkVariantProps = CommonProps & Omit<ExclusiveProps, 'url'> {
  url: string;
}

type ButtonVariantProps = CommonProps & Omit<ExclusiveProps, 'onClick'> {
  onClick: () => void;
}

type Props = LinkVariantProps | ButtonVariantProps;
```

## Framer Motion `key` prop

<div class="post-date">6/28/22</div>

Framer motion elements (e.g., `<motion.div>`) require a `key` prop even if they're not rendered inside a list.

Reference: <https://www.framer.com/docs/animate-presence/##unmount-animations>

## Pseudo-buttons

<div class="post-date">6/27/22</div>

In order to make a non-button act like a button you need to add 4 things:

- `role="button"`
- `tabIndex={0}`
- `onClick={...}`
- `onKeyDown={...}`

The `keydown` listener allows the pseudo button to be activated with specific keys just like regular buttons.

Here's an example:

```
onKeyDown={(e) => {
  if (e.key === ' ' || e.key === 'Enter') {
    e.currentTarget.click();
  }
}}
```

You can package this up into a helpful utility:

```
const buttonTriggerKeys = ['Enter', 'Space', ' '];

function getButtonRoleProps<TElement extends HTMLElement>(
  onClick: (e: MouseEvent<TElement>) => void
) {
  return {
    onClick,
    onKeyDown: (e: KeyboardEvent<TElement>) => {
      if (buttonTriggerKeys.includes(e.key)) {
        e.currentTarget.click();
      }
    },
    role: 'button',
    tabIndex: 0,
  };
}
```

Reference: <https://benfrain.com/converting-divs-into-accessible-pseudo-buttons/>

## Focus Jest tests

<div class="post-date">6/24/22</div>

You can focus one or more Jest tests by using `test.only`. You can skip one or more tests by using `test.skip`.

## Log focus changes

<div class="post-date">6/23/22</div>

I came across a code snippet that lets you log every time a new element gains focus. Very helpful for debugging!

```
document.addEventListener('focusin', function() {
  console.log('DEBUG: focus', document.activeElement)
}, true);
```

Reference: <https://hidde.blog/console-logging-the-focused-element-as-it-changes/>

## CSS visibility and focus

<div class="post-date">6/22/22</div>

Can be focused

- `opacity: 0`
- `tabIndex={0}`

Cannot be focused

- `display: none`
- `visibility: hidden`
- `tabIndex={-1}`

Reference: <https://fuzzbomb.github.io/accessibility-demos/visually-hidden-focus-test.html>

## Spacing flexbox/grid children

<div class="post-date">6/17/22</div>

You can separate flexbox/grid children using `gap: 4px` instead of having to do `div + div { margin-left: 4px }`. This removes the need to know class names or require specific positions of child elements.

`gap` encompasses both `row-gap` and `column-gap`. Like margin, you can specify 1 value that applies to both or separate them into 2 values: `gap: <row-gap> <column-gap>`.

Reference: <https://developer.mozilla.org/en-US/docs/Web/CSS/gap>

## Event bubbling in React

<div class="post-date">6/17/22</div>

All events in React bubble! Normally, events like `focus` and `blur` are local to the DOM element, but in React they bubble all the way up including through portals.

> Since a portal can be anywhere in the DOM tree, it may seem that event propagation may occur separately. However, this is not the case.
> The portal retains its position in the React tree, regardless of its actual position in the DOM tree. This means that events fired in a portal will propagate upwards to ancestors in the containing React tree, even if it is somewhere else in the DOM tree.

Reference: <https://jwwnz.medium.com/react-portals-and-event-bubbling-8df3e35ca3f1>
