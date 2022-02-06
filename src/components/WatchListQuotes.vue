<template>
<div class="row col-md-12">
    <list-quotes :quotes='quotes' :listen-quotes='listenQuotes' @unlisten='onUnListen'/>
    <div class="col-md 12 mt-2 text-end ">
        <cite>
            Atualizará novamente em <strong>{{nextUpdateTime}} segundos</strong>
        </cite>
    </div>
</div>
</template>
<script>
import { onMounted, onUnmounted, reactive, ref, toRefs, watch } from "vue";
import ListQuotes from "./ListQuotes";
import api from "@/services/api";
const CURRENT_UPDATE_TIME = 10;
export default {
  components: { ListQuotes },
  props: {
    listenQuotes: { type: Array, required: true },
  },
  emits: ["unlisten"],
  setup(props, { emit }) {
    const interval = ref(null);
    const quotes = ref({});
    const nextUpdateTime = ref(CURRENT_UPDATE_TIME);
    const methods = reactive({
      onUnListen(code) {
        emit("unlisten", code);
      },
      restartInterval() {
        clearInterval(interval.value);
        nextUpdateTime.value = CURRENT_UPDATE_TIME;
        interval.value = setInterval(() => {
          nextUpdateTime.value -= 1;
          if (nextUpdateTime.value === 0) {
            nextUpdateTime.value = CURRENT_UPDATE_TIME;
            this.refresh();
          }
        }, 1000);
      },
      async refresh() {
        const { data } = await api.listen(props.listenQuotes);
        quotes.value = data;
      },
    });
    onMounted(() => {
      methods.refresh();
      methods.restartInterval();
    });
    onUnmounted(() => {
      clearInterval(interval.value);
    });
    watch(props, () => {
      methods.refresh();
      methods.restartInterval();
    });
    return {
      ...toRefs(methods),
      quotes,
      nextUpdateTime,
    };
  },
};
</script>