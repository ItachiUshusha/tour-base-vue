// капчу нужно сделать mounted // disabledDates Date[] массив с датами которые
будут являться неактивными

<template>
  <div class="calendar">
    <h2>Выберите дату</h2>
    <Calendar
      v-model="dates"
      inputId="dates"
      showIcon
      iconDisplay="input"
      dateFormat="dd/mm/yy"
      selectionMode="range"
      :manualInput="false"
      :selectOtherMonths="true"
      :minDate="setMinDate"
      :maxDate="setMaxDate"
      :disabledDates="disDates"
      showButtonBar
    />
    <form
      method="post"
      @submit.prevent="validate"
      class="selectables"
      v-if="dates?.length > 0"
    >
      <my-select
        v-model="selected_room_type"
        :options="this.rooms_types"
        :selectText="'Выберите количество кроватей'"
      >
      </my-select>
      <my-select
        v-model="selectes_connection"
        :options="connections"
        :selectText="'Как нам с вами связаться?'"
      >
      </my-select>
      <my-input
        v-if="selectes_connection"
        v-model="input_connection"
        :placeholder="choosePlaceholder"
        :type="chooseType"
        :pattern="choosePattern"
        required
      >
      </my-input>
      <VueRecaptcha
        class="vue-recaptcha"
        ref="recaptcha"
        size="invisible"
        :sitekey="this.sitekey"
        @verify="sendData"
        @expired="onCaptchaExpired"
      >
      </VueRecaptcha>
      <my-button type="submit"> Отправить </my-button>
    </form>
  </div>
</template>

<script>
import { VueRecaptcha } from 'vue-recaptcha';
export default {
  components: {
    VueRecaptcha,
  },
  data() {
    return {
      dates: [],
      selected_room_type: '',
      selectes_connection: '',
      input_connection: '',
      sitekey: '6LflZOsqAAAAAJrgd1R2ROCQEanjem4ffF-1kHO-',
      rooms_types: [
        { value: '3 beds', name: '3 кровати' },
        { value: '4 beds', name: '4 кровати' },
        { value: '5 beds', name: '5 кроватей' },
        { value: '6 beds', name: '6 кроватей' },
      ],
      connections: [
        { value: 'telegram', name: 'Telegram' },
        { value: 'whatsapp', name: 'WhatsApp' },
        { value: 'email', name: 'Почта' },
      ],
      disDates: [
        // new Date(2025, 5, 20),
        // new Date(2025, 5, 21),
        // new Date(2025, 5, 22),
      ],
    };
  },
  methods: {
    async sendData(recaptchaToken) {
      const formattedDates = this.dates.map((date) => {
        if (date !== null) {
          const year = date.getFullYear();
          const month = String(date.getMonth() + 1).padStart(2, '0');
          const day = String(date.getDate()).padStart(2, '0');
          return `${year}-${month}-${day}`;
        }
      });

      const data = {
        dates:
          formattedDates[1] === undefined
            ? [formattedDates[0]]
            : formattedDates,
        room_type: this.selected_room_type,
        connection: this.selectes_connection,
        user_con: this.input_connection,
        captcha_response: recaptchaToken,
      };

      try {
        const response = await fetch('http://127.0.0.1:8000/send-to-telegram', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json',
          },
          body: JSON.stringify(data),
        });

        if (response.ok) {
          alert('Данные успешно отправлены!');
          this.dates = [];
          this.selected_room_type = '';
          this.selectes_connection = '';
          this.input_connection = '';
        } else {
          alert('Не получилось отправить данные :(');
        }
      } catch (e) {
        console.error('Ошибка', e);
        alert('Произошла ошибка при отправке данных');
      }
    },
    validate() {
      this.$refs.recaptcha.execute();
    },
    onCaptchaExpired() {
      this.$refs.recaptcha.reset();
    },
  },
  computed: {
    setMinDate() {
      let today = new Date();

      const year = today.getFullYear();

      if (today.getMonth() < 5) {
        return new Date(year, 5, 1);
      } else if (today.getMonth() > 8) {
        return new Date(year + 1, 5, 1);
      }
    },
    setMaxDate() {
      let today = new Date();

      const year = today.getFullYear();

      if (today.getMonth() < 5) {
        return new Date(year, 9, 0);
      } else if (today.getMonth() > 8) {
        return new Date(year + 1, 9, 0);
      }
    },
    choosePlaceholder() {
      switch (this.selectes_connection) {
        case 'Telegram':
          return '@MyAcc';
        case 'WhatsApp':
          return '+7...';
        case 'Почта':
          return 'example@mail.com';
      }
    },
    chooseType() {
      switch (this.selectes_connection) {
        case 'Telegram':
          return 'text';
        case 'WhatsApp':
          return 'tel';
        case 'Почта':
          return 'email';
      }
    },
    choosePattern() {
      if (this.selectes_connection === 'WhatsApp') {
        if (this.input_connection[0] === '+') {
          return '[+][0-9]{11}';
        } else {
          return '[0-9]{11}';
        }
      }
    },
  },
};
</script>

<style scoped>
.calendar {
  display: flex;
  align-items: center;
  flex-direction: column;
  gap: 10px;
}
.selectables {
  display: flex;
  flex-direction: column;
  flex-wrap: wrap;
  gap: 10px;
  width: 100%;
}
.vue-recaptcha {
  display: none;
}
</style>
