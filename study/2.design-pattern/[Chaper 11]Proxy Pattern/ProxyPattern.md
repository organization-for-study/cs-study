## Proxy Pattern

### 0. Proxy ?

> Proxy는 '대리' 를 의미 합니다.
객체 지향 언어에서 원래의 객체를 대신하여 행동 해야 할 때는 언제 일까요?

### 1-1. 원격 프록시

>첫번째로 알아볼 내용은 다른 힙에 있는 객체의 호출을 돕는 프록시 입니다.

1. 원격 프록시는 서로 다른 주소 공간에 있는 객체의 메소드 호출을 돕습니다.
2. JAVA 프로그램을 이용한 뽑기 기계가 전국에 있다고 가정합니다.
3. 그 뽑기 기계는 각자 네트워크 통신을 하기 위한 IP 를 가지고 있습니다.
4. 우리는 JAVA 프로그램을 통해 원하는 지역의 기계 상태와 재고를 알고 싶습니다.

### 1-2. RMI 쉽게 이해 해보자!

>
전국에 있는 뽑기 기계가 Server 라고 생각하면 됩니다. 재고 및 상태를 알고 싶은 우리는 클라이언트가 되겠죠.
우리는 우리가 알고자 하는 뽑기 기계들의 IP 주소를 알고 있습니다.
뽑기 기계는 각자 rmiregistry를 실행하고 RMI Registry에 자신의 객체 인스턴스를 등록 함으로써
클라이언트의 요청을 받아들일 수 있는 준비를 마칩니다.

(서버) 원격 뽑기기계 인터페이스는 RMI의 Remote 인터페이스를 확장함으로써 네트워크 통신이 가능한 객체가 될 수 있는 토대를 마련합니다.
```java
import java.rmi.*;
public interface 원격 뽑기기계 extends Remote{
	// 메소드 들은 Remote Exception 을 던질 수 있습니다.
}
```

```java
// 기존의 뽑기 기계는 위의 원격 뽑기 기계 인터페이스를 구현 합니다.
public class 뽑기기계 extends UnicastRemoteObject implements 원격 뽑기기계{

	//생성자
	public 뽑기기계(String 위치,int 재고) throws RemoteException{
    	// 생성코드
    }
}

```
(서버) 뽑기 기계의 인스턴스를 생성하고 서버에 생성한 인스턴스를 등록합니다.

```
rmiregistry & //서버 실행
```
```java
public class 서버테스트{
	메인{
		뽑기기계 뽑기기계 인스턴스 = new 뽑기기계(IP,재고);
		Naming.rebind("rmi://IP, 뽑기기계 인스턴스); //rmiregistry에 등록
        }
}
```

(클라이언트) 클라이언트쪽 코드를 살펴보면 원격 프록시 개념이 조금 더 확고해 집니다.

```java
public class 뽑기기계 모니터 {
	원격 뽑기기계 machine; // 원격 뽑기 기계 인터페이스를 구성
    public 뽑기기계 모니터(원격 뽑기기계 machine){
    	this.machine = machine;
    }
    
	public void report(){
    	try{
        	System.out.println(machine.getLocation());
        }
    }
}
```

```java
public class 클라이언트 테스트{

	public static void main(Sring[] args){
    	원격 뽑기기계  machine = (원격뽑기기계) Naming.lookup(rmi://서버 IP); 
        //machine이 프록시 입니다!
        뽑기기계 모니터 모니터 = new 뽑기기계 모니터(machine);
        모니터.report();
    }

}
```

### 2. 가상 프록시

>
1. 가상 프록시를 통해 생성하기 힘든 객체를 생성 중에 프로그램을 정상 동작 시킬 수 있습니다.
2. 가상 프록시는 생성하기 힘든 객체의 레퍼런스를 가지고 있으며, 해당 객체를 생성 및 삭제 할 수 있습니다.
3. 가상 프록시는 주 객체(위에서 얘기한 생성하는데 크기가 큰) 대신 해서 쓰일 수 있게끔 주 객체와 동일한
인터페이스를 구현합니다.

이미지(무거운)를 불러오는 동안 이미지 자리에 대체 텍스트나 이미지(가볍게 생성 가능)를 준비하고
사용자는 이미지가 생성되는 동안 정상적으로 서비스를 이용하는 프로그램을 작성 하려고 합니다.

```java
class 아이콘프록시 implenets 아이콘{
	final URL imageURL;
    volatile 이미지아이콘 imageIcon;
    boolean retrieving = false;
    
    //생성자
	public 아이콘프록시(URl url){
    	this.imageURL = url;
    }
    
    public int 이미지높이(){
    	if(Icon이 생성되었으면)
        	return Icon 높이;
        else
        	retrun 800;
    }
    
    syncghronized void setImageIcon(이미지아이콘 imageIcon){
    	this.imageIcon = imageIcon;
    }
    
    public void paintIcon(){
    	if(Icon이 생성되었으면)
        	imageIcon.paint();
        else{
        	if(!retrieving){	// 이미지 불러오는 중이 아니면
            	retrieving = true;
        		print("이미지를 불러오는 중입니다.") // 대체 텍스트
            	스레드  = new 스레드(new Runnable(){
            		public void run(){
                		try{
                        	setImageIcon(new ImageIcon(imageUrl));
						} catch (Exception e){
                    		e.printStactrace();
                    	}
                	}
             	});
            	스레드.start();
           }
        }    	
    }
}

```

