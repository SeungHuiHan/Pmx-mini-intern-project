<template>
  <div class="container-fluid p-0">
    <div class="jumbotron bg-dark text-center">
      <div class="button-container">
        <button @click="zoomIn" class="btn btn-primary">
          <i class="fa fa-search-plus"></i> Zoom In
        </button>
        <button @click="zoomOut" class="btn btn-danger">
          <i class="fa fa-search-minus"></i> Zoom Out
        </button>
        <button @click="addEllipse" class="btn btn-warning">SVG 추가</button>
      </div>
      <div class="image-container">
        <svg
          ref="svgContainer"
          width="100%"
          height="100%"
          xmlns="http://www.w3.org/2000/svg"
          @mousedown="startDrag"
          @mousemove="dragEllipse"
          @mouseup="endDrag"
          @mouseleave="endDrag"
        >
          <g>
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
          </g>
        </svg>
        <img
          :src="imageUrl"
          class="img-fluid"
          :style="{
            transform: `scale(${scale}) translate(${translateX}px, ${translateY}px)`,
          }"
          @wheel="handleZoom"
          @mousedown="startPan"
          @mousemove="panImage"
          @mouseup="endPan"
          @mouseleave="endPan"
          ref="image"
        />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      imageUrl:
        "https://health.chosun.com/site/data/img_dir/2022/05/04/2022050401754_0.jpg",
      scale: 1,
      translateX: 0,
      translateY: 0,
      panning: false,
      startX: 0,
      startY: 0,
      ellipses: [],
      dragging: false,
      currentEllipse: null,
      resizing: false,
      resizeIndex: -1,
      resizeX: 0,
      resizeY: 0,
      pointers: [],
    };
  },
  methods: {
    // 이미지 확대
    zoomIn() {
      this.scale += 0.1;
    },
    // 이미지 축소
    zoomOut() {
      this.scale -= 0.1;
    },
    // 마우스 휠로 이미지 줌 제어
    handleZoom(event) {
      const delta = event.deltaY;
      if (delta > 0) {
        this.zoomOut();
      } else {
        this.zoomIn();
      }
    },
    // 이미지 패닝 시작
    startPan(event) {
      this.panning = true;
      this.startX = event.clientX - this.translateX;
      this.startY = event.clientY - this.translateY;
    },
    // 이미지 패닝
    panImage(event) {
      if (this.panning) {
        this.translateX = event.clientX - this.startX;
        this.translateY = event.clientY - this.startY;
      }
    },
    // 이미지 패닝 종료
    endPan() {
      this.panning = false;
    },
    // SVG 타원 추가
    addEllipse() {
      const newEllipse = {
        cx: 100, // Initial X position
        cy: 100, // Initial Y position
        rx: 30, // X radius
        ry: 20, // Y radius
        stroke: "yellow", // Border color
        dashArray: "5,5", // 점선 스타일 (예: 5px 실선, 5px 공백)
      };
      this.ellipses.push(newEllipse);
    },

    // 타원 드래그 시작

    startDrag(event, ellipse) {
      this.dragging = true;
      this.currentEllipse = ellipse;
      const svg = this.$refs.svgContainer;
      const point = svg.createSVGPoint();
      point.x = event.clientX;
      point.y = event.clientY;
      const cursor = point.matrixTransform(svg.getScreenCTM().inverse());
      this.currentEllipse.cx = cursor.x;
      this.currentEllipse.cy = cursor.y;
    },
    // 타원 드래그
    dragEllipse(event) {
      if (this.dragging && this.currentEllipse) {
        const svg = this.$refs.svgContainer; // Ref로 SVG 요소 가져오기
        const point = svg.createSVGPoint();
        point.x = event.clientX;
        point.y = event.clientY;
        const cursor = point.matrixTransform(svg.getScreenCTM().inverse());
        this.currentEllipse.cx = cursor.x;
        this.currentEllipse.cy = cursor.y;
        this.enforceBounds(this.currentEllipse);
      }
    },
    // 타원 크기 조정 시작
    startResize(index) {
      this.resizing = true;
      this.resizeIndex = index;
      this.resizeX = this.ellipses[index].rx;
      this.resizeY = this.ellipses[index].ry;
    },
    // 타원 크기 조정
    resizeEllipse(index) {
      if (this.resizing) {
        this.ellipses[index].rx = this.resizeX + (event.clientX - this.resizeX);
        this.ellipses[index].ry = this.resizeY + (event.clientY - this.resizeY);
        this.enforceBounds(this.ellipses[index]);
      }
    },
    // 타원 크기 조정 종료
    endResize() {
      this.resizing = false;
      this.resizeIndex = -1;
    },

    // 타원 드래그 종료
    endDrag() {
      this.dragging = false;
      this.currentEllipse = null;
    },
  },
};
</script>

<style>
.image-container {
  background-color: black;
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
  height: 100vh;
}

.button-container {
  display: flex;
  justify-content: space-between;
  margin-top: 10px;
  padding: 10px;
}

.btn {
  margin-right: 10px;
}

.btn-warning {
  background-color: yellow;
}

img {
  max-height: 100%;
}

ellipse {
  cursor: move;
}
circle {
  cursor: pointer;
}
</style>
