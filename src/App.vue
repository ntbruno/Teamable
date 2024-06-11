<template>
  <div v-show="!isEditMode">
    <h1>User Profile</h1>
    <img :src="image" />
    <span>Name:</span><b id="name">{{ name }}</b>
    <hr />
    <span>Email:</span><b id="email"> {{ email }} </b>
    <hr />
    <span>Interests:</span><b id="interests">{{ interests }}</b>
    <hr />
    <button @click="handleEditProfile">Edit Profile</button>
  </div>
  <div v-show="isEditMode">
    <h1>User Profile</h1>
    <img :src="image" />
    <span>Name:</span>
    <input type="text" id="input-name" v-model="name" />
    <hr />
    <span>Email:</span>
    <input type="text" id="input-email" v-model="email" />
    <hr />
    <span>Interests:</span>
    <input type="text" id="input-interests" v-model="interests" />
    <hr />

    <button @click="handelUpdateProfile">Update Profile</button>
  </div>
</template>

<script>
import image from "./profile.jpeg";
export default {
  name: "App",
  data() {
    return {
      image: image,
      name: "",
      email: "",
      interests: "",
      isEditMode: false,
    };
  },

  async created() {
    const userData = await this.fetchUserProfile();
    this.name = userData.name;
    this.email = userData.email;
    this.interests = userData.interests;
  },

  methods: {
    handleEditProfile() {
      this.isEditMode = true;
    },
    async handelUpdateProfile() {
      // It is important to note that, since the change of information will happen when the user click on profile button, it is here thath the new payload should be define.
      const payload = {
        name: this.name,
        email: this.email,
        interests: this.interests,
      };
      const resJson = await this.updateUserProfile(payload);
      console.log(resJson);
      this.isEditMode = false;
    },
    // Creating data to be load when the browser page open from backEnd
    async fetchUserProfile() {
      const res = await fetch("get-profile");
      return await res.json();
    },

    //Update and send new data to be saved from the frontEnd to the backEnd
    async updateUserProfile(payload) {
      const res = await fetch("update-profile", {
        method: "POST",
        headers: {
          "Content-type": "application/json",
          Accept: "application.json",
        },
        body: JSON.stringify(payload),
      });
      return await res.json();
    },
  },
};
</script>

<style>
img {
  width: 400px;
  height: 270px;
  display: block;
  margin-bottom: 40px;
}

div {
  margin: 40px auto;
  width: 80%;
}
hr {
  width: 400px;
  margin-left: 0;
  margin-top: 25px;
  margin-bottom: 25px;
}
button {
  width: 160px;
  font-size: 15px;
  border-radius: 5px;
  height: 45px;
}
button:hover {
  cursor: pointer;
}
input {
  width: 200px;
  font-size: 15px;
  padding: 10px;
}
</style>
