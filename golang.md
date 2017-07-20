*GO언어 시작하기*

1. 설치
 
 ```
 https://golang.org/
 > Download Go
 ```

2. 테스트 환경 만들기

 ```
 Go Web Framework에는 대표적으로 Revel이 있지만... 여기선 그냥 서브라임으로...
 (웹 프레임웍 종류는 'http://golang.site/go/article/112-Go-웹-프레임워크' 에서 확인)
 
 서브라임 실행 > command + shift + p > install package > GoSublime 설치
 서브라임 재실행 아래 코드를 작성 후 저장
 ```
 > main.go
 
 <pre><code>package main
	import "fmt"
	
	func main() {
	     fmt.Println("Hello, World!")
	}
 </code></pre>
 ```
 command + b
 콘솔이 나오면 'go run' 입력으로 실행
 'Hello, World!' 가 출력되면 OK!
 ```
 
3. 기본문법

 ```
 구구단 n단을 출력하는 코드로 기본문법을 익혀보자
 > 폴더생성 gugudan
 > cd gugudan, gugudon.go 생성
 
 ```
 > gugudan.go
 
 <pre><code>package gugudan
	import (
	    "fmt"
	    "strconv"
	)
	
	func Gugudan(x int) string {
	
		if x > 9 || x < 2 {
			return "2~9만 지원함."
		} else {
			fmt.Println(toString(x) + "단 출력 시작")
		}
		
		for i := 0; i < 9; i++ {
			fmt.Println(toString(x) + " * " + toString(i + 1) +  " = " + toString(x * (i + 1)))
		}	
	
		return toString(x) + "단 출력 완료"
	}
	
	func toString(num int) string {
		
		return strconv.Itoa(num)
	}
 </code></pre>
 
 > main.go
 
 <pre><code>package main
	import (
		"./gugudan"
		"fmt"
	)
	
	func main() {
    	var result = gugudan.Gugudan(2)
		fmt.Println(result)
	}
 </code></pre>

4. go 루틴, 2단과 3단을 동시에 출력해 볼까요?
	> 실행할 함수 앞에 go를 써주면 됩니다.<br/>
	> main.go를 수정해볼까요?
	
	<pre><code>package main
	import (
		"./gugudan"
		"time"
	)
	
	func main() {
	    go gugudan.Gugudan(2)
	    go gugudan.Gugudan(3)
	
		time.Sleep(100 * time.Millisecond)
	}
	</code></pre>
	

5. 동시성 제어
 > 자 그럼 이제 2x1, 3x1, 2x2, 3x2 ...... 순서로 출력해 볼까요?
 
 > gugudan.go 수정
 
 <pre><code>package gugudan
	import (
	    "fmt"
	    "strconv"
	)
	
	func Gugudan(x int, c chan string) string {
		var minGugudan = 2
		maxGugudan := 9
	
		if x > maxGugudan || x < minGugudan {
			return "2~9만 지원함."
		} else {
			fmt.Println(toString(x) + "단 출력 시작")
		}
		
		for i := 0; i < 9; i++ {
			k := (toString(x) + " * " + toString(i + 1) +  " = " + toString(x * (i + 1)))
			
			c <- k
		}
	
		return (toString(x) + "단 출력 완료")
	}
	
	func toString(num int) string {
		
		return strconv.Itoa(num)
	}
 </code></pre>
 
 > main.go
 
 <pre><code>package main
	import (
		"./gugudan"
		"fmt"
	)
	
	func main() {
		//c := make(chan string)
		var c1 chan string = make(chan string)
		var c2 chan string = make(chan string)
	
	    go gugudan.Gugudan(2, c1)
	    go gugudan.Gugudan(3, c2)
	    
		for {
			fmt.Println(<- c1)
			fmt.Println(<- c2)
		}
	}
 </code></pre>
 
6. 채팅 프로그램 만들기
 > go get github.com/googollee/go-socket.io
 > 코드는 슬랙에...
 
