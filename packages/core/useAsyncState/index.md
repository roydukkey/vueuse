---
category: State
---

# useAsyncState

Reactive async state. Will not block your setup function and will trigger changes once the promise is ready. The state is a `shallowRef` by default.

## Usage

```ts
import axios from 'axios'
import { useAsyncState } from '@vueuse/core'

const { state, isReady, isLoading } = useAsyncState(
  axios
    .get('https://jsonplaceholder.typicode.com/todos/1')
    .then(t => t.data),
  { id: null },
)
```

### onCancel

If the `execute` function is called before the previous execution has completed, an attempt is made to cancel the outstanding execution. Here is an example showing how to incorporate with the fetch API.

```ts
import axios from 'axios'
import { useAsyncState } from '@vueuse/core'

const { state, isReady, isLoading } = useAsyncState(
  async (onCancel) => {
    const abortController = new AbortController()

    onCancel(() => abortController.abort())

    return axios.get(
      'https://jsonplaceholder.typicode.com/todos/1',
      { signal: abortController.signal },
    )
      .then(response => response.ok ? response.data : { id: null })
  },
  { id: null },
)
```
