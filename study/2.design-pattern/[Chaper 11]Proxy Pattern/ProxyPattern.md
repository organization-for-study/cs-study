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

### 1-1. RMI 쉽게 이해 해보자!

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

### 1-2. 가상 프록시

>
1. 가상 프록시를 통해 생성하기 힘든 객체 자원으로의 접근을 제어 할수 있습니다.
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











