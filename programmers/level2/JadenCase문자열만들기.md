## 문제 요약

### 문제

- 첫글자 대문자, 나머지 글자 소문자 인 문자열을 **JadenCase** 라고 함.

### 입력

- 1이상 200이하 문자열

### 조건

- 숫자는 단어의 첫문자로만 나오며 숫자로만 이루어진 단어 없음
- 공백문자 연속해서 나올 수 있음

### 출력

- 문자열이 주어졌을때 변환하여 출력

## 내 코드

```java
//v230708
class Solution {
    public String solution(String s) {
        String answer = "";
        String temp="";
        for(int i=0; i<s.length(); i++){
            char t=s.charAt(i);

            if(t==' '){
                temp+=" ";
                temp=temp.toLowerCase();
                answer+=temp.substring(0,1).toUpperCase()+temp.substring(1,temp.length());
                temp="";
            }else{
                temp+=t;
            }
            if(i==s.length()-1&& temp.length()!=0){
                temp=temp.toLowerCase();
                answer+=temp.substring(0,1).toUpperCase()+temp.substring(1,temp.length());
            }
        }


        return answer;
    }
}
```

### 생각해야 할 것

1. 공백이 두개가 주어질 수 있음을 생각 -> split(" ")을 사용할 수 없음

2. (개선사항) lowercase로 변환 이후 모든 알파벳을 String 으로 입력 -> flag(공백 여부, 첫글자인지 확인) 에 따라서 upper로 바꿀지만 확인
