<template src="./productDisplayTemplate.html"></template>
<script>
  import "./productDisplayStyle.css";
  import "bootstrap/dist/css/bootstrap.css";
  import "@fortawesome/fontawesome-free/js/all.js";
  import products from "./products.json";

  export default {
    data() {
      return {
        product_list: products,
        search: "",
        quantities: {},
        currentSort:'item_code',
        currentSortDir:'asc'
      };
    },
    methods: {
      find() {
        var parent = this;
        parent.product_list = [];
        let searchTxt = parent.search.toUpperCase().trim();
        if (searchTxt == "") {
          parent.product_list = products
        } else {
          products.forEach((element) => {
            if (element.item_code.toUpperCase() == searchTxt ||
              element.product.toUpperCase() == searchTxt ||
              element.category.toUpperCase() == searchTxt) {
              parent.product_list.push(element);
            }
          });
        }
      },
      sort(s) {
        var parent = this;

        // if s == current sort, reverse
        if(s === parent.currentSort) {
          parent.currentSortDir = parent.currentSortDir === 'asc' ? 'desc' : 'asc';
        }
        parent.currentSort = s;

        // Sort the list
        parent.product_list.sort((a,b) => {
          let modifier = 1;
          if(parent.currentSortDir === 'desc') modifier = -1;
          if(a[parent.currentSort] < b[parent.currentSort]) return -1 * modifier;
          if(a[parent.currentSort] > b[parent.currentSort]) return 1 * modifier;
        });
      },
      updateQty(item_id){
        var parent = this;
        parent.product_list.forEach((element, counter) => {
            if(element.id == item_id){
                element.units = parent.quantities[item_id];
                parent.product_list[counter] = element;
            }
        })
      }
    },
    mounted: function(){
        products.forEach(item => {
            this.quantities[item.id]= item.units
        })
    }
  };
</script>
