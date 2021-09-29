# Redux Saga
Redux saga là một thư viện được phát triển nhằm hỗ trợ quản lý các side effect trong ứng dụng. 

Redux saga được sử dụng thay thế cho **thunk** của Redux, giúp tránh được callback hell, dễ dàng test hơn và giữ tính thuần túy của action.

Saga được cài đặt bằng các **Generator function**. Điểm khác biệt của generator function so với các function thông thường là từ khóa `yield` bên trong hàm, nó có tác dụng tạm dừng và tiếp tục quá trình thực thi của generator.

Ví dụ: 
```javascript
function * generatorFunction() { // Line 1
  console.log('This will be executed first.');
  yield 'Hello, ';   // Line 2
  console.log('I will be printed after the pause');  
  yield 'World!';
}
const generatorObject = generatorFunction(); // Line 3
console.log(generatorObject.next().value); // Line 4
console.log(generatorObject.next().value); // Line 5
console.log(generatorObject.next().value); // Line 6
// This will be executed first.
// Hello, 
// I will be printed after the pause
// World!
// undefined
```

## I.Effect
Effect là một bản chỉ dẫn cho middleware thực hiện một số thao tác nhất định gọi là **pattern**.

### Effect creator
#### Thao tác với Saga
- `take(pattern)`: truyền tới một Effect thông báo middleware đợi một action nào đó trong Store. Saga sẽ được trì hoãn cho đến khi action được dispatch. Middleware cũng cấp một action `END`, khi action này được dispatch, tất cả Saga bị `take` block sẽ bị hủy bỏ.

- `takeMaybe(pattern)`: tương tự như `take` nhưng thay vì hủy các Saga bị block, nó gửi action `END` đến các Saga đó.

- `takeEvery(pattern, saga, ...args)`: `spawn` Saga mỗi khi action khớp với `pattern`.

- `takeLatest(pattern, saga, ...args)`: `fork` Saga khi action khớp với `pattern`, nếu Saga ứng với `pattern` này được gọi trước đó chưa hoàn thành, thì sẽ tự động hủy và chuyển sang cái mới.

- `takeLeading(pattern, saga, ...args)`: `spawn` Saga khi action khớp với `pattern` và đợi Saga hoàn thành trước khi tiếp tục lắng nghe `pattern`.

#### Thao tác với action
- `put(action)`: dispatch action.

- `putResolve(action)`: dispatch action. Nếu dispatch trả về promise, nó sẽ đợi promise đó resolve.

#### Thao tác với hàm
- `call(fn, ...args)`: thực thi hàm `fn` với tham số là `args`.

- `fork(fn, ...args)`: tương tự như `call` nhưng sẽ không block Saga để đợi kết quả từ `fn`.

- `spawn(fn, ...args)`: tương tự như `fork` nhưng sẽ tạo ra một tác vụ riêng độc lập với tác vụ cha của nó. Tác vụ cha sẽ không đợi nó hoàn thành trước khi trả về kết quả.

## II.Channel
Channel là một hàng đợi các action được `fork`.

### actionChannel
Buffer lại kết quả của các action cho đến khi saga cần sử dụng.
```javascript
import { take, actionChannel, call, ... } from 'redux-saga/effects'

function* watchRequests() {
  const requestChan = yield actionChannel('REQUEST')
  while (true) {
    const {payload} = yield take(requestChan)
    yield call(handleRequest, payload)
  }
}

function* handleRequest(payload) { ... }
```

### eventChannel
Sử dụng để subcribe một event source từ bên ngoài và trả về kết quả cho Saga
```javascript
import { take, put, call } from 'redux-saga/effects'
import { eventChannel, END } from 'redux-saga'

// creates an event Channel from an interval of seconds
function countdown(secs) {
  return eventChannel(emitter => {
      const iv = setInterval(() => {
        secs -= 1
        if (secs > 0) {
          emitter(secs)
        } else {
          // this causes the channel to close
          emitter(END)
        }
      }, 1000);
      // The subscriber must return an unsubscribe function
      return () => {
        clearInterval(iv)
      }
    }
  )
}

export function* saga() {
  const chan = yield call(countdown, value)
  try {    
    while (true) {
      // take(END) will cause the saga to terminate by jumping to the finally block
      let seconds = yield take(chan)
      console.log(`countdown: ${seconds}`)
    }
  } finally {
    console.log('countdown terminated')
  }
}
```

### channel
`channel` sử dụng tương tự như actionChannel những có thể xứ lí nhiều request cùng 1 lúc thay vì chỉ 1 request 1 lần.
```javascript
import { channel } from 'redux-saga'
import { take, fork, ... } from 'redux-saga/effects'

function* watchRequests() {
  // create a channel to queue incoming requests
  const chan = yield call(channel)

  // create 3 worker 'threads'
  for (var i = 0; i < 3; i++) {
    yield fork(handleRequest, chan)
  }

  while (true) {
    const {payload} = yield take('REQUEST')
    yield put(chan, payload)
  }
}

function* handleRequest(chan) {
  while (true) {
    const payload = yield take(chan)
    // process the request
  }
}
```

## III.Effect Combination

### race(effects)
Tương tự như `Promise.race()`, `yield` kết quả của effect đầu tiên hoàn thành trong danh sách. Các effect chạy chậm hơn sẽ bị hủy tự động.

### all(effects)
Tương tự `Promise.all()`, `yield` danh sách kết quả trả về của tất cả effect.  

