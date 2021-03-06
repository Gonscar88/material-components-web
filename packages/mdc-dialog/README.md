<!--docs:
title: "Dialogs"
layout: detail
section: components
excerpt: "Modal dialogs."
iconId: dialog
path: /catalog/dialogs/
-->

# Dialog

<!--<div class="article__asset">
  <a class="article__asset-link"
     href="https://material-components.github.io/material-components-web-catalog/#/component/dialog">
    <img src="{{ site.rootpath }}/images/mdc_web_screenshots/dialogs.png" width="714" alt="Dialogs screenshot">
  </a>
</div>-->

Dialogs inform users about a task and can contain critical information, require decisions, or involve multiple tasks.

## Design & API Documentation

<ul class="icon-list">
  <li class="icon-list-item icon-list-item--spec">
    <a href="https://material.io/go/design-dialogs">Material Design guidelines: Dialogs</a>
  </li>
  <li class="icon-list-item icon-list-item--link">
    <a href="https://material-components.github.io/material-components-web-catalog/#/component/dialog">Demo</a>
  </li>
</ul>

## Installation

```
npm install @material/dialog
```

## Basic Usage

### HTML Structure

```html
<div class="mdc-dialog"
     role="alertdialog"
     aria-modal="true"
     aria-labelledby="my-dialog-title"
     aria-describedby="my-dialog-content">
  <div class="mdc-dialog__container">
    <!-- Title cannot contain leading whitespace due to mdc-typography-baseline-top() -->
    <h2 class="mdc-dialog__title" id="my-dialog-title"><!--
   -->Dialog Title<!--
 --></h2>
    <section class="mdc-dialog__content" id="my-dialog-content">
      Dialog body text goes here.
    </section>
    <footer class="mdc-dialog__actions">
      <button type="button" class="mdc-button mdc-dialog__button" data-mdc-dialog-action="no">No</button>
      <button type="button" class="mdc-button mdc-dialog__button" data-mdc-dialog-action="yes">Yes</button>
    </footer>
  </div>
  <div class="mdc-dialog__scrim"></div>
</div>
```

> *NOTE*: Titles cannot contain leading whitespace due to how `mdc-typography-baseline-top()` works.

### Styles

```scss
@import "@material/dialog/mdc-dialog";
```

> *NOTE*: Styles for any components you intend to include within dialogs (e.g. List, Checkboxes, etc.) must also be
> imported.

### JavaScript Instantiation

```js
import {MDCDialog} from '@material/dialog';
const dialog = new MDCDialog(document.querySelector('.mdc-dialog'));
```

> See [Importing the JS component](../../docs/importing-js.md) for more information on how to import JavaScript.

> *NOTE*: MDC Dialog makes no assumptions about what will be added to the `mdc-dialog__content` element.
> Any List, Checkboxes, etc. must also be instantiated.
> Additionally, call `layout` on any components within the content when `MDCDialog:opened` is emitted.

## Variants

### Simple Dialog

The Simple Dialog contains a list of potential actions. It does not contain buttons.

```html
<div class="mdc-dialog"
     role="alertdialog"
     aria-modal="true"
     aria-labelledby="my-dialog-title"
     aria-describedby="my-dialog-content">
  <div class="mdc-dialog__container">
    <!-- Title cannot contain leading whitespace due to mdc-typography-baseline-top() -->
    <h2 class="mdc-dialog__title" id="my-dialog-title"><!--
   -->Choose a Ringtone<!--
 --></h2>
    <section class="mdc-dialog__content" id="my-dialog-content">
      <ul class="mdc-list">
        <li class="mdc-list-item">
          <span class="mdc-list-item__text">None</span>
        </li>
        <li class="mdc-list-item">
          <span class="mdc-list-item__text">Callisto</span>
        </li>
        <!-- ... -->
      </ul>
    </section>
  </div>
  <div class="mdc-dialog__scrim"></div>
</div>
```

### Confirmation Dialog

The Confirmation Dialog contains a list of choices, and buttons to confirm or cancel. Choices are accompanied by
radio buttons (indicating single selection) or checkboxes (indicating multiple selection).

