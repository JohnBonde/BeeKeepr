<template id="modal-template">
  <div class="modal-backdrop">
    <div class="modal">
      <header class="modal-header">
        <slot name="header">
          <h3>Create Comb</h3>
        </slot>
      </header>
      <section class="modal-body">
        <slot name="body">
          <form class="new-hive-form" @submit.prevent="addHive(), close()">
            <div id="name-form">
              <input
                required
                id="title"
                type="text"
                placeholder="Enter hive tile"
                v-model="newHive.name"
              />
              <input
                required
                id="description"
                type="text"
                placeholder="Enter hive description"
                v-model="newHive.description"
              />
            </div>
            <footer>
              <button type="submit" class="btn btn-success m-1">Submit</button>
              <button type="button" class="btn btn-danger m-1" @click="close">Cancel</button>
            </footer>
          </form>
        </slot>
      </section>
    </div>
  </div>
</template>

<script>
export default {
  name: "modal",
  data() {
    return {
      newHive: {
        name: "",
        description: ""
      }
    };
  },
  methods: {
    async addHive() {
      let hive = { ...this.newHive };
      this.$store.dispatch("addVault", hive);
      this.newHive = {
        name: "",
        description: ""
      };
    },
    close() {
      this.$emit("close");
    }
  }
};
</script>
<style scoped>
.modal-backdrop {
  position: fixed;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  background-color: rgba(0, 0, 0, 0.3);
  display: flex;
  justify-content: center;
  align-items: center;
}
.modal {
  background: #ffffff;
  box-shadow: 2px 2px 20px 1px;
  overflow-x: auto;
  display: flex;
  flex-direction: column;
  width: 50vh;
  height: 45%;
  top: auto;
  left: auto;
  background-position: center;
}
.modal-header,
.modal-footer {
  padding: 15px;
  display: flex;
}
.modal-header {
  border-bottom: 1px solid #eeeeee;
  color: black;
  justify-content: space-between;
  align-items: baseline;
  height: 9vh;
}
.modal-footer {
  border-top: 1px solid #eeeeee;
  justify-content: flex-end;
}
.modal-body {
  position: relative;
  padding: 20px 10px;
  margin-bottom: 0;
}
.btn-close {
  border: none;
  font-size: 20px;
  padding: 20px;
  cursor: pointer;
  font-weight: bold;
  color: black;
  background: transparent;
}
input {
  width: 101%;
}
label {
  margin-bottom: 2pt;
}
textarea {
  width: 100%;
  height: 8vh;
}
#name-form,
#title-form {
  display: flex;
  flex-direction: column;
  margin-bottom: 12pt;
  margin-top: 0;
}
#name-form {
  padding: 7%;
}
footer {
  display: flex;
  justify-content: flex-end;
}
</style>
