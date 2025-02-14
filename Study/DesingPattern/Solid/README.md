## Solid 원칙

객체지향 프로그래밍을 하면서 지켜야 하는 중요 5대 원칙

### SRP(Single Responsibility Principle)
### 단일 책임 원칙

각 클래스(객체)는 하나의 책임만을 가져야 한다는 원칙.

```C++
class Door
{
  bool isOpen;
  public Open();
  public Close();
}
```
Door가 있을 경우, Door은 기본적으로 열리고 닫히며 공간을 연결하거나 단절하는 기능을 지니고 있는데, 
해당 책임을 벗어나는 구현을 하게 되면 비직관적이게 되고 유지보수가 어려워진다.

```C++
class Door
{
  bool isOpen;
  public Open();
  public Close();
  public Explode();//불필요
}
```



### OCP(Open Closed Priciple)
### 개방 폐쇄 원칙

변화에는 닫혀있고, 확장에는 열려있어야 한다는 원칙

```C++
class AllDoorSystem
{
  vector<Door> doors;
  
}
class Door
{
  bool isOpen;
  public Open();
  public Close();
}
```

### LSP(Listov Substitution Priciple)
### 리스코프 치환 원칙

### ISP(Interface Segregation Principle)
### 인터페이스 분리 원칙

### DIP(Dependency Inversion Principle)
### 의존 역전 원칙


https://inpa.tistory.com/entry/OOP-%F0%9F%92%A0-%EA%B0%9D%EC%B2%B4-%EC%A7%80%ED%96%A5-%EC%84%A4%EA%B3%84%EC%9D%98-5%EA%B0%80%EC%A7%80-%EC%9B%90%EC%B9%99-SOLID
사이트 참고하며 공부
