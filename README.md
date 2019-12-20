# preactive 

A R&D project to evaluate an alternative API developing
components and hook functions with Preact.

### Installation

```
git clone https://github.com/mcjazzyfunky/preactive.git
cd preactive
npm install
```

### Running demos

```
npm run storybook
```

## Example

```jsx
import { h, render } from 'preact'
import { statefulComponent } from 'preactive'
import { useProps, useValue } from 'preactive/hooks'

const counterDefaults = {
  initialValue: 0,
  label: 'Counter'
}

const Counter = statefulComponent('Counter', c => {
  const
    props = useProps(c, counterDefaults),
    [count, setCount] = useValue(c, props.initialValue),
    onIncrement = () => setCount(it => it + 1),
    onInput = ev => setCount(ev.currentTarget.valueAsNumber)

  return () =>
    <div>
      <h3>{props.label}: </h3>
      <input type="number" value={count.value} onInput={onInput} />
      <button onClick={onIncrement}>{count.value}</button>
    </div>
})

render(<Counter/>, document.getElementById('app'))
```

In the above example the `c` is a so called component controller
(some kind of representation for the component instance).
Its type is currently (may change in future):

```typescript
type Ctrl<P extends Props = {}> = {
  getProps(): P,
  isMounted(): boolean,
  update(): void,
  getContextValue<T>(Context<T>): T,
  afterMount(subscriber: Subscriber): void,
  beforeUpdate(subscriber: Subscriber): void,
  afterUpdate(subscriber: Subscriber): void,
  beforeUnmount(subscriber: Subscriber): void,
  runOnceBeforeUpdate(task: Task): void
}

type Props = Record<string, any>
type Subscriber = () => void
type Task = () => void
type Context<T> = Preact.Context<T>
```
