<template>
  <div class="about">
    <h1>Hello {{ their_name }}, This is an about page</h1>
    <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna ali...</p>
  </div>
</template>

<script>
export default {
  name: 'Skills',
  data(){
    return {
        their_name: this.$route.params.name
    }
  },
  methods: {
    addSkill(){
      this.$validator.validateAll().then((result) => {
          if (result) {
            this.skills.push({skill: this.skill});
            this.skill = '';
          } else {
            console.log('Not valid');
          }
        })
    }
  }
}
</script>