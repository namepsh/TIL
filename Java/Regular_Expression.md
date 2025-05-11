# 정규식
#### 자주 사용 되는 정규식
표현|설명
---|---
^[0-9]*$|숫자
^[a-zA-Z]*$|영문자
^[ㄱ-ㅎ가-힣]*$ |한글
\\w+@\\w+\\.\\w+(\\.\\w+)?|이메일
^\d{2,3}-\d{3,4}-\d{4}$|전화번호
^01(?:0\|1\|[6-9])-(?:\d{3}\|\d{4})-\d{4}$|휴대 전화번호
\d{6} \- [1-4]\d{6}|주민등록번호
^\d{3}-\d{2}$|우편번호

<br>

## matches
> 일치 여부를 판단하여 ture 또는 false를 반환
``` Java
public class TestController {
    public static void main(String[] args) {
        // 포함 여부
        String boolStr = "fjdl";
        // [a-z]+
        // > a-z: 알파벳 소문자 a부터 z까지 중에
        // > +  : 하나 또는 많이 포함하고 있는지(묶음)
        boolean boolResult = boolStr.matches("[a-z]+");
        // [0-9]+
        // > 0-9: 0부터 9까지 숫자 중에
        // > +  : 하나 또는 많이 포함하고 있는지
        boolean boolResult2 = boolStr.matches("[0-9]+");
        System.out.println("소문자로만 이루어져 있는지? " + boolResult);
        System.out.println("숫자로만 이루어져 있는지? " + boolResult2);

        // 문자열의 시작과 끝
        String num1 = "478978";
        String num2 = "fdsa478978fd";
        // ^[0-9]*$
        // [0-9]: 숫자로
        // ^: 시작하는데([] 밖에 있으면 시작을 의미)
        // *: 숫자는 있을 수도 있고 없을 수도 있고
        // $: 숫자로 끝난다
        boolean numResult1 = num1.matches("^[0-9]*$");
        boolean numResult2 = num2.matches("^[0-9]*$");
        System.out.println("숫자로 시작하면서 숫자로 끝났는지?");
        System.out.println("-> num1(478978): " + numResult1);
        System.out.println("-> num2(fdsa478978fd): " + numResult2);
    }
}

```
```
소문자로만 이루어져 있는지? true
숫자로만 이루어져 있는지? false

숫자로 시작하면서 숫자로 끝났는지?
-> num1(478978): true
-> num2(fdsa478978fd): false
```

## replaceAll 
> 정규표현식과 일치하는 모든 값 치환
``` Java
public class TestController {
    public static void main(String[] args) {
        String rplStr = "fdsklAKFJ85890_*";
        // [^a-z0-9]
        // > a-z0-9: 소문자 및 숫자까지를
        // > ^     : 제외한 문자
        String rplResult = rplStr.replaceAll("[^a-z0-9]", "#");
        System.out.println("소문자 및 숫자를 제외한 문자를 모두 #으로 치환: " + rplResult);
    }
}
```
```
소문자 및 숫자를 제외한 문자를 모두 #으로 치환: fdskl####85890##
```

## split
> 정규식과 매치되는 문자열을 구분자로 사용하여 분할 후 배열로 반환
``` Java
public class TestController {
    public static void main(String[] args) {
        String splStr = "fdsklAKFJ85890_*";
        // [A-Z]+
        // > A-Z: 대문자 전체 중에
        // > +  : 하나 또는 많이 포함하고 있는지
        String[] splResult = splStr.split("[A-Z]+");
        for(String s : splResult) {
            System.out.println("대문자를 구분자로 사용하여 분할: " + s);
        }
    }
}
```
```
대문자를 구분자로 사용하여 분할: fdskl
대문자를 구분자로 사용하여 분할: 85890_*
```

## Matcher
> 문자열 패턴을 해석하고 주어진 패턴과 일치하는 판별 후 반환된 필터링된 결과값을 지니고 있다
 
