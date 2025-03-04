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

[스레드 패닉이란?]

스레드 패닉은 실행 중인 스레드가 예기치 않게 비정상적인 상태에 직면했을 때 발생합니다. 이는 주로 다음과 같은 상황에서 발생할 수 있습니다:

런타임 에러: 예를 들어, 벡터의 유효하지 않은 인덱스에 접근하려고 할 때 panic! 매크로가 호출될 수 있습니다.

패닉 익셉션: 스레드에서 발생한 패닉을 처리할 수 있는 익셉션 핸들러를 명시적으로 정의하지 않았을 경우 발생합니다.

~~~



#### 연습문제 1 (함수) : 피보나치 재귀함수를 Rust로 구현

~~~rust

fn fib(n: u32) -> u32 {
    if n <= 2 {
        1;
    } else {
        fib(n - 1) + fib(n - 2)  // 이전 두 항의 합을 반환
    }
}

fn main() {
    let n = 20;
    println!("fib({}) = {}", n, fib(n));
}

~~~


---


#### 연습문제 2 (배열) : 중첩 배열(2차원)을 전치행렬 적용하기

~~~rust

fn transpose(matrix: [[i32; 3]; 3]) -> [[i32; 3]; 3] {
    let mut result = [[0; 3]; 3];
    for i in 0..3 {
        for j in 0..3 {
            result[j][i] = matrix[i][j]; 
        }
    }
    result

// 러스트에서 ;의 여부

// 없으면 그냥 표현식임. 작업은 수행하지만 값을 반환하진 않음(와우..)

// ; 있으면 rust컴파일러는 이를 명령문으로 처리

// 추후 소유권 문제에 대해 관리하는데 이 개념이 사용된다.

}


        //  result[y][x] = matrix[y][x]
    
}

fn test_transpose() {
    let matrix = [
        [101, 102, 103], //
        [201, 202, 203],
        [301, 302, 303],
    ];
    let transposed = transpose(matrix);
    assert_eq!(
        transposed,
        [
            [101, 201, 301], //
            [102, 202, 302],
            [103, 203, 303],
        ]
    );
}

fn main() {
    let matrix = [
        [101, 102, 103], // <-- 주석으로 rustfmt가 줄바꿈을 추가합니다.
        [201, 202, 203],
        [301, 302, 303],
    ];

    println!("행렬: {:#?}", matrix);
    let transposed = transpose(matrix);
    println!("전치행렬: {:#?}", transposed);
}


~~~



0705..공유참조부터 추가 학습~
