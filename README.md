# vue-project
VUE3 콤포넌트 개발 프로젝트

## 프로젝트 소개
피맥스에서 세계 최초로 의료용 분석용 인공지능앱을 병원에서 획기적인 개선을 할 수 있도록 서비스를 제공하고 있다. 이 과정에서 라벨링한 진단 부위 영역에 VUE3 타원형 컴포넌트로 영역을 표시하는 프로젝트를 진행하고자 한다.

### 구현해야 할것
- 특정 사이즈의 이미지를 로딩
- 이미지위에 SVG ELLIPSE를 구현
- 배경 이미지 줌인/줌아웃/패닝 기능
- ELLIPSE 크기 변경 회전 기능

### 구현 스택
- 언어: javascript
- 프레임워크: vue3
- 사용 api: web drag and drop

### 진행상황
![vue3 시현영상](https://github.com/SeungHuiHan/Pmx-mini-intern-project/assets/98226400/107b8d92-ec7a-4606-a216-00511154240d)


## 이미지 로딩
![image](https://github.com/SeungHuiHan/Pmx-mini-intern-project/assets/98226400/73f8fe4c-818e-47ac-b562-f39e8a3b6774)
```
<img
          :src="imageUrl"
          class="img-fluid"
          :style="{
            transform: `scale(${scale}) translate(${translateX}px, ${translateY}px)`,
          }"
          생략
        />
```
- imageUrl:  특정 이미지의 URL 을 저장하는 역할


## SVG ELLIPSE
![image](https://github.com/SeungHuiHan/Pmx-mini-intern-project/assets/98226400/7262383f-9d7e-495e-8537-b192e198ff01)

```
<ellipse
              v-for="(ellipse, index) in ellipses"
              :cx="ellipse.cx"
              :cy="ellipse.cy"
              :rx="ellipse.rx"
              :ry="ellipse.ry"
              :stroke="ellipse.stroke"
              :stroke-dasharray="ellipse.dashArray"
              v-bind:key="ellipse"
            />
```
- ellpise 구현


## 이미지 줌인/줌아웃/패닝
![image](https://github.com/SeungHuiHan/Pmx-mini-intern-project/assets/98226400/e5a1c824-7690-4ef0-9bfb-087b85676c18)
![image](https://github.com/SeungHuiHan/Pmx-mini-intern-project/assets/98226400/91b37465-29b2-4f0c-a7ba-58e4cc73a275)
```
<img
          생략
          @wheel="handleZoom"
          @mousedown="startPan"
          @mousemove="panImage"
          @mouseup="endPan"
          @mouseleave="endPan"
          ref="image"
        />
```
- 마우스로 이미지 확대,축소,패닝 가능
- 화면 상단 zoomin,zoomout버튼으로 이미지 확대,축소 가능


## ELLIPSE 크기변경 회전기능
- 구현중..

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Lints and fixes files
```
npm run lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).
