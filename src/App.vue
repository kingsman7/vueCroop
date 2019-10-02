<template>
  <div id="app">
	<div class="container">
		<!-- Top Navigation -->
		<div class="content">
			<div class="component">
				<div class="overlay">
					<div class="overlay-inner">
					</div>
				</div>
				<img class="resize-image" src="./assets/img/image.jpg" alt="image for resizing">
				<a class="btn-crop js-crop" href="img/image.jpg">Crop<img class="icon-crop"
						src="./assets/img/crop.svg"></a>
			</div>
		</div><!-- /content -->
	</div> <!-- /container -->
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      event_state : {},
      constrain : false,
      min_width : null, // Change as required
      min_height : null,
      max_width : null, // Change as required
      max_height : null,
      orig_src: null,
      resize_canvas:null,
    }
  },
  mounted(){
    const imagen = document.querySelector('img')
    this.resizeableImage(imagen)
    this.saveEvenetState()
  },

  methods:{

    imageTemplate(){
      return(
        `<div class="resize-container">
          <span class="resize-handle resize-handle-nw" @mousedown="startResize" ></span>
          <span class="resize-handle resize-handle-n" @mousedown="startResize" ></span>
          <span class="resize-handle resize-handle-ne" @mousedown="startResize" ></span>
          <span class="resize-handle resize-handle-s" @mousedown="startResize" ></span>
          <span class="resize-handle resize-handle-se" @mousedown="startResize" ></span>
          <span class="resize-handle resize-handle-e" @mousedown="startResize" ></span>
          <span class="resize-handle resize-handle-sw" @mousedown="startResize" ></span>
          <span class="resize-handle resize-handle-w" @mousedown="startResize" ></span>
        </div>`
      )
    },
    
    resizeableImage (image_target) {
      let container
      const overlay = document.querySelector('a')

      this.orig_src = new Image(),
      this.constrain = false,
      this.min_width = 60, // Change as required
      this.min_height = 60,
      this.max_width = 800, // Change as required
      this.max_height = 900,
      this.resize_canvas = document.createElement('canvas');
      

      const HTMLString = this.imageTemplate()
      const html = document.implementation.createHTMLDocument()
      html.body.innerHTML = HTMLString
      let parent = html.body.children[0]
      parent.children[2].before(image_target)
      console.log(parent)
      overlay.before(parent)
    },
    
    startResize(e){
      e.preventDefault();
      e.stopPropagation()
      document.onmousemove(resizing()) 
      document.onmouseup(endResize())
    },

    endtResize(e){
      e.preventDefault();
      //document.onmousemove(resizing()) 
      //document.onmouseup(endResize())
    },

    saveEvenetState(e){
      console.log(parent)
    }
  }
}
</script>

<style lang="scss">

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.component {
	position: relative;
	background: url(./assets/img/gridme.png) repeat center center;
	padding: 4em;
	height: 500px;
	border: 3px solid #49708A;
	max-width: 901px;
	overflow: hidden;
	margin: 0 auto;
}

.resize-container {
    position: relative;
    display: inline-block;
    cursor: move;
    margin: 0 auto;
}

.resize-container img {
    display: block
}

.resize-container:hover img,
.resize-container:active img {
    outline: 2px dashed rgba(222,60,80,.9);
}

.resize-handle-n,
.resize-handle-s,
.resize-handle-e,
.resize-handle-w,
.resize-handle-ne,
.resize-handle-se,
.resize-handle-nw,
.resize-handle-sw {
    position: absolute;
    display: block;
    width: 10px;
    height: 10px;
    background: rgba(222,60,80,.9);
    z-index: 999;
}

.resize-handle-nw {
    top: -5px;
    left: -5px;
    cursor: nw-resize;
}

.resize-handle-sw {
    bottom: -5px;
    left: -5px;
    cursor: sw-resize;
}

.resize-handle-ne {
    top: -5px;
    right: -5px;
    cursor: ne-resize;
}

.resize-handle-se {
    bottom: -5px;
    right: -5px;
    cursor: se-resize;
}
.resize-handle-n {
    top: -5px;
    left: 300px;
    cursor: nw-resize;
}

.resize-handle-s {
    bottom: -5px;
    left: 300px;
    cursor: sw-resize;
}

.resize-handle-e {
    top: 190px;
    right: -5px;
    cursor: ne-resize;
}

.resize-handle-w {
    top: 190px;
    left: -5px;
    cursor: se-resize;
}

.overlay {
	position: absolute;
	left: 50%;
	top: 50%;
	margin-left: -100px;
	margin-top: -100px;
	z-index: 999;
	width: 200px;
	height: 200px;
    border: solid 2px rgba(222,60,80,.9);
	box-sizing: content-box;
	pointer-events: none;
}

.overlay:after,
.overlay:before {
	content: '';
	position: absolute;
	display: block;
	width: 204px;
	height: 40px;
    border-left: dashed 2px rgba(222,60,80,.9);
	border-right: dashed 2px rgba(222,60,80,.9);
}

.overlay:before {
	top: 0;
	margin-left: -2px;
	margin-top: -40px;
}

.overlay:after {
	bottom: 0;
	margin-left: -2px;
	margin-bottom: -40px;
}

.overlay-inner:after,
.overlay-inner:before {
	content: '';
	position: absolute;
	display: block;
	width: 40px;
	height: 204px;
    border-top: dashed 2px rgba(222,60,80,.9);
	border-bottom: dashed 2px rgba(222,60,80,.9);
}

.overlay-inner:before {
	left: 0;
	margin-left: -40px;
	margin-top: -2px;
}

.overlay-inner:after{
	right: 0;
	margin-right: -40px;
	margin-top: -2px;
}

.btn-crop {
	position: absolute;
	vertical-align: bottom;
	right: 5px;
	bottom: 5px;
	padding: 6px 10px;
	z-index: 999;
	background-color: rgb(222,60,80);
	border: none;
	border-radius: 5px;
	color: #FFF;
}

.btn-crop img {
	vertical-align: middle;
	margin-left: 8px;
}

</style>