#### Matcher 클래스 메서드 종류 
이름|설명
---|---
find()|패턴이 일치하는 경우 true, 불일치하는 경우 false
find(int start)|start 위치 이후부터 매칭 검색
start()|매칭되는 문자열의 시작 위치 반환
start(int group)|지정된 그룹이 매칭되는 시작 위치 반환
end()|매칭되는 문자열 끝 위치의 다음 문자 위치 반환
end(int group)|지정된 그룹이 매칭되는 끝 위치의 다음 문자 위치 반환
group()|매칭된 부분을 반환
group(int group)|그룹화되어 매칭된 패턴 중 group번째 부분 반환
groupCount()|괄호로 지정해서 그룹핑한 패턴의 전체 개수 반환
matches()|패턴이 전체 문자열과 일치할 경우 true 반환

``` Java
import java.util.regex.Matcher;
import java.util.regex.Pattern;

public class TestController {
    public static void main(String[] args) {
        /* 다중 결과값 출력하기
         * ^[0-9]+
         * 결과: 123
         * 설명: 432는 byebye 뒤에 나오는 값이기 때문에 숫자로 시작하는 거라고 볼 수 없다
         * [0-9]+
         * 결과: 123, 432 나오는데 반복문 출력문에서는 432만 출력됨
         * 설명: 반복문 위에서 matcher.group()으로 123을 이미 출력했기 때문에
         *      반복문 안에서 실행되는 출력문에는 432만 출력된다
         *      matcher.find()는 앞에서부터 차례로 매칭을 시도하며 매칭된 이후에는 그 다음 위치부터 찾기 시작
         */
        System.out.println("<<< 다중 결과값 출력 >>>");
        String matStr = "123byebye432";
        // 패턴으로 만들 문자열 형태의 정규표현식 문법
        String patternStr = "^[0-9]+";
        // 문자열 형태의 정규표현식 문법을 정규식 패턴으로 변환
        Pattern pattern = Pattern.compile(patternStr);
        // 패턴 객체를 matcher 메서드를 통해 문자열을 검사하고 필터링된 결과를 매처 객체로 반환
        Matcher matcher = pattern.matcher(matStr);
        System.out.println("매칭된 결과가 있는지? " + matcher.find());
        System.out.println("매칭된 부분: " + matcher.group());
        while (matcher.find()) {
            System.out.println(matcher.group());
        }

        /* 그룹핑 결과값 출력하기
         * 전화번호 배열 안에 07070 넣으면 해당 변호는 결과값을 출력 x */
        System.out.println("");
        System.out.println("<<< 그룹핑 결과값 출력 >>>");
        // 배열로 전화번호를 받는다
        String[] tel = {"0707-123-4567", "02-1235-7897"};
        // 패턴으로 만들 정규표현식 문자열
        // 0\d{1,3})-(\d{3,4})-(\d{4}
        // 0: 0으로 시작
        // \d: 숫자만 허용
        // {1,3}: 1~3자리
        String telPatt = "(0\\d{1,3})-(\\d{3,4})-(\\d{4})";
        for(String s : tel) {
            // 패턴 변환 + 매처 객체 반환
            Matcher telMat = Pattern.compile(telPatt).matcher(s);
            // 반드시 find() 또는 matches() 먼저 호출
            if (telMat.matches()) {
                System.out.println("전화번호 전체: " + telMat.group(0));   // 전체
                System.out.println("지역번호: " + telMat.group(1));       // 그룹 1
                System.out.println("중간번호: " + telMat.group(2));       // 그룹 2
                System.out.println("끝번호: " + telMat.group(3));        // 그룹 3
            }
        }
    }
}
```
```
<<< 다중 결과값 출력 >>>
매칭된 결과가 있는지? true
매칭된 부분: 123

<<< 그룹핑 결과값 출력 >>>
전화번호 전체: 0707-123-4567
지역번호: 0707
중간번호: 123
끝번호: 4567
전화번호 전체: 02-1235-7897
지역번호: 02
중간번호: 1235
끝번호: 7897
```