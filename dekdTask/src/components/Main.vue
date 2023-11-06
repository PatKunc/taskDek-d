<script setup>
import { onMounted, ref,onBeforeUnmount } from 'vue';
import { ListBulletIcon,BookmarkIcon,TrashIcon } from '@heroicons/vue/24/solid'

const imgData = ref({})
const url = ref("")
const currentComponent = ref("default")
const deleteItem = ref()
const idsToDelete = ref([])
const curImg = ref(0)
let intervalid

const images = ref([
           "https://fastly.picsum.photos/id/7/4728/3168.jpg?hmac=c5B5tfYFM9blHHMhuu4UKmhnbZoJqrzNOP9xjkV4w3o",
           "https://fastly.picsum.photos/id/0/5000/3333.jpg?hmac=_j6ghY5fCfSD6tvtcV74zXivkJSPIfR9B8w34XeQmvU",
           "https://fastly.picsum.photos/id/12/2500/1667.jpg?hmac=Pe3284luVre9ZqNzv1jMFpLihFI6lwq7TPgMSsNXw2w"
        ])

const dataArr = ref([
    {
        id:1,
        title: "Test 1",
        author: "AuthorTest",
        chapter: "ตอนที่ 18: Test1",
        updatedAt: "2023-07-18T01:30:00.000-05:00"
    },
    {
        id:2,
        title: "Test 2",
        author: "AuthorTest",
        chapter: "ตอนที่ 18: Test1",
        updatedAt: "2023-07-18T01:30:00.000-05:00"
    },
    {
        id:3,
        title: "Test 3",
        author: "AuthorTest",
        chapter: "ตอนที่ 18: Test1",
        updatedAt: "2023-07-18T01:30:00.000-05:00"
    },
    {
        id:4,
        title: "Test 4",
        author: "AuthorTest",
        chapter: "ตอนที่ 18: Test1",
        updatedAt: "2023-07-18T01:30:00.000-05:00"
    },
    {
        id:5,
        title: "Test 5",
        author: "AuthorTest",
        chapter: "ตอนที่ 18: Test1",
        updatedAt: "2023-07-18T01:30:00.000-05:00"
    },
    {
        id:6,
        title: "Test 6",
        author: "AuthorTest",
        chapter: "ตอนที่ 18: Test1",
        updatedAt: "2023-07-18T01:30:00.000-05:00"
    },
    {
        id:7,
        title: "Test 7",
        author: "AuthorTest",
        chapter: "ตอนที่ 18: Test1",
        updatedAt: "2023-07-18T01:30:00.000-05:00"
    },
    {
        id:8,
        title: "Test 8",
        author: "AuthorTest",
        chapter: "ตอนที่ 18: Test1",
        updatedAt: "2023-07-18T01:30:00.000-05:00"
    },
    {
        id:9,
        title: "Test 9",
        author: "AuthorTest",
        chapter: "ตอนที่ 18: Test1",
        updatedAt: "2023-07-18T01:30:00.000-05:00"
    },
    {
        id:10,
        title: "Test 10",
        author: "AuthorTest",
        chapter: "ตอนที่ 18: Test1",
        updatedAt: "2023-07-18T01:30:00.000-05:00"
    },
    {
        id:11,
        title: "Test 11",
        author: "AuthorTest",
        chapter: "ตอนที่ 18: Test1",
        updatedAt: "2023-07-18T01:30:00.000-05:00"
    },
])

const deleteById = (ids)=>{
dataArr.value = dataArr.value.filter((x) => !x.selected)
switchComponent()
}
const changeTime = (date) => {
  const time = new Date(date)
  if (time.getFullYear() > 1970)
  return time.toLocaleString('en-GB',{dateStyle:'medium', timeStyle:'short'})
  else return '-'
}
const getImg = async () => {
  try {
    const res = await fetch("https://picsum.photos/seed/picsum/info");

    if(res.ok) {
      const ann = await res.json();
      return ann;
      }
  } catch (error) {
    console.log(`ERROR cannot read data: ${error}`);
  }
};
const switchComponent = ()=>{
    currentComponent.value = currentComponent.value==="default"?"edit":"default"
}
const handleCheckboxChange = (item) => {
  if (item.selected) {
    idsToDelete.value.push(item.id);
  } else {
    const index = idsToDelete.value.indexOf(item.id);
    if (index !== -1) {
      idsToDelete.value.splice(index, 1);
    }
  }
  console.log(idsToDelete.value)
};
const nextImg = ()=>{
    if(curImg.value >= images.value.length){curImg.value = 1}
    else{
    curImg.value = (curImg.value +1)%images.value.length}
}
const prevImg = ()=>{
    if(curImg.value < 1){curImg.value = images.value.length-1}
    else{
    curImg.value--
}
}

onMounted(async ()=>{
imgData.value = await getImg()
url.value = imgData.value.download_url
})
onBeforeUnmount(()=>{
    clearInterval(intervalid)
})
intervalid = setInterval(nextImg,3000)
</script>
 
<template>
    <link href='https://fonts.googleapis.com/css?family=Kanit&subset=thai,latin' rel='stylesheet' type='text/css'>
