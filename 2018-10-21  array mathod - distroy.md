
 # TIL(Today I learend)


##  Array Mathod 

### 1. 원본배열을 파괴 시키지 않음 
 #### 1. Array.prototype.slice(begin?,end?)
   - begin에서 시작해 end 바로 앞까지 복사해 넣은 새 배열을 반환합니다
  
  ```
    const arr = [1, 2, 3, 4, 5];

    const newArr = arr.slice(2, 4); // [3, 4]

    newArr[0] = 5;

    console.log(newArr); // [5, 4]

    console.log(arr); // [1, 2, 3, 4, 5]

```
   - 원본배열에는 영향을 미치지 않습니다.
   - end 를 생략하면 배열의 길이를 기본값으로 씁니다.
   - 두 인덱스를 모두 생략하면 배열을 복사합니다.

```
> [1, 2, 3, 4, 5].slice()
> [1, 2, 3, 4, 5]
```
- 인덱스가 음수면 배열의 길이를 더합니다. 

---
#### 2. Array.prototype.join(separator?)
- 배열요소 전체에 toString()을 적용해 문자열로 바꾸고 그 사이에 seperator를 끼워넣어 만든 문자열을 반환합니다.
```
const arr = [1, 2, 3];

arr.join('&'); // '1&2&3'
```

- seperator을 생략하면 기본값은 ','입니다
```
const arr = [1, 2, 3];

arr.join(); // '1,2,3'
```
- join()은 undifined와 null을 빈 문자열로 바꿉니다.

```
>[undefined, null].join('#')
>'#'
```

- 배열에 있는 구멍도 빈 문자열로 바뀝니다
```
>['a', ,'b'].join('-')
>'a--b'



   










---
## Today I Found / Feel

배열 매서드를 익히는게 쉽지가 않았다. 계속 봐도 정리가 잘 안되었다. 그래서 알고리즘을 풀 때 꼭 다시찾아봐야 하거나, 또 그 메서드를 알고리즘에서 사용하는게 쉽지 않아서,개념을 천천히 다시 익히면서 그것을 이용한 알고리즘도 다시봐야겠다. 하지만 메서드가 많기 때문에 한꺼번에 하는 것은 어려울 것 같아서 매일 조금씩 정리를 해나야 겠다.

---