```java
	class 테스트{
    	ImageComponent imageComponent;
    	메인{
        	Icon icon = new ImageProxy(initialURL);
            imageComponent = new ImageComponent(icon);
        }
    }
```

### 3. 보호 프록시 (With Java의 동적 프록시)

> 보호 프록시는 객체로의 접근을 제어 합니다.
객체를 대신하는 프록시 객체에 접근제어에 관한 행동을 추가할 수 있지만 지금은 Java 에서 제공하는
API를 활용하여 접근 제어 프록시를 구현 해보겠습니다.

1. 소개팅 어플리케이션을 구현하려고 합니다. 해당 소개팅 어플리케이션에서는 개인의 개인 정보와 그 사람이
얼마나 인기 있는지 표현하는 호감 지수 필드가 있습니다.
2. 개인은 자신의 개인 정보를 수정 할 수 있지만 자신의 호감지수는 변경할 수 없어야 합니다.
3. 개인은 타인의 개인정보를 수정 할 수 없으며 호감 지수는 변화 시킬 수 있습니다.

```java
public interface Person{
	String getName();
    int getLike();
    
    void setName();
    void decreaseLike();
    void increaseLike();
    
}
```

```java
public class PersonImpl implements Person{
 	String name;
 	int like;
    
    String getName(){
    	return this.name;
    }
    
    void setName(String name){
    	this.name = name;
	}
    int getLike(){
    	return this.like;
    }
    
    void increaseLike(){
    	this.like++;
	}
    void decreaseLike(){
    	this.like--;
	}
}
```

이제 위의 객체에 접근을 제어할 프록시를 구현 해봅시다.	프록시는 Java 내장 메소드를 통해 실행 중 생성할 예정입니다. 우리는 프록시를 직접 구현하는 것이 아니기 때문에 다른 방법을 이용하여 프록시의 행동을 결정 하려고 합니다.

```java
Person getOwnerProxy(Person person){
	return (Person)Proxy.newProxyInstance(	//
    person.getClass().getClassLoader(),
    person.getClass().getInterface(),
    new OwnerInvocationHandler(person)); // 프록시의 행동을 Handler 에서 구현 합니다.
}	

// getNonOwnerProxy 메소드는 NonOwnerInvocationHandler를 인자로 받습니다.

```

Handler의 구현을 살펴보기전에 중간 정리를 해보도록 합시다.

1. 우리는 Person 객체에 직접 set , get 하지 않아야 합니다.
2. Person 객체 대신에 Proxy 객체가 들어갈 수 있어야 하기 때문에 Proxy는 Person Interface를 
구현 해야 합니다. (위의 코드를 보면 자바 내장 API를 통해 자동으로 proxy를 생성 합니다. Person 인터페이스를 가져와 구현하는 것까지 전부 자바 자체에서 지원합니다.)
3. 개인이 자신의 호감 지수를 변경 하려고 할 때 프록시의 동작 과정은 아래와 같습니다.

```java
	메인{
 		Person 철수 = new Person("철수",0);
        Person 영희 = new Person("영희",0);
 		Person 철수의 본인 프록시 = getOwnerProxy(철수);
        // 본인 프록시는 철수가 자기의 호감 지수를 변경하지 못하도록 합니다.
        
        철수 본인 프록시.incraseLike(); // 예외 발생!
        철수 본인 프록시.getName(); // 철수는 철수의 이름을 조회 할 수 있습니다.
        Person 철수의 타인 프록시 = getNonOwnerProxy(영희);
        철수 타인 프록시.increaseLike(); // 영희의 호감도를 상승 
        철수 타인 프록시.setName("바보"); // 예외 발생! 
 }
 ```
 
 > 철수 본인 프록시.getName > Owner 핸들러에 method 전달 > 핸들러에서 검증한 후
철수 객체의 getName 메소드를 실행

그럼 Handler의 구현을 간단하게 살펴봅시다.
Handler는 두개를 구현해야 합니다.

```java
import java.lang.reflect.*;

public class OwnerInvocationHandler implements InvocationHandler{
	Person person;
}
public OwnerInvocationHandler(Person person){
	this.person = person;
}

public Object invoke(Object proxy, Method method, Object[] args)
throws IllegalAccessException {

	try{
    	if(method.getName().startsWith("get")){
        	return method.invoke(person,args); //  철수 객체 메소드 실행 
        } else if(method.getName().equals("increaseLike")){
        	throw new IllegalException();
		}
    }
	
}

```

1. 메소드를 통해 Porxy 인스턴스를 생성 할때 Handler 의 레퍼런스를 인자로 넘깁니다.
```java
Person getOwnerProxy(Person person){
	return (Person)Proxy.newProxyInstance(	
    person.getClass().getClassLoader(),
    person.getClass().getInterface(),
    new OwnerInvocationHandler(person)); // 여기!!
}	
```

2. 프록시가 호출한 메소드를 핸들러에서 받고 진짜 객체의 메소드를 실행할지 예외를 던질지 판단합니다.

```java
public Object invoke(Object proxy, Method method, Object[] args)
throws IllegalAccessException {

	try{
    	if(method.getName().startsWith("get")){
        	return method.invoke(person,args); //  철수 객체 메소드 실행 
        } else if(method.getName().equals("increaseLike")){
        	throw new IllegalException();
		}
    }	
}
```

### 4. 프록시 패턴 in Spring AOP 