<div class="all">

    <div class="bandiv">
        <!-- <img :src="url" class="banner" :alt="'Author: '+imgData.author"> -->
        <!-- <transition name="fade" mode="out-in"> -->
        <img class="banner" :src="images[curImg]" :key="curImg">
        <!-- </transition> -->
        <button class="pagebut" @click="prevImg">previous </button>
        <button class="pagebut" @click="nextImg">next</button>
    </div>

    <div class="list">
        <h1 style="padding-top: 30px;padding-bottom: 0px;">รายการที่คั่นไว้</h1>
    </div>
    <div class="totalMarked">
        <button v-if="currentComponent === 'default'" class="edit" @click="switchComponent">แก้ไข</button>
        <button v-if="currentComponent === 'edit'" class="edit" @click="switchComponent">ยกเลิก</button>
        <button v-if="currentComponent === 'edit'" class="edit" @click="deleteById(idsToDelete)">
             <TrashIcon style="height: 15px;margin-right: 5px;" />{{idsToDelete.length}} รายการ
        </button>
        <p>
            จำนวนทั้งหมด {{ dataArr.length }} รายการ
        </p>
    </div>
    <div class="item-container">
    <div v-for="(dt,index) in dataArr" :key="index" class="item">
        <div class="content">
            <div class="img-container">
            <img src="https://picsum.photos/200/300.jpg" style="border-radius: 10px;"/>
            </div>
            <div class="text-container">
            <p style="font-weight: bold;">{{ dt.title }}</p>   <input v-if="currentComponent ==='edit'" style="float: right;" type="checkbox" v-model="dt.selected" @change="handleCheckboxChange(dt)">
            <p style="font-weight: bold;">{{ dt.author }}</p>
            <p style="color: gray;font-size: 11px;"> <ListBulletIcon style="height: 15px;margin-right: 5px;" />{{ dt.chapter }}</p>
            <p style="color: gray;font-size: 11px;"> <BookmarkIcon style="height: 15px;margin-right: 5px;" />คั่นล่าสุด {{ changeTime(dt.updatedAt) }}</p>
            </div>
        </div>
    </div>
    </div>

</div>
</template>
 
<style scoped>
.all{
    /* font-family: "Trebuchet MS", "Lucida Sans Unicode", "Lucida Grande",
    "Lucida Sans", Arial, sans-serif; */
    font-family: 'Kanit', sans-serif;
}
/* banner ---------------------------------------------------- */
.banner{
    width: 100%;
    height: 300px;
    transition: opacity 0.5s ease-in-out; /* Add transition for opacity */
}
.bandiv{
    margin: 0;
    padding: 0;
    position: relative;
    /* display: flex;
  justify-content: center;
  align-items: center;
  height: 300px;
  overflow: hidden; */
}
/* banner ---------------------------------------------------- */

/* list ---------------------------------------------------- */
.list{
    padding-left: 150px;
    border-bottom: 0.5px solid lightgray;
    height: 80px;
}
/* list ---------------------------------------------------- */

.item-container {
  display: flex;
  flex-wrap: wrap;
  margin-top: 50px
}
.item {
  padding-left: 150px;
  margin-bottom: 10px; /* Add some space between items */
  flex-basis: calc(20% - 10px); /* 20% width with 10px spacing */
  /* margin: 5px; Add some spacing between items */
  box-sizing: border-box;
  /* border: 1px solid black; */
  /* padding-right: 30px; */
}

.content {
  display: flex;
  align-items: start; /* Align items to the top */
}

.content p {
    /* padding-top: -80px; */
  display: flex;
  /* align-items: center; */
}

.content img {
  max-width: 120px; /* Adjust the max-width of the image */
  margin-right: 10px; /* Add some space between image and text */
}
.img-container {
  margin-right: 10px; /* Add some space between image and text */
}
.text-container{
    width: 200px;
}


.totalMarked{
    margin-top: 40px;
    padding-left: 150px;
    color: gray;
}

.pagebut{
    margin-right: 5px;
    border-radius: 15px;
    border: 0.25px lightgray solid;
    padding: 10px;
    font-family: 'Kanit', sans-serif;
}
.edit{
    float: right;
    margin-right: 40px;
    border-radius: 15px;
    border: 0.25px lightgray solid;
    padding: 10px;
    font-family: 'Kanit', sans-serif;
}
.edit:hover{
    background-color: lightgray;
}


@media (max-width: 768px) {
  .list {
    padding-left: 0; /* Adjust padding for smaller screens */
  }

  .item-container {
    margin-top: 30px; /* Adjust spacing between items */
  }

  .item {
    padding-left: 0; /* Adjust padding for smaller screens */
    flex-basis: calc(33.33% - 10px); /* 33.33% width with 10px spacing */
  }
}

/* For screens 576px and smaller (common phone sizes) */
@media (max-width: 576px) {
  .item {
    flex-basis: calc(50% - 10px); /* 50% width with 10px spacing */
  }
}

/* For screens 375px and smaller (iPhone 6/7/8 sizes) */
@media (max-width: 375px) {
  .item {
    flex-basis: calc(100% - 10px); /* 100% width with 10px spacing */
  }
}
</style>