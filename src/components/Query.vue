<template>
  <div>
    <b-input-group size="sm" class="mb-2">
      <b-input-group-prepend is-text>
        <b-icon icon="search"></b-icon>
      </b-input-group-prepend>
      <b-form-input
        type="search"
        v-model="input"
        placeholder="Please, start entering the text to search..."
        @input="isTyping = true"
      ></b-form-input>
    </b-input-group>
    <b-list-group v-show="input" v-if="loaded == true">
      <b-list-group-item
        v-for="item in itemList"
        :key="item.title"
        class="flex-column align-items-start"
      >
        <div class="d-flex w-100 justify-content-between">
          <a href="" class="mb-1">{{ item.title }}</a>
        </div>
        <p class="mb-1">
          {{ item.body }}
        </p>
      </b-list-group-item>
    </b-list-group>
    <b-list-group v-show="input" v-if="loaded == false">
      <b-list-group-item class="flex-column align-items-start">
        <div class="d-flex w-100">
          <p class="h6 mb-2">
            <b-icon
              icon="arrow-clockwise"
              animation="spin"
              font-scale="1"
            ></b-icon>
            Loading...
          </p>
        </div>
      </b-list-group-item>
    </b-list-group>
    <b-list-group v-if="loaded == true && itemList.length == 0">
      <b-list-group-item class="flex-column align-items-start">
        <div class="d-flex w-100">
          <p class="h6 mb-2">
            Sorry, we couldn't find anything :(
          </p>
        </div>
      </b-list-group-item>
    </b-list-group>
  </div>
</template>

<script>
import axios from "axios";
import _ from "lodash";

export default {
  name: "Query",
  props: {
    msg: String,
  },
  data() {
    return {
      itemList: [],
      input: "",
      loaded: false,
      isTyping: false
    };
  },
  watch: {
    input: _.debounce(function () {
      this.isTyping = false;
      this.loaded = false;
    }, 1000),
    isTyping: function (value) {
      if (!value && this.input) {
        this.itemList = [];
        this.searchPost(this.input);
        this.loaded = false;
      }
    },
  },
  methods: {
    searchPost: function (input) {
      this.isLoading = true;
      axios
        .get("https://jsonplaceholder.typicode.com/posts?q=" + input)
        .then((response) => {
          this.itemList = response.data;
          this.isLoading = false;
          this.loaded = true;
        });
    }
  }
};
</script>

<style>
</style>
