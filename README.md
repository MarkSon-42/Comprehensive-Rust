# Comprehensive-Rust
https://google.github.io/comprehensive-rust/

 - 러스트는 2015년에 버전 1.0 릴리즈 (아직 10년도 안되었다!!)

 - 다양한 장치에서 사용 가능하다

 - C++가 사용되는 대부분의 곳에 사용 가능하다

/[Rust의 장점/]

 - 높은 유연성
 - 높은 수준의 제어
 - MCU같은 매우 제한된 장치로 scale down 가능
 - 안정성과 안전에 중점을 둔 언어



---  

### 초기화되지 않는 변수가 없다,

~~~rust

fn main() {
    let x: i32; // 변수 `x`를 초기화하지 않고 선언

    // `x`를 초기화하기 전에 사용하려고 하면 컴파일 시간 오류가 발생합니다
    println!("x의 값은: {}", x); // 이 줄은 컴파일 시간 오류를 발생시킴.
}
~~~


### 메모리 이중 해제가 원천적으로 불가능

~~~rust
fn main() {
    let mut v = vec![1, 2, 3];

    let ptr = v.as_mut_ptr();

    v.clear();  // 벡터를 비움. 그러나 v는 여전히 유효.

    // 이후의 예제는 너무 난이도가 있기에 현 시점에서 생략.
}
~~~



### 스레드간의 데이터 레이스를 막아준다.

~~~rust

use std::thread;

fn main() {
    let mut data = vec![1, 2, 3];

    let thread1 = thread::spawn(move || {
        // 1번 스레드 데이터에 접근하여 수정
        for i in 0..3 {
            data[i] += 1;
        }
    });

    let thread2 = thread::spawn(move || {
        // 2번 스레드 데이터에 접근하여 수정
        for i in 0..3 {
            data[i] *= 2;
        }
    });

    // 두 스레드가 실행될 때까지 기다림
    thread1.join().expect("Thread 1 panicked");
    thread2.join().expect("Thread 2 panicked");

    // 데이터 출력
    println!("최종 데이터: {:?}", data);
}

// Thread Panick??

스레드 패닉이란?
스레드 패닉은 실행 중인 스레드가 예기치 않게 비정상적인 상태에 직면했을 때 발생합니다. 이는 주로 다음과 같은 상황에서 발생할 수 있습니다:

런타임 에러: 예를 들어, 벡터의 유효하지 않은 인덱스에 접근하려고 할 때 panic! 매크로가 호출될 수 있습니다.

패닉 익셉션: 스레드에서 발생한 패닉을 처리할 수 있는 익셉션 핸들러를 명시적으로 정의하지 않았을 경우 발생합니다.

~~~





### 동시성 심화 학습 환경설정 : 새 크레이트 설정하고 의존성 다운로드

~~~
cargo init concurrency
~~~

~~~
cd coucurrency
~~~

~~~
cargo add tokio --features full
~~~

~~~
cargo run
~~~
