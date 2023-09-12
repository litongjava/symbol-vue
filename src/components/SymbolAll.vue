<template>
  <div>
    <div class="search">
      <!-- 搜索框 -->
      <input class="search-input" v-model="searchTerm" size="100" placeholder="Search by Chinese or English name..."/>
    </div>
    <p>
      <a href="#/table" target="_blank">Table</a>
    </p>
    <p>Symbols:</p>
    <span
      v-for="symbol in filteredSymbols"
      :key="symbol.html"
      class="symbol"
      :data-name-ch="symbol.nameCh"
      :data-name-en="symbol.nameEn"
      v-html="symbol.html"
      @click="copyToClipboard(symbol.html)"
    ></span>
  </div>
</template>

<script>
  import symbols from './symbols'

  export default {
    name: "SymbolAll",
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

  .symbol {
    position: relative; /* 设置为 relative，以便我们的浮窗相对于这个元素定位 */
    cursor: pointer;
    margin: 5px;
    padding: 5px;
    border: 1px solid blue;
    display: inline-block;

  }

  .symbol:hover::after {
    content: attr(data-name-ch) " / " attr(data-name-en); /* 从 data- 属性中获取中英文名称 */
    position: absolute;
    top: 100%; /* 定位在符号的下方 */
    left: 50%; /* 使浮窗在水平方向上居中 */
    transform: translateX(-50%); /* 使浮窗在水平方向上居中 */
    background-color: rgba(0, 0, 0, 0.75); /* 半透明的黑色背景 */
    color: white;
    padding: 5px 10px;
    border-radius: 5px; /* 轻微的圆角 */
    white-space: nowrap; /* 防止文字换行 */
    pointer-events: none; /* 确保浮窗不会影响其他交互 */
    z-index: 100; /* 确保浮窗始终在顶部 */
  }
</style>
