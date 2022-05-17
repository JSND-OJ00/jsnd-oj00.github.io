# Underbar Library

underbar는 자바스크립트의 배열이나 객체를 다루기 위한 라이브 러리이며, 배열 메소드와 유사한 기능, 더 유용할지도 모를 기능, 쓸모 없을지도 모를 기능(뭐든 쓰기 나름이겠지만)들이 정의되어 있다. 오늘 underbar의 기능들을 구현해보며 바닐라 js의 배열 메소드가 어떻게 동작하는지 짐작해 볼 수 있었다. 마지막으로 느낀점이 있다면 이런 라이브러리 같은 거 만드는 사람, 대단하다.

.identity : 전달인자가 무엇이든, 그대로 리턴

```jsx
_.identity = function (val) {
  return val;
};
```

_.slice : 배열의 start 인덱스부터 end 인덱스 이전까지의 요소를 shallow copy하여 새로운 배열을 리턴

```jsx
 _.slice = function (arr, start, end) {
  let _start = start || 0, // `start`가 undefined인 경우, slice는 0부터 동작합니다.
    _end = end;

  // 입력받은 인덱스가 음수일 경우, 마지막 인덱스부터 매칭한다. (예. -1 => arr.length - 1, -2 => arr.length - 2)
  // 입력받은 인덱스는 0 이상이어야 한다.
  if (start < 0) _start = Math.max(0, arr.length + start);
  if (end < 0) _end = Math.max(0, arr.length + end);

  // `end`가 생략될 경우(undefined), slice는 마지막 인덱스까지 동작합니다.
  // `end`가 배열의 범위를 벗어날 경우, slice는 마지막 인덱스까지 동작합니다.
  if (_end === undefined || _end > arr.length) _end = arr.length;

  let result = [];
  // `start`가 배열의 범위를 벗어날 경우, 빈 배열을 리턴합니다.
  for (let i = _start; i < _end; i++) {
    result.push(arr[i]);
  }

  return result;
};
```

_.take : 배열의 head로부터 n개의 element를 담은 새로운 배열을 리턴

```jsx
_.take = function (arr, n) {
  let _n = n;
  let result = [];
  if (_n === undefined || _n < 0) return result;
  if (_n > arr.length) return (result = arr);
  for (let i = 0; i < _n; i++) {
    result.push(arr[i]);
  }
  return result;
};
```

_.drop :  _.take와 달리 head로부터 n개의 element를 drop하고 새로운 배열을 리턴

```jsx
_.drop = function (arr, n) {
  let _n = n;
  let result = [];
  if (_n === undefined || _n < 0) return (result = arr);
  if (_n > arr.length) return result;
  for (let i = _n; i < arr.length; i++) {
    result.push(arr[i]);
  }
  return result;
};
```

_.last : 배열의 tail로부터 n개의 element를 담은 새로운 배열을 리턴

```jsx
_.last = function (arr, n) {
  let _n = n;
  let result = [];
  if (_n === undefined || _n < 0) {
    result.push(arr[arr.length - 1]);
  }
  if (_n > arr.length) return (result = arr);
  for (let i = arr.length - _n; i < arr.length; i++) {
    result.push(arr[i]);
  }
  return result;
};
```

_.each : 

_.each는 collection의 각 데이터에 반복적인 작업을 수행합니다.
1. collection(배열 혹은 객체)과 함수 iteratee(반복되는 작업)를 인자로 전달받아 (iteratee는 함수의   인자로 전달되는 함수이므로 callback 함수)
2. collection의 데이터(element 또는 property)를 순회하면서
3. iteratee에 각 데이터를 인자로 전달하여 실행합니다.

* iteratee는 차례대로 데이터(element 또는 value), 접근자(index 또는 key), collection을 다룰 수 있어야 합니다.

*  배열 arr을 입력받을 경우, iteratee(ele, idx, arr)

*  객체 obj를 입력받을 경우, iteratee(val, key, obj)

* 이처럼 collection의 모든 정보가 iteratee의 인자로 잘 전달되어야 모든 경우를 다룰 수 있습니다.

