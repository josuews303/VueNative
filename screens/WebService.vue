<template>
  <view v-if="showUsers" class="container">
    <button :on-press="generateUsers" title="Generar Usuario Aleatorio" color="black"/>
    <view class="wea">
      
      <view v-for="(usuario, index) in users" :key="index">
        <touchable-opacity :on-press="() => getPhotos(usuario.user.username)">
          <image class="foticoperfil" :source="{ uri: usuario.user.profile_image.large }" />
          <text class="text-fotos">{{ usuario.user.username }}</text>
        </touchable-opacity>
      </view>
    </view>
  </view>

  <view v-else-if="showPhotos">
    <button :on-press="changePage" title="<- Regresar" />
    <text class="text-color-primary">{{ clickedUser }}</text>
    <view class="test" v-for="(foto, index) in photos" :key="index">
      <image class="foticos" :source="{ uri: foto.urls.small }" />
      <text class="text-fotos" v-if="foto.description">
        {{
        foto.description
        }}
      </text>
      <text class="text-fotos" v-else-if="foto.alt_description">
        {{
        foto.alt_description
        }}
      </text>
      <text class="text-fotos" v-else>Sin descripción</text>
    </view>
  </view>

  <view v-else>
    <text class="text-color-primary">{{ message }}</text>
  </view>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      message: "API UNSPLASH AGOTADA",
      users: null,
      photos: null,
      clickedUser: "",
      mostrarUsuarios: true,
      mostrarFotos: false,
      client_id: "9b5Caw71smFDUvbxMOZKIXkLcWaGMFnAnz7TxTki9Fw"
    };
  },
  mounted() {
    this.generateUsers();
  },
  methods: {
    generateUsers() {
      axios
        .get(
          "https://api.unsplash.com/photos/random/?count=1&client_id=" +
            this.client_id
        )
        .then(response => {
          this.users = response.data;
          console.log(this.users);
        })
        .catch(error => {
          console.log(error);
        });
    },
    getPhotos(usuario) {
      this.changePage();
      this.clickedUser = usuario;
      console.log(this.clickedUser);
      axios
        .get(
          "https://api.unsplash.com/users/" +
            this.clickedUser +
            "/photos?per_page=1&client_id=" +
            this.client_id
        )
        .then(response => {
          this.photos = response.data;
          console.log(this.photos);
        })
        .catch(error => {
          console.log(error);
        });
    },
    changePage() {
      this.mostrarUsuarios = !this.mostrarUsuarios;
      this.mostrarFotos = !this.mostrarFotos;
    }
  },
  computed: {
    showUsers() {
      return this.users && this.mostrarUsuarios;
    },
    showPhotos() {
      return this.photos && this.mostrarFotos;
    }
  }
};
</script>

<style>
.container {
  flex: 1;
  background-color: white;
}
.test {
  align-items: center;
  justify-content: center;
}
.text-color-primary {
  color: lightseagreen;
  text-decoration-line: underline;
  padding-top: 10;
  font-size: 30;
  text-align: center;
}
.text-fotos {
  color: lightseagreen;
  font-size: 14;
  text-align: center;
}
.foticoperfil {
  width: 105;
  height: 105;
  
}
.foticos {
  width: 250;
  height: 250;
  margin: 10;
  align-items: center;
  justify-content: center;
}
.wea {
  flex: 2;
  align-items: center;
  justify-content: center;
  width: 100%;
  height: 100%;
}
</style>
