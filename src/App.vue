<template>
  <div id="app">
	  <div class="container">
      <div class="columns">
        <div class="column">
          hola
        </div>
          <div class="column is-one-quarter">
            <canvas id="canvas" :width=c_width :height=c_height></canvas>
            <button @click="crop" class="btn-crop js-crop">Crop<img class="icon-crop" src="./assets/img/crop.svg"></button>
            <input @change="uploadImage" class="btn-primary  btn-upload js-crop" type="file" id="fileUpload2" accept="image/*" />
          </div>
        <div class="column">
          ke ases
        </div>
      </div>
		</div>
  </div>
</template>

<script>
import { fabric } from 'fabric'
import { setTimeout } from 'timers';
export default {
  name: 'app',
  data () {
    return {
      c_height:500,
      c_width:900,
      widthCrop : 800, 
      heightCrop : 250,
      canvas : null,
      clipPath:null,
      main_image: null,

    }
  },
  mounted(){
    
    this.canvas = new fabric.Canvas('canvas'); 
    this.renderCanvas()
  },

  methods:{

    renderCanvas () {
      this.clipPath = new fabric.Rect({
      width: this.widthCrop,
      height: this.heightCrop,
      fill:'transparent',
      opacity: 1,
      strokeWidth:1,
      stroke:'red',
      selectable: false,
      lockMovementX : true,
      lockMovementY : true
    }); 

      this.canvas.centerObject(this.clipPath)
      this.canvas.add(this.clipPath)
    },
    
    uploadImage(e) {
      if(this.main_image){
        this.canvas.remove(this.main_image)
        this.canvas.remove(this.clipPath)
        this.canvas.add(this.clipPath)
      }

        var reader = new FileReader();
        self = this
        reader.onload = function (f) {
          let imObj = new Image()
          imObj.src = f.target.result;
          imObj.onload = function() {
            const imgFabri = new fabric.Image(imObj)
            self.main_image = imgFabri
            self.main_image.set({
              transparentCorners: false,
              cornerColor: 'red',
              cornerStrokeColor: 'red',
              borderColor: 'red',
              cornerSize: 12,
              padding: 10,
              cornerStyle: 'circle',
              borderDashArray: [3, 3]
            });
            self.canvas.centerObject(self.main_image)
            self.main_image.set({globalCompositeOperation: 'destination-over'});
            self.canvas.add(self.main_image)
          }
        }
        reader.readAsDataURL(e.target.files[0])
      
    },

    crop(){

      if (this.canvas.getActiveObject()){
        let ctx = this.canvas.getContext('2d')
        let object = this.canvas.getActiveObject()  
        let img = document.createElement('img')
        img.src = object.toDataURL("image/png")


        const xstart = this.clipPath.aCoords.bl.x - object.aCoords.bl.x
        const ystart = this.clipPath.aCoords.tl.y - object.aCoords.tl.y
        const width = this.clipPath.aCoords.br.x - this.clipPath.aCoords.bl.x
        const height = this.clipPath.aCoords.bl.y- this.clipPath.aCoords.tl.y
        const x = this.clipPath.left
        const y = this.clipPath.top

        console.log('stop')
        let newImg = new Image();
        newImg.src = img.src;
        
        newImg.onload = function () {
          ctx.drawImage(img,xstart,ystart,width,height,x,y,width,height)
        }
        this.canvas.remove(this.main_image)
        this.canvas.remove(this.clipPath)
        
        //saveCanvasToImage()

      }else{
        alert('seleccione la imagen')
      }
    },
    saveCanvasToImage() {
      let canvasNode = document.querySelectorAll('canvas')
      canvasNode.width = img.width
      canvasNode.height = img.height
      const canvasImg = document.createElement('img')
      var dataURL = canvasNode[0].toDataURL();
      canvasImg.src = dataURL;

      document.body.appendChild(canvasImg)
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

#canvas{
	border: 1px solid black
}


</style>
