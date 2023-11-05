<script>
import UploadService from "../services/UploadFilesService";
import http from "../http-common";
import { formToJSON } from "axios";

export default{
  el: '#app',
  
  data() {
    return {
      isCameraOpen: false,
      isPhotoTaken: false,
      isShotPhoto: false,
      isLoading: false,
      link: '#',

      currentImage: undefined,
      previewImage: undefined,
      progress: 0,
      message: "",
      imageInfos: [],

      dataReturned: false,
      dataJSON: [],
    }
  },
  
  methods: {
    toggleCamera() {
      if(this.isCameraOpen) {
        this.isCameraOpen = false;
        this.isPhotoTaken = false;
        this.isShotPhoto = false;
        this.dataReturned = false;
        this.dataJSON = []
        this.stopCameraStream();
      } else {
        this.isCameraOpen = true;
        this.createCameraElement();
      }
    },
    
    createCameraElement() {
      this.isLoading = true;
      
      const constraints = (window.constraints = {
				audio: false,
				video: true
			});


			navigator.mediaDevices
				.getUserMedia(constraints)
				.then(stream => {
          this.isLoading = false;
					this.$refs.camera.srcObject = stream;
				})
				.catch(error => {
          this.isLoading = false;
					alert("May the browser didn't support or there is some errors.");
				});
    },
    
    stopCameraStream() {
      let tracks = this.$refs.camera.srcObject.getTracks();

			tracks.forEach(track => {
				track.stop();
			});
    },
    
    takePhoto() {
      this.dataJSON = [];
      if(!this.isPhotoTaken) {
        this.isShotPhoto = true;

        const FLASH_TIMEOUT = 50;

        setTimeout(() => {
          this.isShotPhoto = false;
        }, FLASH_TIMEOUT);
      }
      
      this.isPhotoTaken = !this.isPhotoTaken;
      
      const context = this.$refs.canvas.getContext('2d');
      context.drawImage(this.$refs.camera, 0, 0, 450, 337.5);
    },
    
    downloadImage() {
      const download = document.getElementById("downloadPhoto");
      const canvas = document.getElementById("photoTaken").toDataURL("image/jpeg")
    .replace("image/jpeg", "image/octet-stream");
      download.setAttribute("href", canvas);
    },

    uploadImage() {
      this.dataJSON = []
      console.log("Uploading image")
      const upload = document.getElementById("uploadPhoto");
      // const canvas = document.getElementById("photoTaken").toDataURL("image/jpeg").replace("image/jpeg", "image/imgBase64");
      const canvas = document.getElementById("photoTaken")
      canvas.toBlob(function(blob){
        const formData = new FormData();
        formData.append('file', blob, 'my-photo.png');

        // Post via axios or other transport method
        http.post('http://127.0.0.1:5000/search', formData,{
          headers: {
            "Content-Type": "multipart/form-data"
        }
        }).then((response) => {
          console.log(response.data)
          const results = response.data
          console.log(results)
          const tables_container = document.getElementById('results-container')
          console.log(tables_container)
          function insertRow(information, title, tb) {
            const row = document.createElement("tr");
            const info_cell = document.createElement("td")
            info_cell.setAttribute('class', "text-sm text-gray-900 font-light px-6 py-4 whitespace-nowrap border-r")
            const info_title = document.createElement("td")
            info_title.setAttribute('class', "text-sm text-gray-900 font-light px-6 py-4 whitespace-nowrap border-r")
            info_cell.innerHTML = information
            info_title.innerHTML = title
            row.append(info_title)
            row.append(info_cell)
            tb.append(row)
          }
          results.forEach(food => {
            const food_info = food.items[0]
            const table = document.createElement("table");
            table.setAttribute('class', "min-w-full border text-center")
            const row = document.createElement("tr");
            row.setAttribute('class', "border-b")
            const food_name = document.createElement("th");
            food_name.setAttribute('class', 'text-sm font-medium text-gray-900 px-6 py-4 border-r')
            food_name.innerHTML = food_info.name.toUpperCase()
            const nutritional_facts = document.createElement("th");
            nutritional_facts.innerHTML="Nutritional Facts"
            nutritional_facts.setAttribute('class', 'text-sm font-medium text-gray-900 px-6 py-4 border-r')
            row.append(food_name)
            row.append(nutritional_facts)
            table.append(row)
            insertRow(food_info.calories, "Calories", table)
            insertRow(food_info.serving_size_g, "Serving Size (g)", table)
            insertRow(food_info.carbohydrates_total_g, "Carbohydrates (g)", table)
            insertRow(food_info.fat_total_g, "Fat (g)", table)
            insertRow(food_info.protein_g, "Protein (g)", table)
            insertRow(food_info.sugar_g, "Sugar (g)", table)
            tables_container.append(table)
          })

        
        })
      
      });
      this.dataReturned = true;
      // console.log(this.dataJSON)
    },

    uploadImageFromFiles() {
                this.dataJSON = [];
                this.dataReturned = true;
                const image = document.getElementById("dropzone-file")
                
                var file = image.files[0];

                const formData = new FormData();
                const blob = new Blob([file], { type: "image/png" });
                console.log(blob)
                formData.append('file', blob, 'my-photo.png');
                console.log(formData)
                http.post('http://127.0.0.1:5000/search', formData,{
                    headers: {
                         "Content-Type": "multipart/form-data"
                    }
                }).then((response) => {
          console.log(response.data)
          const results = response.data
          console.log(results)
          const tables_container = document.getElementById('results-container')
          console.log(tables_container)
          function insertRow(information, title, tb) {
            const row = document.createElement("tr");
            const info_cell = document.createElement("td")
            info_cell.setAttribute('class', "text-sm text-gray-900 font-light px-6 py-4 whitespace-nowrap border-r")
            const info_title = document.createElement("td")
            info_title.setAttribute('class', "text-sm text-gray-900 font-light px-6 py-4 whitespace-nowrap border-r")
            info_cell.innerHTML = information
            info_title.innerHTML = title
            row.append(info_title)
            row.append(info_cell)
            tb.append(row)
          }
          results.forEach(food => {
            const food_info = food.items[0]
            const table = document.createElement("table");
            table.setAttribute('class', "min-w-full border text-center")
            const row = document.createElement("tr");
            row.setAttribute('class', "border-b")
            const food_name = document.createElement("th");
            food_name.setAttribute('class', 'text-sm font-medium text-gray-900 px-6 py-4 border-r')
            food_name.innerHTML = food_info.name.toUpperCase()
            const nutritional_facts = document.createElement("th");
            nutritional_facts.innerHTML="Nutritional Facts"
            nutritional_facts.setAttribute('class', 'text-sm font-medium text-gray-900 px-6 py-4 border-r')
            row.append(food_name)
            row.append(nutritional_facts)
            table.append(row)
            insertRow(food_info.calories, "Calories", table)
            insertRow(food_info.serving_size_g, "Serving Size (g)", table)
            insertRow(food_info.carbohydrates_total_g, "Carbohydrates (g)", table)
            insertRow(food_info.fat_total_g, "Fat (g)", table)
            insertRow(food_info.protein_g, "Protein (g)", table)
            insertRow(food_info.sugar_g, "Sugar (g)", table)
            tables_container.append(table)
          })
      
      });
            
            }
  }
};
</script>

