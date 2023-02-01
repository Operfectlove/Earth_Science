# Solar System Simulator

> the solarsysten simulator created by Vue.js, ThreeJS, TypeScript

![img](./thumb.png)

[Live Demo](https://evan-moon.github.io/solarsystemts/#/)

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

# build for production and view the bundle analyzer report
npm run build --report

# run unit tests
npm run unit

# run all tests
npm test
```

## 기술설명

# 데이터 정의 
지구의 궤도 데이터는 다음과 같다.
<img width="448" alt="스크린샷 2023-02-01 오전 11 27 05" src="https://user-images.githubusercontent.com/106957635/215955451-905ae1f7-f896-4568-b75a-21ae306327cf.png">
base 프로퍼티에 들어있는 값은 궤도의 기본 요소들을 의미하며 이 값들은 천문학에서의 역기점인 J2000때 측정된 값을 의미한다. 
J2000은 2000년 1월 1일 정오를 의미한다. 
그리고 cy프로퍼티에 있는 값들은 1세기당 궤도 요소들의 변화량을 의미한다.

a는 장반경, e는 이심률, i는 기울기, o는 승교점 적경, l은 평균 경도, lp는 근일점 경도를 의미한다. 
이 중 장반경의 경우 AU값으로 선언되어있기 때문에 1AU를 km로 환산한 값인 149597870를 곱해줘야 한다. 
이때 1AU는 태양과 지구 사이의 거리를 의미한다.

하지만 이 데이터에는 케플러 6요소 중 근일점 편각과 근일점 통과시각이 없다. 
그렇기 때문에 필자는 근일점 경도와 승교점 적경을 사용하여 근일점 편각을 구하려고 한다. 
그리고 근일점 통과 시각의 경우 평균근점이각을 구할 때 필요한데, 근일점 통과 시각을 사용하여 구하는 것보다 근일점 경도와 평균 경도를 사용하는 방법이 훨씬 공식이 간단하기 때문에 오히려 편해진 상황이다.

# 시간 설정과 궤도 요소 계산
천문학에서는 우리가 평소 사용하는 그레고리력이 아닌 율리우스력을 사용한다.

율리우스력이란 원래 BC 4713년 1월 1일 월요일 정오를 기점으로 계산한 날짜 수 이다. 
하지만 기원전 4713년부터 서기 2017년까지의 날짜를 세면 자릿수가 너무 커지기 때문에 1976년 IAU(국제천문연맹)에서 결정한 J2000을 사용하는 것이다.

