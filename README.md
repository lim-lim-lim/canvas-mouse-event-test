# 캔버스에서 마우스 이벤트 구현하기

## 개요
즉시모드로 렌더링 되는 HTML5 Canvas 엘리먼트에서 사용자 마우스에 반응하는 인터렉티브한 컨텐츠 만들기.

## 목적
아직 해결되지 않은 고민점과 실패담을 공유하고 더 나은 아이디어를 얻기 위함.
(https://github.com/lim-lim-lim/stagejs)

## 클릭 이벤트 구현방법
### 사각형 충돌체크
- 클릭된 지점의 좌표가 사각형 영역안에 포함되면 클릭! 

```
 hitTest(x, y) {
    const rect = this.rect;
    const minX = rect.x;
    const maxX = minX + rect.width;
    const minY = rect.y;
    const maxY = minY + rect.height;
    return x > minX !== x > maxX && y > minY !== y > maxY;
}
```
### 원형 충돌체크
- 클릭된 지점의 좌표에서 원의 중심점의 거리가 원의 반지름 보다 작다면 클릭!

<img width="751" alt="스크린샷 2019-05-03 오전 2 04 28" src="https://user-images.githubusercontent.com/11947298/57093147-14781e00-6d48-11e9-809e-e969c1ed339d.png">


![c5400d92def339a6f58d8c7b6614c887_1553135793_4698](https://user-images.githubusercontent.com/11947298/57091747-d7f6f300-6d44-11e9-938a-77c3a0fc084c.png)
```
hitTest(x, y) {
    const circle = this.circle;
    const centerX = this.circle.x;
    const centerY = this.circle.y;
    const distance = Math.sqrt(Math.pow(x - centerX, 2) + Math.pow(y - centerY, 2));
    return distance <= circle.radius;
}
```
### 더 정교한 충돌체크
#### 픽셀검출 1
- 이벤트 전용 캔버스를 생성하고
- 클릭 이벤트가 발생하는 순간
- 오브젝트들을 등록된 역순으로 그리며 픽셀을 체크함.

<img width="895" alt="스크린샷 2019-05-03 오전 2 27 52" src="https://user-images.githubusercontent.com/11947298/57094269-1e4f5080-6d4b-11e9-93f1-bc6ca86fe099.png">

```
hitTest(x, y) {
    const imageData = this.context.getImageData(x, y, 1, 1).data;
    return imageData[3];
}
```
#### 픽셀검출 2
- 오브젝트별로 고유의 컬러키를 설정하고
- 이것을 이벤트 전용 캔버스에 랜더링
- 마우스가 클릭된 좌표의 픽셀의 색깔과 오브젝트들의 고유 컬러키를 대조

<img width="987" alt="스크린샷 2019-05-03 오전 2 35 36" src="https://user-images.githubusercontent.com/11947298/57094728-2b207400-6d4c-11e9-9bbc-7fbf1976f6db.png">

```
this.imageMap = new Map();
this.imageMap.set('ff0000ff', { name: '티라노사우르스', x: 80, y: 25, width: 300, height: 300, path: 'assets/trex.png', ref: null });
this.imageMap.set('00ff00ff', { name: '알로사우르스', x: 150, y: 125, width: 250, height: 250, path: 'assets/alo.png', ref: null });
this.imageMap.set('0000ffff', { name: '트리케라톱스', x: 0, y: 200, width: 200, height: 200, path: 'assets/triceratops.png', ref: null });
```
```
getColor(x, y) {
    const imageData = this.eventContext.getImageData(x, y, 1, 1).data;
    const color = imageData.reduce((data, pixel)=>{
        const hex = pixel.toString(16);
        data += hex.length < 2 ? `0${hex}` : hex;
        return data;
    }, '');
    return color;
}
```

### 고민중인 것
#### 성능을 위한 타일링
- 베지어 곡선의 Bounds 찾기
- Border가 있는 패스로 그려진 도형의 Bounds 찾기
- Border가 있는 서로 다른 선분이 닿는 LineJoin 처리
- Border가 있는 선분의 끝처리 LineCap 처리

![Canvas_bezier](https://user-images.githubusercontent.com/11947298/57091126-68343880-6d43-11e9-8af0-dc3506332275.png)
![Canvas_linejoin](https://user-images.githubusercontent.com/11947298/57090862-c3196000-6d42-11e9-9b83-571a4e8a4339.png)
![Canvas_linecap](https://user-images.githubusercontent.com/11947298/57090985-1be8f880-6d43-11e9-8a14-2a802889734f.png)