<template>
    <div>
      <b1>    
          <form className="mt-8 space-y-6" action="" method="">
                <div class="flex items-center justify-center w-full py-3">
                    <label for="dropzone-file" class="flex flex-col items-center justify-center w-full h-64 border-2 border-gray-300 border-dashed rounded-lg cursor-pointer hover:bg-gray-200 hover:border-gray-500 ">
                        <div class="flex flex-col items-center justify-center pt-5 pb-6">
                            <svg aria-hidden="true" class="w-10 h-10 mb-3 text-gray-400" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg%22%3E"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M7 16a4 4 0 01-.88-7.903A5 5 0 1115.9 6L16 6a5 5 0 011 9.9M15 13l-3-3m0 0l-3 3m3-3v12"></path></svg>
                            <p class="mb-2 text-sm text-gray-500 dark:text-gray-400"><span class="font-semibold">Click to upload</span> or drag and drop</p>
                            <p class="text-xs text-gray-500 dark:text-gray-400">SVG, PNG, JPG or GIF (MAX. 800x400px)</p>
                        </div>

                        <input id="dropzone-file" type="file" />
                        
                    </label>
                </div> 
                <div class="flex items-center justify-center w-full py-3">

                <a class="button" id="uploadFromForm" role="button" @click="uploadImageFromFiles">
                            Upload
                        </a>
                </div>
            </form>
        </b1>
    </div>
