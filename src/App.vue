<template>
<html>
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
	<title>::sample ::</title>
   
   
    <link href='http://fonts.googleapis.com/css?family=Open+Sans:400,700,600' rel='stylesheet' type='text/css'>
 
	
</head>
<body>

    <div class="container">
    	 <div class="register col-md-5 col-sm-6">
                <h1 class="title"><strong>Bio Data</strong>
				</h1>
				
				
                <form role="form">
                    <div class="form-group">
                    <label class="reg_txt">Name <span>*</span></label>
                        <div class="controls form-inline">       
                          <input type="text" class="input-name" v-model="firstName" :disabled="viewMode" placeholder="First">
                          <input type="text" class="input-name" v-model="lastName" :disabled="viewMode" placeholder="Last">
   					    </div>                        
                    </div>
                    <div class="clearfix"></div>
                    
                    <div class="form-group">
                    <label class="reg_txt">Email  <span>*</span></label>
                        <input type="text" v-model="email" :disabled="viewMode || disableEmail" class="form-register text" id="" placeholder="E-mail">
                    </div>
                    <div class="clearfix"></div>
                    
                    <div class="form-group" style="height:70px;">
                    <label class="reg_txt">Phone Number  <span>*</span></label>
                    	<div class="clearfix"></div>
                       <div class="wsite-form">
							<input type="text" class="text input-name1" v-model="phoneStateCode" :disabled="viewMode" >
					   </div>
                       <div class="line">-</div>
                       <div class="wsite-form">
							<input type="text" class="text input-name1" v-model="phoneSecond" :disabled="viewMode" >
					   </div>
                       <div class="line">-</div>
                       <div class="wsite-form">
							<input type="text" class="text input-name1" v-model="phoneThird" :disabled="viewMode" >
					   </div>
                       
                    </div>
                    
                    <div class="clearfix"></div>
                    
                    <div class="form-group">
                    <label class="reg_txt">Address  <span>*</span></label>
                        <input type="text" class="form-register text" v-model="addressLine1" id="" placeholder="Line 1" style="margin-bottom:15px;" :disabled="viewMode">
                        <input type="text" class="form-register text" v-model="addressLine2" id="" placeholder="Line 2" :disabled="viewMode">
                    </div>
                    
                    <div class="form-group">                    
                        <div class="controls form-inline">       
                          <input type="text" class="input-name" v-model="city" placeholder="City" :disabled="viewMode" >
                          <input type="text" class="input-name" v-model="state" placeholder="State" :disabled="viewMode" >
   					    </div>                        
                    </div>
                    
                    <div class="form-group">                    
                        <div class="controls form-inline">       
                          <input type="text" class="input-name" v-model="zipCode" placeholder="Zip Code" :disabled="viewMode" >
                          <input type="text" class="input-name" v-model="country" placeholder="Country" :disabled="viewMode" >
   					    </div>                        
                    </div>
					
					<div class="form-group">
                    <label class="reg_txt">Write Your qualification <span>*</span></label>
                        <input type="text" v-model="qualification" class="form-register text" id="" placeholder="" style="margin-bottom:15px;" :disabled="viewMode">
                        <!-- <input type="text" class="form-register text" id="" placeholder="Add your qualification"> <span><img alt="" src="images/plus.png" class="add"></span> -->
                    </div>
                    
                    
                    <div class="clearfix"></div>
                    
                    <div class="form-group">
                    <label class="reg_txt">Comment  <span>*</span></label>                        
                        <textarea v-model="comment" class="form-register text" :disabled="viewMode"></textarea>
                    </div>
                    
                    <div class="form-group">
                        <button v-if="!viewMode"  class="btn btn-default submit" @click.prevent="checkRequired" style="width:97%;">Submit</button>
                    </div>
                </form>
             
        </div>
		
		<div class="col-md-6 tabt">
		    <table class="table">
  
          <tbody>
		  <tr class="ztxt">
	  <th>Name</th>
      <th>Email</th>
      <th>Phone</th>
      <th>Edit</th>
	    <th>Delete</th>
	    <th>View</th>
      </tr>
    <tr v-for="user in userList.users"
        :key="user.email">
	    <td style="max-width:25px;overflow: hidden;">{{ user.firstName }}</td>
      <td style="max-width:25px;overflow: hidden;">{{ user.email }}</td>
      <td style="max-width:25px;overflow: hidden;">phone</td>
	    <td>
        <div v-if="selectedUser!==user.email && selectedUser ===''" ><button class="ed" @click="fieldHydrate(user,'editButtonCLicked')">Edit</button></div>
        <div v-else-if="selectedUser!=='' && selectedUser==user.email" ><button class="ed" @click="selectedUserToggle()">cancel</button></div>
      </td>
	    <td><button class="ed" style="background:#f00;" @click="deleteUser(user.email)">Delete</button></td>
	    <td>
        <div  v-if="!viewMode"><button class="ed" style="background:#000;" @click=" viewUserDetails(user)">View</button></div>
        <div v-else-if="viewMode && selectedUser===user.email"><button class="ed" style="background:#000;" @click="toggleViewMode()">close</button></div>
      </td>
    </tr>
	</tbody>
	</table>
		</div>
       
    </div>
   

</body>
</html>

</template>
<script setup>
import { ref, reactive, onMounted } from "vue";
import emailValidator from "email-validator";

