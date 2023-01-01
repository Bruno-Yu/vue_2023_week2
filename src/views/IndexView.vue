<template>
  <div>
    <h2>這是index</h2>
    <!-- search wrapper -->

    {{ confirm }}
    <!-- content-card -->
    <!-- <SearchCardA :findCompany="findCompany"/> -->
    <SearchResult :findCompanies="findCompanies" />



  </div>
</template>


<script>
// 台灣水泥股份有限公司
import { mapState } from 'pinia';
// import { companyStore } from '../stores/companyInfo';
import { userStore } from '@/stores';
// import { computed } from 'vue';


// import SearchCardA from '../components/SearchCardA.vue';
import SearchResult from '../components/SearchResult.vue';

export default {
  data() {
    return {
      findCompanies: [],
    }
  },
  components: {
    SearchResult,
  },
  // 對應store
  computed: {
    ...mapState(userStore, ['confirm'])
  },
  mounted() {
    const searchBtn = document.querySelector("[data-search-btn]");
    console.log('pinia', userStore);
    // console.log(this.rawData);
    searchBtn.addEventListener('click', () => {
      const searchInput = document.querySelector("[data-search-input]");
      let keyWord = searchInput.value.trim().toLowerCase();
      console.log(keyWord);
      if (keyWord.length) {
        this.findCompanies = this.rawData.filter((item) => item['公司名稱'].match(keyWord));
        console.log(this.findCompanies);
      } else {
        return
      }
    })
  }
}
</script>