```html
<div class="mdc-dialog"
     role="alertdialog"
     aria-modal="true"
     aria-labelledby="my-dialog-title"
     aria-describedby="my-dialog-content">
  <div class="mdc-dialog__container">
    <!-- Title cannot contain leading whitespace due to mdc-typography-baseline-top() -->
    <h2 class="mdc-dialog__title" id="my-dialog-title"><!--
   -->Choose a Ringtone<!--
 --></h2>
    <section class="mdc-dialog__content" id="my-dialog-content">
      <ul class="mdc-list">
        <li class="mdc-list-item" tabindex="0">
          <span class="mdc-list-item__graphic">
            <div class="mdc-radio">
              <input class="mdc-radio__native-control"
                     type="radio"
                     id="test-dialog-baseline-confirmation-radio-1"
                     name="test-dialog-baseline-confirmation-radio-group"
                     checked>
              <div class="mdc-radio__background">
                <div class="mdc-radio__outer-circle"></div>
                <div class="mdc-radio__inner-circle"></div>
              </div>
            </div>
          </span>
          <label id="test-dialog-baseline-confirmation-radio-1-label"
                 for="test-dialog-baseline-confirmation-radio-1">None</label>
        </li>
        <!-- ... -->
      </ul>
    </section>
    <footer class="mdc-dialog__actions">
      <button type="button" class="mdc-button mdc-dialog__button" data-mdc-dialog-action="close">Cancel</button>
      <button type="button" class="mdc-button mdc-dialog__button" data-mdc-dialog-action="accept">OK</button>
    </footer>
  </div>
  <div class="mdc-dialog__scrim"></div>
</div>
```

> *NOTE*: In the example above, the Cancel button intentionally has the `close` action to align with the behavior of
> clicking the scrim or pressing the Escape key, allowing all interactions involving dismissal without taking an action
> to be detected the same way.

### Additional Information

#### Dialog Actions

All dialog variants support the concept of dialog actions. Any element within a dialog may include the
`data-mdc-dialog-action` attribute to indicate that interacting with it should close the dialog with the specified action.
This action is then reflected via `event.detail.action` in the `MDCDialog:closing` and `MDCDialog:closed` events.

Additionally, two interactions have defined actions by default:

* Clicking on the scrim
* Pressing the Escape key within the dialog

Both of these map to the `close` action by default. This can be accessed and customized via the component's
`scrimClickAction` and `escapeKeyAction` properties, respectively.

Setting either of these properties to an empty string will result in that interaction being disabled (i.e. the dialog
will no longer close in response to the interaction). Exercise caution when doing this - it should always be possible
for a user to dismiss the dialog.

Any action buttons within the dialog which equate strictly to a dismissal with no further action should also use the
`close` action; this will make it easy to handle all such interactions consistently, while separately handling other
actions.

#### Action Button Arrangement

