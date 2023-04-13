<script>
import AirDatepicker from "air-datepicker";
import "air-datepicker/air-datepicker.css";
import { onMounted } from "vue";

export default {
  setup() {
    onMounted(() => {
      new AirDatepicker("#departure");
      new AirDatepicker("#arrival");
    });

    return {};
  },
};
</script>

<template>
  <section class="hero">
    <div class="container">
      <div class="hero__content">
        <form class="hero__form">
          <label for="from">
            <img src="/src/assets/icons/location.svg" alt="" />
            <input type="text" placeholder="Откуда" id="from" />
          </label>
          <label for="to">
            <img src="/src/assets/icons/location.svg" alt="" />
            <input type="text" placeholder="До куда" id="to" />
          </label>
          <label for="departure">
            <input type="text" placeholder="Вылет" id="departure" />
            <img src="/src/assets/icons/calendar.svg" alt="" />
          </label>
          <label for="arrival">
            <input type="text" placeholder="Прилет" id="arrival" />
            <img src="/src/assets/icons/calendar.svg" alt="" />
          </label>
        </form>
      </div>
    </div>
  </section>
</template>

<style lang="scss">
.hero {
  background: #005688;
  height: 100vh;
  display: flex;
  align-items: center;

  &__content {
    text-align: center;
    background: rgba(246, 246, 246, 0.5);
    border-radius: 50px;
    padding: 14px 17px;
  }

  &__form {
    background: #f6f6f6;
    border-radius: 50px;
    padding: 10px 30px;
    box-sizing: border-box;
    display: flex;
    gap: 20px;

    label:nth-child(2) {
      border-left: 1px solid #808080;
    }

    #departure, #arrival {
      border-left: 1px solid #808080;
      width: 110px;
    }

    label {
      display: flex;
      align-items: center;
      padding: 0 10px;
    }

    input {
      background: transparent;
      border: none;
      padding: 17px 10px;
      box-sizing: border-box;
    }
  }
}
</style>
