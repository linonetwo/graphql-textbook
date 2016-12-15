

```javascript
const DataLoader = require('dataloader');

const getPostTagsUsingPostId = (ids) => {
  console.log(ids);

  return Promise.resolve(ids);
};

const TagByPostIdLoader = new DataLoader(getPostTagsUsingPostId);

TagByPostIdLoader.load(1);
TagByPostIdLoader.load(2);
TagByPostIdLoader.load(3);

// Force next-tick
setTimeout(() => {
  TagByPostIdLoader.load(4);
  TagByPostIdLoader.load(5);
  TagByPostIdLoader.load(6);
}, 100);

// Force next-tick
setTimeout(() => {
  TagByPostIdLoader.load(7);
  TagByPostIdLoader.load(8);
  TagByPostIdLoader.load(9);
}, 200);

setTimeout(() => {
  TagByPostIdLoader.load(10);
  TagByPostIdLoader.load(11);
  TagByPostIdLoader.load(12);
}, 200);

// [1, 2, 3]
// [4, 5, 6]
// [7, 8, 9, 10, 11, 12]
```