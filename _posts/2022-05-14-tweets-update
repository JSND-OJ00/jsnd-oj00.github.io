# 트윗 필터링 기능 추가

```jsx
<select value={currentUserName} onChange={handleFilterTweet}>
          <option value="default">All</option>
          {tweets
            .reduce((acc, cur) => {
              // acc에 트윗이 차곡차곡 쌓입니다.
              // const isNotUnique는 트윗 유저네임의 유니크함을 판별합니다.
              const isNotUnique = acc.reduce((a, c) => {
                // acc에 새로운 트윗(cur)을 넣기 전에 acc내부를 순회하여 acc에 쌓인 트윗들의 유저 네임과 필터링 전인 트윗 cur의 유저네임을 비교합니다.
                if (c.username === cur.username) {
                  return true;
                }
                return a === true ? true : false; // 일치하는 유저네임이 없다면(유니크하다면) false가 반환됩니다.
              }, false);

              return isNotUnique ? acc : [...acc, cur]; // isNotUnique가 false 라면(트윗의 유저네임이 유니크 하다면) 옵션을 생성하기 위한 배열에 트윗에 샇이게 됩니다.
            }, [])
            .map((tweet) => {
              return (
                <option key={tweet.id} value={tweet.username}>
                  {tweet.username}
                </option>
              );
            })}
        </select>
```

select를 사용하여 표시할 유저를 선택합니다.

option에는 유저명이 담기게 되는데, 중복되는 유저명이 없도록 reduce를 사용하여 reduce를 사용, 필터링해줍니다.

```jsx
const [filteredTweets, setFilteredTweets] = useState(dummyTweets);
const [isFiltered, setIsFiltered] = useState(false);
const [currentUserName, setCurrentUserName] = useState("default");
```

사용되는 state는 위와 같습니다.

```jsx
const handleFilterTweet = (event) => {
    if (event.target.value === "default") {
      setTweets(tweets);
      setIsFiltered(false);
    } else {
      const filtered = tweets.filter(
        (tweet) => tweet.username === event.target.value
      );
      setIsFiltered(true);
      setFilteredTweets(filtered);
    }
    setCurrentUserName(event.target.value);
  };
```

select의 value가 defalut라면, 즉 선탣된 유저가 없다면 디폴트 상태의 트윗(dummyTweets)이 사용됩니다.

유저가 선택되었다면 select에 유저명이 담기고, 전체 트윗을 순회하여 선택된 유저명과 일치하는 유저명을 지닌 트윗만 filtered에 넣어줍니다.

setIsFilterd는 Tweet컴포넌트 렌더링에 기본 트윗이 쓰이는지, 필터링된 트윗이 쓰이는지를 정해줍니다.

기본 트윗의 변경을 막기 위해 핑터링된 트윗은 filteredTweets에  저장됩니다.
