# JAVA - NullPointerException

## 사건의 전말  
 필자는 공모전을 위해 자바를 이용하여 프로그램을 짜고 있었다. 프로그램을 완성하고 난 뒤, 프로그램을 실행하였으나 NullPointerException이라는 아주 이상한 친구를 만날 뿐, 프로그램이 시작되지 않는다. 코드와 함께 상황을 알아보자. 

```
Exception in thread "main" java.lang.NullPointerException:
 Cannot assign field "x" because "this.loc" is null
        at proj1.Ship.SetLoc(Ship.java:12)
	at proj1.Voyage.Settings(Voyage.java:14)
	at proj1.testDrive.main(testDrive.java:7)
```
```
// main
public static void main(String[] args) {  
    Voyage test = new Voyage();  
    test.Settings(25, "path");  
    test.Sail();  
}
```
```
public class Voyage {  
	 ....
	 public void Settings(int length,String path){  
	      myship.SetLimit(length);  
	      myship.SetLoc(length-1,0);  
	      myship.SetStorage(path);  
     }
  
  
```
```
public class Ship {  
    private Point loc;
    ...
    public void SetLoc(int x, int y) {  
        loc.x = x;  
	loc.y = y;  
    }
```

즉 main에서 voyage를 하나 만들어 그 안에서 ship을 출발시키는 방식으로 프로그램이 시작된다. 그래서 ship의 시작 좌표를 넣어주는 일을 하는데 여기서 에러가 생긴 것이다.   

생각해보면 에러가 날 일이 없다. voyage 객체가 ship 객체를 만들고, 
ship객체가 만들어지면서 인스턴스 변수로 선언된 loc을 만들 것이다. 그런데 왜 loc이 null이라고 하는 걸까?   

인터넷을 검색해봐도 답을 찾을 수 없었다. 그래서 인스턴스 생성의 기본 원리를 생각해보기로 하였다. 

1. 레퍼런스 변수 생성 :  Dog mydog  
2. 객체 생성 :  new Dog();	
3. 레퍼런스와 객체 연결 : Dog mydog = new Dog();
4. 객체 생성 후 인스턴스 변수 또한 메모리에 할당된다. 

그렇다. 에러가 떴을 당시 Point객체 loc을 인스턴스 변수로 생성했으며, 정상적이라면 loc또한 할당이 되었어야 한다. 이때, 무언가가 뇌리를 스치었다. 

</br></br>
__"인스턴스 변수인  Point형   loc도 객체잖아. loc도 객체처럼 선언했어???"__
</br></br>
  </br>
오 마이 갓. 코드를 확인해보자. 
```
public class Ship {  
    private Point loc; // ????????
```
이럴수가. 파이썬에 너무 심취한 나머지 모든 코드의 인스턴스 변수로 선언된 객체를 일반 변수와 같은 방법으로 선언하였던 것이다.....  

모든 인스턴스 변수로 선언된 객체를 올바르게 선언해주자, 프로그램이 정상적으로 작동하였다....   

생각해보니 모든 인스턴스 변수로 선언한 객체에서 이런 문장이 보였던 것 같다. 

```
Private field 'Instance variable' 
is never assigned 
```
생각해보니 이 말이 맞았다. = new Instance(); 를 붙이지 않았으니깐......

 
 # 결론 
  NullPointerExcpetion : Cannot assign field "something"
  because "something2" is null. 
  
  이 에러가 보인다면 먼저 객체를 올바르게 선언했는지 확인해보자. 

</br>

# PS
  자바가 너무 밉다........

21.08.24 ICU222 작성







