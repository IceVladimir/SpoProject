<script setup>
import { onMounted, ref, watch, reactive, provide } from 'vue'
import axios from 'axios'

import Header from './components/Header.vue'
import CardList from './components/CardList.vue'
import Drawer from './components/Drawer.vue'
import Login from './components/Login.vue'
import Profile from './components/Profile.vue'

const items = ref([])
const cart = ref([])
const userMiscInfo = ref([])
const cartSummary = ref('0')
const cartDiscount = ref('0')

const UserInfo = reactive({
  isAuth: 'false',
  UserId: '',
})

const filters = reactive({
  sortBy: 'name',
  searchQuerry: '',
})

const changedUser = async () => {
  sessionStorage.setItem('isAuth', UserInfo.isAuth || 'false')
  sessionStorage.setItem('UserId', UserInfo.UserId || '')
}

const drawerOpen = ref(false)
const loginOpen = ref(false)
const profileOpen = ref(false)
const authFailed = ref(false)

const closeDrawer = () => {
  drawerOpen.value = false
}

const openDrawer = () => {
  drawerOpen.value = true
}

const closeLogin = () => {
  loginOpen.value = false
}

const openLogin = () => {
  loginOpen.value = true
}

const closeProfile = () => {
  profileOpen.value = false
}

const openProfile = () => {
  profileOpen.value = true
}

const onChangeSelect = (event) => {
  filters.sortBy = event.target.value
}

const onChangeSearchInput = (event) => {
  filters.searchQuerry = event.target.value
}

const addItemToCart = async (title, price, imageUrl, discount) => {
  var token = UserInfo.UserId
  if (token == '') {
    return
  }
  try {
    await axios
      .post(`https://7a5c2211aae9a6e8.mokky.dev/cart`, {
        title: title,
        price: price,
        imageUrl: imageUrl,
        user_id: token,
        discount: discount,
      })
      .then(function (response) {
        getUserCart()
        console.log(response)
      })
      .catch(function (error) {
        console.log(error)
      })
  } catch (err) {
    console.log(err)
  }
}

const deleteItemFromCart = async (id) => {
  try {
    await axios.delete(`https://7a5c2211aae9a6e8.mokky.dev/cart/` + id)
    getUserCart()
  } catch (err) {
    console.log(err)
  }
}

const tryToLogout = async () => {
  try {
    UserInfo.UserId = ''
    UserInfo.isAuth = 'false'
    closeProfile()
    getUserCart()
  } catch (err) {
    console.log(err)
  }
}

const tryToDeleteAccount = async () => {
  var token = UserInfo.UserId
  try {
    await axios.delete(`https://7a5c2211aae9a6e8.mokky.dev/users/` + token)
    tryToLogout()
  } catch (err) {
    console.log(err)
  }
}

const getUserCart = async () => {
  var token = UserInfo.UserId
  var summary = 0
  var discount = 0
  try {
    const { data } = await axios.get(`https://7a5c2211aae9a6e8.mokky.dev/cart?user_id=` + token)
    for (let i = 0; i < data.length; i++) {
      summary = summary + data[i].price
      discount = discount + data[i].discount
    }
    cartSummary.value = summary.toString()
    cartDiscount.value = discount.toString()
    cart.value = data
  } catch (err) {
    console.log(err)
  }
}

const getUserMiscInfo = async () => {
  var token = UserInfo.UserId
  try {
    const { data } = await axios.get(`https://7a5c2211aae9a6e8.mokky.dev/users?id=` + token)
    userMiscInfo.value = data[0]
  } catch (err) {
    console.log(err)
  }
}

const tryToLogin = async () => {
  var email = document.getElementById('emailField').value
  var password = document.getElementById('passwordField').value
  try {
    const { data } = await axios.get(`https://7a5c2211aae9a6e8.mokky.dev/users`)
    for (let i = 0; i < data.length; i++) {
      console.log(data[i].email, email, data[i].pass, password)
      if (data[i].email == email && data[i].pass == password) {
        UserInfo.UserId = data[i].id
        UserInfo.isAuth = 'true'
        closeLogin()
        getUserCart()
      }
    }
    authFailed.value = true
    setTimeout(() => {
      authFailed.value = false
    }, 2000)
  } catch (err) {
    console.log(err)
  }
}

function setUpUser() {
  UserInfo.isAuth = sessionStorage.getItem('isAuth') || 'false'
  UserInfo.UserId = sessionStorage.getItem('UserId') || ''
  sessionStorage.setItem('isAuth', UserInfo.isAuth || 'false')
  sessionStorage.setItem('UserId', UserInfo.UserId || '')
  getUserCart()
}

const fetchItems = async () => {
  try {
    const params = {
      sortBy: filters.sortBy,
    }

    if (filters.searchQuerry) {
      params.name = `*${filters.searchQuerry}*`
    }

    const { data } = await axios.get(`https://7a5c2211aae9a6e8.mokky.dev/drugs`, { params })
    items.value = data
  } catch (err) {
    console.log(err)
  }
}

onMounted(fetchItems)
onMounted(setUpUser)

watch(filters, fetchItems)
watch(UserInfo, changedUser)

provide('cartActions', {
  closeDrawer,
  openDrawer,
  addItemToCart,
  getUserCart,
  deleteItemFromCart,
})

provide('loginActions', {
  closeLogin,
  openLogin,
  tryToLogin,
  getUserMiscInfo,
  tryToLogout,
  tryToDeleteAccount,
})

provide('profileActions', {
  closeProfile,
  openProfile,
})
</script>

<script>
import Carousel from './components/carousel/Carousel.vue'

export default {
  name: 'App',
  components: { Carousel },
  data: () => ({
    slides: ['./banners/banner1.png', './banners/banner2.png'],
  }),
}
</script>

<template>
  <Drawer
    v-if="drawerOpen"
    :cartItems="cart"
    :cartSummary="cartSummary"
    :cartDiscount="cartDiscount"
  />
  <Login @tryToLogin="tryToLogin" :authFailed="authFailed" v-if="loginOpen" />
  <Profile
    v-if="profileOpen"
    :miscInfo="userMiscInfo"
    @tryToLogout="tryToLogout"
    @tryToDeleteAccount="tryToDeleteAccount"
  />
  <div class="bg-white w-4/5 m-auto rounded-xl shadow-xl mt-14">
    <Header
      @openDrawer="openDrawer"
      @openLogin="openLogin"
      @openProfile="openProfile"
      @getUserMiscInfo="getUserMiscInfo"
      :isAuth="UserInfo.isAuth === 'true' ? true : false"
      :cartSummary="cartSummary"
    />

    <carousel :slides="slides" :interval="3000" controls indicators></carousel>

    <div class="p-10">
      <div class="flex justify-between items-center">
        <h2 class="text-3xl font-bold mb-10">Доступные товары:</h2>

        <div class="flex gap-4 mb-10">
          <select @change="onChangeSelect" class="py-2 px-3 border rounded-md outline-none">
            <option value="name">По названию</option>
            <option value="price">По цене (дешевые)</option>
            <option value="-price">По цене (дорогие)</option>
          </select>

          <div class="relative">
            <img class="absolute left-3 top-3.5" src="/search.svg" />
            <input
              @input="onChangeSearchInput"
              class="border rounded-md py-2 pl-11 pr-4 transition outline-none focus:border-gray-400"
              type="text"
              placeholder="Поиск..."
            />
          </div>
        </div>
      </div>
      <div class="mt-10">
        <CardList :items="items" />
      </div>
    </div>
  </div>
</template>
