<template>
  <div id="product-design">
    <section class="main-title mb-0">
      <div class="container">
        <p>產品設計</p>
        <h2 class="fs-1 fw-bold pb-3 pb-md-4">PRODUCT DESIGN</h2>
      </div>
    </section>
    <section class="categorySelecter position-sticky mb-0">
      <div class="container">
        <ul class="d-none d-md-flex flex-wrap justify-content-start">
          <li v-for="item in categoryList" :key="item.id">
            <p @click="filterCategory(item)" class="btn-tag" :class="{ active: category === item.label }">{{ item.label }}</p>
          </li>
        </ul>
        <select @change="goTop" v-model="category" class="form-select d-md-none" aria-label="categorySelect">
          <option v-for="item in categoryList" :key="item.id" :value="item.label">{{ item.label }}</option>
        </select>
      </div>
    </section>
    <section class="design-item mb-0">
      <div class="container">
        <div class="row justify-content-center">
          <div class="col-md-10 col-12">
            <div class="row">
              <div class="col-lg-4 col-md-6 col-12 pb-4 pb-lg-5" v-for="item in filterProductData(productData)" :key="item.id" data-aos="fade-up" data-aos-duration="800">
                <div :id="item.name" data-bs-interval="false" class="carousel slide" data-bs-ride="carousel">
                  <div class="carousel-inner">
                    <div v-for="(img, index) in item.imgIntro" :key="index" class="carousel-item" :class="{ active: index === 0 }"><img :src="img.imgUrl" class="d-block w-100" loading="lazy" :alt="img.alt" /></div>
                  </div>
                  <button v-if="item.imgIntro.length > 1" class="carousel-control-prev" type="button" :data-bs-target="`#${item.name}`" data-bs-slide="prev">
                    <span class="carousel-arrow" aria-hidden="true"><i class="bi bi-chevron-left"></i></span>
                    <span class="visually-hidden">Previous</span>
                  </button>
                  <button v-if="item.imgIntro.length > 1" class="carousel-control-next" type="button" :data-bs-target="`#${item.name}`" data-bs-slide="next">
                    <span class="carousel-arrow" aria-hidden="true"><i class="bi bi-chevron-right"></i></span>
                    <span class="visually-hidden">Next</span>
                  </button>
                </div>
                <h3 class="pt-2 pt-mb-3">{{ item.chTitle }}</h3>
                <p class="text-secondary fs-7 py-2">{{ item.enTitle }}</p>
                <span class="badge">{{ item.category }}</span>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>
<script>
export default {
  data() {
    return {
      category: "all",
      categoryList: [
        {
          id: 1,
          label: "all",
        },
        {
          id: 2,
          label: "包包",
        },
        {
          id: 3,
          label: "月曆",
        },
        {
          id: 4,
          label: "帽子",
        },
        {
          id: 5,
          label: "馬克杯",
        },
        {
          id: 6,
          label: "手機殼",
        },
        {
          id: 7,
          label: "撲克牌",
        },
        {
          id: 8,
          label: "娃娃",
        },
        {
          id: 9,
          label: "T-shirt",
        },
      ],
      productData: [],
    };
  },
  computed: {
    filterProductData() {
      return (data) => {
        if (this.category === "all") {
          return data;
        } else {
          return data.filter((item) => item.chCategory === this.category);
        }
      };
    },
  },
  methods: {
    getData() {
      this.axios({
        method: "get",
        url: "https://raw.githubusercontent.com/Rosette-xii/vue-mysite/main/src/assets/static/data.json",
      })
        .then((res) => {
          this.productData = res.data.productData;
        })
        .catch((err) => {
          console.log(err);
        });
    },
    filterCategory(item) {
      this.category = item.label;
      this.goTop();
    },
    goTop() {
      window.scrollTo(0, 0);
    },
  },
  mounted() {
    this.getData();
  },
};
</script>