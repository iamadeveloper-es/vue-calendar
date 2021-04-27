<template>
  <header class="header">
      <div class="container">
        <nav class="header__nav">
            <ul class="header__ul">
                <li class="header__li" v-for="(link, i) in navLinks" :key="i">
                    <router-link class="header__link" :to="link.url">{{link.text}}</router-link>
                </li>
            </ul>
        </nav>
      </div>
  </header>
</template>

<script>
export default {
    name: 'AppHeader',
    data(){
        return{
            navLinks: [
                {
                    text: 'Home',
                    url: '/'
                },
                {
                    text: 'About',
                    url: '/about'
                }
            ]
        }
    }
}
</script>

<style lang="scss" scoped>
    .header{
        background-color: #1497F6;
        padding: 25px 0;
    }
    .header__ul{
        padding: 0;
        margin: 0;
        display: flex;
    }
    .header__li{
        list-style: none;
    }
    .header__link{
        color: #ffffff;
        text-decoration: none;
        padding: 10px;
    }
</style>