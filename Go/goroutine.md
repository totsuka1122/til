# ctx

## 子のctxがキャンセルされたとき、親コンテキストを終了させる方法

1. 親のcancelをコールする

2. sync.ErrGroupを使用する(recommendation)
- https://deeeet.com/writing/2016/10/12/errgroup/
