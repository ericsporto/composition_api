<template>
<div class="container-fluid">
  <div class='row border text-center border-3 m-2' v-if='listenQuotes.length > 0'>
    <div class="col-md-12 text-center p-2" >
      <h1>Acompanhando</h1>

    </div>
    <div class="col-md-12 text-center">
      <WatchListQuotes :listen-quotes='listenQuotes' @unlisten='onUnListen' />
    </div>

  </div>
  <div class='row border text-center border-3 m-2'>
    <div class="col-md-12 text-center p-2">
      <h1>Todas as moedas</h1>

    </div>
    <div class="col-md-12 text-center">
      <ListQuotes 
      :quotes= 'quotes' :listen-quotes='listenQuotes '
      @listen='onListen'
      @unlisten='onUnListen'/>
    </div>

  </div>

</div>
</template>

<script>
import { onMounted, reactive, toRefs } from "vue";
import api from "@/services/api";
import ListQuotes from "./components/ListQuotes";
import WatchListQuotes from "./components/WatchListQuotes";
export default {
  name: "App",
  components: { ListQuotes, WatchListQuotes },
  setup() {
    const data = reactive({
      quotes: {},
      listenQuotes: [],
    });
    onMounted(async () => {
      const response = await api.all();
      data.quotes = response.data;
    });
    function onListen(code) {
      data.listenQuotes.push(code);
    }
    function onUnListen(code) {
      data.listenQuotes = data.listenQuotes.filter((key) => key !== code);
    }
    return {
      ...toRefs(data),
      onListen,
      onUnListen,
    };
  },
};
</script>