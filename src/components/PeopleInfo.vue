<template>
  <div
    class="feature-people"
    @click="$router.push({ name: 'peopleInfo', params: { id: id } })"
  >
    <div class="feature-people__img"><img :src="dataUrl" /></div>
    <div class="feature-people__info">
      <h2 class="feature-people__title">{{ person.name }}</h2>
      <ul class="feature-people__list">
        <li class="feature-info__list">Gender: {{ person.gender }}</li>
        <li class="feature-info__list">Birth year: {{ person.birth_year }}</li>
        <li class="feature-info__list">Eye color: {{ person.eye_color }}</li>
      </ul>
    </div>
  </div>
</template>

<script>
import { getImgService } from "@/services/imageService.js";
import { Formatter } from "@/helpers/formatter";
import img from "../assets/img/big-placeholder.jpg";

export default {
  data() {
    return { imagePeople: null };
  },
  props: {
    person: { type: Object, required: true },
    id: { tupe: Number, required: true, default: 0 },
  },
  async created() {
    try {
      this.imagePeople = await getImgService().getPersonImgById("1");
    } catch (error) {
      this.imagePeople = img;
    }
  },
  watch: {
    id: {
      handler(newVal) {
        getImgService()
          .getPersonImgById(newVal + 1)
          .then((response) => {
            this.imagePeople = response;
          })
          .catch(() => {
            this.imagePeople = img;
          });
      },
      immediate: true,
    },
  },
  computed: {
    dataUrl() {
      return Formatter.dataUrl(this.imagePeople);
    },
  },
};
</script>

<style lang="scss">
.feature-people {
  cursor: pointer;
  display: flex;
  width: 45%;
  padding: 16px;
  border-radius: 5px;
  background-color: var(--dynamic-background-color);
  &__img {
    width: 180px;
    height: 250px;
    border-radius: 10px;
    margin-right: 30px;
  }
  &__img img {
    width: 100%;
    height: 100%;
    border-radius: 10px;
  }
  &__info {
    margin-top: 20px;
  }
  &__title {
    font-size: 28px;
    font-weight: 500;
    letter-spacing: 0.05em;
    margin-bottom: 15px;
    color: var(--dynamic-text-color);
  }
  &__list li {
    padding: 12px 10px;
    border: 1px solid var(--dynamic-border-color);
    border-width: 1px 0 0px;
    font-size: 16px;
    color: var(--dynamic-text-color);
  }
}
@media screen and (max-width: 1200px) {
  .feature-people {
    width: 50%;
  }
}
@media screen and (max-width: 990px) {
  .feature-people {
    flex-direction: column;
    &__img {
      margin: 0 auto;
    }
  }
}
@media screen and (max-width: 576px) {
  .feature-people__img {
    width: 160px;
    height: 220px;
  }
  .feature-people__info {
    width: 160px;
  }
  .feature-people__title {
    font-size: 22px;
  }
  .feature-people__list li {
    width: 160px;
  }
}
@media screen and (max-width: 413px) {
  .feature-people {
    width: 100%;
    &__img {
      width: 140px;
      height: 200px;
    }
    &__info {
      margin: 0 auto;
      margin-top: 20px;
      width: 100%;
    }
    &__title {
      font-size: 18px;
    }
    &__list li {
      width: 100%;
      padding: 12px 5px;
      font-size: 14px;
    }
  }
}
</style>
