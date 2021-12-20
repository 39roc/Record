# SOLID Principle

> 클린 아키텍처에서 설명하는 SOLID 원칙에 대해 정리



- SRP
- OCP
- LSP
- ISP
- DIP



### SRP

**단일 모듈은 변경의 이유가 하나, 오직 하나뿐이어야 한다.**

=> 하나의 모듈은 하나의, 오직 하나의 액터에 대해서만 책임져야 한다.

> 액터: 해당 변경을 요청하는 한명  이상의 사람



원칙을 위반하는 징후들

- 우발적 중복
- 병합



#### 우발적 중복

```
class Employee {

  public void calculatePay() {
    regularHours();
  }

  public void reportHours() {
    regularHours();
  }

  public void save() {
  }

  private void regularHours() {
  }

}

calculatePay, reportHours 메서드 내부에서 regularHours 을 호출하는 상황이다.
만약 calculatePay 의 변경 요청으로 인하여 regularHours 를 변경할 경우 reportHours 도 영향을 받으므로,
이는 단일 책임의 원칙을 위배한다.
```



#### 병합

Employee 테이블의 스키마가 변경 될 경우 calculatePay, reportHours, save 을 각각 호출하는 액터에게 모두 영향을 준다.

그러므로 단일 책임의 원칙을 위반한다.



#### 해결

- 데이터와 메서드를 분리하는 방식

```java
class PayCalculator {
  public void calculatePay() {
  }
}

class HourReporter {
  public void reportHours() {
  }
}

class EmployeeSaver {
  public void saveEmployee() {
  }
}

class EmployeeData {
}

세가지 클래스를 인스턴스화하고 추적해야 하는 단점
=> Facade 패턴
```



- Facade 패턴

```java
class EmployeeFacade {
  private PayCalculator calculator;
  private HourReporter hourReporter;
  private EmployeeSaver employeeSaver;
  
  public void calculatePay() {
    calculator.calculatePay();
  }
  public void reportHours() {
    hourReporter.reportHours();
  }
  public void saveEmployee() {
    employeeSaver.saveEmployee();
  }
}
```



- 비지니스 로직과 데이터를 가깝체 배치하는 방식

```java
class Employee {

  private String employeeData;
  private HourReporter hourReporter;
  private EmployeeSaver employeeSaver;
  public void calculatePay() {
    // do something
  }

  public void reportHours() {
    hourReporter.reportHours();
  }

  public void save() {
    employeeSaver.saveEmployee();
  }

}

가장 중요한 메서드(calculatePay)는 그대로 유지하고 나머지 메서드들에 대해서 Facade 이용
```



<br>

### OCP

소프트 웨어 개체는 확장에는 열려 있어야 하고, 변경에는 닫혀 있어야 한다.

