<template>
  <div class="container">
    <main class="main">
      <h1 class="main__title">Государственные закупки</h1>
      <div class="card-box">
        <div v-for="(item, index) in data" :key="index" class="card">
          <ul class="descr">
            <li>
              <a href="{{ item.url }}"> <span>Номер закупки:</span> {{ item.id }} </a>
            </li>
            <li><span>Объект закупки:</span> {{ item.title }}</li>
            <li><span>Начальная цена закупки:</span> {{ item.price }}</li>
            <li><span>Дата окончания подачи заявок:</span> {{ item.subm_ends }}</li>
            <li><span>Дата начала исполнения контракта:</span> {{ item.work_starts }}</li>
            <li><span>Срок исполнения контракта:</span> {{ item.work_ends }}</li>
            <li class="descr"><span>Информация об объекте закупки:</span> {{ item.descr }}</li>
          </ul>
          <div class="btn-wrap">
            <button class="button" @click="currentCard = item">Ознакомиться с ТЗ</button>
            <button class="button" @click="currentInfo = item">Информация о заказчике</button>
            <button class="button" @click="currentBuy = item">Информация об объекте закупки</button>
          </div>
        </div>
        <div v-if="currentCard" class="popup" :class="{ hidden: !currentCard }">
          <div class="popup__body">
            <div>
              <h2>Номер закупки: {{ currentCard.id }}</h2>
              <p><span>Сокращенное техническое задание:</span> {{ currentCard.summary_tz }}</p>
              <button @click="currentCard = null" class="close-btn">Х</button>
            </div>
          </div>
        </div>

        <div v-if="currentInfo" class="popup" :class="{ hidden: !currentInfo }">
          <div class="popup__body">
            <div>
              <h2>Номер закупки: {{ currentInfo.id }}</h2>
              <ul>
                <li>
                  <a href="{{ currentInfo.org_url }}">Ссылка: {{ currentInfo.org_url }}</a>
                </li>
                <li><span>Контактное лицо:</span> {{ currentInfo.org_contact }}</li>
                <li><span>Адрес:</span> {{ currentInfo.org_addr }}</li>
                <li><span>ИНН организации:</span> {{ currentInfo.org_inn }}</li>
              </ul>

              <button @click="currentInfo = null" class="close-btn">Х</button>
            </div>
          </div>
        </div>

        <div v-if="currentBuy" class="popup" :class="{ hidden: !currentBuy }">
          <div class="popup__body">
            <div>
              <h2>Номер закупки: {{ currentBuy.id }}</h2>
              <p><span>Информация об объекте закупки:</span> {{ currentBuy.descr }}</p>
              <button @click="currentBuy = null" class="close-btn">Х</button>
            </div>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'

const data = ref([])
const currentCard = ref(null)
const currentInfo = ref(null)
const currentBuy = ref(null)
const fetchData = async () => {
  try {
    const response = await fetch('https://df554ea9f6cf9309.mokky.dev/data')
    data.value = await response.json()
  } catch (error) {
    console.error('Ошибка загрузки данных:', error)
  }
}

onMounted(fetchData)
</script>

<style>
.btn-wrap button:not(:last-child) {
  text-align: center;
  margin-right: 20px;
}

li {
  list-style-type: none;
}
.popup__body span {
  display: block;
  margin-top: 20px;
  margin-bottom: 10px;
  font-weight: 700;
}
.descr {
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
}

.descr span {
  font-weight: 700;
}
.container {
  width: 100%;
  height: 100%;
  position: relative;
  overflow: auto;
}

.main {
  max-width: 1000px;
  margin: 0 auto;
}

.main__title {
  text-align: center;
  font-size: 50px;
  margin-top: 20px;
  background-color: #fff;
  padding: 30px;
  border-radius: 0px 38px 38px 0px;
  -moz-border-radius: 0px 38px 38px 0px;
  -webkit-border-radius: 0px 38px 38px 0px;
  border: 0px solid #000000;
  -webkit-box-shadow: 3px 10px 5px -1px rgba(0, 0, 0, 0.34);
  -moz-box-shadow: 3px 10px 5px -1px rgba(0, 0, 0, 0.34);
  box-shadow: 3px 10px 5px -1px rgba(0, 0, 0, 0.34);
}

.card-box {
  margin-bottom: 100px;
}

.card {
  display: flex;
  flex-direction: column;
  gap: 15px;
  font-size: 24px;
  -webkit-box-shadow: 16px 29px 33px -22px rgba(0, 0, 0, 0.83);
  -moz-box-shadow: 16px 29px 33px -22px rgba(0, 0, 0, 0.83);
  box-shadow: 16px 29px 33px -22px rgba(0, 0, 0, 0.83);
  margin-bottom: 20px;
  margin-top: 40px;
  padding: 20px;
  text-align: justify;
  background-color: #fff;
  transition: 0.4s;
  border-radius: 10px;
}

.card a {
  text-decoration: none;
  color: black;
}

.card li {
  list-style-type: none;
}

.button {
  max-width: 50%;
  margin: 0 auto;
  padding: 15px 20px;
  border: none;
  border-radius: 10px;
  text-decoration: none;
  color: white;
  background: #0b63f6;
  box-shadow: 0 5px 0 #003cc5;
  cursor: pointer;
  transition: 0.4s;
}

.button:active {
  opacity: 0.6;
}

.button:hover {
  transform: scale(1.1);
}

.popup {
  display: flex;
  justify-content: center;
  align-items: center;
  position: fixed;
  overflow: hidden;
  top: 0;
  left: 0;
  bottom: 0;
  right: 0;
  background-color: rgba(0, 0, 0, 0.8);
  color: black;
  font-size: 25px;
  box-shadow: 0 0 1px 1px;
  backdrop-filter: blur(10px);
  z-index: 3;
}

.popup__body {
  height: auto;
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  overflow-y: scroll;
  max-width: 800px;
  margin: 0 auto;
  background-color: white;
  padding: 50px;
  height: 500px;
  margin-top: 30px;
  border-radius: 15px;
}

.close-btn {
  position: absolute;
  top: 10px;
  right: 10px;
  cursor: pointer;
  border: none;
  background-color: transparent;
  font-size: 24px;
  transition: 0.4s;
}

.close-btn:hover {
  transform: rotate(180deg);
}

.pagination {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 30px;
  margin-bottom: 60px;
}

.pagination button {
  padding: 10px 15px;
  text-align: center;
  font-size: 16px;
  cursor: pointer;
  border: none;
  background-color: #1d1d1db2;
  color: white;
}

.hidden {
  overflow: hidden;
}
</style>
