<template>
  <div class="hello">
    <table>
      <tbody>
        <tr v-for="(sale,i) in sales" :key="i">
          <td v-for="(cell, j) in sale" :key="i-j">{{ cell.Sale }}</td>
        </tr>
      </tbody>
    </table>
    <p>{{shitLinked}}</p>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  data() {
    return {
      sales: [
        [
          { Year: 2018, Sale: 512 },
          { Year: 2018, Sale: 1025 },
          { Year: 2018, Sale: 70 }
        ],
        [
          { Year: 2017, Sale: 155 },
          { Year: 2017, Sale: 12 },
          { Year: 2017, Sale: 69 }
        ],
        [
          { Year: 2016, Sale: 8767 },
          { Year: 2016, Sale: 29 },
          { Year: 2016, Sale: 72 }
        ]
      ],
    shitLinked: 'Please appear'
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