* 실제로 전달되는 callback 함수는 collection의 모든 정보가 필요하지 않을 수도 있습니다.

_.each는 명시적으로 어떤 값을 리턴하지 않습니다.

```jsx
_.each = function (collection, iteratee) {
  if (Array.isArray(collection)) {
    for (let i = 0; i < collection.length; i++) {
      iteratee(collection[i], i, collection);
    }
  } else if (typeof collection === "object") {
    for (let key in collection) {
      iteratee(collection[key], key, collection);
    }
  }
};
```

_.indexOf : target으로 전달되는 값이 arr의 요소인 경우, 배열에서의 위치(index)를 리턴

없으면 -1 리턴

target과 일치하는 element가 복수 존재하는 경우, 가장 낮은 index 리턴

```jsx
_.indexOf = function (arr, target) {
  let result = -1;

  _.each(arr, function (item, index) {
    if (item === target && result === -1) {
      result = index;
    }
  });

  return result;
};
```

_.filter :  test 함수를 모두 통과하는 모든 요소를 담은 새로운 배열을 리턴

test(element)의 결과(return 값)가 truthy할 경우 통과

```jsx
_.filter = function (arr, test) {
  let result = [];

  _.each(arr, function (item) {
    if (test(item)) {
      result.push(item);
    }
  });
  return result;
};
```

_.rejext : test와 반대로 test 함수를 통과하지 못한 모든 요소를 담은 새로운 배열을 리턴

```jsx
_.reject = function (arr, test) {
  let result = [];

  _.each(arr, function (item) {
    if (!test(item)) {
      result.push(item);
    }
  });
  return result;
};
```

_.uniq : 주어진 배열의 요소 중 중복이 없는(고유한) 요소를 담은 새로운 배열을 리턴

엄격한 동치 연산(strict equally, ===)을 사용

```jsx
_.uniq = function (arr) {
  let result = [];

  _.each(arr, function (item) {
    if (_.indexOf(result, item) === -1) {
      result.push(item);
    }
  });
  return result;
};
```

_.map : iteratee(반복괴는 작업)를 배열의 각 요소에 적용(apply)한 결과를 담은 새로운 배열을 리턴

```jsx
_.map = function (arr, iteratee) {
  let result = [];

  _.each(arr, function (item) {
    result.push(iteratee(item));
  });
  return result;
}; 
```

_.pluck : 

1. 객체 또는 배열을 요소로 갖는 배열과 각 요소에서 찾고자 하는 key 또는 index를 입력받아
2. 각 요소의 해당 값 또는 요소만을 추출하여 새로운 배열에 저장하고,
3. 최종적으로 새로운 배열을 리턴합니다.
예를 들어, 각 개인의 정보를 담은 객체를 요소로 갖는 배열을 통해서, 모든 개인의 나이만으로 구성된 별도의 배열을 만들 수 있습니다.
최종적으로 리턴되는 새로운 배열의 길이는 입력으로 전달되는 배열의 길이와 같아야 합니다.
따라서 찾고자 하는 key 또는 index를 가지고 있지 않은 요소의 경우, 추출 결과는 undefined 입니다.

```jsx
_.pluck = function (arr, keyOrIdx) {
  // _.pluck을 _.each를 사용해 구현하면 아래와 같습니다.
  // let result = [];
  // _.each(arr, function (item) {
  //   result.push(item[keyOrIdx]);
  // });
  // return result;
  // _.pluck은 _.map을 사용해 구현하시기 바랍니다.
  // TODO: 여기에 코드를 작성합니다/
  return _.map(arr, function (item) {
    return item[keyOrIdx];
  });
};
```

_.reduce :

1. 배열을 순회하며 각 요소에 iteratee 함수를 적용하고,

2. 그 결과값을 계속해서 누적(accumulate)합니다.

3. 최종적으로 누적된 결과값을 리턴합니다.

예를 들어, 배열 [1, 2, 3, 4]를 전부 더해서 10이라는 하나의 값을 리턴합니다.

