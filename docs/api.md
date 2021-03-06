# API

## `compact(object: object) => object`

Removes "null" values from the first layer of a given object.

```javascript
import { compact } from 'obbie'

compact({ a: null, b: undefined, c: 3 })
//=> { c: 3 }

// It won't go deep inside the given object.
compact({ a: { b: null } })
//=> { a: { b: null } }
```

## `deleteIf(object: object, expectation?: Function = (() => {})) => object`

Removes entries matching a given expectation.

```javascript
import { deleteIf } from 'obbie'

const myObject = {
  a: 1,
  b: 2,
  c: 3
}

deleteIf(myObject,
         (key, value) => (value % 2) === 0)
//=> { a: 1, b: 2 }

deleteIf(myObject)
//=> { a: 1, b: 2, c: 3 }
```

## `dig(object: object, ...keySequence: number|string|Array<number|string>) => any`

Returns the value of a key sequence searched in a given object.

```javascript
import { dig } from 'obbie'

const myObject = {
  a: {
    b: {
      c: [1, 2, 3]
    }
  }
}

dig(myObject, 'a', 'b', 'c', 1)
//=> 2

dig(myObject, ['a', 'b', 'c', 1])
//=> 2
```

## `fetch(object: object, key: string, defaultValue?: any) => any`

Fetches a value from a given key in the object passed.

```javascript
import { fetch } from 'obbie'

const myObject = {
  a: 1,
  b: 2
}

fetch(myObject, 'b')
//=> 2

fetch(myObject, 'c', 'I love memes')
//=> 'I love memes'

// It ignores the default value if key returns a value
fetch(myObject, 'b', 'I love memes')
//=> 2

fetch(myObject, 'c')
//=> throws 'KeyError: key not found: "c"'

fetch(myObject, 'c', key => `The key is: ${key}`)
//=> "The key is: c"

// Returns "null" if default value is a function returning "undefined"
fetch(myObject, 'c', () => {})
//=> null
```

## `fetchValues(object: object, keys: Array<string|number>, defaultValue?: any) => any[]`

Returns the values of the keys in the object using [`Obbie.fetch`](https://git.io/vpiD9).

**NOTE:** Our usage is a little bit different from what Ruby looks like. It accepts the keys (the second argument) as an `Array`. The decision is intended to support an optional parameter `defaultValue`, which can be the `block` (as a function) or any other value.

```javascript
import { fetchValues } from 'obbie'

const myObject = {
  a: 1,
  b: 2
}

fetchValues(myObject, ['a', 'b'])
//=> [1, 2]

fetchValues(myObject, ['a', 'b', 'c'], 'I love memes')
//=> [1, 2, 'I love memes']

// It ignores the default value if key returns a value
fetchValues(myObject, ['a', 'b'], 'I love memes')
//=> [1, 2]

fetchValues(myObject, ['a', 'b', 'c'])
//=> throws 'KeyError: key not found: "c"'

fetchValues(myObject, ['a', 'b', 'c'], key => `The key is: ${key}`)
//=> [1, 2, 'The key is: c']

// Returns "null" if default value is a function returning "undefined"
fetchValues(myObject, ['a', 'b', 'c'], () => {})
//=> [1, 2, null]
```

## `length(object: object) => number`

Returns the amount of entries in a given object.

```javascript
import { length } from 'obbie'

length({ a: 1, b: undefined, c: 3, d: null })
//=> 4

length({ a: 1, b: 2 })
//=> 2
```
