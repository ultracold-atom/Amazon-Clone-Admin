<template>
  <main>
    <div class="container-fluid">
      <div class="row">
        <div class="col-sm-3"></div>
        <div class="col-sm-6">
          <div class="a-section">
            <div class="a-spacing-top-medium"></div>
            <h2 style="text-align:center">Update the Product</h2>
            <form action="">
            
              <div class="a-spacing-top-medium">
                <label>Category</label>
                <select class="a-select-option" v-model="categoryID">
                    <option v-for="category in categories" :key="category._id" 
                    value="category._id">{{category.type}}</option>
                </select> 
              </div>

              <div class="a-spacing-top-medium">
                <label>Owner</label>
                <select class="a-select-option" v-model="ownerID">
                    <option v-for="owner in owners" :key="owner._id" 
                    value="owner._id">{{owner.name}}</option>
                </select> 
              </div>  

              <!-- Title Dropdown  -->
              <div class="a-spacing-top-medium">
                <label style="margin-bottom:10px">Title : </label>
                <input v-model="title" type="text" class="a-input-text" style="width:100%">
              </div>

              <!-- Price Dropdown  -->
              <div class="a-spacing-top-medium">
                <label style="margin-bottom:10px">Price : </label>
                <input v-model="price" type="number" class="a-input-text" style="width:100%">
              </div>

              <!-- StockQuantity Dropdown  -->
              <div class="a-spacing-top-medium">
                <label style="margin-bottom:10px">Stock Quantity : </label>
                <input v-model="stockQuantity" type="number" class="a-input-text" style="width:100%">
              </div>

              <!-- Description Dropdown  -->
              <div class="a-spacing-top-medium">
                <label style="margin-bottom:10px">Description : </label>
                <textarea v-model="description" placeholder="Provide details about the product" style="width:100%"> </textarea>
              </div>

              <!-- Photo Dropdown  -->
              <div class="a-spacing-top-medium">
                <label style="margin-bottom:0px">Add Photo </label>
                <div class="a-row a-spacing-top-medium">
                  <label class="choosefile-button">
                    <i class="fal fa-plus"></i>
                    <input type="file" @change="onFileSelected">
                    <p style="margin-top: -60px">{{fileName}}</p>
                  </label>  
                </div>
              </div>   
              
              <hr />

              <!-- Button -->
              <div class="a-spacing-top-large">
                <span class="a-button-register">
                  <span class="a-button-inner">
                    <span class="a-button-text" @click="onAddProduct">Add Product</span>
                  </span>  
                </span>
              </div>   

            </form>
          </div>  
        </div>
      </div>  
    </div>
  </main> 
</template>

<script>
export default {
  async asyncData({$axios,params}){
    try {
      let categories = $axios.$get("http://localhost:3000/api/categories")
      let owners = $axios.$get("http://localhost:3000/api/owners")
      let product = $axios.$get(`http://localhost:3000/api/products/${params.id}`)     
      
      const [catResponse,ownerResponse,productResponse] = await Promise.all([
        categories,
        owners,
        product
      ])
      
      console.log(productResponse)

      return{
        categories:catResponse.categories,
        owners:ownerResponse.owners,
        product:productResponse.product
      }

    } catch (error) {
      console.log(error)  
    }
  },

  data () {
   return {
     categoryID:null,
     ownerID:null,
     title:"",
     price:0,
     description:null,
     selectedFile:null,
     stockQuantity: 1,
     fileName:""
   }  
  },

  methods:{
    onFileSelected(e) {
      this.selectedFile = e.target.files[0]
      this.fileName = e.target.files[0].name
    },
    async onAddProduct() {
      let data = new FormData()
      data.append("title",this.title) 
      data.append("price",this.price)
      data.append("description",this.description)
      data.append("stockQuantity",this.stockQuantity)
      data.append("ownerID",this.ownerID)
      data.append("categoryID",this.categoryID)
      data.append("photo",this.selectedFile,this.selectedFile.name) 

      let result = await this.$axios.$post("http://localhost:3000/api/products",data)
      console.log(result)

      this.$router.push("/")
    } 

  }  
}
</script>