각 요소가 처리될 때마다 누적되는 값은 차례대로 1, 1+2, 1+2+3, 1+2+3+4 입니다.

이처럼 _.reduce는 배열이라는 다수의 정보가 하나의 값으로 축소(응축, 환원, reduction)되기 때문에 reduce라는 이름이 붙게 된 것입니다.

_.reduce는 위에서 구현한 많은 함수처럼, 입력으로 배열과 각 요소에 반복할 작업(iteratee)을 전달받습니다.

iteratee에 대해서 복습하면 아래와 같습니다. (일반적으로 객체를 reduce 하지는 않으므로, 배열 부분만 복습합니다.)

iteratee는 차례대로 데이터(element 또는 value), 접근자(index 또는 key), collection을 다룰 수 있어야 합니다.

배열 arr을 입력받을 경우, iteratee(ele, idx, arr)

_.reduce는 반복해서 값을 누적하므로 이 누적되는 값을 관리해야 합니다.

따라서 _.reduce의 iteratee는 인자가 하나 더 추가되어 최종 형태는 아래와 같습니다.

iteratee(acc, ele, idx, arr)

누적되는 값은 보통 tally, accumulator(앞글자만 따서 acc로 표기하기도 함)로 표현하거나

목적을 더 분명하게 하기 위해 sum(합), prod(곱), total 등으로 표현하기도 합니다.

이때, acc는 '이전 요소까지'의 반복 작업의 결과로 누적된 값입니다.

ele는 잘 아시다시피 반복 작업을 수행할(아직 수행하지 않은) 현재의 요소입니다.

여기까지 내용을 정리하면 다음과 같습니다.

_.reduce(arr, iteratee)

iteratee(acc, ele, idx, arr)

그런데 사실 누적값에 대해서 빠뜨린 게 하나 있습니다.

바로 '누적값은 어디서부터 시작하는가'라는 의문에 대한 대답을 하지 않았습니다.

이를 해결하는 방법은 초기 값을 직접 설정하거나 자동으로 설정하는 것입니다.

_.reduce는 세 번째 인자로 초기 값을 전달받을 수 있습니다.

이 세 번째 인자로 초기 값이 전달되는 경우, 그 값을 누적값의 기초(acc)로 하여 배열의 '첫 번째' 요소부터 반복 작업이 수행됩니다.

반면 초기 값이 전달되지 않은 경우, 배열의 첫 번째 요소를 누적값의 출발로 하여 배열의 '두 번째' 요소부터 반복 작업이 수행됩니다.

따라서 최종적인 형태는 아래와 같습니다.

_.reduce(arr, iteratee, initVal)

iteratee(acc, ele, idx, arr)

아래 예제를 참고하시기 바랍니다.

const numbers = [1,2,3];

const sum = _.reduce(numbers, function(total, number){

return total + number;

}); 

// 초기 값이 주어지지 않았으므로, 초기 값은 배열의 첫 요소인 1입니다. 두 번째 요소부터 반복 작업이 시작됩니다.

      // 1 + 2 = 3; (첫 작업의 결과가 누적되어 다음 작업으로 전달됩니다.)

      // 3 + 3 = 6; (마지막 작업이므로 최종적으로 6이 리턴됩니다.)

const identity = _.reduce([3, 5], function(total, number){

   return total + number * number;

 }, 2); // 초기 값이 2로 주어졌습니다. 첫 번째 요소부터 반복 작업이 시작됩니다.

         // 2 + 3 * 3 = 11; (첫 작업의 결과가 누적되어 다음 작업으로 전달됩니다.)

         // 11 + 5 * 5 = 36; (마지막 작업이므로 최종적으로 36이 리턴됩니다.)

```jsx
_.reduce = function (arr, iteratee, initVal) {
  let acc = initVal;
  _.each(arr, function (item, idx) {
    if (initVal === undefined && idx === 0) {
      acc = item;
    } else {
      acc = iteratee(acc, item, idx, arr);
    }
  });
  return acc;
};
```
