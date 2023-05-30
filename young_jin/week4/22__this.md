# this의 역할

객체 인스턴스를 가리킨다.

<!DOCTYPE html>
<html>
<head>
  <style>
    .code-container {
      display: flex;
      justify-content: center;
      padding: 20px;
    };
    pre {
      padding: 10px;
      border-radius: 8px;
    }

  </style>
</head>
<body>
  <div class="code-container">
    <pre>
    객체 타입
    자기자신 바로 지정가능

    const sky = {
        title: "father of skynet",
        printTitle: function () {
            console.log(sky.title);
        }
    };

코드 내부에서 바로 sky를 지정하고있다.

</pre>
<pre>
     생성자 함수
     인스턴스의 이름이 정해지지 않음

    function postSky(title) {
        ???.title = title;
        ???.printTitle = function () {
            console.log(???.title)
        }
    }

    const skyJR = new postSky("sky cattle citizen")

인스턴스를 만들어야 비로소 ???가 지정할 식별자가 skyJR로 정해짐.

</pre>
 </div>
</body>
</html>

---

# 다른 언어와의 비교

- java의 `this`, python의 `self` 와 같은기능

<!DOCTYPE html>
<html>
<head>
  <style>
    .code-container {
      display: flex;
      justify-content: space-between;
      padding: 20px;
    }

    pre {
      background-color: #f0f0f0;
      padding: 10px;
      border-radius: 8px;
    }

  </style>
</head>
<body>
  <div class="code-container">
    <pre>
    자바 코드

    public Class Person {
        private String name;

        public Person(String name) {
        this.name = name;
        }
    }

    //인스턴스 생성
    Person person = new Person("Choi");

</pre>
<pre>
    파이썬 코드

    class Person:
        def **init**(self, name):
        self.name = name

    //인스턴스 생성
    person = Person("KD")

</pre>
<pre>
    자바스크립트 코드

    function Person(name) {
        this.name = name;
    }
    //인스턴스 생성
    const person = new Person("Chloe");

</pre>

  </div>
</body>
</html>

---

# 자바스크립트 this 만의 매력

메서드 사용시 호출된 객체로 this가 동적으로 바인딩된다.

```

const A = {
    name: "A",
    print: function(){
        console.log(this.name)
    }
}

A.print() // 출력결과 "A"

const B = {
    this.name ="B"
}

//A의 메서드를 복사
B.printName = A.printName

B.printName() // 출력결과 "B"
```

---

# 자바스크립트에서 함수를 호출하는 방법

### 함수의 종류

| 종류 | 일반함수              | 메서드                  |
| ---- | --------------------- | ----------------------- |
| 형태 | function funcName(){} | 객체내부의 function(){} |

### 함수 호출방법

```

const choi = function () {
  console.dir(this);
};

// 1. 함수 호출 - this는 전역객체

choi(); // window


// 2. 메서드 호출 -this는 호출객체


const chloe = { callChFamily: choi };
chloe.callChFamily(); // chloe Object

// 3. 생성자 함수 호출 - this는 생성된 instance

const kd = new choi(); // kd object - choi의 instance

// 4. apply/call/bind 호출 - this를  apply/call/bind의 인자에 묶어준다.

const sky = { name: 'sky' };
choi.call(sky);   // sky object
choi.apply(sky);  // sky object
choi.bind(sky)(); // sky object
```

### 아래의 콜백에 this가 사용된다면 메서드 형태를 사용해야합니다.

addEventListener, schema.pre or schema.static