const firstName = ref("");
const lastName = ref("");
const email = ref("");
const addressLine1 = ref("");
const addressLine2 = ref("");
const phoneStateCode = ref("");
const phoneSecond = ref("");
const phoneThird = ref("");
const city = ref("");
const state = ref("");
const zipCode = ref("");
const country = ref("");
const qualification = ref("");
const comment = ref("");
let viewMode = ref(false)
let selectedUser = ref('')
let disableEmail = ref(false)
let userList = reactive({
  users: [
  {
    firstName:'barsha',
    lastName:"singha",
    email:"bhk@email.com",
    addressLine1:"near china",
    addressLine2:"aiya",
    phone:"215-000-0000",
    city:'wuhan',
    state:'assam',
    zipCode:'0000',
    country:'india',
    qualification:'form validation',
    comment:'says aiya a lot',
    phoneStateCode:'215',
    phoneSecond:'000',
    phoneThird:'0005'
  },
  {
    firstName:'manisj',
    lastName:"kamalehs",
    email:"manish@email.com",
    phone:"210-000-0000",
    addressLine1:"bihar",
    addressLine2:"patna",
    city:'somehwer',
    state:'bihari',
    zipCode:'1111',
    country:'india',
    qualification:'backend',
    comment:'nothing',
    phoneStateCode:'215',
    phoneSecond:'000',
    phoneThird:'0004'
  }
]
})
onMounted( () => {
  this.$notify({
  title: "Important message",
  text: "Hello user!",
});
})
function checkRequired(){
  const user = {
    firstName: firstName.value,
    lastName: lastName.value,
    email: email.value,
    addressLine1: addressLine1.value,
    addressLine2: addressLine2.value,
    phone: phoneStateCode + "-" + phoneSecond + "-" + phoneThird,
    city: city.value,
    state: state.value,
    zipCode: zipCode.value,
    country: country.value,
    qualification: qualification.value,
    comment: comment.value,
    phoneStateCode:phoneStateCode.value,
    phoneSecond:phoneSecond.value,
    phoneThird:phoneThird.value
  }
  if(firstName.value !== '' && email.value !== '' && addressLine1.value !== '' &&  qualification.value !== '' && comment.value !== '' && phoneStateCode.value !== '' && phoneSecond.value !== ''&& phoneThird.value !== ''){
    console.log("user details:",firstName,lastName,email,addressLine1,addressLine2,city,state,zipCode,country,qualification,comment)
    const userExist = checkUserExist(email.value)
    let isValid = fieldValidation()
    if(isValid){
      if(userExist){
      editUserDetail(user)
    } else{
      createUser(user)
    }
    }
    else{
      console.log("not valid")
    }
    
    
  }
  else{
    alert("enter all required fields")
    console.log("enter all input")
  }
}
function fieldValidation(){
  console.log("//",emailValidator.validate("test@email.com"));
  if(emailValidator.validate(email.value) && !isNaN(phoneStateCode.value) && phoneStateCode.value.length === 3 && !isNaN(phoneSecond.value) && phoneSecond.value.length === 3  &&  phoneThird.value.length === 4 && !isNaN(phoneThird.value)){
    return true
  }
  else{
    alert("enter valid inputs")
    return false
  }
}
function checkUserExist(email){
  if(userList.users.some(user => user.email === email)){
    console.log("user exist")
    return true
  }
  else{
    console.log("user do not exist")
    return false
  }
}
function createUser(user){
  userList.users.push(user)
  alert("user created successfully")
  clearFields()
  console.log("after creating:",userList)
}
function selectedUserToggle(){
  selectedUser.value = ''
  disableEmail.value = false
  clearFields()
}
function viewUserDetails(user){
  fieldHydrate(user)
  selectedUser.value = user.email
  console.log("selectedUser",selectedUser.value)
  viewMode.value = true
}
function toggleViewMode(){
  viewMode.value = false
  clearFields()
}
function fieldHydrate(user,mode = "editButtonNotClicked"){
  if(mode === "editButtonCLicked"){
    selectedUser.value = user.email
    disableEmail.value = true
  }
  firstName.value = user.firstName
  lastName.value = user.lastName
  email.value = user.email
  addressLine1.value = user.addressLine1
  addressLine2.value = user.addressLine2
  city.value = user.city
  state.value = user.city
  zipCode.value = user.zipCode
  country.value = user.country
  comment.value = user.comment
  qualification.value = user.qualification
  phoneStateCode.value = user.phoneStateCode
  phoneSecond.value = user.phoneSecond
  phoneThird.value = user.phoneThird
}
function editUserDetail(user){
  for (const obj of userList.users) {
  if (obj.email === user.email) {
    obj.firstName =user.firstName
    obj.lastName = user.lastName
    obj.email = user.email
    obj.addressLine1 = user.addressLine1
    obj.addressLine2 = user.addressLine2
    obj.city = user.city
    obj.state = user.state
    obj.zipCode = user.zipCode
    obj.country = user.country
    obj.comment = user.comment
    obj.qualification = user.qualification
    obj.phone = user.phoneStateCode + '-' + user.phoneSecond + '-' + user.phoneThird
    obj.phoneStateCode = user.phoneStateCode
    obj.phoneSecond = user.phoneSecond
    obj.phoneThird = user.phoneThird
    fieldHydrate(user)
    alert("edit success")
    clearFields()
    break;
  }
  else{
    clearFields()
  }
}
}
async function deleteUser(email){
  console.log("user email  in delete:",email)
  const userCheck = checkUserExist(email)
  if(userCheck === true){
    userList.users = userList.users.filter(function (el) {
            return el.email !== email;
  });
  clearFields()
  }
  else{
    console.log("user not found")
    
  }
}
function clearFields(){
  firstName.value = ""
  lastName.value = ""
  email.value = ""
  addressLine1.value = ""
  addressLine2.value = ""
  city.value = ""
  state.value = ""
  zipCode.value = ""
  country.value = ""
  comment.value = ""
  qualification.value = ""
  phoneStateCode.value = ""
  phoneSecond.value = ""
  phoneThird.value = ""
}

</script>
<style scoped>

</style>
