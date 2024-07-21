<template>
     <header class="flex gap-5 justify-between border-b border-t-slate-300 pb-5 px-10 py-8">
      <div class="flex items-center gap-4">
        <img src="/public/logo.png" alt="logo" class="w-10 h-10" />
        <div>
          <h2 class="text-xl font-bold uppercase">Vue Sneaker</h2>
          <p class="title">The Best Sneaker Town</p>
        </div>
      </div>
      <ul class="flex gap-10">
        <li class="navList hover:text-black hover:font-bold"><img src="/public/cart.svg" alt="" /><p>1205 $</p></li>
        <li class="navList hover:text-black hover:font-bold"><img src="/public/heart.svg" alt="" /><p>Закладки</p></li>
        <li class="navList hover:text-black hover:font-bold"><img src="/public/profile.svg" alt="" /><p>Profile</p></li>
      </ul>
    </header>
</template>

<style scope>
.navList {
  display: flex;
  align-items: center;
  gap: 10px;
  color: rgb(130, 130, 130); 
  cursor: pointer;
}
.title {
  color: hsla(0, 0%, 62%, 1); 
}
</style>