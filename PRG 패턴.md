# PRG (Post/Redirect/Get)

웹 브라우저의 **새로고침은 마지막에 서버에 전송한 데이터를 다시 전송**한다.    

**PRG 패턴 적용 전**
![image](https://user-images.githubusercontent.com/108853290/180989004-825e5b18-adb1-4257-aa7c-0795941599f6.png)
    
    
상품을 등록하는 기능과 매핑되어있는 url에서 새로고침을 하게 될 경우,    
이전에 등록했던 상품등록 폼의 데이터를 가지고 POST 요청이 반복되어 등록되는 문제가 발생한다.

이를 막기 위해서   
`POST 요청을 하고 난 뒤 GET방식으로 Redirect` 시키는 방식을 쓸 수 있다.    
**마지막의 기능을 GET으로 돌림으로써 새로고침해도 POST방식이 남지 않게 한다.**     


**PRG 패턴 적용 후**
![image](https://user-images.githubusercontent.com/108853290/180990537-14eb131b-d0dd-412d-be1f-8e5cdc30a740.png)
