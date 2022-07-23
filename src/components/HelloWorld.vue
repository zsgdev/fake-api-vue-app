<template>
  <v-container>
    <v-row class="text-center">
      <v-col cols="12">
        <v-card
          v-for="(user, id) of users"
          :key="id"
          class="mx-auto mt-5"
          max-width="444"
          outlined
        >
          <v-list-item three-line>
            <v-list-item-content>
              <div class="text-overline mb-4">{{ userInfo(user) }}</div>
              <v-list-item-title class="text-h5 mb-1"> </v-list-item-title>
              <v-list-item-subtitle></v-list-item-subtitle>
            </v-list-item-content>

            <v-list-item-avatar tile size="80" color="grey">
              <img :src="user.avatar" alt="" srcset="" />
            </v-list-item-avatar>
          </v-list-item>

          <v-card-actions>
            <v-btn text color="teal accent-4" @click="reveal = true">
              Show profile
            </v-btn>
          </v-card-actions>
        </v-card>
        <v-pagination
          v-model="pages"
          :length="numberOfPages"
          @input="onClick($event)"
          >{{ pages }}</v-pagination
        >
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import api from "../api/apis.js";
export default {
  name: "HelloWorld",
  data: () => ({
    users: [],
    pages: 1,
    numberOfPages: null,
  }),
  methods: {
    userInfo(user) {
      return user.first_name + " " + user.last_name;
    },
    onClick(v) {
      console.log(v);
    },
    async getData() {
      try {
        await api.get("/users", { params: { page: 1 } }).then((res) => {
          this.numberOfPages = res.data.total_pages;
          this.users = res.data.data;
          console.log(res.data);
        });
      } catch {
        console.log("error");
      }
    },
  },
  mounted() {
    this.getData();
  },
};
</script>
