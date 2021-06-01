<template>
  <q-page class="row items-center justify-evenly full-height">
    <q-card
      class="container-card full-height full-width bg-primary"
      flat
      square
    >
      <q-card-section class="relative-position full-height">
        <div class="row top-money-input full-width absolute q-col-gutter-sm">
          <div class="col-5">
            <q-card class="left-card shadow-4">
              <q-card-section class="full-height flex items-center">
                <div class="text-h6 text-weight-bold">
                  US${{ currentBalance }}
                  <div class="text-body2 text-weight-bold">Available</div>
                </div>
              </q-card-section>
            </q-card>
          </div>
          <div class="col-7">
            <q-card class="right-card shadow-4">
              <q-card-section class="full-height flex flex-center">
                <div class="full-width">
                  <q-input
                    v-model="sendAmount"
                    autofocus
                    outlined
                    placeholder="Send amount"
                    type="number"
                    color="info"
                    @update:model-value="calculateReceiveAmount"
                  ></q-input>
                </div>
              </q-card-section>
            </q-card>
          </div>
        </div>

        <div class="row bot-money-input full-width absolute q-col-gutter-sm">
          <div class="col-5">
            <q-card class="left-card shadow-4">
              <q-card-section class="full-height flex items-center">
                <div class="full-width">
                  <q-select
                    v-model="receiveCurrency"
                    :options="exchangeRates"
                    option-label="name"
                    option-value="rate"
                    dropdown-icon="mdi-chevron-down"
                    color="info"
                    borderless
                    @update:model-value="calculateSendAmount"
                  ></q-select>
                </div>
              </q-card-section>
            </q-card>
          </div>
          <div class="col-7">
            <q-card class="right-card shadow-4">
              <q-card-section class="full-height flex flex-center">
                <div class="full-width">
                  <q-input
                    v-model="receiveAmount"
                    outlined
                    placeholder="Receipient gets"
                    type="number"
                    color="info"
                    @update:model-value="calculateSendAmount"
                  ></q-input>
                </div>
              </q-card-section>
            </q-card>
          </div>
        </div>
      </q-card-section>
    </q-card>
  </q-page>
</template>

<script lang="ts">
// import { Todo, Meta } from 'components/models';
// import ExampleComponent from 'components/CompositionComponent.vue';
import { defineComponent, ref } from 'vue';

export default defineComponent({
  name: 'PageIndex',
  // components: { ExampleComponent },
  setup() {
    const exchangeRates = [
      {
        name: 'USD',
        rate: 1,
      },
      {
        name: 'EUR',
        rate: 0.815894,
      },
      {
        name: 'CAD',
        rate: 1.204355,
      },
      {
        name: 'GBP',
        rate: 0.70602,
      },
      {
        name: 'MXN',
        rate: 19.88162,
      },
      {
        name: 'PLN',
        rate: 3.66121,
      },
    ];

    const defaultReceiveCurrency = exchangeRates.find((c) => c.name === 'USD');

    const currentBalance = ref(500);
    const sendAmount = ref(0);
    const receiveCurrency = ref(defaultReceiveCurrency);
    const receiveAmount = ref(
      receiveCurrency.value ? sendAmount.value * receiveCurrency.value.rate : 0
    );

    const calculateReceiveAmount = () => {
      if (!isNaN(Number(sendAmount.value)) && receiveCurrency.value) {
        receiveAmount.value = sendAmount.value * receiveCurrency.value.rate;
      }
    };

    const calculateSendAmount = () => {
      if (!isNaN(Number(receiveAmount.value)) && receiveCurrency.value) {
        sendAmount.value = receiveAmount.value / receiveCurrency.value.rate;
      }
    };

    return {
      exchangeRates,
      currentBalance,
      sendAmount,
      receiveCurrency,
      receiveAmount,
      calculateReceiveAmount,
      calculateSendAmount,
    };
  },
});
</script>

<style lang="scss">
.container-card {
  max-width: 27.875rem;
  height: 22.625rem !important;

  .right-card {
    border-top-left-radius: 0 !important;
    border-bottom-left-radius: 0 !important;
  }
  .left-card {
    border-top-right-radius: 0 !important;
    border-bottom-right-radius: 0 !important;
  }

  .top-money-input {
    min-width: 32.1875rem;
    top: 0;
    left: 50%;
    transform: translateX(-50%);

    .q-card {
      height: 6.5rem;
    }
  }

  .bot-money-input {
    min-width: 32.1875rem;
    bottom: 0;
    left: 50%;
    transform: translateX(-50%);
    .q-card {
      height: 6.5rem;
    }
  }
}
</style>
