<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar>
        <ion-title>Cryptocurrency Tracker</ion-title>
      </ion-toolbar>
    </ion-header>

    <ion-content :fullscreen="true" class="ion-padding">
      <div class="refresh-container">
        <ion-button expand="block" @click="fetchCryptoData">
          <ion-icon :icon="refreshOutline" slot="start"></ion-icon> Refresh Data
        </ion-button>
      </div>

      <ion-list>
        <ion-item v-for="coin in cryptoList" :key="coin.id" class="crypto-item">
          <ion-grid>
            <ion-row>
              
              <ion-col size="2" class="col-rank">
                <div class="label">Rank</div>
                <div class="value">{{ coin.rank }}</div>
              </ion-col>

              <ion-col size="3" class="col-name">
                <img :src="'https://www.coinlore.com/img/25x25/' + coin.nameid + '.png'" 
                    alt="icon" 
                    style="width: 25px; height: 25px;" 
                    @error="handleImageError" />
                <div class="label">{{ coin.name }}</div>
                <div class="value">{{ coin.symbol }}</div>
              </ion-col>

              <ion-col size="4" class="col-price">
                <div class="label">USD $</div>
                <div class="value">{{ coin.price_usd }}</div>
              </ion-col>

              <ion-col size="3" class="col-right">
                <div class="label">24h %</div>
                <div 
                  class="value" 
                  :style="{ 
                    color: parseFloat(coin.percent_change_24h) >= 0 ? '#28a745' : '#dc3545', 
                    fontWeight: 'bold',
                    fontSize: '0.9rem'
                  }"
                >
                  {{ parseFloat(coin.percent_change_24h) >= 0 ? '+' : '' }}{{ coin.percent_change_24h }}%
                </div>
              </ion-col>

            </ion-row>
          </ion-grid>
        </ion-item>
      </ion-list>
    </ion-content>
  </ion-page>
</template>

<script lang="ts">
import { 
  IonContent, 
  IonHeader, 
  IonPage, 
  IonTitle, 
  IonToolbar, 
  IonList, 
  IonItem, 
  IonButton,
  IonGrid,
  IonRow,
  IonCol,
  IonIcon
} from '@ionic/vue';
import { defineComponent, ref, onMounted } from 'vue';
import axios from 'axios';
import { refreshOutline } from 'ionicons/icons';

interface Crypto {
  id: string;
  symbol: string;
  name: string;
  nameid: string;
  rank: number;
  price_usd: string;
  percent_change_24h: string;
}

export default defineComponent({
  name: 'HomePage',
  components: {
    IonContent,
    IonHeader,
    IonPage,
    IonTitle,
    IonToolbar,
    IonList,
    IonItem,
    IonButton,
    IonGrid,
    IonRow,
    IonCol,
    IonIcon
  },
  setup() {
    const cryptoList = ref<Crypto[]>([]);

    const fetchCryptoData = async () => {
      try {
        const response = await axios.get('https://api.coinlore.net/api/tickers/');
        cryptoList.value = response.data.data;
        console.log('Data berhasil diambil:', cryptoList.value);
      } catch (error) {
        console.error('Gagal mengambil data:', error);
        alert('Gagal memuat data, silakan coba lagi.');
      }
    };

    const handleImageError = (event: Event) => {
      const target = event.target as HTMLImageElement;
      target.src = 'https://www.coinlore.com/img/25x25/bitcoin.png';
    };

    onMounted(() => {
      fetchCryptoData();
    });

    return {
      cryptoList,
      fetchCryptoData,
      refreshOutline,
      handleImageError
    };
  }
});
</script>

<style scoped>
.refresh-container {
  margin-bottom: 10px;
}
.crypto-item {
  --background: #FFF8DC;
  --border-color: #ddd;
  margin-bottom: 2px;
}
.label {
  font-size: 0.75rem;
  color: #666;
  margin-bottom: 2px;
}
.value {
  font-size: 1.1rem;
  font-weight: bold;
  color: #000;
}
.col-rank {
  text-align: center;
}
.col-name {
  text-align: left;
}
.col-price {
  text-align: left;
}
</style>