As indicated in the [Dialog design article](https://material.io/design/components/dialogs.html#anatomy), buttons within
the `mdc-dialog__actions` element are arranged horizontally by default, with the confirming action _last_.

In cases where the button text is too long for all buttons to fit on a single line, the buttons are stacked vertically,
with the confirming action _first_.

MDC Dialog detects and handles this automatically by default, reversing the buttons when applying the stacked layout.
This automatic behavior can be disabled by setting `autoStackButtons` to `false` on the component instance:

```js
dialog.autoStackButtons = false;
```

This will also be disabled if the `mdc-dialog--stacked` modifier class is applied manually to the root element before the
component is instantiated, but note that dialog action button labels are recommended to be short enough to fit on a
single line if possible.

#### Actions and Selections

Dialogs which require making a choice via selection controls should initially disable any button which performs an
action if no choice is selected by default. MDC Dialog does not include built-in logic for this, since it aims to remain
as unopinionated as possible regarding dialog contents, aside from relaying information on which action is taken.

## Style Customization

### CSS Classes

CSS Class | Description
--- | ---
`mdc-dialog` | Mandatory. The root DOM element containing the surface and the container.
`mdc-dialog__scrim` | Mandatory. Semitransparent backdrop that displays behind a dialog.
`mdc-dialog__container` | Mandatory. Wrapper element needed to ensure flexbox behavior in IE 11.
`mdc-dialog__surface` | Mandatory. The bounding box for the dialog's content.
`mdc-dialog__title` | Optional. Brief summary of the dialog's purpose.
`mdc-dialog__content` | Optional. Primary content area. May contain a list, a form, or prose.
`mdc-dialog__actions` | Optional. Footer area containing the dialog's action buttons.
`mdc-dialog__button` | Optional. Individual action button. Typically paired with [`mdc-button`](../mdc-button).
`mdc-dialog--open` | Optional. Indicates that the dialog is open and visible.
`mdc-dialog--opening` | Optional. Applied automatically when the dialog is in the process of animating open.
`mdc-dialog--closing` | Optional. Applied automatically when the dialog is in the process of animating closed.
`mdc-dialog--scrollable` | Optional. Applied automatically when the dialog has overflowing content to warrant scrolling.
`mdc-dialog--stacked` | Optional. Applied automatically when the dialog's action buttons can't fit on a single line and must be stacked.

### Sass Mixins

Mixin | Description
--- | ---
`mdc-dialog-container-fill-color($color)` | Sets the fill color of the dialog.
`mdc-dialog-scrim-color($color, $opacity)` | Sets the color of the scrim behind the dialog.
`mdc-dialog-title-ink-color($color, $opacity)` | Sets the color of the dialog's title text.
`mdc-dialog-content-ink-color($color, $opacity)` | Sets the color of the dialog's content text.
`mdc-dialog-scroll-divider-color($color, $opacity)` | Sets the color of the dividers which display around scrollable content.
`mdc-dialog-corner-radius($radius)` | Sets the corner radius to the given amount (defaults to 4px on all sides).
`mdc-dialog-min-width($min-width)` | Sets the minimum width of the dialog (defaults to 280px).
`mdc-dialog-max-width($max-width, $margin)` | Sets the maximum width of the dialog (defaults to 560px max width and 16px margins).
`mdc-dialog-max-height($max-height, $margin)` | Sets the maximum height of the dialog (defaults to no max height and 16px margins).

> *NOTE*: The `max-width` and `max-height` mixins only apply their maximum when the viewport is large enough to accommodate the specified value when accounting for the specified margin on either side. When the viewport is smaller, the dialog is sized such that the given margin is retained around the edges.

## `MDCDialog` Properties and Methods

Property | Value Type | Description
--- | --- | ---
`isOpen` | `boolean` (read-only) | Proxies to the foundation's `isOpen` method.
`escapeKeyAction` | `string` | Proxies to the foundation's `getEscapeKeyAction` and `setEscapeKeyAction` methods.
`scrimClickAction` | `string` | Proxies to the foundation's `getScrimClickAction` and `setScrimClickAction` methods.
`autoStackButtons` | `boolean` | Proxies to the foundation's `getAutoStackButtons` and `setAutoStackButtons` methods.

Method Signature | Description
--- | ---
`layout() => void` | Recalculates layout and automatically adds/removes modifier classes like `--scrollable`.
`open() => void` | Opens the dialog.
`close(action: string?) => void` | Closes the dialog, optionally with the specified action indicating why it was closed.

### Events

Event Name | `event.detail` | Description
--- | --- | ---
`MDCDialog:opening` | `{}` | Indicates when the dialog begins its opening animation.
`MDCDialog:opened` | `{}` | Indicates when the dialog finishes its opening animation.
`MDCDialog:closing` | `{action: string?}` | Indicates when the dialog begins its closing animation. `action` represents the action which closed the dialog.
`MDCDialog:closed` | `{action: string?}` | Indicates when the dialog finishes its closing animation. `action` represents the action which closed the dialog.

## Usage within Web Frameworks

If you are using a JavaScript framework, such as React or Angular, you can create a Dialog for your framework. Depending on your needs, you can use the _Simple Approach: Wrapping MDC Web Vanilla Components_, or the _Advanced Approach: Using Foundations and Adapters_. Please follow the instructions [here](../../docs/integrating-into-frameworks.md).

### `MDCDialogAdapter`

Method Signature | Description
--- | ---
`addClass(className: string) => void` | Adds a class to the root element.
`removeClass(className: string) => void` | Removes a class from the root element.
`hasClass(className: string) => boolean` | Returns whether the given class exists on the root element.
`addBodyClass(className: string) => void` | Adds a class to the `<body>`.
`removeBodyClass(className: string) => void` | Removes a class from the `<body>`.
`eventTargetHasClass(target: !EventTarget, className: string) => void` | Returns `true` if the target element has the given CSS class, otherwise `false`.
`computeBoundingRect()`: Forces the component to recalculate its layout; in the vanilla DOM implementation, this calls `computeBoundingClientRect`.
`trapFocus() => void` | Sets up the DOM such that keyboard navigation is restricted to focusable elements within the dialog surface (see [Handling Focus Trapping](#handling-focus-trapping) below for more details).
`releaseFocus() => void` | Removes any effects of focus trapping on the dialog surface (see [Handling Focus Trapping](#handling-focus-trapping) below for more details).
`isContentScrollable() => boolean` | Returns `true` if `mdc-dialog__content` can be scrolled by the user, otherwise `false`.
`areButtonsStacked() => boolean` | Returns `true` if `mdc-dialog__action` buttons (`mdc-dialog__button`) are stacked vertically, otherwise `false` if they are side-by-side.
`getActionFromEvent(event: !Event) => ?string` | Retrieves the value of the `data-mdc-dialog-action` attribute from the given event's target, or an ancestor of the target.
`reverseButtons() => void` | Reverses the order of action buttons in the `mdc-dialog__actions` element. Used when switching between stacked and unstacked button layouts.
`notifyOpening() => void` | Broadcasts an event denoting that the dialog has just started to open.
`notifyOpened() => void` | Broadcasts an event denoting that the dialog has finished opening.
`notifyClosing(action: string) {}` | Broadcasts an event denoting that the dialog has just started closing. If a non-empty `action` is passed, the event's `detail` object should include its value in the `action` property.
`notifyClosed(action: string) {}` | Broadcasts an event denoting that the dialog has finished closing. If a non-empty `action` is passed, the event's `detail` object should include its value in the `action` property.

### `MDCDialogFoundation`

Method Signature | Description
--- | ---
`open()` | Opens the dialog.
`close(action: string)` | Closes the dialog, optionally with the specified action indicating why it was closed.
`isOpen() => boolean` | Returns whether the dialog is open.
`layout()` | Recalculates layout and automatically adds/removes modifier classes e.g. `--scrollable`.
`getEscapeKeyAction() => string` | Returns the action reflected when the Escape key is pressed.
`setEscapeKeyAction(action: string)` | Sets the action reflected when the Escape key is pressed. Setting to `''` disables closing the dialog via Escape key.
`getScrimClickAction() => string` | Returns the action reflected when the scrim is clicked.
`setScrimClickAction(action: string)` | Sets the action reflected when the scrim is clicked. Setting to `''` disables closing the dialog via scrim click.
`getAutoStackButtons() => boolean` | Returns whether stacked/unstacked action button layout is automatically handled during layout logic.
`setAutoStackButtons(autoStack: boolean) => void` | Sets whether stacked/unstacked action button layout is automatically handled during layout logic.
`handleClick(event: Event)` | Handles `click` events on or within the dialog's root element
`handleDocumentKeydown(event: Event)` | Handles `keydown` events on or within the document while the dialog is open

#### Event Handlers

When wrapping the Dialog foundation, the following events must be bound to the indicated foundation methods:

Event | Target | Foundation Handler | Register | Deregister
--- | --- | --- | --- | ---
`click` | `.mdc-dialog` (root) | `handleClick` | During initialization | During destruction
`keydown` | `document` | `handleDocumentKeydown` | On `MDCDialog:opening` | On `MDCDialog:closing`
`resize` | `window` | `layout` | On `MDCDialog:opening` | On `MDCDialog:closing`
`orientationchange` | `window` | `layout` | On `MDCDialog:opening` | On `MDCDialog:closing`

### The Util API

External frameworks and libraries can use the following utility methods from the `util` module when implementing their own component.

Method Signature | Description
--- | ---
`createFocusTrapInstance(surfaceEl: !Element, initialFocusEl: ?Element, focusTrapFactory: function(): !FocusTrap) => !FocusTrap` | Creates a properly configured [focus-trap][] instance.
`isScrollable(el) => boolean` | Determines if the given element is scrollable.
`areTopsMisaligned(els) => boolean` | Determines if two or more of the given elements have different `offsetTop` values.

### Handling Focus Trapping

In order for dialogs to be fully accessible, they must conform to the guidelines outlined in:

* https://www.w3.org/TR/wai-aria-practices/#dialog_modal
* https://www.w3.org/TR/wai-aria-practices-1.1/examples/dialog-modal/dialog.html
* https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/ARIA_Techniques/Using_the_dialog_role

The main implication of these guidelines is that the only focusable elements are those contained within a dialog
surface.

Trapping focus correctly for a modal dialog requires a complex set of events and interaction
patterns that we feel is best not duplicated within the logic of this component. Furthermore,
frameworks and libraries may have their own ways of trapping focus that framework authors may want
to make use of. For this reason, we have two methods on the adapter that should be used to handle
focus trapping:

* `trapFocus()` is called when the dialog is open and should set up focus trapping adhering
  to the ARIA practices in the link above.
* `releaseFocus()` is called when the dialog is closed and should tear down any focus
  trapping set up when the dialog was open.

The `MDCDialog` component uses the [focus-trap][] package to handle this.
**You can use `util.createFocusTrapInstance()` (see below) to easily create
a focus trapping solution for your component code.**

[focus-trap]: https://github.com/davidtheclark/focus-trap

#### `createFocusTrapInstance()`

```js
const {activate, deactivate} =
  util.createFocusTrapInstance(surfaceEl, initialFocusEl, focusTrapFactory = require('focus-trap'));
```

Given a dialog surface element, an initial element to focus, and an optional `focusTrap` factory
function, such that:

* The focus is trapped within the `surfaceEl`
* The `initialFocusEl` receives focus when the focus trap is activated
* Closing the dialog in any way (including pressing Escape or clicking outside the dialog) deactivates focus trapping
* Focus is returned to the previously focused element before the focus trap was activated

This focus trap instance can be used to implement the `trapFocus` and `releaseFocus` adapter methods by calling
`instance.activate()` and `instance.deactivate()` respectively within those methods.

The `focusTrapFactory` can be used to override the `focus-trap` function used to create the focus trap. Its API is the
same as focus-trap's [createFocusTrap](https://github.com/davidtheclark/focus-trap#focustrap--createfocustrapelement-createoptions)
(which is what it defaults to). You can pass in a custom function for mocking out the actual function within tests,
or to modify the arguments passed to the function before it's called.
