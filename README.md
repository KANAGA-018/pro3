# pro3
<!DOCTYPE html>
<html>
<head>
<title>Smart Product Finder</title>

<style>

body{
font-family:Arial;
margin:0;
background:#f2f2f2;
}

/* LOGIN */

#loginPage{
text-align:center;
padding-top:120px;
}

input,select{
padding:10px;
margin:8px;
width:200px;
}

button{
padding:10px 15px;
background:#2c7be5;
color:white;
border:none;
cursor:pointer;
}

button:hover{
background:#1a5edb;
}

/* MAIN */

#mainPage{
display:none;
padding:20px;
}

header{
display:flex;
justify-content:space-between;
background:white;
padding:10px;
box-shadow:0 2px 5px rgba(0,0,0,0.2);
}

.products{
display:grid;
grid-template-columns:repeat(auto-fill,minmax(250px,1fr));
gap:20px;
margin-top:20px;
}

.card{
background:white;
padding:15px;
border-radius:8px;
box-shadow:0 2px 8px rgba(0,0,0,0.1);
}

.card img{
width:100%;
height:150px;
object-fit:cover;
}

.price{
color:green;
font-weight:bold;
}

.shop{
background:#eee;
padding:5px;
margin-top:5px;
}

/* ADMIN */

#adminPage{
display:none;
padding:20px;
}

</style>

</head>

<body>

<!-- LOGIN -->

<div id="loginPage">

<h2>Smart Product Finder</h2>

<input id="mobile" placeholder="Mobile Number"><br>

<select id="location">
<option>Karur</option>
<option>Trichy</option>
<option>Salem</option>
</select><br>

<select id="role">
<option>User</option>
<option>Admin</option>
</select><br>

<button onclick="login()">Login</button>

</div>


<!-- USER PAGE -->

<div id="mainPage">

<header>

<div>
<input id="search" placeholder="Search phone, face wash, hair oil">
<button onclick="searchProduct()">Search</button>
</div>

<div>
User: <span id="userMobile"></span> |
Location: <span id="userLocation"></span>
</div>

</header>

<div id="filters"></div>

<div class="products" id="productList"></div>

</div>


<!-- ADMIN PAGE -->

<div id="adminPage">

<h2>Admin Dashboard</h2>

<input id="pname" placeholder="Product name"><br>
<input id="pimage" placeholder="Image URL"><br>
<input id="amazonprice" placeholder="Amazon price"><br>
<input id="flipkartprice" placeholder="Flipkart price"><br>
<input id="shopname" placeholder="Shop name"><br>
<input id="shopprice" placeholder="Shop price"><br>

<button onclick="addProduct()">Add Product</button>

</div>


<script>

/* PRODUCT DATABASE */

let products=[

{
name:"iPhone 14",
type:"mobile",
brand:"Apple",
image:"https://images.unsplash.com/photo-1511707171634",
rating:"4.8",
review:"Excellent camera",
amazon:"https://www.amazon.in",
flipkart:"https://www.flipkart.com",
amazonPrice:78999,
flipkartPrice:78499,
shop:"Reliance Digital",
shopPrice:79000
},

{
name:"Samsung Galaxy S23",
type:"mobile",
brand:"Samsung",
image:"https://images.unsplash.com/photo-1580910051074",
rating:"4.7",
review:"Great performance",
amazon:"https://www.amazon.in",
flipkart:"https://www.flipkart.com",
amazonPrice:70999,
flipkartPrice:69999,
shop:"Mobile World",
shopPrice:70500
},

{
name:"Nivea Face Wash",
type:"skin",
skin:"Oily",
image:"https://images.unsplash.com/photo-1596755389378",
rating:"4.4",
review:"Good for oily skin",
amazon:"https://www.amazon.in",
flipkart:"https://www.flipkart.com",
amazonPrice:249,
flipkartPrice:239,
shop:"Apollo Pharmacy",
shopPrice:240
},

{
name:"Parachute Hair Oil",
type:"hair",
hair:"Normal",
image:"https://images.unsplash.com/photo-1620912189868",
rating:"4.5",
review:"Healthy hair",
amazon:"https://www.amazon.in",
flipkart:"https://www.flipkart.com",
amazonPrice:180,
flipkartPrice:175,
shop:"Super Market",
shopPrice:170
}

]

/* LOGIN */

function login(){

let mobile=document.getElementById("mobile").value
let location=document.getElementById("location").value
let role=document.getElementById("role").value

if(role=="Admin"){

document.getElementById("loginPage").style.display="none"
document.getElementById("adminPage").style.display="block"

}
else{

document.getElementById("loginPage").style.display="none"
document.getElementById("mainPage").style.display="block"

document.getElementById("userMobile").innerText=mobile
document.getElementById("userLocation").innerText=location

showProducts(products)

}

}

/* DISPLAY PRODUCTS */

function showProducts(list){

let html=""

list.forEach(p=>{

html+=`

<div class="card">

<img src="${p.image}">

<h3>${p.name}</h3>

⭐ ${p.rating}

<p>${p.review}</p>

<p class="price">Amazon ₹${p.amazonPrice}</p>
<a href="${p.amazon}" target="_blank">Buy Amazon</a>

<p class="price">Flipkart ₹${p.flipkartPrice}</p>
<a href="${p.flipkart}" target="_blank">Buy Flipkart</a>

<h4>Local Shop</h4>

<div class="shop">
${p.shop} ₹${p.shopPrice}
</div>

</div>

`

})

document.getElementById("productList").innerHTML=html

}

/* SEARCH */

function searchProduct(){

let q=document.getElementById("search").value.toLowerCase()

let result=products.filter(p=>p.name.toLowerCase().includes(q))

showProducts(result)

showFilters(q)

}

/* FILTERS */

function showFilters(q){

let html=""

if(q.includes("phone")||q.includes("mobile")){

html=`
<h3>Brand</h3>
<button onclick="filterBrand('Apple')">Apple</button>
<button onclick="filterBrand('Samsung')">Samsung</button>
`
}

if(q.includes("face")||q.includes("skin")){

html=`
<h3>Skin Type</h3>
<button onclick="filterSkin('Oily')">Oily</button>
`
}

if(q.includes("hair")){

html=`
<h3>Hair Type</h3>
<button onclick="filterHair('Normal')">Normal</button>
`
}

document.getElementById("filters").innerHTML=html

}

/* FILTER FUNCTIONS */

function filterBrand(b){
showProducts(products.filter(p=>p.brand==b))
}

function filterSkin(s){
showProducts(products.filter(p=>p.skin==s))
}

function filterHair(h){
showProducts(products.filter(p=>p.hair==h))
}

/* ADMIN ADD PRODUCT */

function addProduct(){

let p={

name:document.getElementById("pname").value,
image:document.getElementById("pimage").value,
amazonPrice:document.getElementById("amazonprice").value,
flipkartPrice:document.getElementById("flipkartprice").value,
shop:document.getElementById("shopname").value,
shopPrice:document.getElementById("shopprice").value,
amazon:"#",
flipkart:"#",
rating:"4.5",
review:"New product"

}

products.push(p)

alert("Product Added")

}

</script>

</body>
</html>
