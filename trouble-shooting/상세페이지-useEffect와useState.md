## 상세페이지- useEffect useState
### 문제점
상세페이지로 이동 할 경우 useEffect를 이용하여 페이지가 실행되자마자 값을 DB로 부터 받아와 나타내주도록 하였다.   
`postId`를 받아와 Spring으로 해당하는 데이터를 조회해야 하는데 `postId`값을 받지 못하여서 계속해서 null로 넘어가 화면이 안 뜨는 에러가 발생하였다.

### 해결방법
분명이 게시물 생성에서 상세페이지로 넘겨주는 `postId`값을 확인하였을 때는 null이 아닌 값이 존재하였는데   
상세페이지로 넘어가면 null값이 찍히는걸 확인하여 useEffect부분에 useState가 제데로 작동하지 않는 것 같아 작동원리를 찾아보았다.   
   
#### useState
- 예를들어, 아래처럼 `postId`가 선언되어 있으면
```java
const [postId, setPostId] = useState(null);
```
`postId`의 최초값은 null이다.   
 `setPostId(1)`이라고 하고 실행 시키면 `postId`에 바로 1이라는 값이 적용되는것이 아닌 rerendering후에 1이라는 값이 적용된다.
 때문에 이와같이
 ```java
  useEffect(() => {
    setPostId(location.state.postId)
    console.log(postId);
    getPost(); 
  },[]);
 ```
 실행하게 된다면 rerendering하기 전에는 `postId`의 값에 `location.state.postId`가 적용될 수 없다.   
 Spring에서 데이터를 가져오는 시점이 rerendering하기 전이므로 당연히 `postId`의 값에는 null값이 할당되어 있는 것이다.   
    
따라서 이와같이 코드를 수정하였더니 더 이상 null값으로 데이터를 주지 않게 되었다. 
 ```java
 const [postId, setPostId] = useState(location.state.postId);
 
 useEffect(() => {
    console.log(postId);
    getPost(); 
  },[]);
 ```
 
 
