<template>
  <div class="Dashboard">
    <div class="row" style="width:98%; margin-left:1%">
      <div class="col-2 bg-light rounded" style="margin-top: 20px; margin-bottom: 20px; padding: 0px">
        <div class="userNav text-success">
          <li>
            <ul>
              <p>{{ email }}</p>
            </ul>
            <ul v-if="role == 'customer'">
              <a href="#" @click.prevent="changePage('cartList')">Cart</a>
            </ul>
            <ul v-if="role == 'customer'">
              <a href="#" @click.prevent="changePage('wishList')">Wishlist</a>
            </ul>
          </li>
        </div>
      </div>
      <div class="col-10 border bg-light rounded shadow-sm" style="margin-top: 20px; margin-bottom: 20px; padding: 30px;">
        <div class="row">
          <div class="col-4 offset-4 text-success">
            <h1 v-if="currentPage == 'productList'">Products</h1>
            <h1 v-if="currentPage == 'addForm'">Add Product</h1>
            <h1 v-if="currentPage == 'editForm'">Edit Product</h1>
            <h1 v-if="currentPage == 'cartList'">Cart</h1>
            <h1 v-if="currentPage == 'wishList'">Wishlist</h1>
          </div>
          <div class="col-4" style="display: flex; justify-content: flex-end">
            <button v-if="currentPage == 'productList' && role == 'admin'" @click.prevent="changePage('addForm')" class="btn btn-success" style="padding-top: 8px">Add Product</button>
            <button v-if="currentPage !== 'productList'" @click.prevent="changePage('productList')" class="btn btn-success" style="padding-top: 8px">Back</button>
          </div>
        </div>
        <br>
        <div v-if="currentPage == 'productList'" class="products">
          <Product
          v-for="product in products"
          :key="product.id"
          :product="product"
          :changePage="changePage"
          />
        </div>
        <div v-if="currentPage == 'cartList'" class="carts">
          <Cart
          v-for="cart in carts"
          :key="cart.id"
          :cart="cart"
          :changePage="changePage"
          />
        </div>
        <div v-if="currentPage == 'wishList'" class="wishlist">
          <Wishlist
          v-for="wish in wishlist"
          :key="wish.id"
          :wish="wish"
          :changePage="changePage"
          />
        </div>
        <div v-if="currentPage == 'addForm'">
          <AddForm :changePage="changePage"/>
        </div>
        <div v-if="currentPage == 'editForm'">
          <EditForm :changePage="changePage"/>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import Product from '../components/Products'
import AddForm from '../components/AddForm'
import EditForm from '../components/EditForm'
import Cart from '../components/Carts'
import Wishlist from '../components/Wishlist'

export default {
  name: 'Dashboard',
  data () {
    return {
      email: localStorage.email,
      role: localStorage.role
    }
  },
  components: {
    Product,
    AddForm,
    EditForm,
    Cart,
    Wishlist
  },
  methods: {
    changePage (page) {
      this.$store.dispatch('changePage', page)
    }
  },
  computed: {
    currentPage () {
      return this.$store.state.currentPage
    },
    products () {
      return this.$store.state.products
    },
    carts () {
      return this.$store.state.carts
    },
    userDetail () {
      return this.$store.state.userDetail
    },
    wishlist () {
      return this.$store.state.wishlist
    }
  },
  created () {
    this.$store.dispatch('fetchCarts')
    this.$store.dispatch('fetchWishlist')
    return this.$store.dispatch('fetchProducts')
  }
}
</script>
