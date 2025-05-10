# 정규식
## matches
> 일치 여부를 판단하여 ture 또는 false를 반환
``` Java
public class TestController {
    public static void main(String[] args) {
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
    }
}

```
```
소문자로만 이루어져 있는지? true
숫자로만 이루어져 있는지? false
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