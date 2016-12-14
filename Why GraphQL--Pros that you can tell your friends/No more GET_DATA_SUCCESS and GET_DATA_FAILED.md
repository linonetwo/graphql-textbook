# 异步 action 一千八，再也不用写它啦

我们之所以有这么多的异步 Action，很大程度上是为了获得乐观更新：给一条朋友圈点赞之后立即看到自己的名字出现在这条朋友圈的点赞列表里。  
要做到这一点，你需要知道点赞发出的网络请求是否失败了，我们首先乐观地认为网络请求成功，立即更新界面，只有远方传来失败了的消息，我们才伤心地在界面上撤回这个修改。  
要怎么知道网络请求是否失败了呢？我们一般这么做：  

```javascript
// action types
const GET_DATA = 'GET_DATA';
const GET_DATA_SUCCESS = 'GET_DATA_SUCCESS';
const GET_DATA_FAILED = 'GET_DATA_FAILED';
// action creator
function getDataAction(id) {
  return function optimisticUpdateData(dispatch, getState) {
    dispatch({
      type: GET_DATA,
      payload: id
    });
    Promise.try(() =>
      api.getData(id)
    )
    .then((response) => {
      dispatch({
        type: GET_DATA_SUCCESS,
        payload: response
      });
    })
    .catch((error) => {
      dispatch({
        type: GET_DATA_FAILED,
        payload: error
      });
    });
  };
};

// reducer
const reducer = function (oldState, action) {
  switch (action.type) {
    case GET_DATA :
      return oldState;
    case GET_DATA_SUCCESS :
      return successState;
    case GET_DATA_FAILED :
    default:
      return errorState;
  }
};
// 略去业务代码
```

这长度和黑人有得一拼！而且在项目中我们可能要面对七八十个这样的黑人，如果都要帮他们手动弄出来的话，实在是太累了。  
所以有经验的前端会殊途同归地来到 redux-saga 面前，膜拜数据库管理员前辈们的伟大思想。

```javascript
import { takeEvery } from 'redux-saga';
import { call, put } from 'redux-saga/effects';

function* getData(action) {
  try {
    const response = yield call(api.getData, action.payload.id);
    yield put({ type: 'GET_DATA_SUCCEEDED', payload: response });
  } catch (e) {
    yield put({ type: 'GET_DATA_FAILED', payload: error });
  }
}

function* mySaga() {
  yield* takeEvery('GET_DATA', getData);
}

export default mySaga;
// 略去业务代码
```

而当使用了 GraphQL，你只需要写：

```javascript
// Nothing about reduxcers
```

事实上连一行注释都不用。我们完全可以自动生成这些常量、action creator、initial state 和 reducers，直接开始写业务逻辑。
