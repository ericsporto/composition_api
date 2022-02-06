<template>
<table class="col-md-12">
  <thead>
    <tr>
      <th>Código</th>
      <th>Nome</th>
      <th>Máximo</th>
      <th>Mínimo</th>
      <th>Variação</th>
    </tr>
  </thead>
  <tbody >
    <tr class='border border-2' v-for='(quote, key) in quotes' :key='key'>
      <td class="ps-2 pe-2">{{ key }}</td>
      <td class="ps-2 pe-2">{{ quote.name }}</td>
      <td class="ps-2 pe-2">{{ quote.high  }}</td>
      <td class="ps-2 pe-2">{{ quote.low  }}</td>
      <td>
        <span class="ps-2 pe-2 me-3"
        :class="{'border border-danger bg-danger ': quote.pctChange < 0, 'border border-success bg-success': quote.pctChange > 0 }"
        >          
          {{ quote.pctChange  }} %
        </span>
      </td>
      <td>
        <a 
        v-if='!listenQuotes.includes(key)' 
        class="btn btn-primary  me-2"
        data-tooltip='Seguir'
        @click="$emit('listen', key)">
        <i class="icon icon-plus">+</i>
        
        </a>
        <a
        v-else 
        style='width:2.3em'
        class="btn btn-danger me-2" data-tooltip='Remover'
        @click="$emit('unlisten', key)">
        <i class="icon icon-minus">-</i>
        </a>
      </td>

    </tr>
  </tbody>
</table>
  
</template>

<script>
export default {
  props: {
    quotes: {type: Object, required: true},
    listenQuotes: {type: Array, required: true}
    },

    emits: ['listen', 'unlisten']
};
</script>

