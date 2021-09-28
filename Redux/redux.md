# Redux

## Khái niệm
**Redux** là một pattern và thư việc tập trung vào việc quản lí **state** thông qua quyết định và thực hiện các **action**.

### State
State trong Redux là bất biến, ta thay đổi các giá trị trong state bằng cách trả về một copy với giá trị đẫ thay đổi.

### Action
Action là các sự kiện xảy ra trong ứng dụng mà gây ra thay đổi trong state.

Action được định nghĩa dưới dạng object với 2 thuộc tính `type` và `payload`
- `type`: mô tả sự kiện xảy ra trong hệ thống.
- `payload`: dữ liệu đi kèm với action. 

### Reducer
Reducer là một hàm nhận vào `state` hiện tại và `action` làm tham số, nó thực hiện `action` và trả về state mới.

Reducer có 3 điều kiện bắt buộc trong việc định nghĩa:
- Chỉ sử dụng 2 tham số là `state` và `action`.
- Không thay đổi giá trị trong `state`, chỉ điều chỉnh giá trị trong copy của `state`.
- Không chứa logic bất đồng bộ, không gây ra side effect.

Bên trong hàm reducer là logic điều kiện dựa trên `action`, quyết định việc giữ nguyên hay trả về một state mới.

### Store
Store là nơi chứa tất cả các state cần chia sẻ. State trong store được truy vấn bằng  phương thức `getState`.

### Dispatch
Dispatch là phương thức trigger event trong ứng dụng. Nó nhận vào một action object và truyền nó vào `reducer` cùng với `state`.

## Luồng dữ liệu

1. Event được trigger từ UI.
2. Hệ thống dispatch một `action` tương ứng với event.
3. Store thực thi reducer với `state` hiện tại và `action` được truyền vào, trả về state mới.
4. Store thông báo cập nhật đến các component liên quan.
5. Component kiểm tra xem phần dữ liệu nó cần có bị thay đổi không.
6. Nếu có, component render lại với giá trị mới.

## Sử dụng
Dùng hàm `createSlice` để khởi tạo state và reducer và export danh sách reducer để sử dụng.
```javascript
import { createSlice } from '@reduxjs/toolkit';

export const counterSlice = createSlice({
  // Tên state
  name: 'counter',
  // Giá trị khởi tạo
  initialState: {
    value: 0,
  },
  // Định nghĩa reducer và các action
  reducers: {
    increment: state => {
      state.value += 1;
    },
    decrement: state => {
      state.value -= 1;
    },
    incrementByAmount: (state, action) => {
      state.value += action.payload;
    },
    decrementByAmount: (state, action) => {
      state.value += action.payload;
    }
  }
})

// export các action creator
export const {increment, decrement, incrementByAmount, decrementByAmount} = counterSlice.actions;

// export reducer
export default counterSlice.reducer;
```
Truyền reducer vào hàm `configureStore` để tạo state trên store.
```javascript
import { configureStore } from '@reduxjs/toolkit';
import counterReducer from './counterSlice';

export default configureStore({
  reducer: {
    counter: counterReducer
  }
})
```
Thêm `<Provider>` để cung cấp cho các component cần sử dụng.
```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';
import store from './store';
import { Provider } from 'react-redux';

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
);
```
Truy vấn state bằng hook `useSelector`.
```javascript
const value = useSelector(state => state.counter.value);
```
Trigger event bằng hook `useDispatch`.
```javascript
const dispatch = useDispatch();
dispatch(increment())
```

## Bất đồng bộ
Để thêm các logic bất đồng bộ cho store, chẳng hạn như call API để lấy payload cho action, ta sử dụng action creator `createAsyncThunk`.
```javascript
const resolveAfter2Seconds = () => {
  return new Promise(resolve => {
    setTimeout(() => {
      resolve(100);
    }, 2000);
  });
}

export const plus100 = createAsyncThunk('counter/plus100', async () => {
  const response = await resolveAfter2Seconds();
  return response;
})
```
Sau đó thêm reducer xử lý các trạng thái của promise
```javascript
export const counterSlice = createSlice({
  name: 'counter',
  initialState: {
    value: 0,
    status: 'idle',
    error: null
  },
  reducers: {
    ...
  },
  extraReducers(builder) {
    builder
      .addCase(plus100.pending, (state) => {
        state.status = 'loading'
      })
      .addCase(plus100.fulfilled, (state, action) => {
        state.status = 'succeeded'
        state.value = state.value+=action.payload
      })
      .addCase(plus100.rejected, (state, action) => {
        state.status = 'failed'
        state.error = action.error.message
      })
  }
})
```
Lúc này ta có thể dispatch như action bình thường
```javascript
dispatch(plus100())
```