<div id="app" class="web-camera-container">
  <div class="camera-button">
      <button type="button" class="button is-rounded" :class="{ 'is-primary' : !isCameraOpen, 'is-danger' : isCameraOpen}" @click="toggleCamera">
        <span v-if="!isCameraOpen">Open Camera</span>
        <span v-else>Close Camera</span>
    </button>
  </div>
  
  <div v-show="isCameraOpen && isLoading" class="camera-loading">
    <ul class="loader-circle">
      <li></li>
      <li></li>
      <li></li>
    </ul>
  </div>
  
  <div v-if="isCameraOpen" v-show="!isLoading" class="camera-box" :class="{ 'flash' : isShotPhoto }">
    
    <div class="camera-shutter" :class="{'flash' : isShotPhoto}"></div>
      
    <video v-show="!isPhotoTaken" ref="camera" :width="450" :height="337.5" autoplay></video>
    
    <canvas v-show="isPhotoTaken" id="photoTaken" ref="canvas" :width="450" :height="337.5"></canvas>
  </div>
  
  <div v-if="isCameraOpen && !isLoading" class="camera-shoot">
    <button type="button" class="button" @click="takePhoto">
      <img src="https://img.icons8.com/material-outlined/50/000000/camera--v2.png">
    </button>
  </div>
  
  <div>
    <div v-if="isPhotoTaken && isCameraOpen" class="camera-download">
    <a id="downloadPhoto" download="my-photo.jpg" class="button" role="button" @click="downloadImage">
      Download
    </a>
  </div>
  <br>
  <div v-if="isPhotoTaken && isCameraOpen" class="camera-upload">
    <a id="uploadPhoto" download="my-photo.jpg" class="button" role="button" @click="uploadImage">
      Upload
    </a>
  </div>

  </div>
 
</div>

<div v-if="dataReturned" class="flex items-center justify-center w-full py-3">
  <label for="dropzone-file" class="flex flex-col items-center justify-center w-full h-64 border-2 bg-white border-gray-300 border-dashed rounded-lg ">
    <div class="relative overflow-x-auto" id="results-container">
        
    </div>
  </label>
</div>


</template>

<style scoped>

  #results-container{
    width: 90%;
  }

  #photoTaken {
    border-radius: 3px;
    margin: 0.5rem;
  }
  .camera-box {
    border-radius: 3px;
    margin: 0.5rem;
    align-self: center;
    width: 450px;
    height: 337.5px;
  }
  .camera-shoot {
    margin: 1rem 0;
  }
  .camera-shoot img {
    height: 35px;
    object-fit: cover;
  }
  .camera-shoot button {
      height: 60px;
      width: 60px;
      display: flex;
      align-items: center;
      justify-content: center;
      background-color: #155263;
      border-radius: 6px;
  }

  .camera-shoot button:hover{
    background-color: #ff6f3c;
  }
  
  .web-camera-container {
    margin-top: 2rem;
    margin-bottom: 2rem;
    padding: 5rem;
    display: flex;
    flex-direction: column;
    justify-content: center;
    text-align: center;
    align-items: center;
    border: 1px solid #ccc;
    border-radius: 4px;
    width: full;
    height: fit-content;
    background-color: white;
    min-height: 300px;
  }

  .camera {
    border-radius: 10px;
    width: fit-content;
    padding: 2px;
  }

  .camera-button {
    color: white;
    background-color: #008B8B;
    border-radius: 5px;
    padding-left: 8.5%;
    padding-right: 10%;
    width: 100px;
  }

  .camera-shoot img{
    filter: brightness(0) invert(1); 
  }

  .camera-button:hover{
    background-color: #590059;
  }





  #uploadPhoto,
  #uploadFromForm,
  #downloadPhoto{
    color: white;
    background-color: #008B8B;
    border-radius: 6px;
    padding-left: 5%;
    padding-right: 5%;
    padding-top: 2%;
    padding-bottom: 2%;
    width: 100px;
  }
  

  #uploadFromForm{
    text-align: center;
    padding-left: 10px;
    padding-right: 10px;
  }

#uploadPhoto:hover,
#uploadFromForm:hover,
#downloadPhoto:hover {
  background-color: #590059;
}


</style>