# idlens

## Intro

Execute a function only when the user has interacted with the app in any of the below mentioned ways.
It's simple, configurable, typescript friendly and has an easy chainable API.

### Events

Supported NativeScript events:

"tap",
"doubleTap",
"longPress",
"pan",
"pinch",
"rotate", // will remove perhaps
"swipe",
"touch",
"scroll",
"scrollStarted",
"scrollEnded",
"scrollToTop",
"scrollToBottom",
"scrollToHorizontalOffset",
"scrollToVerticalOffset",
"focus",
"blur",
"textChange",
"returnPress",
"checkedChange",
"selectedIndexChange"

#### Usage

```typescript
import { Application } from '@nativescript/core';
import { Idle } from 'idlens';

// Get the root view of the application
const rootView = Application.getRootView();

// with predefined events on `document`
const idle = new Idle(rootView)
  .whenNotInteractive()
  .within(5)
  .do(() => logoutUser())
  .start();

// another example with custom events which are useful if events aren't bubbling up to the document
const idle = new Idle(rootView)
  .whenNot([
    {
      events: ['click', 'hover'],
      target: buttonEl,
    },
    {
      events: ['click', 'input'],
      target: inputEl,
    },
  ])
  .whenNotInteractive()
  .within(10)
  .do(logoutUser)
  .start();
```

For more features or examples please check the [tests](./src/idle.spec.ts) and [source]('./src/idle.ts) code.

### NotIdle

Executes the callback function (`do`), if at least **one** of the specified events have occured within given time, in other words when user is not idle or interactive.

#### Usage of NotIdle

```typescript
import { NotIdle } from 'idlens';
import { Application } from '@nativescript/core';

// Get the root view of the application
const rootView = Application.getRootView();

// with predefined events on `document`
const idle = new Idle(rootView)
  .whenInteractive()
  .within(10)
  .do(() => log('user was active in the last 10 minutes'))
  .start();

// another example with custom events which are useful if events aren't bubbling up to the `document`
const notIdle = new NotIdle(rootView)
  .when([
    {
      events: ['click', 'hover'],
      target: buttonEl,
    },
    {
      events: ['click', 'input'],
      target: inputEl,
    },
  ])
  .whenInteractive()
  .within(10)
  .do(() => log('user was active in the last 10 minutes'))
  .start();
```

For more features or examples please check the [tests](./src/not-idle.spec.ts) and [source]('./src/not-idle.ts) code.

### Setting time

Second parameter of `within` is time unit in miliseconds, by default 60000 (a minute).

```typescript
// will trigger if nothing happens for 5 minutes
new Idle(rootView).within(5);

// will trigger if nothing happens for 5 seconds
new Idle(rootView).within(5, 1000);
```
