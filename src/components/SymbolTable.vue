<template>
  <div>
    <div class="search">
      <!-- 搜索框 -->
      <input class="search-input" v-model="searchTerm" size="100" placeholder="Search by Chinese or English name..."/>
    </div>
    <p>Symbols:</p>
    <table>
      <thead>
      <tr>
        <th>No.</th>
        <th>NameCh</th>
        <th>NameEn</th>
        <th>Symbol</th>
      </tr>
      </thead>
      <tbody>
      <tr
        v-for="(symbol, index) in filteredSymbols"
        :key="symbol.html"
      >
        <td>{{ index + 1 }}</td>
        <td>{{ symbol.nameCh }}</td>
        <td>{{ symbol.nameEn }}</td>
        <td
          class="symbol"
          :data-name-ch="symbol.nameCh"
          :data-name-en="symbol.nameEn"
          v-html="symbol.html"
          @click="copyToClipboard(symbol.html)"
        ></td>
      </tr>
      </tbody>
    </table>
  </div>
</template>


<script>
  import symbols from './symbols'

  export default {
    name: "SymbolTable",
    data() {
      return {
        // 用户在搜索框中输入的内容
        searchTerm: '',
        // 所有可用的符号
        symbols: symbols
      };
    },
    computed: {
      // 根据 searchTerm 过滤符号
      filteredSymbols() {
        if (!this.searchTerm) return this.symbols;

        const lowerCaseSearchTerm = this.searchTerm.toLowerCase();

        return this.symbols.filter(symbol => {
          return (
            symbol.nameCh.toLowerCase().includes(lowerCaseSearchTerm) ||
            symbol.nameEn.toLowerCase().includes(lowerCaseSearchTerm)
          );
        });
      }
    },
    methods: {
      async copyToClipboard(entity) {
        try {
          const textArea = document.createElement('textarea');
          textArea.innerHTML = entity;
          const decodedEntity = textArea.innerText;

          await navigator.clipboard.writeText(decodedEntity);

        } catch (err) {
          console.error('Failed to copy text: ', err);
        }
      }
    }
  };
</script>

<style scoped>
  .search {
    display: grid;
    place-items: center;
  }

  .search-input {
    height: 40px; /* 调整高度 */
    padding: 8px 12px; /* 调整内边距以提供适当的文本间距 */
    font-size: 16px; /* 调整字体大小，使其与新的高度匹配 */
    border: 1px solid #ccc; /* 示例边框 */
    border-radius: 4px; /* 添加轻微的圆角 */

  }



  table {
    width: 100%;
    border-collapse: collapse;
  }

  th, td {
    padding: 8px 12px;
    border: 1px solid #ccc;
    text-align: left;
  }

  th {
    background-color: #f2f2f2;
  }

  .symbol:hover::after {
    /* ... (keep the existing styles for this class) ... */
    left: 0;
    transform: none;
  }

</style>
