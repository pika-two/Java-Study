# Ch1. CSS

## 1. Basic Reset using the universal selector
- `box-sizing: border-box;` 요소의 크기를 테두리와 패딩을 전부 포함해서 적용시킨다.
- universel selector를 통해 모든요소의 마진과 패딩을 없애준다.
- padding은 상속되지 않으므로, body에만 padding을 넣어준다
- 폰트와 색깔은 상속되므로, body에 선언해준다.
```css
* {

margin: 0;

padding: 0;

box-sizing: border-box;

}

  

body {

font-family: "Lato", sans-serif;

font-weight: 400;

font-size: 16px;

list-style: 1.7;

color : #777;

padding: 30px;

}
```
## 2. How to set project-wide font definitions
- body에 전체적으로 사용할 폰트와, 폰트사이즈, 색상등을 설정해준다.

## 3. How to clip parts of elements using clip-path
- clip path를 통해 다각형으로 요소를 자를수 있다. 설정한 svg를 통해서도 설정가능하다.
```css
#clip-path {
    clip-path: circle(40% at 50% 50%);
    background : url("http://resrc.devdic.com/img/bysize/below_200/01.png") center/cover;
    width : 350px;
    height : 350px;
}
```
- https://bennettfeely.com/clippy/


## 4. etc
- `background-size : cover`  : 가로세로 비율을 맞추면서, 요소를 전부 덮을 수 있도록 확장시킨다.
- `background-position : top` : 백그라운드의 위치를 조정해주는 것으로, top은 키워드 밸류이다. 그 위치를 기준으로 짤리는 이미지를 결정한다.