# 캔버스에서 마우스 이벤트 구현하기

## 개요
즉시모드로 렌더링 되는 HTML5 Canvas 엘리먼트에서 사용자 마우스에 반응하는 인터렉티브한 컨텐츠 만들기.

## 목적
아직 해결되지 않은 고민점과 실패담을 공유하고 더 나은 아이디어를 얻기 위함.
(https://github.com/lim-lim-lim/stagejs)

## 클릭 이벤트 구현방법
### 사각형 충돌체크
- 클릭된 지점의 좌표가 사각형 영역안에 포함되면 클릭! 
### 원형 충돌체크
- 클릭된 지점의 좌표에서 원의 중심점의 거리가 원의 반지름 보다 작다면 클릭!
### 더 정교한 충돌체크
#### 픽셀검출 1
- 이벤트 전용 캔버스를 생성하고
- 클릭 이벤트가 발생하는 순간
- 오브젝트들을 등록된 역순으로 그리며 픽셀을 체크함.
#### 픽셀검출 2
- 오브젝트별로 고유의 컬러키를 설정하고
- 이것을 이벤트 전용 캔버스에 랜더링
- 마우스가 클릭된 좌표의 픽셀의 색깔과 오브젝트들의 고유 컬러키를 대조

### 고민중인 것
#### 성능을 위한 타일링
- 베지어 곡선의 Bounds 찾기

![Canvas_bezier](https://user-images.githubusercontent.com/11947298/57091126-68343880-6d43-11e9-8af0-dc3506332275.png)


- Border가 있는 패스로 그려진 도형의 Bounds 찾기



- Border가 있는 서로 다른 선분이 닿는 LineJoin 처리

![Canvas_linejoin](https://user-images.githubusercontent.com/11947298/57090862-c3196000-6d42-11e9-9b83-571a4e8a4339.png)

- Border가 있는 선분의 끝처리 LineCap 처리

![Canvas_linecap](https://user-images.githubusercontent.com/11947298/57090985-1be8f880-6d43-11e9-8a14-2a802889734f.png)




