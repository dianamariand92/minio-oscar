<template>    
	<v-container 
	class="fill-height"
	fluid
	>   
	<div  v-show="loading" class="loading-overlay is-active">
		<span class="fa fa-spinner fa-5x fa-spin"></span>
	</div>
		<v-row align="center" justify="center">
			<v-col >          
				<v-parallax src="@/assets/fondoazul.jpg" alt="">
				<!-- <v-spacer></v-spacer> -->
				<v-layout
					column
					align-center
					justify-center
					class="white--text"
				>       
					
				<img src="@/assets/fondo1.png" height="100%">            
				</v-layout>
				</v-parallax>
			</v-col>
		</v-row>
		<v-row>
			<v-card class="col-12 col-md-6">
				<h1 class="text-center">Input Bucket</h1>
				<v-select
					:items="buckets"
					label="Input Bucket"
					single-line
					@change="inputFilesBucket"
				></v-select>
			</v-card>

			<v-card class="col-12 col-md-6">
				<h1 class="text-center">Output Bucket</h1>
				 <v-select
					:items="buckets"
					label="Output Bucket"
					single-line
					@change="outputFilesBucket"
				></v-select>
			</v-card>

		</v-row>
		</v-container>
</template>

<script>
  
 import axios from 'axios';
 import moment from 'moment';
 import vue2Dropzone from 'vue2-dropzone';
 import 'vue2-dropzone/dist/vue2Dropzone.min.css';
 import Services from './services';
 
  export default {
    components: {
    vueDropzone: vue2Dropzone
    },
	mixins:[Services],
    
    data: () => ({
		buckets:[],
		dropzoneOptions: {
			url: 'https://httpbin.org/post',
			thumbnailWidth: 150,
			// maxFilesize: 0.5,          
			addRemoveLinks: true, 
			destroyDropzone: false,
		},
		loading: false,
	 
                       
    }),  
    created(){      
		this.loading = true;
		this.getBucketListCall(this.getBucketListCallBack)
		//   this.listBuckets();
		//   this.getfiles();

		this.$vuetify.theme.light = true 
		var current= new Date(document.lastModified);          
		this.lastupdate = moment(current).format("MMMM Do YYYY, h:mm:ss a")

      
    },
    watch: {      
      		  
        

    },
    methods: {    

		getBucketListCallBack(response){
			if(response.length > 0){
				for (let i = 0; i < response.length; i++) {
					this.buckets.push(response[i].name)
				}
			}else{
				console.log("error")
			}
			this.loading = false;
		},

		inputFilesBucket(item){
			this.loading = true;
			var params = {
				name: item,
				prefix: ''
			}
			this.getBucketFilesCall(params,this.getBucketFilesCallBack)
		},

		outputFilesBucket(item){
			console.log(item);
		},


		getBucketFilesCallBack(response){
			console.log(response)
			this.loading = false;
		},

	

		
       
    },    
    computed: {
		showSelectedFiles () {
			// return this.files.length > 0
			return this.files.length > 0
			},      
		size () {
		const size = {xs:'x-small',sm:'small',lg:'large'}[this.$vuetify.breakpoint.name];
		return size ? { [size]: true } : {}
		}
  
    }
}

</script>

<style>
 .loading-overlay {
  display: none;
  background: rgba(255, 255, 255, 0.7);
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  top: 0;
  z-index: 9998;
  align-items: center;
  justify-content: center;
}

.loading-overlay.is-active {
  display: flex;
}

</style>

