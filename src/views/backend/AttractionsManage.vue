<template>
  <main>
    <div class="grid gap-2 grid-cols-1 xl:grid-cols-2">
      <div class="flex flex-col mb-8 xl:mb-0">
        <div class="overflow-x-auto sm:-mx-6 lg:-mx-8">
          <div class="py-4 inline-block min-w-full sm:px-6 lg:px-8">
            <div class="overflow-hidden border border-solid border-gray-300">
              <table class="min-w-full text-center ">
                <thead class="border-b bg-gray-800">
                  <tr>
                    <th scope="col" class="text-sm font-medium text-white px-6 py-4">
                      產品名稱
                    </th>
                    <th scope="col" class="text-sm font-medium text-white px-6 py-4">
                      原價
                    </th>
                    <th scope="col" class="text-sm font-medium text-white px-6 py-4">
                      售價
                    </th>
                    <th scope="col" class="text-sm font-medium text-white px-6 py-4">
                      是否啟用
                    </th>
                    <th scope="col" class="text-sm font-medium text-white px-6 py-4">
                      查看細節
                    </th>
                  </tr>
                </thead>
                <tbody>
                  <tr class="bg-white border-b" v-for="item in data" :key="item.id">
                    <td class="px-6 py-4 whitespace-nowrap text-sm font-medium text-gray-900"> {{ item.title }}</td>
                    <td class="text-sm text-gray-900 font-light px-6 py-4 whitespace-nowrap">
                      {{ item.origin_price }}
                    </td>
                    <td class="text-sm text-gray-900 font-light px-6 py-4 whitespace-nowrap">
                      {{ item.price }}
                    </td>
                    <td class="text-sm text-gray-900 font-light px-6 py-4 whitespace-nowrap">
                      <div class="flex justify-center">
                        <div class="form-check form-switch">
                          <input
                            class="form-check-input appearance-none w-9 -ml-10 rounded-full float-left h-5 align-top bg-white bg-no-repeat bg-contain bg-gray-300 focus:outline-none cursor-pointer shadow-sm"
                            type="checkbox" role="switch" id="flexSwitchCheckDefault"
                            :checked="item.is_enabled === 1 ? true : false">
                          <label class="form-check-label inline-block text-gray-800"
                            for="flexSwitchCheckDefault"></label>
                        </div>
                      </div>
                    </td>
                    <td class="text-sm text-gray-900 font-light px-6 py-4 whitespace-nowrap">
                      <button type="button"
                        class="inline-block px-6 py-2.5 bg-gray-800 text-white font-medium text-xs leading-tight uppercase rounded shadow-md hover:bg-gray-900 hover:shadow-lg focus:bg-gray-900 focus:shadow-lg focus:outline-none focus:ring-0 active:bg-gray-900 active:shadow-lg transition duration-150 ease-in-out"
                        @click="check(item)">查看細節</button>
                    </td>
                  </tr>
                </tbody>
              </table>
            </div>
          </div>
        </div>
        <p class="font-semibold">目前有產品 {{ data.length }} 項</p>
      </div>
      <div>
        <h2 class="text-4xl mb-5 font-bold">詳情如下</h2>
        <div v-if="Object.keys(currentItem).length > 0" class="flex justify-center">
          <div class="flex flex-col md:flex-row rounded-lg bg-white shadow-lg">
            <img class=" w-full h-96 md:h-auto object-cover md:w-48 rounded-t-lg md:rounded-none md:rounded-l-lg"
              :src="currentItem.imageUrl" :alt="currentItem.title" />
            <div class="p-6 flex flex-col justify-start">
              <h5 class="text-gray-900 text-xl font-medium mb-2">{{ currentItem.title }}</h5>
              <p class="text-gray-700 text-base mb-2">
                {{ currentItem.category }}
              </p>
              <p class="text-gray-700 text-base mb-4">
                {{ currentItem.description }}
              </p>
              <p>{{ currentItem.price }}/{{ currentItem.unit }} <span
                  class="text-gray-600 text-xs">還剩{{ currentItem.num }}個</span> </p>
              <div v-if="currentItem.imagesUrl.length" class="grid gap-1 grid-rows-1 grid-cols-3 shadow-lg">
                <img v-for="(item, index) in currentItem.imagesUrl" :key="index" class="h-40 w-40 block object-cover"
                  :src="item" :alt="item">

              </div>
            </div>
          </div>
        </div>
        <p v-else class="text-gray-600">請選擇單一產品查看</p>
      </div>
    </div>
  </main>
</template>

<script>
// import IndexView from './IndexView.vue'
//  產品資料
import atrApi from '@/api/atrAPI';
import { ref, onMounted } from 'vue';
import { userStore } from '@/stores';
import { useRouter } from 'vue-router';


export default {
  // components: { IndexView }
  setup() {
    const data = ref([]);
    const currentItem = ref({});
    const store = userStore();
    const router = useRouter();

    function check(prop) {
      currentItem.value = prop;
    }
    async function getAdminProducts() {
      const res = await atrApi.getAdminProducts();
      console.log(res);
      if (res.success) {
        data.value = res.products;
      }
    }

    onMounted(() => {
      // data.value = products
      if (store.$state.login) {
        getAdminProducts();
      } else {
        router.push('/');
      }
    });
    return {
      data,
      currentItem,
      check
    };
  }
}


</script>