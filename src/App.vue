<script>
import AirDatepicker from "air-datepicker";
import "air-datepicker/air-datepicker.css";
import { onMounted, ref } from "vue";

export default {
  setup() {
    const cityVal = ref(null);
    const toInput = ref(null);
    const show = ref(false);
    const cities = [
      "Бишкек",
      "Ош",
      "Джалал-Абад",
      "Каракол",
      "Нарын",
      "Талас",
      "Баткен",
      "Токмок",
      "Кызыл-Кия",
      "Кара-Балта",
      "Чолпон-Ата",
      "Токтогул",
      "Исфана",
      "Ала-Бука",
      "Кок-Жангак",
      "Сузак",
      "Ак-Суу",
      "Балыкчы",
      "Кант",
      "Жалал-Абад",
    ];
    const filteredCities = ref(cities);

    const getAddressFromLatLng = (lat, lng) => {
      const xhr = new XMLHttpRequest();
      const url = `https://nominatim.openstreetmap.org/reverse?format=json&lat=${lat}&lon=${lng}`;

      xhr.open("GET", url, true);

      xhr.onload = () => {
        if (xhr.status === 200) {
          const response = JSON.parse(xhr.responseText);
          console.log(response.address.city);
          cityVal.value = response.address.city.replace("город", "");
        } else {
          console.log(`Request failed.  Returned status of ${xhr.status}`);
        }
      };

      xhr.send();
    };

    const handleClick = (city) => {
      toInput.value = city;
      show.value = !show.value;
    };

    const filterCities = () => {
      filteredCities.value = cities.filter((item) =>
        item.toLowerCase().includes(toInput.value.toLowerCase().trim())
      );
    };

    onMounted(() => {
      new AirDatepicker("#departure");
      new AirDatepicker("#arrival");

      if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(
          (position) => {
            const latitude = position.coords.latitude;
            const longitude = position.coords.longitude;
            getAddressFromLatLng(latitude, longitude);
          },
          (error) => {
            console.log(`Geolocation error: ${error.message}`);
          }
        );
      } else {
        console.log("Geolocation is not supported by this browser.");
      }
    });

    return {
      cityVal,
      filteredCities,
      filterCities,
      toInput,
      show,
      handleClick,
    };
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
            <input
              v-model="cityVal"
              type="text"
              placeholder="Откуда"
              id="from"
            />
          </label>
          <label @click="show = !show" for="to">
            <img src="/src/assets/icons/location.svg" alt="" />
            <input
              v-model="toInput"
              @input="filterCities"
              type="text"
              placeholder="До куда"
              id="to"
            />
            <div v-if="show" class="hero__dropdown dropdown">
              <div
                v-for="city in filteredCities"
                :key="city"
                class="dropdown__item"
              >
                <span @click="() => handleClick(city)">
                  {{ city }}
                </span>
              </div>
            </div>
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
  font-family: sans-serif;

  &__content {
    text-align: center;
    background: rgba(246, 246, 246, 0.5);
    border-radius: 50px;
    padding: 14px 17px;
  }

  &__dropdown {
    position: absolute;
    top: 100%;
    left: 0;
    background: #ccc;
    border-radius: 15px;
    width: 100%;
    max-height: 200px;
    overflow-y: auto;
  }

  .dropdown {
    flex-direction: column;
    gap: 15px;
    display: flex;

    &__item {
      cursor: pointer;

      &:hover span {
        text-decoration: underline;
      }
    }
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

    #departure,
    #arrival {
      border-left: 1px solid #808080;
      width: 110px;
    }

    label {
      display: flex;
      align-items: center;
      padding: 0 10px;
      position: relative;
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
