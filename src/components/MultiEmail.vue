<template>
  <div class="multiple_emails-container">
    <input class="email-input" v-bind:class="{'invalid-email': invalid_email }" v-model="current_email" @keyup.enter="saveEmail"/>
    <div> 
      <ul>
        <li class="multiple_emails-email" v-for="(email, index) in email_list" :key="index" @click="selectBy(index)">
          <span class="multiple_emails-close">
            <button @click="deleteEmail(index)" type="button"><i class="fa fa-calendar-plus-o"></i></button>
          </span>
          <span>{{email}}</span>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    value: {
      type: String,
      default: ""
    },
  },
  data: () => ({
    current_email: null,
    invalid_email: false,
    selected_index: null,
    email_regexp: /^(([^<>()\]\\.,;:\s@"]+(\.[^<>()\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/
  }),
  computed: {
    email_list: {
      get() {
        return this.value === "" ? [] : this.value.split(",")
      },
      set(email_list) {
        this.$emit('input', email_list.join())
      }
    }
  },
  methods: {
    saveEmail() {
      if(this.selected_index === null) {
        this.addEmail()
      } else {
        this.updateEmail()
      }
    },
    addEmail() {
      let email = this.current_email.trim()
      this.invalid_email = !this.validate(email) || this.exist(email)
      if(!this.invalid_email) {
        let email_list = [...this.email_list]
        email_list.push(email)
        this.email_list = email_list
        this.current_email = null
      }
    },
    updateEmail() {
      let email = this.current_email.trim()
      this.invalid_email = !this.validate(email) || this.exist(email)
      if(!this.invalid_email) {
        let email_list = [...this.email_list]
        email_list[this.selected_index] = email
        this.email_list = email_list
        this.current_email = null
        this.selected_index = null
      }
    },
    validate(email) {
      return this.email_regexp.test(email) === true
    },
    exist(email) {
      return this.email_list.indexOf(email) !== -1
    },
    selectBy(index) {
      this.current_email = this.email_list[index]
      this.selected_index = index
      this.invalid_email = false
    },
    deleteEmail(index) {
      let email_list = [...this.email_list]
      email_list.splice(index, 1)
      this.email_list = email_list
    }
  },
  watch: {
    current_email: function (email) {
      if(email ===null || !email.length) {
        this.selected_index = null
      }
    }
  }
}
</script>

<style scoped>
  .email-input {
    width: 100%;
    box-sizing: border-box;
    border-radius: 3px; 
    padding: 3px 5px 3px 5px; 
  }

  .invalid-email {
    box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075), 0 0 6px red !important;
    outline: thin auto red !important;
  }

  .multiple_emails-container { 
    border:1px #00ff00 solid;
    border-radius: 1px;
    box-shadow: inset 0 1px 1px rgba(0,0,0,.075);
    padding:0; margin: 0; cursor:text; width:100%;
  }
  
  .multiple_emails-container ul {	
    list-style-type:none;
    padding-left: 0;
  }

  .multiple_emails-close { 
    float:left;
    margin:0 3px;
  }

  .multiple_emails-email { 
    margin: 3px 5px 3px 5px;
    padding: 3px 5px 3px 5px;
    border:1px #BBD8FB solid;
    border-radius: 3px;
    background: #F3F7FD;
    text-align: left;
  }
</style>